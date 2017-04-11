<properties
    pageTitle="Konfiguriranje zemlj replikacije baze podataka Azure SQL s SQL transakcija | Microsoft Azure"
    description="Konfiguriranje zemlj replikacije baze podataka SQL Azure pomoću Transact-SQL"
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
    ms.workload="NA"
    ms.date="10/13/2016"
    ms.author="carlrab"/>

# <a name="configure-geo-replication-for-azure-sql-database-with-transact-sql"></a>Konfiguriranje zemlj replikacije baze podataka Azure SQL s SQL transakcija

> [AZURE.SELECTOR]
- [Pregled](sql-database-geo-replication-overview.md)
- [Portal za Azure](sql-database-geo-replication-portal.md)
- [PowerShell](sql-database-geo-replication-powershell.md)
- [T-SQL](sql-database-geo-replication-transact-sql.md)

U ovom se članku objašnjava konfiguriranje aktivni replikacije zemlj za baze podataka SQL Azure s Transact-SQL.

Da biste započeli prebacivanje pomoću Transact-SQL potražite u članku [pokretanje planiranog ili neplanirano Prebacivanje baze podataka SQL Azure s Transact-SQL](sql-database-geo-replication-failover-transact-sql.md).

>[AZURE.NOTE] Sada je dostupan za sve baze podataka u sve razine servisa Active zemlj. – replikacije (čitljivi secondaries). U travnju 2017 će povučena koje nisu čitljiv sekundarne vrsta i postojeće baze podataka koje nisu čitljiv automatski nadogradit će se čitati secondaries.

Da biste konfigurirali aktivni replikacije zemlj pomoću Transact-SQL, potrebno je sljedeće:

- Azure pretplate.
- Logička poslužitelj baze podataka SQL Azure <MyLocalServer> i baze podataka SQL <MyDB> -primarni bazu podataka koju želite za replikaciju.
- Jedan ili više logičke baze podataka SQL Azure poslužitelji < MySecondaryServer(n) > - u logičke koji će biti partnera poslužitelji u kojem ćete stvoriti sekundarne baze podataka.
- Prijava koja se nalazi DBManager na primarni, imate db_ownership će zemlj replicirati, lokalnu bazu podataka i mora biti DBManager na partnera poslužitelji u koju ćete konfigurirati zemlj replikacije.
- SQL Server Management Studio (SSMS)

