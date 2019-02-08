---
title: Het gebruik van Batch Transcriptie - spraakservices
titlesuffix: Azure Cognitive Services
description: Batch transcriptie is ideaal als u wilt dat een grote hoeveelheid audio in opslag, zoals Azure Blobs transcriberen. U kunt met behulp van de toegewezen REST-API, wijst u audio-bestanden met een shared access signature (SAS) URI en asynchroon ontvangen transcripties.
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: panosper
ms.custom: seodec18
ms.openlocfilehash: 0e03c388dac4a70fc45150287154406551ac2672
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55867117"
---
# <a name="why-use-batch-transcription"></a>Waarom Batch transcriptie gebruiken?

Batch transcriptie is ideaal als u wilt dat een grote hoeveelheid audio in opslag, zoals Azure Blobs transcriberen. U kunt met behulp van de toegewezen REST-API, wijst u audio-bestanden met een shared access signature (SAS) URI en asynchroon ontvangen transcripties.

>[!NOTE]
> Een standard-abonnement (S0) voor Speech Services is vereist voor het gebruik van batch transcriptie. Gratis abonnementssleutels (F0) werkt niet. Zie voor meer informatie, [prijzen en beperkingen](https://azure.microsoft.com/en-us/pricing/details/cognitive-services/speech-services/).

## <a name="the-batch-transcription-api"></a>De Batch-transcriptie API

De Batch-API voor transcriptie biedt asynchrone transcriptie van spraak naar tekst, samen met extra functies. Er is een REST-API die methoden voor het beschrijft:

1. Het maken van batch verwerken van aanvragen
1. Querystatus
1. Transcripties downloaden

> [!NOTE]
> De Batch-API voor transcriptie is ideaal voor callcenters, die meestal worden verzameld duizenden uur aan audio. De API wordt geleid door een 'geactiveerd en vergeten' filosofie, waardoor u gemakkelijk grote hoeveelheden audio-opnamen transcriberen.

### <a name="supported-formats"></a>Ondersteunde indelingen

De Batch-API voor transcriptie ondersteunt de volgende indelingen:

| Indeling | Codec | Bitrate | Samplefrequentie |
|--------|-------|---------|-------------|
| WAV | PCM | 16-bits | 8 of 16 kHz, mono, stereo |
| MP3 | PCM | 16-bits | 8 of 16 kHz, mono, stereo |
| OGG | OPUS | 16-bits | 8 of 16 kHz, mono, stereo |

> [!NOTE]
> De Batch-API voor transcriptie vereist een S0-sleutel (betalen laag). Het werkt niet met een sleutel gratis (f0).

De Batch-transcriptie API wordt de linker- en -kanaal voor stereo audiostreams gesplitst tijdens de transcriptie. De twee JSON-bestanden met het resultaat zijn alle gemaakt van één kanaal. De tijdstempels per utterance bieden de ontwikkelaar te maken van een geordende definitieve transcriptie. De volgende JSON ziet u een voorbeeld van een aanvraag, includuing eigenschappen voor het instellen van de grof taalgebruik filteren, het model interpunctie en word-niveau tijdstempels

```json
{
  "recordingsUrl": "https://contoso.com/mystoragelocation",
  "models": [],
  "locale": "en-US",
  "name": "Transcription using locale en-US",
  "description": "An optional description of the transcription.",
  "properties": {
    "ProfanityFilterMode": "Masked",
    "PunctuationMode": "DictatedAndAutomatic",
    "AddWordLevelTimestamps" : "True"
  },
```

> [!NOTE]
> De Batch transcriptie-API maakt gebruik van een REST-service voor het aanvragen van transcripties, hun status en de bijbehorende resultaten. U kunt de API vanuit een willekeurige taal gebruiken. De volgende sectie wordt beschreven hoe de API wordt gebruikt.

### <a name="query-parameters"></a>Queryparameters

Deze parameters kunnen worden opgenomen in de query-tekenreeks van de REST-aanvraag.

| Parameter | Description | Vereiste / optioneel |
|-----------|-------------|---------------------|
| `ProfanityFilterMode` | Geeft aan hoe grof taalgebruik in herkenningsresultaten worden verwerkt. Geaccepteerde waarden zijn `none` die wordt uitgeschakeld grof taalgebruik filteren, `masked` die grof taalgebruik vervangen door sterretjes, `removed` waarbij alle scheldwoorden worden verwijderd uit het resultaat, of `tags` zodat 'grof taalgebruik' tags aan wordt toegevoegd. De standaardinstelling is `masked`. | Optioneel |
| `PunctuationMode` | Geeft aan hoe interpunctie in herkenningsresultaten worden verwerkt. Geaccepteerde waarden zijn `none` die wordt uitgeschakeld interpunctie, `dictated` dit expliciete interpunctie houdt `automatic` waarmee de decoder interpunctie, behandelt of `dictatedandautomatic` dit houdt bepaald leestekens of automatisch. | Optioneel |


## <a name="authorization-token"></a>Autorisatietoken

Als met alle functies van de spraak-service die u een abonnementssleutel van maakt de [Azure-portal](https://portal.azure.com) door onze [introductiehandleiding](get-started.md). Als u van plan bent om op te halen transcripties van onze basislijn-modellen, is het maken van een sleutel alles die wat u nodig om te doen.

Als u wilt aanpassen en gebruiken van een aangepast model, het abonnementssleutel toevoegen aan de portal voor aangepaste spraak door het volgende te doen:

1. Aanmelden bij [aangepaste spraak](https://customspeech.ai).

2. Selecteer in de rechterbovenhoek, **abonnementen**.

3. Selecteer **verbinding maken met bestaande abonnement**.

4. In het pop-upvenster, moet u de abonnementssleutel en een alias toevoegen.

    ![Het venster abonnement toevoegen](media/stt/Subscriptions.jpg)

5. Kopieer en plak deze sleutel in de clientcode in het volgende voorbeeld.

> [!NOTE]
> Als u van plan bent te gebruiken van een aangepast model, moet u de ID van dit model te. Deze ID is niet de eindpunt-ID die u op de detailweergave van het eindpunt vinden. Het is de model-ID die u ophalen kunt wanneer u de details van dit model selecteert.

## <a name="sample-code"></a>Voorbeeldcode

Pas de volgende voorbeeldcode met een abonnementssleutel en een API-sleutel. Deze actie kunt u een bearer-token verkrijgen.

```cs
     public static CrisClient CreateApiV2Client(string key, string hostName, int port)

        {
            var client = new HttpClient();
            client.Timeout = TimeSpan.FromMinutes(25);
            client.BaseAddress = new UriBuilder(Uri.UriSchemeHttps, hostName, port).Uri;
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", key);

            return new CrisClient(client);
        }
```

Nadat u de token, ophalen, geeft u de SAS-URI die verwijst naar het audiobestand waarvoor transcriptie. De rest van de code doorloopt de status en de resultaten worden weergegeven. Op het eerste gezicht instellen van de sleutel, regio, modellen en het gebruik en de SA, zoals wordt weergegeven in het volgende codefragment. Vervolgens maakt instantiëren u de client en de POST-aanvraag.

```cs
            private const string SubscriptionKey = "<your Speech subscription key>";
            private const string HostName = "westus.cris.ai";
            private const int Port = 443;

            // SAS URI
            private const string RecordingsBlobUri = "SAS URI pointing to the file in Azure Blob Storage";

            // adapted model Ids
            private static Guid AdaptedAcousticId = new Guid("guid of the acoustic adaptation model");
            private static Guid AdaptedLanguageId = new Guid("guid of the language model");

            // Creating a Batch Transcription API Client
            var client = CrisClient.CreateApiV2Client(SubscriptionKey, HostName, Port);

            var transcriptionLocation = await client.PostTranscriptionAsync(Name, Description, Locale, new Uri(RecordingsBlobUri), new[] { AdaptedAcousticId, AdaptedLanguageId }).ConfigureAwait(false);
```

Nu dat u de aanvraag hebt gemaakt, kunt u query's uitvoeren en downloaden van de resultaten transcriptie, zoals wordt weergegeven in het volgende codefragment:

```cs

            // get all transcriptions for the user
            transcriptions = await client.GetTranscriptionAsync().ConfigureAwait(false);

            // for each transcription in the list we check the status
            foreach (var transcription in transcriptions)
            {
                switch(transcription.Status)
                {
                    case "Failed":
                    case "Succeeded":

                            // we check to see if it was one of the transcriptions we created from this client.
                        if (!createdTranscriptions.Contains(transcription.Id))
                        {
                            // not created from here, continue
                            continue;
                        }

                        completed++;

                        // if the transcription was successful, check the results
                        if (transcription.Status == "Succeeded")
                        {
                            var resultsUri = transcription.ResultsUrls["channel_0"];
                            WebClient webClient = new WebClient();
                            var filename = Path.GetTempFileName();
                            webClient.DownloadFile(resultsUri, filename);
                            var results = File.ReadAllText(filename);
                            Console.WriteLine("Transcription succeeded. Results: ");
                            Console.WriteLine(results);
                        }

                    break;
                    case "Running":
                    running++;
                     break;
                    case "NotStarted":
                    notStarted++;
                    break;

                    }
                }
            }
        }
```

Zie voor meer informatie over het aanroepen van de voorgaande onze [swagger-document](https://westus.cris.ai/swagger/ui/index). Voor het volledige voorbeeld dat hier wordt weergegeven, gaat u naar [GitHub](https://github.com/PanosPeriorellis/Speech_Service-BatchTranscriptionAPI).

> [!NOTE]
> In de bovenstaande code wordt de abonnementssleutel is van de spraak-resource die u in Azure portal maakt. Sleutels die u via de Custom Speech Service resource werken niet.

Noteer de asynchrone instellingen voor het plaatsen van audio en transcriptie status ontvangen. De client die u maakt, is een .NET-HTTP-client. Er is een `PostTranscriptions` methode voor het verzenden van de details van de audio-bestand en een `GetTranscriptions` methode voor het ontvangen van de resultaten. `PostTranscriptions` retourneert een ingang en `GetTranscriptions` gebruikt voor het maken van een greep om de status van de transcriptie.

De huidige voorbeeldcode opgeven niet een aangepast model. De service maakt gebruik van de basislijn-modellen voor te transcriberen van het bestand of de bestanden. U kunt doorgeven om op te geven de modellen, op dezelfde manier als de model-id voor de akoestische en het taalmodel.

Als u niet wilt gebruiken van de basislijn, geeft u de model-id's voor zowel akoestische en modellen.

> [!NOTE]
> Voor basislijn transcripties hebt u geen om aan te geven van de eindpunten van de basislijn-modellen. Als u gebruiken van aangepaste modellen wilt, bieden u de eindpunten id's als de [voorbeeld](https://github.com/PanosPeriorellis/Speech_Service-BatchTranscriptionAPI). Als u een akoestisch basislijn gebruiken met een basislijn taalmodel wilt, hebt u alleen id van het eindpunt van het aangepaste model declareren Microsoft detecteert het model van de basislijn partner&mdash;of om akoestische of taal&mdash;en gebruikt deze om de transcriptie-aanvraag niet voltooien.

### <a name="supported-storage"></a>Ondersteunde opslag

Momenteel de enige opslag ondersteund door Azure Blob-opslag.

## <a name="download-the-sample"></a>Het voorbeeld downloaden

U kunt het voorbeeld in dit artikel vinden op [GitHub](https://github.com/PanosPeriorellis/Speech_Service-BatchTranscriptionAPI).

> [!NOTE]
> WE bieden een SLA van de tijd voor audio trascriptions via batch. Echter, zodra de taak transcriptie mailbericht (in de status actief) is, is typially verwerkt sneller dan de real-time.

## <a name="next-steps"></a>Volgende stappen

* [Uw proefabonnement voor Speech ophalen](https://azure.microsoft.com/try/cognitive-services/)
