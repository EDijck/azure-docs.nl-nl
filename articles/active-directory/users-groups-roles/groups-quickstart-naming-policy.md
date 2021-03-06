---
title: Quickstart voor naambeleid voor Office 365-groepen in Microsoft Azure Active Directory | Microsoft Docs
description: Legt uit hoe u nieuwe gebruikers toevoegt of bestaande gebruikers verwijdert in Azure Active Directory
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: quickstart
ms.date: 01/31/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: d4105fa17041c7cefd1387d1ee50c177b8c55fc9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60471209"
---
# <a name="quickstart-naming-policy-for-groups-in-azure-active-directory"></a>Snelstartgids: Naamgevingsbeleid voor groepen in Azure Active Directory

In deze snelstart stelt u naambeleid in uw Azure Active Directory (Azure AD)-tenant in voor Office 365-groepen die door gebruikers zijn gemaakt. Hierdoor kunt u de groepen van de tenant sorteren en doorzoeken. U kunt het naambeleid onder meer gebruiken voor:

* Het communiceren van de functie van een groep, het lidmaatschap, de geografische regio of de maker van een groep.
* Het classificeren van groepen in het adresboek.
* Zorgen dat het gebruik van bepaalde woorden voor groepsnamen en -aliassen wordt geblokkeerd.

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="install-powershell-cmdlets"></a>PowerShell-cmdlets installeren

Verwijder een oudere versie van Azure Active Directory PowerShell voor Graph Module voor Windows PowerShell en installeer [Azure Active Directory PowerShell for Graph - Public Preview Release 2.0.0.137](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137) (Azure Active Directory PowerShell voor Graph - Release 2.0.0.137 voor openbare preview) voordat u de PowerShell-opdrachten uitvoert. 

1. Open de Windows PowerShell-app als beheerder.
2. Verwijder eventuele oudere versies van AzureADPreview.
  

   ```powershell
   Uninstall-Module AzureADPreview
   ```

3. Installeer de nieuwste versie van AzureADPreview.
  

   ```powershell
   Install-Module AzureADPreview
   ```

   Als u wordt gevraagd een niet-vertrouwde opslagplaats te openen, typt u **Y**. Het kan enkele minuten duren voordat de nieuwe module is geïnstalleerd.

## <a name="set-up-naming-policy"></a>Naambeleid instellen

### <a name="step-1-sign-in-using-powershell-cmdlets"></a>Stap 1: Aanmelden met behulp van PowerShell-cmdlets

1. Open de Windows PowerShell-app. U hebt geen verhoogde bevoegdheden nodig.

2. Voer de volgende opdrachten uit als voorbereiding op het uitvoeren van de cmdlets.
  

   ```powershell
   Import-Module AzureADPreview
   Connect-AzureAD
   ```

   In het scherm **Sign in to your Account** dat verschijnt, voert u uw beheerdersaccount en wachtwoord in om verbinding te maken met uw service. Selecteer vervolgens **Aanmelden**.

3. Volg de stappen in [Azure Active Directory-cmdlets voor het configureren van groepsinstellingen](groups-settings-cmdlets.md) om groepsinstellingen voor deze tenant te maken.

### <a name="step-2-view-the-current-settings"></a>Stap 2: Huidige instellingen weergeven

1. Bekijk de instellingen voor het huidige naambeleid.
  

   ```powershell
   $Setting = Get-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | Where-Object -Property DisplayName -Value "Group.Unified" -EQ).id
   ```

  
2. Geef de instellingen voor de huidige groep weer.
  

   ```powershell
   $Setting.Values
   ```

  

### <a name="step-3-set-the-naming-policy-and-any-custom-blocked-words"></a>Stap 3: Naamgevingsbeleid en eventuele aangepaste, geblokkeerde woorden instellen

1. Stel de voor- en achtervoegsels van de groepsnaam in in Azure AD PowerShell. [GroupName] moet in de instelling worden opgenomen om de functie goed te laten werken.
  

   ```powershell
   $Setting["PrefixSuffixNamingRequirement"] =“GRP_[GroupName]_[Department]"
   ```

  
2. Stel de aangepaste, geblokkeerde woorden in die u wilt verbieden. In het volgende voorbeeld wordt getoond hoe u uw eigen aangepaste woorden kunt toevoegen.
  

   ```powershell
   $Setting["CustomBlockedWordsList"]=“Payroll,CEO,HR"
   ```

  
3. Sla de instellingen voor het nieuwe beleid op zodat het van kracht wordt, zoals in het volgende voorbeeld.
  

   ```powershell
   Set-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | Where-Object -Property DisplayName -Value "Group.Unified" -EQ).id -DirectorySetting $Setting
   ```

  
Dat is alles. U hebt het naambeleid ingesteld en uw aangepaste, geblokkeerde woorden toegevoegd.

## <a name="clean-up-resources"></a>Resources opschonen

1. Wis de voor- en achtervoegsels van de groepsnaam in Azure AD PowerShell.
  

   ```powershell
   $Setting["PrefixSuffixNamingRequirement"] =""
   ```

  
2. Maak de aangepaste lijst met geblokkeerde woorden leeg.
  

   ```powershell
   $Setting["CustomBlockedWordsList"]=""
   ```

  
3. Sla de instellingen op.
  

   ```powershell
   Set-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | Where-Object -Property DisplayName -Value "Group.Unified" -EQ).id -DirectorySetting $Setting
   ```

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u geleerd hoe u PowerShell-cmdlets kunt gebruiken om het naambeleid voor uw Azure AD-tenant in te stellen.

Voor meer informatie over technische beperkingen, het toevoegen van een lijst met aangepaste, geblokkeerde woorden of de ervaringen van eindgebruikers met Office 365-apps, gaat u verder naar het volgende artikel.
> [!div class="nextstepaction"]
> [Naambeleid: alle details](groups-naming-policy.md)