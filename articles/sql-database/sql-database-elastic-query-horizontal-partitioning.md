<properties
    pageTitle="Izvješćivanje o pogreškama u oblak skalirana iz baze podataka | Microsoft Azure"
    description="kako postaviti elastic upita putem vodoravni particije"    
    services="sql-database"
    documentationCenter=""  
    manager="jhubbard"
    authors="torsteng"/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/27/2016"
    ms.author="torsteng" />

# <a name="reporting-across-scaled-out-cloud-databases-preview"></a>Izvješćivanje o pogreškama u oblak skalirana iz baze podataka (pretpregled)

![Slanje upita za shards][1]

Baza podataka za sharded raspodijelite redaka skaliranu izgleda podataka sloju. Shema jednak sve koji sudjeluju baze podataka, poznata i kao vodoravni particija. Korištenje upita za elastic, možete stvoriti izvješća koja obuhvaćaju sve baze podataka u bazi podataka sharded.

Brzi početak rada potražite u članku [izvješćivanja putem oblaka skalirana iz baze podataka](sql-database-elastic-query-getting-started.md).

Baze podataka koje nisu sharded, potražite u članku [upita putem oblaka baze podataka s različitim sheme](sql-database-elastic-query-vertical-partitioning.md). 

 
## <a name="prerequisites"></a>Preduvjeti

* Stvaranje karte shard pomoću klijentska biblioteka elastic baze podataka. potražite u članku [Shard mapiranje upravljanje](sql-database-elastic-scale-shard-map-management.md). Ili pomoću aplikacije uzorak u [Početak rada s alatima elastic baze podataka](sql-database-elastic-scale-get-started.md).
* Osim toga, potražite u članku [Migracija postojeće baze podataka na skalirana iz baze podataka](sql-database-elastic-convert-to-use-elastic-tools.md).
* Korisnik mora imaju dozvole PROMIJENITI bilo koje VANJSKI IZVOR podataka. Tu dozvolu dolazi s dozvolom ALTER baze podataka.
* Da biste se pozvali u temeljnom izvoru podataka, potrebne su dozvole PROMIJENITI bilo koje VANJSKI IZVOR podataka.

## <a name="overview"></a>Pregled

Te naredbe stvaranje hijerarhije metapodataka na sloju sharded podataka u bazu podataka elastic upita. 


