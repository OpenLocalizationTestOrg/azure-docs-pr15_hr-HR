<properties
    pageTitle="Kako omogućiti SSO unakrsno aplikacije na iOS pomoću ADAL | Microsoft Azure"
    description="Kako koristiti značajke ADAL SDK da biste omogućili jedinstvenu prijavu preko aplikacija. "
    services="active-directory"
    documentationCenter=""
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>


# <a name="how-to-enable-cross-app-sso-on-ios-using-adal"></a>Kako omogućiti SSO unakrsno aplikacija na iOS pomoću ADAL


Pružanje jedan prijavu (SSO) tako da korisnici morati samo jednom unositi vjerodajnice i automatski imati vjerodajnice radi preko aplikacije sada se očekuje prema klijentima. Poteškoće prilikom u unosu svoje korisničko ime i lozinku na malom zaslonu, često vremena u kombinaciji s programa dodatne varijance (2FA) kao što je telefonski poziv ili texted kod rezultira brzi nezadovoljstva ako korisnik ima da biste to učinili više puta proizvoda.

Osim toga, ako pod utjecajem platforme identitet koji druge aplikacije mogu koristiti kao što je Microsoft Accounts ili poslovni račun u sustavu Office 365, klijentima očekivati one vjerodajnice da bi bio dostupan za korištenje na svim svojim aplikacijama bez obzira na to dobavljača.

Platforme Microsoft Identity, zajedno s oglednim Microsoft identiteta SDK-ovi, sve ovaj unos uložili mnogo truda ne umjesto vas te vam nudi mogućnost delight klijentima uz SSO ili unutar vlastite paketa aplikacije ili, kao i naše broker mogućnost i autentikatora aplikacije, preko cijele uređaja.

Ovaj vodič se kako konfigurirati naš SDK aplikacije radi olakšavanja ova pogodnost klijentima.

Ovaj vodič odnosi se na:

* Azure Active Directory
* B2C Azure Active Directory
* B2B Azure Active Directory
* Uvjetno pristup Azure Active Directory


