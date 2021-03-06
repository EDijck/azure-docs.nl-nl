---
title: Een Azure Resource Manager-sjabloon gebruiken om een werkruimte te maken
titleSuffix: Azure Machine Learning service
description: Leer hoe u een Azure Resource Manager-sjabloon gebruiken om te maken van een nieuwe werkruimte voor Azure Machine Learning-service.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: larryfr
author: Blackmist
ms.date: 04/16/2019
ms.custom: seoapril2019
ms.openlocfilehash: abe497ed96515e8194fb2ddefd8e7f4cb9908758
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65205140"
---
# <a name="use-an-azure-resource-manager-template-to-create-a-workspace-for-azure-machine-learning-service"></a>Een Azure Resource Manager-sjabloon gebruiken om te maken van een werkruimte voor Azure Machine Learning-service

In dit artikel leert u enkele manieren om te maken van een werkruimte van Azure Machine Learning-service met behulp van Azure Resource Manager-sjablonen. Resource Manager-sjabloon kunt u eenvoudig is om resources te maken als een enkele, gecoördineerde bewerking. Een sjabloon is een JSON-document waarin de resources die nodig zijn voor een implementatie. Het kan ook implementatieparameters opgeven. Parameters worden gebruikt voor invoerwaarden bij het gebruik van de sjabloon.

Zie voor meer informatie, [een toepassing implementeren met Azure Resource Manager-sjabloon](../../azure-resource-manager/resource-group-template-deploy.md).

## <a name="prerequisites"></a>Vereisten

* Een **Azure-abonnement**. Als u een hebt, kunt u de [gratis of betaalde versie van Azure Machine Learning-service](https://aka.ms/AMLFree).

* Een sjabloon van een CLI wilt gebruiken, moet u een [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azps-1.2.0) of de [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="resource-manager-template"></a>Resource Manager-sjabloon

De volgende Resource Manager-sjabloon kan worden gebruikt om een werkruimte van Azure Machine Learning-service en de gekoppelde Azure-resources te maken:

[!code-json[create-azure-machine-learning-service-workspace](~/quickstart-templates/101-machine-learning-create/azuredeploy.json)]

Deze sjabloon maakt u de volgende Azure-services:

* Azure-resourcegroep
* Azure Storage-account
* Azure Key Vault
* Azure Application Insights
* Azure Container Registry
* Azure Machine Learning-werkruimte

De resourcegroep is de container met de services. De verschillende services zijn vereist voor de Azure Machine Learning-werkruimte.

De voorbeeldsjabloon heeft twee parameters:

* De **locatie** waar de resourcegroep en de services worden gemaakt.

    De sjabloon wordt de locatie die u selecteert voor de meeste resources gebruiken. De uitzondering hierop is de Application Insights-service is niet beschikbaar in alle locaties zijn de andere services. Als u een locatie waar deze niet beschikbaar is selecteert, wordt de service worden gemaakt in de locatie Zuid-centraal VS.

* De **Werkruimtenaam**, dit is de beschrijvende naam van de Azure Machine Learning-werkruimte.

    De namen van de andere services worden willekeurig gegenereerd.

Zie de volgende artikelen voor meer informatie over sjablonen:

* [Azure Resource Manager-sjablonen maken](../../azure-resource-manager/resource-group-authoring-templates.md)
* [Een toepassing implementeren met Azure Resource Manager-sjablonen](../../azure-resource-manager/resource-group-template-deploy.md)
* [Microsoft.MachineLearningServices resource types](https://docs.microsoft.com/azure/templates/microsoft.machinelearningservices/allversions)

## <a name="use-the-azure-portal"></a>Azure Portal gebruiken

1. Volg de stappen in [resources implementeren vanuit een aangepaste sjabloon](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy-portal#deploy-resources-from-custom-template). Als u aankomen bij de __template bewerken__ scherm, in de sjabloon van dit document plakken.
1. Selecteer __opslaan__ de sjabloon wilt gebruiken. Geef de volgende informatie en ga akkoord met de vermelde voorwaarden:

   * Abonnement: Selecteer het Azure-abonnement moet worden gebruikt voor deze resources.
   * Resourcegroep: Selecteer of maak een resourcegroep voor de services bevatten.
   * Werkruimtenaam: De naam moet worden gebruikt voor de Azure Machine Learning-werkruimte die worden gemaakt. Naam van de werkruimte moet tussen 3 en 33 tekens lang zijn. Deze mag alleen alfanumerieke tekens bevatten en '-'.
   * Locatie: Selecteer de locatie waar de resources worden gemaakt.

     ![De sjabloonparameters in Azure portal](media/how-to-create-workspace-template/template-parameters.png)

Zie voor meer informatie, [resources implementeren vanuit een aangepaste sjabloon](../../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template).

## <a name="use-azure-powershell"></a>Azure PowerShell gebruiken

In dit voorbeeld wordt ervan uitgegaan dat u de sjabloon hebt opgeslagen naar een bestand met de naam `azuredeploy.json` in de huidige map:

```powershell
New-AzResourceGroup -Name examplegroup -Location "East US"
new-azresourcegroupdeployment -name exampledeployment `
  -resourcegroupname examplegroup -location "East US" `
  -templatefile .\azuredeploy.json -workspaceName "exampleworkspace"
```

Zie voor meer informatie, [resources implementeren met Resource Manager-sjablonen en Azure PowerShell](../../azure-resource-manager/resource-group-template-deploy.md) en [persoonlijke Resource Manager-sjabloon implementeren met SAS-token en Azure PowerShell](../../azure-resource-manager/resource-manager-powershell-sas-token.md).

## <a name="use-azure-cli"></a>Azure CLI gebruiken

In dit voorbeeld wordt ervan uitgegaan dat u de sjabloon hebt opgeslagen naar een bestand met de naam `azuredeploy.json` in de huidige map:

```azurecli-interactive
az group create --name examplegroup --location "East US"
az group deployment create \
  --name exampledeployment \
  --resource-group examplegroup \
  --template-file azuredeploy.json \
  --parameters workspaceName=exampleworkspace
```

Zie voor meer informatie, [resources implementeren met Resource Manager-sjablonen en Azure CLI](../../azure-resource-manager/resource-group-template-deploy-cli.md) en [persoonlijke Resource Manager-sjabloon implementeren met SAS-token en Azure CLI](../../azure-resource-manager/resource-manager-cli-sas-token.md).

## <a name="next-steps"></a>Volgende stappen

* [Resources implementeren met Resource Manager-sjablonen en Resource Manager REST API](../../azure-resource-manager/resource-group-template-deploy-rest.md).
* [Het maken en implementeren van Azure-resourcegroepen met Visual Studio](../../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).
