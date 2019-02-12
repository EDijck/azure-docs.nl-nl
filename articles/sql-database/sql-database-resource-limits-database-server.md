---
title: Azure SQL Database server-bronlimieten | Microsoft Docs
description: In dit artikel biedt een overzicht van de Azure SQL Database-server resourcelimieten voor individuele databases en elastische pools. Het bevat ook informatie over wat er gebeurt wanneer deze resourcelimieten zijn bereikt of overschreden.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: sashan,moslake
manager: craigg
ms.date: 02/07/2019
ms.openlocfilehash: 670ca1b8ba16122d4e969a41f8679e1a6d1b27c6
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "55990101"
---
# <a name="sql-database-resource-limits-for-azure-sql-database-server"></a>SQL Database-resourcebeperkingen voor Azure SQL Database-server

In dit artikel biedt een overzicht van de SQL Database-resourcebeperkingen voor een SQL Database-server die wordt beheerd van individuele databases en elastische pools. Het bevat ook informatie over wat er gebeurt wanneer deze resourcelimieten zijn bereikt of overschreden.

> [!NOTE]
> Zie voor de grenzen van beheerde exemplaren, [SQL Database-resourcebeperkingen voor beheerde exemplaren](sql-database-managed-instance-resource-limits.md).

## <a name="maximum-resource-limits"></a>Maximale resourcebeperkingen

| Resource | Limiet |
| :--- | :--- |
| Databases per server | 5000 |
| Standaardaantal servers per abonnement in andere regio 's | 20 |
| Maximumaantal servers per abonnement in andere regio 's | 200 |  
| DTU / eDTU-quotum per server | 54,000 |  
| vCore quotum per server-exemplaar | 540 |
| Maximum aantal toepassingen per server | Beperkt door het aantal dtu's of vCores. Bijvoorbeeld, als elke groep 1000 dtu's is, klikt u vervolgens een server kan ondersteunen 54 pools.|
||||

> [!NOTE]
> Als u wilt meer DTU-quotum voor /eDTU, vCore quotum of meer servers dan de standaardinstelling, kan een nieuwe ondersteuningsaanvraag in de Azure-portal voor het abonnement met probleemtype 'Target' worden verzonden. De DTU / eDTU-quota en database-limiet per server beperkt het aantal elastische pools per server.
> [!IMPORTANT]
> Als het aantal databases de limiet is per SQL Database-server nadert, kan het volgende gebeuren:
> - Latentie in het uitvoeren van query's op de hoofddatabase te vergroten.  Dit omvat de weergave van gebruiksstatistieken resource, zoals sys.resource_stats.
> - Latentie van bewerkingen vergroten en rendering portal standpunten die betrekking hebben op het inventariseren van databases op de server.

## <a name="what-happens-when-database-resource-limits-are-reached"></a>Wat gebeurt er na het verstrijken van database-resourcebeperkingen

### <a name="compute-dtus-and-edtus--vcores"></a>COMPUTE (dtu's en Edtu / vCores)

Wanneer Databasegebruik voor rekenkracht (gemeten door het aantal dtu's en edtu's of vCores) toeneemt, wordt latentie van query vergroot en kan zelfs time-out. In deze omstandigheden vindt query's kunnen in de wachtrij geplaatst door de service en vindt u bronnen voor uitvoering als resource zijn gratis.
Als hoge compute-gebruik worden aangetroffen, wordt een risicobeperking opties zijn onder andere:

- Het compute vergroten van de database of elastische pool voor de database met meer rekenresources. Zie [schalen van één database-resources](sql-database-single-database-scale.md) en [resources voor elastische pool schalen](sql-database-elastic-pool-scale.md).
- Query's te verminderen van het Resourcegebruik van elke query te optimaliseren. Zie voor meer informatie, [Query afstemmen/Hinting](sql-database-performance-guidance.md#query-tuning-and-hinting).

### <a name="storage"></a>Opslag

Wanneer ruimte in database gebruikt de maximaal toegestane grootte is bereikt, database ingevoegd en niet bijwerken waarmee u de grootte van de gegevens te verhogen en -clients ontvangen een [foutbericht](sql-database-develop-error-messages.md). Database selecteren en doorgaan met verwijderen te voltooien.

Als hoge ruimteverbruik worden aangetroffen, wordt een risicobeperking opties zijn onder andere:

- Vergroten van de maximale grootte van de database of elastische pool of Voeg meer opslag toe. Zie [schalen van één database-resources](sql-database-single-database-scale.md) en [resources voor elastische pool schalen](sql-database-elastic-pool-scale.md).
- Als de database zich in een elastische pool, kan vervolgens ook de database worden verplaatst buiten de groep zodat de opslagruimte wordt niet gedeeld met andere databases.
- Een database voor het vrijmaken van ongebruikte ruimte verkleinen. Zie voor meer informatie, [bestandsruimte in Azure SQL Database beheren](sql-database-file-space-management.md)

### <a name="sessions-and-workers-requests"></a>Sessies en werknemers (aanvragen)

Het maximum aantal sessies en -werkrollen worden bepaald door de service tier en compute-grootte (dtu's en edtu's). Nieuwe aanvragen zijn afgewezen als sessie- of werkrollen limieten zijn bereikt en clients een foutbericht weergegeven wordt. Terwijl het aantal verbindingen beschikbaar kan worden beheerd door de toepassing, is het aantal gelijktijdige werknemers vaak mogelijk moeilijker te schatten en beheren. Dit geldt met name tijdens perioden van piekbelasting wanneer database resourcelimieten zijn bereikt en werknemers zich opstapelen vanwege langer uitvoeren van query's.

Als hoog gebruik van sessie- of werkrollen worden aangetroffen, wordt een risicobeperking opties zijn onder andere:

- Vergroten van de service tier of compute-grootte van de database of elastische pool. Zie [schalen van één database-resources](sql-database-single-database-scale.md) en [resources voor elastische pool schalen](sql-database-elastic-pool-scale.md).
- Optimaliseren query's om te verlagen van het Resourcegebruik van elke query als de oorzaak van het toegenomen worker-gebruik is vanwege conflicten over rekenresources. Zie voor meer informatie, [Query afstemmen/Hinting](sql-database-performance-guidance.md#query-tuning-and-hinting).

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over algemene Azure-limieten [Azure-abonnement en Servicelimieten, quotums en beperkingen](../azure-subscription-service-limits.md).
- Zie voor meer informatie over dtu's en edtu's [dtu's en edtu's](sql-database-purchase-models.md#dtu-based-purchasing-model).
- Zie voor meer informatie over de maximale grootte tempdb [TempDB in Azure SQL Database](https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database).
