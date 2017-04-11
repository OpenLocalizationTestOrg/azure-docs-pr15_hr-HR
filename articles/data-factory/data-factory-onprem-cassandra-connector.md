<properties 
    pageTitle="Premještanje podataka iz Cassandra pomoću podataka tvorničke | Microsoft Azure" 
    description="Saznajte kako premjestiti podatke iz lokalnog Cassandra baze podataka pomoću tvorničke Azure podataka." 
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
    ms.date="09/07/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-an-on-premises-cassandra-database-using-azure-data-factory"></a>Premještanje podataka iz lokalnog Cassandra baze podataka pomoću tvorničke Azure podataka
U ovom se članku opisuje kako možete koristiti aktivnosti Kopiraj u na tvorničke Azure podataka da biste kopirali podatke iz lokalne baze podataka Cassandra sve spremišta podataka na popisu u odjeljku primatelj stupac u odjeljku [Podržani izvori i primatelji](data-factory-data-movement-activities.md#supported-data-stores) . U ovom se članku sastavlja na članak [aktivnosti premještanje podataka](data-factory-data-movement-activities.md) koja predstavlja Općenito pregled premještanje podataka s Kopiraj aktivnosti i kombinacijama podržanih trgovine.

Tvorničke podataka trenutno podržava samo premještanje podataka iz baze podataka za Cassandra podržane u budućnosti [primatelj podataka trgovine](data-factory-data-movement-activities.md#supported-data-stores), ali ne premještanje podataka iz drugih trgovina podataka u bazu podataka Cassandra.

## <a name="prerequisites"></a>Preduvjeti
Za servis tvorničke Azure podataka da biste se mogli povezati s lokalnim Cassandra baze podataka, morate instalirati sljedeće: 

- Pristupnik za upravljanje podacima 2.0 ili noviji na istom računalu koje hostira baze podataka ili na zasebnom računalo da biste izbjegli natječu za resurse s bazom podataka. Pristupnik za upravljanje podacima je softver koja povezuje lokalnih izvora podataka za servise u oblaku sigurne i upravljani način. Potražite u članku [Premještanje podataka između lokalnog i oblaka](data-factory-move-data-between-onprem-and-cloud.md) članak detalje o pristupnik za upravljanje podacima.
  
    Kada instalirate pristupnika, automatski instalira Microsoft Cassandra ODBC upravljački program koje se koriste za povezivanje s bazom podataka Cassandra. 

> [AZURE.NOTE] [Problema](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) potražite u članku otklanjanje poteškoća pristupnika za savjeta za otklanjanje poteškoća veze/pristupnik probleme vezane uz. 

## <a name="copy-data-wizard"></a>Čarobnjak za kopiranje podataka
Da biste stvorili kanal koja se kopira podatke iz baze podataka Cassandra svih podržanih primatelj služi za pohranu podataka najjednostavnije za korištenje čarobnjaka za kopiranje podataka. U odjeljku [Praktični vodič: Stvaranje kanal pomoću čarobnjaka za kopiranje](data-factory-copy-data-wizard-tutorial.md) Brzi vodič za stvaranje kanal pomoću čarobnjaka za kopiranje podataka. 

U sljedećem primjeru sadrži ogledne JSON definicije koje možete koristiti da biste stvorili na kanal pomoću [portala za Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) ili [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) ili [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Prikazuju kako kopirati podatke iz baze podataka Cassandra spremište blobova platforme Azure. Međutim, podaci mogu kopirati na neku razinu na primatelji navedeni [u nastavku](data-factory-data-movement-activities.md#supported-data-stores) pomoću aktivnosti Kopiraj u tvorničke Azure podataka.   


## <a name="sample-copy-data-from-cassandra-to-blob"></a>Primjer: Kopiranje podataka iz Cassandra blobova platforme
Uzorak kopiranje podataka iz baze podataka Cassandra blobova platforme Azure svaki sat. Svojstvima JSON koji se koriste u ta uzorka opisana su u odjeljcima pratiti primjere. Podaci mogu kopirati izravno u bilo koji od primatelji naveden u članku [Aktivnosti premještanje podataka](data-factory-data-movement-activities.md#supported-data-stores) pomoću aktivnosti Kopiraj u tvorničke Azure podataka. 

- Povezane servis vrste [OnPremisesCassandra](#onpremisescassandra-linked-service-properties).
- Povezane servis vrste [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
- Za unos [dataset](data-factory-create-datasets.md) vrste [CassandraTable](#cassandratable-properties).
- Za izlazni [skup podataka](data-factory-create-datasets.md) vrste [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
- Na [kanal](data-factory-create-pipelines.md) s Kopiraj aktivnosti koje koristi [CassandraSource](#cassandrasource-type-properties) i [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

**Servis za Cassandra povezana**

U ovom se primjeru koristi **Cassandra** povezana servis. Sekciji [Cassandra povezani servisa](#onpremisescassandra-linked-service-properties) potražite u članku svojstava podržava povezane servis.  

    {
        "name": "CassandraLinkedService",
        "properties":
        {
            "type": "OnPremisesCassandra",
            "typeProperties":
            {
                "authenticationType": "Basic",
                "host": "mycassandraserver",
                "port": 9042,
                "username": "user",
                "password": "password",
                "gatewayName": "mygateway"
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

**Unos dataset Cassandra**

    {
        "name": "CassandraInput",
        "properties": {
            "linkedServiceName": "CassandraLinkedService",
            "type": "CassandraTable",
            "typeProperties": {
                "tableName": "mytable",
                "keySpace": "mykeyspace" 
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }

Postavka **vanjske** **True** obavještava servis tvorničke podataka skup podataka ne ovisi o tvorničke podataka, a ne osnovu aktivnost u tvorničke podataka.

**Blobova platforme Azure izlazni skup podataka**

Podaci se upisuju u novi blob svaki sat (učestalost: h, interval: 1). 

    {
        "name": "AzureBlobOutput",
        "properties":
        {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties":
            {
                "folderPath": "adfgetstarted/fromcassandra"
            },
            "availability":
            {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }


**Kanal s Kopiraj aktivnosti**

Kanal sadrži aktivnosti Kopiraj koji je konfiguriran za korištenje skupova podataka ulazni i izlazni je zakazano izvođenje svaki sat. U kanalu JSON definicija vrsta **izvora** postavljen na **CassandraSource** , a **primatelj** je vrsta **BlobSink**. 

Popis svojstava podržava na RelationalSource potražite u članku [RelationalSource vrsta svojstva](#cassandrasource-type-properties) . 
    
    {  
        "name":"SamplePipeline",
        "properties":{  
            "start":"2016-06-01T18:00:00",
            "end":"2016-06-01T19:00:00",
            "description":"pipeline with copy activity",
            "activities":[  
            {
                "name": "CassandraToAzureBlob",
                "description": "Copy from Cassandra to an Azure blob",
                "type": "Copy",
                "inputs": [
                {
                    "name": "CassandraInput"
                }
                ],
                "outputs": [
                {
                    "name": "AzureBlobOutput"
                }
                ],
                "typeProperties": {
                    "source": {
                        "type": "CassandraSource",
                        "query": "select id, firstname, lastname from mykeyspace.mytable"
        
                    },
                    "sink": {
                        "type": "BlobSink"
                    }
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "OldestFirst",
                    "retry": 0,
                    "timeout": "01:00:00"
                }
            }
            ]   
        }
    }
## <a name="onpremisescassandra-linked-service-properties"></a>Svojstva OnPremisesCassandra povezana servisa

Sljedeća tablica sadrži opis elemenata JSON specifične za servis Cassandra povezani.

| Svojstvo | Opis | Obavezno |
| -------- | ----------- | -------- | 
| Vrsta | Svojstvo vrsta mora biti postavljeno na: **OnPremisesCassandra** | Da |
| glavno računalo | Jedan ili više IP adresa ili nazivi glavnog računala poslužitelja Cassandra.<br/><br/>Navedite popis odvojenih zarezom IP adresa ili nazivi glavnog računala da biste se povezali sa svim poslužiteljima istovremeno. | Da | 
| priključak | TCP priključka koji poslužitelj Cassandra koristi da biste preslušali za povezivanje. | Ne, zadana vrijednost: 9042 |
| authenticationType | Osnovni ili anonimnih | Da |
| korisničko ime |Navedite korisničko ime za korisnički račun. | Da, ako je authenticationType postavljeno na Basic. |
| lozinke | Unesite lozinku za korisnički račun.  | Da, ako je authenticationType postavljeno na Basic. |
| gatewayName | Naziv pristupnika koji se koristi za povezivanje s bazom podataka za lokalni Cassandra. | Da |
| encryptedCredential | Vjerodajnica šifrirane pristupnik. | ne | 

## <a name="cassandratable-properties"></a>Svojstva CassandraTable

Potpuni popis sekcija i svojstva dostupna za definiranje skupove podataka potražite u članku [Stvaranje skupova podataka](data-factory-create-datasets.md) . Sekcija kao što su strukturu, dostupnost i pravila dataset JSON su slične za sve vrste skup podataka (Azure SQL, blobova platforme Azure, tablica platforme Azure itd.).

U odjeljku **typeProperties** razlikuje za svaku vrstu skup podataka i daje informacije o lokaciji podataka u spremištu podataka. Odjeljak typeProperties za skup podataka vrste **CassandraTable** sadrži sljedeća svojstva

| Svojstvo | Opis | Obavezno |
| -------- | ----------- | -------- |
| keyspace | Naziv keyspace ili shemu u bazi podataka Cassandra. | Da (Ako je **upit** za **CassandraSource** je definiran). |
| tableName | Naziv tablice u bazi podataka Cassandra. | Da (Ako je **upit** za **CassandraSource** je definiran). |


## <a name="cassandrasource-type-properties"></a>Svojstva vrste CassandraSource
Potpuni popis sekcija i svojstva dostupna za definiranje aktivnosti, potražite u članku [Stvaranje kanali](data-factory-create-pipelines.md) . Svojstva kao što su naziv, opis, ulazni i izlazni tablice, i pravila dostupni su za sve vrste aktivnosti. 

Svojstva dostupna u odjeljku typeProperties aktivnosti razlikuju se s druge strane sa svakom vrstom aktivnosti. Kopiraj aktivnosti, oni ovisi o vrsti izvora i primatelji.

Kada je izvor vrste **CassandraSource**, sljedeća svojstva dostupne su u odjeljku typeProperties:

| Svojstvo | Opis | Dopuštena vrijednost | Obavezno |
| -------- | ----------- | -------------- | -------- |
| upit | Čitanje podataka pomoću prilagođenog upita. | SQL-92 upita ili CQL upita. Potražite u članku [Referenca CQL](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html). <br/><br/>Prilikom korištenja SQL upita, navedite **naziv name.table keyspace** za predstavljanje tablicu u kojoj želite poslati upit. | Ne (Ako su definirana tableName i keyspace na skup podataka).  |
| consistencyLevel | Razina dosljednost određuje koliko replike mora odgovoriti na zahtjev za čitanje prije vraćanja podataka u klijentsku aplikaciju. Cassandra provjerava na određeni broj replike za podatke na način koji zadovoljava zahtjev za čitanje. | JEDAN DVA, TRI, KVORUM, SVI, LOCAL_QUORUM EACH_QUORUM, LOCAL_ONE. Detalje potražite u članku [Konfiguriranje podataka dosljednost](http://docs.datastax.com/en//cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) . | ne. Zadana vrijednost je JEDAN. |  


### <a name="type-mapping-for-cassandra"></a>Mapiranje vrsta za Cassandra
Vrsta Cassandra | .NET temelji vrsta
--------------- | ---------------
ASCII | Niz 
BIGINT | Int64
BLOB | Bajt]
BOOLEOVE VRIJEDNOSTI | Booleove vrijednosti
BROJA DECIMALNIH MJESTA | Broja decimalnih mjesta
DVAPUT | Dvaput
VRIJEDNOST S POMIČNIM ZAREZOM | Jedan
INET | Niz
INT | Int32
TEKST | Niz
VREMENSKA OZNAKA | Datum i vrijeme
TIMEUUID | GUID
UUID | GUID
VARCHAR | Niz
VARINT | Broja decimalnih mjesta

> [AZURE.NOTE]  
> Za zbirku vrste (mapu, skup, popisa, itd.), pogledajte odjeljak za [Rad s Cassandra zbirke vrste pomoću virtualne tablice](#work-with-collections-using-virtual-table) . 
> 
> Korisnički definirana vrsta nisu podržani.
> 
> Duljina binarni stupac i niz stupac duljine ne može biti veći od 4000. 

## <a name="work-with-collections-using-virtual-table"></a>Rad s zbirke pomoću virtualne tablice
Azure tvorničke podataka koristi ugrađene ODBC upravljački program za povezivanje i kopiranje podataka iz baze podataka Cassandra. Zbirka uključujući karte, postavljanje i popisa, vrste upravljački program ponovno normalizes podatke u odgovarajuću virtualne tablice. Konkretno, ako tablica sadrži sve stupce zbirke, upravljački program generira virtualne u tablicama u nastavku:

-   **Osnovna tablica**, koja sadrži iste podatke kao tablicu realni osim stupaca zbirke. Osnovna tablica koristi isti naziv kao realni tablicu koja predstavlja.
-   **Virtualna tablice** za svaki stupac zbirke koja proširuje ugniježđene podataka. Virtualna tablice koje predstavljaju zbirke imenovane su s nazivom realni tablice, razdjelnik "_vt_", a naziv stupca.

Virtualne tablica se referirati na podatke u realne tablici Omogućivanje upravljački program za pristup Denormalizirani podaci. Pogledajte primjer sekcije detalje. Sadržaj zbirke Cassandra možete pristupiti tako slanje upita i pridruživanje virtualne tablice.

Korištenje [Čarobnjaka za kopiranje](data-factory-data-movement-activities.md#data-factory-copy-wizard) intuitivno prikaz popis tablica u bazi podataka Cassandra uključujući virtualne tablice i pretpregled podataka unutar. Sastavljanje upita u čarobnjaku za kopiranje i provjeru da biste vidjeli rezultat.

### <a name="example"></a>Primjer
Na primjer, sljedeće "ExampleTable" je Cassandra bazu podataka koja sadrži na cijeli stupac primarnog ključa pod nazivom "pk_int", tekstni stupac pod nazivom vrijednost, stupca popisa, mapa stupca i postavi stupac (pod nazivom "StringSet").

pk_int | Vrijednost | Popis | Karte |   StringSet
------ | ----- | ---- | --- | --------
1 | "uzorka vrijednost 1" | ["1", "2", "3"]  | {"S1": "U," "S2": "b"} | {"A", "B", "C"}
3 | "uzorka vrijednost 3" | ["100", "101", "102", "105"] | {"S1": "t"} | {"A", "E"}

Upravljački program želite generirati više virtualne tablica predstavlja ovaj jednu tablicu. Vanjski ključ stupce u virtualne tablice referencu stupaca primarnog ključa u tablici u stvarnom i označavanje koji realni tablice Redak odgovara redak virtualne tablice. 

Prvi virtualne tablica je osnovni tablice pod nazivom "ExampleTable" prikazuju se u tablici u nastavku. Osnovna tablica sadrži iste podatke kao izvorna tablica baze podataka osim zbirki, koje su izostavljena iz ove tablice i proširenog u drugim tablicama virtualne.

pk_int | Vrijednost
------ | -----
1 | "uzorka vrijednost 1"
3 | "uzorka vrijednost 3"

U sljedećoj tablici prikazane virtualne tablice koje renormalize podatke iz stupaca popisa, mapa i StringSet. Stupce s nazivima koje završavaju s "_index" ili "_key" određuju položaja podataka izvornom popisu ili u mapu. Stupci s nazivima koje završavaju s "_value" sadrže prošireni podaci iz zbirke.

#### <a name="table-exampletablevtlist"></a>Tablica "ExampleTable_vt_List":
pk_int | List_index | List_value
------ | ---------- | ----------
1 | 0 | 1
1 | 1 | 2
1 | 2 | 3
3 | 0 | 100
3 | 1 | 101
3 | 2 | 102
3 | 3 | 103

#### <a name="table-exampletablevtmap"></a>Tablica "ExampleTable_vt_Map":
pk_int | Map_key | Map_value
------ | ------- | ---------
1 | S1 | ODGOVORA
1 | S2 | b
3 | S1 | t

#### <a name="table-exampletablevtstringset"></a>Tablica "ExampleTable_vt_StringSet":
pk_int | StringSet_value
------ | ---------------
1 | ODGOVORA
1 | B
1 | C
3 | ODGOVORA
3 | E





[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]
[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>Performanse i ugađanje  
Potražite u članku [Kopiranje aktivnosti performanse i vodič za ugađanje](data-factory-copy-activity-performance.md) dodatne informacije o ključa čimbenici koji utjecaj na performanse pomicanja podataka (Kopiraj aktivnosti) u tvorničke Azure podataka i razni načini optimizirati.
