---
title: Wat is Azure Application Gateway?
description: Ontdek hoe u een Azure-toepassingsgateway kunt gebruiken voor het beheren van webverkeer naar uw toepassing.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: overview
ms.custom: mvc
ms.date: 1/22/2019
ms.author: victorh
ms.openlocfilehash: c574e3ab82f97f5fffc7c834a53d19df93fc426f
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54448939"
---
# <a name="what-is-azure-application-gateway"></a>Wat is Azure Application Gateway?

Azure Application Gateway is een load balancer voor webverkeer waarmee u het verkeer naar uw webapps kunt beheren. Traditionele load balancers werken op de transportlaag (OSI-laag 4 - TCP en UDP) en routeren verkeer op basis van IP-bronadres en een bronpoort naar een IP-doeladres en doelpoort.

![Application Gateway conceptual](media/overview/figure1-720.png)

Maar met de Application Gateway kunt u nog specifieker zijn. U kunt bijvoorbeeld verkeer op basis van de binnenkomende URL routeren. Dus als `/images` de binnenkomende URL is, kunt u verkeer routeren naar een specifieke set servers (ook wel een pool genoemd) die is geconfigureerd voor installatiekopieën. Als `/video` zich in de URL bevindt, wordt dat verkeer gerouteerd naar een andere pool die geoptimaliseerd is voor video's.

![imageURLroute](./media/application-gateway-url-route-overview/figure1-720.png)

Dit type routering staat bekend al taakverdeling op de toepassingslaag (OSI-laag 7). Azure Application Gateway kan URL-gebaseerde routering en meer uitvoeren. 

Azure Application Gateway bevat de volgende functies:

## <a name="autoscaling-public-preview"></a>Openbare preview van Automatisch schalen

Behalve de functies die in dit artikel worden beschreven, biedt Application Gateway ook een openbare preview van een nieuwe SKU [Standard_V2] die automatisch schalen en andere belangrijke prestatieverbeteringen levert.

- **Automatisch schalen**: met Application Gateway- of WAF-implementaties van de SKU van Automatische schalen kunt u omhoog of omlaag schalen op basis van veranderende verkeerbelastingspatronen. Automatisch schalen heft ook de vereiste op om tijdens het inrichten een implementatiegrootte of het aantal instanties te kiezen. 

- **Zoneredundantie**: een Application Gateway- of WAF-implementatie kan meerdere Azure-beschikbaarheidszones overbruggen, waardoor het niet nodig is om afzonderlijke Application Gateway-instanties in elke zone in te richten en te implementeren met een Traffic Manager.

- **Statische VIP**: de Application Gateway-VIP biedt nu exclusief ondersteuning voor het VIP-type. Dit zorgt ervoor dat het VIP-adres dat aan Application Gateway is gekoppeld niet verandert, zelfs niet nadat het systeem opnieuw is opgestart.

- **Snellere implementatie en update** in vergelijking met de algemeen beschikbare SKU. 

- **5 keer betere prestaties voor SSL-offload** in vergelijking met de algemeen beschikbare SKU.

Meer informatie over de preview-functies van de openbare preview van Application Gateway raadpleegt u [Automatisch schalen en zone-redundantie voor Application Gateway (openbare preview)](application-gateway-autoscaling-zone-redundant.md).

## <a name="secure-sockets-layer-ssl-termination"></a>SSL-beëindiging (Secure Sockets Layer)

Application Gateway ondersteunt SSL-beëindiging op de gateway, waarna het verkeer normaal gesproken onversleuteld naar de back-endservers wordt doorgeleid. Met deze functie voorkomt u prijzige overhead voor het versleutelen en ontsleutelen voor uw webservers. Maar soms is onversleutelde communicatie met de server echter geen aanvaardbare optie. Dit kan komen door de beveiligings- of nalevingsvereisten, of misschien kan de toepassing alleen worden gebruikt via een beveiligde verbinding. Voor deze toepassingen ondersteunt Application Gateway end-to-end SSL-versleuteling.

