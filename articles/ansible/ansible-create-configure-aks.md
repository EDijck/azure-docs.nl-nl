---
title: Azure Kubernetes Service-clusters maken en configureren in Azure met behulp van Ansible
description: Leer hoe u Ansible gebruikt om een Azure Kubernetes Service-cluster in Azure te maken en beheren
ms.service: azure
keywords: ansible, azure, devops, bash, cloudshell, playbook, aks, container, Kubernetes
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 08/23/2018
ms.openlocfilehash: 2270a9225d26329f3d78d78895223aaa6ccc855f
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58176392"
---
# <a name="create-and-configure-azure-kubernetes-service-clusters-in-azure-using-ansible"></a>Azure Kubernetes Service-clusters maken en configureren in Azure met behulp van Ansible
U kunt Ansible ook gebruiken om de implementatie en configuratie van resources in uw omgeving te automatiseren. U kunt Ansible gebruiken voor het beheren van uw Azure Kubernetes Service (AKS). Dit artikel laat zien hoe u Ansible gebruikt om een Azure Kubernetes Service-cluster te maken en beheren.

## <a name="prerequisites"></a>Vereisten
- **Azure-abonnement**: als u geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) aan voordat u begint.
- **Azure service-principal**: houd bij [het maken van de service-principal](/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest) rekening met de volgende waarden: **appId**, **displayName**, **wachtwoord**, en **tenant**.

- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)] [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation2.md)]

> [!Note]
> Ansible 2.6 is vereist voor het uitvoeren van de volgende playbooks-voorbeelden in deze zelfstudie.

## <a name="create-a-managed-aks-cluster"></a>Een beheerd AKS-cluster maken
De code in dit gedeelte is een Ansible-playbook-voorbeeld voor het maken van een resourcegroep en een AKS-cluster dat zich in de resourcegroep bevindt.

> [!Tip]
> Voor de tijdelijke aanduiding `your_ssh_key` voert u uw openbare RSA-sleutel in in de indeling met één regel, beginnend met 'ssh-rsa' (zonder aanhalingstekens).

  ```yaml
  - name: Create Azure Kubernetes Service
    hosts: localhost
    connection: local
    vars:
      resource_group: myResourceGroup
      location: eastus
      aks_name: myAKSCluster
      username: azureuser
      ssh_key: "your_ssh_key"
      client_id: "your_client_id"
      client_secret: "your_client_secret"
    tasks:
    - name: Create resource group
      azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        location: "{{ location }}"
    - name: Create a managed Azure Container Services (AKS) cluster
      azure_rm_aks:
        name: "{{ aks_name }}"
        location: "{{ location }}"
        resource_group: "{{ resource_group }}"
        dns_prefix: "{{ aks_name }}"
        linux_profile:
          admin_username: "{{ username }}"
          ssh_key: "{{ ssh_key }}"
        service_principal:
          client_id: "{{ client_id }}"
          client_secret: "{{ client_secret }}"
        agent_pool_profiles:
          - name: default
            count: 2
            vm_size: Standard_D2_v2
        tags:
          Environment: Production
  ```

De onderstaande lijst met opsommingstekens helpt om bovenstaande code van het Ansible-playbook uit te leggen:
- De eerste sectie binnen **taken** definieert een resourcegroep met de naam **myResourceGroup** binnen de locatie **eastus**.
- De tweede sectie in **taken** definieert een AKS-cluster met de naam **myAKSCluster** binnen de resourcegroep **myResourceGroup**.

Voor het maken van het AKS-cluster met Ansible slaat u het voorgaande playbook-voorbeeld op als `azure_create_aks.yml`, en voert u het playbook uit met de volgende opdracht:

  ```bash
  ansible-playbook azure_create_aks.yml
  ```

De uitvoer van de **ansible-playbook*-opdracht lijkt op het volgende, dat laat zien dat het AKS-cluster is aangemaakt:

  ```Output
  PLAY [Create AKS] ****************************************************************************************

  TASK [Gathering Facts] ********************************************************************************************
  ok: [localhost]

  TASK [Create resource group] **************************************************************************************
  changed: [localhost]

  TASK [Create an Azure Container Services (AKS) cluster] ***************************************************
  changed: [localhost]

  PLAY RECAP *********************************************************************************************************
  localhost                  : ok=3    changed=2    unreachable=0    failed=0
  ```

