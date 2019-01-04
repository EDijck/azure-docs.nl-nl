---
title: Verbinding maken met Azure Stack met CLI | Microsoft Docs
description: Meer informatie over het gebruik van de platformoverschrijdende opdrachtregelinterface (CLI) om te beheren en implementeren van resources in Azure Stack
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/03/2019
ms.author: sethm
ms.reviewer: sijuman
ms.openlocfilehash: 2ab696436a8cf139eff92edc3b8ff2c27b40a7aa
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54018378"
---
# <a name="use-api-version-profiles-with-azure-cli-in-azure-stack"></a>API-versieprofielen gebruiken met Azure CLI in Azure Stack

U kunt de stappen in dit artikel om in te stellen van de Azure-opdrachtregelinterface (CLI) voor het beheren van Azure Stack Development Kit resources vanuit Linux, Mac en Windows-clientplatforms.

## <a name="install-cli"></a>CLI installeren

Aanmelden bij uw ontwikkelwerkstation en CLI installeren. Azure Stack is versie 2.0 of hoger van Azure CLI vereist. U kunt deze versie installeren met behulp van de stappen in de [Azure CLI installeren](/cli/azure/install-azure-cli) artikel. Om te controleren of de installatie geslaagd is, open een terminal of opdrachtpromptvenster en voer de volgende opdracht uit:

```azurecli
az --version
```

Hier ziet u de versie van Azure CLI en andere afhankelijke bibliotheken die zijn geïnstalleerd op uw computer.

## <a name="trust-the-azure-stack-ca-root-certificate"></a>Vertrouwen van de Azure Stack-CA-basiscertificaat

