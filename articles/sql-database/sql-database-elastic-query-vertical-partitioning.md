<properties
    pageTitle="Upit putem oblaka baze podataka s različitim shemom | Microsoft Azure"
    description="upute za postavljanje upita unakrsno-baze podataka putem okomiti particije"    
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

# <a name="query-across-cloud-databases-with-different-schemas-preview"></a>Slanje upita za baze podataka oblaka s različitim shema (pretpregled)

![Slanje upita za tablice u različite baze podataka][1]

Okomito particije baze podataka pomoću različitih skupova tablice na različite baze podataka. To znači da shemi razlikuje na različite baze podataka. Na primjer, sve tablice za zalihe su na jednoj bazi podataka dok su sve računovodstveni povezane tablice na drugu bazu podataka. 

## <a name="prerequisites"></a>Preduvjeti

* Korisnik mora imaju dozvole PROMIJENITI bilo koje VANJSKI IZVOR podataka. Tu dozvolu dolazi s dozvolom ALTER baze podataka.
* Da biste se pozvali u temeljnom izvoru podataka, potrebne su dozvole PROMIJENITI bilo koje VANJSKI IZVOR podataka.

## <a name="overview"></a>Pregled

**Napomena**: za razliku od s vodoravne particija te DDL naredbe ne ovise o definiranje podataka sloj s kartom shard putem klijenta biblioteke elastic baze podataka.

