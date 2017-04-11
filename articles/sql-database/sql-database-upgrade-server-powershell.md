<properties
    pageTitle="Nadogradnja V12 za baze podataka SQL Azure pomoću komponente PowerShell | Microsoft Azure"
    description="U članku se objašnjava kako nadograditi na V12 za baze podataka SQL Azure uključujući nadogradnju Web i poslovnih baza podataka i nadogradnja V11 poslužitelja migracije njegov baze podataka izravno u elastic baze podataka grupe aplikacija pomoću komponente PowerShell."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="sstein"/>

# <a name="upgrade-to-azure-sql-database-v12-using-powershell"></a>Nadogradnja V12 za baze podataka SQL Azure pomoću komponente PowerShell


> [AZURE.SELECTOR]
- [Portal za Azure](sql-database-upgrade-server-portal.md)
- [PowerShell](sql-database-upgrade-server-powershell.md)


V12 baze podataka SQL je najnovija verzija tako da je nadogradnja V12 baze podataka SQL preporučuje.
SQL baza podataka V12 sadrži brojne [prednosti u odnosu na prethodne verzije](sql-database-v12-whats-new.md) uključujući:

- Povećana kompatibilnost sa sustavom SQL Server.
- Poboljšani premium performanse i nove razine performansi.
- [Grupe elastic baze podataka](sql-database-elastic-pool.md).

Ovaj članak sadrži upute za nadogradnju postojeći poslužitelji za V11 SQL baze podataka i bazama podataka na V12 SQL baze podataka.

Tijekom postupka nadogradnje V12 nadogradite na bilo kojem Web i poslovnih baza podataka novog servisnog sloja sustava tako da su upute za nadogradnju Web i poslovnih baza podataka.

Osim toga, migracije [skup elastic baze podataka](sql-database-elastic-pool.md) može biti troškova učinkovitiji od nadogradnje na pojedinačne performanse razine (cijene razine) za jedan baze podataka. Grupe Pojednostavnite upravljanje bazom podataka jer samo morate upravljati postavkama performansi na resurse umjesto zasebno upravljanje performansama razine pojedinačne baze podataka. Ako imate baze podataka na više poslužitelja, preporučujemo da ih premjestite u istom poslužitelju i snimanja prednost stavljanje ih u zajedničko područje.

Slijedite korake u ovom članku jednostavno premještanje baze podataka s poslužitelja V11 izravno u grupe elastic baze podataka.

Imajte na umu vaših baza podataka će ostati u mreži i nastavili raditi tijekom postupka nadogradnje. Vrijeme stvarni prijelaz na novu razinu performanse privremene ispuštanje veza s bazom podataka može dogoditi vrlo malen trajanja koja je obično oko 90 sekundi, ali može biti pet minuta. Ako je vaša aplikacija [tranzitne kvara rukovanje preuskih vezu](sql-database-connectivity-issues.md) pa je dovoljno zaštitili izostavljanje veze na kraju nadogradnje.

Nadogradnja V12 baze podataka SQL nije moguće poništiti. Nakon nadogradnje na poslužitelju ne može biti vratit će V11.

Nakon nadogradnje na V12, [servis sloju preporuke](sql-database-service-tier-advisor.md) i [preporuke elastic skup](sql-database-elastic-pool-create-portal.md) odmah neće dostupno dok servis je vrijeme za procjenu sustava radnih opterećenja na novom poslužitelju. V11 poslužitelj preporuke povijest ne primjenjuju se na poslužiteljima V12 tako da ne zadržava.  

## <a name="prepare-to-upgrade"></a>Priprema za nadogradnju

