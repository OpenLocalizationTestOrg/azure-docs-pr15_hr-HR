<properties
   pageTitle="Vraćanje Azure SQL Data Warehouse (Portal) | Microsoft Azure"
   description="Azure portala zadataka za vraćanje programa Data Warehouse SQL Azure."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="Lakshmi1812"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/21/2016"
   ms.author="lakshmir;barbkess;sonyama"/>

# <a name="restore-an-azure-sql-data-warehouse-portal"></a>Vraćanje Azure SQL Data Warehouse (Portal)

> [AZURE.SELECTOR]
- [Pregled][]
- [Portal][]
- [PowerShell][]
- [OSTALE][]

U ovom članku će Saznajte kako vratiti skladištu Azure SQL podataka pomoću portala za Azure.

## <a name="before-you-begin"></a>Prije početka

**Provjerite je li vaša DTU kapaciteta.** Svaki SQL Data Warehouse hostira je SQL server (npr. myserver.database.windows.net) koja ima zadani DTU ograničenja.  Prije vraćanja SQL Data Warehouse provjerite sustava SQL server nije dovoljno preostale DTU kvote za bazu podataka koja se vratiti. Da biste saznali kako da biste izračunali DTU potrebna ili da biste zatražili dodatne DTU, potražite u članku [zahtjev za promjenom DTU kvote][].


## <a name="restore-an-active-or-paused-database"></a>Vraćanje baze podataka aktivna ni pauzirano

Da biste vratili bazu podataka:

1. Prijavite se na [portal za Azure][]
2. Na lijevoj strani zaslona odaberite **Pregledaj** , a zatim odaberite **SQL Server**
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-browse-for-sql-server.png)
    
3. Idite na poslužitelju, a zatim ga odaberite
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-select-server.png)

4. Pronalaženje SQL Data Warehouse koju želite vratiti, a zatim ga odaberite
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-select-active-dw.png)
5. Pri vrhu plohu Data Warehouse, kliknite **Vrati**
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-select-restore-from-active.png)

6. Unesite novi **Naziv baze podataka**
7. Odaberite najnoviju **Točku vraćanja**
    1. Provjerite jeste li odabrali najnovije točke vraćanja.  Budući da točke vraćanja prikazuju se u UTC-u, ponekad zadana mogućnost prikazuju se ne najnovije točke vraćanja.
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-restore-blade-from-active.png)

8. Kliknite **u redu**
9. Postupak vraćanja baze podataka započet će i moguće nadzirati pomoću **obavijesti**

>[AZURE.NOTE] Po dovršetku vraćanja možete konfigurirati bazu podataka oporavljene tako [konfigurirati bazu podataka nakon oporavka][]sljedeće.


## <a name="restore-a-deleted-database"></a>Vraćanje izbrisane baze podataka

Da biste vratili izbrisanu bazu podataka:

1. Prijavite se na [portal za Azure][]
2. Na lijevoj strani zaslona odaberite **Pregledaj** , a zatim odaberite **SQL Server**
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-browse-for-sql-server.png)

3. Idite na poslužitelju, a zatim ga odaberite
    
    ![](./media/sql-data-warehouse-restore-database-portal/02-select-server.png)

4. Pomaknite se prema dolje do odjeljka operacije na Elektronička ploča vašeg poslužitelja
5. Kliknite pločicu **Izbrisane baze podataka**
    
    ![](./media/sql-data-warehouse-restore-database-portal/02-select-deleted-dws.png)

6. Odaberite izbrisanu bazu podataka koju želite vratiti
    
    ![](./media/sql-data-warehouse-restore-database-portal/02-select-deleted-dw.png)

7. Unesite novi **Naziv baze podataka**
    
    ![](./media/sql-data-warehouse-restore-database-portal/02-restore-blade-from-deleted.png)
    
8. Kliknite **u redu**
9. Postupak vraćanja baze podataka započet će i moguće nadzirati pomoću **obavijesti**

>[AZURE.NOTE] Da biste konfigurirali bazu podataka po dovršetku vraćanja, potražite u članku [Konfiguriranje bazu podataka nakon oporavka][]. 

## <a name="next-steps"></a>Daljnji koraci
Da biste saznali više o značajki poslovnog continuity izdanja baze podataka SQL Azure, pročitajte [Pregled continuity za baze podataka SQL Azure tvrtke][].

<!--Image references-->

<!--Article references-->
[Azure SQL baze podataka tvrtke continuity pregled]: ./sql-database-business-continuity.md
[Pregled]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[OSTALE]: ./sql-data-warehouse-restore-database-rest-api.md
[Konfiguriranje baze podataka nakon oporavka]: ./sql-database-disaster-recovery.md#configure-your-database-after-recovery
[Zahtjev za promjenom DTU kvote]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change

<!--MSDN references-->

<!--Blog references-->

<!--Other Web references-->
[Portal za Azure]: https://portal.azure.com/
