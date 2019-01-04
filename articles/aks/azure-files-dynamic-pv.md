---
title: Een volume bestanden met meerdere schillen zijn dynamisch maken in Azure Kubernetes Service (AKS)
description: Informatie over het dynamisch een permanent volume maken met Azure Files voor gebruik met meerdere gelijktijdige schillen in Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 10/08/2018
ms.author: iainfou
ms.openlocfilehash: 841c65fd8420fdfe681cb99ee7054cb4edd5fcd3
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2019
ms.locfileid: "53968981"
---
# <a name="dynamically-create-and-use-a-persistent-volume-with-azure-files-in-azure-kubernetes-service-aks"></a>Dynamisch maken en gebruiken van een permanent volume met Azure Files in Azure Kubernetes Service (AKS)

Een permanent volume vertegenwoordigt een stukje opslag die is ingericht voor gebruik met Kubernetes-schillen. Een permanent volume kan worden gebruikt door een of meer schillen en kan worden dynamisch of statisch ingericht. Als meerdere schillen gelijktijdige toegang tot de dezelfde opslagvolume moeten, kunt u Azure Files verbinding maken met behulp van de [Server Message Block (SMB)-protocol][smb-overview]. Dit artikel ziet u hoe u dynamisch maakt een Azure-bestandsshare voor gebruik door meerdere schillen in een cluster Azure Kubernetes Service (AKS).

Zie voor meer informatie over Kubernetes permanente volumes [Kubernetes permanente volumes][kubernetes-volumes].

## <a name="before-you-begin"></a>Voordat u begint

In dit artikel wordt ervan uitgegaan dat u een bestaand AKS-cluster hebt. Als u een cluster AKS nodig hebt, raadpleegt u de Quick Start voor AKS [met de Azure CLI] [ aks-quickstart-cli] of [met behulp van de Azure-portal][aks-quickstart-portal].

Ook moet de Azure CLI-versie 2.0.46 of later zijn geïnstalleerd en geconfigureerd. Voer  `az --version` uit om de versie te bekijken. Als u de Azure CLI wilt installeren of upgraden, raadpleegt u  [Azure CLI installeren][install-azure-cli].

## <a name="create-a-storage-account"></a>Create a storage account

Wanneer u een Azure-bestandsshare als een Kubernetes-volume dynamisch maakt, een opslagaccount kan worden gebruikt als deze zich in de AKS **knooppunt** resourcegroep. Deze groep is met de *MC_* voorvoegsel op dat is gemaakt door de inrichting van de resources voor het AKS-cluster. De naam van de resource met de [az aks show] [ az-aks-show] opdracht.

```azurecli
$ az aks show --resource-group myResourceGroup --name myAKSCluster --query nodeResourceGroup -o tsv

MC_myResourceGroup_myAKSCluster_eastus
```

Gebruik de [az storage-account maken] [ az-storage-account-create] opdracht om het opslagaccount te maken.

Update `--resource-group` verzameld door de naam van de resourcegroep in de vorige stap en `--name` naar een naam van uw keuze. Geef uw eigen unieke opslagaccountnaam:

```azurecli
az storage account create --resource-group MC_myResourceGroup_myAKSCluster_eastus --name mystorageaccount --sku Standard_LRS
```

> [!NOTE]
> Azure-bestanden op dit moment werkt alleen met de Standard-opslag. Als u Premium-opslag gebruikt, kan het volume niet inrichten.

## <a name="create-a-storage-class"></a>Maak een opslagklasse

Een opslagklasse wordt gebruikt om te definiëren hoe een Azure-bestandsshare wordt gemaakt. Een storage-account kan worden opgegeven in de klasse. Als een storage-account niet opgegeven is, een *skuName* en *locatie* moet worden opgegeven, en alle opslagaccounts in de bijbehorende resourcegroep worden geëvalueerd voor een overeenkomst. Zie voor meer informatie over Kubernetes-Opslagklassen voor Azure Files, [Kubernetes Opslagklassen][kubernetes-storage-classes].

Maak een bestand met de naam `azure-file-sc.yaml` en kopieer het volgende voorbeeld-manifest. Update de *storageAccount* waarde met de naam van uw opslagaccount in de vorige stap hebt gemaakt. Voor meer informatie over *mountOptions*, Zie de [koppelingsopties] [ mount-options] sectie.

