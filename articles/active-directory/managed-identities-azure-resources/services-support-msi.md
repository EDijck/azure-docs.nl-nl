---
title: Azure-Services die ondersteuning bieden voor beheerde identiteiten voor Azure-resources
description: Lijst met services die ondersteuning bieden voor beheerde identiteiten voor Azure-resources en Azure AD-verificatie
services: active-directory
author: daveba
ms.author: daveba
ms.date: 11/28/2018
ms.topic: conceptual
ms.service: active-directory
ms.component: msi
manager: mtillman
ms.openlocfilehash: 3fdbac019849bc97e8d336b75f26a8fe0a05c449
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53713111"
---
# <a name="services-that-support-managed-identities-for-azure-resources"></a>Services die ondersteuning bieden voor beheerde identiteiten voor Azure-resources

Beheerde identiteiten voor Azure-resources biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory. Met behulp van een beheerde identiteit, kunt u verifiëren met een service die ondersteuning biedt voor Azure AD-verificatie zonder referenties in uw code. We zijn bezig met integratie van beheerde identiteiten voor Azure-resources en Azure AD-verificatie in Azure. Controleer regelmatig of er updates.

> [!NOTE]
> Beheerde identiteiten voor Azure-resources is de nieuwe naam voor de service die eerder de naam Managed Service Identity (MSI) had.

## <a name="azure-services-that-support-managed-identities-for-azure-resources"></a>Azure-services die beheerde identiteiten voor Azure-resources ondersteunen

De volgende Azure-services bieden ondersteuning voor beheerde identiteiten voor Azure-resources:

| Service | Het systeem toegewezen status | status van de gebruiker toegewezen| Configureren | Een token verkrijgen |
| ------- | ------ | ---- | --------- | ----------- |
| Azure Virtual Machines | Beschikbaar | Preview | [Azure Portal](qs-configure-portal-windows-vm.md)<br>[PowerShell](qs-configure-powershell-windows-vm.md)<br>[Azure-CLI](qs-configure-cli-windows-vm.md)<br>[Azure Resource Manager-sjablonen](qs-configure-template-windows-vm.md)<br>[REST](qs-configure-rest-vm.md) | [REST](how-to-use-vm-token.md#get-a-token-using-http)<br>[.NET](how-to-use-vm-token.md#get-a-token-using-c)<br>[Bash/Curl](how-to-use-vm-token.md#get-a-token-using-curl)<br>[Go](how-to-use-vm-token.md#get-a-token-using-go)<br>[PowerShell](how-to-use-vm-token.md#get-a-token-using-azure-powershell) |
| Virtual Machine Scale Sets | Beschikbaar | Preview | [Azure Portal](qs-configure-portal-windows-vmss.md)<br>[PowerShell](qs-configure-powershell-windows-vmss.md)<br>[Azure-CLI](qs-configure-cli-windows-vmss.md)<br>[Azure Resource Manager-sjablonen](qs-configure-template-windows-vmss.md)<br>[REST](qs-configure-rest-vmss.md) | [REST](how-to-use-vm-token.md#get-a-token-using-http)<br>[.NET](how-to-use-vm-token.md#get-a-token-using-c)<br>[Bash/Curl](how-to-use-vm-token.md#get-a-token-using-curl)<br>[Go](how-to-use-vm-token.md#get-a-token-using-go)<br>[PowerShell](how-to-use-vm-token.md#get-a-token-using-azure-powershell)
| Azure App Service | Windows: Beschikbaar <br> Linux: Preview | Preview | [Azure Portal](/azure/app-service/overview-managed-identity#using-the-azure-portal)<br>[Azure-CLI](/azure/app-service/overview-managed-identity#using-the-azure-cli)<br>[Azure PowerShell](/azure/app-service/overview-managed-identity#using-azure-powershell)<br>[Azure Resource Manager-sjabloon](/azure/app-service/overview-managed-identity#using-an-azure-resource-manager-template) | [REST](/azure/app-service/overview-managed-identity#using-the-rest-protocol)<br>[.NET](/azure/app-service/overview-managed-identity#asal)<br>[JavaScript](/azure/app-service/overview-managed-identity#token-js)<br>[PowerShell](/azure/app-service/overview-managed-identity#token-powershell)  |
| Azure Functions | Beschikbaar | Preview | [Azure Portal](/azure/app-service/overview-managed-identity#using-the-azure-portal)<br>[Azure-CLI](/azure/app-service/overview-managed-identity#using-the-azure-cli)<br>[Azure PowerShell](/azure/app-service/overview-managed-identity#using-azure-powershell)<br>[Azure Resource Manager-sjabloon](/azure/app-service/overview-managed-identity#using-an-azure-resource-manager-template) | [REST](/azure/app-service/overview-managed-identity#using-the-rest-protocol)<br>[.NET](/azure/app-service/overview-managed-identity#asal)<br>[JavaScript](/azure/app-service/overview-managed-identity#token-js)<br>[PowerShell](/azure/app-service/overview-managed-identity#token-powershell) |
| Azure Logic Apps | Beschikbaar | Niet beschikbaar | [Azure Portal](/azure/logic-apps/create-managed-service-identity#azure-portal)<br>[Azure Resource Manager-sjabloon](/azure/app-service/overview-managed-identity#deployment-template) |  |
| Azure Data Factory V2 | Beschikbaar | Niet beschikbaar | [Azure Portal](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity)<br>[PowerShell](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-powershell)<br>[REST](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-rest-api)<br>[SDK](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-sdk) |
| Azure API Management | Beschikbaar | Niet beschikbaar | [Azure Resource Manager-sjabloon](/azure/api-management/api-management-howto-use-managed-service-identity) |
| Azure Container Instances | Linux: Preview<br>Windows: Niet beschikbaar | Linux: Preview<br>Windows: Niet beschikbaar | [Azure-CLI](~/articles/container-instances/container-instances-managed-identity.md)<br>[Azure Resource Manager-sjabloon](~/articles/container-instances/container-instances-managed-identity.md#enable-managed-identity-using-resource-manager-template)<br>[YAML](~/articles/container-instances/container-instances-managed-identity.md#enable-managed-identity-using-yaml-file) |  |


## <a name="azure-services-that-support-azure-ad-authentication"></a>Azure-services die ondersteuning voor Azure AD-verificatie

De volgende services ondersteuning bieden voor Azure AD-verificatie, en zijn getest met clientservices die gebruikmaken van beheerde identiteiten voor Azure-resources.

| Service | Resource-id | Status | Date | Toegang toewijzen |
| ------- | ----------- | ------ | ---- | ------------- |
| Azure Resource Manager | `https://management.azure.com/` | Beschikbaar | September 2017 | [Azure Portal](howto-assign-access-portal.md) <br>[PowerShell](howto-assign-access-powershell.md) <br>[Azure-CLI](howto-assign-access-CLI.md) <br>[Azure Resource Manager-sjabloon](../../role-based-access-control/role-assignments-template.md) |
| Azure Key Vault | `https://vault.azure.net` | Beschikbaar | September 2017 | |
| Azure Data Lake | `https://datalake.azure.net/` | Beschikbaar | September 2017 | |
| Azure SQL | `https://database.windows.net/` | Beschikbaar | Oktober 2017 | |
| Azure Event Hubs | `https://eventhubs.azure.net` | Preview | December 2017 | |
| Azure Service Bus | `https://servicebus.azure.net` | Preview | December 2017 | |
| Azure Storage | `https://storage.azure.com/` | Preview | Mei 2018 | |