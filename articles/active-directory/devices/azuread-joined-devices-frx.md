---
title: Een nieuw Windows 10-apparaat aan Azure AD koppelen tijdens de eerste uitvoering | Microsoft Docs
description: Een onderwerp waarin wordt uitgelegd hoe gebruikers Azure AD-koppeling kunnen instellen tijdens de eerste uitvoering.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.component: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 08/25/2018
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 3693115d72d9928bea0ca8bbe4fae5858e0f3cb7
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54446422"
---
# <a name="tutorial-join-a-new-windows-10-device-with-azure-ad-during-a-first-run"></a>Zelfstudie: Een nieuw Windows 10-apparaat aan Azure AD koppelen tijdens de eerste uitvoering

Met apparaatbeheer in Azure Active Directory (Azure AD) kunt u ervoor zorgen dat uw gebruikers toegang tot uw bronnen hebben vanaf apparaten die voldoen aan uw normen voor beveiliging en naleving. Zie [Inleiding tot apparaatbeheer in Azure Active Directory](overview.md) voor meer informatie.

Met Windows 10 kunt u een nieuw apparaat aan Azure AD koppelen tijdens de eerste uitvoering.  
Zo kunt u apparaten in krimpverpakking naar uw werknemers of leerlingen/studenten distribueren.

Als Windows 10 Professional of Windows 10 Enterprise op een apparaat is geïnstalleerd, wordt de uitvoering standaard ingesteld op het installatieproces voor apparaten die eigendom van het bedrijf zijn.

In de *Out-Of-Box Experience* van Windows wordt koppeling van een on-premises AD-domein (Active Directory) niet ondersteund. Als u van plan bent een computer aan een AD-domein te koppelen, moet u tijdens de installatie de koppeling **Windows instellen met een lokaal account** selecteren. Vervolgens kunt u het domein koppelen via de instellingen op uw computer.
 
In deze zelfstudie leert u hoe u een apparaat aan Azure AD kunt koppelen tijdens de eerste uitvoering:
 > [!div class="checklist"]
> * Vereisten
> * Een apparaat koppelen
> * Verificatie

## <a name="prerequisites"></a>Vereisten

Om een Windows 10-apparaat te koppelen, moet de apparaatregistratieservice zodanig zijn geconfigureerd dat u apparaten kunt registreren. U moet niet alleen toestemming hebben om apparaten te koppelen in uw Azure AD-tenant, maar er moeten ook minder apparaten zijn geregistreerd dan het geconfigureerde maximum. Zie [Apparaatinstellingen configureren](device-management-azure-portal.md#configure-device-settings) voor meer informatie.

Als uw tenant is gefedereerd, MOET uw identiteitsprovider bovendien het WS-Fed en WS-Trust gebruikersnaam/wachtwoord-eindpunt ondersteunen. Dit kan versie 1.3 of 2005 zijn. Deze protocolondersteuning is vereist om het apparaat aan Azure AD te koppelen en om u met een wachtwoord bij het apparaat aan te melden.

## <a name="joining-a-device"></a>Een apparaat koppelen

**Ga als volgt te werk om een Windows 10-apparaat aan Azure AD te koppelen tijdens de eerste uitvoering:**


1. Wanneer u uw nieuwe apparaat aanzet en het installatieproces start, moet u het bericht **Voorbereiding** zien. Volg de prompts om uw apparaat in te stellen.

2. Begin door uw regio en taal aan te passen. Accepteer vervolgens de Licentievoorwaarden voor Microsoft-software.
 
    ![Aanpassen voor uw regio](./media/azuread-joined-devices-frx/01.png)

3. Selecteer het netwerk dat u wilt gebruiken om verbinding te maken met internet.

4. Klik op **Dit apparaat is van mijn organisatie**. 

    ![Scherm Van wie is deze pc?](./media/azuread-joined-devices-frx/02.png)

5. Voer de referenties in die uw organisatie aan u heeft verstrekt, en klik vervolgens op **Aanmelden**.

    ![Scherm Aanmelden](./media/azuread-joined-devices-frx/03.png)

6. Uw apparaat zoekt een overeenkomende tenant in Azure AD. Als u zich in een federatief domein bevindt, wordt u omgeleid naar uw on-premises STS-server (Secure Token Service), bijvoorbeeld Active Directory Federation Services (AD FS).

7. Als u een gebruiker in een niet-federatief domein bent, voert u uw referenties rechtstreeks op de Azure AD-gehoste pagina in. 

8. U wordt gevraagd om meervoudige verificatie. 
 
9. Azure AD controleert of inschrijving voor mobiel apparaatbeheer vereist is.

10. Windows registreert het apparaat in de organisatiedirectory in Azure AD en schrijft het in voor mobiel apparaatbeheer, indien van toepassing.

11. Als u:
    - Een beheerde gebruiker bent, leidt Windows u naar het bureaublad via het automatische aanmeldingsproces.

    - Een federatieve gebruiker bent, wordt u naar het Windows-aanmeldingsscherm geleid, waar u uw referenties moet invoeren.

## <a name="verification"></a>Verificatie

Om te verifiëren of een apparaat aan uw Azure AD is gekoppeld, bekijkt u het dialoogvenster **Werk- of schoolaccount openen** op uw Windows-apparaat. Het dialoogvenster moet aangeven dat u bent verbonden met uw Azure AD-directory.

![Werk- of schoolaccount openen](./media/azuread-joined-devices-frx/13.png)


## <a name="next-steps"></a>Volgende stappen

- Zie [Inleiding tot apparaatbeheer in Azure Active Directory](overview.md) voor meer informatie.

- Zie [Apparaten beheren met behulp van de Azure-portal](device-management-azure-portal.md) voor meer informatie over het beheren van apparaten in de Azure AD-portal.
