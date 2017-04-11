<properties 
    pageTitle="Kako koristiti SendGrid servis za e-pošte (i) | Microsoft Azure" 
    description="Saznajte kako za slanje e-pošte sa servisom SendGrid e-pošte na Azure. Primjere koda napisan i." 
    documentationCenter="php" 
    services="" 
    manager="sendgrid" 
    editor="mollybos" 
    authors="thinkingserious"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="PHP" 
    ms.topic="article" 
    ms.date="10/30/2014" 
    ms.author="elmer.thomas@sendgrid.com; erika.berkland@sendgrid.com; vibhork; matt.bernier@sendgrid.com"/>
# <a name="how-to-use-the-sendgrid-email-service-from-php"></a>Upute za korištenje servisa e-pošte SendGrid i

Ovaj vodič pokazuje kako izvoditi uobičajene zadatke programiranje sa servisom SendGrid e-pošte na Azure. Primjere zapisuju u PHP.
Scenariji u kojima je moguće uvrstiti **izgradnje e-pošte**, **Slanje e-pošte**i **Dodavanje privitka**. Dodatne informacije o SendGrid i slanje e-pošte potražite u odjeljku [Sljedeće korake](#next-steps) .

## <a name="what-is-the-sendgrid-email-service"></a>Što je SendGrid servis za e-pošte?

SendGrid je [servis za e-pošte utemeljene na oblaku] koji omogućuje pouzdanog [isporuke transakcijskih e-pošte], skalabilnost i u stvarnom vremenu analize uz fleksibilne API-ji koji olakšavaju integraciju prilagođene. Uobičajeni scenariji za korištenje SendGrid obuhvaćaju sljedeće:

-   Automatski slanju potvrda klijentima
-   Administriranje raspodjele popisi za slanje klijentima mjesečnih e-fliers i posebne ponude
-   Prikupljanje u stvarnom vremenu metriku za elemente kao što su blokirane e-pošte i reaktivnosti klijenta
-   Generiranje izvješća da biste lakše prepoznavanje trendova
-   Prosljeđivanje anketa klijenta
- Obavijesti e-poštom iz aplikacije

Dodatne informacije potražite u članku [https://sendgrid.com][].

## <a name="create-a-sendgrid-account"></a>Stvaranje računa SendGrid

[AZURE.INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="using-sendgrid-from-your-php-application"></a>Korištenje SendGrid iz aplikacije i

Korištenje SendGrid u aplikaciji i Azure potrebno ne posebni konfiguraciju ili kod. Budući da SendGrid uslugu, ga možete pristupiti na isti način kao i iz aplikacije oblak kao što možete iz lokalnog aplikacije.

## <a name="how-to-send-an-email"></a>Kako: slanje poruke e-pošte

Možete poslati e-pošte pomoću SMTP ili API Web nudi SendGrid.

### <a name="smtp-api"></a>SMTP API-JA

Da biste poslali e-pošte pomoću SendGrid SMTP API, koristite *Swift pošiljatelja*, koji se temelji na komponenti biblioteke za slanje poruke e-pošte iz aplikacija i. *Slanje e-pošte Swift* biblioteke možete preuzeti iz [http://swiftmailer.org/download][] v5.3.0 (koristite [Skladatelj] da biste instalirali Swift slanje e-pošte). Slanje e-pošte s bibliotekom tiče stvaranje pojave na <span class="auto-style2">Swift\_SmtpTransport</span>, <span class="auto-style2">Swift\_slanje e-pošte</span>, i <span class="auto-style2">Swift\_poruke</span> klase, postavljanje odgovarajuća svojstva i pozivanje na <span class="auto-style2">Swift\_Mailer::send</span> način.

    <?php
     include_once "vendor/autoload.php";
     /*
      * Create the body of the message (a plain-text and an HTML version).
      * $text is your plain-text email
      * $html is your html version of the email
      * If the reciever is able to view html emails then only the html
      * email will be displayed
      */ 
     $text = "Hi!\nHow are you?\n";
     $html = "<html>
           <head></head>
           <body>
               <p>Hi!<br>
                   How are you?<br>
               </p>
           </body>
           </html>";
     // This is your From email address
     $from = array('someone@example.com' => 'Name To Appear');
     // Email recipients
     $to = array(
           'john@contoso.com'=>'Destination 1 Name',
           'anna@contoso.com'=>'Destination 2 Name'
     );
     // Email subject
     $subject = 'Example PHP Email';

     // Login credentials
     $username = 'yoursendgridusername';
     $password = 'yourpassword';
     
     // Setup Swift mailer parameters
     $transport = Swift_SmtpTransport::newInstance('smtp.sendgrid.net', 587);
     $transport->setUsername($username);
     $transport->setPassword($password);
     $swift = Swift_Mailer::newInstance($transport);

     // Create a message (subject)
     $message = new Swift_Message($subject);

     // attach the body of the email
     $message->setFrom($from);
     $message->setBody($html, 'text/html');
     $message->setTo($to);
     $message->addPart($text, 'text/plain');
     
     // send message 
     if ($recipients = $swift->send($message, $failures))
     {
         // This will let us know how many users received this message
         echo 'Message sent out to '.$recipients.' users';
     }
     // something went wrong =(
     else
     {
         echo "Something went wrong - ";
         print_r($failures);
     }

### <a name="web-api"></a>API na webu

Pomoću PHP, [curl funkcija][] slanje e-pošte SendGrid Web API-JA.

    <?php

     $url = 'https://api.sendgrid.com/';
     $user = 'USERNAME';
     $pass = 'PASSWORD'; 

     $params = array(
          'api_user' => $user,
          'api_key' => $pass,
          'to' => 'john@contoso.com',
          'subject' => 'testing from curl',
          'html' => 'testing body',
          'text' => 'testing body',
          'from' => 'anna@contoso.com',
       );
       
     $request = $url.'api/mail.send.json';
     
     // Generate curl request
     $session = curl_init($request);
     
     // Tell curl to use HTTP POST
     curl_setopt ($session, CURLOPT_POST, true);
     
     // Tell curl that this is the body of the POST
     curl_setopt ($session, CURLOPT_POSTFIELDS, $params);
     
     // Tell curl not to return headers, but do return the response
     curl_setopt($session, CURLOPT_HEADER, false);
     curl_setopt($session, CURLOPT_RETURNTRANSFER, true);
     
     // obtain response
     $response = curl_exec($session);
     curl_close($session);
     
     // print everything out
     print_r($response);

API-SendGrid Web vrlo je sličan REST API-JA iako nije zaista biti RESTful API jer u većini poziva, oba se i objavu glagoli naizmjence može se koristiti.

## <a name="how-to-add-an-attachment"></a>Kako: Dodavanje privitka

### <a name="smtp-api"></a>SMTP API-JA

Slanje privitka pomoću SMTP API uključuje jedan dodatni redak koda u primjeru skriptu za slanje poruke e-pošte s Swift pošiljatelja.

    <?php
     include_once "vendor/autoload.php";
     /*
      * Create the body of the message (a plain-text and an HTML version).
      * $text is your plain-text email
      * $html is your html version of the email
      * If the reciever is able to view html emails then only the html
      * email will be displayed
      */
     $text = "Hi!\nHow are you?\n";
      $html = "<html>
          <head></head>
          <body>
             <p>Hi!<br>
                How are you?<br>
             </p>
          </body>
          </html>";

     // This is your From email address
     $from = array('someone@example.com' => 'Name To Appear');
     
     // Email recipients
     $to = array(
          'john@contoso.com'=>'Destination 1 Name',
          'anna@contoso.com'=>'Destination 2 Name'
     );
     // Email subject
     $subject = 'Example PHP Email';
     
     // Login credentials
     $username = 'yoursendgridusername';
     $password = 'yourpassword';
     
     // Setup Swift mailer parameters
     $transport = Swift_SmtpTransport::newInstance('smtp.sendgrid.net', 587);
     $transport->setUsername($username);
     $transport->setPassword($password);
     $swift = Swift_Mailer::newInstance($transport);
     
     // Create a message (subject)
     $message = new Swift_Message($subject);
     
     // attach the body of the email
     $message->setFrom($from);
     $message->setBody($html, 'text/html');
     $message->setTo($to);
     $message->addPart($text, 'text/plain');
     $message->attach(Swift_Attachment::fromPath("path\to\file")->setFileName("file_name"));
     
     // send message 
     if ($recipients = $swift->send($message, $failures))
     {
          // This will let us know how many users received this message
          echo 'Message sent out to '.$recipients.' users';
     }
     // something went wrong =(
     else
     {
          echo "Something went wrong - ";
          print_r($failures);
     }

Dodatni redak kod je na sljedeći način:

     $message->attach(Swift_Attachment::fromPath("path\to\file")->setFileName('file_name'));

Ovu liniju koda poziva prilaganje način na na <span class="auto-style2">Swift\_poruke</span> objekt, a zatim koristi statički način <span class="auto-style2">fromPath</span> na na <span class="auto-style2">Swift\_privitak</span> klase za dohvat i prilaganje datoteke poruci.

### <a name="web-api"></a>API na webu

Slanje privitka pomoću Web API vrlo je sličan slanje poruke e-pošte pomoću Web API. Međutim, imajte na umu da u primjeru koji se nalazi iza polja parametar mora sadržavati taj element:

    'files['.$fileName.']' => '@'.$filePath.'/'.$fileName

Primjer:

    <?php

     $url = 'https://api.sendgrid.com/';
     $user = 'USERNAME';
     $pass = 'PASSWORD';
     
     $fileName = 'myfile';
     $filePath = dirname(__FILE__);

     $params = array(
         'api_user' => $user,
         'api_key' => $pass,
         'to' =>'john@contoso.com',
         'subject' => 'test of file sends',
         'html' => '<p> the HTML </p>',
         'text' => 'the plain text',
         'from' => 'anna@contoso.com',
         'files['.$fileName.']' => '@'.$filePath.'/'.$fileName
     );
     
     print_r($params);
     
     $request = $url.'api/mail.send.json';
     
     // Generate curl request
     $session = curl_init($request);
     
     // Tell curl to use HTTP POST
     curl_setopt ($session, CURLOPT_POST, true);
     
     // Tell curl that this is the body of the POST
     curl_setopt ($session, CURLOPT_POSTFIELDS, $params);
     
     // Tell curl not to return headers, but do return the response
     curl_setopt($session, CURLOPT_HEADER, false);
     curl_setopt($session, CURLOPT_RETURNTRANSFER, true);
     
     // obtain response
     $response = curl_exec($session);
     curl_close($session);
     
     // print everything out
     print_r($response);

## <a name="how-to-use-filters-to-enable-footers-tracking-and-analytics"></a>Kako: pomoću filtara radi omogućivanja podnožja, praćenje i analize

SendGrid nudi funkcije e-pošte dodatnim pomoću "filtri". Ovo su postavke koje se može dodati u poruku e-pošte da biste omogućili određene funkcije, primjerice omogućite kliknite praćenje, Google analytics, pretplatu za praćenje i tako dalje.

Filtri se mogu primijeniti na poruku pomoću filtara svojstava. Svaki filtar određen raspršivanje koja sadrži postavke specifične za filtar. U sljedećem primjeru omogućuje filtar podnožja i određuje tekstnu poruku koja će biti dodana na dno poruke e-pošte.
U ovom primjeru koristit ćemo [sendgrid i biblioteke].
Da biste instalirali biblioteke pomoću [Skladatelj] :
    
    php composer.phar require sendgrid/sendgrid 2.1.1

Primjer:    

    <?php
     /*
      * This example is used for sendgrid-php V2.1.1 (https://github.com/sendgrid/sendgrid-php/tree/v2.1.1)
      */
     include "vendor/autoload.php";

     $email = new SendGrid\Email();
     // The list of addresses this message will be sent to
     // [This list is used for sending multiple emails using just ONE request to SendGrid]
     $toList = array('john@contoso.com', 'anna@contoso.com');

     // Specify the names of the recipients
     $nameList = array('Name 1', 'Name 2');

     // Used as an example of variable substitution
     $timeList = array('4 PM', '5 PM');

     // Set all of the above variables
     $email->setTos($toList);
     $email->addSubstitution('-name-', $nameList);
     $email->addSubstitution('-time-', $timeList);

     // Specify that this is an initial contact message
     $email->addCategory("initial");

     // You can optionally setup individual filters here, in this example, we have 
     // enabled the footer filter
     $email->addFilter('footer', 'enable', 1);
     $email->addFilter('footer', "text/plain", "Thank you for your business");
     $email->addFilter('footer', "text/html", "Thank you for your business");

     // The subject of your email
     $subject = 'Example SendGrid Email';

     // Where is this message coming from. For example, this message can be from 
     // support@yourcompany.com, info@yourcompany.com
     $from = 'someone@example.com';

     // If you do not specify a sender list above, you can specifiy the user here. If 
     // a sender list IS specified above, this email address becomes irrelevant.
     $to = 'john@contoso.com';

     # Create the body of the message (a plain-text and an HTML version). 
     # text is your plain-text email 
     # html is your html version of the email
     # if the receiver is able to view html emails then only the html
     # email will be displayed

     /*
      * Note the variable substitution here =)
      */
     $text = "
     Hello -name-,
     Thank you for your interest in our products. We have set up an appointment to call you at -time- EST to discuss your needs in more detail.
     Regards,
     Fred";

     $html = "
     <html> 
     <head></head>
     <body>
     <p>Hello -name-,<br>
     Thank you for your interest in our products. We have set up an appointment
     to call you at -time- EST to discuss your needs in more detail.

     Regards,

     Fred<br>
     </p>
     </body>
     </html>";

     // set subject
     $email->setSubject($subject);

     // attach the body of the email
     $email->setFrom($from);
     $email->setHtml($html);
     $email->addTo($to);
     $email->setText($text);

     // Your SendGrid account credentials
     $username = 'sendgridusername@yourdomain.com';
     $password = 'example';

     // Create SendGrid object
     $sendgrid = new SendGrid($username, $password);

     // send message
     $response = $sendgrid->send($email);

     print_r($response);

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste naučili osnove servisa SendGrid e-poštu, slijedite ove veze da biste saznali više.

-   Dokumentacija SendGrid: <https://sendgrid.com/docs>
-   Biblioteka SendGrid i: <https://github.com/sendgrid/sendgrid-php>
-   SendGrid posebnu ponudu za klijente Azure: <https://sendgrid.com/windowsazure.html>

Dodatne informacije potražite u odjeljku [Centar za razvojne inženjere i](/develop/php/).


  [https://sendgrid.com]: https://sendgrid.com
  [https://sendgrid.com/transactional-email/pricing]: https://sendgrid.com/transactional-email/pricing
  [special offer]: https://www.sendgrid.com/windowsazure.html
  [Packaging and Deploying PHP Applications for Azure]: http://msdn.microsoft.com/library/windowsazure/hh674499(v=VS.103).aspx
  [http://swiftmailer.org/Download]: http://swiftmailer.org/download
  [Funkcija zakretanja]: http://php.net/curl
  [servis u oblaku e-pošte]: https://sendgrid.com/email-solutions
  [isporuke transakcijskih e-pošte]: https://sendgrid.com/transactional-email
  [sendgrid i biblioteke]: https://github.com/sendgrid/sendgrid-php/tree/v2.1.1
  [Skladatelj]: https://getcomposer.org/download/
