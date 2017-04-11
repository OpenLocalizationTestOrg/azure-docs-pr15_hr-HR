<properties 
    pageTitle="SQL Server pohranjene Procedure aktivnosti" 
    description="Saznajte kako možete koristiti u SQL Server pohranjene Procedure aktivnosti pozvati pohranjena procedura baze podataka SQL Azure ili Azure SQL Data Warehouse iz podataka tvorničke kanal." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/30/2016" 
    ms.author="spelluru"/>

# <a name="sql-server-stored-procedure-activity"></a>SQL Server pohranjene Procedure aktivnosti
> [AZURE.SELECTOR]
[Grozd](data-factory-hive-activity.md)  
[Svinja](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoop strujanje](data-factory-hadoop-streaming-activity.md)
[Strojnog učenja](data-factory-azure-ml-batch-execution-activity.md) 
[Pohranjene Procedure](data-factory-stored-proc-activity.md)
[Podataka U u analize Lake-SQL](data-factory-usql-activity.md)
[.NET prilagođene](data-factory-use-custom-activities.md)

SQL Server pohranjena procedura aktivnosti u podataka tvorničke [kanal](data-factory-create-pipelines.md) možete koristiti za pozivanje pohranjena procedura u jednom od sljedećih spremišta podataka: 


- Baze podataka Azure SQL 
- Data Warehouse Azure SQL  
- SQL Server baza podataka u tvrtki ili je VM Azure. Morate instalirati pristupnik za upravljanje podacima na istom računalu koje hostira baze podataka ili na zasebnom računalo da biste izbjegli natječu za resurse s bazom podataka. Pristupnik za upravljanje podacima je softver koji povezuje lokalnih podataka izvora/podataka izvora hosed u Azure VMs servise u oblaku, siguran i upravljani način. Potražite u članku [Premještanje podataka između lokalnog i oblaka](data-factory-move-data-between-onprem-and-cloud.md) članak detalje o pristupnik za upravljanje podacima. 

U ovom se članku sastavlja na članak [aktivnosti transformacije podataka](data-factory-data-transformation-activities.md) koja predstavlja Općenito pregled i transformaciju podataka te aktivnosti podržani transformaciju.

## <a name="walkthrough"></a>Vodič

### <a name="sample-table-and-stored-procedure"></a>Ogledna tablica i pohranjena procedura
1. Stvorite sljedeća **tablica** u bazi podataka SQL Azure pomoću SQL Server Management Studio ili bilo koji drugi alat ste upoznati s. Stupac datetimestamp je datum i vrijeme kada je generiran odgovarajuće ID-a. 

        CREATE TABLE dbo.sampletable
        (
            Id uniqueidentifier,
            datetimestamp nvarchar(127)
        )
        GO

        CREATE CLUSTERED INDEX ClusteredID ON dbo.sampletable(Id);
        GO

    ID jedinstven označena je i stupac datetimestamp je datum i vrijeme kada je generiran odgovarajuće ID-a.
    ![Ogledni podaci](./media/data-factory-stored-proc-activity/sample-data.png)

    > [AZURE.NOTE] Ovaj primjer koristi baze podataka SQL Azure, ali funkcionira na isti način za Azure SQL Data Warehouse i baze podataka SQL Server. 
2. Stvorite na sljedeći **pohranjena procedura** koji se umeće podatke u **sampletable**.

        CREATE PROCEDURE sp_sample @DateTime nvarchar(127)
        AS
        
        BEGIN
            INSERT INTO [sampletable]
            VALUES (newid(), @DateTime)
        END

    > [AZURE.IMPORTANT] **Ime** i **kućište** parametra (DateTime u ovom primjeru) moraju se podudarati koji parametar naveden u kanal/aktivnosti JSON. Definicija pohranjena procedura Pobrinite se da **@** se koristi kao prefiks za parametar.
    
