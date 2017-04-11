<properties
   pageTitle="Nadzor baze podataka Azure SQL pomoću prikaza dinamički upravljanje | Microsoft Azure"
   description="Saznajte kako prepoznati i dijagnosticiranje uobičajene probleme s performansama pomoću prikaza dinamički upravljanje praćenje baza podataka Microsoft Azure SQL."
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/20/2016"
   ms.author="carlrab"/>

# <a name="monitoring-azure-sql-database-using-dynamic-management-views"></a>Nadzor baze podataka SQL Azure pomoću dinamičke Upravljanje prikazima

Baza podataka Microsoft Azure SQL omogućuje podskup dinamički Upravljanje prikazima dijagnosticiranje probleme s performansama, koje se može dogoditi ili blokirani dugoročnih upita, grla resursa, tarife nisku upita i tako dalje. Ova tema sadrži informacije o tome kako otkriti uobičajene probleme s performansama pomoću dinamičke Upravljanje prikazima.

SQL baze podataka djelomično podržava tri kategorije dinamički upravljanje prikaza:

- Prikazi vezane uz baze podataka dinamički upravljanje.
- Prikazi vezane uz izvođenja dinamički upravljanje.
- Prikazi vezane uz transakcije dinamički upravljanje.

Detaljne informacije o dinamički Upravljanje prikazima, potražite u članku [dinamički Upravljanje prikazima i funkcijama (Transact-SQL)](https://msdn.microsoft.com/library/ms188754.aspx) u SQL Server knjige na mreži.

## <a name="permissions"></a>Dozvole

U bazi podataka SQL upita prikaza dinamički upravljanja potrebne dozvole za **STANJE baze podataka za PRIKAZ** . **STANJE baze podataka za PRIKAZ** dozvola vraća informacije o svim objektima u trenutnoj bazi podataka.
Da biste dodijelili dozvolu **STANJE baze podataka za PRIKAZ** korisniku određene baze podataka, pokrenite sljedeći upit:

```GRANT VIEW DATABASE STATE TO database_user; ```

U instance komponente SQL Server lokalnog, dinamični Upravljanje prikazima vratiti informacije o stanju poslužitelja. U bazi podataka SQL mogu vratiti informacije o trenutnoj logičke bazi podataka samo.

## <a name="calculating-database-size"></a>Izračunavanje Veličina baze podataka

Sljedeći upit vraća Veličina baze podataka (u megabajtima):

```
-- Calculates the size of the database.
SELECT SUM(reserved_page_count)*8.0/1024
FROM sys.dm_db_partition_stats;
GO
```

Sljedeći upit vraća veličinu pojedinačne objekte (u megabajtima) u bazi podataka.

```
-- Calculates the size of individual database objects.
SELECT sys.objects.name, SUM(reserved_page_count) * 8.0 / 1024
FROM sys.dm_db_partition_stats, sys.objects
WHERE sys.dm_db_partition_stats.object_id = sys.objects.object_id
GROUP BY sys.objects.name;
GO
```

## <a name="monitoring-connections"></a>Nadzor veze

Prikaz [sys.dm_exec_connections](https://msdn.microsoft.com/library/ms181509.aspx) možete koristiti za dohvaćanje informacija o vezama uspostaviti određene poslužitelj baze podataka SQL Azure i detalje o svaku vezu. Osim toga, prikaz [sys.dm_exec_sessions](https://msdn.microsoft.com/library/ms176013.aspx) koristan je kada Dohvaćanje informacija o sve veze aktivnih korisnika i interna zadatke.
Sljedeći upit dohvaća podatke na trenutnu vezu:

```
SELECT
    c.session_id, c.net_transport, c.encrypt_option,
    c.auth_scheme, s.host_name, s.program_name,
    s.client_interface_name, s.login_name, s.nt_domain,
    s.nt_user_name, s.original_login_name, c.connect_time,
    s.login_time
FROM sys.dm_exec_connections AS c
JOIN sys.dm_exec_sessions AS s
    ON c.session_id = s.session_id
WHERE c.session_id = @@SPID;
```

> [AZURE.NOTE] Prilikom izvršavanja **sys.dm_exec_requests** i **sys.dm_exec_sessions prikaze**, ako imate dozvolu za **STANJE baze podataka za PRIKAZ** na bazu podataka, što vidite sve izvršni sesije na baze podataka. u suprotnom, vidjet ćete samo trenutna sesija.

## <a name="monitoring-query-performance"></a>Praćenje performansi upita

Sporo ili dugo izvodi upiti mogu trošiti značajnim resurse. U ovom se odjeljku pokazuje kako koristiti prikaze dinamički upravljanje da biste otkrili nekoliko uobičajenih upita probleme s performansama. Referencu starije, ali i dalje korisne za rješavanje potražite je u članku [Otklanjanje poteškoća probleme s performansama sustava SQL Server 2008](http://download.microsoft.com/download/D/B/D/DBDE7972-1EB9-470A-BA18-58849DB3EB3B/TShootPerfProbs2008.docx) na Microsoft TechNet.

### <a name="finding-top-n-queries"></a>Traženje N najgornjih upita

Sljedeći primjer vraća informacije o Najčešći upiti pet prema Prosječno vrijeme procesora. U ovom se primjeru objedinjuje upita prema njihovim raspršivanje upit tako da se logički ekvivalentan upita grupirane prema njihovim potrošnje kumulativne resursa.

```
SELECT TOP 5 query_stats.query_hash AS "Query Hash",
    SUM(query_stats.total_worker_time) / SUM(query_stats.execution_count) AS "Avg CPU Time",
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
```

### <a name="monitoring-blocked-queries"></a>Nadzor blokiranih upita

Sporo ili dugoročnih upita možete priložiti potrošnje preopterećenje resursa i biti consequence blokiranih upita. Blokiranje uzroka može biti dizajniranje nisku aplikacije, tarife neispravni upita, a zatim nedostatak korisne indekse i tako dalje. Prikaz sys.dm_tran_locks možete koristiti da biste dobili informacije o trenutnom zaključavanja aktivnosti u bazi podataka SQL Azure. Na primjer kod, potražite u članku [sys.dm_tran_locks (Transact-SQL)](https://msdn.microsoft.com/library/ms190345.aspx) u SQL Server knjige na mreži.

### <a name="monitoring-query-plans"></a>Nadzor tarife upita

Tarifu Neučinkovit upita mogu povećati potrošnju procesora. Sljedeći primjer koristi prikaz [sys.dm_exec_query_stats](https://msdn.microsoft.com/library/ms189741.aspx) da biste utvrdili koji upit koristi najčešće kumulativne procesora.

```
SELECT
    highest_cpu_queries.plan_handle,
    highest_cpu_queries.total_worker_time,
    q.dbid,
    q.objectid,
    q.number,
    q.encrypted,
    q.[text]
FROM
    (SELECT TOP 50
        qs.plan_handle,
        qs.total_worker_time
    FROM
        sys.dm_exec_query_stats qs
    ORDER BY qs.total_worker_time desc) AS highest_cpu_queries
    CROSS APPLY sys.dm_exec_sql_text(plan_handle) AS q
ORDER BY highest_cpu_queries.total_worker_time DESC;
```

## <a name="see-also"></a>Vidi također

[Uvod u SQL baze podataka](sql-database-technical-overview.md)
