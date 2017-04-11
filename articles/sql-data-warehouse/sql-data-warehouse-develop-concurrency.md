<properties
   pageTitle="Upravljanje istodobnosti i radno opterećenje u SQL Data Warehouse | Microsoft Azure"
   description="Objašnjenje istodobnosti i radno opterećenje upravljanje u Azure SQL Data Warehouse za razvoj rješenja."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/27/2016"
   ms.author="sonyama;barbkess;jrj"/>

# <a name="concurrency-and-workload-management-in-sql-data-warehouse"></a>Upravljanje istodobnosti i radno opterećenje u SQL Data Warehouse

Izlaganje predvidljivi performansi na razini, Microsoft Azure SQL Data Warehouse pomaže kontrolirati istodobnosti razine i alokacija resursa, kao što su memorije i procesora prioritet. U ovom se članku predstavlja koncepata istodobnosti i radno opterećenje upravljanje kojemu se objašnjava kako obje značajke implementirana te kako ih možete kontrolirati u skladištu podataka. Upravljanje radno opterećenje SQL Data Warehouse namijenjen je da biste lakše podržava okruženjima s više korisnika. To nije namijenjen radnih opterećenja više klijenta.

## <a name="concurrency-limits"></a>Ograničenja istodobnosti

SQL Data Warehouse omogućuje do 1024 Istodobni veze. Sve veze za 1024 istovremeno mogu slati upiti. Međutim, da biste optimizirali propusnost SQL Data Warehouse možda red čekanja neki upiti da biste bili sigurni da svaki upit prima minimalnog memorije Dodjela. Stavljanje pojavljuje se na vrijeme izvršavanja upita. Stavljanja upiti kada su ograničenja istodobnosti, SQL Data Warehouse povećati ukupnu propusnost tako da jamči da Aktivni upiti dobiti pristup kritično potrebne memorijskih resursa.  

Ograničenja istodobnosti određena su dva pojma: *Istodobni upita* i *slobodnih istodobnosti*. Za upit za izvršavanje ga morate izvršiti unutar ograničenja istodobnosti upita i dodjela istodobnosti vremensko razdoblje.

