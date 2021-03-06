---
title: Aan de slag met de Java SDK voor Azure Media Services | Microsoft Docs
description: In deze zelfstudie wordt u begeleid bij het implementeren van een basisservice voor levering van VoD-inhoud (Video-on-Demand) met de AMS-toepassing (Azure Media Services) via Java.
services: media-services
documentationcenter: java
author: juliako
manager: femila
editor: johndeu
ms.assetid: b884bd61-dbdb-42ea-b170-8fb02e7fded7
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: conceptual
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: fedec6ea764394d36f5b4d7c883f7cb9f9520a29
ms.sourcegitcommit: e7d4881105ef17e6f10e8e11043a31262cfcf3b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/29/2019
ms.locfileid: "64869023"
---
# <a name="get-started-with-the-java-client-sdk-for-azure-media-services"></a>Aan de slag met de SDK voor Java-clients voor Azure Media Services  

> [!NOTE]
> Er worden geen nieuwe functies of functionaliteit meer aan Media Services v2. toegevoegd. <br/>Maak kennis met de nieuwste versie, [Media Services v3](https://docs.microsoft.com/azure/media-services/latest/). Zie ook [hulp bij de migratie van v2 naar v3](../latest/migrate-from-v2-to-v3.md)

In deze zelfstudie wordt u begeleid bij het implementeren van een basisservice voor levering van video-inhoud met Azure Media Services via de SDK voor Java-clients.

## <a name="prerequisites"></a>Vereisten

Hieronder wordt aangegeven wat de vereisten zijn om deze zelfstudie te voltooien:

* Een Azure-account. Zie [Gratis proefversie van Azure](https://azure.microsoft.com/pricing/free-trial/) voor meer informatie.
* Een Media Services-account. Zie [Een Media Services-account maken](media-services-portal-create-account.md) voor meer informatie over het maken van een Media Services-account.
* De huidige [Azure Media Services Java SDK](https://mvnrepository.com/artifact/com.microsoft.azure/azure-media/latest)

## <a name="how-to-import-the-azure-media-services-java-client-sdk-package"></a>Procedure: Importeer het pakket voor de Azure Media Services Java client SDK

Voeg een verwijzing naar de huidige versie (0.9.8) van het `azure-media`-pakket vanuit de [Azure Media Services Java SDK](https://mvnrepository.com/artifact/com.microsoft.azure/azure-media/latest) toe om de Media Services SDK voor Java te gebruiken.

Als uw build-hulpprogramma bijvoorbeeld `gradle` is, voegt u de volgende afhankelijkheid aan uw `build.gradle`-bestand toe:

    compile group: 'com.microsoft.azure', name: 'azure-media', version: '0.9.8'

>[!IMPORTANT]
>De SDK voegt ondersteuning toe voor AAD-verificatie (Azure Active Directory) en verwijdert ondersteuning voor Azure ACS-verificatie (Access Control Service) vanaf de `azure-media`-pakketversie `0.9.8`. We raden u aan om zo snel mogelijk naar het Azure Active Directory-verificatiemodel te migreren. Lees het artikel [Access the Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md) (Toegang tot de API van Azure Media Services met Azure Active Directory-verificatie) voor meer informatie over migratie.

>[!NOTE]
>U vindt de broncode van de Azure Media Services Java SDK in onze [GitHub-opslagplaats](https://github.com/Azure/azure-sdk-for-java/tree/0.9/services/azure-media). Zorg ervoor dat u overschakelt naar de branch 0.9 en niet de master branch. 

## <a name="how-to-use-azure-media-services-with-java"></a>Procedure: Azure Media Services gebruiken met Java

>[!NOTE]
>Wanneer uw Media Services-account is gemaakt, wordt er een **standaard** streaming-eindpunt met de status **Gestopt** aan uw account toegevoegd. Als u inhoud wilt streamen en gebruik wilt maken van dynamische pakketten en dynamische versleuteling, moet het streaming-eindpunt van waar u inhoud wilt streamen, de status **Wordt uitgevoerd** hebben.

De volgende code geeft aan hoe u een asset maakt, een mediabestand naar de asset uploadt, een taak uitvoert met de opdracht om de asset te transformeren en een locator maakt om uw video te streamen.

Stel een Media Services-account in voordat u deze code gebruikt. Zie [Een Media Services-account maken](media-services-portal-create-account.md) voor meer informatie over het instellen van een account.

De code maakt verbinding met de Azure Media Services API met behulp van service-principal-verificatie via Azure Active Directory. Maak een Azure Active Directory-toepassing en geef de waarden voor de volgende variabelen in de code op:
* `tenant`: Het Azure Active Directory-tenantdomein waarin de Azure Active Directory-toepassing zich bevindt
* `clientId`: De client-id van de Azure Active Directory-toepassing
* `clientKey`: De client-sleutel van de Azure Active Directory-toepassing
* `restApiEndpoint`: Het REST API-eindpunt van het Azure Media Services-account

U kunt een Azure Active Directory-toepassing maken en de voorgaande configuratiewaarden ophalen via Azure Portal. Zie het gedeelte **Service principal authentication** (Verificatie van service-principal) in [Getting started with Azure AD authentication using the Azure portal](https://docs.microsoft.com/azure/media-services/media-services-portal-get-started-with-aad) (Aan de slag met Azure Active Directory-verificatie via Azure Portal) voor meer informatie.

De code maakt ook gebruik van een lokaal opgeslagen videobestand. U moet de code bewerken om uw eigen lokale bestand te krijgen om te uploaden.

```java
    import java.io.*;
    import java.net.URI;
    import java.security.NoSuchAlgorithmException;
    import java.util.EnumSet;
    import java.util.concurrent.ExecutorService;
    import java.util.concurrent.Executors;

    import com.microsoft.windowsazure.Configuration;
    import com.microsoft.windowsazure.exception.ServiceException;
    import com.microsoft.windowsazure.services.media.MediaConfiguration;
    import com.microsoft.windowsazure.services.media.MediaContract;
    import com.microsoft.windowsazure.services.media.MediaService;
    import com.microsoft.windowsazure.services.media.WritableBlobContainerContract;
    import com.microsoft.windowsazure.services.media.authentication.AzureAdClientSymmetricKey;
    import com.microsoft.windowsazure.services.media.authentication.AzureAdTokenCredentials;
    import com.microsoft.windowsazure.services.media.authentication.AzureAdTokenProvider;
    import com.microsoft.windowsazure.services.media.authentication.AzureEnvironments;
    import com.microsoft.windowsazure.services.media.models.AccessPolicy;
    import com.microsoft.windowsazure.services.media.models.AccessPolicyInfo;
    import com.microsoft.windowsazure.services.media.models.AccessPolicyPermission;
    import com.microsoft.windowsazure.services.media.models.Asset;
    import com.microsoft.windowsazure.services.media.models.AssetFile;
    import com.microsoft.windowsazure.services.media.models.AssetFileInfo;
    import com.microsoft.windowsazure.services.media.models.AssetInfo;
    import com.microsoft.windowsazure.services.media.models.Job;
    import com.microsoft.windowsazure.services.media.models.JobInfo;
    import com.microsoft.windowsazure.services.media.models.JobState;
    import com.microsoft.windowsazure.services.media.models.ListResult;
    import com.microsoft.windowsazure.services.media.models.Locator;
    import com.microsoft.windowsazure.services.media.models.LocatorInfo;
    import com.microsoft.windowsazure.services.media.models.LocatorType;
    import com.microsoft.windowsazure.services.media.models.MediaProcessor;
    import com.microsoft.windowsazure.services.media.models.MediaProcessorInfo;
    import com.microsoft.windowsazure.services.media.models.Task;

    public class Program
    {
        // Media Services account credentials configuration
        private static String tenant = "tenant.domain.com";
        private static String clientId = "<client id>";
        private static String clientKey = "<client key>";
        private static String restApiEndpoint = "https://account_name.restv2.region_name.media.azure.net/api/";

        // Media Services API
        private static MediaContract mediaService;

        // Encoder configuration
        // This is using the default Adaptive Streaming encoding preset. 
        // You can choose to use a custom preset, or any other sample defined preset. 
        // In addition you can use other processors, like Speech Analyzer, or Redactor if desired.
        private static String preferredEncoder = "Media Encoder Standard";
        private static String encodingPreset = "Adaptive Streaming";

        public static void main(String[] args)
        {
            ExecutorService executorService = Executors.newFixedThreadPool(1);

            try {
                // Setup Azure AD Service Principal Symmetric Key Credentials
                AzureAdTokenCredentials credentials = new AzureAdTokenCredentials(
                        tenant,
                        new AzureAdClientSymmetricKey(clientId, clientKey),
                        AzureEnvironments.AZURE_CLOUD_ENVIRONMENT);

                AzureAdTokenProvider provider = new AzureAdTokenProvider(credentials, executorService);

                // Create a new configuration with the credentials
                Configuration configuration = MediaConfiguration.configureWithAzureAdTokenProvider(
                        new URI(restApiEndpoint),
                        provider);

                // Create the media service provisioned with the new configuration
                mediaService = MediaService.create(configuration);

                // Upload a local file to an Asset
                AssetInfo uploadAsset = uploadFileAndCreateAsset("Video Name", "C:/path/to/video.mp4");
                System.out.println("Uploaded Asset Id: " + uploadAsset.getId());

                // Transform the Asset
                AssetInfo encodedAsset = encode(uploadAsset);
                System.out.println("Encoded Asset Id: " + encodedAsset.getId());

                // Create the Streaming Origin Locator
                String url = getStreamingOriginLocator(encodedAsset);

                System.out.println("Origin Locator URL: " + url);
                System.out.println("Sample completed!");

            } catch (ServiceException se) {
                System.out.println("ServiceException encountered.");
                System.out.println(se.toString());
            } catch (Exception e) {
                System.out.println("Exception encountered.");
                System.out.println(e.toString());
            } finally {
                executorService.shutdown();
            }
        }

        private static AssetInfo uploadFileAndCreateAsset(String assetName, String fileName)
            throws ServiceException, FileNotFoundException, NoSuchAlgorithmException {

            WritableBlobContainerContract uploader;
            AssetInfo resultAsset;
            AccessPolicyInfo uploadAccessPolicy;
            LocatorInfo uploadLocator = null;

            // Create an Asset
            resultAsset = mediaService.create(Asset.create().setName(assetName).setAlternateId("altId"));
            System.out.println("Created Asset " + fileName);

            // Create an AccessPolicy that provides Write access for 15 minutes
            uploadAccessPolicy = mediaService
                .create(AccessPolicy.create("uploadAccessPolicy", 15.0, EnumSet.of(AccessPolicyPermission.WRITE)));

            // Create a Locator using the AccessPolicy and Asset
            uploadLocator = mediaService
                .create(Locator.create(uploadAccessPolicy.getId(), resultAsset.getId(), LocatorType.SAS));

            // Create the Blob Writer using the Locator
            uploader = mediaService.createBlobWriter(uploadLocator);

            File file = new File(fileName);

            // The local file that will be uploaded to your Media Services account
            InputStream input = new FileInputStream(file);

            System.out.println("Uploading " + fileName);

            // Upload the local file to the media asset
            uploader.createBlockBlob(file.getName(), input);

            // Inform Media Services about the uploaded files
            mediaService.action(AssetFile.createFileInfos(resultAsset.getId()));
            System.out.println("Uploaded Asset File " + fileName);

            mediaService.delete(Locator.delete(uploadLocator.getId()));
            mediaService.delete(AccessPolicy.delete(uploadAccessPolicy.getId()));

            return resultAsset;
        }

        // Create a Job that contains a Task to transform the Asset
        private static AssetInfo encode(AssetInfo assetToEncode)
            throws ServiceException, InterruptedException {

            // Retrieve the list of Media Processors that match the name
            ListResult<MediaProcessorInfo> mediaProcessors = mediaService
                            .list(MediaProcessor.list().set("$filter", String.format("Name eq '%s'", preferredEncoder)));

            // Use the latest version of the Media Processor
            MediaProcessorInfo mediaProcessor = null;
            for (MediaProcessorInfo info : mediaProcessors) {
                if (null == mediaProcessor || info.getVersion().compareTo(mediaProcessor.getVersion()) > 0) {
                    mediaProcessor = info;
                }
            }

            System.out.println("Using Media Processor: " + mediaProcessor.getName() + " " + mediaProcessor.getVersion());

            // Create a task with the specified Media Processor
            String outputAssetName = String.format("%s as %s", assetToEncode.getName(), encodingPreset);
            String taskXml = "<taskBody><inputAsset>JobInputAsset(0)</inputAsset>"
                    + "<outputAsset assetCreationOptions=\"0\"" // AssetCreationOptions.None
                    + " assetName=\"" + outputAssetName + "\">JobOutputAsset(0)</outputAsset></taskBody>";

            Task.CreateBatchOperation task = Task.create(mediaProcessor.getId(), taskXml)
                    .setConfiguration(encodingPreset).setName("Encoding");

            // Create the Job; this automatically schedules and runs it.
            Job.Creator jobCreator = Job.create()
                    .setName(String.format("Encoding %s to %s", assetToEncode.getName(), encodingPreset))
                    .addInputMediaAsset(assetToEncode.getId()).setPriority(2).addTaskCreator(task);
            JobInfo job = mediaService.create(jobCreator);

            String jobId = job.getId();
            System.out.println("Created Job with Id: " + jobId);

            // Check to see if the Job has completed
            checkJobStatus(jobId);
            // Done with the Job

            // Retrieve the output Asset
            ListResult<AssetInfo> outputAssets = mediaService.list(Asset.list(job.getOutputAssetsLink()));
            return outputAssets.get(0);
        }


        public static String getStreamingOriginLocator(AssetInfo asset) throws ServiceException {
            // Get the .ISM AssetFile
            ListResult<AssetFileInfo> assetFiles = mediaService.list(AssetFile.list(asset.getAssetFilesLink()));
            AssetFileInfo streamingAssetFile = null;
            for (AssetFileInfo file : assetFiles) {
                if (file.getName().toLowerCase().endsWith(".ism")) {
                    streamingAssetFile = file;
                    break;
                }
            }

            AccessPolicyInfo originAccessPolicy;
            LocatorInfo originLocator = null;

            // Create a 30-day read only AccessPolicy
            double durationInMinutes = 60 * 24 * 30;
            originAccessPolicy = mediaService.create(
                    AccessPolicy.create("Streaming policy", durationInMinutes, EnumSet.of(AccessPolicyPermission.READ)));

            // Create a Locator using the AccessPolicy and Asset
            originLocator = mediaService
                    .create(Locator.create(originAccessPolicy.getId(), asset.getId(), LocatorType.OnDemandOrigin));

            // Create a Smooth Streaming base URL
            return originLocator.getPath() + streamingAssetFile.getName() + "/manifest";
        }

        private static void checkJobStatus(String jobId) throws InterruptedException, ServiceException {
            boolean done = false;
            JobState jobState = null;
            while (!done) {
                // Sleep for 5 seconds
                Thread.sleep(5000);

                // Query the updated Job state
                jobState = mediaService.get(Job.get(jobId)).getState();
                System.out.println("Job state: " + jobState);

                if (jobState == JobState.Finished || jobState == JobState.Canceled || jobState == JobState.Error) {
                    done = true;
                }
            }
        }
    }
```

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="additional-resources"></a>Aanvullende resources
Zie [Azure Java Developer Center][Azure Java Developer Center] en [Azure voor Java-ontwikkelaars][Azure for Java developers] voor meer informatie over het ontwikkelen van Java-toepassingen in Azure.


Zie [Azure-bibliotheken voor Java-documentatie][Azure-bibliotheken voor Java-documentatie] voor Javadoc-documentatie voor Media Services.

<!-- URLs. -->

[Azure Media Services SDK Maven Package]: https://mvnrepository.com/artifact/com.microsoft.azure/azure-media/latest
[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Azure for Java developers]: https://docs.microsoft.com/java/azure/
[Media Services Client Development]: https://msdn.microsoft.com/library/windowsazure/dn223283.aspx