## <a name="scale-aks-nodes"></a>AKS-knooppunten schalen

Het voorbeeld-playbook in de vorige sectie definieert twee knooppunten. U kunt het aantal knooppunten eenvoudig aanpassen als u meer of minder container-workloads in uw cluster nodig heeft. Het voorbeeld-playbook in deze sectie verhoogt het aantal knooppunten van twee knooppunten tot drie. Het wijzigen van het aantal knooppunten wordt uitgevoerd door het wijzigen van de waarde **aantal** in het blok **agent_pool_profiles**.

> [!Tip]
> Voor de tijdelijke aanduiding `your_ssh_key` voert u uw openbare RSA-sleutel in in de indeling met één regel, beginnend met 'ssh-rsa' (zonder aanhalingstekens).

```yaml
- name: Scale AKS cluster
  hosts: localhost
  connection: local
  vars:
    resource_group: myResourceGroup
    location: eastus
    aks_name: myAKSCluster
    username: azureuser
    ssh_key: "your_ssh_key"
    client_id: "your_client_id"
    client_secret: "your_client_secret"
  tasks:
  - name: Scaling an existed AKS cluster
    azure_rm_aks:
        name: "{{ aks_name }}"
        location: "{{ location }}"
        resource_group: "{{ resource_group }}"
        dns_prefix: "{{ aks_name }}"
        linux_profile:
          admin_username: "{{ username }}"
          ssh_key: "{{ ssh_key }}"
        service_principal:
          client_id: "{{ client_id }}"
          client_secret: "{{ client_secret }}"
        agent_pool_profiles:
          - name: default
            count: 3
            vm_size: Standard_D2_v2
```

Als u het Azure Kubernetes Service-cluster wilt schalen met Ansible, slaat u het voorgaande playbook op als *azure_configure_aks.yml* en voert u het playbook als volgt uit:

  ```bash
  ansible-playbook azure_configure_aks.yml
  ```

In de volgende uitvoer ziet u dat het AKS-cluster is aangemaakt:

  ```Output
  PLAY [Scale AKS cluster] ***************************************************************

  TASK [Gathering Facts] ******************************************************************
  ok: [localhost]

  TASK [Scaling an existed AKS cluster] **************************************************
  changed: [localhost]

  PLAY RECAP ******************************************************************************
  localhost                  : ok=2    changed=1    unreachable=0    failed=0
  ```
## <a name="delete-a-managed-aks-cluster"></a>Wis een beheerd AKS-cluster

In het volgende gedeelte van een Ansible-playbook-voorbeeld ziet u hoe u een AKS-cluster verwijdert:

  ```yaml
  - name: Delete a managed Azure Container Services (AKS) cluster
    hosts: localhost
    connection: local
    vars:
      resource_group: myResourceGroup
      aks_name: myAKSCluster
    tasks:
    - name:
      azure_rm_aks:
        name: "{{ aks_name }}"
        resource_group: "{{ resource_group }}"
        state: absent
   ```

Als u het Azure Kubernetes Service-cluster met Ansible wilt verwijderen, slaat u het voorgaande playbook op als *azure_delete_aks.yml* en voert u het playbook als volgt uit:

  ```bash
  ansible-playbook azure_delete_aks.yml
  ```

In de volgende uitvoer ziet u dat het AKS-cluster is verwijderd:
  ```Output
PLAY [Delete a managed Azure Container Services (AKS) cluster] ****************************

TASK [Gathering Facts] ********************************************************************
ok: [localhost]

TASK [azure_rm_aks] *********************************************************************

PLAY RECAP *********************************************************************
localhost                  : ok=2    changed=1    unreachable=0    failed=0
  ```

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Zelfstudie: een toepassing schalen in AKS (Azure Kubernetes Service)](https://docs.microsoft.com/azure/aks/tutorial-kubernetes-scale)