## <a name="azure-kubernetes-service-aks-ingress-controller-preview"></a>Ingangscontroller van Azure Kubernetes Service (AKS) (preview) 

De ingangscontroller van Application Gateway wordt uitgevoerd als een pod binnen het AKS-cluster en zorgt ervoor dat de Gateway-toepassing kan fungeren als ingang voor een AKS-cluster. 

Zie [Azure Application Gateway Ingress Controller](https://azure.github.io/application-gateway-kubernetes-ingress/) voor meer informatie.

## <a name="connection-draining"></a>Verwerkingsstop voor verbindingen

Verwerkingsstop voor verbindingen helpt u om back-endgroepsleden zonder problemen te verwijderen tijdens geplande service-updates. Deze instelling wordt ingeschakeld via de HTTP-instelling van de back-end en kan tijdens het maken van de regel worden toegepast op alle leden van een back-endgroep. Wanneer de instelling is ingeschakeld, zorgt Application Gateway ervoor dat alle exemplaren die worden uitgeschreven bij een back-endgroep geen nieuwe aanvraag ontvangen en dat bestaande aanvragen binnen een geconfigureerde tijdslimiet worden voltooid. Dit geldt voor zowel back-endexemplaren die door een API-aanroep expliciet uit de back-endgroep worden verwijderd als voor back-endexemplaren die door de statuscontroles worden gerapporteerd als beschadigd.

## <a name="custom-error-pages"></a>Aangepaste foutpagina's
Met Application Gateway kunt u aangepaste foutpagina's maken in plaats van standaardfoutpagina's weer te geven. U kunt uw eigen huisstijl en lay-out hanteren door een aangepaste foutpagina te gebruiken.

Zie voor meer informatie [Aangepaste foutpagina's maken voor Application Gateway](custom-error.md).

## <a name="web-application-firewall"></a>Web Application Firewall

Web Application Firewall (WAF) is een functie van Application Gateway die gecentraliseerde beveiliging van uw webtoepassingen tegen algemene aanvallen en beveiligingsproblemen biedt. WAF is gebaseerd op regels uit de [Core Rule Set 3.0 of 2.2.9 van OWASP (Open Web Application Security Project)](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project). 

Webtoepassingen zijn in toenemende mate het doel van aanvallen die gebruikmaken van veelvoorkomende bekende beveiligingsproblemen. Veelvoorkomende aanvallen zijn hierbij onder andere aanvallen met SQL-injecties en aanvallen via scripting op meerdere sites. Het kan een hele uitdaging zijn om dergelijke aanvallen in toepassingscode te voorkomen en dit kan tevens veel onderhoud, patching en controle vereisen op meerdere lagen van de toepassingstopologie. Een gecentraliseerde firewall voor webtoepassingen maakt het beveiligingsbeheer veel eenvoudiger en biedt toepassingsbeheerders meer veiligheid tegen bedreigingen of aanvallen. Een WAF-oplossing kan ook sneller reageren op een beveiligingsrisico door een patch voor een bekend beveiligingsprobleem toe te passen op een centrale locatie in plaats van elke afzonderlijke webtoepassing te beveiligen. Bestaande toepassingsgateways kunnen eenvoudig worden geconverteerd naar een toepassingsgateway met Web Application Firewall.

## <a name="url-based-routing"></a>URL-gebaseerde routering

Met op URL-pad gebaseerde routering kunt u verkeer routeren naar back-endserverpools die zijn gebaseerd op de URL-paden van de aanvraag. Een van de scenario's is het routeren van aanvragen voor verschillende inhoudstypen naar een andere pool.

Aanvragen voor `http://contoso.com/video/*` worden bijvoorbeeld doorgestuurd naar VideoServerPool en aanvragen voor `http://contoso.com/images/*` worden doorgestuurd naar ImageServerPool. Als geen van de padpatronen overeenkomen, wordt DefaultServerPool geselecteerd.

## <a name="multiple-site-hosting"></a>Hosting van meerdere sites

Door meerdere sites te hosten, kunt u meer dan een website configureren op dezelfde instantie van de toepassingsgateway. Met deze functie kunt u een efficiëntere topologie voor uw implementaties configureren door maximaal 100 websites toe te voegen aan één toepassingsgateway. Elke website kan worden omgeleid naar een eigen pool. Application Gateway kan bijvoorbeeld verkeer regelen voor `contoso.com` en `fabrikam.com` vanaf twee servergroepen genaamd ContosoServerPool en FabrikamServerPool.

Aanvragen voor `http://contoso.com` worden gerouteerd naar ContoServerPool en aanvragen voor `http://fabrikam.com` worden gerouteerd naar FabrikamServerPool.

Op dezelfde manier kunnen twee subdomeinen van hetzelfde bovenliggende domein worden gehost op dezelfde implementatie van een toepassingsgateway. Voorbeelden van subdomeinen die worden gehost op één toepassingsgateway-implementatie, zijn `http://blog.contoso.com` en `http://app.contoso.com`.

## <a name="redirection"></a>Omleiding

Een veelvoorkomend scenario voor veel webtoepassingen is de ondersteuning van automatische HTTP-naar-HTTPS-omleiding zodat alle communicatie tussen een toepassing en gebruikers plaatsvindt via een versleuteld pad. 

In het verleden hebt u mogelijk gebruikgemaakt van technieken zoals het maken van een specifieke pool met als enige doel het omleiden van aanvragen die worden ontvangen op een HTTP-naar-HTTPS. Application Gateway ondersteunt het omleiden van verkeer op de Application Gateway. Dit vereenvoudigt de configuratie van toepassingen, optimaliseert het resourcegebruik en biedt ondersteuning voor nieuwe omleidingsscenario's, waaronder de globale en op pad gebaseerde omleidingen. Ondersteuning voor Application Gateway-omleiding is niet beperkt tot uitsluitend HTTP-naar-HTTPS-omleiding. Dit is een algemeen omleidingsmechanisme, zodat u op basis van regels kunt omleiden van en naar elke poort die u gebruikt. Ook omleiding naar een externe site wordt ondersteund.

Ondersteuning voor Application Gateway-omleiding biedt de volgende mogelijkheden:

- Globale omleiding van de ene poort naar de andere poort op de Gateway. Hierdoor is HTTP-naar-HTTPS-omleiding op een site mogelijk.
- Padgebaseerde omleiding. Dit type omleiding maakt HTTP-naar-HTTPS-omleiding alleen mogelijk op een specifiek sitegebied, bijvoorbeeld een winkelwagengebied aangegeven door `/cart/*`.
- Omleiden naar een externe site.

## <a name="session-affinity"></a>Sessieaffiniteit

De functie Sessieaffiniteit op basis van cookies is handig als u een gebruikerssessie op dezelfde server wilt behouden. Met behulp van de gatewaybeheerde cookies kan de Application Gateway het daarop volgende verkeer van een gebruikerssessie naar dezelfde server leiden voor verwerking. Dit is belangrijk wanneer de sessiestatus lokaal wordt opgeslagen op de server voor een gebruikerssessie.

## <a name="websocket-and-http2-traffic"></a>Websocket- en HTTP-/2-verkeer

Application Gateway biedt systeemeigen ondersteuning voor de WebSocket- en HTTP-/2-protocollen. Er is geen door de gebruiker configureerbare instelling om selectief WebSocket-ondersteuning in of uit te schakelen.

De WebSocket- en HTTP-/2-protocollen maken full-duplex-communicatie tussen een server en een client mogelijk via een langdurige TCP-verbinding. Dit maakt een meer interactieve communicatie mogelijk tussen de webserver en de client, die bidirectioneel kan zijn zonder dat hiervoor polling nodig is, zoals vereist in implementaties op basis van HTTP. Deze protocollen hebben weinig overhead, in tegenstelling tot HTTP, en kunnen dezelfde TCP-verbinding gebruiken voor meerdere aanvragen/reacties. Dit resulteert in een efficiënter gebruik van resources. Deze protocollen zijn ontworpen om te werken via de traditionele HTTP-poorten: 80 en 443.

## <a name="rewrite-http-headers-public-preview"></a>HTTP-headers opnieuw genereren (openbare preview)

Via HTTP-headers kan vanaf de client en de server aanvullende informatie worden doorgegeven bij de aanvraag of het antwoord. Als u deze HTTP-headers opnieuw genereert, kan dat u helpen diverse belangrijke scenario's te realiseren, bijvoorbeeld het toevoegen van beveiligingsgerelateerde headervelden zoals HSTS/X-XSS-beveiliging, of het verwijderen van de antwoordheadervelden die gevoelige informatie kunnen onthullen, zoals de naam van de back-endserver. 

Application Gateway ondersteunt nu de mogelijkheid om headers van zowel de inkomende HTTP-aanvragen als de uitgaande HTTP-antwoorden opnieuw te genereren. U kunt headers van HTTP-aanvragen en -antwoorden toevoegen, verwijderen of bijwerken terwijl aanvraag- en antwoordpakketten zich verplaatsen tussen de client en back-endpools. U kunt zowel standaardheadervelden (gedefinieerd in [RFC 2616](https://www.ietf.org/rfc/rfc2616.txt)) als niet-standaardheadervelden opnieuw genereren.  

Zie [HTTP-headers opnieuw genereren](rewrite-http-headers.md) voor meer informatie over deze openbare preview-functie.

## <a name="sizing"></a>Grootte aanpassen

Application Gateway wordt momenteel aangeboden in drie grootten: **klein**, **middelgroot** en **groot**. Kleine exemplaargrootten zijn bedoeld voor het ontwikkelen en testen van scenario's.

Zie [Servicelimieten voor Application Gateway](../azure-subscription-service-limits.md?toc=%2fazure%2fapplication-gateway%2ftoc.json#application-gateway-limits) voor een volledige lijst van toepassingsgateway-limieten.

In de volgende tabel staan gemiddelde doorvoerprestaties voor elk toepassingsgateway-exemplaar waarvoor SSL-offload is uitgeschakeld:

| Gemiddelde grootte van een antwoord van de back-endpagina | Klein | Middelgroot | Groot |
| --- | --- | --- | --- |
| 6 kB |7,5 Mbps |13 Mbps |50 Mbps |
| 100 kB |35 Mbps |100 Mbps |200 Mbps |

> [!NOTE]
> Deze waarden zijn geschatte waarden voor de doorvoer van een toepassingsgateway. De werkelijke doorvoer hangt af van verschillende details van de omgeving, zoals de gemiddelde paginagrootte, locatie van back-endexemplaren en de verwerkingstijd voor een pagina. Voor nauwkeurige prestatiecijfers moet u uw eigen tests uitvoeren. Deze waarden worden alleen geboden als richtlijn voor de capaciteitsplanning.

## <a name="next-steps"></a>Volgende stappen

Afhankelijk van uw vereisten en omgeving, kunt u een Application Gateway voor testdoeleinden maken met behulp van de Azure Portal, Azure PowerShell of Azur CLI:

- [Snelstart: Webverkeer omleiden met Azure Application Gateway - Azure Portal](quick-create-portal.md).
- [Snelstart: Webverkeer omleiden met Azure Application Gateway - Azure PowerShell](quick-create-powershell.md)
- [Snelstart: Webverkeer omleiden met Azure Application Gateway - Azure CLI](quick-create-cli.md)
