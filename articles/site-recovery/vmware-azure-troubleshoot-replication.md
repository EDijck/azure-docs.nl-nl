---
title: Oplossen van problemen met replicatie voor herstel na noodgevallen van virtuele VMware-machines en fysieke servers naar Azure met behulp van Azure Site Recovery | Microsoft Docs
description: Dit artikel bevat informatie over probleemoplossing voor algemene problemen met replicatie tijdens herstel na noodgevallen van virtuele VMware-machines en fysieke servers naar Azure met behulp van Azure Site Recovery.
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 03/14/2019
ms.author: mayg
ms.openlocfilehash: 1aaf13f01c7e7197001f3099fabd4b8be8545f0d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60565059"
---
# <a name="troubleshoot-replication-issues-for-vmware-vms-and-physical-servers"></a>Problemen met replicatie voor virtuele VMware-machines en fysieke servers

U kunt een specifiek foutbericht tegenkomen wanneer u uw virtuele VMware-machines of fysieke servers beschermen met behulp van Azure Site Recovery. In dit artikel beschrijft enkele veelvoorkomende problemen die optreden mogelijk wanneer u on-premises VMware-machines en fysieke servers naar Azure met behulp van repliceren [siteherstel](site-recovery-overview.md).

## <a name="monitor-process-server-health-to-avoid-replication-issues"></a>Status van de processerver om te voorkomen dat problemen met replicatie bewaken

Het verdient aanbeveling om de status van de proces-Server (PS) in de portal om ervoor te zorgen dat de replicatie vordert voor uw gekoppelde bronmachines te bewaken. Ga in de kluis naar beheren > Site Recovery-infrastructuur > configuratieservers. Klik op de blade van de configuratieserver op de processerver onder Servers die zijn gekoppeld. Proces Server-blade wordt geopend met de status van statistieken. U kunt CPU-gebruik, geheugengebruik, de status van de PS-services die nodig zijn voor replicatie, de vervaldatum van certificaat en de beschikbare vrije ruimte op te volgen. De status van alle statistische gegevens moet groen. 

