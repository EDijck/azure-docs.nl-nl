---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 01/11/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 9d96bd76a4d284e9b4390c564446e8b27c43d591
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/22/2019
ms.locfileid: "60118499"
---
## <a name="benefits-of-managed-disks"></a>Voordelen van beheerde schijven

We gaan over de voordelen die u met behulp van beheerde schijven krijgen.

### <a name="highly-durable-and-available"></a>Zeer duurzaam en hoge beschikbaarheid

Beheerde schijven zijn ontworpen voor beschikbaarheid van 99,999%. Beheerde schijven doen dit door te voorzien van drie replica's van uw gegevens, waardoor het hoge duurzaamheid. Als er zich problemen voordoen met een of zelfs twee van de replica’s, kunnen de resterende replica’s ervoor zorgen dat uw gegevens bewaard blijven en storingen weinig kwaad kunnen. Deze architectuur heeft geholpen Azure consistent bieden geavanceerde duurzaamheid voor infrastructuur als een dienst (IaaS) schijven, met een toonaangevende nul % op jaarbasis foutpercentage.

### <a name="simple-and-scalable-vm-deployment"></a>Eenvoudige en schaalbare VM-implementatie

Beheerde schijven gebruiken, kunt u maximaal 50.000 VM maken **schijven** van een type in een abonnement per regio, zodat u kunt maken van duizenden **VMs** in één abonnement. Deze functie ook verder verhoogt de schaalbaarheid van [virtuele-machineschaalsets](../articles/virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) doordat u maximaal 1000 virtuele machines in een schaalset van de virtuele machine met behulp van een Marketplace-installatiekopie maken.

### <a name="integration-with-availability-sets"></a>Integratie met beschikbaarheidssets

