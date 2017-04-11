<properties 
    pageTitle="Praktični vodič: Stvaranje na kanal s Kopiraj aktivnosti pomoću portala za Azure | Microsoft Azure" 
    description="U ovom ćete praktičnom vodiču stvorite kanala na tvorničke podataka Azure s Kopiraj aktivnosti pomoću tvorničke uređivač podataka na portalu za Azure." 
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
    ms.topic="get-started-article" 
    ms.date="09/16/2016" 
    ms.author="spelluru"/>

# <a name="tutorial-create-a-pipeline-with-copy-activity-using-azure-portal"></a>Praktični vodič: Stvaranje na kanal s Kopiraj aktivnosti pomoću portala za Azure
> [AZURE.SELECTOR]
- [Pregled i preduvjeti](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Kopiranje čarobnjaka](data-factory-copy-data-wizard-tutorial.md)
- [Portal za Azure](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [Predloška Azure Voditelj resursa](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [REST API-JA](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET API-JA](data-factory-copy-activity-tutorial-using-dotnet-api.md)



Pomoću ovog praktičnog vodiča pokazuje kako stvoriti i nadzirati na tvorničke Azure podataka pomoću portala za Azure. Kanal na tvorničke podataka koristi aktivnost Kopiraj da biste kopirali podatke iz spremišta blobova platforme Azure s bazom podataka SQL Azure.

Ovdje su navedeni koraci za izvođenje kao dio ovog praktičnog vodiča:

Korak | Opis
-----| -----------
[Stvaranje na tvorničke Azure podataka](#create-data-factory) | U ovom ćete koraku stvoriti na tvorničke Azure podataka pod nazivom **ADFTutorialDataFactory**.  
[Stvorite povezani servisi](#create-linked-services) | U ovom ćete koraku stvoriti dvije povezane usluge: **AzureStorageLinkedService** i **AzureSqlLinkedService**. <br/><br/>Na AzureStorageLinkedService povezuje Azure prostora za pohranu i AzureSqlLinkedService baze podataka Azure SQL povezuje se s ADFTutorialDataFactory. Unos podataka za kanal koji se nalazi u spremniku blob blobova platforme Azure podataka za pohranu i izlaz nalaziti u tablici u bazi podataka Azure SQL. Dakle, dodajte ove dvije podataka sprema kao povezani servisi tvorničke podataka.      
[Stvaranje ulazni i izlazni skupova podataka](#create-datasets) | U prethodnom koraku stvoriti povezani servisi koji se odnose na služi za pohranu podataka koji sadrže podatke ulaza i izlaza. U ovom ćete koraku definirati dvaju skupova podataka – **InputDataset** i **OutputDataset** – koji predstavljaju unos/izlazne podatke koji su pohranjenu u služi za pohranu podataka. <br/><br/>InputDataset, navedite spremnik blob koji sadrži blob s izvorišnim podacima i na OutputDataset, navedite SQL tablicu u kojoj su pohranjeni podaci izlaz. Možete odrediti i druga svojstva kao što su strukturu, dostupnost i pravila. 
[Stvaranje na kanal](#create-pipeline) | U ovom ćete koraku stvorite kanal pod nazivom **ADFTutorialPipeline** u na ADFTutorialDataFactory. <br/><br/>Dodavanje **aktivnosti Kopiraj** kanal kopije ulazne podatke iz na Azure bloba Azure SQL tablicu izlaz. Kopiraj aktivnosti izvodi premještanje podataka na tvorničke Azure podataka. Pokreće se globalno dostupna usluga koje možete kopirati podataka između različitih podataka trgovine sigurne pouzdan te skalabilni način. Članak [Aktivnosti premještanje podataka](data-factory-data-movement-activities.md) potražite u članku dodatne informacije o aktivnosti Kopiraj. 
[Monitor kanal](#monitor-pipeline) | U ovom ćete koraku praćenje isječke ulazni i izlazni tablice pomoću portala za Azure.

## <a name="prerequisites"></a>Preduvjeti 
Preduvjeti za dovršavanje navedene u članku [Vodič pregled](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) prije izvođenja ovog praktičnog vodiča.

## <a name="create-data-factory"></a>Stvaranje tvorničke podataka
U ovom ćete koraku pomoću portala za Azure da biste stvorili na tvorničke Azure podataka pod nazivom **ADFTutorialDataFactory**.

1.  Nakon prijave na [portal za Azure](https://portal.azure.com/), kliknite **Novo**, odaberite **Obavještavanje + analize**pa kliknite **Tvorničke podataka**. 

    ![Novi -> DataFactory](./media/data-factory-copy-activity-tutorial-using-azure-portal/NewDataFactoryMenu.png)  

6. U plohu **Novi tvorničke podataka** :
    1. Unesite **ADFTutorialDataFactory** **naziv**. 
    
        ![Novi plohu tvorničke podataka](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-new-data-factory.png)

        Naziv tvorničke Azure podataka mora biti **globalno jedinstveni**. Ako vam se prikaže sljedeća pogreška, promijenite naziv tvorničke podataka (primjerice, yournameADFTutorialDataFactory), a zatim ponovno pokušajte stvoriti. Pogledajte temu [Tvorničke podataka – pravila imenovanja](data-factory-naming-rules.md) za imenovanja pravila za artefakte tvorničke podataka.
    
            Data factory name “ADFTutorialDataFactory” is not available  
     
        ![Naziv tvorničke podataka nije dostupna](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-data-factory-not-available.png)
    2. Odaberite Azure **pretplate**.
    3. Za grupu resursa učinite nešto od sljedećeg postupka:
        1. Odaberite **Koristi postojeću**, a zatim postojeću grupu resursa s padajućeg popisa. 
        2. Odaberite **Stvori novi**, a zatim unesite naziv za grupu resursa.   
    
            Neke korake ovog praktičnog vodiča pretpostavlja da koristite naziv: **ADFTutorialResourceGroup** za grupu resursa. Dodatne informacije o grupama resursa, potražite u članku [Korištenje resursa grupe da biste upravljali Azure resurse](../azure-resource-manager/resource-group-overview.md).  
    4. Odaberite **mjesto** za tvorničke podataka. Samo područja podržava usluga tvorničke podataka prikazuju se na padajućem popisu.
    5. Odaberite **Prikvači na Startboard**.     
    6. Kliknite **Stvori**.

        > [AZURE.IMPORTANT] Da biste stvorili instance tvorničke podataka, morate biti član uloge [Suradnika tvorničke podataka](../active-directory/role-based-access-built-in-roles.md/#data-factory-contributor) na razini grupe pretplate/resursa.
        >  
        >  Naziv tvorničke podataka možda registrirana kao naziv DNS-a u budućnosti i zato postaju javno vidljivi.              
9.  Da biste vidjeli poruke status/obavijesti, kliknite ikonu zvona na alatnoj traci. 

    ![Obavijesti o](./media/data-factory-copy-activity-tutorial-using-azure-portal/Notifications.png) 
10. Po dovršetku stvaranje vidite plohu **Tvorničke podataka** kao što je prikazano na slici.

    ![Podaci tvorničke Početna stranica](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-data-factory-home-page.png)

## <a name="create-linked-services"></a>Stvorite povezani servisi
Povezani servisi veza služi za pohranu podataka ili izračunati servisa na tvorničke Azure podataka. Potražite u članku [pohranjuje podržanih](data-factory-data-movement-activities.md##supported-data-stores-and-formats) izvora i primatelji podržava aktivnosti Kopiraj. U odjeljku [povezani servisi za izračun](data-factory-compute-linked-services.md) za popis servisa računalnim podržava tvorničke podataka. U ovom ćete praktičnom vodiču neće koristiti bilo koji servis računalnim. 

U ovom ćete koraku stvoriti dvije povezane usluge: **AzureStorageLinkedService** i **AzureSqlLinkedService**. AzureStorageLinkedService povezani veze servisa Azure račun za pohranu i AzureSqlLinkedService povezuje baze podataka Azure SQL **ADFTutorialDataFactory**. Stvorite na kanal u nastavku ovog praktičnog vodiča kopira podatke s blob spremnik u AzureStorageLinkedService tablice SQL u AzureSqlLinkedService.

### <a name="create-a-linked-service-for-the-azure-storage-account"></a>Stvaranje povezanih servisa za račun za Azure prostora za pohranu
1.  U plohu **Tvorničke podataka** kliknite **autora i implementacija** pločicu za pokretanje **uređivača** za tvorničke podataka.

    ![Stvaranje i implementacija pločica](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-author-deploy-tile.png) 
5. U **uređivaču** **Spremanje nove podatke** na kliknite gumb na alatnoj traci, a zatim odaberite **Azure pohranom** na padajućem izborniku. Trebali biste vidjeti JSON predloška za stvaranje na servis povezane Azure prostora za pohranu u desnom oknu. 

    ![Uređivač nove podatke gumb pohrana](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-newdatastore-button.png)    
6. Zamjena `<accountname>` i `<accountkey>` s nazivom račun i račun vrijednosti ključa za vaš račun za Azure prostora za pohranu. 

    ![Uređivač blobova JSON](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-blob-storage-json.png) 
6. Na alatnoj traci kliknite **Implementiraj** . Sada trebali biste vidjeti distribuiranih **AzureStorageLinkedService** u prikazu stabla. 

    ![Spremište blobova platforme uređivač implementacije](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-blob-storage-deploy.png)

> [AZURE.NOTE]
> Potražite u članku [Premještanje podataka s blobova platforme Azure](data-factory-azure-blob-connector.md#azure-storage-linked-service) detalje o JSON svojstva.

### <a name="create-a-linked-service-for-the-azure-sql-database"></a>Stvaranje povezanih servisa za baze podataka SQL Azure
1. U **Uređivaču tvorničke podataka**kliknite **Spremanje nove podatke** gumb na alatnoj traci, a zatim s padajućeg izbornika odaberite **Bazu podataka SQL Azure** . Trebali biste vidjeti JSON predloška za stvaranje servisa SQL Azure povezana u desnom oknu.
2. Zamjena `<servername>`, `<databasename>`, `<username>@<servername>`, a `<password>` s nazivima Azure SQL poslužitelja, bazu podataka, korisnički račun i lozinku. 
3. Na alatnoj traci za stvaranje i implementacija **AzureSqlLinkedService**zatim **Implementiraj** .
4. Provjerite da vidite **AzureSqlLinkedService** u prikazu stabla. 

> [AZURE.NOTE]
> Potražite u članku [Premještanje podataka s bazom podataka SQL Azure](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties) detalje o JSON svojstva.

## <a name="create-datasets"></a>Stvaranje skupova podataka
U prethodnom koraku stvoriti povezani servisi **AzureStorageLinkedService** i **AzureSqlLinkedService** da biste se povezali račun za Azure prostora za pohranu i baze podataka Azure SQL tvorničke podataka: **ADFTutorialDataFactory**. U ovom ćete koraku definirati dvaju skupova podataka – **InputDataset** i **OutputDataset** – koji predstavljaju unos/izlazne podatke koji su pohranjenu u služi za pohranu podataka odnosno poziva AzureStorageLinkedService i AzureSqlLinkedService. InputDataset, navedite spremnik blob koji sadrži blob s izvorišnim podacima i OutputDataset, navedite SQL tablicu u kojoj su pohranjeni podaci izlaz. 

### <a name="create-input-dataset"></a>Stvaranje unosa skup podataka 
U ovom ćete koraku stvoriti skup podataka pod nazivom **InputDataset** koja upućuje na spremniku blob u spremište Azure predstavlja servis **AzureStorageLinkedService** povezani.

1. U **uređivaču** za tvorničke podataka, kliknite **... Dodatne**kliknite **novi skup podataka**, a zatim na padajućem izborniku kliknite **spremište blobova platforme Azure** . 

    ![Novi izbornik skup podataka](./media/data-factory-copy-activity-tutorial-using-azure-portal/new-dataset-menu.png)
2. Zamijenite JSON u desnom oknu sljedeće JSON isječak: 

        {
          "name": "InputDataset",
          "properties": {
            "structure": [
              {
                "name": "FirstName",
                "type": "String"
              },
              {
                "name": "LastName",
                "type": "String"
              }
            ],
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
              "folderPath": "adftutorial/",
              "fileName": "emp.txt",
              "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
              }
            },
            "external": true,
            "availability": {
              "frequency": "Hour",
              "interval": 1
            }
          }
        }
        
     Imajte na umu sljedeće točke: 
    
    - skup podataka **Vrsta** postavljena na **AzureBlob**.
    - **linkedServiceName** postavljen je na **AzureStorageLinkedService**. Stvara povezane servis u koraku 2.
    - **folderPath** postavljen je na spremnik **adftutorial** . Možete odrediti i naziv blob unutar mape pomoću svojstvo **naziv datoteke** . Budući da se ne koji određuje naziv blob-om, podaci iz svih blob-ova u spremniku smatra se kao ulaznih podataka.  
    - Oblikovanje **Vrsta** postavljena na **TextFormat**
    - Postoje dva polja u tekstnoj datoteci – **ime** i **Prezime** – odvojene znakom zarezom (**columnDelimiter**) 
    - **Dostupnost** postavljen na **zaračunava** (**Učestalost** postavljena na **sat** i **interval** postavljeno na **1**). Stoga tvorničke podataka traži ulazne podatke svaki sat u korijenskoj mapi spremnika blob (**adftutorial**) koje ste naveli. 
    
    Ako ne odredite **naziv datoteke** za **unos** skup podataka, sve datoteke/blob-ova iz mape ulazne (**folderPath**) smatraju se unose. Ako odredite naziv datoteke u na JSON, samo određeni datoteka/blob-om se smatra asn unos.
 
    Ako ne odredite **naziv datoteke** za **izlaznu tablicu**, generirani datoteke u **folderPath** se nazivaju u sljedećem obliku: podataka. &lt;Guid\&gt;. txt (primjer: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.).

    Da biste postavili **folderPath** i **naziv datoteke** dinamički na temelju vremena **SliceStart** , koristite svojstvo **partitionedBy** . U sljedećem primjeru folderPath koristi godinu, mjesec i dan iz SliceStart (vrijeme početka isječka obrade), a naziv datoteke koristi sat iz na SliceStart. Na primjer, ako se u isječak Proizvodi za 2016-09-20T08:00:00, na nazivmape postavljen na wikidatagateway/wikisampledataout/2016/09/20 i naziv datoteke je postavljeno na 08.csv. 

            "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
            "fileName": "{Hour}.csv",
            "partitionedBy": 
            [
                { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
                { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
                { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
                { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
            ],
2. Zatim **Implementiraj** na alatnoj traci za stvaranje i implementacija **InputDataset** skupu podataka. Provjerite nalaze li se **InputDataset** u prikazu stabla.

> [AZURE.NOTE]
> Potražite u članku [Premještanje podataka s blobova platforme Azure](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) detalje o JSON svojstva.

### <a name="create-output-dataset"></a>Stvaranje skupa podataka za izlaz
U ovom dijelu koraka stvorite izlazni skup podataka pod nazivom **OutputDataset**. U ovom dataset upućuje na SQL tablice u bazi podataka Azure SQL predstavlja **AzureSqlLinkedService**. 

1. U **uređivaču** za tvorničke podataka, kliknite **... Dodatne**kliknite **novi skup podataka**, a zatim na padajućem izborniku kliknite **Azure SQL** . 
2. Zamijenite JSON u desnom oknu sljedeće JSON isječak:

        {
          "name": "OutputDataset",
          "properties": {
            "structure": [
              {
                "name": "FirstName",
                "type": "String"
              },
              {
                "name": "LastName",
                "type": "String"
              }
            ],
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
              "tableName": "emp"
            },
            "availability": {
              "frequency": "Hour",
              "interval": 1
            }
          }
        }
        
     Imajte na umu sljedeće točke: 
    
    - skup podataka **Vrsta** postavljena na **AzureSQLTable**.
    - **linkedServiceName** postavljen je na **AzureSqlLinkedService** (ste stvorili povezane servis u korak 2).
    - **tablename** postavljen je na **emp**.
    - Postoje tri stupca – **ID-a**, **ime**i **Prezime** – u tablici emp u bazi podataka. ID je stupca identiteta da morate navesti **ime** i **Prezime** u nastavku.
    - **Dostupnost** postavljen je na **zaračunava** (**Učestalost** postavite na **sat** i **interval** postavite na **1**).  Servis podataka tvorničke generira u isječak podataka izlaz svaki sat **emp** tablice u bazi podataka Azure SQL.

3. Zatim **Implementiraj** na alatnoj traci za stvaranje i implementacija **OutputDataset** skupu podataka. Provjerite nalaze li se **OutputDataset** u prikazu stabla. 

> [AZURE.NOTE]
> Potražite u članku [Premještanje podataka s bazom podataka SQL Azure](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties) detalje o JSON svojstva.

## <a name="create-pipeline"></a>Stvaranje kanala
U ovom ćete koraku stvoriti na kanal s **Aktivnosti Kopiraj** koji koristi **InputDataset** kao unos i **OutputDataset** kao rezultat.

1. U **uređivaču** za tvorničke podataka, kliknite **... Dodatne**, a zatim kliknite **Novi kanal**. Osim toga, možete desnom tipkom miša kliknite **kanali** u prikazu stabla i kliknite **Novi kanal**.
2. Zamijenite JSON u desnom oknu sljedeće JSON isječak: 
        
        {
          "name": "ADFTutorialPipeline",
          "properties": {
            "description": "Copy data from a blob to Azure SQL table",
            "activities": [
              {
                "name": "CopyFromBlobToSQL",
                "type": "Copy",
                "inputs": [
                  {
                    "name": "InputDataset"
                  }
                ],
                "outputs": [
                  {
                    "name": "OutputDataset"
                  }
                ],
                "typeProperties": {
                  "source": {
                    "type": "BlobSource"
                  },
                  "sink": {
                    "type": "SqlSink",
                    "writeBatchSize": 10000,
                    "writeBatchTimeout": "60:00:00"
                  }
                },
                "Policy": {
                  "concurrency": 1,
                  "executionPriorityOrder": "NewestFirst",
                  "retry": 0,
                  "timeout": "01:00:00"
                }
              }
            ],
            "start": "2016-07-12T00:00:00Z",
            "end": "2016-07-13T00:00:00Z"
          }
        } 

    Imajte na umu sljedeće točke:

    - U odjeljku aktivnosti postoji samo jedan aktivnosti čija je **Vrsta** postavljen na **kopiji**.
    - Unos aktivnosti je **InputDataset** i izlazni aktivnosti postavljen na **OutputDataset**.
    - U odjeljku **typeProperties** **BlobSource** navedena je kao vrsta izvora i **SqlSink** je naveden kao vrstu primatelj.

    Vrijednost svojstva **pokretanje** zamijenite trenutnu vrijednost dan i **Kraj** s sljedeći dan. Možete navesti samo dio datuma i preskočite vremenski dio datum vrijeme. Na primjer, "2016-02-03", koja je jednaka "2016-02-03T00:00:00Z"
    
    Oba pokrenite i vrijeme završetka mora biti u [obliku ISO](http://en.wikipedia.org/wiki/ISO_8601). Na primjer: 2016-10-14T16:32:41Z. Vrijeme **završetka** nije obavezno, ali koristimo ovog praktičnog vodiča. 
    
    Ako ne navedete vrijednost za svojstvo **Završi** , izračunava se kao "**Početak + 48 sati**". Da biste pokrenuli beskonačno kanal, navedite **9999 09 09** vrijednosti za svojstvo **Završi** .
    
    U prethodnom primjeru, postoje 24 isječke podataka kao svaki isječak podataka zaračunava stvorio.
    
4. Na alatnoj traci za stvaranje i implementacija **ADFTutorialPipeline**zatim **Implementiraj** . Provjerite nalaze li se kanal u prikazu stabla. 
5. Nakon toga zatvorite **uređivač** plohu tako da kliknete **X**. Kliknite **X** da biste posjetite početnu stranicu **Tvorničke podataka** za **ADFTutorialDataFactory**.

**Čestitamo!** Uspješno stvorili na tvorničke Azure podataka, povezani servisi, tablice i na kanal i zakazano kanal.   
 
### <a name="view-the-data-factory-in-a-diagram-view"></a>Prikaz tvorničke podataka u prikazu dijagrama 
1. U plohu **Tvorničke podataka** kliknite **dijagrama**.

    ![Podaci tvorničke plohu - pločica dijagrama](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-datafactoryblade-diagramtile.png)
2. Trebali biste vidjeti dijagram slično kao na sljedećoj slici: 

    ![Prikaz dijagrama](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-diagram-blade.png)

    Povećavanje, Smanjivanje, Zumiranje na 100%, zumiranje Prilagodi, automatski smjestite kanali i tablice i prikaz informacija o podrijetlu (ističe upstream i do stavke odabrane stavke).  Dvokliknite objekt (ulazni i izlazni tablice ili kanal) da biste pogledali svojstva za njega. 
3. Desnom tipkom miša kliknite **ADFTutorialPipeline** u prikazu dijagrama, a zatim kliknite **Otvori kanal**. 

    ![Otvaranje kanal](./media/data-factory-copy-activity-tutorial-using-azure-portal/DiagramView-OpenPipeline.png)
4. Trebali biste vidjeti aktivnosti u kanalu uz ulazni i izlazni skupova podataka za aktivnosti. U ovom ćete praktičnom vodiču imate samo jedan aktivnosti u kanalu (Kopiraj aktivnosti) s InputDataset kao unos skup podataka i OutputDataset kao izlazni skup podataka.   

    ![Otvoriti prikaz kanala](./media/data-factory-copy-activity-tutorial-using-azure-portal/DiagramView-OpenedPipeline.png)
5. Kliknite **tvorničke podataka** u navigaciji u gornjem lijevom kutu da biste se vratili u prikaz dijagrama. Prikaz dijagrama prikazuje sve kanali. U ovom primjeru samo stvorite jedan kanal.   
 

## <a name="monitor-pipeline"></a>Monitor kanal
U ovom ćete koraku praćenje što se događa u na tvorničke Azure podataka pomoću portala za Azure. 

### <a name="monitor-pipeline-using-diagram-view"></a>Kanal monitora pomoću prikaza dijagrama

1. Kliknite **X** da biste zatvorili prikaz **dijagrama** da biste vidjeli na početnu stranicu podataka tvorničke tvorničke podataka. Ako ste zatvorili web-preglednik, učinite sljedeće: 
    2. Otvorite [Centar za Azure](https://portal.azure.com/). 
    2. Dvokliknite **ADFTutorialDataFactory** na **Startboard** (ili) **factories podataka** na lijevom izborniku kliknite, a traženje ADFTutorialDataFactory. 
3. Trebali biste vidjeti count i naziva tablica i kanala koji ste stvorili na ovom plohu.

    ![Početna stranica s nazivima](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-datafactory-home-page-pipeline-tables.png)
4. Sada kliknite pločicu **skupova podataka** .
5. U plohu **skupove podataka** kliknite **InputDataset**. Ovaj skup podataka je unos skup podataka za **ADFTutorialPipeline**.

    ![Skupove podataka s InputDataset odabrana](./media/data-factory-copy-activity-tutorial-using-azure-portal/DataSetsWithInputDatasetFromBlobSelected.png)   
5. Kliknite **... (Trotočje)** Da biste vidjeli sve isječke podataka.

    ![Sve isječke podataka za unos](./media/data-factory-copy-activity-tutorial-using-azure-portal/all-input-slices.png)  

    Obratite pozornost na to da su sve isječke podataka do trenutnog vremena **spremna** , jer postoji datoteka **emp.txt** cijelo vrijeme u spremniku blob: **adftutorial\input**. Provjerite da nema isječaka prikazuju se u odjeljku **nedavno nije uspjelo isječaka** na dnu.

    Popise i **nedavno ažurirano isječaka** i **nedavno nije uspjela isječaka** su sortirani prema **vrijeme ZADNJEG ažuriranja**. 
    
    Na alatnoj traci da biste filtrirali isječke kliknite **Filtar** .  
    
    ![Filtriranje unos isječaka](./media/data-factory-copy-activity-tutorial-using-azure-portal/filter-input-slices.png)
6. Zatvorite na blades dok ne ugledate plohu **skupova podataka** . Kliknite **OutputDataset**. U ovom dataset je izlazni skup podataka za **ADFTutorialPipeline**.

    ![plohu skupa podataka.](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-datasets-blade.png)
6. Trebali biste vidjeti plohu **OutputDataset** kao što je prikazano na sljedećoj slici:

    ![plohu tablice](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-table-blade.png) 
7. Obratite pozornost na to da isječke podataka do trenutnog vremena već su proizvodi i jesu li **spremni**. Nema odsječaka prikazivati u odjeljku **isječke Problem** pri dnu.
8. Kliknite **... (Trotočje)** Da biste vidjeli sve isječke.

    ![plohu isječke podataka](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-dataslices-blade.png)
9. Kliknite bilo koji isječkom podataka s popisa, a trebali biste vidjeti plohu **isječak podataka** .

    ![plohu isječak podataka](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-dataslice-blade.png)
  
    Ako isječak nije **spreman** stanje, vidjet ćete upstream isječci koji su spremni onemogućuju trenutni isječak izvršavaju na popisu **Upstream isječke koje su nije spreman** .
11. U plohu **ISJEČAK podatke** trebali biste vidjeti sve aktivnosti pokreće se u popisu pri dnu. Kliknite za **pokretanje aktivnosti** da biste vidjeli plohu **aktivnosti pokrenuti pojedinosti** . 

    ![Aktivnosti pokrenuti pojedinosti](./media/data-factory-copy-activity-tutorial-using-azure-portal/ActivityRunDetails.png)
12. Kliknite **X** da biste zatvorili sve blades dok ne dobijete natrag za kućne plohu za **ADFTutorialDataFactory**.
14. (neobavezno) Kliknite **kanali** na početnoj stranici za **ADFTutorialDataFactory**kliknite **ADFTutorialPipeline** u plohu **kanali** , a Brazdanje unos tablica (**Consumed**) ili izlazna tablica (**Produced**).
15. Pokretanje **SQL Server Management Studio**, povezivanje s bazom podataka SQL Azure i provjerite je li da se reci umeću u tablicu **emp** u bazi podataka.

    ![Rezultati upita za SQL](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-sql-query-results.png)

### <a name="monitor-pipeline-using-monitor--manage-app"></a>Praćenje kanal pomoću nadzor i upravljanje aplikacije
Također možete koristiti monitora i upravljanje njima aplikaciju za praćenje na kanali. Detaljne informacije o korištenju ove aplikacije potražite u članku [nadzor i upravljanje kanali tvorničke Azure podataka pomoću nadzor i upravljanje aplikacija](data-factory-monitor-manage-app.md).

1. Kliknite pločicu **nadzor i upravljanje** na početnoj stranici za tvorničke vaše podatke.

    ![Praćenje i upravljanje njima pločica](./media/data-factory-copy-activity-tutorial-using-azure-portal/monitor-manage-tile.png) 
2. Trebali biste vidjeti **nadzor i upravljanje aplikacija**. Promijenite **vrijeme početka** i **završetka** obuhvaćaju start (2016-07 – 12) i kraja snimke (2016-07-13) na kanal i kliknite **Primijeni**. 

    ![Praćenje i upravljanje njima aplikacije](./media/data-factory-copy-activity-tutorial-using-azure-portal/monitor-and-manage-app.png) 
3. Prozor programa aktivnosti odaberite na popisu **Aktivnosti Windows** da biste vidjeli detalje o njoj. 
    ![Detalji o aktivnosti prozora](./media/data-factory-copy-activity-tutorial-using-azure-portal/activity-window-details.png)

## <a name="summary"></a>Sažetak 
U ovom ćete praktičnom vodiču ste stvorili na tvorničke Azure podataka da biste kopirali podatke iz programa Azure bloba s bazom podataka Azure SQL. Portal za Azure koristi se za stvaranje tvorničke podataka, povezani servisi, skupova podataka i na kanal. Evo nekoliko koraka za više razine izvršiti pomoću ovog praktičnog vodiča:  

1.  Stvara Azure **tvorničke podataka**.
2.  Stvara **povezani servisi**:
    1. Servis **Za pohranu Azure** povezana da biste se povezali račun za Azure pohranu koja sadrži ulazne podatke.    
    2. Servis **Azure SQL** povezana da biste se povezali bazu podataka Azure SQL koja sadrži podatke. 
3.  Stvara **skupove podataka** koje opisuju ulaznih podataka i podatke za kanali.
4.  Stvara **kanal** s **Kopiraj aktivnosti** s **BlobSource** kao izvor i **SqlSink** kao primatelj.  


## <a name="see-also"></a>Vidi također
| Tema | Opis |
| :---- | :---- |
| [Aktivnosti premještanje podataka](data-factory-data-movement-activities.md) | Ovaj članak sadrži detaljne informacije o aktivnosti Kopiraj koji se koriste u praktičnom vodiču. |
| [Planiranje i izvođenja](data-factory-scheduling-and-execution.md) | U ovom se članku objašnjava zakazivanja i izvođenja aspekte Azure podataka tvorničke modelu. |
| [Kanali](data-factory-create-pipelines.md) | U ovom se članku olakšava razumijevanje kanali i aktivnosti u tvorničke Azure podataka. |
| [Skupove podataka](data-factory-create-datasets.md) | U ovom se članku olakšava razumijevanje skupova podataka na tvorničke Azure podataka.
| [Nadzor i upravljanje kanali pomoću aplikacije za praćenje](data-factory-monitor-manage-app.md) | U ovom se članku opisuje kako nadzor, upravljanje i kanali pomoću nadzor i upravljanje aplikacija za ispravljanje pogrešaka. 


