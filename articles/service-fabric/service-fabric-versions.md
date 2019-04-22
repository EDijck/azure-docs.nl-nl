---
title: Meer informatie over Azure Service Fabric-clusterversies | Microsoft Docs
description: Ondersteunde versies van Azure Service Fabric-cluster
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chakdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: troubleshooting
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 9/24/2018
ms.author: aljo
ms.openlocfilehash: d99000e1682b662f4bceb28096395243c894282f
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59681604"
---
# <a name="supported-service-fabric-versions"></a>Ondersteunde versies van de Service Fabric

Zorg ervoor dat uw cluster altijd wordt uitgevoerd een ondersteunde versie van Service Fabric. Als en wanneer we kondigen wij de release van een nieuwe versie van Service Fabric, betekent dit dat de vorige versie zich na een minimum van 60 dagen vanaf die datum is gemarkeerd voor einde van ondersteuning. De nieuwe releases worden aangekondigd [op de blog van het Service Fabric-team](https://blogs.msdn.microsoft.com/azureservicefabric/).

Raadpleeg de volgende documenten voor meer informatie over het behouden van uw cluster met een ondersteunde versie van Service Fabric.

- [Service Fabric-versie op een Azure-cluster upgraden](service-fabric-cluster-upgrade.md)
- [Service Fabric-versie op een zelfstandige windows server-cluster upgraden](service-fabric-cluster-upgrade-windows-server.md)

## <a name="supported-versions"></a>Ondersteunde versies

De volgende tabel geeft een lijst van de Service Fabric-versies die worden ondersteund en de einddatum van ondersteuning.

| **Service Fabric-runtime in het cluster** | **Rechtstreeks vanuit de clusterversie kunt upgraden** |**Compatibel SDK / NuGet-pakket versies** | **Einde van ondersteuning voor datum** |
| --- | --- |--- | --- |
| Alle cluster versies vóór 5.3.121 | 5.1.158* |Kleiner dan of gelijk zijn aan versie 2.3 |20 januari 2017 |
| 5.3.* | 5.1.158.* |Kleiner dan of gelijk zijn aan versie 2.3 |24 februari 2017 |
| 5.4.* | 5.1.158.* |Kleiner dan of gelijk zijn aan versie 2.4 |Kan 10,2017       |
| 5.5.* | 5.4.164.* |Kleiner dan of gelijk zijn aan versie 2.5 |Augustus 10,2017    |
| 5.6.* | 5.4.164.* |Kleiner dan of gelijk zijn aan versie 2.6 |Oktober 13,2017   |
| 5.7.* | 5.4.164.* |Kleiner dan of gelijk zijn aan versie 2.7 |December 15,2017  |
| 6.0.* | 5.6.205.* |Kleiner dan of gelijk zijn aan versie 2.8 |30,2018 maart     |
| 6.1.* | 5.7.221.* |Kleiner dan of gelijk zijn aan versie 3.0 |Juli 15,2018      |
| 6.2.* | 6.0.232.* |Kleiner dan of gelijk zijn aan versie 3.1 |Oktober 26,2018   |
| 6.3.* | 6.1.480.* |Kleiner dan of gelijk zijn aan versie 3.2 |31,2019 maart  |
| 6.4.* | 6.2.301.* |Kleiner dan of gelijk zijn aan versie 3.3 |Huidige versie en dus geen einddatum |

## <a name="supported-operating-systems"></a>Ondersteunde besturingssystemen

 De volgende tabel bevat de ondersteunde besturingssystemen voor de ondersteunde versies van de Service Fabric.

| **Besturingssysteem** | **Vroegste ondersteunde Service Fabric-versie** |
| --- | --- |
| Windows Server 2012 R2 | Alle versies |
| Windows Server 2016 | Alle versies |
| Windows Server 1709 | 6.0 |
| WindowsServer 1803 | 6.4 |
| Windows Server 1809 | 6.4.654.9590 |
| Windows Server 2019 | 6.4.654.9590 |
| Linux Ubuntu 16.04 | 6.0 |

