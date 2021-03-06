---
title: 'Zelfstudie: Azure Active Directory-integratie met Zscaler persoonlijke toegang (ZPA) | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Zscaler persoonlijke toegang (ZPA).
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 83711115-1c4f-4dd7-907b-3da24b37c89e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/29/2019
ms.author: jeedes
ms.openlocfilehash: 3213667e95c1e5cb68a849d6031db9629e5b273b
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2019
ms.locfileid: "64692670"
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-private-access-zpa"></a>Zelfstudie: Azure Active Directory-integratie met Zscaler persoonlijke toegang (ZPA)

In deze zelfstudie leert u hoe u Zscaler persoonlijke toegang (ZPA) integreren met Azure Active Directory (Azure AD).
Zscaler persoonlijke toegang (ZPA) integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang hebben naar Zscaler persoonlijke toegang (ZPA).
* U kunt uw gebruikers worden automatisch aangemeld op Zscaler persoonlijke toegang (ZPA) (Single Sign-On) inschakelen met hun Azure AD-accounts.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Zscaler persoonlijke toegang (ZPA), moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/)
* Eenmalige aanmelding Zscaler persoonlijke toegang (ZPA) ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor persoonlijke toegang (ZPA) Zscaler **SP** gestart door SSO

## <a name="adding-zscaler-private-access-zpa-from-the-gallery"></a>Zscaler persoonlijke toegang (ZPA) uit de galerie toe te voegen

Voor het configureren van de integratie van de Zscaler persoonlijke toegang (ZPA) in Azure AD, moet u Zscaler persoonlijke toegang (ZPA) uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Zscaler persoonlijke toegang (ZPA) uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **Zscaler persoonlijke toegang (ZPA)**, selecteer **Zscaler persoonlijke toegang (ZPA)** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![Zscaler persoonlijke toegang (ZPA) in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Zscaler persoonlijke toegang (ZPA) op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Zscaler persoonlijke toegang (ZPA) tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Zscaler persoonlijke toegang (ZPA), moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureren van eenmalige aanmelding Zscaler persoonlijke toegang (ZPA)](#configure-zscaler-private-access-zpa-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maak de testgebruiker Zscaler persoonlijke toegang (ZPA)](#create-zscaler-private-access-zpa-test-user)**  - hebben een equivalent van Britta Simon in Zscaler persoonlijke toegang (ZPA) die is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met Zscaler persoonlijke toegang (ZPA), moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **Zscaler persoonlijke toegang (ZPA)** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![Zscaler persoonlijke toegang (ZPA)-domein en URL's, eenmalige aanmelding informatie](common/sp-identifier.png)

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://samlsp.private.zscaler.com/auth/login?domain=<your-domain-name>`

    b. Typ een URL in het vak **Id (Entiteits-id)**: `https://samlsp.private.zscaler.com/auth/metadata`

    > [!NOTE]
    > De waarde voor **Aanmeldings-URL** is niet echt. Werk de waarde bij met de werkelijke aanmeldings-URL. Neem contact op met [Zscaler persoonlijke toegang (ZPA) Client-ondersteuningsteam](https://help.zscaler.com/zpa-submit-ticket) om de waarde. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **XML-bestand met federatieve metagegevens**  te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

6. Op de **van Zscaler persoonlijke toegang (ZPA) ingesteld** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-zscaler-private-access-zpa-single-sign-on"></a>Zscaler persoonlijke toegang (ZPA) eenmalige aanmelding configureren

1. In een ander browservenster, meld u aan bij uw bedrijf Zscaler persoonlijke toegang (ZPA) site als beheerder.

2. Navigeer naar **beheerder** en klik vervolgens op **Idp-configuratie**.

    ![Eenmalige aanmelding aan app-zijde configureren](./media/zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_04.png)

3. In de **Idp-configuratie** sectie, klikt u op **nieuwe IDP-configuratie toevoegen**.

    ![Eenmalige aanmelding aan app-zijde configureren](./media/zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_05.png)

4. In de **nieuwe IDP-configuratie** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding aan app-zijde configureren](./media/zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_06.png)

    a. Klik op **bestand selecteren** en uw gedownloade metagegevensbestand uploaden.

    b. Klik op de knop **Save**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken 

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In de **gebruikersnaam** veldtype brittasimon@yourcompanydomain.extension. Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Zscaler persoonlijke toegang (ZPA).

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **Zscaler persoonlijke toegang (ZPA)**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **Zscaler persoonlijke toegang (ZPA)**.

    ![De koppeling Zscaler persoonlijke toegang (ZPA) in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-zscaler-private-access-zpa-test-user"></a>Testgebruiker Zscaler persoonlijke toegang (ZPA) maken

In deze sectie maakt u een gebruiker met de naam Britta Simon in Zscaler persoonlijke toegang (ZPA). Neem contact op met [Zscaler persoonlijke toegang (ZPA)-ondersteuningsteam](https://help.zscaler.com/zpa-submit-ticket) om toe te voegen de gebruikers in het platform Zscaler persoonlijke toegang (ZPA).

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Zscaler persoonlijke toegang (ZPA) in het toegangsvenster, moet u worden automatisch aangemeld op de Zscaler persoonlijke toegang (ZPA) waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

