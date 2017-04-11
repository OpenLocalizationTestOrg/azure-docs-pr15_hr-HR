   <properties
   pageTitle="Podatke učitali u Azure SQL Data Warehouse | Microsoft Azure"
   description="Saznajte uobičajeni scenariji za podatke učitavanje u SQL Data Warehouse. To obuhvaća pomoću PolyBase, blobova platforme Azure, paušalni datoteke i dostave disk. Možete koristiti i alati drugih proizvođača."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="07/12/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="load-data-into-azure-sql-data-warehouse"></a>Podatke učitali u skladištu podataka za SQL Azure

Sažetak mogućnosti scenarij i preporuke za učitavanje podataka u SQL Data Warehouse.

Najteži dio učitavanja podataka obično Priprema podataka za mogućnost Učitaj. Azure pojednostavljuje učitavanje pomoću blobova platforme Azure kao zajednički izvor podataka za mnoge servise, a pomoću Azure podataka tvorničke orkestrirali komunikacije i podataka kretanje između servisa Azure. Ti procesi integrirati tehnologijom PolyBase koji koristi massively paralelno obrada (MPP) da biste učitali podatke paralelno iz spremišta blobova platforme Azure u SQL Data Warehouse. 

Vodiči za koje učitajte ogledne baze podataka, potražite u članku [Učitavanje ogledne baze podataka][].

## <a name="load-from-azure-blob-storage"></a>Učitavanje iz spremišta blobova platforme Azure
Najbrži način za uvoz podataka u SQL Data Warehouse je za korištenje PolyBase da biste učitali podatke iz spremišta blobova platforme Azure. PolyBase koristi dizajn SQL Data Warehouse massively paralelno obrada (MPP) da biste učitali podatke paralelno iz spremišta blobova platforme Azure. Da biste koristili PolyBase, možete koristiti T SQL naredbi ili na tvorničke podataka Azure kanal.

### <a name="1-use-polybase-and-t-sql"></a>1. koristite PolyBase i T SQL

Sažetak učitavanja postupak:

2. Oblikujte podatke kao UTF-8 jer PolyBase trenutno ne podržava UTF-16.
2. Premještanje podataka u spremište blobova platforme Azure i spremite ga na tekstne datoteke.
3. Konfiguriranje vanjskog objekata u SQL Data Warehouse da biste odredili mjesto i oblikovanje podataka
4. Pokrenite T SQL naredbu da biste učitali podatke paralelno u novu tablicu u bazi podataka.

<!-- 5. Schedule and run a loading job. --> 

Praktični vodič, potražite u članku [učitavanja podataka iz spremište blobova platforme Azure SQL Data Warehouse (PolyBase)][].

### <a name="2-use-azure-data-factory"></a>2. tvorničke Azure podataka koristi

Za jednostavniji način za PolyBase, možete stvoriti na tvorničke podataka Azure kanal koji koristi PolyBase da biste učitali podatke iz spremišta blobova platforme Azure u SQL Data Warehouse. Ovo je za konfiguriranje Budući da ne morate definirati T SQL objekata. Ako vam je potrebna za dohvaćanje vanjskih podataka bez njihovog uvoza, koristite T SQL. 

Sažetak učitavanja postupak:

2. Oblikujte podatke kao UTF-8 jer PolyBase trenutno ne podržava UTF-16.
2. Premještanje podataka u spremište blobova platforme Azure i spremite ga na tekstne datoteke.
3. Stvorite na tvorničke podataka Azure kanal za ingest podatke. Pomoću mogućnosti PolyBase.
4. Zakazivanje i pokretanje kanal.

Praktični vodič, potražite u članku [učitavanja podataka iz spremište blobova platforme Azure SQL Data Warehouse (Azure podataka tvorničke)][].


## <a name="load-from-sql-server"></a>Učitavanje iz sustava SQL Server
Da biste učitali podatke iz sustava SQL Server SQL Data Warehouse možete koristiti integraciju servisa (SSIS), prijenos paušalni datoteka ili diskova isporuke Microsoftu. Čitanje na potražite u članku sažetak različite učitavanje postupke i veze na vodiče.

Planiranje migracije svih podataka iz sustava SQL Server u SQL Data Warehouse potražite u članku [Pregled migracije][]. 

### <a name="use-integration-services-ssis"></a>Korištenje servisima za integraciju (SSIS)
Ako već koristite paketi za integraciju servisa (SSIS) da biste učitali u SQL Server, možete ažurirati vaše paketa da biste koristili SQL Server kao izvor i SQL Data Warehouse kao odredište. Ovo je brz i jednostavan izvršite i dobar je ako ne želite migrirati proces učitavanja već korištenja podataka u oblaku. U tradeoff je opterećenje bit će sporije od korištenja PolyBase jer ovo SSIS izvoditi opterećenje paralelno.

Sažetak učitavanja postupak:

1. Pregledavanje pakiranju servisima za integraciju tako da pokazuje na instancu sustava SQL Server za izvor i baze podataka SQL Data Warehouse za odredište.
2. Da biste SQL Data Warehouse migrirati sheme Ako još niste već.
3. Promijeni mapiranje u vašem paketa koristiti samo vrste podataka koje podržava SQL Data Warehouse.
3. Zakazivanje i pokretanje paketa.

Praktični vodič, potražite u članku [učitavanja podataka iz sustava SQL Server za Azure SQL podataka skladištu (SSIS)][].

