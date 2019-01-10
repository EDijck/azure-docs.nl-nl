---
title: Containerdoorvoer inrichten in Azure Cosmos DB
description: Meer informatie over het inrichten van doorvoer op containerniveau in Azure Cosmos DB
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 11/06/2018
ms.author: mjbrown
ms.openlocfilehash: eb34385087118614f8d7057c2229bc3c9e8d1ae4
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54039483"
---
# <a name="provision-throughput-for-an-azure-cosmos-db-container"></a>Doorvoer inrichten voor een Azure Cosmos DB-container

In dit artikel wordt uitgelegd hoe u doorvoer inricht voor een container (collectie, graaf, tabel) in Azure Cosmos DB. U kunt de doorvoer inrichten voor één container of [inrichten voor een database](how-to-provision-database-throughput.md) en deze delen met de containers in de database. U kunt de doorvoer voor een container inrichten met behulp van de Azure-portal, Azure CLI of CosmosDB-SDK's.

## <a name="provision-throughput-using-azure-portal"></a>Doorvoer inrichten met behulp van de Azure-portal

1. Meld u aan bij de [Azure-portal](https://portal.azure.com/).

1. [Maak een nieuw Cosmos DB-account](create-sql-api-dotnet.md#create-a-database-account) of selecteer een bestaand account.

1. Open het deelvenster **Data Explorer** en selecteer **Nieuwe verzameling**. Vul vervolgens de volgende details in het formulier in:

   * Maak een nieuwe database of gebruik een bestaande.
   * Voer de id in van een collectie (of tabel, graaf).
   * Voer een waarde voor de partitiesleutel in, bijvoorbeeld `/userid`.
   * Voer een doorvoer in, bijvoorbeeld 1000 RU's.
   * Selecteer **OK**.

![SQL API: doorvoer voor containers inrichten](./media/how-to-provision-container-throughput/provision-container-throughput-portal-all-api.png)

## <a name="provision-throughput-using-azure-cli"></a>Doorvoer inrichten met behulp van Azure CLI

```azurecli-interactive
# Create a container with a partition key and provision throughput of 1000 RU/s
az cosmosdb collection create \
    --resource-group $resourceGroupName \
    --collection-name $containerName \
    --name $accountName \
    --db-name $databaseName \
    --partition-key-path /myPartitionKey \
    --throughput 1000
```

Als u de doorvoer inricht voor een Cosmos-account dat is geconfigureerd met de Azure Cosmos DB-API voor MongoDB, gebruikt u '/myShardKey' voor het partitiesleutelpad. Als u de doorvoer inricht voor een Cosmos-account dat is geconfigureerd voor de Cassandra-API, gebruikt u '/myPrimaryKey' voor het partitiesleutelpad.

## <a name="provision-throughput-using-net-sdk"></a>Doorvoer inrichten met behulp van .NET SDK

> [!Note]
> Gebruik de SQL API voor het inrichten van doorvoer voor alle API's, met uitzondering van Cassandra-API.

### <a id="dotnet-most"></a>SQL-, MongoDB-, Gremlin- en Table-API's

```csharp
// Create a container with a partition key and provision throughput of 1000 RU/s
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "myContainerName";
myCollection.PartitionKey.Paths.Add("/myPartitionKey");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("myDatabaseName"),
    myCollection,
    new RequestOptions { OfferThroughput = 1000 });
```

### <a id="dotnet-cassandra"></a>Cassandra-API

```csharp
// Create a Cassandra table with a partition (primary) key and provision throughput of 1000 RU/s
session.Execute(CREATE TABLE myKeySpace.myTable(
    user_id int PRIMARY KEY,
    firstName text,
    lastName text) WITH cosmosdb_provisioned_throughput=1000);
```

## <a name="next-steps"></a>Volgende stappen

Zie de volgende artikelen voor meer informatie over het inrichten van doorvoer in Cosmos DB:

* [Doorvoer inrichten voor een database](how-to-provision-database-throughput.md)
* [Aanvraageenheden en doorvoer in Azure Cosmos DB](request-units.md)
