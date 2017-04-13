<properties
    pageTitle="Stvaranje virtualnog računala za SQL Server u ljusci PowerShell Azure (Voditelj resursa) | Microsoft Azure"
    description="Nudi korake i skripti ljuske PowerShell za stvaranje programa Azure VM sa slikama Galerija virtualnog računala za SQL Server."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-resource-manager" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/25/2016"
    ms.author="jroth"/>

# <a name="provision-a-sql-server-virtual-machine-using-azure-powershell-resource-manager"></a>Dodjela resursa u SQL Server virtualnog računala pomoću komponente PowerShell Azure (Voditelj resursa)

> [AZURE.SELECTOR]
- [Portal](virtual-machines-windows-portal-sql-server-provision.md)
- [PowerShell](virtual-machines-windows-ps-sql-create.md)

## <a name="overview"></a>Pregled

Pomoću ovog praktičnog vodiča prikazuje kako stvoriti na jednom Azure virtualnog računala pomoću modela implementaciju **Upravljanja resursima Azure** pomoću cmdleta ljuske PowerShell Azure. U ovom ćete praktičnom vodiču ćemo će stvoriti jednu virtualnog računala pomoću jedne pogon diska slike u galeriji SQL. Ne možemo će stvoriti novu davatelja usluga za pohranu, mreže i računalnim resursa koji će se koristiti virtualnog računala. Ako imate postojeću davatelja usluga za bilo koju od tih resursa, možete koristiti tih davatelja usluga.

Ako vam je potrebna klasičnu verziju ove teme, potražite u članku [Dodjeljivanje virtualni stroj SQL Server pomoću Azure PowerShell klasični](virtual-machines-windows-classic-ps-sql-create.md).

## <a name="prerequisites"></a>Preduvjeti

Za ovaj vodič morat ćete:

- Račun za Azure i pretplate prije nego što počnete. Ako ga nemate, prijavite se za [besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/).
- [Azure PowerShell)](../powershell-install-configure.md), minimalna verzija 1.4.0 ili noviji (ovog praktičnog vodiča napisali pomoću verzije 1.5.0).
    - Da biste dohvatili verziji, upišite **Get-modul Azure ListAvailable**.

## <a name="configure-your-subscription"></a>Konfiguriranje pretplate

Otvorite Windows PowerShell i uspostaviti pristup računu Azure tako da pokrenete sljedeći cmdlet. Primit ćete s na zaslonu za prijavu unesite vjerodajnice. Koristite e-pošte i lozinku koje koristite za prijavu na portal za Azure.

    Add-AzureRmAccount

Nakon uspješnog prijave vidjet ćete neke informacije na zaslonu koja sadrži SubscriptionId s kojima ste se prijavili u. Ovo je pretplata u kojem će se stvoriti resursi za ovog praktičnog vodiča osim ako promijenite neku drugu pretplatu. Ako imate više SubscriptionIds, pokrenite sljedeći cmdlet da biste se vratili na popis svih vaše SubscriptionIds:

    Get-AzureRmSubscription

Da biste promijenili druge SubscriptionID, pokrenite sljedeći cmdlet s na željenom SubscriptionId.

    Select-AzureRmSubscription -SubscriptionId xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

## <a name="define-image-variables"></a>Definiranje varijable slika

Da biste pojednostavnili upotrebljivosti i razumijevanja dovršene skriptu iz ovog praktičnog vodiča, ne možemo počet će definiranjem broju varijabli. Promijenite vrijednosti parametara prema svojim potrebama, ali čuvajte imenovanja ograničenja vezane uz naziv duljine i posebnih znakova prilikom promjene vrijednosti navedene.

### <a name="location-and-resource-group"></a>Mjesto i grupa resursa
Pomoću dvije varijable da biste odredili područje podataka i grupa resursa u koju ćete stvoriti ostale resurse za virtualnog računala.

Izmijenite po želji, a zatim izvršite sljedeće Cmdlete Inicijalizacija tih varijabli.

    $Location = "SouthCentralUS"
    $ResourceGroupName = "sqlvm1"

### <a name="storage-properties"></a>Svojstva za pohranu

