---
title: SDK's en voorbeelden - Content Moderator, Python, Java, Node.js en .NET
titlesuffix: Azure Cognitive Services
description: SDK's en voorbeelden voor Content Moderator ophalen
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: sample
ms.date: 02/27/2018
ms.author: sajagtap
ms.openlocfilehash: fd71a48372bcdb459bb3b7509e9a9c5dba529555
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60607110"
---
# <a name="content-moderator-sdks-and-samples"></a>SDK's en voorbeelden voor Content Moderator

## <a name="sdks-for-python-java-nodejs-and-net"></a>SDK's voor Python, Java, Node.js en .NET

- [Content Moderator SDK voor Python](https://pypi.python.org/pypi/azure-cognitiveservices-vision-contentmoderator)
- [Content Moderator SDK voor Java](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-cognitiveservices-contentmoderator%22)
- [Content Moderator SDK voor Node.js](https://www.npmjs.com/package/azure-cognitiveservices-contentmoderator)
- [Content Moderator SDK voor .NET](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/)

## <a name="net-sdk-samples"></a>.NET SDK-voorbeelden

De volgende lijst bevat links naar de codevoorbeelden die zijn gecompileerd met behulp van de Azure Content Moderator SDK voor .NET.

- **Hulpbibliotheek**: [Een Content Moderator-client maken voor gebruik in andere voorbeelden](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ModeratorHelper/Clients.cs). Zie [snelstart](content-moderator-helper-quickstart-dotnet.md).

### <a name="moderation"></a>Toezicht 
- **Beheer van afbeeldingen**: [Een afbeelding controleren op erotische en ongepaste inhoud, tekst en gezichten](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageModeration/Program.cs). Zie [snelstart](image-moderation-quickstart-dotnet.md).
- **Aangepaste afbeeldingen**: [Inhoud beheren met aangepaste afbeeldingslijsten](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageListManagement/Program.cs). Zie [snelstart](image-lists-quickstart-dotnet.md).

> [!NOTE]
> Er is een maximumlimiet van **5 afbeeldingslijsten** waarbij elke lijst **niet meer dan 10.000 afbeeldingen mag bevatten**.
>

- **Tekstbeheer**: [Tekst voor grof taalgebruik en persoonlijke gegevens scherm](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/TextModeration/Program.cs). Zie [snelstart](text-moderation-quickstart-dotnet.md).
- **Aangepaste terminologie**: [Inhoud beheren met aangepaste terminologielijsten](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/TermListManagement/Program.cs). Zie [snelstart](term-lists-quickstart-dotnet.md).

> [!NOTE]
> Er is een maximumlimiet van **5 terminologielijsten** waarbij elke lijst **niet meer dan 10.000 termen mag bevatten**.
>

- **Beheer van video's**: [Een video controleren op erotische en ongepaste inhoud en resultaten ontvangen](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoModeration/Program.cs). Zie [snelstart](video-moderation-api.md).

### <a name="review"></a>Beoordelen
- **Taken voor afbeeldingen**: [Een beheertaak starten om beoordelingen te scannen en maken](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageJobs/Program.cs). Zie [snelstart](moderation-jobs-quickstart-dotnet.md).
- **Beoordelingen van afbeeldingen**: [Beoordelingen maken voor HITL (human-in-the-loop)](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageReviews/Program.cs). Zie [snelstart](moderation-reviews-quickstart-dotnet.md).
- **Videobeoordelingen**: [Videobeoordelingen maken voor HITL (human-in-the-loop)](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoReviews/Program.cs). Zie [Snelstart](video-reviews-quickstart-dotnet.md)
- **Videotranscriptiebeoordelingen**: [Beoordelingen van videotranscripties maken voor HITL (human-in-the-loop)](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoTranscriptReviews/Program.cs) Zie de [quickstart](video-reviews-quickstart-dotnet.md)

Zie alle .NET-voorbeelden bij de [Voorbeelden van Content Moderator .NET op GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator).

## <a name="rest-api-samples-in-c"></a>REST API-voorbeelden in C#

- [Beheer van afbeeldingen](https://github.com/MicrosoftContentModerator/ContentModerator-API-Samples/tree/master/ImageModeration)
- [Beheer van tekst](https://github.com/MicrosoftContentModerator/ContentModerator-API-Samples/tree/master/TextModeration)
- [Beheer van video](https://github.com/MicrosoftContentModerator/ContentModerator-API-Samples/tree/master/VideoModeration)
- [Beoordelingen van afbeeldingen](https://github.com/MicrosoftContentModerator/ContentModerator-API-Samples/tree/master/ImageReviews)
- [Taken voor afbeeldingen](https://github.com/MicrosoftContentModerator/ContentModerator-API-Samples/tree/master/ImageJob)

Bekijk de [webinar op aanvraag](https://info.microsoft.com/cognitive-services-content-moderator-ondemand.html) om een beeld te krijgen van deze voorbeelden.

## <a name="tutorials"></a>Zelfstudies
- [Beheer van eCommerce-catalogus](https://github.com/MicrosoftContentModerator/samples-eCommerceCatalogModeration). Zie [zelfstudie](ecommerce-retail-catalog-moderation.md).
- [Beheer van Facebook-inhoud](https://github.com/MicrosoftContentModerator/samples-fbPageModeration). Zie [zelfstudie](facebook-post-moderation.md).
- [Beheer van video en transcriptie oplossing voor beheer](https://github.com/MicrosoftContentModerator/VideoReviewConsoleApp) Zie [zelfstudie](video-transcript-moderation-review-tutorial-dotnet.md)

## <a name="on-demand-webinars"></a>Webinars op aanvraag
- [Geautomatiseerd inhoudstoezicht op schaal met Content Moderator](https://info.microsoft.com/cognitive-services-content-moderator-ondemand.html)
