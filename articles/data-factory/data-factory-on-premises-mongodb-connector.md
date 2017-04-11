<properties 
    pageTitle="Premještanje podataka iz MongoDB pomoću podataka tvorničke | Microsoft Azure" 
    description="Saznajte više o premještanje podataka iz baze podataka MongoDB pomoću tvorničke Azure podataka." 
    services="data-factory" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/04/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-mongodb-using-azure-data-factory"></a>Premještanje podataka iz MongoDB pomoću tvorničke Azure podataka

U ovom se članku opisuje kako možete koristiti aktivnosti Kopiraj u na tvorničke Azure podataka za premještanje podataka iz baze podataka MongoDB lokalnog u drugom spremišta podataka. U ovom se članku sastavlja na članak [aktivnosti premještanje podataka](data-factory-data-movement-activities.md) koja predstavlja Općenito pregled premještanje podataka s Kopiraj aktivnosti i kombinacije izvor/primatelj podataka iz trgovine koji podržava aktivnosti Kopiraj.

Podatkovnog servisa tvorničke podržava povezivanje s lokalnim MongoDB izvorima pomoću pristupnika za upravljanje podacima. Potražite u članku [Pristupnik za upravljanje podacima](data-factory-data-management-gateway.md) dodatne informacije o pristupnik za upravljanje podacima i članak [Premještanje podataka iz lokalnog na cloud](data-factory-move-data-between-onprem-and-cloud.md) detaljne upute o postavljanju pristupnika kanala podataka da biste premjestili sadržaj. 

> [AZURE.NOTE] Morate koristiti pristupnika možete povezati MongoDB čak i ako se ona nalazi u Azure IaaS VMs. Ako pokušavate povezati instance komponente MongoDB smješten u oblak, možete instalirati i instance pristupnika u IaaS VM.

Tvorničke podataka trenutno podržava samo premještanje podataka iz MongoDB u drugim spremišta podataka, ali ne za premještanje podataka iz drugih podataka trgovina MongoDB.

## <a name="prerequisites"></a>Preduvjeti
Za servis tvorničke Azure podataka da biste se mogli povezati s lokalnim MongoDB baze podataka, morate instalirati sljedeće komponente: 

- Pristupnik za upravljanje podacima 2.0 ili noviji na istom računalu koje hostira baze podataka ili na zasebnom računalo da biste izbjegli natječu za resurse s bazom podataka. Pristupnik za upravljanje podacima je softver koja povezuje lokalnih izvora podataka za servise u oblaku sigurne i upravljani način. Pogledajte članak [Pristupnik za upravljanje podacima](data-factory-data-management-gateway.md) detalje o pristupnik za upravljanje podacima.
  
    Kada instalirate pristupnika, automatski instalira Microsoft MongoDB ODBC upravljački program koje se koristi za povezivanje s MongoDB. 

## <a name="copy-data-wizard"></a>Čarobnjak za kopiranje podataka
Da biste stvorili kanal koja se kopira podatke iz baze podataka MongoDB svih podržanih primatelj služi za pohranu podataka najjednostavnije za korištenje čarobnjaka za kopiranje podataka. U odjeljku [Praktični vodič: Stvaranje kanal pomoću čarobnjaka za kopiranje](data-factory-copy-data-wizard-tutorial.md) Brzi vodič za stvaranje kanal pomoću čarobnjaka za kopiranje podataka. 

