<properties
   pageTitle="Implementacija višestruki NIC VMs pomoću komponente PowerShell u modelu uvođenje klasičnog | Microsoft Azure"
   description="Saznajte kako uvesti više NIC VMs pomoću komponente PowerShell u modelu klasični implementacije"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

#<a name="deploy-multi-nic-vms-classic-using-powershell"></a>Implementacija višestruki NIC VMs (klasični) pomoću komponente PowerShell

[AZURE.INCLUDE [virtual-network-deploy-multinic-classic-selectors-include.md](../../includes/virtual-network-deploy-multinic-classic-selectors-include.md)]

Možete stvoriti virtualnim strojevima (VMs) u Azure i priložite svaki od vašeg VMs više mreža sučelja (NIC-ovi). NIC-ovi više omogućiti odvojenosti vrsta promet preko NIC-ovi. Ako, na primjer, jedan NIC možda komunicirati s Internetom, dok drugi komunicira samo s interne resurse niste povezani s Internetom. Za mnoge mreže virtualne aparata, kao što su isporuka aplikacija i rješenja WAN optimizaciju potreban je mogućnost odvojiti mrežni promet preko više NIC-ovi.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Saznajte kako [izvesti sljedeće korake pomoću modela Voditelj resursa](virtual-network-deploy-multinic-arm-ps.md).

