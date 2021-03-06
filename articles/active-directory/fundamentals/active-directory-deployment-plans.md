---
title: Implementatieplannen voor - Azure Active Directory | Microsoft Docs
description: End-to-end-richtlijnen over het implementeren van de vele mogelijkheden van Azure Active Directory.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 02/08/2019
ms.author: lizross
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8f04fdc84c0bed26bc6d5f16e5cef14c6855d02b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60249764"
---
# <a name="azure-active-directory-deployment-plans"></a>Azure Active Directory-implementatieplannen
Bent u op zoek naar end-to-end-richtlijnen over het implementeren van sommige mogelijkheden van Azure Active Directory (Azure AD)? De volgende implementatieplannen geven richtlijnen bij de vereiste bedrijfswaarde, planningsoverwegingen, het ontwerp en de operationele procedures voor de implementatie van enkele algemene mogelijkheden van Azure AD. 

In de documenten vindt u e-mailsjablonen, diagrammen met systeemarchitectuur, algemene testscenario's en meer. 

Uw feedback over de documenten wordt op prijs gesteld. Vul deze korte [enquête](https://aka.ms/deploymentplanfeedback) in verband met de documenten in. 

|Scenario |Description |
|-|-|
|[Multi-Factor Authentication](../authentication/howto-mfa-getstarted.md)|Azure Multi-Factor Authentication (MFA) is een Microsoft-oplossing voor verificatie in twee stappen. Met behulp van door een beheerder goedgekeurde verificatiemethoden, helpt Azure MFA met het beveiligen van gegevens en toepassingen, terwijl tegelijkertijd aan de behoefte aan een eenvoudige aanmeldingsprocedure wordt voldaan.|
|Voorwaardelijke toegang [downloadbare plan](https://aka.ms/CADPDownload) of [online-abonnement](https://aka.ms/deploymentplans/ca)|Met voorwaardelijke toegang kunt u geautomatiseerde beslissingen voor toegangsbeheer implementeren voor wie toegang heeft tot uw cloud-apps op basis van voorwaarden.|
|[Self-service voor wachtwoord opnieuw instellen](https://aka.ms/SSPRDPDownload)|De selfservice voor het opnieuw instellen van wachtwoorden helpt uw gebruikers overal en altijd hun wachtwoord opnieuw in te stellen zonder dat er een beheerder aan te pas komt.|
|[Privileged Identity Management](../privileged-identity-management/pim-deployment-plan.md)|Azure AD Privileged Identity Management (PIM) helpt u bij het beheren van bevoegde beheerdersrollen in Azure AD, Azure-resources en andere Microsoft Online Services. PIM voorziet in oplossingen zoals just-in-time-toegang, aanvraag goedkeuringswerkstromen en volledig geïntegreerde toegang beoordelingen, zodat u identificeren kunt, ontdekken en te voorkomen dat schadelijke activiteiten van bevoorrechte rollen in realtime.|
|[Eenmalige aanmelding](https://aka.ms/SSODPDownload)|Met eenmalige aanmelding kunt u alle apps en resources openen die u voor uw bedrijf nodig hebt. U hoeft u slechts eenmaal aan te melden met één gebruikersaccount. Nadat u zich hebt aangemeld, gaat u vanuit Microsoft Office naar SalesForce en naar Box zonder dat u zich een tweede keer hoeft te verifiëren (bijvoorbeeld door een wachtwoord te typen).|
|[Naadloze eenmalige aanmelding](https://aka.ms/SeamlessSSODPDownload)|Met Naadloze eenmalige aanmelding van Azure Active Directory (Naadloze SSO van Azure AD) worden gebruikers aangemeld als hun bedrijfsapparaten zijn verbonden met net bedrijfsnetwerk. Als deze functie is ingeschakeld, hoeft de gebruiker geen wachtwoord in te typen om zich bij Azure AD aan te melden. Zelfs de gebruikersnaam hoeft niet te worden ingetypt. Deze functie biedt een gebruiker eenvoudig toegang tot cloudtoepassingen zonder dat er aanvullende on-premises onderdelen nodig zijn.|
|[Toegangsvenster](https://aka.ms/AccessPanelDPDownload)|Biedt uw gebruikers een eenvoudige hub om te ontdekken en toegang tot al hun toepassingen. Ze productiever met selfservice mogelijkheden, zoals de mogelijkheid om aan te vragen van toegang tot nieuwe apps en -groepen, inschakelen of beheren van toegang tot deze bronnen namens anderen.|
|AD FS voor synchronisatie van Wachtwoordhashes [downloadbare plan](https://aka.ms/ADFSTOPHSDPDownload) of [online-abonnement](https://aka.ms/deploymentplans/adfs2phs)|Met wachtwoordhash-synchronisatie worden hashes van gebruikerswachtwoorden vanuit on-premises Active Directory gesynchroniseerd met Azure AD, waarbij Azure AD gebruikers verifieert zonder interactie met de on-premises Active Directory|
|AD FS doorgeven via verificatie [downloadbare plan](https://aka.ms/ADFSTOPTADPDownload) of [online-abonnement](https://aka.ms/deploymentplans/adfs2pta)|Met Azure AD Pass Through-verificatie kunnen gebruikers zich met hetzelfde wachtwoord aanmelden bij zowel on-premises als cloudtoepassingen. Deze functie biedt een betere gebruikerservaring (een wachtwoord minder te onthouden) en het vermindert de kosten voor de IT-helpdesk omdat gebruikers minder vaak een wachtwoord vergeten. Als iemand zich aanmeldt bij Azure AD, worden met deze functie de wachtwoorden rechtstreeks gecontroleerd tegen de on-premises Active Directory.|
|[Azure AD-toepassingsproxy](https://aka.ms/deploymentplans/appproxy)|De huidige werknemer wil overal, op elke plek en op elk apparaat productief kunnen zijn. Hij of zij wil op het eigen apparaat werken, of dat nu een tablet, telefoon of laptop is. En een werknemer verwacht dat hij toegang heeft tot al zijn toepassingen, zowel SaaS-apps in cloud als on-premises, zakelijke apps. Voorheen was bij het toegang bieden tot on-premises toepassingen een VPN (Virtual Private Network) of een DMZ (gedemilitariseerde zone) nodig. Dit is niet alleen ingewikkeld en moeilijk te beveiligen, maar het instellen en beheren ervan is duur. Er is een betere manier. - Azure AD-toepassingsproxy|
|[Inrichten van gebruikers](https://aka.ms/UserProvisioningDPDownload)|Met Azure AD kunt u het maken, onderhouden en verwijderen van gebruikers-id's in cloud(SaaS)-toepassingen als Dropbox, Salesforce, ServiceNow en andere, automatiseren.|
|[Inrichten van WorkDay gebaseerde inkomende gebruikers](https://aka.ms/WorkdayDeploymentPlan)|WorkDay gebaseerde inkomende inrichten van gebruikers naar Active Directory maakt u een basis voor lopende identiteitsbestuur, en verbetert de kwaliteit van zakelijke processen die afhankelijk van de gezaghebbende identiteitsgegevens zijn. Met deze functie kunt u naadloos beheren de levenscyclus van de identiteit van werknemers en voorwaardelijke werknemers door regels die binding-Mover-Leaver processen (zoals nieuwe werven, beëindigen, overdracht) aan IT inrichting acties (zoals het maken, inschakelen toewijzen, te configureren Uitschakelen, accounts verwijderen).|