> [AZURE.IMPORTANT] Preporučuje se da uvijek koristite najnoviju verziju Management Studio ostati sinkronizirani s ažuriranjima za Microsoft Azure i SQL baze podataka. [Ažuriranje SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="add-secondary-database"></a>Dodavanje sekundarni baze podataka

Iskaz **ALTER baze podataka** možete koristiti da biste stvorili zemlj replicirati sekundarne baze podataka na poslužitelju partnera. U ovom naredba execute na glavnom bazom podataka koja sadrži baze podataka da biste je replicirati poslužitelja. Zemlj replicirati baza podataka ("primarni baze podataka") će imati isti naziv kao baze podataka koja se replicirati i će prema zadanom imaju istu razinu servisa kao primarni baze podataka. Sekundarni baze podataka može čitati ili nije čitljiv, a može biti jedne baze podataka ili elastic databbase. Dodatne informacije potražite u članku [Bazu podataka s ALTER (Transact-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) i [Servis razine](sql-database-service-tiers.md).
Kad sekundarne baze podataka stvorene i seeded, podaci će početi replikaciju asinkrono s primarnom bazom podataka. Koraci u nastavku opisuje način konfiguriranja zemlj replikacije pomoću Management Studio. Su navedeni koraci za stvaranje koje nisu čitljiv i čitljiv secondaries, pomoću jedne baze podataka ili elastic baze podataka.

> [AZURE.NOTE] Ako postoji baze podataka na poslužitelju navedeni partnera s istim nazivom kao primarni baza podataka naredbu neće uspjeti.


### <a name="add-non-readable-secondary-single-database"></a>Dodavanje nije čitljiv sekundarni (jedan baza podataka)

Poduzmite sljedeće korake da biste stvorili koje nisu čitljiv sekundarni kao jedan baze podataka.

1. Pomoću verzije 13.0.600.65 ili novija verzija sustava SQL Server Management Studio.

     > [AZURE.IMPORTANT] Preuzmite [najnoviju](https://msdn.microsoft.com/library/mt238290.aspx) verziju sustava SQL Server Management Studio. Preporučuje se da uvijek koristite najnoviju verziju Management Studio bude sinkronizacija ažuriranja Azure portalu.


2. Otvorite mapu u bazama podataka, proširite mapu **Sustava baze podataka** , desnom tipkom miša kliknite **glavni**, a zatim **Novi upit**.

3. Koristite sljedeću naredbu **ALTER baze podataka** da biste lokalne baze podataka u replikacijom zemlj primarni s koje nisu čitljiv sekundarni baze podataka na MySecondaryServer1 gdje je MySecondaryServer1 naziv poslužitelja neslužbeni.

        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer1> WITH (ALLOW_CONNECTIONS = NO);

4. Kliknite **izvršavanje** da biste pokrenuli upit.


### <a name="add-readable-secondary-single-database"></a>Dodavanje čitljiv sekundarni (jedan baza podataka)
Poduzmite sljedeće korake da biste stvorili čitljiv sekundarni kao jedan baze podataka.

1. U Management Studio povezati s poslužiteljem baze podataka SQL Azure logičke.

2. Otvorite mapu u bazama podataka, proširite mapu **Sustava baze podataka** , desnom tipkom miša kliknite **glavni**, a zatim **Novi upit**.

3. Koristiti sljedeći iskaz **ALTER baze podataka** te s lokalnom bazom podataka u replikacije zemlj primarni s čitljiv sekundarne baze podataka na sekundarni poslužitelj.

        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer2> WITH (ALLOW_CONNECTIONS = ALL);

4. Kliknite **izvršavanje** da biste pokrenuli upit.



### <a name="add-non-readable-secondary-elastic-database"></a>Dodavanje koje nisu čitljiv sekundarni (elastic baza podataka)

Poduzmite sljedeće korake da biste stvorili koje nisu čitljiv sekundarni elastic bazom podataka.

1. U Management Studio povezati s poslužiteljem baze podataka SQL Azure logičke.

2. Otvorite mapu u bazama podataka, proširite mapu **Sustava baze podataka** , desnom tipkom miša kliknite **glavni**, a zatim **Novi upit**.

3. Da bi lokalne baze podataka u replikacije zemlj primarni s koje nisu čitljiv sekundarne baze podataka u poslužiteljskom sekundarne u elastic skup koristiti sljedeći iskaz **ALTER baze podataka** .

        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer3> WITH (ALLOW_CONNECTIONS = NO
           , SERVICE_OBJECTIVE = ELASTIC_POOL (name = MyElasticPool1));

4. Kliknite **izvršavanje** da biste pokrenuli upit.



### <a name="add-readable-secondary-elastic-database"></a>Dodavanje čitljiv sekundarni (elastic baza podataka)
Poduzmite sljedeće korake da biste stvorili čitljiv sekundarni elastic bazom podataka.

1. U Management Studio povezati s poslužiteljem baze podataka SQL Azure logičke.

2. Otvorite mapu u bazama podataka, proširite mapu **Sustava baze podataka** , desnom tipkom miša kliknite **glavni**, a zatim **Novi upit**.

3. Koristiti sljedeći iskaz **ALTER baze podataka** te s lokalnom bazom podataka u replikacije zemlj primarni s čitljiv sekundarne baze podataka u poslužiteljskom sekundarne u elastic grupe aplikacija.

        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer4> WITH (ALLOW_CONNECTIONS = ALL
           , SERVICE_OBJECTIVE = ELASTIC_POOL (name = MyElasticPool2));

4. Kliknite **izvršavanje** da biste pokrenuli upit.



## <a name="remove-secondary-database"></a>Uklanjanje sekundarne baze podataka

Iskaz **ALTER baze podataka** možete koristiti da biste trajno prekinuli replikacije partnerstvo između sekundarne baze podataka i njegove primarne. Izjavu o zaštiti se izvršava na glavnom bazom podataka na kojem se nalazi primarnom bazom podataka. Nakon prekida odnos sekundarne baze podataka postaje običnoj bazi podataka za čitanje i pisanje. Ako je prekinuta veza s sekundarne baze podataka naredba je uspjela, ali sekundarne će postati čitanja i pisanja nakon vratit će se povezivanje. Dodatne informacije potražite u članku [Bazu podataka s ALTER (Transact-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) i [Servis razine](sql-database-service-tiers.md).

Poduzmite sljedeće korake da biste uklonili zemlj replicirati sekundarni zemlj replikacije partnerstvo.

1. U Management Studio povezati s poslužiteljem baze podataka SQL Azure logičke.

2. Otvorite mapu u bazama podataka, proširite mapu **Sustava baze podataka** , desnom tipkom miša kliknite **glavni**, a zatim **Novi upit**.

3. Da biste uklonili sekundarni zemlj replicirati, koristite sljedeću naredbu **ALTER baze podataka** .

        ALTER DATABASE <MyDB>
           REMOVE SECONDARY ON SERVER <MySecondaryServer1>;

4. Kliknite **izvršavanje** da biste pokrenuli upit.

## <a name="monitor-geo-replication-configuration-and-health"></a>Praćenje konfiguracija zemlj replikacije i stanju sustava

Nadzor poslovi obuhvaćaju nadzor konfiguracije zemlj replikacije i praćenje stanja replikacije podataka.  Prikaz za dinamičku upravljanja **sys.dm_geo_replication_links** u glavnom bazom podataka možete koristiti da biste se vratili informacije o sve veze replikacije licenci za svaku bazu podataka na poslužitelju baze podataka SQL Azure logičke. Ovaj prikaz sadrži redak za svaku vezu replikacije između primarnih i sekundarnih baza podataka. Prikaz dinamički upravljanja **sys.dm_replication_link_status** možete koristiti da biste se vratili u retku za svaki baze podataka Azure SQL koji trenutno aktivan u replikacije replikacije vezu. To obuhvaća primarnih i sekundarnih baze podataka. Ako postoji više veza neprekinuti replikacije za dani primarni bazu podataka, ova tablica sadrži redak za svaki unos odnosa. Prikaz je stvoren u sve baze podataka, uključujući logičke matrice. Međutim, slanje upita u tom prikazu u matrici logičke vraća prazan skup. Prikaz dinamički upravljanja **sys.dm_operation_status** možete koristiti da biste prikazali status za sve baze podataka operacije uključujući stanje veze replikacije. Dodatne informacije potražite u članku [sys.geo_replication_links (bazom podataka SQL Azure)](https://msdn.microsoft.com/library/mt575501.aspx), [sys.dm_geo_replication_link_status (bazom podataka SQL Azure)](https://msdn.microsoft.com/library/mt575504.aspx)i [sys.dm_operation_status (bazom podataka SQL Azure)](https://msdn.microsoft.com/library/dn270022.aspx).

Praćenje zemlj replikacije partnerstvo pomoću sljedećih koraka.

1. U Management Studio povezati s poslužiteljem baze podataka SQL Azure logičke.

2. Otvorite mapu u bazama podataka, proširite mapu **Sustava baze podataka** , desnom tipkom miša kliknite **glavni**, a zatim **Novi upit**.

3. Koristite sljedeću naredbu da biste prikazali sve baze podataka s vezama zemlj replikacije.

        SELECT database_id, start_date, modify_date, partner_server, partner_database, replication_state_desc, role, secondary_allow_connections_desc FROM [sys].geo_replication_links;

4. Kliknite **izvršavanje** da biste pokrenuli upit.
5. Otvorite mapu u bazama podataka, proširite mapu **Sustava baze podataka** , desnom tipkom miša kliknite **MyDB**, a zatim **Novi upit**.
6. Koristite sljedeću naredbu da biste prikazali replikacije lags i zadnji replikacije vremena Moje sekundarne baza podataka MyDB.

        SELECT link_guid, partner_server, last_replication, replication_lag_sec FROM sys.dm_geo_replication_link_status

7. Kliknite **izvršavanje** da biste pokrenuli upit.
8. Koristite sljedeću naredbu da biste prikazali najnovije operacije zemlj replikacije povezan s bazom podataka MyDB.

        SELECT * FROM sys.dm_operation_status where major_resource_id = 'MyDB'
        ORDER BY start_time DESC

9. Kliknite **izvršavanje** da biste pokrenuli upit.

## <a name="upgrade-a-non-readable-secondary-to-readable"></a>Nadogradite koje nisu čitljiv sekundarni čitljiv

U travnju 2017 će povučena koje nisu čitljiv sekundarne vrsta i postojeće baze podataka koje nisu čitljiv automatski nadogradit će se čitati secondaries. Ako koristite koje nisu čitljiv secondaries danas, a želite nadograditi ih da biste mogli čitati, možete koristiti sljedeće jednostavne korake za svaku sekundarni.

> [AZURE.IMPORTANT] Ne postoji način samostalno lokalno nadogradnje sustava nije čitljiv sekundarni za čitljiv. Ako ispustite na samo sekundarni, zatim primarnom bazom podataka će ostati nezaštićena do novog sekundarni potpuno sinkroniziraju. Ako je SLA vaše aplikacije potreban je uvijek zaštićen primarni, razmislite o stvaranju paralelno sekundarni u drugi poslužitelj prije primjene nadogradnje gore navedene korake. Imajte na umu svaki primarni mogu imati do 4 sekundarni baze podataka.


1. Prvo povezivanje *sekundarni* poslužitelj i odbacivanje koje nisu čitljiv sekundarni baze podataka:  
        
        DROP DATABASE <MyNonReadableSecondaryDB>;

2. Sada se povezati s poslužiteljem *primarnog* i dodati nove čitljiv sekundarnu

        ALTER DATABASE <MyDB>
            ADD SECONDARY ON SERVER <MySecondaryServer> WITH (ALLOW_CONNECTIONS = ALL);




## <a name="next-steps"></a>Daljnji koraci

- Dodatne informacije o Active zemlj. – replikacije potražite u članku - [Active replikacije zemlj.](sql-database-geo-replication-overview.md)
- Pregled za continuity tvrtke i scenariji potražite u članku [Pregled continuity tvrtke](sql-database-business-continuity.md)
