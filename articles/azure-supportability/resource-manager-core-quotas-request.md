---
title: Azure Resource Manager-vCPU-quotum aanvragen verhogen | Microsoft Docs
description: Azure Resource Manager-vCPU-aanvragen voor het verhogen van quota
author: ganganarayanan
ms.author: gangan
ms.date: 6/13/2018
ms.topic: article
ms.service: azure
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: 204deaf3a67984c0dd5eca5352686719e7767885
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60649295"
---
# <a name="resource-manager-vcpu-quota-increase-requests"></a>Resource Manager-vCPU-aanvragen voor het verhogen van quota

Resource Manager-vCPU-quota worden afgedwongen op het regioniveau en de SKU-familie niveau.
Meer informatie over hoe quota worden afgedwongen op de [Azure-abonnement en Servicelimieten](https://aka.ms/quotalimits) pagina.
Voor meer informatie over SKU-Families, kunt u kosten en prestaties vergelijken op de [prijzen van virtuele Machines](https://aka.ms/pricingcompute) pagina.

Volg de instructies hieronder gebruiken voor het maken van een ondersteuningsaanvraag via Azure 'Gebruik + quota' om een verhoging blade beschikbaar in de Azure-Portal. 

## <a name="request-quota-increase-at-subscription-level"></a>Verhoging van quota op abonnementsniveau

1. Van https://portal.azure.com, selecteer **abonnementen**.

   ![Subscriptions](./media/resource-manager-core-quotas-request/subscriptions.png)

2. Selecteer het abonnement waarvoor het quotum moet worden verhoogd.

   ![Abonnement selecteren](./media/resource-manager-core-quotas-request/select-subscription.png)

3. Selecteer **gebruik + quota**

   ![Gebruik en quota's selecteren](./media/resource-manager-core-quotas-request/select-usage-quotas.png)

4. Selecteer in de rechterbovenhoek **verhoging**.

   ![Verhoging aanvragen](./media/resource-manager-core-quotas-request/request-increase.png)

5. Stap 1: Selecteer **kernen** als het type offerte. 

   ![Formulier invullen](./media/resource-manager-core-quotas-request/forms.png)
   
6. Stap: 2: Selecteer implementatie modelleren als 'Resource Manager' en selecteer een locatie.

    ![Quotum probleem blade](./media/resource-manager-core-quotas-request/Problem-step.png)

3. Selecteer de SKU-Families waarvoor een verhoging.

    ![SKU-reeksen geselecteerd](./media/resource-manager-core-quotas-request/SKU-selected.png)

4. Geef de nieuwe limieten die u wilt dat voor het abonnement.

    ![Nieuwe quotumaanvraag-SKU](./media/resource-manager-core-quotas-request/SKU-new-quota.png)

- Als u wilt verwijderen van een regel, schakel het selectievakje van de SKU in de SKU-familie vervolgkeuzelijst of klik op het pictogram 'x' negeren.
Na het invoeren van de gewenste quotum voor elke SKU-familie, klikt u op 'Volgende' op de pagina van het probleem stap om door te gaan met het maken van de aanvraag voor ondersteuning.

