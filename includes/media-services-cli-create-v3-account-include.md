---
title: bestand opnemen
description: bestand opnemen
services: media-services
author: Juliako
ms.service: media-services
ms.topic: include
ms.date: 11/11/2018
ms.author: juliako
ms.custom: include file
ms.openlocfilehash: 4207031652db7ec4b2ae5468dc159320f6efdbc2
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55228811"
---
## <a name="create-a-media-services-account"></a>Een Media Services-account kunt maken

U moet u eerst een Media Services-account maken. Deze sectie wordt beschreven wat u nodig hebt voor het account te maken met de Azure CLI.

### <a name="create-a-resource-group"></a>Een resourcegroep maken

Gebruik de volgende opdracht om een resourcegroep te maken. Een Azure-resourcegroep is een logische container waarin resources, zoals Azure Media Services-accounts en bijbehorende Storage-accounts worden geïmplementeerd en beheerd.

```azurecli
az group create --name amsResourceGroup --location westus2
```

### <a name="create-a-storage-account"></a>Create a storage account

Als u een Media Services-account gaat maken, moet u de naam van een Azure Storage-accountresource opgeven. De opgegeven opslagaccount wordt gekoppeld aan uw Media Services-account. 

U kunt maar één **primaire** opslagaccount koppelen aan uw Media Services-account, maar een onbeperkt aantal **secundaire** opslagaccounts. Media Services ondersteunt **GPv2**-accounts (General-purpose v2) of **GPv1**-accounts (General-purpose v1). Blob-accounts kunt u niet instellen als **primaire** account. Zie [Opties voor Azure Storage-account](../articles/storage/common/storage-account-options.md) voor meer informatie over opslagaccounts. 

Zie [Storage-accounts](../articles/media-services/latest/storage-account-concept.md) voor meer informatie over het gebruik van Storage-accounts in Media Services.

Met de volgende opdracht maakt u een Storage-account die wordt gekoppeld aan de Media Services-account. In het onderstaande script kunt u `storageaccountforams` door uw waarde vervangen. De accountnaam moet uit minder dan 24 tekens bestaan.

```azurecli
az storage account create --name storageaccountforams \  
--kind StorageV2 \
--sku Standard_RAGRS \
--resource-group amsResourceGroup
```

### <a name="create-a-media-services-account"></a>Een Media Services-account kunt maken

Met de volgende Azure CLI-opdracht wordt een nieuwe Media Services-account gemaakt. U kunt de volgende waarden vervangen: `amsaccount`  `storageaccountforams` (moet overeenkomen met de waarde die u voor uw opslagaccount hebt opgegeven), en `amsResourceGroup` (moet overeenkomen met de waarde die u hebt opgegeven voor de resourcegroep).

```azurecli
az ams account create --name amsaccount --resource-group amsResourceGroup --storage-account storageaccountforams
```
