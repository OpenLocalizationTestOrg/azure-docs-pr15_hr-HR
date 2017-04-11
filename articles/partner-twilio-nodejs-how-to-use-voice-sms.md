<properties 
    pageTitle="Korištenje Twilio za govor, VoIP i SMS poruka u Azure" 
    description="Saznajte kako nazvati i slanje SMS poruke sa servisom Twilio API na Azure. Primjere koda pisane Node.js." 
    services="" 
    documentationCenter="nodejs" 
    authors="devinrader" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="11/25/2014" 
    ms.author="wpickett"/>


# <a name="using-twilio-for-voice-voip-and-sms-messaging-in-azure"></a>Korištenje Twilio za govor, VoIP i SMS poruka u Azure

Ovaj vodič pokazuje kako izraditi aplikacije koje komunicirati s Twilio i node.js na Azure.

<a id="whatis"/>
## <a name="what-is-twilio"></a>Što je Twilio?

Twilio je platforme API koja olakšava inženjerima omogućuje upućivanje i primanje telefonskih poziva slati i primati SMS poruke te ugrađivanje VoIP zovete u utemeljenima na pregledniku i izvorni mobilne aplikacije.  Pogledajmo ukratko ići preko kako to funkcionira prije čitanje.

### <a name="receiving-calls-and-text-messages"></a>Primanje poziva te tekstne poruke

Twilio razvojnim inženjerima omogućuje [tipkovnicu telefonski brojevi za kupnju] [ purchase_phone] koja se koristi za slanje i primanje poziva te poruke s tekstom.  Kada broj Twilio primi dolazni poziv ili tekst, Twilio poslat će web-aplikacije HTTP POST ili zahtjev za DOHVAĆANJE zatražiti upute o tome kako rukovati poziva ili teksta.  Poslužitelj će odgovoriti na zahtjev HTTP-Twilio s [TwiML][twiml], jednostavno skup XML oznake koje sadrže upute o tome kako rukovati poziva ili teksta.  Ne možemo će pogledajte primjere TwiML u samo trenutak.

### <a name="making-calls-and-sending-text-messages"></a>Upućivanje poziva i slanje tekstnih poruka

Uspostavljanjem HTTP zahtjevima API usluge za Twilio web razvojnim inženjerima možete slanja tekstnih poruka ili pokretanje izlaznih telefonske pozive.  Za odlazne pozive za razvojne inženjere morate navesti URL koji vraća TwiML upute za rukovanje Izlazni poziv kada je povezano.

### <a name="embedding-voip-capabilities-in-ui-code-javascript-ios-or-android"></a>Ugrađivanje VoIP mogućnosti korisničkog Sučelja kod (JavaScript, iOS ili Android)

Twilio nudi klijentsko SDK koje možete pretvoriti sve radne površine web-preglednik, aplikacije za iOS ili aplikacija za Android u VoIP telefona.  U ovom se članku ne možemo sadrži upute za korištenje VoIP poziva u pregledniku.  Osim JavaScript Twilio SDK izvode u pregledniku, poslužiteljsko aplikacije (naš program node.js) mora se koristiti za izdavanje "token mogućnost" klijentu JavaScript.  Dodatne informacije o korištenju VoIP s node.js [na blogu razvojni Twilio][voipnode].

<a id="signup"/>
## <a name="sign-up-for-twilio-microsoft-discount"></a>Prijavite se za Twilio (popust Microsoft)

Prije korištenja usluge Twilio, morate [registrirati za račun]prvi[signup].  Microsoft Azure klijenti dobiju poseban popust - [biti sigurni da biste se registrirali ovdje][signup]!

<a id="azuresite"/>
## <a name="create-and-deploy-a-nodejs-azure-website"></a>Stvorite i implementirajte node.js Azure web-mjesta

Nakon toga će potrebnih za stvaranje web-mjesto node.js sustavom Azure.  [Službeni dokumentaciju za to nalazi se ovdje][azure_new_site].  Visoke razine koji će biti sljedeći:

* Registracija za Azure račun ako ga nemate već
* Korištenje konzole za Azure administrator da biste stvorili novo web-mjesto
* Dodavanje izvora (će pretpostavimo da ste koristili brojka) podrška za kontrole
* Stvaranje datoteke `server.js` s web-aplikacije jednostavne node.js
* Implementacija ove jednostavne aplikacije za Azure

