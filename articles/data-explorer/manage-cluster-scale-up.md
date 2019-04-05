---
title: Een Azure Data Explorer-cluster om te voldoen aan de veranderende vraag opschalen
description: Dit artikel wordt beschreven stappen voor het omhoog schalen en een Azure Data Explorer-cluster op basis van veranderende vraag verkleinen.
author: radennis
ms.author: radennis
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 02/18/2019
ms.openlocfilehash: 565953c8e0c6f9765d5eeb16a9fa18c3e79b8370
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2019
ms.locfileid: "59044926"
---
# <a name="manage-cluster-scale-up-to-accommodate-changing-demand"></a>Cluster omhoog schalen om te voldoen aan veranderende vraag beheren

Formaat van een cluster op de juiste wijze is essentieel dat de prestaties van Azure Data Explorer. Maar vraag op een cluster met absolute nauwkeurigheid kan niet worden voorspeld. Een statische clustergrootte kan leiden tot ondermaats gebruik of overutilization, geen van beide is ideaal.

Een betere aanpak is het *schaal* een cluster, capaciteit en CPU-resources met het wijzigen van de aanvraag toevoegen en verwijderen. Er zijn twee werkstromen voor het schalen van: scale-up en scale-out. In dit artikel laat zien hoe voor het beheren van cluster omhoog.

1. Ga naar het cluster. Onder **instellingen**, selecteer **omhoog schalen**.

    U kunt een lijst met beschikbare SKU's worden weergegeven. Bijvoorbeeld, zijn in de volgende afbeelding, alleen de vier SKU's zijn beschikbaar.

    ![Omhoog schalen](media/manage-cluster-scale-up/scale-up.png)

    SKU's zijn uitgeschakeld omdat ze de huidige SKU of ze zijn niet beschikbaar in de regio waar het cluster zich bevindt.

1. Als uw SKU wijzigen, selecteert u de SKU die u wilt en kies de **Selecteer** knop.

> [!NOTE]
> Het proces voor omhoog schalen kan een paar minuten duren en gedurende die tijd is uw cluster, worden opgeschort. Houd er rekening mee dat omlaag schalen schadelijk voor de clusterprestaties van uw zijn kan.

U hebt nu een bewerking omhoog of omlaag schalen voor uw Azure Data Explorer-cluster klaar. U kunt ook [beheren cluster scale-out](manage-cluster-scale-out.md) dynamisch opschalen van het aantal exemplaren op basis van metrische gegevens die u opgeeft.

Als u hulp nodig hebt bij problemen met een cluster schalen [een ondersteuningsaanvraag](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) in Azure portal.
