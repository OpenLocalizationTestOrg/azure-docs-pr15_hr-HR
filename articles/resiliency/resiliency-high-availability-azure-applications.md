<properties
   pageTitle="Visoke dostupnosti za Azure aplikacije | Microsoft Azure"
   description="Tehnički pregled i detaljne informacije o dizajniranje i stvaranje aplikacija za visoke dostupnosti na Microsoft Azure."
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#<a name="high-availability-for-applications-built-on-microsoft-azure"></a>Visoke dostupnosti za aplikacije koje se temelji na Microsoft Azure

Iznimno dostupnih aplikacija absorbs prilagođavati razlikama u dostupnost, učitavanje i privremeni problemi s ovisne servise i hardver. Aplikacije i dalje raditi na prihvatljiva korisnika i razina systemic odgovor, kako je definirano parametrom poslovanja ili aplikacije razini usluge (SLA).

##<a name="azure-high-availability-features"></a>Azure visoku dostupnost značajki

Azure sadrži mnoge ugrađene platformu značajke koji podržavaju iznimno dostupna aplikacija. U ovom se odjeljku opisuju neke od tih značajki. Detaljnije analiza platforme, potražite u članku [Azure otpornost Tehnički vodič](./resiliency-technical-guidance.md).

###<a name="fabric-controller"></a>Tkanina kontroler

Kontrolerom Azure tkanina dodjeljuje i nadzire uvjet Azure računalnim instanci. Kontrolerom tkanina provjerava stanje hardvera i softvera strojno instanci glavno računalo i gosta. Kada se otkrije pogreške, nameće SLA tako da automatski relocating VM instance. Koncept dodatno kvara i nadogradnja domene podržava SLA računalnim.

Kada uvode se više instanci uloga u Oblaku, Azure uvodi ove instance različite kvara domene. Domene granicu kvara zapravo je različite hardverske bicikle u istom području. Domena kvara smanjiti vjerojatnost da lokalizirane hardverskog kvara će se zaustaviti servis aplikacije. Nije moguće upravljati broj kvara domene koje se dodijeliti tempiranja ili uloge web. Kontrolerom tkanina koristi namjenski resursa koje se razlikuju od Azure hostirane aplikacije. Vrijeme aktivnosti 100 posto ima jer služi kao nucleus Azure sustava. Nadzire i upravlja uloga instance svim kvara domenama.

Sljedeći dijagram prikazuje Azure zajedničkim resursima kontrolerom tkanina uvodi te upravlja preko različitih kvara domene.

![Pojednostavnjeni prikaz odvajanja kvara domene](./media/resiliency-high-availability-azure-applications/fault-domain-isolation.png)

Nadogradnje domene slični su kvara domena u funkciji, no oni podržavaju nadogradnje umjesto pogrešaka. Nadogradnja domena je logičke jedinice instancu odvojenosti koji određuje koje će se instance određenu uslugu nadogradit će se u trenutku u vremenu. Prema zadanim postavkama za implementaciju sustava servis definiraju pet nadogradnje domene. Međutim, možete promijeniti tu vrijednost u datoteci definicije servisa. Ako, na primjer, pretpostavimo da imate osam pojavljivanja vaša uloga web. Pojavit će se dvije instance u tri nadogradnje domene i dvije instance u jednu domenu za nadogradnju. Azure određuje redoslijed ažuriranja, ali se temelji na broju nadogradnje domene. Dodatne informacije o nadogradnji domena potražite u članku [Ažuriranje servis u oblaku](../cloud-services/cloud-services-update-azure-service.md).

###<a name="features-in-other-services"></a>Značajke u drugim servisima

Osim platformu značajke koje podržavaju visoke računalnim dostupnost Azure ulaže visoku dostupnost značajki u njegove servise. Ako, na primjer, Azure prostora za pohranu održava tri replike blob, tablice i red podataka. Omogućuje i mogućnost zemlj replikacije za spremanje sigurnosne kopije blob-ova i tablica u sekundarni regiji. Mrežni isporuku sadržaja za Azure omogućuje blob-ova predmemoriju diljem svijeta zalihosti i skalabilnost. Baze podataka SQL Azure održava kao i više replike.

