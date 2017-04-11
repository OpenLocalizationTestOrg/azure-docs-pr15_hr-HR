<properties 
    pageTitle="Početak rada s poslužiteljem Azure višestruke provjere autentičnosti"
    description="To je stranica za provjeru autentičnosti Azure višestruke provjere koji opisuje kako započeti s poslužiteljem Azure MFA."
    services="multi-factor-authentication"
    keywords="provjeru autentičnosti poslužitelja, azure višestruki faktor provjere autentičnosti aktivaciju stranica aplikacije, preuzimanje provjeru autentičnosti poslužitelja"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="kgremban"/>

# <a name="getting-started-with-the-azure-multi-factor-authentication-server"></a>Početak rada s poslužiteljem Azure višestruke provjere autentičnosti




<center>![Oblak](./media/multi-factor-authentication-get-started-server/server2.png)</center>

Nakon što smo Utvrdite li koristiti lokalne višestruka provjera autentičnosti, recimo brz početak. Ova stranica opisuje novu instalaciju poslužitelja i početak postavljanje s lokalnim servisom Active Directory. Ako već imate poslužitelja PhoneFactor instaliran i traže da biste nadogradili, potražite u članku [Upgrading poslužitelj za Azure multi-factor](multi-factor-authentication-get-started-server-upgrade.md) ili ako tražite informacije o instaliranju samo web-servisa potražite u članku [Implementacija Azure višestruku provjeru autentičnosti poslužitelja mobilne aplikacije web-servisa](multi-factor-authentication-get-started-server-webservice.md).


## <a name="download-the-azure-multi-factor-authentication-server"></a>Preuzimanje Server Azure višestruka provjera autentičnosti



Postoje dva načina možete preuzeti Azure višestruku provjeru autentičnosti poslužitelja. Oba gotovi se putem portala za Azure. Prvi je upravljanjem davatelja provjere autentičnosti višestruku izravno. Drugi je putem postavke servisa. Druga mogućnost potrebna je multi-factor davatelja provjere autentičnosti ili licencu za Azure MFA, Azure AD Premium ili paket mobilnost za tvrtke.


### <a name="to-download-the-azure-multi-factor-authentication-server-from-the-azure-portal"></a>Da biste preuzeli server Azure višestruke provjere autentičnosti na portalu za Azure
--------------------------------------------------------------------------------

1. Prijavite se na Portal za Azure kao Administrator.
2. Na lijevoj strani odaberite servisa Active Directory.
3. Na stranici servisa Active Directory, pri vrhu kliknite **Davatelji provjere autentičnosti višestruke provjere**
4. Pri dnu kliknite **Upravljanje**
5. Otvorit će se nova stranica.  Kliknite **preuzimanja.** 
 ![Preuzimanje](./media/multi-factor-authentication-sdk/download.png)
6. Iznad **Generiranje vjerodajnice za aktivaciju**kliknite **Preuzmite.** 
 ![Preuzimanje](./media/multi-factor-authentication-get-started-server/download4.png)
7. Spremite preuzimanje.



### <a name="to-download-the-azure-multi-factor-authentication-server-via-the-service-settings"></a>Da biste preuzeli Azure višestruku provjeru autentičnosti poslužitelja putem postavke servisa


1. Prijavite se na Portal za Azure kao Administrator.
2. Na lijevoj strani odaberite servisa Active Directory.
3. Dvaput pritisnite instanci programa Azure AD.
4. Pri vrhu kliknite **Konfiguriraj**
![preuzimanje](./media/multi-factor-authentication-sdk/download2.png)
5. U odjeljku višestruka provjera autentičnosti odaberite **Upravljanje postavkama servisa**
6. Na stranici Postavke servisa pri dnu zaslona kliknite **Idi na portal**.
![Preuzimanje](./media/multi-factor-authentication-get-started-server/servicesettings.png)
7. Otvorit će se nova stranica. Kliknite **preuzimanja.**
8. Iznad **Generiranje vjerodajnice za aktivaciju**kliknite **Preuzmite.**
9. Spremite preuzimanje.