Koristite sljedeće varijable za definiranje računa za pohranu i vrsta mjesta za pohranu koriste virtualnog računala.

Izmijenite po želji, a zatim izvršite sljedeći cmdlet za inicijalizaciju tih varijabli. Imajte na umu da u ovom primjeru smo koristite [Za pohranu Premium](../storage/storage-premium-storage.md), što je preporučeno za radnih opterećenja radnog. Dodatne informacije o tome upute i druge preporuke potražite u članku [performanse najbolje prakse za SQL Server na virtualnim strojevima sa sustavom Azure](virtual-machines-windows-sql-performance.md).

    $StorageName = $ResourceGroupName + "storage"
    $StorageSku = "Premium_LRS"

### <a name="network-properties"></a>Svojstva mreže

Koristite sljedeće varijable za definiranje mrežnog sučelja, način dodjele TCP/IP, naziv virtualne mreže, naziv virtualne podmreže, raspon IP adresa za virtualne mreže, raspon IP adresa za podmreži i oznaku naziv domene za javno koja će se koristiti putem mreže u virtualnog računala.

Izmijenite po želji, a zatim izvršite sljedeći cmdlet za inicijalizaciju tih varijabli.

    $InterfaceName = $ResourceGroupName + "ServerInterface"
    $TCPIPAllocationMethod = "Dynamic"
    $VNetName = $ResourceGroupName + "VNet"
    $SubnetName = "Default"
    $VNetAddressPrefix = "10.0.0.0/16"
    $VNetSubnetAddressPrefix = "10.0.0.0/24"
    $DomainName = "sqlvm1"   

### <a name="virtual-machine-properties"></a>Svojstva virtualnog računala

Koristite sljedeće varijable za definiranje naziva virtualnog računala, naziv računala, veličina virtualnog računala i naziv diska operacijski sustav za virtualnog računala.

Izmijenite po želji, a zatim izvršite sljedeći cmdlet za inicijalizaciju tih varijabli.

    $VMName = $ResourceGroupName + "VM"
    $ComputerName = $ResourceGroupName + "Server"
    $VMSize = "Standard_DS13"
    $OSDiskName = $VMName + "OSDisk"

### <a name="image-properties"></a>Svojstva slike

Koristite sljedeće varijable za definiranje sliku koja se koristi za virtualnog računala. U ovom se primjeru koristi se slika SQL Server 2016 Enterprise.

Izmijenite po želji, a zatim izvršite sljedeći cmdlet za inicijalizaciju tih varijabli.

    $PublisherName = "MicrosoftSQLServer"
    $OfferName = "SQL2016-WS2016"
    $Sku = "Enterprise"
    $Version = "latest"

Imajte na umu da ćete dobiti cijeli popis ponude slika sustava SQL Server pomoću naredbe Get-AzureRmVMImageOffer:

    Get-AzureRmVMImageOffer -Location 'East US' -Publisher 'MicrosoftSQLServer'

I vidjet ćete SKU-ove za programa nuditi naredbom Get-AzureRmVMImageSku. Sljedeća naredba prikazuje sve SKU-ove za **SQL2014SP1 WS2012R2** nude.

    Get-AzureRmVMImageSku -Location 'East US' -Publisher 'MicrosoftSQLServer' -Offer 'SQL2014SP1-WS2012R2' | Select Skus

## <a name="create-a-resource-group"></a>Stvaranje grupa resursa

Uvođenje model upravljanja resursima prvi objekt koji ste stvorili je grupu resursa. Koristit ćemo cmdleta [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/mt759837.aspx) da biste stvorili grupu Azure resursa i njegovih resursa resursa naziv i mjesto definira varijabli koje ste prethodno pokrenut.

Pokrenite sljedeći cmdlet da biste stvorili novu grupu resursa.

    New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

## <a name="create-a-storage-account"></a>Stvaranje računa za pohranu

