<properties
    pageTitle="Azure AD B2C: Sigurne API web-mjesto pomoću Node.js | Microsoft Azure"
    description="Sastavljanje Node.js web-mjesto prihvaća tokena iz klijenta B2C API-JA"
    services="active-directory-b2c"
    documentationCenter=""
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="hero-article"
    ms.date="08/30/2016"
    ms.author="brandwe"/>

# <a name="azure-ad-b2c-secure-a-web-api-by-using-nodejs"></a>Azure AD B2C: Sigurne API web-mjesto pomoću Node.js

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

S B2C Azure Active Directory (Azure AD), možete zaštititi API web-mjesto pomoću OAuth 2.0 pristupna tokena. Tokena omogućuju klijentskih aplikacija koje koriste Azure AD B2C za provjeru autentičnosti u API-JA. U ovom se članku objašnjava da biste stvorili "popis obaveza" API-jem koje korisnicima omogućuje dodavanje i popis zadataka. Web API zaštićen je Azure AD B2C i samo omogućuje provjerene korisnike da biste upravljali svoj popis zadataka.

> [AZURE.NOTE]  Ovaj primjer napisan biti povezani s korištenjem naše [iOS B2C probna aplikacija](active-directory-b2c-devquickstarts-ios.md). Najprije učinite trenutni walk-through, a zatim slijedite uz taj uzorka.

**Passport** je proizvod provjere autentičnosti za Node.js. Prilagodljivo i modularan, Passport se unobtrusively instalirati u bilo kojem utemeljen na Express ili Restify web-aplikacije. Opsežan skup strategije podržava provjeru autentičnosti pomoću korisničkog imena i lozinke, Facebook, Twitter i više. Ne možemo ste razvili Strategije za Azure Active Directory (Azure AD). Instalacija ovom modulu, a zatim dodajte Azure AD `passport-azure-ad` dodatka.

Da biste izvršili ovaj uzorak, morate:

1. Registracija aplikacije s Azure AD.
2. Postavljanje aplikacije da biste koristili Passport na `azure-ad-passport` dodatka.
3. Konfiguriranje klijentske aplikacije da biste nazvali web "popis obaveza" API-JA.


## <a name="get-an-azure-ad-b2c-directory"></a>Početak u imeniku Azure AD B2C

Prije korištenja Azure AD B2C morate stvoriti direktorij ili smjernice.  Direktorij je spremnik za sve korisnike, aplikacije, grupe i više.  Ako ga nemate već, [stvorite direktorij B2C](active-directory-b2c-get-started.md) prije nego što nastavite.

## <a name="create-an-application"></a>Stvaranje aplikacije komponente

Ćete morati stvoriti aplikaciju u direktoriju B2C koja daje Azure AD neke informacije potrebne za sigurno komunikaciju s aplikacijom. U ovom slučaju aplikacije klijenta i API na webu su predstavljeni jedan **ID aplikacije**jer oni čine jedan logičke aplikacija. Da biste stvorili aplikaciju, slijedite [ove upute](active-directory-b2c-app-registration.md). Obavezno:

- Uključiti u **web-aplikacije/webom api** u aplikaciji
- Unesite `http://localhost/TodoListService` kao **odgovor URL-a**. Je zadani URL za ovaj uzorak koda.
- Stvaranje **aplikaciji tajna** aplikacije i njezino kopiranje. Ove podatke zatrebate kasnije. Imajte na umu da ta vrijednost mora biti [unijeti prespojni znak XML](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape) mogli koristiti.
- Kopirajte **ID aplikacije** koji vam je dodijeljen aplikacije. Ove podatke zatrebate kasnije.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Stvaranje police

U Azure AD B2C, svaki korisnički doživljaj definira [pravila](active-directory-b2c-reference-policies.md). Aplikacije sadrži dva sučelja identiteta: prijavite se i prijavite se. Morate stvoriti jednu pravila svake vrste kao što je opisano u [članku referenca pravila](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy).  Kada stvorite tri police, ne zaboravite:

