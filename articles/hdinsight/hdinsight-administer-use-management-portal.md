---
title: Windows-gebaseerde Apache Hadoop-clusters in HDInsight met behulp van de Azure-portal beheren
description: Informatie over het beheer van HDInsight-Service. Een HDInsight-cluster maken, opent u de interactieve JavaScript-console en open de console van de opdracht Apache Hadoop.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/25/2017
ms.author: hrasheed
ROBOTS: NOINDEX
ms.openlocfilehash: 4210528003cdb1f584bec3dea80c1aa1db2f86df
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53632013"
---
# <a name="manage-windows-based-apache-hadoop-clusters-in-hdinsight-by-using-the-azure-portal"></a>Windows-gebaseerde Apache Hadoop-clusters in HDInsight beheren met behulp van de Azure-portal

Met behulp van de [Azure-portal][azure-portal], u kunt maken op basis van Windows [Apache Hadoop](https://hadoop.apache.org/) -clusters in Azure HDInsight en Hadoop-gebruiker-wachtwoord wijzigen (Remote Desktop Protocol) inschakelen RDP) zodat u toegang krijgt tot de console van de Hadoop-opdracht op het cluster.

De informatie in dit artikel is alleen van toepassing op op basis van een venster HDInsight-clusters. Zie voor meer informatie over het beheren van clusters op basis van Linux [beheren Apache Hadoop-clusters in HDInsight met behulp van de Azure-portal](hdinsight-administer-use-portal-linux.md).

[!INCLUDE [windows-retirement-notice](../../includes/windows-retirement-notice.md)]

## <a name="prerequisites"></a>Vereisten

Voordat u dit artikel gaat lezen, moet u beschikken over het volgende:

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Azure Storage-account** -een HDInsight-cluster maakt gebruik van een Azure Blob storage-container als het standaardbestandssysteem. Zie voor meer informatie over hoe Azure Blob-opslag een naadloze ervaring met HDInsight-clusters biedt, [Azure Blob Storage gebruiken met HDInsight](hdinsight-hadoop-use-blob-storage.md). Zie voor meer informatie over het maken van een Azure Storage-account [over het maken van een Storage-Account](../storage/common/storage-create-storage-account.md).

## <a name="open-the-portal"></a>De Portal openen
1. Meld u aan bij [https://portal.azure.com](https://portal.azure.com).
2. Nadat u de portal opent, kunt u het volgende doen:

   * Klik op **een resource maken** in het menu links om een nieuwe cluster te maken:

       ![knop Nieuwe HDInsight-cluster](./media/hdinsight-administer-use-management-portal/azure-portal-new-button.png)
   * Klik op **HDInsight-Clusters** in het menu links.

       ![Azure HDInsight-cluster de knop portal](./media/hdinsight-administer-use-management-portal/azure-portal-hdinsight-button.png)

     Als **HDInsight** niet wordt weergegeven in het menu links, klikt u op **Bladeren**.

     ![Azure portal knop in het cluster bladeren](./media/hdinsight-administer-use-management-portal/azure-portal-browse-button.png)

## <a name="create-clusters"></a>Clusters maken
Zie voor instructies voor het maken met behulp van de Portal, [maken van HDInsight-clusters](hdinsight-hadoop-provision-linux-clusters.md).

HDInsight werkt met een breed bereik van Apache Hadoop-onderdelen. Zie voor een lijst van de onderdelen die zijn geverifieerd en wordt ondersteund, [welke versie van Apache Hadoop in Azure HDInsight is](hdinsight-component-versioning.md). U kunt HDInsight aanpassen met behulp van een van de volgende opties:

* Gebruik scriptacties om uit te voeren van aangepaste scripts die een cluster te wijzigen van de configuratie van het cluster of aangepaste onderdelen, zoals Giraph of Solr installeren kunnen aanpassen. Zie voor meer informatie, [aanpassen HDInsight-cluster met Script Action](hdinsight-hadoop-customize-cluster.md).
* Gebruik de parameters voor het aanpassen van cluster in de HDInsight .NET SDK of Azure PowerShell tijdens het maken van clusters. Deze wijzigingen in de configuratie vervolgens via de levensduur van het cluster worden bewaard en worden niet beïnvloed door de cluster-knooppunt reimages die Azure-platform periodiek onderhoud wordt uitgevoerd. Zie voor meer informatie over het gebruik van de parameters van de aanpassing cluster [maken van HDInsight-clusters](hdinsight-hadoop-provision-linux-clusters.md).
* Sommige systeemeigen Java-onderdelen, zoals [Apache Mahout](https://mahout.apache.org/) en [trapsgewijze](https://www.cascading.org/), kunnen worden uitgevoerd op het cluster als JAR-bestanden. Deze JAR-bestanden kunnen worden gedistribueerd naar Azure Blob storage en worden verzonden naar HDInsight-clusters met behulp van Hadoop-taak indienen mechanismen. Zie voor meer informatie, [indienen Apache Hadoop-taken via een programma](hadoop/submit-apache-hadoop-jobs-programmatically.md).

  > [!NOTE]  
  > Als u problemen ondervindt het JAR-bestanden implementeren met HDInsight-clusters of aanroepen van de JAR-bestanden op HDInsight-clusters, contact op met [Microsoft Support](https://azure.microsoft.com/support/options/).
  >
  > Trapsgewijze wordt niet ondersteund door HDInsight en is niet in aanmerking komen voor Microsoft Support. Zie voor lijsten met ondersteunde onderdelen, [wat is er nieuw in de clusterversies geleverd door HDInsight](hdinsight-component-versioning.md).

De installatie van aangepaste software op het cluster met behulp van de verbinding met extern bureaublad wordt niet ondersteund. U dient te vermijden voor het opslaan van alle bestanden op de schijven van het hoofdknooppunt als ze verloren gaan als u nodig hebt om opnieuw de clusters te maken. Het is raadzaam bestanden opslaan op Azure Blob-opslag. BLOB-opslag is permanent.

## <a name="list-and-show-clusters"></a>Clusters tonen en vermelden
1. Meld u aan bij [https://portal.azure.com](https://portal.azure.com).
2. Klik op **HDInsight-Clusters** in het menu links.
3. Klik op de naam van het cluster. Als de clusterlijst te lang is, kunt u filteren boven aan de pagina.
4. Dubbelklik op een cluster in de lijst om de details weer te geven.

    **Menu- en essentials**:

    ![Azure portal HDInsight-cluster essentials](./media/hdinsight-administer-use-management-portal/hdinsight-essentials.png)

   * Voor het aanpassen van het menu met de rechtermuisknop op een willekeurige plaats in het menu en klik vervolgens op **aanpassen**.
   * **Instellingen voor** en **alle instellingen**: Geeft de **instellingen** blade voor het cluster, zodat u kunt de gedetailleerde configuratie-informatie voor het cluster.
   * **Dashboard**, **Cluster-Dashboard** en **URL**: Dit zijn alle manieren toegang krijgen tot de clusterdashboard, dat Ambari Web voor op basis van Linux-clusters is.
   * **Secure Shell**: Ziet u de instructies voor het verbinden met het cluster via Secure Shell (SSH)-verbinding.
   * **Cluster schalen**: Hiermee kunt u het aantal worker-knooppunten voor dit cluster wijzigen.
   * **Verwijder**: Hiermee verwijdert u het cluster.
   * **Snelstartgids**: Geeft informatie weer die u helpen aan de slag met HDInsight.
   * **Gebruikers**: Hiermee kunt u machtigingen instellen voor *portal management* van dit cluster voor andere gebruikers in uw Azure-abonnement.

     > [!IMPORTANT]  
     > Dit *alleen* is van invloed op toegang en machtigingen voor dit cluster in Azure portal en heeft geen invloed op wie kan verbinding maken met of verzenden van taken naar het HDInsight-cluster.

   * **Tags**: Tags kunnen u om in te stellen van sleutel/waarde-paren voor het definiëren van een aangepaste taxonomie van uw cloud-services. Bijvoorbeeld, u kunt maken met een sleutel met de naam **project**, en vervolgens een overeenkomende waarde gebruiken voor alle services die zijn gekoppeld aan een bepaald project.
   * **Ambari-weergaven**: Koppelingen naar Ambari Web.

     > [!IMPORTANT]  
     > Voor het beheren van de services die worden geleverd door het HDInsight-cluster, moet u de Ambari Web- of de Ambari REST-API gebruiken. Zie voor meer informatie over het gebruik van Ambari [beheren HDInsight-clusters met behulp van Apache Ambari](hdinsight-hadoop-manage-ambari.md).

     **Gebruik**:

     ![Gebruik Azure portal HDInsight-cluster](./media/hdinsight-administer-use-management-portal/hdinsight-portal-cluster-usage.png)
5. Klik op **Instellingen**.

    ![Gebruik Azure portal HDInsight-cluster](./media/hdinsight-administer-use-management-portal/hdinsight.portal.cluster.settings.png)

   * **Eigenschappen**: Bekijk de eigenschappen van het cluster.
   * **AAD-identiteit cluster**:
   * **Azure Storage-sleutels**: Bekijk het standaardaccount voor opslag en de bijbehorende sleutel. Het opslagaccount is configuratie tijdens het maken van het cluster.
   * **Cluster-aanmelding**: De cluster HTTP-gebruikersnaam en wachtwoord wijzigen.
   * **Externe Metastores**: Weergave de [Apache Hive](https://hive.apache.org/) en [Apache Oozie](https://oozie.apache.org/) metastores. De metastores kan alleen worden geconfigureerd tijdens het maken van het cluster.
   * **Cluster schalen**: Verhogen en verlagen van het aantal worker-clusterknooppunten.
   * **Extern bureaublad**: Inschakelen en uitschakelen van extern bureaublad (RDP)-toegang en configureren van de RDP-gebruikersnaam.  De RDP-gebruikersnaam moet verschillen van de naam van het HTTP-gebruiker.
   * **Partner van Record**:

     > [!NOTE]  
     > Dit is een algemeen overzicht van beschikbare instellingen. niet alle wordt aanwezig zijn voor alle clustertypen.

6. Klik op **eigenschappen**:

    De sectie met eigenschappen geeft een lijst van de volgende:

   * **Hostnaam**: De naam van het cluster.
   * **Cluster-URL**.
   * **Status**: Opnemen is afgebroken, geaccepteerd, ClusterStorageProvisioned, AzureVMConfiguration, HDInsightConfiguration, operationeel is, wordt uitgevoerd, fout, te verwijderen, verwijderd, time-out, DeleteQueued, DeleteTimedout, DeleteError, PatchQueued, CertRolloverQueued, ResizeQueued, ClusterCustomization
   * **Regio**: Azure-locatie. Zie voor een lijst van ondersteunde Azure-locaties, de **regio** vervolgkeuzelijst op [HDInsight prijzen](https://azure.microsoft.com/pricing/details/hdinsight/).
   * **Gegevens die zijn gemaakt**.
   * **Besturingssysteem**: Een van beide **Windows** of **Linux**.
   * **Type**: Hadoop, HBase, Storm, Spark.
   * **Versie**. Zie [HDInsight-versies](hdinsight-component-versioning.md)
   * **Abonnement**: Abonnementsnaam.
   * **Abonnements-ID**.
   * **Primaire gegevensbron**. De Azure Blob storage-account gebruikt als het standaardbestandssysteem voor Hadoop.
   * **Worker-knooppunten prijscategorie**.
   * **Hoofdknooppunt prijscategorie**.

## <a name="delete-clusters"></a>Clusters verwijderen
Een cluster verwijderen wordt niet verwijderd voor het standaardopslagaccount of alle gekoppelde opslagaccounts. U kunt het cluster opnieuw maken met behulp van de dezelfde opslagaccounts en de dezelfde metastores.

1. Aanmelden bij de [Portal][azure-portal].
2. Klik op **door alles bladeren** in het menu links, klikt u op **HDInsight-Clusters**, klikt u op de clusternaam van uw.
3. Klik op **verwijderen** in het menu bovenaan en volg de instructies.

Zie ook [onderbreken/clusters afsluiten](#pauseshut-down-clusters).

## <a name="scale-clusters"></a>Clusters schalen
Het cluster schalen functie kunt u het aantal worker-knooppunten die worden gebruikt door een cluster dat wordt uitgevoerd in Azure HDInsight zonder te hoeven maken van het cluster opnieuw wijzigen.

> [!NOTE]  
> Alleen clusters met HDInsight versie 3.1.3 of hoger worden ondersteund. Als u de versie van het cluster niet zeker weet, kunt u de eigenschappenpagina controleren.  Zie [clusters tonen en vermelden](#list-and-show-clusters).

De gevolgen van het wijzigen van het aantal gegevensknooppunten voor elk type cluster die door HDInsight worden ondersteund:

* Apache Hadoop

    Het aantal worker-knooppunten in een Hadoop-cluster dat wordt uitgevoerd zonder gevolgen voor alle taken die in behandeling of wordt uitgevoerd, kunt u naadloos verhogen. Nieuwe taken kunnen ook worden verzonden terwijl de bewerking uitgevoerd wordt. Fouten in een bewerking voor vergroten/verkleinen probleemloos verwerkt zodat het cluster altijd in een functionele staat blijft.

    Wanneer een Hadoop-cluster is omlaag geschaald door het aantal gegevensknooppunten te verminderen, zijn sommige van de services in het cluster opnieuw opgestart. Dit zorgt ervoor dat alle actieve en in behandeling zijnde taken mislukken na het voltooien van de bewerking vergroten/verkleinen. U kunt echter de taken opnieuw zodra de bewerking voltooid is.
* Apache HBase

    U kunt naadloos toevoegen of verwijderen van knooppunten in uw HBase-cluster, terwijl deze wordt uitgevoerd. Regionale Servers worden automatisch verdeeld binnen een paar minuten na voltooiing van de vergroten/verkleinen bewerking. U kunt echter ook handmatig de regionale servers verdelen door te melden bij het hoofdknooppunt van het cluster en de volgende opdrachten uitvoert vanuit een opdrachtpromptvenster:

        >pushd %HBASE_HOME%\bin
        >hbase shell
        >balancer

    Zie voor meer informatie over het gebruik van de HBase-shell,]
* Apache Storm

    U kunt naadloos toevoegen of verwijderen van gegevensknooppunten naar uw Storm-cluster, terwijl deze wordt uitgevoerd. Maar na een geslaagde bewerking vergroten/verkleinen is voltooid, moet u opnieuw verdelen van de topologie.

    Herverdelen kan worden uitgevoerd op twee manieren:

  * Apache Storm-Webgebruikersinterface
  * Opdrachtregelinterface (CLI)-hulpprogramma

    Raadpleeg de [Apache Storm documentatie](https://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) voor meer informatie.

    De Storm-webgebruikersinterface is beschikbaar op het HDInsight-cluster:

    ![HDInsight storm schaal opnieuw verdelen](./media/hdinsight-administer-use-management-portal/hdinsight-portal-scale-cluster-storm-rebalance.png)

    Hier volgt een voorbeeld van hoe u de CLI-opdracht opnieuw verdelen van de Storm-topologie:

        ## Reconfigure the topology "mytopology" to use 5 worker processes,
        ## the spout "blue-spout" to use 3 executors, and
        ## the bolt "yellow-bolt" to use 10 executors
        $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10

**Voor het schalen van clusters**

1. Aanmelden bij de [Portal][azure-portal].
2. Klik op **door alles bladeren** in het menu links, klikt u op **HDInsight-Clusters**, klikt u op de clusternaam van uw.
3. Klik op **instellingen** van het bovenste menu en klik vervolgens op **Cluster schalen**.
4. Voer **nummer van de Worker-knooppunten**. De limiet voor het aantal clusterknooppunt verschilt per Azure-abonnementen. U kunt contact opnemen met ondersteuning voor facturering om de limiet te verhogen.  De kostengegevens weer de wijzigingen die u hebt aangebracht in het aantal knooppunten.

    ![Spark voor HDInsight Hadoop HBase Storm schaal](./media/hdinsight-administer-use-management-portal/hdinsight.portal.scale.cluster.png)

## <a name="pauseshut-down-clusters"></a>Onderbreken/clusters afsluiten
De meeste van Hadoop-taken zijn batchtaken die alleen af en toe worden uitgevoerd. Er zijn grote perioden die het cluster niet wordt gebruikt voor de verwerking voor de meeste Hadoop-clusters. Met HDInsight worden uw gegevens opgeslagen in Azure Storage zodat u een cluster veilig kunt verwijderen wanneer deze niet wordt gebruikt.
Voor een HDInsight-cluster worden ook kosten in rekening gebracht, zelfs wanneer het niet wordt gebruikt. Aangezien de kosten voor het cluster vaak zoveel hoger zijn dan de kosten voor opslag, is het financieel gezien logischer clusters te verwijderen wanneer ze niet worden gebruikt.

Er zijn veel manieren waarop u het proces kunt programmeren:

* Gebruiker Azure-Gegevensfactory. Zie [gekoppelde Azure HDInsight-Service](../data-factory/compute-linked-services.md) en [transformeren en analyseren met Azure Data Factory](../data-factory/transform-data.md) voor on-demand en zelf gedefinieerde HDInsight gekoppelde services.
* Azure PowerShell gebruiken.  Zie [gegevens over vertraagde vluchten analyseren](hdinsight-analyze-flight-delay-data.md).
* Gebruik Azure klassieke CLI. Zie [beheren HDInsight-clusters met behulp van de klassieke Azure-CLI](hdinsight-administer-use-command-line.md).
* HDInsight .NET SDK gebruiken. Zie [indienen Apache Hadoop-taken](hadoop/submit-apache-hadoop-jobs-programmatically.md).

Zie voor informatie over de prijzen, [HDInsight prijzen](https://azure.microsoft.com/pricing/details/hdinsight/). Als een cluster verwijderen uit de Portal, Zie [clusters verwijderen](#delete-clusters)

## <a name="change-cluster-username"></a>Gebruikersnaam van de cluster wijzigen
Een HDInsight-cluster kan twee gebruikersaccounts hebben. Het gebruikersaccount van de HDInsight-cluster is gemaakt tijdens het proces voor het maken. U kunt ook een RDP-gebruikersaccount voor toegang tot het cluster via RDP maken. Zie [extern bureaublad inschakelen](#connect-to-hdinsight-clusters-by-using-rdp).

**De gebruikersnaam van HDInsight-cluster en het wachtwoord wijzigen**

1. Aanmelden bij de [Portal][azure-portal].
2. Klik op **door alles bladeren** in het menu links, klikt u op **HDInsight-Clusters**, klikt u op de clusternaam van uw.
3. Klik op **instellingen** van het bovenste menu en klik vervolgens op **aanmelding bij Cluster**.
4. Als **aanmelding bij Cluster** is ingeschakeld, moet u klikken op **uitschakelen**, en klik vervolgens op **inschakelen** voordat u de gebruikersnaam en wachtwoord kunt wijzigen...
5. Wijzig de **aanmeldingsnaam van de Cluster** en/of de **wachtwoord voor Clusteraanmelding**, en klik vervolgens op **opslaan**.

    ![HDInsight cluster gebruiker gebruikersnaam wachtwoord HTTP-gebruiker wijzigen](./media/hdinsight-administer-use-management-portal/hdinsight.portal.change.username.password.png)

## <a name="grantrevoke-access"></a>Toegang verlenen of in te trekken
HDInsight-clusters hebben de volgende HTTP-webservices (al deze services hebben RESTful-eindpunten):

* ODBC
* JDBC
* Ambari
* Oozie
* Templeton

Standaard worden deze services worden verleend om toegang te krijgen. U kunt in te trekken/verlenen toegang tot de vanuit Azure portal.

> [!NOTE]  
> Door de toegang verlenen/intrekken, stelt u de cluster-gebruikersnaam en wachtwoord.

**Web-services toegang te verlenen of in te trekken HTTP**

1. Aanmelden bij de [Portal][azure-portal].
2. Klik op **door alles bladeren** in het menu links, klikt u op **HDInsight-Clusters**, klikt u op de clusternaam van uw.
3. Klik op **instellingen** van het bovenste menu en klik vervolgens op **aanmelding bij Cluster**.
4. Als **aanmelding bij Cluster** is ingeschakeld, moet u klikken op **uitschakelen**, en klik vervolgens op **inschakelen** voordat u de gebruikersnaam en wachtwoord kunt wijzigen...
5. Voor **de gebruikersnaam voor Clusteraanmelding** en **wachtwoord voor Clusteraanmelding**, geeft u de nieuwe gebruikersnaam en het wachtwoord (respectievelijk) voor het cluster.
6. Klik op **OPSLAAN**.

    ![Grand HDInsight toegang tot HTTP-webservice verwijderen](./media/hdinsight-administer-use-management-portal/hdinsight.portal.change.username.password.png)

## <a name="find-the-default-storage-account"></a>Het standaardopslagaccount vinden
Elk HDInsight-cluster is een standaardopslagaccount. Het standaardaccount voor opslag en de sleutels voor een cluster wordt weergegeven onder **instellingen**/**eigenschappen**/**Azure Storage-sleutels**. Zie [clusters tonen en vermelden](#list-and-show-clusters).

## <a name="find-the-resource-group"></a>Zoek de resourcegroep
In de modus Azure Resource Manager wordt elke HDInsight-cluster gemaakt met een Azure-resourcegroep. De Azure-resourcegroep die deel uitmaakt van een cluster te wordt weergegeven in:

* De clusterlijst heeft een **resourcegroep** kolom.
* Cluster **essentiële** tegel.  

Zie [clusters tonen en vermelden](#list-and-show-clusters).

## <a name="open-hdinsight-query-console"></a>HDInsight-Query-console openen
De queryconsole van HDInsight omvat de volgende functies:

* **Hive-Editor**: Een GUI-webinterface voor het indienen van Hive-taken.  Zie [uitvoeren Apache Hive-query's met behulp van de Queryconsole](hadoop/apache-hadoop-use-hive-query-console.md).

    ![HDInsight portal hive-editor](./media/hdinsight-administer-use-management-portal/hdinsight-hive-editor.png)
* **Taakgeschiedenis**: Hadoop-taken bewaken.  

    ![HDInsight-portal Taakgeschiedenis](./media/hdinsight-administer-use-management-portal/hdinsight-job-history.png)

    Klik op **querynaam** om weer te geven van de informatie over de eigenschappen, zoals **Taakquery**, en ** Taakuitvoer. U kunt zowel de query en de uitvoer ook downloaden naar uw werkstation.
* **Bestand Browser**: Blader door het standaardaccount voor opslag en de gekoppelde storage-accounts.

    ![HDInsight portal bestand browser bladeren](./media/hdinsight-administer-use-management-portal/hdinsight-file-browser.png)

    Op de schermopname is de **<Account>** type geeft aan dat het item is een Azure storage-account.  Klik op de accountnaam op door de bestanden bladeren.
* **Hadoop-gebruikersinterface**.

    ![Portal voor HDInsight Hadoop UI](./media/hdinsight-administer-use-management-portal/hdinsight-hadoop-ui.png)

    Van **Hadoop UI*, kunt u door bestanden bladeren en controleert u Logboeken.
* **Yarn-gebruikersinterface**.

    ![YARN-gebruikersinterface van de HDInsight-portal](./media/hdinsight-administer-use-management-portal/hdinsight-yarn-ui.png)

## <a name="run-hive-queries"></a>Hive-query's uitvoeren
Voor Hive-taken uitvoeren vanaf de Portal, klikt u op **Hive-Editor** aan de queryconsole van HDInsight. Zie [Open HDInsight queryconsole](#open-hdinsight-query-console).

## <a name="monitor-jobs"></a>Taken controleren
Als u wilt bewaken van taken vanuit de Portal, klikt u op **Taakgeschiedenis** aan de queryconsole van HDInsight. Zie [Open HDInsight queryconsole](#open-hdinsight-query-console).

## <a name="browse-files"></a>Door bestanden bladeren
Als u bestanden die zijn opgeslagen in het standaardopslagaccount en de gekoppelde storage-accounts, klikt u op **bestandsbrowser** aan de queryconsole van HDInsight. Zie [Open HDInsight queryconsole](#open-hdinsight-query-console).

U kunt ook de **bladeren in het bestandssysteem** hulpprogramma van de **Hadoop UI** in de HDInsight-console.  Zie [Open HDInsight queryconsole](#open-hdinsight-query-console).

## <a name="monitor-cluster-usage"></a>Cluster-gebruik bewaken
De **gebruik** sectie van de HDInsight-cluster-blade vindt u informatie over het aantal kernen beschikbaar voor uw abonnement voor gebruik met HDInsight, evenals het aantal kernen dat is toegewezen aan dit cluster en hoe ze worden toegewezen voor de knooppunten in dit cluster. Zie [clusters tonen en vermelden](#list-and-show-clusters).

> [!IMPORTANT]  
> Voor het controleren van de services die worden geleverd door het HDInsight-cluster, moet u de Ambari Web- of de Ambari REST-API gebruiken. Zie voor meer informatie over het gebruik van Ambari [beheren HDInsight-clusters met behulp van Apache Ambari](hdinsight-hadoop-manage-ambari.md).

## <a name="open-hadoop-ui"></a>Open Hadoop-UI
Voor het controleren van het cluster, het bestandssysteem, bladeren en controleert u Logboeken, klikt u op **Hadoop UI** aan de queryconsole van HDInsight. Zie [Open HDInsight queryconsole](#open-hdinsight-query-console).

## <a name="open-yarn-ui"></a>Open Yarn-gebruikersinterface
Voor het gebruik van Yarn-gebruikersinterface, klikt u op **gebruikersinterface van Yarn** aan de queryconsole van HDInsight. Zie [Open HDInsight queryconsole](#open-hdinsight-query-console).

## <a name="connect-to-clusters-using-rdp"></a>Verbinding maken met clusters met behulp van RDP
De referenties voor het cluster dat u hebt opgegeven bij het maken van de geven toegang tot de services op het cluster, maar niet aan het cluster zelf via Extern bureaublad. U kunt Extern bureaublad-toegang inschakelen wanneer u een cluster inrichten of nadat een cluster is ingericht. Zie voor instructies over het inschakelen van extern bureaublad bij het maken van [maken van HDInsight-cluster](hdinsight-hadoop-provision-linux-clusters.md).

**Extern bureaublad inschakelen**

1. Aanmelden bij de [Portal][azure-portal].
2. Klik op **door alles bladeren** in het menu links, klikt u op **HDInsight-Clusters**, klikt u op de clusternaam van uw.
3. Klik op **instellingen** van het bovenste menu en klik vervolgens op **extern bureaublad**.
4. Voer **verloopt op**, **gebruikersnaam voor extern bureaublad** en **wachtwoord voor extern bureaublad**, en klik vervolgens op **inschakelen**.

    ![Extern bureaublad uitschakelen voor HDInsight configureren](./media/hdinsight-administer-use-management-portal/hdinsight.portal.remote.desktop.png)

    De standaardwaarden voor verloopt op is een week.

   > [!NOTE]  
   > U kunt ook de HDInsight .NET SDK gebruiken om in te schakelen van extern bureaublad op een cluster. Gebruik de **EnableRdp** methode voor het HDInsight-client-object in de volgende manier: **client. EnableRdp (clusternaam, locatie, "rdpuser", "rdppassword", DateTime.Now.AddDays(6))**. Op dezelfde manier als wilt uitschakelen extern bureaublad op het cluster, kunt u **client. DisableRdp (clusternaam, locatie)**. Zie voor meer informatie over deze methoden, [HDInsight .NET SDK-referentie](https://go.microsoft.com/fwlink/?LinkId=529017). Dit is alleen van toepassing op HDInsight-clusters die worden uitgevoerd op Windows.

**Verbinding maken met een cluster met behulp van RDP**

1. Aanmelden bij de [Portal][azure-portal].
2. Klik op **door alles bladeren** in het menu links, klikt u op **HDInsight-Clusters**, klikt u op de clusternaam van uw.
3. Klik op **instellingen** van het bovenste menu en klik vervolgens op **extern bureaublad**.
4. Klik op **Connect** en volg de instructies. Als u verbinding maken is uitgeschakeld, moet u het eerst inschakelen. Zorg ervoor dat de gebruikersnaam voor extern bureaublad-gebruiker en het wachtwoord.  U kunt de referenties van de Cluster-gebruiker niet gebruiken.

## <a name="open-hadoop-command-line"></a>Hadoop-opdrachtregels openen
Als u wilt verbinding maken met het cluster met behulp van extern bureaublad en de Hadoop-opdrachtregel gebruiken, moet u eerst hebt ingeschakeld extern bureaublad-toegang tot het cluster zoals beschreven in de vorige sectie.

**Openen van een Hadoop-opdrachtregel**

1. Verbinding maken met het cluster met behulp van extern bureaublad.
2. Dubbelklik op het bureaublad op **Hadoop-opdrachtregels**.

    ![HDI.HadoopCommandLine][image-hadoopcommandline]

    Zie voor meer informatie over Hadoop-opdrachten [Apache Hadoop-opdrachten verwijzing](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/CommandsManual.html).

Naam van de map heeft in de vorige schermafbeelding, de Hadoop versienummer dat is ingesloten. Het versienummer kunt wijzigen op basis van de versie van de Hadoop-onderdelen geïnstalleerd op het cluster. U kunt omgevingsvariabelen Hadoop gebruiken om te verwijzen naar deze mappen. Bijvoorbeeld:

    cd %hadoop_home%
    cd %hive_home%
    cd %hbase_home%
    cd %pig_home%
    cd %sqoop_home%
    cd %hcatalog_home%

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe u een HDInsight-cluster maakt met behulp van de Portal en het openen van het opdrachtregelprogramma van Apache Hadoop. Zie de volgende artikelen voor meer informatie:

* [HDInsight met behulp van Azure PowerShell beheren](hdinsight-administer-use-powershell.md)
* [HDInsight met behulp van Azure Classic CLI beheren](hdinsight-administer-use-command-line.md)
* [HDInsight-clusters maken](hdinsight-hadoop-provision-linux-clusters.md)
* [Programmatisch verzenden van Apache Hadoop-taken](hadoop/submit-apache-hadoop-jobs-programmatically.md)
* [Aan de slag met Azure HDInsight](hadoop/apache-hadoop-linux-tutorial-get-started.md)
* [Welke versie van Apache Hadoop is in Azure HDInsight?](hdinsight-component-versioning.md)

[azure-portal]: https://portal.azure.com
[image-hadoopcommandline]: ./media/hdinsight-administer-use-management-portal/hdinsight-hadoop-command-line.png "Hadoop-opdrachtregel"
