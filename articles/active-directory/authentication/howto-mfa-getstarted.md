---
title: Krijgen de slag met Azure MFA in de cloud
description: Microsoft Azure multi-factor Authentication aan de slag met voorwaardelijke toegang
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 09/01/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: cbfade56ff12aa719927d496606a6ce4b4fe5a38
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56167147"
---
# <a name="deploy-cloud-based-azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication op basis van cloud implementeren

Aan de slag met Azure multi-factor Authentication (Azure MFA) is een eenvoudig proces.

Voordat u begint, zorg er dan voor dat u hebt de volgende vereisten:

* Een globale beheerdersaccount in uw Azure AD-tenant. Als u nodig hebt voor hulp bij deze stap is voltooid, raadpleegt u ons artikel [aan de slag met Azure AD](../get-started-azure-ad.md).
* Juiste licenties zijn toegewezen aan gebruikers. Als u meer informatie, Zie het artikel [over het verkrijgen van Azure multi-factor Authentication](concept-mfa-licensing.md).

## <a name="choose-how-to-enable"></a>Kiezen hoe u om in te schakelen

**Door beleid voor voorwaardelijke toegang ingeschakeld** -met deze methode wordt besproken in dit artikel. Het is de meest flexibele manier om in te schakelen van verificatie in twee stappen voor uw gebruikers. Inschakelen met behulp van alleen beleid voor voorwaardelijke toegang werkt voor Azure MFA in de cloud en is een premium-functie van Azure AD.

**Ingeschakeld met Azure AD Identity Protection** -met deze methode maakt gebruik van de Azure AD Identity Protection-beleid voor gebruikersrisico's om te vereisen van verificatie in twee stappen alleen op basis van aanmeldingsrisico voor alle cloudtoepassingen. Deze methode is een Azure Active Directory P2-licentie vereist. Meer informatie over deze methode kan worden gevonden [het configureren van het beleid voor gebruikersrisico's](../identity-protection/howto-user-risk-policy.md).

**Ingeschakeld door het veranderen van gebruikersstatus** -dit is de traditionele methode voor het vereisen van verificatie in twee stappen. Het werkt met beide Azure MFA in de cloud en Azure MFA-Server. Met deze methode vereist dat gebruikers om uit te voeren van verificatie in twee stappen **telkens** ze zich aanmelden en vervangt u beleid voor voorwaardelijke toegang. Meer informatie over deze methode kan worden gevonden [hoe u verificatie in twee stappen vereist voor een gebruiker](howto-mfa-userstates.md).

