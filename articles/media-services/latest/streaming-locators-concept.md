---
title: Streaming-Locators in Azure mediaservices | Microsoft Docs
description: Dit artikel bevat een uitleg over wat Streaming-Locators zijn en hoe ze worden gebruikt door Azure Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 12/20/2018
ms.author: juliako
ms.openlocfilehash: 658843fd5acbe0d4e29947e99c00edf4909fe9f4
ms.sourcegitcommit: 21466e845ceab74aff3ebfd541e020e0313e43d9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53742742"
---
# <a name="streaming-locators"></a>Streaming-locators

U moet uw clients met een URL die kan worden gebruikt om af te spelen gecodeerde video of audio-bestanden opgeven, moet u maken een [Streaming-Locator gemaakt](https://docs.microsoft.com/rest/api/media/streaminglocators) en de streaming-URL's maken. Zie voor meer informatie, [Stream van een bestand](stream-files-dotnet-quickstart.md).

## <a name="streaminglocator-definition"></a>StreamingLocator definitie

De volgende tabel ziet u de eigenschappen van de StreamingLocator en biedt de definities.

|Name|Description|
|---|---|
|id |Volledig gekwalificeerde resource-ID voor de resource.|
|naam|De naam van de resource.|
|properties.alternativeMediaId|Alternatieve Media-ID van deze Streaming-Locator.|
|properties.assetName|Assetnaam|
|properties.contentKeys|De Inhoudssleutels die worden gebruikt door deze Streaming-Locator gemaakt.|
|Properties.created|De aanmaaktijd van de Streaming-Locator gemaakt.|
|properties.defaultContentKeyPolicyName|De naam van de standaard ContentKeyPolicy die worden gebruikt door deze Streaming-Locator gemaakt.|
|properties.endTime|De eindtijd van de Streaming-Locator gemaakt.|
|properties.startTime|De begintijd van de Streaming-Locator gemaakt.|
|properties.streamingLocatorId|De StreamingLocatorId van de Streaming-Locator.|
|properties.streamingPolicyName |De naam van het Streaming-beleid die worden gebruikt door deze Streaming-Locator gemaakt. Geef de naam van Streaming-beleid dat u hebt gemaakt of gebruik een van de vooraf gedefinieerde beleidsregels voor Streaming. De vooraf gedefinieerde Streaming beleidsregels beschikbaar zijn: 'Predefined_DownloadOnly', 'Predefined_ClearStreamingOnly', 'Predefined_DownloadAndClearStreaming', 'Predefined_ClearKey', 'Predefined_MultiDrmCencStreaming' en 'Predefined_MultiDrmStreaming'|
|type|Het type van de resource.|

Zie voor de definitie van de volledige [Streaming-Locators](https://docs.microsoft.com/rest/api/media/streaminglocators).

## <a name="filtering-ordering-paging"></a>Filters, bestellen, wisselbestand

Media Services ondersteunt de volgende OData-queryopties voor Streaming-Locators: 

* $filter 
* $orderby 
* $top 
* $skiptoken 

Beschrijving van de operator:

* EQ = gelijk zijn aan
* Ne = niet gelijk zijn aan
* Ge = groter dan of gelijk aan
* Le = kleiner dan of gelijk aan
* Gt = groter dan
* Lt = minder dan

### <a name="filteringordering"></a>Filteren/bestellen

De volgende tabel ziet u hoe deze opties kunnen worden toegepast op de StreamingLocator-eigenschappen: 

|Name|Filteren|Bestellen|
|---|---|---|
|id |||
|naam|Eq, ne, ge, le, gt, lt|Oplopend of aflopend|
|properties.alternativeMediaId  |||
|properties.assetName   |||
|properties.contentKeys |||
|Properties.created |Eq, ne, ge, le, gt, lt|Oplopend of aflopend|
|properties.defaultContentKeyPolicyName |||
|properties.endTime |Eq, ne, ge, le, gt, lt|Oplopend of aflopend|
|properties.startTime   |||
|properties.streamingLocatorId  |||
|properties.streamingPolicyName |||
|type   |||

### <a name="pagination"></a>Paginering

Paginering wordt voor elk van de vier ingeschakelde sorteervolgorde ondersteund. Op dit moment is de grootte van 10.

> [!TIP]
> U moet de volgende koppeling altijd gebruiken om inventariseren van de verzameling en niet afhankelijk van het formaat van een bepaalde pagina.

Als een query-antwoord veel items bevat, retourneert de service een "\@odata.nextLink" eigenschap om de volgende pagina van de resultaten. Dit kan worden gebruikt door de volledige resultatenset. U kunt het formaat van de pagina niet configureren. 

Als StreamingLocators worden gemaakt of verwijderd, wanneer de verzameling worden er pagina's, worden de wijzigingen doorgevoerd in de geretourneerde resultaten (als deze wijzigingen zijn in het gedeelte van de verzameling die niet zijn gedownload.) 

De volgende C#-voorbeeld laat zien hoe om te inventariseren alle StreamingLocators in het account.

```csharp
var firstPage = await MediaServicesArmClient.StreamingLocators.ListAsync(CustomerResourceGroup, CustomerAccountName);

var currentPage = firstPage;
while (currentPage.NextPageLink != null)
{
    currentPage = await MediaServicesArmClient.StreamingLocators.ListNextAsync(currentPage.NextPageLink);
}
```

Zie voor voorbeelden van REST [Streaming-Locators - lijst](https://docs.microsoft.com/rest/api/media/streaminglocators/list)

## <a name="next-steps"></a>Volgende stappen

[Een bestand streamen](stream-files-dotnet-quickstart.md)
