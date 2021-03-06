---
title: bestand opnemen
description: bestand opnemen
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.subservice: luis
ms.topic: include
ms.custom: include file
ms.date: 12/21/2018
ms.author: diberry
ms.openlocfilehash: 355fe134939b26c51d6e03368f782845628a6b96
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60597462"
---
De clienttoepassing moet weten of een uiting niet zinvol of gepast is voor de toepassing. De intentie **Geen** wordt als onderdeel van het maakproces aan elke toepassing toegevoegd om te bepalen of een uiting niet kan worden beantwoord door de clienttoepassing.

Als LUIS de intentie **Geen** retourneert voor een uiting, kan de clienttoepassing vragen of de gebruiker het gesprek wil beëindigen of meer aanwijzingen voor het vervolgen van het gesprek wil geven. 

> [!CAUTION] 
> Laat niet de intentie **Geen** niet leeg. 

1. Selecteer **Intents** in het linkerpaneel.

2. Selecteer de intent **None**. Voeg drie uitingen toe die een gebruiker zou kunnen invoeren, maar die niet relevant zijn voor uw HR-app:

    | Voorbeelden van utterances|
    |--|
    |Blaffende honden zijn irritant|
    |Bestel een pizza voor me|
    |Pinguïns in de oceaan|
