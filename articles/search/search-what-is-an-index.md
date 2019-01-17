---
title: Definitie van de index en concepten - Azure Search
description: Inleiding tot index termen en concepten in Azure Search, met inbegrip van fysieke structre van een omgekeerde index.
author: brjohnstmsft
manager: jlembicz
ms.author: brjohnst
services: search
ms.service: search
ms.topic: conceptual
ms.date: 11/08/2017
ms.custom: seodec2018
ms.openlocfilehash: 5a39021367c2f51125876081e9174eb372d7b9c9
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2019
ms.locfileid: "54353155"
---
# <a name="indexes-and-indexing-overview-in-azure-search"></a>Indexen en indexering overzicht in Azure Search

In Azure Search, een *index* is een permanente opslag van *documenten* en andere constructies die worden gebruikt voor gefilterde en de volledige tekst zoekt naar een Azure Search-service. Een document is een eenheid die bestaat uit gegevens die kunnen worden doorzocht in uw index. Een e-commercedetailhandel heeft bijvoorbeeld een document voor elk item dat wordt verkocht, een nieuwsbureau heeft een document voor elk artikel, enzovoort. Deze begrippen aan betrouwbaardere database-equivalenten toewijzen: een *index* lijkt conceptueel gezien op een *tabel* en *documenten* lijken ruwweg op *rijen* in een tabel.

Wanneer u toevoegt of uploadt documenten of naar Azure Search zoekopdrachten, verzendt u aanvragen naar een specifieke index in uw zoekservice. Het proces van documenten toe te voegen aan een index wordt genoemd *indexeren*.

## <a name="field-types-and-attributes-in-an-azure-search-index"></a>Veldtypen en kenmerken in een Azure Search-index
Bij het definiëren van het schema moet u de naam, het type en de kenmerken van elk veld in de index opgeven. Het veldtype classificeert de gegevens die in dat veld worden opgeslagen. Kenmerken worden ingesteld op afzonderlijke velden om op te geven hoe het veld wordt gebruikt. De volgende tabellen bevatten de typen en kenmerken die u kunt opgeven.

### <a name="field-types"></a>Veldtypen
| Type | Description |
| --- | --- |
| *Edm.String* |Tekst die van tokens kan worden voorzien om te zoeken in de volledige tekst (woordafbreking, afleiding, enzovoort). |
| *Collection(Edm.String)* |Een lijst met tekenreeksen die van tokens kan worden voorzien om te zoeken in de volledige tekst. Er is geen theoretische bovengrens voor het aantal items in een verzameling, maar de bovengrens van 16 MB voor de nettolading geldt voor alle verzamelingen. |
| *Edm.Boolean* |Bevat de waarden waar/niet waar. |
| *Edm.Int32* |32-bits waarden van een heel getal. |
| *Edm.Int64* |64-bits waarden van een heel getal. |
| *Edm.Double* |Numerieke gegevens met dubbele precisie. |
| *Edm.DateTimeOffset* |Datum- en tijdwaarden die worden weergegeven in de OData-V4-indeling (bijvoorbeeld `yyyy-MM-ddTHH:mm:ss.fffZ` of `yyyy-MM-ddTHH:mm:ss.fff[+/-]HH:mm`). |
| *Edm.GeographyPoint* |Een punt voor een geografische locatie op de wereld. |

Gedetailleerdere informatie over ondersteunde Azure-Search[-gegevenstypen vindt u hier](https://docs.microsoft.com/rest/api/searchservice/Supported-data-types).

### <a name="field-attributes"></a>Veldkenmerken
| Kenmerk | Description |
| --- | --- |
| *Sleutel* |Een tekenreeks met de unieke id van elk document. Deze reeks wordt gebruikt om op te zoeken. Elke index moet een sleutel hebben. Slechts één veld kan de sleutel zijn en het type moet zijn ingesteld op Edm.String. |
| *Ophalen mogelijk* |Hiermee geeft u op of een veld in een zoekresultaat kan worden geretourneerd. |
| *Filterbaar* |Hiermee kan het veld in een filterquery's worden gebruikt. |
| *Sorteerbaar* |Hiermee kan een query de zoekresultaten sorteren op basis van dit veld. |
| *Facetten mogelijk* |Hiermee kunt u een veld gebruiken in een [meervoudige navigatie](search-faceted-navigation.md)structuur om op de gebruiker te filteren. Doorgaans werken velden met terugkerende waarden die u kunt gebruiken om meerdere documenten te groeperen (bijvoorbeeld meerdere documenten die in een bepaalde merk- of servicecategorie vallen) het beste als facetten. |
| *Doorzoekbaar* |Hiermee kunt u in dit veld in de volledige tekst zoeken. |

Gedetailleerdere informatie over ondersteunde Azure-Search[-indexkenmerken vindt u hier](https://docs.microsoft.com/rest/api/searchservice/Create-Index).

## <a name="guidance-for-defining-an-index-schema"></a>Richtlijnen voor het definiëren van een indexschema
Bij het ontwerpen van uw index moet u in de planningsfase de tijd nemen om elke beslissing goed te overwegen. Als u uw index opzet, is het belangrijk dat u in uw achterhoofd houdt wat de gebruiker en uw bedrijf nodig hebben, aangezien aan elk veld de [relevante kenmerken](https://docs.microsoft.com/rest/api/searchservice/Create-Index) moeten worden toegewezen. Als u een index wilt wijzigen nadat deze is geïmplementeerd, moet u de gegevens opnieuw opbouwen en laden.

Als u de vereisten voor gegevensopslag wilt wijzigen, kunt u de capaciteit vergroten of verkleinen door partities toe te voegen of te verwijderen. Zie[Uw zoekservice in Azure beheren](search-manage.md) of [Servicelimieten](search-limits-quotas-capacity.md) voor meer informatie.

