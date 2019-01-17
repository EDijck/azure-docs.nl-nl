---
title: 'Zelfstudie: Taakverdeling voor internetverkeer naar virtuele machines instellen - Azure Portal'
titlesuffix: Azure Load Balancer
description: In deze zelfstudie vindt u informatie over het maken en beheren van een Standard Load Balancer via Azure Portal.
services: load-balancer
documentationcenter: na
author: KumudD
manager: twooley
Customer intent: I want to create and Standard Load balancer so that I can load balance internet traffic to VMs and add and remove VMs from the load-balanced set.
ms.service: load-balancer
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/20/18
ms.author: kumud
ms.custom: seodec18
ms.openlocfilehash: c22f69764447ffd4f8b67e9162fd8b45b40b175b
ms.sourcegitcommit: a512360b601ce3d6f0e842a146d37890381893fc
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2019
ms.locfileid: "54230030"
---
# <a name="tutorial-load-balance-internet-traffic-to-vms-using-the-azure-portal"></a>Zelfstudie: Taakverdeling voor internetverkeer naar virtuele machines instellen met behulp van Azure Portal

Taakverdeling zorgt voor een hogere beschikbaarheid en betere schaalbaarheid door binnenkomende aanvragen te spreiden over meerdere virtuele machines. In deze zelfstudie leert u meer over de verschillende onderdelen van Azure Standard Load Balancer die internetverkeer verdelen naar VM's en zorgen voor hoge beschikbaarheid. In deze zelfstudie leert u procedures om het volgende te doen:


> [!div class="checklist"]
> * Een Azure-load balancer maken
> * Virtuele machines maken en IIS-server installeren
> * Resources voor load balancer maken
> * Een load balancer in actie zien
> * VM's toevoegen aan en verwijderen uit een load balancer

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint. 

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij de Azure Portal op [http://portal.azure.com](http://portal.azure.com).

## <a name="create-a-standard-load-balancer"></a>Een Load Balancer van het type Standard maken

In deze sectie maakt u een openbare load balancer die helpt bij het laden van virtuele machines. Standard Load Balancer biedt alleen ondersteuning voor een standaard, openbaar IP-adres. Wanneer u een Standard Load Balancer maakt, moet u ook een nieuw, standaard, openbaar IP-adres maken dat als de front-end (standaard *LoadBalancerFrontend* genoemd) wordt geconfigureerd voor de Standard Load Balancer. 

1. Klik linksboven in het scherm op **Een resource maken** > **Netwerken** > **Load balancer**.
2. Voer op de pagina **Load balancer maken** de volgende gegevens in of selecteer deze, accepteer de standaardwaarden voor de overige instellingen en selecteer **Maken**:
    
    | Instelling                 | Waarde                                              |
    | ---                     | ---                                                |
    | Naam                   | *myLoadBalancer*                                   |
    | Type          | Public                                        |
    | SKU           | Standard                          |
    | Openbaar IP-adres | Selecteer **Nieuwe maken** en typ *myPublicIP* in het tekstvak. De Standard SKU voor het openbare IP-adres is standaard geselecteerd. Als **Beschikbaarheidszone** selecteert u **Zone-redundant**. |
    | Abonnement               | Selecteer uw abonnement.    |
    |Resourcegroep | Selecteer **Nieuw** en typ *myResourceGroupSLB*.    |
    | Locatie           | Selecteer **Europa - west**.                          |
    

![Een load balancer maken](./media/load-balancer-standard-public-portal/create-load-balancer.png)
   
## <a name="create-backend-servers"></a>Back-endservers maken

In deze sectie maakt u een virtueel netwerk en twee virtuele machines voor de back-endpool van de load balancer en installeert u IIS op de virtuele machines om de load balancer te testen.

### <a name="create-a-virtual-network"></a>Een virtueel netwerk maken
1. Klik linksboven in de Azure Portal op **Een resource maken** > **Netwerken** > **Virtueel netwerk** en voer deze waarden in voor het virtuele netwerk:
    |Instelling|Waarde|
    |---|---|
    |Naam|Voer *myVNet* in.|
    |Abonnement| Selecteer uw abonnement.|
    |Resourcegroep| Selecteer **Bestaande gebruiken** en selecteer vervolgens *myResourceGroupSLB*.|
    |Subnetnaam| Voer *myBackendSubnet* in.|
    
2. Klik op **Maken** om het virtuele netwerk te maken.

### <a name="create-virtual-machines"></a>Virtuele machines maken

1. Klik linksboven in de Azure Portal op **Een resource maken** > **Compute** > **Windows Server 2016 Datacenter** en voer deze waarden in voor de virtuele machine:
    1. Voer *myVM1* als naam van de virtuele machine.        
    2. Onder **Resourcegroep** selecteert u **Bestaande gebruiken** en vervolgens *myResourceGroupSLB*.
2. Klik op **OK**.
3. Selecteer **DS1_V2** als grootte van de virtuele machine en klik op **Selecteren**.
4. Voer deze waarden in voor de instellingen van de VM:
    1. Zorg ervoor dat *myVNet* is geselecteerd voor het virtuele netwerk en dat het subnet *myBackendSubnet* is.
    2. Selecteer als **Openbaar IP-adres**, in het venster **Openbaar IP-adres maken**, **Standard**, en selecteer vervolgens **OK**.
    3. Selecteer voor **Netwerkbeveiligingsgroep** **Geavanceerd**, en doe vervolgens het volgende:
        1. Selecteer *Netwerkbeveiligingsgroep (firewall), en de pagina **Netwerkbeveiligingsgroep kiezen**, selecteer **Nieuwe maken**. 
        2. Voer op de pagina **Netwerkbeveiligingsgroep kiezen** bij **Naam** *myNetworkSecurityGroup* in als de naam van de nieuwe netwerkbeveiligingsgroep en selecteer vervolgens **OK**.
5. Klik op **Uitgeschakeld** om diagnostische gegevens over opstarten uit te schakelen.
6. Klik op **OK**, controleer de instellingen op de overzichtspagina en klik op **Maken**.
7. Maak nog twee VM's met de naam *VM2* en *VM3*, met *myVnet* als het virtuele netwerk, *myBackendSubnet* als het subnet, en *myNetworkSecurityGroup* als de netwerkbeveiligingsgroep via stap 1 t/m 6. 

### <a name="create-network-security-group-rule"></a>Regel voor netwerkbeveiligingsgroep maken

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
4. Selecteer **Toevoegen**.

### <a name="install-iis-on-vms"></a>IIS installeren op VM's

1. Klik in het linkermenu op **Alle resources** en klik in de lijst met resources op **myVM1**, die zich in de resourcegroep *myResourceGroupSLB* bevindt.
2. Klik op de pagina **Overzicht** op **Verbinding maken** om extern verbinding te maken met de VM.
3. Selecteer in het pop-upvenster **Verbinding maken met virtuele machine** **RDP-bestand downloaden**, en open vervolgens het gedownloade RDP-bestand.
4. In het venster **Verbinding met extern bureaublad** klikt u op **Verbinden**.
5. Meld u aan bij de virtuele machine met de referenties die u hebt opgegeven tijdens het maken van deze virtuele machine. Hiermee wordt een sessie met extern bureaublad met virtuele machine *myVM1* gestart.
6. Ga op de serverdesktop naar **Windows Systeembeheer**>**Windows Powershell**.
7. Voer in het venster PowerShell de volgende opdrachten uit om de IIS-server te installeren, het standaardbestand iisstart.htm te verwijderen en een nieuw bestand iisstart.htm toe te voegen dat de naam van de VM weergeeft:

   ```azurepowershell-interactive
    
    # install IIS server role
    Install-WindowsFeature -name Web-Server -IncludeManagementTools
    
    # remove default htm file
     remove-item  C:\inetpub\wwwroot\iisstart.htm
    
    # Add a new htm file that displays server name
     Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from " + $env:computername)
   ```
6. Sluit de RDP-sessie met *myVM1*.
7. Herhaal stappen 1 tot en met 6 om IIS en het bijgewerkte bestand iisstart.htm te installeren op *myVM2* en *myVM3*.

## <a name="create-load-balancer-resources"></a>Resources voor load balancer maken

In deze sectie configureert u de instellingen van de load balancer voor een back-endadresgroep en een statustest en geeft u een regel voor de load balancer op.

### <a name="create-a-backend-address-pool"></a>Een back-endadresgroep maken

Om verkeer te distribueren naar de VM's bevat een back-end-adresgroep de IP-adressen van de virtuele netwerkinterfacekaarten (NIC's) die zijn verbonden met de load balancer. Maak de back-endadresgroep *myBackendPool* om *VM1* en *VM2* op te nemen.

