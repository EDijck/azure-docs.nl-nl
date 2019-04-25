---
title: Azure AD v2 Windows Desktop aan de slag - Config | Microsoft Docs
description: Hoe een toepassing voor Windows Desktop .NET (XAML) een toegangstoken en een API die wordt beveiligd door Azure Active Directory v2-eindpunt aanroepen.
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: dc9b5cbc4904c6b2238e56742381feb4d2811437
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60301138"
---
# <a name="add-the-applications-registration-information-to-your-app"></a>De registratiegegevens van de toepassing toevoegen aan uw app
In deze stap moet u de toepassings-Id toevoegen aan uw project.

1.  Open `App.xaml.cs` en vervang de regel die de `ClientId` met:

```csharp
private static string ClientId = "[Enter the application Id here]";
```

### <a name="what-is-next"></a>Wat is de volgende

[!INCLUDE [Test and Validate](../../../../includes/active-directory-develop-guidedsetup-windesktop-test.md)]