1. [STVORITE GLAVNI KLJUČ](https://msdn.microsoft.com/library/ms174382.aspx)
2. [STVARANJE BAZE PODATAKA IZ DJELOKRUGA VJERODAJNICA](https://msdn.microsoft.com/library/mt270260.aspx)
3. [STVARANJE VANJSKOG IZVORA PODATAKA](https://msdn.microsoft.com/library/dn935022.aspx)
4. [STVARANJE VANJSKE TABLICE](https://msdn.microsoft.com/library/dn935021.aspx) 

## <a name="11-create-database-scoped-master-key-and-credentials"></a>Glavni ključ i vjerodajnice iz djelokruga 1.1 stvaranje baze podataka 

Vjerodajnicu elastic upit koristi za povezivanje s udaljenom baze podataka.  

    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';
    CREATE DATABASE SCOPED CREDENTIAL <credential_name>  WITH IDENTITY = '<username>',  
    SECRET = '<password>'
    [;]
 
>[AZURE.NOTE] Provjerite je li u *"\<korisničko ime\>"* sadrži neki *"@servername"* sufiks. 

## <a name="12-create-external-data-sources"></a>1.2 Stvaranje vanjskih izvora podataka

Sintaksa:

    <External_Data_Source> ::=    
    CREATE EXTERNAL DATA SOURCE <data_source_name> WITH                                            
            (TYPE = SHARD_MAP_MANAGER,
                    LOCATION = '<fully_qualified_server_name>',
            DATABASE_NAME = ‘<shardmap_database_name>',
            CREDENTIAL = <credential_name>, 
            SHARD_MAP_NAME = ‘<shardmapname>’ 
                   ) [;] 

### <a name="example"></a>Primjer 

    CREATE EXTERNAL DATA SOURCE MyExtSrc 
    WITH 
    ( 
        TYPE=SHARD_MAP_MANAGER,
        LOCATION='myserver.database.windows.net', 
        DATABASE_NAME='ShardMapDatabase', 
        CREDENTIAL= SMMUser, 
        SHARD_MAP_NAME='ShardMap' 
    );
 
Dohvaćanje popisa trenutni vanjskim izvorima podataka: 

    select * from sys.external_data_sources; 

Vanjski izvor podataka odnosi se na karti shard. Elastic upita pa koristi vanjski izvor podataka i temeljni shard karte numerirati baze podataka koje sudjeluju u sloju podataka. Istih vjerodajnica koriste se za čitanje shard karte i pristup podacima na na shards tijekom obrade elastic upita. 

## <a name="13-create-external-tables"></a>1.3 stvaranje vanjskih tablica 
 
Sintaksa:  

    CREATE EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name  
        ( { <column_definition> } [ ,...n ])     
        { WITH ( <sharded_external_table_options> ) }
    ) [;]  
    
    <sharded_external_table_options> ::= 
      DATA_SOURCE = <External_Data_Source>,       
      [ SCHEMA_NAME = N'nonescaped_schema_name',] 
      [ OBJECT_NAME = N'nonescaped_object_name',] 
      DISTRIBUTION = SHARDED(<sharding_column_name>) | REPLICATED |ROUND_ROBIN

**Primjer**

    CREATE EXTERNAL TABLE [dbo].[order_line]( 
         [ol_o_id] int NOT NULL, 
         [ol_d_id] tinyint NOT NULL,
         [ol_w_id] int NOT NULL, 
         [ol_number] tinyint NOT NULL, 
         [ol_i_id] int NOT NULL, 
         [ol_delivery_d] datetime NOT NULL, 
         [ol_amount] smallmoney NOT NULL, 
         [ol_supply_w_id] int NOT NULL, 
         [ol_quantity] smallint NOT NULL, 
         [ol_dist_info] char(24) NOT NULL 
    ) 
    
    WITH 
    ( 
        DATA_SOURCE = MyExtSrc, 
        SCHEMA_NAME = 'orders', 
        OBJECT_NAME = 'order_details', 
        DISTRIBUTION=SHARDED(ol_w_id)
    ); 

Dohvatiti popis vanjskih tablica iz trenutne baze podataka: 

    SELECT * from sys.external_tables; 

Da biste ispustite vanjske tablice:

    DROP EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name[;]

### <a name="remarks"></a>Napomene

Podaci\_IZVORA uvjet definira vanjski izvor podataka (shard karta) koji se koristi za vanjske tablice.  

SHEMA\_ime i OBJEKT\_naziv rečenice mapiranje definicija vanjskog tablice u tablicu u drugu shemu. Ako se ispusti, sheme udaljene objekt pretpostavlja se da "vlasnika baze podataka" i njegov naziv pretpostavlja da se podudara s nazivom vanjska tablica se definira. To je korisno ako naziv udaljenog tablice već koristi se u bazu podataka koju želite stvoriti vanjski tablicu. Ako, na primjer, želite definirati vanjsku tablicu da biste dobili zbrajanja prikaz kataloga prikaza ili sloju DMVs na skaliranu izgleda podataka. Budući da kataloga prikaza i DMVs već postoji lokalno, nećete moći koristiti njihova imena definicija vanjska tablica. Umjesto toga koristite neki drugi naziv i koristiti prikaz katalog ili na DMV naziva u SHEMI\_ime i/ili OBJEKT\_naziv rečenice. (Pogledajte primjer u nastavku). 

Uvjet RASPODJELE određuje distribucija podataka koji se koristi za tu tablicu. Procesor upita koristi informacija navedenih u uvjetu RASPODJELE da biste sastavili najučinkovitiji tarife upita.  

1. **SHARDED** znači podataka vodoravno particije preko baze podataka. Stvaranje particija ključ za raspodjelu podataka je parametar **< sharding_column_name >** .
2. **REPLICATED** znači identične kopije tablici postoje na svaku bazu podataka. Je odgovornost da na replike provjerite jesu li jednake preko baze podataka.
3. **ROUND\_ROBIN** znači da u tablici ima vodoravno particije pomoću metode raspodjele ovisne o aplikaciji. 

**Razina podataka referenca**: vanjska tablica DDL se odnosi na vanjski izvor podataka. Vanjski izvor podataka određuje kartu shard koji omogućuje vanjske tablice s podacima koje su potrebne da biste pronašli sve baze podataka u sloju vaše podatke. 


### <a name="security-considerations"></a>Sigurnosna pitanja vezana uz 

Korisnici s pristupom vanjskih tablice automatski dobiti pristup tablice u podlozi udaljene u odjeljku vjerodajnica u definiciju izvora vanjskih podataka. Izbjegavajte neželjenih povećanje ovlasti putem vjerodajnica s vanjskim izvorom podataka. Koristite GRANT ili REVOKE za vanjsku tablicu kao da je obične tablice.  

Kada definirate vanjskog izvora podataka i vanjske tablice, sada možete koristiti cijeli T-SQL nad vanjskim tablice.

## <a name="example-querying-horizontal-partitioned-databases"></a>Primjer: slanje upita vodoravni particioniranom baze podataka 

Sljedeći upit izvodi tri način spoj između skladištima, narudžbe i recima naloga i koristi nekoliko zbrajanja i selektivno filtra. Pretpostavlja (1) vodoravni stvaranje particija (sharding) i (2) da skladištima, narudžbe i recima naloga sharded po stupcu id skladištu i elastic upita možete zajednički pronađite spojevi u na shards i obrada skupi dio upit na shards paralelno. 

    select  
         w_id as warehouse,
         o_c_id as customer,
         count(*) as cnt_orderline,
         max(ol_quantity) as max_quantity,
         avg(ol_amount) as avg_amount, 
         min(ol_delivery_d) as min_deliv_date
    from warehouse 
    join orders 
    on w_id = o_w_id
    join order_line 
    on o_id = ol_o_id and o_w_id = ol_w_id 
    where w_id > 100 and w_id < 200 
    group by w_id, o_c_id 
 
## <a name="stored-procedure-for-remote-t-sql-execution-spexecuteremote"></a>Pohranjena procedura za daljinsko izvršenje T SQL: sp\_execute_remote

Elastic upita predstavlja i pohranjena procedura koja omogućuje izravan pristup na shards. Pohranjena procedura zove [sp\_izvršavanje \_udaljene](https://msdn.microsoft.com/library/mt703714) i može se koristiti za izvršavanje udaljene pohranjene procedure ili T SQL koda na udaljena baza podataka. Traje sljedećih parametara: 

* Naziv izvora podataka (nvarchar): naziv vanjskog izvora podataka vrste RDBMS. 
* Upit (nvarchar): izvršavanje na svakom shard T-SQL upita. 
* Parametar deklariranje (nvarchar) – Neobavezno: niz definicije vrsta podataka za parametre u parametra upita (kao što je sp_executesql). 
* Parametar popisa vrijednosti - Neobavezno: odvojenih zarezom popis vrijednosti parametara (kao što je sp_executesql).

U sp\_izvršavanje\_alat za analizu daljinske koristi vanjski izvor podataka u parametara poziva izvršiti navedeni T SQL naredbe na udaljena baza podataka. Koristi vjerodajnica vanjski izvor podataka za povezivanje s shardmap Upravitelj baze podataka i udaljena baza podataka.  

Primjer: 

    EXEC sp_execute_remote
        N'MyExtSrc',
        N'select count(w_id) as foo from warehouse' 

## <a name="connectivity-for-tools"></a>Povezivanje za alate  

Povezivanje aplikacije, pomoću običnog nizove veze za SQL Server vaše BI i podataka Alati za integraciju s bazom podataka s definicija vanjska tablica. Provjerite je li SQL Server podržana kao izvor podataka za vaše alat. Zatim reference elastic upita baze podataka kao što je sve druge baze podataka SQL Server povezani alat i korištenje vanjskih tablice iz alata ili aplikacije kao da su lokalne tablice. 

## <a name="best-practices"></a>Najbolje prakse 

* Provjerite je li koji bazu podataka za krajnju točku elastic upita je dodijelio pristup shardmap baze podataka i sve shards kroz vatrozide SQL DB.  

* Provjerite valjanost ili Nametni distribucija podataka koji je definirao vanjske tablice. Ako vaš raspodjele stvarnih podataka razlikuje se od raspodjele naveden u definicije tablice, upiti mogu yield neočekivane rezultate. 

* Elastic upita trenutno ne potvrđuje shard uklanjanja kada predikati putem tipku sharding želite dopustiti da biste sigurno izuzeli određene shards iz obrada.

* Elastic upita najbolje funkcionira za upite gdje se najčešće se izračuni možete učiniti na na shards. Obično preuzimanje najbolje performanse upita s predikati selektivno filtar koji je moguće vrednovati na shards ili spojeva stvaranje particija prečaci koji se može izvoditi na način poravnat particije na sve shards. Drugi upit uzoraka možda morate učitati velike količine podataka iz na shards glavni čvor i možda slabije

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-query-horizontal-partitioning/horizontalpartitioning.png
<!--anchors-->
