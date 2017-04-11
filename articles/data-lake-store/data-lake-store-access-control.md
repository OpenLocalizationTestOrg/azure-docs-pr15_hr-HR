<properties
   pageTitle="Pregled kontrola pristupa u spremištu podataka Lake | Microsoft Azure"
   description="Objašnjenje kako pristupiti kontrola u spremištu Lake podataka za Azure"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/06/2016"
   ms.author="nitinme"/>

# <a name="access-control-in-azure-data-lake-store"></a>Kontrola pristupa u spremištu Lake podataka za Azure

Pohrana podataka Lake implementira model kontrole pristupa koja je izvedena iz HDFS, i, model kontrole pristupa POSIX. U ovom članku navedene su osnove model kontrole pristupa za spremište Lake podataka. Da biste saznali više o na HDFS pristup kontrola modela potražite u članku [Vodič HDFS dozvole](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html).

## <a name="access-control-lists-on-files-and-folders"></a>Popisi za upravljanje na datotekama i mapama

Postoje dvije vrste pristup popisi za upravljanje (ACL) – **Pristup ACL-a** i **Zadani ACL-a**.

* **Pristup ACL-a** – te kontrola pristupa na objekt. Datoteke i mape imati pristup ACL-a.

* **Zadani ACL-a** – "Predložak" ACL-a pridružene mape da biste odredili pristup ACL-a za sve podređenih stavki koje su stvorene u toj mapi. Datoteka ne sadrži zadani ACL-a.

![Pohrana podataka Lake ACL-a](./media/data-lake-store-access-control/data-lake-store-acls-1.png)

Pristup ACL-a i zadani ACL-a imaju istu strukturu.

![Pohrana podataka Lake ACL-a](./media/data-lake-store-access-control/data-lake-store-acls-2.png)

>[AZURE.NOTE] Promjena zadane ACL nadređeni ne utječe na Access ACL ili zadani ACL podređenih stavki koje već postoje.

## <a name="users-and-identities"></a>Korisnicima i identiteta

Svaka datoteka i mapa sadrži distinct dozvole za ove identiteta:

* Vlasnički korisnika datoteke
* Vlasnički grupe
* Imenovani korisnika
* Imenovani grupe
* Svi ostali korisnici

Identiteti korisnika i grupa tako da su Azure Active Directory (AAD) identiteta osim ako drugačije "korisnik", u kontekstu Lake spremišta podataka ili znači programa AAD korisnika ili sigurnosnu grupu sustava AAD.

## <a name="permissions"></a>Dozvole

Dozvole na datotečnom sustavu objekt su **čitanje**, **Pisanje**, a **izvrši** , a oni mogu se na datotekama i mapama kao što je prikazano u tablici u nastavku.

|            |    Datoteka     |   Mapa |
|------------|-------------|----------|
| **Read (R)** | Može čitati sadržaj datoteke | Potreban je **čitanje** i **izvršavanje** na popisu sadržaja mape.|
| **Pisanje (W)** | Pisanje ili dodati datoteke | Potreban je **pisanje i izvršavanje** da biste stvorili podređene stavke u mapi. |
| **Izvršavanje (X)** | Znače ništa u kontekstu Lake spremišta podataka | Obavezno prolaska podređenih stavki u mapi. |

### <a name="short-forms-for-permissions"></a>Kratki obrazaca za dozvole

**RWX**koristi se za označavanje **čitati + pisanje + izvršiti**. Postoji više Zgusnuto numerički obrazac u kojem **čitanje = 4**, **Pisanje = 2**, i **izvršavanje = 1** i njihov zbroj predstavlja dozvole. Slijedi nekoliko primjera.

| Brojčani obrasca | Kratki oblik |      Što znači     |
|--------------|------------|------------------------|
| 7            | RWX        | Čitanje + pisanje + izvršiti |
| 5            | R-X        | Čitanje + izvršiti         |
| 4            | R –        | Čitanje                   |
| 0            | ---        | Nema dozvola         |


### <a name="permissions-do-not-inherit"></a>Ne nasljeđuju dozvole

U modelu POSIX stil koriste spremište podataka Lake dozvole za stavke pohranjuju se na same stavke. Drugim riječima, dozvole za stavke ne nasljeđuju od nadređene stavke.

## <a name="common-scenarios-related-to-permissions"></a>Uobičajeni scenariji vezane uz dozvole

