<properties
    pageTitle="Azure AD 2.0 NodeJS web-aplikacije | Microsoft Azure"
    description="Sastavljanje čvor JS web-aplikacije koje se potpisuje korisnika pomoću oba Microsoftova Account i računa tvrtke ili obrazovne ustanove."
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

# <a name="add-sign-in-to-a-nodejs-web-app"></a>Dodavanje prijavu nodeJS Web App


> [AZURE.NOTE]
    Neke scenarije servisa Azure Active Directory i značajke podržava krajnju točku 2.0.  Da biste utvrdili koristite krajnju točku 2.0, Saznajte više o [2.0 ograničenja](active-directory-v2-limitations.md).


U nastavku ćemo pomoću Passport za:

- Prijava korisnika u aplikaciji Azure AD i krajnju točku 2.0.
- Prikaz neke informacije o korisniku.
- Prijava korisnika iz aplikacije.

**Passport** je proizvod provjere autentičnosti za Node.js. Iznimno fleksibilne i modularan, Passport može biti unobtrusively ispuštanje da biste sve Express sustavom ili Resitify web-aplikacije. Opsežan skup strategije podržava provjeru autentičnosti pomoću korisničko ime i lozinku, Facebook, Twitter i drugo. Ne možemo ste razvili Strategije za Microsoft Azure Active Directory. Ne možemo će instalirati ovom modulu, a zatim dodajte Microsoft Azure Active Directory `passport-azure-ad` dodatka.

## <a name="download"></a>Preuzimanje

Kod za ovog praktičnog vodiča je spremljen [na GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs).  Da biste pratili, možete [preuzeti aplikacije skeleton kao u .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs/archive/skeleton.zip) ili Kloniraj na skeleton:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs.git```

Dovršeni aplikacija navedeni su na kraju ovog vodiča.

## <a name="1-register-an-app"></a>1. registrirati aplikacije
Stvaranje nove aplikacije na [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)ili slijedite ove [detaljne upute](active-directory-v2-app-registration.md).  Obavezno:

- Kopiraj dolje **Id aplikacije** dodijeljene aplikacije, morat ćete ga uskoro.
- Dodajte **Web** platformu za aplikacije.
- Unesite točne **Preusmjeravanje URI**. Preusmjeravanje URI naznačuje da Azure AD gdje treba usmjeriti odgovore provjere autentičnosti – zadano za ovog praktičnog vodiča `http://localhost:3000/auth/openid/return`.

## <a name="2-add-pre-requisities-to-your-directory"></a>2. dodati pre requisities direktorija

Iz naredbenog retka, promijenite direktorija korijensku mapu ako nije već postoji, a zatim izvršite sljedeće naredbe:

- `npm install express`
- `npm install ejs`
- `npm install ejs-locals`
- `npm install restify`
- `npm install mongoose`
- `npm install bunyan`
- `npm install assert-plus`
- `npm install passport`
- `npm install webfinger`
- `npm install body-parser`
- `npm install express-session`
- `npm install cookie-parser`

- Osim toga, ne možemo ste korištenje `passport-azure-ad` u skeleton za brzi početak rada.

- `npm install passport-azure-ad`


To se može instalirati biblioteke ovise o tom passport azure-ad.

## <a name="3-set-up-your-app-to-use-the-passport-node-js-strategy"></a>3. postavljanje aplikacije da biste koristili strategije passport čvor js
Ovdje, ne možemo konfigurirati proizvod Express koristiti protokol za povezivanje OpenID provjere autentičnosti.  Passport koristit će izdati zahtjeva za prijavu i sign-out i upravljanje njima sesija, informacije o korisniku, između ostalog.

-   Da biste započeli, otvorite na `config.js` datoteku u korijenskoj mapi projekta, a potom unesite vrijednosti za konfiguraciju vaše aplikacije u na `exports.creds` sekciju.
    -   U `clientID:` je **Id aplikacije** dodijeljene aplikacije na portalu za registraciju.
    -   U `returnURL` je **Preusmjeravanje URI** koju ste unijeli na portalu.
    - U `clientSecret` je tajna generira na portalu.

- Sljedećeg otvaranja `app.js` datoteka u korijenskom direktoriju u proejct i dodavanje follwing poziva za pozivanje na `OIDCStrategy` strategije koji se isporučuju s`passport-azure-ad`


