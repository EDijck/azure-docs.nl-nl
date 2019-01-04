---
title: Inhoud van de belangrijkste beleidsregels in mediaservices - Azure | Microsoft Docs
description: Dit artikel bevat een uitleg over wat inhoudsbeleid van de sleutels zijn, en hoe ze worden gebruikt door Azure Media Services.
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
ms.custom: seodec18
ms.openlocfilehash: f12632b20d516c81e21a50cfdda7e40d4163afc1
ms.sourcegitcommit: 21466e845ceab74aff3ebfd541e020e0313e43d9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53742215"
---
# <a name="content-key-policies"></a>Beleid voor inhoudssleutels

U kunt Azure Media Services gebruiken voor het beveiligen van uw media vanaf het moment dat het verlaten van uw computer via opslag, verwerking en levering. Met Media Services, kunt u uw live en on-demand inhoud dynamisch wordt versleuteld met Advanced Encryption Standard (AES-128) of een van de drie belangrijkste digital rights management (DRM)-systemen leveren: Microsoft PlayReady, Google Widevine en FairPlay van Apple. Media Services biedt ook een service voor het leveren van AES-sleutels en DRM (PlayReady, Widevine en FairPlay) licenties voor geautoriseerde clients.

In Azure Media Services v3, een [inhoud sleutel beleid](https://docs.microsoft.com/rest/api/media/contentkeypolicies) kunt u opgeven hoe de inhoudssleutel wordt geleverd als u wilt beëindigen van clients via het onderdeel voor de levering van Media Services-sleutel. Zie voor meer informatie, [overzicht van de beveiliging van inhoud](content-protection-overview.md).

Het is raadzaam dat u opnieuw de dezelfde ContentKeyPolicy voor al uw bedrijfsmiddelen gebruiken. ContentKeyPolicies kunnen worden bijgewerkt, dus als u wilt een rouleren van de sleutel vervolgens u een nieuwe ContentKeyPolicyOption aan de bestaande ContentKeyPolicy met een beperking van de token met de nieuwe sleutels toevoegen kunt. Of u kunt de primaire verificatiesleutel en de lijst met verificatiesleutels in het bestaande beleid en de optie alternatieve bijwerken. Het kan tot 15 minuten voor de levering van sleutel-caches om te werken en het bijgewerkte beleid pikken duren.

## <a name="contentkeypolicy-definition"></a>ContentKeyPolicy definitie

De volgende tabel ziet u de eigenschappen van de ContentKeyPolicy en biedt de definities.

|Name|Description|
|---|---|
|id|Volledig gekwalificeerde resource-ID voor de resource.|
|naam|De naam van de resource.|
|Properties.created |De aanmaakdatum van het beleid|
|Properties.Description |Een beschrijving voor het beleid.|
|properties.lastModified|De datum van laatste wijziging van het beleid|
|Properties.Options |De sleutel beleidsopties.|
|properties.policyId|De verouderde beleids-ID.|
|type|Het type van de resource.|

Zie voor de definitie van de volledige [Inhoudbeleidsregels sleutel](https://docs.microsoft.com/rest/api/media/contentkeypolicies).

## <a name="filtering-ordering-paging"></a>Filters, bestellen, wisselbestand

Media Services ondersteunt de volgende OData-queryopties voor ContentKeyPolicies: 

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

De volgende tabel ziet u hoe deze opties kunnen worden toegepast op de ContentKeyPolicies-eigenschappen: 

|Name|Filteren|Bestellen|
|---|---|---|
|id|||
|naam|Eq, ne, ge, le, gt, lt|Oplopend of aflopend|
|Properties.created |Eq, ne, ge, le, gt, lt|Oplopend of aflopend|
|Properties.Description |Eq, ne, ge, le, gt, lt||
|properties.lastModified|Eq, ne, ge, le, gt, lt|Oplopend of aflopend|
|Properties.Options |||
|properties.policyId|Eq, ne||
|type|||

### <a name="pagination"></a>Paginering

Paginering wordt voor elk van de vier ingeschakelde sorteervolgorde ondersteund. Op dit moment is de grootte van 10.

> [!TIP]
> U moet de volgende koppeling altijd gebruiken om inventariseren van de verzameling en niet afhankelijk van het formaat van een bepaalde pagina.

Als een query-antwoord veel items bevat, retourneert de service een "\@odata.nextLink" eigenschap om de volgende pagina van de resultaten. Dit kan worden gebruikt door de volledige resultatenset. U kunt het formaat van de pagina niet configureren. 

Als ContentKeyPolicies worden gemaakt of verwijderd, wanneer de verzameling worden er pagina's, worden de wijzigingen doorgevoerd in de geretourneerde resultaten (als deze wijzigingen zijn in het gedeelte van de verzameling die niet zijn gedownload.) 

De volgende C#-voorbeeld laat zien hoe om te inventariseren alle ContentKeyPolicies in het account.

```csharp
var firstPage = await MediaServicesArmClient.ContentKeyPolicies.ListAsync(CustomerResourceGroup, CustomerAccountName);

var currentPage = firstPage;
while (currentPage.NextPageLink != null)
{
    currentPage = await MediaServicesArmClient.ContentKeyPolicies.ListNextAsync(currentPage.NextPageLink);
}
```

Zie voor voorbeelden van REST [inhoud sleutelbeleid - lijst](https://docs.microsoft.com/rest/api/media/contentkeypolicies/list)

## <a name="next-steps"></a>Volgende stappen

[Gebruik dynamische AES-128-versleuteling en de sleutelleveringsservice](protect-with-aes128.md)

[Gebruik DRM dynamische versleuteling en licentie leveringsservice voor](protect-with-drm.md)
