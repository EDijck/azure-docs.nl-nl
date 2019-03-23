---
title: Een Java-HBase-toepassing ontwikkelen voor Azure HDInsight op basis van Windows
description: Informatie over het gebruik van Apache Maven in een Apache HBase op basis van een Java-toepassing bouwen en vervolgens deze implementeren naar een Azure HDInsight op basis van een Windows-cluster.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 02/05/2017
ms.author: hrasheed
ROBOTS: NOINDEX
ms.openlocfilehash: c0ecfd3f148cecae713740ef37d4fe7a2e2f184f
ms.sourcegitcommit: 223604d8b6ef20a8c115ff877981ce22ada6155a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58361809"
---
# <a name="use-apache-maven-to-build-java-applications-that-use-apache-hbase-with-windows-based-hdinsight-apache-hadoop"></a>Apache Maven gebruiken om Java-toepassingen die gebruikmaken van Apache HBase met HDInsight op basis van Windows (Apache Hadoop) te bouwen
Meer informatie over het maken en bouw een [Apache HBase](https://hbase.apache.org/) toepassing in Java met behulp van Apache Maven. Gebruik vervolgens de toepassing met Azure HDInsight (Apache Hadoop).

[Apache Maven](https://maven.apache.org/) is een software-project beheer en begrip hulpprogramma waarmee u software, documentatie en rapporten voor Java-projecten maken. In dit artikel leert u hoe u een eenvoudige Java-toepassing die wordt gemaakt, query's, en Hiermee verwijdert u een HBase-tabel op een Azure HDInsight-cluster maken.

> [!IMPORTANT]  
> De stappen in dit document moet een HDInsight-cluster dat gebruik maakt van Windows. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

## <a name="requirements"></a>Vereisten

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

* [Java-platform JDK](https://aka.ms/azure-jdks) 7 of hoger
* [Apache Maven](https://maven.apache.org/)
* Een cluster met HDInsight op basis van Windows met HBase

    > [!NOTE]  
    > De stappen in dit document zijn getest met HDInsight-cluster versie 3.2 of 3.3. De standaardwaarden die is opgegeven in de voorbeelden zijn voor een 3.3 HDInsight-cluster.

## <a name="create-the-project"></a>Het project maken
1. Vanaf de opdrachtregel in uw ontwikkelingsomgeving, wijzig de mappen in de locatie waar u wilt maken van het project, bijvoorbeeld `cd code\hdinsight`.
2. Gebruik de **mvn** opdracht, die is geïnstalleerd met Maven, voor het genereren van het hulpprogramma voor het project.

        mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    Met deze opdracht maakt een map in de huidige locatie met de naam die is opgegeven door de **artifactID** parameter (**hbaseapp** in dit voorbeeld.) Deze map bevat de volgende items:

   * **pom.xml**:  De Project-objectmodel ([POM](https://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) bevat informatie en configuratie die wordt gebruikt om het project te bouwen.
   * **src**: De map waarin zich de **main\java\com\microsoft\examples** directory waarin u de toepassing zal maken.
3. Verwijder de **src\test\java\com\microsoft\examples\apptest.java** bestand omdat deze niet in dit voorbeeld wordt gebruikt.

## <a name="update-the-project-object-model"></a>Het objectmodel Project bijwerken
1. Bewerk de **pom.xml** -bestand en voeg de volgende code in de `<dependencies>` sectie:

        <dependency>
          <groupId>org.apache.hbase</groupId>
          <artifactId>hbase-client</artifactId>
          <version>1.1.2</version>
        </dependency>

    Dit gedeelte wordt uitgelegd Maven dat het project is vereist **hbase-client** versie **1.1.2**. Bij het compileren, wordt deze afhankelijkheid gedownload vanuit de standaard Maven-opslagplaats. U kunt de [Apache Maven Central Repository zoeken](https://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) voor meer informatie over deze afhankelijkheid.

   > [!IMPORTANT]  
   > Het versienummer moet overeenkomen met de versie van HBase die beschikbaar is in uw HDInsight-cluster. Gebruik de volgende tabel om te vinden van het juiste versienummer.
   >
   >

   | HDInsight-clusterversie | HBase-versie moet worden gebruikt |
   | --- | --- |
   | 3.2 |0.98.4-hadoop2 |
   | 3,3 |1.1.2 |

    Zie voor meer informatie over HDInsight-versies en onderdelen [wat de verschillende beschikbaar met HDInsight Apache Hadoop-onderdelen zijn](hdinsight-component-versioning.md).
2. Als u van een 3.3 HDInsight-cluster gebruikmaakt, moet u ook het volgende toevoegen de `<dependencies>` sectie:

        <dependency>
            <groupId>org.apache.phoenix</groupId>
            <artifactId>phoenix-core</artifactId>
            <version>4.4.0-HBase-1.1</version>
        </dependency>

    Met deze afhankelijkheid laadt de phoenix-core-onderdelen, die worden gebruikt door Hbase versie 1.1.x.
3. Voeg de volgende code aan de **pom.xml** bestand. In deze sectie moet zich binnen in de `<project>...</project>` labels in het bestand, bijvoorbeeld tussen `</dependencies>` en `</project>`.

        <build>
          <sourceDirectory>src</sourceDirectory>
          <resources>
              <resource>
                <directory>${basedir}/conf</directory>
                <filtering>false</filtering>
                <includes>
                  <include>hbase-site.xml</include>
                </includes>
              </resource>
            </resources>
          <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.3</version>
                <configuration>
                    <source>1.7</source>
                    <target>1.7</target>
                </configuration>
              </plugin>
            <plugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-shade-plugin</artifactId>
              <version>2.3</version>
              <configuration>
                <transformers>
                  <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                    </transformer>
                  </transformers>
              </configuration>
              <executions>
                <execution>
                  <phase>package</phase>
                  <goals>
                    <goal>shade</goal>
                  </goals>
                </execution>
              </executions>
            </plugin>
          </plugins>
        </build>

    De `<resources>` sectie configureert u een resource (**conf\hbase site.xml**) die configuratie-informatie voor HBase bevat.

   > [!NOTE]  
   > U kunt ook instellen-configuratiewaarden via code. Zie de opmerkingen in de **CreateTable** voorbeeld hieronder voor hoe u dit doet.
   >
   >

    Dit `<plugins>` sectie configureert u de [Apache Maven-Compiler invoegtoepassing](https://maven.apache.org/plugins/maven-compiler-plugin/) en [Apache Maven-invoegtoepassing voor tint](https://maven.apache.org/plugins/maven-shade-plugin/). De invoegtoepassing compiler wordt gebruikt voor het compileren van de topologie. De tint van de invoegtoepassing wordt gebruikt om te voorkomen dat de licentie duplicatie in het JAR-pakket dat is gebouwd met Maven. De reden dat deze wordt gebruikt, is dat de dubbele licentiebestanden ervoor zorgen een fout opgetreden tijdens de uitvoering van het HDInsight-cluster dat. Met behulp van maven-tint-invoegtoepassing met de `ApacheLicenseResourceTransformer` implementatie wordt voorkomen dat deze fout.

    De maven-tint-invoegtoepassing ook produceert een uber jar (of fat jar) die de afhankelijkheden die zijn vereist voor de toepassing bevat.
4. Sla het bestand **pom.xml** op.
5. Maak een nieuwe map met de naam **conf** in de **hbaseapp** directory. In de **conf** directory, maak een bestand met de naam **hbase-site.xml**. Gebruik de volgende als de inhoud van het bestand:

        <?xml version="1.0"?>
        <?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
        <!--
        /**
          * Copyright 2010 The Apache Software Foundation
          *
          * Licensed to the Apache Software Foundation (ASF) under one
          * or more contributor license agreements.  See the NOTICE file
          * distributed with this work for additional information
          * regarding copyright ownership.  The ASF licenses this file
          * to you under the Apache License, Version 2.0 (the
          * "License"); you may not use this file except in compliance
          * with the License.  You may obtain a copy of the License at
          *
          *     https://www.apache.org/licenses/LICENSE-2.0
          *
          * Unless required by applicable law or agreed to in writing, software
          * distributed under the License is distributed on an "AS IS" BASIS,
          * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
          * See the License for the specific language governing permissions and
          * limitations under the License.
          */
        -->
        <configuration>
          <property>
            <name>hbase.cluster.distributed</name>
            <value>true</value>
          </property>
          <property>
            <name>hbase.zookeeper.quorum</name>
            <value>zookeeper0,zookeeper1,zookeeper2</value>
          </property>
          <property>
            <name>hbase.zookeeper.property.clientPort</name>
            <value>2181</value>
          </property>
        </configuration>

    Dit bestand wordt gebruikt voor het laden van de HBase-configuratie voor een HDInsight-cluster.

   > [!NOTE]  
   > Dit is een minimale hbase-site.xml-bestand en de bare-minimale instellingen voor het HDInsight-cluster bevat.

6. Sla de **hbase-site.xml** bestand.

## <a name="create-the-application"></a>De toepassing maken
1. Ga naar de **hbaseapp\src\main\java\com\microsoft\examples** directory en de naam van het app.java-bestand naar **CreateTable.java**.
2. Open de **CreateTable.java** -bestand en vervang de bestaande inhoud door de volgende code:

        package com.microsoft.examples;
        import java.io.IOException;

        import org.apache.hadoop.conf.Configuration;
        import org.apache.hadoop.hbase.HBaseConfiguration;
        import org.apache.hadoop.hbase.client.HBaseAdmin;
        import org.apache.hadoop.hbase.HTableDescriptor;
        import org.apache.hadoop.hbase.TableName;
        import org.apache.hadoop.hbase.HColumnDescriptor;
        import org.apache.hadoop.hbase.client.HTable;
        import org.apache.hadoop.hbase.client.Put;
        import org.apache.hadoop.hbase.util.Bytes;

        public class CreateTable {
          public static void main(String[] args) throws IOException {
            Configuration config = HBaseConfiguration.create();

            // Example of setting zookeeper values for HDInsight
            // in code instead of an hbase-site.xml file
            //
            // config.set("hbase.zookeeper.quorum",
            //            "zookeepernode0,zookeepernode1,zookeepernode2");
            //config.set("hbase.zookeeper.property.clientPort", "2181");
            //config.set("hbase.cluster.distributed", "true");
            // The following sets the znode root for Linux-based HDInsight
            //config.set("zookeeper.znode.parent","/hbase-unsecure");

            // create an admin object using the config
            HBaseAdmin admin = new HBaseAdmin(config);

            // create the table...
            HTableDescriptor tableDescriptor = new HTableDescriptor(TableName.valueOf("people"));
            // ... with two column families
            tableDescriptor.addFamily(new HColumnDescriptor("name"));
            tableDescriptor.addFamily(new HColumnDescriptor("contactinfo"));
            admin.createTable(tableDescriptor);

            // define some people
            String[][] people = {
                { "1", "Marcel", "Haddad", "marcel@fabrikam.com"},
                { "2", "Franklin", "Holtz", "franklin@contoso.com" },
                { "3", "Dwayne", "McKee", "dwayne@fabrikam.com" },
                { "4", "Rae", "Schroeder", "rae@contoso.com" },
                { "5", "Rosalie", "burton", "rosalie@fabrikam.com"},
                { "6", "Gabriela", "Ingram", "gabriela@contoso.com"} };

            HTable table = new HTable(config, "people");

            // Add each person to the table
            //   Use the `name` column family for the name
            //   Use the `contactinfo` column family for the email
            for (int i = 0; i< people.length; i++) {
              Put person = new Put(Bytes.toBytes(people[i][0]));
              person.add(Bytes.toBytes("name"), Bytes.toBytes("first"), Bytes.toBytes(people[i][1]));
              person.add(Bytes.toBytes("name"), Bytes.toBytes("last"), Bytes.toBytes(people[i][2]));
              person.add(Bytes.toBytes("contactinfo"), Bytes.toBytes("email"), Bytes.toBytes(people[i][3]));
              table.put(person);
            }
            // flush commits and close the table
            table.flushCommits();
            table.close();
          }
        }

    Dit is de **CreateTable** klasse Hiermee u een tabel met de naam maakt **mensen** en deze vullen met bepaalde vooraf gedefinieerde gebruikers.
3. Sla de **CreateTable.java** bestand.
4. In de **hbaseapp\src\main\java\com\microsoft\examples** directory, maak een nieuw bestand met de naam **SearchByEmail.java**. Gebruik de volgende code als de inhoud van dit bestand:

        package com.microsoft.examples;
        import java.io.IOException;

        import org.apache.hadoop.conf.Configuration;
        import org.apache.hadoop.hbase.HBaseConfiguration;
        import org.apache.hadoop.hbase.client.HTable;
        import org.apache.hadoop.hbase.client.Scan;
        import org.apache.hadoop.hbase.client.ResultScanner;
        import org.apache.hadoop.hbase.client.Result;
        import org.apache.hadoop.hbase.filter.RegexStringComparator;
        import org.apache.hadoop.hbase.filter.SingleColumnValueFilter;
        import org.apache.hadoop.hbase.filter.CompareFilter.CompareOp;
        import org.apache.hadoop.hbase.util.Bytes;
        import org.apache.hadoop.util.GenericOptionsParser;

        public class SearchByEmail {
          public static void main(String[] args) throws IOException {
            Configuration config = HBaseConfiguration.create();

            // Use GenericOptionsParser to get only the parameters to the class
            // and not all the parameters passed (when using WebHCat for example)
            String[] otherArgs = new GenericOptionsParser(config, args).getRemainingArgs();
            if (otherArgs.length != 1) {
              System.out.println("usage: [regular expression]");
              System.exit(-1);
            }

            // Open the table
            HTable table = new HTable(config, "people");

            // Define the family and qualifiers to be used
            byte[] contactFamily = Bytes.toBytes("contactinfo");
            byte[] emailQualifier = Bytes.toBytes("email");
            byte[] nameFamily = Bytes.toBytes("name");
            byte[] firstNameQualifier = Bytes.toBytes("first");
            byte[] lastNameQualifier = Bytes.toBytes("last");

            // Create a new regex filter
            RegexStringComparator emailFilter = new RegexStringComparator(otherArgs[0]);
            // Attach the regex filter to a filter
            //   for the email column
            SingleColumnValueFilter filter = new SingleColumnValueFilter(
              contactFamily,
              emailQualifier,
              CompareOp.EQUAL,
              emailFilter
            );

            // Create a scan and set the filter
            Scan scan = new Scan();
            scan.setFilter(filter);

            // Get the results
            ResultScanner results = table.getScanner(scan);
            // Iterate over results and print  values
            for (Result result : results ) {
              String id = new String(result.getRow());
              byte[] firstNameObj = result.getValue(nameFamily, firstNameQualifier);
              String firstName = new String(firstNameObj);
              byte[] lastNameObj = result.getValue(nameFamily, lastNameQualifier);
              String lastName = new String(lastNameObj);
              System.out.println(firstName + " " + lastName + " - ID: " + id);
              byte[] emailObj = result.getValue(contactFamily, emailQualifier);
              String email = new String(emailObj);
              System.out.println(firstName + " " + lastName + " - " + email + " - ID: " + id);
            }
            results.close();
            table.close();
          }
        }

    De **SearchByEmail** klasse kan worden gebruikt om op te vragen rijen door e-mailadres. Omdat deze gebruikmaakt van een reguliere expressie-filter, kunt u een tekenreeks of een reguliere expressie opgeven bij het gebruik van de klasse.
5. Sla de **SearchByEmail.java** bestand.
6. In de **hbaseapp\src\main\hava\com\microsoft\examples** directory, maak een nieuw bestand met de naam **DeleteTable.java**. Gebruik de volgende code als de inhoud van dit bestand:

        package com.microsoft.examples;
        import java.io.IOException;

        import org.apache.hadoop.conf.Configuration;
        import org.apache.hadoop.hbase.HBaseConfiguration;
        import org.apache.hadoop.hbase.client.HBaseAdmin;

        public class DeleteTable {
          public static void main(String[] args) throws IOException {
            Configuration config = HBaseConfiguration.create();

            // Create an admin object using the config
            HBaseAdmin admin = new HBaseAdmin(config);

            // Disable, and then delete the table
            admin.disableTable("people");
            admin.deleteTable("people");
          }
        }

    Deze klasse is voor het opschonen van dit voorbeeld door te schakelen en neer te zetten in de tabel die zijn gemaakt door de **CreateTable** klasse.
7. Sla de **DeleteTable.java** bestand.

## <a name="build-and-package-the-application"></a>Bouwen en de toepassing verpakken
1. Open een opdrachtprompt en wijzig de mappen op de **hbaseapp** directory.
2. Gebruik de volgende opdracht om te maken van een JAR-bestand dat de toepassing bevat:

        mvn clean package

    Dit opschonen van alle voorgaande build-artefacten, downloadt alle afhankelijkheden die nog niet hebt geïnstalleerd, klikt u vervolgens bouwt en pakketten van de toepassing.
3. Wanneer de opdracht is voltooid, de **hbaseapp\target** map bevat een bestand met de naam **hbaseapp-1.0-SNAPSHOT.jar**.

   > [!NOTE]  
   > De **hbaseapp-1.0-SNAPSHOT.jar** bestand is een uber jar (ook wel een fat jar genoemd) die de afhankelijkheden die zijn vereist voor het uitvoeren van de toepassing bevat.

## <a name="upload-the-jar-file-and-start-a-job"></a>Het JAR-bestand uploaden en een taak starten
Er zijn veel manieren voor het uploaden van een bestand met uw HDInsight-cluster, zoals beschreven in [gegevens uploaden voor Apache Hadoop-taken in HDInsight](hdinsight-upload-data.md). De volgende stappen gebruikt Azure PowerShell.

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

1. Nadat het installeren en configureren van Azure PowerShell, maakt een nieuw bestand met de naam **hbase-runner.psm1**. Gebruik de volgende als de inhoud van dit bestand:

        <#
        .SYNOPSIS
        Copies a file to the primary storage of an HDInsight cluster.
        .DESCRIPTION
        Copies a file from a local directory to the blob container for
        the HDInsight cluster.
        .EXAMPLE
        Start-HBaseExample -className "com.microsoft.examples.CreateTable"
        -clusterName "MyHDInsightCluster"

        .EXAMPLE
        Start-HBaseExample -className "com.microsoft.examples.SearchByEmail"
        -clusterName "MyHDInsightCluster"
        -emailRegex "contoso.com"

        .EXAMPLE
        Start-HBaseExample -className "com.microsoft.examples.SearchByEmail"
        -clusterName "MyHDInsightCluster"
        -emailRegex "^r" -showErr
        #>

        function Start-HBaseExample {
        [CmdletBinding(SupportsShouldProcess = $true)]
        param(
        #The class to run
        [Parameter(Mandatory = $true)]
        [String]$className,

        #The name of the HDInsight cluster
        [Parameter(Mandatory = $true)]
        [String]$clusterName,

        #Only used when using SearchByEmail
        [Parameter(Mandatory = $false)]
        [String]$emailRegex,

        #Use if you want to see stderr output
        [Parameter(Mandatory = $false)]
        [Switch]$showErr
        )

        Set-StrictMode -Version 3

        # Is the Azure module installed?
        FindAzure

        # Get the login for the HDInsight cluster
        $creds=Get-Credential -Message "Enter the login for the cluster" -UserName "admin"

        # The JAR
        $jarFile = "wasb:///example/jars/hbaseapp-1.0-SNAPSHOT.jar"

        # The job definition
        $jobDefinition = New-AzHDInsightMapReduceJobDefinition `
            -JarFile $jarFile `
            -ClassName $className `
            -Arguments $emailRegex

        # Get the job output
        $job = Start-AzHDInsightJob `
            -ClusterName $clusterName `
            -JobDefinition $jobDefinition `
            -HttpCredential $creds
        Write-Host "Wait for the job to complete ..." -ForegroundColor Green
        Wait-AzHDInsightJob `
            -ClusterName $clusterName `
            -JobId $job.JobId `
            -HttpCredential $creds
        if($showErr)
        {
        Write-Host "STDERR"
        Get-AzHDInsightJobOutput `
                    -Clustername $clusterName `
                    -JobId $job.JobId `
                    -HttpCredential $creds `
                    -DisplayOutputType StandardError
        }
        Write-Host "Display the standard output ..." -ForegroundColor Green
        Get-AzHDInsightJobOutput `
                    -Clustername $clusterName `
                    -JobId $job.JobId `
                    -HttpCredential $creds
        }

        <#
        .SYNOPSIS
        Copies a file to the primary storage of an HDInsight cluster.
        .DESCRIPTION
        Copies a file from a local directory to the blob container for
        the HDInsight cluster.
        .EXAMPLE
        Add-HDInsightFile -localPath "C:\temp\data.txt"
        -destinationPath "example/data/data.txt"
        -ClusterName "MyHDInsightCluster"
        .EXAMPLE
        Add-HDInsightFile -localPath "C:\temp\data.txt"
        -destinationPath "example/data/data.txt"
        -ClusterName "MyHDInsightCluster"
        -Container "MyContainer"
        #>

        function Add-HDInsightFile {
            [CmdletBinding(SupportsShouldProcess = $true)]
            param(
                #The path to the local file.
                [Parameter(Mandatory = $true)]
                [String]$localPath,

                #The destination path and file name, relative to the root of the container.
                [Parameter(Mandatory = $true)]
                [String]$destinationPath,

                #The name of the HDInsight cluster
                [Parameter(Mandatory = $true)]
                [String]$clusterName,

                #If specified, overwrites existing files without prompting
                [Parameter(Mandatory = $false)]
                [Switch]$force
            )

            Set-StrictMode -Version 3

            # Is the Azure module installed?
            FindAzure

            # Get authentication for the cluster
            $creds=Get-Credential

            # Does the local path exist?
            if (-not (Test-Path $localPath))
            {
                throw "Source path '$localPath' does not exist."
            }

            # Get the primary storage container
            $storage = GetStorage -clusterName $clusterName

            # Upload file to storage, overwriting existing files if -force was used.
            Set-AzStorageBlobContent -File $localPath `
                -Blob $destinationPath `
                -force:$force `
                -Container $storage.container `
                -Context $storage.context
        }

        function FindAzure {
            # Is there an active Azure subscription?
            $sub = Get-AzSubscription -ErrorAction SilentlyContinue
            if(-not($sub))
            {
                throw "No active Azure subscription found! If you have a subscription, use the Connect-AzAccount cmdlet to login to your subscription."
            }
        }

        function GetStorage {
            param(
                [Parameter(Mandatory = $true)]
                [String]$clusterName
            )
            $hdi = Get-AzHDInsightCluster -ClusterName $clusterName
            # Does the cluster exist?
            if (!$hdi)
            {
                throw "HDInsight cluster '$clusterName' does not exist."
            }
            # Create a return object for context & container
            $return = @{}
            $storageAccounts = @{}

            # Get storage information
            $resourceGroup = $hdi.ResourceGroup
            $storageAccountName=$hdi.DefaultStorageAccount.split('.')[0]
            $container=$hdi.DefaultStorageContainer
            $storageAccountKey=(Get-AzStorageAccountKey `
                -Name $storageAccountName `
            -ResourceGroupName $resourceGroup)[0].Value
            # Get the resource group, in case we need that
            $return.resourceGroup = $resourceGroup
            # Get the storage context, as we can't depend
            # on using the default storage context
            $return.context = New-AzStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
            # Get the container, so we know where to
            # find/store blobs
            $return.container = $container
            # Return storage accounts to support finding all accounts for
            # a cluster
            $return.storageAccount = $storageAccountName
            $return.storageAccountKey = $storageAccountKey

            return $return
        }
        # Only export the verb-phrase things
        export-modulemember *-*

    Dit bestand bevat twee modules:

   * **Voeg HDInsightFile** : wordt gebruikt voor bestanden uploaden naar HDInsight
   * **Start-HBaseExample** : wordt gebruikt voor het uitvoeren van de klassen die eerder hebt gemaakt
2. Sla de **hbase-runner.psm1** bestand.
3. Open een nieuw Azure PowerShell-venster, wijzig de mappen op de **hbaseapp** directory en voer de volgende opdracht uit.

        PS C:\ Import-Module c:\path\to\hbase-runner.psm1

    Wijzig het pad naar de locatie van de **hbase-runner.psm1** bestand dat u eerder hebt gemaakt. Hiermee wordt de module geregistreerd voor deze Azure PowerShell-sessie.
4. Gebruik de volgende opdracht om te uploaden de **hbaseapp-1.0-SNAPSHOT.jar** met uw HDInsight-cluster.

        Add-HDInsightFile -localPath target\hbaseapp-1.0-SNAPSHOT.jar -destinationPath example/jars/hbaseapp-1.0-SNAPSHOT.jar -clusterName hdinsightclustername

    Vervang **hdinsightclustername** met de naam van uw HDInsight-cluster. De opdracht uploadt de **hbaseapp-1.0-SNAPSHOT.jar** naar de **voorbeeld/JAR-bestanden** locatie in de primaire opslag voor uw HDInsight-cluster.
5. Nadat de bestanden zijn geüpload, gebruikt u de volgende code in het maken van een tabel met de **hbaseapp**:

        Start-HBaseExample -className com.microsoft.examples.CreateTable -clusterName hdinsightclustername

    Vervang **hdinsightclustername** met de naam van uw HDInsight-cluster.

    Deze opdracht maakt u een nieuwe tabel met de naam **mensen** in uw HDInsight-cluster. Met deze opdracht wordt geen uitvoer niet weergegeven in het consolevenster.
6. Als u wilt zoeken naar vermeldingen in de tabel, gebruik de volgende opdracht:

        Start-HBaseExample -className com.microsoft.examples.SearchByEmail -clusterName hdinsightclustername -emailRegex contoso.com

    Vervang **hdinsightclustername** met de naam van uw HDInsight-cluster.

    Deze opdracht wordt de **SearchByEmail** klasse om te zoeken naar alle rijen waarin de **contactinformation** kolomfamilie en de **e** kolom bevat de tekenreeks **contoso.com**. U ontvangt de volgende resultaten:

          Franklin Holtz - ID: 2
          Franklin Holtz - franklin@contoso.com - ID: 2
          Rae Schroeder - ID: 4
          Rae Schroeder - rae@contoso.com - ID: 4
          Gabriela Ingram - ID: 6
          Gabriela Ingram - gabriela@contoso.com - ID: 6

    Met behulp van **fabrikam.com** voor de `-emailRegex` waarde als resultaat geeft de gebruikers die hebben **fabrikam.com** in het veld e-mailbericht. Omdat deze zoekopdracht wordt geïmplementeerd met behulp van een reguliere expressie op basis van filter, kunt u ook opgeven reguliere expressies, zoals **^ r**, retourneert gegevens waar het e-mailbericht met de letter 'r' begint.

## <a name="delete-the-table"></a>De tabel verwijderen
Wanneer u klaar bent met het voorbeeld, gebruik de volgende opdracht uit vanuit de Azure PowerShell-sessie om te verwijderen de **mensen** tabel die wordt gebruikt in dit voorbeeld:

    Start-HBaseExample -className com.microsoft.examples.DeleteTable -clusterName hdinsightclustername

Vervang **hdinsightclustername** met de naam van uw HDInsight-cluster.

## <a name="troubleshooting"></a>Problemen oplossen
### <a name="no-results-or-unexpected-results-when-using-start-hbaseexample"></a>Er zijn geen resultaten of onverwachte resultaten bij het gebruik van Start-HBaseExample
Gebruik de `-showErr` parameter om de standaardfout (STDERR) dat wordt gegenereerd tijdens het uitvoeren van de taak weer te geven.
