---
title: Minste bevoorrechte rollen door beheerder taak - Azure Active Directory delegeren | Microsoft Docs
description: Functies voor identiteit-taken in Azure Active Directory delegeren
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 01/31/2019
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 59c06ae83327683942885190e4b401617dc020f9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60468314"
---
# <a name="administrator-roles-by-admin-task-in-azure-active-directory"></a>Beheerdersrollen door admin-taak in Azure Active Directory

In dit artikel vindt u de informatie die nodig zijn voor het beperken van een gebruiker beheerdersmachtigingen minste bevoorrechte rollen in Azure Active Directory (Azure AD) toe te wijzen. Vindt u beheerderstaken geordend op het functiegebied en de minste bevoorrechte rol is vereist voor het uitvoeren van elke taak, samen met aanvullende niet - globale beheerdersrollen die de taak kunt uitvoeren.

## <a name="application-proxy"></a>Toepassingsproxy

Taak | Minste bevoorrechte rol | Aanvullende functies
---- | --------------------- | ----------------
Application proxy-app configureren | Toepassingsbeheerder | 
Eigenschappen voor connector configureren | Toepassingsbeheerder | 
Maken van registratie van toepassing wanneer de mogelijkheid is uitgeschakeld voor alle gebruikers | Toepassingsontwikkelaar | Beheerder van de cloudtoepassing, beheerder van de toepassing
Connectorgroep maken | Toepassingsbeheerder | 
Connectorgroep verwijderen | Toepassingsbeheerder | 
Toepassingsproxy uitschakelen | Toepassingsbeheerder | 
Service-connector downloaden | Toepassingsbeheerder | 
Alle configuratie lezen | Toepassingsbeheerder | 

## <a name="b2c"></a>B2C

Taak | Minste bevoorrechte rol | Aanvullende functies
---- | --------------------- | ----------------
Azure AD B2C-directory's maken | Alle niet-gastgebruikers ([Raadpleeg de documentatie bij](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | 
B2C-toepassingen maken | Globale beheerder | 
Zakelijke toepassingen maken | Cloudtoepassingsbeheerder | Toepassingsbeheerder
Maken, lezen, bijwerken en verwijderen van de B2C-beleid | Globale beheerder | 
Maken, lezen, bijwerken en verwijderen van de id-providers | Globale beheerder | 
Maken, lezen, bijwerken en verwijderen van de gebruikersstromen voor wachtwoord opnieuw instellen | Globale beheerder | 
Maken, lezen, bijwerken en verwijderen van de gebruikersstromen voor profielbewerking | Globale beheerder | 
Maken, lezen, bijwerken en verwijderen van aanmelding gebruikersstromen | Globale beheerder | 
Maken, lezen, bijwerken en meld u aan de gebruikersstroom verwijderen |Globale beheerder | 
Maken, lezen, bijwerken en verwijderen van de kenmerken van gebruiker | Globale beheerder | 
Maken, lezen, bijwerken en verwijderen van gebruikers | Globale beheerder ([Raadpleeg de documentatie bij](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-faqs))
Alle configuratie lezen | Globale beheerder | 
Lezen B2C-auditlogboeken | Globale beheerder ([Raadpleeg de documentatie bij](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-faqs)) | 

## <a name="company-branding"></a>Aangepaste huisstijl

Taak | Minste bevoorrechte rol | Aanvullende functies
---- | --------------------- | ----------------
Bedrijfshuisstijl configureren | Globale beheerder | 
Alle configuratie lezen | Adreslijstlezers | Standaard-gebruikersrol ([Raadpleeg de documentatie bij](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions))

## <a name="company-properties"></a>Eigenschappen van het bedrijf

Taak | Minste bevoorrechte rol | Aanvullende functies
---- | --------------------- | ----------------
Eigenschappen van het bedrijf configureren | Globale beheerder | 

## <a name="connect"></a>Verbinding maken