- Odaberite **zaslonsko ime** i ostale registracije atribute Pravilnik za prijavu.
- Odaberite aplikaciju zahtjevima **zaslonsko ime** i **ID objekta** u svakoj pravila.  Možete odabrati druge zahtjevima.
- Nakon stvaranja, kopirajte dolje **naziv** svakog pravila. Treba imati prefiks `b2c_1_`.  Nazive pravila zatrebate kasnije.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Kada stvorite tri police, spremni ste za stvaranje aplikacije.

Da biste saznali kako funkcioniraju pravilnici u Azure AD B2C, započnite [.NET web app uvodni vodič](active-directory-b2c-devquickstarts-web-dotnet.md).

## <a name="download-the-code"></a>Preuzimanje kod

Kod za ovaj Praktični vodič [je spremljen na GitHub](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS). Da biste sastavili uzorka tijekom rada, možete [preuzeti skeleton projekt kao .zip datoteku](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/skeleton.zip). Mogu Kloniraj i na skeleton:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS.git
```

Aplikaciju dovršene nije [dostupan kao .zip datoteku](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/complete.zip) ili u `complete` podružnice u istom spremištu.

## <a name="download-nodejs-for-your-platform"></a>Preuzmite Node.js za svoju platformu

Da biste uspješno koristili ovaj uzorak, potrebno vam je ispravno instaliran Node.js. 

Instalirajte Node.js iz [nodejs.org](http://nodejs.org).

## <a name="install-mongodb-for-your-platform"></a>Instalacija MongoDB za svoju platformu

Da biste uspješno koristili ovaj uzorak, morate ispravno instaliran MongoDB. Da biste REST API-JA stalni kroz sve instance poslužitelja koristimo MongoDB.

Instalirajte MongoDB iz [mongodb.org](http://www.mongodb.org).

> [AZURE.NOTE] U ovom walk-through pretpostavlja da koristite krajnje točke instalaciju i poslužitelja zadani za MongoDB koji se u trenutku pisanja ovog je `mongodb://localhost`.

## <a name="install-the-restify-modules-in-your-web-api"></a>Instaliranje modula Restify u web-API-JA

Koristimo Restify da biste sastavili REST API-JA. Restify okvir aplikacije Node.js minimalne i prilagodljivo proizlazi iz Express. Ima robusni skup značajki za stvaranje REST API-ji pri vrhu za povezivanje.

### <a name="install-restify"></a>Instalacija Restify

Iz naredbenog retka promjena direktorija za `azuread`. Ako u `azuread` direktorija ne postoji, stvorite ga.

`cd azuread`ili`mkdir azuread;`

Unesite sljedeću naredbu:

`npm install restify`

Ta se naredba instalira Restify.

#### <a name="did-you-get-an-error"></a>Jeste li dođe do pogreške?

U nekim operacijskim sustavima prilikom korištenja `npm`, možda ćete primiti pogrešku `Error: EPERM, chmod '/usr/local/bin/..'` i zahtjeva za račun Pokreni kao administrator. Ako se problem pojavljuje, koristite na `sudo` naredbu koju želite pokrenuti `npm` više razine ovlasti.

#### <a name="did-you-get-a-dtrace-error"></a>Je li se poruka o pogrešci u DTrace?

Možda ćete vidjeti nešto kao što je ovaj tekst kada instalirate Restify:

```Shell
clang: error: no such file or directory: 'HD/azuread/node_modules/restify/node_modules/dtrace-provider/libusdt'
make: *** [Release/DTraceProviderBindings.node] Error 1
gyp ERR! build error
gyp ERR! stack Error: `make` failed with exit code: 2
gyp ERR! stack     at ChildProcess.onExit (/usr/local/lib/node_modules/npm/node_modules/node-gyp/lib/build.js:267:23)
gyp ERR! stack     at ChildProcess.EventEmitter.emit (events.js:98:17)
gyp ERR! stack     at Process.ChildProcess._handle.onexit (child_process.js:789:12)
gyp ERR! System Darwin 13.1.0
gyp ERR! command "node" "/usr/local/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js" "rebuild"
gyp ERR! cwd /Volumes/Development HD/azuread/node_modules/restify/node_modules/dtrace-provider
gyp ERR! node -v v0.10.11
gyp ERR! node-gyp -v v0.10.0
gyp ERR! not ok
npm WARN optional dep failed, continuing dtrace-provider@0.2.8
```

