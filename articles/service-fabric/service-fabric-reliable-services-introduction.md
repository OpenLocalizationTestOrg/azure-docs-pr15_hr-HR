<properties
   pageTitle="Pregled model programiranja servis tkanina pouzdanog servis | Microsoft Azure"
   description="Dodatne informacije o model programiranja za servis tkanina pouzdanog servisa, i počnite vlastite servise za pisanje."
   services="Service-Fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor="vturecek; mani-ramaswamy"/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="03/25/2016"
   ms.author="masnider;vturecek"/>

# <a name="reliable-services-overview"></a>Pregled pouzdanog Services
Azure servisa tkanina pojednostavljuje pisanje i upravljanje bez praćenja stanja i s praćenjem stanja pouzdanog servise. Ovaj dokument će objasniti:

- Pouzdani servisi model programiranja za bez praćenja stanja i s praćenjem stanja servisa.
- Mogućnosti sami morate prilikom pisanja pouzdanog servisa.
- Neke scenarije i primjeri kada koristiti pouzdanog servise i način na koji se pišu.

Pouzdani servisi jedan je od programiranje dostupne na servis tkanina modela. Dodatne informacije o model programiranja pouzdanog Glumci potražite u članku [Uvod u usluge tkanina pouzdanog Glumci](service-fabric-reliable-actors-introduction.md).

U tkanina servisa servisa sastoji od konfiguracija kod aplikacije i (nije obavezno) stanja.

Servis tkanina upravlja vijek servise, dodjele resursa i implementaciju putem nadogradnje i brisanje putem [servisa tkanina Upravljanje aplikacijama](service-fabric-deploy-remove-applications.md).

## <a name="what-are-reliable-services"></a>Što su pouzdani servisi?
Pouzdani servisi omogućuje jednostavno Napredna, najviše razine programiranje model da biste lakše express što je važno u aplikaciji. Sa servisima pouzdanog programiranje model, primit ćete:

- Za s praćenjem stanja servisa, model programiranja pouzdanog servisa omogućuje dosljedno i pouzdano pohranjuju svojih stanje unutar usluge pomoću pouzdanog zbirke. To je jednostavno skup klasa iznimno dostupne zbirke koji će biti upoznati svakome tko ovo koristio C# zbirke. Najčešći services potrebna Vanjski sustavi za upravljanje pouzdanog stanje. S pouzdanog zbirke, možete spremiti stanje uz vaše računalnim s istom visoke dostupnosti i pouzdanosti došli ste očekivali iz iznimno dostupne vanjskim trgovina i sadrže dodatne Latencija poboljšanja čiji Suradnja pronalaženje računalnim i stanje.

- Jednostavan model za izvođenje vlastitog koda koja izgleda ovako programiranje modela koji se koriste za. Kod je dobro definiranom ulaza i jednostavno upravljanih životni ciklus.

- Model uključiv komunikacije. Koristite prijenosa po izboru, kao što su HTTP s [Web API](service-fabric-reliable-services-communication-webapi.md)WebSockets, prilagođene protokoli TCP, itd. Pouzdani servisi omogućuju neki sjajno Izlaz u-tvorničke mogućnosti možete koristiti ili možete unijeti vlastitu.

## <a name="what-makes-reliable-services-different"></a>Što pouzdanog Services čini različite?
Pouzdani servisi u servis tkanina razlikuje se od servisa možda ste napisali prije. Servis tkanina nudi pouzdanosti, dostupnost, dosljednost i skalabilnost.  

- **Pouzdanost**– na servisu ostat će se čak i u nepouzdanih okruženja mjesto vašeg računala možda neće funkcionirati ili kliknite problema s mrežom.

- **Dostupnost**– usluge bit će dostupan i odredište. (To ne znači da ne možete imati servise koje ne mogu pronaći ni otvoriti s izvan.)

- **Skalabilnost**– usluge su samostalne iz određeni hardver i Povećaj ili Smanji prema potrebi kroz Dodavanje ili uklanjanje hardver ili virtualnih resursa. Usluge jednostavno particije (posebno u slučaju da s praćenjem stanja) da bi se neovisno dijelove servis možete skaliranja i odgovaranje na pogreške neovisno. Na kraju, servis tkanina potiče services bude laganih dopuštanja tisuće servisa dodijeljeni resursi unutar jedne procesa, umjesto potrebno ili put izračunala cijelu instance OS na instancu određeni radno opterećenje.

- **Dosljednost**– podatke pohranjene u taj servis je možete zajamčiti da budu dosljedni (odnosi se samo s praćenjem stanja servisa – dodatne informacije o tome kasnije)

## <a name="service-lifecycle"></a>Servis za životni ciklus
Na servisu je li s praćenjem stanja ili bez praćenja stanja, pouzdani servisi omogućuju jednostavne životni ciklus koji vam omogućuje brzo priključite u kodu i početak rada.  Postoje samo jednu ili dvije metode koje su vam potrebne za implementaciju da biste pristupili servisu prema gore i pokrenut.

