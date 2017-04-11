<properties
    pageTitle="Azure AD 2.0 AngularJS Uvod | Microsoft Azure"
    description="Sastavljanje aplikaciju Kutna JS jedne stranice koja se prijavi korisnika završavaju na oba osobnog Microsoftova računa i računa tvrtke ili obrazovne ustanove."
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


# <a name="add-sign-in-to-an-angularjs-single-page-app---nodejs"></a>Dodavanje prijavu AngularJS jedne stranice aplikaciju – NodeJS

U ovom članku ćemo dodati prijavite se pomoću računa za Microsoft pokreće za aplikaciju AngularJS pomoću 2.0 krajnje Azure Active Directory. Krajnja točka 2.0 omogućuju izvođenje jedan integracija u aplikaciji i provjeru autentičnosti korisnika s osobne i poslovne/školske računi.

Ovaj uzorak je jednostavno aplikacija jedne stranice popis zadataka koji se pohranjuju zadaci u pozadinskog REST API-JA, pisan NodeJS i zaštićen OAuth nošenja tokena iz Azure AD.  Aplikaciju AngularJS će koristiti naše Otvori izvor JavaScript provjera autentičnosti biblioteke [adal.js](https://github.com/AzureAD/azure-activedirectory-library-for-js) cijeli postupak prijave, a Nabava tokeni za pozivanje REST API-JA.  Isti uzorak primjenjuje se na provjeru autentičnosti druge REST API-ji, kao što je [Microsoft Graph](https://graph.microsoft.com) ili API-ji resursima Azure.

> [AZURE.NOTE]
    Neke scenarije servisa Azure Active Directory i značajke podržava krajnju točku 2.0.  Da biste utvrdili koristite krajnju točku 2.0, Saznajte više o [2.0 ograničenja](active-directory-v2-limitations.md).

## <a name="download"></a>Preuzimanje

Da bismo započeli, morat ćete preuzeti i instalirati [node.js](https://nodejs.org).  Tada mogu Kloniraj ili [Preuzimanje](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-NodeJS/archive/skeleton.zip) skeleton aplikaciju:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-NodeJS.git
```

Aplikaciju skeleton obuhvaća sve predloženog kodove za jednostavne AngularJS aplikacije, ali nema svih dijelova identitet vezanih.  Ako ne želite pratiti, koje možete umjesto toga Kloniraj ili [Preuzimanje](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-NodeJS/archive/complete.zip) dovršene uzorka.

```
git clone https://github.com/AzureADSamples/SinglePageApp-AngularJS-NodeJS.git
```

## <a name="register-an-app"></a>Registracija aplikacije

Najprije stvorite aplikacije za [Registraciju Portal za aplikacije](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)ili slijedite ove [detaljne upute](active-directory-v2-app-registration.md).  Obavezno:

- Dodajte **Web** platformu za aplikacije.
- Unesite točne **Preusmjeravanje URI**. Zadana vrijednost za ovaj uzorak je `http://localhost:8080`.
- Ostavite **Dopusti implicitno tijeka** potvrdni okvir omogućen. 

Kopiraj dolje **ID aplikacije** na koji je dodijeljen aplikacije, morat ćete ga uskoro. 

## <a name="install-adaljs"></a>Instalacija adal.js
Da biste započeli, dođite do projekta koji preuzeti i instalirati adal.js.  Ako imate instaliran [bower](http://bower.io/) , možete pokrenuti samo ta naredba.  Za sve nepodudarnosti verzija ovisnost samo odaberite noviju verziju.
```
bower install adal-angular#experimental
```

Osim toga, možete ručno preuzeti [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal.min.js) i [adal angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal-angular.min.js).  Dodavanje obje datoteke da biste na `app/lib/adal-angular-experimental/dist` direktorija.

Sada otvorite projekt u uređivaču teksta omiljene i učitavanje adal.js na kraju tijelo stranice:

```html
<!--index.html-->

...

<script src="App/bower_components/dist/adal.min.js"></script>
<script src="App/bower_components/dist/adal-angular.min.js"></script>

...
```

## <a name="set-up-the-rest-api"></a>Postavljanje REST API-JA

Dok se ne možemo postavljate stvari, omogućuje početak rada REST API-JA pozadinski.  U naredbeni redak tako da pokrenete "instalacija" potrebne paketa (pripazite da ste u najviše razine imeniku projekt):

```
npm install
```

Sada otvorite `config.js` i zamijeni u `audience` vrijednosti:

```js
exports.creds = {
     
     // TODO: Replace this value with the Application ID from the registration portal
     audience: '<Your-application-id>',
     
     ...
}
```

REST API-JA tu ćete vrijednost koristiti za provjeru valjanosti tokeni primi u aplikaciji Kutna na zahtjeve za AJAX-a.  Napomena koji ovaj jednostavni REST API-JA sprema podatke u memoriji - tako da svaki put da biste prestali poslužitelj, izgubit ćete sve već stvorili zadatke.

To je cijelo vrijeme smo namjeravate provedu opisuje kako REST API-JA funkcionira.  Slobodno Probijte u kodu, ali ako želite saznati više o zaštiti web API-ji s Azure AD pogledajte [ovog članka](active-directory-v2-devquickstarts-node-api.md). 

## <a name="sign-users-in"></a>Prijava korisnika u
Vrijeme za neke kod identitet.  Možda ste već primijetili te adal.js sadrži davateljem AngularJS koji se reproducira radi boljeg s Kutna mehanizme za usmjeravanje.  Započnite dodavanjem adal modul aplikaciju:

```js
// app/scripts/app.js

angular.module('todoApp', ['ngRoute','AdalAngular'])
.config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
 function ($routeProvider, $httpProvider, adalProvider) {

...
```

Sada možete pokrenuti na `adalProvider` s identifikacijskog Broja za aplikaciju:

```js
// app/scripts/app.js

...

adalProvider.init({
        
        // Use this value for the public instance of Azure AD
        instance: 'https://login.microsoftonline.com/', 
        
        // The 'common' endpoint is used for multi-tenant applications like this one
        tenant: 'common',
        
        // Your application id from the registration portal
        clientId: '<Your-application-id>',
        
        // If you're using IE, uncommment this line - the default HTML5 sessionStorage does not work for localhost.
        //cacheLocation: 'localStorage',
         
    }, $httpProvider);
```

Sjajan, adal.js se sve informacije potrebne za održavanje aplikacije i prijaviti korisnicima.  Da biste nametnuli prijava za određeni usmjeravanje u aplikaciji, potrebno je samo jedan redak kod:

```js
// app/scripts/app.js

...

}).when("/TodoList", {
    controller: "todoListCtrl",
    templateUrl: "/static/views/TodoList.html",
    requireADLogin: true, // Ensures that the user must be logged in to access the route
})

...
```

Sada kad korisnik klikne na `TodoList` vezu adal.js će automatski preusmjeravanje Azure AD za prijavu prema potrebi.  Zahtjevi za prijavu i sign-out možete poslati i izričito po pozivanje adal.js u vašem kontrolera:

```js
// app/scripts/homeCtrl.js

angular.module('todoApp')
// Load adal.js the same way for use in controllers and views   
.controller('homeCtrl', ['$scope', 'adalAuthenticationService','$location', function ($scope, adalService, $location) {
    $scope.login = function () {
        
        // Redirect the user to sign in
        adalService.login();
        
    };
    $scope.logout = function () {
        
        // Redirect the user to log out    
        adalService.logOut();
    
    };
...
```

## <a name="display-user-info"></a>Prikaz podaci o korisniku
Sad kad je korisnik prijavljen, vjerojatno morate da biste pristupili podataka za provjeru autentičnosti prijavljeni u korisnika u svojoj aplikaciji.  Adal.js izlaže ove informacije o programu na `userInfo` objekt.  Da biste pristupili taj objekt u prikazu, najprije dodajte adal.js opseg korijenski odgovarajuće kontrolera:

```js
// app/scripts/userDataCtrl.js

angular.module('todoApp')
// Load ADAL for use in view
.controller('userDataCtrl', ['$scope', 'adalAuthenticationService', function ($scope, adalService) {}]);
```

Zatim se možete pozabaviti izravno u `userInfo` objekt u prikazu: 

```html
<!--app/views/UserData.html-->

...

    <!--Get the user's profile information from the ADAL userInfo object-->
    <tr ng-repeat="(key, value) in userInfo.profile">
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
...
```

Možete koristiti u `userInfo` objekt da biste odredili ako korisnik prijavljen ili ne.

```html
<!--index.html-->

...

    <!--Use the ADAL userInfo object to show the right login/logout button-->
    <ul class="nav navbar-nav navbar-right">
        <li><a class="btn btn-link" ng-show="userInfo.isAuthenticated" ng-click="logout()">Logout</a></li>
        <li><a class="btn btn-link" ng-hide="userInfo.isAuthenticated" ng-click="login()">Login</a></li>
    </ul>
...
```

## <a name="call-the-rest-api"></a>Pozivanje REST API-JA
Na kraju, vrijeme je za početak neke tokeni poziva REST API-JA za stvaranje, čitanje, ažuriranje i brisanje zadataka.  Dobro pogoditi što?  Ne morate učiniti *nečemu*.  Adal.js automatski će voditi brigu o početak, predmemoriranje i osvježavanje tokena.  Će voditi brigu i o prilaganju te tokeni odlazne AJAX zahtjeva koje šaljete REST API-JA.  

Točno onako kako to funkcionira? To je sve uz pomoć poseban [AngularJS interceptors](https://docs.angularjs.org/api/ng/service/$http), koji omogućuje adal.js Pretvorba odlazne i dolazne poruke HTTP-a.  Osim toga, adal.js pretpostavlja da sve zahtjeve za slanje iste domene kao što je prozor trebali biste koristiti tokeni namijenjen isti ID aplikacije AngularJS aplikacija.  To je Zašto koristi iste ID aplikacije u oba Kutna aplikaciju i NodeJS REST API-JA.  Naravno, možete promijeniti i recite adal.js da biste dobili tokeni za druge REST API-ji ako je potrebno – ali za ovaj jednostavni scenarij neće imati zadane vrijednosti.

Evo isječak koji pokazuje kako je lako slanja zahtjeva za nošenja tokena iz Azure AD:

```js
// app/scripts/todoListSvc.js

...
return $http.get('/api/tasks');
...
```

Čestitamo!  Integrirano jednu stranicu aplikacije Azure AD je dovršena.  Nastaviti, trebati animacije.  To možete provjeru autentičnosti korisnika, sigurno poziva pozadinskog REST API-JA pomoću OpenID povezivanja i osnovne informacije o korisniku.  U okvir podržava svaki korisnik s osobnim Microsoftovim Account ili tvrtke/obrazovne ustanove računa iz Azure AD.  Isprobajte aplikaciju tako da pokrenete:

```
node server.js
```

U pregledniku dođite do `http://localhost:8080`.  Prijavite se pomoću osobnog Microsoftova računa ili računa za poslovne/školske.  Dodavanje zadataka na korisnikovu popisu zadataka i odjava.  Pokušajte se prijaviti pomoću drugu vrstu računa. Ako vam je potrebna klijent Azure AD za poslovne/školske Dodavanje korisnika, [upute da biste ga nabavili ovdje](active-directory-howto-tenant.md) (to je besplatno).

Da biste nastavili učenje o na krajnjoj točki 2.0 povratak na našem [vodiču za razvojne inženjere 2.0](active-directory-appmodel-v2-overview.md).  Za dodatne resurse, pogledajte:

- [Azure uzorke na GitHub >>](https://github.com/Azure-Samples)
- [Azure AD stogu višak >>](http://stackoverflow.com/questions/tagged/azure-active-directory)
- Azure AD dokumentaciju na [Azure.com >>](https://azure.microsoft.com/documentation/services/active-directory/)

## <a name="get-security-updates-for-our-products"></a>Sigurnosna ažuriranja za naši proizvodi

Pozivamo vas da biste dobili obavijesti kada doći do sigurnošću potražite na [toj stranici](https://technet.microsoft.com/security/dd252948) i pretplata na upozorenja savjeta za sigurnost.
