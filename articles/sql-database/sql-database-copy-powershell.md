<properties 
    pageTitle="Kopiranje baze podataka Azure SQL pomoću komponente PowerShell | Microsoft Azure" 
    description="Stvaranje kopije baze podataka Azure SQL pomoću komponente PowerShell" 
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="09/08/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="copy-an-azure-sql-database-using-powershell"></a>Kopiranje baze podataka Azure SQL pomoću komponente PowerShell


> [AZURE.SELECTOR]
- [Pregled](sql-database-copy.md)
- [Portal za Azure](sql-database-copy-portal.md)
- [PowerShell](sql-database-copy-powershell.md)
- [T-SQL](sql-database-copy-transact-sql.md)

U ovom se članku objašnjava da biste kopirali SQL baze podataka pomoću komponente PowerShell na isti poslužitelj na drugi poslužitelj ili kopirajte bazu podataka u [skup elastic baze podataka](sql-database-elastic-pool.md). Kopiranje baze podataka koristi cmdleta [New-AzureRmSqlDatabaseCopy](https://msdn.microsoft.com/library/mt603644.aspx) . 


Da biste dovršili ovaj članak, potrebno je sljedeće:

- Azure SQL baze podataka (baze podataka da biste kopirali). Ako nemate SQL baze podataka, stvoriti jednu slijedeći korake u ovom se članku: [Stvaranje prve baze podataka SQL Azure](sql-database-get-started.md).
- Najnovija verzija Azure PowerShell. Detaljne informacije potražite u članku [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md).


Mnoge nove značajke baze podataka SQL podržani su samo ako koristite [model implementacije Azure Voditelj resursa](../azure-resource-manager/resource-group-overview.md), da bi se primjerima pomoću [cmdleta ljuske PowerShell za baze podataka SQL Azure](https://msdn.microsoft.com/library/azure/mt574084.aspx) za Voditelj resursa. Postojeći model [baze podataka SQL Azure (klasični) cmdleta](https://msdn.microsoft.com/library/azure/dn546723.aspx) za klasični uvođenje podržani za kompatibilnost s prijašnjim verzijama, no preporučujemo da pomoću cmdleta Voditelj resursa.


>[AZURE.NOTE] Ovisno o veličini baze podataka, postupak kopiranja može potrajati neko vrijeme da biste dovršili.


## <a name="copy-a-sql-database-to-the-same-server"></a>Kopiranje bazi podataka sustava SQL na isti poslužitelj

Da biste stvorili kopiju na istom poslužitelju, izostavite na `-CopyServerName` parametar (ili postaviti na isti poslužitelj).

    New-AzureRmSqlDatabaseCopy -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -CopyDatabaseName "database1_copy"

## <a name="copy-a-sql-database-to-a-different-server"></a>Kopiranje bazi podataka sustava SQL na drugi poslužitelj

Da biste stvorili kopiju na drugi poslužitelj, uvrstite u `-CopyServerName` parametar i postavite ga na drugi poslužitelj. *Kopiraj* poslužitelja mora već postojati. Ako je u grupi različite resursa, a zatim morate uključiti u `-CopyResourceGroupName` parametar.

    New-AzureRmSqlDatabaseCopy -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -CopyServerName "server2" -CopyDatabaseName "database1_copy"


## <a name="copy-a-sql-database-into-an-elastic-database-pool"></a>Kopirajte SQL baze podataka u grupe aplikacija programa elastic baze podataka

Da biste stvorili kopiju baze podataka SQL u zajedničko područje, postavite na `-ElasticPoolName` parametar postojeće grupe aplikacija.

    New-AzureRmSqlDatabaseCopy -ResourceGroupName "resourcegoup1" -ServerName "server1" -DatabaseName "database1" -CopyResourceGroupName "poolResourceGroup" -CopyServerName "poolServer1" -CopyDatabaseName "database1_copy" -ElasticPoolName "poolName"


## <a name="resolve-logins"></a>Razrješavanje prijave

Da biste riješili prijave nakon dovršetka postupka Kopiraj, potražite u članku [Razrješavanje prijave](sql-database-copy-transact-sql.md#resolve-logins-after-the-copy-operation-completes)


## <a name="example-powershell-script"></a>Primjer skriptu komponente PowerShell

Sljedeću skriptu pretpostavlja svih grupa resursa, poslužitelji i na resurse već postoji (zamjena vrijednostima varijabli postojećih resursa). Sve se mora nalaziti, osim kopiju baze podataka.

    # Sign in to Azure and set the subscription to work with
    # ------------------------------------------------------
    $SubscriptionId = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    Add-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $SubscriptionId
    
    
    # SQL database source (the existing database to copy)
    # ---------------------------------------------------
    $sourceDbName = "db1"
    $sourceDbServerName = "server1"
    $sourceDbResourceGroupName = "rg1"
    
    # SQL database copy (the new db to be created)
    # --------------------------------------------
    $copyDbName = "db1_copy"
    $copyDbServerName = "server2"
    $copyDbResourceGroupName = "rg2"
    
    # Copy a database to the same server
    # ----------------------------------
    New-AzureRmSqlDatabaseCopy -ResourceGroupName $sourceDbResourceGroupName -ServerName $sourceDbServerName -DatabaseName $sourceDbName -CopyDatabaseName $copyDbName
    
    # Copy a database to a different server
    # -------------------------------------
    New-AzureRmSqlDatabaseCopy -ResourceGroupName $sourceDbResourceGroupName -ServerName $sourceDbServerName -DatabaseName $sourceDbName -CopyResourceGroupName $copyDbResourceGroupName -CopyServerName $copyDbServerName -CopyDatabaseName $copyDbName
    
    # Copy a database into an elastic database pool
    # ---------------------------------------------
    $poolName = "pool1"
    
    New-AzureRmSqlDatabaseCopy -ResourceGroupName $sourceDbResourceGroupName -ServerName $sourceDbServerName -DatabaseName $sourceDbName -CopyResourceGroupName $copyDbResourceGroupName -CopyServerName $copyDbServerName -ElasticPoolName $poolName -CopyDatabaseName $copyDbName



    

## <a name="next-steps"></a>Daljnji koraci

- Potražite u članku [kopiranje baze podataka Azure SQL](sql-database-copy.md) pregled kopiranje baze podataka SQL Azure.
- Potražite u članku [kopiranje baze podataka Azure SQL pomoću portala za Azure](sql-database-copy-portal.md) da biste kopirali baze podataka pomoću portala za Azure.
- Potražite u članku [kopiju baze podataka Azure SQL pomoću T SQL](sql-database-copy-transact-sql.md) za kopiranje baze podataka pomoću Transact-SQL.
- Pogledajte [kako upravljati sigurnost baze podataka Azure SQL nakon oporavka Izrada](sql-database-geo-replication-security-config.md) dodatne informacije o upravljanju korisnicima i prijave pri kopiranju baze podataka na drugi poslužitelj za logičke.


## <a name="additional-resources"></a>Dodatni resursi

- [Novi AzureRmSqlDatabase](https://msdn.microsoft.com/library/mt603644.aspx)
- [Get-AzureRmSqlDatabaseActivity](https://msdn.microsoft.com/library/mt603687.aspx)
- [Upravljanje prijave](sql-database-manage-logins.md)
- [Povezivanje s bazom podataka SQL s SQL Server Management Studio i obaviti primjer T SQL upita](sql-database-connect-query-ssms.md)
- [Izvoz u BACPAC baze podataka](sql-database-export.md)
- [Pregled Continuity tvrtke](sql-database-business-continuity.md)
- [Dokumentacija SQL baze podataka](https://azure.microsoft.com/documentation/services/sql-database/)
- [Azure SQL baze podataka komponente PowerShell Cmdlet Reference](https://msdn.microsoft.com/library/mt574084.aspx)
