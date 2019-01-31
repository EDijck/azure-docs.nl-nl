---
title: Met behulp van labels aan instrument query's in SQL Data Warehouse | Microsoft Docs
description: Tips voor het gebruik van labels aan query's die is geregistreerd in Azure SQL Data Warehouse om oplossingen te ontwikkelen.
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: implement
ms.date: 04/17/2018
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: 5c60a7dc44471599834c4b1225b397c8e6eabbd5
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55456456"
---
# <a name="using-labels-to-instrument-queries-in-azure-sql-data-warehouse"></a>Met behulp van labels aan query's die is geregistreerd in Azure SQL Data Warehouse
Tips voor het gebruik van labels aan query's die is geregistreerd in Azure SQL Data Warehouse om oplossingen te ontwikkelen.


## <a name="what-are-labels"></a>Wat zijn de labels?
SQL Data Warehouse biedt ondersteuning voor een concept genaamd query labels. Voordat u doorgaat in een uitgebreid, kunt u een voorbeeld laten we kijken:

```sql
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query Label')
;
```

De laatste regel van de tekenreeks 'Mijn Query Label' aan de query met tags. Deze code is vooral handig omdat het label query kunnen via de DMV's. Query's uitvoeren voor labels biedt een mechanisme voor probleem-query's zoeken en helpt bij het identificeren van de voortgang via een ELT uitvoeren.

Een goede naamconventie helpt. Het label te beginnen met het PROJECT, helpt PROCEDURE, instructie of opmerking bijvoorbeeld voor het aanduiden van de query door de code in broncodebeheer.

De volgende query gebruikt een dynamische Beheerweergave om te zoeken op label.

```sql
SELECT  *
FROM    sys.dm_pdw_exec_requests r
WHERE   r.[label] = 'My Query Label'
;
```

> [!NOTE]
> Het is essentieel voor het vierkante haken of dubbele aanhalingstekens rond het word-label plaatsen wanneer een query op. Label is een gereserveerd woord en veroorzaakt een fout op wanneer deze is niet gescheiden. 
> 
> 

## <a name="next-steps"></a>Volgende stappen
Zie voor meer tips voor ontwikkelaars [overzicht voor ontwikkelaars](sql-data-warehouse-overview-develop.md).


