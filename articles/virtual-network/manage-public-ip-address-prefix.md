---
title: Maken, wijzigen of verwijderen van een Azure openbare IP-adresvoorvoegsel
titlesuffix: Azure Virtual Network
description: Informatie over het maken, wijzigen of verwijderen van een openbare IP-adresvoorvoegsel.
services: virtual-network
documentationcenter: na
author: anavinahar
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/24/2018
ms.author: anavin
ms.openlocfilehash: f4da0f992914037f5c95050324af5762e90a2ca4
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/04/2019
ms.locfileid: "55696819"
---
# <a name="create-change-or-delete-a-public-ip-address-prefix"></a>Maken, wijzigen of verwijderen van een openbare IP-adresvoorvoegsel

Meer informatie over een openbaar IP-adresvoorvoegsel en het maken, wijzigen en verwijderen. Een openbare IP-adresvoorvoegsel is een aaneengesloten reeks adressen op basis van het aantal openbare IP-adres dat u opgeeft. De adressen worden toegewezen aan uw abonnement. Wanneer u een openbare IP-adresresource maakt, kunt u een statisch openbaar IP-adres toewijzen uit het voorvoegsel en koppelen aan virtuele machines, load balancers of andere bronnen waarmee verbinding met internet. Als u niet bekend met openbare IP-adresvoorvoegsels bent, Zie [openbare IP-adres voorvoegsel overzicht](public-ip-address-prefix.md)

## <a name="before-you-begin"></a>Voordat u begint

