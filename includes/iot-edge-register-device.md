---
title: bestand opnemen
description: bestand opnemen
services: iot-edge
author: kgremban
ms.service: iot-edge
ms.topic: include
ms.date: 06/25/2018
ms.author: kgremban
ms.custom: include file
ms.openlocfilehash: bb0b140dd1f42cae1d5d4bb670af8780d66c1f80
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60768630"
---
Maak een apparaat-id voor uw gesimuleerde apparaat, zodat het met uw IoT-hub kan communiceren. Omdat IoT Edge-apparaten zich anders gedragen en anders kunnen worden beheerd dan typische IoT-apparaten, geeft u vanaf het begin aan dat dit een IoT Edge-apparaat is. 

1. Ga in Azure Portal naar uw IoT-hub.
1. Selecteer **IoT Edge** en selecteer vervolgens **IoT Edge-apparaat toevoegen**.

   ![IoT Edge-apparaat toevoegen](./media/iot-edge-register-device/add-device.png)

1. Geef uw gesimuleerde apparaat een unieke apparaat-ID.
1. Selecteer **Opslaan** om uw apparaat toe te voegen.
1. Selecteer het nieuwe apparaat in de lijst met apparaten.
1. Kopieer de waarde voor **Verbindingsreeks: primaire sleutel** en sla deze op. U gebruikt deze waarde voor het configureren van de IoT Edge-runtime in de volgende sectie. 

