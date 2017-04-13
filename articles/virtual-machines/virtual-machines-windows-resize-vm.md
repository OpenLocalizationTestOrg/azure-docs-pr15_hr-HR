<properties
    pageTitle="Promjena veličine Windows VM | Microsoft Azure"
    description="Promjena veličine na Windows virtualnog računala stvorene u model implementacije resursima pomoću komponente Powershell Azure."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="Drewm3"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="drewm"/>

    
# <a name="resize-a-windows-vm"></a>Promjena veličine Windows VM

U ovom se članku objašnjava kako biste promijenili veličinu Windows VM stvorene u model implementacije resursima pomoću komponente Powershell Azure.

Kada stvorite virtualnog računala (VM), koje mogu mijenjati veličinu na VM prema gore ili prema dolje tako da promijenite veličinu VM. U nekim slučajevima, morate najprije deallocate na VM. To se može dogoditi ako novu veličinu nije dostupan na klasteru hardver koji se trenutno nalaze u VM.

## <a name="resize-a-windows-vm-not-in-an-availability-set"></a>Promjena veličine VM Windows nije u skupu dostupnosti

1. Popis veličina VM koji su dostupni na klaster hardver gdje se nalazi na VM. 

    ```powershell
    Get-AzureRmVMSize -ResourceGroupName <resourceGroupName> -VMName <vmName> 
    ```

2. Ako je naveden do željene veličine, pokrenite sljedeće naredbe za promjenu veličine na VM. Ako željene veličine nije naveden, prijeđite na korak 3.

    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroupName> -VMName <vmName>
    $vm.HardwareProfile.VmSize = "<newVMsize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName <resourceGroupName>
    ```

3. Ako željene veličine nije naveden, pokrenite sljedeće naredbe deallocate VM, veličine i ponovno pokrenite na VM.

    ```powershell
    $rgname = "<resourceGroupName>"
    $vmname = "<vmName>"
    Stop-AzureRmVM -ResourceGroupName $rgname -VMName $vmname -Force
    $vm = Get-AzureRmVM -ResourceGroupName $rgname -VMName $vmname
    $vm.HardwareProfile.VmSize = "<newVMSize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName $rgname
    Start-AzureRmVM -ResourceGroupName $rgname -Name $vmname
    ```

> [AZURE.WARNING] Deallocating na VM izdaje dodijeljene u VM sve dinamičku IP adresu. Ne utječe na diskova OS i podatke. 

## <a name="resize-a-windows-vm-in-an-availability-set"></a>Promjena veličine VM Windows u skupu dostupnosti

Ako novu veličinu za VM u skupu dostupnost nije dostupan na klaster hardver koji se trenutno nalaze u VM, zatim sve VMs u odjeljku Postavljanje dostupnosti morat ćete biti deallocated da biste promijenili veličinu u VM. Možda morate ažurirati veličine druge VMs dostupnosti postaviti nakon smanjena jedan VM. Da biste promijenili veličinu VM u skupu dostupnost, poduzeti sljedeće korake.

1. Popis veličina VM koji su dostupni na klaster hardver gdje se nalazi na VM.

    ```powershell
    Get-AzureRmVMSize -ResourceGroupName <resourceGroupName> -VMName <vmName>
    ```

2. Ako je naveden do željene veličine, pokrenite sljedeće naredbe za promjenu veličine na VM. Ako nije naveden, idite na 3.

    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroupName> -VMName <vmName>
    $vm.HardwareProfile.VmSize = "<newVmSize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName <resourceGroupName>
    ```

3. Ako željene veličine nije naveden, nastavite sa sljedećim koracima deallocate sve VMs u odjeljku Postavljanje dostupnosti, promjena veličine VMs te ih ponovno pokrenuti.

4.  Zaustavite sve VMs u skupu dostupnost.

    ```powershell
    $rg = "<resourceGroupName>"
    $as = Get-AzureRmAvailabilitySet -ResourceGroupName $rg
    $vmIds = $as.VirtualMachinesReferences
    foreach ($vmId in $vmIDs){
        $string = $vmID.Id.Split("/")
        $vmName = $string[8]
        Stop-AzureRmVM -ResourceGroupName $rg -Name $vmName -Force
    } 
    ```
              
5.  Promjena veličine i ponovno pokrenite na VMs u skupu dostupnost.

    ```powershell
    $rg = "<resourceGroupName>"
    $newSize = "<newVmSize>"
    $as = Get-AzureRmAvailabilitySet -ResourceGroupName $rg
    $vmIds = $as.VirtualMachinesReferences
    foreach ($vmId in $vmIDs){
        $string = $vmID.Id.Split("/")
        $vmName = $string[8]
        $vm = Get-AzureRmVM -ResourceGroupName $rg -Name $vmName
        $vm.HardwareProfile.VmSize = $newSize
        Update-AzureRmVM -ResourceGroupName $rg -VM $vm
        Start-AzureRmVM -ResourceGroupName $rg -Name $vmName
    }
    ```

## <a name="next-steps"></a>Daljnji koraci

- Dodatni skalabilnost pokrenuti više instanci VM i Vremensko mjerilo. Dodatne informacije potražite u članku [automatski promijeniti omjer računalima sustava Windows u skupu na skaliranje virtualnog računala](../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md).



