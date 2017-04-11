<properties 
    pageTitle="Dodavanje shard pomoću alata baze podataka za elastic | Microsoft Azure" 
    description="Kako koristiti API-ji Elastic mjerilo da biste dodali novi shards na shard postavite." 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="ddove" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove"/>

# <a name="adding-a-shard-using-elastic-database-tools"></a>Dodavanje shard pomoću alata za Elastic baze podataka

## <a name="to-add-a-shard-for-a-new-range-or-key"></a>Da biste dodali shard za novi raspon ili ključ  

Aplikacija često je potrebno da biste jednostavno dodajte nove shards za upravljanje podacima koji se očekuje nove ključeve ili ključa rasponima, shard kartu koja već postoji. Na primjer, sharded aplikacije po klijentu ID možda ćete morati Dodjela nove shard za novi klijent ili sharded mjesečno podataka možda će biti potrebno novi shard dodjeli prije početka svaki novi mjesec. 

Ako se novi raspon vrijednosti ključa još nije dio postojeće preslikavanje, je vrlo jednostavne da biste dodali novi shard i pridružiti novi ključ ili raspona na tom shard. 

### <a name="example--adding-a-shard-and-its-range-to-an-existing-shard-map"></a>Primjer: dodavanje na shard i njegovu rasponu postojeću mapu shard
Ovaj primjer koristi u [TryGetShard](https://msdn.microsoft.com/library/azure/dn823929.aspx) [CreateShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.createshard.aspx)metode [CreateRangeMapping](https://msdn.microsoft.com/library/azure/dn807221.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeShardMap`1.CreateRangeMapping(Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeMappingCreationInfo{`0})) i stvara instancu klase [ShardLocation](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardlocation.shardlocation.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardLocation.) . U nastavku uzorku bazu pod nazivom **sample_shard_2** i sve potrebne sheme objekte unutar je stvorena na čuvanje raspon [300, 400).  

    // sm is a RangeShardMap object.
    // Add a new shard to hold the range being added. 
    Shard shard2 = null; 

    if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
    { 
        shard2 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_2"));  
    } 

    // Create the mapping and associate it with the new shard 
    sm.CreateRangeMapping(new RangeMappingCreationInfo<long> 
                            (new Range<long>(300, 400), shard2, MappingStatus.Online)); 


Umjesto toga, možete koristiti Powershell da biste stvorili novi Upravitelj karte Shard. Primjer dostupna je [u nastavku](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).
## <a name="to-add-a-shard-for-an-empty-part-of-an-existing-range"></a>Da biste dodali shard za prazan dio postojeći raspon  

U nekim okolnostima možda ste već mapirani raspon u shard i djelomično ispunjenom s podacima, ali sada želite nadolazeće podaci preusmjereni na različitim shard. Ako, na primjer, koje shard prema rasponu dan i mora već dodijeljene 50 dana u shard, ali na dan 24 želite buduće podaci krenuti u različitim shard. Elastic baze podataka [alata za podjelu cirkularnog pisma](sql-database-elastic-scale-overview-split-and-merge.md) možete izvršiti ovaj postupak, ali ako premještanje podataka nije potrebno (na primjer, podaci za raspon days [25, 50), odnosno, dan 25 uključivo do 50 isključujući te dvije vrijednosti, još ne postoji) možete izvoditi to, pomoću upravljanja API-ji karte Shard izravno.

### <a name="example-splitting-a-range-and-assigning-the-empty-portion-to-a-newly-added-shard"></a>Primjer: Podjela raspon i korisnicima dodijelite prazan dio shard za dodani

Stvorena je bazu pod nazivom "sample_shard_2" i sve potrebne sheme objekte unutar njega.  

 
    // sm is a RangeShardMap object.
    // Add a new shard to hold the range we will move 
    Shard shard2 = null; 

    if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
    { 
    
        shard2 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_2"));  
    } 

    // Split the Range holding Key 25 

    sm.SplitMapping(sm.GetMappingForKey(25), 25); 

    // Map new range holding [25-50) to different shard: 
    // first take existing mapping offline 
    sm.MarkMappingOffline(sm.GetMappingForKey(25)); 
    // now map while offline to a different shard and take online 
    RangeMappingUpdate upd = new RangeMappingUpdate(); 
    upd.Shard = shard2; 
    sm.MarkMappingOnline(sm.UpdateMapping(sm.GetMappingForKey(25), upd)); 

**Važno**: prije korištenja ove tehnike samo ako ste sigurni da raspon za mapiranje ažurirana je prazan.  Navedenog načina provjeru podataka u rasponu Premjesti, pa se preporučuje se da biste dodali provjere u kodu.  Ako postoji redaka u rasponu premješten, raspodjele stvarnih podataka ne odgovaraju ažurirane shard karte. Pomoću [alata za podjelu cirkularnog pisma](sql-database-elastic-scale-overview-split-and-merge.md) za izvođenje operacije umjesto u tim slučajevima.  


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
 
