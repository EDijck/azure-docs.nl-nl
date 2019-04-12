---
title: Aan de slag met SAP op Azure Virtual machines | Microsoft Docs
description: Meer informatie over SAP-oplossingen die worden uitgevoerd op virtuele machines (VM's) in Microsoft Azure
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: ad8e5c75-0cf6-4564-ae62-ea1246b4e5f2
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/10/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3e07824db683b55cf5827962c1030d6cfd6c9f66
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/11/2019
ms.locfileid: "59490426"
---
# <a name="using-azure-for-hosting-and-running-sap-workload-scenarios"></a>Met behulp van Azure voor het hosten en uitvoeren van SAP-workloadscenario 's

Door te kiezen voor Microsoft Azure bent u op betrouwbare wijze uw essentiële SAP workloads en scenario's uitvoeren op een schaalbaar, compatibel en zakelijk bewezen platform.  Ga voor de schaalbaarheid, flexibiliteit en kostenbesparingen van Azure. Met de uitgebreide samenwerking tussen Microsoft en SAP, SAP-toepassingen uitvoeren in dev/test- en productiescenario's in Azure - en volledig wordt ondersteund. Van SAP NetWeaver tot SAP S/4HANA, SAP BI, Linux tot Windows, SAP HANA tot SQL, hebt u.

Naast het hosten van SAP NetWeaver-scenario's met de verschillende DBMS-systemen in Azure, u kunt hosten verschillende andere SAP-workloadscenario's, zoals BI SAP op Azure. 

De uniekheid van Azure voor SAP HANA is een aanbieding die Azure naast de concurrentie stelt. Om in te schakelen die als host fungeert meer geheugen en CPU-bronnen veeleisende SAP-scenario's met betrekking tot SAP HANA, Azure biedt het gebruik van de klant toegewezen hardware van bare-metal ten behoeve van SAP HANA-implementaties die tot 24 TB (120 TB scale-out) geheugen nodig hebben die worden uitgevoerd voor S/4HANA of andere SAP HANA-werkbelastingen. Deze unieke Azure-oplossing van SAP HANA op Azure (grote instanties) kunt u SAP HANA uitvoeren op de specifieke bare-metal hardware met de SAP-toepassingslaag of workload middleware-laag in systeemeigen Azure Virtual Machines die worden gehost. Deze oplossing wordt beschreven in meerdere documenten in de sectie "SAP HANA op Azure (grote instanties)."   

SAP-workloadscenario's in Azure die als host fungeert ook eisen van Identity integration en eenmalige aanmelding met Azure Active Directory aan verschillende onderdelen van SAP en SAP SaaS kunt maken of PaaS biedt. Een lijst van deze integratie en Single-Sign-On scenario's met Azure Active Directory (AAD) en SAP entiteiten wordt beschreven en wordt beschreven in de sectie ' AAD SAP Identity Integration en eenmalige aanmelding. "

## <a name="latest-changes"></a>Meest recente wijzigingen

Release van [grote Azure HANA-instanties beheren via Azure portal](hana-li-portal.md)

Release van [hoge beschikbaarheid voor SAP NetWeaver op Azure VM's in SUSE Linux Enterprise Server met Azure NetApp-bestanden voor SAP-toepassingen](high-availability-guide-suse-netapp-files.md)

Uitleg over **Linux-besturingssysteem parameter net.ipv4.tcp_timestamps** instellingen in combinatie met Azure load balancer

Release van [configuraties van SAP-werkbelasting met Azure-Beschikbaarheidszones](sap-ha-availability-zones.md)

Release van [SAP werkbelasting controlelijst voor planning en implementatie](sap-deployment-checklist.md)




## <a name="sap-hana-on-sap-hana-on-azure-large-instances"></a>SAP HANA op SAP HANA op Azure (grote instanties)

Een reeks documentatie leidt u door SAP HANA op Azure (grote instanties) of korte in HANA grote instanties. De documenten hebben betrekking op de weergegeven gebieden van HANA grote instanties:

- [Overzicht van SAP HANA op Azure (grote instanties)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture)
- [Architectuur van SAP HANA op Azure (grote instanties)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-architecture)
- [Infrastructuur en connectiviteit voor SAP HANA op Azure (grote instanties)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-infrastructure-connectivity)
- [SAP HANA op SAP HANA op Azure (grote instanties) installeren](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-installation)
- [Hoge beschikbaarheid en herstel na noodgevallen van SAP HANA op Azure (grote instanties)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-high-availability-disaster-recovery)
- [Probleemoplossing en bewaking van SAP HANA op Azure (grote instanties)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/troubleshooting-monitoring)

Volgende stappen:

- Lezen [overzicht en architectuur van SAP HANA op Azure (grote instanties)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture)



## <a name="sap-hana-on-azure-virtual-machines"></a>SAP HANA in virtuele machines op Azure
Deze sectie van de documentatie bevat informatie over de verschillende aspecten van SAP HANA. Als een vereiste moet u bekend bent met de principal-services van Azure die elementaire services van Azure IaaS, dus meestal kennis van Azure compute, opslag en netwerken leveren. Veel van deze onderwerpen worden verwerkt in de SAP NetWeaver gerelateerde [Azure Planningshandleiding](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide). 

De documentatie die specifiek is voor HANA op Azure bestaat uit deze lijst met artikelen en hun subarticles:

- [Quickstart: Handmatige installatie van één exemplaar SAP HANA op Azure Virtual machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-get-started)
- [SAP S/4HANA of BW/4HANA op Azure implementeren](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h)
- [Configuraties en bewerkingen van SAP HANA-infrastructuur in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations)
- [SAP HANA met hoge beschikbaarheid voor Azure virtual machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-overview)
- [SAP HANA-beschikbaarheid binnen één Azure-regio](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-one-region)
- [Beschikbaarheid van SAP HANA op Azure-regio 's](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-across-regions)
- [Hoge beschikbaarheid van SAP HANA op Azure virtual machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-high-availability)
- [Back-uphandleiding voor SAP HANA op Azure Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide)
- [SAP HANA Azure back-up op bestandsniveau](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-file-level)
- [Back-up van SAP HANA op basis van opslagmomentopnamen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-storage-snapshots)

 

## <a name="sap-netweaver-deployed-on-azure-virtual-machines"></a>SAP NetWeaver op Azure Virtual Machines geïmplementeerd
In deze sectie vindt u documentatie bij de planning en implementatie voor SAP NetWeaver en Business One on Azure. De documentatie in dit hoofdstuk is gericht meestal met betrekking tot de basisprincipes en het gebruik van niet-HANA-databases met SAP-workloads op Azure. Dat zijn de basis voor HANA met hoge beschikbaarheid in Azure ook de documenten en artikelen voor hoge beschikbaarheid. de lijst met artikelen, zoals:

- [SAP Business One in virtuele Azure-machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/business-one-azure)
- [SAP IDES EHP7 SP3 implementeren voor SAP ERP 6.0 op Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-ides-erp6-erp7-sp3-sql)
- [SAP NetWeaver uitvoeren op Microsoft Azure SUSE Linux VM's](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/suse-quickstart)
- [Azure virtuele Machines, planning en implementatie van SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide)
- [Azure Virtual Machines-implementatie voor SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/deployment-guide)
- [Beveiligen van een implementatie met meerdere lagen SAP NetWeaver-toepassing met behulp van Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-sap)
- [SAP LaMa-connector voor Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/lama-installation)

Met betrekking tot niet-HANA-databases onder de werkbelasting SAP op azure is de lijst met standaarddocumenten, zoals:

- [Overwegingen voor Azure Virtual Machines DBMS-implementatie voor de werkbelasting van SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general)
- [SQL Server Azure virtuele Machines DBMS-implementatie voor SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_sqlserver)
- [DBMS-implementatie voor SAP-werkbelasting in virtuele Azure-machines voor Oracle](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_oracle)
- [DBMS-implementatie voor SAP-werkbelasting in virtuele Azure-machines voor IBM DB2](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_ibm)
- [DBMS-implementatie voor SAP-werkbelasting in virtuele Azure-machines voor SAP ASE](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_sapase)
- [SAP MaxDB liveCache en implementatie van de Server voor webinhoud op Azure Virtual machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_maxdb)

Controleer de sectie SAP HANA op Azure Virtual Machines voor SAP HANA-databases op Azure.

Het document vermelding is voor hoge beschikbaarheid van SAP-workloads op Azure:

- [Azure virtuele Machines hoge beschikbaarheid voor SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-start)

De post-document verwijst naar verschillende andere architectuur en scenario documenten. In de volgende scenario documenten vindt u koppelingen naar gedetailleerde technische documenten uitleg over de implementatie en configuratie van de verschillende hoge beschikbaarheid-methoden. De verschillende documenten van het opzetten en configureren van hoge beschikbaarheid voor de werkbelasting van SAP NetWeaver zijn die betrekking hebben op Linux, evenals de Windows-besturingssystemen.


Voor de integratie tussen Azure Active Directory (AAD) en SAP-Services en Single-Sign-On, documenten, zoals:

- [Zelfstudie: Azure Active Directory-integratie met SAP Cloud for Customer](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-customer-cloud-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Zelfstudie: Azure Active Directory-integratie met SAP Cloud Platform-identiteitsverificatie](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-hana-cloud-platform-identity-authentication-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Zelfstudie: Azure Active Directory-integratie met SAP Cloud Platform](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-hana-cloud-platform-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Zelfstudie: Azure Active Directory-integratie met SAP NetWeaver](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-netweaver-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Zelfstudie: Azure Active Directory-integratie met SAP Business ByDesign](https://docs.microsoft.com/azure/active-directory/saas-apps/sapbusinessbydesign-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Zelfstudie: Azure Active Directory-integratie met SAP HANA](https://docs.microsoft.com/azure/active-directory/saas-apps/saphana-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Uw omgevingen S/4HANA Fiori Launchpad SAML eenmalige aanmelding met Azure AD](https://blogs.sap.com/2017/02/20/your-s4hana-environment-part-7-fiori-launchpad-saml-single-sing-on-with-azure-ad/)

Voor de integratie van Azure-Services in SAP-onderdelen van lijkt de lijst met documenten:

- [SAP HANA gebruiken in Power BI Desktop](https://docs.microsoft.com/power-bi/desktop-sap-hana)
- [DirectQuery en SAP HANA](https://docs.microsoft.com/power-bi/desktop-directquery-sap-hana)
- [De SAP BW-connector gebruiken in Power BI Desktop](https://docs.microsoft.com/power-bi/desktop-sap-bw-connector) 
- [Azure Data Factory biedt integratie van SAP HANA- en Business Warehouse-gegevens](https://azure.microsoft.com/blog/azure-data-factory-offer-sap-hana-and-business-warehouse-data-integration)




