---
title: Hulpprogramma voor elastische taken verwijderen
description: Informatie over het verwijderen van de elastische taken onderdelen met de Azure-portal van PowerShell.
services: sql-database
ms.service: sql-database
ms.subservice: elastic-scale
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 12/04/2018
ms.openlocfilehash: 2131aa63f0f5cdbbc9e0b4eb7c1f57604fe75cdb
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/06/2019
ms.locfileid: "55754951"
---
# <a name="uninstall-elastic-database-jobs-components"></a>Onderdelen van elastische taken verwijderen


[!INCLUDE [elastic-database-jobs-deprecation](../../includes/sql-database-elastic-jobs-deprecate.md)]


**Taken voor elastic Database** onderdelen kunnen worden verwijderd met de Azure portal of PowerShell.

## <a name="uninstall-elastic-database-jobs-components-using-the-azure-portal"></a>Elastische taken onderdelen met behulp van de Azure portal verwijderen
1. Open de [Azure Portal](https://portal.azure.com/).
2. Navigeer naar het abonnement met **taken voor Elastic Database** onderdelen, namelijk het abonnement in welke Elastic Database jobs onderdelen zijn geïnstalleerd.
3. Klik op **Bladeren** en klikt u op **resourcegroepen**.
4. Selecteer de resourcegroep met de naam '__ElasticDatabaseJob'.
5. Verwijder de resourcegroep.

## <a name="uninstall--elastic-database-jobs-components-using-powershell"></a>Verwijderen van onderdelen van Elastic Database-taken met behulp van PowerShell
1. Start een Microsoft Azure PowerShell-opdrachtvenster en navigeer naar de onderliggende map van de hulpprogramma's onder de map Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x: Type **cd-hulpprogramma's**.
   
     PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*>cd tools
2. De.\UninstallElasticDatabaseJobs.ps1 PowerShell-script uitvoeren.
   
     PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools > deblokkeren bestand.\UninstallElasticDatabaseJobs.ps1 PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools >. \ UninstallElasticDatabaseJobs.ps1

Of gewoon, voer het volgende script, ervan uitgaande dat het standaard waarden gebruikt voor de installatie van de onderdelen:

```powershell
$ResourceGroupName = "__ElasticDatabaseJob"
Switch-AzureMode AzureResourceManager

$resourceGroup = Get-AzureResourceGroup -Name $ResourceGroupName
if(!$resourceGroup)
{
     Write-Host "The Azure Resource Group: $ResourceGroupName has already been deleted.  Elastic database job components are uninstalled."
     return
}

Write-Host "Removing the Azure Resource Group: $ResourceGroupName.  This may take a few minutes.”
Remove-AzureResourceGroup -Name $ResourceGroupName -Force
Write-Host "Completed removing the Azure Resource Group: $ResourceGroupName.  Elastic database job components are now uninstalled."
```

## <a name="next-steps"></a>Volgende stappen
Zie opnieuw installeren van taken voor Elastic Database [installatie van de service van de taak Elastic Database](sql-database-elastic-jobs-service-installation.md)

Zie voor een overzicht van taken voor Elastic Database, [overzicht van de taken van Elastic Database](sql-database-elastic-jobs-overview.md).
