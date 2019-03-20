---
title: Apache Hadoop Pig gebruiken met rest-opslag in HDInsight - Azure
description: Informatie over het gebruik van REST Pig Latin uitvoeren op een Apache Hadoop-cluster in Azure HDInsight.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/10/2018
ms.author: hrasheed
ms.openlocfilehash: 52ad40a2521f21efee3b9f98b46c2e2e6343b656
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58098301"
---
# <a name="run-apache-pig-jobs-with-apache-hadoop-on-hdinsight-by-using-rest"></a>Apache Pig-taken uitvoeren met Apache Hadoop op HDInsight met behulp van REST

[!INCLUDE [pig-selector](../../../includes/hdinsight-selector-use-pig.md)]

Leer hoe u Apache Pig Latin-taken uitvoert door REST-aanvragen met een Azure HDInsight-cluster. CURL is gebruikt om te demonstreren hoe u kunt werken met HDInsight met behulp van de WebHCat REST-API.

> [!NOTE]  
> Als u al bekend bent met behulp van Apache Hadoop op basis van Linux-servers, maar u niet bekend bent met HDInsight, raadpleegt u [Linux gebaseerde HDInsight Tips](../hdinsight-hadoop-linux-information.md).

## <a id="prereq"></a>Vereisten

* Een Azure HDInsight (Hadoop op HDInsight)-cluster (op basis van Linux of op basis van Windows)

  > [!IMPORTANT]  
  > Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

* [Curl](https://curl.haxx.se/)

* [jq](https://stedolan.github.io/jq/)

## <a id="curl"></a>Apache Pig-taken uitvoeren met Curl


> [!NOTE]
> De REST-API wordt beveiligd via [eenvoudige verificatie](https://en.wikipedia.org/wiki/Basic_access_authentication). U moet aanvragen altijd uitvoeren via HTTP Secure (HTTPS) om ervoor te zorgen dat uw referenties veilig worden verzonden naar de server.
>
> Wanneer u de opdrachten in deze sectie, Vervang `USERNAME` met de gebruiker om te verifiëren met het cluster en vervang `PASSWORD` met het wachtwoord voor het gebruikersaccount. Vervang `CLUSTERNAME` door de naam van uw cluster.
>


1. Gebruik een opdrachtregel met de volgende opdracht om te controleren of u verbinding met uw HDInsight-cluster kunt maken:

    ```bash
    curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
    ```

    U ontvangt de volgende JSON-antwoord:

        {"status":"ok","version":"v1"}

    In deze opdracht worden de volgende parameters gebruikt:

   * **-u**: De gebruikersnaam en wachtwoord voor het verifiëren van de aanvraag
   * **-G**: Geeft aan dat deze aanvraag een GET-aanvraag is

     Het begin van de URL, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, is hetzelfde voor alle aanvragen. Het pad **status**, geeft aan dat de aanvraag is de status van WebHCat (ook wel bekend als Templeton) moet worden geretourneerd voor de server.

2. Gebruik de volgende code om een taak Pig Latin aan het cluster te verzenden:

    ```bash
    curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="LOGS=LOAD+'/example/data/sample.log';LEVELS=foreach+LOGS+generate+REGEX_EXTRACT($0,'(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)',1)+as+LOGLEVEL;FILTEREDLEVELS=FILTER+LEVELS+by+LOGLEVEL+is+not+null;GROUPEDLEVELS=GROUP+FILTEREDLEVELS+by+LOGLEVEL;FREQUENCIES=foreach+GROUPEDLEVELS+generate+group+as+LOGLEVEL,COUNT(FILTEREDLEVELS.LOGLEVEL)+as+count;RESULT=order+FREQUENCIES+by+COUNT+desc;DUMP+RESULT;" -d statusdir="/example/pigcurl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/pig
    ```

    In deze opdracht worden de volgende parameters gebruikt:

   * **-d**: Omdat `-G` niet wordt gebruikt, de aanvraag standaard ingesteld op de POST-methode. `-d` Hiermee geeft u de waarden die worden verzonden met de aanvraag.

   * **User.name**: De gebruiker die de opdracht wordt uitgevoerd
   * **execute**: De Pig Latin-instructies uit te voeren
   * **statusdir**: De map die de status voor deze taak worden geschreven naar

     > [!NOTE]  
     > U ziet dat de spaties in Pig Latin-instructies zijn vervangen door de `+` gebruikt in combinatie met Curl teken.

     Met deze opdracht moet een taak-ID die kan worden gebruikt om te controleren op de status van de taak, bijvoorbeeld retourneren:

       {"id":"job_1415651640909_0026"}

3. Gebruik de volgende opdracht om te controleren of de status van de taak,

     ```bash
    curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
    ```

     Vervang `JOBID` met de waarde die wordt geretourneerd in de vorige stap. Bijvoorbeeld, als de geretourneerde waarde is `{"id":"job_1415651640909_0026"}`, klikt u vervolgens `JOBID` is `job_1415651640909_0026`.

    Als de taak is voltooid, wordt de status is **geslaagd**.

    > [!NOTE]  
    > Deze aanvraag Curl retourneert een JavaScript Object Notation (JSON)-document met informatie over de taak en jq wordt gebruikt om op te halen, alleen de statuswaarde.

## <a id="results"></a>Resultaten weergeven

Wanneer de status van de taak is gewijzigd in **geslaagd**, kunt u de resultaten van de taak ophalen. De `statusdir` parameter die is doorgegeven met de query bevat de locatie van het uitvoerbestand; in dit geval `/example/pigcurl`.

HDInsight kunt gebruiken ook Azure Storage of Azure Data Lake Storage als het standaardarchief van gegevens. Er zijn verschillende manieren om op te halen op de gegevens die, afhankelijk van wat u gebruikt. Zie voor meer informatie de sectie voor opslag van de [HDInsight op basis van Linux informatie](../hdinsight-hadoop-linux-information.md#hdfs-azure-storage-and-data-lake-storage) document.

## <a id="summary"></a>Samenvatting

Zoals in dit document wordt gedemonstreerd, kunt u een onbewerkte HTTP-aanvraag uitvoeren, controleren en de resultaten van Pig-taken weergeven op uw HDInsight-cluster.

Zie voor meer informatie over de REST-interface gebruikt in dit artikel, de [WebHCat verwijzing](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).

## <a id="nextsteps"></a>Volgende stappen

Voor algemene informatie over Pig in HDInsight:

* [Apache Pig gebruiken met Apache Hadoop op HDInsight](hdinsight-use-pig.md)

Voor meer informatie over andere manieren kunt u werken met Hadoop op HDInsight:

* [Apache Hive gebruiken met Apache Hadoop op HDInsight](hdinsight-use-hive.md)
* [MapReduce gebruiken met Apache Hadoop op HDInsight](hdinsight-use-mapreduce.md)
