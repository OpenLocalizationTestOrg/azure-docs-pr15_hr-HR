<properties
   pageTitle="Implementacija višestruki NIC VMs pomoću komponente PowerShell u upravitelju resursa | Microsoft Azure"
   description="Saznajte kako uvesti više NIC VMs pomoću komponente PowerShell u upravitelju resursa"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

#<a name="deploy-multi-nic-vms-using-powershell"></a>Implementacija višestruki NIC VMs pomoću komponente PowerShell

[AZURE.INCLUDE [virtual-network-deploy-multinic-arm-selectors-include.md](../../includes/virtual-network-deploy-multinic-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-deploy-multinic-intro-include.md](../../includes/virtual-network-deploy-multinic-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][uvođenje klasičnog modela](virtual-network-deploy-multinic-classic-ps.md).

[AZURE.INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

Trenutno ne možete imati VMs s jednom NIC i VMs s više NIC-ovi isti skup dostupnost. Stoga ćete morati implementirati pozadinskih poslužitelja u grupu resursa za različite od drugih komponenti. Koraci u nastavku koristiti grupu resursa pod nazivom *IaaSStory* za grupu glavnom resursa i *IaaSStory pozadinskog* za pozadinskih poslužitelja.

## <a name="prerequisites"></a>Preduvjeti

Prije nego što možete implementirati pozadinskih poslužitelja, morate uvesti grupe glavnom resursa s sve potrebne resurse za taj scenarij. Da biste implementirali tih resursa, slijedite korake u nastavku.

1. Dođite do [stranice za predložak](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).
2. Na stranici predložak s desne strane **nadređenu grupu resursa**, zatim **Implementiraj za Azure**.
3. Ako je potrebno, promijenite vrijednosti parametra, a zatim slijedite korake na portalu Azure pretpregled za implementaciju u grupu resursa.

> [AZURE.IMPORTANT] Provjerite jesu li imena pohranu računa jedinstveni. Ne možete imati nazive računa dupliciranih prostora za pohranu Azure.

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="deploy-the-back-end-vms"></a>Implementacija pozadinskih VMs

VMs pozadinskog ovise o stvaranje resursa navedena u nastavku.

- **Račun za pohranu za diskova podataka**. Bolje performanse diskova podataka na poslužitelje baza podataka će koristiti pune stanje pogon (SSD) tehnologiju koja potreban je račun za pohranu premium. Provjerite je li Azure mjesto implementacije podržava premium prostora za pohranu.
- **NIC-ovi**. Svaki VM će imati dvije NIC-ovi, jedan za pristup bazi podataka, a jedan za upravljanje.
- **Postavljanje dostupnosti**. Dodat će se poslužiteljima baze podataka na jednom dostupnost postavili, da biste bili sigurni barem jedno od na VMs prema gore i pokretanje tijekom održavanja.  

### <a name="step-1---start-your-script"></a>Korak 1 – pokrenuti skriptu

Možete preuzeti cijeli skriptu PowerShell koristi [ovdje](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/arm/virtual-network-deploy-multinic-arm-ps.ps1). Slijedite korake u nastavku da biste promijenili skripta za rad u svom okruženju.

[AZURE.INCLUDE [powershell-preview-include.md](../../includes/powershell-preview-include.md)]

1. Promijenite vrijednosti varijabli ispod koji se temelji na postojeću grupu resursa u [preduvjeti](#Prerequisites)implementirana iznad.

        $existingRGName        = "IaaSStory"
        $location              = "West US"
        $vnetName              = "WTestVNet"
        $backendSubnetName     = "BackEnd"
        $remoteAccessNSGName   = "NSG-RemoteAccess"
        $stdStorageAccountName = "wtestvnetstoragestd"

2. Promijenite vrijednosti varijabli dolje na temelju vrijednosti koje želite koristiti za implementaciju sustava pozadinskog.

        $backendRGName         = "IaaSStory-Backend"
        $prmStorageAccountName = "wtestvnetstorageprm"
        $avSetName             = "ASDB"
        $vmSize                = "Standard_DS3"
        $publisher             = "MicrosoftSQLServer"
        $offer                 = "SQL2014SP1-WS2012R2"
        $sku                   = "Standard"
        $version               = "latest"
        $vmNamePrefix          = "DB"
        $osDiskPrefix          = "osdiskdb"
        $dataDiskPrefix        = "datadisk"
        $diskSize              = "120"  
        $nicNamePrefix         = "NICDB"
        $ipAddressPrefix       = "192.168.2."
        $numberOfVMs           = 2

3. Dohvaćanje resursima postojeće koji su potrebni za implementaciju sustava.

        $vnet                  = Get-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $existingRGName
        $backendSubnet         = $vnet.Subnets|?{$_.Name -eq $backendSubnetName}
        $remoteAccessNSG       = Get-AzureRmNetworkSecurityGroup -Name $remoteAccessNSGName -ResourceGroupName $existingRGName
        $stdStorageAccount     = Get-AzureRmStorageAccount -Name $stdStorageAccountName -ResourceGroupName $existingRGName

### <a name="step-2---create-necessary-resources-for-your-vms"></a>Korak 2 – da biste stvorili potrebne resurse za vaše VMs

Morate stvoriti novu grupu resursa, s računom za pohranu za diskova podataka i postavljanje za sve VMs raspoloživost. Alos morate lokalne administratorske vjerodajnice računa za svaki VM. Da biste stvorili tih resursa, izvršiti sljedeće korake.

1. Stvorite novu grupu resursa.

        New-AzureRmResourceGroup -Name $backendRGName -Location $location

2. Stvaranje novog računa za pohranu premium u grupi resursa koja je stvorena iznad.

        $prmStorageAccount = New-AzureRmStorageAccount -Name $prmStorageAccountName `
            -ResourceGroupName $backendRGName -Type Premium_LRS -Location $location

3. Stvaranje novog skupa dostupnost.

        $avSet = New-AzureRmAvailabilitySet -Name $avSetName -ResourceGroupName $backendRGName -Location $location

4. Pronađite lokalni administrator vjerodajnice računa koja će se koristiti za svaki VM.

        $cred = Get-Credential -Message "Type the name and password for the local administrator account."

### <a name="step-3---create-the-nics-and-backend-vms"></a>Korak 3 – stvaranje NIC-ovi i pozadinskog VMs

Morate koristiti petlje da biste stvorili proizvoljan broj VMs kao i stvaranje potrebne NIC-ovi i VMs unutar petlje. Da biste stvorili NIC-ovi i VMs, izvršiti sljedeće korake.

1. Pokretanje programa `for` Ponavljaj Ponavljanje naredbe za stvaranje na VM i dva NIC-ovi kao proizvoljan broj puta, na temelju vrijednosti u `$numberOfVMs` varijablu.

        for ($suffixNumber = 1; $suffixNumber -le $numberOfVMs; $suffixNumber++){

2. Stvaranje NIC koji se koristi za pristup bazi podataka.

            $nic1Name = $nicNamePrefix + $suffixNumber + "-DA"
            $ipAddress1 = $ipAddressPrefix + ($suffixNumber + 3)
            $nic1 = New-AzureRmNetworkInterface -Name $nic1Name -ResourceGroupName $backendRGName `
                -Location $location -SubnetId $backendSubnet.Id -PrivateIpAddress $ipAddress1

3. Stvaranje NIC-a za daljinski pristup. Obratite pozornost na to kako ova SLIKA je programa NSG povezane s njom.

            $nic2Name = $nicNamePrefix + $suffixNumber + "-RA"
            $ipAddress2 = $ipAddressPrefix + (53 + $suffixNumber)
            $nic2 = New-AzureRmNetworkInterface -Name $nic2Name -ResourceGroupName $backendRGName `
                -Location $location -SubnetId $backendSubnet.Id -PrivateIpAddress $ipAddress2 `
                -NetworkSecurityGroupId $remoteAccessNSG.Id

4. Stvaranje `vmConfig` objekt.

            $vmName = $vmNamePrefix + $suffixNumber
            $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize -AvailabilitySetId $avSet.Id

5. Stvorite dva diskova podataka po VM. Obratite pozornost na to da su diskova podatke na računu za pohranu premium ste ranije stvorili.

            $dataDisk1Name = $vmName + "-" + $osDiskPrefix + "-1"    
            $data1VhdUri = $prmStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $dataDisk1Name + ".vhd"
            Add-AzureRmVMDataDisk -VM $vmConfig -Name $dataDisk1Name -DiskSizeInGB $diskSize `
                -VhdUri $data1VhdUri -CreateOption empty -Lun 0

            $dataDisk2Name = $vmName + "-" + $dataDiskPrefix + "-2"    
            $data2VhdUri = $prmStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $dataDisk2Name + ".vhd"
            Add-AzureRmVMDataDisk -VM $vmConfig -Name $dataDisk2Name -DiskSizeInGB $diskSize `
                -VhdUri $data2VhdUri -CreateOption empty -Lun 1

6. Konfiguriranje operacijskih sustava, a sliku koja će se koristiti za na VM.

            $vmConfig = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $vmName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
            $vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -PublisherName $publisher -Offer $offer -Skus $sku -Version $version

7. Dodavanje dva mrežne kartice da biste stvorili iznad u `vmConfig` objekt.

            $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic1.Id -Primary
            $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic2.Id

8. Stvaranje diska OS i stvorite na VM. Obavijest na `}` koji završava s `for` petlji.

            $osDiskName = $vmName + "-" + $osDiskSuffix
            $osVhdUri = $stdStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $osDiskName + ".vhd"
            $vmConfig = Set-AzureRmVMOSDisk -VM $vmConfig -Name $osDiskName -VhdUri $osVhdUri -CreateOption fromImage
            New-AzureRmVM -VM $vmConfig -ResourceGroupName $backendRGName -Location $location
        }

### <a name="step-4---run-the-script"></a>Korak 4 - pokrenuti skriptu

Sad kad preuzeti, a promijenila se skripte ovisno o vašim potrebama, runt on skripte za stvaranje pozadinska VMs baze podataka s više NIC-ovi.

1. Spremite skripte i pokretanje iz naredbenog retka **PowerShell** ili **Očisti filtar**. Prikazat će se početni izlaz kao što je prikazano u nastavku.

        ResourceGroupName : IaaSStory-Backend
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                            Actions  NotActions
                            =======  ==========
                            *                  

        ResourceId        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/IaaSStory-Backend

2. Nakon nekoliko minuta, unesite vjerodajnice upit, a zatim kliknite **u redu**. Izlaz ispod predstavlja jednu VM. Obavijest o cijeli postupak trajalo 8 minuta.

        ResourceGroupName            :
        Id                           :
        Name                         : DB2
        Type                         :
        Location                     :
        Tags                         :
        TagsText                     : null
        AvailabilitySetReference     : Microsoft.Azure.Management.Compute.Models.AvailabilitySetReference
        AvailabilitySetReferenceText : {
                                         "ReferenceUri": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend/providers/
                                       Microsoft.Compute/availabilitySets/ASDB"
                                       }
        Extensions                   :
        ExtensionsText               : null
        HardwareProfile              : Microsoft.Azure.Management.Compute.Models.HardwareProfile
        HardwareProfileText          : {
                                         "VirtualMachineSize": "Standard_DS3"
                                       }
        InstanceView                 :
        InstanceViewText             : null
        NetworkProfile               :
        NetworkProfileText           : null
        OSProfile                    :
        OSProfileText                : null
        Plan                         :
        PlanText                     : null
        ProvisioningState            :
        StorageProfile               : Microsoft.Azure.Management.Compute.Models.StorageProfile
        StorageProfileText           : {
                                         "DataDisks": [
                                           {
                                             "Lun": 0,
                                             "Caching": null,
                                             "CreateOption": "empty",
                                             "DiskSizeGB": 127,
                                             "Name": "DB2-datadisk-1",
                                             "SourceImage": null,
                                             "VirtualHardDisk": {
                                               "Uri": "https://wtestvnetstorageprm.blob.core.windows.net/vhds/DB2-datadisk-1.vhd"
                                             }
                                           }
                                         ],
                                         "ImageReference": null,
                                         "OSDisk": null
                                       }
        DataDiskNames                : {DB2-datadisk-1}
        NetworkInterfaceIDs          :
        RequestId                    :
        StatusCode                   : 0


        ResourceGroupName            :
        Id                           :
        Name                         : DB2
        Type                         :
        Location                     :
        Tags                         :
        TagsText                     : null
        AvailabilitySetReference     : Microsoft.Azure.Management.Compute.Models.AvailabilitySetReference
        AvailabilitySetReferenceText : {
                                         "ReferenceUri": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend/providers/
                                       Microsoft.Compute/availabilitySets/ASDB"
                                       }
        Extensions                   :
        ExtensionsText               : null
        HardwareProfile              : Microsoft.Azure.Management.Compute.Models.HardwareProfile
        HardwareProfileText          : {
                                         "VirtualMachineSize": "Standard_DS3"
                                       }
        InstanceView                 :
        InstanceViewText             : null
        NetworkProfile               :
        NetworkProfileText           : null
        OSProfile                    :
        OSProfileText                : null
        Plan                         :
        PlanText                     : null
        ProvisioningState            :
        StorageProfile               : Microsoft.Azure.Management.Compute.Models.StorageProfile
        StorageProfileText           : {
                                         "DataDisks": [
                                           {
                                             "Lun": 0,
                                             "Caching": null,
                                             "CreateOption": "empty",
                                             "DiskSizeGB": 127,
                                             "Name": "DB2-datadisk-1",
                                             "SourceImage": null,
                                             "VirtualHardDisk": {
                                               "Uri": "https://wtestvnetstorageprm.blob.core.windows.net/vhds/DB2-datadisk-1.vhd"
                                             }
                                           },
                                           {
                                             "Lun": 1,
                                             "Caching": null,
                                             "CreateOption": "empty",
                                             "DiskSizeGB": 127,
                                             "Name": "DB2-datadisk-2",
                                             "SourceImage": null,
                                             "VirtualHardDisk": {
                                               "Uri": "https://wtestvnetstorageprm.blob.core.windows.net/vhds/DB2-datadisk-2.vhd"
                                             }
                                           }
                                         ],
                                         "ImageReference": null,
                                         "OSDisk": null
                                       }
        DataDiskNames                : {DB2-datadisk-1, DB2-datadisk-2}
        NetworkInterfaceIDs          :
        RequestId                    :
        StatusCode                   : 0


        EndTime             : 10/30/2015 9:30:03 AM -08:00
        Error               :
        Output              :
        StartTime           : 10/30/2015 9:22:54 AM -08:00
        Status              : Succeeded
        TrackingOperationId : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
        RequestId           : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
        StatusCode          : OK
