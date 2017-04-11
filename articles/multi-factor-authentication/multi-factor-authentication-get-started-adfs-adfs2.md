<properties
    pageTitle="Korištenje poslužitelja Azure MFA AD fs 2.0 | Microsoft Azure"
    description="To je stranica Azure višestruke provjere autentičnosti koji opisuje način za početak rada s Azure MFA i AD FS 2.0."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/14/2016"
    ms.author="kgremban"/>

# <a name="secure-cloud-and-on-premises-resources-using-azure-multi-factor-authentication-server-with-ad-fs-20"></a>Zaštita oblaka i lokalnog resursa pomoću Azure višestruke provjere autentičnosti poslužitelja za AD FS 2.0

Ovaj se članak odnosi tvrtkama ili ustanovama koje povezani Azure Active Directory, a želite sigurne resurse koji su na lokalni ili u oblaku. Zaštita resursa pomoću provjere autentičnosti poslužitelja za višestruku Azure i konfigurirati za rad sa servisa AD FS tako da se dva koraka potvrdu aktivira za krajnje točke visoke vrijednosti.

Ovo je dokumentacija pokriva pomoću provjere autentičnosti poslužitelja za višestruku Azure AD FS 2.0.  Dodatno se informirajte o [Zaštita oblaka lokalnog resursa i korištenje Azure višestruke provjere autentičnosti poslužitelja sustava Windows Server 2012 R2 AD fs](multi-factor-authentication-get-started-adfs-w2k12.md).


## <a name="secure-ad-fs-20-with-a-proxy"></a>Sigurne AD FS 2.0 s proxy poslužitelj
Sigurnost AD FS 2.0 s proxy poslužitelj, kliknite pločicu na poslužitelj za Azure višestruku provjeru autentičnosti proxy poslužitelj za ADFS i konfiguriranje poslužitelja.

### <a name="configure-iis-authentication"></a>Konfiguriranje provjere autentičnosti IIS

