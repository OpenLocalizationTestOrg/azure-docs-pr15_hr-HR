<properties
    pageTitle="Korištenje prostora za pohranu Azure Premium s SQL Server | Microsoft Azure"
    description="U ovom se članku koristi resursa koje su stvorene pomoću modela uvođenje klasičnog te se iznosi smjernice o pohranu Premium Azure pomoću SQL Server pokrenut na virtualnim strojevima sa sustavom Azure."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="danielsollondon"
    manager="jhubbard"
    editor="monicar"    
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/19/2016"
    ms.author="jroth"/>

# <a name="use-azure-premium-storage-with-sql-server-on-virtual-machines"></a>Koristite za pohranu Azure Premium sa sustavom SQL Server na virtualnim računalima


## <a name="overview"></a>Pregled

[Prostor za pohranu za Premium Azure](../storage/storage-premium-storage.md) je generacije prostora za pohranu na kojoj se navode niske latencije i visoke propusnost IO. Je najbolja za ključne IO intenzivno radnih opterećenja, primjerice, SQL Server na IaaS [virtualnih računala](https://azure.microsoft.com/services/virtual-machines/).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


U ovom se članku daje planiranja i upute za migriranje virtualnog računala koja se izvodi SQL Server da biste koristili Premium prostora za pohranu. To obuhvaća Azure infrastrukture (umrežavanje, spremište) i goste Windows VM korake. Primjer u [dodatak](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage) prikazuje potpuni potpun end da biste završetka migracije kako premjestiti veće VMs da iskoristite prednost poboljšani lokalno spremište SSD sa servisom PowerShell.

Važno da biste shvatili postupak završetka do kraja korištenja Azure Premium prostora za pohranu sa sustavom SQL Server na IAAS VMs je. To obuhvaća:

- Identifikacijski preduvjete da biste koristili Premium prostora za pohranu.
- Primjeri implementaciji sustava SQL Server na IaaS Premium za pohranu za nove implementacije.
- Primjeri Migracija postojeće implementacije samostalni poslužitelji i implementaciji koristite SQL uvijek na dostupnost grupe.
- Postupke mogućih Migracija.
- Primjer potpuni završetka do kraja prikazuje Azure, Windows i SQL Server korake za migraciju implementacija postojeće uvijek na.

Više informacija na SQL Server na virtualnim strojevima sa sustavom Azure potražite u članku [SQL Server na virtualnim strojevima sa sustavom Azure](virtual-machines-windows-sql-server-iaas-overview.md).

**Autor:** Dan Sol **Tehnička pregledavatelji:** Luis Carlos Vargas Herring, Sanjay Mishra, Pravin Mital, Juergen pošta, Gonzalo Ruizu.

## <a name="prerequisites-for-premium-storage"></a>Preduvjeti za pohranu Premium

Postoji nekoliko preduvjeta za korištenje Premium prostora za pohranu.

### <a name="machine-size"></a>Veličina računala

Za korištenje prostora za pohranu Premium morat ćete koristiti DS serija virtualnim strojevima (VM). Ako niste koristili DS niz računala u oblaku prije, morate izbrisati postojeće VM, zadržali priložene diskova i zatim stvorite novi servis u oblaku prije stvaranja VM kao DS * uloga veličinu. Dodatne informacije o veličinama virtualnog računala potražite u članku [virtualnog računala i veličine servisa oblaka za Azure](virtual-machines-linux-sizes.md).

### <a name="cloud-services"></a>Servisi u oblaku

DS * VMs možete koristiti samo s Premium pohranom prilikom stvaranja u novi u oblaku. Ako koristite SQL Server uvijek na servisu Azure, uvijek na ga Slušatelj će se adresa Azure interne i vanjske IP raspoređivača opterećenja vezan uz servis u oblaku. U ovom se članku usredotočuje se na migriranju uz zadržavanje dostupnost u ovom scenariju.

> [AZURE.NOTE] Niz DS * mora biti prvi VM koji je implementiran na novi servis u Oblaku.

### <a name="regional-vnets"></a>Regionalne VNETS

Za DS * VMs morate konfigurirati na virtualne mreže (VNET) hostiranje vaše VMs biti regionalne. To "widens" na VNET omogućuje veće VMs biti dodijeljena druge klastere i dopustiti komunikaciju između njih. U sljedećim snimka istaknuto mjesto prikazuje regionalne VNETs dok se prvi rezultat prikazuje "usko" VNET.

![RegionalVNET][1]

Možete postići programa Microsoft zahtjev za podršku možete preseliti regionalne VNET, Microsoft će promjene, a zatim da biste dovršili o migraciji regionalne VNETs promijeniti svojstvo AffinityGroup u mrežnoj konfiguraciji. Izvesti mrežnoj konfiguraciji u ljusci PowerShell, a zatim svojstvo **AffinityGroup** element **VirtualNetworkSite** zamijenite svojstvo **mjesta** . Odredite `Location = XXXX` gdje `XXXX` je Azure regija. Zatim uvesti konfiguraciju novi.

Ako, na primjer, preporučuje se VNET konfiguraciju sljedeće:

    <VirtualNetworkSite name="danAzureSQLnet" AffinityGroup="AzureSQLNetwork">
    <AddressSpace>
      <AddressPrefix>10.0.0.0/8</AddressPrefix>
      <AddressPrefix>172.16.0.0/12</AddressPrefix>
    </AddressSpace>
    <Subnets>
    ...
    </VirtualNetworkSite>

Da biste premjestili to regional VNET u Europi Zapad, promijenite konfiguraciju sljedeće:

    <VirtualNetworkSite name="danAzureSQLnet" Location="West Europe">
    <AddressSpace>
      <AddressPrefix>10.0.0.0/8</AddressPrefix>
      <AddressPrefix>172.16.0.0/12</AddressPrefix>
    </AddressSpace>
    <Subnets>
    ...
    </VirtualNetworkSite>

### <a name="storage-accounts"></a>Računi za pohranu

Morat ćete stvoriti novi račun za pohranu koji je konfiguriran za pohranu Premium. Imajte na umu da koristite za pohranu Premium je postavljen na račun za pohranu, ne i na pojedinačne VHDs Međutim kada se koristi s VM niz za DS * VHD, možete priložiti od Premium i standardne prostora za pohranu računa. To može preporučujemo ako želite smjestiti VHD OS korištenjem računa Premium prostora za pohranu.

Sljedeća naredba **Novo AzureStorageAccountPowerShell** s "Premium_LRS" **Vrsta** stvara račun za pohranu Premium:

    $newstorageaccountname = "danpremstor"
    New-AzureStorageAccount -StorageAccountName $newstorageaccountname -Location "West Europe" -Type "Premium_LRS"   

### <a name="vhds-cache-settings"></a>Postavke predmemorije VHDs

Glavna razlika između stvaranja diskova koje su dio računa za pohranu Premium je postavka predmemorije diska. Za SQL Server Data glasnoću diskova preporučuje se da koristite '**Čitanje predmemoriranje**'. Za transakcije zapisnika količine, postavku predmemorije diska trebalo biti postavljeno na '**nijedna**'. Time se razlikuje od preporuke za račune standardne prostora za pohranu.

Kada se pridružite se VHDs postavku predmemorije nije moguće izmijeniti. Trebali biste odvojiti i ponovno priložiti u VHD u postavci ažurirane predmemoriju.

### <a name="windows-storage-spaces"></a>Prostori za pohranu sustava Windows

[Prostori za pohranu Windows](https://technet.microsoft.com/library/hh831739.aspx) možete koristiti kao prethodne standardne prostora za pohranu, to će vam da biste migrirali VM koji je već korištenja prostori za pohranu omogućiti. U primjeru u [dodatak](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage) (korak 9 i prosljeđivanje) pokazuje Powershell kod za izdvajanje i uvoz VM s više priložene VHDs.

Grupe za pohranu su se s računa za pohranu standardne Azure da biste poboljšali propusnost i Latencija smanjiti. Može pronaći vrijednost u testiranje za pohranu grupe s Premium prostora za pohranu za nove implementacije, ali dodaju dodatne složenosti s postavljanjem prostora za pohranu.

#### <a name="how-to-find-which-azure-virtual-disks-map-to-storage-pools"></a>Kako pronaći koja će se karta Azure virtualne diskova za pohranu grupe

Kao što su različite predmemorije postavka preporuke za priložene VHDs, možda odlučite da biste kopirali u VHDs račun za pohranu Premium. Međutim, kada ih ponovno priložili novi niz DS VM, možda ćete morati promijeniti postavke predmemorije. To je jednostavnije da biste primijenili prostora za pohranu Premium preporučenih postavki predmemorije kad imate zasebnom VHDs za SQL podatkovne datoteke i datoteke zapisnika (umjesto jednog VHD koja sadrži oba).

> [AZURE.NOTE] Ako imate datoteke podataka i zapisnika sustava SQL Server na istoj jedinici, predmemoriranja mogućnost odaberete ovisi o uzoraka pristup IO za radnih opterećenja sustava baze podataka. Samo testiranje znanje možete koju predmemoriranja mogućnost je najbolje za taj scenarij.

Međutim, ako koristite Windows prostori za pohranu koji se sastoje od više VHDs morat ćete pregled izvorne skripte za prepoznavanje koji je pridružen VHDs su u koje određeni skup tako da možete postaviti postavke predmemorije sukladno tome za svaki disk.

Ako nemate izvornu skripte dostupna da bi se prikazala koji VHDs mapiranje na resurse za pohranu, možete koristiti sljedeće korake da biste odredili skup preslikavanja disk/prostora za pohranu.

Za svaki disk, poduzmite sljedeće korake:

1. Pojavljuje se popis diskova priložiti VM naredbom **Get-AzureVM** :

    Get-AzureVM - naziv servisa <servicename> -naziv <vmname> | Get-AzureDataDisk

1. Imajte na umu Diskname i LUN.

    ![DisknameAndLUN][2]

1. Udaljena radna površina u na VM. Otvorite **Upravljanje računalom** | **Upravitelj uređaja** | **diskovnih pogona**. Pregled svojstava svake od "Microsoft virtualne diskova"

    ![VirtualDiskProperties][3]

1. Broj LUN ovdje je referenca na LUN broj koji ste naveli prilikom pridruživanja na VHD da biste na VM.
1. 'Microsoft virtualne Disk' otvorite karticu **Detalji** , zatim u popis **svojstava** otvorite **Ključ upravljački program**. **Vrijednost**, imajte na umu na **Pomak**, koji je 0002 u sljedeće snimku zaslona. Na 0002 označava PhysicalDisk2 koja se odnosi na resurse za pohranu.

    ![VirtualDiskPropertyDetails][4]

2. Za svaki skup za pohranu ispis izgleda pridružene diskova:

    Get-StoragePool - FriendlyName AMS1pooldata | Get-PhysicalDisk

    ![GetStoragePool][5]

Sada možete koristiti ti podaci povezivati VHDs priložiti fizičkih diskova u grupe za pohranu.

Kada ste mapirali VHDs fizičkih diskova u prostor za pohranu grupe možete zatim odvajanje i kopirajte ih putem računa za pohranu Premium, a zatim ih priložite s postavkom točan predmemorije. Pogledajte primjer u [dodatak](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage)korake 8 do 12. Sljedeći vam koraci prikazuju kako izdvojiti VM priloženom VHD konfiguraciju disk u CSV datoteku, kopirajte na VHDs, mijenjati postavke predmemorije za konfiguraciju na disku i na kraju implementirati na VM kao niz DS VM s priloženom diskova.

### <a name="vm-storage-bandwidth-and-vhd-storage-throughput"></a>Propusnosti VM prostora za pohranu i propusnost VHD prostora za pohranu

Količinu prostora za pohranu performanse ovisi o definirane veličine DS * VM i veličine VHD. Na VMs imaju različite dostupan za broj VHDs koje se mogu priložiti i Maksimalna propusnost oni će podržavati (MB/s). Brojevi određene propusnosti potražite u članku [virtualnog računala i veličine servisa oblaka za Azure](virtual-machines-linux-sizes.md).

Povećana IOPS postići s veće disk. Trebali biste to kada mišljenje o vašem put migracije. Detalje [potražite u tablici IOPS i vrste Disk](../storage-premium-storage.md#scalability-and-performance-targets-when-using-premium-storage).

Na kraju, preporučuje se da imaju VMs drugi Maksimalna disk bandwidths oni će podrške za sve diskova priložene. Velikom opterećenju nije zasititi dostupna za tu ulogu veličina VM propusnost Maksimalna disk. Na primjer će podržavati na Standard_DS14 do 512 MB/s; Stoga s tri P30 diskova nije zasititi propusnost diska na VM. No u ovom primjeru može biti prekoračeno ograničenje propusnost ovisno o kombinacije čitanje i pisanje IOs.

## <a name="new-deployments"></a>Novi implementacije

Sljedeća dva odjeljka demonstrirati uvođenje SQL Server VMs Premium za pohranu. Kao što je rečeno prije ne moraju nužno biti potrebno da biste smjestili na disku OS na Premium prostora za pohranu. Možete učiniti ako se intending da biste postavili intenzivno radnih opterećenja IO na OS VHD.

Prvi primjer pokazuje korištenja postojeće slike Azure galerije. Drugi primjer pokazuje kako koristiti prilagođenu sliku VM koji se nalaze u postojeći račun standardne prostora za pohranu.

> [AZURE.NOTE] U ovim se primjerima pretpostavlja da ste već stvorili regionalne VNET.

### <a name="create-a-new-vm-with-premium-storage-with-gallery-image"></a>Stvaranje nove VM s Premium prostora za pohranu pomoću galerije

U primjeru u nastavku prikazuje kako postavite VHD OS na premium prostora za pohranu i priložite VHDs Premium prostora za pohranu. Međutim, postaviti OS disk u standardni prostora za pohranu računa, a zatim priložite VHDs koje se nalaze na računu za pohranu Premium. Planirati su oba scenarija.

    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Set up subscription
    Set-AzureSubscription -SubscriptionName $mysubscription
    Select-AzureSubscription -SubscriptionName $mysubscription -Current  

#### <a name="step-1-create-a-premium-storage-account"></a>Korak 1: Stvaranje računa za pohranu Premium


    #Create Premium Storage account, note Type
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  


#### <a name="step-2-create-a-new-cloud-service"></a>Korak 2: Stvaranje novog servis u Oblaku

    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location


#### <a name="step-3-reserve-a-cloud-service-vip-optional"></a>Korak 3: Rezervirati u oblak servisa VIP (nije obavezno)
    #check exisitng reserved VIP
    Get-AzureReservedIP

    $reservedVIPName = “sqlcloudVIP”
    New-AzureReservedIP –ReservedIPName $reservedVIPName –Label $reservedVIPName –Location $location

#### <a name="step-4-create-a-vm-container"></a>Korak 4: Stvaranje spremnika VM
    #Generate storage keys for later
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    ##Generate storage acc contexts
    $xioContext = New-AzureStorageContext –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary   

    #Create container
    $containerName = 'vhds'
    New-AzureStorageContainer -Name $containerName -Context $xioContext

#### <a name="step-5-placing-os-vhd-on-standard-or-premium-storage"></a>Korak 5: Postavljanje OS VHD na standardni prikaz ili Premium prostora za pohranu
    #NOTE: Set up subscription and default storage account which will be used to place the OS VHD in

    #If you want to place the OS VHD Premium Storage Account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount  $newxiostorageaccountname  

    #If you wanted to place the OS VHD Standard Storage Account but attach Premium Storage VHDs then you would run this instead:
    $standardstorageaccountname = "danstdams"

    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount  $standardstorageaccountname

#### <a name="step-6-create-vm"></a>Korak 6: Stvaranje VM
    #Get list of available SQL Server Images from the Azure Image Gallery.
    $galleryImage = Get-AzureVMImage | where-object {$_.ImageName -like "*SQL*2014*Enterprise*"}
    $image = $galleryImage.ImageName

    #Set up Machine Specific Information
    $vmName = "dsDan1"
    $vnet = "dansvnetwesteur"
    $subnet = "SQL"
    $ipaddr = "192.168.0.8"

    #Remember to change to DS series VM
    $newInstanceSize = "Standard_DS1"

    #create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS"

    #Machine User Credentials
    $userName = "myadmin"
    $pass = "mycomplexpwd4*"

    #Create VM Config
    $vmConfigsl = New-AzureVMConfig -Name $vmName -InstanceSize $newInstanceSize -ImageName $image  -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` -AdminUserName $userName -Password $pass | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Add Data and Log Disks to VM Config
    #Note the size specified ‘-DiskSizeInGB 1023’, this will attach 2 x P30 Premium Storage Disk Type
    #Utilising the Premium Storage enabled Storage account

    $vmConfigsl | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 0 -HostCaching "ReadOnly"  -DiskLabel "DataDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-data1.vhd"
    $vmConfigsl | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 1 -HostCaching "None"  -DiskLabel "logDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-log1.vhd"

    #Create VM
    $vmConfigsl  | New-AzureVM –ServiceName $destcloudsvc -VNetName $vnet ## Optional (-ReservedIPName $reservedVIPName)  

    #Add RDP Endpoint
    $EndpointNameRDPInt = "3389"
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName | Add-AzureEndpoint -Name "EndpointNameRDP" -Protocol "TCP" -PublicPort "53385" -LocalPort $EndpointNameRDPInt  | Update-AzureVM

    #Check VHD storage account, these should be in $newxiostorageaccountname
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName | Get-AzureDataDisk
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName |Get-AzureOSDisk


### <a name="create-a-new-vm-to-use-premium-storage-with-a-custom-image"></a>Stvaranje nove VM da biste koristili Premium prostora za pohranu s prilagođenu sliku

Ovaj scenarij pokazuje gdje ste postojeće prilagođene slike koje se nalaze u standardni prostora za pohranu računa. Kao što je rečeno ako želite smjestiti OS VHD na pohranu Premium morat ćete kopirati slike koji postoji u standardni prostora za pohranu računa i prenijeti Premium prostora za pohranu prije nego što se može koristiti. Ako imate sliku u lokalnu, možete koristiti i ovaj postupak da biste kopirali koji izravno na račun za pohranu Premium.

#### <a name="step-1-create-storage-account"></a>Korak 1: Stvaranje računa za pohranu
    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Create Premium Storage account
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  

    #Standard Storage account
    $origstorageaccountname = "danstdams"

#### <a name="step-2-create-cloud-service"></a>Korak 2 Stvaranje servis u Oblaku
    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location


#### <a name="step-3-use-existing-image"></a>Korak 3: Korištenje postojeće slike
Možete koristiti postojeće slike. Ili, možete [poduzeti sliku postojeće računala](virtual-machines-windows-classic-capture-image.md). Imajte na umu strojno slike koji ne moraju biti DS* računala. Nakon što dodate sliku, na sljedeći način pokazati kako kopirati na račun za pohranu Premium s na * *Start AzureStorageBlobCopy** cmdleta ljuske PowerShell.

    #Get storage account keys:
    #Standard Storage account
    $originalstorage =  Get-AzureStorageKey -StorageAccountName $origstorageaccountname
    #Premium Storage account
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    #Set up contexts for the storage accounts:
    $origContext = New-AzureStorageContext  –StorageAccountName $origstorageaccountname -StorageAccountKey $originalstorage.Primary
    $destContext = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary  

#### <a name="step-4-copy-blob-between-storage-accounts"></a>Korak 4: Kopiranje Blob između računa za pohranu
    #Get Image VHD
    $myImageVHD = "dansoldonorsql2k14-os-2015-04-15.vhd"
    $containerName = 'vhds'

    #Copy Blob between accounts
    $blob = Start-AzureStorageBlobCopy -SrcBlob $myImageVHD -SrcContainer $containerName `
    -DestContainer vhds -Destblob "prem-$myImageVHD" `
    -Context $origContext -DestContext $destContext  

#### <a name="step-5-regularly-check-copy-status"></a>Korak 5: Redovito Provjera stanja kopija:
    $blob | Get-AzureStorageBlobCopyState

#### <a name="step-6-add-image-disk-to-azure-disk-repository-in-subscription"></a>Korak 6: Dodavanje slike na disku Azure disk spremište u pretplatu
    $imageMediaLocation = $destContext.BlobEndPoint+"/"+$myImageVHD
    $newimageName = "prem"+"dansoldonorsql2k14"

    Add-AzureVMImage -ImageName $newimageName -MediaLocation $imageMediaLocation

> [AZURE.NOTE] Može se dogoditi čak i ako se status izvješća kao uspjeh, i dalje ne može se pojaviti pogreška Zakup disk. U ovom slučaju, pričekajte otprilike 10 minuta.

#### <a name="step-7--build-the-vm"></a>Korak 7: Stvaranje na VM
Ovdje su izgradnju u VM iz sliku i spajanje dva VHDs prostora za pohranu Premium:

    $newimageName = "prem"+"dansoldonorsql2k14"
    #Set up Machine Specific Information
    $vmName = "dansolchild"
    $vnet = "westeur"
    $subnet = "Clients"
    $ipaddr = "192.168.0.41"

    #This will need to be a new cloud service
    $destcloudsvc = "danregsvcamsxio2"

    #Use to DS Series VM
    $newInstanceSize = "Standard_DS1"

    #create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS3"

    #Machine User Credentials
    $userName = "myadmin"
    $pass = "theM)stC0mplexP@ssw0rd!”


    #Create VM Config
    $vmConfigsl2 = New-AzureVMConfig -Name $vmName -InstanceSize $newInstanceSize -ImageName $newimageName  -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` -AdminUserName $userName -Password $pass | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    $vmConfigsl2 | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 0 -HostCaching "ReadOnly"  -DiskLabel "DataDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-Datadisk-1.vhd"
    $vmConfigsl2 | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 1 -HostCaching "None"  -DiskLabel "LogDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-logdisk-1.vhd"



    $vmConfigsl2 | New-AzureVM –ServiceName $destcloudsvc -VNetName $vnet

## <a name="existing-deployments-that-do-not-use-always-on-availability-groups"></a>Postojeće implementacijama koje koriste uvijek na grupe dostupnosti

> [AZURE.NOTE] Za postojeće implementacije, najprije u odjeljku [preduvjeti](#prerequisites-for-premium-storage) ove teme.

Postoje različite okolnosti pri implementaciji sustava SQL Server koji koriste uvijek na dostupnost grupe i one koje želite. Ako ne koristite uvijek na, a imate postojeće samostalne SQL Server, pomoću novog oblaka i servis za pohranu računa možete nadograditi Premium prostora za pohranu. Imajte na umu sljedeće mogućnosti:

- **Stvaranje nove VM poslužitelja SQL**. Novi VM SQL Server koja koristi račun Premium prostora za pohranu, možete stvarati kao navedenih u novi implementacije. Zatim sigurnosno kopiranje i vraćanje sustava SQL Server Konfiguracija korisnika baza podataka i. Aplikacija će morati ažurirati referentni nove SQL Server ako ga je pristupa interno ili kao vanjski. Trebat ćete da biste kopirali sve objekte za 'iz db"kao da ste radili tijekom migracije usporedno SQL Server (SxS). To obuhvaća objekte kao što su prijave, potvrde i povezani poslužitelji.
- **Migriranje na postojeće VM SQL Server**. To je potrebno izvanmrežno uzimajući VM za SQL Server, a zatim prijenos na novi servis u oblaku, koja sadrži sve njegove priloženom VHDs kopiranje račun Premium prostora za pohranu. Kada se na VM online, aplikacija će se referencirati naziv glavnog poslužitelja kao prije. Imajte na umu da će veličini postojeće diska utjecati na performanse karakteristike. Ako, na primjer, na disku 400 GB dobiva zaokružuje naviše na P20. Ako znate da ne zahtijeva koji performanse diska, pa možete ponovno stvoriti VM kao VM za niz DS i priložite Premium prostora za pohranu VHDs opisa veličina/performanse ako su vam potrebne. Zatim možete odvajanje i ponovno priložiti datoteke baze podataka SQL.

> [AZURE.NOTE] Kada kopiranje diskova VHD Imajte na umu da veličine, ovisno o veličini prekinu vrsti Premium prostora za pohranu Disk podijeljene su, time se određuje specifikacija performanse diska. Azure će zaokruživanje na najbliži disk veličinu, pa ako imate 400 GB disk, to će se zaokružuje naviše na P20. Po vlastitom nahođenju postojeće IO od OS VHD ne bi dobro da to migrirati s računom za pohranu Premium.

Ako je SQL Server vanjsko pristupa, promijenit će servisa VIP oblaka. Također morat ćete ažuriranja krajnje točke, ACL-a i DNS postavke.

## <a name="existing-deployments-that-use-always-on-availability-groups"></a>Postojeće implementacijama koje koriste uvijek na grupe dostupnosti

> [AZURE.NOTE] Za postojeće implementacije, najprije u odjeljku [preduvjeti](#prerequisites-for-premium-storage) ove teme.

Na početku u ovom odjeljku ne možemo će pogledajte uvijek na interakciju s Azure s mrežom. Ne možemo će zatim raščlanjuju migracije u dva scenarija: Migracija na kojem je moguće tolerated neke nedostupnost i migracije koju morate postigli minimalnog nedostupnost.

Lokalnog sustava SQL Server uvijek na dostupnost grupe pomoću programa ga Slušatelj lokalnog koja registrira virtualne DNS naziva uz IP adresu koja se zajednički koriste SQL poslužitelja. Kada se klijenti povezuju su usmjerena kroz IP ga slušatelj na primarni SQL Server. To je poslužitelj vlasništvu uvijek na IP resursa u tom trenutku.

![DeploymentsUseAlways na][6]

U Microsoft Azure imate samo jedan IP adresa dodijeljena NIC na na VM, tako da biste postigli iste sloj apstrakcije kao informacije o lokalnom Azure koristi IP adresu koja vam je dodijeljen učitavanja Balancers interno/vanjsko (ILB na ELB). IP resurs koji se zajednički koristiti poslužitelje postavljena na istu IP kao ILB/ELB. To je objaviti u DNS-om, a klijent promet prošli kroz ILB/ELB da biste replike primarni SQL Server. ILB/ELB zna što je SQL Server primarni jer koristi probes da bi isprobati uvijek na IP resursa. U prethodnom primjeru je probes svaki čvor koji sadrži krajnje referencira ELB/ILB, ovisno o tome što odgovara primarni SQL Server.

> [AZURE.NOTE] ILB i ELB oba dodjeljuju se na određenom Azure u oblaku, stoga sve oblaka migracije u Azure najvjerojatnije prekinu promijenit će se IP raspoređivača opterećenja.

### <a name="migrating-always-on-deployments-that-can-allow-some-downtime"></a>Migriranje uvijek na uvođenja koje možete omogućiti neke nedostupnost

Postoje dvije strategije će se migrirati uvijek na uvođenja koje omogućuju neke nedostupnost:

1. **Dodavanje više sekundarnog replike na postojeće uvijek na klaster**
1. **Migracija na novu uvijek na klaster**

#### <a name="1-add-more-secondary-replicas-to-an-existing-always-on-cluster"></a>1. dodati više sekundarnog replike na postojeće uvijek na klaster

Jedna od mogućih strategija je da biste dodali više secondaries uvijek na dostupnost grupi. Morate dodati ih u novi servis u oblaku i ažurirajte ga slušatelj novi IP raspoređivača opterećenja.

##### <a name="points-of-downtime"></a>Upućuje nedostupnost:

- Provjera valjanosti klaster.
- Testiranje uvijek na failovers nove Secondaries.

Ako koristite Windows prostora za pohranu grupe unutar na VM za veću propusnost IO, zatim te će se izvanmrežno tijekom cijelog provjere valjanosti klaster. Prilikom dodavanja čvorove klaster, potrebna je provjera valjanosti. Vrijeme potrebno za pokretanje test mogu se razlikovati, da bi se trebali biste to provjerili u predstavniku testnom okruženju da biste dobili djelomičnog doba koliko to može potrajati.

Trebali biste Dodjela vrijeme u kojem možete izvršiti ručno prebacivanje i chaos testiranje na novododani čvorove da biste bili sigurni uvijek na visoke dostupnosti funkcije prema očekivanjima.

![DeploymentUseAlways On2][7]

> [AZURE.NOTE] Trebali biste zaustaviti sve instance sustava SQL Server na kojima se koriste grupe za pohranu prije pokretanja provjere valjanosti.
##### <a name="high-level-steps"></a>Više razine korake

1. Stvorite dva nova SQL poslužitelja u novi u oblaku s priloženom Premium prostora za pohranu.
1. Kopiranje preko CIJELOG sigurnosnog kopiranja i vraćanja s **NORECOVERY**.
1. Kopirajte putem "iz korisnik DB" zavisne objekte, primjerice prijave itd.
1. Stvaranje novih u novi interni učitavanja opterećenja (ILB) ili pomoću programa raspoređivača učitavanja vanjskih (ELB) i postavite učitavanja raspoređen krajnje točke na oba novi čvorovi.
> [AZURE.NOTE] Provjerite sve čvorove imati ispravne konfiguraciju krajnju točku prije nastavka

1. Prekid pristupa korisnički program sustava SQL Server (Ako koristite za pohranu grupe).
1. Zaustavljanje servisa SQL Server modul Services na sve čvorove (Ako koristite za pohranu grupe).
1. Dodajte novi čvorovi skupine i pokretanje cijelog provjere valjanosti.
1. Kada je provjera valjanosti uspješan, pokrenite sve servise sustava SQL Server.
1. Sigurnosno kopiranje zapisnicima transakcija i vraćati baze podataka za korisnika.
1. Dodajte novi čvorovi u uvijek na dostupnost grupu i postavite replikacije u **Synchronous**.
1. Dodavanje resursa IP adresa od u novi oblaka servisa ILB/ELB putem komponente PowerShell za uvijek na koji se temelji na više web-mjesta na primjer u [dodatak](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage). U sustavu Windows Klasteriranje postavite **mogućih vlasnika** resursa **IP adrese** na novi čvorovi stari. [Dodatak](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage)u odjeljku "Dodavanje IP adresa resursa na istoj podmreži".
1. Prebacivanje na neki od novi čvorovi.
1. Unesite novi čvorovi test i automatsko prebacivanje partnere failovers.
1. Uklanjanje grupe dostupnosti izvorne čvorove.

##### <a name="advantages"></a>Prednosti

- Novi SQL Server može biti testirati (SQL Server i aplikacija) prije nego što se dodaju uvijek na.
- Možete promijeniti veličinu VM i prilagodbu prostora za pohranu po potrebi točan. Međutim, bi korisni da bi svi putovi datoteke SQL isti.
- Možete kontrolirati pokretanja prijenos sigurnosne kopije baze podataka na sekundarnog replike. Razlikuje se od pomoću cmdleta Azure **Start AzureStorageBlobCopy** da biste kopirali VHDs, jer je asinkronog Kopiraj.

##### <a name="disadvantages"></a>Nedostaci
- Kada koristite Windows prostora za pohranu grupe, postoji klaster nedostupnost tijekom cijelog klaster provjere valjanosti za nove dodatne čvorove.
- Ovisno o verziji SQL Server i postojeći broj sekundarnog replike, možda nećete moći dodati više sekundarnog replike ne uklonite postojeće secondaries.
- Mogući su SQL podataka prijenos dugo pri postavljanju sustava secondaries.
- Postoji dodatni trošak tijekom migracije dok novog računala koji se izvode paralelno.

#### <a name="2-migrate-to-a-new-always-on-cluster"></a>2. Migracija na novu uvijek na klaster

Drugi strategije je stvoriti novost uvijek na klaster čvorove novost u novi u oblaku i preusmjeravanje klijenata za korištenje.

##### <a name="points-of-downtime"></a>Točke nedostupnost

Postoji nedostupnost kada prenosite aplikacija i korisnika da biste ga slušatelj za nove uvijek na. Na nedostupnost ovisi o:

- Vrijeme potrebno za vraćanje sigurnosne kopije zapisnika konačni transakcije baze podataka na nove poslužitelje.
- Vrijeme potrebno da biste ažurirali klijentske aplikacije da biste koristili nova stalna ga slušatelj.

##### <a name="advantages"></a>Prednosti

- Možete testirati i stvarne radnog okruženja, SQL Server i OS izraditi promjene.
- Imate mogućnost za prilagodbu prostora za pohranu i potencijalno smanjiti veličinu VM. To može rezultirati smanjenja trošak.
- Tijekom tog postupka možete ažurirati Sastavi SQL Server ili verziju. Također možete nadograditi operacijski sustav.
- Prethodni uvijek na klaster može poslužiti kao cilj pune vraćanje.

##### <a name="disadvantages"></a>Nedostaci

- Morate promijeniti DNS naziva ga slušatelj ako želite da se oba uvijek na klastere radi istodobno. Time se dodaje Administracija indirektni tijekom migracije kao nizovi aplikacije klijenta mora odražavaju novi naziv ga Slušatelj.
- Morate provesti mehanizam za sinkronizaciju između ta dva okruženja njihovo što bliže moguće da biste minimizirali preduvjeti konačni sinkronizacije prije migracije.
- Došlo je dodatni trošak tijekom migracije dok novom okruženju na pokrenut.

### <a name="migrating-always-on-deployments-for-minimal-downtime"></a>Migriranje uvijek na implementacija Minimalna nedostupnost

Postoje dvije Strategije implementacije za migriranje uvijek na za minimalnog nedostupnost:

1. **Korištenje postojeće sekundarni: jednog web-mjesta**
1. **Korištenje postojeće sekundarne Replica(s): više web-mjesta**

#### <a name="1-utilize-an-existing-secondary-single-site"></a>1. koristiti postojeće sekundarni: jednog web-mjesta

Jedan strategija minimalnog nedostupnost je da biste snimili postojeće oblaka sekundarne i uklonite ga s trenutnom servis u oblaku. Zatim kopirajte na VHDs novi račun za pohranu Premium i stvorite na VM u novi servis u oblaku. Zatim ažurirajte ga slušatelj u Klasteriranje i prebacivanje.

##### <a name="points-of-downtime"></a>Točke nedostupnost

- Postoji nedostupnost nakon što ažurirate čvor konačno s krajnju točku raspoređen Učitaj.
- Ponovno povezivanje vaš klijent se može odgoditi ovisno o konfiguraciji vašeg klijenta/DNS.
- Postoji dodatni nedostupnost Ako odaberete da bi uvijek na klaster grupi izvan mreže da biste zamijenili članak IP adrese. Izbjegavanje ovog pomoću ili ovisnost i mogućih vlasnika za dodatnu resursa IP adrese. [Dodatak](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage)u odjeljku "Dodavanje IP adresa resursa na istoj podmreži".

> [AZURE.NOTE] Ako želite dodatnu čvor da biste partake u kao uvijek na prebacivanje Partner, morate dodati krajnje Azure s referencom na učitavanja raspoređen skup. Kada pokrenete naredbu **Dodaj AzureEndpoint** da biste to učinili, trenutni veze ostati otvoren, ali nove veze da biste ga slušatelj nećete moći uspostaviti dok je ažurirao raspoređivača opterećenja. U ovo testiranje je vidjeti na zadnji 90-120seconds, trebali biste to testira.

##### <a name="advantages"></a>Prednosti

- Nema extra trošak nastale tijekom migracije.
- Ikone migracije.
- Smanjene složenosti.
- Omogućuje za veću IOPS od Premium prostora za pohranu SKU-ove. Kad u diskova odvojeni od na VM i kopirali u novu servis u oblaku, s 3 strana alata mogu se povećati veličinu VHD koji omogućuje veće throughputs. Povećanje veličine VHD potražite u članku [forum rasprave](https://social.msdn.microsoft.com/Forums/azure/4a9bcc9e-e5bf-4125-9994-7c154c9b0d52/resizing-azure-data-disk?forum=WAVirtualMachinesforWindows).

##### <a name="disadvantages"></a>Nedostaci

- Tijekom migracije postoji privremeni gubitak HA i DR.
- Budući da je 1:1 migracije, morat ćete koristiti Minimalna veličina VM koji će podržavati broj VHDs, pa možda nećete moći downsize vaše VMs.
- Ovaj scenarij koristi cmdleta Azure **Start AzureStorageBlobCopy** koji je asinkronog. Postoji bez SLA nakon dovršetka Kopiraj. Vrijeme kopije ovisi dok to ovisi o čekanja u redu čekanja će i ovise o količinu podataka za prijenos. Vrijeme Kopiraj povećava ako prijenos će drugi Azure podatkovnom centru koji podržava Premium prostora za pohranu u drugoj regiji. Ako imate 2 čvorove, razmislite o moguće ublažiti u slučaju kopiju traje dulje nego u testiranja. To može obuhvaćati sljedeće ideje.
    - Dodajte privremene 3 čvor SQL Server za HA prije migracije s dogovoreni isključiti.
    - Pokrenite migraciju izvan Azure zakazano održavanje.
    - Provjerite je li imati ispravno konfigurirano vaše kvorum klaster.  

##### <a name="high-level-steps"></a>Više razine korake

Ovaj dokument ne sadrži potpuni primjer end da biste završetka, no [dodatak](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage) sadrži detalje koji mogu leveraged da biste izvršili taj.

![MinimalDowntime][8]

- Prikupite konfiguracija diska, a zatim Ukloni čvor (izbrišite priložene VHDs).
- Stvaranje računa za pohranu Premium i kopirajte VHDs s računa za standardne prostora za pohranu
- Stvaranje nove servis u oblaku i ponovno implementirate VM SQL2 u taj servis u oblaku. Stvaranje VM pomoću izvorne kopirane OS VHD i pridruživanje kopirane VHDs.
- Konfiguriranje ILB / ELB i dodavanje krajnje točke.
- Ažurirajte ga Slušatelj nešto od sljedećeg:
    - Izvan mreže poduzimanja uvijek na grupu i ažuriranje uvijek na ga Slušatelj s novi ILB / ELB IP adresa.
    - Ili dodavanju resursa IP adrese od novi oblaka servisa ILB/ELB putem komponente PowerShell u sustavu Windows Klasteriranje. Pa postavite mogućih vlasnika resursa IP adrese migriranim čvor SQL2, i to postaviti kao ovisnost ili na naziv mreže. [Dodatak](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage)u odjeljku "Dodavanje IP adresa resursa na istoj podmreži".
- Provjeri DNS konfiguraciju/prijenosa za klijente.
- Migriranje SQL1 VM i proći kroz korake 2 – 4.
- Ako koristite 5ii korake, zatim dodajte SQL1 kao mogući vlasnik dodane resursa IP adresa
- Testirajte failovers.

#### <a name="2-utilize-existing-secondary-replicas-multi-site"></a>2. koristiti postojeće sekundarne replica(s): više web-mjesta

Ako imate čvorove u više od jedne Azure podatkovnog centra (Kontroler) ili ako imate hibridnog okruženja, pa možete koristiti uvijek na konfiguraciju u ovom okruženju da biste minimizirali prekid u radu.

Pristup je da biste promijenili sinkronizacije uvijek na Synchronous za lokalni ili sekundarne Kontroler Azure i zatim prebacivanje preko tog sustava SQL Server. Zatim kopirajte na VHDs s računom za pohranu Premium i implementirati na računalu u novi servis u oblaku. Ažurirajte ga slušatelj, a neće vratiti.

##### <a name="points-of-downtime"></a>Točke nedostupnost

Na nedostupnost sastoji se od vremena za prebacivanje na drugu Kontroler i natrag. Ovisi o vašoj konfiguraciji klijenta/DNS-a, a zatim ponovno povezivanje vaš klijent se može odgoditi.
Razmislite o sljedećem primjeru hibridnu konfiguraciju uvijek na:

![MultiSite1][9]

##### <a name="advantages"></a>Prednosti

- Možete koristiti postojeću infrastrukturu.
- Imate mogućnost prije nadogradnje Azure spremište na Kontroler DR Azure na prvi put.
- Moguće je rekonfigurirati DR Azure Kontroler prostora za pohranu.
- Postoji najmanje dva failovers tijekom migracije, isključujući test failovers.
- Ne morate premještanje podataka sustava SQL Server pomoću sigurnosnog kopiranja i vraćanja.

##### <a name="disadvantages"></a>Nedostaci

- Ovisno o klijentski pristup za SQL Server možda postoje povećana kašnjenje kada je SQL Server pokrenut u drugu Kontroler aplikaciji.
- Vrijeme Kopiraj VHDs za pohranu Premium može biti dugo. To može utjecati na vaša odluka o tome da biste zadržali čvor u grupi dostupnost. Razmislite o sljedećem kada zapisnika intenzivno opterećenje koristite tijekom migracije je obavezna, budući da primarni čvor morat će zadržati unreplicated transakcije njegov zapisnik transakcija. Stoga to može rasti znatno.
- Ovaj scenarij koristi cmdleta Azure **Start AzureStorageBlobCopy** koji je asinkronog. Nakon dovršetka postoji bez SLA. Vrijeme kopije mijenja se, a to ovisi o čekanja u redu čekanja, također će ovise o količinu podataka za prijenos. Stoga samo jedan čvor u imate 2 podatkovnog centra, ublažiti korake morate poduzeti u slučaju kopiju traje dulje nego u testiranja. To može obuhvaćati sljedeće ideje.
    - Dodajte privremene 2 SQL čvor za HA prije migracije s dogovoreni isključiti.
    - Pokrenite migraciju izvan Azure zakazano održavanje.
    - Provjerite je li imati ispravno konfigurirano vaše kvorum klaster.

Ovaj scenarij pretpostavlja da ste navedenih instalacijom i znate mapiranje prostora za pohranu da bi se promjene postavki predmemorije optimalnih disk.

##### <a name="high-level-steps"></a>Više razine korake
![Multisite2][10]

- Provjerite na lokalni / alternativni Kontroler Azure SQL Server primarni i ukloniti ga u druge automatsko prebacivanje partnera (AFP).
- Prikupljanje podataka o konfiguraciji diska iz SQL2 i uklanjanje čvor (izbrišite priloženom VHDs).
- Stvaranje računa za pohranu Premium i kopirajte VHDs s računa za standardne prostora za pohranu.
- Stvaranje nove servise u oblaku i stvaranje SQL2 VM s njegova diskova za pohranu premije priložene.
- Konfiguriranje ILB / ELB i dodavanje krajnje točke.
- Ažurirajte uvijek na ga Slušatelj nove ILB / ELB IP adresa i testiranje prebacivanje.
- Provjerite DNS konfiguraciju.
- Promijenite u AFP SQL2, i migrirati SQL1 i proći kroz korake 2 – 5.
- Testirajte failovers.
- Prebacite se na AFP ponovno SQL1 i SQL2

## <a name="appendix-migrating-a-multisite-always-on-cluster-to-premium-storage"></a>Dodatak: Migracija Multisite uvijek na klaster Premium prostora za pohranu

Ostatak u ovoj se temi sadrži detaljne primjer pretvaranja klaster uvijek na više web-mjesta za pohranu Premium. Ga i pretvara ga Slušatelj s vanjskim raspoređivača opterećenja (ELB) možete koristiti u interne raspoređivača opterećenja (ILB).

### <a name="environment"></a>Okruženje

- Windows 2k 12 / SQL 2k 12
- 1 DB datoteke na SP
- 2 x grupe za pohranu po čvor

![Appendix1][11]

### <a name="vm"></a>VM:

U ovom primjeru smo prolaze da bismo pokazali prijelaz s programa ELB na ILB. ELB bile dostupne prije ILB, tako da pokazuje kako se prebaciti na to tijekom migracije.

![Appendix2][12]

### <a name="pre-steps-connect-to-subscription"></a>Koraci pre: Povezivanje s pretplate

    Add-AzureAccount

    #Set up subscription
    Get-AzureSubscription

#### <a name="step-1-create-new-storage-account-and-cloud-service"></a>Korak 1: Stvaranje novog računa za pohranu i servis u Oblaku
    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Storage accounts
    #current storage account where the vm to migrate resides
    $origstorageaccountname = "danstdams"

    #Create Premium Storage account
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  

    #Generate storage keys for later
    $originalstorage =  Get-AzureStorageKey -StorageAccountName $origstorageaccountname
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    #Generate storage acc contexts
    $origContext = New-AzureStorageContext  –StorageAccountName $origstorageaccountname -StorageAccountKey $originalstorage.Primary
    $xioContext = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary  

    #Set up subscription and default storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $origstorageaccountname
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #CREATE NEW CLOUD SVC
    $vnet = "dansvnetwesteur"

    ##Existing cloud service
    $sourceSvc="dansolSrcAms"

    ##Create new cloud service
    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location

#### <a name="step-2-increase-the-permitted-failures-on-resources-optional"></a>Korak 2: Povećati dopušteno neuspjeha resursa<Optional>
Na određene resurse koji pripadaju uvijek na dostupnost grupi postoje ograničenja na koliko pogreške koji se pojavljuju u razdoblju, gdje će pokušati klaster servisa da biste ponovno pokrenuli grupu resursa. Preporučuje se to povećati whilst koje su prolaska kroz ovaj postupak od ako ne ručno prebacivanje i okidača failovers tako da isključite strojeva možete dobiti blizu to ograničenje.

To je dobro koristiti dvostruki odbitak pogreške, da biste to učinili u upravitelju prebacivanje klaster, otvorite svojstva grupe uvijek na resursa:

![Appendix3][13]

Promijenite maksimalnu neuspjeha 6.

#### <a name="step-3-addition-ip-address-resource-for-cluster-group-optional"></a>Korak 3: Zbrajanje IP adresa resursa za grupu klaster<Optional>

Ako imate samo jedan IP adresa za grupu klaster, a to je poravnat prema podmreže oblaka, ne zaboravite, ako slučajno prenositi izvanmrežno sve čvorove klaster u oblaku na mreži zatim resursa IP klaster, a naziv mreže klaster nećete moći prijavi. U slučaju to će ga na druge resurse za klaster spriječiti ažuriranja.

#### <a name="step-4-dns-configuration"></a>Korak 4: Konfiguriranje DNS

Za primjenu na jednostavni prijelaza ovisi kako se DNS-a koristi se i ažurirati.
Uvijek na kojima je instalirana komponenta, stvara grupu resursa klaster Windows ako otvorite upravitelj klaster prebacivanje, vidjet ćete da je će imati barem tri resursa, dva koji se odnosi na dokument:

- Virtualna Network Name (VNN) – to je naziv DNS tog klijenta povezati kad zanima povezuje s poslužiteljima SQL putem uvijek na.
- IP adresa resursa – to je IP adresa koji je povezan s na VNN, imate više od jedne i u dodjeljivanju resursa konfiguraciji imat ćete IP adresu po web-mjesta/podmreže.

Prilikom povezivanja sa sustavom SQL Server, SQL Server Client upravljački program će dohvatiti DNS zapise pridružene ga slušatelj i pokušate se povezati s svaki uvijek na pridruženom IP adresa, ispod smo govori o nekoliko čimbenika koji možete utjecati to.

Broj Istodobni DNS zapisa koji su povezani s nazivom ga slušatelj ovisi o ne samo broj IP adresu povezanu, ali ' RegisterAllIpProviders'setting u klasteriranja uvijek Uključeno VNN resursa.

Ako pokrenete uvijek na servisu Azure postoje različite korake da biste stvorili ga Slušatelj i IP adrese, ćete morati ručno konfigurirati 'RegisterAllIpProviders' 1, to je različite na lokalnu implementaciju uvijek na kojem je već postavljen na 1.

Ako 'RegisterAllIpProviders' je 0, a zatim vidjet ćete samo jedan DNS zapis u DNS pridružene ga Slušatelj:

![Appendix4][14]

Ako je 'RegisterAllIpProviders' 1:

![Appendix5][15]

Ispod kod će ispis izgleda VNN postavke i postaviti umjesto vas, imajte na umu da se promjene primijenile morat ćete izvanmrežni na VNN i pretvoriti u mrežni način, ovaj uzimajući u ga Slušatelj izvanmrežne uzrokuje prekidu povezivanje klijenta.

    ##Always On Listener Name
    $ListenerName = "Mylistener"
    ##Get AlwaysOn Network Name Settings
    Get-ClusterResource $ListenerName| Get-ClusterParameter
    ##Set RegisterAllProvidersIP
    Get-ClusterResource $ListenerName| Set-ClusterParameter RegisterAllProvidersIP  1

U noviji koraka migracije ćete morati ažurirati uvijek na ga slušatelj ažurirane IP adresu koja se pozivati raspoređivača opterećenja to obuhvaćaju Uklanjanje resursa IP adresa i zbrajanje. Nakon ažuriranja IP morate provjeriti novu IP adresu ažurirana u DNS Zone i klijente ažurirate lokalni DNS predmemoriju.

Ako klijentima nalaze u segmentu drugog mrežnog i referencu drugi DNS poslužitelj, treba uzeti u obzir što se događa o DNS Zone prijenos tijekom migracije aplikacije povezati put će biti ograničeno tako da barem Zone prijenos vrijeme novi IP adrese da ga slušatelj. Ako ste u odjeljku ograničenje vremena ovdje, trebali biste raspravljati i testiranje prisilno o prijenosu rastuće zone sa svojim timovima Windows i postaviti DNS zapis glavnog računala da biste na donjem vrijeme važenja (TTL), pa klijente ažuriranje. Dodatne informacije potražite u članku [Rastuće Zone prijenosi](https://technet.microsoft.com/library/cc958973.aspx) i [Početak DnsServerZoneTransfer](https://technet.microsoft.com/library/jj649917.aspx).

Prema zadanim postavkama TTL za DNS zapis koji je povezan s ga Slušatelj uvijek na servisu Azure je 1200 sekundi. Možda želite smanjiti ako se nalazite u odjeljku vrijeme ograničenja tijekom migraciju da biste bili sigurni klijente ažurirati svojim DNS ažurirane IP adresa da ga slušatelj. Možete pogledati i izmijeniti konfiguraciju tako da se pražnjenje out konfiguraciju u VNN:

    $AGName = "myProductionAG"
    $ListenerName = "Mylistener"
    #Look at HostRecordTTL
    Get-ClusterResource $ListenerName| Get-ClusterParameter

    #Set HostRecordTTL Examples
    Get-ClusterResource $ListenerName| Set-ClusterParameter -Name "HostRecordTTL" 120

Uzmite u obzir, na donjem na 'HostRecordTTL', doći će do veće količine prometa u DNS-a.

##### <a name="client-application-settings"></a>Postavke aplikacije klijenta

Ako klijentsku aplikaciju SQL podržava .net 4,5 SQLClient, a možete koristiti "MULTISUBNETFAILOVER = TRUE' ključnu riječ, to se preporučuje da se primjenjuje kao što omogućuje brže veze s SQL uvijek na grupe dostupnosti tijekom prebacivanje. Zbraja kroz sve IP adrese pridruženu stalna ga slušatelj paralelno i izvodi redovno brzinu TCP veze pokušaj tijekom na prebacivanje.

Dodatne informacije o postavkama iznad, pročitajte članak [MultiSubnetFailover ključne riječi i povezane značajke](https://msdn.microsoft.com/library/hh213080.aspx#MultiSubnetFailover). Pogledati i [SqlClient podrška za visoke dostupnosti, Izrada oporavak](https://msdn.microsoft.com/library/hh205662(v=vs.110).aspx).

#### <a name="step-5-cluster-quorum-settings"></a>Korak 5: Klaster kvorum postavke

Kao što je namjeravate poduzimanja out barem jedan SQL Server prema dolje po, izmjenu kvorum postavku klaster u ako pomoću datoteke zrcaljenja za zajedničko korištenje (FSW) 2 čvorove, postavite kvorum omogućuje Dopusti Većina čvor i korištenje dinamički glasovanja i to je da biste dopustili jedan čvor bude položaj.


    Set-ClusterQuorum -NodeMajority  

Dodatne informacije o upravljanju i konfiguriranju kvorum klaster, pročitajte članak [Konfiguriranje i upravljanje kvorum prebacivanje klasteru Windows Server 2012](https://technet.microsoft.com/library/jj612870.aspx).

#### <a name="step-6-extract-existing-endpoints-and-acls"></a>Korak 6: Izdvajanje postojeće krajnje točke i ACL-a
    #GET Endpoint info
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmNameToMigrate | Get-AzureEndpoint
    #GET ACL Rules for Each EP, this example is for the Always On Endpoint
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmNameToMigrate | Get-AzureAclConfig -EndpointName "myAOEndPoint-LB"  

Spremite ih u tekstnu datoteku.

#### <a name="step-7-change-failover-partners-and-replication-modes"></a>Korak 7: Promjena partnera za prebacivanje Načini rada s replikacijom

Ako imate više od 2 SQL Server, trebali biste promijeniti prebacivanje drugi sekundarni u drugom Kontroler ili u lokalnu 'Synchronous' i njezino programa partnera prebacivanje automatsko (AFP), to je da bi se održavanje HA whilst mijenjate. To možete učiniti pomoću TSQL od izmjena kroz SSMS:

![Appendix6][16]

#### <a name="step-8-remove-secondary-vm-from-cloud-service"></a>8 korak: Uklanjanje sekundarne VM servis u oblaku

Trebali biste Planiranje migriranja oblaka sekundarne čvor najprije ako je trenutno primarni, trebali biste započeli ručno prebacivanje.

    $vmNameToMigrate="dansqlams2"

    #Check machine status
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

    #Shutdown secondary VM
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | stop-AzureVM


    #Extract disk configuration

    ##Building Existing Data Disk Configuration
    $file = "C:\Azure Storage Testing\mydiskconfig_$vmNameToMigrate.csv"
    $datadisks = @(Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureDataDisk )
    Add-Content $file “lun, vhdname, hostcaching, disklabel, diskName”
    foreach ($disk in $datadisks)
    {
      $vhdname = $disk.MediaLink.AbsolutePath -creplace  "/vhds/"
      $disk.Lun, , $disk.HostCaching, $vhdname, $disk.DiskLabel,$disks.DiskName
    # Write-Host "copying disk $disk"
    $adddisk = "{0},{1},{2},{3},{4}" -f $disk.Lun,$vhdname, $disk.HostCaching, $disk.DiskLabel, $disk.DiskName
    $adddisk | add-content -path $file
    }

    #Get OS Disk
    $osdisks = Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureOSDisk ## | select -ExpandProperty MediaLink
    $osvhdname = $osdisks.MediaLink.AbsolutePath -creplace  "/vhds/"
    $osdisks.OS, $osdisks.HostCaching, $osvhdname, $osdisks.DiskLabel, $osdisks.DiskName
    $addosdisk = "{0},{1},{2},{3},{4}" -f $osdisks.OS,$osvhdname, $osdisks.HostCaching, $osdisks.Disklabel , $osdisks.DiskName
    $addosdisk | add-content -path $file

    #Import disk config
    $diskobjects  = Import-CSV $file

    #Check disk config, make sure below returns the disks associated with the VM
    $diskobjects

    #Identify OS Disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osdiskforbuild = $osdiskimport.diskName

    #Check machibe is off
    Get-AzureVM -ServiceName $sourceSvc -Name  $vmNameToMigrate

    #Drop machine and rebuild to new cls
    Remove-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

#### <a name="step-9-change-disk-caching-settings-in-csv-file-and-save"></a>Korak 9: Promjena diska predmemoriranje postavki u CSV datoteku i spremanje

Za količine podataka te mora biti postavljena na samo za ČITANJE.

Za TLOG količine te mora biti postavljena na NIŠTA.

![Appendix7][17]

#### <a name="step-10-copy-vhds"></a>Korak 10: Kopiranje VHDS
    #Ensure you have created the container for these:
    $containerName = 'vhds'

    #Create container
    New-AzureStorageContainer -Name $containerName -Context $xioContext

    ####DISK COPYING####
    #Get disks from csv, get settings for each VHDs and copy to Premium Storage accoun
    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName
       Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname has cache setting : $cacheoption"

       #Start async copy
       Start-AzureStorageBlobCopy -srcUri "https://$origstorageaccountname.blob.core.windows.net/vhds/$vhdname" `
    -SrcContext $origContext `
    -DestContainer $containerName `
    -DestBlob $vhdname `
    -DestContext $xioContext
       }



Možete provjeriti status Kopiraj VHDs na račun za pohranu Premium:

    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName

       $copystate = Get-AzureStorageBlobCopyState -Blob $vhdname -Container $containerName -Context $xioContext
    Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname, STATUS = " $copystate.Status
       }

![Appendix8][18]

Pričekajte dok se sve to bilježe kao uspjeh.

Informacije za pojedinačne blob-ova:

    Get-AzureStorageBlobCopyState -Blob "blobname.vhd" -Container $containerName -Context $xioContext

#### <a name="step-11-register-os-disk"></a>Korak 11: Na disku Register OS

    #Change storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountname
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #Register OS disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osvhd = $osdiskimport.vhdname
    $osdiskforbuild = $osdiskimport.diskName

    #Registering OS disk, but as XIO disk
    $xioDiskName = $osdiskforbuild + "xio"
    Add-AzureDisk -DiskName $xioDiskName -MediaLocation  "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$osvhd"  -Label "BootDisk" -OS "Windows"

#### <a name="step-12-import-secondary-into-new-cloud-service"></a>Korak 12: Uvoz sekundarne u novi servis u oblaku

Kod ispod i koristi mogućnost dodane Ovdje možete uvesti na računalu i koristiti retainable VIP.

    #Build VM Config
    $ipaddr = "192.168.0.5"
    #Remember to change to XIO
    $newInstanceSize = "Standard_DS13"
    $subnet = "SQL"

    #Create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS"

    #build machine config into object
    $vmConfig = New-AzureVMConfig -Name $vmNameToMigrate -InstanceSize $newInstanceSize -DiskName $xioDiskName -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Reload disk config
    $diskobjects  = Import-CSV $file
    $datadiskimport = $diskobjects | where {$_.lun -ne "Windows"}

    ForEach ( $attachdatadisk in $datadiskimport)
       {
    $label = $attachdatadisk.disklabel
    $lunNo = $attachdatadisk.lun
    $hostcach = $attachdatadisk.hostcaching
    $datadiskforbuild = $attachdatadisk.diskName
    $vhdname = $attachdatadisk.vhdname

    ###Attaching disks to a VM during a deploy to a new cloud service and new storage account is different from just attaching VHDs to just a redeploy in a new cloud service
    $vmConfig | Add-AzureDataDisk -ImportFrom -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vhdname" -LUN $lunNo -HostCaching $hostcach -DiskLabel $label

    }

    #Create VM
    $vmConfig  | New-AzureVM –ServiceName $destcloudsvc –Location $location -VNetName $vnet ## Optional (-ReservedIPName $reservedVIPName)

#### <a name="step-13-create-ilb-on-new-cloud-svc-add-load-balanced-endpoints-and-acls"></a>Korak 13: Stvaranje ILB na novu oblaka Svc, dodajte učitavanja raspoređen krajnje točke i ACL-a
    #Check for existing ILB
    GET-AzureInternalLoadBalancer -ServiceName $destcloudsvc

    $ilb="sqlIntIlbDest"
    $subnet = "SQL"
    $IP="192.168.0.25"
    Add-AzureInternalLoadBalancer -ServiceName $destcloudsvc -InternalLoadBalancerName $ilb –SubnetName $subnet –StaticVNetIPAddress $IP

    #Endpoints
    $epname="sqlIntEP"
    $prot="tcp"
    $locport=1433
    $pubport=1433
    Get-AzureVM –ServiceName $destcloudsvc –Name $vmNameToMigrate  | Add-AzureEndpoint -Name $epname -Protocol $prot -LocalPort $locport -PublicPort $pubport -ProbePort 59999 -ProbeIntervalInSeconds 5 -ProbeTimeoutInSeconds 11  -ProbeProtocol "TCP" -InternalLoadBalancerName $ilb -LBSetName $ilb -DirectServerReturn $true | Update-AzureVM

    #SET Azure ACLs or Network Security Groups & Windows FWs

    #http://msdn.microsoft.com/library/azure/dn495192.aspx

    ####WAIT FOR FULL AlwaysOn RESYNCRONISATION!!!!!!!!!#####

####<a name="step-14-update-always-on"></a>Korak 14: Ažuriranje uvijek na
    #Code to be executed on a Cluster Node
    $ClusterNetworkNameAmsterdam = "Cluster Network 2" # the azure cluster subnet network name
    $newCloudServiceIPAmsterdam = "192.168.0.25" # IP address of your cloud service

    $AGName = "myProductionAG"
    $ListenerName = "Mylistener"


    Add-ClusterResource "IP Address $newCloudServiceIPAmsterdam" -ResourceType "IP Address" -Group $AGName -ErrorAction Stop |  Set-ClusterParameter -Multiple @{"Address"="$newCloudServiceIPAmsterdam";"ProbePort"="59999";SubnetMask="255.255.255.255";"Network"=$ClusterNetworkNameAmsterdam;"OverrideAddressMatch"=1;"EnableDhcp"=0} -ErrorAction Stop

    #set dependancy and NETBIOS, then remove old IP address

    #set NETBIOS, then remove old IP address
    Get-ClusterGroup $AGName | Get-ClusterResource -Name "IP Address $newCloudServiceIPAmsterdam" | Set-ClusterParameter -Name EnableNetBIOS -Value 0

    #set dependency to Listener (OR Dependency) and delete previous IP Address resource that references:

    #Make sure no static records in DNS

![Appendix9][19]

Sada ukloniti servis stare oblaka IP adresa.

![Appendix10][20]

#### <a name="step-15-dns-update-check"></a>Korak 15: DNS traženje ažuriranja

Sada treba Provjera DNS poslužitelji klijentskog mreža sustava SQL Server i provjerite je li Klasteriranje je dodan dodatni glavno računalo zapisa za dodatnu IP adresa. Ako te DNS poslužitelji su ažurirani, razmislite o prisilno DNS Zone prijenos i provjerite klijente u dva podmreže možete riješiti na oba uvijek na IP adresa, to je da morate pričekati automatsko replikacije DNS.

#### <a name="step-16-reconfigure-always-on"></a>Korak 16: Konfigurirajte uvijek na

U ovom trenutku čekanja za sekundarne taj čvor koji je prenijeli u potpunosti ponovno sinkroniziranje s čvor na lokaciji i prijeđite na sinkrono replikacije čvor i provjerite na AFP.  

#### <a name="step-17-migrate-second-node"></a>Korak 17: Migracija drugi čvor
    $vmNameToMigrate="dansqlams1"

    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

    #Get endpoint information
    $endpoint = Get-AzureVM -ServiceName $sourceSvc  -Name $vmNameToMigrate | Get-AzureEndpoint

    #Shutdown VM
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | stop-AzureVM

    #Get disk config

    #Building Existing Data Disk Configuration
    $file = "C:\Azure Storage Testing\mydiskconfig_$vmNameToMigrate.csv"
    $datadisks = @(Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureDataDisk )
    Add-Content $file “lun, vhdname, hostcaching, disklabel, diskName”
    foreach ($disk in $datadisks)
    {
      $vhdname = $disk.MediaLink.AbsolutePath -creplace  "/vhds/"
      $disk.Lun, , $disk.HostCaching, $vhdname, $disk.DiskLabel,$disks.DiskName
    # Write-Host "copying disk $disk"
    $adddisk = "{0},{1},{2},{3},{4}" -f $disk.Lun,$vhdname, $disk.HostCaching, $disk.DiskLabel, $disk.DiskName
    $adddisk | add-content -path $file
    }

    #Get OS Disk
    $osdisks = Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureOSDisk ## | select -ExpandProperty MediaLink
    $osvhdname = $osdisks.MediaLink.AbsolutePath -creplace  "/vhds/"
    $osdisks.OS, $osdisks.HostCaching, $osvhdname, $osdisks.DiskLabel, $osdisks.DiskName
    $addosdisk = "{0},{1},{2},{3},{4}" -f $osdisks.OS,$osvhdname, $osdisks.HostCaching, $osdisks.Disklabel , $osdisks.DiskName
    $addosdisk | add-content -path $file

    #Import disk config
    $diskobjects  = Import-CSV $file

    #Check disk configuration
    $diskobjects

    #Identify OS Disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osdiskforbuild = $osdiskimport.diskName

    #Check machine is off
    Get-AzureVM -ServiceName $sourceSvc -Name  $vmNameToMigrate

    #Drop machine and rebuild to new cls
    Remove-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

#### <a name="step-18-change-disk-caching-settings-in-csv-file-and-save"></a>18 korak: Promjena diska predmemoriranje postavki u CSV datoteku i spremanje

Za količine podataka te mora biti postavljena na samo za ČITANJE.

Za TLOG jedinicama te mora biti postavljena na NIŠTA.

![Appendix11][21]

#### <a name="step-19-create-new-independent-storage-account-for-secondary-node"></a>19 korak: Stvaranje novog računa neovisno prostora za pohranu za sekundarne čvor
    $newxiostorageaccountnamenode2 = "danspremsams2"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountnamenode2 -Location $location -Type "Premium_LRS"  

    #Reset the storage account src if node 1 in a different storage account
    $origstorageaccountname2nd = "danstdams2"

    #Generate storage keys for later
    $xiostoragenode2 = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountnamenode2

    #Generate storage acc contexts
    $xioContextnode2 = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountnamenode2 -StorageAccountKey $xiostoragenode2.Primary  

    #Set up subscription and default storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountnamenode2
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

#### <a name="step-20-copy-vhds"></a>20 korak: Kopiranje VHDS
    #Ensure you have created the container for these:
    $containerName = 'vhds'

    #Create container
    New-AzureStorageContainer -Name $containerName -Context $xioContextnode2  

    ####DISK COPYING####
    ##get disks from csv, get settings for each VHDs and copy to Premium Storage accoun
    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName
       Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname has cache setting : $cacheoption"

       #Start async copy
       Start-AzureStorageBlobCopy -srcUri "https://$origstorageaccountname2nd.blob.core.windows.net/vhds/$vhdname" `
        -SrcContext $origContext `
        -DestContainer $containerName `
        -DestBlob $vhdname `
        -DestContext $xioContextnode2
       }

    #Check for copy progress

    #check induvidual blob status
    Get-AzureStorageBlobCopyState -Blob "danRegSvcAms-dansqlams1-2014-07-03.vhd" -Container $containerName -Context $xioContext


Možete provjeriti status Kopiraj VHD sve VHDs: ForEach ($disk u $diskobjects) {$lun = $disk. Lun $vhdname = $disk.vhdname $cacheoption = $disk. HostCaching $disklabel = $disk. DiskLabel $diskName = $disk. DiskName

       $copystate = Get-AzureStorageBlobCopyState -Blob $vhdname -Container $containerName -Context $xioContextnode2
    Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname, STATUS = " $copystate.Status
       }

![Appendix12][22]

Pričekajte dok se sve to bilježe kao uspjeh.

Informacije za pojedinačne blob-ova: #Check induvidual blob status Get-AzureStorageBlobCopyState-bloba "danRegSvcAms-dansqlams1-2014.-07-03.vhd"-spremnik $containerName-kontekst $xioContextnode2

#### <a name="step-21-register-os-disk"></a>Korak 21: Na disku Register OS
    #change storage account to the new XIO storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountnamenode2
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #Register OS disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osvhd = $osdiskimport.vhdname
    $osdiskforbuild = $osdiskimport.diskName

    #Registering OS disk, but as XIO disk
    $xioDiskName = $osdiskforbuild + "xio"
    Add-AzureDisk -DiskName $xioDiskName -MediaLocation  "https://$newxiostorageaccountnamenode2.blob.core.windows.net/vhds/$osvhd"  -Label "BootDisk" -OS "Windows"

    #Build VM Config
    $ipaddr = "192.168.0.4"
    $newInstanceSize = "Standard_DS13"

    #Join to existing Avaiability Set

    #Build machine config into object
    $vmConfig = New-AzureVMConfig -Name $vmNameToMigrate -InstanceSize $newInstanceSize -DiskName $xioDiskName -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Reload disk config
    $diskobjects  = Import-CSV $file
    $datadiskimport = $diskobjects | where {$_.lun -ne "Windows"}

    ForEach ( $attachdatadisk in $datadiskimport)
       {
    $label = $attachdatadisk.disklabel
    $lunNo = $attachdatadisk.lun
    $hostcach = $attachdatadisk.hostcaching
    $datadiskforbuild = $attachdatadisk.diskName
    $vhdname = $attachdatadisk.vhdname

    ###This is different to just a straight cloud service change
    #note if you do not have a disk label the command below will fail, populate as required.
    $vmConfig | Add-AzureDataDisk -ImportFrom -MediaLocation "https://$newxiostorageaccountnamenode2.blob.core.windows.net/vhds/$vhdname" -LUN $lunNo -HostCaching $hostcach -DiskLabel $label

    }

    #Create VM
    $vmConfig  | New-AzureVM –ServiceName $destcloudsvc –Location $location -VNetName $vnet -Verbose

#### <a name="step-22-add-load-balanced-endpoints-and-acls"></a>22 korak: Dodavanje učitavanja raspoređen krajnje točke i ACL-a
    #Endpoints
    $epname="sqlIntEP"
    $prot="tcp"
    $locport=1433
    $pubport=1433
    Get-AzureVM –ServiceName $destcloudsvc –Name $vmNameToMigrate  | Add-AzureEndpoint -Name $epname -Protocol $prot -LocalPort $locport -PublicPort $pubport -ProbePort 59999 -ProbeIntervalInSeconds 5 -ProbeTimeoutInSeconds 11  -ProbeProtocol "TCP" -InternalLoadBalancerName $ilb -LBSetName $ilb -DirectServerReturn $true | Update-AzureVM


    #STOP!!! CHECK in the Azure classic portal or Machine Endpoints through powershell that these Endpoints are created!

    #SET ACLs or Azure Network Security Groups & Windows FWs

    #http://msdn.microsoft.com/library/azure/dn495192.aspx

#### <a name="step-23-test-failover"></a>Korak 23: Prebacivanje Test

Sada obavijestite čvor migriranim sinkronizirati s na lokaciji uvijek na čvor, postavite ga u načinu sinkrono replikacije i pričekajte dok se ne sinkronizira se. Zatim prebacivanje iz lokalno prvi čvor migrirati, koji se na AFP. Kada koji je radio, promijenite zadnji migriranim čvor u AFP.

Trebali biste testirati failovers između sve čvorove i pokretanje iako očekuje testira chaos da biste bili sigurni failovers rada kao i u pravovremeno manor.

#### <a name="step-24-put-back-cluster-quorum-settings--dns-ttl--failover-pntrs--sync-settings"></a>Korak 24: Ponovno postavljanje klaster kvorum postavke / DNS TTL / prebacivanje Pntrs / postavke sinkronizacije
##### <a name="adding-ip-address-resource-on-same-subnet"></a>Dodavanje resursa IP adresu na istoj podmreži

Ako imate samo 2 SQL Server i želite migrirati ih na novi servis u oblaku, ali želite zadržati ih na istoj podmreži, možete izbjeći poduzimanja ga slušatelj izvan mreže da biste izbrisali izvorna uvijek na IP adresa i dodajte nove IP adresa. Ako premještate u VMs u drugom podmreži ne morat ćete to učiniti jer će biti programa dodatne klaster mreža koja se pozivati te podmreže.

Nakon što ne unese gore migriranim sekundarne i dodali u novi resurs IP adresa za novi servis u oblaku prije prebacivanje postojeći primarni, trebali biste poduzmite sljedeće korake unutar klaster prebacivanje upravitelju:

Da biste dodali u IP adresu, potražite u članku [dodatak](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage), korak 14.

1. Trenutni resursa IP adrese, promijenite mogući vlasnik na 'Postojeći primarni SQL Server', u primjeru u nastavku, 'dansqlams4':

    ![Appendix13][23]

1. Novi resurs IP adresa promijenite mogući vlasnik na 'Migrated sekundarne SQL Server', u primjeru u nastavku, 'dansqlams5':

    ![Appendix14][24]

1. Nakon postavljanja to možete učiniti prebacivanje, a kada je zadnji čvor migrira se mogućih vlasnika treba urediti tako da se taj čvor biti dodano u obliku mogući vlasnik:

    ![Appendix15][25]

## <a name="additional-resources"></a>Dodatni resursi
- [Prostor za pohranu Azure Premium](../storage/storage-premium-storage.md)
- [Virtualnim strojevima](https://azure.microsoft.com/services/virtual-machines/)
- [SQL Server na virtualnim računalima za Azure](virtual-machines-windows-sql-server-iaas-overview.md)

<!-- IMAGES -->
[1]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/1_VNET_Portal.png
[2]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/2_Diskname_Lun.png
[3]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/3_Virtual_Disk_Properties.png
[4]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/4_Virtual_Disk_Properties_Details.png
[5]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/5_Get_Storage_Pool.png
[6]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/6_Deployments_Always_On.png
[7]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/7_Add_More_Secondaries.png
[8]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/8_Use_Existing_Secondary_Single_Site.png
[9]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/9_Use_Existing_Secondary_Multi_Site.png
[10]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/9_Use_Existing_Secondary_Multi_Site_b.png
[11]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_01.png
[12]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_02.png
[13]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_03.png
[14]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_04.png
[15]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_05.png
[16]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_06.png
[17]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_07.png
[18]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_08.png
[19]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_09.png
[20]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_10.png
[21]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_11.png
[22]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_12.png
[23]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_13.png
[24]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_14.png
[25]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_15.png
