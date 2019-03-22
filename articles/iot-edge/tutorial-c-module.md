---
title: 'Zelfstudie: Aangepaste C-module maken - Azure IoT Edge | Microsoft Docs'
description: In deze zelfstudie ziet u hoe u een IoT Edge-module met C-code maakt en deze implementeert op een Edge-apparaat
services: iot-edge
author: shizn
manager: philmea
ms.author: xshi
ms.date: 01/04/2019
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc, seodec18
ms.openlocfilehash: 98406df3746bb0ca2fc658697ee25b1f11b54c0b
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58084586"
---
# <a name="tutorial-develop-a-c-iot-edge-module-and-deploy-to-your-simulated-device"></a>Zelfstudie: Een IoT Edge-module in C ontwikkelen en implementeren op uw gesimuleerde apparaat

U kunt IoT Edge-modules gebruiken voor het implementeren van code die uw bedrijfslogica rechtstreeks op uw IoT Edge-apparaten implementeert. In deze zelfstudie leert u een IoT Edge-module te maken die sensorgegevens filtert. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Visual Studio Code gebruiken om een IoT Edge-module in C te maken
> * Visual Studio Code en Docker gebruiken om een Docker-installatiekopie te maken en deze te publiceren in een containerregister
> * De module implementeren op uw IoT Edge-apparaat
> * Gegenereerde gegevens weergeven


De IoT Edge-module die u maakt in deze zelfstudie filtert de temperatuurgegevens die door uw apparaat worden gegenereerd. Er worden alleen gegevens upstream gezonden als de temperatuur boven een opgegeven drempelwaarde komt. Dit soort analyse is nuttig om de hoeveelheidgegevens die worden gecommuniceerd naar en opgeslagen in de cloud te reduceren.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Vereisten

Een Azure IoT Edge-apparaat:

* U kunt uw ontwikkelcomputer of een virtuele machine gebruiken als een Edge-apparaat door de stappen te volgen in de snelstart voor [Linux-](quickstart-linux.md) of [Windows-apparaten](quickstart.md). 
* C-modules voor Azure IoT Edge bieden geen ondersteuning voor Windows-containers. Als uw IoT Edge-apparaat een Windows-apparaat is, controleert u of dit is geconfigureerd om Linux-containers te kunnen gebruiken. Zie [De Azure IoT Edge-runtime installeren op Windows](how-to-install-iot-edge-windows.md) voor informatie over de verschillen in installatie bij Windows- en Linux-containers.

Cloudresources:

* Een gratis of standaard [IoT Hub](../iot-hub/iot-hub-create-through-portal.md)-laag in Azure.

Ontwikkelingsresources:

