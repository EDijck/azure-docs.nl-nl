---
title: Wat is de Face-API?
titleSuffix: Azure Cognitive Services
description: Informatie over het gebruik van de Face-service voor het detecteren en analyseren van gezichten op afbeeldingen.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: overview
ms.date: 02/20/2019
ms.author: pafarley
ms.openlocfilehash: 6fba9e6387e60f2156ee01461c3bba22405376fc
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2019
ms.locfileid: "64729572"
---
# <a name="what-is-the-azure-face-api"></a>Wat is de Azure Face-API?

De Face-API van Azure is een cognitieve service die algoritmen levert voor het detecteren, herkennen en analyseren van menselijke gezichten op afbeeldingen. De mogelijkheid menselijke gezichten te verwerken is belangrijk in veel verschillende softwarescenario's, zoals voor beveiliging, natuurlijke gebruikersinterface, analyse van afbeeldingsinhoud, mobiele apps en robotica.

De Face-API biedt verschillende functies die elk in de volgende secties worden beschreven. Lees verder voor meer informatie over deze functies.

## <a name="face-detection"></a>Gezichtsdetectie

De Face APi is in staat om menselijke gezichten te detecteren op een afbeelding en de locatiecoördinaten van de gezichtsrechthoek te retourneren. Gezichtsdetectie kan eventueel een reeks face-gerelateerde kenmerken zoals head houding, geslacht, leeftijd, emotie, gezichtshaar en bril extraheren.

> [!NOTE]
> De gezichtsdetectiefunctie is ook beschikbaar via de [Computer Vision-API](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home), maar als u meer bewerkingen wilt uitvoeren met gezichtsgegevens, moet u de Face-API (deze service) gebruiken.

![Een afbeelding van een vrouw en een man met rechthoeken getekend rond de gezichten, en de leeftijd en het geslacht weergegeven](./Images/Face.detection.jpg)

Zie voor meer informatie over gezichtsdetectie, de [Gezichtsdetectie](concepts/face-detection.md) concepten artikel of zoek het [API detecteren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) verwijzen naar documentatie.

## <a name="face-verification"></a>Gezichtsverificatie

De Verificatie-API voert een verificatie uit op basis van twee gedetecteerde gezichten of op basis van een gedetecteerd gezicht in relatie tot een object van één persoon. Waar het eigenlijk op neerkomt is dat de Verificatie-API evalueert of twee gezichten van dezelfde persoon zijn. Dit kan goed van pas komen in beveiligingsscenario's. Zie voor meer informatie de [Gezichtsherkenning-](concepts/face-recognition.md) gids voor concepten of de [API controleren](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a) verwijzen naar documentatie.

## <a name="find-similar-faces"></a>Vergelijkbare gezichten zoeken

De Vergelijkbare zoeken-API neemt een doelgezicht en een set gezichten die dit doelgezicht zouden kunnen zijn, en zoekt vervolgens naar een kleinere set gezichten die het meest op het doelgezicht lijken. Er worden twee werkingsmodi, **matchPerson** en **matchFace**, ondersteund. De modus **matchPerson** retourneert vergelijkbare gezichten nadat er een filter is toegepast waarbij dezelfde persoon (met behulp van de [Verificatie-API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a)) als uitgangspunt is genomen. De modus **matchFace** negeert de filter met dezelfde persoon en retourneert een lijst van vergelijkbare kandidaatgezichten die al dan niet van dezelfde persoon zijn.

In het volgende voorbeeld is dit het doelgezicht:

![Een glimlachende vrouw](./Images/FaceFindSimilar.QueryFace.jpg)

En dit zijn de kandidaatgezichten:

![Vijf afbeeldingen van glimlachende mensen De afbeeldingen (a) en (b) zijn van dezelfde persoon](./Images/FaceFindSimilar.Candidates.jpg)

