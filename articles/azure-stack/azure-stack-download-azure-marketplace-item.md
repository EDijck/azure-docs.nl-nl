---
title: Marketplace-items downloaden van Azure | Microsoft Docs
description: De cloud-operator kunt marketplace-items downloaden van Azure aan mijn Azure Stack-implementatie.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/10/2018
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 12/10/2018
ms.openlocfilehash: 465f9d51f724bf1c8713259cd1d0b6ebabc230fe
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55239754"
---
# <a name="download-marketplace-items-from-azure-to-azure-stack"></a>Marketplace-items van Azure naar Azure Stack downloaden

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Als een cloud-operator, kunt u items downloaden van de Azure Marketplace en stel ze beschikbaar in Azure Stack. Er zijn de items die u kunt kiezen uit een gecureerde lijst van de Azure Marketplace-items die zijn getest en worden ondersteund als u wilt werken met Azure Stack. Aanvullende items worden regelmatig toegevoegd aan deze lijst, dus kom later terug voor nieuwe inhoud blijven. 

Er zijn twee scenario's voor het verbinden met de Azure Marketplace: 

- **Een verbonden scenario** -die uw Azure Stack-omgeving moet worden verbonden met internet vereist. U kunt de Azure Stack-portal gebruiken om te zoeken en downloaden van items. 
- **Een verbinding is verbroken of gedeeltelijk verbonden scenario** -waarvoor u toegang tot het Internet met behulp van de marketplace syndication-hulpprogramma voor het downloaden van items voor de marketplace is vereist. Vervolgens brengt u de downloads naar uw niet-verbonden Azure Stack-installatie. In dit scenario maakt gebruik van PowerShell.

Zie [Azure Marketplace-items voor Azure Stack](azure-stack-marketplace-azure-items.md) voor een lijst van de marketplace-items die u kunt downloaden.


## <a name="connected-scenario"></a>Verbonden scenario
Als Azure Stack verbinding met internet maakt, kunt u de beheerportal kunt gebruiken voor het downloaden van items voor de marketplace.

### <a name="prerequisites"></a>Vereisten
Uw Azure Stack-implementatie moet over een internetverbinding beschikt, en worden [geregistreerd bij Azure](azure-stack-register.md).

### <a name="use-the-portal-to-download-marketplace-items"></a>Marketplace-items downloaden via de portal  
1. Aanmelden bij de Azure Stack-beheerdersportal.

