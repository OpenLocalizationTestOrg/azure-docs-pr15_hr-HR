<properties 
    pageTitle="Upravljanje bazom podataka SQL s SSMS | Microsoft Azure" 
    description="Saznajte kako koristiti SQL Server Management Studio za upravljanje bazom podataka SQL poslužitelja i baza podataka." 
    services="sql-database" 
    documentationCenter=".net" 
    authors="stevestein" 
    manager="jhubbard" 
    editor="tysonn"/>

<tags 
    ms.service="sql-database" 
    ms.workload="data-management" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/29/2016" 
    ms.author="sstein"/>


# <a name="managing-azure-sql-database-using-sql-server-management-studio"></a>Upravljanje bazom podataka SQL Azure pomoću SQL Server Management Studio 


> [AZURE.SELECTOR]
- [Portal za Azure](sql-database-manage-portal.md)
- [SSMS](sql-database-manage-azure-ssms.md)
- [PowerShell](sql-database-manage-powershell.md)

SQL Server Management Studio (SSMS) možete koristiti za administriranje poslužitelja baze podataka SQL Azure i baza podataka. U ovoj se temi vodit će vas kroz uobičajene zadatke s SSMS. Već moraju imati poslužitelj i bazu podataka stvorenu u bazi podataka SQL Azure prije nego što počnete. Potražite u članku [Stvaranje prve baze podataka SQL Azure](sql-database-get-started.md) i [Povezivanje i upita pomoću SSMS](sql-database-connect-query-ssms.md) dodatne informacije.

Preporučuje se da koristite najnoviju verziju SSMS kada radite s bazom podataka SQL Azure. 

> [AZURE.IMPORTANT] Uvijek koristite najnoviju verziju SSMS jer neprestano poboljšani za rad s najnovijim ažuriranjima Azure i SQL baze podataka. Najnoviju verziju, pročitajte članak [Preuzimanje SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).



## <a name="create-and-manage-azure-sql-databases"></a>Stvaranje i upravljanje bazama podataka Azure SQL

Dok ste povezani s **glavnom** bazom podataka, možete stvoriti baze podataka na poslužitelju i izmijeniti ili ispustite postojeće baze podataka. Sljedeći koraci opisuju kako postići nekoliko uobičajenih baze podataka zadataka upravljanja kroz Management Studio. Da biste izveli sljedeće zadatke, provjerite je li ste povezani s **glavnom** bazom podataka s prijava glavni razini poslužitelja koji ste stvorili kada postavite na poslužitelju.

Da biste otvorili prozor query Management Studio, otvorite mapu u bazama podataka, proširite mapu **Sustava baze podataka** , desnom tipkom miša kliknite **glavni**, a zatim **Novi upit**.

