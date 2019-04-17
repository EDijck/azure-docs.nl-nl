---
title: Definitietaal van werkstroom - Azure Logic Apps-schemaverwijzing
description: Naslaggids voor Definitietaal van werkstroomschema in Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: reference
ms.date: 04/30/2018
ms.openlocfilehash: d80ffa862546f56e93a338a7a1db031e2cb55990
ms.sourcegitcommit: fec96500757e55e7716892ddff9a187f61ae81f7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2019
ms.locfileid: "59616795"
---
# <a name="schema-reference-for-workflow-definition-language-in-azure-logic-apps"></a>Definitietaal van werkstroom in Azure Logic Apps-schemaverwijzing

Wanneer u een logische app in maakt [Azure Logic Apps](../logic-apps/logic-apps-overview.md), uw logische app heeft een onderliggende werkstroomdefinitie die worden beschreven van de werkelijke logica die in uw logische app wordt uitgevoerd. Deze werkstroomdefinitie maakt gebruik van [JSON](https://www.json.org/) en een structuur die gevalideerd door het schema Definitietaal van werkstroom volgt. Deze referentie bevat een overzicht van deze structuur en hoe elementen in het schema wordt gedefinieerd in de werkstroomdefinitie van de.

## <a name="workflow-definition-structure"></a>Structuur van de werkstroom-definitie

Altijd een werkstroomdefinitie bevat een trigger voor het instantiëren van uw logische app, plus een of meer acties die worden uitgevoerd na de trigger wordt geactiveerd.

Hier volgt de structuur op hoog niveau voor de werkstroomdefinitie van een:

```json
"definition": {
  "$schema": "<workflow-definition-language-schema-version>",
  "contentVersion": "<workflow-definition-version-number>",
  "parameters": { "<workflow-parameter-definitions>" },
  "triggers": { "<workflow-trigger-definitions>" },
  "actions": { "<workflow-action-definitions>" },
  "outputs": { "<workflow-output-definitions>" }
}
```

| Element | Vereist | Description |
|---------|----------|-------------|
| definitie | Ja | Het eerste element voor uw werkstroomdefinitie |
| $schema | Alleen wanneer u extern verwijst naar een definitie van de werkstroom | De locatie voor de JSON-schema-bestand dat beschrijft de versie Definitietaal van werkstroom, die u hier kunt vinden: <p>`https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json`</p> |
| contentVersion | Nee | Het versienummer voor de werkstroomdefinitie van uw, namelijk '1.0.0.0"standaard. Om te identificeren en controleer of de definitie van de juiste bij het implementeren van een werkstroom, Geef een waarde te gebruiken. |
| parameters | Nee | De definities voor een of meer parameters die het doorgeven van gegevens in uw werkstroom <p><p>Maximum aantal parameters: 50 |
| triggers | Nee | De definities voor een of meer triggers die exemplaar maken van uw werkstroom. U kunt meer dan één trigger, maar alleen met de Werkstroomdefinitietaal, visueel niet via de ontwerper van logische Apps definiëren. <p><p>Maximum-triggers: 10 |
| Acties | Nee | De definities voor een of meer acties uit te voeren op de werkstroom-runtime <p><p>Maximaal aantal acties: 250 |
| uitvoer | Nee | De definities voor de uitvoer die uit een werkstroom ophalen <p><p>Maximale uitvoer: 10 |
||||

## <a name="parameters"></a>Parameters

In de `parameters` sectie, het definiëren van de werkstroomparameters die gebruikmaakt van de werkstroomdefinitie van de bij de implementatie voor het accepteren van invoer. Zowel parameterdeclaraties en parameterwaarden zijn vereist tijdens de implementatie. Voordat u deze parameters in andere gedeelten van de werkstroom gebruiken kunt, ervoor zorgen dat u de parameters in deze secties declareren. 

Hier volgt de algemene structuur voor een parameterdefinitie:

```json
"parameters": {
  "<parameter-name>": {
    "type": "<parameter-type>",
    "defaultValue": "<default-parameter-value>",
    "allowedValues": [ <array-with-permitted-parameter-values> ],
    "metadata": {
      "key": {
        "name": "<key-value>"
      }
    }
  }
},
```

| Element | Vereist | Type | Beschrijving |
|---------|----------|------|-------------|
| type | Ja | int, float, string, securestring, bool, matrix, JSON-object, secureobject <p><p>**Opmerking**: Voor alle wachtwoorden, sleutels en geheimen, gebruikt u de `securestring` en `secureobject` omdat de `GET` bewerking deze typen niet retourneren. Zie voor meer informatie over het beveiligen van parameters [uw logische app beveiligen](../logic-apps/logic-apps-securing-a-logic-app.md#secure-action-parameters) | Het type voor de parameter |
| defaultValue | Ja | Gelijk aan `type` | De standaardwaarde voor parameter wanneer er geen waarde is opgegeven wanneer de werkstroom waarmee een wordt gemaakt |
| allowedValues | Nee | Gelijk aan `type` | Een matrix met waarden die de parameter kunt accepteren |
| metagegevens | Nee | JSON-object | Andere details van de parameter, bijvoorbeeld de naam of een leesbare beschrijving op voor uw logische app of stroom of het moment van ontwerp-gegevens die worden gebruikt door Visual Studio of andere hulpprogramma 's |
||||

## <a name="triggers-and-actions"></a>Triggers en acties

In het werkstroomdefinitie van een, de `triggers` en `actions` secties definiëren de aanroepen die ontstaan tijdens de uitvoering van uw werkstroom. Zie voor de syntaxis en meer informatie over deze secties, [werkstroomtriggers en acties](../logic-apps/logic-apps-workflow-actions-triggers.md).

## <a name="outputs"></a>Uitvoer

In de `outputs` sectie, het definiëren van de gegevens die door uw werkstroom kunt wanneer u klaar bent met. Bijvoorbeeld, voor het volgen van een specifieke status of de waarde van elke uitvoering, opgeven dat de werkstroomuitvoer die gegevens retourneert.

> [!NOTE]
> Als u reageert op binnenkomende aanvragen van een service een REST-API, gebruik dan niet `outputs`. Gebruik in plaats daarvan de `Response` actietype. Zie voor meer informatie, [werkstroomtriggers en acties](../logic-apps/logic-apps-workflow-actions-triggers.md).

Hier volgt de algemene structuur voor de uitvoerdefinitie van een:

```json
"outputs": {
  "<key-name>": {
    "type": "<key-type>",
    "value": "<key-value>"
  }
}
```

| Element | Vereist | Type | Description |
|---------|----------|------|-------------|
| <*key-name*> | Ja | String | De naam van de sleutel voor de uitvoer waarde retourneren |
| type | Ja | int, float, string, securestring, bool, matrix, JSON-object | Het type voor de geretourneerde waarde van uitvoer |
| waarde | Ja | Gelijk aan `type` | De geretourneerde waarde van uitvoer |
|||||

Als u de uitvoer van een werkstroom, bekijk de uitvoeringsgeschiedenis en details in de Azure-portal van uw logische app of gebruik de [werkstroom REST-API](https://docs.microsoft.com/rest/api/logic/workflows). U kunt ook uitvoer doorgeven aan externe systemen, bijvoorbeeld Power BI, zodat u kunt dashboards maken.

<a name="expressions"></a>

## <a name="expressions"></a>Expressies

Met JSON hebt u letterlijke waarden die aanwezig zijn in de ontwerpfase, bijvoorbeeld:

```json
"customerName": "Sophia Owen",
"rainbowColors": ["red", "orange", "yellow", "green", "blue", "indigo", "violet"],
"rainbowColorsCount": 7
```

U kunt ook de waarden die niet tot uitvoering hebben. Als u wilt deze waarden vertegenwoordigen, kunt u *expressies*, die tijdens de uitvoering worden geëvalueerd. Een expressie is een reeks met een of meer [functies](#functions), [operators](#operators), variabelen, expliciete waarden of constanten. In het definitie van de werkstroom, kunt u een expressie overal in een JSON-tekenreeks-waarde door het voorvoegsel van de expressie met het apenstaartje (\@). Bij het evalueren van een expressie die een JSON-waarde vertegenwoordigt de hoofdtekst van de expressie wordt opgehaald door het verwijderen van de \@ teken en altijd resultaten in een andere JSON-waarde.

Bijvoorbeeld: voor de eerder gedefinieerde `customerName` eigenschap, kunt u de waarde van de eigenschap ophalen met behulp van de [parameters()](../logic-apps/workflow-definition-language-functions-reference.md#parameters) werken in een expressie en toe te wijzen die waarde aan de `accountName` eigenschap:

```json
"customerName": "Sophia Owen",
"accountName": "@parameters('customerName')"
```

*Tekenreeks interpolatie* ook kunt u meerdere expressies in tekenreeksen die zijn ingepakt door de \@ teken en de accolades ({}). Hier volgt de syntaxis:

```json
@{ "<expression1>", "<expression2>" }
```

Het resultaat is altijd een tekenreeks, waardoor deze mogelijkheid die vergelijkbaar is met de `concat()` functioneren, bijvoorbeeld: 

```json
"customerName": "First name: @{parameters('firstName')} Last name: @{parameters('lastName')}"
```

Als u een letterlijke tekenreeks begint hebt met de \@ teken, het voorvoegsel de \@ teken met een andere \@ teken als een escape-teken: \@\@

Deze voorbeelden geven weer hoe expressies worden geëvalueerd:

| JSON-waarde | Resultaat |
|------------|--------|
| "Sophia Owen" | Deze tekens retourneren: 'Sophia Owen' |
| "[1] array" | Deze tekens retourneren: 'array [1]' |
| "\@\@" | Deze tekens retourneren als een tekenreeks met één teken: '\@' |
| " \@" | Deze tekens retourneren als een tekenreeks van twee tekens: ' \@' |
|||

Stel dat u 'myBirthMonth' gelijk is aan "Januari" en "myAge" gelijk aan het aantal 42 definiëren voor deze voorbeelden:

```json
"myBirthMonth": "January",
"myAge": 42
```

Deze voorbeelden laten zien hoe de volgende expressies worden geëvalueerd:

| JSON-expressie | Resultaat |
|-----------------|--------|
| "\@parameters('myBirthMonth')" | Deze tekenreeks geretourneerd: "Januari" |
| "\@{parameters('myBirthMonth')}" | Deze tekenreeks geretourneerd: "Januari" |
| "\@parameters('myAge')" | Retourneert dit getal: 42 |
| "\@{parameters('myAge')}" | Retourneert dit getal als een tekenreeks: "42" |
| "Mijn leeftijd is \@{parameters('myAge')}" | Deze tekenreeks geretourneerd: "Mijn leeftijd is 42" |
| "\@concat ('mijn leeftijd is', string(parameters('myAge')))" | Deze tekenreeks geretourneerd: "Mijn leeftijd is 42" |
| "Mijn leeftijd is \@ \@{parameters('myAge')}" | Deze tekenreeks, waaronder de expressie retourneren: ' Mijn leeftijd is \@{parameters('myAge')}' |
|||

Wanneer u visueel in de ontwerper van logische Apps werkt, kunt u bijvoorbeeld expressies via de opbouwfunctie voor expressies maken:

![Ontwerper van logische Apps > opbouwfunctie voor expressies](./media/logic-apps-workflow-definition-language/expression-builder.png)

Wanneer u klaar bent, de expressie wordt weergegeven voor de overeenkomstige eigenschap in de definitie van de werkstroom, bijvoorbeeld, de `searchQuery` eigenschap hier:

```json
"Search_tweets": {
  "inputs": {
    "host": {
      "connection": {
        "name": "@parameters('$connections')['twitter']['connectionId']"
      }
    }
  },
  "method": "get",
  "path": "/searchtweets",
  "queries": {
    "maxResults": 20,
    "searchQuery": "Azure @{concat('firstName','', 'LastName')}"
  }
},
```

<a name="operators"></a>

## <a name="operators"></a>Operators

In [expressies](#expressions) en [functies](#functions), operators uitvoeren van specifieke taken, zoals een eigenschap of een waarde in een matrix.

| Operator | Taak |
|----------|------|
| ' | Voor het gebruik van een letterlijke tekenreeks zijn als invoer of in expressies en functies, verpakken de tekenreeks alleen met enkele aanhalingstekens, bijvoorbeeld `'<myString>'`. Gebruik geen dubbele aanhalingstekens (""), die in strijd zijn met de JSON-indeling om een volledige expressie. Bijvoorbeeld: <p>**Ja**: length('Hello') </br>**Geen**: length("Hello") <p>Als u matrices of getallen worden gehaald, hoeft u geen interpunctie wrapping. Bijvoorbeeld: <p>**Ja**: lengte ([1, 2, 3]) </br>**Geen**: lengte ('[1, 2, 3]') |
| [] | Als u wilt verwijzen naar een waarde op een specifieke positie (index) in een matrix, gebruikt u vierkante haken. Als u bijvoorbeeld het tweede item in een matrix ophalen: <p>`myArray[1]` |
| . | Als u wilt verwijzen naar een eigenschap van een object, gebruikt u de puntoperator. Bijvoorbeeld, om op te halen de `name` eigenschap voor een `customer` JSON-object: <p>`"@parameters('customer').name"` |
| ? | Als u verwijst naar null eigenschappen in een object zonder een runtime-fout, gebruikt u de operator vraagteken. Bijvoorbeeld, voor het afhandelen van null uitvoer van een trigger, kunt u deze expressie: <p>`@coalesce(trigger().outputs?.body?.<someProperty>, '<property-default-value>')` |
|||

<a name="functions"></a>

## <a name="functions"></a>Functions

Sommige expressies worden de waarden van de runtime-acties die mogelijk nog niet wanneer de werkstroomdefinitie van de wordt gestart om uit te voeren. U kunt gebruiken om te verwijzen naar of te werken met deze waarden in expressies, [ *functies* ](../logic-apps/workflow-definition-language-functions-reference.md) waarmee de Definitietaal van werkstroom.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [Definitietaal van werkstroom acties en triggers](../logic-apps/logic-apps-workflow-actions-triggers.md)
* Meer informatie over het programmatisch maken en beheren van logische apps met de [werkstroom REST-API](https://docs.microsoft.com/rest/api/logic/workflows)
