---
title: bestand opnemen
description: bestand opnemen
services: container-registry
author: rockboyfor
ms.service: container-registry
ms.topic: include
origin.date: 01/23/2019
ms.date: 02/18/2019
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: cd97c61e7493249785293ae331713ba1a98efee3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60787380"
---
<!--Verify successfully-->
## <a name="run-image-from-registry"></a>Installatiekopie vanuit register uitvoeren

U kunt nu de `hello-world:v1`-containerinstallatiekopie uit het containerregister ophalen en deze uitvoeren met behulp van [docker run][docker-run]:

```
docker run <acrLoginServer>/hello-world:v1  
```

Voorbeelduitvoer: 

```
Unable to find image 'mycontainerregistry007.azurecr.cn/hello-world:v1' locally
v1: Pulling from hello-world
Digest: sha256:662dd8e65ef7ccf13f417962c2f77567d3b132f12c95909de6c85ac3c326a345
Status: Downloaded newer image for mycontainerregistry007.azurecr.cn/hello-world:v1

Hello from Docker!
This message shows that your installation appears to be working correctly.

[...]
```

<!-- LINKS - External -->
[docker-run]: https://docs.docker.com/engine/reference/commandline/run/

<!--Update_Description: new articles on container registry quickstart docker pull -->
<!--ms.date: 02/18/2019-->