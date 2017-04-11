<properties
    pageTitle="Vraćanje baze podataka Azure SQL prethodnog točku u vremenu (portal za Azure) | Microsoft Azure"
    description="Vraćanje baze podataka Azure SQL prethodnog točku u vremenu."
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


# <a name="restore-an-azure-sql-database-to-a-previous-point-in-time-with-the-azure-portal"></a>Vraćanje baze podataka Azure SQL prethodnog točku u vremenu pomoću portala za Azure


> [AZURE.SELECTOR]
- [Pregled](sql-database-recovery-using-backups.md)
- [Vraćanje točke u vrijeme: PowerShell](sql-database-point-in-time-restore-powershell.md)

U ovom se članku objašnjava da biste vratili bazu podataka ranije točke u vremenu iz [baze podataka SQL automatizirano sigurnosno kopiranje](sql-database-automated-backups.md) pomoću portala za Azure.

## <a name="restore-a-sql-database-to-a-previous-point-in-time"></a>Vraćanje baze podataka za SQL prethodnu točku u vremenu

Odabir baze podataka da biste vratili na portalu za Azure:

1.  Otvorite [portal za Azure](https://portal.azure.com).
2.  Na lijevoj strani zaslona odaberite **više usluge** > **baze podataka SQL**.
3.  Kliknite bazu podataka koju želite vratiti.
4.  Pri vrhu stranice svoje baze podataka, odaberite **Vrati**:

    ![Vraćanje baze podataka Azure SQL](./media/sql-database-point-in-time-restore-portal/restore.png)

5.  Na stranici **vraćanja** odaberite datum i vrijeme (u vremenu UTC-a) da biste vratili bazu podataka, a zatim **u redu**:

    ![Vraćanje baze podataka Azure SQL](./media/sql-database-point-in-time-restore-portal/restore-details.png)

## <a name="monitor-the-restore-operation"></a>Praćenje postupak vraćanja

1. Nakon što kliknete **u redu** u prethodnom koraku, kliknite ikonu obavijesti u gornjem desnom kutu stranice, a zatim kliknite obavijest o **vraćanju SQL baze podataka** detalje.

    ![Vraćanje baze podataka Azure SQL](./media/sql-database-point-in-time-restore-portal/notification-icon.png)

2. Otvorit će se stranica za vraćanje SQL baze podataka s podacima o stanju Vrati. Možete kliknuti redak stavke detalje:

    ![Vraćanje baze podataka Azure SQL](./media/sql-database-point-in-time-restore-portal/inprogress.png)

 

## <a name="next-steps"></a>Daljnji koraci

- Pregled za continuity tvrtke i scenariji potražite u članku [Pregled continuity tvrtke](sql-database-business-continuity.md)
- Da biste saznali više o sigurnosne kopije baze podataka SQL Azure automatski, potražite u članku [baze podataka SQL automatskog sigurnosnog kopiranja](sql-database-automated-backups.md)
- Dodatne informacije o korištenju automatizirano sigurnosno kopiranje za oporavak, potražite u članku [Vraćanje baze podataka iz sigurnosne kopije pokrenut servis](sql-database-recovery-using-backups.md)
- Da biste saznali više o brže mogućnosti za oporavak, potražite u članku [Aktivno zemlj replikacije](sql-database-geo-replication-overview.md)  
- Dodatne informacije o korištenju automatizirano sigurnosno kopiranje za arhiviranje, pročitajte članak [kopiranje baze podataka](sql-database-copy.md)
