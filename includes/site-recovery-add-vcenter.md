---
author: rayne-wiselman
ms.service: site-recovery
ms.topic: include
ms.date: 10/26/2018
ms.author: raynew
ms.openlocfilehash: 926fb3e9a2c09d30da549330842d8b7e185674ae
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60600432"
---
Geef in **vCenter toevoegen** een beschrijvende naam op voor de vSphere-host of vCenter-server, en geef vervolgens het IP-adres of de FQDN op voor de server. Behoud poort 443 tenzij de VMware-servers zijn geconfigureerd om te luisteren naar aanvragen van een andere poort. Selecteer het account dat is verbonden met de VMware vCenter-of vSphere ESXi-server. Klik op **OK**.

    ![VMware](./media/site-recovery-add-vcenter/vmware-server.png)

   > [!NOTE]
   > Als u toevoegen wilt de VMware vCenter-server of VMware vSphere-host met een account dat geen beheerrechten hebben op de vCenter- of host-server, zorg ervoor dat heeft het account deze bevoegdheden zijn ingeschakeld: Datacenter, gegevensopslag, map, Host, netwerk, Resource, virtuele machine en vSphere gedistribueerde Switch. Daarnaast moet voor de VMware vCenter-server de bevoegdheid Opslagweergaven zijn ingeschakeld.
