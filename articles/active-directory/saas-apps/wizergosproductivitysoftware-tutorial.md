---
title: 'Zelfstudie: Azure Active Directory-integratie met Wizergos productiviteitssoftware | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Wizergos productiviteitssoftware.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: acc04396-13c5-4c24-ab9a-30fbc9234ebd
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/24/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 217c8e9eed9e3e1edf2a6080afdf244ddc3fae94
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56208092"
---
# <a name="tutorial-azure-active-directory-integration-with-wizergos-productivity-software"></a>Zelfstudie: Azure Active Directory-integratie met Wizergos productiviteitssoftware

In deze zelfstudie leert u hoe u Wizergos productiviteit om Software te integreren met Azure Active Directory (Azure AD).

Wizergos productiviteitssoftware integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Wizergos productiviteitssoftware heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij de productiviteitssoftware Wizergos (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Wizergos productiviteitssoftware, moet u de volgende items:

- Een Azure AD-abonnement
- Een Wizergos productiviteitssoftware eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Wizergos productiviteitssoftware uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-wizergos-productivity-software-from-the-gallery"></a>Wizergos productiviteitssoftware uit de galerie toe te voegen
Voor het configureren van de integratie van Wizergos productiviteitssoftware in Azure AD, moet u Wizergos productiviteitssoftware uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Wizergos productiviteitssoftware in de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak **Wizergos productiviteitssoftware**, selecteer **Wizergos productiviteitssoftware** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Wizergos productiviteitssoftware in de lijst met resultaten](./media/wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Wizergos productiviteitssoftware op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Wizergos productiviteitssoftware is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Wizergos productiviteitssoftware tot stand worden gebracht.

In Wizergos productiviteitssoftware, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Als u wilt configureren en Azure AD eenmalige aanmelding met Wizergos productiviteitssoftware testen, moet u uitvoeren van de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **[Maak een testgebruiker Wizergos productiviteitssoftware](#create-a-wizergos-productivity-software-test-user)**  : als u wilt een equivalent van Britta Simon in Wizergos productiviteitssoftware die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Wizergos productiviteit.

**Voor het configureren van Azure AD eenmalige aanmelding met Wizergos productiviteitssoftware, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Wizergos productiviteitssoftware** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_samlbase.png)

1. Op de **Wizergos productiviteit Software domein en URL's** sectie, voert u de volgende stappen uit:

    ![Wizergos productiviteit Software domein en URL's, eenmalige aanmelding informatie](./media/wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_url.png)

    In de **id** tekstvak typt u de URL: `https://www.wizergos.net`

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat** en slaat u het certificaatbestand op uw computer.

    ![De link om het certificaat te downloaden](./media/wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_certificate.png) 

1. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/wizergosproductivitysoftware-tutorial/tutorial_general_400.png)

1. Op de **Wizergos productiviteit softwareconfiguratie** sectie, klikt u op **Wizergos productiviteitssoftware configureren** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![De softwareconfiguratie voor Wizergos productiviteit](./media/wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_configure.png) 

1. In een ander browservenster aanmelden voor uw tenant Wizergos productiviteitssoftware als een beheerder.

1. Selecteer in het hamburgermenu **Admin**.

    ![Configureren van eenmalige aanmelding op App-zijde](./media/wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_000.png)

1. Selecteer in de beheerpagina in het menu links **verificatie** en klikt u op **Azure AD**.

    ![Configureren van eenmalige aanmelding op App-zijde](./media/wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_002.png)

1. Voer de volgende stappen uit op **verificatie** sectie.

    ![Configureren van eenmalige aanmelding op App-zijde](./media/wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_003.png)
    
    a. Klik op **uploaden** knop om de gedownloade certificaat van Azure AD te uploaden.
    
    b. In de **URL-verlener** tekstvak, plak de **SAML entiteit-ID** waarde die u hebt gekopieerd vanuit Azure portal.
    
    c. In de **-URL met eenmalige aanmelding** tekstvak, plak de **Single Sign-On Service URL voor SAML** waarde die u hebt gekopieerd vanuit Azure portal.
    
    d. In de **URL voor eenmalige afmelding** tekstvak, plak de **afmelding URL** waarde die u hebt gekopieerd vanuit Azure portal.
    
    e. Klik op de knop **Save**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/wizergosproductivitysoftware-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/wizergosproductivitysoftware-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/wizergosproductivitysoftware-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/wizergosproductivitysoftware-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-wizergos-productivity-software-test-user"></a>Maak een testgebruiker Wizergos productiviteitssoftware

In deze sectie maakt u een gebruiker met de naam van Britta Simon in Wizergos productiviteitssoftware. Neem contact op met [Wizergos productiviteitssoftware ondersteuningsteam](mailTo:support@wizergos.com) om toe te voegen de gebruikers in het platform Software voor Wizergos productiviteit.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Wizergos productiviteitssoftware.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon Wizergos productiviteit software, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Wizergos productiviteitssoftware**.

    ![De koppeling Wizergos productiviteitssoftware in de lijst met toepassingen](./media/wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Wizergos productiviteitssoftware in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Wizergos productiviteit.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/wizergosproductivitysoftware-tutorial/tutorial_general_01.png
[2]: ./media/wizergosproductivitysoftware-tutorial/tutorial_general_02.png
[3]: ./media/wizergosproductivitysoftware-tutorial/tutorial_general_03.png
[4]: ./media/wizergosproductivitysoftware-tutorial/tutorial_general_04.png

[100]: ./media/wizergosproductivitysoftware-tutorial/tutorial_general_100.png

[200]: ./media/wizergosproductivitysoftware-tutorial/tutorial_general_200.png
[201]: ./media/wizergosproductivitysoftware-tutorial/tutorial_general_201.png
[202]: ./media/wizergosproductivitysoftware-tutorial/tutorial_general_202.png
[203]: ./media/wizergosproductivitysoftware-tutorial/tutorial_general_203.png

