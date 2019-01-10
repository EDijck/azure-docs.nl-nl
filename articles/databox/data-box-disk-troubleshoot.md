---
title: Problemen in Azure Data Box Disk oplossen | Microsoft Docs
description: Hierin wordt beschreven hoe u problemen in Azure Data Box Disk oplost.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: article
ms.date: 01/09/2019
ms.author: alkohli
ms.openlocfilehash: 83b3a271006df38744b9de49ed6350bea3aeef4d
ms.sourcegitcommit: 33091f0ecf6d79d434fa90e76d11af48fd7ed16d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2019
ms.locfileid: "54159380"
---
# <a name="troubleshoot-issues-in-azure-data-box-disk"></a>Oplossen van problemen in Azure Data Box-schijf

Dit artikel is van toepassing op Microsoft Azure Data Box-schijf en een beschrijving van de werkstromen die wordt gebruikt voor het oplossen van problemen die u ziet wanneer u deze oplossing implementeert. 

In dit artikel bevat de volgende secties:

- Logboeken met diagnostische gegevens downloaden
- Logboeken met queryactiviteit
- Fouten met het ontgrendelingsprogramma voor Data Box Disk
- Fouten met het splits- en kopieerprogramma voor Data Box Disk

## <a name="download-diagnostic-logs"></a>Logboeken met diagnostische gegevens downloaden

Als er fouten optreden tijdens het kopiëren van gegevens, geeft de portal een pad weer naar de map waarin de logboeken met diagnostische gegevens zich bevinden. 

De soorten diagnostische logboeken van Azure zijn:
- Foutenlogboeken
- Uitgebreide logboeken  

Als u naar het pad met een kopie van het logboek wilt navigeren, gaat u naar het dat is gekoppeld aan uw Data Box-order. 

1.  Ga naar **Algemeen > Ordergegevens** en noteer het opslagaccount dat is gekoppeld aan uw order.
 

2.  Ga naar **Alle resources** en zoek naar het opslagaccount dat in de vorige stap is aangegeven. Selecteer en klik op het opslagaccount.

    ![Logboeken kopiëren 1](./media/data-box-disk-troubleshoot/data-box-disk-copy-logs1.png)

3.  Ga naar **Blob-service > Blobs verkennen** en zoek naar de blob die overeenkomt met het opslagaccount. Ga naar **diagnosticslogcontainer > waies**. 

    ![Logboeken kopiëren 2](./media/data-box-disk-troubleshoot/data-box-disk-copy-logs2.png)

    U ziet zowel de foutenlogboeken als de uitgebreide logboeken voor het kopiëren van gegevens. Selecteer en klik op elk bestand en download vervolgens een lokale kopie.

## <a name="query-activity-logs"></a>Logboeken met queryactiviteit

Gebruik de activiteitenlogboeken om fouten te vinden bij foutoplossing of om te controleren hoe een gebruiker in uw organisatie een resource heeft gewijzigd. Met activiteitenlogboeken kunt u het volgende bepalen:

- Welke bewerkingen zijn uitgevoerd voor de resources in uw abonnement.
- Wie de bewerking heeft gestart. 
- Wanneer de bewerking is uitgevoerd.
- De status van de bewerking.
- De waarden van andere eigenschappen die u kunnen helpen bij het onderzoeken van het probleem.

In het activiteitenlogboek staan alle schrijfbewerkingen (zoals PUT, POST, DELETE) die op uw resources zijn uitgevoerd, maar er staan geen leesbewerkingen (zoals GET) in. 

Activiteitenlogboeken worden gedurende 90 dagen bewaard. U kunt een query uitvoeren voor een willekeurig bereik van datums, zolang de begindatum niet verder dan 90 dagen in het verleden is. U kunt ook filteren op een van de ingebouwde query's in Insights. Klik bijvoorbeeld op een fout en selecteer vervolgens specifieke problemen om de hoofdoorzaak te begrijpen.

## <a name="data-box-disk-unlock-tool-errors"></a>Fouten met het ontgrendelingsprogramma voor Data Box Disk


