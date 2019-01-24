---
title: 'Zelfstudie: Azure Active Directory-integratie met van (MBC Mercer-BenefitsCentral) | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Mercer BenefitsCentral (van MBC).
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 3788b28c-49aa-4208-9acd-630362008e89
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/21/2017
ms.author: jeedes
ms.openlocfilehash: 0bb6b5ac2a52f559461077d6318eee8ed52aedc2
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54809510"
---
# <a name="tutorial-azure-active-directory-integration-with-mercer-benefitscentral-mbc"></a>Zelfstudie: Azure Active Directory-integratie met van (MBC Mercer-BenefitsCentral)

In deze zelfstudie leert u hoe u Mercer BenefitsCentral (van MBC) integreren met Azure Active Directory (Azure AD).

(Van MBC Mercer BenefitsCentral) integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang hebben naar van (MBC Mercer-BenefitsCentral).
- U kunt uw gebruikers automatisch ophalen aangemeld op Mercer BenefitsCentral (van MBC) (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met van (MBC Mercer-BenefitsCentral), moet u de volgende items:

- Een Azure AD-abonnement
- Een Mercer BenefitsCentral (van MBC) eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. (Van MBC Mercer BenefitsCentral) uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-mercer-benefitscentral-mbc-from-the-gallery"></a>(Van MBC Mercer BenefitsCentral) uit de galerie toe te voegen
Voor het configureren van de integratie van van (MBC Mercer-BenefitsCentral) in Azure AD, moet u Mercer BenefitsCentral (van MBC) uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen (van MBC Mercer BenefitsCentral) uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak **Mercer BenefitsCentral (van MBC)**, selecteer **Mercer BenefitsCentral (van MBC)** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Mercer BenefitsCentral (van MBC) in de lijst met resultaten](./media/mercerhrs-tutorial/tutorial_mercerhrs_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Mercer BenefitsCentral (van MBC) op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in van (MBC Mercer-BenefitsCentral) is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in van (MBC Mercer-BenefitsCentral) tot stand worden gebracht.

In Mercer BenefitsCentral (van MBC), wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met van (MBC Mercer-BenefitsCentral), moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **[Maak een testgebruiker (van MBC Mercer BenefitsCentral)](#create-a-mercer-benefitscentral-mbc-test-user)**  - hebben een equivalent van Britta Simon in Mercer BenefitsCentral (van MBC) die is gekoppeld aan de Azure AD-weergave van de gebruiker.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing (van MBC Mercer BenefitsCentral).

**Voor het configureren van Azure AD eenmalige aanmelding met van (MBC Mercer-BenefitsCentral), moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Mercer BenefitsCentral (van MBC)** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/mercerhrs-tutorial/tutorial_mercerhrs_samlbase.png)

1. Op de **Mercer BenefitsCentral (van MBC)-domein en URL's** sectie, voert u de volgende stappen uit:

    ![Mercer BenefitsCentral (van MBC)-domein en URL's, eenmalige aanmelding informatie](./media/mercerhrs-tutorial/tutorial_mercerhrs_url.png)

    a. Typ in het tekstvak **Id** een URL met het volgende patroon: `stg.mercerhrs.com/saml2.0`

    b. In het tekstvak **Antwoord-URL** typt u een URL met behulp van het volgende patroon: `https://ssous-stg.mercerhrs.com/SP2/Saml2AssertionConsumer.aspx`

    > [!NOTE] 
    > De waarde van de antwoord-URL is niet echt. Deze waarde bijwerken met de werkelijke antwoord-URL. Neem contact op met [Mercer BenefitsCentral (van MBC)-ondersteuningsteam](https://www.mercer.com/contact-us.html) deze waarde op te halen.

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/mercerhrs-tutorial/tutorial_mercerhrs_certificate.png) 

1. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/mercerhrs-tutorial/tutorial_general_400.png)

1. Op de **Mercer BenefitsCentral (van MBC) configuratie** sectie, klikt u op **configureren Mercer BenefitsCentral (van MBC)** openen **aanmelding configureren** venster. Kopiëren de **Single Sign-On Service URL voor SAML** uit de **Naslaggids sectie.**

    ![Configuratie van Mercer BenefitsCentral (van MBC)](./media/mercerhrs-tutorial/tutorial_mercerhrs_configure.png) 

1. Het configureren van eenmalige aanmelding op **Mercer BenefitsCentral (van MBC)** zijde, moet u voor het verzenden van de gedownloade **Metadata XML** en **Single Sign-On Service URL voor SAML** naar [Mercer BenefitsCentral (van MBC)-ondersteuningsteam](https://www.mercer.com/contact-us.html). Het team stelt de instellingen zo in dat de verbinding tussen SAML en eenmalige aanmelding aan beide zijden goed is ingesteld.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/mercerhrs-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/mercerhrs-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/mercerhrs-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/mercerhrs-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
  
### <a name="create-a-mercer-benefitscentral-mbc-test-user"></a>Maak een testgebruiker (van MBC Mercer BenefitsCentral)

In deze sectie maakt u een gebruiker met de naam van Britta Simon in Mercer uur. Werken met [Mercer BenefitsCentral (van MBC)-ondersteuningsteam](https://www.mercer.com/contact-us.html) om toe te voegen de gebruikers in het platform Mercer uur. Er moeten gebruikers worden gemaakt en geactiveerd voordat u eenmalige aanmelding kunt gebruiken.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan van (MBC Mercer-BenefitsCentral).

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon naar van (MBC Mercer-BenefitsCentral), moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Mercer BenefitsCentral (van MBC)**.

    ![De koppeling Mercer BenefitsCentral (van MBC) in de lijst met toepassingen](./media/mercerhrs-tutorial/tutorial_mercerhrs_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel (van MBC Mercer BenefitsCentral) in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing (van MBC Mercer BenefitsCentral).
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/mercerhrs-tutorial/tutorial_general_01.png
[2]: ./media/mercerhrs-tutorial/tutorial_general_02.png
[3]: ./media/mercerhrs-tutorial/tutorial_general_03.png
[4]: ./media/mercerhrs-tutorial/tutorial_general_04.png

[100]: ./media/mercerhrs-tutorial/tutorial_general_100.png

[200]: ./media/mercerhrs-tutorial/tutorial_general_200.png
[201]: ./media/mercerhrs-tutorial/tutorial_general_201.png
[202]: ./media/mercerhrs-tutorial/tutorial_general_202.png
[203]: ./media/mercerhrs-tutorial/tutorial_general_203.png

