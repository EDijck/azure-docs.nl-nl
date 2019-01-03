---
title: bestand opnemen
description: bestand opnemen
services: iot-hub
author: dominicbetts
ms.service: iot-hub
ms.topic: include
ms.date: 05/17/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: e518263fc7f427ade4943f91fdc840b62a235205
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/17/2018
ms.locfileid: "53550020"
---
### <a name="enable-logging-with-diagnostics-settings"></a>Logboekregistratie inschakelen met de diagnostische instellingen

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) en navigeer naar uw IoT-Hub.

2. Selecteer **diagnostische instellingen**.

3. Selecteer **diagnostische gegevens inschakelen**.

   ![Diagnostische gegevens inschakelen](./media/iot-hub-diagnostics-settings/turnondiagnostics.png)

4. De diagnostische instellingen voor een naam geven.

5. Kies waar u om de logboeken te verzenden. U kunt een willekeurige combinatie van de drie opties selecteren:

   * Archiveren naar een opslagaccount
   * Streamen naar een Event Hub
   * Verzenden naar Log Analytics

6. Kiezen welke bewerkingen u wilt bewaken, en schakel logboeken voor deze bewerkingen. Er zijn de bewerkingen die diagnostische instellingen kunnen worden weergegeven:

   * Verbindingen
   * Telemetrie van apparaten
   * Cloud-naar-apparaat-berichten
   * Bewerkingen voor apparaat-id
   * Uploaden van bestanden
   * Berichtroutering
   * Cloud-naar-apparaat dubbele bewerkingen
   * Dubbele apparaat-naar-cloud-bewerkingen
   * Dubbele bewerkingen
   * Taakbewerkingen
   * Directe methoden  

6. De nieuwe instellingen opslaan. 

Als u inschakelen op de instellingen voor diagnostische gegevens met PowerShell wilt, gebruikt u de volgende code:

```azurepowershell
Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName <subscription that includes your IoT Hub>
Set-AzureRmDiagnosticSetting -ResourceId <your resource Id> -ServiceBusRuleId <your service bus rule Id> -Enabled $true
```

Nieuwe instellingen van kracht in ongeveer 10 minuten. Hierna logboeken worden weergegeven in de geconfigureerde archivering doel op de **diagnostische instellingen** blade. Zie voor meer informatie over het configureren van diagnostische gegevens [verzamelen en gebruiken van logboekgegevens van uw azure-resources](../articles/azure-monitor/platform/diagnostic-logs-overview.md).