Restify nudi Napredna mehanizam za praćenje OSTALE poziva pomoću DTrace. Međutim, mnoge operacijski sustavi ne sadrži DTrace dostupna. Sigurno možete zanemariti te pogreške.

Izlaz iz naredbu trebao izgledati slično ovaj tekst:

    restify@2.6.1 node_modules/restify
    ├── assert-plus@0.1.4
    ├── once@1.3.0
    ├── deep-equal@0.0.0
    ├── escape-regexp-component@1.0.2
    ├── qs@0.6.5
    ├── tunnel-agent@0.3.0
    ├── keep-alive-agent@0.0.1
    ├── lru-cache@2.3.1
    ├── node-uuid@1.4.0
    ├── negotiator@0.3.0
    ├── mime@1.2.11
    ├── semver@2.2.1
    ├── spdy@1.14.12
    ├── backoff@2.3.0
    ├── formidable@1.0.14
    ├── verror@1.3.6 (extsprintf@1.0.2)
    ├── csv@0.3.6
    ├── http-signature@0.10.0 (assert-plus@0.1.2, asn1@0.1.11, ctype@0.5.2)
    └── bunyan@0.22.0 (mv@0.0.5)

## <a name="install-passport-in-your-web-api"></a>Instalirajte Passport u vašem API na webu

Iz naredbenog retka promjena direktorija za `azuread`, ako već nije ondje.

Instalirajte Passport pomoću sljedeće naredbe:

`npm install passport`

Rezultat naredbe mora biti sličan ovaj tekst:

    passport@0.1.17 node_modules\passport
    ├── pause@0.0.1
    └── pkginfo@0.2.3

## <a name="add-passport-azuread-to-your-web-api"></a>Dodavanje passport azuread vaše web-mjesto API-JA

Nakon toga dodavanje strategije OAuth pomoću `passport-azuread`, paketa strategije koji se povezuju Azure AD s Passportom. Koristite ovaj Strategije za nošenja tokeni u uzorku REST API-JA.

> [AZURE.NOTE] Iako OAuth2 daje okvir u kojem možete izdati poznate tokena vrste, samo određene vrste tokena ste stekli Primamo koristi. Tokeni za zaštitu krajnje točke su nošenja tokena. Ove vrste tokeni su najčešće široko Izdana u OAuth2. Mnoge implementacije pretpostavlja da su nošenja tokeni samo vrstu znak izdan.

Iz naredbenog retka promjena direktorija za `azuread`, ako već nije ondje.

Instalacija putovnice `passport-azure-ad` modul pomoću sljedeće naredbe:

`npm install passport-azure-ad`

Rezultat naredbe mora biti sličan ovaj tekst:

``
passport-azure-ad@1.0.0 node_modules/passport-azure-ad
├── xtend@4.0.0
├── xmldom@0.1.19
├── passport-http-bearer@1.0.1 (passport-strategy@1.0.0)
├── underscore@1.8.3
├── async@1.3.0
├── jsonwebtoken@5.0.2
├── xml-crypto@0.5.27 (xpath.js@1.0.6)
├── ursa@0.8.5 (bindings@1.2.1, nan@1.8.4)
├── jws@3.0.0 (jwa@1.0.1, base64url@1.0.4)
├── request@2.58.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, tunnel-agent@0.4.1, oauth-sign@0.8.0, isstream@0.1.2, extend@2.0.1, json-stringify-safe@5.0.1, node-uuid@1.4.3, qs@3.1.0, combined-stream@1.0.5, mime-types@2.0.14, form-data@1.0.0-rc1, http-signature@0.11.0, bl@0.9.4, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)
└── xml2js@0.4.9 (sax@0.6.1, xmlbuilder@2.6.4)
``

## <a name="add-mongodb-modules-to-your-web-api"></a>Dodavanje modula MongoDB vaše web-mjesto API-JA

Ovaj primjer koristi MongoDB kao spremište podataka. Za koje instalacija Mongoose, često korištenih dodatak za upravljanje modela i sheme.

