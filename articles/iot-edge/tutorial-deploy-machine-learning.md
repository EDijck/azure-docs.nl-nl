---
title: 'Zelfstudie: Azure Machine Learning implementeren op een apparaat - Azure IoT Edge | Microsoft Docs'
description: In deze zelfstudie implementeert u Azure Machine Learning als een module op een Edge-apparaat
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 11/15/2018
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc, seodec18
ms.openlocfilehash: 464d16d4bbcbdbefd36ce1132630ad702d7a0c90
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/26/2019
ms.locfileid: "55076963"
---
# <a name="tutorial-deploy-azure-machine-learning-as-an-iot-edge-module-preview"></a>Zelfstudie: Azure Machine Learning als een IoT Edge-module implementeren (preview)

U kunt IoT Edge-modules gebruiken voor het implementeren van code die uw bedrijfslogica rechtstreeks op uw IoT Edge-apparaten implementeert. Deze zelfstudie laat u stapsgewijs zien hoe u een Azure Machine Learning-module kunt implementeren waarmee wordt voorspeld wanneer een apparaat mislukt op basis van gesimuleerde machinetemperatuurgegevens. Zie [Documentatie voor Azure Machine Learning](../machine-learning/service/how-to-deploy-to-iot.md) voor meer informatie over de service Azure Machine Learning op IoT Edge.

De Azure Machine Learning-module die u in deze zelfstudie maakt, leest de omgevingsgegevens die door uw apparaat zijn gegenereerd en markeert de berichten als afwijkend of niet.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een Azure Machine Learning-module maken
> * Een module-container naar een Azure-containerregister zenden
> * Een Azure Machine Learning-module implementeren op uw IoT Edge-apparaat
> * Gegenereerde gegevens weergeven

>[!NOTE]
>Azure Machine Learning-modules in Azure IoT Edge zijn in de openbare preview.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Vereisten

Een Azure IoT Edge-apparaat:

* U kunt uw ontwikkelcomputer of een virtuele machine gebruiken als een Edge-apparaat door de stappen te volgen in de snelstart voor [Linux-](quickstart-linux.md) of [Windows-apparaten](quickstart.md).
* De Azure Machine Learning-module ondersteunt geen ARM-processoren.

Cloudresources:

* Een gratis of standaard [IoT Hub](../iot-hub/iot-hub-create-through-portal.md)-laag in Azure.
* Een Azure Machine Learning-werkruimte. Volg de instructies in [Voorbereidingen voor het implementeren van modellen in IoT Edge](../machine-learning/service/how-to-deploy-to-iot.md) om er een te maken.


### <a name="disable-process-identification"></a>Procesidentificatie uitschakelen

>[!NOTE]
>
> In de preview-fase biedt Azure Machine Learning geen ondersteuning voor de beveiligingsfunctie voor procesidentificatie die standaard is ingeschakeld voor IoT Edge.
> Hieronder ziet u hoe u deze functie kunt uitschakelen. Dit is echter niet geschikt voor productieomgevingen. Deze stappen zijn alleen nodig op Linux, omdat u dit hebt gedaan tijdens de installatie van de Windows Edge-runtime.

Als u procesidentificatie wilt uitschakelen op uw IoT Edge-apparaat, moet u het IP-adres en de poort voor **workload_uri** en **management_uri** opgeven in de sectie **verbinding maken** van de IoT Edge-daemonconfiguratie.

Haal eerst het IP-adres op. Voer in de opdrachtregel `ipconfig` in en kopieer het IP-adres van de **docker0**-interface.

Bewerk het configuratiebestand voor de IoT Edge-daemon:

```cmd/sh
sudo nano /etc/iotedge/config.yaml
```

Werk de sectie **verbinding maken** van de configuratie bij met uw IP-adres. Bijvoorbeeld:
```yaml
connect:
  management_uri: "http://172.17.0.1:15580"
  workload_uri: "http://172.17.0.1:15581"
```

Voer in de sectie **luisteren** van de configuratie dezelfde adressen in. Bijvoorbeeld:

```yaml
listen:
  management_uri: "http://172.17.0.1:15580"
  workload_uri: "http://172.17.0.1:15581"
```

