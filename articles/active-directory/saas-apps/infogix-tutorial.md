---
title: 'Zelfstudie: Azure Active Directory-integratie met Infogix Data3Sixty beheren | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Infogix Data3Sixty beheren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: aa3109b8-bdbe-45ae-933a-2eb4dc03855c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/23/2018
ms.author: jeedes
ms.openlocfilehash: 5e9b805786346abd6dfe830c9ce6ae4cc341c9e7
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55194267"
---
# <a name="tutorial-azure-active-directory-integration-with-infogix-data3sixty-govern"></a>Zelfstudie: Azure Active Directory-integratie met Infogix Data3Sixty beheren

In deze zelfstudie leert u over het integreren van Infogix Data3Sixty beheren met Azure Active Directory (Azure AD).

Infogix Data3Sixty regelen integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Infogix Data3Sixty beheren heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Infogix Data3Sixty beheren (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Infogix Data3Sixty beheren, moet u de volgende items:

- Een Azure AD-abonnement
- Een Infogix Data3Sixty regelen eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Toe te voegen Infogix Data3Sixty beheren vanuit de galerie
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-infogix-data3sixty-govern-from-the-gallery"></a>Toe te voegen Infogix Data3Sixty beheren vanuit de galerie
Voor het configureren van de integratie van Infogix Data3Sixty beheren in Azure AD, moet u Infogix Data3Sixty beheren vanuit de galerie aan uw lijst met beheerde SaaS-apps toevoegen.

**Als u wilt toevoegen Infogix Data3Sixty beheren vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak **Infogix Data3Sixty bepalen**, selecteer **Infogix Data3Sixty regelen** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Infogix Data3Sixty beheren in de lijst met resultaten](./media/infogix-tutorial/tutorial_infogix_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Infogix Data3Sixty bepalen op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Infogix Data3Sixty beheren in Azure AD aan een gebruiker is. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Infogix Data3Sixty bepalen tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Infogix Data3Sixty beheren, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **[Maak een testgebruiker Infogix Data3Sixty regelen](#create-an-infogix-data3sixty-govern-test-user)**  : als u wilt een equivalent van Britta Simon in Infogix Data3Sixty beheren die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Infogix Data3Sixty beheren.

**Voor het configureren van Azure AD eenmalige aanmelding met Infogix Data3Sixty beheren, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Infogix Data3Sixty regelen** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/infogix-tutorial/tutorial_infogix_samlbase.png)

1. Op de **Infogix Data3Sixty regelen domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![Infogix Data3Sixty regelen domein en URL's, eenmalige aanmelding informatie](./media/infogix-tutorial/tutorial_infogix_url.png)

    a. In de **id** tekstvak, een URL typen: `https://data3sixty.com/ui`

    b. In het tekstvak **Antwoord-URL** typt u een URL met behulp van het volgende patroon: `https://<subdomain>.data3sixty.com/sso/acs`

1. Controleer **geavanceerde URL-instellingen weergeven** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Infogix Data3Sixty regelen domein en URL's, eenmalige aanmelding informatie](./media/infogix-tutorial/tutorial_infogix_url1.png)

    Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://<subdomain>.data3sixty.com`
     
    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden bij met de echte antwoord-URL en aanmeldings-URL. Neem contact op met [Infogix Data3Sixty regelen Client ondersteuningsteam](mailto:data3sixtysupport@infogix.com) om deze waarden te verkrijgen.

1. Toepassing Infogix Data3Sixty regelen wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit de sectie **Gebruikerskenmerken** op de integratiepagina van de toepassing-beheren. In de volgende schermopname ziet u een voorbeeld hiervan.
    
    ![Single Sign-On attb configureren](./media/infogix-tutorial/tutorial_infogix_attribute.png)
    
1. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de afbeelding en voer de volgende stappen uit:
    
    | Naam kenmerk | Waarde kenmerk |
    | ------------------- | -------------------- |    
    | firstname           | user.givenname |
    | lastname        | user.surname |
    | gebruikersnaam       | user.mail    |
    
    a. Klik op **kenmerk toevoegen** openen de **kenmerk toevoegen** dialoogvenster.

    ![Configureren van eenmalige aanmelding toevoegen](./media/infogix-tutorial/tutorial_attribute_04.png)

    ![Single Sign-On Addattb configureren](./media/infogix-tutorial/tutorial_attribute_05.png)

    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.

    c. Uit de **waarde** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    d. Laat **Naamruimte** leeg.
    
    e. Klik op **OK**.

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Raw)** en slaat u het certificaatbestand op uw computer.

    ![De link om het certificaat te downloaden](./media/infogix-tutorial/tutorial_infogix_certificate.png)

1. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/infogix-tutorial/tutorial_general_400.png)
    
1. Op de **Infogix Data3Sixty beheren configuratie** sectie, klikt u op **configureren Infogix Data3Sixty regelen** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Infogix Data3Sixty beheren configuratie](./media/infogix-tutorial/tutorial_infogix_configure.png) 

1. Het configureren van eenmalige aanmelding op **Infogix Data3Sixty regelen** zijde, moet u voor het verzenden van de gedownloade **certificaat (Raw), URL van de afmelding, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** naar [ Ondersteuningsteam Infogix Data3Sixty regelen](mailto:data3sixtysupport@infogix.com). Het team stelt de instellingen zo in dat de verbinding tussen SAML en eenmalige aanmelding aan beide zijden goed is ingesteld.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/infogix-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/infogix-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/infogix-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/infogix-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-an-infogix-data3sixty-govern-test-user"></a>Maak een testgebruiker Infogix Data3Sixty beheren


Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in Infogix Data3Sixty beheren. Infogix Data3Sixty regelen biedt ondersteuning voor just-in-time inrichting, dit is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot Infogix Data3Sixty beheren als deze nog niet bestaat.

>[!Note]
>Als u maken van een gebruiker handmatig wilt, neem dan contact op met [ondersteuningsteam Infogix Data3Sixty regelen](mailto:data3sixtysupport@infogix.com).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen om te bepalen van Infogix Data3Sixty.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon om te bepalen van Infogix Data3Sixty, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Infogix Data3Sixty regelen**.

    ![De koppeling Infogix Data3Sixty beheren in de lijst met toepassingen](./media/infogix-tutorial/tutorial_infogix_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Infogix Data3Sixty beheren in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Infogix Data3Sixty beheren.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/infogix-tutorial/tutorial_general_01.png
[2]: ./media/infogix-tutorial/tutorial_general_02.png
[3]: ./media/infogix-tutorial/tutorial_general_03.png
[4]: ./media/infogix-tutorial/tutorial_general_04.png

[100]: ./media/infogix-tutorial/tutorial_general_100.png

[200]: ./media/infogix-tutorial/tutorial_general_200.png
[201]: ./media/infogix-tutorial/tutorial_general_201.png
[202]: ./media/infogix-tutorial/tutorial_general_202.png
[203]: ./media/infogix-tutorial/tutorial_general_203.png

