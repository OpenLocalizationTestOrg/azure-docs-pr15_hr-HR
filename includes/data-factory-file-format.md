## <a name="specifying-formats"></a>Određivanje oblika

Azure tvorničke podataka podržava sljedeće vrste oblika: 

- [Oblikovanje teksta](#specifying-textformat)
- [JSON OSNOVNI oblik](#specifying-jsonformat)
- [Oblikovanje Avro](#specifying-avroformat)
- [Oblikovanje ORC](#specifying-orcformat)
- [Oblikovanje parquet](#specifying-parquetformat)

### <a name="specifying-textformat"></a>Određivanje TextFormat

Ako oblika postavljena na **TextFormat**, možete odrediti sljedeće **neobavezno** svojstva u odjeljku **oblik** .

| Svojstvo | Opis | Dopuštena vrijednost | Obavezno |
| -------- | ----------- | -------- | -------- | 
| columnDelimiter | Znak koji se koristi za razdvajanje stupaca u datoteci. | Samo jedan znak je dopušteno. Zadana vrijednost je zarez (','). <br/><br/>Da biste koristili Unicode znak, pogledajte [Unicode znakova](https://en.wikipedia.org/wiki/List_of_Unicode_characters) da biste dobili odgovarajuće kod. Na primjer, razmotrite korištenje rijetko koja se ne ispisuju znak koji vjerojatno ne postoje u vašim podacima: Navedite "\u0001" koji predstavlja počnite od naslov (SOH). | ne |
| rowDelimiter | Znak koji se koristi da biste razdijelili retke u datoteci. | Samo jedan znak dopuštena. Zadana je vrijednost bilo koji od sljedećih vrijednosti na čitanje: ["\r\n", "\r", "\n"] i "\r\n" na unos. | ne |
| escapeChar | Posebni znak za escape razdjelnika stupca u sadržaju ulazne datoteke. <br/><br/>Ne možete navesti escapeChar i quoteChar za tablicu. | Samo jedan znak dopuštena. Nema zadanu vrijednost. <br/><br/>Primjer: Ako imate zarezom (', ') kao graničnik stupaca, ali želite da se znak zareza u tekstu (primjer: "Zdravo, svijete"), možete definirati '$' kao prespojni znak i koristite niz "$Zdravo, svijete" u izvorišnog web-mjesta. | ne | 
| quoteChar | Znak koji se koristi za ponudu vrijednost niza. Graničnika stupaca i redaka u ponudu znakove će se smatrati dio vrijednost niza. Ovo svojstvo se može primijeniti ulazni i izlazni skupovima podataka.<br/><br/>Ne možete navesti escapeChar i quoteChar za tablicu. | Samo jedan znak je dopušteno. Nema zadanu vrijednost. <br/><br/>Na primjer, ako imate zarezom (', ') kao graničnik stupca, ali želite da se znak zareza u tekstu (primjer: < Zdravo, svijete >), možete definirati "(dvostruke navodnike) kao znaka ponude i korištenje niz"Zdravo, svijete"u izvoru. | ne |
| nullValue | Jedan ili više znakova koji predstavlja vrijednost null. | Jedan ili više znakova. Zadane su vrijednosti "\N" i "NULL" na čitanje i "\N" na unos. | ne |
| encodingName | Navedite naziv kodiranja. | Valjani naziv kodiranja. u odjeljku [Svojstva Encoding.EncodingName](https://msdn.microsoft.com/library/system.text.encoding.aspx). Primjer: windows 1250 ili shift_jis. Zadana vrijednost je UTF-8. | ne | 
| firstRowAsHeader | Određuje hoće li se u obzir prvi redak kao zaglavlja. Za unos skupa podataka podataka tvorničke čita prvi redak kao zaglavlja. Za skup podataka za izlaz podataka tvorničke ispisuje prvi redak kao zaglavlja. <br/><br/>Ogledni scenariji potražite u članku [scenariji za korištenje **firstRowAsHeader** i **skipLineCount** ](#scenarios-for-using-firstrowasheader-and-skiplinecount) . | TRUE<br/>FALSE (zadano) | ne |
| skipLineCount | Označava broj redaka da biste preskočili prilikom čitanja podataka iz ulaznih datoteka. Ako su navedeni skipLineCount i firstRowAsHeader, najprije preskaču retke i podaci za zaglavlje je pročitajte iz ulazne datoteke. <br/><br/>Ogledni scenariji potražite u članku [scenariji za korištenje firstRowAsHeader i skipLineCount](#scenarios-for-using-firstrowasheader-and-skiplinecount) . | Cijeli broj | ne | 
| treatEmptyAsNull | Određuje hoće li se Smatraj null ili praznu vrijednost null prilikom čitanja podataka iz ulazne datoteke. | TRUE (zadano)<br/>FALSE | ne |  

#### <a name="textformat-example"></a>Primjer TextFormat
Sljedeći primjer prikazuje neka svojstva oblikovanje TextFormat.

    "typeProperties":
    {
        "folderPath": "mycontainer/myfolder",
        "fileName": "myblobname"
        "format":
        {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "rowDelimiter": ";",
            "quoteChar": "\"",
            "NullValue": "NaN"
            "firstRowAsHeader": true,
            "skipLineCount": 0,
            "treatEmptyAsNull": true
        }
    },

Da biste koristili programa escapeChar umjesto quoteChar, Zamijeni redak quoteChar sa sljedećim escapeChar:

    "escapeChar": "$",



### <a name="scenarios-for-using-firstrowasheader-and-skiplinecount"></a>Scenariji za korištenje firstRowAsHeader i skipLineCount

- Kopirate iz izvora koji nisu datoteke tekstne datoteke, a želite dodati zaglavlje retka koji sadrži metapodatke shema (na primjer: SQL shema). Odredite **firstRowAsHeader** kao true u izlazni skup podataka za taj scenarij. 
- Kopirate iz tekstne datoteke koja sadrži zaglavlje retka koji nisu datoteke primatelj, a želite ispustite tom retku. Odredite **firstRowAsHeader** kao true u unos skupu podataka.
- Kopirate iz tekstne datoteke, a želite preskočiti nekoliko redaka na početku koje sadrže nikakve informacije podataka ili zaglavlje. Navedite **skipLineCount** da biste označili željeni broj redaka treba. Ako je ostatak datoteka sadrži redak zaglavlja, možete odrediti i **firstRowAsHeader**. Ako su navedeni **skipLineCount** i **firstRowAsHeader** , najprije preskaču retke i podaci za zaglavlje je pročitajte iz ulaznog datoteke

### <a name="specifying-avroformat"></a>Određivanje AvroFormat
Ako je oblik postavljeno na AvroFormat, ne morate navesti svojstva u odjeljku oblik u odjeljku typeProperties. Primjer:

    "format":
    {
        "type": "AvroFormat",
    }

Da biste koristili oblik Avro u tablicu vrste Hive, možete se referirati [Apache vrste Hive, vodič](https://cwiki.apache.org/confluence/display/Hive/AvroSerDe).

Imajte na umu sljedeće točke:  

- [Vrstama složenih podataka](http://avro.apache.org/docs/current/spec.html#schema_complex) nisu podržani (zapisa, enums, polja, karte, unije i fiksno)

### <a name="specifying-jsonformat"></a>Određivanje JsonFormat

Ako oblik je postavljeno na **JsonFormat**, možete odrediti sljedeća **neobavezno** svojstva u odjeljku **oblik** .

| Svojstvo | Opis | Obavezno |
| -------- | ----------- | -------- |
| filePattern | Određivanje uzorka podataka pohranjenih u svakoj datoteci JSON. Dopuštena su vrijednosti: **setOfObjects** i **arrayOfObjects**. **Zadana** vrijednost je: **setOfObjects**. Potražite u odjeljcima detalje o te uzoraka slijedeći.| ne |
| encodingName | Navedite naziv kodiranja. Na popisu valjani nazivi kodiranja, potražite u članku: [Encoding.EncodingName](https://msdn.microsoft.com/library/system.text.encoding.aspx) svojstvo. Na primjer: windows 1250 ili shift_jis. **Zadana** vrijednost je: **UTF-8**. | ne | 
| nestingSeparator | Znak koji se koristi za razdvajanje razina gniježđenja. Zadana je vrijednost "." (točka). | ne | 


#### <a name="setofobjects-file-pattern"></a>Uzorak setOfObjects datoteka

Svaka datoteka sadrži jedan objekt ili više crta-razgraničeno/spajaju objekte. Kada je ta mogućnost odabrali u skup podataka za izlaz, Kopiraj aktivnosti daje jednu JSON datoteka uz svaki objekt po retku (redak zarezom).

**jedan objekt** 

    {
        "time": "2015-04-29T07:12:20.9100000Z",
        "callingimsi": "466920403025604",
        "callingnum1": "678948008",
        "callingnum2": "567834760",
        "switch1": "China",
        "switch2": "Germany"
    }

**redak razgraničeno JSON** 

    {"time":"2015-04-29T07:12:20.9100000Z","callingimsi":"466920403025604","callingnum1":"678948008","callingnum2":"567834760","switch1":"China","switch2":"Germany"}
    {"time":"2015-04-29T07:13:21.0220000Z","callingimsi":"466922202613463","callingnum1":"123436380","callingnum2":"789037573","switch1":"US","switch2":"UK"}
    {"time":"2015-04-29T07:13:21.4370000Z","callingimsi":"466923101048691","callingnum1":"678901578","callingnum2":"345626404","switch1":"Germany","switch2":"UK"}
    {"time":"2015-04-29T07:13:22.0960000Z","callingimsi":"466922202613463","callingnum1":"789037573","callingnum2":"789044691","switch1":"UK","switch2":"Australia"}
    {"time":"2015-04-29T07:13:22.0960000Z","callingimsi":"466922202613463","callingnum1":"123436380","callingnum2":"789044691","switch1":"US","switch2":"Australia"}

**ulančani JSON**

    {
        "time": "2015-04-29T07:12:20.9100000Z",
        "callingimsi": "466920403025604",
        "callingnum1": "678948008",
        "callingnum2": "567834760",
        "switch1": "China",
        "switch2": "Germany"
    }
    {
        "time": "2015-04-29T07:13:21.0220000Z",
        "callingimsi": "466922202613463",
        "callingnum1": "123436380",
        "callingnum2": "789037573",
        "switch1": "US",
        "switch2": "UK"
    }
    {
        "time": "2015-04-29T07:13:21.4370000Z",
        "callingimsi": "466923101048691",
        "callingnum1": "678901578",
        "callingnum2": "345626404",
        "switch1": "Germany",
        "switch2": "UK"
    }


#### <a name="arrayofobjects-file-pattern"></a>Uzorak datoteke arrayOfObjects. 

Svaka datoteka sadrži polja objekata. 

    [
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        },
        {
            "time": "2015-04-29T07:13:21.0220000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789037573",
            "switch1": "US",
            "switch2": "UK"
        },
        {
            "time": "2015-04-29T07:13:21.4370000Z",
            "callingimsi": "466923101048691",
            "callingnum1": "678901578",
            "callingnum2": "345626404",
            "switch1": "Germany",
            "switch2": "UK"
        },
        {
            "time": "2015-04-29T07:13:22.0960000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "789037573",
            "callingnum2": "789044691",
            "switch1": "UK",
            "switch2": "Australia"
        },
        {
            "time": "2015-04-29T07:13:22.0960000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789044691",
            "switch1": "US",
            "switch2": "Australia"
        },
        {
            "time": "2015-04-29T07:13:24.2120000Z",
            "callingimsi": "466922201102759",
            "callingnum1": "345698602",
            "callingnum2": "789097900",
            "switch1": "UK",
            "switch2": "Australia"
        },
        {
            "time": "2015-04-29T07:13:25.6320000Z",
            "callingimsi": "466923300236137",
            "callingnum1": "567850552",
            "callingnum2": "789086133",
            "switch1": "China",
            "switch2": "Germany"
        }
    ]

### <a name="jsonformat-example"></a>Primjer JsonFormat

Ako imate JSON datoteku s sljedećeg sadržaja:  

    {
        "Id": 1,
        "Name": {
            "First": "John",
            "Last": "Doe"
        },
        "Tags": ["Data Factory”, "Azure"]
    }

i koje želite kopirati u tablicu programa Azure SQL u sljedećem obliku: 

ID-a  | Name.First | Name.Middle | Name.Last | Oznaka
--- | ---------- | ----------- | --------- | ----
1 | Nevena | null | N. | ["Podataka tvorničke", "Azure"]

Unos dataset s vrstom JsonFormat definira na sljedeći način: (djelomična definicija s važne dijelove)

    "properties": {
        "structure": [
            {"name": "Id", "type": "Int"},
            {"name": "Name.First", "type": "String"},
            {"name": "Name.Middle", "type": "String"},
            {"name": "Name.Last", "type": "String"},
            {"name": "Tags", "type": "string"}
        ],
        "typeProperties":
        {
            "folderPath": "mycontainer/myfolder",
            "format":
            {
                "type": "JsonFormat",
                "filePattern": "setOfObjects",
                "encodingName": "UTF-8",
                "nestingSeparator": "."
            }
        }
    }

Ako struktura je definiran, aktivnosti Kopiraj poravnava strukturu po zadanom i kopirajte svaku stvar. 

#### <a name="supported-json-structure"></a>Podržani JSON strukture
Imajte na umu sljedeće točke: 

- Svaki objekt s zbirka parova naziv/vrijednost mapirano na jedan redak podataka u tabličnom obliku. Ugnježđivanje objekte, a možete odrediti kako stopi struktura u skup podataka s gniježđenja razdjelnik (.) prema zadanim postavkama. Pogledajte [primjer JsonFormat](#jsonformat-example) na prethodni sekcije, na primjer.  
- Ako je struktura nije definirano u skupu podataka tvorničke podataka, aktivnosti Kopiraj otkrije sheme prvi objekt i stopi cijeli objekt. 
- Ako je unos JSON polja, aktivnosti Kopiraj pretvara vrijednost cijelog polja u niz. Možete odabrati da biste preskočili pomoću [mapiranja stupca ili filtriranja](#column-mapping-with-translator-rules).
- Ako postoje dvostrukih naziva na istoj razini, aktivnosti Kopiraj izdvajanja posljednje.
- Svojstvo imena su velika i mala slova. Dva svojstva s istim nazivom, ali različite casings smatraju dva zasebna svojstva. 

### <a name="specifying-orcformat"></a>Određivanje OrcFormat
Ako je oblik postavljeno na OrcFormat, ne morate navesti svojstva u odjeljku oblik u odjeljku typeProperties. Primjer:

    "format":
    {
        "type": "OrcFormat"
    }

> [AZURE.IMPORTANT] Ako ne kopirate datoteke ORC **kao-je** između lokalnog i oblaka služi za pohranu podataka, morate instalirati JRE 8 (Java okruženje za izvođenje) na pristupnom računalu. Pristupnik za 64-bitne zahtijeva JRE 64-bitni i 32-bitni pristupnika zahtijeva JRE 32-bitni. Možete pronaći obje verzije s [ovdje](http://go.microsoft.com/fwlink/?LinkId=808605). Odaberite odgovarajući jedan.

Imajte na umu sljedeće točke:

-   Složenih podataka vrste nisu podržani (STRUCT, KARTE, popisa, UNIJE)
-   Datoteka ORC ima tri [mogućnosti vezanih uz komprimiranje](http://hortonworks.com/blog/orcfile-in-hdp-2-better-compression-better-performance/): NIŠTA, ZLIB, SNAPPY. Tvorničke podataka podržava čitanja podataka iz datoteke ORC u bilo kojem od ovih Komprimirana oblika. Koristi spajanja kodek je u metapodacima čitati podatke. Međutim, prilikom pisanja na datoteku ORC, tvorničke podataka odabire ZLIB, što je zadana za ORC. Trenutno ne postoji mogućnost da biste nadjačali takvo ponašanje. 

### <a name="specifying-parquetformat"></a>Određivanje ParquetFormat
Ako je oblik postavljeno na ParquetFormat, ne morate navesti svojstva u odjeljku oblik u odjeljku typeProperties. Primjer:

    "format":
    {
        "type": "ParquetFormat"
    }

> [AZURE.IMPORTANT] Ako ne kopirate datoteke Parquet **kao-je** između lokalnog i oblaka služi za pohranu podataka, morate instalirati JRE 8 (Java okruženje za izvođenje) na pristupnom računalu. Pristupnik za 64-bitne zahtijeva JRE 64-bitni i 32-bitni pristupnika zahtijeva JRE 32-bitni. Možete pronaći obje verzije s [ovdje](http://go.microsoft.com/fwlink/?LinkId=808605). Odaberite odgovarajući jedan.

Imajte na umu sljedeće točke:

-   Složenih podataka vrste nisu podržane (KARTA popisa)
-   Datoteka parquet ima sljedeće mogućnosti vezane uz komprimiranje: NIŠTA, SNAPPY, GZIP i LZO. Tvorničke podataka podržava čitanja podataka iz datoteke ORC u bilo kojem od ovih Komprimirana oblika. Sažimanje kodek koristi u metapodacima pročitati podatke. Međutim, prilikom pisanja Parquet datoteku, tvorničke podataka odabire SNAPPY, što je zadana za Parquet oblik. Trenutno ne postoji mogućnost da biste nadjačali takvo ponašanje. 
