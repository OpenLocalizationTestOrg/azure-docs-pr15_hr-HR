<properties 
    pageTitle="Upravljanje skup elastic baze podataka (PowerShell) | Microsoft Azure" 
    description="Saznajte kako pomoću ljuske PowerShell za upravljanje sustava skup elastic baze podataka."  
    services="sql-database" 
    documentationCenter="" 
    authors="srinia" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management" 
    ms.date="06/22/2016"
    ms.author="srinia"/>

# <a name="monitor-and-manage-an-elastic-database-pool-with-powershell"></a>Nadzor i upravljanje elastic baze podataka grupe aplikacija sa servisom PowerShell 

> [AZURE.SELECTOR]
- [Portal za Azure](sql-database-elastic-pool-manage-portal.md)
- [PowerShell](sql-database-elastic-pool-manage-powershell.md)
- [C#](sql-database-elastic-pool-manage-csharp.md)
- [T-SQL](sql-database-elastic-pool-manage-tsql.md)

Upravljanje programa [skup elastic baze podataka](sql-database-elastic-pool.md) pomoću cmdleta ljuske PowerShell. 

Uobičajeni kodovi pogreške, potražite u članku [šifre pogreške SQL baze podataka SQL klijentske aplikacije: baze podataka pogreška veze i ostalih problema](sql-database-develop-error-messages.md).

Vrijednosti za grupe nalazi se u [web-mjesto eDTU i u okvir za pohranu ograničenja](sql-database-elastic-pool.md#eDTU-and-storage-limits-for-elastic-pools-and-elastic-databases). 

## <a name="prerequisites"></a>Preduvjeti

* Azure PowerShell 1.0 ili noviji. Detaljne informacije potražite u članku [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md).
* Elastic baze podataka grupe su dostupne samo s V12 baze podataka SQL poslužitelja. Ako imate V11 baze podataka SQL server, [koristite PowerShell za nadogradnju na V12 i stvaranje zajedničko područje](sql-database-upgrade-server-portal.md) u jednom koraku.


## <a name="move-a-database-into-an-elastic-pool"></a>Premještanje baze podataka u elastic grupe aplikacija

Bazu podataka možete premjestiti pojavljivanje ili iščezavanje skup s [Skup AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619433.aspx). 

    Set-AzureRmSqlDatabase -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"

## <a name="change-performance-settings-of-a-pool"></a>Promjena postavki performansi zajedničko područje

Kada suffers performanse, možete promijeniti postavke na resurse kako bi odgovarao rasta. Pomoću cmdleta [Skup AzureRmSqlElasticPool](https://msdn.microsoft.com/library/azure/mt603511.aspx) . Postavite parametar - Dtu eDTUs po grupe aplikacija. Potražite u članku [ograničenja eDTU i pohranu](sql-database-elastic-pool.md#eDTU-and-storage-limits-for-elastic-pools-and-elastic-databases) mogućih vrijednosti.  

    Set-AzureRmSqlElasticPool –ResourceGroupName “resourcegroup1” –ServerName “server1” –ElasticPoolName “elasticpool1” –Dtu 1200 –DatabaseDtuMax 100 –DatabaseDtuMin 50 


## <a name="get-the-status-of-pool-operations"></a>Dohvatite status skup operacije

Stvaranje zajedničko područje može potrajati. Da biste pratili status operacije skup uključujući stvaranja i ažuriranja, koristite cmdlet [Get-AzureRmSqlElasticPoolActivity](https://msdn.microsoft.com/library/azure/mt603812.aspx) .

    Get-AzureRmSqlElasticPoolActivity –ResourceGroupName “resourcegroup1” –ServerName “server1” –ElasticPoolName “elasticpool1” 


## <a name="get-the-status-of-moving-an-elastic-database-into-and-out-of-a-pool"></a>Dohvatite status premještanja elastic baze podataka u i Odjava iz njega u grupu

Premještanje baze podataka može potrajati. Premještanje status pomoću cmdleta [Get-AzureRmSqlDatabaseActivity](https://msdn.microsoft.com/library/azure/mt603687.aspx) značajke evidentiranja.

    Get-AzureRmSqlDatabaseActivity -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"

## <a name="get-resource-usage-data-for-a-pool"></a>Dohvaćanje podataka o korištenju resursa za zajedničko područje

Mjernih podataka koje je moguće dohvatiti kao postotak ograničenje skup resursa:   


| Naziv mjerenja | Opis |
| :-- | :-- |
| CPU\_posto | AVERAGE izračunati Upotreba postotak ograničenje na resurse. |
| Fizička\_podataka\_čitati\_posto | Prosječna/i upotreba u postotka na temelju ograničenje na resurse. |
| zapisnik\_pisanje\_posto | Prosjek pisanje Upotreba resursa u postotak ograničenje na resurse. | 
| DTU\_potrošnje\_posto | Upotreba Prosječna eDTU postotak eDTU ograničenje na resurse | 
| prostor za pohranu\_posto | Upotreba AVERAGE prostora za pohranu u postotak ograničenje prostora na resurse za pohranu. |  
| Zaposlenici zaduženi za\_posto | Maksimalna Istodobni zaposlenici zaduženi za (zahtjeve) u postotka na temelju ograničenje na resurse. |  
| sesije\_posto | Maksimalan broj Istodobni sesija u postotka na temelju ograničenje na resurse. | 
| eDTU_limit | Trenutne Maks elastic skup DTU postavke za ovaj elastic skup Ovaj interval. |
| prostor za pohranu\_ograničenje | Trenutni Maks elastic skup ograničenje prostora za pohranu postavka za ovaj elastic skup u megabajtima Ovaj interval. |
| eDTU\_koristi | Prosječna eDTUs koristi skup u tom intervalu. |
| prostor za pohranu\_koristi | AVERAGE za pohranu koriste skup u tom intervalu u bajtova |

**Metriku preciznosti/zadržavanja razdoblja:**

* Podaci će se vratiti na preciznosti 5 minuta.  
* Zadržavanje podataka je 35 dana.  

Ovaj cmdlet i API ograničava broj redaka koji se mogu biti dohvaćeni u jedan poziv 1000 redaka (oko 3 dana podataka na 5 minuta preciznosti). No tu naredbu možete pozvati više puta s različitim početak i kraj vremenske intervale za učitavanje dodatnih podataka 

Da biste dohvatili metriku:

    $metrics = (Get-AzureRmMetric -ResourceId /subscriptions/<subscriptionId>/resourceGroups/FabrikamData01/providers/Microsoft.Sql/servers/fabrikamsqldb02/elasticPools/franchisepool -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime "4/18/2015" -EndTime "4/21/2015")  


## <a name="get-resource-usage-data-for-an-elastic-database"></a>Dohvaćanje podataka o korištenju resursa za elastic bazu podataka

Ove API-ji isti su kao trenutnu (V12) API-ji za nadzor Upotreba resursa samostalne baze podataka, osim sljedećeg semantičkog razlike. 

Za ovaj API metriku dohvatiti su izražen kao postotak u po Maks eDTUs (ili ekvivalentne kapaciteta podlozi metrike kao što su procesora, IO itd) za tu grupu. Ako, na primjer, 50% Upotreba bilo kojeg od tih metriku upućuje na to da utrošak određenog resursa na 50% na po ograničenje kapaciteta baze podataka resursa u nadređenu grupu. 

Da biste dohvatili metriku:

    $metrics = (Get-AzureRmMetric -ResourceId /subscriptions/<subscriptionId>/resourceGroups/FabrikamData01/providers/Microsoft.Sql/servers/fabrikamsqldb02/databases/myDB -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime "4/18/2015" -EndTime "4/21/2015") 

## <a name="add-an-alert-to-a-pool-resource"></a>Dodavanje upozorenja u skup resursa

Resursi za slanje obavijesti e-poštom ili upozorenja nizovi [krajnje točke URL-a](https://msdn.microsoft.com/library/mt718036.aspx) kada resurs dodirne Upotreba prag koji ste postavili možete dodati upozorenja pravila. Pomoću cmdleta Dodaj AzureRmMetricAlertRule. 

> [AZURE.IMPORTANT]Upotreba resursa nadzor za Elastic grupe ima kašnjenja barem 20 minuta. Postavljanje upozorenja manje od 30 minuta za Elastic grupe trenutno nisu podržani. Bilo koji upozorenja postavljena za Elastic grupe točkom (parametar pod nazivom "-WindowSize" u PowerShell API-JA) manje od 30 minuta možda neće se pokrenuti. Provjerite da bilo koje ste definirali za Elastic grupe koriste tijekom razdoblja (WindowSize) 30 minuta ili više njih.

U ovom se primjeru dodaje upozorenja za početak obavijest kada je skup eDTU potrošnje dolazi iznad praga.

    # Set up your resource ID configurations
    $subscriptionId = '<Azure subscription id>'      # Azure subscription ID
    $location =  '<location'                         # Azure region
    $resourceGroupName = '<resource group name>'     # Resource Group
    $serverName = '<server name>'                    # server name
    $poolName = '<elastic pool name>'                # pool name 

    #$Target Resource ID
    $ResourceID = '/subscriptions/' + $subscriptionId + '/resourceGroups/' +$resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/elasticpools/' + $poolName

    # Create an email action
    $actionEmail = New-AzureRmAlertRuleEmail -SendToServiceOwners -CustomEmail JohnDoe@contoso.com

    # create a unique rule name
    $alertName = $poolName + "- DTU consumption rule"

    # Create an alert rule for DTU_consumption_percent
    Add-AzureRMMetricAlertRule -Name $alertName -Location $location -ResourceGroup $resourceGroupName -TargetResourceId $ResourceID -MetricName "DTU_consumption_percent"  -Operator GreaterThan -Threshold 80 -TimeAggregationOperator Average -WindowSize 00:60:00 -Actions $actionEmail 

## <a name="add-alerts-to-all-databases-in-a-pool"></a>Dodavanje upozorenja za sve baze podataka u zajedničko područje

Sve baze podataka u elastic grupu za slanje obavijesti e-poštom možete dodati pravila za upozorenja ili upozorenja nizova za [krajnje točke URL-a](https://msdn.microsoft.com/library/mt718036.aspx) kada resurs dodirne Upotreba prag postavio upozorenje. 

> [AZURE.IMPORTANT] Upotreba resursa nadzor za Elastic grupe ima kašnjenja barem 20 minuta. Postavljanje upozorenja manje od 30 minuta za Elastic grupe trenutno nisu podržani. Bilo koji upozorenja postavljena za Elastic grupe točkom (parametar pod nazivom "-WindowSize" u PowerShell API-JA) manje od 30 minuta možda neće se pokrenuti. Provjerite da bilo koje ste definirali za Elastic grupe koriste tijekom razdoblja (WindowSize) 30 minuta ili više njih.

U ovom se primjeru dodaje upozorenja za svaki od baza podataka u grupe aplikacija za početak obavijest kada potrošnje DTU tu bazu podataka dolazi iznad praga.

    # Set up your resource ID configurations
    $subscriptionId = '<Azure subscription id>'      # Azure subscription ID
    $location = '<location'                          # Azure region
    $resourceGroupName = '<resource group name>'     # Resource Group
    $serverName = '<server name>'                    # server name
    $poolName = '<elastic pool name>'                # pool name 

    # Get the list of databases in this pool.
    $dbList = Get-AzureRmSqlElasticPoolDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName

    # Create an email action
    $actionEmail = New-AzureRmAlertRuleEmail -SendToServiceOwners -CustomEmail JohnDoe@contoso.com

    # Get resource usage metrics for a database in an elastic database for the specified time interval.
    foreach ($db in $dbList)
    {
    $dbResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/databases/' + $db.DatabaseName

    # create a unique rule name
    $alertName = $db.DatabaseName + "- DTU consumption rule"

    # Create an alert rule for DTU_consumption_percent
    Add-AzureRMMetricAlertRule -Name $alertName  -Location $location -ResourceGroup $resourceGroupName -TargetResourceId $dbResourceId -MetricName "dtu_consumption_percent"  -Operator GreaterThan -Threshold 80 -TimeAggregationOperator Average -WindowSize 00:60:00 -Actions $actionEmail

    # drop the alert rule
    #Remove-AzureRmAlertRule -ResourceGroup $resourceGroupName -Name $alertName
    } 



## <a name="collect-and-monitor-resource-usage-data-across-multiple-pools-in-a-subscription"></a>Prikupljanje i praćenje podataka o korištenju resursa preko više grupe u pretplatu

Kada imate velik broj baza podataka u pretplatu, je naporan praćenje svaki skup elastic zasebno. Umjesto toga cmdleta ljuske PowerShell za SQL baze podataka i T SQL upita možete kombinirati za prikupljanje podataka o korištenju resursa iz većeg broja grupe i svoje baze podataka za praćenje i analize korištenja resursa. [Uzorak implementaciju](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools) takav skup skripte komponente powershell pronaći ćete u spremištu uzoraka GitHub SQL Server uz dokumentaciju o čemu služi i kako je koristiti.

Da biste koristili Ova implementacija uzorka slijedite ove korake navedene.


1. Preuzimanje [skripte i u dokumentaciji o](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools):
2. Izmjena skripte za vaše okruženje. Navedite jednu ili više poslužitelja na kojem se nalaze elastic grupe.
3. Odredite gdje su prikupljene metriku pohraniti telemetrijskih baze podataka. 
4. Prilagodba skriptu da biste odredili trajanje izvođenja u skripti.

Skripte visoke razine, učinite sljedeće:

*   Zbraja sve poslužitelje u određenom Azure pretplate (ili navedeni popis poslužitelja).
*   Pokreće posao u pozadini za svaki poslužitelj. Posao pokreće se u petlji u pravilnim vremenskim razmacima i prikuplja telemetrijskih podataka za sve grupe na poslužitelju. Zatim učitava prikupljene podatke u bazu podataka za navedeni telemetrijskih.
*   Zbraja popis baza podataka u svaki skup za prikupljanje podataka o korištenju baze podataka resursa. Zatim učitava prikupljene podatke u bazu podataka za telemetriju.

Prikupljene mjernih podataka u bazi podataka za telemetriju mogu se analizirati i praćenje stanja elastic grupe i bazama podataka u njemu. Skripta instalira i unaprijed definiranih funkcija vrijednost tablice (TVF) u bazi podataka za telemetriju da biste lakše zbrajanja metriku za određeni vremenski prozor. Ako, na primjer, mogu se rezultate u TVF da bi se prikazala "gornju N elastic grupe s Upotreba Maksimalna eDTU u prozoru određenog vremenskog." Po želji, koristite analitičkih alata kao što je Excel ili dodatka Power BI za slanje upita i analizirati podatke prikupljene.

## <a name="example-retrieve-resource-consumption-metrics-for-a-pool-and-its-databases"></a>Primjer: dohvaćanje metriku potrošnje resursa u grupu i njegov baze podataka

U ovom se primjeru dohvaća metriku potrošnje za dani elastic skup i sve njegove baze podataka. Prikupljene podaci oblikovani i zapisan oblikovani .csv datoteke. Datoteke mogu biti klikanju s programom Excel.

    $subscriptionId = '<Azure subscription id>'       # Azure subscription ID
    $resourceGroupName = '<resource group name>'             # Resource Group
    $serverName = <server name>                              # server name
    $poolName = <elastic pool name>                          # pool name
        
    # Login to Azure account and select the subscription.
    Login-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $subscriptionId
    
    # Get resource usage metrics for an elastic pool for the specified time interval.
    $startTime = '4/27/2016 00:00:00'  # start time in UTC
    $endTime = '4/27/2016 01:00:00'    # end time in UTC
    
    # Construct the pool resource ID and retrive pool metrics at 5 minute granularity.
    $poolResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/elasticPools/' + $poolName
    $poolMetrics = (Get-AzureRmMetric -ResourceId $poolResourceId -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime $startTime -EndTime $endTime) 
    
    # Get the list of databases in this pool.
    $dbList = Get-AzureRmSqlElasticPoolDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName
    
    # Get resource usage metrics for a database in an elastic database for the specified time interval.
    $dbMetrics = @()
    foreach ($db in $dbList)
    {
        $dbResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/databases/' + $db.DatabaseName
        $dbMetrics = $dbMetrics + (Get-AzureRmMetric -ResourceId $dbResourceId -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime $startTime -EndTime $endTime)
    }
    
    #Optionally you can format the metrics and output as .csv file using the following script block.
    $command = {
    param($metricList, $outputFile)

    # Format metrics into a table.
    $table = @()
    foreach($metric in $metricList) { 
      foreach($metricValue in $metric.MetricValues) {
        $sx = New-Object PSObject -Property @{
            Timestamp = $metricValue.Timestamp.ToString()
            MetricName = $metric.Name; 
            Average = $metricValue.Average;
            ResourceID = $metric.ResourceId 
          }
          $table = $table += $sx
      }
    }
    
    # Output the metrics into a .csv file.
    write-output $table | Export-csv -Path $outputFile -Append -NoTypeInformation
    }
    
    # Format and output pool metrics
    Invoke-Command -ScriptBlock $command -ArgumentList $poolMetrics,c:\temp\poolmetrics.csv
    
    # Format and output database metrics
    Invoke-Command -ScriptBlock $command -ArgumentList $dbMetrics,c:\temp\dbmetrics.csv



## <a name="latency-of-elastic-pool-operations"></a>Latencija elastic skup operacije

- Promjena eDTUs min po bazi podataka ili Maks eDTUs po bazi podataka obično dovršava u pet minuta ili manje.
- Promjena eDTUs po skup ovisi o ukupnu količinu prostora koji se koriste sve baze podataka u. Promjena Prosječna 90 minuta ili manje po 100 GB. Ako, na primjer, ako ukupan prostor koristi sve baze podataka u je 200 GB, a zatim očekivani kašnjenje za promjenu eDTU skup po skup je 3 sata ili manje.

## <a name="migrate-from-v11-to-v12-servers"></a>Migrirati iz V11 V12 poslužiteljima

Da biste pokrenuli, zaustavili ili monitor nadogradnje V12 za baze podataka SQL Azure s V11 ili neku drugu verziju pre V12 dostupnih cmdleta ljuske PowerShell.

- [Nadogradnja V12 SQL baze podataka pomoću komponente PowerShell](sql-database-upgrade-server-powershell.md)

Referentnu dokumentaciju o tih cmdleta ljuske PowerShell potražite u članku:


- [Get-AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603582.aspx)
- [Početak AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt619403.aspx)
- [Zaustavi AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603589.aspx)


Cmdlet Zaustavi znači otkazati, bez pauze. Ne postoji način da biste nastavili nadogradnju, osim ponovno počevši od početka. Cmdlet Zaustavi čisti i izdavanje sve odgovarajuće resurse.

## <a name="next-steps"></a>Daljnji koraci

- [Stvaranje elastic poslove](sql-database-elastic-jobs-overview.md) Elastic poslove omogućuju pokrećete T SQL skripte za bilo koji broj baza podataka u.
- U odjeljku [Skaliranje odgovor s bazom podataka SQL Azure](sql-database-elastic-scale-introduction.md): pomoću Alati baze podataka za elastic mjerilo Izlaz, premještanje podataka, upit ili stvorite transakcije.
