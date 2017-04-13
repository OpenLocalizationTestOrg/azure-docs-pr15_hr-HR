<properties
   pageTitle="Stvaranje Windows VM s više NIC-ovi | Microsoft Azure"
   description="Saznajte kako stvoriti Windows VM s više NIC-ovi povezan s predlošcima Azure PowerShell ili upravitelj resursa."
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure"
   ms.date="10/27/2016"
   ms.author="iainfou"/>

# <a name="creating-a-windows-vm-with-multiple-nics"></a>Stvaranje Windows VM s više NIC-ovi
Možete stvoriti virtualnog računala (VM) u Azure koja ima više virtualne mreže sučelja (NIC-ovi) na pridružen. Uobičajeni scenarij bi da bi drugi podmreže za povezivanje sučelja i stražnje ili mreže za nadzor ili sigurnosne kopije rješenje. Ovaj članak sadrži brze naredbe za stvaranje na VM s više NIC-ovi pridružen. Detaljne informacije, uključujući kako stvoriti više NIC-ovi unutar vlastitih skripti komponente PowerShell dodatne informacije o [implementaciji VMs više SLIKA](../virtual-network/virtual-network-deploy-multinic-arm-ps.md). Različitim [veličinama VM](virtual-machines-windows-sizes.md) podržavaju različit broj NIC-ovi, pa veličina vaše VM sukladno tome.

>[AZURE.WARNING] NIC-ovi više morate pridružiti kada stvarate na VM – ne možete dodati NIC-ovi postojeće VM. Možete [stvoriti VM temelju izvorne virtualne diskove](virtual-machines-windows-vhd-copy.md) i stvaranje više NIC-ovi, kao što je implementirati u VM.

## <a name="create-core-resources"></a>Stvaranje temeljni resursi
Provjerite jeste li povezani s [najnovijim Azure PowerShell instalacije i konfiguracije](../powershell-install-configure.md). Prijavite se na račun za Azure:

```powershell
Login-AzureRmAccount
```

U sljedećim primjerima zamijenite primjer Nazivi parametara vlastitih vrijednosti. Nazivi parametara primjer dio `myResourceGroup`, `mystorageaccount`, i `myVM`.

Prvo, stvorite grupu resursa. Sljedeći primjer stvara grupu resursa pod nazivom `myResourceGroup` u na `WestUs` mjesto:

```powershell
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "WestUS"
```

Stvorite račun za pohranu za vaše VMs. Sljedeći primjer stvara za pohranu račun pod nazivom `mystorageaccount`:

```powershell
$storageAcc = New-AzureRmStorageAccount -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "mystorageaccount" `
    -Kind "Storage" -SkuName "Premium_LRS" 
```

## <a name="create-virtual-network-and-subnets"></a>Stvaranje virtualne mreže i podmreže
Definiranje dva podmreže virtualne mreže – jedan za sučelja promet i jedan za pozadinsku promet. U sljedećem primjeru definira dva podmreže pod nazivom `mySubnetFrontEnd` i `mySubnetBackEnd`:

```powershell
$mySubnetFrontEnd = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnetFrontEnd" `
    -AddressPrefix "192.168.1.0/24"
$mySubnetBackEnd = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnetBackEnd" `
    -AddressPrefix "192.168.2.0/24"
```

Stvaranje virtualne mreže i podmreže. Sljedeći primjer stvara virtualne mreže pod nazivom `myVnet`:

```powershell
$myVnet = New-AzureRmVirtualNetwork -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "myVnet" -AddressPrefix "192.168.0.0/16" `
    -Subnet $mySubnetFrontEnd,$mySubnetBackEnd
```


## <a name="create-multiple-nics"></a>Stvaranje više NIC-ovi
Stvorite dva NIC-ovi, jedan NIC u sučelja podmreži i jedan NIC priložite podmreži pozadinsku. Sljedeći primjer stvara dva NIC-ovi, pod nazivom `myNic1` i `myNic2`:

```powershell
$frontEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetFrontEnd'}
$myNic1 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "myNic1" -SubnetId $frontEnd.Id

$backEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetBackEnd'}
$myNic2 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "myNic2" -SubnetId $backEnd.Id
```

