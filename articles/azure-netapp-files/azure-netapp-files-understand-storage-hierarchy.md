---
title: Wat is de hiërarchie van de opslag van Azure NetApp Files | Microsoft Docs
description: In dit artikel wordt de opslaghiërarchie beschreven, waaronder Azure NetApp Files-accounts, capaciteitspools en volumes.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 04/16/2019
ms.author: b-juche
ms.openlocfilehash: fec9e22b15eca3f95be606776066cf573046b5fd
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2019
ms.locfileid: "64687483"
---
# <a name="what-is-the-storage-hierarchy-of-azure-netapp-files"></a>Wat is de hiërarchie van de opslag van Azure NetApp Files

Voordat u een volume maakt in Azure NetApp Files, moet u een pool kopen en instellen voor de ingerichte capaciteit.  U heeft een NetApp-account nodig om een capaciteitspool in te stellen. Door inzicht te krijgen in de opslaghiërarchie kunt u uw Azure NetApp Files-resources beter instellen en beheren.

## <a name="azure_netapp_files_account"></a>NetApp-accounts

- Een NetApp-account fungeert als een beheergroep van de bijbehorende capaciteitspools.  
- Een NetApp-account is niet hetzelfde als uw algemene Azure-opslagaccount. 
- Een NetApp-account heeft een regionaal bereik.   
- U kunt meerdere NetApp-accounts hebben in een regio, maar elk NetApp-account is gekoppeld aan slechts één regio.

## <a name="capacity_pools"></a>Capaciteitspools

- Een capaciteitspool wordt gemeten aan de hand van de ingerichte capaciteit.  
- De capaciteit wordt ingericht met de vaste SKU's die u hebt gekocht (bijvoorbeeld een capaciteit van 4 TiB).
- Een capaciteitspool kan slechts één serviceniveau hebben.  
- Elke capaciteitspool kan deel uitmaken van slechts één NetApp-account. Er kunnen echter meerdere capaciteitspools binnen een NetApp-account bestaan.  
- Een capaciteitspool kan niet worden verplaatst tussen NetApp-accounts.   
  In het onderstaande [conceptueel diagram van een opslaghiërarchie](#conceptual_diagram_of_storage_hierarchy) kan Capaciteitspool 1 bijvoorbeeld niet worden verplaatst van het NetApp-account in US oost naar het NetApp-account in US west 2.  

## <a name="volumes"></a>Volumes

- Een volume wordt gemeten aan de hand van het logische capaciteitsverbruik en is schaalbaar. 
- Capaciteitsgebruik van een volume wordt in mindering gebracht op de ingerichte capaciteit van de pool.
- Elk volume is gekoppeld aan slechts één pool, maar een pool kan meerdere volumes bevatten. 
- U kunt een volume binnen hetzelfde NetApp-account tussen verschillende pools verplaatsen.    
  In het onderstaande [conceptueel diagram van een opslaghiërarchie](#conceptual_diagram_of_storage_hierarchy) kunt u de volumes van Capaciteitspool 1 verplaatsen naar Capaciteitspool 2.

## <a name="conceptual_diagram_of_storage_hierarchy"></a>Conceptueel diagram van een opslaghiërarchie 
Het volgende voorbeeld toont de relaties tussen een Azure-abonnement, NetApp-accounts, capaciteitspools en volumes.   

![Conceptueel diagram van een opslaghiërarchie](../media/azure-netapp-files/azure-netapp-files-storage-hierarchy.png)

## <a name="next-steps"></a>Volgende stappen

- [Resourcelimieten voor Azure NetApp Files](azure-netapp-files-resource-limits.md)
- [Registreren voor Azure NetApp Files](azure-netapp-files-register.md)
