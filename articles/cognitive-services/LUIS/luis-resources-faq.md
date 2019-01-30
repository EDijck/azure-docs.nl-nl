---
title: Veelgestelde vragen
titleSuffix: Azure Cognitive Services
description: In dit artikel bevat antwoorden op veelgestelde vragen over Language Understanding (LUIS).
author: diberry
manager: cgronlun
ms.custom: seodec18
services: cognitive-services
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 01/23/2019
ms.author: diberry
ms.openlocfilehash: 704c3c6b4c998526936e7532fcd92c85ccce54e9
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55222181"
---
# <a name="language-understanding-frequently-asked-questions-faq"></a>Language Understanding Frequently Asked Questions (FAQ)

In dit artikel bevat antwoorden op veelgestelde vragen over Language Understanding (LUIS).

## <a name="luis-authoring"></a>LUIS ontwerpen

### <a name="what-are-the-luis-best-practices"></a>Wat zijn de aanbevolen procedures van LUIS?
Beginnen met de [ontwerpen cyclus](luis-concept-app-iteration.md), leest u de [aanbevolen procedures](luis-concept-best-practices.md).

### <a name="what-is-the-best-way-to-start-building-my-app-in-luis"></a>Wat is de beste manier om te beginnen met het samenstellen van mijn app in LUIS?

De beste manier om uw app te bouwen, is via een [incrementele proces](luis-concept-app-iteration.md).

### <a name="what-is-a-good-practice-to-model-the-intents-of-my-app-should-i-create-more-specific-or-more-generic-intents"></a>Wat is een goede gewoonte om het model van de intenties van mijn app? Moet ik specifieker of een meer algemene intents maken?

Kies intents die niet dus algemeen zijn worden overlappende, maar niet zo specifiek die het maakt het moeilijk voor LUIS onderscheid maken tussen vergelijkbare intents. Het maken van discriminative specifieke intents is een van de aanbevolen procedures voor het modelleren van LUIS.

### <a name="is-it-important-to-train-the-none-intent"></a>Is het belangrijk om te trainen de intentie geen?

Ja, is het raadzaam om het trainen van uw **geen** intentie met meer uitingen als u meer labels aan andere intents toevoegen. Een goede verhouding is 1 of 2 labels op **geen** voor elke 10 labels toegevoegd aan een doel. Deze verhouding verhoogt de discriminative kracht van LUIS.

### <a name="how-can-i-correct-spelling-mistakes-in-utterances"></a>Hoe kan ik in het corrigeren van spelfouten in uitingen?

Zie de [Bing spellingcontrole controleren-API-versie 7](luis-tutorial-bing-spellcheck.md) zelfstudie. LUIS afgedwongen limieten opgelegd door Bing spellingcontrole controleren-API-versie 7.

