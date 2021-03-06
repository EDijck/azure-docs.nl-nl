---
title: Azure SQL-Database zonder server (preview) | Microsoft Docs
description: Dit artikel worden de nieuwe laag voor serverless Computing en vergelijkt deze met de bestaande ingerichte Computing-laag
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: oslake
ms.author: moslake
ms.reviewer: sstein, carlrab
manager: craigg
ms.date: 05/07/2019
ms.openlocfilehash: 2ab8f272fc264f153144803be772d381c1780512
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65143274"
---
# <a name="sql-database-serverless-preview"></a>SQL-Database zonder server (preview)

## <a name="what-is-the-serverless-compute-tier"></a>Wat is de serverloze compute-laag

SQL-Database zonder server (preview) is een compute-laag die facturen voor het bedrag van compute die worden gebruikt door een individuele database op basis van per seconde. Zonder server is geoptimaliseerd voor individuele databases met onregelmatige gebruikspatronen waarvoor het niet erg enige vertraging in compute warmen na gebruik van niet-actieve perioden voor de prijs-prestaties.
Openbaar beschikbare aanbiedingen in SQL-Database vandaag factuur voor het bedrag van compute, daarentegen, ingerichte op uurbasis. Deze laag voor de ingerichte Computing is prijs-prestaties is geoptimaliseerd voor individuele databases of elastische pools met hogere Gemiddeld gebruik dat geen enkele eventuele vertragingen in compute warmen.

Een database in de serverloze computerlaag is door het compute-bereik dat kan worden gebruikt en een vertraging autopause parameters.

![serverloze facturering](./media/sql-database-serverless/serverless-billing.png)

### <a name="performance"></a>Prestaties

- `MinVcore` en `MaxVcore` configureerbare parameters die het bereik van de rekencapaciteit die beschikbaar zijn voor de database te definiëren. Geheugen en i/o-limieten zijn in verhouding met het vCore-bereik dat is opgegeven.  
- De vertraging autopause is een configureerbare parameter waarmee wordt gedefinieerd welke periode die de database inactief zijn moet voordat deze automatisch is onderbroken. De database wordt automatisch hervat wanneer de volgende aanmelding plaatsvindt.

### <a name="pricing"></a>Prijzen

- De totale factuur voor een database zonder servers is de som van de compute-factuur en de factuur voor opslag.
Facturering voor compute is gebaseerd op de hoeveelheid vCores gebruikt en het geheugen dat per seconde wordt gebruikt.
- Aan de minimale rekenresources in rekening gebracht is gebaseerd op min vCores en min-geheugen.
- Terwijl de database is onderbroken, wordt alleen opslag in rekening gebracht.

## <a name="scenarios"></a>Scenario's

Zonder server is geoptimaliseerd voor individuele databases met onregelmatige gebruikspatronen waarvoor het niet erg enige vertraging in compute warmen na gebruik van niet-actieve perioden van prijs-prestatieverhouding. De ingerichte Computing-laag is geoptimaliseerd voor één of gegroepeerde databases met een hogere Gemiddeld gebruik dat geen enkele eventuele vertragingen in compute warmen van prijs-prestatieverhouding.

De volgende tabel vergelijkt serverloze compute-laag met de ingerichte Computing-laag:

||Serverloze rekenervaring|Ingerichte compute|
|---|---|---|
|**Typische gebruiksscenario**|Databases met onregelmatige, onvoorspelbare gebruik afgewisseld met inactieve perioden|Databases of elastische pools met meer normale gebruik|
|**Prestaties management inspanning**|Lagere|Hoger|
|**COMPUTE schalen**|Automatisch|Handmatig|
|**COMPUTE-reactietijd**|Lagere na inactieve perioden|Onmiddellijk|
|**Facturering granulariteit**|Per seconde|Per uur|
|

### <a name="scenarios-well-suited-for-serverless-compute"></a>Scenario's die zeer geschikt voor serverless Computing

- Individuele databases met onregelmatige gebruikspatronen afgewisseld met perioden van inactiviteit kunnen profiteren van prijs besparingen op basis van facturering per seconde voor de hoeveelheid rekenkracht die wordt gebruikt.
- Individuele databases met de vraag naar resources die moeilijk te voorspellen en klanten die liever delegeren compute schaling naar de service.
- Individuele databases in de ingerichte Computing-laag van de prestatieniveaus die vaak worden gewijzigd.