* `npm install mongoose`

## <a name="install-additional-modules"></a>Instalacija dodatnih modula

Nakon toga instalirati ostale potrebne module.

Iz naredbenog retka promjena direktorija za `azuread`, ako već nije ondje:

`cd azuread`

Instaliranje modula u vašem `node_modules` direktorija:

* `npm install assert-plus`
* `npm install ejs`
* `npm install ejs-locals`
* `npm install express`
* `npm install bunyan`


## <a name="create-a-serverjs-file-with-your-dependencies"></a>Stvaranje datoteke server.js s vašeg ovisnosti

Na `server.js` datoteke nudi većina funkcija za poslužitelj za API na webu. 

Iz naredbenog retka promjena direktorija za `azuread`, ako već nije ondje:

`cd azuread`

Stvaranje na `server.js` datoteku u uređivaču. Dodajte sljedeće podatke:

```Javascript
'use strict';
/**
* Module dependencies.
*/
var fs = require('fs');
var path = require('path');
var util = require('util');
var assert = require('assert-plus');
var mongoose = require('mongoose/');
var bunyan = require('bunyan');
var restify = require('restify');
var config = require('./config');
var passport = require('passport');
var OIDCBearerStrategy = require('passport-azure-ad').BearerStrategy;
```

Spremite datoteku. Vratite da biste ga kasnije.

## <a name="create-a-configjs-file-to-store-your-azure-ad-settings"></a>Stvaranje config.js datoteke za spremanje postavki Azure AD

Datoteka kod prosljeđuje parametre konfiguracije iz svog Azure AD portala na `Passport.js` datoteku. Ove konfiguracije vrijednosti stvara prilikom dodavanja web API-JA na portal u prvi dio na walk-through. Ne možemo objašnjavaju što želite staviti vrijednosti parametara kada kopirate kod.

Iz naredbenog retka promjena direktorija za `azuread`, ako već nije ondje:

`cd azuread`

Stvaranje na `config.js` datoteku u uređivaču. Dodajte sljedeće podatke:

```Javascript
// Don't commit this file to your public repos. This config is for first-run
exports.creds = {
clientID: <your client ID for this Web API you created in the portal>
mongoose_auth_local: 'mongodb://localhost/tasklist', // Your mongo auth uri goes here
audience: '<your audience URI>', // the Client ID of the application that is calling your API, usually a web API or native client
identityMetadata: 'https://login.microsoftonline.com/<tenant name>/.well-known/openid-configuration', // Make sure you add the B2C tenant name in the <tenant name> area
tenantName:'<tenant name>', 
policyName:'b2c_1_<sign in policy name>' // This is the policy you'll want to validate against in B2C. Usually this is your Sign-in policy (as users sign in to this API)
passReqToCallback: false // This is a node.js construct that lets you pass the req all the way back to any upstream caller. We turn this off as there is no upstream caller.
};

```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

### <a name="required-values"></a>Tražene vrijednosti

`clientID`ID klijenta: Web API aplikacije.

`IdentityMetadata`: To je mjesto `passport-azure-ad` traži konfiguracijskih podataka za davatelja identiteta. Također se traži slučajeve tipki za provjeru valjanosti web tokeni JSON. 

`audience`: Standardizirani identifikator resursa (URI) na portalu koja služi za identifikaciju pokrenuti aplikacije. 

`tenantName`: Klijentu naziv (na primjer, **contoso.onmicrosoft.com**).

`policyName`: Pravilo koje želite provjeriti valjanost tokeni dolaze na poslužitelj. Ovo pravilo mora biti ista pravila koje koristite na klijentskoj aplikaciji za prijavu.

> [AZURE.NOTE] Za naše pretpregled B2C koristiti isti pravilnike preko postupak klijentske i poslužiteljske postavljanje. Ako ste već dovršen na walk-through i stvorili ta pravila, ne morate učiniti ponovno. Jer dovršen na walk-through, nećete morati postaviti novi pravilnike za klijentski walk-throughs na web-mjestu.

## <a name="add-configuration-to-your-serverjs-file"></a>Dodavanje konfiguraciju u datoteku server.js

