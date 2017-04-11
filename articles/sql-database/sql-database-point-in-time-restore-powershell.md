<properties
    pageTitle="Vraćanje baze podataka SQL Azure prethodnog točku u vremenu (PowerShell) | Microsoft Azure"
    description="Vraćanje baze podataka SQL Azure prethodnog točku u vremenu"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="powershell"
    ms.workload="NA"
    ms.date="07/17/2016"
    ms.author="sstein"/>

# <a name="restore-an-azure-sql-database-to-a-previous-point-in-time-with-powershell"></a>Vraćanje baze podataka SQL Azure prethodnog točku u vremenu pomoću komponente PowerShell

> [AZURE.SELECTOR]
- [Pregled](sql-database-recovery-using-backups.md)
- [Vrati točke u vrijeme: Portal za Azure](sql-database-point-in-time-restore-portal.md)

U ovom se članku objašnjava da biste vratili bazu podataka ranije točke u vremenu iz [baze podataka SQL automatizirano sigurnosno kopiranje](sql-database-automated-backups.md). To možete učiniti pomoću komponente PowerShell.

[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="restore-your-database-to-a-point-in-time-as-a-standalone-database"></a>Vraćanje baze podataka na mjesto na kao samostalan proizvod baze podataka

1. Početak bazu podataka koju želite vratiti pomoću na [Get-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt603648(v=azure.300\).aspx) cmdlet.

        $Database = Get-AzureRmSqlDatabase -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"

2. Vraćanje baze podataka točku u vremenu pomoću na [vraćanja-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt693390(v=azure.300\).aspx) cmdlet.

        Restore-AzureRmSqlDatabase –FromPointInTimeBackup –PointInTime UTCDateTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.ServerName -TargetDatabaseName "RestoredDatabase" –ResourceId $Database.ResourceID -Edition "Standard" -ServiceObjectiveName "S2"


## <a name="restore-your-database-to-a-point-in-time-into-an-elastic-database-pool"></a>Vraćanje baze podataka točku u vremenu u grupe aplikacija programa elastic baze podataka

1. Početak bazu podataka koju želite vratiti pomoću na [Get-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt603648(v=azure.300\).aspx) cmdlet.

        $Database = Get-AzureRmSqlDatabase -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"

2. Vraćanje baze podataka točku u vremenu pomoću na [vraćanja-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt693390(v=azure.300\).aspx) cmdlet.

        Restore-AzureRmSqlDatabase –FromPointInTimeBackup –PointInTime UTCDateTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.ServerName -TargetDatabaseName "RestoredDatabase" –ResourceId $Database.ResourceID –ElasticPoolName "elasticpool01"


## <a name="next-steps"></a>Daljnji koraci

- Pregled za continuity tvrtke i scenariji potražite u članku [Pregled continuity tvrtke](sql-database-business-continuity.md)
- Da biste saznali više o sigurnosne kopije baze podataka SQL Azure automatski, potražite u članku [baze podataka SQL automatskog sigurnosnog kopiranja](sql-database-automated-backups.md)
- Dodatne informacije o korištenju automatizirano sigurnosno kopiranje za oporavak, potražite u članku [Vraćanje baze podataka iz sigurnosne kopije pokrenut servis](sql-database-recovery-using-backups.md)
- Da biste saznali više o brže mogućnosti za oporavak, potražite u članku [Aktivno zemlj replikacije](sql-database-geo-replication-overview.md)  
- Dodatne informacije o korištenju automatizirano sigurnosno kopiranje za arhiviranje, pročitajte članak [kopiranje baze podataka](sql-database-copy.md)