- Istodobni upiti su upiti koji se izvodi u isto vrijeme. SQL Data Warehouse podržava najviše 32 Istodobni upita na veće DWU.
- Istodobnosti slobodnih su dodijeliti na temelju DWU. Svaki 100 DWU nudi 4 slobodnih istodobnosti. Na primjer, u DW100 dodjeljuje 4 istodobnosti slobodnih i DW1000 dodjeljuje 40. Svaki upit troši jedan ili više istodobnosti slobodnih, ovisi o [resursa klasa](#resource-classes) upita. Upiti koji se izvodi u predmete resursa smallrc zauzimaju jedan istodobnosti vremensko razdoblje. Upiti koji se izvodi u noviji predmete resursa zauzimaju više slobodnih istodobnosti.

U sljedećoj su tablici opisana ograničenja za istodobni upita i istodobnosti slobodnih s različitim veličinama DWU.

### <a name="concurrency-limits"></a>Ograničenja istodobnosti

|  DWU   | Max Istodobni upita  | Istodobnosti slobodnih dodijeliti |
| :----  | :---------------------: | :-------------------------: |
| DW100  |            4            |                4            |
| DW200  |            8            |                8            |
| DW300  |           12            |               12            |
| DW400  |           16            |               16            |
| DW500  |           20            |               20            |
| DW600  |           24            |               24            |
| DW1000 |           32            |               40            |
| DW1200 |           32            |               48            |
| DW1500 |           32            |               60            |
| DW2000 |           32            |               80            |
| DW3000 |           32            |              120            |
| DW6000 |           32            |              240            |

Kada je jedan od ovih pragovi ispunjen, nove upite su u redu čekanja i izvršava na temelju first-out prvog-u.  Završi u upitima i broj upita i slobodnih pada ispod ograničenja, u redu čekanja upita objavljivanja. 

> [AZURE.NOTE]  *Odabir* upita izvršavanja isključivo na dinamički upravljanje prikaze (DMVs) ili prikaze kataloga mjerodavni su ne po bilo kojem istodobnosti ograničenja. Možete nadzirati sustava bez obzira na broj upita koji se izvršavaju na njemu.

## <a name="resource-classes"></a>Klase resursa

Resurs klasa iz registra pomoći odrediti dodjelu memorije i procesora ciklusa dodijeljen upita. Četiri klase resursa možete dodijeliti korisnika u obliku *uloge baze podataka*. Klase četiri resursa su **smallrc**, **mediumrc**, **largerc**i **xlargerc**. Korisnici u smallrc ponudit će vam manjih količina memorije i možete iskoristiti veći istodobnosti. Nasuprot tome, korisnici dodijeljeni xlargerc ponudit će vam velike količine memorije i stoga manje upite možete pokrenuti istovremeno.

Prema zadanim postavkama svaki korisnik pripada klase small resursa, smallrc. Postupak `sp_addrolemember` se koristi da biste povećali predmete resursa i `sp_droprolemember` se koristi da biste smanjili predmete resursa. Tu naredbu, na primjer, želite povećati klasa resursa loaduser za largerc:

```sql
EXEC sp_addrolemember 'largerc', 'loaduser'
```

Dobro je trajno Dodjela korisnika u razredu resursa umjesto promjene svoje predmete resursa. Ako, na primjer, opterećenje grupirani columnstore tablice stvorite bolje kvalitete indeksi kada dodijeliti dodatnu memoriju. Da biste bili sigurni da opterećenje imati pristup veći memorije, stvaranje korisnika namijenjene učitavanja podataka i trajno ovom korisniku dodijelite veći predmete resursa.

Postoji nekoliko vrsta upita koji im veće dodjelu memorije. Sustav će zanemariti svoje predmete Dodjela resursa i uvijek pokrenuti tih upita u razredu small resursa umjesto toga. Ako tih upita uvijek pokrenuti u razredu small resursa, oni pokrenuti kada istodobnosti slobodnih su u odjeljku tlaka i oni neće zauzeti više slobodnih od potrebne. Dodatne informacije potražite u odjeljku [iznimke za predmete resursa](#query-exceptions-to-concurrency-limits) .

Nekoliko dodatne informacije o predmete resursa:

- Da biste promijenili predmete resursa korisnika potrebna je dozvola za *ALTER uloge* .  
- Iako jednu ili više veći klase resursa možete dodati korisnika, korisnici će se na atribute najveće klase resursa na koji se dodjeljuju. To jest, ako korisnik je dodijeljen mediumrc i largerc, klasu noviji resursa (largerc) je klasu resursa koji će se na snazi.  
- Nije moguće promijeniti klasa resursa korisnika administratora sustava.

Primjer s detaljne potražite u članku [Promjena korisničkog resursa predmete primjer](#changing-user-resource-class-example).

## <a name="memory-allocation"></a>Dodjela memorije

Postoje argumente za i protiv za povećanje korisničke resursa predmete. Povećanje predmete resursa za korisnika steći njihove upita programa access za dodatnu memoriju koji može značiti upita brže izvoditi.  Međutim, veća resursa klase smanjite broj Istodobni upite koji se može pokrenuti. Ovo je trade-off između dodjeljivanje velike količine memorije u jednom upit ili dopuštanja druge upite koji trebati alokacija memorije, da biste pokrenuli istodobno. Jedan korisnik dobiva visoke alokacija memorije za upit, drugi korisnici neće imati pristup tom isti memorije za izvođenje upita.

U sljedećoj su tablici karata u memorije svaki distribuciji klase DWU i resursa.

### <a name="memory-allocations-per-distribution-mb"></a>Alokacija memorije po raspodjele (MB)

|  DWU   | smallrc | mediumrc | largerc | xlargerc |
| :----- | :-----: | :------: | :-----: | :------: |
| DW100  |   100   |    100   |    200  |    400   |
| DW200  |   100   |    200   |    400  |    800   |
| DW300  |   100   |    200   |    400  |    800   |
| DW400  |   100   |    400   |    800  |  1,600   |
| DW500  |   100   |    400   |    800  |  1,600   |
| DW600  |   100   |    400   |    800  |  1,600   |
| DW1000 |   100   |    800   |  1,600  |  3,200   |
| DW1200 |   100   |    800   |  1,600  |  3,200   |
| DW1500 |   100   |    800   |  1,600  |  3,200   |
| DW2000 |   100   |  1,600   |  3,200  |  6,400   |
| DW3000 |   100   |  1,600   |  3,200  |  6,400   |
| DW6000 |   100   |  3,200   |  6,400  |  12,800  |

Iz prethodne tablice, vidjet ćete upit pokrenuti na DW2000 u razredu resursa xlargerc želite imati pristup 6,400 MB memorije u svakom od 60 raspodijeljeno baze podataka.  U SQL Data Warehouse postoje 60 distribucije. Dakle, da biste izračunali Dodjela ukupni memorije za upit u razredu navedeni resursa, gornje vrijednosti mora se množiti 60.

### <a name="memory-allocations-system-wide-gb"></a>Memorije alokacija sistemskih (GB)

|  DWU   | smallrc | mediumrc | largerc | xlargerc |
| :----- | :-----: | :------: | :-----: | :------: |
| DW100  |    6    |    6     |    12   |    23    |
| DW200  |    6    |    12    |    23   |    47    |
| DW300  |    6    |    12    |    23   |    47    |
| DW400  |    6    |    23    |    47   |    94    |
| DW500  |    6    |    23    |    47   |    94    |
| DW600  |    6    |    23    |    47   |    94    |
| DW1000 |    6    |    47    |    94   |   188    |
| DW1200 |    6    |    47    |    94   |   188    |
| DW1500 |    6    |    47    |    94   |   188    |
| DW2000 |    6    |    94    |   188   |   375    |
| DW3000 |    6    |    94    |   188   |   375    |
| DW6000 |    6    |   188    |   375   |   750    |

Iz ove tablice alokacija sistemskih memorije vidjet ćete upit pokrenuti na DW2000 u razredu resursa xlargerc je dodijeliti Ukupno 375 GB memorije (6,400 MB * 60 distribucija / 1024 da biste pretvorili GB) putem cijelosti SQL Data Warehouse.

## <a name="concurrency-slot-consumption"></a>Potrošnje istodobnosti vremensko razdoblje

SQL Data Warehouse daje dodatnu memoriju za upite koji se izvodi u noviji klase resursa. Fixed resurs je memorije.  Zbog toga više memorije dodijeljeno po upitu, manje Istodobni upita možete izvršiti. U sljedećoj su tablici reiterates sve prethodne koncepta u jednom prikazu koji prikazuje broj istodobnosti slobodnih dostupne po DWU i slobodnih troše svaki predmete resursa.

### <a name="allocation-and-consumption-of-concurrency-slots"></a>Dodjela i potrošnje istodobnosti slobodnih

|  DWU   | Najveći broj Istodobni upita  | Istodobnosti slobodnih dodijeliti | Slobodnih koristi smallrc |  Slobodnih koristi mediumrc |  Slobodnih koristi largerc |  Slobodnih koristi xlargerc |
| :----  | :---------------------: | :-------------------------: | :-----: | :------: | :-----: | :------: |
| DW100  |            4            |                4            |    1    |     1    |    2    |    4     |
| DW200  |            8            |                8            |    1    |     2    |    4    |    8     |
| DW300  |           12            |               12            |    1    |     2    |    4    |    8     |
| DW400  |           16            |               16            |    1    |     4    |    8    |   16     |
| DW500  |           20            |               20            |    1    |     4    |    8    |   16     |
| DW600  |           24            |               24            |    1    |     4    |    8    |   16     |
| DW1000 |           32            |               40            |    1    |     8    |   16    |   32     |
| DW1200 |           32            |               48            |    1    |     8    |   16    |   32     |
| DW1500 |           32            |               60            |    1    |     8    |   16    |   32     |
| DW2000 |           32            |               80            |    1    |    16    |   32    |   64     |
| DW3000 |           32            |              120            |    1    |    16    |   32    |   64     |
| DW6000 |           32            |              240            |    1    |    32    |   64    |  128     |


Iz ove tablice možete vidjeti taj SQL Data Warehouse izvodi kao DW1000 dodjeljuje maksimalno 32 Istodobni upita i ukupno 40 slobodnih istodobnosti. Ako su svi korisnici pokrenuti u smallrc, 32 Istodobni upita će biti dopuštena jer svaki upit želite trošiti 1 istodobnosti vremensko razdoblje. Ako sve korisnike u DW1000 pokrećete u mediumrc, svaki upit će se dodijeliti 800 MB po distribucije za dodjelu ukupni memorije 47 GB po upitu i istodobnosti će biti ograničeno na 5 korisnika (40 istodobnosti slobodnih / 8 slobodnih po korisniku mediumrc).

## <a name="query-importance"></a>Važnost upita

SQL Data Warehouse implementira resursa klase pomoću radno opterećenje grupa. Postoje Ukupno osam radno opterećenje grupe koji nadzire ponašanje klase resursa preko različitih veličina DWU. Za sve DWU SQL Data Warehouse koristi samo četiri od osam radno opterećenje grupe. To vam odgovara jer svake grupe radno opterećenje je dodijeljen jednu od četiri klase resursa: smallrc mediumrc, largerc, ili xlargerc. Važnost grupe radno opterećenje je da neke od tih grupa radno opterećenje postaviti na višu *važnost*. Važnost koristi se za procesora zakazivanja. Upiti koji su pokrenuti s visokom važnošću će dobiti dodatne triput procesora kreće od onih s srednje važnosti. Zbog toga istodobnosti vremensko razdoblje mapiranja određivanje prioriteta procesora. Kada je upit troši 16 ili više slobodnih, pokreće visoke važnosti.

Sljedeća tablica prikazuje važnost mapiranja za svaku grupu radno opterećenje.

### <a name="workload-group-mappings-to-concurrency-slots-and-importance"></a>Radno opterećenje grupe mapiranja istodobnosti slobodnih i važnost

| Radno opterećenje grupe | Mapiranje istodobnosti vremensko razdoblje | MB / distribuciju. | Mapiranje važnost |
| :-------------- | :----------------------: | :---------------: | :----------------- |
| SloDWGroupC00   |            1             |         100       |       Srednje       |
| SloDWGroupC01   |            2             |         200       |       Srednje       |
| SloDWGroupC02   |            4             |         400       |       Srednje       |
| SloDWGroupC03   |            8             |         800       |       Srednje       |
| SloDWGroupC04   |           16             |       1,600       |       Visoke         |
| SloDWGroupC05   |           32             |       3,200       |       Visoke         |
| SloDWGroupC06   |           64             |       6,400       |       Visoke         |
| SloDWGroupC07   |          128             |      12,800       |       Visoke         |

Iz grafikona **dodijeljeni i potrošnje istodobnosti slobodnih** vidjet ćete da je DW500 koristi 1, 4, 8 ili 16 istodobnosti slobodnih smallrc, mediumrc, largerc i xlargerc, odnosno. Možete potražiti te se vrijednosti u prethodnom grafikona da biste pronašli važnost za svaku klasu resursa.

### <a name="dw500-mapping-of-resource-classes-to-importance"></a>Mapiranje DW500 klase resursa na važnost

| Klase resursa | Radno opterećenje grupe | Istodobnosti slobodnih koristi | MB / distribuciju. | Važnost |
| :------------- | :------------- | :--------------------: | :---------------: | :--------- |
| smallrc        | SloDWGroupC00  |           1            |         100       |   Srednje   |
| mediumrc       | SloDWGroupC02  |           4            |         400       |   Srednje   |
| largerc        | SloDWGroupC03  |           8            |         800       |   Srednje   |
| xlargerc       | SloDWGroupC04  |          16            |        1,600      |   Visoke     |


Sljedeći upit DMV možete koristiti da biste vidjeli razlike u Dodjela resursa memorije detaljno iz perspektive governor resursa ili da biste analizirali aktivne i povijesnim korištenje grupa za radno opterećenje pri otklanjanju.

```sql
WITH rg
AS
(   SELECT  
     pn.name                        AS node_name
    ,pn.[type]                      AS node_type
    ,pn.pdw_node_id                 AS node_id
    ,rp.name                        AS pool_name
    ,rp.max_memory_kb*1.0/1024              AS pool_max_mem_MB
    ,wg.name                        AS group_name
    ,wg.importance                  AS group_importance
    ,wg.request_max_memory_grant_percent        AS group_request_max_memory_grant_pcnt
    ,wg.max_dop                     AS group_max_dop
    ,wg.effective_max_dop               AS group_effective_max_dop
    ,wg.total_request_count             AS group_total_request_count
    ,wg.total_queued_request_count          AS group_total_queued_request_count
    ,wg.active_request_count                AS group_active_request_count
    ,wg.queued_request_count                AS group_queued_request_count
    FROM    sys.dm_pdw_nodes_resource_governor_workload_groups wg
    JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools rp    
            ON  wg.pdw_node_id  = rp.pdw_node_id
            AND wg.pool_id      = rp.pool_id
    JOIN    sys.dm_pdw_nodes pn
            ON  wg.pdw_node_id  = pn.pdw_node_id
    WHERE   wg.name like 'SloDWGroup%'
        AND     rp.name = 'SloDWPool'
)
SELECT  pool_name
,       pool_max_mem_MB
,       group_name
,       group_importance
,       (pool_max_mem_MB/100)*group_request_max_memory_grant_pcnt AS max_memory_grant_MB
,       node_name
,       node_type
,       group_total_request_count
,       group_total_queued_request_count
,       group_active_request_count
,       group_queued_request_count
FROM    rg
ORDER BY
    node_name
,   group_request_max_memory_grant_pcnt
,   group_importance
;
```

## <a name="queries-that-honor-concurrency-limits"></a>Upiti koji poštovati ograničenja istodobnosti

Većina upita mjerodavni su po klase resursa. Oba Istodobni upita i istodobnosti vremensko razdoblje pragovi morate stao tih upita. Korisnik ne možete odabrati da biste izuzeli upit iz modela istodobnosti vremensko razdoblje.

Da biste reiterate, sljedeće naredbe poštovati klase resursa:

- UMETANJE ODABERITE
- AŽURIRANJE
- BRISANJE
- Odaberite (prilikom postavljanja upita korisniku tablica)
- IZVOĐENJE ALTER INDEKSA
- ISKAZ ALTER INDEKS REORGANIZIRAJ
- IZVOĐENJE ALTER TABLICE
- STVARANJE INDEKSA
- STVARANJE INDEKSA GRUPIRANI COLUMNSTORE
- STVARANJE TABLICE KAO ODABERITE (CTAS)
- Učitavanje podataka
- Operacija premještanje podataka obavljaju tako da na podataka premještanje servisa (DMS)

## <a name="query-exceptions-to-concurrency-limits"></a>Upit iznimke istodobnosti ograničenja

Neki upiti poštovati predmete resurs koji je dodijeljen korisnika. Ove iznimke ograničenja istodobnosti nastaju kada memorijskih resursa koja su potrebna za određene naredbe niske, često su jer je naredba operacija metapodataka. Da biste izbjegli veće memorije alokacija za upite koji nikad ne morate ih je cilja te iznimaka. U tim slučajevima zadanu klasu small resursa (smallrc) uvijek koristi se bez obzira na to predmete stvarni resursa Dodijeljeno korisniku. Na primjer, `CREATE LOGIN` uvijek funkcionirat će u smallrc. Resurse potrebne za ispunjavanje ovog postupka su nisku, pa provjerite odgovara da biste uključili upit u modelu istodobnosti vremensko razdoblje.  Tih upita i ne ograničen istodobnosti ograničenje 32 korisnika, neograničen broj tih upita možete pokrenuti prema gore u sesiju ograničenje od 1024 sesije.

Sljedeće naredbe poštovati klase resursa:

- Stvaranje i ISPUSTITE TABLICE
- ISKAZ ALTER TABLICA... Promjena, PODIJELJENI ili SPAJANJE PARTITION
- ONEMOGUĆIVANJE ALTER INDEKSA
- ODBACIVANJE INDEKSA
- Stvaranje, ažuriranje i ISPUSTITE STATISTIKA
- IZREŽITE TABLICE
- ISKAZ ALTER AUTORIZACIJE
- STVARANJE PRIJAVA
- Stvaranje, ALTER ili DROP USER
- Stvaranje, ALTER ili ISPUSTITE postupak
- Stvaranje i ISPUSTITE PRIKAZ
- UMETANJE VRIJEDNOSTI
- Odabir iz prikaza sustava i DMVs
- U ČLANKU SE OBJAŠNJAVA
- DBCC

<!--
Removed as these two are not confirmed / supported under SQLDW
- CREATE REMOTE TABLE AS SELECT
- CREATE EXTERNAL TABLE AS SELECT
- REDISTRIBUTE
-->

## <a name="change-a-user-resource-class-example"></a>Promjena klase primjer resursa za korisnika

1. **Prijava stvaranje:** Otvaranje veze s **glavnom** bazom podataka na SQL server hostira baze podataka SQL Data Warehouse i izvršiti sljedeće naredbe.

    ```sql
    CREATE LOGIN newperson WITH PASSWORD = 'mypassword';
    CREATE USER newperson for LOGIN newperson;
    ```

    > [AZURE.NOTE] Preporučuje se u da biste stvorili korisnika u glavnom bazom podataka za Azure SQL Data Warehouse korisnike. Stvaranje korisnika u matrici korisnicima omogućuje Prijava pomoću alata kao što je SSMS bez navođenja naziv baze podataka.  Omogućuje da koristite explorer objekt da biste prikazali sve baze podataka na SQL server.  Dodatne informacije o stvaranju i upravljanje korisnicima potražite u članku [sigurne baze podataka u SQL Data Warehouse][].

2. **Data Warehouse stvaranje SQL korisnika:** Otvorite vezu s bazom podataka sustava **SQL Data Warehouse** , a zatim pokrenite sljedeću naredbu.

    ```sql
    CREATE USER newperson FOR LOGIN newperson;
    ```

3. **Dodjela dozvola:** U sljedećem primjeru daje `CONTROL` **SQL Data Warehouse** bazu podataka. `CONTROL`u bazi podataka razina je ekvivalent db_owner u sustavu SQL Server.

    ```sql
    GRANT CONTROL ON DATABASE::MySQLDW to newperson;
    ```

4. **Povećati resursa klasa:** Koristite sljedeći upit za dodavanje korisnika u grupu veći uloga upravljanje radno opterećenje.

    ```sql
    EXEC sp_addrolemember 'largerc', 'newperson'
    ```

5. **Smanji resursa klasa:** Da biste korisnika uklonili iz uloge za upravljanje radno opterećenje koristite sljedeći upit.

    ```sql
    EXEC sp_droprolemember 'largerc', 'newperson';
    ```

    > [AZURE.NOTE] Nije moguće uklanjanje korisnika iz smallrc.

## <a name="queued-query-detection-and-other-dmvs"></a>U redu čekanja upita otkrivanje i druge DMVs

Možete koristiti u `sys.dm_pdw_exec_requests` DMV za prepoznavanje upiti koji čekaju u redu čekanja za istodobnosti. Upiti čekanje vremensko razdoblje istodobnosti će imati status **obustavljeno**.

```sql
SELECT   r.[request_id]              AS Request_ID
        ,r.[status]              AS Request_Status
        ,r.[submit_time]             AS Request_SubmitTime
        ,r.[start_time]              AS Request_StartTime
        ,DATEDIFF(ms,[submit_time],[start_time]) AS Request_InitiateDuration_ms
        ,r.resource_class                         AS Request_resource_class
FROM    sys.dm_pdw_exec_requests r;
```

Uloge za upravljanje radno opterećenje moguće je prikazati s `sys.database_principals`.

```sql
SELECT  ro.[name]           AS [db_role_name]
FROM    sys.database_principals ro
WHERE   ro.[type_desc]      = 'DATABASE_ROLE'
AND     ro.[is_fixed_role]  = 0;
```

Sljedeći upit prikazuje koje uloge dodijeljene svakom korisniku.

```sql
SELECT   r.name AS role_principal_name
        ,m.name AS member_principal_name
FROM    sys.database_role_members rm
JOIN    sys.database_principals AS r            ON rm.role_principal_id     = r.principal_id
JOIN    sys.database_principals AS m            ON rm.member_principal_id   = m.principal_id
WHERE   r.name IN ('mediumrc','largerc', 'xlargerc');
```

SQL Data Warehouse sastoji se od sljedećih Pričekajte vrste:

- **LocalQueriesConcurrencyResourceType**: upite koji sjesti izvan framework istodobnosti vremensko razdoblje. Upiti DMV i sustava funkcionira kao što su `SELECT @@VERSION` Primjeri lokalne upita.
- **UserConcurrencyResourceType**: upite koji sjesti unutar framework istodobnosti vremensko razdoblje. Upite odabiranja krajnjeg korisnika tablica predstavlja primjeri koji će koristiti ovu vrstu resursa.
- **DmsConcurrencyResourceType**: čeka koja je rezultat operacije premještanja podataka.
- **BackupConcurrencyResourceType**: Ova čekanja označava da baze podataka se stvara sigurnosnu kopiju. Maksimalna vrijednost za tu vrstu resursa je 1. Ako u isto vrijeme tražene više kopija, red na druge će čekanja.

Na `sys.dm_pdw_waits` DMV može se koristiti za potražite u članku odabir resursa zahtjev čekanje.

```sql
SELECT  w.[wait_id]
,       w.[session_id]
,       w.[type]            AS Wait_type
,       w.[object_type]
,       w.[object_name]
,       w.[request_id]
,       w.[request_time]
,       w.[acquire_time]
,       w.[state]
,       w.[priority]
,   SESSION_ID()            AS Current_session
,   s.[status]          AS Session_status
,   s.[login_name]
,   s.[query_count]
,   s.[client_id]
,   s.[sql_spid]
,   r.[command]         AS Request_command
,   r.[label]
,   r.[status]          AS Request_status
,   r.[submit_time]
,   r.[start_time]
,   r.[end_compile_time]
,   r.[end_time]
,   DATEDIFF(ms,r.[submit_time],r.[start_time])     AS Request_queue_time_ms
,   DATEDIFF(ms,r.[start_time],r.[end_compile_time])    AS Request_compile_time_ms
,   DATEDIFF(ms,r.[end_compile_time],r.[end_time])      AS Request_execution_time_ms
,   r.[total_elapsed_time]
FROM    sys.dm_pdw_waits w
JOIN    sys.dm_pdw_exec_sessions s  ON w.[session_id] = s.[session_id]
JOIN    sys.dm_pdw_exec_requests r  ON w.[request_id] = r.[request_id]
WHERE   w.[session_id] <> SESSION_ID();
```

Na `sys.dm_pdw_resource_waits` DMV prikazuje samo čekanje resursa koji dani upit. Vrijeme čekanja resursa samo mjeri vrijeme čekanja za resursa koji će se pružati, umjesto vrijeme čekanja signala, što je vrijeme potrebno za podlozi SQL Server da biste zakazali upita na Procesor.

```sql
SELECT  [session_id]
,       [type]
,       [object_type]
,       [object_name]
,       [request_id]
,       [request_time]
,       [acquire_time]
,       DATEDIFF(ms,[request_time],[acquire_time])  AS acquire_duration_ms
,       [concurrency_slots_used]                    AS concurrency_slots_reserved
,       [resource_class]
,       [wait_id]                                   AS queue_position
FROM    sys.dm_pdw_resource_waits
WHERE   [session_id] <> SESSION_ID();
```

Na `sys.dm_pdw_wait_stats` DMV moguće je koristiti za analizu prijašnje trend čekanje.

```sql
SELECT  w.[pdw_node_id]
,       w.[wait_name]
,       w.[max_wait_time]
,       w.[request_count]
,       w.[signal_time]
,       w.[completed_count]
,       w.[wait_time]
FROM    sys.dm_pdw_wait_stats w;
```

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o upravljanju korisnicima baze podataka i sigurnost, potražite u članku [sigurne baze podataka u SQL Data Warehouse][]. Dodatne informacije o načinu veće resursu klase možete poboljšati kvalitetu grupirani columnstore indeks, pročitajte članak [Rebuilding indeksa radi poboljšanja kvalitete segmenta].

<!--Image references-->

<!--Article references-->
[Zaštita baze podataka u skladištu podataka za SQL]: ./sql-data-warehouse-overview-manage-security.md
[Ponovno stvaranje indeksa radi poboljšanja kvalitete segmenta]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Zaštita baze podataka u skladištu podataka za SQL]: ./sql-data-warehouse-overview-manage-security.md

<!--MSDN references-->
[Managing Databases and Logins in Azure SQL Database]:https://msdn.microsoft.com/library/azure/ee336235.aspx

<!--Other Web references-->
