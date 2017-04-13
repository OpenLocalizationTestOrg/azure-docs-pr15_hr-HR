<properties
    pageTitle="Upravljanje VMs pomoću upravitelja resursa i PowerShell | Microsoft Azure"
    description="Upravljanje virtualnim strojevima pomoću upravitelja resursa Azure i PowerShell."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="davidmu"/>

# <a name="manage-azure-virtual-machines-using-resource-manager-and-powershell"></a>Upravljanje virtualnim računalima sustava Azure pomoću upravitelja resursa i komponente PowerShell

## <a name="install-azure-powershell"></a>Instaliranje modula Azure PowerShell
 
Informacije o instalacija najnovije verzije sustava Azure PowerShell, Odabir pretplate i prijave na račun potražite u članku [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md) .

## <a name="set-variables"></a>Postavljanje varijable

Sve naredbe u članku potreban je naziv grupe resursa u kojem se nalazi virtualnog računala i virtualnog računala da biste upravljali. Zamijenite vrijednost **$rgName** naziv grupe resursa koji sadrži virtualnog računala. Zamijenite vrijednost **$vmName** naziv u VM. Stvaranje varijabli.

    $rgName = "resource-group-name"
    $vmName = "VM-name"

## <a name="display-information-about-a-virtual-machine"></a>Prikaz informacija o virtualnog računala

Dohvaćanje podataka virtualnog računala.
  
    Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName

Vraća nešto kao u ovom primjeru:

    ResourceGroupName        : rg1
    Id                       : /subscriptions/{subscription-id}/resourceGroups/
                               rg1/providers/Microsoft.Compute/virtualMachines/vm1
    Name                     : vm1
    Type                     : Microsoft.Compute/virtualMachines
    Location                 : centralus
    Tags                     : {}
    AvailabilitySetReference : {
                                  "id": "/subscriptions/{subscription-id}/resourceGroups/
                                  rg1/providers/Microsoft.Compute/availabilitySets/av1"
                               }
    Extensions               : []
    HardwareProfile          : {
                                  "vmSize": "Standard_A0"
                               }
    InstanceView             : null
    NetworkProfile           : {
                                  "networkInterfaces": [
                                    {
                                      "properties.primary": null,
                                      "id": "/subscriptions/{subscription-id}/resourceGroups/
                                      rg1/providers/Microsoft.Network/networkInterfaces/nc1"
                                    }
                                  ]
                               }
    OSProfile                : {
                                  "computerName": "vm1",
                                  "adminUsername": "myaccount1",
                                  "adminPassword": null,
                                  "customData": null,
                                  "windowsConfiguration": {
                                    "provisionVMAgent": true,
                                    "enableAutomaticUpdates": true,
                                    "timeZone": null,
                                    "additionalUnattendContents": [],
                                    "winRM": null
                                  },
                                  "linuxConfiguration": null,
                                  "secrets": []
                               }
    Plan                     : null
    ProvisioningState        : Succeeded
    StorageProfile           : {
                                  "imageReference": {
                                    "publisher": "MicrosoftWindowsServer",
                                    "offer": "WindowsServer",
                                    "sku": "2012-R2-Datacenter",
                                    "version": "latest"
                                  },
                                  "osDisk": {
                                    "osType": "Windows",
                                    "encryptionSettings": null,
                                    "name": "osdisk",
                                    "vhd": {
                                      "Uri": "http://sa1.blob.core.windows.net/vhds/osdisk1.vhd"
                                    }
                                    "image": null,
                                    "caching": "ReadWrite",
                                    "createOption": "FromImage"
                                  }
                                  "dataDisks": [],
                               }
    DataDiskNames            : {}
    NetworkInterfaceIDs      : {/subscriptions/{subscription-id}/resourceGroups/
                                rg1/providers/Microsoft.Network/networkInterfaces/nc1}

## <a name="stop-a-virtual-machine"></a>Zaustavljanje virtualnog računala

Zaustavljanje pokrenute virtualnog računala.

    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName

Zatraži potvrdu:

    Virtual machine stopping operation
    This cmdlet will stop the specified virtual machine. Do you want to continue?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
        
Unesite **Y** da biste prestali virtualnog računala.

Nakon nekoliko minuta, vratit će otprilike ovako u ovom primjeru:

    StatusCode : Succeeded
    StartTime  : 9/13/2016 12:11:57 PM
    EndTime    : 9/13/2016 12:14:40 PM

## <a name="start-a-virtual-machine"></a>Pokretanje virtualnog računala

Ako je zaustavljen, pokrenite virtualnog računala.

    Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName

