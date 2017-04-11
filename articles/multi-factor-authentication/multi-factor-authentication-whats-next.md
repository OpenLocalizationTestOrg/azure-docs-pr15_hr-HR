<properties
    pageTitle="Azure višestruka provjera autentičnosti - daljnji koraci"
    description="To je stranica Azure višestruke provjere autentičnosti koji opisuje što učiniti s MFA.  To obuhvaća izvješća, prijevare upozorenja, jednokratni zaobilazak, prilagođene glasovne poruke, predmemoriranja pouzdanih IP-ovi i aplikacije lozinke."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/23/2016"
    ms.author="kgremban"/>

# <a name="configuring-azure-multi-factor-authentication"></a>Konfiguriranje Azure višestruka provjera autentičnosti

Ovaj članak sadrži upravljanje Azure višestruka provjera autentičnosti sad kad ste s radom.  Pokriva različite teme koje olakšavaju korištenje svih pogodnosti Azure višestruke provjere autentičnosti.  Neke od tih značajki dostupnih u svakoj verziji Azure višestruke provjere autentičnosti.

Konfiguracija za neke od značajki ispod nalazi se na portalu za upravljanje Azure višestruke provjere autentičnosti. Postoje dva načina različite kojima možete pristupiti portala za upravljanje MFA koje obje gotovi putem portala za Azure. Prvi je putem upravljanja davatelja provjere autentičnosti višestruku ako koristite utemeljen na potrošnje MFA. Drugi je putem postavke servisa MFA. Druga mogućnost potrebna je multi-factor davatelja provjere autentičnosti ili licencu za Azure MFA, Azure AD Premium ili paket mobilnost za tvrtke.

Da biste pristupili Portal za upravljanje MFA putem davatelja usluge provjere autentičnosti višestruku Azure, prijavite se u Azure portal kao administrator, a zatim odaberite željenu mogućnost servisa Active Directory. Kliknite karticu **Davatelji provjere autentičnosti višestruke provjere** , a zatim odaberite direktorija i kliknite gumb **Upravljanje** pri dnu.

Da biste pristupili Portal za upravljanje MFA putem stranice Postavke servisa MFA, prijavite se u Azure portal kao administrator, a zatim odaberite željenu mogućnost servisa Active Directory. Kliknite na direktorija, a zatim kliknite karticu **Konfiguracija** . U odjeljku višestruka provjera autentičnosti odaberite **Upravljanje postavkama servisa**. Pri dnu stranice Postavke servisa MFA, kliknite vezu **idite na portal** .


