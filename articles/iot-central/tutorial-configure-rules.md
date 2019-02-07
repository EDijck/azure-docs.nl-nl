---
title: Regels en acties configureren in Azure IoT Central | Microsoft Docs
description: Deze zelfstudie laat u zien hoe u als bouwer op telemetrie gebaseerde regels en acties in uw Azure IoT Central-toepassing kunt configureren.
author: ankitscribbles
ms.author: ankitgup
ms.date: 10/12/2018
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: d7269f61579ce1ffd9a686634effd153837a2f25
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/02/2019
ms.locfileid: "55662978"
---
# <a name="tutorial-configure-rules-and-actions-for-your-device-in-azure-iot-central"></a>Zelfstudie: Regels en acties voor uw apparaat configureren in Azure IoT Central

*Dit artikel is van toepassing op operators, opbouwfuncties en beheerders.*

In deze zelfstudie maakt u een regel die een e-mail verzendt wanneer de temperatuur in een aangesloten airconditioningapparaat hoger is dan 32 &deg;C.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een regel op basis van telemetrie maken
> * Een actie toevoegen

[!INCLUDE [iot-central-experimental-note](../../includes/iot-central-experimental-note.md)]

## <a name="prerequisites"></a>Vereisten

Voer voordat u begint de zelfstudie [Een nieuw apparaattype definiëren in uw toepassing](tutorial-define-device-type.md) uit om de apparaatsjabloon **Aangesloten airconditioner** te maken om mee te werken.

## <a name="create-a-telemetry-based-rule"></a>Een regel op basis van telemetrie maken

1. Als u een ​​nieuwe op telemetrie gebaseerde regel wilt toevoegen aan uw toepassing, kiest u in het navigatiemenu aan de linkerkant **Device Explorer**:

    ![Pagina Device Explorer](media/tutorial-configure-rules/explorerpage1.png)

    U ziet de apparaatsjabloon **Aangesloten airconditioner (1.0.0)**  en het apparaat **Aangesloten airconditioner-1** dat u in de vorige zelfstudie hebt gemaakt.

2. Als u uw aangesloten airconditioningapparaat wilt aanpassen, kiest u het apparaat dat u in de vorige zelfstudie hebt gemaakt:

    ![Pagina Aangesloten airconditioner](media/tutorial-configure-rules/builderdevicelist1.png)

3. Als u een regel wilt toevoegen in de weergave **Regels**, kiest u **Regels** en klikt u vervolgens op **Sjabloon toevoegen**:

    ![Weergave Regels](media/tutorial-configure-rules/builderedittemplate.png)

4. Klik om een telemetrieregel op basis van een drempelwaarde te maken op **Nieuwe regel** en vervolgens op **Telemetrie**.

    ![Sjabloon bewerken](media/tutorial-configure-rules/buildernewrule.png)

5. Gebruik de informatie in de volgende tabel om uw regel te definiëren:

    | Instelling                                      | Waarde                             |
    | -------------------------------------------- | ------------------------------    |
    | Naam                                         | Waarschuwing temperatuur airconditioner |
    | Regel inschakelen voor alle apparaten van deze sjabloon | Aan                                |
    | Regel voor dit apparaat inschakelen                   | Aan                                |
    | Voorwaarde                                    | Temperatuur is hoger dan 32    |
    | Aggregatie                                  | Geen                              |

    ![Voorwaarde temperatuurregel](media/tutorial-configure-rules/buildertemperaturerule1.png)

## <a name="add-an-action"></a>Een actie toevoegen

Wanneer u een regel definieert, definieert u ook een actie die moet worden uitgevoerd wanneer aan de voorwaarden van de regel wordt voldaan. In deze zelfstudie voegt u een actie toe om een ​​e-mail te verzenden als melding dat de regel is geactiveerd.

1. Als u een **Actie**wilt toevoegen, moet u de regel eerst **Opslaan**. Daarna schuift u omlaag in het deelvenster **Telemetrieregel configureren** en kiest u **+**, naast **Acties**, en selecteert u vervolgens **E-mail**:

    ![Actie temperatuurregel](media/tutorial-configure-rules/builderaddaction1.png)

2. Gebruik de informatie in de volgende tabel om uw actie te definiëren:

    | Instelling   | Waarde                          |
    | --------- | ------------------------------ |
    | Handeling        | Uw e-mailadres             |
    | Opmerkingen     | De temperatuur van de airconditioner heeft de drempelwaarde overschreden. |

    > [!NOTE]
    > Om een e-mailmelding te ontvangen, moet het e-mailadres een [gebruikers-ID in de toepassing](howto-administer.md) zijn, en moet die gebruiker zich minimaal één keer hebben aangemeld bij de toepassing.

    ![Temperatuuractie in Application Builder](media/tutorial-configure-rules/buildertemperatureaction.png)

3. Kies **Opslaan**. De regel wordt weergegeven op de pagina **Regels**:

    ![Regels in Application Builder](media/tutorial-configure-rules/builderrules1.png)

4. Kies **Gereed** om de modus **Sjabloon bewerken** af te sluiten.
 

## <a name="test-the-rule"></a>De regel testen

Kort nadat u de regel hebt opgeslagen, wordt deze actief. Wanneer aan de voorwaarden in de regel wordt voldaan, verzendt uw toepassing een bericht naar het e-mailadres dat u in de actie hebt opgegeven.

![E-mailactie](media/tutorial-configure-rules/email.png)

> [!NOTE]
> Nadat u klaar bent met testen, schakelt u de regel uit om geen waarschuwingen meer te ontvangen in uw postvak. 

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

<!-- Repeat task list from intro -->
> [!div class="nextstepaction"]
> * Een regel op basis van telemetrie maken
> * Een actie toevoegen

Nu u een regel op basis van drempelwaarden hebt gedefinieerd, is de voorgestelde volgende stap [de weergaven van de operator aan te passen ](tutorial-customize-operator.md).

Zie voor meer informatie over verschillende soorten regels in Azure IoT Central en over het parametriseren van de regeldefinitie:
* [Een telemetrieregel maken en meldingen instellen](howto-create-telemetry-rules.md).
* [Een gebeurtenisregel maken en meldingen instellen](howto-create-event-rules.md).

<!-- Next tutorials in the sequence -->