### <a name="create-a-data-factory"></a>Stvaranje podataka tvorničke  
4. Prijavite se na [Azure portal](https://portal.azure.com/). 
5. Na izborniku lijevo kliknite **NOVO** , kliknite **Obavještavanje + analize**pa kliknite **Tvorničke podataka**.
    
    ![Novi tvorničke podataka](media/data-factory-stored-proc-activity/new-data-factory.png)   
4.  U plohu **Novi tvorničke podataka** unesite **SProcDF** naziv. **Globalno**jedinstvenih Azure imena tvorničke podataka. Morate prefiks naziv tvorničke podataka uz naziv da biste omogućili uspjelo stvaranje na tvorničke.

    ![Novi tvorničke podataka](media/data-factory-stored-proc-activity/new-data-factory-blade.png)      
3.  Odaberite **Azure pretplate**. 
4.  **Grupa resursa**za učinite nešto od sljedećeg postupka: 
    1.  Kliknite **Stvori novi** , a zatim unesite naziv za grupu resursa.
    2.  Kliknite **Koristi postojeći** , a zatim odaberite postojeću grupu resursa.  
5.  Odaberite **mjesto** za tvorničke podataka.
6.  Odaberite **Prikvači na nadzornu ploču** tako da možete vidjeti tvorničke podatke na nadzornoj ploči prilikom sljedeće prijave u. 
6.  Kliknite **Stvori** **Novi podataka tvorničke** plohu.
6.  Vidjet ćete tvorničke podataka koji je stvoren u **nadzorne ploče** Azure portal. Nakon tvorničke podataka uspješno je stvorena, vidjet ćete stranici tvorničke podataka koji prikazuje sadržaj tvorničke podataka.
    ![Podaci tvorničke Početna stranica](media/data-factory-stored-proc-activity/data-factory-home-page.png)

### <a name="create-an-azure-sql-linked-service"></a>Stvaranje programa servisa SQL Azure povezana  
Nakon stvaranja tvorničke podataka, stvoriti na servis SQL Azure povezana koja vodi baze podataka SQL Azure tvorničke podataka. Ova baza podataka sadrži na sampletable tablica i sp_sample pohranjena procedura.

7.  Kliknite **autora i implementacija** na **Tvorničke podataka** plohu za **SProcDF** da biste pokrenuli uređivač tvorničke podataka.
2.  Kliknite **Spremanje nove podatke** na traci izbornika i odaberite **Baze podataka SQL Azure**. Trebali biste vidjeti JSON skripte za stvaranje na servis SQL Azure povezana u uređivaču. 

    ![Novi spremišta podataka](media/data-factory-stored-proc-activity/new-data-store.png)
4. U skriptu JSON, provjerite sljedeće promjene: 
    1. Zamjena ** &lt;naziv poslužitelja&gt; ** s nazivom poslužitelja baze podataka SQL Azure.
    2. Zamjena ** &lt;ImeBazePodataka&gt; ** s bazom podataka u kojem ste stvorili tablicu i pohranjena procedura.
    3. Zamjena ** &lt; username@servername ** s korisničkih računa koji ima pristup bazi podataka.
    4. Zamjena ** &lt;lozinka&gt; ** pomoću lozinke za korisnički račun. 

    ![Novi spremišta podataka](media/data-factory-stored-proc-activity/azure-sql-linked-service.png)
5. Kliknite **uvođenja** trake s naredbama za implementaciju povezane servisa. Provjerite nalaze li se AzureSqlLinkedService u prikazu stabla na lijevoj strani. 

    ![Prikaz stabla sa servisom za povezane](media/data-factory-stored-proc-activity/tree-view.png)

### <a name="create-an-output-dataset"></a>Stvaranje skupa podataka na Izlaz
6. Kliknite **... Dodatne** na alatnoj traci kliknite **novi skup podataka**pa kliknite **Azure SQL**. **Novi skup podataka** na naredbenoj traci i odaberite **Azure SQL**.

    ![Prikaz stabla sa servisom za povezane](media/data-factory-stored-proc-activity/new-dataset.png)
7. Kopirajte i zalijepite sljedeću skriptu JSON u uređivač JSON.

        {               
            "name": "sprocsampleout",
            "properties": {
                "type": "AzureSqlTable",
                "linkedServiceName": "AzureSqlLinkedService",
                "typeProperties": {
                    "tableName": "sampletable"
                },
                "availability": {
                    "frequency": "Hour",
                    "interval": 1
                }
            }
        }
7. Kliknite **uvođenja** trake s naredbama za implementaciju skupu podataka. Provjerite nalaze li se skup podataka u prikazu stabla. 

    ![Prikaz stabla povezani servisi sustava](media/data-factory-stored-proc-activity/tree-view-2.png)

### <a name="create-a-pipeline-with-sqlserverstoredprocedure-activity"></a>Stvaranje na kanal s SqlServerStoredProcedure aktivnosti
Sada ćemo stvoriti na kanal s SqlServerStoredProcedure aktivnosti.
 
9. Kliknite **... Dodatne** na naredbu traku, a zatim kliknite **Novi kanal**. 
9. Kopirajte i zalijepite sljedeće JSON isječka. Postavite **storedProcedureName** **sp_sample**. Ime i kućište parametra **DateTime** mora podudarati ime i kućište parametra u definiciji pohranjena procedura.  

        {
            "name": "SprocActivitySamplePipeline",
            "properties": {
                "activities": [
                    {
                        "type": "SqlServerStoredProcedure",
                        "typeProperties": {
                            "storedProcedureName": "sp_sample",
                            "storedProcedureParameters": {
                                "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)"
                            }
                        },
                        "outputs": [
                            {
                                "name": "sprocsampleout"
                            }
                        ],
                        "scheduler": {
                            "frequency": "Hour",
                            "interval": 1
                        },
                        "name": "SprocActivitySample"
                    }
                ],
                "start": "2016-08-02T00:00:00Z",
                "end": "2016-08-02T05:00:00Z",
                "isPaused": false
            }
        }

    Ako morate null za parametar, pomoću ove sintakse: "param1": null (mala slova). 
9. Na alatnoj traci za implementaciju kanal, zatim **Implementiraj** .  

### <a name="monitor-the-pipeline"></a>Praćenje kanal

6. Kliknite **X** da biste zatvorili blades uređivač tvorničke podataka i vratite se na tvorničke podataka plohu, a zatim **dijagrama**.

    ![pločica dijagrama](media/data-factory-stored-proc-activity/data-factory-diagram-tile.png)
7. U **Prikazu dijagrama**, pogledajte pregled kanali i skupova podataka koristi ovog praktičnog vodiča. 

    ![pločica dijagrama](media/data-factory-stored-proc-activity/data-factory-diagram-view.png)
8. U prikazu dijagrama dvokliknite dataset **sprocsampleout**. Vidjet ćete isječaka u stanju spremno. Treba pet isječke jer je isječak Proizvodi za svaki sat između vrijeme početka i vrijeme završetka od na JSON.

    ![pločica dijagrama](media/data-factory-stored-proc-activity/data-factory-slices.png) 
10. Kada je isječak u stanju **spremno** , pokrenite s * *Odaberite* sampletable** upita u bazi podataka Azure SQL da biste potvrdili da podatke je umetnuti u tablici pohranjena procedura.

    ![Izlazne podatke](./media/data-factory-stored-proc-activity/output.png)

    Detaljne informacije o nadzoru kanali tvorničke Azure podataka potražite u članku [Monitor kanal](data-factory-monitor-manage-pipelines.md) .  

> [AZURE.NOTE] U ovom primjeru u SprocActivitySample sadrži bez unosa. Ako želite da lanac ove aktivnosti s upstream aktivnosti (to jest, prethodnih obrada), izlaze upstream aktivnosti koje se mogu koristiti kao unose u ova aktivnosti. U tom slučaju aktivnost izvršiti sve dok ne dovrši upstream aktivnosti i izlaze upstream aktivnosti dostupni (u spreman statusa). Ulaza nije moguće koristiti izravno kao parametar aktivnosti pohranjena procedura

## <a name="json-format"></a>JSON OSNOVNI oblik
    {
        "name": "SQLSPROCActivity",
        "description": "description", 
        "type": "SqlServerStoredProcedure",
        "inputs":  [ { "name": "inputtable"  } ],
        "outputs":  [ { "name": "outputtable" } ],
        "typeProperties":
        {
            "storedProcedureName": "<name of the stored procedure>",
            "storedProcedureParameters":  
            {
                "param1": "param1Value"
                …
            }
        }
    }

## <a name="json-properties"></a>Svojstva JSON

Svojstvo | Opis | Obavezno
-------- | ----------- | --------
ime | Naziv aktivnosti | Da
Opis | Tekst koji opisuje što aktivnost koristi | ne
Vrsta | SqlServerStoredProcedure | Da
unosa | Neobavezno. Ako navedete unos skup podataka, moraju biti dostupne (u 'spreman statusa) pohranjena procedura aktivnosti da biste pokrenuli. Unos skup podataka ne potrošnju pohranjena procedura kao parametar. Koristi se samo da biste provjerili ovisnosti prije pokretanja pohranjena procedura aktivnosti. | ne
izlaze | Morate navesti izlaz sa skupovima podataka pohranjena procedura aktivnosti. Izlazni skup podataka određuje **raspored** aktivnosti pohranjena procedura (zaračunava, tjedno, mjesečno, itd.). <br/><br/>Izlazni skup podataka morate koristiti **povezane servisa** koji se odnosi na baze podataka SQL Azure ili je Data Warehouse SQL Azure ili baze podataka SQL Server u kojem želite da pohranjena procedura da biste pokrenuli. <br/><br/>Skup podataka za izlaz vam može poslužiti kao rezultat pohranjene procedure za prosljeđivanje narednih obrade tako da druge aktivnosti ([Ulančavanje aktivnosti](data-factory-scheduling-and-execution.md#chaining-activities)) u kanalu način. Međutim, tvorničke podataka automatski pisanje izlaz pohranjena procedura za ovaj skup podataka. To je pohranjena procedura koji zapisuje u SQL tablicu koja pokazuje izlazni skup podataka. <br/><br/>U nekim slučajevima izlazni skup podataka može biti **taj skup podataka**, koji se koristi samo za određivanje rasporeda za pokretanje aktivnosti pohranjena procedura. | Da
storedProcedureName | Odredite naziv pohranjene procedure u bazi podataka Azure SQL ili skladištu podataka SQL Azure koji predstavlja povezane servis koji koristi izlazne tablice. | Da
storedProcedureParameters | Navedite vrijednosti za parametre pohranjena procedura. Ako je potrebno proći null za parametar pomoću ove sintakse: "param1": vrijednosti null (sve mala slova). Pogledajte sljedeći primjer dodatne informacije o korištenju to svojstvo.| ne

## <a name="passing-a-static-value"></a>Prosljeđivanje statična vrijednost 
Sada ćemo razmislite o dodavanju drugi stupac pod nazivom 'Scenarij' u tablici koja sadrži statična vrijednost pod nazivom "Dokumenta uzorak".

![Ogledni podaci 2](./media/data-factory-stored-proc-activity/sample-data-2.png)

    CREATE PROCEDURE sp_sample @DateTime nvarchar(127) , @Scenario nvarchar(127)
    
    AS
    
    BEGIN
        INSERT INTO [sampletable]
        VALUES (newid(), @DateTime, @Scenario)
    END

Sada, prenesite parametar scenarij i vrijednost iz pohranjena procedura aktivnosti. U odjeljku typeProperties u prethodnom primjeru izgleda sljedeće isječak:

    "typeProperties":
    {
        "storedProcedureName": "sp_sample",
        "storedProcedureParameters": 
        {
            "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)",
            "Scenario": "Document sample"
        }
    }

