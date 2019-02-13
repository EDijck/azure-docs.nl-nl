---
title: Azure S2S VPN-verbindingen maken en beheren met behulp van PowerShell | Microsoft Docs
description: 'Zelfstudie: Azure S2S VPN-verbindingen maken en beheren met behulp van de Azure PowerShell-module'
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure
ms.date: 05/08/2018
ms.author: yushwang
ms.custom: mvc
ms.openlocfilehash: 0c71062bded65f8aa7c259c0678ee6675e2dab38
ms.sourcegitcommit: fea5a47f2fee25f35612ddd583e955c3e8430a95
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55509470"
---
# <a name="create-and-manage-s2s-vpn-connections-with-the-azure-powershell-module"></a>Azure S2S VPN-verbindingen maken en beheren met behulp van de Azure PowerShell-module

Azure S2S VPN-verbindingen bieden veilige, cross-premises connectiviteit tussen de klanten-premises en Azure. Deze zelfstudie neemt levenscycli van IPsec S2S VPN-verbindingen door zoals het maken en beheren van een S2S VPN-verbinding. In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Maken van een S2S VPN-verbinding
> * Bijwerken van de verbindingseigenschap: vooraf gedeelde sleutel, BGP, IPsec-/IKE-beleid
> * Toevoegen van meer VPN-verbindingen
> * Verwijderen van een VPN-verbinding

Het volgende diagram toont de topologie voor deze zelfstudie:

![Site-naar-site-VPN-verbindingsdiagram](./media/vpn-gateway-tutorial-vpnconnection-powershell/site-to-site-diagram.png)

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Als u PowerShell lokaal wilt installeren en gebruiken, is voor deze zelfstudie moduleversie 5.3 of later van Azure PowerShell vereist. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/azurerm/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Login-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.

## <a name="requirements"></a>Vereisten

Voltooi de eerste zelfstudie: '[Een Azure VPN-gateway maken met behulp van Microsoft Azure PowerShell](vpn-gateway-tutorial-create-gateway-powershell.md)' om de volgende resources te maken:

1. Resourcegroep (TestRG1), het virtuele netwerk (VNet1) en GatewaySubnet
2. VPN-gateway (VNet1GW)

De parameterwaarden van het virtueel netwerk worden hieronder vermeld. Let op de aanvullende waarden voor de lokale netwerkgateway om uw on-premises netwerk weer te geven. Wijzig de waarden op basis van uw omgeving en netwerk-instellingen.

```azurepowershell-interactive
# Virtual network
$RG1         = "TestRG1"
$VNet1       = "VNet1"
$Location1   = "East US"
$VNet1Prefix = "10.1.0.0/16"
$VNet1ASN    = 65010
$Gw1         = "VNet1GW"

# On-premises network
$LNG1        = "VPNsite1"
$LNGprefix1  = "10.101.0.0/24"
$LNGprefix2  = "10.101.1.0/24"
$LNGIP1      = "YourDevicePublicIP"

# Optional - on-premises BGP properties
$LNGASN1     = 65011
$BGPPeerIP1  = "10.101.1.254"

# Connection
$Connection1 = "VNet1ToSite1"
```

De werkstroom voor het maken van een S2S VPN-verbinding is eenvoudig:

1. Maak een lokale netwerkgateway ter vertegenwoordiging van uw on-premises netwerk
2. Maak een verbinding tussen uw Azure VPN-gateway en de lokale netwerkgateway

## <a name="create-a-local-network-gateway"></a>Een lokale netwerkgateway maken

Een lokale netwerkgateway vertegenwoordigt uw on-premises netwerk. U kunt de eigenschappen van uw on-premises netwerk opgeven in de lokale netwerkgateway, met inbegrip van:

* Openbare IP-adres van uw VPN-apparaat
* On-premises adresruimte
* (Optioneel) BGP-kenmerken (BGP-peer-IP-adres en AS-nummer)

Maak een lokale netwerkgateway met de opdracht [Nieuwe AzureRmLocalNetworkGateway](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermlocalnetworkgateway?view=azurermps-6.8.1).

```azurepowershell-interactive
New-AzureRmLocalNetworkGateway -Name $LNG1 -ResourceGroupName $RG1 `
  -Location 'East US' -GatewayIpAddress $LNGIP1 -AddressPrefix $LNGprefix1,$LNGprefix2
```

## <a name="create-a-s2s-vpn-connection"></a>Een S2S VPN-verbinding maken

Maak vervolgens een Site-naar-Site VPN-verbinding tussen uw virtuele netwerkgateway en uw VPN-apparaat met de [New-AzureRmVirtualNetworkGatewayConnection](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermvirtualnetworkgatewayconnection?view=azurermps-6.8.1). U ziet dat het -ConnectionType voor Site-naar-Site VPN *IPsec* is.

```azurepowershell-interactive
$vng1 = Get-AzureRmVirtualNetworkGateway -Name $GW1  -ResourceGroupName $RG1
$lng1 = Get-AzureRmLocalNetworkGateway   -Name $LNG1 -ResourceGroupName $RG1

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection1 -ResourceGroupName $RG1 `
  -Location $Location1 -VirtualNetworkGateway1 $vng1 -LocalNetworkGateway2 $lng1 `
  -ConnectionType IPsec -SharedKey "Azure@!b2C3"
