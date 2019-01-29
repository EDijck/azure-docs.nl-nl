---
title: 'Zelfstudie: Azure Active Directory-integratie met duidelijke controleren | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Schakel controle.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 8264159a-11a2-4a8c-8285-4efea0adac8c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/12/2018
ms.author: jeedes
ms.openlocfilehash: 577dc0192dc9956e302e327092bc21d59fb5a0c0
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55179545"
---
# <a name="tutorial-azure-active-directory-integration-with-clear-review"></a>Zelfstudie: Azure Active Directory-integratie met duidelijke controleren

In deze zelfstudie leert u hoe u om te wissen revisie integreren met Azure Active Directory (Azure AD).

Schakel controle integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot wissen controleren heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij wissen controleren (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met duidelijke controleren, moet u de volgende items:

- Een Azure AD-abonnement
- Een duidelijke revisie eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Schakel controle uit de galerie toevoegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-clear-review-from-the-gallery"></a>Schakel controle uit de galerie toevoegen
Voor het configureren van de integratie van duidelijke controleren in Azure AD, moet u duidelijk revisie toevoegen uit de galerie aan de lijst met beheerde SaaS-apps.

**Als u wilt wissen revisie toevoegen uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak **wissen revisie**, selecteer **wissen revisie** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Controleer in de lijst met resultaten wissen](./media/clearreview-tutorial/tutorial_clearreview_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met duidelijke beoordeling op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in duidelijke beoordeling is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker wissen gecontroleerd tot stand worden gebracht.

Wissen gecontroleerd, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met duidelijke controleren, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **[Maak een testgebruiker wissen revisie](#create-a-clear-review-test-user)**  : als u wilt een equivalent van Britta Simon in duidelijke controleren die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing wissen controleren.

**Voor het configureren van Azure AD eenmalige aanmelding met duidelijke controleren, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **wissen revisie** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/clearreview-tutorial/tutorial_clearreview_samlbase.png)

1. Op de **wissen revisie domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IdP gestart** modus:

    ![Duidelijke revisie domein en URL's eenmalige aanmelding informatie](./media/clearreview-tutorial/tutorial_clearreview_url.png)

    a. Typ in het tekstvak **Id** een URL met het volgende patroon: `https://<customer name>.clearreview.com/sso/metadata/`

    b. In het tekstvak **Antwoord-URL** typt u een URL met behulp van het volgende patroon: `https://<customer name>.clearreview.com/sso/acs/`

1. Controleer **geavanceerde URL-instellingen weergeven** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Duidelijke revisie domein en URL's eenmalige aanmelding informatie](./media/clearreview-tutorial/tutorial_clearreview_url_sp.png)

    In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon:`https://<customer name>.clearreview.com`

    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden met de werkelijke aanmeldings-URL, de id en de antwoord-URL. Neem contact op met [wissen revisie ondersteuningsteam](https://clearreview.com/contact/) om deze waarden te verkrijgen.

1. De unieke gebruikers-id-waarde wissen revisie toepassing verwacht in de naam id claim. U moet de gebruiker-id-waarde om te worden toegewezen **user.mail**.

    ![De kenmerk-sectie](./media/clearreview-tutorial/attribute.png)


1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base64)** en slaat u het certificaatbestand op uw computer.

    ![De link om het certificaat te downloaden](./media/clearreview-tutorial/tutorial_clearreview_certificate.png)

1. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/clearreview-tutorial/tutorial_general_400.png)

1. Op de **wissen revisie configuratie** sectie, klikt u op **wissen controle configureren** openen **aanmelding configureren** venster. Kopiëren de **URL van de afmelding, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Wissen van de configuratie controleren](./media/clearreview-tutorial/tutorial_clearreview_configure.png) 

1. Het configureren van eenmalige aanmelding op **wissen revisie** aan clientzijde, open de **wissen revisie** portal met beheerdersreferenties.

1. Selecteer **Admin** in de linkernavigatiebalk.

    ![De knop voor enkelvoudige aanmelding configureren](./media/clearreview-tutorial/tutorial_clearreview_app_admin1.png)

1. Selecteer **wijziging** aan de onderkant van de pagina.

    ![De knop voor enkelvoudige aanmelding configureren](./media/clearreview-tutorial/tutorial_clearreview_app_admin2.png)

1. Voert u de volgende stappen uit op **instellingen voor eenmalige aanmelding** pagina

    ![De knop voor enkelvoudige aanmelding configureren](./media/clearreview-tutorial/tutorial_clearreview_app_admin3.png)

    a. In de **URL-verlener** tekstvak, plak de waarde van **SAML entiteit-ID** die u hebt gekopieerd vanuit Azure portal.

    b. In de **SAML-eindpunt** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML** die u hebt gekopieerd vanuit Azure portal.    

    c. In de **SLO eindpunt** tekstvak, plak de waarde van **aanmeldings-URL van Service** die u hebt gekopieerd vanuit Azure portal. 

    d. Open het gedownloade certificaat in Kladblok en plak de inhoud in de **X.509-certificaat** tekstvak.   

1. Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/clearreview-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/clearreview-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/clearreview-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/clearreview-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
  
### <a name="create-a-clear-review-test-user"></a>Maak een testgebruiker wissen controleren

In deze sectie maakt maken u een gebruiker met de naam van Britta Simon in duidelijke beoordeling. Neem contact op met [wissen revisie ondersteuningsteam](https://clearreview.com/contact/) om toe te voegen de gebruikers in het platform wissen controleren.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen om te wissen controle.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon naar duidelijk overzicht, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **wissen revisie**.

    ![De koppeling wissen controleren in de lijst met toepassingen](./media/clearreview-tutorial/tutorial_clearreview_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel wissen controleren in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing wissen controleren.
Zie [Introduction to the Access Panel](../user-help/active-directory-saas-access-panel-introduction.md) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster. 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/clearreview-tutorial/tutorial_general_01.png
[2]: ./media/clearreview-tutorial/tutorial_general_02.png
[3]: ./media/clearreview-tutorial/tutorial_general_03.png
[4]: ./media/clearreview-tutorial/tutorial_general_04.png

[100]: ./media/clearreview-tutorial/tutorial_general_100.png

[200]: ./media/clearreview-tutorial/tutorial_general_200.png
[201]: ./media/clearreview-tutorial/tutorial_general_201.png
[202]: ./media/clearreview-tutorial/tutorial_general_202.png
[203]: ./media/clearreview-tutorial/tutorial_general_203.png
