---
title: Resultaten van de toegangsbeoordeling voor groepen of toepassingen in toegangsbeoordelingen - Azure Active Directory ophalen | Microsoft Docs
description: Informatie over het ophalen van resultaten van de toegangsbeoordeling voor leden van beveiligingsgroep of toegang tot toepassingen in Azure Active Directory-toegangsbeoordelingen.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 06/21/2018
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: eae4bafb3eefcee2785c784030d7be8dde66988e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60245828"
---
# <a name="retrieve-access-review-results-for-groups-or-applications-in-azure-ad-access-reviews"></a>Ophalen van de toegangsbeoordeling resultaten voor groepen of toepassingen in Azure AD-toegangsbeoordelingen

Beheerders kunnen Azure Active Directory (Azure AD) gebruiken om een [toegangsbeoordeling](create-access-review.md) te maken voor groepsleden of gebruikers die zijn toegewezen aan een toepassing.  Een gebruiker die zich in de **hoofdbeheerder**, **Gebruikerbeheerder**, **beveiligingsbeheerder** of **beveiligingslezer** rol kan ook Lees de resultaten van een toegangscontrole.  U gebruikers toewijzen aan een van deze rollen, kan een beheerder met bevoorrechte rol Azure AD PIM gebruiken zodat een gebruiker in aanmerking voor het activeren van de rol of globale beheerder permanent kunt [een gebruiker toewijzen aan de rol](../fundamentals/active-directory-users-assign-role-azure-portal.md).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="locating-an-access-review"></a>Een toegangsbeoordeling vinden

Als u weet welk programma de toegangsbeoordeling bevat, gaat u naar de pagina met toegangsbeoordelingen, selecteert u **Programma's**en selecteert u het programma dat het besturingselement voor toegangsbeoordeling bevat.  Selecteer vervolgens **Besturingselementen**, en selecteer het besturingselement voor toegangsbeoordeling. Als het programma veel besturingselementen bevat, kunt u filteren op besturingselementen van een specifiek type en deze sorteren op status. U kunt ook zoeken op de naam van het besturingselement voor toegangsbeoordeling of op de weergavenaam van de eigenaar die het heeft gemaakt. 

Als u niet weet welk programma de toegangsbeoordeling bevat, gaat u naar de pagina met toegangsbeoordelingen en selecteert u **Besturingselementen** .  U kunt filteren op besturingselementen van een specifiek type en deze sorteren op status, en u kunt ook zoeken op basis van de naam van het besturingselement voor toegangsbeoordeling of op de weergavenaam van de eigenaar die het heeft gemaakt. 

## <a name="retrieving-the-results-for-a-one-time-access-review"></a>De resultaten voor een eenmalige toegangsbeoordeling ophalen

Als het herhalingstype van beoordeling Eenmalig is, kunt u de voortgang van een actieve toegangsbeoordeling en de resultaten van een voltooide toegangsbeoordeling ophalen uit de sectie **Resultaten**.  U kunt de weergavenaam of user principal name van een gebruiker wiens toegang wordt beoordeeld typen om alleen de toegang van die gebruiker te bekijken.  Als u alle resultaten van een voltooide toegangsbeoordeling wilt ophalen, klikt u op de knop **Downloaden**.

## <a name="retrieving-the-results-for-multiple-instances-of-a-recurring-access-review"></a>De resultaten voor meerdere exemplaren van een terugkerende toegangsbeoordeling ophalen

Als u de voortgang van een terugkerende actieve toegangsbeoordeling wilt bekijken, klikt u op de sectie **Resultaten**.  U kunt de weergavenaam of user principal name typen van een gebruiker wiens toegang wordt beoordeeld.

Als u de resultaten van een voltooid exemplaar van een terugkerende toegangsbeoordeling wilt bekijken, selecteert u **Beoordelingsgeschiedenis** en selecteert u vervolgens het specifieke exemplaar in de lijst met voltooide toegangsbeoordelingen, op basis van de begin- en einddatum van het exemplaar.   De resultaten van dit exemplaar kunnen worden opgehaald uit de sectie **Resultaten**.  U kunt de weergavenaam of user principal name van een gebruiker wiens toegang wordt beoordeeld typen om alleen de toegang van die gebruiker te bekijken.  Als u alle resultaten van een voltooid exemplaar van een terugkerende toegangsbeoordeling wilt ophalen, klikt u op de knop **Downloaden**.


## <a name="removing-users-from-an-access-review"></a>Gebruikers verwijderen uit een toegangsbeoordeling

Standaard geldt dat een verwijderde gebruiker gedurende dertig dagen in Azure AD aanwezig blijft met de status Verwijderd. In deze periode kan de gebruiker eventueel door een beheerder worden teruggezet.  Na dertig dagen wordt die gebruiker definitief verwijderd.  Daarnaast kan een hoofdbeheerder via de Azure Active Directory-portal expliciet [een recentelijk verwijderde gebruiker definitief verwijderen](../fundamentals/active-directory-users-restore.md) voordat de dertig dagen zijn verstreken.  Als een gebruiker definitief is verwijderd, worden gegevens over die gebruiker vervolgens uit actieve toegangsbeoordelingen verwijderd.  Controle-informatie over verwijderde gebruikers blijft in het auditlogboek aanwezig.

## <a name="next-steps"></a>Volgende stappen

- [Gebruikerstoegang beheren met Azure AD-toegangsbeoordelingen](manage-user-access-with-access-reviews.md)
- [Gasttoegang beheren met Azure AD-toegangsbeoordelingen](manage-guest-access-with-access-reviews.md)
- [Programma's en controles voor Azure AD-toegangsbeoordelingen beheren](manage-programs-controls.md)
- [Een toegangsbeoordeling van groepen of toepassingen maken](create-access-review.md)
- [Een toegangsbeoordeling maken van gebruikers met een Azure AD-beheerderrol](../privileged-identity-management/pim-how-to-start-security-review.md)


