---
title: Azure-VM's migreren naar Managed Disks | Microsoft Docs
description: Migreer Azure virtuele machines die zijn gemaakt met behulp van niet-beheerde schijven in de storage-accounts naar beheerde schijven.
services: virtual-machines-windows
documentationcenter: ''
author: roygara
manager: twooley
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/03/2018
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 12cd1caa4cb96dbd5862776589d4a34aeb294ca1
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2019
ms.locfileid: "64689720"
---
# <a name="migrate-azure-vms-to-managed-disks-in-azure"></a>Azure-VM's migreren naar Managed Disks in Azure

Azure Managed Disks vereenvoudigt het beheer van uw opslag door het verwijderen van de noodzaak voor het storage-accounts afzonderlijk beheren.  U kunt ook uw bestaande Azure-VM's migreren naar Managed Disks profiteren van betere betrouwbaarheid van virtuele machines in een Beschikbaarheidsset. Dit zorgt ervoor dat de schijven van verschillende virtuele machines in een Beschikbaarheidsset voldoende geïsoleerd van elkaar om te voorkomen dat één punt van fouten. Deze plaatst automatisch schijven van verschillende virtuele machines in een Beschikbaarheidsset in verschillende kasten (stempels) zodat ze worden beperkt de gevolgen van één opslag scale unit storingen veroorzaakt door hardware en software.
Op basis van uw behoeften, kunt u kiezen uit vier verschillende opties voor opslag. Zie voor meer informatie over de typen beschikbare schijfruimte, dan het artikel [een schijf selecteren](disks-types.md)

## <a name="migrate-scenarios"></a>Scenario's migreren

U kunt migreren naar Managed Disks in de volgende scenario's:

| **Migreren...**                                            | **Koppeling voor documentatie**                                                                                                                                                                                                                                                                  |
|----------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Zelfstandige virtuele machines en virtuele machines in een beschikbaarheidsset naar beheerde schijven converteren   | [Virtuele machines voor het gebruik van beheerde schijven converteren](convert-unmanaged-to-managed-disks.md) |
| Eén VM van klassiek naar Resource Manager op beheerde schijven     | [Een virtuele machine maken van een klassieke VHD](create-vm-specialized-portal.md)  | 
| Alle VM's van klassiek naar Resource Manager op beheerde schijven     | [Migreren van IaaS-resources van klassiek naar Resource Manager](migration-classic-resource-manager-ps.md) en vervolgens [een VM van niet-beheerde schijven converteren naar beheerde schijven](convert-unmanaged-to-managed-disks.md) | 


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [Managed Disks](managed-disks-overview.md)
- Controleer de [prijzen voor Managed Disks](https://azure.microsoft.com/pricing/details/managed-disks/).
