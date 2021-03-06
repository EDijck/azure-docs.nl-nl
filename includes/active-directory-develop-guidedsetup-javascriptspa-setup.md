---
title: bestand opnemen
description: bestand opnemen
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: nacanuma
ms.custom: include file
ms.openlocfilehash: 805b773795e7c8a96fe1b94abecd860475a4c7a1
ms.sourcegitcommit: abeefca6cd5ca01c3e0b281832212aceff08bf3e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "64993293"
---
## <a name="setting-up-your-web-server-or-project"></a>Instellen van uw webserver of project

> Voorkeur voor het downloaden van dit voorbeeldproject in plaats daarvan?
> - [Download de projectbestanden](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/quickstart.zip) om uit te voeren met een lokale webserver, zoals knooppunt
>
> of
> - (Optioneel) [Downloaden van het Visual Studio-project](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/vsquickstart.zip) om uit te voeren met IIS-server
>
> En ga vervolgens verder met de [configuratiestap](#register-your-application) configureren in het codevoorbeeld voordat deze wordt uitgevoerd.

## <a name="prerequisites"></a>Vereisten
Een lokale webserver zoals [Node.js](https://nodejs.org/en/download/), [.NET Core](https://www.microsoft.com/net/core), of integratie met IIS Express [Visual Studio 2017](https://www.visualstudio.com/downloads/) is vereist voor het uitvoeren van deze zelfstudie.

Als u Node.js gebruikt om uit te voeren van het project, installeert u een IDE zoals [Visual Studio Code](https://code.visualstudio.com/download) de projectbestanden te bewerken.

Instructies in deze handleiding zijn gebaseerd op Node.js en Visual Studio 2017, maar gebruik een andere ontwikkelomgeving of webserver.

## <a name="create-your-project"></a>Uw project maken

> ### <a name="option-1-node-other-web-servers"></a>Optie 1: Knooppunt / andere webservers
> Zorg ervoor dat u hebt geïnstalleerd [Node.js](https://nodejs.org/en/download/), volg de onderstaande stappen:
> - Maak een map voor het hosten van uw toepassing.

<p><!-- -->

> ### <a name="option-2-visual-studio"></a>Optie 2: Visual Studio
> Als u met behulp van Visual Studio en een nieuw project maakt, volgt u de stappen hieronder om een nieuwe Visual Studio-oplossing maken:
> 1.    In Visual Studio:  **Bestand > Nieuw > Project**
> 2.    Onder **Visual C# \Web**, selecteer **ASP.NET-webtoepassing (.NET Framework)**
> 3.    Voer een naam voor uw toepassing en selecteer **OK**
> 4.    Onder **nieuwe ASP.NET-webtoepassing**, selecteer **leeg zijn**

## <a name="create-your-single-page-applications-ui"></a>Maken van de gebruikersinterface van uw toepassing met één pagina
1. Maak een `index.html` -bestand voor uw JavaScript beveiligd-WACHTWOORDVERIFICATIE. Als u van Visual Studio gebruikmaakt, selecteer het project (project-basismap), klik met de rechtermuisknop en selecteer: **Toevoegen > Nieuw Item > HTML-pagina** en noem het index.html.

2. Voeg de volgende code naar uw pagina:
   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <title>Quickstart for MSAL JS</title>
       <script src="https://cdnjs.cloudflare.com/ajax/libs/bluebird/3.3.4/bluebird.min.js"></script>
       <script src="https://secure.aadcdn.microsoftonline-p.com/lib/1.0.0-preview.4/js/msal.js"></script>
   </head>
   <body>
       <h2>Welcome to MSAL.js Quickstart</h2><br/>
       <h4 id="WelcomeMessage"></h4>
       <button id="SignIn" onclick="signIn()">Sign In</button><br/><br/>
       <pre id="json"></pre>
       <script>
           //JS code
       </script>
   </body>
   </html>
   ```

   > [!TIP]
   > U kunt de versie van MSAL.js in bovenstaande script vervangen door de meest recente releaseversie onder [MSAL.js releases](https://github.com/AzureAD/microsoft-authentication-library-for-js/releases).
