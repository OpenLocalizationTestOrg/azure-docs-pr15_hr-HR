<properties 
    pageTitle="Uobičajeni DocumentDB pomoću slučajeva | Microsoft Azure" 
    description="Informirajte se o vrha pet namijenjen slučajeva DocumentDB: korisnik generira sadržaja, zapisivanje događaja, katalog podataka, korisničke preference podatke i Internet stvari (IoT)." 
    services="documentdb" 
    authors="h0n" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/28/2016" 
    ms.author="hawong"/>

# <a name="common-documentdb-use-cases"></a>Uobičajena slučaja koristi DocumentDB
Ovaj članak sadrži pregled nekoliko uobičajena slučaja koristi za DocumentDB.  Preporuke u ovom članku služe kao početnu točku kao razvoj aplikacija s DocumentDB.   

Kad pročitate članak ćete je moći odgovaraju na sljedeća pitanja: 
 
- Koji su uobičajeni slučajevi koristi za DocumentDB?
- Koje su prednosti korištenja DocumentDB kao korisnik generira sadržajem pohrana?
- Koje su prednosti korištenja DocumentDB kao katalog podataka pohraniti?
- Koje su prednosti korištenja DocumentDB kao zapisnik događaja pohraniti?
- Koje su prednosti korištenja DocumentDB kao korisničkih podataka preference pohraniti?
- Koje su prednosti korištenja DocumentDB kao izvor podataka za sustave Internet stvari (IoT)?

## <a name="common-use-cases-for-documentdb"></a>Uobičajena slučaja koristi za DocumentDB
Azure DocumentDB je opće namjene NoSQL baze podataka koja se koristi u širok raspon aplikacija i korištenje slučajeva. Je dobar izbor za bilo koju aplikaciju koja treba vremena niskog redoslijed-od-od milisekundi odgovor, a mora skaliranje informacija. Slijede neki atributi DocumentDB koje ga čine dobro prikladniji za aplikacije visokih performansi.

- DocumentDB nativno partitions podataka za visoke dostupnosti i skalabilnost.
- DocumentDB korisnika ima SSD sigurnosno prostora za pohranu uz vremena najniža Latencija redoslijed-od-od milisekundi odgovor.
- DocumentDB je podrška za dosljednost razine kao što su usmjerenog, sesije i bounded staleness omogućuje niskog trošak na performanse-omjera. 
- DocumentDB ima fleksibilne prikladna za podataka cijene modelu koji metar prostora za pohranu i propusnost neovisno.
- DocumentDB na rezervirane propusnost model omogućuje razmišljati broj čitanja/pisanja umjesto procesora i memorije/IOPs podlozi hardvera.
- Omogućuje dizajn DocumentDB na skaliranje zahtjev za pretraživanje velikog jedinicama obliku milijarde zahtjevi po danu.

Kada je riječ o web-mjesto mobilnim, igraće i IoT aplikacije koje je potrebno odgovor malo vremena i morate učiniti pretraživanje velikog količine čitanja i pisanja, osobito korisni su te atribute. 

## <a name="user-generated-content"></a>Korisnički generirani sadržaj
Uobičajena slučaja koristi za DocumentDB je za pohranu i korisnički upit generira sadržaja (UGC) za web i mobilnim aplikacijama, osobito društvenih mreža aplikacije.  Primjeri UGC su sesije razgovor, tweets, članaka za blog, ocjene i komentari.  Često UGC u aplikacijama za društvene mreže je blend prostoručni oblik teksta, svojstva, oznake i odnosa koji su bounded po čvrsta strukturu.   

Sadržaj kao što su razgovori, komentare i objave mogu spremiti u DocumentDB bez transformacije ili složene objekt relacijski mapiranje slojevima.  Svojstva podataka mogu dodati ili izmijeniti jednostavno u skladu potrebama, kao što je razvojni inženjeri ponavljanje kod aplikacije, stoga prosljeđivanja brz razvoj.  

Aplikacije koje se integrirati s raznim društvenim mrežama morate odgovoriti promjena sheme s njima.  Kao što je podataka će se automatski indeksirati prema zadanim postavkama u DocumentDB, podaci spremni može poslati upit u bilo kojem trenutku.  Dakle, te aplikacije imaju fleksibilnost za dohvaćanje projekcija u skladu sa svojim potrebama odgovarajuća.       

