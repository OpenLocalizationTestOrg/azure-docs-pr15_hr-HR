<properties 
    pageTitle="Azure SQL Elastic mjerilo najčešća pitanja vezana uz | Microsoft Azure" 
    description="Najčešća pitanja o Elastic promjena veličine baze podataka Azure SQL." 
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
    ms.date="05/03/2016" 
    ms.author="ddove"/>

# <a name="elastic-database-tools-faq"></a>Elastic Alati baze podataka najčešća Pitanja 

#### <a name="if-i-have-a-single-tenant-per-shard-and-no-sharding-key-how-do-i-populate-the-sharding-key-for-the-schema-info"></a>Ako imam na klijentu jedne po shard i ključ ne sharding, kako učinite I popunjavanje tipku sharding sheme informacije?

Objekt informacije sheme koristi se samo za podjelu scenariji spajanja. Ako aplikacija nije čini jednog klijenta, zahtijevaju alata za podjelu spajanje i stoga nema potrebe za popunjavanje objekt sheme informacije.

#### <a name="ive-provisioned-a-database-and-i-already-have-a-shard-map-manager-how-do-i-register-this-new-database-as-a-shard"></a>Jeste li dodijeljeni resursi baze podataka i već imate Upravitelj Shard karte, kako registrirati ovu novu bazu podataka kao u shard?

Pročitajte članak **[Dodavanje shard aplikaciju pomoću klijentska biblioteka elastic baze podataka](sql-database-elastic-scale-add-a-shard.md)**. 

#### <a name="how-much-do-elastic-database-tools-cost"></a>Koliko je učinite cijena Alati elastic baze podataka?

Korištenje klijentska biblioteka elastic baze podataka plaćati sve troškove. Troškovi ako skupi samo za baze podataka sustava Azure SQL koju koristite za shards i nadređenu mapu Shard, kao i web-tempiranja uloge Dodjela resursa za alat za spajanje Podijeli.

#### <a name="why-are-my-credentials-not-working-when-i-add-a-shard-from-a-different-server"></a>Zašto moje vjerodajnice ne funkcioniraju kada dodate na shard iz različitih server?
Pomoću vjerodajnica u obliku "korisnika ID=username@servername”, umjesto jednostavno koristite" korisnički ID = korisničko ime ".  Osim toga, obavezno prijavu "korisničko ime" ima dozvole na na shard.

#### <a name="do-i-need-to-create-a-shard-map-manager-and-populate-shards-every-time-i-start-my-applications"></a>Potrebne za stvaranje karte upravitelja Shard i popunjavanje shards prilikom svakog pokretanja svoje aplikacije?

Ne – stvaranje upravitelju Shard mapu (na primjer, **[ShardMapManagerFactory.CreateSqlShardMapManager](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.createsqlshardmapmanager.aspx)**) je jednokratni postupak.  Aplikacija trebali biste koristiti poziv **[ShardMapManagerFactory.TryGetSqlShardMapManager()](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx)** u trenutku pokretanja aplikacije.  Postoji treba samo jedan takve poziv po aplikacije domene.

#### <a name="i-have-questions-about-using-elastic-database-tools-how-do-i-get-them-answered"></a>Imam pitanja o korištenju Alati elastic baze podataka, kako dobiti odgovore? 

Provjerite stupili nam na [forumu baze podataka SQL Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted).

#### <a name="when-i-get-a-database-connection-using-a-sharding-key-i-can-still-query-data-for-other-sharding-keys-on-the-same-shard--is-this-by-design"></a>Ako se veza s bazom podataka putem ključa sharding, ne mogu i dalje podaci upita za ostale sharding tipke na istom shard.  Je li ovo namjerno?

Elastic API-ji skaliranje dati povezati točan bazom podataka za ključ sharding, ali omogućuje sharding ključa filtriranja.  Ako je potrebno, dodajte **gdje** rečenice u upit da biste ograničili opseg tipku navedeni sharding.

#### <a name="can-i-use-a-different-azure-database-edition-for-each-shard-in-my-shard-set"></a>Mogu li koristiti drugo izdanje Azure baze podataka za svaku shard u skupu Moje shard?

Da, u shard je pojedinačne baze podataka i stoga jedan shard može biti Premium edition dok drugi to Standard edition. Nadalje, edition na shard mogu mijenjati veličinu prema gore ili dolje više puta tijekom života u shard.

#### <a name="does-the-split-merge-tool-provision-or-delete-a-database-during-a-split-or-merge-operation"></a>Ne Dodjela resursa za alat za spajanje Podijeli (ili brisanje) bazu podataka tijekom operacije Podijeli i cirkularnih pisama? 

ne. **Podjela** operacije ciljna baza podataka mora postojati s odgovarajućim shemom i biti registriran u upravitelju Shard karta.  Za operacije **spajanja** , morate izbrisati u shard od upravitelja shard karte i brisanje baze podataka.

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
 