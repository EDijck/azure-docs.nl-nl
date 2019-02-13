---
title: 'Zelfstudie: Azure Active Directory-integratie met Silverback | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Silverback.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 32cfc96f-2137-49ff-818b-67feadcd73b7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/17/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: b5614c061586c39e44f04f3542285e55e07f14d9
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56172704"
---
# <a name="tutorial-azure-active-directory-integration-with-silverback"></a>Zelfstudie: Azure Active Directory-integratie met Silverback

In deze zelfstudie leert u hoe u Silverback integreren met Azure Active Directory (Azure AD).

Silverback integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Silverback heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Silverback (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Silverback, moet u de volgende items:

- Een Azure AD-abonnement
- Een actief abonnement op Silverback

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Silverback uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-silverback-from-the-gallery"></a>Silverback uit de galerie toe te voegen
Voor het configureren van de integratie van Silverback in Azure AD, moet u Silverback uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Silverback uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

4. Typ in het zoekvak **Silverback**, selecteer **Silverback** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Silverback in de lijst met resultaten](./media/silverback-tutorial/tutorial_silverback_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Silverback op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Silverback is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Silverback tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Silverback, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
3. **[Maak een testgebruiker Silverback](#create-a-silverback-test-user)**  : als u wilt een equivalent van Britta Simon in Silverback die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Silverback.

**Voor het configureren van Azure AD eenmalige aanmelding met Silverback, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Silverback** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/silverback-tutorial/tutorial_silverback_samlbase.png)

3. Op de **Silverback domein en URL's** sectie, voert u de volgende stappen uit:

    ![Silverback domein en URL's, eenmalige aanmelding informatie](./media/silverback-tutorial/tutorial_silverback_url.png)

    a. Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://<YOURSILVERBACKURL>.com/ssp`

    b. In de **id** tekstvak, een URL met behulp van het volgende patroon: `<YOURSILVERBACKURL>.com`

    c. In het tekstvak **Antwoord-URL** typt u een URL met behulp van het volgende patroon: `https://<YOURSILVERBACKURL>.com/sts/authorize/login`

    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke id, de antwoord-URL en de aanmeldings-URL. Neem contact op met [Silverback Client ondersteuningsteam](mailto:helpdesk@matrix42.com) om deze waarden te verkrijgen. 

4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op de knop kopiëren om te kopiëren **App-Url voor federatieve metagegevens** en plak deze in Kladblok.

    ![De link om het certificaat te downloaden](./media/silverback-tutorial/tutorial_silverback_certificate.png) 

5. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/silverback-tutorial/tutorial_general_400.png)

6.  Meld u aan bij uw Silverback-Server als beheerder en voer de volgende stappen uit:

    a.  Navigeer naar **Admin** > **verificatieprovider**.

    b. Op de **verificatie-instellingen voor Provider** pagina, voert u de volgende stappen uit:

    ![De beheerder ](./media/silverback-tutorial/tutorial_silverback_admin.png)

    c.  Klik op **importeren vanaf URL**.
    
    d.  Plak de gekopieerde metagegevens-URL en klik op **OK**.
    
    e.  Controleer of met **OK** en vervolgens de waarden worden automatisch ingevuld.
    
    f.  Schakel **weergeven op de aanmeldingspagina**.
    
    g.  Schakel **dynamische maken door gebruiker** als u wilt met Azure AD gemachtigde gebruikers automatisch toevoegen (optioneel).
    
    h.  Maak een **titel** voor de knop op de selfserviceportal.

    i.  Upload een **pictogram** door te klikken op **bestand kiezen**.
    
    j.  Selecteer de achtergrond **kleur** voor de knop.
    
    k.  Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/silverback-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/silverback-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/silverback-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/silverback-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-silverback-test-user"></a>Maak een testgebruiker Silverback

Als u wilt dat Azure AD-gebruikers zich aanmelden bij Silverback, moeten ze worden ingericht voor Silverback. In Silverback is inrichten een handmatige taak.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1. Meld u aan met uw Silverback-Server als beheerder.

2. Navigeer naar **gebruikers** en **toevoegen van een nieuwe apparaatgebruiker**.

3. Op de **Basic** pagina, voert u de volgende stappen uit:

    ![De gebruiker ](./media/silverback-tutorial/tutorial_silverback_user.png)

    a. In **gebruikersnaam** tekst voert u de naam van gebruiker, zoals **Julia**.

    b. Voer in het tekstvak **Voornaam** de voornaam van de gebruiker in, zoals **Britta**.

    c. In **achternaam** tekst voert u de achternaam van de gebruiker, zoals **Simon**.

    d. In **e-mailadres** tekst vak, voer het e-mailadres van gebruiker, zoals **Brittasimon@contoso.com**.

    e. In de **wachtwoord** tekst vak, Voer uw wachtwoord.
    
    f. In de **wachtwoord bevestigen** tekstvak, uw wachtwoord opnieuw invoeren en bevestigen.

    g. Klik op **Opslaan**.

>[!NOTE]
>Als u niet wilt dat elke gebruiker handmatig maken inschakelen de **dynamische maken door gebruiker** selectievakje onder **Admin** > **verificatieprovider**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Silverback.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon aan Silverback toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Silverback**.

    ![De koppeling Silverback in de lijst met toepassingen](./media/silverback-tutorial/tutorial_silverback_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Silverback in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Silverback.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/silverback-tutorial/tutorial_general_01.png
[2]: ./media/silverback-tutorial/tutorial_general_02.png
[3]: ./media/silverback-tutorial/tutorial_general_03.png
[4]: ./media/silverback-tutorial/tutorial_general_04.png

[100]: ./media/silverback-tutorial/tutorial_general_100.png

[200]: ./media/silverback-tutorial/tutorial_general_200.png
[201]: ./media/silverback-tutorial/tutorial_general_201.png
[202]: ./media/silverback-tutorial/tutorial_general_202.png
[203]: ./media/silverback-tutorial/tutorial_general_203.png

