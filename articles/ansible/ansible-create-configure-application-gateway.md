---
title: Ansible gebruiken om webverkeer te beheren met Azure Application Gateway
description: Meer informatie over hoe u Ansible gebruikt om een Azure Application Gateway te maken en te configureren voor het beheren van webverkeer
ms.service: ansible
keywords: ansible, azure, devops, bash, playbook, application gateway, load balancer, webverkeer
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 09/20/2018
ms.openlocfilehash: af7f22ae5c289a01e6876d8ce586cb32383c8d3b
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2018
ms.locfileid: "53253349"
---
# <a name="manage-web-traffic-with-azure-application-gateway-by-using-ansible"></a>Ansible gebruiken om webverkeer te beheren met Azure Application Gateway

[Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/) is een load balancer voor webverkeer waarmee u het verkeer naar uw webapps kunt beheren.

Ansible kan worden gebruikt om de implementatie en configuratie van resources in uw omgeving te automatiseren. In dit artikel leest u hoe u Ansible gebruikt om een toepassingsgateway te maken. U kunt hier ook lezen u hoe u de gateway kunt gebruiken voor het beheren van verkeer naar twee webservers die worden uitgevoerd in Azure-containerinstanties.

In deze zelfstudie ontdekt u hoe u:

> [!div class="checklist"]
> * Het netwerk instellen
> * Twee Azure-containerinstanties maken met HTTPD-installatiekopieën
> * Een toepassingsgateway maken die werkt met de Azure-containerinstanties in de serverpool

## <a name="prerequisites"></a>Vereisten

- **Azure-abonnement**: als u geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) aan voordat u begint.
- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)] [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation2.md)]

> [!Note]
> Ansible 2.7 is vereist om de volgende voorbeeld-playbooks in deze zelfstudie uit te voeren. 

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd.  

In het volgende voorbeeld wordt een resourcegroep met de naam **myResourceGroup** gemaakt op de locatie **VS - oost**.

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    location: eastus 
  tasks:
    - name: Create a resource group
      azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        location: "{{ location }}"
```

Sla dit playbook op als *rg.yml*. Als u het playbook wilt uitvoeren, gebruikt u de opdracht **ansible-playbook** als volgt:

```bash
ansible-playbook rg.yml
```

## <a name="create-network-resources"></a>Netwerkbronnen maken

Maak eerst een virtueel netwerk om de toepassingsgateway in staat te stellen te communiceren met andere resources.

In het volgende voorbeeld worden een virtueel netwerk met de naam **myVNet**, een subnet met de naam **myAGSubnet** en een openbaar IP-adres met de naam **myAGPublicIPAddress** met een domein met de naam **mydomain** gemaakt.

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    location: eastus 
    vnet_name: myVNet 
    subnet_name: myAGSubnet 
    publicip_name: myAGPublicIPAddress
    publicip_domain: mydomain
  tasks:
    - name: Create a virtual network
      azure_rm_virtualnetwork:
        name: "{{ vnet_name }}"
        resource_group: "{{ resource_group }}"
        address_prefixes_cidr:
            - 10.1.0.0/16
            - 172.100.0.0/16
        dns_servers:
            - 127.0.0.1
            - 127.0.0.2

    - name: Create a subnet
      azure_rm_subnet:
        name: "{{ subnet_name }}"
        virtual_network_name: "{{ vnet_name }}"
        resource_group: "{{ resource_group }}"
        address_prefix_cidr: 10.1.0.0/24

    - name: Create a public IP address
      azure_rm_publicipaddress:
        resource_group: "{{ resource_group }}" 
        allocation_method: Dynamic
        name: "{{ publicip_name }}"
        domain_name_label: "{{ publicip_domain }}"
```

Sla dit playbook op als *vnet_create.yml*. Als u het playbook wilt uitvoeren, gebruikt u de opdracht **ansible-playbook** als volgt:

```bash
ansible-playbook vnet_create.yml
```

## <a name="create-servers"></a>Servers maken

In het volgende voorbeeld wordt getoond hoe u twee Azure-containerinstanties met HTTPD-installatiekopieën maakt die worden gebruikt als webservers voor de toepassingsgateway.  

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    location: eastus 
    aci_1_name: myACI1
    aci_2_name: myACI2
  tasks:
    - name: Create a container with httpd image 
      azure_rm_containerinstance:
        resource_group: "{{ resource_group }}"
        name: "{{ aci_1_name }}"
        os_type: linux
        ip_address: public
        location: "{{ location }}"
        ports:
          - 80
        containers:
          - name: mycontainer
            image: httpd
            memory: 1.5
            ports:
              - 80

    - name: Create another container with httpd image 
      azure_rm_containerinstance:
        resource_group: "{{ resource_group }}"
        name: "{{ aci_2_name }}"
        os_type: linux
        ip_address: public
        location: "{{ location }}"
        ports:
          - 80
        containers:
          - name: mycontainer
            image: httpd
            memory: 1.5
            ports:
              - 80
