---
title: 'Quickstart: Omzetten van spraak, Java (Windows, Linux) - Speech Services'
titleSuffix: Azure Cognitive Services
description: In deze quickstart maakt u een eenvoudige Java-toepassing om spraak van gebruikers vast te leggen, deze om te zetten in een andere taal en de tekst uit te voeren naar de opdrachtregel. Deze handleiding is bedoeld voor Windows- en Linux-gebruikers.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: quickstart
ms.date: 12/13/2018
ms.author: erhopf
ms.openlocfilehash: e0c5139203b360be1eed2292d3ca07c9948b2ca5
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53729517"
---
# <a name="quickstart-translate-speech-with-the-speech-sdk-for-java"></a>Quickstart: Omzetten van spraak met de Speech SDK voor Java

In deze quickstart maakt u een eenvoudige Java-toepassing waarmee spraak van gebruikers via de microfoon van de computer wordt vastgelegd, waarna de spraak wordt omgezet en de omgezette tekst in realtime wordt getranscribeerd naar de opdrachtregel. De toepassing is ontworpen voor gebruik met 64-bits Windows of 64-bits Ubuntu Linux 16.04/18.04, en wordt gebouwd met het Speech SDK Maven-pakket en de Eclipse Java IDE.

Zie [Taalondersteuning](language-support.md) voor een volledige lijst met talen die voor spraakomzetting beschikbaar zijn.

## <a name="prerequisites"></a>Vereisten

Voor deze snelstart zijn de volgende zaken vereist:

* Besturingssysteem: 64-bits Windows of 64-bits Ubuntu Linux 16.04/18.04
* [Eclipse Java IDE](https://www.eclipse.org/downloads/)
* [Java 8](https://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) of [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* Een Azure-abonnementssleutel voor de Spraakservice. [Gratis downloaden](get-started.md).

Als u Ubuntu 16.04/18.04 gebruikt, zorg er dan voor dat deze afhankelijkheden zijn geïnstalleerd voordat u Eclipse start.

```console
sudo apt-get update
sudo apt-get install build-essential libssl1.0.0 libcurl3 libasound2 wget
```

> [!NOTE]
> Zie [Speech Devices SDK](speech-devices-sdk.md) voor de Speech Devices-SDK en het Roobo-apparaat.

## <a name="create-and-configure-project"></a>Project maken en configureren

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-java-create-proj.md)]

## <a name="add-sample-code"></a>Voorbeeldcode toevoegen

1. Om een nieuwe lege klasse aan uw Java-project toe te voegen, selecteert u **Bestand** > **Nieuw** > **Klasse**.

1. Voer in het venster **Nieuwe Java-klasse**, in het veld **Pakket**, **speechsdk.quickstart** in en voer in het veld **Naam** **Main** in.

   ![Schermopname van het venster Nieuwe Java-klasse](media/sdk/qs-java-jre-06-create-main-java.png)