U sljedećem primjeru sadrži ogledne JSON definicije koje možete koristiti da biste stvorili na kanal pomoću [portala za Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) ili [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) ili [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Prikazuju kako kopirati podatke iz baze podataka MongoDB spremište blobova platforme Azure. Međutim, podaci mogu kopirati na neku razinu na primatelji navedeni [u nastavku](data-factory-data-movement-activities.md#supported-data-stores) pomoću aktivnosti Kopiraj u tvorničke Azure podataka.

## <a name="sample-copy-data-from-mongodb-to-azure-blob"></a>Primjer: Kopiranje podataka iz MongoDB blobova platforme Azure
Ovaj primjer prikazuje način da biste kopirali podatke iz lokalne baze podataka MongoDB blobova Azure. Međutim, podaci mogu biti kopirana **izravno** u bilo koji od na primatelji navedeni [u nastavku](data-factory-data-movement-activities.md#supported-data-stores) pomoću aktivnosti Kopiraj u tvorničke Azure podataka.  
 
Uzorak sastoji se od sljedećih entiteti tvorničke podataka:

1.  Povezane servis vrste [OnPremisesMongoDb](#linked-service-properties).
2.  Povezane servis vrste [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Za unos [dataset](data-factory-create-datasets.md) vrste [MongoDbCollection](#dataset-type-properties).
4.  Za izlazni [skup podataka](data-factory-create-datasets.md) vrste [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  Na [kanal](data-factory-create-pipelines.md) s Kopiraj aktivnosti koje koristi [MongoDbSource](#copy-activity-type-properties) i [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Uzorak kopiranje podataka iz rezultata upita u bazi podataka MongoDB blob svaki sat. Svojstvima JSON koji se koriste u ta uzorka opisana su u odjeljcima pratiti primjere. 

Kao prvi korak, instalacija pristupnik za upravljanje podacima po upute u članku [Pristupnik za upravljanje podacima](data-factory-data-management-gateway.md) . 

**Servis za MongoDB povezana**

    { 
        "name": "OnPremisesMongoDbLinkedService", 
        "properties": 
        { 
            "type": "OnPremisesMongoDb", 
            "typeProperties": 
            { 
                "authenticationType": "<Basic or Anonymous>", 
                "server": "< The IP address or host name of the MongoDB server >",  
                "port": "<The number of the TCP port that the MongoDB server uses to listen for client connections.>", 
                "username": "<username>", 
                "password": "<password>",
               "authSource": "< The database that you want to use to check your credentials for authentication. >",
                "databaseName": "<database name>",
                "gatewayName": "<mygateway>"
            } 
        }
    } 


**Azure servis za pohranu povezana**

    {
      "name": "AzureStorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**Unos dataset MongoDB** Postavljanje "vanjski": "true" obavještava servis tvorničke podataka u tablici ne ovisi o tvorničke podataka, a ne osnovu aktivnost u tvorničke podataka.
    
    {
         "name":  "MongoDbInputDataset",
        "properties": { 
            "type": "MongoDbCollection", 
            "linkedServiceName": "OnPremisesMongoDbLinkedService", 
            "typeProperties": { 
                "collectionName": "<Collection name>"   
            }, 
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true
        } 
    }



**Blobova platforme Azure izlazni skup podataka**

Podaci se upisuju u novi blob svaki sat (učestalost: h, interval: 1). Put do mape za blob-om dinamički vrednuje na temelju vremena početka isječka koji obrađuje. Put do mape koristi godinu, mjesec, dan i sati dijelove vrijeme početka.

    {
        "name": "AzureBlobOutputDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/frommongodb/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
                "format": {
                    "type": "TextFormat",
                    "rowDelimiter": "\n",
                    "columnDelimiter": "\t"
                },
                "partitionedBy": [
                    {
                        "name": "Year",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "yyyy"
                        }
                    },
                    {
                        "name": "Month",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "%M"
                        }
                    },
                    {
                        "name": "Day",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "%d"
                        }
                    },
                    {
                        "name": "Hour",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "%H"
                        }
                    }
                ]
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }



**Kanal s Kopiraj aktivnosti**

Kanal sadrži aktivnosti Kopiraj koji je konfiguriran za korištenje iznad ulazni i izlazni skupova podataka i zakazana da biste pokrenuli svaki sat. U kanalu JSON definicija vrsta **izvora** postavljen na **MongoDbSource** , a **primatelj** je vrsta **BlobSink**. SQL upit za svojstvo **upita** odabrat ćete podatke u zadnjem satu da biste kopirali.
    
    {
        "name": "CopyMongoDBToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "MongoDbSource",
                            "query": "$$Text.Format('select * from  MyTable where LastModifiedDate >= {{ts\'{0:yyyy-MM-dd HH:mm:ss}\'}} AND LastModifiedDate < {{ts\'{1:yyyy-MM-dd HH:mm:ss}\'}}', WindowStart, WindowEnd)"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "MongoDbInputDataset"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutputDataSet"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "MongoDBToAzureBlob"
                }
            ],
            "start": "2016-06-01T18:00:00Z",
            "end": "2016-06-01T19:00:00Z"
        }
    }


## <a name="linked-service-properties"></a>Svojstva povezane servisa

Sljedeća tablica sadrži opis elemenata JSON specifične za servis **OnPremisesMongoDB** povezani.

| Svojstvo | Opis | Obavezno |
| -------- | ----------- | -------- | 
| Vrsta | Svojstvo vrsta mora biti postavljeno na: **OnPremisesMongoDb** | Da |
| poslužitelj | IP adresa ili naziv glavnog računala poslužitelja MongoDB. | Da | 
| priključak | TCP priključak poslužitelja MongoDB koristi da biste preslušali za povezivanje. | Obavezan, zadana vrijednost: 27017 |
| authenticationType | Osnovni ili anonimne. | Da | 
| korisničko ime | Korisnički račun za pristup MongoDB. | Da (Ako je knjiga koristi se osnovna provjera autentičnosti).|
| lozinke | Lozinka za korisnika. |   Da (Ako je knjiga koristi se osnovna provjera autentičnosti). | 
| authSource | Naziv MongoDB bazu podataka koju želite koristiti da biste provjerili vjerodajnice za provjeru autentičnosti. | Neobavezno (Ako je knjiga koristi se osnovna provjera autentičnosti). zadani: koristi administratorskog računa i navedena pomoću ImeBazePodataka svojstvo baza podataka. |  
| ImeBazePodataka | Naziv MongoDB bazu podataka koju želite im pristupiti. | Da |
| gatewayName | Naziv pristupnika koji pristupa spremišta podataka. | Da | 
| encryptedCredential | Vjerodajnica šifrirane pristupnik. | Neobavezno |


Detalje o postavljanju vjerodajnica za lokalni izvor podataka MongoDB u odjeljku [postavka vjerodajnice i sigurnost](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) .

## <a name="dataset-type-properties"></a>Svojstva vrste skup podataka

Potpuni popis sekcija i svojstva dostupna za definiranje skupove podataka potražite u članku [Stvaranje skupova podataka](data-factory-create-datasets.md) . Sekcija kao što su strukturu, dostupnost i pravila dataset JSON su slične za sve vrste skup podataka (Azure SQL, blobova platforme Azure, tablica platforme Azure itd.).

U odjeljku **typeProperties** razlikuje za svaku vrstu skup podataka i daje informacije o lokaciji podataka u spremištu podataka. Odjeljak typeProperties za skup podataka vrste **MongoDbCollection** sadrži sljedeća svojstva:

| Svojstvo | Opis | Obavezno |
| -------- | ----------- | -------- |
| collectionName | Naziv zbirke u bazi podataka MongoDB. | Da | 

## <a name="copy-activity-type-properties"></a>Kopiranje svojstva vrste aktivnosti

Potpuni popis sekcija i svojstva dostupna za definiranje aktivnosti, potražite u članku [Stvaranje kanali](data-factory-create-pipelines.md) . Svojstva kao što su naziv, opis, ulazni i izlazni tablice, i pravila dostupni su za sve vrste aktivnosti. 

Svojstva dostupna u odjeljku **typeProperties** aktivnosti razlikuju se s druge strane sa svakom vrstom aktivnosti. Kopiraj aktivnosti, oni ovisi o vrsti izvora i primatelji.

Kad je izvor vrste **MongoDbSource** sljedeća svojstva dostupne su u odjeljku typeProperties:

| Svojstvo | Opis | Dopuštena vrijednost | Obavezno |
| -------- | ----------- | -------------- | -------- |
| upit | Čitanje podataka pomoću prilagođenog upita. | Niz SQL-92 upita. Na primjer: Odaberite * iz MojaTablica. | Ne (Ako je naveden **collectionName** **skup podataka** ) | 

## <a name="schema-by-data-factory"></a>Shema po tvorničke podataka
Pomoću najnovije 100 dokumenata u zbirci servisa Azure tvorničke podataka automatski aktivira shemu iz zbirke MongoDB. Ako te 100 dokumente sadrže potpunu shemu, neki stupci možda zanemaruju tijekom kopiranja. 

## <a name="type-mapping-for-mongodb"></a>Mapiranje vrsta za MongoDB

Kao što je rečeno u članku [aktivnosti premještanje podataka](data-factory-data-movement-activities.md) Kopiraj aktivnosti izvodi Automatska vrsta pretvorbi iz vrste izvora da biste sita vrste s sljedeće pristup 2 koraka:

1. Pretvaranje iz vrste nativni izvora u vrstu .NET
2. Pretvaranje iz vrste .NET vrstu izvorni primatelj

Kada premještanju podataka MongoDB sljedeće mapiranja koriste vrste MongoDB .NET vrste.

| Vrsta MongoDB | Vrsta .NET framework |
| ------------------- | ------------------- | 
| Binarni | Bajt] |
| Booleove vrijednosti | Booleove vrijednosti |
| Datum | Datum i vrijeme |
| NumberDouble | Dvaput |
| NumberInt | Int32 |
| NumberLong | Int64 |
| ID objekta | Niz |
| Niz | Niz |
| UUID | GUID | 
| Objekt | Renormalized u stopi stupci s "_" kao ugniježđene razdjelnik |

> [AZURE.NOTE]  
> Dodatne informacije o podršci za polja korištenje virtualnih tablica, pogledajte odjeljak [podrška za složene vrste pomoću virtualne tablice](#support-for-complex-types-using-virtual-tables) ispod. 

Trenutno nisu podržane sljedeće vrste podataka MongoDB: DBPointer, JavaScript, Max-Min ključ, uobičajeni izraz, simbol, vremenska oznaka, Nedefinirano

## <a name="support-for-complex-types-using-virtual-tables"></a>Podrška za složene vrste korištenje virtualnih tablica
Azure tvorničke podataka koristi ugrađene ODBC upravljački program za povezivanje i kopiranje podataka iz baze podataka MongoDB. Za složene vrste kao što su polja ili objekte s različitim vrstama preko dokumente, upravljački program ponovno normalizes podataka u odgovarajuće virtualne tablice. Konkretno, ako tablica sadrži takve stupce, upravljački program generira virtualne u tablicama u nastavku:

-   **Osnovna tablica**, koja sadrži iste podatke kao realni tablicu osim Složena vrsta stupaca. Osnovna tablica koristi isti naziv kao realni tablicu koja predstavlja.
-   **Virtualna tablice** za svaki stupac složenoj koja proširuje ugniježđene podataka. Imenovane su virtualne tablice pod nazivom realni tablice, razdjelnik "_", a naziv polja ili objekta.

Virtualne tablica se referirati na podatke u realne tablici Omogućivanje upravljački program za pristup Denormalizirani podaci. Pogledajte primjer sekciju ispod pojedinosti. Sadržaj polja MongoDB možete pristupiti tako slanje upita i pridruživanje virtualne tablice.

Pomoću [Čarobnjaka za kopiranje](data-factory-data-movement-activities.md#data-factory-copy-wizard) intuitivno prikaz popisa tablica u bazi podataka MongoDB uključujući virtualne tablice i pretpregled podataka unutar. Sastavljanje upita u čarobnjaku za kopiranje i provjeru da biste vidjeli rezultat.

### <a name="example"></a>Primjer

Na primjer, "ExampleTable" ispod je MongoDB tablicu koja sadrži jedan stupac s polja objekata u svakoj ćeliji – fakturama i jedan stupac s polja vrste skalarna – ocjena. 

_id | Ime klijenta | Fakture | Razina usluge | Ocjena
--- | ------------- | -------- | ------------- | -------
1111 | ABC | [{invoice_id: "123", stavku: "toaster", cijena: "456", popust: "0,2"}, {invoice_id: "124", stavku: "pećnica", cijena: "1235" popust: "0,2"}] | Srebrno | [5,6]
2222 | NEKI | [{invoice_id: "135", stavku: "fridge", cijena: "12543", popust: "0.0"}] | Zlatna | [1,2]

Upravljački program želite generirati više virtualne tablica predstavlja ovaj jednu tablicu. Prvi virtualne tablica je osnovni tablice pod nazivom "ExampleTable" koje je prikazano u nastavku. Osnovna tablica sadrži sve podatke izvorna tablica, ali podaci iz poljima sadrži je ispušten i proširen je u virtualne tablicama.

_id | Ime klijenta | Razina usluge
--- | ------------- | -------------
1111 | ABC | Srebrno
2222 | NEKI | Zlatna

U sljedećoj tablici prikazane virtualne tablice koje predstavljaju izvorne polja u primjeru. U ovim su tablicama sadržavati sljedeće: 

- Referenca izvornog stupca primarnog ključa koja odgovara retku izvornog polja (putem stupac _id)
- Oznaka položaj podataka u izvornom polja 
- Prošireni podaci za svaki element unutar polja

Tablica "ExampleTable_Invoices":

_id | ExampleTable_Invoices_dim1_idx | invoice_id | stavke | cijena | Popust
--- | -------------- | ---------- | ---- | ----- | --------
1111 | 0 | 123 | toaster | 456 | 0,2
1111 | 1 | 124 | pećnica | 1235 | 0,2
2222 | 0 | 135 | fridge | 12543 | 0.0

Tablica "ExampleTable_Ratings":

_id | ExampleTable_Ratings_dim1_idx | ExampleTable_Ratings
--- | ------------- | -------------
1111 | 0 | 5
1111 | 1 | 6
2222 | 0 | 1
2222 | 1 | 2

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>Performanse i ugađanje  
Potražite u članku [Kopiranje aktivnosti performanse i vodič za ugađanje](data-factory-copy-activity-performance.md) dodatne informacije o ključa čimbenici koji utjecaj na performanse pomicanja podataka (Kopiraj aktivnosti) u tvorničke Azure podataka i razni načini optimizirati.

## <a name="next-steps"></a>Daljnji koraci
Potražite u članku [Premještanje podataka između lokalnog i oblaka](data-factory-move-data-between-onprem-and-cloud.md) detaljne upute za stvaranje podataka kanal koji premješta podatke iz lokalnih podataka trgovine je iz trgovine Azure podataka. 

