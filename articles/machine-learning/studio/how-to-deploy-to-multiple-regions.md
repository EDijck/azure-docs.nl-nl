---
Titel: Studio-webservice implementeert in meerdere regio's titleSuffix: Azure Machine Learning Studio description: Stappen voor het implementeren (kopie) een nieuwe webservice in andere regio's. Eenvoudig een webservice implementeert in meerdere regio's zonder meerdere abonnementen of werkruimten.
Services: machine learning ms.service: machine learning ms.component: studio ms.topic: artikel

Auteur: ericlicoding ms.author: amlstudiodocs ms.custom: seodec18 ms.date: 04/19/2017
---
# <a name="deploy-an-azure-machine-learning-studio-web-service-to-multiple-regions"></a>Een Azure Machine Learning Studio-webservice implementeren in meerdere regio 's

De nieuwe Azure-Web-Services kunt u eenvoudig een Azure Machine Learning Studio-webservice implementeren in meerdere regio's zonder meerdere abonnementen of werkruimten. 

Prijzen zijn regiospecifiek, dat daarom moet u een abonnement voor elke regio waarin u de webservice implementeren.

## <a name="to-create-a-plan-in-another-region"></a>Een plan maken in een andere regio
1. Meld u aan bij [Microsoft Azure Machine Learning-webservices](https://services.azureml.net/).
2. Klik op de **plannen** menu-optie.
3. Klik op de abonnementen via de weergavepagina **nieuw**.
4. Uit de **abonnement** vervolgkeuzelijst, selecteer het abonnement waarin het nieuwe abonnement worden geplaatst.
5. Uit de **regio** vervolgkeuzelijst, selecteer een regio voor het nieuwe abonnement. De opties plannen voor de geselecteerde regio wordt weergegeven in de **opties plannen** sectie van de pagina.
6. Uit de **resourcegroep** vervolgkeuzelijst, selecteer een resourcegroep voor het abonnement. Zie voor meer informatie over resourcegroepen [overzicht van Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).
7. In **Plannaam** typt u de naam van het plan.
8. Onder **abonnementsopties**, klikt u op het niveau van de facturering voor het nieuwe abonnement.
9. Klik op **Create**.

## <a name="deploying-the-web-service-to-another-region"></a>De webservice implementeert naar een andere regio
1. Klik op de **webservices** menu-optie.
2. Selecteer de Web-Service die u naar een nieuwe regio implementeert.
3. Klik op **kopie**.
4. In **naam van de webservice**, typt u een nieuwe naam voor de webservice.
5. In **Web servicebeschrijving**, typ een beschrijving voor de webservice.
6. Uit de **abonnement** vervolgkeuzelijst, selecteer het abonnement waarin de nieuwe webservice zich bevindt.
7. Uit de **resourcegroep** vervolgkeuzelijst, selecteer een resourcegroep voor de webservice. Zie voor meer informatie over resourcegroepen [overzicht van Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).
8. Uit de **regio** vervolgkeuzelijst, selecteer de regio waarin u kunt de webservice implementeren.
9. Uit de **opslagaccount** vervolgkeuzelijst, selecteer een storage-account waarin u voor het opslaan van de webservice.
10. Uit de **Prijsplan** vervolgkeuzelijst, selecteer een abonnement in de regio die u hebt geselecteerd in stap 8.
11. Klik op **kopie**.

