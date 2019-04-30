---
title: De referenties voor een cluster Azure Kubernetes Service (AKS) opnieuw instellen
description: Informatie over hoe bijwerken of opnieuw instellen van de service-principal-referenties voor een cluster in Azure Kubernetes Service (AKS)
services: container-service
author: rockboyfor
ms.service: container-service
ms.topic: article
origin.date: 01/30/2019
ms.date: 03/04/2019
ms.author: v-yeche
ms.openlocfilehash: d880615d0d132403c935fe39e8478d7b3fc48dbe
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "61029352"
---
# <a name="update-or-rotate-the-credentials-for-a-service-principal-in-azure-kubernetes-service-aks"></a>Bijwerken of het draaien van de referenties voor een service-principal in Azure Kubernetes Service (AKS)

AKS-clusters worden standaard gemaakt met een service-principal met een verlooptijd van één jaar. U kunt de referenties voor het uitbreiden van de service-principal voor een aanvullende periode opnieuw als u in de buurt van de vervaldatum. U kunt ook te werken of te draaien, de referenties als onderdeel van een gedefinieerde beveiligingsbeleid. Dit artikel wordt uitgelegd hoe u kunt deze referenties voor een AKS-cluster bijwerken.

## <a name="before-you-begin"></a>Voordat u begint

U moet de Azure CLI versie 2.0.56 of later geïnstalleerd en geconfigureerd. Voer  `az --version` uit om de versie te bekijken. Als u de Azure CLI wilt installeren of upgraden, raadpleegt u  [Azure CLI installeren][install-azure-cli].

## <a name="choose-to-update-or-create-a-service-principal"></a>Ervoor kiezen om te werken of een service-principal maken

Wanneer u bijwerken van de referenties voor een AKS-cluster wilt, kunt u om te kiezen:

* werk de referenties voor de bestaande service-principal die worden gebruikt door het cluster, of
* een service-principal maken en bijwerken van het cluster voor het gebruik van deze nieuwe referenties.

Als u wilt een service-principal maken en bijwerken van het AKS-cluster, de rest van de stappen in deze sectie overslaan en doorgaan naar [maken van een service-principal](#create-a-service-principal). Als u bijwerken van de referenties voor de bestaande service-principal die door het AKS-cluster gebruikt wilt, gaat u verder met de stappen in deze sectie.

### <a name="get-the-service-principal-id"></a>De service-principal-ID ophalen

Voor het bijwerken van de referenties voor de bestaande service-principal, krijgen de service-principal-ID van het gebruik van uw cluster de [az aks show] [ az-aks-show] opdracht. Het volgende voorbeeld wordt de ID voor het cluster met de naam *myAKSCluster* in de *myResourceGroup* resourcegroep. De service-principal-ID is ingesteld als een variabele voor gebruik in extra opdracht.

```azurecli
SP_ID=$(az aks show -g myResourceGroup -n myAKSCluster --query servicePrincipalProfile.clientId -o tsv)
```

### <a name="update-the-service-principal-credentials"></a>De referenties voor de service-principal bijwerken

Met een variabele is ingesteld met de service-principal-ID, nu de referenties opnieuw instellen met behulp van [az ad sp referentie opnieuw][az-ad-sp-credential-reset]. Het volgende voorbeeld kunt het Azure-platform voor het genereren van een nieuwe beveiligde geheim voor de service-principal. Dit nieuwe beveiligde geheim wordt ook opgeslagen als een variabele.

```azurecli
SP_SECRET=$(az ad sp credential reset --name $SP_ID --query password -o tsv)
```

Nu gaat u naar [update AKS-cluster met de nieuwe referenties](#update-aks-cluster-with-new-credentials).

## <a name="create-a-service-principal"></a>Een service-principal maken

Als u ervoor kiest om bij te werken van de bestaande service-principal referenties in de vorige sectie, moet u deze stap overslaan. Blijven [update AKS-cluster met de nieuwe referenties](#update-aks-cluster-with-new-credentials).

Voor het maken van een service-principal en werk vervolgens de AKS-cluster voor het gebruik van deze nieuwe referenties, gebruikt u de [az ad sp create-for-rbac] [ az-ad-sp-create] opdracht. In het volgende voorbeeld wordt met de parameter `--skip-assignment` voorkomen dat eventuele extra standaardtoewijzingen worden toegewezen:

```azurecli
az ad sp create-for-rbac --skip-assignment
```

De uitvoer lijkt op die in het volgende voorbeeld. Noteer uw eigen `appId` en `password`. Deze waarden worden gebruikt in de volgende stap.

```json
{
  "appId": "7d837646-b1f3-443d-874c-fd83c7c739c5",
  "name": "7d837646-b1f3-443d-874c-fd83c7c739c",
  "password": "a5ce83c9-9186-426d-9183-614597c7f2f7",
  "tenant": "a4342dc8-cd0e-4742-a467-3129c469d0e5"
}
```

Nu Definieer variabelen voor de service principal-ID en -geheim met behulp van de uitvoer van uw eigen [az ad sp create-for-rbac] [ az-ad-sp-create] opdracht, zoals wordt weergegeven in het volgende voorbeeld. De *SP_ID* is uw *appId*, en de *SP_SECRET* is uw *wachtwoord*:

```azurecli
SP_ID=7d837646-b1f3-443d-874c-fd83c7c739c5
SP_SECRET=a5ce83c9-9186-426d-9183-614597c7f2f7
```

## <a name="update-aks-cluster-with-new-credentials"></a>AKS-cluster met de nieuwe referenties bijwerken

Ongeacht of u de referenties voor de bestaande service-principal bijwerken of een service-principal maken kiest, u nu bijwerken het AKS-cluster met uw nieuwe referenties met behulp van de [az aks-referenties voor update] [ az-aks-update-credentials] opdracht. De variabelen voor de *--service-principal* en *--client-secret* worden gebruikt:

```azurecli
az aks update-credentials \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --reset-service-principal \
    --service-principal $SP_ID \
    --client-secret $SP_SECRET
```

Het duurt een paar minuten voor de referenties voor de service-principal moet worden bijgewerkt in de AKS.

## <a name="next-steps"></a>Volgende stappen

In dit artikel wordt is de service-principal voor het AKS-cluster zelf bijgewerkt. Zie voor meer informatie over het beheren van identiteit voor werkbelastingen binnen een cluster [aanbevolen procedures voor verificatie en autorisatie in AKS][best-practices-identity].

<!-- LINKS - internal -->
[install-azure-cli]: https://docs.azure.cn/zh-cn/cli/install-azure-cli?view=azure-cli-latest
[az-aks-show]: https://docs.microsoft.com/cli/azure/aks?view=azure-cli-latest#az-aks-show
[az-aks-update-credentials]: https://docs.microsoft.com/cli/azure/aks?view=azure-cli-latest#az-aks-update-credentials
[best-practices-identity]: operator-best-practices-identity.md
[az-ad-sp-create]: https://docs.azure.cn/zh-cn/cli/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac
[az-ad-sp-credential-reset]: https://docs.azure.cn/zh-cn/cli/ad/sp/credential?view=azure-cli-latest#az-ad-sp-credential-reset