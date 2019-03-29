---
title: Overzicht van Azure Monitor voor containers | Microsoft Docs
description: Dit artikel wordt beschreven van Azure Monitor voor containers die bewaakt AKS Container Insights-oplossing en de waarde die het biedt een door de bewaking van de status van uw AKS-clusters en exemplaren van de Container in Azure.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/27/2019
ms.author: magoedte
ms.openlocfilehash: a31380c8581503a340c55c374afc02c6e1fa290b
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2019
ms.locfileid: "58577166"
---
# <a name="azure-monitor-for-containers-overview"></a>Azure Monitor voor containers: overzicht

Azure Monitor voor containers is een functie die is ontworpen voor het bewaken van de prestaties van containerworkloads die zijn geïmplementeerd in Azure Container Instances- of beheerde Kubernetes-clusters die worden gehost in Azure Kubernetes Service (AKS). Controle van uw containers is kritiek, met name wanneer u een productiecluster op schaal, met meerdere toepassingen uitvoert.

Azure Monitor voor containers biedt u de zichtbaarheid van de prestaties door verzamelen geheugen en processors metrische gegevens van domeincontrollers, knooppunten en containers die beschikbaar in Kubernetes via de API voor metrische gegevens zijn. Er worden ook containerlogboeken verzameld.  Nadat u de bewaking van Kubernetes-clusters hebt ingeschakeld, deze metrische gegevens en logboeken worden automatisch verzameld voor u via een beperkte versie van de Log Analytics-agent voor Linux en opgeslagen in uw [Log Analytics](../log-query/log-query-overview.md) werkruimte. 
 
## <a name="what-does-azure-monitor-for-containers-provide"></a>Wat kost Azure Monitor voor containers bieden?

Azure Monitor voor containers bevat verschillende vooraf gedefinieerde weergaven die de die containerwerkbelastingen en wat van invloed is op de prestatiestatus van het bewaakte Kubernetes-cluster dat u kunt weergeven:  

* Identificeer de AKS-containers die worden uitgevoerd op het knooppunt en het gemiddelde gebruik van de processor en geheugen. Aan de hand van deze kennis kunt u knelpunten in de resource.
* Identificeer de processor en geheugen gebruik van groepen met containers en de containers die worden gehost in Azure Container Instances.  
* Bepalen waar de container bevindt zich in een domeincontroller of een schil. Aan de hand van deze kennis kunt u de algehele prestaties van de van de domeincontroller of de schil weergeven.
* Controleer het brongebruik van workloads die worden uitgevoerd op de host die niet gerelateerd zijn aan de standaard processen die ondersteuning bieden voor de schil.
* Begrijp het gedrag van het cluster bij gemiddelde en zwaarste belasting. Deze kennis kunt u identificeren behoeften aan capaciteit en bepaal de maximale belasting van het cluster kan tolereren. 

U kunt ook waarschuwingen voor het proactief waarschuwen u of leg deze vast bij het gebruik van de CPU en geheugen op knooppunten of containers de drempelwaarden overschrijden configureren.  

## <a name="how-do-i-access-this-feature"></a>Hoe krijg ik toegang tot deze functie?
U kunt toegang tot Azure Monitor voor twee manieren van Azure Monitor of rechtstreeks vanuit het geselecteerde AKS-cluster-containers. Van Azure Monitor die u hebt een globaal overzicht van alle containers geïmplementeerd, die worden bewaakt en die niet zo is, zodat u kunt zoeken en filteren in uw abonnementen en resourcegroepen, en vervolgens omlaag naar Azure Monitor voor containers op de geselecteerde de container.  Anders kunt u gewoon de functie openen rechtstreeks vanuit een geselecteerde AKS-container vanuit de AKS-pagina.  

![Overzicht van methoden voor toegang tot Azure Monitor voor containers](./media/container-insights-overview/azmon-containers-views-1812.png)

Als u geïnteresseerd bent in controle en beheer van uw Docker- en Windows containerhosts configuratie weergeven, controle en gebruik van resources, Zie de [Container Monitoring solution](../../azure-monitor/insights/containers.md).

## <a name="next-steps"></a>Volgende stappen
Om te beginnen met controleren van uw AKS-cluster, Bekijk [hoe zorgen voor onboarding Azure controleren voor containers](container-insights-onboard.md) om te begrijpen van de vereisten en de beschikbare methoden bewaking wilt inschakelen.  