Mnoge društvenih aplikacija korištenja globalni mjerilo, a možete imati nepredviđeni upotrebe.  Fleksibilnosti pri skaliranje spremišta podataka ključan kao aplikacijskom sloju mijenja veličinu tako da odgovara zahtjev za korištenje.  Skaliranje jednu dodavanjem particije dodatne podatke u odjeljku DocumentDB račun.  Osim toga, dodatne račune DocumentDB možete stvoriti na više područja. Dostupnost područja za usluge DocumentDB potražite u članku [Azure područja](https://azure.microsoft.com/regions/#services).   

## <a name="catalog-data"></a>Katalog podataka
Scenariji za korištenje kataloga podataka obuhvaćaju spremanje i slanje upita skup atribute entiteti kao što su ljudi, mjesta i Proizvodi.  Primjeri katalog podataka su korisnički računi, katalozi, registries uređaj za IoT i računa sustava materijale.  Atributi tih podataka mogu se razlikovati, a možete promijeniti tijekom vremena da stane preduvjeti za aplikaciju.  

Na Samopogonjeni dijelovi dobavljača uzmite u obzir primjer kataloga proizvoda. Svaki dio može imati vlastitu atribute osim uobičajenih atributa koji svi dijelovi imaju.  Osim toga, atribute za određeni dio možete promijeniti sljedeće godine kada je izdala novi model.  Pohrana dokumenata JSON, DocumentDB podržava fleksibilne sheme i omogućuje vam da biste predstavili podatke s ugniježđenim svojstva te stoga je dobro prikladniji za spremanje podataka kataloga proizvoda.       

## <a name="logging-and-time-series-data"></a>Zapisivanje i vremenski niz podataka
Zapisivanje aplikacije često čuje u velike količine i možda imaju različiti atribute na temelju verzija distribuiranih aplikacije ili komponente zapisivanje događaja.  Podaci iz zapisnika nije bounded složenih odnosa ili čvrsta strukture. Sve, podaci iz zapisnika je ista i u obliku JSON jer je JSON laganih i lako ljudi za čitanje.
   
Obično se dva glavna koristi slučaja povezani podaci iz zapisnika događaja.  Prvi slučaj koristi je izvođenje ad hoc upite podskup podataka za otklanjanje poteškoća.  Tijekom otklanjanje poteškoća, podskup podataka najprije dohvaća iz zapisnika, obično po vremenski niz.  Nakon toga dubinske analize provodi filtriranjem skup podataka s razine pogreške i poruke o pogreškama. Ovo je gdje je spremanje zapisnika događaja u DocumentDB je. Podaci iz zapisnika pohranjene u DocumentDB automatski indeksirati prema zadanim postavkama i zato je spremna za može poslati upit u bilo kojem trenutku. Osim toga, podaci iz zapisnika možete se ista i preko particije podataka kao vremenski niz. Stariji zapisnika se može povući Hladno prostor za pohranu po pravilnika zadržavanja.          

Korištenje kutije drugi uključuje dugo izvođenje zadataka za analize podataka izvanmrežno izvršiti putem velik broj podaci iz zapisnika.  U ovom slučaju korištenje Primjeri komponente analysis dostupnost, analizu pogreške aplikacije i analiza podataka clickstream.  Obično Hadoop koristi se za izvođenje ove vrste analize.  S Hadoop Connector DocumentDB DocumentDB baza podataka funkcionirati kao izvora podataka i primatelji za Svinja, grozd i karte/smanjivanje zadatke. Detalje o Hadoop poveznik za DocumentDB potražite u članku [Pokreni Hadoop s DocumentDB i HDInsight](documentdb-run-hadoop-with-hdinsight.md).      

## <a name="gaming"></a>Igraće
Presudne komponenta igraće aplikacija je razina baze podataka. Moderna igre grafički obrada izvedete mobile/konzole klijentima, ali ovise o oblaka za isporuku prilagođenog i personalizirane sadržaja kao što je stat u utakmica, integracijom društvenih mreža i leaderboards visoke rezultat. Igre zahtijevaju iznimno malo latencies za čitanje zapisivanja možete unijeti na zanimljivih u – igra sučelje i razina baze podataka mora učiniti maksimalne i niskih ocjena učenika u zahtjev za tečajeve tijekom nove igre pokreće i ažuriranje značajki.

Koristi DocumentDB igre pretraživanje velikog mjerilo kao što su [u objašnjenje reagira: bez Muškarac sustava](https://azure.microsoft.com//blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/) prema [Sljedećem igre](http://www.nextgames.com/)i [Halo 5: skrbnicima](https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/). U oba pomoću slučajeva, ključnih prednosti DocumentDB su sljedeće:

- DocumentDB omogućuje performanse da biste se skalirana gore ili dolje elastically. Time se omogućuje igre učiniti ažuriranju profila i stat iz desetke milijune istodobno gamers tako da jedan API poziva.
- DocumentDB podržava od milisekundi čitanja i piše da biste izbjegli sve lags tijekom igre Reproduciraj.
- Automatsko indeksiranje DocumentDB, omogućuje za filtriranje prema više različitih svojstava u stvarnom vremenu, npr. Pronađite reproduktora njihove Interna reproduktora ID-ove, ili njihovim GameCenter, Facebook, Google ID-a ili upita na temelju reproduktora članstvu u guild. Ovo je moguće bez stvaranje složenih indeksiranja ili sharding infrastrukture.
- Značajke društvenih mreža uključujući poruku u utakmica razgovora, članstva guild reproduktora, izazove dovršiti, Visoko rezultat leaderboards i društvenih grafikona je jednostavnije implementaciju sa shemom fleksibilne.
- DocumentDB kao upravljanih platformu-kao-na-service (PaaS) potrebna minimalnog Postavljanje upravljanja rad da biste omogućili radi brzog iteracije i smanjiti vrijeme na tržištu.


## <a name="user-preferences-data"></a>Korisničke preference podatke
Danas, najčešće Moderna web i mobilne aplikacije imaju složene prikaza i sučelja. Ove prikaze i sučelja su obično dinamički, ugostiteljstvo korisničkih postavki ili moods i zaštitnih znakova potrebama.  Dakle, aplikacije moraju moći učinkovito dohvatiti osobne postavke da bi se brzo prikazivanje elemente korisničkog Sučelja i sučelja. 

JSON je učinkovitih oblikovanje da biste predstavili podatke izgleda korisničkog Sučelja kao što je samo nije laganih, ali također mogu se jednostavno prikazati tako da JavaScript.  DocumentDB nudi tunable dosljednost razine koji omogućuju brzo čitanja s niske latencije zapisivanja. Dakle, spremanje korisničkog Sučelja raspored podataka uključujući osobne postavke kao JSON dokumente u DocumentDB je učinkovitih znači da biste te podatke putem na žičani.

## <a name="iot-and-device-sensor-data"></a>Podaci o senzora IoT i uređaja
IoT korištenje slučajevima najčešće zajedničko korištenje neke uzorke kako ih ingest obradi te pohrane podataka.  Najprije te sustavima omogućuju intake podataka koje možete ingest bursts podataka iz senzori uređaj s različitim jezicima.  Nakon toga te sustavima obraditi i analize strujanja podataka za dobivanje glavne uvida u stvarnom vremenu. I zadnji ali ne najmanje Većina ako ne svi podaci će naposljetku krenuti u spremištu podataka za ad hoc upite i izvanmrežno analize.    

Microsoft Azure nudi obogaćenog servise koje možete leveraged za IoT pomoću slučajeva.  Servisa Azure IoT su postavljeni servisi uključujući Azure događaj koncentratora, Azure DocumentDB, Azure strujanje analize, Azure obavijesti koncentrator, Azure strojnog učenja, Azure HDInsight i PowerBI. 

Bursts podataka možete biti ingested po Azure događaj koncentratora kao nudi ingestion visoke protok podataka s niske latencije. Ingested potrebne podatke za obradu za uvid u stvarnom vremenu možete funneled za Azure strujanje Analytics za analizu u stvarnom vremenu. Podatke možete staviti u DocumentDB za ad hoc upite. Kada u DocumentDB učita je podatke, podaci spremni se.  Podaci u DocumentDB može se koristiti kao referentnih podataka kao dio analize u stvarnom vremenu. Uz to, podataka mogu dodatno poboljšali i obradili povezivanje podataka DocumentDB HDInsight za Svinja, grozd ili karte/smanjivanje zadatke.  Natrag na DocumentDB za izvješćivanje, zatim učitati dobili profinjeniji podataka.   

IoT uzorak rješenje pomoću DocumentDB, EventHubs i oluja, potražite u članku [hdinsight oluja Primjeri spremište na GitHub](https://github.com/hdinsight/hdinsight-storm-examples/).

Dodatne informacije o Azure ponude za IoT potražite u članku [Stvaranje Internet of Your stvari](http://www.microsoft.com/en-us/server-cloud/internet-of-things.aspx).

## <a name="next-steps"></a>Daljnji koraci
 
Za početak rada s DocumentDB, možete stvoriti [račun](https://azure.microsoft.com/pricing/free-trial/) i slijedite naš [tečaj](https://azure.microsoft.com/documentation/learning-paths/documentdb/) za dodatne informacije o DocumentDB i pronaći potrebne informacije. 

Ili, ako želite dodatne informacije o klijentima pomoću DocumentDB, sljedeće priča kupca dostupne su:

- [Sljedeći igre](https://azure.microsoft.com//blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/). Na Walking reagira: bez Muškarac Narodna sustava igre soars # 1 podržava Azure DocumentDB.
- [Halo](https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/). Kako Halo 5 implementirana društvenih gameplay pomoću Azure DocumentDB.
- [Galerija Cortana analize](https://azure.microsoft.com/blog/cortana-analytics-gallery-a-scalable-community-site-built-on-azure-documentdb/). Cortana analize Galerija – web-mjesta zajednice skalabilni utemeljena na Azure DocumentDB.
- [Povjetarac](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18602). Početnih Integrator daje multinacionalnoj rada globalni uvid u minutama s tehnologijama fleksibilne oblaka.
- [Republika novosti](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18639). Dodavanje obavještavanje novosti da biste dobili informacije s više nije potrebna za aktivan građana. 
- [Međunarodni SGS](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18653). Glavna marke za dosljedan boju diljem svijeta zadaće SGS. I SGS uključuje za Azure.
- [Telenor](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18608). Globalni Vodeća crta Telenor koristi oblaka da biste premjestili s brzine pokretanje. 
- [XOMNI](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18667). Pohrana u budućnosti pokreće brzi pretraživanja i jednostavno protok podataka.
