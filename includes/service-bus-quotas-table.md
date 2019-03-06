---
title: bestand opnemen
description: bestand opnemen
services: service-bus-messaging
author: spelluru
ms.service: service-bus-messaging
ms.topic: include
ms.date: 12/13/2018
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 1123f16e11dd5e35f49e1435eef097f0d53f613b
ms.sourcegitcommit: 8b41b86841456deea26b0941e8ae3fcdb2d5c1e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57350493"
---
De volgende tabel bevat quotumgegevens die specifiek zijn voor Service Bus-berichten. Zie voor informatie over prijzen en andere quota's voor Service Bus, de [prijzen van Service Bus](https://azure.microsoft.com/pricing/details/service-bus/) overzicht.

| Naam van quotum | Bereik | Opmerkingen | Value |
| --- | --- | --- | --- | --- |
| Maximum aantal basic / standard-naamruimten per Azure-abonnement |Naamruimte |De volgende aanvragen voor extra basic / standard-naamruimten worden geweigerd door de portal. |100|
| Maximum aantal premium-naamruimten per Azure-abonnement |Naamruimte |De volgende aanvragen voor extra premium-naamruimten worden geweigerd door de portal. |25 |
| Grootte van de wachtrij/onderwerp |Entiteit |Bij het maken van de wachtrij/onderwerp van gedefinieerd. <br/><br/> Binnenkomende berichten worden geweigerd en een uitzondering is ontvangen door de aanroepende code. |1, 2, 3, 4 of 5 GB.<br /><br />In de Premium-SKU, evenals de Standard met [partitioneren](/azure/service-bus-messaging/service-bus-partitioning) ingeschakeld, wordt de grootte van de wachtrij/onderwerp van maximaal 80 GB. |
| Aantal gelijktijdige verbindingen voor een naamruimte |Naamruimte |De volgende aanvragen voor extra verbindingen worden geweigerd en een uitzondering is ontvangen door de aanroepende code. REST-bewerkingen tellen niet mee voor gelijktijdige TCP-verbindingen. |NetMessaging: 1000<br /><br />AMQP: 5.000 |
| Aantal gelijktijdige aanvragen voor een entiteit wachtrij/onderwerp/abonnement ontvangen |Entiteit |Volgende ontvangen aanvragen zijn afgewezen en een uitzondering is ontvangen door de aanroepende code. Dit quotum is van toepassing op het gecombineerde aantal gelijktijdige bewerkingen voor alle abonnementen op een onderwerp ontvangen. |5.000 |
| Aantal onderwerpen/wachtrijen per naamruimte |Naamruimte |De volgende aanvragen voor het maken van een nieuw onderwerp of een wachtrij op de naamruimte worden geweigerd. Als gevolg hiervan, indien geconfigureerd via de [Azure-portal][Azure portal], een foutbericht dat wordt gegenereerd. Als met de naam van de beheer-API, wordt een uitzondering ontvangen door de aanroepende code. |1000 (basic/Standard-laag). Het totale aantal onderwerpen en -wachtrijen in een naamruimte moet kleiner dan of gelijk zijn aan 1000. <br/><br/>Voor premium-laag, 1000 per messaging unit(MU). Maximale limiet is 4000. |
| Aantal [gepartitioneerd onderwerpen/wachtrijen](/azure/service-bus-messaging/service-bus-partitioning) per naamruimte |Naamruimte |De volgende aanvragen voor het maken van een nieuwe gepartitioneerde onderwerp of een wachtrij op de naamruimte worden geweigerd. Als gevolg hiervan, indien geconfigureerd via de [Azure-portal][Azure portal], een foutbericht dat wordt gegenereerd. Als met de naam van de beheer-API, een **QuotaExceededException** uitzondering wordt ontvangen door de aanroepende code. |Basic en Standard-laag - 100<br/><br/>Gepartitioneerde entiteiten worden niet ondersteund in de [Premium](../articles/service-bus-messaging/service-bus-premium-messaging.md) laag.<br/><br />Elke gepartitioneerde wachtrij of onderwerp telt voor het quotum van 1000 entiteiten per naamruimte. |
| Maximale grootte van alle berichten pad van de entiteit: wachtrij of onderwerp |Entiteit |- |260 tekens |
| Maximale grootte van alle berichten entiteitnaam: naamruimte, abonnement of het abonnement regel |Entiteit |- |50 tekens |
| Maximale grootte van een [bericht-ID](/dotnet/api/microsoft.azure.servicebus.message.messageid) | Entiteit |- | 128 |
| Maximale grootte van een bericht [sessie-id](/dotnet/api/microsoft.azure.servicebus.message.sessionid) | Entiteit |- | 128 |
| Grootte van het bericht voor een wachtrij/onderwerp/abonnement-entiteit |Entiteit |Binnenkomende berichten die groter zijn dan deze quota worden geweigerd en een uitzondering is ontvangen door de aanroepende code. |Maximale berichtgrootte: 256 KB ([Standard-laag](../articles/service-bus-messaging/service-bus-premium-messaging.md)) / 1 MB ([Premium-laag](../articles/service-bus-messaging/service-bus-premium-messaging.md)). <br /><br />Vanwege de systeembelasting is deze limiet kleiner dan deze waarden.<br /><br />Maximale header-grootte: 64 kB<br /><br />Maximum aantal eigenschappen van de koptekst in eigenschappenverzameling: **byte/int. Maximumwaarde**<br /><br />Maximale grootte van de eigenschap in de eigenschappenverzameling: Er is geen expliciete limiet. Beperkt door de maximale header-grootte. |
| De eigenschap berichtgrootte voor een wachtrij/onderwerp/abonnement-entiteit |Entiteit |Een **SerializationException** uitzondering wordt gegenereerd. |Maximale berichtgrootte eigenschap voor elke eigenschap is 32 K. cumulatieve grootte van alle eigenschappen niet langer zijn dan 64 K. Deze limiet geldt voor de hele koptekst van de [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage), die is voorzien van beide eigenschappen van gebruikers, evenals eigenschappen (zoals [SequenceNumber](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.sequencenumber), [Label](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.label), [ MessageId](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.messageid), enzovoort). |
| Aantal abonnementen per onderwerp |Entiteit |De volgende aanvragen voor het maken van aanvullende abonnementen voor het onderwerp worden geweigerd. Als gevolg hiervan als geconfigureerd via de portal, wordt een foutbericht weergegeven. Als met de naam van de API management is een uitzondering ontvangen door de aanroepende code. |Standard-laag - elk abonnement in mindering gebracht op het quotum van 1000 entiteiten (wachtrijen, onderwerpen en abonnementen) per naamruimte. <br/> <br/> Premium-laag - 2000 |
| Het aantal SQL-filters per onderwerp |Entiteit |De volgende aanvragen voor het maken van aanvullende filters op het onderwerp worden geweigerd en een uitzondering wordt ontvangen door de aanroepende code. |2,000 |
| Aantal correlatiefilters per onderwerp |Entiteit |De volgende aanvragen voor het maken van aanvullende filters op het onderwerp worden geweigerd en een uitzondering wordt ontvangen door de aanroepende code. |100.000 |
| Grootte van de SQL-filters/acties |Naamruimte |De volgende aanvragen voor het maken van extra filters worden geweigerd en een uitzondering is ontvangen door de aanroepende code. |Maximale lengte van de filtertekenreeks voor de voorwaarde: 1024 (1 K).<br /><br />Maximale lengte van de regel actie tekenreeks: 1024 (1 K).<br /><br />Maximum aantal expressies per regelactie: 32. |
| Aantal [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) regels per naamruimte, wachtrij of onderwerp |Entiteit, naamruimte |De volgende aanvragen voor het maken van aanvullende regels worden geweigerd en een uitzondering wordt ontvangen door de aanroepende code. |Maximum aantal regels: 12. <br /><br /> Regels die zijn geconfigureerd op een Service Bus-naamruimte van toepassing op alle wachtrijen en onderwerpen in die naamruimte. |
| Aantal berichten per transactie | Transactie | Extra binnenkomende berichten worden geweigerd en een uitzondering met de mededeling ' kan niet meer dan 100 berichten verzenden in één transactie"is ontvangen door de aanroepende code. | 100 <br /><br /> Voor beide **Send()** en **SendAsync()** bewerkingen. |

[Azure portal]: https://portal.azure.com