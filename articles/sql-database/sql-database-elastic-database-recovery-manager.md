<properties 
    pageTitle="Rješavanje problema s kartu shard pomoću upravitelja oporavak | Microsoft Azure" 
    description="Rješavanje problema s kartama shard pomoću klase RecoveryManager" 
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
    ms.date="05/05/2016" 
    ms.author="ddove"/>

# <a name="using-the-recoverymanager-class-to-fix-shard-map-problems"></a>Korištenje klase RecoveryManager za rješavanje problema s shard kartu

Klase [RecoveryManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.recoverymanager.aspx) omogućuje aplikacije servisa za ADO.Net jednostavno otkrivanje i ispravljanje postoje nedosljednosti između karti globalni shard (GSM) i na karti lokalni shard (LSM) na okruženja za sharded baze podataka. 

GSM i LSM pratiti mapiranje svaku bazu podataka u okruženju sharded. Ponekad prijeloma pojavljuje se između na GSM i na LSM. U tom slučaju pomoću klase RecoveryManager otkrivanje i popravak prijelom.

Klase RecoveryManager je dio [biblioteke klijent Elastic baze podataka](sql-database-elastic-database-client-library.md). 


![Shard karte][1]


Definicija termina potražite u članku [Pojmovnik Alati Elastic baze podataka](sql-database-elastic-scale-glossary.md). Da biste naučili kako je **ShardMapManager** služi za upravljanje podacima u sharded rješenja, potražite u članku [Shard mapiranje upravljanje](sql-database-elastic-scale-shard-map-management.md).


## <a name="why-use-the-recovery-manager"></a>Zašto koristiti upravitelj za oporavak?

U okruženju sharded bazi podataka postoji jednog klijenta po bazi podataka i mnogih baza podataka po poslužitelju. Također može mnogo poslužitelji u okruženju. Svaku bazu podataka mapirani na karti shard tako da pozive moguće usmjeriti na odgovarajući poslužitelj i bazu podataka. Baza podataka prate prema **sharding ključ**, a svaki shard vam je dodijeljen **raspon vrijednosti ključa**. Na primjer, ključ sharding možda predstavljaju nazive kupaca s "D" "F" Mapiranje sve shards (ili baza podataka) i njihovih mapiranje raspona nalaze se u **globalni shard karte (GSM)**. Svaku bazu podataka i sadrži kartu raspona koje se nalaze na na shard – to je poznato kao **lokalne shard mapiranje (LSM)**. Kada aplikacije povezuje s shard, mapiranje predmemoriranih će se s aplikacijom za brzo dohvaćanje. Na LSM koristi se za provjeru valjanosti predmemoriranih podataka. 

GSM i LSM mogu postati usklađen iz sljedećih razloga:

1. Brisanje shard čije raspon je believed da više ne bude se koristi ili preimenovanje na shard. Brisanje shard rezultira **napuštenih shard mapiranje**. Similary, preimenovanju bazu podataka može dovesti do napuštena shard mapiranje. Ovisno o namjerom promjene na shard možda ćete morati ukloniti ili mjesto shard je potrebno ažurirati. Da biste vratili izbrisanu bazu podataka, pročitajte članak [Vraćanje baze podataka na prethodnu točku u vremenu, vraćanje izbrisanih baze podataka ili oporavak nedostupnosti za centar za podatke](sql-database-troubleshoot-backup-and-restore.md).
2. Pojavljuje se u događaj zemlj prebacivanje. Da biste nastavili, jedan morate ažurirati naziv poslužitelja i naziv baze podataka shard upravitelja karte u aplikaciji i ažuriranje detalja preslikavanja shard za sve shards na karti shard. U slučaju na zemlj. – prebacivanje takve logike oporavak treba automatski unutar tijeka rada za prebacivanje. Automatske akcije oporavak omogućuje frictionless mogućnost upravljanja za zemlj omogućeno baze podataka i izbjegava ručno Ljudski akcije.
3. Na shard ili baze podataka ShardMapManager vratit će se na ranije točke u vremensku.

Dodatne informacije o alatima za Azure SQL baza podataka Elastic baze podataka zemlj replikacije i vraćanje, pogledajte sljedeće: 

* [Pregled: U Oblaku tvrtke continuity i baza podataka Izrada oporavak SQL baze podataka](sql-database-business-continuity.md) 
* [Početak rada s alatima elastic baze podataka](sql-database-elastic-scale-get-started.md)  
* [Upravljanje ShardMap](sql-database-elastic-scale-shard-map-management.md)

## <a name="retrieving-recoverymanager-from-a-shardmapmanager"></a>Dohvaćanje RecoveryManager s na ShardMapManager 

U prvi je korak da biste stvorili RecoveryManager instance. [Način GetRecoveryManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getrecoverymanager.aspx) vraća Upravitelj za oporavak za trenutne instance [ShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx) . Da bi se riješio postoje nedosljednosti na karti shard, najprije morate dohvatiti RecoveryManager za određeni shard kartu. 

    ShardMapManager smm = ShardMapManagerFactory.GetSqlShardMapManager(smmConnnectionString,  
             ShardMapManagerLoadPolicy.Lazy);
             RecoveryManager rm = smm.GetRecoveryManager(); 

