---
title: Overzicht van Azure Stream Analytics
description: Lees over Stream Analytics, een beheerde service waarmee u streaminggegevens van het Internet of Things (IoT) in realtime kun analyseren.
author: jseb225
ms.author: jeanb
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: overview
ms.custom: seodec18
ms.date: 12/07/2018
ms.openlocfilehash: 09f402f81700b53eb9e4a95e36545ef02850660a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "61479684"
---
# <a name="what-is-azure-stream-analytics"></a>Wat is Azure Stream Analytics?

Azure Stream Analytics is een gebeurtenissen verwerkende engine waarmee u grote volumina gegevens kunt bekijken die vanaf apparaten worden gestreamd. De inkomende gegevens kunnen afkomstig zijn van apparaten, sensoren, websites, socialemediafeeds, toepassingen, enzovoort. Het ondersteunt tevens het extraheren van gegevens uit gegevensstromen, het identificeren van patronen en relaties. U kunt deze patronen gebruiken om andere acties, zoals het maken van waarschuwingen, downstream te activeren, gegevens in een rapportageprogramma in te voeren of op te slaan voor later gebruik.

Hier volgen enkele voorbeelden waarbij Azure Stream Analytics kan worden gebruikt: 

* Sensorenfusie voor het Internet of Things (IoT) en realtime analytische gegevens voor apparaattelemetrie
* Weblogboeken/clickstream-analyses op internet
* Georuimtelijke analyses voor fleetmanagement en zelfrijdende auto’s
* Op afstand bewaken en predictief onderhoud van hoogwaardige middelen
* Realtime analyse van Point of Sale-gegevens voor voorraadbeheer en anomaliedetectie

## <a name="how-does-stream-analytics-work"></a>Hoe werkt Stream Analytics?

Azure Stream Analytics begint met een bron van streaming gegevens die in Azure Event Hub of Azure IoT Hub wordt opgenomen of afkomstig is van een gegevensarchief als Azure Blob Storage. Voor het onderzoeken van de stromen maakt u een Stream Analytics-taak die de invoerbron aangeeft die de gegevens streamt. De taak bevat ook een specificatie van een transformatiequery; hoe er wordt gezocht naar gegevens, patronen of relaties. De transformatiequery maakt gebruik van een SQL-querytaal waarmee streaminggegevens gedurende een bepaalde tijd eenvoudig kunnen worden gefilterd, gesorteerd, gecombineerd en samengevoegd. Als de taak wordt uitgevoerd, kunt u de opties voor het sorteren van de gebeurtenissen en de duur van tijdvensters aanpassen bij het uitvoeren van combinatiebewerkingen.

Nadat de binnenkomende gegevens zijn geanalyseerd, geeft u een uitvoer op voor de getransformeerde gegevens en kunt u bepalen wat u moet doen als reactie op de geanalyseerde gegevens. U kunt bijvoorbeeld de volgende acties uitvoeren:

* Gegevens verzenden naar een bewaakte wachtrij om downstream waarschuwingen of aangepaste werkstromen te activeren.
* Gegevens verzenden naar Power BI-dashboard voor realtime visualisatie.
* Gegevens in andere Azure-opslagservices opslaan, zodat u een Machine Learning-model kunt trainen op basis van historische gegevens of batchanalyses kunt uitvoeren.

In de volgende afbeelding ziet u de Stream Analytics-pijplijn. De Stream Analytics-taak kan gebruikmaken van alle of een bepaald deel van de in- en uitvoer. In deze afbeelding wordt getoond hoe gegevens naar Stream Analytics worden verzonden, geanalyseerd en verder verzonden voor andere acties, zoals opslag of presentatie:

![Pijplijn voor binnenkomende gegevens voor Stream Analytics](./media/stream-analytics-introduction/stream-analytics-intro-pipeline.png)

## <a name="key-capabilities-and-benefits"></a>Belangrijkste mogelijkheden en voordelen

Azure Stream Analytics is gebruiksvriendelijk, flexibel, betrouwbaar en schaalbaar tot elke taakgrootte. Het is beschikbaar in meerdere Azure-regio's. In de volgende afbeelding worden de belangrijkste mogelijkheden van Azure Stream Analytics getoond:

![Belangrijkste mogelijkheden van Azure Stream Analytics](./media/stream-analytics-introduction/stream-analytics-key-capabilities.png)

