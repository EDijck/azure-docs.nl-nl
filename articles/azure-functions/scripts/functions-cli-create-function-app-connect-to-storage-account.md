---
title: Een Azure-functie maken die verbinding maakt met een Azure Storage | Microsoft Docs
description: Azure CLI-scriptvoorbeeld - Een Azure-functie maken die verbinding maakt met een Azure Storage
services: functions
documentationcenter: functions
author: ggailey777
manager: jeconnoc
ms.assetid: ''
ms.service: azure-functions
ms.devlang: azurecli
ms.topic: sample
ms.date: 04/20/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 48f6124373d80d22766d319ec58ed4ad7adaf6f5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60325655"
---
# <a name="create-a-function-app-that-connects-to-an-azure-storage-account"></a>Een functie-app maken die verbinding maakt met een Azure Storage-account

Met dit Azure Functions-voorbeeldscript wordt een functie-app gemaakt en wordt de functie verbonden met een Azure Storage-account. De gemaakte app-instelling die de verbinding bevat, kan worden gebruikt met een [opslagtrigger of -binding](../functions-bindings-storage-blob.md). 

[!INCLUDE [upgrade runtime](../../../includes/functions-cli-version-note.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u de CLI lokaal gebruikt, moet u ervoor zorgen dat u Azure CLI versie 2.0 of hoger gebruikt. Voer `az --version` uit om de versie te bekijken. Als u uw CLI wilt installeren of upgraden, raadpleegt u [De Azure CLI installeren](/cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Voorbeeldscript

In dit voorbeeld wordt een Azure-functie-app gemaakt en wordt de opslagverbindingsreeks toegevoegd aan een app-instelling.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-connect-to-storage/create-function-app-connect-to-storage-account.sh "Integrate Function App into Azure Storage Account")]


## <a name="clean-up-deployment"></a>Opschonen van implementatie

Voer nadat het voorbeeldscript is uitgevoerd de volgende opdracht uit om de resourcegroep en alle gerelateerde resources te verwijderen:

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az-group-create) | Hiermee maakt u een resourcegroep met locatie. |
| [az storage account create](https://docs.microsoft.com/cli/azure/storage/account#az-storage-account-create) | Een opslagaccount maken. |
| [az functionapp create](https://docs.microsoft.com/cli/azure/functionapp#az-functionapp-create) | Hiermee maakt u een functie-app in het serverloze [verbruiksabonnement](../functions-scale.md#consumption-plan). |

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure) voor meer informatie over de Azure CLI.

Aanvullende CLI-voorbeeldscripts voor Azure Functions vindt u in de [Azure Functions-documentatie](../functions-cli-samples.md).
