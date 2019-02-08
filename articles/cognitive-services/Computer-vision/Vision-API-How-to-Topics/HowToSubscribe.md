---
title: Abonnementssleutels - Computer Vision ophalen
titlesuffix: Azure Cognitive Services
description: Informatie over het verkrijgen van abonnementssleutels voor aanroepen naar de Computer Vision-API in Azure Cognitive Services.
services: cognitive-services
author: KellyDF
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: article
ms.date: 05/19/2017
ms.author: kefre
ms.custom: seodec18
ms.openlocfilehash: 08838ce0af16cc4ae768bd5d2ecf72c57f8fae97
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55858073"
---
# <a name="how-to-obtain-subscription-keys"></a>Abonnementssleutels ophalen

Computer Vision-services vereist speciale abonnementssleutels. Voor elke aanroep naar de Computer Vision-API is een abonnementssleutel vereist. Deze sleutel moet worden doorgegeven via een tekenreeksparameter of zijn opgegeven in de aanvraagheader.

Als u zich voor abonnementssleutels, Zie [abonnementen](https://azure.microsoft.com/try/cognitive-services/). Het is gratis te registreren. Prijzen voor deze services is onderhevig aan wijzigingen.

>[!NOTE]
Uw abonnementssleutels zijn geldig voor slechts één van deze [Microsoft Azure-regio's](https://azure.microsoft.com/regions/). 

| Regio | Adres |
|---|---|
| US - west | westus.API.cognitive.Microsoft.com |
| US - oost 2 | eastus2.api.cognitive.microsoft.com |
| US - west-centraal | westcentralus.api.cognitive.microsoft.com |
| Europa -west | westeurope.api.cognitive.microsoft.com |
| Azië - zuidoost | southeastasia.api.cognitive.microsoft.com |

Als u zich registreert met behulp van de gratis proefversie van Computer Vision, uw abonnementssleutels zijn geldig voor de **westcentral** regio (`https://westcentralus.api.cognitive.microsoft.com/`). Dat is het meest voorkomende geval. Echter, als u zich aanmeldt voor Computer Vision met uw Microsoft Azure-via account de [ https://azure.microsoft.com/ ](https://azure.microsoft.com/) website, u de regio opgeven voor de evaluatie van de voorgaande lijst met regio's.

Bijvoorbeeld, als u zich registreren voor Computer Vision met uw Microsoft Azure-account en u geeft `westus` voor uw regio, moet u de `westus` regio voor uw REST API-aanroepen (`https://westus.api.cognitive.microsoft.com/`).

Als u de regio voor uw abonnementssleutel nadat u hebt verkregen van de sleutel van uw proefversie vergeet, kunt u uw regio op [ https://azure.microsoft.com/try/cognitive-services/my-apis/ ](https://azure.microsoft.com/try/cognitive-services/my-apis/).

### <a name="related-links"></a>Verwante koppelingen:

* [Prijsopties voor Azure Cognitive Services-API 's](https://azure.microsoft.com/pricing/details/cognitive-services/)
