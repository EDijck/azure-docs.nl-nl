---
title: bestand opnemen
description: bestand opnemen
services: functions
author: tdykstra
manager: cfowler
ms.service: functions
ms.topic: include
origin.date: 05/17/2018
ms.date: 05/30/2018
ms.author: v-junlch
ms.custom: include file
ms.openlocfilehash: f8733ef907b8f31ace7ea72f705ba1b37d1adece
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60306723"
---
### <a name="azure-storage-sdk-version-in-functions-1x"></a>Azure Storage-SDK-versie in de functies 1.x

In de functies 1.x, de opslag-triggers en bindingen versie 7.2.1 van de Azure Storage SDK gebruiken ([WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage/7.2.1) NuGet-pakket). Als u naar een andere versie van de Storage-SDK verwijzen, en u op het type van de Storage-SDK in uw functiehandtekening verbindt, kan de Functions-runtime melden dat het kan geen binding met dat type. De oplossing is om te controleren of uw projectverwijzingen [WindowsAzure.Storage 7.2.1](https://www.nuget.org/packages/WindowsAzure.Storage/7.2.1).

<!-- ms.date: 05/30/2018 -->