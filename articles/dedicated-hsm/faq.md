---
title: Veelgestelde vragen - Azure toegewezen HSM | Microsoft Docs
description: Veelgestelde vragen over die betrekking hebben op andere onderwerpen over Azure toegewezen HSM
services: dedicated-hsm
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.custom: mvc
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: concepts
ms.date: 12/11/2018
ms.author: barclayn
ms.openlocfilehash: ec814bf641dea6f47b907e9f9e2646dfcf84ade1
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54451745"
---
# <a name="frequently-asked-questions-faq"></a>Veelgestelde vragen

Vind antwoorden op veelgestelde vragen over Microsoft Azure toegewezen HSM.

## <a name="the-basics"></a>De basisbeginselen

### <a name="q-what-is-a-hardware-security-module-hsm"></a>VRAAG: Wat is er een hardware security module (HSM)?

Een Hardware Security Module (HSM) is een fysieke computer of fysiek apparaat gebruikt om te beveiligen en beheren van cryptografische sleutels. Sleutels die zijn opgeslagen in HSM's kunnen worden gebruikt voor cryptografische bewerkingen. Het sleutelmateriaal blijft veilig in vervalsen, verzegelde hardware-modules. De HSM kan alleen geverifieerde en gemachtigde toepassingen om de sleutels te gebruiken. De sleutels blijven altijd binnen het bereik van de HSM-beveiliging.

### <a name="q-what-is-the-azure-dedicated-hsm-offering"></a>VRAAG: Wat is de Azure toegewezen HSM aanbieding?

Azure toegewezen HSM is een cloud-gebaseerde service waarmee HSM's die worden gehost in Azure-datacenters die rechtstreeks zijn verbonden met het virtuele netwerk van een klant. Deze HSM's zijn toegewezen netwerkapparaten (de Gemalto SafeNet netwerk HSM 7 Model A790). Ze zijn geïmplementeerd rechtstreeks naar een klanten privé IP-adresruimte en Microsoft heeft geen toegang heeft tot de cryptografische functionaliteit van de HSM's. Alleen de klant heeft volledige administratieve en cryptografische controle over deze apparaten. Klanten zijn verantwoordelijk voor het beheer van het apparaat en ze volledig activiteitenlogboeken rechtstreeks vanaf hun apparaten kunnen krijgen. Toegewezen HSM's helpen klanten te voldoen aan naleving/wettelijke vereisten zoals FIPS 140-2 Level 3, HIPAA, PCI-DSS en eIDAS en nog veel meer.

### <a name="q-what-hardware-is-used-for-dedicated-hsm"></a>VRAAG: Welke hardware wordt gebruikt voor het toegewezen HSM?