Maak een omgevingsvariabele IOTEDGE_HOST met het adres van de management_uri. (Als u de variabele permanent wilt instellen, voegt u deze toe aan `/etc/environment`). Bijvoorbeeld:

```cmd/sh
export IOTEDGE_HOST="http://172.17.0.1:15580"
```


## <a name="create-the-azure-machine-learning-service-container"></a>De servicecontainer voor Azure Machine Learning maken
In deze sectie downloadt u de bestanden van het getrainde model en converteert die in een Azure Machine Learning-servicecontainer.

Volg de instructies in de documentatie van [Voorbereidingen voor het implementeren van modellen in IoT Edge](../machine-learning/service/how-to-deploy-to-iot.md) om een Docker-container met uw Machine Learning-model te maken.  Alle onderdelen die vereist zijn voor de Docker-installatiekopie bevinden zich in de [AI-werkset voor Azure IoT Edge Git-repo](https://github.com/Azure/ai-toolkit-iot-edge/tree/master/IoT%20Edge%20anomaly%20detection%20tutorial).

### <a name="view-the-container-repository"></a>De containeropslagplaats bekijken

Controleer of de containerinstallatiekopie is gemaakt en opgeslagen in het Azure-containerregister dat is gekoppeld aan de machine learning-omgeving.

1. Ga in [Azure Portal](https://portal.azure.com) naar **Alle services** en selecteer **Containerregisters**.
2. Selecteer uw register. De naam moet beginnen met **mlcr** en behoren tot de resourcegroep en locatie die, en het abonnement dat u hebt gebruikt om Module-beheer in te stellen.
3. Selecteer **Toegangssleutels**
4. Kopieer **Aanmeldingsserver**, **Gebruikersnaam** en **Wachtwoord**.  Deze hebt u nodig om het register van uw Edge-apparaten te openen.
5. Selecteer **Opslagplaatsen**
6. Selecteer **machinelearningmodule**
7. U hebt nu het volledige pad van de installatiekopie van de container. Noteer dit pad voor de volgende sectie. Het zou er als volgt uit moeten zien: **<registry_name>.azurecr.io/machinelearningmodule:1**

## <a name="deploy-to-your-device"></a>Uw apparaat implementeren

1. Ga in [Azure Portal](https://portal.azure.com) naar uw IoT-hub.

1. Ga naar **IoT Edge** en selecteer het IoT Edge-apparaat.

1. Selecteer **Modules instellen**.

1. Voeg in de sectie **Registerinstellingen** de referenties toe die u hebt gekopieerd uit het Azure-containerregister.

   ![Registerreferenties toevoegen aan manifest](./media/tutorial-deploy-machine-learning/registry-settings.png)

1. Als u de tempSensor-module eerder op uw IoT Edge-apparaat hebt geïmplementeerd, wordt het mogelijk automatisch ingevuld. Als die nog niet in uw lijst met modules staat, voeg deze dan toe.

    1. Klik op **Toevoegen** en selecteer **IoT Edge-module**.
    2. Voer in het veld **Naam** `tempSensor` in.
    3. Voer in het veld **URI installatiekopie** `mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0` in.
    4. Selecteer **Opslaan**.

1. Voeg de machine learning-module toe die u hebt gemaakt.

    1. Klik op **Toevoegen** en selecteer **IoT Edge-module**.
    1. Voer in het veld **Naam** `machinelearningmodule` in.
    1. Voer in het veld **Installatiekopie** het adres van uw installatiekopie in, bijvoorbeeld `<registry_name>.azurecr.io/machinelearningmodule:1`.
    1. Selecteer **Opslaan**.

1. Terug in de stap **Modules toevoegen** selecteert u **Volgende**.

1. Kopieer in de stap **Routes opgeven** de onderstaande JSON in het tekstvak. De eerste route transporteert berichten van de temperatuursensor naar de machine learning-module via het ‘amlInput’-eindpunt. Dit is het eindpunt dat alle Azure Machine Learning-modules gebruiken. De tweede route transporteert berichten van de Machine Learning- module naar IoT Hub. In deze route is ‘amlOutput’ het eindpunt dat alle Azure Machine Learning-modules gebruiken voor gegevens en ‘$upstream’ beschrijft IoT Hub.

    ```json
    {
        "routes": {
            "sensorToMachineLearning":"FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/machinelearningmodule/inputs/amlInput\")",
            "machineLearningToIoTHub": "FROM /messages/modules/machinelearningmodule/outputs/amlOutput INTO $upstream"
        }
    }
    ```

1. Selecteer **Volgende**.

1. Selecteer in de stap **Implementatie beoordelen** de optie **Verzenden**.

1. Ga terug naar de detailpagina van het apparaat en selecteer **Vernieuwen**.  U ziet nu dat de nieuwe **machinelearningmodule** samen met de **tempSensor**-module en de IoT Edge-runtime-modules wordt uitgevoerd.

## <a name="view-generated-data"></a>Gegenereerde gegevens weergeven

U kunt berichten weergeven die worden gegenereerd vanaf elke IoT Edge-module, en u kunt berichten weergeven die worden bezorgd bij de IoT Edge-hub.

### <a name="view-data-on-your-iot-edge-device"></a>Gegevens weergeven op het IoT Edge-apparaat

Op het IoT Edge-apparaat kunt u de berichten weergeven die worden verzonden vanaf elke afzonderlijke module.

Als u deze opdrachten uitvoert op een Linux-apparaat, moet u mogelijk `sudo` gebruiken voor verhoogde machtigingen.

1. Geef alle modules op het IoT Edge-apparaat weer.

   ```cmd/sh
   iotedge list
   ```

2. Geef de berichten weer die zijn verzonden vanaf een specifiek apparaat. Gebruik de modulenaam uit de uitvoer van de vorige opdracht.

   ```cmd/sh
   iotedge logs <module_name> -f
   ```

### <a name="view-data-arriving-at-your-iot-hub"></a>Gegevens weergeven die binnenkomen bij de IoT-hub

U kunt de apparaat-naar-cloud-berichten die de IoT-hub ontvangt, weergeven met behulp van de [Azure IoT Hub Toolkit-extensie voor Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) (voorheen Azure IoT Toolkit-extensie).

In de volgende stappen ziet u hoe u Visual Studio Code kunt instellen om apparaat-naar-cloud-berichten te controleren die binnenkomen bij de IoT-hub.

1. Selecteer in Visual Studio Code **IoT Hub-apparaten**.

2. Selecteer in het menu **...** en vervolgens **Set IoT Hub-verbindingsreeksen**.

   ![IoT Hub-verbindingsreeks instellen](./media/tutorial-deploy-machine-learning/set-connection.png)

3. Voer in het tekstvak dat bovenaan de pagina wordt geopend de iothubowner-verbindingsreeks in voor uw IoT Hub. Uw IoT Edge-apparaat moet worden weergegeven in de lijst met IoT Hub-apparaten.

4. Selecteer **...** opnieuw en vervolgens **Controle D2C-bericht starten**.

5. Bekijk de berichten die de tempSensor elke vijf seconden verzendt. De berichttekst bevat een eigenschap genaamd **afwijking** die de machinelearningmodule voorziet van een waarde ‘waar’ of ‘onwaar’. De eigenschap **AzureMLResponse** bevat de waarde ‘OK’ als het model is uitgevoerd.

   ![Reactie van Azure Machine Learning-service in de hoofdtekst van het bericht](./media/tutorial-deploy-machine-learning/ml-output.png)

## <a name="clean-up-resources"></a>Resources opschonen

Als u van plan bent door te gaan met het volgende aanbevolen artikel, kunt u de resources en configuraties die u hebt gemaakt behouden en opnieuw gebruiken. U kunt ook hetzelfde IoT Edge-apparaat blijven gebruiken als een testapparaat.

Anders kunt u de lokale configuraties en Azure-resources die u in dit artikel hebt gemaakt, verwijderen om kosten te voorkomen.

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

[!INCLUDE [iot-edge-clean-up-local-resources](../../includes/iot-edge-clean-up-local-resources.md)]


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een IoT Edge-module geïmplementeerd die werd aangedreven door Azure Machine Learning. U kunt doorgaan met elke andere zelfstudie om te leren waarmee Azure IoT Edge u nog meer kan helpen uw gegevens om te zetten in bedrijfsinzichten.

> [!div class="nextstepaction"]
> [Afbeeldingen classificeren met Custom Vision Service](tutorial-deploy-custom-vision.md)

