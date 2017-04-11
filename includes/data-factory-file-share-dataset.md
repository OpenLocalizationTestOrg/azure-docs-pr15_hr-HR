## <a name="fileshare-dataset-type-properties"></a>Svojstva vrste dataset FileShare

Cijeli popis sekcija i svojstva dostupna za definiranje skupove podataka potražite u članku [Stvaranje skupova podataka](../articles/data-factory/data-factory-create-datasets.md) . Sekcija kao što su strukturu, dostupnost i pravila dataset JSON su slične za sve vrste skupa podataka. 

U odjeljku **typeProperties** razlikuje za svaku vrstu skupa podataka. Pruža informacije koje se odnose na vrstu skupa podataka. Odjeljak typeProperties za skup podataka vrste **FileShare** dataset sadrži sljedeća svojstva:

Svojstvo | Opis | Obavezno
-------- | ----------- | --------
folderPath | Sub put do mape. Koristiti prespojni znak ' \ ' za posebne znakove u nizu. Primjeri potražite [Uzorak povezana definicije web-mjesto servisa i u okvir za skup podataka](#sample-linked-service-and-dataset-definitions) .<br/><br/>To svojstvo, možete kombinirati s **partitionBy** da bi odsječak na temelju putevima mapa početak i kraj datuma-vremena. | Da
Naziv datoteke | Ako želite da se u tablici da biste se pozvali na određenom datotekom u mapi, navedite naziv datoteke u **folderPath** . Ako ne navedete bilo koja vrijednost za ovo svojstvo tablice upućuje na sve datoteke u mapi.<br/><br/>Kada se naziv datoteke za skup podataka za izlaz nije naveden, naziv datoteke generirani bio u nastavku ovaj oblik: <br/><br/>Podaci. <Guid>.txt (primjer: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt | ne
partitionedBy | partitionedBy se može koristiti da biste odredili dinamične folderPath naziv datoteke za vrijeme niza podataka. Na primjer, folderPath s parametrima za svaki sat podataka. | ne
Oblikovanje | Podržane su sljedeće vrste oblika: **TextFormat**, **AvroFormat**, **JsonFormat**i **OrcFormat**. Postavite svojstvo **Vrsta** u odjeljku oblik na jednu od ovih vrijednosti. [Određivanje TextFormat](#specifying-textformat), [Pri određivanju AvroFormat](#specifying-avroformat), [Određivanje JsonFormat](#specifying-jsonformat)i [Određivanje OrcFormat](#specifying-orcformat) sekcije detalje potražite u članku. Ako želite kopirati datoteke kao-je između utemeljenih na datotekama trgovine (binarni kopiranje), možete preskočiti u odjeljku oblik i definicije ulazni i izlazni skup podataka. | ne
fileFilter | Navedite filtar koji će se koristiti da biste odabrali podskup datoteka na folderPath umjesto sve datoteke.<br/><br/>Dopuštene su vrijednosti: `*` (više znakova) i `?` (pojedinačni znak).<br/><br/>Primjeri 1:`"fileFilter": "*.log"`<br/>Primjer 2:`"fileFilter": 2014-1-?.txt"`<br/><br/> fileFilter je primjenjivo za unos FileShare skup podataka. To svojstvo ne podržava HDFS.  | ne
| Sažimanje | Navedite vrstu i razina kompresije za podatke. Podržane vrste su: **GZip**, **Deflate**i **BZip2** i podržanim razinama: **Optimal** i **najbrže**. Trenutno postavki sažimanja nisu podržani za podatke u **AvroFormat** ili **OrcFormat**. Dodatne informacije potražite u članku [podrška za sažimanje](#compression-support) sekciji.  | ne |
| useBinaryTransfer | Odredite hoće li koristiti način prijenosa binarni. TRUE za binarni način rada i false ASCII. Zadana vrijednost: True. Ovo svojstvo može se koristiti samo kada je vrsta pridružene povezane usluge vrste: FtpServer. | ne | 
 

> [AZURE.NOTE] Naziv datoteke i fileFilter nije moguće koristiti istodobno.

### <a name="using-partionedby-property"></a>Pomoću svojstva partionedBy

Kao što je rečeno u prethodnom odjeljku, možete odrediti dinamičke folderPath naziv datoteke za vrijeme niza podataka pomoću partitionedBy. To možete učiniti pomoću makronaredbe tvorničke podataka i sustava varijablu SliceStart, SliceEnd koje označavaju logičke vremensko razdoblje podataka isječak. 

Da biste saznali više o skupova vrijeme niz podataka, Planiranje i isječke, potražite u članku [Stvaranje skupova podataka](../articles/data-factory/data-factory-create-datasets.md), [Raspored i izvršavanje](../articles/data-factory/data-factory-scheduling-and-execution.md)i [Stvaranje kanali](../articles/data-factory/data-factory-create-pipelines.md) članaka. 

#### <a name="sample-1"></a>Primjer 1:

    "folderPath": "wikidatagateway/wikisampledataout/{Slice}",
    "partitionedBy": 
    [
        { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
    ],

U ovom primjeru {odsječak} stupaca mijenja vrijednošću varijable tvorničke podataka sustava SliceStart u obliku (YYYYMMDDHH) naveden. U SliceStart se odnosi na početak isječak. Na folderPath razlikuje za svaki isječak. Primjer: wikisampledataout/wikidatagateway/2014100103 ili wikisampledataout/wikidatagateway/2014100104.

#### <a name="sample-2"></a>Primjer 2:

    "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
    "fileName": "{Hour}.csv",
    "partitionedBy": 
     [
        { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
    ],

U ovom primjeru u zasebnom varijabli koje koriste folderPath i naziv svojstva se izdvajaju godini, mjesecu, dan i vrijeme SliceStart.
