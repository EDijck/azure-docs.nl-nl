---
title: 'Quickstart: Een standaard load balancer maken - Azure Portal'
titlesuffix: Azure Load Balancer
description: In deze snelstart vindt u informatie over het maken van een standaard load balancer via Azure Portal.
services: load-balancer
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
tags: azure-resource-manager
Customer intent: I want to create a Standard Load balancer so that I can load balance internet traffic to VMs.
ms.assetid: ''
ms.service: load-balancer
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/26/2019
ms.author: kumud
ms.custom: mvc
ms.openlocfilehash: 97d1cf2817ebfbf2eb1a6ba5a4d20d457b6369c6
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2019
ms.locfileid: "56961734"
---
# <a name="quickstart-create-a-standard-load-balancer-to-load-balance-vms-using-the-azure-portal"></a>Quickstart: Een Standard load balancer maken om taken van VM's te verdelen via Azure Portal

Taakverdeling zorgt voor een hogere beschikbaarheid en betere schaalbaarheid door binnenkomende aanvragen te spreiden over meerdere virtuele machines. U kunt in Azure Portal een load balancer maken om taken van virtuele machines (VM's) te verdelen. In deze snelstart wordt getoond hoe u taken van VM's kunt verdelen met een Standard Load Balancer.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint. 

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij de Azure Portal op [http://portal.azure.com](http://portal.azure.com).

## <a name="create-a-public-load-balancer"></a>Openbare load balancer maken

In deze sectie maakt u een openbare load balancer die helpt bij het laden van virtuele machines. Standard Load Balancer biedt alleen ondersteuning voor een standaard, openbaar IP-adres. Als u een Standard Load Balancer maakt, moet u ook een nieuw, standaard, openbaar IP-adres maken dat als de front-end (standaard *LoadBalancerFrontend* genoemd) wordt geconfigureerd voor de Standard Load Balancer. 

1. Klik linksboven in het scherm op **Een resource maken** > **Netwerken** > **Load balancer**.
2. Voer op het tabblad **Basis** van de pagina **Load balancer maken** de volgende gegevens in of selecteer deze, accepteer de standaardwaarden voor de overige instellingen en selecteer **Controleren + maken**:

    | Instelling                 | Waarde                                              |
    | ---                     | ---                                                |
    | Abonnement               | Selecteer uw abonnement.    |    
    | Resourcegroep         | Selecteer **Nieuwe maken** en typ *MyResourceGroupSLB* in het tekstvak.|
    | Naam                   | *myLoadBalancer*                                   |
    | Regio         | Selecteer **Europa - west**.                                        |
    | Type          | Selecteer **Openbaar**.                                        |
    | SKU           | selecteer **Standaard**.                          |
    | Openbaar IP-adres | Selecteer **Nieuw maken**. |
    | Naam openbaar IP-adres              | Typ *myPublicIP* in het tekstvak.   |
    |Beschikbaarheidszone| Selecteer **Zone-redundant**.    |
3. Klik op het tabblad **Beoordelen en maken** op **Maken**.   

## <a name="create-backend-servers"></a>Back-endservers maken

In deze sectie maakt u een virtueel netwerk, twee virtuele machines voor de back-endpool van de load balancer en installeert u IIS op de virtuele machines om de load balancer te testen.

### <a name="create-a-virtual-network"></a>Een virtueel netwerk maken
1. Klik linksboven in het scherm op **Nieuw** > **Netwerken** > **Virtueel netwerk** en voer deze waarden in voor het virtuele netwerk:
    - *myVnet* als naam van het virtuele netwerk.
    - *myResourceGroupSLB* als naam van de bestaande resourcegroep
    - *myBackendSubnet* als naam van het subnet.
2. Klik op **Maken** om het virtuele netwerk te maken.

    ![Een virtueel netwerk maken](./media/load-balancer-standard-public-portal/2-load-balancer-virtual-network.png)

### <a name="create-virtual-machines"></a>Virtuele machines maken

1. Klik linksboven in het scherm op **Nieuw** > **Compute** > **Windows Server 2016 Datacenter** en voer deze waarden in voor de virtuele machine:
    - *myVM1* als naam van de virtuele machine.        
    - *myResourceGroupSLB*: voor **Resource group** selecteert u **Bestaande gebruiken** en vervolgens *myResourceGroupSLB*.
2. Klik op **OK**.
3. Selecteer **DS1_V2** als grootte van de virtuele machine en klik op **Selecteren**.
4. Voer deze waarden in voor de instellingen van de VM:
    1. Zorg ervoor dat *myVNet* is geselecteerd voor het virtuele netwerk en dat het subnet *myBackendSubnet* is.
    2. Selecteer als **Openbaar IP-adres**, in het venster **Openbaar IP-adres maken**, **Standard**, en selecteer vervolgens **OK**.
    3. Selecteer voor **Netwerkbeveiligingsgroep** **Geavanceerd**, en doe vervolgens het volgende:
        1. Selecteer *Netwerkbeveiligingsgroep (firewall), en de pagina **Netwerkbeveiligingsgroep kiezen**, selecteer **Nieuwe maken**. 
        2. Op de pagina **Netwerkbeveiligingsgroep maken** voert u bij **Naam** *myNetworkSecurityGroup* in en selecteert vervolgens **OK**.
5. Klik op **Uitgeschakeld** om diagnostische gegevens over opstarten uit te schakelen.
6. Klik op **OK**, controleer de instellingen op de overzichtspagina en klik op **Maken**.
7. Maak met behulp van stap 1-6 een tweede VM met de naam *VM2*, met *myVnet* als het virtuele netwerk, *myBackendSubnet* als het subnet, en **myNetworkSecurityGroup* als de netwerkbeveiligingsgroep. 

### <a name="create-nsg-rule"></a>Een NSG-regel maken

In deze sectie maakt u een NSG-regel om inkomende verbindingen via HTTP toe te staan.

1. Klik in het linkermenu op **Alle resources** en klik in de lijst met resources op **myNetworkSecurityGroup**, die zich in de resourcegroep **myResourceGroupSLB** bevindt.
2. Klik onder **Instellingen** op **Inkomende beveiligingsregels** en vervolgens op **Toevoegen**.
3. Voer deze waarden in voor de inkomende beveiligingsregel met de naam *myHTTPRule* om een binnenkomende HTTP-verbinding via poort 80 toe te staan:
    - *Service Tag* bij **Bron**.
    - *Internet* bij **Bronservicetag**
    - *80* bij **Poortbereiken van doel**
    - *TCP* bij **Protocol**
    - *Allow* bij **Actie**
    - *100* bij **Prioriteit**
    - *myHTTPRule* als naam
    - *Allow HTTP* als beschrijving
4. Klik op **OK**.
 
### <a name="install-iis"></a>IIS installeren

1. Klik in het linkermenu op **Alle resources** en klik in de lijst met resources op **myVM1**, die zich in de resourcegroep *myResourceGroupLB* bevindt.
2. Klik op de pagina **Overzicht** op **Verbinding maken** om extern verbinding te maken met de VM.
3. Meld u aan bij de VM met gebruikersnaam *azureuser*.
4. Ga op de serverdesktop naar **Windows Systeembeheer**>**Serverbeheer**.
5. Klik in Serverbeheer op **Rollen en functies toevoegen**.
6. Gebruik in de **wizard Rollen en functies toevoegen** de volgende waarden:
    - Klik op de pagina **Type installatie selecteren** op **Installatie die op de functie of het onderdeel is gebaseerd**.
    - Klik op de pagina **De doelserver selecteren** op **myVM1**
    - Klik op de pagina **Een serverfunctie selecteren** op **Webserver (IIS)**
    - Volg de aanwijzingen om de rest van de wizard te voltooien 
7. Herhaal stap 1 t/m 6 voor virtuele machine *myVM2*.

## <a name="create-load-balancer-resources"></a>Resources voor load balancer maken

In deze sectie configureert u de instellingen van de load balancer voor een back-end-adresgroep en een statustest en geeft u een regel voor de load balancer op.


### <a name="create-a-backend-address-pool"></a>Een back-endadresgroep maken

Om verkeer te distribueren naar de VM's bevat een back-end-adresgroep de IP-adressen van de virtuele netwerkinterfacekaarten (NIC's) die zijn verbonden met de load balancer. Maak de back-endadresgroep *myBackendPool* om *VM1* en *VM2* op te nemen.

1. Klik in het linkermenu op **Alle resources** en vervolgens in de lijst met resources op **myLoadBalancer**.
2. Klik onder **Instellingen** op **Back-endpools** en vervolgens op **Toevoegen**.
3. Ga als volgt te werk op de pagina **Een back-endpool toevoegen**:
   - Typ *myBackEndPool* als de naam van uw back-endpool.
   - Als **Virtueel netwerk**, selecteert u *myVNet*.
   - Voeg *myVM1* en *myVM2*toe onder **Virtuele machine**, samen met de bijbehorende IP-adressen en selecteer vervolgens **Toevoegen**.
    - Klik op **OK**.

3. Controleer of de instelling voor de back-endpool van de load balancer beide VM's (**VM1** en **VM2**) weergeeft.

### <a name="create-a-health-probe"></a>Een statustest maken

U gebruikt een statustest om de load balancer de status van uw app te laten bewaken. De statustest voegt dynamisch VM's toe aan de load balancer-rotatie of verwijdert ze, op basis van hun reactie op statuscontroles. Maak een statustest (*myHealthProbe*) om de status van de VM's te bewaken.

1. Klik in het linkermenu op **Alle resources** en vervolgens in de lijst met resources op **myLoadBalancer**.
2. Klik onder **Instellingen** op **Tests** en klik op **Toevoegen**.
3. Gebruik deze waarden om de statustest te maken:
    - *myHealthProbe* als naam van de statustest.
    - **HTTP** als protocoltype.
    - *80* als poortnummer.
    - *Healthprobe.aspx* - voor het URI-pad. U kunt deze waarde vervangen door een willekeurige andere URI of de standaardwaarde van het pad (**\\**) behouden om de standaard-URI op te halen.
    - *15* als **interval** in seconden tussen tests.
    - *2* als aantal **drempelwaarden voor onjuiste status** of opeenvolgende mislukte tests dat moet optreden voordat wordt besloten dat een VM een onjuiste status heeft.
4. Klik op **OK**.

   ![Een test toevoegen](./media/load-balancer-standard-public-portal/4-load-balancer-probes.png)

### <a name="create-a-load-balancer-rule"></a>Een load balancer-regel maken

Een load balancer-regel wordt gebruikt om de verdeling van het verkeer over de VM's te definiëren. U definieert de front-end-IP-configuratie voor het inkomende verkeer en de back-end-IP-groep om het verkeer te ontvangen, samen met de gewenste bron- en doelpoort. Maak de regel *myLoadBalancerRuleWeb* voor de load balancer voor het luisteren naar poort 80 in de front-end *FrontendLoadBalancer* en het verzenden van netwerkverkeer met gelijke taakverdeling naar de back-endadresgroep *myBackEndPool* waarbij ook van poort 80 gebruik wordt gemaakt. 

1. Klik in het linkermenu op **Alle resources** en vervolgens in de lijst met resources op **myLoadBalancer**.
2. Klik onder **Instellingen** op **Taakverdelingsregels** en vervolgens op **Toevoegen**.
3. Gebruik deze waarden om de taakverdelingsregel te configureren:
    - *myHTTPRule* als naam van de taakverdelingsregel.
    - **TCP** als protocoltype.
    - *80* als poortnummer.
    - *80* als back-endpoort.
    - *myBackendPool* als naam van de back-endpool.
    - *myHealthProbe* als naam van de statustest.
4. Klik op **OK**.
    
    ![Een taakverdelingsregel toevoegen](./media/load-balancer-standard-public-portal/5-load-balancing-rules.png)

## <a name="test-the-load-balancer"></a>Load balancer testen
1. Zoek op het scherm **Overzicht** het openbare IP-adres voor de load balancer. Klik op **Alle resources** en vervolgens op **myPublicIP**.

2. Kopieer het openbare IP-adres en plak het in de adresbalk van de browser. De standaardpagina van IIS-webserver wordt weergegeven in de browser.

      ![IIS-webserver](./media/load-balancer-standard-public-portal/9-load-balancer-test.png)

## <a name="clean-up-resources"></a>Resources opschonen

Verwijder de resourcegroep, de load balancer en alle gerelateerde resources, wanneer u deze niet meer nodig hebt. Selecteer hiertoe de resourcegroep met de load balancer en klik op **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u een standaard Load Balancer gemaakt, VM's daaraan gekoppeld, een regel voor het Load Balancer-verkeer geconfigureerd, een statustest gemaakt en vervolgens de load balancer getest. Voor meer informatie over Azure Load Balancer gaat u verder met de zelfstudies voor Azure Load Balancer.

> [!div class="nextstepaction"]
> [Zelfstudies voor Azure Load Balancer](tutorial-load-balancer-standard-public-zone-redundant-portal.md)