Evo nekih uobičajenih scenarija da biste shvatili potrebne dozvole potrebne za izvođenje određenih operacija na računu za spremište Lake podataka.

### <a name="permissions-needed-to-read-a-file"></a>Potrebne su dozvole za čitanje datoteke

![Pohrana podataka Lake ACL-a](./media/data-lake-store-access-control/data-lake-store-acls-3.png)

* Datoteke da biste pročitali - pozivatelj potrebne dozvole za **čitanje**
* Za sve mape u strukturi mape koje sadrže datoteke - pozivatelj potrebne dozvole za **izvršavanje**

### <a name="permissions-needed-to-append-to-a-file"></a>Potrebne su dozvole da biste dodali datoteku

![Pohrana podataka Lake ACL-a](./media/data-lake-store-access-control/data-lake-store-acls-4.png)

* Datoteke da biste pridruživati-pozivatelj potrebne dozvole za **Pisanje**
* Za sve mape koje sadrže datoteke - pozivatelj potrebne dozvole za **izvršavanje**

### <a name="permissions-needed-to-delete-a-file"></a>Dozvole potrebne za brisanje datoteke

![Pohrana podataka Lake ACL-a](./media/data-lake-store-access-control/data-lake-store-acls-5.png)

* Za nadređenu mapu - pozivatelj potrebne dozvole za **Pisanje + izvršiti**
* Za sve druge mape kao put datoteke - pozivatelj potrebne dozvole za **izvršavanje**

>[AZURE.NOTE] Dozvole za datoteku nije potrebno da biste izbrisali datoteku pod uvjetom da iznad dva uvjeti istiniti za pisanje.

### <a name="permissions-needed-to-enumerate-a-folder"></a>Dozvole potrebne za numeriranje mape

![Pohrana podataka Lake ACL-a](./media/data-lake-store-access-control/data-lake-store-acls-6.png)

* Za mapu numerirati - pozivatelj potrebne dozvole za **čitanje + izvršavanje**
* Za sve nadređenim mape - pozivatelj potrebne dozvole za **izvršavanje**

## <a name="viewing-permissions-in-the-azure-portal"></a>Prikaz dozvola na portalu za Azure

Računa spremišta podataka Lake plohu **Explorer podataka** , kliknite **Access** da biste vidjeli ACL-a za datoteku ili mapu. U nastavku snimka kliknite Access da biste vidjeli ACL-a za mapu **kataloga** u odjeljku **mydatastore** račun.

![Pohrana podataka Lake ACL-a](./media/data-lake-store-access-control/data-lake-store-show-acls-1.png)

Nakon toga plohu **Access** kliknite **Jednostavan prikaz** da biste vidjeli jednostavniji prikaz.

![Pohrana podataka Lake ACL-a](./media/data-lake-store-access-control/data-lake-store-show-acls-simple-view.png)

Kliknite **Naprednog prikaza** da biste vidjeli dodatne prikaz.

![Pohrana podataka Lake ACL-a](./media/data-lake-store-access-control/data-lake-store-show-acls-advance-view.png)

## <a name="the-super-user"></a>Super korisnik

Super korisnik ima najviše prava svih korisnika u spremištu Lake podataka. Super korisnika:

* RWX dozvolama za **sve** datoteke i mape
* možete promijeniti dozvole na sve datoteke i mape.
* možete promijeniti vlasnički korisnika ili vlasnički grupu sve datoteke i mape.

U Azure, računa za spremište Lake podataka sadrži nekoliko Azure uloge:

* Vlasnici
* Suradnika
* Čitatelji
* Itd.

Svi u ulozi **vlasnika** računa spremišta podataka Lake je automatski superkorisnika za taj račun. Da biste saznali više o Azure uloga temelji pristup kontrola (RBAC) potražite u članku [Kontrola pristupa na temelju uloga](../active-directory/role-based-access-control-configure.md).

## <a name="the-owning-user"></a>Vlasnički korisnika

Korisnika koji je stvorio stavku je automatski vlasnički korisnik stavke. Korisnik sustava vlasnički učiniti sljedeće:

* Promjena dozvola za datoteku koja pripada
* Promijenite grupi vlasnički datoteke koji pripada, pod uvjetom da je vlasnički korisnik član grupe ciljne.

