---
title: Azure Data Factory gegevensstroom toewijzing bestaat transformatie
description: Azure Data Factory gegevensstroom toewijzing bestaat transformatie
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/30/2019
ms.openlocfilehash: 6ce27ba699ae766ed4d2428f67d91379464bb9f1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60730975"
---
# <a name="azure-data-factory-mapping-data-flow-exists-transformation"></a>Azure Data Factory gegevensstroom toewijzing bestaat transformatie

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

De transformatie Exists is een rij transformatie die service is gestopt of rijen in uw gegevens door kunnen stromen kunt filteren. De bestaat transformatie is vergelijkbaar met ```SQL WHERE EXISTS``` en ```SQL WHERE NOT EXISTS```. Na een transformatie Filter de resulterende rijen uit de gegevensstroom ofwel bevat alle rijen waarin kolomwaarden van bron 1 aanwezig zijn in de bron 2 of bestaan niet in de bron 2.

![Instellingen bestaat](media/data-flow/exsits.png "1 bestaat")

Kies de tweede bron voor uw Exists zodat gegevensstroom, kunt u de waarden van Stream 1 op basis van Stream 2 vergelijken.

Selecteer de kolom van bron 1 en 2 van de bron waarvan u wilt controleren op bestaat of bestaan niet op basis van de waarden.

## <a name="multiple-exists-conditions"></a>Meerdere bestaat voorwaarden

Naast elke rij in uw voorwaarden kolom voor Exsits, vindt u een + aanmelding beschikbaar wanneer u de muisaanwijzer op rij bereikt. Hierdoor kunt u meerdere rijen voor Exists voorwaarden toevoegen.

## <a name="next-steps"></a>Volgende stappen

