<properties
    pageTitle="Dodatne informacije: Upravljanje Azure AD lozinku | Microsoft Azure"
    description="Dodatne teme na upravljanje Azure AD lozinku, uključujući lozinku upisima funkcioniranja, zaštita lozinkom upisima, kako ponovno postavljanje lozinke funkcioniranje portala i podataka koje koriste ponovno postavljanje lozinke."
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

# <a name="learn-more-about-password-management"></a>Dodatne informacije o upravljanju lozinke

> [AZURE.IMPORTANT] **Jeste li ovdje jer je što imate poteškoća s prijavom?** Ako je tako, [Evo kako možete promijeniti i ponovno postavili vlastitu lozinku](active-directory-passwords-update-your-own-password.md).

Ako ste već ste implementirali upravljanje lozinkama, ili samo želite dodatne informacije o Tehnički nitty gritty od načina funkcioniranja tijeka rada prije no što implementirate, u ovom se odjeljku steći ćete dobar pregled tehničke konceptima servis. Ćemo objasniti sljedeće:

* [**Pregled upisima lozinke**](#password-writeback-overview)
  - [Kako funkcionira lozinku upisima](#how-password-writeback-works)
  - [Scenariji podržana za lozinku upisima](#scenarios-supported-for-password-writeback)
  - [Model sigurnosti upisima lozinke](#password-writeback-security-model)
* [**Kako lozinku vratiti portala rada?**](#how-does-the-password-reset-portal-work)
  - [Podataka koje koriste ponovno postavljanje lozinke?](#what-data-is-used-by-password-reset)
  - [Kako pristupiti lozinku ponovno postavljanje podataka za korisnike](#how-to-access-password-reset-data-for-your-users)

## <a name="password-writeback-overview"></a>Pregled upisima lozinke
Lozinka upisima je komponenta za [Azure Active Directory povezivanje](active-directory-aadconnect.md) koji se mogu omogućiti i koristi trenutni pretplatnike Azure Active Directory Premium. Dodatne informacije potražite u članku [Azure Active Directory izdanja](active-directory-editions.md).

Upisima lozinka omogućuje konfiguriranje klijenta oblaka za pisanje lozinke koje ste lokalnog servisa Active Directory.  Obviates koje ne morate postaviti i upravljati rješenje složene lokalnog samostalno ponovno postavljanje, a ona omogućuje praktično oblaku za korisnike za ponovno postavljanje lozinke lokalnog gdje god se nalazili.  Pročitajte na za neke ključne značajke programa upisima lozinku:

- **Nula odgode povratne informacije.**  Lozinka upisima je sinkrono operacija.  Korisnici će primati obavijesti odmah svoju lozinku ne zadovoljava pravila ili nije mogao ponovno postavljanje ili promjena iz bilo kojeg razloga.
- **Ponovno postavljanje lozinke za korisnike putem servisa AD FS i druge tehnologije vanjski pristup podržava.**  S upisima lozinku, pod uvjetom da pridruženim korisničke račune sinkroniziraju u klijentu za Azure AD će moći upravljati svojim lokalnog AD lozinke iz oblaka.
- **Ponovno postavljanje lozinki za korisnika putem sinkronizaciju lozinke raspršivanje podržava.** Kada servis za ponovno postavljanje lozinke otkrije sinkronizirane korisnički račun omogućena za sinkronizaciju lozinke predmemorije, ne možemo izvorno ovaj račun lokalnog i lozinka oblaka istodobno.
- **Promjena lozinke iz teksta i Office 365 podržava.**  Kada vanjskog ili sinkronizaciju lozinke će korisnici Dođite na mijenjati istekle ili nije istekao lozinke, ne možemo ćete odgovorili te lozinke za lokalni okruženje za AD.
- **Podržava pisanje natrag lozinki kada administrator ponovno ih na** [**Portal za upravljanje azure**](https://manage.windowsazure.com).  Kada administrator vraća koji se želite korisničke lozinke na [Portal za upravljanje Azure](https://manage.windowsazure.com), ako je korisnik vanjskog ili sinkronizaciju lozinke, ne možemo ćete postaviti lozinku administrator odabire na vašem lokalnom AD, kao i.  To trenutno nije podržano u Centar za administratore sustava Office.
- **Primjenjuje lokalnih AD lozinki.**  Kada korisnik vraća svojem lozinku, ne možemo provjerite je li ispunjava li lokalnih pravila AD prije potvrđivanja taj imenik.  To obuhvaća povijest, složenosti, dob, filtri lozinku i druge lozinku ograničenja koje ste definirali u lokalnog servisa Active Directory.
- **Sva pravila vatrozida za unutarnje nisu potrebne.**  Lozinka upisima koristi za prijenos Bus servisa Azure kao u podlozi komunikacijskog kanala, što znači da ne morate otvoriti sve ulaznog priključaka u vatrozidu za ovu značajku za rad.
- **Nije podržana za korisničke račune koji postoje unutar zaštićeni grupa servisa Active Directory lokalnog.** Dodatne informacije o grupama za zaštićene potražite u članku [zaštićeni račune i grupe u servisu Active Directory](https://technet.microsoft.com/library/dn535499.aspx).

### <a name="how-password-writeback-works"></a>Kako funkcionira upisima lozinke
Lozinka upisima sastoji se od tri glavne komponente:

- Lozinku ponovno postavljanje u oblaku (to je i integriran u stranice Promjena lozinke za Azure AD)
- Prijenos Bus servisa Azure specifične za klijenta
- Krajnja točka za ponovno postavljanje lozinke lokalno

Zajedno kao što je opisano u stane na ispod dijagrama:

  ![][001]

Kada vanjsko ili lozinka raspršivanje sinkronizirati želite korisnik potječe za ponovno postavljanje ili promjena lozinke upisom u oblaku, događa se sljedeće:

1.  Ne možemo provjeriti koju vrstu lozinku korisnik ima.  Ako Vidimo lozinke upravlja lokalno, zatim ćemo provjerite je li servis upisima s radom.  Ako je, smo korisniku prije nastavka omogućiti ako nije, ne možemo reći korisnika koji svoju lozinku nije moguće vratiti ovdje.
2.  Nakon toga korisnik prosljeđuje Stevensu odgovarajuće provjeru autentičnosti i dosegne zaslona za ponovno postavljanje lozinke.
3.  Korisnik odabere novu lozinku i potvrditi ga.
4.  Nakon kliknete Pošalji, ne možemo Šifriraj lozinku običan tekst s ključem simetričnu koji je stvoren tijekom postupka postavljanja upisima.
5.  Nakon šifriranje lozinku, ne možemo uključiti u teret koji će poslati putem kanala HTTPS vaše preusmjeravanja klijentu određenog servisa bus na (koji smo također umjesto vas postaviti tijekom postupka postavljanja upisima).  U ovom preusmjeravanja zaštićen slučajno generiranim lozinku koju samo lokalnim instalacijama zna.
6.  Kada poruku dostigne bus servisa, krajnja točka za ponovno postavljanje lozinke automatski aktiviranja prema gore i vidi je zahtjev za ponovno postavljanje na čekanju.
7.  Servis zatim traži korisnika u pitanju pomoću atribut sidrenja oblaka.  Iz ovog pretraživanja da biste uspije, korisničkom objektu moraju se nalaziti u prostor poveznik AD, moraju biti povezani odgovarajući MV objekt, a moraju biti povezani odgovarajući objekt poveznik AAD. Na kraju, za sinkronizaciju da biste pronašli ovaj korisnički račun, vezu iz AD poveznik objekta MV moraju imati pravilo za sinkronizaciju `Microsoft.InfromADUserAccountEnabled.xxx` vezu.  To nije potrebno jer kada se poziv iz oblaka, modula za sinkroniziranje koristi atribut cloudAnchor da biste potražili AAD poveznik prostora objekt, a zatim prati vezu vratili na objekt MV, a zatim slijedi vezu vratili na objekt servisa Active Directory. Jer se može postojati više objekata AD (više skupa stabala) za istoga korisnika, modula za sinkroniziranje ovisi na `Microsoft.InfromADUserAccountEnabled.xxx` vezu da biste odabrali ispravnu.
8.  Nakon što se nalazi se na korisnički račun, ne možemo pokušati ponovno postavljanje lozinke izravno na odgovarajući skup stabala u AD.
9.  Ako operacija postavljanje lozinke ne uspije, ne možemo prepoznati korisnika svoju lozinku izmijenjena i izmjene na njihove Sretan način.
10. Ako lozinku postupak ne uspije, ne možemo vratiti pogrešku korisniku i obavijestite tu osobu pokušajte ponovno.  Postupak možda neće uspjeti jer servis prema dolje, budući da lozinku kako ne zadovoljava pravilnicima tvrtke ili ustanove, jer smo nije moguće pronaći korisnika u lokalnu AD ili bilo koji broj razloga.  Radimo imaju određenu poruku za mnoge od tih slučajeva i obavijest korisniku što mogu učiniti da biste riješili problem.

### <a name="scenarios-supported-for-password-writeback"></a>Scenariji podržana za lozinku upisima
U tablici u nastavku opisuju koje scenariji podržani za koje verzije naš mogućnost sinkronizacije.  Općenito govoreći, preporučujemo da instalirate najnoviju verziju programa [Azure AD Connect](active-directory-aadconnect.md#install-azure-ad-connect) ako želite koristiti lozinku upisima.

  ![][002]

### <a name="password-writeback-security-model"></a>Model sigurnosti upisima lozinke
Lozinka upisima je vrlo sigurne i robusne servis.  Da biste osigurali podatke zaštićen, ne možemo omogućiti 4 tiered sigurnost modelu koji je opisan u nastavku.

- **Klijentu određene servisa bus preusmjeravanja** – kada postavljate servis, ne možemo postaviti preusmjeravanja bus specifične za klijenta servisa koji su zaštićeni tako da slučajno generiranim jaku lozinku koju Microsoft nikad ne može pristupiti.
- **Zaključano dolje jake, ključa za šifriranje lozinke** – nakon stvaranja bus preusmjeravanja servisa, ćemo stvoriti jaku simetričnu ključ koji smo šifriranje pomoću lozinke kao dođe pokazivač na žičani.  Ovaj ključ nalazi se samo na vaše tvrtke tajnu pohrane u oblaku, koja je intenzivnog zaključan prema dolje i nadzirati, baš kao i sve lozinke u direktoriju.
- **Standardne TLS industrijskih** – kada lozinku ponovno postavljanje ili promjena postupak koji se pojavljuje u oblak, ne možemo poduzeti lozinku običan tekst i Šifriraj pomoću javni ključ.  Ne možemo zatim plop koji u HTTPS poruku koja se šalje putem kanala šifrirane pomoću certifikati o Microsoftovu SSL da biste na servis bus preusmjeravanja.  Kad stigne poruke u Bus servis, agent za lokalno aktiviranja prema gore, potvrđuje Bus servis pomoću jaku lozinku koju ste prethodno generiran, uzima šifriranu poruku, dešifrira pomoću privatni ključ smo generira, a zatim prilikom pokušaja da biste postavili lozinku putem u AD DS SetPassword API-JA.  Ovaj korak nije što omogućuje nam da biste nametnuli AD lokalno lozinku pravilnika (složenosti dob, povijest, filtri, itd) u oblaku.
- **Pravila isteka poruke** – na kraju, ako zbog nekog razloga poruka nalazi u servis Bus jer je servis za lokalno prema dolje, bit će biti isteklo i ukloniti nakon nekoliko minuta da biste povećali sigurnost još više.

## <a name="how-does-the-password-reset-portal-work"></a>Kako lozinku vratiti portala rada?
Kada korisnik vodi na portal za vraćanje izvorne lozinke, tijek rada kicked da biste odredili je li taj korisnički račun valjana, što organizacije kojoj korisnici pripada, gdje se upravlja lozinku za tog korisnika i je li korisnik licenciran za korištenje značajke.  Pročitajte upute za dodatne informacije o logike iza stranici za ponovno postavljanje lozinke.

1.  Korisnik klikne se ne mogu pristupiti računu vezu ili bude izravno [https://passwordreset.microsoftonline.com](https://passwordreset.microsoftonline.com).
2.  Korisnik unosi korisnički id i prosljeđuje na captcha.
3.  Azure AD provjerava je li korisnik moći koristiti tu značajku tako da učinite sljedeće:
    - Provjerava ima li korisnik Ova funkcija nije omogućena i dodijelili licencu za Azure AD.
        - Ako korisnik nema Ova funkcija nije omogućena ili dodijeljena licenca, od korisnika će se zatražiti da se obratite svojem administratoru za ponovno postavljanje lozinke za svoj.
    - Provjerava ima li korisnik s desne strane izazov podatke koji su definirana na svoj račun skladu pravila administratora.
        - Ako pravilo zahtijeva samo jedan test, pa ga je ensured ima li korisnik odgovarajuće podatke definirali za barem jedno od izazove omogućiti pravila administratora.
          - Ako korisnik nije konfiguriran, korisnik se preporučuje da se obratite svojem administratoru za ponovno postavljanje lozinke za svoj.
        - Ako je pravilnik potrebna dva izazove, pa ga je ensured ima li korisnik odgovarajuće podatke definirane za barem dva izazove omogućiti pravila administratora.
          - Ako korisnik nije konfiguriran, a zatim ćemo je korisnik preporučuje se da se obratite svojem administratoru za ponovno postavljanje lozinke za svoj.
    - Provjerava je li se korisnikova lozinka upravlja lokalno (vanjskog ili sinkronizaciju lozinke raspršivanje imali).
       - Ako je implementiran upisima i korisničke lozinke upravlja lokalno, korisnik je dopušteno da biste prešli na provjeru autentičnosti i ponovno postavljanje lozinke upisom.
       - Ako upisima je implementiran i korisničke lozinke upravlja lokalno, od korisnika se zatražiti da se obratite svojem administratoru za ponovno postavljanje lozinke za svoj.
4.  Ako se određuje se da je korisnik moći uspješno ponovno postavljanje lozinke za svoj, a zatim je korisnik Vođena kroz postupak Vrati izvorne postavke.

Saznajte kako implementirati upisima lozinku pri [Početak rada: Upravljanje lozinkama Azure AD](active-directory-passwords-getting-started.md).

### <a name="what-data-is-used-by-password-reset"></a>Podataka koje koriste ponovno postavljanje lozinke?
Sljedeća tablica konture gdje i kako se koristi tijekom ponovno postavljanje lozinke te podatke osmišljene su da biste lakše odlučili koji mogućnosti provjere autentičnosti su odgovarajuće za tvrtku ili ustanovu. Ova tablica prikazuje i svim oblikovanja zahtjevima za slučajeve u kojima su pružati podatke ime korisnika iz unos putove koji provjeriti valjanost podataka na ovom.

> [AZURE.NOTE] Telefon Office ne pojavi na portalu za registraciju jer se korisnici trenutno ne možete uređivati ovo svojstvo u direktoriju.

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Naziv vrstu kontakta</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Element podataka Azure Active Directory</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Korišteni / podesiti gdje?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Preduvjeti za oblikovanje</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Telefon za Office</p>
            </td>
            <td>
              <p>PhoneNumber</p>
              <p>primjer skup-MsolUser - UserPrincipalName JWarner@contoso.com - PhoneNumber "+ 1 1234567890 x 1234"</p>
            </td>
            <td>
              <p>Koristi se u:</p>
              <p>Portal za ponovno postavljanje lozinke</p>
              <p>Moguće postaviti na:</p>
              <p>PhoneNumber nije moguće postaviti PowerShell, DirSync, Portal za upravljanje Azure i portala za administratore sustava Office</p>
            </td>
            <td>
              <p>+ ccc xxxyyyzzzz (npr. 1234567890 + 1)</p>
              <ul>
                <li class="unordered">
Morate unijeti pozivni broj<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Morate unijeti pozivni broj (gdje je to primjenjivo)<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Morate navesti na + ispred države kod<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Morate imati razmak između pozivni broj države i ostale broja<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Nastavci nisu podržani, ako imate proširenja naveden, ne možemo će uklanjanje je od broja prije i isporuka telefonski poziv.<br><br></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>Mobilni telefon</p>
            </td>
            <td>
              <p>AuthenticationPhone</p>
              <p>OR</p>
              <p>MobilePhone</p>
              <p>(Provjeru autentičnosti telefona koristi li previše podataka, u suprotnom to pada natrag u polju mobilni telefon).</p>
              <p>primjer skup-MsolUser - UserPrincipalName JWarner@contoso.com - MobilePhone "+ 1 1234567890 x 1234"</p>
            </td>
            <td>
              <p>Koristi se u:</p>
              <p>Portal za ponovno postavljanje lozinke</p>
              <p>Portal za registraciju</p>
              <p>Moguće postaviti na: </p>
              <p>AuthenticationPhone nije moguće postaviti na portal za registraciju ponovno postavljanje lozinke ili MFA Registracija portal.</p>
              <p>MobilePhone nije moguće postaviti PowerShell, DirSync, Portal za upravljanje Azure i portala za administratore sustava Office</p>
            </td>
            <td>
              <p>+ ccc xxxyyyzzzz (npr. 1234567890 + 1)</p>
              <ul>
                <li class="unordered">
Morate unijeti pozivni broj.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Morate unijeti pozivni broj (gdje je to primjenjivo).<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Morate navesti ste na + ispred države kod.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Morate imati razmak između pozivni broj države i ostale broja.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Nastavci nisu podržani, ako imate proširenja naveden, ne možemo zanemarite je kada i isporuka telefonski poziv.<br><br></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>Zamjenska e-pošte</p>
            </td>
            <td>
              <p>AuthenticationEmail</p>
              <p>OR</p>
              <p>AlternateEmailAddresses [0] </p>
              <p>(Provjera autentičnosti e-pošte koristi li previše podataka, u suprotnom to pada natrag u polje zamjenska e-pošte).</p>
              <p>Napomena: polje zamjenska e-pošte naveden je u obliku matrice nizova u direktoriju.  Koristimo prvu stavku u ovom polju.</p>
              <p>primjer skup-MsolUser - UserPrincipalName JWarner@contoso.com - AlternateEmailAddresses"email@live.com"</p>
            </td>
            <td>
              <p>Koristi se u:</p>
              <p>Portal za ponovno postavljanje lozinke</p>
              <p>Portal za registraciju</p>
              <p>Moguće postaviti na: </p>
              <p>AuthenticationEmail nije moguće postaviti na portal za registraciju ponovno postavljanje lozinke ili MFA Registracija portal.</p>
              <p>AlternateEmail nije moguće postaviti PowerShell, Portal za upravljanje Azure i portala za administratore sustava Office</p>
            </td>
            <td>
              <p>
                <a href="mailto:user@domain.com">user@domain.com</a>ili甲斐@黒川.日本</p>
              <ul>
                <li class="unordered">
Poruke e-pošte morate slijediti standardno oblikovanje kao po.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Podržane su Unicode poruke e-pošte.<br><br></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>Sigurnosna pitanja i odgovore</p>
            </td>
            <td>
              <p>Nije dostupno za izmjenu izravno u direktoriju.</p>
            </td>
            <td>
              <p>Koristi se u:</p>
              <p>Portal za ponovno postavljanje lozinke</p>
              <p>Portal za registraciju </p>
              <p>Moguće postaviti na: </p>
              <p>Jedini način da biste postavili sigurnosna pitanja se putem portala za upravljanje Azure.</p>
              <p>Jedini način da biste postavili odgovore na pitanja sigurnosti za dani korisnika se putem portala za registraciju.</p>
            </td>
            <td>
              <p>Sigurnosna pitanja imaju maksimum od 200 znakova i min od 3 znaka</p>
              <p>Odgovori imaju max od 40 znakova i min od 3 znaka</p>
            </td>
          </tr>
        </tbody></table>

###<a name="how-to-access-password-reset-data-for-your-users"></a>Kako pristupiti lozinku ponovno postavljanje podataka za korisnike
####<a name="data-settable-via-synchronization"></a>Moguće je postaviti putem sinkronizacije podataka
Sljedeća polja mogu se sinkronizirati s lokalnim:

* Mobilni telefon
* Telefon za Office

####<a name="data-settable-with-azure-ad-powershell"></a>Moguće je postaviti s Azure AD PowerShell podataka
Pristupiti Azure AD PowerShell & API grafikonu su sljedeća polja:

* Zamjenska e-pošte
* Mobilni telefon
* Telefon za Office
* Provjera autentičnosti telefona
* Provjera autentičnosti e-pošte

####<a name="data-settable-with-registration-ui-only"></a>Moguće je postaviti s Registracija korisničkog Sučelja samo podatke
Sljedeća polja dostupni samo putem SSPR Registracija korisničkog Sučelja (https://aka.ms/ssprsetup):

* Sigurnosna pitanja i odgovore

####<a name="what-happens-when-a-user-registers"></a>Što se događa kada korisnik registrira?
Kada korisnik registrira, stranicu za registraciju **uvijek** skup će sljedeća polja:

* Provjera autentičnosti telefona
* Provjera autentičnosti e-pošte
* Sigurnosna pitanja i odgovore

Ako ste omogućili vrijednost za **mobilni telefon** ili **Zamjenska e-pošte**, korisnici mogu odmah koristiti one za ponovno postavljanje lozinke, čak i ako ih niste registrirao za servis.  Uz to, korisnici će vidjeti te se vrijednosti prilikom registracije za prvo te ih mijenjati ako ih želite.  Međutim, kad su uspješno registrirati ove vrijednosti će se ista i u poljima **Telefona za provjeru autentičnosti** i **Provjeru autentičnosti e-pošte** odnosno.

To može biti koristan način za deblokiranje velikog broja korisnika da biste koristili SSPR pritom ipak omogućuje korisnicima da biste provjerili valjanost ti podaci kroz postupak registracije.

####<a name="setting-password-reset-data-with-powershell"></a>Podataka pomoću ljuske PowerShell za ponovno postavljanje lozinke postavka
Možete postaviti vrijednosti za sljedeća polja s Azure AD PowerShell.

* Zamjenska e-pošte
* Mobilni telefon
* Telefon za Office

Da bismo započeli, najprije morate [preuzeti](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule)i instalirati modul Azure AD PowerShell.  Kada imate instaliran, možete slijedite korake u nastavku da biste konfigurirali svako polje.

#####<a name="alternate-email"></a>Zamjenska e-pošte
```
Connect-MsolService
Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com")
```

#####<a name="mobile-phone"></a>Mobilni telefon
```
Connect-MsolService
Set-MsolUser -UserPrincipalName user@domain.com -MobilePhone "+1 1234567890"
```

#####<a name="office-phone"></a>Telefon za Office
```
Connect-MsolService
Set-MsolUser -UserPrincipalName user@domain.com -PhoneNumber "+1 1234567890"
```

####<a name="reading-password-reset-data-with-powershell"></a>Podataka pomoću ljuske PowerShell za ponovno postavljanje lozinke za čitanje
Možete pročitati vrijednosti za sljedeća polja s Azure AD PowerShell.

* Zamjenska e-pošte
* Mobilni telefon
* Telefon za Office
* Provjera autentičnosti telefona
* Provjera autentičnosti e-pošte

Da bismo započeli, najprije morate [preuzeti](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule)i instalirati modul Azure AD PowerShell.  Kada imate instaliran, možete slijedite korake u nastavku da biste konfigurirali svako polje.

#####<a name="alternate-email"></a>Zamjenska e-pošte
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select AlternateEmailAddresses
```

#####<a name="mobile-phone"></a>Mobilni telefon
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select MobilePhone
```

#####<a name="office-phone"></a>Telefon za Office
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select PhoneNumber
```

#####<a name="authentication-phone"></a>Provjera autentičnosti telefona
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select PhoneNumber
```

#####<a name="authentication-email"></a>Provjera autentičnosti e-pošte
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select Email
```

## <a name="links-to-password-reset-documentation"></a>Dokumentacija za ponovno postavljanje veze na lozinke
U nastavku su veze na sve stranice dokumentaciju za ponovno postavljanje lozinke za Azure AD:

* **Jeste li ovdje jer je što imate poteškoća s prijavom?** Ako je tako, [Evo kako možete promijeniti i ponovno postavili vlastitu lozinku](active-directory-passwords-update-your-own-password.md).
* [**Kako to funkcionira**](active-directory-passwords-how-it-works.md) – Saznajte više o šest različitih komponenata servisa i što svaki ne
* [**Početak rada**](active-directory-passwords-getting-started.md) – upute kako bi korisnici mogli vratiti i mijenjati oblaka ili na lokalnim poslužiteljima lozinke
* [**Prilagodba**](active-directory-passwords-customize.md) – Saznajte kako prilagoditi izgled i dojam i pružanja usluge potrebama vaše tvrtke ili ustanove
* [**Najbolje prakse**](active-directory-passwords-best-practices.md) – Saznajte kako brzo i učinkovito upravljanje lozinkama u tvrtki ili ustanovi
* [**Početak uvida**](active-directory-passwords-get-insights.md) – dodatne informacije o oglednim integrirane mogućnosti izvješćivanja
* [**Najčešća pitanja vezana uz**](active-directory-passwords-faq.md) – Pronađite odgovore na najčešća pitanja
* [**Otklanjanje poteškoća**](active-directory-passwords-troubleshoot.md) – Saznajte kako brzo otklanjanje poteškoća sa servisom



[001]: ./media/active-directory-passwords-learn-more/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-learn-more/002.jpg "Image_002.jpg"
