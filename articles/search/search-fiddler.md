---
title: REST-API's in Postman of Fiddler - Azure Search verkennen
description: Hoe u Postman of Fiddler HTTP-aanvragen en REST-API-aanroepen naar Azure Search.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: quickstart
ms.date: 03/12/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: c99380faee8fd1bc42922f7f0e367edde1154a9b
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58368898"
---
# <a name="quickstart-explore-azure-search-rest-apis-using-postman-or-fiddler"></a>Quickstart: REST API's voor Azure Search verkennen met Fiddler of Postman

Een van de eenvoudigste manieren om de [REST API voor Azure Search](https://docs.microsoft.com/rest/api/searchservice) te verkennen is door Fiddler of Postman te gebruiken om HTTP-aanvragen te formuleren en de reacties te bekijken. Met de juiste hulpmiddelen en deze instructies kunt u aanvragen verzenden en antwoorden bekijken voordat u code gaat schrijven.

> [!div class="checklist"]
> * Een testprogramma voor web-API's downloaden
> * De API-sleutel en het eindpunt voor uw zoekservice ophalen
> * Aanvraagheaders configureren
> * Een index maken
> * Een index laden
> * Een index doorzoeken

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) voordat u begint en meldt u zich daarna aan voor [Azure Search](search-create-service-portal.md).

## <a name="download-tools"></a>Hulpprogramma's downloaden

De volgende hulpprogramma's worden veel gebruikt voor webontwikkeling, maar ook als u bekend bent met een ander programma, zouden de instructies in dit artikel nog steeds van toepassing moeten zijn.

