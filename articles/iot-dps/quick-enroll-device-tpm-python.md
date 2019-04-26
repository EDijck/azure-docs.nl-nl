---
title: TPM-apparaat inschrijven bij Azure Device Provisioning Service met behulp van Python | Microsoft Docs
description: 'Azure-snelstart: TPM-apparaat inschrijven bij Azure IoT Hub Device Provisioning Service met behulp van de Python inrichtingsservice-SDK. In deze snelstart wordt gebruikgemaakt van afzonderlijke inschrijvingen.'
author: wesmc7777
ms.author: wesmc
ms.date: 01/26/2018
ms.topic: quickstart
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.devlang: python
ms.custom: mvc
ms.openlocfilehash: 6e38d5f3a959d363347c8b266b7bbaf165f34937
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60517140"
---
# <a name="enroll-tpm-device-to-iot-hub-device-provisioning-service-using-python-provisioning-service-sdk"></a>TPM-apparaat inschrijven bij IoT Hub Device Provisioning Service met behulp van de Python inrichtingsservice-SDK
[!INCLUDE [iot-dps-selector-quick-enroll-device-tpm](../../includes/iot-dps-selector-quick-enroll-device-tpm.md)]

In deze stappen ziet u hoe u programmatisch een afzonderlijke inschrijving voor een TPM-apparaat in Azure IoT Hub Device Provisioning Service kunt maken met behulp van de [Python inrichtingsservice-SDK](https://github.com/Azure/azure-iot-sdk-python/tree/master/provisioning_service_client) en een Python-voorbeeldtoepassing. Hoewel de Python service-SDK werkt op Windows- en Linux-computers, gebruiken we in dit artikel een Windows-ontwikkelcomputer om stapsgewijs het inschrijvingsproces te doorlopen.

Zorg ervoor dat u [IoT Hub Device Provisioning Service instelt met Azure Portal](./quick-setup-auto-provision.md) voordat u verdergaat.


<a id="prepareenvironment"></a>

## <a name="prepare-the-environment"></a>De omgeving voorbereiden 

1. [Download en installeer Python 2.x of 3.x](https://www.python.org/downloads/). Zorg ervoor dat u de 32-bits of 64-bits installatie gebruikt, zoals vereist door uw configuratie. Zorg ervoor dat u Python toevoegt aan uw platformspecifieke omgevingsvariabelen als u hierom wordt gevraagd tijdens de installatie. 

1. Kies een van de volgende opties:

    - Bouw en compileer de **Azure IoT Python SDK**. Volg [deze instructies](https://github.com/Azure/azure-iot-sdk-python/blob/master/doc/python-devbox-setup.md) voor het bouwen van de Python-pakketten. Als u een Windows-besturingssysteem hebt, gebruikt dan ook het [herdistribueerbare pakket van Visual C++](https://www.microsoft.com/download/confirmation.aspx?id=48145) om het gebruik van systeemeigen DLL's van Python mogelijk te maken.

    - [Installeer of upgrade *pip*, het Python pakketbeheersysteem](https://pip.pypa.io/en/stable/installing/) en installeer het pakket met behulp van de volgende opdracht:

        ```cmd/sh
        pip install azure-iothub-provisioningserviceclient
        ```

1. U hebt de goedkeuringssleutel nodig voor uw apparaat. Als u de stappen in de quickstart [Create and provision a simulated device](quick-create-simulated-device.md) (Een gesimuleerd apparaat maken en inrichten) voor het maken van een gesimuleerd TPM-apparaat hebt gevolgd, gebruikt u de sleutel voor dit apparaat. U kunt anders de volgende goedkeuringssleutel gebruiken die is opgegeven met de SDK:

    ```
    AToAAQALAAMAsgAgg3GXZ0SEs/gakMyNRqXXJP1S124GUgtk8qHaGzMUaaoABgCAAEMAEAgAAAAAAAEAtW6MOyCu/Nih47atIIoZtlYkhLeCTiSrtRN3q6hqgOllA979No4BOcDWF90OyzJvjQknMfXS/Dx/IJIBnORgCg1YX/j4EEtO7Ase29Xd63HjvG8M94+u2XINu79rkTxeueqW7gPeRZQPnl1xYmqawYcyzJS6GKWKdoIdS+UWu6bJr58V3xwvOQI4NibXKD7htvz07jLItWTFhsWnTdZbJ7PnmfCa2vbRH/9pZIow+CcAL9mNTNNN4FdzYwapNVO+6SY/W4XU0Q+dLMCKYarqVNH5GzAWDfKT8nKzg69yQejJM8oeUWag/8odWOfbszA+iFjw3wVNrA5n8grUieRkPQ==
    ```


## <a name="modify-the-python-sample-code"></a>De Python-voorbeeldcode wijzigen

In deze sectie ziet u hoe u de inrichtingsgegevens van het TPM-apparaat toevoegt aan de voorbeeldcode. 

1. Start een teksteditor en maak een nieuw bestand **TpmEnrollment.py**.

1. Voeg de volgende `import` -instructies en -variabelen aan het begin van het bestand **TpmEnrollment.py** toe. Vervang vervolgens `dpsConnectionString` door uw verbindingsreeks die te vinden is onder **Gedeeld toegangsbeleid** in **Device Provisioning Service** in **Azure Portal**. Vervang `endorsementKey` door de waarde die u eerder hebt genoteerd toen u [de omgeving voorbereidde](quick-enroll-device-tpm-python.md#prepareenvironment). Maak ten slotte een unieke `registrationid` en zorg dat deze alleen bestaat uit kleine letters en afbreekstreepjes.  
   
    ```python
    from provisioningserviceclient import ProvisioningServiceClient
    from provisioningserviceclient.models import IndividualEnrollment, AttestationMechanism

    CONNECTION_STRING = "{dpsConnectionString}"

    ENDORSEMENT_KEY = "{endorsementKey}"

    REGISTRATION_ID = "{registrationid}"
    ```

1. Voeg de volgende functie en functieaanroep toe om het aanmaken van de groepsinschrijving te implementeren:
   
    ```python
    def main():
        print ( "Starting individual enrollment..." )

        psc = ProvisioningServiceClient.create_from_connection_string(CONNECTION_STRING)

        att = AttestationMechanism.create_with_tpm(ENDORSEMENT_KEY)
        ie = IndividualEnrollment.create(REGISTRATION_ID, att)

        ie = psc.create_or_update(ie)
    
        print ( "Individual enrollment successful." )
    
    if __name__ == '__main__':
        main()
    ```

1. Sla het bestand **TpmEnrollment.py** op en sluit het.
 

## <a name="run-the-sample-tpm-enrollment"></a>De voorbeeld-TPM-inschrijving uitvoeren

1. Open een opdrachtprompt en voer het script uit.

    ```cmd/sh
    python TpmEnrollment.py
    ```

1. Houd de uitvoer in de gaten voor een geslaagde inschrijving.

1. Navigeer naar de inrichtingsservice in Azure Portal. Klik op **Inschrijvingen beheren**. U ziet dat het TPM-apparaat wordt weergegeven op het tabblad **Afzonderlijke registraties** met de naam `registrationid` die u eerder hebt gemaakt. 

    ![De TPM-inschrijving controleren in de portal](./media/quick-enroll-device-tpm-python/1.png)  


## <a name="clean-up-resources"></a>Resources opschonen
Als u van plan bent om het voorbeeld van de Java.js-service te verkennen, verwijdert u de resources die u in deze quickstart hebt gemaakt, niet. Als u niet wilt doorgaan, gebruikt u de volgende stappen om alle resources die via deze Snelstartgids zijn gemaakt, te verwijderen.

1. Sluit het uitvoervenster van het Python-voorbeeld op de computer.
1. Als u een gesimuleerd TPM-apparaat hebt gemaakt, sluit u het venster van de TPM-simulator.
1. Navigeer naar Device Provisioning Service in Azure Portal, klik op **Inschrijvingen beheren** en selecteer vervolgens het tabblad **Afzonderlijke inschrijvingen**. Selecteer de *Registratie-id* voor de inschrijvingsvermelding die u hebt gemaakt met behulp van deze quickstart. Klik vervolgens op de knop **Verwijderen** boven aan de blade.  


## <a name="next-steps"></a>Volgende stappen
In deze quickstart hebt u programmatisch een afzonderlijke inschrijvingsvermelding gemaakt voor een TPM-apparaat. Ook hebt u desgewenst een gesimuleerd TPM-apparaat op de computer gemaakt en dit apparaat ingericht voor uw IoT-hub met Azure IoT Hub Device Provisioning Service. Voor meer informatie over device provisioning, gaat u verder met de zelfstudie voor het instellen van Device Provisioning Service in Azure Portal.

> [!div class="nextstepaction"]
> [Zelfstudies over Azure IoT Hub Device Provisioning Service](./tutorial-set-up-cloud.md)