[AZURE.INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

Trenutno ne možete imati VMs s jednom NIC i VMs s više NIC-ovi u istom servis u oblaku. Stoga ćete morati implementirati pozadinskih poslužitelja u različitim oblaku od i druge komponente scenarija. Koraci u nastavku koristite neki servis u oblaku pod nazivom *IaaSStory* za glavni resurse i *IaaSStory pozadinskog* za pozadinskih poslužitelja.

## <a name="prerequisites"></a>Preduvjeti

Prije možete implementirati pozadinskih poslužitelja, morate je implementirati glavni servisa u oblaku s potrebne resurse za taj scenarij. Barem, morate stvoriti virtualne mreže s podmreže za pozadinski. Posjetite [Stvaranje virtualne mreže pomoću komponente PowerShell](virtual-networks-create-vnet-classic-netcfg-ps.md) da biste saznali kako uvesti virtualne mreže.

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="deploy-the-back-end-vms"></a>Implementacija pozadinskih VMs

VMs pozadinskog ovise o stvaranje resursa navedena u nastavku.

- **Pozadinskog podmreže**. Poslužitelje baza podataka koji će biti dio zasebnom podmreže, da biste segregate promet. U nastavku skriptu očekuje ovaj podmreže postoji u vnet pod nazivom *WTestVnet*.
- **Račun za pohranu za diskova podataka**. Bolje performanse diskova podataka na poslužitelje baza podataka će koristiti pune stanje pogon (SSD) tehnologiju koja potreban je račun za pohranu premium. Provjerite je li Azure mjesto implementacije podržava premium prostora za pohranu.
- **Postavljanje dostupnosti**. Dodat će se poslužiteljima baze podataka na jednom dostupnost postavili, da biste bili sigurni barem jedno od na VMs prema gore i pokretanje tijekom održavanja.

### <a name="step-1---start-your-script"></a>Korak 1 – pokrenuti skriptu

Možete preuzeti cijeli skriptu PowerShell koristi [ovdje](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/classic/virtual-network-deploy-multinic-classic-ps.ps1). Slijedite korake u nastavku da biste promijenili skripta za rad u svom okruženju.

1. Promijenite vrijednosti varijabli ispod koji se temelji na postojeću grupu resursa u [preduvjeti](#Prerequisites)implementirana iznad.

        $location              = "West US"
        $vnetName              = "WTestVNet"
        $backendSubnetName     = "BackEnd"

2. Promijenite vrijednosti varijabli dolje na temelju vrijednosti koje želite koristiti za implementaciju sustava pozadinskog.

        $backendCSName         = "IaaSStory-Backend"
        $prmStorageAccountName = "iaasstoryprmstorage"
        $avSetName             = "ASDB"
        $vmSize                = "Standard_DS3"
        $diskSize              = 127
        $vmNamePrefix          = "DB"
        $dataDiskSuffix        = "datadisk"
        $ipAddressPrefix       = "192.168.2."
        $numberOfVMs           = 2

### <a name="step-2---create-necessary-resources-for-your-vms"></a>Korak 2 – da biste stvorili potrebne resurse za vaše VMs

Morate stvoriti novi servis u oblaku i račun za pohranu za diskova podataka za sve VMs. Morate navesti sliku, a lokalni administratorski račun za na VMs. Da biste stvorili tih resursa, izvršiti sljedeće korake.

1. Stvorite novi servis u oblaku.

        New-AzureService -ServiceName $backendCSName -Location $location

2. Stvaranje novog računa za pohranu premium.

        New-AzureStorageAccount -StorageAccountName $prmStorageAccountName `
            -Location $location `
            -Type Premium_LRS

3. Postavljanje računa za pohranu stvorena iznad kao trenutni račun za pohranu za vašu pretplatu.

        $subscription = Get-AzureSubscription `
            | where {$_.IsCurrent -eq $true}  
        Set-AzureSubscription -SubscriptionName $subscription.SubscriptionName `
            -CurrentStorageAccountName $prmStorageAccountName

4. Odaberite sliku na VM.

        $image = Get-AzureVMImage `
            | where{$_.ImageFamily -eq "SQL Server 2014 RTM Web on Windows Server 2012 R2"} `
            | sort PublishedDate -Descending `
            | select -ExpandProperty ImageName -First 1

5. Postavljanje lokalni administrator vjerodajnica računa.

        $cred = Get-Credential -Message "Enter username and password for local admin account"

### <a name="step-3---create-vms"></a>Korak 3 – stvaranje VMs

Morate koristiti petlje da biste stvorili proizvoljan broj VMs kao i stvaranje potrebne NIC-ovi i VMs unutar petlje. Da biste stvorili NIC-ovi i VMs, izvršiti sljedeće korake.

1. Pokretanje programa `for` Ponavljaj Ponavljanje naredbe za stvaranje na VM i dva NIC-ovi kao proizvoljan broj puta, na temelju vrijednosti u `$numberOfVMs` varijablu.

        for ($suffixNumber = 1; $suffixNumber -le $numberOfVMs; $suffixNumber++){

2. Stvaranje na `VMConfig` objekt koji određuje slike, veličinu i dostupnost za na VM.

            $vmName = $vmNamePrefix + $suffixNumber
            $vmConfig = New-AzureVMConfig -Name $vmName `
                            -ImageName $image `
                            -InstanceSize $vmSize `
                            -AvailabilitySetName $avSetName  

3. Dodjela resursa u VM kao Windows VM.

            Add-AzureProvisioningConfig -VM $vmConfig -Windows `
                -AdminUsername $cred.UserName `
                -Password $cred.Password

4. Postavljanje zadanog NIC i dodijelite joj statičke IP adrese.

            Set-AzureSubnet -SubnetNames $backendSubnetName -VM $vmConfig
            Set-AzureStaticVNetIP -IPAddress ($ipAddressPrefix+$suffixNumber+3) -VM $vmConfig

5. Dodajte drugi NIC za svaki VM.

            Add-AzureNetworkInterfaceConfig -Name ("RemoteAccessNIC"+$suffixNumber) `
                -SubnetName $backendSubnetName `
                -StaticVNetIPAddress ($ipAddressPrefix+(53+$suffixNumber)) `
                -VM $vmConfig

6. Stvaranje diskova podatke za svaku VM.

            $dataDisk1Name = $vmName + "-" + $dataDiskSuffix + "-1"    
            Add-AzureDataDisk -CreateNew -VM $vmConfig `
                -DiskSizeInGB $diskSize `
                -DiskLabel $dataDisk1Name `
                -LUN 0       

            $dataDisk2Name = $vmName + "-" + $dataDiskSuffix + "-2"   
            Add-AzureDataDisk -CreateNew -VM $vmConfig `
                -DiskSizeInGB $diskSize `
                -DiskLabel $dataDisk2Name `
                -LUN 1

7. Stvorite svaki VM i završavanje petlje.

            New-AzureVM -VM $vmConfig `
                -ServiceName $backendCSName `
                -Location $location `
                -VNetName $vnetName
        }

### <a name="step-4---run-the-script"></a>Korak 4 - pokrenuti skriptu

Sad kad preuzeti, a promijenila se skripte ovisno o vašim potrebama, runt on skripte za stvaranje pozadinska VMs baze podataka s više NIC-ovi.

1. Spremite skripte i pokretanje iz naredbenog retka **PowerShell** ili **Očisti filtar**. Prikazat će se početni izlaz kao što je prikazano u nastavku.

        OperationDescription    OperationId                          OperationStatus
        --------------------    -----------                          ---------------
        New-AzureService        xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded      
        New-AzureStorageAccount xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded      

        WARNING: No deployment found in service: 'IaaSStory-Backend'.

2. Ispunite podatke koji su potrebni u upit vjerodajnice i kliknite **u redu**. Prikazat će se ispod izlaz.

        New-AzureVM             xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
        New-AzureVM             xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
