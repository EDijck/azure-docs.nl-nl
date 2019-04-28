---
title: Een processerver voor herstel na noodgevallen van virtuele VMware-machines en fysieke servers naar Azure met behulp van Azure Site Recovery beheren | Microsoft Docs
description: Dit artikel wordt beschreven voor het beheren van een processerver instellen voor herstel na noodgevallen van virtuele VMware-machines en fysieke server naar Azure met Azure Site Recovery.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 03/11/2019
ms.author: ramamill
ms.openlocfilehash: 0a0b6c83f800c0a479ba7a16c91b497d1a11da9e
ms.sourcegitcommit: a95dcd3363d451bfbfea7ec1de6813cad86a36bb
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "62732482"
---
# <a name="manage-process-servers"></a>Processervers beheren

De processerver die wordt gebruikt wanneer u virtuele VMware-machines of fysieke servers naar Azure repliceert wordt standaard geïnstalleerd op de servercomputer van de on-premises configuratie. Er zijn een aantal instanties waarin u moet voor het instellen van een afzonderlijk proces-server:

- Voor grote implementaties moet u mogelijk extra on-premises processervers dat moet capaciteit kunt schalen.
- Voor failback, moet u een tijdelijke processerver in Azure instellen. Wanneer failback is voltooid, kunt u deze virtuele machine verwijderen. 

In dit artikel bevat een overzicht van veelvoorkomende beheertaken voor deze extra processervers.

## <a name="upgrade-a-process-server"></a>Upgrade van een processerver

Een processerver die wordt uitgevoerd on-premises of in Azure (voor failback testdoeleinden), als volgt bijwerken:

[!INCLUDE [site-recovery-vmware-upgrade -process-server](../../includes/site-recovery-vmware-upgrade-process-server-internal.md)]

> [!NOTE]
>   Wanneer u de installatiekopie van de Azure-galerie gebruiken om u te maken van een processerver in Azure voor de doeleinden van failback, wordt dit meestal de meest recente beschikbare versie uitgevoerd. De Site Recovery teams release oplossingen en verbeteringen op gezette tijden, en we raden dat u processervers up-to-date houden.

## <a name="balance-the-load-on-process-server"></a>Verdeel de belasting op de processerver

Om taken te verdelen tussen twee processervers mogelijk

1. Navigeer naar **Recovery Services-kluis** > **beheren** > **infrastructuur voor Site Recovery** > **voor VMware en fysieke machines** > **configuratieservers**.
2. Klik op de configuratieserver waaraan de processervers zijn geregistreerd bij.
3. Lijst met processervers die zijn geregistreerd bij de van configuratieservers zijn beschikbaar op de pagina.
4. Klik op de processerver op die u wilt wijzigen van de werkbelasting.

    ![LoadBalance](media/vmware-azure-manage-process-server/LoadBalance.png)

5. U kunt beide gebruiken **taakverdeling** of **Switch** opties, zoals wordt uitgelegd, volgens de vereisten.

### <a name="load-balance"></a>Taakverdeling

Via deze optie is, u kunt een of meer virtuele machines selecteren en kunt overbrengen naar een andere processerver.

1. Klik op **verdelen**, selecteer de doelprocesserver in de vervolgkeuzelijst omlaag. Klik op **OK**

    ![LoadPS](media/vmware-azure-manage-process-server/LoadPS.PNG)