Da biste pročitali vrijednosti iz na `config.js` datoteke koji ste stvorili, dodajte na `.config` datoteku kao potrebnih resursa u aplikaciji, a zatim globalne varijable onima iz na `config.js` dokumenta.

Iz naredbenog retka promjena direktorija za `azuread`, ako već nije ondje:

`cd azuread`

Otvaranje u `server.js` datoteku u uređivaču. Dodajte sljedeće podatke:

```Javascript
var config = require('./config');
```
Dodajte novu sekciju na `server.js` koja sadrži sljedeći kod:

```Javascript
// We pass these options in to the ODICBearerStrategy.

var options = {
    // The URL of the metadata document for your app. We put the keys for token validation from the URL found in the jwks_uri tag of the in the metadata.
    identityMetadata: config.creds.identityMetadata,
    clientID: config.creds.clientID,
    tenantName: config.creds.tenantName,
    policyName: config.creds.policyName,
    validateIssuer: config.creds.validateIssuer,
    audience: config.creds.audience,
    passReqToCallback: config.creds.passReqToCallback

};
```

Nakon toga dodat ćemo neka rezervirana mjesta za korisnike ćemo dobiti iz naših pozivanja aplikacija.

```Javascript
// array to hold logged in users and the current logged in user (owner)
var users = [];
var owner = null;
```

Gornja nastaviti i stvorite naše zapisivaču previše.

```Javascript
// Our logger
var log = bunyan.createLogger({
    name: 'Microsoft Azure Active Directory Sample'
});
```

## <a name="add-the-mongodb-model-and-schema-information-by-using-mongoose"></a>Dodavanje MongoDB modela i sheme podataka pomoću Mongoose

Starije Priprema daje kao Premjesti te tri datoteke zajedno u servisu REST API-JA.

Za ovaj walk-through koristite MongoDB za pohranu zadataka, kao što je spomenuli.

U na `config.js` datoteke, naziva **tasklist**za bazu podataka. Taj naziv također je staviti na kraju na `mongoose_auth_local` URL veze. Ne morate prije toga stvorite bazu podataka u MongoDB. Stvara bazu podataka za vas na prvog pokretanja programa poslužitelja.

Nakon prepoznali poslužitelj baze podataka koja MongoDB za korištenje, morate neke dodatne kod, a da biste stvorili modela i sheme za poslužitelj zadataka.

### <a name="expand-the-model"></a>Proširivanje modela

Ovaj model sheme je jednostavno. Proširite ga po potrebi.

`owner`: Koji je dodijeljen zadatak. Taj objekt je **niz**.  

`Text`Zadatak: sam. Taj objekt je **niz**.

`date`: Datum koji je krajnji rok zadatka. Taj objekt je **datuma i vremena**.

`completed`: Ako je zadatak dovršen. **Booleova**je taj objekt.

### <a name="create-the-schema-in-the-code"></a>Stvaranje sheme kod

Iz naredbenog retka promjena direktorija za `azuread`, ako već nije ondje:

`cd azuread`

Otvaranje u `server.js` datoteku u uređivaču. Dodajte sljedeće informacije u nastavku unos konfiguracije:

```Javascript
// MongoDB setup
// Setup some configuration
var serverPort = process.env.PORT || 3000; // Note we are hosting our API on port 3000
var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;

// Connect to MongoDB
global.db = mongoose.connect(serverURI);
var Schema = mongoose.Schema;
log.info('MongoDB Schema loaded');

// Here we create a schema to store our tasks and users. Pretty simple schema for now.
var TaskSchema = new Schema({
    owner: String,
    Text: String,
    completed: Boolean,
    date: Date
});

// Use the schema to register a model
mongoose.model('Task', TaskSchema);
var Task = mongoose.model('Task');
```
Stvaranje shemu, a zatim stvorite modela objekta koji koristite za pohranu podataka kod prilikom definiranja **usmjerava**.

## <a name="add-routes-for-your-rest-api-task-server"></a>Dodavanje smjerova za poslužitelj za zadatak REST API-JA

Sad kad imate model baze podataka da biste radili s, dodajte usmjerava koji koristite za poslužitelj REST API-JA.

