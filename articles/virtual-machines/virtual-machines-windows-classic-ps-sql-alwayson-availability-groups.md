<properties
    pageTitle="Konfiguriranje uvijek na grupe dostupnosti u Azure VM sa servisom PowerShell"
    description="Pomoću ovog praktičnog vodiča koristi resursa koje su stvorene pomoću model klasični implementacije i koristi PowerShell da biste stvorili uvijek na dostupnost granu u Azure."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/22/2016"
    ms.author="mikeray" />

# <a name="configure-always-on-availability-group-in-azure-vm-with-powershell"></a>Konfiguriranje uvijek na grupe dostupnosti u Azure VM sa servisom PowerShell

> [AZURE.SELECTOR]
- [Voditelj resursa: predloška](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)
- [Voditelj resursa: ručno](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)
- [Klasični: korisničko Sučelje](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md)
- [Klasični: PowerShell](virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md)

<br/>

> [AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Azure virtualnim strojevima (VMs) može pomoći za administratore baze podataka za implementaciju manji trošak visoke dostupnosti sustava SQL Server. Pomoću ovog praktičnog vodiča pokazuje kako implementirati granu dostupnost pomoću SQL Server uvijek na kraj do kraja unutar Azure okruženja. Na kraju vodič rješenje SQL Server uvijek na servisu Azure će se sastojati od sljedećih elemenata:

- Virtualne mreže koja sadrži više podmreže, uključujući korisničko sučelje i pozadinske podmreže

- Kontroler domene domenu Active Directory (AD)

- Dva SQL Server VMs implementiran na podmreži pozadinsku i pridruženo domeni AD

- 3 čvor WSFC klaster s modelom kvorum Većina čvor

- Dostupnost grupe sustava s dvije replike sinkrono potvrdi dostupnosti baze podataka

Ovaj scenarij je odabran za njegov jednostavnosti, ne i za njegov trošak učinkovitosti ili Ostali čimbenici na Azure. Ako, na primjer, možete smanjiti broj VMs dostupnost dvije replike grupe da biste spremili na računalnim sate u Azure pomoću kontrolerom domene kao zrcaljenja zajedničko korištenje datoteka na kvorum klasteru WSFC 2 čvor. Ta metoda smanjuje broj VM jednu od gore konfiguracije.

Pomoću ovog praktičnog vodiča namijenjen je da bi se prikazala korake potrebne da biste postavili opisana rješenje iznad bez elaborating detalja svakog koraka. Dakle, umjesto prikazivanja navedeni koraci za konfiguraciju GUI, koristit će PowerShell skriptiranje da biste brzo vodi kroz svaki korak. Podrazumijeva sljedeće:

- Već imate račun za Azure s pretplatom virtualnog računala.

- Instalirali ste [cmdleta ljuske PowerShell Azure](../powershell-install-configure.md).

- Već imate pune razumijevanja uvijek na dostupnost grupe na lokaciji rješenja. Dodatne informacije potražite u članku [Uvijek na grupe dostupnosti (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx).

## <a name="connect-to-your-azure-subscription-and-create-the-virtual-network"></a>Povezivanje s pretplate Azure i stvaranje virtualne mreže

1. U prozoru PowerShell na lokalnom računalu, Uvoz modul Azure, preuzeti objavljivanje postavke datoteku na računalu te je povežite sesiju ljuske PowerShell u pretplatu za Azure uvozom preuzete postavke objavljivanja.

        Import-Module "C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\Azure\Azure.psd1"
        Get-AzurePublishSettingsFile
        Import-AzurePublishSettingsFile <publishsettingsfilepath>

    Naredba **Get-AzurePublishgSettingsFile** automatski generira na upravljanje certifikat Azure preuzimanja na vaše računalo. U pregledniku otvorit će se automatski, a zatim se od vas zatraži da unesete vjerodajnice Microsoftova računa za vašu pretplatu Azure. Datoteka preuzeta **.publishsettings** sadrži sve potrebne informacije da biste upravljali pretplatom Azure. Kada spremite datoteku na lokalni direktorij, uvezite ga pomoću naredbe **Uvoz AzurePublishSettingsFile** .

    >[AZURE.NOTE] Datoteka publishsettings sadrži vjerodajnice (dekodirani) koji se koriste za administriranje Azure pretplate i usluge. Sigurnost preporučenim načinom rada za tu datoteku je privremeno spremiti izvan vaše direktorija izvora (primjerice u mapi Libraries\Documents), a zatim ga izbrišite u kad je uvoz dovršen. Zlonamjerni gaining pristupa publishsettings datoteku možete uređivati, stvaranje i brisanje servisa Azure.

1. Definiranje niz varijabli koje ćete koristiti za stvaranje oblaka IT infrastrukturu.

        $location = "West US"
        $affinityGroupName = "ContosoAG"
        $affinityGroupDescription = "Contoso SQL HADR Affinity Group"
        $affinityGroupLabel = "IaaS BI Affinity Group"
        $networkConfigPath = "C:\scripts\Network.netcfg"
        $virtualNetworkName = "ContosoNET"
        $storageAccountName = "<uniquestorageaccountname>"
        $storageAccountLabel = "Contoso SQL HADR Storage Account"
        $storageAccountContainer = "https://" + $storageAccountName + ".blob.core.windows.net/vhds/"
        $winImageName = (Get-AzureVMImage | where {$_.Label -like "Windows Server 2008 R2 SP1*"} | sort PublishedDate -Descending)[0].ImageName
        $sqlImageName = (Get-AzureVMImage | where {$_.Label -like "SQL Server 2012 SP1 Enterprise*"} | sort PublishedDate -Descending)[0].ImageName
        $dcServerName = "ContosoDC"
        $dcServiceName = "<uniqueservicename>"
        $availabilitySetName = "SQLHADR"
        $vmAdminUser = "AzureAdmin"
        $vmAdminPassword = "Contoso!000"
        $workingDir = "c:\scripts\"

    Obratite pozornost na sljedeće da biste bili sigurni da će se naredbe kasnije uspjeti:

    - Varijable **$storageAccountName** i **$dcServiceName** mora biti jedinstvena zato što se koriste za prepoznavanje oblaka za pohranu računa i oblaka poslužitelj, odnosno na Internetu.

    - Nazivi navedeni varijable **$affinityGroupName** i **$virtualNetworkName** konfigurirane u dokumentu Konfiguracija virtualne mreže koju ćete koristiti kasnije.

    - **$sqlImageName** određuje naziv ažurirane VM slike koji sadrži SQL Server 2012 servisa Service Pack 1 Enterprise Edition.

    - Zbog jednostavnosti, **Contoso! 000** je koristiti u cijeloj cijelu vodič istu lozinku.

1. Stvaranje grupe sustava afinitet.

        New-AzureAffinityGroup `
            -Name $affinityGroupName `
            -Location $location `
            -Description $affinityGroupDescription `
            -Label $affinityGroupLabel

1. Stvaranje virtualne mreže tako da uvezete datoteku konfiguracije.

        Set-AzureVNetConfig `
            -ConfigurationPath $networkConfigPath

    Konfiguracijska datoteka sadrži sljedeće XML dokument. U brief određuje virtualne mreže naziva **ContosoNET** u grupi afinitet naziva **ContosoAG**, a sadrži prostor adresu **10.10.0.0/16** i da bi ima dvije podmreže, **10.10.1.0/24** i **10.10.2.0/24**koji su prednju podmreži te ponovno podmreže, odnosno. Prednja podmreže je gdje možete smjestiti klijentskim aplikacijama kao što je Microsoft SharePoint, a natrag podmreže gdje ćete smjestiti VMs za SQL Server. Ako promijenite varijable **$affinityGroupName** i **$virtualNetworkName** ranije, morate promijeniti i odgovarajući Nazivi ispod.

        <NetworkConfiguration xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
          <VirtualNetworkConfiguration>
            <Dns />
            <VirtualNetworkSites>
              <VirtualNetworkSite name="ContosoNET" AffinityGroup="ContosoAG">
                <AddressSpace>
                  <AddressPrefix>10.10.0.0/16</AddressPrefix>
                </AddressSpace>
                <Subnets>
                  <Subnet name="Front">
                    <AddressPrefix>10.10.1.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="Back">
                    <AddressPrefix>10.10.2.0/24</AddressPrefix>
                  </Subnet>
                </Subnets>
              </VirtualNetworkSite>
            </VirtualNetworkSites>
          </VirtualNetworkConfiguration>
        </NetworkConfiguration>

1. Stvorite račun za pohranu koji je povezan s grupom afinitet stvorili i postavite ga kao trenutni račun za pohranu za pretplatu.

        New-AzureStorageAccount `
            -StorageAccountName $storageAccountName `
            -Label $storageAccountLabel `
            -AffinityGroup $affinityGroupName
        Set-AzureSubscription `
            -SubscriptionName (Get-AzureSubscription).SubscriptionName `
            -CurrentStorageAccount $storageAccountName

1. Stvorite poslužitelj Kontroler u novog oblaka servisa i dostupnosti skupa.

        New-AzureVMConfig `
            -Name $dcServerName `
            -InstanceSize Medium `
            -ImageName $winImageName `
            -MediaLocation "$storageAccountContainer$dcServerName.vhd" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -Windows `
                -DisableAutomaticUpdates `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword |
                New-AzureVM `
                    -ServiceName $dcServiceName `
                    –AffinityGroup $affinityGroupName `
                    -VNetName $virtualNetworkName

    Ovaj niz piped naredbe učinite sljedeće:

    - **Novo AzureVMConfig** stvara VM konfiguracije.

    - **Dodavanje AzureProvisioningConfig** daje parametre konfiguracije samostalne Windows server.

    - **Dodavanje AzureDataDisk** dodaje podatkovni disk koji će se koristiti za pohranu podataka servisa Active Directory, s predmemoriranje mogućnost postavljen na ništa.

    - **Novo AzureVM** stvara novi servis u oblaku i stvara novi VM Azure novi servis u oblaku.

1. Pričekajte novi VM da biste potpuno dodjeli i preuzeli udaljene radne površine datoteke da biste radni imenik. Budući da novi VM Azure dugo za dodjelu resursa, Ponavljaj dok nastavljaju se za novi VM sve dok ne bude spreman za korištenje.

        $VMStatus = Get-AzureVM -ServiceName $dcServiceName -Name $dcServerName

        While ($VMStatus.InstanceStatus -ne "ReadyRole")
        {
            write-host "Waiting for " $VMStatus.Name "... Current Status = " $VMStatus.InstanceStatus
            Start-Sleep -Seconds 15
            $VMStatus = Get-AzureVM -ServiceName $dcServiceName -Name $dcServerName
        }

        Get-AzureRemoteDesktopFile `
            -ServiceName $dcServiceName `
            -Name $dcServerName `
            -LocalPath "$workingDir$dcServerName.rdp"

Poslužitelj za Kontroler sada uspješno dodijeljeni resursi. Nakon toga će konfigurirati domene servisa Active Directory na ovaj poslužitelj Kontroler. Ostavite prozoru PowerShell otvoren na lokalnom računalu. Će vam trebati kasnije ponovno stvoriti na dva VMs SQL Server.

## <a name="configure-the-domain-controller"></a>Konfiguriranje kontrolerom domene

1. Se povezati s poslužiteljem Kontroler pokretanja udaljene radne površine datoteke. Koristite AzureAdmin za korisničko ime i lozinku administrator računala **Contoso! 000**, koje ste naveli prilikom stvaranja novog VM.

1. Otvorite prozor PowerShell u načinu rada za administratora.

1. Pokrenite sljedeće **DCPROMO. EXE** naredba postavljanje **corp.contoso.com** domene u sustavu direktorija podataka na disku M.

        dcpromo.exe `
            /unattend `
            /ReplicaOrNewDomain:Domain `
            /NewDomain:Forest `
            /NewDomainDNSName:corp.contoso.com `
            /ForestLevel:4 `
            /DomainNetbiosName:CORP `
            /DomainLevel:4 `
            /InstallDNS:Yes `
            /ConfirmGc:Yes `
            /CreateDNSDelegation:No `
            /DatabasePath:"C:\Windows\NTDS" `
            /LogPath:"C:\Windows\NTDS" `
            /SYSVOLPath:"C:\Windows\SYSVOL" `
            /SafeModeAdminPassword:"Contoso!000"

    Kada se dovrši naredbu, automatski se pokreće na VM.

1. Se povezati s poslužiteljem Kontroler ponovno pokretanje udaljene radne površine datoteke. Ovaj put prijavite se kao **CORP\Administrator**.

1. Otvorite prozor PowerShell u načinu administrator i uvesti modul ljuske PowerShell za Active Directory pomoću sljedeće naredbe:

        Import-Module ActiveDirectory

1. Pokrenite sljedeće naredbe da biste dodali tri korisnika na domenu.

        $pwd = ConvertTo-SecureString "Contoso!000" -AsPlainText -Force
        New-ADUser `
            -Name 'Install' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true
        New-ADUser `
            -Name 'SQLSvc1' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true
        New-ADUser `
            -Name 'SQLSvc2' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true

    **CORP\Install** koristi se za konfiguraciju sve vezano uz instanci servisa SQL Server, klaster WSFC i grupe dostupnosti. **CORP\SQLSvc1** i **CORP\SQLSvc2** koriste kao računa servisa SQL Server za dva VMs poslužitelja SQL.

1. Nakon toga pokrenite sljedeće naredbe da bi se dobilo **CORP\Install** dozvole za stvaranje objekata računalo u domeni.

        Cd ad:
        $sid = new-object System.Security.Principal.SecurityIdentifier (Get-ADUser "Install").SID
        $guid = new-object Guid bf967a86-0de6-11d0-a285-00aa003049e2
        $ace1 = new-object System.DirectoryServices.ActiveDirectoryAccessRule $sid,"CreateChild","Allow",$guid,"All"
        $corp = Get-ADObject -Identity "DC=corp,DC=contoso,DC=com"
        $acl = Get-Acl $corp
        $acl.AddAccessRule($ace1)
        Set-Acl -Path "DC=corp,DC=contoso,DC=com" -AclObject $acl

    GUID gore navedene je GUID za vrstu objekta računala. Račun za **CORP\Install** mora **Sva svojstva za čitanje** i **Stvaranje računalne objekte** dozvolu za stvaranje aktivnog Izravni objekata radi WSFC klaster. Dozvole za **Čitanje svih svojstava** već daje CORP\Install prema zadanim postavkama, pa ne morate dodijeliti ga izričito. Dodatne informacije o dozvolama koje su potrebne za stvaranje klaster WSFC potražite u članku [Step-by-Step vodiča za prebacivanje klaster: Konfiguriranje računa servisa Active Directory](https://technet.microsoft.com/library/cc731002%28v=WS.10%29.aspx).

    Sad kad ste gotovi s Konfiguriranje servisa Active Directory i objektima korisnika, će stvoriti dvije VMs SQL Server i pridružiti ovu domenu.

## <a name="create-the-sql-server-vms"></a>Stvaranje VMs za SQL Server

1. Nastavite koristiti prozor PowerShell koja je otvorena na lokalnom računalu. Definiranje sljedeće dodatne varijable:

        $domainName= "corp"
        $FQDN = "corp.contoso.com"
        $subnetName = "Back"
        $sqlServiceName = "<uniqueservicename>"
        $quorumServerName = "ContosoQuorum"
        $sql1ServerName = "ContosoSQL1"
        $sql2ServerName = "ContosoSQL2"
        $availabilitySetName = "SQLHADR"
        $dataDiskSize = 100
        $dnsSettings = New-AzureDns -Name "ContosoBackDNS" -IPAddress "10.10.0.4"

    Adresa IP **10.10.0.4** obično se dodjeljuje prvi VM koje stvorite u **10.10.0.0/16** podmreži Azure virtualne mreže. Trebali biste provjerite to je adresa poslužitelja Kontroler tako da pokrenete **IPCONFIG**.

1. Pokrenite sljedeće naredbe piped da biste stvorili prvu VM klasteru WSFC, pod nazivom **ContosoQuorum**:

        New-AzureVMConfig `
            -Name $quorumServerName `
            -InstanceSize Medium `
            -ImageName $winImageName `
            -MediaLocation "$storageAccountContainer$quorumServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    New-AzureVM `
                        -ServiceName $sqlServiceName `
                        –AffinityGroup $affinityGroupName `
                        -VNetName $virtualNetworkName `
                        -DnsSettings $dnsSettings

    Imajte na umu sljedeće vezane uz gornje naredbe:

    - **Novo AzureVMConfig** stvara VM konfiguracije s naziv željene dostupnosti skupa. Sljedeći VMs stvorit će se s istim nazivom postavljanje dostupnosti tako da su se uključili u isti skup dostupnost.

    - **Dodavanje AzureProvisioningConfig** pridružuje u VM domene servisa Active Directory koji ste stvorili.

    - **Postavljanje AzureSubnet** na VM potvrđuje podmreže natrag.

    - **Novo AzureVM** stvara novi servis u oblaku i stvara novi VM Azure novi servis u oblaku. Parametar **DnsSettings** određuje ima li DNS poslužitelj za poslužitelje u novi servis u oblaku na IP adresa **10.10.0.4**, što je IP adresa poslužitelja Kontroler. Ovaj parametar potrebno je omogućiti nove VMs u servis u oblaku da biste se pridružili domene servisa Active Directory uspješno. Bez taj parametar, morate ručno postavljanje postavki IPv4 vaše VM da biste koristili poslužitelja Kontroler kao primarni DNS poslužitelj kada je na VM dodjeli i pridružite se VM domene servisa Active Directory.

1. Pokrenite sljedeće naredbe piped da biste stvorili VMs SQL Server koji je pod nazivom **ContosoSQL1** i **ContosoSQL2**.

        # Create ContosoSQL1...
        New-AzureVMConfig `
            -Name $sql1ServerName `
            -InstanceSize Large `
            -ImageName $sqlImageName `
            -MediaLocation "$storageAccountContainer$sql1ServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -HostCaching "ReadOnly" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    Add-AzureEndpoint `
                        -Name "SQL" `
                        -Protocol "tcp" `
                        -PublicPort 1 `
                        -LocalPort 1433 |
                        New-AzureVM `
                            -ServiceName $sqlServiceName

        # Create ContosoSQL2...
        New-AzureVMConfig `
            -Name $sql2ServerName `
            -InstanceSize Large `
            -ImageName $sqlImageName `
            -MediaLocation "$storageAccountContainer$sql2ServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -HostCaching "ReadOnly" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    Add-AzureEndpoint `
                        -Name "SQL" `
                        -Protocol "tcp" `
                        -PublicPort 2 `
                        -LocalPort 1433 |
                        New-AzureVM `
                            -ServiceName $sqlServiceName

    Imajte na umu sljedeće vezane uz gore navedene naredbe:

    - **Novo AzureVMConfig** koristi isti naziv skupa dostupnost kao Kontroler poslužitelj i koristi SQL Server 2012 servisa Service Pack 1 Enterprise Edition sliku u galeriji virtualnog računala. Njome se postavlja disk operacijski sustav za čitanje – predmemoriranje samo (bez zapisivanju). Preporučuje se migriranje datoteke baze podataka u zasebnom podatkovni disk da priložiti u VM i konfiguriranje uz bez čitanje ili pisanje predmemoriranje. Sljedeće najbolje je da biste uklonili zapisivanju na disku operacijski sustav, jer ne možete ukloniti čitati predmemoriranje na disku operacijski sustav.

    - **Dodavanje AzureProvisioningConfig** pridružuje u VM domene servisa Active Directory koji ste stvorili.

    - **Postavljanje AzureSubnet** na VM potvrđuje podmreže natrag.

    - **Dodavanje AzureEndpoint** dodaje krajnje točke access tako da se klijentske aplikacije mogu pristupiti ove instance servisa SQL Server na Internetu. Različite priključke ponudit će ContosoSQL1 i ContosoSQL2.

    - **Novo AzureVM** stvara novi VM poslužitelja SQL u istom servis u oblaku kao ContosoQuorum. Ako želite da budu u istom postavljanje dostupnosti morate postaviti na VMs u istom servis u oblaku.

1. Pričekajte svaki VM da biste potpuno dodjeli i preuzeli udaljene radne površine datoteke da biste radni imenik. U petlji provjeravaju se tri nova VMs i izvršava naredbe unutar najviše razine vitičasta zagrada za svaku od njih.

        Foreach ($VM in $VMs = Get-AzureVM -ServiceName $sqlServiceName)
        {
            write-host "Waiting for " $VM.Name "..."

            # Loop until the VM status is "ReadyRole"
            While ($VM.InstanceStatus -ne "ReadyRole")
            {
                write-host "  Current Status = " $VM.InstanceStatus
                Start-Sleep -Seconds 15
                $VM = Get-AzureVM -ServiceName $VM.ServiceName -Name $VM.InstanceName
            }

            write-host "  Current Status = " $VM.InstanceStatus

            # Download remote desktop file
            Get-AzureRemoteDesktopFile -ServiceName $VM.ServiceName -Name $VM.InstanceName -LocalPath "$workingDir$($VM.InstanceName).rdp"
        }

    VMs za SQL Server su sada dodjeli i pokretanje, no oni su instalirani sa sustavom SQL Server s zadane mogućnosti.

## <a name="initialize-the-wsfc-cluster-vms"></a>Pokretanje klaster VMs WSFC

U ovom odjeljku ćete morati izmijeniti tri poslužitelje koje ćete koristiti u klaster WSFC i instalacija sustava SQL Server. Konkretno:

- (Poslužiteljima) Morate instalirati značajku **Klasteriranja** .

- (Poslužiteljima) Morate dodati **CORP\Install** kao **administrator**za računala.

- (ContosoSQL1 i ContosoSQL2 samo) Morate dodati **CORP\Install** kao uloga **sustava** u zadanu bazu podataka.

- (ContosoSQL1 i ContosoSQL2 samo) Morate dodati **NT AUTHORITY\System** kao prijava s dozvolama za sljedeće:

    - Promijeniti bilo koje grupe dostupnosti

    - Povezivanje SQL

    - Stanje poslužitelja za prikaz

- (ContosoSQL1 i ContosoSQL2 samo) **TCP** protokol već omogućeno na VM za SQL Server. Međutim, i dalje morate otvoriti Vatrozid za daljinski pristup sustava SQL Server.

Sada ste spremni za početak. Počevši od **ContosoQuorum**, slijedite ove korake:

1. Povežite se s **ContosoQuorum** tako da pokrenete datoteke udaljene radne površine. Koristite **AzureAdmin** za korisničko ime i lozinku administrator računala **Contoso! 000**, koje ste naveli prilikom stvaranja na VMs.

1. Provjerite da računalima imati je uspješno pridruženo **corp.contoso.com**.

1. Pričekajte da se instalacija sustava SQL Server da biste dovršili automatsko pokretanje zadataka prije nastavka koji se izvode.

1. Otvorite prozor PowerShell u načinu rada za administratora.

1. Instalirajte značajku Windows klasteriranja.

        Import-Module ServerManager
        Add-WindowsFeature Failover-Clustering

1. Dodajte **CORP\Install** lokalnog administratorskog računa.

        net localgroup administrators "CORP\Install" /Add

1. Odjavite se iz aplikacije ContosoQuorum. Dovršite ovaj poslužitelj sada.

        logoff.exe

Nakon toga pokrenuti **ContosoSQL1** i **ContosoSQL2**. Slijedite korake u nastavku, koji su jednaki za oba VMs za SQL Server.

1. Povezivanje dva VMs poslužitelja SQL tako da pokrenete datoteke udaljene radne površine. Korištenje **AzureAdmin** za korisničko ime i lozinku administratora stroj **Contoso! 000**, koje ste naveli prilikom stvaranja na VMs.

1. Provjerite da računalima imati je uspješno pridruženo **corp.contoso.com**.

1. Pričekajte da se instalacija sustava SQL Server da biste dovršili automatsko pokretanje zadataka prije nastavka koji se izvode.

1. Otvorite prozor PowerShell u načinu rada za administratora.

1. Instalirajte značajku Windows klasteriranja.

        Import-Module ServerManager
        Add-WindowsFeature Failover-Clustering

1. Dodavanje **CORP\Install** kao lokalni administrator

        net localgroup administrators "CORP\Install" /Add

1. Uvoz PowerShell davatelj usluga SQL Server.

        Set-ExecutionPolicy -Execution RemoteSigned -Force
        Import-Module -Name "sqlps" -DisableNameChecking

1. Dodajte **CORP\Install** kao s ulogom zadane instance sustava SQL Server.

        net localgroup administrators "CORP\Install" /Add
        Invoke-SqlCmd -Query "EXEC sp_addsrvrolemember 'CORP\Install', 'sysadmin'" -ServerInstance "."

1. Dodavanje **NT AUTHORITY\System** kao prijava s tri prethodno opisan dozvole.

        Invoke-SqlCmd -Query "CREATE LOGIN [NT AUTHORITY\SYSTEM] FROM WINDOWS" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT ALTER ANY AVAILABILITY GROUP TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT CONNECT SQL TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT VIEW SERVER STATE TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."

1. Otvorite Vatrozid za daljinski pristup sustava SQL Server.

        netsh advfirewall firewall add rule name='SQL Server (TCP-In)' program='C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Binn\sqlservr.exe' dir=in action=allow protocol=TCP

1. Odjavite se iz aplikacije i VMs.

        logoff.exe

Na kraju, spremni ste za konfiguriranje grupe dostupnosti. Davatelj usluga PowerShell SQL Server će se koristiti za izvođenje svih poslova na **ContosoSQL1**.

## <a name="configure-the-availability-group"></a>Konfiguriranje grupe dostupnosti

1. Se povezati s **ContosoSQL1** ponovno pokretanje datoteke udaljene radne površine. Umjesto prijaviti pomoću računa za računala, prijavite se pomoću **CORP\Install**.

1. Otvorite prozor PowerShell u načinu rada za administratora.

1. Definiranje sljedeće varijable:

        $server1 = "ContosoSQL1"
        $server2 = "ContosoSQL2"
        $serverQuorum = "ContosoQuorum"
        $acct1 = "CORP\SQLSvc1"
        $acct2 = "CORP\SQLSvc2"
        $password = "Contoso!000"
        $clusterName = "Cluster1"
        $timeout = New-Object System.TimeSpan -ArgumentList 0, 0, 30
        $db = "MyDB1"
        $backupShare = "\\$server1\backup"
        $quorumShare = "\\$server1\quorum"
        $ag = "AG1"

1. Uvoz PowerShell davatelj usluga za SQL Server.

        Set-ExecutionPolicy RemoteSigned -Force
        Import-Module "sqlps" -DisableNameChecking

1. Promjena računa servisa SQL Server za ContosoSQL1 za CORP\SQLSvc1.

        $wmi1 = new-object ("Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer") $server1
        $wmi1.services | where {$_.Type -eq 'SqlServer'} | foreach{$_.SetServiceAccount($acct1,$password)}
        $svc1 = Get-Service -ComputerName $server1 -Name 'MSSQLSERVER'
        $svc1.Stop()
        $svc1.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc1.Start();
        $svc1.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)

1. Promjena računa servisa SQL Server za ContosoSQL2 za CORP\SQLSvc2.

        $wmi2 = new-object ("Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer") $server2
        $wmi2.services | where {$_.Type -eq 'SqlServer'} | foreach{$_.SetServiceAccount($acct2,$password)}
        $svc2 = Get-Service -ComputerName $server2 -Name 'MSSQLSERVER'
        $svc2.Stop()
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc2.Start();
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)

