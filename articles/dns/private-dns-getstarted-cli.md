---
title: Een Azure DNS-privézone maken met de Azure CLI
description: In deze zelfstudie maakt en test u een DNS-privézone en -record in Azure DNS. Dit is een stapsgewijze handleiding voor het maken en beheren van uw eerste privé-DNS-zone en -record met behulp van de Azure CLI.
services: dns
author: vhorne
ms.service: dns
ms.topic: tutorial
ms.date: 3/11/2019
ms.author: victorh
ms.openlocfilehash: 2758817d58fdd2e80b302b5f833308dbde1a6b63
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57779161"
---
# <a name="create-an-azure-dns-private-zone-using-the-azure-cli"></a>Een Azure DNS-privézone maken met de Azure CLI

Deze zelfstudie leidt u stapsgewijs door de procedure voor het maken van uw eerste privé-DNS-zone en -record met behulp van de Azure CLI.

[!INCLUDE [private-dns-public-preview-notice](../../includes/private-dns-public-preview-notice.md)]

Een DNS-zone wordt gebruikt om de DNS-records voor een bepaald domein te hosten. Als u uw domein wilt hosten in Azure DNS, moet u een DNS-zone maken voor die domeinnaam. Alle DNS-records voor uw domein worden vervolgens gemaakt binnen deze DNS-zone. Als u een privé-DNS-zone wilt publiceren naar uw virtuele netwerk, geeft u de lijst op met virtuele netwerken die zijn toegestaan om records in de zone om te zetten.  Deze worden *virtuele resolutienetwerken* genoemd. U kunt ook een virtueel netwerk opgeven waarvoor Azure DNS hostnaamrecords bewaart wanneer een virtuele machine wordt gemaakt, een ander IP-adres krijgt of wordt verwijderd.  Dit wordt een *virtueel resolutienetwerk* genoemd.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een privé-DNS-zone maken
> * Virtuele testmachines maken
> * Een extra DNS-record maken
> * De privézone testen

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

U kunt deze zelfstudie desgewenst volgen met behulp van [Azure PowerShell](private-dns-getstarted-powershell.md).


