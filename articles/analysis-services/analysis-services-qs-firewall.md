---
title: 'Snelstart: Een firewall configureren voor een Analysis Services-server in Azure | Microsoft Docs'
description: Informatie over het configureren van een firewall voor een instantie van Analysis Services-server in Azure.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: quickstart
ms.date: 01/09/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 198e7d8d32e7142a266881e2f4ddbc3ed573c521
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60533876"
---
# <a name="quickstart-configure-server-firewall---portal"></a>Snelstart: Een serverfirewall configureren - Portal

Deze snelstart helpt u bij het configureren van een firewall voor uw Azure Analysis Services-server. Een firewall inschakelen en het configureren van IP-adresbereiken voor uitsluitend de computers die toegang tot de server hebben, speelt een belangrijke rol bij het beveiligen van uw server en uw gegevens.

## <a name="prerequisites"></a>Vereisten

- Een Azure Analysis Services-server in uw abonnement. Raadpleeg voor meer informatie [Snelstart: Een server maken - Portal](analysis-services-create-server.md) of [Snelstart: Een server maken - PowerShell](analysis-services-create-powershell.md)
- Een of meer IP-adresbereiken voor clientcomputers (indien nodig).

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal 

[Aanmelden bij de portal](https://portal.azure.com)

## <a name="configure-a-firewall"></a>Een firewall configureren

1. Klik op de server om de overzichtspagina te openen. 
2. Klik in **INSTELLINGEN** > **Firewall** > **Firewall inschakelen** op **Aan**.
3. Klik op **Aan** bij **Toegang toestaan vanuit Power BI** om toegang tot DirectQuery toe te staan vanuit Power BI-service.  
4. (Optioneel) Geef een of meer IP-adresbereiken op. Typ een naam, begin- en eind-IP-adressen voor elk bereik. 
5. Klik op **Opslaan**.

     ![Firewallinstellingen](./media/analysis-services-qs-firewall/aas-qs-firewall.png)

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer deze niet langer nodig zijn, verwijdert u de IP-adresbereiken of schakelt u de firewall uit.

## <a name="next-steps"></a>Volgende stappen
In deze snelstart hebt u geleerd hoe u een firewall configureert voor uw server. Nu dat u een server hebt en deze hebt beveiligd met een firewall, kunt u een eenvoudig voorbeeldgegevensmodel vanuit de portal toevoegen. Een voorbeeldmodel is handig als u meer wilt weten over het configureren van modeldatabaserollen en het testen van clientverbindingen. Als u meer wilt weten, gaat u verder met de zelfstudie waarin u leert een voorbeeldmodel toe te voegen.

> [!div class="nextstepaction"]
> [Zelfstudie: Een voorbeeldmodel toevoegen aan uw server](analysis-services-create-sample-model.md)
