<properties
    pageTitle="Stvaranje virtualnog računala skaliranje skup pomoću komponente PowerShell | Microsoft Azure"
    description="Stvaranje virtualnog računala skaliranje skup pomoću komponente PowerShell"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="davidmu"/>

# <a name="create-a-windows-virtual-machine-scale-set-using-azure-powershell"></a>Stvaranje skupa pomoću Azure PowerShell skalu virtualnog računala Windows

Popunite-na-praznine pristup za stvaranje skupa skaliranje Azure virtualnog računala, slijedite ove korake. Potražite u članku [Virtualnog računala skaliranje postavlja pregled](virtual-machine-scale-sets-overview.md) da biste saznali više o skupovima mjerilo.

Da biste pomoću koraka u ovom članku traje 30 minuta.

## <a name="step-1-install-azure-powershell"></a>Korak 1: Instalacija Azure komponente PowerShell

Informacije o instalacija najnovije verzije sustava Azure PowerShell, Odabir pretplate i prijave na račun potražite u članku [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md) .

## <a name="step-2-create-resources"></a>Korak 2: Stvaranje resursi

Stvaranje resursa koje su vam potrebne za vaš novi skup mjerilo.

### <a name="resource-group"></a>Grupa resursa

Skaliranje skupu virtualnog računala moraju se nalaziti u grupu resursa.

1. Dobit ćete popis dostupnih mjesta gdje možete smjestiti resurse:

        Get-AzureLocation | Sort Name | Select Name

2. Odaberite mjesto koji vam najviše odgovara, zamijenite vrijednost **$locName** s tim nazivom mjesto, a zatim stvorite varijable:

        $locName = "location name from the list, such as Central US"

3. Zamijenite vrijednost **$rgName** naziv koji želite koristiti za novu grupu resursa, a zatim stvorite varijable: 

        $rgName = "resource group name"
        
4. Stvaranje grupe resursa:
    
        New-AzureRmResourceGroup -Name $rgName -Location $locName

    Trebali biste vidjeti nešto kao u ovom primjeru:

        ResourceGroupName : myrg1
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/########-####-####-####-############/resourceGroups/myrg1

### <a name="storage-account"></a>Račun za pohranu

Račun za pohranu za pohranu na disku operacijski sustav i dijagnostičkih podataka koji se koriste za skaliranje upotrebljava virtualnog računala. Kada je to moguće je najbolji način imati račun za pohranu za svaku virtualnog računala stvorene u skupu mjerilo. Ako to nije moguće, plan za više od 20 VMs po računu za pohranu. Primjer u ovom se članku prikazuje tri prostora za pohranu računa stvoren za tri virtualnih računala.

1. Zamijenite vrijednost **$saName** naziv računa za pohranu. Testirajte naziv jedinstvenosti. 

        $saName = "storage account name"
        Get-AzureRmStorageAccountNameAvailability $saName

    Ako je odgovor **True**, jedinstven je predloženog naziva.

3. Zamijenite vrijednost **$saType** vrstu računa za pohranu i stvara varijabla:  

        $saType = "storage account type"
        
    Moguće vrijednosti su: Standard_LRS, Standard_GRS, Standard_RAGRS ili Premium_LRS.
        
4. Stvaranje računa:
    
        New-AzureRmStorageAccount -Name $saName -ResourceGroupName $rgName –Type $saType -Location $locName

    Trebali biste vidjeti nešto kao u ovom primjeru:

        ResourceGroupName   : myrg1
        StorageAccountName  : myst1
        Id                  : /subscriptions/########-####-####-####-############/resourceGroups/myrg1/providers/Microsoft
                              .Storage/storageAccounts/myst1
        Location            : centralus
        AccountType         : StandardLRS
        CreationTime        : 3/15/2016 4:51:52 PM
        CustomDomain        :
        LastGeoFailoverTime :
        PrimaryEndpoints    : Microsoft.Azure.Management.Storage.Models.Endpoints
        PrimaryLocation     : centralus
        ProvisioningState   : Succeeded
        SecondaryEndpoints  :
        SecondaryLocation   :
        StatusOfPrimary     : Available
        StatusOfSecondary   :
        Tags                : {}
        Context             : Microsoft.WindowsAzure.Commands.Common.Storage.AzureStorageContext

