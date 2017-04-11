<properties
    pageTitle="Azure AD Cordova Uvod | Microsoft Azure"
    description="Sastavljanje Cordova aplikacije koja integrira Azure AD za prijavu i poziva Azure AD zaštićen API-ji pomoću OAuth."
    services="active-directory"
    documentationCenter=""
    authors="vibronet"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="vittorib"/>

# <a name="integrate-azure-ad-with-an-apache-cordova-app"></a>Azure AD integrirati programa Apache Cordova aplikacije

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Apache Cordova zahvaljujući razvoj aplikacija za HTML5/JavaScript koja se može pokrenuti na mobilnim uređajima kao full-fledged izvornim aplikacijama.
Azure AD enterprise ocjene provjere autentičnosti mogućnosti možete dodati svojim aplikacijama Cordova. Zahvaljujući Cordova dodatak prelamanje Azure AD nativni SDK-ovi na iOS i Android, iz Windows trgovine i Windows Phone možete poboljšati aplikacija podržava znak s računa vaših korisnika AD, ostvariti pristup sustavu Office 365 i Azure API-JA i čak i zaštiti pozive na vlastitu prilagođenu API Web.

U ovom ćete praktičnom vodiču koristit ćemo Apache Cordova dodatak za Active Directory provjera autentičnosti biblioteke (ADAL) da biste poboljšali jednostavne aplikacija sljedeće značajke:

-   Uz samo nekoliko redaka koda, autentičnost AD korisnik i dobiti tokena za pozivanje API Azure AD grafikonu.
-   Pomoću tog tokena pozvati grafikonu API-JA za taj imenik za slanje upita i prikaz rezultata  
-   Korištenje ADAL tokena predmemoriju za minimiziranje unositi vjerodajnice za korisnika.

Da biste to učinili, morat ćete:

2. Registracija aplikacije s Azure AD
2. Dodavanje koda za aplikacije za zahtjev tokena
3. Dodavanje koda za korištenje token za ispitivanje API grafikonu i prikaz rezultata.
4. Stvaranje projekta Cordova implementaciju sa svim platformama ciljano i dodatak za Cordova ADAL i testiranje rješenje u Emulatora.

## <a name="0--prerequisites"></a>*0. preduvjeti za*

Da biste dovršili ovaj Praktični vodič ćete:

- Klijent za Azure AD kojoj imate račun s pravima za razvoj aplikacija
- Okruženje za razvoj konfiguriran za korištenje Apache Cordova  

Ako imate oba već postavili, provjerite prijeđite izravno na korak 1.

Ako nemate klijent za Azure AD, možete pronaći [upute da biste ga nabavili ovdje](active-directory-howto-tenant.md).

Ako nemate Apache Cordova postavljanje na vašem računalu, instalirajte sljedeće:

