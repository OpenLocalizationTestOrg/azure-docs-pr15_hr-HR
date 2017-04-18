<properties 
    pageTitle="Da biste stvorili na VM s poslužiteljem za izvješća u Nativnom načinu rada pomoću komponente PowerShell | Microsoft Azure"
    description="U ovoj se temi opisuju i vodi vas kroz implementaciji i konfiguraciji SQL Server Reporting Services nativnom načinu rada poslužitelj za izvješća u programa Azure virtualnog računala. "
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="guyinacube"
    manager="erikre"
    editor="monicar" 
    tags="azure-service-management"/>
<tags 
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/04/2016"
    ms.author="asaxton" />

# <a name="use-powershell-to-create-an-azure-vm-with-a-native-mode-report-server"></a>Korištenje ljuske PowerShell za stvaranje Azure VM poslužitelju za izvješća u Nativnom načinu rada

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]
 

U ovoj se temi opisuju i vodi vas kroz implementaciji i konfiguraciji sustava SQL Server Reporting Services nativnom načinu rada poslužitelj za izvješća u programa Azure virtualnog računala. Koraci u ovom dokumentu koristite kombinaciju Ručni koraci za stvaranje virtualnog računala i skripte komponente Windows PowerShell da biste konfigurirali Reporting Services na VM. Skripte za konfiguraciju obuhvaća otvaranje priključka vatrozidu za HTTP ili HTTPs.

>[AZURE.NOTE] Ako ne zahtijeva **HTTPS** na poslužitelju izvješća, **preskočite korak 2**.
>
>Kada stvorite na VM u koraku 1, prijeđite na odjeljak koristi skriptu da biste konfigurirali poslužitelj za izvješća i HTTP-a. Nakon pokretanja skriptu poslužitelju za izvješća spremna je za korištenje.

## <a name="prerequisites-and-assumptions"></a>Preduvjeti i pretpostavke

