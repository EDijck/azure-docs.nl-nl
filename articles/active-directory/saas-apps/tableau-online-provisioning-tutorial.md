---
title: 'Zelfstudie: Tableau Online configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en de inrichting ongedaan maken-gebruikersaccounts met Tableau Online.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd-msft
ms.assetid: 0be9c435-f9a1-484d-8059-e578d5797d8e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: v-wingf-msft
ms.collection: M365-identity-device-management
ms.openlocfilehash: f732eebd410a6b52a21a46925a29bf4676f7c8cb
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/08/2019
ms.locfileid: "59270783"
---
# <a name="tutorial-configure-tableau-online-for-automatic-user-provisioning"></a>Zelfstudie: Tableau Online configureren voor het automatisch inrichten van gebruikers

Het doel van deze zelfstudie is ter illustratie van de stappen om te worden uitgevoerd in Tableau Online en Azure Active Directory (Azure AD) naar Azure AD configureren voor automatisch inrichten en verwijdering van gebruikers en/of groepen met Tableau Online.

> [!NOTE]
> Deze zelfstudie beschrijft een connector die is gebaseerd op de Provisioning-Service van Azure AD-gebruiker. Zie voor belangrijke informatie over wat deze service biedt, hoe het werkt en veelgestelde vragen [automatiseren van gebruikersinrichting en -opheffing in SaaS-toepassingen met Azure Active Directory](../manage-apps/user-provisioning.md).

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al het volgende hebt:

*   Een Azure AD-tenant
*   Een [Tableau Online-tenant](https://www.tableau.com/)
*   Een gebruikersaccount in Tableau Online met beheerdersmachtigingen

> [!NOTE]
> De integratie wordt ingericht op Azure AD is afhankelijk van de [Tableau Online Rest-API](https://onlinehelp.tableau.com/current/api/rest_api/en-us/help.htm), die beschikbaar zijn voor ontwikkelaars Tableau Online is.

## <a name="adding-tableau-online-from-the-gallery"></a>Tableau Online toevoegen vanuit de galerie
Alvorens Tableau Online te configureren voor het automatisch gebruikers inrichten met Azure AD, moet u Tableau Online toevoegen uit de galerie met Azure AD aan uw lijst met beheerde SaaS-toepassingen.

**Als u wilt toevoegen van de Azure AD-toepassingsgalerie Tableau Online, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **Tableau Online**, selecteer **Tableau Online** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Tableau Online in de lijst met resultaten](common/search-new-app.png)

## <a name="assigning-users-to-tableau-online"></a>Gebruikers toewijzen aan Tableau Online

Azure Active Directory maakt gebruik van een concept genaamd "toewijzingen" om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het automatisch inrichten van gebruikers, worden alleen de gebruikers en/of groepen die '' aan een toepassing in Azure AD toegewezen zijn gesynchroniseerd.

Voordat u configureren en inschakelen van automatische inrichten van gebruikers, moet u bepalen welke gebruikers en/of groepen in Azure AD toegang hebben tot Tableau Online moeten. Wanneer u beslist, kunt u deze gebruikers en/of groepen toewijzen aan Tableau Online door de instructies hier:

*   [Een gebruiker of groep toewijzen aan een enterprise-app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-tableau-online"></a>Belangrijke tips voor het toewijzen van gebruikers tot Tableau Online

*   Het wordt aanbevolen dat één Azure AD-gebruiker is toegewezen aan Tableau Online voor het testen van de configuratie van de automatische gebruikersinrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

*   Bij het toewijzen van een gebruiker tot Tableau Online, moet u een geldige toepassingsspecifieke-rol (indien beschikbaar) in het dialoogvenster toewijzing selecteren. Gebruikers met de **standaardtoegang** rol worden uitgesloten van het inrichten.

## <a name="configuring-automatic-user-provisioning-to-tableau-online"></a>Automatisch gebruikers inrichten met Tableau Online configureren

Deze sectie helpt u bij de stappen voor het configureren van de Azure AD-inrichtingsservice als u wilt maken, bijwerken en uitschakelen van gebruikers en/of groepen in Tableau Online op basis van gebruiker en/of toewijzingen van groepen in Azure AD.

> [!TIP]
> U kunt er ook voor kiezen om in te schakelen op SAML gebaseerde eenmalige aanmelding voor Tableau Online, volgens de instructies vindt u in de [Tableau eenmalige aanmelding onlinezelfstudie](tableauonline-tutorial.md). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatisch gebruikers inrichten, hoewel deze twee functies een fraaie aanvulling in elkaar.

### <a name="to-configure-automatic-user-provisioning-for-tableau-online-in-azure-ad"></a>Het configureren van automatisch gebruikers inrichten voor Online Tableau in Azure AD:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) en selecteer **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **Tableau Online**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **Tableau Online**.

    ![De Online Tableau koppeling in de lijst met toepassingen](common/all-applications.png)

3. Selecteer de **Provisioning** tabblad.

    ![Tableau Online inrichten](./media/tableau-online-provisioning-tutorial/ProvisioningTab.png)

4. Stel de **Inrichtingsmodus** naar **automatische**.

    ![Tableau Online inrichten](./media/tableau-online-provisioning-tutorial/ProvisioningCredentials.png)

5. Onder de **beheerdersreferenties** sectie, voer de **domein**, **Admin Username**, **beheerderswachtwoord**, en **inhoud URL** van uw Tableau Online-account:

   * In de **domein** veld, vullen subdomein op basis van stap 6.

   * In de **Admin Username** veld, de gebruikersnaam van het beheerdersaccount dat op uw Clarizen Tenant vullen. Voorbeeld: admin@contoso.com.

   * In de **beheerderswachtwoord** veld, het wachtwoord van het beheerdersaccount dat overeenkomt met de gebruikersnaam van beheerder vullen.

   * In de **inhouds-URL** veld, vullen subdomein op basis van stap 6.

6. Na het aanmelden bij uw account met beheerdersrechten voor Online Tableau de waarden voor **domein** en **inhouds-URL** kunnen worden geëxtraheerd uit de URL van de beheerpagina.

    * De **domein** voor uw Tableau Online account van dit deel van de URL kan worden gekopieerd:

        ![Tableau Online inrichten](./media/tableau-online-provisioning-tutorial/DomainUrlPart.png)

    * De **inhouds-URL** voor uw Tableau Online account kan worden gekopieerd in deze sectie en is een waarde die tijdens de installatie van de account is gedefinieerd. In dit voorbeeld is de waarde 'contoso':

        ![Tableau Online inrichten](./media/tableau-online-provisioning-tutorial/ContentUrlPart.png)

        > [!NOTE]
        > Uw **domein** kan afwijken van de hieronder.

7. Bij het invullen van de velden die in stap 5 wordt weergegeven, klikt u op **testverbinding** om te controleren of Azure AD, kan verbinding maken met Tableau Online. Als de verbinding is mislukt, zorg ervoor dat uw Tableau Online-account beheerdersmachtigingen heeft en probeer het opnieuw.

    ![Tableau Online inrichten](./media/tableau-online-provisioning-tutorial/TestConnection.png)

8. In de **e-mailmelding** en voer het e-mailadres van een persoon of groep die u moet de inrichting fout ontvangen en schakel het selectievakje in **een e-mailmelding verzenden wanneer een foutoptreedt**.

    ![Tableau Online inrichten](./media/tableau-online-provisioning-tutorial/EmailNotification.png)

9. Klik op **Opslaan**.

10. Onder de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory-gebruikers Tableau**.

    ![Tableau Online inrichten](./media/tableau-online-provisioning-tutorial/UserMappings.png)

11. Controleer de kenmerken van de gebruiker die worden gesynchroniseerd vanuit Azure AD naar Online Tableau in de **kenmerk toewijzing** sectie. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in Tableau Online voor update-bewerkingen. Selecteer de **opslaan** knop wijzigingen doorvoeren.

    ![Tableau Online inrichten](./media/tableau-online-provisioning-tutorial/UserAttributeMapping.png)

12. Onder de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory-groepen aan Tableau**.

    ![Tableau Online inrichten](./media/tableau-online-provisioning-tutorial/GroupMappings.png)

13. Bekijk de groepskenmerken die worden gesynchroniseerd vanuit Azure AD naar Online Tableau in de **kenmerk toewijzing** sectie. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in Tableau Online voor update-bewerkingen. Selecteer de **opslaan** knop wijzigingen doorvoeren.

    ![Tableau Online inrichten](./media/tableau-online-provisioning-tutorial/GroupAttributeMapping.png)

14. Als u wilt configureren bereikfilters, raadpleegt u de volgende instructies in de [Scoping filter zelfstudie](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

15. Om in te schakelen in de Azure AD-inrichtingsservice voor Online Tableau, wijzigen de **Inrichtingsstatus** naar **op** in de **instellingen** sectie.

    ![Tableau Online inrichten](./media/tableau-online-provisioning-tutorial/ProvisioningStatus.png)

16. De gebruikers en/of groepen die u wilt inrichten met Tableau Online definiëren door het kiezen van de gewenste waarden in **bereik** in de **instellingen** sectie.

    ![Tableau Online inrichten](./media/tableau-online-provisioning-tutorial/ScopeSync.png)

17. Wanneer u klaar om in te richten bent, klikt u op **opslaan**.

    ![Tableau Online inrichten](./media/tableau-online-provisioning-tutorial/SaveProvisioning.png)

Met deze bewerking wordt gestart voor de initiële synchronisatie van alle gebruikers en/of groepen die zijn gedefinieerd **bereik** in de **instellingen** sectie. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de Azure AD-inrichtingsservice wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en koppelingen volgen voor het inrichten van rapport van de activiteit, die alle acties die worden uitgevoerd door de Azure AD-inrichtingsservice op Tableau Online beschrijft.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Het inrichten van gebruikersaccounts voor bedrijfs-Apps beheren](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over het controleren van Logboeken en rapporten over het inrichten van activiteit ophalen](../manage-apps/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/tableau-online-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/tableau-online-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/tableau-online-provisioning-tutorial/tutorial_general_03.png