Taak | Minste bevoorrechte rol | Aanvullende functies
---- | --------------------- | ----------------
Passthrough-verificatie | Globale beheerder | 
Alle configuratie lezen | Globale beheerder | 
Naadloze single sign-on | Globale beheerder | 

## <a name="connect-health"></a>Connect Health

Taak | Minste bevoorrechte rol | Aanvullende functies
---- | --------------------- | ----------------
Toevoegen of verwijderen van services | Eigenaar ([Raadpleeg de documentatie bij](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-health-operations)) | 
Oplossingen voor synchronisatiefout toepassen | Inzender ([Raadpleeg de documentatie bij](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Eigenaar
Meldingen configureren | Inzender ([Raadpleeg de documentatie bij](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Eigenaar
Instellingen configureren | Eigenaar ([Raadpleeg de documentatie bij](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-health-operations)) | 
Synchronisatie-meldingen configureren | Inzender ([Raadpleeg de documentatie bij](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Eigenaar
Beveiligingsrapporten lezen ADFS | Beveiligingslezer | Inzender, eigenaar
Alle configuratie lezen | Lezer ([Raadpleeg de documentatie bij](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Inzender, eigenaar
Synchronisatiefouten lezen | Lezer ([Raadpleeg de documentatie bij](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Inzender, eigenaar
Alleen-Synchronisatieservices | Lezer ([Raadpleeg de documentatie bij](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Inzender, eigenaar
Metrische gegevens weergeven en waarschuwingen | Lezer ([Raadpleeg de documentatie bij](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Inzender, eigenaar
Metrische gegevens weergeven en waarschuwingen | Lezer ([Raadpleeg de documentatie bij](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Inzender, eigenaar
Metrische gegevens weergeven sync-service en waarschuwingen | Lezer ([Raadpleeg de documentatie bij](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Inzender, eigenaar


## <a name="custom-domain-names"></a>Aangepaste domeinnamen

Taak | Minste bevoorrechte rol | Aanvullende functies
---- | --------------------- | ----------------
Domeinen beheren | Globale beheerder | 
Alle configuratie lezen | Adreslijstlezers | Standaard-gebruikersrol ([Raadpleeg de documentatie bij](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions))

## <a name="domain-services"></a>Domain Services

Taak | Minste bevoorrechte rol | Aanvullende functies
---- | --------------------- | ----------------
Azure AD Domain Services-exemplaar maken | Globale beheerder | 
Alle Azure AD Domain Services-taken uitvoeren | Azure AD-DC-beheerdersgroep ([Raadpleeg de documentatie bij](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-admin-guide-administer-domain#administrative-tasks-you-can-perform-on-a-managed-domain)) | 
Alle configuratie lezen | Lezer voor Azure-abonnement met AD DS-service | 

## <a name="devices"></a>Apparaten

Taak | Minste bevoorrechte rol | Aanvullende functies
---- | --------------------- | ----------------
Apparaat uitschakelen | Cloudapparaatbeheerder | 
Apparaat inschakelen | Cloudapparaatbeheerder | 
Basisinformatie over de configuratie lezen | Standaard-gebruikersrol ([Raadpleeg de documentatie bij](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | 
Lezen BitLocker-sleutels | Beveiligingslezer | Wachtwoordbeheerder, beveiligingsbeheerder

## <a name="enterprise-applications"></a>Bedrijfstoepassingen

Taak | Minste bevoorrechte rol | Aanvullende functies
---- | --------------------- | ----------------
Toestemming geven voor gedelegeerde machtigingen | Cloudtoepassingsbeheerder | Toepassingsbeheerder
Instemming met de machtigingen van de toepassing niet met inbegrip van Microsoft Graph of Azure AD Graph | Cloudtoepassingsbeheerder | Toepassingsbeheerder
Instemming met de machtigingen van de toepassing Microsoft Graph of Azure AD Graph | Globale beheerder | 
Instemmen met toepassingen die toegang tot de eigen gegevens | Standaard-gebruikersrol ([Raadpleeg de documentatie bij](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | 
Enterprise-toepassing maken | Cloudtoepassingsbeheerder | Toepassingsbeheerder
Application Proxy beheren | Toepassingsbeheerder | 
Gebruikersinstellingen beheren | Globale beheerder | 
Leestoegang revisie van een groep of van een app | Beveiligingslezer | Beveiligingsbeheerder, Gebruikerbeheerder
Alle configuratie lezen | Standaard-gebruikersrol ([Raadpleeg de documentatie bij](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | 
Toewijzingen van de enterprise-toepassing bijwerken | De eigenaar van de Enterprise-toepassing ([Raadpleeg de documentatie bij](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Beheerder van de cloudtoepassing, beheerder van de toepassing
Enterprise-toepassingseigenaren bijwerken | De eigenaar van de Enterprise-toepassing ([Raadpleeg de documentatie bij](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Beheerder van de cloudtoepassing, beheerder van de toepassing
Enterprise-toepassingseigenschappen bijwerken | De eigenaar van de Enterprise-toepassing ([Raadpleeg de documentatie bij](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Beheerder van de cloudtoepassing, beheerder van de toepassing
Update inrichten van zakelijke toepassingen | De eigenaar van de Enterprise-toepassing ([Raadpleeg de documentatie bij](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Beheerder van de cloudtoepassing, beheerder van de toepassing
Enterprise-toepassing zelf bijwerken | De eigenaar van de Enterprise-toepassing ([Raadpleeg de documentatie bij](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Beheerder van de cloudtoepassing, beheerder van de toepassing
Bijwerken van eigenschappen voor eenmalige aanmelding | De eigenaar van de Enterprise-toepassing ([Raadpleeg de documentatie bij](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Beheerder van de cloudtoepassing, beheerder van de toepassing

## <a name="groups"></a>Groepen

Taak | Minste bevoorrechte rol | Aanvullende functies
---- | --------------------- | ----------------
Licentie toewijzen | Gebruikersbeheerder | 
Groep maken | Gebruikersbeheerder | 
Maken, bijwerken of verwijderen van de toegangsbeoordeling van een groep of van een app | Gebruikersbeheerder | 
Vervaldatum van de groep beheren | Gebruikersbeheerder | 
Groepsinstellingen beheren | Globale beheerder | 
Lezen van alle configuratie (met uitzondering van verborgen lidmaatschap) | Adreslijstlezers | Standaard-gebruikersrol ([Raadpleeg de documentatie bij](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions))
Lezen, verborgen lidmaatschap | Een lid van | Eigenaar van de groep, wachtwoordbeheerder, Exchange-beheerder, SharePoint-beheerder, beheerder van Teams, Gebruikerbeheerder
Lidmaatschap van groepen met verborgen lidmaatschap lezen | Helpdeskbeheerder | Gebruikerbeheerder, beheerder van de Teams
Licentie intrekken | Licentiebeheerder | Gebruikersbeheerder
Lidmaatschap bijwerken | Eigenaar van de groep ([Raadpleeg de documentatie bij](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Gebruikersbeheerder
Groepseigenaren bijwerken | Eigenaar van de groep ([Raadpleeg de documentatie bij](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Gebruikersbeheerder
Eigenschappen van de groep bijwerken | Eigenaar van de groep ([Raadpleeg de documentatie bij](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Gebruikersbeheerder

## <a name="identity-protection"></a>Identiteitsbeveiliging

Taak | Minste bevoorrechte rol | Aanvullende functies
---- | --------------------- | ----------------
Meldingen van waarschuwingen configureren| Beveiligingsbeheerder | 
Configureren en in- of uitschakelen van MFA-beleid| Beveiligingsbeheerder | 
Configureren en in- of uitschakelen van beleid voor aanmeldingsrisico| Beveiligingsbeheerder | 
Configureren en in- of uitschakelen van beleid voor gebruikersrisico 's | Beveiligingsbeheerder | 
Wekelijkse verwerkingen configureren | Beveiligingsbeheerder| 
Alle risicogebeurtenissen verwijderen | Beveiligingsbeheerder | 
Herstellen of te verwijderen van beveiligingsproblemen | Beveiligingsbeheerder | 
Alle configuratie lezen | Beveiligingslezer | 
Alle risicogebeurtenissen lezen | Beveiligingslezer | 
Beveiligingsproblemen lezen | Beveiligingslezer | 

## <a name="licenses"></a>Licenties

Taak | Minste bevoorrechte rol | Aanvullende functies
---- | --------------------- | ----------------
Licentie toewijzen | Licentiebeheerder | Gebruikersbeheerder
Alle configuratie lezen | Adreslijstlezers | Standaard-gebruikersrol ([Raadpleeg de documentatie bij](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions))
Licentie intrekken | Licentiebeheerder | Gebruikersbeheerder
Proberen of kopen van abonnement | Factureringsbeheerder | 


## <a name="monitoring---audit-logs"></a>Controle - auditlogboeken

Taak | Minste bevoorrechte rol | Aanvullende functies
---- | --------------------- | ----------------
Auditlogboeken lezen | Rapportenlezer | Beveiligingslezer, beveiligingsbeheerder

## <a name="monitoring---sign-ins"></a>Controle - aanmeldingen

Taak | Minste bevoorrechte rol | Aanvullende functies
---- | --------------------- | ----------------
Meld u in Logboeken te lezen | Rapportenlezer | Beveiligingslezer, beveiligingsbeheerder

## <a name="multi-factor-authentication"></a>Multi-Factor Authentication

Taak | Minste bevoorrechte rol | Aanvullende functies
---- | --------------------- | ----------------
Alle bestaande appwachtwoorden verwijderen die zijn gegenereerd door de geselecteerde gebruikers | Globale beheerder | 
MFA uitschakelen | Globale beheerder | 
MFA inschakelen | Globale beheerder | 
MFA service-instellingen beheren | Globale beheerder | 
Bepaalde gebruikers moeten opnieuw contactmethoden opgeven | Verificatiebeheerder | 
Meervoudige verificatie op alle geblokkeerde apparaten herstellen  | Verificatiebeheerder | 

## <a name="mfa-server"></a>MFA-server

Taak | Minste bevoorrechte rol | Aanvullende functies
---- | --------------------- | ----------------
Gebruikers blokkeren/deblokkeren | Globale beheerder | 
Accountvergrendeling configureren | Globale beheerder | 
Regels voor opslaan in cache configureren | Globale beheerder | 
Fraudewaarschuwing configureren | Globale beheerder
Meldingen configureren | Globale beheerder | 
Configureren van eenmalige bypass | Globale beheerder | 
Configureren van instellingen van telefoongesprekken | Globale beheerder | 
Providers configureren | Globale beheerder | 
Server-instellingen configureren | Globale beheerder | 
Lees het rapport computeractiviteit | Globale beheerder | 
Alle configuratie lezen | Globale beheerder | 
Status van de server lezen | Globale beheerder |  

## <a name="organizational-relationships"></a>Organisatierelaties

Taak | Minste bevoorrechte rol | Aanvullende functies
---- | --------------------- | ----------------
Id-providers beheren | Globale beheerder | 
Instellingen beheren | Globale beheerder | 
Gebruiksvoorwaarden beheren | Globale beheerder | 
Alle configuratie lezen | Globale beheerder | 

## <a name="password-reset"></a>Wachtwoord opnieuw instellen

Taak | Minste bevoorrechte rol | Aanvullende functies
---- | --------------------- | ----------------
Verificatiemethoden configureren | Globale beheerder |
Aanpassing configureren | Globale beheerder |
Melding configureren | Globale beheerder |
On-premises integratie configureren | Globale beheerder |
Eigenschappen van het wachtwoord opnieuw instellen configureren | Gebruikerbeheerder | Globale beheerder
Registratie configureren | Globale beheerder |
Alle configuratie lezen | Beveiligingsbeheerder | Gebruikerbeheerder |

## <a name="privileged-identity-management"></a>Privileged identity management

Taak | Minste bevoorrechte rol | Aanvullende functies
---- | --------------------- | ----------------
Gebruikers aan rollen toewijzen | Beheerder voor bevoorrechte rollen | 
Rolinstellingen configureren | Beheerder voor bevoorrechte rollen | 
Controle-activiteiten weergeven | Beveiligingslezer | 
Rollidmaatschappen weergeven | Beveiligingslezer | 

## <a name="roles-and-administrators"></a>Rollen en beheerders

Taak | Minste bevoorrechte rol | Aanvullende functies
---- | --------------------- | ----------------
Roltoewijzingen beheren | Beheerder voor bevoorrechte rollen | 
Leestoegang revisie van een Azure AD-rol  | Beveiligingslezer | Beveiligingsbeheerder, beheerder met bevoorrechte rol
Alle configuratie lezen | Standaard-gebruikersrol ([Raadpleeg de documentatie bij](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | 

## <a name="security---authentication-methods"></a>Beveiliging - verificatiemethoden

Taak | Minste bevoorrechte rol | Aanvullende functies
---- | --------------------- | ----------------
Verificatiemethoden configureren | Globale beheerder | 
Alle configuratie lezen | Globale beheerder | 

## <a name="security---conditional-access"></a>Beveiliging - voorwaardelijke toegang

Taak | Minste bevoorrechte rol | Aanvullende functies
---- | --------------------- | ----------------
Configureer MFA vertrouwde IP-adressen | Voorwaardelijke-toegangsbeheerder | 
Maken van aangepaste besturingselementen | Voorwaardelijke-toegangsbeheerder | Beveiligingsbeheerder
Benoemde locaties maken | Voorwaardelijke-toegangsbeheerder | Beveiligingsbeheerder
Beleidsregels maken | Voorwaardelijke-toegangsbeheerder | Beveiligingsbeheerder
Gebruiksvoorwaarden maken | Voorwaardelijke-toegangsbeheerder | Beveiligingsbeheerder
VPN-connectiviteit certificaat maken | Voorwaardelijke-toegangsbeheerder | Beveiligingsbeheerder
Klassiek beleid verwijderen | Voorwaardelijke-toegangsbeheerder | Beveiligingsbeheerder
Gebruiksvoorwaarden verwijderen | Voorwaardelijke-toegangsbeheerder | Beveiligingsbeheerder
VPN-connectiviteit certificaat verwijderen | Voorwaardelijke-toegangsbeheerder | Beveiligingsbeheerder
Klassiek beleid uitschakelen | Voorwaardelijke-toegangsbeheerder | Beveiligingsbeheerder
Aangepaste besturingselementen beheren | Voorwaardelijke-toegangsbeheerder | Beveiligingsbeheerder
Benoemde locaties beheren | Voorwaardelijke-toegangsbeheerder | Beveiligingsbeheerder
Gebruiksvoorwaarden beheren | Voorwaardelijke-toegangsbeheerder | Beveiligingsbeheerder
Alle configuratie lezen | Beveiligingslezer | Beveiligingsbeheerder
Lezen benoemde locaties | Beveiligingslezer | Voorwaardelijke toegang beheerder, beveiligingsbeheerder

## <a name="security---identity-security-score"></a>Beveiliging - identiteit beveiligingsscore

Taak | Minste bevoorrechte rol | Aanvullende functies | 
---- | --------------------- | ----------------
Alle configuratie lezen | Beveiligingslezer | Beveiligingsbeheerder
Beveiligingsscore lezen | Beveiligingslezer | Beveiligingsbeheerder
De gebeurtenisstatus van de bijwerken | Beveiligingsbeheerder | 

## <a name="security---risky-sign-ins"></a>Beveiliging - riskante aanmeldingen

Taak | Minste bevoorrechte rol | Aanvullende functies
---- | --------------------- | ----------------
Alle configuratie lezen | Beveiligingslezer | 
Riskante aanmeldingen lezen | Beveiligingslezer | 

## <a name="security---users-flagged-for-risk"></a>Beveiliging - gebruikers die zijn gemarkeerd voor risico 's

Taak | Minste bevoorrechte rol | Aanvullende functies
---- | --------------------- | ----------------
Alle gebeurtenissen sluiten | Beveiligingsbeheerder | 
Alle configuratie lezen | Beveiligingslezer | 
Gebruikers die zijn gemarkeerd voor risico's lezen | Beveiligingslezer | 

## <a name="users"></a>Gebruikers

Taak | Minste bevoorrechte rol | Aanvullende functies
---- | --------------------- | ----------------
Gebruiker toevoegen aan directory-rol | Beheerder voor bevoorrechte rollen | 
Gebruiker toevoegen aan groep | Gebruikersbeheerder | 
Licentie toewijzen | Licentiebeheerder | Gebruikersbeheerder
Gastgebruiker maken | Afzender van gastuitnodigingen | Gebruikersbeheerder
Gebruiker maken | Gebruikersbeheerder | 
Gebruikers verwijderen | Gebruikersbeheerder | 
Ongeldig vernieuwingstokens van bepaalde beheerders (Zie de documentatie) | Gebruikersbeheerder | 
Ongeldig vernieuwingstokens van niet-beheerders (Zie de documentatie) | Wachtwoordbeheerder | Gebruikersbeheerder
Ongeldig vernieuwingstokens van bevoegde beheerders (Zie de documentatie) | Globale beheerder | 
Basisinformatie over de configuratie lezen | Standaard-gebruikersrol ([Zie documentatie](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions) | 
Wachtwoord opnieuw instellen voor bepaalde beheerders (Zie de documentatie) | Gebruikersbeheerder | 
Wachtwoord opnieuw instellen van niet-beheerders (Zie de documentatie) | Wachtwoordbeheerder | Gebruikersbeheerder
Wachtwoord van bevoegde beheerders opnieuw instellen | Globale beheerder | 
Licentie intrekken | Licentiebeheerder | Gebruikersbeheerder
Alle eigenschappen, behalve de User Principal Name bijwerken | Gebruikersbeheerder | 
User Principal Name voor bepaalde beheerders (Zie de documentatie) bijwerken | Gebruikersbeheerder | 
Werk de eigenschap User Principal Name op bevoegde beheerders (Zie de documentatie) | Globale beheerder | 
Gebruikersinstellingen bijwerken | Globale beheerder | 


## <a name="support"></a>Ondersteuning

Taak | Minste bevoorrechte rol | Aanvullende functies
---- | --------------------- | ----------------
Ondersteuningsticket indienen | Servicebeheerder | Toepassingsbeheerder bent, facturering-beheerder, beheerder van de Cloudtoepassing, beheerder voor naleving, Dynamics 365-beheerder, beheerder van de Desktop-Analytics, Exchange-beheerder, wachtwoordbeheerder, Information Protection Intune-beheerder, Skype voor bedrijven-beheerder, Power BI-beheerder, beheerder met bevoorrechte verificatie, SharePoint-beheerder, beheerder van de communicatie Teams, Teams-beheerder, beheerder van de gebruiker,-beheerder Workplace Analytics-Administrator

## <a name="next-steps"></a>Volgende stappen

* [Het toewijzen of verwijderen van azure AD-beheerdersrollen](directory-manage-roles-portal.md)
* [Azure AD-beheerdersrollen verwijzen naar](directory-assign-admin-roles.md)
