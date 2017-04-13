<properties
    pageTitle="Stvaranje VM iz generalizirano VHD | Microsoft Azure"
    description="Saznajte kako stvoriti virtualnog računala za Windows s generalizirano VHD slike pomoću komponente PowerShell Azure u model implementacije Voditelj resursa."
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
    ms.date="10/10/2016"
    ms.author="cynthn"/>

# <a name="create-a-vm-from-a-generalized-vhd-image"></a>Stvaranje na VM iz generalizirano VHD slike

Generalizirano VHD slika je prodao svim vašim informacijama osobnog računa ukloniti pomoću [Sysprep](virtual-machines-windows-generalize-vhd.md). Generalizirano VHD možete stvoriti tako da pokrenete Sysprep na lokalni VM, [Prijenos VHD za Azure](virtual-machines-windows-upload-image.md)ili tako da pokrenete Sysprep na programa postojeće VM Azure, a zatim [Kopiranje u VHD](virtual-machines-windows-vhd-copy.md).

Ako želite stvoriti u VM iz specijalizirane VHD potražite u članku [Stvaranje VM iz specijalizirane VHD](virtual-machines-windows-create-vm-specialized.md).

Najbrži način da biste stvorili na VM iz generalizirano VHD jest korištenje [brzi početak rada predloška] (https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image). 


## <a name="prerequisites"></a>Preduvjeti

Ako namjeravate koristiti VHD prenesene s lokalnim VM, kao što je stvoren pomoću značajke Hyper-V, provjerite je li slijediti upute u [odjeljku Priprema VHD Windows da biste prenijeli na Azure](virtual-machines-windows-prepare-for-upload-vhd-image.md). 

Preneseni VHDs i postojeće VHDs VM Azure moraju biti GRG generalizirano da biste mogli stvarati na VM koristite taj način. Dodatne informacije potražite u članku [konf virtualnog računala za Windows koristi Sysprep](virtual-machines-windows-generalize-vhd.md). 


## <a name="set-the-uri-of-the-vhd"></a>Postavljanje URI na VHD

URI za VHD da biste koristili je u obliku: https://**mystorageaccount**.blob.core.windows.net/**mycontainer**/**MyVhdName**.vhd. U ovom primjeru VHD pod nazivom **myVHD** nalazi se u račun za pohranu **mystorageaccount** u spremniku **mycontainer**.

```powershell
$imageURI = "https://mystorageaccount.blob.core.windows.net/mycontainer/myVhd.vhd"
```


## <a name="create-a-virtual-network"></a>Stvaranje virtualne mreže

Stvaranje vNet i podmreže [virtualne mreže](../virtual-network/virtual-networks-overview.md).


1. Stvaranje podmreži. Sljedeća Ogledna formula stvara podmreže pod nazivom **mySubnet** u grupu resursa **myResourceGroup** prefiksom adresu **10.0.0.0/24**.  

    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
      
2. Stvaranje virtualne mreže. Sljedeća Ogledna formula stvara virtualne mreže pod nazivom **myVnet** mjestu **Zapad SAD -a** s prefiksom adresu **10.0.0.0/16**.  

    ```powershell
    $location = "West US"
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    
            
## <a name="create-a-public-ip-address-and-network-interface"></a>Stvaranje na javnu IP adresa i mrežne sučelja

Da biste omogućili komunikaciju s virtualnog računala u virtualne mreže, morate na [javnu IP adresa](../virtual-network/virtual-network-ip-addresses-overview-arm.md) i mrežno sučelje.

1. Stvaranje javne IP adrese. U ovom se primjeru stvara javnu IP adresu pod nazivom **myPip**. 

    ```powershell
    $ipName = "myPip"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location -AllocationMethod Dynamic
    ```       

2. Stvaranje na NIC. U ovom se primjeru stvara NIC pod nazivom **myNic**. 

    ```powershell
    $nicName = "myNic"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

## <a name="create-the-network-security-group-and-an-rdp-rule"></a>Stvaranje sigurnosne grupe mreže i pravilo RDP

Da biste se mogli prijaviti na vašem VM pomoću RDP, morate koristiti sigurnosna pravila koja omogućuje pristup RDP na priključak 3389. 

U ovom se primjeru stvara sustava NSG pod nazivom **myNsg** koje sadrži pravilo pod nazivom **myRdpRule** koji omogućuje RDP promet putem priključka 3389. Dodatne informacije o NSGs potražite u članku [Otvaranje priključke za VM u Azure pomoću komponente PowerShell](virtual-machines-windows-nsg-quickstart-powershell.md).

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```


## <a name="create-a-variable-for-the-virtual-network"></a>Stvaranje tjednog prikaza kalendara za virtualne mreže

Stvaranje tjednog prikaza kalendara za dovršene virtualne mreže. 

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName
```

## <a name="create-the-vm"></a>Stvaranje na VM

Sljedeću skriptu komponente PowerShell pokazuje kako postaviti konfiguracija virtualnog računala i koristiti prenesene VM sliku kao izvorišnog web-mjesta za novu instalaciju.

</br>


```powershell
    # Enter a new user name and password to use as the local administrator account 
    # for remotely accessing the VM.
    $cred = Get-Credential
    
    # Name of the storage account where the VHD is located. This example sets the 
    # storage account name as "myStorageAccount"
    $storageAccName = "myStorageAccount"
    
    # Name of the virtual machine. This example sets the VM name as "myVM".
    $vmName = "myVM"
    
    # Size of the virtual machine. This example creates "Standard_D2_v2" sized VM. 
    # See the VM sizes documentation for more information: 
    # https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/
    $vmSize = "Standard_D2_v2"
    
    # Computer name for the VM. This examples sets the computer name as "myComputer".
    $computerName = "myComputer"
    
    # Name of the disk that holds the OS. This example sets the 
    # OS disk name as "myOsDisk"
    $osDiskName = "myOsDisk"
    
    # Assign a SKU name. This example sets the SKU name as "Standard_LRS"
    # Valid values for -SkuName are: Standard_LRS - locally redundant storage, Standard_ZRS - zone redundant storage, Standard_GRS - geo redundant storage, Standard_RAGRS - read access geo redundant storage, Premium_LRS - premium locally redundant storage. 
    $skuName = "Standard_LRS"
    
    # Get the storage account where the uploaded image is stored
    $storageAcc = Get-AzureRmStorageAccount -ResourceGroupName $rgName -AccountName $storageAccName
    
    # Set the VM name and size
    $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize
    
    #Set the Windows operating system configuration and add the NIC
    $vm = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $computerName `
        -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
    
    # Create the OS disk URI
    $osDiskUri = '{0}vhds/{1}-{2}.vhd' `
        -f $storageAcc.PrimaryEndpoints.Blob.ToString(), $vmName.ToLower(), $osDiskName
    
    # Configure the OS disk to be created from the existing VHD image (-CreateOption fromImage).
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri `
        -CreateOption fromImage -SourceImageUri $imageURI -Windows
    
    # Create the new VM
    New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm
```

## <a name="verify-that-the-vm-was-created"></a>Provjerite je li u VM stvorena 

Po završetku, trebali biste vidjeti novostvorenu VM [Azure portal](https://portal.azure.com) u odjeljku **Pregled** > **virtualnih računala**ili pomoću sljedeće naredbe ljuske PowerShell:

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a>Daljnji koraci

Da biste upravljali novi virtualnog računala s Azure PowerShell, potražite u članku [Upravljanje virtualnim strojevima pomoću upravitelja resursa Azure i PowerShell](virtual-machines-windows-ps-manage.md).


