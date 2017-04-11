<properties 
    pageTitle="Kako pomoću servisa za e-pošte SendGrid (.NET) | Microsoft Azure" 
    description="Saznajte kako za slanje e-pošte sa servisom SendGrid e-pošte na Azure. Uzorci pisane C# kod i korištenje .NET API-JA." 
    services="app-service\web" 
    documentationCenter=".net" 
    authors="thinkingserious" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="01/14/2016" 
    ms.author="team-pi@sendgrid.com"/>





# <a name="how-to-send-email-using-sendgrid-with-azure"></a>Upute za slanje e-pošte pomoću SendGrid Azure


## <a name="overview"></a>Pregled

Ovaj vodič pokazuje kako izvoditi uobičajene zadatke programiranje sa servisom SendGrid e-pošte na Azure. Primjere zapisuju u C\#
i korištenje .NET API-JA. Scenariji u kojima je moguće uvrstiti **izgradnje e-pošte**, **Slanje e-pošte**, **Dodavanje privitka**i **pomoću filtara**. Dodatne informacije o SendGrid i slanje e-pošte potražite u odjeljku [sljedeće korake][] .

## <a name="what-is-the-sendgrid-email-service"></a>Što je SendGrid servis za e-pošte?

SendGrid je [servis za e-pošte utemeljene na oblaku] koji omogućuje pouzdanog [isporuke transakcijskih e-pošte], skalabilnost i u stvarnom vremenu analize uz fleksibilne API-ji koji olakšavaju integraciju prilagođene. Uobičajeni scenariji za korištenje SendGrid obuhvaćaju sljedeće:

-   Automatsko slanje potvrde klijentima.
-   Administriranje raspodjele popis za slanje klijentima mjesečnih e-fliers i posebne ponude.
-   Prikupljanje u stvarnom vremenu metriku elemente kao što su blokirane e-pošte i odziv klijenta.
-   Generiranje izvješća da biste lakše prepoznavanje trendova.
-   Prosljeđivanje anketa klijenta.
-   Obrada dolazne poruke e-pošte.