2.  Bekijk de beschikbare opslagruimte voor het downloaden van items voor de marketplace. Wanneer u items downloaden selecteert, kunt u later de downloadgrootte vergelijken met de capaciteit van de beschikbare opslag. Als beperkte capaciteit heeft, kunt u de opties voor [beheer van beschikbare ruimte](azure-stack-manage-storage-shares.md#manage-available-space). 

    Om te controleren van beschikbare ruimte **regiobeheer** selecteert u de regio die u wilt verkennen en ga vervolgens naar **Resourceproviders** > **opslag**.

    [ ![Bekijk opslagruimte](media/azure-stack-download-azure-marketplace-item/storagesm.png "opslagruimte controleren") ](media/azure-stack-download-azure-marketplace-item/storage.png#lightbox)

    
3. Open de Azure Stack Marketplace en verbinding maken met Azure. Om dit te doen, selecteert u **Marketplace management**, en selecteer vervolgens **toevoegen vanuit Azure**.

    [ ![Toevoegen van Azure](media/azure-stack-download-azure-marketplace-item/marketplacesm.png "toevoegen van Azure") ](media/azure-stack-download-azure-marketplace-item/marketplace.png#lightbox)

    De portal wordt weergegeven de lijst met items die beschikbaar zijn voor het downloaden vanuit de Azure Marketplace. U kunt klikken op elk item om de beschrijving en aanvullende informatie over het, onder andere de downloadgrootte weer te geven. 

    [ ![Lijst met Marketplace](media/azure-stack-download-azure-marketplace-item/image03sm.png "Marketplace-lijst") ](media/azure-stack-download-azure-marketplace-item/image03.png#lightbox)

4. Selecteer het item dat u wilt en selecteer vervolgens **downloaden**. Downloadtijden variëren.

    [ ![Bericht downloaden](media/azure-stack-download-azure-marketplace-item/image04.png "downloaden van bericht") ](media/azure-stack-download-azure-marketplace-item/image04.png#lightbox)

    Nadat het downloaden is voltooid, kunt u de nieuwe marketplace-item als een Azure Stack-operators of een gebruiker kunt implementeren.

5. Voor het implementeren van het gedownloade item, selecteer **+ een resource maken**, en zoekt u tussen de categorieën voor het nieuwe marketplace-item. Vervolgens selecteert u het item om te beginnen met het implementatieproces. Het proces varieert voor verschillende marketplace-items. 

## <a name="disconnected-or-a-partially-connected-scenario"></a>De verbinding verbroken of een gedeeltelijk verbonden scenario

Als Azure Stack in een niet-verbonden modus en zonder verbinding met internet heeft is, gebruikt u PowerShell en de *marketplace syndication hulpprogramma* voor het downloaden van de marketplace-items aan een machine met een internetverbinding. Vervolgens brengt u de items aan uw Azure Stack-omgeving. In een omgeving zonder verbinding, kunt u items voor de marketplace niet downloaden met behulp van de Azure Stack-portal. 

Het hulpprogramma voor marketplace-syndicatie kan ook worden gebruikt in een verbonden scenario. 

Er zijn twee onderdelen voor dit scenario:
- **Deel 1:** Downloaden van Azure Marketplace. Op de computer met internettoegang PowerShell configureren, downloadt u het hulpprogramma syndication en download vervolgens items formulier, de Azure Marketplace.  
- **Deel 2:** Uploaden en publiceren naar de Azure Stack Marketplace. U verplaatst u de bestanden die u hebt gedownload naar uw Azure Stack-omgeving, importeer deze in Azure Stack en ze vervolgens te publiceren op Azure Stack Marketplace.  


### <a name="prerequisites"></a>Vereisten
- Uw Azure Stack-implementatie moet [geregistreerd bij Azure](azure-stack-register.md).  

- De computer die verbinding heeft met internet moet hebben **PowerShell-Module voor Azure Stack versie 1.2.11** of hoger. Als deze niet al aanwezig, [Azure Stack-specifieke PowerShell-modules installeren](azure-stack-powershell-install.md).  

- Om in te schakelen van het importeren van een gedownloade marketplace-item, de [PowerShell-omgeving voor de Azure Stack-operator](azure-stack-powershell-configure-admin.md) moet worden geconfigureerd.  

- Hebt u een [opslagaccount](azure-stack-manage-storage-accounts.md) in Azure Stack waaraan een openbaar toegankelijke container (dit is een blob storage). U kunt de container gebruikt als tijdelijke opslag voor de marketplace-items galeriebestanden. Als u niet bekend met storage-accounts en containers bent, Zie [werken met blobs - Azure-portal](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal) in de documentatie van Azure.

- De marketplace-syndicatie tool wordt gedownload tijdens de eerste procedure. 

- U kunt installeren [AzCopy](../storage/common/storage-use-azcopy.md) voor downloaden van optimale prestaties, maar dit is niet vereist.

### <a name="use-the-marketplace-syndication-tool-to-download-marketplace-items"></a>Gebruik het hulpprogramma marketplace syndication naar marketplace-items downloaden

1. Open een PowerShell-console als beheerder op een computer met een internetverbinding.

2. De Azure-account dat u hebt gebruikt voor het registreren van Azure Stack toevoegen. Het account toevoegen in PowerShell uitvoeren `Add-AzureRmAccount` zonder parameters. U wordt gevraagd de referenties van uw Azure-account in te voeren en u mogelijk gebruikmaken van 2-factor-verificatie op basis van de configuratie van uw account.

3. Als u meerdere abonnementen hebt, voert u de volgende opdracht uit om het abonnement dat u hebt gebruikt voor de registratie te selecteren:  

   ```PowerShell  
   Get-AzureRmSubscription -SubscriptionID '<Your Azure Subscription GUID>' | Select-AzureRmSubscription
   $AzureContext = Get-AzureRmContext
   ```

4. Download de nieuwste versie van het hulpprogramma voor marketplace-syndicatie met behulp van het volgende script:  

   ```PowerShell
   # Download the tools archive.
   [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12 
   invoke-webrequest https://github.com/Azure/AzureStack-Tools/archive/master.zip `
     -OutFile master.zip

   # Expand the downloaded files.
   expand-archive master.zip `
     -DestinationPath . `
     -Force

   # Change to the tools directory.
   cd .\AzureStack-Tools-master

   ```

5. Importeer de module syndicatie en start vervolgens het hulpprogramma door het uitvoeren van de volgende opdrachten. Vervang `Destination folder path` met een locatie voor het opslaan van de bestanden die u vanuit Azure Marketplace downloadt.   

   ```PowerShell  
   Import-Module .\Syndication\AzureStack.MarketplaceSyndication.psm1

   Export-AzSOfflineMarketplaceItem -Destination "Destination folder path in quotes" 
   ```

6. Wanneer het hulpprogramma wordt uitgevoerd, ziet u een scherm dat vergelijkbaar is met de volgende afbeelding, met de lijst met beschikbare marketplace-items:

   [ ![Azure Marketplace-items pop](media/azure-stack-download-azure-marketplace-item/image05.png "items voor de Azure Marketplace") ](media/azure-stack-download-azure-marketplace-item/image05.png#lightbox)

7. Selecteer het item dat u wilt downloaden en noteer de *versie*. U houdt de *Ctrl* sleutel te selecteren van meerdere installatiekopieën. U verwijst naar de *versie* wanneer het importeren van het item in de volgende procedure. 
   
   U kunt ook de lijst met installatiekopieën filteren met behulp van de **criteria toevoegen** optie.

8. Selecteer **OK**, en controleer en accepteer de juridische bepalingen. 

9. De tijd die het downloaden duurt, is afhankelijk van de grootte van het item. Nadat het downloaden is voltooid, is het item is beschikbaar in de map die u hebt opgegeven in het script. De download bevat onder andere een VHD-bestand (voor virtuele machines) of een ZIP-bestand (voor extensies van virtuele machines). Een galerijpakket in deze ook advies inwinnen de *.azpkg* -indeling, dit gewoon een zipbestand is.

10. Als het downloaden is mislukt, kunt u het opnieuw proberen door de volgende PowerShell-cmdlet opnieuw uit te voeren:

    ```powershell
    Export-AzSOfflineMarketplaceItem -Destination "Destination folder path in quotes”
    ```

    Voordat opnieuw wordt geprobeerd, verwijdert u de hoofdmap van het product waarin de download is mislukt. Bijvoorbeeld, als het downloadscript is mislukt bij het downloaden te `D:\downloadFolder\microsoft.customscriptextension-arm-1.9.1`, verwijder de `D:\downloadFolder\microsoft.customscriptextension-arm-1.9.1` map en voer de cmdlet.
 
### <a name="import-the-download-and-publish-to-azure-stack-marketplace-1811-and-higher"></a>Het downloaden van het importeren en publiceren op Azure Stack Marketplace (1811 en hoger)

1. U moet de bestanden die u hebt verplaatst [eerder gedownloade](#use-the-marketplace-syndication-tool-to-download-marketplace-items) lokaal zodat ze beschikbaar voor uw Azure Stack-omgeving zijn. Het hulpprogramma voor marketplace-syndicatie moet ook zijn beschikbaar voor uw Azure Stack-omgeving, omdat u moet het hulpprogramma gebruiken om uit te voeren van de importbewerking.

   De volgende afbeelding toont een voorbeeld van een map structuur. `D:\downloadfolder` bevat alle gedownloade marketplace-items. Elke submap is een marketplace-item (bijvoorbeeld `microsoft.custom-script-linux-arm-2.0.3`) met de naam van de product-ID. Binnen elke submap is van de marketplace-item gedownloade inhoud.

   [ ![Mapstructuur van Marketplace downloaden](media/azure-stack-download-azure-marketplace-item/mp1sm.png "Marketplace downloaden van directory-structuur") ](media/azure-stack-download-azure-marketplace-item/mp1.png#lightbox)

2. Volg de instructies in [in dit artikel](azure-stack-powershell-configure-admin.md) het configureren van de Azure Stack-Operator PowerShell-sessie. 

3. Importeer de module syndicatie en start vervolgens het hulpprogramma voor marketplace-syndicatie het volgende script uit te voeren:

   ```PowerShell
   $credential = Get-Credential -Message "Enter the azure stack operator credential:"
   Import-AzSOfflineMarketplaceItem -origin "marketplace content folder" -armendpoint "Environment Arm Endpoint" -AzsCredential $credential
   ```
   De `-AzsCredential` parameter is optioneel. Het wordt gebruikt voor het vernieuwen van het toegangstoken als deze is verlopen. Als de `-AzsCredential` parameter niet wordt opgegeven en het token is verlopen, wordt u gevraagd de referenties van de operator in te voeren.

4. Nadat het script is voltooid, het item moet zijn beschikbaar in de Azure Stack Marketplace.

### <a name="import-the-download-and-publish-to-azure-stack-marketplace-1809-and-lower"></a>Het downloaden van het importeren en publiceren op Azure Stack Marketplace (1809 en lager)

1. De bestanden voor installatiekopieën van virtuele machines of sjablonen voor oplossingen die u hebt [eerder gedownloade](#use-the-marketplace-syndication-tool-to-download-marketplace-items) moeten lokaal beschikbaar zijn voor uw Azure Stack-omgeving worden gemaakt.  

2. De beheerportal de marketplace-item-pakket (de .azpkg-bestand) en de installatiekopie van virtuele harde schijf (.vhd-bestand) uploaden naar Azure Stack-Blob-opslag gebruiken. Het uploaden van het pakket en schijf-bestanden maakt ze beschikbaar voor Azure Stack, zodat u kunt het item later publiceren naar de Azure Stack Marketplace.

   Uploaden, moet u een opslagaccount met een openbaar toegankelijke container hebt (Zie de vereisten voor dit scenario).  
   1. In de Azure Stack-beheerportal, gaat u naar **alle services** en klik vervolgens onder de **gegevens en opslag** categorie, selecteer **opslagaccounts**.  
   
   2. Selecteer een opslagaccount van uw abonnement en klik vervolgens onder **BLOB-SERVICE**, selecteer **Containers**.  
      [ ![BLOB-service](media/azure-stack-download-azure-marketplace-item/blob-service.png "Blob-service") ](media/azure-stack-download-azure-marketplace-item/blob-service.png#lightbox)  
   
   3. Selecteer de container die u wilt gebruiken en selecteer vervolgens **uploaden** openen de **blob uploaden** deelvenster.  
      [ ![Container](media/azure-stack-download-azure-marketplace-item/container.png "Container") ](media/azure-stack-download-azure-marketplace-item/container.png#lightbox)  
   
   4. Blader in het deelvenster van de blob uploaden naar de pakket- en schijf-bestanden wilt laden in opslag en selecteer vervolgens **uploaden**: [ ![Uploaden](media/azure-stack-download-azure-marketplace-item/uploadsm.png "uploaden") ](media/azure-stack-download-azure-marketplace-item/upload.png#lightbox)  

   5. Bestanden die u uploadt, worden weergegeven in het deelvenster van de container. Selecteer een bestand en kopieer vervolgens de URL van de **Blob-eigenschappen** deelvenster. U gebruikt deze URL in de volgende stap bij het importeren van de marketplace-item in Azure Stack.  In de volgende afbeelding is de container is *blob-opslag-testen* en het bestand is *Microsoft.WindowsServer2016DatacenterServerCore ARM.1.0.801.azpkg*.  Het bestand dat de URL is *https://testblobstorage1.blob.local.azurestack.external/blob-test-storage/Microsoft.WindowsServer2016DatacenterServerCore-ARM.1.0.801.azpkg*.  
      [ ![BLOB-eigenschappen](media/azure-stack-download-azure-marketplace-item/blob-storagesm.png "Blob-eigenschappen") ](media/azure-stack-download-azure-marketplace-item/blob-storage.png#lightbox)  

3. De VHD-installatiekopie importeren naar Azure Stack met behulp van de **toevoegen AzsPlatformimage** cmdlet. Wanneer u deze cmdlet gebruikt, vervangen door de *publisher*, *bieden*, en andere parameterwaarden met de waarden van de installatiekopie die u wilt importeren. 

   U krijgt de *publisher*, *bieden*, en *sku* waarden van de installatiekopie van het tekstbestand dat wordt gedownload met het bestand AZPKG. Het tekstbestand dat is opgeslagen in de doellocatie. De *versie* waarde is de versie die u bij het downloaden van het item van Azure in de vorige procedure hebt genoteerd. 
 
   In het volgende voorbeeldscript worden waarden voor de Windows Server 2016 Datacenter - Server Core-virtuele machine gebruikt. De waarde voor *- Osuri* is een van voorbeeldpad naar de locatie van de blob-opslag voor het item.

   Als alternatief voor dit script kunt u de [procedure in dit artikel beschreven](azure-stack-add-vm-image.md#add-a-vm-image-through-the-portal) voor het importeren van de. VHD-installatiekopie met behulp van de Azure portal.

   ```PowerShell  
   Add-AzsPlatformimage `
    -publisher "MicrosoftWindowsServer" `
    -offer "WindowsServer" `
    -sku "2016-Datacenter-Server-Core" `
    -osType Windows `
    -Version "2016.127.20171215" `
    -OsUri "https://mystorageaccount.blob.local.azurestack.external/cont1/Microsoft.WindowsServer2016DatacenterServerCore-ARM.1.0.801.vhd"  
   ```

   **Over oplossingssjablonen:** Sommige sjablonen kunnen een klein 3 MB bevatten. VHD-bestand met de naam van de **fixed3.vhd**. U hoeft niet te importeren dat bestand in Azure Stack. Fixed3.VHD.  Dit bestand is opgenomen in sommige oplossingssjablonen om te voldoen aan vereisten voor de Azure Marketplace voor publiceren.

   Lees de beschrijving van de sjablonen downloaden en importeer vervolgens de aanvullende vereisten zoals VHD's die nodig zijn voor het werken met de oplossingssjabloon.  
   
   **Over extensies:** Wanneer u met de extensies voor virtuele machine-installatiekopie werkt, gebruikt u de volgende parameters:
   - *Publisher*
   - *Type*
   - *Versie*  

   U gebruikt geen *bieden* voor extensies.   


4.  PowerShell gebruiken voor de marketplace-item publiceren naar Azure Stack met behulp van de **toevoegen AzsGalleryItem** cmdlet. Bijvoorbeeld:  
    ```PowerShell  
    Add-AzsGalleryItem `
     -GalleryItemUri "https://mystorageaccount.blob.local.azurestack.external/cont1/Microsoft.WindowsServer2016DatacenterServerCore-ARM.1.0.801.azpkg" `
     –Verbose
    ```
5. Nadat u een galerij-item hebt gepubliceerd, is het nu beschikbaar voor gebruik. Om te bevestigen dat het galerie-item wordt gepubliceerd, gaat u naar **alle services**, en klik vervolgens onder de **algemene** categorie, selecteer **Marketplace**.  Als uw download een oplossingssjabloon is, zorg er dan voor dat u eventuele afhankelijke VHD-installatiekopie voor deze oplossingssjabloon toevoegen.  
  [ ![Weergave marketplace](media/azure-stack-download-azure-marketplace-item/view-marketplacesm.png "marketplace weergeven") ](media/azure-stack-download-azure-marketplace-item/view-marketplace.png#lightbox)  

U kunt nu extensies voor virtuele machines met de versie van PowerShell voor Azure Stack 1.3.0 toevoegen. Bijvoorbeeld:

```PowerShell
Add-AzsVMExtension -Publisher "Microsoft" -Type "MicroExtension" -Version "0.1.0" -ComputeRole "IaaS" -SourceBlob "https://github.com/Microsoft/PowerShell-DSC-for-Linux/archive/v1.1.1-294.zip" -SupportMultipleExtensions -VmOsType "Linux"
```

## <a name="next-steps"></a>Volgende stappen

[Een Marketplace-item maken en publiceren](azure-stack-create-and-publish-marketplace-item.md)