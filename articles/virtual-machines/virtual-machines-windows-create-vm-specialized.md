<properties
    pageTitle="Stvaranje kopije sustava Windows VM | Microsoft Azure"
    description="Saznajte kako stvoriti kopiju vaše specijalizirane Azure VM sa sustavom Windows, u modelu implementacije Voditelj resursa."
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
    ms.date="09/21/2016"
    ms.author="cynthn"/>

# <a name="create-a-vm-from-a-specialized-vhd"></a>Stvaranje na VM iz specijalizirane VHD

Stvorite novi VM Pridruživanjem specijalizirane VHD kao disk OS pomoću komponente Powershell. Specijalizirane VHD održava korisničkih računa, aplikacija i druge podatke o stanju iz vaše izvorne VM. 

Ako želite stvoriti u VM iz generalizirano VHD potražite u članku [Stvaranje VM iz generalizirano VHD slike](virtual-machines-windows-create-vm-generalized.md).

## <a name="create-the-subnet-and-vnet"></a>Stvaranje podmreži i vNet

Stvaranje vNet i podmreži [virtualne mreže](../virtual-network/virtual-networks-overview.md).

1. Stvaranje podmreži. U ovom se primjeru stvara podmreži pod nazivom **mySubNet**, u na resursa grupe **myResourceGroup**, i postavlja adresu predbroj **10.0.0.0/24**.

    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubNet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```

2. Stvorite na vNet. U ovom se primjeru postavlja naziv virtualne mreže **myVnetName**, mjesto **Zapad SAD -a**i prefiks adresa za virtualne mreže da biste **10.0.0.0/16**. 

    ```powershell
    $location = "West US"
    $vnetName = "myVnetName"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    
            
## <a name="create-a-public-ip-address-and-nic"></a>Stvaranje javne IP adrese i SLIKA

Da biste omogućili komunikaciju s virtualnog računala u virtualne mreže, morate na [javnu IP adresa](../virtual-network/virtual-network-ip-addresses-overview-arm.md) i mrežno sučelje.

1. Stvaranje javne IP. U ovom primjeru javnu IP adresa ime postavljen je na **myIP**.

    ```powershell
    $ipName = "myIP"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location -AllocationMethod Dynamic
    ```       

2. Stvaranje na NIC. U ovom primjeru naziv NIC postavljen je na **myNicName**.

    ```powershell
    $nicName = "myNicName"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

## <a name="create-the-network-security-group-and-an-rdp-rule"></a>Stvaranje sigurnosne grupe mreže i pravilo RDP

Da biste se mogli prijaviti na vašem VM pomoću RDP, morate koristiti pravilo sigurnosti koje omogućuje pristup RDP na priključak 3389. Jer VHD za nove VM stvoren iz postojećeg specijalizirane VM nakon stvaranja na VM koje možete koristiti postojeći račun na računalu virtualnim izvora koji su imali dozvolu za prijavi se korištenjem RDP.

U ovom se primjeru postavlja naziv NSG **myNsg** i RDP naziv pravila za **myRdpRule**.

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```

Dodatne informacije o pravilima NSG i krajnje točke potražite u članku [Otvaranje priključke za VM u Azure pomoću komponente PowerShell](virtual-machines-windows-nsg-quickstart-powershell.md).

## <a name="create-the-vm-configuration"></a>Stvaranje konfiguracije VM

Postavljanje konfiguracije VM priložiti kopirane VHD kao OS VHD.


```powershell
    # Set the URI for the VHD that you want to use. In this example, the VHD file named "myOsDisk.vhd" is kept in a storage account named "myStorageAccount" in a container named "myContainer".
    $osDiskUri = "https://myStorageAccount.blob.core.windows.net/myContainer/myOsDisk.vhd"
    
    #Set the VM name and size. This example sets the VM name to "myVM" and the VM size to "Standard_A2".
    $vmName = "myVM"
    $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A2"

    #Add the NIC
    $vm = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id

    #Add the OS disk by using the URL of the copied OS VHD. In this example, when the OS disk is created, the term "osDisk" is appened to the VM name to create the OS disk name. This example also specifies that this Windows-based VHD should be attached to the VM as the OS disk.
    $osDiskName = $vmName + "osDisk"
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption attach -Windows
```


Ako imate diskova podataka koje je potrebno da bi se priložiti u VM, trebali biste dodati i sljedeće: 

```powershell
    # Optional: Add data disks by using the URLs of the copied data VHDs at the appropriate Logical Unit Number (Lun).
    $dataDiskName = $vmName + "dataDisk"
    $vm = Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -VhdUri $dataDiskUri -Lun 0 -CreateOption attach
```

Podaci i operacijski sustav na disku URL izgledati ovako: `https://StorageAccountName.blob.core.windows.net/BlobContainerName/DiskName.vhd`. Možete pronaći to portala za pregledavanje spremniku prostora za pohranu za ciljnu, klikom operacijski sustav ili VHD kopirane podatke, a zatim kopirajte sadržaj URL-a.


## <a name="create-the-vm"></a>Stvaranje na VM

Stvaranje VM pomoću konfiguracija koji smo upravo stvorili.

```powershell
#Create the new VM
New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm
```

Ako ta naredba nije uspjela, vidjet ćete izlaz ovako:

```
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   
 
```
 
## <a name="verify-that-the-vm-was-created"></a>Provjerite je li u VM stvorena 
 
Trebali biste vidjeti novostvorenu VM ili [Azure portal](https://portal.azure.com)u odjeljku **Pregled** > **virtualnih računala**ili pomoću sljedeće naredbe ljuske PowerShell:

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a>Daljnji koraci

Da biste se prijavili novi virtualnog računala, pronađite VM [portal](https://portal.azure.com), kliknite **Poveži**i otvorite datoteku udaljene radne površine RDP. Pomoću vjerodajnica računa izvorne virtualnog računala prijavite se u novi virtualnog računala. Dodatne informacije potražite u članku [Povezivanje i prijavite se na Azure virtualnog računala sa sustavom Windows](virtual-machines-windows-connect-logon.md).







