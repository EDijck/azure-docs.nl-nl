---
title: Een Device Provisioning Service instellen met behulp van de Azure CLI | Microsoft Docs
description: Azure-snelstart - Azure IoT Hub Device Provisioning-service instellen met de Azure CLI
author: wesmc7777
ms.author: wesmc
ms.date: 02/26/2018
ms.topic: quickstart
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 3062fb640985498ba35e23f6310828a2bd59bfed
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60363713"
---
# <a name="set-up-the-iot-hub-device-provisioning-service-with-azure-cli"></a>IoT Hub Device Provisioning Service instellen met de Azure CLI

De Azure CLI wordt gebruikt voor het maken en beheren van Azure-resources vanaf de opdrachtregel of in scripts. Deze snelstart gaat gedetailleerd in op hoe de Azure CLI kan worden gebruikt om een IoT Hub en een IoT Hub Device Provisioning-service te maken, en deze twee services aan elkaar te koppelen. 

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

> [!IMPORTANT]
> Zowel de IoT hub als de inrichtingsservice die u in deze snelstart gaat maken, kan openbaar worden gedetecteerd als DNS-eindpunt. Voorkom dat u gevoelige informatie wijzigt als u de namen wijzigt die voor deze resources worden gebruikt.
>



## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een resourcegroep maken met de opdracht [az group create](/cli/azure/group#az-group-create). Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. 

In het volgende voorbeeld wordt een resourcegroep met de naam *my-sample-resource-group* gemaakt op de locatie *westus*.

```azurecli-interactive 
az group create --name my-sample-resource-group --location westus
```

> [!TIP]
> In het voorbeeld wordt de resourcegroep gemaakt op de locatie US - west. U kunt een lijst met beschikbare locaties weergeven met de opdracht `az account list-locations -o table`.
>
>

## <a name="create-an-iot-hub"></a>Een IoT Hub maken