Osim niz [otpornost Tehnički vodič](https://aka.ms/bctechguide) članaka potražite u članku [Najbolje prakse za usluge dizajna Large-Scale na servise u Oblaku Azure](https://azure.microsoft.com/blog/best-practices-for-designing-large-scale-services-on-windows-azure/) papira. Ove radova pružaju dublju rasprave značajke dostupnosti platforme Azure.

Iako Azure sadrži više značajki koje podržavaju visoke dostupnosti, važno je znati njihove ograničenja:

- Za računalnim, Azure jamčiti da vaša uloga su dostupne i pokrenut, ali znati je li vaša aplikacija izvodi i prepun.
- Za baze podataka SQL Azure, podaci replicirati će se sinkronizirano unutar područja. Možete odabrati aktivne zemlj. – replikacije, koji omogućuje do četiri kopije dodatne baze podataka u istom području (ili različite regije). Ove replike baze podataka nisu točke u vrijeme sigurnosne kopije. Baze podataka SQL nude mogućnosti za sigurnosne kopije točke u vrijeme. Da biste saznali više o mogućnostima SQL baza podataka točke-u – vrijeme, pročitajte [Točke za baze podataka SQL Azure u vrijeme vratiti](https://azure.microsoft.com/blog/azure-sql-database-point-in-time-restore/).
- Za pohranu Azure podatkovne tablice i blob je replicirati po zadanom zamjenski regiju. Međutim, ne možete pristupiti na replike dok Microsoft odabire uvoza putem zamjenski web-mjesto. Područje prebacivanje pojavljuje samo ako tijekom duljih regija razini usluge prekidu i postoji bez SLA zemlj prebacivanje put. Također je imati na umu da bilo kakvo oštećenje podataka brzo širi na replike.

Zbog sljedećih razloga morate dodatku izjavi značajke dostupnosti platformu značajke dostupnosti specifičnim aplikacijama. Značajke dostupnosti specifičnim aplikacijama obuhvaćaju značajku snimke blobova platforme za stvaranje sigurnosne kopije točke u vrijeme blob podataka.

###<a name="availability-sets-for-azure-virtual-machines"></a>Dostupnost postavlja virtualnim računalima sustava za Azure

Većina ovog članka usredotočuje se na servise u oblaku, koji koristi platforma kao model servisa (PaaS). No postoje i određene dostupnost značajki za Azure virtualnim strojevima, koji koristi do infrastrukture kao model servisa (IaaS). Da biste postigli visoke dostupnosti s virtualnim strojevima, morate koristiti dostupnost skupove. Skup dostupnost služi slične funkcije kvara i nadogradnja domene. Unutar skupa dostupnost Azure smješta virtualnim strojevima na način koji sprječava prebacivanja dolje sve strojeva u toj grupi kvarove lokalizirane hardver i održavanje aktivnosti. Dostupnost skupove su potrebne da biste postigli SLA Azure dostupnosti virtualnim računalima.

Na sljedećem su dijagramu omogućuje prikaz dvaju skupova dostupnost toj grupi web i virtualnim računalima sustava SQL Server, odnosno.

![Dostupnost postavlja virtualnim računalima sustava za Azure](./media/resiliency-high-availability-azure-applications/availability-set-for-azure-virtual-machines.png)

>[AZURE.NOTE] U prethodnom dijagram, SQL Server je instalirana i pokrenuta na virtualnim računalima. Time se razlikuje od prethodne rasprave Azure SQL baze podataka, koji pruža bazu podataka kao servis za upravljane.

##<a name="application-strategies-for-high-availability"></a>Strategije aplikacije visoke dostupnosti

Većina strategije aplikacije visoke dostupnosti obuhvaćaju zalihosti ili uklanjanje teško međuzavisnosti aplikacije komponente. Dizajniranje aplikacije treba podržava kvara odstupanje tijekom povremeni gubitak nedostupnost Azure ili servisa drugih proizvođača. U sljedećim odjeljcima opisuju uzoraka aplikacije za povećanje dostupnost servisa u oblaku.

###<a name="asynchronous-communication-and-durable-queues"></a>Asinkrona komunikacija i durable reda čekanja

Razmislite o asinkronog komunikaciju između slobodnije povezano services da biste povećali dostupnosti u Azure aplikacije. U ovaj uzorak pisanje poruka reda čekanja za pohranu ili Bus servisa Azure redova za kasniju obradu. Prilikom pisanja poruke u red, kontrole se odmah vraća pošiljatelju poruke. Drugi sloju aplikacije rukuje poruke obrade, obično implementirana kao ulogu suradnika. Ako funkcionira ulogu suradnika, poruke se skupiti u redu čekanja dok vratit će se servis za obradu. Dok god redu čekanja postane dostupan, postoji bez Izravni ovisnosti između pristupne pošiljatelja i procesor poruke. Time se uklanja obavezu pozivi sinkrono servisa koji može biti usko grlo propusnost u raspodijeljeno aplikacijama.

Varijacije ovog koristi prostora za pohranu Azure (blob-ova, tablice, redove) ili servis Bus redova kao mjesto prebacivanje za pozive nije uspjelo baze podataka. Ako, na primjer, sinkrono poziv aplikacije na drugi servis (kao što su baze podataka SQL Azure) ne uspijeva više puta. Možda ćete moći serijalizirati tih podataka u durable prostora za pohranu. Kasnije trenutku kada usluga ili baza podataka se vratite u mrežni, aplikacija možete ponovno Pošalji zahtjev iz spremišta. Razlike u ovaj model je srednja mjesto nije konstantnim dio aplikacije tijeka rada. Koristi se samo u nekim scenarijima nije uspjelo.

U oba scenarija Asinkrona komunikacija i srednja prostora za pohranu onemogućuju downed servisa pozadinske prebacivanja dolje cijelu aplikaciju. Redovi služe kao u logičke privremene. Dodatne upute o odabiru točne stavljanja servisa potražite u članku [Azure redova "i" Redovi Bus servisa Azure – usporedbi i iz Outlooka nasuprot](../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md).

###<a name="fault-detection-and-retry-logic"></a>Logika kvara otkrivanje i pokušajte ponovno

Ključne točke u dizajnu iznimno dostupnih aplikacija je koristiti pokušaj logike unutar kod za obavljanje rukovanje servis privremeno prema dolje. [Tranzitne kvara zadužen za blokiranje aplikacije](https://msdn.microsoft.com/library/hh680934.aspx), razvio timsko web-mjesto Microsoft Patterns i u okvir za prakse pomaže razvojnim inženjerima ovog postupka. Riječ "prolaznim" znači privremeno koji traje samo za jedan relativno kratki. U kontekstu ovog članka, rukovanje tranzitne neuspjeha je dio razvoj iznimno dostupnih aplikacija. Tranzitne uvjeta Primjeri pogrešaka Povremeni mreže i prekinula se veza s bazom podataka.

Tranzitne bloka kvara zadužen za aplikaciju je pojednostavnjeni onako kako želite rukovati pogreške unutar graceful način. Možete je koristiti da biste poboljšali dostupnosti aplikacija dodavanjem robusne tranzitne kvara obradu logike. U većini slučajeva, pokušajte ponovno logiku rukuje kratak prekida i ponovno poveže pošiljatelj i primatelj nakon jednog ili više nije uspjelo pokušaja. Uspješan pokušaj pokušaj obično odlazak uočen korisnike aplikacije.

Razvojni inženjeri postoje tri mogućnosti za upravljanje njihove pokušaj logika: rastuće, fiksnim interval i eksponencijalne. Rastuća čekanje više prije svaki ponovno rastuće Linearni način (na primjer, 1, 2, 3 i 4 sekunde). FIXED interval čeka jednakih razmaka između svakog pokušaj (na primjer, dvije sekunde). Za više slučajni mogućnost na eksponencijalnom natrag slobodno čeka više između ponovne pokušaje. Međutim, koristi eksponencijalne ponašanje (na primjer, 2, 4, 8 i 16 sekunde).

Više razine strategije unutar je:

1. Definirajte pravila i pokušajte ponovno strategije.
1. Ponovite postupak koji može rezultirati tranzitne kvara.
1. Ako se pojavi tranzitne kvara, pozivanje pravila Ponovi.
1. Ako se svi ponovne pokušaje neće funkcionirati, pojavljivanje konačni iznimke.

Testirajte logiku pokušaj u Simulirani pogrešaka da biste bili sigurni da ponovne pokušaje na uzastopni operacije neće rezultirati nepredviđena dugotrajan odgode. Prije nego što odlučite uvoza cjelokupan zadatka, učinite sljedeće.

###<a name="reference-data-pattern-for-high-availability"></a>Uzorak podataka referenca za visoke dostupnosti

Referenca podaci samo za čitanje podataka aplikacije. Ove podatke nudi tvrtke kontekst u kojem aplikaciju generira transakcijskih podataka tijekom postupka tvrtke. Transakcijskih podataka je funkcija točke u vrijeme referentnih podataka. Dakle, njegov integritet ovisi o snimke referentnih podataka u trenutku transakcije. Ovo je Pomalo su definicija, ali ga treba suffice naš svrhu.

Referentnih podataka u kontekstu aplikacije nužni za funkcioniranje aplikacije. Odgovarajući aplikacije stvoriti i održavati podatke referencu. Sustavi upravljanja (MDM) glavnih podataka često izvesti tu funkciju. Ti sustavi odgovorni za životni ciklus referentnih podataka. Primjeri referentnih podataka uključuju kataloga proizvoda, Matrica zaposlenika, glavni dijelovi i opreme matrice. Pregled podataka možete potječu iz izvan tvrtke ili ustanove, na primjer, poštanske brojeve ili porezne stope. Strategije za povećanje dostupnosti referentnih podataka otežavaju obično manje od onih za transakcijskih podatke. Referentnih podataka ima prednost uglavnom immutable.

Možete unijeti Azure web i tempiranja uloge koje referentnih podataka Autonomna prilikom izvođenja uvođenjem referentnih podataka te aplikacije. Ako veličina lokalno spremište dopušta takve implementaciju, to je idealna stanje. Ugrađeni baze podataka (SQL, NoSQL) ili XML datoteke implementiran na lokalni datotečni sustav će vam pomoći s Autonomno područje Azure računalnim skaliranje jedinice. Međutim, imat ćete mehanizam da biste ažurirali podatke u svaku ulogu ne zahtijeva ponovno uvođenje. Da biste to učinili, postavite ažuriranja referentnih podataka za pohranu oblaka krajnje (na primjer, spremište blobova platforme Azure ili SQL baze podataka). Dodavanje koda za ulogama preuzima ažuriranja podataka u čvorove računalnim prilikom pokretanja uloge. Umjesto toga možete dodati kod koji omogućuje administrator da biste izvršili prisilne preuzimanja u instance uloge.

Da biste povećali dostupnost, uloge također mora sadržavati skupa podataka za referencu u slučaju da je prostor za pohranu prema dolje. Time se omogućuje uloge da biste započeli osnovni skup referentnih podataka dok je resursa za pohranu postane dostupna ažuriranja.

![Aplikacija visoke dostupnosti kroz Autonomna računalnim čvorove](./media/resiliency-high-availability-azure-applications/application-high-availability-through-autonomous-compute-nodes.png)

Jedan razmatranje za ovaj uzorak je implementacije i pokretanje brzinu svoje uloge. Ako su implementacija ili preuzimanje velikih količina podataka referenca pri pokretanju, to možete povećati koliko dugo traje Okretni novi implementacijama ili uloga instance. To može biti prihvatljiva tradeoff za Autonomno područje pojavljuju referentnih podataka odmah dostupan na ulogama umjesto ovisno o vanjskim prostora za pohranu servisa.

###<a name="transactional-data-pattern-for-high-availability"></a>Uzorak transakcijskih podataka za visoke dostupnosti

Transakcijskih podaci se nalaze podaci koje generira aplikacije u kontekstu tvrtke. Transakcijskih podaci su kombinacija skup poslovnih procesa koji implementira aplikacije i referentnih podataka koje podržava sljedeće postupke. Primjeri transakcijskih podataka možete uključiti narudžbe, napredne dostave obavijesti, faktura i prilika za upravljanje (CRM) odnosa klijenta. Transakcijskih podataka stoga generira će se umeće za vanjski sustavi za vođenje evidencije ili radi daljnje obrade.

Imajte na umu referenci tog podataka možete promijeniti u sustavima koji su odgovorni za te podatke. Zbog toga transakcijskih podataka morate spremiti podatkovni kontekst referenca točke u vrijeme tako da ima minimalnog vanjske ovisnosti njegov semantičkog dosljednost. Na primjer, razmotrite uklanjanje proizvoda iz kataloga nekoliko mjeseca nakon što je ispunjena reda. Najbolja praksa je ugrađivanje suvišni konteksta podataka reference kao izvedivo transakcije. To zadržava semantiku pridružene transakcije, čak i ako se mijenja referentnih podataka kada se hvata transakcije.

Kao što je već rečeno, arhitekturi koji koriste su coupling i Asinkrona komunikacija posuđivati same više razine dostupnost. To se odnosi i transakcijskih podataka, ali složeniji je implementacije. Tradicionalni transakcijskih notions obično je za bazu podataka za jamstva transakcije. Kada predstavljanje Srednja slojeve, kod aplikacije morate pravilno rukovati podacima na razne slojeve da biste bili sigurni dovoljno dosljednost i rok trajanja.

Sljedeći niz opisuje tijeka rada koji dijeli snimanje transakcijskih podatke iz njezinih obrada:

1. Čvor web računalnim: izlaganje odnose na podatke.
1. Vanjski prostora za pohranu: spremanje Srednja transakcijskih podataka.
1. Čvor web računalnim: dovršavanje transakcije krajnjeg korisnika.
1. Čvor web računalnim: slanje dovršene transakcijskih podataka, zajedno s podatkovni kontekst referenca privremeno durable spremište koje se zajamčeno da bi se dobilo predvidljivi odgovor.
1. Čvor web računalnim: signala krajnjeg korisnika nakon dovršetka transakcija.
1. Pozadine izračunati čvor: izdvajanje transakcijskih podataka, nakon obrade ako je potrebno i slanje na mjesto konačni prostora za pohranu u trenutni sustav.

Sljedeći dijagram prikazuje jednu moguće implementaciji sustava ovaj dizajn u programa Azure hostirane u oblaku.

![Visoke dostupnosti kroz su coupling](./media/resiliency-disaster-recovery-high-availability-azure-applications/application-high-availability-through-loose-coupling.png)

Crtkani strelice u prethodnom dijagramu označavaju asinkronog obrada. Uloga sučelja web nije svjesni ovaj asinkronog obrada. To potencijalnih klijenata za pohranu transakcije na odredište konačni with reference to trenutni sustav. Zbog Latencija koji predstavlja ovaj asinkronog model transakcijskih podataka nije odmah dostupna za upit. Zbog toga svaki jedinice transakcijskih podataka mora biti spremljena u predmemoriju ili mora sesije korisnika da bi odgovarao odmah korisničkog Sučelja.

Uloga web je Autonomna od ostatka Infrastruktura. Kombinacija ulogu web i Azure red i ne cijele Infrastruktura je njegov profil dostupnost. Osim visoke dostupnosti takvog omogućuje ulogu web skaliranja vodoravno, neovisno o pozadinske prostora za pohranu. Ovaj model visoku dostupnost možete imati utjecaj na economics operacije. Dodatne komponente kao što su Azure redovima i uloge suradnika može utjecati na mjesečni Upotreba troškovi.

Imajte na umu da prethodna dijagram prikazuje jednu implementacije takvog samostalne transakcijskih podataka. Postoji mnogo drugih moguće implementacije. Sljedeći popis sadrži neke Alternative:

 * Uloge suradnika mogu nalaziti između ulogu web i reda čekanja za pohranu.
 * Red čekanja Bus servisa može se koristiti umjesto red čekanja za Azure prostora za pohranu.
 * Konačni odredišni možda Azure prostora za pohranu ili davatelj usluga za drugu bazu podataka.
 * Azure predmemorije sloju web može se koristiti za davanje odmah predmemoriranja preduvjeti nakon transakcije.

###<a name="scalability-patterns"></a>Uzorci skalabilnost

Nije važno Imajte na umu da skalabilnost servisa u oblaku izravno utječe dostupnost. Ako Poboljšana učitavanja uzrokuje vaša usluga ne reagira, prikazuje se korisnik je aplikacija je prema dolje. Slijede preporučene postupke za skalabilnost ovisno o vašoj učitavanja očekivani aplikacije i buduće očekivanja. Najveći skale obuhvaća mnogo pitanja vezana uz, kao što su korištenje jedne nasuprot više prostora za pohranu računa, zajedničko korištenje preko više baza podataka i predmemoriranja strategije. Detaljnije pogledati te uzorke, potražite u članku [Najbolje prakse za usluge dizajna Large-Scale na servise u Oblaku Azure](https://azure.microsoft.com/blog/best-practices-for-designing-large-scale-services-on-windows-azure/).

##<a name="next-steps"></a>Daljnji koraci

Ovaj je članak dio niza članaka filtriran na [oporavak Izrada i visoke dostupnosti za aplikacije koje se temelji na Microsoft Azure](./resiliency-disaster-recovery-high-availability-azure-applications.md). Sljedeći članak u ovoj seriji je [Izrada oporavak za aplikacije koje se temelji na Microsoft Azure](./resiliency-disaster-recovery-azure-applications.md).