1. Klik in het linkermenu op **Alle resources** en vervolgens in de lijst met resources op **myLoadBalancer**.
2. Klik onder **Instellingen** op **Back-endpools** en vervolgens op **Toevoegen**.
3. Ga als volgt te werk op de pagina **Een back-endpool toevoegen**:
   - Typ *myBackEndPool* als de naam van uw back-endpool.
   - Als **Virtueel netwerk**, selecteert u *myVNet*.
   - Voeg *myVM1*, *myVM2*, en *myVM3* onder **Virtuele machine** in, samen met hun bijbehorende IP-adressen en selecteer vervolgens  **Toevoegen**.
4. Controleer of de instelling voor de back-endpool van de load balancer alle drie de VM's (*myVM1*, *myVM2* en *myVM3*) weergeeft en klik daarna op **OK**.

### <a name="create-a-health-probe"></a>Een statustest maken

U gebruikt een statustest om de load balancer de status van uw app te laten bewaken. De statustest voegt dynamisch VM's toe aan de load balancer-rotatie of verwijdert ze, op basis van hun reactie op statuscontroles. Maak een statustest (*myHealthProbe*) om de status van de VM's te bewaken.

1. Klik in het linkermenu op **Alle resources** en vervolgens in de lijst met resources op **myLoadBalancer**.
2. Klik onder **Instellingen** op **Tests** en klik op **Toevoegen**.
3. Gebruik deze waarden om de statustest te maken:
    - *myHealthProbe* als naam van de statustest.
    - **HTTP** als protocoltype.
    - *80* als poortnummer.
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

## <a name="test-the-load-balancer"></a>Load balancer testen
1. Zoek op het scherm **Overzicht** het openbare IP-adres voor de load balancer. Klik op **Alle resources** en vervolgens op **myPublicIP**.

2. Kopieer het openbare IP-adres en plak het in de adresbalk van de browser. De standaardpagina van IIS-webserver wordt weergegeven in de browser.

      ![IIS-webserver](./media/tutorial-load-balancer-standard-zonal-portal/load-balancer-test.png)

U kunt het vernieuwen van de webbrowser forceren om te zien hoe het verkeer met behulp van de load balancer wordt verdeeld over de drie VM’s waarop de app wordt uitgevoerd.

## <a name="remove-or-add-vms-from-the-backend-pool"></a>Virtuele machines toevoegen aan of verwijderen uit de back-endpool
Het is mogelijk dat u onderhoud moet uitvoeren op de VM's waarop uw app wordt uitgevoerd, zoals het installeren van besturingssysteemupdates. U moet mogelijk extra VM's toevoegen vanwege toegenomen verkeer naar uw app. In dit gedeelte wordt beschreven hoe u een VM aan de load balancer toevoegt of ervan verwijdert.

1. Klik in het linkermenu op **Alle resources** en vervolgens in de lijst met resources op **myLoadBalancer**.
2. Klik onder **Instellingen** op **Back-endpools**. Klik vervolgens in de lijst van de back-endpool op **myBackendPool**.
3. Als u op de pagina **myBackendPool** onder **IP-configuraties doelnetwerk** *VM1* wilt verwijderen uit de back-end, klikt u op het pictogram Verwijderen naast **Virtuele machine:myVM1**

Nu *myVM1* niet meer in de back-endadresgroep zit, kunt u onderhoudstaken uitvoeren op *myVM1*, zoals het installeren van software-updates. Omdat *VM1** niet meer beschikbaar is, wordt de belasting verdeeld over *myVM2* en *myVM3*. 

Als u *myVM1* weer wilt toevoegen aan de back-endpool, volgt u de procedure in de sectie *VM's toevoegen aan de back-endpool* van dit artikel.

## <a name="clean-up-resources"></a>Resources opschonen

Verwijder de resourcegroep, de load balancer en alle gerelateerde resources, wanneer u deze niet meer nodig hebt. Selecteer hiertoe de resourcegroep met de load balancer en klik op **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een Standard Load Balancer gemaakt, hieraan VM's gekoppeld, een regel geconfigureerd voor verkeer van deze load balancer, een statustest gemaakt en de load balancer vervolgens getest. U hebt ook een VM verwijderd uit de set load balancers en deze weer toegevoegd aan de back-endadresgroep. Voor meer informatie over Azure Load Balancer gaat u verder met de zelfstudies voor Azure Load Balancer.

> [!div class="nextstepaction"]
> [Zelfstudies voor Azure Load Balancer](tutorial-load-balancer-standard-public-zone-redundant-portal.md)
