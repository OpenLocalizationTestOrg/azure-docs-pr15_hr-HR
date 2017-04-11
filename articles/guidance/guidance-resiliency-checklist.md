<properties
   pageTitle="Kontrolni popis za otpornost | Microsoft Azure"
   description="Kontrolnog popisa koji sadrži smjernice za otpornost opasnosti tijekom dizajna."
   services=""
   documentationCenter="na"
   authors="petertaylor9999"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="best-practice"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="petertay"/>

# <a name="azure-resiliency-guidance-resiliency-checklist"></a>Upute za Azure otpornost: kontrolni popis za otpornost

Dizajniranje aplikacije za otpornost zahtijeva planiranje i mitigating brojne načine pogreške koji se može pojaviti. Pregled stavki u odnosu dizajnu aplikacije da bi više prebacuju navedeni popis za provjeru.

## <a name="requirements"></a>Preduvjeti

- **Definirati zahtjeve za dostupnost klijenta.** Klijent sadržavat će dostupnost preduvjeti za komponente u aplikaciji, a to će utjecati na vaše aplikacije dizajna. Dohvaćanje ugovor iz klijenta za dostupnost odredišta svaki dio aplikacije, u suprotnom dizajnu možda ispunila očekivanja klijenta. Dodatne informacije potražite u odjeljku [Definiranje preduvjeti za otpornost](guidance-resiliency-overview.md#defining-your-resiliency-requirements) [dizajniranje prebacuju aplikacije za Azure](guidance-resiliency-overview.md) dokumenta.

## <a name="failure-mode-analysis"></a>Nije uspjelo način analizu

- **Tema neuspjeh način (FMA) za svoju aplikaciju.** FMA je postupak za otpornost u aplikaciju na početku faze dizajna. Ciljevi programa FMA obuhvaćaju sljedeće:  

    - Odredite koje vrste pogrešaka aplikacije se mogu pojaviti. 
    - Snimite potencijalne efekata i utjecaj svaku vrstu pogreške u aplikaciji.
    - Odredite Strategije oporavka. 

    Dodatne informacije potražite u članku [dizajniranje prebacuju aplikacije za Azure: pogreška način analizu][fma].  

## <a name="application"></a>Aplikacija

- **Implementacija više instanci servisa.** Usluge inevitably neće uspjeti, a ako aplikacija ovisi o instancu servisa je inevitably neće i. Dodjela više instanci [aplikacije](../app-service/app-service-value-prop-what-is.md)servisa za Azure, odaberite je [Plan za aplikaciju servisa](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) koji nudi više instanci. Za servise u Oblaku Azure, konfigurirati sve svoje uloge da biste koristili [više instanci](../cloud-services/cloud-services-choose-me.md#scaling-and-management). Za [Azure virtualnim strojevima (VMs)](../virtual-machines/virtual-machines-windows-about.md), provjerite je li da se vaša arhitektura VM sadrži više od jedne VM i svaki VM uvrštava u programa [Postavljanje dostupnosti][availability-sets].   

- **Pomoću raspoređivača opterećenja distribucija zahtjeva.** Raspoređivača opterećenja distribuira vaše aplikacije zahtjevi za instanci dobar servisa tako da uklonite dobro instance rotacije. Ako na servisu koristi aplikacije servisa za Azure ili Azure servise u Oblaku, već je rasporediti umjesto vas opterećenje. Međutim, ako aplikacija koristi Azure VMs, morat ćete Dodjela raspoređivača opterećenja. Potražite u članku pregled [Azure opterećenja](../load-balancer/load-balancer-overview.md) više pojedinosti. 

- **Konfiguriranje Azure pristupnika aplikacije da biste koristili više instanci.** Ovisno o preduvjetima za vaše aplikacije, [Azure aplikacije pristupnika](../application-gateway/application-gateway-introduction.md) možda bolje prikladniji distribucija zahtjeva za usluge vaše aplikacije. Međutim, pojedine instance pristupnika aplikacije servisa ne jamči tako da se SLA tako da bude moguće nije uspjeti aplikacija ne uspijete instanci pristupnika aplikacije. Dodjela resursa za više od jedne Srednje ili veća aplikacije pristupnika instance da bi dostupnost usluge pod uvjetima [SLA](https://azure.microsoft.com/support/legal/sla/application-gateway/v1_0/).

- **Korištenje dostupnost skupovi za svaki razina aplikacije**. Stavljanje na instance u programa [Postavljanje dostupnosti] [ availability-sets] jamčiti povezivanje najmanje jedan VM instanci unutar uvjeta [SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_2/). Ako vaš VMs nisu u skupu dostupnost nemate jamčiti zaštita ih i moguće je ih nije moguće sve neće uspjeti ili istodobno ažurirati. 

- **Razmislite o implementaciji aplikacije preko više područja.** Ako je vaša aplikacija implementiran na jedno područje, koje se rijetko pojavljuju događaj postane nedostupan cijelo područje, aplikacije i neće biti dostupna. To može biti prihvatljiva pod uvjetima SLA vaše aplikacije. Ako je tako, razmislite o implementaciji vaše aplikacije i uslugama preko više područja. Implementacija regije možete koristiti programa uzorak aktivno aktivno (distribucija zahtjeve preko više instanci aktivni) ili je aktivno pasivni uzorak (zadržavanjem "Toplo" instancu rezervnih, u slučaju da ne uspije instancu primarni). Preporučujemo da implementacija više instanci vaše aplikacije servisa preko regionalne parove. Dodatne informacije potražite u članku [tvrtke continuity i Izrada oporavak (BCDR): Uparena područja Azure](../best-practices-availability-paired-regions.md).

- **Uzorci otpornost za daljinske operacije implementirati primjereno.** Ako aplikacija ovisi o komunikaciji između servise udaljene, put komunikacije inevitably neće uspjeti. Ako postoji više pogrešaka, Preostali dobar instance vaše aplikacije servisa se može overwhelmed zahtjevima. Su nekoliko uzoraka korisne za uobičajene pogreške uključujući uzorak vremensko ograničenje, [pokušajte ponovno uzorak][retry-pattern], [prepoznavanje elektronička] [ circuit-breaker] uzorak i drugim korisnicima. Dodatne informacije potražite u članku [dizajniranje prebacuju aplikacije za Azure](guidance-resiliency-overview.md#implementing-resiliency-strategies). 

- **Koristite autoscaling odgovoriti povećanja u učitavanja.** Ako aplikacija nije konfiguriran tako da biste skalirali automatski kao učitavanja povećava, moguće je da vaše aplikacije servisa neće uspjeti ako oni postaju zasićene zahtjevima korisnički. Dodatne informacije potražite u sljedećim člancima:

    - Općenito: [kontrolni popis za skalabilnost](../best-practices-scalability-checklist.md) 
    - Aplikacije servisa Azure: [skalirali broj instanci automatski ili ručno][app-service-autoscale]
    - Servisi u oblaku: [kako automatsko skaliranje servis u oblaku][cloud-service-autoscale]
    - Virtualnim strojevima: [postavlja automatske promjene veličine i virtualnog računala skala][vmss-autoscale]


- **Implementacija asinkronog operacije kad god je moguće.** Sinkrono operacije možete monopolize resurse i blokiranje ostalih operacija dok pozivatelj čeka da biste dovršili postupak. Dizajnirajte svaki dio aplikacije da biste omogućili radi asinkronog operacije kad god je moguće. Dodatne informacije o tome kako implementirati asinkronog programiranja u C# potražite u članku [asinkronog programiranje asinkrone i await][asynchronous-c-sharp].

- **Pomoću upravitelja Azure promet usmjerili promet na vaše aplikacije za različite regije.**  [Azure promet Upravitelj] [ traffic-manager] izvodi na razini DNS za ujednačavanje opterećenja i možete usmjeriti promet za različite regije na temelju [usmjeravanje prometa] [ traffic-manager-routing] metode koju navedete i stanje krajnje točke vaše aplikacije. 

- **Konfiguriranje i testiranje probes stanja za učitavanje balancers i upravitelji promet.** Provjerite je li logiku stanja provjerava ključnih dijelove sustava i odgovori pravilno probes stanje.

    - Stanje probes za [Azure promet upravitelja] [ traffic-manager] i [Azure opterećenja] [ load-balancer] služe određenu funkciju. Za Upravitelj promet probni stanja određuje hoće li se neće iznad neke druge regije. Za raspoređivača opterećenja određuje želite li da biste uklonili s VM rotacije.      

    - Za probni u upravitelju promet na krajnjoj točki stanja biste trebali provjeriti ključnih ovisnosti koje uvode se unutar iste područja, a čije neuspjeh pokreću prebacivanje na drugoj regiji.  

    - Za raspoređivača opterećenja krajnju točku stanja treba izvješća stanja u VM. Nemoj uvrštavati druge razine ili vanjske servise. U suprotnom pogreške koji se pojavljuje izvan na VM uzrokovat će opterećenja da biste uklonili na VM rotacije.

    - Smjernice o implementacijom nadzor stanja u aplikaciji, potražite u članku [Stanja nadzor uzorak krajnjoj točki](https://msdn.microsoft.com/library/dn589789.aspx).

- **Praćenje servisa drugih proizvođača.** Ako je vaša aplikacija ovisnosti na servisima trećih strana, odredite gdje i kako uspijeva te servise drugih proizvođača i što efekta tih pogrešaka imat će na aplikaciju. Usluge trećih strana možda ne sadrže nadzor i Dijagnostika, stoga je važno da se prijavi na instanci od njih i njihovo povezivanje s vašeg računala stanja i dijagnostički prijave pomoću Jedinstveni identifikator. Dodatne informacije o najbolje prakse za nadzor i dijagnostici potražite u članku [smjernice za nadzor i dijagnostici] [ monitoring-and-diagnostics-guidance] dokumenta.

- **Provjerite je li da bilo koji servis drugih proizvođača koji zauzimaju nudi programa SLA.** Ako aplikacija ovisi o servisu trećih strana, ali trećih strana nudi jamstva dostupnost u obliku programa SLA, dostupnost vaša aplikacija i ne može se zajamčiti. Vaš SLA je samo dobro, kao komponentu najmanje dostupnih aplikacija.


## <a name="data-management"></a>Upravljanje podacima

- **Razumijevanje načina replikacije izvore podataka za aplikaciju sustava.** Podataka aplikacije bit će spremljeni u različitih vrsta izvora i imaju različite dostupnost zahtjeve. Analiza replikacije načina za svaku vrstu podataka za pohranom na servisu Azure, uključujući [Replikacije Azure prostora za pohranu](../storage/storage-redundancy.md) i [SQL replikacije baze podataka Active zemlj. –](../sql-database/sql-database-geo-replication-overview.md) Pobrinite se da su zadovoljena potrebama vaše aplikacije podataka.

- **Provjerite imaju li nema jednog korisničkog računa pristup podacima radnog i sigurnosno kopiranje.** Sigurnosne kopije podataka su ugrožena ako jedan jednog korisničkog računa imaju dozvolu za pisanje proizvodnje i sigurnosne kopije izvora. Zlonamjeran korisnik može nastojanju izbrisati sve podatke, dok je redoviti korisnik može slučajno izbrišete. Dizajniranje aplikacije da biste ograničili dozvole za svaki korisnički račun tako da samo korisnici koji zahtijevaju pristup za pisanje imaju pristup za pisanje, a zatim je samo proizvodnje ili sigurnosne kopije, ali ne oba.

- **Dokument s izvorom podataka neće uspjeti iznad, a neće uspjeti natrag obradi i ga testirate.** U slučaju gdje izvor podataka ne uspije catastrophically, Ljudski operator ćete morati slijedite skup dokumentirane upute da biste se neće putem novi izvor podataka. Ako dokumentirane korake pogreške, operator će moći uspješno slijedite ih i neće uspjeti putem resurs. Redovito testirajte uputa korake da biste provjerili je li operator pratiti ih moći uspješno neće iznad te se neće vratiti izvora podataka.

- **Provjera valjanosti podataka sigurnosne kopije.** Redovito sigurnosno kopiranje podataka provjerite je li što ste očekivali tako da pokrenete skriptu da biste provjerili valjanost integriteta podataka, sheme i upitima. Ne postoji točka imate sigurnosnu kopiju ako ona još nije korisno da biste vratili izvora podataka. Prijava i stvaranje izvješća postoje nedosljednosti da servis za sigurnosne kopije možete popraviti.

- **Razmislite o korištenju vrstu računa za pohranu koji je zemlj suvišnih.** Podaci spremljeni u račun za Azure pohranu uvijek lokalno replicirati. No postoje više strategije replikacije na raspolaganju kad je na račun za pohranu dodjeli. Odaberite [Azure pristup za čitanje zemlj suvišnih prostor za pohranu (RA GRS)](../storage/storage-redundancy.md#read-access-geo-redundant-storage) za zaštitu podataka aplikacije protiv rijetko slučaj kada cijelo područje postane nedostupan.

    > [AZURE.NOTE] Za VMs, je za replikaciju RA GRS da biste vratili VM diskova (VHD datoteke). Umjesto toga koristite [Sigurnosne kopije Azure][azure-backup].   

## <a name="operations"></a>Operacije

- **Implementacija nadzor i upozorenjem najbolje prakse u aplikaciji.** Bez odgovarajuće nadzor, Dijagnostika i upozorenjem, ne postoji način za otkrivanje pogrešaka u aplikaciji i upozorenja operator za njihovo rješavanje. Dodatne informacije o najbolje prakse potražite u članku [smjernice za nadzor i dijagnostici] [ monitoring-and-diagnostics-guidance] dokumenta.

- **Izmjerite Statistički podaci o pozivima udaljene, a zatim omogućiti podatke u tim aplikacije.**  Ako ne pratite i izvješća Statistički podaci o pozivima udaljene u stvarnom vremenu i omogućuju jednostavan način pregledajte ove podatke, tim operacije neće imati trenutačno prikaz u stanju aplikacije. A ako ste samo mjere average poziva udaljene vrijeme nećete imati dovoljno informacija da biste otkrili problemi u servisima. Sažetak poziva udaljene metriku kao što su Latencija, propusnost i pogrešaka u percentili 99 i 95. Statistička analiza izvedete metriku za otkrivanje pogrešaka koje se pojavljuju u svakom percentil.

- **Praćenje broj tranzitne iznimke i ponovne pokušaje putem odgovarajuće vremenskog razdoblja.** Ako ne praćenje i nadzor tranzitne iznimke i ponovnim pokušajima s vremenom, moguće je da problema ili pogreške nije moguće sakriti tako da vaše aplikacije pokušaj logike. To jest, ako vaš nadzora i bilježenja samo prikazuje uspjelo ili nije operacije, činjenica da operacija nije morali se ponoviti više puta zbog iznimke će biti skriven. Trend povećavanja iznimke tijekom vremena označava da servis imate problem i možda neće uspjeti. Dodatne informacije potražite u članku [ponovno pokušajte servisa neke vodiče][retry-service-guidance].

- **Implementacija sustava upozorenje za early koja upozorava operator.** Odredite ključnog učinka Pokazatelji stanja vaše aplikacije, kao što su tranzitne iznimke i daljinski poziva Latencija i postavljanje prag odgovarajuće vrijednosti za svaki od njih. Slanje upozorenja operacije kad zbirka dostigne vrijednosti praga. Postavljanje tih pragovi na razinama koje označavaju probleme da bi se one postaju ključnih i potreban je oporavak odgovor.

- **Dokumenata postupka izdanje aplikacije.** Bez detaljne izdanje dokumentacije o postupku, operator možda implementacije neispravni ažuriranje ili nepravilno Konfiguriranje postavki za svoju aplikaciju. Jasno definiranje dokumenta proces izdanje i bili sigurni da je dostupan za cijeli tim operacije. Najbolje prakse za prebacuju implementaciju aplikacije detaljno opisane u [prebacuju implementaciju] [ guidance-resilient-deployment] odjeljku upute za otpornost dokumenta.

- **Provjerite je li da više osoba na tim je put uvježbavan nadzora aplikacije i izvršite sve ručne oporavak.** Ako imate samo jedan operator članova tima koji možete nadzirati aplikacije i izbaciti korake za oporavak, ta osoba postaje jednu točku nije uspjelo. Obuka više osoba na otkrivanje i oporavak i provjerite je li uvijek barem jedan aktivan u bilo kojem trenutku.

- **Automatiziranje postupak implementacije vaše aplikacije.** Ako je osoblju operacije morati ručno implementacija aplikacije, Ljudski pogreške mogu prouzročiti implementacije neće uspjeti. Dodatne informacije o najbolje prakse za automatiziranje implementaciju aplikacije potražite u članku [prebacuju implementaciju] [ guidance-resilient-deployment] odjeljku upute za otpornost dokumenta.

- **Dizajnirajte proces izdanje Maksimiziranje dostupnosti aplikacija.** Ako proces izdanje zahtijeva services u izvanmrežni tijekom implementacije, aplikacija neće biti dostupno dok se vratite u mrežni način. Korištenje postupak implementacije [plavo-zeleno](http://martinfowler.com/bliki/BlueGreenDeployment.html) ili [oslobađanje canary](http://martinfowler.com/bliki/CanaryRelease.html) za implementaciju aplikacije da biste radni. Oba ove tehnike obuhvaćaju implementacija kod izdanje duž radnog kod tako da korisnici izdanje koda možete biti preusmjereni na radni kod u slučaju pogreške. Dodatne informacije potražite u članku [prebacuju implementaciju] [ guidance-resilient-deployment] odjeljku upute za otpornost dokumenta.

- **Prijavite se i nadzor implementacijama vaše aplikacije.** Ako koristite kopirana bez postavljanja tehnike implementacije kao što su plavo-zeleno ili canary izdanja će biti više od jedne verzije aplikacije radi u radni. Ako se problem bi se trebale provoditi, ključne je važnosti da biste utvrdili koju verziju aplikacije uzrokuje probleme. Implementacija strategije robusne zapisivanje da biste snimili suvišni informacije specifične za verziju moguće. 

- **Provjerite je li aplikacija ne pokreće s programima [ograničava Azure pretplate](../azure-subscription-service-limits.md).** Azure pretplate je ograničenja na određene vrste resursa, kao što su broj grupe resursa, broj jezgri i broj računa za pohranu.  Ako preduvjeti za aplikaciju premašuju ograničenja Azure pretplatu, stvorite drugi Azure web-mjesto pretplata i u okvir za dodjeljivanje dovoljno resursa postoji.

- **Provjerite je li aplikacija ne pokreće s programima [ograničava - servisa](../azure-subscription-service-limits.md).** Pojedinačnih servisa Azure imaju ograničenja energije &mdash; , na primjer, ograničenja prostora za pohranu, propusnost, broj veze, za u sekundi i druge metriku. Aplikacija neće uspjeti ako je pokušava koristiti resurse izvan ta ograničenja. To će rezultirati servisa regulacije i moguće isključiti za zahvaćene korisnike. 

    Ovisno o određenim servisa i preduvjeti za aplikaciju, često možete izbjeći ta ograničenja tako da promjene veličine (na primjer, odaberete neki drugi cijene sloju) ili mijenjanje veličine out (Dodavanje novih instanci).  

- **Dizajnirajte svoje aplikacije za pohranu preduvjete za nalaziti unutar ograničenja prostora za pohranu Azure skalabilnost i performanse ciljnih web-mjesta.** Azure prostora za pohranu namijenjen je funkcija unutar definiranog skalabilnost i performanse ciljnih web-mjesta, pa dizajniranja aplikacije mogu koristiti za pohranu unutar te ciljnih web-mjesta. Ako premašite te ciljeve aplikacija će se dogoditi ograničenja prostora za pohranu. Da biste riješili taj problem, dodijelite dodatne račune za pohranu. Ako pokrenete s programima ograničenje prostora za pohranu računa, Dodjela resursa za dodatna Azure pretplate i dodjela ima dodatne račune za pohranu. Dodatne informacije potražite u članku [skalabilnost Azure prostora za pohranu i ciljeve](../storage/storage-scalability-targets.md).

- **Odaberite prave veličine VM aplikacije.** Izmjerite stvarni procesora, memorije, disk i vaše VMs u radnog u/i, a zatim provjerite je li veličina VM odaberete potrebne. Ako nije, aplikacije mogu se pojaviti kapaciteta problemi kao što je na VMs postići njihove ograničenja. Veličina VM su detaljno opisane u u dokumentu [veličine za virtualnim strojevima u Azure](../virtual-machines/virtual-machines-windows-sizes.md) .

- **Određivanje Ako vaše aplikacije radno opterećenje stabilan ili koje fluktuiraju tijekom vremena.** Ako svoje radno opterećenje fluktuiraju tijekom vremena, korištenje Azure VM skaliranje automatski skaliranje postavlja broj instanci VM. U suprotnom će morati ručno povećati ili smanjiti broj VMs. Dodatne informacije potražite u članku [Pregled za skupove skaliranje virtualnog računala](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).

- **Odaberite sloj desnom servisa za baze podataka SQL Azure.** Ako aplikacija koristi baze podataka SQL Azure, provjerite je li koju ste odabrali odgovarajuću servisnu sloju. Ako ste odabrali sloj koji se ne možete učiniti vaše aplikacije baze podataka transakcije jedinica (DTU) preduvjeti, podatkovnim prometom će biti ograničio vrijeme. Dodatne informacije o odabirom tarifa ispravni usluge potražite u odjeljku na [Mogućnosti SQL baze podataka i performanse: objašnjenje što dostupna je u sloju svaki servis](../sql-database/sql-database-service-tiers.md) dokumenta. 

- **Imati tarifu za vraćanje radi implementacije.** Moguće je da implementaciju sustava aplikacija nije neće uspjeti zbog čega aplikacije da biste postati nedostupna. Dizajnirajte procesa vraćanje vratite se na zadnju dobar verziju i minimiziranja isključiti. Potražite u članku [prebacuju implementaciju] [ guidance-resilient-deployment] dio dokumenta otpornost smjernice za dodatne informacije. 

- **Stvaranje postupak za interakciju s podrškom za Azure.** Ako je postupak za kontaktiranje [Azure podržava](https://azure.microsoft.com/support/plans/) nije postavljen prije službi za podršku potreba, nedostupnost će produžen kao što je postupak za podršku preusmjereni prvi put. Uvrstite postupak obratite podršci i escalating problemi kao dio otpornost vaše aplikacije iz sustava outset.

- **Provjerite ne više od maksimalni broj računa za pohranu po pretplati pomoću aplikacije.** Azure omogućuje najviše 200 račune za pohranu po pretplati. Ako aplikacija potrebno više prostora za pohranu računa nego trenutno nisu dostupni u svoju pretplatu, morat ćete stvoriti novu pretplatu i stvorite račune za dodatni prostor za pohranu postoji. Dodatne informacije potražite u članku [Azure pretplate i ograničenja servisa, kvote, i ograničenja](../azure-subscription-service-limits.md#storage-limits).

- **Provjerite je li vaša aplikacija ne premaši skalabilnost ciljevi za diskova virtualnog računala.** Programa Azure IaaS VM podržava prilaganja broj podataka diskova ovisno o nekoliko čimbenika, uključujući veličina VM i vrstu računa za pohranu. Ako aplikacija premašuje skalabilnost ciljevi za diskova virtualnog računala, račune dodatni prostor za pohranu za dodjelu resursa i stvorite diskova virtualnog računala postoji. Dodatne informacije potražite u članku [skalabilnost Azure prostor za pohranu i ciljeve] (.. / storage/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks)

## <a name="test"></a>Test

- **Izvođenje prebacivanje i failback testiranje za svoju aplikaciju.** Ako još niste potpuno testirati prebacivanje i failback, koje ne mogu biti određene koja ovisne servise u aplikaciji se pojavljuju natrag u sinkroniziranu način tijekom Izrada oporavka. Provjerite je li vaša aplikacija zavisne services prebacivanje i prekida u točno određenim redoslijedom.

- **Izvođenje kvara unos testiranje za svoju aplikaciju.** Aplikaciju možete uspjeti mnogo razloga, kao što su isteka certifikata pojedinačno sistemske resurse u VM ili pogreške prostora za pohranu. Testirajte aplikacije u okruženju što bliže moguće proizvodnje simulaciju ili pokretanje realni pogreške. Ako, na primjer, brisanje certifikata, umjetno zauzeti resurse sustava ili brisanje izvor prostora za pohranu. Provjerite je li vaša aplikacija mogućnost da biste vratili iz svih vrsta kvarove samog i u kombinaciji. Provjera pogrešaka ne prijenos ili kaskadno putem sustava.

- **Pokrenite testira radnog pomoću stilova sintetičkih i realni korisničkih podataka u obje za potvrdu.** Testiranje i radni jednaki rijetko, stoga je važno koristite plavo-zeleno ili canary implementaciju i testiranje vaše aplikacije u radnog. Time da biste testirali aplikacije u radnog opterećenju realne i bili sigurni da će funkcionirati prema očekivanjima kada potpuno implementiran.

## <a name="security"></a>Sigurnost

- **Implementacija aplikacije razinu zaštite od raspodijeljeno uskraćivanje napada servisa (DDoS).** Azure servisa zaštićene protiv napada DDos sloju mreže. Međutim, Azure nije moguće zaštititi protiv napada aplikacijskom sloju jer je teško razlikovati zahtjevima true korisnika iz zahtjeva za zlonamjernih korisnika. Dodatne informacije o zaštiti od napada DDoS aplikacijskom sloju, u odjeljku "Zaštita protiv DDoS" [Microsoft Azure mrežne sigurnosti](http://download.microsoft.com/download/C/A/3/CA3FC5C0-ECE0-4F87-BF4B-D74064A00846/AzureNetworkSecurity_v3_Feb2015.pdf) (Preuzimanje PDF-a).

- **Implementacija načelo najmanjih ovlasti za pristup resursima aplikacije.** Zadano za pristup resursima aplikacije mora biti ograničenja, kao što je moguće. Dodjela više razine dozvola na osnovi odobrenje. Dopuštanje pretjerano čemu pristupa resursima za vaše aplikacije po zadanom može rezultirati netko nastojanju ili slučajno izbrišete resursi. Azure nudi [Kontrola pristupa na temelju uloga](../active-directory/role-based-access-built-in-roles.md) upravljanje korisničke ovlasti, ali je važno da biste provjerili najmanjih ovlasti dozvole za ostale resurse koji imaju vlastite sustavi dozvole kao što je SQL Server. 

## <a name="telemetry"></a>Telemetrijskih

- **Prijavite se telemetrijskih podataka pokrenutom aplikacije u okruženju proizvodnje.** Snimite robusne telemetrijskih podataka tijekom program radi u okruženju proizvodnje ili neće imati dovoljno informacija za dijagnosticiranje uzroka problema dok je aktivno posluživanje korisnika. Dostupne su dodatne informacije u zapisivanje najbolje prakse dostupna je u [smjernice za nadzor i dijagnostici] [ monitoring-and-diagnostics-guidance] dokumenta.

- **Implementacija zapisivanje pomoću asinkronog uzorka.** Ako sinkrono operacije zapisivanje, možda blokira kodu aplikacije. Provjerite je li vaše operacije zapisivanje primjenjuju kao asinkronog operacije. 

- **Povezivanje podataka u zapisniku preko granica servisa.** Uobičajeni aplikaciji n sloja, zahtjev korisnika mogu prolaziti nekoliko ograničenja servisa. Ako, na primjer, zahtjev za korisnika obično potječe u sloju web je proslijeđen sloju tvrtke i ista i na kraju u sloju podataka. U složenije scenarijima zahtjev korisnika mogu raspodijeliti na mnogo različitih trgovine servisa i podataka. Provjerite je li da sustav zapisivanje korelaciji pozive preko granica servisa tako da možete pratiti zahtjev cijeloj aplikacije.

##  <a name="azure-resources"></a>Resursi za Azure 

- **Korištenje predložaka Azure Voditelj resursa za dodjele resursa.** Voditelj resursa predložaka provjerite da biste automatizirali implementacijama putem komponente PowerShell ili EŽA Azure koji vodi na više pouzdanog postupak implementacije. Dodatne informacije potražite u članku [Pregled upravljanja resursima Azure][resource-manager].

- **Dodijelite smisleni nazivi resursa.** Dodjeljivanja resursa smisleni nazivi olakšava pronalaženje određenog resursa i razumijevanje njegova uloga. Dodatne informacije potražite u članku [preporučeno imenovanja konvencije za Azure resursi](guidance-naming-conventions.md) 

- **Kontrola pristupa na temelju uloga korištenje (RBAC)**. Koristite RBAC za kontrolu pristupa Azure resursa koji implementacije. RBAC možete dodijeliti uloge autorizacije članovi vašeg tima DevOps da biste spriječili nenamjerno brisanje ili promjena distribuiranih resursi. Dodatne informacije potražite u članku [Početak rada s upravljanjem pristup portalu za Azure](../active-directory/role-based-access-control-what-is.md) 

- **Pomoću zaključavanja resursa za ključne resurse, kao što su VMs.** Ograničenja resursa operator spriječiti slučajno izbrišete resurs. Dodatne informacije potražite u članku [Lock resursi s Azure Voditelj resursa](../resource-group-lock-resources.md) 

- **Regionalne parove.** Kada za implementaciju u dva područja, odaberite područja isti regionalne par. U slučaju općenite prekida, oporavak određenu regiju je prioritet iz svaki par. Neke servise kao što je zemlj suvišnih prostora za pohranu omogućuju automatsko replikacije upareni regiju. Dodatne informacije potražite u članku [tvrtke continuity i Izrada oporavak (BCDR): Uparena područja Azure](../best-practices-availability-paired-regions.md) 

- **Grupa resursa za organiziranje (opis funkcije) i životni ciklus**.  Općenito govoreći, grupu resursa mora sadržavati resursi koji se zajednički koristiti iste životni ciklus. To olakšava upravljanje implementacijama i brisanje test implementacijama te dodjeljivanje prava pristupa smanjiti vjerojatnost da implementacije radni se slučajno izbrišete ili izmijenili. Stvaranje grupe zasebnom resursa za proizvodnju, razvoj i testiranje okruženja. U implementaciji regije, postavite resursi za svako područje u zasebnom resursa grupe. To olakšava implementirati određenu regiju bez utjecaja na druge region(s). 

## <a name="azure-services"></a>Azure Services 

Sljedeće stavke kontrolnog popisa odnose na određene usluge u Azure.

###  <a name="app-service"></a>Aplikacije servisa 

- **Koristite standardni prikaz ili Premium sloju.** Ove razine podržava pripremna slobodnih i automatizirano sigurnosno kopiranje. Dodatne informacije potražite u članku [detaljnije pregled tarife za aplikacije servisa za Azure](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) 

- **Izbjegavajte skaliranje prema gore ili dolje.** Umjesto toga odaberite sloj i veličina instanci koje odgovaraju potrebama performanse u odjeljku standardne učitavanja, a zatim [Skaliranje izvan](../app-service-web/web-sites-scale.md) instance učiniti promjene u opsegu promet. Promjena veličine gore i dolje može pokrenuti je ponovno pokretanje računala.  

- **Konfiguracija spremnika kao postavki aplikacije.** Pomoću postavki aplikacije držite konfiguracijske postavke kao postavki aplikacije. Definiranje postavki u predloške Voditelj resursa ili pomoću komponente PowerShell tako da možete primijeniti ih kao dio automatiziranog implementacije / ažuriranje procesa koji se više pouzdan. Dodatne informacije potražite u članku [Konfiguriranje web-aplikacije u servisu Azure aplikacije](../app-service-web/web-sites-configure.md). 

- **Stvorite odvojene aplikacije servisa za tarife za proizvodnju i testiranje.** Nemojte koristiti slobodnih u implementaciji proizvodnje za testiranje.  Sve aplikacije u istu tarifu aplikacije servisa za zajedničko korištenje iste VM instance. Ako spremate Proizvodnja i testiranje implementacije u istu tarifu, ga mogu negativno utjecati na implementaciju radnog. Učitavanje testira, na primjer, možda slabije uživo radnog web-mjesta. Postavljanjem test implementacije u zasebnom tarifu koju izdvajanja ih iz verzije radnog.  

- **Zasebnom web-aplikacije s web-mjesta API-ji**. Ako je rješenje sučelja web-mjesto i web-mjesto API-JA, razmislite o decomposing ih u zasebnom aplikacije servisa za aplikacije. Ovaj dizajn olakšava decompose rješenje po radno opterećenje. Web-aplikacije i u API-JA možete pokrenuti u zasebnom tarifama za aplikacije servisa za tako neovisno skalirana. Ako ne trebate te razine skalabilnost pri najprije možete uvesti aplikacije u istu tarifu i ih premjestite u zasebnom tarife kasnije, ako je potrebno.

- **Izbjegavajte značajku sigurnosnu kopiju aplikacije servisa za sigurnosno kopiranje baze podataka Azure SQL.** Umjesto toga koristite [baze podataka SQL automatizirano sigurnosno kopiranje][sql-backup]. Sigurnosno kopiranje aplikacije servisa za izvozi baze podataka SQL .bacpac datoteke, što troškove DTUs.  

- **Implementacija pripremna vremensko razdoblje.** Stvaranje implementacije razdoblje za pripremna. Implementacija aplikacije ažuriranja pripremna vremensko razdoblje, pa provjerite implementacije prije zamjena u radnog. Time se smanjuju izgledi neispravni ažuriranja u radnog. Također osigurava da su sve instance warmed prije nego što se zamjenjuju u radnog. Mnoge aplikacije imati značajan warmup i Hladno početno vrijeme. Dodatne informacije potražite u članku [Postavljanje pripremna okruženja za web-aplikacije u servisu Azure aplikacije](../app-service-web/web-sites-staged-publishing.md). 

-  **Stvaranje vremensko razdoblje za implementaciju držati implementacije (LKG) posljednje dobro.** Prilikom implementacije ažuriranje radnog premjestite prethodne implementacije radnog LKG razdoblje. To se olakšava vratiti neispravni implementacije. Ako kasnije otkrijete problem, brzo možete vratiti LKG verziju. Dodatne informacije potražite u članku [arhitektura Azure referenca: osnovni web-aplikacije](guidance-web-apps-basic.md). 

- **Omogućivanje Dijagnostika zapisivanje**, uključujući zapisivanje aplikacije i zapisivanje poslužitelja web. Zapisivanje je važno za nadzor i Dijagnostika. U odjeljku [Omogućivanje Dijagnostika bilježenje u zapisnik web-aplikacijama u aplikacije servisa za Azure](../app-service-web/web-sites-enable-diagnostic-log.md)

- **Zapisnik bloba prostora za pohranu**. To olakšava mogli ste prikupljati i analizirati podatke. 

- **Stvaranje računa zasebnom prostora za pohranu za zapisnike.** Nemojte koristiti isti prostor za pohranu račun zapisnika i podataka iz aplikacije. To sprječava zapisivanje iz smanjuje performanse računala. 

- **Praćenje performansi.** Koristite performanse servis kao što je [Novi Relic](http://newrelic.com/) ili [Uvida aplikacije](../application-insights/app-insights-overview.md) na monitoru računala izvedbe i ponašanja opterećenju za nadzor.  Praćenje performansi daje uvid u stvarnom vremenu u aplikaciju. Omogućuje vam dijagnosticiranje problema i analizu korijenskog uzroka pogrešaka. 


###  <a name="application-gateway"></a>Pristupnik za aplikaciju 

- **Dodjela barem dvije instance.** Implementacija aplikacije pristupnik s barem dvije instance. Jednokratan je jednu točku nije uspjelo. Korištenje dva ili više instanci zalihosti i skalabilnost. Da bi se zadovoljavate [SLA](https://azure.microsoft.com/support/legal/sla/application-gateway/v1_0/), morate Dodjela dva ili više instanci Srednje ili veća. 

### <a name="azure-search"></a>Azure pretraživanja

- **Dodjela resursa za više od jedne replike.** Koristite najmanje dva replike za čitanje visoku dostupnost ili tri za čitanje i pisanje visoku dostupnost.

- **Konfiguriranje indexers za implementacije više područja.** Ako imate više područja implementaciju, razmislite o mogućnostima continuity u indeksiranja.

    - Ako je izvor podataka zemlj replicirati, pokažite svaki indeksiranje svaki regionalne servisa za pretraživanje Azure njegove izvorišne replike lokalnih podataka.  

    - Ako izvor podataka nije zemlj replicirati, pokažite više indexers na isti izvor podataka, tako da se servisa Azure pretraživanja u više područja kontinuirano i neovisno indeksiranje izvora podataka. Dodatne informacije potražite u članku [pretraživanje Azure performanse i optimizirati pitanja vezana uz][search-optimization].

###  <a name="azure-storage"></a>Azure prostora za pohranu 

- **Da biste postigli podataka aplikacije koristite za pohranu zemlj suvišnih pristup za čitanje (RA GRS).** RA GRS prostora za pohranu replicira podatke na sekundarnom regija, a omogućuje pristup samo za čitanje iz sekundarne područja. Postoji li za pohranu prekida u primarni regiji, aplikacija možete pročitati podatke iz sekundarne područja. Dodatne informacije potražite u članku [replikacije Azure prostora za pohranu](../storage/storage-redundancy.md).

- **Diskova VM za korištenje Premium prostora za pohranu** Dodatne informacije potražite u članku [prostora za pohranu Premium: visokih performansi prostor za pohranu radnih opterećenja virtualnog računala Azure](../storage/storage-premium-storage.md).

- **Za pohranu u redu čekanja, stvorite sigurnosnu kopiju reda čekanja u drugoj regiji.** Za pohranu reda čekanja samo za čitanje replike sadrži ograničen koristi, jer ne red čekanja ili dequeue stavke. Stvorite sigurnosnu kopiju reda čekanja na računu za pohranu u drugoj regiji. Postoji li za pohranu prekida, aplikacija možete koristiti reda čekanja sigurnosne kopije dok primarni regija ponovo postane dostupna. Na taj način aplikaciju možete i dalje obraditi nove zahtjeve.  

###  <a name="documentdb"></a>DocumentDB 

- **Preko područja replicirati bazu podataka.** S računom regije DocumentDB baza podataka sadrži jedan unos regija i više područja za čitanje. Ako postoji pogreška u području za unos, možete čitati iz drugog replike. SDK klijentskih ovime rukuje automatski. Možete uspjeti i iznad područja za pisanje u drugoj regiji. Dodatne informacije potražite u odjeljku [Podaci za raspodjelu globalno s DocumentDB](../documentdb/documentdb-distribute-data-globally.md).


###  <a name="sql-database"></a>SQL baze podataka 

- **Koristite standardni prikaz ili Premium sloju.** Ove razine navedite razdoblje više Vrati točke u vrijeme (35 dana). Dodatne informacije potražite u članku [Mogućnosti SQL baze podataka i performanse](../sql-database/sql-database-service-tiers.md).

- **Omogućite nadzor SQL baze podataka.** Nadzor može se koristiti za dijagnosticiranje zlonamjernog napada ili Ljudski pogreške. Dodatne informacije potražite u članku [Uvod u SQL baze podataka nadzor](../sql-database/sql-database-auditing-get-started.md). 

- **Korištenje aktivni zemlj. – replikacije** Koristite Active replikacije zemlj da biste stvorili čitljiv sekundarni u nekoj drugoj regiji.  Ako vaš primarni baze podataka ne uspije, ili jednostavno mora biti izvanmrežno, obavljanje ručno prebacivanje na sekundarnom bazu podataka.  Dok ne preko sekundarne baze podataka ostaje samo za čitanje.  Dodatne informacije potražite u članku [SQL baza podataka aktivna zemlj. – replikacije](../sql-database/sql-database-geo-replication-overview.md). 

- **Korištenje sharding**. Razmislite o korištenju sharding za vodoravno particija baze podataka. Sharding unijeti odvajanja kvara. Dodatne informacije potražite u članku [Promjena veličine odgovor s bazom podataka SQL Azure](../sql-database/sql-database-elastic-scale-introduction.md). 

- **Koristite točke u vrijeme Vrati se može oporaviti iz Ljudski pogreške.**  Vraćanje točke u vrijeme vraća bazu podataka ranije točke u vremenu. Dodatne informacije potražite u članku [oporavak baze podataka Azure SQL pomoću sigurnosne kopije baze podataka za automatiziranog][sql-restore].

- **Pomoću zemlj Vrati se može oporaviti iz servisa prekida.** Zemlj vraćanja vraća bazu podataka iz sigurnosne kopije zemlj suvišnih.  Dodatne informacije potražite u članku [oporavak baze podataka Azure SQL pomoću sigurnosne kopije baze podataka za automatiziranog][sql-restore].


###  <a name="sql-server-running-in-a-vm"></a>SQL Server (koja se izvodi u na VM)

- **Replicirati bazu podataka.** SQL Server uvijek na dostupnost grupe koristite za replikaciju baze podataka. Nudi visoke dostupnosti ne uspijete jednu instancu sustava SQL Server. Dodatne informacije potražite u članku [Dodatne informacije...](guidance-compute-n-tier-vm.md) 

- **Sigurnosno kopiranje baze podataka**. Ako već koristite [Sigurnosne kopije Azure](https://azure.microsoft.com/documentation/services/backup/) sigurnosnu kopiju vašeg VMs, razmislite o korištenju [Azure sigurnosna kopija radnih opterećenja sustava SQL Server pomoću DPM](../backup/backup-azure-backup-sql.md). Na taj se način postoji jedan ulogu pričuvni administrator za tvrtku ili ustanovu, a objedinjenih oporavak postupak za VMs i SQL Server. U suprotnom pomoću [SQL Server upravlja sigurnosnu kopiju za Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx). 


###  <a name="traffic-manager"></a>Upravitelj promet 

- **Izvođenje ručno failback.** Nakon prebacivanje na promet Upravitelj izvođenje ručno failback umjesto da automatski ne uspijeva natrag. Prije nego što ne uspijeva natrag, provjerite jesu li sve aplikacije podsustava dobar.  U suprotnom, možete stvoriti situaciji gdje aplikacije natrag i naprijed Zrcali između centre za podatke. Dodatne informacije potražite u članku [Pokretanje VMs u više područja](guidance-compute-multiple-datacenters.md).

- **Stvaranje krajnje za probni stanja**. Stvorite prilagođene krajnju točku koja izvješća o stanju cjelokupan aplikacije. Time se omogućuje promet Upravitelj uvoza putem ključnih put ne uspije, ne samo sučelje. Ako je bilo koji od ključne važnosti ovisnost dobro ili ga nije moguće doseći krajnju točku mora vratiti HTTP kod pogreške. No se ne izvješće pogrešaka za rješavaju servise. U suprotnom probni stanja može pokrenuti prebacivanje kada ona nije potrebna, stvaranje false positives. Dodatne informacije potražite u članku [Upravitelj promet krajnjoj točki nadzor i prebacivanje](../traffic-manager/traffic-manager-monitoring.md).


###  <a name="virtual-machines"></a>Virtualnim strojevima 

- **Nemojte pokretati radno opterećenje proizvodnje na jednom VM.** Jedan VM implementacije nije prebacuju na planirane ili neplanirano održavanja. Umjesto toga, smjestite više VMs dostupnosti skupa ili skup VM skala s raspoređivača opterećenja ispred. Da bi se zadovoljavate na SLA, najmanje dva virtualnim strojevima mora se implementirati unutar istog dostupnosti skupa. Dodatne informacije potražite u članku [Pregled za skupove skaliranje virtualnog računala](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md). 

- **Navedite dostupnost postaviti prilikom dodjele resursa u VM.** Trenutno ne postoji način da biste dodali resursima VM raspoloživost postaviti nakon na VM je dodijeljena. Prilikom dodavanja nove VM na postojeće dostupnost postavljanje, provjerite jeste li stvorili na NIC za na VM i dodavanje NIC-a u pozadinskoj skupna adresa na raspoređivača opterećenja. U suprotnom raspoređivača opterećenja neće mrežni promet usmjerili na tom VM. 

- **Smjestite svaki razina aplikacije u zasebnom dostupnosti skupa.** U aplikaciji N sloja, nemojte smještati VMs iz različite razine u isti skup dostupnost. VMs u skupu dostupnost spremaju se svim domenama kvara (FDs) i ažuriranje domene (UD). Međutim, da biste dobili zalihosti prednost FDs i UDs, svaki VM u skupu dostupnost moraju imati mogućnost rukovati isti zahtjevi klijenta. 

- **Odaberite prave veličine VM performanse potrebama.** Prilikom pomicanja postojeće radno opterećenje za Azure, započnite s veličina VM koji je najsličniji onome lokalne poslužitelje. Zatim mjerenje performanse stvarni radno opterećenje vezana uz procesora i memorije disk IOPS i prilagoditi veličinu po potrebi. Pomaže da biste bili sigurni aplikacije ponaša očekivani u okruženje oblaka. Osim toga, ako trebate više NIC-ovi, imajte na umu ograničenja NIC za svaki veličinu. 

- **Korištenje premium prostora za pohranu za VHDs.** Prostor za pohranu Azure Premium pruža podršku visokih performansi, najniža Latencija disk. Dodatne informacije potražite u članku [prostora za pohranu Premium: visokih performansi prostor za pohranu radnih opterećenja virtualnog računala Azure](../storage/storage-premium-storage.md) odaberite veličina VM koji podržava premium prostora za pohranu. 

- **Stvaranje računa zasebnom prostora za pohranu za svaku VM.** Mjesto VHDs za jednu VM zasebnom prostora za pohranu račun. Pomaže da biste izbjegli odlazak IOPS ograničenja za račune za pohranu. Dodatne informacije potražite u članku [skalabilnost Azure prostora za pohranu i ciljeve](../storage/storage-scalability-targets.md). Međutim, ako implementirate velik broj VMs, imajte na umu ograničenja po pretplatu za račune za pohranu. Potražite u članku [ograničenja prostora za pohranu](../azure-subscription-service-limits.md#storage-limits).

- **Stvaranje računa zasebnom prostora za pohranu za dijagnostičke zapisnike**. Ne Zapisivanje dijagnostičkih zapisnika jedan račun za pohranu kao VHDs da biste izbjegli Zapisivanje dijagnostičkih podataka utječe na IOPS za VM diskova. Standardni račun lokalno suvišnih prostora za pohranu (LRS) je dovoljni za dijagnostičke zapisnike. 

- **Instaliranje aplikacija na podatkovni disk, ne OS disk.** U suprotnom može doći do ograničenje veličine na disku. 

- **Sigurnosne kopije VMs pomoću Azure sigurnosnu kopiju.** Sigurnosno kopiranje Zaštita od gubitka podataka slučajne. Dodatne informacije potražite u članku [Zaštita VMs Azure s sigurnog za usluge oporavak](../backup/backup-azure-vms-first-look-arm.md).

- **Omogućivanje dijagnostičke zapisnike**, uključujući metriku osnovni stanja, Infrastruktura zapisnika i [Pokretanje dijagnostike][boot-diagnostics]. Pokretanje dijagnostike omogućuju Dijagnosticiranje pogreške prilikom pokretanja Ako vaš VM dobiti u koje nisu izraditi stanje. Dodatne informacije potražite u članku [Pregled programa Azure dijagnostičke zapisnike][diagnostics-logs].

- **Korištenje AzureLogCollector nastavak**. (Windows VMs samo.) Ovaj kućni broj objedinjuje platforme Azure zapisnika i prenese ih Azure prostor za pohranu, bez operator daljinski zapisivanje u na VM. Dodatne informacije potražite u članku [AzureLogCollector nastavak](../virtual-machines/virtual-machines-windows-log-collector-extension.md).


###  <a name="virtual-network"></a>Virtualne mreže 

- **Da biste whitelist ili bloka javnu IP adresa, dodajte je NSG podmreži.** Blokiranje pristupa od zlonamjernih korisnika ili dopustiti pristup samo s korisnicima koji imaju ovlasti za pristup aplikaciji.  

- **Stvorite prilagođene stanja probni.** Probes stanje raspoređivača opterećenja možete testirati HTTP-a ili TCP. Ako je VM izvodi HTTP poslužitelj, probni HTTP je bolje pokazatelj stanja status od TCP probni. Da biste postigli probni programa HTTP koristite prilagođene krajnju točku koja izvješća stanje aplikaciju, uključujući sve ključnih ovisnosti. Dodatne informacije potražite u članku [Pregled Azure raspoređivača opterećenja](../load-balancer/load-balancer-overview.md).

- **Nemojte blokirati probni stanja.** Stanje raspoređivača opterećenja probni šalje se s poznatim IP adrese, 168.63.129.16. Nemojte blokirati promet ili s ovom IP u bilo kojem pravila vatrozida ili mreže sigurnosnih grupa (NSG) pravila. Blokiranje stanje probni prouzročiti opterećenja da biste uklonili na VM rotacije. 

- **Omogućite zapisivanje opterećenja.** U zapisnicima navedene koliko VMs na pozadinske stiže mrežni promet zbog nije uspjelo probni odgovore. Dodatne informacije potražite u članku [zapisnika analize Azure raspoređivača opterećenja](../load-balancer/load-balancer-monitor-log.md).


<!-- links -->
[app-service-autoscale]: ../monitoring-and-diagnostics/insights-how-to-scale.md
[asynchronous-c-sharp]:https://msdn.microsoft.com/library/mt674882.aspx
[availability-sets]:../virtual-machines/virtual-machines-windows-manage-availability.md
[azure-backup]: https://azure.microsoft.com/documentation/services/backup/
[boot-diagnostics]: https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/
[circuit-breaker]: https://msdn.microsoft.com/library/dn589784.aspx
[cloud-service-autoscale]: ../cloud-services/cloud-services-how-to-scale.md
[diagnostics-logs]: ../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md
[fma]: guidance-resiliency-failure-mode-analysis.md
[guidance-resilient-deployment]: guidance-resiliency-overview.md#resilient-deployment
[load-balancer]: load-balancer/load-balancer-overview.md
[monitoring-and-diagnostics-guidance]: ../best-practices-monitoring.md
[resource-manager]: ../azure-resource-manager/resource-group-overview.md
[retry-pattern]: https://msdn.microsoft.com/library/dn589788.aspx
[retry-service-guidance]: ../best-practices-retry-service-specific.md
[search-optimization]: ../search/search-performance-optimization.md
[sql-backup]: ../sql-database/sql-database-automated-backups.md
[sql-restore]: ../sql-database/sql-database-recovery-using-backups.md
[traffic-manager]: ../traffic-manager/traffic-manager-overview.md
[traffic-manager-routing]: ../traffic-manager/traffic-manager-routing-methods.md
[vmss-autoscale]: ../virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview.md