Maak een IoT Hub met de opdracht [az iot hub create](/cli/azure/iot/hub#az-iot-hub-create).

In het volgende voorbeeld wordt een IoT-hub met de naam *my-sample-hub* op de locatie *westus* gemaakt.  

```azurecli-interactive 
az iot hub create --name my-sample-hub --resource-group my-sample-resource-group --location westus
```

## <a name="create-a-provisioning-service"></a>Een inrichtingsservice maken

Een inrichtingsservice maken met de opdracht [az iot dps create](/cli/azure/iot/dps#az-iot-dps-create). 

In het volgende voorbeeld wordt een inrichtingsservice met de naam *my-sample-dps* op de locatie *westus* gemaakt.  

```azurecli-interactive 
az iot dps create --name my-sample-dps --resource-group my-sample-resource-group --location westus
```

> [!TIP]
> In het voorbeeld wordt de inrichtingsservice gemaakt op de locatie US - west. U kunt een lijst met beschikbare locaties weergeven door de opdracht `az provider show --namespace Microsoft.Devices --query "resourceTypes[?resourceType=='ProvisioningServices'].locations | [0]" --out table` uit te voeren of door naar de pagina [Status van Azure](https://azure.microsoft.com/status/) te gaan en te zoeken naar 'device provisioning service'. In opdrachten kunnen locaties worden opgegeven in één woord of in meerdere woorden, bijvoorbeeld: uswest, US - west, US WEST enzovoort. De waarde is niet hoofdlettergevoelig. Als u meerdere woorden gebruikt om een locatie op te geven, voeg dan de waarde toe tussen aanhalingstekens; bijvoorbeeld: `-- location "West US"`.
>


## <a name="get-the-connection-string-for-the-iot-hub"></a>De verbindingsreeks voor de IoT-hub ophalen

U hebt de verbindingsreeks voor uw IoT-hub nodig om deze aan de Device Provisioning Service te koppelen. Gebruik de opdracht [az iot hub show-connection-string](/cli/azure/iot/hub#az-iot-hub-show-connection-string) om de verbindingsreeks op te halen en om met de uitvoer ervan een variabele in te stellen die u gaat gebruiken om de twee resources aan elkaar te koppelen. 

In het volgende voorbeeld wordt de variabele *hubConnectionString* ingesteld op de waarde van de verbindingsreeks voor de primaire sleutel van het hubbeleid *iothubowner*. U kunt een ander beleid opgeven met de parameter `--policy-name`. De opdracht maakt gebruik van de [query-](/cli/azure/query-azure-cli) en [uitvoer](/cli/azure/format-output-azure-cli#tsv-output-format)opties van de Azure CLI om de verbindingsreeks op te halen uit de uitvoer van de opdracht.

```azurecli-interactive 
hubConnectionString=$(az iot hub show-connection-string --name my-sample-hub --key primary --query connectionString -o tsv)
```

U kunt de opdracht `echo` gebruiken om de verbindingsreeks weer te geven.

```azurecli-interactive 
echo $hubConnectionString
```

> [!NOTE]
> Deze twee opdrachten zijn geldig voor een host die wordt uitgevoerd onder Bash. Als u van een lokale Windows-/CMD-shell of een PowerShell-host gebruikmaakt, moet u de opdrachten wijzigen zodat de juiste syntaxis voor deze omgeving wordt gebruikt.
>

## <a name="link-the-iot-hub-and-the-provisioning-service"></a>De IoT-hub en de inrichtingsservice aan elkaar koppelen

U kunt de IoT-hub en uw inrichtingsservice aan elkaar koppelen met de opdracht [az iot dps linked-hub create](/cli/azure/iot/dps/linked-hub#az-iot-dps-linked-hub-create). 

In het volgende voorbeeld wordt een IoT-hub met de naam *my-sample-hub* op de locatie *westus* gekoppeld aan een Device Provisioning Service met de naam *my-sample-dps*. Hierbij wordt gebruikgemaakt van de verbindingsreeks voor *my-sample-hub* die in de vorige stap is opgeslagen in de variabele *hubConnectionString*.

```azurecli-interactive 
az iot dps linked-hub create --dps-name my-sample-dps --resource-group my-sample-resource-group --connection-string $hubConnectionString --location westus
```

## <a name="verify-the-provisioning-service"></a>De inrichtingsservice controleren

De details ophalen uit uw inrichtingsservice met de opdracht [az iot dps show](/cli/azure/iot/dps#az-iot-dps-show).

In het volgende voorbeeld worden de details van een inrichtingsservice met de naam *my-sample-dps* opgehaald. De gekoppelde IoT-hub wordt weergegeven in de verzameling *properties.iotHubs*.

```azurecli-interactive
az iot dps show --name my-sample-dps
```

## <a name="clean-up-resources"></a>Resources opschonen

Andere Quick Starts in deze verzameling zijn op deze Quick Start gebaseerd. Als u van plan bent om door te gaan met andere snelstartgidsen of met de zelfstudies, verwijdert u de resources die u in deze snelstartgids hebt gemaakt niet. Als u niet van plan bent om door te gaan, kunt u de volgende opdrachten gebruiken om de inrichtingsservice, de IoT-hub of de resourcegroep en alle bijbehorende resources te verwijderen.

Voer de opdracht [az iot dps delete](/cli/azure/iot/dps#az-iot-dps-delete) uit om de inrichtingsservice te verwijderen:

```azurecli-interactive
az iot dps delete --name my-sample-dps --resource-group my-sample-resource-group
```
Voer de opdracht [az iot hub delete](/cli/azure/iot/hub#az-iot-hub-delete) uit om de IoT-hub te verwijderen:

```azurecli-interactive
az iot hub delete --name my-sample-hub --resource-group my-sample-resource-group
```

Voer de opdracht [az group delete](/cli/azure/group#az-group-delete) uit om de resourcegroep en alle bijbehorende resources te verwijderen:

```azurecli-interactive
az group delete --name my-sample-resource-group
```

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u een IoT-hub en een Device Provisioning Service-exemplaar geïmplementeerd en de twee resources gekoppeld. Als u wilt weten hoe u deze instellingen gebruikt voor het inrichten van een gesimuleerd apparaat, gaat u verder met de rest van de snelstartgids voor het maken van een gesimuleerd apparaat.

> [!div class="nextstepaction"]
> [Snelstartgids voor het maken van een gesimuleerd apparaat](./quick-create-simulated-device.md)

