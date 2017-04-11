<properties
    pageTitle="Azure AD AngularJS Uvod | Microsoft Azure"
    description="Sastavljanje Kutna JS jednu stranicu aplikacije koja integrira Azure AD za prijavu i poziva Azure AD zaštićen API-ji pomoću OAuth."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>


# <a name="securing-angularjs-single-page-apps-with-azure-ad"></a>Zaštita AngularJS jednu stranicu aplikacije s Azure AD

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Azure AD olakšava jednostavne i jasan dodati znak, Odjava i sigurne OAuth API pozive na jednu stranicu aplikacija.  Omogućuje aplikacije za provjeru autentičnosti korisnika s računa za Active Directory i trošiti sve API zaštićen Azure AD, kao što su API-ji sa sustavom Office 365 ili Azure API na webu.

Za javascript aplikacije koji se izvodi u pregledniku, Azure AD omogućuje provjeru autentičnosti biblioteke imenika Active Directory ili adal.js.  Adal.js-isključivom svrhom u životu je da biste olakšali aplikacije da biste dobili pristup tokena.  Da bismo pokazali samo kako je lako, ovdje bismo ćete stvoriti aplikaciju AngularJS učinite popisa koji:

- U aplikaciji Azure AD kao davatelja identiteta potpisuje korisnika.
- Prikazat će se neke informacije o korisniku.
- Sigurno poziva aplikacije da biste učinili popis API pomoću nošenja tokena iz AAD.
- Potpisuje korisnika iz aplikacije.

Da biste sastavili dovršeno aplikacije raditi, morat ćete:

2. Registracija aplikacije s Azure AD.
3. Instalacija ADAL i konfiguriranje na SPA.
5. Pomoću ADAL sigurne stranice na SPA.

Da biste počeli, [Preuzimanje skeleton aplikacije](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/skeleton.zip) ili [Preuzimanje dovršene uzorka](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip).  Trebat ćete klijent za Azure AD Dodavanje korisnika i Registracija aplikacije.  Ako još nemate klijent, [Saznajte kako da biste ga nabavili](active-directory-howto-tenant.md).

## <a name="1-register-the-directorysearcher-application"></a>*1. registrirati DirectorySearcher aplikacije*
Da biste omogućili aplikacije za provjeru autentičnosti korisnika i tokena, najprije morate registrirati u klijentu za Azure AD:

