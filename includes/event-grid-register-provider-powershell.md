---
title: bestand opnemen
description: bestand opnemen
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: include
ms.date: 07/05/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 68a208af1a9aa9e73f2af99021d195f264fb21f1
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/02/2019
ms.locfileid: "55664434"
---
## <a name="enable-event-grid-resource-provider"></a>Event Grid-resourceprovicer inschakelen

Als u Event Grid in uw Azure-abonnement nog niet eerder hebt gebruikt, moet u mogelijk de Event Grid-resourceprovider registreren. Voer de volgende opdracht uit:

```azurepowershell-interactive
Register-AzResourceProvider -ProviderNamespace Microsoft.EventGrid
```

Het kan even duren voordat de registratie is voltooid. Voer de volgende opdracht uit om de status te controleren:

```azurepowershell-interactive
Get-AzResourceProvider -ProviderNamespace Microsoft.EventGrid
```

Wanneer `RegistrationStatus` `Registered` is, bent u klaar om door te gaan.
