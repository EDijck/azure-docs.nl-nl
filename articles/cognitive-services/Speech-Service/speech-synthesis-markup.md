---
title: Spraak synthese Markup Language (SSML) - spraakservices
titleSuffix: Azure Cognitive Services
description: Met behulp van de spraakherkenning synthese Markup Language voor het beheren van uitspraak en prosody in tekst naar spraak.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/13/2018
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: 57fc7e699d88dbe777750e3acdb7f96794b66fc0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "61460283"
---
# <a name="speech-synthesis-markup-language-ssml"></a>Speech Synthesis Markup Language (SSML)

Spraak synthese Markup Language (SSML) is een op XML gebaseerde opmaaktaal die een manier voor het beheren van de uitspraak biedt en *prosody* van tekst naar spraak. Prosody verwijst naar het ritme en verkopen van spraak: de muziek, als u wordt. U kunt woorden Fonetisch opgeven, hints geven voor de interpretatie van getallen, invoegen wordt onderbroken, inspiratie besturingselement, volume, en snelheid en meer. Zie voor meer informatie, [spraak synthese Markup Language (SSML) versie 1.0](https://www.w3.org/TR/2009/REC-speech-synthesis-20090303/).

Zie voor een volledige lijst van ondersteunde talen, landinstellingen en stemmen (neurale en standaard), [taalondersteuning](language-support.md#text-to-speech).

De volgende secties bevatten voorbeelden voor algemene spraak synthese taken.

## <a name="add-a-break"></a>Einde toevoegen
```xml
<speak version='1.0' xmlns="https://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, Jessa24kRUS)'>
    Welcome to Microsoft Cognitive Services <break time="100ms" /> Text-to-Speech API.
</voice> </speak>
```

## <a name="change-speaking-rate"></a>Snelheid van spreken wijzigen

Tarief spreken kan worden toegepast op standard stemmen in het woord of de zin op serverniveau. Terwijl u spreekt tarief kan alleen worden toegepast op neurale stemmen op het niveau van de zin.

```xml
<speak version='1.0' xmlns="https://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, Guy24kRUS)'>
<prosody rate="+30.00%">
    Welcome to Microsoft Cognitive Services Text-to-Speech API.
</prosody></voice> </speak>
```

## <a name="pronunciation"></a>Uitspraak van
```xml
<speak version='1.0' xmlns="https://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, Jessa24kRUS)'>
    <phoneme alphabet="ipa" ph="t&#x259;mei&#x325;&#x27E;ou&#x325;"> tomato </phoneme>
</voice> </speak>
```

## <a name="change-volume"></a>Volume wijzigen

Volumewijzigingen kunnen worden toegepast op standard stemmen in het woord of de zin op serverniveau. Terwijl het volume worden gewijzigd, kunnen alleen worden toegepast op neurale stemmen op het niveau van de zin.

```xml
<speak version='1.0' xmlns="https://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'>
<prosody volume="+20.00%">
    Welcome to Microsoft Cognitive Services Text-to-Speech API.
</prosody></voice> </speak>
```

## <a name="change-pitch"></a>Breedte wijzigen

Presentatie wijzigingen kunnen worden toegepast op standard stemmen in het woord of de zin op serverniveau. Terwijl de tekenbreedte wijzigingen kunnen alleen worden toegepast op neurale stemmen op het niveau van de zin.

```xml
<speak version='1.0' xmlns="https://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
    <voice  name='Microsoft Server Speech Text to Speech Voice (en-US, Guy24kRUS)'>
    Welcome to <prosody pitch="high">Microsoft Cognitive Services Text-to-Speech API.</prosody>
</voice> </speak>
```

## <a name="change-pitch-contour"></a>Wijziging inspiratie contour

> [!IMPORTANT]
> Tekenbreedte contour wijzigingen worden niet ondersteund met neurale stemmen.

```xml
<speak version='1.0' xmlns="https://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'>
<prosody contour="(80%,+20%) (90%,+30%)" >
    Good morning.
</prosody></voice> </speak>
```

## <a name="use-multiple-voices"></a>Meerdere stemmen
```xml
<speak version='1.0' xmlns="https://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'>
    Good morning!
</voice>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, Guy24kRUS)'>
    Good morning to you too Jessa!
</voice> </speak>
```

## <a name="next-steps"></a>Volgende stappen

* [Taalondersteuning: stemmen, landinstellingen, talen](language-support.md)
