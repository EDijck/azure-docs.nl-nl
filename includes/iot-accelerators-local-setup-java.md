---
title: bestand opnemen
description: bestand opnemen
services: iot-accelerators
author: v-krghan
ms.service: iot-accelerators
ms.topic: include
ms.date: 01/25/2019
ms.author: v-krghan
ms.custom: include file
ms.openlocfilehash: 5909533c2153fe7c6697f516ce7568787b82bdab
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/28/2019
ms.locfileid: "55105986"
---
## <a name="download-the-source-code"></a>De broncode downloaden

De bewaking op afstand broncodeopslagplaatsen bevatten de broncode en de Docker-configuratiebestanden die u nodig hebt voor het uitvoeren van de microservices Docker-installatiekopieën.

Klonen en maken van een lokale versie van de opslagplaats, uw opdrachtregel omgeving te gebruiken om te navigeren naar een geschikte map op uw lokale computer. Voer vervolgens een van de volgende sets opdrachten om de java-opslagplaats te klonen:

Voor het downloaden van de meest recente versie van de java-microservices-implementaties, voert u de volgende uit:


```cmd/sh
git clone --recurse-submodules https://github.com/Azure/azure-iot-pcs-remote-monitoring-java.git

# To retrieve the latest submodules, run the following command:

cd azure-iot-pcs-remote-monitoring-java
git submodule foreach git pull origin master
```

> [!NOTE]
> Deze opdrachten download de broncode voor de microservices naast de scripts die u kunt de microservices lokaal uitvoeren. Hoewel u niet de broncode voor het uitvoeren van de microservices in Docker nodig hebt, is de broncode is handig als u later wilt wijzigen van de oplossingsversnellers en uw wijzigingen lokaal testen.

## <a name="deploy-the-azure-services"></a>De Azure-services implementeren

Hoewel dit artikel u hoe u de microservices lokaal uitvoert leest, worden ze vertrouwen op Azure-services die worden uitgevoerd in de cloud. Het volgende script gebruiken voor het implementeren van de Azure-services. Het volgende scriptvoorbeelden wordt ervan uitgegaan dat u de java-opslagplaats op een Windows-machine. Als u in een andere omgeving werkt, aanpassen op de juiste wijze de paden, bestandsextensies en padscheidingstekens.

### <a name="create-new-azure-resources"></a>Nieuwe Azure-resources maken

Als u de vereiste Azure-resources is nog niet hebt gemaakt, volgt u deze stappen:

1. In de opdrachtregel-omgeving, gaat u naar de **\services\scripts\local\launch** map in de gekloonde kopie van de opslagplaats.

1. Voer de volgende opdrachten voor het installeren van de **pc's** CLI-hulpprogramma en meld u aan bij uw Azure-account:

    ```cmd
    npm install -g iot-solutions
    pcs login
    ```

1. Voer de **start.cmd** script. Het script vraagt u om de volgende informatie:
    * De naam van een oplossing.
    * Het te gebruiken Azure-abonnement.
    * De locatie van het Azure-datacenter te gebruiken.

    Het script wordt de resourcegroep gemaakt in Azure met de naam van uw oplossing. Deze resourcegroep bevat de Azure-resources die maakt gebruik van de solution accelerator. Nadat u de bijbehorende resources niet meer nodig hebt, kunt u deze resourcegroep verwijderen.

    Het script wordt ook een set van omgevingsvariabelen met een voorvoegsel toegevoegd **pc's** naar uw lokale computer. Wanneer u de Docker-containers of lokaal microservice-projecten start, lezen ze hun configuratiewaarden van deze omgevingsvariabelen.

    > [!TIP]
    > Wanneer het script is voltooid, ook wordt opgeslagen in de omgevingsvariabelen van een bestand met de naam  **\<uw basismap\>\\.pcs\\\<oplossingsnaam\>.env** . U kunt deze gebruiken voor toekomstige solution accelerator-implementaties. Houd er rekening mee dat omgevingsvariabelen instellen op uw lokale computer, overschrijven de waarden in de **services\\scripts\\lokale\\.env** tijdens het uitvoeren van het bestand **docker-compose**.

1. Afsluiten van uw opdrachtregelomgeving.

### <a name="use-existing-azure-resources"></a>Bestaande Azure-bronnen

Als u de vereiste Azure-resources al hebt gemaakt, maakt u de bijbehorende omgevingsvariabelen op uw lokale computer. Deze kunnen worden opgeslagen de  **\<uw basismap\>\\.pcs\\\<oplossingsnaam\>.env** -bestand van de implementatie. Houd er rekening mee dat omgevingsvariabelen instellen op uw lokale computer, overschrijven de waarden in de **services\\scripts\\lokale\\.env** tijdens het uitvoeren van het bestand **docker-compose**.