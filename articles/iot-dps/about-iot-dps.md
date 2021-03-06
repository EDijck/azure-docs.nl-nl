---
title: Overzicht van Azure IoT Hub Device Provisioning Service | Microsoft Docs
description: In dit artikel wordt beschreven hoe u in Azure apparaten inricht met Device Provisioning Service en IoT Hub
author: nberdy
ms.author: nberdy
ms.date: 12/05/2017
ms.topic: overview
ms.service: iot-dps
services: iot-dps
manager: briz
ms.openlocfilehash: 89e0b6b5bedf36c89bf3a54f4baf42f7019625c0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60746050"
---
# <a name="provisioning-devices-with-azure-iot-hub-device-provisioning-service"></a>Apparaten inrichten met Azure IoT Hub Device Provisioning Service
Microsoft Azure biedt een uitgebreide set geïntegreerde openbare cloudservices om tegemoet te komen aan al uw IoT-vragen. IoT Hub Device Provisioning Service is een Helper-service voor IoT Hub die Just-In-Time-inrichting naar de juiste IoT-hub mogelijk maakt zonder tussenkomst van de gebruiker, zodat klanten miljoenen apparaten op een veilige en schaalbare manier kunnen inrichten.

## <a name="when-to-use-device-provisioning-service"></a>Wanneer Device Provisioning Service gebruiken
Er zijn veel scenario's waarin Device Provisioning Service een uitstekende keuze is voor het verbinden en configureren van apparaten met IoT Hub, zoals:

* Apparaat zonder tussenkomst van gebruiker inrichten voor een afzonderlijke IoT-oplossing zonder dat hiervoor in de fabriek (eerste installatie) hardcoding van verbindingsgegevens voor IoT-Hub nodig is
* Taken van apparaten verdelen over meerdere hubs
* Apparaten verbinden met de IoT-oplossing van hun eigenaar op basis van verkooptransactiegegevens (multitenancy)
* Apparaten verbinden met een bepaalde IoT-oplossing, afhankelijk van het gebruiksscenario (isolatie van oplossing)
* Apparaat met de laagst mogelijke latentie (geo-sharding) verbinden met de IoT-hub
* Herinrichten op basis van een wijziging op het apparaat
* Aanpassen van de sleutels die door het apparaat worden gebruikt om verbinding te maken met IoT Hub (als er geen verbinding wordt gemaakt met behulp van X.509-certificaten)

## <a name="behind-the-scenes"></a>Achter de schermen
Alle scenario's die hierboven worden vermeld kunnen met dezelfde werkstroom en zonder tussenkomst van de gebruiker worden uitgevoerd met behulp van de inrichtingsservice. Veel van de handmatige stappen die gebruikelijk zijn bij het inrichten van apparaten zijn geautomatiseerd met de Device Provisioning Service om zo het implementeren van IoT-apparaten te versnellen en de kans op handmatige fouten te verkleinen. In de volgende sectie wordt beschreven wat er achter de schermen gebeurt om een apparaat in te richten. De eerste stap is handmatig, alle overige gebeuren automatisch.

![Basiswerkstroom voor inrichting van apparaat](./media/about-iot-dps/dps-provisioning-flow.png)

