---
title: Reserveringen voor Azure-resources weergeven | Microsoft Docs
description: Leer hoe u Azure reserveringen weergeven in Azure portal.
services: billing
documentationcenter: ''
author: yashesvi
manager: yashar
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/03/2018
ms.author: banders
ms.openlocfilehash: bf18d845b7128c8d6f740555f1a0f791767240ae
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58650220"
---
# <a name="view-azure-reservations-in-the-azure-portal"></a>Azure reserveringen weergeven in Azure portal

Afhankelijk van uw abonnementstype en machtigingen zijn er een aantal manieren om weer te geven reserveringen voor Azure.

## <a name="view-reservations-as-owner-or-reader"></a>Reserveringen weergeven als de eigenaar of lezer

Standaard, wanneer u een reservering koopt vindt u en de accountbeheerder de reservering. U en de accountbeheerder krijgt automatisch de rol van eigenaar van de reservering. Als u wilt dat anderen om de reservering weer te geven, moet u ze als toevoegen een **eigenaar** of **lezer** op de reservering. Zie voor meer informatie, [toevoegen of wijzigen van de gebruikers die een reservering kunnen beheren](billing-manage-reserved-vm-instance.md#add-or-change-users-who-can-manage-a-reservation).
 
Om een reservering als een eigenaar of lezer, weer te geven

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Zoeken op **reserveringen**.

    ![Schermafbeelding van zoeken in Azure portal](./media/billing-view-reservation/portal-reservation-search.png)

1. U ziet een lijst van de reserveringen waar u de rol eigenaar of lezer hebt.

Als u wilt wijzigen van het bereik van een reservering, wijzigen die een reservering kan beheren of gesplitste een reservering Zie [Azure-reserveringen beheren](billing-manage-reserved-vm-instance.md).

## <a name="view-reservation-transactions-for-enterprise-enrollments"></a>Reservering-transacties voor Enterprise-inschrijvingen weergeven

 Als u een partner onder leiding van Enterprise-inschrijving hebt, reserveringen weergeven door te gaan naar **rapporten** in de EA-portal. Voor andere Enterprise-inschrijvingen, kunt u reserveringen weergeven in de EA-portal en in de Azure-portal. U moet een EA-beheerder om reservering transacties weer te geven.

Transacties van de reservering bekijken in Azure-portal

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Zoeken op **kosten Management en facturering**.

    ![Schermafbeelding van zoeken in Azure portal](./media/billing-view-reservation/portal-cm-billing-search.png)

1. Selecteer **reservering transacties**.
1. Als u wilt de resultaten te filteren, selecteert u **Timespan**, **Type**, of **beschrijving**.
1. Selecteer **Toepassen**.

    ![Schermafbeelding van reservering transacties resultaten](./media/billing-view-reservation/portal-billing-reservation-transaction-results.png)

Als u de gegevens met behulp van een API, Zie [gereserveerde instantie ophalen transactiekosten in rekening gebracht voor zakelijke klanten](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-charges).

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Azure reserveringen, de volgende artikelen:

- [Wat zijn de reserveringen voor Azure?](billing-save-compute-costs-reservations.md)
- [Reserveringen voor Azure beheren](billing-manage-reserved-vm-instance.md)

Een service-abonnement kopen:

- [Betaal vooruit voor Cosmos DB gereserveerde capaciteit](../cosmos-db/cosmos-db-reserved-capacity.md)
- [Vooruitbetalen voor compute-resources van SQL Database met gereserveerde capaciteit voor Azure SQL Database](../sql-database/sql-database-reserved-capacity.md)
- [Vooruitbetalen voor Virtual Machines met Azure Reserved VM Instances](../virtual-machines/windows/prepay-reserved-vm-instances.md)

Koop een softwareabonnement:

- [Betaal vooruit voor Red Hat software plannen uit de Azure-reserveringen](../virtual-machines/linux/prepay-rhel-software-charges.md)
- [Vooruitbetalen voor SUSE-software-abonnementen vanuit Azure Reservations](../virtual-machines/linux/prepay-suse-software-charges.md)

Inzicht in gebruik:

- [Gebruik van de reservering voor uw abonnement op gebruiksbasis begrijpen](billing-understand-reserved-instance-usage.md)
- [Inzicht in gebruik van de reservering voor uw Enterprise-inschrijving](billing-understand-reserved-instance-usage-ea.md)
- [Informatie over het gebruik van de reservering voor CSP-abonnementen](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-us"></a>Hulp nodig? Contact opnemen

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://go.microsoft.com/fwlink/?linkid=2083458).
