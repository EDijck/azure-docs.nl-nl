---
title: Toegang tot Azure Data Lake Storage Gen2 Preview-gegevens met Azure Databricks met behulp van Spark | Microsoft Docs
description: Informatie over het uitvoeren van Spark-query's in een Azure Databricks-cluster voor toegang tot gegevens in een Azure Data Lake Storage Gen2-opslagaccount.
services: storage
author: dineshmurthy
ms.component: data-lake-storage-gen2
ms.service: storage
ms.topic: tutorial
ms.date: 12/06/2018
ms.author: dineshm
ms.openlocfilehash: 88a05eb8fa59740012ca6c7a8d8508d565854dc7
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52974155"
---
# <a name="tutorial-access-azure-data-lake-storage-gen2-preview-data-with-azure-databricks-using-spark"></a>Zelfstudie: Toegang tot Azure Data Lake Storage Gen2 Preview-gegevens met Azure Databricks met behulp van Spark

In deze zelfstudie leert u hoe u Spark-query's kunt uitvoeren op een Azure Databricks-cluster om gegevens te doorzoeken in een Azure-opslagaccount met Azure Data Lake Storage Gen2 Preview.

> [!div class="checklist"]
> * Een Databricks-cluster maken
> * Niet-gestructureerde gegevens opnemen in een opslagaccount
> * Analyse uitvoeren op gegevens in Blob-opslag

## <a name="prerequisites"></a>Vereisten

