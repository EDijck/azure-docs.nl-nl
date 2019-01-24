---
title: 'Zelfstudie: Azure Active Directory-integratie met Insperity ExpensAble | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Insperity ExpensAble.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: c579c453-580e-417d-8a5e-9b6b352795c0
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: jeedes
ms.openlocfilehash: b078f459faf4da468a0b0e4ec7c6e880627e4a4d
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54821175"
---
# <a name="tutorial-azure-active-directory-integration-with-insperity-expensable"></a>Zelfstudie: Azure Active Directory-integratie met Insperity ExpensAble

In deze zelfstudie leert u hoe u Insperity ExpensAble integreren met Azure Active Directory (Azure AD).

Insperity ExpensAble integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Insperity ExpensAble heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Insperity ExpensAble (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Insperity ExpensAble, moet u de volgende items:

- Een Azure AD-abonnement
- Een Insperity ExpensAble eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Toe te voegen Insperity ExpensAble uit de galerie
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-insperity-expensable-from-the-gallery"></a>Toe te voegen Insperity ExpensAble uit de galerie
Voor het configureren van de integratie van Insperity ExpensAble in Azure AD, moet u Insperity ExpensAble toevoegen uit de galerie aan de lijst met beheerde SaaS-apps.

**Als u wilt Insperity ExpensAble uit de galerie toevoegen, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **Insperity ExpensAble**.

    ![Het maken van een Azure AD-testgebruiker](./media/insperityexpensable-tutorial/tutorial_insperityexpensable_search.png)

1. Selecteer in het deelvenster resultaten **Insperity ExpensAble**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/insperityexpensable-tutorial/tutorial_insperityexpensable_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Insperity ExpensAble op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Insperity ExpensAble is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Insperity ExpensAble tot stand worden gebracht.

In Insperity ExpensAble, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Insperity ExpensAble, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een Insperity ExpensAble testgebruiker](#creating-an-insperity-expensable-test-user)**  : als u wilt een equivalent van Britta Simon in Insperity ExpensAble die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw Insperity ExpensAble toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met Insperity ExpensAble, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Insperity ExpensAble** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/insperityexpensable-tutorial/tutorial_insperityexpensable_samlbase.png)

1. Op de **Insperity ExpensAble domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/insperityexpensable-tutorial/tutorial_insperityexpensable_url.png)

    a. Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://server.expensable.com/esapp/Authenticate?companyId=<company ID>`

    > [!NOTE] 
    > Deze waarde is niet echt. Deze waarde bijwerken met de werkelijke aanmeldings-URL. Neem contact op met [Insperity ExpensAble Client ondersteuningsteam](http://expensable.com/support/support-overview) deze waarde op te halen. 
 
1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/insperityexpensable-tutorial/tutorial_insperityexpensable_certificate.png) 

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/insperityexpensable-tutorial/tutorial_general_400.png)

1. Op de **Insperity ExpensAble configuratie** sectie, klikt u op **Insperity ExpensAble configureren** openen **aanmelding configureren** venster. Kopiëren de **SAML entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/insperityexpensable-tutorial/tutorial_insperityexpensable_configure.png) 

1. Het configureren van eenmalige aanmelding op **Insperity ExpensAble** zijde, moet u voor het verzenden van de gedownloade **Metadata XML**, **Single Sign-On Service URL voor SAML** en  **SAML-entiteit-ID** naar [Insperity ExpensAble ondersteuningsteam](http://expensable.com/support/support-overview). Het team stelt de instellingen zo in dat de verbinding tussen SAML en eenmalige aanmelding aan beide zijden goed is ingesteld.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/insperityexpensable-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/insperityexpensable-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/insperityexpensable-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/insperityexpensable-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-an-insperity-expensable-test-user"></a>Het maken van een Insperity ExpensAble testgebruiker

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in Insperity ExpensAble. Neem contact op met [Insperity ExpensAble ondersteuningsteam](http://expensable.com/support/support-overview) om toe te voegen de gebruikers in de Insperity ExpensAble-account.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Insperity ExpensAble.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan Insperity ExpensAble toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Insperity ExpensAble**.

    ![Eenmalige aanmelding configureren](./media/insperityexpensable-tutorial/tutorial_insperityexpensable_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de Insperity ExpensAble tegel in het toegangsvenster, u moet u automatisch aangemeld bij uw Insperity ExpensAble toepassing.
Zie [Introduction to the Access Panel](../user-help/active-directory-saas-access-panel-introduction.md) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/insperityexpensable-tutorial/tutorial_general_01.png
[2]: ./media/insperityexpensable-tutorial/tutorial_general_02.png
[3]: ./media/insperityexpensable-tutorial/tutorial_general_03.png
[4]: ./media/insperityexpensable-tutorial/tutorial_general_04.png
[100]: ./media/insperityexpensable-tutorial/tutorial_general_100.png
[200]: ./media/insperityexpensable-tutorial/tutorial_general_200.png
[201]: ./media/insperityexpensable-tutorial/tutorial_general_201.png
[202]: ./media/insperityexpensable-tutorial/tutorial_general_202.png
[203]: ./media/insperityexpensable-tutorial/tutorial_general_203.png