1. Preuzmite **CreateAzureFailoverCluster.ps1** iz [Stvaranje WSFC klaster za uvijek na dostupnost grupe u Azure VM](http://gallery.technet.microsoft.com/scriptcenter/Create-WSFC-Cluster-for-7c207d3a) lokalnom direktoriju rad. Ova skripta će se koristiti za stvaranje funkcionalne WSFC klaster. Važne informacije o interakciju WSFC s Azure mreže, potražite u članku [visoke dostupnosti i oporavak Izrada za SQL Server na virtualnim strojevima sa sustavom Azure](virtual-machines-windows-sql-high-availability-dr.md).

1. Promjena radni direktorij i stvorite klaster WSFC sa preuzete skripte.

        Set-ExecutionPolicy Unrestricted -Force
        .\CreateAzureFailoverCluster.ps1 -ClusterName "$clusterName" -ClusterNode "$server1","$server2","$serverQuorum"

1. Omogućite uvijek na grupe dostupnosti zadane instance sustava SQL Server na **ContosoSQL1** i **ContosoSQL2**.

        Enable-SqlAlwaysOn `
            -Path SQLSERVER:\SQL\$server1\Default `
            -Force
        Enable-SqlAlwaysOn `
            -Path SQLSERVER:\SQL\$server2\Default `
            -NoServiceRestart
        $svc2.Stop()
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc2.Start();
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)

