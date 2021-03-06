---
author: diberry
ms.author: v-junlch
ms.service: cognitive-services
ms.topic: include
origin.date: 03/25/2019
ms.date: 04/23/2019
ms.openlocfilehash: 94e95864d8bac2d6dc0ff690a2a8f53bd2db5a40
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60598892"
---
## <a name="validate-container-is-running"></a>Valideren container wordt uitgevoerd 

Er zijn verschillende manieren voor het valideren van de container wordt uitgevoerd: 

|Aanvraag|Doel|
|--|--|
|`http://localhost:5000/`|De container biedt een startpagina.|
|`http://localhost:5000/status`|Aangevraagd met GET, wordt voor het valideren van de container uitgevoerd zonder dat een eindpunt-query. Dit kan worden gebruikt voor Kubernetes [liveness en gereedheid voor tests](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/).|
|`http://localhost:5000/swagger`|De container biedt een volledige set met documentatie voor de eindpunten, evenals een `Try it now` functie. Deze functie kunt u uw instellingen invoeren in een web gebaseerde HTML-formulier en de query zonder code te schrijven. Nadat de query retourneert, een voorbeeld van de CURL-opdracht om te laten zien hoe de HTTP-headers en hoofdtekst van de vereiste indeling is opgegeven. |

![Startpagina van de container](./media/cognitive-services-containers-api-documentation/container-webpage.png)

