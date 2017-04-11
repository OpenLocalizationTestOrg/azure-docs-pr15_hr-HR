<properties
    pageTitle="Stvaranje prve podataka tvorničke (Visual Studio) | Microsoft Azure"
    description="U ovom ćete praktičnom vodiču stvorite kanal za Azure podataka tvorničke uzorka pomoću Visual Studio."
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
    ms.topic="hero-article" 
    ms.date="10/17/2016"
    ms.author="spelluru"/>

# <a name="tutorial-build-your-azure-first-data-factory-using-microsoft-visual-studio"></a>Praktični vodič: Stvaranje prve Azure podataka tvorničke koristeći Microsoft Visual Studio
> [AZURE.SELECTOR]
- [Pregled i preduvjeti](data-factory-build-your-first-pipeline.md)
- [Portal za Azure](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Voditelj resursa predloška](data-factory-build-your-first-pipeline-using-arm.md)
- [REST API-JA](data-factory-build-your-first-pipeline-using-rest-api.md)

U ovom članku, koristite Microsoft Visual Studio za stvaranje vaš prvi tvorničke Azure podataka.

## <a name="prerequisites"></a>Preduvjeti
1. Pročitajte članak [Vodič](data-factory-build-your-first-pipeline.md) , a zatim dovršite korake **preduvjeta** .
2. Da biste mogli objaviti entiteti tvorničke podataka iz Visual Studio tvorničke Azure podataka, morate biti **administrator Azure pretplate** .
3. Morate imati instaliran na vašem računalu sljedeće: 
    - Visual Studio 2013 ili Visual Studio 2015.
    - Preuzmite Azure SDK za Visual Studio 2013 ili Visual Studio 2015. Dođite do [Stranice za preuzimanje Azure](https://azure.microsoft.com/downloads/) i kliknite **Dodavanje veze za VANJSKIH 2013** ili **Dodavanje veze za VANJSKIH 2015** u odjeljku **.NET** .
    - Preuzmite najnovije Azure podataka tvorničke dodatak za Visual Studio: [Dodavanje veze za VANJSKIH 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) ili [Dodavanje veze za VANJSKIH 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005). Dodatak možete ažurirati i tako da učinite sljedeće: na izborniku kliknite **Alati** -> **proširenja i ažuriranja** -> **Online** -> **Visual Studio Galerija** -> **Microsoft Azure podataka tvorničke Tools za Visual Studio** -> **Ažuriranje**. 
 
Sada ćemo pomoću programa Visual Studio stvaranje na tvorničke Azure podataka. 


## <a name="create-visual-studio-project"></a>Stvaranje projekta za Visual Studio 
1. Pokrenite **Visual Studio 2013** ili **Visual Studio 2015**. Kliknite **datoteka**, pokažite na **Novo**, a kliknite **projekt**. Trebali biste vidjeti dijaloški okvir **Novi projekt** .  
2. U dijaloškom okviru **Novi projekt** odaberite željeni predložak **DataFactory** pa kliknite **Prazni projekt tvorničke podataka**.   

    ![Dijaloški okvir za novi projekt](./media/data-factory-build-your-first-pipeline-using-vs/new-project-dialog.png)

3. Unesite **naziv** za projekt, **mjesto**i naziv **rješenja**, a zatim kliknite **u redu**.

    ![Preglednik rješenja](./media/data-factory-build-your-first-pipeline-using-vs/solution-explorer.png)

## <a name="create-linked-services"></a>Stvorite povezani servisi
Podaci tvorničke može imati jednu ili više kanali. Na kanal može imati jednu ili više aktivnosti u njoj. Na primjer, Kopiraj aktivnost da biste kopirali podatke iz izvora Odredišno spremište podataka i HDInsight grozd aktivnosti da biste pokrenuli grozd skripta za pretvaranje ulaznih podataka. Potražite u članku [pohranjuje podržanih](data-factory-data-movement-activities.md##supported-data-stores-and-formats) izvora i primatelji podržava aktivnosti Kopiraj. U odjeljku [povezani servisi za izračun](data-factory-compute-linked-services.md) za popis servisa računalnim podržava tvorničke podataka. 

U ovom ćete koraku povežete račun za Azure prostora za pohranu i programa klaster Azure HDInsight na zahtjev na tvorničke podataka. Račun za Azure pohranu sadrži podatke ulazni i izlazni za kanal u ovom primjeru. Servis za HDInsight povezana koristi se za pokrenuti skriptu grozd naveden u aktivnosti kanala u ovom primjeru. Prepoznavanje podataka koje je spremište/računalnim servisa se koriste u scenariju i veza tih servisa na tvorničke podataka stvaranjem povezane usluge.  

Navedite naziv i postavke tvorničke podataka naknadno Kad objavite rješenje tvorničke podataka.

#### <a name="create-azure-storage-linked-service"></a>Stvaranje servisa Azure prostora za pohranu povezana
U ovom ćete koraku povezati s računom za pohranu Azure na tvorničke vaše podatke. Za ovog praktičnog vodiča koristiti isti prostor za pohranu Azure račun za pohranu podataka ulaza i izlaza i HQL skriptna datoteka. 

4. Desnom tipkom miša kliknite **Povezani servisi** u pregledniku rješenja, pokažite na **Dodaj**pa kliknite **Nova stavka**.      
5. U dijaloškom okviru **Dodaj novu stavku** na popisu odaberite **Povezanu servis za pohranu za Azure** , a zatim kliknite **Dodaj**. 
3. Zamijenite **accountname** i **accountkey** naziv računa za Azure prostora za pohranu i njegova ključa. Da biste saznali kako dobiti pristup ključ za pohranu, pročitajte članak [Prikaz, Kopiraj i regenerate prostora za pohranu pristupnih tipki](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys)

    ![Azure prostora za pohranu povezana servisa](./media/data-factory-build-your-first-pipeline-using-vs/azure-storage-linked-service.png)

4. Spremite datoteku **AzureStorageLinkedService1.json** .

#### <a name="create-azure-hdinsight-linked-service"></a>Stvaranje servisa Azure HDInsight povezana
U ovom ćete koraku veza do klaster HDInsight na zahtjev na tvorničke vaše podatke. HDInsight Klaster se automatski stvara prilikom izvođenja i izbrisati nakon što to učiniti obradi i neaktivnosti za određeno vrijeme. Koristite vlastitu klaster HDInsight umjesto korištenja programa klaster HDInsight na zahtjev. Detalje potražite u članku [Izračunati povezani servisi](data-factory-compute-linked-services.md) . 

1. U **Pregledniku rješenja**, desnom tipkom miša kliknite **Povezani servisi**, pokažite na **Dodaj**pa kliknite **Nova stavka**.
2. Odaberite **HDInsight na zahtjev povezane servis**, a zatim kliknite **Dodaj**. 
3. Zamijenite **JSON** sljedeće:

        {
          "name": "HDInsightOnDemandLinkedService",
          "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
              "version": "3.2",
              "clusterSize": 1,
              "timeToLive": "00:30:00",
              "linkedServiceName": "AzureStorageLinkedService1"
            }
          }
        }
    
    Sljedeća tablica sadrži opise JSON svojstvima koji se koriste u u isječak:
    
    Svojstvo | Opis
    -------- | -----------
    Verzija | Određuje stvaraju li se treba 3,2 verziju na HDInsight. 
    ClusterSize | Određuje veličinu klaster HDInsight. 
    TimeToLive | Određuje koje vrijeme neaktivnosti za klaster HDInsight prije nego što je izbrisana.
    linkedServiceName | Određuje računa za pohranu koji se koristi za spremanje zapisnika koje generira HDInsight

    Imajte na umu sljedeće: 
    
    - Tvorničke podataka HDInsight klaster **utemeljen na sustavu Windows** za vas stvara s prethodnom JSON. Nije moguće imate ga stvoriti HDInsight klaster **sustavom Linux** . Detalje potražite u članku [Osvježavati HDInsight povezane servisa](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) . 
    - Koristite **vlastitu klaster HDInsight** umjesto korištenja programa klaster HDInsight na zahtjev. Detalje potražite u članku [Povezani servisa HDInsight](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) .
    - HDInsight klaster stvara **zadani spremnika** u spremište blobova platforme koju ste naveli u JSON (**linkedServiceName**). HDInsight izbrisati ovaj spremnik nakon brisanja klaster. Ovo je zadano ponašanje dizajna. HDInsight povezana uslugom osvježavati HDInsight klaster stvorit će se svaki put kada je isječak obrađuje osim ako postoji postojeće uživo klaster (**timeToLive**). Klaster automatski će se izbrisati kada se radi obrade.
    
        Dok se obrađuju više isječaka, vidjet ćete mnogo spremnika u Azure blobova. Ako ne morate ih za otklanjanje poteškoća s zadataka, preporučujemo vam da biste izbrisali im smanjiti resursa za pohranu. Imena tih spremnika uzorcima: "adf**yourdatafactoryname**-**linkedservicename**- datetimestamp". Da biste izbrisali spremnika u vašem blobova platforme Azure pomoću alata kao što je [Microsoft Explorer prostora za pohranu](http://storageexplorer.com/) .

    Detalje potražite u članku [Osvježavati HDInsight povezane servisa](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) . 
4. Spremite datoteku **HDInsightOnDemandLinkedService1.json** .

## <a name="create-datasets"></a>Stvaranje skupova podataka
U ovom ćete koraku stvoriti skupova podataka za predstavljanje unos i poslati podatke za obradu grozd. Ove skupove podataka odnose se na **AzureStorageLinkedService1** koji ste stvorili ranije u ovom praktičnom vodiču. Povezane servisa upućuje na račun za Azure prostora za pohranu i skupova podataka odredite spremnik, mapu, a zatim naziv datoteke u prostora za pohranu koja sadrži unos i poslati podatke.   

#### <a name="create-input-dataset"></a>Stvaranje unosa skup podataka

1. U **Pregledniku rješenja**, desnom tipkom miša kliknite **tablica**, pokažite na **Dodaj**pa kliknite **Nova stavka**. 
2. Na popisu odaberite **Blobova platforme Azure** , promijenite naziv datoteke u **InputDataSet.json**pa kliknite **Dodaj**.
3. Zamijenite **JSON** u uređivaču sljedeće: 

    U isječak JSON stvarate naziva **AzureBlobInput** koji predstavlja ulaznih podataka za neku aktivnost u kanalu skup podataka. Osim toga, navedite ulaznih podataka nalazi u blob spremnika koji se naziva **adfgetstarted** i mapu pod nazivom **inputdata**
        
        {
            "name": "AzureBlobInput",
            "properties": {
                "type": "AzureBlob",
                "linkedServiceName": "AzureStorageLinkedService1",
                "typeProperties": {
                    "fileName": "input.log",
                    "folderPath": "adfgetstarted/inputdata",
                    "format": {
                        "type": "TextFormat",
                        "columnDelimiter": ","
                    }
                },
                "availability": {
                    "frequency": "Month",
                    "interval": 1
                },
                "external": true,
                "policy": {}
            }
        } 

    Sljedeća tablica sadrži opise JSON svojstvima koji se koriste u u isječak:

  	| Svojstvo | Opis |
  	| :------- | :---------- |
  	| Vrsta | Svojstvo vrsta postavljen je na AzureBlob jer se podaci nalaze u spremište blobova platforme Azure. |  
  	| linkedServiceName | odnosi se na AzureStorageLinkedService1 koji ste prethodno stvorili. |
  	| Naziv datoteke | To svojstvo nije obavezno. Ako izostavite to svojstvo, sve datoteke iz na folderPath se izdvajaju. U ovom slučaju odvija se samo input.log. |
  	| Vrsta | Datoteke zapisnika su u tekstnom obliku, pa koristimo TextFormat. | 
  	| columnDelimiter | Stupci u datoteke zapisnika su zarezima znakom zarez () |
  	| Učestalost/interval | Učestalost postavite na mjesec i interval je 1, što znači unos isječke dostupnosti mjesečno. | 
  	| Vanjski | Ovo svojstvo postavljen na vrijednost true ako ulaznih podataka ne generira servis tvorničke podataka. | 
      
    
3. Spremite datoteku **InputDataset.json** . 

 
#### <a name="create-output-dataset"></a>Stvaranje skupa podataka za izlaz
Sada stvorite izlazni skup podataka da biste predstavili podatke pohranjene u spremište blobova platforme Azure. 

1. U **Pregledniku rješenja**, desnom tipkom miša kliknite **tablica**, pokažite na **Dodaj**pa kliknite **Nova stavka**. 
2. Na popisu odaberite **Blobova platforme Azure** , promijenite naziv datoteke u **OutputDataset.json**pa kliknite **Dodaj**. 
3. Zamijenite **JSON** u uređivaču sljedeće: 

    U isječak JSON stvarate pod nazivom **AzureBlobOutput**i određivanje strukturu podataka prvenstveno skripte grozd skup podataka. Osim toga, navedite rezultate pohranjuju u blob spremnika koji se naziva **adfgetstarted** i mapu pod nazivom **partitioneddata**. U odjeljku **dostupnost** određuje da su dataset izlaz proizvodi mjesec.
    
        {
          "name": "AzureBlobOutput",
          "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService1",
            "typeProperties": {
              "folderPath": "adfgetstarted/partitioneddata",
              "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
              }
            },
            "availability": {
              "frequency": "Month",
              "interval": 1
            }
          }
        }

    Odjeljak **Stvaranje unosa skup podataka** potražite u članku za opise svojstava. Ne postavite svojstvo vanjskih na programa izlazni skup podataka kao što je skup podataka koje je stvorio servis tvorničke podataka.

