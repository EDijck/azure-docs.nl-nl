---
title: Een Azure notification hub maken met behulp van de Azure-portal | Microsoft Docs
description: In deze zelfstudie leert u hoe u een Azure notification hub (meldingenhub) kunt maken met behulp van de Azure-portal.
services: notification-hubs
author: jwargo
manager: patniko
editor: spelluru
ms.service: notification-hubs
ms.workload: mobile
ms.topic: quickstart
ms.date: 02/14/2019
ms.author: jowargo
ms.openlocfilehash: 62e72f27e48f7bf220901f4eb36090f926724a2a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60681947"
---
# <a name="create-an-azure-notification-hub-in-the-azure-portal"></a>Een Azure notification hub maken met behulp van de Azure-portal 
Azure Notification Hubs biedt een eenvoudig te gebruiken en uitgeschaalde push-engine waarmee u vanuit elke back-end (cloud of on-premises) meldingen kunt verzenden naar ieder platform (iOS, Android, Windows, Kindle, Baidu, enzovoort). Zie voor meer informatie over de service [Wat is Azure Notification Hubs?](notification-hubs-push-notification-overview.md).

In deze quickstart maakt u een notification hub in de Azure-portal. De eerste sectie bevat de stappen voor het maken van een Notification Hubs-naamruimte en een hub in die naamruimte. De tweede sectie bevat de stappen voor het maken van een notification hub in een bestaande Notification Hubs-naamruimte. 

## <a name="create-a-namespace-and-a-notification-hub"></a>Een naamruimte en een notification hub maken
In deze sectie maakt u een naamruimte en een hub in die naamruimte. 

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

## <a name="create-a-notification-hub-in-an-existing-namespace"></a>Een notification hub maken in een bestaande naamruimte
In deze sectie maakt u een notification hub in een bestaande naamruimte. 

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **Alle services** in het menu links, zoek **Notification Hub** en selecteer de **ster** (`*`) naast **Notification Hub-naamruimten** om deze toe te voegen aan de sectie **Favorieten** in het menu links. Selecteer **Notification Hub-naamruimten**. 

      ![Azure-portal - Notification Hub-naamruimten selecteren](./media/create-notification-hub-portal/select-notification-hub-namespaces-all-services.png)
3. Op de pagina **Notification Hub-naamruimten** selecteert u uw naamruimte in de lijst. 

      ![Uw naamruimte selecteren in de lijst](./media/create-notification-hub-portal/select-namespace.png)
1. Op de pagina **Notification Hub-naamruimte** selecteert u **Hub toevoegen** op de werkbalk. 

      ![Notification Hub-naamruimten - knop Hub toevoegen](./media/create-notification-hub-portal/add-hub-button.png)
4. Op de pagina **Nieuwe Notification Hub** typt u een naam voor de notification hub en selecteert u **OK**.

      ![Pagina Nieuwe Notification Hub -> Een naam voor uw hub invoeren](./media/create-notification-hub-portal/new-notification-hub-page.png)
4. Selecteer **Meldingen** (belpictogram) boven aan de pagina om de implementatiestatus van de nieuwe hub weer te geven. Selecteer **X** rechtsboven om het meldingenvenster te sluiten. 

      ![Implementatiemelding](./media/create-notification-hub-portal/deployment-notification.png)
5. Vernieuw de webpagina **Notification Hub-naamruimten** om de nieuwe hub weer te geven in de lijst. 

      ![Azure Portal - Meldingen -> Naar de resource gaan](./media/create-notification-hub-portal/new-hub-in-list.png)
6. Selecteer uw **notification hub** om de startpagina van uw notification hub weer te geven. 

      ![Azure Portal - Meldingen -> Naar de resource gaan](./media/create-notification-hub-portal/hub-home-page.png)

## <a name="next-steps"></a>Volgende stappen
In deze quickstart hebt u een notification hub gemaakt. Zie [Een notification hub met PNS-instellingen configureren](configure-notification-hub-portal-pns-settings.md) voor meer informatie over het configureren van de hub met PNS-instellingen (Platform Notification System). 