---
title: Overzicht van Azure Resource Graph
description: Begrijpen hoe de Azure Resource Graph-service mogelijk maakt complexe query's uitvoeren met resources op schaal.
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/29/2019
ms.topic: overview
ms.service: resource-graph
manager: carmonm
ms.openlocfilehash: 28efdabc024fd32c83ba966b15284ec6ff368d4d
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/08/2019
ms.locfileid: "59269280"
---
# <a name="overview-of-the-azure-resource-graph-service"></a>Overzicht van de Azure Resource Graph-service

Azure Resource Graph is een service in Azure die is ontworpen om Azure Resource Management uit te breiden met efficiënte en krachtige resourceverkenning en de mogelijkheid om query's op grote schaal uit te voeren binnen alle abonnementen en beheergroepen, zodat u uw omgeving effectief kunt beheren. Deze query's bieden de volgende mogelijkheden:

- De mogelijkheid om resources op te vragen met geavanceerde opties voor filteren, groeperen en sorteren op resource-eigenschappen.
- De mogelijkheid om resources iteratief te verkennen op basis van governancevereisten en de resulterende expressie te converteren naar een beleidsdefinitie.
- De mogelijkheid om de impact van het toepassen van een beleid in een grote cloudomgeving te beoordelen.

In deze documentatie komt elke mogelijkheid gedetailleerd aan bod.

> [!NOTE]
> Azure Resource Graph wordt gebruikt door de nieuwe bladerervaring Alle resources van de Azure Portal. Het is bedoeld voor klanten met een nodig voor het beheren van grootschalige omgevingen.

## <a name="how-does-resource-graph-complement-azure-resource-manager"></a>Hoe Resource Graph Azure Resource Manager aanvult

Azure Resource Manager verzendt momenteel gegevens naar een beperkte resourcecache die verschillende resourcevelden beschikbaar maakt, met name Resourcenaam, Id, Type, Resourcegroep, Abonnementen en Locatie. Voorheen als er met meer resource-eigenschappen werd gewerkt, moest elke afzonderlijke resourceprovider afzonderlijk worden aangeroepen en de eigenschapsdetails voor elke resource afzonderlijk worden opgevraagd.

Met Azure Resource Graph hebt u toegang tot de eigenschappen die de resourceproviders retourneren zonder dat u elke resourceprovider afzonderlijk moet aanroepen. Zoek voor een lijst van ondersteunde resourcetypen, een **Ja** in de [Resources voor volledige-implementaties](../../azure-resource-manager/complete-mode-deletion.md) tabel.

## <a name="the-query-language"></a>De querytaal

Nu u weet wat Azure Resource Graph is, leert u hoe u query's kunt maken.

Het is belangrijk te weten dat de querytaal van Azure Resource Graph is gebaseerd op de [querytaal van Kusto](../../data-explorer/data-explorer-overview.md) die wordt gebruikt door Azure Data Explorer.

Voor meer informatie over bewerkingen en functies die kunnen worden gebruikt met Azure Resource Graph raadpleegt u eerst [De querytaal van Resource Graph](./concepts/query-language.md). Zie [Resources verkennen](./concepts/explore-resources.md) als u de resources wilt bekijken.

## <a name="permissions-in-azure-resource-graph"></a>Machtigingen in Azure Resource Graph

Om Resource Graph te kunnen gebruiken, moet u over de juiste machtigingen beschikken in [Op rollen gebaseerd toegangsbeheer](../../role-based-access-control/overview.md) (RBAC), met minimaal leestoegang tot de resources die u wilt zoeken. Zonder ten minste `read`-machtigingen voor het Azure-object of de objectgroep worden er geen resultaten geretourneerd.

> [!NOTE]
> Grafiek van resources maakt gebruik van de abonnementen die beschikbaar zijn voor een principal tijdens het aanmelden. Als u wilt zien van resources van een nieuw abonnement gedurende een actieve sessie toegevoegd, moet de principal de context vernieuwen. Deze actie wordt automatisch uitgevoerd wanneer u zich af te melden en opnieuw aanmelden.

## <a name="throttling"></a>Beperking

Query's voor Resource Graph worden beperkt om de beste ervaring en responstijd te bieden aan alle klanten. Als uw organisatie de Resource Graph-API wil gebruiken voor grootschalige en regelmatige query's, gebruikt u de functie Portalfeedback vanaf de Resource Graph-pagina. Omschrijf uw bedrijfsscenario en vergeet niet om het selectievakje 'Microsoft mag per e-mail contact met u opnemen over uw feedback', zodat het team contact met u kan opnemen.

## <a name="running-your-first-query"></a>Uw eerste query uitvoeren

Resourcegrafiek biedt ondersteuning voor Azure CLI, Azure PowerShell en Azure SDK voor .NET. De query is opgebouwd hetzelfde voor elke taal. Meer informatie over het inschakelen van Resource Graph in [Azure CLI](first-query-azurecli.md#add-the-resource-graph-extension) en [Azure PowerShell](first-query-powershell.md#add-the-resource-graph-module).

## <a name="next-steps"></a>Volgende stappen

- Uw eerste query uitvoeren met [Azure CLI](first-query-azurecli.md)
- Uw eerste query uitvoeren met [Azure PowerShell](first-query-powershell.md)
- Beginnen met [Starter query's](./samples/starter.md)
- Uw kennis vergroten met [Geavanceerde query's](./samples/advanced.md)