---
title: Aan de slag met een HBase-voorbeeld in HDInsight - Azure
description: Voer dit voorbeeld voor Apache HBase uit om met Hadoop aan de slag te gaan in HDInsight. Maak tabellen vanuit de HBase-shell en gebruik Hive om query's uit te voeren op de tabellen.
keywords: hbase-opdracht, hbase-voorbeeld
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 02/22/2018
ms.author: hrasheed
ms.openlocfilehash: c1c582c60a7c91de40983daa07bdec1e8d748f8a
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "62123124"
---
# <a name="get-started-with-an-apache-hbase-example-in-hdinsight"></a>Aan de slag met een voorbeeld van Apache HBase in HDInsight

Meer informatie over het maken van een [Apache HBase](https://hbase.apache.org/) -cluster in HDInsight, HBase-tabellen maken en query uitvoeren op tabellen met behulp van [Apache Hive](https://hive.apache.org/).  Zie [Overzicht van HDInsight HBase][hdinsight-hbase-overview] voor algemene informatie over HBase.

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="prerequisites"></a>Vereisten
Voordat u begint met dit HBase-voorbeeld, moet u beschikken over de volgende items:

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* [Secure Shell (SSH)](../hdinsight-hadoop-linux-use-ssh-unix.md). 
* [curl](https://curl.haxx.se/download.html).

## <a name="create-apache-hbase-cluster"></a>Apache HBase-cluster maken
In de volgende procedure wordt een Azure Resource Manager-sjabloon gebruikt om een HBase-cluster en het afhankelijke standaard Azure Storage-opslagaccount te maken. Zie [Op Linux gebaseerde Hadoop-clusters maken in HDInsight](../hdinsight-hadoop-provision-linux-clusters.md) voor meer inzicht in de parameters die voor deze procedure worden gebruikt en andere methoden voor het maken van clusters. Zie voor informatie over het gebruik van Data Lake Storage Gen2 [Snelstart: clusters instellen in HDInsight](../../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md).

1. Klik op de volgende afbeelding om de sjabloon in Azure Portal te openen. De sjabloon bevindt zich in [Azure-snelstartsjablonen](https://azure.microsoft.com/resources/templates/).
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-linux%2Fazuredeploy.json" target="_blank"><img src="./media/apache-hbase-tutorial-get-started-linux/deploy-to-azure.png" alt="Deploy to Azure"></a>
2. Voer op de blade **Aangepaste implementatie** de volgende waarden in:
   
   * **Abonnement**: Selecteer uw Azure-abonnement dat wordt gebruikt om het cluster te maken.
   * **Resourcegroep**: Maken van een Azure-resourcebeheergroep of gebruik een bestaande resourcegroep.
   * **Locatie**: Geef de locatie van de resourcegroep. 
   * **Clusternaam**: Voer een naam voor het HBase-cluster.
   * **Cluster-aanmeldingsnaam en dit wachtwoord**: De standaardaanmeldingsnaam is **admin**.
   * **SSH-gebruikersnaam en wachtwoord**: De standaardgebruikersnaam is **sshuser**.  U kunt de naam wijzigen.
     
     Andere parameters zijn optioneel.  
     
     Elk cluster is afhankelijk van een Azure Storage-account. Nadat u een cluster hebt verwijderd, blijven de gegevens in het opslagaccount staan. De naam van het standaardopslagaccount voor het cluster is de naam waaraan 'store' is toegevoegd. Deze is vastgelegd in de sectie met sjabloonvariabelen.
3. Selecteer **Ik ga akkoord met de bovenstaande voorwaarden** en klik vervolgens op **Kopen**. Het duurt ongeveer 20 minuten om een cluster te maken.

> [!NOTE]  
> Nadat een HBase-cluster is verwijderd, kunt u een ander HBase-cluster maken met de dezelfde standaard blob-container. Het nieuwe cluster haalt de HBase-tabellen op die u hebt gemaakt in het oorspronkelijke cluster. Om inconsistenties te voorkomen, wordt u aangeraden de HBase-tabellen uit te schakelen voordat u het cluster verwijdert.
> 
> 

## <a name="create-tables-and-insert-data"></a>Tabellen maken en gegevens invoegen
U kunt SSH gebruiken om verbinding te maken met HBase-clusters en vervolgens [Apache HBase Shell](https://hbase.apache.org/0.94/book/shell.html) gebruiken om HBase-tabellen te maken, gegevens in te voegen, en gegevens te doorzoeken. Zie [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

Voor de meeste mensen worden de gegevens weergegeven in een tabelindeling:

![Tabelgegevens in HDInsight HBase][img-hbase-sample-data-tabular]

In HBase (een implementatie van [Cloud BigTable](https://cloud.google.com/bigtable/)), dezelfde gegevens ziet eruit als:

![BigTable-gegevens in HDInsight HBase][img-hbase-sample-data-bigtable]


**De HBase-shell gebruiken**

1. Voer de volgende HBase-opdracht uit in SSH:
   
    ```bash
    hbase shell
    ```

2. Maak een HBase met twee kolomfamilies:

    ```hbaseshell   
    create 'Contacts', 'Personal', 'Office'
    list
    ```
3. Voeg enkele gegeven in:
    
    ```hbaseshell   
    put 'Contacts', '1000', 'Personal:Name', 'John Dole'
    put 'Contacts', '1000', 'Personal:Phone', '1-425-000-0001'
    put 'Contacts', '1000', 'Office:Phone', '1-425-000-0002'
    put 'Contacts', '1000', 'Office:Address', '1111 San Gabriel Dr.'
    scan 'Contacts'
    ```
   
    ![HDInsight Hadoop HBase-shell][img-hbase-shell]
4. Een enkel rij ophalen
   
    ```hbaseshell
    get 'Contacts', '1000'
    ```
   
    Aangezien er maar één rij is, zijn de resultaten hetzelfde als voor de scanopdracht.
   
    Zie voor meer informatie over het HBase-tabelschema [Inleiding tot Apache HBase-schemaontwerp][hbase-schema]. Raadpleeg de [Snelzoekgids voor Apache HBase][hbase-quick-start] voor meer HBase-opdrachten.
5. De shell afsluiten
   
    ```hbaseshell
    exit
    ```

**Gegevens bulksgewijs laden in de HBase-tabel met contacten**

U kunt in HBase verschillende methoden gebruiken om gegevens in tabellen te laden.  Zie [Bulk loading](https://hbase.apache.org/book.html#arch.bulk.load) (Bulkgsgewijs laden) voor meer informatie.

Een bestand met voorbeeldgegevens vindt u in een openbare blob-container *wasb://hbasecontacts\@hditutorialdata.blob.core.windows.net/contacts.txt*.  De inhoud van het gegevensbestand is:

    8396    Calvin Raji      230-555-0191    230-555-0191    5415 San Gabriel Dr.
    16600   Karen Wu         646-555-0113    230-555-0192    9265 La Paz
    4324    Karl Xie         508-555-0163    230-555-0193    4912 La Vuelta
    16891   Jonn Jackson     674-555-0110    230-555-0194    40 Ellis St.
    3273    Miguel Miller    397-555-0155    230-555-0195    6696 Anchor Drive
    3588    Osa Agbonile     592-555-0152    230-555-0196    1873 Lion Circle
    10272   Julia Lee        870-555-0110    230-555-0197    3148 Rose Street
    4868    Jose Hayes       599-555-0171    230-555-0198    793 Crawford Street
    4761    Caleb Alexander  670-555-0141    230-555-0199    4775 Kentucky Dr.
    16443   Terry Chander    998-555-0171    230-555-0200    771 Northridge Drive

U kunt een tekstbestand maken en het bestand desgewenst uploaden naar uw eigen opslagaccount. Zie voor instructies [gegevens uploaden voor Apache Hadoop-taken in HDInsight][hdinsight-upload-data].

> [!NOTE]  
> In deze procedure wordt gebruikgemaakt van de HBase-tabel met contacten die u in de laatste procedure hebt gemaakt.

1. Voer de volgende opdracht vanuit SSH uit om het gegevensbestand te transformeren naar StoreFiles en op te slaan naar een relatief pad dat is opgegeven door Dimporttsv.bulk.output.  Als u zich in HBase Shell bevindt, gebruikt u de afsluitopdracht om af te sluiten.

    ```bash   
    hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name,Personal:Phone,Office:Phone,Office:Address" -Dimporttsv.bulk.output="/example/data/storeDataFileOutput" Contacts wasb://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt
    ```

2. Voer de volgende opdracht uit om de gegevens uit /example/data/storeDataFileOutput naar de HBase-tabel te uploaden:
   
    ```bash
    hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /example/data/storeDataFileOutput Contacts
    ```

3. U kunt de HBase-shell openen en de scanopdracht gebruiken om de tabelinhoud weer te geven.

## <a name="use-apache-hive-to-query-apache-hbase"></a>Apache Hive gebruiken met Apache HBase query

U kunt gegevens in HBase-tabellen opvragen met behulp van [Apache Hive](https://hive.apache.org/). In dit gedeelte maakt u een Hive-tabel die is toegewezen aan de HBase-tabel en deze gebruikt om een query voor de gegevens in uw HBase-tabel uit te voeren.

1. Open **PuTTY** en maak verbinding met het cluster.  Zie de instructies in de vorige procedure.
2. Gebruik tijdens de SSH-sessie de volgende opdracht om Beeline te starten:

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin
    ```

    Zie voor meer informatie over Beeline [Hive gebruiken met Hadoop in HDInsight met Beeline](../hadoop/apache-hadoop-use-hive-beeline.md).
       
3. Voer de volgende [HiveQL](https://cwiki.apache.org/confluence/display/Hive/LanguageManual) script voor het maken van een Hive-tabel die is toegewezen aan de HBase-tabel. Zorg ervoor dat u met de HBase-shell de voorbeeldtabel hebt gemaakt waarnaar eerder in deze zelfstudie is verwezen voordat u deze instructie uitvoert.

    ```hiveql   
    CREATE EXTERNAL TABLE hbasecontacts(rowkey STRING, name STRING, homephone STRING, officephone STRING, officeaddress STRING)
    STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
    WITH SERDEPROPERTIES ('hbase.columns.mapping' = ':key,Personal:Name,Personal:Phone,Office:Phone,Office:Address')
    TBLPROPERTIES ('hbase.table.name' = 'Contacts');
    ```

4. Voer het volgende HiveQL-script uit om een query uit te voeren voor de gegevens in de HBase-tabel:

    ```hiveql   
    SELECT count(rowkey) FROM hbasecontacts;
    ```

## <a name="use-hbase-rest-apis-using-curl"></a>HBase REST API's gebruiken met Curl

De REST API is beveiligd via [basisverificatie](https://en.wikipedia.org/wiki/Basic_access_authentication). U moet aanvragen altijd uitvoeren via een beveiligde HTTP-verbinding (HTTPS). Zo zorgt u ervoor dat uw referenties veilig worden verzonden naar de server.

1. Gebruik de volgende opdracht om een lijst met bestaande HBase-tabellen weer te geven:

    ```bash
    curl -u <UserName>:<Password> \
    -G https://<ClusterName>.azurehdinsight.net/hbaserest/
    ```

1. Gebruik de volgende opdracht om een nieuwe HBase-tabel met twee kolomfamilies te maken:

    ```bash   
    curl -u <UserName>:<Password> \
    -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/schema" \
    -H "Accept: application/json" \
    -H "Content-Type: application/json" \
    -d "{\"@name\":\"Contact1\",\"ColumnSchema\":[{\"name\":\"Personal\"},{\"name\":\"Office\"}]}" \
    -v
    ```

    Het schema wordt opgegeven in de JSON-indeling.
1. Gebruik de volgende opdracht om enkele gegevens in te voegen:

    ```bash   
    curl -u <UserName>:<Password> \
    -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/false-row-key" \
    -H "Accept: application/json" \
    -H "Content-Type: application/json" \
    -d "{\"Row\":[{\"key\":\"MTAwMA==\",\"Cell\": [{\"column\":\"UGVyc29uYWw6TmFtZQ==\", \"$\":\"Sm9obiBEb2xl\"}]}]}" \
    -v
    ```
   
    U moet de waarden die in de schakeloptie -d zijn opgegeven, met Base64 coderen. In het voorbeeld:
   
   * MTAwMA==: 1000
   * UGVyc29uYWw6TmFtZQ ==: Persoonlijk: naam
   * Sm9obiBEb2xl: Joep Davids
     
     [false-row-key](https://hbase.apache.org/apidocs/org/apache/hadoop/hbase/rest/package-summary.html#operation_cell_store_single) maakt het mogelijk om meerdere waarden (in batch) in te voegen.
1. Gebruik de volgende opdracht om een rij te verkrijgen:
   
    ```bash 
    curl -u <UserName>:<Password> \
    -X GET "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/1000" \
    -H "Accept: application/json" \
    -v
    ```

Zie [Apache HBase Reference Guide](https://hbase.apache.org/book.html#_rest) (Snelzoekgids voor Apache HBase) voor meer informatie over HBase REST.

> [!NOTE]  
> Thrift wordt niet ondersteund door HBase in HDInsight.
>
> Wanneer u Curl of een andere REST-communicatie gebruikt met WebHCat, moet u de aanvragen verifiëren door de gebruikersnaam en het wachtwoord voor de beheerder van het HDInsight-cluster op te geven. U moet ook de clusternaam gebruiken als onderdeel van de URI (Uniform Resource Identifier) die wordt gebruikt om de aanvragen naar de server te verzenden:
> 
>   
>        curl -u <UserName>:<Password> \
>        -G https://<ClusterName>.azurehdinsight.net/templeton/v1/status
>   
>    Het antwoord dat u ontvangt, is vergelijkbaar met het volgende antwoord:
>   
>        {"status":"ok","version":"v1"}
   


## <a name="check-cluster-status"></a>De clusterstatus controleren
HBase in HDInsight wordt geleverd met een webgebruikersinterface voor het bewaken van clusters. Met de webgebruikersinterface kunt u statistieken of informatie over regio's aanvragen.

**De HBase-hoofdinterface openen**

1. Meld u aan bij de Ambari-Webinterface op https://&lt;Clustername >. azurehdinsight.net.
2. Klik in het linkermenu op **HBase**.
3. Klik op **Quick links** boven aan de pagina, wijs de actieve Zookeeper-knooppuntkoppeling aan en klik vervolgens op **HBase Master UI**.  De interface wordt in een nieuw browsertabblad geopend:

   ![HDInsight HBase HMaster-interface](./media/apache-hbase-tutorial-get-started-linux/hdinsight-hbase-hmaster-ui.png)

   De HBase-hoofdinterface bevat de volgende onderdelen:

   - regioservers
   - back-upmasters
   - tabellen
   - taken
   - softwarekenmerken

## <a name="delete-the-cluster"></a>Het cluster verwijderen
Om inconsistenties te voorkomen, wordt u aangeraden de HBase-tabellen uit te schakelen voordat u het cluster verwijdert.

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a>Problemen oplossen

Zie [Vereisten voor toegangsbeheer](../hdinsight-hadoop-create-linux-clusters-portal.md) als u problemen ondervindt met het maken van HDInsight-clusters.

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe u een Apache HBase-cluster maakt en hoe u tabellen maken en de gegevens in deze tabellen vanuit de HBase-shell weergeeft. U hebt ook geleerd hoe u een Hive-query op gegevens in HBase-tabellen uitvoert en hoe u de HBase C# REST API's gebruikt om een HBase-tabel te maken en gegevens op te halen uit de tabel.

Voor meer informatie zie:

* [Overzicht van HDInsight HBase][hdinsight-hbase-overview]: Apache HBase is een Apache, open-source NoSQL-database gebouwd op Apache Hadoop. deze willekeurige toegang en sterke consistentie voor grote hoeveelheden ongestructureerde en semigestructureerde gegevens biedt.

[hdinsight-upload-data]: ../hdinsight-upload-data.md
[hbase-reference]: https://hbase.apache.org/book.html#importtsv
[hbase-schema]: http://0b4af6cdc2f0c5998459-c0245c5c937c5dedcca3f1764ecc9b2f.r43.cf2.rackcdn.com/9353-login1210_khurana.pdf
[hbase-quick-start]: https://hbase.apache.org/book.html#quickstart





[hdinsight-hbase-overview]:apache-hbase-overview.md
[hdinsight-hbase-provision-vnet]:apache-hbase-provision-vnet.md
[hdinsight-versions]: hdinsight-component-versioning.md
[azure-purchase-options]: https://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: https://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[azure-portal]: https://portal.azure.com/
[azure-create-storageaccount]: https://azure.microsoft.com/documentation/articles/storage-create-storage-account/

[img-hbase-shell]: ./media/apache-hbase-tutorial-get-started-linux/hdinsight-hbase-shell.png
[img-hbase-sample-data-tabular]: ./media/apache-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-tabular.png
[img-hbase-sample-data-bigtable]: ./media/apache-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-bigtable.png
