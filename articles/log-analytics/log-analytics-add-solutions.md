<properties
    pageTitle="Dodavanje rješenja prijava analitiku iz galerije rješenja | Microsoft Azure"
    description="Zapisnik analize rješenja su zbirka logike, vizualizacija i acquisition podataka pravila koja pružaju metriku pivoted oko područje određenog problema."
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
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="add-log-analytics-solutions-from-the-solutions-gallery"></a>Dodavanje rješenja prijava analitiku iz galerije rješenja

Zapisnik analize rješenja su Zbirka **logike**, **vizualizacija** i **pravila za nabavu podataka** koji omogućuju metriku pivoted oko područje određenog problema. U ovom članku rješenja za popise podržana zapisnika analize i objašnjava kako dodavati i uklanjati ih pomoću galerije rješenja.

Rješenja dopustili dublju uvida u:

- Pomoć za istraživanje i rješavanje problema sa radu brže
- Prikupljanje i povezivanje različite vrste podataka za računala
- može pomoći određene proaktivne aktivnosti kao što su planiranje kapaciteta, zakrpu status izvješćivanje i nadzor sigurnost.


>[AZURE.NOTE] Prijava analitiku obuhvaća funkciju zapisnika pretraživanja, pa ne morate instalirati rješenje je omogućiti. Međutim, možete dobiti vizualizacije podataka, predložena pretraživanja i uvida dodavanjem rješenja iz galerije rješenja.

Kada dodate rješenje, podaci prikupljeni s poslužiteljima u sustavu i slati OMS usluga. Obrada tako da na OMS uobičajen servis nekoliko minuta u sat. Kada je servis obrađuje podatke, možete je prikazati u OMS.

Jednostavno možete ukloniti rješenje kada više nije potrebna. Kada uklonite rješenje, njegov se podaci ne šalju na OMS koje možete smanjiti količinu podataka koji se koriste dnevni ograničenja, ako postoji.


## <a name="solutions-supported-by-the-microsoft-monitoring-agent"></a>Rješenja podržava Microsoft Agent nadzora

Trenutačno poslužitelje koji su povezani s OMS koristeći Microsoft Agent nadzor možete koristiti većinu rješenja dostupna, uključujući:

- Procjena servisa Active Directory
- Upozorenja Management (bez upozorenja SCOM)
- Antimalware
- Evidentiranje promjena
- Sigurnost
- Procjena SQL
- Ažuriranja sustava

Međutim, sljedeće se *ne* podržava Microsoft Agent za nadzor i zahtijevaju agent sustava centra operacije Manager (SCOM).

- Upozorenja Management (uključujući SCOM upozorenja)
- Upravljanje kapaciteta
- Konfiguraciji Procjena

Pogledajte [Povezivanje prijava analitiku u komponente Operations Manager](log-analytics-om-agents.md) za informacije o povezivanju SCOM agent zapisnika analize.

### <a name="to-add-a-solution-using-the-solutions-gallery"></a>Da biste dodali rješenje pomoću galerije rješenja

1. Na stranici pregled OMS kliknite pločicu **Galerije rješenja** .    
    ![galerije rješenja](./media/log-analytics-add-solutions/sol-gallery.png)
2. Na stranici galerije rješenja OMS Informirajte se o svakom dostupno rješenje. Kliknite naziv koji želite dodati OMS rješenja.
3. Detaljne informacije o rješenje prikazuju se na stranici za rješenje koje ste odabrali. Kliknite **Dodaj**.
4. Novi pločicu za rješenje koje ste dodali na pregled stranice u OMS, a možete ga počnete koristiti kada OMS usluga obrađuje podataka će se prikazati.

## <a name="to-configure-solutions"></a>Konfiguriranje rješenja
1. Morat ćete konfigurirati neke rješenja. Ako, na primjer, morat ćete konfigurirati Automatizacija, oporavak Azure web-mjesta i sigurnosno kopiranje biste mogli koristiti.
2. Za bilo koju od tih rješenja, kliknite njegov pločica na stranici pregled.  
    ![Konfiguriranje rješenja](./media/log-analytics-add-solutions/configure-additional.png)
3. Nakon toga konfiguriranje rješenja s potrebne informacije, a zatim kliknite **Spremi**.  
    ![Konfiguriranje rješenja](./media/log-analytics-add-solutions/configure.png)

### <a name="to-remove-a-solution-using-the-solutions-gallery"></a>Da biste uklonili rješenje pomoću galerije rješenja

1. Na stranici pregled OMS kliknite pločicu **Postavke** .
2. Na stranici Postavke na kartici rješenja kliknite **Ukloni** za rješenje koje želite ukloniti.
3. U dijaloškom okviru za potvrdu kliknite **da** da biste uklonili rješenja.

## <a name="data-collection-details-for-oms-features-and-solutions"></a>Detalji zbirke podataka za OMS značajke i daje rješenja

