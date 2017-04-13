<properties
   pageTitle="Implementacija višestruki NIC VMs pomoću EŽA Azure u modelu uvođenje klasičnog | Microsoft Azure"
   description="Saznajte kako uvesti više NIC VMs pomoću EŽA Azure u modelu klasični implementacije"
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

#<a name="deploy-multi-nic-vms-classic-using-the-azure-cli"></a>Implementacija pomoću EŽA Azure višestruki NIC VMs (classic)

[AZURE.INCLUDE [virtual-network-deploy-multinic-classic-selectors-include.md](../../includes/virtual-network-deploy-multinic-classic-selectors-include.md)]

Možete stvoriti virtualnim strojevima (VMs) u Azure i priložite svaki od vašeg VMs više mreža sučelja (NIC-ovi). NIC-ovi više omogućiti odvojenosti vrsta promet preko NIC-ovi. Ako, na primjer, jedan NIC možda komunicirati s Internetom, dok drugi komunicira samo s interne resurse niste povezani s Internetom. Za mnoge mreže virtualne aparata, kao što su isporuka aplikacija i rješenja WAN optimizaciju potreban je mogućnost odvojiti mrežni promet preko više NIC-ovi.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Saznajte kako [izvesti sljedeće korake pomoću modela Voditelj resursa](virtual-network-deploy-multinic-arm-cli.md).

