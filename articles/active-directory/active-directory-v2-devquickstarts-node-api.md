<properties
    pageTitle="Azure AD 2.0 NodeJS web-API | Microsoft Azure"
    description="Sastavljanje API-JA Web NodeJS prihvaća tokena iz obje osobnog Microsoftova Account i računi tvrtke ili obrazovne ustanove."
    services="active-directory"
    documentationCenter="nodejs"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>

# <a name="secure-a-web-api-using-nodejs"></a>Sigurne Web API pomoću node.js

> [AZURE.NOTE]
    Neke scenarije servisa Azure Active Directory i značajke podržava krajnju točku 2.0.  Da biste utvrdili koristite krajnju točku 2.0, Saznajte više o [2.0 ograničenja](active-directory-v2-limitations.md).

S Azure Active Directory krajnju točku 2.0, možete zaštititi i Web API pomoću [OAuth 2.0](active-directory-v2-protocols.md#oauth2-authorization-code-flow) pristupna tokena Omogućavanje korisnicima i osobni Microsoftov račun i rad ili školske račune na siguran pristup webu API-jem.

**Passport** je proizvod provjere autentičnosti za Node.js. Iznimno fleksibilne i modularan, Passport može biti unobtrusively ispuštanje da biste sve Express sustavom ili Resitify web-aplikacije. Opsežan skup strategije podržava provjeru autentičnosti pomoću korisničko ime i lozinku, Facebook, Twitter i drugo. Ne možemo ste razvili Strategije za Microsoft Azure Active Directory. Ne možemo će instalirati ovom modulu, a zatim dodajte Microsoft Azure Active Directory `passport-azure-ad` dodatka.

## <a name="download"></a>Preuzimanje
Kod za ovog praktičnog vodiča je spremljen [na GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs).  Da biste pratili, možete [preuzeti aplikacije skeleton kao u .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs/archive/skeleton.zip) ili Kloniraj na skeleton:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs.git```

Dovršeni aplikacije navedeni su na kraju ovog vodiča.


## <a name="1-register-an-app"></a>1. registrirati aplikacije
Stvaranje nove aplikacije na [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)ili slijedite ove [detaljne upute](active-directory-v2-app-registration.md).  Obavezno:

- Kopiraj dolje **Id aplikacije** dodijeljene aplikacije, morat ćete ga uskoro.
- Dodajte platformu **mobilne** aplikacije.
- Kopirajte dolje **URI preusmjeravanje** na portalu. Morate koristiti zadanu vrijednost od `urn:ietf:wg:oauth:2.0:oob`.


## <a name="2-download-nodejs-for-your-platform"></a>2: preuzimanje node.js za svoju platformu
Da biste uspješno koristili ovaj uzorak, morate imati ispravno instaliran Node.js.

Instalirajte Node.js iz [http://nodejs.org](http://nodejs.org).

## <a name="3-install-mongodb-on-to-your-platform"></a>3: instalacija MongoDB na svoju platformu

Da biste uspješno koristili ovaj uzorak, morate imati ispravno instaliran MongoDB. Koristit ćemo MongoDB da biste naš REST API-JA persistant kroz sve instance poslužitelja.

Instalirajte MongoDB iz [http://mongodb.org](http://www.mongodb.org).

> [AZURE.NOTE] Ovaj vodič pretpostavlja da koristite krajnje točke instalaciju i poslužitelja zadani za MongoDB koji se u trenutku pisanja ovog je: mongodb://localhost

## <a name="4-install-the-restify-modules-in-to-your-web-api"></a>4: instalirali module Restify u API na webu

Ne možemo koristit će Resitfy da biste sastavili naš REST API-JA. Restify okvir aplikacije Node.js minimalne i prilagodljivo proizlazi iz Express koja ima robusni skup značajki za stvaranje REST API-ji pri vrhu za povezivanje.

### <a name="install-restify"></a>Instalacija Restify

Iz naredbenog retka, promijenite direktorija u direktoriju azuread. Ako direktorij **azuread** ne postoji, stvorite je.

`cd azuread`– ili –`mkdir azuread;`

Upišite sljedeću naredbu:

`npm install restify`

Ta se naredba instalira Restify.

#### <a name="did-you-get-an-error"></a>Jeste li dođe do pogreške?

Kada pomoću npm u nekim operacijskim sustavima, možda ćete primiti pogrešku pogreške: EPERM, chmod "/ korsnik pitanje/lokalne/smeće /..." a zahtjev za pokušajte pokrenuti račun kao administrator. Ako se to dogodi, koristite naredbu sudo da biste pokrenuli npm više razine ovlasti.

#### <a name="did-you-get-an-error-regarding-dtrace"></a>Je li se pogreške vezane uz DTrace?

Možda ćete vidjeti nešto poput ovoga prilikom instalacije Restify:

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


Restify nudi Napredna mehanizam praćenje poziva OSTALE pomoću DTrace. Međutim, mnoge operacijski sustavi ne sadrži DTrace dostupna. Sigurno možete zanemariti te pogreške.


Izlaz iz ta naredba trebao izgledati otprilike ovako:


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
    └── bunyan@0.22.0(mv@0.0.5)


## <a name="5-install-passportjs-into-your-web-api"></a>5: instalacija Passport.js u vašem API na webu

[Passport](http://passportjs.org/) je proizvod provjere autentičnosti za Node.js. Iznimno fleksibilne i modularan, Passport može biti unobtrusively ispuštanje da biste sve Express sustavom ili Resitify web-aplikacije. Opsežan skup strategije podržava provjeru autentičnosti pomoću korisničko ime i lozinku, Facebook, Twitter i drugo. Ne možemo ste razvili Strategije za Azure Active Directory. Ne možemo će instalirati ovaj modul i dodajte dodatka strategije Azure Active Directory.

Iz naredbenog retka, promijenite direktorija u direktoriju azuread.

Unesite sljedeću naredbu da biste instalirali passport.js

`npm install passport`

Izlaz iz naredbu trebao izgledati otprilike ovako:

    passport@0.1.17 node_modules\passport
    ├── pause@0.0.1
    └── pkginfo@0.2.3

## <a name="6-add-passport-azure-ad-to-your-web-api"></a>6: dodavanje Passport Azure AD vaše web-mjesto API-JA

Nakon toga dodat ćemo strategije OAuth pomoću passport-azuread, paketa strategije koji se povezuju Azure Active Directory s Passportom. Koristit ćemo ovaj Strategije za nošenja tokeni u ovom primjeru Rest API-JA.

> [AZURE.NOTE] Iako OAuth2 daje okvir u kojem možete izdati poznate tokena vrste, samo određene vrste tokena ste stekli sveobuhvatne koristi. Za zaštitu krajnje točke koji se tokeni nošenja uključio odgovor. Tokeni nošenja su najčešće široko izdavanja vrstu token u OAuth2 i mnogo implementacije pretpostavlja da su nošenja tokeni samo vrstu znak izdan.

Iz naredbenog retka, promijenite direktorija u direktoriju azuread

Upišite sljedeću naredbu da biste instalirali Passport.js passport azure ad modula:

`npm install passport-azure-ad`

Izlaz iz naredbu trebao izgledati otprilike ovako:

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

## <a name="7-add-mongodb-modules-to-your-web-api"></a>7: dodavanje MongoDB moduli Web API-jem

Ćemo koristit će MongoDB kao naš datastore zbog toga, moramo instalirati obje široko koristi dodatak za upravljanje modela i sheme pod nazivom Mongoose, kao i upravljački program baze podataka za MongoDB, naziva se i MongoDB.


* `npm install mongoose`
* `npm install mongodb`

## <a name="8-install-additional-modules"></a>8: instalacija dodatnih modula

Nakon toga ne možemo instalirati ostale potrebne module.


Iz naredbenog retka, promijenite direktorija u mapu **azuread** ako nije već postoji:

`cd azuread`


Unesite sljedeće naredbe da biste instalirali sljedeće module u direktoriju node_modules:

* `npm install crypto`
* `npm install assert-plus`
* `npm install posix-getopt`
* `npm install util`
* `npm install path`
* `npm install connect`
* `npm install xml-crypto`
* `npm install xml2js`
* `npm install xmldom`
* `npm install async`
* `npm install request`
* `npm install underscore`
* `npm install grunt-contrib-jshint@0.1.1`
* `npm install grunt-contrib-nodeunit@0.1.2`
* `npm install grunt-contrib-watch@0.2.0`
* `npm install grunt@0.4.1`
* `npm install xtend@2.0.3`
* `npm install bunyan`
* `npm update`


## <a name="9-create-a-serverjs-with-your-dependencies"></a>9: Stvaranje na server.js s vašeg ovisnosti

Datoteka server.js će biti ponuda Većina naš funkcionalnosti za naše poslužitelj Web API. Ne možemo će dodavanje većinu naš kod u tu datoteku. Za namjene bi refactor i funkcija u manje datoteke, kao što su zasebnom usmjerava i kontrolera. Radi Ovaj pokazni koristit ćemo server.js za tu funkciju.

Iz naredbenog retka, promijenite direktorija u mapu **azuread** ako nije već postoji:

`cd azuread`

Stvaranje na `server.js` datoteka u našem omiljene uređivač, a zatim dodajte sljedeće podatke:

```Javascript
'use strict';
/**
* Module dependencies.
*/
var util = require('util');
var assert = require('assert-plus');
var mongoose = require('mongoose/');
var bunyan = require('bunyan');
var restify = require('restify');
var config = require('./config');
var passport = require('passport');
var OIDCBearerStrategy = require('passport-azure-ad').OIDCStrategy;
```

Spremite datoteku. Ne možemo vratit ćete se uskoro.

## <a name="10-create-a-config-file-to-store-your-azure-ad-settings"></a>10: Stvaranje konfiguracijska datoteka za spremanje postavki Azure AD

Kod datoteka konfiguracije parametre prosljeđuje s vašeg Azure Active Directory portala Passport.js. Ove konfiguracije vrijednosti stvara prilikom dodavanja Web API portal u prvi dio u prikazu. Ne možemo će se objašnjava što želite staviti vrijednosti parametara kada kopirate kod.


Iz naredbenog retka, promijenite direktorija u mapu **azuread** ako nije već postoji:

`cd azuread`

Stvaranje na `config.js` datoteka u našem omiljene uređivač, a zatim dodajte sljedeće podatke:

```Javascript
// Don't commit this file to your public repos. This config is for first-run
exports.creds = {
mongoose_auth_local: 'mongodb://localhost/tasklist', // Your mongo auth uri goes here
issuer: 'https://sts.windows.net/**<your application id>**/',
audience: '<your redirect URI>',
identityMetadata: 'https://login.microsoftonline.com/common/.well-known/openid-configuration' // For using Microsoft you should never need to change this.
};

```



### <a name="required-values"></a>Tražene vrijednosti

*IdentityMetadata*: to je mjesto na kojem će potražiti passport azure ad za konfiguraciju podatke u IdP kao i tipki za provjeru valjanosti tokeni JWT. Vjerojatno ne želite promijeniti ako koristite Azure Active Directory.

*ciljne skupine*: vaš preusmjeravanje URI na portalu.

> [AZURE.NOTE]
Ne možemo poništiti naš tipke često korištenih u određenim intervalima. Provjerite da uvijek izvlačenja iz URL-a "openid_keys" i aplikaciju možete pristupiti s Internetom.


## <a name="11-add-configuration-to-your-serverjs-file"></a>11: dodavanje konfiguraciju u datoteku server.js

Potrebna za čitanje ove vrijednosti iz konfiguracijskoj datoteci koju ste upravo stvorili preko naš aplikacije. Da biste to učinili, ne možemo jednostavno dodajte datoteku .config kao potrebnih resursa u našem računala i postavite globalne varijable onima u dokumentu config.js

Iz naredbenog retka, promijenite direktorija u mapu **azuread** ako nije već postoji:

`cd azuread`

Otvaranje vaše `server.js` datoteka u našem omiljene uređivač, a zatim dodajte sljedeće podatke:

```Javascript
var config = require('./config');
```
Zatim dodajte novu sekciju na `server.js` s sljedeći kod:

```Javascript
// We pass these options in to the ODICBearerStrategy.
var options = {
// The URL of the metadata document for your app. We will put the keys for token validation from the URL found in the jwks_uri tag of the in the metadata.
identityMetadata: config.creds.identityMetadata,
issuer: config.creds.issuer,
audience: config.creds.audience
};
// array to hold logged in users and the current logged in user (owner)
var users = [];
var owner = null;
// Our logger
var log = bunyan.createLogger({
name: 'Microsoft Azure Active Directory Sample'
});
```

## <a name="step-12-add-the-mongodb-model-and-schema-information-using-moongoose"></a>Korak 12: Dodavanje u MongoDB Model i informacije o shemi pomoću Moongoose

Sada ovo Priprema će pokrenuti plaćanja ćemo dobiti te tri datoteke zajedno u usluzi REST API-JA.

Za ovaj vodič smo će koristiti MongoDB za pohranu naš zadatke kao što je navedeno u ***koraku 4***.

Ako opozvati iz datoteke config.js koju smo stvorili u koraku 11, ne možemo naziva naš baze podataka *tasklist*, kao što je koji je što smo smjestiti na kraju URL veze mogoose_auth_local. Ne morate prije toga stvorite bazu podataka u MongoDB, stvorit će to bismo na prvog pokretanja naš poslužitelj aplikacije (Ako je već postoji).

Nakon što smo ste obavijestio poslužitelja koje želimo da biste koristili bazu podataka MongoDB, moramo neke dodatne kod, a da biste stvorili modela i sheme za naše poslužitelja zadatke.

#### <a name="discussion-of-the-model"></a>Rasprave modela

Naš model sheme je vrlo jednostavne, a proširite stavku prema potrebi.

Naziv - naziv koji je dodijeljen zadatak. ***Niz***

ZADATAK - zadatak sam. ***Niz***

Datum - datuma koji predstavlja otplatu zadatak. ***Datum i vrijeme***

DOVRŠENO – ako je taj zadatak dovršen ili ne. ***BOOLEOVA***

#### <a name="creating-the-schema-in-the-code"></a>Stvaranje sheme kod


Iz naredbenog retka, promijenite direktorija u mapu **azuread** ako nije već postoji:

`cd azuread`

Otvaranje vaše `server.js` datoteka u našem omiljene uređivač i dodajte sljedeće informacije u nastavku unos konfiguracije:

```Javascript
// MongoDB setup
// Setup some configuration
var serverPort = process.env.PORT || 8080;
var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;
// Connect to MongoDB
global.db = mongoose.connect(serverURI);
var Schema = mongoose.Schema;
log.info('MongoDB Schema loaded');
```
To će se povezati s poslužiteljem MongoDB i rukom vratiti objekt sheme nam.

#### <a name="using-the-schema-create-our-model-in-the-code"></a>Koristite shemu, stvaranje naš modela u kodu

Ispod kod koje ste napisali iznad, dodajte sljedeći kod:

```Javascript
// Here we create a schema to store our tasks and users. Pretty simple schema for now.
var TaskSchema = new Schema({
owner: String,
task: String,
completed: Boolean,
date: Date
});
// Use the schema to register a model
mongoose.model('Task', TaskSchema);
var Task = mongoose.model('Task');
```
Kao što je to možete prepoznati iz kod, ne možemo stvoriti naš sheme a zatim modela objekta koristit ćemo sadržavati naš podataka putem kod kad smo definiranje naš ***usmjerava***.

## <a name="step-13-add-our-routes-for-our-task-rest-api-server"></a>13 korak: Dodavanje naš usmjerava za naše poslužitelj zadatka REST API-JA

Imamo model baze podataka da biste radili s, dodat ćemo usmjerava koristimo za naše poslužitelj REST API-JA.

### <a name="about-routes-in-restify"></a>O usmjerava u Restify

Usmjerava raditi u Restify potpuno isti način obavljaju pomoću stog Express. Definiranje usmjerava pomoću URI koji očekujete klijentske aplikacije da biste uputili poziv. Obično se definiraju puteve u zasebnu datoteku. Naš svrhe smo će staviti naš usmjerava u datoteci server.js. Preporučujemo da ih faktor vlastite datoteku za upotrebu radnih.

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


Ovo je uzorak na razini osnovni. Resitfy (i Express) navedite koliko dublju functionaltiy kao što su definiranje vrsta aplikacija i način složene usmjeravanje preko različitih krajnje točke. Za naše svrhe, ne možemo će zadržati te usmjerava vrlo je jednostavno.

#### <a name="add-default-routes-to-our-server"></a>Dodajte zadani usmjerava na našem poslužitelj

Ne možemo će sada dodavanje osnovni usmjerava CRUD ažuriranja za dohvaćanje, stvaranje i brisanje.

Iz naredbenog retka, promijenite direktorija u mapu **azuread** ako nije već postoji:

`cd azuread`

Otvaranje vaše `server.js` datoteka u našem omiljene uređivač i dodajte sljedeće informacije u nastavku na unosa u bazi podataka koje ste unijeli iznad:

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
if (!req.params.task) {
req.log.warn({
params: p
}, 'createTodo: missing task');
next(new MissingTaskError());
return;
}
_task.owner = owner;
_task.task = req.params.task;
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
// Delete a task by name
function removeTask(req, res, next) {
Task.remove({
task: req.params.task,
owner: owner
}, function(err) {
if (err) {
req.log.warn(err,
'removeTask: unable to delete %s',
req.params.task);
next(err);
} else {
log.info('Deleted task:', req.params.task);
res.send(204);
next();
}
});
}
// Delete all tasks
function removeAll(req, res, next) {
Task.remove();
res.send(204);
return next();
}
// Get a specific task based on name
function getTask(req, res, next) {
log.info('getTask was called for: ', owner);
Task.find({
owner: owner
}, function(err, data) {
if (err) {
req.log.warn(err, 'get: unable to read %s', owner);
next(err);
return;
}
res.json(data);
});
return next();
}
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

### <a name="add-some-error-handling-for-the-routes"></a>Dodavanje neke pogreškom za smjerovima

Je li bolje da biste dodali neki Rukovanje pogreškama kako bi smo mogli komunicirati natrag na klijentu problem ne možemo naišao na način je mogu razumjeti.

Dodajte sljedeći kôd ispod kod koji ste napisali iznad:

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


## <a name="step-14-create-your-server"></a>14 korak: Stvaranje poslužitelja!

Imamo naš određena baza podataka, imamo naš usmjerava na mjestu, a na preostaje je dodavanje naš instanca poslužitelja koji će upravljati naš pozive.

Restify (i izražavanje) imate mnogo precizno prilagodbe možete učiniti za poslužitelj REST API-JA, no ponovno koristit ćemo najčešće Osnovno postavljanje naš svrhe.

```Javascript
/**
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
}));
```
## <a name="15-adding-the-routes-without-authentication-for-now"></a>15: dodavanje smjerova (bez provjere autentičnosti za sada)

```Javascript
/// Now the real handlers. Here we just CRUD
/**
/*
/* Each of these handlers are protected by our OIDCBearerStrategy by invoking 'oidc-bearer'
/* in the pasport.authenticate() method. We set 'session: false' as REST is stateless and
/* we don't need to maintain session state. You can experiement removing API protection
/* by removing the passport.authenticate() method like so:
/*
/* server.get('/tasks', listTasks);
/*
**/
server.get('/tasks', listTasks);
server.get('/tasks', listTasks);
server.get('/tasks/:owner', getTask);
server.head('/tasks/:owner', getTask);
server.post('/tasks/:owner/:task', createTask);
server.post('/tasks', createTask);
server.del('/tasks/:owner/:task', removeTask);
server.del('/tasks/:owner', removeTask);
server.del('/tasks', removeTask);
server.del('/tasks', removeAll, function respond(req, res, next) {
res.send(204);
next();
});
// Register a default '/' handler
server.get('/', function root(req, res, next) {
var routes = [
'GET /',
'POST /tasks/:owner/:task',
'POST /tasks (for JSON body)',
'GET /tasks',
'PUT /tasks/:owner',
'GET /tasks/:owner',
'DELETE /tasks/:owner/:task'
];
res.send(200, routes);
next();
});
server.listen(serverPort, function() {
var consoleMessage = '\n Microsoft Azure Active Directory Tutorial';
consoleMessage += '\n +++++++++++++++++++++++++++++++++++++++++++++++++++++';
consoleMessage += '\n %s server is listening at %s';
consoleMessage += '\n Open your browser to %s/tasks\n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n';
consoleMessage += '\n !!! why not try a $curl -isS %s | json to get some ideas? \n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n\n';
});
```
## <a name="16-before-we-add-oauth-support-lets-run-the-server"></a>16: prije dodat ćemo OAuth podršku, recimo pokrenuti na poslužitelj.

Testirajte vaš poslužitelj prije dodat ćemo provjere autentičnosti

Da biste to učinili najjednostavnije pomoću zakretanja u naredbeni redak. Prije toga moramo, moramo jednostavne utility koji omogućuje nam raščlaniti izlaz kao JSON. Da biste to učinili, instalirajte alat json, kao što je sve primjere u nastavku koristite.

`$npm install -g jsontool`

Ovaj alat JSON instalira globalno. Nakon što smo ste postiže koji – reproducirajmo s poslužiteljem:

Najprije provjerite je li vaša isntance monogoDB sustavom...

`$sudo mongod`

Nakon toga promijenite u direktoriju i počnite curling...

`$ cd azuread`
`$ node server.js`

`$ curl -isS http://127.0.0.1:8080 | json`

```Shell
HTTP/1.1 2.0OK
Connection: close
Content-Type: application/json
Content-Length: 171
Date: Tue, 14 Jul 2015 05:43:38 GMT
[
"GET /",
"POST /tasks/:owner/:task",
"POST /tasks (for JSON body)",
"GET /tasks",
"PUT /tasks/:owner",
"GET /tasks/:owner",
"DELETE /tasks/:owner/:task"
]
```

Zatim ćemo možete dodati zadatak na taj način:

`$ curl -isS -X POST http://127.0.0.1:8888/tasks/brandon/Hello`

Odgovor mora biti:

```Shell
HTTP/1.1 201 Created
Connection: close
Access-Control-Allow-Origin: *
Access-Control-Allow-Headers: X-Requested-With
Content-Type: application/x-www-form-urlencoded
Content-Length: 5
Date: Tue, 04 Feb 2014 01:02:26 GMT
Hello
```
I smo možete popis zadataka za Brandon ovako:

`$ curl -isS http://127.0.0.1:8080/tasks/brandon/`

Ako sve to radi, ne možemo spremni za dodavanje OAuth poslužitelju REST API-JA.

**Imate poslužitelj REST API-JA s MongoDB!**

## <a name="17-add-authentication-to-our-rest-api-server"></a>17: dodavanje provjere autentičnosti naš poslužitelj REST API-JA

Imamo izvodi REST API-JA (congrats, btw!) ćemo dobiti izradi korisne protiv Azure AD.

Iz naredbenog retka, promijenite direktorija u mapu **azuread** ako nije već postoji:

`cd azuread`

### <a name="1-use-the-oidcbearerstrategy-that-is-included-with-passport-azure-ad"></a>1: korištenje oidcbearerstrategy koji se isporučuje se uz passport azure ad

Dosad smo su ugrađeni uobičajeni poslužitelja OSTALE obveze bez bilo kakvu vrstu autorizacije. Ovo je koju ne možemo pokrenuti koji izgradnja.

Najprije ćemo treba da biste naznačili želimo da biste koristili Passport. Da biste postavili to pravo iza vašoj konfiguraciji poslužitelja:

```Javascript
// Let's start using Passport.js

server.use(passport.initialize()); // Starts passport
server.use(passport.session()); // Provides session support
```

> [AZURE.TIP]
Prilikom pisanja API-ji uvijek treba veza s podacima nešto jedinstveni iz token koji korisnik ne može lažiraju. Kada ovaj poslužitelj pohranjuju stavke obveze, sprema ih ovisno o pretplati ID korisnika u token (naziva se kroz token.sub) koje ćemo stavite u polje "vlasnik". Na taj način da samo korisnik pristup svom popisi zadataka i nitko drugi mogu pristupiti popisi zadataka unijeli. Postoji bez izlaganje API "vlasnik" da bi vanjski korisnik može zatražiti popisi zadataka druge osobe čak i ako su provjere autentičnosti.

Zatim ćemo pomoću strategije nošenja povezivanje Otvori ID-a koji se isporučuju s passport azure ad. Samo pogledajte kod Zasad, on će uskoro objašnjava. Stavite to nakon što pated iznad:

```Javascript
/**
/*
/* Calling the OIDCBearerStrategy and managing users
/*
/* Passport pattern provides the need to manage users and info tokens
/* with a FindorCreate() method that must be provided by the implementor.
/* Here we just autoregister any user and implement a FindById().
/* You'll want to do something smarter.
**/
var findById = function(id, fn) {
for (var i = 0, len = users.length; i < len; i++) {
var user = users[i];
if (user.sub === id) {
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
log.info('User was added automatically as they were new. Their sub is: ', token.sub);
users.push(token);
owner = token.sub;
return done(null, token);
}
owner = token.sub;
return done(null, user, token);
});
}
);
passport.use(oidcStrategy);
```

Passport koristi slične uzorak za sve je strategije (Twitter, Facebook, itd.) koja u skladu svim autorima strategije. Pogled na na strategije potražite ćemo proslijediti ga function() token i gotovo kao parametre. Na strategije će dutifully vratite se na nam nakon što se sve dodatka rad. Kada se to čini želimo ćete za pohranu korisnika i stash token pa ćemo nećete morati Pitaj za njega.

> [AZURE.IMPORTANT]
Gore navedeni kod vodi bilo koji korisnik koji će se dogoditi za provjeru autentičnosti naš poslužitelj. To se naziva Registracija automatski. U proizvodne poslužitelje ne želite da biste svima omogućili bez prvog ih proći kroz postupak registracije odlučite. To je obično uzorak vam se prikazuje u aplikacijama potrošača koji omogućuju vam da biste registrirali sa servisom Facebook, no zatražiti da ispunite dodatne informacije. Ako to nije programa naredbenog retka, ne možemo nije su samo izdvojene e-pošte iz tokena objekta koji je vratio i zatražiti da ispunite dodatne podatke. Budući da je to testnom poslužitelju ne možemo jednostavno dodajte ih u bazu podataka u memoriji.

### <a name="2-finally-protect-some-endpoints"></a>2. na kraju, zaštita neke krajnje točke

Zaštita krajnje točke navođenjem passport.authenticate() poziv s protokolom koji želite koristiti.

Uredimo naš usmjeravanje u našem poslužitelja kod da biste učinili nešto zanimljive:

```Javascript
server.get('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), listTasks);
server.get('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), listTasks);
server.get('/tasks/:owner', passport.authenticate('oidc-bearer', {
session: false
}), getTask);
server.head('/tasks/:owner', passport.authenticate('oidc-bearer', {
session: false
}), getTask);
server.post('/tasks/:owner/:task', passport.authenticate('oidc-bearer', {
session: false
}), createTask);
server.post('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), createTask);
server.del('/tasks/:owner/:task', passport.authenticate('oidc-bearer', {
session: false
}), removeTask);
server.del('/tasks/:owner', passport.authenticate('oidc-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), removeAll, function respond(req, res, next) {
res.send(204);
next();
});
```

## <a name="18-run-your-server-application-again-and-ensure-it-rejects-you"></a>18: pokretanje aplikacije poslužitelja ponovno i provjerite je li odbacuje koje

Poslužimo se značajkom `curl` da biste vidjeli je li ćemo sada OAuth2 zaštitu protiv naš krajnje točke. Ne možemo će to prije runnning bilo koji od naše klijent SDK-ovi protiv ovaj krajnjoj točki. Vraća zaglavlja mora biti dovoljno da biste nam smo dolje desno put.

Najprije provjerite je li vaša isntance monogoDB sustavom...

    $sudo mongod

Nakon toga promijenite u direktoriju i počnite curling...

    $ cd azuread
    $ node server.js

Pokušajte osnovne objave:

`$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

```Shell
HTTP/1.1 401 Unauthorized
Connection: close
WWW-Authenticate: Bearer realm="Users"
Date: Tue, 14 Jul 2015 05:45:03 GMT
Transfer-Encoding: chunked
```

Na 401 je odgovor koji tražite ovdje, kao što je koji upućuje na to sloj Passport pokušava preusmjeriti na krajnjoj točki ovlasti koji je upravo ono što želite.


## <a name="congratulations-you-have-a-rest-api-service-using-oauth2"></a>Čestitamo! Imate servisa REST API-JA pomoću OAuth2!

Ste nije koliko god to možete u činiti ovaj poslužitelj bez korištenja kompatibilne klijent za OAuth2. Morate proći dodatne vodič.

Ako samo tražite informacije o tome kako implementirati REST API pomoću Restify i OAuth2, imate više od dovoljno kod da biste zadržali razvoj funkcioniranju servisa i učenjem kako se nadograđuju u ovom primjeru.

## <a name="next-steps"></a>Daljnji koraci

Za referencu, dovršeni uzorka (bez konfiguracijskih vrijednosti) [prikazuje se kao .zip ovdje](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs/archive/complete.zip)ili pak mogu Kloniraj ga iz GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs.git```

Sada se može pomaknuti na dodatne teme.  Preporučujemo vam da biste isprobali:

[Sigurne web-aplikacijama Node.js pomoću krajnju točku 2.0 >>](active-directory-v2-devquickstarts-node-web.md)

Za dodatne resurse, pogledajte:
- [Vodič za razvojne inženjere 2.0 >>](active-directory-appmodel-v2-overview.md)
- [Oznaka StackOverflow "azure-aktivno-directory" >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Sigurnosna ažuriranja za naši proizvodi

Pozivamo vas da biste dobili obavijesti kada doći do sigurnošću potražite na [toj stranici](https://technet.microsoft.com/security/dd252948) i pretplata na upozorenja savjeta za sigurnost.