1. Vervang alle code in `Main.java` door het volgende codefragment:

   ```java
   package speechsdk.quickstart;

   import java.io.IOException;
   import java.util.Map;
   import java.util.Scanner;
   import java.util.concurrent.ExecutionException;
   import com.microsoft.cognitiveservices.speech.*;
   import com.microsoft.cognitiveservices.speech.translation.*;

   public class Main {

       public static void translationWithMicrophoneAsync() throws InterruptedException, ExecutionException, IOException
       {
           // Creates an instance of a speech translation config with specified
           // subscription key and service region. Replace with your own subscription key
           // and service region (e.g., "westus").
           SpeechTranslationConfig config = SpeechTranslationConfig.fromSubscription("YourSubscriptionKey", "YourServiceRegion");

           // Sets source and target language(s).
           String fromLanguage = "en-US";
           config.setSpeechRecognitionLanguage(fromLanguage);
           config.addTargetLanguage("de");

           // Sets voice name of synthesis output.
           String GermanVoice = "Microsoft Server Speech Text to Speech Voice (de-DE, Hedda)";
           config.setVoiceName(GermanVoice);

           // Creates a translation recognizer using microphone as audio input.
           TranslationRecognizer recognizer = new TranslationRecognizer(config);
           {
               // Subscribes to events.
               recognizer.recognizing.addEventListener((s, e) -> {
                   System.out.println("RECOGNIZING in '" + fromLanguage + "': Text=" + e.getResult().getText());

                   Map<String, String> map = e.getResult().getTranslations();
                   for(String element : map.keySet()) {
                       System.out.println("    TRANSLATING into '" + element + "': " + map.get(element));
                   }
               });

               recognizer.recognized.addEventListener((s, e) -> {
                   if (e.getResult().getReason() == ResultReason.TranslatedSpeech) {
                       System.out.println("RECOGNIZED in '" + fromLanguage + "': Text=" + e.getResult().getText());

                       Map<String, String> map = e.getResult().getTranslations();
                       for(String element : map.keySet()) {
                           System.out.println("    TRANSLATED into '" + element + "': " + map.get(element));
                       }
                   }
                   if (e.getResult().getReason() == ResultReason.RecognizedSpeech) {
                       System.out.println("RECOGNIZED: Text=" + e.getResult().getText());
                       System.out.println("    Speech not translated.");
                   }
                   else if (e.getResult().getReason() == ResultReason.NoMatch) {
                       System.out.println("NOMATCH: Speech could not be recognized.");
                   }
               });

               recognizer.synthesizing.addEventListener((s, e) -> {
                   System.out.println("Synthesis result received. Size of audio data: " + e.getResult().getAudio().length);
               });

               recognizer.canceled.addEventListener((s, e) -> {
                   System.out.println("CANCELED: Reason=" + e.getReason());

                   if (e.getReason() == CancellationReason.Error) {
                       System.out.println("CANCELED: ErrorCode=" + e.getErrorCode());
                       System.out.println("CANCELED: ErrorDetails=" + e.getErrorDetails());
                       System.out.println("CANCELED: Did you update the subscription info?");
                   }
               });

               recognizer.sessionStarted.addEventListener((s, e) -> {
                   System.out.println("\nSession started event.");
               });

               recognizer.sessionStopped.addEventListener((s, e) -> {
                   System.out.println("\nSession stopped event.");
               });

               // Starts continuous recognition. Uses StopContinuousRecognitionAsync() to stop recognition.
               System.out.println("Say something...");
               recognizer.startContinuousRecognitionAsync().get();

               System.out.println("Press any key to stop");
               new Scanner(System.in).nextLine();

               recognizer.stopContinuousRecognitionAsync().get();
           }
       }

       public static void main(String[] args) {
           try {
               translationWithMicrophoneAsync();
           } catch (Exception ex) {
               System.out.println("Unexpected exception: " + ex.getMessage());
               assert(false);
               System.exit(1);
           }
       }
   }
   ```

1. Vervang de tekenreeks `YourSubscriptionKey` door uw abonnementssleutel.

1. Vervang de tekenreeks `YourServiceRegion` door de [regio](regions.md) die aan uw abonnement is gekoppeld (bijvoorbeeld `westus` voor het gratis proefabonnement).

1. Sla de wijzigingen in het project op.

## <a name="build-and-run-the-app"></a>De app bouwen en uitvoeren

Druk op F11 of selecteer **Uitvoeren** > **Fouten opsporen**.

De spraakinvoer van de microfoon wordt herkend, getranscribeerd naar het Duits en weergegeven in het consolevenster. Druk op Enter om te stoppen met het vastleggen van spraak.

![Schermafbeelding van console-uitvoer na geslaagde herkenning](media/sdk/qs-translate-java-jre-output.png)

## <a name="next-steps"></a>Volgende stappen

Op GitHub vindt u aanvullende voorbeelden, zoals hoe u spraak kunt lezen vanuit een audiobestand en omgezette tekst als kunstmatige spraak kunt uitvoeren.

> [!div class="nextstepaction"]
> [Bekijk Java-voorbeelden op GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Zie ook

- [Snelstart: Spraak herkennen, Java (Windows, Linux)](quickstart-java-jre.md)
- [Akoestische modellen aanpassen](how-to-customize-acoustic-models.md)
- [Taalmodellen aanpassen](how-to-customize-language-model.md)
