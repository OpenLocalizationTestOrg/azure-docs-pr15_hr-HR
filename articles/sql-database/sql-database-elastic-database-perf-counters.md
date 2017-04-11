<properties
    pageTitle="Mjerača performansi za Upravitelj shard karte"
    description="ShardMapManager predmet i podatke o njima ovisne usmjeravanje mjerača performansi"
    services="sql-database"
    documentationCenter=""
    manager="jhubbard"
    authors="SilviaDoomra"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/23/2016"
    ms.author="SilviaDoomra"/>

# <a name="performance-counters-for-shard-map-manager"></a>Mjerača performansi za Upravitelj shard karte

Performanse [shard karte manager](sql-database-elastic-scale-shard-map-management.md)možete snimiti osobito kada pomoću [podataka o njima ovisne usmjeravanja](sql-database-elastic-scale-data-dependent-routing.md). Mjerača stvaraju se pomoću metode klase Microsoft.Azure.SqlDatabase.ElasticScale.Client.  

Mjerača koji se koriste za praćenje performansi [podataka o njima ovisne usmjeravanje](sql-database-elastic-scale-data-dependent-routing.md) operacije. Ove mjerača su dostupne na monitoru performanse kategoriji "Elastic upravljanje bazom podataka: Shard".

**Za najnoviju verziju:** Idite na [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). Vidi također [Nadogradnja aplikacije programa da biste koristili najnovije klijentska biblioteka elastic baze podataka](sql-database-elastic-scale-upgrade-client-library.md).

## <a name="prerequisites"></a>Preduvjeti

* Da biste stvorili kategoriju performanse i mjerača, korisnik mora biti član lokalne grupe **administratora** na računalu koje hostira aplikacije.  

* Da biste stvorili instancu za brojač izvedbe i ažuriranje na mjerača, korisnik mora biti član grupe **administratora** ili **Korisnika nadzora performansi** . 

## <a name="create-performance-category-and-counters"></a>Stvaranje kategorije performanse i mjerača 

Da biste stvorili na mjerača, nazovite metodu CreatePeformanceCategoryAndCounters [ShardMapManagmentFactory predmete](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.aspx). Samo administrator može izvoditi metoda: 

    ShardMapManagerFactory.CreatePerformanceCategoryAndCounters()  

Vam može poslužiti [ovu](https://gallery.technet.microsoft.com/scriptcenter/Elastic-DB-Tools-for-Azure-17e3d283) skriptu PowerShell za izvršavanje način. Način stvara mjerača performansi sljedeće:  

* **Mapiranja Cached**: broj mapiranja predmemorirani shard karte.
*  **Operacije sec DDR**: rata podataka o njima ovisne usmjeravanja operacije shard karte. Brojač ažurira se kada poziv [OpenConnectionForKey()](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx) rezultira uspjelo povezivanje shard odredište. 
*  **Mapiranje pristupa/sec predmemoriju pretraživanja**: rata operacije pretraživanja uspješno predmemoriju za mapiranja u njoj shard. 
*  **Mapiranje neuspješnih pronalaženja objekata u predmemoriji vrijednostima/s**: rata operacije pretraživanja nije uspjelo predmemoriju za mapiranja u njoj shard.
*  **Mapiranja dodati ili ažurirati u predmemoriji s**: brzina dodaju koje mapiranja ili ažurirani u predmemoriju za mapu shard. 
*  **Mapiranja uklonjena iz predmemoriji s**: Brzina kojom mapiranja koja se uklanjaju iz predmemorije za mapu shard. 

Za svaki predmemorirani shard map po proces se stvaraju mjerača performansi.  


## <a name="notes"></a>Bilješke
Sljedeće događaje aktiviraju stvaranja mjerača performansi:  

* Pokretanje [ShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx) s [krenuti učitavanja](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerloadpolicy.aspx), ako u ShardMapManager sadrži sve shard karte. To obuhvaća [GetSqlShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx?f=255&MSPPError=-2147217396#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardMapManagerFactory.GetSqlShardMapManager%28System.String,Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardMapManagerLoadPolicy%29) i [TryGetSqlShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx) metode.
* Uspješan pretraživanje shard karte (pomoću [GetShardMap()](https://msdn.microsoft.com/library/azure/dn824215.aspx), [GetListShardMap()](https://msdn.microsoft.com/library/azure/dn824212.aspx) ili [GetRangeShardMap()](https://msdn.microsoft.com/library/azure/dn824173.aspx)). 

* Stvaranje karte shard pomoću CreateShardMap() uspješno.

Mjerača performansi ažurirat će se tako da sve operacije predmemorije koji se izvode na kartu shard i mapiranja. Uspješan uklanjanje karte shard pomoću DeleteShardMap () reults brisanja instance mjerača performansi.  

## <a name="best-practices"></a>Najbolje prakse

* Stvaranje kategorije performanse i mjerača treba izvršiti samo jedanput prije stvaranja ShardMapManager objekt. Svaki Izvršavanje naredbe CreatePerformanceCategoryAndCounters() čisti prethodne mjerača (gubitka podataka prijavili sve instance) i stvara nove.  

* Performanse brojač instance stvaraju se po postupak. Bilo koji rušenje aplikacije ili uklanjanje shard karte iz predmemorije će do brisanja instanci mjerača performansi.  

### <a name="see-also"></a>Vidi također

[Pregled značajki za elastic baze podataka](sql-database-elastic-scale-introduction.md)  

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->

