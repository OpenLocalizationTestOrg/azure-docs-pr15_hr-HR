<properties
    pageTitle="Stvaranje nove grupe aplikacija elastic baze podataka pomoću komponente PowerShell | Microsoft Azure"
    description="Saznajte kako pomoću ljuske PowerShell skaliranje iz baze podataka SQL Azure resursi stvaranjem skup skalabilni elastic baze podataka da biste upravljali više baza podataka."
    services="sql-database"
    documentationCenter=""
    authors="srinia"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management"
    ms.date="05/27/2016"
    ms.author="srinia"/>

# <a name="create-a-new-elastic-database-pool-with-powershell"></a>Stvaranje nove grupe aplikacija elastic baze podataka pomoću komponente PowerShell

> [AZURE.SELECTOR]
- [Portal za Azure](sql-database-elastic-pool-create-portal.md)
- [PowerShell](sql-database-elastic-pool-create-powershell.md)
- [C#](sql-database-elastic-pool-create-csharp.md)


Saznajte kako stvoriti na [skup elastic baze podataka](sql-database-elastic-pool.md) pomoću cmdleta ljuske PowerShell. 

Uobičajeni kodovi pogreške, potražite u članku [šifre pogreške SQL baze podataka SQL klijentske aplikacije: baze podataka pogreška veze i ostalih problema](sql-database-develop-error-messages.md).

> [AZURE.NOTE] Elastic grupe su obično dostupnih (GA) u svim regijama Azure osim Sjeverna središnje NAM i Zapad Indija gdje je trenutno u pretpregledu.  Čim objavit ćemo zajedno GA elastic grupe u te regijama. Osim toga, elastic grupe trenutno ne podržava baze podataka koje koriste [u memoriji OLTP ili analize u memoriji](sql-database-in-memory.md).


Morate imati Azure PowerShell 1.0 ili noviji. Detaljne informacije potražite u članku [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md).

## <a name="create-a-new-pool"></a>Stvaranje nove grupe aplikacija

Cmdlet [Novo AzureRmSqlElasticPool](https://msdn.microsoft.com/library/azure/mt619378.aspx) stvara novu grupu. Vrijednosti za eDTU po skup, min i Maks Dtus ograničene su vrijednošću sloju servisa (basic, standard ili premium). Potražite u članku [eDTU i pohranu ograničenjima elastic grupe i elastic baze podataka](sql-database-elastic-pool.md#eDTU-and-storage-limits-for-elastic-pools-and-elastic-databases).

    New-AzureRmSqlElasticPool -ResourceGroupName "resourcegroup1" -ServerName "server1" -ElasticPoolName "elasticpool1" -Edition "Standard" -Dtu 400 -DatabaseDtuMin 10 -DatabaseDtuMax 100


## <a name="create-a-new-elastic-database-in-a-pool"></a>Stvaranje nove baze podataka elastic u zajedničko područje

Pomoću cmdleta [New AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619339.aspx) i postavite parametar **ElasticPoolName** skup cilj. Da biste premjestili postojeće baze podataka u zajedničko područje, potražite u članku [Premještanje baze podataka u elastic grupe aplikacija](sql-database-elastic-pool-manage-powershell.md#Move-a-database-into-an-elastic-pool).

    New-AzureRmSqlDatabase -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"

## <a name="create-a-pool-and-populate-it-with-multiple-new-databases"></a>Stvaranje zajedničko područje i popunjavanje s više nove baze podataka 

Stvaranje velik broj baza podataka u zajedničko područje može potrajati po završetku pomoću portala i PowerShell cmdleti istovremeno možete stvoriti samo jednu bazu podataka. Da biste automatizirali stvaranje u novu grupu, potražite u članku [CreateOrUpdateElasticPoolAndPopulate ](https://gist.github.com/billgib/d80c7687b17355d3c2ec8042323819ae).   

## <a name="example-create-a-pool-using-powershell"></a>Primjer: Stvaranje skupa pomoću komponente PowerShell 

Ova skripta stvara novu grupu resursa Azure i novi poslužitelj. Kada se to od vas zatraži, navedite administratorskog korisničkog imena i lozinke za novi poslužitelj (ne Azure vjerodajnice).

    $subscriptionId = '<your Azure subscription id>'
    $resourceGroupName = '<resource group name>'
    $location = '<datacenter location>'
    $serverName = '<server name>'
    $poolName = '<pool name>'
    $databaseName = '<database name>'

    Login-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $subscriptionId

    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $serverName -Location $location -ServerVersion "12.0"
    New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $serverName -FirewallRuleName "rule1" -StartIpAddress "192.168.0.198" -EndIpAddress "192.168.0.199"

    New-AzureRmSqlElasticPool -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName -Edition "Standard" -Dtu 400 -DatabaseDtuMin 10 -DatabaseDtuMax 100

    New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -ElasticPoolName $poolName -MaxSizeBytes 10GB



## <a name="next-steps"></a>Daljnji koraci

- [Upravljanje vaše grupe aplikacija](sql-database-elastic-pool-manage-powershell.md)
- [Stvaranje elastic poslove](sql-database-elastic-jobs-overview.md) Elastic poslove omogućuju pokrećete T SQL skripte za bilo koji broj baza podataka u.
- [Vremensko mjerilo s bazom podataka SQL Azure](sql-database-elastic-scale-introduction.md): korištenje elastic Alati baze podataka skaliranje istek.

