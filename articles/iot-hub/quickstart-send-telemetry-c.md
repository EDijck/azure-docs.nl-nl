---
title: Telemetrie verzenden naar Azure IoT Hub (C#) | Microsoft Docs
description: In deze snelstart voert u twee C-voorbeeldtoepassingen uit om gesimuleerde telemetrie te verzenden naar een IoT-hub en telemetrie van de IoT-hub te lezen voor verwerking in de cloud.
author: wesmc7777
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: c
ms.topic: quickstart
ms.custom: mvc
ms.date: 02/25/2019
ms.author: wesmc
ms.openlocfilehash: 9c65020c4d9b80d5f32b87aa8c63878a677ae0c9
ms.sourcegitcommit: 045406e0aa1beb7537c12c0ea1fbf736062708e8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/04/2019
ms.locfileid: "59006292"
---
# <a name="quickstart-send-telemetry-from-a-device-to-an-iot-hub-and-read-it-with-a-back-end-application-c"></a>Quickstart: Telemetrie vanaf een apparaat verzenden naar een IoT-hub en lezen met een back-endtoepassing (C)

[!INCLUDE [iot-hub-quickstarts-1-selector](../../includes/iot-hub-quickstarts-1-selector.md)]

IoT Hub is een Azure-service waarmee grote hoeveelheden telemetrie van uw IoT-apparaten naar de cloud kunt opnemen voor opslag of verwerking. In deze snelstartgids verzendt u telemetrie vanuit een toepassing voor een gesimuleerd apparaat via IoT Hub naar een back-endtoepassing voor verwerking.

De snelstartgids maakt gebruik van een voorbeeldtoepassing C van de [Azure IoT device-SDK voor C](iot-hub-device-sdk-c-intro.md) om telemetrie te verzenden naar een IoT-hub. De Azure IoT device SDK's zijn geschreven in [ANSI C (C99)](https://wikipedia.org/wiki/C99) voor portabiliteit en brede platformcompatibiliteit. Voordat u de voorbeeldcode uitvoert, maakt u een IoT-hub en registreert het gesimuleerde apparaat bij die hub.

Dit artikel is geschreven voor Windows, maar u kunt deze snelstartgids ook op Linux uitvoeren.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

* [Visual Studio 2017](https://www.visualstudio.com/vs/) installeren met de workload ['Desktopontwikkeling met C++'](https://www.visualstudio.com/vs/support/selecting-workloads-visual-studio-2017/) ingeschakeld.
* Installeer de meest recente versie van [Git](https://git-scm.com/download/).
* Voer de volgende opdracht om toe te voegen van de Microsoft Azure IoT-extensie voor Azure CLI met de Cloud Shell-sessie. De IOT-extensie worden IoT Hub, IoT Edge en IoT Device Provisioning Service (DPS) specifieke opdrachten toegevoegd aan Azure CLI.

   ```azurecli-interactive
   az extension add --name azure-cli-iot-ext
   ```

## <a name="prepare-the-development-environment"></a>De ontwikkelomgeving voorbereiden

Voor deze snelstart gebruikt u de [Azure IoT device-SDK voor C](iot-hub-device-sdk-c-intro.md). 

U kunt de SDK door het installeren van de pakketten en bibliotheken voor de volgende omgevingen gebruiken:

* **Linux**: apt-get-pakketten zijn beschikbaar voor Ubuntu 16.04 en 18.04 met de volgende CPU-architectuur: amd64 arm64, armhf en i386. Zie [Gebruik van apt-get om een project C apparaat-client op Ubuntu te maken](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/ubuntu_apt-get_sample_setup.md) voor meer informatie.

* **mbed**: voor ontwikkelaars die toepassingen voor apparaten maken op het platform mbed, hebben we een bibliotheek en voorbeelden gepubliceerd waarmee u binnen enkele minuten met Azure IoT Hub bent gestart. Zie voor meer informatie, [Gebruik van de bibliotheek mbed](https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/readme.md#mbed).

* **Arduino**: als u op Arduino ontwikkelt, kunt u gebruikmaken van de Azure IoT-bibliotheek die beschikbaar is in de Arduino IDE-bibliotheekbeheerder. Zie voor meer informatie, [De Azure IoT Hub-bibliotheek voor Arduino](https://github.com/azure/azure-iot-arduino).

* **iOS**: de IoT Hub apparaat-SDK is beschikbaar als CocoaPods voor de ontwikkeling van Mac- en iOS-apparaten. Zie [iOS-voorbeelden voor Microsoft Azure IoT](https://cocoapods.org/pods/AzureIoTHubClient) voor meer informatie.

In deze snelstartgids bereidt u een ontwikkelomgeving voor die wordt gebruikt om de [Azure IoT C-SDK](https://github.com/Azure/azure-iot-sdk-c) van GitHub te klonen en te bouwen. De SDK op GitHub bevat de voorbeeldcode gebruikt in deze snelstartgids. 

1. Download de [CMake-bouwsysteem](https://cmake.org/download/). Controleer of het gedownloade binaire bestand met behulp van de cryptografische hash-waarde die overeenkomt met de versie die u hebt gedownload. Er zijn ook de cryptografische hash-waarden gevonden op de downloadkoppeling CMake is al opgegeven.

    Het volgende voorbeeld Windows PowerShell gebruikt om te controleren of de cryptografische hash voor 3.13.4-versie van de x64 MSI-distributiepunt:

    ```powershell
    PS C:\Downloads> $hash = get-filehash .\cmake-3.13.4-win64-x64.msi
    PS C:\Downloads> $hash.Hash -eq "64AC7DD5411B48C2717E15738B83EA0D4347CD51B940487DFF7F99A870656C09"
    True
    ```

    De volgende hash-waarden voor versie 3.13.4 werden vermeld op de CMake-site ten tijde van dit schrijven:

    ```
    563a39e0a7c7368f81bfa1c3aff8b590a0617cdfe51177ddc808f66cc0866c76  cmake-3.13.4-Linux-x86_64.tar.gz
    7c37235ece6ce85aab2ce169106e0e729504ad64707d56e4dbfc982cb4263847  cmake-3.13.4-win32-x86.msi
    64ac7dd5411b48c2717e15738b83ea0d4347cd51b940487dff7f99a870656c09  cmake-3.13.4-win64-x64.msi
    ```

    Het is belangrijk dat de vereisten voor Visual Studio met (Visual Studio en de workload Desktopontwikkeling met C++) op uw computer zijn geïnstalleerd **voordat** de `CMake`-installatie wordt gestart. Zodra aan de vereisten is voldaan en de download is geverifieerd, installeert u het CMake-bouwsysteem.

2. Open een opdrachtprompt of Git Bash-shell. Voer de volgende opdracht uit voor het klonen van de [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c) GitHub-opslagplaats:
    
    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-sdk-c.git --recursive
    ```
    De grootte van deze opslagplaats is momenteel ongeveer 220 MB. Deze bewerking kan enkele minuten in beslag nemen.


3. Maak de submap `cmake` in de hoofdmap van de Git-opslagplaats en navigeer naar die map. 

    ```cmd/sh
    cd azure-iot-sdk-c
    mkdir cmake
    cd cmake
    ```

4. Voer de volgende opdracht uit om een versie van de SDK te compileren die specifiek is voor uw clientplatform voor ontwikkeling. Er wordt een Visual Studio-oplossing voor het gesimuleerde apparaat gegenereerd in de map `cmake`. 

    ```cmd
    cmake ..
    ```
    
    Als `cmake` uw C++-compiler niet kan vinden, kunnen er fouten in de build optreden tijdens het uitvoeren van de bovenstaande opdracht. Als dit gebeurt, voert u deze opdracht uit bij de [Visual Studio-opdrachtprompt](https://docs.microsoft.com/dotnet/framework/tools/developer-command-prompt-for-vs). 

    Zodra het bouwen is voltooid, zijn de laatste paar uitvoerregels vergelijkbaar met de volgende uitvoer:

    ```cmd/sh
    $ cmake ..
    -- Building for: Visual Studio 15 2017
    -- Selecting Windows SDK version 10.0.16299.0 to target Windows 10.0.17134.
    -- The C compiler identification is MSVC 19.12.25835.0
    -- The CXX compiler identification is MSVC 19.12.25835.0

    ...

    -- Configuring done
    -- Generating done
    -- Build files have been written to: E:/IoT Testing/azure-iot-sdk-c/cmake
    ```


## <a name="create-an-iot-hub"></a>Een IoT Hub maken

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-device"></a>Een apparaat registreren

Een apparaat moet zijn geregistreerd bij uw IoT-hub voordat het verbinding kan maken. In deze sectie gebruikt u Azure Cloud Shell met de [IoT-extensie](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot?view=azure-cli-latest) om een gesimuleerd apparaat te registreren.

1. Voer de volgende opdracht in Azure Cloud Shell te maken van de apparaat-id.

   **YourIoTHubName** : vervang deze tijdelijke aanduiding door een door u gekozen naam voor de IoT-hub.

   **MyCDevice** : dit is de naam van het geregistreerde apparaat. Gebruik MyCDevice, zoals wordt weergegeven. Als u een andere naam voor het apparaat kiest, moet u deze naam ook in de rest van dit artikel gebruiken, en moet u de apparaatnaam bijwerken in de voorbeeldtoepassingen voordat u ze uitvoert.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyCDevice
    ```

2. Voer de volgende opdrachten uit in Azure Cloud Shell om de _apparaatverbindingsreeks_ op te halen voor het apparaat dat u zojuist hebt geregistreerd:

   **YourIoTHubName** : vervang deze tijdelijke aanduiding door een door u gekozen naam voor de IoT-hub.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name YourIoTHubName --device-id MyCDevice --output table
    ```

    Noteer de apparaatverbindingsreeks. Deze ziet er ongeveer als volgt uit:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyNodeDevice;SharedAccessKey={YourSharedAccessKey}`

    U gebruikt deze waarde verderop in de snelstartgids.

## <a name="send-simulated-telemetry"></a>Gesimuleerde telemetrie verzenden

De toepassing voor het gesimuleerde apparaat maakt verbinding met een apparaatspecifiek eindpunt op uw IoT-hub en verzendt een tekenreeks als gesimuleerde telemetrie.

1. Open het bronbestand iothub_convenience_sample.c met een teksteditor, en bekijk de voorbeeldcode voor het verzenden van telemetrie. Het bestand bevindt zich in de volgende locatie:

    ```
    \azure-iot-sdk-c\iothub_client\samples\iothub_convenience_sample\iothub_convenience_sample.c
    ```

2. Zoek de declaratie van de `connectionString` constante:

    ```C
    /* Paste in your device connection string  */
    static const char* connectionString = "[device connection string]";
    ```
    Vervang de waarde van de `connectionString` constante door de apparaatverbindingsreeks die u eerder hebt genoteerd. Sla uw wijzigingen vervolgens op naar **iothub_convenience_sample.c**.

3. Navigeer in een lokaal terminalvenster naar de *iothub_convenience_sample*-projectmap in de CMake-map die u hebt gemaakt in de Azure IoT C SDK.

    ```
    cd /azure-iot-sdk-c/cmake/iothub_client/samples/iothub_convenience_sample
    ```

4. Voer CMake uit in uw lokale terminalvenster om het voorbeeld te bouwen met de bijgewerkte `connectionString`-waarde:

    ```cmd/sh
    cmake --build . --target iothub_convenience_sample --config Debug
    ```

5. Voer in een lokaal terminalvenster de volgende opdracht uit om de toepassing voor het gesimuleerde apparaat uit te voeren:

    ```cmd/sh
    Debug\iothub_convenience_sample.exe
    ```

    De volgende schermafbeelding geeft de uitvoer weer als de toepassing voor het gesimuleerde apparaat telemetriegegevens naar uw IoT-hub verzendt:

    ![Het gesimuleerde apparaat uitvoeren](media/quickstart-send-telemetry-c/simulated-device-app.png)

## <a name="read-the-telemetry-from-your-hub"></a>De telemetrie van uw hub lezen


In deze sectie gebruikt u Azure Cloud Shell met de [IoT-extensie](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot?view=azure-cli-latest) voor het bewaken van de apparaatberichten die worden verzonden door het gesimuleerde apparaat.

1. Voer met de Azure Cloud Shell de volgende opdracht uit om te verbinden en berichten te lezen uit uw IoT-hub:

   **YourIoTHubName** : vervang deze tijdelijke aanduiding door een door u gekozen naam voor de IoT-hub.

    ```azurecli-interactive
    az iot hub monitor-events --hub-name YourIoTHubName --output table
    ```

    ![Lees de apparaatberichten met de Azure CLI](media/quickstart-send-telemetry-c/read-device-to-cloud-messages-app.png)

    

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources.md)]

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u een IoT-hub geconfigureerd, een apparaat geregistreerd, gesimuleerde telemetrie verzonden naar de hub met behulp van een C-toepassing en de telemetrie gelezen van de hub met behulp van de Azure CLI.

Voor meer informatie over het ontwikkelen met de Azure loT Hub C SDK gaat u verder naar de volgende instructiegids:

> [!div class="nextstepaction"]
> [Ontwikkelen met behulp van C-SDK van Azure IoT Hub](iot-hub-devguide-develop-for-constrained-devices.md)