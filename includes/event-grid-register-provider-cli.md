---
title: bestand opnemen
description: bestand opnemen
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: include
ms.date: 08/17/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: ace22a6896a121f5cd8af838c7b0e427bd0287dc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60427640"
---
## <a name="enable-event-grid-resource-provider"></a>Event Grid-resourceprovicer inschakelen

Als u Event Grid in uw Azure-abonnement nog niet eerder hebt gebruikt, moet u mogelijk de Event Grid-resourceprovider registreren. Voer de volgende opdracht uit om de provider te registreren:

```azurecli-interactive
az provider register --namespace Microsoft.EventGrid
```

Het kan even duren voordat de registratie is voltooid. Voer de volgende opdracht uit om de status te controleren:

```azurecli-interactive
az provider show --namespace Microsoft.EventGrid --query "registrationState"
```

Wanneer `registrationState` `Registered` is, bent u klaar om door te gaan.