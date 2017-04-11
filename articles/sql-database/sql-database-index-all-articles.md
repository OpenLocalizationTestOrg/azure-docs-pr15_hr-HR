<properties
    pageTitle="Svih tema vezanih uz servisa za baze podataka SQL | Microsoft Azure"
    description="Popis svih tema vezanih uz servisa za Azure pod nazivom SQL baze podataka koji postoje na http://azure.microsoft.com/documentation/articles/, naslova i opisa."
    services="sql-database"
    documentationCenter=""
    authors="MightyPen"
    manager="jhubbard"
    editor="MightyPen"/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="genemi"/>


# <a name="all-topics-for-azure-sql-database-service"></a>Svih tema vezanih uz servisa za baze podataka SQL Azure

Ova tema sadrži popis svaki temu koja se primjenjuje izravno na servis za **Baze podataka SQL** Azure. Ova web-stranica za ključne riječi možete pretraživati pomoću **Ctrl + F**, da biste pronašli teme trenutni kamata.




## <a name="new"></a>Novi

| &nbsp; | Naslov | Opis |
| --: | :-- | :-- |
| 1 | [Daxko/CSI koriste Azure accelerate njegov ciklusa razvoja i poboljšati njegov služba i performanse](sql-database-implementation-daxko.md) | Informirajte se o tome kako Daxko/CSI koristi baze podataka SQL accelerate njegov ciklusa razvoja i poboljšati njegov služba i performanse |
| 2 | [Azure daje GEP globalni razgovor i veće učinkovitosti](sql-database-implementation-gep.md) | Informirajte se o tome kako GEP koristi baze podataka SQL da dođete do više globalnog klijentima i postigli veći učinkovitosti |
| 3 | [S Azure, SnelStart hitro proširen njegov servisa za poslovno brzinom od 1000 novih Azure SQL baza podataka po mjesecu](sql-database-implementation-snelstart.md) | Informirajte se o tome kako SnelStart koristi SQL baze podataka da biste hitro proširen njegov servisa za poslovno brzinom od 1000 novih Azure SQL baza podataka po mjesecu |
| 4 | [Umbraco koristi baze podataka SQL Azure brzo dodjele resursa i omjere services da biste tisuće drugih korisnika u oblaku](sql-database-implementation-umbraco.md) | Saznajte kako Umbraco koristi baze podataka SQL brzo Dodjela i skaliranje servise za tisuće drugih korisnika u oblaku |
| 5 | [Upravljanje bazom podataka Azure SQL pomoću komponente PowerShell](sql-database-manage-powershell.md) | Upravljanje bazom podataka SQL Azure sa servisom PowerShell. |
| 6 | [SSMS podrška za Azure AD MFA s bazom podataka SQL i SQL Data Warehouse](sql-database-ssms-mfa-authentication.md) | Korištenje više Factored provjere autentičnosti uz SSMS za SQL baze podataka i SQL Data Warehouse. |
| 7 | [Jedinice transakcije baze podataka (DTUs) te elastic baze podataka transakcije jedinice (eDTUs)](sql-database-what-is-a-dtu.md) | Objašnjenje koje Azure SQL baze podataka je transakcija jedinica. |


## <a name="updated-articles-sql-database"></a>Ažurirani članci, SQL baze podataka

U ovom se odjeljku navedeni članci koje su ažurirane nedavno, gdje je ažuriranje veliku ili značajan. Za svaki ažurirani članak gruba isječak teksta dodane markdown prikazat će se. Članci su ažurirani unutar datumskog raspona **2016, 08 i 22** **2016, 10 i 05**.