### <a name="about-routes-in-restify"></a>O usmjerava u Restify

Usmjerava raditi u Restify na isti način na koji rade prilikom korištenja stog Express. Definiranje usmjerava pomoću URI koji očekujete klijentske aplikacije da biste uputili poziv. 

Uobičajeni uzorak Restify usmjeravanje je:

```Javascript
function createObject(req, res, next) {
// do work on Object
_object.name = req.params.object; // passed value is in req.params under object
///...
return next(); // keep the server going
}
....
server.post('/service/:add/:object', createObject); // calls createObject on routes that match this.
```

Restify i Express nudi mnogo detaljnije funkcijama, kao što je definiranje vrsta aplikacija i način složene usmjeravanje preko različitih krajnje točke. Za potrebe ovog praktičnog vodiča, ne možemo Imajte ove usmjerava jednostavne.

#### <a name="add-default-routes-to-your-server"></a>Dodavanje zadani usmjerava na poslužitelju

Sada dodajte osnovni usmjerava CRUD te **Stvaranje** **popisa** za naše REST API-JA. Ostale usmjerava pronaći ćete u na `complete` grani uzorka.

Iz naredbenog retka promjena direktorija za `azuread`, ako već nije ondje:

`cd azuread`

Otvaranje u `server.js` datoteku u uređivaču. U nastavku na unosa u bazi podataka koje ste unijeli iznad dodajte sljedeće podatke:

```Javascript
/**
 *
 * APIs for our REST Task server
 */

// Create a task

function createTask(req, res, next) {

    // Resitify currently has a bug which doesn't allow you to set default headers
    // This headers comply with CORS and allow us to mongodbServer our response to any origin

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    // Create a new task model, fill it up and save it to Mongodb
    var _task = new Task();

    if (!req.params.Text) {
        req.log.warn({
            params: req.params
        }, 'createTodo: missing task');
        next(new MissingTaskError());
        return;
    }

    _task.owner = owner;
    _task.Text = req.params.Text;
    _task.date = new Date();

    _task.save(function(err) {
        if (err) {
            req.log.warn(err, 'createTask: unable to save');
            next(err);
        } else {
            res.send(201, _task);

        }
    });

    return next();

}
```

```Javascript
/// Simple returns the list of TODOs that were loaded.

function listTasks(req, res, next) {
    // Resitify currently has a bug which doesn't allow you to set default headers
    // This headers comply with CORS and allow us to mongodbServer our response to any origin

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    log.info("listTasks was called for: ", owner);

    Task.find({
        owner: owner
    }).limit(20).sort('date').exec(function(err, data) {

        if (err)
            return next(err);

        if (data.length > 0) {
            log.info(data);
        }

        if (!data.length) {
            log.warn(err, "There is no tasks in the database. Add one!");
        }

        if (!owner) {
            log.warn(err, "You did not pass an owner when listing tasks.");
        } else {

            res.json(data);

        }
    });

    return next();
}
```


#### <a name="add-error-handling-for-the-routes"></a>Dodavanje pogreškom smjerovima

Dodajte neki Rukovanje pogreškama tako da možete komunicirati naiđete na probleme natrag za klijenta na način koji je mogu razumjeti.

Dodavanje koda za sljedeće:

```Javascript
///--- Errors for communicating something interesting back to the client
function MissingTaskError() {
restify.RestError.call(this, {
statusCode: 409,
restCode: 'MissingTask',
message: '"task" is a required parameter',
constructorOpt: MissingTaskError
});
this.name = 'MissingTaskError';
}
util.inherits(MissingTaskError, restify.RestError);
function TaskExistsError(owner) {
assert.string(owner, 'owner');
restify.RestError.call(this, {
statusCode: 409,
restCode: 'TaskExists',
message: owner + ' already exists',
constructorOpt: TaskExistsError
});
this.name = 'TaskExistsError';
}
util.inherits(TaskExistsError, restify.RestError);
function TaskNotFoundError(owner) {
assert.string(owner, 'owner');
restify.RestError.call(this, {
statusCode: 404,
restCode: 'TaskNotFound',
message: owner + ' was not found',
constructorOpt: TaskNotFoundError
});
this.name = 'TaskNotFoundError';
}
util.inherits(TaskNotFoundError, restify.RestError);
```