- **CreateServiceReplicaListeners/CreateServiceInstanceListeners** – to je kojima servis definira snopu komunikacije koji se želi koristiti. Stoga komunikacije, kao što su [Web API](service-fabric-reliable-services-communication-webapi.md)je što određuje listening krajnja točka ili krajnje točke za servis (kako klijenti će dosegne ga). Također definira kako poruke koje se pojavljuju završetka gore interakcija s ostatkom kod usluge.

- **RunAsync** – to je mjesto na servisu pokreće njegove poslovne logike. Otkazivanje tokena koje ste dobili je signal kada trebali biste zaustaviti koji funkcioniraju. Na primjer, ako imate usluge koje je potrebno neprestano povlačenje poruke iz pouzdanog reda čekanja i obrada ih, to je gdje će se dogoditi koji funkcioniraju.

### <a name="service-startup"></a>Pokretanje servisa

Glavna događaja u životni ciklus pouzdanog servisa su:

1. Konstruirana objekt servisa (stvar koja je izvedena iz bez praćenja stanja servisa i s praćenjem stanja servisa).

2. U `CreateServiceReplicaListeners` / `CreateServiceInstanceListeners` način zove dodjeljivanja servis vjerojatnost da biste se vratili slušače komunikacije njegov po izboru.
  - Imajte na umu da je to nije obavezno, iako većina services će izravno izložiti neke krajnjoj točki.

3. Kada se stvaraju slušače komunikacije, otvaranja.
  - Komunikacija slušače imati metodu pod nazivom `OpenAsync()`, koji se sada se zove i koja vraća listening adresu za uslugu. Ako na servisu pouzdanog koristi jedan od ugrađenih ICommunicationListeners, ovo rukuje umjesto vas.

4. Nakon što ga slušatelj komunikacije otvoren, u `RunAsync()` zove se metoda na glavnom servis.
  - Imajte na umu da `RunAsync()` nije obavezno. Ako je servis sve rad izravno u odgovoru korisnik se samo pozivi, nema potrebe za implementaciju `RunAsync()`.

### <a name="service-shutdown"></a>Isključivanje servisa

Kada je servis se isključuje (će se izbrisati, nadograđene ili premještena) redoslijed poziv zrcaljene: najprije token otkazivanja zaključale `RunAsync()` je otkazana zatim `CloseAsync()` zove slušače komunikacije.

Stvari nekoliko važnih napomena o isključivanja s praćenjem stanja usluga:

- Servis tkanina će Promicanje drugi replike servisa primarni statusa do `CloseAsync` i `RunAsync` vratilo. Ako koristite na ga slušatelj ugrađene komunikacije s `CloseAsync` je način rukovanja umjesto vas.

- Dok o vraćanju od ovih metoda nema vremensko ograničenje, odmah izgubite mogućnost pouzdan zbirke za pisanje, a stoga možete završiti bilo koji realni rad. Preporučuje se vraćate kao brže moguće nakon primanja zahtjev za otkazivanje.

## <a name="example-services"></a>Primjer services
Zna ovaj model programiranja, pogledajmo Kratak pregled dvije različite servise da biste vidjeli kako tih dijelova usklađuju.

### <a name="stateless-reliable-services"></a>Bez praćenja stanja servisa pouzdan
Bez praćenja stanja servisa je jedan gdje je doslovno slika održava unutar servisa ili stanje u kojem se nalazi se u potpunosti disposable i ne zahtijeva sinkronizacije, replikacije, postojanost ili visoke dostupnosti.

Na primjer, razmotrite Kalkulator koji nema memorije i prima sve uvjete i postupci za istodobno izvršavanje.

U ovom slučaju RunAsync() servisa može biti prazna, jer nema pozadine-obrada zadataka koje je servis nije potrebno učiniti. Prilikom stvaranja servisa Kalkulator će vratiti na ICommunicationListener (primjerice [Web API -JA](service-fabric-reliable-services-communication-webapi.md)) koji se otvara listening krajnjoj točki neke priključak. Ovaj listening krajnjoj točki će priključiti na različite načine (primjer: "Dodaj (n1, n2)") koji definiranje na kalkulator javno API-JA.

Kada poziv sastoji se od klijenta, odgovarajući način se poziva i servis Kalkulator izvodi operacije na navedeni podaci i vraća rezultat. Ih ne sprema sve stanje.

Što ne pohranjuje sve Interna stanje čini u ovom primjeru Kalkulator vrlo jednostavne. No većina usluge ne doista bez praćenja stanja. Umjesto toga, oni externalize njihovo stanje neke Store. (Ako, na primjer, bilo koji web-aplikacije koje ovisi čuvanja stanje sesije u sigurnosne kopije ili predmemorije nije potpuno bez praćenja stanja.)

