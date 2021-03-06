---
title: bestand opnemen
description: bestand opnemen
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 09/24/2018
ms.openlocfilehash: 57fd69542a5d92b9afd1e003d8b94c1ebb64953e
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65031769"
---
1. Aanmelden bij de [Azure-portal](https://portal.azure.com/) met behulp van de referenties voor het Azure-abonnement u gebruikt. 

   ![Azure Portal](./media/aml-create-in-portal/portal-dashboard.png)

1. Selecteer in de linkerbovenhoek van de portal **een resource maken**.

   ![Een resource maken in Azure Portal](./media/aml-create-in-portal/portal-create-a-resource.png)

1. Voer in de zoekbalk typt, **Machine Learning**. Selecteer de **werkruimte van Machine Learning-service** zoekresultaat.

   ![Zoeken naar een werkruimte](./media/aml-create-in-portal/allservices-search.png)

1. In de **ML-werkruimte in service** deelvenster, schuif naar beneden en selecteer **maken** om te beginnen.

   ![Maken](./media/aml-create-in-portal/portal-create-button.png)

1. In de **ML-werkruimte in service** in het deelvenster voor het configureren van uw werkruimte.

   Veld|Description
   ---|---
   Naam van de werkruimte |Voer een unieke naam ter identificatie van uw werkruimte. In dit voorbeeld gebruiken we **docs-ws**. Namen moeten uniek zijn in de resourcegroep. Gebruik een naam die gemakkelijk te trekken en onderscheiden van werkruimten die door anderen zijn gemaakt.  
   Abonnement |Selecteer het Azure-abonnement dat u wilt gebruiken.
   Resourcegroep | Een bestaande resourcegroep gebruiken in uw abonnement, of voer een naam in om een nieuwe resourcegroep te maken. Een resourcegroep is een container met gerelateerde resources voor een Azure-oplossing. In dit voorbeeld gebruiken we **docs-aml**. 
   Locatie | Selecteer de locatie die het dichtst bij uw gebruikers en de gegevensresources. Deze locatie is waar de werkruimte is gemaakt.

   ![Werkruimte maken](./media/aml-create-in-portal/workspace-create.png)

1. Selecteer eerst het maakproces **maken**. Het kan even duren om de werkruimte te maken.

1. Als u wilt controleren op de status van de implementatie, selecteer het pictogram meldingen **bell**, op de werkbalk.

1. Wanneer het proces is voltooid, wordt er een bericht voor de implementatie weergegeven. Het is ook aanwezig in de sectie meldingen. Als u de nieuwe werkruimte, selecteert u **naar de resource gaan**.

   ![Status van het maken van werkruimte](./media/aml-create-in-portal/notifications.png)
