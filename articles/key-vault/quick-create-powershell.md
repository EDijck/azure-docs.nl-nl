---
title: 'Azure-snelstart: Een geheim uit Key Vault instellen en ophalen met PowerShell | Microsoft Docs'
description: ''
services: key-vault
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 1126f665-2e6c-4cca-897e-7d61842e8334
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: powershell
ms.topic: quickstart
ms.custom: mvc
ms.date: 01/07/2019
ms.author: barclayn
ms.openlocfilehash: d3f2682c7e750885a6c3947ce47b5da45f251a25
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54421382"
---
# <a name="quickstart-set-and-retrieve-a-secret-from-azure-key-vault-using-powershell"></a>Quickstart: Een geheim uit Azure Key Vault instellen en ophalen met PowerShell

Azure Key Vault is een cloudservice die werkt als een beveiligd geheimenarchief. U kunt veilig sleutels, wachtwoorden, certificaten en andere geheime informatie opslaan. U kunt het [Overzicht](key-vault-overview.md) raadplegen voor meer informatie over Key Vault. In deze snelstart gebruikt u PowerShell om een sleutelkluis te maken. Vervolgens slaat u een geheim op in de zojuist gemaakte kluis.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Als u PowerShell lokaal wilt installeren en gebruiken, is voor deze zelfstudie versie 5.1.1 of hoger van de Azure PowerShell-module vereist. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/azurerm/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Login-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.

```azurepowershell-interactive
Login-AzureRmAccount
```

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een Azure-resourcegroep met de opdracht [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. 

```azurepowershell-interactive
New-AzureRmResourceGroup -Name ContosoResourceGroup -Location EastUS
```

## <a name="create-a-key-vault"></a>Een sleutelkluis maken

Vervolgens maakt u een sleutelkluis. Om deze stap te kunnen uitvoeren, hebt u enkele gegevens nodig:

Hoewel we in deze snelstart 'Contoso KeyVault2' als naam voor de sleutelkluis gebruiken, moet u een unieke naam opgeven.

- **Kluisnaam** Contoso-Vault2.
- **Naam van resourcegroep** ContosoResourceGroup.
- **Locatie** US - oost.

```azurepowershell-interactive
New-AzureRmKeyVault -Name 'Contoso-Vault2' -ResourceGroupName 'ContosoResourceGroup' -Location 'East US'
```

De uitvoer van deze cmdlet toont eigenschappen van de nieuw gemaakte sleutelkluis. Let op de onderstaande twee eigenschappen:

* **Kluisnaam**: in het voorbeeld is dat **Contoso-Vault2**. U gebruikt deze naam voor andere Sleutelkluis-cmdlets.
* **Kluis-URI**: in dit voorbeeld is dat https://contosokeyvault.vault.azure.net/. Toepassingen die via de REST API gebruikmaken van uw kluis, moeten deze URI gebruiken.

Nadat de kluis is gemaakt, is uw Azure-account het enige account dat iets met deze nieuwe kluis mag doen.

![Uitvoer nadat de opdracht voor het maken van de sleutelkluis is voltooid](./media/quick-create-powershell/output-after-creating-keyvault.png)

## <a name="adding-a-secret-to-key-vault"></a>Een geheim toevoegen aan Key Vault

Als u een geheim wilt toevoegen aan de kluis, hoeft u maar een paar stappen uit te voeren. In dit geval voegt u een wachtwoord toe dat door een toepassing kan worden gebruikt. Het wachtwoord heeft de naam **ExamplePassword** en slaat daarin de waarde van '**Pa$$w0rd**' op.

Converteer eerst de waarde van Pa$$w0rd naar een beveiligde tekenreeks door het volgende te typen:

```azurepowershell-interactive
$secretvalue = ConvertTo-SecureString 'Pa$$w0rd' -AsPlainText -Force
```

Typ vervolgens de onderstaande PowerShell-opdrachten om een geheim te maken in Key Vault met de naam **ExamplePassword** en de waarde **Pa$$w0rd**:

```azurepowershell-interactive
$secret = Set-AzureKeyVaultSecret -VaultName 'ContosoKeyVault' -Name 'ExamplePassword' -SecretValue $secretvalue
```

Als u de waarde in het geheim als tekst zonder opmaak wilt weergeven:

```azurepowershell-interactive
(Get-AzureKeyVaultSecret -vaultName "Contosokeyvault" -name "ExamplePassword").SecretValueText
```

U hebt nu een sleutelkluis gemaakt, een geheim opgeslagen en dat vervolgens opgehaald.

## <a name="clean-up-resources"></a>Resources opschonen

 Andere snelstartgidsen en zelfstudies in deze verzameling zijn gebaseerd op deze snelstartgids. Als u van plan bent om verder te gaan met andere snelstarts en zelfstudies, kunt u deze resources intact laten.

U kunt de opdracht [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) gebruiken om de resourcegroep, sleutelkluis en alle gerelateerde resources te verwijderen wanneer u deze niet meer nodig hebt.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name ContosoResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u een sleutelkluis gemaakt en daarin een softwaresleutel opgeslagen. Meer informatie over Key Vault en hoe u dat kunt gebruiken samen met uw toepassingen is te vinden in de zelfstudie voor webtoepassingen die geschikt zijn voor Key Vault.

> [!div class="nextstepaction"]
> Ga door met de volgende zelfstudie [Een Azure-webtoepassing configureren om een Key Vault-geheim te lezen](quick-create-net.md) voor informatie over het lezen van een Key Vault-geheim van een webtoepassing met behulp van beheerde service-id's voor Azure-resources.
