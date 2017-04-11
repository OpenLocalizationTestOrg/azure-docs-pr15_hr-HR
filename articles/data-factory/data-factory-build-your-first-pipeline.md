<properties
    pageTitle="Praktični vodič tvorničke podataka: prvi kanal podataka | Microsoft Azure"
    description="Pomoću ovog praktičnog vodiča Azure podataka tvorničke prikazuje kako stvoriti i zakazivanje tvorničke podataka koji obrađuje podatke pomoću skripte grozd Hadoop klaster."
    services="data-factory"
    keywords="Azure podataka tvorničke vodiča, hadoop klaster, grozd hadoop"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="09/26/2016"
    ms.author="spelluru"/>

# <a name="tutorial-build-your-first-pipeline-to-process-data-using-hadoop-cluster"></a>Praktični vodič: Stvaranje vaš prvi kanal postupak podatke pomoću Hadoop klaster 
> [AZURE.SELECTOR]
- [Pregled i preduvjeti](data-factory-build-your-first-pipeline.md)
- [Portal za Azure](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Voditelj resursa predloška](data-factory-build-your-first-pipeline-using-arm.md)
- [REST API-JA](data-factory-build-your-first-pipeline-using-rest-api.md)

Pomoću ovog praktičnog vodiča stvaranja prvog tvorničke Azure podataka s kanalima podataka koji obrađuje podatke tako da pokrenete grozd skripte na klasteru programa Azure HDInsight (Hadoop). 

