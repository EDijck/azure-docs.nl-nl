---
title: 'Zelfstudie: gegevens opslaan met SQL-module - Azure IoT Edge | Microsoft Docs'
description: Leer hoe u lokaal gegevens kunt opslaan op uw IoT-Edge-apparaat met een SQL Server-module
services: iot-edge
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 03/28/2019
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc, seodec18
ms.openlocfilehash: a83b8a56a8108f86d868e3420d8368c74fba308a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60612317"
---
# <a name="tutorial-store-data-at-the-edge-with-sql-server-databases"></a>Zelfstudie: Gegevens aan de rand opslaan met SQL Server-databases

Gebruik Azure IoT Edge en SQL Server voor het opslaan en opvragen van gegevens aan de rand. Azure IoT Edge heeft basisopslagmogelijkheden waarmee berichten in cache worden geplaatst als een apparaat offline is, en worden doorgestuurd wanneer de verbinding weer tot stand is gebracht. Mogelijk wilt u echter meer geavanceerde opslagmogelijkheden, zoals het lokaal kunnen opvragen van gegevens. Uw IoT Edge-apparaten kunnen lokale databases gebruiken om uit te voeren complexere computing zonder te onderhouden van een verbinding met IoT Hub. 

Dit artikel bevat instructies voor het implementeren van een SQL Server-database op een IoT Edge-apparaat. Azure Functions, dat op het IoT Edge-apparaat wordt uitgevoerd, structureert de binnenkomende gegevens en verzendt deze naar de database. De stappen in dit artikel kunnen ook worden toegepast op andere databases die in containers werken, zoals MySQL of PostgreSQL.

In deze zelfstudie leert u het volgende: 

> [!div class="checklist"]
> * Visual Studio Code gebruiken om een Azure-functie te maken
> * Een SQL-database op uw IoT Edge-apparaat implementeren
> * Gebruik Visual Studio Code om modules te bouwen en deze op uw IoT Edge-apparaat te implementeren
> * Gegenereerde gegevens weergeven

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

Een Azure IoT Edge-apparaat:

* U kunt een virtuele machine van Azure gebruiken als een IoT Edge-apparaat met de volgende stappen in de Quick Start voor [Linux](quickstart-linux.md).
* SQL Server biedt alleen ondersteuning voor Linux-containers. Als u testen van deze zelfstudie wilt met behulp van een Windows-apparaat als uw IoT Edge-apparaat, moet u deze configureren dat gebruikmaakt van Linux-containers. Zie [De Azure IoT Edge-runtime installeren in Windows](how-to-install-iot-edge-windows.md) voor de vereisten en installatiestappen om de IoT Edge-runtime te configureren voor Linux-containers in Windows.

Cloudresources:

* Een gratis of standaard [IoT Hub](../iot-hub/iot-hub-create-through-portal.md)-laag in Azure. 

Ontwikkelingsresources:

* [Visual Studio Code](https://code.visualstudio.com/). 
* [C#-extensie voor Visual Studio Code (van OmniSharp) voor Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp). 
* [Azure IoT-hulpprogramma's voor Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge). 
* [.NET Core 2.1 SDK](https://www.microsoft.com/net/download). 
* [Docker CE](https://docs.docker.com/install/). 
  * Als u op een Windows-machine ontwikkelt, controleert u of Docker is [geconfigureerd voor het gebruik van Linux-containers](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers). 

## <a name="create-a-container-registry"></a>Een containerregister maken

In deze zelfstudie gebruikt u de Azure IoT-hulpprogramma's voor Visual Studio Code om een module te bouwen en maakt u een **containerinstallatiekopie** van de bestanden. Vervolgens pusht u deze installatiekopie naar een **register** waarin uw installatiekopieën worden opgeslagen en beheerd. Tot slot implementeert u de installatiekopie uit het register voor uitvoering op uw IoT Edge-apparaat.  

U kunt een Docker-register gebruiken om de containerinstallatiekopieën op te slaan. Twee populaire Docker-registerservices zijn [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) en [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). In deze zelfstudie wordt Azure Container Registry gebruikt. 

Als u nog geen containerregister hebt, volgt u deze stappen om een nieuw containerregister te maken in Azure:

1. Selecteer in [Azure Portal](https://portal.azure.com) de optie **Een resource maken** > **Containers** > **Container Registry**.

2. Geef de volgende waarden op om uw containerregister te maken:

   | Veld | Value | 
   | ----- | ----- |
   | Registernaam | Geef hier een unieke naam op. |
   | Abonnement | Selecteer een abonnement in de vervolgkeuzelijst. |
   | Resourcegroep | Het wordt aangeraden om dezelfde resourcegroep te gebruiken voor alle test-resources die u maakt met de snelstartgidsen en zelfstudies voor IoT Edge, zoals **IoTEdgeResources**. |
   | Locatie | Kies een locatie dicht bij u in de buurt. |
   | Beheerder | Stel deze optie in op **Inschakelen**. |
   | SKU | Selecteer **Basic**. | 

5. Selecteer **Maken**.

6. Nadat het containerregister is gemaakt, bladert u ernaartoe en selecteert u vervolgens **Toegangssleutels**. 

7. Kopieer de waarden voor **Aanmeldingsserver**, **Gebruikersnaam** en **wachtwoord**. U gebruikt deze waarden later in de zelfstudie om toegang te verlenen tot het containerregister.  

## <a name="create-a-function-project"></a>Een functieproject maken

Om gegevens naar een database te verzenden hebt u een module nodig die de gegevens correct kan structureren en vervolgens in een tabel opslaat. 

De volgende stappen laten zien hoe u een IoT-Edge-functie maakt met behulp van Visual Studio Code en de Azure IoT Edge-hulpprogramma's.

1. Open Visual Studio Code.

2. Selecteer **View** > **Command Palette** en open het VS Code-opdrachtpalet.

3. Typ in het opdrachtpalet de opdracht **Azure IoT Edge: New IoT Edge solution** in en voer deze uit. Geef in het opdrachtpalet de volgende informatie op om de oplossing te maken: 

   | Veld | Value |
   | ----- | ----- |
   | Map selecteren | Kies de locatie op uw ontwikkelcomputer waar VS Code de oplossingsbestanden moet maken. |
   | Een naam opgeven voor de oplossing | Voer een beschrijvende naam voor de oplossing in, bijvoorbeeld **SqlSolution**, of accepteer de standaardinstelling. |
   | Modulesjabloon selecteren | Kies **Azure Functions - C#**. |
   | Een modulenaam opgeven | Noem uw module **sqlFunction**. |
   | Opslagplaats voor Docker-afbeeldingen voor de module opgeven | Een opslagplaats voor afbeeldingen bevat de naam van het containerregister en de naam van uw containerafbeelding. De containerafbeelding wordt vooraf gevuld vanuit de laatste stap. Vervang **localhost:5000** door de waarde van de aanmeldingsserver uit uw Azure-containerregister. U vindt de aanmeldingsserver op de overzichtspagina van het containerregister in de Azure-portal. <br><br>De uiteindelijke tekenreeks ziet er ongeveer als volgt uit: \<registernaam\>.azurecr.io/sqlFunction. |

   In het VS Code-venster wordt de werkruimte van de IoT Edge-oplossing geladen. 
   
4. Open het \.ENV-bestand in de werkruimte van uw IoT Edge-oplossing. 

   Als u een nieuwe IoT Edge-oplossing maakt, wordt u via VS Code gevraagd om uw registerreferenties op te geven in het \.env-bestand. Het bestand wordt genegeerd in Git, en wordt later met de IoT Edge-extensie gebruikt voor registertoegang tot uw IoT Edge-apparaat. 

   Als u in de vorige stap niet uw containerregister hebt opgeven, maar de standaardinstelling localhost:5000 hebt geaccepteerd, hebt u geen \.ENV-bestand.

5. Geef in het bestand deployment.template.json de IoT Edge runtime uw registerreferenties, zodat deze toegang heeft tot uw module-installatiekopieën. Zoek de secties **CONTAINER_REGISTRY_USERNAME** en **CONTAINER_REGISTRY_PASSWORD** op en voeg uw referenties toe na het gelijkteken: 

   ```env
   CONTAINER_REGISTRY_USERNAME_yourregistry=<username>
   CONTAINER_REGISTRY_PASSWORD_yourregistry=<password>
   ```

6. Sla het .env-bestand op.

7. Open in VS Code Explorer **modules** > **sqlFunction** > **sqlFunction.cs**.

8. Vervang de volledige inhoud van het bestand door de volgende code:

   ```csharp
   using System;
   using System.Collections.Generic;
   using System.IO;
   using System.Text;
   using System.Threading.Tasks;
   using Microsoft.Azure.Devices.Client;
   using Microsoft.Azure.WebJobs;
   using Microsoft.Azure.WebJobs.Extensions.EdgeHub;
   using Microsoft.Azure.WebJobs.Host;
   using Microsoft.Extensions.Logging;
   using Newtonsoft.Json;
   using Sql = System.Data.SqlClient;

   namespace Functions.Samples
   {
       public static class sqlFunction
       {
           [FunctionName("sqlFunction")]
           public static async Task FilterMessageAndSendMessage(
               [EdgeHubTrigger("input1")] Message messageReceived,
               [EdgeHub(OutputName = "output1")] IAsyncCollector<Message> output,
               ILogger logger)
           {
               const int temperatureThreshold = 20;
               byte[] messageBytes = messageReceived.GetBytes();
               var messageString = System.Text.Encoding.UTF8.GetString(messageBytes);

               if (!string.IsNullOrEmpty(messageString))
               {
                   logger.LogInformation("Info: Received one non-empty message");
                   // Get the body of the message and deserialize it.
                   var messageBody = JsonConvert.DeserializeObject<MessageBody>(messageString);

                   //Store the data in SQL db
                   const string str = "<sql connection string>";
                   using (Sql.SqlConnection conn = new Sql.SqlConnection(str))
                   {
                       conn.Open();
                       var insertMachineTemperature = "INSERT INTO MeasurementsDB.dbo.TemperatureMeasurements VALUES (CONVERT(DATETIME2,'" + messageBody.timeCreated + "', 127), 'machine', " + messageBody.machine.temperature + ");";
                       var insertAmbientTemperature = "INSERT INTO MeasurementsDB.dbo.TemperatureMeasurements VALUES (CONVERT(DATETIME2,'" + messageBody.timeCreated + "', 127), 'ambient', " + messageBody.ambient.temperature + ");"; 
                       using (Sql.SqlCommand cmd = new Sql.SqlCommand(insertMachineTemperature + "\n" + insertAmbientTemperature, conn))
                       {
                           //Execute the command and log the # rows affected.
                           var rows = await cmd.ExecuteNonQueryAsync();
                           logger.LogInformation($"{rows} rows were updated");
                       }
                   }

                   if (messageBody != null && messageBody.machine.temperature > temperatureThreshold)
                   {
                       // Send the message to the output as the temperature value is greater than the threashold.
                       var filteredMessage = new Message(messageBytes);
                       // Copy the properties of the original message into the new Message object.
                       foreach (KeyValuePair<string, string> prop in messageReceived.Properties)
                       {filteredMessage.Properties.Add(prop.Key, prop.Value);}
                       // Add a new property to the message to indicate it is an alert.
                       filteredMessage.Properties.Add("MessageType", "Alert");
                       // Send the message.       
                       await output.AddAsync(filteredMessage);
                       logger.LogInformation("Info: Received and transferred a message with temperature above the threshold");
                   }
               }
           }
       }
       //Define the expected schema for the body of incoming messages.
       class MessageBody
       {
           public Machine machine {get; set;}
           public Ambient ambient {get; set;}
           public string timeCreated {get; set;}
       }
       class Machine
       {
           public double temperature {get; set;}
           public double pressure {get; set;}         
       }
       class Ambient
       {
           public double temperature {get; set;}
           public int humidity {get; set;}         
       }
   }
   ```

6. Vervang in regel 35 de tekenreeks **\<sql-verbindingsreeks\>** door de volgende tekenreeks. De **gegevensbron** eigenschap verwijst naar de SQL Server-container, zodat nog niet bestaat, maar u deze maken met de naam van de **SQL** in de volgende sectie. 

   ```csharp
   Data Source=tcp:sql,1433;Initial Catalog=MeasurementsDB;User Id=SA;Password=Strong!Passw0rd;TrustServerCertificate=False;Connection Timeout=30;
   ```

7. Sla het bestand **sqlFunction.cs** op. 

8. Open het bestand **sqlFunction.csproj**.

9. De groep van pakket met verwijzingen zoeken en toevoegen van een nieuw account om op te nemen SqlClient. 

   ```csproj
   <PackageReference Include="System.Data.SqlClient" Version="4.5.1"/>
   ```

10. Sla het bestand **sqlFunction.csproj** op.

## <a name="add-the-sql-server-container"></a>De SQL Server-container toevoegen

Een [distributiemanifest](module-composition.md) declareert welke modules de IoT Edge-runtime op uw IoT Edge-apparaat zal installeren. U hebt opgegeven dat de code voor het maken van een aangepaste functiemodule in de vorige sectie, maar de SQL Server-module is al ingebouwd en beschikbaar in de Azure Marketplace. U hoeft alleen aan de IoT Edge-runtime op te geven dat het deze module moet opnemen, en die vervolgens op uw apparaat te configureren. 

1. Open in Visual Studio Code, het opdrachtenpalet hiervoor **weergave** > **Command palette**.

2. Typ in het opdrachtpalet de opdracht **Azure IoT Edge: IoT Edge-module toevoegen**. Geef de volgende informatie om toe te voegen een nieuwe module in het opdrachtenpalet: 

   | Veld | Value | 
   | ----- | ----- |
   | Implementatiesjabloonbestand selecteren | Het opdrachtenpalet ziet u het bestand deployment.template.json in uw huidige oplossingenmap. Selecteer dat bestand.  |
   | Modulesjabloon selecteren | Selecteer **Module op basis van Azure Marketplace**. |

3. Zoek in de marketplace met Azure IoT Edge-module en selecteer **SQL Server-Module**. 

4. Wijzig de modulenaam in **sql**, alleen kleine letters bevatten. Deze naam komt overeen met de naam van de container die in de verbindingsreeks in het bestand sqlFunction.cs gedeclareerd. 

5. Selecteer **importeren** de module toevoegen aan uw oplossing. 

6. Open in de oplossingenmap van uw, de **deployment.template.json** bestand. 

7. Ga naar de sectie **modules**. Hier ziet u drie modules. De module *tempSensor* is standaard opgenomen in nieuwe oplossingen en biedt testgegevens voor gebruik met uw andere modules. De module *sqlFunction* is de module die u in eerste instantie hebt gemaakt en bijgewerkt met nieuwe code. Ten slotte de module *sql* is geïmporteerd uit Azure Marketplace. 

   >[!Tip]
   >De SQL Server-module wordt geleverd met een standaardwachtwoord instellen in de omgevingsvariabelen van het manifest van de implementatie. Telkens wanneer u een SQL Server-container in een productieomgeving maakt, moet u [het standaardwachtwoord van de systeembeheerder](https://docs.microsoft.com/sql/linux/quickstart-install-connect-docker)wijzigen.

8. Sluit de **deployment.template.json** bestand.

## <a name="build-your-iot-edge-solution"></a>Uw eigen IoT Edge-oplossing bouwen

In de voorgaande secties hebt u een oplossing met één module gemaakt en er vervolgens nog een toegevoegd aan de distributiemanifestsjabloon. De SQL Server-module openbaar wordt gehost door Microsoft, maar u moet de code in de Functions-module in een container plaatsen. In deze sectie, maak de oplossing, containerinstallatiekopieën maakt voor de module sqlFunction en push de installatiekopie naar uw containerregister. 

1. Open de in Visual Studio Code geïntegreerde terminal door **View** > **Terminal** te selecteren.  

1. Meld u aan bij uw containerregister in Visual Studio Code, zodat u uw installatiekopieën naar uw register kunt pushen. Gebruik de dezelfde referenties voor Azure Container Registry (ACR) die u hebt toegevoegd aan het .env-bestand. Voer de volgende opdracht in de geïntegreerde terminal in:

    ```csh/sh
    docker login -u <ACR username> -p <ACR password> <ACR login server>
    ```
    
    U ziet mogelijk een beveiligingswaarschuwing geadviseerd het gebruik van de--wachtwoord stdin-parameter. Hoewel buiten het bestek van dit artikel, wordt u deze best practice aangeraden. Zie voor meer informatie de [dockeraanmelding](https://docs.docker.com/engine/reference/commandline/login/#provide-a-password-using-stdin) opdrachten. 

2. Klik in VS Code Explorer met de rechtermuisknop op het bestand **deployment.template.json** en selecteer **Build and Push IoT Edge solution**. 

Wanneer u Visual Studio Code de opdracht geeft om uw oplossing te bouwen, wordt eerst een bestand deployment.json gemaakt in een nieuwe map genaamd **config** op basis van de informatie in de distributiesjabloon. Vervolgens worden twee opdrachten uitgevoerd in de geïntegreerde terminal: `docker build` en `docker push`. Met deze twee opdrachten wordt de code gebouwd, wordt de module opgeslagen in een container, en wordt de code vervolgens naar het containerregister gepusht dat u hebt opgegeven toen u de oplossing initialiseerde. 

U kunt controleren dat de module sqlFunction met succes is gepusht naar uw containerregister. Ga naar het containerregister in Azure portal. Selecteer **opslagplaatsen** en zoek naar de **sqlFunction**. De andere twee modules, tempSensor en sql, wordt niet naar het containerregister worden gepusht, omdat u al die naar de opslagplaatsen in de Microsoft-registers verwijst.

## <a name="deploy-the-solution-to-a-device"></a>De oplossing implementeren op een apparaat

U kunt modules op een apparaat instellen via de IoT Hub, maar u hebt ook toegang tot uw IoT Hub en apparaten via Visual Studio Code. In dit gedeelte stelt u de toegang tot uw IoT Hub in en gebruikt u VS Code om uw oplossing op uw IoT Edge-apparaat te implementeren. 

1. Selecteer in het opdrachtenpalet van VS Code **Azure IoT Hub: Select IoT Hub**.

2. Volg de aanwijzingen om u aan te melden bij uw Azure-account. 

3. Selecteer in het opdrachtenpalet uw Azure-abonnement en selecteer vervolgens uw IoT Hub. 

4. Vouw in VS Code Explorer de sectie **Azure IoT Hub Devices** uit. 

5. Klik met de rechtermuisknop op het doelapparaat van uw implementatie en selecteer **Implementatie maken voor één apparaat**. 

   ![Implementatie voor één apparaat maken](./media/tutorial-store-data-sql-server/create-deployment.png)

6. Ga in de bestandsverkenner naar de map **config** in uw oplossing en kies **deployment.amd64**. Klik op **Edge-distributiemanifest selecteren**. 

   Gebruik het bestand deployment.template.json niet als een implementatie-manifest.

Als de implementatie is geslaagd, wordt er een bevestigingsbericht weergegeven in de VS Code-uitvoer. 

Vernieuw de status van uw apparaat in de sectie Azure IoT Hub-apparaten van VS Code. De nieuwe modules worden weergegeven en beginnen gedurende de volgende minuten met rapporteren terwijl de containers worden geïnstalleerd en gestart. U kunt ook controleren of alle modules op uw apparaat actief zijn. Voer op uw IoT Edge-apparaat de volgende opdracht uit om de status van de modules te bekijken. 

   ```cmd/sh
   iotedge list
   ```

## <a name="create-the-sql-database"></a>De SQL-database maken

Wanneer u het distributiemanifest op uw apparaat toepast, krijgt u drie modules die worden uitgevoerd. De module tempSensor genereert gesimuleerde omgevingsgegevens. De module sqlFunction neemt de gegevens en deelt deze in voor een database. Deze sectie leidt u door het instellen van de SQL-database voor het opslaan van de temperatuurgegevens. 

Voer de volgende opdrachten uit op uw IoT Edge-apparaat. Met deze opdrachten wordt verbinding gemaakt met de **sql**-module die wordt uitgevoerd op uw apparaat en worden er een database en een tabel gemaakt voor de opslag van de temperatuurgegevens die naar de module worden verzonden. 

1. Gebruik een opdrachtregelprogramma op uw IoT Edge-apparaat om verbinding te maken met uw database. 
      ```bash
      sudo docker exec -it sql bash
      ```

2. Open het SQL-opdrachthulpprogramma.
      ```bash
      /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P 'Strong!Passw0rd'
      ```

3. Uw database maken: 
      ```sql
      CREATE DATABASE MeasurementsDB
      ON
      (NAME = MeasurementsDB, FILENAME = '/var/opt/mssql/measurementsdb.mdf')
      GO
      ```

4. Definieer uw tabel.

   ```sql
   CREATE TABLE MeasurementsDB.dbo.TemperatureMeasurements (measurementTime DATETIME2, location NVARCHAR(50), temperature FLOAT)
   GO
   ```

U kunt het dockerbestand van uw SQL Server aanpassen om uw SQL Server automatisch in te stellen om te worden geïmplementeerd op meerdere IoT Edge-apparaten. Zie het [Microsoft SQL Server-containerdemo-project](https://github.com/twright-msft/mssql-node-docker-demo-app)voor meer informatie. 

## <a name="view-the-local-data"></a>De lokale gegevens bekijken

Nadat uw tabel is gemaakt, begint de module sqlFunction gegevens op te slaan in een lokale SQL Server 2017-database op uw IoT Edge-apparaat. 

Voer vanuit het SQL-opdrachthulpprogramma de volgende opdracht uit om uw opgemaakte tabelgegevens te bekijken: 

   ```sql
   SELECT * FROM MeasurementsDB.dbo.TemperatureMeasurements
   GO
   ```

   ![Inhoud van lokale database bekijken](./media/tutorial-store-data-sql-server/view-data.png)



## <a name="clean-up-resources"></a>Resources opschonen

Als u van plan bent door te gaan met het volgende aanbevolen artikel, kunt u de resources en configuraties die u hebt gemaakt behouden en opnieuw gebruiken. U kunt ook hetzelfde IoT Edge-apparaat blijven gebruiken als een testapparaat. 

Anders kunt u de lokale configuraties en Azure-resources die u in dit artikel hebt gemaakt, verwijderen om kosten te voorkomen. 

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een Azure Functions-module gemaakt die code bevat voor het filteren van onbewerkte gegevens die worden gegenereerd door uw IoT Edge-apparaat. Wanneer u klaar bent om uw eigen modules te bouwen, kunt u meer informatie krijgen over het [ontwikkelen van Azure Functions met Azure IoT Edge voor Visual Studio Code](how-to-develop-csharp-function.md). 

Ga verder met de volgende zelfstudies om te leren hoe Azure IoT Edge u verder kan helpen bij het omzetten van uw gegevens in bedrijfsinzichten.

> [!div class="nextstepaction"]
> [Sensorgegevens filteren met behulp van C#-code](tutorial-csharp-module.md)
