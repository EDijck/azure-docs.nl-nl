---
title: Een echte apparaat toevoegen aan een Azure IoT Central-toepassing | Microsoft Docs
description: Als operator kunt u een nieuw apparaat toevoegen aan uw Azure IoT Central-toepassing.
author: sandeeppujar
ms.author: sandeepu
ms.date: 02/01/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: 5d9e33e5cf4c8a7d5b8085e1e2ff8f4bde3274b7
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57770898"
---
# <a name="tutorial-add-a-real-device-to-your-azure-iot-central-application"></a>Zelfstudie: Een nieuw apparaat toevoegen aan uw Azure IoT Central-toepassing

In deze zelfstudie wordt getoond hoe u en echt apparaat aan uw Microsoft Azure IoT Central-toepassing kunt toevoegen en het kunt configureren.

Deze zelfstudie bestaat uit twee delen:

1. Als operator leert u eerst hoe u een echt apparaat aan uw Azure IoT Central-toepassing kunt toevoegen en het kunt configureren. Aan het eind van dit deel haalt u een verbindingsreeks op die u in het tweede deel kunt gebruiken.
2. Vervolgens krijgt u als apparaatontwikkelaar informatie over de code in het echte apparaat. U voegt de verbindingsreeks uit het eerste deel toe aan de voorbeeldcode.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een nieuw echt apparaat toevoegen
> * Het echte apparaat configureren
> * Verbindingsreeks voor echt apparaat vanuit de toepassing ophalen
> * Leren hoe clientcode aan toepassing wordt toegewezen
> * Clientcode voor echt apparaat configureren

## <a name="prerequisites"></a>Vereisten

Voordat u begint, moet degene die het apparaat maakt ten minste de eerste zelfstudie voor opbouwfuncties voltooien voor het maken van de Azure IoT Central-toepassing:

* [Een nieuw apparaattype definiëren](tutorial-define-device-type.md) (verplicht)
* [Regels en acties voor uw apparaat configureren](tutorial-configure-rules.md) (optioneel)
* [De weergaven van de operator aanpassen](tutorial-customize-operator.md) (optioneel)

## <a name="add-a-real-device"></a>Echt apparaat toevoegen

Als u een echt apparaat aan uw toepassing wilt toevoegen, gebruikt u de apparaatsjabloon **Connected Air Conditioner** die u in de zelfstudie [Nieuw apparaattype definiëren](tutorial-define-device-type.md) hebt gemaakt.

1. Als u als operator een nieuw apparaat wilt toevoegen, kiest u **Device Explorer** in het linker navigatiemenu:

   ![Pagina Device Explorer met verbonden airco](media/tutorial-add-device/explorer.png)

   In **Device Explorer** wordt de apparaatsjabloon **Connected Air Conditioner** getoond en tevens het gesimuleerde apparaat dat automatisch is gemaakt toen de opbouwfunctie de apparaatsjabloon maakte.

2. Selecteer eerst verbinding te maken van een apparaat echte verbonden airconditioner **+**, klikt u vervolgens **echte**:

   ![Nieuwe, verbonden airco toevoegen](media/tutorial-add-device/newreal.png)

3. Voer de apparaat-Id (moet kleine letters zijn) of gebruikt u de voorgestelde apparaat-id. U kunt ook de naam voor uw nieuwe apparaat invoeren en **Maken** selecteren.

   ![Naam van apparaat wijzigen](media/tutorial-add-device/rename.png)

## <a name="configure-a-real-device"></a>Echt apparaat configureren

Het echte apparaat wordt gemaakt van de apparaatsjabloon **Connected Air Conditioner**. U kunt via **Instellingen** het apparaat configureren en eigenschapswaarden instellen om informatie over het apparaat te registreren.

1. Op de pagina **Instellingen** ziet u dat de status van de instelling **Set Temperature** **no update** is. Deze status blijft bestaan totdat het echte apparaat verbinding maakt met de toepassing en bevestigt dat er actie op de instelling is uitgevoerd.

    ![Instellingen vertonen synchronisatie](media/tutorial-add-device/settingssyncing.png)

2. Op de pagina **Eigenschappen** van uw nieuwe, echte verbonden airconditioningsapparaat zijn de eigenschappen voor de servicelocatie en laatste servicedatum beide bewerkbaar. De velden voor het serienummer en de firmwareversie blijven leeg totdat het apparaat is verbonden met de toepassing. Dit zijn alleen-lezen waarden die vanaf het apparaat worden verzonden. U kunt deze waarden niet bewerken.

    ![Apparaateigenschappen van het echte apparaat](media/tutorial-add-device/setproperties1.png)