**Het wordt aanbevolen om geheugen en CPU-gebruik onder 70% hebben en vrije ruimte bovenstaande 25%**. Vrije ruimte verwijst naar de cache schijfruimte op de processerver die wordt gebruikt voor het opslaan van de replicatiegegevens van de bronmachines voordat u uploadt naar Azure. Als deze wordt beperkt tot minder dan 20%, wordt de replicatie voor alle gekoppelde bronmachines worden beperkt. Ga als volgt de [capaciteit richtlijnen](./site-recovery-plan-capacity-vmware.md#capacity-considerations) om te begrijpen van de vereiste configuratie voor het repliceren van de bronmachines.

Zorg ervoor dat de volgende services worden uitgevoerd op de PS-machine. Starten of opnieuw starten van een service die actief is.

**Ingebouwde processerver**

* ProcessServer
* ProcessServerMonitor
* cxprocessserver
* InMage PushInstall
* De Service log Upload (LogUpload)
* InMage Scout Application Service
* Microsoft Azure Recovery Services-Agent (obengine)
* InMage Scout VX Agent - Sentinel/Outpost (svagents)
* tmansvc
* World Wide Web Publishing Service (W3SVC)
* MySQL
* Microsoft Azure Site Recovery-Service (dra)

**Uitbreidbare processerver**

* ProcessServer
* ProcessServerMonitor
* cxprocessserver
* InMage PushInstall
* De Service log Upload (LogUpload)
* InMage Scout Application Service
* Microsoft Azure Recovery Services-Agent (obengine)
* InMage Scout VX Agent - Sentinel/Outpost (svagents)
* tmansvc

**Processerver in Azure voor failback**

* ProcessServer
* ProcessServerMonitor
* cxprocessserver
* InMage PushInstall
* De Service log Upload (LogUpload)

Zorg ervoor dat het StartType van alle services is ingesteld op **automatisch of automatisch (vertraagd starten)**. Microsoft Azure Recovery Services-Agent (obengine) service hoeft niet te hebben het StartType is ingesteld als hierboven.

## <a name="replication-issues"></a>Problemen met replicatie

Initiële en lopende replicatiefouten worden vaak veroorzaakt door problemen met de netwerkverbinding tussen de bron- en de processerver of tussen de processerver en Azure. In de meeste gevallen kunt u deze problemen kunt oplossen door de stappen in de volgende secties te voltooien.

>[!Note]
>Zorg ervoor dat:
>1. Het systeem is tijd voor het beveiligde item gesynchroniseerd.
>2. Er is geen antivirussoftware wordt geblokkeerd door Azure Site Recovery. Informatie over [meer](vmware-azure-set-up-source.md#azure-site-recovery-folder-exclusions-from-antivirus-program) op uitsluitingen die vereist zijn voor Azure Site Recovery.

### <a name="check-the-source-machine-for-connectivity-issues"></a>De broncomputer voor problemen met de netwerkverbinding controleren

De volgende lijst toont manieren waarop u op de bronmachine controleren kunt.

*  Op de opdrachtregel op de bronserver, moet u Telnet gebruiken om te pingen van de processerver via de HTTPS-poort door de volgende opdracht uit. HTTPS-poort 9443 is de standaardinstelling gebruikt door de processerver voor het verzenden en ontvangen van replicatieverkeer. U kunt deze poort wijzigen op het moment van inschrijving. De volgende opdracht uit voor problemen met de netwerkverbinding en voor problemen dat blok de firewallpoort gecontroleerd.


   `telnet <process server IP address> <port>`


   > [!NOTE]
   > Telnet gebruiken om te controleren. Gebruik geen `ping`. Als Telnet niet is geïnstalleerd, voert u de stappen in [Telnet-Client installeren](https://technet.microsoft.com/library/cc771275(v=WS.10).aspx).

   Als telnet geen verbinding maken met succes met de PS-poort is, zou een leeg scherm worden weergegeven.

   Als u geen verbinding met de processerver maken, kunt u de binnenkomende poort 9443 op de processerver. Bijvoorbeeld, u moet mogelijk binnenkomende poort 9443 op de processerver is toegestaan als het netwerk een perimeternetwerk heeft of gescreend subnet wordt genoemd. Controleer vervolgens of het probleem nog steeds voordoet.

*  Als telnet geslaagd is en nog de bronmachine rapporteert de PS is niet bereikbaar, open de webbrowser op de bronmachine en controleer of het adres https://<PS_IP>:<PS_Data_Port>/ bereikbaar is.

    Fout voor HTTPS-certificaat wordt op te maken met dit adres verwacht. Certificaatfout negeren en doorgaan moeten terechtkomen op 400-Ongeldige aanvraag, wat betekent dat de server kan niet fungeren als een aanvraag van de browser en of de standaard HTTPS-verbinding met de server goed en goed werkt.

    Als dit niet werkt, vindt u meer informatie over het foutbericht in browser richtlijnen. Voor bijvoorbeeld als de proxy-verificatie onjuist is, 407: Proxy-verificatie vereist, samen met de vereiste acties in het foutbericht wordt geretourneerd door de proxyserver. 

*  Controleer de volgende logboeken op de bron-VM voor fouten met betrekking tot het netwerk uploadfouten:

       C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\svagents*.log 

### <a name="check-the-process-server-for-connectivity-issues"></a>De processerver voor problemen met de netwerkverbinding controleren

De volgende lijst toont manieren waarop u op de processerver controleren kunt:

> [!NOTE]
> Processerver moet een statische IPv4-adres en moet niet zijn NAT IP geconfigureerd op het.

* **Controleer de verbinding tussen de bronmachines en de processerver**
* In het geval u kunt telnet vanaf broncomputer en nog de PS niet bereikbaar is vanuit de bron is, controleert u de end-to-end-verbinding met de cxprocessserver uit de bron-VM door cxpsclient hulpprogramma uitvoert op de bron-VM:

      <install folder>\cxpsclient.exe -i <PS_IP> -l <PS_Data_Port> -y <timeout_in_secs:recommended 300>

   Controleer de gegenereerde logboeken op de PS in de volgende mappen voor meer informatie over de bijbehorende fouten:

      C:\ProgramData\ASR\home\svsystems\transport\log\cxps.err
      and
      C:\ProgramData\ASR\home\svsystems\transport\log\cxps.xfer
* Raadpleeg de volgende logboeken op de PS als er geen heartbeat van PS uitvoeren. Dit wordt aangeduid met **foutcode 806** in de portal.

      C:\ProgramData\ASR\home\svsystems\eventmanager*.log
      and
      C:\ProgramData\ASR\home\svsystems\monitor_protection*.log

* **Controleer of de processerver actief van gegevens naar Azure pushen is**.

  1. Open Taakbeheer (druk op Ctrl + Shift + Esc) op de processerver.
  2. Selecteer de **prestaties** tabblad, en selecteer vervolgens de **Open Broncontrole** koppeling. 
  3. Op de **Broncontrole** weergeeft, schakelt de **netwerk** tabblad. Onder **processen met Network Activity**, controleert u of **cbengine.exe** actief een grote hoeveelheid gegevens verzendt.

       ![Schermafbeelding van de volumes onder processen met netwerkactiviteit](./media/vmware-azure-troubleshoot-replication/cbengine.png)

  Als cbengine.exe is niet een grote hoeveelheid gegevens verzendt, voltooi de stappen in de volgende secties.

* **Controleer of de processerver verbinding met Azure Blob-opslag maken kunt**.

  Selecteer **cbengine.exe**. Onder **TCP-verbindingen**, controleert u of er naar de URL van de Blog van het Azure-opslag met van de processerver is.

  ![Schermafbeelding van verbinding tussen cbengine.exe en de URL van de Azure Blob-opslag](./media/vmware-azure-troubleshoot-replication/rmonitor.png)

  Als er geen connectiviteit van de processerver op de Blog van het Azure storage-URL, in het Configuratiescherm is, selecteert u **Services**. Controleren of de volgende services worden uitgevoerd:

  *  cxprocessserver
  *  InMage Scout VX Agent-Sentinel/Outpost
  *  Microsoft Azure Recovery Services-agent
  *  Microsoft Azure Site Recovery-service
  *  tmansvc

  Starten of opnieuw starten van een service die actief is. Controleer of het probleem nog steeds voordoet.

* **Controleer of de processerver verbinding met Azure openbaar IP-adres maken kan via poort 443**.

  Open in %programfiles%\Microsoft Azure Recovery Services-Agent\Temp, de meest recente CBEngineCurr.errlog-bestand. Zoek in het bestand **443** of voor de tekenreeks **verbindingspoging is mislukt**.

  ![Schermafbeelding van de fout zich in de map Temp](./media/vmware-azure-troubleshoot-replication/logdetails1.png)

  Als er problemen worden weergegeven, klikt u op de opdrachtregel op de processerver, kunt u Telnet gebruiken om te pingen van uw Azure openbare IP-adres (het IP-adres wordt gemaskeerd in de vorige afbeelding). U kunt uw Azure openbare IP-adres vinden in het bestand CBEngineCurr.currLog via poort 443:

  `telnet <your Azure Public IP address as seen in CBEngineCurr.errlog>  443`

  Als u geen verbinding kunt maken, moet u controleren of het toegangsprobleem afkomstig vanwege een firewall of proxy-instellingen, is zoals beschreven in de volgende stap.

* **Controleer of de IP-adressen gebaseerde firewall op de processerver blokkeert de toegang**.

  Als u IP-adressen gebaseerde firewallregels op de server gebruikt, downloadt u de volledige lijst van [Microsoft Azure datacenter IP-adresbereiken](https://www.microsoft.com/download/details.aspx?id=41653). De IP-adresbereiken toevoegen aan uw firewallconfiguratie om ervoor te zorgen dat de firewall naar Azure (en de standaard HTTPS-poort 443)-communicatie toestaat. IP-adresbereiken voor de Azure-regio van uw abonnement en de regio VS-Azure West (gebruikt voor toegangsbeheer en identiteitsbeheer) toestaan.

* **Controleer of een URL-gebaseerde firewall op de processerver blokkeert de toegang**.

  Als u een URL-gebaseerde firewall-regel op de server gebruikt, voegt u de URL's die worden vermeld in de volgende tabel om de configuratie van de firewall:

[!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]  

*  **Controleer of proxy-instellingen op de processerver blokkeert de toegang**.

   Als u een proxyserver gebruikt, zorgt u ervoor dat de naam van de proxyserver wordt omgezet door de DNS-server. Om te controleren of de waarde die u hebt opgegeven bij het instellen van de configuratieserver, gaat u naar de registersleutel **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Azure Site Recovery\ProxySettings**.

   Vervolgens kunt u zorgen dat dezelfde instellingen worden gebruikt door de Azure Site Recovery-agent om gegevens te verzenden: 
      
   1. Zoeken naar **van Microsoft Azure Backup**. 
   2. Open **Microsoft Azure Backup**, en selecteer vervolgens **actie** > **eigenschappen wijzigen**. 
   3. Op de **proxyconfiguratie** tabblad ziet u de proxy-adres. Het proxyadres moet hetzelfde zijn als de proxy-adres dat wordt weergegeven in de registerinstellingen zijn. Als dat niet het geval is, moet u hetzelfde adres wijzigen.

*  **Controleer of de bandbreedte beperken op de processerver wordt beperkt**.

   De bandbreedte vergroten en controleer vervolgens of het probleem nog steeds voordoet.


## <a name="source-machine-isnt-listed-in-the-azure-portal"></a>Broncomputer niet wordt weergegeven in de Azure-portal

Wanneer u probeert om te selecteren van de bronmachine replicatie in te schakelen met behulp van Site Recovery, de computer mogelijk niet beschikbaar voor een van de volgende redenen:

* **Twee virtuele machines met hetzelfde exemplaar UUID**: Als twee virtuele machines onder het vCenter hetzelfde exemplaar UUID hebt, wordt de eerste virtuele machine is gedetecteerd door de configuratieserver wordt weergegeven in de Azure-portal. U lost dit probleem, zorg ervoor dat er geen twee virtuele machines hetzelfde exemplaar UUID. Dit scenario is gemeenschappelijk zichtbaar in gevallen waar een back-virtuele machine actief en is aangemeld bij onze discovery-gegevens. Raadpleeg [Azure Site Recovery van VMware naar Azure: Het opschonen van dubbele of verouderde vermeldingen](https://social.technet.microsoft.com/wiki/contents/articles/32026.asr-vmware-to-azure-how-to-cleanup-duplicatestale-entries.aspx) om op te lossen.
* **Onjuiste vCenter gebruikersreferenties**: Zorg ervoor dat u de referenties van de juiste vCenter toegevoegd bij het instellen van de configuratieserver met behulp van de OVF-sjabloon of geïntegreerde setup. Als u wilt controleren of de referenties die u hebt toegevoegd tijdens de installatie, Zie [referenties wijzigen voor automatische detectie](vmware-azure-manage-configuration-server.md#modify-credentials-for-automatic-discovery).
* **onvoldoende bevoegdheden voor vCenter**: Als de machtigingen voor toegang tot vCenter die niet de vereiste machtigingen hebt, is mislukt voor het detecteren van virtuele machines kan optreden. Zorg ervoor dat de machtigingen die worden beschreven in [een account voorbereiden voor automatische detectie](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-automatic-discovery) worden toegevoegd aan het account van de vCenter-gebruiker.
* **Azure Site Recovery-beheerservers**: Als de virtuele machine wordt gebruikt als de beheerserver in een of meer van de volgende rollen - configuratie server /scale-out processerver / Master-doelserver, en vervolgens kunt u zich niet kiezen van de virtuele machine uit de portal. Beheerservers kunnen niet worden gerepliceerd.
* **Al beveiligd/een failover via Azure Site Recovery-services**: Als de virtuele machine is al beveiligd of failover met Site Recovery, wordt de virtuele machine niet beschikbaar om te selecteren voor beveiliging in de portal. Zorg ervoor dat de virtuele machine die u in de portal zoekt al is niet beveiligd door een andere gebruiker of onder een ander abonnement.
* **niet verbonden vCenter**: Controleer of vCenter verbonden status heeft is. Als u wilt controleren, gaat u naar de Recovery Services-kluis > Site Recovery-infrastructuur > configuratieservers > Klik op de server van de desbetreffende configuratie > Er wordt een blade geopend aan de rechterkant met details van gekoppelde servers. Controleer of vCenter is verbonden. Als u in de status 'Niet verbonden' los het probleem en vervolgens [vernieuwen van de configuratieserver](vmware-azure-manage-configuration-server.md#refresh-configuration-server) in de portal. Na deze wordt virtuele machine weergegeven op de portal.
* **ESXi uitgeschakeld**: Als ESXi-host waarop de virtuele machine zich bevindt zich in de status, uitgeschakeld vervolgens virtuele machine, worden niet weergegeven of niet worden geselecteerd in de Azure portal. Inschakelen van de ESXi-host [vernieuwen van de configuratieserver](vmware-azure-manage-configuration-server.md#refresh-configuration-server) in de portal. Na deze wordt virtuele machine weergegeven op de portal.
* **Opnieuw opstarten**: Als er een opnieuw opstarten in behandeling op de virtuele machine, zal klikt u vervolgens u niet mogelijk om te selecteren van de machine in Azure portal. Zorg ervoor dat voor het voltooien van de activiteiten opnieuw opstarten in behandeling [vernieuwen van de configuratieserver](vmware-azure-manage-configuration-server.md#refresh-configuration-server). Na deze wordt virtuele machine weergegeven op de portal.
* **IP is niet gevonden**: Als de virtuele machine niet een geldig IP-adres dat is gekoppeld, klikt u vervolgens kunt u zich niet kunt selecteren van de machine in Azure portal. Zorg ervoor dat u een geldig IP-adres toewijzen aan de virtuele machine, [vernieuwen van de configuratieserver](vmware-azure-manage-configuration-server.md#refresh-configuration-server). Na deze wordt virtuele machine weergegeven op de portal.

## <a name="protected-virtual-machines-are-greyed-out-in-the-portal"></a>Beveiligde virtuele machines uit grijs worden weergegeven in de portal

Virtuele machines die worden gerepliceerd in Site Recovery niet beschikbaar zijn in de Azure-portal als er dubbele vermeldingen in het systeem. Raadpleeg voor meer informatie over het verwijderen van verouderde vermeldingen en los het probleem, [Azure Site Recovery van VMware naar Azure: Het opschonen van dubbele of verouderde vermeldingen](https://social.technet.microsoft.com/wiki/contents/articles/32026.asr-vmware-to-azure-how-to-cleanup-duplicatestale-entries.aspx).

## <a name="common-errors-and-recommended-steps-for-resolution"></a>Veelvoorkomende fouten en aanbevolen stappen voor het omzetten van

### <a name="initial-replication-issues-error-78169"></a>Initiële replicatie oplossen [fout 78169]

Via een bovenstaande ervoor zorgen dat er zijn geen connectiviteit, bandbreedte of tijd synchroniseren verwante problemen, zorg ervoor dat:

- Er is geen antivirussoftware wordt geblokkeerd door Azure Site Recovery. Informatie over [meer](vmware-azure-set-up-source.md#azure-site-recovery-folder-exclusions-from-antivirus-program) op uitsluitingen die vereist zijn voor Azure Site Recovery.

### <a name="application-consistency-recovery-point-missing-error-78144"></a>Toepassing consistentiepunt voor herstellen [fout 78144] ontbreekt

 Dit gebeurt vanwege problemen met Volume Shadow copy Service (VSS). U lost dit als volgt op: 
 
- Controleer of de geïnstalleerde versie van de Azure Site Recovery-agent is ten minste 9.22.2. 
- Controleren of de VSS-Provider is geïnstalleerd als een service in Windows-Services en Controleer ook of de MMC Component-Service om te controleren of Azure Site Recovery VSS Provider wordt weergegeven.
- Als de VSS-Provider niet is geïnstalleerd, raadpleegt u de [installatiefout probleemoplossingsartikel](vmware-azure-troubleshoot-push-install.md#vss-installation-failures).

- Als de VSS is uitgeschakeld,
    - Controleren of het opstarttype van de VSS-Provider-service is ingesteld op **automatische**.
    - Start opnieuw op de volgende services:
        - VSS-service
        - Azure Site Recovery VSS Provider
        - VDS-service

### <a name="high-churn-on-source-machine-error-78188"></a>Hoog verloop op de bronmachine [fout 78188]

Mogelijke oorzaken:
- De veranderingssnelheid van gegevens (geschreven bytes per seconde) op de vermelde schijven van de virtuele machine is meer dan de [Azure Site Recovery ondersteunde limieten](site-recovery-vmware-deployment-planner-analyze-report.md#azure-site-recovery-limits) voor opslagaccounttype voor de replicatie van doel.
- Er is een onverwachte piek in het verloop vanwege die grote hoeveelheden gegevens is in behandeling voor het uploaden.

Het probleem kunt oplossen:
- Zorg ervoor dat het account opslagtype (Standard of Premium) volgens de vereisten voor het tarief van verloop bij de bron is ingericht.
- Als het waargenomen verloop tijdelijk is, wacht u enkele uren voor het uploaden van de in behandeling gegevens voor meer informatie en om herstelpunten te maken.
- Als het probleem zich voordoen blijft, gebruikt u de ASR [implementatieplanner](site-recovery-deployment-planner.md#overview) om u te helpen bij het plannen van replicatie.

### <a name="no-heartbeat-from-source-machine-error-78174"></a>Geen Heartbeat van de bron-VM [fout 78174]

Dit gebeurt wanneer de Azure Site Recovery Mobility-agent op de bronmachine niet communiceert met de configuratieserver (CS).

Los het probleem, gebruikt u de volgende stappen uit om te controleren of de netwerkverbinding van de bron-VM naar de Config-Server:

1. Controleren of de bronmachine wordt uitgevoerd.
2. Aanmelden bij de bron-VM met een account dat beheerdersrechten heeft.
3. Controleer of de volgende services worden uitgevoerd en als de services niet opnieuw te starten:
   - Svagents (InMage Scout VX Agent)
   - InMage Scout Application Service
4. Controleer de logboeken op de locatie voor de details van fouten op de bronmachine:

       C:\Program Files (X86)\Microsoft Azure Site Recovery\agent\svagents*log
    
### <a name="no-heartbeat-from-process-server-error-806"></a>Geen heartbeat van de processerver [fout 806]
Als er is geen heartbeat van de proces-Server (PS), controleert u of:
1. PS Virtuele machine actief is
2. Controleer de volgende logboeken op de PS Foutdetails:

       C:\ProgramData\ASR\home\svsystems\eventmanager*.log
       and
       C:\ProgramData\ASR\home\svsystems\monitor_protection*.log

### <a name="no-heartbeat-from-master-target-error-78022"></a>Geen Heartbeat van de Masterdoelserver [fout 78022]

Dit gebeurt wanneer de Azure Site Recovery Mobility-agent op de Hoofddoelserver communiceert niet met de configuratieserver.

Gebruik de volgende stappen om te controleren of de status van het probleem op te lossen:

1. Controleren of de hoofd-doel-VM wordt uitgevoerd.
2. Meld u aan de hoofd-doel-VM met een account dat beheerdersrechten heeft.
    - Controleren of de svagents-service wordt uitgevoerd. Als deze wordt uitgevoerd, start de service opnieuw.
    - Raadpleeg de logboeken op de locatie voor de details van fout:
        
          C:\Program Files (X86)\Microsoft Azure Site Recovery\agent\svagents*log

### <a name="process-server-is-not-reachable-from-the-source-machine-error-78186"></a>Processerver is niet bereikbaar is vanaf de broncomputer [fout 78186]

Deze fout leidt tot een app en foutrapporten consistente punten niet wordt gegenereerd als deze niet wordt besproken. Los het probleem, volg de onderstaande koppelingen oplossen:
1. Zorg ervoor dat [PS-Services worden uitgevoerd](vmware-azure-troubleshoot-replication.md#monitor-process-server-health-to-avoid-replication-issues)
2. [Problemen met de netwerkverbinding van de bron machine controleren](vmware-azure-troubleshoot-replication.md#check-the-source-machine-for-connectivity-issues)
3. [Controleren op problemen met de netwerkverbinding van het proces server](vmware-azure-troubleshoot-replication.md#check-the-process-server-for-connectivity-issues) en volg de richtlijnen voor:
    - De connectiviteit met de bron controleren
    - Problemen met firewall en proxy

### <a name="data-upload-blocked-from-source-machine-to-process-server-error-78028"></a>Gegevens uploaden geblokkeerd vanaf broncomputer naar processerver [fout 78028]

Deze fout leidt tot een app en foutrapporten consistente punten niet wordt gegenereerd als deze niet wordt besproken. Los het probleem, volg de onderstaande koppelingen oplossen:

1. Zorg ervoor dat [PS-Services worden uitgevoerd](vmware-azure-troubleshoot-replication.md#monitor-process-server-health-to-avoid-replication-issues)
2. [Problemen met de netwerkverbinding van de bron machine controleren](vmware-azure-troubleshoot-replication.md#check-the-source-machine-for-connectivity-issues)
3. [Controleren op problemen met de netwerkverbinding van het proces server](vmware-azure-troubleshoot-replication.md#check-the-process-server-for-connectivity-issues) en volg de richtlijnen voor:
    - De connectiviteit met de bron controleren
    - Problemen met firewall en proxy

## <a name="next-steps"></a>Volgende stappen

Als u meer hulp nodig hebt, stel uw vraag in de [Azure Site Recovery-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr). We hebben een actieve community en een van onze technici u kan helpen.
