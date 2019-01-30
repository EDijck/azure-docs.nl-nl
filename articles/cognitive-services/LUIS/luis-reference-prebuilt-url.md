---
title: URL van vooraf gemaakte entiteiten
titleSuffix: Azure
description: In dit artikel bevat een url vooraf gedefinieerde entiteitgegevens in Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 11/27/2018
ms.author: diberry
ms.openlocfilehash: f9e331abd31ef37e9214214118748ebda3eb9470
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55225788"
---
# <a name="url-prebuilt-entity-for-a-luis-app"></a>URL van vooraf gedefinieerde entiteit voor een LUIS-app
URL-entiteit extraheert URL's met domeinnamen of IP-adressen. Omdat deze entiteit wordt al getraind, hoeft u niet om toe te voegen van de voorbeeld-uitingen met URL's naar de toepassing. URL-entiteit wordt ondersteund in `en-us` alleen de cultuur. 

## <a name="types-of-urls"></a>Typen van URL 's
URL wordt beheerd via de [kenmerken tekst](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/Base-URL.yaml) GitHub-opslagplaats

## <a name="resolution-for-prebuilt-url-entity"></a>Oplossing voor vooraf gedefinieerde URL-entiteit
Het volgende voorbeeld ziet u de resolutie van de **builtin.url** entiteit.

```json
{
  "query": "http://www.luis.ai is a great cognitive services example of artificial intelligence",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.781975448
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.781975448
    }
  ],
  "entities": [
    {
      "entity": "http://www.luis.ai",
      "type": "builtin.url",
      "startIndex": 0,
      "endIndex": 17
    }
  ]
}
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de [rangtelwoord](luis-reference-prebuilt-ordinal.md), [getal](luis-reference-prebuilt-number.md), en [temperatuur](luis-reference-prebuilt-temperature.md) entiteiten.
