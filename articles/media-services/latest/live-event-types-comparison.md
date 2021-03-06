---
title: Azure Media Services LiveEvent typen | Microsoft Docs
description: In dit artikel bevat een uitgebreide tabel die LiveEvent typen vergelijken.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 03/01/2019
ms.author: juliako
ms.openlocfilehash: 9952a7bbac1eb79de0d3425f839e3bd30196844e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60322281"
---
# <a name="live-event-types-comparison"></a>Vergelijking van de typen Live gebeurtenis

In Azure Media Services, een [Live gebeurtenis](https://docs.microsoft.com/rest/api/media/liveevents) kan bestaan uit een van de twee typen: live codering en Pass Through-query. 

## <a name="types-comparison"></a>Typen vergelijking 

De volgende tabel vergelijkt de functies van de twee typen van de Live gebeurtenis.

| Functie | Pass Through-Live-gebeurtenis | Standaard Live gebeurtenis |
| --- | --- | --- |
| Single-bitrate-invoer is gecodeerd in meerdere bitsnelheden in de cloud |Nee |Ja |
| Maximale beeldschermresolutie voor bijdrage feed |4K (4096 x 2160 op 60 frames per seconde) |1080p (1920 x 1088 op 30 frames per seconde)|
| Aanbevolen maximale lagen in bijdrage feed|Maximaal 12|Een audio|
| Maximum aantal lagen in de uitvoer| Hetzelfde als invoer|Maximaal 6 (Zie onderstaande systeemwaarden)|
| Maximale cumulatieve bandbreedte van de bijdrage van feed|60 Mbps|N/A|
| Maximale bitrate voor één laag van de bijdrage |20 Mbps|20 Mbps|
| Ondersteuning voor meerdere talen audionummers|Ja|Nee|
| Ondersteunde invoer videocodecs |H.264/AVC en H.265/HEVC|H.264/AVC|
| Ondersteunde uitvoer videocodecs|Hetzelfde als invoer|H.264/AVC|
| Ondersteunde diepte van de video bits, invoer en uitvoer|Maximaal 10-bits inclusief Kopregel 10/HLG|8-bit|
| Ondersteunde invoer audio-codecs|AAC-LC, hij AAC v1, v2 HE-AAC|AAC-LC, hij AAC v1, v2 HE-AAC|
| Ondersteunde uitvoer audio-codecs|Hetzelfde als invoer|AAC-LC|
| Maximale beeldschermresolutie van uitvoervideo|Hetzelfde als invoer|720p (op 30 frames per seconde)|
| Invoer-protocollen|RTMP, gefragmenteerde MP4 (Smooth Streaming)|RTMP, gefragmenteerde MP4 (Smooth Streaming)|
| Prijs|Zie de [pagina met prijzen](https://azure.microsoft.com/pricing/details/media-services/) en klik op het tabblad "Live Video"|Zie de [pagina met prijzen](https://azure.microsoft.com/pricing/details/media-services/) en klik op het tabblad "Live Video"|
| Maximale uitvoeringstijd| 24 uur x 365 dagen live lineaire | Maximaal 24 uur|
| Mogelijkheid om door te geven tot en met ingesloten CEA 608/708 bijschriften gegevens|Ja|Ja|
| Ondersteuning voor slates invoegen|Nee|Nee|
| Ondersteuning voor ad via de API-signalering| Nee|Nee|
| Ondersteuning voor ad-signalering via SCTE 35 in-band-berichten|Ja|Ja|
| Mogelijkheid om te herstellen van korte vertragingen in bijdrage feed|Ja|Nee (Live gebeurtenis begint slating na 6 + seconden zonder invoergegevens)|
| Ondersteuning voor niet-uniforme invoer GOPs|Ja|Nee – invoer GOP duur moet opgelost|
| Ondersteuning voor variabele frame tarief invoer|Ja|Nee – moet dat invoer framesnelheid worden opgelost. Kleine variaties zijn toegestaan, bijvoorbeeld tijdens hoge beweging schermen. Maar de feed bijdrage kan de framesnelheid (bijvoorbeeld om 15 frames per seconde) niet verwijderen.|
| Automatisch-signalen van Live gebeurtenis als invoer-kanaal is verbroken|Nee|Na 12 uur, als er geen LiveOutput uitgevoerd|

## <a name="system-presets"></a>Systeemwaarden

Bij gebruik van live codering (livegebeurtenis ingesteld op **Standard**), bepaalt de vooraf ingestelde codering hoe de binnenkomende stream in meerdere bitrates of lagen wordt gecodeerd. Op dit moment de enige toegestane waarde voor de vooraf ingestelde is *Default720p* (standaard).

**Default720p** wordt de video coderen in de volgende 6 lagen.

### <a name="output-video-stream"></a>Uitvoer Video Stream

| BitRate | Breedte | Hoogte | MaxFPS | Profiel | Naam van de uitvoer-Stream |
| --- | --- | --- | --- | --- | --- |
| 3500 |1280 |720 |30 |Hoog |Video_1280x720_3500kbps |
| 2200 |960 |540 |30 |Hoog |Video_960x540_2200kbps |
| 1350 |704 |396 |30 |Hoog |Video_704x396_1350kbps |
| 850 |512 |288 |30 |Hoog |Video_512x288_850kbps |
| 550 |384 |216 |30 |Hoog |Video_384x216_550kbps |
| 200 |340 |192 |30 |Hoog |Video_340x192_200kbps |

> [!NOTE]
> Als u een aangepaste vooraf ingestelde waarde voor live codering wilt gebruiken, neemt u contact op met amshelp@microsoft.com. Geef de gewenste tabel met resoluties en bitrates op. Controleer of er slechts één laag is op 720 p, en maximaal zes lagen.

### <a name="output-audio-stream"></a>Uitvoer Audio Stream

Audio is naar stereo AAC-LC op 128 k, samplefrequentie van 48 kHz gecodeerd.

## <a name="next-steps"></a>Volgende stappen

[Overzicht van live streaming](live-streaming-overview.md)
