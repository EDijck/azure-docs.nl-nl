---
title: 'Quickstart: Python gebruiken om de Text Analytics-API aan te roepen'
titleSuffix: Azure Cognitive Services
description: Get-informatie en codevoorbeelden om u te helpen snel aan de slag met behulp van de Tekstanalyse-API in Azure Cognitive Services.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: quickstart
ms.date: 04/16/2019
ms.author: aahi
ms.openlocfilehash: 69eb3789586233b824da1ef6a9c338b07281f324
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/22/2019
ms.locfileid: "60001385"
---
# <a name="quickstart-using-python-to-call-the-text-analytics-cognitive-service"></a>Quickstart: Python gebruiken om de Text Analytics Cognitive Service aan te roepen 
<a name="HOLTop"></a>

Hieronder ziet u hoe u de [Text Analytics-API's](//go.microsoft.com/fwlink/?LinkID=759711) met Python kunt gebruiken om [taal te detecteren](#Detect), [gevoel te analyseren](#SentimentAnalysis) en [sleuteltermen op te halen](#KeyPhraseExtraction).

U kunt dit voorbeeld vanaf de opdrachtregel of als een Jupyter-notebook uitvoert op [MyBinder](https://mybinder.org) door te klikken op de lancering Binder badge:

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=TextAnalytics.ipynb)

### <a name="command-line"></a>Opdrachtregel

U moet mogelijk bijwerken [IPython](https://ipython.org/install.html), de kernel voor Jupyter:
```bash
pip install --upgrade IPython
```

U moet mogelijk bijwerken de [aanvragen](http://docs.python-requests.org/en/master/) bibliotheek:
```bash
pip install requests
```

Raadpleeg de [API-definities](//go.microsoft.com/fwlink/?LinkID=759346) voor technische documentatie voor de API's.

## <a name="prerequisites"></a>Vereisten

* [!INCLUDE [cognitive-services-text-analytics-signup-requirements](../../../../includes/cognitive-services-text-analytics-signup-requirements.md)]

* De [eindpunt en de toegangssleutel](../How-tos/text-analytics-how-to-access-key.md) die voor u is gegenereerd tijdens de registratie.

* De volgende invoer, abonnementssleutel, en `text_analytics_base_url` worden gebruikt voor alle snelstartgidsen hieronder. Voeg de import toe.

    ```python
    import requests
    # pprint is pretty print (formats the JSON)
    from pprint import pprint
    from IPython.display import HTML
    ```
    
    Voeg deze regels toe en vervang `subscription_key` met een geldig abonnement-sleutel die u eerder hebt verkregen.
    
    ```python
    subscription_key = '<ADD KEY HERE>'
    assert subscription_key
    ```
    
    Vervolgens voegt u deze regel toe en vervolgens controleren of de regio in `text_analytics_base_url` komt overeen met de versie die u hebt gebruikt bij het instellen van de service. Als u een gratis proefversie sleutel gebruikt, moet u niet van belang.
    
    ```python
    text_analytics_base_url = "https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.1/"
    ```

<a name="Detect"></a>

## <a name="detect-languages"></a>Talen detecteren

Met de Language Detection-API wordt de taal van een tekstdocument gedetecteerd met behulp van de [methode Detect Language](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c7). Het service-eindpunt van de taaldetectie-API voor uw regio is beschikbaar via de volgende URL:

```python
language_api_url = text_analytics_base_url + "languages"
print(language_api_url)
```

    https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.1/languages


De payload voor de API bestaat uit een lijst met `documents`, waarvan elk een `id`- en een `text`-kenmerk bevat. Het `text`-kenmerk bevat de tekst die moet worden geanalyseerd. 

Vervang de `documents`-woordenlijst door andere tekst voor taaldetectie.

```python
documents = { 'documents': [
    { 'id': '1', 'text': 'This is a document written in English.' },
    { 'id': '2', 'text': 'Este es un document escrito en Español.' },
    { 'id': '3', 'text': '这是一个用中文写的文件' }
]}
```

De volgende paar regels code roepen de taaldetectie-API aan met behulp van de `requests`-bibliotheek in Python om de taal in de documenten te bepalen.

```python
headers   = {"Ocp-Apim-Subscription-Key": subscription_key}
response  = requests.post(language_api_url, headers=headers, json=documents)
languages = response.json()
pprint(languages)
```

Met de volgende regels code worden de JSON-gegevens als een HTML-tabel weergegeven.

```python
table = []
for document in languages["documents"]:
    text  = next(filter(lambda d: d["id"] == document["id"], documents["documents"]))["text"]
    langs = ", ".join(["{0}({1})".format(lang["name"], lang["score"]) for lang in document["detectedLanguages"]])
    table.append("<tr><td>{0}</td><td>{1}</td>".format(text, langs))
HTML("<table><tr><th>Text</th><th>Detected languages(scores)</th></tr>{0}</table>".format("\n".join(table)))
```

Geslaagde JSON-antwoord:

```json
    {'documents': [{'detectedLanguages': [{'iso6391Name': 'en',
                                           'name': 'English',
                                           'score': 1.0}],
                    'id': '1'},
                   {'detectedLanguages': [{'iso6391Name': 'es',
                                           'name': 'Spanish',
                                           'score': 1.0}],
                    'id': '2'},
                   {'detectedLanguages': [{'iso6391Name': 'zh_chs',
                                           'name': 'Chinese_Simplified',
                                           'score': 1.0}],
                    'id': '3'}],
     'errors': []}
```

<a name="SentimentAnalysis"></a>

## <a name="analyze-sentiment"></a>Stemming analyseren

De analyse-Gevoels-API detecteert het sentiment (bereik tussen positief of negatief) van een set tekstrecords, met behulp van de [Sentiment methode](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c9). In het volgende voorbeeld worden twee documenten beoordeeld, één in het Engels en één in het Spaans.

Het service-eindpunt voor sentimentanalyse is voor uw regio beschikbaar via de volgende URL:

```python
sentiment_api_url = text_analytics_base_url + "sentiment"
print(sentiment_api_url)
```

    https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.1/sentiment

Net zoals in het voorbeeld van taaldetectie wordt de service voorzien van een woordenlijst met een `documents`-sleutel die uit een lijst met documenten bestaat. Elk document is een tuple die bestaat uit de `id`, de te analyseren `text` en de `language` van de tekst. U kunt de taaldetectie-API uit de vorige sectie gebruiken om dit veld in te vullen.

```python
documents = {'documents' : [
  {'id': '1', 'language': 'en', 'text': 'I had a wonderful experience! The rooms were wonderful and the staff was helpful.'},
  {'id': '2', 'language': 'en', 'text': 'I had a terrible time at the hotel. The staff was rude and the food was awful.'},  
  {'id': '3', 'language': 'es', 'text': 'Los caminos que llevan hasta Monte Rainier son espectaculares y hermosos.'},  
  {'id': '4', 'language': 'es', 'text': 'La carretera estaba atascada. Había mucho tráfico el día de ayer.'}
]}
```

De sentiment-API kan nu worden gebruikt om de documenten te analyseren op de daarin opgenomen sentimenten.

```python
headers   = {"Ocp-Apim-Subscription-Key": subscription_key}
response  = requests.post(sentiment_api_url, headers=headers, json=documents)
sentiments = response.json()
pprint(sentiments)
```

Geslaagde JSON-antwoord:

```json
{'documents': [{'id': '1', 'score': 0.7673527002334595},
                {'id': '2', 'score': 0.18574094772338867},
                {'id': '3', 'score': 0.5}],
    'errors': []}
```

De gevoelsscore voor een document is tussen 0,0 en 1,0, met een hogere score die wijzen op een positiever gevoel.

<a name="KeyPhraseExtraction"></a>

## <a name="extract-key-phrases"></a>Belangrijke woordgroepen herkennen

Met de Key Phrase Extraction-API worden sleuteltermen opgehaald uit een tekstdocument met behulp van de [methode Key Phrases](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c6). In dit gedeelte van het scenario worden sleuteltermen opgehaald voor zowel de Engelse als Spaanse documenten.

Het service-eindpunt voor de service voor het ophalen van sleuteltermen is toegankelijk via de volgende URL:

```python
key_phrase_api_url = text_analytics_base_url + "keyPhrases"
print(key_phrase_api_url)
```

    https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.1/keyPhrases

De verzameling documenten is dezelfde als die voor sentimentanalyse werd gebruikt.

```python
documents = {'documents' : [
  {'id': '1', 'language': 'en', 'text': 'I had a wonderful experience! The rooms were wonderful and the staff was helpful.'},
  {'id': '2', 'language': 'en', 'text': 'I had a terrible time at the hotel. The staff was rude and the food was awful.'},  
  {'id': '3', 'language': 'es', 'text': 'Los caminos que llevan hasta Monte Rainier son espectaculares y hermosos.'},  
  {'id': '4', 'language': 'es', 'text': 'La carretera estaba atascada. Había mucho tráfico el día de ayer.'}
]}
```

Het JSON-object kan worden gerenderd als een HTML-tabel met behulp van de volgende regels code:

```python
table = []
for document in key_phrases["documents"]:
    text    = next(filter(lambda d: d["id"] == document["id"], documents["documents"]))["text"]    
    phrases = ",".join(document["keyPhrases"])
    table.append("<tr><td>{0}</td><td>{1}</td>".format(text, phrases))
HTML("<table><tr><th>Text</th><th>Key phrases</th></tr>{0}</table>".format("\n".join(table)))
```

De volgende paar regels code roepen de taaldetectie-API aan met behulp van de `requests`-bibliotheek in Python om de taal in de documenten te bepalen.
```python
headers   = {'Ocp-Apim-Subscription-Key': subscription_key}
response  = requests.post(key_phrase_api_url, headers=headers, json=documents)
key_phrases = response.json()
pprint(key_phrases)
```

Geslaagde JSON-antwoord:
```json
{'documents': [
    {'keyPhrases': ['wonderful experience', 'staff', 'rooms'], 'id': '1'},
    {'keyPhrases': ['food', 'terrible time', 'hotel', 'staff'], 'id': '2'},
    {'keyPhrases': ['Monte Rainier', 'caminos'], 'id': '3'},
    {'keyPhrases': ['carretera', 'tráfico', 'día'], 'id': '4'}],
    'errors': []
}
```

## <a name="identify-entities"></a>Entiteiten identificeren

De Entities-API identificeert bekende entiteiten in een tekstdocument, met behulp van de [methode Entities](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/5ac4251d5b4ccd1554da7634). In het volgende voorbeeld worden entiteiten geïdentificeerd voor Engelse documenten.

Het service-eindpunt voor de service voor entiteitskoppeling is toegankelijk via de volgende URL:

```python
entity_linking_api_url = text_analytics_base_url + "entities"
print(entity_linking_api_url)
```

    https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.1/entities

De verzameling documenten staat hieronder:

```python
documents = {'documents' : [
  {'id': '1', 'text': 'Microsoft is an It company.'}
]}
```
De documenten kunnen nu worden verzonden naar de Text Analytics-API om het antwoord te ontvangen.

```python
headers   = {"Ocp-Apim-Subscription-Key": subscription_key}
response  = requests.post(entity_linking_api_url, headers=headers, json=documents)
entities = response.json()
```

Geslaagde JSON-antwoord:
```json
{  
   "documents":[  
      {  
         "id":"1",
         "entities":[  
            {  
               "name":"Microsoft",
               "matches":[  
                  {  
                     "wikipediaScore":0.20872054383103444,
                     "entityTypeScore":0.99996185302734375,
                     "text":"Microsoft",
                     "offset":0,
                     "length":9
                  }
               ],
               "wikipediaLanguage":"en",
               "wikipediaId":"Microsoft",
               "wikipediaUrl":"https://en.wikipedia.org/wiki/Microsoft",
               "bingId":"a093e9b9-90f5-a3d5-c4b8-5855e1b01f85",
               "type":"Organization"
            },
            {  
               "name":"Technology company",
               "matches":[  
                  {  
                     "wikipediaScore":0.82123868042800585,
                     "text":"It company",
                     "offset":16,
                     "length":10
                  }
               ],
               "wikipediaLanguage":"en",
               "wikipediaId":"Technology company",
               "wikipediaUrl":"https://en.wikipedia.org/wiki/Technology_company",
               "bingId":"bc30426e-22ae-7a35-f24b-454722a47d8f"
            }
         ]
      }
   ],
    "errors":[]
}
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Text Analytics met Power BI](../tutorials/tutorial-power-bi-key-phrases.md)

## <a name="see-also"></a>Zie ook 

 [Overzicht van Text Analytics](../overview.md)  
 [Veelgestelde vragen](../text-analytics-resource-faq.md)
