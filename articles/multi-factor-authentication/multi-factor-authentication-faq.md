<properties
    pageTitle="Najčešća pitanja vezana uz Azure višestruka provjera autentičnosti"
    description="Daje popis najčešća pitanja i odgovora vezane uz Azure višestruke provjere autentičnosti. Višestruka provjera autentičnosti je metoda potvrđivanja identitet korisnika koje je potrebno više od korisničko ime i lozinku. Pruža dodatne slojeva zaštite korisničku prijavu i transakcije."
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
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="kgremban"/>

# <a name="azure-multi-factor-authentication-faq"></a>Najčešća pitanja vezana uz Azure višestruka provjera autentičnosti


U ovim najčešćim Pitanjima navedeni odgovori na najčešća pitanja o Azure višestruke provjere autentičnosti i putem servisa za višestruke provjere autentičnosti, uključujući pitanja o naplati modela i upotrebljivosti.

## <a name="general"></a>Općenito

**P: kako Azure višestruke provjere autentičnosti poslužitelja rukovati korisničkih podataka?**

S višestruke provjere autentičnosti poslužitelja, korisnički podaci se pohranjuju samo na lokalnim poslužiteljima. Stalni korisničkih podataka pohranjen u oblaku. Kada korisnik radi provjere dva koraka, višestruke provjere autentičnosti poslužitelja šalje podataka u oblaku Azure višestruke provjere autentičnosti za provjeru autentičnosti. Komunikaciju između višestruke provjere autentičnosti poslužitelja i servis u oblaku višestruka provjera autentičnosti priključak 443 izlaznog koristi Secure Sockets Layer (SSL) ili prijenosa sloja sigurnost (TLS).

Kada zahtjeva za provjeru autentičnosti se šalju sa servisom cloud prikupljanja podataka za provjeru autentičnosti i korištenje izvješća. Polja podataka uključeni u dva koraka potvrdu zapisnika su na sljedeći način:

- **Jedinstveni ID** (ili naziv ili na lokalnim višestruku provjeru autentičnosti poslužitelja ID korisnika)
- **Ime i prezime** (neobavezno)
- **Adresa e-pošte** (neobavezno)
- **Broj telefona** (kada se koristi govorni poziv ili provjere autentičnosti SMS)
- **Uređaj tokena** (kad je pomoću provjere autentičnosti u mobilnoj aplikaciji)
- **Način provjere autentičnosti**
- **Provjera autentičnosti rezultata**
- **Naziv poslužitelja višestruka provjera autentičnosti**
- **Višestruka provjera autentičnosti poslužitelja IP**
- **Klijent IP** (Ako je dostupno)

Neobavezna polja moguće je konfigurirati u višestruke provjere autentičnosti poslužitelja.

Provjera rezultat (uspjeh ili uskraćivanje) i razloga ako odbijen, sprema se s podataka za provjeru autentičnosti i dostupna je u provjeru autentičnosti i korištenje izvješća.


## <a name="billing"></a>Naplata

