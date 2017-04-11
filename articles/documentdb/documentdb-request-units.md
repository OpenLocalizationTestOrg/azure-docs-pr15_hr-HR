<properties 
    pageTitle="Zahtjev za jedinice u DocumentDB | Microsoft Azure" 
    description="Saznajte kako razumijevanje, odredite i procjenu zahtjev jedinica preduvjeti u DocumentDB." 
    services="documentdb" 
    authors="syamkmsft" 
    manager="jhubbard" 
    editor="mimig" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/29/2016" 
    ms.author="syamk"/>

#<a name="request-units-in-documentdb"></a>Zahtjev za jedinice u DocumentDB
Sad je dostupna: DocumentDB [Kalkulator jedinica zahtjev](https://www.documentdb.com/capacityplanner). Dodatne informacije u [Estimating mora vaše propusnost](documentdb-request-units.md#estimating-throughput-needs).

![Kalkulator propusnost][5]

##<a name="introduction"></a>Uvod
Ovaj članak sadrži pregled zahtjev jedinice u [Programu Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/). 

Kad pročitate članak ćete je moći odgovaraju na sljedeća pitanja:  

-   Što su zahtjev jedinice i zahtjev troškove?
-   Kako odrediti kapaciteta jedinica zahtjev za zbirku?
-   Kako odrediti mora jedinica zahtjev Moje aplikacije?
-   Što se događa ako kapacitet jedinica zahtjev za zbirku mogu biti dulji od?


##<a name="request-units-and-request-charges"></a>Zahtjev za jedinice i naknade za zahtjev
DocumentDB nudi brzo, predvidljivi performanse po *rezervirati* resurse zadovoljili vaše aplikacije propusnost mora.  Jer aplikacije učitavanje i pristup uzoraka promjena tijekom vremena, DocumentDB omogućuje jednostavno povećati ili smanjiti količinu rezervirane propusnost dostupne u aplikaciji.

S DocumentDB, rezervirane propusnost navedena je pomoću jedinice zahtjev za obradu sekundi.  Zahtjev za jedinice možete smatrati propusnost valutu, što znači *rezervirati* iznos od zajamčena jedinice zahtjev dostupne u aplikaciji na temelju po sekundi.  Svaku operaciju u DocumentDB - pisanje dokumenta, izvođenje upita, ažuriranja dokumenta - troši procesora i memorije IOPS.  To jest, svaka operacija uključuje *zahtjev trošak*izražena u *zahtjev za jedinice*.  Objašnjenje čimbenici koji utjecati na troškove jedinica zahtjev, uz vaše aplikacije propusnost preduvjeti, omogućuje vam da biste pokrenuli aplikaciju kao trošak učinkovito moguće. 

##<a name="specifying-request-unit-capacity"></a>Određivanje zahtjev jedinica kapaciteta
Prilikom stvaranja zbirke DocumentDB, navedite broj jedinica zahtjev sekundi (RUs) koji želite rezervirana mjesta za zbirku.  Nakon stvaranja zbirke Potpuna alokacija RUs naveden je rezervirana za korištenje u zbirci.  Svakoj zbirci uvijek je Namjenska i Izolirani propusnost karakteristike.  

Važno je da Imajte na umu da DocumentDB radi rezervacija model; To je se naplaćuju iznos propusnost *rezervirana* mjesta za zbirku, bez obzira na to koliko te propusnost je aktivno *koriste*.  Imajte na umu, međutim, koji će se kao učitavanja vaše aplikacije, podataka i korištenje uzoraka promjena jednostavno mogu mijenjati veličinu gore i dolje količinu rezervirana RUs putem DocumentDB SDK-ovi ili pomoću [Portala za Azure](https://portal.azure.com).  Dodatne informacije na skaliranje propusnost prema gore ili dolje potražite u članku [DocumentDB performanse razine](documentdb-performance-levels.md).

##<a name="request-unit-considerations"></a>Zahtjev za pitanja vezana uz jedinica
Određivanje broj jedinica zahtjev rezervirati zbirke DocumentDB, važno je sljedeće varijable uzeti u obzir:

- **Veličina dokumenta**. Kao što je dokument veličine povećati jedinice potrošena za čitanje ili pisanje i povećava podatke.
- **Broj svojstva dokumenta**. Pod pretpostavkom zadani indeksiranjem sva svojstva, jedinice potrošena pisati dokumenta povećava se kao povećava broj svojstva.
- **Dosljednost podataka**. Prilikom korištenja razine dosljednost podataka Strong ili Bounded Staleness dodatnih jedinica će se utrošiti čitanje dokumenata.
- **Svojstva indeksirano**. Pravilo indeksa na svakoj zbirci određuje svojstva koja su indeksirati prema zadanim postavkama. Vaš zahtjev jedinica potrošnje možete smanjiti ograničavanjem broja indeksirana svojstva ili omogućivanjem drži indeksiranja.
- **Indeksiranje dokumenta**. Po zadanom svaki dokument će se automatski indeksirati će zauzimaju manje jedinice zahtjeva Ako odlučite da ne želite indeksirati neka vaši dokumenti.
- **Uzorci upita**. Složenost upita utječe koliko jedinica zahtjev potrošiti za operaciju. Broj predikati, priroda predikata, projekcija, broj UDF-ove i veličina skupa izvora podataka sve utjecati trošak operacije upita.
- **Korištenje skripte**.  Kao i kod upita, pohranjene procedure i okidača zauzimaju zahtjev jedinice složenost postupaka koji se izvode na temelju. Kao što je razvoj aplikacija, provjeri u zaglavlju zahtjev obavijesti da biste bolje razumjeli kako svaka operacija koristi kapaciteta jedinica zahtjev.

##<a name="estimating-throughput-needs"></a>Procjena potreba propusnost
Jedinica zahtjev je mjera normaliziranu zahtjeva za obradu trošak. Jedan zahtjev jedinica predstavlja kapaciteta obrada potrebna za čitanje (putem prošao na samostalnom vezu ili ID-ja) jedan dokument JSON 1KB koji se sastoji od 10 svojstvo jedinstvene vrijednosti (bez svojstva sustava). Zahtjev za stvaranje (Umetanje), zamjena ili brisanje isti dokument zauzeti će dodatne obrade iz servisa i time više jedinica zahtjev.   

> [AZURE.NOTE] Osnovne jedinice 1 zahtjev za dokument 1KB odgovara da biste jednostavno VIDJELI prošao na samostalnom vezu ili id dokumenta.

###<a name="use-the-request-unit-calculator"></a>Korištenje jedinica Kalkulator zahtjev
Da biste lakše kupci precizno podešavanje njihove Procjena propusnost, postoji web koji se temelji [Kalkulator jedinica zahtjev](https://www.documentdb.com/capacityplanner) za procjenu preduvjeti jedinica zahtjev za uobičajene operacije, uključujući:

- Dokument stvara (zapisivanje)
- Čitanje dokumenta
- Brisanje dokumenta
- Ažuriranja dokumenta

Alat za obuhvaća i podršku za Procjena podataka za pohranu potrebama oglednih dokumenata unesete na temelju.

Pomoću alata za je jednostavno:

1. Prijenos jednog ili više predstavniku JSON dokumenata.

    ![Prijenos dokumenata na kalkulator jedinica zahtjev][2]

2. Za procjenu potrebama za pohranu podataka, unesite ukupan broj dokumenata koje će se spremati.

3. Unesite broj dokumenta stvaranje, čitanje, ažuriranje i brisanje operacije zahtijevate (na temelju po sekundi). Za procjenu naknada jedinica zahtjev operacija ažuriranja dokumenta, prenesite kopiju dokumenta uzorak iz korak 1 koja obuhvaća ažuriranja uobičajena polja.  Ako, na primjer, ako ažuriranja dokumenta obično izmijenili dva svojstva pod nazivom lastLogin i userVisits, zatim jednostavno kopirajte uzorak dokumenta, ažuriranje vrijednosti za ta dva svojstva i preuzmite kopirani dokument.

    ![Unesite preduvjeti za propusnost u jedinica Kalkulator zahtjev][3]

4. Kliknite izračun i pregledajte rezultate.

    ![Zahtjev za jedinica Kalkulator rezultata][4]

>[AZURE.NOTE]Ako imate vrste dokumenata koje će znatno razlikovati veličina i broj indeksirana svojstva, prijenos uzorak svaku *vrstu* uobičajeni dokumenta alata i izračunavati rezultate.

###<a name="use-the-documentdb-request-charge-response-header"></a>Korištenje oznaka DocumentDB zahtjev trošak
Svaki odgovor servisa DocumentDB sadrži prilagođeno zaglavlje (x-ms-zahtjev-trošak) koji sadrži jedinice zahtjev za zahtjev. Putem DocumentDB SDK-ovi dostupan je i tog zaglavlja. U .NET SDK RequestCharge je svojstvo ResourceResponse objekta.  Za upite, DocumentDB Explorer upita na portalu za Azure sadrži informacije o zahtjevu za upite koji se mogu izvršiti.

![Provjera Pravi troškove u programu Explorer upita][1]

Ovime umu, način Procjena količinu rezervirane propusnost potrebnih aplikacije bilježenje je trošak jedinice zahtjev pridruženi pokrenutim uobičajene operacije protiv predstavniku dokument koristi aplikacija, a zatim Procjena broj operacija će se izvođenje svaki sekundi.  Obavezno za mjerenje i standardne upita i DocumentDB kao i korištenje skripte.

>[AZURE.NOTE]Ako imate vrste dokumenata koje će znatno razlikovati veličina i broj indeksirana svojstva, zapis trošak jedinice zahtjev primjenjivo operacija povezan sa svakom *Vrsta* uobičajeni dokumenta.

Ako, na primjer:

1. Snimanje jedinični trošak zahtjev stvaranja (Umetanje) uobičajeni dokumenta. 
2. Zapis jedinični trošak zahtjev za uobičajeni dokumenta za čitanje.
3. Jedinični trošak zahtjev za ažuriranja uobičajeni dokumenta zapis.
3. Zapis trošak jedinice zahtjev za uobičajene, zajednički dokument upita.
4. Snimanje zahtjev jedinica troška od prilagođene skripte (pohranjene procedure okidača, korisnički definirane funkcije) leveraged aplikacija
5. Izračunajte jedinica potrebna zahtjev Procijenjena broj operacija će se pokrenuti svaki drugi.

##<a name="a-request-unit-estimation-example"></a>Budući da se primjeru procjeni jedinica zahtjev
Imajte na umu sljedeće ~ 1KB dokumenta:

    {
     "id": "08259",
    "description": "Cereals ready-to-eat, KELLOGG, KELLOGG'S CRISPIX",
    "tags": [
        {
        "name": "cereals ready-to-eat"
        },
        {
        "name": "kellogg"
        },
        {
        "name": "kellogg's crispix"
        }
    ],
    "version": 1,
    "commonName": "Includes USDA Commodity B855",
    "manufacturerName": "Kellogg, Co.",
    "isFromSurvey": false,
    "foodGroup": "Breakfast Cereals",
    "nutrients": [
        {
        "id": "262",
        "description": "Caffeine",
        "nutritionValue": 0,
        "units": "mg"
        },
        {
        "id": "307",
        "description": "Sodium, Na",
        "nutritionValue": 611,
        "units": "mg"
        },
        {
        "id": "309",
        "description": "Zinc, Zn",
        "nutritionValue": 5.2,
        "units": "mg"
        }
    ],
    "servings": [
        {
        "amount": 1,
        "description": "cup (1 NLEA serving)",
        "weightInGrams": 29
        }
    ]
    }

>[AZURE.NOTE]Dokumenti se minified u DocumentDB, tako da sustav izračunava veličina iznad dokumenta je malo manja od 1KB.


Sljedeća tablica prikazuje djelomičnog zahtjev jedinica naknade za uobičajene operacije na ovaj dokument (jedinični trošak djelomičnog zahtjev podrazumijeva razinu dosljednost račun postavljen "Sesiju" te se automatski indeksirati sve dokumente):

Postupak|Zahtjev za jedinični trošak 
---|---
Stvaranje dokumenta|~ 15 PRAVI 
Čitanje dokumenta|~ 1 PRAVI
Dokument upita prema ID-a|~2.5 PRAVI

Osim toga, ova tablica prikazuje djelomičnog zahtjev jedinica naknade za uobičajene upiti koji se koriste u aplikaciji:

Upit|Zahtjev za jedinični trošak|# Vraćeni dokumenata
---|---|--- 
Odaberite hrane po ID-a|~2.5 PRAVI|1 
Odaberite hrane proizvođača|~ 7 PRAVI|7
Odaberite grupu za hranu i redoslijed Debljina|~ 70 PRAVI|100
Odaberite gornju 10 hrane u grupi Hrana|~ 10 PRAVI|10

>[AZURE.NOTE]Naknade za Pravi razlikuju se ovisno o broj dokumenata vratio.

Pomoću tih informacija smo možete procjenu Pravi preduvjeti za ovu aplikaciju dobili broj operacije i upiti očekivanog sekundi:

Operacija/upit|Procijenjena broj sekundi|Potrebne RUs 
---|---|--- 
Stvaranje dokumenta|10|150 
Čitanje dokumenta|100|100 
Odaberite hrane proizvođača|25|175 
Odaberite grupu za hranu|10|700 
Odaberite prvih 10|15|150 ukupni zbroj|155|1275

U ovom slučaju očekivanog je prosječna propusnost preduvjet 1,275 Pravi/s.  Zaokruživanje na najbliži 100 smo bi Dodjela 1,300 Pravi/s zbirke ovu aplikaciju.

##<a id="RequestRateTooLarge"></a>Prelazi rezervirane propusnost ograničenja
Opoziv zahtjev jedinica potrošnje procijeni kao stopa sekundi. Za aplikacije koje premašuju stopu jedinica Dodjela resursa zahtjev za zbirku zahtjevi za tu zbirku će biti ograničio vrijeme dok stopu ispod rezervirane razinu. Kada se pojavi u postavci, poslužitelj će preemptively završiti zahtjev s RequestRateTooLargeException (HTTP Šifra stanja 429) i vratiti zaglavlje x-ms-Ponovi-nakon-ms koji označava vrijeme u milisekundama, korisnik mora čekati prije reattempting zahtjev.

    HTTP Status 429
    Status Line: RequestRateTooLarge
    x-ms-retry-after-ms :100

Ako koristite SDK .NET klijenta i LINQ upita, a zatim u većini slučajeva ih ne morate sami baviti iznimku, kao što je trenutna verzija klijenta SDK .NET implicitno dohvaća odgovor, poštuje poslužitelja naveden pokušaj nakon zaglavlje, a ponovnih pokušaja zahtjev. Ako vaš račun nije pristupa istovremeno više klijenata, sljedeći pokušaj uspio.

Ako imate više od jednog klijenta cumulatively radi iznad učestalost zahtjeva zadano ponašanje pokušaj možda suffice i klijent će vratiti DocumentClientException s Šifra stanja 429 aplikaciji. U slučajevima kao što su ova, razmotrite rukovanja pokušaj ponašanje i logike u aplikaciju sustava Pogreška rukovanja postupke ili povećati propusnost rezervirana mjesta za zbirku.

##<a name="next-steps"></a>Daljnji koraci

Da biste saznali više o rezervirane propusnost s bazama podataka Azure DocumentDB, proučite ove resurse:
 
- [Cijene DocumentDB](https://azure.microsoft.com/pricing/details/documentdb/)
- [Upravljanje DocumentDB kapaciteta](documentdb-manage.md) 
- [Modeliranje podataka u DocumentDB](documentdb-modeling-data.md)
- [Razine DocumentDB performansi](documentdb-partition-data.md)

Dodatne informacije o DocumentDB potražite u članku Azure DocumentDB [dokumentacije](https://azure.microsoft.com/documentation/services/documentdb/). 

Početak rada s promjenom veličine i performanse testiranjem DocumentDB, potražite u članku [performanse i skaliranje testiranjem Azure DocumentDB](documentdb-performance-testing.md).


[1]: ./media/documentdb-request-units/queryexplorer.png 
[2]: ./media/documentdb-request-units/RUEstimatorUpload.png
[3]: ./media/documentdb-request-units/RUEstimatorDocuments.png
[4]: ./media/documentdb-request-units/RUEstimatorResults.png
[5]: ./media/documentdb-request-units/RUCalculator2.png