```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: azurefile
provisioner: kubernetes.io/azure-file
mountOptions:
  - dir_mode=0777
  - file_mode=0777
  - uid=1000
  - gid=1000
parameters:
  skuName: Standard_LRS
  storageAccount: mystorageaccount
```

Maak de opslagklasse met de [kubectl toepassen] [ kubectl-apply] opdracht:

```console
kubectl apply -f azure-file-sc.yaml
```

## <a name="create-a-cluster-role-and-binding"></a>Een clusterrol en een binding maken

AKS-clusters gebruiken Kubernetes op rollen gebaseerd toegangsbeheer (RBAC) voor limiet voor acties die kunnen worden uitgevoerd. *Rollen* definiëren de machtigingen te verlenen en *bindingen* toepassen op de gewenste gebruikers. Deze toewijzingen kunnen worden toegepast op een bepaalde naamruimte, of in het hele cluster. Zie voor meer informatie, [met behulp van RBAC-autorisatie][kubernetes-rbac].

Als u wilt toestaan dat het Azure-platform om de vereiste voor opslag-resources te maken, maakt een *ClusterRole* en *ClusterRoleBinding*. Maak een bestand met de naam `azure-pvc-roles.yaml` en kopieer de volgende YAML:

```yaml
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: system:azure-cloud-provider
rules:
- apiGroups: ['']
  resources: ['secrets']
  verbs:     ['get','create']
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: system:azure-cloud-provider
roleRef:
  kind: ClusterRole
  apiGroup: rbac.authorization.k8s.io
  name: system:azure-cloud-provider
subjects:
- kind: ServiceAccount
  name: persistent-volume-binder
  namespace: kube-system
```

De machtigingen met de [kubectl toepassen] [ kubectl-apply] opdracht:

```console
kubectl apply -f azure-pvc-roles.yaml
```

## <a name="create-a-persistent-volume-claim"></a>Maken van een claim permanent volume

Een claim permanent volume (PVC) maakt gebruik van het opslagobject klasse om u te richten op dynamische wijze een Azure-bestandsshare. De volgende YAML kan worden gebruikt voor het maken van een claim permanent volume *5GB* in grootte met *ReadWriteMany* toegang. Zie voor meer informatie over toegangsmodi in de [Kubernetes permanent volume] [ access-modes] documentatie.

Maak nu een bestand met de naam `azure-file-pvc.yaml` en kopieer de volgende YAML. Zorg ervoor dat de *storageClassName* komt overeen met de opslagklasse in de laatste stap hebt gemaakt:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: azurefile
spec:
  accessModes:
    - ReadWriteMany
  storageClassName: azurefile
  resources:
    requests:
      storage: 5Gi
```

Maken van de claim permanent volume met de [kubectl toepassen] [ kubectl-apply] opdracht:

```console
kubectl apply -f azure-file-pvc.yaml
```

Als voltooid, kunt u de bestandsshare wordt gemaakt. Een Kubernetes-geheim wordt ook gemaakt met verbindingsgegevens en referenties. U kunt de [kubectl ophalen] [ kubectl-get] opdracht om de status van de PVC weer te geven:

```
$ kubectl get pvc azurefile

NAME        STATUS    VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
azurefile   Bound     pvc-8436e62e-a0d9-11e5-8521-5a8664dc0477   5Gi        RWX            azurefile      5m
```

## <a name="use-the-persistent-volume"></a>Gebruik de permanent volume

De volgende YAML maakt een schil die gebruikmaakt van de claim permanent volume *azurefile* te koppelen van de Azure-bestandsshare op de */mnt/azure* pad.

Maak een bestand met de naam `azure-pvc-files.yaml`, en kopieer de volgende YAML. Zorg ervoor dat de *claimName* komt overeen met het PVC in de laatste stap hebt gemaakt.

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: mypod
spec:
  containers:
  - name: mypod
    image: nginx:1.15.5
    resources:
      requests:
        cpu: 100m
        memory: 128Mi
      limits:
        cpu: 250m
        memory: 256Mi
    volumeMounts:
    - mountPath: "/mnt/azure"
      name: volume
  volumes:
    - name: volume
      persistentVolumeClaim:
        claimName: azurefile
```

Maken van de schil met de [kubectl toepassen] [ kubectl-apply] opdracht.

