---
title: Configureren van de startpagina van uw toepassing met Azure IoT Central | Microsoft Docs
description: Als een opbouwfunctie voor expressies, informatie over het configureren van de startpagina van uw Azure IoT Central-toepassing.
author: dominicbetts
ms.author: dobett
ms.date: 07/10/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: a03ac0ef66f4ffdce53d0bd2a35839bbe1615d0b
ms.sourcegitcommit: d4f728095cf52b109b3117be9059809c12b69e32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2019
ms.locfileid: "54199082"
---
# <a name="configuring-homepage"></a>Startpagina configureren

De startpagina is de pagina die wordt geladen wanneer gebruikers die toegang tot de toepassing hebben gaat u naar de URL van de toepassing. Als u de "Voorbeeld Contoso" of "Voorbeeld Devkits" Toepassingssjablonen geselecteerd tijdens het maken van uw toepassing, zal uw toepassing zijn vooraf gedefinieerde startpagina's. Als aan de andere kant kunt u de sjabloon van de toepassing 'Aangepaste Application' geselecteerd, wordt uw startpagina niet leeg zijn.

Bijvoorbeeld, als volgt de startpagina voor toepassingen op basis van de sjabloon 'Contoso voorbeeld'. Voor het aanpassen van de startpagina voor uw toepassing, selecteert u eerst **bewerken** in de rechterbovenhoek. 

![Startpagina voor toepassingen op basis van de sjabloon 'Contoso voorbeeld'](media/howto-configure-homepage/image1.png)

Selecteren **bewerken**, de bibliotheek voor het dashboard wordt geopend in een deelvenster aan de linkerkant. Er zijn veel soorten tegels en dashboard primitieven die kunnen worden toegevoegd aan het aanpassen van uw startpagina.

![Bibliotheek voor het dashboard](media/howto-configure-homepage/image2.png)

Bijvoorbeeld, u kunt toevoegen een **instellingen en eigenschappen** tegel om een selectie van de huidige waarden van instellingen en eigenschappen weer te geven. Om dit te doen, selecteert u eerst een **apparaat sjabloon** Selecteer vervolgens een **apparaatexemplaar**. Nadat u dat de tegel geeft een titel en selecteer een **instelling** of een **eigenschap** om weer te geven. In dit geval is geselecteerd **ingesteld temperatuur**. Te klikken op **gedaan** zorgt ervoor dat deze tegel worden weergegeven op de startpagina.

!['Apparaatdetails configureren' formulier met details voor instellingen en eigenschappen](media/howto-configure-homepage/image3.png)

Als een operator de startpagina bekijkt, kunnen ze nu deze tegel waarin de eigenschappen of de instellingen van het apparaat zien:

![Tabblad met de weergegeven instellingen en eigenschappen voor de tegel 'Dashboard'](media/howto-configure-homepage/image4.png)

Experimenteren met de verschillende andere tegeltypen in de bibliotheek om te ontdekken hoe u de startpagina van uw toepassing nog meer kunt aanpassen.

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd hoe u uw Azure IoT Central startpagina configureert, kunt u het volgende doen:

> [!div class="nextstepaction"]
> [Leer hoe u installatiekopieën voorbereidt en uploadt](howto-prepare-images.md)
