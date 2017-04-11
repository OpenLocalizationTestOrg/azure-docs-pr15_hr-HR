<properties
    pageTitle="Vraćanje baze podataka SQL Azure iz sigurnosne kopije suvišnih zemlj. (PowerShell) | Microsoft Azure"
    description="Vraćanje baze podataka SQL Azure u novi poslužitelj iz sigurnosne kopije suvišnih zemlj."
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

# <a name="restore-an-azure-sql-database-from-a-geo-redundant-backup-by-using-powershell"></a>Vraćanje baze podataka SQL Azure iz sigurnosne kopije suvišnih zemlj pomoću komponente PowerShell


> [AZURE.SELECTOR]
- [Pregled](sql-database-recovery-using-backups.md)
- [Vrati zemlj.: Portal za Azure](sql-database-geo-restore-portal.md)

U ovom se članku objašnjava da biste vratili bazu podataka u novi poslužitelj pomoću zemlj vraćanja. To možete učiniti putem komponente PowerShell.

[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="geo-restore-your-database-into-a-standalone-database"></a>Vraćanje zemlj bazu podataka u bazu podataka za samostalni

1. Početak zemlj suvišnih sigurnosnu kopiju baze podataka koju želite vratiti pomoću na [Get-AzureRmSqlDatabaseGeoBackup] (https://msdn.microsoft.com/library/azure/mt693388(v=azure.300\).aspx) cmdlet.

        $GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"

2. Pokrenite vraćanje iz sigurnosne kopije suvišnih zemlj pomoću na [vraćanja-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt693390(v=azure.300\).aspx) cmdlet.

        Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "TargetResourceGroup" -ServerName "TargetServer" -TargetDatabaseName "RestoredDatabase" –ResourceId $GeoBackup.ResourceID -Edition "Standard" -RequestedServiceObjectiveName "S2"


## <a name="geo-restore-your-database-into-an-elastic-database-pool"></a>Vraćanje zemlj bazu podataka u grupe aplikacija programa elastic baze podataka

1. Početak zemlj suvišnih sigurnosnu kopiju baze podataka koju želite vratiti pomoću na [Get-AzureRmSqlDatabaseGeoBackup] (https://msdn.microsoft.com/library/azure/mt693388(v=azure.300\).aspx) cmdlet.

        $GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"

2. Pokrenite vraćanje iz sigurnosne kopije suvišnih zemlj pomoću na [vraćanja-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt693390(v=azure.300\).aspx) cmdlet. Navedite naziv grupe koju želite vratiti bazu podataka u.

        Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "TargetResourceGroup" -ServerName "TargetServer" -TargetDatabaseName "RestoredDatabase" –ResourceId $GeoBackup.ResourceID –ElasticPoolName "elasticpool01"  


## <a name="next-steps"></a>Daljnji koraci

- Pregled za continuity tvrtke i scenarijima potražite u članku [Pregled continuity tvrtke](sql-database-business-continuity.md).
- Da biste saznali više o sigurnosne kopije baze podataka SQL Azure automatski, potražite u članku [baze podataka SQL automatizirano sigurnosno kopiranje](sql-database-automated-backups.md).
- Dodatne informacije o korištenju automatizirano sigurnosno kopiranje za oporavak, potražite u članku [Vraćanje baze podataka iz sigurnosne kopije pokrenut servis](sql-database-recovery-using-backups.md).
- Da biste saznali više o brže mogućnosti za oporavak, potražite u članku [Aktivno zemlj replikacije](sql-database-geo-replication-overview.md).  
- Dodatne informacije o korištenju automatizirano sigurnosno kopiranje za arhiviranje, potražite u članku [kopiranje baze podataka](sql-database-copy.md).