Microsoft is een partnerschap aangegaan met Gemalto om de service Azure toegewezen HSM te leveren. Het apparaat dat wordt gebruikt is de [SafeNet Luna netwerk HSM 7 Model A790](https://safenet.gemalto.com/data-encryption/hardware-security-modules-hsms/safenet-network-hsm/). Dit apparaat wordt niet alleen biedt FIPS 140-2 Level 3 gevalideerd firmware, maar er wordt een biedt ook met lage latentie, hoge prestaties en hoge capaciteit via 10 partities. 

### <a name="q-what-is-an-hsm-used-for"></a>VRAAG: Waarvoor wordt een HSM gebruikt?

HSM's worden gebruikt voor het opslaan van versleutelingssleutels die worden gebruikt voor versleutelingsfunctionaliteit, zoals SSL (Secure Socket Layer), het versleutelen van gegevens, PKI (Public Key Infrastructure), DRM (Digital Rights Management) en het ondertekenen van documenten.

### <a name="q-how-does-dedicated-hsm-work"></a>VRAAG: Hoe werkt toegewezen HSM?

Klanten kunnen inrichten HSM's in specifieke regio's met PowerShell of opdrachtregelinterface. De klant geeft aan welke virtuele netwerk de HSM's worden verbonden met en één keer ingericht de HSM's zijn beschikbaar in het opgegeven subnet op toegewezen IP-adressen in de persoonlijke IP-adresruimte van de klant. Vervolgens klanten verbinding kunnen maken met de HSM's voor het beheer van het apparaat met behulp van SSH en beheer, instellen van de HSM-clientverbindingen, initialiseren van HSM's, partities maken, definiëren en rollen, zoals partitie officer, crypto officer en crypto-gebruiker toe te wijzen. Vervolgens wordt de klant Gemalto opgegeven HSM-hulpprogramma's / SDK/clientsoftware gebruiken om uit te voeren cryptografische bewerkingen via hun toepassingen.

### <a name="q-what-software-is-provided-with-the-dedicated-hsm-service"></a>VRAAG: Welke software wordt geleverd met de toegewezen HSM-service?

Gemalto geeft alle software voor de HSM-apparaat eenmaal is ingericht door Microsoft. De software is beschikbaar op de [Gemalto customer support portal](https://supportportal.gemalto.com/csm/). Klanten die gebruikmaken van de toegewezen HSM-service zijn vereist om te worden geregistreerd voor Gemalto ondersteunen en een klant-ID waarmee de toegang en downloaden van de relevante software hebben. De ondersteunde client-software is versie 7.2 die compatibel met de FIPS 140-2 Level 3 gevalideerd firmwareversie 7.0.3 is. 

### <a name="q-does-azure-dedicated-hsm-offer-password-based-and-ped-based-authentication"></a>VRAAG: Biedt Azure toegewezen HSM verificatie op basis van wachtwoorden en op basis van een PED?

Op dit moment biedt Azure toegewezen HSM alleen HSM's met verificatie op basis van wachtwoorden.

### <a name="q-will-azure-dedicated-hsm-host-my-hsms-for-me"></a>VRAAG: Azure toegewezen HSM host Mijn HSM's voor mij?

Microsoft biedt de Gemalto SafeNet Luna netwerk-HSM via de toegewezen HSM-service alleen en kan geen host zijn geen apparaten klant worden geleverd.

### <a name="q-does-azure-dedicated-hsm-support-pinetf-features-or-meet-pci-hsm-v1-or-v2-certification-requirements"></a>VRAAG: Ondersteunt Azure toegewezen HSM PINCODE/ETF functies of voldoen aan de vereisten van PCI HSM v1 of v2-certificering?

De service Azure toegewezen HSM gebruikt SafeNet Luna netwerk HSM 7 (model A790)-apparaten. Deze apparaten bieden geen ondersteuning voor betaling HSM specifieke functionaliteit (zoals een PINCODE of ETF) of certificaten. Als u Azure toegewezen HSM-service voor de ondersteuning van betaling HSM's in de toekomst, geef u hieronder feedback geven.

## <a name="interoperability"></a>Interoperabiliteit

### <a name="q-how-does-my-application-connect-to-a-dedicated-hsm"></a>VRAAG: Hoe mijn toepassing verbinding met een HSM toegewezen?

U Gemalto opgegeven client-hulpprogramma's / SDK/software van de HSM om uit te voeren cryptografische bewerkingen van uw toepassingen. De software is beschikbaar op de [Gemalto customer support portal](https://supportportal.gemalto.com/csm/). Klanten die gebruikmaken van de toegewezen HSM-service zijn vereist om te worden geregistreerd voor Gemalto ondersteunen en een klant-ID waarmee de toegang en downloaden van de relevante software hebben.

### <a name="q-can-an-application-connect-to-dedicated-hsm-from-a-different-vnet-in-or-across-regions"></a>VRAAG: Kan een toepassing verbinden aan toegewezen HSM vanuit een ander VNET in of meerdere regio's?

Ja, u moet gebruiken [VNET-peering](../virtual-network/virtual-network-peering-overview.md) binnen een regio om verbinding te maken tussen virtuele netwerken. Voor regio-overschrijdende verbindingen, moet u [VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).

### <a name="q-can-i-synchronize-dedicated-hsm-with-on-premises-hsms"></a>VRAAG: Kan ik toegewezen HSM synchroniseren met on-premises HSM's?

Ja, kunt u on-premises HSM's met toegewezen HSM synchroniseren. [Point-to-Point VPN of punt-naar-site](../vpn-gateway/vpn-gateway-about-vpngateways.md) verbinding kan worden gebruikt om verbinding met uw on-premises netwerk te maken.

### <a name="q-can-i-encrypt-data-used-by-other-azure-services-using-keys-stored-in-dedicated-hsm"></a>VRAAG: Kan ik gegevens die worden gebruikt door andere Azure-services met behulp van sleutels die zijn opgeslagen in HSM toegewezen coderen?

Nee. Azure toegewezen HSM's zijn alleen toegankelijk binnen het virtuele netwerk.

### <a name="q-can-i-import-keys-from-an-existing-on-premises-hsm-to-dedicated-hsm"></a>VRAAG: Kan ik sleutels importeren uit een bestaande On-premises HSM naar toegewezen HSM?

Ja, als u on-premises Gemalto SafeNet HSM's. Er zijn meerdere methoden. Raadpleeg de documentatie Gemalto HSM.

### <a name="q-what-operating-systems-are-supported-by-dedicated-hsm-client-software"></a>VRAAG: Welke besturingssystemen worden ondersteund door de clientsoftware van toegewezen HSM?

* Windows, Linux, Solaris, AIX, HP-UX, FreeBSD
* Virtual: VMware, hyperv, Xen, KVM

### <a name="q-how-do-i-configure-my-client-application-to-create-a-high-availability-configuration-with-multiple-partitions-from-multiple-hsms"></a>VRAAG: Hoe configureer ik mijn-clienttoepassing voor het maken van een maximaal beschikbaarheidsconfiguratie met meerdere partities uit meerdere HSM's?

Als u wilt een hoge beschikbaarheid hebben, moet u uw HSM-clientconfiguratie toepassing ingesteld voor het gebruik van de partities van de HSM. Raadpleeg de documentatie van Gemalto HSM client software.

### <a name="q-what-authentication-mechanisms-are-supported-by-dedicated-hsm"></a>VRAAG: Welke verificatiemechanismen worden ondersteund door toegewezen HSM?

Azure toegewezen HSM gebruikt SafeNet netwerk-HSM-7-apparaten (model A790) en ondersteuning van verificatie op basis van wachtwoorden.

### <a name="q-what-sdks-apis-client-software-is-available-to-use-with-dedicated-hsm"></a>VRAAG: Welke software van de client SDK's, API's, is beschikbaar voor gebruik met toegewezen HSM?

PKCS #11, Java (JCA/JCE), Microsoft CAPI en CNG, OpenSSL

### <a name="q-can-i-importmigrate-keys-from-luna-56-hsms-to-azure-dedicated-hsms"></a>VRAAG: Kan ik importeren/migreren sleutels uit Luna 5/6 HSM's naar Azure toegewezen HSM's?

Ja. Raadpleeg de Migratiehandleiding Gemalto. 

## <a name="using-your-hsm"></a>Met behulp van uw HSM

### <a name="q-how-do-i-decide-whether-to-use-azure-key-vault-or-azure-dedicated-hsm"></a>VRAAG: Hoe bepaal ik of Azure Key Vault of Azure toegewezen HSM wilt gebruiken?

Azure toegewezen HSM is de juiste keuze voor ondernemingen migreert naar Azure on-premises toepassingen die gebruikmaken van HSM's. Toegewezen HSM's bieden de mogelijkheid voor het migreren van een toepassing met minimale wijzigingen. Als in de toepassingscode die wordt uitgevoerd in een virtuele machine van Azure of een Web-App worden cryptografische bewerkingen uitgevoerd, kunnen ze toegewezen HSM gebruiken. In het algemeen krimp software die wordt uitgevoerd in de modellen van IaaS (infrastructuur als een service), die ondersteuning bieden voor HSM's zoals een sleutelarchief kunt reserveren HSM, zoals Application gateway of traffic manager voor een SSL, AD CS (Active Directory Certificate Services), of vergelijkbare PKI-hulpprogramma's, hulpprogramma's / toepassingen die worden gebruikt voor het ondertekenen van documenten, ondertekening van programmacode of een SQL Server (IaaS) is geconfigureerd met TDE (transparent databaseversleuteling) met de hoofdsleutel in een HSM met behulp van een EKM-provider (extensible key management). Azure Key Vault is geschikt voor 'geboren-in-cloud'-toepassingen of voor versleuteling-at-rest-scenario's waar klantgegevens worden verwerkt door PaaS (platform als een service) of SaaS (Software als een service)-scenario's zoals de sleutel in de Office 365-klant, Azure Information Protection , Azure Disk Encryption, Azure Data Lake Store-versleuteling met door de klant beheerde sleutel, Azure Storage met de klant beheerde sleutel, en met SQL Azure met de klant beheerde sleutel.

### <a name="q-what-usage-scenarios-best-suit-azure-dedicated-hsm"></a>VRAAG: Welke gebruiksscenario's voor aanbevolen Azure toegewezen HSM aanpassen?

Azure toegewezen HSM is het meest geschikt voor scenario's voor migratie. Dit betekent dat als u on-premises toepassingen naar Azure die al van HSM's gebruikmaken migreert. Dit biedt een lage wrijving optie om te migreren naar Azure met minimale wijzigingen in de toepassing. Als in de toepassingscode die wordt uitgevoerd in Azure VM of Web-App worden cryptografische bewerkingen uitgevoerd, kunnen toegewezen HSM kan worden gebruikt. In het algemeen krimp software die wordt uitgevoerd in de modellen van IaaS (infrastructuur als een service), die ondersteuning bieden voor HSM's zoals een sleutelarchief een speciale HSM, zoals gebruiken kan:

* Application gateway of traffic manager voor een SSL
* AD CS (Active Directory certificaatservices)
* Vergelijkbare PKI-hulpprogramma 's
* Hulpprogramma's / toepassingen die worden gebruikt voor het ondertekenen van documenten
* Ondertekening van programmacode
* SQL Server (IaaS) is geconfigureerd met TDE (transparent databaseversleuteling) met de hoofdsleutel in een HSM met behulp van een EKM-provider (extensible key management)

### <a name="q-can-dedicated-hsm-be-used-with-office-365-customer-key-azure-information-protection-azure-data-lake-store-disk-encryption-azure-storage-encryption-azure-sql-tde"></a>VRAAG: Kan toegewezen HSM worden gebruikt met Office 365 Customer Key, Azure Information Protection, Azure Data Lake Store, Azure Disk Encryption, Azure Storage Service Encryption, Azure SQL TDE?

Nee. Toegewezen HSM wordt ingericht rechtstreeks in privé-IP-adresruimte van een klant, zodat deze niet toegankelijk is voor andere Azure- of Microsoft-services.

## <a name="administration-access-and-control"></a>Beheer, toegang tot en beheer

### <a name="q-does-the-customer-get-full-exclusive-control-over-the-hsms-with-dedicated-hsms"></a>VRAAG: Volledige exclusieve controle over de HSM's met toegewezen HSM's krijgen door de klant?

Ja. Elke HSM-apparaat is volledig toegewezen voor een afzonderlijke klant en niemand anders heeft beheer eenmaal ingericht en de administrator-wachtwoord gewijzigd.

### <a name="q-what-level-of-access-does-microsoft-have-to-my-hsm"></a>VRAAG: Welk niveau van toegang beschikt Microsoft over naar Mijn HSM?

Microsoft heeft geen eventuele administratieve of cryptografische controle over de HSM. Microsoft heeft toegang via een seriële poortverbinding om op te halen van basic telemetriegegevens zoals temperatuur en onderdeel health monitor. Hierdoor kan Microsoft voor proactieve meldingen van statusproblemen. Indien nodig, kan de klant dit account uitschakelen.

### <a name="q-can-microsoft-or-anyone-at-microsoft-access-keys-in-my-dedicated-hsm"></a>VRAAG: Kan Microsoft of iemand bij Microsoft access-toetsen in Mijn toegewezen HSM?

Nee. Microsoft heeft geen toegang heeft tot de sleutels die zijn opgeslagen in klant toegewezen HSM toegewezen.

### <a name="q-can-i-upgrade-softwarefirmware-on-hsms-allocated-to-me"></a>VRAAG: Kan ik software/firmware op HSM's die aan mij toegewezen upgraden?

Als u de beste ondersteuning, wordt aangeraden niet bij te werken software/firmware op de HSM. De klant heeft echter volledig beheren, inclusief het upgraden van software/firmware als specifieke functies nodig zijn uit verschillende firmware-versies zijn. Voordat u wijzigingen aanbrengt, de implicaties moeten worden geïnterpreteerd als dit kan, bijvoorbeeld effect FIPS status gevalideerd. 

### <a name="q-how-do-i-manage-dedicated-hsm"></a>VRAAG: Hoe beheer ik toegewezen HSM?

U kunt de toegewezen HSM's beheren door deze met behulp van SSH.

### <a name="q-how-do-i-manage-partitions-on-the-dedicated-hsm"></a>VRAAG: Hoe beheer ik partities op de HSM toegewezen?

De clientsoftware Gemalto HSM wordt gebruikt voor het beheren van de HSM's en partities.

### <a name="q-how-do-i-monitor-my-hsm"></a>VRAAG: Hoe bewaak ik mijn HSM?

Een klant heeft volledige toegang tot HSM-activiteitenlogboeken via syslog- en SNMP. Een klant moet voor het instellen van een syslog-server of een SNMP-server de logboeken of gebeurtenissen ontvangen van de HSM's.

### <a name="q-can-i-get-full-access-log-of-all-hsm-operations-from-dedicated-hsm"></a>VRAAG: Kan ik volledige toegang tot logboek van alle HSM-bewerkingen van toegewezen HSM krijgen?

Ja. U kunt Logboeken van de HSM-apparaat verzenden naar een syslog-server

## <a name="high-availability"></a>Hoge beschikbaarheid

### <a name="q-is-it-possible-to-configure-high-availability-in-the-same-region-or-across-multiple-regions"></a>VRAAG: Is het mogelijk voor het configureren van hoge beschikbaarheid in dezelfde regio of in meerdere regio's?

Ja. Hoge beschikbaarheid, configuratie en installatie uitgevoerd binnen de HSM-clientsoftware die is geleverd door Gemalto. HSM's in hetzelfde VNET of andere VNETs in dezelfde regio of meerdere regio's of on-premises HSM's zijn verbonden met een VNET maken via site-naar-site of point-to-point VPN kan worden toegevoegd aan dezelfde configuratie voor hoge beschikbaarheid.

### <a name="can-i-add-hsms-from-my-on-premises-network-to-a-high-availability-group-with-azure-dedicated-hsm"></a>Kan ik HSM's van mijn on-premises netwerk aan een groep met hoge beschikbaarheid met Azure toegewezen HSM toevoegen?

Ja. Ze moeten voldoen aan de vereisten voor hoge beschikbaarheid voor 7 SafeNet Luna netwerk-HSM.

### <a name="can-i-add-luna-56-hsms-from-on-premises-networks-to-a-high-availability-group-with-azure-dedicated-hsm"></a>Kan ik Luna 5/6 HSM's van on-premises netwerken toevoegen aan een groep met hoge beschikbaarheid met Azure toegewezen HSM?

Nee.

### <a name="q-how-many-hsms-can-i-add-to-the-same-high-availability-configuration-from-one-single-application"></a>VRAAG: Hoeveel HSM's kan ik toevoegen aan de dezelfde configuratie voor hoge beschikbaarheid van één enkele toepassing?

16.

## <a name="support"></a>Ondersteuning

### <a name="q-what-is-the-sla-for-dedicated-hsm-service"></a>VRAAG: Wat is de SLA voor toegewezen HSM-service?

Op dit moment is er geen SLA aangeboden voor toegewezen HSM-service. Microsoft zorgt ervoor dat niveau netwerktoegang tot het apparaat en kan daarom standard Azure networking Sla's van toepassing.

### <a name="q-how-are-the-hsms-used-in-azure-dedicated-hsm-protected"></a>VRAAG: Hoe worden de HSM's in Azure toegewezen HSM-beveiligde gebruikt?

Azure-datacenters hebben besturingselementen uitgebreide beveiliging voor fysieke en procedures. Daarnaast worden toegewezen HSM's gehost in een gebied verder met beperkte toegang in het datacenter. Deze gebieden zijn extra fysieke toegangscontrole en videocamera bewaking voor extra beveiliging.

### <a name="q-what-happens-if-there-is-a-security-breach-or-hardware-tampering-event"></a>VRAAG: Wat gebeurt er als er een schending van de beveiliging of manipulatie van gebeurtenis hardware?

Toegewezen HSM-service maakt gebruik van SafeNet netwerk-HSM-7-apparaten. Deze apparaten ondersteuning voor detectie van fysieke en logische onrechtmatige wijzigingen. Als er een gebeurtenis onrechtmatige wijzigingen is worden automatisch de HSM's zeroized.

### <a name="q-how-do-i-ensure-that-keys-in-my-dedicated-hsms-are-not-lost-due-to-error-or-a-malicious-insider-attack"></a>VRAAG: Hoe kan ik ervoor zorgen dat er geen sleutels in Mijn toegewezen HSM verloren vanwege een fout of een kwaadwillende insider-aanval?

Het is raadzaam een back-up on-premises HSM-apparaat gebruiken om uit te voeren van reguliere periodieke back-up van de HSM's voor herstel na noodgevallen. U moet een peer-to-peer of site-naar-site VPN-verbinding naar een on-premises werkstation dat is verbonden met een back-HSM-apparaat gebruiken.

### <a name="q-how-do-i-get-support-for-dedicated-hsm"></a>VRAAG: Hoe krijg ik ondersteuning voor toegewezen HSM?

Dezelfde manier als u ondersteuning krijgen voor alle andere Azure-services. Het ondersteuningsteam van Azure wordt doorgestuurd naar het ondersteuningsteam van Gemalto als nodig is afhankelijk van de aanvraag.

### <a name="q-how-do-i-get-access-to-dedicated-hsm-client-software-documentation-firmware-images"></a>VRAAG: Hoe krijg ik toegang tot toegewezen HSM-clientsoftware, documentatie, firmware-installatiekopieën

Klanten moet rechtstreeks met Gemalto toegang te krijgen tot de HSM-clientsoftware, documentatie, OS/firmware-afbeeldingen werken.

### <a name="q-if-there-is-a-security-vulnerability-found-and-a-patch-is-released-by-gemalto-who-is-responsible-for-upgradingpatching-osfirmware"></a>VRAAG: Als er een beveiligingsprobleem gevonden en een patch is uitgegeven door Gemalto, die verantwoordelijk is voor upgraden/patchen OS/Firmware?

Microsoft heeft niet de mogelijkheid om te verbinden met HSM's die zijn toegewezen aan klanten. Klanten moeten upgraden en vullen van de HSM's.

## <a name="cryptography-and-standards"></a>Cryptografie en standaarden

### <a name="q-is-it-safe-to-store-encryption-keys-for-my-most-important-data-in-dedicated-hsm"></a>VRAAG: Is het veilig voor het opslaan van versleutelingssleutels voor de belangrijkste gegevens in toegewezen HSM?

Ja, bepalingen toegewezen HSM SafeNet netwerk-HSM-7-apparaten die gebruik FIPS 140-2 Level 3 HSM's gevalideerde. 

### <a name="q-what-cryptographic-keys-and-algorithms-are-supported-by-dedicated-hsm"></a>VRAAG: Welke cryptografische sleutels en de algoritmen worden ondersteund door toegewezen HSM?

Toegewezen HSM-service bepalingen SafeNet netwerk-HSM-7-apparaten. Ze bieden ondersteuning voor een breed scala aan cryptografische sleuteltypen en algoritmen, met inbegrip van: Volledige Suite B-ondersteuning

* Asymmetrische:
  * RSA
  * DSA
  * Diffie-Hellman-
  * Elliptic Curve
  * Cryptografie (ECDSA, ECDH, Ed25519, ECIES) met naam, de gebruiker gedefinieerde en Brainpool curven, KCDSA
* Symmetrische:
  * AES-GCM
  * Drievoudig DES
  * DES
  * ARIA, SEED
  * RC2
  * RC4
  * RC5
  * CAST
  * Digest/HMAC hash/bericht: SHA-1, SHA-2, SM3
  * Afgeleide sleutel: SP800 108 prestatiemeteritem modus
  * Key Wrapping: SP800-38F
  * Genereren van willekeurige getallen: FIPS 140-2 goedgekeurd DRBG (SP 800-90 CTR modus), die voldoet aan Institution DRG.4

### <a name="q-is-dedicated-hsm-fips-140-2-level-3-validated"></a>VRAAG: Is toegewezen HSM FIPS 140-2 Level 3 is gevalideerd?

Ja. Toegewezen HSM-service bepalingen SafeNet netwerk-HSM-7-apparaten die gebruikmaken van FIPS 140-2 Level 3 gevalideerde HSM's.

### <a name="q-what-do-i-need-to-do-to-make-sure-i-operate-dedicated-hsm-in-fips-140-2-level-3-validated-mode"></a>VRAAG: Wat moet ik doen om ervoor te zorgen dat ik werken toegewezen HSM in FIPS 140-2 Level 3 modus gevalideerd?

De toegewezen HSM-service inricht SafeNet Luna netwerk-HSM 7-apparaten. Deze apparaten gebruik FIPS 140-2 Level 3 gevalideerde HSM's. De standaardconfiguratie geïmplementeerd, het besturingssysteem en de firmware zijn ook FIPS-gevalideerde modules. U hoeft niet te doen voor FIPS 140-2 Level 3-compatibiliteit.

### <a name="how-does-a-customer-ensure-that-when-an-hsm-is-deprovisioned-all-the-key-material-is-wiped-out"></a>Hoe garandeert een klant dat het sleutelmateriaal uit gewist wanneer de inrichting van een HSM is beëindigd?

Voordat u aanvraagt ongedaan maken van inrichting, moet de HSM Gemalto opgegeven HSM clienthulpprogramma's gebruiken op een klant hebt zeroized.

## <a name="performance-and-scale"></a>Prestaties en schaal

### <a name="q-how-many-cryptographic-operations-are-supported-per-second-with-dedicated-hsm"></a>VRAAG: Hoeveel cryptografische bewerkingen per seconde met toegewezen HSM worden ondersteund?

Toegewezen HSM bepalingen SafeNet netwerk-HSM-7-apparaten (model A790). Hier volgt een samenvatting van de maximale prestaties voor bepaalde bewerkingen: 

* RSA-2048: 10.000 transacties per seconde
* ECC P256: 20.000 transacties per seconde
* AES-GCM: 17.000 transacties per seconde

### <a name="q-how-many-partitions-can-be-created-in-dedicated-hsm"></a>VRAAG: Het aantal partities kunnen worden gemaakt in de toegewezen HSM?

Op basis van het specifieke model van de HSM gebruikt, wordt er 10 partities die beschikbaar zijn.

### <a name="q-how-many-keys-can-be-supported-in-dedicated-hsm"></a>VRAAG: Het aantal sleutels kunnen worden ondersteund in toegewezen HSM?

Maximum aantal sleutels. Deze getallen zijn ook van toepassing op paren van sleutels als asymmetrische sleutels.

* RSA-2048 - 19.000
* ECC-P256 - 91,000
* AES-256 - 218,000

Capaciteit varieert, afhankelijk van bepaalde belangrijke kenmerken instellen in de sjabloon voor genereren van sleutels en het aantal partities.

