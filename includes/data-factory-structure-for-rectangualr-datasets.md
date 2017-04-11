## <a name="specifying-structure-definition-for-rectangular-datasets"></a>Određivanje strukture definiciju pravokutni skupova podataka
U odjeljku strukturu skupove podataka JSON je **neobavezna** sekcija za pravokutni tablice (s reci i stupci) i sadrži zbirku stupaca tablice. U odjeljku struktura će se koristiti za neki providing informacije o vrsti za pretvorbe vrsta ili način mapiranja stupaca. U sljedećim odjeljcima opisuju te značajke detaljno. 

Svaki se stupac sadrži sljedeća svojstva:

| Svojstvo | Opis | Obavezno |
| -------- | ----------- | -------- |
| ime | Naziv stupca. | Da |
| Vrsta | Vrsta podataka u stupcu. Potražite sekciji pretvorbe vrsta ispod za dodatne detalje o vezi kada treba navedete informacije o vrsti | ne |
| Kultura | .NET temelji kulture će se koristiti prilikom vrsta navedena je i je li .NET vrsta Datetime ili Datetimeoffset. Zadana vrijednost je "en-us".  | ne |
| Oblikovanje | Oblikovanje niza koja će se koristiti kada vrsta navedena je i je li .NET vrsta Datetime ili Datetimeoffset. | ne |

Sljedeći primjer prikazuje odjeljak strukturu JSON za tablicu koja sadrži tri stupca ID korisnika, naziv i lastlogindate.

    "structure": 
    [
        { "name": "userid"},
        { "name": "name"},
        { "name": "lastlogindate"}
    ],

Koristite sljedeće smjernice za kada obuhvatiti informacije "struktura" i što će se uvrstiti u odjeljku **strukturu** .

- **Za strukturirane izvore podataka** kojima se pohranjuju sheme i vrstu podataka s podacima sam (izvora kao što je tablica platforme Azure SQL Server, Oracle, itd.), navedite u odjeljku "struktura" u samo ako želite učiniti mapiranja stupca određenog izvora stupaca na određene stupce u primatelj i njihova imena nisu jednake (detalje potražite u odjeljku mapiranje stupca ispod). 

    Kao što je rečeno iznad, informacije o vrsti nije obavezno u odjeljku "struktura". Strukturirane izvore informacija o vrsti već je dostupna kao dio definicije skup podataka u spremištu podataka, tako da ne smiju informacije o vrsti kad uključite sekciji "struktura".
- **Za shemu na izvora podataka za čitanje (posebno blobova platforme Azure)** možete odabrati da biste pohranili podatke bez pohrane sheme ili vrste podataka s podacima. Za sljedeće vrste izvora podataka mora uključiti "struktura" u sljedećim slučajevima 2:
    - Želite učiniti mapiranja stupca.
    - Kada je skup podataka izvora u kopiju aktivnosti, možete unijeti informacije o vrsti "strukture" i tvorničke podataka će koristiti te upišite podatke za pretvorbu nativni vrste primatelj. Članak [Premještanje podataka i iz blobova platforme Azure](../articles/data-factory/data-factory-azure-blob-connector.md) potražite u članku dodatne informacije.

### <a name="supported-net-based-types"></a>Podržane. Neto utemeljene vrste 
Tvorničke podataka podržava sljedeće CLS usklađen .NET temelji vrsta vrijednosti za pruža informacije o vrsti "strukture" za shemu na dodatne izvore kao što su blobova platforme Azure.

- Int16
- Int32 
- Int64
- Jedan
- Dvaput
- Broja decimalnih mjesta
- Bajt]
- Booleovom
- Niz 
- GUID
- Datum i vrijeme
- Datetimeoffset
- Vremenski raspon 

Za Datetime & Datetimeoffset i po želji možete navesti niz "kulture" i "Oblikovanje" da biste olakšali Raščlanjivanje vaše prilagođen niz datuma i vremena. U odjeljku uzorka za pretvaranje vrste ispod.

