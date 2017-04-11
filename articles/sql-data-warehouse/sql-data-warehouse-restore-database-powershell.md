<properties
   pageTitle="Vraćanje Azure SQL Data Warehouse (PowerShell) | Microsoft Azure"
   description="Zadaci ljuske PowerShell za vraćanje programa Data Warehouse SQL Azure."
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

# <a name="restore-an-azure-sql-data-warehouse-powershell"></a>Vraćanje Azure SQL Data Warehouse (PowerShell)

> [AZURE.SELECTOR]
- [Pregled][]
- [Portal][]
- [PowerShell][]
- [OSTALE][]

U ovom članku će Saznajte kako vratiti skladištu Azure SQL podataka pomoću komponente PowerShell.

## <a name="before-you-begin"></a>Prije početka

**Provjerite je li vaša DTU kapaciteta.** Svaki SQL Data Warehouse hostira je SQL server (npr. myserver.database.windows.net) koja ima zadani DTU ograničenja.  Prije vraćanja SQL Data Warehouse provjerite sustava SQL server nije dovoljno preostale DTU kvote za bazu podataka koja se vratiti. Da biste saznali kako da biste izračunali DTU potrebna ili da biste zatražili dodatne DTU, potražite u članku [zahtjev za promjenom DTU kvote][].

### <a name="install-powershell"></a>Instaliranje komponente PowerShell

Da biste koristili Azure PowerShell sa SQL Data Warehouse, morat ćete instalirati Azure PowerShell verziju 1.0 ili noviji.  Možete provjeriti vašu verziju tako da pokrenete **modul za Get - ListAvailable-AzureRM naziv**.  Najnoviju verziju moguće je instalirati sa [Instalacijskog programa sustava Microsoft Web platforme][].  Dodatne informacije o instaliranju najnovije verziji, potražite u članku [kako instalirati i konfigurirati Azure PowerShell][].

## <a name="restore-an-active-or-paused-database"></a>Vraćanje baze podataka aktivna ni pauzirano

Da biste vratili bazu podataka iz snimke stanja pomoću cmdleta ljuske PowerShell [Vrati AzureRmSqlDatabase][] .

1. Otvorite Windows PowerShell.
2. Povezivanje s računom za Azure i navođenje svih pretplata povezana s vašim računom.
3. Odaberite pretplatu koja sadrži baze podataka koju želite vratiti.
4. Popis točke vraćanja za bazu podataka.
5. Odaberite točku željeni vraćanja pomoću na RestorePointCreationDate.
6. Vraćanje baze podataka točke željeni vraćanja.
7. Provjerite je li vraćenu bazu podataka online.

```Powershell

$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# List the last 10 database restore points
((Get-AzureRMSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName ($DatabaseName).RestorePointCreationDate)[-10 .. -1]

# Or list all restore points
Get-AzureRmSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Get the specific database to restore
$Database = Get-AzureRmSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Pick desired restore point using RestorePointCreationDate
$PointInTime="<RestorePointCreationDate>"  

# Restore database from a restore point
$RestoredDatabase = Restore-AzureRmSqlDatabase –FromPointInTimeBackup –PointInTime $PointInTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.$ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $Database.ResourceID

# Verify the status of restored database
$RestoredDatabase.status

```

>[AZURE.NOTE] Po dovršetku vraćanja možete konfigurirati bazu podataka oporavljene tako [konfigurirati bazu podataka nakon oporavka][]sljedeće.


## <a name="restore-a-deleted-database"></a>Vraćanje izbrisane baze podataka

Da biste vratili izbrisanu bazu podataka, koristite cmdlet [Vrati AzureRmSqlDatabase][] .

1. Otvorite Windows PowerShell.
2. Povezivanje s računom za Azure i navođenje svih pretplata povezana s vašim računom.
3. Odaberite pretplatu koja sadrži izbrisane baze podataka koju želite vratiti.
4. Pronađite određene izbrisanu bazu podataka.
5. Vraćanje izbrisane baze podataka.
6. Provjerite je li vraćenu bazu podataka online.

```Powershell
$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# Get the deleted database to restore
$DeletedDatabase = Get-AzureRmSqlDeletedDatabaseBackup -ResourceGroupName $ResourceGroupNam -ServerName $ServerName -DatabaseName $DatabaseName

# Restore deleted database
$RestoredDatabase = Restore-AzureRmSqlDatabase –FromDeletedDatabaseBackup –DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $DeletedDatabase.ResourceGroupName -ServerName $DeletedDatabase.ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $DeletedDatabase.ResourceID

# Verify the status of restored database
$RestoredDatabase.status
```

>[AZURE.NOTE] Po dovršetku vraćanja možete konfigurirati bazu podataka oporavljene tako [konfigurirati bazu podataka nakon oporavka][]sljedeće.


## <a name="restore-from-an-azure-geographical-region"></a>Vraćanje iz Azure zemljopisnom području

Da biste vratili bazu podataka, koristite cmdlet [Vrati AzureRmSqlDatabase][] .

1. Otvorite Windows PowerShell.
2. Povezivanje s računom za Azure i navođenje svih pretplata povezana s vašim računom.
3. Odaberite pretplatu koja sadrži baze podataka koju želite vratiti.
4. Pronađite bazu podataka koju želite oporaviti.
5. Stvaranje zahtjeva za oporavak za bazu podataka.
6. Provjerite stanje baze podataka zemlj vratiti.

```Powershell
Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName "<Subscription_name>"

# Get the database you want to recover
$GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourServerName>" -DatabaseName "<YourDatabaseName>"

# Recover database
$GeoRestoredDatabase = Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourTargetServer>" -TargetDatabaseName "<NewDatabaseName>" –ResourceId $GeoBackup.ResourceID

# Verify that the geo-restored database is online
$GeoRestoredDatabase.status
```

>[AZURE.NOTE] Da biste konfigurirali bazu podataka po dovršetku vraćanja, potražite u članku [Konfiguriranje bazu podataka nakon oporavka][]. 


Oporavljene baze podataka bit će TDE omogućena ako je baza podataka izvor TDE omogućena.


## <a name="next-steps"></a>Daljnji koraci
Da biste saznali više o značajki poslovnog continuity izdanja baze podataka SQL Azure, pročitajte [Pregled continuity za baze podataka SQL Azure tvrtke][].

<!--Image references-->

<!--Article references-->
[Azure SQL baze podataka tvrtke continuity pregled]: sql-database-business-continuity.md
[Zahtjev za promjenom DTU kvote]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change
[Konfiguriranje baze podataka nakon oporavka]: ./sql-database-disaster-recovery.md#configure-your-database-after-recovery
[Kako instalirati i konfigurirati Azure PowerShell]: powershell-install-configure.md
[Pregled]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[OSTALE]: ./sql-data-warehouse-restore-database-rest-api.md
[Konfiguriranje baze podataka nakon oporavka]: ./sql-database-disaster-recovery.md#configure-your-database-after-recovery

<!--MSDN references-->
[Vraćanje AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx

<!--Other Web references-->
[Azure Portal]: https://portal.azure.com/
[Instalacijski program platformu Microsoft Web]: https://aka.ms/webpi-azps
