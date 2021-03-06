---
title: 'Zelfstudie: Azure Active Directory-integratie met Clear Review | Microsoft Docs'
description: Leer hoe u eenmalige aanmelding configureert tussen Azure Active Directory en Clear Review.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 8264159a-11a2-4a8c-8285-4efea0adac8c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/16/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: cf2576aa112d58e499f0c4a16bf8e9261114974b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60281438"
---
# <a name="tutorial-azure-active-directory-integration-with-clear-review"></a>Zelfstudie: Azure Active Directory-integratie met Clear Review

In deze zelfstudie leert u hoe u Clear Review kunt integreren met Azure Active Directory (Azure AD).
De integratie van Clear Review met Azure AD biedt de volgende voordelen:

* U kunt in Azure AD bepalen wie er toegang heeft tot Clear Review.
* U kunt instellen dat gebruikers automatisch met hun Azure AD-account worden aangemeld bij Clear Review (eenmalige aanmelding).
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Clear Review hebt u het volgende nodig:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/)
* Een abonnement op Clear Review waarvoor eenmalige aanmelding is ingeschakeld

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Clear Review ondersteunt door **SP en IDP** geïnitieerde eenmalige aanmelding

## <a name="adding-clear-review-from-the-gallery"></a>Clear Review toevoegen vanuit de galerie

Om de integratie van Clear Review in Azure AD te configureren, moet u Clear Review vanuit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Ga als volgt te werk om Clear Review vanuit de galerie toe te voegen:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ **Clear Review** in het zoekvak, selecteer **Clear Review** in het deelvenster met resultaten en klik vervolgens op de knop **Toevoegen** om de toepassing toe te voegen.

     ![Clear Review toevoegen vanuit de galerie](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In dit gedeelte configureert en test u eenmalige aanmelding van Azure AD met Clear Review op basis van een testgebruiker met de naam **Britta Simon**.
Eenmalige aanmelding werkt alleen als er een koppelingsrelatie tussen een Azure AD-gebruiker en de daaraan gerelateerde gebruiker in Clear Review tot stand is gebracht.

Als u eenmalige aanmelding van Azure AD met Clear Review wilt configureren en testen, moet u de volgende procedures uitvoeren:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Eenmalige aanmelding voor Clear Review configureren](#configure-clear-review-single-sign-on)**: de instellingen voor eenmalige aanmelding aan de clientzijde configureren.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Testgebruiker voor Clear Review maken](#create-clear-review-test-user)**: als u een tegenhanger van Britta Simon in Clear Review wilt hebben die is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voer de volgende stappen uit om eenmalige aanmelding van Azure AD te configureren voor Clear Review:

1. Ga in de [Azure-portal](https://portal.azure.com/) naar de pagina van de integratie van **Clear Review** en selecteer **Eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In het gedeelte **Standaard SAML-configuratie** voert u de volgende stappen uit als u de toepassing in de door **IDP** geïnitieerde modus wilt configureren:

    ![Gegevens van domein en URL's voor eenmalige aanmelding bij Clear Review](common/idp-intiated.png)

    a. In het tekstvak **Id** typt u een URL met het volgende patroon: `https://<customer name>.clearreview.com/sso/metadata/`

    b. In het tekstvak **Antwoord-URL** typt u een URL met de volgende notatie: `https://<customer name>.clearreview.com/sso/acs/`

5. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

    ![Gegevens van domein en URL's voor eenmalige aanmelding bij Clear Review](common/metadata-upload-additional-signon.png)

    In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<customer name>.clearreview.com`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke-id, de antwoord-URL en de aanmeldings-URL. Neem contact op met [het klantondersteuningsteam van Clear Review](https://clearreview.com/contact/) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

6. Schakel controle-toepassing wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de SAML-token kenmerken. In de volgende schermafbeelding ziet u de lijst met standaardkenmerken, waarbij **nameidentifier** is toegewezen aan **user.userprincipalname**. Schakel controle toepassing verwacht **nameidentifier** worden toegewezen met **user.mail**, dus u de kenmerktoewijzing van het bewerken moet door te klikken op **bewerken** pictogram en wijzig de kenmerk wordt toegewezen.

    ![image](common/edit-attribute.png)

7. Op de **gebruikerskenmerken en Claims** dialoogvenster, voer de volgende stappen uit:

    a. Klik op **bewerkingspictogram** aan de rechterkant van **naam id-waarde**.

    ![image](./media/clearreview-tutorial/attribute02.png)

    ![image](./media/clearreview-tutorial/attribute01.png)

    b. Uit de **bronkenmerk** in de lijst met de **user.mail** kenmerkwaarde voor die rij.

    c. Klik op **Opslaan**.

8. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

9. Kopieer in de sectie **Clear Review instellen** de juiste URL('s) op basis van uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-clear-review-single-sign-on"></a>Eenmalige aanmelding voor Clear Review configureren

1. Als u eenmalige aanmelding wilt configureren in **Clear Review**, opent u de portal van **Clear Review** met beheerdersreferenties.

2. Selecteer **Admin** in de linkernavigatie.

    ![De knop voor enkelvoudige aanmelding configureren](./media/clearreview-tutorial/tutorial_clearreview_app_admin1.png)

3. In de **integraties** sectie aan de onderkant van de pagina, klikt u op de **wijziging** knop aan de rechterkant van **instellingen voor eenmalige aanmelding**.

    ![De knop voor enkelvoudige aanmelding configureren](./media/clearreview-tutorial/tutorial_clearreview_app_admin2.png)

4. Voer de volgende stappen uit op de pagina **Single Sign-On Settings**.

    ![De knop voor enkelvoudige aanmelding configureren](./media/clearreview-tutorial/tutorial_clearreview_app_admin3.png)

    a. In de **URL-verlener** tekstvak, plak de waarde van **Azure AD-id** die u hebt gekopieerd vanuit Azure portal.

    b. Plak in het tekstvak **SAML2 Endpoint** de waarde van **Aanmeldings-URL** die u hebt gekopieerd uit de Azure-portal.  

    c. Plak in het tekstvak **SLO Endpoint** de waarde van **Afmeldings-URL** die u hebt gekopieerd uit de Azure-portal.  

    d. Open het gedownloade certificaat in Kladblok en plak de inhoud in het tekstvak **X.509 Certificate**.   

    e. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken 

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In de **gebruikersnaam** veldtype **brittasimon\@yourcompanydomain.extension**  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In dit gedeelte gaat u Britta Simon toestemming geven voor gebruik van eenmalige aanmelding met Azure door haar toegang te geven tot Clear Review.

1. Selecteer in de Azure-portal achtereenvolgens **Bedrijfstoepassingen**, **Alle toepassingen** en **Clear Review**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer **Clear Review** in de lijst met toepassingen.

    ![De koppeling naar Clear Review in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-clear-review-test-user"></a>Een testgebruiker voor Clear Review maken

In dit gedeelte gaat u in Clear Review een gebruiker maken met de naam Britta Simon. Neem contact op met het [ondersteuningsteam van Clear Review](https://clearreview.com/contact/) om de gebruikers toe te voegen aan het Clear Review-platform.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u in het toegangsvenster op de tegel van Clear Review klikt, wordt u automatisch aangemeld bij het exemplaar van Clear Review waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

