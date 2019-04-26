---
title: 'Zelfstudie: een Azure Active Directory B2C-tenant maken | Microsoft Docs'
description: Informatie over het voorbereiden voor het registreren van uw toepassingen door het maken van een Azure Active Directory B2C-tenant met behulp van de Azure portal.
services: B2C
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 01/11/2019
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: b8878eccb079bf78c45ff9c1e4040659d109b1ab
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60360158"
---
# <a name="tutorial-create-an-azure-active-directory-b2c-tenant"></a>Zelfstudie: Een Azure Active Directory B2C-tenant maken

Voordat u uw toepassingen kunnen communiceren met Azure Active Directory (Azure AD) B2C, moeten deze worden geregistreerd in een tenant die u beheert.

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Een Azure AD B2C-tenant maken
> * Uw tenant koppelt aan uw abonnement

Leert u hoe u een toepassing registreren in de volgende zelfstudie.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="create-an-azure-ad-b2c-tenant"></a>Een Azure AD B2C-tenant maken

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Zorg ervoor dat u van de map waarin u uw abonnement gebruikmaakt door te klikken op de **map- en abonnementsfilter** in het bovenste menu en het kiezen van de map waarvan deze deel uitmaakt. Deze map is anders dan die het bevat uw Azure AD B2C-tenant.

    ![Schakel over naar de abonnementsmap](./media/tutorial-create-tenant/switch-directory-subscription.png)

3. Kies **een resource maken** in de linkerbovenhoek van Azure portal.
4. Zoek en selecteer **Active Directory B2C**, en klik vervolgens op **maken**.
5. Kies **maken van een nieuwe Azure AD B2C-Tenant**, voer een naam van de organisatie en de initiële domeinnaam die wordt gebruikt in de naam van de tenant, selecteert u het land (deze kan niet later worden gewijzigd) en klik vervolgens op **maken**.

    ![Een tenant maken](./media/tutorial-create-tenant/create-tenant.png)

    In dit voorbeeld is de naam van de tenant contoso0926Tenant.onmicrosoft.com

6. Op de **nieuwe B2C-Tenant maken of een koppeling naar een bestaande Tenant** pagina, kies **koppeling een bestaande Azure AD B2C-Tenant aan mijn Azure-abonnement**, selecteert u de tenant die u hebt gemaakt, wordt uw abonnement te selecteren en vervolgens Klik op **nieuw**.
7. Voer een naam voor de resourcegroep die wordt bevatten van de tenant, selecteert u de locatie en klik vervolgens op **maken**.
8. Als u wilt gaan met deze nieuwe tenant, zorg ervoor dat u de map met uw Azure AD B2C-tenant door te klikken op de **map- en abonnementsfilter** in het bovenste menu en het kiezen van de map waarvan deze deel uitmaakt.

    ![Overschakelen naar de directory-tenant](./media/tutorial-create-tenant/switch-directories.png)

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u het volgende geleerd:

> [!div class="checklist"]
> * Een Azure AD B2C-tenant maken
> * Uw tenant koppelt aan uw abonnement

> [!div class="nextstepaction"]
> [Uw toepassingen registreren](tutorial-register-applications.md)
