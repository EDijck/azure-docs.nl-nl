---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
origin.date: 09/24/2018
ms.date: 04/01/2019
ms.author: v-biyu
ms.openlocfilehash: 31482cc7850c574b952c454021af729da324ba15
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60769937"
---
<!-- N.B. no header, no intents here, language-agnostic -->

De Cognitive Services [spraak SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) biedt de eenvoudigste manier om het gebruik van **spraak naar tekst** in uw toepassing met volledige functionaliteit.

1. Een spraak-configuratie maken en geef een abonnementssleutel voor spraak-service (of een verificatietoken) en een [regio](~/articles/cognitive-services/speech-service/regions.md) als parameters. Indien nodig, kunt u de configuratie wijzigen. Bijvoorbeeld, bieden een aangepast eindpunt om op te geven van een niet-standaard service-eindpunt of Selecteer de gesproken taal of de indeling van uitvoer.

1. Maak een spraakherkenning van de configuratie van spraak. Geef een audio-configuratie als u wilt dat kent van een andere bron dan de standaard-microfoon (bijvoorbeeld audiostream of audio-bestand).

1. De gebeurtenissen voor asynchrone bewerking bezighouden indien gewenst. De gebeurtenis-handlers de herkenning wordt aangeroepen wanneer er een tijdelijke en laatste resultaten. Anders ontvangt uw toepassing alleen een resultaat van de definitieve transcriptie.

1. Opname starten. Voor één spraakherkenning, zoals opdracht of een query spraakherkenning, gebruikt u de `RecognizeOnceAsync()` (of gelijkwaardige taal) methode. Deze methode retourneert de eerste herkende utterance. Voor langlopende spraakherkenning, zoals transcriptie, gebruikt u de `StartContinuousRecognitionAsync()` (of gelijkwaardige taal) methode. De gebeurtenissen voor asynchrone herkenningsresultaten bezighouden.

Zie de volgende codefragmenten voor spraak opname-scenario's die gebruikmaken van de spraak-SDK.

[!INCLUDE [Get a subscription key](cognitive-services-speech-service-get-subscription-key.md)]
