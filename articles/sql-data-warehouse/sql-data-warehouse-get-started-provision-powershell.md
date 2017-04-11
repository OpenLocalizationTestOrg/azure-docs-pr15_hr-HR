<properties
   pageTitle="Stvaranje SQL Data Warehouse pomoću komponente PowerShell | Microsoft Azure"
   description="Stvaranje SQL Data Warehouse pomoću komponente PowerShell"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/25/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="create-sql-data-warehouse-using-powershell"></a>Stvaranje SQL Data Warehouse pomoću komponente PowerShell

> [AZURE.SELECTOR]
- [Portal za Azure](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)

U ovom se članku objašnjava da biste stvorili SQL Warehouse podataka pomoću komponente PowerShell.

## <a name="prerequisites"></a>Preduvjeti

Da bismo započeli, potrebno je:

- **Račun za Azure**: posjetiti [Azure besplatnu probnu verziju][] ili [MSDN Azure kredita][] da biste stvorili račun.
- **Azure SQL server**: potražite u članku [Stvaranje baze podataka SQL Azure logičke poslužitelja s portala za Azure][] ili [Stvaranje baze podataka SQL Azure logičke poslužitelj sa servisom PowerShell][] dodatne pojedinosti.
- **Grupa resursa**: korištenje istoj grupi resursa kao poslužitelj za Azure SQL ili pogledajte [kako stvoriti grupu resursa][].
- **PowerShell verzije 1.0.3 ili noviji**: možete provjeriti vašu verziju tako da pokrenete **modul za Get - ListAvailable-naziv Azure**.  Najnoviju verziju moguće je instalirati sa [Instalacijskog programa sustava Microsoft Web platforme][].  Dodatne informacije o instaliranju najnovije verziji, potražite u članku [kako instalirati i konfigurirati Azure PowerShell][].

> [AZURE.NOTE] Stvaranje SQL Data Warehouse može uzrokovati novog servisa za naplatu.  Dodatne informacije o cijenama potražite [SQL Data Warehouse cijene][] .

## <a name="create-a-sql-data-warehouse"></a>Stvaranje SQL Data Warehouse

1. Otvorite Windows PowerShell.
2. Pokrenite ovaj cmdlet da biste se prijavili za Azure Voditelj resursa.

    ```Powershell
    Login-AzureRmAccount
    ```
    
3. Odaberite pretplatu u koju želite koristiti za trenutnu sesiju.

    ```Powershell
    Get-AzureRmSubscription -SubscriptionName "MySubscription" | Select-AzureRmSubscription
    ```

4.  Stvaranje baze podataka. U ovom se primjeru stvara bazu pod nazivom "mynewsqldw" razinom servisa ciljna "DW400" poslužitelj pod nazivom "sqldwserver1", koja je u grupi resursa pod nazivom "mywesteuroperesgp1".

    ```Powershell
    New-AzureRmSqlDatabase -RequestedServiceObjectiveName "DW400" -DatabaseName "mynewsqldw" -ServerName "sqldwserver1" -ResourceGroupName "mywesteuroperesgp1" -Edition "DataWarehouse" -CollationName "SQL_Latin1_General_CP1_CI_AS" -MaxSizeBytes 10995116277760
    ```

Parametri potrebni su:

- **RequestedServiceObjectiveName**: iznos [DWU][] koji tražite.  Podržani vrijednosti: DW100, DW200, DW300, DW400, DW500, DW600, DW1000, DW1200, DW1500, DW2000, DW3000 i DW6000.
- **ImeBazePodataka**: naziv SQL Data Warehouse koju stvarate.
- **Naziv poslužitelja**: naziv poslužitelja koji koristite za stvaranje (mora biti V12).
- **ResourceGroupName**: koristite grupu resursa.  Da biste pronašli dostupnih resursa grupe u svoju pretplatu koristite Get-AzureResource.
- **Izdanje**: mora biti "DataWarehouse" da biste stvorili SQL Data Warehouse.

Neobavezni parametar koji su:

- **CollationName**: zadano uparivanje ako nije naveden je SQL_Latin1_General_CP1_CI_AS.  Razvrstavanje nije moguće promijeniti u bazi podataka.
- **MaxSizeBytes**: Zadani maksimalni Veličina baze podataka je 10 GB.


Dodatne informacije o mogućnosti parametara potražite u članku [Novo AzureRmSqlDatabase][] i [Stvaranje baze podataka (Azure SQL Data Warehouse)][].

## <a name="next-steps"></a>Daljnji koraci

Nakon završetka dodjeljivanje SQL Data Warehouse trebali biste pokušajte [učitavanja ogledne podatke][] ili pogledajte kako [razviti][], [Učitavanje][]ili [migrirati][].

Ako vas zanimaju dodatne informacije o upravljanju SQL Data Warehouse programski, pogledajte naše članak o korištenju [PowerShell cmdleti i REST API -ji][].

<!--Image references-->

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[Migriranje]: ./sql-data-warehouse-overview-migrate.md
[razvoj]: ./sql-data-warehouse-overview-develop.md
[Učitavanje]: ./sql-data-warehouse-load-with-bcp.md
[Učitavanje oglednih podataka]: ./sql-data-warehouse-load-sample-databases.md
[PowerShell cmdleti i REST API-ji]: ./sql-data-warehouse-reference-powershell-cmdlets.md
[firewall rules]: ../sql-database-configure-firewall-settings.md

[Kako instalirati i konfigurirati Azure PowerShell]: ../powershell/powershell-install-configure.md
[how to create a SQL Data Warehouse from the Azure Portal]: ./sql-data-warehouse-get-started-provision.md
[Stvaranje logičke poslužitelj za baze podataka SQL Azure pomoću portala za Azure]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[Stvaranje logičke poslužitelj za baze podataka SQL Azure pomoću komponente PowerShell]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[Kako stvoriti grupu resursa]: ../resource-group-template-deploy-portal.md#create-resource-group

<!--MSDN references--> 
[MSDN]: https://msdn.microsoft.com/library/azure/dn546722.aspx
[Novi AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619339.aspx
[Stvaranje baze podataka (Data Warehouse Azure SQL)]: https://msdn.microsoft.com/library/mt204021.aspx

<!--Other Web references-->
[Instalacijski program platformu Microsoft Web]: https://aka.ms/webpi-azps
[SQL Data Warehouse cijene]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure besplatnu probnu verziju]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure kredita]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F
