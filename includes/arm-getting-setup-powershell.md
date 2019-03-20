---
author: sdwheeler
ms.service: azure-powershell
ms.topic: include
ms.date: 11/09/2018
ms.author: sewhee
ms.openlocfilehash: c5440555c11d98fb89f8594eec1d4b7e74ea8667
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57891320"
---
## <a name="setting-up-powershell-for-resource-manager-templates"></a>PowerShell instellen voor Resource Manager-sjablonen
Voordat u Azure PowerShell met Resource Manager gebruiken kunt, moet u het recht Windows PowerShell en Azure PowerShell-versies hebben.

### <a name="verify-powershell-versions"></a>Controleer of versies van PowerShell
Controleer of dat u Windows PowerShell versie 3.0 of 4.0 hebt. Als u wilt zien welke versie van Windows PowerShell, typ de volgende opdracht achter de opdrachtprompt van Windows PowerShell.

    $PSVersionTable

U ontvangt het volgende type informatie:

    Name                           Value
    ----                           -----
    PSVersion                      3.0
    WSManStackVersion              3.0
    SerializationVersion           1.1.0.1
    CLRVersion                     4.0.30319.18444
    BuildVersion                   6.2.9200.16481
    PSCompatibleVersions           {1.0, 2.0, 3.0}
    PSRemotingProtocolVersion      2.2


Controleer de waarde van **PSVersion** 3.0 of 4.0 is. Als dit niet het geval is, Zie [Windows Management Framework 3.0](https://www.microsoft.com/download/details.aspx?id=34595) of [Windows Management Framework 4.0](https://www.microsoft.com/download/details.aspx?id=40855).

### <a name="set-your-azure-account-and-subscription"></a>Uw Azure-account en -abonnement instellen
Als u nog een Azure-abonnement hebt, kunt u uw [voordelen als MSDN-abonnee](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) zich ook aanmelden voor een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).

Een Azure PowerShell-opdrachtprompt openen en aanmelden bij Azure met deze opdracht.

    Connect-AzureRmAccount

Als u meerdere Azure-abonnementen hebt, kunt u uw Azure-abonnementen met deze opdracht weergeven.

    Get-AzureRmSubscription

U ontvangt het volgende type informatie:

    SubscriptionId            : fd22919d-eaca-4f2b-841a-e4ac6770g92e
    SubscriptionName          : Visual Studio Ultimate with MSDN
    Environment               : AzureCloud
    SupportedModes            : AzureServiceManagement,AzureResourceManager
    DefaultAccount            : johndoe@contoso.com
    Accounts                  : {johndoe@contoso.com}
    IsDefault                 : True
    IsCurrent                 : True
    CurrentStorageAccountName :
    TenantId                  : 32fa88b4-86f1-419f-93ab-2d7ce016dba7

U kunt het huidige Azure-abonnement instellen door het uitvoeren van deze opdrachten bij de Azure PowerShell-opdrachtprompt. Vervang alles binnen de aanhalingstekens in, met inbegrip van de < en > tekens, met de juiste naam.

    $subscr="<SubscriptionName from the display of Get-AzureRmSubscription>"
    Select-AzureRmSubscription -SubscriptionName $subscr -Current

Zie voor meer informatie over Azure-abonnementen en accounts [het: Verbinding maken met uw abonnement](/powershell/azureps-cmdlets-docs#step-3-connect).
