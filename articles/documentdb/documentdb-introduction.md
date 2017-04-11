<properties 
    pageTitle="Uvod u DocumentDB, JSON baze podataka | Microsoft Azure" 
    description="Saznajte više o Azure DocumentDB, NoSQL JSON baze podataka. Ova baza podataka dokument se temelji velikih skupova podataka, elastic skalabilnost i visoke dostupnosti." 
    keywords="JSON baze podataka, a zatim dokument baze podataka"
    services="documentdb" 
    authors="mimig1" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="09/13/2016" 
    ms.author="mimig"/>

# <a name="introduction-to-documentdb-a-nosql-json-database"></a>Uvod u DocumentDB: NoSQL JSON baze podataka

##<a name="what-is-documentdb"></a>Što je DocumentDB?

DocumentDB je potpuno Upravljani servis NoSQL baze podataka za brz i predvidljivi performanse, visoke dostupnosti, elastic skaliranje, globalni raspodjele i jednostavnog razvoja. Kao sheme slobodno NoSQL baze podataka, DocumentDB nudi obogaćenog poznatih SQL upita mogućnosti i s dosljedan malo latencies na podacima JSON - jamči da su poslužena 99% vaše čitanja u odjeljku 10 milisekundama i 99% vaše zapisivanja su poslužena u odjeljku 15 milisekundi. Te provjerite jedinstveni pogodnosti DocumentDB na odlično stane za web-mjesto, mobilnim, igraće, i IoT i mnoge druge aplikacije koje su potrebne objedinjenog mjerilo, a globalni replikacije.

## <a name="how-can-i-learn-about-documentdb"></a>Kako mogu saznati više o DocumentDB? 

Brz način Saznajte više o DocumentDB i pogledajte ga na djelu tako da slijedite korake u nastavku tri: 