### <a name="scenarios-well-suited-for-provisioned-compute"></a>Scenario's die zeer geschikt voor de ingerichte Computing

- Individuele databases met meer normale en meer ingrijpende compute-gebruik na verloop van tijd.
- Databases die kunnen tolereren prestaties wisselwerking resultaat zijn van meer geheugen bijsnijden frequente of vertraging in autoresuming van de status onderbroken ingeschakeld.
- Meerdere databases met onregelmatige gebruikspatronen die kunnen worden samengevoegd tot één server en elastische pools gebruiken voor betere prijsoptimalisatie.


## <a name="purchasing-model-and-service-tier"></a>Model en de servicelaag aanschaffen

SQL-Database zonder server is momenteel alleen ondersteund in de categorie Algemeen gebruik op 5 generatie hardware in de vcore model kopen.

## <a name="autoscaling"></a>Automatisch schalen

### <a name="scaling-responsiveness"></a>Reactiesnelheid schalen

In het algemeen databases worden uitgevoerd op een computer met onvoldoende capaciteit om te voldoen aan de vraag naar resources zonder onderbreking voor elke hoeveelheid compute aangevraagd binnen de grenzen van de `maxVcores` waarde. Taakverdeling automatisch gebeurt af en toe als de computer kan niet voldoen aan de vraag naar resources binnen een paar minuten. De database blijft online tijdens de taakverdeling, met uitzondering van een korte periode aan het einde van de bewerking wanneer verbindingen worden verwijderd.

### <a name="memory-management"></a>Geheugenbeheer van

Geheugen voor serverloze databases wordt vrijgemaakt meer vaak dan voor de ingerichte databases. Dit gedrag is belangrijk om te beheersen in zonder server. In tegenstelling tot de ingerichte Computing-geheugen van de SQL-cache van een database zonder servers vrijgemaakt, wanneer het CPU- of cache gebruik laag is.

## <a name="autopause-and-autoresume"></a>Autopause en autoresume

### <a name="autopause"></a>Autopause

Autopause wordt geactiveerd als alle volgende voorwaarden waar voor de duur van de vertraging autopause zijn:

- Aantal sessies = 0
- CPU = 0 (voor de werkbelasting voor gebruikers die worden uitgevoerd in de groep gebruikers)

Een optie wordt geboden om uit te schakelen autopause indien gewenst.

### <a name="autoresume"></a>Autoresume

Autoresume wordt geactiveerd als een van de volgende voorwaarden voldaan op elk gewenst moment wordt:

|Functie|Autoresume trigger|
|---|---|
|Verificatie en autorisatie|Aanmelden|
|Detectie van bedreigingen|Threat detection-instellingen in-/ uitschakelen op het niveau van de database of server<br>Threat detection-instellingen op het niveau van de database of server wijzigen|
|Gegevensdetectie en -classificatie|Toevoegen, wijzigen, verwijderen of de gevoeligheidslabels weergeven|
|Controleren|Controle records weergeven.<br>Bij te werken of weergeven van controlebeleid|
|Gegevensmaskering|Toevoegen, wijzigen, verwijderen of gegevensmaskering regels weergeven|
|Transparent Data Encryption|Status weergeven of de status van transparante gegevensversleuteling|
|Gegevensarchief query (prestaties)|Wijzigen of weergeven van instellingen voor query store; automatisch afstemmen|
|Autotuning|Toepassings- en controle van autotuning aanbevelingen, zoals automatisch indexeren|
|Database kopiëren|-Database als kopie maken<br>Exporteren naar een BACPAC-bestand|
|SQL data sync|Synchronisatie tussen hub en lid van de databases die worden uitgevoerd op een configureerbaar schema of handmatig worden uitgevoerd|
|Metagegevens van een bepaalde database wijzigen|Nieuwe database labels toe te voegen<br>Maximale vcores, min vcores, autopause vertraging wijzigen|
|SQL Server Management Studio (SSMS)|Met behulp van SSMS versie 18 en openen van een nieuw queryvenster voor elke database op de server wordt elke database automatisch onderbroken op dezelfde server hervat. Dit probleem treedt niet als met behulp van SSMS versie 17.9.1 met IntelliSense uitgeschakelde.|

### <a name="connectivity"></a>Connectiviteit

