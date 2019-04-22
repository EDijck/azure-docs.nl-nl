---
title: Aangepaste module zelfstudie over Java - Azure IoT Edge | Microsoft Docs
description: In deze zelfstudie ziet u hoe u een IoT Edge-module met Java-code maakt en deze implementeert op een Edge-apparaat.
services: iot-edge
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 04/04/2019
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc, seodec18
ms.openlocfilehash: f654f33fe03b29a3aa93386d49e8f5a43cffc9c8
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2019
ms.locfileid: "59493039"
---
# <a name="tutorial-develop-a-java-iot-edge-module-and-deploy-to-your-simulated-device"></a>Zelfstudie: een IoT Edge-module in Java ontwikkelen en implementeren op uw gesimuleerde apparaat

U kunt Azure IoT Edge-modules gebruiken voor het implementeren van code die uw bedrijfslogica rechtstreeks op uw IoT Edge-apparaten implementeert. In deze zelfstudie leert u een IoT Edge-module te maken die sensorgegevens filtert. U gebruikt de gesimuleerde IoT Edge-apparaat dat u hebt gemaakt in de implementatie van Azure IoT Edge op een gesimuleerd apparaat in [Linux](quickstart-linux.md) Quick Start. In deze zelfstudie leert u het volgende:    

> [!div class="checklist"]
> * Visual Studio Code gebruiken om een ​​IoT Edge Java-module te maken op basis van het Azure IoT Edge maven-sjabloonpakket en Azure IoT Java-apparaat-SDK.
> * Visual Studio Code en Docker gebruiken om een Docker-installatiekopie te maken en deze te publiceren naar het register.
> * De module implementeren op uw IoT Edge-apparaat.
> * Gegenereerde gegevens weergeven.


De IoT Edge-module die u maakt in deze zelfstudie filtert de temperatuurgegevens die door uw apparaat worden gegenereerd. Er worden alleen gegevens upstream gezonden als de temperatuur boven een opgegeven drempelwaarde komt. Dit soort analyse is nuttig om de hoeveelheid gegevens te reduceren die worden gecommuniceerd naar en worden opgeslagen in de cloud. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Vereisten

Een Azure IoT Edge-apparaat:

* U kunt een virtuele machine van Azure gebruiken als een IoT Edge-apparaat met de volgende stappen in de Quick Start voor [Linux](quickstart-linux.md). 
* Java-modules voor IoT Edge bieden geen ondersteuning voor Windows-containers. 

Cloudresources:

* Een gratis of standaard [IoT Hub](../iot-hub/iot-hub-create-through-portal.md)-laag in Azure. 

Ontwikkelingsresources:

