---
title: Maak een Azure Media Services-taak van een lokaal bestand | Microsoft Docs
description: In dit onderwerp laat zien hoe de Taakinvoer van een maken vanuit een lokaal bestand.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 02/10/2019
ms.author: juliako
ms.openlocfilehash: 4860390820c29cd8f44ebbf5e05e8c58090d5740
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "55996929"
---
# <a name="create-a-job-input-from-a-local-file"></a>De Taakinvoer van een maken vanuit een lokaal bestand

Wanneer u taken voor het verwerken van uw video's, verzendt in Media Services v3 hebt u Media Services zien waar vind ik de invoervideo. De invoervideo kan worden opgeslagen als een activum van Media Service, in welk geval het maken van een invoer asset op basis van een bestand (lokaal of in Azure Blob-opslag opgeslagen). In dit onderwerp laat zien hoe de Taakinvoer van een maken vanuit een lokaal bestand. Zie voor een compleet voorbeeld [GitHub voorbeeld](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs).

## <a name="net-sample"></a>.NET-voorbeeld

De volgende code laat zien hoe een invoer asset maken en gebruiken als de invoer voor de taak. De functie CreateInputAsset voert de volgende handelingen uit:

* Hiermee maakt u de Asset
* Haalt een beschrijfbare [SAS-URL](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1) voor de [container in opslag](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-dotnet?tabs=windows#upload-blobs-to-the-container) van de asset
* Uploadt het bestand naar de container in opslag met de SAS-URL

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#CreateInputAsset)]

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#SubmitJob)]

## <a name="next-steps"></a>Volgende stappen

[De Taakinvoer van een maken van een HTTPS-URL](job-input-from-http-how-to.md).
