---
title: DRM dynamische versleuteling en licentie leveringsservice voor gebruik met Azure Media Services | Microsoft Docs
description: U kunt Azure Media Services gebruiken voor het leveren van streams die zijn versleuteld met licenties van Microsoft PlayReady, Google Widevine of Apple FairPlay.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 24ea6b2b44518b4cf75389585caf42ff6bc6722f
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65191074"
---
# <a name="tutorial-use-drm-dynamic-encryption-and-license-delivery-service"></a>Zelfstudie: Het gebruik van de Digital Rights Management-service voor dynamische versleuteling en licentielevering

U kunt Azure Media Services gebruiken voor het leveren van streams die zijn versleuteld met licenties van Microsoft PlayReady, Google Widevine of Apple FairPlay. Zie voor gedetailleerde uitleg [Content protection met dynamische versleuteling](content-protection-overview.md).

Media Services voorziet bovendien in een service voor het leveren van DRM-licenties van PlayReady, Widevine en FairPlay. Wanneer een gebruiker door DRM beveiligde inhoud aanvraagt, wordt met de spelertoepassing een licentie aangevraagd bij de licentieservice van Media Services. Als de spelertoepassing is geautoriseerd, ontvangt de speler een licentie van de licentieservice van Media Services. Een licentie bevat de ontsleutelingssleutel die kan worden gebruikt door de clientspeler om de inhoud te ontsleutelen en te streamen.

Dit artikel is gebaseerd op het voorbeeld waarbij [DRM wordt gebruikt voor de versleuteling](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM). 

In het voorbeeld in dit artikel wordt het volgende resultaat bereikt:

![AMS met door DRM beschermde video](./media/protect-with-drm/ams_player.png)

In deze zelfstudie ontdekt u hoe u:    

> [!div class="checklist"]
> * Maak een codering transformatie
> * De ondertekeningssleutel gebruikt voor verificatie van uw token instellen
> * Vereisten op het beleid voor de inhoud van de sleutels instellen
> * Een StreamingLocator maken met de opgegeven streaming beleid
> * Maak een URL die wordt gebruikt om af te spelen uw bestand

## <a name="prerequisites"></a>Vereisten

Hieronder wordt aangegeven wat de vereisten zijn om de zelfstudie te voltooien.

* Lees het artikel [Content protection overview](content-protection-overview.md) (Overzicht inhoudsbeveiliging).
* Controleer de [multi-DRM beveiligde inhoud systeem met toegangsbeheer ontwerpen](design-multi-drm-system-with-access-control.md)
* Visual Studio Code of Visual Studio installeren
* Maak een nieuw Media Services-account zoals beschreven in deze [snelstart](create-account-cli-quickstart.md).
* Zorg dat u referenties hebt die nodig zijn om Media Services-API's te kunnen gebruiken via [toegangs-API's](access-api-cli-how-to.md)
* Stel de juiste waarden in in het configuratiebestand van de toepassing (appsettings.json).

## <a name="download-code"></a>Code downloaden