1. Stvaranje sigurnosne kopije direktorija i dajte dozvole za računa servisa SQL Server. Da biste pripremili dostupnost baza podataka na sekundarnog replike će koristiti taj imenik.

        $backup = "C:\backup"
        New-Item $backup -ItemType directory
        net share backup=$backup "/grant:$acct1,FULL" "/grant:$acct2,FULL"
        icacls.exe "$backup" /grant:r ("$acct1" + ":(OI)(CI)F") ("$acct2" + ":(OI)(CI)F")

1. Stvaranje baze podataka na **ContosoSQL1** naziva **MyDB1**, potpuno sigurnosno kopiranje i zapisnika sigurnosnu kopiju i njihova vraćanja **ContosoSQL2** s mogućnošću **S NORECOVERY** .

        Invoke-SqlCmd -Query "CREATE database $db"
        Backup-SqlDatabase -Database $db -BackupFile "$backupShare\db.bak" -ServerInstance $server1
        Backup-SqlDatabase -Database $db -BackupFile "$backupShare\db.log" -ServerInstance $server1 -BackupAction Log
        Restore-SqlDatabase -Database $db -BackupFile "$backupShare\db.bak" -ServerInstance $server2 -NoRecovery
        Restore-SqlDatabase -Database $db -BackupFile "$backupShare\db.log" -ServerInstance $server2 -RestoreAction Log -NoRecovery

