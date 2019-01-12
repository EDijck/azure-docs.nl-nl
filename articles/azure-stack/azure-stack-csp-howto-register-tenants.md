---
title: Toevoegen van tenants voor het gebruik en facturering met Azure Stack | Microsoft Docs
description: Een eindgebruiker toevoegen de vereiste stappen aan Azure Stack beheerd door een Cloud Service Provider (CSP).
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2019
ms.author: sethm
ms.reviewer: alfredop
ms.openlocfilehash: 5dbaa00b51f791bdf400ff4498b22952addddb24
ms.sourcegitcommit: f4b78e2c9962d3139a910a4d222d02cda1474440
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2019
ms.locfileid: "54246293"
---
# <a name="add-tenant-for-usage-and-billing-to-azure-stack"></a>Toevoegen van tenant voor gebruik en facturering met Azure Stack

*Van toepassing op: Azure Stack-geïntegreerde systemen*

In dit artikel wordt beschreven hoe u een eindgebruiker toevoegen aan een Azure Stack-implementatie beheerd door een Cloud Service Provider (CSP). Wanneer de nieuwe tenant maakt gebruik van resources, wordt in Azure Stack gebruik rapporteert aan haar CSP-abonnement.

CSP's bieden services vaak meerdere eindgebruikers (tenants) op de Azure Stack-implementatie. Tenants toe te voegen aan de Azure Stack-registratie zorgt ervoor dat elke tenant gebruik gerapporteerd en kosten in rekening op de bijbehorende CSP-abonnement gebracht. Als u niet de stappen in dit artikel uitvoert, wordt tenantgebruik verrekend met het abonnement dat is gebruikt in de registratie van Azure Stack. Voordat u een eindklant aan Azure Stack toevoegen kunt voor het bijhouden van gebruik en voor het beheren van hun tenant, moet u Azure Stack configureren als een CSP. Zie voor stappen en resources, [beheren gebruik en facturering voor Azure Stack als een Cloudserviceprovider](azure-stack-add-manage-billing-as-a-csp.md).

De volgende afbeelding ziet u de stappen die een CSP volgen om in te schakelen van een nieuwe klant om te gebruiken van Azure Stack, en moet voor het instellen van het gebruik bijhouden voor de klant. Door toe te voegen de eindklant, bent u ook resources in Azure Stack kan beheren. U hebt twee opties voor het beheren van hun resources:

1. U kunt de eindklant behouden en geef referenties op voor de lokale Azure Stack-abonnement voor de eindklant.  
2. De eindklant kunt werken met hun abonnement lokaal en de CSP toevoegen als gast met eigenaarsmachtigingen.  

## <a name="steps-to-add-an-end-customer"></a>Stappen voor het toevoegen van een eindklant

![Cloudserviceprovider instellen voor het bijhouden van gebruik en voor het beheren van de klant-account beëindigen](media/azure-stack-csp-enable-billing-usage-tracking/process-csp-enable-billing.png)

### <a name="create-a-new-customer-in-partner-center"></a>Maak een nieuwe klant in Partnercentrum