| &nbsp; | Članak | Ažurirani tekst, isječka | Kada ažurirati |
| --: | :-- | :-- | :-- |
| 8 | [Upravljanje bazom podataka Azure SQL pomoću komponente PowerShell](sql-database-command-line-tools.md) | Stvaranje grupe resursa za naše SQL baze podataka i povezani resursi Azure pomoću cmdleta New-AzureRmResourceGroup (https://msdn.microsoft.com/library/azure/mt759837.aspx). *$resourceGroupName = "resourcegroup1" $resourceGroupLocation = "northcentralus" Novo AzureRmResourceGroup-naziv $resourceGroupName – mjesto $resourceGroupLocation* Dodatne informacije potražite u članku korištenje Azure PowerShell s Azure Voditelj resursa (.. / powershell-azure-resursa-manager.md). Ogledne skripte, potražite u članku Stvaranje baze podataka SQL skriptu PowerShell (sql-baze podataka – get-rada – powershell.md create-a-sql-database-powershell-script). **Stvaranje baze podataka SQL server** Stvaranje baze podataka SQL server pomoću cmdleta New-AzureRmSqlServer (https://msdn.microsoft.com/library/azure/mt603715.aspx). Zamijenite *Poslužitelj1* naziv poslužitelja. Nazivi poslužitelja mora biti jedinstvena na svim poslužiteljima baze podataka SQL Azure. Ako naziv poslužitelja već preuzeli, prikazat će se pogreška. Ta se naredba može potrajati nekoliko minuta. Na resou | 2016 09 14 |
| 9 | [Kopiranje baze podataka Azure SQL pomoću komponente PowerShell](sql-database-copy-powershell.md) |   SQL izvor baze podataka (postojeće baze podataka da biste kopirali) – $sourceDbName = "DG1" $sourceDbServerName = "Poslužitelj1" $sourceDbResourceGroupName = "rg1" SQL kopiju baze podataka (u novi db će biti stvoren) – $copyDbName = "db1_copy" $copyDbServerName = "server2" $copyDbResourceGroupName = "rg2" kopiju baze podataka na isti poslužitelj – novo AzureRmSqlDatabaseCopy - ResourceGroupName $sourceDbResourceGroupName - naziv poslužitelja $sourceDbServerName - ImeBazePodataka $sourceDbName - CopyDatabaseName $copyDbName kopiranje baze podataka na drugi poslužitelj – novo AzureRmSqlDatabaseCopy - ResourceGroupName $sourceDbResourceGroupName - naziv poslužitelja $sourceDbServerName - ImeBazePodataka $sourceDbName - CopyResourceGroupName $copyDbResourceGroupName - CopyServerName $copyDbServerName - CopyDatabaseName $copyDbName kopirajte bazu podataka u skup elastic baze podataka – $poolName = "pool1" Novo AzureRmSqlDatabaseCopy - ResourceGroupName $ sourceDbResourceGroupName - naziv poslužitelja $sourceDbServerName - ImeBazePodataka $sourceDbName - CopyReso | 2016-09-08 |
| 10 | [Stvaranje grupe aplikacija programa elastic bazu podataka s C#](sql-database-elastic-pool-create-csharp.md) |   Console.WriteLine ("poslužitelj vatrozid...");  FirewallRuleGetResponse fwr = CreateOrUpdateFirewallRule (_sqlMgmtClient, _resourceGroupName, _serverName, _firewallRuleName, _startIpAddress, _endIpAddress);  Console.WriteLine ("vatrozid poslužitelja:" + fwr. FirewallRule.Id);  Console.WriteLine ("Elastic skup...");  ElasticPoolCreateOrUpdateResponse epr = CreateOrUpdateElasticDatabasePool (_sqlMgmtClient, _resourceGroupName, _serverName, _poolName, _poolEdition, _poolDtus, _databaseMinDtus, _databaseMaxDtus);  Console.WriteLine ("Elastic skup:" + epr. ElasticPool.Id);  Console.WriteLine("Database...");  DatabaseCreateOrUpdateResponse dbr = CreateOrUpdateDatabase (_sqlMgmtClient, _resourceGroupName, _serverName, _databaseName, _poolName);  Console.WriteLine ("baza podataka:" + dbr. Database.Id);  Console.WriteLine ("pritisnite bilo koju tipku da biste nastavili...");  Console.ReadKey();  } statične ResourceGroup CreateOrUpdateResourceGroup(ResourceManagementClient resourceMgmtClient, string subscriptionId, string resourceGroupNa | 2016 09 14 |
| 11 | [Skriptu PowerShell za označavanje baze podataka prikladna za grupe aplikacija programa elastic baze podataka](sql-database-elastic-pool-database-assessment-powershell.md) | Kandidata **za Elastic skup baze podataka, ne možemo izuzimanje matrice i nijedna baza podataka koji se već nalaze u zbirci web. Možete dodati više baza podataka na popis isključenih ispod prema potrebi** $ListOfDBs = Get-AzureRmSqlDatabase - naziv poslužitelja $servername. Split('.') 0 - ResourceGroupName $ResourceGroupName / gdje objekt {$_. ImeBazePodataka - notin ("glavni") - a $_. CurrentServiceLevelObjectiveName - notin ("ElasticPool")- a $_. CurrentServiceObjectiveName - notin ("ElasticPool")} $outputConnectionString = "izvora podataka = $outputServerName; integriranu sigurnost = false; početnog kataloga = $outputdatabaseName; Korisnički Id = $outputDBUsername; Lozinka = $outputDBpassword "$destinationTableName ="resource_stats_output" **Stvaranje tablice u bazi podataka za izlaz za zbirku metriku** $sql =" ako ključnih riječi NOT EXISTS (odaberite * iz sys.objects object_id gdje = OBJECT_ID(N'$($destinationTableName)'), a zatim upišite u (N'U ")) $($destinationTableName) početak stvaranja tablice (database_name varchar(128), a sporim varchar(20), end_time datetime, avg_cpu plutati, Prosj_ | 2016 09 29 |
| 12 | [Baze podataka SQL Praktični vodič: Stvaranje baze podataka SQL u minutama pomoću portala za Azure](sql-database-get-started.md) |   ! Nove baze podataka sql 1 (. / media/sql-database-get-started/sql-database-new-database-1.png) 3. Kliknite **SQL baze podataka** da biste otvorili plohu SQL baze podataka. Sadržaj na ovom plohu razlikuje se ovisno o broju pretplate i svoje postojeće objekte (primjerice postojeći poslužitelji).  ! Nove baze podataka sql 2 (. / media/sql-database-get-started/sql-database-new-database-2.png) 4. U tekstni okvir **Naziv baze podataka** Navedite naziv prve baze podataka – kao što su "Moje bazu podataka". Zelena kvačica upućuje na to koju ste naveli valjani naziv.  ! Nove baze podataka sql 3 (. / media/sql-database-get-started/sql-database-new-database-3.png) 5. Ako imate više pretplata, odaberite pretplatu. 6. **Grupa resursa**, kliknite **Stvori novi** i navedite naziv za prvu grupu resursa – kao što su "Moje--grupa resursa". Zelena kvačica upućuje na to koju ste naveli valjani naziv.  ! Nove baze podataka sql 4 (. / media/sql-database-get-started/sql-database-new-database-4.png) 7. U odjeljku **Odaberite izvor* | 2016-09-08 |
| 13 | [Pokušajte SQL baza podataka: Korištenje C# za stvaranje baze podataka SQL biblioteku baze podataka SQL za .NET](sql-database-get-started-csharp.md) | **Stvorite glavni pristupa resursima servisa** Sljedeću skriptu komponente PowerShell stvara aplikacije Active Directory (AD) i glavnicu usluge koje je potrebna za provjeru autentičnosti naša aplikacija za C. Skripta proizvodi vrijednosti moramo prethodni primjer C. Detaljne informacije potražite u članku korištenje ljuske PowerShell Azure da biste stvorili glavni servisa za pristup resursima (.. / resursa – grupa-autentičnost-servis-principal.md).  Prijavite se u Azure.  Dodavanje AzureRmAccount ako imate više pretplata, uklonite i postavljen je na pretplatu koju želite raditi.  $subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}" skup AzureRmContext - SubscriptionId $subscriptionId unesite sljedeće vrijednosti za novu aplikciju AAD.  $appName je zaslonsko ime za aplikaciju, mora biti jedinstvena u direktoriju.  $uri moraju biti stvarni uri.  $secret je lozinka stvarate.  $appName = "{-naziv aplikacije}" $uri = "http://{app-name}" $secret = "{aplikacije-lozinku}" Stvaranje AAD aplikacije $azureAdApplication = novo AzureRmADApp | 2016-09-23 |
| 14 | [Upravljanje baze podataka SQL Azure pomoću portala za Azure](sql-database-manage-portal.md) | Da biste pogledali ili ažuriranje postavki baze podataka, kliknite željenu postavku plohu baze podataka SQL:! Postavke baze podataka SQL (. / media/sql-database-manage-portal/settings.png) **kako pronaći naziv potpuno kvalificiran poslužitelja baze podataka SQL?** Da biste vidjeli naziv poslužitelja baze podataka, kliknite **Pregled** plohu **SQL baze podataka** i Imajte na umu i naziva poslužitelja:! Postavke baze podataka SQL (. / media/sql-database-manage-portal/server-name.png) **kako upravljati pravila vatrozida za kontrolu pristupa na SQL server i baze podataka?** Za prikaz, stvaranje ili ažuriranje pravila vatrozida, kliknite **Postavljanje poslužitelja vatrozid** plohu **SQL baze podataka** . Detalje potražite u članku konfiguriranje pravilo vatrozid razini poslužitelja baze podataka SQL Azure pomoću portala za Azure (sql-baze podataka-konfiguracija-vatrozid – settings.md). ! Pravila vatrozida (. / media/sql-database-manage-portal/sql-database-firewall.png) **kako promijeniti Moje SQL baze podataka usluge sloju ili performanse razine?** Da biste ažurirali sloju ili performanse razine servisa SQL baze podataka, kliknite ** određivanje cijena sloju (s | 2016-09-20 |





## <a name="get-started"></a>Početak rada

| &nbsp; | Naslov | Opis |
| --: | :-- | :-- |
| 15 | [Sastavlja više klijentske aplikacije s bazom podataka Azure SQL s odvajanja i učinkovitosti](sql-database-build-multi-tenant-apps.md) | Saznajte kako SQL baze podataka sastavlja više klijentske aplikacije |
| 16 | [Povezivanje s bazom podataka SQL s SQL Server Management Studio i izvršavanje primjer T SQL upita](sql-database-connect-query-ssms.md) | Saznajte kako se povezati s bazom podataka SQL Azure pomoću SQL Server Management Studio (SSMS). Izvedite primjer upita pomoću Transact-SQL (T-SQL). |
| 17 | [Povezivanje s bazom podataka SQL pomoću .NET (C#)](sql-database-develop-dotnet-simple.md) | Korištenje uzorak koda u ovom brzi izgradnje Moderna aplikaciju s C# i sigurnosno prema naprednih relacijske baze podataka u oblaku s bazom podataka SQL Azure. |
| 18 | [Stvaranje nove grupe aplikacija elastic baze podataka pomoću portala za Azure](sql-database-elastic-pool-create-portal.md) | Kako dodati grupe aplikacija skalabilni elastic baze podataka u konfiguraciji baze podataka SQL radi jednostavnijeg Administracija i resursa zajedničko korištenje u mnoge baze podataka. |
| 19 | [Istražite vodiči za baze podataka Azure SQL](sql-database-explore-tutorials.md) | Dodatne informacije o baze podataka SQL značajke i mogućnosti |
| 20 | [Baze podataka SQL Praktični vodič: Stvaranje baze podataka SQL u minutama pomoću portala za Azure](sql-database-get-started.md) | Saznajte kako postaviti logičke poslužitelj baze podataka SQL, pravila vatrozida poslužitelj baze podataka SQL i ogledne podatke. Osim toga, upute za povezivanje s klijentskim alatima, konfiguriranje korisnika i postavljanje pravila vatrozida baze podataka. |
| 21 | [Baze podataka Azure SQL Secures i štiti](sql-database-helps-secures-and-protects.md) | Saznajte kako SQL baze podataka pridonosi sigurne i zaštititi |
| 22 | [Pratit će baze podataka Azure SQL &amp; prilagođavati im](sql-database-learn-and-adapt.md) | Saznajte kako SQL baze podataka Pratit će te prilagođavati im |
| 23 | [Odaberite oblak mogućnost SQL Server: baze podataka SQL Azure (PaaS) ili SQL Server na Azure VMs (IaaS)](sql-database-paas-vs-sql-server-iaas.md) | Saznajte koju mogućnost SQL Server oblaka odgovara aplikacije: baze podataka SQL Azure (PaaS) ili SQL Server u oblaku na virtualnim strojevima sa sustavom Azure. |
| 24 | [Azure ljestvice SQL baze podataka u hodu](sql-database-scale-on-the-fly.md) | Saznajte kako mijenja veličinu SQL baze podataka u hodu |
| 25 | [Istražite Azure SQL baza podataka rješenja brzog pokretanja](sql-database-solution-quick-starts.md) | Dodatne informacije o rješenja Azure SQL baze podataka |
| 26 | [Baze podataka Azure SQL funkcionira u svom okruženju](sql-database-works-in-your-environment.md) | Saznajte kako SQL baze podataka pridonosi, secures i štiti |



## <a name="build-an-app"></a>Stvaranje aplikacije

| &nbsp; | Naslov | Opis |
| --: | :-- | :-- |
| 27 | [Otklanjanje poteškoća s, dijagnosticiranje i spriječili SQL vezu pogreške i tranzitne pogreške za baze podataka SQL](sql-database-connectivity-issues.md) | Upute za otklanjanje poteškoća s dijagnosticiranje i spriječili pogreška SQL veze ili tranzitne pogrešaka u bazi podataka SQL Azure.  |
| 28 | [Povezivanje s bazom podataka SQL s Visual Studio](sql-database-connect-query.md) | Napišite program u C# za slanje upita i povezivanje s bazom podataka SQL. Informacije o IP adrese, nizove veze, sigurna prijava i besplatne Visual Studio. |
| 29 | [Priključci izvan 1433 za ADO.NET 4,5 i V12 baze podataka SQL](sql-database-develop-direct-route-ports-adonet-v12.md) | Klijent veze iz ADO.NET V12 za baze podataka SQL Azure ponekad zaobići proxy poslužitelj i interakciju izravno s bazom podataka. Priključci osim 1433 postaju važna. |
| 30 | [Kodovi pogrešaka SQL za baze podataka SQL klijentske aplikacije: pogreška veze i ostalih problema za baze podataka](sql-database-develop-error-messages.md) | Saznajte više o šifre pogreške SQL baze podataka SQL klijentske aplikacije, kao što su uobičajenih pogrešaka u bazi podataka povezivanja, poteškoća kopiju baze podataka i općenite pogreške.  |
| 31 | [Povezivanje s bazom podataka SQL pomoću i u sustavu Windows](sql-database-develop-php-simple.md) | Predstavlja program za uzorak i koje se povezuje s bazom podataka SQL Azure Windows klijenta i navode veze na komponente potreban softver potreban za klijent. |
| 32 | [Pokušajte SQL baza podataka: Korištenje C# za stvaranje baze podataka SQL biblioteku baze podataka SQL za .NET](sql-database-get-started-csharp.md) | Pokušajte SQL baze podataka za razvoj aplikacija za SQL i C# i stvaranje baze podataka SQL Azure s C# pomoću SQL baza podataka biblioteke za .NET. |
| 33 | [Uvod u značajke JSON u bazi podataka SQL Azure](sql-database-json-features.md) | Baze podataka SQL Azure omogućuje rastavljanju, upita i oblikovanje podataka JavaScript objekt notaciju (JSON) notacijom. |
| 34 | [Rješavanje problema s povezivanjem s bazom podataka SQL Azure](sql-database-troubleshoot-common-connection-issues.md) | Koraci za prepoznavanje i rješavanje uobičajenih pogrešaka veze za baze podataka SQL Azure. |
| 35 | [Pogreška "Baze podataka na poslužitelju nije dostupan" prilikom povezivanja s bazom podataka sql](sql-database-troubleshoot-connection.md) | Otklanjanje poteškoća s baze podataka na poslužitelj nije dostupan pogreške kada se aplikacija poveže s bazom podataka SQL. |



## <a name="develop"></a>Razvoj

| &nbsp; | Naslov | Opis |
| --: | :-- | :-- |
| 36 | [Početak tražene vrijednosti za provjeru autentičnosti aplikacije da biste pristupili SQL baze podataka iz koda](sql-database-client-id-keys.md) | Stvorite glavni servisa za pristup SQL baze podataka iz koda. |
| 37 | [Povezivanje s bazom podataka SQL pomoću Java JDBC u sustavu Windows](sql-database-develop-java-simple.md) | Prikazuje uzorak kod Java možete koristiti da biste se povezali s bazom podataka SQL Azure. Uzorak koristi JDBC te će se pokrenuti na klijentskom računalu sa sustavom Windows. |
| 38 | [Povezivanje s bazom podataka SQL pomoću Node.js](sql-database-develop-nodejs-simple.md) | Prikazuje uzorak Node.js kod možete koristiti da biste se povezali s bazom podataka SQL Azure. |
| 39 | [Pregled razvoj baze podataka za SQL](sql-database-develop-overview.md) | Informirajte se o dostupna povezivanje biblioteke i najbolje prakse za aplikacije za povezivanje s bazom podataka SQL. |
| 40 | [Povezivanje s bazom podataka SQL pomoću Python](sql-database-develop-python-simple.md) | Prikazuje uzorak koda Python, možete koristiti da biste se povezali s bazom podataka SQL Azure. |
| 41 | [Povezivanje s bazom podataka SQL pomoću Ruby](sql-database-develop-ruby-simple.md) | Dajte uzorka Ruby kod možete pokrenuti da biste se povezali s bazom podataka SQL Azure. |
| 42 | [Uvod u vremenski tablica u bazi podataka Azure SQL](sql-database-temporal-tables.md) | Saznajte kako započeti s korištenjem vremenski tablica u bazi podataka SQL Azure. |



## <a name="performance-and-scale"></a>Performanse i promjena veličine

| &nbsp; | Naslov | Opis |
| --: | :-- | :-- |
| 43 | [Savjetnik za baze podataka SQL](sql-database-advisor.md) | Savjetnik za baze podataka SQL Azure nudi preporuke za postojećih SQL baza podataka koje možete poboljšati performanse trenutni upit. |
| 44 | [SQL Savjetnik za baze podataka pomoću portala za Azure](sql-database-advisor-portal.md) | Koristite Savjetnik za baze podataka SQL Azure na portalu za Azure da pregledavaju i implementirati preporuke za postojećih SQL baza podataka koje možete poboljšati performanse trenutni upit. |
| 45 | [Pregled usporednih za Azure SQL baze podataka](sql-database-benchmark-overview.md) | U ovoj se temi opisuju usporednih baze podataka SQL Azure koristi u mjerenje performanse baze podataka SQL Azure. |
| 46 | [Kada se skup elastic baze podataka želite koristiti?](sql-database-elastic-pool-guidance.md) | Na skupna elastic baze podataka nije skup dostupnih resursa koje zajednički koriste grupe elastic baze podataka. Ovaj dokument sadrži upute da biste lakše procijenite prikladnosti korištenja programa skup elastic baze podataka za grupu baze podataka. |
| 47 | [Početak rada s alatima Elastic baze podataka](sql-database-elastic-scale-get-started.md) | Osnovni objašnjenje elastic baze podataka alata značajka servisa Azure SQL baze podataka, uključujući jednostavno da biste pokrenuli aplikaciju uzorka. |
| 48 | [Najčešća pitanja vezana uz baze podataka SQL](sql-database-faq.md) | Odgovori na uobičajena pitanja kupaca o oblaka baze podataka i baze podataka SQL Azure, sustav upravljanja relacijske baze podataka tvrtke Microsoft (RDBMS) i baze podataka zatražite od kao servis u oblaku. |
| 49 | [Uvod u memoriji (pretpregled) u SQL baze podataka](sql-database-in-memory.md) | SQL memorije tehnologije znatno poboljšati performanse transakcijskih i radnih opterećenja analize. Saznajte kako da iskoristite prednost tih tehnologija. |
| 50 | [Korištenje memorije OLTP (pretpregled) da biste poboljšali performanse računala u SQL baze podataka](sql-database-in-memory-oltp-migration.md) | Da biste poboljšali performanse transakcijskih u postojećoj bazi podataka SQL adopt OLTP u memoriji. |
| 51 | [Spremanje u memoriji OLTP monitora](sql-database-in-memory-oltp-monitoring.md) | Procjena i nadzor prostora za pohranu u memoriji za XTP koriste, kapaciteta. Razrješavanje pogreške kapaciteta 41823 |
| 52 | [Radi pohrane upita u bazi podataka Azure SQL](sql-database-operate-query-store.md) | Saznajte kako raditi u spremište upita u bazi podataka SQL Azure |
| 53 | [Uvid performanse baze podataka za SQL](sql-database-performance.md) | Baza podataka SQL Azure nudi alate performanse da biste lakše prepoznali područja koja mogu poboljšati performanse trenutni upit. |
| 54 | [Azure SQL baze podataka i performanse za jednu baze podataka](sql-database-performance-guidance.md) | U ovom se članku pojednostavljuju određivanje koji sloju servisa da biste odabrali za svoju aplikaciju. Preporučuje se i načina za ugađanje aplikacije da biste na najbolji s bazom podataka SQL Azure. |
| 55 | [Azure SQL baza podataka upita performanse uvida](sql-database-query-performance.md) | Praćenje performansi upita služi za identifikaciju Većina upita procesora koristi za baze podataka SQL Azure. |
| 56 | [Promjena servisa sloju i performanse razine (cijene sloju) SQL baze podataka](sql-database-scale-up.md) | Promjena sloju servisa i razinom performansi baze podataka Azure SQL pokazuje kako se promijenila veličina baze podataka sustava SQL prema gore ili dolje. Promjena cijene sloj baze podataka Azure SQL. |
| 57 | [Praćenje performansi baze podataka u bazi podataka SQL Azure](sql-database-single-database-monitor.md) | Dodatne informacije o mogućnostima za nadzor bazu podataka s Azure Alati i dinamični Upravljanje prikazima. |
| 58 | [Savjeti za ugađanje performansi SQL baze podataka](sql-database-troubleshoot-performance.md) | Savjeti za ugađanju u bazi podataka SQL Azure putem procjenu i poboljšanja performansi. |
| 59 | [Kako poboljšati performanse aplikacije SQL baze podataka pomoću grupnog slanja promjena](sql-database-use-batching-to-improve-performance.md) | U temi dokazuje te batching postupaka baze podataka znatno imroves brzine i skalabilnost aplikacija za baze podataka SQL Azure. Iako ove tehnike batching raditi za sve baze podataka SQL Server, fokus u članku nalazi se na Azure. |



## <a name="tools-for-scaling-out"></a>Alati za skaliranje izgleda

| &nbsp; | Naslov | Opis |
| --: | :-- | :-- |
| 60 | [Dizajniranje obrazaca za složene SaaS aplikacije i baze podataka SQL Azure](sql-database-design-patterns-multi-tenancy-saas-applications.md) | U ovom se članku govori o preduvjeti i uobičajene uzorke podataka arhitektura aplikacija u okruženju oblaka treba uzeti u obzir složene baze podataka i različite tradeoffs povezan te uzorcima. Ga i objašnjava kako baze podataka SQL Azure, s elastic grupe i Alati za elastic pomoći adresa te uvjete u ne-narušena pouzdanost način. |
| 61 | [Migracija postojeće baze podataka da biste skala Izlaz](sql-database-elastic-convert-to-use-elastic-tools.md) | Pretvaranje sharded baza podataka pomoću alata elastic baze podataka stvaranjem na shard Upravitelj kartu |
| 62 | [Stvaranje baze podataka skalabilni oblaka](sql-database-elastic-database-client-library.md) | Sastavljanje skalabilni .NET baze podataka aplikacije s bibliotekom klijent elastic baze podataka |
| 63 | [Mjerača performansi za Upravitelj shard karte](sql-database-elastic-database-perf-counters.md) | ShardMapManager predmet i podatke o njima ovisne usmjeravanje mjerača performansi |
| 64 | [Dodavanje shard pomoću alata za Elastic baze podataka](sql-database-elastic-scale-add-a-shard.md) | Kako koristiti API-ji Elastic mjerilo da biste dodali novi shards na shard postavite. |
| 65 | [Uvođenje servisa Podjela cirkularnog pisma](sql-database-elastic-scale-configure-deploy-split-and-merge.md) | Podjela i spajanje s alatima elastic baze podataka |
| 66 | [Podaci o njima ovisne usmjeravanja](sql-database-elastic-scale-data-dependent-routing.md) | Upute za podataka ovisne usmjeravanje, značajka elastic baze podataka za baze podataka SQL Azure pomoću klase ShardMapManager u aplikacijama za .NET |
| 67 | [Elastic Alati baze podataka najčešća Pitanja](sql-database-elastic-scale-faq.md) | Najčešća pitanja o Elastic promjena veličine baze podataka Azure SQL. |
| 68 | [Elastic Pojmovnik Alati baze podataka](sql-database-elastic-scale-glossary.md) | Objašnjenje izraze koji se koriste za alate elastic baze podataka |
| 69 | [Promjena veličine odgovor s bazom podataka SQL Azure](sql-database-elastic-scale-introduction.md) | Softver kao razvojnim inženjerima servisa (SaaS) možete jednostavno stvara elastic, skalabilni baze podataka u oblak pomoću tih alata |
| 70 | [Vjerodajnica korištenih za pristup biblioteci klijent Elastic baze podataka](sql-database-elastic-scale-manage-credentials.md) | Kako postaviti pravilnu razinu vjerodajnice, administrator da biste samo za čitanje, za aplikacije elastic baze podataka |
| 71 | [Više shard upita](sql-database-elastic-scale-multishard-querying.md) | Pokretanje upita preko shards pomoću klijentska biblioteka elastic baze podataka. |
| 72 | [Premještanje podataka iz oblaka skalirana iz baze podataka](sql-database-elastic-scale-overview-split-and-merge.md) | U članku se objašnjava kako rukovati shards i premještanje podataka putem samostalnih servis bazi elastic API-ji. |
| 73 | [Promjena veličine izgleda baze podataka s upraviteljem shard karte](sql-database-elastic-scale-shard-map-management.md) | Kako koristiti ShardMapManager, biblioteka klijentski elastic baze podataka |
| 74 | [Konfiguracija sigurnosti Podijeli cirkularnog pisma](sql-database-elastic-scale-split-merge-security-configuration.md) | Postavljanje x409 certifikata za šifriranje |
| 75 | [Nadogradnja aplikacije programa da biste koristili najnovije biblioteku klijent elastic baze podataka](sql-database-elastic-scale-upgrade-client-library.md) | Nadogradnja aplikacije i biblioteka pomoću Nuget |
| 76 | [Elastic biblioteka klijentski baze podataka s entitet Framework](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) | Korištenje biblioteka klijentski Elastic baze podataka i entitet Framework kodiranje baze podataka |
| 77 | [Biblioteka klijentski elastic baze podataka pomoću Dapper](sql-database-elastic-scale-working-with-dapper.md) | Biblioteka klijentski elastic baze podataka pomoću Dapper. |
| 78 | [Više klijentske aplikacije s Alati elastic baze podataka i sigurnost na razini retka](sql-database-elastic-tools-multi-tenant-row-level-security.md) | Saznajte kako koristiti elastic Alati baze podataka i sigurnost na razini retka da biste sastavili aplikacije s iznimno skalabilni podataka sloju baze podataka SQL Azure koji podržava više klijentu shards. |



## <a name="elastic-jobs"></a>Elastic poslova

| &nbsp; | Naslov | Opis |
| --: | :-- | :-- |
| 79 | [Stvaranje i upravljanje skaliranu izvan baze podataka SQL Azure pomoću elastic poslove (pretpregled)](sql-database-elastic-jobs-create-and-manage.md) | Voditi kroz stvaranje i upravljanje za posao elastic baze podataka. |
| 80 | [Početak rada sa zadacima Elastic baze podataka](sql-database-elastic-jobs-getting-started.md) | kako koristiti poslove elastic baze podataka |
| 81 | [Upravljanje bazama podataka skalirana iz oblaka](sql-database-elastic-jobs-overview.md) | Prikazuje usluga zadatka elastic baze podataka |
| 82 | [Stvaranje i upravljanje zadacima elastic baze podataka za SQL baze podataka pomoću komponente PowerShell (pretpregled)](sql-database-elastic-jobs-powershell.md) | PowerShell koji se koriste za upravljanje bazom podataka SQL Azure grupe |
| 83 | [Pregled zadataka za instaliranje Elastic baze podataka](sql-database-elastic-jobs-service-installation.md) | Prođite kroz instalacije značajke elastic posao. |
| 84 | [Deinstalacija Elastic komponente baze podataka poslova](sql-database-elastic-jobs-uninstall.md) | Kako deinstalirati elastic alata baze podataka zadataka |



## <a name="elastic-pools"></a>Elastic grupe

| &nbsp; | Naslov | Opis |
| --: | :-- | :-- |
| 85 | [Što je Azure elastic skup?](sql-database-elastic-pool.md) | Upravljanje stotine ili tisuće baze podataka koje koriste u grupu. Jedan cijenu za skup jedinice performanse mogu distribuirati pokazivač na resurse. Premještanje baze podataka ili smanjili pri će. |
| 86 | [Stvaranje grupe aplikacija programa elastic bazu podataka s C#](sql-database-elastic-pool-create-csharp.md) | Koristite C# baze podataka razvojnih tehnika stvaranja grupe aplikacija skalabilni elastic baze podataka u bazi podataka SQL Azure tako da možete omogućiti zajedničko korištenje resursa preko mnoge baze podataka. |
| 87 | [Stvaranje nove grupe aplikacija elastic baze podataka pomoću komponente PowerShell](sql-database-elastic-pool-create-powershell.md) | Saznajte kako pomoću ljuske PowerShell skaliranje iz baze podataka SQL Azure resursi stvaranjem skup skalabilni elastic baze podataka da biste upravljali više baza podataka. |
| 88 | [Skriptu PowerShell za označavanje baze podataka prikladna za grupe aplikacija programa elastic baze podataka](sql-database-elastic-pool-database-assessment-powershell.md) | Na skupna elastic baze podataka nije skup dostupnih resursa koje zajednički koriste grupe elastic baze podataka. Ovaj dokument sadrži skriptu Powershell pomoći u procjeni prikladnosti korištenja programa skup elastic baze podataka za grupu baze podataka. |
| 89 | [Nadzor i upravljanje elastic baze podataka grupe aplikacija uz C#](sql-database-elastic-pool-manage-csharp.md) | Upravljanje grupe aplikacija programa za baze podataka SQL Azure elastic baze podataka pomoću C# baze podataka razvojnih tehnika. |
| 90 | [Nadzor i upravljanje za skup elastic baze podataka pomoću portala za Azure](sql-database-elastic-pool-manage-portal.md) | Saznajte kako pomoću portala za Azure i ugrađene obavještavanje SQL baze podataka na upravljanje, praćenje i desno veličina skalabilni elastic baze podataka skup optimiziranja performansi baze podataka i upravljanje trošak. |
| 91 | [Nadzor i upravljanje elastic baze podataka grupe aplikacija sa servisom PowerShell](sql-database-elastic-pool-manage-powershell.md) | Saznajte kako pomoću ljuske PowerShell za upravljanje sustava skup elastic baze podataka. |
| 92 | [Nadzor i upravljanje elastic baze podataka grupe aplikacija s Transact-SQL](sql-database-elastic-pool-manage-tsql.md) | T-SQL možete koristiti za stvaranje baze podataka Azure SQL u elastic grupe aplikacija. Ili koristite T SQL da biste premjestili na datbase i grupe. |
| 93 | [Naplata skup elastic baze podataka i informacije o cijenama](sql-database-elastic-pool-price.md) | Informacije o cijenama specifične za grupe elastic baze podataka. |



## <a name="elastic-query"></a>Elastic upita

| &nbsp; | Naslov | Opis |
| --: | :-- | :-- |
| 94 | [Prijavite se putem oblaka skalirana iz baze podataka (pretpregled)](sql-database-elastic-query-getting-started.md) | Kako se koristi Unakrsni upiti baze podataka za baze podataka |
| 95 | [Početak rada s upitima izdvojiti bazu podataka (okomito particija) (pretpregled)](sql-database-elastic-query-getting-started-vertical.md) | kako koristiti elastic upita baze podataka s okomito particije baze podataka |
| 96 | [Izvješćivanje o pogreškama preko oblak skalirana iz baze podataka (pretpregled)](sql-database-elastic-query-horizontal-partitioning.md) | kako postaviti elastic upita putem vodoravni particije |
| 97 | [Azure pregled (pretpregled) baze podataka SQL elastic baze podataka upita](sql-database-elastic-query-overview.md) | Pregled značajki elastic upita |
| 98 | [Slanje upita za baze podataka oblak s različitim shema (pretpregled)](sql-database-elastic-query-vertical-partitioning.md) | upute za postavljanje upita unakrsno-baze podataka putem okomiti particije |



## <a name="elastic-transaction"></a>Elastic transakcije

| &nbsp; | Naslov | Opis |
| --: | :-- | :-- |
| 99 | [Raspodijeljeno transakcije putem oblaka baze podataka](sql-database-elastic-transactions-overview.md) | Pregled Elastic transakcije baze podataka Azure SQL baze podataka |



## <a name="backup-and-recovery"></a>Sigurnosno kopiranje i vraćanje

| &nbsp; | Naslov | Opis |
| --: | :-- | :-- |
| 100 | [Sigurnosno kopiranje baze podataka SQL](sql-database-automated-backups.md) | Informirajte se o baze podataka SQL ugrađene sigurnosne kopije baze podataka koji omogućuju vam da biste ponovno baze podataka SQL Azure prethodnog točku vremena ili kopiranje baze podataka u novu bazu podataka u zemljopisnom području (do 35 dana). |
| 101 | [Pregled poslovanje s bazom podataka SQL Azure](sql-database-business-continuity.md) | Saznajte kako baze podataka SQL Azure podržava poslovanje u oblak i oporavak baze podataka te pomaže zadržati zaštita njihove privatnosti ovise ključnih oblaka aplikacija. |
| 102 | [Kako vratiti jednu tablicu iz programa sigurnosne kopije baze podataka SQL Azure](sql-database-cloud-migrate-restore-single-table-azure-backup.md) | Saznajte kako vratiti jednu tablicu iz sigurnosne kopije baze podataka SQL Azure. |
| 103 | [Dizajniranje aplikacije za oblak Izrada oporavak u bazi podataka SQL pomoću aktivni replikacije zemlj.](sql-database-designing-cloud-solutions-for-disaster-recovery.md) | Saznajte kako dizajniranje rješenja za oporavak Izrada oblaku tvrtke continuity planiranja pomoću zemlj replikacije za sigurnosno kopiranje podataka aplikacije s bazom podataka SQL Azure. |
| 104 | [Vraćanje baze podataka SQL Azure ili prebacivanje sekundarni](sql-database-disaster-recovery.md) | Saznajte kako oporaviti baze podataka iz otkazivanja s Azure SQL replikacije baze podataka Active zemlj. – i mogućnosti zemlj vraćanja ili nedostupnosti regionalnog podatkovnog centra. |
| 105 | [Izvođenje Izrada oporavak analize](sql-database-disaster-recovery-drills.md) | Dodatne upute i najbolje prakse za korištenje baze podataka SQL Azure za izvođenje Izrada oporavak bušilice koji će pomoći zadržati svoje zaštita njihove privatnosti ovise ključnih poslovnih aplikacija prebacuju pogrešaka i kvarove. |
| 106 | [Izrada oporavak Strategije za aplikacije koje koriste Elastic grupe aplikacija za baze podataka SQL](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md) | Saznajte kako dizajnirati rješenje oblaka za oporavak Izrada tako da odaberete uzorak desnom prebacivanje. |
| 107 | [Korištenje klase RecoveryManager za rješavanje problema s shard kartu](sql-database-elastic-database-recovery-manager.md) | Rješavanje problema s kartama shard pomoću klase RecoveryManager |
| 108 | [Uvoz datoteke BACPAC za stvaranje baze podataka Azure SQL](sql-database-import.md) | Stvaranje baze podataka Azure SQL uvozom postojeću datoteku BACPAC. |
| 109 | [Vraćanje baze podataka SQL Azure prethodnog točku u vremenu pomoću portala za Azure](sql-database-point-in-time-restore-portal.md) | Vraćanje baze podataka SQL Azure prethodnog točku u vremenu. |
| 110 | [Vraćanje baze podataka SQL Azure prethodnog točku u vremenu pomoću komponente PowerShell](sql-database-point-in-time-restore-powershell.md) | Vraćanje baze podataka SQL Azure prethodnog točku u vremenu |
| 112 | [Oporavak baze podataka Azure SQL pomoću sigurnosne kopije automatiziranog baze podataka](sql-database-recovery-using-backups.md) | Saznajte više o Vrati točke u vrijeme koje omogućuje vam da biste vratili baze podataka SQL Azure prethodnog točke u vremenu (do 35 dana). |
| 113 | [Vraćanje izbrisane Azure SQL baze podataka pomoću portala za Azure](sql-database-restore-deleted-database-portal.md) | Vraćanje izbrisane Azure SQL baze podataka (Portal za Azure). |
| 114 | [Vraćanje izbrisane baze podataka SQL Azure pomoću komponente PowerShell](sql-database-restore-deleted-database-powershell.md) | Vraćanje izbrisane Azure SQL baze podataka (PowerShell). |
| 115 | [Vraćanje baze podataka na prethodnu točku u vremenu, vraćanje izbrisanih baze podataka ili oporavak nedostupnosti za centar za podataka](sql-database-troubleshoot-backup-and-restore.md) | Saznajte kako oporaviti oblaka baze podataka iz pogreške i kvarove pomoću sigurnosnog kopiranja i replike u bazi podataka SQL Azure. |



## <a name="migrate"></a>Migriranje

| &nbsp; | Naslov | Opis |
| --: | :-- | :-- |
| 116 | [Izvoz baze podataka SQL Server BACPAC datoteku pomoću SqlPackage](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md) | Microsoft Azure SQL baze podataka, Migracija baze podataka izvezite baze podataka, izvoz datoteke BACPAC, sqlpackage |
| 117 | [Izvoz baze podataka SQL Server BACPAC datoteku pomoću SQL Server Management Studio](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md) | Microsoft Azure SQL baze podataka, Migracija baze podataka izvezite baze podataka, izvoz datoteke BACPAC, čarobnjak za izvoz podataka aplikacije u sloju |
| 118 | [Uvoz iz BACPAC datoteku pomoću SqlPackage u SQL baze podataka](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md) | Microsoft Azure SQL baze podataka, Migracija baze podataka, Uvoz baze podataka, Uvoz datoteke BACPAC, sqlpackage |
| 119 | [Uvoz iz BACPAC u SQL baze podataka pomoću SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md) | Microsoft Azure SQL baze podataka, baza podataka implementacije, Migracija baze podataka, Uvoz baze podataka, izvoz baze podataka, a zatim Čarobnjak za migraciju |
| 120 | [Migracija baze podataka SQL Server u SQL baze podataka pomoću implementacija baze podataka radi Čarobnjak za baze podataka Microsoft Azure](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md) | Microsoft Azure SQL baze podataka, Migracija baze podataka, čarobnjak za baze podataka Microsoft Azure |
| 121 | [Migracija baze podataka SQL Server s bazom podataka SQL Azure pomoću transakcijskih replikacije](sql-database-cloud-migrate-compatible-using-transactional-replication.md) | Microsoft Azure SQL baze podataka, Migracija baze podataka, Uvoz baze podataka, transakcijskih replikacije |
| 122 | [Određivanje kompatibilnosti SQL baze podataka pomoću SqlPackage.exe](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md) | Microsoft Azure SQL baze podataka, web-mjesto Migracija baze podataka, u okvir za kompatibilnost SQL baze podataka, SqlPackage |
| 123 | [Da biste odredili baze podataka SQL kompatibilnosti prije migracije s bazom podataka SQL Azure pomoću SQL Server Management Studio](sql-database-cloud-migrate-determine-compatibility-ssms.md) | Microsoft Azure SQL baze podataka, web-mjesto Migracija baze podataka, u okvir za kompatibilnost SQL baze podataka, podataka sloju aplikacije čarobnjaka za izvoz |
| 124 | [Korištenje čarobnjaka za migraciju SQL Azure SQL Server riješiti probleme s kompatibilnošću baze podataka prije migracije s bazom podataka SQL Azure](sql-database-cloud-migrate-fix-compatibility-issues.md) | Microsoft Azure SQL baze podataka, Migracija baze podataka, kompatibilnost, čarobnjak za migraciju SQL Azure |
| 125 | [Migracija baze podataka SQL Server s bazom podataka Azure SQL pomoću SQL Server Data Tools za Visual Studio](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md) | Web-mjesto Microsoft Azure SQL baze podataka, Migracija baze podataka, kompatibilnost, u okvir za SQL Azure Čarobnjak za migraciju, SSDT |
| 126 | [Rješavanje problema kompatibilnosti baze podataka za SQL Server pomoću SQL Server Management Studio prije migracije u SQL baze podataka](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md) | Microsoft Azure SQL baze podataka, Migracija baze podataka, kompatibilnost, čarobnjak za migraciju SQL Azure |
| 127 | [Arhiviranje baze podataka Azure SQL BACPAC datoteku pomoću komponente PowerShell](sql-database-export-powershell.md) | Arhiviranje baze podataka Azure SQL BACPAC datoteku pomoću komponente PowerShell |
| 128 | [Uvoz datoteke BACPAC stvaranje baze podataka Azure SQL pomoću komponente PowerShell](sql-database-import-powershell.md) | Uvoz datoteke BACPAC stvaranje baze podataka Azure SQL pomoću komponente PowerShell |
| 129 | [Učitavanje podataka iz CSV u Azure SQL Data Warehouse (paušalni datoteke)](sql-database-load-from-csv-with-bcp.md) | Za veličinom small podataka koristi bcp da biste uvezli podatke u bazu podataka SQL Azure. |
| 130 | [Prebacivanje baze podataka na poslužitelje, s pretplate, a i Azure](sql-database-troubleshoot-moving-data.md) | Brze korake da biste kopirali, premjestite i Migracija podataka i bazama podataka u bazi podataka SQL Azure. |



## <a name="move-data-in-and-out"></a>Premještanje podataka i smanjivanje

| &nbsp; | Naslov | Opis |
| --: | :-- | :-- |
| 131 | [Migracija baze podataka SQL Server s bazom podataka SQL u oblaku](sql-database-cloud-migrate.md) | Saznajte kako otprilike lokalnog sustava SQL Server Migracija baze podataka s bazom podataka SQL Azure u oblaku. Pomoću alata za migraciju baze podataka za provjeru kompatibilnosti prije migracije baze podataka. |
| 132 | [Kopiranje baze podataka Azure SQL](sql-database-copy.md) | Stvaranje kopije baze podataka Azure SQL |
| 133 | [Arhiviranje baze podataka Azure SQL BACPAC datoteku pomoću portala za Azure](sql-database-export.md) | Arhiviranje baze podataka Azure SQL BACPAC datoteku pomoću portala za Azure |



## <a name="security"></a>Sigurnost

| &nbsp; | Naslov | Opis |
| --: | :-- | :-- |
| 134 | [Povezivanje s bazom podataka SQL ili SQL Data Warehouse pomoću provjere autentičnosti Azure Active Directory](sql-database-aad-authentication.md) | Saznajte kako se povezati s bazom podataka SQL pomoću provjere autentičnosti Azure Active Directory. |
| 135 | [Uvijek šifriranih: Zaštita povjerljive podatke u bazi podataka SQL i pohraniti ključeva za šifriranje u Windows spremište certifikata](sql-database-always-encrypted.md) | Zaštita povjerljive podatke u bazi podataka za SQL u minutama. |
| 136 | [Uvijek šifrirane: Zaštita osjetljivi podaci u bazi podataka SQL i pohranjivati ključeva za šifriranje u sigurnog ključ Azure](sql-database-always-encrypted-azure-key-vault.md) | Zaštita povjerljive podatke u bazi podataka za SQL u minutama. |
| 137 | [Podrška za baze podataka SQL - sa starijim verzijama i IP krajnje promjene za nadzor](sql-database-auditing-and-dynamic-data-masking-downlevel-clients.md) | Saznajte više o podršci za klijente hijerarhijski niži baze podataka SQL-a i IP- a promjene krajnje točke za nadzor. |
| 138 | [Konfigurirati pravila vatrozida razini poslužitelja baze podataka SQL Azure pomoću komponente PowerShell](sql-database-configure-firewall-settings-powershell.md) | Saznajte kako konfigurirati Vatrozid za IP adrese Azure SQL baze podataka programa access. |
| 139 | [Konfigurirati pravila vatrozida za razini poslužitelja za baze podataka SQL Azure pomoću REST API-JA](sql-database-configure-firewall-settings-rest.md) | Saznajte kako konfigurirati Vatrozid za IP adrese Azure SQL baze podataka programa access. |
| 140 | [Konfiguriranje pravila poslužitelja baze podataka – razini i vatrozida za baze podataka SQL Azure pomoću T SQL](sql-database-configure-firewall-settings-tsql.md) | Saznajte kako konfigurirati Vatrozid za IP adrese Azure SQL baze podataka programa access. |
| 141 | [Uvod u SQL baze podataka dinamične podatke Masking (Portal za Azure)](sql-database-dynamic-data-masking-get-started.md) | Upute za početak rada s SQL baza podataka dinamične podatke Masking na portalu za Azure |
| 142 | [Uvod u SQL baze podataka dinamične podatke Masking (klasični Portal Azure)](sql-database-dynamic-data-masking-get-started-portal.md) | Upute za početak rada s SQL baza podataka dinamične podatke Masking na portalu klasični Azure |
| 143 | [Konfigurirati pravila vatrozida baze podataka SQL Azure \- pregled](sql-database-firewall-configure.md) | Saznajte kako konfigurirati Vatrozid za baze podataka SQL poslužitelja baze podataka – razini i vatrozid pravila za upravljanje pristupom. |
| 144 | [Baze podataka SQL Praktični vodič: Stvaranje baze podataka SQL korisničke račune za pristup i upravljanje bazom podataka](sql-database-get-started-security.md) | Saznajte kako stvoriti korisničke račune za pristup i upravljanje bazom podataka. |
| 145 | [Provjera autentičnosti baze podataka SQL i autorizacije: omogućivanju pristupa](sql-database-manage-logins.md) | Informacije o upravljanju sigurnost SQL baze podataka, posebice način upravljanja sigurnošću web-mjesto programa access i u okvir za prijavu baze podataka putem računa glavni razini poslužitelja. |
| 146 | [Upute o sigurnosti baze podataka SQL Azure i ograničenja](sql-database-security-guidelines.md) | Informirajte se o baza podataka Microsoft Azure SQL smjernice i ograničenja vezanih uz sigurnost. |
| 147 | [Početak rada s otkrivanje prijetnju baze podataka za SQL](sql-database-threat-detection-get-started.md) | Upute za početak rada s prijetnju otkrivanje za baze podataka SQL Azure portalu |
| 148 | [Kako izvoditi uobičajene administrativne zadatke kao što je ponovno postavljanje lozinke za administratore u bazi podataka SQL Azure](sql-database-troubleshoot-permissions.md) | U članku se opisuje kako izvoditi uobičajene administrativne zadatke u SQL baze podataka. Na primjer, ponovno postavljanje lozinke za administratore, dodjelu i uklanjanje programa access. |



## <a name="manage-your-database"></a>Upravljanje bazom podataka programa

| &nbsp; | Naslov | Opis |
| --: | :-- | :-- |
| 149 | [Početak rada s nadzor baze podataka SQL](sql-database-auditing-get-started.md) | Početak rada s nadzor baze podataka SQL |
| 150 | [Konfiguriranje pravilo vatrozid razini poslužitelja baze podataka SQL Azure pomoću portala za Azure](sql-database-configure-firewall-settings.md) | Saznajte kako konfigurirati Vatrozid za IP adrese pristup Azure SQL server. |
| 151 | [Kopirajte bazu SQL Azure pomoću portala za Azure](sql-database-copy-portal.md) | Stvaranje kopije baze podataka Azure SQL |
| 152 | [Upravljanje bazama podataka SQL Azure pomoću automatizacije Azure](sql-database-manage-automation.md) | Saznajte kako se servisa Azure Automatizacija može koristiti za upravljanje bazama podataka Azure SQL na razini. |
| 153 | [Pregled: Upravljanje Alati za baze podataka SQL](sql-database-manage-overview.md) | Uspoređuje alata i mogućnosti za upravljanje bazom podataka SQL Azure |
| 154 | [Nadzor baze podataka SQL Azure pomoću dinamične Upravljanje prikazima](sql-database-monitoring-with-dmvs.md) | Saznajte kako prepoznati i dijagnosticiranje uobičajene probleme s performansama pomoću prikaza dinamički upravljanje praćenje baza podataka Microsoft Azure SQL. |
| 155 | [Zaštita baze podataka sustava SQL](sql-database-security.md) | Saznajte više o sigurnosti baze podataka SQL Azure i SQL Server, uključujući razlike u oblak i SQL Server lokalnog kada je riječ o autentičnosti, autorizacije, sigurnost veze, šifriranje i usklađenost. |



## <a name="extended-events"></a>Prošireni događaja

| &nbsp; | Naslov | Opis |
| --: | :-- | :-- |
| 156 | [Kod ciljnu datoteku događaja za prošireni događaje u SQL baze podataka](sql-database-xevent-code-event-file.md) | Nudi PowerShell i Transact-SQL za uzorak two-phase kod koji pokazuje ciljnu datoteku događaja u događaj prošireni na baze podataka SQL Azure. Azure prostora za pohranu je obaveznu komponentu scenarij. |
| 157 | [Pozivanje međuspremnik cilj kod za prošireni događaje u SQL baze podataka](sql-database-xevent-code-ring-buffer.md) | Nudi uzorka Transact-SQL kod koji je stvoren jednostavno i brzo pomoću cilja Nazovi međuspremnika u bazi podataka SQL Azure. |
| 158 | [Prošireni događaja u SQL baze podataka](sql-database-xevent-db-diff-from-svr.md) | U članku se opisuje prošireni događaja (XEvents) u bazi podataka SQL Azure i kako event sesija malo razlikovati od događaj sesije u Microsoft SQL Server. |



## <a name="geo-replication"></a>Replikacija zemlj.

| &nbsp; | Naslov | Opis |
| --: | :-- | :-- |
| 159 | [Pokretanje planiranog ili neplanirano prebacivanje za baze podataka SQL Azure pomoću portala za Azure](sql-database-geo-replication-failover-portal.md) | Pokretanje planiranog ili neplanirano Prebacivanje baze podataka SQL Azure pomoću portala za Azure |
| 160 | [Pokretanje planiranog ili neplanirano Prebacivanje baze podataka SQL Azure pomoću komponente PowerShell](sql-database-geo-replication-failover-powershell.md) | Pokretanje planiranog ili neplanirano Prebacivanje baze podataka SQL Azure pomoću komponente PowerShell |
| 161 | [Pokretanje planiranog ili neplanirano Prebacivanje baze podataka SQL Azure s Transact-SQL](sql-database-geo-replication-failover-transact-sql.md) | Pokretanje planiranog ili neplanirano prebacivanje za baze podataka SQL Azure pomoću Transact-SQL |
| 162 | [Pregled: SQL baza podataka aktivna zemlj. – replikacije](sql-database-geo-replication-overview.md) | Aktivni replikacije zemlj omogućuje postavljanje 4 replike baze podataka u bilo kojem od Azure podatkovnim centrima. |
| 163 | [Konfiguriranje zemlj replikacije baze podataka SQL Azure pomoću portala za Azure](sql-database-geo-replication-portal.md) | Konfiguriranje zemlj replikacije baze podataka SQL Azure pomoću portala za Azure |
| 164 | [Konfiguriranje zemlj replikacije za baze podataka Azure SQL pomoću komponente PowerShell](sql-database-geo-replication-powershell.md) | Konfiguriranje aktivni replikacije zemlj za baze podataka SQL Azure pomoću komponente PowerShell |
| 165 | [Upravljanje bazom podataka SQL Azure sigurnost nakon oporavka Izrada](sql-database-geo-replication-security-config.md) | U ovoj se temi objašnjava sigurnosna pitanja vezana uz za upravljanje sigurnost nakon Vraćanje baze podataka ili je prebacivanje. |
| 166 | [Konfiguriranje zemlj replikacije baze podataka Azure SQL s SQL transakcija](sql-database-geo-replication-transact-sql.md) | Konfiguriranje zemlj replikacije baze podataka SQL Azure pomoću Transact-SQL |
| 167 | [Vraćanje zemlj baze podataka SQL Azure pomoću portala za Azure zemlj suvišnih sigurnosne kopije](sql-database-geo-restore-portal.md) | Zemlj Vraćanje baze podataka SQL Azure iz sigurnosne kopije suvišnih zemlj. (Portal za Azure). |
| 168 | [Vraćanje baze podataka SQL Azure iz sigurnosne kopije suvišnih zemlj pomoću komponente PowerShell](sql-database-geo-restore-powershell.md) | Vraćanje baze podataka SQL Azure u novi poslužitelj iz sigurnosne kopije suvišnih zemlj. |



## <a name="upgrade"></a>Nadogradnja

| &nbsp; | Naslov | Opis |
| --: | :-- | :-- |
| 169 | [Poboljšane performanse upita s kompatibilnošću 130 razina u bazi podataka SQL Azure](sql-database-compatibility-level-query-performance-130.md) | Alati za određivanje razini kompatibilnosti je najbolje za bazu podataka na baze podataka SQL Azure ili Microsoft SQL Server i koraka |
| 170 | [Upravljanje vodoravnim nadogradnje oblaka aplikacije koje se koriste SQL baza podataka aktivna zemlj. – replikacije](sql-database-manage-application-rolling-upgrade.md) | Saznajte kako koristiti zemlj replikacije baze podataka SQL Azure za podršku online nadogradnje aplikacije oblaka. |
| 171 | [Nadogradnja V12 za baze podataka SQL Azure pomoću portala za Azure](sql-database-upgrade-server-portal.md) | U članku se objašnjava kako nadograditi na V12 za baze podataka SQL Azure uključujući nadogradnju Web i poslovnih baza podataka i nadogradnja V11 poslužitelja migracije njegov baze podataka izravno u programa skup elastic baze podataka pomoću portala za Azure. |
| 172 | [Nadogradnja V12 za baze podataka SQL Azure pomoću komponente PowerShell](sql-database-upgrade-server-powershell.md) | U članku se objašnjava kako nadograditi na V12 za baze podataka SQL Azure uključujući nadogradnju Web i poslovnih baza podataka i nadogradnja V11 poslužitelja migracije njegov baze podataka izravno u elastic baze podataka grupe aplikacija pomoću komponente PowerShell. |
| 173 | [Planiranje i Priprema za nadogradnju na V12 baze podataka SQL](sql-database-v12-plan-prepare-upgrade.md) | U članku se opisuje pripreme i ograničenja prilikom nadogradnje na verziju V12 od Azure SQL baze podataka. |
| 174 | [Što je novo u V12 baze podataka SQL](sql-database-v12-whats-new.md) | U članku se opisuje zašto sustavi tvrtke koje koriste baze podataka SQL Azure u oblaku će koje prednosti prilikom nadogradnje na verziju V12 sada. |
| 175 | [Web- a Business Edition ruža najčešća pitanja vezana uz](sql-database-web-business-sunset-faq.md) | Saznajte kada Azure SQL Web i poslovnih baza podataka će biti povučena i informacije o značajkama i funkcijama nove razine servisa. |



## <a name="tools"></a>Alati

| &nbsp; | Naslov | Opis |
| --: | :-- | :-- |
| 176 | [Upravljanje bazom podataka Azure SQL pomoću komponente PowerShell](sql-database-command-line-tools.md) | Upravljanje bazom podataka SQL Azure sa servisom PowerShell. |
| 177 | [Praktični vodič SQL baze podataka: povezivanje programa Excel s bazom podataka Azure SQL i stvaranje izvješća](sql-database-connect-excel.md) | Saznajte kako u programu Microsoft Excel povezivanje s bazom podataka Azure SQL u oblaku. Uvoz podataka u programu Excel za istraživanje podataka i izvješćivanje o pogreškama. |
| 178 | [Stvaranje baze podataka SQL i izvoditi uobičajene zadatke Postavljanje baze podataka pomoću cmdleta ljuske PowerShell](sql-database-get-started-powershell.md) | Saznajte kako stvoriti bazu podataka za SQL sa servisom PowerShell. Uobičajeni zadaci postavljanja baze podataka upravlja se putem cmdleta ljuske PowerShell. |
| 179 | [Uvod u sinkronizaciju podataka Azure SQL (pretpregled)](sql-database-get-started-sql-data-sync.md) | Pomoću ovog praktičnog vodiča pojednostavnjuje početak rada za sinkronizaciju podataka SQL Azure (pretpregled). |
| 180 | [Upravljanje bazom podataka SQL Azure pomoću SQL Server Management Studio](sql-database-manage-azure-ssms.md) | Saznajte kako koristiti SQL Server Management Studio za upravljanje bazom podataka SQL poslužitelja i baza podataka. |
| 181 | [Upravljanje baze podataka SQL Azure pomoću portala za Azure](sql-database-manage-portal.md) | Saznajte kako koristiti Azure Portal za upravljanje relacijske baze podataka u oblak pomoću portala za Azure. |
| 182 | [Promjena servisa sloju i performanse razine (cijene sloju) SQL baze podataka pomoću komponente PowerShell](sql-database-scale-up-powershell.md) | Promjena sloju servisa i razinom performansi baze podataka Azure SQL pokazuje kako promjena veličine baze podataka sustava SQL prema gore ili dolje sa servisom PowerShell. Promjena cijene sloju baze podataka Azure SQL pomoću komponente PowerShell. |
| 183 | [Korištenje SQL Server Management Studio Azure RemoteApp da biste se povezali s bazom podataka SQL](sql-database-ssms-remoteapp.md) | Pomoću ovog praktičnog vodiča da biste saznali kako koristiti SQL Server Management Studio Azure RemoteApp za sigurnost i performanse prilikom povezivanja s bazom podataka SQL |



## <a name="reference"></a>Referenca

| &nbsp; | Naslov | Opis |
| --: | :-- | :-- |
| 184 | [Biblioteka veza za SQL baze podataka i SQL Server](sql-database-libraries.md) | Prikazuje broj Minimalna verzija za svaki upravljački program koji klijentski programi možete koristiti za povezivanje s bazom podataka SQL Azure ili Microsoft SQL Server. Vezu možete pronaći verziju informacije o upravljačke programe koji su objavio putem zajednice umjesto Microsoft. |
| 185 | [Ograničenja resursa za Azure SQL baze podataka](sql-database-resource-limits.md) | Ova stranica opisuje neke uobičajene ograničenja resursa za bazu podataka SQL Azure. |
| 186 | [Razlike u Transact-SQL baze podataka SQL Azure](sql-database-transact-sql-information.md) | Transakcija SQL naredbe koje manje od u potpunosti podržane u bazi podataka SQL Azure |



## <a name="miscellaneous"></a>Različiti

| &nbsp; | Naslov | Opis |
| --: | :-- | :-- |
| 187 | [Kopiranje baze podataka Azure SQL pomoću komponente PowerShell](sql-database-copy-powershell.md) | Stvaranje kopije baze podataka Azure SQL pomoću komponente PowerShell |
| 188 | [Kopiranje baze podataka Azure SQL pomoću Transact-SQL](sql-database-copy-transact-sql.md) | Stvaranje kopije baze podataka Azure SQL pomoću Transact-SQL |
| 189 | [Smjernice i ograničenja Općenito baze podataka Azure SQL](sql-database-general-limitations.md) | Ova stranica opisuje određena Općenito ograničenja za baze podataka SQL Azure, kao i područja interoperabilnost i podrška. |
| 190 | [Baze podataka SQL cijene sloju preporuke](sql-database-service-tier-advisor.md) | Prilikom promjene cijene razine Azure portalu cijene sloju preporuke su pod uvjetom da preporučeno sloju koji najbolje odgovaraju radi radno opterećenje postojeće Azure SQL baze podataka na. Cijene razine opisuju sloju i performanse razinu servisa SQL baze podataka. |


&nbsp;

<hr/>

<a name="extras_append"></a>

## <a name="extras"></a>Dodaci

<a name="extra_related_articles"></a>

### <a name="related-articles-from-other-azure-services"></a>Povezani članci s drugih servisa za Azure

- [Sigurnosno kopiranje aplikacije servisa Azure aplikacije u Azure](../app-service-web/web-sites-backup.md)

<a name="extra_documentation_tools"></a>

### <a name="documentation-tools"></a>Dokumentacija Alati

- [Ažuriranja objaviti za baze podataka SQL Azure](http://azure.microsoft.com/updates/?service=sql-database)

- [Pretraživanje sve vrste dokumentaciji sustava Microsoft Azure s filtrima](http://azure.microsoft.com/search/documentation/)

<a name="extra_learning_path_graphics"></a>

### <a name="learning-path-graphics"></a>Grafika put za učenje

- [Učenje put grafike za sve servise sustava Microsoft Azure](http://azure.microsoft.com/documentation/learning-paths/)

- [Tečaj: Saznajte baze podataka Azure SQL](http://azure.microsoft.com/documentation/learning-paths/sql-database-training-learn-sql-database/)

- [Tečaj: Baze podataka SQL elastic skala](http://azure.microsoft.com/documentation/learning-paths/sql-database-elastic-scale/)


<!--
AzIxAA.ExtraAppend.sql-database.txt
C:\_MainW\VS15\AzureIndexAllArticles\AzureIndexAllArticles\In-Out\In\

This bullet link is improperly disallowed by publishing automation due to presence of string 'docuXXmentation/arXXticles':
- [Search SQL Database documentation, with filters](http://azure.microsoft.com/docuXXmentation/arXXticles/?service=sql-database)
-->