Značajka| Opis| Što je obuhvaćeno
:------------- | :------------- | :------------- |
[Upozorenje prijevara](#fraud-alert)|Upozorenje prijevare možete konfigurirati i postaviti tako da korisnici mogu prijaviti lažno pokušava pristupiti svojim resursima.|Upute za postavljanje, konfiguriranje i izvješća prijevara
[Jednokratno zaobilazak](#one-time-bypass) |Jednokratno zaobilazak korisnicima omogućuje provjeru autentičnosti jednom vremenskom po "zaobilaženje" višestruke provjere autentičnosti.|Kako postaviti i konfigurirati jednokratni zaobilazak
[Prilagođeni glasovne poruke](#custom-voice-messages) |Prilagođeni glasovne poruke omogućuju koristite vlastitu snimke ili čestitke s višestruke provjere autentičnosti. |Upute za postavljanje i konfiguriranje prilagođene čestitke i poruke
[Predmemoriranje](#caching-in-azure-multi-factor-authentication)|Predmemoriranje omogućuje vam da biste postavili određeno vremensko razdoblje tako da se prilikom pokušaja sljedeće provjere autentičnosti uspjeti automatski. |Upute za postavljanje i konfiguriranje provjere autentičnosti predmemorije.
[Pouzdani IP-ovi](#trusted-ips)|Pouzdani IP-ovi je značajka servisa višestruke provjere autentičnosti koja omogućuje administratorima upravljanih ili vanjski klijent mogućnost da biste zaobišli višestruke provjere autentičnosti za korisnike koji su prijave u Lokalni intranet tvrtke.|Konfiguriranje i postavljanje IP adresama izuzetih za multi-factor authentication
[Aplikacija lozinki](#app-passwords)|U aplikaciji lozinka omogućuje aplikaciju koja nije MFA-umu zaobići višestruke provjere autentičnosti i nastaviti s radom.|Informacije o lozinkama aplikacije.
[Imajte na umu višestruke provjere autentičnosti za zapamćenih uređaji i preglednici](#remember-multi-factor-authentication-for-devices-users-trust)|Omogućuje vam da ne zaboravite uređaji za postavljanje broj dana nakon korisnik uspješno prijavio pomoću MFA.|Informacije o omogućivanju značajka i postavljanje broj dana.
[Moguće odabrati potvrdu metode](#selectable-verification-methods)|Omogućuje vam da odaberete metoda provjere autentičnosti koje su dostupne korisnicima da biste koristili.|Informacije o omogućivati ili onemogućivati određene metoda provjere autentičnosti kao što je poziv ili SMS poruke.



## <a name="fraud-alert"></a>Upozorenje prijevara
Upozorenje prijevare možete konfigurirati i postaviti tako da korisnici mogu prijaviti lažno pokušava pristupiti svojim resursima.  Korisnici mogu prijaviti prijevare pomoću aplikacije za mobilne uređaje ili putem telefon.

### <a name="to-set-up-and-configure-fraud-alert"></a>Da biste postavili i konfiguriranje upozorenja prijevara

1.  Prijavite se na http://azure.microsoft.com
2.  Otvorite Portal za upravljanje MFA prema uputama pri vrhu ove stranice.
3.  Upravljanje portalu Azure višestruke provjere autentičnosti u odjeljku Konfiguriranje kliknite postavke.
4.  U odjeljku prijevare upozorenja na stranici Postavke potvrdite okvir Dopusti korisnicima slanje upozorenja prijevare potvrdni okvir.
5.  Ako želite da korisnici će biti blokiran kada prijevare prijavljuje, potvrdite korisnika bloka kada prijavljuje prijevara.
6.  U tekstni okvir **Kod za izvješće prijevare tijekom početne pozdrava** unesite broj kod koji se mogu koristiti tijekom poziva provjere. Ako korisnik unese ovaj kod plus # umjesto samo znak #, zatim prijevare upozorenje će se prijavljivati.
7.  Pri dnu, kliknite Spremi.

>[AZURE.NOTE]
>Pozdrava glasovne zadani Microsoftovim uputite korisnike da pritisnu 0# slanje prijevare upozorenja. Ako želite koristiti kod koji nije 0, trebali biste snimiti i prenijeti vlastite prilagođene govorne čestitke s odgovarajućim uputama.


![Oblak](./media/multi-factor-authentication-whats-next/fraud.png)

### <a name="to-report-fraud-alert"></a>Da biste izvješće prijevare upozorenja
Upozorenje prijevare možete prijavio dva načina.  Ili putem aplikacije za mobilne uređaje ili putem telefona.  

### <a name="to-report-fraud-alert-with-the-mobile-app"></a>Da biste izvješće prijevare upozorenja pomoću aplikacije za mobilne uređaje



1. Prilikom provjere se šalje na telefon, odaberite ga da biste pokrenuli aplikaciju Microsoft Authenticator.
2. Da biste izvješće prijevare, kliknite Odustani i prijevare izvješća. Tako ćete prikazati okvir koja vas obavještava da će biti obaviješteni osoblja za IT podršku vaše tvrtke ili ustanove.
3. Kliknite izvješće prijevara.
4. Na aplikaciju, kliknite Zatvori.

![Oblak](./media/multi-factor-authentication-whats-next/report1.png)


![Oblak](./media/multi-factor-authentication-whats-next/fraud2.png)

### <a name="to-report-fraud-alert-with-the-phone"></a>Da biste izvješće prijevare upozorenje s telefona

1. Kada poziv potvrdu isporučuje se s telefonom, odgovarati ga.  
2. Da biste izvješće prijevare, unesite kod koji je konfiguriran odgovaraju izvještaja o prijevare putem telefona, a zatim znak #. Bit ćete obaviješteni da je poslana prijevare upozorenja.
3. Završili poziv.

### <a name="to-view-the-fraud-report"></a>Da biste pogledali izvješće prijevara

1. Prijavite se u [http://azure.microsoft.com](https://azure.microsoft.com/)
2. Na lijevoj strani odaberite servisa Active Directory.
3. Pri vrhu odaberite davatelja usluge provjere autentičnosti višestruke provjere. Tako ćete prikazati popis vašeg davatelja provjere autentičnosti višestruke provjere.
4. Ako imate više od jednog davatelja usluge provjere autentičnosti višestruke provjere, odaberite onu koju želite prikaz izvješća upozorenja prijevare i kliknite Upravljanje pri dnu stranice. Ako imate samo jedan, kliknite Upravljanje. Otvorit će se portala za upravljanje Azure višestruke provjere autentičnosti.
5. Na Azure višestruke provjere autentičnosti portala za upravljanje, na lijevoj strani, u odjeljku prikaz odgovora izvješća kliknite prijevare upozorenja.
6. Odredite raspon datuma koji želite prikazati u izvješću. Također možete odrediti neki određeni korisnička imena, telefonske brojeve i status korisnika.
7. Kliknite Pokreni. Tako ćete prikazati izvješće slično onome u nastavku. Izvoz u CSV možete kliknuti i ako želite izvesti izvješće.

## <a name="one-time-bypass"></a>Jednokratno zaobilazak

Jednokratno zaobilazak korisnicima omogućuje provjeru autentičnosti jednom vremenskom po "zaobilaženje" višestruke provjere autentičnosti. Na zaobilazak privremeni i istječe nakon navedenog broja sekundi.  Da bi se u slučajevima gdje mobilne aplikacije ili telefonski ne prima obavijest ili telefonski poziv možete omogućiti jednokratni zaobilazak tako da korisnik može pristupiti željeni resursa.

### <a name="to-create-a-one-time-bypass"></a>Da biste stvorili jednokratni zaobilazak

1.  Prijavite se na http://azure.microsoft.com
2.  Otvorite Portal za upravljanje MFA prema uputama pri vrhu ove stranice.
3.  U Azure višestruke provjere autentičnosti portala za upravljanje, ako vidite ime klijenta ili davatelja MFA Azure s lijeve strane s na + pokraj njega, kliknite na + potražite u članku različitih grupa replikacije MFA poslužitelj i Azure zadane grupe. Kliknite odgovarajuću grupu.
4.  U odjeljku Administracija korisnika, kliknite **One-Time sadržaje**.
![Oblak](./media/multi-factor-authentication-whats-next/create1.png)
5.  Na stranici One-Time zaobilazak kliknite **Novi One-Time sadržaje**.
6.  Unesite korisničko ime korisnika na željeni broj sekundi koje će se zaobilazak postoje, razlog na zaobilazak i kliknite **zaobići**.
![Oblak](./media/multi-factor-authentication-whats-next/create2.png)
7.  Sada, korisnik mora se prijaviti u prije isteka jednokratni sadržaje.



### <a name="to-view-the-one-time-bypass-report"></a>Da biste prikazali jednokratni zaobilazak izvješće

1. Prijavite se u [http://azure.microsoft.com](https://azure.microsoft.com/)
2. Na lijevoj strani odaberite servisa Active Directory.
3. Pri vrhu odaberite davatelja usluge provjere autentičnosti višestruke provjere. Tako ćete prikazati popis vašeg davatelja provjere autentičnosti višestruke provjere.
4. Ako imate više od jednog davatelja usluge provjere autentičnosti višestruke provjere, odaberite onu koju želite prikaz izvješća upozorenja prijevare i kliknite Upravljanje pri dnu stranice. Ako imate samo jedan, kliknite Upravljanje. Otvorit će se portala za upravljanje Azure višestruke provjere autentičnosti.
5. Na Azure višestruke provjere autentičnosti portala za upravljanje, na lijevoj strani, u odjeljku prikaz odgovora izvješća kliknite One-Time sadržaje.
6. Odredite raspon datuma koji želite prikazati u izvješću. Također možete odrediti neki određeni korisnička imena, telefonske brojeve i status korisnika.
7. Kliknite Pokreni. Tako ćete prikazati izvješće slično onome u nastavku. Izvoz u CSV možete kliknuti i ako želite izvesti izvješće.

<center>![Oblak](./media/multi-factor-authentication-whats-next/report.png)</center>

## <a name="custom-voice-messages"></a>Prilagođeni glasovne poruke

Prilagođeni glasovne poruke omogućuju koristite vlastitu snimke ili čestitke s višestruke provjere autentičnosti.  To može koristiti uz ili da biste zamijenili Microsoft zapise.

Prije nego što počnete Imajte na umu sljedeće:

- Trenutni oblici datoteka podržani su .wav i .mp3.
- Ograničenje veličine datoteke je 5 MB.
- Preporučuje se da za provjeru autentičnosti poruke biti dulji od 20 sekundi. Ništa veće od to može uzrokovati potvrdu uvoza jer je korisnik možda neće reagirati prije Završi poruku i provjera ističe.



### <a name="to-set-up-custom-voice-messages-in-azure-multi-factor-authentication"></a>Da biste postavili Prilagođena glasovne poruke u Azure višestruka provjera autentičnosti
1.  Stvorite prilagođene govornu poruku pomoću jedne od podržanih formata datoteka.
2.  Prijavite se na http://azure.microsoft.com
3.  Otvorite Portal za upravljanje MFA prema uputama pri vrhu ove stranice.
4.  Upravljanje portalu Azure višestruke provjere autentičnosti kliknite porukama govorne pošte u odjeljku Konfiguriranje.
5.  U odjeljku porukama govorne pošte, kliknite **Nova poruka govorne**.
![Oblak](./media/multi-factor-authentication-whats-next/custom1.png)
6.  Na konfiguraciju: nove poruke govorne stranice, kliknite **Upravljanje zvučne datoteke**.
![Oblak](./media/multi-factor-authentication-whats-next/custom2.png)
7.  Na konfiguraciju: zvučne datoteke stranice, kliknite **Prenesi datoteku zvuka**.
![Oblak](./media/multi-factor-authentication-whats-next/custom3.png)
8.  Na konfiguraciju: prijenos zvučne datoteke, kliknite **Pregledaj** i pronađite govornu poruku, kliknite **Otvori**.
![Oblak](./media/multi-factor-authentication-whats-next/custom4.png)
9.  Dodavanje opisa, a zatim kliknite Prenesi.
10. Kada se to završi, poruke potvrđuje uspješno prenijeli datoteku.
11. Na lijevoj strani kliknite porukama govorne pošte.
12. U odjeljku porukama govorne pošte, kliknite Nova poruka govorne.
13. Jezik padajućem popisu, odaberite jezik.
14. Ovo je poruka za određenu aplikaciju, navedite ga u okvir.
15. Vrsta poruke odaberite vrstu poruka zamijene naš novu prilagođenu poruku.
16. Audiodatoteka padajućem popisu, odaberite zvučna datoteka.
17. Kliknite **Stvori**. Poruke potvrđuje uspješno stvorili govornu poruku.
![Oblak](./media/multi-factor-authentication-whats-next/custom5.png)</center>



## <a name="caching-in-azure-multi-factor-authentication"></a>Predmemoriranje u Azure višestruka provjera autentičnosti

Predmemoriranje omogućuje vam da biste postavili određeno vremensko razdoblje tako da se prilikom pokušaja sljedeće provjere autentičnosti uspjeti automatski. Koristi se prvenstveno kada lokalni sustavi kao što je VPN slanje više zahtjeva za potvrdu dok je zahtjev za prvu još u tijeku. Time se omogućuje daljnji zahtjevi da automatski kada korisnik ne uspije provjere valjanosti u tijeku. Imajte na umu da predmemoriranje nije namijenjen će se koristiti za prijavu dodatke za Azure AD.


### <a name="to-set-up-caching-in-azure-multi-factor-authentication"></a>Da biste postavili predmemoriranje u Azure višestruka provjera autentičnosti

1.  Prijavite se na http://azure.microsoft.com
2.  Otvorite Portal za upravljanje MFA prema uputama pri vrhu ove stranice.
3.  Upravljanje portalu Azure višestruke provjere autentičnosti pritisnite Međuspremanje u odjeljku Konfiguriranje.
4.  Na konfiguriranje predmemorije stranice kliknite novu predmemoriju
5.  Odaberite vrstu predmemorije i predmemorije sekundi. Kliknite Stvori.

<center>![Oblak](./media/multi-factor-authentication-whats-next/cache.png)</center>

## <a name="trusted-ips"></a>Pouzdani IP-ovi

Pouzdani IP-ovi je značajka servisa višestruke provjere autentičnosti koja omogućuje administratorima upravljanih ili vanjski klijent mogućnost da biste zaobišli višestruke provjere autentičnosti za korisnike koji su prijave u Lokalni intranet tvrtke. Značajke dostupne su za Azure AD korisnicima koji imaju Azure AD Premium, paket Enterprise mobilnost ili Azure višestruka provjera autentičnosti licenci.


Vrsti klijenta za Azure AD| Dostupne mogućnosti za pouzdane IP
:------------- | :------------- |
Upravljani|Određeni rasponi IP adresa – administratori možete odrediti raspon IP adresa koji se može zaobići višestruke provjere autentičnosti za korisnike koji su prijave u je intranet tvrtke.
Pridružene|<li>Svi vanjski korisnici - svi vanjski korisnici koji su prijave u iz unutar tvrtke ili ustanove može zaobići višestruka provjera autentičnosti pomoću zahtjeva izdala AD FS.</li><li>Određeni rasponi IP adresa – administratori možete odrediti raspon IP adresa koji se može zaobići višestruke provjere autentičnosti za korisnike koji su prijave u je intranet tvrtke.

To zaobići funkcionira samo s unutar tvrtke intraneta. Tako, primjerice, ako samo odabrane svim vanjskim korisnicima i prijave iz izvan tvrtke intranetu, taj korisnik ima za provjeru autentičnosti pomoću višestruka provjera autentičnosti čak i ako se korisnik predstavlja zahtjeva za AD FS. U sljedećoj tablici opisane kad su potrebne unutar vaše corpnet i izvan vaše corpnet višestruke provjere autentičnosti i aplikacije lozinki kada je omogućen pouzdana IP-ovi.


|Pouzdani IP-ovi omogućeno| Pouzdani IP-ovi onemogućena
:------------- | :------------- | :------------- |
Unutrašnji corpnet|Za tijekove preglednika višestruka provjera autentičnosti nije obavezno.|Za tijekove preglednika potrebne višestruka provjera autentičnosti
|Za aplikacije obogaćenog klijenta običnog lozinke funkcioniraju ako korisnik ima stvorili lozinke aplikacije. Nakon što je aplikacija lozinku, potrebni su aplikacije lozinke.|Za aplikacije obogaćenog klijenta, obavezno aplikacije lozinki
Vanjski corpnet|Višestruka provjera autentičnosti, potreban za tijekove preglednika.|Višestruka provjera autentičnosti, potreban za tijekove preglednika.
|Za aplikacije obogaćenog klijenta lozinke aplikacije potreban.|Za aplikacije obogaćenog klijenta lozinke aplikacije potreban.

### <a name="to-enable-trusted-ips"></a>Da biste omogućili pouzdana IP-ovi

1. Prijava na portal Azure klasični.
2. Na lijevoj strani kliknite servisa Active Directory.
3. U odjeljku direktorija kliknite imenik želite postaviti pouzdana IPsing na.
4. Na imenik koje ste odabrali, kliknite Konfiguriraj.
5. U odjeljku višestruka provjera autentičnosti kliknite Postavke servisa za upravljanje.
6. Na stranici Postavke servisa, u odjeljku IP-pouzdana ovi, odaberite nešto od sljedećeg:

    - Za zahtjeve iz vanjskih korisnika koji potječu iz moje intranetu – sve vanjskih korisnika koji su prijave u poslovnoj mreži može zaobići višestruka provjera autentičnosti pomoću zahtjeva izdala AD FS.
    - Za zahtjeve iz određeni raspon javnu IP-ovi – unesite IP adresa u odgovarajuće okvire pomoću CIDR notacije. Na primjer: xxx.xxx.xxx.0/24 za IP adresa u xxx.xxx.xxx.1 raspon – xxx.xxx.xxx.254 ili xxx.xxx.xxx.xxx/32 jedan IP adresa. Možete unijeti do 50 rasponi IP adresa.

7. Kliknite Spremi.
8. Kad su primijenjeni ažuriranja, kliknite Zatvori.



![Pouzdani IP-ovi](./media/multi-factor-authentication-whats-next/trustedips3.png)




## <a name="app-passwords"></a>Aplikacija lozinki

U nekim aplikacijama, kao što je Office 2010 ili stariji i Apple Mail ne možete koristiti višestruke provjere autentičnosti.  Da biste koristili te aplikacije, morat ćete koristiti "aplikacije lozinke" umjesto tradicionalni lozinku.  Lozinka aplikacija omogućuje aplikacije zaobići višestruke provjere autentičnosti i nastaviti s radom.

>[AZURE.NOTE] Moderna provjere autentičnosti za klijente za Office 2013
>
> Klijenti za Office 2013 (uključujući Outlook) sada podržava novi protokole za provjeru autentičnosti i mogu biti omogućene za podršku višestruke provjere autentičnosti.  To znači da kada je omogućen, aplikacija lozinke nisu potrebni za korištenje s klijentskim programima sustava Office 2013.  Dodatne informacije potražite u članku [Moderna provjere autentičnosti javno pretpregleda sustava Office 2013 objaviti](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).



### <a name="important-things-to-know-about-app-passwords"></a>Važno što treba znati o lozinkama aplikacije

Slijedi važne popis stvari koje biste trebali znati o lozinkama aplikacije.

- Korisnici mogu sadržavati više lozinki aplikacije koje se povećava površina za krađu. Jer teško je zapamtiti aplikacije lozinke, možda potaknuli da zapišete to drugi korisnici. Ne preporučuje se i trebali biste discouraged jer je za prijavu pomoću lozinke aplikacije potreban je samo jedan.
- Aplikacije koje predmemoriju lozinke i koristiti u lokalni scenariji započeti ne uspijeva jer lozinku aplikacije nije poznat izvan tvrtke ili ustanove ID-a. Primjer Exchange poruke e-pošte koje su lokalnog dok arhivirane e-pošti je u oblak. Istu lozinku ne funkcionira.
- Stvarni lozinka automatski se generiraju i nije naveden korisnik. To je jer je teže napadaču pogoditi automatski generirani lozinku i sigurnosti.
- Trenutno postoji ograničenje od 40 lozinke po korisniku. Od vas će se zatražiti da biste izbrisali jedan vaše postojeće lozinke aplikacije da biste stvorili novi.
- Kada višestruka provjera autentičnosti omogućena na korisničkom računu, lozinke aplikacije mogu se koristiti s Većina-pregledniku klijentima poput programa Outlook i Lync, ali administratorske akcije koje nije moguće izvesti lozinkama aplikaciju putem aplikacije koje nisu preglednika kao što je Windows PowerShell čak i ako korisnik ima administratorske račun.  Provjerite je li stvoriti račun servisa s jaku lozinku da biste pokrenuli skripte komponente PowerShell i omogućivanje taj račun za višestruke provjere autentičnosti.

>[AZURE.WARNING]  Lozinke aplikacije ne funkcioniraju u hibridnim okruženjima gdje klijenti komunikaciju i lokalne i automatsko otkrivanje krajnje točke u oblaku. To je zato lozinke domene potrebne za provjeru autentičnosti lokalnog i aplikacije lozinke su potrebne za provjeru s oblaka.


### <a name="naming-guidance-for-app-passwords"></a>Imenovanje smjernice za lozinke aplikacije
Preporučuje se nazivi lozinku aplikacije moraju odražavati uređaja na kojem se koriste. Ako, primjerice, imate li prijenosno računalo koje sadrži aplikacije koje nisu preglednika kao što su Outlook, Word i Excel, samo morate stvoriti jednu aplikaciju lozinku pod nazivom prijenosno računalo i koristiti aplikacije lozinku u svim te aplikacije. Iako možete stvoriti zasebnu lozinke za sve te aplikacije, ne preporučuje se. Na preporučeno način je da biste koristili jednu aplikaciju lozinka po uređaja.


<center>![Oblak](./media/multi-factor-authentication-whats-next/naming.png)</center>


### <a name="federated-sso-app-passwords"></a>Pridruženi (SSO) aplikacije lozinki
Azure AD podržava pridruženi lokalnog sustava Windows Server Active Directory Domain Services (AD DS). Ako vaša tvrtka ili ustanova federated(SSO) s Azure AD i namjeravate koristiti Azure višestruka provjera autentičnosti, zatim slijedi važne informacije koje ste trebali imati na umu prilikom korištenja lozinki za aplikacije. Primjenjuje se samo na federated(SSO) klijentima.

- Lozinka aplikacije potvrde Azure AD i zato zaobilazi vanjski pristup. Vanjski pristup samo aktivno koristi prilikom postavljanja aplikacije lozinku.
- Za korisnike federated(SSO), ne možemo nikad ne otvorite davatelja identiteta (IdP) za razliku od pasivni tijek. Lozinke se pohranjuju u polju id tvrtke ili ustanove. Ako korisnik napusti tvrtku, te informacije mora tijeka organizacijske ID-a pomoću DirSync u stvarnom vremenu. Onemogućivanje brisanja račun može proći do tri sata da biste sinkronizirali, što Onemogući/brisanje lozinku aplikacije u Azure AD.
- Postavke za kontrolu pristupa klijenta lokalnog su snazi aplikacije lozinkom
- Provjere autentičnosti lokalnog prijava / nadzora mogućnost dostupna je za aplikaciju lozinke
- Za klijentskog programa Microsoft Lync 2013 potreban je više Obrazovanje krajnjeg korisnika. Potrebni koraci potražite u članku da biste promijenili lozinku u e-poštu u aplikaciju lozinku.
- Za određene napredne arhitektonski dizajna možda ćete morati pomoću kombinacije tvrtke ili ustanove korisničkog imena i lozinke i aplikacije lozinke prilikom korištenja višestruka provjera autentičnosti s klijentima, ovisno o tome gdje su provjeru autentičnosti. Za klijente provjeru autentičnosti infrastruktura za na lokaciji, koristit ćete tvrtke ili ustanove korisničko ime i lozinku. Za klijente provjeru autentičnosti Azure AD, koristit ćete lozinku aplikacije.

Recimo da imate Arhitektura programa koji se sastoji od sljedećeg:

- Su združivanju s pojavljivanja Active Directory s Azure AD na lokaciji
- Koristite Exchange online
- Koristite Lync koji je izričito na lokaciji
- Koristite Azure višestruka provjera autentičnosti


![Proofup](./media/multi-factor-authentication-whats-next/federated.png)

 U ove instance, učinite sljedeće:

- Prilikom prijave u Lync, pomoću korisničkog imena i lozinke vaše tvrtke ili ustanove.
- Pri pokušaju pristupiti adresar putem klijenta za Outlook koja se povezuje sa sustavom Exchange online, koristite za lozinku za aplikacije.

### <a name="allowing-app-password-creation"></a>Dopuštanje stvaranjem aplikacije lozinke
Prema zadanim postavkama, korisnici ne mogu stvarati aplikacije lozinke.  Ta značajka mora biti omogućen.  Da biste korisnicima omogućili mogućnost stvaranja aplikacije lozinki, koristite sljedeći postupak.

#### <a name="to-enable-users-to-create-app-passwords"></a>Omogućivanje korisnicima stvaranje aplikacija lozinke



1. Prijavite se na portal Azure klasični.
2. Na lijevoj strani kliknite servisa Active Directory.
3. U odjeljku direktorija kliknite direktorija za korisnika koje želite omogućiti.
4. Pri vrhu kliknite korisnicima.
5. Pri dnu stranice kliknite Upravljanje višestruke provjere za izradu deklaracije  
6. Pri vrhu stranice višestruka provjera autentičnosti, kliknite Postavke servisa.
7. Provjerite je li odabran izborni gumb pokraj Dopusti korisnicima da biste stvorili lozinke aplikacije da biste se prijavili u aplikacije koje nisu preglednika.


![Stvaranje aplikacije lozinki](./media/multi-factor-authentication-whats-next/trustedips3.png)

### <a name="creating-app-passwords"></a>Stvaranje aplikacije lozinki
Korisnici mogu stvarati aplikacije lozinke tijekom njihova početne registracije.  Dobiju mogućnost na kraju postupka registracije koji im omogućuje da ih stvorite.

Uz to korisnici možete stvoriti aplikaciju lozinke kasnije promjenom njihove postavke na portalu Azure, portal sustava Office 365 ili tako da

### <a name="to-create-app-passwords-in-the-office-365-portal"></a>Da biste stvorili aplikacije lozinke na portalu za Office 365
--------------------------------------------------------------------------------


1. Prijavite se na portal sustava Office 365
2. U gornjem desnom kutu odaberite postavke miniaplikacije
3. Na lijevoj strani odaberite dodatna sigurnost provjere valjanosti
4. Na desnoj strani odaberite **Ažuriraj Moji telefonski brojevi koji se koriste za sigurnost računa**
5. Na stranici proofup na vrhu, odaberite aplikaciju lozinki
6. Kliknite **Stvori**
7. Unesite naziv aplikacije lozinku, a zatim kliknite **Dalje**
8. Kopirajte lozinku aplikacije u međuspremnik i zalijepiti u aplikaciju.

<center>![Oblak](./media/multi-factor-authentication-whats-next/security.png)</center>


### <a name="to-create-app-passwords-in-the-azure-portal"></a>Da biste stvorili aplikacije lozinke na portalu za Azure
--------------------------------------------------------------------------------
1. Prijavite se na portal Azure klasični.
3. U gornjem desnom tipkom miša kliknite korisničko ime, a zatim odaberite dodatna sigurnost provjere.
5. Na stranici proofup na vrhu, odaberite aplikaciju lozinki
6. Kliknite **Stvori**
7. Unesite naziv aplikacije lozinku, a zatim kliknite **Dalje**
8. Kopirajte lozinku aplikacije u međuspremnik i zalijepiti u aplikaciju.


![Aplikacija lozinki](./media/multi-factor-authentication-whats-next/app2.png)

### <a name="to-create-app-passwords-if-you-do-not-have-an-office-365-or-azure-subscription"></a>Da biste stvorili aplikacije lozinke ako nemate pretplatu na Office 365 ili Azure
--------------------------------------------------------------------------------
1. Prijavite se u [https://myapps.microsoft.com](https://myapps.microsoft.com)
2. Pri vrhu odaberite profil.
3. Kliknite svoje korisničko ime i odaberite dodatna sigurnost provjere.
5. Na stranici proofup na vrhu, odaberite aplikaciju lozinki
6. Kliknite **Stvori**
7. Unesite naziv aplikacije lozinku, a zatim kliknite **Dalje**
8. Kopirajte lozinku aplikacije u međuspremnik i zalijepiti u aplikaciju.

![Aplikacija lozinki](./media/multi-factor-authentication-whats-next/myapp.png)

## <a name="remember-multi-factor-authentication-for-devices-users-trust"></a>Imajte na umu višestruke provjere autentičnosti za pouzdanost korisnici uređaja

Višestruka provjera autentičnosti plaćanju za uređaji i preglednici besplatne značajka za sve korisnike MFA je pouzdanost korisnika.  Omogućuje korisnicima mogućnost by-pass MFA za postavljanje broj dana nakon izvršavanja je uspješno prijavu pomoću MFA. Time mogu poboljšati upotrebljivost za korisnike.

No Budući da u korisnici će moći zapamtiti MFA pouzdanih uređaja, ta značajka može smanjiti sigurnost računa. Da biste bili sigurni sigurnost računa, trebali biste vraćanje višestruke provjere autentičnosti za svoje uređaje za bilo koju od sljedećih situacija:

- Ako postaju ugrožena njihove korporativni račun
- Ako izgubite ili krađe zapamćenih uređaja

> [AZURE.NOTE] Značajka je implementirana kao predmemoriju za kolačića u pregledniku. Ne funkcionira ako kolačići preglednika nisu omogućeni.

### <a name="how-to-enabledisable-remember-multi-factor-authentication"></a>Kako omogućiti ili onemogućiti Zapamti višestruka provjera autentičnosti

1. Prijavite se na portal Azure klasični.
2. Na lijevoj strani kliknite servisa Active Directory.
3. U odjeljku servisa Active Directory, kliknite imenik želite postaviti Imajte na umu višestruke provjere autentičnosti za uređaje.
4. Na imenik koje ste odabrali, kliknite Konfiguriraj.
5. U odjeljku višestruka provjera autentičnosti kliknite Postavke servisa za upravljanje.
6. Na stranici Postavke servisa u odjeljku Upravljanje korisničke postavke uređaja, odaberite/poništavanje **Dopusti korisnicima da Imajte na umu višestruke provjere autentičnosti na uređajima oni pouzdanim**.
![Imajte na umu uređaja](./media/multi-factor-authentication-whats-next/remember.png)
8. Postavite broj dana tijekom kojih želite dopustiti u obustavljanje. Zadano je 14 dana.
9. Kliknite Spremi.
10. Kliknite Zatvori.


## <a name="selectable-verification-methods"></a>Moguće odabrati potvrdu metode
Na oblaku i lokalne verzije, možete odabrati su dostupne za korisnike koji metoda provjere. U tablici u nastavku sadrži kratak pregled svake metode.

Kada korisnici registrirati svoje račune za MFA, odaberite ih njihove zeleni list iz mogućnosti koje ste omogućili. Upute za njihov postupka Registracija opisana su u odjeljku [moj račun za potvrdu dva koraka za postavljanje](multi-factor-authentication-end-user-first-time.md)

Način|Opis
:------------- | :------------- |
Poziv na telefon |  Stavlja poziv automatskog telefonskog telefon provjere autentičnosti. Korisnik odgovori na poziv i pritisne # na tipkovnici telefona za provjeru autentičnosti. Ovaj telefonski broj neće sinkronizirati s lokalnim servisom Active Directory.
Tekstne poruke na telefon | Šalje poruku s tekstom koji sadrže kod za potvrdu korisniku. Korisnik je zatražiti da neki odgovori tekstnu poruku s kod za potvrdu ili da biste unijeli kod za potvrdu u sučelje za prijavu.
Obavijest putem aplikacije za mobilne uređaje | U tom načinu aplikaciju Microsoft Authenticator sprječava neovlaštenog pristupa s računima i zaustavlja lažno transakcije. To možete učiniti pomoću automatske obavijesti da biste je telefon ili registrirani uređaj. Samo prikaz obavijesti i ako je valjane dodirnite potvrdi. U suprotnom možete odaberite Nemoj dopustiti ili uskratiti i izvješća lažno obavijesti. Informacije o izvješćivanja lažno obavijesti potražite u članku korištenje Uskrati i značajka prijevare izvješća za višestruke provjere autentičnosti.</br></br>Aplikacija Microsoft Authenticator je dostupne za [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072)i [IOS](http://go.microsoft.com/fwlink/?Linkid=825073).|
Kod za potvrdu iz mobilne aplikacije | U tom načinu aplikaciju Microsoft Authenticator mogu se kao token softver za generiranje programa OATH kod za potvrdu. Kod za potvrdu mogu se unijeti uz korisničko ime i lozinku za drugi obrazac provjere autentičnosti.</li><br><p> Aplikacija Microsoft Authenticator je dostupne za [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072)i [IOS](http://go.microsoft.com/fwlink/?Linkid=825073).

### <a name="how-to-enabledisable-authentication-methods"></a>Kako omogućiti ili onemogućiti metoda provjere autentičnosti

1. Prijavite se na portal Azure klasični.
2. Na lijevoj strani kliknite servisa Active Directory.
3. U odjeljku servisa Active Directory, kliknite imenik želite omogućiti ili onemogućiti metoda provjere autentičnosti.
4. Na imenik koje ste odabrali, kliknite Konfiguriraj.
5. U odjeljku višestruka provjera autentičnosti kliknite Postavke servisa za upravljanje.
6. Na stranici Postavke servisa u odjeljku mogućnosti za potvrdu odaberite/poništavanje odabira mogućnosti koje želite koristiti.</br></br>
![Mogućnosti provjere](./media/multi-factor-authentication-whats-next/authmethods.png)
9. Kliknite Spremi.
10. Kliknite Zatvori.
