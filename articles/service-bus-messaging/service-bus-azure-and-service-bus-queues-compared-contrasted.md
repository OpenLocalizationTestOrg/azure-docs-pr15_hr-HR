<properties 
    pageTitle="Azure redova i usluge Bus redovi – Usporedba i iz Outlooka nasuprot | Microsoft Azure"
    description="Analizira razlike i sličnosti između dvije vrste reda čekanja nudi Azure."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="tysonn" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="tbd"
    ms.date="09/23/2016"
    ms.author="sethm" />

# <a name="azure-queues-and-service-bus-queues---compared-and-contrasted"></a>Azure redova i usluge Bus redovi – Usporedba i iz Outlooka nasuprot

U ovom se članku analizira razlike i sličnosti između dvije vrste reda čekanja danas nudi Microsoft Azure: Azure redovima i usluge Bus redova. Pomoću ove informacije možete usporediti i kontrast odgovarajući tehnologije i moći odluku dodatno informirali o koje rješenje najbolje odgovara vašim potrebama.

## <a name="introduction"></a>Uvod

Microsoft Azure podržava dvije vrste mehanizme reda čekanja: **Azure redova** "i" **Redovi Bus servisa**.

**Azure redova**, koji su dio infrastruktura za [Azure prostora za pohranu](https://azure.microsoft.com/services/storage/) , značajka jednostavne utemeljen na OSTALE Get/stavi/uvid u sučelju pruža pouzdan, stalni poruke unutar i između servisa.

**Servis Bus redova** su dio na šira [Azure poruka](https://azure.microsoft.com/services/service-bus/) infrastrukture koji podržava čekanja te objavljivanja/pretplate, remoting servisa Web i integracija uzoraka. Dodatne informacije o redovima Bus servisa, teme i pretplate i preusmjeravanje potražite u članku [Pregled Bus usluge razmjene poruka](service-bus-messaging-overview.md).

Premda oba stavljanja tehnologije postoji istovremeno, Azure redova uvedene najprije kao mehanizam za pohranu namjenski reda čekanja on nudi servisa Azure prostora za pohranu. Ugrađenih servisa Bus redova pri vrhu šira "brokered poruka" Infrastruktura namijenjene integraciji aplikacije ili aplikacije komponente koje može obuhvaćati više protokola za komunikaciju, ugovore podataka pouzdanosti domene i/ili mreže okruženja.

U ovom se članku uspoređuju tehnologije dva reda čekanja nudi Azure tako da razgovarate o razlikama između ponašanje i implementacija značajke koje nudi svaki. Članak sadrži i smjernice za odabir koje značajke najbolje odgovaraju vašim potrebama za razvoj aplikacija.

## <a name="technology-selection-considerations"></a>Razmatranja odabira tehnologije

Azure redovima i usluge Bus redova su implementacije reda čekanja usluge trenutno Microsoft Azure poruka. Svaka ima malo drugačiju značajka skup, što znači da možete odabrati jedan ili drugi, ili oboje, ovisno o potrebama određenog rješenja ili tvrtke/tehničkih problema su rješavanja.

Prilikom određivanja tehnologiji koju stavljanja odgovara namjenu određenom rješenja, rješenje projektantima i razvojnim inženjerima razmislite o preporuke u nastavku. Dodatne informacije potražite u sljedećem odjeljku.

Kao rješenje Graditelj/razvojni inženjer, **razmislite o korištenju Azure redova** kada:

- Aplikacija morate spremiti veće od 80 GB poruke u redu čekanja, gdje poruke imaju kraće od sedam dana života.

- Aplikacija želi pratiti napredak za obradu poruku unutar reda. To je korisno ako ruši tempiranja obrade poruke. Sljedeći tempiranja možete koristiti te podatke da biste nastavili s mjesta na kojem su stali prethodnog radnih.

- Potrebna vam je poslužitelj zapisnicima svih transakcija izvršeno na temelju vašeg redova.

Kao rješenje Graditelj/razvojni inženjer, **razmislite o korištenju servisa Bus redova** kada:

- Rješenje mora biti primati poruke bez potrebe za ankete reda. Pomoću servisa Bus, to se može postići pomoću dugu ankete primanje operacija putem protokola TCP-poštu s podrškom za Bus servisa.

- Rješenje zahtijeva reda čekanja možete unijeti na sigurno prvog-u-first-out (FIFO) naručili isporuke.

- Želite da se simetričnu sučelje u Azure i sustavu Windows Server (privatne oblaka). Dodatne informacije potražite u članku [Bus servisa za Windows Server](https://msdn.microsoft.com/library/dn282144.aspx).

- Rješenje moraju imati mogućnost za automatsko otkrivanje duplikata za podršku.

- Želite aplikaciju postupak poruke kao paralelno strujanja dugoročnih (poruke su povezane s strujanje pomoću [ID sesije](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.sessionid.aspx) svojstva poruku). U ovom modelu svaki čvor u aplikaciji dosta competes za strujanja umjesto poruke. Kada strujanje dobiva dosta čvor, čvor možete provjeriti stanje stanje strujanje aplikacije pomoću transakcije.

- Rješenje potrebno je transakcijskih ponašanje i atomicity prilikom slanja ili primanja više poruka iz reda.

- U vrijeme važenja (TTL) osobine radno opterećenje specifičnim aplikacijama može biti veći od 7 dana razdoblja.

- Aplikacija upravlja porukama koje može biti veći od 64 KB, ali će ograničiti pristup ne vjerojatno 256 KB.

- Postupanje s potrebama omogućuje pristup na temelju uloga modela redovima i druge prava i dozvole za pošiljatelja i primatelja.

- Veličina poštanskog reda čekanja će rasti veće od 80 GB.

- Želite li koristiti protokol razmjenu utemeljenih na standardima AMQP 1.0. Dodatne informacije o AMQP potražite u članku [Pregled AMQP Bus servisa](./service-bus-amqp-overview.md).

- Možete envision usmjerenog migracije sa sustavom reda čekanja point-to-point komunikacije na uzorka poruka sustava exchange koji omogućuje objedinjenog integraciju dodatne primatelje (pretplatnike), od kojih svaka prima neovisno kopije neke ili sve poruke poslane na red. Drugu mogućnost odnosi se na mogućnost Objavi/pretplata nativno nudi Bus servisa.

- Razmjenu rješenje moraju imati mogućnost podržava jamstva isporuke "Pri Većina-jednom" bez potrebe za sastavljanje dodatna Infrastruktura komponente.

- Želite li moći objaviti i zauzeti serijama od poruke.

- Potrebna vam je potpuna Integracija s stog komunikacije Windows Communication Foundation (WCF) u .NET Framework.

## <a name="comparing-azure-queues-and-service-bus-queues"></a>Usporedba Azure redovima i usluge Bus reda čekanja

Tablica u sljedećim se odjeljcima navedite logičke grupiranja reda čekanja značajki i omogućuju vam Usporedba na prvi pogled, mogućnosti dostupne u Azure redovima i redova Bus servisa.

## <a name="foundational-capabilities"></a>Foundational mogućnosti

U ovom se odjeljku uspoređuju neke od mogućnosti temeljne stavljanja nudi Azure redovima i usluge Bus redova.

|Kriteriji uspoređivanja|Azure reda čekanja|Servis Bus reda čekanja|
|---|---|---|
|Redoslijed jamstva|**ne** <br/><br>Dodatne informacije potražite u članku prvi bilješke u odjeljku "Dodatne informacije".</br>|**Da – prvi-u-First Out (FIFO)**<br/><br>(pomoću poruka sesije)|
|Jamstva isporuke|**Pri najmanjih-jednom**|**Pri najmanjih-jednom**<br/><br/>**Na većini-jednom**|
|Podrška za atomske operacije|**ne**|**Da**<br/><br/>|
|Primanje ponašanje|**Osobe koje nisu blokiranje**<br/><br/>(dovršava odmah ako nalazi se ne prikazuje se nova poruka)|**Blokiranje sa/bez vremenskog ograničenja**<br/><br/>(ponuda dugog ankete ili ["Comet postupak"](http://go.microsoft.com/fwlink/?LinkId=613759))<br/><br/>**Osobe koje nisu blokiranje**<br/><br/>(pomoću .NET upravljani API za samo)|
|Automatske stil API-JA|**ne**|**Da**<br/><br/>[OnMessage](https://msdn.microsoft.com/library/azure/jj908682.aspx) i **OnMessage** sesije .NET API-JA.|
|Primanje načinu rada|**Uvid u & Zakup**|**Uvid u & Lock**<br/><br/>**Primanje i Izbriši**|
|Način isključivog pristupa|**Zakup-poštu**|**Zaključavanje-poštu**|
|Zakup/Lock trajanje|**30 sekundi (zadano)**<br/><br/>**7 dana (maksimalno)** (Možete obnoviti ili pustite poruke Zakup pomoću [UpdateMessage](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.updatemessage.aspx) API.)|**60 sekundi (zadano)**<br/><br/>Možete obnoviti lokot poruka pomoću [RenewLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.renewlock.aspx) API-JA.|
|Preciznost Zakup/Lock|**Razina poruke**<br/><br/>(svaka poruka može imati i različite vremenskog ograničenja vrijednost, koje zatim možete ažurirati potrebi tijekom obrade poruke, pomoću [UpdateMessage](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.updatemessage.aspx) API-JA)|**Red čekanja razina**<br/><br/>(svaki reda čekanja ima lock preciznosti primjenjuje na sve njezine poruke, ali možete obnoviti lock pomoću [RenewLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.renewlock.aspx) API.)|
|Odbacivanja primanje|**Da**<br/><br/>(izričito broj poruka prilikom preuzimanja poruka maksimalno 32 poruke do)|**Da**<br/><br/>(implicitno Omogućivanje stara dohvaćanje svojstava ili izričito pomoću transakcije)|
|Slanje odbacivanja|**ne**|**Da**<br/><br/>(pomoću transakcije ili grupnog slanja promjena klijentsko)|

### <a name="additional-information"></a>Dodatne informacije

- Poruke u redovima Azure su obično prvog-u-first-out, ali ponekad zna biti ispravan; na primjer, trajanje vremenskog ograničenja vidljivost na poruku isteka (na primjer, uslijed klijentska aplikacija ruši tijekom obrade). Po isteku vremenskog ograničenja vidljivost poruku postaje vidljivo ponovno u redu čekanja za drugi tempiranja da biste ga dequeue. U tom trenutku upravo vidljivi poruke možda nalaziti u redu čekanja (da biste se dequeued ponovno) nakon poruke koja se izvorno nalazila enqueued iza nje.

- Ako već koristite blob polja za pohranu Azure ili tablice, i početak korištenja redovi, su zajamčiti 99.9% dostupnost. Ako koristite blob-ova ili tablice sa servisa Bus redovima, imat ćete donjem dostupnost.

- Sigurno FIFO uzorak u redovima servisa Bus zahtijeva korištenje sesije razmjene poruka. Za slučaj da tijekom obrade poruke primljene u načinu **uvid i zaključavanje** ruši se aplikacija, sljedeći put reda čekanja primatelj Prihvati sesije razmjene poruka će započeti porukom nije uspjelo nakon isteka njegov razdoblje vrijeme važenja (TTL).

- Azure redova osmišljeni su za podržava standardnu stavljanja scenarije, kao što su decoupling aplikacije komponente da biste povećali skalabilnost i toleranciju na pogreške, pogreške učitavanje ujednačavanje te izgradnju tijekovima rada za postupak.

- Servis Bus redova podržava isporuke jamstva *Pri najmanjih-jednom* . Uz to, na *Pri Većina-jednom* semantičkog biti podržane pomoću stanje sesije za pohranu stanja računala i pomoću transakcije atomically poruka i ažuriranje stanja sesije.

- Azure redova omogućuje standardizirani su i dosljedni model programiranja redovi, tablice i blob-ova – za razvojne inženjere i za operacije timovima.

- Servis Bus redova nudi podršku za lokalni transakcije u kontekstu jedne reda.

- **Primanje i brisanje** način podržava Bus servisa omogućuje da biste smanjili razmjenu operacija count (i pridruženi trošak) gotovinu jamstvo spušteni isporuke.

- Azure redova leases pružiti mogućnost da biste proširili leases za poruke. Time se omogućuje zaposlenici zaduženi za održavanje kratki leases poruka. Dakle, ako se ruši se u, poruka može brzo obraditi ponovno drugi tempiranja. Osim toga, u možete proširiti Zakup na poruku ako je potrebno obrade dulje od trenutnog vremena Zakup.

- Azure redova nude vidljivost vremensko ograničenje koje možete postaviti nakon na enqueueing ili dequeuing poruke. Uz to, možete ažurirati poruke s različitim Zakup vrijednosti pri izvođenju i ažuriranje različite vrijednosti u svim porukama u istom redu čekanja. Vremenska ograničenja servisa Bus lock su definirani u metapodataka reda čekanja; Međutim, možete obnoviti zaključavanje tako da nazovete metodu [RenewLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.renewlock.aspx) .

- Maksimalno vremensko ograničenje za na blokiranje primanje operacije u redovima Bus servis je 24 dana. Međutim, koji se temelji na OSTALE vremensko ograničenje imati maksimalne vrijednosti 55 sekundi.

- Klijentsko grupnog slanja promjena koje ste dobili od servisa Bus omogućuje reda čekanja klijenta za skupnu više poruka u operaciji jedan Pošalji. Grupno slanje promjena dostupna je samo za operacije asinkronog Pošalji.

- Značajke kao što su ceiling 200 TB Azure redova (više kada virtualizacije računi) i neograničeno reda čekanja postane idealna platformu SaaS davatelja usluga.

- Azure redova navedite na fleksibilne i performant delegirani mehanizam za kontrolu pristupa.

## <a name="advanced-capabilities"></a>Dodatne mogućnosti

U ovom se odjeljku uspoređuju naprednih mogućnosti nudi Azure redovima i usluge Bus redova.

|Kriteriji uspoređivanja|Azure reda čekanja|Servis Bus reda čekanja|
|---|---|---|
|Zakazana isporuka|**Da**|**Da**|
|Automatsko reagira lettering|**ne**|**Da**|
|Povećanje vrijednost vrijeme važenja reda čekanja|**Da**<br/><br/>(putem lokalno ažuriranje vidljivost vremenskog ograničenja)|**Da**<br/><br/>(pružati putem namjenski funkcija API-JA)|
|Poison podrška za poruke|**Da**|**Da**|
|Lokalno ažuriranje|**Da**|**Da**|
|Zapisnik transakcija poslužiteljsko|**Da**|**ne**|
|Prostor za pohranu metrika|**Da**<br/><br/>**Minute metriku**: nudi u stvarnom vremenu metriku dostupnost, TPS, API poziva broji, broji pogreške i drugo – sve u stvarnom vremenu (pridružuje u minuti i potrebnom za nekoliko minuta iz samo gdje je u radnog. Dodatne informacije potražite u članku [O metrike pohrane analize](https://msdn.microsoft.com/library/azure/hh343258.aspx).|**Da**<br/><br/>(skupno upite tako da nazovete [GetQueues](https://msdn.microsoft.com/library/azure/hh293128.aspx))|
|Upravljanje stanja|**ne**|**Da**<br/><br/>[Microsoft.ServiceBus.Messaging.EntityStatus.Active](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.entitystatus.aspx), [Microsoft.ServiceBus.Messaging.EntityStatus.Disabled](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.entitystatus.aspx) [Microsoft.ServiceBus.Messaging.EntityStatus.SendDisabled](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.entitystatus.aspx), [Microsoft.ServiceBus.Messaging.EntityStatus.ReceiveDisabled](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.entitystatus.aspx)|
|Automatsko prosljeđivanje poruka|**ne**|**Da**|
|Čišćenje reda čekanja (opis funkcije)|**Da**|**ne**|
|Grupa poruka|**ne**|**Da**<br/><br/>(pomoću poruka sesije)|
|Stanje aplikacije po poruke grupi|**ne**|**Da**|
|Otkrivanje duplikata|**ne**|**Da**<br/><br/>(konfigurirati na strani pošiljatelja)|
|Integracija WCF|**ne**|**Da**<br/><br/>(nudi povezivanja Izlaz u-tvorničke WCF)|
|Integracija WF|**Prilagođeno**<br/><br/>(zahtijeva stvaranje prilagođene aktivnosti WF)|**Izvorni**<br/><br/>(nudi Izlaz u-tvorničke WF aktivnosti)|
|Pregledavanje poruka grupe|**ne**|**Da**|
|Dohvaćanje sesije poruka po ID-a|**ne**|**Da**|

### <a name="additional-information"></a>Dodatne informacije

- Oba stavljanja tehnologije omogućite poruka za planiranje za isporuku kasnije.

- Automatsko prosljeđivanje reda čekanja omogućuje tisuće reda čekanja za automatsko prosljeđivanje svoje poruke u jedan red iz kojeg primanju aplikacije troši poruku. Možete koristiti ovaj mehanizam da biste postigli sigurnost, upravljanje tijek i izdvajanja prostora za pohranu između svake poruke programa publisher.

- Azure redova nudi podršku za ažuriranje sadržaja poruke. Ta je funkcija možete koristiti za održavanju informacije o stanju i rastuće tijeku ažuriranja u poruku tako da se ona se obrađuje iz zadnjeg poznati Kontrolna točka, ne biste morali počinjati iznova. Uz redove Bus servisa, možete omogućiti istu scenarij pomoću sesije poruke. Sesije omogućuju vam da biste spremili i dohvatiti obrada stanja računala (pomoću [SetState](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.setstate.aspx) i [GetState](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.getstate.aspx)).

- [Reagira lettering](service-bus-dead-letter-queues.md), koje se samo podržava redova Bus servisa, može biti korisno za izoliranja poruke koje nije moguća uspješno tako da prima aplikacije ili poruke ne može pristupiti svojim odredište zbog do istekli vrijeme važenja (TTL) svojstvo. TTL vrijednost određuje koliko dugo poruka ostaje u redu. S Bus servisa poruka će se premjestiti posebne red naziva $DeadLetterQueue kada istekne razdoblje TTL.

- Da biste pronašli "poison" poruke u redovima Azure kada dequeuing poruke aplikacije ispituje svojstvo **[DequeueCount](https://msdn.microsoft.com/library/azure/dd179474.aspx)** poruke. Ako je **DequeueCount** gore navedeni prag, aplikacija premješta poruku čekanja za definira aplikacija "dead slovo".

- Azure redova omogućuju vam da biste dobili detaljni zapis svih transakcija izvršeno protiv reda čekanja, kao i kao zbroja metriku. Obje mogućnosti korisne su za ispravljanje pogrešaka i objašnjenje kako aplikacija koristi redova Azure. Također su korisne za ugađanje performansi aplikacije i smanjenju troškova korištenja redova.

- Koncept "poruka sesije" podržava Bus servisa omogućuje poruke koje pripadaju određene logičke grupe biti povezane s navedeni primatelj čime se shodno sesiju nalik afinitet između poruke i njihov odgovarajući primatelja. Ovaj napredne funkcije u servis Bus možete omogućiti tako da svojstvo [ID sesije](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.sessionid.aspx) na poruku. Primatelje možete osluškuju ID određene sesije i primili poruku da je identifikator navedeni sesiju za zajedničko korištenje.

- Funkcija otkrivanje dupliciranje automatski podržava redova Bus servisa uklanja duplicirane poruke poslane na redu ili tema, na temelju vrijednosti svojstva [MessageID](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx) .

## <a name="capacity-and-quotas"></a>Kapacitet i kvotama

U ovom se odjeljku uspoređuju Azure redovima i usluge Bus redova iz perspektive [kapacitetu i kvote](service-bus-quotas.md) koje može primijeniti.

|Kriteriji uspoređivanja|Azure reda čekanja|Servis Bus reda čekanja|
|---|---|---|
|Veličina Maksimalna reda čekanja|**200 TB**<br/><br/>(ograničeno na jednom kapaciteta račun)|**1 GB 80 GB**<br/><br/>(definirani nakon stvaranja reda čekanja i [Omogućivanje particija](service-bus-partitioning.md) – pogledajte odjeljak "Dodatne informacije")|
|Najveće dopuštene veličine poruka|**64 KB**<br/><br/>(48 KB prilikom korištenja **Base64** kodiranje)<br/><br/>Azure podržava velike poruke kombiniranjem redova i blob-ova – čega možete enqueue do 200GB za jednu stavku.|**256 KB** ili **1 MB**<br/><br/>(uključujući zaglavlje i tijelo zaglavlja Maksimalna veličina: 64 KB).<br/><br/>Ovisi o [sloju servisa](service-bus-premium-messaging.md).|
|Maksimalna poruke TTL|**sedam dana**|**`TimeSpan.Max`**|
|Maksimalni broj reda čekanja|**Neograničeno**|**10 000**<br/><br/>(po polje naziva servisa, možete povećati)|
|Maksimalni broj Istodobni klijenti|**Neograničeno**|**Neograničeno**<br/><br/>(100 ograničenje Istodobni veze samo odnosi se na komunikacije utemeljen na protokol TCP)|

### <a name="additional-information"></a>Dodatne informacije

- Servis Bus primjenjuje ograničenja veličine red. Veličina Maksimalna reda čekanja navedena je nakon stvaranja reda čekanja, a mogu sadržavati vrijednost između 1 i 80 GB. Ako zbirka dostigne reda čekanja veličine vrijednost postavite na stvaranje reda čekanja će biti odbijene dodatne dolazne poruke i iznimku će primio pozivanja kod. Dodatne informacije o kvotama u servis Bus potražite u članku [Kvota Bus servisa](service-bus-quotas.md).

- Možete stvoriti redove Bus servisa u 1, 2, 3, 4 i 5 GB veličinama (zadano je 1 GB). S particija omogućeno (što je zadana postavka), servis Bus stvara 16 particije za svaki GB koju navedete. Kao takve, ako stvorite red čekanja koji je 5 GB po veličini, sa 16 particije veličinu Maksimalna reda čekanja postane (5 * 16) = 80 GB. Maksimalna veličina particioniranom reda čekanja ili temu možete vidjeti tako da pogledate na stavku [Azure portal][].

- Sa redovima Azure ako sadržaj poruke nije XML sigurnih, zatim mora biti **Base64** kodiran. Ako ste **Base64**-kodiranje poruku, opseg korisnik može biti do 48 KB, umjesto 64 KB.

- Sa redovima Bus servisa svake poruke spremljene u redu čekanja sastoji se od dva dijela: zaglavlje i tijelo. Ukupne veličine poruke ne može biti dulji od najveće dopuštene veličine poruka podržava sloja u sustavu.

- Kada klijenti komunikaciju sa servisa Bus redovima protokolom TCP, maksimalni broj Istodobni veza u jedan red Bus servisa ograničeno je na 100. Taj broj zajednički koriste između pošiljatelja i primatelja. Ako je dostigne ovaj kvote, daljnji zahtjevi za dodatne veze će biti odbijene i iznimku će primio pozivanja kod. To ograničenje nije zadana u klijentima povezati redova pomoću utemeljen na REST API-JA.

- Ako je potrebno više od 10 000 redova u jednom naziva Bus servisa, možete, obratite se timu za podršku za Azure i zatražite povećava. Da biste skalirali izvan 10 000 redova s Bus servisa, možete stvoriti i pomoću [portala za Azure][]dodatne prostora naziva.

## <a name="management-and-operations"></a>Upravljanje i operacije

U ovom se odjeljku uspoređuju značajke upravljanja nudi Azure redovima i usluge Bus redova.

|Kriteriji uspoređivanja|Azure reda čekanja|Servis Bus reda čekanja|
|---|---|---|
|Upravljanje protokola|**POKAZIVAČ HTTP/HTTPS**|**POKAZIVAČ HTTPS**|
|Protokol za vrijeme izvođenja|**POKAZIVAČ HTTP/HTTPS**|**POKAZIVAČ HTTPS**<br/><br/>**AMQP 1.0 Standard (TCP s TLS)**|
|Upravljani API za .NET|**Da**<br/><br/>(.NET upravljani API za pohranu klijenta)|**Da**<br/><br/>(.NET upravlja brokered razmjenu API-JA)|
|Izvorni C++|**Da**|**ne**|
|Java API|**Da**|**Da**|
|PHP API-JA|**Da**|**Da**|
|Node.js API-JA|**Da**|**Da**|
|Podrška za proizvoljne metapodataka|**Da**|**ne**|
|Pravila za imenovanje reda čekanja|**Najviše 63 znakova**<br/><br/>(Slova u nazivu reda čekanja mora biti mala slova.)|**Do 260 znakova**<br/><br/>(Putova reda čekanja i nazivi su velika i mala slova.)|
|Početak reda čekanja duljine (funkcija)|**Da**<br/><br/>(Približna vrijednost ako se poruke istječu izvan na TTL bez brisanja.)|**Da**<br/><br/>(Točno, točke u vrijeme vrijednost.)|
|Pogled (funkcija)|**Da**|**Da**|

### <a name="additional-information"></a>Dodatne informacije

- Azure redova nudi podršku za proizvoljne atributa koji se mogu primijeniti na opisa reda čekanja u obliku parova naziv/vrijednost.

- Oba tehnologije reda čekanja nudi mogućnost nakratko poruke bez potrebe za zaključavanje, koji mogu biti korisni kad implementacijom alat za preglednik/explorer reda čekanja.

- .NET Bus servisa brokered razmjenu API-ji leverage cijelog dvostrano TCP veza za bolje performanse pri usporedbi s OSTATKOM putem HTTP-a, a oni podržavaju AMQP 1.0 standardni protokol.

- Nazivi Azure redova može biti dugo 3 63 znakova, mogu sadržavati mala slova, brojeve i spojnice. Dodatne informacije potražite u članku [imenovanja redovima i metapodacima](https://msdn.microsoft.com/library/azure/dd179349.aspx).

- Bus servisa reda čekanja nazivi može biti najviše 260 znakova i imaju manje ograničenja pravila imenovanja. Nazivi reda čekanja Bus servisa može sadržavati slova, brojeve, razdoblja, rastavnice i podvlake.

## <a name="authentication-and-authorization"></a>Provjera autentičnosti i autorizacije

U ovom se odjeljku opisuje značajke provjere autentičnosti i autorizacije koje podržava Azure redovima i usluge Bus redova.

|Kriteriji uspoređivanja|Azure reda čekanja|Servis Bus reda čekanja|
|---|---|---|
|Provjera autentičnosti|**Simetričnu ključ**|**Simetričnu ključ**|
|Model sigurnosti|Delegirana pristup putem SAS tokena.|SAS|
|Vanjski pristup identitetu davatelja usluga|**ne**|**Da**|

### <a name="additional-information"></a>Dodatne informacije

- Svaki zahtjev za bilo koju od stavljanja tehnologije mora proći. Javni redovi s anonimnim pristupom nisu podržani. Korištenje [SAS](service-bus-sas-overview.md)se možete pozabaviti scenarij objavljivanjem SAS samo za pisanje, SAS samo za čitanje ili čak i potpuno pristup SAS.

- Shemu provjere autentičnosti koje ste dobili od redova Azure uključuje korištenje simetričnu tipke koja je u utemeljen na raspršivanje poruka provjere autentičnosti kod (HMAC), izračunata s algoritam SHA 256 i kodira kao niz **Base64** . Dodatne informacije o odgovarajući protocol potražite u članku [Provjera autentičnosti za Azure servise za pohranu](https://msdn.microsoft.com/library/azure/dd179428.aspx). Servis Bus redova podržava slične modela pomoću simetričnu tipki. Dodatne informacije potražite u članku [Zajedničko korištenje programa Access potpis Authentication s Bus servisa](service-bus-shared-access-signature-authentication.md).

## <a name="cost"></a>Trošak

U ovom se odjeljku uspoređuju Azure redovima i usluge Bus redova iz perspektive trošak.

|Kriteriji uspoređivanja|Azure reda čekanja|Servis Bus reda čekanja|
|---|---|---|
|Red čekanja trošak|**$0.0036**<br/><br/>(po 100 000 transakcije)|**Osnovni sloju**: **$0,05**<br/><br/>(po milijuna operacije)|
|Postupci za naplatu|**Sve**|**Slanje/primanje samo**<br/><br/>(to bez naknade za ostalih operacija)|
|Neaktivne transakcije|**Naplatu**<br/><br/>(Ispitivanje prazan red se broji kao naplatu transakcije.)|**Naplatu**<br/><br/>(Primanje na temelju prazan red se smatra naplatu poruke.)|
|Prostor za pohranu trošak|**$0.07**<br/><br/>(po GB/mjesec)|**0,00 kn**|
|Troškovi prijenosa izlaznog podataka|**$0.12 - $0.19**<br/><br/>(Ovisno o zemljopisnoj.)|**$0.12 - $0.19**<br/><br/>(Ovisno o zemljopisnoj.)|

### <a name="additional-information"></a>Dodatne informacije

- Prijenos podataka se naplaćuju ovisno o ukupnu količinu podataka ostavite Azure podatkovnim centrima putem Interneta u dano razdoblje naplate.

- Prijenos podataka između Azure servise koji se nalazi unutar iste područja ne podložne su trošak.

- Na ovom pisanje sve prijenosi ulaznih podataka nisu primjenjuju trošak.

- Da biste dobili podršku za dugačke ankete, pomoću servisa Bus redova može biti troškova učinkovitih u slučajevima u kojima je potrebno najniža Latencija isporuke.

>[AZURE.NOTE] Svi troškovi podložne su promjenama. U ovoj su tablici odražava trenutne cijene i ne obuhvaća promotivne ponude koje trenutno biti dostupne. Najnovije informacije o Azure cijene potražite u članku stranici [Azure cijene](https://azure.microsoft.com/pricing/) . Dodatne informacije o cijene servisa Bus potražite u članku [servis Bus cijene](https://azure.microsoft.com/pricing/details/service-bus/).

## <a name="conclusion"></a>Zaključak

Po pokušavaju dublju razumijevanja dva tehnologije moći da biste dodatno informirali odluku o tehnologiji reda čekanja koju želite koristiti, i kada. Odluku o kada koristiti Azure redova ili servis Bus redova jasno ovisi o nekoliko čimbenika. Sljedećih čimbenika intenzivnog možda ovise o potrebama vaše aplikacije i njegova arhitektura. Ako aplikacija već koristi core mogućnosti programa Microsoft Azure, bilo bi dobro da biste odabrali redove Azure, osobito ako tražite osnovni komunikacije i poruka između servisa ili potreba redove koje je moguće veće od 80 GB po veličini.

Jer je servis Bus redova navedite broj napredne značajke, kao što su sesije, transakcije, dvostruki automatsko otkrivanje mogućnosti rada lettering i durable objavljivanja/pretplate se možda biti željeni odabir ako izrađujete hibridnog aplikacije ili ako vaše aplikacije u suprotnom zahtijeva te značajke.

## <a name="next-steps"></a>Daljnji koraci

Sljedeći članci sadrže dodatne upute i informacije o korištenju Azure redova ili servis Bus redova.

- [Upute za korištenje servisa Bus redovi](service-bus-dotnet-get-started-with-queues.md)
- [Kako koristiti servis za pohranu reda čekanja](../storage/storage-dotnet-how-to-use-queues.md)
- [Praktični savjeti za korištenje servisa Bus poboljšanja performansi brokered razmjene poruka](service-bus-performance-improvements.md)
- [Uvod u redovima i teme u Azure Service Bus](http://www.code-magazine.com/article.aspx?quickid=1112041)
- [Vodič za razvojne inženjere za servis Bus](http://www.cloudcasts.net/devguide/Default.aspx?id=11030)
- [Putem servisa za stavljanja u Azure](http://www.developerfusion.com/article/120197/using-the-queuing-service-in-windows-azure/)
- [Objašnjenje Azure prostora za pohranu naplatu – propusnosti, transakcije i kapaciteta](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/07/09/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity.aspx)

[Portal za Azure]: https://portal.azure.com
 
