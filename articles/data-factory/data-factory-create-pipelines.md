<properties 
    pageTitle="Stvaranje i raspored kanali, lanac aktivnosti u tvorničke podataka | Microsoft Azure" 
    description="Naučite kako stvoriti podataka kanala u Azure tvorničke podataka za premještanje i pretvaranje podataka. Stvaranje tijeka rada podataka utemeljenima na čime se dobiva spreman za korištenje informacija o." 
    keywords="Podaci kanal, podataka utemeljenima na tijeka rada"
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article"
    ms.date="09/12/2016" 
    ms.author="shlo"/>

# <a name="pipelines-and-activities-in-azure-data-factory"></a>Kanali i aktivnosti u tvorničke Azure podataka
U ovom se članku olakšava razumijevanje kanali i aktivnosti u tvorničke Azure podatke i koristiti ih da biste izradili završetka do kraja utemeljenih na podacima tijekova rada za premještanje podataka i scenariji za obradu podataka.  

> [AZURE.NOTE] U ovom se članku pretpostavlja da ste nestaje kroz [Uvod u tvorničke Azure podataka](data-factory-introduction.md). Ako nemate hands-on-ovi sa stvaranjem podataka želite factories, prolaze kroz [Sastavljanje vaš prvi tvorničke podataka](data-factory-build-your-first-pipeline.md) vodič pomoći razumjeti bolje ovog članka.  

## <a name="what-is-a-data-pipeline"></a>Što je podataka kanal?
**Kanal** je razine grupiranja logično povezane **aktivnosti**. Koristi se za grupu aktivnosti u jedinice koja se izvodi zadatak. Da biste bolje razumjeli kanali, morate najprije razumjeti aktivnost. 

## <a name="what-is-an-activity"></a>Što je aktivnosti?
Aktivnosti definiraju akcije da biste izvršili nad podacima. Svakoj aktivnosti uzima nula ili više [skupova podataka](data-factory-create-datasets.md) kao unosa i daje jednu ili više skupova podataka kao rezultat. 

Na primjer, možda pomoću Kopiraj aktivnosti i orkestrirali kopiranje podataka iz trgovine jedan podatkovni drugi spremišta podataka. Slično tome, možete koristiti HDInsight grozd aktivnosti da biste pokrenuli upit grozd na programa Azure HDInsight klaster za pretvaranje podataka. Azure tvorničke podacima nudi širok raspon [transformaciju podataka](data-factory-data-transformation-activities.md)te aktivnosti [Premještanje podataka](data-factory-data-movement-activities.md) . Također možete odabrati da biste stvorili prilagođene aktivnosti .NET da biste pokrenuli vlastitog koda. 