1. Stvorite dostupnost krajnje točke grupe na VMs za SQL Server i postavite odgovarajuće dozvole na krajnjih točaka.

        $endpoint =
            New-SqlHadrEndpoint MyMirroringEndpoint `
            -Port 5022 `
            -Path "SQLSERVER:\SQL\$server1\Default"
        Set-SqlHadrEndpoint `
            -InputObject $endpoint `
            -State "Started"
        $endpoint =
            New-SqlHadrEndpoint MyMirroringEndpoint `
            -Port 5022 `
            -Path "SQLSERVER:\SQL\$server2\Default"
        Set-SqlHadrEndpoint `
            -InputObject $endpoint `
            -State "Started"

        Invoke-SqlCmd -Query "CREATE LOGIN [$acct2] FROM WINDOWS" -ServerInstance $server1
        Invoke-SqlCmd -Query "GRANT CONNECT ON ENDPOINT::[MyMirroringEndpoint] TO [$acct2]" -ServerInstance $server1
        Invoke-SqlCmd -Query "CREATE LOGIN [$acct1] FROM WINDOWS" -ServerInstance $server2
        Invoke-SqlCmd -Query "GRANT CONNECT ON ENDPOINT::[MyMirroringEndpoint] TO [$acct1]" -ServerInstance $server2

