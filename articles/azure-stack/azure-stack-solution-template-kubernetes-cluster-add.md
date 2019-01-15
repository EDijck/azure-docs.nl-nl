---
title: Kubernetes toevoegen aan de Azure Stack Marketplace | Microsoft Docs
description: Informatie over het toevoegen van Kubernetes op Azure Stack Marketplace.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2019
ms.author: mabrigg
ms.reviewer: waltero
ms.openlocfilehash: e89575323b87ba28ef4f062da098fea4f0e27035
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/14/2019
ms.locfileid: "54264051"
---
# <a name="add-kubernetes-to-the-azure-stack-marketplace"></a>Kubernetes op Azure Stack Marketplace toevoegen

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

> [!note]  
> Kubernetes in Azure Stack is in preview.

Als een Marketplace-item kunt u Kubernetes aanbieden aan uw gebruikers. Uw gebruikers kunnen Kubernetes implementeren in een enkele, gecoördineerde bewerking.

Het volgende artikel bekijken met behulp van een Azure Resource Manager-sjabloon te implementeren en de resources voor een zelfstandige Kubernetes-cluster inrichten. De Kubernetes-Cluster Marketplace-item 0.3.0 vereist Azure Stack-versie 1808. Voordat u begint, controleert u uw Azure Stack en de instellingen van de globale Azure-tenant. De vereiste gegevens verzamelen over uw Azure Stack. Benodigde resources toevoegen aan uw tenant en de Azure Stack Marketplace. Het cluster is afhankelijk van een Ubuntu-server, aangepaste scripts en de Kubernetes-items in de marketplace.

## <a name="create-a-plan-an-offer-and-a-subscription"></a>Een plan, een aanbieding en een abonnement maken

Maak een plan, een aanbieding en een abonnement voor het Kubernetes-Marketplace-item. U kunt ook gebruik van een bestaand plan en aanbieding.

1. Aanmelden bij de [-beheerportal.](https://adminportal.local.azurestack.external)

1. Maak een plan als het basisplan. Zie voor instructies [een plan maken in Azure Stack](azure-stack-create-plan.md).

1. Maak een aanbieding. Zie voor instructies [een aanbieding maken in Azure Stack](azure-stack-create-offer.md).

1. Selecteer **biedt**, en zoek de aanbieding die u hebt gemaakt.

1. Selecteer **overzicht** in de blade van de aanbieding.

1. Selecteer **in een statuswijziging**. Selecteer **Openbaar**.

1. Selecteer **+ een resource maken** > **aanbiedingen en abonnementen** > **abonnement** naar een nieuw abonnement maken.

    a. Voer een **weergavenaam**.

    b. Voer een **gebruiker**. Gebruik Azure AD-account dat is gekoppeld aan uw tenant.

    c. **Beschrijving van de provider**

    d. Stel de **Directory-tenant** bij Azure AD-tenant voor uw Azure Stack. 

    e. Selecteer **bieden**. Selecteer de naam van de aanbieding die u hebt gemaakt. Noteer de abonnement-ID.

## <a name="add-an-ubuntu-server-image"></a>De installatiekopie van een Ubuntu-server toevoegen

De volgende Ubuntu-Server-installatiekopie toevoegen aan de Marketplace:

1. Aanmelden bij de [beheerportal](https://adminportal.local.azurestack.external).

1. Selecteer **alle services**, en klik vervolgens onder de **beheer** categorie, selecteer **Marketplace management**.

1. Selecteer **+ toevoegen vanuit Azure**.

1. Voer `UbuntuServer` in.

1. Selecteer de nieuwste versie van de server. De volledige versie controleren en ervoor te zorgen dat u de nieuwste versie hebt:
    - **Publisher**: Canonical
    - **Bieden**: UbuntuServer
    - **Versie**: 16.04.201806120 (of hoger)
    - **SKU**: 16.04-LTS

1. Selecteer **downloaden.**

## <a name="add-a-custom-script-for-linux"></a>Toevoegen van een aangepast script voor Linux

De Kubernetes uit de Marketplace toevoegen:

1. Open de [beheerportal](https://adminportal.local.azurestack.external).

1. Selecteer **alle services** en klik vervolgens onder de **beheer** categorie, selecteer **Marketplace Management**.

1. Selecteer **+ toevoegen vanuit Azure**.

1. Voer `Custom Script for Linux` in.

1. Het script met het volgende profiel te selecteren:
    - **Bieden**: Aangepast Script voor Linux 2.0
    - **Versie**: 2.0.6 (of hoger)
    - **Publisher**: Microsoft Corp

    > [!Note]  
    > Meer dan één versie van het aangepaste Script voor Linux kan worden vermeld. U moet de meest recente versie van het item toevoegen.

1. Selecteer **downloaden.**


## <a name="add-kubernetes-to-the-marketplace"></a>Kubernetes toevoegen aan de marketplace

1. Open de [beheerportal](https://adminportal.local.azurestack.external).

1. Selecteer **alle services** en klik vervolgens onder de **beheer** categorie, selecteer **Marketplace Management**.

1. Selecteer **+ toevoegen vanuit Azure**.

1. Voer `Kubernetes` in.

1. Selecteer `Kubernetes Cluster`.

1. Selecteer **downloaden.**

    > [!note]  
    > Het duurt vijf minuten voor de marketplace-item wordt weergegeven in de Marketplace.

    ![Kubernetes](user/media/azure-stack-solution-template-kubernetes-deploy/marketplaceitem.png)

## <a name="update-or-remove-the-kubernetes"></a>Bijwerken of verwijderen van de Kubernetes 

Tijdens het bijwerken van het Kubernetes-item, moet u het item dat in de Marketplace te verwijderen. Vervolgens kunt u de instructies in dit artikel voor het toevoegen van de Kubernetes aan de marketplace volgen.

De Kubernetes-item verwijderen:

1. Verbinding maken met Azure Stack met PowerShell als een operator. Zie voor instructies, [verbinding maken met Azure Stack met PowerShell als operator](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-configure-admin).

2. Het huidige item van de Kubernetes-Cluster niet vinden in de galerie.

    ```PowerShell  
    Get-AzsGalleryItem | Select Name
    ```
    
3. Noteer de naam van het huidige item, zoals `Microsoft.AzureStackKubernetesCluster.0.3.0`

4. Gebruik de volgende PowerShell-cmdlet om het item te verwijderen:

    ```PowerShell  
    $Itemname="Microsoft.AzureStackKubernetesCluster.0.3.0"

    Remove-AzsGalleryItem -Name $Itemname
    ```

## <a name="next-steps"></a>Volgende stappen

[Een Kubernetes met Azure Stack implementeren](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-solution-template-kubernetes-deploy)

[Overzicht van services in Azure Stack-aanbieding](azure-stack-offer-services-overview.md)