* [Visual Studio Code](https://code.visualstudio.com/). 
* [Java-uitbreidingspakket](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-pack) voor Visual Studio Code.
* [Azure IoT-hulpprogramma's](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) voor Visual Studio Code. 
* [Java SE Development Kit 10](https://aka.ms/azure-jdks); en [stel de omgevingsvariabele `JAVA_HOME` in](https://docs.oracle.com/cd/E19182-01/820-7851/inst_cli_jdk_javahome_t/) om te verwijzen naar uw JDK-installatie.
* [Maven](https://maven.apache.org/)
* [Docker CE](https://docs.docker.com/install/)
   * Als u op een Windows-apparaat ontwikkelt, moet u ervoor zorgen dat Docker is [geconfigureerd voor het gebruik van Linux-containers](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers). 


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
   | Resourcegroep | Gebruik voor eenvoudiger beheer dezelfde resourcegroep voor alle testresources die u maakt met de quickstarts en zelfstudies voor IoT Edge. zoals **IoTEdgeResources**. |
   | Locatie | Kies een locatie dicht bij u in de buurt. |
   | Beheerder | Stel deze optie in op **Inschakelen**. |
   | SKU | Selecteer **Basic**. | 

5. Selecteer **Maken**.

6. Nadat het containerregister is gemaakt, bladert u ernaartoe en selecteert u vervolgens **Toegangssleutels**. 

7. Kopieer de waarden voor **Aanmeldingsserver**, **Gebruikersnaam** en **wachtwoord**. U gebruikt deze waarden later in de zelfstudie om toegang te verlenen tot het containerregister. 

## <a name="create-an-iot-edge-module-project"></a>Een IoT Edge-moduleproject creëren
Met de volgende stappen maakt u een ​​IoT Edge-moduleproject op basis van het Azure IoT Edge maven-sjabloonpakket en de Azure IoT Java-apparaat-SDK. U kunt het project maken met behulp van Visual Studio Code en de Azure IoT-hulpprogramma's.

### <a name="create-a-new-solution"></a>Een nieuwe oplossing maken

Maak een Java-oplossingssjabloon die u met uw eigen code kunt aanpassen. 

1. Selecteer in Visual Studio Code **View** > **Command Palette** om het VS Code-opdrachtpalet te openen. 

2. Voer in het opdrachtpalet de opdracht **Azure IoT Edge: New IoT Edge solution** in en voer deze uit. Volg de aanwijzingen in het opdrachtpalet om uw oplossing te maken.

   | Veld | Value |
   | ----- | ----- |
   | Map selecteren | Kies de locatie op uw ontwikkelcomputer waar VS Code de oplossingsbestanden moet maken. |
   | Een naam opgeven voor de oplossing | Voer een beschrijvende naam voor de oplossing in of accepteer de standaardnaam **EdgeSolution**. |
   | Modulesjabloon selecteren | Kies **Java Module**. |
   | Waarde opgeven voor groupId | Geef een waarde op voor de groeps-id of accepteer de standaardwaarde **com.edgemodule**. |
   | Een modulenaam opgeven | Geef de module de naam **JavaModule**. |
   | Opslagplaats voor Docker-afbeeldingen voor de module opgeven | Een opslagplaats voor afbeeldingen bevat de naam van het containerregister en de naam van uw containerafbeelding. De containerafbeelding wordt vooraf gevuld vanuit de naam die u in de laatste stap hebt opgegeven. Vervang **localhost:5000** door de waarde van de aanmeldingsserver uit uw Azure-containerregister. U vindt de aanmeldingsserver op de overzichtspagina van het containerregister in de Azure-portal. <br><br>De uiteindelijke opslagplaats voor de installatiekopie ziet er ongeveer als volgt uit: \<registernaam\>.azurecr.io/javamodule. |
 
   ![Opslagplaats voor Docker-installatiekopieën opgeven](./media/tutorial-java-module/repository.png)
   
Als dit de eerste keer is dat u een Java-module maakt, kan het enkele minuten duren om maven-pakketten te downloaden. Vervolgens wordt de werkruimte van de IoT Edge-oplossing geladen in het VS Code-venster. De werkruimte voor de oplossing bevat vijf onderdelen op het hoogste niveau. De map **modules** bevat de Java-code voor uw module evenals Docker-bestanden voor het bouwen van uw module als een containerinstallatiekopie. In het bestand **\.env** worden uw referenties voor het containerregister opgeslagen. Het bestand **deployment.template.json** bevat de gegevens die de IoT Edge-runtime gebruikt om modules op een apparaat te implementeren. En het bestand **deployment.debug.template.json** bevat de foutopsporingsversie van modules. In deze zelfstudie gaan we niet de map **\.vscode** of het bestand **\.gitignore** bewerken. 

Als u geen containerregister hebt opgegeven bij het maken van uw oplossing, maar de standaardwaarde localhost:5000 hebt geaccepteerd, is er geen \.env-bestand gemaakt. 

### <a name="add-your-registry-credentials"></a>Uw registerreferenties toevoegen

In het omgevingsbestand worden de referenties voor het containerregister opgeslagen. Deze referenties worden gedeeld met de IoT Edge-runtime. De runtime heeft deze referenties nodig om uw persoonlijke installatiekopieën naar het IoT Edge-apparaat te halen. 

1. Open in VS Code Explorer het .env-bestand. 
2. Werk de velden **gebruikersnaam** en **wachtwoord** bij met de waarden die u hebt gekopieerd uit het Azure-containerregister. 
3. Sla dit bestand op. 

### <a name="update-the-module-with-custom-code"></a>De module bijwerken met aangepaste code

1. Open in de VS Code explorer **modules** > **JavaModule** > **src** > **main** > **java** > **com** > **edgemodule** > **App.java**.

5. Voeg bovenaan het bestand de volgende code toe om nieuwe klassen waarnaar wordt verwezen, te importeren.

    ```java
    import java.io.StringReader;
    import java.util.concurrent.atomic.AtomicLong;
    import java.util.HashMap;
    import java.util.Map;
    
    import javax.json.Json;
    import javax.json.JsonObject;
    import javax.json.JsonReader;
    
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.Pair;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.Property;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.TwinPropertyCallBack;
    ```

5. Voeg de volgende definitie toe aan de klasse **App**. Met deze variabele wordt een drempelwaarde voor temperatuur ingesteld. De gemeten machinetemperatuur wordt niet bij IoT Hub gerapporteerd totdat deze waarde wordt overschreden. 

    ```java
    private static final String TEMP_THRESHOLD = "TemperatureThreshold";
    private static AtomicLong tempThreshold = new AtomicLong(25);
    ```

7. Vervang de execute-methode van **MessageCallbackMqtt** door de volgende code. Deze methode wordt aangeroepen wanneer de module een MQTT-bericht ontvangt van de IoT Edge-hub. Berichten die temperaturen onder de drempelwaarde die via de moduledubbel is ingesteld, worden hierdoor gefilterd.

    ```java
    protected static class MessageCallbackMqtt implements MessageCallback {
        private int counter = 0;
        @Override
        public IotHubMessageResult execute(Message msg, Object context) {
            this.counter += 1;
 
            String msgString = new String(msg.getBytes(), Message.DEFAULT_IOTHUB_MESSAGE_CHARSET);
            System.out.println(
                   String.format("Received message %d: %s",
                            this.counter, msgString));
            if (context instanceof ModuleClient) {
                try (JsonReader jsonReader = Json.createReader(new StringReader(msgString))) {
                    final JsonObject msgObject = jsonReader.readObject();
                    double temperature = msgObject.getJsonObject("machine").getJsonNumber("temperature").doubleValue();
                    long threshold = App.tempThreshold.get();
                    if (temperature >= threshold) {
                        ModuleClient client = (ModuleClient) context;
                        System.out.println(
                            String.format("Temperature above threshold %d. Sending message: %s",
                            threshold, msgString));
                        client.sendEventAsync(msg, eventCallback, msg, App.OUTPUT_NAME);
                    }
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
            return IotHubMessageResult.COMPLETE;
        }
    }
    ```

8. Voeg de volgende twee statische binnenste klassen toe aan de klasse **App**. Met deze klassen wordt de variabele tempThreshold bijgewerkt wanneer de gewenste eigenschap van de moduledubbel verandert. Alle modules hebben hun eigen moduledubbel, waardoor u rechtstreeks in de cloud de code kunt configureren die in een module wordt uitgevoerd.

    ```java
    protected static class DeviceTwinStatusCallBack implements IotHubEventCallback {
        @Override
        public void execute(IotHubStatusCode status, Object context) {
            System.out.println("IoT Hub responded to device twin operation with status " + status.name());
        }
    }
 
    protected static class OnProperty implements TwinPropertyCallBack {
        @Override
        public void TwinPropertyCallBack(Property property, Object context) {
            if (!property.getIsReported()) {
                if (property.getKey().equals(App.TEMP_THRESHOLD)) {
                    try {
                        long threshold = Math.round((double) property.getValue());
                        App.tempThreshold.set(threshold);
                    } catch (Exception e) {
                        System.out.println("Faile to set TemperatureThread with exception");
                        e.printStackTrace();
                    }
                }
            }
        }
    }
    ```

9. Voeg de volgende regels toe aan de module **main**, na **client.open()**, om u te abonneren op de updates van de moduledubbel.

    ```java
    client.startTwin(new DeviceTwinStatusCallBack(), null, new OnProperty(), null);
    Map<Property, Pair<TwinPropertyCallBack, Object>> onDesiredPropertyChange = new HashMap<Property, Pair<TwinPropertyCallBack, Object>>() {
        {
            put(new Property(App.TEMP_THRESHOLD, null), new Pair<TwinPropertyCallBack, Object>(new OnProperty(), null));
        }
    };
    client.subscribeToTwinDesiredProperties(onDesiredPropertyChange);
    client.getTwin();
    ```

11. Sla het App.java-bestand op.

12. Open in VS Code Explorer het bestand **deployment.template.json** in de werkruimte van de IoT Edge-oplossing. Dit bestand vertelt de IoT Edge agent welke modules te implementeren, en de IoT Edge hub vertelt over het routeren van berichten tussen deze. In dit geval de twee modules zijn **tempSensor** en **JavaModule**. De Visual Studio Code-extensie vult automatisch het overgrote deel van de informatie in die u nodig hebt in de implementatiesjabloon. Controleer echter wel of alles klopt voor uw oplossing: 

   1. Het standaardplatform van uw IoT Edge is ingesteld op **amd64** in de statusbalk van uw VS Code. Dat betekent dat uw **JavaModule** is ingesteld op Linux amd64-versie van de installatiekopie. Wijzig in de statusbalk het standaardplatform van **amd64** in **arm32v7** als dit de architectuur van het IoT Edge-apparaat is. 

      ![Het installatiekopieplatform van de module bijwerken](./media/tutorial-java-module/image-platform.png)

   2. Controleer of de sjabloon de juiste modulenaam heeft, niet de **SampleModule**-standaardnaam die u hebt gewijzigd tijdens het maken van de IoT Edge-oplossing.

   3. In de sectie **registryCredentials** worden uw Docker-registerreferenties opgeslagen zodat de IoT Edge-agent uw module-installatiekopie kan ophalen. De actuele gebruikersnaam en het bijbehorende wachtwoord zijn opgeslagen in het .env-bestand, dat wordt genegeerd door Git. Voeg uw referenties toe aan het ENV-bestand als u dat nog niet hebt gedaan.  

   4. Als u meer informatie wilt over distributiemanifesten, gaat u naar [Meer informatie over het implementeren van modules en het instellen van routes in IoT Edge](module-composition.md).

13. Voeg de moduledubbel **JavaModule** toe aan het distributiemanifest. Voeg de volgende JSON-inhoud onderaan de sectie **moduleContent** in, na de moduledubbel **$edgeHub**: 

   ```json
     "JavaModule": {
         "properties.desired":{
             "TemperatureThreshold":25
         }
     }
   ```

   ![Moduledubbel toevoegen aan implementatiesjabloon](./media/tutorial-java-module/module-twin.png)

14. Sla het bestand deployment.template.json op.

## <a name="build-your-iot-edge-solution"></a>Uw eigen IoT Edge-oplossing bouwen

In het vorige gedeelte hebt u een IoT Edge-oplossing gemaakt en code toegevoegd aan de **JavaModule** om berichten te filteren waarin de gemelde temperatuur van de machine onder de aanvaardbare limiet is. Nu gaat u de oplossing bouwen als een containerinstallatiekopie en deze naar het containerregister pushen. 

1. Meld u aan bij Docker door de volgende opdracht in de Visual Studio Code-terminal in te voeren. Vervolgens kunt u de module-installatiekopie naar het Azure-containerregister pushen.
     
   ```csh/sh
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```
   Gebruik de gebruikersnaam, het wachtwoord en de aanmeldingsserver die u in de eerste sectie hebt gekopieerd uit het Azure-containerregister. U kunt deze waarden ook ophalen in de sectie **Toegangssleutels** van het register in de Azure-portal.

2. Klik in VS Code Explorer met de rechtermuisknop op het bestand deployment.template.json en selecteer **Build and Push IoT Edge solution**. 

Wanneer u Visual Studio Code de opdracht geeft om uw oplossing te bouwen, wordt eerst een bestand deployment.json gemaakt in een nieuwe map genaamd **config** op basis van de informatie in de distributiesjabloon. Vervolgens worden twee opdrachten uitgevoerd in de geïntegreerde terminal: `docker build` en `docker push`. Met deze twee opdrachten wordt uw code gebouwd, wordt de Java-app in een container opgeslagen en wordt de code vervolgens naar het containerregister gepusht dat u hebt opgegeven toen u de oplossing initialiseerde. 

U kunt het volledige adres van de containerinstallatiekopie, inclusief de tag, zien in de geïntegreerde terminal van VS Code. Het adres van de installatiekopie is opgebouwd uit informatie uit het bestand module.json, in de indeling \<opslagplaats\>:\<versie\>-\<platform\>. Voor deze zelfstudie ziet dit adres er als volgt uit: registryname.azurecr.io/javamodule:0.0.1-amd64.

>[!TIP]
>Als u een foutmelding krijgt bij het bouwen en pushen van de module, controleert u het volgende:
>* Hebt u zich bij Docker in Visual Studio Code aangemeld met de referenties uit uw containerregister? Deze referenties zijn anders dan de referenties die u gebruikt om u aan te melden bij de Azure Portal.
>* Hebt u de juiste containeropslagplaats? Open **modules** > **JavaModule** > **module.json** en zoek de **opslagplaats** veld. De opslagplaats voor de installatiekopie ziet er ongeveer uit als **\<registryname\>.azurecr.io/javamodule**. 
>* Bouwt u hetzelfde type containers dat door uw ontwikkelcomputer wordt uitgevoerd? Visual Studio Code wordt teruggezet op de Linux amd64-standaardcontainers. Als op uw ontwikkelcomputer Linux arm32v7-containers worden uitgevoerd, werkt u het platform bij op de blauwe statusbalk onder aan het Visual Studio Code-venster, zodat dit overeenkomt met uw containerplatform.
>* Java-modules voor IoT Edge bieden geen ondersteuning voor Windows-containers.

## <a name="deploy-and-run-the-solution"></a>De oplossing implementeren en uitvoeren

In dit snelstartartikel voor het instellen van uw IoT Edge-apparaat hebt u een module geïmplementeerd met behulp van de Azure-portal. U kunt modules ook implementeren via de Azure IoT Hub Toolkit-extensie (voorheen Azure IoT Toolkit-extensie) voor Visual Studio Code. U hebt al een implementatiemanifest voorbereid voor uw scenario, namelijk het bestand **deployment.json**. U hoeft nu alleen nog maar een apparaat te selecteren dat de implementatie moet ontvangen.

1. Voer in het opdrachtpalet van VS Code de opdracht **Azure: Aanmelden** in, voer deze uit en volg de instructies om u aan te melden bij uw Azure-account. Als u al bent aangemeld, kunt u deze stap overslaan.

2. Voer in het opdrachtpalet van VS Code de opdracht **Azure IoT Hub: Select IoT Hub**. 

3. Kies het abonnement en de IoT-hub met het IoT Edge-apparaat dat u wilt configureren. 

4. Vouw in VS Code Explorer de sectie **Azure IoT Hub Devices** uit. 

5. Klik met de rechtermuisknop op de naam van het IoT Edge-apparaat en selecteer **Implementatie voor één apparaat maken**. 

   ![Implementatie voor één apparaat maken](./media/tutorial-java-module/create-deployment.png)

6. Selecteer het bestand **deployment.json** in de **configuratiemap** en klik vervolgens op **Edge-distributiemanifest selecteren**. Gebruik niet het bestand deployment.template.json. 

7. Klik op de knop Vernieuwen. U ziet nu dat de nieuwe **JavaModule** wordt uitgevoerd, samen met de module **TempSensor** en de modules **$edgeAgent** en **$edgeHub**.  

## <a name="view-generated-data"></a>Gegenereerde gegevens weergeven

Als u het implementatiemanifest op uw IoT Edge-apparaat toepast, verzamelt de IoT Edge-runtime op het apparaat de informatie over de nieuwe implementatie en wordt deze uitgevoerd. Modules die worden uitgevoerd op het apparaat en die niet zijn opgenomen in het implementatiemanifest, worden gestopt. Alle modules die ontbreken op het apparaat worden gestart. 

U kunt de status van uw IoT Edge-apparaat bekijken via de sectie **Azure IoT Hub Devices** van de Visual Studio Code explorer. Vouw de details van uw apparaat uit voor een overzicht van de modules die worden geïmplementeerd en uitgevoerd. 

Op het IoT Edge-apparaat kunt u de status van uw implementatiemodules bekijken met behulp van de opdracht `iotedge list`. U ziet vier modules: de twee modules van de IoT Edge-runtime, tempSensor en de aangepaste module die u in deze zelfstudie hebt gemaakt. Het kan een paar minuten duren voordat alle modules zijn gestart, dus voer de opdracht opnieuw uit als u ze in eerste instantie niet allemaal ziet. 

Als u de berichten die worden gegenereerd door een module wilt weergeven, gebruikt u de opdracht `iotedge logs <module name>`. 

U kunt de berichten weergeven wanneer ze binnenkomen op uw IoT-hub met behulp van Visual Studio Code. 

1. Klik op het weglatingsteken (**...**) en selecteer **Controle D2C-berichten starten** om de gegevens te controleren die binnenkomen bij de IoT-hub.
2. Als u de D2C-berichten voor een specifiek apparaat wilt controleren, klikt u met de rechtermuisknop op dit apparaat in de lijst en selecteert u **Controle D2C-berichten starten**.
3. Als u wilt stoppen met het bewaken van gegevens, voert u de opdracht **Azure IoT Hub: Stop monitoring D2C message** in het opdrachtpalet uit. 
4. Als u de moduledubbel wilt weergeven of bewerken, klikt u met de rechtermuisknop op deze moduledubbel in de lijst en selecteert u **Moduledubbel bewerken**. Als u de moduledubbel wilt bijwerken, slaat u het dubbele JSON-bestand op, klikt u met de rechtermuisknop in de editor en selecteert u **Moduledubbel bijwerken**.
5. Als u Docker-logboeken wilt bekijken, installeert u [Docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) voor VS Code. U kunt de actieve modules lokaal zoeken in Docker Explorer. Selecteer in het contextmenu **Logboeken weergeven** om ze te bekijken in de geïntegreerde terminal.
 
## <a name="clean-up-resources"></a>Resources opschonen 

Als u van plan bent door te gaan met het volgende aanbevolen artikel, kunt u de resources en configuraties die u hebt gemaakt behouden en opnieuw gebruiken. U kunt ook hetzelfde IoT Edge-apparaat blijven gebruiken als een testapparaat. 

Anders kunt u de lokale configuraties en Azure-resources die u in dit artikel hebt gemaakt, verwijderen om kosten te voorkomen. 

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

[!INCLUDE [iot-edge-clean-up-local-resources](../../includes/iot-edge-clean-up-local-resources.md)]


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een IoT Edge-module gemaakt met code voor het filteren van onbewerkte gegevens die worden gegenereerd door uw IoT Edge-apparaat. U kunt verdergaan met de volgende zelfstudies om te leren hoe Azure IoT Edge u nog meer kan helpen bij het omzetten van uw gegevens in bedrijfsinzichten.

> [!div class="nextstepaction"]
> [Gegevens opslaan met SQL Server-databases](tutorial-store-data-sql-server.md)

