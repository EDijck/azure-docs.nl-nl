---
title: Een toegangscontrole starten met Azure AD-Toegangsbeoordelingen | Microsoft Docs
description: Leer hoe u een toegangscontrole starten met behulp van Azure Active Directory-Toegangsbeoordelingen.
services: active-directory
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 07/16/2018
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1a6a137796c24f97364b044484d2be739ae5412d
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56171168"
---
# <a name="start-an-access-review-with-azure-ad-access-reviews"></a>Een toegangscontrole starten met Azure AD-Toegangsbeoordelingen

Azure Active Directory (Azure AD) vereenvoudigt de manier waarop ondernemingen toegang tot toepassingen en leden van groepen beheren in Azure AD en andere Microsoft Online Services met een functie met de naam van de toegang beoordeelt. U kunt mogelijk geen e-mail ontvangen van Microsoft waarmee u wordt gevraagd te toegang beoordelen voor leden van een groep of gebruikers met toegang tot een toepassing. 

## <a name="open-an-access-review"></a>Een toegangsbeoordeling openen

De in behandeling zijnde om beoordelingen te bekijken, klikt u op de koppeling voor de controle-toegang in het e-mailbericht. Vanaf augustus 2018, hebben de e-mailmeldingen voor Azure AD-rollen voor het ontwerp van een bijgewerkte. Hieronder ziet u een voorbeeld van de e-mailbericht wordt verzonden wanneer een gebruiker is uitgenodigd om te worden van een revisor. 

![Controleer toegang tot e-mail](./media/perform-access-review/new-ar-email.png)

Als u het e-mailbericht niet hebt, kunt u de toegangsbeoordelingen vinden door de volgende stappen:

1. Meld u bij de [Azure AD-toegangspaneel](https://myapps.microsoft.com).

2. Selecteer het symbool gebruiker in de rechterbovenhoek van de pagina, die uw organisatie naam en het standaard weergegeven. Als meer dan één organisatie wordt weergegeven, selecteert u de organisatie die een toegangsbeoordeling aangevraagd.

3. Als een tegel met het label **Toegangsbeoordelingen** is aan de rechterkant van de pagina, selecteer het. Als de tegel niet zichtbaar is, er zijn geen toegangsbeoordelingen om uit te voeren voor deze organisatie en is er op dit moment geen actie nodig.

## <a name="fill-out-an-access-review"></a>Vul een toegangsbeoordeling

Wanneer u een toegangscontrole uit de lijst selecteert, ziet u de namen van gebruikers die moeten worden gecontroleerd. Ziet u mogelijk slechts één naam van uw eigen--als de aanvraag is uw eigen toegang controleren.

Voor elke rij in de lijst, kunt u beslissen of goedkeuren of weigeren van toegang van de gebruiker. Selecteer de rij en kiest u of u wilt goedkeuren of weigeren. (Als u de gebruiker niet weet, kunt u aangeven dat te.)

De revisor mogelijk dat u een reden voor het goedkeuren van blijvende toegang of lidmaatschap van groep opgeven.

## <a name="next-steps"></a>Volgende stappen

Een geweigerde gebruikerstoegang wordt niet onmiddellijk verwijderd. Het kan worden verwijderd wanneer de beoordeling is voltooid of wanneer een beheerder de controle wordt gestopt. Als u wilt wijzigen van het antwoord en een eerder geweigerde gebruiker goedkeuren of weigeren van een eerder goedgekeurde gebruikers, selecteert u de rij, opnieuw instellen van het antwoord en selecteert u een nieuw antwoord. U kunt deze stap kunt doen, totdat de toegangsbeoordeling is voltooid.



