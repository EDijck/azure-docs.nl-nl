---
title: Aanbevolen procedures voor Azure Service Fabric-toepassing en cluster | Microsoft Docs
description: Aanbevolen procedures voor het beheren van Service Fabric-clusters en toepassingen.
services: service-fabric
documentationcenter: .net
author: peterpogorski
manager: chackdan
editor: ''
ms.assetid: 19ca51e8-69b9-4952-b4b5-4bf04cded217
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/23/2019
ms.author: pepogors
ms.openlocfilehash: a0174a5442de7a10e45e9dc2a2f43f5f401d1a69
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60803164"
---
# <a name="azure-service-fabric-application-and-cluster-best-practices"></a>Aanbevolen procedures voor Azure Service Fabric-toepassing en cluster

Voor het beheren van Azure Service Fabric-toepassingen en -clusters is, zijn er bewerkingen die we dat u uitvoert raden, om te optimaliseren voor de betrouwbaarheid van uw productieomgeving; uitvoeren van bewerkingen die zijn gedefinieerd in dit document en selecteer een van onze [Azure voorbeelden van Service Fabric-Cluster sjablonen](https://github.com/Azure-Samples/service-fabric-cluster-templates) begint met het ontwerpen van uw productie-oplossing of de bestaande sjabloon om deze procedures wijzigen.

## <a name="security"></a>Beveiliging 

* [Best practices voor beveiliging](service-fabric-best-practices-security.md)

## <a name="networking"></a>Netwerken

* [Aanbevolen procedures voor netwerken](service-fabric-best-practices-networking.md)

## <a name="compute-planning-and-scaling"></a>Compute-planning en schalen

* [Aanbevolen procedures voor het schalen van compute](service-fabric-best-practices-capacity-scaling.md)
* [COMPUTE-capaciteitsplanning](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity)

## <a name="infrastructure-as-code"></a>Infrastructure als code

* [Aanbevolen procedures voor het implementeren van infrastructuur als code](service-fabric-best-practices-infrastructure-as-code.md)

## <a name="monitoring-and-diagnostics"></a>Controle en diagnose

* [Aanbevolen procedures voor het clustercontrole en diagnose](service-fabric-best-practices-monitoring.md)

## <a name="checklist"></a>Controlelijst

Nadat u alle van de bovenstaande secties hebt voltooid, zorg ervoor dat u alle van de procedures in de controlelijst voor de gereedheid van de productie hebt geïntegreerd:
* [Azure Service Fabric productie gereedheid controlelijst](https://docs.microsoft.com/azure/service-fabric/service-fabric-production-readiness-checklist)

## <a name="next-steps"></a>Volgende stappen

* Een cluster maken op virtuele machines of op computers met Windows Server: [Service Fabric-cluster maken voor Windows Server](service-fabric-cluster-creation-for-windows-server.md)
* Een cluster maken op VM's of computers waarop Linux wordt uitgevoerd: [Een Linux-cluster maken](service-fabric-cluster-creation-via-portal.md)
* Probleemoplossing: [Service Fabric-gids voor probleemoplossing](https://github.com/Azure/Service-Fabric-Troubleshooting-Guides)