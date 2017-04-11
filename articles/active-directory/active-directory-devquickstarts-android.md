<properties
    pageTitle="Azure AD Android Uvod | Microsoft Azure"
    description="Sastavljanje Android aplikacije koja integrira Azure AD za prijavu i poziva Azure AD zaštićen API-ji pomoću OAuth."
    services="active-directory"
    documentationCenter="android"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>

# <a name="integrate-azure-ad-into-an-android-app"></a>Integriranje Azure AD u aplikacija za Android

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)] 

Ako razvijate aplikaciji za stolna računala, Azure AD olakšava jednostavne i jasan za provjeru autentičnosti korisnika pomoću računa za Active Directory.  Omogućuje i aplikacije da biste sigurno zauzeti sve API zaštićen Azure AD, kao što su API-ji sa sustavom Office 365 ili Azure API na webu.

Android klijenti koje moraju imati pristup zaštićeni resursa, Azure AD predstavlja provjera autentičnosti biblioteke imenika Active Directory ili ADAL.  ADAL-isključivom svrhom u životu je da biste olakšali aplikacije da biste dobili pristup tokena.  Da bismo pokazali samo kako je lako, ovdje bismo ćete stvoriti aplikacija sa sustavom Android popis zadataka koji:

-   Uzima pristupiti tokeni za pozivanje API za popis zadataka za [protokol OAuth 2.0 provjere autentičnosti](https://msdn.microsoft.com/library/azure/dn645545.aspx).
-   Dobiti na korisnikovu popisu zadataka
-   Znak korisnika izgleda.

Da bismo započeli, potreban vam je klijent za Azure AD Dodavanje korisnika i Registracija aplikacije.  Ako još nemate klijent, [Saznajte kako da biste ga nabavili](active-directory-howto-tenant.md).

> [AZURE.TIP] Isprobajte pretpregled značajke naš novi [portala za razvojne inženjere](https://identity.microsoft.com/Docs/Android) koji će vam omogućiti brz početak rada s Azure Active Directory u samo nekoliko minuta!  Portala za razvojne inženjere će vas voditi kroz postupak registracije aplikacije i integracija Azure AD u kodu.  Kada završite, imat ćete jednostavan aplikacije koje možete provjere autentičnosti korisnika u klijentu i s pozadinskom koje možete prihvatiti tokeni i izvršiti provjeru valjanosti. 

## <a name="step-1-download-and-run-the-nodejs-rest-api-todo-sample-server"></a>Korak 1: Preuzimanje i pokretanje poslužitelja za uzorak Node.js REST API obveze

Ovaj primjer zapisuje posebno za rad s oglednim postojeće za stvaranje jednog klijenta poslova REST API-JA za Microsoft Azure Active Directory. Ovo je stara obavezna za brzo pokretanje.

Informacije o tome kako postaviti posjetite naš postojeće uzoraka ovdje:

* [Servis API REST web-aplikacije Microsoft Azure Active Directory uzorka za Node.js](active-directory-devquickstarts-webapi-nodejs.md)

## <a name="step-2-register-your-web-api-with-your-microsoft-azure-ad-tenant"></a>Korak 2: Registrirate Web API klijenta sustava Microsoft Azure AD

**Što možemo učiniti?**

*Microsoft Active Directory podržava dodavanje dvije vrste aplikacije. Web-API-ji koje nude usluge za korisnicima i aplikacijama (ili na webu ili aplikacije koje se izvode na uređaju) koji pristup tim API-ji Web. U ovom ćete koraku registriranja API Web koji se izvodi lokalno za testiranje ovaj uzorak. Obično taj API Web bi servisa REST koji nudi funkcije želite aplikacije za pristup. Microsoft Azure Active Directory možete zaštititi sve krajnje točke!*

*U nastavku ćemo su pretpostavkom registriranja obveze REST API-JA poziva iznad, ali to funkcionira za sve API Web htjeli Azure Active Directory za zaštitu.*

Koraci za registriranje Web API Microsoft Azure AD

1. Prijavite se na [portal za upravljanje Azure](https://manage.windowsazure.com).
2. U navigaciju lijevoj strani kliknite na servisu Active Directory
3. Kliknite klijentu direktorija gdje želite da biste registrirali primjer aplikacije.
4. Kliknite karticu aplikacije.
5. U ladica, kliknite Dodaj.
6. Kliknite "Dodaj aplikaciju je moje tvrtke ili ustanove razvoju".
7. Unesite neslužbeni naziv aplikacije, na primjer "TodoListService", odaberite "Web aplikacije and/or Web API-JA", a zatim kliknite Dalje.
8. Za prijavu URL-a, unesite URL osnovni uzorka, što je prema zadanim postavkama `https://localhost:8080`.
9. ID URI aplikacije unesite `https://<your_tenant_name>/TodoListService`, zamjene `<your_tenant_name>` pod nazivom Azure AD klijentu.  Kliknite u redu da biste dovršili registraciju.
10. Ne napuštajući portala za Azure karticu konfiguracija aplikacije.
11. **Pronalaženje vrijednosti ID klijenta i njezino kopiranje odvojite**, trebat će vam to kasnije prilikom konfiguriranja aplikacije.

## <a name="step-3-register-the-sample-android-native-client-application"></a>Korak 3: Registrirati aplikacija sa sustavom Android nativni klijent uzorka

Registracija web-aplikaciju prvi je korak. Nakon toga morat ćete odrediti Azure Active Directory o vaše aplikacije. Time se omogućuje aplikacija možete komunicirati s samo registrirani API Web

**Što možemo učiniti?**  

*Kao što je rečeno iznad, Microsoft Azure Active Directory podržava dodavanje dvije vrste aplikacije. Web-API-ji koje nude usluge za korisnicima i aplikacijama (ili na webu ili aplikacije koje se izvode na uređaju) koji pristup tim API-ji Web. U ovom ćete koraku registriranja aplikacije u ovom primjeru. Morate to redoslijedom za ovu aplikaciju da biste mogli zahtjeva za pristup Web API samo registriran. Azure Active Directory će odbiti čak i dopustili aplikaciji zatražiti prijavu osim ako je registriran! Koja je dio sigurnosnih modela.*

*U nastavku ćemo su pretpostavkom registriranja ovu aplikaciju uzorak poziva iznad, ali to funkcionira za bilo koju aplikaciju razvijate.*

**Zašto se I umetanja aplikacije i Web API u jednog klijenta?**

*Kako vam može imati pogoditi, nije moguće sastaviti aplikaciju koja se može pristupiti vanjskim API koja je registrirana u servisu Azure Active Directory iz drugi klijent. Ako to učinite, korisnici će se zatražiti da pristanak na korištenje API-JA u aplikaciji. Bolje dio je, provjera autentičnosti biblioteke imenika Active Directory za iOS vodi brigu o ovaj pristanak za vas! Kao što smo pristupiti naprednije značajke, vidjet ćete ovo je važan dio posla potrebnog za pristup paket APIs Microsoft Azure i sustava Office kao i bilo kojeg davatelja usluga. Zasad jer je registrirana vaša Web API i aplikacije u istom klijentu nećete vidjeti bilo koji od vas traži pristanak. To je obično slučaj ako razvijate aplikaciju samo za vlastite tvrtke.*

1. Prijavite se na [portal za upravljanje Azure](https://manage.windowsazure.com).
2. U navigaciju lijevoj strani kliknite na servisu Active Directory
3. Kliknite klijentu direktorija gdje želite da biste registrirali primjer aplikacije.
4. Kliknite karticu aplikacije.
5. U ladica, kliknite Dodaj.
6. Kliknite "Dodaj aplikaciju je moje tvrtke ili ustanove razvoju".
7. Upišite neslužbeni naziv aplikacije, na primjer "TodoListClient-Android", odaberite "nativni klijentska aplikacija", a zatim kliknite sljedeće.
8. URI preusmjeravanje unesite `http://TodoListClient`.  Kliknite Završi.
9. Kliknite karticu konfiguracija aplikacije.
10. Pronalaženje vrijednosti ID klijenta i njezino kopiranje odvojite, potrebno je to kasnije prilikom konfiguriranja aplikacije.
11. "Dozvole za ostale aplikacije", kliknuti "Dodaj aplikaciju."  Odaberite "Ostalo" na padajućem popisu "Prikaz", a zatim kliknite gornji kvačicu.  Pronađite i kliknite u TodoListService i kliknite kvačicu dolje da biste dodali aplikaciju.  Odaberite "Access TodoListService" na padajućem izborniku "Dodijeliti dozvole", a spremanje konfiguracije.



Da biste sastavili s Maven, možete koristiti u pom.xml na najvišoj razini

  * Kloniraj repo u direktoriju po izboru:

  `$ git clone git@github.com:AzureADSamples/NativeClient-Android.git`  

  * Slijedite korake u [odjeljku Prerequests za postavljanje vašeg maven za android](https://github.com/MSOpenTech/azure-activedirectory-library-for-android/wiki/Setting-up-maven-environment-for-Android)
  * Postavljanje emulator s SDK 19
  * Idite u korijensku mapu u kojoj klonirana na repo
  * Pokrenite naredbu: instalacija mvn clean
  * Promijeniti direktorij u uzorku brzi početak rada: samples\hello CD-a
  * Pokrenite naredbu: mvn sa sustavom android: implementacija android: pokretanje
  * Trebali biste vidjeti pokretanje aplikacije
  * Unesite korisničke vjerodajnice za testiranje pokušati!

Posudu paketa će se i poslati pokraj aar paketa.

### <a name="step-4-download-the-android-adal-and-add-it-to-your-eclipse-workspace"></a>Korak 4: Preuzimanje Android ADAL i dodajte ga Eclipse radnog prostora

Unijeli smo je jednostavno je više mogućnosti da biste koristili tu biblioteku u Android projektu:

* Izvorni kod možete koristiti da biste uvezli biblioteke u Eclipse i veza u aplikaciji.
* Ako koristite Android Studio, možete koristiti oblik paketa *aar* i referencu u binarne datoteke.

####<a name="option-1-source-zip"></a>Mogućnost 1: Zip izvor

Da biste preuzeli kopiju izvornog koda na desnoj strani stranice kliknite "Preuzimanje poštanski broj" ili kliknite [ovdje](https://github.com/AzureAD/azure-activedirectory-library-for-android/archive/v1.0.9.tar.gz).

####<a name="option-2-source-via-git"></a>Mogućnost 2: Izvor putem brojka

Da biste dobili šifru izvora SDK putem brojka samo upišite:

    git clone git@github.com:AzureAD/azure-activedirectory-library-for-android.git
    cd ./azure-activedirectory-library-for-android/src

####<a name="option-3-binaries-via-gradle"></a>Mogućnost 3: Binarne datoteke putem Gradle

U binarne datoteke možete pristupiti iz središnje repo Maven. Paket AAR možete uvrstiti na sljedeći način u projektu u AndroidStudio:

```gradle
repositories {
    mavenCentral()
    flatDir {
        dirs 'libs'
    }
    maven {
        url "YourLocalMavenRepoPath\\.m2\\repository"
    }
}
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile('com.microsoft.aad:adal:1.1.1') {
        exclude group: 'com.android.support'
    } // Recent version is 1.1.1
}
```

####<a name="option-4-aar-via-maven"></a>Mogućnost 4: aar putem Maven

Ako koristite dodatak za m2e Eclipse, možete odrediti ovisnosti u datoteci pom.xml:

```xml
<dependency>
    <groupId>com.microsoft.aad</groupId>
    <artifactId>adal</artifactId>
    <version>1.1.1</version>
    <type>aar</type>
</dependency>
```


####<a name="option-5-jar-package-inside-libs-folder"></a>Mogućnost 5: posudu paketa mapi libs
Možete dobiti posudu datoteku iz maven u repo i ispustite u mapu *libs* u projektu. Morate kopirati potrebni resursi u projekt jer paketa posudu Nemoj uvrštavati ih.


### <a name="step-5-add-references-to-android-adal-to-your-project"></a>Korak 5: Dodavanje reference na Android ADAL u projekt


2. Dodajte referencu u projekt i navesti kao Android biblioteke. Ako niste sigurni kako to učiniti, [kliknite ovdje da biste saznali više] (http://developer.android.com/tools/projects/projects-eclipse.html)

3. Dodavanje ovisnost projekta za ispravljanje pogrešaka u postavkama projekta

4. Ažuriranja u projekt AndroidManifest.xml datoteku da biste uvrstili:

    ```Java
      <uses-permission android:name="android.permission.INTERNET" />
      <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
      <application
            android:allowBackup="true"
            android:debuggable="true"
            android:icon="@drawable/ic_launcher"
            android:label="@string/app_name"
            android:theme="@style/AppTheme" >

            <activity
                android:name="com.microsoft.aad.adal.AuthenticationActivity"
                android:label="@string/title_login_hello_app" >
            </activity>
      ....
      <application/>
    ```

7. Stvaranje instance komponente AuthenticationContext na glavnom aktivnosti. Detalje o ovog poziva su obuhvaćeno ovom README, ali možete dobiti dobar start tako da pogledate [Android nativni klijent uzorka](https://github.com/AzureADSamples/NativeClient-Android). U nastavku je primjeru:

    ```Java
    // Authority is in the form of https://login.windows.net/yourtenant.onmicrosoft.com
    mContext = new AuthenticationContext(MainActivity.this, authority, true); // This will use SharedPreferences as            default cache
    ```
  * Napomena: mContext je polje u aktivnosti

8. Kopirajte ovaj blok kod za rukovanje kraj AuthenticationActivity kada korisnik unese vjerodajnice i prima autorizacije kod:

    ```Java
     @Override
     protected void onActivityResult(int requestCode, int resultCode, Intent data) {
         super.onActivityResult(requestCode, resultCode, data);
         if (mContext != null) {
             mContext.onActivityResult(requestCode, resultCode, data);
         }
     }
    ```

9. Zatražiti token definirate u povratnog

    ```Java
    private AuthenticationCallback<AuthenticationResult> callback = new AuthenticationCallback<AuthenticationResult>() {

            @Override
            public void onError(Exception exc) {
                if (exc instanceof AuthenticationException) {
                    textViewStatus.setText("Cancelled");
                    Log.d(TAG, "Cancelled");
                } else {
                    textViewStatus.setText("Authentication error:" + exc.getMessage());
                    Log.d(TAG, "Authentication error:" + exc.getMessage());
                }
            }

            @Override
            public void onSuccess(AuthenticationResult result) {
                mResult = result;

                if (result == null || result.getAccessToken() == null
                        || result.getAccessToken().isEmpty()) {
                    textViewStatus.setText("Token is empty");
                    Log.d(TAG, "Token is empty");
                } else {
                    // request is successful
                    Log.d(TAG, "Status:" + result.getStatus() + " Expired:"
                            + result.getExpiresOn().toString());
                    textViewStatus.setText(PASSED);
                }
            }
        };
    ```
10. Na kraju, zatražite token pomoću tog povratnog:

    ```Java
     mContext.acquireToken(MainActivity.this, resource, clientId, redirect, user_loginhint, PromptBehavior.Auto, "",
                    callback);
    ```

Objašnjenje parametre:

  * Resurs potreban je te je resurs koji pokušavate pristupiti.
  * Clientid potreban je i dolazi s portala sustava AzureAD.
  * RedirectUri možete postaviti kao vaš naziv paketa. Nije potrebno koristiti za poziv acquireToken.
  * PromptBehavior olakšava traži vjerodajnice da biste preskočili predmemorije i kolačića.
  * Povratnog će se pozivati nakon autorizacije kod se razmjenjuju token.

  Povratnog će imati objekta AuthenticationResult koja ima accesstoken, istekao rok i informacije o idtoken.

Neobavezno: **acquireTokenSilent**

Možete nazvati **acquireTokenSilent** učiniti predmemoriranje i tokena Osvježi. Pruža kao i verzija za sinkronizaciju. Korisnički ID Prihvati kao parametar.

    ```java
     mContext.acquireTokenSilent(resource, clientid, userId, callback );
    ```

11. **Broker**: tvrtke Microsoft Intune-portal za aplikacije vam ponuditi komponentu broker. ADAL će koristi stvaranja računa broker, ako postoji jedan korisnički račun na ovom autentikatora i za razvojne inženjere odabrati da ne ga preskočite. Za razvojne inženjere možete preskočiti broker korisnika s:

    ```java
     AuthenticationSettings.Instance.setSkipBroker(true);
    ```

 Mora registrirati posebno redirectUri za korištenje broker za razvojne inženjere. RedirectUri je u obliku msauth://packagename/Base64UrlencodedSignature. Možete dobiti na redirecturi aplikacije pomoću skripte "brokerRedirectPrint.ps1" ili mContext.getBrokerRedirectUri poziv API-JA. Potpis je povezan s potpisnog certifikata.

 Trenutni broker modela je za jednog korisnika. AuthenticationContext nudi API način da biste dobili broker korisnika.

 ```java
 String brokerAccount =  mContext.getBrokerUser();
 ```
 Broker korisnika će se vratiti ako je valjan račun.

 Vaše aplikacije manifest mora imati dozvolu za korištenje AccountManager računa: http://developer.android.com/reference/android/accounts/AccountManager.html

 * GET_ACCOUNTS
 * USE_CREDENTIALS
 * MANAGE_ACCOUNTS


Koristite ovaj vodič, imat ćete što vam je potrebno da biste uspješno integrirati s Azure Active Directory. Još primjera ovaj rada potražite u AzureADSamples / spremište na GitHub.

## <a name="important-information"></a>Važne informacije

### <a name="customization"></a>Prilagodba

Biblioteka resursima za projekt možete prebrisati resursi za aplikacije. To se događa pri aplikacijom sastavlja. Zbog toga, možete prilagoditi izgled provjere autentičnosti aktivnosti onako kako želite. Morate da biste bili sigurni da bi taj ADAL uses(Webview) id kontrole.

### <a name="broker"></a>Broker

Komponenta broker biti isporučena s tvrtke Microsoft Intune-portal za aplikacije. Račun će se stvoriti u Upravitelj računa. Vrsta računa je "com.microsoft.workaccount". Omogućuje se samo jedan račun SSO. Stvorit će kolačića SSO za tog korisnika nakon dovršetka uređaj test za jednu od aplikacija.

### <a name="authority-url-and-adfs"></a>Url za izdavanje certifikata i ADFS

ADFS nije prepoznata kao proizvodne STS, tako da morate isključiti instancu otkrivanje i proći false u AuthenticationContext Graditelj.

Url za izdavanje certifikata mora STS instancu i ime klijenta: https://login.windows.net/yourtenant.onmicrosoft.com

### <a name="querying-cache-items"></a>Slanje upita stavki u predmemoriji

ADAL pruža zadane predmemorije u SharedPreferences s nekoliko jednostavnih predmemorije funkcije upita. Trenutni predmemorije možete doći s AuthenticationContext sa:
```Java
 ITokenCacheStore cache = mContext.getCache();
```
Možete unijeti i implementaciju predmemorije, ako želite da biste ga prilagodili.
```Java
mContext = new AuthenticationContext(MainActivity.this, authority, true, yourCache);
```

### <a name="promptbehavior"></a>PromptBehavior

ADAL nudi mogućnost navedite upit ponašanjem. PromptBehavior.Auto pojavit će se korisničko Sučelje ako osvježavanje token nije valjan, a potrebni su korisničke vjerodajnice. PromptBehavior.Always će preskočite korištenje predmemorije i stalan prikaz korisničkog Sučelja.

### <a name="silent-token-request-from-cache-and-refresh"></a>Tihu tokena zahtjev iz predmemorije i osvježavanje

Ovu metodu koristite korisničko Sučelje pop gore, a ne zahtijeva aktivnost. To će vratiti tokena iz predmemorije ako je dostupna. Ako je istekao token će pokušati osvježiti ga. Ako osvježavanje token je istekla ili nije uspjela, će vratiti AuthenticationException.

    ```Java
    Future<AuthenticationResult> result = mContext.acquireTokenSilent(resource, clientid, userId, callback );
    ```

Možete promijeniti i sinkronizaciju poziva Ako koristite taj način. Možete postaviti null za povratnog ili acquireTokenSilentSync.

### <a name="diagnostics"></a>Dijagnostika

Ovo su primarni izvora podataka za dijagnosticiranje problema:

+ Iznimke
+ Zapisnici
+ Kašnjenja mreže

Osim toga, imajte na umu da ID korelacije središnje za dijagnostiku u biblioteci. Možete postaviti svoje korelacije ID-a na temelju zahtjeva za po upute za povezivanje programa ADAL zahtjev s ostalih operacija u kodu. Ako ne postavite korelacija id, a zatim ADAL generirat će na slučajni nešto and sve zapisnika poruka i poziva mreže će biti oznaku ID korelacije. Na samopotpisani generira id promjene na svaki zahtjev za potvrdu.

#### <a name="exceptions"></a>Iznimke

Ovo je pogrešno prvi Dijagnostika. Pokušati ne možemo pružiti korisne poruka. Ako pronađete onu koja je korisno prijavite problem i Javite nam. Također sadrže informacije o uređaju kao što su modela i SDK #.

#### <a name="logs"></a>Zapisnici

Možete konfigurirati biblioteke da biste generirali prijavi poruke koje možete koristiti za dijagnostiku problema. Konfiguriranje zapisivanja podataka po upućivanje poziva pomoću sljedeće da biste konfigurirali povratnog kojima ADAL će se koristiti za predavanje isključivanje svake poruke zapisnika kao generira.


 ```Java
 Logger.getInstance().setExternalLogger(new ILogger() {
     @Override
     public void Log(String tag, String message, String additionalMessage, LogLevel level, ADALError errorCode) {
      ...
      // You can write this to logfile depending on level or errorcode.
      writeToLogFile(getApplicationContext(), tag +":" + message + "-" + additionalMessage);
     }
 }
 ```
Poruke se mogu biti napisani prilagođene datoteke zapisnika kao što se vidi ispod. Nažalost, ne postoji standardni način početka zapisnika s uređaja. Postoje neki servisi koji vam mogu pomoći s ovom. Koje možete i iznaći vlastitih, kao što su slanja datoteke na poslužitelju.

```Java
private syncronized void writeToLogFile(Context ctx, String msg) {
       File directory = ctx.getDir(ctx.getPackageName(), Context.MODE_PRIVATE);
       File logFile = new File(directory, "logfile");
       FileOutputStream outputStream = new FileOutputStream(logFile, true);
       OutputStreamWriter osw = new OutputStreamWriter(outputStream);
       osw.write(msg);
       osw.flush();
       osw.close();
}
```

##### <a name="logging-levels"></a>Razine zapisivanja

+ Error(Exceptions)
+ Warn(Warning)
+ Info (informacije svrhe)
+ Tekstni (više detalja)

Morate postaviti na razinu zapisnika ovako:
```Java
Logger.getInstance().setLogLevel(Logger.LogLevel.Verbose);
 ```

 Sve poruke zapisnika šalju logcat uz sve prilagođene zapisnika pozive.
Zapisnik logcat za obrazac datoteke možete dobiti kao što je prikazano belog:

 ```
  adb logcat > "C:\logmsg\logfile.txt"
 ```
 Još primjera o adb cmds: https://developer.android.com/tools/debugging/debugging-log.html#startingLogcat

#### <a name="network-traces"></a>Kašnjenja mreže

Da biste snimili promet HTTP koje generira ADAL možete koristiti različite alate.  To je najčešće korisno ako ste upoznati s protokolom OAuth ili ako je potrebno unijeti dijagnostičke informacije Microsoftu ni drugih kanala podrška.

Fiddler je najjednostavnije HTTP alat za praćenje.  Pomoću sljedećih veza za postavljanje najviše pravilno zapisa ADAL mrežni promet.  Da bi biti korisno je potrebno da biste konfigurirali fiddler ili bilo koji drugi alat kao što su Charles snimite šifrirane promet SSL.  Napomena: Kašnjenja generira na taj način mogu sadržavati korisnik s velikim ovlastima podatke kao što su pristupna tokena, korisnička imena i lozinke.  Ako koristite radni račune, dijeliti te kašnjenja s 3 stranama.  Ako je potrebno navesti praćenje nekome da biste dobili podršku reproduciranje problema s računom privremene s korisnička imena i lozinke koje ne smetaju zajedničko korištenje.

+ [Postavljanje Fiddler za Android](http://docs.telerik.com/fiddler/configure-fiddler/tasks/ConfigureForAndroid)
+ [Konfiguriranje Fiddler pravila za ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki/How-to-listen-to-httpUrlConnection-in-Android-app-from-Fiddler)


### <a name="dialog-mode"></a>Dijaloški okvir načinu
način acquireToken bez aktivnosti podržava dijaloški upit.

### <a name="encryption"></a>Šifriranje

ADAL šifrira tokeni i trgovine u SharedPreferences prema zadanim postavkama. Možete pogledati klase StorageHelper da biste vidjeli detalje. Android uvedena AndroidKeyStore za sigurnu pohranu privatni ključevi 4.3(API18). ADAL koji se koristi za API18 i noviji. Ako želite koristiti ADAL za verzije na donjem SDK morate unijeti tajnu tipku po AuthenticationSettings.INSTANCE.setSecretKey

### <a name="oauth2-bearer-challenge"></a>Test Oauth2 nošenja

Klase AuthenticationParameters nudi funkcije da biste na authorization_uri s Oauth2 nošenja test.

### <a name="session-cookies-in-webview"></a>Sesije kolačića u prikaz za web

Android prikaza očistite kolačiće sesiju nakon zatvaranja aplikacije. To možete učiniti kod uzorka ispod:
```java
CookieSyncManager.createInstance(getApplicationContext());
CookieManager cookieManager = CookieManager.getInstance();
cookieManager.removeSessionCookie();
CookieSyncManager.getInstance().sync();
```
Više o tome da Kolačići: http://developer.android.com/reference/android/webkit/CookieSyncManager.html

### <a name="resource-overrides"></a>Nadjačavanja resursa

Biblioteku ADAL obuhvaća engleski nizova za sljedeće dvije ProgressDialog poruke.

Aplikacija trebali biste ih prebrisati želji lokalizirane nizova.

```Java
<string name="app_loading">Loading...</string>
<string name="broker_processing">Broker is processing</string>
<string name="http_auth_dialog_username">Username</string>
<string name="http_auth_dialog_password">Password</string>
<string name="http_auth_dialog_title">Sign In</string>
<string name="http_auth_dialog_login">Login</string>
<string name="http_auth_dialog_cancel">Cancel</string>
```

=======

### <a name="ntlm-dialog"></a>Dijaloški okvir NTLM
Adal verzija 1.1.0 podržava NTLM dijaloški okvir koji se obrađuju kroz onReceivedHttpAuthRequest događaj iz WebViewClient. Dijaloški okvir izgleda i nizova moguće je prilagoditi.

### <a name="cross-app-sso"></a>SSO unakrsno aplikacije
Saznajte [kako omogućiti SSO unakrsno aplikacije na pomoću ADAL sa sustavom Android](active-directory-sso-android.md)  


[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
