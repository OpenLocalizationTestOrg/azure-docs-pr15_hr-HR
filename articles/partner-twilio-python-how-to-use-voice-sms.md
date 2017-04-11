<properties
    pageTitle="Kako koristiti Twilio za glas i SMS (i) | Microsoft Azure"
    description="Saznajte kako nazvati i slanje SMS poruke sa servisom Twilio API na Azure. Primjere koda napisan i."
    services=""
    documentationCenter="python"
    authors="devinrader"
    manager="twilio"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="python" 
    ms.topic="article"
    ms.date="02/19/2015"
    ms.author="MicrosoftHelp@twilio.com"/>





# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-php"></a>Kako koristiti Twilio za glas i mogućnosti SMS u PHP
Ovaj vodič pokazuje kako izvoditi uobičajene zadatke programiranje sa servisom Twilio API na Azure. Scenariji u kojima je moguće uvrstiti upućivanje telefonskog poziva i slanja poruke kratki servisa (SMS) poruka. Dodatne informacije o Twilio i njihovo korištenje glasa i SMS u vaše aplikacije potražite u odjeljku [Sljedeće korake](#NextSteps) .

## <a id="WhatIs"></a>Što je Twilio?
Twilio je powering budućnost komunikacije tvrtke, omogućivanje razvojnim inženjerima da biste ugradili govorne, VoIP i poruka u aplikacije. Oni virtualizacije infrastruktura za sve potrebne u oblaku, globalni okruženju će putem platforme komunikacije API Twilio. Aplikacije su jednostavni Sastavljanje i skalabilni. Uživajte u fleksibilnost sa plaćanje-kao otvorite cijene i prednosti pouzdanosti oblaka.

**Govorna Twilio** omogućuje aplikacija upućivati i primati telefonske pozive. **Twilio SMS** omogućuje aplikacije za slanje i primanje tekstnih poruka. **Klijent Twilio** omogućuje vam za VoIP pozive s bilo kojeg telefona, tableta ili preglednika i podržava WebRTC.

## <a id="Pricing"></a>Twilio cijene i posebne ponude

Azure klijenti dobiju [posebnu ponudu] $10 Twilio odobrenja prilikom nadogradnje Twilio račun. Ovaj Twilio odobrenja primjenjuje se na sve Twilio korištenje ($10 odobrenja jednako 1000 SMS poruke slati i primati do 1000 ulaznog govorne minuta, ovisno o lokaciji telefonski broj i poruku ili poziv odredište). Aktivacija ovaj Twilio odobrenja i početak rada na: [ahoy.twilio.com/azure].

Twilio je pay-as-you-go servis. Postoje bez naknade za postavljanje međuverzije i u bilo kojem trenutku možete zatvoriti svoj račun. Dodatne informacije možete pronaći na [Cijene Twilio] [twilio_pricing].

## <a id="Concepts"></a>Koncepti
Twilio API je RESTful API koja omogućuje glasa i funkcionalnosti SMS za aplikacije. Klijent biblioteka dostupni na više jezika na popisu u odjeljku [Twilio API biblioteke] [twilio_libraries].

Ključni aspekte Twilio API su Twilio glagoli i Twilio Markup Language (TwiML).

### <a id="Verbs"></a>Glagoli Twilio
U API omogućuje korištenje Twilio glagoli; na primjer, u ** &lt;izgovorite&gt; ** glagolski upućuje Twilio govorno isporučiti poruku na poziv.

Slijedi popis Twilio glagola. Dodatne informacije o drugim glagoli i mogućnosti putem [Twilio Markup Language dokumentaciju] [http://www.twilio.com/docs/api/twiml].

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

### <a id="TwiML"></a>TwiML
TwiML je skup koji se temelji na XML upute na temelju glagoli Twilio koji obavijestiti Twilio način obrade poziva ili SMS.

Kao primjer, sljedeće TwiML želite pretvoriti taj tekst **Pozdrav svijeta** govor.

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

Kada aplikacija nazove Twilio API, jedan od parametara API jest URL koji vraća TwiML odgovor. Za razvoj svrhe, možete koristiti URL-ovi Twilio možete unijeti odgovore TwiML koristi aplikacija. Nije moguće hostira i vlastite URL-ovi, čime se dobiva TwiML odgovore, a druga mogućnost je pomoću **TwiMLResponse** objekta.

Dodatne informacije o Twilio glagoli, njihove atribute i TwiML potražite u članku [TwiML] [twiml]. Dodatne informacije o Twilio API potražite u članku [Twilio API] [twilio_api].

## <a id="CreateAccount"></a>Stvaranje računa Twilio
Kada ste spremni za početak Twilio računa, prijavite se na [Pokušajte Twilio] [try_twilio]. Možete započeti s besplatan račun i kasnije nadograditi račun.

Kada se registrirate za račun Twilio, primit ćete ID računa i token za provjeru autentičnosti. Oba bit će potrebno da biste pozive Twilio API-JA. Da biste spriječili neovlašteni pristup računu, zaštiti token za provjeru autentičnosti. ID računa i provjera autentičnosti token mogu se prikazati na [stranici računa Twilio] [twilio_account], u polja s natpisom **SID RAČUNA** i **Provjera autentičnosti TOKENA**, odnosno.

## <a id="create_app"></a>Stvaranje i aplikacije
Aplikacija i koji koristi servis Twilio i sustavom Azure je ne razlikuje se od druge aplikacije i koja koristi uslugu Twilio. Dok Twilio services su utemeljene na OSTALE, a možete pozvati iz i na nekoliko načina, ovaj članak sadrži o korištenju servisa Twilio s [bibliotekom Twilio za PHP iz GitHub][twilio_php]. Dodatne informacije o korištenju biblioteka Twilio za PHP, potražite u članku [http://readthedocs.org/docs/twilio-php/en/latest/index.html][twilio_lib_docs].

Detaljne upute za stvaranje i implementacija Twilio/i aplikaciju za Azure dostupne su na [način na koji Twilio pomoću za telefonski poziv u aplikaciji i na Azure][howto_phonecall_php].

## <a id="configure_app"></a>Konfiguriranje aplikacija za korištenje Twilio biblioteka
Možete konfigurirati aplikacije da biste koristili biblioteku Twilio za PHP na dva načina:

1. Preuzimanje biblioteke Twilio za PHP iz GitHub ([https://github.com/twilio/twilio-php][twilio_php]) i dodajte imenik **servisa** aplikacija.

    – ILI –

2. Instalirajte Twilio biblioteka za i kao paket KRUŠAKA. Moguće je instalirati sljedeće naredbe:

        $ pear channel-discover twilio.github.com/pear
        $ pear install twilio/Services_Twilio

Nakon instalacije biblioteku Twilio za PHP, pa možete dodati **require_once** izjava pri vrhu datoteke i referencu u biblioteku:

        require_once 'Services/Twilio.php';

Dodatne informacije potražite u članku [https://github.com/twilio/twilio-php/blob/master/README.md][twilio_github_readme].

## <a id="howto_make_call"></a>Kako: izvršiti odlazni poziv
Slijedi primjer kako izvršiti odlazni poziv pomoću klase **Services_Twilio** . Kod koristi i web-mjesta Twilio – pod uvjetom da biste se vratili odgovor Twilio Markup Language (TwiML). Zamjenske vrijednosti za telefonske brojeve **od** i **do** pa provjerite je li za vaš račun Twilio prije pokretanja koda provjerite telefonskog broja **iz** .

    // Include the Twilio PHP library.
    require_once 'Services/Twilio.php';

    // Library version.
    $version = "2010-04-01";

    // Set your account ID and authentication token.
    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";

    // The number of the phone initiating the the call.
    // (Must be previously validated with Twilio.)
    $from_number = "NNNNNNNNNNN";

    // The number of the phone receiving call.
    $to_number = "NNNNNNNNNNN";

    // Use the Twilio-provided site for the TwiML response.
    $url = "http://twimlets.com/message";

    // The phone message text.
    $message = "Hello world.";

    // Create the call client.
    $client = new Services_Twilio($sid, $token, $version);

    //Make the call.
    try
    {
        $call = $client->account->calls->create(
            $from_number,
            $to_number,
            $url.'?Message='.urlencode($message)
        );
    }
    catch (Exception $e)
    {
        echo 'Error: ' . $e->getMessage();
    }

Kao što je rečeno, kod koristi web-mjesta pod uvjetom Twilio da biste se vratili TwiML odgovor. Nije moguće umjesto toga koristite web-mjestu možete unijeti odgovora TwiML; Dodatne informacije potražite u članku [kako unijeti TwiML odgovore od vaše vlastite Web-mjesta](#howto_provide_twiml_responses).


- **Napomena**: da biste otklonili pogreške provjere valjanosti za SSL certifikata, potražite u članku [http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html][ssl_validation]


## <a id="howto_send_sms"></a>Kako: slanje SMS poruka
Slijedi primjer kako slanje SMS poruke pomoću klase **Services_Twilio** . Broj **s** osigurava Twilio za probno razdoblje račune za slanje SMS poruka. Broj **Da biste** se moraju provjeriti za vaš račun Twilio prije pokretanja kod.

    // Include the Twilio PHP library.
    require_once 'Services/Twilio.php';

    // Library version.
    $version = "2010-04-01";

    // Set your account ID and authentication token.
    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";


    $from_number = "NNNNNNNNNNN"; // With trial account, texts can only be sent from your Twilio number.
    $to_number = "NNNNNNNNNNN";
    $message = "Hello world.";

    // Create the call client.
    $client = new Services_Twilio($sid, $token, $version);

    // Send the SMS message.
    try
    {
        $client->account->sms_messages->create($from_number, $to_number, $message);
    }
    catch (Exception $e)
    {
        echo 'Error: ' . $e->getMessage();
    }

## <a id="howto_provide_twiml_responses"></a>Kako: Navedite TwiML odgovore od vlastito web-mjesto
Kada aplikacija pokreće poziv Twilio API, Twilio će poslati zahtjev URL koji se očekuje da biste se vratili TwiML odgovor. Gornji primjer koristi URL-ovi Twilio [http://twimlets.com/message][twimlet_message_url]. (Dok TwiML namijenjen korištenju tako da Twilio, možete pogledati it u pregledniku. Na primjer, kliknite [http://twimlets.com/message] [ twimlet_message_url] da biste vidjeli prazna `<Response>` element; drugi primjer kliknite [http://twimlets.com/message?Message%5B0%5D=Hello%20World] [ twimlet_message_url_hello_world] da biste vidjeli na `<Response>` element koji sadrži na `<Say>` element.)

Tipkanje URL-ovi Twilio, možete stvoriti vlastitu web-mjesta koja vraća HTTP odgovora. Možete stvoriti web-mjesto u bilo koji jezik koji prikazuje odgovore XML; u ovoj se temi pretpostavlja da će koristiti i da biste stvorili na TwiML.

Na sljedećoj stranici i rezultira TwiML odgovora koja vas obavještava da **Pozdrav svijetu** na poziv.

    <?php
        header("content-type: text/xml");
        echo "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n";
    ?>
    <Response>
        <Say>Hello world.</Say>
    </Response>

Kao što možete vidjeti iz gornji primjer, odgovor TwiML je jednostavno XML dokument. Biblioteka Twilio za PHP sadrži klase koje će generirati TwiML umjesto vas. Primjeru u nastavku daje ekvivalentan odgovor, kao što je prikazano gore, ali koristi u **Services\_Twilio\_Twiml** klase u biblioteku Twilio PHP:

    require_once('Services/Twilio.php');

    $response = new Services_Twilio_Twiml();
    $response->say("Hello world.");
    print $response;

Dodatne informacije o TwiML potražite u članku [https://www.twilio.com/docs/api/twiml][twiml_reference].

Nakon što dodate i stranici postavljen za pružaju TwiML odgovore, koristite URL stranice i kao što je URL ušli u na `Services_Twilio->account->calls->create` način. Na primjer, ako imate pod nazivom **MyTwiML** implementiran na web-aplikacije programa Azure hostira servis, a naziv stranice i je **mytwiml.php**, URL-a mogu se proslijediti **Services_Twilio -> račun -> pozive -> Stvaranje** kao što je prikazano u sljedećem primjeru:

    require_once 'Services/Twilio.php';

    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";
    $from_number = "NNNNNNNNNNN";
    $to_number = "NNNNNNNNNNN";
    $url = "http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.php";

    // The phone message text.
    $message = "Hello world.";

    $client = new Services_Twilio($sid, $token, "2010-04-01");

    try
    {
        $call = $client->account->calls->create(
            $from_number,
            $to_number,
            $url.'?Message='.urlencode($message)
        );
    }
    catch (Exception $e)
    {
        echo 'Error: ' . $e->getMessage();
    }

Dodatne informacije o korištenju Twilio u Azure s i Saznajte [Kako izraditi Twilio pomoću za telefonski poziv u aplikaciji i na Azure][howto_phonecall_php].

## <a id="AdditionalServices"></a>Kako: korištenje dodatnih Twilio usluga
Osim ovdje prikazani Primjeri, Twilio nudi utemeljen na webu API-ji koje možete koristiti odražava dodatne funkcije Twilio iz aplikacije za Azure. Sve pojedinosti potražite u [dokumentaciji Twilio API] [twilio_api_documentation].

## <a id="NextSteps"></a>Daljnji koraci
Sad kad ste naučili osnove servisa Twilio, slijedite ove veze na dodatne informacije:

* [Upute o sigurnosti Twilio] [twilio_security_guidelines]
* [Vodilice Twilio Nemoderirana i kod uzorka] [twilio_howtos]
* [Vodiči za brzi početak rada Twilio][twilio_quickstarts]
* [Twilio na GitHub] [twilio_on_github]
* [Obratite se podršci za Twilio] [twilio_support]

[twilio_php]: https://github.com/twilio/twilio-php
[twilio_lib_docs]: http://readthedocs.org/docs/twilio-php/en/latest/index.html
[twilio_github_readme]: https://github.com/twilio/twilio-php/blob/master/README.md
[ssl_validation]: http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html

[howto_phonecall_php]: http://windowsazure.com/documentation/articles/partner-twilio-php-make-phone-call
[twimlet_message_url]: http://twimlets.com/message
[twimlet_message_url_hello_world]: http://twimlets.com/message?Message%5B0%5D=Hello%20World
[twiml_reference]: https://www.twilio.com/docs/api/twiml
[twilio_pricing]: http://www.twilio.com/pricing

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
