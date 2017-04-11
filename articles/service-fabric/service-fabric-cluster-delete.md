<properties
   pageTitle="Brisanje Azure klaster i njegovih resursa | Microsoft Azure"
   description="Saznajte kako u potpunosti izbrisati servisa tkanina skupine ili brisanje grupa resursa koji sadrži klaster ili selektivno brisanje resursi."
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/09/2016"
   ms.author="chackdan"/>

# <a name="delete-a-service-fabric-cluster-on-azure-and-the-resources-it-uses"></a>Brisanje klaster tkanina servisa Azure i resursi koji se koristi

Servis tkanina klaster sastoji se od mnogo drugih Azure resursa osim resursa klaster sam. Tako da u potpunosti izbrisati servisa tkanina klaster trebate izbrisati sve resurse koji se sastoji od.
Imate dvije mogućnosti: ili izbrisati grupu resursa koji se klaster (koje briše klaster resursa i drugih resursa u grupu resursa) ili posebno Izbriši resursa klaster, a zatim je mu pridružene resurse (ali ne drugih resursa u grupu resursa).

>[AZURE.NOTE] Brisanje na klaster resursa **ne** briše sve na ostale resurse koji svoj klaster servisa tkanina sastoji se od.

## <a name="delete-the-entire-resource-group-rg-that-the-service-fabric-cluster-is-in"></a>Brisanje grupe cijelu resursa (ru) koji se klaster tkanina servisa

Ovo je najjednostavniji način da biste bili sigurni da izbrišete sve resurse pridružene svoj klaster, uključujući grupu resursa. Možete izbrisati grupu resursa pomoću komponente PowerShell ili putem portala za Azure. Ako je grupu resursa resursi koji se odnose na servis tkanina klaster, možete izbrisati određene resurse.

### <a name="delete-the-resource-group-using-azure-powershell"></a>Brisanje grupe resursa pomoću komponente PowerShell Azure

Grupa resursa možete izbrisati i tako da pokrenete sljedeće Cmdlete Azure PowerShell. Provjerite je li 1.0 PowerShell Azure ili noviji instaliran na vašem računalu. Ako niste to prije, slijedite korake navedene u [kako instalirati i konfigurirati Azure PowerShell.](../powershell-install-configure.md)

Otvorite prozor PowerShell i pokrenite sljedeće Cmdlete PS:

```powershell
Login-AzureRmAccount

Remove-AzureRmResourceGroup -Name <name of ResouceGroup> -Force
```

Prikazat će se upit za potvrdu brisanja ako niste koristili u *– prisilno* mogućnost. Na potvrdu na ru i svi resursi sadrži se brišu.

### <a name="delete-a-resource-group-in-the-azure-portal"></a>Brisanje grupe resursa na portalu za Azure  

1. Prijavite se na [portal za Azure](https://portal.azure.com).
2. Dođite do klaster tkanina servis koji želite izbrisati.
3. Kliknite naziv grupe resursa na stranici essentials klaster.
4. Tako ćete prikazati stranici **Essentials grupu resursa** .
5. Kliknite **Izbriši**.
6. Slijedite upute na toj stranici da biste dovršili brisanje grupu resursa.

![Brisanje grupe resursa][ResourceGroupDelete]


## <a name="delete-the-cluster-resource-and-the-resources-it-uses-but-not-other-resources-in-the-resource-group"></a>Brisanje klaster resursa i resursima koristi, ali ne drugih resursa u grupu resursa

Ako je grupu resursa samo resursi vezani uz klaster tkanina servis koji želite izbrisati, zatim je jednostavnije da biste izbrisali grupu cijela resursa. Ako želite da biste selektivno brisanje resursa jedan po jedan u grupu resursa, slijedite ove korake.

Ako implementiran klaster pomoću portala za ili neki od predložaka Voditelj resursa tkanina servisa iz galerije predložaka, svi resursi koji koristi klaster su označene sljedeća dva oznakama. Možete ih koristiti da biste odlučili resursa koji želite izbrisati.

***Oznake #1:*** Ključ = clusterName, vrijednost = naziv od klaster

***Oznake #2:*** Ključ = resourceName, vrijednost = ServiceFabric

### <a name="delete-specific-resources-in-the-azure-portal"></a>Brisanje određenih resursa na portalu za Azure

1. Prijavite se na [portal za Azure](https://portal.azure.com).
2. Dođite do klaster tkanina servis koji želite izbrisati.
3. Idite na **sve postavke** na plohu essentials.
4. Kliknite **oznake** u odjeljku **Upravljanje resursima** u plohu postavke.
5. Kliknite jednu od **oznake** u plohu oznake da biste dobili popis svih resursa s tom oznakom.

    ![Oznaka resursa][ResourceTags]

6. Nakon što dodate na popisu označeni resursa, kliknite sve resurse i njihovo brisanje.

    ![Označeni resursi][TaggedResources]

### <a name="delete-the-resources-using-azure-powershell"></a>Brisanje resursima pomoću komponente PowerShell Azure

Resursi za jednu po jednu možete izbrisati tako da pokrenete sljedeće Cmdlete Azure PowerShell. Provjerite je li 1.0 PowerShell Azure ili noviji instaliran na vašem računalu. Ako niste to prije, slijedite korake navedene u [kako instalirati i konfigurirati Azure PowerShell.](../powershell-install-configure.md)

Otvorite prozor PowerShell i pokrenite sljedeće Cmdlete PS:

```powershell
Login-AzureRmAccount
```
Za sve resurse koji želite izbrisati, pokrenite sljedeću naredbu:

```powershell
Remove-AzureRmResource -ResourceName "<name of the Resource>" -ResourceType "<Resource Type>" -ResourceGroupName "<name of the resource group>" -Force
```

Da biste izbrisali klaster resursa, pokrenite sljedeću naredbu:

```powershell
Remove-AzureRmResource -ResourceName "<name of the Resource>" -ResourceType "Microsoft.ServiceFabric/clusters" -ResourceGroupName "<name of the resource group>" -Force
```

## <a name="next-steps"></a>Daljnji koraci
Pročitajte sljedeće da biste informacije o nadogradnji na klaster te particija usluge:

- [Dodatne informacije o nadogradnji klaster](service-fabric-cluster-upgrade.md)
- [Dodatne informacije o particija s praćenjem stanja servisa za maksimalno skaliranje](service-fabric-concepts-partitioning.md)


<!--Image references-->
[ResourceGroupDelete]: ./media/service-fabric-cluster-delete/ResourceGroupDelete.PNG

[ResourceTags]: ./media/service-fabric-cluster-delete/ResourceTags.png

[TaggedResources]: ./media/service-fabric-cluster-delete/TaggedResources.PNG
