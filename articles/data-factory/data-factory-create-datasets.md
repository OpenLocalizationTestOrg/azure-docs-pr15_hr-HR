<properties 
    pageTitle="Stvaranje skupova podataka na tvorničke Azure podataka | Microsoft Azure" 
    description="Saznajte kako stvoriti skupova podataka na tvorničke podataka Azure s primjere koji koriste svojstva kao što su pomak i anchorDateTime."
    keywords="Stvaranje skupa podataka, dataset primjer, pomak primjer"
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
    ms.date="09/13/2016" 
    ms.author="shlo"/>

# <a name="datasets-in-azure-data-factory"></a>Skupova podataka na tvorničke Azure podataka
U ovom se članku opisuje skupova podataka na tvorničke Azure podataka i sadrži primjere kao što su pomak, anchorDateTime i offset/stil baze podataka.

Kada stvorite skup podataka, koji stvarate pokazivač da biste podatke koje želite obrađivati. Podaci se obrađuju (ulazni i izlazni) u aktivnosti i aktivnosti nalazi se na kanal. Unos dataset predstavlja unosa za neku aktivnost u kanalu, a skup podataka za izlaz izlaz aktivnosti.

Skupove podataka odredite podatke iz različitih vrsta služi za pohranu, kao što su tablice, datoteka, mapa i dokumenata. Kada stvorite skup podataka, možete je koristiti s aktivnosti u na kanal. Na primjer, skup podataka može biti ulazni i izlazni skup podataka aktivnost Kopiraj ili HDInsightHive aktivnost. Portal za Azure omogućuje vizualni izgled sve unose kanali i podataka i izlaza. Kratak, prikazuju se sve odnose i ovisnosti vaše kanali sve izvore da biste uvijek znali gdje se podataka dolazi iz i gdje se događa.

U Azure tvorničke podataka, možete dohvaćanje podataka iz skup podataka pomoću aktivnosti Kopiraj u na kanal.

> [AZURE.NOTE] Ako ste novi korisnik tvorničke Azure podataka, pročitajte članke [Uvod u tvorničke Azure podataka](data-factory-introduction.md) da biste saznali kako servisa Azure podataka tvorničke. Potražite u članku [Sastavljanje vaš prvi tvorničke podataka](data-factory-build-your-first-pipeline.md) vodič za stvaranje na prvi tvorničke podataka. Ove dvije članci sadrže pozadine potrebne informacije da biste bolje razumjeli u ovom se članku. 

## <a name="define-datasets"></a>Definiranje skupova podataka
Skup podataka u Azure podataka tvorničke se definira na sljedeći način: 


    {
        "name": "<name of dataset>",
        "properties": {
            "type": "<type of dataset: AzureBlob, AzureSql etc...>",
            "external": <boolean flag to indicate external data. only for input datasets>,
            "linkedServiceName": "<Name of the linked service that refers to a data store.>",
            "structure": [
                {
                    "name": "<Name of the column>",
                    "type": "<Name of the type>"
                }
            ],
            "typeProperties": {
                "<type specific property>": "<value>",
                "<type specific property 2>": "<value 2>",
            },
            "availability": {
                "frequency": "<Specifies the time unit for data slice production. Supported frequency: Minute, Hour, Day, Week, Month>",
                "interval": "<Specifies the interval within the defined frequency. For example, frequency set to 'Hour' and interval set to 1 indicates that new data slices should be produced hourly>"
            },
           "policy": 
            {      
            }
        }
    }

U sljedećoj tablici opisane svojstava u iznad JSON:   