2. Klik op **machines selecteren**, kiest u de virtuele machines die u wilt verplaatsen van de huidige processerver naar de doelprocesserver. Details van de gemiddelde gegevenswijziging worden weergegeven op basis van elke virtuele machine.
3. Klik op **OK**. De voortgang van de taak onder **Recovery Services-kluis** > **bewaking** > **Site Recovery-taken**.
4. Het duurt 15 minuten om de wijzigingen in overeenstemming met na voltooiing van deze bewerking of [vernieuwen van de configuratieserver](vmware-azure-manage-configuration-server.md#refresh-configuration-server) voor onmiddellijk van kracht wordt.

### <a name="switch"></a>Switch

Hele werkbelasting die wordt beschermd onder een processerver wordt tot en met deze optie wordt verplaatst naar een andere processerver.

1. Klik op **Switch**, de doelprocesserver selecteren, klikt u op **OK**.

    ![Switch](media/vmware-azure-manage-process-server/Switch.PNG)

2. De voortgang van de taak onder **Recovery Services-kluis** > **bewaking** > **Site Recovery-taken**.
3. Het duurt 15 minuten om de wijzigingen in overeenstemming met na voltooiing van deze bewerking of [vernieuwen van de configuratieserver](vmware-azure-manage-configuration-server.md#refresh-configuration-server) voor onmiddellijk van kracht wordt.

## <a name="process-server-selection-guidance"></a>Richtlijnen voor Server-selectie verwerken

Azure Site Recovery wordt automatisch geïdentificeerd als processerver de gebruikslimieten nadert. Wanneer u voor het instellen van een uitbreidbare processerver worden richtlijnen gegeven.

|Status  |Uitleg  | Beschikbaarheid van resources  | Aanbeveling|
|---------|---------|---------|---------|
| In orde (groen)    |   Processerver is verbonden en in orde is      |Gebruik van de CPU en geheugen lager is dan 80%; Beschikbaarheid van de vrije ruimte is meer dan 30%| Deze processerver kan worden gebruikt om extra servers beveiligen. Zorg ervoor dat de nieuwe werkbelasting binnen de [gedefinieerd proces Serverlimieten](vmware-azure-set-up-process-server-scale.md#sizing-requirements).
|Waarschuwing (oranje):    |   Processerver is verbonden, maar bepaalde resources gaat maximale limieten bereiken  |   CPU-en geheugengebruik is tussen 80% - 95%; De beschikbaarheid van de vrije ruimte is tussen 25% tot 30%       | Het gebruik van de processerver is bijna drempelwaarden. Nieuwe servers toevoegen aan dezelfde processerver zal leiden tot een overschrijding van de drempelwaarden en kunnen invloed hebben op bestaande beveiligde items. Het wordt aanbevolen [instellen van een uitbreidbare processerver](vmware-azure-set-up-process-server-scale.md#before-you-start) voor nieuwe replicaties.
|Waarschuwing (oranje):   |   Processerver is verbonden, maar gegevens in de afgelopen 30 minuten niet is geüpload naar Azure  |   Resourcegebruik is binnen de grenzen van de drempelwaarde       | Problemen oplossen [fouten bij het uploaden van gegevens](vmware-azure-troubleshoot-replication.md#monitor-process-server-health-to-avoid-replication-issues) vóór het toevoegen van nieuwe werkbelastingen **of** [instellen van een uitbreidbare processerver](vmware-azure-set-up-process-server-scale.md#before-you-start) voor nieuwe replicaties.
|Kritiek (rood)    |     Processerver zou kunnen ontkoppeld  |  Resourcegebruik is binnen de grenzen van de drempelwaarde      | Problemen oplossen [verwerking van problemen met de verbinding](vmware-azure-troubleshoot-replication.md#monitor-process-server-health-to-avoid-replication-issues) of [instellen van een uitbreidbare processerver](vmware-azure-set-up-process-server-scale.md#before-you-start) voor nieuwe replicaties.
|Kritiek (rood)    |     Gebruik van resources is gepasseerd grenswaarden |  Gebruik van de CPU en geheugen hoger is dan 95%. Beschikbaarheid van de vrije ruimte is minder dan 25%.   | Toevoegen van nieuwe werkbelastingen aan dezelfde processerver is uitgeschakeld als resource drempelwaarde limieten zijn al bereikt. Dus [instellen van een uitbreidbare processerver](vmware-azure-set-up-process-server-scale.md#before-you-start) voor nieuwe replicaties.
Kritiek (rood)    |     Gegevens is niet van Azure naar Azure worden geüpload in de afgelopen 45 minuten. |  Resourcegebruik is binnen de grenzen van de drempelwaarde      | Problemen oplossen [fouten bij het uploaden van gegevens](vmware-azure-troubleshoot-replication.md#monitor-process-server-health-to-avoid-replication-issues) voordat u nieuwe werkbelastingen toevoegt aan dezelfde processerver of [instellen van een uitbreidbare processerver](vmware-azure-set-up-process-server-scale.md#before-you-start)

## <a name="reregister-a-process-server"></a>Een processerver registreren

Als u wilt registreren van een processerver on-premises uitgevoerd of in Azure, met de configuratieserver, doet het volgende:

[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]

Nadat u de instellingen hebt opgeslagen, het volgende doen:

1. Open een opdrachtprompt als administrator op de processerver.
2. Blader naar de map **%PROGRAMDATA%\ASR\Agent**, en voer de opdracht uit:

    ```
    cdpcli.exe --registermt
    net stop obengine
    net start obengine
    ```

## <a name="modify-proxy-settings-for-an-on-premises-process-server"></a>Proxy-instellingen voor een on-premises processerver wijzigen

Als de processerver een proxy verbinding maken met Site Recovery in Azure, gebruikt u deze procedure als u nodig hebt om bestaande proxyinstellingen te wijzigen.

1. Meld u aan bij de server-machine proces. 
2. Open een opdrachtvenster Admin PowerShell en voer de volgende opdracht uit:
   ```powershell
   $pwd = ConvertTo-SecureString -String MyProxyUserPassword
   Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber –ProxyUserName domain\username -ProxyPassword $pwd
   net stop obengine
   net start obengine
   ```
2. Blader naar de map **%PROGRAMDATA%\ASR\Agent**, en voer de volgende opdracht uit:
   ```
   cmd
   cdpcli.exe --registermt

   net stop obengine

   net start obengine

   exit
   ```

## <a name="remove-a-process-server"></a>Een processerver verwijderen

[!INCLUDE [site-recovery-vmware-unregister-process-server](../../includes/site-recovery-vmware-unregister-process-server.md)]

## <a name="manage-anti-virus-software-on-process-servers"></a>Antivirussoftware op de processervers beheren

Als de antivirussoftware op een zelfstandige processerver of de hoofddoelserver actief is, moet u de volgende mappen uitsluiten van antivirusprogramma's operations:


- C:\Program Files\Microsoft Azure Recovery Services Agent
- C:\ProgramData\ASR
- C:\ProgramData\ASRLogs
- C:\ProgramData\ASRSetupLogs
- C:\ProgramData\LogUploadServiceLogs
- C:\ProgramData\Microsoft Azure Site Recovery
- Proces installatiemap server, voorbeeld: C:\Program Files (x86) \Microsoft Azure Site Recovery