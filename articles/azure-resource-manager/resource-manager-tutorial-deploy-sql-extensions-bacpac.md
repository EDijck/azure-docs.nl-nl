---
title: SQL BACPAC-bestanden importeren met Azure Resource Manager-sjablonen | Microsoft Docs
description: Meer informatie over het gebruik van SQL Database-extensie om SQL BACPAC-bestanden te importeren met Azure Resource Manager-sjablonen.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: ''
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 04/08/2019
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 08befabfbd14651475fa56dec95bdf4c2fe54c9c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60390268"
---
# <a name="tutorial-import-sql-bacpac-files-with-azure-resource-manager-templates"></a>Zelfstudie: SQL BACPAC-bestanden importeren met Azure Resource Manager-sjablonen

Meer informatie over het gebruik van Azure SQL Database-extensies om een BACPAC-bestand te importeren met Azure Resource Manager-sjablonen. Implementatieartefacten zijn, naast het hoofdsjabloonbestand, bestanden die vereist zijn om een implementatie te voltooien. Het BACPAC-bestand is een artefact. In deze zelfstudie maakt u een sjabloon voor het implementeren van een Azure SQL-server, een SQL-database, en importeert u een BACPAC-bestand. Voor informatie over het implementeren van extensies van virtuele Azure-machines met behulp van Azure Resource Manager-sjablonen, raadpleegt u [# Zelfstudie: Extensies voor virtuele machines implementeren met Azure Resource Manager-sjablonen](./resource-manager-tutorial-deploy-vm-extensions.md).

Deze zelfstudie bestaat uit de volgende taken:

> [!div class="checklist"]
> * Een BACPAC-bestand voorbereiden
> * Een snelstartsjabloon openen
> * De sjabloon bewerken
> * De sjabloon implementeren
> * De implementatie controleren

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Als u dit artikel wilt voltooien, hebt u het volgende nodig:

* [Visual Studio Code](https://code.visualstudio.com/) met de extensie Resource Manager Tools. Zie [De extensie installeren](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).
* Gebruik een gegenereerd wachtwoord voor het beheerdersaccount van SQL Server om de beveiliging te verbeteren. Hier volgt een voorbeeld voor het genereren van een wachtwoord:

    ```azurecli-interactive
    openssl rand -base64 32
    ```
    Azure Key Vault is ontworpen om cryptografische sleutels en andere geheimen te beveiligen. Zie [Zelfstudie: Azure Key Vault integreren in de Resource Manager-sjabloonimplementatie](./resource-manager-tutorial-use-key-vault.md) voor meer informatie. We raden u ook aan om uw wachtwoord elke drie maanden te wijzigen.

## <a name="prepare-a-bacpac-file"></a>Een BACPAC-bestand voorbereiden

Een BACPAC-bestand wordt gedeeld in een [Azure Storage-account](https://armtutorials.blob.core.windows.net/sqlextensionbacpac/SQLDatabaseExtension.bacpac) met openbare toegang. Zie [Een Azure SQL-database exporteren naar een BACPAC-bestand](../sql-database/sql-database-export.md) als u een eigen account wilt maken. Als u ervoor kiest het bestand naar uw eigen locatie te publiceren, moet u de sjabloon later in de zelfstudie bijwerken.

## <a name="open-a-quickstart-template"></a>Een snelstartsjabloon openen

De sjabloon die wordt gebruikt in deze zelfstudie wordt opgeslagen in een [Azure Storage-account](https://armtutorials.blob.core.windows.net/createsql/azuredeploy.json). 

1. Selecteer in Visual Studio Code **Bestand**>**Bestand openen**.
2. Plak de volgende URL in **Bestandsnaam**:

    ```url
    https://armtutorials.blob.core.windows.net/createsql/azuredeploy.json
    ```
3. Selecteer **Openen** om het bestand te openen.

    Er worden drie resources gedefinieerd in de sjabloon:

   * `Microsoft.Sql/servers`. Zie de [sjabloonverwijzing](https://docs.microsoft.com/azure/templates/microsoft.sql/servers).
   * `Microsoft.SQL/servers/securityAlertPolicies`. Zie de [sjabloonverwijzing](https://docs.microsoft.com/azure/templates/microsoft.sql/servers/securityalertpolicies).
   * `Microsoft.SQL.servers/databases`.  Zie de [sjabloonverwijzing](https://docs.microsoft.com/azure/templates/microsoft.sql/servers/databases).

     Het is handig om enige basiskennis te hebben van de sjabloon voordat u deze gaat aanpassen.
4. Selecteer **Bestand**>**Opslaan als** om het bestand op uw lokale computer op te slaan als **azuredeploy.json**.

## <a name="edit-the-template"></a>De sjabloon bewerken

Voeg twee extra resources aan de sjabloon toe.

* Als u wilt toestaan dat de SQL database-extensie BACPAC-bestanden kan importeren, moet u toegang tot Azure-services toestaan. Voeg de volgende JSON toe aan de definitie van de SQL-server:

    ```json
    {
        "type": "firewallrules",
        "name": "AllowAllAzureIps",
        "location": "[parameters('location')]",
        "apiVersion": "2015-05-01-preview",
        "dependsOn": [
            "[variables('databaseServerName')]"
        ],
        "properties": {
            "startIpAddress": "0.0.0.0",
            "endIpAddress": "0.0.0.0"
        }
    }
    ```

    De sjabloon zal er als volgt uitzien:

    ![Azure Resource Manager sql-extensies importeren BACPAC](./media/resource-manager-tutorial-deploy-sql-extensions-bacpac/resource-manager-tutorial-deploy-sql-extensions-bacpac-firewall.png)

* Voeg met de volgende JSON een SQL Database-extensieresource toe aan de databasedefinitie:

    ```json
    "resources": [
        {
            "name": "Import",
            "type": "extensions",
            "apiVersion": "2014-04-01",
            "dependsOn": [
                "[resourceId('Microsoft.Sql/servers/databases', variables('databaseServerName'), variables('databaseName'))]"
            ],
            "properties": {
                "storageKeyType": "SharedAccessKey",
                "storageKey": "?",
                "storageUri": "https://armtutorials.blob.core.windows.net/sqlextensionbacpac/SQLDatabaseExtension.bacpac",
                "administratorLogin": "[variables('databaseServerAdminLogin')]",
                "administratorLoginPassword": "[variables('databaseServerAdminLoginPassword')]",
                "operationMode": "Import",
            }
        }
    ]
    ```

    De sjabloon zal er als volgt uitzien:

    ![Azure Resource Manager sql-extensies importeren BACPAC](./media/resource-manager-tutorial-deploy-sql-extensions-bacpac/resource-manager-tutorial-deploy-sql-extensions-bacpac.png)

    Zie de [verwijzing voor SQL Database-extensies](https://docs.microsoft.com/azure/templates/microsoft.sql/servers/databases/extensions) voor meer informatie over de resourcedefinitie. Hier volgen enkele belangrijke elementen:

    * **dependsOn**: de extensieresource moet worden gemaakt nadat de SQL-database is gemaakt.
    * **storageKeyType**: het type te gebruiken opslagsleutel. De waarde kan `StorageAccessKey` of `SharedAccessKey` zijn. Omdat het opgegeven BACPAC-bestand in een Azure Storage-account met openbare toegang wordt gedeeld, wordt hier 'SharedAccessKey' gebruikt.
    * **storageKey**: de te gebruiken opslagsleutel. Als het type opslagsleutel SharedAccessKey is, moet deze worden voorafgegaan door een '?'.
    * **storageUri**: de te gebruiken opslag-URI. Als u ervoor kiest om het verstrekte BACPAC-bestand niet te gebruiken, moet u de waarden bijwerken.
    * **administratorLoginPassword**: het wachtwoord van de SQL-beheerder. Gebruik een gegenereerd wachtwoord. Zie [Vereisten](#prerequisites).

## <a name="deploy-the-template"></a>De sjabloon implementeren

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Raadpleeg de sectie [De sjabloon implementeren](./resource-manager-tutorial-create-templates-with-dependent-resources.md#deploy-the-template) voor de implementatieprocedure. Gebruik in plaats daarvan het volgende PowerShell-implementatiescript:

```azurepowershell
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$adminUsername = Read-Host -Prompt "Enter the SQL admin username"
$adminPassword = Read-Host -Prompt "Enter the admin password" -AsSecureString

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -adminUser $adminUsername `
    -adminPassword $adminPassword `
    -TemplateFile "$HOME/azuredeploy.json"
```

Gebruik een gegenereerd wachtwoord. Zie [Vereisten](#prerequisites).

## <a name="verify-the-deployment"></a>De implementatie controleren

Selecteer in de portal de SQL-database in de geïmplementeerde resourcegroep. Selecteer **Query-editor (preview)** en voer vervolgens de beheerdersreferenties in. U ziet dat twee tabellen in de database worden geïmporteerd:

![Azure Resource Manager sql-extensies importeren BACPAC](./media/resource-manager-tutorial-deploy-sql-extensions-bacpac/resource-manager-tutorial-deploy-sql-extensions-bacpac-query-editor.png)

## <a name="clean-up-resources"></a>Resources opschonen

Schoon de geïmplementeerd Azure-resources, wanneer u deze niet meer nodig hebt, op door de resourcegroep te verwijderen.

1. Selecteer **Resourcegroep** in het linkermenu van Azure Portal.
2. Voer de naam van de resourcegroep in het veld **Filter by name** in.
3. Selecteer de naam van de resourcegroep.  U ziet in totaal zes resources in de resourcegroep.
4. Selecteer **Resourcegroep verwijderen** in het bovenste menu.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een SQL Server en een SQL Database geïmplementeerd en een BACPAC-bestand geïmporteerd. Het BACPAC-bestand wordt opgeslagen in een Azure Storage-account. Iedereen die de URL heeft, kan het bestand openen. Als u wilt weten hoe u het BACPAC-bestand (artefact) wilt beveiligen, raadpleegt u

> [!div class="nextstepaction"]
> [Secure the artifacts](./resource-manager-tutorial-secure-artifacts.md) (Artefacten beveiligen)