| Fout bericht/gedrag hulpprogramma      | Aanbevelingen                                                                                               |
|-------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------|
| Geen<br><br>Het ontgrendelingsprogramma voor Data Box Disk loopt vast.                                                                            | BitLocker is niet geïnstalleerd. Zorg ervoor dat het hulpprogramma BitLocker is geïnstalleerd op de computer waarop het ontgrendelingsprogramma voor Data Box Disk wordt uitgevoerd.                                                                            |
| Het huidige .NET Framework wordt niet ondersteund. De ondersteunde versies zijn 4.5 of hoger.<br><br>Hulpprogramma wordt beëindigd met een bericht.  | .NET 4.5 is niet geïnstalleerd. Installeer .NET 4.5 of hoger op de hostcomputer waarop het ontgrendelingsprogramma voor Data Box Disk wordt uitgevoerd.                                                                            |
| Kan geen volumes ontgrendelen of verifiëren. Neem contact op met Microsoft Ondersteuning.  <br><br>Vergrendelde stations kunnen niet worden ontgrendeld of geverifieerd door het hulpprogramma. | De vergrendelde stations kunnen niet worden ontgrendeld door het hulpprogramma met de verstrekte wachtwoordsleutel. Neem contact op met Microsoft Ondersteuning voor volgende stappen.                                                |
| De volgende volumes zijn ontgrendeld en geverifieerd. <br>Volume stationsletters: E:<br>Er konden geen volumes worden ontgrendeld met de volgende wachtwoordsleutels: werwerqomnf, qwerwerqwdfda <br><br>Door het hulpprogramma worden sommige stations ontgrendeld en worden de letters van stations waarbij het is mislukt weergegeven.| Gedeeltelijk geslaagd. Sommige van de stations kunnen niet worden ontgrendeld met de verstrekte wachtwoordsleutel. Neem contact op met Microsoft Ondersteuning voor volgende stappen. |
| De vergrendelde volumes zijn niet gevonden. Controleer of de schijf die is ontvangen van Microsoft op de juiste manier is aangesloten en is vergrendeld.          | Er zijn geen vergrendelde stations gevonden door het hulpprogramma. De stations zijn al ontgrendeld of niet gedetecteerd. Zorg ervoor dat de stations zijn verbonden en zijn vergrendeld.                                                           |
| Onherstelbare fout: Ongeldige parameter<br>Parameternaam: invalid_arg<br>GEBRUIK:<br>DataBoxDiskUnlock /PassKeys:<passkey_list_separated_by_semicolon><br><br>Voorbeeld: DataBoxDiskUnlock /PassKeys:passkey1; passkey2; passkey3<br>Voorbeeld: DataBoxDiskUnlock /SystemCheck<br>Voorbeeld: DataBoxDiskUnlock/Help<br><br>/ Sleutels:       Deze sleutel ophalen uit Azure DataBox schijforder. De wachtwoordsleutel ontgrendelt uw schijven.<br>/ Help:           Deze optie biedt Help-informatie voor cmdlet-syntaxis en voorbeelden.<br>/ SystemCheck:    Deze optie wordt gecontroleerd of uw systeem voldoet aan de vereisten voor het uitvoeren van het hulpprogramma.<br><br>Druk op een willekeurige toets om af te sluiten. | Ongeldige parameter ingevoerd. De enige toegestane parameters zijn /SystemCheck, /PassKey en /Help.                                                                            |

## <a name="data-box-disk-split-copy-tool-errors"></a>Fouten met het splits- en kopieerprogramma voor Data Box Disk

|Foutbericht/waarschuwingen  |Aanbevelingen |
|---------|---------|
|[Info] Bitlocker-wachtwoord ophalen voor volume: m <br>[Fout] Uitzondering opgetreden tijdens het ophalen van bitlocker-sleutel voor volume m:<br> Reeks bevat geen elementen.|Deze fout wordt gegenereerd als de doel-Data Box Disk offline is. <br> Gebruik hulpprogramma `diskmgmt.msc` voor online schijven.|
|[Fout] Uitzondering geretourneerd: WMI-bewerking is mislukt:<br> Method=UnlockWithNumericalPassword, ReturnValue=2150694965, <br>Win32Message=De indeling van het verstrekte herstelwachtwoord is ongeldig. <br>BitLocker-herstelwachtwoorden bestaan uit 48 cijfers. <br>Controleer of het herstelwachtwoord de juiste indeling heeft en probeer het opnieuw.|Gebruik eerst het ontgrendelingsprogramma van Data Box Disk om de schijven te ontgrendelen en voer dan de opdracht opnieuw uit. Voor meer informatie gaat u naar <li> [Data Box Disk ontgrendelen voor Windows-clients](data-box-disk-deploy-set-up.md#unlock-disks-on-windows-client). </li><li> [Data Box Disk ontgrendelen voor Linux-clients](data-box-disk-deploy-set-up.md#unlock-disks-on-linux-client). </li>|
|[Fout] Uitzondering geretourneerd: Er bestaat een DriveManifest.xml-bestand op de doel-station. <br> Dit betekent dat het doelstation mogelijk is voorbereid met een ander logboekbestand. <br>Als u meer gegevens aan hetzelfde station wilt toevoegen, gebruik dan het vorige logboekbestand. Als u bestaande gegevens wilt verwijderen en het doelstation wilt hergebruiken voor een nieuwe importtaak, verwijdert u het bestand DriveManifest.xml van de schijf. Voer deze opdracht opnieuw uit met een nieuw logboekbestand.| Deze fout wordt gegenereerd wanneer u dezelfde reeks stations probeert te gebruiken voor meerdere importsessies. <br> Gebruik één set schijven maar voor één splits- en kopieersessie.|
|[Fout] Uitzondering geretourneerd: CopySessionId importgegevens-september-test-1 verwijst naar een eerdere sessie. kopiëren en niet opnieuw kan worden gebruikt voor een nieuwe kopieersessie.|Deze fout wordt gerapporteerd wanneer wordt geprobeerd om dezelfde taaknaam voor een nieuwe taak te gebruiken als een eerder met succes voltooide taak.<br> Geef een unieke naam op voor de nieuwe taak.|
|[Info] De naam van het doelbestand of de doelmap overschrijdt de lengtelimiet van NTFS. |Dit bericht wordt gegenereerd als de naam van het doelbestand is gewijzigd vanwege een lang pad.<br> Wijzig de verwijderingsoptie in bestand `config.json` om dit gedrag te regelen.|
|[Fout] Uitzondering geretourneerd: Ongeldige JSON escape-volgorde. |Dit bericht wordt gegenereerd als de config.json een ongeldige opmaak heeft. <br> Valideer uw `config.json` met behulp van [JSONlint](https://jsonlint.com/) voordat u het bestand opslaat.|



## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [Data Box Disk beheren via Azure Portal](data-box-portal-ui-admin.md).