1. Ophalen van de Azure Stack-CA-basiscertificaat van [uw Azure Stack-operators](../azure-stack-cli-admin.md#export-the-azure-stack-ca-root-certificate) en vertrouwt. Als u het basiscertificaat van de Azure Stack-CA vertrouwt, voegt u deze toe aan het bestaande certificaat voor Python.

1. De certificaatlocatie op uw computer vinden. De locatie kan variëren, afhankelijk van waar u Python hebt geïnstalleerd. U moet hebben [pip](https://pip.pypa.io) en de [gecertificeerde](https://pypi.org/project/certifi/) -module geïnstalleerd. U kunt de volgende Python-opdracht uit vanuit de bash-prompt:

    ```bash  
    python -c "import certifi; print(certifi.where())"
    ```

    Noteer de certificaatlocatie: bijvoorbeeld, `~/lib/python3.5/site-packages/certifi/cacert.pem`. Het specifieke pad, is afhankelijk van uw besturingssysteem en de versie van Python gebruikt die u hebt geïnstalleerd.

### <a name="set-the-path-for-a-development-machine-inside-the-cloud"></a>Het pad voor een ontwikkelcomputer instellen in de cloud

Als u CLI vanaf een Linux-machine die wordt gemaakt binnen de Azure Stack-omgeving uitvoert, voer de volgende bash-opdracht door het pad naar het certificaat.

```bash
sudo cat /var/lib/waagent/Certificates.pem >> ~/<yourpath>/cacert.pem
```

### <a name="set-the-path-for-a-development-machine-outside-the-cloud"></a>Het pad voor een ontwikkelcomputer instellen buiten de cloud

Als u CLI vanaf een computer buiten de Azure Stack-omgeving uitvoert:  

1. Instellen van [VPN-verbinding met Azure Stack](azure-stack-connect-azure-stack.md).
1. Kopieer het PEM-certificaat dat u hebt verkregen via de Azure Stack-operators en noteer de locatie van het bestand (PATH_TO_PEM_FILE).
1. Voer de opdrachten in de volgende secties, afhankelijk van het besturingssysteem op uw ontwikkelwerkstation.

#### <a name="linux"></a>Linux

```bash
sudo cat PATH_TO_PEM_FILE >> ~/<yourpath>/cacert.pem
```

#### <a name="macos"></a>macOS

```bash
sudo cat PATH_TO_PEM_FILE >> ~/<yourpath>/cacert.pem
```

#### <a name="windows"></a>Windows

```powershell
$pemFile = "<Fully qualified path to the PEM certificate Ex: C:\Users\user1\Downloads\root.pem>"

$root = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2
$root.Import($pemFile)

Write-Host "Extracting required information from the cert file"
$md5Hash    = (Get-FileHash -Path $pemFile -Algorithm MD5).Hash.ToLower()
$sha1Hash   = (Get-FileHash -Path $pemFile -Algorithm SHA1).Hash.ToLower()
$sha256Hash = (Get-FileHash -Path $pemFile -Algorithm SHA256).Hash.ToLower()

$issuerEntry  = [string]::Format("# Issuer: {0}", $root.Issuer)
$subjectEntry = [string]::Format("# Subject: {0}", $root.Subject)
$labelEntry   = [string]::Format("# Label: {0}", $root.Subject.Split('=')[-1])
$serialEntry  = [string]::Format("# Serial: {0}", $root.GetSerialNumberString().ToLower())
$md5Entry     = [string]::Format("# MD5 Fingerprint: {0}", $md5Hash)
$sha1Entry    = [string]::Format("# SHA1 Fingerprint: {0}", $sha1Hash)
$sha256Entry  = [string]::Format("# SHA256 Fingerprint: {0}", $sha256Hash)
$certText = (Get-Content -Path $pemFile -Raw).ToString().Replace("`r`n","`n")

$rootCertEntry = "`n" + $issuerEntry + "`n" + $subjectEntry + "`n" + $labelEntry + "`n" + `
$serialEntry + "`n" + $md5Entry + "`n" + $sha1Entry + "`n" + $sha256Entry + "`n" + $certText

Write-Host "Adding the certificate content to Python Cert store"
Add-Content "${env:ProgramFiles(x86)}\Microsoft SDKs\Azure\CLI2\Lib\site-packages\certifi\cacert.pem" $rootCertEntry

Write-Host "Python Cert store was updated to allow the Azure Stack CA root certificate"
```

## <a name="get-the-virtual-machine-aliases-endpoint"></a>Het eindpunt van de virtuele machine-aliassen ophalen

Voordat u virtuele machines maken kunt met behulp van CLI, moet u contact op met de Azure Stack-operators en ophalen van het eindpunt van de virtuele machine-aliassen URI. Azure gebruikt bijvoorbeeld de volgende URI: `https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json`. Beheerder van de cloud moet instellen van een soortgelijke eindpunt voor Azure Stack met de afbeeldingen die beschikbaar in de Azure Stack marketplace zijn. U moet slagen voor de URI van het eindpunt van de `endpoint-vm-image-alias-doc` parameter voor de `az cloud register` opdracht, zoals wordt weergegeven in de volgende sectie. 
  
## <a name="connect-to-azure-stack"></a>Verbinding maken met Azure Stack

Gebruik de volgende stappen uit om te verbinden met Azure Stack:

1. Registreren van uw Azure Stack-omgeving door het uitvoeren van de `az cloud register` opdracht.
   
    a. Om u te registreren de *cloud administratieve* omgeving gebruiken:

      ```azurecli
      az cloud register \ 
        -n AzureStackAdmin \ 
        --endpoint-resource-manager "https://adminmanagement.local.azurestack.external" \ 
        --suffix-storage-endpoint "local.azurestack.external" \ 
        --suffix-keyvault-dns ".adminvault.local.azurestack.external" \ 
        --endpoint-vm-image-alias-doc <URI of the document which contains virtual machine image aliases>
      ```
    b. Om u te registreren de *gebruiker* omgeving gebruiken:

      ```azurecli
      az cloud register \ 
        -n AzureStackUser \ 
        --endpoint-resource-manager "https://management.local.azurestack.external" \ 
        --suffix-storage-endpoint "local.azurestack.external" \ 
        --suffix-keyvault-dns ".vault.local.azurestack.external" \ 
        --endpoint-vm-image-alias-doc <URI of the document which contains virtual machine image aliases>
      ```
    c. Om u te registreren de *gebruiker* in een omgeving Multitenancy gebruiken:

      ```azurecli
      az cloud register \ 
        -n AzureStackUser \ 
        --endpoint-resource-manager "https://management.local.azurestack.external" \ 
        --suffix-storage-endpoint "local.azurestack.external" \ 
        --suffix-keyvault-dns ".vault.local.azurestack.external" \ 
        --endpoint-vm-image-alias-doc <URI of the document which contains virtual machine image aliases> \
        --endpoint-active-directory-resource-id=<URI of the ActiveDirectoryServiceEndpointResourceID> \
        --profile 2018-03-01-hybrid
      ```
    d. Gebruik het volgende als u voor het registreren van de gebruiker in een AD FS-omgeving:

      ```azurecli
      az cloud register \
        -n AzureStack  \
        --endpoint-resource-manager "https://management.local.azurestack.external" \
        --suffix-storage-endpoint "local.azurestack.external" \
        --suffix-keyvault-dns ".vault.local.azurestack.external"\
        --endpoint-active-directory-resource-id "https://management.adfs.azurestack.local/<tenantID>" \
        --endpoint-active-directory-graph-resource-id "https://graph.local.azurestack.external/"\
        --endpoint-active-directory "https://adfs.local.azurestack.external/adfs/"\
        --endpoint-vm-image-alias-doc <URI of the document which contains virtual machine image aliases> \
        --profile "2018-03-01-hybrid"
      ```
1. De actieve omgeving instellen met behulp van de volgende opdrachten.
   
    a. Voor de *cloud administratieve* omgeving gebruiken:

      ```azurecli
      az cloud set \
        -n AzureStackAdmin
      ```

    b. Voor de *gebruiker* omgeving gebruiken:

      ```azurecli
      az cloud set \
        -n AzureStackUser
      ```

1. Werk de configuratie van uw omgeving voor het gebruik van de Azure Stack specifieke API-versie-profiel. Voor het bijwerken van de configuratie, moet u de volgende opdracht uitvoeren:

    ```azurecli
    az cloud update \
      --profile 2018-03-01-hybrid
   ```

    >[!NOTE]  
    >Als u een versie van Azure Stack voordat de 1808-build uitvoert, moet u het profiel van API-versie **2017-03-09-profiel** in plaats van de API-versie profiel **2018-03-01-hybride**.

1. Aanmelden bij uw Azure Stack-omgeving met behulp van de `az login` opdracht. U kunt aanmelden bij de Azure Stack-omgeving als een gebruiker of als een [service-principal](../../active-directory/develop/app-objects-and-service-principals.md). 

    * Azure AD-omgevingen
      * Meld u aan als een *gebruiker*: U kunt de gebruikersnaam en het wachtwoord rechtstreeks binnen opgeven de `az login` opdracht of verifiëren met behulp van een browser. U moet de laatste doen als uw account multi-factor authentication ingeschakeld heeft:

      ```azurecli
      az login \
        -u <Active directory global administrator or user account. For example: username@<aadtenant>.onmicrosoft.com> \
        --tenant <Azure Active Directory Tenant name. For example: myazurestack.onmicrosoft.com>
      ```

      > [!NOTE]
      > Als uw gebruikersaccount multi-factor authentication ingeschakeld heeft, kunt u de `az login command` zonder op te geven de `-u` parameter. Met deze opdracht geeft u een URL en een code die u gebruiken moet om te verifiëren.
   
      * Meld u aan als een *service-principal*: Voordat u zich aanmeldt, [maken van een service-principal via de Azure-portal](azure-stack-create-service-principals.md) of CLI en een rol toewijzen. Nu aanmelden met behulp van de volgende opdracht uit:

      ```azurecli  
      az login \
        --tenant <Azure Active Directory Tenant name. For example: myazurestack.onmicrosoft.com> \
        --service-principal \
        -u <Application Id of the Service Principal> \
        -p <Key generated for the Service Principal>
      ```
    * AD FS-omgevingen

        * Meld u aan als een gebruiker via een webbrowser:  
              ```azurecli  
              az login
              ```
        * Meld u aan als een gebruiker via een webbrowser met de apparaatcode van een:  
              ```azurecli  
              az login --use-device-code
              ```
        > [!Note]  
        >Met de opdracht geeft u een URL en een code die u gebruiken moet om te verifiëren.

        * Meld u aan als een service-principal:
        
          1. Bereid het .pem-bestand moet worden gebruikt voor service-principal-aanmelding.

            * Op de clientcomputer waar de principal is gemaakt, de service-principal-certificaat exporteren als een pfx met de persoonlijke sleutel te vinden op `cert:\CurrentUser\My`; de naam heeft dezelfde naam als de principal certificaat.
        
            * Converteer de pfx naar pem (Gebruik het hulpprogramma OpenSSL).

          2.  Meld u aan bij de CLI:
            ```azurecli  
            az login --service-principal \
              -u <Client ID from the Service Principal details> \
              -p <Certificate's fully qualified name, such as, C:\certs\spn.pem>
              --tenant <Tenant ID> \
              --debug 
            ```

## <a name="test-the-connectivity"></a>De connectiviteit testen

Met Alles instellen, CLI gebruiken voor het maken van resources in Azure Stack. U kunt bijvoorbeeld een resourcegroep voor een toepassing maken en toevoegen van een virtuele machine. Gebruik de volgende opdracht om een resourcegroep met de naam 'MyResourceGroup' te maken:

```azurecli
az group create \
  -n MyResourceGroup -l local
```

Als de resourcegroep is gemaakt, voert de vorige opdracht de volgende eigenschappen van de zojuist gemaakte resource:

![Resourcegroep uitvoer maken](media/azure-stack-connect-cli/image1.png)

## <a name="known-issues"></a>Bekende problemen

Er zijn bekende problemen bij het gebruik van de CLI in Azure Stack:

 - De interactieve modus van de CLI; bijvoorbeeld, de `az interactive` opdracht, wordt nog niet ondersteund in Azure Stack.
 - Als u de lijst met installatiekopieën van virtuele machines beschikbaar in Azure Stack, gebruikt de `az vm image list --all` opdracht in plaats van de `az vm image list` opdracht. Op te geven de `--all` optie zorgt ervoor dat het antwoord retourneert alleen de installatiekopieën die beschikbaar in uw Azure Stack-omgeving zijn.
 - Virtuele machine-installatiekopie aliassen die beschikbaar in Azure zijn is mogelijk niet van toepassing op Azure Stack. Wanneer u installatiekopieën van virtuele machines, moet u de volledige URN-parameter (Canonical: UbuntuServer:14.04.3-LTS:1.0.0) in plaats van de alias van de installatiekopie. Deze URN moet overeenkomen met de specificaties van de installatiekopie die is afgeleid van de `az vm images list` opdracht.

## <a name="next-steps"></a>Volgende stappen

- [Sjablonen implementeren met Azure CLI](azure-stack-deploy-template-command-line.md)
- [Azure CLI voor Azure Stack-gebruikers (Operator) inschakelen](../azure-stack-cli-admin.md)
- [Gebruikersmachtigingen beheren](azure-stack-manage-permissions.md) 