---
title: Azure CLI-voorbeeldscript - Peering van twee virtuele netwerken | Microsoft Docs
description: Azure CLI-voorbeeldscript - Peering van twee virtuele netwerken.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: twooley
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 03/20/2018
ms.author: kumud
ms.openlocfilehash: 82f4f06cd4e615df38f2251be9aef9e8578b9df4
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2019
ms.locfileid: "64726796"
---
# <a name="peer-two-virtual-networks-script-sample"></a>Voorbeeldscript voor peering van twee virtuele netwerken

Met dit voorbeeldscript worden twee virtuele netwerken in dezelfde regio gemaakt en verbonden via het Azure-netwerk. Nadat het script is uitgevoerd, hebt u een peering tussen twee virtuele netwerken.

U kunt het script uitvoeren vanuit de Azure [Cloud Shell](https://shell.azure.com/bash) of vanuit een lokale installatie van de Azure CLI. Als u de CLI lokaal gebruikt, hebt u versie 2.0.28 of hoger nodig om dit script uit te voeren. Voer `az --version` uit om na te gaan welke versie er is geïnstalleerd. Als u uw CLI wilt installeren of upgraden, raadpleegt u [De Azure CLI installeren](/cli/azure/install-azure-cli). Als u de CLI lokaal uitvoert, moet u ook `az login` uitvoeren om verbinding te maken met Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a>Voorbeeldscript

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/peer-two-virtual-networks/peer-two-virtual-networks.sh "Peer two networks")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Voer de volgende opdracht uit om de resourcegroep, VM en alle gerelateerde resources te verwijderen:

```azurecli
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt voor het maken van een resourcegroep, een virtuele machine en alle gerelateerde resources. Elke opdracht in de volgende tabel is een koppeling naar opdrachtspecifieke documentatie:

| Opdracht | Opmerkingen |
|---|---|
| [az group create](/cli/azure/group) | Hiermee wordt een resourcegroep gemaakt waarin alle resources worden opgeslagen. |
| [az network vnet create](/cli/azure/network/vnet) | Hiermee maakt u een virtueel Azure-netwerk en -subnet. |
| [az network vnet peering create](/cli/azure/network/vnet/peering) | Hiermee maakt u een peering tussen twee virtuele netwerken.  |
| [az group delete](/cli/azure/vm/extension) | Hiermee verwijdert u een resourcegroep met inbegrip van alle geneste resources. |

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure) voor meer informatie over de Azure CLI.

Meer CLI-voorbeeldscripts voor virtuele netwerken kunt u vinden in [CLI-voorbeelden voor virtuele netwerken](../cli-samples.md).