Als een serverloze databases wordt onderbroken, wordt de eerste aanmelding de hervatten van de database en retourneren een foutmelding waarin staat dat de database niet beschikbaar is. Zodra de database wordt hervat, moet de aanmelding opnieuw worden uitgevoerd voor het maken van verbinding. Database-clients met de logica voor opnieuw proberen verbinding hoeft niet te worden gewijzigd.

### <a name="latency"></a>Latentie

De latentie autopause of autoresume een serverloze database is algemeen orde van grootte 1 minuut.

### <a name="feature-support"></a>Ondersteuning van functies

De volgende functies ondersteunen geen autopausing en autoresuming. Dat wil zeggen, als een van de volgende functies worden gebruikt, blijft klikt u vervolgens de database online, ongeacht de duur van inactiviteit van de database:

- Geo-replicatie (actieve geo-replicatie en automatische failover-groepen)
- Langetermijnretentie (LTR)
- De synchronisatiedatabase die wordt gebruikt in SQL data sync.


## <a name="on-boarding-into-the-serverless-compute-tier"></a>Bepalen in de serverloze compute-laag

Het maken van een nieuwe database of het verplaatsen van dat een bestaande database in een serverloze compute-laag volgt hetzelfde patroon als het maken van een nieuwe database in compute-laag ingericht en omvat de volgende twee stappen:

1. Geef de naam van de serviceniveaudoelstelling. De volgende tabel worden de beschikbare service-laag en compute-formaten is momenteel beschikbaar in de openbare preview.

   |Servicelaag|COMPUTE-grootte|
   |---|---|
   |Algemeen doel|GP_S_Gen5_1|
   |Algemeen doel|GP_S_Gen5_2|
   |Algemeen doel|GP_S_Gen5_4|

2. (Optioneel) Geef de minimale vCores en autopause vertraging als u wilt wijzigen, hun standaardwaarden. De volgende tabel toont de beschikbare waarden voor deze parameters.

   |Parameter|Opties voor waarde|Standaardwaarde|
   |---|---|---|---|
   |Minimum vCores|Een van de {0,5, 1, 2, 4} niet meer dan max vCores|0,5 vCores|
   |Autopause vertraging|Min: 360 minuten (6 uur)<br>Max.: 10080 minuten (7 dagen)<br>Stappen: 60 minuten<br>Autopause uitschakelen: -1|360 minuten|

> [!NOTE]
> Met behulp van T-SQL te verplaatsen van een bestaande database naar serverloze of te wijzigen van de compute-grootte wordt momenteel niet ondersteund, maar kan worden gedaan via de Azure portal of PowerShell.

### <a name="create-new-database-using-the-azure-portal"></a>Nieuwe database met behulp van de Azure portal maken

Zie [Quickstart: Een individuele database maken in Azure SQL Database met behulp van de Azure-portal](sql-database-single-database-get-started.md).

### <a name="create-new-database-using-powershell"></a>Nieuwe database maken met PowerShell

Het volgende voorbeeld wordt een nieuwe database in de serverloze compute-laag gedefinieerd door de servicedoelstelling GP_S_Gen5_4 met standaardwaarden met de naam voor de min-vCores en autopause vertraging.

Zonder server vereist een nieuwere versie van PowerShell is momenteel in de galerie, dus voeren `Update-Module Az.Sql` om op te halen van de meest recente serverloze ingeschakeld-cmdlets.

```powershell
New-AzSqlDatabase `
  -ResourceGroupName $resourceGroupName `
  -ServerName $serverName `
  -DatabaseName $databaseName `
  -ComputeModel Serverless `
  -Edition GeneralPurpose `
  -ComputeGeneration Gen5 `
  -MinVcore 0.5 `
  -MaxVcore 2 `
  -AutoPauseDelay 720
```

### <a name="move-existing-database-into-the-serverless-compute-tier"></a>Bestaande database verplaatsen naar de serverloze compute-laag

Het volgende voorbeeld wordt een bestaande database voor één van de ingerichte Computing-laag naar de serverloze compute-laag. In dit voorbeeld gebruikt de standaardwaarden voor de vCores min, max vCores en autopause vertraging.

```powershell
Set-AzSqlDatabase
  -ResourceGroupName $resourceGroupName `
  -ServerName $serverName `
  -DatabaseName $databaseName `
  -Edition GeneralPurpose `
  -ComputeModel Serverless `
  -ComputeGeneration Gen5 `
  -MinVcore 1 `
  -MaxVcore 4 `
  -AutoPauseDelay 1440
