---
title: Voorbeeld van Azure CLI-script - Definitie van een beheerde toepassing maken | Microsoft Docs
description: Voorbeeld van Azure CLI-script - Definitie van een beheerde toepassing maken
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
ms.service: managed-applications
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/25/2017
ms.author: tomfitz
ms.openlocfilehash: 79af2918c2342d1a0cc3b11763c30b35ab5e9f72
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "61043889"
---
# <a name="create-a-managed-application-definition-with-azure-cli"></a>Een definitie van een beheerde toepassing maken met Azure CLI

Met dit script implementeert u de definitie van een beheerde toepassing naar een servicecatalogus. 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeldscript

[!code-azurecli[main](../../../cli_scripts/managed-applications/create-definition/create-definition.sh "Create definition")]


## <a name="script-explanation"></a>Uitleg van het script

In dit script wordt de volgende opdracht gebruikt om de definitie van de beheerde toepassing te maken. Elke opdracht in de tabel is een koppeling naar opdracht-specifieke documentatie.

| Opdracht | Opmerkingen |
|---|---|
| [az managedapp definition create](https://docs.microsoft.com/cli/azure/managedapp/definition#az-managedapp-definition-create) | Hiermee maakt u de definitie van een beheerde toepassing. Geef het pakket op dat de vereiste bestanden bevat. |


## <a name="next-steps"></a>Volgende stappen

* Zie [Overzicht van door Azure beheerde toepassingen](../overview.md) voor algemene informatie over beheerde toepassingen.
* Raadpleeg de [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure) voor meer informatie over de Azure CLI.
