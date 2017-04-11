<properties
    pageTitle="Vraćanje izbrisane Azure SQL baze podataka (portal za Azure) | Microsoft Azure"
    description="Vraćanje izbrisane Azure SQL baze podataka (portal za Azure)."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="10/12/2016"
    ms.author="sstein"
    ms.workload="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="restore-a-deleted-azure-sql-database-using-the-azure-portal"></a>Vraćanje izbrisane Azure SQL baze podataka pomoću portala za Azure

> [AZURE.SELECTOR]
- [Pregled](sql-database-recovery-using-backups.md)
- [**Vraćanje izbrisanih DB: Portal**](sql-database-restore-deleted-database-portal.md)
- [Vraćanje izbrisanih DB: PowerShell](sql-database-restore-deleted-database-powershell.md)

## <a name="select-the-database-to-restore"></a>Odaberite bazu podataka da biste vratili 

Da biste vratili izbrisanu bazu podataka na portalu za Azure:

1.  [Portal za Azure](https://portal.azure.com)kliknite **Dodatne usluge** > **SQL poslužitelja**.
3.  Odaberite poslužitelju na kojem se nalaze u bazu podataka koju želite vratiti.
4.  Pomaknite se do odjeljka **operacije** Elektronička ploča za poslužitelja, a zatim odaberite **Izbrisane baza podataka**: ![Vraćanje baze podataka Azure SQL](./media/sql-database-restore-deleted-database-portal/restore-deleted-trashbin.png)
5.  Odaberite bazu podataka koju želite vratiti.
6.  Navedite naziv baze podataka, a zatim kliknite **u redu**:

    ![Vraćanje baze podataka Azure SQL](./media/sql-database-restore-deleted-database-portal/restore-deleted.png)


## <a name="next-steps"></a>Daljnji koraci

- Pregled za continuity tvrtke i scenariji potražite u članku [Pregled continuity tvrtke](sql-database-business-continuity.md)
- Da biste saznali više o sigurnosne kopije baze podataka SQL Azure automatski, potražite u članku [baze podataka SQL automatskog sigurnosnog kopiranja](sql-database-automated-backups.md)
- Dodatne informacije o korištenju automatizirano sigurnosno kopiranje za oporavak, potražite u članku [Vraćanje baze podataka iz sigurnosne kopije pokrenut servis](sql-database-recovery-using-backups.md)
- Da biste saznali više o brže mogućnosti za oporavak, potražite u članku [Aktivno zemlj replikacije](sql-database-geo-replication-overview.md)  
- Dodatne informacije o korištenju automatizirano sigurnosno kopiranje za arhiviranje, pročitajte članak [kopiranje baze podataka](sql-database-copy.md)
