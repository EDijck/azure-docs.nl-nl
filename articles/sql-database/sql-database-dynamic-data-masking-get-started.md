---
title: Azure SQL Database dynamische gegevensmaskering | Microsoft docs
description: SQL Database dynamische gegevensmaskering blootstelling van gevoelige gegevens beperkt door het maskeren voor niet-gemachtigde gebruikers
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: ronitr
ms.author: ronitr
ms.reviewer: vanto
manager: craigg
ms.date: 12/16/2018
ms.openlocfilehash: 3e807033b109b8281057f6881a315f5c1c783a22
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/17/2018
ms.locfileid: "53536369"
---
# <a name="sql-database-dynamic-data-masking"></a>Dynamische gegevensmaskering voor SQL-Database

SQL Database dynamische gegevensmaskering blootstelling van gevoelige gegevens door deze te maskeren voor niet-gemachtigde gebruikers. 

Dynamische gegevensmaskering helpt onbevoegde toegang tot gevoelige gegevens te voorkomen, doordat klanten kunnen aangeven hoeveel van de gevoelige gegevens mag worden vrijgegeven, met minimale gevolgen voor de toepassingslaag. Dit is een beveiligingsfunctie op basis van beleid. De gevoelige gegevens in de resultatenset van een query die is uitgevoerd op toegewezen databasevelden worden verborgen, terwijl de gegevens in de database niet worden gewijzigd.

Bijvoorbeeld, een vertegenwoordiger van de service op een callcenter aanroepers kan worden geïdentificeerd door verschillende cijfers van hun creditcardnummer, maar deze gegevensitems niet volledig worden blootgesteld aan de klantenservice. Een maskeringsregel kan worden gedefinieerd dat alle maskers, maar de laatste vier cijfers van een willekeurig aantal creditcard is geregistreerd in het resultaat van een query moet worden ingesteld. Als een ander voorbeeld kan een masker voor de juiste gegevens ter bescherming van gegevens van persoonlijk identificeerbare informatie (PII), worden gedefinieerd zodat een ontwikkelaar productie-omgevingen opvragen kan voor het oplossen van problemen zonder te schenden van de regelgeving.

## <a name="sql-database-dynamic-data-masking-basics"></a>SQL Database dynamische gegevensmaskering grondbeginselen
U instellen kunt een beleid in de Azure-portal voor een dynamische gegevensmaskering door het selecteren van de bewerking in de blade SQL Database-configuratie of de instellingenblade voor dynamische gegevensmaskering.

### <a name="dynamic-data-masking-permissions"></a>Dynamic data masking machtigingen
Dynamische gegevensmaskering kan worden geconfigureerd door de beheerder van de Azure-Database, serverbeheerder of [SQL Security Manager](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#sql-security-manager) rollen.

### <a name="dynamic-data-masking-policy"></a>Beleid voor dynamische gegevens maskeren
* **SQL-gebruikers die zijn uitgesloten van maskering** : een set van SQL-gebruikers of AAD-identiteiten die toegankelijk maakt gegevens in de SQL ophalen-queryresultaten. Gebruikers met beheerdersrechten zijn altijd uitgesloten van maskeren en de oorspronkelijke gegevens zonder een masker zien.
* **Regels maskeren** -een set regels die de aangewezen velden om te worden gemaskeerd definiëren en de maskeringsfunctie die wordt gebruikt. De opgegeven velden kunnen worden gedefinieerd met behulp van een schema databasenaam, de tabelnaam en de kolomnaam.
* **Functies maskeren** -een aantal methoden waarmee de blootstelling van gegevens voor verschillende scenario's.

| Maskeringsfunctie | Logische maskeren |
| --- | --- |
| **Standaard** |**Volledige maskeren op basis van de gegevenstypen van de desbetreffende velden**<br/><br/>• Gebruik XXXX of minder Xs als de grootte van het veld minder dan 4 tekens in voor de gegevenstypen string (nchar, ntext, nvarchar is).<br/>• Gebruik een waarde van nul voor numerieke gegevenstypen (bigint, bits, decimal, int, geld, numerieke, smallint, smallmoney, tinyint, float, real).<br/>• Gebruik 01-01-1900 voor datum/tijd-gegevenstypen (datum, datetime2, datum/tijd, datetimeoffset, smalldatetime, tijd).<br/>• Voor SQL-variant, de standaardwaarde van het huidige type wordt gebruikt.<br/>• Voor het document XML <masked/> wordt gebruikt.<br/>• Gebruik een lege waarde voor speciale gegevenstypen (timestamp tabel, hierarchyid, GUID, binaire bestanden, afbeeldingen, varbinary ruimtelijke typen). |
| **Creditcard** |**Methode, waarmee wordt aangegeven dat de laatste vier cijfers van de opgegeven velden te maskeren** en wordt een constante tekenreeks toegevoegd als een voorvoegsel in de vorm van een creditcard.<br/><br/>XXXX-XXXX-XXXX-1234 |
| **E-mail** |**Methode, waarmee wordt aangegeven dat de eerste letter en vervangt u het domein met XXX.com maskeren** met behulp van een constante tekenreeks-voorvoegsel in de vorm van een e-mailadres.<br/><br/>aXX@XXXX.com |
| **Willekeurig getal** |**Methode, waarbij een willekeurig getal genereert maskeren** op basis van de geselecteerde grenzen en de werkelijke gegevenstypen. Als de opgegeven grenzen bevindt gelijk zijn, is de maskeringsfunctie een constante waarde.<br/><br/>![Navigatiedeelvenster](./media/sql-database-dynamic-data-masking-get-started/1_DDM_Random_number.png) |
| **Aangepaste tekst** |**Methode, waarmee wordt aangegeven dat de eerste en laatste tekens maskeren** en wordt een tekenreeks met aangepaste opvulling toegevoegd in het midden. Als de oorspronkelijke reeks korter dan de beschikbaar gemaakte voorvoegsel en het achtervoegsel is, worden alleen de opvulling-tekenreeks wordt gebruikt. <br/>voorvoegsel [opvulling] achtervoegsel<br/><br/>![Navigatiedeelvenster](./media/sql-database-dynamic-data-masking-get-started/2_DDM_Custom_text.png) |

<a name="Anchor1"></a>

### <a name="recommended-fields-to-mask"></a>Aanbevolen velden om te maskeren
De recommendations-engine van DDM vlaggen bepaalde velden uit de database als potentieel gevoelige velden die zijn goede kandidaten voor maskeren. In de blade Dynamic Data Masking in de portal ziet u de aanbevolen kolommen voor uw database. U hoeft alleen is, klikt u op **masker toevoegen** voor een of meer kolommen en vervolgens **opslaan** om toe te passen een masker voor deze velden.

## <a name="set-up-dynamic-data-masking-for-your-database-using-powershell-cmdlets"></a>Instellen van dynamische gegevensmaskering voor uw database met behulp van Powershell-cmdlets
Zie [Azure SQL Database-Cmdlets](https://docs.microsoft.com/powershell/module/azurerm.sql).

## <a name="set-up-dynamic-data-masking-for-your-database-using-rest-api"></a>Instellen van dynamische gegevensmaskering voor uw database met behulp van REST-API
Zie [bewerkingen voor Azure SQL Database](https://msdn.microsoft.com/library/dn505719.aspx).