Uobičajeni primjer kako se koriste bez praćenja stanja servisa tkanina servis je kao sučelja koji se izlaže dostupnog javnosti API-JA za web-aplikaciju. Servis za sučelja zatim razgovara s praćenjem stanja servisa da biste dovršili zahtjeva za korisnika. U ovom slučaju pozive klijenata oni koji sadrže poznati priključak, kao što su 80, gdje priključuje bez praćenja stanja servisa. U ovom bez praćenja stanja servisa prima poziv i određuje je li se iz pouzdanog proizvođača kao kao koji servis je namijenjene prikazivanju na poziv.  Nakon toga bez praćenja stanja servisa prosljeđuje poziv na odgovarajuće particije s praćenjem stanja servisa i čeka odgovor. Kada bez praćenja stanja servisa primi odgovor, odgovore vratili na izvornu klijenta.

### <a name="stateful-reliable-services"></a>S praćenjem stanja servisa pouzdan
S praćenjem stanja servisa jest ona koja morate imati neki dio stanje drži dosljedne i koje su prisutne u usluge (opis funkcije). Razmislite o servis koji neprestano izračunava rolling prosjek neke vrijednosti na temelju ažuriranja primi. Da biste to učinili, mora imati skup zahtjevi potrebne za postupak, kao i trenutni prosjek. Bilo koji servis koji dohvaća, obrađuje i sprema podatke u vanjskim spremište (kao što je Azure blob ili tablici trgovina danas) je s praćenjem stanja. Samo ostavlja stanje u trgovini vanjskih stanje.

Većina services danas pohranite njihovo stanje vanjsko, budući da je vanjski spremište što omogućuje pouzdanosti, dostupnost, skalabilnost i dosljednost za to stanje. U tkanina servisa s praćenjem stanja servisa ne morate spremiti kao vanjski; njihovo stanje Servis tkanina vodi brigu o tim preduvjetima za kod usluge i stanje servisa.

Recimo da želimo pisanje servis koji vodi zahtjeve za niz pretvorbe koje je potrebno izvršiti na sliku i slike koje je potrebno pretvoriti.  Za ovaj servis je vratiti na komunikacije ga slušatelj (pretpostavimo da Web API-JA) koji se otvara Komunikacijski priključak i omogućuje kao što su poslane stavke putem API `ConvertImage(Image i, IList<Conversion> conversions)`. U ovom API servis nije poduzeti podatke i pohraniti zahtjev u redu čekanja za pouzdane i zatim se vratite neki token klijentu da ga ne može pratiti zahtjev (jer zahtjeve može potrajati neko vrijeme).

U ovaj servis RunAsync može biti složenije. Servis može imati petlje unutar njegov RunAsync povlači zahtjeve iz IReliableQueue izvodi pretvorbe na popisu, a sprema rezultate u IReliableDictionary, tako da se kada se klijent Vrati, možete dobiti njihove pretvorene slike. Kako bi se čak i ako nešto ne uspije slike ne gube, taj servis pouzdanog želite povući iz reda čekanja, izvođenje konverzije i spremite rezultat u transakcije. U ovom slučaju poruke zapravo uklanja samo iz reda čekanja, a rezultati se pohranjuju u rječniku rezultat nakon dovršetka konverzije. Ako nešto ne uspije u sredini (kao što su ovu instancu kod je pokrenut na računalu), zahtjev ostaje u redu čekanja čekaju ponovno obradu.

Jedna od stvari Imajte na umu o taj servis je Zvuči li kao normalne .NET servisa. Jedina razlika je tkanina servis nudi strukture podataka koristi (IReliableQueue i IReliableDictionary), a dakle vrše iznimno pouzdan, dostupne i dosljedni.

## <a name="when-to-use-reliable-services-apis"></a>Kada koristiti pouzdanog API servisa
Ako je nešto od sljedećeg karakteriziraju vašim potrebama aplikacije servisa, trebali biste razmotriti pouzdanog API servisa:

- Morate omogućuje ponašanja aplikacije više jedinica stanja (npr., narudžbe i naručivanje stavki retka).

- Stanje vašeg računala mogu biti prirodan katalog modeliran kao pouzdan rječnici "i" Redovi.

- Stanje mora biti vrlo dostupne s pristupom niske latencije.

- Da biste kontrolirali istodobnosti i preciznosti operacije transakcije u jednu ili više pouzdanog zbirkama mora se aplikacija.

- Želite upravljati komunikacije i kontrola stvaranje particija sheme servisa.

- Kod mora slobodno niti runtime okruženje.

- Aplikacija mora dinamičko stvaranje ili uništiti pouzdanog rječnici ili redovi prilikom izvođenja.

- Morate programski kontrolirati navedene za servis tkanina sigurnosno kopiranje i vraćanje značajke za vaš servis stanja *.

- Da biste zadržali povijest promjena za njegov jedinicama stanje * mora se aplikacija.

- Želite li razviti ili zauzeti treće-strana-razvio prilagođene stanje davatelji *.

> [AZURE.NOTE] * Značajki dostupnih na SDK Općenito dostupan.


## <a name="next-steps"></a>Daljnji koraci
+ [Pouzdani servisi brzi početak](service-fabric-reliable-services-quick-start.md)
+ [Pouzdani servisi Napredno korištenje](service-fabric-reliable-services-advanced-usage.md)
+ [Model programiranja pouzdanog Glumci](service-fabric-reliable-actors-introduction.md)