>[AZURE.NOTE] Na vlasnički korisnik **ne može** promijeniti vlasnički korisnika ili drugu datoteku vlasništvu. Samo super-korisnici mogu mijenjati vlasnički korisnik datoteku ili mapu.

## <a name="the-owning-group"></a>Vlasnički grupe

U POSIX ACL-a, svaki korisnik povezan je s "primarna grupa". Na primjer, korisnik "alice" možda pripadaju grupi "financije". Alice možda nalaze u više grupa, ali jedne grupe uvijek je označen kao svoj primarni grupe. U POSIX, grupi vlasnički te datoteke Alice stvara datoteku, postavljen se na svoj primarni grupa, koji u ovom je slučaju "financije".
 
Prilikom stvaranja nove stavke datotečnom sustavu spremišta podataka Lake dodjeljuje vrijednost vlasnički grupu. 

* **Slučaja 1** - korijensku mapu "/". Ova mapa stvorena prilikom stvaranja računa za spremište Lake podataka. U ovom slučaju grupi vlasnički postavljen je na korisnika koji je stvorio račun.
* **Slučaja 2** (svaka druge sanduk) – prilikom stvaranja nove stavke, grupi vlasnički kopirana nadređenu mapu.

Vlasnički grupe mogu mijenjati:
* Svi super-korisnici
* Vlasnički korisnika, ako je korisnik vlasnički i član grupe cilj.

## <a name="access-check-algorithm"></a>Algoritam potvrdite programa Access

Na sljedećoj slici predstavlja algoritam Potvrdite pristup za račune spremišta Lake podataka.

![Algoritam spremišta podataka Lake ACL-a](./media/data-lake-store-access-control/data-lake-store-acls-algorithm.png)


## <a name="the-mask-and-effective-permissions"></a>Maske za unos i "važeće dozvole"

**Maske za unos** je RWX vrijednost koja se koristi za ograničavanje pristupa za **pod nazivom korisnika**, **vlasnik grupe**i **pod nazivom grupe** prilikom izvršavanja algoritam pristupa potvrdite. Evo koncepata maske. 

* Maska stvara "važeće dozvole", odnosno mijenja dozvole u trenutku provjerite pristup.
* Maska možete izravno uređivati vlasnik datoteke i super-korisnici.
* Maska ima mogućnost da biste uklonili dozvole za stvaranje učinkovite dozvola. Dozvole za maske za unos **ne možete** dodati važeće dozvole. 

Javite nam pogledajte primjere. U nastavku maska postavljen je na **RWX**, što znači da maska ukloniti sve dozvole. Obratite pozornost na to da važeće dozvole za imenovani korisnika, vlasnički grupa i imenovani grupe ne mijenja prilikom provjere programa access.

![Pohrana podataka Lake ACL-a](./media/data-lake-store-access-control/data-lake-store-acls-mask-1.png)

U primjeru u nastavku maska postavljen je na **R-X**. Stoga je **isključuje dozvolu za pisanje** za **pod nazivom korisnika**, **vlasnik grupe**i **pod nazivom grupe** u trenutku pristupa potvrdite.

![Pohrana podataka Lake ACL-a](./media/data-lake-store-access-control/data-lake-store-acls-mask-2.png)

Za referencu, Evo gdje maska za datoteku ili mapu pojavit će se na portalu za Azure.

![Pohrana podataka Lake ACL-a](./media/data-lake-store-access-control/data-lake-store-show-acls-mask-view.png)

>[AZURE.NOTE] Za novi račun za pohranu podataka Lake maske za unos za pristup ACL i zadani ACL korijenske mape ("/") su po zadanom se unose u RWX.

## <a name="permissions-on-new-files-and-folders"></a>Dozvole za nove datoteke i mape

Stvaranja nove datoteke ili mape u odjeljku postojeću mapu, određuje se ACL zadani na nadređenu mapu:

* Zadani ACL podređene mape i ACL programa Access
* Podređeni datoteke Access ACL (datoteke imaju ACL za zadano)

### <a name="a-child-file-or-folders-access-acl"></a>Podređeni datoteke ili mape programu Access ACL

Stvaranja podređeni datoteku ili mapu, nadređeni zadani ACL se kopira kao podređeni datoteke ili mape programu Access ACL. Osim toga, ako **drugi** korisnik ima dozvole RWX u zadani nadređene ACL, ga je potpuno uklonjena iz ACL pristup podređene stavke.