[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-the-resource-group"></a>De resourcegroep maken

Eerst maakt u een resourcegroep die de DNS-zone gaat bevatten: 

```azurecli
az group create --name MyAzureResourceGroup --location "East US"
```

## <a name="create-a-dns-private-zone"></a>Een privé-DNS-zone maken

Een DNS-zone wordt gemaakt door de opdracht `az network dns zone create` met de waarde *Private* te gebruiken voor de parameter **ZoneType**. Het volgende voorbeeld wordt een DNS-zone met de naam **private.contoso.com** in de resourcegroep met de naam **MyAzureResourceGroup** en maakt u de DNS-zone beschikbaar voor het virtuele netwerk met de naam  **MyAzureVnet**.

Als de parameter **ZoneType** wordt weggelaten, wordt de zone gemaakt als een openbare zone, dus deze parameter is vereist voor het maken van een privézone.

```azurecli
az network vnet create \
  --name myAzureVNet \
  --resource-group MyAzureResourceGroup \
  --location eastus \
  --address-prefix 10.2.0.0/16 \
  --subnet-name backendSubnet \
  --subnet-prefixes 10.2.0.0/24

az network dns zone create -g MyAzureResourceGroup \
   -n private.contoso.com \
  --zone-type Private \
  --registration-vnets myAzureVNet
```

Als u een zone voor alleen naamomzetting (geen automatische hostnaam maken) wilt maken, kunt u de parameter *resolution-vnets* gebruiken in plaats van de parameter*registration-vnets*.

> [!NOTE]
> U kunt de automatisch gemaakte hostnaamrecords niet zien. Maar later kunt u een test uitvoeren om te controleren of ze bestaan.

### <a name="list-dns-private-zones"></a>Lijst met privé-DNS-zones weergeven

Gebruik `az network dns zone list` als u een opsomming wilt maken van DNS-zones. Zie `az network dns zone list --help` voor help.

Bij het opgeven van de resourcegroep worden alleen de zones in de resourcegroep vermeld:

```azurecli
az network dns zone list \
  --resource-group MyAzureResourceGroup
```

Als de resourcegroepen worden weggelaten, worden alle zones in het abonnement vermeld:

```azurecli
az network dns zone list 
```

## <a name="create-the-test-virtual-machines"></a>De virtuele testmachines maken

Maak nu twee virtuele machines, zodat u uw DNS-privézone kunt testen:

```azurecli
az vm create \
 -n myVM01 \
 --admin-username test-user \
 -g MyAzureResourceGroup \
 -l eastus \
 --subnet backendSubnet \
 --vnet-name myAzureVnet \
 --image win2016datacenter

az vm create \
 -n myVM02 \
 --admin-username test-user \
 -g MyAzureResourceGroup \
 -l eastus \
 --subnet backendSubnet \
 --vnet-name myAzureVnet \
 --image win2016datacenter
```

Het duurt enkele minuten voordat dit is voltooid.

## <a name="create-an-additional-dns-record"></a>Een extra DNS-record maken

Gebruik de opdracht `az network dns record-set [record type] add-record` om een DNS-record te maken. Voor hulp met bijvoorbeeld het toevoegen van A-records raadpleegt u `azure network dns record-set A add-record --help`.

 Het volgende voorbeeld wordt een record met de relatieve naam **db** in de DNS-Zone **private.contoso.com**, in de resourcegroep **MyAzureResourceGroup**. De volledig gekwalificeerde naam van de recordset is **db.private.contoso.com**. Het recordtype is A, met IP-adres 10.2.0.4.

```azurecli
az network dns record-set a add-record \
  -g MyAzureResourceGroup \
  -z private.contoso.com \
  -n db \
  -a 10.2.0.4
```

### <a name="view-dns-records"></a>DNS-records weergeven

Als u de DNS-records in uw zone wilt weergeven,voert u de volgende opdracht uit:

```azurecli
az network dns record-set list \
  -g MyAzureResourceGroup \
  -z private.contoso.com
```
Denk eraan dat u de automatisch gemaakte A-records niet kunt zien voor uw twee virtuele testmachines.

## <a name="test-the-private-zone"></a>De privézone testen

Nu kunt u de naamomzetting voor testen uw **private.contoso.com** privézone.

### <a name="configure-vms-to-allow-inbound-icmp"></a>VM’s configureren voor het toestaan van inkomende ICMP

U kunt de pingopdracht gebruiken voor het testen van naamomzetting. Configureer de firewall dus op beide virtuele machines om inkomende ICMP-pakketten toe te staan.

1. Maak verbinding met myVM01 en open een Windows PowerShell-venster met beheerdersbevoegdheden.
2. Voer de volgende opdracht uit:

   ```powershell
   New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
   ```

Herhaal voor myVM02.

### <a name="ping-the-vms-by-name"></a>De VM's pingen op naam

1. Ping vanuit de myVM02 Windows PowerShell-opdrachtprompt myVM01 met de naam van de automatisch geregistreerde hostnaam:
   ```
   ping myVM01.private.contoso.com
   ```
   De uitvoer ziet er ongeveer als volgt uit:
   ```
   PS C:\> ping myvm01.private.contoso.com

   Pinging myvm01.private.contoso.com [10.2.0.4] with 32 bytes of data:
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time=1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128

   Ping statistics for 10.2.0.4:
       Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
   Approximate round trip times in milli-seconds:
       Minimum = 0ms, Maximum = 1ms, Average = 0ms
   PS C:\>
   ```
2. Ping nu de **db**-naam die u eerder hebt gemaakt:
   ```
   ping db.private.contoso.com
   ```
   De uitvoer ziet er ongeveer als volgt uit:
   ```
   PS C:\> ping db.private.contoso.com

   Pinging db.private.contoso.com [10.2.0.4] with 32 bytes of data:
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128

   Ping statistics for 10.2.0.4:
       Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
   Approximate round trip times in milli-seconds:
       Minimum = 0ms, Maximum = 0ms, Average = 0ms
   PS C:\>
   ```

## <a name="delete-all-resources"></a>Alle resources verwijderen

Verwijder de **MyAzureResourceGroup** als u deze niet langer nodig hebt om de resources die u in deze zelfstudie hebt gemaakt te verwijderen.

```azurecli
az group delete --name MyAzureResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een DNS-privézone geïmplementeerd, een DNS-record gemaakt en de zone getest.
U kunt nu meer leren over DNS-privézones.

> [!div class="nextstepaction"]
> [Azure DNS gebruiken voor persoonlijke domeinen](private-dns-overview.md)
