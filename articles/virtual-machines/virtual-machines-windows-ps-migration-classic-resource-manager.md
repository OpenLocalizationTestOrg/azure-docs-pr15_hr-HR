<properties
    pageTitle="Migracija na Voditelj resursa sa servisom PowerShell | Microsoft Azure"
    description="U ovom se članku objašnjavaju migracije podržane platforme IaaS resursa od klasičnog Azure resursa upravitelju pomoću naredbe ljuske PowerShell za Azure"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="cynthn"/>

# <a name="migrate-iaas-resources-from-classic-to-azure-resource-manager-by-using-azure-powershell"></a>Migriranje resursi IaaS od klasičnog Azure resursa upravitelju pomoću komponente PowerShell Azure

Sljedeći vam koraci prikazuju kako pomoću naredbe ljuske PowerShell Azure migrirati infrastrukture kao usluge (IaaS) resurse iz modela uvođenje klasičnog model implementacije Azure Voditelj resursa. 

Ako želite, možete i migrirati resursima pomoću [Azure naredbeni redak sučelja (Azure EŽA)](virtual-machines-linux-cli-migration-classic-resource-manager.md).

- Pozadina na scenarije podržani migracije, potražite u članku [podržane platforme migracije IaaS resursa iz klasični za Azure Voditelj resursa](virtual-machines-windows-migration-classic-resource-manager.md). 
- Detaljne upute i vodič za migraciju, potražite u članku [tehničke precizno dive na podržane platforme migracije iz klasični za Azure Voditelj resursa](virtual-machines-windows-migration-classic-resource-manager-deep-dive.md).

## <a name="step-1-plan-for-migration"></a>Korak 1: Planiranje za migraciju

Slijedi popis nekoliko najboljih postupaka koji preporučujemo da se prilikom donošenja migracije IaaS resursa iz klasični upravitelju resursa:

- Pročitajte kroz [podržane i nepodržane značajke i konfiguracija](virtual-machines-windows-migration-classic-resource-manager.md). Ako imate virtualnim strojevima koji koristi nepodržane konfiguracije ili značajke, preporučujemo da Pričekajte konfiguraciju i podržanim značajkama da biste se objaviti. Osim toga, ako je najbolje odgovara vašim potrebama, uklonite tu značajku ili premjestiti iz tog konfiguracije da biste omogućili migracije.
-   Ako ste automatski skripte koje danas implementacija infrastrukturu i aplikacije, pokušajte da biste stvorili sličan postavljanje test pomoću tih skripte za migraciju. Osim toga, možete postaviti okruženja uzorka pomoću portala za Azure.

>[AZURE.IMPORTANT]Virtualne mreže pristupnika (web-mjesta-na-web-mjesta, Azure ExpressRoute pristupnik za aplikaciju, točke na web) trenutno nisu podržani za migraciju od klasičnog upravitelju resursa. Da biste migrirali klasični virtualne mreže s pristupnika, uklonite pristupnika prije pokretanja operacije potvrdi da biste premjestili s mrežom (možete pokrenuti korak Priprema bez brisanje pristupnika). Kada dovršite migracije, ponovno povezati pristupnika u Azure Voditelj resursa.

## <a name="step-2-install-the-latest-version-of-azure-powershell"></a>Korak 2: Instalirajte najnoviju verziju programa Azure PowerShell