Virtualnog računala potreban je resursa za pohranu na disku operacijski sustav i za datoteke sustava SQL Server podataka i zapisnika. Zbog jednostavnosti, ne možemo će stvoriti jedan disk za oba. Možete priložiti dodatnih diskova kasnije pomoću cmdleta [Dodaj Azure Disk](https://msdn.microsoft.com/library/azure/dn495252.aspx) da biste mogli smjestiti podatke iz sustava SQL Server i prijavu datoteka na namjenski diskova. Koristit ćemo cmdleta [New AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) stvaranje računa standardnu prostora za pohranu u novu grupu resursa i uz naziv računa za pohranu, za pohranu Sku naziv i mjesto definirati pomoću varijabli koje ste prethodno pokrenut.

Pokrenite sljedeći cmdlet da biste stvorili novi račun za pohranu.  

    $StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location

## <a name="create-network-resources"></a>Stvaranje mrežni resursi

Virtualnog računala potreban broj mrežni resursi za veza s mrežom.

- Svaki virtualnog računala potreban je virtualne mreže.
- Virtualne mreže, morate imati barem jedan podmreže definiran.
- Mrežno sučelje potrebno je definirati javnom ili privatne IP adrese.

### <a name="create-a-virtual-network-subnet-configuration"></a>Stvaranje konfiguraciju podmreži virtualne mreže

Ne možemo počet će stvaranjem konfiguracija podmreže za naše virtualne mreže. Za naše vodič ćemo će stvoriti podmreži zadani pomoću cmdleta [New-AzureRmVirtualNetworkSubnetConfig](https://msdn.microsoft.com/library/mt619412.aspx) . Ne možemo stvorit će naš virtualne konfiguracije podmreže prefiksom podmreže ime i adresu definirati pomoću varijabli koje ste prethodno pokrenut.

>[AZURE.NOTE] Možete definirati dodatna svojstva konfiguracija podmreže virtualne mreže pomoću ovaj cmdlet, ali koji izlazi iz ovog praktičnog vodiča.

Pokrenite sljedeći cmdlet da biste stvorili konfiguraciju virtualne podmreže.

    $SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix

### <a name="create-a-virtual-network"></a>Stvaranje virtualne mreže

Zatim ćemo stvoriti naš virtualne mreže pomoću cmdleta [New-AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603657.aspx) . Ne možemo ćete stvoriti naš virtualne mreže u novu grupu resursa, ima naziv, mjesto i adresu prefiks definirani pomoću varijabli koje ste prethodno pokrenut, a pomoću konfiguracije podmreže koji ste definirali u prethodnom koraku.

Pokrenite sljedeći cmdlet da biste stvorili virtualne mreže.

    $VNet = New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig

### <a name="create-the-public-ip-address"></a>Stvaranje na javnu IP adresu

Imamo naš virtualne mreže definirane, moramo konfiguriranje IP adresa za povezivanje virtualnog računala. Za ovaj vodič ćemo će stvoriti javnu IP adresu pomoću dinamičke IP adresiranja za podršku s Internetom. Koristit ćemo cmdleta [New-AzureRmPublicIpAddress](https://msdn.microsoft.com/library/mt603620.aspx) da biste stvorili javnu IP adresu u grupu resursa koji je stvorio prevously i uz naziv, mjesto, način dodjele i definirati pomoću varijabli koje ste prethodno pokrenut na naljepnice za DNS domene naziv.

>[AZURE.NOTE] Možete definirati dodatna svojstva javnu IP adresu pomoću ovaj cmdlet, ali koji izlazi iz ovog početne praktičnog vodiča. Nije moguće stvoriti i privatni adresu ili adresu s statičke adrese, ali koji je izvan opsega ovog praktičnog vodiča.

Pokrenite sljedeći cmdlet da biste stvorili javnu IP adresa.

    $PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName

### <a name="create-the-network-interface"></a>Stvaranje mrežnog sučelja

Spremni smo da biste stvorili mrežnog sučelja koji će koristiti naše virtualnog računala. Koristit ćemo cmdleta [New-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619370.aspx) da biste stvorili naš mrežnog sučelja u stvorili ranije i s imenom, mjesto, podmreže i javnu IP adresu prethodno definirane grupe resursa.

Pokrenite sljedeći cmdlet da biste stvorili mrežno sučelje.

    $Interface = New-AzureRmNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id

## <a name="configure-a-vm-object"></a>Konfiguriranje VM objekta

Imamo resursa za pohranu i mrežne definirane, ne možemo spremni za definiranje računalnim resursa za virtualnog računala. Za naše vodič smo će odredite veličinu virtualnog računala i razne značajke operacijski sustav, navedite mrežno sučelje koje ćemo ranije stvorili definiranje blobova, a zatim navedite disk operacijski sustav.

### <a name="create-the-vm-object"></a>Stvaranje objekta VM

Ne možemo počet će navođenjem veličina virtualnog računala. Ne možemo navodite na DS13 za ovog praktičnog vodiča. Koristit ćemo cmdleta [New-AzureRmVMConfig](https://msdn.microsoft.com/library/mt603727.aspx) da biste stvorili objekt programa konfigurirati virtualnog računala s imenom i veličine definirati pomoću varijabli koje ste prethodno pokrenut.

Pokrenite sljedeći cmdlet da biste stvorili objekt virtualnog računala.

    $VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize

### <a name="create-a-credential-object-to-hold-the-name-and-password-for-the-local-administrator-credentials"></a>Stvaranje objekta vjerodajnica za držite ime i lozinku za vjerodajnica za lokalni administrator

Prije nego što smo možete postaviti svojstva virtualnog računala operacijski sustav, moramo navesti vjerodajnica za lokalni administratorski račun kao sigurnu niz. Da biste to postigli, koristit ćemo cmdlet [Get-vjerodajnica](https://technet.microsoft.com/library/hh849815.aspx) .

Izvršavanje sljedeći cmdlet i u prozoru zahtjeva vjerodajnica komponente Windows PowerShell upišite ime i lozinku za lokalni administratorski račun u sustavu Windows virtualnog računala.

    $Credential = Get-Credential -Message "Type the name and password of the local administrator account."

### <a name="set-the-operating-system-properties-for-the-virtual-machine"></a>Postavljanje svojstava operacijski sustav za virtualnog računala

Sada ćemo spremni da biste postavili svojstva virtualnog računala operacijski sustav. Koristit ćemo cmdlet [Skup AzureRmVMOperatingSystem](https://msdn.microsoft.com/library/mt603843.aspx) postavljen vrstu operacijski sustav Windows, zahtijevaju [virtualnog računala agent](virtual-machines-windows-classic-agents-and-extensions.md) se instalirati, odrediti da cmdlet omogućuje automatsko ažuriranje i postavite naziv virtualnog računala, naziv računala i vjerodajnica pomoću varijabli koje ste prethodno pokrenut.

Pokrenite sljedeći cmdlet da biste postavili svojstva operacijski sustav virtualnog računala.

    $VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate

### <a name="add-the-network-interface-to-the-virtual-machine"></a>Dodavanje mrežnog sučelja virtualnog računala

Nakon toga dodat ćemo sučelje mreže koju smo stvorili prethodno za virtualnog računala. Koristit ćemo cmdlet za [Dodavanje AzureRmVMNetworkInterface](https://msdn.microsoft.com/library/mt619351.aspx) da biste dodali mrežno sučelje korištenjem varijable sučelja mreže koju ste ranije definirali.

Pokrenite sljedeći cmdlet postavljanje mrežnog sučelja za virtualnog računala.

    $VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id

### <a name="set-the-blob-storage-location-for-the-disk-to-be-used-by-the-virtual-machine"></a>Postavljanje mjesta za pohranu blob diska koriste virtualnog računala

Nakon toga ne možemo postaviti blob mjesta za pohranu za disk koriste virtualnog računala pomoću varijabli koje ste definirali neke starije verzije.

Pokrenite sljedeći cmdlet da biste postavili mjesto spremišta blobova platforme.

    $OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"

### <a name="set-the-operating-system-disk-properties-for-the-virtual-machine"></a>Postavljanje operacijski sustav na disku svojstva virtualnog računala

Zatim ćemo će postavite operacijski sustav na disku svojstva virtualnog računala. Koristit ćemo cmdlet [Skup AzureRmVMOSDisk](https://msdn.microsoft.com/library/mt603746.aspx) da biste odredili u operacijskom sustavu virtualnog računala će potjecati iz sliku da biste postavili predmemoriranje čitati samo (jer je SQL Server instaliran na istom disku) i definirati naziv virtualnog računala i na disku operacijski sustav definirati pomoću varijabli koji ste definirali neke starije verzije.

Pokrenite sljedeći cmdlet da biste postavili operacijski sustav na disku svojstva virtualnog računala.

    $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage

### <a name="specify-the-platform-image-for-the-virtual-machine"></a>Navedite sliku platformu za virtualnog računala

Naš posljednji korak konfiguriranje je da biste odredili slike platformu za naše virtualnog računala. Za naše vodič smo koristite najnoviju SQL Server 2016 CTP slike. Koristit ćemo cmdlet [Skup AzureRmVMSourceImage](https://msdn.microsoft.com/library/mt619344.aspx) da biste koristili sliku kako je definirano varijabli koje ste definirali neke starije verzije.

Pokrenite sljedeći cmdlet da biste odredili slike platformu za virtualnog računala.

    $VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version

## <a name="create-the-sql-vm"></a>Stvaranje SQL VM

Sad kad ste gotovi s navedeni koraci za konfiguraciju, spremni ste za stvaranje virtualnog računala. Koristit ćemo cmdleta [New-AzureRmVM](https://msdn.microsoft.com/library/mt603754.aspx) da biste stvorili virtualnog računala pomoću varijabli koje ste definirali.

Pokrenite sljedeći cmdlet da biste stvorili virtualnog računala.

    New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine

Stvara se virtualnog računala. Imajte na umu da u standardni prostora za pohranu stvaranja računa za pokretanje dijagnostike jer je račun navedeni prostor za pohranu za disk virtualnog računala s računom za pohranu premium.

Sada možete pogledati ovo računalo na portalu Azure da biste vidjeli [javne IP adrese i njegov potpuno kvalificirani naziv domene](virtual-machines-windows-portal-sql-server-provision.md#Connect).  

## <a name="example-script"></a>Primjer skripte

Sljedeću skriptu sadrži potpuni skriptu PowerShell za ovog praktičnog vodiča. Pretpostavlja se da imate li već postavljanje Azure pretplata će se koristiti za naredbe **Dodaj AzureRmAccount** i **Odaberite AzureRmSubscription** .


    # Variables
    ## Global
    $Location = "SouthCentralUS"
    $ResourceGroupName = "sqlvm1"
    ## Storage
    $StorageName = $ResourceGroupName + "storage"
    $StorageSku = "Premium_LRS"

    ## Network
    $InterfaceName = $ResourceGroupName + "ServerInterface"
    $VNetName = $ResourceGroupName + "VNet"
    $SubnetName = "Default"
    $VNetAddressPrefix = "10.0.0.0/16"
    $VNetSubnetAddressPrefix = "10.0.0.0/24"
    $TCPIPAllocationMethod = "Dynamic"
    $DomainName = "sqlvm1"

    ##Compute
    $VMName = $ResourceGroupName + "VM"
    $ComputerName = $ResourceGroupName + "Server"
    $VMSize = "Standard_DS13"
    $OSDiskName = $VMName + "OSDisk"

    ##Image
    $PublisherName = "MicrosoftSQLServer"
    $OfferName = "SQL2016-WS2016"
    $Sku = "Enterprise"
    $Version = "latest"

    # Resource Group
    New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

    # Storage
    $StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location

    # Network
    $SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix
    $VNet = New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig
    $PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName
    $Interface = New-AzureRmNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id

    # Compute
    $VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize
    $Credential = Get-Credential -Message "Type the name and password of the local administrator account."
    $VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate #-TimeZone = $TimeZone
    $VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id
    $OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"
    $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage

    # Image
    $VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version

    ## Create the VM in Azure
    New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine

## <a name="next-steps"></a>Daljnji koraci
Nakon stvaranja virtualnog računala, spremni ste za povezivanje s virtualnog računala pomoću RDP i postavljanje povezivanja. Dodatne informacije potražite u članku [povezivanje za SQL Server virtualni stroj na Azure (Voditelj resursa)](virtual-machines-windows-sql-connect.md).
