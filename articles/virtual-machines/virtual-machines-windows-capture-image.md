<properties
    pageTitle="Snimite sliku VM iz generalizirano Azure VM | Microsoft Azure"
    description="Saznajte kako snimiti VM iz generalizirano Azure VM stvorene u model implementacije Voditelj resursa"
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
    ms.date="10/20/2016"
    ms.author="cynthn"/>

# <a name="how-to-capture-a-vm-image-from-a-generalized-azure-vm"></a>Kako snimiti VM iz generalizirano VM Azure


U ovom se članku objašnjava korištenje Azure PowerShell da biste stvorili sliku generalizirano VM Azure. Sliku možete koristiti da biste stvorili drugi VM. Slika sadrži OS disku i diskova podataka koje su priložene virtualnog računala. Sliku ne obuhvaća resursi virtualne mreže da morate postaviti tih resursa prilikom stvaranja novog VM. 


## <a name="prerequisites"></a>Preduvjeti

- Morate koristiti već [GRG generalizirano na VM](virtual-machines-windows-generalize-vhd.md). Generalizing na VM uklanja sve osobnih podataka o računu, između ostalog, web-mjesta i Priprema računala koja će se koristiti kao sliku.

- Potrebne su vam verzije Azure PowerShell 1.0.x ili noviji instaliran. Ako već niste instalirali PowerShell, pročitajte [upute za instalaciju i konfiguriranje Azure PowerShell](../powershell-install-configure.md) za korake za instalaciju.


## <a name="log-in-to-azure-powershell"></a>Prijavite se u sustav Azure PowerShell

1. Otvorite Azure PowerShell i prijavite se na račun za Azure.

    ```powershell
    Login-AzureRmAccount
    ```

    Otvorit će se skočni prozor za da unesete vjerodajnice za račun za Azure.

2. Pronađite pretplate ID-a za pretplate dostupna.

    ```powershell
    Get-AzureRmSubscription
    ```

3. Postavljanje odgovarajuće pretplate pomoću ID pretplate

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

## <a name="deallocate-the-vm-and-set-the-state-to-generalized"></a>Deallocate na VM i postavljanje stanja GRG generalizirano       

1. Deallocate VM resursi.

    ```powershell
    Stop-AzureRmVM -ResourceGroupName <resourceGroup> -Name <vmName>
    ```

    *Status* VM na portalu za Azure mijenja iz **prekine** u **zaustavljen (deallocated)**.

2. Postavljanje statusa virtualnog računala da biste **Generalized**. 

    ```powershell
    Set-AzureRmVm -ResourceGroupName <resourceGroup> -Name <vmName> -Generalized
    ```

3. Provjerite status u VM. U odjeljku **OSState/GRG generalizirano** na VM moraju imati **DisplayStatus** postavljena na **GRG generalizirano VM**.  

    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroup> -Name <vmName> -Status
    $vm.Statuses
    ```

## <a name="create-the-image"></a>Stvaranje slike 

1. Kopiranje slike virtualnog računala spremnik za pohranu odredište pomoću ove naredbe. Slika se stvara u isti prostor za pohranu račun kao izvornu virtualnog računala. Na `-Path` parametar sprema kopiju predloška JSON lokalno. Na `-DestinationContainerName` parametar je naziv spremnika u koji želite da držite slike. Ako ne postoji spremnik, stvaranja umjesto vas.

    ```powershell
    Save-AzureRmVMImage -ResourceGroupName <resourceGroupName> -Name <vmName> `
        -DestinationContainerName <destinationContainerName> -VHDNamePrefix <templateNamePrefix> `
        -Path <C:\local\Filepath\Filename.json>
    ```

    URL-a sliku možete pristupiti iz predloška datoteke JSON. Idite na **resurse** > **storageProfile** > **osDisk** > **Slika** > odjeljak**uri** za potpuni put sliku. URL slike izgleda ovako: `https://<storageAccountName>.blob.core.windows.net/system/Microsoft.Compute/Images/<imagesContainer>/<templatePrefix-osDisk>.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`.
    
    Možete provjeriti i URI na portalu. Slika se kopira kontejner **sustava** na vašem računu za pohranu. 


## <a name="next-steps"></a>Daljnji koraci

- Sada možete [stvoriti VM iz slike](virtual-machines-windows-create-vm-generalized.md).