### <a name="use-azcopy-recommended-for--10-tb-data"></a>Korištenje AZCopy (preporučuje za podatke < 10 TB)
Ako je veličina poštanskog podataka < 10 TB, možete izvesti podatke iz sustava SQL Server u plošnu datoteke, kopirajte datoteke blobova platforme Azure, i pomoću PolyBase učitali podatke u SQL Data Warehouse

Sažetak učitavanja postupak:

1. Izvoz podataka iz sustava SQL Server u paušalni datoteke pomoću pomoćnog programa naredbenog retka bcp.
2. Da biste kopirali podatke iz datoteka s nehijerarhijskom blobova platforme Azure pomoću pomoćnog programa naredbenog retka AZCopy.
3. Da biste učitali u SQL Data Warehouse pomoću PolyBase.

Praktični vodič, potražite u članku [učitavanja podataka iz spremište blobova platforme Azure SQL Data Warehouse (PolyBase)][].

### <a name="use-bcp"></a>Korištenje bcp
Ako imate mali količinu podataka bcp možete koristiti da biste učitali izravno u Azure SQL Data Warehouse.

Sažetak učitavanja postupak:
1. Izvoz podataka iz sustava SQL Server u paušalni datoteke pomoću pomoćnog programa naredbenog retka bcp.
2. Da biste učitali podatke iz paušalni datoteka izravno u SQL Data Warehouse pomoću bcp.

Praktični vodič, potražite u članku [učitavanja podataka iz sustava SQL Server za Azure SQL Data Warehouse (bcp)][].


### <a name="use-importexport-recommended-for--10-tb-data"></a>Korištenje uvoz/izvoz (preporučuje se radi o podacima > 10 TB)
Ako je vaš veličina podataka > 10 TB i želite da biste premjestili Azure, preporučujemo da koristite naš disk dostavu servis za [Uvoz/izvoz][]. 

Sažetak učitavanja postupak
2. Koristite uslužni bcp naredbenog retka za izvoz podataka iz sustava SQL Server paušalni datotekama na transferrable diskova.
3. Isporuka diskova Microsoftu.
4. Microsoft učitava podatke u SQL Data Warehouse

## <a name="load-from-hdinsight"></a>Učitavanje iz servisa HDInsight
SQL Data Warehouse podržava učitavanja podataka iz servisa HDInsight putem PolyBase. Postupak je isti kao učitavanja podataka iz spremišta blobova platforme Azure – pomoću PolyBase da biste se povezali sa servisom HDInsight da biste učitali podatke. 

### <a name="1-use-polybase-and-t-sql"></a>1. koristite PolyBase i T SQL

Sažetak učitavanja postupak:

2. Oblikujte podatke kao UTF-8 jer PolyBase trenutno ne podržava UTF-16.
2. Premještanje podataka u HDInsight i spremite ga na tekstne datoteke, ORC ili Parquet oblikovanja.
3. Konfiguriranje vanjskog objekata u SQL Data Warehouse da biste odredili mjesto i oblik podataka.
4. Pokrenite T SQL naredbu da biste učitali podatke paralelno u novu tablicu u bazi podataka.

Praktični vodič, potražite u članku [učitavanja podataka iz spremište blobova platforme Azure SQL Data Warehouse (PolyBase)][].

## <a name="recommendations"></a>Preporuke

Mnoge našim partnerima imati učitavanja rješenja. Da biste saznali više, pogledajte popis naš [rješenja partnera][]. 

Ako vaši podaci dolazi iz izvora koji nisu relacijski i želite da biste učitali u SQL Data Warehouse morat ćete njihovo pretvaranje u retke i stupce prije nego ga učitati. Transformiranih podataka ne mora biti pohranjena u bazi podataka, sprema u tekstne datoteke.

Stvaranje Statistika upravo učitavanja podataka. Azure SQL Data Warehouse ne još nisu podršku automatsko stvaranje i automatsko ažuriranje Statistika.  Da biste ostvarili najbolje performanse zatražite od upite, važno je da biste stvorili Statistika na sve stupce sve tablice nakon prve učitavanja ili znatno promjene se pojavljuju u podacima.  Detalje potražite u članku [Statistika][].


## <a name="next-steps"></a>Daljnji koraci
Dodatne savjete za razvoj potražite u članku [Pregled razvoj][].

<!--Image references-->

<!--Article references-->
[Učitavanje podataka iz spremišta blobova platforme Azure SQL Data Warehouse (PolyBase)]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md
[Učitavanje podataka iz spremišta blobova platforme Azure SQL Data Warehouse (Azure podataka tvorničke)]: ./sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md
[Učitavanje podataka iz sustava SQL Server za Azure SQL podataka skladištu (SSIS)]: ./sql-data-warehouse-load-from-sql-server-with-integration-services.md
[Učitavanje podataka iz sustava SQL Server Azure SQL Data Warehouse (bcp)]: ./sql-data-warehouse-load-from-sql-server-with-bcp.md
[Load data from SQL Server to Azure SQL Data Warehouse (AZCopy)]: ./sql-data-warehouse-load-from-sql-server-with-azcopy.md

[Učitavanje ogledne baze podataka]: ./sql-data-warehouse-load-sample-databases.md
[Pregled migracije]: ./sql-data-warehouse-overview-migrate.md
[rješenja partnera]: ./sql-data-warehouse-partner-business-intelligence.md
[Pregled razvoj]: ./sql-data-warehouse-overview-develop.md
[Statistika]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->

<!--Other Web references-->
[Uvoz/izvoz]: https://azure.microsoft.com/documentation/articles/storage-import-export-service/