## <a name="install-and-configure-the-azure-multi-factor-authentication-server"></a>Instaliranje i konfiguriranje poslužitelja za Azure višestruka provjera autentičnosti
Sad kad ste preuzeli server može instalirati i konfigurirati.  Provjerite ispunjava li poslužitelj ga instalirate na sljedeće preduvjete:



Preduvjeti za Server Azure višestruka provjera autentičnosti|Opis|
:------------- | :------------- |
Hardvera|<li>200 MB prostora na tvrdom disku</li><li>x32 ili x64 koje je moguće povezati procesor</li><li>1 GB ili veća RAM-a</li>
Softver|<li>Windows Server 2008 ili noviji ako je na računalo koje hostira poslužitelja OS</li><li>Windows 7 ili noviji ako je na računalo koje hostira klijent OS</li><li>4.0 za Microsoft .NET Framework</li><li>IIS 7.0 ili noviji ako instalacije portal za korisnika ili web-servisa SDK</li>

### <a name="azure-multi-factor-authentication-server-firewall-requirements"></a>Preduvjeti za vatrozid Azure višestruke provjere autentičnosti poslužitelja
--------------------------------------------------------------------------------
Svaki poslužitelj MFA moraju imati mogućnost za komunikaciju na priključak 443 izlaznog ovako:

- https://pfd.phonefactor.NET
- https://pfd2.phonefactor.NET
- https://CSS.phonefactor.NET

Ako izlazni vatrozidima su ograničeni na priključak 443, sljedeće rasponi IP adresa morat ćete otvoriti:

IP podmreže|Maska mreže|IP raspona
:------------- | :------------- | :------------- |
134.170.116.0/25|255.255.255.128|134.170.116.1 – 134.170.116.126
134.170.165.0/25|255.255.255.128|134.170.165.1 – 134.170.165.126
70.37.154.128/25|255.255.255.128|70.37.154.129 – 70.37.154.254

Ako ne koristite značajke Azure višestruke provjere autentičnosti događaj potvrdu i ako korisnici nisu provjere autentičnosti putem provjere višestruke provjere autentičnosti aplikacija za mobilne uređaje s uređaja na mreži tvrtke na IP rasponi mogu se smanjiti ovako:


IP podmreže|Maska mreže|IP raspona
:------------- | :------------- | :------------- |
134.170.116.72/29|255.255.255.248|134.170.116.72 – 134.170.116.79
134.170.165.72/29|255.255.255.248|134.170.165.72 – 134.170.165.79
70.37.154.200/29|255.255.255.248|70.37.154.201 – 70.37.154.206


### <a name="to-install-and-configure-the-azure-multi-factor-authentication-server"></a>Da biste instalirali i konfigurirati poslužitelj za Azure višestruka provjera autentičnosti
--------------------------------------------------------------------------------


1. Dvokliknite izvršnu datoteku. Započet će instalaciju.
2. Na zaslonu odaberite instalacijsku mapu provjeriti je li on točan mapu, a zatim kliknite Dalje.
3. Kada dovršite instalaciju, kliknite Završi.  To će se pokrenuti čarobnjak za konfiguraciju.
4. U čarobnjaku za konfiguraciju zaslon dobrodošlice, potvrdite **Preskoči pomoću čarobnjaka za konfiguraciju provjeru autentičnosti** , a zatim kliknite **Dalje**.  To će zatvorite čarobnjak i pokrenite poslužitelj.
    ![Oblak](./media/multi-factor-authentication-get-started-server/skip2.png)
5. Natrag na stranici koje ćemo preuzimaju poslužiteljem putem kliknite gumb za **Generiranje vjerodajnice za aktivaciju** .  Ove podatke kopirajte u MFA Server Azure u odgovarajuće okvire i kliknite **Aktiviraj**.


Gore navedeni koraci Prikaži eksplicitnih postavljanje pomoću čarobnjaka za konfiguraciju.  Možete ponovno pokrenuti čarobnjak za provjeru autentičnosti odabirom na izborniku Alati na poslužitelju.



##<a name="import-users-from-active-directory"></a>Uvoz korisnika iz servisa Active Directory

