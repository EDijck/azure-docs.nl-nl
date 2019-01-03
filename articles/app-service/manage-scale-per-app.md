---
title: Hosting met hoge dichtheid met behulp van de per-app schalen - Azure App Service | Microsoft Docs
description: Hosting met hoge dichtheid op Azure App Service
author: btardif
manager: erikre
editor: ''
services: app-service\web
documentationcenter: ''
ms.assetid: a903cb78-4927-47b0-8427-56412c4e3e64
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 01/22/2018
ms.author: byvinyal
ms.custom: seodec18
ms.openlocfilehash: 49b5978fd647a4667503676528120a36495021c6
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53731089"
---
# <a name="high-density-hosting-on-azure-app-service-using-per-app-scaling"></a>Hosting met hoge dichtheid op Azure App Service met behulp van per-app schalen
Standaard, schaalt u App Service-apps door te schalen de [App Service-plan](overview-hosting-plans.md) ze worden uitgevoerd op. Wanneer meerdere apps worden uitgevoerd in hetzelfde App Service-plan, wordt alle apps in elk exemplaar van de scale-out uitgevoerd in het abonnement.

U kunt inschakelen *per-app schalen* plannen niveau van de App-Service. Het kan worden geschaald voor een app onafhankelijk van de App Service-plan die als host fungeert. Op deze manier een App Service-plan kan worden geschaald naar 10 exemplaren, maar een app kan worden ingesteld op het gebruik van slechts vijf.

> [!NOTE]
> Per-app schalen is alleen beschikbaar voor **Standard**, **Premium**, **Premium V2** en **geïsoleerd** Prijscategorieën.
>

## <a name="per-app-scaling-using-powershell"></a>Per app schalen met behulp van PowerShell

Maak een plan met de per-app schalen door te geven in de ```-PerSiteScaling $true``` parameter voor de ```New-AzureRmAppServicePlan``` cmdlet.

```powershell
New-AzureRmAppServicePlan -ResourceGroupName $ResourceGroup -Name $AppServicePlan `
                            -Location $Location `
                            -Tier Premium -WorkerSize Small `
                            -NumberofWorkers 5 -PerSiteScaling $true
```

Inschakelen van per-app schalen met een bestaande App Service-Plan door te geven in de `-PerSiteScaling $true` parameter voor de ```Set-AzureRmAppServicePlan``` cmdlet.

```powershell
# Enable per-app scaling for the App Service Plan using the "PerSiteScaling" parameter.
Set-AzureRmAppServicePlan -ResourceGroupName $ResourceGroup `
   -Name $AppServicePlan -PerSiteScaling $true
```

Op het niveau van de app, het aantal exemplaren van die de app in de App Service-plan gebruiken kunt te configureren.

In het volgende voorbeeld wordt is de app beperkt tot twee instanties, ongeacht hoeveel exemplaren van de onderliggende app service-plan geschaald naar.

```powershell
# Get the app we want to configure to use "PerSiteScaling"
$newapp = Get-AzureRmWebApp -ResourceGroupName $ResourceGroup -Name $webapp
    
# Modify the NumberOfWorkers setting to the desired value.
$newapp.SiteConfig.NumberOfWorkers = 2
    
# Post updated app back to azure
Set-AzureRmWebApp $newapp
```

> [!IMPORTANT]
> `$newapp.SiteConfig.NumberOfWorkers` wijkt af van `$newapp.MaxNumberOfWorkers`. Per-app vergroten/verkleinen gebruikt `$newapp.SiteConfig.NumberOfWorkers` om te bepalen van de kenmerken van de schaal van de app.

## <a name="per-app-scaling-using-azure-resource-manager"></a>Per-app schalen met Azure Resource Manager

De volgende Azure Resource Manager-sjabloon wordt gemaakt:

- Een App Service-plan geschaald naar 10 exemplaren
- een app die geconfigureerd om te schalen naar een maximum van vijf exemplaren.

Het instellen van de App Service-plan de **PerSiteScaling** eigenschap op ' True ' `"perSiteScaling": true`. Het instellen van de app de **aantal werknemers** gebruiken tot en met 5 `"properties": { "numberOfWorkers": "5" }`.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters":{
        "appServicePlanName": { "type": "string" },
        "appName": { "type": "string" }
        },
    "resources": [
    {
        "comments": "App Service Plan with per site perSiteScaling = true",
        "type": "Microsoft.Web/serverFarms",
        "sku": {
            "name": "P1",
            "tier": "Premium",
            "size": "P1",
            "family": "P",
            "capacity": 10
            },
        "name": "[parameters('appServicePlanName')]",
        "apiVersion": "2015-08-01",
        "location": "West US",
        "properties": {
            "name": "[parameters('appServicePlanName')]",
            "perSiteScaling": true
        }
    },
    {
        "type": "Microsoft.Web/sites",
        "name": "[parameters('appName')]",
        "apiVersion": "2015-08-01-preview",
        "location": "West US",
        "dependsOn": [ "[resourceId('Microsoft.Web/serverFarms', parameters('appServicePlanName'))]" ],
        "properties": { "serverFarmId": "[resourceId('Microsoft.Web/serverFarms', parameters('appServicePlanName'))]" },
        "resources": [ {
                "comments": "",
                "type": "config",
                "name": "web",
                "apiVersion": "2015-08-01",
                "location": "West US",
                "dependsOn": [ "[resourceId('Microsoft.Web/Sites', parameters('appName'))]" ],
                "properties": { "numberOfWorkers": "5" }
            } ]
        }]
}
```

## <a name="recommended-configuration-for-high-density-hosting"></a>Aanbevolen configuratie voor het hosten van high-densitysampling
Per app schalen is een functie die is ingeschakeld in beide globale Azure-regio's en [App Service-omgevingen](environment/app-service-app-service-environment-intro.md). De aanbevolen strategie is echter gebruiken van App Service-omgevingen om te profiteren van geavanceerde functies en de grotere groepen van de capaciteit.  

Volg deze stappen voor het configureren van hoge dichtheid die als host fungeert voor uw apps:

1. De App Service Environment configureren en kies een groep met werkrollen die is aan het scenario met hosting met hoge dichtheid toegewezen.
2. Één App Service-plan maken en schalen van het gebruik van de beschikbare capaciteit in de groep met werkrollen.
3. Stel de `PerSiteScaling` vlag op ' True ' voor de App Service-plan.
4. Nieuwe apps worden gemaakt en toegewezen aan het App Service-plan met de **numberOfWorkers** eigenschap ingesteld op **1**. Met deze configuratie geeft de hoogst mogelijke dichtheid op deze groep met werkrollen.
5. Het aantal werknemers kan afzonderlijk worden geconfigureerd per app voor het verlenen van extra resources naar behoefte. Bijvoorbeeld:
    - Een app intensief gebruik kunt stellen **numberOfWorkers** naar **3** hebben meer verwerkingscapaciteit voor die app. 
    - Laag gebruik apps wordt ingesteld **numberOfWorkers** naar **1**.

## <a name="next-steps"></a>Volgende stappen

- [Gedetailleerd overzicht van Azure App Service-plannen](overview-hosting-plans.md)
- [Inleiding tot de App Service-omgeving](environment/app-service-app-service-environment-intro.md)
