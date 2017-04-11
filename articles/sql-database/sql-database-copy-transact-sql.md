<properties 
    pageTitle="Kopiranje baze podataka Azure SQL pomoću Transact-SQL | Microsoft Azure" 
    description="Stvaranje kopije baze podataka Azure SQL pomoću Transact-SQL" 
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="09/19/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="copy-an-azure-sql-database-using-transact-sql"></a>Kopiranje baze podataka Azure SQL pomoću Transact-SQL


> [AZURE.SELECTOR]
- [Pregled](sql-database-copy.md)
- [Portal za Azure](sql-database-copy-portal.md)
- [PowerShell](sql-database-copy-powershell.md)
- [T-SQL](sql-database-copy-transact-sql.md)


U ovom sljedeći koraci pokazuju kako da biste kopirali SQL baze podataka pomoću Transact-SQL na istom poslužitelju ili na drugi poslužitelj. Kopiranje baze podataka koristi naredbu za [Stvaranje baze podataka](https://msdn.microsoft.com/library/ms176061.aspx) .

Da biste prošli korake u ovom članku potrebno je sljedeće:

- Azure pretplate. Ako vam je potrebna pretplata na Azure jednostavno kliknite **BESPLATNU probnu VERZIJU** pri vrhu ove stranice, a zatim vratite do kraja ovog članka.
- Baze podataka Azure SQL. Ako nemate SQL baze podataka, stvoriti jednu slijedeći korake u ovom se članku: [Stvaranje prve baze podataka SQL Azure](sql-database-get-started.md).
- [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/ms174173.aspx). Ako nemate SSMS ili značajki opisanih u ovom članku nisu dostupne, [Preuzmite najnoviju verziju](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="copy-your-sql-database"></a>Kopiranje baze podataka sustava SQL

Prijavite se na glavnom bazom podataka pomoću web-mjesto prijava glavni razini poslužitelja ili u okvir za prijavu koja je stvorila bazu podataka koju želite kopirati. Prijave koji nisu glavnicu razini poslužitelja moraju biti članovi uloge dbmanager da biste kopirali baze podataka. Dodatne informacije o prijave i povezivanja s poslužiteljem potražite u članku [Upravljanje prijave](sql-database-manage-logins.md).

Kopiranja izvorišne baze podataka pomoću naredbe za [Stvaranje baze podataka](https://msdn.microsoft.com/library/ms176061.aspx) . Izvođenje izjavu o zaštiti pokreće baze podataka kopiranjem postupak. Jer se asinkronog postupak kopiranje baze podataka, naredbe za stvaranje baze podataka vraća prije bazu podataka dovršava kopiranja.


### <a name="copy-a-sql-database-to-the-same-server"></a>Kopiranje bazi podataka sustava SQL na isti poslužitelj

Prijavite se na glavnom bazom podataka pomoću web-mjesto prijava glavni razini poslužitelja ili u okvir za prijavu koja je stvorila bazu podataka koju želite kopirati. Prijave koji nisu glavnicu razini poslužitelja moraju biti članovi uloge dbmanager da biste kopirali baze podataka.

Ova kopija naredba Database1 na novu bazu podataka s nazivom Database2 na istom poslužitelju. Ovisno o veličini baze podataka kopiranja može potrajati neko vrijeme da biste dovršili.

    -- Execute on the master database.
    -- Start copying.
    CREATE DATABASE Database1_copy AS COPY OF Database1;

### <a name="copy-a-sql-database-to-a-different-server"></a>Kopiranje bazi podataka sustava SQL na drugi poslužitelj

Prijavite se na glavnom bazom podataka odredišnog poslužitelja, poslužitelj baze podataka SQL Azure gdje je novu bazu podataka će biti stvoren. Koristite prijava u izvorišne baze podataka na poslužitelju baze podataka SQL Azure izvora kao vlasnik baze podataka (vlasnika baze podataka) ima isti naziv i lozinku. Prijava na odredišnom poslužitelju moraju također biti član uloge dbmanager ili prijavu glavni razini poslužitelja.

Ta se naredba kopira Database1 na Poslužitelj1 - novu bazu podataka s nazivom Database2 na server2. Ovisno o veličini baze podataka kopiranja može potrajati neko vrijeme da biste dovršili.


    -- Execute on the master database of the target server (server2)
    -- Start copying from Server1 to Server2
    CREATE DATABASE Database1_copy AS COPY OF server1.Database1;
    

## <a name="monitor-the-progress-of-the-copy-operation"></a>Praćenje napretka kopiranja

Praćenje postupak kopiranja postavljanjem upita sys.databases i sys.dm_database_copies prikaze. Dok je u tijeku kopiranja, stupac state_desc sys.databases prikaza za novu bazu podataka postavljen je na kopiranje.


- Ako kopiranja ne uspije, stupac state_desc sys.databases prikaza za novu bazu podataka postavljen je na SUMNJIVE. U ovom slučaju izvršiti naredba DROP na novu bazu podataka i pokušajte ponovno.
- Ako se potvrdi kopiranja stupcu state_desc sys.databases prikaza za novu bazu podataka postavljen je na mreži. U ovom slučaju kopiranja Završi i novu bazu podataka je običnoj bazi podataka, možete mijenjati neovisno o izvorišne baze podataka.

> [AZURE.NOTE] – Ako odlučite da biste poništili kopiranje dok je u tijeku, izvršiti naredbu [ODBACI bazu podataka](https://msdn.microsoft.com/library/ms178613.aspx) na novu bazu podataka. Osim toga, izvršavanja izjava ODBACI bazu podataka na izvorišne baze podataka i otkazuje postupak kopiranja.


## <a name="resolve-logins-after-the-copy-operation-completes"></a>Razrješavanje prijave nakon dovršetka postupka Kopiraj

Kada je novu bazu podataka na Internetu na odredišnom poslužitelju, pomoću iskaz [ALTER USER](https://msdn.microsoft.com/library/ms176060.aspx) promijenite korisnicima novu bazu podataka prijave na odredišnom poslužitelju. Da biste riješili bez nadređene korisnika, potražite u članku [Otklanjanje poteškoća s bez nadređene korisnika](https://msdn.microsoft.com/library/ms175475.aspx). Pogledajte i [kako upravljati sigurnost baze podataka Azure SQL nakon oporavka Izrada](sql-database-geo-replication-security-config.md).

Svi korisnici u novoj bazi podataka imaju dozvole koje su imali u izvorišne baze podataka. Korisnik koji je pokrenuo kopiju baze podataka postaje vlasnik baze podataka nove baze podataka i se dodjeljuje novi sigurnosni identifikator (SID). Nakon uspješnog kopiranja, a prije nego što su neispravno drugim korisnicima, samo prijavu pokrenut kopiranja, vlasnik baze podataka (vlasnika baze podataka), možete prijaviti u novu bazu podataka.


## <a name="next-steps"></a>Daljnji koraci

- Pročitajte članak [kopiranje baze podataka Azure SQL](sql-database-copy.md) za pregled kopiranje baze podataka SQL Azure.
- Potražite u članku [kopiranje baze podataka Azure SQL pomoću portala za Azure](sql-database-copy-portal.md) da biste kopirali baze podataka pomoću portala za Azure.
- Potražite u članku [kopiju baze podataka Azure SQL pomoću komponente PowerShell](sql-database-copy-powershell.md) za kopiranje baze podataka pomoću komponente PowerShell.
- Pogledajte [kako upravljati sigurnost baze podataka Azure SQL nakon oporavka Izrada](sql-database-geo-replication-security-config.md) dodatne informacije o upravljanju korisnicima i prijave pri kopiranju baze podataka na drugi poslužitelj za logičke.



## <a name="additional-resources"></a>Dodatni resursi

- [Upravljanje prijave](sql-database-manage-logins.md)
- [Povezivanje s bazom podataka SQL s SQL Server Management Studio i obaviti primjer T SQL upita](sql-database-connect-query-ssms.md)
- [Izvoz u BACPAC baze podataka](sql-database-export.md)
- [Pregled Continuity tvrtke](sql-database-business-continuity.md)
- [Dokumentacija SQL baze podataka](https://azure.microsoft.com/documentation/services/sql-database/)