Sad kad je poslužitelj instalacije i konfiguracije možete brzo uvesti korisnici Azure MFA Server.

### <a name="to-import-users-from-active-directory"></a>Da biste uvezli korisnika iz servisa Active Directory
--------------------------------------------------------------------------------


1. MFA Server Azure, na lijevoj strani odaberite **korisnici**.
2. Pri dnu, odaberite **Uvoz iz servisa Active Directory**.
3. Sada možete pretraživati za pojedinačne korisnike ili pretraživanje imenika AD za organizacijske jedinice s korisnicima u njima.  U ovom slučaju ne možemo navedite korisnike Organizacijska Jedinica.
4. Istaknite svim korisnicima na desnoj strani, a zatim kliknite **Uvezi**.  Trebali biste dobiti skočni prozor koja vas obavještava da ste uspješno.  Zatvorite prozor uvoz.

![Oblak](./media/multi-factor-authentication-get-started-server/import2.png)

## <a name="send-users-an-email"></a>Korisnicima poslati poruku e-pošte
Sad kad korisnici ste uvezli na poslužitelj za Azure višestruke provjere autentičnosti, ga je svakako koje šaljete korisnici poruke e-pošte koja ih obavještava da su ste su se prijavili višestruke provjere autentičnosti.

S poslužiteljem Azure višestruke provjere autentičnosti postoje razni načini konfiguriranje korisnika za korištenje višestruke provjere autentičnosti.  Na primjer, ako znate telefonskih brojeva korisnika ili nije moguće uvesti telefonski brojevi Azure višestruku provjeru autentičnosti poslužitelja iz imenika svoje tvrtke, e-pošte poslat će korisnici znali da ih nije konfiguriran pomoću Azure višestruke provjere autentičnosti, sadrže neke upute o korištenju Azure višestruke provjere autentičnosti i obavještavanje korisnika telefonski broj te osobe primit će njihove authentications na.  

Sadržaj e-pošte računa ovisi o metoda provjere autentičnosti koja je postavljena za korisnika (primjerice telefonski poziv, SMS, mobilne aplikacije).  Na primjer, ako korisnik mora koristiti PIN-a dok ih provjeru autentičnosti, e-poštu će recite im što njihove početne PIN postavljen da.  Korisnici obično moraju da biste promijenili njihov PIN tijekom njihova prvi provjera autentičnosti.

Ako telefonskih brojeva korisnika nisu konfigurirana ili uvesti na poslužitelj za Azure višestruke provjere autentičnosti ili korisnici su unaprijed konfigurirana za korištenje mobilne aplikacije za provjeru autentičnosti, možete im poslati poruku e-pošte koji omogućuje osobu o tome da ste nije konfiguriran za korištenje Azure višestruke provjere autentičnosti i on će ih da biste dovršili njihove Registracija računa putem portala za Azure višestruke provjere autentičnosti korisnika usmjeriti.  Hiperveza će se uvrstiti da korisnik klikne pristup portalu za korisnika. Prilikom klika na hipervezu, web-pregledniku će otvoriti i odvesti svoje tvrtke Azure višestruke provjere autentičnosti korisnika Portal.   


### <a name="configuring-email-and-email-templates"></a>Konfiguriranje e-pošte i predlošci e-pošte

Klikom na ikonu e-pošte na lijevoj strani možete postaviti postavke za slanje te poruke e-pošte.  Ovo je gdje možete unijeti podatke SMTP poslužitelja za poštu i omogućuje slanje na okvirnog širine e-pošte tako da dodate provjere slanje pošte korisnika potvrdni okvir.

![Postavke e-pošte](./media/multi-factor-authentication-get-started-server/email1.png)

Na kartici sadržaja e-pošte, vidjet ćete sve različite predloške e-pošte koji su dostupni za odabir.  Ovisno o tome kako ste konfigurirali da korisnici upotrebljavaju višestruka provjera autentičnosti, možete odabrati predložak te najbolje vama odgovara.

![Predlošci e-pošte](./media/multi-factor-authentication-get-started-server/email2.png)

