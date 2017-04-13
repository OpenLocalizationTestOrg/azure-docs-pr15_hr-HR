<properties
    pageTitle="Odvajanje disk podataka iz Windows VM | Microsoft Azure"
    description="Saznajte kako odvajanje disk podataka s virtualnog računala u Azure pomoću modela implementacije Voditelj resursa."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>



# <a name="how-to-detach-a-data-disk-from-a-windows-virtual-machine"></a>Upute za odvajanje disk podataka s virtualnog računala za Windows


Kada više ne trebate podatkovni disk koji je pridružen virtualnog računala, to možete jednostavno odvajanje. Time se uklanja disk iz virtualnog računala, ali se ne Ukloni iz spremišta. 

> [AZURE.WARNING] Ako odvajanje na disku koji se neće automatski izbrisan. Ako ste se pretplatili Premium prostora za pohranu, nastavit ćete plaćati naknade za pohranu za disk. Dodatne informacije potražite na [web-mjesto određivanje cijena i u okvir za naplatu prilikom korištenja Premium prostora za pohranu](../storage/storage-premium-storage.md#pricing-and-billing). 

Ako želite ponovno koristiti postojeće podatke na disku, možete ponovno priložiti isti virtualnog računala ili neku drugu.  


## <a name="detach-a-data-disk-using-the-portal"></a>Odvajanje na disku podataka pomoću portala

1. U središtu portala odaberite **virtualnim računalima**.

2. Odaberite virtualnog računala koja sadrži podatkovni disk koji želite odvojiti, a zatim kliknite **sve postavke**.

3. U plohu **Postavke** odaberite **diskova**.

4. Odaberite podatkovni disk koji želite odvojiti plohu **diskova** .

5. U plohu za podatkovni disk, kliknite **Odvoji**.


    ![Snimka zaslona s prikazom gumba Odvoji.](./media/virtual-machines-windows-detach-disk/detach-disk.png)

Disk ostaje u prostor za pohranu, ali više nije pridružen virtualnog računala.


## <a name="detach-a-data-disk-using-powershell"></a>Odvajanje podatkovni disk pomoću komponente PowerShell

U ovom primjeru prve naredbe dohvaća virtualnog računala pod nazivom **MyVM07** u grupi resursa **RG11** pomoću cmdleta Get-AzureRmVM. Naredba u varijablu **$VirtualMachine** pohranjuje virtualnog računala. 

Drugi naredba uklanja podatkovni disk pod nazivom DataDisk3 iz virtualnog računala. 

Naredba konačan ažurira stanje virtualnog računala da biste dovršili postupak uklanjanja podatkovni disk.

    $VirtualMachine = Get-AzureRmVM -ResourceGroupName "RG11" -Name "MyVM07" 
    Remove-AzureRmVMDataDisk -VM $VirtualMachine -Name "DataDisk3"
    Update-AzureRmVM -ResourceGroupName "RG11" -Name "MyVM07" -VM $VirtualMachine


Dodatne informacije potražite u članku [Uklanjanje AzureRmVMDataDisk](https://msdn.microsoft.com/library/mt603614.aspx)

## <a name="next-steps"></a>Daljnji koraci

Ako želite da biste ponovno koristili podatkovni disk, možete ga samo [priključite drugi VM](virtual-machines-windows-attach-disk-portal.md)
