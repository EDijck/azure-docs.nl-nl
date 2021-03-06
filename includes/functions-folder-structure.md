---
title: bestand opnemen
description: bestand opnemen
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: include
origin.date: 09/12/2018
ms.date: 10/19/2018
ms.author: v-junlch
ms.custom: include file
ms.openlocfilehash: 27dc1b1315a8e33b8ac13b34d4a86ad0343388b4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60731183"
---
De code voor alle functies in een specifieke functie-app bevindt zich in een hoofdmap van het project met een configuratiebestand voor de host en een of meer submappen. Elke submap bevat de code voor een afzonderlijke functie, zoals in de volgende afbeelding:

```
FunctionApp
 | - host.json
 | - Myfirstfunction
 | | - function.json
 | | - ...  
 | - mysecondfunction
 | | - function.json
 | | - ...  
 | - SharedCode
 | - bin
```

In versie 2.x van de Functions-runtime, alle functies in de functie-app moet dezelfde taal werknemer delen.  

De [host.json](../articles/azure-functions/functions-host-json.md) bestand, dat een runtime-specifieke configuraties bevat, is in de hoofdmap van de functie-app. Een `bin` map bevat pakketten en andere bibliotheekbestanden die vereist zijn voor de functie-app. Zie de taal-specifieke vereisten voor een functie-app-project:

- [C# class library (.csproj)](../articles/azure-functions/functions-dotnet-class-library.md#functions-class-library-project)
- [C# script (.csx)](../articles/azure-functions/functions-reference-csharp.md#folder-structure)
- [F#script](../articles/azure-functions/functions-reference-fsharp.md#folder-structure)
- [Java](../articles/azure-functions/functions-reference-java.md#folder-structure)
- [JavaScript](../articles/azure-functions/functions-reference-node.md#folder-structure)


<!-- ms.date: 10/19/2018 -->