## <a name="create-your-server"></a>Stvaranje poslužitelja

Sada definirani bazu podataka i postavljanje puteve na mjestu. Preostaje za što želite učiniti tako da dodate instanca poslužitelja koja upravlja pozive.

Restify i Express nude precizno prilagodbe za poslužitelj REST API-JA, ali koristimo najčešće Osnovno postavljanje ovdje. 

```Javascript

**
 * Our Server
 */


var server = restify.createServer({
    name: "Microsoft Azure Active Directroy TODO Server",
    version: "2.0.1"
});

// Ensure we don't drop data on uploads
server.pre(restify.pre.pause());

// Clean up sloppy paths like //todo//////1//
server.pre(restify.pre.sanitizePath());

// Handles annoying user agents (curl)
server.pre(restify.pre.userAgentConnection());

// Set a per request bunyan logger (with requestid filled in)
server.use(restify.requestLogger());

// Allow 5 requests/second by IP, and burst to 10
server.use(restify.throttle({
    burst: 10,
    rate: 5,
    ip: true,
}));

// Use the common stuff you probably want
server.use(restify.acceptParser(server.acceptable));
server.use(restify.dateParser());
server.use(restify.queryParser());
server.use(restify.gzipResponse());
server.use(restify.bodyParser({
    mapParams: true
})); // Allows for JSON mapping to REST
server.use(restify.authorizationParser()); // Looks for authorization headers

// Let's start using Passport.js

server.use(passport.initialize()); // Starts passport
server.use(passport.session()); // Provides session support


```
## <a name="add-the-routes-to-the-server-without-authentication"></a>Dodavanje smjerovima poslužitelju (bez provjera autentičnosti)

```Javascript
server.get('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), listTasks);
server.get('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), listTasks);
server.get('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), getTask);
server.head('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), getTask);
server.post('/api/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
    session: false
}), createTask);
server.post('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), createTask);
server.del('/api/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), removeAll, function respond(req, res, next) {
    res.send(204);
    next();
});


// Register a default '/' handler

server.get('/', function root(req, res, next) {
    var routes = [
        'GET     /',
        'POST    /api/tasks/:owner/:task',
        'POST    /api/tasks (for JSON body)',
        'GET     /api/tasks',
        'PUT     /api/tasks/:owner',
        'GET     /api/tasks/:owner',
        'DELETE  /api/tasks/:owner/:task'
    ];
    res.send(200, routes);
    next();
});
```

```Javascript

server.listen(serverPort, function() {

    var consoleMessage = '\n Microsoft Azure Active Directory Tutorial';
    consoleMessage += '\n +++++++++++++++++++++++++++++++++++++++++++++++++++++';
    consoleMessage += '\n %s server is listening at %s';
    consoleMessage += '\n Open your browser to %s/api/tasks\n';
    consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n';
    consoleMessage += '\n !!! why not try a $curl -isS %s | json to get some ideas? \n';
    consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n\n';

    //log.info(consoleMessage, server.name, server.url, server.url, server.url);

});

``` 

## <a name="add-authentication-to-your-rest-api-server"></a>Dodavanje provjere autentičnosti na poslužitelju REST API-JA

Sad kad imate izvodi poslužitelj REST API-JA, možete ga učiniti korisne protiv Azure AD.

Iz naredbenog retka promjena direktorija za `azuread`, ako već nije ondje:

`cd azuread`

### <a name="use-the-oidcbearerstrategy-that-is-included-with-passport-azure-ad"></a>Korištenje OIDCBearerStrategy koji se isporučuje se uz passport azure ad


> [AZURE.TIP]
Prilikom pisanja API-ji uvijek povežite podatke nešto jedinstveni iz tokena koje korisnik ne može lažiraju. Kada poslužitelj pohranjuju stavke obveze, ono tako da na temelju **oid** korisnika u token (naziva se kroz token.oid), koja će vas odvesti u polju "vlasnik". Ta vrijednost osigurava da bi taj korisnik može pristupiti vlastite obveze stavke. Postoji bez izlaganje API vlasnika"," pa vanjskog korisnika možete zatražiti tuđa stavke obveze čak i ako su provjere autentičnosti.