- **Azure pretplatu**: Provjerite je li broj jezgri dostupna u vašoj pretplati Azure. Ako stvorite preporučena VM veličine **A3**, morat ćete dostupna jezgri **4** . Ako koristite veličina VM **a2**, morat ćete dostupna jezgri **2** .
    
    - Da biste provjerili ograničenje core pretplate na Azure klasični portal u gornji izbornik kliknite postavke u lijevom oknu pa zatim kliknite korištenje.
    
    - Da biste povećali kvote core, obratite se [Podršci za Azure](https://azure.microsoft.com/support/options/). Informacije o veličini VM, potražite u članku [Veličine virtualnog računala za Azure](virtual-machines-linux-sizes.md).

- **Skripte za Windows PowerShell**: temi podrazumijeva Osnovno znanje rad Windows PowerShell. Dodatne informacije o korištenju programa Windows PowerShell potražite u sljedećim člancima:

    - [Pokretanje komponente Windows PowerShell u sustavu Windows Server](https://technet.microsoft.com/library/hh847814.aspx)
    
    - [Početak rada s komponentom Windows PowerShell](https://technet.microsoft.com/library/hh857337.aspx)

## <a name="step-1-provision-an-azure-virtual-machine"></a>Korak 1: Dodjela Azure virtualnog računala

1. Otvorite Azure klasični portal.

1. U lijevom oknu kliknite **virtualnih računala** .

    ![virtualnim strojevima sa sustavom Microsoft azure](./media/virtual-machines-windows-classic-ps-sql-report/IC660124.gif)

1. Kliknite **Novo**.

    ![gumb novi](./media/virtual-machines-windows-classic-ps-sql-report/IC692019.gif)

1. Kliknite **iz galerije**.

    ![Novi vm iz galerije](./media/virtual-machines-windows-classic-ps-sql-report/IC692020.gif)

1. Kliknite **SQL Server 2014 RTM Standard – Windows Server 2012 R2** , a zatim kliknite strelicu da biste nastavili.

    ![Sljedeći](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)

    Ako vam je potrebna podataka komponente Reporting Services utemeljenima na značajku pretplate, odaberite **SQL Server 2014 RTM Enterprise – Windows Server 2012 R2**. Dodatne informacije o izdanja sustava SQL Server i podržanim značajkama potražite u članku [Značajke podržane u izdanja sustava SQL Server 2012](https://msdn.microsoft.com/library/cc645993.aspx#Reporting).

1. Na stranici **Konfiguracija virtualnog računala** , urediti sljedeća polja:
                                    
    - Ako postoji više **Datum IZDAVANJA VERZIJU**, odaberite najnoviju verziju.
    
    - **Naziv virtualnog računala**: naziv računala i koristiti na sljedećoj stranici Konfiguracija kao zadani naziv DNS servis oblaka. Naziv DNS-a mora biti jedinstvena putem servisa Azure. Razmislite o konfiguriranju na VM s računala naziv koji opisuje što u VM koristi. Na primjer ssrsnativecloud.
    
    - **Razina**: Standard
    
    - **Veličina: A3** je preporučena VM veličine radnih opterećenja sustava SQL Server. Ako je VM koristi samo kao poslužitelj za izvješća, veličina VM a2 je dovoljno, osim ako poslužitelj za izvješća iskustvo velike radno opterećenje. Informacije o cijenama VM, potražite u članku [Virtualnim strojevima cijene](https://azure.microsoft.com/pricing/details/virtual-machines/).
    
    - **Novo korisničko ime**: naziv unesete stvara se kao administrator sustava VM.
    
    - **Novu lozinku** i **potvrdite**. Ovu lozinku koristi se za novi račun administratora i preporučuje se korištenje jaku lozinku.
    
    - Kliknite **Dalje**. ![Sljedeći](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)

1. Na sljedećoj stranici urediti sljedeća polja:

    - **Servis u oblaku**: odaberite **Stvori novi servis u Oblaku**.
    
    - **Naziv DNS servis oblaka**: to je javno DNS naziva servisa u Oblaku koji je pridružen na VM. Zadani naziv je naziv koji ste upisali u VM naziv. Ako je u noviji koracima u sklopu teme, stvorite pouzdano SSL certifikat, a zatim će se naziv DNS koristiti za vrijednost na "**Izdano za**" certifikata.
    
    - **Regija/afinitet grupe/virtualne mreže**: Odaberite područje najbliže krajnjim korisnicima.
    
    - **Račun za pohranu**: pomoću računa za automatski generirani prostora za pohranu.
    
    - **Postavljanje dostupnosti**: ništa.
    
    - **Krajnje točke** Zadrži krajnje točke **Udaljene radne površine** i **programa PowerShell** , a zatim dodajte HTTP ili HTTPS krajnje točke, ovisno o okruženju.

        - **HTTP**: zadani javne i privatne priključci su **80**. Imajte na umu da ako koristite privatne priključak osim 80, izmijeniti **$HTTPport = 80** u skripti HTTP-a.

        - **HTTPS**: zadani javne i privatne priključci su **443**. Preporučenim načinom rada sigurnost je da biste promijenili privatne priključak i konfiguriranje vatrozida i poslužitelj za izvješća da biste koristili privatne priključak. Dodatne informacije o krajnje točke potražite [u](virtual-machines-windows-classic-setup-endpoints.md)članku postavljanje komunikacije s virtualnog računala. Imajte na umu da ako koristite priključak osim 443, promijenite taj parametar **$HTTPsport = 443** u skripti HTTPS.
    
    - Kliknite Dalje. ![Sljedeći](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)

1. Na zadnjoj stranici čarobnjaka zadržite zadani **instalirati VM agent** odabrana. Koracima u ovoj temi koristiti VM agent, ali ako planirate zadržati ovaj VM VM agent i proširenja će vam omogućiti da biste poboljšali on CM.  Dodatne informacije o VM agent potražite u članku [VM Agent i proširenja – 1 dijela](https://azure.microsoft.com/blog/2014/04/11/vm-agent-and-extensions-part-1/). Jedna od ad programski dodaci instalirani zadani pokrenut je proširenje "BGINFO" koji se prikazuje na radnoj površini VM, informacije o sustavu kao što su interni IP i slobodan prostor na pogonu.

1. Kliknite dovrši. ![ok](./media/virtual-machines-windows-classic-ps-sql-report/IC660122.gif)

1. **Status** u VM prikazuje kao **Početni (Provisioning)** tijekom postupka dodjele resursa i prikazuje kao da ste **pokrenuli** kada je na VM dodijeljenu i spreman za korištenje.

## <a name="step-2-create-a-server-certificate"></a>Korak 2: Stvaranje certifikat poslužitelja

>[AZURE.NOTE] Ako ne zahtijeva HTTPS na poslužitelju za izvješća, možete **preskočiti korak 2** i prijeđite na odjeljak **koristi skriptu da biste konfigurirali poslužitelj za izvješća i HTTP-a**. Koristite HTTP skriptu da biste brzo konfigurirali poslužitelj za izvješća i poslužitelj za izvješća bit će spremna za korištenje.

Da biste koristili HTTPS na na VM, potreban vam je pouzdanih SSL certifikata. Ovisno o scenariju, možete koristiti jedan od sljedeća dva načina:

- Valjani SSL certifikat Izdana od ustanove za izdavanje certifikata (CA) i pouzdana Microsoft. Ustanove za Izdavanje korijenskih certifikata potrebnih distribuirati putem Microsoftova programa korijenski certifikat. Dodatne informacije o programu potražite u članku [Windows i programu sustava Windows Phone 8 SSL korijenski certifikat (člana CA)](http://social.technet.microsoft.com/wiki/contents/articles/14215.windows-and-windows-phone-8-ssl-root-certificate-program-member-cas.aspx) i [Uvod u Microsoft korijenski certifikat programa](http://social.technet.microsoft.com/wiki/contents/articles/3281.introduction-to-the-microsoft-root-certificate-program.aspx).

- Samopotpisani certifikat. Samopotpisane potvrde ne preporučuje se za radnog okruženja.

### <a name="to-use-a-certificate-created-by-a-trusted-certificate-authority-ca"></a>Da biste koristili certifikat koji je stvorio od pouzdanih certifikata za izdavanje certifikata (CA)

1. **Zahtjev za potvrdom poslužitelja web-mjesta iz ustanova za izdavanje certifikata**. 

    Čarobnjak za certifikat poslužitelja Web možete koristiti da biste generirali zahtjev datoteke potvrda (Certreq.txt) koju ste poslali ustanova za izdavanje certifikata ili da biste generirali zahtjeva za mrežni izdavanje. Na primjer, Microsoft Certificate Services u sustavu Windows Server 2012. Ovisno o razini identifikacijski jamstvo nudi certifikat poslužitelja, je nekoliko dana do nekoliko mjeseca za izdavanje da biste odobrili zahtjev, a zatim Pošalji datoteka certifikata. 

    Dodatne informacije o traži poslužitelj certifikata, pogledajte sljedeće: 
    
    - Pomoću [Certreq](https://technet.microsoft.com/library/cc725793.aspx), [Certreq](https://technet.microsoft.com/library/cc725793.aspx).
    
    - Alati za sigurnost da biste saznali više o administraciji sustava Windows Server 2012.

    [Alati za sigurnost da biste saznali više o administraciji sustava Windows Server 2012](https://technet.microsoft.com/library/jj730960.aspx)

    >[AZURE.NOTE] Polje **izdala** pouzdana SSL certifikat mora biti jednak **Oblaka usluge DNS naziv** koji ste koristili za nove VM.

1. **Instalacija certifikat poslužitelja na web-poslužitelj**. Web-poslužitelj je u tom slučaju VM koji hostira poslužitelju za izvješća i web-mjesta stvorili u kasnije prilikom konfiguriranja servisa Reporting Services. Dodatne informacije o instaliranju certifikat poslužitelja na web-poslužitelju pomoću dodatka za BLOG certifikata potražite u članku [Instalacija certifikat poslužitelja](https://technet.microsoft.com/library/cc740068).
    
    Ako želite koristiti skripte uključen u ovoj temi za konfiguriranje poslužitelja za izvješća vrijednost certifikate **otisak prsta** potreban je kao parametar skripte. U sljedećem odjeljku pojedinosti o tome kako nabaviti otisak prsta potvrde.

1. Certifikat poslužitelja dodijeliti poslužitelju za izvješća. Dodjela dovršetka u sljedećem odjeljku prilikom konfiguriranja poslužitelju za izvješća.

### <a name="to-use-the-virtual-machines-self-signed-certificate"></a>Da biste koristili virtualnim strojevima samopotpisane potvrde

Samopotpisani certifikat stvorena na na VM kada se VM je dodijeljena. Certifikat ima isti naziv kao VM DNS naziv. Da biste izbjegli pogreške certifikata je potrebna potvrda je pouzdana na VM sam i svi korisnici web-mjesta.

1. Da biste pouzdanom korijenu CA certifikat na lokalnom VM, dodajte certifikat **Izdavanje korijenskih**. Slijedi sažetak koraci potrebni. Detaljne upute o pouzdanosti CA potražite u članku [Instalacija certifikat poslužitelja](https://technet.microsoft.com/library/cc740068).

    1. Azure klasični, na portalu odaberite na VM, a zatim kliknite Poveži. Ovisno o vašoj konfiguraciji preglednika može se zatražiti da biste spremili .rdp datoteku za povezivanje s VM.
    
        ![povezivanje s azure virtualnog računala](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) Korištenje korisničko ime VM, korisničkog imena i lozinke koje ste konfigurirali stvaranja na VM. 
    
        Na primjer, na sljedećoj slici naziv VM je **ssrsnativecloud** i korisničko ime je **testuser**.
        
        ![korisničko ime za prijavu inlcudes vm](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)
    
    1. Pokrenite mmc.exe. Dodatne informacije potražite u članku [način: prikaz certifikata s dodatkom BLOG](https://msdn.microsoft.com/library/ms788967.aspx).
    
    1. Na izborniku **datoteka** aplikacije konzole dodali dodatak za **potvrde** , odaberite **Račun na računalu** kada se to od vas zatraži i zatim kliknite **Dalje**.
    
    1. Odaberite **Lokalnog računala** za upravljanje, a zatim kliknite **Završi**.
    
    1. Kliknite **u redu** , a zatim proširite stavku na **Certifikati - osobne** čvorove, a zatim **Certifikati**. Certifikat pod nazivom nakon naziva DNS-a na VM, a završava **cloudapp.net**. Desnom tipkom miša kliknite certifikat, a zatim kliknite **Kopiraj**.
    
    1. Proširite čvor **Izdavanje korijenskih** i desnom tipkom miša kliknite **Certifikati** , a zatim kliknite **Zalijepi**.
    
    1. Da biste provjerili valjanost, dvaput kliknite naziv certifikata u odjeljku **Izdavanje korijenskih** i provjerite nema li pogrešaka i vidite naziv certifikata. Ako želite koristiti skripte HTTPS uključen u ovoj temi za konfiguriranje poslužitelja za izvješća vrijednost certifikate **otisak prsta** potreban je kao parametar skripte. **Da biste dohvatili vrijednost otisak prsta**, ispunite sljedeće. Postoji uzorak ljuske PowerShell za dohvaćanje otisak prsta u odjeljku [koristi skriptu da biste konfigurirali poslužitelj za izvješća i HTTPS](#use-script-to-configure-the-report-server-and-HTTPS).
        
        1. Dvokliknite naziv certifikata, primjerice ssrsnativecloud.cloudapp.net.
        
        1. Kliknite karticu **Detalji** .
        
        1. Kliknite **otisak prsta**. Vrijednost na otisak prsta prikazuje se u polje detalje o, na primjer a6 08 3c df f9 0b f7 e3 7c 25 redi a4 redi 7e ac 91 9c 2c fb 2f.
        
        1. Kopirajte na otisak prsta i spremanje vrijednosti za kasnije ili urediti skriptu sada.
        
        1. (*) Prije nego što pokrenete skriptu, uklonite razmake između parova vrijednosti. Na primjer otisak prsta zabilježili prije bit će a6083cdff90bf7e37c25eda4ed7eac919c2cfb2f.
        
        1. Certifikat poslužitelja dodijeliti poslužitelju za izvješća. Dodjela dovršetka u sljedećem odjeljku prilikom konfiguriranja poslužitelju za izvješća.

Ako koristite samopotpisani SSL certifikat, naziv na certifikatu već usklađuje glavnog računala na VM. Stoga DNS računalu već registriran globalno i možete pristupiti iz bilo kojeg klijenta.

## <a name="step-3-configure-the-report-server"></a>Korak 3: Konfiguriranje poslužitelju za izvješća

U ovom se odjeljku vodit će vas kroz konfiguriranje na VM kao poslužitelj za izvješća komponente Reporting Services nativnom načinu rada. Da biste konfigurirali poslužitelj za izvješća možete koristiti jednu od sljedećih načina:

- Koristite skriptu da biste konfigurirali poslužitelj za izvješća

- Pomoću upravitelja konfiguracije konfiguriranje poslužitelja za izvješća.

Detaljnije upute potražite u odjeljku [Povezivanje s virtualnog računala i pokrenite Upravitelj konfiguracije Reporting Services](virtual-machines-windows-classic-ps-sql-bi.md#connect-to-the-virtual-machine-and-start-the-reporting-services-configuration-manager).

**Bilješke provjere autentičnosti:** Provjera autentičnosti sustava Windows je način preporučena provjere autentičnosti te je zadana Reporting Services provjera autentičnosti. Samo korisnici konfigurirane u VM omogućuje pristup Reporting Services i dodijeljene ulogama Reporting Services.

### <a name="use-script-to-configure-the-report-server-and-http"></a>Pomoću sljedeće skripte za konfiguriranje poslužitelju za izvješća i HTTP

Da biste koristili skripte komponente Windows PowerShell za konfiguraciju poslužitelja izvješća, provedite sljedeće korake. Konfiguracija obuhvaća HTTP, a zatim nije HTTPS:

1. Azure klasični, na portalu odaberite na VM, a zatim kliknite Poveži. Ovisno o vašoj konfiguraciji preglednika može se zatražiti da biste spremili .rdp datoteku za povezivanje s VM.

    ![povezivanje s azure virtualnog računala](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) Korištenje korisničko ime VM, korisničkog imena i lozinke koje ste konfigurirali stvaranja na VM. 

    Na primjer, na sljedećoj slici naziv VM je **ssrsnativecloud** i korisničko ime je **testuser**.
    
    ![korisničko ime za prijavu inlcudes vm](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)

1. Na VM, otvorite **Windows Očisti filtar** s administratorskim ovlastima. Očisti filtar instaliran je prema zadanim postavkama u sustavu Windows server 2012. Preporučuje se koristiti u filtar umjesto standardne komponente Windows PowerShell prozoru tako da skriptu zalijepite u filtar, izmijenite skripte i pokrenuti skriptu.

1. U sustavu Windows Očisti filtar kliknite izbornik **Prikaz** , a zatim kliknite **Pokaži okno skripte**.

1. Kopirajte sljedeću skriptu i zalijepite skriptu u okno skripte za Windows Očisti filtar.

        ## This script configures a Native mode report server without HTTPS
        $ErrorActionPreference = "Stop"
        
        $server = $env:COMPUTERNAME
        $HTTPport = 80 # change the value if you used a different port for the private HTTP endpoint when the VM was created.
        
        ## Set PowerShell execution policy to be able to run scripts
        Set-ExecutionPolicy RemoteSigned -Force
        
        ## Utility method for verifying an operation's result
        function CheckResult
        {
            param($wmi_result, $actionname)
            if ($wmi_result.HRESULT -ne 0) {
                write-error "$actionname failed. Error from WMI: $($wmi_result.Error)"
            }
        }
        
        $starttime=Get-Date
        write-host -foregroundcolor DarkGray $starttime StartTime
        
        ## ReportServer Database name - this can be changed if needed
        $dbName='ReportServer'
        
        ## Register for MSReportServer_ConfigurationSetting
        ## Change the version portion of the path to "v11" to use the script for SQL Server 2012
        $RSObject = Get-WmiObject -class "MSReportServer_ConfigurationSetting" -namespace "root\Microsoft\SqlServer\ReportServer\RS_MSSQLSERVER\v12\Admin"
        
        ## Report Server Configuration Steps
        
        ## Setting the web service URL ##
        write-host -foregroundcolor green "Setting the web service URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## SetVirtualDirectory for ReportServer site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportServerWebService','ReportServer',1033)
            CheckResult $r "SetVirtualDirectory for ReportServer"
        
        ## ReserveURL for ReportServerWebService - port $HTTPport (for local usage)
            write-host "Calling ReserveURL port $HTTPport"
            $r = $RSObject.ReserveURL('ReportServerWebService',"http://+:$HTTPport",1033)
            CheckResult $r "ReserveURL for ReportServer port $HTTPport" 
           
        ## Setting the Database ##
        write-host -foregroundcolor green "Setting the Database"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## GenerateDatabaseScript - for creating the database
            write-host "Calling GenerateDatabaseCreationScript for database $dbName"
            $r = $RSObject.GenerateDatabaseCreationScript($dbName,1033,$false)
            CheckResult $r "GenerateDatabaseCreationScript"
            $script = $r.Script
        
        ## Execute sql script to create the database
            write-host 'Executing Database Creation Script'
            $savedcvd = Get-Location
            Import-Module SQLPS              ## this automatically changes to sqlserver provider
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
          
        ## GenerateGrantRightsScript 
            $DBUser = "NT Service\ReportServer"
            write-host "Calling GenerateDatabaseRightsScript with user $DBUser"
            $r = $RSObject.GenerateDatabaseRightsScript($DBUser,$dbName,$false,$true)
            CheckResult $r "GenerateDatabaseRightsScript"
            $script = $r.Script
        
        ## Execute grant rights script
            write-host 'Executing Database Rights Script'
            $savedcvd = Get-Location
            cd sqlserver:\
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
        
        ## SetDBConnection - uses Windows Service (type 2), username is ignored
            write-host "Calling SetDatabaseConnection server $server, DB $dbName"
            $r = $RSObject.SetDatabaseConnection($server,$dbName,2,'','')
            CheckResult $r "SetDatabaseConnection"  
        
        ## Setting the Report Manager URL ##
        
        write-host -foregroundcolor green "Setting the Report Manager URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## SetVirtualDirectory for Reports (Report Manager) site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportManager','Reports',1033)
            CheckResult $r "SetVirtualDirectory"
        
        ## ReserveURL for ReportManager  - port $HTTPport
            write-host "Calling ReserveURL for ReportManager, port $HTTPport"
            $r = $RSObject.ReserveURL('ReportManager',"http://+:$HTTPport",1033)
            CheckResult $r "ReserveURL for ReportManager port $HTTPport"
        
        write-host -foregroundcolor green "Open Firewall port for $HTTPport"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## Open Firewall port for $HTTPport
            New-NetFirewallRule -DisplayName “Report Server (TCP on port $HTTPport)” -Direction Inbound –Protocol TCP –LocalPort $HTTPport
            write-host "Added rule Report Server (TCP on port $HTTPport) in Windows Firewall"
        
        write-host 'Operations completed, Report Server is ready'
        write-host -foregroundcolor DarkGray $starttime StartTime
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time

1. Ako ste stvorili u VM s HTTP priključak osim 80, izmijeniti parametar $HTTPport = 80.

1. Skripta trenutno je konfiguriran za Reporting Services. Ako želite pokrenuti skriptu komponente Reporting Services, izmijenite verziju dio put do naziva za "v11" na početak WmiObject izjava.

1. Pokrenite skriptu.

**Provjera valjanosti**: da biste provjerili funkcionira li funkcionalnosti poslužitelja osnovno izvješće, u odjeljku [Provjera konfiguracije](#verify-the-configuration) u nastavku ovog članka.

### <a name="use-script-to-configure-the-report-server-and-https"></a>Možete konfigurirati na poslužitelju za izvješća i HTTPS pomoću sljedeće skripte

Da biste koristili Windows PowerShell za konfiguraciju poslužitelja izvješća, slijedite sljedeće korake. Konfiguracija obuhvaća HTTPS, ne HTTP.

1. Azure klasični, na portalu odaberite na VM, a zatim kliknite Poveži. Ovisno o vašoj konfiguraciji preglednika može se zatražiti da biste spremili .rdp datoteku za povezivanje s VM.

    ![povezivanje s azure virtualnog računala](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) Korištenje korisničko ime VM, korisničkog imena i lozinke koje ste konfigurirali stvaranja na VM. 

    Na primjer, na sljedećoj slici naziv VM je **ssrsnativecloud** i korisničko ime je **testuser**.

    ![korisničko ime za prijavu inlcudes vm](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)

1. Na VM, otvorite **Windows Očisti filtar** s administratorskim ovlastima. Očisti filtar instaliran je prema zadanim postavkama u sustavu Windows server 2012. Preporučuje se koristiti u filtar umjesto standardne komponente Windows PowerShell prozoru tako da skriptu zalijepite u filtar, izmijenite skripte i pokrenuti skriptu.

1. Da biste omogućili pokretanje skripti, pokrenite sljedeću naredbu komponente Windows PowerShell:

        Set-ExecutionPolicy RemoteSigned

    Zatim možete pokrenuti na sljedeći način provjerite je li pravila:

        Get-ExecutionPolicy

1. U **Windows Očisti filtar**, kliknite izbornik **Prikaz** , a zatim kliknite **Pokaži okno skripte**.

1. Kopirajte sljedeću skriptu i zalijepite ih u okno skripte za Windows Očisti filtar.

        ## This script configures the report server, including HTTPS
        $ErrorActionPreference = "Stop"
        $httpsport=443 # modify if you used a different port number when the HTTPS endpoint was created.
        
        # You can run the following command to get (.cloudapp.net certificates) so you can copy the thumbprint / certificate hash
        #dir cert:\LocalMachine -rec | Select-Object * | where {$_.issuer -like "*cloudapp*" -and $_.pspath -like "*root*"} | select dnsnamelist, thumbprint, issuer
        #
        # The certifacte hash is a REQUIRED parameter
        $certificatehash="" 
        # the certificate hash should not contain spaces
        
        if ($certificatehash.Length -lt 1) 
        {
            write-error "certificatehash is a required parameter"
        } 
        # Certificates should be all lower case
        $certificatehash=$certificatehash.ToLower()
        $server = $env:COMPUTERNAME
        # If the certificate is not a wildcard certificate, comment out the following line, and enable the full $DNSNAme reference.
        $DNSName="+"
        #$DNSName="$server.cloudapp.net"
        $DNSNameAndPort = $DNSName + ":$httpsport"
        
        ## Utility method for verifying an operation's result
        function CheckResult
        {
            param($wmi_result, $actionname)
            if ($wmi_result.HRESULT -ne 0) {
                write-error "$actionname failed. Error from WMI: $($wmi_result.Error)"
            }
        }
        
        $starttime=Get-Date
        write-host -foregroundcolor DarkGray $starttime StartTime
        
        ## ReportServer Database name - this can be changed if needed
        $dbName='ReportServer'
        
        write-host "The script will use $DNSNameAndPort as the DNS name and port" 
        
        ## Register for MSReportServer_ConfigurationSetting
        ## Change the version portion of the path to "v11" to use the script for SQL Server 2012
        $RSObject = Get-WmiObject -class "MSReportServer_ConfigurationSetting" -namespace "root\Microsoft\SqlServer\ReportServer\RS_MSSQLSERVER\v12\Admin"
        
        ## Reporting Services Report Server Configuration Steps
        
        ## 1. Setting the web service URL ##
        write-host -foregroundcolor green "Setting the web service URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## SetVirtualDirectory for ReportServer site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportServerWebService','ReportServer',1033)
            CheckResult $r "SetVirtualDirectory for ReportServer"
        
        ## ReserveURL for ReportServerWebService - port 80 (for local usage)
            write-host 'Calling ReserveURL port 80'
            $r = $RSObject.ReserveURL('ReportServerWebService','http://+:80',1033)
            CheckResult $r "ReserveURL for ReportServer port 80" 
        
        ## ReserveURL for ReportServerWebService - port $httpsport
            write-host "Calling ReserveURL port $httpsport, for URL: https://$DNSNameAndPort"
            $r = $RSObject.ReserveURL('ReportServerWebService',"https://$DNSNameAndPort",1033)
            CheckResult $r "ReserveURL for ReportServer port $httpsport" 
        
        ## CreateSSLCertificateBinding for ReportServerWebService port $httpsport
            write-host "Calling CreateSSLCertificateBinding port $httpsport, with certificate hash: $certificatehash"
            $r = $RSObject.CreateSSLCertificateBinding('ReportServerWebService',$certificatehash,'0.0.0.0',$httpsport,1033)
            CheckResult $r "CreateSSLCertificateBinding for ReportServer port $httpsport" 
            
        ## 2. Setting the Database ##
        write-host -foregroundcolor green "Setting the Database"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## GenerateDatabaseScript - for creating the database
            write-host "Calling GenerateDatabaseCreationScript for database $dbName"
            $r = $RSObject.GenerateDatabaseCreationScript($dbName,1033,$false)
            CheckResult $r "GenerateDatabaseCreationScript"
            $script = $r.Script
        
        ## Execute sql script to create the database
            write-host 'Executing Database Creation Script'
            $savedcvd = Get-Location
            Import-Module SQLPS                    ## this automatically changes to sqlserver provider
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
          
        ## GenerateGrantRightsScript 
            $DBUser = "NT Service\ReportServer"
            write-host "Calling GenerateDatabaseRightsScript with user $DBUser"
            $r = $RSObject.GenerateDatabaseRightsScript($DBUser,$dbName,$false,$true)
            CheckResult $r "GenerateDatabaseRightsScript"
            $script = $r.Script
        
        ## Execute grant rights script
            write-host 'Executing Database Rights Script'
            $savedcvd = Get-Location
            cd sqlserver:\
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
        
        ## SetDBConnection - uses Windows Service (type 2), username is ignored
            write-host "Calling SetDatabaseConnection server $server, DB $dbName"
            $r = $RSObject.SetDatabaseConnection($server,$dbName,2,'','')
            CheckResult $r "SetDatabaseConnection"  
        
        ## 3. Setting the Report Manager URL ##
        
        write-host -foregroundcolor green "Setting the Report Manager URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## SetVirtualDirectory for Reports (Report Manager) site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportManager','Reports',1033)
            CheckResult $r "SetVirtualDirectory"
        
        ## ReserveURL for ReportManager  - port 80
            write-host 'Calling ReserveURL for ReportManager, port 80'
            $r = $RSObject.ReserveURL('ReportManager','http://+:80',1033)
            CheckResult $r "ReserveURL for ReportManager port 80"
        
        ## ReserveURL for ReportManager - port $httpsport
            write-host "Calling ReserveURL port $httpsport, for URL: https://$DNSNameAndPort"
            $r = $RSObject.ReserveURL('ReportManager',"https://$DNSNameAndPort",1033)
            CheckResult $r "ReserveURL for ReportManager port $httpsport" 
        
        ## CreateSSLCertificateBinding for ReportManager port $httpsport
            write-host "Calling CreateSSLCertificateBinding port $httpsport with certificate hash: $certificatehash"
            $r = $RSObject.CreateSSLCertificateBinding('ReportManager',$certificatehash,'0.0.0.0',$httpsport,1033)
            CheckResult $r "CreateSSLCertificateBinding for ReportManager port $httpsport" 
        
        write-host -foregroundcolor green "Open Firewall port for $httpsport"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## Open Firewall port for $httpsport
            New-NetFirewallRule -DisplayName “Report Server (TCP on port $httpsport)” -Direction Inbound –Protocol TCP –LocalPort $httpsport
            write-host "Added rule Report Server (TCP on port $httpsport) in Windows Firewall"
        
        write-host 'Operations completed, Report Server is ready'
        write-host -foregroundcolor DarkGray $starttime StartTime
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time

1. Izmjena parametra **$certificatehash** u skripti:

    - To je **obavezan** parametar. Ako niste spremili certifikat vrijednost iz prethodnih koraka, koristite jedan od sljedećih načina da biste kopirali raspršivanje vrijednost certifikat otisak prsta potvrde.:

        Na VM, otvorite Windows Očisti filtar, a zatim pokrenite sljedeću naredbu:

            dir cert:\LocalMachine -rec | Select-Object * | where {$_.issuer -like "*cloudapp*" -and $_.pspath -like "*root*"} | select dnsnamelist, thumbprint, issuer

        Rezultat će izgledati otprilike ovako. Ako skriptu vraća prazni redak, na VM ne certifikat konfiguriran na primjer, potražite u odjeljku [za korištenje virtualnih računala Self-signed potvrde](#to-use-the-virtual-machines-self-signed-certificate).
    
    OR
    
    - Na VM pokrenut mmc.exe, a zatim dodajte dodatak za **potvrde** .
    
    - U odjeljku čvor **Pouzdanih korijenskih certifikata za izdavanje certifikata** dvokliknite naziv certifikata. Ako koristite samopotpisani certifikat na VM certifikat pod nazivom nakon naziva DNS-a na VM, a završava **cloudapp.net**.
    
    - Kliknite karticu **Detalji** .
    
    - Kliknite **otisak prsta**. Vrijednost na otisak prsta prikazuje se u polje detalje o, na primjer af 11 60 b6 4b 28 8 d 89 0a 82 12 ff 6b a9 c3 66 4f 31 90 48
    
    - **Prije nego što pokrenete skriptu**, uklanjanje razmaka između parova vrijednosti. Na primjer af1160b64b288d890a8212ff6ba9c3664f319048

1. Izmjena parametra **$httpsport** : 

    - Ako ste koristili priključak 443 za krajnju točku HTTPS, pa ne morate ažurirati taj parametar u skripti. U suprotnom pomoću vrijednost priključka koji ste odabrali kada ste konfigurirali krajnjoj točki privatnih HTTP na VM.

1. Izmjena parametra **$DNSName** : 

    - Skripta je konfiguriran za potvrdu zamjenske $DNSName = "+". Ako to ne želite da biste konfigurirali za povezivanje za potvrdu zamjenskih znakova, komentar izvan $DNSName ="+"i omogućivanje sljedeći redak, cijeli $DNSNAme referenca, ## $DNSName="$server.cloudapp.net".

        Ako ne želite koristiti naziv DNS virtualnog računala Reporting Services, promijenite vrijednost $DNSName. Ako koristite parametar, certifikat mora koristiti taj naziv i Registriranje naziva globalno na DNS poslužitelj.

1. Skripta trenutno je konfiguriran za Reporting Services. Ako želite pokrenuti skriptu komponente Reporting Services, izmijenite verziju dio put do naziva za "v11" na početak WmiObject izjava.

1. Pokrenite skriptu.

**Provjera valjanosti**: da biste provjerili funkcionira li funkcionalnosti poslužitelja osnovno izvješće, u odjeljku [Provjera konfiguracije](#verify-the-connection) u nastavku ovog članka. Da biste provjerili certifikat povezivanje otvorite naredbeni redak s administratorskim ovlastima, a zatim pokrenite sljedeću naredbu:

    netsh http show sslcert

Rezultat će obuhvaćaju sljedeće:

    IP:port                      : 0.0.0.0:443

    Certificate Hash             : f98adf786994c1e4a153f53fe20f94210267d0e7

### <a name="use-configuration-manager-to-configure-the-report-server"></a>Pomoću upravitelja konfiguracije konfigurirajte poslužitelj za izvješća

Ako ne želite pokrenuti skriptu PowerShell konfiguriranje poslužitelja za izvješće, slijedite korake u ovom odjeljku da biste pomoću upravitelja konfiguracije nativnom načinu rada servisa Reporting Services da biste konfigurirali poslužitelj za izvješća.

1. Azure klasični, na portalu odaberite na VM, a zatim kliknite Poveži. Pomoću korisničkog imena i lozinke koje ste konfigurirali stvaranja na VM.

    ![povezivanje s azure virtualnog računala](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif)

1. Pokretanje servisa Windows update i instaliranje ažuriranja sustava VM. Ako je potrebno ponovno pokretanje u VM, ponovno pokrenite na VM i povezati s VM s portala za Azure klasični.

1. S izbornika Start na VM upišite **Reporting Services** i otvorite **Upravitelj konfiguracije za usluge Reporting**.

1. Ostavite zadane vrijednosti za **Naziv poslužitelja** i **Instanca poslužitelja za izvješća**. Kliknite **Poveži**.

1. U lijevom oknu kliknite **URL web-servisa**.

1. Prema zadanim postavkama RS je konfiguriran za HTTP priključak 80 IP "Sve dodijeljeno". Da biste dodali HTTPS:

    1. U **SSL certifikata**: Odaberite certifikat koji želite koristiti, na primjer [naziv VM]. cloudapp.net. Ako nisu navedeni potvrde potražite u odjeljku **Korak 2: Stvaranje certifikata za poslužitelj** da biste saznali kako instalirati i pouzdanosti certifikata na na VM.
    
    1. U odjeljku **SSL priključak**: Odaberite 443. Ako ste konfigurirali krajnjoj točki privatnih HTTP u VM s drugi privatne priključak, koristiti tu vrijednost ovdje.
    
    1. Kliknite **Primijeni** i pričekajte da biste dovršili postupak.

1. U lijevom oknu kliknite **baza podataka**.

    1. Kliknite **Promijeni Databas**e.
    
    1. Kliknite **Stvori novi izvješća poslužitelja baze podataka** , a zatim kliknite **Dalje**.
    
    1. Ostavite zadani **Naziv poslužitelja**: kao na VM naziv i ostavite zadana **Vrsta provjere autentičnosti** kao **Trenutnog korisnika** – **Integriranu sigurnost**. Kliknite **Dalje**.
    
    1. Ostavite zadani **Naziv baze podataka** kao **ReportServer** , a zatim kliknite **Dalje**.
    
    1. Ostavite zadana **Vrsta provjere autentičnosti** kao što je **Servis za vjerodajnice** , a zatim kliknite **Dalje**.
    
    1. Kliknite **Dalje** na stranici **Sažetak** .
    
    1. Po dovršetku konfiguracije kliknite **Završi**.

1. U lijevom oknu kliknite **Report Manager URL-a**. Ostavite zadani **Virtualnog direktorija** kao **izvješća** , a zatim kliknite **Primijeni**.

1. Klikom na **Izlaz** zatvorite Upravitelj konfiguracije Reporting Services.

## <a name="step-4-open-windows-firewall-port"></a>Korak 4: Otvori Windows vatrozid priključak

>[AZURE.NOTE] Ako koristite neku od skripte konfiguriranje poslužitelja za izvješća možete preskočiti ovaj odjeljak. Skripta uključeni korak da biste otvorili priključak za vatrozid. Zadani je priključak 80 za HTTP i HTTPS na priključak 443.

Da biste povezali daljinski Report Manager ili poslužitelju izvješća za virtualnog računala, TCP krajnje potreban je na VM. Potreban je da biste otvorili isti priključak u vatrozidu za na VM. Krajnja točka stvorena kada se VM je dodijeljena.

Ovo poglavlje sadrži osnovne informacije o tome da biste otvorili priključak za vatrozid. Dodatne informacije potražite u članku [Konfiguriranje Vatrozida za pristup poslužitelju za izvješća](https://technet.microsoft.com/library/bb934283.aspx)

>[AZURE.NOTE] Ako ste koristili skriptu da biste konfigurirali poslužitelj za izvješća, možete preskočiti ovaj odjeljak. Skripta uključeni korak da biste otvorili priključak za vatrozid.

Ako ste konfigurirali privatne priključak za HTTPS osim 443, komponenta izmijeniti sljedeću skriptu. Da biste otvorili priključak **443** Vatrozid za Windows, napravite sljedeće:

1. Da biste otvorili prozor programa Windows PowerShell s administratorskim ovlastima.

1. Ako ste koristili priključak osim 443 kada ste konfigurirali krajnju točku HTTPS na VM, ažurirajte priključka u sljedeću naredbu, a zatim pokrenite naredbu:

        New-NetFirewallRule -DisplayName “Report Server (TCP on port 443)” -Direction Inbound –Protocol TCP –LocalPort 443

1. Kada se naredba dovrši, **u redu** prikazuje se u naredbeni redak.

Da biste provjerili priključak otvoren, otvorite prozor komponente Windows PowerShell i pokrenite sljedeću naredbu:

    get-netfirewallrule | where {$_.displayname -like "*report*"} | select displayname,enabled,action

## <a name="verify-the-configuration"></a>Provjerite konfiguraciju

Da biste potvrdili da sada radi funkcionalnosti poslužitelja osnovno izvješće, otvorite preglednik s administratorskim ovlastima, a zatim prijeđite do sljedeće izvješća poslužitelja ad izvješća Upravitelj URL-ova:

- Na VM, idite na URL poslužitelja za izvješća:

        http://localhost/reportserver

- Na VM, idite na URL Upravitelj izvješća:

        http://localhost/Reports

- S lokalnog računala, otvorite **udaljene** izvješća Upravitelj na na VM. Ažurirajte DNS naziv u sljedećem primjeru po potrebi. Kada se pojavi upit za lozinku, koristite administratorske vjerodajnice ste napravili kad je na VM je dodijeljena. Korisničko ime se [domena]\[korisničko ime] obliku, gdje je domena VM naziv računala, primjerice ssrsnativecloud\testuser. Ako se ne koriste HTTP**S**, uklonite **s** u URL-u. U sljedećem odjeljku informacije o stvaranju ostale korisnike na VM.

        https://ssrsnativecloud.cloudapp.net/Reports

- S lokalnog računala, otvorite URL poslužitelja za daljinski izvješća. Ažurirajte DNS naziv u sljedećem primjeru po potrebi. Ako se ne koriste HTTPS, uklonite na s u URL-u.

        https://ssrsnativecloud.cloudapp.net/ReportServer

## <a name="create-users-and-assign-roles"></a>Dodavanje korisnika i Dodjela uloga

Nakon konfiguriranja i provjera poslužitelju za izvješća, uobičajene administrativne zadatak je stvaranje jednog ili više korisnika i korisnicima dodijelite uloge servisa Reporting Services. Dodatne informacije potražite u sljedećim člancima:

- [Stvaranje lokalne korisničkog računa](https://technet.microsoft.com/library/cc770642.aspx)

- [Dodjela korisničkog pristupa na poslužitelju za izvješća (Report Manager)](https://msdn.microsoft.com/library/ms156034.aspx))

- [Stvaranje i upravljanje dodjele uloga](https://msdn.microsoft.com/library/ms155843.aspx)

## <a name="to-create-and-publish-reports-to-the-azure-virtual-machine"></a>Stvaranje i objavljivanje izvješća Azure virtualnog računala

U sljedećoj su tablici prikazane su neke od mogućnosti dostupnih za objavljivanje postojeća izvješća s lokalnog računala s poslužiteljem izvješća koji se nalaze na sustava Microsoft Azure virtualnog računala:

- **Skripta RS.exe**: korištenje RS.exe skriptu da biste kopirali izvješće stavki iz i postojećeg izvješća poslužitelja za sustava Microsoft Azure virtualnog računala. Dodatne informacije potražite u odjeljku "Nativnom načinu rada u nativni način rada – Microsoft Azure virtualnog računala" u [rs.exe Reporting Services ogledne skripte za migriranje sadržaja između poslužiteljima izvješća](https://msdn.microsoft.com/library/dn531017.aspx).

- **Report Builder**: virtualnog računala obuhvaća kliknite-jednom verziju sustava Microsoft SQL Server Report Builder. Da biste pokrenuli izvješće Sastavljač prvi put na virtualnog računala:

    1. Pokretanje preglednika s administratorskim ovlastima.
    
    1. Pronađite izvješće Upravitelj na virtualnog računala i na vrpci kliknite **Report Builder** .

    Dodatne informacije potražite u članku [Instalacija, Uninstalling, i pomoćne Report Builder](https://technet.microsoft.com/library/dd207038.aspx).

- **SQL Server Data Tools: VM**: Ako ste stvorili VM s SQL Server 2012, a zatim SQL Server Data Tools je instaliran na računalu virtualne i možete koristiti za stvaranje **Izvješća poslužitelja projekata** i izvješća na virtualnog računala. SQL Server Data Tools možete objaviti izvješća na poslužitelju izvješća na virtualnog računala.
    
    Ako ste stvorili u VM sa sustavom SQL server 2014., možete instalirati SQL Server podataka Alati – BI za visual Studio. Dodatne informacije potražite u sljedećim člancima:

    - [Microsoft SQL Server Data Tools – Business Intelligence za Visual Studio 2013](https://www.microsoft.com/download/details.aspx?id=42313)

    - [Microsoft SQL Server Data Tools – Business Intelligence za Visual Studio 2012](https://www.microsoft.com/download/details.aspx?id=36843)

    - [SQL Server Data Tools i SQL Server poslovno obavještavanje (SSDT BI)](http://curah.microsoft.com/30004/sql-server-data-tools-ssdt-and-sql-server-business-intelligence)

- **SQL Server Data Tools: udaljene**: na lokalnom računalu, stvaranje projekta Reporting Services u SQL Server Data Tools koja sadrži izvješća servisa Reporting Services. Konfiguriranje projekta možete povezati URL web-servisa.

    ![Svojstva projekta SSDT SSRS projekta](./media/virtual-machines-windows-classic-ps-sql-report/IC650114.gif)

- **Koristi skriptu**: koristite skriptu da biste kopirali sadržaj poslužitelj za izvješća. Dodatne informacije potražite u članku [rs.exe Reporting Services ogledne skripte za migriranje sadržaja između poslužiteljima izvješća](https://msdn.microsoft.com/library/dn531017.aspx).

## <a name="minimize-cost-if-you-are-not-using-the-vm"></a>Minimiziranje trošak ako se ne koriste u VM

>[AZURE.NOTE] Da biste minimizirali naknade za na virtualnim računalima sustava Azure kada nije u upotrebi, isključite VM s portala za Azure klasični. Ako koristite Windows mogućnosti power unutar na VM isključiti na VM, i dalje se naplaćuju isti iznos za na VM. Da biste smanjili troškove, morate isključiti VM Azure klasični portalu. Ako vam više nije potrebna na VM, imajte na umu da biste izbrisali na VM i pridruženi .vhd datoteke radi izbjegavanja naknada za pohranu. Dodatne informacije potražite u odjeljku najčešća Pitanja na [Virtualnim strojevima cijene pojedinosti](https://azure.microsoft.com/pricing/details/virtual-machines/).

## <a name="more-information"></a>Dodatne informacije

### <a name="resources"></a>Resursi

- Sličan sadržaj koji se odnose na implementaciju poslužitelja SQL Server poslovno obavještavanje i sustava SharePoint 2013, potražite u članku [Korištenje komponente Windows PowerShell za stvaranje Azure VM s SQL Server BI i sustavu SharePoint 2013](https://msdn.microsoft.com/library/azure/dn385843.aspx).

- Sličan sadržaj koji se odnose na uvođenje više poslužitelja SQL Server poslovno obavještavanje i sustava SharePoint 2013, potražite u članku [Implementacija sustava SQL Server poslovno obavještavanje u Azure virtualnim računalima](https://msdn.microsoft.com/library/dn321998.aspx).

- Općenite informacije vezane uz implementacijama sustava SQL Server poslovno obavještavanje u Azure virtualnim strojevima, potražite u članku [SQL Server poslovno obavještavanje u Azure virtualnih računala](virtual-machines-windows-classic-ps-sql-bi.md).

- Dodatne informacije o trošak Azure naknade za izračun potražite karticu virtualnim strojevima [Azure Kalkulator cijene](https://azure.microsoft.com/pricing/calculator/?scenario=virtual-machines).

### <a name="community-content"></a>Sadržaj iz zajednice

- Korak po korak upute o stvaranju Reporting Services izvorni poslužitelj za izvješća način bez korištenja skripte potražite u članku [Hosting SQL servis za prijavu na Azure virtualnog računala](http://adititechnologiesblog.blogspot.in/2012/07/hosting-sql-reporting-service-on-azure.html).

### <a name="links-to-other-resources-for-sql-server-in-azure-vms"></a>Veze na druge resurse za SQL Server u Azure VMs

[SQL Server na pregled Azure virtualnim strojevima](virtual-machines-windows-sql-server-iaas-overview.md)
