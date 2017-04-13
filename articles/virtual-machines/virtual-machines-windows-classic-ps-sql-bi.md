<properties
    pageTitle="SQL Server poslovno obavještavanje | Microsoft Azure"
    description="U ovoj se temi koristi resursa koje su stvorene pomoću model klasični implementacije i opisane različite poslovne inteligencije (BI) značajki dostupnih za SQL Server na virtualnim računalima sustava Azure (VMs) pokrenut."
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

# <a name="sql-server-business-intelligence-in-azure-virtual-machines"></a>SQL Server poslovno obavještavanje u Azure virtualnim strojevima

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Galerija Microsoft Azure virtualnog računala sadrži slike koje sadrže instalacija sustava SQL Server. Izdanja sustava SQL Server podržane u galeriji slike su iste instalacijske datoteke možete instalirati na web-mjesto lokalnog računala i u okvir za virtualnim računalima. U ovoj se temi ukratko se prikazuju SQL Server poslovne inteligencije (BI) značajke instalirane u slike i konfiguracija korake potrebne nakon je dodijeljen virtualnog računala. U ovoj se temi opisuju i topologija podržana implementacija značajke Poslovnog obavještavanja i najbolje prakse.

## <a name="license-considerations"></a>Razmatranja licence

Da biste licencu sustava SQL Server na virtualnim strojevima programa Microsoft Azure na dva načina:

1. Licenca mobilnost pogodnosti koje su dio Tehnološko jamstvo. Dodatne informacije potražite u članku [Mobilnost licenci putem Tehnološko jamstvo na Azure](https://azure.microsoft.com/pricing/license-mobility/).

1. Plaćanje po sat stopa od Azure virtualnim strojevima sa sustavom SQL Server instaliran. U odjeljku "SQL Server" u [Virtualnim strojevima cijene](https://azure.microsoft.com/pricing/details/virtual-machines/#Sql).

Dodatne informacije o licenci i aktualne stope potražite u članku [Najčešća pitanja o licenciranju virtualnim računalima](https://azure.microsoft.com/pricing/licensing-faq/%20/).

## <a name="sql-server-images-available-in-azure-virtual-machine-gallery"></a>SQL Server dostupnoj u galeriji Azure virtualnog računala

Galerija Microsoft Azure virtualnog računala sadrži nekoliko slike koje sadrže Microsoft SQL Server. Softvera instaliranog na slike virtualnog računala razlikuje se ovisno o verzija operacijskog sustava i verzija sustava SQL Server. Na popisu slike koje su dostupne u galeriji Azure virtualnog računala često mijenja.

![Slika SQL azure VM galerije](./media/virtual-machines-windows-classic-ps-sql-bi/IC741367.png)

![PowerShell](./media/virtual-machines-windows-classic-ps-sql-bi/IC660119.gif) Sljedeću skriptu komponente PowerShell vraća popis Azure slike koje sadrže "SQL-Server" na na ImageName:

    # assumes you have already uploaded a management certificate to your Microsoft Azure Subscription. View the thumbprint value from the "settings" menu in Azure classic portal.

    $subscriptionID = ""    # REQUIRED: Provide your subscription ID.
    $subscriptionName = "" # REQUIRED: Provide your subscription name.
    $thumbPrint = "" # REQUIRED: Provide your certificate thumbprint.
    $certificate = Get-Item cert:\currentuser\my\$thumbPrint # REQUIRED: If your certificate is in a different store, provide it here.-Ser  store is the one specified with the -ss parameter on MakeCert

    Set-AzureSubscription -SubscriptionName $subscriptionName -Certificate $certificate -SubscriptionID $subscriptionID

    Write-Host -foregroundcolor green "List of available gallery images where imagename contains 2016"
    Write-Host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
    get-azurevmimage | where {$_.ImageName -Like "*SQL-Server-2016*"} | select imagename,category, location, label, description

    Write-Host -foregroundcolor green "List of available gallery images where imagename contains 2014"
    Write-Host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
    get-azurevmimage | where {$_.ImageName -Like "*SQL-Server-2014*"} | select imagename,category, location, label, description

Dodatne informacije o izdanja i značajke koje podržava SQL Server potražite u sljedećim člancima:

- [Izdanja sustava SQL Server](https://www.microsoft.com/server-cloud/products/sql-server-editions/#fbid=Zae0-E6r5oh)

- [Značajke koje podržavaju izdanja sustava SQL Server 2016](https://msdn.microsoft.com/library/cc645993.aspx)

### <a name="bi-features-installed-on-the-sql-server-virtual-machine-gallery-images"></a>Značajke POSLOVNOG obavještavanja instalirana na SQL Server virtualnog računala galerije slika

U sljedećoj su tablici navedene značajke potpore poslovnom odlučivanju koji je instaliran na uobičajeni slika galerije Microsoft Azure virtualnog računala za SQL Server"

- SQL Server 2016 RC3

- Enterprise SP1 za SQL Server 2014.

- Standardna SP1 za SQL Server 2014.

- SQL Server 2012 SP2 Enterprise

- SQL Server 2012 SP2 Standard

|Značajka BI za SQL Server|Instalirano na galerije|Bilješke|
|---|---|---|
|**Nativnom načinu rada servisa za izvješćivanje o pogreškama**|Da|Instaliran, ali zahtijeva konfiguracije, uključujući URL Upravitelj izvješća. U odjeljku [Konfiguriranje servisa za izvješćivanje](#configure-reporting-services).|
|**Komponente Reporting Services načinu rada sustava SharePoint**|ne|Galerija brzih Microsoft Azure virtualnog računala ne obuhvaća SharePoint ili SharePoint instalacijskih datoteka. <sup>1</sup>|
|**Dubinsko pretraživanje Analysis Services višedimenzionalne i podataka (OLAP-a)**|Da|Instalacije i konfiguracije kao zadane instance komponente Analysis Services|
|**Tablični komponente Analysis Services**|ne|Podržane u sustavu SQL Server 2012, 2014 i 2016 slike, ali ga nije instaliran prema zadanim postavkama. Instalirajte drugu instancu komponente Analysis Services. U ovoj temi potražite u članku u odjeljku instalirajte drugih servisa SQL Server i značajke.|
|**Analysis Services Power Pivot za SharePoint**|ne|Galerija brzih Microsoft Azure virtualnog računala ne obuhvaća SharePoint ili SharePoint instalacijskih datoteka. <sup>1</sup>|

<sup>1</sup> dodatne informacije o web-mjesto sustava SharePoint i u okvir za Azure virtualnim računalima potražite u članku [Microsoft Azure arhitekturi za SharePoint 2013](https://technet.microsoft.com/library/dn635309.aspx) i [Implementaciju sustava SharePoint na virtualnim strojevima programa Microsoft Azure](https://www.microsoft.com/download/details.aspx?id=34598).

![PowerShell](./media/virtual-machines-windows-classic-ps-sql-bi/IC660119.gif) Pokrenite sljedeću naredbu komponente PowerShell dobit ćete popis Instalirani servisi koji sadrže "SQL" u nazivu servisa.

    get-service | Where-Object{ $_.DisplayName -like '*SQL*' } | Select DisplayName, status, servicetype, dependentservices | format-Table -AutoSize

## <a name="general-recommendations-and-best-practices"></a>Općenite preporuke i najbolje prakse

- Minimalna veličina preporučenih za virtualnog računala jest **A3** prilikom korištenja SQL Server Enterprise Edition. Veličina virtualnog računala **A4** preporučuje implementacijama SQL Server BI Analysis Services i komponente Reporting Services.

    Informacije o trenutnom veličina VM potražite u članku [Veličine virtualnog računala za Azure](virtual-machines-linux-sizes.md).

- Preporučenim načinom rada za upravljanje disk je da biste pohranili podatke, zapisnika i sigurnosno kopiranih datoteka na pogonima osim **C**: i **D**:. Ako, primjerice, stvoriti diskova podataka **E**: i **F**:.

    - Pogon predmemoriranje pravila za zadani pogon **C**: nije optimiziran za rad s podacima.

    - **D**: pogon je privremeni pogon koji se koristi prvenstveno za datoteku stranice. **D**: pogon je ista i i ne sprema se u spremište blobova platforme. Upravljanje zadatke kao što su promjena veličine virtualnog računala Vrati **D**: pogon. Preporučuje se **ne** koriste **D**: pogon za datoteke baze podataka, uključujući tempdb.

    Dodatne informacije o stvaranju i pridruživanje diskova potražite u članku [kako priložiti podatkovni Disk na virtualnog računala](virtual-machines-windows-classic-attach-disk.md).

- Zaustavljanje ili deinstalacija servisa ne namjeravate koristiti. Za primjer ako virtualnog računala koristi samo Reporting Services, zaustavljanje ili deinstalacija servisa Analysis Services i SQL Integracijskim uslugama poslužitelja. Na sljedećoj je slici primjer servise koji su pokrenuti prema zadanim postavkama.

    ![Servisa SQL Server](./media/virtual-machines-windows-classic-ps-sql-bi/IC650107.gif)

    >[AZURE.NOTE] Modul za baze podataka sustava SQL Server, potreban je podržana scenariji BI. U jednom poslužitelju VM topologije modul baze podataka potreban je da biste se izvoditi na istom VM.

    Dodatne informacije potražite na sljedeći način: [Deinstalacija servisa Reporting Services](https://msdn.microsoft.com/library/hh479745.aspx) i [Deinstalacija programa Instance programa Analysis Services](https://msdn.microsoft.com/library/ms143687.aspx).

- **Windows Update** potražiti nove 'važna ažuriranja". Microsoft Azure virtualnog računala slike često se osvježavaju; No važna ažuriranja nije postaju dostupne putem **Servisa Windows Update** kada slika VM posljednjeg osvježavanja.

## <a name="example-deployment-topologies"></a>Primjer implementacije topologija

Slijedi primjer implementacijama koje koriste virtualnim računalima sustava Microsoft Azure. Topologija u te dijagrami su samo neke od mogućih topologija možete koristiti pomoću značajke SQL Server Poslovnog obavještavanja i virtualnim računalima sustava Microsoft Azure.

### <a name="single-virtual-machine"></a>Jedan virtualnog računala

Analysis Services, Reporting Services, modul za baze podataka SQL Server, i izvora podataka na jednom računalu virtualne.

![scenarij poslovnog iass 1 virtualnog računala](./media/virtual-machines-windows-classic-ps-sql-bi/IC650108.gif)

### <a name="two-virtual-machines"></a>Dva virtualnim strojevima

- Komponente Analysis Services, Reporting Services i modul za baze podataka SQL Server na jednom računalu virtualne. U ovom implementacije obuhvaća izvješća poslužitelja baze podataka.

- Izvori podataka na drugi VM. Drugi VM obuhvaća modul za baze podataka SQL Server kao izvor podataka.

![scenarij 2 virtualnim strojevima poslovnog iaas](./media/virtual-machines-windows-classic-ps-sql-bi/IC650109.gif)

### <a name="mixed-azure--data-on-azure-sql-database"></a>Kombinirani Azure – podatke na baze podataka Azure SQL

- Komponente Analysis Services, Reporting Services i modul za baze podataka SQL Server na jednom računalu virtualne. U ovom implementacije obuhvaća izvješća poslužitelja baze podataka.

- Izvor podataka je baze podataka Azure SQL.

![vm scenariji poslovnog iaas i AzureSQL kao izvor podataka](./media/virtual-machines-windows-classic-ps-sql-bi/IC650110.gif)

### <a name="hybrid-data-on-premises"></a>Hibridno – podataka lokalnog

- U ovom primjeru implementaciji servisa Analysis Services Reporting Services i modul za baze podataka SQL Server pokreću se na jednu virtualnog računala. Virtualnog računala hostira baze podataka za poslužitelj za izvješća. Virtualnog računala pridruženo lokalne domene putem virtualne umrežavanje Azure ili neke druge VPN tuneliranje rješenja.

- Lokalni je izvor podataka.

![poslovnog iaas scenariji vm i na lokalnu instancu izvora podataka](./media/virtual-machines-windows-classic-ps-sql-bi/IC654384.gif)

## <a name="reporting-services-native-mode-configuration"></a>Konfiguriranje Nativnom načinu rada servisa za izvješćivanje o pogreškama

Galerije virtualnog računala za SQL Server sadrži Reporting Services nativni način instaliran, no poslužitelj za izvješća nije konfiguriran. Koraci u ovom odjeljku Konfiguriranje poslužitelja za izvješća servisa Reporting Services. Detaljnije informacije o konfiguriranju Reporting Services Nativnom načinu rada potražite u članku [Instalacija Reporting Services nativni način izvješća poslužitelja (SSRS)](https://msdn.microsoft.com/library/ms143711.aspx).

>[AZURE.NOTE] Sličan sadržaj koji koristi skripte komponente Windows PowerShell za konfiguriranje poslužitelja za izvješće, potražite u članku [Korištenje ljuske PowerShell za stvaranje Azure VM s na izvorni način izvješće poslužitelj za](virtual-machines-windows-classic-ps-sql-report.md).

### <a name="connect-to-the-virtual-machine-and-start-the-reporting-services-configuration-manager"></a>Povezivanje s virtualnog računala i počnite upravitelju konfiguracije Reporting Services

Postoje dva uobičajena tijekova rada za povezivanje za programa Azure virtualnog računala:

- Da biste povezali u navigacijskom oknu kliknite naziv virtualnog računala, a zatim kliknite **Poveži**. Otvorit će se udaljene radne površine i naziv računala automatski popunjavaju.

    ![povezivanje s azure virtualnog računala](./media/virtual-machines-windows-classic-ps-sql-bi/IC650112.gif)

- Povezivanje s virtualnog računala sa sustavom Windows udaljene radne površine. U korisničkom sučelju udaljene radne površine:

    1. Upišite **Naziv usluge oblaka** kao naziv računala.

    1. Upišite dvotočku (:) i broj javno priključka koji je konfiguriran za krajnje TCP udaljene radne površine.

        Myservice.cloudapp.NET:63133

        Dodatne informacije potražite u članku [što je servis u oblaku?](https://azure.microsoft.com/manage/services/cloud-services/what-is-a-cloud-service/).

**Pokrenite Upravitelj konfiguracije servisa za izvješćivanje o pogreškama.**

1. U **sustavu Windows Server 2012**:

1. Na **početnom** zaslonu upišite **Reporting Services** da biste vidjeli popis aplikacija.

1. Desnom tipkom miša kliknite **Upravitelj konfiguracije za usluge Reporting** , a zatim kliknite **Pokreni kao Administrator**.

1. U **sustavu Windows Server 2008 R2**:

1. Kliknite **Start**, a zatim **Svi programi**.

1. Kliknite **Microsoft SQL Server 2016**.

1. Kliknite **Alati za konfiguraciju**.

1. Desnom tipkom miša kliknite **Upravitelj konfiguracije za usluge Reporting** , a zatim kliknite **Pokreni kao Administrator**.

Ili

1. Kliknite **Start**.

1. U dijaloški okvir **Pretraži programe i datoteke** upišite **reporting services**. Ako na VM se izvodi Windows Server 2012, upišite **reporting services** na zaslonu Start u sustavu Windows Server 2012.

1. Desnom tipkom miša kliknite **Upravitelj konfiguracije za usluge Reporting** , a zatim kliknite **Pokreni kao Administrator**.

    ![Potraži upravitelja konfiguracije ssrs](./media/virtual-machines-windows-classic-ps-sql-bi/IC650113.gif)

### <a name="configure-reporting-services"></a>Konfiguriranje servisa za izvješćivanje o pogreškama

**Račun za servis i URL web-usluge:**

1. Provjerite je li **Naziv poslužitelja** naziva lokalnog poslužitelja, a zatim kliknite **Poveži**.

1. Imajte na umu prazna **Baza podataka naziv poslužitelja za izvješća**. Baza podataka stvorena je nakon dovršetka konfiguracije.

1. Provjerite je li **Status izvješća poslužitelja** **rada**. Ako želite provjeriti servisa u sustavu Windows Server Manager servis je Windows servisa **SQL Server Reporting Services** .

1. Kliknite **Račun servisa** , a zatim Promjena računa po potrebi. Ako virtualnog računala se koristi u okruženju spojene koje nisu domena, dovoljno je ugrađeni račun za **ReportServer** . Dodatne informacije o računu usluge potražite u članku [Račun servisa](https://msdn.microsoft.com/library/ms189964.aspx).

1. U lijevom oknu kliknite **URL web-servisa** .

1. Kliknite **Primijeni** da biste konfigurirali zadane vrijednosti.

1. Imajte na umu **izvješća poslužitelja Web servisa URL-ova**. Imajte na umu zadani je priključak TCP 80 a dio URL-a. U noviji ćemo koraku stvoriti Microsoft Azure virtualnog računala krajnje za priključak.

1. U oknu s **rezultatima** , provjerite je li akcije uspješno dovršena.

**Baza podataka:**

1. U lijevom oknu kliknite **bazu podataka** .

1. Kliknite **Promijeni baza podataka**.

1. **Stvaranje novog izvješća poslužitelja baze podataka** provjerite je li odabrana, a zatim kliknite Dalje.

1. Provjerite je li **Naziv poslužitelja** , a zatim kliknite **Testiraj vezu**.

1. Ako je rezultat **Testiraj vezu uspjela**, kliknite **u redu** , a zatim kliknite **Dalje**.

1. Imajte na umu naziv baze podataka je **ReportServer** i **načinu poslužitelja za izvješće** je **izvorni** , a zatim kliknite **Dalje**.

1. Kliknite **Dalje** na stranici **vjerodajnica** .

1. Kliknite **Dalje** na stranici **Sažetak** .

1. Na stranici **tijek i završi** , kliknite **Dalje** .

**Web-URL portala ili Report Manager URL za 2012 i 2014.:**

1. Kliknite **Web-URL portala**ili **Report Manager URL** za 2014 i 2012, u lijevom oknu.

1. Kliknite **Primijeni**.

1. U oknu s **rezultatima** , provjerite je li akcije uspješno dovršena.

1. Kliknite **Izlaz**.

Informacije o poslužitelju dozvole za izvješća potražite u članku [Dodjelu dozvola za nativni način poslužitelju za izvješća](https://msdn.microsoft.com/library/ms156014.aspx).

### <a name="browse-to-the-local-report-manager"></a>Pregled da biste na lokalni Report Manager

Da biste provjerili konfiguraciju, otvorite upravitelj izvješća na na VM.

1. Na VM, pokrenite Internet Explorer s administratorskim ovlastima.

1. Pronađite http://localhost/reports na na VM.

### <a name="to-connect-to-remote-web-portal-or-report-manager-for-2014-and-2012"></a>Da biste povežite se s udaljenog web-portal ili Report Manager za 2014 i 2012.

Ako se želite povezati na web-portalu ili Report Manager za 2014 i 2012 na virtualnog računala s udaljenog računala, stvorite novi virtualnog računala TCP krajnjoj točki. Prema zadanim postavkama, poslužitelj za izvješća prati HTTP zahtjeva na **priključak 80**. Ako je konfigurirati URL adresa poslužitelja koristiti drugi priključak, morate navesti taj broj priključka u sljedeće upute.

1. Stvaranje krajnje točke za virtualnog računala priključka TCP 80. Dodatne informacije potražite u odjeljku [krajnje točke virtualnog računala i priključaka u vatrozidu](#virtual-machine-endpoints-and-firewall-ports) u ovom dokumentu.

1. Otvaranje priključka 80 u vatrozidu za virtualnog računala.

1. Idite na web-portalu ili Upravitelj izvješća pomoću Azure virtualnog računala **DNS naziva** kao naziv poslužitelja u URL-u. Ako, na primjer:

    **Poslužitelj za izvješća**: http://uebi.cloudapp.net/reportserver  **web-portalu**: http://uebi.cloudapp.net/reports

    [Konfiguriranje Vatrozida za pristup poslužitelju za izvješća](https://msdn.microsoft.com/library/bb934283.aspx)

### <a name="to-create-and-publish-reports-to-the-azure-virtual-machine"></a>Stvaranje i objavljivanje izvješća Azure virtualnog računala

U sljedećoj su tablici prikazane su neke od mogućnosti dostupnih za objavljivanje postojeća izvješća s lokalnog računala s poslužiteljem izvješća koji se nalaze na sustava Microsoft Azure virtualnog računala:

- **Report Builder**: virtualnog računala obuhvaća kliknite-jednom verziju sustava Microsoft SQL Server Report Builder za 2014 SQL i 2012. Da biste pokrenuli izvješća sastavljača prvi put na virtualnog računala s SQL 2016:

    1. Pokretanje preglednika s administratorskim ovlastima.

    1. Pronađite web-portal virtualnog računala i u gornjem desnom kutu odaberite ikonu za **Preuzimanje** .
    
    1. Odaberite **Report Builder**.

    Dodatne informacije potražite u članku [Pokretanje alata Report Builder](https://msdn.microsoft.com/library/ms159221.aspx).

- **SQL Server Data Tools**: VM: SQL Server Data Tools je instaliran na računalu virtualne, a može koristiti za stvaranje **Izvješća poslužitelja projekata** i izvješća na virtualnog računala. SQL Server Data Tools možete objaviti izvješća na poslužitelju izvješća na virtualnog računala.

- **SQL Server Data Tools: udaljene**: na lokalnom računalu, stvaranje projekta Reporting Services u SQL Server Data Tools koja sadrži izvješća servisa Reporting Services. Konfiguriranje projekta možete povezati URL web-servisa.

    ![Svojstva projekta SSDT SSRS projekta](./media/virtual-machines-windows-classic-ps-sql-bi/IC650114.gif)

- Stvaranje programa. VHD tvrdi disk koji sadrži izvješća i zatim prenesite i priložite pogon.

    1. Stvaranje programa. VHD tvrdi disk na lokalnom računalu koji sadrži izvješća.

    1. Stvaranje i instalirajte upravljanje certifikata.

    1. Prijenos datoteke VHD Azure pomoću cmdleta Dodaj AzureVHD [Stvaranje i prijenos programa Windows Server VHD za Azure](virtual-machines-windows-classic-createupload-vhd.md).

    1. Prilaganje disk virtualnog računala.

## <a name="install-other-sql-server-services-and-features"></a>Instalacija druge servisa SQL Server i značajke

Da biste instalirali dodatne servisa SQL Server, kao što su Analysis Services u tabličnom načinu pokrenite čarobnjak za postavljanje sustava SQL server. Datoteke za postavljanje nalaze se na lokalni disk virtualnog računala.

1. Kliknite **Start** , a zatim **Svi programi**.

1. Kliknite **Microsoft SQL Server 2016**, **2014 sustava Microsoft SQL Server** ili **Microsoft SQL Server 2012** , a zatim kliknite **Alati za konfiguraciju**.

1. Kliknite **Centar za instalaciju sustava SQL Server**.

Ili pokrenuti C:\SQLServer_13.0_full\setup.exe, C:\SQLServer_12.0_full\setup.exe ili C:\SQLServer_11.0_full\setup.exe

>[AZURE.NOTE] Prvi put kada pokrenete instalacijski program za SQL Server, dodatne instalacijske datoteke mogu se preuzeti i zahtijeva ponovno pokretanje računala virtualne i ponovnog pokretanja instalacije sustava SQL Server.
>
>Ako je potrebno više puta prilagodite sliku odabrana iz sustava Microsoft Azure virtualnog računala, razmislite o stvaranju vlastite slike za SQL Server. Analysis Services SysPrep funkcija omogućivanja s SQL Server 2012 SP1 CU2. Dodatne informacije potražite u članku [Zahtjevi za instalaciju sustava SQL Server pomoću SysPrep](https://msdn.microsoft.com/library/ee210754.aspx) i [Sysprep podrška za uloge poslužitelja](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).

### <a name="to-install-analysis-services-tabular-mode"></a>Da biste instalirali tablični načinu rada servisa za analizu

Koraci u ovom sekcije **Sažetak** instalacije dodatka Analysis Services tabular način. Dodatne informacije potražite u sljedećim člancima:

- [Instalirajte Analysis Services u tabličnom načinu rada](https://msdn.microsoft.com/library/hh231722.aspx)

- [Tablični Modeliranje (Adventure Works praktičnom vodiču)](https://msdn.microsoft.com/library/140d0b43-9455-4907-9827-16564a904268)

**Da biste instalirali Analysis Services Tablični način:**

1. U čarobnjaku za instalaciju sustava SQL Server u lijevom oknu kliknite **instalacije** , a zatim kliknite **Samostalna instalacija nove SQL poslužitelja ili dodavanje značajki na postojeće instalacije**.

    - Ako vidite **Pregled mapa**, dođite do c:\SQLServer_13.0_full, c:\SQLServer_12.0_full ili c:\SQLServer_11.0_full, a zatim kliknite **u redu**.

1. Kliknite **Dalje** na stranici ažuriranja proizvoda.

1. Na stranici **Instalacija vrsta** odaberite **Izvedi novu instalaciju sustava SQL Server** , a zatim kliknite **Dalje**.

1. Na stranici **Postavljanje ulogu** , kliknite **Instalacije značajke za SQL Server**.

1. Na stranici **Odabir značajki** kliknite **Analysis Services**.

1. Na stranici **Konfiguracija instancu** upišite opisni naziv, kao što su **tablično** u tekstne okvire **Pod nazivom instancu** i **Instance Id** .

1. Na stranici **Konfiguracija Analysis Services** odaberite **Tablični način**. Dodavanje trenutnog korisnika na popis administratorske dozvole.

1. Dovršite i zatvorite čarobnjak za instalaciju sustava SQL Server.

## <a name="analysis-services-configuration"></a>Konfiguracija Analysis Services

### <a name="remote-access-to-analysis-services-server"></a>Daljinski pristup poslužitelju Analysis Services

Analysis Service server podržava samo provjera autentičnosti sustava windows. Udaljeni pristup Analysis Services s klijentskim aplikacijama kao što je SQL Server Management Studio ili SQL Server Data Tools, virtualnog računala mora biti pridruženo lokalne domene, pomoću virtualne umrežavanje Azure. Dodatne informacije potražite u člancima, [Azure virtualne mreže](../virtual-network/virtual-networks-overview.md).

**Zadane instance** komponente Analysis Services očekuje podatke TCP priključak **2383**. Otvorite priključak u vatrozidu za virtualnih računala. Grupirani imenovani instance komponente Analysis Services također očekuje podatke priključka **2383**.

Za **pod nazivom instancu** komponente Analysis Services, za upravljanje pristupom priključak potreban je servisa SQL Server preglednik. Konfiguracija SQL Server preglednik zadani je priključak **2382**.

U vatrozidu za virtualnim strojevima otvorite priključak **2382** i stvorite statički Analysis Services pod nazivom priključak instance.

1. Da biste provjerili priključke koji se već koriste u VM i koje se koristi za proces priključke, pokrenite sljedeću naredbu s administratorskim ovlastima:

        netstat /ao

1. Korištenje SQL Server Management Studio Stvaranje statične Analysis Services pod nazivom instancu priključak ažuriranjem 'priključak vrijednost u tabličnom obliku instancu općenitih svojstava. Dodatne informacije potražite u članku na "koristi fixed priključak za zadanu ili pod nazivom instance" u članku [Konfiguriranje Vatrozida za Windows da biste Dopusti pristup Analysis Services](https://msdn.microsoft.com/library/ms174937.aspx#bkmk_fixed).

1. Ponovno pokrenite tablični instanca servisa Analysis Services.

Dodatne informacije potražite u odjeljku **krajnje točke virtualnog računala i priključaka u vatrozidu** u ovom dokumentu.

## <a name="virtual-machine-endpoints-and-firewall-ports"></a>Krajnje točke virtualnog računala i priključaka u vatrozidu

U ovom se odjeljku navedene Microsoft krajnje točke virtualnog računala Azure da biste stvorili i priključke za otvaranje u vatrozidima virtualnog računala. U sljedećoj su tablici ukratko opisano **TCP** priključke za stvaranje krajnje točke za priključke za otvaranje u vatrozidu za virtualnih računala.

- Ako koristite jednu VM i sljedeća dva stavke su true, morate stvoriti VM krajnje točke, a ne morate otvoriti priključaka u vatrozidu na na VM.

    - Ne daljinski povežete s značajki sustava SQL Server na na VM. Uspostavljanje udaljene radne površine veze da biste na VM i pristupanje značajki sustava SQL Server lokalno na na VM se smatra udaljene značajki sustava SQL Server.

    - Nemoj uključiti u VM za lokalne domene putem virtualne umrežavanje Azure ili drugo rješenje tunneling VPN-a.

- Ako domena nije pridružen virtualnog računala, ali želite daljinski povezati značajki sustava SQL Server na VM:

    - Otvaranje priključaka u vatrozidu na na VM.

    - Stvaranje krajnje točke virtualnog računala navedena priključke (*).

- Ako pridruženo virtualnog računala domene pomoću VPN tunelom kao što su Azure virtualne s mrežom, zatim krajnjih točaka nisu potrebni. No otvorite priključaka u vatrozidu na na VM.

  	|Priključak|Vrsta|Opis|
|---|---|---|
|**80**|TCP|Poslužitelj za izvješća daljinski pristup (*).|
|**1433**|TCP|SQL Server Management Studio (*).|
|**1434**|UDP|SQL Server preglednik. To je potrebno ako pridruženo VM u domeni.|
|**2382**|TCP|SQL Server preglednik.|
|**2383**|TCP|Zadane instance komponente SQL Server Analysis Services i grupirani imenovanih instance.|
|**Korisnički definirane**|TCP|Stvorite statički Analysis Services pod nazivom instancu priključak za broj priključka koji je odaberete i deblokiranje broj priključka u vatrozidu.|

Dodatne informacije o stvaranju krajnje točke potražite u sljedećim člancima:

- Stvaranje krajnje točke:[Postavljanje krajnje točke za virtualnog računala](virtual-machines-windows-classic-setup-endpoints.md).

- SQL Server: Potražite u odjeljku "Dovršeno konfiguracije korake za povezivanje s virtualnog računala korištenje SQL Server Management Studio" aktiviranja [sustava SQL Server virtualnog računala na Azure](virtual-machines-windows-portal-sql-server-provision.md).

Sljedeći dijagram prikazuje priključke za otvaranje u vatrozidu za VM dopustiti daljinski pristup značajkama i komponente na na VM.

![priključci da biste otvorili poslovnog aplikacijama u Azure VMs](./media/virtual-machines-windows-classic-ps-sql-bi/IC654385.gif)

## <a name="resources"></a>Resursi

- Pregledajte pravila o podršci za Microsoft poslužiteljski softver koristi u okruženju Azure virtualnog računala. Sljedeće teme navedene podrška za značajke kao što su BitLocker, klasteriranja i mrežnog opterećenja. [Poslužiteljski softver Microsoft podržava virtualnim računalima sustava za Azure](http://support.microsoft.com/kb/2721672).

- [SQL Server na pregled Azure virtualnim strojevima](virtual-machines-windows-sql-server-iaas-overview.md)

- [Virtualnim strojevima](https://azure.microsoft.com/documentation/services/virtual-machines/)

- [Dodjeljivanje SQL Server virtualnog računala na Azure](virtual-machines-windows-portal-sql-server-provision.md)

- [Kako priložiti podatkovni Disk virtualnog računala](virtual-machines-windows-classic-attach-disk.md)

- [Migracija baze podataka SQL Server na Azure VM](virtual-machines-windows-migrate-sql.md)

- [Odrediti način poslužitelja instance Analysis Services](https://msdn.microsoft.com/library/gg471594.aspx)

- [Višedimenzionalne Modeliranje (Adventure Works praktičnom vodiču)](https://technet.microsoft.com/library/ms170208.aspx)

- [Centar za dokumentaciju Azure](https://azure.microsoft.com/documentation/)

- [Korištenje servisa Power BI u Hibridnom okruženju](https://msdn.microsoft.com/library/dn798994.aspx)

>[AZURE.NOTE] [Slanje povratnih informacija i podaci za kontakt putem povezivanje za Microsoft SQL Server](https://connect.microsoft.com/SQLServer/Feedback)

### <a name="community-content"></a>Sadržaj iz zajednice

- [Upravljanje bazom podataka Azure SQL pomoću komponente PowerShell](http://blogs.msdn.com/b/windowsazure/archive/2013/02/07/windows-azure-sql-database-management-with-powershell.aspx)
