---
title: Resources voor het ontwikkelen van een datawarehouse in Azure | Microsoft Docs
description: Ontwikkeling van concepten, ontwerpbeslissingen, aanbevelingen en codering technieken voor SQL Data Warehouse.
services: sql-data-warehouse
author: WenJason
manager: digimobile
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
origin.date: 08/29/2018
ms.date: 10/15/2018
ms.author: v-jay
ms.reviewer: igorstan
ms.openlocfilehash: 8c04e0409faa3b63a8a2957284ac7aa96740ae03
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60747826"
---
# <a name="design-decisions-and-coding-techniques-for-sql-data-warehouse"></a>Ontwerpbeslissingen en codering technieken voor SQL Data Warehouse
Bekijk via deze ontwikkeling artikelen voor meer informatie over belangrijke ontwerpbeslissingen, aanbevelingen en codering technieken voor SQL Data Warehouse.

## <a name="key-design-decisions"></a>Belangrijkste ontwerpbeslissingen
De volgende artikelen markeren concepten en volg de ontwerpbeslissingen voor het ontwikkelen van een gedistribueerde datawarehouse met SQL Data Warehouse:

* [Verbindingen][connections]
* [concurrency][concurrency]
* [Transacties][transactions]
* [gebruiker gedefinieerde schema 's][user-defined schemas]
* [tabeldistributie][table distribution]
* [tabelindexen][table indexes]
* [Tabelpartities][table partitions]
* [CTAS][CTAS]
* [statistieken][statistics]

## <a name="development-recommendations-and-coding-techniques"></a>Aanbevelingen voor ontwikkeling en technieken voor codering
Deze artikelen markeren specifieke code technieken, tips en aanbevelingen voor het ontwikkelen van uw SQL Data Warehouse:

* [Opgeslagen procedures][stored procedures]
* [labels][labels]
* [Weergaven][views]
* [Tijdelijke tabellen][temporary tables]
* [Dynamic SQL][dynamic SQL]
* [lussen][looping]
* [groeperen op Opties][group by options]
* [variabele toewijzing][variable assignment]

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie, [SQL Data Warehouse T-SQL-instructies](sql-data-warehouse-reference-tsql-statements.md).

<!--Image references-->

<!--Article references-->
[concurrency]: ./resource-classes-for-workload-management.md
[connections]: ./sql-data-warehouse-connect-overview.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[dynamic SQL]: ./sql-data-warehouse-develop-dynamic-sql.md
[group by options]: ./sql-data-warehouse-develop-group-by-options.md
[labels]: ./sql-data-warehouse-develop-label.md
[looping]: ./sql-data-warehouse-develop-loops.md
[statistics]: ./sql-data-warehouse-tables-statistics.md
[stored procedures]: ./sql-data-warehouse-develop-stored-procedures.md
[table distribution]: ./sql-data-warehouse-tables-distribute.md
[table indexes]: ./sql-data-warehouse-tables-index.md
[table partitions]: ./sql-data-warehouse-tables-partition.md
[temporary tables]: ./sql-data-warehouse-tables-temporary.md
[transactions]: ./sql-data-warehouse-develop-transactions.md
[user-defined schemas]: ./sql-data-warehouse-develop-user-defined-schemas.md
[variable assignment]: ./sql-data-warehouse-develop-variable-assignment.md
[views]: ./sql-data-warehouse-develop-views.md


<!--MSDN references-->
[renaming objects]: https://msdn.microsoft.com/library/mt631611.aspx

<!--Other Web references-->