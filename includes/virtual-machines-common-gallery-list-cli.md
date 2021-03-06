---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 09/20/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 1e78109472668c0f9a73af6430253a0d709979af
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65149679"
---
## <a name="using-rbac-to-share-images"></a>Met het RBAC voor het delen van afbeeldingen

U kunt installatiekopieën delen tussen abonnementen met behulp van toegangsbeheer op basis van rollen (RBAC). Elke gebruiker die heeft voor de versie van een installatiekopie, zelfs tussen abonnementen leesmachtigingen, zich op een virtuele Machine met behulp van de versie van de installatiekopie implementeren.

Zie voor meer informatie over het delen van resources met behulp van RBAC [beheren van toegang via RBAC en Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli).


## <a name="list-information"></a>Gegevens weergeven

De locatie, de status en andere informatie over de beschikbare afbeeldingsgalerieën met behulp van [az sig lijst](/cli/azure/sig#az-sig-list).

```azurecli-interactive 
az sig list -o table
```

Lijst met de definities van de installatiekopie in een galerie, inclusief informatie over het type besturingssysteem en de status, met behulp van [lijst met de definitie van de installatiekopie van de az sig](/cli/azure/sig/image-definition#az-sig-image-definition-list).

```azurecli-interactive 
az sig image-definition list -g myGalleryRG -r myGallery -o table
```

De versies van de gedeelde installatiekopie in een galerie weergeven met behulp van [lijst met de versie van de installatiekopie van de az sig](/cli/azure/sig/image-version#az-sig-image-version-list).

```azurecli-interactive
az sig image-version list -g myGalleryRG -r myGallery -i myImageDefinition -o table
```

Haal de ID van een installatiekopie versie via [az sig-versie van de installatiekopie show](/cli/azure/sig/image-version#az-sig-image-version-show).

```azurecli-interactive
az sig image-version show \
   -g myGalleryRG \
   -r myGallery \
   -i myImageDefinition \
   --gallery-image-version 1.0.0 \
   --query "id"
```