```

### <a name="move-a-database-out-of-the-serverless-compute-tier"></a>Verplaatsen van een database buiten de serverloze compute-laag

Een database zonder servers kan worden verplaatst naar een laag ingerichte Computing op dezelfde manier als het verplaatsen van een database van de ingerichte Computing in een serverloze compute-laag.

## <a name="modify-serverless-configuration-parameters"></a>Parameters voor de serverloze configuratie wijzigen

### <a name="maximum-vcores"></a>Maximum vCores

Wijzigen van de maximale vCores is uitgevoerd met behulp van de [Set AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabase) opdracht in PowerShell met behulp van de `MaxVcore` argument.

### <a name="minimum-vcores"></a>Minimum vCores

Wijzigen van de maximale vCores is uitgevoerd met behulp van de [Set AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabase) opdracht in PowerShell met behulp van de `MinVcore` argument.

### <a name="autopause-delay"></a>Autopause vertraging

Wijzigen van de maximale vCores is uitgevoerd met behulp van de [Set AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabase) opdracht in PowerShell met behulp van de `AutoPauseDelay` argument.

## <a name="monitor-serverless-database"></a>Monitor zonder Server-database

### <a name="resources-used-and-billed"></a>Resources die worden gebruikt en in rekening gebracht

De resources van een database zonder servers worden ingekapseld door de volgende entiteiten:

#### <a name="app-package"></a>App-pakket

Het app-pakket is met de buitenste meeste resource management grens voor een database, ongeacht of de database zich in een serverloze of ingerichte compute-laag. Het app-pakket bevat de SQL-exemplaar en externe services die samen scope alle gebruikers- en resources die worden gebruikt door een database in SQL-Database. Voorbeelden van externe services zijn de R- en zoeken in volledige tekst. Het SQL-exemplaar bepaalt het totale Resourcegebruik in het algemeen via het app-pakket.

#### <a name="user-resource-pool"></a>Resourcegroep van de gebruiker

De resourcegroep van de gebruiker de binnenste meeste resource management-grens voor een database, ongeacht of de database zich in een serverloze of ingerichte compute-laag is. De resourcegroep van de gebruiker het bereik van CPU- en i/o-voor de werkbelasting voor gebruikers die zijn gegenereerd door een DDL-query's (bijvoorbeeld CREATE, ALTER, enzovoort) en DML-query's (bijvoorbeeld selecteren, invoegen, bijwerken, verwijderen, enzovoort). Deze query's vertegenwoordigen meestal de meest aanzienlijk deel van het gebruik binnen het app-pakket.

### <a name="metrics"></a>Metrische gegevens

|Entiteit|Gegevens|Description|Eenheden|
|---|---|---|---|
|App-pakket|app_cpu_percent|Percentage van de vCores die worden gebruikt door de app ten opzichte van maximale vCores toegestaan voor de app.|Percentage|
|App-pakket|app_cpu_billed|De hoeveelheid rekenkracht in rekening gebracht voor de app tijdens de rapportageperiode. Het aankoopbedrag tijdens deze periode is het product van deze metrische gegevens en de vCore-prijs per eenheid.<br>Waarden van deze metrische gegevens worden bepaald door het samenvoegen van na verloop van tijd gebruikt voor het maximum van CPU en geheugen gebruikt per seconde.<br>Als het aantal gebruikte kleiner dan het minimum aan nodige ingericht zoals ingesteld door de min-vCores en min-geheugen, is en vervolgens het minimum aan nodige ingericht wordt in rekening gebracht.  Als u wilt vergelijken CPU met geheugen voor factureringsdoeleinden, geheugen genormaliseerd in eenheden van vCores door schaling aanpassen van de hoeveelheid geheugen in GB met 3 GB per vCore.|vCore seconden|
|App-pakket|app_memory_percent|Hoeveelheid geheugen die wordt gebruikt door de app ten opzichte van maximale geheugen die is toegestaan voor de app.|Percentage|
|Gebruiker van toepassingen|cpu_percent|Percentage van de vCores die worden gebruikt door de werkbelasting voor gebruikers ten opzichte van maximale vCores toegestaan voor de werkbelasting van de gebruiker.|Percentage|
|Gebruiker van toepassingen|data_IO_percent|Percentage van de gegevens die worden gebruikt door de werkbelasting voor gebruikers ten opzichte van de gegevens IOPS van de maximale IOPS toegestaan voor de werkbelasting van de gebruiker.|Percentage|
|Gebruiker van toepassingen|log_IO_percent|Percentage van logboek MB/s die worden gebruikt door de werkbelasting voor gebruikers ten opzichte van maximale log MB/s is toegestaan voor de werkbelasting van de gebruiker.|Percentage|
|Gebruiker van toepassingen|workers_percent|Percentage van de werknemers die worden gebruikt door de werkbelasting voor gebruikers ten opzichte van maximale werknemers die zijn toegestaan voor de werkbelasting van de gebruiker.|Percentage|
|Gebruiker van toepassingen|sessions_percent|Percentage van de sessies die worden gebruikt door de werkbelasting voor gebruikers ten opzichte van het maximumaantal sessies toegestaan voor de werkbelasting van de gebruiker.|Percentage|
____

> [!NOTE]
> Metrische gegevens in Azure portal zijn beschikbaar in het deelvenster van de database voor één database onder **bewaking**.

### <a name="pause-and-resume-status"></a>Onderbreken en hervatten van status

In de Azure-portal, wordt de status van de database weergegeven in het overzichtsvenster van de server met een lijst met de databases die deze bevat. De status van de database wordt ook weergegeven in het overzichtsvenster voor de database.

Query uitvoeren op het onderbreken en hervatten van de status van een database via de volgende PowerShell-opdracht:

```powershell
Get-AzSqlDatabase `
  -ResourceGroupName $resourcegroupname `
  -ServerName $servername `
  -DatabaseName $databasename `
  | Select -ExpandProperty "Status"