Sljedeća tablica prikazuje metode zbirke podataka i druge detalje o načinu prikupljanja podataka za OMS značajke i daje rješenja. Izravni agenata i SCOM agenata jednake zapravo, no Izravni agent obuhvaća dodatnim funkcijama koje dopušta povezivanje s radnim prostorom OMS i usmjeravajte proxy poslužitelj. Ako koristite SCOM agent, morate ciljani kao agent OMS možete komunicirati s OMS. SCOM agenata u ovoj tablici su OMS agenata koji su povezani s SCOM. Informacije o povezivanju okruženje SCOM OMS potražite u članku [Povezivanje prijava analitiku u komponente Operations Manager](log-analytics-om-agents.md) .

>[AZURE.NOTE] Vrsta koji koristite određuje kako se podaci šalju u OMS, uz sljedeće uvjete:

- Ili koristite Izravni agent ili SCOM priložene OMS agent.
- Kada je potrebno SCOM, SCOM agent za rješenje uvijek slanje podataka OMS pomoću grupe za upravljanje SCOM. Uz to, kada je potrebno SCOM, samo agent SCOM koristi rješenja.
- Kada SCOM nije potrebna, a u tablici prikazane su SCOM agent podataka slanja OMS pomoću upravljanja grupe, zatim SCOM agent uvijek slanje podataka OMS pomoću upravljanja grupama. Izravni agente zaobići grupu za upravljanje i poslati svoje podatke izravno OMS.
- Kada SCOM agent se podaci ne šalju pomoću upravljanja grupe, zatim podaci se šalju izravno u OMS – zaobilaženje grupu upravljanje.


|Vrsta podataka| platforme | Izravni Agent | Agent za SCOM | Prostor za pohranu za Azure | SCOM potrebne? | SCOM agent podataka šalju putem upravljanja grupe | Učestalost zbirke |
|---|---|---|---|---|---|---|---|
|Procjena AD|Windows|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)|  sedam dana|
|Status AD replikacije|Windows|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|5 dana|
|Upozorenja (Nagios)|Linux|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|pristignu|
|Upozorenja (Zabbix)|Linux|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|1 min|
|Upozorenja (Operations Manager)|Windows|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)|3 minute|
|Antimalware|Windows|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)| hourly|
|Upravljanje kapaciteta|Windows|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)| hourly|
|Evidentiranje promjena|Windows|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)| hourly|
|Evidentiranje promjena|Linux|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|hourly|
|Konfiguraciji Procjena (Savjetnik za naslijeđene)|Windows|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)| dvaput prema danu|
|ETW|Windows|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|pet minuta|
|IIS zapisnika|Windows|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|pet minuta|
|Ključ sefovi|Windows|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|10 minuta|
|Mrežni aplikacije pristupnika|Windows|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|10 minuta|
|Mrežni sigurnosne grupe|Windows|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|10 minuta|
|Office 365|Windows|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|obavijest o|
|Mjerača performansi|Windows|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|kao zakazano, najmanje 10 sekundi|
|Mjerača performansi|Linux|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|kao zakazano, najmanje 10 sekundi|
|Tkanina servisa|Windows|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|pet minuta|
|Procjena SQL|Windows|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)| sedam dana|
|SurfaceHub|Windows|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|pristignu|
|Syslog|Linux|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|iz spremišta Azure: 10 minuta; agenta: pristignu|
|Ažuriranja sustava|Windows|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)| najmanje 2 puta po dan i 15 minuta nakon instaliranja ažuriranja|
|Zapisnici događaja sigurnost sustava Windows|Windows|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)| za Azure pohranu: 10 min; za agenta: pristignu|
|Zapisnici vatrozida za Windows|Windows|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)| pristignu|
|Zapisnike događaja sustava Windows|Windows|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)| za Azure pohranu: 1 min; za agenta: pristignu|
|Žičani podataka|Windows (2012 R2 / 8.1 ili noviji)|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Da](./media/log-analytics-add-solutions/oms-bullet-green.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)|![ne](./media/log-analytics-add-solutions/oms-bullet-red.png)| Svaki 1 min|

## <a name="log-analytics-preview-solutions-and-features"></a>Zapisnik analize pretpregled rješenja i značajke

Tako da radi servisa i slijedite postupke devops možemo partnera s klijentima za razvoj značajke i daje rješenja.

Tijekom privatne pretpregleda ne možemo dati maloj grupi korisnika pristupa Prijevremeni implementacije značajka ili rješenja da biste dobili povratne informacije i poboljšati. Ova Prijevremeni implementacija ima minimalnog značajke i radu mogućnosti.

Naš cilj je pokušati stvari brzo da bismo mogli pronaći što radi, a što ne funkcionira. Ne možemo iteracija kroz postupak dok povratnih informacija od klijenata privatne pretpregled obavještava nam nas jeste li spremni za javne pretpregled.