## <a name="how-the-azure-multi-factor-authentication-server-handles-user-data"></a>Način postupanja s poslužiteljima za provjeru autentičnosti Azure multi-factor korisničkih podataka

Kada koristite na višestruke provjere autentičnosti (MFA) informacije o lokalnom poslužitelju, korisničke podaci se pohranjuju u lokalne poslužitelje. Stalni korisničkih podataka pohranjen u oblaku. Kad korisnik izvodi dvostruka provjera autentičnosti, MFA poslužitelj šalje podataka u oblaku Azure MFA da biste izvršili provjeru autentičnosti. Kada te zahtjeve za provjeru autentičnosti šalju se sa servisom cloud, sljedeća polja šalju se u zahtjev i zapisnika tako da su dostupne u izvješćima za provjeru autentičnosti i korištenje klijenta. Neka polja nisu obavezna tako da ga omogućiti ili onemogućiti unutar višestruke provjere autentičnosti poslužitelja. Komunikacija s poslužiteljem MFA u oblaku MFA koristi SSL/TLS priključak 443 izlaznog. Ta polja nisu:

- Jedinstveni ID - korisničko ime ili ID poslužitelja za interne MFA
- Prvi i prezime - neobavezno
- Adresa e-pošte – neobavezno
- Telefonski broj - pri izvođenju govorni poziv ili SMS provjere autentičnosti
- Uređaj token – kada način provjere autentičnosti u mobilnoj aplikaciji
- Način provjere autentičnosti
- Provjera autentičnosti rezultata
- Naziv poslužitelja za MFA
- Poslužitelj za MFA IP
- Klijent IP – ako takvi postoje



Uz iznad polja rezultata provjere autentičnosti (Uspjeh na uskraćivanje) i razlog za sve denials je spremljena s podataka za provjeru autentičnosti i dostupne putem provjere autentičnosti i korištenje izvješća.


## <a name="advanced-azure-multi-factor-authentication-server-configurations"></a>Dodatna konfiguracija Server Azure višestruka provjera autentičnosti
Dodatne informacije o Napredno postavljanje i informacije o konfiguraciji u sljedećoj tablici.

Način|Opis
:------------- | :------------- |
[Portal za korisnika](multi-factor-authentication-get-started-portal.md)|  Informacije o instalaciju i konfiguriranje korisnički portal uključujući implementacije i korisnik samostalno.
[Servisa Active Directory Federation](multi-factor-authentication-get-started-adfs.md)|Informacije o postavljanju višestruka provjera autentičnosti Azure AD fs.
[Provjera autentičnosti POLUMJER](multi-factor-authentication-get-started-server-radius.md)|  Informacije o instalaciju i konfiguriranje Azure MFA Server s POLUMJER.
[Provjera autentičnosti IIS](multi-factor-authentication-get-started-server-iis.md)|Informacije o instalaciju i konfiguriranje Azure MFA Server s IIS-om.
[Provjera autentičnosti sustava Windows](multi-factor-authentication-get-started-server-windows.md)|  Informacije o instalaciju i konfiguriranje Azure MFA Server pomoću provjere autentičnosti sustava Windows.
[Provjera autentičnosti LDAP](multi-factor-authentication-get-started-server-ldap.md)|Informacije o instalaciju i konfiguriranje Azure MFA Server s provjerom autentičnosti LDAP.
[Udaljene radne površine pristupnik i pomoću POLUMJER na poslužitelj za Azure višestruke provjere autentičnosti](multi-factor-authentication-get-started-server-rdg.md)|  Informacije o instalaciju i konfiguriranje Azure MFA Server s udaljene radne površine pristupnika pomoću POLUMJER.
[Sinkronizacija s Windows Server Active Directory](multi-factor-authentication-get-started-server-dirint.md)|Informacije o instalaciju i konfiguriranje sinkronizacije između servisa Active Directory i poslužitelj za Azure MFA.
[Implementacija Azure višestruka provjera autentičnosti poslužitelja mobilne aplikacije web-usluge](multi-factor-authentication-get-started-server-webservice.md)|Informacije o instalaciju i konfiguriranje web-servisa server Azure MFA.
