<properties
    pageTitle="Najčešća pitanja vezana uz: Azure AD lozinku upravljanje | Microsoft Azure"
    description="Najčešća pitanja o upravljanju lozinku u Azure AD, uključujući vraćanje izvorne lozinke, Registracija, izvješća i upisima lokalni Active Directory."
    services="active-directory"
    documentationCenter=""
    authors="asteen"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="asteen"/>

# <a name="password-management-frequently-asked-questions"></a>Upravljanje lozinkama najčešća pitanja

> [AZURE.IMPORTANT] **Jeste li ovdje jer je što imate poteškoća s prijavom?** Ako je tako, [Evo kako možete promijeniti i ponovno postavili vlastitu lozinku](active-directory-passwords-update-your-own-password.md).

U nastavku su najčešća pitanja za sve vezane uz upravljanje lozinkom.

Ako pronađete sami Nadogradnja koje ne znate odgovor na ili tražite pomoć s problemom u određeni su nasuprotne, možete čitati na ispod da biste vidjeli ako smo ste prekriveno ga već.  Ako ne možemo još niste učinili, ne brinite! Slobodno pitajte sve pitanje imate koje se ovdje primjenjuju na [forume za Azure AD](https://social.msdn.microsoft.com/Forums/home?forum=WindowsAzureAD) i smo će se vratiti na čim možemo.

U ovim najčešćim Pitanjima dijeli na sljedeće odjeljke:

- [**Pitanja o registraciji ponovno postavljanje lozinke**](#password-reset-registration)
- [**Pitanja o ponovno postavljanje lozinke**](#password-reset)
- [**Pitanja o izvješćima upravljanja lozinkom**](#password-management-reports)
- [**Pitanja o Upisima lozinke**](#password-writeback)

## <a name="password-reset-registration"></a>Registracija za ponovno postavljanje lozinke
 - **P: mogu li korisnicima registrirati vlastite podataka za ponovno postavljanje lozinke?**

 > **A:** Da, kao omogućen ponovno postavljanje lozinke, a oni licencu, možete se na portal za registraciju za ponovno postavljanje lozinke na http://aka.ms/ssprsetup registrirati svoje podatke za provjeru autentičnosti koja će se koristiti s ponovno postavljanje lozinke. Korisnicima možete i registrirati tako ploču pristup pri http://myapps.microsoft.com, klikom na karticu profila, kliknete Registrirajte se za mogućnost za ponovno postavljanje lozinke. Saznajte više o tome kako se vaši korisnici konfigurirana za za vraćanje izvorne lozinke tako da pročitate kako korisnici konfigurirana za ponovno postavljanje lozinke.

 - **P: mogu li se podaci za ponovno postavljanje lozinke definirati i ime korisnike?**

 > **A:** Da, to možete učiniti s DirSync ili PowerShell ili putem portala za [Portal za upravljanje Azure](https://manage.windowsazure.com) ili administrator sustava Office. Saznajte više o toj značajci na blogu poboljšani izjave o zaštiti privatnosti za Azure AD MFA i brojeve telefona ponovno postavljanje lozinke i čitanjem Saznajte kako podataka koristi lozinku ponovno postaviti.

 - **P: mogu li sinkronizirati podatke za sigurnosna pitanja iz lokalnog?**

 > **A:** Ne, to nije moguće danas, ali ne možemo ga razmišljate.

 - **P: mogu korisnicima registrirati podataka na način da drugi korisnici ne vide ove podatke?**

 > **A:** Da, kada korisnici registrirate podataka pomoću portala za registraciju lozinku ponovno postaviti ga se sprema u polja privatne provjere autentičnosti koje su vidljive globalni administratori i korisnik sebi. Saznajte više o toj značajci na blogu poboljšani izjave o zaštiti privatnosti za Azure AD MFA i brojeve telefona ponovno postavljanje lozinke i čitanjem Saznajte kako podataka koristi lozinke ponovno postaviti.

 - **P: Moje imaju li korisnici Registriranje prije nego što se mogu koristiti i ponovno postavljanje lozinke?**

 > **A:** Ne, ako definirate dovoljno podatke za provjeru autentičnosti u njihovo ime, korisnici neće imati da biste registrirali. Ponovno postavljanje lozinke funkcionirat će samo precizno pod uvjetom da pravilno oblikovani podaci spremljeni u odgovarajućim poljima u direktoriju. Dodatne informacije o čitanjem Saznajte kako podataka koristi ponovno postavljanje lozinke.

 - **P: mogu li se sinkronizirati ili postaviti provjere autentičnosti telefona, e-pošte za provjeru autentičnosti ili zamjenske provjere autentičnosti polja Ime korisnike?**

 > **A:** Trenutno ne, no ne možemo razmišljate Omogućivanje tu mogućnost.

 - **P: kako portal za registraciju znati koje mogućnosti da biste prikazali korisnike?**

 > **A:** Registracija portal za ponovno postavljanje lozinke samo prikazuju se mogućnosti koje ste omogućili za korisnike u odjeljku pravila za ponovno postavljanje lozinke korisničkog vaš imenik Konfiguriraj kartici. To znači da ako ne omogućite, recimo, sigurnosna pitanja, zatim korisnici nećete moći da biste registrirali za tu mogućnost.

 - **P: kada korisnik smatra se registrirati?**

 > **A:** Korisnik se smatra registrirana kada korisnik ima najmanje definirali N dijelova informacije za provjeru autentičnosti, pri čemu je N broj od provjere autentičnosti metode obavezno koju ste postavili za [Portal za upravljanje Azure](https://manage.windowsazure.com). Dodatne informacije potražite u članku Prilagodba korisnik Vrati Pravilnik za lozinke.


## <a name="password-reset"></a>Ponovno postavljanje lozinke

 - **P: koliko treba čekati programa e-pošte, SMS ili telefonskom pozivu primanje ponovno postavljanje lozinke?**

 > **A:** E-pošte, SMS poruke i telefonske pozive u odjeljku 1 minute, trebali biste stižu s slova normalno se 5-20 sekundi. Ako ne primaju obavijesti u ovog vremenskog razdoblja, provjerite mapu neželjene koji broj / kontakt s poslužiteljem e-pošte je onaj koji ste očekivali, a ne i podataka za provjeru autentičnosti u direktoriju u ispravno oblikovane. Dodatne informacije o oblikovanju brojeva telefona i e-pošte adrese za ponovno postavljanje lozinke potražite u članku kako podataka koristi ponovno postavljanje lozinke.

 - **P: koje jezike podržava ponovno postavljanje lozinke?**

 > **A:** Korisničko Sučelje za vraćanje izvorne lozinke SMS poruke i govornih poziva lokalizirani u istom 40 jezike koje su podržane u sustavu Office 365. To su: arapski, Bugarske abecede, pojednostavljeni, kineski, tradicionalni kineski, Hrvatski, Češka, danski, nizozemski, engleski, estonski, finski, francuski, njemački, grčki, hebrejski, hindskom, mađarskom, Indonezijski, talijanski, japanskom, kazaškom, korejski, latvijski, Litvanski, Malajski (Malezija), Norveški (bokmal), poljski, portugalski (Brazil), portugalski (Portugal), rumunjski, ruski, Srpski (latinica), slovačkom, slovenskog, španjolski, švedski, tajlandski, turski, ukrajinskom, i vijetnamski.

 - **P: dijelovi sučelje za ponovno postavljanje lozinke dobiti brandiranog Kada postavim tvrtke ili ustanove brendiranje u moj direktoriju je konfigurirati kartica?**

 > **A:** Portal za ponovno postavljanje lozinke prikazat će se logotipa tvrtke ili ustanove te će vam omogućiti i konfigurirati kontakt administratora veza na prilagođenu e-pošte ili URL. Sve e-pošte koja će se poslati tako da ponovno postavljanje lozinke neće sadržavati stavku vaše tvrtke ili ustanove logotipom, boje (u tom slučaju crveni), ime i prezime u tijelu e-pošte, i prilagoditi iz naziv. Pogledajte primjer s sve brandiranog elemente ispod. Da biste saznali više, pročitajte ponovno postavljanje lozinke Prilagodba izgleda i dojma.

  ![][001]

 - **P: kako educirajte korisnike o gdje za ponovno postavljanje lozinke?**

 > **A:** Možete poslati korisnicima https://passwordreset.microsoftonline.com izravno ili ih kliknite možete uputiti na nije moguće pristupiti računu veza na bilo kojem ID tvrtke ili obrazovne ustanove zaslon za prijavu u. Koje možete slobodno objavljivanje ove veze (ili stvaranje preusmjeravanja URL-a da biste ih) u bilo koje mjesto na kojem se jednostavno pristupiti svojim korisnicima.

 - **P: mogu li pomoću ove stranice s mobilnog uređaja?**

 > **A:** Da, ova stranica funkcionira na mobilnim uređajima.

 - **P: koje podržavaju otključavanje lokalnim servisom active directory računi kada korisnici ponovno postavljanje lozinke?**

 > **A:** Da, kada korisnik vraća upisom lozinke i Upisima lozinka je distribuiranih sa svim verzijama Azure AD Connect ili verzije Azure AD sinkronizacije 1.0.0485.0222 ili noviji, a zatim taj korisnički račun bit će automatski otključane kada korisnik vraća upisom lozinke.

 - **P: kako integrirati za vraćanje izvorne lozinke izravno u moj korisnički doživljaj radne površine prijave?**

 > **A:** To nije moguće danas. Međutim, ako uistinu potrebne tu mogućnost, a kupac Azure AD Premium, možete instalirati Microsoftov Upravitelj identiteta pri bez dodatnih troškova i implementacija rješenja ponovno postavljanje lozinke lokalnog pronaći therein da biste riješili taj zahtjev.

 - **P: mogu postaviti različite sigurnosna pitanja za različite regionalne sheme?**

 > **A:** Ne, to nije moguće danas, ali ne možemo ga razmišljate.

 - **P: koliko pitanja smo konfigurirati za mogućnost obavezne provjere autentičnosti sigurnosna pitanja?**

 > **A:** [Portal za upravljanje Azure](https://manage.windowsazure.com)možete konfigurirati do 20 prilagođene sigurnosna pitanja.

 - **P: koliko sigurnosna pitanja možda?**

 > **A:** Sigurnosna pitanja možda između 3 i 200 znakova.

 - **P: koliko odgovore na pitanja sigurnosti možda?**

 > **A:** Odgovori na pitanja možda 3 i 40 znakova.

 - **P: su duplicirane odgovore na pitanja sigurnosti odbijena?**

 > **A:** Da, ne možemo odbacivanje duplicirane odgovore na pitanja sigurnosti.

 - **P: može korisnik registrirati više isti sigurnosno pitanje?**

 > **A:** Ne, kada korisnik registrira određeni pitanje, on možda ne Registrirajte se za to pitanje drugi put.

 - **P: je moguće da biste postavili minimalni ograničenje od sigurnosna pitanja za registraciju i ponovno postaviti?**

 > **A:** Da, jedan ograničenje moguće je postaviti za registraciju, a drugi za ponovno postavljanje. Sigurnosna pitanja 3 do 5 možda je potreban za registraciju i 3 do 5 možda je potreban za ponovno postavljanje.

 - **P: Ako korisnik ima više od maksimalni broj pitanja morati vratiti izvorne postavke registriran, kako su sigurnosna pitanja odabrane tijekom ponovnog postavljanja?**

 > **A:** N sigurnosna pitanja su nasumično odabran iz ukupan broj pitanja korisnika je registriran za, pri čemu je N najmanji broj pitanja potreban za ponovno postavljanje lozinke. Ako, na primjer, ako korisnik ima registrirana 5 sigurnosna pitanja, ali samo 3 su potrebne za ponovno postavljanje, 3 od tih 5 će biti nasumično odabran i prikazuje korisniku u trenutku ponovno postavljanje. Ako korisnik dobiti odgovore na pitanja pogrešan, postupak odabira pojavljuje se ponovno da biste spriječili pitanje napade putem zagušenja poslužitelja.

 - **P: učinite korisnici neće moći pokušaja za vraćanje izvorne lozinke više puta u razdoblju kratko vrijeme?**

 > **A:** Postoji nekoliko sigurnosnim značajkama ugrađenu ponovno postavljanje lozinke. Korisnici mogu samo pokušajte 5 pokušaji ponovno postavljanje lozinke u jedan sat prije nego što se zaključan 24 sata. Korisnici pokušati samo za provjeru valjanosti telefonskog broja 5 puta unutar jedan sat prije nego što se zaključan 24 sata. Korisnici samo pokušati s jednog načina provjere autentičnosti 5 puta unutar jedan sat prije nego što se zaključan 24 sata.

 - **P: za koliko su e-pošte i SMS mikrofona valjani?**

 > **A:** Vijek trajanja sesije za ponovno postavljanje lozinke je 105 minuta. To znači da se od početka lozinku ponovno postavljanje operacija korisnik ima 105 minuta da biste ponovno upisom lozinku. E-pošte i SMS mikrofona nisu valjani nakon isteka ovog vremenskog razdoblja.


## <a name="password-management-reports"></a>Upravljanje lozinkama izvješća

 - **P: koliko dugo traje podataka koja će se prikazivati na upravljanje izvješćima lozinku?**

 > **A:** Podataka prikazivati na upravljanje izvješćima lozinku za 5 10 minuta. Ga nekim slučajevima možda će trebati prema gore u sat da se prikazuju.

 - **P: kako filtriranje izvješća o za upravljanje lozinku?**

 > **A:** Upravljanje izvješćima lozinku možete filtrirati tako da kliknete small Povećalo ekstremne desno od natpise stupaca, pri vrhu izvješća (pogledajte snimka). Ako želite učiniti bogatiji filtriranja, možete preuzeti da izvješće programa excel, a zatim Stvori zaokretnu tablicu.

  ![][002]

 - **P: što je maksimalni broj događaja se pohranjuju u izvješća o za upravljanje lozinku?**

 > **A:** Do 1000 lozinku ponovno postaviti ili lozinku događaje Registracija Vrati izvorne postavke spremaju se u izvješćima upravljanja lozinkom.  Radimo da biste proširili taj broj da biste dodali više događaja.

 - **P: koliko natrag upravljanje izvješćima lozinke otvorite?**

 > **A:** Upravljanje izvješćima lozinku prikaz postupaka koji se pojavljuje u zadnjih 30 dana. Ne možemo trenutno su istražuje kako to učiniti dulje vremensko razdoblje. Zasad, ako vam je potrebna za arhiviranje podataka, možete preuzeti izvješća povremeno i spremite ih u nekom drugom mjestu.

 - **P: postoji maksimalan broj redaka koji se može pojaviti na upravljanje izvješćima lozinku?**

 > **A:** Da, najviše 1000 redaka pojaviti na bilo koju od izvješća upravljanje lozinkama hoće li se prikazuju u korisničkom Sučelju ili preuzimanja. Ne možemo trenutno su istražuje kako povećati to ograničenje.

 - **P: postoji API-JA za pristup ponovno postavljanje lozinke ili Registracija izvješćivanja podataka?**

 > **A:** Da, pogledajte sljedeće dokumentaciju da biste saznali kako pristupiti ponovno postavljanje lozinke u tijeku podataka za izvješćivanje o pogreškama.  [Saznajte kako pristupiti lozinku ponovno postavljanje izvješćivanja događaje programski](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent).

## <a name="password-writeback"></a>Lozinka Upisima
 - **P: kako Upisima lozinku radi u pozadini?**

 > **A:** Pogledajte [kako Upisima lozinku radi](active-directory-passwords-learn-more.md#how-password-writeback-works) za detaljnije objašnjenje što se događa kada omogućite Upisima lozinke, kao i kako podataka teče kroz sustav zaključali lokalnog okruženja. Potražite u odjeljku [model sigurnosti Upisima lozinku](active-directory-passwords-learn-more.md#password-writeback-security-model) u kako Upisima lozinku da biste saznali kako ćemo provjerite je li lozinka Upisima iznimno sigurne servisa.

 - **P: koliko ne Upisima lozinku potrebno za rad?  Je li sinkronizacije odgode kao što su za sinkronizaciju raspršivanje lozinku?**

 > **A:** Trenutno je Upisima lozinku. Nije sinkronizirano kanal koji funkcionira bitno razlikuju od sinkronizacije raspršivanje lozinku. Lozinka Upisima korisnicima omogućuje prikupljanje povratnih informacija Realno vrijeme o uspjehu njihove ponovno postavljanje lozinke ili promjena operacija. Prosječno vrijeme za uspješan upisima lozinke je u odjeljku 500 ms.

 - **P: koje vrste računa Upisima lozinke funkcionira za?**

 > **A:** Upisima lozinku radi vanjskim i sinkronizaciju lozinke raspršivanje poslati korisnicima.

 - **P: ne Upisima lozinku Nametni lozinki za svoje domene?**

 > **A:** Da, lozinku Upisima nameće starost lozinke, povijest, složenosti, filtri i druge ograničenje se može smjestiti na mjestu na lozinke u vaše lokalne domene.

 - **P: sigurna Upisima lozinku?  Kako mogu li se neće dobiti hacked?**

 > **A:** Da, lozinka Upisima sigurna iznimno. Dodatne informacije o 4 slojeva zaštite implementirana servis Upisima lozinku, pogledajte [model sigurnosti Upisima lozinku](active-directory-passwords-learn-more.md#password-writeback-security-model) kako Upisima lozinku radi.




## <a name="links-to-password-reset-documentation"></a>Dokumentacija za ponovno postavljanje veze na lozinke
U nastavku su veze na sve stranice dokumentaciju za ponovno postavljanje lozinke za Azure AD:

* **Jeste li ovdje jer je što imate poteškoća s prijavom?** Ako je tako, [Evo kako možete promijeniti i ponovno postavili vlastitu lozinku](active-directory-passwords-update-your-own-password.md).
* [**Kako to funkcionira**](active-directory-passwords-how-it-works.md) – Saznajte više o šest različitih komponenata servisa i što svaki ne
* [**Početak rada**](active-directory-passwords-getting-started.md) – upute kako bi korisnici mogli vratiti i mijenjati oblaka ili na lokalnim poslužiteljima lozinke
* [**Prilagodba**](active-directory-passwords-customize.md) – Saznajte kako prilagoditi izgled i dojam i ponašanje servisa potrebama vaše tvrtke ili ustanove
* [**Najbolje prakse**](active-directory-passwords-best-practices.md) – Saznajte kako brzo i učinkovito upravljanje lozinkama u tvrtki ili ustanovi
* [**Početak uvida**](active-directory-passwords-get-insights.md) – dodatne informacije o oglednim integrirane mogućnosti izvješćivanja
* [**Otklanjanje poteškoća**](active-directory-passwords-troubleshoot.md) – Saznajte kako brzo otklanjanje poteškoća sa servisom
* [**Saznajte više**](active-directory-passwords-learn-more.md) – otvorite obuhvaća više razina u tehničke pojedinosti funkcioniranje servisa


[001]: ./media/active-directory-passwords-faq/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-faq/002.jpg "Image_002.jpg"
