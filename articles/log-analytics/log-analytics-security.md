<properties
    pageTitle="Prijava analitiku sigurnost podataka | Microsoft Azure"
    description="Saznajte kako prijava analitiku štiti vaše privatnosti i secures podataka."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/23/2016"
    ms.author="banders"/>

# <a name="log-analytics-data-security"></a>Prijavite se zaštite analize podataka

Microsoft predano radi na zaštiti privatnosti i zaštiti podataka tijekom izlaganja softvera i usluga koje olakšavaju upravljanje IT infrastrukturu vaše tvrtke ili ustanove. Ne možemo prepoznati da je kada entrust podataka s drugima, te pouzdanost zahtijeva stroge sigurnost. Microsoft se pridržava izričite smjernice za usklađenost i sigurnost – iz kodiranje za operacijski servisa.

Zaštita i zaštiti podataka je gornja prioritet Microsoft. Obratite nam se s bilo kojim pitanja, prijedloge ili pitanja o sljedeće podatke, uključujući naš sigurnosna pravila na [Azure mogućnosti podrške](http://azure.microsoft.com/support/options/).

U ovom se članku objašnjava kako koji se prikupljaju, obrađuju i osigurava zapisnika analize u na operacije upravljanja paket (OMS) podataka. Povezivanje s web-servisa, prikupljanje podataka o radu pomoću sustava centar za komponentu Operations Manager ili dohvaćanje podataka iz Azure Dijagnostika za korištenje zapisnika analize možete koristiti agenata. Prikupljene podaci se šalju putem interneta pomoću provjere autentičnosti utemeljene na certifikat i SSL 3 servis za prijava analitiku, koji se nalazi u Microsoft Azure. Sažimanje podataka te tako da agenta prije slanja.

Servis zapisnika analize podataka u oblaku upravlja sigurno na sljedeće načine:

- segregation podataka
- zadržavanje podataka
- Fizička sigurnosti
- Upravljanje incidenta
- usklađenost
- Sigurnost standarde certifications


## <a name="data-segregation"></a>Segregation podataka

Podatke o klijentu čuvati logično zasebnom svaku komponentu cijeloj OMS usluga. Svi podaci označen po tvrtke ili ustanove. Ovaj označavanje nastavi cijeloj životni ciklus podataka, a Poboljšana je svaki sloju servisa. Svaki korisnik ima namjenski Azure blob koju se pohranjuju Dugoročne podataka

## <a name="data-retention"></a>Zadržavanje podataka

Indeksirani podaci iz zapisnika pretraživanja prilikom spremanja i zadržavati plan cijene. Dodatne informacije potražite u članku [Zapisnika analize cijene](https://azure.microsoft.com/pricing/details/log-analytics/).

Microsoft briše korisničkih podataka od 30 dana nakon zatvaranja OMS radnog prostora. Microsoft briše i račun za pohranu Azure gdje se nalaze podaci. Kada se uklanja podatke o klijentu, bez fizičke pogona se uništavaju.

U sljedećoj su tablici navedeni neki od dostupnih rješenja u OMS i oni prikupljanje Primjeri vrste podataka.

| **Rješenja** | **Vrste podataka** |
| --- | --- |
| Konfiguraciji Procjena | Konfiguracija podataka, metapodataka i podatke o stanju |
| Planiranje kapaciteta | Performanse podaci i metapodaci |
| Antimalware | Konfiguriranje podaci i metapodaci |
| Procjena ažuriranja sustava | Metapodaci i stanje podataka |
| Upravljanje zapisnikom | Korisnički definirane zapisnika događaja, zapisnika događaja sustava Windows i/ili IIS zapisnika |
| Evidentiranje promjena | Zaliha softvera i servisa Windows metapodataka |
| SQL i procjenu servisa Active Directory | WMI podataka, podatke u registru, podataka o performansama i SQL Server dinamički upravljanje prikaz rezultata |

Sljedeća tablica sadrži primjere vrste podataka:

| **Vrsta podataka** | **Polja** |
| --- | --- |
| Upozorenje | Upozorenje naziv, opis upozorenja, BaseManagedEntityId, ID problema, IsMonitorAlert, RuleId, ResolutionState, prioriteta, težinu, kategorija, vlasnika, ResolvedBy, TimeRaised, TimeAdded, LastModified, LastModifiedBy, LastModifiedExceptRepeatCount, TimeResolved, TimeResolutionStateLastModified, TimeResolutionStateLastModifiedInDB, RepeatCount |
| Konfiguracija | IdKlijenta "," AgentID "," EntityID "," ManagedTypeID "," ManagedTypePropertyID "," parametar Sadašnjavrijednost, ChangeDate |
| Događaja | Iddogađaja, EventOriginalID, BaseManagedEntityInternalId, RuleId, PublisherId, PublisherName, FullNumber, broj, kategorija, ChannelLevel, LoggingComputer, EventData, EventParameters, TimeGenerated, TimeAdded <br>**Bilješke:** Kada se odjavite događaje s prilagođenim poljima u zapisniku događaja sustava Windows, OMS prikuplja ih. |
| Metapodataka | BaseManagedEntityId, ObjectStatus, OrganizationalUnit, ActiveDirectoryObjectSid, PhysicalProcessors, NetworkName, IPAdresa, ForestDNSName, NetbiosComputerName, VirtualMachineName, LastInventoryDate, HostServerNameIsVirtualMachine, IP adresa, NetbiosDomainName, LogicalProcessors, DNSName, riješiti problem, DomainDnsName, ActiveDirectorySite, PrincipalName, OffsetInMinuteFromGreenwichTime |
| Performanse | ObjectName, CounterName, PerfmonInstanceName, PerformanceDataId, PerformanceSourceInternalID, SampleValue, TimeSampled, TimeAdded |
| Stanje | StateChangeEventId, StateId, NewHealthState, OldHealthState, kontekst, TimeGenerated, TimeAdded, StateId2, BaseManagedEntityId, MonitorId, HealthState, LastModified, LastGreenAlertGenerated, DatabaseTimeModified |

## <a name="physical-security"></a>Fizička sigurnosti

Zapisnik analize u servisu OMS je manned Microsoftovo osoblje i sve aktivnosti prijavljeni i moguće nadzirati. Servis pokreće se u potpunosti u Azure i usklađenost sa Azure zajedničkih inženjerskim kriterija. Na stranici 18 [Pregled sigurnosti za Microsoft Azure](http://download.microsoft.com/download/6/0/2/6028B1AE-4AEE-46CE-9187-641DA97FC1EE/Windows%20Azure%20Security%20Overview%20v1.01.pdf)možete pogledati detalje o fizičke sigurnost Azure resursi. Prava pristupa za fizičke sigurne područja mijenjaju unutar tvrtke jedan dan za sve koji više ne sadrži odgovornost za servis OMS, uključujući prijenos i prekid. Saznajte o globalni fizičke infrastrukture koristimo [Podatkovnim centrima Microsoft](https://www.microsoft.com/en-us/server-cloud/cloud-os/global-datacenters.aspx).

## <a name="incident-management"></a>Upravljanje incidenta

OMS ima proces incidenta upravljanja koja u skladu sve Microsoftove servise. Ukratko, ne možemo:

- Korištenje modela zajedničke odgovornosti gdje dio sigurnost odgovornosti pripada Microsoft dok neki dio pripada kupcu
- Upravljanje Azure sigurnošću
  - Otkrivanje incident pri prvom podatak, da biste pokrenuli istrazi
  - Procijenite utjecaj i težinu incident po član tima za incidenta odgovor na poziv. Na temelju dokaz na vrednovanje možda ili može rezultirati dodatno eskalacija sigurnost timu za odgovor.
  - Dijagnosticiranje uz incident vezan uz sigurnost odgovor stručnjaci provedbe istraživanja tehničke ili forensic, prepoznavanje strategije zaustavljanja, ublažiti i zaobilazno rješenje. Ako tim sigurnosti koje misli da korisničkih podataka možda imaju postaju izložen pojedinac nezakonitom ili neovlaštenih, paralelnog izvođenja procesa kupca Incident obavijesti započinje paralelno.  
  - Stabiliziraj i oporavak incident. U tim incidenta odgovor stvara plan za oporavak u smanjenju problem. Koraci zaustavljanja crisis kao što su quarantining utjecati sustavi se može dogoditi odmah i paralelno s Dijagnostika. Više mitigations termina može biti planirano koji se pojavljuju nakon odmah rizika.  
  - Zatvorite incident i vođenje mortem objavu. U tim incidenta odgovor stvara objavu-mortem koji navode pojedinosti incident s namjera da biste ispravili pravila, postupke i procesa da biste spriječili reoccurrence događaja.
- Obavijestite kupce sigurnošću
  - Određivanje opsega utjecati klijenata i bilo koja osoba koja utjecati kao detaljne obavijest moguće
  - Stvaranje obavijesti o omogućuje korisnicima putem detaljne dovoljno informacije tako da mogu izvršiti istrazi na njihove završetka i zadovoljavaju prakse za zaštitu učinjene njihove krajnjim korisnicima dok ne unduly što postupak obavijesti.
  - Potvrdite i deklarirati incident prema potrebi.
  - Obavijestite kupce uz incident obavijest bez unreasonable odgode i skladu s bilo kojeg izvršenja pravne ili stvaranje. Na bilo koji način odabire Microsoft, uključujući putem e-pošte će isporuku obavijesti o sigurnošću na jednu ili više administratora klijenta.
- Vođenje spremnosti za tim i obuka
  - Microsoftovo osoblje su potrebne za dovršetak sigurnost i obuka Informiranost koji štedi za prepoznavanje i stvaranje izvješća sumnjivu sigurnosnim problemima.  
  - Operatori rad na servisu Microsoft Azure imati zbrajanja obuka obaveze oko njihov pristup za osjetljive sustavi za hostiranje podatke o klijentu.
  - Microsoftovo sigurnost odgovor osoblje primanje specijalizirane obuka za svoje uloge


U slučaju gubitka podataka za sve korisnike, ne možemo obavijesti svaki klijent unutar jedan dan. Međutim, gubitka podataka kupca nikad pojavila s OMS. Osim toga, ne možemo održavanje kopije podataka koja je stvorena i geografski distribuira.

Dodatne informacije o kako Microsoft odgovori sa sigurnošću potražite u članku [Microsoft Azure sigurnost odgovor u Oblaku](https://gallery.technet.microsoft.com/Azure-Security-Response-in-dd18c678/file/150826/1/Microsoft Azure Security Response in the cloud.pdf).

## <a name="compliance"></a>Usklađenost

OMS software development i usluga tima informacije o sigurnosti i upravljanja program podržava njegov preduvjeti za tvrtke, a pridržava zakonima i pravilnicima kao što je opisano na web-mjesto [Centra za pouzdanost Microsoft Azure](https://azure.microsoft.com/support/trust-center/) i u okvir za [Usklađenost centar za pouzdanost Microsoft](https://www.microsoft.com/en-us/TrustCenter/Compliance/default.aspx). Kako OMS uspostavlja sigurnosne preduvjete, služi za identifikaciju sigurnosne kontrole, upravlja i nadzire opasnosti i opisane su tamo. Dobiti godišnje, provest pregled od pravila, standarde, postupke i smjernice.

Svaki član tima za razvoj OMS prima obuka sigurnost formalno aplikacije. Interno, sustavu kontrole verzija koristimo za razvoj softvera. Svaki projekt softver zaštićen sustavu kontrole verzija.

Microsoft je sigurnost i usklađenost tima koji nadgleda assesses sve servise u programu Microsoft. Informacije o sigurnosti Službenici čine tima i nisu pridružene inženjerskim odjele koji razvijaju OMS. Službenici za sigurnost imaju vlastite lanac i vođenje neovisno konfiguracije proizvoda i usluga da biste bili sigurni sigurnost i usklađenost.

Microsoftovi ploče uprava obavještavanja i Godišnje izvješće o svim sigurnost podataka programa Microsoft.

OMS softver razvoj i servis za tim aktivno rad s timovima Microsoft Legal i usklađenost i druge partnere industrijskih dobiti raznih certifications.

## <a name="security-standards-certifications"></a>Sigurnost standarde certifications

Prijava analitiku u OMS trenutno zadovoljiti sljedeće standarde sigurnost:

- [ISO IEC 27001](http://www.iso.org/iso/home/standards/management-standards/iso27001.htm) i [ISO IEC 27018:2014](http://www.iso.org/iso/home/store/catalogue_tc/catalogue_detail.htm?csnumber=61498) usklađen
- Plaćanje kartice industrijskih (PCI usklađen) podataka sigurnost Standard (PCI DSS) tako da na PCI sigurnost standarde sastanka.
- [Vrsta usluge tvrtke ili ustanove kontrole (PnP) 1 1 i PnP 2 vrsta 1](https://www.microsoft.com/en-us/TrustCenter/Compliance/SOC1-and-2) usklađen
- Windows uobičajenih inženjering kriterija
- Microsoft pouzdana ustanova računalstvo
- Kao Azure service komponente koje koristi OMS drži zahtjevima za usklađenosti Azure. Dodatne informacije na [Usklađenost centar za pouzdanost Microsoft](https://www.microsoft.com/en-us/TrustCenter/Compliance/default.aspx).


## <a name="cloud-computing-security-data-flow"></a>Računalstvo sigurnost toka podataka u oblaku
Na sljedećem su dijagramu bit će oblaka sigurnosna arhitektura tijeka informacija iz vaše tvrtke i kako je osigurani kao što je premjestiti servis prijava analitiku konačni vidjeti koje ste na portalu OMS. Dodatne informacije o svakom koraku slijedi dijagram.

![Slika gumba OMS prikupljanje podataka i sigurnosti](./media/log-analytics-security/log-analytics-security-diagram.png)

## <a name="1-sign-up-for-log-analytics-and-collect-data"></a>1. registracije za zapisnika analize i prikupljanja podataka

Za tvrtku ili ustanovu slanje podataka u zapisniku analize, konfigurirati Windows agenata, programe koji se izvode na Azure virtualnim strojevima ili agenata OMS za Linux. Ako koristite Operations Manager agenata, zatim koristit ćete čarobnjak za konfiguraciju na konzoli operacije da biste ih konfigurirali. Korisnici (što može biti vama, drugi pojedinačnih korisnika ili određena skupina korisnika) stvorite jedan ili više računa OMS (OMS radne prostore), a registrirati agenata pomoću neke od sljedećih računa:


- [ID tvrtke ili ustanove](../active-directory/sign-up-organization.md)

- [Microsoftova računa – Outlook, Office Live, MSN](http://www.microsoft.com/account/default.aspx)

Radni prostor programa OMS je gdje podataka koji se prikupljaju, pridružuje, analizirati, a prikazuju. Radni prostor prvenstveno se koristi kao sredstvo particija podataka, a jedinstvenim pojedinom radnom prostoru. Na primjer, možda ćete morati imati radni podataka s jednog radnog prostora OMS i testiranje podataka s drugog radnog prostora. Radni prostori pomoći i programa administrator kontrola korisničkog pristupa podacima. Pojedinom radnom prostoru možete imati više korisničkih računa koji su povezani i svaki korisnički račun možete pristupiti više OMS radnog prostora. Stvaranje radne prostore koji se temelji na područje podatkovnog centra. Pojedinom radnom prostoru je replicirati na drugim podatkovnim centrima u regiji prvenstveno za OMS dostupnost usluge.

Za Operations Manager nakon dovršetka čarobnjaka za konfiguriranje svake grupe upravljanje Operations Manager uspostavlja vezu sa servisom zapisnika analize. Zatim pomoću čarobnjaka za dodavanje računala možete odabrati kojim računalima u grupi Upravljanje dopušteno slanje podataka na servis. Za ostale vrste agent za svaki povezuje sigurno OMS usluga.

Svu komunikaciju između povezanog sustavi i prijava analitiku servis je šifriran.  Protokol TLS (HTTPS) koristi se za šifriranje.  Microsoft SDL postupak je slijediti provjerite je li prijava analitiku najnovije najnovije tehnološke šifriranja protokola.

Agent za svaku vrstu prikuplja podatke za analizu zapisnika. Vrste podataka koji se prikupljaju ovisi o vrstama rješenja koristi. Možete vidjeti sažetak prikupljanje podataka na [Dodavanje analize zapisnika rješenja iz galerije rješenja](log-analytics-add-solutions.md). Uz to, detaljnije informacije o zbirke dostupna je za većinu rješenja. Rješenje je paket unaprijed definirane prikaze, upita za pretraživanje zapisnika, pravila zbirke podataka i logiku obrade. Samo administratori mogu koristiti prijava analitiku da biste uvezli rješenje. Nakon uvoza rješenja, ona se premješta s poslužiteljima upravljanja Operations Manager (Ako se koristi), a zatim sve agenata koje ste odabrali. Nakon toga u agenata prikupiti podatke.

## <a name="2-send-data-from-agents"></a>2. slati podatke iz agenata

Sve vrste agent registrirate ključa za registraciju i sigurno povezivanje uspostavljanja između agenta i prijava analitiku usluge provjere autentičnosti utemeljene na certifikat i SSL pomoću priključak 443. OMS koristi tajnu spremište za generiranje i održavanje tipke. Privatni ključevi su zakrenut svakih 90 dana i spremaju se u Azure i upravlja Azure operacije koji prate izričite propisima i usklađenost prakse.

Operations Manager registrirati radnog prostora programa Groove sa servisom zapisnika analize i sigurno povezivanje HTTPS uspostavljanja između poslužitelju za upravljanje Operations Manager.

Za Windows programe koji se izvode na Azure virtualnim računalima, samo za čitanje prostora za pohranu ključ se koristi da biste pročitali dijagnostičkih događaja u Azure tablicama.

Ako je bilo koji agent može komunicirati s uslugom iz bilo kojeg razloga, prikupljene podatke pohranjuju lokalno u predmemoriju za privremene i poslužitelj za upravljanje pokuša ponovno slanje podataka svakih 8 minuta za dva sata. Predmemorirani podaci u agent zaštićen spremište vjerodajnica za operacijski sustav. Ako servis nije moguće obrade podataka nakon 2 sata, na agenata će red čekanja podatke. Ako redu čekanja postane puni, OMS pokreće ispuštanje vrste podataka, počevši od podataka o performansama. Agent reda čekanja ograničenje iznosi ključ registra radi izmjene, ako je potrebno. Prikupljene podaci se komprimiraju i na servisu, zaobilaženje lokalne baze podataka, tako da sve učitavanja dodati na njih. Nakon slanja prikupljenih podataka, znači da je uklonjena iz predmemorije.

Prethodno opisan, podaci iz vaše agenata se šalju putem SSL s podatkovnim centrima sustava Microsoft Azure. Po želji možete koristiti ExpressRoute za davanje dodatne sigurnosti za podatke. ExpressRoute je način povezati izravno Azure iz postojeće mreže WAN, kao što su više protokol natpis prijelaz između VPN (MPLS), koje ste dobili od davatelja usluga mreže. Dodatne informacije potražite u članku [ExpressRoute](https://azure.microsoft.com/services/expressroute/).


## <a name="3-the-log-analytics-service-receives-and-processes-data"></a>3. na servis zapisnika Analytics prima i obrađuje podatke

Servis prijava analitiku osigurava ulazne podatke iz pouzdanog izvora tako da provjeri valjanosti certifikata i cjelovitosti podataka pomoću Azure provjere autentičnosti. Neprovedenih sirovim podacima pohranjuje kao blob u [Spremište na platformi Microsoft Azure](../storage/storage-introduction.md) i je šifriran. No svaki Azure spremište blobova platforme ima skup Jedinstveni skup tipki koja je dostupna samo za tog korisnika. Vrstu podataka koja je spremljena ovisi o vrsti rješenja koja su uvesti i koristiti da biste prikupili podatke. Nakon toga prijava analitiku servisa obrađuje sirovim podacima za blog Azure prostora za pohranu.

## <a name="4-use-log-analytics-to-access-the-data"></a>4 korištenje zapisnika Analytics za pristup podacima

Se možete prijaviti na prijava analitiku na portalu OMS pomoću računa tvrtke ili ustanove ili Microsoftov račun koji ste postavili prethodno. Sav promet između OMS portal i prijava analitiku u OMS se šalje putem sigurnog kanala HTTPS. Kada pomoću portala za OMS sesiju ID generira se na klijentskom računalu korisnika (web-preglednik), a podaci se pohranjuju u lokalnu predmemoriju dok se prekida sesiju. Kada je prekinut, briše se predmemoriju. Klijentsko kolačića koje sadrže-osobne podatke, se automatski uklanjaju. Sesije kolačići označavaju HTTPOnly i su zaštićeni. Nakon unaprijed odredio razdoblje neaktivnosti sesije portala OMS je prekinuti.

Pomoću portala za OMS, podatke možete izvesti u CSV datoteku i možete pristupiti podacima pomoću API-ji za pretraživanje. Izvoz u obliku CSV ograničeno je na 50.000 redaka po izvoz i API podataka je ograničeno na 5000 redaka po pretraživanja.

## <a name="next-steps"></a>Daljnji koraci

- [Početak rada s prijava analitiku](log-analytics-get-started.md) da biste saznali dodatne informacije o zapisniku analize i početi u minutama.
