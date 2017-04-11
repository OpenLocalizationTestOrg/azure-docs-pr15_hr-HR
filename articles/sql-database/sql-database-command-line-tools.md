<properties
    pageTitle="Upravljanje bazom podataka Azure SQL sa servisom PowerShell | Microsoft Azure"
    description="Upravljanje bazom podataka SQL Azure sa servisom PowerShell."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="sstein"/>

# <a name="manage-azure-sql-database-with-powershell"></a>Upravljanje bazom podataka Azure SQL pomoću komponente PowerShell


> [AZURE.SELECTOR]
- [Portal za Azure](sql-database-manage-portal.md)
- [SQL transakcija (SSMS)](sql-database-manage-azure-ssms.md)
- [PowerShell](sql-database-manage-powershell.md)

Ova tema prikazuje cmdleta ljuske PowerShell koje služe za izvođenje mnoge zadatke baze podataka SQL Azure. Cjelovit popis potražite u članku [Cmdleti za baze podataka SQL Azure](https://msdn.microsoft.com/library/mt574084.aspx).


## <a name="create-a-resource-group"></a>Stvaranje grupa resursa

Stvaranje grupe resursa za naše SQL baze podataka i povezani resursi Azure pomoću cmdleta [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/azure/mt759837.aspx) .

```
$resourceGroupName = "resourcegroup1"
$resourceGroupLocation = "northcentralus"
New-AzureRmResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation
```

Dodatne informacije potražite u članku [Korištenje Azure PowerShell s Azure Voditelj resursa](../powershell-azure-resource-manager.md).
Primjer skripte potražite u članku [Stvaranje baze podataka SQL skriptu PowerShell](sql-database-get-started-powershell.md#create-a-sql-database-powershell-script).

## <a name="create-a-sql-database-server"></a>Stvaranje baze podataka SQL server

Stvaranje baze podataka SQL server pomoću cmdleta [New-AzureRmSqlServer](https://msdn.microsoft.com/library/azure/mt603715.aspx) . Zamijenite *Poslužitelj1* naziv poslužitelja. Nazivi poslužitelja mora biti jedinstvena na svim poslužiteljima baze podataka SQL Azure. Ako naziv poslužitelja već preuzeli, prikazat će se pogreška. Ta se naredba može potrajati nekoliko minuta. Grupa resursa mora već postojati u svoju pretplatu.

```
$resourceGroupName = "resourcegroup1"

$sqlServerName = "server1"
$sqlServerVersion = "12.0"
$sqlServerLocation = "northcentralus"
$serverAdmin = "loginname"
$serverPassword = "password" 
$securePassword = ConvertTo-SecureString –String $serverPassword –AsPlainText -Force
$creds = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $serverAdmin, $securePassword
    

$sqlServer = New-AzureRmSqlServer -ServerName $sqlServerName `
 -SqlAdministratorCredentials $creds -Location $sqlServerLocation `
 -ResourceGroupName $resourceGroupName -ServerVersion $sqlServerVersion
```

Dodatne informacije potražite u članku [što je SQL baze podataka](sql-database-technical-overview.md). Primjer skripte potražite u članku [Stvaranje baze podataka SQL skriptu PowerShell](sql-database-get-started-powershell.md#create-a-sql-database-powershell-script).


## <a name="create-a-sql-database-server-firewall-rule"></a>Stvaranje pravila vatrozida baze podataka SQL server

Stvaranje pravila vatrozida za pristup poslužitelju pomoću cmdleta [New-AzureRmSqlServerFirewallRule](https://msdn.microsoft.com/library/azure/mt603860.aspx) . Pokrenite sljedeću naredbu, zamjena IP adrese početka i završetka valjane vrijednosti za vaš klijent. Grupa resursa i poslužitelja mora postojati u svoju pretplatu.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$firewallRuleName = "firewallrule1"
$firewallStartIp = "0.0.0.0"
$firewallEndIp = "255.255.255.255"

New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -FirewallRuleName $firewallRuleName `
 -StartIpAddress $firewallStartIp -EndIpAddress $firewallEndIp
```

Da biste omogućili pristup drugih servisa Azure na poslužitelju, stvaranje pravila vatrozida i oba na `-StartIpAddress` i `-EndIpAddress` da biste **0.0.0.0**. Ovo pravilo vatrozida za posebne omogućuje sve Azure promet za pristup poslužitelju.

Dodatne informacije potražite u članku [Vatrozida za baze podataka SQL Azure](https://msdn.microsoft.com/library/azure/ee621782.aspx). Primjer skripte potražite u članku [Stvaranje baze podataka SQL skriptu PowerShell](sql-database-get-started-powershell.md#create-a-sql-database-powershell-script).


## <a name="create-a-sql-database-blank"></a>Stvaranje baze podataka SQL (prazno)

Stvaranje baze podataka pomoću cmdleta [New-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619339.aspx) . Grupa resursa i poslužitelja mora postojati u svoju pretplatu. 

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$databaseName = "database1"
$databaseEdition = "Standard"
$databaseServiceLevel = "S0"

$currentDatabase = New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -DatabaseName $databaseName `
 -Edition $databaseEdition -RequestedServiceObjectiveName $databaseServiceLevel
```

Dodatne informacije potražite u članku [što je SQL baze podataka](sql-database-technical-overview.md). Primjer skripte potražite u članku [Stvaranje baze podataka SQL skriptu PowerShell](sql-database-get-started-powershell.md#create-a-sql-database-powershell-script).


## <a name="change-the-performance-level-of-a-sql-database"></a>Promjena razine performanse SQL baze podataka

Promjena veličine bazom podataka prema gore ili dolje pomoću cmdleta [Skup AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619433.aspx) . Grupa resursa, poslužitelj i bazu podataka mora postojati u svoju pretplatu. Postavljanje na `-RequestedServiceObjectiveName` za jedan razmak (kao što je sljedeća isječak) za osnovni sloju. Postavite *S0* *S1*, *P1*, *P6*, itd., kao u prethodnom primjeru za druge razine.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$databaseName = "database1"
$databaseEdition = "Basic"
$databaseServiceLevel = " "

Set-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -DatabaseName $databaseName `
 -Edition $databaseEdition -RequestedServiceObjectiveName $databaseServiceLevel
```

Dodatne informacije potražite u članku [Mogućnosti SQL baze podataka i performanse: objašnjenje što dostupna je u sloju svaki servis](sql-database-service-tiers.md). Primjer skripte potražite u članku [skriptu PowerShell uzorka da biste promijenili sloju i performanse razina servisa SQL baze podataka](sql-database-scale-up-powershell.md#sample-powershell-script-to-change-the-service-tier-and-performance-level-of-your-sql-database).

## <a name="copy-a-sql-database-to-the-same-server"></a>Kopiranje bazi podataka sustava SQL na isti poslužitelj

Kopirajte bazu podataka sustava SQL na isti poslužitelj pomoću cmdleta [New-AzureRmSqlDatabaseCopy](https://msdn.microsoft.com/library/azure/mt603644.aspx) . Postavljanje na `-CopyServerName` i `-CopyResourceGroupName` iste vrijednosti kao grupu izvor bazu podataka poslužitelja i resursa.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"
$databaseName = "database1"

$copyDatabaseName = "database1_copy"
$copyServerName = $sqlServerName
$copyResourceGroupName = $resourceGroupName

New-AzureRmSqlDatabaseCopy -DatabaseName $databaseName `
 -ServerName $sqlServerName -ResourceGroupName $resourceGroupName `
 -CopyDatabaseName $copyDatabaseName -CopyServerName $sqlServerName `
 -CopyResourceGroupName $copyResourceGroupName
```

Dodatne informacije potražite u članku [kopiranje baze podataka SQL Azure](sql-database-copy.md). Primjer skripte potražite u članku [kopiranje baze podataka SQL skriptu PowerShell](sql-database-copy-powershell.md#example-powershell-script).


## <a name="delete-a-sql-database"></a>Brisanje baze podataka SQL

Brisanje baze podataka SQL pomoću cmdleta [Ukloni AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619368.aspx) . Grupa resursa, poslužitelj i bazu podataka mora postojati u svoju pretplatu.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"
$databaseName = "database1"

Remove-AzureRmSqlDatabase -DatabaseName $databaseName `
 -ServerName $sqlServerName -ResourceGroupName $resourceGroupName
```

## <a name="delete-a-sql-database-server"></a>Brisanje baze podataka SQL server

Brisanje poslužitelj s cmdlet [Ukloni AzureRmSqlServer](https://msdn.microsoft.com/library/azure/mt603488.aspx) .

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

Remove-AzureRmSqlServer -ServerName $sqlServerName -ResourceGroupName $resourceGroupName
```

## <a name="create-and-manage-elastic-database-pools-using-powershell"></a>Stvaranje i upravljanje grupe elastic baze podataka pomoću komponente PowerShell

Detalje o stvaranju grupe elastic baze podataka pomoću komponente PowerShell potražite u članku [Stvaranje nove grupe aplikacija elastic baze podataka sa servisom PowerShell](sql-database-elastic-pool-create-powershell.md).

Detalje o upravljanju grupe elastic baze podataka pomoću komponente PowerShell potražite u članku [nadzor i upravljanje elastic baze podataka grupe aplikacija sa servisom PowerShell](sql-database-elastic-pool-manage-powershell.md).



## <a name="related-information"></a>Povezane informacije

- [Cmdleti za baze podataka Azure SQL](https://msdn.microsoft.com/library/azure/mt574084.aspx)
- [Azure Cmdlet Reference](https://msdn.microsoft.com/library/azure/dn708514.aspx)
