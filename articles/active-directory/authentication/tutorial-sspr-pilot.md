---
title: Een Azure AD SSPR-pilot inschakelen
description: In deze zelfstudie schakelt u de selfservice voor wachtwoordherstel van Azure AD in voor een groep pilotgebruikers
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: tutorial
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: a6e1095cc2c5937fa5de762f91a9830161b8d2a5
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2019
ms.locfileid: "59362120"
---
# <a name="tutorial-complete-an-azure-ad-self-service-password-reset-pilot-roll-out"></a>Zelfstudie: Een pilot van een Azure AD-selfservice voor wachtwoordherstel uitrollen

In deze zelfstudie rolt u in uw organisatie een pilot uit van de Azure AD-selfservice voor wachtwoordherstel (SSPR) en test u deze met behulp van een niet-beheerdersaccount.

Het is belangrijk dat het testen van de selfservice voor wachtwoordherstel wordt uitgevoerd met niet-beheerdersaccounts. Microsoft beheert het wachtwoordherstelbeleid voor beheerdersaccounts en vereist het gebruik van sterkere verificatiemethoden. Dit beleid staat het gebruik van beveiligingsvragen en -antwoorden niet toe en vereist het gebruik van twee methoden voor het opnieuw instellen.

> [!div class="checklist"]
> * Selfservice voor wachtwoordherstel inschakelen
> * SSPR testen als een gebruiker

## <a name="prerequisites"></a>Vereisten

* Een globale-beheerdersaccount

## <a name="enable-self-service-password-reset"></a>Selfservice voor wachtwoordherstel inschakelen

1. Meld u aan bij [Azure Portal](https://portal.azure.com) met een globale-beheerdersaccount.
1. Blader naar **Azure Active Directory** en selecteer **Wachtwoord opnieuw instellen**.
1. Start een pilotgroep door de selfservice voor wachtwoordherstel in te schakelen voor een subset van gebruikers in uw organisatie.
   * Op de pagina **Eigenschappen** kiest u **Geselecteerd** onder **Selfservice voor wachtwoord opnieuw instellen is ingeschakeld** en kiest u een pilotgroep.
      * Alleen leden van de specifieke Azure Active Directory-groep die u kiest, kunnen de SSPR-functionaliteit gebruiken. Het is raadzaam om een groep gebruikers te definiëren en deze instelling te gebruiken wanneer u deze functionaliteit implementeert voor het testen van een concept. Het nesten van beveiligingsgroepen wordt hier ondersteund.
      * Zorg ervoor dat de gebruikers in de groep die u hebt gekozen de juiste licentie hebben.
   * Klik op **Opslaan**.
1. Op de pagina **Verificatiemethoden**
   * Stel de **aantal methoden dat is vereist om opnieuw in te** naar **1**
   * Kies welke **Methoden voor gebruikers** uw organisatie wil toestaan. Schakel de selectievakjes om in te schakelen voor deze zelfstudie **e**, **mobiele telefoon**, **telefoon (werk)**, **mobiele app-meldingen (preview)** en  **Mobiele app-code (preview)**.
   * Klik op **Opslaan**.
1. Op de pagina **Registratie**
   * Selecteer **Ja** bij **Vereisen dat gebruiker zich bij aanmelding registreren**.
   * Stel **Aantal dagen waarna gebruikers wordt gevraagd om de verificatiegegevens opnieuw te bevestigen** in op **180**.
   * Klik op **Opslaan**.
1. Op de pagina **Meldingen**
   * Stel **Gebruikers een melding tonen over het opnieuw instellen van hun wachtwoord** in op **Ja**.
   * Stel **Alle beheerders waarschuwen wanneer andere beheerders hun wachtwoord opnieuw instellen** in op **Ja**.
1. Op de pagina **Aanpassing**
   * Microsoft raadt aan **Helpdeskkoppeling aanpassen** in te stellen op **Ja** en in het veld **Aangepast e-mailadres of aangepaste URL van de helpdesk** een e-mailadres of webpagina-URL op te geven waar uw gebruikers meer hulp van uw organisatie kunnen krijgen.
   * Voor deze zelfstudie laten we **Aangepast e-mailadres of aangepaste URL van de helpdesk** op **Nee** staan.

Selfservice voor wachtwoordherstel is nu geconfigureerd voor cloudgebruikers in uw pilotgroep.

## <a name="test-sspr-as-a-user"></a>SSPR testen als een gebruiker

Test de selfservice voor wachtwoordherstel met behulp van een testgebruiker die geen beheerder is en die lid is van uw pilotgroep. **Houd er rekening mee dat als u een account gebruikt waaraan beheerdersrollen zijn toegewezen, de verificatiemethoden en het aantal daarvan anders kunnen zijn dan wat u hebt geselecteerd, aangezien Microsoft het beheerdersbeleid beheert.**

1. Open een nieuw browservenster om InPrivate- of incognitomodus.
1. Gebruik een testgebruiker om u met behulp van de registratieportal op [https://aka.ms/ssprsetup](https://aka.ms/ssprsetup) te registreren voor het zelf opnieuw instellen van een wachtwoord.
1. Ga als dezelfde testgebruiker naar de selfserviceportal voor wachtwoordherstel [​​ https://aka.ms/sspr](https://aka.ms/sspr) en probeer uw wachtwoord opnieuw in te stellen met behulp van de informatie die u in de vorige stap hebt verstrekt.
1. U zou uw wachtwoord met succes opnieuw moeten kunnen instellen.

## <a name="clean-up-resources"></a>Resources opschonen

Als u besluit dat u niet langer gebruik wilt maken van de functionaliteit die u als onderdeel van deze zelfstudie hebt geconfigureerd, moet u de volgende wijziging aanbrengen.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Blader naar **Azure Active Directory** en selecteer **Wachtwoord opnieuw instellen**.
1. Op de pagina **Eigenschappen** kiest u **Geen** onder **Selfservice voor wachtwoord opnieuw instellen is ingeschakeld**.
1. Klik op **Opslaan**.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u de selfservice voor wachtwoordherstel ingeschakeld. Ga door naar de volgende zelfstudie om te zien hoe een on-premises Active Directory Domain Services-infrastructuur kan worden geïntegreerd met de selfservice voor wachtwoordherstel.

> [!div class="nextstepaction"]
> [On-premises write-backintegratie van selfservice voor wachtwoordherstel inschakelen](tutorial-enable-writeback.md)
