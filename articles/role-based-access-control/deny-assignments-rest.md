---
title: Lijst weigeren toewijzingen voor Azure-resources met behulp van de REST-API - Azure | Microsoft Docs
description: Meer informatie over het lijst weigeren toewijzingen voor gebruikers, groepen en toepassingen met behulp van op rollen gebaseerd toegangsbeheer (RBAC) voor Azure-resources en de REST-API.
services: active-directory
documentationcenter: na
author: rolyon
manager: mtillman
editor: ''
ms.assetid: ''
ms.service: role-based-access-control
ms.workload: multiple
ms.tgt_pltfrm: rest-api
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: ba1c60d45fb53be158d9e302748366ddf417f23e
ms.sourcegitcommit: 7f7c2fe58c6cd3ba4fd2280e79dfa4f235c55ac8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/25/2019
ms.locfileid: "56805474"
---
# <a name="list-deny-assignments-for-azure-resources-using-the-rest-api"></a>Lijst weigeren toewijzingen voor Azure-resources met behulp van de REST-API

Op dit moment weigeren toewijzingen worden **alleen-lezen** en kan alleen worden ingesteld door Microsoft. U kunt geen eigen weigeringstoewijzingen maken, maar u kunt wel een lijst met weigeringstoewijzingen weergeven omdat deze invloed kunnen hebben op uw effectieve machtigingen. Dit artikel wordt beschreven hoe u om weer te geven met RBAC en de REST-API toewijzingen weigeren.

## <a name="list-a-single-deny-assignment"></a>Lijst met één opdracht weigeren

1. Beginnen met de volgende aanvraag:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/denyAssignments/{deny-assignment-id}?api-version=2018-07-01-preview
    ```

1. Vervang in de URI, *{bereik}* met het bereik waarvoor u wilt weergeven van de toewijzingen weigeren.

    | Bereik | Type |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonnement |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Resourcegroep |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Resource |

1. Vervang *{weigeren-toewijzing-id}* met de weigeren toewijzing-id die u wilt ophalen.

## <a name="list-multiple-deny-assignments"></a>Lijst met meerdere toewijzingen weigeren

1. Begin met een van de volgende aanvragen:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview
    ```

    Met de volgende optionele parameters:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview&$filter={filter}
    ```

1. Vervang in de URI, *{bereik}* met het bereik waarvoor u wilt weergeven van de toewijzingen weigeren.

    | Bereik | Type |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonnement |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Resourcegroep |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Resource |

1. Vervang *{filter}* met de voorwaarde die u wilt toepassen op filter de lijst weigeren toewijzing.

    | Filteren | Description |
    | --- | --- |
    | (geen filter) | Lijst met alle weigeren toewijzingen aan, boven en onder het opgegeven bereik. |
    | `$filter=atScope()` | Lijst weigeren toewijzingen voor alleen het opgegeven bereik en hoger. Geen toewijzingen op subscopes weigeren. |
    | `$filter=denyAssignmentName%20eq%20'{deny-assignment-name}'` | Lijst weigeren toewijzingen met de opgegeven naam. |

## <a name="list-deny-assignments-at-the-root-scope-"></a>Lijst met toewijzingen voor de scope hoofdmap (/) weigeren

1. Uw toegangsrechten zoals beschreven in [toegangsrechten voor een globale beheerder in Azure Active Directory](elevate-access-global-admin.md).

1. Gebruik de volgende aanvraag:

    ```http
    GET https://management.azure.com/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview&$filter={filter}
    ```

1. Vervang *{filter}* met de voorwaarde die u wilt toepassen op filter de lijst weigeren toewijzing. Een filter is vereist.

    | Filteren | Description |
    | --- | --- |
    | `$filter=atScope()` | Lijst weigeren toewijzingen voor alleen het bereik van de hoofdmap. Geen toewijzingen op subscopes weigeren. |
    | `$filter=denyAssignmentName%20eq%20'{deny-assignment-name}'` | Lijst weigeren toewijzingen met de opgegeven naam. |

1. Verwijder toegang met verhoogde bevoegdheid.

## <a name="next-steps"></a>Volgende stappen

- [Informatie over toewijzingen voor Azure-resources te weigeren](deny-assignments.md)
- [Toegangsbevoegdheid voor een globale beheerder verhogen in Azure Active Directory](elevate-access-global-admin.md)
- [Azure REST API-naslaginformatie](/rest/api/azure/)