```JavaScript
var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

// Add some logging
var log = bunyan.createLogger({
    name: 'Microsoft OIDC Example Web Application'
});
```

- Nakon toga pomoću strategije smo samo poziva za rukovanje naš zahtjevima za prijavu

```JavaScript
// Use the OIDCStrategy within Passport. (Section 2)
//
//   Strategies in passport require a `validate` function, which accept
//   credentials (in this case, an OpenID identifier), and invoke a callback
//   with a user object.
passport.use(new OIDCStrategy({
    callbackURL: config.creds.returnURL,
    realm: config.creds.realm,
    clientID: config.creds.clientID,
    clientSecret: config.creds.clientSecret,
    oidcIssuer: config.creds.issuer,
    identityMetadata: config.creds.identityMetadata,
    responseType: config.creds.responseType,
    responseMode: config.creds.responseMode,
    skipUserProfile: config.creds.skipUserProfile
    scope: config.creds.scope
  },
  function(iss, sub, profile, accessToken, refreshToken, done) {
    log.info('Example: Email address we received was: ', profile.email);
    // asynchronous verification, for effect...
    process.nextTick(function () {
      findByEmail(profile.email, function(err, user) {
        if (err) {
          return done(err);
        }
        if (!user) {
          // "Auto-registration"
          users.push(profile);
          return done(null, profile);
        }
        return done(null, user);
      });
    });
  }
));
```
Passport koristi slične uzorak za sve je strategije (Twitter, Facebook, itd.) koja u skladu svim autorima strategije. Pogled na na strategije potražite ćemo proslijediti ga function() token i gotovo kao parametre. Na strategije će dutifully vratite se na nam nakon što se sve dodatka rad. Kada se to čini želimo ćete za pohranu korisnika i stash token pa ćemo nećete morati Pitaj za njega.

> [AZURE.IMPORTANT]
Gore navedeni kod vodi bilo koji korisnik koji će se dogoditi za provjeru autentičnosti naš poslužitelj. To se naziva Registracija automatski. U proizvodne poslužitelje ne želite da biste svima omogućili bez prvog ih proći kroz postupak registracije odlučite. To je obično uzorak vam se prikazuje u aplikacijama potrošača koji omogućuju vam da biste registrirali sa servisom Facebook, no zatražiti da ispunite dodatne informacije. Ako to nije probna aplikacija, ne možemo nije su samo izdvojene e-pošte iz tokena objekta koji je vratio i zatražiti da ispunite dodatne podatke. Budući da je to testnom poslužitelju ne možemo jednostavno dodajte ih u bazu podataka u memoriji.

- Nakon toga dodat ćemo metode omogućit će nam da biste pratili na prijavljenih korisnika po potrebi po Passport. To obuhvaća Serijalizacija i prekidanja serije korisničke podatke:

```JavaScript

// Passport session setup. (Section 2)

//   To support persistent login sessions, Passport needs to be able to
//   serialize users into and deserialize users out of the session.  Typically,
//   this will be as simple as storing the user ID when serializing, and finding
//   the user by ID when deserializing.
passport.serializeUser(function(user, done) {
  done(null, user.email);
});

passport.deserializeUser(function(id, done) {
  findByEmail(id, function (err, user) {
    done(err, user);
  });
});

// array to hold logged in users
var users = [];

var findByEmail = function(email, fn) {
  for (var i = 0, len = users.length; i < len; i++) {
    var user = users[i];
    log.info('we are using user: ', user);
    if (user.email === email) {
      return fn(null, user);
    }
  }
  return fn(null, null);
};

```

- Nakon toga dodat ćemo koda za učitavanje eksplicitnih modul. Ovdje potražite koristimo /views zadane i /routes uzorak koji Express nudi.

```JavaScript

// configure Express (Section 2)

var app = express();


app.configure(function() {
  app.set('views', __dirname + '/views');
  app.set('view engine', 'ejs');
  app.use(express.logger());
  app.use(express.methodOverride());
  app.use(cookieParser());
  app.use(expressSession({ secret: 'keyboard cat', resave: true, saveUninitialized: false }));
  app.use(bodyParser.urlencoded({ extended : true }));
  // Initialize Passport!  Also use passport.session() middleware, to support
  // persistent login sessions (recommended).
  app.use(passport.initialize());
  app.use(passport.session());
  app.use(app.router);
  app.use(express.static(__dirname + '/../../public'));
});

```