U ovom primjeru u RecoveryManager pokrenut će se iz u ShardMapManager. ShardMapManager koji sadrži na ShardMap i već pokrenut. 

Budući da kod aplikacije upravlja karti shard sam, vjerodajnica korištenih u način tvorničke (u primjeru iznad, smmConnectionString) mora biti vjerodajnica koje imaju dozvole za čitanje i pisanje na bazu podataka GSM referencira niz za povezivanje. Te vjerodajnice se obično razlikuju od vjerodajnica koje služe za otvaranje veze za usmjeravanje ovisne podataka. Dodatne informacije potražite u članku [Korištenje vjerodajnice u klijentu elastic baze podataka](sql-database-elastic-scale-manage-credentials.md).

## <a name="removing-a-shard-from-the-shardmap-after-a-shard-is-deleted"></a>Uklanjanjem na shard na ShardMap nakon brisanja na shard

[Način DetachShard](https://msdn.microsoft.com/library/azure/dn842083.aspx) odvaja danu shard iz mape shard i briše mapiranja pridružene na shard.  

* Mjesto parametar nije mjesto shard izričito naziv poslužitelja i naziv baze podataka, shard se odvojene. 
* Parametar shardMapName je naziv mape shard. To je samo potrebne za više mapa shard upravlja istog upravitelja karte shard. Neobavezno. 

**Važno**: prije korištenja ove tehnike samo ako ste sigurni da raspon za mapiranje ažurirana je prazan. Navedenog načina provjeru podataka u rasponu Premjesti, pa se preporučuje se da biste dodali provjere u kodu.

U ovom se primjeru uklanja shards iz mape shard. 

    rm.DetachShard(s.Location, customerMap); 

Karta mjesto shard u GSM prije brisanja na shard. Jer je izbrisana na shard, pretpostavlja se da je to je namjerno i raspon ključa sharding se više ne koristi. Ako to nije tako, mogu izvršiti vremena točke vraćanja. Da biste vratili na shard iz programa ranije točke-u – vrijeme. (U tom slučaju, pogledajte odjeljak u nastavku da biste otkrili nedosljednosti shard.) Da biste oporavili, pročitajte članak [Vraćanje baze podataka na prethodnu točku u vremenu, vraćanje izbrisanih baze podataka ili oporavak nedostupnosti za centar za podatke](sql-database-troubleshoot-backup-and-restore.md).

Budući da je pretpostavlja se da je baza podataka namjerno, akciju konačni administratora čišćenje je izbrisati stavku na shard u upravitelju shard kartu. To sprječava aplikaciju da slučajno zapisivanja informacija na raspon koji se očekuje.

## <a name="to-detect-mapping-differences"></a>Da biste otkrili razlike mapiranja 

[Način DetectMappingDifferences](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.recoverymanager.detectmappingdifferences.aspx) odabire i vraća jedan od karte shard (lokalni ili globalne) kao izvor istine i usklađuje mapiranja u obje mape shard (GSM i LSM).

    rm.DetectMappingDifferences(location, shardMapName);

* *Mjesto* određuje naziv poslužitelja i naziv baze podataka. 
* Parametar *shardMapName* je naziv mape shard. To je samo potreban ako više mapa shard upravlja istog upravitelja karte shard. Neobavezno. 

## <a name="to-resolve-mapping-differences"></a>Da biste riješili razlike mapiranja

[Način ResolveMappingDifferences](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.recoverymanager.resolvemappingdifferences.aspx) odabir jedne mape shard (lokalni ili globalne) kao izvor istine i usklađuje mapiranja u obje mape shard (GSM i LSM).

    ResolveMappingDifferences (RecoveryToken, MappingDifferenceResolution.KeepShardMapping);
   
* Parametar *RecoveryToken* zbraja razlike u odjeljku mapiranja između na GSM i LSM za određene shard. 

* [Numeriranje MappingDifferenceResolution](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.mappingdifferenceresolution.aspx) koristi se za označavanje način za rješavanje razlika između mapiranja shard. 
* U slučaju da na LSM sadrži točne mapiranje i stoga se mapiranje u na shard trebali biste koristiti, preporučuje se **MappingDifferenceResolution.KeepShardMapping** . To je obično slučaj slučaju na prebacivanje: u shard sada se nalazi na novi poslužitelj. Budući da u shard moraju biti uklonjeni iz GSM (pomoću metode RecoveryManager.DetachShard), preslikavanje se više ne postoji na na GSM. Zbog toga se ponovno uspostaviti mapiranje shard moraju se koristiti u LSM.

## <a name="attach-a-shard-to-the-shardmap-after-a-shard-is-restored"></a>Priložiti u shard na ShardMap nakon što vratit će se na shard 

[Način AttachShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.recoverymanager.attachshard.aspx) pridružuje navedeni shard shard karte. Zatim otkrije postoje nedosljednosti shard karte i prilagođava mapiranja tako da odgovara shard mjestu vraćanje shard. Pretpostavlja se da su baze podataka i preimenovati u skladu s vizualnim izvorni naziv baze podataka (prije no što kada u shard je vraćena), nakon zareza u vrijeme Vraćanje zadanih vrijednosti za novu bazu podataka dodaju se vremenska oznaka. 

    rm.AttachShard(location, shardMapName) 

* Parametar *mjesto* je naziv poslužitelja i naziv baze podataka, shard se priložene. 

* Parametar *shardMapName* je naziv mape shard. To je samo potrebne za više mapa shard upravlja istog upravitelja karte shard. Neobavezno. 

U ovom se primjeru dodaje u shard shard kartu koju je nedavno vraćena iz programa ranije točke u vrijeme. Budući da se shard (odnosno mapiranje za shard u na LSM) vraćen je potencijalno koje nisu usklađene s shard unos u GSM. Izvan kod u ovom primjeru u shard je vratiti i preimenovati izvorni naziv baze podataka. Budući da je vraćen, pretpostavlja se da je mapiranja u na LSM je pouzdanih mapiranje. 

    rm.AttachShard(s.Location, customerMap); 
    var gs = rm.DetectMappingDifferences(s.Location); 
      foreach (RecoveryToken g in gs) 
       { 
       rm.ResolveMappingDifferences(g, MappingDifferenceResolution.KeepShardMapping); 
       } 

## <a name="updating-shard-locations-after-a-geo-failover-restore-of-the-shards"></a>Ažuriranje shard mjesta nakon na zemlj. – prebacivanje (Vraćanje) na shards

U slučaju na zemlj. – prebacivanje sekundarne baza podataka postane pisanja pristupačne i postaje novu bazu podataka za primarni. Naziv poslužitelja i potencijalno baze podataka (ovisno o vašoj konfiguraciji), mogu se razlikovati od izvorne primarni. Stoga stavke mapiranje shard u GSM i LSM mora biti popravljeno. Isto tako, ako bazu podataka je vraćen na drugi naziv ili mjesto ili ranije točke u vremenu, to nepravilnim nedosljednosti shard karte. Upravitelj karte Shard rukuje raspodjele otvaranje veze točan bazu podataka. Raspodjele temelji se na podacima u mapu shard i vrijednost ključa sharding koji je cilj zahtjeva za aplikacije. Nakon što je zemlj. – prebacivanje taj podatak mora se ažurirati s nazivom točne poslužitelja, naziv baze podataka i mapiranje shard oporavljene baze podataka. 

## <a name="best-practices"></a>Najbolje prakse

Zemlj prebacivanje i oporavak su operacije obično upravlja administrator oblak aplikacije namjerno korištenja baze podataka SQL Azure tvrtke continuity značajke. Planiranje continuity tvrtke zahtijeva procesa, postupke i mjere da biste bili sigurni tvrtke operacije nastavka bez prekida. Dostupni načini kao dio klase RecoveryManager trebali biste koristiti i unutar ovaj tijek rada da bi GSM i LSM čuvaju ažurne na temelju za oporavak poduzimati. Postoje 5 Osnovni koraci za pravilno osiguravanje GSM i LSM odražava točne podatke nakon prebacivanje događaj. Kod aplikacije izvršiti ove korake može se integrirati u postojeće Alati i tijek rada. 

1. Dohvaćanje na RecoveryManager iz na ShardMapManager. 
2. Odvajanje stare shard iz mape shard.
3. Novi shard priložiti karti shard, uključujući na novo mjesto shard.
4. Pronalaženje nedosljednosti u mapiranja između GSM i LSM. 
5. Razrješavanje razlike između na GSM i LSM, povjerenje u LSM. 

U ovom se primjeru izvršava sljedeće korake:
1. Uklanja shards iz Shard kartu koja odražava shard mjesta prije događaja prebacivanje.
2. Pridružuje shards kartu Shard o nova mjesta shard (parametar "Configuration.SecondaryServer" je novi naziv poslužitelja ali isti naziv baze podataka).
3. Dohvaća tokeni oporavak otkrivanjem mapiranje razlike u GSM LSM za svaku shard. 
4. Prihvaćanje preslikavanje iz LSM svaki shard razrješava nedosljednosti. 

    VAR shards = smm. GetShards();  foreach (shard s u shards) {ako (s.Location.Server == Configuration.PrimaryServer) {ShardLocation slNew = novi ShardLocation (Configuration.SecondaryServer, s.Location.Database) 
        
          rm.DetachShard(s.Location); 
        
          rm.AttachShard(slNew); 
        
          var gs = rm.DetectMappingDifferences(slNew); 
    
          foreach (RecoveryToken g in gs) 
            { 
               rm.ResolveMappingDifferences(g, MappingDifferenceResolution.KeepShardMapping); 
            } 
        } 
    } 



[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]


<!--Image references-->
[1]: ./media/sql-database-elastic-database-recovery-manager/recovery-manager.png
 
