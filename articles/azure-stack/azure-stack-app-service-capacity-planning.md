---
title: Capaciteitsplanning voor Azure App Service-server-rollen in Azure Stack | Microsoft Docs
description: Capaciteitsplanning voor Azure App Service-server-rollen in Azure Stack
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/11/2019
ms.author: jeffgilb
ms.reviewer: anwestg
ms.lastreviewed: 10/15/2018
ms.openlocfilehash: 2c726675d799a8bb5f9ed1d1dd595aa7f4700036
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57774589"
---
# <a name="capacity-planning-for-azure-app-service-server-roles-in-azure-stack"></a>Capaciteitsplanning voor Azure App Service-server-rollen in Azure Stack

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Als u een gereed productie-implementatie van Azure App Service in Azure Stack instelt, moet u van plan bent voor de capaciteit die u verwacht dat het systeem om te ondersteunen.  

In dit artikel bevat richtlijnen voor het minimum aantal rekenprocessen en rekencapaciteit SKU's die u voor een productie-implementatie gebruiken moet.

U kunt met behulp van deze richtlijnen strategie voor de capaciteit van uw App Service plannen.

| App Service-serverrol | Minimaal aanbevolen aantal exemplaren | Aanbevolen compute SKU|
| --- | --- | --- |
| Controller | 2 | A1 |
| Front-end | 2 | A1 |
| Beheer | 2 | A3 |
| Uitgever | 2 | A1 |
| Webwerkrollen - gedeeld | 2 | A1 |
| Webwerkrollen - toegewezen | 2 per laag | A1 |

## <a name="controller-role"></a>De functie van domeincontroller

**Aanbevolen minimum**: Twee exemplaren van Standard A1

De Azure App Service-controller ervaringen doorgaans laag verbruik van CPU, geheugen- en netwerkbronnen. Voor hoge beschikbaarheid, moet u echter twee controllers hebben. Twee controllers zijn ook het maximum aantal domeincontrollers dat is toegestaan. Kunt u de tweede websites-controller direct van het installatieprogramma tijdens de implementatie.

## <a name="front-end-role"></a>-Front-endrol

**Aanbevolen minimum**: Twee exemplaren van Standard A1

De front-end-routes-aanvragen voor webwerkrollen, afhankelijk van beschikbaarheid van de werknemer web. Voor hoge beschikbaarheid, hebt u meer dan één front-end en u kunt meer dan twee hebben. Voor doeleinden voor capaciteitsplanning, kunt u dat elke core ongeveer 100 aanvragen per seconde kan verwerken.

## <a name="management-role"></a>Beheerrol

**Aanbevolen minimum**: Twee exemplaren van Standard A3

De Azure App Service management-rol is verantwoordelijk voor het App Service Azure Resource Manager en API-eindpunten, portal-extensies (admin, tenant, Functions-portal) en de gegevensservice. De beheerserverrol is meestal alleen ongeveer 4 GB RAM-geheugen in een productieomgeving. Het kan echter hoog CPU-niveaus optreden wanneer u veel beheertaken (zoals het maken van web site) worden uitgevoerd. Voor hoge beschikbaarheid, moet u meer dan één server die is toegewezen aan deze rol hebben en ten minste twee cores per server.

## <a name="publisher-role"></a>Uitgeverrol

**Aanbevolen minimum**: Twee exemplaren van Standard A1

Als veel gebruikers gelijktijdig publiceert, kan de uitgeversrol van de intensief CPU-gebruik optreden. Voor hoge beschikbaarheid, zorgt u ervoor dat meer dan één uitgeversrol beschikbaar. FTP-/ FTPS-verkeer wordt alleen verwerkt door de uitgever.

## <a name="web-worker-role"></a>Web-werkrol

**Aanbevolen minimum**: Twee exemplaren van Standard A1

Voor hoge beschikbaarheid, moet u ten minste vier webwerkrollen, twee voor de modus gedeelde web site en twee voor elke specifieke werkrol-laag die u van plan bent te bieden hebben. De gedeelde en toegewijde compute-modi bieden verschillende niveaus van de service aan tenants. U moet mogelijk meer webwerkrollen als veel van uw klanten zijn:

- Met behulp van werkrolniveaus van speciale berekenings-modus (de resource-intensieve).
- Uitgevoerd in de gedeelde rekenmodus wordt ingeschakeld.

Nadat een gebruiker kan een App Service-plan voor een toegewezen rekenmodus wordt ingeschakeld SKU heeft gemaakt, is het aantal web werkrol(len) opgegeven in het App Service-plan niet meer beschikbaar voor gebruikers.

Azure Functions om aan te bieden gebruikers in het plan verbruik model, moet u gedeelde webwerkrollen implementeren.

Bij het bepalen van het aantal gedeelde webwerkrollen wilt gebruiken, Controleer deze overwegingen:

- **Geheugen**: Geheugen is de meest kritieke resource voor een web-werkrol. Er is onvoldoende geheugen heeft invloed op prestaties van de web site wanneer virtueel geheugen van de schijf wordt gewisseld. Elke server vereist ongeveer 1,2 GB aan RAM-geheugen voor het besturingssysteem. RAM-geheugen boven deze drempelwaarde kan worden gebruikt om uit te voeren van websites.
- **Percentage van actieve websites**: Normaal gesproken zijn ongeveer 5 procent van de toepassingen in een Azure App Service op Azure Stack-implementatie actief. Het percentage van de toepassingen die actief op elk gewenst moment zijn kan echter hoger of lager zijn. Het maximum aantal toepassingen te plaatsen in een Azure App Service op Azure Stack-implementatie moet een actieve toepassing snelheid van 5%, minder dan 20 keer het aantal actieve websites (5 x 20 = 100).
- **Het geheugengebruik van gemiddelde**: Het gemiddelde geheugengebruik voor toepassingen die zijn waargenomen in een productieomgeving is bijna 70 MB. Met behulp van deze footprint, het geheugen toegewezen voor alle web worker-rol computers of virtuele machines kan worden als volgt berekend:

   `Number of provisioned applications * 70 MB * 5% - (number of web worker roles * 1044 MB)`

   Bijvoorbeeld, als er 5000 toepassingen op de omgeving die 10 webwerkrollen wordt uitgevoerd, moet elke werkrol web VM 7060 MB RAM-geheugen:

   `5,000 * 70 * 0.05 – (10 * 1044) = 7060 (= about 7 GB)`

   Zie voor meer informatie over het toevoegen van meer werkrolinstanties [toe te voegen meer werkrollen](azure-stack-app-service-add-worker-roles.md).

## <a name="file-server-role"></a>Rol van bestandsserver

Voor de rol van bestandsserver, kunt u een zelfstandige bestandsserver gebruikt voor ontwikkelen en testen; bijvoorbeeld bij het implementeren van Azure App Service op de Azure Stack Development Kit (ASDK) kunt u deze sjabloon: https://aka.ms/appsvconmasdkfstemplate. Voor productiedoeleinden moet u een vooraf geconfigureerde Windows-bestandsserver of een vooraf geconfigureerde niet-Windows-bestandsserver.

De rol van bestandsserver ervaringen in een productieomgeving, intensieve schijf-i/o. Omdat het ook alle inhoud en -toepassingen bestanden voor gebruiker web sites nieuwste, moet u een van de volgende bronnen voor deze rol vooraf configureren:

- Windows-bestandsserver
- Windows-bestandsservercluster
- Niet-Windows-bestandsserver
- Niet-Windows-bestandsservercluster
- NAS (Network Attached Storage)-apparaat

Zie voor meer informatie, [inrichten van een bestandsserver](azure-stack-app-service-before-you-get-started.md#prepare-the-file-server).

## <a name="next-steps"></a>Volgende stappen

Zie het volgende artikel voor meer informatie:

[Voordat u aan de slag met App Service in Azure Stack](azure-stack-app-service-before-you-get-started.md)