- Na kraju, dodat ćemo usmjerava OBJAVA koje će rukom isključivanje stvarni prijava zahtjevi za na `passport-azure-ad` modul:

```JavaScript

// Our Auth routes (Section 3)

// GET /auth/openid
//   Use passport.authenticate() as route middleware to authenticate the
//   request.  The first step in OpenID authentication will involve redirecting
//   the user to their OpenID provider.  After authenticating, the OpenID
//   provider will redirect the user back to this application at
//   /auth/openid/return

app.get('/auth/openid',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {
    log.info('Authenitcation was called in the Sample');
    res.redirect('/');
  });

// GET /auth/openid/return
//   Use passport.authenticate() as route middleware to authenticate the
//   request.  If authentication fails, the user will be redirected back to the
//   login page.  Otherwise, the primary route function function will be called,
//   which, in this example, will redirect the user to the home page.
app.get('/auth/openid/return',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {

    res.redirect('/');
  });

// POST /auth/openid/return
//   Use passport.authenticate() as route middleware to authenticate the
//   request.  If authentication fails, the user will be redirected back to the
//   login page.  Otherwise, the primary route function function will be called,
//   which, in this example, will redirect the user to the home page.

app.post('/auth/openid/return',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {

    res.redirect('/');
  });
```

## <a name="4-use-passport-to-issue-sign-in-and-sign-out-requests-to-azure-ad"></a>4. koristi Passport za izdavanje prijave i sign-out zahtjeve za Azure AD

Pokrenite aplikaciju sada pravilno konfigurirani možete komunicirati s krajnje 2.0 koristiti protokol za povezivanje OpenID provjeru autentičnosti.  `passport-azure-ad`sadrži snimanja brigu o sve ugly pojedinosti obratite poruka provjere autentičnosti, provjera valjanosti tokena iz Azure AD i održavanje korisničke sesije.  Sve što ostaje je dati korisnicima omogućuje prijaviti, odjaviti i prikupljanje dodatnih informacija na prijavljenog korisnika.

- Najprije omogućuje dodavanje zadane, prijavu, račun i odjavite načina za naše `app.js` datoteke:

```JavaScript

//Routes (Section 4)

app.get('/', function(req, res){
  res.render('index', { user: req.user });
});

app.get('/account', ensureAuthenticated, function(req, res){
  res.render('account', { user: req.user });
});

app.get('/login',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {
    log.info('Login was called in the Sample');
    res.redirect('/');
});

app.get('/logout', function(req, res){
  req.logout();
  res.redirect('/');
});

```

-   Pogledajmo te detaljno:
    -   Na `/` usmjeravanje bit ćete preusmjereni na prikaz index.ejs prosljeđivanje korisnika u zahtjevu za (Ako postoji)
    - Na `/account` usmjeravanje će prvi ***provjerite ne možemo provjere autentičnosti*** (smo implementirati ispod) i zatim prenesite korisnika u zahtjevu za tako da se ne možemo možete dobiti dodatne informacije o korisniku.
    - Na `/login` usmjeravanje podići naše autentikatora azuread openidconnect iz `passport-azuread` i ako koji ne uspije će preusmjeravanje korisnika ponovno /login
    - Na `/logout` će jednostavno poziva na logout.ejs (i usmjeravanje) koji čisti kolačiće i zatim vratili korisnika index.ejs


- Za zadnji dio `app.js`, dodat ćemo način EnsureAuthenticated koja se koristi u `/account` iznad.

```JavaScript

// Simple route middleware to ensure user is authenticated. (Section 4)

//   Use this route middleware on any resource that needs to be protected.  If
//   the request is authenticated (typically via a persistent login session),
//   the request will proceed.  Otherwise, the user will be redirected to the
//   login page.
function ensureAuthenticated(req, res, next) {
  if (req.isAuthenticated()) { return next(); }
  res.redirect('/login')
}

```

- Na kraju, zapravo stvaranje poslužitelja sam u `app.js`:

```JavaScript

app.listen(3000);

```


## <a name="5-create-the-views-and-routes-in-express-to-display-our-user-in-the-website"></a>5. stvaranje prikaza i usmjerava u express da biste prikazali naših korisnika na web-mjestu

