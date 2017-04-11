<properties
   pageTitle="Praćenje svoje radno opterećenje pomoću DMVs | Microsoft Azure"
   description="Saznajte kako praćenje svoje radno opterećenje pomoću DMVs."
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
   ms.date="10/08/2016"
   ms.author="sonyama;barbkess"/>

# <a name="monitor-your-workload-using-dmvs"></a>Praćenje svoje radno opterećenje pomoću DMVs

U ovom se članku opisuje način korištenja upravljanja dinamičnih prikaza (DMVs) i praćenje svoje radno opterećenje istražiti izvršavanje upita u skladištu podataka za SQL Azure.

## <a name="permissions"></a>Dozvole

Upit DMVs u ovom članku, potrebna je dozvola za STANJE baze podataka za PRIKAZ ili KONTROLU. STANJE baze podataka za PRIKAZ dodjelu obično je Preferirani dozvola kao što je mnogo veća ograničenja.

```sql
GRANT VIEW DATABASE STATE TO myuser;
```

## <a name="monitor-connections"></a>Monitor veze

Sve prijave za SQL Data Warehouse su se prijavili [sys.dm_pdw_exec_sessions][].  U ovom DMV sadrži zadnji 10 000 prijave.  U session_id je primarni ključ i dodijeljuje sekvencijalno za svaku novu prijavu.

```sql
-- Other Active Connections
SELECT * FROM sys.dm_pdw_exec_sessions where status <> 'Closed' and session_id <> session_id();
```

## <a name="monitor-query-execution"></a>Izvršavanje upita monitora

Svi upiti koji se izvršava na SQL Data Warehouse su se prijavili [sys.dm_pdw_exec_requests][].  U ovom DMV sadrži zadnji 10 000 upite koji se izvršava.  Na request_id jedinstveno označava svaki upit, a je primarni ključ za ovaj DMV.  U request_id se dodjeljuju sekvencijalno za svaki novi upit te je mjestu s QID kratica za upit ID-a.  Slanje upita u ovom DMV za dani session_id prikazuje sve upite za dani prijavu.

>[AZURE.NOTE] Pohranjene procedure pomoću više zahtjev za ID-a.  Zahtjev za ID-a dodjeljuju je u slijedu. 

Evo koraka da biste počeli pratiti da biste istražili tarife za izvršavanje upita i vremena za određeni upita.

### <a name="step-1-identify-the-query-you-wish-to-investigate"></a>KORAK 1: Prepoznavanje upit želite li istražiti

```sql
-- Monitor active queries
SELECT * 
FROM sys.dm_pdw_exec_requests 
WHERE status not in ('Completed','Failed','Cancelled')
  AND session_id <> session_id()
ORDER BY submit_time DESC;

-- Find top 10 queries longest running queries
SELECT TOP 10 * 
FROM sys.dm_pdw_exec_requests 
ORDER BY total_elapsed_time DESC;

-- Find a query with the Label 'My Query'
-- Use brackets when querying the label column, as it it a key word
SELECT  *
FROM    sys.dm_pdw_exec_requests
WHERE   [label] = 'My Query';
```

Iz prethodnog rezultata upita, **bilješke ID zahtjev za** upit koji želite istražiti.

Upiti u stanju **obustavljeno** koja se u redu čekanja zbog ograničenja istodobnosti. Tih upita prikazuju u upitu čekanje sys.dm_pdw_waits vrstom UserConcurrencyResourceType. Dodatne informacije o ograničenjima istodobnosti potražite u članku [Upravljanje istodobnosti i radno opterećenje][] . Upite možete pričekati drugih razloga kao što su za zaključavanja objekta.  Ako je upit resursa, potražite u članku [Investigating upita čekanje resursa][] daljnje prema dolje u ovom članku.

Da biste pojednostavnili pronalaženje upita u tablici sys.dm_pdw_exec_requests pomoću [OZNAKA][] možete dodijeliti komentar u upit koji se pretražuje u prikazu sys.dm_pdw_exec_requests.

```sql
-- Query with Label
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query')
;
```

### <a name="step-2-investigate-the-query-plan"></a>KORAK 2: Istražiti plan upita

Pomoću ID zahtjev za učitavanje upita raspodijeljeno SQL (DSQL) tarifu iz [sys.dm_pdw_request_steps][].

```sql
-- Find the distributed query plan steps for a specific query.
-- Replace request_id with value from Step 1.

SELECT * FROM sys.dm_pdw_request_steps
WHERE request_id = 'QID####'
ORDER BY step_index;
```

Kada DSQL plan traje dulje nego je očekivano, uzrok može biti složene plan s DSQL Communicator ili samo jedan korak neobično dugo.  Ako je plan Communicator s nekoliko Premjesti operacija, razmislite o optimiziranje vaše distribucija tablice da biste smanjili premještanje podataka. U članku [tablice raspodjele][] objašnjava zašto podataka mora se premjestiti u rješavanje upita i objašnjava neke strategije raspodjele da biste minimizirali premještanje podataka.

Da biste istražili dodatne detalje o jedan korak stupcu *operation_type* dugoročnih koraka upita i Imajte na umu **Indeks koraka**:

- Nastavite s 3a korak za **SQL operacije**: OnOperation, RemoteOperation, ReturnOperation.
- Nastavite s 3b korak za **Premještanje podataka operacije**: ShuffleMoveOperation "," BroadcastMoveOperation "," TrimMoveOperation "," PartitionMoveOperation "," MoveOperation "," CopyOperation.

### <a name="step-3a-investigate-sql-on-the-distributed-databases"></a>3a KORAK: istražiti SQL na raspodijeljeno baze podataka

Dohvaćanje detalja iz [sys.dm_pdw_sql_requests][], gdje se nalaze informacije izvođenja koraka upita na svim raspodijeljeno baze podataka pomoću ID zahtjev i indeksa korak.

```sql
-- Find the distribution run times for a SQL step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_sql_requests
WHERE request_id = 'QID####' AND step_index = 2;
```

Kada je pokrenut koraka upita, [DBCC PDW_SHOWEXECUTIONPLAN][] može se koristiti za dohvaćanje Procijenjena plan za SQL Server iz predmemorije tarifu sustava SQL Server koraka koji se izvode na određeni distribuciju.

```sql
-- Find the SQL Server execution plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(1, 78);
```

### <a name="step-3b-investigate-data-movement-on-the-distributed-databases"></a>3b KORAK: istražiti premještanje podataka na raspodijeljeno baze podataka

Pomoću ID-a zahtjev i indeksa korak Dohvaćanje informacija o koracima za premještanje podataka radi na svakom raspodjele iz [sys.dm_pdw_dms_workers][].

```sql
-- Find the information about all the workers completing a Data Movement Step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_dms_workers
WHERE request_id = 'QID####' AND step_index = 2;
```

- Provjerite *total_elapsed_time* stupca da biste vidjeli ako je određeni raspodjele traje znatno dulje od ostalih za premještanje podataka.
- Za raspodjelu dugoročnih potvrdite stupcu *rows_processed* je li broj redaka koji su unosa premještena s tom raspodjele znatno veću od ostalih. Ako je tako, to može značiti skew temeljnih podataka.

Ako se izvodi upit [DBCC PDW_SHOWEXECUTIONPLAN][] može koristiti za dohvaćanje Procijenjena plan za SQL Server iz predmemorije za planiranje sustava SQL Server za trenutno izvode SQL korak unutar određenog distribucije.

```sql
-- Find the SQL Server estimated plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(55, 238);
```

<a name="waiting"></a>
## <a name="monitor-waiting-queries"></a>Praćenje upita na čekanju

Otkrijete li da upit ne čini tijeku jer je čekanje resursa, Evo upita koji prikazuje sve resurse čekanje upita.

```sql
-- Find queries 
-- Replace request_id with value from Step 1.

SELECT waits.session_id,
      waits.request_id,  
      requests.command,
      requests.status,
      requests.start_time,  
      waits.type,
      waits.state,
      waits.object_type,
      waits.object_name
FROM   sys.dm_pdw_waits waits
   JOIN  sys.dm_pdw_exec_requests requests
   ON waits.request_id=requests.request_id
WHERE waits.request_id = 'QID####'
ORDER BY waits.object_name, waits.object_type, waits.state;
```

Ako je upit aktivno čekanje resursa iz drugog upita, stanje bit će **AcquireResources**.  Ako je upit potrebni resursi, stanje u kojem će biti **Granted**.

## <a name="next-steps"></a>Daljnji koraci
Dodatne informacije o DMVs potražite u članku [Sistemski prikazi][] .
Dodatne informacije o najbolje prakse potražite u članku [najbolje prakse za SQL Data Warehouse][]

<!--Image references-->

<!--Article references-->
[Manage overview]: ./sql-data-warehouse-overview-manage.md
[Najbolje prakse za SQL Data Warehouse]: ./sql-data-warehouse-best-practices.md
[Sistemski prikazi]: ./sql-data-warehouse-reference-tsql-system-views.md
[Tablica raspodjele]: ./sql-data-warehouse-tables-distribute.md
[Upravljanje istodobnosti i radno opterećenje]: ./sql-data-warehouse-develop-concurrency.md
[Istražuje upita čekanje resursa]: ./sql-data-warehouse-manage-monitor.md#waiting

<!--MSDN references-->
[sys.dm_pdw_dms_workers]: http://msdn.microsoft.com/library/mt203878.aspx
[sys.dm_pdw_exec_requests]: http://msdn.microsoft.com/library/mt203887.aspx
[sys.dm_pdw_exec_sessions]: http://msdn.microsoft.com/library/mt203883.aspx
[sys.dm_pdw_request_steps]: http://msdn.microsoft.com/library/mt203913.aspx
[sys.dm_pdw_sql_requests]: http://msdn.microsoft.com/library/mt203889.aspx
[DBCC PDW_SHOWEXECUTIONPLAN]: http://msdn.microsoft.com/library/mt204017.aspx
[DBCC PDW_SHOWSPACEUSED]: http://msdn.microsoft.com/library/mt204028.aspx
[OZNAKA]: https://msdn.microsoft.com/library/ms190322.aspx