Nakon nekoliko minuta, vratit će otprilike ovako u ovom primjeru:

    StatusCode : Succeeded
    StartTime  : 9/13/2016 12:32:55 PM
    EndTime    : 9/13/2016 12:35:09 PM

Ako želite ponovno pokrenuti virtualnog računala na kojem je već pokrenut, korištenje **Ponovno pokretanje AzureRmVM** opisane dalje.

## <a name="restart-a-virtual-machine"></a>Ponovno pokrenite virtualnog računala

Ponovno pokrenite izvodi virtualnog računala.

    Restart-AzureRmVM -ResourceGroupName $rgName -Name $vmName

Vraća nešto kao u ovom primjeru:

    StatusCode : Succeeded
    StartTime  : 9/13/2016 12:54:40 PM
    EndTime    : 9/13/2016 12:55:54 PM

## <a name="delete-a-virtual-machine"></a>Brisanje virtualnog računala

Brisanje virtualnog računala.  

    Remove-AzureRmVM -ResourceGroupName $rgName –Name $vmName

> [AZURE.NOTE] Možete koristiti u **– prisilno** parametar da biste preskočili odzivnik za potvrdu.

Ako niste koristili-parametar prisilno Zatraži potvrdu:

    Virtual machine removal operation
    This cmdlet will remove the specified virtual machine. Do you want to continue?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):

Vraća nešto kao u ovom primjeru:

    RequestId  IsSuccessStatusCode  StatusCode  ReasonPhrase
    ---------  -------------------  ----------  ------------
                              True          OK  OK

## <a name="update-a-virtual-machine"></a>Ažuriranje virtualnog računala

U ovom se primjeru prikazuje kako ažurirati veličina virtualnog računala.
        
    $vmSize = "Standard_A1"
    $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    $vm.HardwareProfile.vmSize = $vmSize
    Update-AzureRmVM -ResourceGroupName $rgName -VM $vm
    
Vraća nešto kao u ovom primjeru:

    RequestId  IsSuccessStatusCode  StatusCode  ReasonPhrase
    ---------  -------------------  ----------  ------------
                              True          OK  OK
                              
Popis dostupnih veličine za virtualnog računala potražite u članku [veličine za virtualnim strojevima u Azure](virtual-machines-windows-sizes.md) .

## <a name="add-a-data-disk-to-a-virtual-machine"></a>Dodavanje podatkovni disk virtualnog računala

U ovom se primjeru prikazuje kako dodati na disku podataka u postojeće virtualnog računala.

    $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    Add-AzureRmVMDataDisk -VM $vm -Name "disk-name" -VhdUri "https://mystore1.blob.core.windows.net/vhds/datadisk1.vhd" -LUN 0 -Caching ReadWrite -DiskSizeinGB 1 -CreateOption Empty
    Update-AzureRmVM -ResourceGroupName $rgName -VM $vm

Disk koji ste dodali nije pokrenut. Pokretanje disk, prijavite se na njega i koristiti disk management. Ako ste instalirali WinRM i certifikat na njemu kada je stvoren, možete koristiti udaljene ljuske PowerShell za inicijalizaciju disk. Možete koristiti i nastavkom prilagođene skripte: 

    $location = "location-name"
    $scriptName = "script-name"
    $fileName = "script-file-name"
    Set-AzureRmVMCustomScriptExtension -ResourceGroupName $rgName -Location $locName -VMName $vmName -Name $scriptName -TypeHandlerVersion "1.4" -StorageAccountName "mystore1" -StorageAccountKey "primary-key" -FileName $fileName -ContainerName "scripts"

Datoteka skripte mogu sadržavati nešto poput kod za inicijalizaciju na diskova:

    $disks = Get-Disk |   Where partitionstyle -eq 'raw' | sort number

    $letters = 70..89 | ForEach-Object { ([char]$_) }
    $count = 0
    $labels = @("data1","data2")

    foreach($d in $disks) {
        $driveLetter = $letters[$count].ToString()
        $d | 
        Initialize-Disk -PartitionStyle MBR -PassThru |
        New-Partition -UseMaximumSize -DriveLetter $driveLetter |
        Format-Volume -FileSystem NTFS -NewFileSystemLabel $labels[$count] `
            -Confirm:$false -Force 
        $count++
    }

## <a name="next-steps"></a>Daljnji koraci

Ako postoje problemi s implementacije, možda pogledajte [Otklanjanje poteškoća resursa grupe implementacijama pomoću portala za Azure](../resource-manager-troubleshoot-deployments-portal.md)
