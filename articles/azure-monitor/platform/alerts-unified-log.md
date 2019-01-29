---
title: Waarschuwingen in Azure Monitor
description: Trigger e-mailberichten, meldingen, aanroepen websites URL's (webhooks) of automatisering van wanneer de analytische query door u opgegeven voorwaarden wordt voldaan voor Azure-waarschuwingen.
author: msvijayn
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 10/01/2018
ms.author: vinagara
ms.subservice: alerts
ms.openlocfilehash: e568f2adb3ff9310ed92ed19c9543f249cca7658
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/28/2019
ms.locfileid: "55098694"
---
# <a name="log-alerts-in-azure-monitor"></a>Waarschuwingen in Azure Monitor
Dit artikel vindt u details van waarschuwingen zijn een van de typen waarschuwingen die worden ondersteund in de [Azure-waarschuwingen](../../azure-monitor/platform/alerts-overview.md) en gebruikers van Azure-platform voor streaminganalyse gebruiken als basis voor waarschuwingen.

Waarschuwing bestaat uit regels voor zoeken in logboeken die zijn gemaakt voor [Azure Log Analytics](../../azure-monitor/learn/tutorial-viewdata.md) of [Application Insights](../../azure-monitor/app/cloudservices.md#view-azure-diagnostics-events). Zie voor meer informatie over het gebruik ervan, [waarschuwingen maken in Azure](../../azure-monitor/platform/alerts-log.md)

> [!NOTE]
> Populaire logboekgegevens van [Azure Log Analytics](../../azure-monitor/learn/tutorial-viewdata.md) is nu ook beschikbaar op de metrische platform in Azure Monitor. Voor de detailweergave [metrische waarschuwingen voor logboeken](../../azure-monitor/platform/alerts-metric-logs.md)


## <a name="log-search-alert-rule---definition-and-types"></a>Search waarschuwingsregel - definitie en typen

Er worden door Azure Alerts regels gemaakt voor het zoeken in logboeken om met regelmatige intervallen automatisch opgegeven logboekzoekopdrachten uit te voeren.  Als de resultaten van de logboekzoekopdracht aan bepaalde criteria voldoen, wordt een waarschuwingsrecord gemaakt. De regel kan vervolgens automatisch een of meer acties uitvoeren met behulp van [actiegroepen](../../azure-monitor/platform/action-groups.md). [Inzender voor Azure Monitoring](../../azure-monitor/platform/roles-permissions-security.md) rol voor het maken, wijzigen en het bijwerken van waarschuwingen is mogelijk vereist; samen met toegang & query tot uitvoering van de rechten voor de analytics-doelen in waarschuwingsregel of Waarschuwingsquery. Als het maken van de gebruiker heeft geen toegang tot alle analytics doelen in waarschuwingsregel of Waarschuwingsquery - maken van de regel kan mislukken of de waarschuwingsregel wordt uitgevoerd met gedeeltelijke resultaten.

Log search regels zijn gedefinieerd door de volgende gegevens:

- **Meld u Query**.  De query die wordt uitgevoerd telkens als de waarschuwingsregel wordt geactiveerd.  De records die zijn geretourneerd door deze query worden gebruikt om te bepalen of een waarschuwing wordt geactiveerd. Analytics-query kan worden voor een specifieke Log Analytics-werkruimte of Application Insights-app en zelfs overbruggen [meerdere Log Analytics en Application Insights-resources](../../azure-monitor/log-query/cross-workspace-query.md#querying-across-log-analytics-workspaces-and-from-application-insights) mits de gebruiker toegangsrechten op de externe heeft toepassingen. Combinaties en specifieke analytische opdrachten zijn niet compatibel met gebruikt in waarschuwingen; voor meer informatie weergeven, [waarschuwingsquery's zich in Azure Monitor](../../azure-monitor/platform/alerts-log-query.md).

    > [!IMPORTANT]
    > Waarschuwing voor **niet** ondersteuning voor het gebruik van [functies](../log-query/functions.md) om veiligheidsredenen. En de gebruiker moet de volledige analytics-query opgeven en hebben volledige toegang tot & uitvoeringsrechten voor het maken van een waarschuwingsregel met deze.

- **Periode**.  Hiermee geeft u het tijdsbereik voor de query. De query retourneert alleen de records die zijn gemaakt binnen dit tijdsbereik. Periode Hiermee beperkt u de gegevens opgehaald voor de query voor om misbruik te voorkomen en willekeurige opdracht van de tijd heeft (zoals geleden) gebruikt in logboekquery. <br>*Bijvoorbeeld, als de periode is ingesteld op 60 minuten en de query wordt uitgevoerd op 13:15 uur, wordt alleen de records die zijn gemaakt tussen 12:15 uur en 13:15 uur geretourneerd logboekquery uit te voeren. Als de logboekquery gebruikt tijd opdracht zoals geleden nu (7d), de logboekquery wordt uitgevoerd alleen voor gegevens die tussen 12:15 uur en 1:15 PM - als gegevens bestaan alleen de afgelopen 60 minuten. En niet voor zeven dagen aan gegevens die zijn opgegeven in de query voor.*

- **Frequentie**.  Hiermee geeft u op hoe vaak de query moet worden uitgevoerd. Een waarde tussen 5 minuten en 24 uur kan zijn. Moet gelijk zijn aan of kleiner is dan de periode.  Als de waarde groter dan de periode is, riskeert u records worden overgeslagen.<br>*Neem bijvoorbeeld een periode van 30 minuten en een frequentie van 60 minuten.  Als de query wordt uitgevoerd om 1:00, wordt er records tussen 12:30 en 13:00 uur.  De volgende keer dat de query wordt uitgevoerd, is 2:00 wanneer hij records tussen 1:30 en 2:00 terugkeert.  Alle records gemaakt tussen 1:00 uur en 1:30 zouden nooit worden geëvalueerd.*

- **Drempelwaarde**.  De resultaten van zoeken in Logboeken worden geëvalueerd om te bepalen of een waarschuwing moet worden gemaakt.  De drempelwaarde is verschillend voor de verschillende typen waarschuwingsregels zoeken.

Log search regels worden voor [Azure Log Analytics](../../azure-monitor/learn/tutorial-viewdata.md) of [Application Insights](../../azure-monitor/app/cloudservices.md#view-azure-diagnostics-events), zijn twee soorten. Elk van deze typen is beschreven in de volgende secties.

- **[Aantal resultaten](#number-of-results-alert-rules)**. Één waarschuwing gemaakt wanneer het aantal records geretourneerd door de zoeken in Logboeken groter zijn dan een opgegeven getal.
- **[Meting van metrische gegevens](#metric-measurement-alert-rules)**.  Waarschuwing gemaakt voor elk object in de resultaten van zoeken in Logboeken met waarden die groter zijn dan de opgegeven drempelwaarde.

De verschillen tussen waarschuwingsregel typen zijn als volgt.

- *Aantal resultaten* waarschuwingsregels maakt altijd één waarschuwing, even *meting van metrische gegevens* waarschuwingsregel maakt een waarschuwing voor elk object dat de drempelwaarde overschrijdt.
- *Aantal resultaten* waarschuwingsregels genereren een waarschuwing als de drempelwaarde voor één keer is overschreden. *Meting van metrische gegevens* waarschuwingsregels kunnen genereren een waarschuwing als de drempelwaarde wordt overschreden een bepaald aantal keren gedurende een bepaald tijdsinterval.

### <a name="number-of-results-alert-rules"></a>Aantal resultaten waarschuwingsregels

**Aantal resultaten** waarschuwingsregels één waarschuwing maken wanneer het aantal records dat wordt geretourneerd door de zoekopdracht langer zijn dan de opgegeven drempelwaarde. Dit type van de waarschuwingsregel is ideaal voor het werken met gebeurtenissen, zoals Windows-Logboeken, Syslog antwoord van de Web-App en aangepaste logboeken.  U wilt maken van een waarschuwing wanneer een bepaalde fout-gebeurtenis wordt gemaakt, of wanneer er meerdere gebeurtenissen op foutniveau worden gemaakt binnen een bepaalde periode.

**Drempel**: De drempelwaarde voor een aantal resultaten waarschuwingsregels is groter dan of kleiner is dan een bepaalde waarde.  Als het aantal records dat wordt geretourneerd door de zoeken in Logboeken aan deze criteria voldoen, wordt een waarschuwing gemaakt.

Om u te waarschuwen wanneer één gebeurtenis, het aantal resultaten ingesteld op groter dan 0 en controleren op het exemplaar van een gebeurtenis die is gemaakt sinds de laatste keer dat de query is uitgevoerd. Sommige toepassingen kunnen zich aanmelden voor een incidentele fout mag niet per se een waarschuwing geven.  De toepassing kan bijvoorbeeld proberen het proces dat de foutgebeurtenis is gemaakt en slaagt de volgende keer.  In dit geval kunt u niet te maken van de waarschuwing, tenzij er meerdere gebeurtenissen worden gemaakt binnen een bepaalde periode.  

In sommige gevallen kunt u een waarschuwing maken in de afwezigheid van een gebeurtenis.  Bijvoorbeeld, een proces kan zich aanmelden reguliere gebeurtenissen om aan te geven dat deze correct werkt.  Als dit niet een van deze gebeurtenissen zich binnen een bepaalde periode, moet klikt u vervolgens een waarschuwing worden gemaakt.  In dit geval stelt u de drempelwaarde op **minder dan 1**.

#### <a name="example-of-number-of-records-type-log-alert"></a>Voorbeeld van het type waarschuwing aantal Records

U hebt een scenario waarin u wilt weten wanneer uw web-apps biedt een antwoord naar gebruikers met code 500 (dat wil zeggen) interne serverfout. U zou een waarschuwingsregel maken met de volgende details:  

- **Query:** aanvragen | waarbij resultCode == "500"<br>
- **Tijdsperiode:** 30 minuten<br>
- **Waarschuwingsfrequentie:** vijf minuten<br>
- **Drempelwaarde:** Groter dan 0<br>

Vervolgens waarschuwing zou de query uitvoeren om de 5 minuten, 30 minuten van gegevens - om te controleren of voor elke record waarop resultaatcode is 500. Als een dergelijke record wordt gevonden, wordt de waarschuwing wordt geactiveerd en activeert de actie die is geconfigureerd.

### <a name="metric-measurement-alert-rules"></a>Waarschuwingsregels meting van metrische gegevens

- **Meting van metrische gegevens** waarschuwingsregels een waarschuwing maken voor elk object in een query met een waarde die groter is dan een opgegeven drempelwaarde.  Ze hebben de volgende toch duidelijke verschillen van **aantal resultaten** waarschuwingsregels.
- **Statistische functie**: Hiermee bepaalt u de berekening die wordt uitgevoerd en mogelijk een numeriek veld om samen te voegen.  Bijvoorbeeld, **count()** retourneert het aantal records in de query **avg(CounterValue)** retourneert het gemiddelde van het veld CounterValue over het interval. Statistische functie in de query moet zijn met de naam/met de naam: AggregatedValue en geef een numerieke waarde. 
- **Veld groep**: Een record met een geaggregeerde waarde voor elk exemplaar van dit veld is gemaakt en een waarschuwing worden gegenereerd voor elke.  Bijvoorbeeld, als u wilt dat er waarschuwingen gegenereerd voor elke computer, gebruikt u **per Computer**. In het geval er meerdere groepsveld dat is opgegeven in Waarschuwingsquery zijn, kunt u opgeven welk veld moet worden gebruikt om te sorteren resultaten met behulp van de **cumulatieve op** (metricColumn)-parameter

    > [!NOTE]
    > *Cumulatieve op* (metricColumn)-optie is beschikbaar voor waarschuwingen voor type meting van metrische gegevens voor Application Insights en log waarschuwingen voor [Log Analytics geconfigureerd met behulp van scheduledQueryRules API](alerts-log-api-switch.md) alleen.

- **Interval**:  Hiermee definieert u het tijdsinterval op waarover de gegevens worden samengevoegd.  Bijvoorbeeld, als u hebt opgegeven **vijf minuten**, een record voor elk exemplaar van het groepsveld samengevoegd met intervallen van 5 minuten gedurende de periode die is opgegeven voor de waarschuwing moet worden gemaakt.

    > [!NOTE]
    > BIn-functie moet worden gebruikt in query opgeven voor interval. Als bin() tot ongelijk tijdsintervallen leiden kan - wordt waarschuwing automatisch geconverteerd bin opdracht bin_at opdracht met de juiste tijd tijdens runtime, om ervoor te zorgen resultaten met een vast punt. Het type waarschuwing voor een meting van metrische gegevens is ontworpen voor gebruik met query's met maximaal drie exemplaren van de opdracht bin()
    
- **Drempel**: De drempelwaarde voor de meting van metrische gegevens waarschuwingsregels is gedefinieerd door een cumulatieve waarde en een aantal schendingen.  Als een gegevenspunt in de logboekzoekopdracht deze waarde overschrijdt, heeft deze beschouwd als een schending.  Als het aantal schendingen in voor een object in de resultaten de opgegeven waarde overschrijdt, wordt een waarschuwing gemaakt voor dat object.

Onjuiste configuratie van de *cumulatieve op* of *metricColumn* optie waarschuwingsregels naar ontstekingsfouten kan veroorzaken. Zie voor meer informatie, [problemen oplossen wanneer de waarschuwingsregel meting van metrische gegevens onjuist is](alert-log-troubleshoot.md#metric-measurement-alert-rule-is-incorrect).

#### <a name="example-of-metric-measurement-type-log-alert"></a>Voorbeeld van een waarschuwing voor een type meting van metrische gegevens

U hebt een scenario waarin u een waarschuwing wilt als een computer processorgebruik van 90% drie keer meer dan 30 minuten overschreden.  U zou een waarschuwingsregel maken met de volgende details:  

- **Query:** Perf | waarbij ObjectName == 'Processor' en CounterName == "% processortijd" | summarize AggregatedValue = avg(CounterValue) door bin (TimeGenerated, 5 min.), Computer<br>
- **Tijdsperiode:** 30 minuten<br>
- **Waarschuwingsfrequentie:** vijf minuten<br>
- **Cumulatieve waarde:** Groter is dan 90<br>
- **De waarschuwing activeren op basis van:** Totaal aantal kiezen oplossingen groter is dan 2<br>

De query maakt een gemiddelde waarde voor elke computer met 5 minuten durende intervallen.  Deze query worden uitgevoerd om de 5 minuten voor gegevens die worden verzameld in de vorige 30 minuten.  Hieronder ziet u voorbeeldgegevens voor de drie computers.

![De resultaten van de voorbeeld-query](media/alerts-unified-log/metrics-measurement-sample-graph.png)

In dit voorbeeld zou er afzonderlijke waarschuwingen voor srv02 en srv03 worden gemaakt omdat ze de drempelwaarde 90% drie keer gedurende de periode geschonden.  Als de **waarschuwing activeren op basis van:** zijn gewijzigd in **opeenvolgend** en vervolgens een waarschuwing alleen voor srv03 zouden worden gemaakt omdat deze de drempelwaarde voor drie opeenvolgende steekproeven geschonden.

## <a name="log-search-alert-rule---firing-and-state"></a>Search waarschuwingsregel - starten en de status

Waarschuwingsregel zoeken werkt op de logica echter door de gebruiker aan de hand van configuratie en de aangepaste analytics-query die wordt gebruikt. Sinds de logica van de exacte voorwaarde of de reden waarom de waarschuwingsregel moet trigger ingekapseld in een Analytics-query - die in elke waarschuwingsregel kan verschillen. Azure-waarschuwingen heeft schaarse informatie van de specifieke onderliggende hoofdoorzaak in de resultaten van het logboek wanneer de voorwaarde drempelwaarde van waarschuwingsregel zoeken is bereikt of overschreden. Dus logboekwaarschuwingen tot als status zonder worden aangeduid en wordt geactiveerd telkens wanneer het zoekresultaat log is voldoende om te groter zijn dan de drempelwaarde die is opgegeven in de waarschuwingen van *aantal resultaten* of *meting van metrische gegevens* type voorwaarde. Waarschuwingsregels wordt voortdurend blijven cursorparameter, als voorwaarde voor de waarschuwing wordt voldaan door het resultaat van het aangepaste analytische query geleverd; zonder de waarschuwing elke ophalen is opgelost. Als de logica van de exacte oorzaak van de bewaking van de fout wordt gemaskeerd binnen de analytics-query die is opgegeven door de gebruiker. Er is geen methode met welke Azure-waarschuwingen naar afdoende deduceren of log zoekresultaat voldoen niet aan de drempel geeft aan de resolutie van het probleem dat.

Nu wordt ervan uitgegaan dat er een waarschuwingsregel met de naam *Contoso-Log-waarschuwing*, zoals per configuratie in de [voorbeeld dat is opgegeven voor het nummer van de resultaten type waarschuwing](#example-of-number-of-records-type-log-alert). 
- Op 1:05 uur als Contoso-Log-waarschuwing is uitgevoerd door Azure-waarschuwingen, het zoekresultaat log i/o 0 records. onder de drempelwaarde is en kan daarom niet de waarschuwing wordt geactiveerd. 
- Op de volgende iteratie om 1:10 uur als Contoso-Log-waarschuwing is uitgevoerd door Azure-waarschuwingen opgegeven log zoekresultaat 5 records. de drempelwaarde overschrijden en hierdoor wordt de waarschuwing door te activeren snel na de [actiegroep](../../azure-monitor/platform/action-groups.md) die zijn gekoppeld. 
- Op 13:15 uur als Contoso-Log-waarschuwing is uitgevoerd door Azure-waarschuwingen, mits log zoekresultaat 2 records. de drempelwaarde overschrijden en hierdoor wordt de waarschuwing door te activeren snel na de [actiegroep](../../azure-monitor/platform/action-groups.md) die zijn gekoppeld.
- Op de volgende iteratie om 1:20 uur als Contoso-Log-waarschuwing is uitgevoerd door Azure waarschuwing nu opgegeven log zoekresultaat opnieuw 0 records. onder de drempelwaarde is en kan daarom niet de waarschuwing wordt geactiveerd.

Maar in de hierboven vermelde geval op 13:15 uur - Azure-waarschuwingen kunnen niet bepalen dat de onderliggende problemen zien bij 1:10 zich blijven voordoen en als er netto nieuwe fouten; Als door gebruiker opgegeven query kan worden rekening houdend met oudere records - kunnen Azure-waarschuwingen moet zijn. Daarom aan de fout aan de kant van de waarschuwing, wanneer de Contoso-Log-waarschuwing wordt uitgevoerd op 13:15 uur, geconfigureerd [actiegroep](../../azure-monitor/platform/action-groups.md) opnieuw wordt geactiveerd. Nu om 1:20 uur wanneer er zijn geen records worden gezien - Azure-waarschuwingen kunnen niet zeker weet dat is de oorzaak van de records opgelost; Contoso-Log-waarschuwing wordt daarom niet worden gewijzigd in opgelost in een waarschuwing voor Azure-dashboard en/of meldingen verzonden met vermelding van de resolutie van de waarschuwing.


## <a name="pricing-and-billing-of-log-alerts"></a>Prijzen en facturering van waarschuwingen

Prijzen voor Logboekwaarschuwingen wordt vermeld op de [prijzen voor Azure Monitor](https://azure.microsoft.com/pricing/details/monitor/) pagina. In de Azure-facturen, waarschuwingen worden weergegeven als type `microsoft.insights/scheduledqueryrules` met:

- Waarschuwingen voor Application Insights weergegeven met de exacte naam van waarschuwing, samen met de resourcegroep en de eigenschappen van de waarschuwing
- Waarschuwingen in Log Analytics weergegeven met de exacte naam van waarschuwing, samen met de resourcegroep en de eigenschappen van de waarschuwing; Wanneer gemaakt met behulp van [scheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) 
- Meld u waarschuwingen in Log Analytics weergegeven met de naam van de waarschuwing als `<WorkspaceName>|<savedSearchId>|<scheduleId>|<ActionId>` samen met de resourcegroep en de eigenschappen van de waarschuwing, als het maken via is [verouderde Log Analytics-API](api-alerts.md) of het gebruik van Azure portal **zonder** vrijwillig overschakelen naar de nieuwe API

    > [!NOTE]
    > Als ongeldige tekens zoals `<, >, %, &, \, ?, /` aanwezig zijn, wordt deze vervangen door een `_` in de factuur. Verwijderen van scheduleQueryRules resources gemaakt voor facturering van regels voor waarschuwingen met behulp van [verouderde Log Analytics-API](api-alerts.md) -gebruiker moet het verwijderen van de oorspronkelijke planning en het gebruik van de actie bij waarschuwing [verouderde Log Analytics-API](api-alerts.md)

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [maken in de waarschuwingen in Azure](../../azure-monitor/platform/alerts-log.md).
* Inzicht in [webhooks in waarschuwingen in Azure](alerts-log-webhook.md).
* Meer informatie over [Azure-waarschuwingen](../../azure-monitor/platform/alerts-overview.md).
* Meer informatie over [Application Insights](../../azure-monitor/app/analytics.md).
* Meer informatie over [Log Analytics](../../azure-monitor/log-query/log-query-overview.md).    

