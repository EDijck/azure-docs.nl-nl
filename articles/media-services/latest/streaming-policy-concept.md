---
title: Streaming-beleid in Azure mediaservices | Microsoft Docs
description: Dit artikel bevat een uitleg over wat Streaming beleidsregels zijn en hoe ze worden gebruikt door Azure Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 12/22/2018
ms.author: juliako
ms.openlocfilehash: d74ce913a2189dd1062b30f9def919cbbabe7b64
ms.sourcegitcommit: 21466e845ceab74aff3ebfd541e020e0313e43d9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53742521"
---
# <a name="streaming-policies"></a>Beleid voor streaming

In Azure Media Services v3 kunt beleid voor Streaming u protocollen voor streaming- en versleutelingsopties voor uw StreamingLocators definiëren. U kunt de naam opgeven van Streaming-beleid dat u hebt gemaakt of gebruik een van de vooraf gedefinieerde beleidsregels voor Streaming. De vooraf gedefinieerde Streaming beleidsregels die momenteel beschikbaar zijn: 'Predefined_DownloadOnly', 'Predefined_ClearStreamingOnly', 'Predefined_DownloadAndClearStreaming', 'Predefined_ClearKey', 'Predefined_MultiDrmCencStreaming' en 'Predefined_MultiDrmStreaming'.

> [!IMPORTANT]
> Wanneer u een aangepaste [StreamingPolicy](https://docs.microsoft.com/rest/api/media/streamingpolicies), moet u een beperkte set van dergelijk beleid ontwerpen voor uw Media Service-account en opnieuw gebruiken voor uw Streaming-Locators wanneer de dezelfde versleutelingsopties en protocollen die nodig zijn. Uw Media Service-account heeft een quotum voor het aantal vermeldingen van het toegangsbeleid voor Streaming. U moet niet worden het maken van een nieuw beleid voor Streaming voor elke Streaming-Locator gemaakt.

## <a name="streamingpolicy-definition"></a>StreamingPolicy definitie

De volgende tabel ziet u de eigenschappen van de StreamingPolicy en biedt de definities.

|Name|Description|
|---|---|
|id|Volledig gekwalificeerde resource-ID voor de resource.|
|naam|De naam van de resource.|
|properties.commonEncryptionCbcs|Configuratie van CommonEncryptionCbcs|
|properties.commonEncryptionCenc|Configuratie van CommonEncryptionCenc|
|Properties.created |Tijd voor het maken van beleid voor Streaming|
|properties.defaultContentKeyPolicyName |Standaard ContentKey gebruikt door het huidige beleid voor Streaming|
|properties.envelopeEncryption  |Configuratie van EnvelopeEncryption|
|properties.noEncryption|Configuraties van NoEncryption|
|type|Het type van de resource.|

Zie voor de definitie van de volledige [Streaming beleid](https://docs.microsoft.com/rest/api/media/streamingpolicies).

## <a name="filtering-ordering-paging"></a>Filters, bestellen, wisselbestand

Media Services ondersteunt de volgende OData-queryopties voor Streaming-beleid: 

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

De volgende tabel ziet u hoe deze opties kunnen worden toegepast op de StreamingPolicy-eigenschappen: 

|Name|Filteren|Bestellen|
|---|---|---|
|id|||
|naam|Eq, ne, ge, le, gt, lt|Oplopend of aflopend|
|properties.commonEncryptionCbcs|||
|properties.commonEncryptionCenc|||
|Properties.created |Eq, ne, ge, le, gt, lt|Oplopend of aflopend|
|properties.defaultContentKeyPolicyName |||
|properties.envelopeEncryption|||
|properties.noEncryption|||
|type|||

### <a name="pagination"></a>Paginering

Paginering wordt voor elk van de vier ingeschakelde sorteervolgorde ondersteund. Op dit moment is de grootte van 10.

> [!TIP]
> U moet de volgende koppeling altijd gebruiken om inventariseren van de verzameling en niet afhankelijk van het formaat van een bepaalde pagina.

Als een query-antwoord veel items bevat, retourneert de service een "\@odata.nextLink" eigenschap om de volgende pagina van de resultaten. Dit kan worden gebruikt door de volledige resultatenset. U kunt het formaat van de pagina niet configureren. 

Als StreamingPolicy worden gemaakt of verwijderd, wanneer de verzameling worden er pagina's, worden de wijzigingen doorgevoerd in de geretourneerde resultaten (als deze wijzigingen zijn in het gedeelte van de verzameling die niet zijn gedownload.) 

De volgende C#-voorbeeld laat zien hoe om te inventariseren alle StreamingPolicies in het account.

```csharp
var firstPage = await MediaServicesArmClient.StreamingPolicies.ListAsync(CustomerResourceGroup, CustomerAccountName);

var currentPage = firstPage;
while (currentPage.NextPageLink != null)
{
    currentPage = await MediaServicesArmClient.StreamingPolicies.ListNextAsync(currentPage.NextPageLink);
}
```

Zie voor voorbeelden van REST [Streaming beleid - lijst](https://docs.microsoft.com/rest/api/media/streamingpolicies/list)

## <a name="next-steps"></a>Volgende stappen

[Een bestand streamen](stream-files-dotnet-quickstart.md)
