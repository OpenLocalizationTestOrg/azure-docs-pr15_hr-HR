< svojstva
    
    pageTitle="Upgrade to the latest elastic database client library | Microsoft Azure" 
    description="Upgrade apps and libraries using Nuget" 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="ddove"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove" />

# <a name="upgrade-an-app-to-use-the-latest-elastic-database-client-library"></a>Nadogradnja aplikacije programa da biste koristili najnovije biblioteku klijent elastic baze podataka

Nove verzije u [biblioteci klijent Elastic baze podataka](sql-database-elastic-database-client-library.md) dostupni su putem NuGetand upravitelja NuGetPackage sučelja u Visual Studio. Nadograđuje sadrže popravci programskih pogrešaka i podršku posvećeno nove mogućnosti biblioteke klijenta.

**Za najnoviju verziju:** Idite na [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).

Ponovno stvaranje aplikacije s novu biblioteku, kao i promjena metapodatka Shard karte Upravitelj postojeće spremljene u bazi podataka SQL Azure za podršku nove značajke.

Izvođenje sljedeće korake redoslijedom osigurava da stare verzije klijentska biblioteka više nisu prisutne u okruženju sustava kada objekata metapodataka ažuriraju, što znači da se objekti metapodataka stare verzije neće stvarati nakon nadogradnje.   

## <a name="upgrade-steps"></a>Koraci za nadogradnju

**1. nadogradnju aplikacije.** U Visual Studio, preuzmite i referencu na najnoviju verziju klijentskog biblioteke u sve projekte razvoj koje koriste u biblioteku zatim ponovno stvaranje i implementacija. 

 * U Visual Studio rješenje, odaberite **Alati** --> **Upravitelj paketa NuGet** -->  **Upravljanje NuGet paketa rješenja**. 
 * (Visual Studio 2013) U lijevom oknu odaberite **ažuriranja**, a zatim gumb **Ažuriranje** na paket **Azure SQL baza podataka Elastic mjerilo klijentska biblioteka** koja će se prikazati u prozoru.
 * (Visual Studio 2015) Postavite okvir filtar da biste **nadogradili dostupna**. Odaberite paket da biste ažurirali pa kliknite gumb **Ažuriranje** .
    
 
 * Stvorite i implementirajte. 

**2. nadogradite skripte.** Ako koristite skripti **ljuske PowerShell** za upravljanje shards, [Preuzmite nova verzija za biblioteku](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/) i njezino kopiranje u direktoriju s kojeg izvršavanje skripti. 

**3. nadogradite na servisu Podijeli cirkularnog pisma.** Ako koristite elastic alata baze podataka podijeljene cirkularnog pisma da biste preuredili sharded podatke, [Preuzmite i implementacija najnoviju verziju alata](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge/). Detaljne korake nadogradnje za servis možete pronaći [ovdje](sql-database-elastic-scale-overview-split-and-merge.md). 

**4. nadogradnje baze podataka programa Shard karte Upravitelj**. Nadogradite metapodataka za podršku vaše Shard karte u bazi podataka SQL Azure.  Postoje dva načina na koje možete izvršiti to, pomoću programa PowerShell ili C#. Obje mogućnosti možete prikazano u nastavku.

***Mogućnost 1: Nadogradnja metapodataka pomoću komponente PowerShell***

1. Preuzmite najnovije naredbenog retka utility za NuGet iz [ovdje](http://nuget.org/nuget.exe) i spremite u mapu. 

2. Otvorite naredbeni redak, dođite do mape u istom i problema naredbu:`nuget install Microsoft.Azure.SqlDatabase.ElasticScale.Client`

3. Otvorite podmapu koja sadrži nova verzija klijenta DLL ste upravo preuzeli, na primjer:`cd .\Microsoft.Azure.SqlDatabase.ElasticScale.Client.1.0.0\lib\net45`

4. Preuzmite nadogradnje scriptlet klijent elastic baze podataka iz [Centra za skripte](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-Elastic-6442e6a9)i spremite u istu mapu koja sadrži DLL-a.

5. Iz te mape pokrenuti "PowerShell.\upgrade.ps1" iz naredbenog retka, a zatim slijedite upute.
 
***Mogućnost 2: Nadogradnja metapodataka pomoću C#***

Osim toga, stvaranje aplikacije Visual Studio otvorit će se vaš ShardMapManager, zadanu ponovno izračunava preko svih shards i izvodi Nadogradnja metapodataka tako da nazovete metode [UpgradeLocalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradelocalstore.aspx) i [UpgradeGlobalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradeglobalstore.aspx) kao u ovom primjeru: 

    ShardMapManager smm =
       ShardMapManagerFactory.GetSqlShardMapManager
       (connStr, ShardMapManagerLoadPolicy.Lazy); 
    smm.UpgradeGlobalStore(); 
    
    foreach (ShardLocation loc in
     smm.GetDistinctShardLocations()) 
    {   
       smm.UpgradeLocalStore(loc); 
    } 

Ove tehnike o nadogradnjama metapodataka mogu primijeniti više puta bez nanošenje. Ako, na primjer, ako starijih verzija klijenta slučajno stvara na shard kada već su ažurirani, možete pokrenuti nadogradnju ponovno preko svih shards da biste bili sigurni da je najnoviju verziju metapodataka izlaganje cijelom sustavu. 

**Bilješke:**  Nove verzije klijentska biblioteka objavljene na datum nastaviti rad s prethodne verzije Shard karte Upravitelj metapodataka na baze podataka SQL Azure i obratno.   No iskoristiti nove značajke u najnovije klijentu, metapodataka potrebno je nadograditi.   Imajte na umu da metapodataka nadogradnje ne utječu korisničkih podataka ni podatke specifične za aplikaciju, samo objekti stvara i koristi Shard nadređenu mapu.  I nastaviti raditi putem slijed nadogradnje prethodno opisan aplikacije. 

## <a name="elastic-database-client-version-history"></a>Povijest verzija klijenta elastic baze podataka 

Povijest verzija, potražite u odjeljku [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]  


<!--Image references-->
[1]:./media/sql-database-elastic-scale-upgrade-client-library/nuget-upgrade.png
 