---
title: Docker/Go-app maken in Linux - Azure App Service
description: Een Docker-installatiekopie waarop een Go-app wordt uitgevoerd naar Web App for Containers implementeren.
keywords: azure-app-service, web-app, go, docker, container
services: app-service
author: msangapu
manager: jeconnoc
ms.assetid: b97bd4e6-dff0-4976-ac20-d5c109a559a8
ms.service: app-service
ms.devlang: go
ms.topic: quickstart
ms.date: 03/28/2019
ms.author: msangapu
ms.custom: seodec18
ms.openlocfilehash: 88c9996ce3f2d89ae58881c913f6bd4e549b5814
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "62117744"
---
# <a name="run-a-custom-linux-container-in-azure-app-service"></a>Uitvoeren van een aangepaste Linux-container in Azure App Service

[App Service Linux](app-service-linux-intro.md) biedt vooraf gedefinieerde toepassingsstacks op Linux met ondersteuning voor talen zoals .NET, PHP, Node.js en meer. U kunt een aangepaste Docker-installatiekopie ook gebruiken om uw web-app uit te voeren op een toepassingsstack die nog niet in Azure is gedefinieerd. Deze snelstartgids laat zien hoe u een web-app kunt maken en een Go-installatiekopie kunt implementeren uit Docker Hub. U maakt de web-app met behulp van [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).

![Voorbeeld-app die wordt uitgevoerd in Azure](media/quickstart-docker-go/hello-world-in-browser.png)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-linux.md)]

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux.md)]

## <a name="create-a-web-app"></a>Een webtoepassing maken

Maak een [web-app](../overview.md) in het App Service-plan `myAppServicePlan` met de opdracht [az webapp create](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create). Vergeet niet om `<app name>` te vervangen door een wereldwijd unieke app-naam.

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app name> --deployment-container-image-name microsoft/azure-appservices-go-quickstart
```

In de voorgaande opdracht verwijst `--deployment-container-image-name` naar de installatiekopie van de openbare Docker Hub [microsoft/azure-appservices-go-quickstart](https://hub.docker.com/r/microsoft/azure-appservices-go-quickstart/).

Wanneer de web-app is gemaakt, toont de Azure CLI soortgelijke uitvoer als in het volgende voorbeeld:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app name>.scm.azurewebsites.net/<app name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

## <a name="browse-to-the-app"></a>Bladeren naar de app

```bash
http://<app_name>.azurewebsites.net/hello
```

![Voorbeeld-app die wordt uitgevoerd in Azure](media/quickstart-docker-go/hello-world-in-browser.png)

**Gefeliciteerd!** U hebt een aangepaste Docker-installatiekopie waarop een Go-app wordt uitgevoerd naar Web App for Containers geïmplementeerd.

[!INCLUDE [Clean-up section](../../../includes/cli-script-clean-up.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Zelfstudie: Implementeren vanaf de privé-containeropslagplaats](tutorial-custom-docker-image.md)

> [!div class="nextstepaction"]
> [Een aangepaste container configureren](configure-custom-container.md)

> [!div class="nextstepaction"]
> [Zelfstudie: Meerdere containers WordPress-app](tutorial-multi-container-app.md)
