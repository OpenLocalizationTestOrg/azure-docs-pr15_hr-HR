<properties 
    pageTitle="Promjena servisa sloju i performanse razine baze podataka Azure SQL pomoću komponente PowerShell | Microsoft Azure" 
    description="Promjena sloju servisa i razinom performansi baze podataka Azure SQL pokazuje kako promjena veličine baze podataka sustava SQL prema gore ili dolje sa servisom PowerShell. Promjena cijene sloju baze podataka Azure SQL pomoću komponente PowerShell." 
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
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="change-the-service-tier-and-performance-level-pricing-tier-of-a-sql-database-with-powershell"></a>Promjena servisa sloju i performanse razine (cijene sloju) SQL baze podataka pomoću komponente PowerShell


> [AZURE.SELECTOR]
- [Portal za Azure](sql-database-scale-up.md)
- [**PowerShell**](sql-database-scale-up-powershell.md)


Razine usluge i razine performanse opisuju značajke i dostupnih resursa za bazu podataka sustava SQL i može se ažurirati kao potrebama vaše promjene aplikacije. Detalje potražite u članku [Servis razine](sql-database-service-tiers.md).

Imajte na umu da se promjene sloju servisa i/ili razinom performansi baze podataka stvara kopiju izvorne baze podataka u novu razinu performanse i prelazi veze replike. Nema podataka tijekom postupka se gubi, ali tijekom kratak trenutka kada smo premjestite na replike, veza s bazom podataka onemogućeni, tako da neke transakcija u leta mogu vratiti. U ovom prozoru ovisi, ali je u odjeljku 4 sekunde, a u više od 99% slučajeva je manji od 30 sekundi. Vrlo diskovni, osobito ako postoje velike onemogućene su brojevi transakcija u leta u trenutku povezivanja, taj prozor može biti dulji.  

Trajanje cijeli postupak skaliranje gore ovisi o oba na veličinu i servisni sloju baze podataka prije i poslije promjene. Ako, na primjer, 250 GB bazu podataka koja se mijenja da biste iz ili unutar sloj standardnog servisa, trebali biste dovrši unutar 6 sati. Za bazu podataka iste veličine da je Promjena razine performanse unutar servisa sloju Premium, je potrebno dovršiti unutar 3 sata.


- Da biste prijeći na nižu baze podataka, baza podataka mora biti manja od maksimalne dopuštene veličine cilj sloja u sustavu. 
- Prilikom nadogradnje baze podataka s [Zemlj replikacije](sql-database-geo-replication-portal.md) omogućena, najprije morate nadograditi njegov sekundarne baze podataka u sloju željene performanse prije nadogradnje primarnom bazom podataka.
- Kada prijelaz na stariju od Premium servisnog sloja sustava, najprije morate prekinuti svi odnosi zemlj replikacije. Slijedite korake opisana u temi [oporaviti iz programa nedostupnosti](sql-database-disaster-recovery.md) da biste zaustavili proces replikacije između primarnih i aktivni sekundarni baze podataka.
- Ponude servisa Vrati se razlikuju za različite razine servisa. Ako vam se prijelaz na stariju koje mogu izgubiti mogućnost vraćanja na točku u vremenu ili imati donjem razdoblje zadržavanja sigurnosne kopije. Dodatne informacije potražite u članku [i vraćanja sigurnosnih kopija za baze podataka SQL Azure](sql-database-business-continuity.md).
- Nova svojstva baze podataka se ne primjenjuju dok ne dovršite promjene.



**Da biste dovršili ovaj članak potrebno je sljedeće:**

- Azure pretplate. Ako vam je potrebna pretplata na Azure jednostavno kliknite **BESPLATAN RAČUN** pri vrhu ove stranice, a zatim vratite do kraja ovog članka.
- Baze podataka Azure SQL. Ako nemate SQL baze podataka, stvoriti jednu slijedeći korake u ovom se članku: [Stvaranje prve baze podataka SQL Azure](sql-database-get-started.md).
- Azure PowerShell.


[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]



## <a name="change-the-service-tier-and-performance-level-of-your-sql-database"></a>Promjena sloju i performanse razine servisa SQL baze podataka

Pokretanje na [skup-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt619433(v=azure.300\).aspx) cmdlet i postavljanje **-RequestedServiceObjectiveName** razinu performanse željeni cijene sloju; na primjer *S0*, *S1*, *S2*, *S3*, *P1*, *P2*,...

```
$ResourceGroupName = "resourceGroupName"
    
$ServerName = "serverName"
$DatabaseName = "databaseName"

$NewEdition = "Standard"
$NewPricingTier = "S2"

Set-AzureRmSqlDatabase -DatabaseName $DatabaseName -ServerName $ServerName -ResourceGroupName $ResourceGroupName -Edition $NewEdition -RequestedServiceObjectiveName $NewPricingTier
```

  

   


## <a name="sample-powershell-script-to-change-the-service-tier-and-performance-level-of-your-sql-database"></a>Poslušajte skriptu PowerShell da biste promijenili sloju i performanse razina servisa SQL baze podataka

Zamjena ```{variables}``` pomoću vlastitih vrijednosti (ne obuhvaća vitičaste zagrade).

```
$SubscriptionId = "{4cac86b0-1e56-bbbb-aaaa-000000000000}"
    
$ResourceGroupName = "{resourceGroup}"
$Location = "{AzureRegion}"
    
$ServerName = "{server}"
$DatabaseName = "{database}"
    
$NewEdition = "{Standard}"
$NewPricingTier = "{S2}"
    
Add-AzureRmAccount
Set-AzureRmContext -SubscriptionId $SubscriptionId
    
Set-AzureRmSqlDatabase -DatabaseName $DatabaseName -ServerName $ServerName -ResourceGroupName $ResourceGroupName -Edition $NewEdition -RequestedServiceObjectiveName $NewPricingTier
```
        


## <a name="next-steps"></a>Daljnji koraci

- [Promjena veličine izgleda i u](sql-database-elastic-scale-get-started.md)
- [Povezivanje i upita baze podataka SQL pomoću SSMS](sql-database-connect-query-ssms.md)
- [Izvoz baze podataka Azure SQL](sql-database-export-powershell.md)

## <a name="additional-resources"></a>Dodatni resursi

- [Pregled Continuity tvrtke](sql-database-business-continuity.md)
- [Dokumentacija SQL baze podataka](http://azure.microsoft.com/documentation/services/sql-database/)
- [Cmdleti za baze podataka azure SQL] (http://msdn.microsoft.com/library/azure/mt574084https :/ / msdn.microsoft.com/biblioteke/azure/mt619433(v=azure.300\).aspx.aspx)