-   Prijavite se na [Portal za upravljanje Azure](https://manage.windowsazure.com)
-   U navigacijskom oknu lijevoj strani kliknite **Servisa Active Directory**
-   Odaberite klijenta u kojoj se registrirati aplikaciju.
-   Kliknite karticu **aplikacije** pa kliknite **Dodaj** u donjem ladica.
-   Slijedite upite i stvaranje je nove **web-aplikacije i/ili WebAPI**.
    -   **Naziv** aplikacije opisane su aplikacije krajnjim korisnicima.
    -   **Preusmjeravanje Uri** je mjesto na kojem AAD će vratiti tokena.  Zadano mjesto za ovaj uzorak je`https://localhost:44326/`
-   Kad dovršite Registracija, AAD će dodijeliti aplikacije jedinstveni **ID klijenta**.  Potreban vam je tu vrijednost u sljedećim odjeljcima tako da ga kopirali na kartici **Konfiguriraj** .
- Adal.js koristi tijek implicitno OAuth možete komunicirati s Azure AD.  Implicitno tijek morate omogućiti za svoju aplikaciju po:
    - Preuzmite programski manifest tako da kliknete **Upravljanje manifesta**.
    - Otvorite manifest i pronađite u `oauth2AllowImplicitFlow` svojstvo. Postavite vrijednost na `true`.
    - Spremanje i prijenos programski manifest tako da opet kliknete **Upravljanje manifesta** .

## <a name="2-install-adal--configure-the-spa"></a>*2. Instalirajte ADAL i konfiguriranje na SPA*
Sad kad ste aplikacije u Azure AD, možete instalirati adal.js i svoj identitet vezanih kod.

-   Započnite dodavanjem adal.js projekt TodoSPA korištenju konzole za Upravitelj paketa:
  - Preuzmite [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal.js) i dodajte ga na `App/Scripts/` direktorija projekta.
  - Preuzmite [adal angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal-angular.js) i dodajte ga na `App/Scripts/` direktorija projekta.
  - Učitavanje svaki skripte prije kraja na `</body>` u `index.html`:

```js
...
<script src="App/Scripts/adal.js"></script>
<script src="App/Scripts/adal-angular.js"></script>
...
```

-   Za pozadinskog sustava SPA da biste učinili popis API da biste prihvatili tokena iz web-preglednika, pozadinski mora konfiguracijske informacije o registraciji aplikacije. U programu project TodoSPA otvorite `web.config`.  Zamjena vrijednosti elemenata u na `<appSettings>` sekciju u skladu s vizualnim vrijednosti unos u Azure Portal.  Kod pozivati te vrijednosti svaki put kada se koristi ADAL.
    -   U `ida:Tenant` je domena Azure AD klijent, npr. contoso.onmicrosoft.com
    -   Na `ida:Audience` mora biti **ID klijenta** aplikaciju koju ste kopirali iz portalu.

## <a name="3--use-adal-to-secure-pages-in-the-spa"></a>*3. ADAL za zaštitu koristiti stranice na SPA*
Adal.js sadrži su u komponenti za integraciju s AngularJS usmjeravanje i http proizvođača, koji omogućuje sigurne pojedinačne prikaza u vašem SPA.

- U `App/Scripts/app.js`, Premjesti u modulu adal.js:

```js
angular.module('todoApp', ['ngRoute','AdalAngular'])
.config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
 function ($routeProvider, $httpProvider, adalProvider) {
...
```
- Sada možete pokrenuti na `adalProvider` vrijednostima konfiguracije svoju registraciju aplikacije i u `App/Scripts/app.js`:

```js
adalProvider.init(
  {
      instance: 'https://login.microsoftonline.com/',
      tenant: 'Enter your tenant name here e.g. contoso.onmicrosoft.com',
      clientId: 'Enter your client ID here e.g. e9a5a8b6-8af7-4719-9821-0deef255f68e',
      extraQueryParameter: 'nux=1',
      //cacheLocation: 'localStorage', // enable this for IE, as sessionStorage does not work for localhost.
  },
  $httpProvider
);
```
- Sada radi zaštite u `TodoList` prikazivati u aplikaciji samo jedan redak koda nije potrebno – `requireADLogin`.

```js
...
}).when("/TodoList", {
        controller: "todoListCtrl",
        templateUrl: "/App/Views/TodoList.html",
        requireADLogin: true,
...
```

Sada imate aplikaciju sigurne jedne stranice s mogućnošću Prijava korisnika i problema nošenja tokena zaštićene zahtjeve za API pozadinskog.  Kada korisnik klikne na `TodoList` vezu adal.js će automatski preusmjeravanje Azure AD za prijavu u prema potrebi.  Osim toga, adal.js će automatski priložiti programa access_token ajax zahtjeva koji su poslani pozadinskog aplikacije.  Navedenog se Gola minimalne potrebne da biste sastavili SPA s adal.js -, no mnoga ostalih značajki koje su korisne SPAs:

- Izričito problem prijava i odjava zahtjeva možete definirati funkcije u vašem kontrolera koji pozivanje adal.js.  In `App/Scripts/homeCtrl.js`:

```js
...
$scope.login = function () {
    adalService.login();
};
$scope.logout = function () {
    adalService.logOut();
};
...
```
- Možda želite izlagati korisničke informacije u korisničkom Sučelju aplikacije.  Servis adal već dodali da biste na `userDataCtrl` kontroler, da biste mogli pristupati u `userInfo` objekt u prikazu povezane `App/Views/UserData.html`:

```js
<p>{{userInfo.userName}}</p>
<p>aud:{{userInfo.profile.aud}}</p>
<p>iss:{{userInfo.profile.iss}}</p>
...
```

- Nema više situacija u kojima želite znati ako je korisnik prijavljen ili ne.  Možete koristiti u `userInfo` objekt prikupite te podatke.  Na primjer, u `index.html` možete prikazati gumb "Prijava" ili "Odjavite" stanja provjere autentičnosti na temelju:

```js
<li><a class="btn btn-link" ng-show="userInfo.isAuthenticated" ng-click="logout()">Logout</a></li>
<li><a class="btn btn-link" ng-hide=" userInfo.isAuthenticated" ng-click="login()">Login</a></li>
```

Čestitamo! Azure AD integrirane aplikacije za jednu stranicu je dovršen.  To možete provjeru autentičnosti korisnika, sigurno poziva njegov pozadinskog pomoću OAuth 2.0 i osnovne informacije o korisniku.  Ako to već niste učinili, sada je vrijeme za popunjavanje vaš klijent s nekim korisnicima.  Pokretanje sustava da biste učinili popis SPA i prijavite se pomoću jednog od tih korisnika.  Dodavanje zadataka da biste korisnicima omogućili popisa, Odjava i ponovno se prijavite.

Adal.js olakšava sve ove značajke uobičajenih identiteta ugraditi u aplikaciji.  Ga brine sve poslovne dirty umjesto vas - upravljanje predmemorije, podrška OAuth protokola izlaganje korisnika s Prijava korisničko Sučelje, osvježavanje istekle tokeni i drugo.

Za referencu, dovršeni uzorka (bez konfiguracijskih vrijednosti) navedeni su [u nastavku](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip).  Možete odmah premjestiti dodatne scenarije.  Preporučujemo vam da biste isprobali:

[Pozivanje CORS web-mjesto API iz programa SPA >>](https://github.com/AzureAdSamples/SinglePageApp-WebAPI-AngularJS-DotNet)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
