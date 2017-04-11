<properties
    pageTitle="Upravljanje i otklanjanje poteškoća s bazom podataka rastezanje | Microsoft Azure"
    description="Saznajte kako upravljati i otklanjanje poteškoća s rastezanje baze podataka."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/27/2016"
    ms.author="douglasl"/>

# <a name="manage-and-troubleshoot-stretch-database"></a>Upravljanje i rješavanje problema s rastezanje baze podataka

Upravljanje i otklanjanje poteškoća s bazom podataka rastezanje, poslužite se Alati i metode opisane u ovoj temi.

## <a name="manage-local-data"></a>Upravljanje lokalnim podacima

### <a name="LocalInfo"></a>Dohvaćanje informacija o lokalne baze podataka i tablice omogućen za rastezanje baze podataka
Otvorite kataloga prikazi **sys.databases** i **sys.tables** da biste vidjeli informacije o rastezanje\-omogućeno baze podataka SQL Server i tablice. Dodatne informacije potražite u članku [sys.databases (Transact-SQL)](https://msdn.microsoft.com/library/ms178534.aspx) i [sys.tables (Transact-SQL)](https://msdn.microsoft.com/library/ms187406.aspx).

Da biste pogledali koliko prostora na rastezanje\-omogućeni tablicu koristi u sustavu SQL Server, pokrenite sljedeću naredbu.

 ```tsql
USE <Stretch-enabled database name>;
GO
EXEC sp_spaceused '<Stretch-enabled table name>', 'true', 'LOCAL_ONLY';
GO
 ```
## <a name="manage-data-migration"></a>Upravljanje migracije podataka

### <a name="check-the-filter-function-applied-to-a-table"></a>Provjerite funkciju filtar primjenjuje na tablicu
Otvorite prikaz kataloga **sys.remote\_podataka\_arhiva\_tablice** i provjerite vrijednost u **filtar\_predikata** stupca da biste odredili funkcija koja se koristi rastezanje baze podataka da biste odabrali retke će se migrirati. Ako je vrijednost null cijelu tablicu je uvjete migrirati. Dodatne informacije potražite u članku [sys.remote_data_archive_tables (Transact-SQL)](https://msdn.microsoft.com/library/dn935003.aspx) , a zatim [Odaberite retke za migriranje pomoću funkcije Filtar](sql-server-stretch-database-predicate-function.md).

### <a name="Migration"></a>Provjera statusa migracije podataka
Odaberite zadaci **| Rastezanje | Monitor** za bazu podataka u sustavu SQL Server Management Studio praćenje migracije podataka u rastezanje baze podataka. Dodatne informacije potražite u članku [monitora i otklanjanje poteškoća s migracije podataka (baza podataka rastezanje)](sql-server-stretch-database-monitor.md).

Možete i otvoriti prikaz dinamički upravljanje **sys.dm\_db\_rda\_migracije\_status** da biste vidjeli je prenijela koliko serije i retke podataka.

### <a name="Firewall"></a>Otklanjanje poteškoća s migracije podataka
Prijedloge za otklanjanje poteškoća, potražite u članku [monitora i otklanjanje poteškoća s migracije podataka (baza podataka rastezanje)](sql-server-stretch-database-monitor.md).

## <a name="manage-remote-data"></a>Upravljanje udaljeni podaci

### <a name="RemoteInfo"></a>Dohvaćanje informacija o udaljena baza podataka i tablice korištene rastezanje baze podataka
Otvaranje prikaza kataloga **sys.remote\_podataka\_arhiva\_baze podataka** i **sys.remote\_podataka\_arhiva\_tablice** da biste vidjeli informacije o udaljena baza podataka i tablice u kojoj je pohranjen premještene podatke. Dodatne informacije potražite u članku [sys.remote_data_archive_databases (Transact-SQL)](https://msdn.microsoft.com/library/dn934995.aspx) i [sys.remote_data_archive_tables (Transact-SQL)](https://msdn.microsoft.com/library/dn935003.aspx).

Da biste pogledali koliko prostora tablica rastezanje omogućeno koristi u Azure, pokrenite sljedeću naredbu.

 ```tsql
USE <Stretch-enabled database name>;
GO
EXEC sp_spaceused '<Stretch-enabled table name>', 'true', 'REMOTE_ONLY';
GO
 ```

### <a name="delete-migrated-data"></a>Brisanje migriranim podataka  
Ako želite da biste izbrisali podatke koji su već prenijeli Azure, slijedite korake opisane u [sys.sp_rda_reconcile_batch](https://msdn.microsoft.com/library/mt707768.aspx).

## <a name="manage-table-schema"></a>Upravljanje shemom tablice

### <a name="dont-change-the-schema-of-the-remote-table"></a>Nemojte mijenjati sheme udaljenoj tablici
Ne promjena sheme udaljene Azure tablice koji je povezan s tablicom sustava SQL Server konfiguriran za rastezanje bazu podataka. Točnije, nemojte mijenjati naziv i vrstu podataka stupca. Značajka rastezanje baze podataka omogućuje različite pretpostavke informacije o shemi udaljenoj tablici prema shemi tablice sustava SQL Server. Ako promijenite udaljene shemu baze podataka rastezanje prestane funkcionirati za izmijenjenu tablicu.

### <a name="reconcile-table-columns"></a>Usklađivanje stupaca tablice  
Ako slučajno izbrišete stupaca u tablici udaljene, pokrenite **sp_rda_reconcile_columns** da biste dodali stupce u tablici udaljene koji postoje u na rastezanje\-omogućeno tablica sustava SQL Server, ali ne i u udaljenoj tablici. Dodatne informacije potražite u članku [sys.sp_rda_reconcile_columns](https://msdn.microsoft.com/library/mt707765.aspx).  

  > [!IMPORTANT] Kada **sp_rda_reconcile_columns** obnavlja stupaca koje ste slučajno izbrisali iz udaljenoj tablici, vratiti podatke koje je prethodno u izbrisane stupce.

**sp_rda_reconcile_columns** Izbriši stupce iz tablice udaljene koji postoji u tablici daljinski, ali ne i u na rastezanje\-omogućeno tablica sustava SQL Server. Ako je stupaca u tablici udaljene Azure koji više ne postoje u na rastezanje\-omogućeno SQL Server tablice, te stupaca ne sprječava rastezanje baze podataka radi normalno. Dodatni stupce po želji možete ukloniti ručno.  

## <a name="manage-performance-and-costs"></a>Upravljanje performansama i troškove  

### <a name="troubleshoot-query-performance"></a>Otklanjanje poteškoća s performansama upita
Upiti koji sadrže rastezanje\-omogućeno tablica se očekuje da biste izvršili sporije nego što prije tablice nisu omogućena za rastezanje. Ako je znatno smanjuje performanse upita, pogledajte sljedeće moguće probleme.

-   Poslužitelj za Azure je u različitim zemljopisnom području od SQL Server? Konfiguriranje Azure poslužitelja u istom zemljopisnom području kao SQL Server da biste smanjili latenciju mreže.

-   Mrežni uvjeti možda imaju smanjena. Informacije o nedavne problema ili kvarove zatražite od administratora mreže.

### <a name="increase-azure-performance-level-for-resource-intensive-operations-such-as-indexing"></a>Povećanje razine Azure performanse resursa\-intenzivno operacija kao što su indeksiranja
Kada sastavljanje, ponovno stvaranje ili reorganiziranje indeksa velike tablice koji je konfiguriran za rastezanje bazu podataka, a predviđate podebljanom ispitivanje migriranim podataka u Azure tijekom određenog razdoblja, razmislite o povećanju razinom performansi odgovarajuće Udaljena Azure baza podataka za trajanje operacije. Dodatne informacije o razinama performanse i cijene potražite u članku [SQL Server rastezanje baze podataka cijene](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).

### <a name="you-cant-pause-the-sql-server-stretch-database-service-on-azure"></a>Ne možete zaustaviti servis rastezanje baze podataka za SQL Server Azure  
 Provjerite jeste li odabrali odgovarajuće performanse i cijene razinu. Ako povećati razinu performanse privremeno za resurs\-intenzivno operacija vraćanje na prethodnu razinu nakon dovršetka postupka. Dodatne informacije o razinama performanse i cijene potražite u članku [SQL Server rastezanje baze podataka cijene](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).  

## <a name="change-the-scope-of-queries"></a>Promjena opsega upita  
 Upite odabiranja rastezanje\-omogućeno tablice vratiti lokalnih i udaljenih podatke prema zadanim postavkama. Možete promijeniti opseg upita za sve upite tako da svi korisnici ili samo jedan upita administrator.  

### <a name="change-the-scope-of-queries-for-all-queries-by-all-users"></a>Promjena opsega upita za sve upite tako da svi korisnici  
 Da biste promijenili opseg svi upiti svi korisnici, pokrenite pohranjena procedura **sys.sp_rda_set_query_mode**. Možete smanjiti opseg upita lokalnih podataka, onemogućiti sve upite ili vratiti zadane postavke. Dodatne informacije potražite u članku [sys.sp_rda_set_query_mode](https://msdn.microsoft.com/library/mt703715.aspx).  

### <a name="queryHints"></a>Promjena opsega upita za upit s jednom administrator  
 Da biste promijenili opseg upita jedan član uloge db_owner, dodajte na * *s \( REMOTE_DATA_ARCHIVE_OVERRIDE = *vrijednost* \)** podsjetnik upita da biste iskaza SELECT. Podsjetnik za REMOTE_DATA_ARCHIVE_OVERRIDE upit može sadržavati sljedeće vrijednosti.  
 -   **LOCAL_ONLY**. Upit lokalnih podataka.  

 -   **REMOTE_ONLY**. Udaljeni podaci za upite.  

 -   **STAGE_ONLY**. Upit podatke u tablici kojoj faze baze podataka rastezanje redaka uvjete za migraciju i zadržava migriranim redaka za navedeno razdoblje nakon migracije. Podsjetnik za upit je jedini način upita pripremna tablice.  

Na primjer, sljedeći upit vraća Lokalni rezultati.  

 ```tsql  
 USE <Stretch-enabled database name>;
 GO
 SELECT * FROM <Stretch_enabled table name> WITH (REMOTE_DATA_ARCHIVE_OVERRIDE = LOCAL_ONLY) WHERE ... ;
 GO
```  

## <a name="adminHints"></a>Provjerite administratora ažuriranja i brisanja  
 Po zadanom ne možete AŽURIRATI ili Izbriši retke koji ispunjavaju uvjete za migraciju ili retke koji ste već je migrirati, u na rastezanje\-omogućeno tablice. Kada se morate riješiti problem, član uloge db_owner možete pokrenuti operaciju ažuriranje i brisanje dodavanjem u * *s \( REMOTE_DATA_ARCHIVE_OVERRIDE = *vrijednost* \)** podsjetnik za upit izjavu. Podsjetnik za REMOTE_DATA_ARCHIVE_OVERRIDE upit može sadržavati sljedeće vrijednosti.  
 -   **LOCAL_ONLY**. Ažuriranje i brisanje lokalnih podataka.  

 -   **REMOTE_ONLY**. Ažuriranje i brisanje udaljeni podaci.  

 -   **STAGE_ONLY**. Ažuriranje i brisanje podataka u tablici kojoj faze rastezanje baze podataka redaka uvjete za migraciju i zadržava migriranim redaka za navedeno razdoblje nakon migracije.  

## <a name="see-also"></a>Vidi također

[Monitor rastezanje baze podataka](sql-server-stretch-database-monitor.md)

[Sigurnosno kopiranje baze podataka rastezanje omogućeno](sql-server-stretch-database-backup.md)

[Vraćanje rastezanje omogućeno baze podataka](sql-server-stretch-database-restore.md)
