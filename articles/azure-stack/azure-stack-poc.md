---
title: Wat is Azure Stack? | Microsoft Docs
description: Via Azure Stack kunt u Azure-services uitvoeren in uw datacenter.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: d9e6aee1-4cba-4df5-b5a3-6f38da9627a3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 02/25/2019
ms.author: jeffgilb
ms.reviewer: unknown
ms.custom: mvc
ms.lastreviewed: 02/25/2019
ms.openlocfilehash: 65894ccd9514bce1d429b336f8bd5e6674048e65
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56820260"
---
# <a name="what-is-azure-stack"></a>Wat is Azure Stack?

Microsoft Azure Stack is een hybride cloudplatform waarmee u Azure-services kunt leveren in uw datacenter. Dit platform is ontworpen ter ondersteuning van uw groeiende zakelijke vereisten. Met Azure Stack kunt u nieuwe scenario's voor uw moderne toepassingen mogelijk maken, zoals rand- en niet-verbonden omgevingen, en voldoen aan specifieke vereisten ten aanzien van beveiliging en naleving.

Azure Stack wordt geleverd in twee implementatievarianten voor verschillende behoeften.


## <a name="azure-stack-development-kit"></a>Azure Stack Development Kit

De Microsoft [Azure Stack Development Kit (ASDK)](./asdk/asdk-what-is.md) is een Azure Stack-implementatie met één knooppunt die u kunt gebruiken om Azure Stack te evalueren en beter te leren kennen.  U kunt ook de ASDK als een ontwikkelomgeving instellen voor het bouwen van apps met behulp van de API's en hulpprogramma's die consistent is met Azure gebruiken.

>[!Note]
>De ASDK is niet bedoeld voor gebruik als productieomgeving.

De ASDK heeft de volgende beperkingen:

* De ASDK is gekoppeld aan een enkele Azure Active Directory (Azure AD) of Active Directory Federation Services (AD FS)-id-provider. U kunt in deze directory meerdere gebruikers maken en aan elke gebruiker abonnementen toewijzen.
* Omdat Azure Stack-onderdelen zijn geïmplementeerd op een afzonderlijke host-computer, zijn er beperkte fysieke resources beschikbaar voor tenantresources. Deze configuratie is niet bedoeld om te schalen of voor evaluatie van de prestaties.
* Het aantal netwerkscenario's is beperkt vanwege de vereisten met betrekking tot NIC-implementatie en gebruik van één host.

## <a name="azure-stack-integrated-systems"></a>Geïntegreerde Azure Stack-systemen
Geïntegreerde Azure Stack-systemen worden aangeboden op basis van een samenwerking tussen Microsoft en diverse [hardwarepartners](https://azure.microsoft.com/overview/azure-stack/integrated-systems/). Dit staat garant voor een oplossing die de innovatieve mogelijkheden van de cloud combineert met eenvoudig rekenbeheer. Omdat Azure Stack wordt geleverd als een geïntegreerd hardware- en softwaresysteem, beschikt u over alle flexibiliteit en controle die u nodig hebt, terwijl u tegelijkertijd vanuit de cloud kunt innoveren. Azure Stack geïntegreerde systemen in grootte variëren van 4-16 knooppunten en gezamenlijk worden ondersteund door de hardware-partner en Microsoft.  Geïntegreerde Azure Stack-systemen zijn geknipt voor het maken van nieuwe scenario's en voor het implementeren van nieuwe oplossingen voor uw productieworkloads.

## <a name="next-steps"></a>Volgende stappen

[Belangrijke functies en concepten](azure-stack-key-features.md)

[Azure Stack: Een uitbreiding van Azure (PDF-bestand)](https://azure.microsoft.com/resources/azure-stack-an-extension-of-azure/)
