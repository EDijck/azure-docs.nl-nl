---
title: 'Zelfstudie: Azure Active Directory-integratie met Five9 Plus-Adapter (CTI, neem contact op met Center-Agents) | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Five9 Plus Adapter (CTI, neem contact op met Center-Agents).
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 88dc82ab-be0b-4017-8335-c47d00775d7b
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/04/2019
ms.author: jeedes
ms.openlocfilehash: daec6e169805c193b48781dfecbabd9349bdc59b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60278672"
---
# <a name="tutorial-azure-active-directory-integration-with-five9-plus-adapter-cti-contact-center-agents"></a>Zelfstudie: Azure Active Directory-integratie met Five9 Plus-Adapter (CTI, neem contact op met Center-Agents)

In deze zelfstudie leert u hoe u Five9 Plus Adapter (CTI, neem contact op met Center-Agents) integreren met Azure Active Directory (Azure AD).
Five9 Plus Adapter (CTI, neem contact op met Center-Agents) integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot Five9 Plus Adapter (CTI, neem contact op met Center-Agents heeft).
* U kunt uw gebruikers worden automatisch aangemeld bij Five9 Plus Adapter (CTI, neem contact op met Center-Agents) inschakelen (Single Sign-On) met hun Azure AD-accounts.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Five9 Plus-Adapter (CTI, neem contact op met Center-Agents), moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/).
* Eenmalige aanmelding Five9 Plus Adapter (CTI, neem contact op met Center-Agents) ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Five9 Plus Adapter (CTI, neem contact op met Center-Agents) ondersteunt **IDP** gestart door SSO

## <a name="adding-five9-plus-adapter-cti-contact-center-agents-from-the-gallery"></a>Five9 Plus Adapter (CTI, neem contact op met Center-Agents) uit de galerie toevoegen