Imamo naše `app.js` dovršeno. Sada možemo jednostavno ćete morati dodati usmjerava i prikaze koje će biti prikazani podaci ćemo dobiti korisnika, kao i pokazivač na `/logout` i `/login` usmjerava smo ste stvorili.

- Stvaranje na `/routes/index.js` usmjeravanje u korijenskom direktoriju.

```JavaScript

/*
 * GET home page.
 */

exports.index = function(req, res){
  res.render('index', { title: 'Express' });
};
```

- Stvaranje na `/routes/user.js` usmjeravanje u korijenskom direktoriju

```JavaScript

/*
 * GET users listing.
 */

exports.list = function(req, res){
  res.send("respond with a resource");
};
```

Ove jednostavne usmjerava samo proći kroz duž zahtjev za naše prikaze, uključujući korisnika ako postoji.

- Stvaranje na `/views/index.ejs` prikaz u korijenskom direktoriju. To je jednostavno stranica koje će poziva naš prijava i odjavite metode i dopustite nam da biste privukli podatke o računu. Obavijest da možete koristiti uvjetno `if (!user)` kao korisnik koji se prošli kroz u zahtjevu za je dokaz imamo na prijavljenih u korisnika.

```JavaScript
<% if (!user) { %>
    <h2>Welcome! Please log in.</h2>
    <a href="/login">Log In</a>
<% } else { %>
    <h2>Hello, <%= user.displayName %>.</h2>
    <a href="/account">Account Info</a></br>
    <a href="/logout">Log Out</a>
<% } %>
```

- Stvaranje na `/views/account.ejs` prikazali u korijenskom direktoriju tako da se ne možemo možete vidjeti dodatne informacije koje `passport-azuread` prekinut zahtjev za korisnika.

```Javascript
<% if (!user) { %>
    <h2>Welcome! Please log in.</h2>
    <a href="/login">Log In</a>
<% } else { %>
<p>displayName: <%= user.displayName %></p>
<p>givenName: <%= user.name.givenName %></p>
<p>familyName: <%= user.name.familyName %></p>
<p>UPN: <%= user._json.upn %></p>
<p>Profile ID: <%= user.id %></p>
<p>Full Claimes</p>
<%- JSON.stringify(user) %>
<p></p>
<a href="/logout">Log Out</a>
<% } %>
```

- Na kraju, recimo provjerite to izgleda vrlo dodavanjem rasporeda. Stvaranje na ' / prikaz views/layout.ejs unesenim u korijenskom direktoriju

```HTML

<!DOCTYPE html>
<html>
    <head>
        <title>Passport-OpenID Example</title>
    </head>
    <body>
        <% if (!user) { %>
            <p>
            <a href="/">Home</a> |
            <a href="/login">Log In</a>
            </p>
        <% } else { %>
            <p>
            <a href="/">Home</a> |
            <a href="/account">Account</a> |
            <a href="/logout">Log Out</a>
            </p>
        <% } %>
        <%- body %>
    </body>
</html>
```

Na kraju, stvaranje i pokretanje aplikacije!

Pokrenite `node app.js` i otiđite do`http://localhost:3000`


Prijavite se pomoću osobnog Microsoftova Account ili pak računa tvrtke ili obrazovne ustanove i obratite pozornost na to kako će se na popisu /account odražava identitet korisnika.  Sada imate web-aplikacijama osigurani pomoću standardnih industrijskih protokola koje možete provjeru autentičnosti korisnika s njihovim osobne i poslovne/školske računi.

##<a name="next-steps"></a>Daljnji koraci

Za referencu, dovršeni uzorka (bez konfiguracijskih vrijednosti) [prikazuje se kao .zip ovdje](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs/archive/complete.zip)ili pak mogu Kloniraj ga iz GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs.git```

Sada se može pomaknuti na dodatne teme.  Preporučujemo vam da biste isprobali:

[Sigurne na node.js web-api pomoću krajnju točku 2.0 >>](active-directory-v2-devquickstarts-node-api.md)

Za dodatne resurse, pogledajte:
- [Vodič za razvojne inženjere 2.0 >>](active-directory-appmodel-v2-overview.md)
- [Oznaka StackOverflow "azure-aktivno-directory" >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Sigurnosna ažuriranja za naši proizvodi

Pozivamo vas da biste dobili obavijesti kada doći do sigurnošću potražite na [toj stranici](https://technet.microsoft.com/security/dd252948) i pretplata na upozorenja savjeta za sigurnost.
