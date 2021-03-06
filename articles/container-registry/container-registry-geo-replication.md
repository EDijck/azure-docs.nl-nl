---
title: Geo-replicatie van een Azure-containerregister
description: Aan de slag met het maken en beheren van geo-replicatie van Azure-containerregisters.
services: container-registry
author: stevelas
manager: jeconnoc
ms.service: container-registry
ms.topic: overview
ms.date: 04/10/2018
ms.author: stevelas
ms.openlocfilehash: 2dc314dd1d1e728f03c1d0c660d9339254ddc462
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60868943"
---
# <a name="geo-replication-in-azure-container-registry"></a>Geo-replicatie in Azure Container Registry

Bedrijven die lokale aanwezigheid willen, of een back-up zonder opnieuw opstarten, kunnen services uitvoeren vanuit meerdere Azure-regio's. Het plaatsen van een containerregister in elke regio waarin installatiekopieën worden uitgevoerd, is een best practice die bewerkingen dicht bij het netwerk mogelijk maakt, wat zorgt voor snelle, betrouwbare overdrachten van installatiekopielagen. Met geo-replicatie kan een Azure-containerregister als één register functioneren en meerdere regio's bedienen met regionale registers voor meerdere masters.

Een geo-gerepliceerd register biedt de volgende voordelen:

* Dezelfde register-/installatiekopie-/tagnamen kunnen worden gebruikt in meerdere regio's
* Toegang tot het register dicht bij het netwerk via regionale implementaties
* Geen extra kosten voor uitgaand verkeer, aangezien de installatiekopieën worden opgehaald uit een lokaal, gerepliceerd register in dezelfde regio als uw containerhost
* Beheer van een enkel register voor meerdere regio's

> [!NOTE]
> Als u kopieën van containerinstallaties in meer dan één Azure-containerregister wilt behouden, kunt u met Azure Container Registry ook [installatiekopieën importeren](container-registry-import-images.md). U kunt bijvoorbeeld binnen een DevOps-werkstroom een installatiekopie uit een ontwikkelingsregister naar een productieregister importeren zonder Docker-opdrachten te gebruiken.
>

## <a name="example-use-case"></a>Voorbeeld van een toepassing
Contoso voert een website voor openbare aanwezigheid uit voor de VS, Canada en Europa. Contoso voert AKS-clusters ([Azure Kubernetes Service](/azure/aks/)) uit in US - west, US - oost, Canada - centraal en Europa - west om deze markten met lokale inhoud van dicht bij het netwerk te bedienen. De webtoepassing, geïmplementeerd als een Docker-installatiekopie, gebruikt dezelfde code en installatiekopie in alle regio's. Inhoud, die lokaal is voor een bepaalde regio, wordt opgehaald uit een database, die uniek is ingericht in elke regio. Elke regionale implementatie heeft een unieke configuratie voor resources zoals de lokale database.

Het ontwikkelteam bevindt zich in Seattle, WA en gebruikt het datacentrum US - west.

![Pushen naar meerdere registers](media/container-registry-geo-replication/before-geo-replicate.png)<br />*Pushen naar meerdere registers*

Voordat Contoso de functies voor geo-replicatie ging gebruiken, had het bedrijf een in de VS gebaseerd register in US - west, met een extra register in Europa - west. Het ontwikkelteam pushte installatiekopieën naar twee verschillende registers om deze verschillende regio's te bedienen.

```bash
docker push contoso.azurecr.io/public/products/web:1.2
docker push contosowesteu.azurecr.io/public/products/web:1.2
```
![Ophalen vanuit meerdere registers](media/container-registry-geo-replication/before-geo-replicate-pull.png)<br />*Ophalen vanuit meerdere registers*

Typische uitdagingen van meerdere registers zijn onder meer:

* De clusters US - oost, US - west en Canada - centraal halen allemaal gegevens op uit het register US - west, waarbij extra kosten voor uitgaand verkeer worden gegenereerd omdat al deze externe containerhosts installatiekopieën ophalen vanuit datacenters in US - west.
* Het ontwikkelteam moet installatiekopieën pushen naar registers in US - west en Europa - west.
* Het ontwikkelteam moet elke regionale implementatie configureren en onderhouden met installatiekopienamen die verwijzen naar het lokale register.
* Toegang tot het register moet voor elke regio worden geconfigureerd.

