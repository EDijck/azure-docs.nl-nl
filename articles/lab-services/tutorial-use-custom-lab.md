---
title: Een lab openen in Azure DevTest Labs | Microsoft Docs
description: In deze zelfstudie opent u het lab dat is gemaakt met Azure DevTest Labs, claimt u virtuele machines, gebruikt u deze en maakt u de claims op een virtuele machine weer ongedaan.
services: devtest-lab, lab-services, virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 01/18/2019
ms.author: spelluru
ms.openlocfilehash: b5abb8d4aad7c58bf673aa578255efe12d32ad4b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "61083534"
---
# <a name="tutorial-access-a-lab-in-azure-devtest-labs"></a>Zelfstudie: Een lab openen in Azure DevTest Labs
In deze zelfstudie gebruikt u de testomgeving die is gemaakt in de [zelfstudie: Een lab maken in Azure DevTest Labs](tutorial-create-custom-lab.md) .

In deze zelfstudie voert u de volgende acties uit:

> [!div class="checklist"]
> * Een virtuele machine (VM) in het lab claimen
> * Verbinding maken met de virtuele machine
> * De claim op de virtuele machine ongedaan maken

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/) aan voordat u begint.

## <a name="access-the-lab"></a>Toegang tot het lab

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **Alle resources** in het menu aan de linkerkant. 
3. Selecteer **DevTest Labs** voor het resourcetype. 
4. Selecteer het lab. 

    ![Het lab selecteren](./media/tutorial-use-custom-lab/search-for-select-custom-lab.png)

## <a name="claim-a-vm"></a>Een virtuele machine claimen

1. Selecteer in de lijst met **Claimbare virtuele machines** **...** (beletselteken) en selecteer **Machine claimen**.

    ![Virtuele machine claimen](./media/tutorial-use-custom-lab/claim-virtual-machine.png)
1. Controleer of u de virtuele machine in de lijst **Mijn virtuele machines** ziet.

    ![Mijn virtuele machine](./media/tutorial-use-custom-lab/my-virtual-machines.png)

## <a name="connect-to-the-vm"></a>Verbinding maken met de virtuele machine

1. Selecteer de VM in de lijst. U ziet de **pagina Virtuele Machine** voor uw virtuele machine. Selecteer **Verbinding maken** op de werkbalk.

    ![Verbinding maken met de virtuele machine](./media/tutorial-use-custom-lab/connect-button.png)
2. Sla het gedownloade **RDP**-bestand van de harde schijf op en gebruik het om verbinding te maken met de virtuele machine. Geef de gebruikersnaam en het wachtwoord op die u bij het maken van de VM in de vorige sectie hebt opgegeven. 

    > [!NOTE] 
    > Als u verbinding wilt maken met een Linux-VM, moet SSH- en/of RDP-toegang worden ingeschakeld voor de virtuele machine. Zie, [Extern bureaublad installeren en configureren om verbinding te maken met een virtuele Linux-machine in Azure](../virtual-machines/linux/use-remote-desktop.md) voor stappen voor het maken van verbinding met een Linux-VM via RDP. 


## <a name="unclaim-the-vm"></a>De claim op de virtuele machine ongedaan maken
Nadat u klaar bent met het gebruik van de virtuele machine, kunt u de claim op de virtuele machine ongedaan maken door de volgende stappen te volgen: 

1. Ga naar de pagina van de virtuele machine en selecteer **Claim ongedaan maken** op de werkbalk. 

    ![Claim op virtuele machine ongedaan maken](./media/tutorial-use-custom-lab/unclaim-vm-menu.png)
1. De virtuele machine wordt uitgeschakeld voordat de claim ongedaan wordt gemaakt. U kunt de status van deze bewerking zien in meldingen.  
3. Ga terug naar de DevTest Lab-pagina door in het breadcrumb-menu bovenaan op de naam van uw lab te klikken. 
    
    ![Terug navigeren naar lab](./media/tutorial-use-custom-lab/breadcrumb-to-lab.png)
1. Controleer of de virtuele machine wordt weergegeven in de lijst met **Claimbare virtuele machines** onderaan het scherm.

    
## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u gezien hoe u toegang kunt krijgen tot en gebruik kunt maken van een lab dat is gemaakt met Azure DevTest Labs. Voor meer informatie over toegang tot en het gebruik van virtuele machines in een lab raadpleegt u 

> [!div class="nextstepaction"]
> [Procedure: Virtuele machines gebruiken in een lab](devtest-lab-add-vm.md)

