---
title: Azure Stack-firewall plannen voor Azure Stack-geïntegreerde systemen | Microsoft Docs
description: Beschrijft de aandachtspunten voor de firewall van Azure Stack voor implementaties met meerdere knooppunten Azure Stack Azure-verbinding.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/12/2019
ms.author: jeffgilb
ms.reviewer: wfayed
ms.lastreviewed: 10/15/2018
ms.openlocfilehash: 023201d221ee5d7ec884c6a760407e8da8340d3f
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56207115"
---
# <a name="azure-stack-firewall-integration"></a>Azure Stack-firewall-integratie
Het verdient aanbeveling dat u een firewall-apparaat gebruiken om te beveiligen Azure Stack. Firewalls kunnen beschermen tegen zaken zoals gedistribueerde denial-of-service (DDOS) aanvallen, inbraakdetectie en inhoudsinspectie. Maar kunnen ze ook een knelpunt doorvoer voor Azure storage-services, zoals blobs, tabellen en wachtrijen.

 Als een niet-verbonden implementatiemodus wordt gebruikt, moet u het eindpunt van AD FS publiceren. Zie voor meer informatie de [datacenter-integratie identiteit artikel](azure-stack-integrate-identity.md).

De Azure Resource Manager (beheerder), de beheerdersportal en de Key Vault (beheerder)-eindpunten vereisen per se geen externe publiceren. Als een serviceprovider kunt u de kwetsbaarheid voor aanvallen kan beperken door alleen beheren van Azure Stack uit in uw netwerk en niet vanaf het internet.

Voor ondernemingen, kan het externe netwerk het bestaande bedrijfsnetwerk zijn. In dit scenario, moet u de eindpunten voor het gebruik van Azure Stack van het bedrijfsnetwerk publiceren.

### <a name="network-address-translation"></a>Network Address Translation
NAT (Network Address Translation) is de aanbevolen methode om toe te staan van de implementatie van virtuele machine (DVM) voor toegang tot externe resources en het internet tijdens de implementatie, evenals de Emergency Recovery Console (ERCS) virtuele machines of bevoegde eindpunt (PEP) tijdens registratie en het oplossen van problemen.

NAT kan ook een alternatief voor het openbare IP-adressen op het externe netwerk of de openbare VIP's zijn. Het is echter niet aanbevolen om dit te doen omdat het beperken van de gebruikerservaring van de tenant en verhoogt de complexiteit. Een optie is een één-op-een NAT die nog steeds een openbaar IP-adres per gebruiker IP-adres op de groep vereist. Een andere optie is een veel-op-één NAT waarvoor een NAT-regel per gebruiker VIP voor alle poorten, een gebruiker kan gebruiken.

Enkele van de nadelen van het gebruik van NAT voor openbaar VIP-adres zijn:
- NAT wordt overhead toegevoegd bij het beheren van firewall-regels, omdat gebruikers hun eigen eindpunten en hun eigen regels voor publicatie in de stack software gedefinieerde netwerken (SDN beheren). Gebruikers moeten contact opnemen met de Azure Stack-operators om hun VIP's gepubliceerd en bijwerken van de lijst met poorten.
- Zolang de NAT-gebruik beperkt de gebruikerservaring, biedt het volledig beheer met de operator over het publiceren van aanvragen.
- Overweeg voor hybride cloud met Azure, Azure biedt geen ondersteuning voor het instellen van een VPN-tunnel naar een eindpunt met behulp van NAT bevinden.

### <a name="ssl-decryption"></a>SSL-decodering
Het is momenteel aanbevolen om uit te schakelen SSL ontsleutelen op alle Azure Stack-verkeer. Als dit wordt ondersteund in toekomstige updates, wordt informatie over het inschakelen van SSL-ontsleuteling voor Azure Stack worden opgegeven.

## <a name="edge-firewall-scenario"></a>Scenario voor edge-firewall
Azure Stack is geïmplementeerd in een edge-implementatie, rechtstreeks achter de edge router of de firewall. In deze scenario's wordt ondersteund voor de firewall om te worden boven de rand (Scenario 1), waar deze ondersteunt zowel actief / actief en actief-passief firewallconfiguraties of fungeert als het apparaat van rand (Scenario 2) waar het ondersteunt alleen actief / actief-firewall configuratie van Relying Party op gelijk kosten meervoudige pad (ECMP) met BGP of statische routering voor failover.

Openbaar routeerbare IP-adressen zijn opgegeven voor de openbare VIP-groep van het externe netwerk tijdens de implementatie. In een scenario edge, wordt deze niet aanbevolen voor gebruik van openbaar routeerbare IP's op een ander netwerk voor uit veiligheidsoverwegingen. In dit scenario kan een gebruiker de volledige zelf beheerde cloud-ervaring in een openbare cloud, zoals Azure-ervaring.  

![Voorbeeld van Azure Stack edge firewall](./media/azure-stack-firewall/firewallScenarios.png)

## <a name="enterprise-intranet-or-perimeter-network-firewall-scenario"></a>Intranet- of perimeter network firewall bedrijfsscenario
In een enterprise-implementatie van intranet of een perimeternetwerk, Azure Stack geïmplementeerd op een ingedeeld in de multi-zone-firewall of tussen de edge-firewall en firewall van het interne, zakelijke netwerk. Het verkeer wordt dan gedistribueerd tussen de beveiligde, perimeternetwerk bevinden (of DMZ) en niet-beveiligde zones als hieronder beschreven:

- **Beveiligde zone**: Dit is het interne netwerk dat gebruikmaakt van interne of zakelijke routeerbare IP-adressen. Het beveiligde netwerk kan worden onderverdeeld, uitgaande toegang tot internet via NAT hebben op de Firewall en meestal toegankelijk is vanaf een willekeurige plaats in uw datacenter via het interne netwerk. Alle Azure Stack-netwerken moeten zich bevinden in de veilige zone, met uitzondering van het externe netwerk openbare VIP-groep.
- **Perimeter-zone**. Het perimeternetwerk is wanneer het externe of internetgerichte toepassingen, zoals webservers worden doorgaans geïmplementeerd. Dit wordt meestal bewaakt door een firewall om te voorkomen dat aanvallen, zoals DDoS en indringers (hacken) terwijl u nog steeds opgegeven binnenkomend verkeer van internet. Alleen het externe netwerk openbare VIP-groep van Azure Stack moet zich bevinden in de DMZ-zone.
- **Niet-beveiligde zone**. Dit is het externe netwerk, het internet. Deze **is niet** aanbevolen om te implementeren, Azure Stack in de niet-beveiligde zone.

![Voorbeeld van Azure Stack perimeter network](./media/azure-stack-firewall/perimeter-network-scenario.png)

## <a name="learn-more"></a>Meer informatie
Meer informatie over [poorten en protocollen die worden gebruikt door Azure Stack-eindpunten](azure-stack-integrate-endpoints.md).

## <a name="next-steps"></a>Volgende stappen
[Azure Stack-PKI-vereisten](azure-stack-pki-certs.md)