Dodatne informacije potražite u članku [https://sendgrid.com](https://sendgrid.com) ili naših [C# biblioteke][sendgrid csharp]

## <a name="create-a-sendgrid-account"></a>Stvaranje računa SendGrid

[AZURE.INCLUDE [sendgrid-sign-up](../../includes/sendgrid-sign-up.md)]

## <a name="reference-the-sendgrid-net-class-library"></a>Referenca Biblioteka klasa SendGrid .NET

[Paket SendGrid NuGet](https://www.nuget.org/packages/Sendgrid) je najjednostavniji način da biste dobili SendGrid API i da biste konfigurirali aplikaciju s sve ovisnosti. NuGet je dio sustava Microsoft Visual Studio 2015 koja olakšava instaliranje i ažuriranje biblioteke i Alati za proširenje za Visual Studio. 

> [AZURE.NOTE] Da biste instalirali NuGet ako koristite verziju programa Visual Studio starija od Visual Studio 2015, posjetite [http://www.nuget.org](http://www.nuget.org)pa kliknite gumb **Instalacija NuGet** .

Da biste instalirali paket SendGrid NuGet u aplikaciji, učinite sljedeće:

1.  Stvara novi projekt.

    ![Stvaranje novog projekta][create-new-project]

2.  Odaberite predložak.

    ![Odabir predloška][select-a-template]

3.  U **Pregledniku rješenja**, desnom tipkom miša kliknite **reference**, a zatim kliknite **Upravljanje NuGet paketa**.

4.  Potražite **SendGrid** i odaberite **SendGrid** stavku na popisu rezultata.

    ![SendGrid NuGet paketa][SendGrid-NuGet-package]

5.  Kliknite da biste dovršili instalaciju **instalirati** , a zatim zatvorite u ovom dijaloškom okviru.

Biblioteka klasa .NET SendGrid na zove **SendGridMail**. Sadrži sljedeće prostori:

-   **SendGridMail** za stvaranje i rad s e-pošte stavki.
-   **SendGridMail.Transport** za slanje e-pošte pomoću protokola **SMTP** ili protokol HTTP 1.1 s **Web-OSTALE**.

Dodajte sljedeće deklaracija prostor naziva kod vrh sve C\# datoteku u kojoj želite li programatski pristup servisu SendGrid e-pošte.
**System.Net** i **System.Net.Mail** se prostori .NET Framework uključene su jer je to su vrste obično ćete koristiti s API-ji SendGrid.

    using System;
    using System.Net;
    using System.Net.Mail;
    using SendGrid;

## <a name="how-to-create-an-email"></a>Kako: Stvaranje poruke e-pošte

Pomoću objekta **SendGridMessage** da biste stvorili poruku e-pošte. Nakon stvaranja objekta poruke, možete postaviti svojstva i načine, uključujući e-pošte pošiljatelja, primatelja e-pošte i predmet i tijelo e-pošte.

Sljedeći primjer pokazuje kako stvoriti potpuno popunjena e-pošte objekta:

    // Create the email object first, then add the properties.
    var myMessage = new SendGridMessage();

    // Add the message properties.
    myMessage.From = new MailAddress("john@example.com");

    // Add multiple addresses to the To field.
    List<String> recipients = new List<String>
    {
        @"Jeff Smith <jeff@example.com>",
        @"Anna Lidman <anna@example.com>",
        @"Peter Saddow <peter@example.com>"
    };

    myMessage.AddTo(recipients);

    myMessage.Subject = "Testing the SendGrid Library";

    //Add the HTML and Text bodies
    myMessage.Html = "<p>Hello World!</p>";
    myMessage.Text = "Hello World plain text!";

Dodatne informacije o svim svojstva i metode vrsta **SendGrid** podržava potražite u članku [sendgrid csharp][] na GitHub.

## <a name="how-to-send-an-email"></a>Kako: slanje poruke e-pošte

Kada stvorite poruku e-pošte, možete poslati pomoću API Web nudi SendGrid. Osim toga, možda koristite [. Neto-ugrađen u biblioteci](https://sendgrid.com/docs/Code_Examples/csharp.html).

Slanje e-pošte zahtijeva da unesete vjerodajnice računa SendGrid (korisničko ime i lozinka) ili SendGrid ključ za API-JA. Ključ za API je željeni način. Ako vam je potrebna detalje o konfiguriranju API tipke, posjetite naš [dokumentacija](https://sendgrid.com/docs/Classroom/Send/api_keys.html)

Te vjerodajnice može spremiti putem portala za Azure tako da kliknete KONFIGURIRAJ i dodavanje parove ključa vrijednosti u odjeljku "postavki aplikacije".

 ![Postavki Azure aplikacije][azure_app_settings]

 Nakon toga biste možda im pristupati na sljedeći način: 
    
    var username = System.Environment.GetEnvironmentVariable("SENDGRID_USERNAME"); 
    var pswd = System.Environment.GetEnvironmentVariable("SENDGRID_PASSWORD");
    var apiKey = System.Environment.GetEnvironmentVariable("SENDGRID_APIKEY");

Pomoću vjerodajnica:
    
    // Create network credentials to access your SendGrid account
    var username = "your_sendgrid_username";
    var pswd = "your_sendgrid_password";

    var credentials = new NetworkCredential(username, pswd);
    // Create an Web transport for sending email.
    var transportWeb = new Web(credentials);

Korištenje ključa API-JA:

    var apiKey = "your_sendgrid_api_key";  
    // create a Web transport, using API Key
    var transportWeb = new Web(apiKey);


Sljedeći primjeri pokazuju slanju poruke pomoću Web API.

    // Create the email object first, then add the properties.
    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    // Create credentials, specifying your user name and password.
    var credentials = new NetworkCredential("username", "password");

    // Create an Web transport for sending email.
    var transportWeb = new Web(credentials);

    // Send the email, which returns an awaitable task.
    transportWeb.DeliverAsync(myMessage);

    // If developing a Console Application, use the following
    // transportWeb.DeliverAsync(mail).Wait();

## <a name="how-to-add-an-attachment"></a>Kako: Dodavanje privitka

Privitke možete dodati u poruku pozivanje metodu **AddAttachment** i navođenjem naziva i puta do datoteke koju želite priložiti.
Možete uključiti većeg broja privitaka tako da nazovete ovu metodu nakon što za svaku datoteku koju želite priložiti. Sljedeći primjer prikazuje Dodavanje privitka u poruku:

    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    myMessage.AddAttachment(@"C:\file1.txt");
    
Privitke možete dodati i iz podataka **strujanje**. To možete učiniti tako da nazovete isti način kao iznad **AddAttachment**, ali po Prenos u toka podataka, a naziv datoteke koji ga želite prikazati kao poruku. U ovom slučaju morate dodati System.IO biblioteke.

    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    using (var attachmentFileStream = new FileStream(@"C:\file.txt", FileMode.Open))
    {
        myMessage.AddAttachment(attachmentFileStream, "My Cool File.txt");
    }


## <a name="how-to-use-apps-to-enable-footers-tracking-and-analytics"></a>Kako: korištenje aplikacija da biste omogućili podnožja, praćenje i analize

SendGrid nudi funkcije dodatne e-pošte pomoću aplikacije. Ovo su postavke koje se može dodati u poruku e-pošte da biste omogućili određene funkcije kao što su kliknite praćenje, Google analytics pretplate praćenje i tako dalje. Cijeli popis aplikacija potražite u članku [Postavki aplikacije][].

Aplikacija primjenjuje se na poruke e-pošte **SendGrid** pomoću metode kao dio klase **SendGrid** .

Sljedeći primjeri demonstrirati podnožja i kliknite praćenje filtre:

### <a name="footer"></a>Podnožje

    // Create the email object first, then add the properties.
    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    // Add a footer to the message.
    myMessage.EnableFooter("PLAIN TEXT FOOTER", "<p><em>HTML FOOTER</em></p>");

### <a name="click-tracking"></a>Kliknite evidentiranje

    // Create the email object first, then add the properties.
    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Html = "<p><a href=\"http://www.example.com\">Hello World Link!</a></p>";
    myMessage.Text = "Hello World!";
    
    // true indicates that links in plain text portions of the email 
    // should also be overwritten for link tracking purposes. 
    myMessage.EnableClickTracking(true);

## <a name="how-to-use-additional-sendgrid-services"></a>Kako: koristite dodatne SendGrid usluge

SendGrid nudi API-ji i webhooks koje možete koristiti odražava dodatne funkcije SendGrid iz aplikacije za Azure temeljenih na webu. Sve pojedinosti potražite u [dokumentaciji SendGrid API -JA][].

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste naučili osnove servisa SendGrid e-poštu, slijedite ove veze da biste saznali više.

*   SendGrid C\# repo biblioteke: [sendgrid csharp][]
*   Dokumentacija SendGrid API: <https://sendgrid.com/docs>
*   SendGrid posebnu ponudu za klijente Azure: [https://sendgrid.com](https://sendgrid.com)

  [Daljnji koraci]: #next-steps
  [What is the SendGrid Email Service?]: #whatis
  [Create a SendGrid Account]: #createaccount
  [Reference the SendGrid .NET Class Library]: #reference
  [How to: Create an Email]: #createemail
  [How to: Send an Email]: #sendemail
  [How to: Add an Attachment]: #addattachment
  [How to: Use Filters to Enable Footers, Tracking, and Analytics]: #usefilters
  [How to: Use Additional SendGrid Services]: #useservices
  
  [special offer]: https://www.sendgrid.com/windowsazure.html
  
  [create-new-project]: ./media/sendgrid-dotnet-how-to-send-email/create_new_project.png
  [select-a-template]: ./media/sendgrid-dotnet-how-to-send-email/select_a_template.png
  [SendGrid-NuGet-package]: ./media/sendgrid-dotnet-how-to-send-email/sendgrid_nuget.png
  [azure_app_settings]: ./media/sendgrid-dotnet-how-to-send-email/app_settings.png
  [sendgrid csharp]: https://github.com/sendgrid/sendgrid-csharp
  [SMTP vs. Web API]: https://sendgrid.com/docs/Integrate/index.html
  [Postavke za aplikaciju]: https://sendgrid.com/docs/API_Reference/SMTP_API/apps.html
  [Dokumentacija SendGrid API-JA]: https://sendgrid.com/docs
  
  [servis u oblaku e-pošte]: https://sendgrid.com/email-solutions
  [isporuke transakcijskih e-pošte]: https://sendgrid.com/transactional-email
 
