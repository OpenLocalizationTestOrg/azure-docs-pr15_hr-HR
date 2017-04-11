<properties
   pageTitle="Stvaranje SQL Data Warehouse s TSQL | Microsoft Azure"
   description="Saznajte kako stvoriti programa Data Warehouse SQL Azure s TSQL"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""
   tags="azure-sql-data-warehouse"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/24/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="create-a-sql-data-warehouse-database-by-using-transact-sql-tsql"></a>Stvaranje baze podataka SQL Data Warehouse pomoću Transact-SQL (TSQL)

> [AZURE.SELECTOR]
- [Portal za Azure](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)

U ovom se članku objašnjava da biste stvorili skladištu podataka za SQL pomoću T SQL.

## <a name="prerequisites"></a>Preduvjeti

Da bismo započeli, potrebno je: 

- **Račun za Azure**: posjetiti [Azure besplatnu probnu verziju][] ili [MSDN Azure kredita][] da biste stvorili račun.
- **Azure SQL server**: potražite u članku [Stvaranje baze podataka SQL Azure logičke poslužitelja s portala za Azure][] ili [Stvaranje baze podataka SQL Azure logičke poslužitelj sa servisom PowerShell][] dodatne pojedinosti.
- **Grupa resursa**: korištenje istoj grupi resursa kao poslužitelj za Azure SQL ili pogledajte [kako stvoriti grupu resursa][].
- **Okruženje za izvođenje T SQL naredbe**: koristite [Visual Studio][Installing Visual Studio and SSDT], [sqlcmd][]ili [SSMS][] za izvršavanje T SQL.

> [AZURE.NOTE] Stvaranje SQL Data Warehouse može uzrokovati novog servisa za naplatu.  Dodatne informacije o cijenama potražite [SQL Data Warehouse cijene][] .

## <a name="create-a-database-with-visual-studio"></a>Stvaranje baze podataka s Visual Studio

Ako ste novi korisnik programa Visual Studio, potražite u članku [Upita Azure SQL Data Warehouse (Visual Studio)][].  Da biste započeli, otvorite Eksplorer za objekta SQL Server u Visual Studio i povezati se s poslužiteljem koji će hostira baze podataka SQL Data Warehouse.  Nakon uspostave SQL Data Warehouse možete stvoriti tako da pokrenete sljedeću SQL naredbu u odnosu na **glavnom** bazom podataka.  Ta naredba stvara bazu podataka MySqlDwDb s na servis cilj od DW400 i Dopusti baze podataka radi povećanja maksimalne veličine 10 TB.

```sql
CREATE DATABASE MySqlDwDb COLLATE SQL_Latin1_General_CP1_CI_AS (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB);
```

## <a name="create-a-database-with-sqlcmd"></a>Stvaranje baze podataka s sqlcmd

Osim toga, možete pokrenuti jednaka naredbi s sqlcmd tako da pokrenete sljedeći naredbeni redak.

```sql
sqlcmd -S <Server Name>.database.windows.net -I -U <User> -P <Password> -Q "CREATE DATABASE MySqlDwDb COLLATE SQL_Latin1_General_CP1_CI_AS (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB)"
```

Zadano uparivanje kada nije naveden je RAZVRSTAVANJE SQL_Latin1_General_CP1_CI_AS.  Na `MAXSIZE` može biti između 250 GB i 240 TB.  Na `SERVICE_OBJECTIVE` može biti između DW100 i DW2000 [DWU][].  Popis svih valjane vrijednosti, potražite u članku [Stvaranje baze podataka][]potražite u dokumentaciji MSDN.  Od MAXSIZE i SERVICE_OBJECTIVE mogu se promijeniti s T SQL naredbu [ALTER baze podataka][] .  Razvrstavanje baze podataka nije moguće promijeniti nakon stvaranja.   Oprez treba koristiti kada promjena SERVICE_OBJECTIVE promjenjivim DWU uzrokuje ponovno pokretanje servisa, koja otkazuje sve upite u leta.  Promjena od MAXSIZE pokrenite servise kao što je samo postupak jednostavne metapodataka.

## <a name="next-steps"></a>Daljnji koraci

Nakon SQL Data Warehouse je završio dodjele resursa koje možete [učitati ogledne podatke][] ili pogledajte kako [razviti][], [Učitavanje][]ili [migrirati][].

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[how to create a SQL Data Warehouse from the Azure portal]: sql-data-warehouse-get-started-provision.md
[Upit Azure SQL Data Warehouse (Visual Studio)]: sql-data-warehouse-query-visual-studio.md
[Migriranje]: sql-data-warehouse-overview-migrate.md
[razvoj]: sql-data-warehouse-overview-develop.md
[Učitavanje]: sql-data-warehouse-overview-load.md
[Učitavanje oglednih podataka]: sql-data-warehouse-load-sample-databases.md
[Stvaranje logičke poslužitelj za baze podataka SQL Azure pomoću portala za Azure]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[Stvaranje logičke poslužitelj za baze podataka SQL Azure pomoću komponente PowerShell]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[Kako stvoriti grupu resursa]: ../resource-group-template-deploy-portal.md#create-resource-group
[Installing Visual Studio and SSDT]: sql-data-warehouse-install-visual-studio.md
[sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md

<!--MSDN references--> 
[STVARANJE BAZE PODATAKA]: https://msdn.microsoft.com/library/mt204021.aspx
[ZAMIJENI BAZU PODATAKA]: https://msdn.microsoft.com/library/mt204042.aspx
[SSMS]: https://msdn.microsoft.com/library/mt238290.aspx

<!--Other Web references-->
[SQL Data Warehouse cijene]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure besplatnu probnu verziju]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure kredita]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F
