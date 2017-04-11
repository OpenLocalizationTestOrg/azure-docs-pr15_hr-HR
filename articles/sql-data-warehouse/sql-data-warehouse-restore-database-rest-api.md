<properties
   pageTitle="Vraćanje Azure SQL Data Warehouse (REST API-JA) | Microsoft Azure"
   description="Zadaci REST API-JA za vraćanje programa Data Warehouse SQL Azure."
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

# <a name="restore-an-azure-sql-data-warehouse-rest-api"></a>Vraćanje Azure SQL Data Warehouse (REST API-JA)

> [AZURE.SELECTOR]
- [Pregled][]
- [Portal][]
- [PowerShell][]
- [OSTALE][]

U ovom članku će Saznajte kako vratiti skladištu Azure SQL podataka pomoću REST API-JA.

## <a name="before-you-begin"></a>Prije početka

**Provjerite je li vaša DTU kapaciteta.** Svaki SQL Data Warehouse hostira je SQL server (npr. myserver.database.windows.net) koja ima zadani DTU ograničenja.  Prije vraćanja SQL Data Warehouse provjerite sustava SQL server nije dovoljno preostale DTU kvote za bazu podataka koja se vratiti. Da biste saznali kako da biste izračunali DTU potrebna ili da biste zatražili dodatne DTU, potražite u članku [zahtjev za promjenom DTU kvote][].

## <a name="restore-an-active-or-paused-database"></a>Vraćanje baze podataka aktivna ni pauzirano

Da biste vratili bazu podataka:

1. Pronađite popis točaka Vraćanje baze podataka pomoću točke vraćanja baze podataka za početak operacije.
2. Započnite vaše vraćanja pomoću postupka za [Stvaranje baze podataka vrati zahtjev][] .
3. Praćenje statusa na Vrati pomoću operacije [status operacije baze podataka][] .

>[AZURE.NOTE] Po dovršetku vraćanja možete konfigurirati bazu podataka oporavljene tako [konfigurirati bazu podataka nakon oporavka][]sljedeće.

## <a name="restore-a-deleted-database"></a>Vraćanje izbrisane baze podataka

Da biste vratili izbrisanu bazu podataka:

1.  Popis svih restorable izbrisane baze podataka pomoću operacije [popis restorable izostavljanje baze podataka][] .
2.  Detalje zatražite za izbrisane bazu podataka koju želite vratiti pomoću operacije [dobiti restorable izostavljanje baze podataka][] .
3.  Započnite vaše vraćanja pomoću postupka za [Stvaranje baze podataka vrati zahtjev][] .
4.  Praćenje statusa na Vrati pomoću operacije [status operacije baze podataka][] .

>[AZURE.NOTE] Da biste konfigurirali bazu podataka po dovršetku vraćanja, potražite u članku [Konfiguriranje bazu podataka nakon oporavka][]. 


## <a name="next-steps"></a>Daljnji koraci
Da biste saznali više o značajki poslovnog continuity izdanja baze podataka SQL Azure, pročitajte [Pregled continuity za baze podataka SQL Azure tvrtke][].

<!--Image references-->

<!--Article references-->
[Azure SQL baze podataka tvrtke continuity pregled]: ./sql-database-business-continuity.md
[Zahtjev za promjenom DTU kvote]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change
[Konfiguriranje baze podataka nakon oporavka]: ./sql-database-disaster-recovery.md#configure-your-database-after-recovery
[How to install and configure Azure PowerShell]: ./powershell-install-configure.md
[Pregled]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[OSTALE]: ./sql-data-warehouse-restore-database-rest-api.md

<!--MSDN references-->
[Stvaranje zahtjeva za vraćanje baze podataka]: https://msdn.microsoft.com/library/azure/dn509571.aspx
[Status operacije baze podataka]: https://msdn.microsoft.com/library/azure/dn720371.aspx
[Početak restorable izostavljanje baze podataka]: https://msdn.microsoft.com/library/azure/dn509574.aspx
[Popis restorable prekida baze podataka]: https://msdn.microsoft.com/library/azure/dn509562.aspx
[Restore-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx

<!--Other Web references-->
[Azure Portal]: https://portal.azure.com/
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