```

Voeg de optionele eigenschap **- EnableBGP $True** toe om BGP in te schakelen voor de verbinding als u van BGP gebruikmaakt. Deze optie is standaard uitgeschakeld.

## <a name="update-the-vpn-connection-pre-shared-key-bgp-and-ipsecike-policy"></a>Werk de vooraf gedeelde sleutel voor VPN-verbinding, BGP en het IPsec-/IKE-beleid bij

### <a name="view-and-update-your-pre-shared-key"></a>Weergeven en bijwerken van de vooraf gedeelde sleutel

Azure S2S VPN-verbinding gebruikt een vooraf gedeelde sleutel (geheim) om te verifiëren tussen uw on-premises VPN-apparaat en de Azure VPN-gateway. U kunt de vooraf gedeelde sleutel weergeven en bijwerken voor een verbinding met [Get-AzureRmVirtualNetworkGatewayConnectionSharedKey](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermvirtualnetworkgatewayconnectionsharedkey?view=azurermps-6.8.1) en [Set AzureRmVirtualNetworkGatewayConnectionSharedKey](https://docs.microsoft.com/powershell/module/azurerm.network/set-azurermvirtualnetworkgatewayconnectionsharedkey?view=azurermps-6.8.1).

> [!IMPORTANT]
> De vooraf gedeelde sleutel is een tekenreeks met **afdrukbare ASCII-tekens** van niet langer dan 128.

Deze opdracht geeft u de vooraf gedeelde sleutel voor de verbinding:

```azurepowershell-interactive
Get-AzureRmVirtualNetworkGatewayConnectionSharedKey `
  -Name $Connection1 -ResourceGroupName $RG1
```

De uitvoer zal **Azure@!b2C3** zijn na het bovenstaande voorbeeld. Gebruik de onderstaande opdracht om de waarde van de vooraf gedeelde sleutel te wijzigen naar **Azure@!_b2=C3**:

```azurepowershell-interactive
Set-AzureRmVirtualNetworkGatewayConnectionSharedKey `
  -Name $Connection1 -ResourceGroupName $RG1 `
  -Value "Azure@!_b2=C3"
```

### <a name="enable-bgp-on-vpn-connection"></a>BGP inschakelen op de VPN-verbinding

Azure VPN-gateway biedt ondersteuning voor een dynamisch BGP-routeringsprotocol. U kunt BGP inschakelen voor elke afzonderlijke verbinding, afhankelijk van of u BGP in uw on-premises netwerken en apparaten gebruikt. Geef de volgende BGP-eigenschappen op voordat u BGP inschakelt op de verbinding:

* Azure VPN-ASN (Autonomous System Number)
* On-premises lokale netwerkgateway ASN
* On-premises lokale netwerkgateway BGP-peer-IP-adres