1. [STVORITE GLAVNI KLJUČ](https://msdn.microsoft.com/library/ms174382.aspx)
2. [STVARANJE BAZE PODATAKA IZ DJELOKRUGA VJERODAJNICA](https://msdn.microsoft.com/library/mt270260.aspx)
3. [STVARANJE VANJSKOG IZVORA PODATAKA](https://msdn.microsoft.com/library/dn935022.aspx)
4. [STVARANJE VANJSKA TABLICA](https://msdn.microsoft.com/library/dn935021.aspx) 


## <a name="create-database-scoped-master-key-and-credentials"></a>Stvaranje baze podataka iz djelokruga glavni ključ i vjerodajnice 

Vjerodajnicu elastic upit koristi za povezivanje s udaljenom baze podataka.  

    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';
    CREATE DATABASE SCOPED CREDENTIAL <credential_name>  WITH IDENTITY = '<username>',  
    SECRET = '<password>'
    [;]
 
**Napomena**    Pobrinite se da u *<username>* sadrži neki *"@servername"* sufiks. 

## <a name="create-external-data-sources"></a>Stvaranje vanjske izvore podataka

Sintaksa:

    <External_Data_Source> ::=
    CREATE EXTERNAL DATA SOURCE <data_source_name> WITH 
               (TYPE = RDBMS,
                LOCATION = ’<fully_qualified_server_name>’,
                DATABASE_NAME = ‘<remote_database_name>’,  
                CREDENTIAL = <credential_name> 
                ) [;] 

**Važno**   Parametar TYPE mora biti postavljeno na **RDBMS**. 

### <a name="example"></a>Primjer 

Sljedeći primjer prikazuje korištenje iskaz CREATE za vanjske izvore podataka. 

    CREATE EXTERNAL DATA SOURCE RemoteReferenceData 
    WITH 
    ( 
        TYPE=RDBMS, 
        LOCATION='myserver.database.windows.net', 
        DATABASE_NAME='ReferenceData', 
        CREDENTIAL= SqlUser 
    ); 
 
Da biste dohvatili popis trenutni vanjskim izvorima podataka: 

    select * from sys.external_data_sources; 

### <a name="external-tables"></a>Vanjski tablice 

Sintaksa:

    CREATE EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name . ] table_name  
    ( { <column_definition> } [ ,...n ])     
    { WITH ( <rdbms_external_table_options> ) } 
    )[;] 
    
    <rdbms_external_table_options> ::= 
      DATA_SOURCE = <External_Data_Source>, 
      [ SCHEMA_NAME = N'nonescaped_schema_name',] 
      [ OBJECT_NAME = N'nonescaped_object_name',] 

### <a name="example"></a>Primjer  

    CREATE EXTERNAL TABLE [dbo].[customer]( 
        [c_id] int NOT NULL, 
        [c_firstname] nvarchar(256) NULL, 
        [c_lastname] nvarchar(256) NOT NULL, 
        [street] nvarchar(256) NOT NULL, 
        [city] nvarchar(256) NOT NULL, 
        [state] nvarchar(20) NULL, 
        [country] nvarchar(50) NOT NULL, 
    ) 
    WITH 
    ( 
           DATA_SOURCE = RemoteReferenceData 
    ); 

Sljedeći primjer pokazuje kako dohvatiti popis vanjskih tablice iz trenutne baze podataka: 

    select * from sys.external_tables; 

### <a name="remarks"></a>Napomene

Elastic upita proširuje sintaksa postojeće vanjska tablica da biste definirali vanjske tablice koje se koriste vanjskim izvorima podataka te vrste RDBMS. Definiciju vanjska tablica za okomite particija pokriva sljedeće aspekata: 

* **Sheme**: vanjska tablica DDL definira shemu upite možete koristiti. Shema navedenih u definicije vanjska tablica mora se podudarati sheme tablica u kojem je pohranjena stvarnih podataka udaljena baza podataka. 

* **Referenca udaljena baza podataka**: vanjska tablica DDL se odnosi na vanjski izvor podataka. Vanjski izvor podataka određuje logički poslužitelja i naziv baze podataka udaljena baza podataka, gdje je pohranjena na stvarne podatke tablice. 

Korištenje vanjskog izvora podataka kao što je vidljivo u prethodnom odjeljku, vidjet ćete da sintaksa za stvaranje vanjskog tablica je kako slijedi: 

Uvjet DATA_SOURCE definira vanjski izvor podataka (odnosno udaljena baza podataka u slučaju okomiti particija) koji se koristi za vanjske tablice.  

Rečenice SCHEMA_NAME i OBJECT_NAME pružaju mogućnost da biste mapirali definicija vanjskog tablice u tablicu u drugu shemu na udaljenom bazom podataka ili u tablicu pod drugim nazivom, odnosno. To je korisno ako želite da biste definirali vanjsku tablicu u prikazu katalog ili DMV udaljena baza podataka – ili druge situacija gdje naziv udaljenog tablice već koristi se lokalno.  

Sljedeća naredba DDL izostavlja postojeću definiciju vanjska tablica iz lokalne kataloga. Ne utječe na udaljenom bazom podataka. 

    DROP EXTERNAL TABLE [ [ schema_name ] . | schema_name. ] table_name[;]  

**Dozvole za stvaranje/PADAJUĆE VANJSKA TABLICA**: dozvole PROMIJENITI bilo koje VANJSKI IZVOR podataka su vam potrebne za vanjske tablice DDL koje je potrebno i da biste se pozvali u temeljnom izvoru podataka.  

## <a name="security-considerations"></a>Sigurnosna pitanja vezana uz
Korisnici s pristupom vanjske tablice automatski dobiti pristup tablice u podlozi udaljene u odjeljku vjerodajnica danih u definiciju izvora vanjskih podataka. Trebali biste pažljivo Upravljanje pristupom vanjskih tablicu da biste izbjegli neželjenih povećanje ovlasti putem vjerodajnica s vanjskim izvorom podataka. Uobičajeni SQL dozvole se može koristiti da biste DODIJELILI ili POVUKLI pristup vanjskim tablice kao da je obične tablice.  


## <a name="example-querying-vertically-partitioned-databases"></a>Primjer: slanje upita okomito particije baze podataka 

Sljedeći upit izvodi tri način spoj između dvije tablice lokalne za narudžbe i recima naloga i udaljenoj tablici Klijenti. Ovo je primjer kutije referenca podataka koristi za elastic upit: 

    SELECT      
     c_id as customer,
     c_lastname as customer_name,
     count(*) as cnt_orderline, 
     max(ol_quantity) as max_quantity,
     avg(ol_amount) as avg_amount,
     min(ol_delivery_d) as min_deliv_date
    FROM customer 
    JOIN orders 
    ON c_id = o_c_id
    JOIN  order_line 
    ON o_id = ol_o_id and o_c_id = ol_c_id
    WHERE c_id = 100


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

Uobičajeni nizove veze za SQL Server možete koristiti da biste se povezali vaše BI i podataka Alati za integraciju s bazama podataka na poslužitelju baze podataka SQL elastic upita omogućeno i vanjske tablice koji su definirani. Provjerite je li SQL Server podržana kao izvor podataka za vaše alat. Zatim pogledajte elastic upita baze podataka i vanjske tablice baš kao i sve druge baze podataka SQL Server kojim se želite povezati pomoću alata za vaše. 

## <a name="best-practices"></a>Najbolje prakse 
 
* Provjerite je li koji bazu podataka za krajnje točke elastic upita je dodijelio pristup udaljenom bazom podataka tako da omogućite pristup za servise Azure u svoju konfiguraciju vatrozid SQL DB. I provjerite je li vjerodajnica koje se nalaze u definiciju izvora podataka za vanjski uspješno prijavite se u udaljena baza podataka i ima dozvole za pristup udaljenoj tablici.  

* Elastic upita najbolje funkcionira za upite koje se najčešće se izračuni mogu izvršiti na udaljena baza podataka. Obično se najbolje performanse upita s predikati selektivno filtar koji je moguće vrednovati udaljena baza podataka ili spojevi koji mogu izvršiti potpuno na udaljenom bazom podataka. Drugi upit uzoraka možda morate učitati velike količine podataka iz udaljena baza podataka, a možda slabije. 


## <a name="next-steps"></a>Daljnji koraci

Upit vodoravno particije baze podataka (poznat i kao sharded baza podataka), potražite u članku [upita u bazama podataka sharded oblaka ((vodoravno particije))](sql-database-elastic-query-horizontal-partitioning.md).

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]


<!--Image references-->
[1]: ./media/sql-database-elastic-query-vertical-partitioning/verticalpartitioning.png


<!--anchors-->