<a id="twiliomodule"/>
## <a name="configure-the-twilio-module"></a>Konfiguriranje modula Twilio

Nakon toga ne možemo će početi pisati jednostavne node.js aplikacije koje koristi Twilio API-JA.  Prije nego što smo započeli, moramo naše Twilio vjerodajnice računa za konfiguriranje.  

### <a name="configuring-twilio-credentials-in-system-environment-variables"></a>Konfiguriranje Twilio vjerodajnice u varijable okruženja sustava

Da biste omogućili zahtjeve čija je autentičnost provjerena protiv Twilio pozadinskih moramo našu stranicu računa SID i token za provjeru autentičnosti, koju funkciju kao korisničko ime i lozinku za račun naše Twilio postavljena. Najsigurnija način konfiguriranja te za korištenje s modula čvor u Azure je putem varijable okruženja sustava koje možete postaviti izravno u Azure administracijskoj konzoli sustava.

Odaberite node.js web-mjesta, a zatim kliknite vezu "KONFIGURIRAJ".  Ako pomičite bit će biti navedeno područje u kojem možete postaviti svojstva konfiguracije aplikacije.  Unesite vjerodajnice za račun Twilio ([nalazi se na nadzornu ploču Twilio][twilio_dashboard]) kao što je prikazano – provjerite naziv ih "TWILIO_ACCOUNT_SID" i "TWILIO_AUTH_TOKEN", odnosno:

![Azure administratorske konzole][azure-admin-console]

Nakon što ste konfigurirali ove varijable, pokrenite aplikaciju na konzoli za Azure.

### <a name="declaring-the-twilio-module-in-packagejson"></a>Deklariranje modula Twilio u package.json

Sljedeći je korak da biste stvorili package.json da biste upravljali naš čvor modul ovisnosti putem [npm].  Na istoj razini kao "server.js" datoteku koju ste stvorili u ovom praktičnom vodiču Azure/node.js stvoriti datoteku pod nazivom "package.json".  Unutar ove datoteke, postaviti sljedeće:

  {"naziv": "do naziva aplikacije", "verzije": "0.0.1", "Privatno": true, "skripte": {"Pokreni": "poslužitelj za čvor"}, "ovisnosti": {"express": "3.1.0", "ejs": "*", "twilio": "*"}}

To deklarira modul twilio kao ovisnosti, kao i popularne [framework eksplicitnih web] [ express] i modul EJS predložak.  U redu, sada ćemo sve je spremno - recimo pisanje neke koda!

<a id="makecall"/>
## <a name="make-an-outbound-call"></a>Provjerite odlazne poziva

Pogledajmo stvorite jednostavan obrazac koji će uspostave poziva na broj smo odaberite.  Otvorite server.js pa unesite sljedeći kod.  Imajte na umu mjesto na kojem piše "CHANGE_ME" - ima stavite naziv azure web-mjesta:

    // Module dependencies
    var express = require('express'), 
      path = require('path'), 
      http = require('http'), 
      twilio = require('twilio');

    // Create Express web application
    var app = express();

    // Express configuration
    app.configure(function(){
      app.set('port', process.env.PORT || 3000);
      app.set('views', __dirname + '/views');
      app.set('view engine', 'ejs');
      app.use(express.favicon());
      app.use(express.logger('dev'));
      app.use(express.bodyParser());
      app.use(express.methodOverride());
      app.use(app.router);
      app.use(express.static(path.join(__dirname, 'public')));
    });
    app.configure('development', function(){
      app.use(express.errorHandler());
    });

    // Render an HTML user interface for the application's home page
    app.get('/', function(request, response) {
      response.render('index');
    });

    // Handle the form POST to place a call
    app.post('/call', function(request, response) {
      var client = twilio();
      client.makeCall({
          // make a call to this number
          to:request.body.number,

          // Change to a Twilio number you bought - see:
          // https://www.twilio.com/user/account/phone-numbers/incoming
          from:'+15558675309',

          // A URL in our app which generates TwiML
          // Change "CHANGE_ME" to your app's name
          url:'https://CHANGE_ME.azurewebsites.net/outbound_call'
      }, function(error, data) {
          // Go back to the home page
          response.redirect('/');
      });
    });

    // Generate TwiML to handle an outbound call
    app.post('/outbound_call', function(request, response) {
      var twiml = new twilio.TwimlResponse();

      // Say a message to the call's receiver 
      twiml.say('hello - thanks for checking out Twilio and Azure', {
          voice:'woman'
      });

      response.set('Content-Type', 'text/xml');
      response.send(twiml.toString());
    });

    // Start server
    http.createServer(app).listen(app.get('port'), function(){
      console.log("Express server listening on port " + app.get('port'));
    });

