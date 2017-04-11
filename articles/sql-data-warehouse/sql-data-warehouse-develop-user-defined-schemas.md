<properties
   pageTitle="Korisnički definirane sheme u SQL Data Warehouse | Microsoft Azure"
   description="Savjeti za korištenje Transact-SQL sheme programa Azure SQL Data Warehouse za razvoj rješenja."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/14/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="user-defined-schemas-in-sql-data-warehouse"></a>Korisnički definirane sheme u SQL Data Warehouse

Tradicionalni podataka skladištima često koriste zasebnim bazama podataka da biste stvorili granice aplikacije na temelju radno opterećenje, domene ili sigurnosne. Tradicionalni skladištu podataka sustava SQL Server, na primjer, mogu sadržavati pripremna baze podataka, skladištu bazu podataka i neke podatke čavanje baze podataka. U ovom topologiji svaku bazu podataka pristajete radno opterećenje i sigurnost granicu arhitekture.

Za razliku od toga SQL Data Warehouse pokreće radno opterećenje skladištu čitavog sadržaja iz jedne baze podataka. Unakrsni baze podataka spojeva nije dopušteno. Stoga SQL Data Warehouse očekuje sve tablice koje se koristi u skladištu da bi se nalaziti unutar jedne baze podataka.

> [AZURE.NOTE] SQL Data Warehouse ne podržava Unakrsni upiti baze podataka bilo koje vrste. Zbog toga implementacije skladištu podataka pod utjecajem ovaj uzorak morat ćete biti izmijenjenim.

## <a name="recommendations"></a>Preporuke

To su preporuke za konsolidiranje radnih opterećenja, sigurnost, domene i funkcionalne granice pomoću korisnički definirane sheme

1. Korištenje jedne baze podataka SQL Data Warehouse da biste pokrenuli svoje radno opterećenje skladištu čitavog sadržaja
2. Konsolidacija postojeće okruženje skladištu podataka za korištenje jedne baze podataka SQL Data Warehouse
3. Korištenje **korisnički definirane sheme** omogućuju granicu prethodno implementirati pomoću baze podataka.

Korisnički definirane sheme već iskorištene prethodno pa koristite li čisto. Jednostavno koristiti stari naziv baze podataka kao osnovu za vaš korisnički definirane sheme u bazi podataka za SQL Data Warehouse.

Ako su sheme korišteni pa imate nekoliko mogućnosti:

1. Uklanjanje imena naslijeđene sheme i počnite Osvježi
2. Zadržavanje nazive naslijeđene sheme tako da prije komentara Navedite naziv naslijeđene sheme za naziv tablice
3. Zadržati nazive naslijeđene sheme implementacijom prikaza preko tablice u dodatne sheme da biste ponovno stvorili stare sheme strukturu.

> [AZURE.NOTE] Na prvom provjere mogućnost 3 možda čini se kao mogućnost za većinu životopisa. No u devil se detalje. Prikazi su za čitanje samo u SQL Data Warehouse. Izmjene podataka ili tablice morate provesti prema Osnovna tablica. Mogućnost 3 predstavlja i u mapi Zajednički dokumenti prikaza u sustavu. Možda ćete to dati neke dodatne misli ako se već koriste prikazi u arhitekturi.


### <a name="examples"></a>Primjeri:

Implementacija korisnički definirane shema na temelju naziva baze podataka

```sql
CREATE SCHEMA [stg]; -- stg previously database name for staging database
GO
CREATE SCHEMA [edw]; -- edw previously database name for the data warehouse
GO
CREATE TABLE [stg].[customer] -- create staging tables in the stg schema
(       CustKey BIGINT NOT NULL
,       ...
);
GO
CREATE TABLE [edw].[customer] -- create data warehouse tables in the edw schema
(       CustKey BIGINT NOT NULL
,       ...
);
```

Zadržati naslijeđene sheme naziva tako da prije komentara navedite ih nazivu tablice. Korištenje sheme za radno opterećenje granicu.

```sql
CREATE SCHEMA [stg]; -- stg defines the staging boundary
GO
CREATE SCHEMA [edw]; -- edw defines the data warehouse boundary
GO
CREATE TABLE [stg].[dim_customer] --pre-pend the old schema name to the table and create in the staging boundary
(       CustKey BIGINT NOT NULL
,       ...
);
GO
CREATE TABLE [edw].[dim_customer] --pre-pend the old schema name to the table and create in the data warehouse boundary
(       CustKey BIGINT NOT NULL
,       ...
);
```

Zadržavanje imena naslijeđene sheme pomoću prikaza

```sql
CREATE SCHEMA [stg]; -- stg defines the staging boundary
GO
CREATE SCHEMA [edw]; -- stg defines the data warehouse boundary
GO
CREATE SCHEMA [dim]; -- edw defines the legacy schema name boundary
GO
CREATE TABLE [stg].[customer] -- create the base staging tables in the staging boundary
(       CustKey BIGINT NOT NULL
,       ...
)
GO
CREATE TABLE [edw].[customer] -- create the base data warehouse tables in the data warehouse boundary
(       CustKey BIGINT NOT NULL
,       ...
)
GO
CREATE VIEW [dim].[customer] -- create a view in the legacy schema name boundary for presentation consistency purposes only
AS
SELECT  CustKey
,       ...
FROM    [edw].customer
;
```

> [AZURE.NOTE] Promjene u shemi strategije mora pregleda model sigurnosti za bazu podataka. U mnogim slučajevima možda ćete moći pojednostavniti model sigurnosti dodijelite dozvole na razini shemu. Ako su potrebne dozvole za precizniji možete koristiti uloge baze podataka.

## <a name="next-steps"></a>Daljnji koraci
Dodatne savjete za razvoj potražite u članku [Pregled razvoj][].

<!--Image references-->

<!--Article references-->
[Pregled razvoj]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
