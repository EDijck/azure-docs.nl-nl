---
title: Plannen van capaciteit en schaalbaarheid voor noodherstel van VMware naar Azure met Azure Site Recovery | Microsoft Docs
description: Dit artikel om de capaciteit plannen en schaal gebruiken bij het instellen van herstel na noodgevallen van virtuele VMware-machines naar Azure met Azure Site Recovery
author: nsoneji
manager: garavd
ms.service: site-recovery
ms.date: 12/12/2018
ms.topic: conceptual
ms.author: mayg
ms.openlocfilehash: dc903fca206f5d40f631181b83252f505b9f57a2
ms.sourcegitcommit: 3ab534773c4decd755c1e433b89a15f7634e088a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/07/2019
ms.locfileid: "54065203"
---
# <a name="plan-capacity-and-scaling-for-vmware-disaster-recovery-to-azure"></a>Plannen van capaciteit en schaalbaarheid voor noodherstel van VMware naar Azure

In dit artikel gebruiken om te achterhalen van de capaciteit plannen en te schalen, bij het repliceren van on-premises VMware-machines en fysieke servers naar Azure met [Azure Site Recovery](site-recovery-overview.md).

## <a name="how-do-i-start-capacity-planning"></a>Hoe start ik plannen van capaciteit?

Als u wilt weten de vereisten voor Azure Site Recovery-infrastructuur, verzamelt u informatie over uw replicatieomgeving door de [Azure Site Recovery Deployment Planner](https://aka.ms/asr-deployment-planner-doc) voor VMware-replicatie. [Meer informatie](site-recovery-deployment-planner.md) over dit hulpprogramma. Dit hulpprogramma biedt een rapport met volledige informatie op compatibele en niet-compatibele virtuele machines, schijven per VM en gegevensverloop per schijf. Het hulpprogramma bevat ook een overzicht van netwerkbandbreedtevereisten om te voldoen aan de RPO-doel en de Azure-infrastructuur die nodig zijn voor succesvolle replicatie en failover.

## <a name="capacity-considerations"></a>Overwegingen voor capaciteit

**Onderdeel** | **Details** |
--- | --- | ---
**Replicatie** | **Maximale dagelijkse veranderingssnelheid:** Een beveiligde virtuele machine kan alleen een processerver gebruiken en een één processerver kan verwerken een dagelijkse wijziging beoordelen tot 2 TB. 2 TB is dus dat de maximale dagelijkse veranderingssnelheid van gegevens die wordt ondersteund voor een beveiligde machine.<br/><br/> **Maximale doorvoer:** Een gerepliceerde machine kan deel uitmaken van een opslagaccount in Azure. Een standard storage-account kan een maximum van 20.000 aanvragen per seconde verwerken, en het is raadzaam het aantal i/o-bewerkingen per seconde (IOPS) te behouden tussen een broncomputer 20.000. Bijvoorbeeld, als u een bronmachine met 5 schijven, en elke schijf 120 IOPS (8 kB grootte) op de broncomputer genereert, is klikt u vervolgens het binnen de door Azure per schijf-IOPS-limiet van 500. (Het aantal vereiste opslagaccounts voor is gelijk aan de totale bronmachine IOP's, gedeeld door 20.000.)
**Configuratieserver** | De configuratieserver moet kunnen zijn voor het afhandelen van het dagelijkse tarief capaciteit voor wijzigen voor alle workloads die worden uitgevoerd op beveiligde machines en moet voldoende bandbreedte continu om gegevens te repliceren naar Azure Storage.<br/><br/> Als een best practice, zoekt u de configuratieserver op hetzelfde netwerk- en LAN-segment als de machines die u wilt beveiligen. Deze kan zich bevinden op een ander netwerk, maar machines die u wilt beveiligen moet zichtbaarheid van laag-3-netwerk toe.<br/><br/> Aanbevelingen voor de grootte voor de configuratieserver worden samengevat in de tabel in de volgende sectie.
**Processerver** | De eerste processerver is standaard geïnstalleerd op de configuratieserver. U kunt extra processervers om te schalen van uw omgeving kunt implementeren. <br/><br/> De processerver ontvangt replicatiegegevens van beveiligde machines en optimaliseert met caching, compressie en codering. Vervolgens verzendt de gegevens naar Azure. De proces-server-machine moet voldoende bronnen zijn voor deze taken uitvoeren.<br/><br/> De processerver maakt gebruik van een cache op basis van een schijf. Gebruik een afzonderlijke cacheschijf 600 GB of meer om gegevenswijzigingen die zijn opgeslagen in het geval van een knelpunt netwerk of een storing te verwerken.

## <a name="size-recommendations-for-the-configuration-server-along-with-in-built-process-server"></a>Aanbevelingen voor de grootte voor de configuratieserver (samen met ingebouwde processerver)

Elke configuratieserver geïmplementeerd via [OVF-sjabloon](vmware-azure-deploy-configuration-server.md#deployment-of-configuration-server-through-ova-template) een ingebouwde processerver heeft. Resources van de configuratieserver, zoals CPU, geheugen, schijfruimte worden gebruikt op een ander tarief als ingebouwde processerver wordt gebruikt om virtuele machines te beveiligen. Daarom kan verschillen de vereisten als ingebouwde processerver wordt gebruikt.
Een configuratieserver waar ingebouwde processerver wordt gebruikt ter bescherming van de werkbelasting kan maximaal 200 virtuele machines op basis van de volgende configuraties worden verwerkt

**CPU** | **Geheugen** | **Cachegrootte van de schijf** | **Veranderingssnelheid van gegevens** | **Beveiligde machines**
--- | --- | --- | --- | ---
8 vcpu's (2-sockets * 4 kernen \@ 2,5 GHz [gigahertz]) | 16 GB | 300 GB | 500 GB of minder | Minder dan 100 machines repliceren.
12 vcpu's (2-sockets * 6 kernen \@ 2,5 GHz) | 18 GB | 600 GB | 500 GB tot 1 TB | 100-150-machines repliceren.
16 vcpu's (2-sockets * 8 kernen \@ 2,5 GHz) | 32 GB | 1 TB | 1 TB tot 2 TB | Repliceren tussen 150 tot 200-machines.
Implementeren van een andere configuratieserver via [OVF-sjabloon](vmware-azure-deploy-configuration-server.md#deployment-of-configuration-server-through-ova-template) | | | | Nieuwe configuratieserver implementeren als u meer dan 200 machines repliceert.
Implementatie van een andere [processerver](vmware-azure-set-up-process-server-scale.md#download-installation-file) | | | &GT; 2 TB| Nieuwe uitbreidbare processerver implementeren als de totale dagelijkse veranderingssnelheid van gegevens is groter dan 2 TB.

Waar:

* Elke bron-VM is geconfigureerd met 3 schijven van 100 GB elk.
* We benchmarking opslag van 8 SAS-schijven van 10 K RPM, RAID 10 voor de cache schijf metingen gebruikt.

## <a name="size-recommendations-for-the-process-server"></a>Aanbevelingen voor de grootte voor de processerver

Processerver is het onderdeel die verantwoordelijk is voor de procedure voor replicatie van gegevens in Azure Site Recovery. Als de dagelijkse veranderingssnelheid groter dan 2 TB is, moet u het toevoegen van een scale-out-proces voor het afhandelen van de belasting van de replicatie. Als u wilt schalen, kunt u het volgende doen:

* Het aantal configuratieservers verhogen door te implementeren via [OVF-sjabloon](vmware-azure-deploy-configuration-server.md#deployment-of-configuration-server-through-ova-template). Bijvoorbeeld, kunt u maximaal 400 machines met twee configuratieservers beveiligen.
* Voeg [scale-out processervers](vmware-azure-set-up-process-server-scale.md#download-installation-file), en deze gebruiken voor het afhandelen van replicatieverkeer in plaats van (of naast) de configuratieserver.

De volgende tabel beschrijft een scenario waarin:

* U kunt een uitbreidbare processerver hebt ingesteld.
* U kunt beveiligde virtuele machines voor het gebruik van de uitbreidbare processerver hebt geconfigureerd.
* Elke beveiligde bron-VM is geconfigureerd met drie schijven van 100 GB elk.

**Aanvullende processerver** | **Cachegrootte van de schijf** | **Veranderingssnelheid van gegevens** | **Beveiligde machines**
--- | --- | --- | ---
4 vcpu's (2-sockets * 2 kernen \@ 2,5 GHz), 8 GB geheugen | 300 GB | 250 GB of minder | 85 of minder machines repliceren.
8 vcpu's (2-sockets * 4 kernen \@ 2,5 GHz), 12 GB geheugen | 600 GB | 250 GB tot 1 TB | 85-150-machines repliceren.
12 vcpu's (2-sockets * 6 kernen \@ 2,5 GHz) 24 GB geheugen | 1 TB | 1 TB tot 2 TB | Repliceren tussen 150 225 machines.

De manier waarop u uw servers schalen, is afhankelijk van uw voorkeur voor een scale-up- of scale-out-model.  U door het implementeren van een paar geavanceerde configuratie en processervers omhoog schalen of uitschalen door meer servers met minder resources implementeren. Bijvoorbeeld, als u beveiligen 200 machines met veranderingssnelheid van algemene gegevens elke dag om 1,5 TB wilt, kan u doen het volgende:

* Instellen van een enkel proces-server met 16 vCPU, 24 GB RAM-geheugen.
* Instellen van twee processervers (2 x 8 vCPU, 2 * 12 GB RAM-geheugen).

## <a name="control-network-bandwidth"></a>Netwerkbandbreedte

Nadat u hebt gebruikt de [het hulpprogramma Deployment Planner](site-recovery-deployment-planner.md) voor het berekenen van de bandbreedte die u nodig hebt voor replicatie (initiële replicatie en klikt u vervolgens delta), kunt u de hoeveelheid bandbreedte die wordt gebruikt voor replicatie met behulp van een aantal opties beheren:

* **Bandbreedte beperken**: VMware-verkeer dat wordt gerepliceerd naar Azure wordt gerouteerd via een specifiek proces. U kunt de bandbreedte op de machines die worden uitgevoerd als processervers beperken.
* **Bandbreedte van invloed zijn op**: U kunt de bandbreedte die wordt gebruikt voor replicatie via diverse registersleutels beïnvloeden:
  * De **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication\UploadThreadsPerVM** registerwaarde Hiermee geeft u het aantal threads die worden gebruikt voor gegevensoverdracht (initiële replicatie of deltareplicatie) van een schijf. Hoe hoger de waarde, hoe meer netwerkbandbreedte er voor replicatie wordt gebruikt.
  * De **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication\DownloadThreadsPerVM** Hiermee geeft u het aantal threads die worden gebruikt voor gegevensoverdracht tijdens een failback.

### <a name="throttle-bandwidth"></a>Bandbreedte beperken

1. Open de Azure Backup MMC-module op de computer die fungeert als de processerver. Een snelkoppeling voor back-up is standaard beschikbaar zijn op het bureaublad of in de volgende map: C:\Program Files\Microsoft Azure Recovery Services Agent\bin.
2. Klik in de module op **Eigenschappen wijzigen**.

    ![Schermopname van Azure Backup MMC-module optie voor het wijzigen van eigenschappen](./media/site-recovery-vmware-to-azure/throttle1.png)
3. Op de **beperking** tabblad **inschakelen voor gebruik van internetbandbreedte voor back-upbewerkingen**. Stel de limieten in voor werktijden en niet-wordt gewerkt. Geldige bereiken liggen tussen 512 Kbps en 1023 Mbps per seconde.

    ![In het dialoogvenster schermopname van Azure Backup-eigenschappen](./media/site-recovery-vmware-to-azure/throttle2.png)

U kunt ook de cmdlet [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) gebruiken om een beperking in te stellen. Hier volgt een voorbeeld:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set-OBMachineSetting -NoThrottle** geeft aan dat er geen beperking is vereist.

### <a name="influence-network-bandwidth-for-a-vm"></a>Netwerkbandbreedte beïnvloeden voor een virtuele machine

1. In het register van de virtuele machine, gaat u naar **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.
   * Wijzig de waarde van de bandbreedteverkeer op een replicerende schijf beïnvloeden, **UploadThreadsPerVM**, of maak de sleutel als deze niet bestaat.
   * Als u wilt hoeveel bandbreedte er voor failbackverkeer vanuit Azure, wijzig de waarde van **DownloadThreadsPerVM**.
2. De standaardwaarde is 4. In netwerken met overprovisioning moeten deze registersleutels zodanig worden gewijzigd dat niet de standaardwaarden worden gebruikt. Het maximum is 32. Houd het verkeer in de gaten om de waarde te optimaliseren.

## <a name="setup-azure-site-recovery-infrastructure-to-protect-more-than-500-virtual-machines"></a>Azure Site Recovery-infrastructuur voor het beveiligen van meer dan 500 virtuele machines instellen

Voordat u van Azure Site Recovery-infrastructuur instelt, moet u toegang tot de omgeving om te meten van de volgende factoren: compatibele virtuele machines, gegevens dagelijkse veranderingssnelheid vereiste netwerkbandbreedte voor de gewenste RPO, aantal Azure site recovery onderdelen die vereist zijn, gebruikte tijd voor het voltooien van de initiële replicatie enz.,

1. Als u wilt deze parameters meten, zorg ervoor dat u de deployment planner uitvoeren op uw omgeving met behulp van de richtlijnen voor gedeelde [hier](site-recovery-deployment-planner.md).
2. Een configuratieserver implementeren aan de vereisten die hierboven worden vermeld. Als uw productie-werkbelasting groter is dan 650 virtuele machines, implementeert u een andere configuratieserver.
3. Implementeren op basis van de gemeten dagelijkse veranderingssnelheid van gegevens, [scale-out processervers](vmware-azure-set-up-process-server-scale.md#download-installation-file) met behulp van de grootte richtlijnen vermeld [hier](site-recovery-plan-capacity-vmware.md#size-recommendations-for-the-process-server).
4. Als u verwacht de veranderingssnelheid van gegevens dat voor een virtuele machine van schijf 2 MBps wordt overschreden, zorg er dan aan [instellen van een premium storage-account](tutorial-prepare-azure.md#create-a-storage-account). Omdat de deployment planner wordt uitgevoerd voor een bepaalde periode, veranderingssnelheid pieken in de gegevens van andere periode punten kunnen niet worden vastgelegd in het rapport.
5. Aan de hand van de gewenste RPO [instellen van de netwerkbandbreedte](site-recovery-plan-capacity-vmware.md#control-network-bandwidth).
6. Na de installatie van de infrastructuur, volgt u de richtlijnen gepubliceerd onder [sectie procedures](vmware-azure-set-up-source.md) om in te schakelen van herstel na noodgevallen voor uw workload.

## <a name="deploy-additional-process-servers"></a>Extra processervers implementeren

Als u hebt voor het schalen van buiten uw implementatie meer dan 200 bronmachines, of hebt u een totaal dagelijks verloop snelheid van meer dan 2 TB, moet u extra processervers om het verkeersvolume te verwerken. Volg de instructies op [in dit artikel](vmware-azure-set-up-process-server-scale.md) voor het instellen van de processerver. Na het instellen van de server, kunt u migreren bronmachines om deze te gebruiken.

### <a name="migrate-machines-to-use-the-new-process-server"></a>Migreren van machines voor het gebruik van de nieuwe processerver

1. In **instellingen** > **Site Recovery-servers**, klikt u op de configuratieserver uit en vouw vervolgens **servers verwerken**.

    ![Schermafbeelding van de processerver-dialoogvenster](./media/site-recovery-vmware-to-azure/migrate-ps2.png)
2. Met de rechtermuisknop op de processerver die momenteel in gebruik en klikt u op **Switch**.

    ![Schermafbeelding van de configuratie in het dialoogvenster van server](./media/site-recovery-vmware-to-azure/migrate-ps3.png)
3. In **doelprocesserver selecteren**, selecteert u de nieuwe processerver die u wilt gebruiken, en selecteer vervolgens de virtuele machines op de server af te handelen. Klik op het informatiepictogram voor informatie over de server. Voor hulp bij het laden van beslissingen maken, wordt de gemiddelde ruimte die nodig is voor het repliceren van elke geselecteerde virtuele machine naar de nieuwe processerver weergegeven. Klik op het vinkje om te beginnen met het repliceren naar de nieuwe processerver.

## <a name="deploy-additional-master-target-servers"></a>Aanvullende hoofddoelservers implementeren

U moet extra hoofddoelserver tijdens de volgende scenario 's

1. Als u een op basis van Linux virtuele machine te beschermen wilt.
2. Als de hoofddoelserver beschikbaar is op de configuratieserver heeft geen toegang tot het gegevensarchief van virtuele machine.
3. Als het totale aantal schijven op de master doelserver (niet. is groter dan 60 schijven van lokale schijven op server) + schijven moeten worden beveiligd.

Om toe te voegen een nieuwe hoofddoelserver voor **op basis van Linux virtuele machine**, [Klik hier](vmware-azure-install-linux-master-target.md).

Voor **Windows gebaseerde virtuele machine**, volgt u de hieronder opgegeven instructies.

1. Navigeer naar **Recovery Services-kluis** > **Site Recovery-infrastructuur** > **configuratieservers**.
2. Klik op de vereiste configuratie-server > **+ Masterdoelserver**.![ toevoegen-master-doel-server.png](media/site-recovery-plan-capacity-vmware/add-master-target-server.png)
3. De geïntegreerde instellingen downloaden en uitvoeren op de virtuele machine voor het instellen van de hoofddoelserver.
4. Kies **installeren hoofddoel** > **volgende**. ![Kies MT.PNG](media/site-recovery-plan-capacity-vmware/choose-MT.PNG)
5. Kies de standaardlocatie voor installatie > klikt u op **installeren**. ![MT-installatie](media/site-recovery-plan-capacity-vmware/MT-installation.PNG)
6. Klik op **gaat u verder met de configuratie van** hoofddoel registreren met de configuratieserver. ![MT-gaan configuration.PNG](media/site-recovery-plan-capacity-vmware/MT-proceed-configuration.PNG)
7. Voer het IP-adres van de configuratieserver & wachtwoordzin. [Klik hier](vmware-azure-manage-configuration-server.md#generate-configuration-server-passphrase) voor informatie over het genereren van wachtwoordzin.![ CS-ip-wachtwoordzin](media/site-recovery-plan-capacity-vmware/cs-ip-passphrase.PNG)
8. Klik op **registreren** en na registratie op **voltooien**.
9. Na de registratie is gelukt, deze server wordt weergegeven op de portal onder **Recovery Services-kluis** > **Site Recovery-infrastructuur** > **configuratie servers** > doelservers van de relevante configuratieserver-master.

 >[!NOTE]
 >U kunt ook downloaden de nieuwste versie van Master target server geïntegreerde instellen voor Windows [hier](https://aka.ms/latestmobsvc).

## <a name="next-steps"></a>Volgende stappen

Downloaden en uitvoeren van de [Azure Site Recovery Deployment Planner](https://aka.ms/asr-deployment-planner)