![Pohrana podataka Lake ACL-a](./media/data-lake-store-access-control/data-lake-store-acls-child-items-1.png)

U većini slučajeva, gore navedenih podataka je sve potrebno što trebate znati kako se određuje ACL pristup podređene stavke. Međutim, ako ste upoznati sa sustavima POSIX i želite da biste shvatili detaljnije kako se postiže ovu transformaciju, potražite u odjeljku [uloge Umask korisnika u stvaranju ACL pristup za nove datoteke i mape](#umasks-role-in-creating-the-access-acl-for-new-files-and-folders) u nastavku ovog članka.
 

### <a name="a-child-folders-default-acl"></a>Zadani ACL podređenih mapa

Podmapi stvaranja u odjeljku nadređenu mapu ACL zadani nadređenu mapu kopirali iznad, kao što je, zadani ACL podređene mape.

![Pohrana podataka Lake ACL-a](./media/data-lake-store-access-control/data-lake-store-acls-child-items-2.png)

## <a name="advanced-topics-for-understanding-acls-in-data-lake-store"></a>Dodatne teme za razumijevanje ACL-a u spremištu Lake podataka

Slijede nekoliko dodatne teme pomoću kojih se objašnjava kako ACL-a ovise o spremišta podataka Lake datoteke i mape.

### <a name="umasks-role-in-creating-the-access-acl-for-new-files-and-folders"></a>Uloga Umask korisnika u stvaranju ACL pristup za nove datoteke i mape

U sustavu za POSIX usklađen Općenito pojam je taj umask 9-bitna vrijednost na nadređenu mapu koja se koristi za pretvorbu dozvole za **korisnik vlasnik**, **vlasnik grupe**i **druge** na nove podređene datoteke ili mape programu Access ACL. Bitova na umask odredite koji bitova da biste isključili u ACL pristup podređene stavke. Stoga je koriste se za selektivno spriječiti prijenos dozvole za korisnik, vlasnik grupe, vlasnik i druge.
  
U sustavu za HDFS, u umask obično je mogućnost konfiguracija cijelo web-mjesto koje upravljaju administratori. Spremište Lake podataka koristi se **umask razini račun** koji nije moguće promijeniti. Sljedeća tablica prikazuje podatke Lake spremište umask.

| Grupa korisnika  | Postavka | Učinak na ACL nove podređene stavke programa Access |
|------------ |---------|---------------------------------------|
| Korisnik vlasnik | ---     | Nema učinka                             |
| Vlasnik grupe| ---     | Nema učinka                             |
| Drugi       | RWX     | Uklanjanje čitanje + pisanje + izvršiti         | 

Sljedeća ilustracija prikazuje ova umask u akciji. Neto efekt je da biste uklonili **čitati + pisanje + izvršavanje** **drugih** korisnika. Budući da u umask niste naveli bitova **korisnik vlasnik** i **vlasnik grupe**, te dozvole se su transformacije.

![Pohrana podataka Lake ACL-a](./media/data-lake-store-access-control/data-lake-store-acls-umask.png) 

### <a name="the-sticky-bit"></a>Ljepljive bitne

Ljepljive bit je naprednija značajka datotečnom sustavu POSIX. U kontekstu spremišta Lake podataka, vjerojatno će biti potrebno ljepljive bitne je.

U sljedećoj tablici prikazane funkcioniranje ljepljive bitne u spremištu Lake podataka.

| Grupa korisnika         | Datoteka    | Mapa |
|--------------------|---------|-------------------------|
| Ljepljive bitne **ISKLJUČENO** | Nema učinka   | Nema učinka           |
| Ljepljive bitne **Dalje**  | Nema učinka   | Svi osim **super-korisnika** i na **korisnik vlasnik** podređene stavke iz brisanje ili preimenovanje tu stavku podređeni sprječava.               |

Ljepljive bitne se ne prikazuje na portalu za Azure.

## <a name="common-questions-for-acls-in-data-lake-store"></a>Najčešća pitanja za ACL-a u spremištu Lake podataka

Evo neka pitanja koja se često javiti vezana uz ACL-a u spremištu Lake podataka.

### <a name="do-i-have-to-enable-support-for-acls"></a>Imati da biste omogućili podršku za ACL-a?

ne. Kontrola pristupa putem ACL-a uvijek je na računa spremišta Lake podataka.

### <a name="what-permissions-are-required-to-recursively-delete-a-folder-and-its-contents"></a>Potrebne dozvole su potrebne za rekurzivno izbrisati mapu i njezin sadržaj?

* Nadređenu mapu, morate imati **Pisanje + izvršiti**.
* Mapa će se izbrisati i svake mape u njoj, potreban je **pročitati + pisanje + izvršiti**.
>[AZURE.NOTE] Brisanje datoteka u mape zahtijeva unos s tim datotekama. Osim toga, korijensku mapu "/" **Nikad ne** mogu se izbrisati.

### <a name="who-is-set-as-the-owner-of-a-file-or-folder"></a>Koji je postavljen kao vlasnik datoteke ili mape?

Alat za stvaranje datoteke ili mape postaje vlasnik.

### <a name="who-is-set-as-the-owning-group-of-a-file-or-folder-at-creation"></a>Koja je postavljena kao vlasnički skupine datoteku ili mapu stvaranja?

Kopira se iz grupi vlasnički nadređenu mapu u kojoj se stvara novu datoteku ili mapu.

### <a name="i-am-the-owning-user-of-a-file-but-i-dont-have-the-rwx-permissions-i-need-what-do-i-do"></a>Nisam vlasnički korisnika datoteke, ali nemam dozvole RWX moram. Što učiniti?

Vlasnički korisnik može jednostavno promijeniti dozvole datoteke da bi se dobilo same sve RWX dozvole koje su im potrebne.

### <a name="does-data-lake-store-support-inheritance-of-acls"></a>Podržava li spremišta podataka Lake nasljeđivanje ACL-a?

ne.

### <a name="what-is-the-difference-between-mask-and-umask"></a>Koja je razlika između maske za unos i umask?

| maske za unos | umask|
|------|------|
| Svojstvo **Maska** dostupna na svakoj datoteka i mapa. | **Umask** je svojstvo računa spremišta Lake podataka. Stoga je samo jedan umask Lake spremišta podataka.    |
| Svojstvo maska na datoteku ili mapu možete promijeniti vlasnički korisnika ili vlasnički grupe datoteke ili super-korisnika. | Svojstvo umask nije moguće mijenjati svaki korisnik, čak i super korisnik. Nije nepromjenjiv, konstante vrijednost.|
| Svojstvo maska koristi se za tijekom algoritam pristup provjerite prilikom izvođenja da biste odredili ima li korisnik desno da biste izvršili operaciju na datoteku ili mapu. Da biste stvorili "važeće dozvole" u trenutku Provjera pristupa je uloga maske. | U umask ne koristi tijekom pristupa potvrdite uopće. Na umask koristi se za određivanje ACL pristup novih stavki podređene mape. |
| Maska je vrijednost 3-bitni RWX koji se primjenjuje na imenovane korisnika, imenovani grupa i vlasnički korisnika u trenutku Provjera pristupa.| U umask je 9 bitna vrijednost koja se odnosi na vlasnički korisnika, vlasnički grupe i drugi novi podređeni.| 

### <a name="where-can-i-learn-more-about-posix-access-control-model"></a>Gdje mogu saznati više o model kontrole pristupa POSIX?

* [http://www.vanemery.com/Linux/ACL/POSIX_ACL_on_Linux.HTML](http://www.vanemery.com/Linux/ACL/POSIX_ACL_on_Linux.html)

* [Vodič za HDFS dozvola](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html) 

* [NAJČEŠĆA PITANJA VEZANA UZ POSIX](http://www.opengroup.org/austin/papers/posix_faq.html)

* [POSIX 1003.1 2008](http://standards.ieee.org/findstds/standard/1003.1-2008.html)

* [POSIX 1003.1e 1997](http://users.suse.com/~agruen/acl/posix/Posix_1003.1e-990310.pdf)

* [POSIX ACL Linux](http://users.suse.com/~agruen/acl/linux-acls/online/)

* [Korištenje popisa za kontrolu pristupa na Linux ACL-a](http://bencane.com/2012/05/27/acl-using-access-control-lists-on-linux/)

## <a name="see-also"></a>Vidi također

* [Pregled Lake spremišta podataka za Azure](data-lake-store-overview.md)

* [Početak rada s analize Lake Azure podataka](../data-lake-analytics/data-lake-analytics-get-started-portal.md)