Kloon met de volgende opdracht een GitHub-opslagplaats met het volledige .NET-voorbeeld dat in dit artikel wordt beschreven, op de computer:

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials.git
 ```
 
Het voorbeeld 'Versleutelen met DRM' bevindt zich in de map [EncryptWithDRM](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM).

> [!NOTE]
> Met het voorbeeld worden unieke resources gemaakt telkens wanneer u de toepassing uitvoert. Normaal gesproken gebruikt u bestaande resources, zoals transformaties en beleidsregels opnieuw (mits de bestaande resource over de vereiste configuraties beschikt). 

## <a name="start-using-media-services-apis-with-net-sdk"></a>Starten met het gebruik van Media Services API's met .NET SDK

Als u wilt starten met Media Services API's met .NET, moet u een **AzureMediaServicesClient**-object maken. Als u het object wilt maken, moet u referenties opgeven die de client nodig heeft om verbinding te maken met Azure met behulp van Microsoft Azure Active Directory. In de code die u aan het begin van het artikel hebt gekloond, wordt met de functie **GetCredentialsAsync** het object ServiceClientCredentials gemaakt op basis van de referenties die zijn opgegeven in het lokale configuratiebestand. 

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#CreateMediaServicesClient)]

## <a name="create-an-output-asset"></a>Een uitvoeractivum maken  

In de [uitvoerasset](https://docs.microsoft.com/rest/api/media/assets) wordt het resultaat van de coderingstaak opgeslagen.  

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#CreateOutputAsset)]
 
## <a name="get-or-create-an-encoding-transform"></a>Een coderingstransformatie verkrijgen of maken

Bij het maken van een nieuw [transformatie](https://docs.microsoft.com/rest/api/media/transforms)-exemplaar, moet u opgeven wat u als uitvoer wilt maken. De vereiste parameter is een **TransformOutput**-object, zoals weergegeven in de onderstaande code. Elke **transformatie-uitvoer** bevat een **voorinstelling**. **Voorinstelling** bevat de stapsgewijze instructies van de video- en/of audioverwerkingen die moeten worden gebruikt voor het genereren van de gewenste **TransformOutput**. Het voorbeeld dat in dit artikel wordt beschreven, maakt gebruik van een ingebouwde voorinstelling genaamd **AdaptiveStreaming** . De voorinstelling codeert de invoervideo in een automatisch gegenereerde bitrate-ladder (bitrate-resolutieparen) op basis van de invoerresolutie en bitsnelheid en produceert ISO MP4-bestanden met H.264-video en AAC-audio die overeenkomen met elk bitrate-resolutiepaar. 

Voordat u een nieuwe [transformatie](https://docs.microsoft.com/rest/api/media/transforms) gaat maken, moet u eerst controleren of er al een bestaat. Dit doet u met de methode **Ophalen**, zoals weergegeven in de volgende code.  In Media Services-v3 retourneert de methode **Ophalen** van entiteiten **null** als de entiteit (een hoofdlettergevoelige controle van de naam).

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#EnsureTransformExists)]

## <a name="submit-job"></a>Taak indienen

Zoals eerder vermeld, is het [transformatie](https://docs.microsoft.com/rest/api/media/transforms)-object het recept en is de [taak](https://docs.microsoft.com/rest/api/media/jobs) de werkelijke aanvraag bij Media Services om deze **transformatie** toe te passen op een bepaalde invoervideo of audio-inhoud. De **taak** bevat informatie zoals de locatie van de invoervideo en de locatie voor de uitvoer.

In deze zelfstudie maakt u invoer voor de taak op basis van een bestand dat rechtstreeks vanuit een [HTTPs bron-URL](job-input-from-http-how-to.md) is opgenomen.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#SubmitJob)]

## <a name="wait-for-the-job-to-complete"></a>Wacht tot de taak is voltooid

De taak neemt enige tijd in beslag en wanneer deze is voltooid, wordt u hiervan op de hoogte gesteld. In het onderstaande codevoorbeeld ziet u hoe de status van de [taak](https://docs.microsoft.com/rest/api/media/jobs) kan worden opgevraagd in de service. Polling is geen aanbevolen best practice voor productietoepassingen vanwege mogelijke latentie. Polling kan worden beperkt bij een te intensief gebruik op een account. Ontwikkelaars moeten in plaats daarvan Event Grid gebruiken. Zie [Gebeurtenissen routeren naar een aangepast eindpunt](job-state-events-cli-how-to.md).

De **taak** doorloopt meestal de volgende statussen: **Gepland**, **In de wachtrij geplaatst**, **Verwerken**, **Voltooid** (de eindstatus). Als bij de taak een fout is opgetreden is, krijgt u de status **Fout**. Als de taak momenteel wordt geannuleerd, krijgt u de melding **Wordt geannuleerd** en **Geannuleerd** wanneer het annuleren is voltooid.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#WaitForJobToFinish)]

## <a name="create-a-contentkeypolicy"></a>Een ContentKeyPolicy maken

Een inhoudssleutel biedt veilige toegang tot uw assets. U moet beleid voor de inhoudssleutel maken om te configureren hoe de inhoudssleutel naar eindclients moet worden geleverd. De inhoudssleutel is gekoppeld aan de StreamingLocator. Media Services verzorgt ook de sleutelleveringsservice waarmee versleutelingssleutels en -licenties aan gemachtigde gebruikers worden geleverd. 

U moet de vereisten (beperkingen) instellen voor inhoudssleutelbeleid waaraan moet worden voldaan voor het leveren van sleutels met de opgegeven configuratie. In dit voorbeeld worden de volgende configuraties en vereisten ingesteld:

* Configuratie 

    De licenties van [PlayReady](playready-license-template-overview.md) en [Widevine](widevine-license-template-overview.md) zijn geconfigureerd om te kunnen worden geleverd door de Media Services-service voor het leveren van licenties. Hoewel met deze voorbeeld-app de licentie van [FairPlay](fairplay-license-overview.md) niet wordt geconfigureerd, bevat deze een methode waarmee u FairPlay kunt configureren. U kunt het configureren van FairPlay als een andere optie toevoegen.

* Beperking

    Door de app wordt voor het beleid een beperking voor tokens van het type JWT-tokens ingesteld.

Wanneer een stream wordt aangevraagd door een speler, maakt Media Services gebruik van de opgegeven sleutel om uw inhoud dynamisch te versleutelen. Voor het ontsleutelen van de stream, wordt door de speler de sleutel van de sleutelleveringsservice aangevraagd. De service evalueert het door u opgegeven beleid voor de inhoudssleutel om te bepalen of de gebruiker is gemachtigd om de sleutel op te halen.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#GetOrCreateContentKeyPolicy)]

## <a name="create-a-streaminglocator"></a>Een StreamingLocator maken

Nadat de codering is voltooid en het inhoudssleutelbeleid is ingesteld, bestaat de volgende stap eruit om de video in de uitvoerasset beschikbaar te maken voor weergave door clients. U doet dit in twee stappen: 

1. Een [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators) maken.
2. De streaming-URL's bouwen die clients kunnen gebruiken. 

Het proces voor het maken van de **StreamingLocator** wordt 'publiceren' genoemd. Standaard is de **StreamingLocator** onmiddellijk geldig nadat u de API-aanroepen hebt gemaakt en totdat deze wordt verwijderd, tenzij u de optionele start- en eindtijden configureert. 

Bij het maken van een [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators) moet u de gewenste **StreamingPolicyName** opgeven. In deze zelfstudie gebruiken we een van de vooraf gedefinieerde StreamingPolicies. Deze worden door Azure Media Services gebruikt om te bepalen hoe de inhoud voor streaming moet worden gepubliceerd. In dit voorbeeld stellen we StreamingLocator.StreamingPolicyName in op het beleid Predefined_MultiDrmCencStreaming. Dit beleid geeft aan dat u wilt dat er twee inhoudssleutels (envelop en CENC) worden gegenereerd en ingesteld voor de locator. Er worden dan de envelop-, PlayReady- en Widevine-coderingen toegepast (de sleutel wordt aan de afspeelclient geleverd op basis van de geconfigureerde DRM-licenties). Als u wilt dat uw stream ook met CBCS (FairPlay) wordt versleuteld, gebruikt u Predefined_MultiDrmStreaming. 

> [!IMPORTANT]
> Wanneer u een aangepast [streamingbeleid](https://docs.microsoft.com/rest/api/media/streamingpolicies) gebruikt, moet u een beperkte set met dergelijke beleidsregels ontwerpen voor uw Media Service-account, en deze opnieuw gebruiken voor de StreamingLocators wanneer dezelfde versleutelingsopties en protocollen nodig zijn. Uw Media Service-account heeft een quotum voor het aantal StreamingPolicy-vermeldingen. U dient geen nieuw StreamingPolicy voor elke StreamingLocator te maken.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#CreateStreamingLocator)]

## <a name="get-a-test-token"></a>Een test-token ophalen
        
In deze zelfstudie geven we voor het inhoudssleutelbeleid op dat het een tokenbeperking heeft. Het beleid met de tokenbeperking moet vergezeld gaan van een token dat is uitgegeven door een beveiligingstokenservice (STS). Media Services biedt ondersteuning voor tokens in de JWT-indelingen [(JSON Web Token)](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) en dat is wat we in het voorbeeld gaan configureren.

De ContentKeyIdentifierClaim wordt in de ContentKeyPolicy gebruikt, wat inhoudt dat de token die aan de sleutelleveringsservice wordt gepresenteerd, de identificatie van de ContentKey moet bevatten. In het voorbeeld wordt geen inhoudssleutel opgegeven bij het maken van de StreamingLocator, het systeem maakt een willekeurige aan. Om de testtoken te kunnen maken, moet de ContentKeyId de claim ContentKeyIdentifierClaim kunnen invoegen.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#GetToken)]

## <a name="build-a-streaming-url"></a>Een streaming-URL maken

Nu de [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators) is gemaakt, kunt u de streaming-URL's ophalen. Als u de URL wilt samenstellen, moet u de hostnaam van het [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/streamingendpoints) en het pad van de **StreamingLocator** samenvoegen. In dit voorbeeld wordt het *standaard* **StreamingEndpoint** gebruikt. Wanneer u een Media Service-account voor het eerst maakt, wordt dit *standaard* **StreamingEndpoint** gestopt. Daarom moet u **Start** aanroepen.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#GetMPEGStreamingUrl)]

Als u de app uitvoert, ziet u het volgende:

![Beschermen met drm](./media/protect-with-drm/playready_encrypted_url.png)

U kunt een browser openen en de resulterende URL erin plakken om de demopagina van Azure Media Player te starten waarin de URL en token al zijn ingevuld. 
 
## <a name="clean-up-resources-in-your-media-services-account"></a>Resources in uw Media Services-account opschonen

Over het algemeen moet u alles opschonen, behalve objecten die u van plan bent te hergebruiken (meestal gebruikt u transformaties opnieuw en behoudt u StreamingLocators, enzovoort). Als u wilt dat uw account na het experiment is opgeschoond, moet u de resources verwijderen die u niet van plan bent te hergebruiken.  Met de volgende code verwijdert u bijvoorbeeld taken.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#CleanUp)]

## <a name="clean-up-resources"></a>Resources opschonen

Als u de resources van de resourcegroep niet meer nodig hebt, met inbegrip van de Media Services en opslagaccounts die u hebt gemaakt voor deze zelfstudie, verwijdert u de resourcegroep die u eerder hebt gemaakt. 

Voer de volgende CLI-opdracht uit:

```azurecli
az group delete --name amsResourceGroup
```

## <a name="ask-questions-give-feedback-get-updates"></a>Stel vragen, feedback geven, updates ophalen

Bekijk de [Azure Media Services-community](media-services-community.md) artikel om te zien van verschillende manieren kunt u vragen stellen, feedback te geven en updates over Media Services ophalen.

## <a name="next-steps"></a>Volgende stappen

Ga naar het

> [!div class="nextstepaction"]
> [Beveiligen met AES-128](protect-with-aes128.md)