Nakon toga stvorite direktorij nazivaju se "prikazi" - unutar taj imenik, stvorite datoteku pod nazivom "index.ejs" sadržajem sljedeće:

    <!DOCTYPE html>
    <html>
    <head>
      <title>Twilio Test</title>
      <style>
        input { height:20px; width:300px; font-size:18px; margin:5px; padding:5px; }
      </style>
    </head>
    <body>
      <h1>Twilio Test</h1>
      <form action="/call" method="POST">
          <input placeholder="Enter a phone number" name="number"/>
          <br/>
          <input type="submit" value="Call the number above"/>
      </form>
    </body>
    </html>

Sada implementacija web-mjesta Azure i otvorite vašem domu.  Trebali biste moći unesite telefonski broj u polju teksta i primanje poziva na vaš telefon Twilio!

<a id="sendmessage"/>
## <a name="send-an-sms-message"></a>Slanje SMS poruka

Sada postavimo korisničko sučelje i obrasca rukovanje logike da biste poslali poruku s tekstom.  Otvorite "server.js", a zatim dodajte sljedeći kod nakon zadnjeg poziv "app.post":

    app.post('/sms', function(request, response) {
      var client = twilio();
      client.sendSms({
          // send a text to this number
          to:request.body.number,

          // A Twilio number you bought - see:
          // https://www.twilio.com/user/account/phone-numbers/incoming
          from:'+15558675309',

          // The body of the text message
          body: request.body.message
          
      }, function(error, data) {
          // Go back to the home page
          response.redirect('/');
      });
    });

U "views/index.ejs", dodajte drugog obrasca u odjeljku prvoga slanje broj i tekst poruke:

    <form action="/sms" method="POST">
      <input placeholder="Enter a phone number" name="number"/>
      <br/>
      <input placeholder="Enter a message to send" name="message"/>
      <br/>
      <input type="submit" value="Send text to the number above"/>
    </form>

Ponovno implementirate aplikaciju Azure, a sada trebali biste moći poslati obrazac i sami (ili na najbliži prijateljima) tekstne poruke slati na!

<a id="nextsteps"/>
## <a name="next-steps"></a>Daljnji koraci

Sada ste naučili osnove korištenja node.js i Twilio da biste sastavili aplikacije koje komunikaciju.  No u ovim se primjerima takoreći odlaganje površina što je moguće s Twilio i node.js.  Dodatne informacije o korištenju Twilio s node.js, potražite u sljedećim resursima:

* [Modul za službeni dokumenti][docs]
* [Vodič za VoIP s aplikacijama node.js][voipnode]
* [Votr – u stvarnom vremenu SMS aplikacijom node.js i CouchDB (tri dijela) za glasovanje][votr]
* [Par programirati u web-pregledniku s node.js][pair]

Nadamo ih hacking node.js i Twilio na Azure!

[purchase_phone]: https://www.twilio.com/user/account/phone-numbers/available/local
[twiml]: https://www.twilio.com/docs/api/twiml
[signup]: http://ahoy.twilio.com/azure
[azure_new_site]: /app-service-web/web-sites-nodejs-develop-deploy-mac.md
[twilio_dashboard]: https://www.twilio.com/user/account
[npm]: http://npmjs.org
[express]: http://expressjs.com
[voipnode]: http://www.twilio.com/blog/2013/04/introduction-to-twilio-client-with-node-js.html
[docs]: http://twilio.github.io/twilio-node/
[votr]: http://www.twilio.com/blog/2012/09/building-a-real-time-sms-voting-app-part-1-node-js-couchdb.html
[pair]: http://www.twilio.com/blog/2013/06/pair-programming-in-the-browser-with-twilio.html
[azure-admin-console]: ./media/partner-twilio-nodejs-how-to-use-voice-sms/twilio_1.png