3. U kunt de pagina's **Metingen**, **Regels** en **Dashboard** voor het echte apparaat weergeven.

## <a name="generate-connection-string"></a>Verbindingsreeks genereren

Een apparaatontwikkelaar dient de *verbindingsreeks* voor het echte apparaat in te sluiten in de code die op het apparaat wordt uitgevoerd. Dankzij de verbindingsreeks kan het apparaat veilig verbinding maken met de Azure IoT Central-toepassing. U genereert de verbindingsreeks wanneer u de clientcode in Node.js voorbereidt in de volgende stappen. De Node.js-toepassing vertegenwoordigt de echte verbonden airconditioning. 

## <a name="prepare-the-client-code"></a>Clientcode voorbereiden

De voorbeeldcode in dit artikel is geschreven in [Node.js](https://nodejs.org/) en toont voldoende code om:

* Verbinding te maken als een apparaat met uw Azure IoT Central-toepassing.
* Temperatuurtelemetrie te verzenden als verbonden airco.
* Apparaateigenschappen naar uw Azure IoT Central-toepassing te verzenden.
* Te reageren op een operator die de instelling **Set Temperature** gebruikt.
* De echo-opdracht van uw Azure IoT Central-toepassing te verwerken.

In de artikelen met praktijkvoorbeelden waarnaar in de sectie [Volgende stappen](#next-steps) wordt verwezen, bieden meer complete voorbeelden en laten het gebruik zien van andere programmeertalen. Zie het artikel [Device connectivity](concepts-connectivity.md) (Apparaatconnectiviteit) voor meer informatie over hoe apparaten verbinding maken met Azure IoT Central.

In de volgende stappen ziet u hoe u het [Node.js](https://nodejs.org/)-voorbeeld voorbereidt:

1. Installeer [Node.js](https://nodejs.org/) versie 4.0.x of nieuwer op uw computer. Node.js is beschikbaar voor een groot aantal verschillende besturingssystemen.

1. Maak de map `connectedairconditioner` op uw computer.

1. Ga in uw opdrachtregelprogramma naar de map `connectedairconditioner` die u hebt gemaakt.

1. Installeer de DPS-sleutelgenerator met behulp van de volgende opdracht:

    ```cmd/sh
    npm i -g dps-keygen
    ```

   Meer informatie over het [opdrachtregelprogramma hier](https://www.npmjs.com/package/dps-keygen).

1. De verbindingsreeks voor een apparaatinstantie in uw toepassing wordt gegenereerd op basis van apparaatinformatie die wordt verstrekt door IoT Central.

   Ga terug naar de IoT Central-portal. Kies op het apparaatscherm voor uw echte, verbonden airconditioning de optie **Verbinding maken**.

   ![Apparaatpagina met koppeling voor het weergeven van verbindingsgegevens](media/tutorial-add-device/connectionlink.png)

1. Kopieer op de pagina Apparaatverbinding de bereik-id, apparaat-id en primaire sleutel en plak deze in een teksteditor. Sla deze waarden vervolgens op. U gaat ze in de volgende stap gebruiken.

   ![Verbindingsdetails](media/tutorial-add-device/device-connect.png)

1. Ga terug naar de opdrachtregelomgeving en genereer uw verbindingsreeks door het volgende uit te voeren:

    ```cmd/sh
    dps-keygen -si:<scope_id> -di:<device_id> -dk:<Primary Key>
    ```

   Kopieer de uitvoer en sla deze op in een nieuw bestand (bijvoorbeeld verbinding.txt).

1. Voor het initialiseren van het Node.js-project, voert u de volgende opdracht uit, waarbij u alle standaardwaarden overneemt:

    ```cmd/sh
    npm init
      ```

1. Voor het installeren van de benodigde pakketten, voert u de volgende opdracht uit:

    ```cmd/sh
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. Maak in een teksteditor een bestand met de naam **ConnectedAirConditioner.js** in de map `connectedairconditioner`.

1. Voeg de volgende `require`-instructies toe aan het begin van het bestand **ConnectedAirConditioner.js**:

    ```javascript
    'use strict';

    var clientFromConnectionString = require('azure-iot-device-mqtt').clientFromConnectionString;
    var Message = require('azure-iot-device').Message;
    var ConnectionString = require('azure-iot-device').ConnectionString;
    ```

1. Voeg de volgende variabelendeclaraties aan het bestand toe:

    ```javascript
    var connectionString = '{your device connection string}';
    var targetTemperature = 0;
    var client = clientFromConnectionString(connectionString);
    ```

    > [!NOTE]
    > De tijdelijke aanduiding `{your device connection string}` werkt u in een volgende stap bij.

1. Sla de wijzigingen op die u tot nu toe hebt gemaakt, maar houd het bestand open.

## <a name="review-client-code"></a>Clientcode bekijken

In de vorige sectie hebt u een opzet gemaakt van een Node.js-project voor een toepassing waarmee u verbinding kunt maken met uw Azure IoT Central-toepassing. In deze sectie voert u de code toe om:

* Verbinding te maken met uw Azure IoT Central-toepassing.
* Telemetrie te verzenden naar uw Azure IoT Central-toepassing.
* Apparaateigenschappen naar uw Azure IoT Central-toepassing te verzenden.
* Instellingen te ontvangen van uw Azure IoT Central-toepassing.
* De echo-opdracht van uw Azure IoT Central-toepassing te verwerken.

1. Als u telemetrie over de temperatuur wilt verzenden naar uw Azure IoT Central-toepassing, voegt u de volgende code toe aan het bestand **ConnectedAirConditioner.js**:

    ```javascript
    // Send device telemetry.
    function sendTelemetry() {
      var temperature = targetTemperature + (Math.random() * 15);
      var data = JSON.stringify({ temperature: temperature });
      var message = new Message(data);
      client.sendEvent(message, (err, res) => console.log(`Sent message: ${message.getData()}` +
        (err ? `; error: ${err.toString()}` : '') +
        (res ? `; status: ${res.constructor.name}` : '')));
    }
    ```

    De naam van het veld in de JSON die u verzendt, moet overeenkomen met de naam van het veld dat u hebt opgegeven voor temperatuurtelemetrie in de sjabloon voor het apparaat. In dit voorbeeld is **temperature** de naam van het veld.

1. Als u apparaateigenschappen zoals **firmwareVersion** en **serialNumber** wilt verzenden, voegt u de volgende definitie toe:

    ```javascript
    // Send device properties
    function sendDeviceProperties(twin) {
      var properties = {
        firmwareVersion: "9.75",
        serialNumber: "10001"
      };
      twin.properties.reported.update(properties, (errorMessage) => 
      console.log(` * Sent device properties ` + (errorMessage ? `Error: ${errorMessage.toString()}` : `(success)`)));
    }
    ```

1. Als u de instellingen die door uw apparaat worden ondersteund (bijvoorbeeld **setTemperature**), wilt definiëren, voegt u de volgende definitie toe:

    ```javascript
    // Add any settings your device supports
    // mapped to a function that is called when the setting is changed.
    var settings = {
      'setTemperature': (newValue, callback) => {
        // Simulate the temperature setting taking two steps.
        setTimeout(() => {
          targetTemperature = targetTemperature + (newValue - targetTemperature) / 2;
          callback(targetTemperature, 'pending');
          setTimeout(() => {
            targetTemperature = newValue;
            callback(targetTemperature, 'completed');
          }, 5000);
        }, 5000);
      }
    };
    ```

1. Voor het afhandelen van instellingen die vanaf Azure IoT Central zijn verzonden, voegt u de volgende functie toe (hiermee wordt de juiste apparaatcode gelokaliseerd en uitgevoerd):

    ```javascript
    // Handle settings changes that come from Azure IoT Central via the device twin.
    function handleSettings(twin) {
      twin.on('properties.desired', function (desiredChange) {
        for (let setting in desiredChange) {
          if (settings[setting]) {
            console.log(`Received setting: ${setting}: ${desiredChange[setting].value}`);
            settings[setting](desiredChange[setting].value, (newValue, status, message) => {
              var patch = {
                [setting]: {
                  value: newValue,
                  status: status,
                  desiredVersion: desiredChange.$version,
                  message: message
                }
              }
              twin.properties.reported.update(patch, (err) => console.log(`Sent setting update for ${setting}; ` +
                (err ? `error: ${err.toString()}` : `status: success`)));
            });
          }
        }
      });
    }
    ```

    Deze functie:

    * Controleert of Azure IoT Central een gewenste eigenschap verzendt.
    * Lokaliseert de juiste functie om ervoor te zorgen dat de wijziging in de instelling wordt verwerkt.
    * Stuurt een bevestiging terug naar de Azure IoT Central-toepassing.

1. Als u wilt reageren op een opdracht zoals **echo** vanuit uw Azure IoT Central-toepassing, voegt u de volgende definitie toe:

    ```javascript
    // Respond to the echo command
    function onCommandEcho(request, response) {
      // Display console info
      console.log(' * Echo command received');
      // Respond
      response.send(10, 'Success', function (errorMessage) {});
    }
    ```

1. Voeg de volgende code toe voor het voltooien van de verbinding met Azure IoT Central en het aansluiten van de functies in de clientcode:

    ```javascript
    // Handle device connection to Azure IoT Central.
    var connectCallback = (err) => {
      if (err) {
        console.log(`Device could not connect to Azure IoT Central: ${err.toString()}`);
      } else {
        console.log('Device successfully connected to Azure IoT Central');
        // Send telemetry measurements to Azure IoT Central every 1 second.
        setInterval(sendTelemetry, 1000);
        // Setup device command callbacks
        client.onDeviceMethod('echo', onCommandEcho);
        // Get device twin from Azure IoT Central.
        client.getTwin((err, twin) => {
          if (err) {
            console.log(`Error getting device twin: ${err.toString()}`);
          } else {
            // Send device properties once on device start up
            sendDeviceProperties(twin);
            // Apply device settings and handle changes to device settings.
            handleSettings(twin);
          }
        });
      }
    };

    client.open(connectCallback);
    ```

1. Sla de wijzigingen op die u tot nu toe hebt gemaakt, maar houd het bestand open.

## <a name="configure-client-code"></a>Clientcode configureren

<!-- Add the connection string to the sample code, build, and run --> Als u de clientcode wilt configureren zodat verbinding kan worden gemaakt met de Azure IoT Central-toepassing, dan dient u de verbindingsreeks voor het echte apparaat dat u eerder in deze zelfstudie hebt genoteerd, toe te voegen.

1. Zoek in bestand **ConnectedAirConditioner.js** de volgende regel met code:

    ```javascript
    var connectionString = '{your device connection string}';
    ```

1. Vervang `{your device connection string}` door de verbindingsreeks van het echte apparaat. U hebt de verbindingsreeks eerder opgeslagen in een teksteditor.

1. Sla de wijzigingen op in het bestand **ConnectedAirConditioner.js**.

1. Als u het voorbeeld wilt uitvoeren, voert u de volgende opdracht uit in uw opdrachtregelprogramma:

    ```cmd/sh
    node ConnectedAirConditioner.js
    ```

    > [!NOTE]
    > Zorg ervoor dat u zich in de map `connectedairconditioner` bevindt als u deze opdracht uitvoert.

1. Uitvoer wordt naar de console afgedrukt:

   ![Uitvoer van clienttoepassing](media/tutorial-add-device/output.png)

1. Na circa dertig seconden ziet u de telemetrie op de pagina **Metingen** van het apparaat:

   ![Echte ~ ~ telemetrie](media/tutorial-add-device/realtelemetry.png)

1. Op de pagina **Instellingen** ziet u dat de instelling nu wordt gesynchroniseerd. Toen het apparaat voor het eerst verbinding maakte, werd de instellingswaarde ontvangen en de wijziging bevestigd:

   ![Instelling gesynchroniseerd](media/tutorial-add-device/settingsynced.png)

1. Stel op de pagina **Instellingen** de apparaattemperatuur in op **95** en kies **Update device**. De voorbeeldtoepassing ontvangt deze wijziging en verwerkt deze:

   ![Instelling ontvangen en verwerken](media/tutorial-add-device/receivesetting.png)

   > [!NOTE]
   > Er zijn twee berichten over het bijwerken van de instelling. Een als de status `pending` wordt verzonden en een als de status `completed` wordt verzonden.

1. Op de pagina **Metingen** ziet u dat het apparaat waarden met een hogere temperatuur verzendt:

    ![Temperatuurtelemetrie is nu hoger](media/tutorial-add-device/highertemperature.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="nextstepaction"]
> * Een nieuw echt apparaat toevoegen
> * Nieuwe apparaat configureren
> * Verbindingsreeks voor echt apparaat vanuit de toepassing ophalen
> * Leren hoe clientcode aan toepassing wordt toegewezen
> * Clientcode voor echt apparaat configureren

Nu u een echt apparaat met uw Azure IoT Central-toepassing hebt verbonden, volgen hier de aanbevolen volgende stappen:

Als operator leert u het volgende:

* [Uw apparaten beheren](howto-manage-devices.md)
* [Apparaatsets gebruiken](howto-use-device-sets.md)
* [Aangepaste analyses maken](howto-use-device-sets.md)

Als apparaatontwikkelaar, leert u het volgende:

* [DevKit voorbereiden en verbinden](howto-connect-devkit.md)
* [Raspberry Pi voorbereiden en verbinden](howto-connect-raspberry-pi-python.md)
* [Een generieke Node.js-client verbinden met uw Azure IoT Central-toepassing](howto-connect-nodejs.md)
* [Uw code aanpassen][lnk-nodejs-device-ref]


[lnk-nodejs-device-ref]: /javascript/api/azure-iot-device/?view=azure-iot-typescript-latest