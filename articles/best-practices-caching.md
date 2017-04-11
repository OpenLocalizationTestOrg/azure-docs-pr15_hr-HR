<properties
   pageTitle="Predmemoriranje smjernice | Microsoft Azure"
   description="Smjernice o predmemoriranje da biste poboljšali performanse i skalabilnost."
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="best-practice"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/14/2016"
   ms.author="masashin"/>


# <a name="caching-guidance"></a>Predmemoriranje upute

[AZURE.INCLUDE [pnp-header](../includes/guidance-pnp-header-include.md)]

Predmemoriranje je uobičajeni postupak koji trebao da biste poboljšali performanse i skalabilnost sustava. To čini kopiranjem privremeno često pristupa podacima da biste brzo prostora za pohranu koji se nalazi Zatvori da biste aplikaciju. Ako je ovo spremište brzo podataka bliže smještena aplikaciju od izvornog izvora, zatim predmemoriranje možete znatno poboljšati odgovor vremena za klijentske aplikacije brže posluživanje podataka.

Predmemoriranje je učinkovitijeg kada klijent instancu pritišćite čita iste podatke, osobito ako svi sljedeći uvjeti odnose se na izvorni spremišta podataka:
- Ostaje relativno statičnog.
- Je spora usporedbi brzinu predmemoriju.
- Ga podložni visoku razinu Nadmetanje.
- To je daleko kada latenciju mreže mogu prouzročiti pristup biti sporo.

## <a name="caching-in-distributed-applications"></a>Predmemoriranje u raspodijeljeno aplikacijama

Raspodijeljeno aplikacije obično implementirati jednu ili obje sljedeće strategije kada predmemoriranja podataka:

- Korištenje privatne predmemorije, gdje podataka je riječ o javnom lokalno na računalu na kojem se izvodi instance komponente aplikaciju ili servis.
- Korištenje zajedničke predmemorije kao zajednički izvor koji mogu pristupiti više procesa i/ili računala.

U oba slučaja, predmemoriranja može izvoditi klijentsko i/ili poslužiteljsko. Predmemoriranje klijentsko Završi proces koji nudi korisničko sučelje za sustav, kao što je web-preglednik ili za stolna računala.
Predmemoriranje poslužiteljsko obavlja tvrtka postupak koji omogućuje servisa za poslovno daljinski pokrenute.

### <a name="private-caching"></a>Privatni predmemoriranja

Osnovni Vrsta predmemorije je iz trgovine sustava u memoriji. On sadrži sadrži prostor adrese jedan procesa i izravno pristupiti kod koji se izvodi u taj proces. Ta vrsta predmemorije je vrlo brzi pristup. Za pohranu modest količina statične podataka jer veličina predmemorije obično je ograničeno glasnoću memorije koja je dostupna na računalu koje hostira postupka ga možete unijeti i iznimno učinkovitih srednje vrijednosti.

Ako vam je potrebna predmemoriju više nego što je fizički moguće u memoriji, možete napisati predmemoriranih podataka u lokalnom sustavu datoteka. To će biti sporije da biste pristupili od podataka koji se održava u memoriji, ali i dalje mora biti brže i više pouzdane od dohvaćanja podataka putem mreže.

Ako imate više instanci aplikacije koja koristi ovaj modelu koji se izvodi istovremeno, svaku instancu aplikacije ima vlastitu neovisno predmemorije držanjem vlastitu kopiju podataka.

Zamislite predmemorije kao snimku stanja izvornih podataka u nekom trenutku u prošlosti. Ako se ti podaci ne statične, vjerojatno je instanci drugu aplikaciju držite različite verzije sustava podatke u svoje predmemorije. Stoga istog upita obavlja ove instance može vratiti različite rezultate kao što je prikazano na slici 1.

![Korištenje programa u memoriji predmemorije u različite instance programa](media/best-practices-caching/Figure1.png)

_Slika 1: Korištenje programa u memoriji predmemorije u različite instance programa_

### <a name="shared-caching"></a>Zajedničko korištenje predmemoriranja

Korištenje zajedničke predmemorije može vam pomoći ćete opasnosti da podataka može se razlikovati od u svakom predmemoriji koje se može pojaviti prilikom predmemoriranja u memoriji. Zajedničke predmemoriranje osigurava da instance drugu aplikaciju vidjeti isti prikaz predmemoriranih podataka. To čini pronalaženjem predmemoriju na nekom drugom mjestu obično hostira kao dio zaseban, kao što je prikazano na slici 2.

![Korištenje zajedničke predmemorije](media/best-practices-caching/Figure2.png)

_Slika 2: Korištenje zajedničkog predmemorije_

Važno prednost zajedničke predmemoriranja pristup je skalabilnost nudi. Mnoge zajedničke predmemorije servise su implementiran putem klaster poslužitelja i koristiti softver distribuira podatke preko klaster prozirne način. Instance aplikacije jednostavno šalje zahtjev u servisu predmemoriju.
Infrastruktura za temeljni je zadužen za određivanje lokacije predmemoriranim podacima u klasteru. Možete jednostavno promijenila veličina predmemorije dodavanjem više poslužitelja.

Postoje dva glavna nedostatka zajednički pristup predmemoriranja:
- Predmemoriju je sporije da biste pristupili jer ga je više ne sadrži lokalno za svaku instancu aplikacije.
- Zahtjev za implementaciju usluge zasebnom predmemorije može dodati i složenosti rješenje.

## <a name="considerations-for-using-caching"></a>Okolnosti pri korištenju predmemoriranja

U sljedećim odjeljcima opisuju detaljnije okolnosti pri dizajniranju i korištenju predmemoriju.

### <a name="decide-when-to-cache-data"></a>Odlučite kada podatke u predmemoriju

Predmemoriranje znatno poboljšati performanse i skalabilnost dostupnost. Što više podataka koji imate i veći broj korisnika koji se moraju imati pristup ovim podacima veći prednosti predmemoriranje postaju. To je zato predmemoriranje smanjuje Latencija i Nadmetanje pridružen obrada velike količine istovremeni zahtjevi u izvornom spremište podataka.

Baze podataka, na primjer, možda podržava ograničen broj Istodobni veze. Dohvaćanje podataka iz zajedničke predmemorije, međutim, umjesto podlozi baze podataka, zahvaljujući klijentska aplikacija za pristup ove podatke, čak i ako trenutno iskorištavanja broj dostupna veza. Osim toga, ako bazu podataka postane dostupan, klijentske aplikacije možda moći nastaviti koristeći podatke koji se održava u predmemoriji.

Razmislite o predmemoriranja podataka koji se često čitate, ali izmjene diskovni (na primjer, podatke s veći proporcije operacije čitanja od pisanje operacije). Međutim, ne preporučujemo korištenje predmemorije kao mjerodavne spremište ključnih informacija. Umjesto toga, provjerite je li da sve promjene koje aplikacija ne smijete izgubiti uvijek spremaju se u spremištu stalni podataka. To znači da ako predmemorije nije dostupan, aplikacije i dalje možete nastaviti raditi pomoću spremišta podataka, a ne biste izgubili važne informacije.

### <a name="determine-how-to-cache-data-effectively"></a>Određivanje načina učinkovito podatke u predmemoriju

Ključ za učinkovito korištenje predmemoriju leži u određivanje najprikladnije podataka u predmemoriju i predmemoriranja u zakazano vrijeme. Podatke možete dodati u predmemoriju na zahtjev prvi put dohvaća se aplikacija. To znači da aplikaciju treba dohvaćanje podataka samo jednom iz spremišta podataka, a taj naknadni pristup može biti zadovoljeni pomoću predmemoriju.

Osim toga, predmemoriju možete potpuno ili djelomično popuniti podataka unaprijed, obično kada aplikacija pokrene (pristup koji se nazivaju seeding). No možda neće biti da implementirati seeding za velike predmemoriju jer se taj se način možete nametnuti iznenadno, Visoko opterećenje izvornog spremišta podataka prilikom pokretanja aplikacije pokrenut.

Često analizu uzoraka korištenja pomoći da odlučite želite li da potpuno ili djelomično prethodno unijeti predmemoriju te da biste odabrali podatke u predmemoriju. Na primjer, to može biti korisno seed predmemoriju s statične korisničkih profila podataka za korisnike koji koristite aplikaciju redovito (možda svakodnevno), ali ne i za korisnike računala samo jednom tjedno.

Predmemoriranje obično dobro funkcionira s podataka koji je immutable ili da će diskovni promjene. Primjeri referentne informacije kao što su proizvoda i podatke o cijenama u aplikaciju za e-trgovine ili zajednički statične resurse koji su skup za sastavljanje. Neke ili sve te podatke možete staviti u predmemoriju prilikom pokretanja aplikacije da biste minimizirali zahtjev resursa, a da biste poboljšali performanse. Također može biti odgovarajuće imaju pozadinu procesa koji se povremeno ažurira referenca podatke u predmemoriji da biste bili sigurni da je u tijeku ili koji osvježava predmemoriju kada odnose na podatke promjene.

Predmemoriranje manje koristan je za dinamičke podatke, iako postoje neke iznimke ovaj razmotriti (pogledajte odjeljak predmemorije visoko dinamičke podatke u nastavku ovog članka dodatne informacije). Prilikom promjene izvornih podataka redovito, predmemorirani podaci postaje zastarjele vrlo brzo ili indirektnog sinkroniziranja predmemoriju s izvornog spremišta podataka smanjuje učinkovitosti predmemoriranje.

Imajte na umu da predmemoriju ne morate uključiti dovršeno podataka za entitet. Na primjer, ako je podatkovna stavka predstavlja polja s više objekata kao što su banke klijenta s imenom, adresom i saldo računa, neke od tih elemenata možda ostaju statične (kao što su ime i adresu), dok drugi (kao što su saldo računa) to dinamičnije. U takvim slučajevima može biti korisna predmemoriju statične dijelove podataka i dohvaćanje (ili izračun) samo preostale kada je potrebna.

Preporučujemo da izvršavanje analizu testiranje i korištenje performansi da biste utvrdili je li stara populacije ili osvježavati učitavanja predmemoriju ili kombinacijom tih dvaju načina, odgovarajući. Odluku da se temelji na volatility i korištenje uzorak podataka. Upotreba i performanse analiza predmemorije je osobito važno u aplikacijama koje su naišli podebljanom opterećenje i mora biti vrlo skalabilni. Ako, na primjer, u iznimno skalabilni scenarijima je možda smisla seed predmemoriju da biste smanjili opterećenje spremišta podataka Vršna vrijeme.

Predmemoriranje također se poslužite da biste izbjegli ponavljajuće computations dok se izvodi aplikacije. Ako je postupak pretvorbe podataka ili izvodi izračun složene, ga možete spremiti rezultate operacije u predmemoriji. Ako se isti izračun potreban je kasnije, aplikacija možete jednostavno dohvatiti rezultate iz predmemorije.

Aplikaciju možete mijenjati podatke koji se održava u predmemoriju. No preporučujemo misle predmemorije kao izvor podataka za tranzitne koji može nestati u bilo kojem trenutku. Spremanje koristan podataka u predmemoriju samo; Provjerite je li održavate informacije u u izvornom podataka spremište. To znači da ako predmemoriju postane dostupan, koje minimiziranje mogućnost gubitka podataka.

### <a name="cache-highly-dynamic-data"></a>Predmemorija iznimno dinamičke podatke