-   Stvaranje baze podataka pomoću naredbe za **Stvaranje baze podataka** . Dodatne informacije potražite u članku [Stvaranje baze podataka (SQL baza podataka)](https://msdn.microsoft.com/library/dn268335.aspx). Sljedeća naredba stvara bazu pod nazivom **myTestDB** i određuje je li Standard S0 Edition baze podataka pomoću Zadana maksimalna veličina od 250 GB.

        CREATE DATABASE myTestDB
        (EDITION='Standard',
         SERVICE_OBJECTIVE='S0');

Kliknite **izvršavanje** da biste pokrenuli upit.

-   Iskaz **ALTER baze podataka** pomoću možete izmijeniti postojeću bazu podataka, na primjer ako želite promijeniti naziv i edition baze podataka. Dodatne informacije potražite u članku [Baze podataka s ALTER (SQL baza podataka)](https://msdn.microsoft.com/library/ms174269.aspx). Sljedeća naredba mijenja bazu podataka koju ste stvorili u prethodnom koraku da biste promijenili edition standardne S1.

        ALTER DATABASE myTestDB
        MODIFY
        (SERVICE_OBJECTIVE='S1');

-   Korištenje **Baze podataka ISPUSTITE** izjava da biste izbrisali postojeću bazu podataka. Dodatne informacije potražite u članku [ODBACI bazu podataka (SQL baza podataka)](https://msdn.microsoft.com/library/ms178613.aspx). Sljedeća naredba briše **myTestDB** bazu podataka, ali ne ispustite ga sada jer će vam trebati da biste stvorili prijave u sljedećem koraku.

        DROP DATABASE myTestBase;

-   Glavni baza podataka ima prikaz **sys.databases** koje možete koristiti da biste pogledali detalje o sve baze podataka. Da biste pogledali sve postojeće baze podataka, izvršiti sljedeće naredbe:

        SELECT * FROM sys.databases;

-   U bazi podataka SQL izjava **koristi** nije podržana za prebacivanje između baza podataka. Umjesto toga, morate uspostaviti vezu ciljna baza podataka.

>[AZURE.NOTE] Morate pokrenuti unutar vlastite serije mnoge Transact-SQL naredbe koje stvaranje ili izmjenu baze podataka i ne mogu biti grupirane s drugim Transact-SQL naredbe. Dodatne informacije potražite u članku informacije specifične za izjava.

## <a name="create-and-manage-logins"></a>Stvaranje i upravljanje prijave

**Glavnom** bazom podataka sadrži prijave i koje se prijave imati dozvolu za stvaranje baze podataka ili druge prijave. Prijave možete upravljati tako da povezivanja s **glavnom** bazom podataka s prijava glavni razini poslužitelja koji ste stvorili kada postavite na poslužitelju. Koristite naredbe za **Stvaranje PRIJAVE**, **MIJENJATI prijavu**ili **ISPUSTITE prijava** za izvršavanje upite odabiranja glavnom bazom podataka koja upravlja prijave preko cijelog poslužitelja. Dodatne informacije potražite u članku [Upravljanje bazama podataka i prijave u SQL baze podataka](http://msdn.microsoft.com/library/azure/ee336235.aspx). 


-   Koristite naredbu **Stvori prijava** za stvaranje razini poslužitelja prijava. Dodatne informacije potražite u članku [Stvaranje prijavu (SQL baza podataka)](https://msdn.microsoft.com/library/ms189751.aspx). Sljedeća naredba stvara prijava pod nazivom **login1**. Zamijenite **password1** lozinka po izboru.

        CREATE LOGIN login1 WITH password='password1';

-   Iskaz **CREATE USER** koristite da biste dodijelili dozvole na razini baze podataka. Sve prijave moraju se stvoriti u **glavnom** bazom podataka. Za prijavu da biste se povezali s drugom bazom podataka, morate dati ga baze podataka razinu dozvole pomoću iskaz **CREATE USER** na toj bazi podataka. Dodatne informacije potražite u članku [Stvaranje korisnika (SQL baza podataka)](https://msdn.microsoft.com/library/ms173463.aspx). 

-   Da biste dodijelili dozvole login1 s bazom podataka naziva **myTestDB**, slijedite sljedeće korake:

 1.  Da biste osvježili Explorer objekt da biste pogledali **myTestDB** bazu podataka koju ste stvorili, desnom tipkom miša kliknite naziv poslužitelja u programu Explorer objekt, a zatim kliknite **Osvježi**.  

     Ako zatvorili veze, možete ponovno povezati tako da odaberete **Povezati objekt Explorer** na izborniku datoteka.

 2. Desnom tipkom miša kliknite **myTestDB** baze podataka, a zatim odaberite **Novi upit**.

    3.  Izvođenje sljedeće naredbe myTestDB baze podataka da biste stvorili pod nazivom **login1User** koji odgovara razini poslužitelja prijava **login1**korisnik baze podataka.

            CREATE USER login1User FROM LOGIN login1;

-   Korištenje na **sp\_addrolemember** pohranjena procedura da bi se dobilo korisnički račun odgovarajuću razinu dozvole za bazu podataka. Dodatne informacije potražite u članku [sp_addrolemember (Transact-SQL)](http://msdn.microsoft.com/library/ms187750.aspx). Sljedeća naredba daje **login1User** dozvole samo za čitanje u bazu podataka dodavanjem **login1User** da biste na **db\_datareader** uloge.

        exec sp_addrolemember 'db_datareader', 'login1User';    

-   Koristite iskaz **ALTER prijava** da biste izmijenili postojeći prijave, na primjer ako želite promijeniti lozinku za prijavu. Dodatne informacije potražite u članku [ALTER prijavu (SQL baza podataka)](https://msdn.microsoft.com/library/ms189828.aspx). Iskaz **ALTER prijava** trebale bi funkcionirati u odnosu na **glavnom** bazom podataka. Prebacite se natrag u prozoru upita koja je povezana s bazom podataka. Sljedeća naredba mijenja **login1** prijave za ponovno postavljanje lozinke. Zamijenite **novalozinka** lozinka za odabir i **staralozinka** s trenutnu lozinku za prijavu.

        ALTER LOGIN login1
        WITH PASSWORD = 'newPassword'
        OLD_PASSWORD = 'oldPassword';

-   Koristite izjavu **ISPUSTITE prijava** da biste izbrisali postojeću prijava. Brisanje Prijava na razini poslužitelja briše i sve korisničke račune pridruženih baza podataka. Dodatne informacije potražite u članku [ODBACI bazu podataka (SQL baza podataka)](https://msdn.microsoft.com/library/ms178613.aspx). Iskaz **ISPUSTITE prijava** trebale bi funkcionirati u odnosu na **glavnom** bazom podataka. Iskaz briše **login1** prijava.

        DROP LOGIN login1;

-   Glavni baza podataka ima u **sys.sql\_prijave** prikaz koje možete koristiti da biste pogledali prijave. Da biste pogledali sve postojeće prijave, izvršiti sljedeće naredbe:

        SELECT * FROM sys.sql_logins;

## <a name="monitor-sql-database-using-dynamic-management-views"></a>Praćenje SQL baze podataka pomoću dinamičnih prikaza upravljanja

Baze podataka SQL podržava nekoliko dinamičkih upravljanje prikaza koji možete koristiti za praćenje pojedinačne baze podataka. Pratite nekoliko primjera vrsta podataka monitora možete dohvatiti putem tih prikaza. Sve pojedinosti i još primjera korištenja potražite u članku [nadzor SQL baze podataka pomoću dinamičnih prikaza upravljanja](https://msdn.microsoft.com/library/azure/ff394114.aspx).

-   Ispitivanje prikaza dinamički upravljanja potrebne su dozvole **STANJE baze podataka za PRIKAZ** . Da biste dodijelili dozvolu **STANJE baze podataka za PRIKAZ** korisniku određene baze podataka, povezivanje s bazom podataka i izvršiti sljedeće naredbe u bazu podataka:

        GRANT VIEW DATABASE STATE TO login1User;

-   Izračun pomoću Veličina baze podataka u **sys.dm\_db\_particija\_stat** prikaz. U **sys.dm\_db\_particija\_stat** prikaz vraća informacije o stranice i broj redaka za svaku particije u bazi podataka koje možete koristiti za izračun Veličina baze podataka. Sljedeći upit vraća veličinu baze podataka u megabajtima:

        SELECT SUM(reserved_page_count)*8.0/1024
        FROM sys.dm_db_partition_stats;   

-   Korištenje na **sys.dm\_izvršavanja prve\_veze** i **sys.dm\_izvršavanja prve\_sesije** prikaza za dohvaćanje informacija o trenutnom veze i internim zadaci koji su povezani s bazom podataka. Sljedeći upit vraća informacije o trenutnu vezu.

        SELECT
            e.connection_id,
            s.session_id,
            s.login_name,
            s.last_request_end_time,
            s.cpu_time
        FROM
            sys.dm_exec_sessions s
            INNER JOIN sys.dm_exec_connections e
              ON s.session_id = e.session_id;

-   Korištenje na **sys.dm\_izvršavanja prve\_upita\_stat** prikaza za dohvaćanje statistiku o izvedbi zbrajanja za predmemorirani tarife upita. Sljedeći upit vraća informacije o Najčešći upiti pet prema Prosječno vrijeme procesora.

        SELECT TOP 5 query_stats.query_hash AS "Query Hash",
            SUM(query_stats.total_worker_time), SUM(query_stats.execution_count) AS "Avg CPU Time",
            MIN(query_stats.statement_text) AS "Statement Text"
        FROM
            (SELECT QS.*,
            SUBSTRING(ST.text, (QS.statement_start_offset/2) + 1,
            ((CASE statement_end_offset
                WHEN -1 THEN DATALENGTH(ST.text)
                ELSE QS.statement_end_offset END
                    - QS.statement_start_offset)/2) + 1) AS statement_text
             FROM sys.dm_exec_query_stats AS QS
             CROSS APPLY sys.dm_exec_sql_text(QS.sql_handle) as ST) as query_stats
        GROUP BY query_stats.query_hash
        ORDER BY 2 DESC;
 
 