Obično se mogu stvoriti [mreže sigurnosne grupe](../virtual-network/virtual-networks-nsg.md) ili [Učitavanje opterećenja](../load-balancer/load-balancer-overview.md) olakšavaju upravljanje i raspodijelite promet na vaše VMs. U članku [detaljnije višestruki NIC VM](../virtual-network/virtual-network-deploy-multinic-arm-ps.md) vodi vas kroz stvaranje sigurnosne grupe mreže i dodjeljivanje NIC-ovi.


## <a name="create-the-virtual-machine"></a>Stvaranje virtualnog računala
Da biste sastavili konfiguraciju VM početi. Svaki je veličina VM ograničena ukupan broj NIC-ovi koje možete dodati na VM. Dodatne informacije o [veličinama VM sustava Windows](virtual-machines-windows-sizes.md). 

Prvo postavljanje vjerodajnica VM da biste na `$cred` varijabla na sljedeći način:

```powershell
$cred = Get-Credential
```

U sljedećem primjeru definira VM pod nazivom `myVM` i koristi veličina VM koji podržava do dva NIC-ovi (`Standard_DS2_v2`):

```powershell
$vmConfig = New-AzureRmVMConfig -VMName "myVM" -VMSize "Standard_DS2_v2"
```

Stvaranje ostatak datoteci config VM. Sljedeći primjer stvara sustava Windows Server 2012 R2 VM:

```powershell
$vmConfig = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName Te"MyVM" `
    -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
$vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -PublisherName "MicrosoftWindowsServer" `
    -Offer "WindowsServer" -Skus "2012-R2-Datacenter" -Version "latest"
```

Dodavanje dva mrežne kartice koji ste prethodno stvorili:

```powershell
$vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $myNic1.Id -Primary
$vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $myNic2.Id
```

Konfiguriranje virtualne disku i prostor za pohranu za svoje nove VM:

```powershell
$blobPath = "vhds/WindowsVMosDisk.vhd"
$osDiskUri = $storageAcc.PrimaryEndpoints.Blob.ToString() + $blobPath
$diskName = "windowsvmosdisk"
$vmConfig = Set-AzureRmVMOSDisk -VM $vmConfig -Name $diskName -VhdUri $osDiskUri `
    -CreateOption "fromImage"
```

Na kraju, stvorite je VM:

```powershell
New-AzureRmVM -VM $vmConfig -ResourceGroupName "myResourceGroup" -Location "WestUS"
```

## <a name="creating-multiple-nics-using-resource-manager-templates"></a>Stvaranje više NIC-ovi s predlošcima Voditelj resursa
Azure resursima predložaka koristite deklarativno JSON datoteke da biste definirali vaše okruženje. [Pregled Azure Voditelj resursa](../azure-resource-manager/resource-group-overview.md)možete čitati. Voditelj resursa predložaka omogućuje stvaranje više instanci resursa tijekom uvođenja, kao što su stvaranje više NIC-ovi. Koristite *kopiju* da biste odredili koliko je instanci da biste stvorili:

```bash
"copy": {
    "name": "multiplenics"
    "count": "[parameters('count')]"
}
```

Saznajte više o [stvaranju više instanci *kopiji*](../resource-group-create-multiple.md). 

Možete koristiti na `copyIndex()` da biste dodali broj naziv resursa koji omogućuje stvaranje `myNic1`, `MyNic2`, itd. Na sljedećoj je slici prikazan primjer dodavanje vrijednost indeksa:

```bash
"name": "[concat('myNic', copyIndex())]", 
```

Dovršavanje primjera [Stvaranje više NIC-ovi s predlošcima resursima](../virtual-network/virtual-network-deploy-multinic-arm-template.md)može čitati.

## <a name="next-steps"></a>Daljnji koraci
Provjerite je li da biste pregledali [Veličina Windows VM](virtual-machines-windows-sizes.md) prilikom stvaranja na VM s više NIC-ovi. Obratite pozornost na najveći broj NIC-ovi svaki veličina VM podržava. 

Imajte na umu da ne možete dodati dodatne NIC-ovi postojeće VM prilikom implementacije sustava VM morate stvoriti sve mrežne kartice. Pobrinuti se prilikom planiranja vaše implementacije da biste bili sigurni da imate sve potrebne mrežne veze iz na outset.