Postoje dvije glavne mogućnosti da biste instalirali Azure PowerShell: [Galerija PowerShell](https://www.powershellgallery.com/profiles/azure-sdk/) ili [Platformu Web instalacijskog programa (WebPI)](http://aka.ms/webpi-azps). WebPI prima mjesečne. Galerija PowerShell prima ažuriranja na Neprekinuti temelj. U ovom se članku temelji se na Azure PowerShell verzije 2.1.0.

Upute za instalaciju potražite u članku [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md).

<br>
## <a name="step-3-set-your-subscription-and-sign-up-for-migration"></a>Korak 3: Postavljanje pretplate i prijaviti se za migraciju

Najprije odzivniku komponente PowerShell. Za migraciju, morate postaviti okruženja za oba klasični i resursima.

Prijavite se na račun za model Voditelj resursa.

```powershell
    Login-AzureRmAccount
```

Da biste dobili dostupna pretplate pomoću sljedeće naredbe:

```powershell
    Get-AzureRMSubscription | Sort SubscriptionName | Select SubscriptionName
```

Postavite Azure pretplatu za trenutnu sesiju. U ovom se primjeru postavlja zadani naziv pretplate na **Moje Azure pretplate**. Zamijenite naziv pretplate primjer vlastitu. 

```powershell
    Select-AzureRmSubscription –SubscriptionName "My Azure Subscription"
```

>[AZURE.NOTE] Registracija je jednokratni korak, ali to morate učiniti jedanput prije migracije. Bez registracije, pogledajte sljedeća poruka o pogrešci: 

>   *BadRequest: Pretplate nije registriran za migraciju.* 

Registrirajte se s davateljem resursa migracije pomoću sljedeće naredbe:

```powershell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

Pričekajte pet minuta za registraciju da biste završili. Pomoću sljedeće naredbe možete provjeriti status odobrenja:

```powershell
    Get-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

Provjerite je li RegistrationState `Registered` prije nastavka. 

Sada se prijavite na račun za klasični model.

```powershell
    Add-AzureAccount
```

Da biste dobili dostupna pretplate pomoću sljedeće naredbe:

```powershell
    Get-AzureSubscription | Sort SubscriptionName | Select SubscriptionName
```

Postavite Azure pretplatu za trenutnu sesiju. U ovom se primjeru postavlja zadane pretplate **Moje Azure**pretplatu. Zamijenite naziv pretplate primjer vlastitu. 

```powershell
    Select-AzureSubscription –SubscriptionName "My Azure Subscription"
```

<br>

## <a name="step-4-make-sure-you-have-enough-azure-resource-manager-virtual-machine-cores-in-the-azure-region-of-your-current-deployment-or-vnet"></a>Korak 4: Provjerite je li dovoljno jezgri Azure resursima virtualnog računala u području Azure trenutni implementacije ili VNET

Koristite sljedeću naredbu komponente PowerShell da biste provjerili trenutni broj jezgri imate u Azure Voditelj resursa. Dodatne informacije o kvotama core potražite u članku [ograničenja i Voditelj resursa Azure](../azure-subscription-service-limits.md#limits-and-the-azure-resource-manager). 

U ovom se primjeru provjerava dostupnost u regiji **Zapad SAD -a** . Zamijenite naziv regije primjer vlastitu. 

```powershell
Get-AzureRmVMUsage -Location "West US"
```

## <a name="step-5-run-commands-to-migrate-your-iaas-resources"></a>Korak 5: Pokretanje naredbe za migriranje IaaS resursa

>[AZURE.NOTE] Sve operacije u nastavku opisane su idempotent. Ako imate problema s koji nije nepodržane značajke ili pogreške u konfiguraciji, preporučujemo da ponovno pokušajte Priprema, prekinuti ili izvršavanje operacija. Platforme, onda pokušava željenu akciju.

### <a name="migrate-virtual-machines-in-a-cloud-service-not-in-a-virtual-network"></a>Migriranje virtualnih računala u oblaku (ne u virtualne mreže)

Dohvaćanje popisa servise u oblaku pomoću sljedeću naredbu, a zatim odaberite servisa u oblaku koju želite migrirati. Ako su VMs u servis u oblaku u virtualne mreže ili imaju weba ili tempiranja uloge, naredba vraća poruku o pogrešci.

```powershell
    Get-AzureService | ft Servicename
```

Dohvaćanje naziva implementacije za servis u oblaku. U ovom primjeru naziv usluge je **Moj servis**. Primjer naziva servisa zamijenite vlastiti naziv servisa. 

```powershell
    $serviceName = "My Service"
    $deployment = Get-AzureDeployment -ServiceName $serviceName
    $deploymentName = $deployment.DeploymentName
```

Priprema virtualnih računala u oblaku za migraciju. Imate dvije mogućnosti za odabir.

* **Mogućnost 1. Prenesite na VMs platformu stvorili virtualne mreže**

    Najprije provjerite valjanost ako možete migrirati servisa u oblaku pomoću sljedeće naredbe:

    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    $validate.ValidationMessages
    ```

    Prethodni naredba prikazuje sve upozorenja i pogreške blokira migracije. Ako je provjera valjanosti uspije, pa možete premještati na **Priprema** korak:

    ```powershell
    Move-AzureService -Prepare -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    ```

* **Mogućnost 2. Migracija postojeće virtualne mreže u modelu implementacije Voditelj resursa**

    U ovom se primjeru postavlja naziv grupe resursa **myResourceGroup**, naziv virtualne mreže da biste **myVirtualNetwork** i naziv podmreži **mySubNet**. Zamijenite nazive u primjeru imena vlastite resurse.

    ```powershell
    $existingVnetRGName = "myResourceGroup"
    $vnetName = "myVirtualNetwork"
    $subnetName = "mySubNet"
    ```

    Najprije provjerite valjanost ako možete migrirati servisa u oblaku pomoću sljedeće naredbe:

    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName -VirtualNetworkName $vnetName -SubnetName $subnetName
    $validate.ValidationMessages
    ```

    Prethodni naredba prikazuje sve upozorenja i pogreške blokira migracije. Ako je provjera valjanosti uspije, pa možete nastaviti sa sljedećim korakom Priprema:

    ```powershell
    Move-AzureService -Prepare -ServiceName $serviceName -DeploymentName $deploymentName `
        -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName `
        -VirtualNetworkName $vnetName -SubnetName $subnetName
    ```
    

Nakon uspješnog postupka Priprema pomoću jedne od prethodne mogućnosti upita migracije stanje na VMs. Provjerite jesu li u na `Prepared` stanje.

U ovom se primjeru postavlja naziv VM **myVM**. Primjer naziva zamijenite vlastiti naziv VM.

    ```powershell
    $vmName = "myVM"
    $vm = Get-AzureVM -ServiceName $serviceName -Name $vmName
    $vm.VM.MigrationState
    ```

Provjerite konfiguraciju pripremljeni resursa pomoću programa PowerShell ili Azure portal. Ako niste spremni za migraciju i želite da biste se vratili na staru stanje, koristite sljedeću naredbu:

```powershell
    Move-AzureService -Abort -ServiceName $serviceName -DeploymentName $deploymentName
```

Ako ste zadovoljni izgledom pripremljeni konfiguraciju, možete kretanje naprijed i primjenu resursa pomoću sljedeće naredbe:

```powershell
    Move-AzureService -Commit -ServiceName $serviceName -DeploymentName $deploymentName
```

### <a name="migrate-virtual-machines-in-a-virtual-network"></a>Migriranje virtualnim strojevima u virtualne mreže

Da biste migrirali virtualnim strojevima u virtualne mreže migrirati mrežu. Virtualnim strojevima moguće automatski migrirati s mrežom. Odaberite virtualne mreže koju želite migrirati. 

U ovom se primjeru postavlja naziv virtualne mreže **myVnet**. Zamijenite primjer imena virtualne mreže vlastitu. 

```powershell
    $vnetName = "myVnet"
```

>[AZURE.NOTE] Ako virtualne mreže sadrži weba ili uloge suradnika ili VMs s nepodržane konfiguracije, primit ćete poruku pogreške provjere valjanosti.

Najprije provjerite valjanost ako možete migrirati virtualne mreže pomoću sljedeće naredbe:

```powershell
    Move-AzureVirtualNetwork -Validate -VirtualNetworkName $vnetName
```

Prethodni naredba prikazuje sve upozorenja i pogreške blokira migracije. Ako je provjera valjanosti uspije, pa možete nastaviti sa sljedećim korakom Priprema:

```powershell
    Move-AzureVirtualNetwork -Prepare -VirtualNetworkName $vnetName
```

Provjerite konfiguraciju za pripremljeni virtualnim strojevima pomoću Azure PowerShell ili Azure portal. Ako niste spremni za migraciju i želite da biste se vratili na staru stanje, koristite sljedeću naredbu:

```powershell
    Move-AzureVirtualNetwork -Abort -VirtualNetworkName $vnetName
```

Ako ste zadovoljni izgledom pripremljeni konfiguraciju, možete kretanje naprijed i primjenu resursa pomoću sljedeće naredbe:

```powershell
    Move-AzureVirtualNetwork -Commit -VirtualNetworkName $vnetName
```

### <a name="migrate-a-storage-account"></a>Migracija na račun za pohranu

Kada završite migracije virtualnim strojevima, preporučujemo da migrirati račune za pohranu.

Priprema svaki račun za pohranu za migraciju pomoću sljedeće naredbe. U ovom se primjeru naziv računa spremišta je **myStorageAccount**. Primjer naziva zamijenite naziv računa za pohranu. 

```powershell
    $storageAccountName = "myStorageAccount"
    Move-AzureStorageAccount -Prepare -StorageAccountName $storageAccountName
```

Provjerite konfiguraciju računa pripremljeni prostora za pohranu pomoću Azure PowerShell ili Azure portal. Ako niste spremni za migraciju i želite da biste se vratili na staru stanje, koristite sljedeću naredbu:

```powershell
    Move-AzureStorageAccount -Abort -StorageAccountName $storageAccountName
```

Ako ste zadovoljni izgledom pripremljeni konfiguraciju, možete kretanje naprijed i primjenu resursa pomoću sljedeće naredbe:

```powershell
    Move-AzureStorageAccount -Commit -StorageAccountName $storageAccountName
```

## <a name="next-steps"></a>Daljnji koraci

- Dodatne informacije o migraciji potražite u članku [podržane platforme migracije IaaS resursa iz klasični za Azure Voditelj resursa](virtual-machines-windows-migration-classic-resource-manager.md).
- Da biste migrirali dodatni mrežni resursi za Voditelj resursa pomoću komponente PowerShell Azure, koristite slične korake [Premjesti AzureNetworkSecurityGroup](https://msdn.microsoft.com/library/mt786729.aspx), [Premještanje AzureReservedIP](https://msdn.microsoft.com/library/mt786752.aspx)i [Premjesti AzureRouteTable](https://msdn.microsoft.com/library/mt786718.aspx).
- Otvori izvor skripte koristite migrirajte Azure resursa iz klasični upravitelju resursa, potražite u odjeljku [Alati zajednice za migraciju za migraciju Voditelj resursa Azure](virtual-machines-windows-migration-scripts.md)