## <a name="benefits-of-geo-replication"></a>Voordelen van geo-replicatie

![Ophalen vanuit een geo-gerepliceerd register](media/container-registry-geo-replication/after-geo-replicate-pull.png)

De functie voor geo-replicatie van Azure Container Registry biedt de volgende voordelen:

* Eén register beheren voor alle regio's: `contoso.azurecr.io`
* Eén configuratie van installatiekopie-implementaties beheren, omdat voor alle regio's dezelfde installatiekopie-URL wordt gebruikt: `contoso.azurecr.io/public/products/web:1.2`
* Pushen naar een enkel register, terwijl ACR de geo-replicatie beheert, met inbegrip van regionale webhooks voor lokale meldingen

## <a name="configure-geo-replication"></a>Geo-replicatie configureren
Het configureren van geo-replicatie is net zo gemakkelijk als regio's aanklikken op een kaart.

De functie voor geo-replicatie is alleen voor [Premium-registers](container-registry-skus.md) beschikbaar. Als uw register nog niet Premium is, kunt u overstappen van Basic en Standard naar Premium in de [Azure-portal](https://portal.azure.com):

![Van SKU wisselen in de Azure-portal](media/container-registry-skus/update-registry-sku.png)

Als u geo-replicatie voor een Premium-register wilt configureren, moet u zich aanmelden bij de Azure-portal via https://portal.azure.com.

Ga naar Azure Container Registry en selecteer **Replicaties**:

![Replicaties in de containerregister-UI van Azure Portal](media/container-registry-geo-replication/registry-services.png)

Er wordt een kaart weergegeven met alle huidige Azure-regio's:

 ![Kaart met regio's in Azure Portal](media/container-registry-geo-replication/registry-geo-map.png)

* Blauwe zeshoeken staan voor huidige replica's
* Groene zeshoeken staan voor mogelijk replicatieregio's
* Grijze zeshoeken staan voor Azure-regio's die nog niet beschikbaar zijn voor replicatie

Als u een replica wilt configureren, selecteert u een groene zeshoek en selecteert u vervolgens **Maken**:

 ![Gebruikersinterface voor het maken van een replicatie in Azure Portal](media/container-registry-geo-replication/create-replication.png)

Als u extra replica's wilt configureren, selecteert u de groene zeshoeken voor andere regio's en klikt u op **Maken**.

ACR begint installatiekopieën te synchroniseren voor de geconfigureerde replica's. Zodra dit is voltooid, geeft de portal *Gereed* weer. De status van de replica in de portal wordt niet automatisch bijgewerkt. Gebruik de vernieuwknop om de bijgewerkte status te bekijken.

## <a name="geo-replication-pricing"></a>Prijzen van geo-replicatie

Geo-replicatie is een functie van de [Premium SKU](container-registry-skus.md) van Azure Container Registry. Wanneer u een register naar de gewenste regio's repliceert, worden er kosten voor het Premium-register voor elke regio gemaakt.

In het voorgaande voorbeeld ging Contoso van twee registers naar één en voegde het bedrijf replica's toe aan US - oost, Canada - centraal en Europa - west. Contoso zou vier keer per maand Premium betalen, zonder extra configuratie of beheer. Elke regio haalt nu installatiekopieën lokaal op, wat zorgt voor betere prestaties en een hogere betrouwbaarheid zonder kosten voor uitgaand netwerkverkeer voor US - west naar Canada en US - oost.

## <a name="summary"></a>Samenvatting

Met geo-replicatie kunt u uw regionale datacenters als één globale cloud beheren. Aangezien installatiekopieën voor vele Azure-services worden gebruikt, kunt u profiteren van een enkel beheervlak terwijl u een snelle en betrouwbare voorziening van lokale installatiekopieën dicht bij het netwerk in stand houdt.

## <a name="next-steps"></a>Volgende stappen

Bekijk de driedelige zelfstudiereeks [Geo-replicatie in Azure Container Registry](container-registry-tutorial-prepare-registry.md). Ontdek hoe u een geo-gerepliceerd register maakt, een container bouwt en het vervolgens met één `docker push`-opdracht implementeert naar meerdere regionale Web App for Containers-instanties.

> [!div class="nextstepaction"]
> [Geo-replicatie in Azure Container Registry](container-registry-tutorial-prepare-registry.md)
