---
title: Configureren van instellingen voor het gebruik in leslokaallabs van Azure Lab-Services | Microsoft Docs
description: Informatie over het configureren van het aantal gebruikers voor de testomgeving, ze geregistreerd bij de testomgeving, bepalen het aantal uren dat de virtuele machine, en meer kan worden gebruikt.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/17/2019
ms.author: spelluru
ms.openlocfilehash: 371751f2eb643c3699e5462fe44380d7792243ef
ms.sourcegitcommit: 9f07ad84b0ff397746c63a085b757394928f6fc0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2019
ms.locfileid: "54388307"
---
# <a name="configure-usage-settings-and-policies"></a>Instellingen voor het gebruik en het beleid configureren
In dit artikel wordt beschreven hoe u gebruikers toevoegen aan het lab, ze geregistreerd bij de testomgeving, bepalen het aantal uren dat de virtuele machine, en meer kan worden gebruikt. 


## <a name="add-users-to-the-lab"></a>Gebruikers toevoegen aan het lab
Als u hebt de **toegang beperken** ingeschakeld, gebruikers (e-mailadressen) toevoegen aan de lijst.

1. Selecteer **Gebruikers** in het menu links.
2. Selecteer **Gebruikers toevoegen** op de werkbalk. 
3. Op de pagina **Gebruikers toevoegen** voert u e-mailadressen van gebruikers in op afzonderlijke regels of op één regel gescheiden door puntkomma's. 

    ![E-mailadressen van gebruikers toevoegen](../media/how-to-configure-student-usage/add-users-email-addresses.png)
4. Selecteer **Opslaan**. U ziet de e-mailadressen van gebruikers en hun status (al dan niet geregistreerd) in de lijst. 

    ![Lijst met gebruikers](../media/how-to-configure-student-usage/users-list-new.png)

## <a name="send-registration-link-to-students"></a>Registratiekoppeling naar studenten verzenden
De volgende procedure bevat stappen voor het verzenden van een registratiekoppeling voor gebruikers. Als de **toegang beperken** is ingeschakeld voor de testomgeving, alleen gebruikers in de lijst met gebruikers de registratiekoppeling gebruiken kunnen om u te registreren met het lab. 

1. Schakel over naar de **gebruikers** weergeven door te selecteren **gebruikers** in het menu links. 
2. Selecteer de tegel **Registratiekoppeling ophalen**.

    ![Registratiekoppeling voor studenten](../media/tutorial-setup-classroom-lab/dashboard-user-registration-link.png)
1. Selecteer in het dialoogvenster **Gebruikersregistratie** de knop **Kopiëren**. De koppeling wordt naar het klembord gekopieerd. Plak deze in een e-mailprogramma en verstuur een e-mail naar de student. 

    ![Registratiekoppeling voor studenten](../media/tutorial-setup-classroom-lab/registration-link.png)
2. Selecteer in het dialoogvenster **Gebruikersregistratie** de optie **Sluiten**. 
4. Deel de registratielink met een student zodat de student zich voor de les kan registreren. Als u de instelling **Optie beperken** hebt ingeschakeld en een lijst met gebruikers in de lijst hebt, voert u de volgende acties uit:
    1. Selecteer het **e-mailadres** van de gebruiker in de lijst. 
    2. U ziet een venster van uw standaard-e-mailprogramma met het **Aan**-adres ingevuld. 
    3. Plak de **registratie-URL** die u eerder hebt gekopieerd in het bericht. 
    4. Verzend het **e-mailbericht**. 

## <a name="view-users-registered-with-the-lab"></a>Gebruikers weergeven die zijn geregistreerd bij het lab

Selecteer **gebruikers** geregistreerd in het menu links om te zien van de lijst met gebruikers met de testomgeving. 

![Lijst met gebruikers die zijn geregistreerd bij de testomgeving](../media/how-to-configure-student-usage/users-list-new.png)

## <a name="set-quotas-per-user"></a>Quota instellen per gebruiker
U kunt quota's per gebruiker instellen met behulp van de volgende stappen uit: 

1. Selecteer **Gebruikers** in het menu links.
2. Selecteer **quotum per gebruiker: onbeperkte** op de werkbalk. 
3. Op de **quotum per gebruiker** weergeeft, schakelt **beperken het aantal uren dat een gebruiker kan een virtuele machine gebruiken**. 
4. Voor **hoeveel uur wilt u geven aan elke gebruiker**, voer het aantal uren en selecteert u **opslaan**. 

    ![Aantal uren per gebruiker](../media/how-to-configure-student-usage/number-of-hours-per-user.png)
5. Ziet u het aantal uren op de werkbalk nu: **Quotum per gebruiker: &lt;aantal uren&gt;**. 

    ![Quotum per gebruiker](../media/how-to-configure-student-usage/quota-per-user.png)

### <a name="add-users-by-uploading-a-csv-file"></a>Gebruikers toevoegen door een CSV-bestand te uploaden
U kunt ook gebruikers toevoegen door het uploaden van een CSV-bestand met e-mailadressen van gebruikers.

1. Selecteer **uploaden CSV** op de werkbalk.
2. Selecteer het CSV-bestand met e-mailadressen. Alle e-mailadressen moeten zich in één kolom, wanneer u het opent in Excel. 

## <a name="manage-user-vms"></a>Gebruiker virtuele machines beheren
Wanneer studenten registreren met Azure Lab Services met behulp van de registratie van de koppeling opgegeven, ziet u de virtuele machines toegewezen aan studenten op de **virtuele machines** tabblad. 

![Virtuele machines die zijn toegewezen aan studenten](../media/how-to-manage-classroom-labs/virtual-machines-students.png)

U kunt de volgende taken op een student VM kunt doen: 

- Een virtuele machine stoppen als de virtuele machine wordt uitgevoerd. 
- Start een virtuele machine als de virtuele machine is gestopt. 
- Maak verbinding met de VM. 
- De virtuele machine verwijderen. 
- Bekijk het aantal uren dat gebruikers de virtuele machine gebruikt. 

## <a name="update-number-of-virtual-machines-in-lab"></a>Aantal virtuele machines in het lab bijwerken
Voor het bijwerken van het aantal virtuele machines in het lab, voert u de volgende stappen uit de **virtuele Machines** pagina:

1. Selecteer **virtuele machines** in het menu links. 
2. Selecteer **Lab capaciteit: &lt;getal&gt; computer (s)** op de werkbalk. 
3. Voer de **getal** van virtuele machines.
4. Selecteer **Opslaan**.

    ![Virtuele machines in het lab](../media/how-to-configure-student-usage/number-virtual-machines.png)


## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen:

- [Labaccounts maken en beheren als beheerder](how-to-manage-lab-accounts.md)
- [Labs maken en beheren als labeigenaar](how-to-manage-classroom-labs.md)
- [Sjablonen instellen en publiceren als labeigenaar](how-to-create-manage-template.md)
- [Als een lab-gebruiker toegang krijgen tot leslokaallabs](how-to-use-classroom-lab.md)
