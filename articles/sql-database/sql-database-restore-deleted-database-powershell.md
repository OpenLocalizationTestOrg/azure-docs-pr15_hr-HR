<properties
    pageTitle="Vraćanje izbrisane Azure SQL baze podataka (PowerShell) | Microsoft Azure"
    description="Vraćanje izbrisane Azure SQL baze podataka (PowerShell)."
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


# <a name="restore-a-deleted-azure-sql-database-by-using-powershell"></a>Vraćanje izbrisane baze podataka SQL Azure pomoću komponente PowerShell

> [AZURE.SELECTOR]
- [Pregled](sql-database-recovery-using-backups.md)
- [Vraćanje izbrisanih DB: Portal](sql-database-restore-deleted-database-portal.md)
- [**Vraćanje izbrisanih DB: PowerShell**](sql-database-restore-deleted-database-powershell.md)

[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]


## <a name="get-a-list-of-deleted-databases"></a>Dobit ćete popis izbrisane baze podataka

```
$resourceGroupName = "resourcegroupname"
$sqlServerName = "servername"

$DeletedDatabases = Get-AzureRmSqlDeletedDatabaseBackup -ResourceGroupName $resourceGroupName -ServerName $sqlServerName
```

## <a name="restore-your-deleted-database-into-a-standalone-database"></a>Vraćanje izbrisane baze podataka u bazu podataka za samostalni

Pronađite kopije izbrisanu bazu podataka koju želite vratiti pomoću cmdleta [Get-AzureRmSqlDeletedDatabaseBackup](https://msdn.microsoft.com/library/azure/mt693387(v=azure.300/).aspx) . Pomoću cmdleta za [Vraćanje AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt693390(v=azure.300/).aspx) počnite vraćanje iz sigurnosne kopije izbrisanu bazu podataka.

```
$resourceGroupName = "resourcegroupname"
$sqlServerName = "servername"
$databaseName = "deletedDbToRestore"

$DeletedDatabase = Get-AzureRmSqlDeletedDatabaseBackup -ResourceGroupName $resourceGroupName -ServerName $sqlServerName -DatabaseName $databaseName

Restore-AzureRmSqlDatabase –FromDeletedDatabaseBackup –DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $DeletedDatabase.ResourceGroupName -ServerName $DeletedDatabase.ServerName -TargetDatabaseName "RestoredDatabase" –ResourceId $DeletedDatabase.ResourceID -Edition "Standard" -ServiceObjectiveName "S2"
```


## <a name="restore-your-deleted-database-into-an-elastic-database-pool"></a>Vraćanje izbrisane baze podataka u grupe aplikacija programa elastic baze podataka

Pronađite kopije izbrisanu bazu podataka koju želite vratiti pomoću cmdleta [Get-AzureRmSqlDeletedDatabaseBackup](https://msdn.microsoft.com/library/azure/mt693387(v=azure.300/).aspx) . Pomoću cmdleta za [Vraćanje AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt693390(v=azure.300/).aspx) počnite vraćanje iz sigurnosne kopije izbrisanu bazu podataka.

```
$resourceGroupName = "resourcegroupname"
$sqlServerName = "servername"
$databaseName = "deletedDbToRestore"

$DeletedDatabase = Get-AzureRmSqlDeletedDatabaseBackup -ResourceGroupName $resourceGroupName -ServerName $sqlServerName -DatabaseName $databaseName

Restore-AzureRmSqlDatabase –FromDeletedDatabaseBackup –DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $DeletedDatabase.ResourceGroupName -ServerName $DeletedDatabase.ServerName -TargetDatabaseName "RestoredDatabase" –ResourceId $DeletedDatabase.ResourceID –ElasticPoolName "elasticpool01"
```


## <a name="next-steps"></a>Daljnji koraci

- Pregled za continuity tvrtke i scenariji potražite u članku [Pregled continuity tvrtke](sql-database-business-continuity.md)
- Da biste saznali više o sigurnosne kopije baze podataka SQL Azure automatski, potražite u članku [baze podataka SQL automatskog sigurnosnog kopiranja](sql-database-automated-backups.md)
- Dodatne informacije o korištenju automatizirano sigurnosno kopiranje za oporavak, potražite u članku [Vraćanje baze podataka iz sigurnosne kopije pokrenut servis](sql-database-recovery-using-backups.md)
- Da biste saznali više o brže mogućnosti za oporavak, potražite u članku [Aktivno zemlj replikacije](sql-database-geo-replication-overview.md)  
- Dodatne informacije o korištenju automatizirano sigurnosno kopiranje za arhiviranje, pročitajte članak [kopiranje baze podataka](sql-database-copy.md)
