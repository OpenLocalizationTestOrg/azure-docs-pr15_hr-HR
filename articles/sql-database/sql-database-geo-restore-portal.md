<properties
    pageTitle="Vraćanje baze podataka Azure SQL iz automatsko sigurnosno kopiranje (portal za Azure) | Microsoft Azure"
    description="Vraćanje baze podataka Azure SQL iz automatsko sigurnosno kopiranje (portal za Azure)."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="10/18/2016"
    ms.author="sstein"
    ms.workload="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="restore-an-azure-sql-database-from-an-automatic-backup-using-the-azure-portal"></a>Vraćanje baze podataka Azure SQL iz automatsko sigurnosno kopiranje pomoću portala za Azure


> [AZURE.SELECTOR]
- [Pregled](sql-database-recovery-using-backups.md#geo-restore)
- [Zemlj Vrati: PowerShell](sql-database-geo-restore-powershell.md)

U ovom se članku objašnjava da biste vratili bazu podataka iz [Automatsko sigurnosno kopiranje](sql-database-automated-backups.md) u novi poslužitelj s [Zemlj Vrati](sql-database-recovery-using-backups/.md#geo-restore) pomoću portala za Azure.

## <a name="select-a-database-to-restore"></a>Odabir baze podataka da biste vratili

Da biste vratili bazu podataka na portalu za Azure, učinite sljedeće:

1.  Idite na [portal za Azure](https://portal.azure.com).
2.  Na lijevoj strani zaslona odaberite **+ Novo** > **baza podataka** > **SQL baze podataka**:

    ![Vraćanje baze podataka Azure SQL](./media/sql-database-geo-restore-portal/new-sql-database.png)

3.  Odaberite **sigurnosne kopije** kao izvor, a zatim odaberite sigurnosne kopije koju želite vratiti. Navedite naziv baze podataka s poslužitelja koju želite vratiti bazu podataka u, a zatim kliknite **Stvori**:
  
    ![Vraćanje baze podataka Azure SQL](./media/sql-database-geo-restore-portal/geo-restore.png)

Praćenje statusa postupak vraćanja tako da kliknete ikonu obavijesti u gornjem desnom kutu stranice. 


## <a name="next-steps"></a>Daljnji koraci

- Pregled za continuity tvrtke i scenariji potražite u članku [Pregled continuity tvrtke](sql-database-business-continuity.md)
- Da biste saznali više o sigurnosne kopije baze podataka SQL Azure automatski, potražite u članku [baze podataka SQL automatskog sigurnosnog kopiranja](sql-database-automated-backups.md)
- Dodatne informacije o korištenju automatizirano sigurnosno kopiranje za oporavak, potražite u članku [Vraćanje baze podataka iz sigurnosne kopije pokrenut servis](sql-database-recovery-using-backups.md)
- Da biste saznali više o brže mogućnosti za oporavak, potražite u članku [Aktivno zemlj replikacije](sql-database-geo-replication-overview.md)  
- Dodatne informacije o korištenju automatizirano sigurnosno kopiranje za arhiviranje, pročitajte članak [kopiranje baze podataka](sql-database-copy.md)
