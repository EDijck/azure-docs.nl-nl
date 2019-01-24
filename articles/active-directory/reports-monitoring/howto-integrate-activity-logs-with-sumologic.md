---
title: Over het integreren van Azure Active Directory-logboeken met SumoLogic met behulp van Azure Monitor (preview) | Microsoft Docs
description: Meer informatie over het integreren van Azure Active Directory-logboeken met SumoLogic met behulp van Azure Monitor (preview)
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.assetid: 2c3db9a8-50fa-475a-97d8-f31082af6593
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 164026cc1abea43760d06024ded5c083a92160f4
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54810598"
---
# <a name="integrate-azure-active-directory-logs-with-sumologic-using-azure-monitor-preview"></a>Logboeken van Azure Active Directory integreren met SumoLogic met behulp van Azure Monitor (preview)

In dit artikel leert u hoe u Azure Active Directory (Azure AD)-logboeken integreren met SumoLogic met behulp van Azure Monitor. U eerst de logboeken versturen naar een Azure event hub, en vervolgens het integreren van de event hub met SumoLogic.

## <a name="prerequisites"></a>Vereisten

U hebt het volgende nodig om deze functie te gebruiken:
* Een Azure event hub met Azure AD-activiteit zich aanmeldt. Meer informatie over het [uw activiteitenlogboeken streamen naar een event hub streamen](quickstart-azure-monitor-stream-logs-to-event-hub.md). 
* Een SumoLogic eenmalige aanmelding ingeschakeld abonnement.

## <a name="steps-to-integrate-azure-ad-logs-with-sumologic"></a>Stappen voor het integreren van Azure AD-logboeken met SumoLogic 

1. Eerste, [stream het Azure AD-logboeken naar een Azure event hub](quickstart-azure-monitor-stream-logs-to-event-hub.md).
2. Configureren van uw exemplaar SumoLogic naar [verzamelen van Logboeken voor Azure Active Directory](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure_Active_Directory/Collect_Logs_for_Azure_Active_Directory).
3. [Installeer de app Azure AD SumoLogic](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure_Active_Directory/Install_the_Azure_Active_Directory_App_and_View_the_Dashboards) het gebruik van de vooraf geconfigureerde dashboards die voorzien in realtime analyse van uw omgeving.

 ![Dashboard](./media/howto-integrate-activity-logs-with-sumologic/overview-dashboard.png)

## <a name="next-steps"></a>Volgende stappen

* [Interpret audit logs schema in Azure Monitor](reference-azure-monitor-audit-log-schema.md) (Auditlogboekenschema interpreteren in Azure Monitor)
* [Interpret sign-in logs schema in Azure Monitor](reference-azure-monitor-sign-ins-log-schema.md) (Aanmeldingslogboekenschema interpreteren in Azure Monitor)
* [Veelgestelde vragen en bekende problemen](concept-activity-logs-azure-monitor.md#frequently-asked-questions)
