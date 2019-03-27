---
title: Wire Data-oplossing in Azure Monitor | Microsoft Docs
description: Gegevens van wire data is een geconsolideerde netwerk en de prestaties van gegevens van computers met Log Analytics-agents. Netwerkgegevens worden gecombineerd met uw logboekgegevens om te helpen bij het correleren van gegevens.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: fc3d7127-0baa-4772-858a-5ba995d1519b
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 10/03/2018
ms.author: magoedte
ms.openlocfilehash: 5a4ba784402774750d4d7770652589b598ee00d8
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58485574"
---
# <a name="wire-data-20-preview-solution-in-azure-monitor"></a>Wire Data 2.0 (Preview) solution in Azure Monitor

![Symbool Wire Data](media/wire-data/wire-data2-symbol.png)

Gegevens van wire data is een geconsolideerde netwerk en prestaties van gegevens die worden verzameld van verbonden Windows en Linux-verbonden computers met de Log Analytics-agent, met inbegrip van die worden bewaakt door Operations Manager in uw omgeving. Netwerkgegevens worden gecombineerd met uw andere logboekgegevens om te helpen bij het correleren van gegevens.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

Naast de Log Analytics-agent gebruikt de gegevens van Wire Data-oplossing Microsoft afhankelijkheid Agents die u op computers in uw IT-infrastructuur installeert. Agents voor afhankelijkheden controleren netwerkgegevens die worden verzonden naar en van uw computers voor netwerkniveaus 2-3 in het [OSI-model](https://en.wikipedia.org/wiki/OSI_model), met inbegrip van de verschillende gebruikte protocollen en poorten. Gegevens worden vervolgens verzonden naar Azure Monitor met behulp van agents.  

>[!NOTE]
>Als u Serviceoverzicht al hebt geïmplementeerd of Serviceoverzicht overweegt of [Azure Monitor voor virtuele machines](../../azure-monitor/insights/vminsights-overview.md), er is een nieuwe metrische gegevens verbindingsset te verzamelen en opslaan in Azure Monitor die vergelijkbare informatie aan gegevens van Wire Data biedt.

Azure Monitor registreert standaard gegevens voor CPU, geheugen, schijf en netwerk-prestatiegegevens van de items die zijn ingebouwd in Windows en Linux, evenals andere prestatiemeteritems die u kunt opgeven. Het verzamelen van netwerk- en andere gegevens wordt voor elke agent in realtime uitgevoerd, met inbegrip van subnetten en protocollen op toepassingsniveau die door de computer worden gebruikt.  Wire Data kijkt naar netwerkgegevens op toepassingsniveau, niet naar die op de TCP-transportlaag.  De oplossing kijkt niet naar afzonderlijke ACK's en SYN's.  Zodra de handshake is voltooid, wordt dit als een live-verbinding beschouwd en wordt deze gemarkeerd als verbonden. Die verbinding blijft actief zolang beide zijden het erover eens zijn dat de socket geopend is en gegevens heen en weer kunnen worden gestuurd.  Wanneer een van beide zijden de verbinding sluit, wordt deze gemarkeerd als Verbroken.  Daarom wordt alleen de bandbreedte van voltooide pakketten meegeteld en wordt niet gemeld of pakketten opnieuw of niet zijn verzonden.

Als u [sFlow](http://www.sflow.org/) of andere software hebt gebruikt met het [NetFlow-protocol van Cisco](https://www.cisco.com/c/en/us/products/collateral/ios-nx-os-software/ios-netflow/prod_white_paper0900aecd80406232.html), zullen de statistieken en gegevens die u in gegevens van Wire Data ziet, u bekend voorkomen.

Enkele van de typen ingebouwde query's voor zoeken in logboeken:

- Agents die gegevens van Wire Data leveren
- IP-adressen van agents die gegevens van Wire Data leveren
- Uitgaande communicatie per IP-adres
- Aantal bytes verzonden per toepassingsprotocol
- Aantal bytes verzonden per toepassingsservice
- Bytes ontvangen door verschillende protocollen
- Totaal aantal bytes verzonden en ontvangen per IP-versie
- Gemiddelde latentie voor verbindingen die betrouwbaar zijn gemeten
- Computerprocessen waarmee netwerkverkeer is gestart of ontvangen
- Hoeveelheid netwerkverkeer voor een proces

Wanneer u zoekt met behulp van Wire Data, kunt u gegevens filteren en groeperen zodat u informatie kunt bekijken over de belangrijkste agents en protocollen. U kunt ook zien wanneer bepaalde computers (IP-adressen/MAC-adressen) met elkaar hebben gecommuniceerd, hoelang dat duurde en hoeveel gegevens er zijn verzonden. In wezen bekijkt u metagegevens over het netwerkverkeer, wat op zoeken is gebaseerd.

Maar aangezien u metagegevens bekijkt, is dat niet per se nuttig voor een diepgaande probleemoplossing. Gegevens van wire data in Azure Monitor is niet een volledige vastleggen van gegevens van het netwerk.  De oplossing is niet bedoeld voor het oplossen van problemen op een diep pakketniveau. In vergelijking met andere methoden is het voordeel van het gebruik van de agent dat u geen apparaten hoeft te installeren, geen netwerkswitches opnieuw hoeft te configureren of ingewikkeld configuraties moet uitvoeren. Gegevens van Wire Data zijn gewoon gebaseerd op agents: u installeert de agent op een computer en de agent zal dan het eigen netwerkverkeer controleren. Een ander voordeel is wanneer u werkbelastingen in cloudproviders of een serviceprovider of Microsoft Azure wilt hosten, waarbij de gebruiker niet de eigenaar van de infrastructuurlaag is.

## <a name="connected-sources"></a>Verbonden bronnen

Wire Data haalt zijn gegevens uit de Microsoft-agent voor afhankelijkheden. De Agent voor afhankelijkheden, is afhankelijk van de Log Analytics-agent voor de verbindingen met Azure Monitor. Dit betekent dat een server de Log Analytics-agent geïnstalleerd en geconfigureerd met de agent voor afhankelijkheden moet hebben. De volgende tabel beschrijft de verbonden bronnen die worden ondersteund door Wire Data.

| **Verbonden bron** | **Ondersteund** | **Beschrijving** |
| --- | --- | --- |
| Windows-agents | Ja | Wire Data analyseert en verzamelt gegevens van Windows-agentcomputers. <br><br> Naast de [Log Analytics-agent voor Windows](../../azure-monitor/platform/agent-windows.md), Windows-agents vereist de Microsoft-Agent voor afhankelijkheden. Zie de [ondersteunde besturingssystemen](../../azure-monitor/insights/service-map-configure.md#supported-windows-operating-systems) voor een volledige lijst met versies van besturingssystemen. |
| Linux-agents | Ja | Wire Data analyseert en verzamelt gegevens van Linux-agentcomputers.<br><br> Naast de [Log Analytics-agent voor Linux](../../azure-monitor/learn/quick-collect-linux-computer.md), Linux-agents vereist de Microsoft-Agent voor afhankelijkheden. Zie de [ondersteunde besturingssystemen](../../azure-monitor/insights/service-map-configure.md#supported-linux-operating-systems) voor een volledige lijst met versies van besturingssystemen. |
| Beheergroep System Center Operations Manager | Ja | Wire Data analyseert en verzamelt gegevens van Windows- en Linux-agents in een verbonden [System Center Operations Manager-beheergroep](../../azure-monitor/platform/om-agents.md). <br><br> Er is een directe verbinding van de System Center Operations Manager agent-computer naar Azure Monitor vereist. |
| Azure Storage-account | Nee | Omdat Wire Data gegevens van agentcomputers verzamelt, zijn er geen gegevens te verzamelen van Azure Storage. |

Op Windows, wordt de Microsoft Monitoring Agent (MMA) door System Center Operations Manager en Azure Monitor gebruikt om te verzamelen en verzenden van gegevens. Afhankelijk van de context, wordt de agent de System Center Operations Manager-Agent, de Log Analytics-agent, de MMA of Direct Agent genoemd. System Center Operations Manager en Azure Monitor bieden enigszins verschillende versies van de MMA. Deze versies kunnen elk rapport naar System Center Operations Manager, naar Azure Monitor of op beide.

Op Linux, de Log Analytics-agent voor Linux verzamelt en verzendt gegevens naar Azure Monitor. U kunt gegevens van Wire Data gebruiken op servers met Azure Monitor rechtstreeks verbonden agents of op servers die zijn verbonden met Azure Monitor via System Center Operations Manager-beheergroepen.

De agent voor afhankelijkheden stuurt zelf geen gegevens door en er zijn geen wijzigingen in de firewalls en poorten voor nodig. De gegevens in de gegevens van Wire Data worden altijd verzonden door de Log Analytics-agent naar Azure Monitor, rechtstreeks of via de gateway van Log Analytics.

![diagram van agent](./media/wire-data/agents.png)

Als u een System Center Operations Manager-gebruiker met een beheergroep die is verbonden met Azure Monitor:

- Er is geen aanvullende configuratie vereist wanneer de System Center Operations Manager-agents kunnen toegang tot het Internet verbinding maken met Azure Monitor.
- U moet de Log Analytics-gateway met System Center Operations Manager te werken wanneer de System Center Operations Manager-agents geen toegang Azure Monitor via Internet tot configureren.

Als uw Windows- of Linux-computers kunnen niet rechtstreeks verbinding met de service maken, moet u de Log Analytics-agent verbinding maken met Azure Monitor met behulp van de Log Analytics-gateway configureren. U kunt de Log Analytics gateway downloaden vanaf de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=52666).

## <a name="prerequisites"></a>Vereisten

- Hiervoor is de oplossing [Insight and Analytics](https://www.microsoft.com/cloud-platform/operations-management-suite-pricing) vereist.
- Als u de vorige versie van Wire Data gebruikt, moet u deze eerst verwijderen. Alle gegevens die zijn vastgelegd via de oorspronkelijke versie van Wire Data zijn echter nog steeds beschikbaar in Wire Data 2.0 en in zoeken in logboeken.
- Er zijn beheerdersbevoegdheden vereist om de agent voor afhankelijkheden te installeren of verwijderen.
- De agent voor afhankelijkheden moet worden geïnstalleerd op een computer met een 64-bits besturingssysteem.

### <a name="operating-systems"></a>Besturingssystemen

In de volgende secties worden de ondersteunde besturingssystemen voor de agent voor afhankelijkheden vermeld. Wire Data biedt geen ondersteuning voor 32-bits architecturen van besturingssystemen.

#### <a name="windows-server"></a>Windows Server

- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2 SP1

#### <a name="windows-desktop"></a>Windows-bureaublad

- Windows 10
- Windows 8.1
- Windows 8
- Windows 7

#### <a name="red-hat-enterprise-linux-centos-linux-and-oracle-linux-with-rhel-kernel"></a>Red Hat Enterprise Linux, CentOS Linux en Oracle Linux (met RHEL Kernel)

- Alleen standaard- en SMP Linux kernelversies worden ondersteund.
- Niet-standaard kernelversies, zoals PAE en Xen, worden voor geen enkele Linux-distributie ondersteund. Een systeem met de versietekenreeks _2.6.16.21-0.8-xen_ wordt bijvoorbeeld niet ondersteund.
- Aangepaste kernels, met inbegrip van hercompilaties van standaardkernels, worden niet ondersteund.
- CentOSPlus-kernel wordt niet ondersteund.
- Oracle Unbreakable Enterprise Kernel (UEK) wordt verderop in dit artikel beschreven.

#### <a name="red-hat-linux-7"></a>Red Hat Linux 7

| **Versie van het besturingssysteem** | **Kernelversie** |
| --- | --- |
| 7.0 | 3.10.0-123 |
| 7.1 | 3.10.0-229 |
| 7.2 | 3.10.0-327 |
| 7.3 | 3.10.0-514 |

#### <a name="red-hat-linux-6"></a>Red Hat Linux 6

| **Versie van het besturingssysteem** | **Kernelversie** |
| --- | --- |
| 6.0 | 2.6.32-71 |
| 6.1 | 2.6.32-131 |
| 6.2 | 2.6.32-220 |
| 6.3 | 2.6.32-279 |
| 6.4 | 2.6.32-358 |
| 6.5 | 2.6.32-431 |
| 6.6 | 2.6.32-504 |
| 6.7 | 2.6.32-573 |
| 6.8 | 2.6.32-642 |

#### <a name="red-hat-linux-5"></a>Red Hat Linux 5

| **Versie van het besturingssysteem** | **Kernelversie** |
| --- | --- |
| 5.8 | 2.6.18-308 |
| 5.9 | 2.6.18-348 |
| 5.10 | 2.6.18-371 |
| 5.11 | 2.6.18-398 <br> 2.6.18-400 <br>2.6.18-402 <br>2.6.18-404 <br>2.6.18-406 <br> 2.6.18-407 <br> 2.6.18-408 <br> 2.6.18-409 <br> 2.6.18-410 <br> 2.6.18-411 <br> 2.6.18-412 <br> 2.6.18-416 <br> 2.6.18-417 <br> 2.6.18-419 |

#### <a name="oracle-enterprise-linux-with-unbreakable-enterprise-kernel"></a>Oracle Enterprise Linux met Unbreakable Enterprise Kernel

#### <a name="oracle-linux-6"></a>Oracle Linux 6

| **Versie van het besturingssysteem** | **Kernelversie** |
| --- | --- |
| 6.2 | Oracle 2.6.32-300 (UEK R1) |
| 6.3 | Oracle 2.6.39-200 (UEK R2) |
| 6.4 | Oracle 2.6.39-400 (UEK R2) |
| 6.5 | Oracle 2.6.39-400 (UEK R2 i386) |
| 6.6 | Oracle 2.6.39-400 (UEK R2 i386) |

#### <a name="oracle-linux-5"></a>Oracle Linux 5

| **Versie van het besturingssysteem** | **Kernelversie** |
| --- | --- |
| 5.8 | Oracle 2.6.32-300 (UEK R1) |
| 5.9 | Oracle 2.6.39-300 (UEK R2) |
| 5.10 | Oracle 2.6.39-400 (UEK R2) |
| 5.11 | Oracle 2.6.39-400 (UEK R2) |

#### <a name="suse-linux-enterprise-server"></a>SUSE Linux Enterprise Server

#### <a name="suse-linux-11"></a>SUSE Linux 11

| **Versie van het besturingssysteem** | **Kernelversie** |
| --- | --- |
| 11 | 2.6.27 |
| 11 SP1 | 2.6.32 |
| 11 SP2 | 3.0.13 |
| 11 SP3 | 3.0.76 |
| 11 SP4 | 3.0.101 |

#### <a name="suse-linux-10"></a>SUSE Linux 10

| **Versie van het besturingssysteem** | **Kernelversie** |
| --- | --- |
| 10 SP4 | 2.6.16.60 |

#### <a name="dependency-agent-downloads"></a>Downloads agent voor afhankelijkheden

| **File** | **Besturingssysteem** | **Versie** | **SHA-256** |
| --- | --- | --- | --- |
| [InstallDependencyAgent-Windows.exe](https://aka.ms/dependencyagentwindows) | Windows | 9.0.5 | 73B3F6A2A76A08D58F72A550947FF839B588591C48E6EDDD6DDF73AA3FD82B43 |
| [InstallDependencyAgent-Linux64.bin](https://aka.ms/dependencyagentlinux) | Linux | 9.0.5 | A1BAD0B36EBF79F2B69113A07FCF48C68D90BD169C722689F9C83C69FC032371 |



## <a name="configuration"></a>Configuratie

Voer de volgende stappen uit om Wire Data te configureren voor uw werkruimten.

1. Inschakelen van de oplossing Activity Log Analytics van de [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.WireData2OMS?tab=Overview) of met behulp van de procedure beschreven in [mnitoring oplossingen toevoegen vanuit de galerie van oplossingen](../../azure-monitor/insights/solutions.md).
2. Installeer de agent voor afhankelijkheden op elke computer waarop u gegevens wilt ophalen. De agent voor afhankelijkheden kan verbindingen met computers in de directe nabijheid controleren, zodat er wellicht geen agent op elke computer nodig is.

> [!NOTE]
> U kunt de vorige versie van Wire Data niet toevoegen aan nieuwe werkruimten. Als u de oorspronkelijke versie van Wire Data hebt ingeschakeld, kunt u die blijven gebruiken. Als u echter Wire Data 2.0 wilt gebruiken, moet u eerst de oorspronkelijke versie verwijderen.
> 
> ### <a name="install-the-dependency-agent-on-windows"></a>De agent voor afhankelijkheden installeren op Windows

Er zijn beheerdersbevoegdheden vereist om de agent te installeren of verwijderen.

De agent voor afhankelijkheden wordt geïnstalleerd op computers met Windows via InstallDependencyAgent-Windows.exe. Als u dit uitvoerbare bestand zonder opties uitvoert, wordt er een wizard gestart die u kunt volgen om interactief te installeren.

Gebruik de volgende stappen voor het installeren van de agent voor afhankelijkheden op elke computer met Windows:

1. De volgende stappen in Log Analytics-agent installeren [gegevens verzamelen van Windows-computers die worden gehost in uw omgeving](../../azure-monitor/platform/agent-windows.md).
2. Download de Windows-agent voor afhankelijkheden met behulp van de koppeling in de vorige sectie en voer de agent uit met behulp van de volgende opdracht: `InstallDependencyAgent-Windows.exe`
3. Volg de wizard om de agent te installeren.
4. Als de agent voor afhankelijkheden niet start, controleert u de logboeken voor uitgebreide foutinformatie. Voor Windows-agents is dit de logboekmap: %Programfiles%\Microsoft Dependency Agent\logs.

#### <a name="windows-command-line"></a>Windows-opdrachtregel

Gebruik opties uit de volgende tabel om de agent te installeren vanaf een opdrachtregel. Een lijst van vlaggen voor de installatie wilt bekijken, voert u het installatieprogramma met behulp van de /? markeren als volgt.

InstallDependencyAgent-Windows.exe /?

| **Vlag** | **Beschrijving** |
| --- | --- |
| <code>/?</code> | Een lijst met de opdrachtregelopties ophalen. |
| <code>/S</code> | Een installatie op de achtergrond uitvoeren zonder gebruikersvragen. |

Bestanden voor de Windows-agent voor afhankelijkheden worden standaard geplaatst in C:\Program Files\Microsoft Dependency Agent.

### <a name="install-the-dependency-agent-on-linux"></a>De agent voor afhankelijkheden installeren op Linux

Toegang tot de hoofdmap is vereist om de agent te installeren of configureren.

De agent voor afhankelijkheden wordt geïnstalleerd op Linux-computers via InstallDependencyAgent-Linux64.bin, een shell-script met een zelfuitpakkend binair bestand. U kunt het bestand uitvoeren met behulp van _sh_ of door aan het bestand zelf schrijfrechten toe te kennen.

Gebruik de volgende stappen voor het installeren van de agent voor afhankelijkheden op elke Linux-computer:

1. De volgende stappen in Log Analytics-agent installeren [gegevens verzamelen van Linux-computers die worden gehost in uw omgeving](../../azure-monitor/learn/quick-collect-linux-computer.md#obtain-workspace-id-and-key).
2. Download de Linux-agent voor afhankelijkheden met behulp van de koppeling in de vorige sectie en installeer deze als hoofdmap met de volgende opdracht: sh InstallDependencyAgent-Linux64.bin
3. Als de agent voor afhankelijkheden niet start, controleert u de logboeken voor uitgebreide foutinformatie. Voor Linux-agents is dit de logboekmap: /var/opt/microsoft/dependency-agent/log.

Als u een overzicht van de installatievlaggen wilt zien, voert u als volgt het installatieprogramma uit met behulp van de `-help`-vlag.

```
InstallDependencyAgent-Linux64.bin -help
```

| **Vlag** | **Beschrijving** |
| --- | --- |
| <code>-help</code> | Een lijst met de opdrachtregelopties ophalen. |
| <code>-s</code> | Een installatie op de achtergrond uitvoeren zonder gebruikersvragen. |
| <code>--check</code> | Controleer machtigingen en het besturingssysteem, maar installeer niet de agent. |

Bestanden voor de agent voor afhankelijkheden worden in de volgende mappen geplaatst:

| **Bestanden** | **Locatie** |
| --- | --- |
| Kernbestanden | /opt/microsoft/dependency-agent |
| Logboekbestanden | /var/opt/microsoft/dependency-agent/log |
| Configuratiebestanden | /etc/opt/microsoft/dependency-agent/config |
| Uitvoerbare bestanden van de service | /opt/microsoft/dependency-agent/bin/microsoft-dependency-agent<br><br>/opt/microsoft/dependency-agent/bin/microsoft-dependency-agent-manager |
| Binaire opslagbestanden | /var/opt/microsoft/dependency-agent/storage |

### <a name="installation-script-examples"></a>Voorbeelden van installatiescript

Als u de agent voor afhankelijkheden op een groot aantal servers tegelijk wilt implementeren, is het handig om een script gebruiken. U kunt de volgende scriptvoorbeelden gebruiken om de agent voor afhankelijkheden op Windows of Linux te downloaden en installeren.

#### <a name="powershell-script-for-windows"></a>PowerShell-script voor Windows

```powershell

Invoke-WebRequest "https://aka.ms/dependencyagentwindows" -OutFile InstallDependencyAgent-Windows.exe

.\InstallDependencyAgent-Windows.exe /S

```

#### <a name="shell-script-for-linux"></a>Shell-script voor Linux

```
wget --content-disposition https://aka.ms/dependencyagentlinux -O InstallDependencyAgent-Linux64.bin
```

```
sh InstallDependencyAgent-Linux64.bin -s
```

### <a name="desired-state-configuration"></a>Desired State Configuration

Voor het implementeren van de agent voor afhankelijkheden via Desired State Configuration kunt u de module xPSDesiredStateConfiguration en een stukje code zoals dit gebruiken:

```
Import-DscResource -ModuleName xPSDesiredStateConfiguration

$DAPackageLocalPath = "C:\InstallDependencyAgent-Windows.exe"



Node $NodeName

{

    # Download and install the Dependency Agent

    xRemoteFile DAPackage

    {

        Uri = "https://aka.ms/dependencyagentwindows"

        DestinationPath = $DAPackageLocalPath

        DependsOn = "[Package]OI"

    }

    xPackage DA

    {

        Ensure = "Present"

        Name = "Dependency Agent"

        Path = $DAPackageLocalPath

        Arguments = '/S'

        ProductId = ""

        InstalledCheckRegKey = "HKEY\_LOCAL\_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\DependencyAgent"

        InstalledCheckRegValueName = "DisplayName"

        InstalledCheckRegValueData = "Dependency Agent"

    }

}

```
### <a name="uninstall-the-dependency-agent"></a>De agent voor afhankelijkheden verwijderen

Gebruik de volgende secties voor hulp bij het verwijderen van de agent voor afhankelijkheden.

#### <a name="uninstall-the-dependency-agent-on-windows"></a>De agent voor afhankelijkheden verwijderen op Windows

Een beheerder kan de Windows-agent voor afhankelijkheden verwijderen via het Configuratiescherm.

Een beheerder kan ook %Programfiles%\Microsoft Dependency Agent\Uninstall.exe uitvoeren om de agent te verwijderen.

#### <a name="uninstall-the-dependency-agent-on-linux"></a>De agent voor afhankelijkheden verwijderen op Linux

Om de agent voor afhankelijkheden volledig te verwijderen van Linux, moet u de agent zelf en de connector verwijderen, die automatisch samen met de agent wordt geïnstalleerd. U kunt beide verwijderen met behulp van de volgende opdracht:

```
rpm -e dependency-agent dependency-agent-connector
```

## <a name="management-packs"></a>Management packs

Wanneer Wire Data wordt geactiveerd in een werkruimte van Log Analytics, wordt een management pack van 300 kB verzonden naar alle Windows-servers in die werkruimte. Als u System Center Operations Manager-agents in een [verbonden beheergroep](../platform/om-agents.md) gebruikt, wordt het management pack Afhankelijkheidsmonitor geïmplementeerd vanuit System Center Operations Manager. Als de agents rechtstreeks verbonden zijn, biedt Azure Monitor het managementpack.

De naam van het management pack is Microsoft.IntelligencePacks.ApplicationDependencyMonitor. Het management pack wordt geschreven naar: %Microsoft Monitoring Agent\Agent\Health Service State\Management Packs. De gegevensbron waarvan het management pack gebruikmaakt is: %Program files%\Microsoft Monitoring Agent\Agent\Health Service State\Resources&lt;AutoGeneratedID&gt;\Microsoft.EnterpriseManagement.Advisor.ApplicationDependencyMonitorDataSource.dll.

## <a name="using-the-solution"></a>De oplossing gebruiken

**De oplossing installeren en configureren**

Gebruik de volgende informatie om de oplossing te installeren en configureren.

- De oplossing Wire Data verkrijgt gegevens van computers met Windows Server 2012 R2, Windows 8.1 en latere besturingssystemen.
- Microsoft .NET Framework 4.0 of hoger is vereist op computers waarvan u draadgegevens wilt ophalen.
- De gegevens van Wire Data-oplossing toevoegt aan uw Log Analytics-werkruimte met behulp van de procedure beschreven in [toevoegen bewakingsoplossingen van de galerie van oplossingen](solutions.md). Er is geen verdere configuratie nodig.
- Als u draadgegevens voor een specifieke oplossing wilt weergeven, moet de oplossing al zijn toegevoegd aan uw werkruimte.

Nadat agents zijn geïnstalleerd en u de oplossing installeert, wordt de tegel Wire Data 2.0 weergegeven in de werkruimte.

![Tegel Wire Data](./media/wire-data/wire-data-tile.png)

## <a name="using-the-wire-data-20-solution"></a>De oplossing Wire Data 2.0 gebruiken

Op de pagina **Overzicht** voor uw Log Analytics-werkruimte in de Azure Portal klikt u op de tegel **Wire Data 2.0** om het dashboard van Wire Data te openen. Het dashboard bevat de blades in de volgende tabel. Elke blade bevat maximaal tien items die overeenkomen met de criteria voor het opgegeven bereik en de duur van die blade. U kunt zoeken in logboeken waarmee alle records worden geretourneerd door onderaan in de blade te klikken op **Alles bekijken** of door te klikken op de koptekst van de blade.

| **Blade** | **Beschrijving** |
| --- | --- |
| Agents waarmee het netwerkverkeer wordt vastgelegd | Toont het aantal agents dat netwerkverkeer vastlegt en geeft de eerste 10 computers weer die het meeste verkeer vastleggen. Klik op het nummer om zoeken in logboeken uit te voeren voor <code>WireData \| summarize sum(TotalBytes) by Computer \| take 500000</code>. Klik op een computer in de lijst om zoeken in logboeken uit te voeren waarmee het totale aantal vastgelegde bytes wordt geretourneerd. |
| Lokale subnetten | Toont het aantal lokale subnetten dat door agents is gedetecteerd.  Klik op het nummer om zoeken in logboeken uit te voeren voor <code>WireData \| summarize sum(TotalBytes) by LocalSubnet</code>, waarmee alle subnetten worden weergegeven met het aantal bytes dat via elk daarvan is verzonden. Klik op een subnet in de lijst om zoeken in logboeken uit te voeren waarmee het totale aantal via het subnet verzonden bytes wordt geretourneerd. |
| Protocollen op toepassingsniveau | Toont het aantal protocollen op toepassingsniveau in gebruik, zoals gedetecteerd door agents. Klik op het nummer om zoeken in logboeken uit te voeren voor <code>WireData \| summarize sum(TotalBytes) by ApplicationProtocol</code>. Klik op een protocol om zoeken in logboeken uit te voeren waarmee het totale aantal met het protocol verzonden bytes wordt geretourneerd. |

![Wire Data-dashboard](./media/wire-data/wire-data-dash.png)

U kunt de blade **Agents waarmee het netwerkverkeer wordt vastgelegd** gebruiken om te bepalen hoeveel netwerkbandbreedte wordt verbruikt door computers. Met deze blade kunt u gemakkelijk de _drukst bezette_ computer in uw omgeving terugvinden. Deze computers kunnen overbelast zijn, zich abnormaal gedragen of meer netwerkbronnen dan normaal gebruiken.

![voorbeeld van zoeken in logboek](./media/wire-data/log-search-example01.png)

Op soortgelijke wijze kunt u de blade **Lokale subnetten** gebruiken om te bepalen hoeveel netwerkverkeer via uw subnetten plaatsvindt. Gebruikers definiëren vaak subnetten rondom essentiële gebieden voor hun toepassingen. Deze blade geeft een beeld van die gebieden.

![voorbeeld van zoeken in logboek](./media/wire-data/log-search-example02.png)

De blade **Protocollen op toepassingsniveau** is handig omdat u hiermee kunt vaststellen welke protocollen er worden gebruikt. U verwacht bijvoorbeeld dat SSH niet in uw netwerkomgeving wordt gebruikt. Aan de hand van de informatie in de blade kunt u dan snel vaststellen of die verwachting al dan niet is uitgekomen.

![voorbeeld van zoeken in logboek](./media/wire-data/log-search-example03.png)

Het is ook handig om te weten als protocolverkeer gedurende een bepaalde periode toe- of afneemt. Als bijvoorbeeld de hoeveelheid gegevens die wordt verzonden door een toepassing toeneemt, is dat misschien iets waarmee u rekening wilt houden.

## <a name="input-data"></a>Invoergegevens

Wire Data verzamelt metagegevens over netwerkverkeer met behulp van de agents die u hebt ingeschakeld. Elke agent verzendt ongeveer elke 15 seconden gegevens.

## <a name="output-data"></a>Uitvoergegevens

Een record met het type _WireData_ is gemaakt voor elk type invoergegevens. WireData-records hebben eigenschappen die worden weergegeven in de volgende tabel:

| Eigenschap | Description |
|---|---|
| Computer | De naam van de computer waar gegevens zijn verzameld |
| TimeGenerated | Tijd van de record |
| LocalIP | IP-adres van de lokale computer |
| SessionState | Verbonden of verbroken |
| ReceivedBytes | Hoeveelheid ontvangen bytes |
| ProtocolName | Naam van het gebruikte netwerkprotocol |
| IPVersion | IP-versie |
| Richting | Binnenkomend of uitgaand |
| MaliciousIP | IP-adres van een bekende schadelijke bron |
| Severity | Ernst van mogelijke malware |
| RemoteIPCountry | Land van het externe IP-adres |
| ManagementGroupName | Naam van de Operations Manager-beheergroep |
| SourceSystem | Bron waar gegevens zijn verzameld |
| SessionStartTime | Starttijd van de sessie |
| SessionEndTime | Eindtijd van de sessie |
| LocalSubnet | Subnet waar gegevens zijn verzameld |
| LocalPortNumber | Nummer van de lokale poort |
| RemoteIP | Extern IP-adres gebruikt door de externe computer |
| RemotePortNumber | Poortnummer gebruikt door het externe IP-adres |
| SessionID | Een unieke waarde die de communicatiesessie tussen twee IP-adressen aangeeft |
| SentBytes | Aantal verzonden bytes |
| TotalBytes | Totaal aantal bytes dat tijdens de sessie is verzonden |
| ApplicationProtocol | Type van het gebruikte netwerkprotocol   |
| ProcessID | Windows-proces-id |
| ProcessName | Pad en bestandsnaam van het proces |
| RemoteIPLongitude | Waarde IP-lengtegraad |
| RemoteIPLatitude | Waarde IP-breedtegraad |


## <a name="next-steps"></a>Volgende stappen

- [Zoeken in een logboek](../../azure-monitor/log-query/log-query-overview.md) om gedetailleerde zoekrecords van Wire Data te bekijken.