## <a name="ease-of-getting-started"></a>Eenvoudig aan de slag

Met Azure Stream Analytics kunt u zo aan de slag. Met slecht een paar klikken maakt u verbinding met meerdere bronnen en sinks, en om een end-to-endpijplijn te maken. Stream Analytics kan verbinding maken met [Azure Event Hubs](/azure/event-hubs/) of [Azure IoT Hub](/azure/iot-hub/) voor opname van streaming gegevens. Het kan ook verbinding maken met de service [Azure Blob Storage](/azure/storage/storage-introduction) voor opname van historische gegevens. Het kan gegevens van gebeurtenishubs combineren met andere gegevensbronnen en verwerkingsengines. Taakinvoer kan ook bestaan uit statische verwijzingsgegevens of gegevens die weinig veranderen, en u kunt streaming gegevens met deze verwijzingsgegevens samenvoegen om opzoekbewerkingen uit te voeren.

Stream Analytics kan taakuitvoer omleiden naar talloze opslagsystemen, bijvoorbeeld [Azure Blob](/azure/storage/storage-introduction), [Azure SQL Database](/azure/sql-database/), [Azure Data Lake Stores](/azure/data-lake-store/) of [Azure Cosmos DB](/azure/cosmos-db/introduction). Na opslag kunt u batch-analyses uitvoeren met HDInsight of de uitvoer verzenden naar een andere service, zoals gebeurtenishubs voor verbruik, of naar [Power BI](https://docs.microsoft.com/power-bi/) voor realtime visualisatie door gebruik te maken van een streaming API van Power BI.

## <a name="programmer-productivity"></a>Productiviteit van programmeurs

Azure Stream Analytics maakt gebruik van een eenvoudige, op SQL gebaseerde querytaal die is uitgebreid met krachtige, tijdelijke beperkingen voor het analyseren van gegevens in beweging. Voor het definiëren van taaktransformaties gebruikt u een eenvoudige, declaratieve [Stream Analytics-querytaal](https://msdn.microsoft.com/library/azure/dn834998.aspx) waarmee u complexe, tijdelijke query’s en analyses kunt schrijven met behulp van SQL-constructs. De querytaal van Stream Analytics is consistent met SQL. Als u bekend bent met SQL, is dat voldoende om taken te kunnen maken. U kunt ook taken maken met ontwikkelaarstalen als Azure PowerShell, [Stream Analytics Visual Studio tools](stream-analytics-tools-for-visual-studio-install.md) of Azure Resource Manager-sjablonen. Met ontwikkelaarstalen kunt u offline transformatiequery’s ontwikkelen en de [CI/CD-pijplijn](stream-analytics-tools-for-visual-studio-cicd.md) gebruiken om taken bij Azure in te dienen. 

De querytaal van Stream Analytics biedt een breed spectrum aan functies voor het analyseren en verwerken van streaming gegevens. Deze querytaal ondersteunt eenvoudige gegevensmanipulatie, van aggregatiefuncties tot complexe, georuimtelijke functies. U kunt query’s in de portal bewerken en ze testen met voorbeeldgegevens die uit de live stream worden geëxtraheerd.

U kunt de mogelijkheden van de querytaal uitbreiden door extra functie te definiëren en aan te roepen. In de Azure Machine Learning-service kunt u functieaanroepen definiëren om gebruik te kunnen maken van Azure Machine Learning-oplossingen en door gebruikers gedefinieerde JavaScript-functies (UDF’s) of door gebruikers gedefinieerde aggregaten te integreren voor het uitvoeren van complexe berekeningen als onderdeel van een Stream Analytics-query.

## <a name="fully-managed"></a>Volledig beheerd 

Azure Stream Analytics is een volledige beheerde (PaaS) aanbieding in Azure. Dit betekent dat u geen hardware hoeft in te richten of clusters te beheren om uw taken te kunnen uitvoeren. Azure Stream Analytics beheert uw taak volledig door het instellen van complexe rekenclusters in de cloud en het afstemmen van de prestaties om de taak te kunnen uitvoeren. Dankzij integratie met Azure Event Hubs en Azure IoT Hub kunnen taken miljoenen gebeurtenissen per seconde opnemen, afkomstig van verbonden apparaten, clickstreams, logboekbestanden, enzovoort. Met de partitioneringsfunctie van Event Hubs kunt u berekeningen partitioneren in logische stappen, die elk verder kunnen worden gepartitioneerd voor meer schaalbaarheid.

## <a name="run-in-the-cloud-on-in-the-intelligent-edge"></a>Voer in de cloud op in de intelligente edge

Azure Stream Analytics kunt uitvoeren in de cloud, voor grootschalige analyse, of op de intelligente edge voor zeer lage latentie analyses uitvoeren.
Azure Stream Analytics gebruikt dezelfde querytaal op zowel de cloud en de intelligente edge, zodat ontwikkelaars kunnen bouwen werkelijk hybride architecturen voor de verwerking.

## <a name="low-total-cost-of-ownership"></a>Lage total cost of ownership

Als cloudservice is Stream Analytics kostenefficiënt. Er is geen sprake van opstartkosten; u betaalt slechts voor de [streaming-eenheden die u verbruikt](stream-analytics-streaming-unit-consumption.md) en de hoeveelheid verwerkte gegevens. Er is geen verplichting en het inrichten van clusters is niet nodig. U kunt de streaming taken omhoog of omlaag schalen, afhankelijk van de bedrijfsbehoeften. 

## <a name="mission-critical-ready"></a>Essentieel gereed
Azure Stream Analytics is beschikbaar in meerdere regio's over de hele wereld en is ontworpen om uw essentiële workloads uit te voeren doordat de vereisten voor betrouwbaarheid, veiligheid en naleving worden ondersteund.
### <a name="reliability"></a>Betrouwbaarheid
Azure Stream Analytics garandeert Exactly-once-gebeurtenissenverwerking en At-least-once-levering van gebeurtenissen, zodat gebeurtenissen nooit verloren gaan. Exact-één keer worden verwerkt is gegarandeerd met geselecteerde uitvoer, zoals beschreven in [gebeurtenis levering garanties](/stream-analytics-query/event-delivery-guarantees-azure-stream-analytics).
Azure Stream Analytics bevat ingebouwde herstelfuncties in geval de levering van een gebeurtenis mislukt. Stream Analytics biedt ook de ingebouwde mogelijkheid voor het gebruik van controlepunten om de status van uw taak bij te houden, en levert herhaalbare resultaten.

Als een beheerde service garandeert Stream Analytics een beschikbaarheid op minuutniveau van 99,9% voor de verwerking van gebeurtenissen. Zie de pagina [Stream Analytics SLA](https://azure.microsoft.com/support/legal/sla/stream-analytics/v1_0/) voor meer informatie. 

### <a name="security"></a>Beveiliging
Wat betreft beveiliging, versleutelt Azure Stream Analytics alle inkomende en uitgaande communicatie en biedt ondersteuning voor TLS 1.2. Ingebouwde controlepunten zijn eveneens versleuteld. Stream Analytics slaat de binnenkomende gegevens niet op omdat alle verwerking in het geheugen gebeurt. 

### <a name="compliance"></a>Naleving
Azure Stream Analytics volgt meerdere nalevingscertificeringen, zoals beschreven in het [Overzicht van Azure-naleving](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942). 

## <a name="performance"></a>Prestaties

Stream Analytics verwerkt miljoenen gebeurtenissen per seconde en kan resultaten met een lage latentie leveren.
U kunt ermee omhoogschalen of uitschalen, zodat u intensieve toepassingen kunt afhandelen die in realtime complexe gebeurtenissen verwerken. Stream Analytics ondersteunt de prestaties door gebruik te maken van partitionering, zodat complexe query's parallel en op meerdere streamingknooppunten kunnen worden uitgevoerd.
Azure Stream Analytics is gebouwd op [Trill](https://github.com/Microsoft/Trill), een krachtige in-memory engine voor streaminganalyse die is ontwikkeld in samenwerking met Microsoft Research. 

## <a name="next-steps"></a>Volgende stappen

U hebt nu een overzicht van Azure Stream Analytics. Hierna kunt u zich verder in de materie verdiepen en uw eerste Stream Analytics-taak maken:

* [Een Stream Analytics-taak maken via Azure Portal](stream-analytics-quick-create-portal.md).
* [Create a Stream Analytics job by using Azure PowerShell](stream-analytics-quick-create-powershell.md) (Een Stream Analytics-taak maken met behulp van Azure PowerShell).
* [Create a Stream Analytics job by using Visual Studio](stream-analytics-quick-create-vs.md) (Een Stream Analytics-taak maken met behulp van Visual Studio).

