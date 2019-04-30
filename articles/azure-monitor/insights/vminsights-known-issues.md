---
title: Azure Monitor voor bekende problemen-machines (preview) | Microsoft Docs
description: In dit artikel bevat informatie over bekende problemen met Azure Monitor voor virtuele machines, een oplossing in Azure en combineert status, toepassing afhankelijkheid detectie en bewaking van toepassingsprestaties van het besturingssysteem van de virtuele machine van Azure.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/02/2019
ms.author: magoedte
ms.openlocfilehash: 5fdee4edce0301a5a4524725879f599b0a2e3532
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "61386375"
---
# <a name="known-issues-with-azure-monitor-for-vms-preview"></a>Bekende problemen met Azure Monitor voor virtuele machines (preview)

In dit artikel bevat informatie over bekende problemen met Azure Monitor voor virtuele machines, een oplossing in Azure en combineert health, detectie van toepassingsonderdelen en bewaking van toepassingsprestaties van het besturingssysteem van de virtuele machine van Azure. 

## <a name="health"></a>Status 
De volgende volgen bekende problemen met de huidige release van de Health-functie:

- Het deelvenster van de eigenschap VM weergegeven 2019 van Windows Server-besturingssysteem als Windows Server 2016. Dit wordt opgelost in een toekomstige release.
- Als een Azure-VM is verwijderd of wordt gewist, wordt deze weergegeven in de VM-lijstweergave voor enige tijd opnieuw uit. Bovendien op de status van een virtuele machine verwijderd of verwijderde klikt, wordt de **diagnostische gegevens over gezondheid** weergeven en start vervolgens een lus laden. Als u de naam van de verwijderde virtuele machine selecteert, wordt een deelvenster geopend met een bericht weergegeven dat de virtuele machine is verwijderd.
- Wijzigingen in de configuratie, zoals het bijwerken van een drempelwaarde, wordt er maximaal 30 minuten duren, zelfs als de portal of Workload Monitor API kan ze onmiddellijk bijwerken. 
- De status van diagnostische gegevens over ervaren updates sneller dan de andere weergaven. De gegevens kan zijn vertraagd wanneer u tussen deze twee schakelt. 
- Voor virtuele Linux-machines heeft de titel van de pagina weergegeven van de health-criteria voor één VM-weergave met de volledige domeinnaam van de virtuele machine in plaats van de naam van de gebruiker gedefinieerde VM. 
- Nadat u bewaking voor een virtuele machine met een van de ondersteunde methoden uitschakelen en u probeert u deze opnieuw implementeert, moet u deze implementeren in dezelfde werkruimte. Als u ervoor kiest een andere werkruimte en probeer om de status voor deze virtuele machine weer te geven, kan deze inconsistent gedrag weergegeven.
- Totale CPU-gebruik status criterium voor Windows bevat een drempelwaarde voor *niet gelijk zijn aan* **4**, betekenis CPU-gebruik is groter dan 95% en systeemwachtrijlengte groter is dan 15. Dit criterium status kan niet worden geconfigureerd in deze Preview-versie.  
- Na het verwijderen van de onderdelen van de oplossing uit uw werkruimte, kunt u doorgaan met de status van uw Azure-VM's, Zie specifiek prestaties en de kaart gegevens wanneer u gaat u naar de weergave in de portal. Gegevens wordt uiteindelijk niet meer weergegeven in de weergave prestaties en de kaart na enige tijd; de statusweergave blijven echter wel om de integriteitsstatus van de voor uw VM's weer te geven. De **Probeer nu** optie is beschikbaar voor re-Onboarding van prestaties en kaart-weergaven.

## <a name="next-steps"></a>Volgende stappen
Bekijk voor meer informatie over de vereisten en -methoden voor het inschakelen van bewaking van uw virtuele machines, [implementeert Azure Monitor voor virtuele machines](vminsights-onboard.md).