+ [Postman bureaublad-app](https://www.getpostman.com/)
+ [Telerik Fiddler](https://www.telerik.com/fiddler)

## <a name="get-the-api-key-and-endpoint"></a>De API-sleutel en het eindpunt ophalen

REST-aanroepen hebben voor elke aanvraag de service-URL en een toegangssleutel nodig. Een zoekservice wordt gemaakt met beide, dus als u Azure Search hebt toegevoegd aan uw abonnement, volgt u deze stappen om de benodigde gegevens op te halen:

1. In de Azure-portal in uw zoekservice **overzicht** pagina, de URL ophalen. Een eindpunt ziet er bijvoorbeeld uit als `https://my-service-name.search.windows.net`.

2. In **instellingen** > **sleutels**, een beheersleutel voor volledige rechten voor de service ophalen. Er zijn twee uitwisselbaar beheersleutels, verstrekt voor bedrijfscontinuïteit voor het geval u moet een meegenomen. U kunt de primaire of secundaire sleutel gebruiken voor verzoeken voor toevoegen, wijzigen en verwijderen van objecten.

![Een HTTP-eindpunt en -sleutel ophalen](media/search-fiddler/get-url-key.png "een HTTP-eindpunt en -sleutel ophalen")

Alle aanvragen vereisen een api-sleutel bij elke aanvraag verzonden naar uw service. Met een geldige sleutel stelt u per aanvraag een vertrouwensrelatie in tussen de toepassing die de aanvraag verzendt en de service die de aanvraag afhandelt.


## <a name="configure-headers"></a>Headers configureren

Elk hulpprogramma bewaart informatie over aanvraagheaders voor de sessie, wat betekent dat u het URL-eindpunt, de API-versie, de API-sleutel en het inhoudstype maar één keer hoeft in te voeren.

De volledige URL moet lijken op het onderstaande voorbeeld, alleen moet in uw geval de naam van de tijdelijke aanduiding **`my-app`** zijn vervangen door een geldige waarde:`https://my-app.search.windows.net/indexes/hotels?api-version=2017-11-11`

De service-URL bestaat uit de volgende elementen:

+ HTTPS-voorvoegsel.
+ Service-URL, verkregen via de portal.
+ Resource, een bewerking die een object in uw service maakt. In deze stap is het een index met de naam *hotels*.
+ api-version, een vereiste tekenreeks in kleine letters die wordt opgegeven als '?api-version=2017-11-11' voor de huidige versie. [API-versies](search-api-versions.md) worden regelmatig bijgewerkt. Als u de API-versie toevoegt aan elke aanvraag, kunt u precies bepalen welke versie wordt gebruikt.  

De aanvraagheader bestaat uit twee elementen, inhoudstype (content-type) en de API-sleutel (api-key) die in het vorige gedeelte werd beschreven:

    api-key: <placeholder>
    Content-Type: application/json


### <a name="postman"></a>Postman

Formuleer een aanvraag die vergelijkbaar is met de volgende schermopname. Kies **PUT** als bewerking. 

![De aanvraagheader voor Postman][6]

### <a name="fiddler"></a>Fiddler

Formuleer een aanvraag die vergelijkbaar is met de volgende schermopname. Kies **PUT** als bewerking. Fiddler voegt `User-Agent=Fiddler` toe. U kunt de twee aanvullende aanvraagheaders op nieuwe regels eronder plakken. Voeg het inhoudstype en de API-sleutel voor uw service toe waarbij u de beheersleutel voor toegang voor uw service gebruikt.

![De aanvraagheader van Fiddler][1]

> [!Tip]
> Uitschakelen van beveiligd webverkeer afwijkende en niet-gerelateerde HTTP-activiteit te verbergen. In de Fiddler **bestand** in het menu uitschakelen **verkeer vastleggen**. 

## <a name="1---create-an-index"></a>1 - Een index maken

Het hoofdgedeelte van de aanvraag bevat de indexdefinitie. Door het hoofdgedeelte van de aanvraag toe te voegen, voltooit u de aanvraag die uw index produceert.

Naast de naam van de index is het belangrijkste onderdeel in de aanvraag de verzameling velden. Deze verzameling velden definieert het indexschema. Geef het type voor elk veld op. Tekenreeksvelden worden gebruikt voor zoekopdrachten in de volledige tekst. Voeg daarom numerieke gegevens als tekenreeksen toe als u wilt dat naar die inhoud kan worden gezocht.

Kenmerken voor het veld bepalen de toegestane bewerking. De REST API's staan standaard veel bewerkingen toe. Standaard kunnen alle tekenreeksen bijvoorbeeld worden doorzocht, opgehaald en gefilterd en zijn ze geschikt voor facetten. Vaak hoeft u de kenmerken alleen in te stellen als u een bewerking wilt uitschakelen. Zie [Index maken (REST)](https://docs.microsoft.com/rest/api/searchservice/create-index) voor meer informatie over kenmerken.

          {
         "name": "hotels",  
         "fields": [
           {"name": "hotelId", "type": "Edm.String", "key":true, "searchable": false},
           {"name": "baseRate", "type": "Edm.Double"},
           {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
           {"name": "hotelName", "type": "Edm.String"},
           {"name": "category", "type": "Edm.String"},
           {"name": "tags", "type": "Collection(Edm.String)"},
           {"name": "parkingIncluded", "type": "Edm.Boolean"},
           {"name": "smokingAllowed", "type": "Edm.Boolean"},
           {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
           {"name": "rating", "type": "Edm.Int32"},
           {"name": "location", "type": "Edm.GeographyPoint"}
          ]
         }


Wanneer u deze aanvraag indient, krijgt u een HTTP 201-respons om aan te geven dat de index is gemaakt. U kunt deze bewerking controleren in de portal, maar houd er rekening mee dat de portalpagina met vernieuwingsintervallen werkt, zodat het enkele minuten kan duren voordat deze actueel is.

Als u een HTTP 504-respons ontvangt, controleert u of de URL HTTPS bevat. Als de HTTP-fout 400 of 404 wordt weergegeven ziet, controleert u of de aanvraagtekst op fouten die mogelijk zijn opgetreden tijden kopiëren en plakken. Een HTTP 403 duidt doorgaans op een probleem met de API-sleutel (een ongeldige sleutel of een syntaxisfout in de opgegeven API-sleutel).


### <a name="postman"></a>Postman

Kopieer de indexdefinitie naar het hoofdgedeelte van de aanvraag, zoals in de volgende schermopname en klik vervolgens op **Send** (Verzenden) rechtsboven om de voltooide aanvraag te verzenden.

![Het hoofdgedeelte van de aanvraag in Postman][8]

### <a name="fiddler"></a>Fiddler

Kopieer de indexdefinitie naar het hoofdgedeelte van de aanvraag, zoals in de volgende schermopname en klik vervolgens op **Execute** (Uitvoeren) rechtsboven om de voltooide aanvraag te verzenden.

![Het hoofdgedeelte van de aanvraag in Fiddler][7]

## <a name="2---load-documents"></a>2 - documenten laden

De index maken en de index vullen zijn afzonderlijke stappen. In Azure Search bevat de index alle doorzoekbare gegevens die u kunt aanleveren als JSON-documenten. Als u de API voor deze bewerking wilt bekijken, raadpleegt u [Add, update, or delete documents (REST)](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) (Documenten toevoegen, bijwerken of verwijderen (REST)).

+ Wijzig de bewerking voor deze stap in **POST**.
+ Wijzig het eindpunt zodat dit `/docs/index` bevat. De volledige URL moet er dan als volgt uitzien: `https://my-app.search.windows.net/indexes/hotels/docs/index?api-version=2017-11-11`.
+ Laat de aanvraagheaders ongewijzigd. 

De aanvraagtekst bevat vier documenten die moeten worden toegevoegd aan de index hotels.

         {
         "value": [
         {
             "@search.action": "upload",
             "hotelId": "1",
             "baseRate": 199.0,
             "description": "Best hotel in town",
             "description_fr": "Meilleur hôtel en ville"
             "hotelName": "Fancy Stay",
             "category": "Luxury",
             "tags": ["pool", "view", "wifi", "concierge"],
             "parkingIncluded": false,
             "smokingAllowed": false,
             "lastRenovationDate": "2010-06-27T00:00:00Z",
             "rating": 5,
             "location": { "type": "Point", "coordinates": [-122.131577, 47.678581] }
           },
           {
             "@search.action": "upload",
             "hotelId": "2",
             "baseRate": 79.99,
             "description": "Cheapest hotel in town",
             "description_fr": "Hôtel le moins cher en ville",
             "hotelName": "Roach Motel",
             "category": "Budget",
             "tags": ["motel", "budget"],
             "parkingIncluded": true,
             "smokingAllowed": true,
             "lastRenovationDate": "1982-04-28T00:00:00Z",
             "rating": 1,
             "location": { "type": "Point", "coordinates": [-122.131577, 49.678581] }
           },
           {
             "@search.action": "upload",
             "hotelId": "3",
             "baseRate": 279.99,
             "description": "Surprisingly expensive",
             "hotelName": "Dew Drop Inn",
             "category": "Bed and Breakfast",
             "tags": ["charming", "quaint"],
             "parkingIncluded": true,
             "smokingAllowed": false,
             "lastRenovationDate": null,
             "rating": 4,
             "location": { "type": "Point", "coordinates": [-122.33207, 47.60621] }
           },
           {
             "@search.action": "upload",
             "hotelId": "4",
             "baseRate": 220.00,
             "description": "This could be the one",
             "hotelName": "A Hotel for Everyone",
             "category": "Basic hotel",
             "tags": ["pool", "wifi"],
             "parkingIncluded": true,
             "smokingAllowed": false,
             "lastRenovationDate": null,
             "rating": 4,
             "location": { "type": "Point", "coordinates": [-122.12151, 47.67399] }
           }
          ]
         }

Over enkele seconden verschijnt er een HTTP 200-respons in de sessielijst. Dit geeft aan dat de documenten zijn gemaakt. 

Als u een 207-respons ontvang, is minimaal één document niet geüpload. Als u een 404-respons ontvangt, bevat de header of het hoofdgedeelte van de aanvraag een syntaxisfout: controleer of u het eindpunt hebt gewijzigd zodat dit `/docs/index` bevat.

> [!Tip]
> Voor bepaalde gegevensbronnen kunt u de alternatieve methode *indexer* gebruiken die de indexering vereenvoudigt en de hoeveelheid code die is vereist vermindert. Zie [Indexeerbewerkingen](https://docs.microsoft.com/rest/api/searchservice/indexer-operations) voor meer informatie.


### <a name="postman"></a>Postman

Wijzig de bewerking in **POST**. Wijzig de URL zodat deze `/docs/index` bevat. Kopieer de documenten naar het hoofdgedeelte van de aanvraag, zoals in de volgende schermopname, en voer de aanvraag uit.

![Aanvraagpayload in Postman][10]

### <a name="fiddler"></a>Fiddler

Wijzig de bewerking in **POST**. Wijzig de URL zodat deze `/docs/index` bevat. Kopieer de documenten naar het hoofdgedeelte van de aanvraag, zoals in de volgende schermopname, en voer de aanvraag uit.

![Aanvraagpayload in Fiddler][9]

## <a name="3---search-an-index"></a>3 - Een index doorzoeken
Nu dat er een index en documenten zijn geladen, kunt u query's op te geven met behulp van [documenten zoeken](https://docs.microsoft.com/rest/api/searchservice/search-documents) REST-API.

+ Wijzig de bewerking voor deze stap in **GET**.
+ Verander het eindpunt zodat dit queryparameters bevat, waaronder zoekreeksen. Een query-URL ziet er bijvoorbeeld uit als `https://my-app.search.windows.net/indexes/hotels/docs?search=motel&$count=true&api-version=2017-11-11`.
+ Laat de aanvraagheaders ongewijzigd.

Deze query zoekt de term 'motel' en retourneert het aantal documenten in de zoekresultaten. De aanvraag en het antwoord moeten eruitzien als in de volgende schermopname voor Postman nadat u op **Send** (Verzenden) hebt geklikt. De statuscode moet 200 zijn.

 ![Reactie op Postman-query][11]

### <a name="tips-for-running-our-sample-queries-in-fiddler"></a>Tips voor het uitvoeren van onze voorbeeldquery's in Fiddler

De volgende voorbeeldquery is afkomstig uit het artikel [Zoekindex (Azure Search API)](https://docs.microsoft.com/rest/api/searchservice/Search-Documents). Veel van de voorbeeldquery's in dit artikel bevatten spaties. Deze zijn niet toegestaan in Fiddler. Vervang elke spatie door een plusteken (+) voordat u de queryreeks in Fiddler plakt en uitvoert.

**Voordat de spaties zijn vervangen (in lastRenovationDate desc):**

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate desc&api-version=2017-11-11

**Nadat de spaties zijn vervangen door + (in lastRenovationDate+desc):**

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate+desc&api-version=2017-11-11

## <a name="get-index-properties"></a>Indexeigenschappen ophalen
U kunt ook een query toepassen op systeemgegevens om het aantal documenten en het opslagverbruik op te vragen: `https://my-app.search.windows.net/indexes/hotels/stats?api-version=2017-11-11`

In Postman moet uw aanvraag er ongeveer als volgt uitzien en bevat de reactie het aantal documenten en de gebruikte ruimte in bytes.

 ![Systeemquery in Postman][12]

Let op dat de syntaxis van api-version verschilt. Voeg voor deze aanvraag `?` toe aan api-version. Het vraagteken (?) scheidt het URL-pad van de querytekenreeks, terwijl & de 'naam= waarde'-paren in de queryreeks scheidt. Voor deze query is api-version het eerste en enige item in de querytekenreeks.

Zie [Get Index Statistics (REST)](https://docs.microsoft.com/rest/api/searchservice/get-index-statistics) (Indexstatistieken ophalen (REST)) voor meer informatie over deze API.


### <a name="tips-for-viewing-index-statistic-in-fiddler"></a>Tips voor het weergeven van indexstatistieken in Fiddler

Klik in Fiddler op het tabblad **Inspectors**, klik op het tabblad **Headers** en selecteer vervolgens de JSON-indeling. Als het goed is wordt het aantal documenten en de opslaggrootte (in kB) weergegeven.

## <a name="next-steps"></a>Volgende stappen

REST-clients zijn zeer nuttig voor onvoorbereide verkenning, maar nu u weet hoe de REST API's werken, kunt u een stapje verdergaan met code. Raadpleeg de volgende koppelingen voor de volgende stappen:

+ [Snelstart: Een index maken met .NET SDK](search-create-index-dotnet.md)
+ [Snelstart: Maken van een index (REST) met behulp van PowerShell](search-create-index-rest-api.md)

<!--Image References-->
[1]: ./media/search-fiddler/fiddler-url.png
[2]: ./media/search-fiddler/AzureSearch_Fiddler2_PostDocs.png
[3]: ./media/search-fiddler/AzureSearch_Fiddler3_Query.png
[4]: ./media/search-fiddler/AzureSearch_Fiddler4_QueryResults.png
[5]: ./media/search-fiddler/AzureSearch_Fiddler5_QueryStats.png
[6]: ./media/search-fiddler/postman-url.png
[7]: ./media/search-fiddler/fiddler-request.png
[8]: ./media/search-fiddler/postman-request.png
[9]: ./media/search-fiddler/fiddler-docs.png
[10]: ./media/search-fiddler/postman-docs.png
[11]: ./media/search-fiddler/postman-query.png
[12]: ./media/search-fiddler/postman-system-query.png