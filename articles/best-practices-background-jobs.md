<properties
   pageTitle="Smjernice za zadatke u pozadini | Microsoft Azure"
   description="Smjernice o pozadini zadataka te izvođenja neovisno korisničkog sučelja."
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
   ms.date="07/21/2016"
   ms.author="masashin"/>

# <a name="background-jobs-guidance"></a>Upute za zadatke pozadine

[AZURE.INCLUDE [pnp-header](../includes/guidance-pnp-header-include.md)]

## <a name="overview"></a>Pregled

Razne vrste aplikacije potreban je pozadinske zadatke koji se izvode neovisno o korisničko sučelje (UI). Primjeri obrade, intenzivno obrada zadataka i dugoročnih procese kao što su tijekovi rada. Pozadinske zadatke možete izvršiti bez interakcije s korisnikom – aplikacija možete pokretanje zadatka, a zatim nastaviti da obradi interaktivne zahtjeva za neke korisnike. To se može pomoći da biste minimizirali opterećenje aplikaciju korisničkog Sučelja koje možete poboljšati dostupnosti i kraće traje interaktivne odgovor.

Na primjer, po potrebi aplikacije da biste generirali minijature slika koje se prenose korisnici ga možete to kao posao u pozadini i spremiti minijaturu za pohranu kada je dovršena – bez korisniku koje je potrebno proći postupak izvršiti. Na isti način kao korisnik narudžba možete pokrenuti tijek rada pozadine koja obrađuje redoslijed, dok je korisničko Sučelje omogućuje korisniku da biste nastavili pregledavati web-aplikaciji. Nakon dovršetka posla pozadine, ažurirajte podatke pohranjene narudžbe možete se i poslati poruku e-pošte korisniku kojom se potvrđuje redoslijed.

Kada ste uzeti u obzir implementaciju zadatak kao posao u pozadini, kriterij glavni je li li zadatak možete pokrenuti bez interakcije s korisnikom i bez korisničkog Sučelja koje je potrebno proći obaviti posao. Zadaci koje je potrebno korisnika ili korisničkog Sučelja Pričekajte dok su napravljeni možda neće biti odgovarajuće kao pozadinske zadatke.

## <a name="types-of-background-jobs"></a>Vrste pozadinske zadatke

Pozadine poslovi obuhvaćaju obično jedan ili više sljedećih vrsta zadataka:

- Procesora ćete morati usko zadataka, kao što su matematičke izračune ili analize strukturnih modela.
- Li/O-ćete morati usko zadataka, kao što su izvršavanja niz transakcije prostora za pohranu ili Indeksiranje datoteke.
- Obrada, kao što su ažuriranja nightly podataka ili obrade.
- Dugoročnih tijekova rada, kao što su naloga ili usluge i sustavi za dodjelu resursa.
- Obrada osjetljivih podataka kojima se zadatak isporučeni sigurnije mjesto za obradu. Ako, na primjer, možda ne želite obrađivati osjetljive podatke iz web-aplikacijama. Umjesto toga možete koristiti uzorak kao što je [Upravitelj protoka informacija](http://msdn.microsoft.com/library/dn589793.aspx) za prijenos podataka proces Izolirani pozadine koja ima pristup zaštićene pohrane.

## <a name="triggers"></a>Okidača

Pozadinske zadatke može se inicirati na nekoliko različitih načina. Podijeljene su na jedan od sljedećih kategorija:

- [**Okidača utemeljenih na događaj**](#event-driven-triggers). Zadatak je pokrenut kao odgovor na događaj, obično akciju zauzima korisnika ili korak u tijeku rada.
- [**Okidača utemeljenih na raspored**](#schedule-driven-triggers). Zadatak se poziva na raspored na temelju vremena. To se može biti raspored koji se ponavlja ili jednokratni poziva koje je navedeno za kasnije.

### <a name="event-driven-triggers"></a>Utemeljenih na događaj okidača

Utemeljenih na događaj poziva koristi okidač da biste pokrenuli zadatak u pozadini. Primjeri korištenja okidača utemeljenih na događaj obuhvaćaju sljedeće:

- Korisničko Sučelje ili neki drugi zadatak smjestiti poruku u redu. Poruka sadrži podatke o akciju koja se događa, kao što su korisnički narudžba. Pozadinski zadatak očekuje podatke na ovom slijedu, a otkrije dolaska nove poruke. Pročita poruku, a koristi podatke u njemu kao ulaz posao u pozadini.
- Korisničko Sučelje ili neki drugi zadatak sprema ili ažurira vrijednost u prostor za pohranu. Zadatak u pozadini nadzire prostora za pohranu i otkrije promjene. Čita podatke i koristi kao unos za posao u pozadini.
- Korisničko Sučelje ili neki drugi zadatak čini zahtjeva za krajnje točke, kao što je URI HTTPS ili API-JA predstavljeni kao web-servisa. Ona prosljeđuje podatke koje je potrebno za dovršenje zadatka pozadine kao dio zahtjev. Krajnja točka ili web-servisa poziva zadatka pozadine koju koristi podatke kao njegov unos.

Uobičajeni zadaci prilagođenih utemeljenih na događaj poziva Primjeri slika obrade tijekova rada, slanje podataka o servise udaljene, slanje poruka e-pošte i dodjeljivanje nove korisnike u složene aplikacije.

### <a name="schedule-driven-triggers"></a>Utemeljenih na raspored okidača

Utemeljenih na raspored poziva koristi vremena da biste pokrenuli zadatak u pozadini. Primjeri korištenja okidača utemeljenih na raspored obuhvaćaju sljedeće:

- Mjerača koji se izvodi lokalno u aplikaciji ili kao dio aplikacije operacijski sustav poziva zadatak u pozadini redovito.
- Mjerača vremena koja se izvodi u neku drugu aplikaciju ili servis mjerača vremena kao što je raspored Azure šalje zahtjev API-JA ili web-servisa redovito. API-JA ili web-servisa poziva zadatak u pozadini.
- Pokreće se mjerača koji uzrokuje pozadinski zadatak treba pozvati jednom nakon određenog vremena stanke ili u određenom trenutku drugi proces ili aplikacije.

Uobičajeni zadaci prilagođenih utemeljenih na raspored poziva Primjeri skupna obrada postupke (kao što su ažuriranje popise povezane proizvode za korisnike koji se temelji na njihovo nedavne ponašanje), zadaci redovno obradu podataka (primjerice ažuriranje indeksa ili skupljena rezultata za generiranje), a zatim analiza podataka za dnevni izvješća, čišćenje zadržavanja podataka i provjera dosljednosti podataka.

Ako koristite utemeljenih na raspored zadataka koje morate pokrenuti kao jednokratan, imajte na umu sljedeće:

- Ako je neproporcionalno instancu računalnim sa sustavom raspored (kao što su virtualnog računala pomoću Windows troši), imat ćete više instanci raspored koji se izvodi. To se može pokrenuti više instanci zadatka.
- Ako zadaci se izvoditi dulje od razdoblja između Raspored događaja, zakazivanje osvježavanja može pokrenuti drugu instancu zadatka dok se izvodi na prethodni slajd.

## <a name="returning-results"></a>Daje rezultate

Pozadinske zadatke izvršiti asinkrono u drugi proces ili čak i u nekom drugom mjestu, korisničkog Sučelja ili postupka pozvati zadatak u pozadini. Najbolje pozadinske zadatke su "pokreću i zaboraviti" operacija, a tijek izvođenja sadrži bez utjecaja na korisničko Sučelje ili proces pozivanja. To znači da proces pozivanja čekanja za obavljanje zadataka. Stoga se ne može automatski prepoznati kada istekne zadatak.

Ako tražite pozadinski zadatak možete komunicirati s pozivanja zadataka da biste naznačili tijek ili dovršetka, morate provesti mehanizam za to. Primjeri su:

- Upišite vrijednost pokazatelja statusa za pohranu koji je dostupan zadatak korisničkog Sučelja ili pozivatelja, koji možete nadzirati ili pak potvrditi tu vrijednost po potrebi. Druge podatke kojima je zadatak u pozadini mora vratiti Pozivatelju se može smjestiti u isti prostor za pohranu.
- Uspostavljanje red čekanja za odgovor korisničkog Sučelja ili pozivatelja očekuje podatke. Zadatak pozadine poruke možete poslati red koji upućuju na stanje i završetka. Podaci koji se zadatak u pozadini mora vratiti Pozivatelju se može smjestiti u poruke. Ako koristite Bus servisa Azure, poslužite se svojstva **OutboundSMTPServer** i **CorrelationId** implementaciju tu mogućnost. Dodatne informacije potražite u članku [korelacije u servis Bus Brokered porukama](http://www.cloudcasts.net/devguide/Default.aspx?id=13029).
- Izlaganje API-JA ili krajnjoj točki iz pozadine zadatak koji je da biste dobili informacije o statusu mogu pristupiti korisničkog Sučelja ili pozivatelja. Podaci koji se zadatak u pozadini mora vratiti pozivatelju možete uvrstiti u odgovoru.
- Imate pozadinski zadatak pozvati korisničkog Sučelja ili pozivatelja kroz API za označavanje stanja na unaprijed definirane točke ili po dovršetku. To se može biti putem događaje potenciju lokalno ili putem mehanizam za objavljivanje – i – pretplata. Podaci koji se zadatak u pozadini mora vratiti pozivatelju budu obuhvaćeni opseg zahtjev ili događaj.

## <a name="hosting-environment"></a>Postavljeno okruženje

Možete hostirati pozadinske zadatke pomoću raspon različite platforme Azure servisi:

- [**Azure web-aplikacije i WebJobs**](#azure-web-apps-and-webjobs). Možete koristiti WebJobs izvršavanje prilagođene poslova utemeljenog na rasponu različitih vrsta skripte ili izvršni programi u kontekstu web-aplikacijama.
- [**Uloge servisa azure oblaka web i tempiranja**](#azure-cloud-services-web-and-worker-roles). Možete upisati kod u ulozi koja se izvršava kao zadatak u pozadini.
- [**Azure virtualnih računala**](#azure-virtual-machines). Ako ste servis Windows ili želite koristiti Raspored zadataka sustava Windows, uobičajeno je za hostiranje pozadinske zadatke unutar namjenski virtualnog računala.
- [**Grupe za azure**](./batch/batch-technical-overview.md). Je servis platformu koja Zakazuje računalnim ćete morati usko rada pokrenuti upravljani skup virtualnim strojevima i možete automatski skaliranje izračunati resursi potrebama svoje zadatke.

U sljedećim se odjeljcima opisuju svaki od tih mogućnosti više detalja, a uzeti da biste lakše odabrali odgovarajuću mogućnost.

## <a name="azure-web-apps-and-webjobs"></a>Azure web-aplikacije i WebJobs

Možete koristiti Azure WebJobs izvršavanje prilagođene poslova kao pozadinske zadatke unutar web-aplikacije programa Azure. WebJobs Pokreni u kontekstu web-aplikacije kao neprekidnog procesa. WebJobs i pokrenuti odgovor na događaj okidača Azure raspored ili vanjski čimbenici, kao što su promjene blob polja za pohranu i redovima poruka. Zadacima možete biti rada zaustavio na zahtjev i isključiti bez poteškoća. Ako stalno izvodi WebJob ne uspije, automatski ponovnog pokretanja. Pokušajte ponovno i pogreškama akcije su konfigurirati.

Kada konfigurirate na WebJob:

- Ako želite da se zadatak odgovoriti okidača utemeljenih na događaj, potrebno je konfigurirati kao **neprestano**. Skripte ili programa pohranjuju se u mapu pod nazivom web-mjesta/wwwroot/app_data/poslove/neprekinuti.
- Ako želite da se zadatak odgovoriti okidač utemeljenih na raspored, potrebno je konfigurirati kao **Pokreni prema rasporedu**. Skripte ili programa pohranjuju se u mapu pod nazivom web-mjesta/wwwroot/app_data/poslove/pokrenuo.
- Ako odaberete mogućnost **pokretanje na zahtjev** prilikom konfiguriranja posla, izvršiti će istu šifru kao mogućnost za **pokretanje na raspored** prilikom pokretanja.

Azure WebJobs pokrenuti u memoriji za testiranje web-aplikacije. To znači da mogu pristupati varijable okruženja i zajedničko korištenje informacija, kao što su nizu za povezivanje s web-aplikaciji. Posao ima pristup Jedinstveni identifikator na računalu sa sustavom posao. Niz za povezivanje s nazivom **AzureWebJobsStorage** omogućuje pristup redovima Azure prostora za pohranu, blob-ova i tablice za aplikaciju podatke i pristup Bus servisa za razmjenu poruka i komunikaciju. Niz za povezivanje s nazivom **AzureWebJobsDashboard** omogućuje pristup datoteke zapisnika posao akcija.

Azure WebJobs sa sljedećim karakteristikama:

- **Sigurnost**: WebJobs zaštićeni su vjerodajnice za implementaciju web-aplikacije.
- **Podržane vrste datoteka**: možete definirati WebJobs pomoću naredbe skripte (.cmd), datoteka grupe (.bat), a zatim skripte komponente PowerShell (.ps1), tulum skripti ljuske (.sh), skripte i (.php), Python skripte (.py), JavaScript kod (.js) i izvršne datoteke programa (.exe, .jar i više).
- **Uvođenje**: možete implementirati skripte i izvršne datoteke pomoću portala za Azure pomoću dodatka [WebJobsVs](https://visualstudiogallery.msdn.microsoft.com/f4824551-2660-4afa-aba1-1fcc1673c3d0) za Visual Studio ili u [Visual Studio 2013 ažuriranje 4](http://www.visualstudio.com/news/vs2013-update4-rc-vs) (možete stvoriti i implementirati uz tu se mogućnost), pomoću [Azure WebJobs SDK](./app-service-web/websites-dotnet-webjobs-sdk-get-started.md)ili ih kopirajte izravno na sljedećim mjestima:
  - Za pokrene izvođenja: web-mjesta/wwwroot/app_data/poslove/pokrene / {zadatak naziv}
  - Za izvršavanje neprekinuti: web-mjesta/wwwroot/app_data/poslove/neprekinuti / {zadatak naziv}
- **Zapisivanje**: Console.Out se smatra (označenu) informacije. Pogreška se smatra Console.Error. Nadzor i dijagnostici informacije mogu pristupati pomoću portala za Azure. Zapisničke datoteke možete preuzeti izravno s web-mjesta. Spremaju se na sljedećim mjestima:
  - Za pokrene izvođenja: Vfs/podataka/poslove/pokrene/jobName
  - Za izvršavanje neprekinuti: Vfs/podataka/poslove/neprekinuti/jobName
- **Konfiguriranje**: možete konfigurirati WebJobs pomoću portala, REST API-JA i PowerShell. Konfiguracijska datoteka pod nazivom settings.job u korijenskom direktoriju isti kao skripte posao možete koristiti možete unijeti informacije o konfiguraciji za posao. Ako, na primjer:
  - {"stopping_wait_time": 60}
  - {"is_singleton": true}

### <a name="considerations"></a>Razmatranja

- Po zadanome, WebJobs Skaliranje s web-aplikaciji. Međutim, možete konfigurirati zadataka da biste pokrenuli na instancu tako da svojstvo **is_singleton** konfiguracije na **true**. Instancu WebJobs korisne su za zadatke koje ne želite skaliranja i pokrenuti kao istodobno više instanci, kao što je li analizu podataka te slične zadatke.
- Da biste minimizirali utjecaj zadataka na performanse web-aplikacije, razmislite o stvaranju prazan instance Azure web-aplikacije u novoj tarifi aplikacije servisa za glavno računalo WebJobs koji mogu biti dugo izvodi ili resurs intenzivno.

### <a name="more-information"></a>Dodatne informacije

- [Azure WebJobs preporučuje resursi](./app-service-web/websites-webjobs-resources.md) popise mnoge korisne resurse, preuzimanja i primjere za WebJobs.

## <a name="azure-cloud-services-web-and-worker-roles"></a>Azure servise u Oblaku web- a tempiranja uloge

Možete izvršiti pozadinske zadatke unutar ulogu web ili u zasebnom tempiranja uloga. Kada odlučujete želite li koristiti ulogu suradnika, razmislite o skalabilnost i preduvjeti elasticity, vijek trajanja zadatka, pustite cadence, sigurnost, odstupanje kvara, Nadmetanje, složenosti i logički arhitektura. Dodatne informacije potražite u članku [Izračunati uzorak konsolidacije resursa](http://msdn.microsoft.com/library/dn589778.aspx).

Implementacija pozadinske zadatke unutar uloge servisa u Oblaku na nekoliko načina:

- Stvorite implementacija klase **RoleEntryPoint** u ulozi i koristite postupke za izvršavanje pozadinske zadatke. Zadaci koje ćete pokrenuti u kontekstu WaIISHost.exe. Da biste učitali konfiguracijske postavke mogu koristiti metodu **GetSetting** klase **CloudConfigurationManager** . Dodatne informacije potražite u članku [životni ciklus (servise u Oblaku)](#lifecycle-cloud-services).
- Pomoću zadataka pokretanje izvoditi pozadinske zadatke prilikom pokretanja aplikacije. Da biste nametnuli zadataka da biste nastavili raditi u pozadini, postavite svojstvo **taskType** **pozadinu** (Ako to ne učinite, postupka pokretanja aplikacije će zaustavili i pričekajte da zadatak da biste dovršili). Dodatne informacije potražite u članku [pokretanje zadaci prilikom pokretanja u Azure](./cloud-services/cloud-services-startup-tasks.md).
- Pomoću WebJobs SDK implementirati pozadinske zadatke kao što su WebJobs koji su pokrenuti zadatak pokretanja. Dodatne informacije potražite u članku [Početak rada s Azure WebJobs SDK](./app-service-web/websites-dotnet-webjobs-sdk-get-started.md).
- Instalirajte servis Windows koji se izvršava pozadinske zadatke pomoću zadatak pokretanja. Morate postavite svojstvo **taskType** **pozadinu** tako da je servis izvršava u pozadini. Dodatne informacije potražite u članku [pokretanje zadaci prilikom pokretanja u Azure](./cloud-services/cloud-services-startup-tasks.md).

### <a name="running-background-tasks-in-the-web-role"></a>Pozadine zadataka koji se izvode u ulozi web

Glavni prednost pozadine zadataka koji se izvode u ulozi web je spremanja u hostiranja troškove jer postoji bez zahtjeva za implementaciju dodatne uloge.

### <a name="running-background-tasks-in-a-worker-role"></a>Pokretanje pozadinske zadatke u ulogu suradnika

Pozadine zadataka koji se izvode u ulogu suradnika ima nekoliko prednosti:

- Omogućuje upravljanje skaliranje zasebno za svaku vrstu uloge. Na primjer, možda će biti potrebno više instanci web uloge za podršku trenutno opterećenje, ali manje pojavljivanja ulogu suradnika koji se izvršava pozadinske zadatke. Ako skaliranje pozadine zadatka računalnim instance odvojeno od uloge korisničkog Sučelja, možete smanjiti hosting troškova uz zadržavanje prihvatljivi performansi.
- Ga offloads indirektni obrada za pozadinske zadatke iz uloge web. Uloga web koji omogućuje korisničkog Sučelja može ostati odredište, a može značiti manje instanci su im potrebne za podršku navedeni količinu zahtjeva za neke korisnike.
- Omogućuje implementirati odvojenosti opasnosti. Svaka vrsta uloga možete implementirati određeni skup jasno definirani i povezanih zadataka. Ovime dizajniranje i održavanje kod jednostavnije jer postoji manje međusobnu ovisnost Šifra i funkcionalnosti između svaku ulogu.
- Pomoću nje možete izdvojiti osjetljive procesa i podatke. Ako, na primjer, web uloge koje implementirati korisničkog Sučelja ne moraju imati pristup podacima koji upravlja i upravljaju ulogu suradnika. To može biti korisno u strengthening sigurnosti, osobito kada koristite uzorak kao što je s [Uzorak Upravitelj protoka informacija](http://msdn.microsoft.com/library/dn589793.aspx).  

### <a name="considerations"></a>Razmatranja

Imajte na umu sljedeće prilikom odabira gdje i kako implementirati pozadinske zadatke prilikom korištenja web- a tempiranja uloge servisa u Oblaku:

- Hostiranje pozadinske zadatke u postojeću web ulogu možete spremiti trošak izvodi u zasebnom radnih ulogu samo za sljedeće zadatke. Međutim, vjerojatno je utjecati na performanse i dostupnost aplikacije ako postoji Nadmetanje za obradu i druge resurse. Uloga web korištenjem zasebnom tempiranja uloge štiti od utjecaj dugoročnih ili resursa ćete morati usko pozadinske zadatke.
- Ako pak hostirate pozadinske zadatke pomoću **RoleEntryPoint** predmete, jednostavno to možete premjestiti u drugu ulogu. Na primjer, ako stvorite klasa u ulozi web, a kasnije odlučite da morate pokrenuti zadataka u ulogu suradnika, možete premjestiti implementaciju klase **RoleEntryPoint** u ulogu suradnika.
- Pokretanje zadataka osmišljeni su za izvođenje programa ili skriptu. Implementacija posao u pozadini kao izvršne program možda teže, osobito ako je potrebno i implementaciju zavisne sklopova. Možda je lakše uvesti i koristiti skriptu da biste definirali posao u pozadini prilikom korištenja pokretanja zadataka.
- Iznimke zadatak u pozadini uvoza imaju različite utjecaj, ovisno o tome na način na koji se nalaze:
  - Ako koristite predmete pristup **RoleEntryPoint** , nije uspjelo zadatka uzrokovat će uloga da biste ponovno pokrenuli tako da se automatski pokreće zadatak. To može utjecati na dostupnost aplikacije. Da biste spriječili, provjerite je li uvrstite robusne iznimku rukovanje unutar klasu **RoleEntryPoint** i sve pozadinske zadatke. Pomoću koda da biste ponovno pokrenuli zadatke koje ne prođu gdje je to odgovarajući i vratiti iznimku da biste ponovno pokrenuli ulogu samo ako bez poteškoća ne mogu oporaviti od neuspjeh unutar.
  - Ako koristite prilikom pokretanja zadataka, odgovorni ste za upravljanje izvršavanje zadataka i provjera ne uspijeva.
- Upravljanje i praćenje zadataka pokretanje teže od korištenja pristup klase **RoleEntryPoint** . Međutim, Azure WebJobs SDK sadrži nadzorne ploče da biste olakšali upravljanje WebJobs pokrenete kroz zadatke za pokretanje.

### <a name="more-information"></a>Dodatne informacije

- [Uzorak konsolidacije resursa za izračun](http://msdn.microsoft.com/library/dn589778.aspx)
- [Početak rada s Azure WebJobs SDK](./app-service-web/websites-dotnet-webjobs-sdk-get-started.md)

## <a name="azure-virtual-machines"></a>Azure virtualnim strojevima

Pozadinske zadatke može primijeniti na način na koji ih onemogućuje uvodi Azure web-aplikacije ili servisa u Oblaku ili tih mogućnosti možda neće biti praktičan. Uobičajeni primjeri su Windows services i uslužni programi drugih proizvođača i izvršne datoteke programa. Drugi primjer možda programa napisanih za okruženju izvođenja koji se razlikuje od onog koji se nalaze aplikacije. Na primjer, možda Unix ili Linux program koji želite izvesti iz aplikacije za Windows ili .NET. Odaberite raspon operacijski sustavi za Azure virtualnog računala i pokretanje servisa ili izvršne datoteke na tom virtualnog računala.

Da biste lakše odabrali korištenje virtualnih računala, potražite u članku [servisa Azure aplikaciju, servise u Oblaku i virtualnim strojevima usporedbe](./app-service-web/choose-web-site-cloud-service-vm.md). Informacije o mogućnostima virtualnim računalima potražite u članku [veličine virtualnog računala i u Oblaku za Azure](http://msdn.microsoft.com/library/azure/dn197896.aspx). Dodatne informacije o operacijskim sustavima i ugrađene slike koje su dostupne za virtualnim računalima potražite u članku [Trgovine virtualnim računalima sustava Windows Azure](https://azure.microsoft.com/gallery/virtual-machines/).

Da biste započeli pozadine zadatka u zasebnom virtualnog računala, imate raspon mogućnosti:

- Zadatak možete izvršiti na zahtjev izravno iz aplikacije slanjem zahtjeva za krajnje izlaže zadatak. Prosljeđuje se u sve podatke koje je potrebno za zadatak. U ovom krajnje točke poziva zadatak.
- Možete konfigurirati pokretanje prema rasporedu pomoću rasporeda ili mjerača vremena koja je dostupna u odabranom operacijski sustav zadatka. Na primjer, u sustavu Windows koristite raspored zadataka sustava Windows za izvršavanje skripte i zadatke. Ili ako imate SQL Server na virtualnim računalu instaliran, možete koristiti agenta za SQL Server za izvršavanje skripte i zadatke.
- Raspored Azure možete koristiti da biste započeli zadatak dodavanjem poruke čekanja za zadatak očekuje podatke ili slanjem zahtjeva za API-JA koji se izlaže zadatak.

U odjeljku starijim [okidača](#triggers) dodatne informacije o načinu ručnom pokretanju pozadinske zadatke.  

### <a name="considerations"></a>Razmatranja

Kada odlučujete želite li implementirati pozadinske zadatke u Azure virtualnog računala Imajte na umu sljedeće:

- Hostiranje pozadinske zadatke u zasebnom Azure virtualnog računala pruža fleksibilnost web-mjesta i omogućuje preciznije kontrolirati pokretanje, izvođenja, raspored i dodjela resursa. Međutim, izvođenja trošak je povećava ako virtualnog računala mora biti implementirano samo da biste pokrenuli pozadinske zadatke.
- Postoji bez funkcijom praćenje zadataka u Azure portal i nema mogućnost automatskog ponovnog pokretanja za nije uspjelo zadatke – Iako možete pratiti osnovni stanje virtualnog računala i upravljati korištenjem [Cmdleti za upravljanje servisom za Azure](http://msdn.microsoft.com/library/azure/dn495240.aspx). No postoje bez funkcije da biste odredili procesa i niti u računalnim čvorove. Obično pomoću virtualnog računala, potrebna dodatna trud implementaciju mehanizam koji prikuplja podatke iz instrumentation u zadatku i operacijskog sustava u virtualnog računala. Jedan rješenje koje možda odgovarajuće je koristiti [Sustav centar paket za upravljanje Azure](http://technet.microsoft.com/library/gg276383.aspx).
- Razmislite o stvaranju nadzora probes prikazat će se putem HTTP krajnje točke. Kod za ove probes nije moguće izvršiti provjeru stanja, Prikupite informacije i Statistika – ili razvrstati podatke o pogreškama i vratite tekst na aplikacija za upravljanje. Dodatne informacije potražite u članku [Stanja nadzor uzorak krajnjoj točki](http://msdn.microsoft.com/library/dn589789.aspx).

### <a name="more-information"></a>Dodatne informacije

- [Virtualnim strojevima](https://azure.microsoft.com/services/virtual-machines/) na Azure
- [Najčešća pitanja vezana uz Azure virtualnim strojevima](virtual-machines/virtual-machines-linux-classic-faq.md)

## <a name="design-considerations"></a>Zahtjevi dizajna

Postoji nekoliko čimbenika temeljne treba uzeti u obzir prilikom dizajniranja pozadinske zadatke. U sljedećim se odjeljcima navode particija, sukoba i koordinaciji.

## <a name="partitioning"></a>Particija

Ako odlučite da biste uključili pozadinske zadatke unutar postojeće računalnim instancu (kao što je web-aplikacije, uloga web, postojeće ulogu suradnika, ili virtualnog računala), morate uzeti u obzir kako će to utjecati atribute kvalitete instancu računalnim i pozadinski zadatak sam. Sljedećih čimbenika će vam pomoći da odlučite hoćete li colocate zadataka s postojećom instancom računalnim ili odvojite ih u zasebnom računalnim instancu:

- **Dostupnost**: pozadinske zadatke možda ne moraju imati istu razinu dostupnost kao ostali dijelovi aplikacije, posebice korisničkog Sučelja i druge dijelove koje slijede izravno u interakcije s korisnikom. Pozadinske zadatke možda fleksibilnije latencije, pogreške koje je postupak ponovljen veze i druge čimbenike te utječu na dostupnost jer operacije može biti u redu čekanja. Međutim, mora postojati dovoljno kapaciteta da biste spriječili sigurnosnu kopiju zahtjevi za koje nije blokirati redovima i utječu na aplikaciju kao cjelinu.
- **Skalabilnost**: pozadinske zadatke vjerojatno imaju različite skalabilnost preduvjet od korisničkog Sučelja i interaktivni dijelovi aplikacije. Skaliranje korisničko Sučelje možda biti potrebno da bi odgovarao peaks u zahtjev, dok je preostala pozadinske zadatke mogu dovršiti tijekom manje vremena na manji broj broj instanci računalnim.
- **Otpornost**: pogreška računalnim instance koji upravo hostira pozadinske zadatke možda fatally utječe na aplikaciju cijela ako zahtjeva za sljedeće zadatke možete u redu čekanja ili odgođena dok je zadatak dostupna ponovno. Ako računalnim instancu i/ili zadaci možete ponovno pokrenuti unutar odgovarajuće intervala, možda neće utjecati korisnici aplikacije.
- **Sigurnost**: pozadinske zadatke možda imaju različite sigurnosne preduvjete ili ograničenja od korisničkog Sučelja ili druge dijelove aplikacija. Pomoću zasebnom računalnim instance možete odrediti u drugoj sigurnosnoj okruženje za zadatke. Uzorci kao što je upravitelj protoka informacija možete koristiti i izdvojiti instance računalnim pozadine u korisničkom Sučelju kako biste maksimizirali sigurnost i odvojenosti.
- **Performanse**: možete odabrati vrstu računalnim instance za pozadinu zadaci posebno podudaranje preduvjeti performanse zadaci. Možda će to znači pomoću manje skupi računalnim mogućnost zadaci moraju istih mogućnosti obrade kao korisničkog Sučelja ili veća instancu ako trebaju dodatne kapaciteta i resursa.
- **Mogućnost upravljanja**: pozadinske zadatke možda imaju različite razvoja i implementaciju rhythm iz glavnog računala kod ili korisničkog Sučelja. Implementacija ih na zasebnom računalnim instancu može pojednostavniti ažuriranja i rad s verzijama.
- **Trošak**: dodavanje izračunati instance izvršiti pozadinske zadatke povećava hostiranje troškove. Trebali biste pažljivo trade-off između dodatne kapaciteta i te dodatnih troškova.

Dodatne informacije potražite u članku [Uzorak izborno Vodeća crta](http://msdn.microsoft.com/library/dn568104.aspx) i [Natječu uzorak koje korisnici](http://msdn.microsoft.com/library/dn568101.aspx).

## <a name="conflicts"></a>U sukobu

Ako imate više instanci posao u pozadini, moguće je da oni će se natječu za pristup resurse i servise, kao što je baza podataka i prostora za pohranu. Istodobni pristup može rezultirati Nadmetanje resursa koji mogu uzrokovati sukobe u dostupnost usluge i integriteta podataka u prostor za pohranu. Mogli biste riješiti Nadmetanje resursa pomoću pesimistična Procjena zaključavanja pristup. To sprječava natječu pojavljivanja zadatka iz istovremeno pristupa na servisu ili oštećenje podataka.

Drugi način za rješavanje sukoba je da biste definirali pozadinske zadatke kao jednočlana, tako da postoji ikad samo jednu instancu pokrenut. Međutim, uklanja pouzdanost i performanse pogodnosti koje možete unijeti više instanci konfiguracije. Ovo je posebno točno korisničko Sučelje možete navesti dovoljno rad da biste zadržali više zadataka pozadine zauzet.

To je ključni da bi se zadatak u pozadini automatski možete započeti i ima li dovoljno kapacitet cope s peaks u zahtjev. To možete postići tako da dodjeljivanje računalnim instanca s dovoljno resursa, implementacijom mehanizam reda čekanja koji se mogu pohraniti zahtjeva za kasnije izvršenje kada smanjuje zahtjev ili pomoću kombinacija ove tehnike.

## <a name="coordination"></a>Koordinaciji

Pozadinske zadatke možda složene i možda će biti potrebno više pojedine zadatke za izvršavanje da biste dobili rezultat ili fulfil sve uvjete. U sljedećim scenarijima podjele zadatak u manje discreet koraka ili podzadataka koji se može izvršavati tako da korisnici više uobičajeno je. Multistep poslove može biti učinkovitiji i čine je fleksibilnijom jer pojedinačne koraci se mogu ponovno iskoristiv u više zadataka. I jednostavno je za dodavanje, uklanjanje ili izmjena redoslijeda korake.

Koordinaciju više zadacima i koracima može biti izazov, ali postoje tri uobičajene uzorke koje možete koristiti da biste vodič za implementaciju rješenja:

- **Decomposing zadataka u više ponovno iskoristiv korake**. Aplikacija se možda potrebne za izvršavanje različitih zadataka različitim složenosti na željene informacije obrađuje. Jednostavne, ali nepomično pristup implementacijom ove aplikacije mogu se da biste izvršili ovaj obrada kao monolithic modul. Međutim, ovaj pristup je vjerojatno da biste smanjili prilika za restrukturiranje kod, optimizirajući ga ili ga ponovno korištenje ako dijelove isti obrada negdje drugdje u aplikaciji. Dodatne informacije potražite u članku [kanali i uzorak filtre](http://msdn.microsoft.com/library/dn568100.aspx).
- **Upravljanje izvođenja koraka za zadatak**. Aplikacije sustava mogu obavljati zadatke koje čine broj koraka (neke od kojih možda pozivanje servise udaljene ili pristup udaljenim resursima). Pojedinačne koraci se mogu međusobno nezavisni, no oni su orchestrated po logike aplikacije koji implementira zadatak. Dodatne informacije potražite u članku [Raspored Agent nadzornik uzorka](http://msdn.microsoft.com/library/dn589780.aspx).
- **Upravljanje oporavak upute za zadatak koji se neće**. Aplikacija možda će biti potrebno da biste poništili rada koji se izvodi pomoću niz koraka (koji zajedno definirali naposljetku dosljedan operaciju) ako je jedan ili više od koraka neće uspjeti. Dodatne informacije potražite u članku [Kompenziranje uzorak transakcije](http://msdn.microsoft.com/library/dn589804.aspx).

## <a name="lifecycle-cloud-services"></a>Životni ciklus (servise u Oblaku)

 Ako odlučite implementirati pozadinske zadatke za servise u Oblaku aplikacije koje koriste web- a tempiranja uloge pomoću klase **RoleEntryPoint** , važno je da biste shvatili životnim ciklusom klase da bi se ispravno ga koristiti.

Uloge web i tempiranja proći kroz skup različite faze, kao što su pokretanje, pokretanje i zaustavljanje. Klase **RoleEntryPoint** izlaže niz događaja koje označavaju kada te su faze prikazane su koje su se pojavile. Se koriste za pokretanje, pokretanje, i zaustavljanje svoje prilagođene pozadinske zadatke. Dovršavanje ciklusa je:

- Azure učitava u sklopu uloga i pretražuje predmet koja je izvedena iz **RoleEntryPoint**.
- Ako se pronađe klase, poziva **RoleEntryPoint.OnStart()**. Zanemarivanje ovu metodu Inicijalizacija pozadinske zadatke.
- Po dovršetku metodu **OnStart** Azure pozive **Application_Start()** u aplikacije Globalna datoteka ako to je prisutnih (na primjer, Global.asax u ulogu web izvodi ASP.NET).
- Azure poziva **RoleEntryPoint.Run()** na novu niti planu koja izvršava paralelno s **OnStart()**. Zanemarivanje ovu metodu da biste pokrenuli pozadinske zadatke.
- Kada istekne korištenje način pokretanja, Azure najprije poziva **Application_End()** u Globalna datoteka aplikacije ako to postoji, a zatim poziva **RoleEntryPoint.OnStop()**. Zanemarivanje metode **OnStop** Zaustavi pozadinske zadatke, čišćenja resursa, rashoda objekata i zatvorite veze koje se zadaci koje ćete možda koristili.
- Azure tempiranja uloga matični proces je zaustavljen. U ovom trenutku ulogu neće biti brisanja i će se pokrenuti.

Dodatne informacije i primjer pomoću metode klase **RoleEntryPoint** potražite u članku [Izračunati uzorak konsolidacije resursa](http://msdn.microsoft.com/library/dn589778.aspx).

## <a name="considerations"></a>Razmatranja

Prilikom planiranja kako će izvoditi pozadinske zadatke u ulozi weba ili tempiranja Imajte na umu sljedeće:

- **Pokretanje** načina zadan u razredu **RoleEntryPoint** sadrži poziv **Thread.Sleep(Timeout.Infinite)** zadržava ulogu aktivnosti beskonačno. Ako nadjačali način **pokrenuti** (što je obično potrebne za izvršavanje pozadinske zadatke), ne morate omogućiti kod da biste izašli iz metodu osim ako ne želite koša za instancu uloge.
- Uobičajeni implementaciju načina za **pokretanje** sadrži kod da biste pokrenuli svako pozadinske zadatke i konstrukta petlja koji se povremeno provjerava stanje sve pozadinske zadatke. Možete započeti bilo koji se neće ili praćenje za otkazivanje tokeni koje označavaju dovršite zadatke.
- Ako je zadatak u pozadini throws neobrađenu iznimku, taj zadatak mora biti brisanja istovremeni pozadinske zadatke u sklopu uloge za nastavak rada. Međutim, ako iznimku uzrokuje oštećenje objekte izvan zadatka, kao što je zajednički prostor za pohranu, iznimka se obrađuje svojoj učionici **RoleEntryPoint** , svi zadaci moraju biti poništeni te način **pokrenuti** smije da biste završili. Azure pa ponovno pokrenite ulogu.
- Upotrijebite metodu **OnStop** zaustaviti ili Ukloni pozadinske zadatke i čišćenja resursi. To može obuhvaćati zaustavljanje dugoročnih ili multistep zadataka. Je ključan treba uzeti u obzir kako to možete učiniti da biste izbjegli nedosljednosti podataka. U slučaju uloga instance iz bilo kojeg razloga osim isključivanja za korisnički pokrenut, kod koji se izvodi u načinu **OnStop** morate izvršiti unutar pet minuta prije prisilno prekida. Provjerite je li kod možete obaviti u tom vremenu, ili tolerate nije pokrenut do kraja.  
- Azure opterećenja pokreće koja usmjeruje promet na instancu ulogu kada metodu **RoleEntryPoint.OnStart** vraća se vrijednost **true**. Stoga preporučujemo da umetanja sav kod za pokretanje u način **OnStart** tako da se instance uloge koje uspješno pokrenuti će primiti sve promet.
- Možete koristiti prilikom pokretanja zadaci osim metode klasu **RoleEntryPoint** . Koristite zadaci prilikom pokretanja sve postavke koje ćete morati promijeniti Azure opterećenja jer zadaci će se izvoditi prije ulogu prima sve zahtjeve za pokretanje. Dodatne informacije potražite u članku [pokretanje zadaci prilikom pokretanja u Azure](./cloud-services/cloud-services-startup-tasks.md).
- Ako je zadatak pokretanja pogrešku, možda prisilno uloga ponovno neprestano. To može spriječiti izvođenje virtualne zamjena adresa IP (VIP) povratak na prethodno postupnu verzije jer se zamjena zahtijeva isključivi pristup ulogu. To nije moguć dok je ponovno pokrenuti ulogu. Da biste riješili taj problem:
    -  Dodajte sljedeći kôd početak načine **OnStart** i **pokretanje** u vaša uloga:

    ```C#
    var freeze = CloudConfigurationManager.GetSetting("Freeze");
    if (freeze != null)
    {
        if (Boolean.Parse(freeze))
        {
            Thread.Sleep(System.Threading.Timeout.Infinite);
        }
    }
    ```

   - Dodajte definicija postavku **Zamrzavanje** kao Booleova vrijednost ServiceDefinition.csdef i ServiceConfiguration. *.cscfg datoteka za ulogu i postavite ga na* *False* *. Ako ulogu prelazi u način rada koji se ponavljaju ponovno pokretanje računala, možete promijeniti postavku * *true** Zamrzavanje izvođenja uloga i dopustiti da bi se zamjenjuju u starijoj verziji.

## <a name="resiliency-considerations"></a>Razmatranja otpornost

Pozadinske zadatke mora biti prebacuju da bi se pouzdanog usluge aplikaciji. Kada su planiranje i dizajniranje pozadinske zadatke, imajte na umu sljedeće:

- Pozadinske zadatke moraju imati mogućnost bez poteškoća rukovati uloga ili servis ponovnog pokretanja bez oštećenje podataka ili Uvod nedosljednosti u aplikaciju. Za dugoročnih ili multistep zadataka, preporučujemo da koristite _provjerite pokazivanjem_ spremanjem u stanje zadataka u stalnih prostor za pohranu ili kao poruke u redu čekanja ako je to prikladno. Ako, na primjer, zadržava informacije o stanju u poruci u redu čekanja i postupno ažurirati ovaj informacije o stanju tijek zadatka tako da se zadatak može obraditi iz zadnjeg poznati dobar Kontrolna točka – umjesto ponovnog pokretanja od početka. Prilikom korištenja redova Bus servisa Azure sesije poruka možete koristiti da biste omogućili isti scenarij. Sesije omogućuju da biste spremili i dohvatiti obrada stanja računala pomoću metode [SetState](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagesession.setstate.aspx) i [GetState](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagesession.getstate.aspx) . Dodatne informacije o dizajniranju pouzdanog multistep procesa i tijekova rada potražite u članku [Raspored Agent nadzornik uzorka](http://msdn.microsoft.com/library/dn589780.aspx).
- Kada koristite weba ili tempiranja uloge za hostiranje više pozadinske zadatke, Dizajnirajte svoje nadjačavanje metode **pokrenite** praćenje za nije uspjelo ili Neaktivni zadaci, a zatim ih ponovno pokrenuti. Gdje je to nije praktično i koristite ulogu suradnika, obavezno ulogu suradnika da biste ponovno pokrenuli po izići iz način **pokrenuti** .
- Kada koristite redova možete komunicirati s pozadinske zadatke redova može poslužiti kao međuspremnika pohraniti zahtjevi za koje se šalju zadaci dok je aplikacija je u odjeljku veće od uobičajeni Učitaj. Time se omogućuje zadatke za provjeru s korisničkog Sučelja tijekom manje zauzet razdoblja. Također znači da recikliranje ulogu neće blokirati korisničkog Sučelja. Dodatne informacije potražite u članku [utemeljen na red uzorak Leveling Učitaj](http://msdn.microsoft.com/library/dn589783.aspx). Ako su neki zadaci važnija od ostalih, razmislite o implementacijom [Uzorak prioritet reda čekanja](http://msdn.microsoft.com/library/dn589794.aspx) da biste bili sigurni prije nego što manje važna one pokrenuti zadaci.
- Pozadinske zadatke koji su pokrenuo poruke ili poruke postupak mora biti dizajnirane za rukovanje nedosljednosti, kao što su poruke stiže izvan redoslijeda, poruke koje više puta uzrokovati pogreške (često se nazivaju _poison poruke_) i poruke koje se isporučuju više puta. Imajte na umu sljedeće:
  - Poruke koje se moraju obraditi određenim redoslijedom, kao što su oni koje se mijenjaju podatke na temelju postojeće vrijednost data (na primjer, dodavanje vrijednosti u postojećoj vrijednosti), možda ne stiže u izvorni redoslijed kojim su poslane. Osim toga, oni možda se obrađuje različite instance zadatka pozadine u različitom redoslijedu zbog različitim opterećenje na svaku instancu. Poruke koje se moraju obraditi određenim redoslijedom mora sadržavati redni broj, ključ ili neke druge pokazatelj koji pozadinske zadatke koje možete koristiti da biste bili sigurni da se obrađuju ispravnim redoslijedom. Ako koristite Bus servisa Azure, pomoću poruka sesije jamči redoslijed isporuke. Međutim, je obično učinkovitiji, gdje je to moguće, za dizajniranje postupka tako da redoslijed poruku nije važno.
  - Obično zadatak u pozadini će pogled na poruke u redu čekanja koja ih privremeno skriva iz druge poruke koje korisnici. Zatim briše poruke kada su uspješno obrade. Ako je zadatak u pozadini neuspješan tijekom obrade poruke, poruke će se ponovno pojaviti na u redu čekanja nakon isteka isteka uvid. Će se obrađuju u drugoj instanci zadatka ili vrijeme početka idućeg kruga obrada tu instancu. Ako poruku dosljedno uzrokuje pogrešku na korisnik, on će blokira zadatak, reda čekanja i naposljetku same aplikacije kada redu čekanja postane puno. Stoga je ključne za otkrivanje i uklanjanje poison poruke iz reda. Ako koristite Bus servisa Azure, poruke koje uzrokuju pogrešku mogu premjestiti automatski ili ručno čekanja za povezane dead slovo.
  - Redovi su zajamčiti na _jedanput_ mehanizme za isporuku, no oni mogu isporučiti ista poruka više puta. Osim toga, ako je zadatak u pozadini prekida se nakon obrade poruke, ali prije nego što ga izbrišete iz reda čekanja, poruke će postaju dostupne za obradu ponovno. Pozadinske zadatke mora biti idempotent, što znači da više puta obrade ista poruka ne uzrokuje pogrešku ili nedosljednosti u podacima za aplikacije. Neki postupci su prirodan idempotent, kao što su postavljanje spremljena vrijednost na određeni novu vrijednost. Međutim, operacija kao što su dodavanje vrijednosti u postojeće spremljene vrijednosti bez provjere da spremljene vrijednosti i dalje ista je kao kada je poruka prvotno poslana uzrokovat će nedosljednosti. Azure redova Bus servis moguće je konfigurirati da biste automatski uklonili dupliciranu poruke.
  - Neke razmjenu sustave, kao što su redova Azure prostora za pohranu i redova Bus servisa Azure podržava svojstvo de-queue broj koji pokazuje koliko je puta je poruka pročitana iz reda. To može biti korisno obrade koji se ponavljaju i poison poruke. Dodatne informacije potražite u članku [Asinkronog Primer za razmjenu poruka](http://msdn.microsoft.com/library/dn589781.aspx) i [Idempotency uzoraka](http://blog.jonathanoliver.com/2010/04/idempotency-patterns/).

## <a name="scaling-and-performance-considerations"></a>Promjena veličine i performanse

Pozadinske zadatke morate nude dovoljno dobre performanse da bi ih ne blokiraj aplikacije ili dovesti do nedosljednosti zbog odgođeno operacija kada je sustav opterećenju. Obično je poboljšana performansi po skaliranje računalnim slučajeve u kojima su hostirani pozadinske zadatke. Kada su planiranje i dizajniranje pozadinske zadatke, imajte na umu sljedeće oko skalabilnost i performanse:

- Azure podržava autoscaling (skaliranje izgleda i promjena veličine u) na temelju trenutne zahtjev i učitavanje – ili na unaprijed definirane rasporedu, web-aplikacijama web servise u Oblaku i uloge suradnika i virtualnim strojevima hostira implementacijama. Pomoću ove značajke provjerite imaju li aplikacija cijela dovoljno performanse mogućnosti tijekom minimiziranje runtime troškove.
- Gdje je pozadinske zadatke imaju različite performanse mogućnost iz drugih dijelova aplikacije za servise u Oblaku (na primjer, korisničkog Sučelja i komponente kao što su sloj pristupa podacima), pozadinske zadatke zajedno u ulozi zasebnom radnom hostiranja omogućuje korisničkog Sučelja i pozadine uloge zadatka da biste skalirali samostalno upravljanje opterećenje. Ako više pozadinske zadatke mogućnosti performanse znatno razlikuje od drugih, razmislite o podjelom zasebnom tempiranja uloge i neovisno skaliranja svaku vrstu uloga. Međutim, imajte na umu da to možda povećava troškove runtime u usporedbi s kombiniranje sve zadatke u manje uloge.
- Jednostavno skaliranje uloge možda neće biti potrebne da biste spriječili gubitak performanse opterećenju. Možda ćete morati skaliranje reda čekanja za pohranu i ostale resurse da biste spriječili jednu točku na Ukupno obrade lanac postao usko grlo. Uzmite u obzir drugih ograničenja, kao što je Maksimalna propusnost za pohranu i ostale servise koje ovise o aplikacija i pozadinske zadatke.
- Namijenjena mora biti skaliranje pozadinske zadatke. Na primjer, moraju biti mogućnosti dinamički prepoznati broj reda čekanja za pohranu koristi da bi se osluškuju ili slati poruke u odgovarajući red.
- Po zadanome, WebJobs Skaliranje s instancom pridružene Azure web-aplikacije. Međutim, ako želite da se WebJob da biste pokrenuli kao samo jednu instancu, možete stvoriti Settings.job datoteku koja sadrži podatke JSON **{"is_singleton": true}**. To navodi Azure da biste se izvoditi samo jednu pojavu WebJob, čak i ako postoji više instanci povezana web-aplikacije. To može biti korisno tehnika Zakazani zadaci koje morate pokrenuti kao samo jednu instancu.

## <a name="related-patterns"></a>Srodni uzoraka

- [Asinkrona Primer za razmjenu poruka](http://msdn.microsoft.com/library/dn589781.aspx)
- [Upute za Autoscaling](http://msdn.microsoft.com/library/dn589774.aspx)
- [Kompenziranje uzorak transakcije](http://msdn.microsoft.com/library/dn589804.aspx)
- [Uzorak konkurentnog koje korisnici](http://msdn.microsoft.com/library/dn568101.aspx)
- [Particija smjernice za izračun](http://msdn.microsoft.com/library/dn589773.aspx)
- [Uzorak konsolidacije resursa za izračun](http://msdn.microsoft.com/library/dn589778.aspx)
- [Uzorak Upravitelj protoka informacija](http://msdn.microsoft.com/library/dn589793.aspx)
- [Uzorak izborno vodeće crte](http://msdn.microsoft.com/library/dn568104.aspx)
- [Kanali i uzorak filtara](http://msdn.microsoft.com/library/dn568100.aspx)
- [Uzorak prioritet reda čekanja](http://msdn.microsoft.com/library/dn589794.aspx)
- [Utemeljen na red učitavanja ujednačavanje uzorak](http://msdn.microsoft.com/library/dn589783.aspx)
- [Raspored Agent nadzornik uzorak](http://msdn.microsoft.com/library/dn589780.aspx)

## <a name="more-information"></a>Dodatne informacije

- [Promjena veličine Azure aplikacije uloga suradnika](http://msdn.microsoft.com/library/hh534484.aspx#sec8)
- [Izvođenje pozadinske zadatke](http://msdn.microsoft.com/library/ff803365.aspx)
- [Azure uloga pokretanje životnog ciklusa](http://blog.syntaxc4.net/post/2011/04/13/windows-azure-role-startup-life-cycle.aspx) (članak na blogu)
- [Životni ciklus za uloge servisa Azure oblaka](http://channel9.msdn.com/Series/Windows-Azure-Cloud-Services-Tutorials/Windows-Azure-Cloud-Services-Role-Lifecycle) (videozapis)
- [Početak rada s Azure WebJobs SDK](./app-service-web/websites-dotnet-webjobs-sdk-get-started.md)
- [Azure redovima i usluge Bus redovi – usporedbi i iz Outlooka nasuprot](./service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md)
- [Kako omogućiti i dijagnostiku u Oblaku](./cloud-services/cloud-services-dotnet-diagnostics.md)
