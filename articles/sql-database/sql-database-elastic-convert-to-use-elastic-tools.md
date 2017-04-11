<properties
   pageTitle="Migracija postojeće baze podataka skaliranje istek | Microsoft Azure"
   description="Pretvaranje sharded baza podataka pomoću alata elastic baze podataka stvaranjem na shard Upravitelj karte"
   services="sql-database"
   documentationCenter=""
   authors="ddove"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management"
   ms.date="10/24/2016"
   ms.author="ddove"/>

# <a name="migrate-existing-databases-to-scale-out"></a>Migracija postojeće baze podataka za skaliranje Izlaz

Jednostavno upravljajte postojećih skalirana izlaz sharded baza podataka pomoću alata baze podataka za baze podataka SQL Azure (kao što je [Biblioteka klijentski Elastic baze podataka](sql-database-elastic-database-client-library.md)). Pretvorite postojeći skup baze podataka da biste koristili [shard mapiranje manager](sql-database-elastic-scale-shard-map-management.md). 

## <a name="overview"></a>Pregled
Migracija postojeće sharded baze podataka: 

1. Priprema [Upravitelj baze podataka za shard karta](sql-database-elastic-scale-shard-map-management.md).
2. Stvaranje mape shard.
3. Priprema pojedinačne shards.  
2. Dodavanje mapiranja shard kartu.

Ove tehnike može se implementirati pomoću [.NET Framework klijentska biblioteka](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)ili skripti komponente PowerShell navedenih u [DB SQL Azure - skripte Alati Elastic baze podataka](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db). U primjerima pomoću skripte komponente PowerShell.

Dodatne informacije o na ShardMapManager potražite u članku [Shard mapiranje upravljanje](sql-database-elastic-scale-shard-map-management.md). Pregled alata za elastic baze podataka, potražite u članku [Pregled značajki Elastic baze podataka](sql-database-elastic-scale-introduction.md).

## <a name="prepare-the-shard-map-manager-database"></a>Priprema shard baze podataka upravitelja karte

Upravitelj karte shard je posebno bazu podataka koja sadrži podatke za upravljanje bazama podataka skalirana izlaz. Možete koristiti postojeću bazu podataka ili stvorite novu bazu podataka. Imajte na umu ulozi shard karte Upravitelj baze podataka mora biti iste baze podataka kao u shard. Imajte na umu i skriptu PowerShell stvaranje baze podataka za vas. 

## <a name="step-1-create-a-shard-map-manager"></a>Korak 1: Stvaranje na shard manager karte

    # Create a shard map manager. 
    New-ShardMapManager -UserName '<user_name>' 
    -Password '<password>' 
    -SqlServerName '<server_name>' 
    -SqlDatabaseName '<smm_db_name>' 
    #<server_name> and <smm_db_name> are the server name and database name 
    # for the new or existing database that should be used for storing 
    # tenant-database mapping information.

### <a name="to-retrieve-the-shard-map-manager"></a>Dohvaćanje upravitelju shard karte

Nakon stvaranja, možete dohvatiti upravitelja karte shard s ovaj cmdlet. Svaki put kada morate koristiti ShardMapManager objekt nije potrebno ovaj korak.

    # Try to get a reference to the Shard Map Manager  
    $ShardMapManager = Get-ShardMapManager -UserName '<user_name>' 
    -Password '<password>' 
    -SqlServerName '<server_name>' 
    -SqlDatabaseName '<smm_db_name>' 

  
## <a name="step-2-create-the-shard-map"></a>Korak 2: Stvaranje karti shard

Morate odabrati vrstu shard karte da biste stvorili. Odabir ovisi o arhitektura baze podataka: 

1. Jednog klijenta po bazi podataka (uvjete, potražite u članku [Pojmovnik](sql-database-elastic-scale-glossary.md).) 
2. Više klijenata po bazi podataka (dvije vrste):
    3. Preslikavanje popisa
    4. Mapiranje raspona
 

Za model jednog klijenta, stvorite kartu shard **preslikavanje popisa** . Model jednostruko klijentu dodjeljuje jedna baza podataka po klijentu. Ovo je učinkovitih model za razvojne inženjere SaaS kao što je pojednostavnjuje upravljanje.

![Preslikavanje popisa][1]

Model više klijentu dodjeljuje nekoliko klijenata jednu bazu podataka (i koje možete raspodijeliti skupine klijenata preko više baza podataka). Koristite ovaj model kada očekujete da svaki klijent da bi potrebama small podataka. U ovaj model ne možemo dodijeliti raspon klijenata s bazom podataka koristeći **Mapiranje raspon**. 
 

![Mapiranje raspona][2]

Ili možete implementirati modela baze podataka više klijentu koristeći *mapiranje popisa* da biste dodijelili više klijenata za jednu bazu podataka. Ako, na primjer, DG1 služi za spremanje informacija o klijentu id 1 i 5, a DB2 sprema podatke za klijent 7 i klijentu 10. 

![Muliple klijenata na jednom DB][3] 

**Ovisno o vašem izboru, odaberite neku od ovih mogućnosti:**