[AZURE.INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

Trenutno ne možete imati VMs s jednom NIC i VMs s više NIC-ovi u istom servis u oblaku. Stoga ćete morati implementirati pozadinskih poslužitelja u različitim oblaku od i druge komponente scenarija. Koraci u nastavku koristite neki servis u oblaku pod nazivom *IaaSStory* za glavni resurse i *IaaSStory pozadinskog* za pozadinskih poslužitelja.

## <a name="prerequisites"></a>Preduvjeti

Prije možete implementirati pozadinskih poslužitelja, morate je implementirati glavni servisa u oblaku s potrebne resurse za taj scenarij. Barem, morate stvoriti virtualne mreže s podmreže za pozadinski. Posjetite [Stvaranje virtualne mreže pomoću EŽA Azure](virtual-networks-create-vnet-classic-cli.md) da biste saznali kako uvesti virtualne mreže.

[AZURE.INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="deploy-the-back-end-vms"></a>Implementacija pozadinskih VMs

VMs pozadinskog ovise o stvaranje resursa navedena u nastavku.

- **Račun za pohranu za diskova podataka**. Bolje performanse diskova podataka na poslužitelje baza podataka će koristiti pune stanje pogon (SSD) tehnologiju koja potreban je račun za pohranu premium. Provjerite je li Azure mjesto implementacije podržava premium prostora za pohranu.
- **NIC-ovi**. Svaki VM će imati dvije NIC-ovi, jedan za pristup bazi podataka, a jedan za upravljanje.
- **Postavljanje dostupnosti**. Dodat će se poslužiteljima baze podataka na jednom dostupnost postavili, da biste bili sigurni barem jedno od na VMs prema gore i pokretanje tijekom održavanja.

### <a name="step-1---start-your-script"></a>Korak 1 – pokrenuti skriptu

Možete preuzeti cijeli tulumu skripta koja se koristi [u nastavku](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/classic/virtual-network-deploy-multinic-classic-cli.sh). Slijedite korake u nastavku da biste promijenili skripta za rad u svom okruženju.

1. Promijenite vrijednosti varijabli ispod koji se temelji na postojeću grupu resursa u [preduvjeti](#Prerequisites)implementirana iznad.

        location="useast2"
        vnetName="WTestVNet"
        backendSubnetName="BackEnd"

2. Promijenite vrijednosti varijabli dolje na temelju vrijednosti koje želite koristiti za implementaciju sustava pozadinskog.

        backendCSName="IaaSStory-Backend"
        prmStorageAccountName="iaasstoryprmstorage"
        image="0b11de9248dd4d87b18621318e037d37__RightImage-Ubuntu-14.04-x64-v14.2.1"
        avSetName="ASDB"
        vmSize="Standard_DS3"
        diskSize=127
        vmNamePrefix="DB"
        osDiskName="osdiskdb"
        dataDiskPrefix="db"
        dataDiskName="datadisk"
        ipAddressPrefix="192.168.2."
        username='adminuser'
        password='adminP@ssw0rd'
        numberOfVMs=2

### <a name="step-2---create-necessary-resources-for-your-vms"></a>Korak 2 – da biste stvorili potrebne resurse za vaše VMs

1. Stvaranje novog servisa oblaka za sve pozadinskog VMs. Obratite pozornost na korištenje na `$backendCSName` varijable za grupu naziv resursa i `$location` za Azure regiju.

        azure service create --serviceName $backendCSName \
            --location $location

2. Stvaranje računa za pohranu premium za diskova OS i podataka za koristi vaš VMs.

        azure storage account create $prmStorageAccountName \
            --location $location \
            --type PLRS

### <a name="step-3---create-vms-with-multiple-nics"></a>Korak 3 – da biste stvorili VMs s više NIC-ovi

1. Pokretanje petlje da biste stvorili više VMs, na temelju na `numberOfVMs` varijabli.

        for ((suffixNumber=1;suffixNumber<=numberOfVMs;suffixNumber++));
        do

2. Za svaki VM Navedite naziv i IP adrese svakog od dvije mrežne kartice.

            nic1Name=$vmNamePrefix$suffixNumber-DA
            x=$((suffixNumber+3))
            ipAddress1=$ipAddressPrefix$x

            nic2Name=$vmNamePrefix$suffixNumber-RA
            x=$((suffixNumber+53))
            ipAddress2=$ipAddressPrefix$x

4. Stvorite na VM. Obratite pozornost na korištenje na `--nic-config` parametar, koji sadrži popis svih NIC-ovi s imenom, podmreže i IP adresa.

            azure vm create $backendCSName $image $username $password \
                --connect $backendCSName \
                --vm-name $vmNamePrefix$suffixNumber \
                --vm-size $vmSize \
                --availability-set $avSetName \
                --blob-url $prmStorageAccountName.blob.core.windows.net/vhds/$osDiskName$suffixNumber.vhd \
                --virtual-network-name $vnetName \
                --subnet-names $backendSubnetName \
                --nic-config $nic1Name:$backendSubnetName:$ipAddress1::,$nic2Name:$backendSubnetName:$ipAddress2::

5. Za svaki VM stvorite dva diskova podataka.

            azure vm disk attach-new $vmNamePrefix$suffixNumber \
                $diskSize \
                vhds/$dataDiskPrefix$suffixNumber$dataDiskName-1.vhd

            azure vm disk attach-new $vmNamePrefix$suffixNumber \
                $diskSize \
                vhds/$dataDiskPrefix$suffixNumber$dataDiskName-2.vhd
        done

### <a name="step-4---run-the-script"></a>Korak 4 - pokrenuti skriptu

Sad kad ste preuzeli i promijeniti skripte ovisno o vašim potrebama, pokrenite skriptu da biste stvorili pozadinska VMs baze podataka s više NIC-ovi.

1. Spremite skripte i pokretanje s na **tulum** terminal. Prikazat će se početni izlaz kao što je prikazano u nastavku.

        info:    Executing command service create
        info:    Creating cloud service
        data:    Cloud service name IaaSStory-Backend
        info:    service create command OK
        info:    Executing command storage account create
        info:    Creating storage account
        info:    storage account create command OK
        info:    Executing command vm create
        info:    Looking up image 0b11de9248dd4d87b18621318e037d37__RightImage-Ubuntu-14.04-x64-v14.2.1
        info:    Looking up virtual network
        info:    Looking up cloud service
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Creating VM

2. Nakon nekoliko minuta izvršavanja će se zaustaviti, a prikazat će se ostatak izlaz kao što je prikazano u nastavku.

        info:    OK
        info:    vm create command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
        info:    Executing command vm create
        info:    Looking up image 0b11de9248dd4d87b18621318e037d37__RightImage-Ubuntu-14.04-x64-v14.2.1
        info:    Looking up virtual network
        info:    Looking up cloud service
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Creating VM
        info:    OK
        info:    vm create command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
