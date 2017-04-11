<properties
   pageTitle="Upravljanje računalnim power u Azure SQL podataka skladištu (REST) | Microsoft Azure"
   description="Zadatke SQL transakcija (T-SQL) skaliranje iz performanse prilagodbom DWUs. Spremite troškove skaliranje natrag tijekom vremena koje nisu Vršna."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/08/2016"
   ms.author="barbkess;sonyama"/>

# <a name="manage-compute-power-in-azure-sql-data-warehouse-t-sql"></a>Upravljanje računalnim power u skladištu podataka SQL Azure (T-SQL)

> [AZURE.SELECTOR]
- [Pregled](sql-data-warehouse-manage-compute-overview.md)
- [Portal](sql-data-warehouse-manage-compute-portal.md)
- [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
- [OSTALE](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)


Skaliranje Vremensko mjerilo uspješnosti izračunati resurse i memorije da bi odgovarao zahtjeva za promjenom od svoje radno opterećenje. Troškova skaliranja natrag resursima tijekom vremena koje nisu Vršna ili privremeno zaustavljanje računalnim potpuno. 

Zbirku zadataka koristi T-SQL da biste:

- Prikaz trenutne postavke DWU
- Promjena računalnim resursi prilagodbom DWUs

Zaustavljanje baze podataka, odaberite jednu od druge platforme mogućnosti pri vrhu ovog članka.

Dodatne informacije potražite u članku [Upravljanje izračunati power pregled][].

<a name="current-dwu-bk"></a>

## <a name="view-current-dwu-settings"></a>Prikaz trenutne postavke DWU

Da biste pogledali trenutne postavke DWU baze podataka:

1. Otvorite Eksplorer za objekta SQL Server u Visual Studio 2015.
2. Povezivanje s glavnom bazom podataka povezan s logičke baze podataka SQL server.
2. Odaberite prikaz dinamički upravljanje sys.database_service_objectives. Evo jednog primjera: 

```
SELECT
 db.name [Database],
 ds.edition [Edition],
 ds.service_objective [Service Objective]
FROM
 sys.database_service_objectives ds
 JOIN sys.databases db ON ds.database_id = db.database_id
```

<a name="scale-dwu-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute"></a>Promjena veličine računalnim

[AZURE.INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Da biste promijenili u DWUs:


1. Povezivanje s glavnom bazom podataka povezan s poslužiteljem baze podataka SQL logičke.
2. Koristite TSQL iskaz [ALTER baze podataka][] . Sljedeći primjer postavlja cilj razini usluge DW1000 za bazu podataka MySQLDW. 

```Sql
ALTER DATABASE MySQLDW
MODIFY (SERVICE_OBJECTIVE = 'DW1000')
;
```

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Daljnji koraci

Druge zadatke upravljanja potražite u članku [Pregled upravljanja][].

<!--Image references-->

<!--Article references-->
[Service capacity limits]: ./sql-data-warehouse-service-capacity-limits.md
[Pregled upravljanja]: ./sql-data-warehouse-overview-manage.md
[Upravljanje računalnim power pregled]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->

[ZAMIJENI BAZU PODATAKA]: https://msdn.microsoft.com/library/mt204042.aspx


<!--Other Web references-->

[Azure portal]: http://portal.azure.com/