Kada spremate hitro promjena podataka u spremištu stalni podataka, možete nametnuti programa indirektni u sustavu. Ako, na primjer, razmislite o uređaju neprestano izvješća status ili neke druge mjera. Ako aplikacija ne odabere predmemoriju tih podataka na temelju da predmemorirani podaci gotovo uvijek biti zastarjelih, iste razmotriti može biti true kad pohranu i dohvaćanje te podatke iz spremišta podataka. U vrijeme koje je potrebno da biste spremili i dohvaćanje te podatke, je možda je promijenio.

U slučaju kao što su ova razmotrite prednosti dinamički informacija o pohrani izravno u predmemoriji umjesto u spremištu stalni podataka. Ako se podaci se rješavaju i ne zahtijeva nadzor, zatim svejedno je dvokliknete li Povremeni promjena se gube.

### <a name="manage-data-expiration-in-a-cache"></a>Upravljanje isteka podataka u predmemoriju

U većini slučajeva, koji čuva se predmemoriju podaci kopiju podataka koje sadrži izvorni spremišta podataka. Podaci u izvornom spremišta podataka može se promijeniti kada ga je predmemorirani, uzrok predmemoriranim podacima postati zastarjele. Mnoge predmemoriranja sustavi omogućuju konfiguriranje predmemorije istječe podataka i smanjivanje razdoblje za koje možda zastarjeli podataka.

Kada istekne predmemoriranih podataka, znači da je uklonjena iz predmemorije, a aplikacija potrebno dohvatiti podatke iz izvorne spremišta podataka (to možete pohraniti upravo dohvaćanja informacija natrag u predmemoriji). Možete postaviti zadani Pravilnik za istek prilikom konfiguriranja predmemoriju. U mnoge servise predmemorije, možete i stipulate razdoblje isteka za pojedinačne objekte kada ih pohranite programski u predmemoriji.
Neke predmemorije vam omogućuju da odredite razdoblje isteka kao apsolutnu vrijednost ili vrijednost klizača koja uzrokuje stavke koje će biti uklonjene iz predmemorije, ako se pristupa unutar određenog vremena. Ta postavka nadjačava sve pravilnika za istek predmemorije razini, ali samo za navedeni objekti.

> [AZURE.NOTE] Razmislite o razdoblje isteka za predmemorije i objekte koji sadrži pažljivo. Ako je premalen, objekti će isteći previše brzo te će smanjiti prednosti korištenja predmemoriju. Ako je predugačak razdoblje, rizikom postanete zastarjele podatke.

Moguće je da predmemoriju možda ispuni gore ako je dopušteno podaci ostaju koje postoji na dulje vrijeme. U ovom slučaju sve zahtjeve za dodavanje novih stavki u predmemoriju može uzrokovati neke stavke koje će biti uklonjene prisilno u postupku naziva eviction. Predmemoriju usluga obično evict podataka na temelju najmanjih-nedavno korišteni (LRU), ali obično možete zaobići to pravilo i onemogućuju se izbaciti stavke. Međutim, ako se primijene takvog vam rizikom prekoračenje memorije koja je dostupna u predmemoriji. Uz iznimku neće uspjeti aplikacije koja pokušava da biste dodali stavku u predmemoriju.

Neke predmemoriranja implementacije dat će dodatnih eviction pravila. Postoji nekoliko vrsta eviction pravila. To obuhvaća:
- Većina-nedavno korišteni pravilo (u očekivanja da podaci neće biti potrebno ponovno).
- Pravilo first-u – first out (od najstarijeg podataka je izbaciti najprije).
- Uklanjanje eksplicitnih pravila koja se temelji na okidačima događaja (kao što su podaci koji se mijenja se).

### <a name="invalidate-data-in-a-client-side-cache"></a>Poništiti valjanost podataka u predmemoriju na klijentskoj strani

Podaci koji čuva se klijentsko predmemorije obično smatra se izvan auspices servis koji omogućuje podatke klijentu. Servis nije moguće izravno prisilno klijenta za dodavanje ili uklanjanje podataka iz predmemorije za klijentsko.

To znači da je moguće za klijenta koji koristi loše konfigurirani predmemorije da biste nastavili koristiti zastarjelih podataka. Ako, na primjer, ako pravila isteka predmemorije nisu pravilno implementirana, klijent koristiti zastarjele podatke koji je lokalno predmemorirani kada je promijenjena informacije u izvornih podataka.

Ako izrađujete web-aplikacije koje služi podataka putem HTTP veze, implicitno možete pokrenuti web-klijenta (kao što je preglednik ili web proxy) radi dohvaćanja najnovije informacije. To možete učiniti ako se resurs ažurira promjenom URI resursa. Web-klijentima obično koriste URI resursa kao ključ u predmemoriji klijentsko, pa ako URI promijeni, web-klijentu zanemaruje bilo prethodno predmemorirane verzije resursa i umjesto toga dohvaćanja novu verziju.

## <a name="managing-concurrency-in-a-cache"></a>Upravljanje istodobnosti u predmemoriju

Predmemorija često dizajnirani su za zajedničko korištenje više instanci aplikacije. Svaku instancu aplikacije mogu čitati i mijenjati podatke u predmemoriji. Zbog toga isti istodobnosti problemi koji će se pojaviti s bilo kojeg pohrane podataka za zajedničko korištenje primijeniti predmemoriju. U slučaju gdje treba aplikaciju za izmjenu podataka koji se održava u predmemoriji bi dobro da biste bili sigurni da ažuriranjima jedne instance programa prebrisati promjene u drugoj instanci.

Ovisno o prirode podatke i vjerojatnost sudara, prihvaćaju jedan od dva načina za istodobnosti:

- __Optimistična Procjena.__ Neposredno prije ažuriranja podataka, provjerava jesu li podatke u predmemoriji je promijenjen nakon što ste ih dohvatili aplikacije. Ako su podaci i dalje isti, promjene se mogu se postaviti. U suprotnom aplikacija mora odlučiti želite li ažurirati. (Poslovne logike koji pogoni tu odluku će biti specifičnim aplikacijama.) Taj se način je prikladan za situacije gdje se rijetko ažuriranja ili gdje sudara vjerojatno neće se pojaviti.
- __Pesimistična Procjena.__ Kad dohvaća podatke, aplikacija je zaključava u predmemoriji da biste spriječili ga promijenite u drugoj instanci. Ovaj postupak osigurava da ne može biti sudara, ali možete blokirati i ostalih instanci koje su potrebne za obradu iste podatke. Pesimistična Procjena istodobnosti utječu skalabilnost rješenje i preporučuje se samo za short-lived operacije. Taj se način mogu prikladna za situacije u kojima sudara veća je vjerojatnost, osobito ako aplikaciju ažurira više stavki u predmemoriji i mora biti sigurni da su te promjene dosljedno primijenjene.

### <a name="implement-high-availability-and-scalability-and-improve-performance"></a>Implementirati visoke dostupnosti i skalabilnost i poboljšanja performansi

Izbjegavanje predmemoriju kao primarni spremište podataka. Ovo je uloga izvornog spremišta podataka iz kojeg se popunjava predmemoriju. Izvorni spremišta podataka je zadužen za osiguravanje postojanost podatke.

Pripazite da ne predstavljanje ključnih međuzavisnosti dostupnosti zajedničkih predmemorije servisa u vašem rješenja. Zatvaranje trebali biste moći nastaviti raditi ako servis koji omogućuje zajedničko predmemorije nije dostupan. Aplikacija trebali biste "smrzavanje" ili neće uspjeti tijekom čekanja za servis predmemorije da biste nastavili.

