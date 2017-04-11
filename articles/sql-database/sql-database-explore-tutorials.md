<properties
   pageTitle="Istražite vodiči za baze podataka Azure SQL | Microsoft Azure"
   description="Dodatne informacije o baze podataka SQL značajke i mogućnosti"
   keywords=""
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management"
   ms.date="08/24/2016"
   ms.author="carlrab"/>
   
# <a name="explore-azure-sql-database-tutorials"></a>Istražite vodiči za baze podataka Azure SQL

Veze u nastavku vas odvesti na pregled svako područje navedenih značajki i jednostavnog korak – početak Praktični vodič za svako područje. Servis za rješenja brzog pokretanja kojima se ilustrira korištenje baze podataka SQL u potpuno rješenje koji se temelji na scenarije stvarnih svijeta potražite u članku [Azure SQL baza podataka rješenja brzog pokretanja](sql-database-solution-quick-starts.md).

## <a name="using-sql-server-management-studio"></a>Pomoću SQL Server Management Studio

U sljedećim vodičima će Saznajte kako pomoću SQL Server Management Studio upravljati i upita baze podataka SQL Azure.


> [AZURE.IMPORTANT] Preporučuje se da uvijek koristite najnoviju verziju Management Studio ostati sinkronizirani s ažuriranjima za Microsoft Azure i SQL baze podataka. [Ažuriranje SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


| Praktični vodič  | Opis  |
|---|---|---|
| [Povezivanje s bazom podataka SQL Azure pomoću upravitelja prijave razini poslužitelja](sql-database-get-started-security.md#connect-to-azure-sql-database-using-a-server-level-principal-login)| U ovom ćete praktičnom vodiču Saznajte kako se povezati s bazom podataka SQL Azure pomoću upravitelja prijave razini poslužitelja.|
| [Povezivanje s bazom podataka SQL Azure kao korisnik](sql-database-get-started-security.md#connect-to-azure-sql-database-as-a-user) | U ovom ćete praktičnom vodiču ćete naučiti da biste se povezali s bazom podataka Azure SQL pomoću baze podataka razinom korisnički račun.|
||||

## <a name="elastic-pools"></a>Elastic grupe

U sljedećim vodičima će Saznajte kako pomoću [elastic grupe](sql-database-elastic-pool.md) da biste upravljali ciljeve za više bazu podataka koja je široko različitim i nepredvidljive uzoraka korištenja.

| Praktični vodič  | Opis  |
|---|---|---|
| [Stvaranje elastic skup](sql-database-elastic-pool-create-portal.md) | U ovom ćete praktičnom vodiču saznati kako stvaranje skalabilni skup baze podataka Azure SQL. |
| [Praćenje elastic baze podataka](sql-database-elastic-pool-manage-portal.md#elastic-database-monitoring) | Ovog praktičnog vodiča saznat ćete kako praćenje pojedinačnih elastic baze podataka za potencijalnih problema. |
| [Dodavanje upozorenja u skup resursa](sql-database-elastic-pool-manage-portal.md#add-an-alert-to-a-pool-resource) | U ovom ćete praktičnom vodiču, Saznajte kako dodati pravila za resurse koji slanje e-pošte osobe ili upozorenja nizova za krajnje točke URL-a kada resurs dodirne Upotreba prag koji ste postavili. |
| [Premještanje baze podataka u elastic grupe aplikacija](sql-database-elastic-pool-manage-portal.md#move-a-database-into-an-elastic-pool) | U ovom ćete praktičnom vodiču Saznajte kako možete premjestiti baze podataka u elastic grupe aplikacija. |
| [Premještanje baze podataka iz elastic grupe aplikacija](sql-database-elastic-pool-manage-portal.md#move-a-database-out-of-an-elastic-pool) | U ovom ćete praktičnom vodiču, Saznajte kako možete premjestiti baze podataka iz elastic grupe aplikacija. |
| [Promjena postavki performansi zajedničko područje](sql-database-elastic-pool-manage-portal.md#change-performance-settings-of-a-pool) | U ovom ćete praktičnom vodiču Saznajte kako prilagoditi ograničenja performanse i prostora za pohranu za zajedničko područje. |
||||

## <a name="elastic-database-jobs"></a>Poslovi elastic baze podataka

U sljedećim vodičima će Saznajte kako pomoću [poslova elastic baze podataka](sql-database-elastic-jobs-overview.md).

| Praktični vodič  | Opis  |
|---|---|---|
| [Početak rada s alatima Elastic baze podataka](sql-database-elastic-scale-get-started.md) | U ovom ćete praktičnom vodiču Saznajte kako koristiti mogućnosti elastic Alati baze podataka pomoću jednostavne sharded aplikacije. |
| [Početak rada s bazom podataka SQL Azure elastic poslove](sql-database-elastic-jobs-getting-started.md)  | Pomoću ovog praktičnog vodiča saznat ćete kako da biste kako stvoriti i upravljanje zadaci koje upravljanje grupom povezanih baza podataka.  | 
||||

## <a name="elastic-queries"></a>Elastic upita

U sljedećim vodičima ćete naučiti o pokretanju [elastic upita](sql-database-elastic-query-overview.md). 

| Praktični vodič  | Opis  |
|---|---|---|
| [Slanje upita u bazi podataka vodoravno particioniranom (sharded))](sql-database-elastic-query-getting-started.md) | Pomoću ovog praktičnog vodiča saznat ćete kako se stvaranje izvješća iz sve baze podataka u vodoravno particioniranom (sharded) baze podataka koja se pomoću [elastic upita](sql-database-elastic-query-overview.md) |
| [Slanje upita preko okomito particioniranom baze podataka)](sql-database-elastic-query-getting-started-vertical.md#create-database-objects) | Pomoću ovog praktičnog vodiča saznat ćete kako se stvaranje izvješća iz sve baze podataka u u okomito baze podataka pomoću [elastic upita](sql-database-elastic-query-overview.md) |
| [Migrirati postojeću bazu podataka za skaliranje Izlaz](sql-database-elastic-convert-to-use-elastic-tools.md)| Ovog praktičnog vodiča saznat ćete vodoravno skaliranja (shard) Azure SQL baze podataka. |
||||

## <a name="performance-optimization"></a>Optimizaciju performansi

Vodiči za sljedeće će saznat ćete o optimiziranje [performanse jedne baze podataka](sql-database-performance-guidance.md). Optimiziranje performanse više baza podataka potražite u članku [Elastic grupe](#elastic-pools).

| Praktični vodič  | Opis  |
|---|---|---|
| [Promjena sloju i performanse razina usluge baze podataka](sql-database-scale-up.md#change-the-service-tier-and-performance-level-of-your-database) | U ovom ćete praktičnom vodiču Saznajte kako proširenja ili skalirali dolje performanse baze podataka Azure SQL pomoću usluga razine. |
| [SQL baza podataka Savjetnik za upit performanse uvida](sql-database-performance.md#performance-overview) | Ovog praktičnog vodiča saznat ćete kako za otvaranje i korištenje SQL baza podataka Savjetnik za upit performanse uvid.|
| [Preporuke za performanse Savjetnik za baze podataka SQL](sql-database-advisor.md#viewing-recommendations) | U ovom ćete praktičnom vodiču Saznajte kako pogledati i primijeniti Savjetnik za baze podataka SQL performanse preporuke. |
| [Pregledajte gornji procesora koristi upita](sql-database-query-performance.md#review-top-cpu-consuming-queries)| U ovom ćete praktičnom vodiču Saznajte kako koristiti SQL baza podataka Savjetnik za upit performanse uvid za pregled gornji procesora koristi upita.|
| [Prikaz pojedinosti pojedinačne upita](sql-database-query-performance.md#viewing-individual-query-details)| U ovom ćete praktičnom vodiču Saznajte kako koristiti SQL baza podataka Savjetnik za upit performanse uvid da biste pogledali detalje o performansama pojedinačne upita.|
||||

## <a name="sql-database-migration-and-archive"></a>Migracija baze podataka SQL i arhiviranja 

U sljedećim vodičima ćete naučiti o [migraciji postojeću bazu podataka sustava SQL Server s bazom podataka SQL Azure](sql-database-cloud-migrate.md).

| Praktični vodič  | Opis  |
|---|---|---|
| [Otkrivanje problema s kompatibilnošću pomoću SQL Server Data Tools za Visual Studio](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md#detecting-compatibility-issues-using-sql-server-data-tools-for-visual-studio) | U ovom ćete praktičnom vodiču saznati kako određivanje kompatibilnosti baze podataka SQL Azure pomoću SQL Server Data Tools za Visual Studio. |
| [Rješavanje problema s kompatibilnošću pomoću SQL Server Data Tools za Visual Studio](sql-database-cloud-migrate-fix-compatibility-issues-ssdt#fixing-compatibility-issues-using-sql-server-data-tools-for-visual-studio) | U ovom ćete praktičnom vodiču, Saznajte kako riješiti probleme s kompatibilnošću baze podataka SQL Azure pomoću SQL Server Data Tools za Visual Studio. |
| [Određivanje kompatibilnosti SQL baze podataka pomoću SqlPackage.exe](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md#using-sqlpackageexe) | U ovom ćete praktičnom vodiču saznati kako određivanje kompatibilnosti baze podataka SQL Azure pomoću pomoćnog programa naredbenog retka SQLPackage.exe.|
| [Određivanje kompatibilnosti SQL baze podataka pomoću SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md#using-sql-server-management-studio) |U ovom ćete praktičnom vodiču saznati kako određivanje kompatibilnosti baze podataka SQL Azure pomoću SQL Server Management Studio.|
| [Migracija baze podataka SQL Server u SQL baze podataka pomoću implementacija baze podataka radi Čarobnjak za baze podataka Microsoft Azure](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md#use-the-deploy-database-to-microsoft-azure-database-wizard) | U ovom ćete praktičnom vodiču će Saznajte kako migrirati kompatibilnim baze podataka SQL Server s bazom podataka SQL Azure pomoću implementacija baze podataka Microsoft Azure Čarobnjak za baze podataka u programu SQL Server Management Studio.
| [Izvoz baze podataka SQL Server BACPAC datoteku pomoću SSMS](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md) | U ovom ćete praktičnom vodiču ćete naučiti da biste izvezli kompatibilne baze podataka SQL Server BACPAC datoteku pomoću podataka sloju aplikacije čarobnjaka za izvoz u SQL Server Management Studio.|
| [Izvoz baze podataka SQL Server BACPAC datoteku pomoću SqlPackage](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md) | U ovom ćete praktičnom vodiču ćete naučiti da biste izvezli kompatibilan s bazom podataka SQL Server BACPAC datoteku korištenjem pomoćnog programa naredbenog retka SQLPackage.exe.|
| [Uvoz datoteke BACPAC u baze podataka SQL Azure pomoću SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md) | U ovom ćete praktičnom vodiču će Saznajte kako uvesti bazu podataka u bazu podataka SQL Azure iz datoteke BACPAC pomoću podataka sloju aplikacije čarobnjaka za izvoz u SQL Server Management Studio. |
| [Uvoz datoteke BACPAC u baze podataka SQL Azure pomoću SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md#import-from-a-bacpac-file-into-azure-sql-database-using-sqlpackage) | U ovom ćete praktičnom vodiču će Saznajte kako uvesti bazu podataka u bazu podataka SQL Azure iz datoteke BACPAC korištenjem pomoćnog programa naredbenog retka SQLPackage. |
| [Uvoz datoteke BACPAC u bazi podataka SQL Azure pomoću portala za Azure](sql-database-import.md) | U ovom ćete praktičnom vodiču će Saznajte kako uvesti baze podataka u bazu podataka SQL Azure BACPAC datoteku koja je spremljena u programa blobova platforme Azure pomoću portala za Azure.|
| [Uvoz datoteke BACPAC u bazi podataka SQL Azure pomoću komponente PowerShell](sql-database-import-powershell.md) | U ovom ćete praktičnom vodiču će Saznajte kako uvesti bazu podataka u bazu podataka SQL Azure iz datoteke BACPAC pomoću komponente PowerShell.|
| [Arhiviranje baze podataka Azure SQL pomoću portala za Azure](sql-database-export.md#export-your-database) | U ovom ćete praktičnom vodiču saznati kako Arhiviraj baze podataka Azure SQL BACPAC datoteku pomoću portala za Azure. |
| [Arhiviranje baze podataka Azure SQL pomoću komponente PowerShell](sql-database-export-powershell.md) | U ovom ćete praktičnom vodiču saznati kako arhivirati baze podataka Azure SQL BACPAC datoteku pomoću komponente PowerShell. |
| [Kopiranje baze podataka Azure SQL pomoću portala za Azure](sql-database-copy.md#copy-your-sql-database) | U ovom ćete praktičnom vodiču saznati kako kopiranje baze podataka Azure SQL pomoću portala za Azure. |
| [Kopiranje baze podataka Azure SQL pomoću komponente PowerShell](sql-database-copy-powershell#copy-your-sql-database) | U ovom ćete praktičnom vodiču saznati kako kopiranje baze podataka Azure SQL pomoću komponente PowerShell. |
| [Kopiranje baze podataka Azure SQL pomoću Transact-SQL](sql-database-copy-transact-sql.md#copy-your-sql-database) | U ovom ćete praktičnom vodiču saznati kako kopiranje baze podataka Azure SQL pomoću Transact-SQL. |
||||

##<a name="develop"></a>Razvoj

U sljedećim vodiči za će informacije o [Razvoju baze podataka SQL](sql-database-develop-overview.md) i pomoću [biblioteka za povezivanje](sql-database-libraries.md).

| Praktični vodič  | Opis  |
|---|---|---|
| [Povezivanje s bazom podataka SQL pomoću .NET (C#)](sql-database-develop-dotnet-simple.md) | U ovom ćete praktičnom vodiču saznati kako povezivanje s bazom podataka Azure SQL pomoću C#. |
| [Povezivanje s bazom podataka SQL pomoću Java](sql-database-develop-java-simple.md) | U ovom ćete praktičnom vodiču saznati kako povezivanje s bazom podataka Azure SQL pomoću Java. |
| [Povezivanje s bazom podataka SQL pomoću Node.js](sql-database-develop-nodejs-simple.md) | U ovom ćete praktičnom vodiču saznati kako povezivanje s bazom podataka Azure SQL pomoću Node.js. |
| [Povezivanje s bazom podataka SQL pomoću PHP](sql-database-develop-php-simple.md) | U ovom ćete praktičnom vodiču saznati kako povezivanje s bazom podataka Azure SQL pomoću PHP. |
| [Povezivanje s bazom podataka SQL pomoću Python](sql-database-develop-python-simple.md) | U ovom ćete praktičnom vodiču saznati kako povezivanje s bazom podataka Azure SQL pomoću Python. |
| [Povezivanje s bazom podataka SQL pomoću Ruby](sql-database-develop-ruby-simple.md) | U ovom ćete praktičnom vodiču saznati kako povezivanje s bazom podataka Azure SQL pomoću Ruby. |
||||
 
## <a name="database-access"></a>Pristup bazi podataka

U sljedećim vodičima će informacije o [stvaranju i upravljanju prijave i korisnicima](sql-database-manage-logins.md).

| Praktični vodič  | Opis  |
|---|---|---|
| [Stvaranje pravila vatrozida razini poslužitelja baze podataka SQL Azure pomoću portala za Azure](sql-database-configure-firewall-settings.md)  | U ovom ćete praktičnom vodiču, Saznajte kako konfigurirati Vatrozid razini poslužitelja SQL baze podataka pomoću portala za Azure.  |
| [Stvaranje pravila vatrozida razinom baze podataka pomoću Transact-SQL](sql-database-configure-firewall-settings-tsql.md#database-level-firewall-rules) | U ovom ćete praktičnom vodiču ćete naučiti da biste stvorili pravilo vatrozid razinom baze podataka pomoću Transact-SQL.|
| [Upravljanje pravilima vatrozid razini poslužitelja pomoću Transact-SQL](sql-database-configure-firewall-settings-tsql.md#manage-server-level-firewall-rules-through-transact-sql) | U ovom ćete praktičnom vodiču će Saznajte kako upravljati vatrozid razini poslužitelja baze podataka SQL Azure koja se pomoću Transact-SQL.|
| [Upravljanje pravilima vatrozid razini poslužitelja pomoću komponente PowerShell](sql-database-configure-firewall-settings-powershell.md#manage-firewall-rules-using-powershell) | U ovom ćete praktičnom vodiču će Saznajte kako upravljati vatrozid razini poslužitelja baze podataka SQL Azure koja se pomoću komponente PowerShell.|
| [Upravljanje pravilima vatrozid razini poslužitelja pomoću REST API-JA](sql-database-configure-firewall-settings-rest.md#manage-firewall-rules-using-the-service-management-rest-api) | U ovom ćete praktičnom vodiču će Saznajte kako upravljati vatrozid razini poslužitelja baze podataka SQL Azure koja se pomoću API ponovno postavljanje.|
| [Povezivanje s bazom podataka SQL Azure pomoću upravitelja prijave razini poslužitelja](sql-database-get-started-security.md#connect-to-azure-sql-database-using-a-server-level-principal-login)| U ovom ćete praktičnom vodiču Saznajte kako se povezati s bazom podataka SQL Azure pomoću upravitelja prijave razini poslužitelja.|
| [Date pristup bazi podataka za prijave] (sql-database-manage-logins.md#granting-database-access-to-a-login() | U ovom ćete praktičnom vodiču saznati kako dopustiti pristup bazi podataka za prijavu na razini poslužitelja.|
| [Stvaranje nove baze podataka korisnika pomoću SSMS](sql-database-get-started-security.md#create-new-database-user-using-ssms) | U ovom ćete praktičnom vodiču saznati kako stvoriti novi korisnik baze podataka u postojeće baze podataka pomoću SSMS.|
| [Dodjela nove baze podataka korisničkih db_owner dozvola](sql-database-get-started-security.md#grant-new-database-user-dbowner-permissions) | U ovom ćete praktičnom vodiču saznati kako Dodjela korisničkih dozvola db_owner postojeću bazu podataka.|
| [Povezivanje s bazom podataka SQL Azure kao korisnik](sql-database-get-started-security.md#connect-to-azure-sql-database-as-a-user) | U ovom ćete praktičnom vodiču Saznajte kako se povezati s bazom podataka Azure SQL pomoću baze podataka razinom korisnički račun.|
||||


## <a name="data-security"></a>Sigurnost podataka

U sljedećim vodičima ćete naučiti o [zaštiti podataka baze podataka SQL Azure](sql-database-security.md). 

| Praktični vodič  | Opis  |
|---|---|---|
| [Omogući prijetnju otkrivanje za bazu podataka pomoću portala za Azure](sql-database-threat-detection-get-started.md#set-up-threat-detection-for-your-database) | U ovom ćete praktičnom vodiču Saznajte kako postaviti prijetnju otkrivanje Azure portalu za bazu podataka.|
| [Sigurne uisng povjerljive podatke uvijek šifrirane](sql-database-always-encrypted-azure-key-vault.md) | U ovom ćete praktičnom vodiču će koristiti Čarobnjak za uvijek šifrirane za zaštitu osjetljivih podataka u bazu podataka Azure SQL.|
| [Sigurna senstive podataka pomoću šifriranje prozirne podataka](https://msdn.microsoft.com/library/dn948096.aspx)| U ovom ćete praktičnom vodiču Saznajte kako koristiti šifriranje prozirnom podataka radi zaštite podataka senstive.|
| [Šifriranje stupac s podacima](https://msdn.microsoft.com/library/ms179331.aspx)| U ovom ćete praktičnom vodiču saznati kako šifriranje stupac s podacima pomoću Transact-SQL.|
| [Postavljanje masking dinamične podatke](sql-database-dynamic-data-masking-get-started.md#set-up-dynamic-data-masking-for-your-database-using-the-azure-portal)  | U ovom ćete praktičnom vodiču Saznajte kako postaviti masking dinamičke podatke za bazu podataka Azure SQL. |
||||

## <a name="business-continuity-and-query-scale-out"></a>Neprekidno poslovanje i skaliranje upita

U sljedećim vodičima koje će Saznajte kako pomoću [zemlj Vrati i aktivni replikacije zemlj](sql-database-business-continuity.md) reccover iz pogreške, za poslovanje i skaliranje upita.

| Praktični vodič  | Opis  |
|---|---|---|
| [Vraćanje baze podataka SQL Azure prethodnog točku u vremenu pomoću portala za Azure](sql-database-point-in-time-restore-portal.md)| U ovom ćete praktičnom vodiču Saznajte kako vratiti bazu podataka ranije točke u vremenu pomoću portala za Azure.|
| [Vraćanje baze podataka SQL Azure prethodnog točku u vremenu pomoću komponente PowerShell](sql-database-point-in-time-restore-powershell.md) | Pomoću ovog praktičnog vodiča saznat ćete kako se vratiti bazu podataka ranije točke u vremenu pomoću komponente PowerShell|
| [Vraćanje izbrisane Azure SQL baze podataka pomoću portala za Azure](sql-database-restore-deleted-database-portal.md) | U ovom ćete praktičnom vodiču saznati kako vraćanje izbrisanih baze podataka pomoću portala za Azure.|
| [Vraćanje izbrisane Azure SQL baze podataka pomoću na komponente PowerShell](sql-database-restore-deleted-database-powershell.md) | Ovog praktičnog vodiča saznat ćete kako da biste vratili izbrisanu bazu podataka pomoću komponente PowerShell.|
| [Konfiguriranje zemlj replikacije baze podataka SQL Azure pomoću portala za Azure](sql-database-geo-replication-portal.md)| U ovom ćete praktičnom vodiču, Saznajte kako konfigurirati aktivni zemlj. – replikacije pomoću portala za Azure.|
| [Konfiguriranje zemlj replikacije baze podataka SQL Azure pomoću komponente PowerShell](sql-database-geo-replication-powershell.md)| Pomoću ovog praktičnog vodiča saznat ćete kako konfigurirati aktivni replikacije zemlj pomoću komponente PowerShell.|
| [Konfiguriranje zemlj replikacije baze podataka SQL Azure pomoću Transact-SQL](sql-database-geo-replication-transact-sql.md)| Pomoću ovog praktičnog vodiča saznat ćete kako konfigurirati aktivni replikacije zemlj pomoću Transact-SQL.|
| [Pokretanje planiranog ili neplanirano Prebacivanje baze podataka SQL Azure pomoću portala za Azure](sql-database-geo-replication-failover-portal.md) | U ovom ćete praktičnom vodiču saznati kako prebacivanje da biste na zemlj replicirati sekundarnog replike pomoću portala za Azure.|
| [Pokretanje planiranog ili neplanirano Prebacivanje baze podataka SQL Azure pomoću komponente PowerShell](sql-database-geo-replication-failover-powershell.md) | U ovom ćete praktičnom vodiču saznati kako prebacivanje da biste na zemlj replicirati sekundarnog replike pomoću komponente PowerShell.|
| [Pokretanje planiranog ili neplanirano prebacivanje za baze podataka SQL Azure pomoću Transact-SQL](sql-database-geo-replication-failover-transact-sql.md) | U ovom ćete praktičnom vodiču saznati kako prebacivanje da biste na zemlj replicirati sekundarnog replike pomoću Transact-SQL.|
||||

## <a name="data-sync"></a>Sinkroniziranje podataka

U ovom ćete praktičnom vodiču će upoznati [Sinkronizacije podataka](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf).

| Praktični vodič  | Opis  |
|---|---|---| 
| [Uvod u sinkronizaciju podataka Azure SQL (pretpregled)](sql-database-get-started-sql-data-sync.md)  | Pomoću ovog praktičnog vodiča saznat ćete osnove sinkronizacije podataka SQL Azure koje pomoću portala za klasični Azure. |
||||

## <a name="next-steps"></a>Daljnji koraci

[Istražite Azure SQL baza podataka rješenja brzog pokretanja](sql-database-solution-quick-starts.md)