### <a name="option-1-create-a-shard-map-for-a-list-mapping"></a>Mogućnost 1: Stvorite kartu shard za mapiranje popisa
Stvaranje karte shard pomoću ShardMapManager objekta. 

    # $ShardMapManager is the shard map manager object. 
    $ShardMap = New-ListShardMap -KeyType $([int]) 
    -ListShardMapName 'ListShardMap' 
    -ShardMapManager $ShardMapManager 
 
 
### <a name="option-2-create-a-shard-map-for-a-range-mapping"></a>Mogućnost 2: Stvorite kartu shard za mapiranje raspona

Imajte na umu da korištenje ove mapiranje uzorak, id klijenta vrijednosti mora biti neprekinuti raspona, a nije prihvatljiva da bi razmaka u rasponima, jednostavno preskakanje raspon prilikom stvaranja baze podataka.

    # $ShardMapManager is the shard map manager object 
    # 'RangeShardMap' is the unique identifier for the range shard map.  
    $ShardMap = New-RangeShardMap 
    -KeyType $([int]) 
    -RangeShardMapName 'RangeShardMap' 
    -ShardMapManager $ShardMapManager 

### <a name="option-3-list-mappings-on-a-single-database"></a>Mogućnost 3: Popis mapiranja u jednom baze podataka
Postavljanje ovaj uzorak zahtijeva stvaranje karti popisa kao što je prikazano u koraku 2, a zatim mogućnost 1.

## <a name="step-3-prepare-individual-shards"></a>Korak 3: Priprema pojedinačne shards

Dodati nadređenu mapu shard svaki shard (baza podataka). To Priprema pojedinačne baze podataka za pohranu informacija. Na svakom shard izvršiti ove metode.
     
    Add-Shard 
    -ShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>'
    # The $ShardMap is the shard map created in step 2.
 

## <a name="step-4-add-mappings"></a>Korak 4: Dodavanje mapiranja

Zbrajanje mapiranja ovisi o vrsti shard kartu koju ste stvorili. Ako ste stvorili popis mapa, dodajte mapiranja popisa. Ako ste stvorili kartu raspon, dodajte mapiranja raspon.

### <a name="option-1-map-the-data-for-a-list-mapping"></a>Mogućnost 1: mapirati podatke za mapiranje popisa

Mapirajte podatke dodavanjem preslikavanje popisa za svaki klijent.  

    # Create the mappings and associate it with the new shards 
    Add-ListMapping 
    -KeyType $([int]) 
    -ListPoint '<tenant_id>' 
    -ListShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>' 

### <a name="option-2-map-the-data-for-a-range-mapping"></a>Mogućnost 2: mapirati podatke za mapiranje raspona

Dodavanje mapiranja raspon za sve klijentu id raspon – pridruživanja baze podataka:

    # Create the mappings and associate it with the new shards 
    Add-RangeMapping 
    -KeyType $([int]) 
    -RangeHigh '5' 
    -RangeLow '1' 
    -RangeShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>' 


### <a name="step-4-option-3-map-the-data-for-multiple-tenants-on-a-single-database"></a>Mogućnost koraku 4 3: Karta podataka za više klijenata za jednu bazu podataka

Za svaki klijent pokrenuti Dodaj ListMapping (mogućnost 1, iznad). 


## <a name="checking-the-mappings"></a>Provjera mapiranja

Informacije o postojeće shards i mapiranja povezana s njima možete mu pomoću iza naredbe:  

    # List the shards and mappings 
    Get-Shards -ShardMap $ShardMap 
    Get-Mappings -ShardMap $ShardMap 

## <a name="summary"></a>Sažetak

Nakon što ste dovršili instalaciju, možete početi koristiti klijentska biblioteka Elastic baze podataka. Možete koristiti i [podataka o njima ovisne usmjeravanje](sql-database-elastic-scale-data-dependent-routing.md) i [više shard upita](sql-database-elastic-scale-multishard-querying.md).

## <a name="next-steps"></a>Daljnji koraci


Skripti komponente PowerShell zatražite od [sripts Alati baze podataka DB Elastic Azure SQL](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).

Alati i nalaze se na GitHub: [Azure/elastic-db – Alati](https://github.com/Azure/elastic-db-tools).

Pomoću alata za podjelu cirkularnog pisma za premještanje podataka iz modela više klijentu jednog klijenta model. U odjeljku [Alat za spajanje Podijeli](sql-database-elastic-scale-get-started.md).

## <a name="additional-resources"></a>Dodatni resursi

Informacije o uobičajenih podataka arhitektura uzoraka aplikacija za baze podataka za više klijentu softver-kao-na-service (SaaS) potražite u članku [Dizajn obrazaca za više klijentske aplikacije SaaS s bazom podataka SQL Azure](sql-database-design-patterns-multi-tenancy-saas-applications.md).

## <a name="questions-and-feature-requests"></a>Pitanja i zahtjeve za značajke

Za pitanja, provjerite stupili nam na [forumu SQL baze podataka](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) i zahtjeve za značajke, dodajte ih na [forum za povratne informacije SQL baze podataka](https://feedback.azure.com/forums/217321-sql-database/).

<!--Image references-->
[1]: ./media/sql-database-elastic-convert-to-use-elastic-tools/listmapping.png
[2]: ./media/sql-database-elastic-convert-to-use-elastic-tools/rangemapping.png
[3]: ./media/sql-database-elastic-convert-to-use-elastic-tools/multipleonsingledb.png
 