- [Brojka](http://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
- [NodeJS](https://nodejs.org/download/)
- [Cordova EŽA](https://cordova.apache.org/) (mogu se jednostavno instalirati putem upravitelja paketima NPM: `npm install -g cordova`)

Imajte na umu da se one surađivati na Računalu i za Mac.

Svaki ciljne platforme ima različite preduvjeti.

- Stvaranje i pokretanje Windows Tablet PC ili verzije aplikacije telefona
    - [Visual Studio 2013 za Windows s ažuriranju 2 ili noviji](http://www.visualstudio.com/downloads/download-visual-studio-vs#d-express-windows-8) (Express ili neka druga verzija).
- Stvaranje i pokretanje za iOS
    -   Xcode 5.x ili noviji. Preuzmite http://developer.apple.com/downloads ili [Mac App Store](http://itunes.apple.com/us/app/xcode/id497799835?mt=12)
    -   [ios sim](https://www.npmjs.org/package/ios-sim) – omogućuje vam pokretanje aplikacija iOS u iOS Simulator iz naredbenog retka (mogu se jednostavno instalirati putem na terminal: `npm install -g ios-sim`)

- Stvaranje i pokretanje aplikacija za Android
    - Instalacija [Programskog jezika Java Development Kit (JDK) 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) ili noviji. Provjerite je li `JAVA_HOME` (varijable okruženja) ispravno je postavljena prema put instalacije JDK (primjerice C:\Program Files\Java\jdk1.7.0_75).
    - Instalacija [Android SDK](http://developer.android.com/sdk/installing/index.html?pkg=tools) i dodavanje `<android-sdk-location>\tools` mjesto (na primjer, C:\tools\Android\android-sdk\tools) za vaše `PATH` varijable okruženja.
    - Otvorite upravitelj SDK Android (na primjer, putem terminal: `android`) i instalacija
    - *Android 5.0.1 (21 API -JA)* platform SDK
    - *Sastavljanje Alati za android SDK* verziju 19.1.0 ili noviji
    - *Spremište podrška za android* (Dodataka)

  Android sdk ne nudi sve zadane instance emulator. Stvorite novi tako da pokrenete `android avd` iz terminal, a zatim odaberete *Stvaranje …* ako želite pokrenuti aplikacija za Android emulator. Preporučena *Razina Api* je 19 ili novija verzija, potražite u članku [AVD Upravitelj] (http://developer.android.com/tools/help/avd-manager.html) dodatne informacije o mogućnostima emulator i stvaranje sa sustavom Android.


## <a name="1--register-an-application-with-azure-ad"></a>*1. Registracija aplikacije s Azure AD*

Napomena: ovaj __korak nije obavezan__. Vodič navedeni su unaprijed dodijeljenu vrijednosti koje će vam omogućuje da vidite uzorak u akciji bez dodjeljivanje vlastite klijentu. No preporučuje se da izvođenje ovog koraka i se upoznali s postupak, kao što je potrebno kada stvorite vlastitu aplikacije.

Azure AD samo izdati tokeni poznati aplikacijama. Prije korištenja Azure AD iz aplikacije, morate da biste stvorili unos za nju na klijentu.  Da biste registrirali novu aplikaciju na klijentu

-   Prijavite se na [Portal za upravljanje Azure](https://manage.windowsazure.com)
-   U navigacijskom oknu lijevoj strani kliknite **Active Directory**
-   Odaberite klijenta na kojem želite da biste registrirali aplikaciju.
-   Kliknite karticu **aplikacije** pa kliknite **Dodaj** u donjem ladica.
-   Slijedite upite i stvaranje nove **Nativni klijentska aplikacija** (bez obzira na fact jesu HTML koji se temelji na Cordova aplikacije, ne možemo su stvaranje nativni klijent aplikacije ovdje pa `Native Client Application` mora biti odabrana mogućnost; u suprotnom neće funkcionirati aplikacije).
    -   **Naziv** aplikacije opisane su aplikacije krajnjim korisnicima
    -   **Preusmjeravanje URI** je URI koristiti da biste se vratili tokeni aplikacije. Unesite `http://MyDirectorySearcherApp`.

Nakon što ste dovršili registraciju, AAD će dodijeliti aplikacije klijent Jedinstveni identifikator.  Potreban vam je tu vrijednost u sljedećim odjeljcima: mogu pronaći na kartici **Konfiguriraj** novostvorenu aplikacije.

Da biste pokrenuli `DirSearchClient Sample`, dodijeliti dozvolu novostvorenu aplikacije za upit _Azure AD grafikonu API-JA_:
-   Na kartici **Konfiguriraj** pronađite odjeljak "Dozvole za ostale aplikacije".  Aplikacija "Azure Active Directory" dodajte pravo **pristupa directory kao korisnik prijavljeni u** odjeljku **Dodijeliti dozvole**.  To će omogućiti aplikacije da biste upit grafikonu API-JA za korisnike.

## <a name="2-clone-the-sample-app-repository-required-for-the-tutorial"></a>*2. Kloniraj spremište aplikacije uzorka potrebne za vodič*

Iz ljuske ili naredbeni redak, upišite sljedeću naredbu:

    git clone -b skeleton https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-Cordova.git

## <a name="3-create-the-cordova-app"></a>*3. Stvaranje aplikacije Cordova*

Stvaranje aplikacije Cordova na više načina. U ovom ćete praktičnom vodiču koristit ćemo Cordova naredbeni redak sučelja (EŽA).
Iz ljuske ili naredbeni redak, upišite sljedeću naredbu:


     cordova create DirSearchClient --copy-from="NativeClient-MultiTarget-Cordova/DirSearchClient"

Koje će stvoriti mapu strukturu i scaffolding Cordova projekta, sadržaj projekta starter kopira u podmapu "www".
Premjestite u novu mapu DirSearchClient.

    cd .\DirSearchClient

Dodajte whitelist dodatak potrebne za pozivanje API grafikonu.

     cordova plugin add cordova-plugin-whitelist

Nakon toga dodali sve platforme želite za podršku. Da bi rad uzorka, morate izvršiti barem jedno od naredbe u nastavku. Imajte na umu da će nećete moći emuliraj iOS u sustavu Windows ili Windows/Windows Phone na Mac.

    cordova platform add android
    cordova platform add ios
    cordova platform add windows

Na kraju, možete dodati ADAL za dodatak Cordova u projekt.

    cordova plugin add cordova-plugin-ms-adal

## <a name="3-add-code-to-authenticate-users-and-obtain-tokens-from-aad"></a>*3. dodavanje koda za provjeru autentičnosti korisnika i dobivanje tokeni AAD*

Aplikacija razvijate ovog praktičnog vodiča dat će pretraživanja značajka Gola bone direktorija, gdje možete krajnjeg korisnika upišite pseudonim za bilo koji korisnik u direktoriju i vizualizacija neki osnovni atributi.  Projekt starter sadrži definiciju osnovni korisničkog sučelja aplikacije (u www/index.html) i scaffolding koji žice kopije osnovne aplikacija događaj kruži povezivanja korisničkog sučelja i rezultata logike prikaza (u www/js/index.js). Samo stvar izostavljeni umjesto vas je dodavanje logike implementacijom identiteta zadatke.

Vrlo prvo što biste trebali učiniti jest predstavljanje u kodu protokol vrijednosti koje koriste AAD za označavanje aplikacije i resursima usmjeriti. Te se vrijednosti će se koristiti za sastavljanje tokena zahtjeva za kasnije. Umetnite isječak ispod pri vrhu datoteke index.js.

```javascript
    var authority = "https://login.windows.net/common",
    redirectUri = "http://MyDirectorySearcherApp",
    resourceUri = "https://graph.windows.net",
    clientId = "a5d92493-ae5a-4a9f-bcbf-9f1d354067d3",
    graphApiVersion = "2013-11-08";
```

Na `redirectUri` i `clientId` vrijednosti moraju se podudarati s opisom aplikacije u AAD vrijednosti. Možete pronaći one na kartici Konfiguriraj na portalu za Azure, kao što je opisano u koraku 1 ranije u ovom praktičnom vodiču.
Napomena: odlučili za ne Registriranje novu aplikaciju u vlastiti klijenta, možete jednostavno zalijepiti unaprijed konfigurirana gornje vrijednosti kakav jest - koji će vam omogućiti da biste vidjeli pokrenut uzorka, iako se uvijek biste trebali stvoriti vlastiti unos za aplikacija namijenjene radnog.

Sljedeći je korak da biste dodali kod stvarni tokena zahtjev. Umetnite sljedeći isječak između na `search `i `renderdata `definicije.

```javascript
    // Shows user authentication dialog if required.
    authenticate: function (authCompletedCallback) {

        app.context = new Microsoft.ADAL.AuthenticationContext(authority);
        app.context.tokenCache.readItems().then(function (items) {
            if (items.length > 0) {
                authority = items[0].authority;
                app.context = new Microsoft.ADAL.AuthenticationContext(authority);
            }
            // Attempt to authorize user silently
            app.context.acquireTokenSilentAsync(resourceUri, clientId)
            .then(authCompletedCallback, function () {
                // We require user credentials so triggers authentication dialog
                app.context.acquireTokenAsync(resourceUri, clientId, redirectUri)
                .then(authCompletedCallback, function (err) {
                    app.error("Failed to authenticate: " + err);
                });
            });
        });

    },
```
Provjerimo funkcija tako da ga raščlaniti u dva glavna dijela.
Ovaj primjer je osmišljeno za klijent, kao što je opposed da biste uz neku određenu. Koristi krajnje "/ uobičajenih", koja omogućuje korisniku da unese svakim računom u vrijeme za provjeru autentičnosti i usmjerava zahtjev klijentu pripada.
Prvi dio metodu pregledava predmemoriju ADAL da biste vidjeli postoji li već spremljena token - i ako postoji, koristi klijenata dolazi iz za ponovno pokretanje ADAL. To je potrebno da biste izbjegli Dodatne upute, kao što je korištenje od "/ uobičajenih" uvijek rezultira pitanje korisniku da biste unijeli novi račun.
```javascript
        app.context = new Microsoft.ADAL.AuthenticationContext(authority);
        app.context.tokenCache.readItems().then(function (items) {
            if (items.length > 0) {
                authority = items[0].authority;
                app.context = new Microsoft.ADAL.AuthenticationContext(authority);
            }
```
Drugi dio metodu izvodi proper tokewn zahtjev.
Na `acquireTokenSilentAsync` način pita ADAL da biste se vratili tokena za određeni resurs bez prikazivanja sve UX. Koji se može dogoditi ako predmemoriju već ima odgovarajuću pristup token spremljena, ili ako postoji token osvježavanja koje je moguće koristiti da biste dobili pristupni token bez shwoing bilo koji upit.
Ako koji pokušaj ne uspije, ne možemo smanjenja `acquireTokenAsync` -koji će vas na prvi pogled korisnika za provjeru autentičnosti.
```javascript
            // Attempt to authorize user silently
            app.context.acquireTokenSilentAsync(resourceUri, clientId)
            .then(authCompletedCallback, function () {
                // We require user credentials so triggers authentication dialog
                app.context.acquireTokenAsync(resourceUri, clientId, redirectUri)
                .then(authCompletedCallback, function (err) {
                    app.error("Failed to authenticate: " + err);
                });
            });
```
Imamo tokena, možemo na kraju pozivanje API grafikonu i izvođenje želimo upit za pretraživanje. Umetnite sljedeći isječak ispod na `authenticate` definicija.

```javascript
// Makes Api call to receive user list.
    requestData: function (authResult, searchText) {
        var req = new XMLHttpRequest();
        var url = resourceUri + "/" + authResult.tenantId + "/users?api-version=" + graphApiVersion;
        url = searchText ? url + "&$filter=mailNickname eq '" + searchText + "'" : url + "&$top=10";

        req.open("GET", url, true);
        req.setRequestHeader('Authorization', 'Bearer ' + authResult.accessToken);

        req.onload = function(e) {
            if (e.target.status >= 200 && e.target.status < 300) {
                app.renderData(JSON.parse(e.target.response));
                return;
            }
            app.error('Data request failed: ' + e.target.response);
        };
        req.onerror = function(e) {
            app.error('Data request failed: ' + e.error);
        }

        req.send();
    },

```
Početne točke datoteke koje su navedene barebone UX za unos korisničke pseudonim u tekstni okvir. Ta metoda koristi tu vrijednost sastavljanje upita, kombinirati s token za pristup, poslali je na grafikonu i analizirati rezultate. RenderData način već postoje u datoteci početnu točku brine vizualizirati rezultate.

## <a name="4-run"></a>*4. Pokreni*
Na kraju spreman za pokretanje je aplikacija! Operacijski ga je vrlo jednostavne: kada se pokrene aplikaciju, unesite u tekstni okvir pseudonim korisnika kojeg želite potražiti – a zatim kliknite gumb. Zatražit će se za provjeru autentičnosti. Nakon uspješne provjere autentičnosti i uspješno pretraživanje, prikazat će se atributi pretražiti korisnika. Sljedeći se pokreće će pokrenuti pretraživanje bez prikazivanja upit, zahvaljujući prisutnosti u predmemoriji tokena nabavili.
Konkretni koraci za pokretanje aplikacije razlikuju se po platforme.

####<a name="windows-10"></a>Windows 10:

   / Tabličnom:`cordova run windows --archs=x64 -- --appx=uap`

   Mobilni telefon (zahtijeva Windows10 mobilnog uređaja povezanih s PC-JA):`cordova run windows --archs=arm -- --appx=uap --phone`

   __Napomena__: tijekom prvog pokretanja možda ćete morati prijaviti za licencu za razvojne inženjere. Dodatne informacije potražite u članku [licencu za razvojne inženjere](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx) .

####<a name="windows-81-tabletpc"></a>Windows 8.1/Tabličnom:

   `cordova run windows`

   __Napomena__: tijekom prvog pokretanja možda ćete morati prijaviti za licencu za razvojne inženjere. Dodatne informacije potražite u članku [licencu za razvojne inženjere](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx) .

####<a name="windows-phone-81"></a>Windows Phone 8.1:

   Da biste pokrenuli na povezani uređaj:`cordova run windows --device -- --phone`

   Da biste pokrenuli na zadani emulator:`cordova emulate windows -- --phone`

   Korištenje `cordova run windows --list -- --phone` da biste vidjeli sve dostupne ciljnih web-mjesta i `cordova run windows --target=<target_name> -- --phone` za pokretanje aplikacije na određeni uređaj ili emulator (na primjer, `cordova run windows --target="Emulator 8.1 720P 4.7 inch" -- --phone`).

####<a name="android"></a>Android:

   Da biste pokrenuli na povezani uređaj:`cordova run android --device`

   Da biste pokrenuli na zadani emulator:`cordova emulate android`

   __Napomena__: Provjerite je li koje ste stvorili emulator instancu pomoću *Upravitelja AVD* dok ga se prikazivao u odjeljku *preduvjeti* .

   Korištenje `cordova run android --list` da biste vidjeli sve dostupne ciljnih web-mjesta i `cordova run android --target=<target_name>` za pokretanje aplikacije na određeni uređaj ili emulator (na primjer, `cordova run android --target="Nexus4_emulator"`).

####<a name="ios"></a>iOS:

   Da biste pokrenuli na povezani uređaj:`cordova run ios --device`

   Da biste pokrenuli na zadani emulator:`cordova emulate ios`

   __Napomena__: Provjerite imate li `ios-sim` paket pokrenuti emulator. Potražite u članku *preduvjeti* odjeljak dodatne informacije.

    Use `cordova run ios --list` to see all available targets and `cordova run ios --target=<target_name>` to run application on specific device or emulator (for example,  `cordova run android --target="iPhone-6"`).

Korištenje `cordova run --help` da biste vidjeli dodatne Sastavi i pokrenite mogućnosti.

Za referencu, dovršeni uzorka (bez konfiguracijskih vrijednosti) navedeni su [u nastavku](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-Cordova/tree/complete/DirSearchClient).  Sada pomicati po za naprednije scenariji (u redu i zanimljive).  Preporučujemo vam da biste isprobali:

[Sigurne Node.js web-mjesto API-JA s Azure AD >>](active-directory-devquickstarts-webapi-nodejs.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
