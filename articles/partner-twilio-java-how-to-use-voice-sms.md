<properties 
    pageTitle="Kako koristiti Twilio za glas i SMS (Java) | Microsoft Azure" 
    description="Saznajte kako nazvati i slanje SMS poruke sa servisom Twilio API na Azure. Primjere koda pisane Java." 
    services="" 
    documentationCenter="java" 
    authors="devinrader" 
    manager="twilio" 
    editor="mollybos"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="11/25/2014" 
    ms.author="microsofthelp@twilio.com"/>

# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-java"></a>Kako koristiti Twilio za glas i mogućnosti SMS u Java

Ovaj vodič pokazuje kako izvoditi uobičajene zadatke programiranje sa servisom Twilio API na Azure. Scenariji u kojima je moguće uvrstiti upućivanje telefonskog poziva i slanja poruke kratki servisa (SMS) poruka. Dodatne informacije o Twilio i njihovo korištenje glasa i SMS u vaše aplikacije potražite u odjeljku [Sljedeće korake](#NextSteps) .

## <a id="WhatIs"></a>Što je Twilio?
Twilio je API za telefoniranje web-usluge koje omogućuju korištenje postojeće jezici web i vještina da biste sastavili glasa i SMS aplikacije. Twilio je servis drugih proizvođača (ne Azure značajke i ne Microsoftovih proizvoda).

**Govorna Twilio** omogućuje aplikacija upućivati i primati telefonske pozive. **Twilio SMS** omogućuje aplikacija upućivati i primati SMS poruke. **Klijent Twilio** omogućuje vaše aplikacije da biste omogućili glasovnu komunikaciju pomoću postojeće internetske veze, uključujući mobilne veze.

## <a id="Pricing"></a>Twilio cijene i posebne ponude
Informacije o Twilio cijene dostupna je na [Twilio cijene] [twilio_pricing]. Azure klijenti dobiju [posebnu ponudu][special_offer]: besplatni kreditne kartice od 1000 tekstova ili 1000 dolazni minuta. Prijavite se ponuda ili saznali više, posjetite [http://ahoy.twilio.com/azure][special_offer].  

## <a id="Concepts"></a>Koncepti
Twilio API je RESTful API koja omogućuje glasa i funkcionalnosti SMS za aplikacije. Klijent biblioteka dostupni na više jezika na popisu u odjeljku [Twilio API biblioteke] [twilio_libraries].

Ključni aspekte Twilio API su Twilio glagoli i Twilio Markup Language (TwiML).

### <a id="Verbs"></a>Glagoli Twilio
U API omogućuje korištenje Twilio glagoli; na primjer, u ** &lt;izgovorite&gt; ** glagolski upućuje Twilio govorno isporučiti poruku na poziv. 

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
Kada budete spremni za početak Twilio računa, prijavite se na [Pokušajte Twilio] [try_twilio]. Možete započeti s besplatan račun i kasnije nadograditi račun.

Kada se registrirate za račun Twilio, primit ćete ID računa i token za provjeru autentičnosti. Oba bit će potrebno da biste pozive Twilio API-JA. Da biste spriječili neovlašteni pristup računu, zaštiti token za provjeru autentičnosti. ID računa i provjera autentičnosti token mogu se prikazati na [stranici računa Twilio] [twilio_account], u polja s natpisom **SID RAČUNA** i **Provjera autentičnosti TOKENA**, odnosno.

## <a id="create_app"></a>Stvaranje aplikacije Java
1. Nabavite POSUDU Twilio i ga dodati na put Sastavi Java i skupa za implementaciju sustava WAR. Pri [https://github.com/twilio/twilio-java][twilio_java], možete preuzeti GitHub izvora i stvoriti vlastiti POSUDU ili preuzimanje ugrađene POSUDU (sa ili bez ovisnosti).
2. Provjerite je li vaša JDK **cacerts** keystore sadrži certifikat Equifax sigurne ustanova za izdavanje certifikata s MD5 otiska prsta 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (serijski broj je 35:DE:F4:CF, a SHA1 otisak D2:32:09:AD:23:D3:14:23:21:74:E4:0 D: 7F:9 D: 62:13:97:86:63:3A). Certifikat za izdavanje certifikata (CA) certifikat za [https://api.twilio.com] [ twilio_api_service] servis, koji se naziva prilikom korištenja API-ji Twilio. Informacije o osiguravanje vaše JDK **cacerts** keystore sadrži ispravne ustanove za Izdavanje certifikata potražite u članku [Dodavanje certifikat u spremište Java ustanove za Izdavanje certifikata][add_ca_cert].

Detaljne upute za korištenje u biblioteku Twilio klijenta za Java dostupne su na [način na koji Twilio pomoću za telefonski poziv u aplikaciji Java na Azure][howto_phonecall_java].

## <a id="configure_app"></a>Konfiguriranje aplikacija za korištenje Twilio biblioteka
U kodu, možete dodati naredbe **Uvoz** pri vrhu izvorne datoteke za pakete Twilio ili klase koji želite koristiti u aplikaciji. 

Za Java izvorne datoteke:

    import com.twilio.*;
    import com.twilio.sdk.*;
    import com.twilio.sdk.resource.factory.*;
    import com.twilio.sdk.resource.instance.*;

Za Java Server Page (JSP) izvorne datoteke:

    import="com.twilio.*"
    import="com.twilio.sdk.*"
    import="com.twilio.sdk.resource.factory.*"
    import="com.twilio.sdk.resource.instance.*"
Ovisno o tome koji Twilio paketa ili klase koji želite koristiti, svojih izjava **Uvoz** mogu se razlikovati.

## <a id="howto_make_call"></a>Kako: izvršiti odlazni poziv
Slijedi primjer kako izvršiti odlazni poziv pomoću klase **CallFactory** . Kod koristi i web-mjesta Twilio – pod uvjetom da biste se vratili odgovor Twilio Markup Language (TwiML). Zamjenske vrijednosti za telefonske brojeve **od** i **do** pa provjerite je li za vaš račun Twilio prije pokretanja koda provjerite telefonskog broja **iz** .

    // Use your account SID and authentication token instead
    // of the placeholders shown here.
    String accountSID = "your_twilio_account";
    String authToken = "your_twilio_authentication_token";

    // Create an instance of the Twilio client.
    TwilioRestClient client;
    client = new TwilioRestClient(accountSID, authToken);

    // Retrieve the account, used later to create an instance of the CallFactory.
    Account account = client.getAccount();

    // Use the Twilio-provided site for the TwiML response.
    String Url="http://twimlets.com/message";
    Url = Url + "?Message%5B0%5D=Hello%20World";

    // Place the call From, To and URL values into a hash map. 
    HashMap<String, String> params = new HashMap<String, String>();
    params.put("From", "NNNNNNNNNN"); // Use your own value for the second parameter.
    params.put("To", "NNNNNNNNNN");   // Use your own value for the second parameter.
    params.put("Url", Url);

    // Create an instance of the CallFactory class.
    CallFactory callFactory = account.getCallFactory();

    // Make the call.
    Call call = callFactory.create(params);

Dodatne informacije o parametara koja u način **CallFactory.create** potražite u članku [http://www.twilio.com/docs/api/rest/making-calls][twilio_rest_making_calls].

Kao što je rečeno, kod koristi web-mjesta pod uvjetom Twilio da biste se vratili TwiML odgovor. Nije moguće umjesto toga koristite web-mjestu možete unijeti odgovora TwiML; Dodatne informacije potražite u članku [kako unijeti TwiML odgovora u aplikaciji Java na Azure](#howto_provide_twiml_responses).

## <a id="howto_send_sms"></a>Kako: slanje SMS poruka
Slijedi primjer kako slanje SMS poruke pomoću klase **SmsFactory** . Na **iz** broj, **4155992671**pruža Twilio za probno razdoblje račune za slanje SMS poruka. Broj **Da biste** se moraju provjeriti za vaš račun Twilio prije pokretanja kod.

    // Use your account SID and authentication token instead
    // of the placeholders shown here.
    String accountSID = "your_twilio_account";
    String authToken = "your_twilio_authentication_token";

    // Create an instance of the Twilio client.
    TwilioRestClient client;
    client = new TwilioRestClient(accountSID, authToken);

    // Retrieve the account, used later to create an instance of the SmsFactory.
    Account account = client.getAccount();

    // Send an SMS message.
    MessageFactory messageFactory = account.getMessageFactory();
    
    List<NameValuePair> params = new ArrayList<NameValuePair>();
    params.add(new BasicNameValuePair("To", "+14159352345")); // Replace with a valid phone number for your account.
    params.add(new BasicNameValuePair("From", "+14158141829")); // Replace with a valid phone number for your account.
    params.add(new BasicNameValuePair("Body", "Where's Wallace?"));
    
    Message sms = messageFactory.create(params);
        
Dodatne informacije o parametara koja u način **SmsFactory.create** potražite u članku [http://www.twilio.com/docs/api/rest/sending-sms][twilio_rest_sending_sms].

## <a id="howto_provide_twiml_responses"></a>Kako: Navedite TwiML odgovore od vlastito web-mjesto
Kada aplikacija pokreće poziv Twilio API, primjerice putem metodu **CallFactory.create** Twilio će poslati zahtjev URL koji se očekuje da biste se vratili TwiML odgovor. Gornji primjer koristi URL-ovi Twilio [http://twimlets.com/message][twimlet_message_url]. (Dok TwiML namijenjen korištenju Web Services, možete pogledati u TwiML u pregledniku. Na primjer, kliknite [http://twimlets.com/message] [ twimlet_message_url] da biste vidjeli prazna ** &lt;odgovor&gt; ** element; drugi primjer kliknite [http://twimlets.com/message?Message%5B0%5D=Hello%20World] [ twimlet_message_url_hello_world] da biste vidjeli na ** &lt;odgovor&gt; ** element koji sadrži na ** &lt;izgovorite&gt; ** element.)

Tipkanje URL-ovi Twilio možete stvoriti vlastitu web-mjesta URL koji vraća HTTP odgovora. Možete stvoriti web-mjesto u bilo koji jezik koji prikazuje odgovore HTTP; u ovoj se temi pretpostavlja da ćete hosting URL-a na stranici JSP.

Na sljedećoj stranici JSP rezultira TwiML odgovora koja vas obavještava da **Pozdrav svijetu** na poziv.

    <%@ page contentType="text/xml" %>
    <Response> 
        <Say>Hello World</Say>
    </Response>

Na sljedećoj stranici JSP rezultira TwiML odgovor koji vas obavještava da neki tekst, sadrži nekoliko stanke i piše informacije o verziji Twilio API-JA i naziv Azure uloge.


    <%@ page contentType="text/xml" %>
    <Response> 
        <Say>Hello from Azure</Say>
        <Pause></Pause>
        <Say>The Twilio API version is <%= request.getParameter("ApiVersion") %>.</Say>
        <Say>The Azure role name is <%= System.getenv("RoleName") %>.</Say>
        <Pause></Pause>
        <Say>Good bye.</Say>
    </Response>

Parametar **ApiVersion** dostupan je u Twilio govorne zahtjeva (ne i zahtjeve SMS). Da biste vidjeli parametara dostupnih zahtjev za Twilio glasa i zahtjevi za SMS potražite u članku <https://www.twilio.com/docs/api/twiml/twilio_request> i <https://www.twilio.com/docs/api/twiml/sms/twilio_request>, odnosno. Varijabla okruženja **Naziv uloge** je dostupan u sklopu Azure implementacije. (Ako želite dodati varijable prilagođene okruženja tako da ih nije moguće treba izdvojiti iz **System.getenv**, potražite u odjeljku varijable okruženja na [Razne postavke konfiguracije uloga][misc_role_config_settings].)

Nakon što dodate JSP stranici postavljen za pružaju TwiML odgovore, koristite URL stranice JSP kao URL ušli u način **CallFactory.create** . Na primjer, ako imate web-aplikacije pod nazivom MyTwiML implementiran na programa Azure hostira servisa, i naziv stranice JSP je mytwiml.jsp, URL-a mogu se proslijediti **CallFactory.create** kao što je prikazano u nastavku:

    // Place the call From, To and URL values into a hash map. 
    HashMap<String, String> params = new HashMap<String, String>();
    params.put("From", "NNNNNNNNNN");
    params.put("To", "NNNNNNNNNN");
    params.put("Url", "http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.jsp");

    CallFactory callFactory = account.getCallFactory();
    Call call = callFactory.create(params);

Druga mogućnost za reagirati TwiML je putem klase **TwiMLResponse** koji je dostupan u paketu **com.twilio.sdk.verbs** .

Dodatne informacije o korištenju Twilio u Azure s Java potražite u članku [kako Twilio pomoću za telefonski poziv u aplikaciji Java na Azure][howto_phonecall_java].

## <a id="AdditionalServices"></a>Kako: korištenje dodatnih Twilio usluga
Osim ovdje prikazani Primjeri, Twilio nudi utemeljen na webu API-ji koje možete koristiti odražava dodatne funkcije Twilio iz aplikacije za Azure. Sve pojedinosti potražite u [dokumentaciji Twilio API] [twilio_api_documentation].

## <a id="NextSteps"></a>Daljnji koraci
Sad kad ste naučili osnove servisa Twilio, slijedite ove veze na dodatne informacije:

* [Upute o sigurnosti Twilio] [twilio_security_guidelines]
* [Twilio Nemoderirana i kod uzorka] [twilio_howtos]
* [Vodiči za brzi početak rada Twilio][twilio_quickstarts] 
* [Twilio na GitHub] [twilio_on_github]
* [Obratite se podršci za Twilio] [twilio_support]

[twilio_java]: https://github.com/twilio/twilio-java
[twilio_api_service]: https://api.twilio.com
[add_ca_cert]: java-add-certificate-ca-store.md
[howto_phonecall_java]: partner-twilio-java-phone-call-example.md
[misc_role_config_settings]: http://msdn.microsoft.com/library/windowsazure/hh690945.aspx
[twimlet_message_url]: http://twimlets.com/message
[twimlet_message_url_hello_world]: http://twimlets.com/message?Message%5B0%5D=Hello%20World
[twilio_rest_making_calls]: http://www.twilio.com/docs/api/rest/making-calls
[twilio_rest_sending_sms]: http://www.twilio.com/docs/api/rest/sending-sms
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