- **Nadogradnja sve baze podataka Web- a tvrtke**: korištenje portala sustava ili pomoću [ljuske PowerShell za nadogradnju baze podataka i poslužitelja](sql-database-upgrade-server-powershell.md).
- **Pregled i obustavi zemlj replikacije**: Ako bazu podataka Azure SQL je konfiguriran za zemlj replikacije treba dokumenta njegov trenutnu konfiguraciju i [Zaustavljanje zemlj replikacije](sql-database-geo-replication-portal.md#remove-secondary-database). Kada Nadogradnja Završi konfigurirajte bazu podataka za zemlj replikaciju.
- **Otvorite sljedeće priključke ako imate klijente na programa Azure VM**: klijentski program povezuje V12 baze podataka SQL tijekom klijent na Azure virtualnog računala (VM), morate otvoriti priključak raspona 11000 11999 i 14000 14999 na na VM. Detalje potražite u članku [priključke za V12 SQL baze podataka](sql-database-develop-direct-route-ports-adonet-v12.md).


## <a name="prerequisites"></a>Preduvjeti

Da biste nadogradili na poslužitelju V12 sa servisom PowerShell, morate imati najnoviju Azure PowerShell instaliran i pokrenut. Detaljne informacije potražite u članku [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md).


## <a name="configure-your-credentials-and-select-your-subscription"></a>Konfiguriranje vjerodajnice i odaberite pretplate

Da biste pokrećete PowerShell cmdleti za pretplatu Azure prvo uspostavite pristupa na račun za Azure. Pokrenite sljedeću naredbu, a prikazat će vam se upit na zaslon za prijavu u da biste unijeli vjerodajnice. Koristite e-pošte i lozinku koje koristite za prijavu na portal za Azure.

    Add-AzureRmAccount

Nakon uspješnog prijave trebali biste vidjeti neke informacije na zaslonu koja sadrži ste se prijavili pomoću ID-a i Azure pretplate kojima imate pristup.

Odaberite pretplatu koju želite surađivati s vama je potrebna pretplata Id (**-SubscriptionId**) ili pretplatu imenujte (**-SubscriptionName**). Kopirate u prethodnom koraku ili ako imate više pretplata pokrenite cmdlet **Get-AzureRmSubscription** i kopirajte željene pretplate podatke iz sa skupom rezultata.

Uz svoje podatke za pretplatu da biste postavili trenutne pretplate, pokrenite sljedeći cmdlet:

    Set-AzureRmContext -SubscriptionId 4cac86b0-1e56-bbbb-aaaa-000000000000

Sljedeće naredbe će se izvoditi na temelju pretplatu koju ste upravo odabrali iznad.

## <a name="get-recommendations"></a>Preporuke

Da biste dobili preporuke za nadogradnju poslužitelja pokrenite sljedeći cmdlet:

    $hint = Get-AzureRmSqlServerUpgradeHint -ResourceGroupName “resourcegroup1” -ServerName “server1”

Dodatne informacije potražite u članku [Stvaranje grupe aplikacija programa elastic baze podataka](sql-database-elastic-pool-create-portal.md) i [baze podataka SQL Azure cijene sloju preporuke](sql-database-service-tier-advisor.md).



## <a name="start-the-upgrade"></a>Pokretanje nadogradnje

Da biste započeli nadogradnju poslužitelja pokrenite sljedeći cmdlet:

    Start-AzureRmSqlServerUpgrade -ResourceGroupName “resourcegroup1” -ServerName “server1” -ServerVersion 12.0 -DatabaseCollection $hint.Databases -ElasticPoolCollection $hint.ElasticPools  


Kada pokrenete postupak nadogradnje naredba će početi. Možete prilagoditi Izlaz na preporuke i dati uređivati preporuke za ovaj cmdlet.


## <a name="upgrade-a-server"></a>Nadogradnja na poslužitelju


    # Adding the account
    #
    Add-AzureRmAccount

    # Setting the variables
    #
    $SubscriptionName = 'YOUR_SUBSCRIPTION'
    $ResourceGroupName = 'YOUR_RESOURCE_GROUP'
    $ServerName = 'YOUR_SERVER'

    # Selecting the right subscription
    #
    Set-AzureRmContext -SubscriptionName $SubscriptionName

    # Getting the upgrade recommendations
    #
    $hint = Get-AzureRmSqlServerUpgradeHint -ResourceGroupName $ResourceGroupName -ServerName $ServerName

    # Starting the upgrade process
    #
    Start-AzureRmSqlServerUpgrade -ResourceGroupName $ResourceGroupName -ServerName $ServerName -ServerVersion 12.0 -DatabaseCollection $hint.Databases -ElasticPoolCollection $hint.ElasticPools  


## <a name="custom-upgrade-mapping"></a>Prilagođeno nadograditi mapiranja

Ako preporuke nisu prikladna za poslužitelj i tvrtke slova, pa možete odabrati kako baze podataka se nadograditi i ih možete mapirati jedan ili elastic baza podataka.

Parametri ElasticPoolCollection i DatabaseCollection nisu obavezni:

    # Creating elastic pool mapping
    #
    $elasticPool = New-Object -TypeName Microsoft.Azure.Management.Sql.Models.UpgradeRecommendedElasticPoolProperties
    $elasticPool.DatabaseDtuMax = 100
    $elasticPool.DatabaseDtuMin = 0
    $elasticPool.Dtu = 800
    $elasticPool.Edition = "Standard"
    $elasticPool.DatabaseCollection = ("DB1", “DB2”, “DB3”, “DB4”)
    $elasticPool.Name = "elasticpool_1"
    $elasticPool.StorageMb = 800

    # Creating single database mapping for 2 databases. DBMain1 mapped to S0 and DBMain2 mapped to S2
    #
    $databaseMap1 = New-Object -TypeName Microsoft.Azure.Management.Sql.Models.UpgradeDatabaseProperties
    $databaseMap1.Name = "DBMain1"
    $databaseMap1.TargetEdition = "Standard"
    $databaseMap1.TargetServiceLevelObjective = "S0"

    $databaseMap2 = New-Object -TypeName Microsoft.Azure.Management.Sql.Models.UpgradeDatabaseProperties
    $databaseMap2.Name = "DBMain2"
    $databaseMap2.TargetEdition = "Standard"
    $databaseMap2.TargetServiceLevelObjective = "S2"

    # Starting the upgrade
    #
    Start-AzureRmSqlServerUpgrade –ResourceGroupName resourcegroup1 –ServerName server1 -ServerVersion 12.0 -DatabaseCollection @($databaseMap1, $databaseMap2) -ElasticPoolCollection @($elasticPool)



## <a name="monitor-databases-after-upgrading-to-sql-database-v12"></a>Praćenje baze podataka nakon nadogradnje na V12 baze podataka SQL


Nakon nadogradnje, preporučuje se praćenje baze podataka aktivno da biste bili sigurni aplikacija trenutno pokrenuto željene performanse i optimizirati korištenje po potrebi.

Uz nadzor možete nadzirati elastic bazu podataka grupe [pomoću portala za](sql-database-elastic-pool-manage-portal.md) pojedinačne baze podataka ili sa [servisom PowerShell](sql-database-elastic-pool-manage-powershell.md)


**Resursa potrošnje podataka:** Za Basic, standardna i Premium baze podataka resursa potrošnje podataka je dostupan putem u [sys.dm_ db_ resource_stats](http://msdn.microsoft.com/library/azure/dn800981.aspx) DMV u bazi podataka za korisnika. U ovom DMV nudi blizu u stvarnom vremenu potrošnje informacije o resursu na 15 drugi preciznosti prethodne h operacije. Postotak potrošnje DTU za interval je izračunati kao potrošnju maksimalan postotak dimenzije procesora, IO i zapisnika. Evo upita za izračunavanje prosjeka potrošnju postotak DTU putem zadnjem satu.

    SELECT end_time
         , (SELECT Max(v)
             FROM (VALUES (avg_cpu_percent)
                         , (avg_data_io_percent)
                         , (avg_log_write_percent)
           ) AS value(v)) AS [avg_DTU_percent]
    FROM sys.dm_db_resource_stats
    ORDER BY end_time DESC;

Dodatne informacije o praćenju:

- [Baze podataka SQL azure performanse smjernice za jednu baze podataka](http://msdn.microsoft.com/library/azure/dn369873.aspx).
- [Cijene i performanse zahtjevi za grupe aplikacija programa elastic baze podataka](sql-database-elastic-pool-guidance.md).
- [Nadzor baze podataka SQL Azure pomoću dinamičke Upravljanje prikazima](sql-database-monitoring-with-dmvs.md)



**Upozorenja:** Postavljanje "Upozorenja" na portalu za Azure da vas obavijesti kada potrošnje DTU nadograđena baze podataka izvršenja određene visoku razinu. Baza podataka upozorenja možete postaviti na portalu za Azure za različite metriku performanse kao što su DTU, procesora, IO i zapisnika. Otvorite bazu podataka i odaberite **upozorenja pravila** plohu **Postavke** .

Na primjer, možete postaviti upozorenja e-pošte na "DTU postotak" ako prosječnu vrijednost postotak DTU premašuje 75% putem posljednjih 5 minuta. Pogledajte da biste [primali upozorenja](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) da biste saznali više o konfiguriranju upozorenja.



## <a name="next-steps"></a>Daljnji koraci

- [Stvaranje grupe aplikacija programa elastic baze podataka](sql-database-elastic-pool-create-portal.md) i dodati neke ili sve baze podataka u na resurse.
- [Promjena servisa sloju i performanse razine bazu podataka](sql-database-scale-up.md).



## <a name="related-information"></a>Povezane informacije

- [Get-AzureRmSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603582.aspx)
- [Početak AzureRmSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt619403.aspx)
- [Zaustavi AzureRmSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603589.aspx)