1. Pogledajte na dvije minute [što je DocumentDB?](https://azure.microsoft.com/documentation/videos/what-is-azure-documentdb/) videozapisa, koja predstavlja prednosti korištenja DocumentDB.
2. Pogledajte na tri minute [Stvaranje DocumentDB na Azure](https://azure.microsoft.com/documentation/videos/create-documentdb-on-azure/) videozapisa, koji se ističe upute za početak rada s DocumentDB pomoću portala za Azure.
3. Posjetite na [Playground upita](http://www.documentdb.com/sql/demo), gdje možete voditi kroz različite aktivnosti dodatne informacije o obogaćenim upite funkcije dostupne u DocumentDB. Nakon toga glavni pokazivač na karticu s memorijom za testiranje i pokretanje vlastite prilagođene SQL upita i Eksperimentirajte s DocumentDB.

Zatim, vratite se u ovom se članku gdje ćemo ćete pristupili.  

## <a name="what-capabilities-and-key-features-does-documentdb-offer"></a>Koje mogućnosti i ključne značajke DocumentDB nudi sustav?  

Azure DocumentDB nudi sljedeće ključne značajke i pogodnosti:

-   **Elastically skalabilni propusnost i pohranu:** Jednostavno proširenja ili skaliranje dolje DocumentDB JSON baze podataka da bi odgovarao vašim potrebama aplikacije. Vaši podaci spremljeni na pune stanje diskova (SSD) za manje predvidljivi latencies. DocumentDB podržava spremnika za spremanje podataka JSON naziva zbirke koje se mogu mijenjati veličinu veličine gotovo neograničeno prostora za pohranu i dodijeljenu propusnost. Možete elastically promijenite li DocumentDB s performansama predvidljivi jednostavno kao rastom aplikacije. 

-   **Replikacije regije:** DocumentDB proziran replicira podataka za sve regije koje ste povezali s računom DocumentDB omogućujući vam za razvoj aplikacija koje je potrebno globalni pristup podacima tijekom pružanja tradeoffs između dosljednost, dostupnost i performanse, sve s odgovarajućih jamstva. DocumentDB sadrži prozirne regionalnih prebacivanje s većim brojem homing API-ji i mogućnost elastically mjerilo propusnost i pohranu diljem svijeta. Saznajte više u [distribucija podataka globalno pomoću DocumentDB](documentdb-distribute-data-globally.md).

-   **Ad hoc upite s poznatih SQL sintaksa:** Pohrana heterogenih JSON dokumenata u DocumentDB i te dokumente putem poznatih SQL sintaksa za upite. DocumentDB koristi iznimno Istodobni lock besplatni, zapisnika strukturiranim indeksiranja tehnologije automatski indeksirati sav sadržaj dokumenta. Time se omogućuje obogaćenog u stvarnom vremenu upita bez potrebe za određivanje sheme savjete, sekundarne ili prikazima. Dodatne informacije u [DocumentDB upita](documentdb-sql-query.md). 

-   **Izvršavanje JavaScripta baze podataka:** Izrazite logike aplikacije kao pohranjene procedure, okidača i korisnički definirane funkcije (UDF-ove) pomoću standardnih JavaScript. Time se omogućuje logiku aplikacije funkcioniranje iznad podataka bez brige o nepodudaranje između aplikacija i shemu baze podataka. DocumentDB nudi potpuni transakcijskih izvođenja logike aplikacije JavaScript izravno unutar modul baze podataka. Precizno Integracija JavaScript omogućuje izvršavanje Umetanje, ZAMIJENI, brisanje i odaberite operacija iz programa JavaScript kao Izolirani transakcije. Dodatne informacije u [DocumentDB poslužiteljsko programiranje](documentdb-programming.md).

-   **Tunable dosljednost razine:** Odabir od četiri razine dobro definiran dosljednost postigli optimalne trade-off između dosljednost i performanse. Za upite i operacije čitanja DocumentDB nudi četiri razine distinct dosljednost: istaknuti, bounded-staleness, sesije i usmjerenog. Ove razine zrnastog, dobro definiranom dosljednost omogućuju vam da zvuk gubitke između dosljednost, dostupnost i Latencija. Dodatno se informirajte korištenja [dosljednost razine Maksimiziranje dostupnosti i performanse DocumentDB](documentdb-consistency-levels.md).

-   **Potpuno upravlja:** Uklonite potrebe za upravljanje resursima baze podataka i računala. Kao potpuno upravlja Microsoft Azure servis, ne morate upravljanje virtualnim strojevima, implementirati i konfigurirati softver, upravljanje skaliranje ili baviti nadogradnje složenih podataka sloja. Svaki baze podataka automatski sigurnosnu kopiju i zaštićene protiv regionalne pogreške. Možete jednostavno dodajte račun DocumentDB i dodjela kapaciteta vam je potrebno, što omogućuje fokusiranje na aplikaciju umjesto operacijskom i upravljanje bazu podataka. 

-   **Otvorilo dizajna:** Brzo počnite s radom pomoću svoje postojeće znanje i alati. Programiranje protiv DocumentDB je jednostavno, dostupnim i nije potrebno prihvaćaju novih alata ili drži prilagođene proširenja JSON ili JavaScript koda. Sve funkcije baze podataka obuhvaća CRUD, upita i JavaScript obrade putem jednostavne RESTful HTTP sučelja mogu pristupiti. DocumentDB embraces postojeća oblikovanja, jezika i Standardi tijekom koja nudi visoke vrijednosti mogućnosti baze podataka pri vrhu ih.

-   **Automatsko indeksiranje:** Prema zadanim postavkama DocumentDB [automatski indeksi](documentdb-indexing.md) sve dokumente u bazi podataka i ne očekujete ili potrebna sheme ili stvaranje sekundarne indeksi. Ne želite da sve indeksirati? Ne brinite, to možete [odustajanje od putova u datotekama sustava JSON](documentdb-indexing-policies.md) .

##<a name="data-management"></a>Kako DocumentDB upravljanje podacima?

Azure DocumentDB upravlja JSON podataka putem dobro definiranom baze podataka resursa. Ove resurse su replicirati za visoke dostupnosti i se jedinstveno adresirati prema svojim logičke URI. DocumentDB nudi jednostavne HTTP temelji RESTful model programiranja za sve resurse. 

Račun za bazu podataka DocumentDB je jedinstveni prostor za naziv koji omogućuje vam pristup Azure DocumentDB. Da biste mogli stvarati račun baze podataka, morate imati Azure pretplatu, koji omogućuje pristup različite Azure usluge. 

Svi resursi unutar DocumentDB katalog modeliran i pohranjenih kao JSON dokumenata. Resursima se upravlja kao stavke, koji su JSON dokumente koji sadrže metapodataka i kao koja su zbirke stavki sažetaka sadržaja. Skupova stavki nalazi se unutar svoje odgovarajući RSS.

Na donjoj slici prikazano odnose između DocumentDB resurse:

![Hijerarhijski odnos između resursa u DocumentDB, NoSQL JSON baze podataka][1] 

Račun za baze podataka sastoji se od skupa baze podataka, svaki koja sadrži više zbirki, od kojih svaki može sadržavati pohranjene procedure, okidača, UDF-ove, dokumenata i povezanih privici. Baze podataka sadrži povezane korisnika, svaka s skup dozvola za pristup različite druge zbirke, pohranjene procedure, okidača, UDF-ove, dokumenti ili privici. Dok baze podataka, korisnici, dozvole i zbirki sustava definirane resursi poznati sheme - sadrže proizvoljne dokumenata, pohranjene procedure, okidača, UDF-ove i privitaka, korisnički definirane JSON sadržaj.  

##<a name="develop"></a>Kako razvijati aplikacije s DocumentDB?

Azure DocumentDB izlaže resursi kroz REST API-JA koje možete pozvati tako da možete upućivanje zahtjeva za HTTP/HTTPS bilo koji jezik. Uz to, DocumentDB nudi programiranje biblioteke za nekoliko Popularni jezika. Te biblioteke Pojednostavnite brojne aspekte rad s Azure DocumentDB obradom detalje kao što su adresa predmemoriranja, upravljanje iznimku, automatsko ponovne pokušaje itd. Biblioteka su trenutno dostupne za sljedeće jezike i platforme:  

Preuzimanje | Dokumentacija
--- | ---
[.NET SDK](http://go.microsoft.com/fwlink/?LinkID=402989) | [Biblioteka za .NET](https://msdn.microsoft.com/library/azure/dn948556.aspx)
[Node.js SDK](http://go.microsoft.com/fwlink/?LinkID=402990) | [Biblioteka Node.js](http://azure.github.io/azure-documentdb-node/)
[Java SDK](http://go.microsoft.com/fwlink/?LinkID=402380) | [Biblioteka Java](http://azure.github.io/azure-documentdb-java/)
[JavaScript SDK](http://go.microsoft.com/fwlink/?LinkID=402991) | [Biblioteka JavaScript](http://azure.github.io/azure-documentdb-js/)
n/d | [Poslužiteljsko JavaScript SDK](http://azure.github.io/azure-documentdb-js-server/)
[Python SDK](https://pypi.python.org/pypi/pydocumentdb) | [Python biblioteke](http://azure.github.io/azure-documentdb-python/)

Osnovni beyond stvaranje, čitanje, ažuriranje i brisanje operacije, DocumentDB omogućuje obogaćenog sučelje SQL upita za dohvaćanje JSON dokumenata i strani poslužitelja podrške za transakcijskih izvođenja logike aplikacije JavaScript. Sučelja izvršavanje upita i skripte dostupni su putem svim bibliotekama platforme, kao i REST API-ji. 

### <a name="sql-query"></a>SQL upit
Azure DocumentDB podržava upite dokumenata pomoću SQL jezika koji s je korijenom u JavaScript vrsta sustava i izrazima s podrškom za relacijske hijerarhijski i prostorno upite. Jezik upita DocumentDB je jednostavni, ali napredni sučelje za dokumente JSON upita. Jezik podržava podskup ANSI SQL gramatike i dodaje precizno Integracija JavaScript objekta, polja, konstrukciju objekta i funkcija poziva. DocumentDB sadrži njegov model upita bez sve eksplicitnih sheme ili indeksiranja savjeti proizvođača.

Korisnički definirane funkcije (UDF-ove) biti registriran u DocumentDB i navedena kao dio SQL upita time proširivanje gramatike za podršku logike prilagođenu aplikaciju. Ove UDF-ovi su ispisane JavaScript programa i izvršava unutar baze podataka. 

Za razvojne inženjere za .NET, DocumentDB nudi davatelja LINQ upita kao dio [.NET SDK](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.aspx). 

### <a name="transactions-and-javascript-execution"></a>Transakcije i izvođenja JavaScript
DocumentDB omogućuje vam pisanje logike aplikacije kao imenovani programi napisali u potpunosti JavaScript. Ti programi registrirane za zbirku i možete izdati postupaka baze podataka na dokumentima unutar određene zbirke. JavaScript može se registrirati za izvršavanje kao okidača, pohranjena procedura ili korisnički definirane funkcije. Okidača pohranjene procedure možete stvaranje, čitanje, ažuriranje i brisanje dokumenata dok korisnički definirane funkcije izvršavanje kao dio logiku izvršavanja upita bez pristup zapisivanju zbirke.

Izvršavanje JavaScripta unutar DocumentDB je katalog modeliran nakon koncepata podržava sustavima relacijskih baza podataka, s JavaScript kao Moderna zamjenu za Transact-SQL. Sve JavaScript logike se izvršava unutar razine okolne ACID transakcija s odvajanja snimke. Tijekom njegova izvođenja, ako JavaScript throws iznimku, zatim cijelu transakcija je prekinuta.

## <a name="next-steps"></a>Daljnji koraci
Već imate račun za Azure? Zatim koje mogu započeti s DocumentDB [Azure Portal](https://portal.azure.com/#gallery/Microsoft.DocumentDB) tako da [stvorite račun za baze podataka DocumentDB](documentdb-create-account.md).

Nemate račun za Azure? možeš:

- Prijavite se na [Azure besplatnu probnu verziju](https://azure.microsoft.com/free/), koja vam daje 30 dana i $200 pokušati Azure services. 
- Ako imate pretplatu na MSDN, ispunjavate uvjete za [$150 u besplatne Azure kredita mjesečno](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) za korištenje na bilo koji servis za Azure. 

Zatim, kada budete spremni da biste saznali više, posjetite naš [tečaj](https://azure.microsoft.com/documentation/learning-paths/documentdb/) za navigaciju sve dostupne resurse učenje. 


[1]: ./media/documentdb-introduction/json-database-resources1.png
 