5. Ponovite korake od 1 do 4 da biste stvorili tri računa za pohranu, primjerice myst1, myst2 i myst3.

### <a name="virtual-network"></a>Virtualne mreže

Za virtualnim strojevima u skupu skaliranje potreban je virtualne mreže.

1. Zamijenite vrijednost **$subnetName** naziv koji želite koristiti za podmreže u virtualne mreže, a zatim stvorite varijable: 

        $subnetName = "subnet name"
        
2. Stvaranje konfiguracije podmreže:
    
        $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
        
    Prefiks adresa mogu se razlikovati u virtualne mreže.

3. Zamijenite vrijednost **$netName** naziv koji želite koristiti za virtualne mreže, a zatim stvorite varijable: 

        $netName = "virtual network name"
        
4. Stvaranje virtualne mreže:
    
        $vnet = New-AzureRmVirtualNetwork -Name $netName -ResourceGroupName $rgName -Location $locName -AddressPrefix 10.0.0.0/16 -Subnet $subnet

### <a name="configuration-of-the-scale-set"></a>Konfiguracija skupa skala

Imate sve resurse koji vam je potrebna skale postavljanje konfiguracije, možemo stvoriti.  

1. Zamijenite vrijednost **$ipName** naziv koji želite koristiti za konfiguraciju IP, a zatim stvorite varijable: 

        $ipName = "IP configuration name"
        
2. Stvaranje IP konfiguracija:

        $ipConfig = New-AzureRmVmssIpConfig -Name $ipName -LoadBalancerBackendAddressPoolsId $null -SubnetId $vnet.Subnets[0].Id

2. Zamijenite vrijednost **$vmssConfig** naziv koji želite koristiti za konfiguraciju postavljanje mjerilo, a zatim stvorite varijable:   

        $vmssConfig = "Scale set configuration name"
        
3. Stvaranje konfiguraciju za postavljanje mjerilo:

        $vmss = New-AzureRmVmssConfig -Location $locName -SkuCapacity 3 -SkuName "Standard_A0" -UpgradePolicyMode "manual"
        
    U ovom se primjeru prikazuje skalu odredite stvoren s tri virtualnih računala. Dodatne informacije o kapacitet skaliranje skupove potražite [Postavlja pregled mjerilo za virtualnog računala](virtual-machine-scale-sets-overview.md) . Ovaj korak obuhvaća i postavljanje veličine (naziva se SkuName) virtualnim strojevima u skupu. Da biste pronašli veličina koji odgovara vašim potrebama, pogledajte [veličine za virtualnih računala](../virtual-machines/virtual-machines-windows-sizes.md).
    
4. Dodavanje mrežna konfiguracija sučelja postavljanje konfiguracije mjerilo:
        
        Add-AzureRmVmssNetworkInterfaceConfiguration -VirtualMachineScaleSet $vmss -Name $vmssConfig -Primary $true -IPConfiguration $ipConfig
        
    Trebali biste vidjeti nešto kao u ovom primjeru:

        Sku                   : Microsoft.Azure.Management.Compute.Models.Sku
        UpgradePolicy         : Microsoft.Azure.Management.Compute.Models.UpgradePolicy
        VirtualMachineProfile : Microsoft.Azure.Management.Compute.Models.VirtualMachineScaleSetVMProfile
        ProvisioningState     :
        OverProvision         :
        Id                    :
        Name                  :
        Type                  :
        Location              : Central US
        Tags                  :

#### <a name="operating-system-profile"></a>Operacijski sustav profila

1. Zamijenite vrijednost **$computerName** prefiks naziva računala koje želite koristiti, a zatim stvorite varijable: 

        $computerName = "computer name prefix"
        
2. Zamijenite vrijednost **$adminName** naziv administratorski račun na virtualnim računalima sustava, a zatim stvorite varijable:

        $adminName = "administrator account name"
        
3. Zamijenite vrijednost **$adminPassword** lozinku računa, a zatim stvorite varijable:

        $adminPassword = "password for administrator accounts"
        
4. Stvaranje profila operacijski sustav:

        Set-AzureRmVmssOsProfile -VirtualMachineScaleSet $vmss -ComputerNamePrefix $computerName -AdminUsername $adminName -AdminPassword $adminPassword