Nakon toga pomoću strategije nošenja koji se isporučuju s `passport-azure-ad`.

```Javascript
var findById = function(id, fn) {
    for (var i = 0, len = users.length; i < len; i++) {
        var user = users[i];
        if (user.oid === id) {
            log.info('Found user: ', user);
            return fn(null, user);
        }
    }
    return fn(null, null);
};


var oidcStrategy = new OIDCBearerStrategy(options,
    function(token, done) {
        log.info('verifying the user');
        log.info(token, 'was the token retreived');
        findById(token.sub, function(err, user) {
            if (err) {
                return done(err);
            }
            if (!user) {
                // "Auto-registration"
                log.info('User was added automatically as they were new. Their sub is: ', token.oid);
                users.push(token);
                owner = token.oid;
                return done(null, token);
            }
            owner = token.sub;
            return done(null, user, token);
        });
    }
);

passport.use(oidcStrategy);
```

Passport koristi isti uzorak za njegov strategije. Što prođete na `function()` koja ima `token` i `done` kao parametri. Na strategije dobivate natrag nakon čega sve rad. Zatim trebali biste spremiti korisnika i spremite token tako da ne morate Pitaj za njega.

> [AZURE.IMPORTANT]
Gore navedeni kod vodi svi korisnici koji će se dogoditi za provjeru autentičnosti na poslužitelju. Ovaj postupak zove autoregistration. U proizvodne poslužitelje nemojte dopustiti u programu access sve korisnike u API bez prvog ih proći kroz postupak registracije. To je obično uzorak koje vidite u korisničke aplikacije koje omogućuju vam da biste registrirali pomoću servisa Facebook, no zatražiti da ispunite dodatne informacije. Ako program nije programa naredbenog retka, ne možemo nije su izdvojene e-pošte iz tokena objekta koji je vratio i od vas zatraži korisnicima ispunjavanje dodatne informacije. Budući da je uzorka, ne možemo ih dodati u bazu podataka u memoriji.



## <a name="run-your-server-application-to-verify-that-it-rejects-you"></a>Pokretanje aplikacije server da biste potvrdili da odbacuje vam

Možete koristiti `curl` da biste saznali je li sada OAuth2 zaštite na temelju vašeg krajnje točke. Vraća zaglavlja mora biti dovoljno reći koju ste na putu desno.

Provjerite je li na instancu MongoDB je pokrenut:

    $sudo mongodb

Promjena u direktoriju i pokretanje poslužitelja:

    $ cd azuread
    $ node server.js

U novom prozoru terminal pokretanje`curl`

Pokušajte osnovne objave:

`$ curl -isS -X POST http://127.0.0.1:3000/api/tasks/brandon/Hello`

```Shell
HTTP/1.1 401 Unauthorized
Connection: close
WWW-Authenticate: Bearer realm="Users"
Date: Tue, 14 Jul 2015 05:45:03 GMT
Transfer-Encoding: chunked
```

401 pogreške je odgovor koji želite. Označava sloj Passport pokušava preusmjeriti na krajnjoj točki ovlasti.


## <a name="you-now-have-a-rest-api-service-that-uses-oauth2"></a>Sada imate REST API-JA servisa koji koristi OAuth2

Pomoću Restify i OAuth ste implementirali REST API-JA! Sada imate li dovoljno kod tako da možete i dalje razvijati usluge i stvoriti u ovom primjeru. Razmijenjeno koliko god to možete u činiti ovaj poslužitelj bez korištenja klijent za OAuth2 kompatibilnog. Koristite dodatne walk-through kao što je naš vodič [za povezivanje s web-mjesto API pomoću iOS uz B2C](active-directory-b2c-devquickstarts-ios.md) za taj sljedeći korak.


## <a name="next-steps"></a>Daljnji koraci

Sada pomicati dodatne teme, kao što su:

[Povežite se s webom API pomoću iOS B2C](active-directory-b2c-devquickstarts-ios.md)