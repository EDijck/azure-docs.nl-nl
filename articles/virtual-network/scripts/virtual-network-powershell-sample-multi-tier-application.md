---
title: Azure PowerShell-voorbeeldscript - Het maken van een netwerk voor toepassingen met meerdere lagen | Microsoft Docs
description: Azure PowerShell-voorbeeldscript - Het maken van een netwerk voor toepassingen met meerdere lagen.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: twooley
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: powershell
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 12/13/2018
ms.author: kumud
ms.openlocfilehash: 2fad78db4fdc92f3dc9c0f320c36d12dea554a61
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2019
ms.locfileid: "64725385"
---
# <a name="create-a-network-for-multi-tier-applications-script-sample"></a>Voorbeeldscript voor het maken van een netwerk voor toepassingen met meerdere lagen

Dit voorbeeldscript maakt een virtueel netwerk met front-end en back-end-subnetten. Verkeer naar het front-end-subnet wordt beperkt tot HTTP en SSH, terwijl het verkeer naar het back-end-subnet wordt beperkt tot MySQL, poort 3306. Nadat het script is uitgevoerd, hebt u twee virtuele machines, één in elk subnet, waarop u webserver- en MySQL-software kunt implementeren.

U kunt het script uitvoeren vanuit de Azure [Cloud Shell](https://shell.azure.com/powershell) of vanuit een lokale installatie van PowerShell. Als u Microsoft Azure PowerShell lokaal gebruikt, vereist dit script de Azure PowerShell-module versie 1.0.0 of hoger. Voer `Get-Module -ListAvailable Az` uit om na te gaan welke versie er is geïnstalleerd. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-az-ps). Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzAccount` uitvoeren om verbinding te kunnen maken met Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeldscript

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

<!-- gitHub issue https://github.com/MicrosoftDocs/azure-docs/issues/17748 -->
Een subnet-ID is toegewezen, nadat u hebt gemaakt een virtueel netwerk. met name met behulp van de cmdlet New-AzVirtualNetwork met de - Subnet-optie. Als u het subnet configureert met de cmdlet New-AzVirtualNetworkSubnetConfig vóór de aanroep naar New-AzVirtualNetwork, ziet u de subnet-id pas nadat New-AzVirtualNetwork is aangeroepen.

[!code-azurepowershell-interactive[main](../../../powershell_scripts/virtual-network/virtual-network-multi-tier-application/virtual-network-multi-tier-application.ps1  "Virtual network for multi-tier application")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie

Voer de volgende opdracht uit om de resourcegroep, VM en alle gerelateerde resources te verwijderen:

```powershell
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt voor het maken van een resourcegroep, een virtueel netwerk en netwerkbeveiligingsgroepen. Elke opdracht in de volgende tabel is een koppeling naar opdrachtspecifieke documentatie:

| Opdracht | Opmerkingen |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Hiermee wordt een resourcegroep gemaakt waarin alle resources worden opgeslagen. |
| [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) | Hiermee maakt u een virtueel Azure-netwerk en front-end-subnet. |
| [New-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig) | Hiermee maakt u een back-end-subnet. |
| [New-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress) | Hiermee maakt u een openbaar IP-adres voor toegang tot de VM via internet. |
| [New-AzNetworkInterface](/powershell/module/az.network/new-aznetworkinterface) | Hiermee maakt u virtuele netwerkinterfaces en koppelt u ze aan de front-end- en back-end-subnetten van het virtuele netwerk. |
| [New-AzNetworkSecurityGroup](/powershell/module/az.network/new-aznetworksecuritygroup) | Hiermee maakt u netwerkbeveiligingsgroepen (NSG's) die zijn gekoppeld aan de front-end- en back-end-subnetten. |
| [New-AzNetworkSecurityRuleConfig](/powershell/module/az.network/new-aznetworksecurityruleconfig) |Hiermee maakt u NSG-regels die bepaalde poorten tot specifieke subnetten blokkeren of toestaan. |
| [New-AzVM](/powershell/module/az.compute/new-azvm) | Hiermee maakt u virtuele machines en koppelt u een NIC aan elke virtuele machine. Met deze opdracht geeft u ook de installatiekopie van de virtuele machine op die moet worden gebruikt, evenals de beheerdersreferenties. |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Hiermee verwijdert u een resourcegroep en de bijhorende resources. |

## <a name="next-steps"></a>Volgende stappen

Zie [Documentatie over Azure PowerShell](/powershell/azure/overview) voor meer informatie over Azure PowerShell.

Meer PowerShell-voorbeeldscripts voor virtuele netwerken vindt u in [PowerShell-voorbeelden voor virtuele netwerken](../powershell-samples.md).