In de Partner Center, een nieuw Azure-abonnement voor de klant te maken. Zie voor instructies [toevoegen van een nieuwe klant](https://msdn.microsoft.com/partner-center/add-a-new-customer).

### <a name="create-an-azure-subscription-for-the-end-customer"></a>Een Azure-abonnement voor de eindklant maken

Nadat u een record van uw klant in Partner Center hebt gemaakt, kunt u deze abonnementen op producten in de catalogus verkopen. Zie voor instructies [maken, onderbreken of -abonnementen annuleren](https://msdn.microsoft.com/partner-center/create-a-new-subscription).

### <a name="create-a-guest-user-in-the-end-customer-directory"></a>Een gastgebruiker in de map van de klant end maken

Als de eindklant hun eigen account beheert, een gastgebruiker in de directory maken en deze gegevens worden verzonden. De eindgebruiker wordt vervolgens de Gast toevoegen en de Gast-machtiging voor het verhogen **eigenaar** voor de Azure Stack-CSP-account.

### <a name="update-the-registration-with-the-end-customer-subscription"></a>De registratie wordt bijgewerkt met het klantabonnement dat einde

Werk uw registratie met het nieuwe klantabonnement. Azure-rapporten van de klant gebruik van de Partner Center met behulp van de identiteit van de klant. Deze stap zorgt ervoor dat elke klant gebruik onder de afzonderlijke CSP-abonnement van de klant wordt gerapporteerd. Dit vereenvoudigt het gebruik van de gebruiker bijhouden en facturering.

> [!NOTE]  
> Als u wilt deze stap uitvoert, moet u hebben [geregistreerd Azure Stack](azure-stack-register.md).

1. Open Windows PowerShell met een opdrachtprompt en voer:  
    `Add-AzureRmAccount`
2. Typ uw Azure-referenties.
3. Voer in de PowerShell-sessie:

   ```powershell
   New-AzureRmResource -ResourceId "subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{customerSubscriptionId}" -ApiVersion 2017-06-01 -Properties <PSObject>
   ```

### <a name="new-azurermresource-powershell-parameters"></a>Nieuwe-AzureRmResource PowerShell-parameters

Het volgende gedeelte bevat de parameters voor de **New-AzureRmResource** cmdlet:

| Parameter | Description |
| --- | --- |
|registrationSubscriptionID | De Azure-abonnement dat is gebruikt voor de registratie van de Azure Stack.|
| customerSubscriptionID | De Azure-abonnement (niet Azure Stack) die horen bij de klant worden geregistreerd. Moet worden gemaakt in de CSP-aanbieding; Dit betekent via Partner Center in de praktijk. Als een klant meer dan één Azure Active Directory-tenant heeft, kan dit abonnement moet worden gemaakt in de tenant die wordt gebruikt voor aanmelding bij Azure Stack. De abonnements-ID van de klant moet kleine letters gebruiken. |
| resourceGroup | De resourcegroep in Azure waarop uw registratie zijn opgeslagen. |
| registrationName | De naam van de registratie van uw Azure Stack. Er is een object dat is opgeslagen in Azure. |
| Properties | Hiermee geeft u de eigenschappen voor de resource. Gebruik deze parameter om op te geven van de waarden van eigenschappen die specifiek voor het resourcetype zijn.

> [!NOTE]  
> Tenants moeten worden geregistreerd bij elke Azure-Stack die ze gebruiken. Als u twee Azure Stack-implementaties hebt en een tenant van beide worden gebruikt, moet u de eerste registraties van elke implementatie bijwerken met de tenantabonnement.

### <a name="onboard-tenant-to-azure-stack"></a>Onboarding-tenant om Azure Stack

Configureer Azure Stack ter ondersteuning van gebruikers van meerdere Azure AD-tenants, het gebruik van services in Azure Stack. Zie voor instructies [multitenancy in Azure Stack inschakelen](azure-stack-enable-multitenancy.md).

### <a name="create-a-local-resource-in-the-end-customer-tenant-in-azure-stack"></a>Een lokale bron maken in de tenant van de end-klant in Azure Stack

Nadat u de nieuwe klant hebt toegevoegd aan Azure Stack, of de klanttenant einde de Gast-account met de van eigenaarsbevoegdheden heeft ingeschakeld, controleert u of u kunt een resource maken in hun tenant. Bijvoorbeeld, kunnen ze [een Windows-machine maken met de Azure Stack-portal](user/azure-stack-quick-windows-portal.md).

## <a name="next-steps"></a>Volgende stappen

- De foutberichten worden weergegeven als ze worden geactiveerd in uw registratieproces Zie [Tenant-registratie foutberichten](azure-stack-csp-ref-infrastructure.md#usage-and-billing-error-codes).
- Zie voor meer informatie over het ophalen van informatie over het gebruik van de resource van Azure Stack, [gebruik en facturering in Azure Stack](azure-stack-billing-and-chargeback.md).
- Als u wilt controleren hoe een eindklant mogen toevoegen, als de CSP, als de beheerder van hun Azure Stack-tenant, Zie [inschakelen van een Cloudserviceprovider voor het beheren van uw Azure Stack-abonnement](user/azure-stack-csp-enable-billing-usage-tracking.md).