```

Sla dit playbook op als *aci_create.yml*. Als u het playbook wilt uitvoeren, gebruikt u de opdracht **ansible-playbook** als volgt:

```bash
ansible-playbook aci_create.yml
```

## <a name="create-the-application-gateway"></a>De toepassingsgateway maken

In het volgende voorbeeld wordt een toepassingsgateway met de naam **myAppGateway** gemaakt, met configuraties voor back-end-, front-end- en HTTP.  

* **appGatewayIP** wordt gedefinieerd in het blok **gateway_ip_configurations**. Er is een subnetverwijzing vereist voor de IP-configuratie van de gateway.
* **appGatewayBackendPool** wordt gedefinieerd in het blok **backend_address_pools**. Een toepassingsgateway moet ten minste één back-endadresgroep hebben.
* **appGatewayBackendHttpSettings** wordt gedefinieerd in het blok **backend_http_settings_collection**. Hiermee wordt aangegeven dat voor de communicatie poort 80 en een HTTP-protocol worden gebruikt.
* **appGatewayHttpListener** wordt gedefinieerd in het blok **backend_http_settings_collection**. Dit is de standaard-listener die aan appGatewayBackendPool is gekoppeld.
* **appGatewayFrontendIP** wordt gedefinieerd in het blok **frontend_ip_configurations**. Hiermee wordt myAGPublicIPAddress aan appGatewayHttpListener toegewezen.
* **Rule1** wordt gedefinieerd in het blok **request_routing_rules**. Dit is de standaardregel voor doorsturen die aan appGatewayHttpListener is gekoppeld.

```yml
- hosts: localhost
  connection: local
  vars:
    resource_group: myResourceGroup
    vnet_name: myVNet
    subnet_name: myAGSubnet
    location: eastus
    publicip_name: myAGPublicIPAddress
    appgw_name: myAppGateway
    aci_1_name: myACI1
    aci_2_name: myACI2
  tasks:
    - name: Get info of Subnet
      azure_rm_resource_facts:
        api_version: '2018-08-01'
        resource_group: "{{ resource_group }}"
        provider: network
        resource_type: virtualnetworks
        resource_name: "{{ vnet_name }}"
        subresource:
          - type: subnets
            name: "{{ subnet_name }}"
      register: subnet

    - name: Get info of backend server 2
      azure_rm_resource_facts:
        api_version: '2018-04-01'
        resource_group: "{{ resource_group }}"
        provider: containerinstance
        resource_type: containergroups
        resource_name: "{{ aci_1_name }}"
      register: aci_1_output
    - name: Get info of backend server 2
      azure_rm_resource_facts:
        api_version: '2018-04-01'
        resource_group: "{{ resource_group }}"
        provider: containerinstance
        resource_type: containergroups
        resource_name: "{{ aci_2_name }}"
      register: aci_2_output

    - name: Create instance of Application Gateway
      azure_rm_appgateway:
        resource_group: "{{ resource_group }}"
        name: "{{ appgw_name }}"
        sku:
          name: standard_small
          tier: standard
          capacity: 2
        gateway_ip_configurations:
          - subnet:
              id: "{{ subnet.response[0].id }}"
            name: appGatewayIP
        frontend_ip_configurations:
          - public_ip_address: "{{ publicip_name }}"
            name: appGatewayFrontendIP
        frontend_ports:
          - port: 80
            name: appGatewayFrontendPort
        backend_address_pools:
          - backend_addresses:
              - ip_address: "{{ aci_1_output.response[0].properties.ipAddress.ip }}"
              - ip_address: "{{ aci_2_output.response[0].properties.ipAddress.ip }}"
            name: appGatewayBackendPool
        backend_http_settings_collection:
          - port: 80
            protocol: http
            cookie_based_affinity: enabled
            name: appGatewayBackendHttpSettings
        http_listeners:
          - frontend_ip_configuration: appGatewayFrontendIP
            frontend_port: appGatewayFrontendPort
            name: appGatewayHttpListener
        request_routing_rules:
          - rule_type: Basic
            backend_address_pool: appGatewayBackendPool
            backend_http_settings: appGatewayBackendHttpSettings
            http_listener: appGatewayHttpListener
            name: rule1
```

Sla dit playbook op als*appgw_create.yml*. Als u het playbook wilt uitvoeren, gebruikt u de opdracht **ansible-playbook** als volgt:

```bash
ansible-playbook appgw_create.yml
```

Het kan enkele minuten duren voordat de toepassingsgateway is gemaakt.

## <a name="test-the-application-gateway"></a>De toepassingsgateway testen

In het voorbeeldplaybook voor netwerkresources hebt u het domein met de naam **mydomain** gemaakt in **eastus**. Ga naar `http://mydomain.eastus.cloudapp.azure.com` in uw browser. Als u de volgende pagina ziet, werkt de toepassingsgateway zoals verwacht.

![Testen of een toepassingsgateway werkt](media/ansible-create-configure-application-gateway/applicationgateway.PNG)

## <a name="clean-up-resources"></a>Resources opschonen

Als u deze resources niet nodig hebt, kunt u ze verwijderen door de volgende code uit te voeren. Hiermee verwijdert u een resourcegroep met de naam **myResourceGroup**.

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
  tasks:
    - name: Delete a resource group
      azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        state: absent
```

Sla het dit playbook op als *rg_delete*.yml. Als u het playbook wilt uitvoeren, gebruikt u de opdracht **ansible-playbook** als volgt:

```bash
ansible-playbook rg_delete.yml
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Ansible in Azure](https://docs.microsoft.com/azure/ansible/)