Als u de BGP-eigenschappen niet hebt geconfigureerd, gebruikt u de volgende opdrachten om deze eigenschappen toe te voegen aan uw VPN-gateway en de lokale netwerkgateway: [Set-AzureRmVirtualNetworkGateway](https://docs.microsoft.com/powershell/module/azurerm.network/set-azurermvirtualnetworkgateway?view=azurermps-6.8.1) en [Set-AzureRmLocalNetworkGateway](https://docs.microsoft.com/powershell/module/azurerm.network/set-azurermlocalnetworkgateway?view=azurermps-6.8.1).

```azurepowershell-interactive
$vng1 = Get-AzureRmVirtualNetworkGateway -Name $GW1  -ResourceGroupName $RG1
Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $vng1 -Asn $VNet1ASN

$lng1 = Get-AzureRmLocalNetworkGateway   -Name $LNG1 -ResourceGroupName $RG1
Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $lng1 `
  -Asn $LNGASN1 -BgpPeeringAddress $BGPPeerIP1
```

Schakel BGP in met [Set-AzureRmVirtualNetworkGatewayConnection](https://docs.microsoft.com/powershell/module/azurerm.network/set-azurermvirtualnetworkgatewayconnection?view=azurermps-6.8.1).

```azurepowershell-interactive
$connection = Get-AzureRmVirtualNetworkGatewayConnection `
  -Name $Connection1 -ResourceGroupName $RG1

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection `
  -EnableBGP $True
```

U kunt BGP uitschakelen door de eigenschapswaarde -EnableBGP te wijzigen in **$False**. Raadpleeg [BGP op Azure VPN-gateways](vpn-gateway-bgp-resource-manager-ps.md) voor meer gedetailleerde uitleg over BGP op Azure VPN-gateways.

### <a name="apply-a-custom-ipsecike-policy-on-the-connection"></a>Een aangepast IPsec-/IKE-beleid toepassen op de verbinding

U kunt een optioneel IPsec-/IKE-beleid toepassen om de exacte combinatie van cryptografische IPsec-/IKE-algoritmen en de kracht van de sleutels op de verbinding op te geven, in plaats van de [standaardvoorstellen](vpn-gateway-about-vpn-devices.md#ipsec) te gebruiken. Het volgende voorbeeldscript maakt een ander beleid voor IPsec/IKE met de volgende algoritmen en parameters:

* IKEv2: AES256, SHA256, DHGroup14
* IPsec: AES128, SHA1, PFS14, SA Lifetime 14.400 seconden en 102.400.000 KB

```azurepowershell-interactive
$connection = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection1 `
                -ResourceGroupName $RG1
$newpolicy  = New-AzureRmIpsecPolicy `
                -IkeEncryption AES256 -IkeIntegrity SHA256 -DhGroup DHGroup14 `
                -IpsecEncryption AES128 -IpsecIntegrity SHA1 -PfsGroup PFS2048 `
                -SALifeTimeSeconds 14400 -SADataSizeKilobytes 102400000

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection `
  -IpsecPolicies $newpolicy
```

Raadpleeg [IPsec/IKE policy for S2S or VNet-to-VNet connections](vpn-gateway-ipsecikepolicy-rm-powershell.md) (IPsec-/IKE-beleid voor S2S- of VNet-naar-VNet-verbindingen) voor een volledige lijst van algoritmen en instructies.

## <a name="add-another-s2s-vpn-connection"></a>Voeg nog een S2S VPN-verbinding toe

Maak om een extra S2S VPN-verbinding toe te voegen aan dezelfde VPN-gateway een andere lokale netwerkgateway en een nieuwe verbinding tussen de lokale netwerkgateway en de VPN-gateway. Volg het voorbeeld in dit artikel.

```azurepowershell-interactive
# On-premises network
$LNG2        = "VPNsite2"
$Location2   = "West US"
$LNGprefix21 = "10.102.0.0/24"
$LNGprefix22 = "10.102.1.0/24"
$LNGIP2      = "YourDevicePublicIP"
$Connection2 = "VNet1ToSite2"

New-AzureRmLocalNetworkGateway -Name $LNG2 -ResourceGroupName $RG1 `
  -Location $Location2 -GatewayIpAddress $LNGIP2 -AddressPrefix $LNGprefix21,$LNGprefix22

$vng1 = Get-AzureRmVirtualNetworkGateway -Name $GW1  -ResourceGroupName $RG1
$lng2 = Get-AzureRmLocalNetworkGateway   -Name $LNG2 -ResourceGroupName $RG1

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection2 -ResourceGroupName $RG1 `
  -Location $Location1 -VirtualNetworkGateway1 $vng1 -LocalNetworkGateway2 $lng2 `
  -ConnectionType IPsec -SharedKey "AzureA1%b2_C3+"
```

Er zijn nu twee S2S VPN-verbindingen naar uw Azure VPN-gateway.

![MultiSite VPN-verbindingen](./media/vpn-gateway-tutorial-vpnconnection-powershell/multisite-connections.png)

## <a name="delete-a-s2s-vpn-connection"></a>Een S2S VPN-verbinding verwijderen

Verwijder een S2S VPN-verbinding met [Remove-AzureRmVirtualNetworkGatewayConnection](https://docs.microsoft.com/powershell/module/azurerm.network/remove-azurermvirtualnetworkgatewayconnection?view=azurermps-6.8.1).

```azurepowershell-interactive
Remove-AzureRmVirtualNetworkGatewayConnection -Name $Connection2 -ResourceGroupName $RG1
```

Verwijder de lokale netwerkgateway als u deze niet langer nodig hebt. U kunt een lokale netwerkgateway niet verwijderen als er andere verbindingen aan zijn gekoppeld.

```azurepowershell-interactive
Remove-AzureRmVirtualNetworkGatewayConnection -Name $LNG2 -ResourceGroupName $RG1
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u meer geleerd over het maken en beheren van S2S VPN-verbindingen, zoals het:

> [!div class="checklist"]
> * Maken van een S2S VPN-verbinding
> * Bijwerken van de verbindingseigenschap: vooraf gedeelde sleutel, BGP, IPsec-/IKE-beleid
> * Toevoegen van meer VPN-verbindingen
> * Verwijderen van een VPN-verbinding

Ga naar de volgende zelfstudies voor meer informatie over S2S-, VNet-naar-VNet- en P2S-verbindingen.

> [!div class="nextstepaction"]
> * [VNet-naar-VNet-verbindingen maken](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [P2S-verbindingen maken](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
