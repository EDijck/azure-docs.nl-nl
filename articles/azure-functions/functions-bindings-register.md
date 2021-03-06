---
title: Azure Functions-binding extensies registreren
description: Informatie over het registreren van een Azure Functions-bindingsuitbreiding op basis van uw omgeving.
services: functions
documentationcenter: na
author: craigshoemaker
manager: jeconnoc
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 02/25/2019
ms.author: cshoe
ms.openlocfilehash: 802e177b6f3844abe4d24c26b7ea2d0d4fb1688c
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2019
ms.locfileid: "64697000"
---
# <a name="register-azure-functions-binding-extensions"></a>Azure Functions-binding extensies registreren

Vanaf Azure Functions versie 2.x [bindingen](./functions-triggers-bindings.md) zijn beschikbaar als afzonderlijke pakketten in de functions-runtime. Terwijl de .NET-functies via NuGet-pakketten toegang krijgen tot bindingen, extensie bundels andere functies toegang geven tot alle bindingen via een configuratie-instelling.

Houd rekening met de volgende items die betrekking hebben op het binden van extensies:

- Bindinguitbreidingen expliciet worden niet geregistreerd in de functies 1.x, behalve wanneer [het maken van een C# met behulp van Visual Studio 2017-klassenbibliotheek vormt](#local-csharp).

- HTTP- en timer triggers standaard worden ondersteund en vereisen geen extensie.

De volgende tabel geeft aan wanneer en hoe u de bindingen registreren.

| Ontwikkelomgeving |Registratie<br/> in de functies 1.x  |Registratie<br/> in de functies 2.x  |
|-------------------------|------------------------------------|------------------------------------|
|Azure Portal|Automatisch|Automatisch|
|Niet-.NET-talen of lokale Azure Core Tools-ontwikkeling|Automatisch|[Gebruik Azure Functions Core Tools en extensie bundels](#local-development-with-azure-functions-core-tools-and-extension-bundles)|
|C#-klassenbibliotheek vormt met behulp van Visual Studio 2017|[NuGet-hulpprogramma's gebruiken](#c-class-library-with-visual-studio-2017)|[NuGet-hulpprogramma's gebruiken](#c-class-library-with-visual-studio-2017)|
|C#-klassenbibliotheek vormt met behulp van Visual Studio Code|N/A|[.NET Core CLI gebruiken](#c-class-library-with-visual-studio-code)|

## <a name="local-development-with-azure-functions-core-tools-and-extension-bundles"></a>Lokale ontwikkeling met Azure Functions Core Tools en extensie bundels

[!INCLUDE [functions-core-tools-install-extension](../../includes/functions-core-tools-install-extension.md)]

<a name="local-csharp"></a>
## <a name="c-class-library-with-visual-studio-2017"></a>C#-klassenbibliotheek vormt met Visual Studio 2017

In **Visual Studio 2017**, u kunt pakketten installeren vanuit de Package Manager-Console met behulp van de [Install-Package](https://docs.microsoft.com/nuget/tools/ps-ref-install-package) opdracht, zoals wordt weergegeven in het volgende voorbeeld:

```powershell
Install-Package Microsoft.Azure.WebJobs.Extensions.ServiceBus -Version <TARGET_VERSION>
```

De naam van het pakket wordt gebruikt voor een bepaalde binding is opgegeven in het referentieartikel voor die binding. Zie voor een voorbeeld: de [pakketten gedeelte van het Service Bus-binding verwijzing artikel](functions-bindings-service-bus.md#packages---functions-1x).

Vervang `<TARGET_VERSION>` in het voorbeeld met een specifieke versie van het pakket, zoals `3.0.0-beta5`. Geldige versies worden weergegeven op de afzonderlijke pakket's op de [NuGet.org](https://nuget.org). De primaire versies die met Functions-runtime overeenkomen 1.x of 2.x zijn opgegeven in het artikel verwijzing voor de binding.

## <a name="c-class-library-with-visual-studio-code"></a>C#-klassenbibliotheek vormt met Visual Studio Code

In **Visual Studio Code**, u kunt pakketten installeren vanuit de opdrachtprompt met de [dotnet-pakket toevoegen](https://docs.microsoft.com/dotnet/core/tools/dotnet-add-package) opdracht in de .NET Core CLI, zoals wordt weergegeven in het volgende voorbeeld:

```terminal
dotnet add package Microsoft.Azure.WebJobs.Extensions.ServiceBus --version <TARGET_VERSION>
```

De .NET Core-CLI kan alleen worden gebruikt voor het ontwikkelen van Azure Functions 2.x.

De naam van het pakket moet worden gebruikt voor een bepaalde binding is opgegeven in het referentieartikel voor die binding. Zie voor een voorbeeld: de [pakketten gedeelte van het Service Bus-binding verwijzing artikel](functions-bindings-service-bus.md#packages---functions-1x).

Vervang `<TARGET_VERSION>` in het voorbeeld met een specifieke versie van het pakket, zoals `3.0.0-beta5`. Geldige versies worden weergegeven op de afzonderlijke pakket's op de [NuGet.org](https://nuget.org). De primaire versies die met Functions-runtime overeenkomen 1.x of 2.x zijn opgegeven in het artikel verwijzing voor de binding.

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Azure functie-trigger en binding voorbeeld](./functions-bindings-example.md)

