<properties
    pageTitle="Stvaranje virtualnog računala za SQL Server u ljusci PowerShell Azure (klasični) | Microsoft Azure"
    description="Nudi korake i skripti ljuske PowerShell za stvaranje programa Azure VM sa slikama Galerija virtualnog računala za SQL Server. U ovoj se temi koristi uvođenje klasičnog načina rada."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/07/2016"
    ms.author="jroth" />

# <a name="provision-a-sql-server-virtual-machine-using-azure-powershell-classic"></a>Dodjela resursa u SQL Server virtualnog računala pomoću komponente PowerShell Azure (Classic)

## <a name="overview"></a>Pregled

Ovaj članak sadrži upute za stvaranje virtualnog računala za SQL Server u Azure pomoću cmdleta ljuske PowerShell.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Voditelj resursa verziju ove teme potražite u članku [Dodjeljivanje virtualni stroj SQL Server pomoću upravitelja resursa za Azure PowerShell](virtual-machines-windows-ps-sql-create.md).

### <a name="install-and-configure-powershell"></a>Instaliranje i konfiguriranje PowerShell:

1. Ako nemate račun za Azure, posjetite [Azure slobodno probnu verziju](https://azure.microsoft.com/pricing/free-trial/).

2. [Preuzmite i instalirajte najnovije naredbe Azure PowerShell](../powershell-install-configure.md).

3. Pokrenite Windows PowerShell te je povežite s pretplate Azure pomoću naredbe **Dodaj AzureAccount** .

        Add-AzureAccount

## <a name="determine-your-target-azure-region"></a>Određivanje ciljnog Azure regija

Sustava SQL Server virtualnog računala će se nalaziti u oblaku koja se nalazi određene Azure regija. Na sljedeći način olakšavaju da biste odredili područje, računa za pohranu i servisa koji će se koristiti za ostale vodič u oblaku.

1. Određivanje podatkovnom centru koji želite koristiti za hostiranje vaše VM SQL Server. Sljedeće naredbe ljuske PowerShell prikazat će se dostupni područja detaljno s popisom sažetka na kraju.

        Get-AzureLocation
        (Get-AzureLocation).Name

2.  Nakon što ste označena Preferirani lokacije, postavite varijablu pod nazivom **$dcLocation** da biste to područje.

        $dcLocation = "<region name>"

## <a name="set-your-subscription-and-storage-account"></a>Postavljanje računa web-mjesto pretplata i u okvir za pohranu

1. Određivanje Azure pretplata će se koristiti za nove virtualnog računala.

        (Get-AzureSubscription).SubscriptionName

1. Dodijelite ciljnu Azure pretplate **$subscr** varijabli. Zatim Postavi ovo kao trenutne Azure pretplate.

        $subscr="<subscription name>"
        Select-AzureSubscription -SubscriptionName $subscr –Current

1. Zatim potražiti postojeće račune za pohranu. Sljedeću skriptu prikazuju se svi računi za pohranu koji postoje u vašoj regiji odabranom:

        (Get-AzureStorageAccount | where { $_.GeoPrimaryLocation -eq $dcLocation }).StorageAccountName

    >[AZURE.NOTE] Ako tražite na novi račun za pohranu, najprije stvoriti naziv računa spremišta sve malim slovima pomoću naredbe nova AzureStorageAccount kao u sljedećem primjeru: **Novo AzureStorageAccount - StorageAccountName "<storage account name>" – mjesto $dcLocation**

1. Dodijelite naziv računa za pohranu za ciljnu **$staccount**. Da biste postavili pretplate i trenutni prostor za pohranu račun koristiti **Skup AzureSubscription** .

        $staccount="<storage account name>"
        Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

## <a name="select-a-sql-server-virtual-machine-image"></a>Odaberite sliku virtualnog računala sustava SQL Server

1. Saznajte više na popisu dostupnih slika virtualnim računalima sustava SQL Server iz galerije. Sve su **ImageFamily** svojstva u programu koji započinje "SQL" te slike. Sljedeći upit prikazuje obitelj slike dostupne koje imaju predinstaliran je SQL Server.

        Get-AzureVMImage | where { $_.ImageFamily -like "SQL*" } | select ImageFamily -Unique | Sort-Object -Property ImageFamily

1. Kada pronađete slika obitelji virtualnog računala, može postojati više objavljene slike u ovom obitelji. Koristite sljedeću skriptu da biste pronašli najnovije objavljene virtualnog računala slika naziv vaše obitelji na odabrane slike (kao što je **SQL Server 2016 RTM Enterprise na Windows Server 2012 R2**):

        $family="<ImageFamily value>"
        $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

        echo "Selected SQL Server image name:"
        echo "   $image"

## <a name="create-the-virtual-machine"></a>Stvaranje virtualnog računala

Na kraju, stvorite virtualnog računala sa servisom PowerShell:

1. Stvaranje neki servis u oblaku za hostiranje novi VM. Imajte na umu da je moguće koristiti postojeće oblaku umjesto toga. Stvorite novi varijable **$svcname** s kratki naziv servisa u oblaku.

        $svcname = "<cloud service name>"
        New-AzureService -ServiceName $svcname -Label $svcname -Location $dcLocation

2. Navedite naziv virtualnog računala i veličinu. Dodatne informacije o veličinama virtualnog računala potražite u članku [Veličine virtualnog računala za Azure](virtual-machines-windows-sizes.md).

        $vmname="<machine name>"
        $vmsize="<Specify one: Large, ExtraLarge, A5, A6, A7, A8, A9, or see the link to the other VM sizes>"
        $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

3. Navedite lokalni administratorski račun i lozinku.

        $cred=Get-Credential -Message "Type the name and password of the local administrator account."
        $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password

4. Pokrenite sljedeću skriptu da biste stvorili virtualnog računala.

        New-AzureVM –ServiceName $svcname -VMs $vm1

>[AZURE.NOTE] Dodatni objašnjenje i mogućnosti konfiguracije potražite u odjeljku **Sastavljanje skupu naredba** [Korištenje ljuske PowerShell Azure da biste stvorili i prethodno konfigurirati virtualnim strojevima utemeljen na sustavu Windows](virtual-machines-windows-classic-create-powershell.md).

## <a name="example-powershell-script"></a>Primjer skriptu komponente PowerShell

Nudi sljedeće skripte i primjer dovršeno skriptu koja stvara **Sustava SQL Server 2016 RTM Enterprise na Windows Server 2012 R2** virtualnog računala. Ako koristite ovu skriptu, morate prilagoditi početni varijable koji se temelji na prethodne korake u nastavku.

    # Customize these variables based on your settings and requirements:
    $dcLocation = "East US"
    $subscr="mysubscription"
    $staccount="mystorageaccount"
    $family="SQL Server 2016 RTM Enterprise on Windows Server 2012 R2"
    $svcname = "mycloudservice"
    $vmname="myvirtualmachine"
    $vmsize="A5"

    # Set the current subscription and storage account
    # Comment out the New-AzureStorageAccount line if the account already exists
    Select-AzureSubscription -SubscriptionName $subscr –Current
    New-AzureStorageAccount -StorageAccountName $staccount -Location $dcLocation
    Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

    # Select the most recent VM image in this image family:
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

    # Create the new cloud service; comment out this line if cloud service exists already:
    New-AzureService -ServiceName $svcname -Label $svcname -Location $dcLocation

    # Create the VM config:
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    # Set administrator credentials:
    $cred=Get-Credential -Message "Type the name and password of the local administrator account."
    $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password

    # Create the SQL Server VM:
    New-AzureVM –ServiceName $svcname -VMs $vm1


## <a name="connect-with-remote-desktop"></a>Povezivanje s udaljene radne površine

1. Stvaranje na. RDP datoteke u mapi dokumenta trenutnog korisnika da biste pokrenuli te virtualnih računala da biste dovršili postavljanje:

        $documentspath = [environment]::getfolderpath("mydocuments")
        Get-AzureRemoteDesktopFile -ServiceName $svcname -Name $vmname -LocalPath "$documentspath\vm1.rdp"

1. U direktoriju dokumente, pokrenite datoteku RDP. Povezivanje s administratorskim korisničko ime i lozinku koju ste dobili neke starije verzije (Ako, na primjer, ako je korisničko ime VMAdmin, navedite "\VMAdmin" kao korisnik i lozinka).

        cd $documentspath
        .\vm1.rdp

## <a name="complete-the-configuration-of-the-sql-server-machine-for-remote-access"></a>Dovršavanje konfiguracije računala SQL Server za daljinski pristup

Nakon prijave na računalo s udaljene radne površine, konfigurirati SQL Server na temelju upute navedene u članku [upute za konfiguriranje veza s poslužiteljem SQL u VM programa Azure](virtual-machines-windows-classic-sql-connect.md#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).

## <a name="next-steps"></a>Daljnji koraci

Dodatne upute za dodjelu resursa u [dokumentaciji virtualnim strojevima](virtual-machines-windows-classic-create-powershell.md)virtualnim strojevima sa servisom PowerShell možete pronaći. Dodatni skripte vezane uz SQL Server i Premium prostora za pohranu, potražite u članku [Korištenje Azure Premium prostora za pohranu sa sustavom SQL Server na virtualnim računalima sustava](virtual-machines-windows-classic-sql-server-premium-storage.md).

U mnogim slučajevima, sljedeći je korak da biste migrirali baze podataka za ovaj novi VM SQL Server. Vodič za migraciju baze podataka, potražite u članku [Migracija baze podataka SQL Server na programa Azure VM](virtual-machines-windows-migrate-sql.md).

Ako vas zanima i pomoću portala za Azure da biste stvorili SQL virtualnim strojevima, potražite u članku [Dodjeljivanje SQL Server virtualnog računala na Azure](virtual-machines-windows-portal-sql-server-provision.md). Imajte na umu da vodič koji vas vodi kroz portalu stvara VMs pomoću preporučenih modela Voditelj resursa, umjesto klasični modela koji se koristi u ovoj temi PowerShell.

Osim tih resursa, preporučujemo da pregledate [druge teme vezane uz izvodi SQL Server na virtualnim strojevima sa sustavom Azure](virtual-machines-windows-sql-server-iaas-overview.md).
