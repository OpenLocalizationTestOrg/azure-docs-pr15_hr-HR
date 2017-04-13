<properties
    pageTitle="Prijenos Windows VHD Voditelj resursa za | Microsoft Azure"
    description="Saznajte kako prenijeti na Windows virtualnog računala VHD lokalnim za Azure, pomoću modela implementacije Voditelj resursa. Možete prenijeti VHD iz ili na generalizirano ili specijalizirane VM."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="cynthn"/>

# <a name="upload-a-windows-vhd-from-an-on-premises-vm-to-azure"></a>Prijenos Windows VHD iz programa VM lokalnog Azure 


U ovom se članku prikazuje kako stvoriti i prenijeti Windows virtualne tvrdog diska (VHD) koja će se koristiti u stvaranju programa Azure Vm. Možete prenijeti VHD iz generalizirano VM ili specijalizirane VM. 

Dodatne informacije o i VHDs u Azure potražite u članku [o i VHDs za virtualnim računalima](virtual-machines-linux-about-disks-vhds.md).


## <a name="prepare-the-vm"></a>Priprema za VM 

Možete prenijeti generalizirano i specijalizirane VHDs Azure. Svaka vrsta potreban je na VM pripremiti prije pokretanja.

- **GRG generalizirano VHD** - generalizirano VHD je prodao svim vašim podacima osobnog računa ukloniti pomoću Sysprep. Ako namjeravate koristiti u VHD kao sliku da biste stvorili novi VMs iz, trebali biste:
    - [Priprema VHD Windows da biste prenijeli na Azure](virtual-machines-windows-prepare-for-upload-vhd-image.md). 
    - [Konf virtualnog računala pomoću Sysprep](virtual-machines-windows-generalize-vhd.md). 

- **Specijalizirane VHD** - specijalizirane VHD održava korisničkih računa, aplikacija i druge podatke o stanju iz vaše izvorne VM. Ako namjeravate koristiti VHD kao-je da biste stvorili novi VM, provjerite je li izvršiti sljedeće korake. 
    - [Priprema VHD Windows da biste prenijeli na Azure](virtual-machines-windows-prepare-for-upload-vhd-image.md). **Ne šalji** generalize VM koristi Sysprep.
    - Uklonite sve goste virtualizacije Alati i agenata koji su instalirani na VM (odnosno VMware Alati).
    - Provjerite je li u VM konfiguriran za izvlačenje njegove IP adresa i postavke DNS-a putem DHCP. Time se osigurava da poslužitelj dohvaća IP adresu unutar na VNet prilikom pokretanja prema gore. 

## <a name="log-in-to-azure"></a>Prijavite se na Azure

Ako još nemate PowerShell verzije 1.4 ili iznad instalirani, Saznajte [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md).

1. Otvorite Azure PowerShell i prijavite se na račun za Azure. Otvorit će se skočni prozor za da unesete vjerodajnice za račun za Azure.

    ```powershell
    Login-AzureRmAccount
    ```


2. Pronađite pretplate ID-a za pretplate dostupna.

    ```powershell
    Get-AzureRmSubscription
    ```

3. Postavljanje odgovarajuće pretplate pomoću ID pretplate Zamjena `<subscriptionID>` ID odgovarajuće pretplate.

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

## <a name="get-the-storage-account"></a>Nabavljanje računa za pohranu

Potreban račun za pohranu u Azure za spremanje prenesenih VM slike. Možete koristiti postojeći račun za pohranu ili stvorite novi. 

Da biste prikazali računi dostupan prostor za pohranu, upišite:

```powershell
Get-AzureRmStorageAccount
```

Ako želite koristiti postojeći račun za pohranu, prijeđite na odjeljak [Prijenos slike VM](#upload-the-vm-vhd-to-your-storage-account) .

Ako vam je potrebna za stvaranje računa za pohranu, slijedite ove korake:

1. Potreban vam je naziv grupe resursa gdje treba stvoriti račun za pohranu. Da biste saznali svih grupa resursa koji su u vašoj pretplati, upišite:

    ```powershell
    Get-AzureRmResourceGroup
    ```

Da biste stvorili grupu resursa pod nazivom **myResourceGroup** u regiji **Zapad SAD -a** , upišite:

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location "West US"
    ```

2. Stvaranje računa za pohranu pod nazivom **mystorageaccount** u ovu grupu resursa pomoću cmdleta [New-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) :

    ```powershell
    New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "West US" -SkuName "Standard_LRS" -Kind "Storage"
    ```
            
    Valjane vrijednosti za - SkuName su:

    - **Standard_LRS** – lokalno suvišnih prostora za pohranu. 
    - **Standard_ZRS** - Zone suvišnih prostora za pohranu.
    - **Standard_GRS** - zemlj suvišnih prostora za pohranu. 
    - **Standard_RAGRS** - suvišnih prostora za pohranu čitanje zemlj.. 
    - **Premium_LRS** - Premium lokalno suvišnih prostora za pohranu. 



## <a name="upload-the-vhd-to-your-storage-account"></a>Prijenos na VHD na račun servisa za pohranu

Pomoću cmdleta [Dodaj AzureRmVhd](https://msdn.microsoft.com/library/mt603554.aspx) prijenos slike u spremniku na vašem računu za pohranu. U ovom se primjeru prenese datoteka **myVHD.vhd** iz `"C:\Users\Public\Documents\Virtual hard disks\"` u spremište račun pod nazivom **mystorageaccount** u grupi **myResourceGroup** resursa. Datoteka će se nalaziti u kontejner **mycontainer** , a novi naziv datoteke će biti **myUploadedVHD.vhd**.

```powershell
$rgName = "myResourceGroup"
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $rgName -Destination $urlOfUploadedImageVhd -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


Ako ne uspije, dobiti odgovor koji izgleda ovako:

```
  C:\> Add-AzureRmVhd -ResourceGroupName myResourceGroup -Destination https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
  MD5 hash is being calculated for the file C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd.
  MD5 hash calculation is completed.
  Elapsed time for the operation: 00:03:35
  Creating new page blob of size 53687091712...
  Elapsed time for upload: 01:12:49

  LocalFilePath           DestinationUri
  -------------           --------------
  C:\Users\Public\Doc...  https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd
```

Ovisno o mrežnu vezu i veličinu datoteke VHD, ta se naredba može potrajati neko vrijeme da biste dovršili


## <a name="next-steps"></a>Daljnji koraci

- [Stvaranje na VM u Azure s generalizirano VHD](virtual-machines-windows-create-vm-generalized.md)
- [Stvaranje VM u Azure s specijalizirane VHD](virtual-machines-windows-create-vm-specialized.md) prilaganja kao na disk OS prilikom stvaranja novog VM.


