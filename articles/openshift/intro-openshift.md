---
title: Inleiding tot Azure Red Hat OpenShift | Microsoft Docs
description: Informatie over de functies en voordelen van Microsoft Azure Red Hat OpenShift op containers gebaseerde toepassingen implementeren en beheren.
services: container-service
author: tylermsft
ms.author: twhitney
ms.service: container-service
manager: jeconnoc
ms.topic: overview
ms.date: 05/06/2019
ms.custom: mvc
ms.openlocfilehash: 6121c0f654a61a147e84f0697f3ddb06b7c5db92
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65081043"
---
# <a name="azure-red-hat-openshift"></a>Azure Red Hat OpenShift

De Microsoft *Azure Red Hat OpenShift* service kunt u implementeren volledig beheerde [OpenShift](https://www.openshift.com/) clusters.

Azure Red Hat OpenShift breidt [Kubernetes](https://kubernetes.io/). Uitvoeren van containers in productie met Kubernetes vereist extra hulpprogramma's en resources, zoals een installatiekopieregister, beheer van opslag, netwerken oplossingen en logboekregistratie en controlehulpprogramma's, die allemaal moeten worden samengesteld en geteste samen. Het bouwen van toepassingen op basis van een container, moet nog meer integratie in combinatie met middleware, -frameworks, databases en CI/CD-hulpprogramma's. Azure Red Hat OpenShift combineert alle dit in één platform, dat het gemak van bewerkingen voor IT-teams tijdens het doorgeven van toepassing teams wat ze nodig hebben om uit te voeren.

Azure Red Hat OpenShift is gezamenlijk gebouwd, beheerd en ondersteund door Red Hat en Microsoft voor een geïntegreerde ondersteuningservaring. Er zijn geen virtuele machines om te werken en er zijn geen patches is vereist. Master, infrastructuur en toepassing knooppunten zijn gevuld, bijgewerkt en door Red Hat en Microsoft namens u bewaakt. Uw Azure Red Hat OpenShift-clusters worden geïmplementeerd in uw Azure-abonnement en worden opgenomen in uw Azure-factuur.

U kunt uw eigen register, netwerken, opslag, en CI/CD-oplossingen, of gebruik de ingebouwde oplossingen voor geautomatiseerde source code management, de container en de toepassing bouwt, implementaties, schalen, statusbeheer en meer. Azure Red Hat OpenShift biedt een geïntegreerde aanmelding via Azure Active Directory.

Voltooien om te beginnen, de [maken van een cluster Azure Red Hat OpenShift](tutorial-create-cluster.md) zelfstudie.

## <a name="access-security-and-monitoring"></a>Toegang, beveiliging en bewaking

Voor betere beveiliging en beheer kunt Azure Red Hat OpenShift integreren met Azure Active Directory (Azure AD) en het gebruik van Kubernetes-op rollen gebaseerd toegangsbeheer (RBAC). U kunt ook de status van uw cluster en resources bewaken.

## <a name="cluster-and-node"></a>Cluster en knooppunt

Azure Red Hat OpenShift-knooppunten worden uitgevoerd op Azure virtual machines. U kunt opslag met knooppunten en pods verbinden, clusteronderdelen bijwerken en GPU's gebruiken.

## <a name="virtual-networks-and-ingress"></a>Virtual Networks en inkomend verkeer

U kunt een Azure Red Hat OpenShift-cluster in een bestaand virtueel netwerk kunt implementeren. In deze configuratie elke pod in het cluster een IP-adres in het virtuele netwerk is toegewezen en kan rechtstreeks communiceren met andere schillen in het cluster en andere knooppunten in het virtuele netwerk. Schillen kunnen ook verbinding maken met andere services in een gekoppeld virtueel netwerk, en on-premises netwerken via [ExpressRoute](https://docs.microsoft.com/azure/expressroute/) of site-naar-site (S2S) VPN-verbindingen.

Zie voor meer informatie, [maken van een cluster Microsoft Red Hat OpenShift op Azure](tutorial-create-cluster.md).

## <a name="kubernetes-certification"></a>Kubernetes-certificering

Azure Red Hat OpenShift-service is CNCF gecertificeerd als Kubernetes in overeenstemming zijn.

## <a name="next-steps"></a>Volgende stappen

Informatie over de vereisten voor Azure Red Hat OpenShift:

> [!div class="nextstepaction"]
> [Uw ontwikkelaarsomgeving instellen](howto-setup-environment.md)