Stoga aplikacija potrebno pripremiti za otkrivanje dostupnost servisa predmemorije i smanjenja izvornog spremišta podataka ako predmemoriju ne može pristupiti. [Elektronička prepoznavanje uzorak](http://msdn.microsoft.com/library/dn589784.aspx) je korisno za rukovanje scenarij. Servis koji omogućuje predmemoriju može oporaviti, a kad postane dostupan, predmemoriju mogu se ponovno popuniti kao podataka je pročitati obrasca izvorne spremišta podataka koji prate strategije kao što su [predmemorije aside uzorak](http://msdn.microsoft.com/library/dn589799.aspx).

Međutim, možda postoje skalabilnost utjecaj u sustavu Ako aplikaciju pada vratili izvorne spremište podataka kada je predmemoriju privremeno nedostupan.
Dok je obnavljaju spremišta podataka, izvornog spremišta podataka nije moguće pronalaženje web-aplikacija zahtjevima za podatke, rezultira vremenska ograničenja i nije uspjela veze.

Razmislite o implementacije lokalno, privatni predmemorije u svaku instancu programa, zajedno s predmemoriju zajedničke kojoj pristup sve instance aplikacije. Kada aplikacija dohvaća stavke, ga možete najprije provjerite svoje lokalne predmemorije, a zatim u zajedničko korištenje predmemorije i na kraju u izvorne podatke sprema. Lokalne predmemorije možete unijeti pomoću podatke u zajedničkoj predmemoriji ili u bazi podataka ako zajedničke predmemorije nije dostupan.

Taj se način zahtijeva oprezni konfiguracije da biste spriječili postaje prevelika zastarjele vezana uz zajedničku predmemorije lokalne predmemorije. Međutim, lokalne predmemorije djeluje kao međuspremnika ako zajedničke predmemorije nije dostupan. Slika 3 prikazuje sljedeću strukturu.

![Lokalno, privatni predmemorije pomoću zajedničkog predmemorije](media/best-practices-caching/Caching3.png)
_Slika 3: lokalno, privatni predmemorije pomoću zajedničkog predmemorije_

Da biste podržava velike predmemorije koji sadrže relativno long-lived podatke, neke predmemorije usluge unesite visoku dostupnost mogućnost koji implementira automatsko prebacivanje ako predmemoriju postane nedostupan. Takvog obično uključuje replikaciju predmemoriranim podacima pohranjenoj na poslužitelju primarni predmemorije predmemorije sekundarni poslužitelj i prijelaz na sekundarni poslužitelj ne uspijete primarni poslužitelj ili povezivanje nestaje.

Da biste smanjili Latencija povezane s pisanjem više odredišta replikaciju sekundarnu poslužitelj mogu se pojaviti asinkrono kada podaci se upisuju u predmemoriju na primarnom poslužitelju. Taj se način potencijalnih klijenata za mogućnost da neki predmemorirani podaci mogu biti izgubljeni u slučaju pogreške, ali proporcije tih podataka mora biti mala veličina predmemorije u usporedbi.

Ako je prevelik zajedničke predmemorije, možda će korisni particija predmemoriranim podacima preko čvorove smanjiti vjerojatno Nadmetanje i poboljšati skalabilnost. Mnoge zajedničke predmemorije podržava mogućnost dinamički dodajte (i uklonite) čvorove i poduzme podatke preko particije. Taj se način možda obuhvaćaju Klasteriranje, u kojem zbirke čvorove se prikazuju s klijentskim aplikacijama kao objedinjenog, jedan predmemoriju. Interno, međutim, podaci se u odnosu između čvorove pratiti strategije unaprijed definirane raspodjele koji salda opterećenje ravnomjerno. [Dokument s podacima stvaranje particija upute](http://msdn.microsoft.com/library/dn589795.aspx) na web-mjestu Microsoft pruža dodatne informacije o mogućim stvaranje particija strategije.

Klasteriranje također možete povećati dostupnost predmemoriju. Ako čvor ne uspije, i dalje dostupna je ostatak predmemoriju.
Klasteriranje često se koristi u kombinaciji s replikacijom i prebacivanje. Moguće je replicirati svaki čvor i replike mogu biti brzo na mreži ne uspijete čvor.

Mnoge čitanje i pisanje operacije vjerojatno obuhvaćaju jedan podatkovni vrijednosti ili objekte. Međutim, ponekad možda je potrebno za pohranu ili brzo dohvatiti velike količine podataka.
Na primjer, seeding predmemoriju nije obuhvaćaju pisanje stotine ili tisuće stavki u predmemoriju. Program možda ćete morati dohvatiti velikog broja povezane stavke iz predmemorije kao dio iste zahtjev.

Mnoge veliki predmemorije pružaju operacije grupe za tu svrhu. Omogućuje klijentsku aplikaciju s objediniti velik broj stavki u jedan zahtjev i smanjuje indirektnog pridružen izvođenje velik broj zahtjeva za small.

## <a name="caching-and-eventual-consistency"></a>Predmemoriranja i usmjerenog dosljednosti

Za uzorak predmemorije aside raditi, instanci aplikacije koja popunjava predmemoriju morate imati pristup najviše nedavne i dosljedni verzije podataka. U sustavu koji implementira usmjerenog dosljednost (kao što je izvor podataka repliciranu) to možda neće biti slučaj.

Instance programa nije izmijenite stavku podataka i poništiti valjanost predmemorirani verziju stavke. Drugu instancu programa može pokušati pročitajte ovu stavku iz predmemorije, čime će se web-neuspješno pronalaženje u predmemoriji, tako da ga čita podatke iz spremišta podataka i dodaje u predmemoriju. Međutim, ako spremišta podataka nije u potpunosti sinkroniziran s drugim replike, instanci aplikacije nije pročitajte i popunjavanje predmemorije stare vrijednošću.

Dodatne informacije o rukovanje dosljednost podataka potražite u članku stranici [primer dosljednost podataka](http://msdn.microsoft.com/library/dn589800.aspx) na Microsoftovu web-mjestu.

### <a name="protect-cached-data"></a>Zaštita predmemoriranim podacima

Bez obzira servisom predmemorije koristiti, razmislite o zaštiti podataka koji se održava u predmemoriji od neovlaštenog pristupa. Postoje dva glavna razloga:

- Privatnost podataka u predmemoriji
- Privatnost podataka kao što je teče između predmemorije i aplikaciju koja koristi predmemoriju

Da biste zaštitili podatke u predmemoriji, servisom predmemorije možda implementirati mehanizam provjere autentičnosti koje je potrebno da aplikacije navedite sljedeće:
- Koje identiteta može pristupiti podacima u predmemoriji.
- Operacijama (čitanje i pisanje) te identiteta dopušteni za izvođenje.

Da biste smanjili indirektni koji je povezan s čitanje i pisanje podataka, nakon identitet odobren pisanje i/ili pristup za čitanje u predmemoriju identitet možete koristiti podatke u predmemoriji.

Ako je potrebno ograničiti pristup podskupove predmemoriranim podacima, možete učiniti nešto od sljedećeg:

- Podijelite predmemoriju particije (pomoću poslužitelja za različite predmemorije), a samo dopustiti pristup identiteta za particije koje će biti dopušteno korištenje.
- Šifriranje podataka u svakom podskup pomoću različite tipke, a pruža ključeve za šifriranje samo na identiteta koje smiju pristupati svaki podskupu. Klijentska aplikacija mogu i dalje moći dohvatiti sve podatke u predmemoriji, ali samo se moći dešifrirati podatke za koje je tipki.

Morate zaštiti podataka kao što je teče i predmemoriju. Da biste to učinili, ovise o sigurnosnim značajkama koje ste dobili od mrežne infrastrukture koja klijentske aplikacije koristi za povezivanje u predmemoriju. Ako predmemoriju je korištenje on-site poslužitelja u istoj tvrtki ili ustanovi domaćini klijentske aplikacije, zatim odvajanja mrežu ne morat ćete poduzeti dodatne korake. Ako predmemoriju nalazi daljinski i zahtijeva TCP i HTTP vezu putem javne mreže (kao što su Internet), preporučujemo da implementacijom SSL.

## <a name="considerations-for-implementing-caching-with-microsoft-azure"></a>Zahtjevi za implementaciju predmemoriranje s Microsoft Azure

Azure nudi predmemoriju Redis Azure. Ovo je implementacija Otvori izvor Redis predmemorije koja se pokreće kao usluga u Azure podatkovnog centra. Pruža predmemoriranja servis koji se može pristupiti iz bilo koju aplikaciju za Azure, hoće li se aplikacija je implementirana kao neki servis u oblaku, web-mjesto ili unutar Azure virtualnog računala. Predmemorija je moguće zajednički koristiti tako da klijentske aplikacije koji imaju odgovarajuće tipkovni prečac.

Predmemorija za Azure Redis je visokih performansi predmemoriranja rješenja koja omogućuje dostupnost, skalabilnost i sigurnost. Obično se pokreće kao usluga podjele na jednu ili više namjenski računalima. Pokušava ga spremiti kao informacija koliko možete u memoriji da biste bili sigurni brzog pristupa. Tu arhitekturu namijenjen je pružanje niske latencije i Visoko propusnost smanjivanjem potrebe za izvođenje sporo/i operacija.

 Azure Redis predmemorije kompatibilan s mnogo različitih API-ji koji se koriste u klijentskim aplikacijama. Ako imate postojeće aplikacije koje se već koristi Redis Azure predmemoriju radi lokalnog, predmemoriju Redis Azure omogućuje brzo migracije put do predmemorije u oblaku.

> [AZURE.NOTE] Azure omogućuje upravljani predmemorije servis. Taj servis temelji se na modul Azure servisa tkanina predmemoriju. Omogućuje stvaranje raspodijeljenoj predmemoriji koje je moguće zajednički koristiti slobodnije povezano aplikacija. Predmemoriju se hostira na poslužiteljima visokih performansi koji se izvode u Azure podatkovnog centra.
Međutim, ta mogućnost ne preporučuje se i samo navedeni su za podršku postojeće programima koji imaju omogućeno ugrađen je koristiti. Za sve nove razvoj, umjesto toga koristite Azure Redis predmemoriju.
>
> Uz to, Azure podržava predmemoriranje u ulogu. Ova značajka omogućuje vam da biste stvorili predmemoriju koji se odnose na neki servis u oblaku.
Predmemoriju održava instance web ili tempiranja uloge, a može se pristupiti samo uloge koje su operacijski kao dio iste jedinice uvođenje servisa oblaka. (Uvođenja jedinici je skup instance uloge koje uvode se kao neki servis u oblaku za određenu regiju.) Grupirani predmemorije, a sve instance uloge unutar iste jedinice implementaciju koji hostira predmemoriju postaju dio iste klaster predmemoriju. Međutim, ta mogućnost ne preporučuje se i samo navedeni su za podršku postojeće programima koji imaju omogućeno ugrađen je koristiti. Za sve nove razvoj, umjesto toga koristite Azure Redis predmemoriju.
>
> Servis predmemorije upravlja Azure i predmemorije u ulozi Azure trenutno slated za umirovljenje u studenom 16th, 2016.
Preporučuje se da migrirati Azure Redis predmemoriju u Priprema za ovaj umirovljenje. Dodatne informacije potražite na stranici   [što je nuditi Azure Redis predmemorije i koju veličinu trebali biste koristiti?](redis-cache/cache-faq.md#what-redis-cache-offering-and-size-should-i-use) na Microsoftovu web-mjestu.


### <a name="features-of-redis"></a>Značajke Redis

 Redis veća od jednostavne predmemorije poslužitelja. Raspodijeljeno baze podataka u memoriji pruža s skupa proširenom naredba koji podržava mnoge uobičajeni scenariji. Te su opisane u nastavku ovog dokumenta, u odjeljku pomoću Redis predmemoriranja. U ovom se odjeljku ukratko opisane neke ključne značajke koje nudi Redis.

### <a name="redis-as-an-in-memory-database"></a>Redis kao baze podataka u memoriji

Redis podržava oba za čitanje i pisanje operacije. U Redis, zapisivanje možete zaštititi od pogreška sustava unosom pohranjivanje povremeno u lokalni snimke datoteke ili u datoteci zapisnika samo za dodavanje. Ovo nije slučaja u mnogo predmemorije (koji razmatranje trgovine transitory podataka).

 Sve zapisivanja su asinkronog i blokiranje klijenti čitanje i pisanje podataka. Kada Redis pokretanje pokrenut, čita podatke iz datoteke snimke ili zapisnik i da bi ga koristi za sastavljanje predmemorije u memoriji. Dodatne informacije potražite u članku [Redis postojanost](http://redis.io/topics/persistence) Redis web-mjesta.

> [AZURE.NOTE] Redis ne jamči da sve zapisivanja spremit će se slučaju Katastrofalna pogreška, ali kod najgore izgubiti samo nekoliko sekundi vrijednosti podataka. Imajte na umu predmemoriju nije namijenjen će poslužiti kao izvora podataka mjerodavne pa je odgovornosti aplikacije koje koriste predmemoriju da biste bili sigurni ključne podatke uspješno spremljen u spremište odgovarajuće podatke. Dodatne informacije potražite u članku [predmemorije aside uzorka](http://msdn.microsoft.com/library/dn589799.aspx).

#### <a name="redis-data-types"></a>Redis vrste podataka

Redis je u trgovini ključa vrijednosti koje sadrže vrijednosti jednostavne vrste ili strukture složenih podataka kao što su raspršivanja, popisi, a postavlja. Podržava skup atomske operacije na ove vrste podataka. Tipke može biti trajno ili označeni s ograničeno vrijeme važenja, pri čemu tipku i njezina odgovarajuću vrijednost se automatski uklanjaju se iz predmemorije. Dodatne informacije o Redis ključeva i vrijednosti, posjetite stranicu [Uvod u Redis vrste podataka i abstractions](http://redis.io/topics/data-types-intro) Redis web-mjesta.

#### <a name="redis-replication-and-clustering"></a>Redis replikaciju i Klasteriranje

Redis podržava matrica/podređeni replikacije radi osiguraj i održavanje propusnost. Pisanje operacije da biste Redis osnovne čvor su replicirati na jednu ili više podređene čvorove. Čitanje operacije možete posluživanje matricu ili bilo koji od na podređena.

U slučaju particije mreže podređena možete nastaviti služiti podataka, a zatim proziran ponovno sinkroniziranje s matricu kada je ponovo uspostaviti vezu. Dodatne pojedinosti potražite na stranici [replikacije](http://redis.io/topics/replication) Redis web-mjesta.

Redis omogućuje Klasteriranje, koji omogućuje proziran particija podataka u shards na poslužiteljima i širenje opterećenje. Ta značajka poboljšava skalabilnost, jer dodavanja novih Redis poslužitelja i podataka repartitioned kao veličina predmemorije povećava.

Osim toga, svakome u klasteru moguće je replicirati pomoću matrice/podređeni replikacije. Time se preko svake čvor u klasteru osigurava dostupnost. Dodatne informacije o Klasteriranje i sharding posjetite [Redis klaster vodiča stranice](http://redis.io/topics/cluster-tutorial) na web-mjestu Redis.

### <a name="redis-memory-use"></a>Redis korištenje memorije

Redis predmemorije ima konačne veličinu koja ovisi o resursima dostupne na glavnom računalu. Kada konfigurirate Redis server, možete odrediti maksimalnu količinu memorije ga možete koristiti. Možete konfigurirati i ključa u predmemoriji Redis da bi se vrijeme isteka, nakon čega ga je automatski uklanjaju iz predmemorije. Ta značajka može spriječiti predmemorije u memoriji ispunjavanja stare ili zastarjele podatke.

Kako se puni memorije, Redis možete automatski evict tipki i njihovih vrijednosti slijedeći broj pravila. Zadana vrijednost je LRU (najmanje nedavno korišteni), ali možete odabrati i druge pravila kao što su nasumično evicting tipke ili isključite eviction potpuno (u koji, slučaja prilikom pokušaja da biste dodali stavke nije uspjelo predmemorije je popunjen). Stranica [Pomoću Redis kao u predmemoriji LRU](http://redis.io/topics/lru-cache) pruža dodatne informacije.

### <a name="redis-transactions-and-batches"></a>Redis transakcije i grupama

Redis omogućuje klijentska aplikacija za slanje niz operacije koje za čitanje i pisanje podataka u predmemoriji kao atomske transakcije. Su sve naredbe transakcije zajamčiti da biste pokrenuli sekvencijalno, a ne naredbe izdala drugim Istodobni klijentima će biti utkali između njih.

Međutim, nisu true transakcije dok ih želite izvesti relacijske baze podataka. Obrada transakcije sastoji se od dvije faze – prvi je kada je u redu čekanja naredbi, a drugi je izvršavanja naredbe. Tijekom faze stavljanja naredba naredbe koje čine transakcije poslane klijenta. Ako se pojavi neka sortiranje pogreške sada (primjerice pogreške u sintaksi ili pogrešan broj parametara) zatim Redis odbije za obradu cijelu transakcije i odbacuje ga.

Tijekom izvođenja faze Redis izvodi svaku naredbu u redu čekanja u nizu. Naredbe ne uspije u ovoj se fazi, Redis nastavlja na sljedeće naredbe u redu čekanja, a ne vraćate efekata naredbi koje ste već pokrenuli. Pojednostavnjeni obrazac transakcije olakšava održavanje performanse i izbjegli probleme s performansama koje uzrokuju Nadmetanje.

Redis implementaciju optimistična Procjena zaključavanje kao pomoć u održavanje dosljednosti oblik. Detaljne informacije o transakcije i zaključavanje s Redis posjetite [stranicu transakcije](http://redis.io/topics/transactions) Redis web-mjesta.

Redis podržava i koje nisu transakcijskih grupnog slanja promjena zahtjeva. Protokol Redis koji klijenti koristiti za slanje naredbi na poslužitelju Redis omogućuje klijenta za slanje skupa postupaka kao dio iste zahtjev. To se može pomoći da biste smanjili Fragmentacija paketa na mreži. Kada grupu obrađuje, svaka naredba se izvodi. Ako su neki od tih naredbi oblikovan, oni će biti odbijene (koji se ne događa s transakcije), ali provest će se preostale naredbe. Nema jamstva o narudžbi u kojoj će se obraditi naredbe u grupi.

### <a name="redis-security"></a>Redis sigurnost

Redis namijenjen je isključivo omogućuje brz pristup podacima, a namijenjen je pokreću se unutar pouzdanih okruženju u kojem se može pristupiti samo pouzdanih klijente. Redis podržava ograničeni sigurnosnih modela na temelju provjeru autentičnosti lozinke. (To je moguće da biste potpuno uklonili provjere autentičnosti iako to ne preporučujemo.)

Sve klijente čija je autentičnost provjerena zajednički koristiti istu lozinku za globalnu i imaju pristup iste resurse. Ako vam je potrebna detaljnije sigurnosti za prijavu, morate provesti vlastite Sigurnosni sloj ispred Redis poslužitelja, a svi zahtjevi klijenta mora proći kroz ovaj dodatne sloja. Redis mora biti izravno izložen nepouzdanih ili Neprovjereni klijenata.

Mogu ograničiti pristup naredbama tako da ih onemogućivanje ili im (i unosom samo povlaštene klijente s novim nazivima).

Redis ne podržava izravno bilo koji obrazac šifriranje podataka, tako da sve kodiranje moraju izvršiti po klijentske aplikacije. Uz to, Redis sadrže bilo koji obrazac prijenosa sigurnost. Ako vam je potrebna za zaštitu podataka kao što je teče putem mreže, preporučujemo da implementacijom SSL proxy poslužitelja.

Dodatne informacije potražite na stranici [Redis sigurnost](http://redis.io/topics/security) Redis web-mjesta.

> [AZURE.NOTE] Azure Redis predmemorije nudi vlastitom sloju sigurnost putem kojega se klijenti povezuju. Temeljni Redis poslužitelji ne pojavljuje se na javnim mrežama.

### <a name="using-the-azure-redis-cache"></a>Korištenje predmemorije Azure Redis

Predmemoriju Azure Redis omogućuje pristup Redis poslužiteljima koji se izvode na poslužiteljima hostira pri Azure podatkovnog centra; djeluje kao façade koji omogućuje kontrolu pristupa i sigurnost. Predmemoriju možete Dodjela pomoću portala za upravljanje Azure. Na portalu omogućuje brojne unaprijed definirane konfiguracije rasponu od 53GB predmemorije izvodi kao namjenski servis koji podržava SSL komunikacije (za zaštitu privatnosti) i replikacije matrica/podređeni pomoću programa SLA 99.9% dostupnost, prema dolje do 250 MB predmemoriju bez replikacije (bez jamstva dostupnost) radi na zajedničkom hardvera.

Pomoću portala za upravljanje Azure, možete konfigurirati pravila eviction predmemorije, i kontrola pristupa u predmemoriju dodavanjem korisnika u uloge navesti. Čitač vlasnika, suradnik. Te uloge definirali operacije koja se izvršava članova. Ako, na primjer, Članovi uloge vlasnika imaju potpunu kontrolu nad predmemorije (uključujući sigurnost) i njezin sadržaj, članovi ulogu suradnika lakše čitali i pisali informacije u predmemoriji i Članovi uloge čitač samo možete dohvatiti podatke iz predmemorije.

Većina administrativne zadatke izvršavaju putem portala za upravljanje Azure i zbog toga mnoge od administratora naredbe dostupne u standardnu verziju Redis su nije dostupan, uključujući mogućnost za izmjenu konfiguraciju programski zatvaranja poslužitelja Redis konfiguriranje dodatne slaves ili prisilno spremanje podataka na disk.

Portal za Azure upravljanje sadrži praktičan grafički prikaz koja omogućuje praćenje performansi predmemoriju. Ako, na primjer, možete prikazati broj veza dijaloški okvir, broj zahtjeva obaviti količinu čitanja i pisanja, i broj predmemorije dodirne nasuprot neuspjele akcije u predmemoriji. Korištenjem ove informacije, možete odrediti učinkovitosti predmemorije i ako je potrebno prijelaz na drugu konfiguraciju ili promijenite pravilnik eviction. Osim toga, možete stvoriti upozorenja koje administrator ako jedan ili više ključnih metriku padaju izvan raspona očekivani slati poruke e-pošte. Ako, na primjer, ako je broj neuspjele akcije u predmemoriji premašuje određenu vrijednost u zadnjem satu, administrator nije će upozoreni možda je predmemorija premalen ili podataka mogu se se izbaciti previše brzo.

Možete nadzirati i procesora, memorije i korištenje mreže za predmemoriju.

Dodatne informacije i primjeri s prikazom načina za stvaranje i konfiguriranje programa Azure Redis predmemorije, posjetite stranicu [prijenosni oko Azure Redis predmemorije](https://azure.microsoft.com/blog/2014/06/04/lap-around-azure-redis-cache-preview/) na blogu Azure.

## <a name="caching-session-state-and-html-output"></a>Stanje predmemoriranja sesije i HTML izlaz

Ako vam sastavljanje ASP.NET web-aplikacije koje pokrenite pomoću Azure web uloge, uštedjet ćete informacije o stanju sesije i HTML izlaz u predmemoriji Redis programa Azure. Davatelj usluga stanja sesije Azure predmemoriju Redis omogućuje zajedničko korištenje informacija sesije između različitih pojavljivanja ASP.NET web-aplikacije i vrlo je praktična u farmi web situacijama gdje afinitet klijent poslužitelj nije dostupan i predmemoriranja sesiju podataka u memoriji nije prikladna.

Pomoću davatelja stanje sesije Azure Redis predmemorije nudi nekoliko prednosti, uključujući:

- To možete omogućiti zajedničko korištenje stanje sesije između velikog broja pojavljivanja ASP.NET web-aplikacije, pruža poboljšane skalabilnost
- Podržava nadziranim Istodobni pristup iste sesije podatke o stanju za više čitatelji i jedan writer, a
- Da biste spremili memorije i poboljšali performanse mreže ga možete koristiti sažimanja.

Dodatne informacije potražite na stranici [ASP.NET sesije stanje davatelja Azure predmemoriju Redis](redis-cache/cache-aspnet-session-state-provider.md) na Microsoftovu web-mjestu.

> [AZURE.NOTE] Pomoću davatelja stanje sesije za Azure Redis predmemoriju za ASP.NET programa koji se pokreću izvan okruženje za Azure. Latencija pristupa u predmemoriji izvan Azure možete isključiti performanse prednosti predmemoriranja podataka.

Isto tako, izlazne predmemorije davatelj usluga za Azure Redis predmemorije omogućuje spremanje odgovora HTTP generira ASP.NET web-aplikacije. Davatelj usluga za izlazne predmemorije pomoću Azure Redis predmemorije možete poboljšati vrijeme odaziva aplikacija koje se iscrtavaju složene HTML izlaz; Korištenje aplikacije instance olakšavaju stvaranje slične odgovore fragmentirane zajedničke izlazne predmemorije umjesto generiranje ovaj HTML izlaz afresh.  Dodatne informacije potražite na stranici [ASP.NET izlazne predmemorije davatelja Azure predmemoriju Redis](redis-cache/cache-aspnet-output-cache-provider.md) na Microsoftovu web-mjestu.

### <a name="azure-redis-cache"></a>Azure Redis predmemorije

Azure Redis predmemorije omogućuje pristup Redis poslužitelja koji se nalaze na Azure podatkovnog centra. Djeluje kao façade koji omogućuje kontrolu pristupa i sigurnost. Predmemoriju možete Dodjela pomoću portala za Azure.

Na portalu omogućuje brojne unaprijed definirane konfiguracije. Ove kreću se od 53 GB predmemorije izvodi kao namjenski servis koji podržava SSL komunikacije (za zaštitu privatnosti) i replikacije matrica/podređeni pomoću programa SLA 99.9% dostupnost, prema dolje do 25 predmemorije 0 MB bez replikacije (bez jamstva dostupnost) radi na zajedničkom hardvera.

Pomoću portala za Azure, možete i konfiguriranje pravilnika eviction predmemorije i kontrola pristupa u predmemoriju dodavanjem korisnika u uloge navedene.  Te uloge koje definirali operacije koja se izvršava članova, obuhvaćaju vlasnika, suradnika i Reader. Ako, na primjer, Članovi uloge vlasnika imaju potpunu kontrolu nad predmemorije (uključujući sigurnost) i njezin sadržaj, članovi ulogu suradnika lakše čitali i pisali informacije u predmemoriji i Članovi uloge čitač samo možete dohvatiti podatke iz predmemorije.

Većina administrativne zadatke izvršavaju se putem portala za Azure. Zbog toga mnoge od administratora naredbe koje su dostupne u standardnoj Redis nisu dostupne, uključujući mogućnost programski izmjenu konfiguracije, isključite poslužitelja Redis, konfigurirati dodatne podređena ili prisilno spremiti podatke na disk.

Portal za Azure sadrži praktičan grafički prikaz koja omogućuje praćenje performansi predmemoriju. Na primjer, možete pogledati broj veza unijeli, broj zahtjeva provodi, količinu čitanja i pisanja, i broj uspješnih predmemorije nasuprot neuspjele akcije u predmemoriji. Pomoću ove informacije, možete odrediti učinkovitosti predmemorije i ako je potrebno, prijeđite na drugu konfiguraciju ili promijenite pravilnik eviction.

Osim toga, možete stvoriti upozorenja koje administrator ako jedan ili više ključnih metriku padaju izvan raspona očekivani slati poruke e-pošte. Na primjer, možda ćete morati Upozori administrator ako je broj neuspjele akcije u predmemoriji premašuje određenu vrijednost u zadnjem satu jer je to znači da predmemoriju biti presitan ili podataka možda se ne izbaciti previše brzo.

Možete nadzirati i procesora, memorije i korištenje mreže za predmemoriju.

Dodatne informacije i primjeri s prikazom načina za stvaranje i konfiguriranje programa Azure Redis predmemorije, posjetite stranicu [prijenosni oko Azure Redis predmemorije](https://azure.microsoft.com/blog/2014/06/04/lap-around-azure-redis-cache-preview/) na blogu Azure.

## <a name="caching-session-state-and-html-output"></a>Stanje predmemoriranja sesije i HTML izlaz

Ako ste stvaranje ASP.NET web-aplikacije koje pokrenite pomoću Azure web uloge, uštedjet ćete sesiju navedite informacije i HTML izlaz u predmemoriji Redis programa Azure. Davatelj usluga stanja sesije Azure predmemoriju Redis omogućuje zajedničko korištenje informacija sesije između različitih pojavljivanja ASP.NET web-aplikacije i vrlo je praktična u farmi web situacijama gdje afinitet klijent poslužitelj nije dostupan i predmemoriranja sesiju podataka u memoriji nije prikladna.

Pomoću davatelja stanje sesije Azure Redis predmemorije nudi nekoliko prednosti, uključujući:

- Stanje sesije zajedničkog korištenja s velikim brojem instanci ASP.NET web-aplikacije.
- Pruža poboljšane skalabilnost.
- Nadziranim Istodobni pristup iste sesije podatke o stanju za podršku za više čitateljima i jedan writer.
- Da biste spremili memorije i poboljšali performanse mreže pomoću spajanja.

Dodatne informacije potražite na stranici [ASP.NET sesije stanje davatelja Azure predmemoriju Redis](redis-cache/cache-aspnet-session-state-provider.md) na Microsoftovu web-mjestu.

> [AZURE.NOTE] Pomoću davatelja stanje sesije Azure predmemoriju Redis s aplikacijama ASP.NET koji se izvode izvan Azure okruženje. Latencija pristupa u predmemoriji izvan Azure možete isključiti performanse prednosti predmemoriranja podataka.

Isto tako, izlazne predmemorije davatelj usluga za Azure Redis predmemorije omogućuje spremanje odgovora HTTP generira ASP.NET web-aplikacije. Davatelj usluga za izlazne predmemorije pomoću Azure Redis predmemorije možete poboljšati vrijeme odaziva aplikacija koje se iscrtavaju složene HTML izlaz. Instance aplikacije koje generiranje slične odgovore olakšavaju korištenje fragmentirane zajedničke izlazne predmemorije umjesto generiranje ovaj HTML afresh izlaz. Dodatne informacije potražite na stranici [ASP.NET izlazne predmemorije davatelja Azure predmemoriju Redis](redis-cache/cache-aspnet-output-cache-provider.md) na Microsoftovu web-mjestu.

## <a name="building-a-custom-redis-cache"></a>Stvaranje prilagođene Redis predmemorije

Azure Redis predmemorije ponaša se kao façade podlozi Redis poslužiteljima. Trenutno podržava fiksni skup konfiguracije ali omogućavaju Klasteriranje Redis. Ako su vam potrebne napredne konfiguracijske koji je prekriveni predmemoriju Azure Redis (kao što su predmemorije veća od 53 GB) možete izraditi i glavnog računala poslužitelja Redis pomoću Azure virtualnih računala.

To je potencijalno složen proces jer možda ćete morati stvoriti nekoliko VMs će poslužiti kao glavni i podređene čvorove ako želite implementirati replikacije. Osim toga, ako želite stvoriti klaster, morate više matrica i podređene poslužitelja. Minimalna grupirani replikacije topologije koja omogućuje veliku dostupnosti i skalabilnost sastoji se od najmanje šest VMs organizirane kao tri parove matrica/podređeni poslužitelja (klaster mora sadržavati najmanje tri osnovne čvorove).

Svaki par matrica/podređeni moraju nalaziti bliže da biste minimizirali Latencija. Međutim, svaki skup parove mogu imati u različitim Azure podatkovnim centrima koja se nalazi u različitim područjima ako želite da biste pronašli predmemoriranim podacima blizu aplikacije koje se najčešće koristi. Stranice [Radi Redis na VM za Linux CentOS u Azure](http://blogs.msdn.com/b/tconte/archive/2012/06/08/running-redis-on-a-centos-linux-vm-in-windows-azure.aspx) na web-mjestu Microsoft vodi kroz primjer u kojem se prikazuje stvaranje i konfiguriranje Redis čvor izvodi kao VM programa Azure.

[AZURE.NOTE] Napominjemo da Ako implementirate Redis predmemoriju na taj način, vi ste odgovorni za nadzor, upravljanje i zaštita servis.

## <a name="partitioning-a-redis-cache"></a>Particija Redis predmemorije

Particija predmemoriju uključuje Podjela predmemoriju na više računala. Ovu strukturu nudi nekoliko prednosti pomoću poslužitelja za jednu predmemorije, uključujući:

- Stvaranje predmemoriju koja je mnogo veća od mogu spremiti na jednom poslužitelju.
- Raspodjela podataka na poslužiteljima, poboljšanje dostupnosti. Ako jedan poslužitelj ne uspije ili ne može pristupiti, podatke koje on sadrži nije dostupan, ali podaci na preostale poslužiteljima i dalje može pristupiti. Predmemoriju, to nije presudne jer je predmemorirani podaci samo tranzitne kopiju podataka koji se održava u bazi podataka. Predmemorirani podaci na poslužitelju koji ne može pristupiti mogu biti predmemorirane na drugi poslužitelj umjesto toga.
- Širenje opterećenje na poslužiteljima, čime ćete poboljšati performanse i skalabilnost.
- Zatvorite Geolocating podataka pristupati, korisnicima tako smanjuju Latencija.

Predmemoriju, najčešće obliku particija je sharding. U ovom strategije svaki particija (ili shard) je Redis predmemorije u vlastitom desno. Podatke je preusmjereni na određene particija pomoću sharding logike koje možete koristiti na različite načine pristupaju distribuirati podatke. [Uzorak Sharding](http://msdn.microsoft.com/library/dn589797.aspx) pruža dodatne informacije o implementacijom sharding.

Možete implementirati particija u predmemoriji za Redis, koje možete poduzeti neki od sljedećih načina:

- _Usmjeravanje upita na strani poslužitelja._ U ovu tehniku klijentska aplikacija šalje zahtjev na bilo koju od poslužiteljima Redis koji čine predmemorije (vjerojatno najbliže server). Svaki poslužitelj Redis pohranjuje metapodataka koji opisuje particija da ga sadrži, a sadrži i podatke o kojem se nalaze particije na drugim poslužiteljima. Poslužitelj Redis pregledava zahtjev klijenta. Ako se može riješiti lokalno, će izvršiti traženu operaciju. U suprotnom je proslijedite zahtjev na odgovarajući poslužitelj. Ovaj model implementira se putem Redis Klasteriranje pa je podrobnije opisan u na stranici [Redis klaster vodič](http://redis.io/topics/cluster-tutorial) Redis web-mjesta. Klasteriranje redis je proziran s klijentskim aplikacijama, a dodatne Redis poslužitelji mogu dodati klaster (i podacima ponovno particije) bez ponovo konfigurirati klijente.

- _Klijentsko particija._ U ovom modelu klijentske aplikacije sadrži logiku (vjerojatno u obliku biblioteke) koji usmjerava zahtjeva za odgovarajući poslužitelj za Redis. Taj se način može se koristiti s Azure Redis predmemoriju. Stvaranje više Azure Redis predmemorije (jedan za svaki particija podataka) i implementacija klijentsko logike koji usmjerava zahtjeve točan predmemoriju. Ako shema particioniranja Promijeni (Ako je predmemorije Redis dodatne Azure stvaraju, na primjer), treba rekonfigurirati možda će biti potrebno klijentske aplikacije.

- _Proxy-Potpomognuta particija._ U ovoj shemi klijentskim aplikacijama Pošalji zahtjeve za servis za privremene proxy poslužitelja koje možete koristiti kako je particije podatke i zatim usmjerava zahtjev na odgovarajuće Redis poslužitelja. Taj se način mogu se koristiti s Azure Redis predmemorije. servis proxy može se implementirati kao Azure oblaku. Taj se način zahtijeva dodatnu razinu složenosti implementira servis, a zahtjevi za može trajati dulje da biste izvršili od korištenja klijentsko particija.

Na stranici [Partitioning: dijeljenju podataka između više instanci Redis](http://redis.io/topics/partitioning) na na Redis web-mjesto pruža dodatne informacije o implementacijom particija s Redis.

### <a name="implement-redis-cache-client-applications"></a>Implementacija Redis predmemorije klijentske aplikacije

Redis podržava klijentske aplikacije na brojne programiranje jezicima. Ako su stvaranje nove aplikacija pomoću .NET Framework, preporučeni način je da biste koristili biblioteku StackExchange.Redis klijenta. Ova biblioteka nudi .NET Framework objektni model koji se abstracts detalje o povezivanja s poslužiteljem Redis, naredbe za slanje i primanje odgovore. Nije dostupan u Visual Studio kao paket NuGet. Možete koristiti isti biblioteke za povezivanje programa Azure Redis predmemorije ili prilagođeni Redis predmemorije hostirane na na VM.

Da biste se povezali s poslužiteljem Redis koristite u statični `Connect` način u `ConnectionMultiplexer` predmete. Vezu koja stvara ovu metodu namijenjen je koristiti u cijeloj vijek klijentska aplikacija, a više Istodobni niti mogu koristiti istu vezu. Povežite i prekinuti svaki put izvođenje operacije na Redis jer je to mogu smanjiti performanse.

Možete navesti parametre veze, kao što su adresu web-mjesto glavnog računala Redis i u okvir za lozinku. Ako koristite Azure Redis predmemorije, lozinka je ili primarni ili sekundarni ključ koji je generirao za predmemoriju Redis Azure pomoću portala za upravljanje Azure.

Nakon povezivanja s poslužiteljem Redis, možete dobiti bolji Redis bazu podataka na kojoj se ponaša kao predmemoriju. Nudi veze Redis na `GetDatabase` način da biste to učinili. Možete dohvatiti stavke iz predmemorije i spremanje podataka u predmemoriju pomoću na `StringGet` i `StringSet` metode. Ovih metoda očekivati ključ kao parametar i vratiti stavke u predmemoriju odgovarajuće vrijednosti (`StringGet`) ili dodali stavku u predmemoriju uz (`StringSet`).

Ovisno o lokaciji poslužitelja Redis puno operacija može doći do neke Latencija dok zahtjeva prenosi na poslužitelj i odgovor, vraća se klijentu. Biblioteka StackExchange navode asinkronog verzije mnoge od načina koji će ga prikazati da biste lakše klijentske aplikacije ostaju odredište. Načini podržava [Utemeljenoj na zadacima asinkronog uzorak](http://msdn.microsoft.com/library/hh873175.aspx) u .NET Framework.

Sljedeći isječak koda prikazuje način pod nazivom `RetrieveItem`. Prikazuju implementacija predmemorije aside uzorka temelji na Redis i StackExchange biblioteke. Način vodi ključa vrijednost niza i pokušava dohvatiti odgovarajuću stavku iz predmemorije Redis tako da nazovete na `StringGetAsync` način (asinkronog verziju `StringGet`).

Ako stavku ne pronađe, je dohvaćanja pomoću u podlozi podataka izvora u `GetItemFromDataSourceAsync` način (što je lokalni metodu i nije dio biblioteke StackExchange). Zatim dodavanja u predmemoriju pomoću na `StringSetAsync` način da je vratite brže sljedeći put.

```csharp
// Connect to the Azure Redis cache
ConfigurationOptions config = new ConfigurationOptions();
config.EndPoints.Add("<your DNS name>.redis.cache.windows.net");
config.Password = "<Redis cache key from management portal>";
ConnectionMultiplexer redisHostConnection = ConnectionMultiplexer.Connect(config);
IDatabase cache = redisHostConnection.GetDatabase();
...
private async Task<string> RetrieveItem(string itemKey)
{
    // Attempt to retrieve the item from the Redis cache
    string itemValue = await cache.StringGetAsync(itemKey);

    // If the value returned is null, the item was not found in the cache
    // So retrieve the item from the data source and add it to the cache
    if (itemValue == null)
    {
        itemValue = await GetItemFromDataSourceAsync(itemKey);
        await cache.StringSetAsync(itemKey, itemValue);
    }

    // Return the item
    return itemValue;
}
```

Na `StringGet` i `StringSet` metode nisu ograničena dohvaćanje ili spremanje vrijednosti niza. One mogu preuzeti bilo koju stavku koja je serijalizirani kao polje bajtova. Ako je potrebno spremanje objekta .NET serijalizirati kao tok bajt i koristiti u `StringSet` način pisanja u predmemoriju.

Isto tako, možete čitati objekta iz predmemorije pomoću na `StringGet` metodu i prekidanja serije kao objekt .NET. Sljedeći kod prikazuje skup proširenje metode IDatabase sučelja (u `GetDatabase` način povezivanja Redis vraća programa `IDatabase` objekta), i neke ogledne kodu koji koristi ovih načina za čitanje i pisanje na `BlogPost` objekta u predmemoriju:

```csharp
public static class RedisCacheExtensions
{
    public static async Task<T> GetAsync<T>(this IDatabase cache, string key)
    {
        return Deserialize<T>(await cache.StringGetAsync(key));
    }

    public static async Task<object> GetAsync(this IDatabase cache, string key)
    {
        return Deserialize<object>(await cache.StringGetAsync(key));
    }

    public static async Task SetAsync(this IDatabase cache, string key, object value)
    {
        await cache.StringSetAsync(key, Serialize(value));
    }

    static byte[] Serialize(object o)
    {
        byte[] objectDataAsStream = null;

        if (o != null)
        {
            BinaryFormatter binaryFormatter = new BinaryFormatter();
            using (MemoryStream memoryStream = new MemoryStream())
            {
                binaryFormatter.Serialize(memoryStream, o);
                objectDataAsStream = memoryStream.ToArray();
            }
        }

        return objectDataAsStream;
    }

    static T Deserialize<T>(byte[] stream)
    {
        T result = default(T);

        if (stream != null)
        {
            BinaryFormatter binaryFormatter = new BinaryFormatter();
            using (MemoryStream memoryStream = new MemoryStream(stream))
            {
                result = (T)binaryFormatter.Deserialize(memoryStream);
            }
        }

        return result;
    }
}
```

Sljedeći kod ilustrira način pod nazivom `RetrieveBlogPost` koji koristi ovih metoda proširenja za čitanje i pisanje na serializable `BlogPost` objekta u predmemoriju pratiti uzorak predmemorije aside:

```csharp
// The BlogPost type
[Serializable]
private class BlogPost
{
    private HashSet<string> tags = new HashSet<string>();

    public BlogPost(int id, string title, int score, IEnumerable<string> tags)
    {
        this.Id = id;
        this.Title = title;
        this.Score = score;
        this.tags = new HashSet<string>(tags);
    }

    public int Id { get; set; }
    public string Title { get; set; }
    public int Score { get; set; }
    public ICollection<string> Tags { get { return this.tags; } }
}
...
private async Task<BlogPost> RetrieveBlogPost(string blogPostKey)
{
    BlogPost blogPost = await cache.GetAsync<BlogPost>(blogPostKey);
    if (blogPost == null)
    {
        blogPost = await GetBlogPostFromDataSourceAsync(blogPostKey);
        await cache.SetAsync(blogPostKey, blogPost);
    }

    return blogPost;
}
```

Redis podržava naredbe pipelining Ako klijentska aplikacija šalje više asinkronog zahtjeva. Redis možete multiplex zahtjeva za korištenje iste veze umjesto primanje poruka i odgovaranje na naredbi u nizu točno.

Taj se način omogućuje da biste smanjili Latencija tako da učinkovitije korištenje mreže. Sljedeći isječak koda prikazuje primjer koji dohvaća detalje o dva korisnika istodobno. Kod šalje dva zahtjeva, a zatim neki drugi obradu (koji nisu prikazani) prije čeka se primanje rezultate. Na `Wait` metode objekta predmemorije je slična .NET Framework `Task.Wait` metoda:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
var task1 = cache.StringGetAsync("customer:1");
var task2 = cache.StringGetAsync("customer:2");
...
var customer1 = cache.Wait(task1);
var customer2 = cache.Wait(task2);
```

Stranica [dokumentaciju Azure Redis predmemorije](https://azure.microsoft.com/documentation/services/cache/) na web-mjestu Microsoft pruža dodatne informacije o pisanju klijentske aplikacije koje možete koristiti predmemoriju Redis Azure. Dodatne informacije o dostupna je na [korištenje osnovne stranice](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Basics.md) na web-mjestu StackExchange.Redis.

[Kanali i multiplexers](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/PipelinesMultiplexers.md) stranica na web-mjestu isti pruža dodatne informacije o asinkronog operacije i pipelining s Redis i StackExchange biblioteke.  U sljedećem odjeljku u ovom članku, pomoću Redis predmemoriranje sadrži primjere nekih naprednije tehnike koje možete primijeniti na podatke koji čuva se Redis predmemoriju.

## <a name="using-redis-caching"></a>Korištenje Redis predmemoriranja

Najjednostavniji korištenje Redis za predmemoriranje opasnosti nije parovima ključnih vrijednosti u kojima je vrijednost uninterpreted niz proizvoljne duljine koja sadrži binarne podatke. (To je zapravo polja bajtova koji se mogu tretirati kao niz znakova). Ovaj scenarij je prikazana u klijentskim aplikacijama sekcije predmemorije Redis implementacija ranije u ovom članku.

Imajte na umu da tipke također sadrže uninterpreted podataka da biste mogli koristiti binarne podatke kao ključ. Na dulje ključno je, no više prostora će trebati za pohranu i na dulje će trebati za izvođenje operacije pretraživanja. Upotrebljivosti i jednostavnog održavanja, pažljivo dizajnirati svoje keyspace i tipkama smisleni (ali ne opširno).

Za predstavljanje tipku kupca sa ID 100, a ne samo "100", na primjer, koristite strukturirane tipke kao što su "kupca: 100". Ovu shemu omogućuje vam da jednostavno razlikovali vrijednosti koja sadržavati različite vrste podataka. Na primjer, tipku "Narudžbe: 100" nije moguće koristiti za predstavljanje ključ za redoslijed sa 100 ID-a.

Osim jednodimenzionalnih binarni nizovi, vrijednost u paru Redis ključa vrijednosti mogu sadržavati više strukturirane podatke, uključujući popise, postavlja (koji su sortirani i sortirano), a raspršivanja. Redis nudi skup potpun naredbe koje možete upravljati tim vrstama i mnoge od tih naredbi dostupnih za .NET Framework aplikacije putem klijenta biblioteke kao što su StackExchange. Stranica [Uvod u Redis vrste podataka i abstractions](http://redis.io/topics/data-types-intro) na web-stranici Redis sadrži detaljnije pregled ove vrste i naredbe koje možete koristiti da biste upravljali njima.

U ovom se odjeljku navedene ponekad zajednički koristi za ove vrste podataka i naredbe.

### <a name="perform-atomic-and-batch-operations"></a>Izvođenje atomske i obrada operacije

Redis podržava niz atomske operacije get i skup vrijednosti niza. Te operacije uklanjanje trkaći moguće opasnosti koje se mogu pojaviti prilikom korištenja zasebnom `GET` i `SET` naredbe. Operacije koje su dostupne obuhvaćaju sljedeće:

- `INCR`, `INCRBY`, `DECR`, i `DECRBY`, koji izvođenje atomske povećanje i smanjenje operacija na cijeli broj numeričkih podataka vrijednosti. Biblioteka StackExchange omogućuje prepun verzijama u `IDatabase.StringIncrementAsync` i `IDatabase.StringDecrementAsync` načina za izvođenje te operacije i vratili se vrijednost rezultata koji je pohranjen u predmemoriji. Sljedeći isječak koda ilustrira kako pomoću ovih metoda:

  ```csharp
  ConnectionMultiplexer redisHostConnection = ...;
  IDatabase cache = redisHostConnection.GetDatabase();
  ...
  await cache.StringSetAsync("data:counter", 99);
  ...
  long oldValue = await cache.StringIncrementAsync("data:counter");
  // Increment by 1 (the default)
  // oldValue should be 100

  long newValue = await cache.StringDecrementAsync("data:counter", 50);
  // Decrement by 50
  // newValue should be 50
  ```

- `GETSET`, koje dohvaća vrijednost koja je povezana s ključem i mijenja u novu vrijednost. Biblioteka StackExchange ovaj postupak postaje dostupan putem na `IDatabase.StringGetSetAsync` način. Ispod isječak koda prikazuje primjer ove metode. Kod vraća trenutnu vrijednost pridružen tipku "podataka: brojač" iz prethodnog primjera. Zatim ga ponovno postavlja vrijednost za ovaj ključ natrag na nulu, sve kao dio iste operacije:

  ```csharp
  ConnectionMultiplexer redisHostConnection = ...;
  IDatabase cache = redisHostConnection.GetDatabase();
  ...
  string oldValue = await cache.StringGetSetAsync("data:counter", 0);
  ```

- `MGET`i `MSET`, koji se možete vratiti ili promijenite skup vrijednosti niza kao jedne operacije. Na `IDatabase.StringGetAsync` i `IDatabase.StringSetAsync` metode su preopterećene da biste podržavaju tu funkciju, kao što je prikazano u sljedećem primjeru:

  ```csharp
  ConnectionMultiplexer redisHostConnection = ...;
  IDatabase cache = redisHostConnection.GetDatabase();
  ...
  // Create a list of key-value pairs
  var keysAndValues =
      new List<KeyValuePair<RedisKey, RedisValue>>()
      {
          new KeyValuePair<RedisKey, RedisValue>("data:key1", "value1"),
          new KeyValuePair<RedisKey, RedisValue>("data:key99", "value2"),
          new KeyValuePair<RedisKey, RedisValue>("data:key322", "value3")
      };

  // Store the list of key-value pairs in the cache
  cache.StringSet(keysAndValues.ToArray());
  ...
  // Find all values that match a list of keys
  RedisKey[] keys = { "data:key1", "data:key99", "data:key322"};
  RedisValue[] values = null;
  values = cache.StringGet(keys);
  // values should contain { "value1", "value2", "value3" }
  ```

Možete kombinirati i više operacije u jednom transakcijom Redis kao što je opisano u odjeljku Redis transakcije i serije ranije u ovom članku. Biblioteka StackExchange pruža podršku za transakcije putem na `ITransaction` sučelja.

Stvorite programa `ITransaction` objekt pomoću na `IDatabase.CreateTransaction` način. Pozivanje naredbe za transakcije načine na koje ste dobili od na `ITransaction` objekt.

Na `ITransaction` sučelja omogućuje pristup skup načina koji je sličan pristupati na `IDatabase` sučelja, osim što se sve načine asinkronog. To znači da su samo kada je izvesti u `ITransaction.Execute` način se poziva. Vrijednost koja se vraća na `ITransaction.Execute` način označava transakcije stvoren uspješno (istinito) ili ako je uspio (false).

Sljedeći isječak koda prikazuje primjer mjerača dva te pomacima i decrements kao dio iste transakcije:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
ITransaction transaction = cache.CreateTransaction();
var tx1 = transaction.StringIncrementAsync("data:counter1");
var tx2 = transaction.StringDecrementAsync("data:counter2");
bool result = transaction.Execute();
Console.WriteLine("Transaction {0}", result ? "succeeded" : "failed");
Console.WriteLine("Result of increment: {0}", tx1.Result);
Console.WriteLine("Result of decrement: {0}", tx2.Result);
```

Imajte na umu da Redis transakcije za razliku od transakcije u relacijskim bazama podataka. Na `Execute` način jednostavno redovi sve naredbe koje čine transakcije će se pokrenuti i ako ih je oblikovan zatim transakcije se zaustaviti. Ako su sve naredbe u redu uspješno, svaka naredba pokreće asinkrono.

Ako bilo kojoj naredbi ne uspije, na druge i dalje nastavite obrada. Ako vam je potrebna potvrda da je uspješno dovršena naredbu, morate dohvatite rezultatima naredbe korištenjem svojstva **rezultat** odgovarajuće zadatka, kao što je prikazano u primjeru iznad. Čitanje **rezultata** svojstvo blokira pozivanja niti dok je zadatak dovršen.

Dodatne informacije potražite u članku stranici [transakcija u Redis](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Transactions.md) StackExchange.Redis web-mjesta.

Izvođenje operacije grupe, možete koristiti u `IBatch` sučelja StackExchange biblioteke. Ovo sučelje omogućuje pristup skup metode sličan je pristupiti s `IDatabase` sučelja, osim što se sve načine asinkronog.

Stvorite programa `IBatch` objekt pomoću na `IDatabase.CreateBatch` način, a zatim pokrenite grupu pomoću na `IBatch.Execute` način, kao što je prikazano u sljedećem primjeru. Kod jednostavno postavlja vrijednost niza, pomacima i decrements isti mjerača koji se koriste u prethodnom primjeru i prikazuje rezultate:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
IBatch batch = cache.CreateBatch();
batch.StringSetAsync("data:key1", 11);
var t1 = batch.StringIncrementAsync("data:counter1");
var t2 = batch.StringDecrementAsync("data:counter2");
batch.Execute();
Console.WriteLine("{0}", t1.Result);
Console.WriteLine("{0}", t2.Result);
```

Važno da biste shvatili da za razliku od transakcije, naredba u seriji ne uspije jer je oblikovan, ostale naredbe i dalje naići je. Na `IBatch.Execute` način vratili oznaka uspjelo ili nije.

### <a name="perform-fire-and-forget-cache-operations"></a>Izvođenje fire i zaboraviti predmemorije operacije

Redis fire podržava i zaboraviti operacije pomoću naredbe zastavice. U tom slučaju klijent jednostavno pokreće postupak, ali je nema kamate u rezultatu i pričekajte izvršiti naredbu. Sljedećem primjeru pokazuje kako izvesti naredbu INCR kao u fire i zaboraviti postupak:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
await cache.StringSetAsync("data:key1", 99);
...
cache.StringIncrement("data:key1", flags: CommandFlags.FireAndForget);
```

### <a name="specify-automatically-expiring-keys"></a>Odredite automatski istekle tipke

Kada spremate stavke u predmemoriji za Redis, možete navesti vremensko ograničenje nakon kojeg se stavke automatski će se ukloniti iz predmemorije. Možete upita i koliko je još jedanput ključ je prije no što istekne pomoću na `TTL` naredbe. Ta je naredba dostupna StackExchange aplikacijama pomoću na `IDatabase.KeyTimeToLive` način.

Sljedeći isječak koda pokazuje kako postaviti vrijeme isteka 20 sekundi na ključ i upit preostali vijek ključa:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
// Add a key with an expiration time of 20 seconds
await cache.StringSetAsync("data:key1", 99, TimeSpan.FromSeconds(20));
...
// Query how much time a key has left to live
// If the key has already expired, the KeyTimeToLive function returns a null
TimeSpan? expiry = cache.KeyTimeToLive("data:key1");
```

Vrijeme isteka možete postaviti na određeni datum i vrijeme pomoću naredbe ISTEKA koji je dostupan u biblioteci StackExchange kao u `KeyExpireAsync` metoda:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
// Add a key with an expiration date of midnight on 1st January 2015
await cache.StringSetAsync("data:key1", 99);
await cache.KeyExpireAsync("data:key1",
    new DateTime(2015, 1, 1, 0, 0, 0, DateTimeKind.Utc));
...
```

> _Savjet:_ Možete ručno uklonite neku stavku iz predmemorije pomoću naredbe DEL koji je dostupan putem StackExchange biblioteke kao u `IDatabase.KeyDeleteAsync` način.

### <a name="use-tags-to-cross-correlate-cached-items"></a>Pomoću oznaka unakrsno povezivanje predmemorirane stavke

Skup Redis je zbirka više stavki koje zajedničko korištenje jedne tipke. Skup možete stvoriti pomoću naredbe SADD. Pomoću naredbe SMEMBERS možete dohvatiti stavke u skupu. Biblioteka StackExchange implementira SADD naredbu na `IDatabase.SetAddAsync` metodu i na SMEMBERS naredbe s na `IDatabase.SetMembersAsync` način.

Možete kombinirati i postojeće skupove da biste stvorili novi skupovi pomoću SDIFF (Postavljanje razlika), SINTER (Postavljanje sjecište) i naredbe SUNION (Postavljanje unija). Biblioteka StackExchange ujednačuje te operacije na `IDatabase.SetCombineAsync` način. Prvi parametar za ovu metodu određuje skup operaciju koja će se izvršiti.

Sljedeće koda pokazuju kako skupove mogu poslužiti za brzo pohranu i dohvaćanje zbirke povezanih stavki. Koristi kod u `BlogPost` vrsta koje je opisano u odjeljku implementacija Redis predmemorije klijentske aplikacije ranije u ovom članku.

A `BlogPost` objekt sadrži četiri polja – ID-ove, naslov, rangiranja rezultata i zbirke oznaka. Prvi koda ispod prikazuje ogledne podatke koji se koristi za popunjavanje C# popis `BlogPost` objekata:

```csharp
List<string[]> tags = new List<string[]>()
{
    new string[] { "iot","csharp" },
    new string[] { "iot","azure","csharp" },
    new string[] { "csharp","git","big data" },
    new string[] { "iot","git","database" },
    new string[] { "database","git" },
    new string[] { "csharp","database" },
    new string[] { "iot" },
    new string[] { "iot","database","git" },
    new string[] { "azure","database","big data","git","csharp" },
    new string[] { "azure" }
};

List<BlogPost> posts = new List<BlogPost>();
int blogKey = 0;
int blogPostId = 0;
int numberOfPosts = 20;
Random random = new Random();
for (int i = 0; i < numberOfPosts; i++)
{
    blogPostId = blogKey++;
    posts.Add(new BlogPost(
        blogPostId,               // Blog post ID
        string.Format(CultureInfo.InvariantCulture, "Blog Post #{0}",
            blogPostId),          // Blog post title
        random.Next(100, 10000),  // Ranking score
        tags[i % tags.Count]));   // Tags--assigned from a collection
                                  // in the tags list
}
```

Za svaki unos koji vam omogućuje pohranu oznake `BlogPost` objekt kao što je postavljeno u predmemoriju Redis i pridružiti svaki skup ID na `BlogPost`. Time se omogućuje aplikacije da biste brzo pronašli sve oznake koje pripadaju određene bloga. Da biste omogućili pretraživanje u suprotnom smjeru i pronalaženje svih članaka za blog zajednički koristite određenih oznaka, možete stvoriti drugi skup koja sadrži objava na blogu o pozivanju ID oznake u ključu:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
// Tags are easily represented as Redis Sets
foreach (BlogPost post in posts)
{
    string redisKey = string.Format(CultureInfo.InvariantCulture,
        "blog:posts:{0}:tags", post.Id);
    // Add tags to the blog post in Redis
    await cache.SetAddAsync(
        redisKey, post.Tags.Select(s => (RedisValue)s).ToArray());

    // Now do the inverse so we can figure how which blog posts have a given tag
    foreach (var tag in post.Tags)
    {
        await cache.SetAddAsync(string.Format(CultureInfo.InvariantCulture,
            "tag:{0}:blog:posts", tag), post.Id);
    }
}
```

Ove strukture omogućuju učinkovitije vrlo izvršavanje brojne uobičajene upite. Na primjer, možete pronaći i prikažite sve oznake na blogu 1 ovako:

```csharp
// Show the tags for blog post #1
foreach (var value in await cache.SetMembersAsync("blog:posts:1:tags"))
{
    Console.WriteLine(value);
}
```

Možete pronaći sve oznake koje su zajedničke blog objavljuju 1 i bloga objavu 2 izvođenjem operacije sjecište skup, na sljedeći način:

```csharp
// Show the tags in common for blog posts #1 and #2
foreach (var value in await cache.SetCombineAsync(SetOperation.Intersect, new RedisKey[]
    { "blog:posts:1:tags", "blog:posts:2:tags" }))
{
    Console.WriteLine(value);
}
```

A možete pronaći svih članaka za blog koji sadrže određene oznake:

```csharp
// Show the ids of the blog posts that have the tag "iot".
foreach (var value in await cache.SetMembersAsync("tag:iot:blog:posts"))
{
    Console.WriteLine(value);
}
```

### <a name="find-recently-accessed-items"></a>Pronalaženje nedavno pristupiti stavki

Uobičajeni zadatka potrebno više aplikacija je da biste pronašli najčešće nedavno pristupiti stavke. Na primjer, web-mjestu bloga možda želite prikazivati informacije o zadnjim pročitanima članaka za blog.

Ta je funkcija možete implementirati pomoću popisa Redis. Popis Redis sadrži više stavki koje imaju isti ključa. Na popisu ponaša se kao reda čekanja dvostruku. Stavke možete automatske ili kraj popisa pomoću naredbe (desno automatske) RPUSH i LPUSH (lijevom automatske). Možete dohvatiti stavke iz ili kraj popisa pomoću naredbe LPOP i RPOP. Pomoću naredbe LRANGE i RASPOREDI možete vratiti i skup elemenata.

Koda ispod pokazuju kako možete izvršiti te operacije pomoću StackExchange biblioteke. Koristi kod u `BlogPost` vrste iz prijašnjih primjera. Kao što je za čitanje na blogu po korisniku, na `IDatabase.ListLeftPushAsync` način ih gura naslov članka za blog na popis koji je pridružen tipku "blog:recent_posts" u predmemoriji Redis.

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
string redisKey = "blog:recent_posts";
BlogPost blogPost = ...; // Reference to the blog post that has just been read
await cache.ListLeftPushAsync(
    redisKey, blogPost.Title); // Push the blog post onto the list
```

Kao što su za čitanje objava na blogu, naslovima pomiču se na istom popisu. Popis narudžbi prema redoslijed kojim su dodani naslove. Zadnjim pročitanima članaka za blog su prema lijevom kraju popisa. (Ako se isti bloga je više puta za čitanje, imat će više stavki na popisu.)

Naslovi zadnjim pročitanima objave možete prikazati pomoću na `IDatabase.ListRange` način. Ovaj postupak vodi ključ koji se nalazi na popisu, početnu točku i završne točke. Sljedeći kod dohvaća naslova na 10 članaka za blog (stavke od 0 do 9) na kraju lijevi popisa:

```csharp
// Show latest ten posts
foreach (string postTitle in await cache.ListRangeAsync(redisKey, 0, 9))
{
    Console.WriteLine(postTitle);
}
```

Imajte na umu da se `ListRangeAsync` način uklanjanje stavki s popisa. Da biste to učinili, možete koristiti u `IDatabase.ListLeftPopAsync` i `IDatabase.ListRightPopAsync` metode.

Da biste spriječili rast beskonačno na popisu, možete povremeno cull stavki po skraćivanje popisa. Isječak koda ispod objašnjava da biste uklonili sve, ali pet lijevi stavki s popisa:

```csharp
await cache.ListTrimAsync(redisKey, 0, 5);
```

### <a name="implement-a-leader-board"></a>Implementacija ploče vodeće crte

Prema zadanim postavkama stavki u skupu drže nisu u nekom određenom redoslijedu. Uređeni skup možete stvoriti pomoću naredbe ZADD (u `IDatabase.SortedSetAdd` način u biblioteci StackExchange). Stavke su poredane pomoću numeričku vrijednost naziva rezultat koji prikazuje se kao parametar naredbu.

Sljedeći isječak koda dodaje naslov članka na blogu numerirani popis. U ovom primjeru svaki blogu ima rezultat polje koje sadrži rangiranje za blog.

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
string redisKey = "blog:post_rankings";
BlogPost blogPost = ...; // Reference to a blog post that has just been rated
await cache.SortedSetAddAsync(redisKey, blogPost.Title, blogpost.Score);
```

Možete dohvatiti naslovi oglasa bloga i uzlaznim redoslijedom rezultat pomoću brojčane pokazatelje na `IDatabase.SortedSetRangeByRankWithScores` metoda:

```csharp
foreach (var post in await cache.SortedSetRangeByRankWithScoresAsync(redisKey))
{
    Console.WriteLine(post);
}
```

> [AZURE.NOTE] Biblioteka StackExchange i omogućuje na `IDatabase.SortedSetRangeByRankAsync` metodu koja vraća podatke redoslijedom rezultat, ali ne vraća rezultati.

Možete dohvatiti stavke silaznim redoslijedom rezultata, te ograničiti broj stavki koje je vratio pruža dodatne parametre da biste na `IDatabase.SortedSetRangeByRankWithScoresAsync` način. U sljedećem primjeru prikazuje naslove i pokazatelje na gornjoj 10 rangiranih bloga:

```csharp
foreach (var post in await cache.SortedSetRangeByRankWithScoresAsync(
                               redisKey, 0, 9, Order.Descending))
{
    Console.WriteLine(post);
}
```

Sljedeći primjer koristi u `IDatabase.SortedSetRangeByScoreWithScoresAsync` metode koje možete koristiti da biste ograničili stavke koje se vraćate onima koji ulaze unutar zadanog rezultat raspona:

```csharp
// Blog posts with scores between 5000 and 100000
foreach (var post in await cache.SortedSetRangeByScoreWithScoresAsync(
                               redisKey, 5000, 100000))
{
    Console.WriteLine(post);
}
```

### <a name="message-by-using-channels"></a>Poruka pomoću kanala

Osim ulozi podataka predmemorije, Redis server nudi porukama putem mehanizam za publisher pretplatnika visokih performansi. Klijentske aplikacije možete se pretplatiti na kanal i druge programe ili servise mogu objavljivati poruke kanal. Pretplata aplikacije ćete dobiti te poruke, a možete ih obrađivati.

Redis sadrži naredbu PRETPLATA za klijentske aplikacije da biste koristili da biste se pretplatili kanala. Ta se naredba očekuje naziv jednog ili više kanala na kojem će aplikacije prihvaća poruke. Sadrži biblioteku StackExchange na `ISubscription` sučelje, koje omogućuje .NET Framework aplikacije pretplatiti i objavite kanale.

Stvorite programa `ISubscription` objekt pomoću na `GetSubscriber` način povezivanja s poslužiteljem Redis. Zatim poslušajte poruke na kanalu pomoću na `SubscribeAsync` način taj objekt. Sljedeći primjer koda pokazuje kako se pretplatiti na kanal pod nazivom "poruka: blogPosts":

```csharp
ConnectionMultiplexer redisHostConnection = ...;
ISubscriber subscriber = redisHostConnection.GetSubscriber();
...
await subscriber.SubscribeAsync("messages:blogPosts", (channel, message) =>
{
    Console.WriteLine("Title is: {0}", message);
});
```

Prvi parametar u `Subscribe` je način naziv kanala. Ovaj naziv slijedi isti konvencije koje koriste tipke u predmemoriji. Naziv može sadržavati binarne podatke, iako nije Uputno koristiti relativno kratak, smisleni nizove kako bi se osigurala dobre performanse i maintainability.

Imajte na umu i da naziva koriste kanala razlikuje se od koji koristi tipke. To znači da može imati kanala i ključeva koji imaju isti naziv, iako to može učiniti kodu aplikacije teže da biste zadržali.

Drugi parametar je do prava akcija. Ovaj delegat asinkrono se pokreće svaki put kada nova poruka se prikazuje na kanal. U ovom se primjeru jednostavno prikazuje poruku na konzoli sustava (poruku će sadržavati i naslov članka na blogu).

Da biste objavili na kanal, aplikaciju možete koristiti naredbu Redis OBJAVLJIVANJE. Biblioteka StackExchange omogućuje na `IServer.PublishAsync` način za izvođenje ovog postupka. Sljedeći isječak koda prikazuje kako objaviti poruku "poruka: blogPosts" kanal:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
ISubscriber subscriber = redisHostConnection.GetSubscriber();
...
BlogPost blogpost = ...;
subscriber.PublishAsync("messages:blogPosts", blogPost.Title);
```

Postoji nekoliko točke upoznati o mehanizam objavljivanja/pretplate:

- Više pretplatnike možete se pretplatiti na istom kanalu, a sve primit će poruke koje su objavljene na taj kanal.
- Pretplatnike primati samo poruke koje su objavljene nakon što ih ste se pretplatili. Kanala su u međuspremniku, a kada je poruka objavljena, Infrastruktura Redis ih gura poruke za svaki pretplatnika i zatim uklanja.
- Prema zadanim postavkama poruke primiti pretplatnike redoslijedom u kojem se šalju. U sustavu iznimno aktivni s velikim brojem poruka i mnogo pretplatnike i izdavači sigurno uzastopnih isporuku poruke mogu usporiti performanse sustava. Ako ne ovisi svake poruke, a redoslijed je nevažan sadržaj, možete omogućiti istovremene obrade Redis sustav, čime se olakšava da biste poboljšali odziv. To možete postići u klijent za StackExchange postavljanjem PreserveAsyncOrder veze koristi pretplatnika FALSE:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
redisHostConnection.PreserveAsyncOrder = false;
ISubscriber subscriber = redisHostConnection.GetSubscriber();
```

## <a name="related-patterns-and-guidance"></a>Srodni uzoraka i upute

Sljedeći uzorak može biti važna za vašu situaciju kada implementirate predmemoriranje u aplikacija:

- [Uzorak predmemorije aside](http://msdn.microsoft.com/library/dn589799.aspx): ovaj uzorak opisuje postupak da biste učitali podatke na zahtjev u predmemoriju iz spremišta podataka. Ovaj uzorak pomaže ostao između podataka koji se održava u predmemoriji i podataka u izvornom pohrane podataka.
- [Uzorak Sharding](http://msdn.microsoft.com/library/dn589797.aspx) daje informacije o implementacijom vodoravni particija radi poboljšanja skalabilnost pri pohrani i pristup velike količine podataka.

## <a name="more-information"></a>Dodatne informacije

- Stranica [MemoryCache predmete](http://msdn.microsoft.com/library/system.runtime.caching.memorycache.aspx) na Microsoftovu web-mjestu
- [Dokumentacija Azure Redis predmemorije](https://azure.microsoft.com/documentation/services/cache/) stranice na Microsoftovu web-mjestu
- [Azure Redis predmemorije najčešća pitanja vezana uz](redis-cache/cache-faq.md) stranice na Microsoftovu web-mjestu
- [Konfiguriranje modela](http://msdn.microsoft.com/library/windowsazure/hh914149.aspx) stranice na Microsoftovu web-mjestu
- Na stranici [Utemeljenoj na zadacima asinkronog uzorak](http://msdn.microsoft.com/library/hh873175.aspx) Microsoftovo web-mjesto
- Stranici [kanali i multiplexers](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/PipelinesMultiplexers.md) repo StackExchange.Redis GitHub
- Na stranici [Redis postojanost](http://redis.io/topics/persistence) Redis web-mjesta
- [Replikacija stranice](http://redis.io/topics/replication) na web-mjestu Redis
- Na stranici [Redis klaster vodič](http://redis.io/topics/cluster-tutorial) Redis web-mjesta
- Na [Partitioning: dijeljenju podataka između više instanci Redis](http://redis.io/topics/partitioning) stranice na web-mjestu Redis
- Na stranici [Pomoću Redis kao u predmemoriji LRU](http://redis.io/topics/lru-cache) Redis web-mjesta
- Na stranici [transakcije](http://redis.io/topics/transactions) Redis web-mjesta
- Na stranici [Redis sigurnost](http://redis.io/topics/security) Redis web-mjesta
- Na stranici [prijenosni oko Azure Redis predmemorije](https://azure.microsoft.com/blog/2014/06/04/lap-around-azure-redis-cache-preview/) Azure bloga
- [Pokretanje Redis na VM za Linux CentOS u Azure](http://blogs.msdn.com/b/tconte/archive/2012/06/08/running-redis-on-a-centos-linux-vm-in-windows-azure.aspx) stranice na Microsoftovu web-mjestu
- Stranicom [ASP.NET davatelja usluga stanja sesije Azure predmemoriju Redis](redis-cache/cache-aspnet-session-state-provider.md) na Microsoftovu web-mjestu
- Stranica [platforme ASP.NET izlazne predmemorije davatelja Azure predmemoriju Redis](redis-cache/cache-aspnet-output-cache-provider.md) na Microsoftovu web-mjestu
- Stranice [Programa Uvod u Redis vrste podataka i abstractions](http://redis.io/topics/data-types-intro) Redis web-mjesta
- [Korištenje osnovne](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Basics.md) stranice na web-stranici StackExchange.Redis
- Na stranici [transakcija u Redis](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Transactions.md) StackExchange.Redis repo
- [Vodič za stvaranje particija podataka](http://msdn.microsoft.com/library/dn589795.aspx) na Microsoftovu web-mjestu
