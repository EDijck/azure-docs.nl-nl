---
title: Azure Functions op Linux met behulp van een aangepaste installatiekopie maken
description: Informatie over het maken van Azure Functions uitgevoerd op een aangepaste installatiekopie van Linux.
services: functions
keywords: ''
author: ggailey777
ms.author: glenga
ms.date: 02/25/2019
ms.topic: tutorial
ms.service: azure-functions
ms.custom: mvc
ms.devlang: azure-cli
manager: jeconnoc
ms.openlocfilehash: 03e1ec58b0ef3ad50a04f82ced7d20119ab3ef5b
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "62106852"
---
# <a name="create-a-function-on-linux-using-a-custom-image"></a>Een functie in Linux met behulp van een aangepaste installatiekopie maken

Met Azure Functions kunt u uw functies voor Linux hosten in uw eigen aangepaste container. U kunt ze ook [hosten op een standaard-Azure App Service-container](functions-create-first-azure-function-azure-cli-linux.md). Deze functionaliteit is vereist [de Functions-runtime 2.x](functions-versions.md).

In deze zelfstudie leert u hoe u uw functies in Azure implementeert als een aangepaste Docker-installatiekopie. Dit patroon is handig wanneer u de installatiekopie van de ingebouwde App Service-container moet aanpassen. U kunt een aangepaste installatiekopie gebruiken wanneer uw functies een specifieke taalversie nodig hebben of een specifieke afhankelijkheid of configuratie vereisen die niet binnen de ingebouwde installatiekopie aanwezig is. Basisinstallatiekopieën ondersteund voor Azure Functions vindt u in de [Azure Functions basis-opslagplaats voor installatiekopieën](https://hub.docker.com/_/microsoft-azure-functions-base). [Ondersteuning voor Python](functions-reference-python.md) is beschikbaar als preview op dit moment.

In deze zelfstudie leert u hoe u Azure Functions Core Tools gebruikt om een functie te maken in een aangepaste Linux-installatiekopie. U publiceert deze installatiekopie naar een functie-app in Azure, die is gemaakt met de Azure CLI.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een functie-app en Dockerfile maken met behulp van Core Tools.
> * Een aangepaste installatiekopie met behulp van Docker bouwen.
> * Een aangepaste installatiekopie naar een containerregister publiceren.
> * Een Azure Storage-account maken.
> * Een Linux App Service-plan maken.
> * Een functie-app vanuit Docker Hub implementeren.
> * Toepassingsinstellingen toevoegen aan de functie-app.
> * Continue implementatie inschakelen

De volgende stappen worden ondersteund op een Mac-, Windows- of Linux-computer.  

## <a name="prerequisites"></a>Vereisten

Voordat u dit voorbeeld kunt uitvoeren moet u ervoor zorgen dat u het volgende hebt:

* Installeer [Azure Core Tools versie 2.x](functions-run-local.md#v2).

* Installeer de [Azure CLI]( /cli/azure/install-azure-cli). In dit artikel is Azure CLI versie 2.0 of hoger vereist. Voer `az --version` uit om te zien welke versie u hebt.  
U kunt ook de [Azure Cloud Shell](https://shell.azure.com/bash) gebruiken.

* Een actief Azure-abonnement.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-the-local-function-app-project"></a>Het lokale functie-app-project maken

Voer de volgende opdracht uit vanaf de opdrachtregel om een functie-app-project te maken in de map `MyFunctionProj` van de huidige lokale map.

```bash
func init MyFunctionProj --docker
```

Wanneer u de optie `--docker` opneemt, wordt een dockerfile voor het project gegenereerd. Dit bestand wordt gebruikt voor het maken van een aangepaste container waarin het project wordt uitgevoerd. De basisinstallatiekopie die wordt gebruikt, is afhankelijk van de gekozen taal voor de runtime van de werkrol.  

Wanneer u hierom wordt gevraagd, kiest u een runtime voor de werkrol uit de volgende talen:

* `dotnet`: hiermee maakt u een .NET-klassebibliotheekproject (.csproj).
* `node`: hiermee maakt u een JavaScript-project.
* `python`: hiermee maakt u een Python-project.

[!INCLUDE [functions-python-preview-note](../../includes/functions-python-preview-note.md)]

Wanneer de opdracht wordt uitgevoerd, ziet u ongeveer de volgende uitvoer:

```output
Writing .gitignore
Writing host.json
Writing local.settings.json
Writing Dockerfile
```

Gebruik de volgende opdracht om naar de nieuwe projectmap `MyFunctionProj` te navigeren.

```bash
cd MyFunctionProj
```

[!INCLUDE [functions-create-function-core-tools](../../includes/functions-create-function-core-tools.md)]

[!INCLUDE [functions-update-function-code](../../includes/functions-update-function-code.md)]

[!INCLUDE [functions-run-function-test-local](../../includes/functions-run-function-test-local.md)]

## <a name="build-the-image-from-the-docker-file"></a>De installatiekopie maken op basis van het Docker-bestand

Bekijk de _Dockerfile_ in de hoofdmap van het project. Dit bestand beschrijft de omgeving die vereist is om de functie-app uit te voeren in Linux. Het volgende voorbeeld is een Dockerfile dat een container maakt die een functie-app uitvoert in runtime van de werkrol voor JavaScript (Node.js): 

```Dockerfile
FROM mcr.microsoft.com/azure-functions/node:2.0

ENV AzureWebJobsScriptRoot=/home/site/wwwroot
COPY . /home/site/wwwroot
```

> [!NOTE]
> De volledige lijst van ondersteunde basisinstallatiekopieën voor Azure Functions vindt u de [Azure Functions-basisinstallatiekopie pagina](https://hub.docker.com/_/microsoft-azure-functions-base).

### <a name="run-the-build-command"></a>De opdracht `build` uitvoeren
Voer in de hoofdmap de opdracht [docker build](https://docs.docker.com/engine/reference/commandline/build/) uit en geef vervolgens een naam, `mydockerimage` en een tag, `v1.0.0`, op. Vervang `<docker-id>` door de ID van uw Docker Hub-account. Met deze opdracht wordt de Docker-installatiekopie voor de container gebouwd.

```bash
docker build --tag <docker-id>/mydockerimage:v1.0.0 .
```

Wanneer de opdracht wordt uitgevoerd, ziet u ongeveer de volgende uitvoer, die in dit geval voor een runtime van een werkrol voor JavaScript is:

```bash
Sending build context to Docker daemon  17.41kB
Step 1/3 : FROM mcr.microsoft.com/azure-functions/node:2.0
2.0: Pulling from azure-functions/node
802b00ed6f79: Pull complete
44580ea7a636: Pull complete
73eebe8d57f9: Pull complete
3d82a67477c2: Pull complete
8bd51cd50290: Pull complete
7bd755353966: Pull complete
Digest: sha256:480e969821e9befe7c61dda353f63298f2c4b109e13032df5518e92540ea1d08
Status: Downloaded newer image for mcr.microsoft.com/azure-functions/node:2.0
 ---> 7c71671b838f
Step 2/3 : ENV AzureWebJobsScriptRoot=/home/site/wwwroot
 ---> Running in ed1e5809f0b7
Removing intermediate container ed1e5809f0b7
 ---> 39d9c341368a
Step 3/3 : COPY . /home/site/wwwroot
 ---> 5e196215935a
Successfully built 5e196215935a
Successfully tagged <docker-id>/mydockerimage:v1.0.0
```

### <a name="test-the-image-locally"></a>De installatiekopie lokaal testen
Controleer of de installatiekopie die u hebt gebouwd werkt door de Docker-installatiekopie uit te voeren in een lokale container. Geef de opdracht [docker run](https://docs.docker.com/engine/reference/commandline/run/) en geef de naam en het label van de installatiekopie door. Zorg ervoor dat u de poort opgeeft met behulp van het argument `-p`.

```bash
docker run -p 8080:80 -it <docker-ID>/mydockerimage:v1.0.0
```

Met de aangepaste installatiekopie die wordt uitgevoerd in een lokale Docker-container controleert u of de functie-app en container goed werken door te bladeren naar <http://localhost:8080>.

![Test de functie-app lokaal.](./media/functions-create-function-linux-custom-image/run-image-local-success.png)

U kunt eventueel uw functie op dit moment opnieuw testen in de lokale container met behulp van de volgende URL:

`http://localhost:8080/api/myhttptrigger?name=<yourname>`

Nadat u de functie-app in de container hebt geverifieerd, stopt u de uitvoering. U kunt nu de aangepaste installatiekopie naar uw Docker Hub-account pushen.

## <a name="push-the-custom-image-to-docker-hub"></a>De aangepaste installatiekopie naar Docker Hub pushen

Een register is een toepassing die als host fungeert voor installatiekopieën en installatiekopie- en containerservices biedt. Om uw installatiekopie te delen, moet u deze naar een register pushen. Docker Hub is een register voor de Docker-installatiekopieën waarmee u uw eigen openbare of particuliere opslagplaatsen kunt hosten.

Voordat u een installatiekopie kunt pushen, moet u zich aanmelden bij Docker Hub met behulp van de opdracht [docker login](https://docs.docker.com/engine/reference/commandline/login/). Vervang `<docker-id>` door uw accountnaam en typ uw wachtwoord bij de opdrachtprompt in de console. Zie [documentatie bij de opdracht voor dockeraanmelding](https://docs.docker.com/engine/reference/commandline/login/) voor andere wachtwoordopties voor Docker Hub.

```bash
docker login --username <docker-id>
```

Een bericht 'aanmelding geslaagd' bevestigt dat u bent aangemeld. Nadat u zich hebt aangemeld, pusht u de installatiekopie naar Docker Hub met behulp van de [docker push](https://docs.docker.com/engine/reference/commandline/push/)-opdracht.

```bash
docker push <docker-id>/mydockerimage:v1.0.0
```

Controleer of de push-bewerking is voltooid door de uitvoer van de opdracht te inspecteren.

```bash
The push refers to a repository [docker.io/<docker-id>/mydockerimage:v1.0.0]
24d81eb139bf: Pushed
fd9e998161c9: Mounted from <docker-id>/mydockerimage
e7796c35add2: Mounted from <docker-id>/mydockerimage
ae9a05b85848: Mounted from <docker-id>/mydockerimage
45c86e20670d: Mounted from <docker-id>/mydockerimage
v1.0.0: digest: sha256:be080d80770df71234eb893fbe4d... size: 1796
```

U kunt nu deze installatiekopie gebruiken als implementatiebron voor een nieuwe functie-app in Azure.

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-linux-app-service-plan"></a>Een Linux App Service-abonnement maken

Linux-hosting voor Functions wordt momenteel niet ondersteund in verbruiksabonnementen. U moet Linux-container-apps hosten in een Linux App Service-plan. Zie [deze vergelijking van hostingabonnementen van Azure Functions](functions-scale.md) voor meer informatie over hosting.

[!INCLUDE [app-service-plan-no-h](../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

## <a name="create-and-deploy-the-custom-image"></a>De aangepaste installatiekopie maken en implementeren

Een functie-app fungeert als host voor de uitvoering van uw functies. Een functie-app maken op basis van een Docker Hub-installatiekopie met behulp van de opdracht [az functionapp create](/cli/azure/functionapp#az-functionapp-create).

Vervang in de volgende opdracht de plaatsaanduiding `<app_name>` door een unieke functie-appnaam en gebruik de naam van het opslagaccount voor `<storage_name>`. De `<app_name>` wordt gebruikt als het standaard DNS-domein voor de functie-app. Om die reden moet de naam uniek zijn in alle apps in Azure. Net als voorheen is `<docker-id>` de naam van uw Docker-account.

```azurecli-interactive
az functionapp create --name <app_name> --storage-account  <storage_name>  --resource-group myResourceGroup \
--plan myAppServicePlan --deployment-container-image-name <docker-id>/mydockerimage:v1.0.0
```

Nadat de functie-app is gemaakt, toont Azure CLI soortgelijke informatie als in het volgende voorbeeld:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "containerSize": 1536,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "quickstart.azurewebsites.net",
  "enabled": true,
  "enabledHostNames": [
    "quickstart.azurewebsites.net",
    "quickstart.scm.azurewebsites.net"
  ],
   ....
    // Remaining output has been truncated for readability.
}
```

De _deployment-container-image-name_-parameter geeft aan dat de afbeelding die wordt gehost op Docker Hub wordt gebruikt om de functie-app te maken. Gebruik de [az functionapp config container show](/cli/azure/functionapp/config/container#az-functionapp-config-container-show) opdracht om informatie over de installatiekopie die wordt gebruikt voor de implementatie weer te geven. Gebruik de [az functionapp config container set](/cli/azure/functionapp/config/container#az-functionapp-config-container-set) opdracht voor het implementeren van een andere afbeelding.

## <a name="configure-the-function-app"></a>De functie-app configureren

De functie heeft de verbindingsreeks nodig om verbinding te maken met het standaardopslagaccount. Als u uw aangepaste installatiekopie naar een persoonlijk containeraccount publiceert, moet u de instellingen voor deze toepassing instellen als omgevingsvariabelen in het Dockerfile met de [ENV-instructie](https://docs.docker.com/engine/reference/builder/#env) of iets soortgelijks.

In dit geval is `<storage_name>` de naam van het opslagaccount dat u hebt gemaakt. Haal de verbindingsreeks op met de opdracht [az storage account show-connection-string](/cli/azure/storage/account). Voeg deze toepassingsinstellingen toe aan de functie-app met de opdracht [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings#az-functionapp-config-appsettings-set).

```azurecli-interactive
storageConnectionString=$(az storage account show-connection-string \
--resource-group myResourceGroup --name <storage_name> \
--query connectionString --output tsv)

az functionapp config appsettings set --name <app_name> \
--resource-group myResourceGroup \
--settings AzureWebJobsDashboard=$storageConnectionString \
AzureWebJobsStorage=$storageConnectionString
```

> [!NOTE]
> Als u een privécontainer gebruikt, moet u ook de volgende toepassingsinstellingen instellen  
> - DOCKER_REGISTRY_SERVER_USERNAME  
> - DOCKER_REGISTRY_SERVER_PASSWORD  
>
> U moet uw functie-app stoppen en opnieuw starten om deze waarden op te halen

U kunt nu uw functies op Linux in Azure testen.

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

## <a name="enable-application-insights"></a>Application Insights inschakelen

De aanbevolen manier voor het bewaken van de uitvoering van uw functies is door uw functie-app integreren met Azure Application Insights. Wanneer u een functie-app in Azure portal maakt, wordt deze integratie voor u uitgevoerd door standaard. Echter, wanneer u uw functie-app maakt met behulp van de Azure CLI, is niet de integratie in uw functie-app in Azure uitgevoerd.

Application Insights inschakelen voor uw functie-app:

[!INCLUDE [functions-connect-new-app-insights.md](../../includes/functions-connect-new-app-insights.md)]

Zie voor meer informatie, [Monitor Azure Functions](functions-monitoring.md).

## <a name="enable-continuous-deployment"></a>Continue implementatie inschakelen

Een van de voordelen van het gebruik van containers wordt updates automatisch implementeren als containers zijn bijgewerkt in het register. Inschakelen van continue implementatie met de [az functionapp deployment container config](/cli/azure/functionapp/deployment/container#az-functionapp-deployment-container-config) opdracht.

```azurecli-interactive
az functionapp deployment container config --enable-cd \
--query CI_CD_URL --output tsv \
--name <app_name> --resource-group myResourceGroup
```

Met deze opdracht retourneert de URL van de webhook implementatie nadat continue implementatie is ingeschakeld. U kunt ook de [az functionapp deployment show-cd-url voor container](/cli/azure/functionapp/deployment/container#az-functionapp-deployment-container-show-cd-url) opdracht om deze URL. 

Kopieer de URL van de implementatie en blader naar uw opslagplaats DockerHub, kiest u de **Webhooks** tabblad, typt u een **webhooknaam** plak de URL in voor de webhook **Webhook-URL**, en kies vervolgens het plusteken (**+**).

![De webhook in uw opslagplaats DockerHub toevoegen](media/functions-create-function-linux-custom-image/dockerhub-set-continuous-webhook.png)  

De webhook is ingesteld, worden updates voor de gekoppelde afbeelding in DockerHub resulteren in de functie-app downloaden en installeren van de meest recente installatiekopie.

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een functie-app en Dockerfile maken met behulp van Core Tools.
> * Een aangepaste installatiekopie met behulp van Docker bouwen.
> * Een aangepaste installatiekopie naar een containerregister publiceren.
> * Een Azure Storage-account maken.
> * Een Linux App Service-plan maken.
> * Een functie-app vanuit Docker Hub implementeren.
> * Toepassingsinstellingen toevoegen aan de functie-app.

Lees hoe u continue integratiefunctionaliteit kunt inschakelen die is ingebouwd in het App Service-kernplatform. U kunt de functie-app configureren, zodat de container opnieuw wordt geïmplementeerd wanneer u de installatiekopie bijwerkt in Docker Hub.

> [!div class="nextstepaction"] 
> [Doorlopende implementatie met Web App for Containers](../app-service/containers/app-service-linux-ci-cd.md)