```

## <a name="resource-limits"></a>Bronlimieten

Zie voor de resourcelimieten, [Serverloze compute-laag](sql-database-vCore-resource-limits-single-databases.md#serverless-compute-tier)

## <a name="billing"></a>Billing

De hoeveelheid rekenkracht in rekening gebracht per seconde is het maximum van CPU-gebruik en het geheugen gebruikt per seconde. Als de hoeveelheid CPU gebruikt en gebruikt geheugen kleiner dan het minimum aan nodige ingericht voor elk is, klikt u vervolgens het ingerichte bedrag wordt in rekening gebracht. Als u wilt vergelijken CPU met geheugen voor factureringsdoeleinden, geheugen genormaliseerd in eenheden van vCores door schaling aanpassen van de hoeveelheid geheugen in GB met 3 GB per vCore.

- **Resource in rekening gebracht**: CPU en geheugen
- **Bedrag in rekening gebracht ($)**: de prijs per eenheid vCore * max (min vCores, vCores gebruikt, min memory GB * geheugen van 1/3 GB gebruikt * 1/3) 
- **Factureringsfrequentie**: Per seconde

De hoeveelheid rekenkracht in rekening gebracht, wordt weergegeven met de volgende metrische gegevens:

- **Metrische gegevens**: app_cpu_billed (vCore seconden)
- **Definitie**: max (min vCores, vCores gebruikt, min memory GB * geheugen van 1/3 GB gebruikt * 1/3) *
- **Frequentie Reporting**: Per minuut

> [!NOTE]
> \* Deze hoeveelheid wordt berekend per seconde en meer dan 1 minuut geaggregeerd.

**Voorbeeld**: Houd rekening met een database GP_S_Gen5_4 gedurende een periode van één uur met de volgende syntaxis:

|Tijd (uren: minuten)|app_cpu_billed (vCore seconden)|
|---|---|
|0:01|63|
|0:02|123|
|0:03|95|
|0:04|54|
|0:05|41|
|0:06 - 1:00|1255|
||Totaal: 1631|

Stel de compute-eenheidsprijs $0.2609/vCore/hour is. Klik aan de rekenresources in rekening gebracht voor deze periode van één uur wordt bepaald met behulp van de volgende formule: **$0.2609/vCore/hour *. 1631 vCore seconden * 1 uur/3600 seconden = $0.1232**

## <a name="available-regions"></a>Beschikbare regio's

De serverloze compute-laag is beschikbaar in alle regio's behalve de volgende regio's: Australië-centraal, China-Oost, China-Noord, Frankrijk-Zuid, Duitsland-centraal, Duitsland-Noordoost, West-India, Zuid-Korea, Zuid-Afrika West, UK-Noord, UK-Zuid, UK-West en West-Centraal VS

## <a name="next-steps"></a>Volgende stappen

Zie voor de resourcelimieten, [zonder compute resource categorielimieten](sql-database-vCore-resource-limits-single-databases.md#serverless-compute-tier).