4. Spremite datoteku **OutputDataset.json** .


### <a name="create-pipeline"></a>Stvaranje kanala
U ovom ćete koraku stvoriti na prvom kanal s **HDInsightHive** aktivnosti. Unos isječak dostupna mjesečno (učestalost: mjesec, interval: 1), izlazna isječak mjesečno proizvodi i svojstvo raspored aktivnosti i postavljeno na mjesečno. Postavke za skup podataka izlazne i raspored aktivnosti moraju se podudarati. Trenutno dataset izlaz je što pogoni rasporedu, tako da je izlazni skup podataka morate stvoriti čak i ako se aktivnosti neće proizvesti bilo kakav izlaz. Ako želite da aktivnosti ne svi se unosi, možete preskočiti stvaranje unosa skupu podataka. Svojstva koja se koriste u sljedeće JSON objašnjeni su na kraju ovog odjeljka.

1. U **Pregledniku rješenja**, desnom tipkom miša kliknite **kanali**, pokažite na **Dodaj**pa kliknite **Nova stavka.** 
2. Na popisu odaberite **Vrste Hive transformaciju kanal** , a zatim kliknite **Dodaj**. 
3. Zamijenite **JSON** sljedeće isječka.

    > [AZURE.IMPORTANT] **storageaccountname** zamijenite nazivom vašeg računa za pohranu.

        {
            "name": "MyFirstPipeline",
            "properties": {
                "description": "My first Azure Data Factory pipeline",
                "activities": [
                    {
                        "type": "HDInsightHive",
                        "typeProperties": {
                            "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                            "scriptLinkedService": "AzureStorageLinkedService1",
                            "defines": {
                                "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
                                "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"
                            }
                        },
                        "inputs": [
                            {
                                "name": "AzureBlobInput"
                            }
                        ],
                        "outputs": [
                            {
                                "name": "AzureBlobOutput"
                            }
                        ],
                        "policy": {
                            "concurrency": 1,
                            "retry": 3
                        },
                        "scheduler": {
                            "frequency": "Month",
                            "interval": 1
                        },
                        "name": "RunSampleHiveActivity",
                        "linkedServiceName": "HDInsightOnDemandLinkedService"
                    }
                ],
                "start": "2016-04-01T00:00:00Z",
                "end": "2016-04-02T00:00:00Z",
                "isPaused": false
            }
        }

    U isječak JSON stvarate kanal koji se sastoji od jedne aktivnosti koja koristi grozd u tijeku podataka na programa klaster HDInsight.
    
    U isječak JSON stvarate kanal koji se sastoji od jedne aktivnosti koja koristi grozd u tijeku podataka na programa klaster HDInsight.
    
    Datoteka skripte grozd, **partitionweblogs.hql**pohranjuju se u račun za Azure prostora za pohranu (određen scriptLinkedService, pod nazivom **AzureStorageLinkedService1**) i mapa za **skripte** u spremniku **adfgetstarted**.

    U odjeljku **definira** se koristi za određivanje runtime postavke koje prosljeđuju skripte grozd kao grozd konfiguracijskih vrijednosti (npr ${hiveconf: inputtable}, ${hiveconf:partitionedtable}).

    **Početak** i **Kraj** svojstva kanal određuje aktivni razdoblje kanal.

    U aktivnosti JSON, navedite skripte grozd pokreće se na računalnim određen u **linkedServiceName** – **HDInsightOnDemandLinkedService**.

    > [AZURE.NOTE] Detalje o svojstvima JSON koji se koriste u primjeru potražite u članku [Anatomy na kanal](data-factory-create-pipelines.md#anatomy-of-a-pipeline) . 
3. Spremite datoteku **HiveActivity1.json** .

### <a name="add-partitionweblogshql-and-inputlog-as-a-dependency"></a>Dodavanje partitionweblogs.hql i input.log kao ovisnosti 

1. Desnom tipkom miša kliknite **ovisnosti** u prozoru **Preglednik rješenja** , pokažite na **Dodaj**pa kliknite **Postojeću stavku**.  
2. Dođite do **C:\ADFGettingStarted** odaberite **partitionweblogs.hql**, **input.log** datoteke i kliknite **Dodaj**. Ove dvije datoteke kao dio preduvjeti ste stvorili iz [Pregled vodiča](data-factory-build-your-first-pipeline.md).

Kad objavite rješenje u sljedećem koraku, mapu skripte u spremniku blob **adfgetstarted** će se prenese datoteka **partitionweblogs.hql** .   

### <a name="publishdeploy-data-factory-entities"></a>Objavljivanje i implementacija entiteti tvorničke podataka

18. Desnom tipkom miša kliknite projekt u pregledniku rješenja, a zatim kliknite **Objavi**. 
19. Ako se prikaže dijaloški okvir **prijavite se na Microsoftov račun** , unesite vjerodajnice za račun koji ima Azure pretplatu, a zatim kliknite **Prijava**.
20. Trebali biste vidjeti sljedeći dijaloški okvir:

    ![Dijaloški okvir objavljivanje](./media/data-factory-build-your-first-pipeline-using-vs/publish.png)

21. Na stranici Konfiguracija podataka tvorničke, učinite sljedeće: 
    1. Odaberite mogućnost **Stvori novu tvorničke podataka** .
    2. Unesite jedinstveni **naziv** tvorničke podataka. Na primjer: **FirstDataFactoryUsingVS09152016**. Naziv mora biti globalno jedinstveni.  
    
    
        > [AZURE.IMPORTANT] Ako vam se prikaže pogreška **je naziv tvorničke podataka "FirstDataFactoryUsingVS" nije dostupno** za objavljivanje, promijenite naziv (na primjer, yournameFirstDataFactoryUsingVS). Pogledajte temu [Tvorničke podataka – pravila imenovanja](data-factory-naming-rules.md) za imenovanja pravila za artefakte tvorničke podataka.
3. Odaberite desno pretplatu za polje **pretplate** .
     
     
        > [AZURE.IMPORTANT] Ako ne vidite sve pretplate, provjerite je li prijavljeni pomoću računa koji je administrator ili ko administrator pretplate.  
        
    4. Odaberite **grupu resursa** za tvorničke podataka će biti stvoren. 
    5. Odaberite **područje** za tvorničke podataka. 
    6. Kliknite **Dalje** da biste prešli na stranicu za **Objavljivanje stavki** . (Pritisnite **TAB** da biste premjestili iz polja Naziv da biste je gumb **Dalje** onemogućen). 
23. Na stranici **Objavljivanje stavki** Pobrinite se da sve na podataka Factories entiteti odabrane pa kliknite **Dalje** da biste prešli na stranicu **Sažetak** .     
24. Pregledajte sažetak i kliknite **Dalje** da biste započeli postupak implementacije i prikaz **Statusa implementacije**.
25. Na stranici **Stanje implementacije** trebali biste vidjeti status postupka implementacije. Po završetku implementaciju, kliknite Završi. 

 
Važne stavke na umu: 

- Ako vam se prikaže pogreška: "**Da biste koristili polje naziva Microsoft.DataFactory nije registriran ove pretplate**", učinite nešto od sljedećeg i pokušajte ponovno objaviti: 

    - U Azure PowerShell pokrenite sljedeću naredbu da biste registrirali davatelja tvorničke podataka. 
        
            Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    
        Možete pokrenite sljedeću naredbu da biste potvrdili koji tvorničke podataka koji je registriran davatelja usluga. 
    
            Get-AzureRmResourceProvider
    - Prijavite se pomoću Azure pretplate u [Azure portal](https://portal.azure.com) i idite na tvorničke podataka plohu (ili) stvaranje podataka tvorničke na portalu za Azure. Ova akcija automatski registrira davatelj usluga za vas.
-   Naziv tvorničke podataka možda registrirana kao naziv DNS-a u budućnosti i zato postaju javno vidljivi.
-   Da biste stvorili instance tvorničke podataka, morate biti administrator ili ko administratori Azure pretplate

 
## <a name="monitor-pipeline"></a>Monitor kanal

### <a name="monitor-pipeline-using-diagram-view"></a>Kanal monitora pomoću prikaza dijagrama
6. Prijavite se na [portal za Azure](https://portal.azure.com/), učinite sljedeće:
    1. Kliknite **Dodatne servise** , a zatim kliknite **factories podataka**.
        ![Pregled factories podataka](./media/data-factory-build-your-first-pipeline-using-vs/browse-datafactories.png) 
    2. Odaberite ime na tvorničke podataka (na primjer: **FirstDataFactoryUsingVS09152016**) na popisu factories podataka. 
        ![Odaberite na tvorničke podataka](./media/data-factory-build-your-first-pipeline-using-vs/select-first-data-factory.png)
7. Na početnoj stranici za vaše podatke tvorničke kliknite **dijagrama**.
  
    ![Pločica dijagrama](./media/data-factory-build-your-first-pipeline-using-vs/diagram-tile.png)
7. U prikazu dijagrama vidjeli pregled kanali i skupova podataka koristi ovog praktičnog vodiča.
    
    ![Prikaz dijagrama](./media/data-factory-build-your-first-pipeline-using-vs/diagram-view-2.png) 
8. Da biste pogledali sve aktivnosti u kanalu, desnom tipkom miša kliknite kanal u dijagramu, a zatim kliknite Otvori kanal. 

    ![Izbornik za otvaranje kanal](./media/data-factory-build-your-first-pipeline-using-vs/open-pipeline-menu.png)
9. Provjerite nalaze li se HDInsightHive aktivnosti u kanalu. 
  
    ![Prikaz Otvori kanal](./media/data-factory-build-your-first-pipeline-using-vs/open-pipeline-view.png)

    Da biste prešli na prethodni prikaz, kliknite **tvorničke podataka** u izbornik za hijerarhijsku navigaciju na vrhu. 
10. U **Prikazu dijagrama**, dvokliknite dataset **AzureBlobInput**. Provjerite je li u stanju **spremno** isječak. Može potrajati nekoliko minuta za isječkom koja će se prikazivati u stanju spremno. Ako se ne događa kada se tijekom čekanja za, potražite u članku ako imate Ulazna datoteka (input.log) u desnom spremnik (adfgetstarted) i mapa (inputdata).

    ![Unos isječak u stanju spremno](./media/data-factory-build-your-first-pipeline-using-vs/input-slice-ready.png)
11. Kliknite **X** da biste zatvorili plohu **AzureBlobInput** . 
12. U **Prikazu dijagrama**, dvokliknite dataset **AzureBlobOutput**. Koji se prikaže isječak koji se trenutno obrađuju.

    ![Skup podataka](./media/data-factory-build-your-first-pipeline-using-vs/dataset-blade.png)
9. Kada završi obrada, vidjet ćete isječak u stanju **spremno** .

    > [AZURE.IMPORTANT] Stvaranje programa klaster HDInsight osvježavati obično traje tijekom (otprilike 20 minuta). Stoga očekivati kanal za **približno 30 minuta** za obradu isječak.  

    ![Skup podataka](./media/data-factory-build-your-first-pipeline-using-vs/dataset-slice-ready.png) 
    
10. Kada je isječak u stanju **spremno** , provjerite mapu **partitioneddata** u spremniku **adfgetstarted** u spremištu blobova za izlazne podatke.  
 
    ![izlazne podatke](./media/data-factory-build-your-first-pipeline-using-vs/three-ouptut-files.png)
11. Kliknite isječak da biste vidjeli detalje o njoj u **isječak podataka** plohu.

    ![Pojedinosti o podacima isječkom](./media/data-factory-build-your-first-pipeline-using-vs/data-slice-details.png)  
12. Kliknite aktivnost pokrenuti **popis pokreće aktivnosti** da biste vidjeli detalje o aktivnosti pokrenuti (grozd aktivnosti u našem scenariju) u prozoru za razmjenu **aktivnosti pokrenuti pojedinosti** .   
    ![Aktivnosti pokrenuti pojedinosti](./media/data-factory-build-your-first-pipeline-using-vs/activity-window-blade.png)  
    
    Iz datoteka zapisnika, vidjet ćete grozd upit koji izvršen i informacije o stanju. Ti zapisnici korisne su za otklanjanje poteškoća.  
 

Potražite u članku [Monitor skupova podataka i kanal](data-factory-monitor-manage-pipelines.md) za upute za praćenje kanal i skupova podataka pomoću portala za Azure ste stvorili pomoću ovog praktičnog vodiča.

### <a name="monitor-pipeline-using-monitor--manage-app"></a>Praćenje kanal pomoću nadzor i upravljanje aplikacije
Također možete koristiti monitora i upravljanje njima aplikaciju za praćenje na kanali. Detaljne informacije o korištenju ove aplikacije potražite u članku [nadzor i upravljanje kanali tvorničke Azure podataka pomoću nadzor i upravljanje aplikacija](data-factory-monitor-manage-app.md).

1. Kliknite nadzor i upravljanje pločica.

    ![Praćenje i upravljanje njima pločica](./media/data-factory-build-your-first-pipeline-using-vs/monitor-and-manage-tile.png) 
2. Trebali biste vidjeti monitora i upravljanje njima aplikacije. Promjena **vrijeme početka** i **vrijeme završetka** tako da odgovara start (04-01-2016 12.00: 00 Prijepodne) i kraja snimke (04-02-2016 12.00: 00 Prijepodne) kanala, a zatim kliknite **Primijeni**.

    ![Praćenje i upravljanje njima aplikacije](./media/data-factory-build-your-first-pipeline-using-vs/monitor-and-manage-app.png) 
3. Prozor programa aktivnosti odaberite na popisu aktivnosti Windows da biste vidjeli detalje o njoj. 
    ![Detalji o aktivnosti prozora](./media/data-factory-build-your-first-pipeline-using-vs/activity-window-details.png)


> [AZURE.IMPORTANT] Kada je isječak uspješno obrađuju se briše ulazne datoteke. Dakle, ako želite ponovno pokrenite isječak ili ponovno učinite vodič, prijenos unos datoteke (input.log) u mapu inputdata spremnika adfgetstarted.
 

## <a name="use-server-explorer-to-view-data-factories"></a>Korištenje Eksplorera za poslužitelj da biste pogledali factories podataka

1. U **Visual Studio**, na izborniku kliknite **Prikaz** pa kliknite **Explorer poslužitelja**.
2. U prozoru programa Explorer poslužitelja proširite **Azure** a **Tvorničke podataka**. Ako vidite **prijavite se u Visual Studio**, unesite **račun** povezan s pretplatom Azure i kliknite **Nastavi**. Unesite **lozinku**i kliknite **Prijava**. Visual Studio pokušava pronaći informacije o svim factories Azure podataka u svoju pretplatu. Prikaz statusa ovaj postupak u prozoru **Popis zadataka tvorničke podataka** .

    ![Poslužitelj programa Explorer](./media/data-factory-build-your-first-pipeline-using-vs/server-explorer.png)
3. Desnom tipkom miša kliknite podataka tvorničke i odaberite **Izvoz podataka tvorničke novi projekt** da biste stvorili Visual Studio projektu na temelju postojeće tvorničke za podatke.

    ![Izvoz podataka tvorničke](./media/data-factory-build-your-first-pipeline-using-vs/export-data-factory-menu.png)

## <a name="update-data-factory-tools-for-visual-studio"></a>Ažuriranje podataka tvorničke Alati za Visual Studio

Da biste ažurirali Azure podataka tvorničke Alati za Visual Studio, učinite sljedeće:

1. Na izborniku kliknite **Alati** , a zatim odaberite **proširenja i ažuriranja**.
2. Odaberite **ažuriranja** u lijevom oknu, a zatim odaberite **Visual Studio galerije**.
3. Odaberite **Alati tvorničke Azure podataka za Visual Studio** , a zatim kliknite **Ažuriraj**. Ako ne vidite tu stavku, već imate najnoviju verziju alata. 

## <a name="use-configuration-files"></a>Korištenje datoteka konfiguracije
Konfiguracija datoteka u Visual Studio možete koristiti da biste konfigurirali svojstva za povezani servisi/tablica/kanali drugačije za svako okruženje. 

Razmotrite sljedeće definiciju JSON servisa za pohranu Azure povezani. Da biste odredili **connectionString** imaju različite vrijednosti za accountname i accountkey ovise o okruženju (Test/razvojni/Proizvodnja) na kojoj su implementacija entiteti tvorničke podataka. Takvo ponašanje možete postići pomoću zasebnom konfiguracijska datoteka za svako okruženje. 

    {
        "name": "StorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "description": "",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        }
    } 

### <a name="add-a-configuration-file"></a>Dodavanje konfiguracijska datoteka
Dodajte konfiguracijska datoteka za svako okruženje pomoću sljedećih koraka:   

1. Desnom tipkom miša kliknite projekt tvorničke podataka u Visual Studio rješenje, pokažite na **Dodaj**pa kliknite **Nova stavka**.
2. Odaberite **Config** na popisu instaliranih predložaka na lijevoj strani, odaberite **Konfiguracijskoj datoteci**, unesite **naziv** za datoteku konfiguracije pa kliknite **Dodaj**.

    ![Dodavanje konfiguracijska datoteka](./media/data-factory-build-your-first-pipeline-using-vs/add-config-file.png)
3. Dodajte parametre konfiguraciju i njihovih vrijednosti u sljedećem obliku.

        {
            "$schema": "http://datafactories.schema.management.azure.com/vsschemas/V1/Microsoft.DataFactory.Config.json",
            "AzureStorageLinkedService1": [
                {
                    "name": "$.properties.typeProperties.connectionString",
                    "value": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
                }
            ],
            "AzureSqlLinkedService1": [
                {
                    "name": "$.properties.typeProperties.connectionString",
                    "value":  "Server=tcp:spsqlserver.database.windows.net,1433;Database=spsqldb;User ID=spelluru;Password=Sowmya123;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
                }
            ]
        }

    U ovom se primjeru konfigurira svojstvo connectionString na servis za pohranu Azure povezani i na servis SQL Azure povezani. Obratite pozornost na to da je vidjet ćete da sintaksa za određivanje naziv [JsonPath](http://goessner.net/articles/JsonPath/).   

    Ako je svojstvo JSON koji sadrži polje s vrijednostima kao što je prikazano u sljedećem kodu:  

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
    
    Konfiguriranje svojstava kao što je prikazano u sljedećim konfiguracijskoj datoteci (koristi polje indeksiranja): 
        
        {
            "name": "$.properties.structure[0].name",
            "value": "FirstName"
        }
        {
            "name": "$.properties.structure[0].type",
            "value": "String"
        }
        {
            "name": "$.properties.structure[1].name",
            "value": "LastName"
        }
        {
            "name": "$.properties.structure[1].type",
            "value": "String"
        }

### <a name="property-names-with-spaces"></a>Nazivi svojstava s razmacima
Naziv svojstva sadrži razmake, kao što je prikazano u sljedećem primjeru (naziv poslužitelja baze podataka) koristite uglatim zagradama: 

     {
         "name": "$.properties.activities[1].typeProperties.webServiceParameters.['Database server name']",
         "value": "MyAsqlServer.database.windows.net"
     }


### <a name="deploy-solution-using-a-configuration"></a>Implementacija rješenja pomoću konfiguraciju
Kada objavljujete Azure podataka tvorničke entiteti Dodavanje veze za VANJSKIH, možete odrediti konfiguracije koji želite koristiti za objavljivanje postupak. 

Da biste objavili entiteti u projektu tvorničke Azure podataka pomoću konfiguracijska datoteka:   

1. Desnom tipkom miša kliknite projekt tvorničke podataka, a zatim kliknite **Objavi** da biste vidjeli dijaloški okvir **Objavljivanje stavki** . 
2. Odaberite postojeći tvorničke za podatke ili odredite vrijednosti za stvaranje na tvorničke podataka na stranici **Konfiguracija podataka tvorničke** pa kliknite **Dalje**.   
3. Na stranici **Objavljivanje stavki** : Pogledajte padajućeg popisa s konfiguracijama dostupnih polja **Odaberite Config implementacije** .

    ![Odaberite konfiguracijska datoteka](./media/data-factory-build-your-first-pipeline-using-vs/select-config-file.png)

4. Odaberite **konfiguracijska datoteka** koje želite koristiti, a zatim kliknite **Dalje**. 
5. Provjerite prikazuje naziv datoteke JSON na stranici **Sažetak** i kliknite **Dalje**. 
6. Nakon dovršetka postupka implementacije, kliknite **Završi** . 

Ako pokrenete, vrijednosti iz konfiguracijskoj datoteci koriste se za postavljanje vrijednosti za svojstva u datotekama JSON za entiteti tvorničke podataka prije nego što su entiteti implementiran na tvorničke Azure podataka servisa.   

## <a name="summary"></a>Sažetak 
U ovom ćete praktičnom vodiču ste stvorili na tvorničke Azure podataka s podacima postupak tako da pokrenete grozd skripte na klaster hadoop HDInsight. Koristi tvorničke uređivač podataka na portalu za Azure poduzeti sljedeće korake:  

1.  Stvara Azure **tvorničke podataka**.
2.  Stvara dva **povezani servisi**:
    1.  **Prostor za pohranu Azure** povezana servisa za povezivanje vašeg blobova platforme Azure koja sadrži datoteke ulaza i izlaza na tvorničke podataka.
    2.  Servis povezane **Azure HDInsight** na zahtjev za povezivanje programa klaster HDInsight Hadoop osvježavati tvorničke podataka. Azure tvorničke podataka stvara HDInsight Hadoop klaster samo-na-jedan za obradu ulaznih podataka i dobivanje izlazne podatke. 
3.  Stvara dvaju **skupova podataka**, koji opisuje ulazni i izlazni podaci za HDInsight grozd aktivnosti u kanalu. 
4.  Da biste stvorili **kanal** s **HDInsight grozd** aktivnosti.  


## <a name="next-steps"></a>Daljnji koraci
U ovom se članku ste stvorili na kanal s transformaciju aktivnosti (HDInsight aktivnosti) koja se izvršava grozd skripte na programa klaster HDInsight na zahtjev. Da biste vidjeli kako koristiti kopiju aktivnosti da biste kopirali podatke iz programa blobova platforme Azure Azure SQL, potražite u članku [Praktični vodič: kopiranje podataka iz programa Azure bloba za Azure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
  
## <a name="see-also"></a>Vidi također
| Tema | Opis |
| :---- | :---- |
| [Aktivnosti transformacije podataka](data-factory-data-transformation-activities.md) | Ovaj članak sadrži popis aktivnosti transformacije podataka (primjerice HDInsight grozd transformaciju ste koristili u ovom ćete praktičnom vodiču) podržava tvorničke Azure podataka. | 
| [Planiranje i izvođenja](data-factory-scheduling-and-execution.md) | U ovom se članku objašnjava zakazivanja i izvođenja aspekte Azure podataka tvorničke modelu. |
| [Kanali](data-factory-create-pipelines.md) | U ovom se članku olakšava razumijevanje kanali i aktivnosti u tvorničke Azure podataka i kako ih koristiti za sastavljanje završetka do kraja utemeljenih na podacima tijekova rada za scenarija ili tvrtke. |
| [Skupove podataka](data-factory-create-datasets.md) | U ovom se članku olakšava razumijevanje skupova podataka na tvorničke Azure podataka.
| [Nadzor i upravljanje kanali pomoću aplikacije za praćenje](data-factory-monitor-manage-app.md) | U ovom se članku opisuje kako nadzor, upravljanje i kanali pomoću nadzor i upravljanje aplikacija za ispravljanje pogrešaka. 