Većina pitanja o naplati možete odgovoriti koje upućuju na [stranici višestruke provjere autentičnosti cijene](https://azure.microsoft.com/pricing/details/multi-factor-authentication/).

**P: moje tvrtke ili ustanove naplatiti telefonske pozive ili poruke s tekstom služi za provjeru autentičnosti korisnike?**

Tvrtke i ustanove se naplatiti za pojedinačne telefonske pozive smjestiti ili tekst poruke poslane korisnicima putem Azure višestruke provjere autentičnosti. Vlasnici telefona možda se naplatiti telefonske pozive ili poruke s tekstom da primaju, prema njihovim osobne telefonska usluga.

**P: ne o naplati modela po korisniku na temelju broj korisnika koji su konfigurirana za korištenje višestruka provjera autentičnosti ili broj korisnika koji izvršavati verifications?**

Naplata temelji se na broj korisnika konfiguriran za korištenje višestruke provjere autentičnosti.

**P: kako funkcionira naplata višestruka provjera autentičnosti**

Kada koristite na "po korisniku" ili "po provjere autentičnosti" modela, Azure MFA je resurs utemeljen na potrošnje. Naknade za bilo koji su naplaćeno Azure pretplate za tvrtke ili ustanove baš kao virtualnih računala, web-mjesta i Dr.

Prilikom korištenja modela licence, licence Azure višestruka provjera autentičnosti su kupili i zatim dodijeljenih korisnicima, baš kao i za Office 365 i druge proizvode pretplate.

**P: postoji besplatnu verziju Azure višestruke provjere autentičnosti za administratore?**

U nekim slučajevima da. Višestruka provjera autentičnosti za administratore Azure nudi podskup Azure MFA značajki nema trošak. Ova se ponuda odnosi članovima grupe Azure globalni administratori u slučajevima Azure Active Directory koje nisu povezane s davateljem Azure višestruke provjere autentičnosti utemeljene na potrošnje. Pomoću davatelja usluga višestruka provjera autentičnosti nadograđuje svim administratorima i korisnicima u direktoriju koji su konfigurirana za korištenje višestruka provjera autentičnosti punu verziju programa Azure višestruke provjere autentičnosti.

**P: postoji besplatnu verziju Azure višestruke provjere autentičnosti za korisnike sustava Office 365?**

U nekim slučajevima da. Višestruka provjera autentičnosti za Office 365 nudi podskup Azure MFA značajki nema trošak. Ova se ponuda odnosi korisnicima koji imaju dodijelili kada davatelja Azure višestruke provjere autentičnosti utemeljene na potrošnje nije povezan s odgovarajućih pojavljivanja Azure Active Directory licencu za Office 365. Pomoću davatelja usluga višestruka provjera autentičnosti nadograđuje svim administratorima i korisnicima u direktoriju koji su konfigurirana za korištenje višestruka provjera autentičnosti punu verziju programa Azure višestruke provjere autentičnosti.

**P: mogu li moje tvrtke ili ustanove prebaciti između po korisniku i po provjere autentičnosti potrošnje naplate modeli u bilo kojem trenutku?**

Vaša tvrtka ili ustanova odabire naplate model kada stvara resursa. Ne možete promijeniti naplate model kada je dodjeli resursa. Međutim, možete stvoriti drugi resurs višestruke provjere autentičnosti da biste zamijenili izvornu. Korisničkih postavki i mogućnosti konfiguracije ne može prenijeti novi resurs.

**P: mogu moje tvrtke ili ustanove prebacivanje između potrošnje naplata i model licence u bilo kojem trenutku?**

Kada se licence dodaju se u direktoriju u kojem je već instalirana davatelja Azure višestruka provjera autentičnosti po korisniku, utemeljen na potrošnje naplata je pala broj licenci vlasništvu. Ako svi korisnici konfiguriran za korištenje višestruka provjera autentičnosti licencama dodijeljenim, administrator možete izbrisati davatelja Azure višestruke provjere autentičnosti.

Ne možete miješati po provjere autentičnosti potrošnje naplate s modelom licence. Kada davatelja višestruka provjera autentičnosti po provjere autentičnosti povezan direktorij, tvrtke ili ustanove naplate za sve zahtjeve potvrdu višestruka provjera autentičnosti, bez obzira na to sve licence vlasništvu.

**P: moje tvrtke ili ustanove mora li koristiti i sinkronizirati identitete koristiti višestruke provjere autentičnosti Azure?**

Kada je tvrtka ili ustanova koristi utemeljen na potrošnje naplate model, Azure Active Directory nije potrebna. Povezivanje višestruka provjera autentičnosti davatelja direktorij nije obavezno. Ako vaša tvrtka ili ustanova nije povezan s direktorij, možete implementirati Azure višestruke provjere autentičnosti poslužitelja ili u Azure višestruke provjere autentičnosti SDK lokalno.

Azure Active Directory za model licencu potreban je Budući da dodaju se imenika licenci kad kupite i dodijeliti ih korisnika u imeniku.


## <a name="usability"></a>Upotrebljivosti

**P: što radi korisnika? ako ih ne dobivate odgovor na njegov telefon ili telefona nije dostupan za korisnika**

Ako korisnik konfigurirao sigurnosne kopije telefona, trebali biste pokušajte ponovno i odaberite tu telefon kada se to od vas zatraži na stranici za prijavu. Ako korisnik nema drugi način konfigurirana, administrator u tvrtki ili ustanovi možete ažurirati dodijeljeni korisnika primarni telefonski broj.


**P: što administrator radi ako korisnik obrati administratoru računa koji više ne mogu pristupiti korisnika?**

Administrator možete vratiti na korisnički račun tako da biste zatražili ponovno prođite kroz postupak registracije. Dodatne informacije o [upravljanju korisnika i postavke uređaja s Azure višestruke provjere autentičnosti u oblaku](multi-factor-authentication-manage-users-and-devices.md).

**P: što administrator radi ako izgubite ili krađe telefona korisnika koji je lozinkama aplikacije?**

Administrator možete izbrisati sve korisničke lozinke aplikacije da biste spriječili neovlašteni pristup. Kada korisnik ima zamjenski uređaj, korisnik možete ponovno stvoriti lozinke. Dodatne informacije o [upravljanju korisnika i postavke uređaja s Azure višestruke provjere autentičnosti u oblaku](multi-factor-authentication-manage-users-and-devices.md).

**P: što se događa ako korisnik ne mogu prijaviti aplikacije koje nisu preglednika?**

Korisnik koji je konfiguriran za korištenje višestruka provjera autentičnosti zahtijeva programa aplikacije lozinku za prijavu na neke se aplikacije koje nisu preglednika. Korisnik mora poništite podatke za prijavu (delete), ponovno pokrenite aplikaciju i prijavite se pomoću korisničkog imena i aplikacije lozinku.

Pronađite dodatne informacije o stvaranju aplikacije lozinke i ostale [Pomoć za aplikacije lozinke](multi-factor-authentication-end-user-app-passwords.md).


>[AZURE.NOTE] Moderna provjere autentičnosti za klijente za Office 2013
>
> Klijenti sustava Office 2013 (uključujući programa Outlook) podržava novi protokole za provjeru autentičnosti. Možete konfigurirati Office 2013 za podršku višestruke provjere autentičnosti. Kada konfigurirate Office 2013, aplikacija lozinke nisu potrebni za klijente za Office 2013. Dodatne informacije potražite u članku [Office 2013 Moderna provjere autentičnosti javno pregled objava](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).

**P: što korisnika radi ako korisnik primati tekstne poruke ili ako korisnik odgovori dvosmjerni tekstnu poruku, ali provjera ističe?**

Održavanje tekst poruke i potvrda odgovora u dvosmjernu SMS se zajamčeno jer je uncontrollable čimbenici koji mogu utjecati na pouzdanosti servisa. Sljedećih čimbenika obuhvaćaju države odredište, prijenosni mobilnog telefona i na signalom.

Korisnici koji se pojave poteškoće prilikom pouzdano primati tekstne poruke odaberite mobilne aplikacije ili telefonskom pozivu metodu umjesto toga. Mobilne aplikacije možete primati obavijesti i na mobilni prijenos podataka i Wi-Fi veze. Osim toga, mobilne aplikacije mogu uzrokovati kodovi za potvrdu čak i kada uređaj ima nema signala uopće. Aplikacija Microsoft Authenticator je dostupne za [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072)i [IOS](http://go.microsoft.com/fwlink/?Linkid=825073).

Ako morate koristiti tekstne poruke, preporučujemo korištenje jednosmjerna SMS umjesto dvosmjerni SMS PORUKE kada je to moguće. Jednosmjerna SMS je više pouzdanog i korisnicima onemogućuje povećavanja globalni SMS troškove iz odgovaranja tekstnu poruku koja je poslana iz druge zemlje.


**P: mogu koristiti hardverski tokeni s Azure višestruke provjere autentičnosti poslužitelja?**

Ako koristite Azure višestruke provjere autentičnosti poslužitelja, Uvoz proizvođača Otvori provjere autentičnosti (OATH) temeljene, Jednokratna lozinke (TOTP) tokeni i ih koristiti radi provjere valjanosti u dva koraka.

Možete koristiti ActiveIdentity tokena koje su OATH TOTP tokeni ako tajnu ključa datoteku smjestite u CSV datoteku, a uvoz Azure višestruke provjere autentičnosti poslužitelja. Tokeni OATH možete koristiti s direktorija vanjski pristup ADFS (Active Services), udaljene provjere autentičnosti biranjem korisnika servisa (POLUMJER) kada klijentskim sustavima može obrađivati odgovore test za pristup i provjeru autentičnosti utemeljenu na obrascima poslužitelja Internet Information (IIS).

Možete uvesti treći dio OATH TOTP tokeni pomoću sljedećih oblika:  
- Prijenosni simetričnu spremnik ključa (PSKC)  
- CSV ako datoteka sadrži serijski broj, tajnu ključ u obliku baze 32 i vremenski interval  

**P: mogu li pomoću Azure višestruke provjere autentičnosti poslužitelja sigurne Terminal Services?**

Da, no ako se pomoću Windows Server 2012 R2 ili noviju verziju, samo korištenjem udaljene radne površine (RD pristupnika).

Sigurnosne postavke u sustavu Windows Server 2012 R2 ste promijenili način na koji Azure višestruke provjere autentičnosti poslužitelja koji se povezuje s paket sigurnosnog lokalne sigurnosti za izdavanje certifikata (LSA) u sustavu Windows Server 2012 i starijim verzijama. Verzija Terminal Services u sustavu Windows Server 2012 ili neke starije verzije, možete [sigurne aplikacije pomoću provjere autentičnosti sustava Windows](multi-factor-authentication-get-started-server-windows.md#to-secure-an-application-with-windows-authentication-use-the-following-procedure). Ako koristite Windows Server 2012 R2, morat ćete RD pristupnik.

**P: zašto bi korisnik primanje poziva višestruka provjera autentičnosti anonimni pozivatelj nakon postavljanja ID pozivatelja?**

Kada su višestruka provjera autentičnosti pozivi putem javne telefonske mreže, katkad oni su usmjerena kroz prijenosni koji ne podržava ID pozivatelja. Zbog toga ID pozivatelja nije uvijek je, čak i ako se višestruka provjera autentičnosti sustava uvijek šalje ga.


## <a name="errors"></a>Pogreške

**P: što treba korisnika? Ako vidite poruku o pogrešci "zahtjev za provjeru autentičnosti nije aktivirani računa" prilikom korištenja mobilne aplikacije obavijesti**

Recite im slijediti ovaj postupak da biste uklonili računa iz mobilne aplikacije, a zatim ga ponovno dodajte:

1. Idite na [profil Azure portala](https://account.activedirectory.windowsazure.com/profile/) i prijavite se pomoću računa tvrtke ili ustanove.
2. Odaberite **dodatne sigurnosti provjere**.
4. Uklonite postojeći račun iz aplikacije za mobilne uređaje.
5. Kliknite **Konfiguriraj**, a zatim slijedite upute da ponovno konfigurira aplikacije za mobilne uređaje.


**P: što korisnicima učiniti ako vidite 0x800434D4L poruku o pogrešci prilikom prijave u aplikaciju koja nije preglednika?**

Trenutno, korisnik može koristiti dodatne sigurnosti potvrdu samo uz aplikacije i servise koje korisnik može pristupiti putem preglednika. Aplikacije koje nisu preglednika (naziva *aplikacije obogaćenog klijenta*) koji su instalirani na lokalnom računalu, kao što je Windows PowerShell, ne funkcionira s računima koji se Traži ovjeru dodatne sigurnosti. U ovom slučaju, korisnik može potražite u članku aplikacije generirati pogrešku 0x800434D4L.

Zaobilazno rješenje za to je imati zasebne korisničke račune za administratore odnose i operacije koji nisu administratori. Naknadno možete povezati poštanskih sandučića između administratorskog računa i račun koji nisu administratori tako da se možete prijaviti u Outlook pomoću računa koji nisu administratori. Dodatne informacije o tome Dodatne upute za [Omogućivanje administratoru otvaranje i pregled sadržaja poštanskog sandučića korisnika](http://help.outlook.com/141/gg709759.aspx?sl=1).

## <a name="next-steps"></a>Daljnji koraci

Ako ovdje niste pronašli odgovor na pitanje, pošaljite ga ostavite u komentarima pri dnu stranice. Ili, Evo nekih dodatne mogućnosti za pristup pomoći:


**P: kako dobiti pomoć za Azure višestruka provjera autentičnosti?**

- Pretraživanje [Microsoftove baze znanja za podršku](https://www.microsoft.com/en-us/Search/result.aspx?form=mssupport&q=phonefactor&form=mssupport) za rješenja uobičajenih tehničkih problema.

- Traženje i pregled tehnička pitanja i odgovore iz zajednice ili postavite vlastitu pitanje na [forumima Azure Active Directory](https://social.msdn.microsoft.com/Forums/azure/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).

- Ako ste korisnik sustava naslijeđene PhoneFactor, a imate pitanja ili je potrebna pomoć za ponovno postavljanje lozinke, koristiti vezu [za ponovno postavljanje lozinke](mailto:phonefactorsupport@microsoft.com) za otvaranje slučaja podrška.

- Kontaktirajte stručnjaka za podršku putem [podrška za Azure višestruke provjere autentičnosti poslužitelja (PhoneFactor)](https://support.microsoft.com/oas/default.aspx?prid=14947). Kada se obratite nam, korisno je ako može obuhvaćati suvišni informacije o problem moguće. Podatke možete navesti sadrži stranice koje se pojavi pogreška, specifična Šifra pogreške, određene sesije ID i ID korisnika koji se pojavi pogreška.