1. Unutar poslužitelja Azure višestruke provjere autentičnosti, kliknite ikonu **IIS provjere autentičnosti** na lijevom izborniku.
2. Kliknite karticu **Utemeljene na obrascu** .
3. Kliknite na **dodatnoj..** gumb.
<center>![Postavljanje](./media/multi-factor-authentication-get-started-adfs-adfs2/setup1.png)</center>
4. Automatsko prepoznavanje korisničko ime, lozinku i domenu varijable, unesite URL prijavu (kao što je https://sso.contoso.com/adfs/ls) dijaloški okvir Auto-Configure Form-Based web-mjesta i kliknite u redu.
5. Potvrdite okvir **odgovaraju zahtijevaju Azure višestruka provjera autentičnosti korisnika** ako svi korisnici su ili će biti uvezeni u poslužitelj i predmet potvrdu dva koraka. Ako značajan broj korisnika još nisu uvezeni u poslužitelja ili bit će izuzeti iz provjere dva koraka, ostavite okvir poništen. Dodatne informacije o toj značajci potražite u članku datoteke pomoći.
6. Ako nije moguće automatski otkriti varijable stranice, kliknite na **Odredite ručno...** gumb u dijaloškom okviru Auto-Configure Form-Based web-mjesta.
7. U dijaloškom okviru Add Form-Based web-mjesta unesite URL na stranicu za prijavu ADFS u polje URL za slanje (kao što je https://sso.contoso.com/adfs/ls) i unesite naziv aplikacije (neobavezno). Naziv aplikacije će se prikazati u izvješćima Azure višestruke provjere autentičnosti i može se prikazati unutar SMS ili mobilnu aplikaciju poruka provjere autentičnosti. Datoteka pomoći za dodatne informacije potražite na URL za slanje.
8. Postavljanje oblika zahtjev za "OBJAVITI ili pak ZATRAŽITE."
9. Unesite korisničko ime varijabla (ctl00$ Rezerviranomjestosadržaja1$ UsernameTextBox) i lozinku varijabla (ctl00$ Rezerviranomjestosadržaja1$ PasswordTextBox). Ako se na stranicu za prijavu utemeljene na obrascu domene tekstni okvir, unesite varijabla domene. Možda ćete morati idite na stranicu za prijavu u web-pregledniku, na stranici, kliknite desnom tipkom miša i odaberite **Prikaži izvor** da biste pronašli imena okvire unosa unutar stranice za prijavu.
10. Potvrdite okvir **odgovaraju zahtijevaju Azure višestruka provjera autentičnosti korisnika** ako svi korisnici su ili će biti uvezeni u poslužitelj i predmet potvrdu dva koraka. Ako značajan broj korisnika još nisu uvezeni u poslužitelja ili bit će izuzeti iz provjere dva koraka, ostavite okvir poništen.
<center>![Postavljanje](./media/multi-factor-authentication-get-started-adfs-adfs2/manual.png)</center>
11. Kliknite **Napredno...** gumb da biste pregledali Napredne postavke. Možete konfigurirati postavke obuhvaća mogućnost da biste odabrali datoteke na stranici prilagođene uskraćivanje predmemoriju uspješno authentications na web-mjesto pomoću kolačiće i odaberite način provjere autentičnosti primarni vjerodajnice.
12. Budući da je proxy poslužitelj za ADFS vjerojatno koji se spajaju domeni, možete koristiti LDAP povezati kontroler domene za uvoz korisnika i prije provjere autentičnosti. U dijaloškom okviru Advanced Form-Based web-mjesta kliknite karticu **Primarni provjeru autentičnosti** i odaberite **LDAP povezivanje** vrste provjere autentičnosti prije provjere autentičnosti.
13. Kad završi, kliknite gumb **u redu** da biste se vratili u dijaloški okvir Add Form-Based web-mjesta. Pogledajte datoteke pomoći za dodatne informacije o Napredne postavke.
14. Kliknite gumb **u redu** da biste zatvorili dijaloški okvir.
15. Nakon varijable URL i stranice su otkrivena ili unijeli, podataka web-mjesta prikazat će se na ploči utemeljene na obrascu.
16. Kliknite karticu **Nativni modulu** , a zatim odaberite poslužitelj, web-mjesto koje proxy poslužitelj za ADFS se izvodi u odjeljku (kao što su "zadano Web-mjesto") ili proxyja aplikacije ADFS (kao što su "ls" u odjeljku "adfs") da biste omogućili IIS dodatka na željenu razinu.
17. Potvrdite okvir **Omogući IIS provjere autentičnosti** pri vrhu zaslona.
18. Sada je omogućena provjera autentičnosti IIS.

### <a name="configure-directory-integration"></a>Konfiguriranje Integracija direktorija

Omogućeno IIS provjere autentičnosti, ali da biste izvršili prije provjere autentičnosti za vaše Active Directory (AD) putem LDAP morate konfigurirati vezu LDAP s kontrolerom domene.

1. Kliknite ikonu **Integracija direktorija** .
2. Na kartici postavke odaberite izborni gumb **koristi određenu LDAP konfiguraciju** .
<center>![Postavljanje](./media/multi-factor-authentication-get-started-adfs-adfs2/ldap1.png)</center>
3. Kliknite na **Uredi...** gumb.
4. U dijaloškom okviru Uređivanje konfiguracije LDAP popuniti polja podatke potrebne za povezivanje s kontrolerom domene servisa Active Directory. Opisi polja nalaze se u tablici u nastavku. Datoteka pomoći Azure višestruke provjere autentičnosti poslužitelja i obuhvaćen taj podatak.
5. Testiranje veze LDAP tako da kliknete gumb za **testiranje** .
<center>![Postavljanje](./media/multi-factor-authentication-get-started-adfs-adfs2/ldap2.png)</center>
6. Ako Testiranje veze LDAP je uspio, kliknite gumb **u redu** .

### <a name="configure-company-settings"></a>Konfiguriranje postavki za tvrtke

1. Zatim kliknite ikonu **Postavke tvrtke** i odaberite karticu **Razlučivost korisničko ime** .
2. Odaberite izborni gumb **LDAP korištenje atributa Jedinstveni identifikator za odgovarajući korisnička imena** .
3. Ako korisnicima unesite svoje korisničko ime u obliku "domena\korisničkoime", poslužitelj moraju imati mogućnost za uklanjanje domene izvan korisničko ime kada stvara LDAP upit. Koje se mogu izvršiti putem postavku registra.
4. Otvorite uređivač registra i prijeđite na HKEY_LOCAL_MACHINE/softver/Wow6432Node/pozitivnog mreža/PhoneFactor 64-bitni server. Ako je na poslužitelju 32-bitni, potrebno "Wow6432Node" iz put. Stvaranje ključa registra DWORD pod nazivom "UsernameCxz_stripPrefixDomain", a vrijednost postavite na 1. Azure višestruka provjera autentičnosti sada je zaštita proxy poslužitelj za ADFS.

Provjerite je li da korisnici su uvezeni iz servisa Active Directory na poslužitelj. Potražite u [odjeljku pouzdana IP -ovi](#trusted-ips) želite li whitelist interne IP adrese tako da se provjera dva koraka nije potrebna prilikom prijave na web-mjesto s te lokacije.

<center>![Postavljanje](./media/multi-factor-authentication-get-started-adfs-adfs2/reg.png)</center>

## <a name="ad-fs-20-direct-without-a-proxy"></a>AD FS 2.0 izravno bez proxy poslužitelj

Možete zaštititi AD FS kada se ne koristi proxy za AD FS. Instalacija na poslužitelj za Azure multi-factor provjere autentičnosti na poslužitelju za AD FS i konfiguriranje poslužitelja za sljedeće korake:

1. Unutar poslužitelja Azure višestruke provjere autentičnosti, kliknite ikonu **IIS provjere autentičnosti** na lijevom izborniku.
2. Kliknite karticu **HTTP-a** .
3. Kliknite na **dodatnoj..** gumb.
4. U dijaloški okvir Dodavanje osnovni URL unesite URL za ADFS web-mjesta na kojima izvršiti provjeru autentičnosti HTTP (kao što je https://sso.domain.com/adfs/ls/auth/integrated) u polje Osnovni URL. Nakon toga unesite naziv aplikacije (neobavezno). Naziv aplikacije će se prikazati u izvješćima Azure višestruke provjere autentičnosti i može se prikazati unutar SMS ili mobilnu aplikaciju poruka provjere autentičnosti.
5. Po želji, prilagodite neaktivnosti vremenskog ograničenja i najviše puta sesiju.
6. Potvrdite okvir **odgovaraju zahtijevaju Azure višestruka provjera autentičnosti korisnika** ako svi korisnici su ili će biti uvezeni u poslužitelj i predmet potvrdu dva koraka. Ako značajan broj korisnika još nisu uvezeni u poslužitelja ili bit će izuzeti iz provjere dva koraka, ostavite okvir poništen. Dodatne informacije o toj značajci potražite u članku datoteke pomoći.
7. Po želji, potvrdite okvir predmemoriju kolačića.
<center>![Postavljanje](./media/multi-factor-authentication-get-started-adfs-adfs2/noproxy.png)</center>
8. Kliknite gumb **u redu** .
9. Kliknite karticu **Nativni modul** , a zatim odaberite poslužitelj, web-mjesta (kao što su "zadano Web-mjesto") ili aplikaciju za ADFS (kao što je "ls" u odjeljku "adfs") da biste omogućili IIS dodatka na željenu razinu.
10. Potvrdite okvir **Omogući IIS provjere autentičnosti** pri vrhu zaslona. Azure višestruka provjera autentičnosti sada je zaštita ADFS.

Provjerite je li da korisnici su uvezeni iz servisa Active Directory na poslužitelj. U odjeljku pouzdana IP-ovi želite li whitelist interne IP adrese tako da se provjera dva koraka nije potrebna prilikom prijave na web-mjesto s te lokacije.


## <a name="trusted-ips"></a>Pouzdani IP-ovi
Pouzdani IP-ovi korisnicima da biste zaobišli Azure višestruka provjera autentičnosti zahtjeva za web-mjesto s određenim IP adresa ili podmreže za. Ako, na primjer, trebali biste izuzetih korisnika iz dva koraka provjere kada se prijaviti u office. To, želite navesti podmreže office kao pouzdane IP-ovi stavku.

### <a name="to-configure-trusted-ips"></a>Konfiguriranje pouzdanih IP-ovi


1. U odjeljku Provjera autentičnosti IIS karticu **Pouzdana IP -ovi** .
1. Kliknite na **dodatnoj..** gumb.
1. Kada se pojavi dijaloški okvir Dodavanje IP-pouzdana ovi, odaberite jednu od izborne gumbe **Jedan IP**, **IP raspona**ili **podmreže** .
1. Unesite IP adresa, raspon IP adresa ili podmreže koje treba whitelisted. Ako unesete podmreži, odaberite odgovarajući maska mreže i kliknite gumb **u redu** . Pouzdani IP sada dodan.


<center>![Postavljanje](./media/multi-factor-authentication-get-started-adfs-adfs2/trusted.png)</center>
