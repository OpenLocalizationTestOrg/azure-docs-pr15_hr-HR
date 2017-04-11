<properties 
    pageTitle="Upravljanje vjerodajnicama u biblioteci klijent elastic baze podataka | Microsoft Azure" 
    description="Kako postaviti pravilnu razinu vjerodajnice, administrator da biste samo za čitanje, za aplikacije elastic baze podataka" 
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

# <a name="credentials-used-to-access-the-elastic-database-client-library"></a>Vjerodajnica korištenih za pristup biblioteci klijent Elastic baze podataka

[Biblioteka klijentski Elastic baze podataka](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/) koristi tri različite vrste vjerodajnice za pristup [Upravitelj shard karta](sql-database-elastic-scale-shard-map-management.md). Ovisno o tome da morate koristiti vjerodajnicu s najmanje razinu pristupa mogućih.

* **Upravljanje vjerodajnicama**: stvaranja ili rukovanje upravitelja shard karta. (Pogledajte [Pojmovnik](sql-database-elastic-scale-glossary.md)). 
* **Vjerodajnice za pristup**: da biste pristupili na postojeće Upravitelj karte shard da biste dobili informacije o shards.
* **Vjerodajnice za povezivanje**: povezivanje s shards. 

Pogledajte i [Upravljanje bazama podataka i prijave u bazi podataka SQL Azure](sql-database-manage-logins.md). 
 
## <a name="about-management-credentials"></a>O upravljanju vjerodajnice

Upravljanje vjerodajnicama se koriste za stvaranje [**ShardMapManager**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx) objekt za aplikacije koje upravljaju shard karte. (Na primjer, potražite u članku [Dodavanje shard pomoću alata za Elastic baze podataka](sql-database-elastic-scale-add-a-shard.md) i [podataka o njima ovisne usmjeravanje](sql-database-elastic-scale-data-dependent-routing.md)) Korisnik klijenta biblioteke elastic mjerilo stvara SQL korisnika i SQL prijave i provjerava je li svaki je dodijeljena dozvola za čitanje/pisanje na globalni shard karte baze podataka i kao i sve baze podataka shard. Da biste zadržali globalni shard karte i karte lokalni shard prilikom promjene na karti shard izvršavaju se koristi te vjerodajnice. Ako, primjerice, koristite vjerodajnice za upravljanje da biste stvorili objekt upravitelja za karte shard (putem [**GetSqlShardMapManager**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx): 

    // Obtain a shard map manager. 
    ShardMapManager shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager( 
            smmAdminConnectionString, 
            ShardMapManagerLoadPolicy.Lazy 
    ); 

Varijable **smmAdminConnectionString** je niz za povezivanje koja sadrži upravljanje vjerodajnicama. Korisnički ID i lozinku omogućuje čitanje/pisanje pristup bazi podataka shard karte i pojedinačne shards. Upravljanje niz za povezivanje sadrži i naziv poslužitelja i naziv baze podataka da biste odredili globalni shard karte bazu podataka. Ovo su uobičajeni niz za tu svrhu:

     "Server=<yourserver>.database.windows.net;Database=<yourdatabase>;User ID=<yourmgmtusername>;Password=<yourmgmtpassword>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;” 

Korištenje vrijednosti u obliku "username@server"—instead samo koristite vrijednost "korisničko ime".  To je zato vjerodajnice morate raditi od upravitelja baze podataka za shard karte i pojedinačne shards koje se mogu na različitim poslužiteljima.

## <a name="access-credentials"></a>Vjerodajnice za pristup
  
Kada stvarate na shard Upravitelj karte u aplikaciji koja administriranje shard karte, pomoću vjerodajnica koje imaju dozvole samo za čitanje na karti globalni shard. Podatke dohvatiti iz globalne shard karte u odjeljku te vjerodajnice se koriste za [usmjeravanje ovisne o podacima](sql-database-elastic-scale-data-dependent-routing.md) i za popunjavanje predmemorije shard mape na klijentskom računalu. Vjerodajnice su dobili putem isti uzorak poziva **GetSqlShardMapManager** kao što je prikazano gore: 

    // Obtain shard map manager. 
    ShardMapManager shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager( 
            smmReadOnlyConnectionString, 
            ShardMapManagerLoadPolicy.Lazy
    );  

Imajte na umu korištenje **smmReadOnlyConnectionString** u skladu s vizualnim korištenje različitih vjerodajnice za pristup ime korisnika **koji nisu administratori** : te vjerodajnice mora sadržavati dozvolu za zapisivanje na karti globalni shard. 

## <a name="connection-credentials"></a>Vjerodajnice za povezivanje 

Pri korištenju metode [**OpenConnectionForKey**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx) da biste pristupili shard povezan s ključem sharding potrebni su dodatni vjerodajnice. Te vjerodajnice potrebne dozvole za pristup tablice mapa lokalne shard residing na na shard samo za čitanje. To je potrebno da biste izvršili provjeru valjanosti veze za usmjeravanje ovisne o podacima na na shard. Ovaj isječak koda omogućuje pristup podacima u kontekstu podataka o njima ovisne usmjeravanja: 
 
    using (SqlConnection conn = rangeMap.OpenConnectionForKey<int>( 
    targetWarehouse, smmUserConnectionString, ConnectionOptions.Validate)) 

U ovom primjeru **smmUserConnectionString** sadrži niz za povezivanje za korisničke vjerodajnice. Za baze podataka SQL Azure, ovo su uobičajeni veza_niz za korisničke vjerodajnice: 

    "User ID=<yourusername>; Password=<youruserpassword>; Trusted_Connection=False; Encrypt=True; Connection Timeout=30;”  

Kao i kod administratorske vjerodajnice nisu vrijednosti u obliku "username@server". Umjesto toga koristiti "korisničko ime".  Imajte na umu da niz za povezivanje sadrži naziv poslužitelja i naziv baze podataka. To je zato poziv **OpenConnectionForKey** automatski Izravni veza ispravne shard koji se temelji na tipku. Dakle, naziv baze podataka i naziva poslužitelja se ne daju. 

## <a name="see-also"></a>Vidi također
[Upravljanje bazama podataka i prijave u bazi podataka SQL Azure](sql-database-manage-logins.md)

[Zaštita baze podataka sustava SQL](sql-database-security.md)

[Početak rada sa zadacima Elastic baze podataka](sql-database-elastic-jobs-getting-started.md)

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
 