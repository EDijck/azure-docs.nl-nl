---
title: Beperken van toegang tot - Azure App Service | Microsoft Docs
description: Beperkingen voor Computertoegang gebruiken met Azure App Service
author: ccompy
manager: stefsch
editor: ''
services: app-service\web
documentationcenter: ''
ms.assetid: 3be1f4bd-8a81-4565-8a56-528c037b24bd
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 04/22/2018
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 558b67b5b0e1ce4f452ce2ca2e97dd7e785c80b6
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2019
ms.locfileid: "64728698"
---
# <a name="azure-app-service-access-restrictions"></a>Beperkingen voor Azure App Service-toegang #

Beperkingen voor Computertoegang kunnen u een prioriteit geordende toestaan/weigeren lijst waarmee de toegang tot het netwerk aan uw app te definiëren. De lijst kan bevatten, IP-adressen of subnetten van virtuele Azure-netwerk. Wanneer er een of meer vermeldingen zijn, is er vervolgens een impliciete 'Alles weigeren' die aan het einde van de lijst voorkomt.

De mogelijkheid toegangsbeperkingen werkt met alle App Service gehoste werk wordt geladen met inbegrip van; web-apps, API-apps, Linux-apps, Linux-container-apps en functies.

Wanneer een aanvraag wordt gedaan aan uw app, wordt het adres van de afzender geëvalueerd op basis van de regels van de IP-adres in de lijst met beperkingen voor toegang. Als het adres van de afzender in een subnet dat is geconfigureerd met service-eindpunten naar Microsoft.Web is, klikt u vervolgens het Bronsubnet vergeleken met de regels van het virtuele netwerk in de lijst met beperkingen voor toegang. Als het adres is niet toegestaan voor toegang op basis van de regels in de lijst, de service reageert met een [HTTP 403](https://en.wikipedia.org/wiki/HTTP_403) statuscode.

De mogelijkheid tot beperkingen is geïmplementeerd in de front-end-rollen van App Service, die zijn upstream van de werknemer hosts waarop uw code wordt uitgevoerd. Beperkingen voor Computertoegang is daarom nagenoeg netwerk ACL's.

De mogelijkheid toegang te beperken tot uw web-app uit een Azure Virtual Network (VNet) heet [service-eindpunten][serviceendpoints]. Service-eindpunten kunnen u tot een service met meerdere tenants in de geselecteerde subnetten beperkt. Dit moet worden ingeschakeld op zowel de zijde van de netwerken als de service die wordt ingeschakeld met. 

![toegangsstroom voor beperkingen](media/app-service-ip-restrictions/access-restrictions-flow.png)

## <a name="adding-and-editing-access-restriction-rules-in-the-portal"></a>Toevoegen en bewerken van beperking van toegang door regels in de portal ##

Als u wilt een beperking toegangsregel toevoegen aan uw app, gebruikt u het menu te openen **netwerk**>**toegangsbeperkingen** en klikt u op **toegangsbeperkingen configureren**

![Netwerkopties van App Service](media/app-service-ip-restrictions/access-restrictions.png)  

U kunt vanuit de gebruikersinterface van de beperkingen voor toegang tot de lijst met regels voor toegang-beperking gedefinieerd voor uw app bekijken.

![lijst met toegangsbeperkingen](media/app-service-ip-restrictions/access-restrictions-browse.png)

Deze lijst wordt aangegeven dat alle van de huidige beperkingen die zich op uw app. Als u een VNet-beperking op uw app hebt, ziet u de tabel of service-eindpunten zijn ingeschakeld voor Microsoft.Web. Als er geen gedefinieerde beperkingen met betrekking tot uw app, wordt uw app vanaf elke locatie toegankelijk zijn.  

U kunt klikken op **[+] toevoegen** om toe te voegen een nieuwe regel voor de beperking van toegang. Als u een regel toevoegt, wordt deze zijn onmiddellijk van kracht. Regels worden afgedwongen in volgorde van prioriteit begonnen met het laagste getal en neemt. Er is een impliciete weigeren die van kracht nadat u zelfs een enkele regel toegevoegd.

![een regel van de beperkingen van de IP-toegang toevoegen](media/app-service-ip-restrictions/access-restrictions-ip-add.png)

Wanneer u een regel maakt, moet u toestaan/weigeren en ook op het type regel selecteren. U zijn ook vereist voor de prioriteitswaarde en wat u bent beperkt de toegang tot.  U kunt desgewenst een naam en beschrijving toevoegen aan de regel.  

Om in te stellen van een IP-adres op basis van de regel, selecteer een type IPv4 of IPv6. Notatie van de IP-adres moet worden opgegeven in CIDR-notatie voor zowel IPv4 als IPv6-adressen. Als u wilt een exact adres opgeeft, kunt u er ongeveer als 1.2.3.4/32 waarin de eerste vier octetten vertegenwoordigen uw IP-adres en /32 is het masker. De IPv4-CIDR-notatie voor alle adressen is 0.0.0.0/0. Voor meer informatie over de CIDR-notatie, kunt u lezen [Classless Inter-Domain Routing](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing). 

![een VNet-regel voor toegang tot beperkingen toevoegen](media/app-service-ip-restrictions/access-restrictions-vnet-add.png)

Als u wilt toegang beperken tot de geselecteerde subnetten, selecteer een type van het Virtueelnetwerk. Daaronder kunt u zich om op te halen van het abonnement, VNet en subnet dat u wilt toestaan of weigeren van toegang met. Als service-eindpunten zijn nog niet ingeschakeld met Microsoft.Web voor het subnet dat u hebt geselecteerd, wordt deze automatisch ingeschakeld voor u, tenzij u het selectievakje gevraagd niet om dat te doen. De situatie waarin u wilt inschakelen op de app, maar niet het subnet is grotendeels met betrekking tot hebt u de machtigingen voor het inschakelen van service-eindpunten op het subnet of niet. Als u nodig hebt om op te halen van iemand anders om in te schakelen van service-eindpunten op het subnet, kunt u het selectievakje en uw App geconfigureerd voor service-eindpunten in afwachting van het later opnieuw wordt ingeschakeld op het subnet. 

U kunt klikken op een rij te bewerken van een bestaande regel voor de beperking van toegang. Bewerkingen zijn van kracht inclusief direct wijzigingen in volgorde van prioriteit.

![een toegangsregel beperking bewerken](media/app-service-ip-restrictions/access-restrictions-ip-edit.png)

Wanneer u een regel hebt bewerkt, kunt u het type tussen een regel voor IP-adres en een regel voor het Virtueelnetwerk niet wijzigen. 

![een toegangsregel beperking bewerken](media/app-service-ip-restrictions/access-restrictions-vnet-edit.png)

Als u wilt een regel hebt verwijderd, klikt u op de **...**  op de regel en klik vervolgens op **verwijderen**.

![regel voor beperking van toegang verwijderen](media/app-service-ip-restrictions/access-restrictions-delete.png)

### <a name="scm-site"></a>SCM-site 

Naast het beheren van toegang tot uw app, kunt u ook toegang tot de scm-site die worden gebruikt door uw app beperken. De scm-site is de web-eindpunt en ook de Kudu-console implementeren. U kunt afzonderlijk toegangsbeperkingen toewijst aan de scm-site uit de app of gebruik dezelfde set voor zowel de app en de scm-site. Wanneer u het selectievakje hebben de dezelfde beperkingen als uw app, wordt alles meetfout uit. Als u het selectievakje uitschakelt, worden de instellingen die u eerder had op de scm-site worden toegepast. 

![lijst met toegangsbeperkingen](media/app-service-ip-restrictions/access-restrictions-scm-browse.png)

## <a name="programmatic-manipulation-of-access-restriction-rules"></a>Programmatische manipulatie van beperking toegangsregels ##

Op dit moment is er geen CLI of PowerShell voor de nieuwe functie voor de toegangsbeperkingen, maar de waarden kunnen handmatig worden ingesteld met een PUT-bewerking op de app-configuratie in Resource Manager. U kunt bijvoorbeeld resources.azure.com gebruiken en bewerken van het blok ipSecurityRestrictions om toe te voegen van de vereiste JSON.

De locatie voor deze informatie in Resource Manager is:

management.azure.com/subscriptions/**subscription ID**/resourceGroups/**resource groups**/providers/Microsoft.Web/sites/**web app name**/config/web?api-version=2018-02-01

De JSON-syntaxis voor het vorige voorbeeld is:

    "ipSecurityRestrictions": [
      {
        "ipAddress": "131.107.159.0/24",
        "action": "Allow",
        "tag": "Default",
        "priority": 100,
        "name": "allowed access"
      }
    ],

## <a name="function-app-ip-restrictions"></a>Functie-App-IP-beperkingen

IP-beperkingen zijn beschikbaar voor zowel functie-Apps met dezelfde functionaliteit als App Service-plannen. Inschakelen van IP-beperkingen, wordt de portal code-editor voor eventuele niet-toegestane IP-adressen uitgeschakeld.

[Hier voor meer informatie](../azure-functions/functions-networking-options.md#inbound-ip-restrictions)


<!--Links-->
[serviceendpoints]: https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview
