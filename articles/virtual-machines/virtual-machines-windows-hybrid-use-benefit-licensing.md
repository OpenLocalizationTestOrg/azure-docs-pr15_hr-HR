<properties
   pageTitle="Pogodnost korištenje Azure hibridnog poslužitelja prozor | Microsoft Azure"
   description="Saznajte kako Maksimiziranje prednosti sustava Windows Server Tehnološko jamstvo da bi se prikazala lokalnog licence za Azure"
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
   ms.workload="infrastructure-services"
   ms.date="07/13/2016"
   ms.author="georgem"/>

# <a name="azure-hybrid-use-benefit-for-windows-server"></a>Prednosti korištenja Azure hibridnog za Windows Server

Za korisnike koji se koriste Windows Server s Tehnološko jamstvo, Premjesti lokalnih licence za Windows Server Azure i korištenja Windows Server VMs u Azure smanjene trošak. Prednost korištenja Azure hibridnog omogućuje vam da biste pokrenuli Windows Server VMs u Azure i samo se naplatiti za stopu osnovni računalnim. Dodatne informacije potražite [pogodnost za korištenje hibridnog Azure licenciranje stranicu](https://azure.microsoft.com/pricing/hybrid-use-benefit/). U ovom se članku objašnjava kako uvesti Windows Server VMs u Azure da biste koristili ovu licenciranja pogodnost.

> [AZURE.NOTE] Azure Marketplace slike ne možete koristiti za implementaciju sustava Windows Server VMs Azure hibridnog koristiti prednosti korištenja. Morate uvesti vaše VMs pomoću programa PowerShell ili upravitelj resursa predložaka ispravno registrirati svoje VMs kao uvjete za diskontna stopa osnovni računalnim.

## <a name="pre-requisites"></a>Prije requisites
Postoji nekoliko stara requisites da bi se koriste Azure hibridnog korištenje prednosti za Windows Server VMs u Azure:

- Imate instaliran modul Azure PowerShell
- Imate sustava Windows Server VHD prenijeli Azure prostora za pohranu

### <a name="install-azure-powershell"></a>Instaliranje modula Azure PowerShell
Provjerite je li [instalacije i konfiguracije najnovije Azure PowerShell](../powershell-install-configure.md). Čak i ako se prilikom prelaska za implementaciju sustava VMs pomoću predložaka Voditelj resursa, i dalje potrebna Azure PowerShell instaliran za prijenos sustava Windows Server VHD (pogledajte sljedeći korak).

### <a name="upload-a-windows-server-vhd"></a>Prijenos Windows Server VHD

Da biste implementirali VM za poslužitelj sustava Windows u Azure, najprije morate stvoriti VHD koja sadrži vaše osnovni Sastavi Windows Server. U ovom VHD morate je komponenta pripremiti putem Sysprep prije prijenosa za Azure. [Saznajte više o VHD preduvjeti i Sysprep postupak](./virtual-machines-windows-upload-image.md) i možete [Sysprep podrška za uloge poslužitelja](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles). Sigurnosno kopirajte na VM prije pokretanja Sysprep. Kada ste spremni vaše VHD, prenijeti na VHD pomoću Azure prostora za pohranu računa na `Add-AzureRmVhd` cmdlet na sljedeći način:

```
Add-AzureRmVhd -ResourceGroupName MyResourceGroup -Destination "https://mystorageaccount.blob.core.windows.net/vhds/myvhd.vhd" -LocalFilePath 'C:\Path\To\myvhd.vhd'
```

> [AZURE.NOTE] Microsoft SQL Server i SharePoint Server Dynamics mogu se koristiti Tehnološko jamstvo licenciranje. I dalje potrebna Priprema slike za Windows Server tako instalacije aplikacija komponente i sukladno tome pruža tipke licence, a zatim prijenos slika na disku za Azure. Pregledajte odgovarajuće dokumentacije za pokretanje Sysprep s aplikacijom, kao što su [Zahtjevi za instalaciju sustava SQL Server koristi Sysprep](https://msdn.microsoft.com/library/ee210754.aspx) ili [izraditi sustava SharePoint Server 2016 referenca sliku (Sysprep)](http://social.technet.microsoft.com/wiki/contents/articles/33789.build-a-sharepoint-server-2016-reference-image-sysprep.aspx).

Također možete potražite dodatne informacije o [prijenosu VHD Azure postupak](./virtual-machines-windows-upload-image.md#upload-the-vm-image-to-your-storage-account).

> [AZURE.TIP] U ovom članku fokus je na implementaciji sustava Windows Server VMs. Na isti način možete implementirati i VMs klijenta sustava Windows. U sljedećim primjerima zamijenite `Server` s `Client` pravilno.

## <a name="deploy-a-vm-via-powershell-quick-start"></a>Implementacija VM putem komponente PowerShell brzi početak rada
Prilikom implementacije sustava Windows Server VM PowerShell vam je dodatni parametar za `-LicenseType`. Nakon što dodate na VHD prenijeli Azure, stvorite novi VM pomoću `New-AzureRmVM` i naveli vrstu licenciranja na sljedeći način:

```
New-AzureRmVM -ResourceGroupName MyResourceGroup -Location "West US" -VM $vm -LicenseType Windows_Server
```

Možete [pročitati detaljnije vodič na implementacija VM u Azure PowerShell](./virtual-machines-windows-hybrid-use-benefit-licensing.md#deploy-windows-server-vm-via-powershell-detailed-walkthrough) ispod ili pročitajte vodič za više opisne na različite korake da biste [stvorili Windows VM pomoću upravitelja resursa i PowerShell](./virtual-machines-windows-ps-create.md).

## <a name="deploy-a-vm-via-resource-manager"></a>Implementacija VM putem upravitelja resursa
Unutar predložaka Voditelj resursa, dodatni parametar za `licenseType` možete navesti. Dodatne informacije o [omogućeno Azure Voditelj resursa](../resource-group-authoring-templates.md). Nakon što dodate na VHD prenijeli Azure, uređivanje predloška resursima za vrstu licence kao dio uključiti davatelja računalnim i implementaciju predložak kao običnu:

```
"properties": {  
   "licenseType": "Windows_Server",
   "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
   },
```
 
## <a name="verify-your-vm-is-utilizing-the-licensing-benefit"></a>Provjerite je li vaša VM bi mogli koristiti licenciranja pogodnost
Kada uveli vaše VM metodom ili na PowerShell ili Voditelj resursa implementaciju provjerite vrstu licence s `Get-AzureRmVM` na sljedeći način:
 
```
Get-AzureRmVM -ResourceGroup MyResourceGroup -Name MyVM
```

Rezultat je slična sljedećoj:

```
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : Windows_Server
```

To contrasts sa sljedećim VM uvesti bez pogodnost za korištenje hibridnog Azure licenciranje, kao što su VM uvesti izravno iz galerije Azure:

```
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : 
```
 
## <a name="detailed-powershell-walkthrough"></a>Detaljni vodič PowerShell

Sljedeće detaljne upute za PowerShell prikaz cijelog implementacije u VM. Saznajte više konteksta stvarni cmdleta i razne komponente stvoren u [Stvaranje Windows VM pomoću upravitelja resursa i PowerShell](./virtual-machines-windows-ps-create.md). Korak kroz stvaranje grupa resursa, računa za pohranu i virtualne mreže, a zatim definirati vaše VM i na kraju Stvori vaše VM.
 
Najprije sigurno Pribavljanje vjerodajnica, postavite mjesto i naziv grupe resursa:

```
$cred = Get-Credential
$location = "West US"
$resourceGroupName = "TestLicensing"
```

Stvorili javnu IP učinite sljedeće:

```
$publicIPName = "testlicensingpublicip"
$publicIP = New-AzureRmPublicIpAddress -Name $publicIPName -ResourceGroupName $resourceGroupName -Location $location -AllocationMethod Dynamic
```

Definiranje vaše podmreže, SLIKA i VNET:

```
$subnetName = "testlicensingsubnet"
$nicName = "testlicensingnic"
$vnetName = "testlicensingvnet"
$subnetconfig = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/8
$vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroupName -Location $location -AddressPrefix 10.0.0.0/8 -Subnet $subnetconfig
$nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $resourceGroupName -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $publicIP.Id
```

Naziv vaše VM i stvorite VM config:

```
$vmName = "testlicensing"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A1"
```

Definiranje sustava OS:

```
$computerName = "testlicensing"
$vm = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $computerName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
```

Dodajte svoje NIC na VM:

```
$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

Definiranje prostora za pohranu račun koji želite koristiti:

```
$storageAcc = Get-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -AccountName testlicensing
```

Prijenos VHD, suitably pripremili i priložite vaše VM za korištenje:

```
$osDiskName = "licensing.vhd"
$osDiskUri = '{0}vhds/{1}{2}.vhd' -f $storageAcc.PrimaryEndpoints.Blob.ToString(), $vmName.ToLower(), $osDiskName
$urlOfUploadedImageVhd = "https://testlicensing.blob.core.windows.net/vhd/licensing.vhd"
$vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption FromImage -SourceImageUri $urlOfUploadedImageVhd -Windows
```

Na kraju, stvorite vaše VM i definirati licenciranja vrstu mogu koristiti prednosti za korištenje hibridnog Azure:

```
New-AzureRmVM -ResourceGroupName $resourceGroupName -Location $location -VM $vm -LicenseType Windows_Server
```

## <a name="next-steps"></a>Daljnji koraci

Saznajte više o [licenciranju pogodnost za korištenje hibridnog Azure](https://azure.microsoft.com/pricing/hybrid-use-benefit/).

Dodatne informacije o [korištenju predložaka Voditelj resursa](../azure-resource-manager/resource-group-overview.md).
