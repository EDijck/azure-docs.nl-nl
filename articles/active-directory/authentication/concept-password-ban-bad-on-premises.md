---
title: Azure AD wachtwoord protection preview
description: Verbieden zwakke wachtwoorden in on-premises Active Directory met behulp van de Azure AD wachtwoord protection preview
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/25/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jsimmons
ms.openlocfilehash: 45c77aa63159a8e62f65c0e250c0ace95b0d344e
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54429902"
---
# <a name="preview-enforce-azure-ad-password-protection-for-windows-server-active-directory"></a>Preview: Afdwingen van de beveiliging van Azure AD-wachtwoord voor Windows Server Active Directory

|     |
| --- |
| Beveiliging van Azure AD-wachtwoord en de lijst met aangepaste uitgesloten wachtwoorden zijn openbare preview-functies van Azure Active Directory. Zie voor meer informatie over previews [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Beveiliging van Azure AD-wachtwoord is een nieuwe functie in openbare preview, mogelijk gemaakt door Azure Active Directory (Azure AD) voor het verbeteren van wachtwoordbeleid in een organisatie. De on-premises implementatie van Azure AD-wachtwoordbeveiliging maakt gebruik van zowel het algemene en aangepaste lijsten met wachtwoorden die zijn opgeslagen in Azure AD geblokkeerd en de dezelfde controles on-premises als Azure AD cloud-gebaseerde wijzigingen uitvoert.

Er zijn drie softwareonderdelen die gezamenlijk protection voor Azure AD-wachtwoord:

* De Azure AD wachtwoord protection proxy-service wordt uitgevoerd op elke machine domein in de huidige Active Directory-forest. Het aanvragen van domeincontrollers worden doorgestuurd naar Azure AD en het antwoord geretourneerd van Azure AD terug naar de domeincontroller.
* De agent-service van Azure AD wachtwoord beveiliging DC ontvangt verzoeken van wachtwoord-validatie van het DLL-bestand van DC-Agent wachtwoord filter, verwerkt met behulp van het huidige lokaal beschikbaar wachtwoordbeleid en retourneert het resultaat (pass\fail). Deze service is verantwoordelijk voor het periodiek (één keer per uur) aanroepen van de Azure AD wachtwoord protection proxy-service om op te halen van nieuwe versies van het wachtwoordbeleid. Communicatie voor aanroepen naar en van de Azure AD wachtwoord protection proxy-service wordt verwerkt via RPC (Remote Procedure Call) via TCP. Bij het ophalen, wordt nieuwe beleid opgeslagen in een sysvol-map waar ze naar andere domeincontrollers repliceren kunnen. De DC-agent-service bewaakt ook de map sysvol voor wijzigingen in het geval er nieuwe wachtwoordbeleid door andere domeincontrollers hebt geschreven, de controle van de proxy-service van Azure AD wachtwoord beveiliging wordt overgeslagen als een voldoende recente beleid al beschikbaar is.
* Het DLL-bestand van DC-Agent wachtwoord filter ontvangt verzoeken van wachtwoord-validatie van het besturingssysteem en ze doorstuurt naar de Azure AD wachtwoord protection DC-agent-service lokaal wordt uitgevoerd op de domeincontroller.

![Hoe Azure AD wachtwoord beveiliging onderdelen samenwerken.](./media/concept-password-ban-bad-on-premises/azure-ad-password-protection.png)

### <a name="license-requirements"></a>Licentievereisten

De voordelen van de lijst met uitgesloten wachtwoorden globale gelden voor alle gebruikers van Azure Active Directory (Azure AD).

De lijst met aangepaste uitgesloten wachtwoorden vereist Azure AD Basic-licenties.

Azure AD-wachtwoordbeveiliging voor Windows Server Active Directory vereist Azure AD Premium-licenties.

Extra licentie-informatie, inclusief kosten, vindt u op de [Azure Active Directory-site prijzen](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="download"></a>Downloaden

Er zijn twee vereist installatieprogramma's voor beveiliging van de Azure AD-wachtwoorden die kan worden gedownload vanaf de [Microsoft Downloadcentrum](https://www.microsoft.com/download/details.aspx?id=57071)

## <a name="answers-to-common-questions"></a>Antwoorden op veelgestelde vragen

* Er is geen verbinding met internet naar de domeincontrollers vereist. De computer (s) met de proxy-service van Azure AD wachtwoord beveiliging zijn de enige machines dat internetconnectiviteit vereist.
* Er is geen netwerkpoorten zijn geopend op domeincontrollers.
* Er zijn geen wijzigingen in Active Directory-schema zijn vereist.
* De software maakt gebruik van de bestaande Active Directory-container en serviceConnectionPoint schemaobjecten.
* Er is geen Active Directory-domein of Forest functionele niveau (DFL\FFL) minimumvereiste.
* De software niet maken of vereisen dat alle accounts in Active Directory-domeinen die worden beschermd.
* Stapsgewijze implementatie wordt ondersteund daar staat tegenover dat beleid van het wachtwoord alleen wordt afgedwongen waar de domain controller-agent is geïnstalleerd.
* Het verdient aanbeveling de DC-agent installeren op alle DC's om te controleren of wachtwoord NAP-afdwinging. 
* Beveiliging van Azure AD-wachtwoord is niet een realtime toepassing beleidsengine. Mogelijk zijn er een vertraging in de tijd tussen een wachtwoordwijziging voor de configuratie van beleid en de tijd die het heeft bereikt en op alle domeincontrollers wordt afgedwongen.

## <a name="next-steps"></a>Volgende stappen

[Wachtwoordbeveiliging in Azure AD implementeren](howto-password-ban-bad-on-premises-deploy.md)
