---
title: bestand opnemen
description: bestand opnemen
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 02/02/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 01c879618ed96d193821b476cdd4772da9cf539b
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56118748"
---
Een [web-app](../articles/app-service/containers/app-service-linux-intro.md) maken in het App Service-plan `myAppServicePlan`. 

In Cloud Shell kunt u de opdracht [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create) gebruiken. Vervang in het volgende voorbeeld `<app_name>` door een unieke naam (geldige tekens zijn `a-z`, `0-9`, en `-`). 

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app_name> --deployment-local-git
```

Wanneer de web-app is gemaakt, toont de Azure CLI soortgelijke uitvoer als in het volgende voorbeeld:

```json
Local git is configured with url of 'https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git'
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

> [!NOTE]
> De URL van de externe Git wordt weergegeven in de eigenschap `deploymentLocalGitUrl`, met de indeling `https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git`. Sla deze URL op, want u hebt deze later nodig.
>