### <a name="how-do-i-edit-my-luis-app-programmatically"></a>Hoe kan ik mijn LUIS-app via een programma bewerken?
Als u wilt uw LUIS-app via een programma bewerken, gebruikt u de [API ontwerpen](https://aka.ms/luis-authoring-apis). Zie [aanroepen LUIS API ontwerpen](./luis-quickstart-node-add-utterance.md) en [een LUIS-App via een programma met behulp van Node.js](./luis-tutorial-node-import-utterances-csv.md) voor voorbeelden van hoe u de API ontwerpen aan te roepen. De API ontwerpen vereist het gebruik van een [ontwerpen sleutel](luis-concept-keys.md#authoring-key) in plaats van een eindpuntsleutel. Programmatische ontwerpen kan maximaal 1.000.000 aanroepen per maand en 5 transacties per seconde. Zie voor meer informatie over de sleutels die u met LUIS gebruikt [sleutels beheren](./luis-concept-keys.md).

### <a name="where-is-the-pattern-feature-that-provided-regular-expression-matching"></a>Waar is de functie patroon die reguliere expressie opgegeven die overeenkomt met?
De vorige **patroonfunctie** momenteel is afgeschaft, vervangen door  **[patronen](luis-concept-patterns.md)**.

### <a name="how-do-i-use-an-entity-to-pull-out-the-correct-data"></a>Hoe gebruik ik een entiteit voor het ophalen van de juiste gegevens?
Zie [entiteiten](luis-concept-entity-types.md) en [gegevensextractie](luis-concept-data-extraction.md).

### <a name="should-variations-of-an-example-utterance-include-punctuation"></a>Moeten variaties van een voorbeeld-utterance leestekens bevatten?
De verschillende variaties als voorbeeld uitingen met de intent toevoegen of het patroon van de voorbeeld-utterance met toevoegen de [syntaxis voor het negeren](luis-concept-patterns.md#pattern-syntax) de leestekens.

### <a name="does-luis-currently-support-cortana"></a>Ondersteunt LUIS momenteel Cortana?

Cortana vooraf gebouwde apps zijn afgeschaft in 2017. Ze worden niet meer ondersteund.

## <a name="luis-endpoint"></a>LUIS-eindpunt

### <a name="my-endpoint-query-returned-unexpected-results-what-should-i-do"></a>Mijn query eindpunt heeft onverwachte resultaten geretourneerd. Wat moet ik doen?

Onverwachte query voorspellingsresultaten zijn gebaseerd op de status van het gepubliceerde model. Om op te lossen het model, u mogelijk nodig hoeven te wijzigen van het model trainen en opnieuw publiceren. 

Bezig met het herstellen van het model begint met [actief leren](luis-how-to-review-endoint-utt.md).

U kunt niet-deterministisch training verwijderen door het bijwerken van de [toepassing versie instellingen API](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/versions-update-application-version-settings) om te kunnen gebruiken alle trainingsgegevens. 

Controleer de [aanbevolen procedures](luis-concept-best-practices.md) voor meer tips. 

### <a name="why-does-luis-add-spaces-to-the-query-around-or-in-the-middle-of-words"></a>Waarom LUIS spaties toevoegen aan de query rond of in het midden woorden
LUIS [basis van woordgrenzen](luis-glossary.md#token) de utterance op basis van de [cultuur](luis-language-support.md#tokenization). Zowel de oorspronkelijke waarde en de waarde van de tokens zijn beschikbaar voor [gegevensextractie](luis-concept-data-extraction.md#tokenized-entity-returned).

### <a name="how-do-i-create-and-assign-a-luis-endpoint-key"></a>Hoe ik maken en toewijzen van een LUIS eindpuntsleutel?
[Maken van de eindpuntsleutel](luis-how-to-azure-subscription.md) in Azure voor uw [service](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/) niveau. [Toewijzen van de sleutel](luis-how-to-azure-subscription.md) op de **[sleutels en eindpunten](luis-how-to-azure-subscription.md)** pagina. Er is geen overeenkomende API voor deze actie. Vervolgens moet u de HTTP-aanvraag naar het eindpunt naar [gebruikt u de nieuwe eindpuntsleutel](luis-concept-keys.md#use-endpoint-key-in-query).

### <a name="how-do-i-interpret-luis-scores"></a>Hoe ik LUIS scores worden geïnterpreteerd?
Uw systeem moet de hoogste score intentie, ongeacht de waarde ervan gebruiken. Bijvoorbeeld, betekent een score lager (minder dan 50%) 0,5 niet noodzakelijkerwijs dat LUIS weinig vertrouwen heeft. Biedt meer training gegevens kunt verhogen de [score](luis-concept-prediction-score.md) van het meest waarschijnlijk doel.

### <a name="why-dont-i-see-my-endpoint-hits-in-my-apps-dashboard"></a>Waarom zie ik geen mijn treffers eindpunt in het Dashboard van mijn app?
De totale eindpunt treffers in uw app Dashboard worden regelmatig bijgewerkt, maar de metrische gegevens die zijn gekoppeld aan uw LUIS-eindpuntsleutel in Azure portal vaker worden bijgewerkt.

Als er geen bijgewerkte eindpunt treffers in het Dashboard, meld u aan bij de Azure-portal en de resource die is gekoppeld aan de eindpuntsleutel LUIS zoeken en openen **metrische gegevens** selecteren de **totaal aantal aanroepen** metrische gegevens. Als de eindpuntsleutel wordt gebruikt voor meer dan één LUIS-app, ziet u de metrische gegevens in Azure portal het cumulatieve aantal aanroepen van alle LUIS-apps die worden gebruikt.

### <a name="is-there-a-powershell-command-to-the-endpoint-quota"></a>Is er een PowerShell-opdracht voor het quotum van het eindpunt?

U kunt een PowerShell-opdracht gebruiken om te zien van het quotum van het eindpunt:

```powershell
Get-AzureRmCognitiveServicesAccountUsage -ResourceGroupName <your-resource-group> -Name <your-resource-name>
``` 

### <a name="my-luis-app-was-working-yesterday-but-today-im-getting-403-errors-i-didnt-change-the-app-how-do-i-fix-it"></a>Mijn app LUIS werkte gisteren, maar vandaag ik krijg 403 fouten. Kan ik niet hebt de app gewijzigd. Hoe herstel ik deze?
Na de [instructies](#how-do-i-create-and-assign-a-luis-endpoint-key) in de volgende veelgestelde vragen over het maken van een LUIS-eindpuntsleutel en deze toewijzen aan de app. Vervolgens moet u de HTTP-aanvraag naar het eindpunt naar [gebruikt u de nieuwe eindpuntsleutel](luis-concept-keys.md#use-endpoint-key-in-query).

### <a name="how-do-i-secure-my-luis-endpoint"></a>Hoe beveilig ik mijn eindpunt LUIS?
Zie [beveiligen van het eindpunt](luis-concept-security.md#securing-the-endpoint).

## <a name="working-within-luis-limits"></a>U werkt binnen de grenzen van LUIS

### <a name="what-is-the-maximum-number-of-intents-and-entities-that-a-luis-app-can-support"></a>Wat is het maximum aantal intenties en entiteiten dat een LUIS-app kan worden ondersteund?
Zie de [grenzen](luis-boundaries.md) verwijzing.

### <a name="i-want-to-build-a-luis-app-with-more-than-the-maximum-number-of-intents-what-should-i-do"></a>Ik wil een LUIS-app ontwikkelen met meer dan het maximum aantal intents. Wat moet ik doen?

Zie [aanbevolen procedures voor intents](luis-concept-intent.md#if-you-need-more-than-the-maximum-number-of-intents).

### <a name="i-want-to-build-an-app-in-luis-with-more-than-the-maximum-number-of-entities-what-should-i-do"></a>Ik wil een app bouwen in LUIS met meer dan het maximum aantal entiteiten. Wat moet ik doen?

Zie [aanbevolen procedures voor entiteiten](luis-concept-entity-types.md#if-you-need-more-than-the-maximum-number-of-entities)

### <a name="what-are-the-limits-on-the-number-and-size-of-phrase-lists"></a>Wat zijn de limieten van het aantal en grootte van woordgroep bevat?
Voor de maximale lengte van een [woordgroepenlijst](./luis-concept-feature.md), Zie de [grenzen](luis-boundaries.md) verwijzing.

### <a name="what-are-the-limits-on-example-utterances"></a>Wat zijn de limieten op voorbeeld uitingen?
Zie de [grenzen](luis-boundaries.md) verwijzing.

## <a name="testing-and-training"></a>Testen en trainen

### <a name="i-see-some-errors-in-the-batch-testing-pane-for-some-of-the-models-in-my-app-how-can-i-address-this-problem"></a>Ik zie een aantal fouten in het deelvenster voor enkele van de modellen in mijn app testen batch. Hoe kan ik dit probleem oplossen?

De fouten erop duiden dat er een discrepantie tussen de labels en de voorspellingen op basis van uw modellen. Om het probleem op te lossen, kunt u een of beide van de volgende taken:
* Om te helpen verbeteren discriminatie tussen intents LUIS, kunt u meer labels toevoegen.
* Toevoegen om te helpen sneller meer LUIS, woordgroep-lijst met functies die domeinspecifieke vocabulaire introduceren.

Zie de [Batch testen](luis-tutorial-batch-testing.md) zelfstudie.

### <a name="when-an-app-is-exported-then-reimported-into-a-new-app-with-a-new-app-id-the-luis-prediction-scores-are-different-why-does-this-happen"></a>Wanneer een app is geëxporteerd en geïmporteerd in een nieuwe app (met een nieuwe app-ID), is de voorspelling LUIS scores zijn verschillend. Waarom gebeurt dit?

Zie [voorspelling verschillen tussen exemplaren van dezelfde app](luis-concept-prediction-score.md#differences-with-predictions).

### <a name="some-utterances-go-to-the-wrong-intent-after-i-made-changes-to-my-app-the-issue-seems-to-disappear-at-random-how-do-i-fix-it"></a>Sommige uitingen gaat u naar de verkeerde bedoelingen nadat ik wijzigingen in mijn app aangebracht. Het probleem lijkt niet meer zichtbaar in willekeurige volgorde. Hoe herstel ik deze? 

Zie [Train met alle gegevens](luis-how-to-train.md#train-with-all-data).

## <a name="app-publishing"></a>App publiceren

### <a name="what-is-the-tenant-id-in-the-add-a-key-to-your-app-window"></a>Wat is de tenant-ID in het venster 'Toevoegen een sleutel aan uw app'?
In Azure vertegenwoordigt een tenant de client of organisatie die is gekoppeld aan een service. Uw tenant-ID niet vinden in Azure portal in de **map-ID** selectievakje in naast selecteren **Azure Active Directory** > **beheren**  >  **Eigenschappen**.

![Tenant-ID in Azure portal](./media/luis-manage-keys/luis-assign-key-tenant-id.png)

<a name="why-are-there-more-subscription-keys-on-my-apps-publish-page-than-i-assigned-to-the-app"></a>
<a name="why-are-there-more-endpoint-keys-on-my-apps-publish-page-than-i-assigned-to-the-app"></a>


### <a name="why-are-there-more-endpoint-keys-assigned-to-my-app-than-i-assigned"></a>Waarom zijn er meer eindpunt sleutels die zijn toegewezen aan mijn app dan ik heb toegewezen?
Elke LUIS-app beschikt over de authoring/starter-sleutel in de lijst met eindpunten gemakkelijker te maken. Deze sleutel kan slechts een paar eindpunt treffers, zodat u LUIS kunt uitproberen.  

Als uw app bestond voordat LUIS is algemeen beschikbaar (GA), worden automatisch LUIS eindpunt sleutels in uw abonnement toegewezen. Dit is gedaan om de migratie van de algemene beschikbaarheid te vereenvoudigen. Geen nieuwe LUIS eindpunt sleutels in Azure portal zijn _niet_ automatisch toegewezen aan LUIS.

## <a name="app-management"></a>App-beheer

### <a name="how-do-i-transfer-ownership-of-a-luis-app"></a>Hoe ik het eigendom overdraagt van een LUIS-app?
Als u wilt een LUIS-app overzetten naar een ander Azure-abonnement, de LUIS-app exporteren en importeren met behulp van een nieuw account. Werk de LUIS-app-ID in de clienttoepassing die wordt aangeroepen. De nieuwe app mogelijk enigszins LUIS scores geretourneerd uit de oorspronkelijke app.

### <a name="how-do-i-download-a-log-of-user-utterances"></a>Hoe kan ik een logboek van de gebruiker uitingen downloaden?
Uw LUIS-app registreert standaard uitingen van gebruikers. Als u wilt een logboek van uitingen die gebruikers naar uw LUIS-app verzenden downloaden, gaat u naar **mijn Apps**, en selecteer de app. Selecteer in de werkbalk contextuele **eindpunt-logboeken exporteren**. Het logboek wordt opgemaakt als een bestand met door komma's gescheiden waarden (CSV).

### <a name="how-can-i-disable-the-logging-of-utterances"></a>Hoe kan ik de registratie van uitingen uitschakelen?
U kunt de registratie van de gebruiker uitingen uitschakelen door in te stellen `log=false` in de eindpunt-URL die uw clienttoepassing maakt gebruik van LUIS query. Echter, uw LUIS-app kunnen voorstellen uitingen of prestaties die gebaseerd op het uitschakelen van logboekregistratie uitgeschakeld [actief leren](luis-concept-review-endpoint-utterances.md#what-is-active-learning). Als u instelt `log=false` vanwege problemen van de privacy van gegevens, kan u een record van deze gebruiker uitingen van LUIS downloaden of die uitingen gebruiken voor het verbeteren van uw app.

Logboekregistratie is de enige opslag van uitingen.

### <a name="why-dont-i-want-all-my-endpoint-utterances-logged"></a>Waarom niet ik wilt dat alle van de eindpunt-uitingen aangemeld?
Als u van het logboek voor de voorspelling analyse gebruikmaakt, test-uitingen in het logboek niet vastlegt.

## <a name="data-management"></a>Gegevensbeheer

### <a name="can-i-delete-data-from-luis"></a>Kan ik gegevens verwijderen van LUIS?

* U kunt altijd voorbeeld uitingen die wordt gebruikt voor het trainen van LUIS verwijderen. Als u een voorbeeld-utterance uit uw LUIS-app verwijdert, wordt verwijderd uit de LUIS-webservice en is niet beschikbaar voor het exporteren.
* U kunt uitingen verwijderen uit de lijst met gebruikers-uitingen die LUIS voorgesteld in de **bekijken eindpunt uitingen** pagina. Uitingen verwijderen uit deze lijst voorkomt dat deze wordt voorgesteld, maar niet verwijderd van Logboeken.
* Als u een account verwijdert, worden alle apps verwijderd, samen met hun voorbeeld uitingen en Logboeken. De gegevens worden bewaard op de servers gedurende 60 dagen voordat deze worden definitief verwijderd.

### <a name="how-does-microsoft-manage-data-i-send-to-luis"></a>Hoe beheert Microsoft gegevens die kan ik naar LUIS verzenden?

De [Trust Center](https://www.microsoft.com/trustcenter) wordt uitgelegd van onze verplichtingen en uw opties voor gegevensbeheer- en toegangsbeheer in Azure-Services.

## <a name="language-and-translation-support"></a>Ondersteuning voor taal en vertaling

### <a name="i-have-an-app-in-one-language-and-want-to-create-a-parallel-app-in-another-language-what-is-the-easiest-way-to-do-so"></a>Ik heb een app in één taal en een parallelle app maken in een andere taal wilt. Wat is de eenvoudigste manier om dit te doen?
1. Exporteer uw app.
2. De gelabelde uitingen in de JSON-bestand van de geëxporteerde app naar de doel-taal vertalen.
3. Mogelijk moet u de namen van de intenties en entiteiten wijzigen of ze laten zoals ze zijn.
4. Ten slotte de app met een LUIS-app in de doeltaal importeren.

## <a name="app-notification"></a>App-melding

### <a name="why-did-i-get-an-email-saying-im-almost-out-of-quota"></a>Waarom krijg ik een e-mailbericht dat ik heb bijna buiten het quotum?
Uw sleutel ontwerpen/starter is alleen toegestaan voor 1000 eindpunt query's per maand. Maak een LUIS-eindpuntsleutel (gratis of betaald) en gebruikt deze sleutel als eindpunt query's. Als u endpoint-query's vanuit een bot of een andere clienttoepassing maakt, moet u de LUIS-eindpuntsleutel er wijzigen.

## <a name="integrating-luis"></a>LUIS integreren

### <a name="where-is-my-luis-app-created-during-the-azure-web-app-bot-subscription-process"></a>Waar wordt mijn LUIS-app gemaakt tijdens de Azure-web-app-bot-abonnement?
Als u een LUIS-sjabloon selecteren en selecteer de **Selecteer** knop in het deelvenster van de sjabloon, het deelvenster aan de linkerkant om op te nemen van het type van de sjabloon wordt gewijzigd en u wordt gevraagd in welke regio de LUIS-sjabloon wilt maken. De web-app-bot proces maken echter een LUIS-abonnement niet.

![LUIS sjabloon web-app-bot regio](./media/luis-faq/web-app-bot-location.png)

### <a name="what-luis-regions-support-bot-framework-speech-priming"></a>In welke regio LUIS ondersteuning voor Bot Framework spraak voorbereiden?
[Spraak voorbereiden](https://docs.microsoft.com/bot-framework/bot-service-manage-speech-priming) wordt alleen ondersteund voor LUIS-apps in het centrale (VS)-exemplaar.

## <a name="api-programming-strategies"></a>Strategieën voor het programmeren van API

### <a name="how-do-i-programmatically-get-the-luis-region-of-a-resource"></a>Hoe ontvang ik via een programma de LUIS-regio van een resource? 

Gebruik het voorbeeld LUIS naar [regio zoeken](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/documentation-samples/find-region) via een programma met C# of Node.Js. 

## <a name="luis-service"></a>LUIS-service

### <a name="is-language-understanding-luis-available-on-premises-or-in-private-cloud"></a>Language Understanding (LUIS) on-premises beschikbaar is of in een privécloud?

Ja, u kunt de LUIS [container](luis-container-howto.md) voor deze scenario's hebt u de benodigde verbindingen het gebruik kan meten. 

### <a name="at-the-build-2018-conference-i-heard-about-a-language-understanding-feature-or-demo-but-i-dont-remember-what-it-was-called"></a>Ik gehoord over een functie voor Language Understanding of een demo maar ik niet meer weet wat het werd aangeroepen op de Build-conferentie 2018?

De volgende functies zijn uitgebracht op de Build 2018-conferentie:

|Name|Inhoud|
|--|--|
|Verbeteringen|[Reguliere expressie](luis-concept-data-extraction.md##regular-expression-entity-data) entiteit en [sleutel woordgroep](luis-concept-data-extraction.md#key-phrase-extraction-entity-data) entiteit
|Patronen|Patronen [concept](luis-concept-patterns.md), [zelfstudie](luis-tutorial-pattern.md), [procedures](luis-how-to-model-intent-pattern.md)<br>[Patterns.Any](luis-concept-entity-types.md) entiteit met inbegrip van concept [expliciete lijst](luis-concept-patterns.md#explicit-lists) voor uitzonderingen<br>[Rollen](luis-concept-roles.md) concept|
|Integraties|[Tekstanalyse](https://docs.microsoft.com/azure/cognitive-services/text-analytics/) integratie van [sentimentanalyse](luis-how-to-publish-app.md#enable-sentiment-analysis)<br>[Spraak](https://docs.microsoft.com/azure/cognitive-services/speech) integratie van spraak voorbereiden in combinatie met [spraak SDK](https://aka.ms/SpeechSDK)|
|Hulpprogramma voor verzending|Onderdeel van [BotBuilder-hulpprogramma's](https://github.com/Microsoft/botbuilder-tools), verzending vanaf de opdrachtregel [hulpprogramma](luis-concept-enterprise.md#when-you-need-to-combine-several-luis-and-qna-maker-apps) meerdere LUIS en QnA Maker apps combineren tot één LUIS-app voor betere intentieherkenning in een Bot

Aanvullende ontwerpen [API-routes](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/authoring-routes.md) zijn opgenomen.

Video's:
* [Azure Friday Build 2018: Cognitive Services - taal (LUIS)](https://channel9.msdn.com/Shows/Azure-Friday/At-Build-2018-Cognitive-Services-Language-LUIS/player)
* [Build 2018 AI Show - wat is er nieuw met Language Understanding Service](https://channel9.msdn.com/Shows/AI-Show/Whats-New-with-Language-Understanding-Service-LUIS/player)
* [Build 2018-sessie - Bot-intelligentie, spraakmogelijkheden en aanbevolen NLU-procedures](https://channel9.msdn.com/events/Build/2018/BRK3208)
* [Build 2018 - LUIS-Updates](https://channel9.msdn.com/events/Build/2018/THR3118/player)

Projecten:
* [Contoso Cafe bot](https://github.com/botbuilderbuild2018/build2018demo) demo - broncode van GitHub

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over LUIS, de volgende bronnen:
* [Stack Overflow vragen voorzien van LUIS](https://stackoverflow.com/questions/tagged/luis)
* [MSDN-Language Understanding Intelligent Services (LUIS)-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=LUIS)