> [!Note]
> Meer informatie over licenties en prijzen vindt u op de [Azure AD](https://azure.microsoft.com/pricing/details/active-directory/
) en [multi-Factor Authentication](https://azure.microsoft.com/pricing/details/multi-factor-authentication/) prijspagina's.

## <a name="choose-authentication-methods"></a>Verificatiemethoden kiezen

Schakel ten minste één verificatiemethode voor uw gebruikers op basis van de vereisten van uw organisatie. We vinden dat wanneer dit is ingeschakeld voor gebruikers van de Microsoft Authenticator-app de beste gebruikerservaring biedt. Als u nodig hebt om te begrijpen welke methoden beschikbaar zijn en hoe u deze instelt, Zie het artikel [wat verificatiemethoden zijn](concept-authentication-methods.md).

## <a name="get-users-to-enroll"></a>Gebruikers te registreren

Nadat u het beleid voor voorwaardelijke toegang inschakelt, worden gebruikers gedwongen om in te schrijven de volgende keer dat ze een app die is beveiligd met het beleid gebruiken. Als u een beleid MFA vereisen voor alle gebruikers op alle cloud-apps wilt inschakelen, kan deze actie kan ervoor zorgen dat problemen voor uw gebruikers en de helpdesk. De aanbeveling is om te vragen van gebruikers voor het registreren van tevoren met behulp van de portal voor wachtwoordregistratie op verificatiemethoden [ https://aka.ms/mfasetup ](https://aka.ms/mfasetup). Veel organisaties vinden dat het maken van posters, tabel-kaarten en e-mailberichten bijdraagt aan een station acceptatie.

## <a name="enable-multi-factor-authentication-with-conditional-access"></a>Multi-factor Authentication met voorwaardelijke toegang inschakelen

Aanmelden bij de [Azure-portal](https://portal.azure.com) met behulp van een globale beheerdersaccount.

### <a name="choose-verification-options"></a>Verificatie-opties kiezen

Voordat u Azure multi-factor Authentication inschakelt, moet uw organisatie te bepalen welke verificatie-opties kunnen. Ten behoeve van deze oefening hebt inschakelen u aanroep van telefoon- en SMS-bericht naar telefoon als ze zijn algemene opties voor dat de meeste mogen gebruiken. Meer informatie over verificatiemethoden en hun gebruik, vindt u in het artikel [wat verificatiemethoden zijn?](concept-authentication-methods.md)

1. Blader naar **Azure Active Directory**, **gebruikers**, **multi-Factor Authentication**.

   ![Toegang tot de multi-factor Authentication-portal van Azure AD-gebruikers-blade in Azure portal](media/howto-mfa-getstarted/users-mfa.png)

1. Blader in het nieuwe tabblad dat geopend naar **service-instellingen**.
1. Onder **verificatieopties**, schakel alle selectievakjes in voor de methoden die beschikbaar zijn voor gebruikers.

   ![Verificatiemethoden configureren in het tabblad instellingen van multi-factor Authentication-service](media/howto-mfa-getstarted/mfa-servicesettings-verificationoptions.png)

4. Klik op **Opslaan**.
5. Sluit de **service-instellingen** tabblad.

### <a name="create-conditional-access-policy"></a>Beleid voor voorwaardelijke toegang maken

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met behulp van een globale beheerdersaccount.
1. Blader naar **Azure Active Directory**, **Voorwaardelijke toegang**.
1. Selecteer **nieuw beleid**.
1. Geef een beschrijvende naam voor uw beleid.
1. Onder **gebruikers en groepen**:
   * Op de **opnemen** tabblad de **alle gebruikers** keuzerondje
   * AANBEVOLEN: Op de **uitsluiten** tabblad, schakel het selectievakje voor **gebruikers en groepen** en kiest u een groep moet worden gebruikt voor uitsluitingen wanneer gebruikers geen toegang tot hun verificatiemethoden.
   * Klik op **Gereed**.
1. Onder **Cloud-apps**, selecteer de **alle cloud-apps** keuzerondje.
   * (OPTIONEEL): Op de **uitsluiten** tabblad, kiest u cloud-apps die uw organisatie geen MFA voor vereist.
   * Klik op **Gereed**.
1. Onder **voorwaarden** sectie:
   * (OPTIONEEL): Als u Azure Identity Protection hebt ingeschakeld, kunt u aanmeldingsrisico beoordelen als onderdeel van het beleid.
   * (OPTIONEEL): Als u de vertrouwde locaties hebt geconfigureerd of benoemde locaties, u opgeven kunt als u wilt opnemen of uitsluiten van deze locaties van het beleid.
1. Onder **verlenen**, zorg ervoor dat de **toegang verlenen** keuzerondje is geselecteerd.
    * Schakel het selectievakje voor **meervoudige verificatie vereisen**.
    * Klik op **Selecteren**.
1. Overslaan de **sessie** sectie.
1. Stel de **beleid inschakelen** overzet naar **op**.
1. Klik op **Create**.

![Maken van een beleid voor voorwaardelijke toegang voor MFA inschakelen voor gebruikers van de Azure portal in de testfasegroep](media/howto-mfa-getstarted/conditionalaccess-newpolicy.png)

### <a name="test-azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication testen

Om te bevestigen dat uw beleid voor voorwaardelijke toegang werkt, test u aanmelden aan een resource die geen MFA vereist en vervolgens naar de Azure-portal die MFA vereist.

1. Open een nieuw browservenster in de InPrivate- of incognitomodus en blader naar [https://account.activedirectory.windowsazure.com](https://account.activedirectory.windowsazure.com).
   * Meld u aan met de testgebruiker gemaakt als onderdeel van de sectie vereisten van dit artikel en houd er rekening mee dat deze niet vraagt u om MFA te voltooien.
   * Sluit het browservenster.
2. Open een nieuw browservenster in de InPrivate- of incognitomodus en blader naar [https://portal.azure.com](https://portal.azure.com).
   * Meld u aan met de test gemaakt als onderdeel van de sectie vereisten van dit artikel en merk op dat u zou nu moeten van de gebruiker vereist om te registreren voor en het gebruik van Azure multi-factor Authentication.
   * Sluit het browservenster.

## <a name="next-steps"></a>Volgende stappen

Gefeliciteerd, u de Azure multi-factor Authentication in de cloud hebt ingesteld.

Waarom is een gebruiker wordt gevraagd of niet wordt gevraagd om uit te voeren van MFA? Zie de sectie [rapport van Azure AD-aanmeldingen in de rapporten in de Azure multi-factor Authentication-document](howto-mfa-reporting.md#azure-ad-sign-ins-report).

Zie het artikel voor het configureren van extra instellingen zoals goedgekeurde IP-adressen, aangepaste spraakberichten en Fraudewaarschuwingen [Azure multi-factor Authentication configureren instellingen](howto-mfa-mfasettings.md).

Informatie over het beheren van gebruikersinstellingen voor Azure multi-factor Authentication kan worden gevonden in het artikel [gebruikersinstellingen met Azure multi-factor Authentication in de cloud beheren](howto-mfa-userdevicesettings.md).

[Geconvergeerde registratie voor meervoudige verificatie en Azure AD Self-service voor wachtwoord opnieuw instellen inschakelen](concept-registration-mfa-sspr-converged.md).
