---
title: Kenmerken van de relatie-entiteit in de Academic Knowledge API
titlesuffix: Azure Cognitive Services
description: Meer informatie over de kenmerken die u met de relatie-entiteit in de Academic Knowledge API gebruiken kunt.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: 82e6a5b66342e58e62da029d617cbd1d74c28149
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "61340510"
---
# <a name="affiliation-entity"></a>Relatie-entiteit

<sub> * De volgende kenmerken zijn specifiek voor de relatie-entiteit. (Ty = '5') </sub>

Name    |Description                            |Type       | Bewerkingen
------- | ------------------------------------- | --------- | ----------------------------
Id      |Entiteits-id                              |Int64      |Is gelijk aan
AfN     |Relatie genormaliseerde naam        |String     |Is gelijk aan
DAfN    |Weergavenaam van de relatie       |String     |geen
CC      |Relatie totale citaat tellen           |Int32      |geen  
ECC     |Relatie totale geschatte citaat tellen |Int32      |geen

## <a name="extended-metadata-attributes"></a>Uitgebreide metagegevens kenmerken ##

Name    | Beschrijving               
--------|---------------------------    
Pc      |Lidmaatschap van papier tellen
