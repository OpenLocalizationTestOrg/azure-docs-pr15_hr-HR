<properties
   pageTitle="Izrada oporavak za Azure aplikacije | Microsoft Azure"
   description="Tehnički pregled i detaljne informacije o dizajniranju aplikacije za oporavak Izrada na Microsoft Azure."
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

#<a name="disaster-recovery-for-applications-built-on-microsoft-azure"></a>Izrada oporavak za aplikacije koje se temelji na Microsoft Azure

Dok je visoke dostupnosti o upravljanju privremene pogreške, Izrada oporavak (DR) je o do teškog oštećenja gubitak funkcionalnosti aplikacije. Na primjer, razmotrite scenarij u kojem funkcionira područja. U ovom slučaju morate imati tarifu za pokretanje aplikacije ili pristup vašim podacima izvan Azure područja. Izvršavanje ovaj plan obuhvaća osobe, postupaka i podrške aplikacije koje omogućuju da funkcija sustav. Tvrtke i tehnologije vlasnika koji se definirati u sustavu operacijski način rada za na Izrada i određuju razinu funkcionalnosti za servis tijekom na Izrada. Razinu funkcionalnosti može potrajati nekoliko obrazaca: potpuno dostupan, djelomično dostupne (smanjena funkcionalnost ili odgoditi za obradu) ili potpuno dostupno.

##<a name="azure-disaster-recovery-features"></a>Izrada Azure oporavak značajke

Kao i kod dostupnost pitanja vezana uz Azure ima [otpornost Tehnički vodič](./resiliency-technical-guidance.md) koji vam omogućuje da podržava Izrada oporavak. Postoji odnos između značajke dostupnosti programa Azure i Izrada oporavak. Ako, na primjer, Upravljanje ulogama svim domenama kvara povećava dostupnosti aplikacija. Bez te upravljanje neuspio neobrađenu hardver će postati scenarij "Izrada". Pa je ispravnim zatvaranjem značajke dostupnosti i strategije važan dio Izrada provjeru aplikacije. Međutim, u ovom se članku prelazi problemi Općenito dostupan na događaje Izrada više ozbiljne (i rijetkim).

##<a name="multiple-datacenter-regions"></a>Više područja podatkovnim centrom

Azure održava podatkovnim centrima u više područja svijeta. Ova Infrastruktura podržava nekoliko scenarija oporavak Izrada, kao što je s-ovi sustava zemlj. – replikacije Azure prostora za pohranu sekundarne područja. Također znači da možete jednostavno i inexpensively implementirati servis u oblaku na više mjesta diljem svijeta. Usporedite to troška i poteškoće prilikom pokretanja vlastite podatkovnim centrima u više područja. Uvođenje podatke i usluge na više područja pomaže u zaštiti aplikaciju iz glavne kvarove u jednom području.

##<a name="azure-traffic-manager"></a>Azure Upravitelj promet

Kada dođe do pogreške namijenjena određenoj regiji, morate preusmjeravanje prometa na servisima ili implementacije u drugoj regiji. To možete učiniti ovo usmjeravanje ručno, ali je učinkovitiji pomoću automatiziranog procesa. Azure promet Upravitelj namijenjen je taj zadatak. Možete je koristiti za automatsko upravljanje prebacivanje korisnika promet na drugoj regiji u slučaju da ne uspije primarni regija. Budući da je promet upravljanje važan dio ukupnog strategije, važno je da biste shvatili osnove upravitelja promet.