Tijekom javno pretpregleda dajemo značajke ili rješenje dostupan za sve korisnike da biste dobili dodatne povratne informacije i provjeriti naš skaliranje i učinkovitosti. Tijekom faze:

- Pretpregled značajke će se prikazivati na kartici postavke, a možete omogućiti tako da svaki korisnik
- Pretpregled rješenja mogu se dodati putem galerije ili korištenja objavljene skripte

### <a name="what-should-i-know-about-preview-features-and-solutions"></a>Što znati o pretpregled značajke i daje rješenja?

Ispričavamo se Probuđeni o novim značajkama i rješenja i smo volite raditi sa sobom za razvoj ih.

Pretpregled značajke i daje rješenja nisu desno za sve kroz, pa je prije koja vas pita da biste se pridružili privatne pretpregleda ili omogućivanje javno pretpregled provjerite je li u redu radite s nešto što je u razvoju.

Prilikom omogućivanja značajke pretpregleda putem portala sustava će biti potražite u članku upozorenje podsjeća značajka je u pretpregledu.

#### <a name="for-both-private-and-public-preview"></a>*Privatni* i *javni* Preview

Sljedeće odnosi se na javne i privatne pretpregleda:

- Stvari koje možda neće uvijek pravilno funkcionirati.
  - Problemi s raspon onemogućuje manji preplave kroz nešto uopće ne funkcionira
- Postoji mogućnost pretpregled imati negativan učinak na sustavima / okruženje
  - Ne možemo pokušajte da biste izbjegli negativan stvari događa u sustavima koristite OMS, ali ponekad neočekivane što će se pojaviti
- Gubitka podataka ubuduće / bi doći do oštećenja
- Može od vas tražimo da biste prikupili dijagnostičke zapisnike ili druge podatke za pomoć pri otklanjanju poteškoća
- Značajka ili rješenje možda će biti uklonjene (privremeno ili trajno)
  - Ovisno o naše learnings tijekom pretpregleda smo odlučite ne oslobodi značajke ili rješenja
- Pretpregledi možda neće funkcionirati ili možda ne testirate s sve konfiguracije i smo ograničiti:
  - U operacijskim sustavima koji se mogu koristiti (npr. značajka možda odnose samo na Linux u pretpregledu)
  - Vrsta agent (MMA, SCOM) koje je moguće koristiti (npr. značajke možda neće funkcionirati s SCOM u pretpregledu)  
- Pretpregled rješenja i značajke pokrivaju razinu usluzi
- Korištenje značajke pretpregleda će uzrokovati naknade za korištenje
- Značajke i mogućnosti koje vam je potrebno za značajku / rješenje biti korisno možda nema ili nepotpuna
- Značajke / rješenja možda nije dostupna u svim regijama
- Značajke / rješenja možda nisu lokalizirani
- Značajke / rješenja možda je ograničenje broja korisnike ili uređaja koje mogu koristiti
- Možda ćete morati koristiti skripte za izvođenje konfiguracija i da biste omogućili rješenje/značajku
- Korisničko sučelje (UI) neće biti dovršen i može se promijeniti iz svakodnevnih
- Javni pretpreglede možda nije prikladna za proizvodnju / ključnih sustavi

#### <a name="for-private-preview"></a>Za *Privatno* pretpregled

Uz gore navedene stavke slijedi specifične za privatne pretpregleda:

- Ćemo vam kako bi pružio nam povratne informacije o iskustva tako da možete dajemo u trenutnoj bolje očekivati
- Ne možemo može od vas traži povratne informacije putem ankete, telefonske pozive ili e-pošte
- Što uvijek ne funkcionira ispravno
- Ne možemo biti potrebna je ugovor bez otkrivanja (NDA) za sudjelovanje ili uključivati povjerljivi sadržaj
  - Prije bloga, tweeting ili neki drugi način komunikacije s trećim stranama provjerite s Program upravitelj zadužen za pretpregled da biste shvatili ograničenja na objavu
- Pokretanje radnog / ključnih sustavi


### <a name="how-do-i-get-access-to-private-preview-features-and-solutions"></a>Kako dobiti pristup privatnim pretpregled značajke i daje rješenja?

Pozivamo klijente privatne pretpregledi kroz nekoliko različitih načina, ovisno o pretpregledu.

- Odgovaranje na mjesečni anketu i daje nam dozvolu za daljnji rad s vama poboljšava vjerojatnost pozvan privatne pretpregled.
- Vaš tim Microsoftov račun možete postaviti vam.
- Možete se prijaviti na temelju pojedinosti objavljena na twitter [msopsmgmt](https://twitter.com/msopsmgmt)
- Možete se prijaviti na osnovu pojedinosti događaja zajedničkih zajednici – potražite nam na natjecanjima kopije, konferencije i mrežne zajednice.


## <a name="next-steps"></a>Daljnji koraci

- [Zapisnika pretraživanja](log-analytics-log-searches.md) da biste pogledali detaljne podatke prikupljene putem rješenja.
