---
title: Azure PowerShell-scriptvoorbeelden voor SQL Database | Microsoft Docs
description: Azure PowerShell-scriptvoorbeelden waarmee u Azure SQL Database-servers, elastische pools, databases en firewalls kunt maken en beheren.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: PowerShell
ms.topic: sample
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 12/04/2018
ms.openlocfilehash: 055ddfa5c0fb399eea4bbe46126626b60bfe9cff
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52966922"
---
# <a name="azure-powershell-samples-for-azure-sql-database"></a>Azure PowerShell-voorbeelden voor Azure SQL Database

De volgende tabel bevat koppelingen naar Azure PowerShell-voorbeeldscripts voor Azure SQL Database.

| |  |
|---|---|
|**Een beheerd exemplaar, een individuele database en een elastische pool maken en configureren**||
| [Een individuele database maken en een firewallregel configureren](scripts/sql-database-create-and-configure-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Met dit PowerShell-script wordt één Azure SQL-database gemaakt en een regel voor een firewall op serverniveau geconfigureerd. |
| [Elastische pools maken en gepoolde databases verplaatsen](scripts/sql-database-move-database-between-pools-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Met dit PowerShell-script worden elastische pools van Azure SQL Database gemaakt, gepoolde databases verplaatst en rekengroottes gewijzigd.|
| [Een beheerd exemplaar maken en beheren](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/06/27/quick-start-script-create-azure-sql-managed-instance-using-powershell/) | Dit PowerShell-script laat zien hoe u een beheerd exemplaar maakt en beheert met Azure PowerShell |
| [Een beheerd exemplaar maken met behulp van een Azure Resource Manager-sjabloon](scripts/sql-managed-instance-create-powershell-azure-resource-manager-template.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Dit PowerShell-script laat zien hoe u een beheerd exemplaar maakt en beheert met Azure PowerShell en een Azure Resource Manager-sjabloon.|
|**Geo-replicatie en failover configureren**||
| [PowerShell gebruiken voor het configureren van actieve geo-replicatie voor één Azure SQL-database](scripts/sql-database-setup-geodr-and-failover-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Met dit PowerShell-script wordt actieve geo-replicatie geconfigureerd voor één Azure SQL-database en wordt overgeschakeld naar de secundaire replica. |
| [PowerShell gebruiken voor het configureren van actieve geo-replicatie voor een gepoolde Azure SQL-database](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Met dit PowerShell-script wordt actieve geo-replicatie geconfigureerd voor een Azure SQL-database in een elastische SQL-pool en wordt overgeschakeld naar de secundaire replica. |
| [Een failovergroep configureren en een failover uitvoeren voor een individuele database](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Met dit PowerShell-script wordt een failovergroep geconfigureerd voor een Azure SQL Database-serverexemplaar, een database toegevoegd voor de failovergroep en overgeschakeld naar de secundaire server |
|**Eén database en een elastische pool schalen**||
| [Een individuele database schalen](scripts/sql-database-monitor-and-scale-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Met dit PowerShell-script worden de prestatiemetrieken gecontroleerd van een Azure SQL-database, waarna de database naar een grotere rekengrootte wordt geschaald en er een waarschuwingsregel voor een van de prestatiemetrieken wordt gemaakt. |
| [Een elastische pool schalen](scripts/sql-database-monitor-and-scale-pool-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Met dit PowerShell-script worden de prestatiemetrieken gecontroleerd van een elastische pool van Azure SQL Database, waarna de database naar een grotere rekengrootte wordt geschaald en er een waarschuwingsregel voor een van de prestatiemetrieken wordt gemaakt.  |
| **Controle en bedreigingen detecteren** |
| [Controle en detectie van bedreigingen configureren](scripts/sql-database-auditing-and-threat-detection-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Met dit PowerShell-script wordt beleid voor controle en detectie van bedreigingen geconfigureerd voor een Azure SQL-database. |
| **Database herstellen, kopiëren en importeren**||
| [Database herstellen](scripts/sql-database-restore-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Met dit PowerShell-script wordt een Azure SQL-database teruggezet vanuit een geo-redundante back-up en wordt een verwijderde Azure SQL-database naar de laatste back-up hersteld. |
| [Database kopiëren naar nieuwe server](scripts/sql-database-copy-database-to-new-server-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Met dit PowerShell-script wordt een kopie van een bestaande Azure SQL-database gemaakt in een nieuwe Azure SQL-server. |
| [Database uit een BACPAC-bestand importeren](scripts/sql-database-import-from-bacpac-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Met dit PowerShell-script wordt een database vanuit een BACPAC-bestand naar een Azure SQL-server geïmporteerd. |
| **Gegevens tussen databases synchroniseren**||
| [Gegevens tussen SQL-databases synchroniseren](scripts/sql-database-sync-data-between-sql-databases.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Met dit PowerShell-script wordt Data Sync geconfigureerd voor het synchroniseren van gegevens tussen meerdere Azure SQL-databases. |
| [Gegevens synchroniseren tussen SQL Database en on-premises SQL Server](scripts/sql-database-sync-data-between-azure-onprem.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Met dit PowerShell-script wordt Data Sync geconfigureerd voor het synchroniseren van gegevens tussen Azure SQL-database en een on-premises SQL Server-database. |
| [Synchronisatieschema van SQL Data Sync bijwerken](scripts/sql-database-sync-update-schema.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Met dit PowerShell-script worden items aan het synchronisatieschema van Data Sync toegevoegd of eruit verwijderd. |
| **Transparent Data Encryption (TDE) configureren**||
| [Transparent Data Encryption beheren in een beheerd exemplaar met behulp van uw eigen sleutel uit Azure Key Vault](scripts/transparent-data-encryption-byok-sql-managed-instance-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Dit PowerShell-script configureert Transparant Data Encryption (TDE) in een Bring Your Own Key-scenario voor een beheerd exemplaar van Azure SQL, met behulp van een sleutel uit Azure Key Vault|
|||