> [!IMPORTANT]
> Openbare IP-voorvoegsel is in een openbare preview in beperkte regio's. U kunt [informatie over wat het betekent Preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Openbaar IP-voorvoegsel is momenteel beschikbaar in: West-Centraal VS, VS-West, VS-West 2, VS-midden, Noord-Europa, West-Europa en Zuidoost-Azië. Ga voor een bijgewerkte lijst met regio's, [Azure-updates](https://azure.microsoft.com/updates/?product=virtual-network).

Voer de volgende taken voordat u de stappen in elke sectie van dit artikel:

- Als u nog een Azure-account hebt, kunt u zich aanmelden voor een [gratis proefaccount](https://azure.microsoft.com/free).
- Als u de portal gebruikt, opent u https://aka.ms/publicipprefixportal, en meld u aan met uw Azure-account.
- Als u PowerShell-opdrachten gebruikt om taken in dit artikel te voltooien, hetzij de opdrachten uitvoert in de [Azure Cloud Shell](https://shell.azure.com/powershell), of door te voeren PowerShell vanaf uw computer. Azure Cloud Shell is een gratis interactieve shell waarmee u de stappen in dit artikel kunt uitvoeren. In deze shell zijn algemene Azure-hulpprogramma's vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. Deze zelfstudie moduleversie 6.3.1 PowerShell azurerm.Network moet dit vereist of hoger. Voer `Get-Module -ListAvailable AzureRM.Network` uit om te kijken welke versie is geïnstalleerd. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](https://github.com/Azure/azure-powershell/releases/tag/AzureRm.Network.6.3.1). Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.
- Als u Azure-opdrachtregelinterface (CLI)-opdrachten voor taken in dit artikel uit te voeren, hetzij de opdrachten uitvoert in de [Azure Cloud Shell](https://shell.azure.com/bash), of door het uitvoeren van de CLI van de computer. In deze zelfstudie gebruikmaken van Azure CLI versie 2.0.41 of hoger. Voer `az --version` uit om te kijken welke versie is geïnstalleerd. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli). Als u de Azure CLI lokaal uitvoert, moet u ook om uit te voeren `az login` voor het maken van een verbinding met Azure.

Het account dat u zich aanmelden bij of verbinding maken met Azure, moet worden toegewezen aan de [Inzender voor netwerken](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rol of een [aangepaste rol](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) die is toegewezen de nodige acties die worden vermeld in [machtigingen ](#permissions).

Openbare IP-adresvoorvoegsels hebben een kosten in rekening gebracht. Zie voor meer informatie, [prijzen](https://azure.microsoft.com/pricing/details/ip-addresses).

## <a name="create-a-public-ip-address-prefix"></a>Maken van een openbare IP-adresvoorvoegsel

1. Selecteer in de linkerbovenhoek hoek van de portal, **+ een resource maken**.
2. Voer *openbare ip-adresvoorvoegsel* in de *Marketplace doorzoeken* vak. Wanneer **openbare IP-adresvoorvoegsel** wordt weergegeven in de zoekresultaten, selecteert u dit.
3. Onder **openbare IP-adresvoorvoegsel**, selecteer **maken**.
4. Typ of Selecteer waarden voor de volgende instellingen onder **maken openbare IP-adresvoorvoegsel**en selecteer vervolgens **maken**:

   |Instelling|Vereist?|Details|
   |---|---|---|
   |Abonnement|Ja|Moet bestaan in dezelfde [abonnement](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) als de resource die u wilt koppelen aan het openbare IP-adres.|
   |Resourcegroep|Ja|Kan bestaan in de dezelfde of verschillende, [resourcegroep](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group) als de resource die u wilt koppelen aan het openbare IP-adres.|
   |Name|Ja|De naam moet uniek zijn binnen de resourcegroep die u selecteert.|
   |Regio|Ja|Moet bestaan in dezelfde [regio](https://azure.microsoft.com/regions)als het openbare IP-adressen u adressen uit het bereik moet toewijzen. Voorvoegsel is momenteel is de Preview-versie in West-Centraal VS, VS-West, VS-West 2, VS-midden, Noord-Europa, West-Europa en Zuidoost-Azië.|
   |Voorvoegselgrootte|Ja| De grootte van het voorvoegsel dat u nodig hebt. Een/28 of 16 IP-adressen is de standaardinstelling. 

**Opdrachten**


|Hulpprogramma|Opdracht|
|---|---|
|CLI|[AZ network public-IP-voorvoegsel maken](/cli/azure/network/public-ip/prefix#az-network-public-ip-prefix-create)|
|PowerShell|[New-AzureRmPublicIpPrefix](/powershell/module/azurerm.network/new-azurermpublicipprefix)|

## <a name="create-a-static-public-ip-address-from-a-prefix"></a>Maken van een statisch openbaar IP-adres uit een voorvoegsel
Als u een voorvoegsel maakt, moet u statische IP-adressen uit het voorvoegsel. Volg hiervoor de onderstaande stappen.

1. In het vak met de tekst *zoeken naar resources* aan de bovenkant van de Azure-portal, typt u *openbare ip-adresvoorvoegsel*. Wanneer **openbare IP-adresvoorvoegsels** worden weergegeven in de lijst met zoekresultaten, selecteert u deze.
2. Selecteer het voorvoegsel dat u wilt maken van openbare IP-adressen.
3. Wanneer deze wordt weergegeven in de lijst met zoekresultaten, selecteert u deze en klikt u op **+ IP-adres toevoegen** in de sectie overzicht. Als u dit niet ziet, Controleer of u de juiste koppeling voor de Preview-versie: https://aka.ms/publicipprefixportal
4. Typ of Selecteer waarden voor de volgende instellingen onder **openbare IP-adres maken**. Aangezien een voorvoegsel voor standaard-SKU, IPv4- en statische is, moet u alleen de volgende informatie:

   |Instelling|Vereist?|Details|
    |---|---|---|
    |Name|Ja|De naam van het openbare IP-adres moet uniek zijn binnen de resourcegroep die u selecteert.|
   |Time-out voor inactiviteit (minuten)|Nee|Het aantal minuten dat een TCP- of HTTP-verbinding open zonder afhankelijkheid van clients voor het verzenden van keepalive-berichten. |
   |DNS-naamlabel|Nee|Moet uniek zijn binnen Azure-regio die voor het maken van de naam van de in (voor alle abonnementen en alle klanten). Azure wordt automatisch geregistreerd de naam en IP-adres in de DNS, zodat u verbinding met een resource met de naam maken kunt. Azure voegt een standaard-subnet, zoals *location.cloudapp.azure.com* (indien de locatie de locatie die u selecteert is) op de naam u opgeeft, voor het maken van de volledig gekwalificeerde DNS-naam. Zie voor meer informatie, [Azure DNS gebruiken met een openbaar IP-adres van Azure](../dns/dns-custom-domain.md?toc=%2fazure%2fvirtual-network%2ftoc.json#public-ip-address).|

## <a name="view-or-delete-a-prefix"></a>Weergeven of verwijderen van een voorvoegsel

1. In het vak met de tekst *zoeken naar resources* aan de bovenkant van de Azure-portal, typt u *openbare ip-adresvoorvoegsel*. Wanneer **openbare IP-adresvoorvoegsels** worden weergegeven in de lijst met zoekresultaten, selecteert u deze.
2. Selecteer de naam van het openbare IP-adresvoorvoegsel dat u wilt weergeven, instellingen voor wijzigen of verwijderen uit de lijst.
3. Voer een van de volgende opties, afhankelijk van of u wilt weergeven, verwijderen of wijzigen van het openbare IP-adresvoorvoegsel.
    - **Weergave**: De **overzicht** sectie bevat belangrijke instellingen voor het openbare IP-adresvoorvoegsel, zoals voorvoegsel.
    - **Verwijder**: Selecteer wilt verwijderen van het openbare IP-adresvoorvoegsel **verwijderen** in de **overzicht** sectie. Als de adressen binnen het voorvoegsel zijn gekoppeld aan resources voor openbaar IP-adres, moet u eerst het openbare IP-adres-resources te verwijderen. Zie [verwijderen van een openbaar IP-adres](virtual-network-public-ip-address.md#view-change-settings-for-or-delete-a-public-ip-address).

**Opdrachten**

|Hulpprogramma|Opdracht|
|---|---|
|CLI|[lijst met AZ network public-IP-voorvoegsel](/cli/azure/network/public-ip/prefix#az-network-public-ip-prefix-list) aan de lijst met openbare IP-adressen, [az network public-IP-voorvoegsel show](/cli/azure/network/public-ip/prefix#az-network-public-ip-prefix-show) om weer te geven van instellingen. [az network public-IP-voorvoegsel update](/cli/azure/network/public-ip/prefix#az-network-public-ip-prefix-update) om bij te werken; [az network public-IP-voorvoegsel verwijderen](/cli/azure/network/public-ip/prefix#az-network-public-ip-prefix-delete) verwijderen|
|PowerShell|[Get-AzureRmPublicIpPrefix](/powershell/module/azurerm.network/get-azurermpublicipprefix) een openbaar IP-adresobject ophalen en weergeven van de instellingen [Set AzureRmPublicIpPrefix](/powershell/module/azurerm.network/set-azurermpublicipprefix) instellingen; bijwerken [Remove-AzureRmPublicIpPrefix](/powershell/module/azurerm.network/remove-azurermpublicipprefix) verwijderen|

## <a name="permissions"></a>Machtigingen

Om uit te voeren taken op openbare IP-adresvoorvoegsels, moet uw account worden toegewezen aan de [Inzender voor netwerken](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rol of een [aangepaste](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) rol die is toegewezen de nodige acties die worden vermeld in de volgende tabel:

| Bewerking                                                                   | Name                                                           |
| ---------                                                                | -------------                                                  |
| Microsoft.Network/publicIPPrefixes/read                           | Een openbare IP-adresvoorvoegsel lezen                                |
| Microsoft.Network/publicIPPrefixes/write                          | Maken of bijwerken van een openbare IP-adresvoorvoegsel                    |
| Microsoft.Network/publicIPPrefixes/delete                         | Een openbare IP-adresvoorvoegsel verwijderen                              |
|Microsoft.Network/publicIPPrefixes/join/action | Maken van een openbaar IP-adres uit een voorvoegsel |

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over scenario's en voordelen van het gebruik van een [openbaar IP-voorvoegsel](public-ip-address-prefix.md)