```console
kubectl apply -f azure-pvc-files.yaml
```

U hebt nu een pod uitgevoerd met uw Azure-bestandsshare die is gekoppeld in de */mnt/azure* directory. Deze configuratie kan worden weergegeven bij de inspectie van uw schil via `kubectl describe pod mypod`. De uitvoer van de volgende verkorte voorbeeld ziet u het volume is gekoppeld in de container:

```
Containers:
  mypod:
    Container ID:   docker://053bc9c0df72232d755aa040bfba8b533fa696b123876108dec400e364d2523e
    Image:          nginx:1.15.5
    Image ID:       docker-pullable://nginx@sha256:d85914d547a6c92faa39ce7058bd7529baacab7e0cd4255442b04577c4d1f424
    State:          Running
      Started:      Wed, 15 Aug 2018 22:22:27 +0000
    Ready:          True
    Mounts:
      /mnt/azure from volume (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-8rv4z (ro)
[...]
Volumes:
  volume:
    Type:       PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
    ClaimName:  azurefile
    ReadOnly:   false
[...]
```

## <a name="mount-options"></a>Koppelingsopties

Standaard *fileMode* en *dirMode* waarden verschillen tussen de Kubernetes-versies, zoals beschreven in de volgende tabel.

| versie | waarde |
| ---- | ---- |
| V1.6.x, v1.7.x | 0777 |
| v1.8.0-v1.8.5 | 0700 |
| V1.8.6 of hoger | 0755 |
| v1.9.0 | 0700 |
| V1.9.1 of hoger | 0755 |

Als een cluster van versie 1.8.5 of hoger en dynamisch de permanent volume maken met een opslagklasse, koppelingsopties kunnen worden opgegeven voor het opslagobject klasse. Het volgende voorbeeld wordt *0777*:

```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: azurefile
provisioner: kubernetes.io/azure-file
mountOptions:
  - dir_mode=0777
  - file_mode=0777
  - uid=1000
  - gid=1000
parameters:
  skuName: Standard_LRS
```

Als een cluster van versie 1.8.5 of hoger en het maken van het object permanent volume statisch, koppelingsopties moeten worden opgegeven op de *PersistentVolume* object. Zie voor meer informatie over het maken van een permanent volume statisch [statische permanente Volumes][pv-static].

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: azurefile
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteMany
  azureFile:
    secretName: azure-secret
    shareName: azurefile
    readOnly: false
  mountOptions:
  - dir_mode=0777
  - file_mode=0777
  - uid=1000
  - gid=1000
```

Als een cluster van versie 1.8.0 - 1.8.4, een beveiligingscontext kan worden opgegeven met de *uitvoerenals* waarde ingesteld op *0*. Zie voor meer informatie over de beveiligingscontext Pod [configureren van een beveiligingscontext][kubernetes-security-context].

## <a name="next-steps"></a>Volgende stappen

Meer informatie over Kubernetes permanente volumes met behulp van Azure Files.

> [!div class="nextstepaction"]
> [Kubernetes-invoegtoepassing voor Azure Files][kubernetes-files]

<!-- LINKS - external -->
[access-modes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubernetes-files]: https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_file/README.md
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[kubernetes-security-context]: https://kubernetes.io/docs/tasks/configure-pod-container/security-context/
[kubernetes-storage-classes]: https://kubernetes.io/docs/concepts/storage/storage-classes/#azure-file
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/
[pv-static]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/#static
[kubernetes-rbac]: https://kubernetes.io/docs/reference/access-authn-authz/rbac/
[smb-overview]: /windows/desktop/FileIO/microsoft-smb-protocol-and-cifs-protocol-overview

<!-- LINKS - internal -->
[az-group-create]: /cli/azure/group#az-group-create
[az-group-list]: /cli/azure/group#az-group-list
[az-resource-show]: /cli/azure/aks#az-aks-show
[az-storage-account-create]: /cli/azure/storage/account#az-storage-account-create
[az-storage-create]: /cli/azure/storage/account#az-storage-account-create
[az-storage-key-list]: /cli/azure/storage/account/keys#az-storage-account-keys-list
[az-storage-share-create]: /cli/azure/storage/share#az-storage-share-create
[mount-options]: #mount-options
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[az-aks-show]: /cli/azure/aks#az-aks-show