1. De apparaatfabrikant voegt de registratiegegevens van het apparaat toe aan de lijst met registraties in Azure Portal.
2. Het apparaat neemt contact op met het eindpunt van de inrichtingsservice dat in de fabriek is ingesteld. Het apparaat geeft de identificatiegegevens door aan de inrichtingsservice om de identiteit aan te tonen.
3. De inrichtingsservice valideert de identiteit van het apparaat door de registratie-id en -sleutel te vergelijken met de vermelding in de lijst met registraties. Dit gebeurt met een nonce-challenge ([Trusted Platform Module](https://trustedcomputinggroup.org/work-groups/trusted-platform-module/)) of standaard x.509-verificatie (X.509).
4. De inrichtingsservice registreert het apparaat bij een IoT-hub en vult de [gewenste dubbelstatus](../iot-hub/iot-hub-devguide-device-twins.md) van het apparaat in.
5. De IoT-hub retourneert de gegevens van de apparaat-id naar de inrichtingsservice.
6. De inrichtingsservice retourneert de verbindingsgegevens van de IoT-hub naar het apparaat. Het apparaat kan nu rechtstreeks gegevens gaan verzenden naar de IoT-hub.
7. Het apparaat maakt verbinding met IoT Hub.
8. Het apparaat krijgt de gewenste status van de apparaatdubbel in IoT Hub.

## <a name="provisioning-process"></a>Inrichtingsproces
Er zijn twee verschillende stappen in het implementatieproces van een apparaat waarbij de inrichtingsservice een rol speelt en die onafhankelijk kunnen worden uitgevoerd:

* De **productiestap** waarin het apparaat wordt gemaakt en voorbereid in de fabriek, en
* De **configuratiestap voor de cloud** waarin Device Provisioning Service wordt geconfigureerd voor automatisch inrichten.

Beide stappen sluiten naadloos aan op bestaande productie- en implementatieprocessen. Device Provisioning Service vereenvoudigt zelfs enkele implementatieprocessen die heel veel handmatig werk vereisen om verbindingsgegevens op het apparaat te krijgen.

### <a name="manufacturing-step"></a>Productiestap
Deze stap omvat alles wat te maken heeft met de productielijn. Voorbeelden van rollen die zijn betrokken bij deze stap zijn siliciumontwerper, siliciumfabrikant, integrator en/of de eindfabrikant van het apparaat. Deze stap gaat om het maken van de hardware zelf.

Device Provisioning Service introduceert geen nieuwe stap in het productieproces. De service wordt geïntegreerd in de bestaande stap waarmee de eerste versie van de software en (in het ideale geval) de HSM worden geïnstalleerd op het apparaat. In plaats van een apparaat-id te maken in deze stap, wordt het apparaat geprogrammeerd met de gegevens van de inrichtingsservice, waardoor het apparaat bij inschakeling de service kan aanroepen om de verbindingsgegevens/toewijzing van de IoT-oplossing op te vragen.

In deze stap verstrekt de fabrikant ook belangrijke identificatiegegevens aan de persoon die het apparaat implementeert/gebruikt (operator). Het verstrekken van die gegevens kan zo eenvoudig zijn als het bevestigen dat alle apparaten beschikken over een X.509-certificaat dat is gegenereerd op basis van een ondertekend certificaat dat is geleverd door de persoon die het apparaat implementeert/gebruikt, of zo ingewikkeld als het extraheren van het openbare deel van een TPM-goedkeuringssleutel van elk TPM-apparaat. Deze services worden vandaag de dag aangeboden door verschillende fabrikanten van silicium.

### <a name="cloud-setup-step"></a>Configuratiestap voor de cloud
Deze stap betreft het configureren van de cloud voor een juiste automatische inrichting. In het algemeen er zijn twee typen gebruikers betrokken bij deze stap: iemand die weet hoe apparaten in eerste instantie moeten worden geconfigureerd (een operator van apparaten) en iemand anders die weet hoe apparaten moeten worden verdeeld tussen de IoT-hubs (een operator van oplossingen).

De inrichting moet in eerste instantie eenmalig worden geconfigureerd, wat meestal wordt afgehandeld door de operator van de oplossing. Zodra de inrichtingsservice is geconfigureerd, hoeft deze niet meer te worden gewijzigd, tenzij het gebruiksscenario verandert.

Nadat de service is geconfigureerd voor automatische inrichting, moet de service worden voorbereid voor de registratie van apparaten. Deze stap wordt uitgevoerd door de operator van het apparaat, die weet wat de gewenste configuratie is van het apparaat of de apparaten en die ervoor moet zorgen dat de inrichtingsservice de identiteit van het apparaat goed kan bevestigen wanneer het apparaat op zoek gaat naar de juiste IoT-hub. De operator van het apparaat neemt de belangrijkste identificatiegegevens van de fabrikant en voegt deze toe aan de lijst met registraties. De lijst met registraties kan nog worden gewijzigd als er nieuwe items worden toegevoegd of bestaande vermeldingen worden bijgewerkt met de meest recente informatie over de apparaten.

## <a name="registration-and-provisioning"></a>Registratie en inrichting
*Inrichting* kan verschillende dingen betekenen, afhankelijk van de branche waarin de term wordt gebruikt. In de context van het inrichten van IoT-apparaten bij hun cloudoplossing bestaat het proces uit twee delen:

1. Het eerste deel omvat het tot stand brengen van de eerste verbinding tussen het apparaat en de IoT-oplossing door het apparaat te registreren.
2. Het tweede gedeelte bestaat uit het toepassen van de juiste configuratie op het apparaat op basis van de specifieke vereisten van de oplossing waarbij het apparaat is geregistreerd.

Als beide stappen zijn voltooid, kunnen we zeggen dat het apparaat volledig is ingericht. Sommige cloudservices bieden alleen de eerste stap van het inrichtingsproces, het registreren van apparaten bij het eindpunt van de IoT-oplossing, maar niet de eerste configuratie. Device Provisioning Service automatiseert beide stappen en biedt zo een naadloze ervaring voor de inrichting van het apparaat.

## <a name="features-of-the-device-provisioning-service"></a>Functies van Device Provisioning Service
Device Provisioning Service heeft allerlei functies, waardoor de service ideaal is voor het inrichten van apparaten.

* Ondersteuning voor **beveiligde attestation** voor identiteiten op basis van zowel X.509 als TPM.
* **Lijst van registraties** met een volledig overzicht van de apparaten/groepen apparaten die op enige moment kunnen worden geregistreerd. De registratielijst bevat informatie over de gewenste configuratie van het apparaat als dit wordt geregistreerd en de lijst kan op elk gewenst moment worden bijgewerkt.
* **Meerdere beleidsregels voor toewijzing** om te bepalen hoe apparaten door Device Provisioning Service worden toegewezen aan IoT-hubs ter ondersteuning van uw scenario's.
* **Controle en logboekregistratie van diagnostische gegevens** om ervoor te zorgen dat alles goed werkt.
* **Ondersteuning voor meerdere hubs** zorgt ervoor dat Device Provisioning Service apparaten kan toewijzen aan meer dan één IoT-hub. Device Provisioning Service kan communiceren met hubs binnen verschillende Azure-abonnementen.
* **Ondersteuning voor meerdere regio's** zorgt ervoor dat Device Provisioning Service apparaten kan toewijzen aan IoT-hubs in andere regio's.

Zie [apparaatconcepten](concepts-device.md), [serviceconcepten](concepts-service.md) en [veiligheidsconcepten](concepts-security.md) voor meer informatie over de concepten en functies die zijn betrokken bij het inrichten van apparaten.

## <a name="cross-platform-support"></a>Ondersteuning voor meerdere platformen
Device Provisioning Service werkt net zoals alle andere Azure IoT-services platformoverschrijdende met tal van besturingssystemen. Azure biedt open source-SDK's in een veelheid aan [talen](https://github.com/Azure/azure-iot-sdks) ter bevordering van het verbinden van apparaten en het beheren van de service. Device Provisioning Service ondersteunt de volgende protocollen voor het verbinden van apparaten:

* HTTPS
* AMQP
* AMQP via WebSockets
* MQTT
* MQTT via WebSockets

Device Provisioning Service ondersteunt alleen HTTPS-verbindingen voor servicebewerkingen.

## <a name="regions"></a>Regio's
Device Provisioning Service is beschikbaar in verschillende regio's. De bijgewerkte lijst met bestaande en nieuwe regio's voor alle services wordt aangekondigd op [Azure-regio's](https://azure.microsoft.com/regions/). U kunt de beschikbaarheid van Device Provisioning Service controleren op de pagina [Status van Azure](https://azure.microsoft.com/status/).

> [!NOTE]
> Device Provisioning Service is globaal en is niet gebonden aan een locatie. U moet echter een regio opgeven waarin de metagegevens worden opgeslagen die zijn gekoppeld aan uw profiel van Device Provisioning Service.

## <a name="availability"></a>Beschikbaarheid
Er wordt een serviceovereenkomst met een beschikbaarheid van 99,9% gehanteerd voor Device Provisioning Service. U kunt [de SLA hier lezen](https://azure.microsoft.com/support/legal/sla/iot-hub/). In de volledige [Azure SLA](https://azure.microsoft.com/support/legal/sla/) wordt de gegarandeerde beschikbaarheid van Azure als geheel uitgelegd.

## <a name="quotas"></a>Quota
Voor elk Azure-abonnement gelden standaardquotalimieten. Deze limieten kunnen invloed hebben op het bereik van uw IoT-oplossing. De huidige limiet voor een abonnementsvariant is 10 Device Provisioning Services per abonnement.

Meer informatie over quotalimieten vindt u hier:

* [Azure-abonnement en servicelimieten, quota's en beperkingen](../azure-subscription-service-limits.md)

## <a name="related-azure-components"></a>Gerelateerde Azure-onderdelen
Device Provisioning Service maakt het mogelijk om het inrichten van apparaten met Azure IoT Hub te automatiseren. Lees hier meer informatie over [IoT Hub](https://docs.microsoft.com/azure/iot-hub/).

## <a name="next-steps"></a>Volgende stappen
U hebt nu een algemeen beeld van het inrichten van IoT-apparaten in Azure. De volgende stap is het uitproberen van een compleet IoT-scenario.
> [!div class="nextstepaction"]
> [IoT Hub Device Provisioning Service instellen met Azure Portal](quick-setup-auto-provision.md)
> [Een gesimuleerd apparaat maken en inrichten](quick-create-simulated-device.md)
> [Apparaat voorbereiden voor inrichten](tutorial-set-up-device.md)
