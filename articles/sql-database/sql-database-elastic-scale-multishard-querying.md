<properties 
    pageTitle="Slanje upita više shard | Microsoft Azure" 
    description="Pokretanje upita preko shards pomoću klijentska biblioteka elastic baze podataka." 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="torsteng" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="04/12/2016" 
    ms.author="torsteng"/>

# <a name="multi-shard-querying"></a>Više shard upita

## <a name="overview"></a>Pregled

[Alati Elastic baze podataka](sql-database-elastic-scale-introduction.md), možete stvoriti rješenja sharded baze podataka. **Slanje upita više shard** koristi se za zadatke kao što su podaci zbirke/izvješćivanje o pogreškama koje je potrebno pokrenete upit koji Rasteže preko nekoliko shards. (Kontrast to [usmjeravanje ovisne podataka](sql-database-elastic-scale-data-dependent-routing.md), koji se izvodi sav rad na jednom shard.) 

## <a name="overview"></a>Pregled

1. Dobiti [**RangeShardMap**](https://msdn.microsoft.com/library/azure/dn807318.aspx) ili [**ListShardMap**](https://msdn.microsoft.com/library/azure/dn807370.aspx) [**TryGetRangeShardMap**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetrangeshardmap.aspx), [**TryGetListShardMap**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetlistshardmap.aspx)ili metodu [**GetShardMap**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getshardmap.aspx) . U odjeljku [**izgradnje na ShardMapManager**](sql-database-elastic-scale-shard-map-management.md#constructing-a-shardmapmanager) i [**dobiti RangeShardMap ili ListShardMap**](sql-database-elastic-scale-shard-map-management.md#get-a-rangeshardmap-or-listshardmap).
2. Stvaranje objekta **[MultiShardConnection](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardconnection.aspx)** .
2. Stvaranje **[MultiShardCommand](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.aspx)**. 
3. Postavite **[svojstva CommandText](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.commandtext.aspx#P:Microsoft.Azure.SqlDatabase.ElasticScale.Query.MultiShardCommand.CommandText)** T SQL naredbu.
3. Izvršavanje naredbe tako da nazovete **[ExecuteReader način](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.executereader.aspx)**.
4. Prikaz rezultata pomoću **[MultiShardDataReader predmete](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multisharddatareader.aspx)**. 

## <a name="example"></a>Primjer

Sljedeći kod ilustrira korištenje više shard upita pomoću zadanog **ShardMap** pod nazivom *myShardMap*. 

    using (MultiShardConnection conn = new MultiShardConnection( 
                                        myShardMap.GetShards(), 
                                        myShardConnectionString) 
          ) 
    { 
    using (MultiShardCommand cmd = conn.CreateCommand())
           { 
            cmd.CommandText = "SELECT c1, c2, c3 FROM ShardedTable"; 
            cmd.CommandType = CommandType.Text; 
            cmd.ExecutionOptions = MultiShardExecutionOptions.IncludeShardNameColumn; 
            cmd.ExecutionPolicy = MultiShardExecutionPolicy.PartialResults; 

            using (MultiShardDataReader sdr = cmd.ExecuteReader()) 
                { 
                    while (sdr.Read())
                        { 
                            var c1Field = sdr.GetString(0); 
                            var c2Field = sdr.GetFieldValue<int>(1); 
                            var c3Field = sdr.GetFieldValue<Int64>(2);
                        } 
                } 
           } 
    } 

 
Ključne razlike je izgradnju više shard veze. Gdje je **SqlConnection** pristajete na jednu bazu podataka, **MultiShardConnection** ***skup shards*** uzima kao njegov unos. Popunjavanje zbirke shards iz shard karte. Upit pa se izvršava u zbirci shards pomoću **UNION ALL** semantiku sastavljanje jedan ukupni rezultat. Po želji, naziv shard gdje u retku potječe možete dodati u izlaz pomoću svojstvo **ExecutionOptions** na naredbu. 

Imajte na umu poziv na **myShardMap.GetShards()**. Ovaj postupak dohvaća sve shards iz shard karte i omogućuje jednostavnu da biste pokrenuli upit preko sve relevantne baze podataka. Skup shards za upit s više shard dobili profinjeniji dodatno izvođenjem upita LINQ putem zbirke vratio poziv na **myShardMap.GetShards()**. U kombinaciji s pravilima djelomični rezultati trenutni mogućnost u ispitivanje više shard je dizajniran za pogodne za tens do stotine shards.

Ograničenje s više shard upita trenutno nedostatak provjere valjanosti za shards i shardlets koji su mu. Dok podataka ovisne usmjeravanja provjerava navedeni shard je dio shard karte u trenutku upita, upiti više shard izvođenje Ova provjera. To može dovesti do više shard upite koji se izvode na baze podataka koje je uklonjena iz mape shard.

## <a name="multi-shard-queries-and-split-merge-operations"></a>Više shard upite i operacije Podijeli cirkularnog pisma

Više shard upita provjerite hoće li se shardlets traženih bazu podataka sudjelovanje u svakodnevnog Podijeli cirkularnog pisma. (Pogledajte [Skaliranje pomoću alata za podjelu cirkularnog pisma Elastic baze podataka](sql-database-elastic-scale-overview-split-and-merge.md)). To može dovesti do nedosljednosti gdje retke iz iste shardlet prikaz za više baza podataka u istom više shard upit. Imajte na umu ta ograničenja i uzeti u obzir draining podjele cirkularnog pisma svakodnevnog i promjene na karti shard prilikom izvođenja više shard upita.

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

## <a name="see-also"></a>Vidi također
Klase **[System.Data.SqlClient](http://msdn.microsoft.com/library/System.Data.SqlClient.aspx)** i metode.


Upravljanje shards korištenju [Biblioteka klijentski Elastic baze podataka](sql-database-elastic-database-client-library.md). Sadrži prostor naziva pod nazivom [Microsoft.Azure.SqlDatabase.ElasticScale.Query](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.aspx) omogućuje više shards pomoću jedne upita i rezultata upita. Pruža upite apstrakcije putem zbirka shards. Također nudi zamjenski izvođenja pravila, posebice djelomični rezultati, baviti pogreške prilikom upita putem mnogo shards.  

 