## <a name="sample-copy-pipeline"></a>Ogledna Kopiraj kanal
U sljedeće ogledne kanal, postoji jedan aktivnosti vrste **Kopiraj** u odjeljku **aktivnosti** . U ovom primjeru [kopirajte aktivnosti](data-factory-data-movement-activities.md) kopira podatke iz Azure blobova s bazom podataka Azure SQL. 

    {
      "name": "CopyPipeline",
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

Objašnjenje stvaranja ovog kanala potražite u članku [Praktični vodič: kopiranje podataka iz spremišta blobova s bazom podataka SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). 

## <a name="sample-transformation-pipeline"></a>Ogledna transformaciju kanal
U kanal sljedeće ogledne postoji jedan aktivnosti vrste **HDInsightHive** u odjeljku **aktivnosti** . U ovom primjeru [HDInsight grozd aktivnosti](data-factory-hive-activity.md) pretvorbe podataka iz Azure blobova tako da pokrenete datoteku skripte grozd na programa Azure HDInsight Hadoop klaster. 

    {
        "name": "TransformPipeline",
        "properties": {
            "description": "My first Azure Data Factory pipeline",
            "activities": [
                {
                    "type": "HDInsightHive",
                    "typeProperties": {
                        "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                        "scriptLinkedService": "AzureStorageLinkedService",
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

Imajte na umu sljedeće točke: 

- U odjeljku aktivnosti postoji samo jedan aktivnosti čije **Vrsta** postavljena na **HDInsightHive**.
- Datoteka skripte grozd, **partitionweblogs.hql**pohranjuju se u račun za Azure prostora za pohranu (određen scriptLinkedService, pod nazivom **AzureStorageLinkedService**) i mapa za **skripte** u spremniku **adfgetstarted**.
- U odjeljku **definira** se koristi za određivanje runtime postavke koje prosljeđuju skripte grozd kao grozd konfiguracijskih vrijednosti (npr ${hiveconf: inputtable}, ${hiveconf:partitionedtable}).

Objašnjenje stvaranja ovog kanala potražite u članku [Praktični vodič: Stvaranje vaš prvi kanal postupak podatke pomoću Hadoop klaster](data-factory-build-your-first-pipeline.md). 

## <a name="chaining-activities"></a>Ulančavanje aktivnosti
Ako imate više aktivnosti u na kanalu i izlaz iz aktivnost nije ulazni druge aktivnosti, aktivnosti pokrenuti paralelno ako spremni aktivnosti da biste ulaznih podataka. 

Možete povezati u lanac dvije aktivnosti tako da vas izlazni skup podataka jedan aktivnosti kao unos dataset druge aktivnosti. Aktivnosti može biti u istoj kanalu ili u drugu kanali. Drugi aktivnosti izvršava samo kada prvoga dovršava uspješno. 

Na primjer, razmotrite sljedeće slučaj:
 
1.  Kanal P1 ima A1 aktivnosti koje zahtijeva vanjski unos dataset D1 te proizvesti **Izlaz** dataset **D2**.
2.  Kanal P2 ima A2 aktivnosti koji zahtijeva **unos** iz skupa podataka **D2**i daje izlaz dataset D3.
 
U ovom scenariju aktivnost A1 pokreće kad vanjskih podataka dostupna, a zbirka dostigne učestalost zakazanog dostupnost.  Aktivnost A2 pokreće kad zakazani isječke iz D2 postaju dostupne i zbirka dostigne učestalost zakazanog dostupnost. Ako je došlo do pogreške na jedan od isječaka u skupu podataka D2, A2 neće pokrenuti za suprotnu dok ne postane dostupna.

Prikaz dijagrama:

![Ulančavanje aktivnosti u dva kanali](./media/data-factory-create-pipelines/chaining-two-pipelines.png)

Prikaz dijagrama s obje aktivnosti u istom kanal: 

![Ulančavanje aktivnosti u istom kanalu](./media/data-factory-create-pipelines/chaining-one-pipeline.png)

Dodatne informacije potražite u članku [Planiranje i izvršavanja](#chaining-activities). 

## <a name="scheduling-and-execution"></a>Planiranje i izvođenja
Dosad ste prepoznata što kanali i aktivnosti su. Ste pokušavali i na način da su definirani i više razine prikaz aktivnosti u tvorničke Azure podataka. Sada nam pogledajte kako se izvršava. 

Na kanal je aktivno samo između njegov vrijeme početka i vrijeme završetka. To se izvršava prije vremena početka ili nakon vrijeme završetka. Ako je pauziran kanal, ona ne izvode bez obzira njegovo vrijeme početka i završetka. Kanal za pokretanje, ona mora moguće pauzirati. Zapravo nije kanal koja se izvršava. To je aktivnosti u kanalu koja se izvršava. No oni učiniti u kontekstu cjelokupan kanal. 

Potražite u članku [Zakazivanje sastanka te izvođenja](data-factory-scheduling-and-execution.md) shvatiti način funkcioniranja zakazivanja i izvršavanja u tvorničke Azure podataka.

## <a name="create-pipelines"></a>Stvaranje kanali
Azure tvorničke podataka nudi razne mehanizme za stvaranje i implementaciju kanali (koji sadrže jedan ili više aktivnosti u njoj). 

### <a name="using-azure-portal"></a>Pomoću portala za Azure
Uređivač tvorničke podataka na portalu za Azure možete koristiti da biste stvorili na kanal. Potražite u članku [Početak rada s tvorničke Azure podataka (Uređivač tvorničke podataka)](data-factory-build-your-first-pipeline-using-editor.md) do kraja do kraja vodič. 

### <a name="using-visual-studio"></a>Korištenje Visual Studio 
Koristite Visual Studio za stvaranje i implementacija kanali tvorničke Azure podataka. Potražite u članku [Početak rada s tvorničke Azure podataka (Visual Studio)](data-factory-build-your-first-pipeline-using-vs.md) do kraja do kraja vodič. 

### <a name="using-azure-powershell"></a>Pomoću Azure komponente PowerShell
Azure PowerShell možete koristiti za stvaranje kanali na tvorničke Azure podataka. Recimo, koje ste definirali kanal JSON u datoteku na c:\DPWikisample.json. Možete prenijeti ga na tvorničke podataka Azure instanci kao što je prikazano u sljedećem primjeru:

    New-AzureRmDataFactoryPipeline -ResourceGroupName ADF -Name DPWikisample -DataFactoryName wikiADF -File c:\DPWikisample.json

Potražite u članku [Početak rada s tvorničke Azure podataka (Azure PowerShell)](data-factory-build-your-first-pipeline-using-powershell.md) do završetka do kraja vodič za stvaranje podataka tvorničke s na kanal. 

### <a name="using-net-sdk"></a>Pomoću .NET SDK-a
Možete stvoriti i implementirati kanal putem .NET SDK previše. U ovom mehanizam se može koristiti za stvaranje kanali programski. Dodatne informacije potražite u članku [Stvaranje, upravljanje, i programski nadzirati factories podataka](data-factory-create-data-factories-programmatically.md). 


### <a name="using-azure-resource-manager-template"></a>Pomoću predloška Azure Voditelj resursa
Možete stvoriti i implementirati kanal pomoću predloška Azure Voditelj resursa. Dodatne informacije potražite u članku [Početak rada s tvorničke Azure podataka (Voditelj resursa za Azure)](data-factory-build-your-first-pipeline-using-arm.md). 

### <a name="using-rest-api"></a>Pomoću REST API-JA
Možete stvoriti i implementirati kanala previše pomoću REST API-ji. U ovom mehanizam se može koristiti za stvaranje kanali programski. Dodatne informacije potražite u članku [Stvaranje ili ažuriranje na kanal](https://msdn.microsoft.com/library/azure/dn906741.aspx). 


## <a name="monitor-and-manage-pipelines"></a>Nadzor i upravljanje kanali  
Kada je implementiran na kanal, možete upravljati i praćenje vaše kanali isječke te se pokreće. Saznajte više o je ovdje: [nadzor i upravljanje kanali](data-factory-monitor-manage-pipelines.md).


## <a name="pipeline-json"></a>Kanal JSON   
Javite nam potrebno detaljnije na način definiran na kanal u JSON OSNOVNI oblik. Generički strukture na kanal izgleda na sljedeći način:

    {
        "name": "PipelineName",
        "properties": 
        {
            "description" : "pipeline description",
            "activities":
            [
    
            ],
            "start": "<start date-time>",
            "end": "<end date-time>"
        }
    }

U odjeljku **aktivnosti** može imati jednu ili više aktivnosti u njemu definirana. Svakoj aktivnosti ima sljedeću strukturu najviše razine:

    {
        "name": "ActivityName",
        "description": "description", 
        "type": "<ActivityType>",
        "inputs":  "[]",
        "outputs":  "[]",
        "linkedServiceName": "MyLinkedService",
        "typeProperties":
        {
    
        },
        "policy":
        {
        }
        "scheduler":
        {
        }
    }

Sljedeći tablici opisuju svojstva unutar JSON definicije aktivnosti i kanal:

Oznaka | Opis | Obavezno
--- | ----------- | --------
ime | Naziv web-mjesto aktivnosti ili u okvir za kanal. Navedite naziv koji predstavlja radnju je konfiguriran za aktivnosti ili kanala<br/><ul><li>Najveći broj znakova: 260</li><li>Morate pokrenuti s broj obavijesti ili podvlaka (_)</li><li>Sljedeći znakovi nisu dopušteni: ".", "+", "?", "/", "<",">", "*", "%", "i", ":","\\"</li></ul> | Da
Opis | Tekst koji opisuje što aktivnosti ili kanal koristi | Da
Vrsta | Navodi vrstu aktivnosti. Potražite u člancima [Aktivnosti premještanje podataka](data-factory-data-movement-activities.md) i [Aktivnosti transformacije podataka](data-factory-data-transformation-activities.md) za različite vrste aktivnosti. | Da
unosa | Unos tablice koristi aktivnost<br/><br/>jedan unos tablice<br/>"unosa": [{"naziv": "inputtable1"}],<br/><br/>dvije tablice za unos <br/>"unosa": [{"naziv": "inputtable1"}, {"naziv": "inputtable2"}], | Da
izlaze | Izlazna tablica koristi activity.// jednu izlaznu tablicu<br/>"Proizvodi": [{"naziv": "outputtable1"}],<br/><br/>dvije tablice Izlaz<br/>"Proizvodi": [{"naziv": "outputtable1"}, {"naziv": "outputtable2"}], | Da
linkedServiceName | Naziv povezane usluge koristi aktivnost. <br/><br/>Aktivnost možda potrebno je navesti povezane servisa koja je povezana s potrebna računalnim okruženje. | Da za HDInsight aktivnosti i Azure strojnog učenja grupe za bilježenje rezultata aktivnosti <br/><br/>Za sve ostale
typeProperties | Svojstva u odjeljku typeProperties ovise o vrsti aktivnosti. | ne
pravila | Pravila koja utječu na ponašanje izvođenju aktivnosti. Ako nisu navedeni, koriste se zadane pravilnike. | ne
pokretanje | Početni datum-vrijeme za kanal. Mora biti u [obliku ISO](http://en.wikipedia.org/wiki/ISO_8601). Na primjer: 2014.-10-14T16:32:41Z. <br/><br/>Nije moguće navesti lokalno vrijeme, na primjer neko vrijeme po istočnom Vremenu. Evo jednog primjera: "2016-02-27T06:00:00**-05: 00**", koja je Predviđeni AM 6<br/><br/>Svojstva početka i završetka zajedno određuju aktivni razdoblje, za kanal. Izlaz isječke samo Proizvodi s u aktivni razdoblja. | ne<br/><br/>Ako navedete vrijednost za svojstvo završi, morate navesti vrijednost za svojstvo start.<br/><br/>Vrijeme početka i završetka i može prazan da biste stvorili na kanal. Morate navesti obje se vrijednosti da biste postavili razdoblje aktivni kanal za pokretanje. Ako ne navedete vremena početka i završetka prilikom stvaranja na kanal, možete postaviti pomoću cmdleta skup AzureRmDataFactoryPipelineActivePeriod kasnije.
END | Datum-vrijeme završetka za kanal. Ako je naveden mora biti u obliku ISO. Na primjer: 2014.-10-14T17:32:41Z <br/><br/>Nije moguće navesti lokalno vrijeme, na primjer neko vrijeme po istočnom Vremenu. Evo jednog primjera: "2016-02-27T06:00:00**-05: 00**", koja je Predviđeni AM 6<br/><br/>Da biste pokrenuli beskonačno kanal, navedite 9999 09 09 vrijednosti za svojstvo Završi. | ne <br/><br/>Ako navedete vrijednost za svojstvo početka, morate navesti vrijednost za svojstvo Završi.<br/><br/>Pogledajte bilješke za svojstvo **start** .
isPaused | Ako je postavljen na true kanal pokrenuti. Zadana vrijednost = false. Ovo svojstvo možete koristiti da biste omogućili ili onemogućili. | ne 
Raspored | Svojstvo "raspored" se koristi za određivanje željenog Pomoćnik za aktivnost. Njegov podsvojstva su jednaki onima u [svojstvo dostupnost u skup podataka](data-factory-create-datasets.md#Availability). | ne |   
| pipelineMode | Način za planiranje pokretanja za kanal. Dopuštene su vrijednosti: zakazano (zadano), onetime.<br/><br/>"Zakazano" označava kanal pokreće se u određenom vremenskom intervalu prema njegov aktivni razdoblja (vrijeme početka i završetka). "Onetime" označava kanal izvodi samo jednom. Onetime kanali jednom stvorili nije moguće mijenjati/ažurirati trenutno. Detalje o postavljanju onetime potražite u članku [Onetime kanal](data-factory-scheduling-and-execution.md#onetime-pipeline) . | ne | 
| expirationTime | Trajanje vremena nakon stvaranja za koje kanal vrijedi i ostaju Dodjela resursa. Ako ga nemate sve aktivne nije uspjelo ili čeka se pokreće, kanal automatski brišu kad dođe vrijeme isteka. | ne | 
| skupove podataka | Popis skupova podataka koriste aktivnosti koji su definirani u kanalu. Ovo svojstvo se može koristiti da biste definirali skupove podataka koji su specifične za ovaj kanal i nije definirano unutar tvorničke podataka. Skupove podataka definirane unutar ovaj kanal može se koristiti samo ovaj kanal, a nije moguće zajednički koristiti. Detalje potražite u članku [iz djelokruga skupova podataka](data-factory-create-datasets.md#scoped-datasets) .| ne |  
 

### <a name="policies"></a>Pravila
Pravila utječu na ponašanje izvođenju aktivnosti, osobito pri obrađuje isječak tablice. Sljedeća tablica sadrži detalje.

Svojstvo | Dopušteno vrijednosti | Zadana vrijednost | Opis
-------- | ----------- | -------------- | ---------------
istodobnosti | Cijeli broj <br/><br/>Maksimalna vrijednost: 10 | 1 | Broj Istodobni izvršavanja aktivnosti.<br/><br/>Određuje broj izvršavanja paralelno aktivnosti koje se može dogoditi na različitim isječke. Ako, na primjer, ako aktivnost mora proći kroz velikog skupa dostupna podataka, imate veću vrijednost istodobnosti ubrzava obradu podataka. 
executionPriorityOrder | NewestFirst<br/><br/>OldestFirst | OldestFirst | Određuje redoslijed isječaka podataka koji se obrađuju se.<br/><br/>Ako, na primjer, ako imate presijeca 2 (jedan događa pri 4 pm i neki drugi kod 5 pm), a oba čekaju izvršavanja. Ako ste postavili executionPriorityOrder biti NewestFirst, najprije odvija se odsječak na 5 Poslijepodne. Na sličan način ako ste postavili executionPriorityORder biti OldestFIrst, zatim isječak pri 4 PM obrađuje. 
Pokušajte ponovno | Cijeli broj<br/><br/>Maksimalna vrijednost može biti 10 | 3 | Broj ponovne pokušaje prije obradu podataka za isječak je označena kao pogreška. Izvršavanje aktivnosti za podataka isječak je pokušati na veći broj navedeni pokušajte ponovno. Na pokušaj obavlja čim nakon pogreške.
Prekoračenje vremena | Vremenski raspon | 00:00:00 | Ograničenja za aktivnost. Primjer: 00:10:00 (podrazumijeva vremenskog ograničenja 10 minuta)<br/><br/>Ako vrijednost nije naveden ili je 0, vremensko ograničenje je beskonačno.<br/><br/>Ako vrijeme obrade podataka u isječak premašuje vrijednost vremenskog ograničenja, otkazat je, a sustav pokušava pokušati obrade. Broj ponovne pokušaje ovisi o svojstvo ponovnog pokušaja. Kada se pojavi vremensko ograničenje, status postavljen na prekoračili vremensko ograničenje.
Odgoda | Vremenski raspon | 00:00:00 | Odredite vrijeme nakon kojeg će obradu podataka pokretanja isječak.<br/><br/>Izvršavanje aktivnosti podataka isječak će se pokrenuti nakon odgode prošle očekivano vrijeme izvođenja.<br/><br/>Primjer: 00:10:00 (podrazumijeva Odgoda od 10 minuta)
longRetry | Cijeli broj<br/><br/>Maksimalna vrijednost: 10 | 1 | Koliko dugo pokušaj pokušava prije izvođenja isječak nije uspio.<br/><br/>Prilikom pokušaja longRetry raspoređene su po longRetryInterval. Da bi vam je potrebna da biste naveli vrijeme između dva pokušaja pokušaj, koristiti longRetry. Ako su navedeni pokušaj i longRetry, svaki pokušaj longRetry obuhvaća pokušaj pokušaja i maksimalni broj pokušaja je pokušaj * longRetry.<br/><br/>Na primjer, ako aktivnosti pravila imamo sljedeće postavke:<br/>Pokušajte ponovno: 3<br/>longRetry: 2<br/>longRetryInterval: 01:00:00<br/><br/>Pretpostavimo da vam se prikazuje samo jedan isječak izvršiti (čeka status) i izvršavanje aktivnosti ne svaki put. Na početku bi postojati 3 uzastopnih izvođenja pokušaja. Nakon svakog pokušaj status isječak bi pokušajte ponovno. Nakon što su najprije 3 prilikom pokušaja iznad, status isječak bi LongRetry.<br/><br/>Nakon čas (to jest, longRetryInteval na vrijednost) nema bi drugi skup 3 uzastopnih izvođenja pokušaja. Nakon toga status isječak želite nije uspjelo i nema više ponovne pokušaje bi će se izvršiti. Dakle ukupni 6 pokušaja nastali.<br/><br/>Ako bilo koji izvođenja potvrdi, status isječak bio spreman i nema više ponovne pokušaje su ponoviti.<br/><br/>longRetry može se koristiti u situacijama stigne zavisnih podataka koje nisu deterministic vrijeme ili cjelokupan okruženje je flaky u odjeljku pojavljuje koje obradu podataka. U tim slučajevima taj način ponovne pokušaje nakon međusobno možda vam neće pomoći i time nakon intervala od vremena rezultate u željeni izlazni rezultat.<br/><br/>Upozorenje: postavljanje visoke vrijednosti za longRetry ili longRetryInterval. Obično velikih vrijednosti impliciraju ostalih systemic problema. 
longRetryInterval | Vremenski raspon | 00:00:00 | Razmak između pokušaja dugo Ponovi 


## <a name="next-steps"></a>Daljnji koraci

- Objašnjenje mogućnosti [zakazivanja i izvršavanja u tvorničke Azure podataka](data-factory-scheduling-and-execution.md).  
- Saznajte više o [premještanju podataka](data-factory-data-movement-activities.md) i [Mogućnosti transformacije podataka](data-factory-data-transformation-activities.md) u tvorničke Azure podataka
- Objašnjenje mogućnosti [upravljanja te praćenje u tvorničke Azure podataka](data-factory-monitor-manage-pipelines.md).
- [Stvaranje i implementaciju na kanal prvog](data-factory-build-your-first-pipeline.md). 