Imajte na umu dokument ispod pretpostavlja koju ste znanje o tome kako [dodjele aplikacije na portalu naslijeđene za Azure Active Directory](./develop/active-directory-how-to-integrate.md) kao i aplikacije su integrirani [identiteta Microsoft iOS SDK](https://github.com/AzureAD/azure-activedirectory-library-for-objc).

## <a name="sso-concepts-in-the-microsoft-identity-platform"></a>SSO koncepte platformu Microsoft identiteta

### <a name="microsoft-identity-brokers"></a>Brokerske djelatnosti Microsoft identiteta

Microsoft pruža aplikacije za svaku mobilne platformu koje omogućuju premošćivanja vjerodajnice svim aplikacijama iz različitih dobavljača kao i omogućuje posebno poboljšane značajke koje je potrebno na jednom mjestu sigurne iz koje treba provjeriti vjerodajnice. Ne možemo poziva te **Brokerske djelatnosti**. Na iOS i Android te se daju kroz koje je moguće preuzeti aplikacije da kupci instalirati neovisno ili možete pritisak na uređaju tvrtki koja upravlja neke ili sve uređaja za svoje zaposlenika. Ove Brokerske djelatnosti podržavaju upravljanje sigurnost samo za neke aplikacije ili cijele uređaja koji se temelji na što žele IT administratorima. U sustavu Windows ta je funkcija navedeni su tako da je račun birač ugrađen u operacijskom sustavu, poznati Tehnički kao Broker provjere autentičnosti Web.

Da biste razumjeli kako koristimo te Brokerske djelatnosti i kako korisnici možda ih vidite u svoje tijek prijava za platformu Microsoft Identity čitati na dodatne informacije.

### <a name="patterns-for-logging-in-on-mobile-devices"></a>Uzorci za zapisivanje u na mobilnim uređajima

Pristup vjerodajnice na uređajima slijedite dvije osnovne uzoraka za platformu Microsoft Identity:

* Osobe koje nisu broker Vođena prijave
* Vi posrednik Vođena prijave

#### <a name="non-broker-assisted-logins"></a>Osobe koje nisu broker Vođena prijave

Osobe koje nisu broker Vođena prijave su sučelja za prijavu koje pokreću u ravnini s aplikacijom i koristiti lokalno spremište na uređaju za tu aplikaciju. Ovaj prostor za pohranu možda zajedničke aplikacije, ali su vjerodajnice čvrsto aplikacije ili paketa aplikacije pomoću tog vjerodajnica. Ovo je sučelje vjerojatno ste naišli na mnogo mobilne aplikacije koje unosite korisničko ime i lozinku same aplikacije.

Ove prijave imati sljedeće prednosti:

-  Korisnički doživljaj postoji, u aplikaciji.
-  Vjerodajnice je moguće zajednički koristiti putem aplikacije koje su potpisane istog certifikata pruža na jednom prijave u vašem paketu aplikacije.
-  Kontrola oko zapisivanje u pisanju dostupni su aplikacije prije i nakon prijave.

Ove prijave imati sljedeće Nedostaci:

- Ne može se dogoditi korisniku jedinstvene prijave na svim aplikacijama za sve koji koriste Identity programa Microsoft samo na one Microsoft Identities koje su aplikacije vlasništvu i koje ste konfigurirali.
- Aplikacija ne može se koristiti s naprednije značajke tvrtke kao što su uvjetno Access ili koristite paket InTune proizvoda.
- Aplikacija ne podržava provjeru autentičnosti certifikat prema za poslovne korisnike.

Ovdje je prikaz funkcioniranje Microsoft identiteta SDK-ovi s zajedničkog prostora za pohranu vaše aplikacije da biste omogućili SSO:

```
+------------+ +------------+  +-------------+
|            | |            |  |             |
|   App 1    | |   App 2    |  |   App 3     |
|            | |            |  |             |
|            | |            |  |             |
+------------+ +------------+  +-------------+
| Azure SDK  | | Azure SDK  |  | Azure SDK   |
+------------+-+------------+--+-------------+
|                                            |
|            App Shared Storage              |
+--------------------------------------------+
```

#### <a name="broker-assisted-logins"></a>Vi posrednik Vođena prijave

Broker Potpomognuta prijave su prijava doživljaja pojaviti u aplikaciji broker i koristite za pohranu i sigurnost brokera da biste zajednički koristili vjerodajnice preko svih aplikacija na uređaju pod utjecajem platformu Microsoft Identity. To znači da će aplikacija ovise o brokera da biste se prijavili korisnika. Na iOS i Android te se daju kroz koje je moguće preuzeti aplikacije da kupci instalirati neovisno ili možete pritisak na uređaju tvrtki koja upravlja uređaj za svoje korisnika. Primjer ove vrste aplikacija je aplikacije za Azure autentikatora na iOS. U sustavu Windows ta je funkcija navedeni su tako da je račun birač ugrađen u operacijskom sustavu, poznati Tehnički kao Broker provjere autentičnosti Web.
Rad s razlikuje platforme i katkad može biti disruptive korisnicima ako ne upravlja pravilno. Koje poznajete vjerojatno najčešće ovaj uzorak ako imate instaliranu aplikaciju servisa Facebook i koristiti funkcije Facebook prijava u nekoj drugoj aplikaciji. Platforme Microsoft Identity upravlja isti uzorak.

Za iOS to potencijalnih klijenata "prijelaza" animacije koju će se aplikacija poslali u pozadinu tijekom aplikacija Azure autentikatora dolazi u prednji plan za korisnika da biste odabrali koji račun ih želite prijavljujete.  

Za Android i Windows birač računa se prikazuje pri vrhu aplikacije koja je manja disruptive korisniku.

#### <a name="how-the-broker-gets-invoked"></a>Kako dobiti brokera poziva

Kompatibilni broker je instalirana na uređaj, kao što je aplikacija Azure autentikatora Microsoft identiteta SDK-ovi automatski učinite radu pozivanje broker umjesto vas kad korisnik upućuje na to što žele prijavite se pomoću bilo kojeg računa iz platformu Microsoft Identity. To se može biti osobnog Microsoftova Account, rad ili školskog računa ili računa koje ste omogućili i glavnog računala u Azure pomoću naši proizvodi B2C i B2B. Iznimno putem sigurne algoritmi i šifriranje smo provjerite je li vjerodajnice se od vas zatraži i siguran način isporuke natrag u aplikaciji. Exact tehničke detalje o te mehanizme nisu objavljene, ali ste je razvio s suradnju Apple i Google.

**Odabir je programer ako Microsoft SDK identiteta poziva brokera ili koristi tijek Vođena koje nisu broker.** No ako programer odluči da nećete koristiti tijek broker Potpomognuta izgubit prednost korištenje SSO vjerodajnice da korisnik možda ste već dodali na uređaju, kao i onemogućuje svoje aplikacije koji se koristi sa značajkama tvrtke Microsoft pruža svojih klijenata, primjerice uvjetno pristup mogućnostima upravljanja Intune i provjeru autentičnosti utemeljenu na certifikata.

Ove prijave imati sljedeće prednosti:

-  Korisničkog sučelja SSO svim svojim aplikacijama bez obzira na to dobavljača.
-  Aplikaciju možete pod utjecajem naprednije značajke tvrtke kao što su uvjetno Access ili paket InTune proizvoda.
-  Aplikacija podržava certifikat prema provjere autentičnosti za poslovne korisnike.
- Mnogo sigurnije prijavu iskustvo kao identitet aplikacija i korisnik provjerio broker aplikacijom algoritama dodatnu zaštitu i šifriranje.

Ove prijave imati sljedeće Nedostaci:

- U iOS korisnik je prebačena izvan vaše aplikacije sučelje, dok su odabrali vjerodajnice.
- Gubitak mogućnost upravljanja sučelje za prijavu za kupce u sklopu aplikacije.



Ovdje je prikaz funkcioniranje Microsoft identiteta SDK-ovi s aplikacijama broker da biste omogućili SSO:

```
+------------+ +------------+   +-------------+
|            | |            |   |             |
|   App 1    | |   App 2    |   |   Someone   |
|            | |            |   |    Else's   |
|            | |            |   |     App     |
+------------+ +------------+   +-------------+
| Azure SDK  | | Azure SDK  |   | Azure SDK   |
+-----+------+-+-----+------+-  +-------+-----+
      |              |                  |
      |       +------v------+           |
      |       |             |           |
      |       | Microsoft   |           |
      +-------> Broker      |^----------+
              | Application
              |             |
              +-------------+
              |             |
              |   Broker    |
              |   Storage   |
              |             |
              +-------------+
```

Armed tih informacija pozadinske trebali biste moći bolje razumijevanje i implementirati SSO unutar vaše aplikacije pomoću platforme Microsoft Identity i SDK-ovi.


## <a name="enabling-cross-app-sso-using-adal"></a>Omogućivanje jedinstvene Prijave unakrsno aplikacije pomoću ADAL

Ovdje ćete koristiti ADAL iOS SDK za:

- Uključivanje koje nisu broker Vođena SSO na paket aplikacije
- Uključivanje podrške za broker Potpomognuta SSO

### <a name="turning-on-sso-for-non-broker-assisted-sso"></a>Uključivanje SSO za koje nisu broker Potpomognuta SSO

Nije broker Vođena SSO preko aplikacije Microsoft identiteta SDK-ovi upravljanje velik dio složenost SSO umjesto vas. To obuhvaća pronalaženje odgovarajuća korisnička u predmemoriji i održavanje popisa prijavljenog korisnika za upit.

Da biste omogućili SSO aplikacija nadležnosti vlasnik trebali biste učiniti sljedeće:

1. Provjerite svih korisnika za aplikacije isti ID klijenta ili ID aplikacije.
* Provjerite je li sve aplikacija zajednički isti potpisnog certifikata Apple da bi se keychains možete omogućiti zajedničko korištenje
* Zahtjev za istu prava spremišta lozinki za svaku aplikacija.
* Odrediti SDK-ovi identiteta Microsoft o zajedničkog spremišta lozinki nam da biste koristili.

#### <a name="using-the-same-client-id--application-id-for-all-the-applications-in-your-suite-of-apps"></a>Korištenje iste ID klijenta / aplikacije ID za sve aplikacije paketa aplikacija

U redoslijedu za platformu Microsoft Identity znati je li omogućila zajedničko korištenje tokena preko aplikacija svaki aplikacija morat ćete zajednički koristiti isti ID klijenta ili ID aplikacije. Ovo je jedinstveni identifikator koji ste dobili kada prvog aplikacija registrira na portalu.

Koje možda se pitate se kako će prepoznati različitih aplikacija sa servisom Microsoft Identity ako se koristi u istom ID aplikacije. Odgovor je s **Ji preusmjeravanje**. Svaka aplikacija može imati više preusmjeravanje ji registrirana na portalu za uhodavanje. Svaku aplikaciju u vašem paketu imat će različite preusmjeravanje URI. Primjer kako to izgleda je ispod:

Preusmjeravanje App1 URI:`x-msauth-mytestiosapp://com.myapp.mytestapp`

Preusmjeravanje App2 URI:`x-msauth-mytestiosapp://com.myapp.mytestapp2`

Preusmjeravanje App3 URI:`x-msauth-mytestiosapp://com.myapp.mytestapp3`

....

Te su ugniježđenih isti ID klijenta / ID aplikacije i traže ovisno o preusmjeravanje URI vratite nama u konfiguraciji SDK.

```
+-------------------+
|                   |
|  Client ID        |
+---------+---------+
          |
          |           +-----------------------------------+
          |           |  App 1 Redirect URI               |
          +----------^+                                   |
          |           +-----------------------------------+
          |
          |           +-----------------------------------+
          +----------^+  App 2 Redirect URI               |
          |           |                                   |
          |           +-----------------------------------+
          |
          +----------^+-----------------------------------+
                      |  App 3 Redirect URI               |
                      |                                   |
                      +-----------------------------------+

```


*Imajte na umu da oblik te preusmjeravanje ji je opisano u nastavku. Možete koristiti bilo koji URI preusmjeravanje osim ako ne želite podržava broker, u tom slučaju morate izgledaju poput navedenog*



#### <a name="create-keychain-sharing-between-applications"></a>Stvaranje spremišta lozinki zajedničko korištenje između aplikacija

Omogućivanje zajedničkog korištenja spremišta lozinki izlazi iz ovog dokumenta i prekriveni Apple u dokumentu [Mogućnosti dodavanja](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html). Što je važno je odlučiti što želite da vaše spremišta lozinki pozivanje i dodajte tu mogućnost preko svih aplikacija.

Kada imate prava pravilno ste postavili trebali biste vidjeti datoteke u projekt direktorija pravo `entitlements.plist` koja sadrži nešto koji izgleda ovako:

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>keychain-access-groups</key>
    <array>
        <string>$(AppIdentifierPrefix)com.myapp.mytestapp</string>
        <string>$(AppIdentifierPrefix)com.myapp.mycache</string>
    </array>
</dict>
</plist>
```

Kada imate prava spremišta lozinki omogućen u svakoj aplikacija i spremni ste pomoću jedinstvene Prijave, obavijestite Microsoft SDK identiteta o vašem spremišta lozinki pomoću sljedeće postavke vašeg `ADAuthenticationSettings` uz sljedeće postavke:

```
defaultKeychainSharingGroup=@"com.myapp.mycache";
```

> [AZURE.WARNING]
Kada omogućite zajedničko korištenje s spremišta lozinki preko aplikacija bilo koju aplikaciju možete izbrisati korisnika ili lošiji brisanje svih tokena preko aplikacija. To je osobito disastrous ako imate aplikacije koje se temelje na tokeni za rad u pozadini. Zajedničko korištenje s spremišta lozinki znači da moraju biti vrlo oprezni u sve uklonite operacije kroz Microsoft identiteta SDK-ovi.

To je to! Microsoft SDK identiteta sada dijeliti vjerodajnice preko svih aplikacija. Popis korisnika i moguće zajednički koristiti kroz sve instance aplikacije.

### <a name="turning-on-sso-for-broker-assisted-sso"></a>Uključivanje SSO za broker Potpomognuta SSO

Mogućnost za aplikaciju da biste koristili neki broker koja je instalirana na uređaj je **po zadanome isključen**. Da biste mogli koristiti aplikaciju s brokera morate učiniti nekoliko dodatnih konfiguraciju i dodavanje koda za neke aplikacije.

Koraci za praćenje su:

1. Omogućivanje načina rada broker u kodu aplikacije poziv MS SDK.
2. Uspostavljanje novi preusmjeravanje URI i koji omogućuje aplikaciju i svoju registraciju aplikacije.
3. Registracija sheme URL-a.
4. Podrška za iOS9: dodavanje dozvole u info.plist datoteku.


#### <a name="step-1-enable-broker-mode-in-your-application"></a>Korak 1: Omogućivanje broker načina rada u aplikaciji
Mogućnost za svoju aplikaciju za korištenje brokera uključena prilikom stvaranja "kontekst" ili početnog postavljanja objekta provjere autentičnosti. To se postavljanjem vrsti vašega vjerodajnice u kodu:

```
/*! See the ADCredentialsType enumeration definition for details */
@propertyADCredentialsType credentialsType;
```
Na `AD_CREDENTIALS_AUTO` postavka omogućuje Microsoft SDK identiteta pokušati pozivanje broker, `AD_CREDENTIALS_EMBEDDED` onemogućuje Microsoft SDK identiteta zovete da biste brokera.

#### <a name="step-2-registering-a-url-scheme"></a>Korak 2: Registracija sheme URL-a
Platforme Microsoft Identity koristi URL-ovi za pozivanje brokera, a zatim se vratite kontrolu natrag u aplikaciji. Da biste dovršili te trajanjem putovanja morate URL shema registriran za aplikaciju koja će zna platformu Microsoft Identity. To može biti uz sve ostale aplikacije sheme koju ste prethodno registriran s aplikacijom.

> [AZURE.WARNING]
Preporučuje se upućivanje URL shema prilično jedinstveni da biste minimizirali vjerojatno nekoj drugoj aplikaciji koristeći istu shemu URL-a. Apple nametnuti jedinstvenosti sheme URL-a registrirane u trgovini app store.

Slijedi primjer kako to izgleda u konfiguraciji projekta. To može učiniti i u XCode kao i:

```
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
        <key>CFBundleURLName</key>
        <string>com.myapp.mytestapp</string>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>x-msauth-mytestiosapp</string>
        </array>
    </dict>
</array>
```

#### <a name="step-3-establish-a-new-redirect-uri-with-your-url-scheme"></a>Korak 3: Uspostavljanje novi preusmjeravanje URI s URL shema

Da biste bili sigurni da ne možemo uvijek vratiti tokeni vjerodajnica ispravnim zatvaranjem, moramo provjerite je li smo pozvati u aplikaciji na način koji operacijski sustav iOS možete provjeriti. Operacijski sustav iOS izvješća broker aplikacije Microsoft ID paket aplikacije pozivanje ga. To nije moguće spoofed rogue aplikacije. Dakle, ne možemo pod utjecajem to uz URI naš broker aplikacije da biste bili sigurni tokena vraćaju odgovarajuće aplikacije. Ne možemo potrebno da biste ostvarili ovaj jedinstveni preusmjeravanje URI i u aplikaciji i postaviti kao URI preusmjeravanje u našem portala za razvojne inženjere.

Preusmjeravanje URI mora biti u obliku proper:

`<app-scheme>://<your.bundle.id>`

Ex: *x msauth mytestiosapp://com.myapp.mytestapp*

Ovaj URI preusmjeravanje mora da bude navedena u svoju registraciju aplikacije pomoću [Azure klasični portal](https://manage.windowsazure.com/). Dodatne informacije o Registracija aplikacije Azure AD potražite u članku [Integrating s Azure Active Directory](./develop/active-directory-how-to-integrate.md).


##### <a name="step-3a-add-a-redirect-uri-in-your-app-and-dev-portal-to-support-certificate-based-authentication"></a>Drugi korak 3a: dodavanje preusmjeravanje URI u aplikaciju i razvojni portal da podržava provjeru autentičnosti certifikat prema

Za podršku certifikata provjeru autentičnosti utemeljenu na drugi "msauth" mora biti registriran u aplikaciji i [Azure klasični portal](https://manage.windowsazure.com/) za rukovanje provjera autentičnosti potvrda ako želite dodati koji podržavaju u aplikaciji.

`msauth://code/<broker-redirect-uri-in-url-encoded-form>`

Ex: *msauth://code/x-msauth-mytestiosapp%3A%2F%2Fcom.myapp.mytestapp*


#### <a name="step-4-ios9-add-a-configuration-parameter-to-your-app"></a>Korak 4: iOS9: dodajte parametar za konfiguraciju aplikacije

ADAL koristi – canOpenURL: da biste provjerili je li instaliran brokera na uređaju. U iOS 9 Apple zaključan dolje što sheme neku aplikaciju možete upit. Morat ćete dodati "msauth" LSApplicationQueriesSchemes dio vašeg `info.plist file`.

<key>LSApplicationQueriesSchemes</key>
<array>
     <string>msauth</string>
</array>

### <a name="youve-configured-sso"></a>Konfigurirali ste SSO!

Sada Microsoft SDK identiteta će automatski vjerodajnice zajednički aplikacija i pozivanje brokera ako postoje na svoj uređaj.
