---
title: bestand opnemen
description: bestand opnemen
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: include
ms.date: 04/29/2019
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: d20e266d1331fc15e65b2d119468483ff53a4c06
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/30/2019
ms.locfileid: "64951518"
---
| Resource | Basic | Standard | Premium |
|---|---|---|---|
| Storage<sup>1</sup> | 10 GiB | 100 GiB| 500 GiB |
| Maximale afbeeldingsgrootte van laag | 200 GiB | 200 GiB | 200 GiB |
| ReadOps per minuut<sup>2, 3</sup> | 1000 | 3.000 | 10.000 |
| WriteOps per minuut<sup>2, 4</sup> | 100 | 500 | 2,000 |
| Downloaden van bandbreedte MBps<sup>2</sup> | 30 | 60 | 100 |
| Uploaden van bandbreedte MBps<sup>2</sup> | 10 | 20 | 50 |
| Webhooks | 2 | 10 | 100 |
| Geo-replicatie | N/A | N/A | [Ondersteund][geo-replication] |
| Inhoud vertrouwen (Preview-versie) | N/A | N/A | [Ondersteund][content-trust] |

<sup>1</sup>de opgegeven opslaglimieten zijn het bedrag van *opgenomen* opslag voor elke laag. U betaalt een extra dagelijks tarief dat per GB voor afbeeldingopslag hierboven deze limieten. Zie voor meer informatie het tarief, [prijzen voor Azure Container Registry][pricing].

<sup>2</sup>*ReadOps*, *WriteOps*, en *bandbreedte* minimale schattingen zijn. Azure Container Registry streeft ernaar om prestaties te verbeteren als gebruik vereist.

<sup>3</sup>A [docker pull](https://docs.docker.com/registry/spec/api/#pulling-an-image) wordt omgezet in meerdere leesbewerkingen op basis van het aantal lagen in de afbeelding, plus het manifest ophalen.

<sup>4</sup>A [docker push](https://docs.docker.com/registry/spec/api/#pushing-an-image) wordt omgezet in meerdere schrijfbewerkingen, op basis van het aantal lagen die moet worden gepusht. Een `docker push` bevat *ReadOps* om op te halen van een manifest voor een bestaande installatiekopie.

<!-- LINKS - External -->
[pricing]: https://azure.microsoft.com/pricing/details/container-registry/

<!-- LINKS - Internal -->
[geo-replication]: ../articles/container-registry/container-registry-geo-replication.md
[content-trust]: ../articles/container-registry/container-registry-content-trust.md