* [Visual Studio Code](https://code.visualstudio.com/).
* [C/C++-extensie](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools) voor Visual Studio Code (Engelstalig).
* [Azure IoT-hulpprogramma's](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) voor Visual Studio Code.
* [Docker CE](https://docs.docker.com/install/).

## <a name="create-a-container-registry"></a>Een containerregister maken

In deze zelfstudie gebruikt u de Azure IoT-hulpprogramma's voor Visual Studio Code om een module te bouwen en maakt u een **containerinstallatiekopie** van de bestanden. Vervolgens pusht u deze installatiekopie naar een **register** waarin uw installatiekopieën worden opgeslagen en beheerd. Tot slot implementeert u de installatiekopie uit het register voor uitvoering op uw IoT Edge-apparaat.

U kunt een Docker-register gebruiken om de containerinstallatiekopieën op te slaan. Twee populaire Docker-registerservices zijn [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) en [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). In deze zelfstudie wordt Azure Container Registry gebruikt.

Als u nog geen containerregister hebt, volgt u deze stappen om een nieuw containerregister te maken in Azure:

1. Selecteer in [Azure Portal](https://portal.azure.com) de optie **Een resource maken** > **Containers** > **Container Registry**.

2. Geef de volgende waarden op om uw containerregister te maken:

   | Veld | Value |
   | ----- | ----- |
   | Registernaam | Geef hier een unieke naam op. |
   | Abonnement | Selecteer een abonnement in de vervolgkeuzelijst. |
   | Resourcegroep | Het wordt aangeraden om dezelfde resourcegroep te gebruiken voor alle test-resources die u maakt met de snelstartgidsen en zelfstudies voor IoT Edge, zoals **IoTEdgeResources**. |
   | Locatie | Kies een locatie dicht bij u in de buurt. |
   | Beheerder | Stel deze optie in op **Inschakelen**. |
   | SKU | Selecteer **Basic**. |

5. Selecteer **Maken**.

6. Nadat het containerregister is gemaakt, bladert u ernaartoe en selecteert u vervolgens **Toegangssleutels**.

7. Kopieer de waarden voor **Aanmeldingsserver**, **Gebruikersnaam** en **wachtwoord**. U gebruikt deze waarden later in de zelfstudie om toegang te verlenen tot het containerregister.

## <a name="create-an-iot-edge-module-project"></a>Een IoT Edge-moduleproject creëren
In de volgende stappen ziet u hoe u een IoT Edge-moduleproject op basis van .NET core 2.0 maakt met behulp van Visual Studio Code en de Azure IoT-hulpprogramma's.

### <a name="create-a-new-solution"></a>Een nieuwe oplossing maken

Maak een C-oplossingssjabloon die u met uw eigen code kunt aanpassen.

1. Selecteer **View** > **Command Palette** om het VS Code-opdrachtpalet te openen.

2. Typ in het opdrachtenpalet de opdracht **Azure: Aanmelden** in, voer deze uit en volg de instructies om u aan te melden bij uw Azure-account. Als u zich al hebt aangemeld, kunt u deze stap overslaan.

3. Typ in het opdrachtpalet de opdracht **Azure IoT Edge: New IoT Edge solution** in en voer deze uit. Volg de aanwijzingen in het opdrachtpalet om uw oplossing te maken.

   | Veld | Value |
   | ----- | ----- |
   | Map selecteren | Kies de locatie op uw ontwikkelcomputer waar VS Code de oplossingsbestanden moet maken. |
   | Een naam opgeven voor de oplossing | Voer een beschrijvende naam voor de oplossing in of accepteer de standaardnaam **EdgeSolution**. |
   | Modulesjabloon selecteren | Kies **C Module**. |
   | Een modulenaam opgeven | Geef de module de naam **CModule**. |
   | Opslagplaats voor Docker-afbeeldingen voor de module opgeven | Een opslagplaats voor afbeeldingen bevat de naam van het containerregister en de naam van uw containerafbeelding. De containerafbeelding wordt vooraf gevuld vanuit de naam die u in de laatste stap hebt opgegeven. Vervang **localhost:5000** door de waarde van de aanmeldingsserver uit uw Azure-containerregister. U vindt de aanmeldingsserver op de overzichtspagina van het containerregister in de Azure-portal. <br><br> De uiteindelijke opslagplaats voor de installatiekopie ziet er ongeveer als volgt uit: \<registernaam\>.azurecr.io/cmodule. |
 
   ![Opslagplaats voor Docker-installatiekopieën opgeven](./media/tutorial-c-module/repository.png)

In het VS Code-venster wordt de werkruimte van de IoT Edge-oplossing geladen. De werkruimte voor de oplossing bevat vijf onderdelen op het hoogste niveau. De map **modules** bevat de C-code voor uw module evenals Docker-bestanden voor het bouwen van uw module als een containerinstallatiekopie. In het bestand **\.env** worden uw referenties voor het containerregister opgeslagen. Het bestand **deployment.template.json** bevat de gegevens die de IoT Edge-runtime gebruikt om modules op een apparaat te implementeren. En het bestand **deployment.debug.template.json** bevat de foutopsporingsversie van modules. In deze zelfstudie gaan we niet de map **\.vscode** of het bestand **\.gitignore** bewerken.

Als u geen containerregister hebt opgegeven bij het maken van uw oplossing, maar de standaardwaarde localhost:5000 hebt geaccepteerd, is er geen \.env-bestand gemaakt.

<!--
   ![C solution workspace](./media/tutorial-c-module/workspace.png)
-->

### <a name="add-your-registry-credentials"></a>Uw registerreferenties toevoegen

In het omgevingsbestand worden de referenties voor het containerregister opgeslagen. Deze referenties worden gedeeld met de IoT Edge-runtime. De runtime heeft deze referenties nodig om uw persoonlijke installatiekopieën naar het IoT Edge-apparaat te halen.

1. Open in VS Code Explorer het .env-bestand.
2. Werk de velden **gebruikersnaam** en **wachtwoord** bij met de waarden die u hebt gekopieerd uit het Azure-containerregister.
3. Sla dit bestand op.

### <a name="update-the-module-with-custom-code"></a>De module bijwerken met aangepaste code

Voeg code toe aan uw C-module zodat deze gegevens uit de sensor kan ophalen, controleert of de gemelde temperatuur van de computer een veilige drempelwaarde heeft overschreden en die informatie doorgeeft aan IoT Hub.

1. De gegevens van de sensor in dit scenario worden in JSON-indeling aangeleverd. Als u berichten wilt filteren in een JSON-indeling, moet u een JSON-bibliotheek voor C importeren. In deze zelfstudie wordt Parson gebruikt.

   1. Download de [Parson GitHub-opslagplaats](https://github.com/kgabis/parson). Kopieer de bestanden **parson.c** en **parson.h** naar de map **CModule**.

   2. Open **modules** > **CModule** > **CMakeLists.txt**. Importeer boven in het bestand de Parson-bestanden als een bibliotheek met de naam **my_parson**.

      ```
      add_library(my_parson
          parson.c
          parson.h
      )
      ```

   3. Voeg **my_parson** toe aan de lijst met bibliotheken in de functie **target_link_libraries** van CMakeLists.txt.

   4. Sla het bestand **CMakeLists.txt** op.

   5. Open **modules** > **CModule** > **main.c**. Onder aan de lijst met instructies, toevoegen van een nieuw account om op te nemen `parson.h` voor JSON-ondersteuning:

      ```c
      #include "parson.h"
      ```

1. Voeg in het bestand **main.c** na de sectie include een globale variabele toe met de naam `temperatureThreshold`. Deze variabele bepaalt de waarde die de gemeten temperatuur moet overschrijden voordat de gegevens naar IoT Hub worden verzonden.

    ```c
    static double temperatureThreshold = 25;
    ```

1. Vervang de volledige functie `CreateMessageInstance` door de volgende code. Deze functie wijst een context voor de callback toe.

    ```c
    static MESSAGE_INSTANCE* CreateMessageInstance(IOTHUB_MESSAGE_HANDLE message)
    {
        MESSAGE_INSTANCE* messageInstance = (MESSAGE_INSTANCE*)malloc(sizeof(MESSAGE_INSTANCE));
        if (NULL == messageInstance)
        {
            printf("Failed allocating 'MESSAGE_INSTANCE' for pipelined message\r\n");
        }
        else
        {
            memset(messageInstance, 0, sizeof(*messageInstance));

            if ((messageInstance->messageHandle = IoTHubMessage_Clone(message)) == NULL)
            {
                free(messageInstance);
                messageInstance = NULL;
            }
            else
            {
                messageInstance->messageTrackingId = messagesReceivedByInput1Queue;
                MAP_HANDLE propMap = IoTHubMessage_Properties(messageInstance->messageHandle);
                if (Map_AddOrUpdate(propMap, "MessageType", "Alert") != MAP_OK)
                {
                    printf("ERROR: Map_AddOrUpdate Failed!\r\n");
                }
            }
        }

        return messageInstance;
    }
    ```

1. Vervang de volledige functie `InputQueue1Callback` door de volgende code. Deze functie implementeert het feitelijke berichtenfilter.

    ```c
    static IOTHUBMESSAGE_DISPOSITION_RESULT InputQueue1Callback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
    {
        IOTHUBMESSAGE_DISPOSITION_RESULT result;
        IOTHUB_CLIENT_RESULT clientResult;
        IOTHUB_MODULE_CLIENT_LL_HANDLE iotHubModuleClientHandle = (IOTHUB_MODULE_CLIENT_LL_HANDLE)userContextCallback;

        unsigned const char* messageBody;
        size_t contentSize;

        if (IoTHubMessage_GetByteArray(message, &messageBody, &contentSize) != IOTHUB_MESSAGE_OK)
        {
            messageBody = "<null>";
        }

        printf("Received Message [%zu]\r\n Data: [%s]\r\n",
                messagesReceivedByInput1Queue, messageBody);

        JSON_Value *root_value = json_parse_string(messageBody);
        JSON_Object *root_object = json_value_get_object(root_value);
        double temperature;
        if (json_object_dotget_value(root_object, "machine.temperature") != NULL && (temperature = json_object_dotget_number(root_object, "machine.temperature")) > temperatureThreshold)
        {
            printf("Machine temperature %f exceeds threshold %f\r\n", temperature, temperatureThreshold);
            // This message should be sent to next stop in the pipeline, namely "output1".  What happens at "outpu1" is determined
            // by the configuration of the Edge routing table setup.
            MESSAGE_INSTANCE *messageInstance = CreateMessageInstance(message);
            if (NULL == messageInstance)
            {
                result = IOTHUBMESSAGE_ABANDONED;
            }
            else
            {
                printf("Sending message (%zu) to the next stage in pipeline\n", messagesReceivedByInput1Queue);

                clientResult = IoTHubModuleClient_LL_SendEventToOutputAsync(iotHubModuleClientHandle, messageInstance->messageHandle, "output1", SendConfirmationCallback, (void *)messageInstance);
                if (clientResult != IOTHUB_CLIENT_OK)
                {
                    IoTHubMessage_Destroy(messageInstance->messageHandle);
                    free(messageInstance);
                    printf("IoTHubModuleClient_LL_SendEventToOutputAsync failed on sending msg#=%zu, err=%d\n", messagesReceivedByInput1Queue, clientResult);
                    result = IOTHUBMESSAGE_ABANDONED;
                }
                else
                {
                    result = IOTHUBMESSAGE_ACCEPTED;
                }
            }
        }
        else
        {
            printf("Not sending message (%zu) to the next stage in pipeline.\r\n", messagesReceivedByInput1Queue);
            result = IOTHUBMESSAGE_ACCEPTED;
        }

        messagesReceivedByInput1Queue++;
        return result;
    }
    ```

1. Voeg een `moduleTwinCallback`-functie toe. Deze methode ontvangt updates van de gewenste eigenschappen van de moduledubbel en updatet de variabele **temperatureThreshold** naar dezelfde waarde. Alle modules hebben hun eigen moduledubbel, waardoor u rechtstreeks in de cloud de code kunt configureren die in een module wordt uitgevoerd.

    ```c
    static void moduleTwinCallback(DEVICE_TWIN_UPDATE_STATE update_state, const unsigned char* payLoad, size_t size, void* userContextCallback)
    {
        printf("\r\nTwin callback called with (state=%s, size=%zu):\r\n%s\r\n",
            ENUM_TO_STRING(DEVICE_TWIN_UPDATE_STATE, update_state), size, payLoad);
        JSON_Value *root_value = json_parse_string(payLoad);
        JSON_Object *root_object = json_value_get_object(root_value);
        if (json_object_dotget_value(root_object, "desired.TemperatureThreshold") != NULL) {
            temperatureThreshold = json_object_dotget_number(root_object, "desired.TemperatureThreshold");
        }
        if (json_object_get_value(root_object, "TemperatureThreshold") != NULL) {
            temperatureThreshold = json_object_get_number(root_object, "TemperatureThreshold");
        }
    }
    ```

1. Vervang de functie `SetupCallbacksForModule` door de volgende code.

   ```c
   static int SetupCallbacksForModule(IOTHUB_MODULE_CLIENT_LL_HANDLE iotHubModuleClientHandle)
   {
       int ret;

       if (IoTHubModuleClient_LL_SetInputMessageCallback(iotHubModuleClientHandle, "input1", InputQueue1Callback, (void*)iotHubModuleClientHandle) != IOTHUB_CLIENT_OK)
       {
           printf("ERROR: IoTHubModuleClient_LL_SetInputMessageCallback(\"input1\")..........FAILED!\r\n");
           ret = __FAILURE__;
       }
       else if (IoTHubModuleClient_LL_SetModuleTwinCallback(iotHubModuleClientHandle, moduleTwinCallback, (void*)iotHubModuleClientHandle) != IOTHUB_CLIENT_OK)
       {
           printf("ERROR: IoTHubModuleClient_LL_SetModuleTwinCallback(default)..........FAILED!\r\n");
           ret = __FAILURE__;
       }
       else
       {
           ret = 0;
       }

       return ret;
   }
   ```

1. Sla het bestand main.c op.

1. Open in VS Code Explorer het bestand **deployment.template.json** in de werkruimte van de IoT Edge-oplossing. Dit bestand laat aan de IoT Edge-agent weten welke modules moeten worden geïmplementeerd. In dit geval gaat het om **tempSensor** en **CModule**. Het bestand laat de IoT Edge-hub ook weten hoe berichten tussen de modules moeten worden gerouteerd. De Visual Studio Code-extensie vult automatisch het overgrote deel van de informatie in die u nodig hebt in de implementatiesjabloon. Controleer echter wel of alles klopt voor uw oplossing: 

   1. Het standaardplatform van uw IoT Edge-apparaat is ingesteld op **amd64** in de VS Code-statusbalk. Dit betekent dat **CModule** is ingesteld op de Linux amd64-versie van de installatiekopie. Wijzig in de statusbalk het standaardplatform van **amd64** in **arm32v7** als dit de architectuur van het IoT Edge-apparaat is. 

      ![Het installatiekopieplatform van de module bijwerken](./media/tutorial-c-module/image-platform.png)

   2. Controleer of de sjabloon de juiste modulenaam heeft, niet de **SampleModule**-standaardnaam die u hebt gewijzigd tijdens het maken van de IoT Edge-oplossing.

   3. In de sectie **registryCredentials** worden uw Docker-registerreferenties opgeslagen zodat de IoT Edge-agent uw module-installatiekopie kan ophalen. De actuele gebruikersnaam en het bijbehorende wachtwoord zijn opgeslagen in het .env-bestand, dat wordt genegeerd door Git. Voeg uw referenties toe aan het ENV-bestand als u dat nog niet hebt gedaan.  

   4. Als u meer informatie wilt over distributiemanifesten, gaat u naar [Meer informatie over het implementeren van modules en het instellen van routes in IoT Edge](module-composition.md).

1. Voeg de moduledubbel CModule toe aan het distributiemanifest. Voeg de volgende JSON-inhoud onder aan de sectie `moduleContent` in, na de moduledubbel `$edgeHub`:

   ```json
       "CModule": {
           "properties.desired":{
               "TemperatureThreshold":25
           }
       }
   ```

   ![Dubbele CModule toevoegen aan implementatiesjabloon](./media/tutorial-c-module/module-twin.png)

1. Sla het bestand **deployment.template.json** op.

## <a name="build-and-push-your-solution"></a>De oplossing bouwen en pushen

In de vorige sectie hebt u een IoT Edge-oplossing gemaakt en code toegevoegd aan de CModule waarmee berichten worden gefilterd waarin de gemelde temperatuur van de computer binnen de aanvaardbare grenzen is. Nu moet u de oplossing bouwen als een containerinstallatiekopie en deze naar het containerregister pushen.

1. Open de met VS Code geïntegreerde terminal door **View** > **Integrated Terminal** te selecteren.

1. Meld u aan bij Docker door de volgende opdracht in de geïntegreerde Visual Studio Code-terminal in te voeren. U moet u aanmelden met uw referenties voor Azure Container Registry zodat u de installatiekopie van de module naar het register kunt pushen.
     
   ```csh/sh
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```
   Gebruik de gebruikersnaam, het wachtwoord en de aanmeldingsserver die u in de eerste sectie hebt gekopieerd uit het Azure-containerregister. Of haal deze opnieuw op in de sectie **Toegangssleutels** van het register in Azure Portal.

2. Klik in VS Code Explorer met de rechtermuisknop op het bestand **deployment.template.json** en selecteer **Build and Push IoT Edge solution**.

Wanneer u Visual Studio Code de opdracht geeft om uw oplossing te bouwen, wordt eerst een `deployment.json`-bestand gemaakt in een nieuwe **configuratiemap**. De informatie voor het bestand deployment.json wordt verzameld uit het sjabloonbestand dat u hebt bijgewerkt, het .env-bestand dat u hebt gebruikt voor het opslaan van de referenties van uw containerregister en het bestand module.json in de map CModule.

Vervolgens worden door Visual Studio Code twee opdrachten uitgevoerd in de geïntegreerde terminal: `docker build` en `docker push`. Met deze twee opdrachten wordt uw code gebouwd, `CModule.dll` opgeslagen in een container, en vervolgens naar het containerregister gepusht dat u hebt opgegeven toen u de oplossing initialiseerde.

U kunt het volledige adres van de containerinstallatiekopie, inclusief de tag, zien in de geïntegreerde terminal van VS Code. Het adres van de installatiekopie is opgebouwd uit informatie uit het `module.json`-bestand, in de indeling **\<opslagplaats\>:\<versie\>-\<platform\>**. Voor deze zelfstudie ziet dit er als volgt uit: **myregistry.azurecr.io/cmodule:0.0.1-amd64**.

>[!TIP]
>Als u een foutmelding krijgt bij het bouwen en pushen van de module, controleert u het volgende:
>* Hebt u zich bij Docker in Visual Studio Code aangemeld met de referenties uit uw containerregister? Deze referenties zijn anders dan de referenties die u gebruikt om u aan te melden bij de Azure Portal.
>* Hebt u de juiste containeropslagplaats? Open **modules** > **cmodule** > **module.json** en zoek het veld **opslagplaats**. De opslagplaats voor de installatiekopie ziet er ongeveer uit als **\<registryname\>.azurecr.io/cmodule**. 
>* Bouwt u hetzelfde type containers dat door uw ontwikkelcomputer wordt uitgevoerd? Visual Studio Code wordt teruggezet op de Linux amd64-standaardcontainers. Als op uw ontwikkelcomputer Linux arm32v7-containers worden uitgevoerd, werkt u het platform bij op de blauwe statusbalk onder aan het Visual Studio Code-venster, zodat dit overeenkomt met uw containerplatform. C-modules kunnen niet worden gebouwd als Windows-containers. 

## <a name="deploy-and-run-the-solution"></a>De oplossing implementeren en uitvoeren

In dit snelstartartikel voor het instellen van uw IoT Edge-apparaat hebt u een module geïmplementeerd met behulp van de Azure-portal. U kunt modules ook implementeren via de Azure IoT Hub Toolkit-extensie (voorheen Azure IoT Toolkit-extensie) voor Visual Studio Code. U hebt al een implementatiemanifest voorbereid voor uw scenario, namelijk het bestand **deployment.json**. U hoeft nu alleen nog maar een apparaat te selecteren dat de implementatie moet ontvangen.

1. Voer in het opdrachtpalet van VS Code de opdracht **Azure IoT Hub: Select IoT Hub**.

2. Kies het abonnement en de IoT-hub met het IoT Edge-apparaat dat u wilt configureren.

3. Vouw in VS Code Explorer de sectie **Azure IoT Hub Devices** uit.

4. Klik met de rechtermuisknop op de naam van het IoT Edge-apparaat en selecteer **Implementatie voor één apparaat maken**.

   ![Implementatie voor één apparaat maken](./media/tutorial-c-module/create-deployment.png)

5. Selecteer het bestand **deployment.json** in de **configuratiemap** en klik vervolgens op **Edge-distributiemanifest selecteren**. Gebruik niet het bestand deployment.template.json.

6. Klik op de knop Vernieuwen. U ziet nu dat de nieuwe **CModule** wordt uitgevoerd, samen met de module **TempSensor** en de modules **$edgeAgent** en **$edgeHub**.

## <a name="view-generated-data"></a>Gegenereerde gegevens weergeven

Als u het implementatiemanifest op uw IoT Edge-apparaat toepast, verzamelt de IoT Edge-runtime op het apparaat de informatie over de nieuwe implementatie en wordt deze uitgevoerd. Modules die worden uitgevoerd op het apparaat en die niet zijn opgenomen in het implementatiemanifest, worden gestopt. Alle modules die ontbreken op het apparaat worden gestart.

U kunt de status van uw IoT Edge-apparaat bekijken via de sectie **Azure IoT Hub Devices** van de Visual Studio Code explorer. Vouw de details van uw apparaat uit voor een overzicht van de modules die worden geïmplementeerd en uitgevoerd.

Op het IoT Edge-apparaat kunt u de status van uw implementatiemodules bekijken met behulp van de opdracht `iotedge list`. U ziet vier modules: de twee modules van de IoT Edge-runtime, tempSensor en de aangepaste module die u in deze zelfstudie hebt gemaakt. Het kan een paar minuten duren voordat alle modules zijn gestart, dus voer de opdracht opnieuw uit als u ze in eerste instantie niet allemaal ziet.

Als u de berichten die worden gegenereerd door een module wilt weergeven, gebruikt u de opdracht `iotedge logs <module name>`.

U kunt de berichten weergeven wanneer ze binnenkomen op uw IoT-hub met behulp van Visual Studio Code.

1. Klik op **...** en selecteer **Controle D2C-berichten starten** om de gegevens te controleren die binnenkomen bij de IoT-hub.
2. Als u de D2C-berichten voor een specifiek apparaat wilt controleren, klikt u met de rechtermuisknop op dit apparaat in de lijst en selecteert u **Controle D2C-berichten starten**.
3. Als u wilt stoppen met het bewaken van gegevens, voert u de opdracht **Azure IoT Hub: Stop monitoring D2C message** (Controle D2C-bericht stoppen) en voer deze uit.
4. Als u de moduledubbel wilt weergeven of bewerken, klikt u met de rechtermuisknop op deze moduledubbel in de lijst en selecteert u **Moduledubbel bewerken**. Als u de moduledubbel wilt bijwerken, slaat u het dubbele JSON-bestand op, klikt u met de rechtermuisknop in de editor en selecteert u **Moduledubbel bijwerken**.
5. Als u Docker-logboeken wilt bekijken, kunt u [Docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) voor VS Code installeren. U kunt de actieve modules lokaal zoeken in Docker Explorer. Klik in het contextmenu op **Logboeken weergeven** om ze te bekijken in de geïntegreerde terminal.

## <a name="clean-up-resources"></a>Resources opschonen

Als u van plan bent door te gaan met het volgende aanbevolen artikel, kunt u de resources en configuraties die u hebt gemaakt behouden en opnieuw gebruiken. U kunt ook hetzelfde IoT Edge-apparaat blijven gebruiken als een testapparaat.

Anders kunt u de lokale configuraties en Azure-resources die u in dit artikel hebt gemaakt, verwijderen om kosten te voorkomen.

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

[!INCLUDE [iot-edge-clean-up-local-resources](../../includes/iot-edge-clean-up-local-resources.md)]


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een IoT Edge-module gemaakt die code bevat voor het filteren van onbewerkte gegevens die worden gegenereerd door uw IoT Edge-apparaat. Als u klaar bent om uw eigen modules te bouwen, kunt u meer informatie krijgen over het [ontwikkelen van een C-module met Azure IoT Edge voor Visual Studio Code](how-to-develop-c-module.md). U kunt verdergaan met de volgende zelfstudies om te leren hoe Azure IoT Edge u nog meer kan helpen bij het omzetten van uw gegevens in bedrijfsinzichten.

> [!div class="nextstepaction"]
> [Gegevens opslaan met SQL Server-databases](tutorial-store-data-sql-server.md)