> [AZURE.NOTE] U ovom se članku ne nudi konceptualni pregled tvorničke Azure podataka. Konceptualni pregled usluge potražite u članku [Uvod u tvorničke Azure podataka](data-factory-introduction.md). Potražite u članku [podataka tvorničke tečaj](https://azure.microsoft.com/documentation/learning-paths/data-factory/) preporučeni način da biste se kretali sadržaj dodatne informacije o tvorničke podataka.

## <a name="whats-covered-in-this-tutorial"></a>Što je obuhvaćeno ovog praktičnog vodiča? 
**Tvorničke Azure podataka** omogućuje vam da biste sastavili podataka **premještanja** i zadacima **obradu** podataka kao utemeljenih na podacima tijekovi rada (naziva se i kanali podataka). Saznajte kako izraditi prvi aktivnosti kanal obradu podataka (ili transformaciju podataka) podatke. Aktivnost koristi HDInsight Hadoop klaster pretvaranje i analizirajte zapisnika uzorak web.  

U ovom ćete praktičnom vodiču poduzeti sljedeće korake:

1.  Stvaranje **tvorničke podataka**. Na tvorničke podataka može sadržavati jedno ili više podataka pipelines te postupak i Premjesti podatke. 
2.  Stvorite **povezani servisi**. Stvorite povezani servisa za povezivanje spremišta podataka ili usluge računalnim na tvorničke podataka. Spremište podataka kao što je prostor za pohranu Azure sadrži podatke ulaza i izlaza aktivnosti u kanalu. Računalnim servisa kao što su HDInsight Hadoop klaster procesa/transformacije podataka.    
3.  Stvaranje ulazni i izlazni **skupova podataka**. Unos dataset predstavlja unosa za neku aktivnost u kanalu, a skup podataka za izlaz izlaz aktivnosti.
3.  Stvaranje **kanala**. Na kanal može imati jednu ili više aktivnosti (Primjeri: kopiranje aktivnosti, aktivnosti vrste Hive HDInsight). Ovaj primjer koristi aktivnost grozd HDInsight koji se izvodi grozd skripte na HDInsight Hadoop klaster. Skripta najprije stvoriti tablicu koja se odnosi na neobrađenog web zapisnika podataka pohranjenih u spremište blobova platforme Azure, a zatim po godini i mjesecu partitions sirovim podacima.

    Postoje dvije vrste aktivnosti koje podržava tvorničke Azure podataka. Su: [aktivnosti premještanje podataka](data-factory-data-movement-activities.md) i [aktivnosti transformacije podataka](data-factory-data-transformation-activities.md). Postoji samo jedan podatkovni premještanja aktivnosti, koji je kopija aktivnosti. U ovom ćete praktičnom vodiču ne koristite aktivnost Kopiraj. Praktični vodič o korištenju aktivnosti Kopiraj, potražite u članku [Praktični vodič: kopiranje podataka iz programa Azure bloba za Azure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). HDInsight grozd aktivnosti pomoću ovog praktičnog vodiča jedan je od aktivnosti transformacije podataka podržava tvorničke podataka.  
 
Ovdje je **Dijagram prikaza** tvorničke podataka uzorka stvaranja ovog praktičnog vodiča. 

![Prikaz dijagrama u praktičnom vodiču tvorničke podataka](./media/data-factory-build-your-first-pipeline/data-factory-tutorial-diagram-view.png)

U ovom ćete praktičnom vodiču **inputdata** mapu spremnika blobova platforme Azure **adfgetstarted** sadrži jednu datoteku pod nazivom input.log. Datoteka zapisnika sadrži unose iz tri mjeseca: siječanj, veljača i ožujak 2016. Evo oglednih redaka za svaki mjesec u ulaznoj datoteci. 

    2016-01-01,02:01:09,SAMPLEWEBSITE,GET,/blogposts/mvc4/step2.png,X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,53175,871 
    2016-02-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871
    2016-03-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871

Kada kanal s HDInsight vrste Hive aktivnosti obrade datoteke, aktivnosti pokreće skriptu grozd klaster HDInsight particije unos podataka po godini i mjesecu. Skripta stvara tri izlazne mape koje sadrže datoteke sa stavkama svaki mjesec.  

    adfgetstarted/partitioneddata/year=2016/month=1/000000_0
    adfgetstarted/partitioneddata/year=2016/month=2/000000_0
    adfgetstarted/partitioneddata/year=2016/month=3/000000_0

Iz redaka uzorka gore navedenoj sintaksi prvoga (s 2016-01-01) napisan 000000_0 datoteku u mjesecu = 1 mape. Isto tako, napisali drugu datoteku u mjesecu = 2 mapu, a treći nešto napisan na datoteku u mjesecu = 3 mape.  


## <a name="pre-requisites"></a>Prije requisites
Prije početka ovog praktičnog vodiča, morate imati sljedeće preduvjete:

1.  **Azure pretplate** – ako nemate pretplatu na Azure, možete stvoriti pomoću računa za probno razdoblje u samo nekoliko minuta. U članku [Besplatnu probnu](https://azure.microsoft.com/pricing/free-trial/) na kako možete nabaviti pomoću računa za probno razdoblje.

2.  **Prostor za pohranu azure** – koristite račun za Azure prostora za pohranu za spremanje podataka pomoću ovog praktičnog vodiča. Ako nemate račun za Azure prostora za pohranu, u članku [Stvaranje računa za pohranu](../storage/storage-create-storage-account.md#create-a-storage-account) . Kada stvorite račun za pohranu, imajte na umu dolje **naziv računa** i **Tipkovni prečac**. Potražite u članku [Prikaz, Kopiraj i regenerate prostora za pohranu pristupnih tipki](../storage/storage-create-storage-account.md#view-and-copy-storage-access-keys). 

### <a name="upload-files-to-azure-storage-for-the-tutorial"></a>Prijenos datoteka na Azure prostora za pohranu za vodič
Prije početka vodič, morate pripremiti računa za pohranu Azure s ogledne datoteke za vodič.

1. Prenesite datoteke upita grozd (HQL) mapa za **skripte** kontejnera blob **adfgetstarted** .
2. Prijenos datoteke unos **inputdata** mapu blob spremnik **adfgetstarted** . 

#### <a name="create-hql-script-file"></a>Stvaranje HQL skriptna datoteka 

1. Pokretanje **Blok za pisanje** i zalijepite sljedeću skriptu HQL. Ova skripta grozd stvara dvije tablice: **WebLogsRaw** i **WebLogsPartitioned**. Na izborniku kliknite **datoteka** pa odaberite **Spremi kao**. Prelazak na **C:\adfgetstarted** mapu na tvrdom disku. Odaberite * *sve datoteke (*.*) **za na** Spremi kao vrstu** polja. Unesite **partitionweblogs.hql** za na **naziv datoteke**. Provjerite na **kodiranje** polje pri dnu dijaloškog okvira postavljeno na **ANSI**. Ako nije, postavite ga na **ANSI **.  

        set hive.exec.dynamic.partition.mode=nonstrict;
        
        DROP TABLE IF EXISTS WebLogsRaw; 
        CREATE TABLE WebLogsRaw (
          date  date,
          time  string,
          ssitename string,
          csmethod  string,
          csuristem  string,
          csuriquery string,
          sport int,
          susername string,
          cipcsUserAgent string,
          csCookie string,
          csReferer string,
          cshost  string,
          scstatus  int,
          scsubstatus  int,
          scwin32status  int,
          scbytes int,
          csbytes int,
          timetaken int
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        LINES TERMINATED BY '\n' 
        tblproperties ("skip.header.line.count"="2");
        
        LOAD DATA INPATH '${hiveconf:inputtable}' OVERWRITE INTO TABLE WebLogsRaw;
        
        DROP TABLE IF EXISTS WebLogsPartitioned ; 
        create external table WebLogsPartitioned (  
          date  date,
          time  string,
          ssitename string,
          csmethod  string,
          csuristem  string,
          csuriquery string,
          sport int,
          susername string,
          cipcsUserAgent string,
          csCookie string,
          csReferer string,
          cshost  string,
          scstatus  int,
          scsubstatus  int,
          scwin32status  int,
          scbytes int,
          csbytes int,
          timetaken int
        )
        partitioned by ( year int, month int)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' 
        STORED AS TEXTFILE 
        LOCATION '${hiveconf:partitionedtable}';
        
        INSERT INTO TABLE WebLogsPartitioned  PARTITION( year , month) 
        SELECT
          date,
          time,
          ssitename,
          csmethod,
          csuristem,
          csuriquery,
          sport,
          susername,
          cipcsUserAgent,
          csCookie,
          csReferer,
          cshost,
          scstatus,
          scsubstatus,
          scwin32status,
          scbytes,
          csbytes,
          timetaken,
          year(date),
          month(date)
        FROM WebLogsRaw

Tijekom izvođenja aktivnosti vrste Hive u kanalu podataka tvorničke prosljeđuje vrijednosti u **inputtable** i **partitionedtable** parametre kao što je prikazano u sljedećim isječak:  

        "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
        "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"

**Storageaccountname** je naziv vašeg računa Azure prostora za pohranu.
 
#### <a name="create-a-sample-input-file"></a>Stvaranje oglednu datoteku za unos
Korištenje Blok za pisanje, stvorite datoteku pod nazivom **input.log** u **c:\adfgetstarted** pomoću sljedećeg sadržaja: 

    #Software: Microsoft Internet Information Services 8.0
    #Fields: date time s-sitename cs-method cs-uri-stem cs-uri-query s-port cs-username c-ip cs(User-Agent) cs(Cookie) cs(Referer) cs-host sc-status sc-substatus sc-win32-status sc-bytes cs-bytes time-taken
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step4.png X-ARR-LOG-ID=4bea5b3d-8ac9-46c9-9b8c-ec3e9500cbea 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 72177 871 47
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step5.png X-ARR-LOG-ID=9b0c14b1-434d-495a-9b0d-46775194257b 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 37931 871 32
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step6.png X-ARR-LOG-ID=f99cff81-ec38-4a13-b2fe-21b10adca996 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 27146 871 47
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step1.png X-ARR-LOG-ID=af94d3b4-8e05-4871-82c4-638f866d4e83 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 66259 871 140
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47

#### <a name="upload-input-file-and-hql-file-to-your-azure-blob-storage"></a>Prijenos unos datoteke i datoteke HQL u spremištu blobova platforme Azure

U ovom se odjeljku daju upute o korištenju alata za **AzCopy** da biste kopirali datoteke input.log i partitionweblogs.hql spremište blobova platforme Azure. Možete koristiti bilo koji alat po izboru (na primjer: [Microsoft Azure prostora za pohranu Explorer](http://storageexplorer.com/), [CloudXPlorer softverom ClumsyLeaf](http://clumsyleaf.com/products/cloudxplorer) da biste izvršili taj zadatak.   
     
1. Preuzmite [najnoviju verziju **AzCopy**](http://aka.ms/downloadazcopy)ili pak [najnoviju verziju za pretpregled](http://aka.ms/downloadazcopypr). Pogledajte [upute za korištenje AzCopy](../storage/storage-use-azcopy.md) članak upute za korištenje uslužni.
2. Dođite do mape u c:\adfgetstarted i pokrenite sljedeću naredbu: 

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\AzCopy" /Source:. /Dest:https://<storageaccountname>.blob.core.windows.net/adfgetstarted/inputdata /DestKey:<storageaccesskey>  /Pattern:input.log

    Ta se naredba prenosi **input.log** datoteke na račun za pohranu (**adfgetstarted** spremnik i **inputdata** mape). Zamijenite **storageaccountname** s nazivom vašeg računa za pohranu i **storageaccesskey** tipkovni prečac za pohranu.

    > [AZURE.NOTE] Ta naredba stvara kontejner **adfgetstarted** u spremištu blobova Azure i kopira **input.log** datoteka s lokalnog pogona mapu **inputdata** u spremniku. 
    
3. Kada datoteku uspješno je prenesen, vidjet ćete slično kao jedna od AzCopy izlaz.
    
        Finished 1 of total 1 file(s).
        [2015/12/16 23:07:33] Transfer summary:
        -----------------
        Total files transferred: 1
        Transfer successfully:   1
        Transfer skipped:        0
        Transfer failed:         0
        Elapsed time:            00.00:00:01
5. Pokrenite sljedeću naredbu da biste prenijeli datoteku **partitionweblogs.hql** na mapa za **skripte** spremnika **adfgetstarted** . Evo naredbu: 
    
        AzCopy /Source:. /Dest:https://<storageaccountname>.blob.core.windows.net/adfgetstarted/script /DestKey:<storageaccesskey>  /Pattern:partitionweblogs.hql

Dovršite preduvjete. Možete stvoriti na tvorničke podataka pomoću jednog od sljedećih načina. Kliknite jednu od kartice na vrhu ili na sljedećim vezama za izvođenje vodič. 

- [Portal za Azure](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Voditelj resursa predloška](data-factory-build-your-first-pipeline-using-arm.md)
- [REST API-JA](data-factory-build-your-first-pipeline-using-rest-api.md)