---
title: 'Zelfstudie: Azure Active Directory-integratie met Dossier | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Dossier.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 7a5fec92-9c01-4ced-99b2-a10e28fc028e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: dd69616fa544c1d13e14eb8eebb92adb6350d9e8
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56217527"
---
# <a name="tutorial-azure-active-directory-integration-with-dossier"></a>Zelfstudie: Azure Active Directory-integratie met Dossier

In deze zelfstudie leert u hoe u Dossier integreren met Azure Active Directory (Azure AD).

Dossier integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Dossier heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Dossier (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Dossier, moet u de volgende items:

- Een Azure AD-abonnement
- Een Dossier eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Dossier uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-dossier-from-the-gallery"></a>Dossier uit de galerie toe te voegen

Voor het configureren van de integratie van Dossier in Azure AD, moet u Dossier uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Dossier uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

4. Typ in het zoekvak **Dossier**, selecteer **Dossier** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Dossier in de lijst met resultaten](./media/dossier-tutorial/tutorial_dossier_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureren en testen Azure AD eenmalige aanmelding met Dossier op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Dossier is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Dossier tot stand worden gebracht.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met Dossier, u nodig hebt voor de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
3. **[Maak een testgebruiker Dossier](#create-a-dossier-test-user)**  : als u wilt een equivalent van Britta Simon in Dossier die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Dossier.

**Voor het configureren van Azure AD eenmalige aanmelding met Dossier, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Dossier** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.

    ![In het dialoogvenster voor eenmalige aanmelding](./media/dossier-tutorial/tutorial_dossier_samlbase.png)

3. Op de **Dossier domein en URL's** sectie, voert u de volgende stappen uit:

    ![Dossier domein en URL's, eenmalige aanmelding informatie](./media/dossier-tutorial/tutorial_dossier_url1.png)

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met het volgende patroon:
    
    | | |
    |-|-|
    | `https://<SUBDOMAIN>.dossiersystems.com/azuresso/account/SignIn`|
    | `https://dossier.<CLIENTDOMAINNAME>/azuresso/account/SignIn`|
    | |

    b. Typ in het tekstvak **Id** een URL met het volgende patroon: `Dossier/<CLIENTNAME>`

    > [!NOTE]
    > Voor id-waarde moet in de indeling van `Dossier/<CLIENTNAME>` of elke gebruiker persoonlijke waarde.

    c. In het tekstvak **Antwoord-URL** typt u een URL met de volgende notatie:
    | | |
    |-|-|
    |  `https://<SUBDOMAIN>.dossiersystems.com/azuresso`|
    | `https://dossier.<CLIENTDOMAINNAME>/azuresso`|
    | |

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke id, antwoord-URL en aanmeldings-URL. Neem contact op met [Dossier Client ondersteuningsteam](mailto:support@intellimedia.ca) om deze waarden te verkrijgen.

4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op de knop kopiëren om te kopiëren **App-Url voor federatieve metagegevens** en plak deze in Kladblok.

    ![De link om het certificaat te downloaden](./media/dossier-tutorial/tutorial_dossier_certificate.png) 

6. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/dossier-tutorial/tutorial_general_400.png)

7. Het configureren van eenmalige aanmelding op **Dossier** zijde, moet u voor het verzenden van de **App-Url voor federatieve metagegevens** naar [Dossier ondersteuningsteam](mailto:support@intellimedia.ca). Het team stelt de instellingen zo in dat de verbinding tussen SAML en eenmalige aanmelding aan beide zijden goed is ingesteld.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/dossier-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/dossier-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/dossier-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/dossier-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.

### <a name="create-a-dossier-test-user"></a>Maak een testgebruiker Dossier

In deze sectie maakt u een gebruiker met de naam van Britta Simon in Dossier. Werken met [Dossier ondersteuningsteam](mailto:support@intellimedia.ca) om toe te voegen de gebruikers in het Dossier-platform. Er moeten gebruikers worden gemaakt en geactiveerd voordat u eenmalige aanmelding kunt gebruiken.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Dossier.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon aan Dossier toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Dossier**.

    ![De koppeling Dossier in de lijst met toepassingen](./media/dossier-tutorial/tutorial_dossier_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Dossier in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Dossier.
Zie [Introduction to the Access Panel](../user-help/active-directory-saas-access-panel-introduction.md) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/dossier-tutorial/tutorial_general_01.png
[2]: ./media/dossier-tutorial/tutorial_general_02.png
[3]: ./media/dossier-tutorial/tutorial_general_03.png
[4]: ./media/dossier-tutorial/tutorial_general_04.png

[100]: ./media/dossier-tutorial/tutorial_general_100.png

[200]: ./media/dossier-tutorial/tutorial_general_200.png
[201]: ./media/dossier-tutorial/tutorial_general_201.png
[202]: ./media/dossier-tutorial/tutorial_general_202.png
[203]: ./media/dossier-tutorial/tutorial_general_203.png

