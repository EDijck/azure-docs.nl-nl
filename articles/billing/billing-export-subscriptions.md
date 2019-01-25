---
title: Uw Azure-abonnement bovenste niveau gegevens exporteren | Microsoft Docs
description: Hierin wordt beschreven hoe u alle Azure-abonnement id's die zijn gekoppeld aan uw account kunt bekijken.
keywords: ''
services: billing
documentationcenter: ''
author: adpick
manager: adpick
editor: ''
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/15/2018
ms.author: banders
ms.openlocfilehash: a2716793a47eae56a8e6eef8416ff446b6f7176f
ms.sourcegitcommit: 644de9305293600faf9c7dad951bfeee334f0ba3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2019
ms.locfileid: "54902564"
---
# <a name="export-and-view-your-top-level-subscription-information"></a>Exporteren en uw abonnementsgegevens op het hoogste niveau weergeven
Als u wilt weergeven van de set van abonnement-id's die zijn gekoppeld aan uw gebruikersreferenties [downloaden van een .json-bestand met gegevens van uw abonnement van het Azure-Accountcentrum](http://account.azure.com/subscriptions/download).

[!INCLUDE [gdpr-dsr-and-stp-note](../../includes/gdpr-dsr-and-stp-note.md)]

De gedownloade .json-bestand bevat de volgende informatie:
- E-mail: Het e-mailadres dat is gekoppeld aan uw account.
- De PUID: De unieke id die is gekoppeld aan uw factureringsrekening.
- SubscriptionIds: Een lijst met abonnementen die deel uitmaken van uw account, geïnventariseerd door abonnements-ID.

### <a name="subscriptionsjson-sample"></a>Subscriptions.JSON voorbeeld

```json
{
  "Email":"admin@contoso.com",
  "Puid":"00052xxxxxxxxxxx",
  "SubscriptionIds":[
    "38124d4d-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "7c8308f1-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "39a25f2b-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "52ec2489-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "e42384b2-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "90757cdc-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
  ]
}
```
