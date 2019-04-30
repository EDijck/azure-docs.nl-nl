---
title: Automatische selectie van ML-algoritme en afstemmen
titleSuffix: Azure Machine Learning service
description: Meer informatie over Azure Machine Learning-service kan automatisch een algoritme kiezen voor u, en genereert een model op basis van dat u tijd besparen met behulp van de parameters en de criteria die u opgeeft op de beste algoritme voor het model te selecteren.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: nacharya1
ms.author: nilesha
ms.date: 12/12/2018
ms.custom: seodec18
ms.openlocfilehash: 620dbd22613df37fdc3c20e34906684446b2251f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60821174"
---
# <a name="what-is-automated-machine-learning"></a>Wat is geautomatiseerde machine learning?

Geautomatiseerde machine learning is het proces van het trainen van gegevens met de functie van een gedefinieerd doel nemen en combinaties van algoritmen en functie van selecties automatisch het beste model selecteren voor uw gegevens op basis van de scores training doorlopen. De traditionele machine learning-model ontwikkelingsproces is zeer resource-intensieve en aanzienlijke domein kennis en het tijdstip investering uitvoeren en vergelijk de resultaten van tientallen modellen vereist. Geautomatiseerde machine learning vereenvoudigt u dit proces door het genereren van modellen die zijn afgestemd op van de doelstellingen en beperkingen die u hebt gedefinieerd voor uw experiment, zoals de tijd voor het experiment dat moet worden uitgevoerd of die op de zwarte-modellen.

## <a name="how-it-works"></a>Hoe werkt het?

1. Configureren van het type machine learning probleem dat u probeert op te lossen. Categorieën van leren met supervisie worden ondersteund:
   + Classificatie
   + Regressie
   + Prognose

   Terwijl geautomatiseerde machine learning algemeen beschikbaar is, **met de prognosefunctie is nog steeds in openbare preview.**

   Zie de [lijst met modellen](how-to-configure-auto-train.md#select-your-experiment-type) Azure Machine Learning kunt proberen bij het trainen van.

1. U geeft de bron en de indeling voor het trainen van gegevens. De gegevens moeten worden gemarkeerd en kunnen worden opgeslagen op uw ontwikkelomgeving of in Azure Blob Storage. Als de gegevens worden opgeslagen op uw ontwikkelomgeving, moet deze zich in dezelfde map als uw trainingsscripts. Deze map wordt gekopieerd naar de compute-doel dat u voor de training selecteert.

    In uw trainingsscript, kunnen de gegevens worden gelezen in Numpy matrices of een Pandas dataframe.

    Kunt u Splitsingsopties voor het selecteren van training en validatie gegevens configureren of kunt u afzonderlijke trainings- en validatie van gegevenssets.

1. Configureer de [compute-doel](how-to-set-up-training-targets.md) die wordt gebruikt voor het model te trainen.

1. Configureer de configuratie van de geautomatiseerde machine learning. Hiermee bepaalt u de parameters die worden gebruikt als Azure Machine Learning over verschillende modellen, hyperparameter instellingen doorloopt, en het bepalen van welke metrische gegevens om te kijken wanneer het beste model

1. Dien een training uitvoeren.

Tijdens de training maakt de Azure Machine Learning-service een aantal pijplijnen die verschillende algoritmen en parameters probeert. Deze wordt gestopt zodra deze treffers in de limiet voor iteraties die u opgeven of wanneer het de doelwaarde voor de metrische gegevens die u opgeeft is bereikt.

[![Geautomatiseerde Machine learning](./media/how-to-automated-ml/automated-machine-learning.png)](./media/how-to-automated-ml/automated-machine-learning.png#lightbox)

U kunt de run logboekgegevens, waarin de metrische gegevens die zijn verzameld tijdens de uitvoering inspecteren. De uitvoering van de training levert ook een Python-geserialiseerd object (`.pkl` bestand) dat het model en de voorverwerking van de gegevens bevat.


## <a name="next-steps"></a>Volgende stappen

Zie de voorbeelden en informatie over het bouwen van modellen met behulp van geautomatiseerde Machine Learning:

+ [Zelfstudie: Automatisch een classificatie model trainen met Azure geautomatiseerde Machine Learning](tutorial-auto-train-models.md)

+ [Laptop-voorbeelden](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/)

+ [Gebruik automatische training op een externe bron](how-to-auto-train-remote.md)

+ [Instellingen configureren voor automatische training](how-to-configure-auto-train.md)