#### <a name="storage-profile"></a>Prostor za pohranu profila

1. Zamijenite vrijednost **$storageProfile** naziv koji želite koristiti za pohranu profila, a zatim stvorite varijable:  

        $storageProfile = "storage profile name"
        
2. Stvaranje varijabli koje definiraju sliku da biste koristili:  
      
        $imagePublisher = "MicrosoftWindowsServer"
        $imageOffer = "WindowsServer"
        $imageSku = "2012-R2-Datacenter"
        
    Da biste pronašli informacije o drugim slike koji želite koristiti, pogledajte [Kretanje i slika odaberite Azure virtualnog računala pomoću komponente Windows PowerShell i EŽA Azure](../virtual-machines/virtual-machines-windows-cli-ps-findimage.md).
        
3. Zamijenite vrijednost **$vhdContainers** s popisa koji sadrži putova gdje se pohranjuju virtualne tvrdom disku kao što su "https://mystorage.blob.core.windows.net/vhds", a zatim stvorite varijable:
       
        $vhdContainers = @("https://myst1.blob.core.windows.net/vhds","https://myst2.blob.core.windows.net/vhds","https://myst3.blob.core.windows.net/vhds")
        
4. Stvaranje profila za pohranu:

        Set-AzureRmVmssStorageProfile -VirtualMachineScaleSet $vmss -ImageReferencePublisher $imagePublisher -ImageReferenceOffer $imageOffer -ImageReferenceSku $imageSku -ImageReferenceVersion "latest" -Name $storageProfile -VhdContainer $vhdContainers -OsDiskCreateOption "FromImage" -OsDiskCaching "None"  

### <a name="virtual-machine-scale-set"></a>Postavljanje skaliranje virtualnog računala

Na kraju, možete stvoriti skup mjerilo.

1. Zamijenite vrijednost **$vmssName** naziv skupa skaliranje virtualnog računala, a zatim stvorite varijable:

        $vmssName = "scale set name"
        
2. Stvaranje skupa mjerilo:

        New-AzureRmVmss -ResourceGroupName $rgName -Name $vmssName -VirtualMachineScaleSet $vmss

    Trebali biste vidjeti nešto poput u ovom se primjeru prikazuju uspješno uvođenje:

        Sku                   : Microsoft.Azure.Management.Compute.Models.Sku
        UpgradePolicy         : Microsoft.Azure.Management.Compute.Models.UpgradePolicy
        VirtualMachineProfile : Microsoft.Azure.Management.Compute.Models.VirtualMachineScaleSetVMProfile
        ProvisioningState     : Updating
        OverProvision         :
        Id                    : /subscriptions/########-####-####-####-############/resourceGroups/myrg1/providers/Microso
                                ft.Compute/virtualMachineScaleSets/myvmss1
        Name                  : myvmss1
        Type                  : Microsoft.Compute/virtualMachineScaleSets
        Location              : centralus
        Tags                  :

## <a name="step-3-explore-resources"></a>Korak 3: Upoznavanje s resursima

Pomoću ove resurse da biste istražili skup skaliranje virtualnog računala koju ste stvorili:

- Portal za Azure - tijekom ograničenog vremenskog podataka dostupna je pomoću portala za.
- [Azure resursa Explorer](https://resources.azure.com/) – ovaj alat je najbolje za istraživanje trenutno stanje skupa mjerilo.
- Azure PowerShell – koristite sljedeću naredbu da biste dobili informacije:

        Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
        
        Or 
        
        Get-AzureRmVmssVM -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
        

## <a name="next-steps"></a>Daljnji koraci

- Upravljanje skup skaliranje koji ste upravo stvorili pomoću informacija u [Upravljanje virtualnim strojevima u skupu na skaliranje virtualnog računala](virtual-machine-scale-sets-windows-manage.md)
- Razmislite o implementaciji automatsko skaliranje vaš skaliranje postaviti pomoću informacija u [postavlja Automatska promjena veličine i virtualnog računala skala](virtual-machine-scale-sets-autoscale-overview.md)
- Dodatne informacije o okomito skaliranje tako da pročitate [Okomiti automatsko skaliranje s skupovima skaliranje virtualnog računala](virtual-machine-scale-sets-vertical-scale-reprovision.md)