1. Stvaranje replike dostupnost.

        $primaryReplica =
            New-SqlAvailabilityReplica `
            -Name $server1 `
            -EndpointURL "TCP://$server1.corp.contoso.com:5022" `
            -AvailabilityMode "SynchronousCommit" `
            -FailoverMode "Automatic" `
            -Version 11 `
            -AsTemplate
        $secondaryReplica =
            New-SqlAvailabilityReplica `
            -Name $server2 `
            -EndpointURL "TCP://$server2.corp.contoso.com:5022" `
            -AvailabilityMode "SynchronousCommit" `
            -FailoverMode "Automatic" `
            -Version 11 `
            -AsTemplate

1. Na kraju, stvorite grupe dostupnosti i pridruživanje sekundarnog replike grupe dostupnosti.

        New-SqlAvailabilityGroup `
            -Name $ag `
            -Path "SQLSERVER:\SQL\$server1\Default" `
            -AvailabilityReplica @($primaryReplica,$secondaryReplica) `
            -Database $db
        Join-SqlAvailabilityGroup `
            -Path "SQLSERVER:\SQL\$server2\Default" `
            -Name $ag
        Add-SqlAvailabilityDatabase `
            -Path "SQLSERVER:\SQL\$server2\Default\AvailabilityGroups\$ag" `
            -Database $db

## <a name="next-steps"></a>Daljnji koraci
Koje su sada uspješno implementirana SQL Server uvijek na stvaranjem granu dostupnost u Azure. Da biste konfigurirali ga slušatelj za ovu grupu dostupnost, potražite u članku [Konfiguriranje programa ILB ga slušatelj za uvijek na dostupnost grupe u Azure](virtual-machines-windows-classic-ps-sql-int-listener.md).

Druge informacije o korištenju sustava SQL Server u Azure potražite u članku [SQL Server na virtualnim strojevima sa sustavom Azure](virtual-machines-windows-sql-server-iaas-overview.md).
