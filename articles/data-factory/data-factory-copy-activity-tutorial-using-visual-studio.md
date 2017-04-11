<properties 
    pageTitle="Praktični vodič: Stvaranje na kanal s Kopiraj aktivnosti pomoću Visual Studio | Microsoft Azure" 
    description="U ovom ćete praktičnom vodiču stvarate kanala na tvorničke podataka Azure s Kopiraj aktivnosti pomoću Visual Studio." 
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
    ms.date="10/17/2016" 
    ms.author="spelluru"/>

# <a name="tutorial-create-a-pipeline-with-copy-activity-using-visual-studio"></a>Praktični vodič: Stvaranje na kanal s Kopiraj aktivnosti pomoću Visual Studio
> [AZURE.SELECTOR]
- [Pregled i preduvjeti](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Kopiranje čarobnjaka](data-factory-copy-data-wizard-tutorial.md)
- [Portal za Azure](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [Predloška Azure Voditelj resursa](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [REST API-JA](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET API-JA](data-factory-copy-activity-tutorial-using-dotnet-api.md)


Pomoću ovog praktičnog vodiča pokazuje kako stvoriti i nadzirati na tvorničke Azure podataka pomoću Visual Studio. Kanal na tvorničke podataka koristi aktivnost Kopiraj da biste kopirali podatke iz spremišta blobova platforme Azure s bazom podataka SQL Azure.

Ovdje su navedeni koraci za izvođenje kao dio ovog praktičnog vodiča:

1. Stvaranje dva povezana servisa: **AzureStorageLinkedService1** i **AzureSqlinkedService1**. 

    Na AzureStorageLinkedService1 povezuje Azure prostora za pohranu i AzureSqlLinkedService1 veze baze podataka Azure SQL na tvorničke podataka: **ADFTutorialDataFactoryVS**. Ulaznih podataka za kanal koji se nalazi u spremniku blob u spremište blobova platforme Azure i izlazni podaci se pohranjuju u tablicu u bazi podataka Azure SQL. Dakle, dodajte ove dvije podataka sprema kao povezani servisi tvorničke podataka.
2. Stvaranje dvaju skupova podataka: **InputDataset** i **OutputDataset**koji predstavljaju ulaza i izlaza podatke koji su pohranjenu u služi za pohranu podataka. 

    Za InputDataset, navedite spremnik blob koji sadrži blob s izvorišnim podacima. Za OutputDataset, navedite tablici SQL u kojoj su pohranjeni podaci izlaz. Možete odrediti i druga svojstva kao što su strukturu, dostupnost i pravila.
3. Stvaranje kanala pod nazivom **ADFTutorialPipeline** u na ADFTutorialDataFactoryVS. 

    Kanal sadrži **Kopiju aktivnosti** koju kopije ulazne podatke iz na Azure bloba Azure SQL tablicu izlaz. Kopiraj aktivnosti izvodi premještanje podataka na tvorničke Azure podataka. Aktivnost se pokreće globalno raspoloživ servis koji možete kopirati podataka između različitih podataka trgovine sigurne pouzdan te skalabilni način. Članak [Aktivnosti premještanje podataka](data-factory-data-movement-activities.md) potražite u članku dodatne informacije o aktivnosti Kopiraj. 
4. Stvaranje podataka tvorničke pod nazivom **VSTutorialFactory**. Implementacija tvorničke podataka i svi entiteti tvorničke podataka (povezani servisi, tablice i kanal).    

## <a name="prerequisites"></a>Preduvjeti

1. Pročitajte članak [Vodič](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) , a zatim dovršite korake **preduvjeta** . 
2. Da biste mogli objaviti entiteti tvorničke podataka na tvorničke Azure podataka, morate biti **administrator Azure pretplate** .  
3. Morate imati instaliran na vašem računalu sljedeće: 
    - Visual Studio 2013 ili Visual Studio 2015.
    - Preuzmite Azure SDK za Visual Studio 2013 ili Visual Studio 2015. Dođite do [Stranice za preuzimanje Azure](https://azure.microsoft.com/downloads/) i kliknite **Dodavanje veze za VANJSKIH 2013** ili **Dodavanje veze za VANJSKIH 2015** u odjeljku **.NET** .
    - Preuzmite najnovije Azure podataka tvorničke dodatak za Visual Studio: [Dodavanje veze za VANJSKIH 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) ili [Dodavanje veze za VANJSKIH 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005). Dodatak možete ažurirati i tako da učinite sljedeće: na izborniku kliknite **Alati** -> **proširenja i ažuriranja** -> **Online** -> **Visual Studio Galerija** -> **Microsoft Azure podataka tvorničke Tools za Visual Studio** -> **Ažuriranje**.

## <a name="create-visual-studio-project"></a>Stvaranje projekta za Visual Studio 
1. Pokrenite **Visual Studio 2013**. Kliknite **datoteka**, pokažite na **Novo**, a kliknite **projekt**. Trebali biste vidjeti dijaloški okvir **Novi projekt** .  
2. U dijaloškom okviru **Novi projekt** odaberite željeni predložak **DataFactory** pa kliknite **Prazni projekt tvorničke podataka**. Ako ne vidite DataFactory predložak, zatvorite Visual Studio, instalirajte Azure SDK za Visual Studio 2013 i ponovno otvorite Visual Studio.  

    ![Dijaloški okvir za novi projekt](./media/data-factory-copy-activity-tutorial-using-visual-studio/new-project-dialog.png)

3. Unesite **naziv** za projekt, **mjesto**i naziv **rješenja**, a zatim kliknite **u redu**.

    ![Preglednik rješenja](./media/data-factory-copy-activity-tutorial-using-visual-studio/solution-explorer.png) 

## <a name="create-linked-services"></a>Stvorite povezani servisi
Povezani servisi veza služi za pohranu podataka ili izračunati servisa na tvorničke Azure podataka. Potražite u članku [pohranjuje podržanih](data-factory-data-movement-activities.md##supported-data-stores-and-formats) izvora i primatelji podržava aktivnosti Kopiraj. U odjeljku [povezani servisi za izračun](data-factory-compute-linked-services.md) za popis servisa računalnim podržava tvorničke podataka. U ovom ćete praktičnom vodiču neće koristiti bilo koji servis računalnim. 

U ovom ćete koraku stvoriti dvije povezane usluge: **AzureStorageLinkedService1** i **AzureSqlLinkedService1**. AzureStorageLinkedService1 povezane veze servisa Azure račun za pohranu i AzureSqlLinkedService veze baze podataka Azure SQL na tvorničke podataka: **ADFTutorialDataFactory**. 

### <a name="create-the-azure-storage-linked-service"></a>Stvaranje servisa Azure prostora za pohranu povezana

4. Desnom tipkom miša kliknite **Povezani servisi** u pregledniku rješenja, pokažite na **Dodaj**pa kliknite **Nova stavka**.      
5. U dijaloškom okviru **Dodaj novu stavku** na popisu odaberite **Povezanu servis za pohranu za Azure** , a zatim kliknite **Dodaj**. 

    ![Nove povezane usluge](./media/data-factory-copy-activity-tutorial-using-visual-studio/new-linked-service-dialog.png)
 
3. Zamjena `<accountname>` i `<accountkey>`* uz naziv računa za Azure prostora za pohranu i njegova ključa. 

    ![Azure prostora za pohranu povezana servisa](./media/data-factory-copy-activity-tutorial-using-visual-studio/azure-storage-linked-service.png)

4. Spremite datoteku **AzureStorageLinkedService1.json** .

> Potražite u članku [Premještanje podataka s blobova platforme Azure](data-factory-azure-blob-connector.md#azure-storage-linked-service) detalje o JSON svojstva.

### <a name="create-the-azure-sql-linked-service"></a>Stvaranje servisa SQL Azure povezana

5. Desnom tipkom miša kliknite čvor **Povezani servisi** u **Pregledniku rješenja** ponovno, pokažite na **Dodaj**pa kliknite **Nova stavka**. 
6. Ovaj put odaberite **Servis za povezane SQL Azure**pa kliknite **Dodaj**. 
7. U **AzureSqlLinkedService1.json datoteku**, zamijenite `<servername>`, `<databasename>`, `<username@servername>`, a `<password>` s nazivima Azure SQL poslužitelja, bazu podataka, korisnički račun i lozinku.    
8.  Spremite datoteku **AzureSqlLinkedService1.json** . 

> [AZURE.NOTE]
> Potražite u članku [Premještanje podataka s bazom podataka SQL Azure](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties) detalje o JSON svojstva.

## <a name="create-datasets"></a>Stvaranje skupova podataka
U prethodnom koraku stvoriti povezani servisi **AzureStorageLinkedService1** i **AzureSqlLinkedService1** da biste se povezali račun za Azure prostora za pohranu i baze podataka Azure SQL tvorničke podataka: **ADFTutorialDataFactory**. U ovom ćete koraku definirati dvaju skupova podataka – **InputDataset** i **OutputDataset** – koji predstavljaju unos/izlazne podatke koji su pohranjenu u služi za pohranu podataka odnosno poziva AzureStorageLinkedService1 i AzureSqlLinkedService1. Za InputDataset, navedite spremnik blob koji sadrži blob s izvorišnim podacima. Za OutputDataset, navedite tablici SQL u kojoj su pohranjeni podaci izlaz.

### <a name="create-input-dataset"></a>Stvaranje unosa skup podataka
U ovom ćete koraku stvoriti skup podataka pod nazivom **InputDataset** koja upućuje na spremniku blob u spremište Azure predstavlja servis **AzureStorageLinkedService1** povezani. Tablice je pravokutni skup podataka i samo vrstu dataset podržane odmah. 

9. Desnom tipkom miša kliknite **tablice** u **Pregledniku rješenja**, pokažite na **Dodaj**pa kliknite **Nova stavka**.
10. U dijaloškom okviru **Dodaj novu stavku** odaberite **Blobova platforme Azure**pa kliknite **Dodaj**.   
10. Zamijenite tekst JSON sljedeći tekst, a zatim spremite datoteku **AzureBlobLocation1.json** . 

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
            "linkedServiceName": "AzureStorageLinkedService1",
            "typeProperties": {
              "folderPath": "adftutorial/",
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

    Da biste postavili **folderPath** i **naziv datoteke** dinamički na temelju vremena **SliceStart** , koristite svojstvo **partitionedBy** . U sljedećem primjeru folderPath koristi godinu, mjesec i dan iz SliceStart (vrijeme početka odsječak obrade), a naziv datoteke koristi sat iz na SliceStart. Na primjer, ako se u isječak Proizvodi za 2016-09-20T08:00:00, na nazivmape postavljen na wikidatagateway/wikisampledataout/2016/09/20 i naziv datoteke je postavljeno na 08.csv. 

            "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
            "fileName": "{Hour}.csv",
            "partitionedBy": 
            [
                { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
                { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
                { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
                { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 

> [AZURE.NOTE]
> Potražite u članku [Premještanje podataka s blobova platforme Azure](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) detalje o JSON svojstva.

### <a name="create-output-dataset"></a>Stvaranje skupa podataka za izlaz
U ovom ćete koraku stvoriti izlazni skup podataka pod nazivom **OutputDataset**. U ovom dataset upućuje na SQL tablice u bazi podataka Azure SQL predstavlja **AzureSqlLinkedService1**. 

11. Ponovno kliknite **tablice** u **Pregledniku rješenja** desnom tipkom miša, pokažite na **Dodaj**pa kliknite **Nova stavka**.
12. U dijaloškom okviru **Dodaj novu stavku** odaberite **Azure SQL**pa kliknite **Dodaj**. 
13. Zamijenite tekst JSON sljedeće JSON, a zatim spremite datoteku **AzureSqlTableLocation1.json** .

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
            "linkedServiceName": "AzureSqlLinkedService1",
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

> [AZURE.NOTE]
> Potražite u članku [Premještanje podataka s bazom podataka SQL Azure](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties) detalje o JSON svojstva.

## <a name="create-pipeline"></a>Stvaranje kanala 
Stvorili ste povezani servisi ulaza i izlaza i tablice dosad. Sada stvorite na kanal s **Kopiraj aktivnosti** da biste kopirali podatke iz sustava Azure bloba s bazom podataka Azure SQL. 


1. Desnom tipkom miša kliknite **kanali** u **Pregledniku rješenja**, pokažite na **Dodaj**pa kliknite **Nova stavka**.  
15. Odaberite **Kopiju podataka kanal** u dijaloškom okviru **Dodaj novu stavku** , a zatim kliknite **Dodaj**. 
16. Zamijenite na JSON sljedeće JSON i spremite datoteku **CopyActivity1.json** .
            
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
                  "style": "StartOfInterval",
                  "retry": 0,
                  "timeout": "01:00:00"
                }
              }
            ],
            "start": "2015-07-12T00:00:00Z",
            "end": "2015-07-13T00:00:00Z",
            "isPaused": false
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

## <a name="publishdeploy-data-factory-entities"></a>Objavljivanje i implementacija entiteti tvorničke podataka
U ovom ćete koraku objavljujete entiteti tvorničke podataka (povezani servisi, skupova podataka i kanal) koju ste ranije stvorili. I navedite naziv novog tvorničke podataka će biti stvoren čekanju te entiteti.  

18. Desnom tipkom miša kliknite projekt u pregledniku rješenja, a zatim kliknite **Objavi**. 
19. Ako se prikaže dijaloški okvir **prijavite se na Microsoftov račun** , unesite vjerodajnice za račun koji ima Azure pretplatu, a zatim kliknite **Prijava**.
20. Trebali biste vidjeti sljedeći dijaloški okvir:

    ![Dijaloški okvir objavljivanje](./media/data-factory-copy-activity-tutorial-using-visual-studio/publish.png)
21. Na stranici Konfiguracija podataka tvorničke, učinite sljedeće: 
    1. Odaberite mogućnost **Stvori novu tvorničke podataka** .
    2. Unesite **VSTutorialFactory** **naziv**.  
    
        > [AZURE.IMPORTANT]  
        > Naziv tvorničke Azure podataka mora biti globalno jedinstveni. Ako se pojavi pogreška o nazivu tvorničke podataka za objavljivanje, promijenite naziv tvorničke podataka (primjerice, yournameVSTutorialFactory) i pokušajte ponovno objaviti. Pogledajte temu [Tvorničke podataka – pravila imenovanja](data-factory-naming-rules.md) za imenovanja pravila za artefakte tvorničke podataka.     
    3. Odaberite Azure pretplatu za polje **pretplate** .
     
        > [AZURE.IMPORTANT]Ako ne vidite sve pretplate, provjerite je li prijavljeni pomoću računa koji je administrator ili ko administrator pretplate.  
    4. odaberite **grupu resursa** za tvorničke podataka će biti stvoren. 5. Odaberite **područje** za tvorničke podataka. Samo područja podržava usluga tvorničke podataka prikazuju se na padajućem popisu.
6. Kliknite **Dalje** da biste prešli na stranicu za **Objavljivanje stavki** .
    
        ![Konfiguriranje stranicu tvorničke podataka](media/data-factory-copy-activity-tutorial-using-visual-studio/configure-data-factory-page.png)   
23. Na stranici **Objavljivanje stavki** Pobrinite se da sve na podataka Factories entiteti odabrane pa kliknite **Dalje** da biste prešli na stranicu **Sažetak** .
    
    ![Objavljivanje stavke stranice](media/data-factory-copy-activity-tutorial-using-visual-studio/publish-items-page.png)     
24. Pregledajte sažetak i kliknite **Dalje** da biste započeli postupak implementacije i prikaz **Statusa implementacije**.

    ![Objavljivanje stranica sa sažetkom](media/data-factory-copy-activity-tutorial-using-visual-studio/publish-summary-page.png)
25. Na stranici **Stanje implementacije** trebali biste vidjeti status postupka implementacije. Po završetku implementaciju, kliknite Završi. 
    ![Stranica sa statusom implementaciju](media/data-factory-copy-activity-tutorial-using-visual-studio/deployment-status.png) Imajte na umu sljedeće točke: 

- Ako vam se prikaže pogreška: "**Da biste koristili polje naziva Microsoft.DataFactory nije registriran ove pretplate**", učinite nešto od sljedećeg i pokušajte ponovno objaviti: 

    - U Azure PowerShell pokrenite sljedeću naredbu da biste registrirali davatelja tvorničke podataka. 
        
            Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    
        Možete pokrenite sljedeću naredbu da biste potvrdili koji tvorničke podataka koji je registriran davatelja usluga. 
    
            Get-AzureRmResourceProvider
    - Prijavite se pomoću Azure pretplate na [portal za Azure](https://portal.azure.com) i idite na tvorničke podataka plohu (ili) stvaranje podataka tvorničke na portalu za Azure. Ova akcija automatski registrira davatelj usluga za vas.
-   Naziv tvorničke podataka možda registrirana kao naziv DNS-a u budućnosti i zato postaju javno vidljivi.

> [AZURE.IMPORTANT] Da biste stvorili instance tvorničke podataka, morate biti administrator/ko-administrator Azure pretplate

## <a name="summary"></a>Sažetak
U ovom ćete praktičnom vodiču ste stvorili na tvorničke Azure podataka da biste kopirali podatke iz programa Azure bloba s bazom podataka Azure SQL. Visual Studio koristi se za stvaranje tvorničke podataka, povezani servisi, skupova podataka i na kanal. Evo nekoliko koraka za više razine izvršiti pomoću ovog praktičnog vodiča:  

1.  Stvara Azure **tvorničke podataka**.
2.  Stvara **povezani servisi**:
    1. Servis **Za pohranu Azure** povezana da biste se povezali račun za Azure pohranu koja sadrži ulazne podatke.    
    2. Servis **Azure SQL** povezana da biste se povezali bazu podataka Azure SQL koja sadrži podatke. 
3.  Stvara **skupove podataka**koje opisuju ulaznih podataka i podatke za kanali.
4.  Stvara **kanal** s **Kopiraj aktivnosti** s **BlobSource** kao izvor i **SqlSink** kao primatelj. 


## <a name="use-server-explorer-to-view-data-factories"></a>Korištenje Eksplorera za poslužitelj da biste pogledali factories podataka

1. U **Visual Studio**, na izborniku kliknite **Prikaz** pa kliknite **Explorer poslužitelja**.
2. U prozoru programa Explorer poslužitelja proširite **Azure** a **Tvorničke podataka**. Ako vidite **prijavite se u Visual Studio**, unesite **račun** povezan s pretplatom Azure i kliknite **Nastavi**. Unesite **lozinku**i kliknite **Prijava**. Visual Studio pokušava pronaći informacije o svim factories Azure podataka u svoju pretplatu. Prikaz statusa ovaj postupak u prozoru **Popis zadataka tvorničke podataka** .
    ![Poslužitelj programa Explorer](./media/data-factory-copy-activity-tutorial-using-visual-studio/server-explorer.png)
3. Desnom tipkom miša kliknite na tvorničke za podatke i odaberite izvoz podataka tvorničke novi projekt da biste stvorili Visual Studio projektu na temelju postojeće tvorničke za podatke.
    ![Izvoz podataka tvorničke Dodavanje veze za VANJSKIH projekt](./media/data-factory-copy-activity-tutorial-using-visual-studio/export-data-factory-menu.png)  

## <a name="update-data-factory-tools-for-visual-studio"></a>Ažuriranje podataka tvorničke Alati za Visual Studio
Da biste ažurirali Azure podataka tvorničke Alati za Visual Studio, učinite sljedeće:

1. Na izborniku kliknite **Alati** , a zatim odaberite **proširenja i ažuriranja**. 
2. Odaberite **ažuriranja** u lijevom oknu, a zatim odaberite **Visual Studio galerije**.
4. Odaberite **Alati tvorničke Azure podataka za Visual Studio** , a zatim kliknite **Ažuriraj**. Ako ne vidite tu stavku, već imate najnoviju verziju alata. 

Potražite u članku [Monitor skupova podataka i kanal](data-factory-copy-activity-tutorial-using-azure-portal.md#monitor-pipeline) za upute za praćenje kanal i skupova podataka pomoću portala za Azure ste stvorili pomoću ovog praktičnog vodiča.

## <a name="see-also"></a>Vidi također
| Tema | Opis |
| :---- | :---- |
| [Aktivnosti premještanje podataka](data-factory-data-movement-activities.md) | Ovaj članak sadrži detaljne informacije o aktivnosti Kopiraj koji se koriste u praktičnom vodiču. |
| [Planiranje i izvođenja](data-factory-scheduling-and-execution.md) | U ovom se članku objašnjava zakazivanja i izvođenja aspekte Azure podataka tvorničke modelu. |
| [Kanali](data-factory-create-pipelines.md) | U ovom se članku olakšava razumijevanje kanali i aktivnosti u tvorničke Azure podataka |
| [Skupove podataka](data-factory-create-datasets.md) | U ovom se članku olakšava razumijevanje skupova podataka na tvorničke Azure podataka.
| [Nadzor i upravljanje kanali pomoću aplikacije za praćenje](data-factory-monitor-manage-app.md) | U ovom se članku opisuje kako nadzor, upravljanje i kanali pomoću nadzor i upravljanje aplikacija za ispravljanje pogrešaka. 
