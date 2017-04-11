<properties
    pageTitle="Upute za upućivanje telefonskog poziva s Twilio (i) | Microsoft Azure"
    description="Saznajte kako nazvati i slanje SMS poruke sa servisom Twilio API na Azure. Uzorci su i aplikacije."
    documentationCenter="php"
    services=""
    authors="devinrader"
    manager="twilio"
    editor="mollybos"/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="11/25/2014"
    ms.author="microsofthelp@twilio.com"/>

# <a name="how-to-make-a-phone-call-using-twilio-in-a-php-application-on-azure"></a>Upute za upućivanje telefonskog poziva pomoću Twilio u aplikaciji i na Azure

Sljedeći primjer pokazuje kako koristiti Twilio upućivanje poziva iz web-stranice i smješten u Azure. Rezultat aplikacije tražit će od korisnika nazvati vrijednosti, kao što je prikazano na sljedećem zaslonu snimka.

![Obrazac za Azure poziv pomoću Twilio i PHP][twilio_php]

Morat ćete biste pomoću koda u ovoj temi, učinite sljedeće:

1. Nabava Twilio računa i provjera autentičnosti tokena. Za početak rada s Twilio, procijeniti cijene pri [http://www.twilio.com/pricing][twilio_pricing]. Možete se prijaviti za probnu račun pri [https://www.twilio.com/try-twilio][try_twilio]. Informacije o API nudi Twilio potražite u članku [http://www.twilio.com/api][twilio_api].
2. Nabavite [Twilio biblioteka za PHP](https://github.com/twilio/twilio-php) ili ga instalirati kao paket KRUŠAKA. Dodatne informacije potražite u članku [datoteke readme](https://github.com/twilio/twilio-php/blob/master/README.md).
3. Instalirajte Azure SDK za i. Da biste saznali SDK i upute za instalaciju potražite u članku [Postavljanje SDK Azure i][setup_php_sdk].

## <a name="create-a-web-form-for-making-a-call"></a>Stvaranje web-obrasca za upućivanje poziva

Sljedeći HTML kod prikazan je način stvaranja web-stranicu (**callform.html**) koja se dohvaća korisničkih podataka za upućivanje poziva:

    <html>
    <head>
        <title>Automated call form</title>
    </head>
    <body>
    <h1>Automated Call Form</h1>
    <p>Fill in all fields and click <b>Make this call</b>.</p>
    <form action="makecall.php" method="post">
    <table>
        <tr>
            <td>To:</td>
            <td><input type="text" size=50 name="callTo" value=""></td>
        </tr>
        <tr>
            <td>From:</td>
            <td><input type="text" size=50 name="callFrom" value=""></td>
        </tr>
        <tr>
            <td>Call message:</td>
            <td><input type="text" size=100 name="callText" value="Hello. This is the call text. Good bye." /></td>
        </tr>
        <tr>
            <td colspan=2><input type="submit" value="Make this call"></td>
        </tr>
    </table>
    </form>
    <br/>
    </body>
    </html>

## <a name="create-the-code-to-make-the-call"></a>Stvaranje kod da biste poziv
Sljedeći kod prikazuje način stvaranja web-stranice (**makecall.php**) koja se naziva i kada korisnik šalje obrazac koji se prikazuje **callform.html**. Kod je prikazano u nastavku stvara poruku poziva i generira poziv. (Koristi Twilio računa i provjera autentičnosti token umjesto vrijednosti rezervirano mjesto dodijeljene **$sid** i **$token** u kodu niže).

    <html>
    <head><title>Making call...</title></head>
    <body>
    <p>Your call is being made.</p>

    <?php
    require_once 'Services/Twilio.php';

    $sid = "your_account_sid";
    $token = "your_authentication_token";

    $from_number = $_POST['callFrom']; // Calls must be made from a registered Twilio number.
    $to_number = $_POST['callTo'];
    $message = $_POST['callText'];

    $client = new Services_Twilio($sid, $token, "2010-04-01");

    $call = $client->account->calls->create(
        $from_number,
        $to_number,
        'http://twimlets.com/message?Message='.urlencode($message)
    );

    echo "Call status: ".$call->status."<br />";
    echo "URI resource: ".$call->uri."<br />";
    ?>
    </body>
    </html>

Osim upućivanje poziva pomoću **makecall.php** prikazuje neke metapodatke poziva (primjer je u nastavku snimka). Dodatne informacije o pozivu metapodataka potražite u članku [https://www.twilio.com/docs/api/rest/call#instance-properties][twilio_call_properties].

![Odgovor Azure poziv pomoću Twilio i PHP][twilio_php_response]

## <a name="run-the-application"></a>Pokrenite aplikaciju
Sljedeći je korak za implementaciju aplikacije da biste Azure web-mjesta. Sljedeći članci sadrže informacije za stvaranje web-mjesta i implementacija kod brojka, FTP ili WebMatrix (iako vrijedi nisu sve informacije svakog članka):

* [Stvaranje Web-mjesta i MySQL Azure i implementacija pomoću brojka][website-git]
* [Stvaranje i MySQL Azure Web-mjesta i implementacija pomoću FTP][website-ftp]

## <a name="next-steps"></a>Daljnji koraci
Da bi se prikazala osnovne funkcije pomoću Twilio i na Azure nije naveden kod. Prije no što implementirate Azure u proizvodnje, preporučujemo vam da biste dodali više pogreškama ili druge značajke. Ako, na primjer:

* Umjesto korištenja web-obrasca, može koristiti Azure prostora za pohranu blob-ova ili SQL baze podataka za pohranu telefonske brojeve i tekst poziva. Informacije o korištenju blob-ova Azure prostora za pohranu u PHP, potražite u članku [Korištenje pohranom Azure s aplikacijama i][howto_blob_storage_php]. Informacije o korištenju baze podataka SQL u PHP, potražite u članku [Korištenje SQL baze podataka pomoću aplikacije i][howto_sql_azure_php].
* Kod **makecall.php** koristi URL-ovi Twilio ([http://twimlets.com/message][twimlet_message_url]) da biste naveli odgovor Twilio Markup Language (TwiML) koja obavještava Twilio upute da biste nastavili s pozivom. Na primjer, mogu sadržavati TwiML koja se vraća na `<Say>` glagolski čiji je rezultat tekst Izgovor poziva primatelju. Umjesto korištenja URL-ovi Twilio nije sastavljanje vlastite servisa odgovoriti na zahtjev na Twilio; Dodatne informacije potražite [u]članku korištenje Twilio za glas i mogućnosti SMS u PHP[howto_twilio_voice_sms_php]. Dodatne informacije o TwiML nalazi se na [http://www.twilio.com/docs/api/twiml][twiml], a dodatne informacije o `<Say>` i druge glagoli Twilio nalazi se na [http://www.twilio.com/docs/api/twiml/say][twilio_say].
* Pročitajte upute o sigurnosti Twilio pri [https://www.twilio.com/docs/security][twilio_docs_security].

Dodatne informacije o Twilio potražite u članku [https://www.twilio.com/docs][twilio_docs].

## <a name="see-also"></a>Vidi također
* [Kako koristiti Twilio za glas i mogućnosti SMS u PHP](partner-twilio-php-how-to-use-voice-sms.md)

[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/api
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[setup_php_sdk]: http://azurephp.interoperabilitybridges.com/articles/setup-the-windows-azure-sdk-for-php
[twimlet_message_url]: http://twimlets.com/message
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api_service]: http://api.twilio.com
[build_php_azure_app]: http://azurephp.interoperabilitybridges.com/articles/build-and-deploy-a-windows-azure-php-application
[howto_twilio_voice_sms_php]: partner-twilio-php-how-to-use-voice-sms.md
[howto_blob_storage_php]: http://azure.microsoft.com/documentation/articles/storage-php-how-to-use-blobs/
[howto_sql_azure_php]: http://azure.microsoft.com/documentation/articles/sql-database-php-how-to-use/
[twilio_call_properties]: https://www.twilio.com/docs/api/rest/call#instance-properties
[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say
[ssl_validation]: http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html
[twilio_php]: ./media/partner-twilio-php-make-phone-call/WA_TwilioPHPCallForm.jpg
[twilio_php_response]: ./media/partner-twilio-php-make-phone-call/WA_TwilioPHPMakeCall.jpg
[website-git]: ./web-sites/web-sites-php-mysql-deploy-use-git.md
[website-ftp]: ./web-sites/web-sites-php-mysql-deploy-use-ftp.md
[twilio_php_github]: https://github.com/twilio/twilio-php