In deze zelfstudie wordt getoond hoe u de vluchtgegevens kunt gebruiken en doorzoeken die beschikbaar zijn via de [United States Department of Transportation](https://transtats.bts.gov/Tables.asp?DB_ID=120&DB_Name=Airline%20On-Time%20Performance%20Data&DB_Short_Name=On-Time). Download de vluchtgegevens van minstens twee jaar (waarbij u alle velden selecteert) en sla het resultaat op de computer op. Noteer de bestandsnaam en het bestandspad voor de download. U hebt deze informatie later nodig.

> [!NOTE]
> Klik op het selectievakje **Prezipped file** om alle gegevensvelden te selecteren. De download is heel groot (vele gigabytes), maar deze hoeveelheid gegevens is nodig voor de analyse.

## <a name="create-an-azure-storage-account-with-analytic-capabilities"></a>Een Azure-opslagaccount met analysemogelijkheden maken

Maak eerst een nieuw [opslagaccount met analysemogelijkheden](data-lake-storage-quickstart-create-account.md) en geef dit account een unieke naam. Navigeer vervolgens naar het opslagaccount om de configuratie-instellingen op te halen.

1. Klik onder **Instellingen** op **Toegangssleutels**.
2. Klik op de knop **Kopiëren** naast **key1** om de sleutelwaarde te kopiëren.

Zowel de accountnaam als de sleutel hebt u nodig in latere stappen in deze zelfstudie. Open een teksteditor en noteer de accountnaam en sleutel voor later gebruik.

## <a name="create-a-databricks-cluster"></a>Een Databricks-cluster maken

De volgende stap is het maken van een [Databricks-cluster](https://docs.azuredatabricks.net/) om een gegevenswerkruimte te maken.

1. Maak een [Databricks-service](https://ms.portal.azure.com/#create/Microsoft.Databricks) en noem deze **myFlightDataService**. (Schakel het selectievakje *Vastmaken aan dashboard* in bij het maken van de service.)
2. Klik op **Werkruimte starten** om de werkruimte te openen in een nieuw browservenster.
3. Klik in de navigatiebalk links op **Clusters**.
4. Klik op **Cluster maken**.
5. Voer **myFlightDataCluster** in het veld *Clusternaam* in.
6. Selecteer **Standard_D8s_v3** in het veld *Type werkrol*.
7. Wijzig de waarde **Min werkrol** in *4*.
8. Klik boven aan de pagina op **Cluster maken**. (Het duurt maximaal 5 minuten om dit proces te voltooien.)
9. Zodra het proces is voltooid, selecteert u linksboven in de navigatiebalk de optie **Azure Databricks**.
10. Selecteer in de sectie **Nieuw** op de onderste helft van de pagina de optie **Notitieblok**.
11. Voer een naam in van uw keuze in het veld **Naam** en selecteer **Python** als taal.
12. U kunt in alle andere velden de standaardwaarden laten staan.
13. Selecteer **Maken**.
14. Plak de volgende code in de cel **Cmd 1**. Vergeet niet de tijdelijke aanduidingen tussen haken in het voorbeeld te vervangen door uw eigen waarden:

    ```scala
    %python%
    configs = {"fs.azure.account.auth.type": "OAuth",
        "fs.azure.account.oauth.provider.type": "org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider",
        "fs.azure.account.oauth2.client.id": "<service-client-id>",
        "fs.azure.account.oauth2.client.secret": "<service-credentials>",
        "fs.azure.account.oauth2.client.endpoint": "https://login.microsoftonline.com/<tenant-id>/oauth2/token"}
        
    dbutils.fs.mount(
        source = "abfss://dbricks@<account-name>.dfs.core.windows.net/folder1",
        mount_point = "/mnt/flightdata",
        extra_configs = configs)
    ```
15. Druk op **SHIFT+ENTER** om de codecel uit te voeren.

## <a name="ingest-data"></a>Gegevens opnemen

### <a name="copy-source-data-into-the-storage-account"></a>Brongegevens kopiëren naar het opslagaccount

De volgende taak is het gebruiken van AzCopy om gegevens uit het *.csv*-bestand te kopiëren naar Azure Storage. Open een opdrachtpromptvenster en voer de volgende opdrachten in. Zorg ervoor dat u de tijdelijke aanduidingen `<DOWNLOAD_FILE_PATH>` en `<ACCOUNT_KEY>` vervangt door de bijbehorende waarden die u in de vorige stap hebt genoteerd.

```bash
set ACCOUNT_NAME=<ACCOUNT_NAME>
set ACCOUNT_KEY=<ACCOUNT_KEY>
azcopy cp "<DOWNLOAD_FILE_PATH>" https://<ACCOUNT_NAME>.dfs.core.windows.net/dbricks/folder1/On_Time --recursive 
```

### <a name="use-databricks-notebook-to-convert-csv-to-parquet"></a>Databricks Notebook gebruiken om CSV te converteren naar Parquet

Open Databricks opnieuw in de browser en voer de volgende stappen uit:

1. Selecteer linksboven in de navigatiebalk de optie **Azure Databricks**.
2. Selecteer in de sectie **Nieuw** op de onderste helft van de pagina de optie **Notitieblok**.
3. Voer in het veld **Naam** in: **CSV2Parquet**.
4. U kunt in alle andere velden de standaardwaarden laten staan.
5. Selecteer **Maken**.
6. Plak de volgende code in de cel **Cmd 1**. (Deze code wordt automatisch opgeslagen in de editor.)

    ```python
    # Use the previously established DBFS mount point to read the data
    # create a dataframe to read data
    flightDF = spark.read.format('csv').options(header='true', inferschema='true').load("/mnt/flightdata/On_Time_On_Time*.csv")
    # read the all the airline csv files and write the output to parquet format for easy query
    flightDF.write.mode("append").parquet("/mnt/flightdata/parquet/flights")
    print("Done")
    ```

## <a name="explore-data-using-hadoop-distributed-file-system"></a>Gegevens verkennen met behulp van Hadoop Distributed File System

Ga terug naar de Databricks-werkruimte en klik in de linkernavigatiebalk op het pictogram **Recent**.

1. Klik op het notitieblok **Analyse van vluchtgegevens**.
2. Druk op **Ctrl + Alt + N** om een nieuwe cel te maken.

Voer elk van de volgende codeblokken in **Cmd 1** in en druk op **Cmd + Enter** om het Python-script uit te voeren.

Als u een lijst met CSV-bestanden wilt ophalen die zijn geüpload met behulp van AzCopy, voert u het volgende script uit:

```python
import os.path
import IPython
from pyspark.sql import SQLContext
display(dbutils.fs.ls("/mnt/flightdata/temp/"))
```

Als u een nieuw bestand en een nieuwe lijst met bestanden wilt maken in de map *parquet/flights*, voert u dit script uit:

```python
dbutils.fs.put("/mnt/flightdata/temp/1.txt", "Hello, World!", True)
dbutils.fs.ls("/mnt/flightdata/temp/parquet/flights")
```

Met deze codevoorbeelden hebt u de hiërarchische aard van HDFS verkend met behulp van gegevens die zijn opgeslagen in een opslagaccount met Azure Data Lake Storage Gen2.

## <a name="query-the-data"></a>Query’s uitvoeren voor de gegevens

Hierna kunt u beginnen met het doorzoeken van de gegevens die u hebt geüpload in het opslagaccount. Voer elk van de volgende codeblokken in **Cmd 1** in en druk op **Cmd + Enter** om het Python-script uit te voeren.

### <a name="simple-queries"></a>Eenvoudige query’s

Als u dataframes wilt maken voor uw gegevensbronnen, voert u het volgende script uit:

> [!IMPORTANT]
> Vervang de tijdelijke aanduiding **<YOUR_CSV_FILE_NAME>**  door de bestandsnaam die u aan het begin van deze zelfstudie hebt gedownload.

```python
#Copy this into a Cmd cell in your notebook.
acDF = spark.read.format('csv').options(header='true', inferschema='true').load("/mnt/flightdata/<YOUR_CSV_FILE_NAME>.csv")
acDF.write.parquet('/mnt/flightdata/parquet/airlinecodes')

#read the existing parquet file for the flights database that was created earlier
flightDF = spark.read.format('parquet').options(header='true', inferschema='true').load("/mnt/flightdata/parquet/flights")

#print the schema of the dataframes
acDF.printSchema()
flightDF.printSchema()

#print the flight database size
print("Number of flights in the database: ", flightDF.count())

#show the first 20 rows (20 is the default)
#to show the first n rows, run: df.show(n)
acDF.show(100, False)
flightDF.show(20, False)

#Display to run visualizations
#preferably run this in a separate cmd cell
display(flightDF)
```

Als u analysequery’s wilt uitvoeren voor deze gegevens, voert u het volgende script uit:

```python
#Run each of these queries, preferably in a separate cmd cell for separate analysis
#create a temporary sql view for querying flight information
FlightTable = spark.read.parquet('/mnt/flightdata/parquet/flights')
FlightTable.createOrReplaceTempView('FlightTable')

#create a temporary sql view for querying airline code information
AirlineCodes = spark.read.parquet('/mnt/flightdata/parquet/airlinecodes')
AirlineCodes.createOrReplaceTempView('AirlineCodes')

#using spark sql, query the parquet file to return total flights in January and February 2016
out1 = spark.sql("SELECT * FROM FlightTable WHERE Month=1 and Year= 2016")
NumJan2016Flights = out1.count()
out2 = spark.sql("SELECT * FROM FlightTable WHERE Month=2 and Year= 2016")
NumFeb2016Flights=out2.count()
print("Jan 2016: ", NumJan2016Flights," Feb 2016: ",NumFeb2016Flights)
Total= NumJan2016Flights+NumFeb2016Flights
print("Total flights combined: ", Total)

# List out all the airports in Texas
out = spark.sql("SELECT distinct(OriginCityName) FROM FlightTable where OriginStateName = 'Texas'") 
print('Airports in Texas: ', out.show(100))

#find all airlines that fly from Texas
out1 = spark.sql("SELECT distinct(Carrier) FROM FlightTable WHERE OriginStateName='Texas'")
print('Airlines that fly to/from Texas: ', out1.show(100, False))
```
### <a name="complex-queries"></a>Complexe query's

Voer de segmenten één voor één uit in het notitieblok en controleer de resultaten om de volgende complexere query’s uit te voeren.

```python
#find the airline with the most flights

#create a temporary view to hold the flight delay information aggregated by airline, then select the airline name from the Airlinecodes dataframe
spark.sql("DROP VIEW IF EXISTS v")
spark.sql("CREATE TEMPORARY VIEW v AS SELECT Carrier, count(*) as NumFlights from FlightTable group by Carrier, UniqueCarrier order by NumFlights desc LIMIT 10")
output = spark.sql("SELECT AirlineName FROM AirlineCodes WHERE AirlineCode in (select Carrier from v)")

#show the top row without truncation
output.show(1, False)

#show the top 10 airlines
output.show(10, False)

#Determine which is the least on time airline

#create a temporary view to hold the flight delay information aggregated by airline, then select the airline name from the Airlinecodes dataframe
spark.sql("DROP VIEW IF EXISTS v")
spark.sql("CREATE TEMPORARY VIEW v AS SELECT Carrier, count(*) as NumFlights from FlightTable WHERE DepDelay>60 or ArrDelay>60 group by Carrier, UniqueCarrier order by NumFlights desc LIMIT 10")
output = spark.sql("select * from v")
#output = spark.sql("SELECT AirlineName FROM AirlineCodes WHERE AirlineCode in (select Carrier from v)")
#show the top row without truncation
output.show(1, False)

#which airline improved its performance
#find the airline with the most improvement in delays
#create a temporary view to hold the flight delay information aggregated by airline, then select the airline name from the Airlinecodes dataframe
spark.sql("DROP VIEW IF EXISTS v1")
spark.sql("DROP VIEW IF EXISTS v2")
spark.sql("CREATE TEMPORARY VIEW v1 AS SELECT Carrier, count(*) as NumFlights from FlightTable WHERE (DepDelay>0 or ArrDelay>0) and Year=2016 group by Carrier order by NumFlights desc LIMIT 10")
spark.sql("CREATE TEMPORARY VIEW v2 AS SELECT Carrier, count(*) as NumFlights from FlightTable WHERE (DepDelay>0 or ArrDelay>0) and Year=2017 group by Carrier order by NumFlights desc LIMIT 10")
output = spark.sql("SELECT distinct ac.AirlineName, v1.Carrier, v1.NumFlights, v2.NumFlights from v1 INNER JOIN v2 ON v1.Carrier = v2.Carrier INNER JOIN AirlineCodes ac ON v2.Carrier = ac.AirlineCode WHERE v1.NumFlights > v2.NumFlights")
#show the top row without truncation
output.show(10, False)

#display for visual analysis
display(output)
```

## <a name="next-steps"></a>Volgende stappen

* [Gegevens extraheren, transformeren en laden met Apache Hive in Azure HDInsight](data-lake-storage-tutorial-extract-transform-load-hive.md)
