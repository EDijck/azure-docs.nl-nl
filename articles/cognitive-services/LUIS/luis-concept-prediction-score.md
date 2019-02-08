---
title: Voorspelling scores
titleSuffix: Language Understanding - Azure Cognitive Services
description: Een voorspelling score geeft aan dat de mate van vertrouwen die LUIS voor voorspellingsresultaten heeft.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: diberry
ms.openlocfilehash: 9386285ae7c8692e16e8c476ad59f119ecb149b6
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55870857"
---
# <a name="prediction-scores-indicate-prediction-accuracy-for-intent-and-entities"></a>Voorspelling scores geven nauwkeurigheid voor de intentie en entiteiten
Een voorspelling score geeft aan dat de mate van vertrouwen die LUIS voor voorspellingsresultaten heeft. 

De score van een voorspelling is tussen de nul (0) en één (1). Een voorbeeld van een zeer vertrouwen LUIS-score is 0,99. Een voorbeeld van een score van lage vertrouwen is 0,01. 

|Score-waarde|Betrouwbaarheid|
|--|--|
|1|goede overeenkomst|
|0,99|hoge betrouwbaarheid|
|0.01|lage vertrouwen|
|0|goede niet overeen met|

Wanneer een utterance in een laag-betrouwbaarheidsscore resulteert, LUIS markeert die in de [LUIS](luis-reference-regions.md) website **kunt u lezen wat** pagina, met de geïdentificeerde **met het label bedoelingen** die worden beschreven met rood. 

![Score discrepantie](./media/luis-concept-score/score-discrepancy.png)

## <a name="top-scoring-intent"></a>Scoring-bovenaan doel
Elke utterance voorspelling retourneert een scoring-bovenaan doel. Dit is een numerieke vergelijking van scores voorspelling. De eerste twee scores kunnen een klein verschil tussen deze twee hebben. LUIS wordt niet aangegeven deze nabijheid dan scores retourneren.  

Als u zich zorgen over de nabijheid van bovenste scores, moet u de score voor alle intents retourneren. U kunt ofwel uitingen toevoegen aan de twee intenties die wijzen op de verschillen met het woord keuze en schema's of u kunt beschikken over de toepassing LUIS-aanroepen, zoals een chatbot, programmatische keuzes over het afhandelen van de twee bovenste intents. 

Twee intents, die ook nauw worden beoordeeld, mogelijk vanwege een niet-deterministische training omkeren. De hoogste score kan de tweede boven en de tweede belangrijkste score kan worden de eerste hoogste score. Toevoegen om te voorkomen dat dit, voorbeeld-uitingen aan elk van de eerste twee intenties voor die utterance word keuze en context die wordt onderscheid gemaakt de twee intents tussen. De twee intenties moeten over hetzelfde aantal voorbeeld uitingen hebben. Vuistregel voor scheiding om te voorkomen dat tekenomkering vanwege training is een verschil 15% in scores.

## <a name="return-prediction-score-for-all-intents"></a>Voorspelling score voor alle intents retourneren
Resultaat van een test- of -eindpunt kan alle intents bevatten. Deze configuratie is ingesteld op de [eindpunt](https://aka.ms/v1-endpoint-api-docs) met de `verbose=true` query uitvoeren op de naam/waarde-paar tekenreeks. 

## <a name="review-intents-with-similar-scores"></a>Intents met vergelijkbare scores bekijken
Controleren van de score voor alle intents is een goede manier om te controleren dat niet alleen de juiste intentie wordt geïdentificeerd, maar die de komende aangegeven kunt u lezen wat de score zijn aanzienlijk lager is consistent voor uitingen. 

Als meerdere intents sluiten voorspelling scores op basis van de context van een utterance kan LUIS schakelen tussen de intents. U kunt dit verhelpen blijven uitingen toevoegen aan elk doel met een breder scala van contextuele verschillen.   

## <a name="e-exponent-notation"></a>De notatie E (exponent)

Voorspelling scores kunt exponent notatie *weergegeven* boven de 0-1 bereik, zoals `9.910309E-07`. Deze score geeft aan dat er een zeer **kleine** getal.

|E-notatie score |Werkelijke score|
|--|--|
|9.910309E-07|.0000009910309|

## <a name="differences-with-predictions"></a>Verschillen met voorspellingen
Wanneer u hetzelfde model in een andere app trainen en de scores niet hetzelfde zijn, is dit omdat er een element van de onzekerheid in de training. Ten tweede betekent een overlapping van een utterance aan meer dan één kunt u lezen wat dat het belangrijkste doel voor de dezelfde utterance kunt wijzigen op basis van de training.

Als uw chatbot een specifieke LUIS score om aan te geven vertrouwen in opzet vereist, moet u in plaats daarvan de score verschil tussen de bovenste twee intents. Dit biedt flexibiliteit voor variaties in Trainingen. 

## <a name="punctuation"></a>Leestekens
Interpunctie is een afzonderlijke token in LUIS. Een utterance met een punt aan het einde ten opzichte van een utterance die niet zijn twee afzonderlijke uitingen en twee verschillende voorspellingen kunnen krijgen. Zorg ervoor dat het model interpunctie ofwel worden verwerkt in de [voorbeeld uitingen](luis-concept-utterance.md) (met en niet met leestekens) of in de [patronen](luis-concept-patterns.md) waar is het eenvoudiger om interpunctie met de syntaxis van de speciale negeren: `I am applying for the {Job} position[.]`

## <a name="next-steps"></a>Volgende stappen

Zie [entiteiten toevoegen](luis-how-to-add-entities.md) voor meer informatie over entiteiten toevoegen aan uw LUIS-app.
