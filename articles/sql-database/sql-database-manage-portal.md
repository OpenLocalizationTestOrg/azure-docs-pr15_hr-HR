<properties
    pageTitle="Upravljanje bazom podataka SQL Azure pomoću portala za Azure | Microsoft Azure"
    description="Saznajte kako koristiti Azure Portal za upravljanje relacijske baze podataka u oblak pomoću portala za Azure."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.date="09/19/2016"
    ms.author="sstein"/>


# <a name="managing-azure-sql-databases-using-the-azure-portal"></a>Upravljanje baze podataka SQL Azure pomoću portala za Azure


> [AZURE.SELECTOR]
- [Portal za Azure](sql-database-manage-portal.md)
- [SSMS](sql-database-manage-azure-ssms.md)
- [PowerShell](sql-database-manage-powershell.md)

[Portal za Azure](https://portal.azure.com/) omogućuje stvaranje, praćenje i upravljanje baze podataka Azure SQL i poslužitelja. Ovaj članak sadrži veze na detalje o uobičajene zadatke i brzo opis.

## <a name="view-your-azure-sql-databases-servers-and-pools"></a>Prikaz Azure SQL baze podataka, poslužitelji i grupe

Da biste pogledali raspoloživim servisima SQL baze podataka, kliknite **Dodatne servise**, a u okvir za pretraživanje upišite **SQL** :

![SQL baze podataka](./media/sql-database-manage-portal/sql-services.png)


## <a name="how-do-i-create-or-view-azure-sql-databases"></a>Kako stvoriti ili prikaz baze podataka Azure SQL?

Da biste otvorili plohu **baze podataka SQL** , kliknite **baze podataka SQL**i bazu podataka koju želite raditi, ili kliknite **+ Dodaj** da biste stvorili SQL baze podataka. Detalje potražite u članku [Stvaranje baze podataka SQL u minutama pomoću portala za Azure](sql-database-get-started.md).


![Baze podataka SQL](./media/sql-database-manage-portal/sql-databases.png)


## <a name="how-do-i-create-or-view-azure-sql-servers"></a>Kako stvoriti ili prikaz Azure SQL Server?

Da biste otvorili plohu **SQL Server** , kliknite **SQL Server**i poslužitelj koji želite raditi, ili kliknite **+ Dodaj** da biste stvorili SQL server. Detalje potražite u članku [Stvaranje baze podataka SQL u minutama pomoću portala za Azure](sql-database-get-started.md).

![SQL Server](./media/sql-database-manage-portal/sql-servers.png)


## <a name="how-do-i-create-or-view-sql-elastic-pools"></a>Kako stvoriti ili prikaz SQL elastic grupe?

Da biste otvorili plohu **SQL elastic grupe** , kliknite **SQL elastic grupe**i skup želite raditi, ili kliknite **+ Dodaj** da biste stvorili zajedničko područje. Detalje potražite u članku [Stvaranje grupe aplikacija programa elastic baze podataka pomoću portala za Azure](sql-database-elastic-pool-create-portal.md).

![SQL elastic grupe](./media/sql-database-manage-portal/elastic-pools.png)



## <a name="how-do-i-update-or-view-sql-database-settings"></a>Kako ažurirati ili prikaz postavki baze podataka SQL-a?

Prikaz i ažuriranje postavki baze podataka, kliknite željenu postavku plohu baze podataka SQL:


![Postavke baze podataka za SQL](./media/sql-database-manage-portal/settings.png)


## <a name="how-do-i-find-a-sql-databases-fully-qualified-server-name"></a>Pronalaženje naziva poslužitelja za potpuno kvalificiran baze podataka za SQL

Da biste vidjeli naziv poslužitelja baze podataka, kliknite **Pregled** plohu **SQL baze podataka** i Imajte na umu naziv poslužitelja:


![Postavke baze podataka za SQL](./media/sql-database-manage-portal/server-name.png)


## <a name="how-do-i-manage-firewall-rules-to-control-access-to-my-sql-server-and-database"></a>Kako upravljati pravila vatrozida za kontrolu pristupa na SQL server i baze podataka?

Za prikaz, stvaranje ili ažuriranje pravila vatrozida, kliknite **Postavljanje poslužitelja vatrozid** plohu **SQL baze podataka** . Detalje potražite u članku [Konfiguriranje pravilo vatrozid razini poslužitelja baze podataka SQL Azure pomoću portala za Azure](sql-database-configure-firewall-settings.md).


![Pravila vatrozida](./media/sql-database-manage-portal/sql-database-firewall.png)


## <a name="how-do-i-change-my-sql-database-service-tier-or-performance-level"></a>Kako promijeniti Moje SQL baze podataka usluge sloju ili performanse razine?


Da biste ažurirali sloju ili performanse razine servisa SQL baze podataka, kliknite **određivanje cijena sloju (skaliranje DTUs)** plohu **SQL baze podataka** . Detalje potražite u članku [Promjena servisa sloju i performanse razine (cijene sloju) SQL baze podataka](sql-database-scale-up.md).


![cijene razine](./media/sql-database-manage-portal/pricing-tier.png)


## <a name="how-do-i-configure-auditing-and-threat-detection-for-a-sql-database"></a>Kako konfigurirati nadzor i prijetnju otkrivanje za SQL baze podataka?

Da biste konfigurirali nadzor i prijetnju otkrivanje za SQL baze podataka, kliknite **nadzor i prijetnju otkrivanje** plohu **SQL baze podataka** . Detalje potražite u članku [Uvod u SQL baze podataka nadzor](sql-database-auditing-get-started.md)i [Početak rada s otkrivanje prijetnju baze podataka za SQL](sql-database-threat-detection-get-started.md).


## <a name="how-do-i-configure-dynamic-data-masking-for-a-sql-database"></a>Kako konfigurirati dinamičke podatke masking baze podataka SQL?

Da biste konfigurirali dinamičke podatke masking za SQL baze podataka, kliknite **dinamičke podatke masking** plohu **SQL baze podataka** . Detalje potražite u članku [Uvod u SQL baze podataka dinamične podatke Masking](sql-database-dynamic-data-masking-get-started.md).


## <a name="how-do-i-configure-transparent-data-encryption-tde-for-a-sql-database"></a>Kako konfigurirati šifriranje prozirne podataka (TDE) za SQL baze podataka?

Da biste konfigurirali šifriranje prozirne podataka za SQL baze podataka, kliknite **šifriranje prozirne podataka** plohu **SQL baze podataka** . Detalje potražite u članku [Omogućivanje TDE u bazi podataka pomoću portala za](https://msdn.microsoft.com/library/dn948096#Anchor_1).

## <a name="how-do-i-view-or-change-the-max-size-of-a-sql-database"></a>Kako prikazati ili promijeniti Maks veličinu baze podataka SQL?

Da biste pogledali ili promijenili veličinu SQL baze podataka, kliknite **Veličina baze podataka** na plohu **SQL baze podataka** . Ažurirajte Maks veličine baze podataka tako da promijenite razinu servisa sloju ili performansi. Detalje potražite u članku [Promjena servisa sloju i performanse razine (cijene sloju) SQL baze podataka](sql-database-scale-up.md).

## <a name="how-do-i-monitor-and-improve-the-performance-of-a-sql-database"></a>Kako praćenje i poboljšati performanse SQL baze podataka?

Praćenje i poboljšati performanse karakteristike bazom podataka SQL, kliknite **Pregled performanse** plohu **SQL baze podataka** . Detalje potražite u članku [Performanse uvid baze podataka za SQL](sql-database-performance.md).


## <a name="how-do-i-configure-geo-replication"></a>Kako konfigurirati zemlj replikacije?

Da biste postavili zemlj replikacije baze podataka SQL, kliknite **Zemlj replikacijom** plohu **SQL baze podataka** . Detalje potražite u članku [Konfiguriranje zemlj. – replikacije baze podataka SQL Azure pomoću portala za Azure](sql-database-geo-replication-portal.md).


## <a name="how-do-i-failover-to-a-geo-replicated-sql-database"></a>Kako prebacivanje s bazom podataka SQL zemlj replicirati?

Za prebacivanje na sekundarni zemlj replicirati kliknite **Zemlj replikacije** plohu **SQL baze podataka** , a zatim kliknite **Prebacivanje**. Detalje potražite u članku [pokretanje planiranog ili neplanirano Prebacivanje baze podataka SQL Azure pomoću portala za Azure](sql-database-geo-replication-failover-portal.md).


## <a name="how-do-i-copy-a-sql-database"></a>Kako kopirati SQL baze podataka?

Da biste kopirali SQL baze podataka, kliknite **Kopiraj** na plohu **SQL baze podataka** . Detalje potražite u članku [kopiranje baze podataka Azure SQL pomoću portala za Azure](sql-database-copy-portal.md).


![Postavke baze podataka za SQL](./media/sql-database-manage-portal/sql-database-copy.png)

## <a name="how-do-i-archive-an-azure-sql-database-to-a-bacpac-file"></a>Kako arhivirati baze podataka Azure SQL BACPAC datoteku?

Da biste stvorili BACPAC SQL baze podataka, kliknite **Izvoz** plohu **SQL baze podataka** . Detalje potražite u članku [arhiva baze podataka Azure SQL BACPAC datoteku pomoću portala za Azure](sql-database-export.md).


![Izvoz baze podataka SQL](./media/sql-database-manage-portal/sql-database-export.png)



## <a name="how-do-i-restore-a-sql-database-to-a-previous-point-in-time"></a>Kako vratiti bazom podataka sustava SQL na prethodnu točku u vremenu?

Da biste vratili SQL baze podataka, kliknite **Vrati** plohu **SQL baze podataka** . Detalje potražite u članku [Vraćanje baze podataka SQL Azure prethodnog točku u vremenu pomoću portala za Azure](sql-database-point-in-time-restore-portal.md).


![Postavke baze podataka za SQL](./media/sql-database-manage-portal/sql-database-restore.png)


## <a name="how-do-i-create-an-azure-sql-database-from-a-bacpac-file"></a>Kako stvoriti baze podataka Azure SQL iz datoteke BACPAC?

Da biste stvorili SQL baze podataka iz datoteke BACPAC, kliknite **Uvoz baze podataka** na Elektronička ploča za **SQL poslužitelja** . Detalje potražite u članku [Uvoz datoteke BACPAC za stvaranje baze podataka Azure SQL](sql-database-import.md).


![SQL server](./media/sql-database-manage-portal/server-commands.png)


## <a name="how-do-i-restore-a-deleted-sql-database"></a>Kako vratiti izbrisane SQL baze podataka?

Da biste vratili izbrisanu SQL baze podataka, kliknite **Izbrisane baze podataka** Elektronička ploča **SQL poslužitelja** (SQL server koja sadrži baze podataka koji je izbrisan). Detalje potražite u članku [Obnavljanje izbrisanih Azure SQL baze podataka pomoću portala za Azure](sql-database-restore-deleted-database-portal.md).

## <a name="how-do-i-delete-a-sql-database"></a>Kako izbrisati SQL baze podataka?

Da biste izbrisali SQL baze podataka, kliknite **Izbriši** plohu **SQL baze podataka** . 

![Postavke baze podataka za SQL](./media/sql-database-manage-portal/sql-database-delete.png)



## <a name="additional-resources"></a>Dodatni resursi

- [SQL baze podataka](sql-database-technical-overview.md)
- [Nadzor i upravljanje za skup elastic baze podataka pomoću portala za Azure](sql-database-elastic-pool-manage-portal.md)