| Svojstvo | Opis | Obavezno | Zadani |
| -------- | ----------- | -------- | ------- |
| ime | Naziv skupu podataka. Pravila imenovanja potražite u članku [Tvorničke podataka Azure - imenovanja pravila](data-factory-naming-rules.md) . | Da | N/D |
| Vrsta | Vrsta skupu podataka. Odredite koje podržava tvorničke Azure podataka (na primjer: AzureBlob, AzureSqlTable). <br/><br/>Detalje potražite u članku [Vrsta skupa podataka](#Type) . | Da | N/D |
| Struktura | Sheme skupu podataka<br/><br/>Detalje potražite u članku [Dataset strukturu](#Structure) sekcije. | ne. | N/D |
| typeProperties | Svojstva odgovaraju za odabranu vrstu. Odjeljak [Vrsta skupa podataka](#Type) potražite u članku informacije o podržanim i njihova svojstva. | Da | N/D |
| Vanjski | Booleova zastavica za određivanje hoće li skup podataka je izričito osnovu kanalima za tvorničke podataka ili ne.  | ne | FALSE | 
| Dostupnost | Određuje prozoru obrada ili slicing modela za radni skup podataka. <br/><br/>Detalje potražite u članku [Dataset dostupnost](#Availability) sekcije. <br/><br/>Informacije o skupu podataka istjecanju model, a zatim potražite u članku [Zakazivanje sastanka te izvođenja](data-factory-scheduling-and-execution.md) članka. | Da | N/D
| pravila | Određuje kriterije ili uvjet koji se mora ispuniti isječke skup podataka. <br/><br/>Detalje potražite u članku [Pravila Dataset](#Policy) sekcije. | ne | N/D |

## <a name="dataset-example"></a>Primjer skup podataka
U sljedećem primjeru skupu podataka predstavlja tablice pod nazivom **MojaTablica** u bazom **podataka Azure SQL**. 

    {
        "name": "DatasetSample",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": 
            {
                "tableName": "MyTable"
            },
            "availability": 
            {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

Imajte na umu sljedeće točke: 

- Vrsta postavljena na AzureSqlTable.
- Svojstvo vrsta tableName (određenu vrstu AzureSqlTable) postavljen je na MojaTablica.
- linkedServiceName odnosi se na servis za povezane vrste AzureSqlDatabase. U odjeljku definiciju sljedeće povezane usluge. 
- Dostupnost učestalost je dan i interval postavljeno na 1, što znači da isječak svakodnevno proizvodi.  

AzureSqlLinkedService se definira na sljedeći način:

    {
        "name": "AzureSqlLinkedService",
        "properties": {
            "type": "AzureSqlDatabase",
            "description": "",
            "typeProperties": {
                "connectionString": "Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>@<servername>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30"
            }
        }
    }

U gornjem JSON: 

- Vrsta postavljena na AzureSqlDatabase
- Svojstvo vrsta connectionString određuje podatke za povezivanje s bazom podataka Azure SQL.  


Kao što vidite, servis za povezane definira kako se povezati s bazom podataka Azure SQL. Skup podataka određuje koje tablica služi kao ulazni i izlazni aktivnosti u na kanal. U odjeljku aktivnosti vaše [kanal](data-factory-create-pipelines.md) JSON određuje koristi li se skupu podataka kao ulazni i izlazni skup podataka.


> [AZURE.IMPORTANT] Osim ako je skup podataka osnovu tvorničke Azure podataka, moraju biti označene kao **vanjski**. Ova postavka primjenjuje se obično na unosa prve aktivnosti u na kanal.   

## <a name="Type"></a>Vrsta skupa podataka
Poravnati su podržani izvori podataka i vrste skupa podataka. Potražite u temama u članku [Aktivnosti premještanje podataka](data-factory-data-movement-activities.md#supported-data-stores) informacije o vrstama i konfiguracija skupova podataka. Ako, na primjer, ako koristite podataka iz baze podataka Azure SQL, kliknite baze podataka SQL Azure na popisu podržanih trgovine da biste vidjeli detaljne informacije.  

## <a name="Structure"></a>Struktura skup podataka
U odjeljku **strukturu** definira shemu skupu podataka. Sadrži skup imena i vrste podataka stupaca.  U sljedećem primjeru skupu podataka ima tri stupca slicetimestamp, nazivprojekta i pageviews i su vrste: niza, niz i decimalnih odnosno.

    structure:  
    [ 
        { "name": "slicetimestamp", "type": "String"},
        { "name": "projectname", "type": "String"},
        { "name": "pageviews", "type": "Decimal"}
    ]

## <a name="Availability"></a>Dostupnost za skup podataka
U odjeljku **dostupnost** skup podataka određuje prozoru obrada (hourly, jedanput dnevno, tjedno itd.) ili slicing modela za skup podataka. Potražite u članku [Zakazivanje sastanka te izvođenja](data-factory-scheduling-and-execution.md) članka dodatne informacije o dataset istjecanju i ovisnosti modela. 

U sljedećoj sekciji dostupnost određuje dataset izlaz proizvedeno hourly (ili) unos skup podataka dostupna je zaračunava:

    "availability": 
    {   
        "frequency": "Hour",        
        "interval": 1   
    }

U sljedećoj tablici opisane svojstva u odjeljku dostupnost možete koristiti: 

| Svojstvo | Opis | Obavezno | Zadani |
| -------- | ----------- | -------- | ------- |
| FREQUENCY | Određuje jedinica vremena za proizvodnju isječak skup podataka.<br/><br/>**Podržani učestalost**: Minute, sat, dan, tjedan, mjesec | Da | N/D |
| Interval | Određuje množitelj za učestalost<br/><br/>"Interval učestalosti x" određuje koliko često je sastavio isječak.<br/><br/>Ako vam je potrebna skup podataka da biste se sliced na svaki sat temelj, postavite **Učestalost** **sat**i **interval** **1**.<br/><br/>**Bilješke:** Ako odredite učestalost kao minutu, preporučujemo da postaviti interval za manje od 15 | Da | N/D |
| Stil | Određuje hoće li se isječak proizvesti na početak i kraj interval.<ul><li>StartOfInterval</li><li>EndOfInterval</li></ul><br/><br/>Ako je učestalost postavili mjesec, a skup stilova možete EndOfInterval, isječak nalazi se na posljednji dan u mjesecu. Ako se željeni stil postavljena na StartOfInterval, isječak nalazi se na prvi dan u mjesecu.<br/><br/>Ako je učestalost je dan i skup stilova možete EndOfInterval, isječak je sastavio u zadnjem satu dana.<br/><br/>Ako je učestalost postavljena na sat i skup stilova možete EndOfInterval, isječak je sastavio na kraju sat. Isječak za isječkom 1 PM – 2 PM razdoblja, na primjer, je sastavio pri 2 Poslijepodne. | ne | EndOfInterval |
| anchorDateTime | Definira apsolutni položaj u vremenu koristi raspored za izračunavanje granice isječak skup podataka. <br/><br/>**Bilješke:** Ako je na AnchorDateTime dijelova datuma koji su precizniji od učestalosti zanemaruju se precizniji dijelove. <br/><br/>Na primjer, ako je **interval** **zaračunava** (učestalost: sat i intervala: 1) i **AnchorDateTime** sadrži **u minutama i sekundama**, a zatim **u minutama i sekundama** dijelove na AnchorDateTime zanemaruju. | ne | 01/01/0001 |
| OFFSET | Vremenski raspon koji se pomiču početak i završetak sve isječke skup podataka. <br/><br/>**Bilješke:** Ako su navedeni anchorDateTime i offset, rezultat je kombinirane shift. | ne | N/D |

### <a name="offset-example"></a>OFFSET primjer

Dnevnu presijeca koji se pokreću pri 6: 00 umjesto zadanog ponoći. 

    "availability":
    {
        "frequency": "Day",
        "interval": 1,
        "offset": "06:00:00"
    }

**Učestalost** je postavljeno na **dan** i **interval** postavljeno na **1** (jednom dnevno): Ako želite da se u isječak treba proizvesti pri 6: 00 umjesto na zadano vrijeme: 12 AM. Imajte na umu da je ovaj put UTC vremena. 

## <a name="anchordatetime-example"></a>primjer anchorDateTime

**Primjer:** 23 sata dataset isječke koje započinju na 2007-04-19T08:00:00

    "availability": 
    {   
        "frequency": "Hour",        
        "interval": 23, 
        "anchorDateTime":"2007-04-19T08:00:00"  
    }

## <a name="offsetstyle-example"></a>Primjer pomak/stila

Ako vam je potrebna skupa podataka na mjesečno na određeni datum i vrijeme (pretpostavimo da na 3 svakog mjeseca na 8:00 Prijepodne), koristite oznaku **Pomak** za postavljanje datuma i vremena pokretati. 

    {
      "name": "MyDataset",
      "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
          "tableName": "MyTable"
        },
        "availability": {
          "frequency": "Month",
          "interval": 1,
          "offset": "3.08:10:00",
          "style": "StartOfInterval"
        }
      }
    }


## <a name="Policy"></a>Pravilnik za skup podataka

U odjeljku **pravila** definicija skupa podataka određuje kriterije ili uvjet koji se mora ispuniti isječke skup podataka.

### <a name="validation-policies"></a>Pravila provjere valjanosti

| Naziv pravila | Opis | Primjenjuje se na | Obavezno | Zadani |
| ----------- | ----------- | ---------- | -------- | ------- |
| minimumSizeMB | Provjerava ispunjava li podatke u programa **Azure bloba** preduvjete Minimalna veličina (u megabajtima). | Blobova platforme Azure | ne | N/D |
|minimumRows | Provjeri valjanost podataka na web-mjesto bazom **podataka Azure SQL** ili u okvir za **Azure tablica** sadrži je najmanji broj redaka. | <ul><li>Baze podataka Azure SQL</li><li>Tablica platforme Azure</li></ul> | ne | N/D

#### <a name="examples"></a>Primjeri

**minimumSizeMB:**

    "policy":
    
    {
        "validation":
        {
            "minimumSizeMB": 10.0
        }
    }

**minimumRows**

    "policy":
    {
        "validation":
        {
            "minimumRows": 100
        }
    }

### <a name="external-datasets"></a>Vanjski skupova podataka

Vanjski skupove podataka se oni koje je stvorio izvodi kanal na tvorničke podataka. Ako skup podataka je označena kao **vanjski**, pravila **ExternalData** može se definirati koji utječe na ponašanje isječak dostupnosti skupa podataka. 

Osim ako je skup podataka osnovu tvorničke Azure podataka, moraju biti označene kao **vanjski**. Ova postavka primjenjuje obično na unosa prve aktivnosti u na kanalu osim ako se koristi aktivnost ili Ulančavanje kanal. 

| Ime | Opis | Obavezno | Zadana vrijednost  |
| ---- | ----------- | -------- | -------------- |
| dataDelay | Vrijeme da biste odgodili Provjera dostupnosti vanjske podatke za dani isječak. Ako, na primjer, ako je podatke potrebno zaračunava biti dostupna, potvrdite da biste vidjeli vanjskih podataka dostupna je i odgovarajuće isječak je spremna može odgoditi pomoću dataDelay.<br/><br/>Odnosi se samo na vrijeme za izlaganje.  Na primjer, ako je 1:00 PM odmah, a ta vrijednost je 10 minuta, provjera valjanosti započinje 1:10 Poslijepodne.<br/><br/>Ta postavka utječe na isječke u prošlosti (isječaka s isječkom vrijeme završetka + dataDelay < sada) obrađuju bez bilo koje vrijeme.<br/><br/>Vrijeme veće od 23:59 sati treba naveden u obliku day.hours:minutes:seconds. Na primjer, da biste odredili 24 sata, nemojte koristiti 24:00:00; Umjesto toga koristite 1.00:00:00. Ako koristite 24:00:00 se smatra 24 dana (24.00:00:00). Jedan dan i 4 sata, navedite 1:04:00:00. | ne | 0 |
| retryInterval | Vrijeme čekanja između pogreške i sljedeći ponovnog pokušaja. Odnosi se na prikaz vremena; Ako prethodna pokušajte nije uspio, ne možemo Pričekajte to dugo nakon zadnjeg pokušajte. <br/><br/>Ako je 1:00 pm odmah, ne možemo započeti u prvom pokušaju. Da biste dovršili prvi Provjera valjanosti traje 1 min i operacija nije uspjela, sljedeći pokušaj li pri 1:00 + 1 min (trajanje) + 1min (Ponovi interval) = 1:02 poslijepodne. <br/><br/>Za isječke u prošlosti postoji nema kašnjenja. Na pokušaj događa odmah. | ne | 00:01:00 (1 minuti) | 
| retryTimeout | Prekoračenje vremena za svaki pokušaj pokušajte ponovno.<br/><br/>Ako je ovo svojstvo je postavljeno na 10 minuta, provjera valjanosti mora dovršiti unutar 10 minuta. Ako je potrebno više od 10 minuta da biste izvršili provjeru, na pokušaj ističe.<br/><br/>Ako se vrijeme sve pokušaje za provjeru, isječak je označena kao prekoračili vremensko ograničenje. | ne | 00:10:00 (10 minuta) |
| maximumRetry | Broj da biste provjerili dostupnost vanjskih podataka. Dopuštene Maksimalna je vrijednost 10. | ne | 3 | 

## <a name="scoped-datasets"></a>Opsegu skupova podataka
Možete stvoriti skupove podataka koji imaju ograničen prikaz samo na kanalima korištenjem svojstva **skupova podataka** . Ove skupove podataka može se koristiti samo po aktivnosti u sklopu ovaj kanal, ali ne po aktivnosti u drugim kanali. U sljedećem primjeru definira kanal s dvaju skupova podataka – InputDataset rdc i OutputDataset rdc - koja će se koristiti u kanal:  

> [AZURE.IMPORTANT] Opsegu skupove podataka podržani su samo s jednokratni kanali (**pipelineMode** postavljena na **OneTime**). Detalje potražite u članku [Onetime kanal](data-factory-scheduling-and-execution.md#onetime-pipeline) .

    {
        "name": "CopyPipeline-rdc",
        "properties": {
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource",
                            "recursive": false
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "InputDataset-rdc"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "OutputDataset-rdc"
                        }
                    ],
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1,
                        "style": "StartOfInterval"
                    },
                    "name": "CopyActivity-0"
                }
            ],
            "start": "2016-02-28T00:00:00Z",
            "end": "2016-02-28T00:00:00Z",
            "isPaused": false,
            "pipelineMode": "OneTime",
            "expirationTime": "15.00:00:00",
            "datasets": [
                {
                    "name": "InputDataset-rdc",
                    "properties": {
                        "type": "AzureBlob",
                        "linkedServiceName": "InputLinkedService-rdc",
                        "typeProperties": {
                            "fileName": "emp.txt",
                            "folderPath": "adftutorial/input",
                            "format": {
                                "type": "TextFormat",
                                "rowDelimiter": "\n",
                                "columnDelimiter": ","
                            }
                        },
                        "availability": {
                            "frequency": "Day",
                            "interval": 1
                        },
                        "external": true,
                        "policy": {}
                    }
                },
                {
                    "name": "OutputDataset-rdc",
                    "properties": {
                        "type": "AzureBlob",
                        "linkedServiceName": "OutputLinkedService-rdc",
                        "typeProperties": {
                            "fileName": "emp.txt",
                            "folderPath": "adftutorial/output",
                            "format": {
                                "type": "TextFormat",
                                "rowDelimiter": "\n",
                                "columnDelimiter": ","
                            }
                        },
                        "availability": {
                            "frequency": "Day",
                            "interval": 1
                        },
                        "external": false,
                        "policy": {}
                    }
                }
            ]
        }
    }