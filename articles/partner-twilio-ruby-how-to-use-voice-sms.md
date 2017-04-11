<properties 
    pageTitle="Kako koristiti Twilio za glas i SMS (Ruby) | Microsoft Azure" 
    description="Saznajte kako nazvati i slanje SMS poruke sa servisom Twilio API na Azure. Primjere koda pisane Ruby." 
    services="" 
    documentationCenter="ruby" 
    authors="devinrader" 
    manager="twilio" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ruby" 
    ms.topic="article" 
    ms.date="11/25/2014" 
    ms.author="MicrosoftHelp@twilio.com"/>





# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-ruby"></a>Kako koristiti Twilio za glas i mogućnosti SMS u Ruby
Ovaj vodič pokazuje kako izvoditi uobičajene zadatke programiranje sa servisom Twilio API na Azure. Scenariji u kojima je moguće uvrstiti upućivanje telefonskog poziva i slanja poruke kratki servisa (SMS) poruka. Dodatne informacije o Twilio i njihovo korištenje glasa i SMS u vaše aplikacije potražite u odjeljku [Sljedeće korake](#NextSteps) .

## <a id="WhatIs"></a>Što je Twilio?
Twilio je API za telefoniranje web-usluge koje omogućuju korištenje postojeće jezici web i vještina da biste sastavili glasa i SMS aplikacije. Twilio je servis drugih proizvođača (ne Azure značajke i ne Microsoftovih proizvoda).

**Govorna Twilio** omogućuje aplikacija upućivati i primati telefonske pozive. **Twilio SMS** omogućuje aplikacija upućivati i primati SMS poruke. **Klijent Twilio** omogućuje vaše aplikacije da biste omogućili glasovnu komunikaciju pomoću postojeće internetske veze, uključujući mobilne veze.

## <a id="Pricing"></a>Twilio cijene i posebne ponude
Informacije o Twilio cijene dostupna je na [Twilio cijene] [twilio_pricing]. Azure klijenti dobiju [posebnu ponudu][special_offer]: besplatni kreditne kartice od 1000 tekstova ili 1000 dolazni minuta. Prijavite se ponuda ili saznali više, posjetite [http://ahoy.twilio.com/azure][special_offer].  

## <a id="Concepts"></a>Koncepti
Twilio API je RESTful API koja omogućuje glasa i funkcionalnosti SMS za aplikacije. Klijent biblioteka dostupni na više jezika na popisu u odjeljku [Twilio API biblioteke] [twilio_libraries].

### <a id="TwiML"></a>TwiML
TwiML je skup koji se temelji na XML upute koje obavijestiti Twilio način obrade poziva ili SMS.

Kao primjer, sljedeće TwiML želite pretvoriti taj tekst **Pozdrav svijeta** govor.

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

Sve dokumente TwiML ste `<Response>` kao njihove korijenski element. Iz nje, koristite Twilio glagoli da biste definirali ponašanje aplikacije.

### <a id="Verbs"></a>Glagoli TwiML
Glagoli Twilio su XML oznake koje određuju Twilio što **učiniti**. Na primjer, u ** &lt;izgovorite&gt; ** glagolski upućuje Twilio govorno isporučiti poruku na poziv. 

Slijedi popis Twilio glagola.

* ** &lt;Pozivanja&gt;**: povezuje pozivatelj na drugi telefon.
* ** &lt;Prikupite&gt;**: prikuplja znamenkama unijeli na tipkovnici telefona.
* ** &lt;Prekidanje&gt;**: završava poziva.
* ** &lt;Reproducirati&gt;**: reproducira zvučnu datoteku.
* ** &lt;Pauziraj&gt;**: tihu čeka određen broj sekundi.
* ** &lt;Zapis&gt;**: zapisuje govorne pozivatelju i vraća URL datoteke koja sadrži snimku.
* ** &lt;Preusmjeravanje&gt;**: prenosi kontrole poziva ili SMS TwiML na drugu URL ADRESU.
* ** &lt;Odbacivanje&gt;**: odbacuje dolazni poziv broju Twilio bez vam za naplatu
* ** &lt;Izgovorite&gt;**: pretvara tekst u govor koji je stvoren tijekom razgovora.
* ** &lt;Sms&gt;**: šalje poruku SMS.

Dodatne informacije o Twilio glagoli, njihove atribute i TwiML potražite u članku [TwiML] [twiml]. Dodatne informacije o Twilio API potražite u članku [Twilio API] [twilio_api].

## <a id="CreateAccount"></a>Stvaranje računa Twilio
Kada budete spremni za početak Twilio računa, prijavite se na [Pokušajte Twilio] [try_twilio]. Možete započeti s besplatan račun i kasnije nadograditi račun.

Kada se registrirate za Twilio račun, dobit ćete besplatni telefonski broj za svoju aplikaciju. Ćete primiti i račun SID i token za provjeru autentičnosti. Oba bit će potrebno da biste pozive Twilio API-JA. Da biste spriječili neovlašteni pristup računu, zaštiti token za provjeru autentičnosti. Računa SID i provjeru autentičnosti token mogu se prikazati na [stranici računa Twilio][twilio_account], u polja s natpisom **SID RAČUNA** i **Provjera autentičnosti TOKENA**, odnosno.

### <a id="VerifyPhoneNumbers"></a>Provjerite je li telefonskih brojeva
Uz broj ponudit će vam tako da Twilio, možete provjeriti i brojevi tu kontrolu (odnosno mobitel ili Kućni telefonski broj) za korištenje u aplikacije. 

Informacije o tome kako provjeriti telefonskog broja potražite u članku [Upravljanje brojeva] [verify_phone].

## <a id="create_app"></a>Stvaranje Ruby aplikacije
Ruby aplikaciju koja koristi servis Twilio i sustavom Azure je ne razlikuje se od neke druge Ruby aplikacije koja koristi uslugu Twilio. Dok su RESTful Twilio servisa, a možete pozvati iz Ruby na nekoliko načina, ovaj članak sadrži o korištenju servisa Twilio s [Twilio pomoćnih biblioteka za Ruby][twilio_ruby].

Prvo, [postaviti novu VM Linux Azure] [ azure_vm_setup] će poslužiti kao glavno računalo za nove Ruby web-aplikacije. Zanemari korake koje su potrebne za stvaranje aplikacije tračnicama, postaviti samo na VM. Provjerite je li stvaranje krajnje s vanjskog priključka od 80 i Interni priključak od 5000.

U primjerima koji slijede, ne možemo koristit će [Sinatra][sinatra], vrlo jednostavne web framework za Ruby. No certainly korištenja pomoćnih biblioteka Twilio za Ruby s bilo koje druge web framework, uključujući Ruby na tračnicama.

SSH u svoje nove VM i stvorite direktorij za nove aplikacije. U taj imenik stvoriti datoteku pod nazivom Gemfile i kopirajte sljedeći kod u njega:

    source 'https://rubygems.org'
    gem 'sinatra'
    gem 'thin'

U naredbenom retku pokrenite `bundle install`. To se može instalirati ovisnosti iznad. Sljedeće stvoriti datoteku pod nazivom `web.rb`. Ta će se gdje se nalaze kod za web-aplikacije. U nju zalijepite sljedeći kod:

    require 'sinatra'

    get '/' do
        "Hello Monkey!"
    end

U ovom trenutku trebali naredbu Pokreni `ruby web.rb -p 5000`. To će okretnog kopirane small web-poslužitelj na priključak 5000. Trebali biste moći pregledajte za aplikaciju u pregledniku tako što ste posjetili URL postaviti za vaše VM Azure. Kada dođete na web app u pregledniku, spremni ste počeli raditi u aplikaciji Twilio.

## <a id="configure_app"></a>Konfiguriranje aplikacija za korištenje Twilio
Možete konfigurirati web-aplikaciju u programa za potvrdu da biste koristili biblioteku Twilio ažuriranjem vaše `Gemfile` da biste dodali redak:

    gem 'twilio-ruby'

U naredbenom retku pokrenite `bundle install`. Sada otvorite `web.rb` tom retku na vrhu, uključujući:

    require 'twilio-ruby'

Sada sve je spremno za korištenje pomoćnih biblioteka Twilio za Ruby u web-aplikaciji.

## <a id="howto_make_call"></a>Kako: izvršiti odlazni poziv
Slijedi primjer kako izvršiti odlazni poziv. Koncepti obuhvaćaju upućivati pozive REST API-JA pomoću pomoćnih biblioteka Twilio za Ruby i vizualizacije TwiML. Zamjenske vrijednosti za telefonske brojeve **od** i **do** pa provjerite je li za vaš račun Twilio prije pokretanja koda provjerite telefonskog broja **iz** .

Dodajte ovu funkciju za `web.md`:

    # Set your account ID and authentication token.
    sid = "your_twilio_account_sid";
    token = "your_twilio_authentication_token";

    # The number of the phone initiating the the call.
    # This should either be a Twilio number or a number that you've verified
    from = "NNNNNNNNNNN";

    # The number of the phone receiving call.
    to = "NNNNNNNNNNN";

    # Use the Twilio-provided site for the TwiML response.
    url = "http://yourdomain.cloudapp.net/voice_url";
      
    get '/make_call' do
      # Create the call client.
      client = Twilio::REST::Client.new(sid, token);
      
      # Make the call
      client.account.calls.create(to: to, from: from, url: url)
    end

    post '/voice_url' do
      "<Response>
         <Say>Hello Monkey!</Say>
       </Response>"
    end
    
Ako se otvori gore `http://yourdomain.cloudapp.net/make_call` u pregledniku koji će pokrenuti poziv Twilio API da biste telefonski poziv. Prva dva parametara u `client.account.calls.create` su prilično self-explanatory: broj je poziv `from` i broj poziv `to`. 

Treći parametar (`url`) je URL koji zahtijeva Twilio da biste dobili upute o tome što učiniti nakon uspostave poziva. U ovom slučaju ne možemo postaviti u URL-a (`http://yourdomain.cloudapp.net`) koji vraća jednostavan TwiML dokument i koristi na `<Say>` glagolski učiniti neke pretvorbe teksta u govor, a zatim izgovorite "Pozdrav Majmun" osoba koja prima poziv.

## <a id="howto_recieve_sms"></a>Kako: primanje SMS poruku
U prethodnom primjeru ne možemo pokrenuti **odlazne** telefonski poziv. Ovaj put, recimo koristiti telefonski broj koji je Twilio dao nam prilikom registracije za obradu **dolazne** SMS poruke.

Prvi, prijava na [nadzornoj ploči Twilio][twilio_account]. Kliknite na "Brojeva" u gornjoj navigacijskoj, a zatim na Twilio broj koji ste naveli. Vidjet ćete dva URL-ovi koje možete konfigurirati. Zahtjev za URL govorne i programa SMS zahtjev URL-a. To su URL-ovi koji Twilio poziva svaki put kada je izvršena na telefonski poziv ili je SMS šalje se vaš broj. URL-ove i nazivaju se "web spojnica".

Željeli bismo obraditi dolazne SMS poruke, možemo ažurirajte URL `http://yourdomain.cloudapp.net/sms_url`. Odgovorite, a zatim kliknite Spremi promjene pri dnu stranice. Sada ponovno `web.rb` ćemo program naš program to učiniti:

    post '/sms_url' do
      "<Response>
         <Message>Hey, thanks for the ping! Twilio and Azure rock!</Message>
       </Response>"
    end

Kada unesete promjene, provjerite je li da biste ponovno pokrenuli web-aplikaciju programa. Sada uklanjanje telefona i pošaljite je SMS vaš telefon Twilio. Trebali biste dobiti nas je odgovor SMS poruka koja vas obavještava da "Hey, Hvala ping! Twilio i Azure rock! ".

## <a id="additional_services"></a>Kako: korištenje dodatnih Twilio usluga
Osim ovdje prikazani Primjeri, Twilio nudi utemeljen na webu API-ji koje možete koristiti odražava dodatne funkcije Twilio iz aplikacije za Azure. Sve pojedinosti potražite u [dokumentaciji Twilio API] [twilio_api_documentation].

### <a id="NextSteps"></a>Daljnji koraci
Sad kad ste naučili osnove servisa Twilio, slijedite ove veze na dodatne informacije:

* [Upute o sigurnosti Twilio] [twilio_security_guidelines]
* [Twilio HowTos i kod uzorka] [twilio_howtos]
* [Vodiči za brzi početak rada Twilio][twilio_quickstarts] 
* [Twilio na GitHub] [twilio_on_github]
* [Obratite se podršci za Twilio] [twilio_support]

[twilio_ruby]: https://www.twilio.com/docs/ruby/install





[twilio_pricing]: http://www.twilio.com/pricing
[special_offer]: http://ahoy.twilio.com/azure
[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api]: http://www.twilio.com/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_account]:  https://www.twilio.com/user/account
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[twilio_api_documentation]: http://www.twilio.com/api
[twilio_security_guidelines]: http://www.twilio.com/docs/security
[twilio_howtos]: http://www.twilio.com/docs/howto
[twilio_on_github]: https://github.com/twilio
[twilio_support]: http://www.twilio.com/help/contact
[twilio_quickstarts]: http://www.twilio.com/docs/quickstart
[sinatra]: http://www.sinatrarb.com/
[azure_vm_setup]: http://www.windowsazure.com/develop/ruby/tutorials/web-app-with-linux-vm/
