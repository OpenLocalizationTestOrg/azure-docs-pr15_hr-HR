<properties 
    pageTitle="Upute za upućivanje telefonskog poziva s Twilio (Java) | Microsoft Azure" 
    description="Saznajte kako nazvati s web-stranice pomoću Twilio u aplikaciji Java na Azure." 
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

# <a name="how-to-make-a-phone-call-using-twilio-in-a-java-application-on-azure"></a>Upute za upućivanje telefonskog poziva pomoću Twilio u aplikaciji Java na Azure 

Sljedeći primjer pokazuje kako koristiti Twilio upućivanje poziva s web-stranice smješten u Azure. Rezultat aplikacije tražit će od korisnika nazvati vrijednosti, kao što je prikazano na sljedećem zaslonu snimka.

![Obrazac za Azure poziv pomoću Twilio i Java][twilio_java]

Morat ćete biste pomoću koda u ovoj temi, učinite sljedeće:

1. Nabava Twilio računa i provjera autentičnosti tokena. Za početak rada s Twilio, procijeniti cijene pri [http://www.twilio.com/pricing][twilio_pricing]. Možete se prijaviti na [https://www.twilio.com/try-twilio][try_twilio]. Informacije o API nudi Twilio potražite u članku [http://www.twilio.com/api][twilio_api].
2. Nabavite Twilio POSUDU. Pri [https://github.com/twilio/twilio-java][twilio_java_github], možete preuzeti GitHub izvora i stvoriti vlastiti POSUDU ili preuzimanje ugrađene POSUDU (sa ili bez ovisnosti).
Kod u ovoj temi napisan pomoću gotove POSUDU TwilioJava 3.3.8 s ovisnosti.
3. Dodajte POSUDU vašem putu Java Sastavi.
4. Ako koristite Eclipse da biste stvorili ovu aplikaciju Java u datoteku aplikacije implementacije (WAR) pomoću značajke za sastavljanje Eclipse na implementaciju uključili POSUDU Twilio. Ako se ne koriste Eclipse da biste stvorili ovu aplikaciju Java, Twilio POSUDU provjerite je li uključene unutar iste Azure uloga kao Java aplikacije, te dodati na put predmete aplikacije.
5. Provjerite je li vaša keystore cacerts sadrži certifikat Equifax sigurne ustanova za izdavanje certifikata s MD5 otiska prsta 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (serijski broj je 35:DE:F4:CF, a SHA1 otisak D2:32:09:AD:23:D3:14:23:21:74:E4:0 D: 7F:9 D: 62:13:97:86:63:3A). Certifikat za izdavanje certifikata (CA) certifikat za [https://api.twilio.com] [ twilio_api_service] servis, koji se naziva prilikom korištenja API-ji Twilio. Informacije o dodavanju ovog ustanove za Izdavanje certifikata na JDK cacert spremište potražite u odjeljku [Dodavanje certifikat za pohranu certifikat za Java CA][add_ca_cert].

Uz to, poznavanje s podacima pri [stvaranju pozdrav svijeta aplikacije pomoću komplet alata za Azure za Eclipse][azure_java_eclipse_hello_world], ili pomoću drugih tehnika za hostiranje Java aplikacije u Azure ako se ne koriste Eclipse se preporučuje.

## <a name="create-a-web-form-for-making-a-call"></a>Stvaranje web-obrasca za upućivanje poziva

Sljedeći kod prikazuje kako stvoriti web-obrasca za dohvaćanje korisničkih podataka za upućivanje poziva. Za potrebe ovog primjera, stvaranja nove dinamičkog web projekta, pod nazivom **TwilioCloud**i **callform.jsp** dodan je kao datoteku JSP.

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Automated call form</title>
    </head>
    <body>
     <p>Fill in all fields and click <b>Make this call</b>.</p>
     <br/>
      <form action="makecall.jsp" method="post">
       <table>
         <tr>
           <td>To:</td>
           <td><input type="text" size=50 name="callTo" value="" />
           </td>
         </tr>
         <tr>
           <td>From:</td>
           <td><input type="text" size=50 name="callFrom" value="" />
           </td>
         </tr>
         <tr>
           <td>Call message:</td>
           <td><input type="text" size=400 name="callText" value="Hello. This is the call text. Good bye." />
           </td>
         </tr>
         <tr>
           <td colspan=2><input type="submit" value="Make this call" />
           </td>
         </tr>
       </table>
     </form>
     <br/>
    </body>
    </html>

## <a name="create-the-code-to-make-the-call"></a>Stvaranje kod da biste poziv
Sljedeći kod, koja se naziva i kada korisnik dovrši obrazac koji se prikazuje callform.jsp, stvara poruku poziva i generira poziv. Za potrebe ovog primjera, datoteka JSP pod nazivom **makecall.jsp** i dodan **TwilioCloud** projekt. (Koristi Twilio računa i provjera autentičnosti token umjesto vrijednosti rezervirano mjesto dodijeljene **accountSID** i **authToken** u kodu niže).

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    import="java.util.*"
    import="com.twilio.*"
    import="com.twilio.sdk.*"
    import="com.twilio.sdk.resource.factory.*"
    import="com.twilio.sdk.resource.instance.*"
    pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Call processing happens here</title>
    </head>
    <body>
        <b>This is my make call page.</b><p/>
     <%
    try 
    {
         // Use your account SID and authentication token instead
         // of the placeholders shown here.
         String accountSID = "your_twilio_account";
         String authToken = "your_twilio_authentication_token";
     
         // Instantiate an instance of the Twilio client.     
         TwilioRestClient client;
         client = new TwilioRestClient(accountSID, authToken);

         // Retrieve the account, used later to retrieve the CallFactory.
         Account account = client.getAccount();

         // Display the client endpoint. 
         out.println("<p>Using Twilio endpoint " + client.getEndpoint() + ".</p>");
     
         // Display the API version.
         String APIVERSION = TwilioRestClient.DEFAULT_VERSION;
         out.println("<p>Twilio client API version is " + APIVERSION + ".</p>");
    
         // Retrieve the values entered by the user.
         String callTo = request.getParameter("callTo");  
         // The Outgoing Caller ID, used for the From parameter,
         // must have previously been verified with Twilio.
         String callFrom = request.getParameter("callFrom");
         String userText = request.getParameter("callText");
     
         // Replace spaces in the user's text with '%20', 
         // to make the text suitable for a URL.
         userText = userText.replace(" ", "%20");
     
         // Create a URL using the Twilio message and the user-entered text.
         String Url="http://twimlets.com/message";
         Url = Url + "?Message%5B0%5D=" + userText;
     
         // Display the message URL.
         out.println("<p>");
         out.println("The URL is " + Url);
         out.println("</p>");
    
         // Place the call From, To and URL values into a hash map. 
         HashMap<String, String> params = new HashMap<String, String>();
         params.put("From", callFrom);
         params.put("To", callTo);
         params.put("Url", Url);
     
         CallFactory callFactory = account.getCallFactory();
         Call call = callFactory.create(params);
         out.println("<p>Call status: " + call.getStatus()  + "</p>"); 
    } 
    catch (TwilioRestException e) 
    {
        out.println("<p>TwilioRestException encountered: " + e.getMessage() + "</p>");
        out.println("<p>StackTrace: " + e.getStackTrace().toString() + "</p>");
    }
    catch (Exception e) 
    {
        out.println("<p>Exception encountered: " + e.getMessage() + "");
        out.println("<p>StackTrace: " + e.getStackTrace().toString() + "</p>");
    }
    %>
    </body>
    </html>

Osim upućivanje poziva pomoću makecall.jsp prikazuje krajnju točku Twilio, API verzija i status poziv. Primjer je snimka zaslona za sljedeće:

![Odgovor Azure poziv pomoću Twilio i Java][twilio_java_response]

## <a name="run-the-application"></a>Pokrenite aplikaciju
Slijede više razine korake da biste pokrenuli aplikaciju; detalje o za ove korake možete pronaći na [Stvaranje pozdrav svijeta aplikacije pomoću alata za Azure za Eclipse][azure_java_eclipse_hello_world].

1. Izvezite vaše WAR TwilioCloud Azure **approot** mapu. 
2. Izmjena **startup.cmd** raspakiraj vaše WAR TwilioCloud.
3. Kompiliranje aplikacija za emulator računalnim.
4. Pokrenite implementaciju sustava u emulator računalnim.
5. Otvorite preglednik i pokrenite **http://localhost:8080/TwilioCloud/callform.jsp**.
6. Unesite vrijednosti u obliku kliknite **upućivanje ovog poziva**, a zatim pogledajte rezultate u makecall.jsp.

Kada ste spremni za implementaciju Azure, recompile za implementaciju u oblak implementacija Azure i pokrenite http://*your_hosted_name*.cloudapp.net/TwilioCloud/callform.jsp u pregledniku (zamjenske vrijednosti za *your_hosted_name*).

## <a name="next-steps"></a>Daljnji koraci
Da bi se prikazala osnovne funkcije pomoću Twilio u Java Azure nije naveden kod. Prije no što implementirate Azure u proizvodnje, preporučujemo vam da biste dodali više pogreškama ili druge značajke. Ako, na primjer:

* Umjesto korištenja web-obrasca, može koristiti Azure prostora za pohranu blob-ova ili SQL baze podataka za pohranu telefonske brojeve i tekst poziva. Informacije o korištenju Azure prostora za pohranu blob-ova u Java potražite u članku [kako pomoću servisa za pohranu servisa Blob Java][howto_blob_storage_java]. Informacije o korištenju baze podataka SQL u Java potražite u članku [Pomoću SQL baze podataka u Java][howto_sql_azure_java].
* Možete koristiti **RoleEnvironment.getConfigurationSettings** za dohvaćanje ID računa Twilio i provjeru autentičnosti tokena iz postavki konfiguriranje implementaciju sustava, umjesto kodiranje vrijednosti u makecall.jsp. Informacije o klase **RoleEnvironment** potražite u članku [koristi biblioteka za izvođenje servisa Azure u JSP] [ azure_runtime_jsp] i u dokumentaciji o paketa izvođenje servisa Azure na [http://dl.windowsazure.com/javadoc][azure_javadoc].
* Kod makecall.jsp dodjeljuje pod uvjetom Twilio URL-a, [http://twimlets.com/message][twimlet_message_url], **Url** varijabli. U ovom URL-a nudi odgovor Twilio Markup Language (TwiML) koja obavještava Twilio upute da biste nastavili s pozivom. Na primjer, mogu sadržavati TwiML koja se vraća na ** &lt;izgovorite&gt; ** glagolski čiji je rezultat tekst Izgovor poziva primatelju. Umjesto korištenja URL-ovi Twilio nije sastavljanje vlastite servisa odgovoriti na zahtjev na Twilio; Dodatne informacije potražite [u]članku korištenje Twilio za glas i mogućnosti SMS u Java[howto_twilio_voice_sms_java]. Dodatne informacije o TwiML nalazi se na [http://www.twilio.com/docs/api/twiml][twiml], a dodatne informacije o ** &lt;izgovorite&gt; ** i druge glagoli Twilio nalazi se na [http://www.twilio.com/docs/api/twiml/say][twilio_say].
* Pročitajte upute o sigurnosti Twilio pri [https://www.twilio.com/docs/security][twilio_docs_security].

Dodatne informacije o Twilio potražite u članku [https://www.twilio.com/docs][twilio_docs].

## <a name="see-also"></a>Vidi također
* [Kako koristiti Twilio za glas i mogućnosti SMS u Java][howto_twilio_voice_sms_java]
* [Dodavanje certifikat u spremište Java ustanove za Izdavanje certifikata][add_ca_cert]

[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/api
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[twilio_java_github]: http://github.com/twilio/twilio-java
[twimlet_message_url]: http://twimlets.com/message
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api_service]: http://api.twilio.com
[add_ca_cert]: java-add-certificate-ca-store.md
[azure_java_eclipse_hello_world]: http://msdn.microsoft.com/library/windowsazure/hh690944.aspx
[howto_twilio_voice_sms_java]: partner-twilio-java-how-to-use-voice-sms.md
[howto_blob_storage_java]: http://www.windowsazure.com/develop/java/how-to-guides/blob-storage/
[howto_sql_azure_java]: http://msdn.microsoft.com/library/windowsazure/hh749029.aspx
[azure_runtime_jsp]: http://msdn.microsoft.com/library/windowsazure/hh690948.aspx
[azure_javadoc]: http://dl.windowsazure.com/javadoc
[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say
[twilio_java]: ./media/partner-twilio-java-phone-call-example/WA_TwilioJavaCallForm.jpg
[twilio_java_response]: ./media/partner-twilio-java-phone-call-example/WA_TwilioJavaMakeCall.jpg