U sljedećem dijagramu korisnici povezuju sa URL ADRESU koja je navedena za web-mjesto promet Manager (__http://myATMURL.trafficmanager.net__) i u okvir za taj sažecima stvarnog web-mjesta URL-ovima (__http://app1URL.cloudapp.net__ i __http://app2URL.cloudapp.net__). Ovisno o tome kako konfigurirati kriterije za za usmjeravanje korisnike, korisnike poslat će se točno stvarni web-mjesto kada određuje pravila. Mogućnosti pravila su-kružnog, performanse ili prebacivanje. Radi u ovom se članku smo bit će zamaraju samo mogućnost prebacivanja u slučaju pogreške.

![Usmjeravanje putem Azure promet Manager](./media/resiliency-disaster-recovery-azure-applications/routing-using-azure-traffic-manager.png)

Kada ste konfiguriranja Upravitelj promet, navedite novi predbroj promet upravitelja DNS-a. To je URL prefiks koji će vam omogućuje korisnicima da biste pristupili servisu. Upravitelj promet sada sažecima jednu razinu prema gore, a ne na razini regija za ujednačavanje opterećenja. DNS Manager promet mapira CNAME za implementacije koja je upravlja.

U upravitelju promet navedete prioritet uvođenja koje će biti proslijeđene korisnicima kada dođe do pogreške. Upravitelj promet nadzire krajnje točke implementacije i napomene kada ne uspije primarni implementacije. Kod pogreške, promet Upravitelj analizira popisu prioritet implementacije i usmjerava korisnika na sljedeću na popisu.

Iako promet Upravitelj odluči gdje se na je prebacivanje, možete odlučiti hoće li prebacivanje je vaša domena Neaktivni ili aktivnog dok niste u načinu rada za prebacivanje. Funkcija ima li ništa učiniti s Azure promet Manager. Upravitelj promet otkriva pogreške u primarni web-mjesta i prelaska na web-mjesto prebacivanje. Upravitelj promet prelaska bez obzira na to je li to web-mjesto je trenutno posluživanje korisnika ili ne.

Dodatne informacije o funkcioniranje Upravitelj promet Azure, pogledajte:

 * [Pregled upravitelja promet](../traffic-manager/traffic-manager-overview.md)
 * [Konfiguriranje metodu usmjeravanja prebacivanje](../traffic-manager/traffic-manager-configure-failover-routing-method.md)

##<a name="azure-disaster-scenarios"></a>Scenariji za Azure Izrada

U sljedećim se odjeljcima obuhvaća nekoliko različitih vrsta Izrada slučajevi. To područje razini usluge izbjeglo nisu samo uzrok neuspjeha cijelu aplikaciju. Loša dizajna ili administraciju pogrešaka također može dovesti do kvarove. Važno je da razmislite o mogući uzroci pogreške tijekom dizajna i testiranja faze tarifa za oporavak. Dobar plan uzima Azure značajke i augments ih sa specifičnim aplikacijama strategije. Odabrani odgovor je diktirali prema važnosti aplikacije, točke cilj oporavak (RPO) i oporavak vrijeme cilj (RTO).

###<a name="application-failure"></a>Pogreška programa

Azure promet Upravitelj automatski rukuje pogrešaka koje su rezultat podlozi softver hardver i operacijski sustav na virtualnog računala glavnog računala. Azure stvara novu instancu uloge na poslužitelju koji funkcionira i dodaje raspoređivača opterećenja zakretanje. Ako je broj instanci uloga veća od one Azure pomiče obradu da biste druge izvodi uloga instance prilikom zamjene čvor nije uspjelo.

Postoje pogreške ozbiljne aplikacije koji se pokreću neovisno o sve pogreške na hardver i operacijski sustav. Program možda neće uspjeti zbog iznimke do teškog oštećenja uzrokuju neispravni logike ili problema s integritetom podataka. Morate ugraditi dovoljno telemetriju u kod tako da nadzor sustava mogu otkriti uvjeta nije uspjelo i obavijesti administrator aplikacije. Administrator koji imaju potpunu znanja oporavak procesa Izrada možete donošenje odluka pozvati prebacivanje procesa. Osim toga, administrator jednostavno možete prihvatiti do prekida dostupnost da biste riješili kritične pogreške.

###<a name="data-corruption"></a>Oštećenja podataka

Azure automatski sprema podatke baze podataka SQL Azure i pohranu Azure triput redundantly unutar različitih kvara domena u istom području. Ako koristite zemlj ponavljanja, na podaci se pohranjuju dodatne triput u nekoj drugoj regiji. Međutim, ako vaših korisnika i aplikacije oštećuje tih podataka u primarni Kopiraj, podatke brzo replicira na druge kopije. Nažalost, rezultat u tri kopije oštećena podataka.

Da biste upravljali potencijalne oštećenje podataka, imate dvije mogućnosti. Najprije možete upravljati prilagođene strategije sigurnosne kopije. Sigurnosne kopije možete spremiti u Azure ili na lokalnim poslužiteljima, ovisno o poslovnim zahtjevima ili upravljanja pravilnicima. Druga mogućnost je koristiti nova mogućnost vraćanja točke u vrijeme za oporavak SQL baze podataka. Dodatne informacije potražite u odjeljku [Strategije podatke za oporavak Izrada](#data-strategies-for-disaster-recovery).

###<a name="network-outage"></a>Prekida mreže

Kada se ne može pristupiti dijelovima Azure mreže, možda nećete moći pristupiti aplikacije ili podatke. Ako je jedan ili više instanci uloge nisu dostupne zbog problema s mrežom, Azure koristi preostale dostupna instanci aplikacije. Ako aplikacija ne možete pristupiti njegove podatke zbog prekida programa Azure mreže, možete potencijalno pokrenete u načinu rada za su smanjene lokalno pomoću predmemoriranih podataka. Morate mijenjanje arhitekture strategija oporavak Izrada za u su smanjene načinu rada u aplikaciji. Za neke aplikacije možda nije praktično.

Da biste pohranili podatke na drugo mjesto dok vratit će se povezivanje je i mogućnost. Ako su smanjene način nije dostupna, preostale mogućnosti su nedostupnost aplikacije ili prebacivanje zamjenski regiju. Dizajn aplikacije u načinu rada za su smanjene je suvišni poslovnih odluka kao tehničke jedan. To je riječ o Dodatno u odjeljku [smanjena funkcionalnost aplikacije](#degraded-application-functionality).

###<a name="failure-of-a-dependent-service"></a>Pogreška o njima ovisne usluge

Azure nudi mnoge servise kojih može doći periodičku isključiti. Razmislite o [Azure Redis predmemorije](https://azure.microsoft.com/services/cache/) kao primjer. Taj servis više klijentu pruža predmemoriranja mogućnosti u aplikaciji. Važno je da razmotrite što se događa u aplikaciji zavisne servis nije dostupan. Na razne načine scenarij je slična scenarij nedostupnosti mreže. Međutim, preporučuje se svaki servis neovisno rezultira potencijalne poboljšanja općim planom.

Azure Redis predmemorije nudi predmemorije aplikacije iz unutar oblaka servis za implementaciju sustava, koji pruža Izrada pogodnosti oporavak. Najprije servis sada pokreće uloge koje su lokalni za implementaciju sustava. Stoga uspijevate bolje nadzor i upravljanje statusom predmemorije kao dio ukupnog postupke upravljanja za servis u oblaku. Ta vrsta predmemoriranje izlaže i nove značajke. Jedna od novih značajki je visoke dostupnosti za predmemoriranih podataka. Pomaže da biste sačuvali predmemoriranim podacima ne uspijete jedan čvor čuvanjem duplicirane kopije na ostale čvorove.

Imajte na umu da visoke dostupnosti smanjuje propusnost i Latencija povećava zbog ažuriranje sekundarne Kopiraj na zapisivanje. Također doubles količinu memorije koja se koristi za svaku stavku, pa plan za to. U ovom se primjeru određene pokazuje svaki zavisne servis možda imaju mogućnosti koje poboljšanju cjelokupan dostupnosti i mogućnost katastrofalnih.

Uz svaki servis zavisne upoznati posljedice servisa prekidu. U primjeru predmemoriranja, možda je moguće pristupiti podatke izravno iz baze podataka dok ne vrati predmemoriju. To bi su smanjene način pomoću performanse, ali bi im punu funkcionalnost telefona pogledu podataka.

###<a name="region-wide-service-disruption"></a>Područje razini usluge prekidu

Prethodni neuspjeha prvenstveno je pogreške koje je moguće upravljati unutar iste Azure područja. Međutim, morate i pripremiti za mogućnost ima li servis prekidu cijelog područja. Ako se pojavi regija razini usluge prekidu, lokalno suvišnih kopije vaših podataka nisu dostupne. Ako ste omogućili zemlj replikacije, postoje tri primjerka blob-ova i tablica u nekoj drugoj regiji. Ako Microsoft deklarira regija izgubiti, Azure remaps sve DNS unose s područjem zemlj replicirati.

>[AZURE.NOTE] Imajte na umu da nemate sve kontrole nad taj postupak, a zatim će se izvršiti samo za prekidu regija razini usluge. Zbog toga, morate je za druge strategije specifičnim aplikacijama sigurnosnu kopiju da biste postigli najvišu razinu dostupnost. Dodatne informacije potražite u odjeljku [Strategije podatke za oporavak Izrada](#data-strategies-for-disaster-recovery).

###<a name="azure-wide-service-disruption"></a>Servis za Azure razini prekidu

Izrada planiranja, morate uzeti u obzir cijeli raspon moguće disasters. Jedno od najviše loši to izbjeglo za servis želite istodobno obuhvaćaju sva područja koja Azure. Kao i s drugih servisa to izbjeglo možda odlučite da rizika privremeno isključiti ćete poduzeti da događaja. To izbjeglo Primamo servis koji obuhvaćaju područja mora biti puno rijetkim od to izbjeglo Izolirani servis koji obuhvaćaju ovisne servise i jedan područja.

No za neke ključne zaštita njihove privatnosti ovise aplikacije možda odlučite da moraju biti sigurnosne kopije plan za taj scenarij. Plan za ovaj događaj mogu sadržavati ne uspijeva putem servisa u [oblak zamjenski](#alternative-cloud) ili [lokalnog hibridnog i rješenje u oblaku](#hybrid-on-premises-and-cloud-solution).

###<a name="degraded-application-functionality"></a>Funkcija su smanjene aplikacije

Dobro osmišljena aplikacija obično koristi zbirka moduli koji međusobno komunicirati kroz implementacije uzoraka slobodnije povezano-razmjene podataka. DR prilagođene aplikacije potreban je odvojenosti zadataka na razini modul. To je da biste spriječili da dolje cijelu aplikaciju prekidu zavisne servisa. Na primjer, razmotrite web-aplikacija trgovina za Y Ako tvrtka. Sljedeće moduli možda čine aplikacija:

 * __Katalog__ korisnicima omogućuje pregled proizvoda.
 * __Košaricu__ korisnicima omogućuje dodavanje i uklanjanje proizvode iz svoje košaricu.
 * __Status naloga__ prikazuje status dostave narudžbe korisnika.
 * __Slanje narudžbi__ finalizes kupovinu sesiju slanjem redoslijed s otplatom.
 * __Obrada naloga__ Provjeri valjanost redoslijed cjelovitosti podataka i izvodi provjeru dostupnosti količina.

Kada funkcionira zavisne modula u ovoj aplikaciji, kako ne modul funkcionirati dok se ne oporavlja taj dio? Dobro architected sustava implementira odvajanja granice kroz odvojenosti zadataka u trenutku dizajniranja i tijekom rada. Možete kategorizirati svaku pogrešku kao oporaviti, a koje nisu oporaviti. Osobe koje nisu oporaviti pogreške se prema dolje u modulu, ali možete prevladavanje popravljiva pogreška kroz alternative. Kako je opisano u odjeljku visoku dostupnost, možete sakriti nekih problema s korisnicima rukovanja pogreškama i poduzimanja akcija zamjenski. Tijekom više ozbiljne prekidu servisa, aplikacija možda neće biti u potpunosti dostupan. Međutim, treća je mogućnost da biste nastavili održavanje korisnika u su smanjene načinu.

Ako, primjerice, funkcionira baze podataka za hostiranje narudžbe, modul Obrada naloga gubi sposobnost za obradu prodajne transakcije. Ovisno o arhitektura, možda je teško ili nemoguće za slanje narudžbi i Obrada naloga dio aplikacije da biste nastavili. Ako aplikacija nije namijenjena rukovati ovom scenariju, cijelu aplikaciju mogli raditi izvanmrežno.

Međutim, u ovom scenariju isti moguće je koji su podaci o proizvodu spremljeni na nekom drugom mjestu. U tom slučaju modul kataloga proizvoda i dalje može se koristiti za prikaz proizvoda. U načinu za su smanjene aplikacije i dalje biti dostupne korisnicima za dostupne funkcije kao što je prikaz kataloga proizvoda. Ostali dijelovi aplikacije, međutim, nisu dostupne, kao što je redoslijed ili zaliha upita.

Drugi varijacija su smanjene način centrima na performanse umjesto mogućnosti. Na primjer, razmotrite scenarij u kojem je putem predmemorije Redis Azure predmemoriranje kataloga proizvoda. Ako predmemoriranje postane dostupan, aplikacija možda Idite izravno u spremište poslužitelja za dohvaćanje podataka iz kataloga proizvoda. No taj pristup može biti manja od predmemorirane verzije. Zbog toga performanse aplikacije smanjena je funkcionalnost dok servis predmemoriranja potpuno vratiti.

Odlučujete koliko aplikacije će i dalje je funkcija u načinu rada za su smanjene poslovna odluka i tehničke odluka. Aplikacija mora odlučiti kako obavijestiti korisnike privremeni problemi. U ovom primjeru aplikacije mogu omogućiti prikaz proizvodi i čak i njihovo dodavanje u košaricu. Međutim, kada se korisnik pokuša izvršiti, aplikacija obavještava korisnika da je prodaje modul privremeno nije moguće pristupiti. Nije prikladno za klijenta, ali ga spriječiti programa prekidu cijelu aplikaciju servisa.

##<a name="data-strategies-for-disaster-recovery"></a>Strategije podatke za oporavak Izrada

Obrada podataka ispravno je Najteži području da biste dobili desno u bilo kojem Izrada tarifi za oporavak. Vraćanje podataka je dio postupka oporavka koji traje najviše vremena. Različite odabire u načinima smanjene performanse rezultirati teško izazove za oporavak podataka iz pogreške i dosljednost nakon nije uspjelo.

Neki od na čimbenika je potrebno da biste vratili ili održavati kopiju podataka aplikacije. Ti podaci će se koristiti za pregled i transakcijskih svrhe pri sekundarnog web-mjesta. Lokalnog postavljanje zahtijeva skupi i dugotrajan planiranja proces implementaciju Strategije za oporavak Izrada više područja. Većina davatelja usluga oblaka, uključujući Azure, jednostavnijeg lako omogućuju implementaciju aplikacije na više područja. Ove područja geografski distribuira se na način da je servis više područja prekidu treba iznimno rijetko. Strategije za rukovanje podataka preko područja jedan je od prateći čimbenika za uspjeh neku tarifu Izrada oporavak.

U sljedećim se odjeljcima navode Izrada oporavak tehnike vezane uz sigurnosne kopije podataka, referentnih podataka i transakcijskih podataka.

###<a name="backup-and-restore"></a>Sigurnosno kopiranje i vraćanje

Redovito sigurnosno kopiranje podataka aplikacije možete podržava neke scenarije oporavak Izrada. Resursi za pohranu za različite zahtijevaju različite tehnike.

Za razine Basic, standardna i Premium SQL baze podataka možete iskoristiti prednost Vrati točke u vrijeme da biste vratili bazu podataka. Dodatne informacije potražite u članku [Pregled: u Oblaku tvrtke continuity i baza podataka Izrada oporavak s bazom podataka SQL](../sql-database/sql-database-business-continuity.md). Druga mogućnost je koristiti Active replikacije zemlj za SQL baze podataka. To automatski replicira promjene baze podataka na sekundarnom baze podataka u istom Azure području ili čak i u nekoj drugoj regiji Azure. To omogućuje potencijalne zamjena za neke od više ručno tehnika sinkronizacije podataka navedene u ovom članku. Dodatne informacije potražite u članku [Pregled: SQL replikacije baze podataka Active zemlj.-](../sql-database/sql-database-geo-replication-overview.md).

Korištenje više ručno pristup za sigurnosno kopiranje i vraćanje. Koristite naredbu Kopiraj bazu podataka da biste stvorili kopiju baze podataka. Koristite sljedeću naredbu da biste dobili sigurnosnu kopiju s transakcijskih dosljednost. Možete koristiti i servis za uvoz/izvoz baze podataka SQL Azure. To podržava izvoz baza podataka BACPAC datoteke koje se pohranjuju u spremište blobova platforme Azure.

Ugrađene zalihosti prostora za pohranu Azure stvara dvije replike datoteku sigurnosne kopije u istom području. Međutim, Učestalost pokretanja postupak sigurnosnog kopiranja određuje RPO, koji je količinu podataka koji se može izgubiti u Izrada scenarijima. Ako, na primjer, zamislite da načinite sigurnosnu kopiju pri vrhu sat, a pojavljuje se na Izrada dvije minute prije vrh sat. Izgubit 58 minuta podataka koji se dogodilo nakon zadnje sigurnosne kopije izvršena. Osim toga, da biste zaštitili regija razini usluge prekidu, kopirajte datoteke BACPAC zamjenski regiju. Imat ćete mogućnost vraćanja te kopije u području zamjenski. Dodatne informacije potražite u članku [Pregled: u Oblaku tvrtke continuity i baza podataka Izrada oporavak s bazom podataka SQL](../sql-database/sql-database-business-continuity.md).

Za pohranu Azure razvijate vlastite prilagođene postupak sigurnosnog kopiranja ili primijenite jednu od mnogo proizvođača alatima za sigurnosno kopiranje. Imajte na umu da većina aplikacije dizajna imaju dodatne complexities gdje resursa za pohranu upućuju jedna na drugu. Na primjer, razmotrite SQL bazu podataka koja sadrži stupac koja se povezuje s blob u Azure prostora za pohranu. Ako sigurnosnih kopija istodobno slučajno, baze podataka može imati pokazivač da biste blob koji je sigurnosne kopije prije pogreške. Aplikaciju ili Izrada oporavak plan morate provesti postupke za rukovanje nedosljednosti nakon na oporavak.

###<a name="reference-data-pattern-for-disaster-recovery"></a>Uzorak podataka referenca za oporavak Izrada

Referenca podaci samo za čitanje podataka koje podržava funkciju aplikacije. Obično ne mijenja često. Iako se sigurnosno kopiranje i vraćanje je način rukovanja to izbjeglo regija razini usluge, u RTO je relativno dugačkih. Ako pokrenete aplikaciju na sekundarnom područje, neke strategije možete poboljšati RTO za referentnih podataka.

Budući da upućuju na promjene podataka diskovni, možete poboljšati u RTO održavanje trajnu kopiju referentnih podataka u području sekundarne. Time se uklanja vremena potrebnog za sigurnosno kopiranje vraćanju na Izrada. Da biste zadovoljava preduvjete za oporavak Izrada više područja, morate implementirati aplikaciju i podatke referenca zajedno u više područja. Kao što je rečeno [Uzorak podataka referenca za visoke dostupnosti](./resiliency-high-availability-azure-applications.md#reference-data-pattern-for-high-availability), možete implementirati referentnih podataka ulozi sam vanjskog prostora za pohranu ili kombinaciju tih dvaju načina.

Referenca podatkovnog modela implementacije unutar računalnim čvorove implicitno zadovoljava preduvjete za oporavak Izrada. Pregled podataka implementaciju s bazom podataka SQL potreban je implementacija kopiju referentnih podataka za svako područje. Isti strategije primjenjuje se na Azure prostora za pohranu. Morate uvesti kopiju referentnih podataka koji je spremljen u Azure prostora za pohranu na primarnih i sekundarnih područja.

![Pregled podataka publikacije primarnih i sekundarnih područja](./media/resiliency-disaster-recovery-azure-applications/reference-data-publication-to-both-primary-and-secondary-regions.png)

Morate provesti vlastite specifičnim aplikacijama sigurnosne kopije postupke za sve podatke, uključujući referentnih podataka. Zemlj replicirati kopije preko područja koriste se samo u regiji razini usluge prekidu. Da biste spriječili prošireni nedostupnost implementirati zaštita njihove privatnosti ovise ključnih dijelove podataka aplikacije sekundarne regiju. Primjer u ovom topologije potražite u članku [Aktivno pasivni modela](#active-passive).

###<a name="transactional-data-pattern-for-disaster-recovery"></a>Uzorak transakcijskih podataka za oporavak Izrada

Implementacija strategije potpuno funkcionalni Izrada način zahtijeva asinkronog replikacije transakcijskih podataka s područjem sekundarne. Windows praktično vrijeme u kojem se može pojaviti na replikacije će odrediti karakteristike RPO aplikacije. I dalje mogu vratiti podatke koje je prekinuta iz primarnog regije tijekom prozoru replikacije. Možda se moći spojiti s sekundarne regija kasnije.

Sljedeći primjeri arhitektura nude nekoliko savjeta na različite načine obrade transakcijskih podataka u scenariju prebacivanje. Važno Imajte na umu da u ovim se primjerima nije iscrpan je. Ako, na primjer, Srednja mjesta za pohranu kao što su redova možda zamjenjuje s bazom podataka SQL Azure. Redovi same možda Azure prostora za pohranu ili Bus servisa Azure redova (pogledajte [Azure redova "i" Redovi servisa Bus – usporedbi i iz Outlooka nasuprot](../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md)). Poslužitelj za pohranu odredišta i ovise, kao što su Azure tablice umjesto SQL baze podataka. Osim toga, uloge suradnika možda umeću kao intermediaries u razne korake. Važno je nije točno emuliraj te arhitekturi, ali treba uzeti u obzir različite alternative u oporavak transakcijskih podataka i povezane module.

####<a name="replication-of-transactional-data-in-preparation-for-disaster-recovery"></a>Replikacija transakcijskih podataka u Priprema za oporavak Izrada

Razmislite o aplikacije koja koristi pohranu Azure redova veličini transakcijskih podataka. Time se omogućuje uloge suradnika obrade transakcijskih podataka u bazu podataka poslužitelja u samostalne arhitekturi. Potreban je transakcija da biste koristili neki oblik privremena predmemoriranja sučelja uloge moraju odmah upit tih podataka. Ovisno o razini odstupanje gubitka podataka, možete odabrati replicirati redova, bazu podataka ili sve od resursa za pohranu. S samo replikacije baze podataka, ako je primarni regija funkcionira, ipak možete oporaviti podatke u redovima kada primarni regija Vrati.

Sljedeći dijagram prikazuje se arhitektura gdje se sinkronizirati bazu podataka poslužitelja preko područja.

![Replikacija transakcijskih podataka u Priprema za oporavak Izrada](./media/resiliency-disaster-recovery-azure-applications/replicate-transactional-data-in-preparation-for-disaster-recovery.png)

Najvećih složeno implementacijom tu arhitekturu je strategije replikacije među područjima. Servis za sinkronizaciju podataka za SQL Azure omogućuje ovu vrstu replikacije. Međutim, servis je i dalje u pretpregledu i ne preporučuje se za radnog okruženja. Dodatne informacije potražite u članku [Pregled: u Oblaku tvrtke continuity i baza podataka Izrada oporavak s bazom podataka SQL](../sql-database/sql-database-business-continuity.md). Za aplikacije radnog ulažete u rješenja drugih proizvođača ili stvorite vlastitu logiku replikacije u kodu. Ovisno o arhitektura, s replikacijom možda Dvosmjeran, koji je složenije.

Jedan bi se mogla potencijalne implementaciji koristite Srednja reda čekanja u prethodnom primjeru. Uloga suradnika koji obrađuje podatke na odredište konačni prostora za pohranu možda unesite željene promjene u području primarni i sekundarni regija. Nisu trivial zadaci, a dovršena smjernice za replikaciju kod izlazi iz ovog članka. Važno točku koja je mnogo vremena i testiranje treba usredotočite se na kako podataka replikaciju sekundarnu područje. Dodatnu obradu i testiranje možete osigurati da procesa prebacivanje i oporavak pravilno rukovati nedosljednosti mogućih podataka ni duplicirane transakcije.

>[AZURE.NOTE] Većina ovog članka usredotočuje se na platformi kao service (PaaS). Međutim, dodatne mogućnosti replikacije i dostupnost za hibridno aplikacije koristiti virtualnim računalima sustava Azure. Ove aplikacije za hibridno upotrijebili infrastrukture usluge (IaaS) za hostiranje sustava SQL Server na virtualnim strojevima u Azure. Time se omogućuje tradicionalni dostupnost pristupa u sustavu SQL Server, kao što su grupe dostupnosti AlwaysOn ili isporuka zapisnika. Neke tehnike, kao što su AlwaysOn, funkcioniraju samo između lokalnog sustava SQL Server instancama i Azure virtualnih računala. Dodatne informacije potražite u članku [visoke dostupnosti i oporavak Izrada za SQL Server na virtualnim strojevima sa sustavom Azure](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).

####<a name="degraded-application-mode-for-transaction-capture"></a>Su smanjene Aplikacijski način rada za prikupljanje transakcije

Razmislite o drugi arhitektura koja radi u načinu rada za su smanjene. Aplikaciju na sekundarnom regija Deaktiviraj funkcionalnost, kao što su izvješća, poslovno obavještavanje (BI) ili kabela redova. Kako je definirano poslovanja, prihvaća samo najvažnije vrste transakcijskih tijekova rada. Sustav spremit će transakcije i zapisuje redova. Sustav može odgoditi obradu podataka tijekom početne fazu prekidu servisa. Ako je sustav na područje primarni je ponovno aktivirati unutar prozora očekivano vrijeme, uloge suradnika u području primarni može potrošiti redova. Ovaj postupak nema potrebe za spajanje baze podataka. Ako servis prekidu primarni regija prelazi tolerable prozora, aplikacija možete pokrenuti obradu redova.

U ovom scenariju baza podataka na sekundarnom sadrži rastuće transakcijskih podatke koji se moraju uvrstiti nakon primarni je aktivirati. Sljedeći dijagram prikazuje ova Strategije za privremeno spremanje transakcijskih podataka dok vratit će se primarni regija.

![Su smanjene Aplikacijski način rada za prikupljanje transakcije](./media/resiliency-disaster-recovery-azure-applications/degraded-application-mode-for-transaction-capture.png)

Dodatne rasprave tehnika upravljanja podacima za prebacuju Azure aplikacije potražite u članku [Failsafe: smjernicama prebacuju oblaka arhitekturi](https://channel9.msdn.com/Series/FailSafe).

##<a name="deployment-topologies-for-disaster-recovery"></a>Topologija implementacije za oporavak Izrada

Morate pripremiti zaštita njihove privatnosti ovise ključnih aplikacije za mogućnost nastanka regija razini usluge prekidu. To možete učiniti tako ugradnje strategije više područja implementacije u radu sa servisom planiranja.

Više područja implementacijama možda obuhvaćaju IT pro procesa da biste objavili podataka aplikacije i reference s područjem sekundarne nakon na Izrada. Ako aplikacija zahtijeva izravne prebacivanje, postupak implementacije možda obuhvaćaju je aktivno/pasivni postavljanje ili postavljanje programa aktivno/aktivno. Ovu vrstu implementacije ima pojavljivanja te aplikacije koje se izvode u području zamjenski. Alat za usmjeravanje kao što je Azure promet Manager nudi servisi za ujednačavanje opterećenja na razini DNS-a. Može prepoznati to izbjeglo servisa i usmjeravanje korisnika za različite regije po potrebi.

Dio uspješno Azure Izrada oporavak je architecting te oporavak u rješenje od početka. Oblak pruža dodatne mogućnosti za oporavak u pogrešaka tijekom na Izrada koje nisu dostupne u tradicionalni davatelja usluge hostiranja. Konkretno, možete dinamički i brzo dodjelu resursa na drugo područje. Zbog toga neće plaćate mnogo neaktivnosti resurse dok čekate za neuspjeh da se pokreće.

U sljedećim se odjeljcima pokrivaju različite implementacije topologija za oporavak Izrada. Obično je na tradeoff povećana trošak i složenosti dodatne dostupnost.

###<a name="single-region-deployment"></a>Uvođenje jednog područja

Implementacija jednog područja zaista nije topologije za oporavak Izrada, ali je namijenjena kontrasta s drugim arhitekturi. Jednom regija implementacijama su uobičajeni za aplikacije u Azure. No tu vrstu implementacije nije ozbiljne contender za plan za oporavak Izrada.

Sljedeći dijagram prikazuje aplikacije koje se izvode u jednom području Azure. Azure Upravitelj promet i korištenje domene kvara i nadogradnja povećanje dostupnosti aplikacije unutar područja.

![Uvođenje jednog područja](./media/resiliency-disaster-recovery-azure-applications/single-region-deployment.png)

Ovdje je vidljiva je baza podataka u jednoj točki kvara. Iako Azure replicira podatke preko različitih kvara domene za interne replike, ova sve pojavljuje se na istom području. Aplikacija ne withstand Katastrofalna pogreška. Ako područje funkcionira, sve domene kvara otvorite dolje – uključujući sve instance servisa i resursa za pohranu.

Za sve osim najmanje ključnih aplikacije, morate razviju plan za implementaciju aplikacije preko više područja. Trebali biste razmislite o RTO i cijena ograničenja u odlučuje koje topologije implementaciju da biste koristili.

Pogledajmo je sada određenim uzorcima za podršku prebacivanje preko različitih područja. U ovim se primjerima svih pomoću dva područja opisuje postupak.

###<a name="redeployment-to-a-secondary-azure-region"></a>Ponovno uvođenje sekundarne Azure područje

Uzorak ponovno uvođenje sekundarne regiju, primarni regija ima aplikacija i baza podataka radi. Sekundarni regija nije postavljen za automatsko prebacivanje. Pa kad se pojavi na Izrada, morate Okretni se svi dijelovi servisa u novo područje. To obuhvaća prijenos servis u oblaku za Azure i Implementacija servisa u oblaku, vraćanja podataka i promjena DNS-a za preusmjeravanje promet.

Iako to je najčešće jeftin mogućnosti više područja, ima najgorim RTO karakteristike. U ovom modelu sigurnosne kopije paketa i baze podataka usluge pohranjuju bilo lokalno ili na instancu spremište blobova platforme Azure područja za sekundarne. Međutim, morate implementacije novog servisa i vratiti podatke prije nego što je nastavlja postupak. Čak i ako se potpuno automatizirati prijenos podataka iz sigurnosne kopije za pohranu, adrese e nove baze podataka okruženja troši puno vremena. Premještanje podataka iz spremišta diska sigurnosne kopije praznu bazu podataka na sekundarnom regija je najčešće skupi dio postupak vraćanja. To morate učiniti, no da biste otvorili novu bazu podataka za radno stanje jer ona nije replicirati.

Najbolje je koristiti za pohranu servisa paketa u spremište blobova platforme u području sekundarne. To nema potrebe da biste prenijeli paket Azure koji će se dogoditi ako pokrenete s računala za razvoj programa lokalnog. Možete brzo implementirati pakete usluga u novi oblaku iz spremišta blobova putem skripti komponente PowerShell.

Ova je mogućnost praktična samo za rješavaju aplikacije koje možete tolerate visoke RTO. Ako, primjerice, to može funkcionirati za aplikaciju koja može biti prema dolje za nekoliko sati, ali moraju imati ponovno u roku od 24 sata.

![Ponovno uvođenje sekundarne Azure područje](./media/resiliency-disaster-recovery-azure-applications/redeploy-to-a-secondary-azure-region.png)

###<a name="active-passive"></a>Aktivni pasivni

Uzorak aktivno pasivni je odabir koji odabrati mnoge tvrtke. Ovaj uzorak daje poboljšanja RTO s razmjerno malo povećati trošak putem uzorak ponovno uvođenje.
U ovom scenariju postoji ponovno primarne i sekundarne Azure regija. Sve promet odlazak aktivnog uvođenja na primarni regija. Sekundarni regija bolje pripremiti za oporavak Izrada jer bazu podataka koja se izvodi na obje regije. Osim toga, mehanizam za sinkronizaciju je na mjestu između njih. Takvog čekanja može obuhvaćati dva varijacije: pristup koja je samo za bazu podataka ili dovršeno implementacije u području sekundarne.

####<a name="database-only"></a>Samo na baze podataka

U prvi varijacija uzorak aktivno pasivni primarni regija ima servisne aplikacije za distribuiranih oblaka. Međutim, za razliku od uzorak ponovno uvođenje obje regije sinkroniziraju sa sadržajem baze podataka. (Dodatne informacije potražite u odjeljku [Uzorak transakcijskih podataka za oporavak Izrada](#transactional-data-pattern-for-disaster-recovery).) Kada se pojavi na Izrada, postoje manje preduvjeti za aktivaciju. Pokretanje aplikacije u području sekundarne, promijenite nizove veze u novu bazu podataka i promjenu stavke DNS-a za preusmjeravanje prometa.

Kao što je uzorak ponovno uvođenje mora već spremljenim pakete usluga u spremište blobova platforme Azure u području sekundarne brže implementacije. Za razliku od uzorak ponovno uvođenje ne plaćati Većina indirektnog koji zahtijeva postupak vraćanja baze podataka. Je baza podataka spremna i pokrenut. Time veliku količinu vremena, čime se jeftin uzorak DR. Preporučuje se i najpopularnijih uzorak DR.

![Aktivni pasivni samo baze podataka](./media/resiliency-disaster-recovery-azure-applications/active-passive-database-only.png)

####<a name="full-replica"></a>Pune replike

U drugi varijacija uzorak aktivno pasivni primarni regija i sekundarne regija imaju puni implementacije. U ovom implementacije obuhvaća servisa u oblaku i sinkronizirani baze podataka. Međutim, samo primarni regija je aktivno obrade zahtjeva za mrežni za korisnike. Sekundarni regija postaje aktivan samo primarni regija sučelja servisa prekidu. U tom slučaju svih novih zahtjeva za mrežni usmjeravanje sekundarne regiju. Azure promet Upravitelj automatski možete upravljati ovaj prebacivanje.

Prebacivanje pojavljuje se brže nego varijacije samo za baze podataka jer su usluge već ste implementirali. Ovaj uzorak nudi nisku RTO. Područje sekundarne prebacivanje mora biti spremna za slanje odmah nakon neuspjeh primarni područja.

Uz brže vrijeme odaziva ovaj uzorak ima prednost unaprijed dodjeljivanje i Implementacija servisa sigurnosne kopije. Ne morate brinuti o područja nemate prostora za dodjelu nove instance na Izrada. To je Važno Ako vašoj regiji sekundarne Azure je nearing kapaciteta. Nema jamstva (ugovor o razini usluge) koje trenutačno moći uvesti brojne nove servise u oblaku u bilo kojem regiji.

Najbrži odgovor put ovaj model, morate imati slično mjerilo (broj instanci ulogu) u regijama primarnih i sekundarnih. Bez obzira na prednosti plaćanja Neiskorišteni računalnim instanci je skup, a to može biti najviše dobro koristiti financijske odabir. Zbog toga je više zajednički koriste verziju malo skalirana prema dolje servise u oblaku na sekundarnom regija. Zatim možete brzo neće uspjeti iznad i skaliranje out sekundarne implementacije ako je potrebno. Trebali biste automatizirali proces prebacivanje tako da nakon primarni regija nije moguće pristupiti, aktivirati dodatne instance, ovisno o opterećenje. To može uključivati upotrebu mehanizam za autoscaling kao što su [postavlja skaliranje virtualnog računala](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).

Sljedeći dijagram prikazuje model kojima područja primarnih i sekundarnih sadrže potpuno distribuiranih oblaku u je aktivno pasivni uzorka.

![Aktivni pasivni, potpuno replike](./media/resiliency-disaster-recovery-azure-applications/active-passive-full-replica.png)

###<a name="active-active"></a>Aktivni aktivno

Po pak koju ste vjerojatno odrediti proizvod uzorke: smanjivanje na RTO povećava troškove i složenosti. Rješenje aktivno aktivno zapravo prijelomi ovaj srednju vrijednost s regard da biste trošak.

U uzorak je aktivno aktivno servise u oblaku i baza podataka se potpuno implementiran u obje regije. Za razliku od modela aktivno pasivni obje regije primiti promet korisnika. Ta mogućnost rezultira najbrže oporavku. Usluge su već skalirana učiniti dio opterećenje na svaku regiju. DNS je već omogućeno korištenje sekundarnog regija. Postoji dodatni složenosti prilikom određivanja kako usmjeravanje korisnika na odgovarajuće područje. Planiranje kružnog možda. Veća je vjerojatnost određenim korisnicima koristila određenih područja gdje se nalazi primarni kopiju svoje podatke.

U slučaju prebacivanje, jednostavno onemogućiti DNS primarni regiju. Usmjerava se sve promet na sekundarnom regiju.

Čak i u model, postoje neka varijacije. Na primjer, sljedeći dijagram prikazuje model gdje područje primarni je vlasnik osnovne kopiju baze podataka. Servisa u oblaku u obje regije pisati primarni baze podataka. Sekundarni implementacije može čitati iz primarnog ili repliciranu bazu podataka. Replikacija u ovom primjeru događa jedan od načina.

![Aktivni aktivno](./media/resiliency-disaster-recovery-azure-applications/active-active.png)

Postoji negativna strana za arhitektura aktivno aktivan u prethodnom dijagrama. Drugu regiju morate pristupiti baze podataka u prvom području jer glavna kopija tamo nalazi. Performanse znatno izostavlja isključiti kada pristupate podatke iz izvan područja. U poziva regije-baze podataka, razmotrite neke vrste grupnog slanja promjena Strategije za poboljšanje performansi te pozive. Dodatne informacije potražite u članku [upute za korištenje grupnog slanja promjena da biste poboljšali performanse računala SQL baze podataka](../sql-database/sql-database-use-batching-to-improve-performance.md).

Zamjenski arhitektura može obuhvaćati svaku regiju izravno pristupiti vlastitu bazu podataka. U tom modelu neke vrste Dvosmjeran replikacije potreban je da biste sinkronizirali baze podataka u svakom području.

U uzorak aktivno Aktivno, nije vam potreban proizvoljan broj instanci primarni regiju kao i u aktivni pasivni uzorka. Ako imate 10 instance na područje primarni je aktivno pasivni arhitektura, bilo bi dobro 5 na svakom području programa arhitekture aktivno aktivno. Obje regije sada zajednički koristiti mogućnost Učitaj. Ako se na Toplo čekanja na područje pasivni 10 instance čekanja za prebacivanje to može biti ušteda troškova iznad aktivne pasivni uzorak.

Shvatili dok se ne vratite primarni regija, sekundarne regija mogli biste primiti iznenadno stabilizatorom novog korisnika. Ako postoje 10 000 korisnika na svakom poslužitelju kada primarni regija sučelja servisa prekidu, sekundarne regija iznenada mora učiniti 20 000 korisnika. Nadzor pravila na sekundarnom regija morate otkriti ovaj povećanje i dvostruki instance u području sekundarne. Dodatne informacije o tome potražite u odjeljku [otkrivanje pogreške](#failure-detection).

##<a name="hybrid-on-premises-and-cloud-solution"></a>Lokalnog hibridnog i rješenje u oblaku

Jedan dodatni Strategije za oporavak Izrada je mijenjanje arhitekture hibridnog aplikacije koji se izvodi lokalno i u oblaku. Ovisno o aplikaciji primarni regija možda lokacije. Razmislite o prethodne arhitekturi i zamislite regija primarni ili sekundarni kao lokalne lokacije.

Postoje neka izazove u te hibridnog arhitekturi. Najprije Većina ovog članka je adresirana PaaS arhitektura uzoraka. Uobičajeni PaaS aplikacije u Azure oslanjate specifične za Azure konstrukta kao što su uloge, servise u oblaku i Upravitelj promet. Da biste stvorili rješenje za lokalni za tu vrstu aplikacije PaaS je potrebna znatno arhitektura. Možda nije izvedivo upravljanje ili trošak perspektive.

No hibridnog rješenje za oporavak Izrada ima manje izazove za tradicionalni arhitekturi koji se jednostavno prijeđete u oblak. To vrijedi od arhitekturi koji koriste IaaS. Aplikacije IaaS koristiti virtualnih računala u oblaku koji mogu sadržavati Izravni lokalnog Ekvivalenti. Povezivanje računala u oblaku s lokalnim mrežni resursi možete koristiti i virtualne mreže. Otvorit će se nekoliko mogućih razloga koji nisu moguće s programima koji su samo za PaaS. Ako je, primjerice, SQL Server možete iskoristiti Izrada oporavak rješenja kao što su grupe dostupnosti AlwaysOn i baze podataka. Detalje potražite u članku [visoke dostupnosti i oporavak Izrada za SQL Server na virtualnim računalima za Azure](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).

Rješenja IaaS nuditi jednostavnije put za korištenje Azure kao mogućnost za prebacivanje aplikacijama lokalnog. Potpuno funkcionalnu aplikacije možda u postojeće područje za lokalni. Međutim, ako nemate odgovarajućih resurse da biste zadržali geografski zasebna područja za prebacivanje? Možda odlučite korištenje virtualnih računala te virtualne mreže vaše aplikacije koje se izvode u Azure. U tom slučaju definirati procesa koji se sinkroniziraju podataka s oblakom. Uvođenje Azure postaje sekundarne područja za prebacivanje. Primarni regija ostaje lokalnog računala. Dodatne informacije o IaaS arhitekturi i mogućnosti potražite u [dokumentaciji virtualnim računalima](https://azure.microsoft.com/documentation/services/virtual-machines/).

##<a name="alternative-cloud"></a>Zamjenski oblaka

Postoje situacije u kojima čak i odličnim Microsoft Cloud ne zadovoljava Interna usklađenost pravila ili pravila koje je potrebno vašoj tvrtki ili ustanovi. Čak i na najbolji Priprema i dizajn možete implementirati sigurnosne kopije sustava tijekom na Izrada nalaziti kratki ako postoji globalni servisa prekidu davatelja usluga za oblaka.

Ćete želite usporediti dostupnost preduvjeti troška i složenost veća dostupnost. Rizik analizu, a zatim definirati RTO i RPO za rješenje. Ako aplikacija nije moguće tolerate sve nedostupnost, možda bi smislu da razmislite o korištenju drugo rješenje oblaka. Osim ako ih ne funkcionira na cijelu Internet, drugo rješenje oblaka možda još uvijek dostupne ako Azure postane globalno može pristupiti.

Kao i kod scenarija hibridnog implementacijama prebacivanje u prethodno arhitekturi oporavak Izrada postoji možete i unutar drugog oblaka rješenja. Zamjenski oblaka DR web-mjesta moraju se koristiti samo za rješenja čije RTO omogućuje vrlo malo, ako nedostupnost. Imajte na umu da rješenja koja ih koriste na web-mjesta izvan Azure zahtijeva više rada za konfiguriranje, razvoj, uvođenje i održavanje. Također je teže implementaciju najbolje prakse u arhitekturu unakrsno oblaka. Iako oblaka platforme imaju slične koncepata više razine, API-ji i arhitekturi se razlikuju.

Ako odlučite da biste podijelili na DR među različite platforme, ona smisla za mijenjanje arhitekture apstrakcije slojeve u dizajnu rješenja. Ako to učinite, nećete morati razviti i održavanje dvije različite verzije iste aplikacije za različite oblaka platforme u slučaju Izrada. Kao s scenarija hibridnog korištenje Azure virtualnih računala ili servisa Azure spremnik može biti jednostavnije u tim slučajevima od korištenja specifične za oblak PaaS dizajna.

##<a name="automation"></a>Automatizacija

Neke od znakova koje ćemo samo spominju potreban je brzi aktivaciju izvanmrežne implementacije i vraćanje određene dijelove sustava. Automatizacija ili skriptiranje podržava mogućnost da biste aktivirali resursa na zahtjev i hitro implementacija rješenja. U ovom se članku vezane uz DR Automatizacija je equated sa [Servisom PowerShell Azure](https://msdn.microsoft.com/library/azure/jj156055.aspx), ali je [Servis za upravljanje REST API -JA](https://msdn.microsoft.com/library/azure/ee460799.aspx) i mogućnost.

Razvoj skripti olakšava upravljanje dijelove DR koji Azure proziran ne rukovati. Ima prednost proizvodnje dosljedan rezultate prilikom svakog koji minimizira izgledi Ljudski pogreške. Imate unaprijed definirane DR skripte smanjuje vrijeme ponovno stvaranje sustava i njezin sastavnoj dio in the midst of na Izrada. Ne želite da biste ručno znate kako vratiti web-mjesta, dok je prema dolje i gubitka novac svake minute.

Kada stvorite skripte, isprobajte ih više puta od početka do kraja. Kada potvrdite svoje osnovne funkcije, provjerite je li testirati ih u [Izrada simulacije](#disaster-simulation). Omogućuje otkrivanje pogreške skripte ili procesa.

Preporučenim načinom rada s Automatizacija je da biste stvorili spremište skripte komponente PowerShell ili sučelje naredbenog retka (EŽA) skripte za oporavak Azure Izrada. Jasno označavanje i kategoriziranje ih jednostavno pretraživanja. Odredite jedna osoba da biste upravljali spremište i upravljanje verzijama skripte. Dokument ih dobro s objašnjenja parametre i primjere za skripte korištenja. Provjeriti da ste sinkroniziranosti ove dokumentacije s vašeg Azure implementacije. To underscores Svrha pojavljuju jedna osoba zadužena svi dijelovi u spremištu.

##<a name="failure-detection"></a>Otkrivanje pogreške

Pravilno rukovati problema s dostupnosti i oporavak Izrada, moraju imati mogućnost za otkrivanje i Dijagnosticiranje pogreške. Trebali biste učiniti napredne poslužitelj i implementaciju nadzor tako da možete brzo znate kada je sustav ili njezin dio iznenada prema dolje. Nadzor alatima koji pogledajte stanje servisa u oblaku i njegov dio ovog rada možete izvršiti. Microsoftova alata za jedan je [2016 centar sustava](https://www.microsoft.com/en-us/server-cloud/products/system-center-2016/). Alati drugih proizvođača također pružaju nadzora mogućnosti. Većina nadzora rješenja dinamično ključnog učinka mjerača dostupnost usluge.

Iako ključni te alate, mogu zamijeniti potrebe za planiranje za otkrivanje kvara i izvješćivanje o pogreškama u neki servis u oblaku. Potrebno je planirati pravilno pomoću dijagnostike Azure. Mjerača prilagođene performansi ili unose u zapisnik događaja mogu biti dio ukupnog strategije. To omogućuje dodatnih podataka tijekom pogrešaka da biste brzo dijagnosticiranje problema i vraćanje cijelog mogućnosti. U njoj se nalaze i dodatni metriku koja alata za nadzor možete koristiti za utvrđivanje stanja računala. Dodatne informacije potražite u članku [Omogućavanje dijagnostike Azure u Azure servise u Oblaku](../cloud-services/cloud-services-dotnet-diagnostics.md). Rasprave kako plan za Ukupno "Stanje model", potražite u članku [Failsafe: smjernicama prebacuju oblaka arhitekturi](https://channel9.msdn.com/Series/FailSafe).

##<a name="disaster-simulation"></a>Izrada simulacije

Testiranje simulacije uključuje stvaranje small stvarnih situacijama floor rad na pridržavajte se kako se Upoznajte članovima tima. Simulations prikazati i u planu oporavak koliko su rješenja. Izvršavanje simulations na način stvoreni scenariji ne poremetiti poslovna dok još uvijek osjećaj kao što je realni situacije.

Razmislite o architecting vrstu "Upravljačka ploča" u aplikaciji za ručno kao zamjenu za problema s dostupnošću. Ako, primjerice, putem parametru meki pokretanje baze podataka programa access iznimke za poručivanja modul tako da ga neispravan rad uzrokuje. Na razini mreže sučelja koje možete poduzeti slične laganih pristupa za druge module.

Simulacije ističe inadequately riješili probleme. Scenariji za Simulirani mora biti potpuno upravljani. To znači da, čak i ako tarifu za oporavak čini se da se ne uspijeva, možete vratiti situaciji vratite se u obični bez izazove sve značajan štetu. Važno je i obavještavanje o prikazale upravljanje o kada i kako se vježbe simulacije će se izvršavati. Ovaj plan potrebno obuhvatiti informacije na vremenu ili resursi koji se može postati unproductive dok se izvodi simulacije test. Kada ste subjecting Izrada oporavak tarifa za testiranje, također važno je da biste odredili koliko će se mjeri uspjeh.

Postoji nekoliko postupaka koje možete koristiti da biste testirali Izrada tarife za oporavak. Međutim, Većina ih je jednostavno promijenjena verzije ovih osnovnih tehnika. Glavni motive iza testiranje se treba vrednovati kako je to moguće, a kako workable plan za oporavak. Izrada oporavak testiranje usredotočuje se na detalje da biste otkrili rupe u planu osnovni oporavak.

##<a name="next-steps"></a>Daljnji koraci

Ovaj je članak dio niza članaka filtriran na [oporavak Izrada i visoke dostupnosti za aplikacije koje se temelji na Microsoft Azure](./resiliency-disaster-recovery-high-availability-azure-applications.md). Prethodni članak u ovoj seriji je [visoke dostupnosti za aplikacije koje se temelji na Microsoft Azure](./resiliency-high-availability-azure-applications.md).
