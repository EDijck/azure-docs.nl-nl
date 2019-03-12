---
title: Naam en sleutel van verlener in BizTalk Services | Microsoft Docs
description: Informatie over het ophalen van naam en sleutel van verlener voor Service Bus of Access Control (ACS) in BizTalk Services. MABS, WABS
services: biztalk-services
documentationcenter: ''
author: MandiOhlinger
manager: anneta
editor: ''
ms.assetid: 067fe356-d1aa-420f-b2f2-1a418686470a
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: 5eac98ec88b960956c9a0931673e67f530aef8da
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57542179"
---
# <a name="biztalk-services-issuer-name-and-issuer-key"></a>BizTalk Services: Naam en sleutel van verlener

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Azure BizTalk Services maakt gebruik van de naam van de Service Bus-verlener en sleutel van verlener, en de naam van Access Control verlener en sleutel van verlener. Specifiek:

| Taak | Welke naam en sleutel van verlener |
| --- | --- |
| Implementeren van uw toepassing vanuit Visual Studio |De naam van een Access Control-verlener en sleutel van verlener |
| De Azure BizTalk Services-Portal configureren |De naam van een Access Control-verlener en sleutel van verlener |
| LOB-Relays maken met de BizTalk Adapter Services in Visual Studio |Naam van Service Bus-verlener en sleutel van verlener |

In dit onderwerp worden de stappen voor het ophalen van de naam en sleutel van verlener. 

## <a name="access-control-issuer-name-and-issuer-key"></a>De naam van een Access Control-verlener en sleutel van verlener
De naam van Access Control verlener en sleutel van verlener worden gebruikt door het volgende:

* Uw Azure BizTalk Service-toepassing in Visual Studio hebt gemaakt: Als u wilt uw BizTalk Service-toepassing in Visual Studio implementeren naar Azure, voert u de naam van de verlener Access Control en sleutel van verlener. 
* De Azure BizTalk Services-Portal: Wanneer u een BizTalk Service maken en de BizTalk Services-Portal opent, worden de naam van Access Control verlener en sleutel van verlener automatisch geregistreerd voor uw implementaties met dezelfde Access Control-waarden.

### <a name="get-the-access-control-issuer-name-and-issuer-key"></a>Ophalen van de naam van Access Control verlener en sleutel van verlener

Als u ACS gebruikt voor verificatie en de naam en sleutel van verlener waarden ophaalt, de algemene stappen zijn onder andere:

1. Installeer de [Azure Powershell-cmdlets](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).
2. Uw Azure-account toevoegen: `Add-AzureAccount`
3. Retourneert de abonnementsnaam van uw: `get-azuresubscription`
4. Selecteer uw abonnement: `select-azuresubscription <name of your subscription>` 
5. Maak een nieuwe naamruimte: `new-azuresbnamespace <name for the service bus> "Location" -CreateACSNamespace $true -NamespaceType Messaging`

    Voorbeeld:    `new-azuresbnamespace biztalksbnamespace "South Central US" -CreateACSNamespace $true -NamespaceType Messaging`
      
5. Wanneer de nieuwe ACS-naamruimte wordt gemaakt (dit kan enkele minuten duren), wordt de naam en sleutel van verlener waarden worden weergegeven in de verbindingsreeks: 

    ```
    Name                  : biztalksbnamespace
    Region                : South Central US
    DefaultKey            : abcdefghijklmnopqrstuvwxyz
    Status                : Active
    CreatedAt             : 10/18/2016 9:36:30 PM
    AcsManagementEndpoint : https://biztalksbnamespace-sb.accesscontrol.windows.net/
    ServiceBusEndpoint    : https://biztalksbnamespace.servicebus.windows.net/
    ConnectionString      : Endpoint=sb://biztalksbnamespace.servicebus.windows.net/;SharedSecretIssuer=owner;SharedSecretValue=abcdefghijklmnopqrstuvwxyz
    NamespaceType         : Messaging
    ```

Samenvatting:  
Naam van verlener = SharedSecretIssuer  
Sleutel van verlener = SharedSecretKey

Meer op de [New-AzureSBNamespace](https://docs.microsoft.com/powershell/module/servicemanagement/azure/new-azuresbnamespace) cmdlet. 

## <a name="service-bus-issuer-name-and-issuer-key"></a>Naam van Service Bus-verlener en sleutel van verlener
Naam van Service Bus-verlener en sleutel van verlener worden gebruikt door de BizTalk Adapter Services. In uw BizTalk Services-project in Visual Studio gebruikt u de BizTalk Adapter Services verbinding maken met een on-premises Line-of-Business (LOB)-systeem. Als u wilt verbinden, maken de Bedrijfstakomleiding en voer de details van uw LOB-systeem. Wanneer u dit doet, voert u ook de naam van Service Bus-verlener en sleutel van verlener.

### <a name="to-retrieve-the-service-bus-issuer-name-and-issuer-key"></a>Om op te halen van de naam van Service Bus-verlener en sleutel van verlener
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Zoeken naar **Service Bus**, en selecteer de naamruimte. 
3. Open de **beleid voor gedeelde toegang** eigenschappen, selecteert u uw beleid en bekijk de **Connection String** voor de naam en sleutelwaarden die zijn.  

## <a name="next"></a>Volgende
Aanvullende onderwerpen voor Azure BizTalk Services:

* [De Azure BizTalk Services SDK installeren](https://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
* [Zelfstudie: Azure BizTalk Services](https://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
* [De Azure BizTalk Services SDK gaan gebruiken](https://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
* [Azure BizTalk Services](https://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>

## <a name="see-also"></a>Zie ook
* [Procedure: ACS-Management-Service gebruiken om te configureren van Service-identiteiten](https://go.microsoft.com/fwlink/p/?LinkID=303942)<br/>
* [BizTalk Services: Developer, Basic, Standard en Premium-edities grafiek](https://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
* [BizTalk Services: Inrichting](https://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
* [BizTalk Services: Diagram van de Status van inrichting](https://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
* [BizTalk Services: Tabbladen dashboard, bewaken en schalen](https://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
* [BizTalk Services: Back-up en herstel](https://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
* [BizTalk Services: Beperking](https://go.microsoft.com/fwlink/p/?LinkID=302282)<br/>