Beheerde schijven zijn geïntegreerd met beschikbaarheidssets om ervoor te zorgen dat de schijven van [virtuele machines in een beschikbaarheidsset](../articles/virtual-machines/windows/manage-availability.md#use-managed-disks-for-vms-in-an-availability-set) voldoende zijn geïsoleerd van elkaar om te voorkomen dat een single point of failure. Schijven worden automatisch in verschillende kasten (stempels) geplaatst. Als een stempel is mislukt vanwege fout bij de hardware of software, wordt alleen de VM-exemplaren met schijven op deze stempels mislukt. Bijvoorbeeld: Stel dat u hebt een toepassing die wordt uitgevoerd op vijf virtuele machines en de virtuele machines zich in een Beschikbaarheidsset. De schijven voor deze virtuele machines wordt niet allemaal worden opgeslagen in de dezelfde stempel, dus als één stamp is uitvalt, worden de andere exemplaren van de toepassing worden uitgevoerd.

### <a name="integration-with-availability-zones"></a>Integratie met Beschikbaarheidszones

Beheerde schijven ondersteunt [Beschikbaarheidszones](../articles/availability-zones/az-overview.md), dit is een hoge beschikbaarheid bieden die uw toepassingen tegen storingen in datacenters beschermt. Beschikbaarheidszones zijn unieke fysieke locaties binnen een Azure-regio. Elke zone bestaat uit een of meer datacenters die zijn uitgerust met onafhankelijke voeding, koeling en netwerken. Om voor tolerantie te zorgen, is er een minimum van drie afzonderlijke zones in alle ingeschakelde regio's. Met beschikbaarheidszones biedt Azure de beste uptime SLA voor VM’s van de branche, van 99,99%.

### <a name="azure-backup-support"></a>Ondersteuning van Azure back-up

Om te beveiligen tegen regionale rampen [Azure Backup](../articles/backup/backup-introduction-to-azure-backup.md) kan worden gebruikt voor het maken van een back-uptaak met back-ups op basis van tijd en voor het bewaren van back-up. Hiermee kunt u eenvoudig VM herstelprocedures moment uitvoeren. Azure Backup ondersteunt momenteel schijfgrootten tot vier tebibyte (TiB)-schijven. Zie voor meer informatie, [back-up met behulp van Azure voor virtuele machines met beheerde schijven](../articles/backup/backup-introduction-to-azure-backup.md#using-managed-disk-vms-with-azure-backup).

### <a name="granular-access-control"></a>Gebruik nauwkeurig toegangsbeheer

U kunt [Azure op rollen gebaseerd toegangsbeheer (RBAC)](../articles/role-based-access-control/overview.md) specifieke machtigingen voor een beheerde schijf aan een of meer gebruikers toewijzen. Maken van de beheerde schijven een aantal bewerkingen, zoals lezen, schrijven (maken/bijwerken), verwijderen en bij het ophalen van een [handtekening voor gedeelde toegang (SAS) URI](../articles/storage/common/storage-dotnet-shared-access-signature-part-1.md) voor de schijf. U kunt toegang verlenen tot alleen de bewerkingen die iemand nodig heeft om uit te voeren hun taak. Bijvoorbeeld, als u niet dat een persoon in een beheerde schijf kopiëren naar een opslagaccount wilt, kunt u geen toegang naar de export-actie voor de beheerde schijf te verlenen. Op dezelfde manier als u niet dat een persoon in een SAS-URI gebruiken om te kopiëren van een beheerde schijf wilt, kunt u geen toestemming te verlenen die voor de beheerde schijf.

## <a name="disk-roles"></a>Schijf-rollen

Er zijn drie belangrijkste schijf rollen in Azure: de gegevensschijf, schijf met het besturingssysteem en de tijdelijke schijf. Deze rollen worden toegewezen aan de schijven die zijn gekoppeld aan uw virtuele machine.

![Schijf-rollen in actie](media/virtual-machines-managed-disks-overview/disk-types.png)

### <a name="data-disk"></a>Gegevensschijf

Een gegevensschijf is een beheerde schijf die gekoppeld aan een virtuele machine voor het opslaan van toepassingsgegevens, of andere gegevens die u wilt houden. Gegevensschijven worden geregistreerd als SCSI-stations en zijn gelabeld met een letter die u kiest. Elke gegevensschijf heeft een maximale capaciteit van 32.767 gibibytes (GiB). De grootte van de virtuele machine bepaalt hoeveel gegevensschijven die u aan het en het type opslag koppelen kunt die u kunt gebruiken voor het hosten van de schijven.

### <a name="os-disk"></a>Besturingssysteemschijf

Elke virtuele machine heeft een gekoppelde besturingssysteemschijf. De OS-schijf heeft een vooraf geïnstalleerde besturingssysteem, die is geselecteerd als de virtuele machine is gemaakt.

Deze schijf heeft een maximale capaciteit van 2048 GiB.

### <a name="temporary-disk"></a>Tijdelijke schijf

Elke virtuele machine bevat een tijdelijke schijf, niet een beheerde schijf is. De tijdelijke schijf opslag op korte termijn biedt voor toepassingen en processen en is bedoeld voor het opslaan van gegevens, zoals pagina-of wisselbestanden alleen. Op de tijdelijke schijf mogelijk gegevens verloren tijdens een [onderhoudsgebeurtenis](../articles/virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#understand-vm-reboots---maintenance-vs-downtime) gebeurtenis of wanneer u [een virtuele machine opnieuw implementeren](../articles/virtual-machines/troubleshooting/redeploy-to-new-node-windows.md?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Ftoc.json). De tijdelijke schijf is /dev/sdb standaard op Azure Linux Virtual machines, en op Windows-VM's is de tijdelijke schijf E: standaard. Tijdens een geslaagde standaard opnieuw opstarten van de virtuele machine blijft de gegevens op de tijdelijke schijf.

## <a name="managed-disk-snapshots"></a>Momentopnamen van de beheerde schijf

Een momentopname van een beheerde schijf is een alleen-lezen kopie van een beheerde schijf die wordt opgeslagen als een standaard beheerde schijf standaard. Met momentopnamen, kunt u back-up uw beheerde schijven op elk gewenst moment in-time. Deze momentopnamen onafhankelijk van de bronschijf bestaan en kunnen worden gebruikt voor het maken van nieuwe beheerde schijven. Ze worden in rekening gebracht op basis van de gebruikte grootte. Bijvoorbeeld, als u een momentopname van een beheerde schijf met ingerichte capaciteit van 64 GiB en de werkelijke gebruikte gegevensgrootte van 10 GiB maken, wordt die momentopname alleen gefactureerd voor de gebruikte gegevensgrootte van 10 GiB.  

Zie de volgende bronnen voor meer informatie over het maken van momentopnamen met beheerde schijven:

* [Kopie maken van een VHD die is opgeslagen als een beheerde schijf met behulp van momentopnamen in Windows](../articles/virtual-machines/windows/snapshot-copy-managed-disk.md)
* [Een kopie maken van een VHD die is opgeslagen als beheerde schijf met behulp van momentopnamen in Linux](../articles/virtual-machines/linux/snapshot-copy-managed-disk.md)

### <a name="images"></a>Installatiekopieën

Beheerde schijven bieden ook ondersteuning voor het maken van een beheerde aangepaste installatiekopie. U kunt een installatiekopie maken van uw aangepaste VHD in een opslagaccount of rechtstreeks vanaf een gegeneraliseerde (Sysprep) virtuele machine. Dit proces vastgelegd een één installatiekopie. Deze installatiekopie bevat alle beheerde schijven die zijn gekoppeld aan een virtuele machine, met inbegrip van zowel het besturingssysteem en gegevensschijven. Deze beheerde aangepaste installatiekopie kunt maken honderden VM's met behulp van uw aangepaste installatiekopie zonder de noodzaak om te kopiëren of een storage-accounts beheren.

Zie de volgende artikelen voor meer informatie over het maken van installatiekopieën:

* [Hoe u een beheerde installatiekopie maken van een gegeneraliseerde VM in Azure](../articles/virtual-machines/windows/capture-image-resource.md)
* [Een virtuele Linux-machine generaliseren en vastleggen met de Azure CLI](../articles/virtual-machines/linux/capture-image.md)

#### <a name="images-versus-snapshots"></a>Installatiekopieën met momentopnamen

Het is belangrijk om te begrijpen van het verschil tussen afbeeldingen en momentopnamen. U kunt een installatiekopie van een gegeneraliseerde virtuele machine die is opgeheven uitvoeren met beheerde schijven. Deze installatiekopie bevat alle schijven die zijn gekoppeld aan de virtuele machine. U kunt deze installatiekopie gebruiken om een VM te maken en omvat alle schijven.

Een momentopname is een kopie van een schijf op het punt in tijd die momentopname wordt gemaakt. Dit geldt alleen voor één schijf. Als u een virtuele machine waarvoor één schijf (schijf met het besturingssysteem) hebt, kunt u een momentopname of een afbeelding van het ondernemen en een virtuele machine maken van de momentopname of de afbeelding.

Een momentopname bevat geen bewust te maken van een schijf, behalve de record bevat. Dit maakt het problematische voor gebruik in scenario's waarin de coördinatie van meerdere schijven, zoals gesegmenteerd te verdelen. Momentopnamen zou moeten kunnen contact opnemen met elkaar en dit wordt momenteel niet ondersteund.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de afzonderlijke schijftypen die Azure biedt en welk type is geschikt voor uw behoeften in ons artikel over schijftypen.