Om vier vergelijkbare gezichten te vinden, zou de modus **matchPerson** (a) en (b) moeten retourneren, die dezelfde persoon aanduiden als het doelgezicht. De modus **matchFace** retourneert (a), (b), (c) en (d), precies vier kandidaten, zelfs als er een aantal andere personen zijn dan het doel of daar weinig gelijkenis mee vertonen. Zie voor meer informatie de [Gezichtsherkenning-](concepts/face-recognition.md) gids voor concepten of [vergelijkbare API vinden](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) verwijzen naar documentatie.

## <a name="face-grouping"></a>Gezichtsgroepering

De Groeperings-API verdeelt een set onbekende gezichten in groepen op basis van gelijkenis. Elke groep is een niet-aaneengesloten juiste subset van de oorspronkelijke set van gezichten. Alle gezichten in een groep zijn waarschijnlijk van dezelfde persoon, maar er kunnen diverse verschillende groepen voor één persoon zijn (die zich van elkaar onderscheiden door een andere factor, zoals gezichtsuitdrukking bijvoorbeeld). Zie voor meer informatie de [Gezichtsherkenning-](concepts/face-recognition.md) gids voor concepten of de [groep API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238) verwijzen naar documentatie.

## <a name="person-identification"></a>Identificatie van personen

De Identificatie-API kan worden gebruikt om een gedetecteerd gezicht te identificeren met behulp van een personendatabase. Dit kan nuttig zijn voor het automatisch taggen van afbeeldingen in software voor het beheren van foto's. U maakt de database vooraf en bewerkt deze na verloop van tijd.

De volgende afbeelding laat een voorbeeld zien van een database met de naam 'myfriends'. Elke groep kan tot wel 1.000.000 verschillende persoonsobjecten bevatten, en voor elk persoonsobject kunnen maximaal 248 gezichten zijn geregistreerd.

![Een raster met 3 kolommen voor verschillende personen, elk met 3 rijen van afbeeldingen van gezichten](./Images/person.group.clare.jpg)

Nadat een database is gemaakt en getraind, kunt u een identificatie uitvoeren op basis van de groep en een nieuw gedetecteerd gezicht. Als het gezicht wordt geïdentificeerd als een persoon in de groep, wordt het persoonsobject geretourneerd.

Zie voor meer informatie over de identificatie van personen, de [Gezichtsherkenning-](concepts/face-recognition.md) gids voor concepten of de [identificeren API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) verwijzen naar documentatie.

## <a name="use-containers"></a>Containers gebruiken

[Gebruik de Face-container](face-how-to-install-containers.md) om gezichten te detecteren, herkennen en identificeren. Hiertoe installeert u een gestandaardiseerde Docker-container die zich dichter bij uw gegevens bevindt.

## <a name="sample-apps"></a>Voorbeeld-apps

De volgende voorbeeldtoepassingen tonen een aantal manieren waarop de Face-API kan worden gebruikt.

- [Microsoft Face-API: Windows Clientbibliotheek en voorbeeld](https://github.com/Microsoft/Cognitive-Face-Windows): een WPF-app in verschillende scenario's voor gezichtsdetectie, -analyse en -identificatie.
- [FamilyNotes UWP-app](https://github.com/Microsoft/Windows-appsample-familynotes): een UWP-app (Universal Windows Platform) die gebruikmaakt van gezichtsidentificatie gecombineerd met spraak, Cortana, inkt en camera in een familiescenario waarin notities worden gedeeld.

## <a name="data-privacy-and-security"></a>Gegevensprivacy en -beveiliging

Zoals geldt voor alle services van Cognitive Services, dienen ontwikkelaars die de Face-service gebruiken op de hoogte te zijn van het beleid van Microsoft inzake klantgegevens. Zie de [pagina Cognitive Services](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) (Engelstalig) in het Microsoft Trust Center voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

Volg een snelstart voor het implementeren van een eenvoudig scenario voor gezichtsdetectie in code.

- [Snelstart: gezichten in een afbeelding detecteren met behulp van de .NET SDK met C#](quickstarts/csharp.md) (in diverse talen beschikbaar)
