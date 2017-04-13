<properties 
   pageTitle="Uobičajene naredbe ljuske PowerShell za VMs | Microsoft Azure"
   description="Uobičajene naredbe ljuske PowerShell za početak stvaranja i upravljanje vaše VMs u Azure u sustavu Windows"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="davidmu1" 
   manager="timlt" 
   editor="tysonn" 
   tags="azure-resource-manager"/>
   
<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="09/27/2016"
   ms.author="davidmu" />

# <a name="common-powershell-commands-for-creating-and-managing-vms"></a>Uobičajene naredbe ljuske PowerShell za stvaranje i upravljanje VMs

U ovom se članku opisuje neke naredbe ljuske PowerShell Azure koje možete koristiti za stvaranje i upravljanje virtualnih računala u vašoj pretplati Azure.  Detaljniju pomoć s određenim skretnice i mogućnosti možete koristiti **Potražite pomoć** *naredbi*.

Informacije o instalacija najnovije verzije sustava Azure PowerShell, Odabir pretplate i prijave na račun potražite u članku [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md) .

## <a name="create-a-vm"></a>Stvaranje na VM

Zadatak | Naredba
-------------- | -------------------------
Stvaranje VM konfiguracija | $vm = [Novo AzureRmVMConfig](https://msdn.microsoft.com/library/mt603727.aspx) - VMName "vm_name" - VMSize "vm_size"<BR></BR><BR></BR>Konfiguriranje VM koristi se za definiranje i ažuriranje postavki za na VM. Konfiguracija je pokrenut s nazivom na VM i njegovu [veličinu](virtual-machines-windows-sizes.md).
Dodavanje konfiguracija postavki | $vm = [Skup AzureRmVMOperatingSystem](https://msdn.microsoft.com/library/mt603843.aspx) - VM $vm – Windows – ComputerName "Naziv_računala"-vjerodajnica $cred - ProvisionVMAgent - EnableAutoUpdate<BR></BR><BR></BR>Operacijski sustav postavke uključujući [vjerodajnice](https://technet.microsoft.com/library/hh849815.aspx) dodaju konfiguracije objekta koji ste prethodno stvorili pomoću novo AzureRmVMConfig.
Dodavanje mrežnog sučelja | $vm = [Dodaj AzureRmVMNetworkInterface](https://msdn.microsoft.com/library/mt619351.aspx) - VM $vm-nic. $ ID-a ID-a<BR></BR><BR></BR>Na VM mora imati [mrežno sučelje](virtual-machines-windows-ps-create.md) za komunikaciju u virtualne mreže. [Get-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619434.aspx) možete koristiti i za dohvaćanje postojećeg objekta sučelja mreže.
Navedite sliku platforme | $vm = [Skup AzureRmVMSourceImage](https://msdn.microsoft.com/library/mt619344.aspx) - VM $vm - PublisherName "publisher_name"-nude "publisher_offer" - SKU-ove "product_sku"-"najnoviji" verzija<BR></BR><BR></BR>[Slika podaci](virtual-machines-windows-cli-ps-findimage.md) se dodaju u konfiguraciji objekta koji ste prethodno stvorili pomoću novo AzureRmVMConfig. Objekt koji je vratio ta naredba koristi samo kada postavite OS disk da biste koristili sliku platforme.
Postavljanje OS disk da biste koristili sliku platforme | $vm = [Skup AzureRmVMOSDisk](https://msdn.microsoft.com/library/mt603746.aspx) - VM $vm-naziv "disk_name" - VhdUri "http://mystore1.blob.core.windows.net/vhds/disk_name.vhd" - CreateOption FromImage<BR></BR><BR></BR>Naziv operacijski sustav disku i njegova mjesta u [prostor za pohranu](../storage/storage-powershell-guide-full.md) dodaje se konfiguracije objekta koji ste prethodno stvorili.
Postavljanje OS disk da biste koristili sliku generalizirano | $vm = skup AzureRmVMOSDisk - VM $vm-naziv "disk_name" - SourceImageUri "https://mystore1.blob.core.windows.net/system/Microsoft.Compute/Images/myimages/myprefix-osDisk. .vhd {guid}"- VhdUri"https://mystore1.blob.core.windows.net/vhds/disk_name.vhd"- CreateOption FromImage-Windows<BR></BR><BR></BR>Naziv diska operacijski sustav, mjesto izvorne slike i mjesto na disku u [prostor za pohranu](../storage/storage-powershell-guide-full.md) dodaje se objekt konfiguracije.
Postavljanje OS disk da biste koristili sliku specijalizirane | $vm = skup AzureRmVMOSDisk - VM $vm-naziv "name_of_disk" - VhdUri "http://mystore1.blob.core.windows.net/vhds/" - CreateOption priložiti - Windows
Stvaranje na VM | [Novo AzureRmVM]() - ResourceGroupName "resource_group_name"-mjesto "location_name" - VM $vm<BR></BR><BR></BR>Svi resursi stvaraju se u [grupu resursa](../powershell-azure-resource-manager.md). Na VM moraju se stvoriti na isto [mjesto](https://msdn.microsoft.com/library/azure/dn495177.aspx) kao grupu resursa. Prije pokretanja naredbe, pokrenite novo AzureRmVMConfig, skup AzureRmVMOperatingSystem, skup AzureRmVMSourceImage, dodavanje AzureRmVMNetworkInterface i postavljanje AzureRmVMOSDisk.

## <a name="get-information-about-vms"></a>Informacije o VMs

Zadatak | Naredba
-------------- | -------------------------
Popis VMs u pretplatu| [Get-AzureRmVM](https://msdn.microsoft.com/library/mt603718.aspx)
Popis VMs u grupu resursa | Get-AzureRmVM - ResourceGroupName "resource_group_name"<BR></BR><BR></BR>Da biste dobili popis grupa resursa u vašoj pretplati, koristite [Get-AzureRmResourceGroup](https://msdn.microsoft.com/library/mt679016.aspx).
Informacije o na VM | Get-AzureRmVM - ResourceGroupName "resource_group_name"-naziv "vm_name"

## <a name="manage-vms"></a>Upravljanje VMs

Zadatak | Naredba
-------------- | -------------------------
Pokretanje programa VM | [Početak AzureRmVM](https://msdn.microsoft.com/library/mt603453.aspx) - ResourceGroupName "resource_group_name"-naziv "vm_name"
Zaustavljanje na VM | [Zaustavi AzureRmVM](https://msdn.microsoft.com/library/mt603483.aspx) - ResourceGroupName "resource_group_name"-naziv "vm_name"
Ponovno pokrenite izvodi VM | [Ponovno pokretanje AzureRmVM](https://msdn.microsoft.com/library/mt603775.aspx) - ResourceGroupName "resource_group_name"-naziv "vm_name"
Brisanje na VM | [Uklanjanje AzureRmVM](https://msdn.microsoft.com/library/mt603641.aspx) - ResourceGroupName "resource_group_name"-naziv "vm_name"
Generalize na VM | [Postavljanje AzureRmVm](https://msdn.microsoft.com/library/mt603688.aspx) - ResourceGroupName YourResourceGroup-naziv "vm_name"-GRG Generalizirano<BR></BR><BR></BR>Prije nego što pokrenete Spremi AzureRmVMImage, pokrenite sljedeću naredbu.
Snimite na VM | [Spremanje AzureRmVMImage](https://msdn.microsoft.com/library/mt619423.aspx) - ResourceGroupName "resource_group_name" - VMName "vm_name" - DestinationContainerName "image_container" - VHDNamePrefix "image_name_prefix"-put "C:\filepath\filename.json"<BR></BR><BR></BR>Virtualno računalo mora biti [isključiti i GRG generalizirano](virtual-machines-windows-generalize-vhd.md) koja će se koristiti za stvaranje slike. Prije pokretanja naredbe, pokrenite postavljanje AzureRmVm.
Ažuriranje programa VM | [Ažuriranje AzureRmVM](https://msdn.microsoft.com/library/mt603662.aspx) - ResourceGroupName "resource_group_name" - VM $vm<BR></BR><BR></BR>Dobili trenutnu konfiguraciju VM pomoću Get-AzureRmVM, promijenite postavke konfiguracije na objekt VM, a zatim pokrenite sljedeću naredbu.
Dodavanje podatkovni disk na VM | [Dodavanje AzureRmVMDataDisk](https://msdn.microsoft.com/library/mt603673.aspx) - VM $vm-naziv "disk_name" - VhdUri "https://mystore1.blob.core.windows.net/vhds/disk_name.vhd" - LUN #-predmemoriranje ReadWrite - DiskSizeinGB # - CreateOption Isprazni<BR></BR><BR></BR>Da biste pristupili VM objekta, slijedite Get-AzureRmVM. Navedite broj LUN i veličinu diska. Pokrenite ažuriranja AzureRmVM da biste primijenili promjene konfiguracije na VM. Disk koji ste dodali nije pokrenut. Informacije o Inicijalizacija diskova tijekom njihova dodavanja, potražite u članku [Upravljanje virtualnim računalima sustava Azure pomoću upravitelja resursa i PowerShell](virtual-machines-windows-ps-manage.md).
Uklanjanje podatkovni disk na VM | [Uklanjanje AzureRmVMDataDisk](https://msdn.microsoft.com/library/mt603614.aspx) - VM $vm-naziv "disk_name"<BR></BR><BR></BR>Da biste pristupili VM objekta, slijedite Get-AzureRmVM. Pokrenite ažuriranje AzureRmVM da biste primijenili promjene konfiguracije na VM.
Dodavanje datotečni nastavak u VM | [Postavljanje AzureRmVMExtension](https://msdn.microsoft.com/library/mt603745.aspx) - ResourceGroupName "resource_group_name"-mjesto "azure_location" - VMName "vm_name"-naziv "extension_name"-Publisher "publisher_name"-Vrsta "extension_type" - TypeHandlerVersion "#. #"-postavke $Settings - ProtectedSettings $ProtectedSettings<BR></BR><BR></BR>Pokrenite sljedeću naredbu odgovarajućim [informacije o konfiguraciji](virtual-machines-windows-extensions-configuration-samples.md) za kućni broj koji želite instalirati.
Uklanjanje nastavkom VM | [Uklanjanje AzureRmVMExtension](https://msdn.microsoft.com/library/mt603782.aspx) - ResourceGroupName "resource_group_name"-naziv "extension_name" - VMName "vm_name"

## <a name="next-steps"></a>Daljnji koraci

- Osnovni koraci za stvaranje virtualnog računala na [Stvaranje Windows VM pomoću upravitelja resursa i PowerShell](virtual-machines-windows-ps-create.md)potražite u članku.

