---
title: 'Zelfstudie: Azure Active Directory-integratie met één Zoho | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en één Zoho.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: bbc3038c-0d8b-45dd-9645-368bd3d01a0f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: aadb3188969e099a709711ac18916b67c83b5da1
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57884435"
---
# <a name="tutorial-azure-active-directory-integration-with-zoho-one"></a>Zelfstudie: Azure Active Directory-integratie met één Zoho

In deze zelfstudie leert u hoe u een Zoho integreren met Azure Active Directory (Azure AD).

Zoho één integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot één Zoho heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij een Zoho (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het Azure AD-integratie configureren met één Zoho, moet u de volgende items:

- Een Azure AD-abonnement
- Een Zoho een eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Toevoegen van een Zoho uit de galerie
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-zoho-one-from-the-gallery"></a>Toevoegen van een Zoho uit de galerie
Voor het configureren van de integratie van één Zoho in Azure AD, moet u één Zoho toevoegen uit de galerie aan de lijst met beheerde SaaS-apps.

**Als u wilt één Zoho uit de galerie toevoegen, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak **Zoho één**, selecteer **Zoho één** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Zoho in de lijst met resultaten](./media/zohoone-tutorial/tutorial_zohoone_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Zoho één op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in één Zoho is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in één Zoho tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met één Zoho, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **[Maak een testgebruiker Zoho één](#create-a-zoho-one-test-user)**  : als u wilt een equivalent van Britta Simon in Zoho één die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw Zoho één toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met één Zoho, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Zoho één** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/zohoone-tutorial/tutorial_zohoone_samlbase.png)

1. Op de **Zoho één domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![Zoho één domein en URL's, eenmalige aanmelding informatie](./media/zohoone-tutorial/tutorial_zohoone_url.png)

    a. In de **id (entiteits-ID)** tekstvak, een URL typen: `one.zoho.com`

    b. In het tekstvak **Antwoord-URL** typt u een URL met behulp van het volgende patroon: `https://accounts.zoho.com/samlresponse/<saml-identifier>`

    c. Controleer **geavanceerde URL-instellingen weergeven**.

    d. In de **Relaystatus** tekstvak, een URL typen:`https://one.zoho.com`

1. Als u de toepassing wilt configureren in door **SP**geïnitieerde modus, voert u de volgende stap uit:

    Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://accounts.zoho.com/samlauthrequest/<domain_name>?serviceurl=https://one.zoho.com`
     
    > [!NOTE] 
    > De voorgaande **antwoord-URL** en **aanmeldings-URL** waarde is niet echt. U kunt de waarde wordt bijgewerkt met de werkelijke antwoord-URL en de aanmeldings-URL die later in de zelfstudie wordt uitgelegd. 

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base64)** en slaat u het certificaatbestand op uw computer.

    ![De link om het certificaat te downloaden](./media/zohoone-tutorial/tutorial_zohoone_certificate.png) 

1. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/zohoone-tutorial/tutorial_general_400.png)
    
1. Op de **Zoho één configuratie** sectie, klikt u op **Zoho één configureren** openen **aanmelding configureren** venster. Kopiëren de **afmelding URL's en SAML Single Sign-On Service** uit de **Naslaggids sectie.**

    ![Zoho één configuratie](./media/zohoone-tutorial/tutorial_zohoone_configure.png) 

1. In een ander browservenster aanmelden met uw Zoho een bedrijf site als beheerder.

1. Op de **organisatie** tabblad, klikt u op **Setup** onder **SAML-verificatie**.

    ![Zoho One org](./media/zohoone-tutorial/tutorial_zohoone_setup.png)

1. Voer de volgende stappen uit op de pop-pagina:

    ![Zoho One sig](./media/zohoone-tutorial/tutorial_zohoone_save.png)

    a. In de **aanmelden URL** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML**, die u hebt gekopieerd vanuit Azure portal.

    b. In de **afmelding URL** tekstvak, plak de waarde van **afmelding URL**, die u hebt gekopieerd vanuit Azure portal.

    c. Klik op **Bladeren** het uploaden van de **certificaat (Base64)** die u hebt gedownload vanuit Azure portal.

    d. Klik op **Opslaan**.

1. Na het opslaan van de SAML-verificatie-instellingen, Kopieer de **SAML-Identfier** waarde en gebruikt deze waarde in de **antwoord-URL** in Azure portal, onder **Zoho één domein en URL's** de sectie.

    ![Zoho One saml](./media/zohoone-tutorial/tutorial_zohoone_samlidenti.png)

1. Ga naar de **domeinen** tabblad en klik vervolgens op **domein toevoegen**.

    ![Zoho One domain](./media/zohoone-tutorial/tutorial_zohoone_domain.png)

1. Op de **domein toevoegen** pagina, voert u de volgende stappen uit:

    ![Zoho één toevoegen domein](./media/zohoone-tutorial/tutorial_zohoone_adddomain.png)

    a. In de **domeinnaam** tekstvak, typ het domein, zoals **contoso.com**.

    b. Klik op **Add**.

    >[!Note]
    >Na het toevoegen van het domein volgen [deze](https://www.zoho.com/one/help/admin-guide/domain-verification.html) stappen om uw domein te verifiëren. Nadat het domein verfified is, gebruikt u de naam van uw domein in **aanmeldings-URL** in **Zoho één domein en URL's** sectie in Azure portal.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/zohoone-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/zohoone-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/zohoone-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/zohoone-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-zoho-one-test-user"></a>Maak een Zoho een testgebruiker

Als u wilt dat Azure AD-gebruikers zich aanmelden bij een Zoho, moeten ze worden ingericht voor Zoho één. In één Zoho is inrichten een handmatige taak.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1. Meld u aan bij Zoho een als een beveiligingsbeheerder.

1. Op de **gebruikers** tabblad, klik op **gebruiker logo**.

    ![Zoho één gebruiker](./media/zohoone-tutorial/tutorial_zohoone_users.png)

1. Op de **gebruiker toevoegen** pagina, voert u de volgende stappen uit:

    ![Zoho één toevoegen gebruiker](./media/zohoone-tutorial/tutorial_zohoone_adduser.png)
    
    a. In **naam** tekst voert u de naam van gebruiker, zoals **Julia steen**.
    
    b. In **e-mailadres** tekst vak, voer het e-mailadres van gebruiker, zoals **brittasimon\@contoso.com**.

    >[!Note]
    >Selecteer uw geverifieerde domein in de lijst met domeinen.

    c. Klik op **Add**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan één Zoho.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon aan één Zoho, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Zoho één**.

    ![De Zoho een koppeling in de lijst met toepassingen](./media/zohoone-tutorial/tutorial_zohoone_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de Zoho een tegel in het toegangsvenster, u moet u automatisch aangemeld bij uw Zoho één toepassing.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/zohoone-tutorial/tutorial_general_01.png
[2]: ./media/zohoone-tutorial/tutorial_general_02.png
[3]: ./media/zohoone-tutorial/tutorial_general_03.png
[4]: ./media/zohoone-tutorial/tutorial_general_04.png

[100]: ./media/zohoone-tutorial/tutorial_general_100.png

[200]: ./media/zohoone-tutorial/tutorial_general_200.png
[201]: ./media/zohoone-tutorial/tutorial_general_201.png
[202]: ./media/zohoone-tutorial/tutorial_general_202.png
[203]: ./media/zohoone-tutorial/tutorial_general_203.png

