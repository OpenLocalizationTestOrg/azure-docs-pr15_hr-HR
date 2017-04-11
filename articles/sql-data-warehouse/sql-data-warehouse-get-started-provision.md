<properties
   pageTitle="Stvaranje SQL Data Warehouse na portalu za Azure | Microsoft Azure"
   description="Saznajte kako stvoriti programa Azure SQL Data Warehouse na portalu za Azure"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="jhubbard"
   editor=""
   tags="azure-sql-data-warehouse"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/25/2016"
   ms.author="barbkess;lodipalm;sonyama"/>

# <a name="create-an-azure-sql-data-warehouse"></a>Stvaranje programa Data Warehouse Azure SQL

> [AZURE.SELECTOR]
- [Portal za Azure](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)

Pomoću ovog praktičnog vodiča za stvaranje SQL Data Warehouse koja sadrži ogledne baze AdventureWorksDW koristi Azure portal.


## <a name="prerequisites"></a>Preduvjeti

Da bismo započeli, potrebno je:

- **Račun za Azure**: posjetiti [Azure besplatnu probnu verziju][] ili [MSDN Azure kredita][] da biste stvorili račun.
- **Azure SQL server**: dodatne informacije u odjeljku [Stvaranje baze podataka SQL Azure logičke poslužitelja s portala za Azure][] .

> [AZURE.NOTE] Stvaranje SQL Data Warehouse može rezultirati novog servisa za naplatu.  Dodatne informacije potražite [SQL Data Warehouse cijene][] .

## <a name="create-a-sql-data-warehouse"></a>Stvaranje SQL Data Warehouse

1. Prijavite se na [portal za Azure](https://portal.azure.com).

2. Kliknite **+ Novo** > **podataka + prostor za pohranu** > **SQL Data Warehouse**.

    ![Stvaranje](./media/sql-data-warehouse-get-started-provision/create-sample.gif)

3. U plohu **SQL Data Warehouse** ispunite podatke potrebne, a zatim pritisnite Stvori da biste stvorili.

    ![Stvaranje baze podataka](./media/sql-data-warehouse-get-started-provision/create-database.png)

    - **Poslužitelj**: preporučujemo da najprije odaberite poslužitelj.  

    - **Naziv baze podataka**: naziv koji se koristi za pozivanje SQL Data Warehouse.  Mora biti jedinstven na poslužitelj.
    
    - **Performanse**: preporučujemo počevši od 400 [DWUs][DWU]. Možete premjestite klizač ulijevo ili udesno da biste prilagodili performanse skladištu podataka ili je skalirali prema gore ili dolje nakon stvaranja.  Da biste saznali više o DWUs, potražite u članku naš dokumentaciju na [Skaliranje](./sql-data-warehouse-manage-compute-overview.md) ili naših [cijene stranice][SQL Data Warehouse cijene]. 

    - **Pretplate**: odaberite [pretplatu] koju će ovaj SQL Data Warehouse fakturu za.

    - **Grupa resursa**: [grupa resursa] [ Resource group] su spremnici omogućuju upravljanje skupovima Azure resursa. Dodatne informacije o [grupama resursa](../azure-resource-manager/resource-group-overview.md).

    - **Odaberite izvor**: kliknite **Odabir izvora** > **uzorka**. Azure automatski popunjava mogućnost **odaberite uzorak** s AdventureWorksDW.

> [AZURE.NOTE] Zadano uparivanje za SQL Data Warehouse je SQL_Latin1_General_CP1_CI_AS. Ako drugi razvrstavanje nije potrebna, [T SQL][] može koristiti za stvaranje baze podataka s različitim razvrstavanje.

4. Kliknite **Stvori** da biste stvorili SQL Data Warehouse.

5. Pričekajte nekoliko minuta. Kada je spreman skladištu podatke koji bi trebala biti vraćena [Azure portal](https://portal.azure.com). SQL Data Warehouse možete pronaći na nadzornoj ploči, na popisu u odjeljku baze podataka za SQL ili u grupi resursa koje ste koristili za stvaranje. 

    ![Prikaz portala](./media/sql-data-warehouse-get-started-provision/database-portal-view.png)

[AZURE.INCLUDE [SQL Database create server](../../includes/sql-database-create-new-server-firewall-portal.md)] 

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste stvorili SQL Data Warehouse, spremni ste za [Povezivanje](./sql-data-warehouse-connect-overview.md) i započeti slanje upita.

Da biste učitali podatke u SQL Data Warehouse, potražite u članku [Učitavanje pregled](./sql-data-warehouse-overview-load.md).

Ako pokušavate migrirati postojeću bazu podataka za SQL Data Warehouse, potražite u članku [Pregled migraciju](./sql-data-warehouse-overview-migrate.md) ili korištenja [Programa za migraciju](./sql-data-warehouse-migrate-migration-utility.md).

Pravila vatrozida može se konfigurirati i pomoću Transact-SQL. Dodatne informacije potražite u članku [sp_set_firewall_rule][] i [sp_set_database_firewall_rule][].

Preporučuje se i odlične ideje možete pogledati [najbolje prakse][].

<!--Article references-->
[Stvaranje logičke poslužitelj za baze podataka SQL Azure pomoću portala za Azure]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[Create an Azure SQL Database logical server with PowerShell]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[resource groups]: ../resource-group-template-deploy-portal.md
[Najbolje prakse]: sql-data-warehouse-best-practices.md
[DWU]: sql-data-warehouse-overview-what-is.md#data-warehouse-units
[pretplate]: ../azure-glossary-cloud-terminology.md#subscription
[resource group]: ../azure-glossary-cloud-terminology.md#resource-group
[T-SQL]: ./sql-data-warehouse-get-started-create-database-tsql.md
 
<!--MSDN references-->
[sp_set_firewall_rule]: https://msdn.microsoft.com/library/dn270017.aspx
[sp_set_database_firewall_rule]: https://msdn.microsoft.com/library/dn270010.aspx

<!--Other Web references-->
[SQL Data Warehouse cijene]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure besplatnu probnu verziju]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure kredita]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F