Als u wilt de integratie van Five9 Plus Adapter (CTI, neem contact op met Center-Agents) in Azure AD configureert, moet u Five9 Plus netwerkadapter (CTI, neem contact op met Center-Agents) worden toegevoegd vanuit de galerie aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Five9 Plus Adapter (CTI, neem contact op met Center-Agents) uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **Five9 Plus Adapter (CTI, neem contact op met Center-Agents)**, selecteer **Five9 Plus Adapter (CTI, neem contact op met Center-Agents)** van resultaat deelvenster klik vervolgens op **toevoegen** knop om toe te voegen van de toepassing.

     ![Five9 Plus-Adapter (CTI, neem contact op met Center-Agents) in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Five9 Plus-Adapter (CTI, neem contact op met Center-Agents) op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker Five9 Plus adapter (CTI, neem contact op met Center-Agents) tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Five9 Plus-Adapter (CTI, neem contact op met Center-Agents), moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureren van eenmalige aanmelding Five9 Plus Adapter (CTI, neem contact op met Center-Agents)](#configure-five9-plus-adapter-cti-contact-center-agents-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maak de testgebruiker Five9 Plus Adapter (CTI, neem contact op met Center-Agents)](#create-five9-plus-adapter-cti-contact-center-agents-test-user)**  : als u wilt een equivalent van Britta Simon in Five9 Plus-Adapter (CTI, neem contact op met Center-Agents) die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met Five9 Plus-Adapter (CTI, neem contact op met Center-Agents), moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **Five9 Plus Adapter (CTI, neem contact op met Center-Agents)** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. Op de pagina **Eenmalige aanmelding instellen met SAML** voert u de volgende stappen uit:

    ![Five9 Plus Adapter (CTI, neem contact op met Center-Agents)-domein en URL's, eenmalige aanmelding informatie](common/idp-intiated.png)

    a. In het tekstvak **Id** typt u een URL met het volgende patroon:
    
    |    Omgeving      |       URL      |
    | :-- | :-- |
    | Voor 'Five9 Plus -Adapter voor Microsoft Dynamics CRM' | `https://app.five9.com/appsvcs/saml/metadata/alias/msdc` |
    | Voor "Five9 Plus -Adapter voor Zendesk" | `https://app.five9.com/appsvcs/saml/metadata/alias/zd` |
    | Voor "Five9 Plus -Adapter voor Agent bureaublad Toolkit" | `https://app.five9.com/appsvcs/saml/metadata/alias/adt` |

    b. In het tekstvak **Antwoord-URL** typt u een URL met het volgende patroon:

    |      Omgeving     |      URL      |
    | :--                  | :--           |
    | Voor 'Five9 Plus -Adapter voor Microsoft Dynamics CRM' | `https://app.five9.com/appsvcs/saml/SSO/alias/msdc` |
    | Voor "Five9 Plus -Adapter voor Zendesk" | `https://app.five9.com/appsvcs/saml/SSO/alias/zd` |
    | Voor "Five9 Plus -Adapter voor Agent bureaublad Toolkit" | `https://app.five9.com/appsvcs/saml/SSO/alias/adt` |

6. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

7. Op de **Five9 Plus Adapter (CTI, neem contact op met Center-Agents) instellen** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-five9-plus-adapter-cti-contact-center-agents-single-sign-on"></a>Five9 Plus Adapter (CTI, neem contact op met Center-Agents) Single Sign-On configureren

1. Het configureren van eenmalige aanmelding op **Five9 Plus Adapter (CTI, neem contact op met Center-Agents)** zijde, moet u voor het verzenden van de gedownloade **Certificate(Base64)** en toepassing van de gekopieerde URL('s) te [Five9 Plus Netwerkadapter (CTI, neem contact op met Center-Agents)-ondersteuningsteam](https://www.five9.com/about/contact). Bovendien ook voor het configureren van eenmalige aanmelding verder Volg de onderstaande stappen op basis van de adapter:

    a. 'Five9 Plus -Adapter voor bureaublad Toolkit Agent' beheerdershandleiding voor de: [https://webapps.five9.com/assets/files/for_customers/documentation/integrations/agent-desktop-toolkit/plus-agent-desktop-toolkit-administrators-guide.pdf](https://webapps.five9.com/assets/files/for_customers/documentation/integrations/agent-desktop-toolkit/plus-agent-desktop-toolkit-administrators-guide.pdf)
    
    b. 'Five9 Plus -Adapter voor Microsoft Dynamics CRM' beheerdershandleiding voor de: [https://webapps.five9.com/assets/files/for_customers/documentation/integrations/microsoft/microsoft-administrators-guide.pdf](https://webapps.five9.com/assets/files/for_customers/documentation/integrations/microsoft/microsoft-administrators-guide.pdf)
    
    c. "Five9 Plus -Adapter voor Zendesk" beheerdershandleiding voor de: [https://webapps.five9.com/assets/files/for_customers/documentation/integrations/zendesk/zendesk-plus-administrators-guide.pdf](https://webapps.five9.com/assets/files/for_customers/documentation/integrations/zendesk/zendesk-plus-administrators-guide.pdf)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken 

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In de **gebruikersnaam** veldtype `brittasimon@yourcompanydomain.extension`. Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Five9 Plus Adapter (CTI, neem contact op met Center-Agents).

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **Five9 Plus Adapter (CTI, neem contact op met Center-Agents)**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **Five9 Plus Adapter (CTI, neem contact op met Center-Agents)**.

    ![De koppeling Five9 Plus Adapter (CTI, neem contact op met Center-Agents) in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-five9-plus-adapter-cti-contact-center-agents-test-user"></a>Testgebruiker Five9 Plus Adapter (CTI, neem contact op met Center-Agents) maken

In deze sectie maakt u een gebruiker met de naam van Britta Simon Five9 Plus adapter (CTI, neem contact op met Center-Agents). Werken met [Five9 Plus (CTI, neem contact op met Center-Agents) ondersteuning adapterteam](https://www.five9.com/about/contact) om toe te voegen de gebruikers in het platform Five9 Plus Adapter (CTI, neem contact op met Center-Agents). Er moeten gebruikers worden gemaakt en geactiveerd voordat u eenmalige aanmelding kunt gebruiken. 

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u klikt op de Five9 Plus-Adapter (CTI, neem contact op met Center-Agents tegel in het toegangsvenster, u moet worden automatisch aangemeld bij de Five9 Plus-Adapter (CTI, neem contact op met Center-Agents) waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

