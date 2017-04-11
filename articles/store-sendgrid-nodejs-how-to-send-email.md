<properties 
    pageTitle="Kako pomoću servisa za e-pošte SendGrid (Node.js) | Microsoft Azure" 
    description="Saznajte kako za slanje e-pošte sa servisom SendGrid e-pošte na Azure. Kod uzorke koji se pišu pomoću Node.js API-JA." 
    services="" 
    documentationCenter="nodejs" 
    authors="erikre" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="01/05/2016" 
    ms.author="erikre"/>
# <a name="how-to-send-email-using-sendgrid-from-nodejs"></a>Upute za slanje e-pošte SendGrid iz Node.js

Ovaj vodič pokazuje kako izvoditi uobičajene zadatke programiranje sa servisom SendGrid e-pošte na Azure. Primjere se pišu pomoću Node.js API-JA. Scenariji u kojima je moguće uvrstiti **izgradnje e-pošte**, **Slanje e-pošte**, **Dodavanje privitaka**, **pomoću filtara**i **Ažuriranje svojstava**. Dodatne informacije o SendGrid i slanje e-pošte potražite u odjeljku [Sljedeće korake](#next-steps) .

## <a name="what-is-the-sendgrid-email-service"></a>Što je SendGrid servis za e-pošte?

SendGrid je [usluga e-pošte utemeljene na oblaku] koje nudi pouzdanog [isporuke transakcijskih e-pošte], skalabilnost i u stvarnom vremenu analitiku uz fleksibilne API-ji koji olakšavaju integraciju prilagođene. Uobičajeni scenariji za korištenje SendGrid obuhvaćaju sljedeće:

-   Automatski slanju potvrda klijentima
-   Administriranje raspodjele popisi za slanje klijentima mjesečnih e-fliers i posebne ponude
-   Prikupljanje u stvarnom vremenu metriku za elemente kao što su blokirane e-pošte i reaktivnosti klijenta
-   Generiranje izvješća da biste lakše prepoznavanje trendova
-   Prosljeđivanje anketa klijenta
-   Obavijesti e-poštom iz aplikacije

Dodatne informacije potražite u članku [https://sendgrid.com](https://sendgrid.com).

## <a name="create-a-sendgrid-account"></a>Stvaranje računa SendGrid

[AZURE.INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="reference-the-sendgrid-nodejs-module"></a>Referenca Node.js modul SendGrid

Modul SendGrid za Node.js moguće je instalirati putem upravitelja paketima čvor (npm) pomoću sljedeće naredbe:

    npm install sendgrid

Nakon instalacije, možete zatražiti modul u aplikaciji pomoću sljedeći kod:

    var sendgrid = require('sendgrid')(sendgrid_username, sendgrid_password);

Modul SendGrid izvoz funkcijama **SendGrid** i **e-pošte** .
**SendGrid** je zadužen za slanje e-pošte putem Web API dok **e-pošte** encapsulates poruke e-pošte.

## <a name="how-to-create-an-email"></a>Kako: Stvaranje poruke e-pošte

Stvaranje poruke e-pošte pomoću modula SendGrid uključuje najprije stvorite poruku e-pošte pomoću funkcije e-pošte, a zatim slanja pomoću funkcije SendGrid. Slijedi primjer stvaranje nove poruke pomoću funkcije e-pošte:

    var email = new sendgrid.Email({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.'
    });

HTML poruka možete odrediti i za klijente koji podržavaju tako da svojstvo html. Ako, na primjer:

    html: This is a sample <b>HTML<b> email message.

Postavljanje svojstava za tekst i html nudi graceful pričuvne u tekstni sadržaj za klijente koja ne podržava HTML porukama.

Dodatne informacije o sva svojstva podržava funkciju e-pošte potražite u članku [[sendgrid nodejs]].

## <a name="how-to-send-an-email"></a>Kako: slanje poruke e-pošte

Kada stvorite poruku e-pošte pomoću funkcije e-pošte, možete poslati pomoću API Web nudi SendGrid. 

### <a name="web-api"></a>API na webu

    sendgrid.send(email, function(err, json){
        if(err) { return console.error(err); }
        console.log(json);
    });

> [AZURE.NOTE] Dok iznad primjeri pokazuju Prenos u funkciji e-pošte i povratnog objekta, možete izravno pozivanje funkciju Pošalji izravno navođenjem svojstva e-pošte. Ako, na primjer:  
>
>`````
sendgrid.send({
    to: 'john@contoso.com',
    from: 'anna@contoso.com',
    subject: 'test mail',
    text: 'This is a sample email message.'
});
`````

## How to: Add an Attachment

Attachments can be added to a message by specifying the file name(s) and
path(s) in the **files** property. The following example demonstrates
sending an attachment:

    sendgrid.send({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.',
        files: [
            {
                filename:     '',           // required only if file.content is used.
                contentType:  '',           // optional
                cid:          '',           // optional, used to specify cid for inline content
                path:         '',           //
                url:          '',           // == One of these three options is required
                content:      ('' | Buffer) //
            }
        ],
    });

> [AZURE.NOTE] When using the **files** property, the file must be accessible
through [fs.readFile](http://nodejs.org/docs/v0.6.7/api/fs.html#fs.readFile). If the file you wish to attach is hosted in Azure Storage, such as in a Blob container, you must first copy the file to local storage or to an Azure drive before it can be sent as an attachment using the **files** property.

## How to: Use Filters to Enable Footers and Tracking

SendGrid provides additional email functionality through the use of
filters. These are settings that can be added to an email message to
enable specific functionality such as enabling click tracking, Google
analytics, subscription tracking, and so on. For a full list of filters,
see [Filter Settings][].

Filters can be applied to a message by using the **filters** property.
Each filter is specified by a hash containing filter-specific settings.
The following examples demonstrate the footer and click tracking filters:

### Footer

    var email = new sendgrid.Email({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.'
    });
    
    email.setFilters({
        'footer': {
            'settings': {
                'enable': 1,
                'text/plain': 'This is a text footer.'
            }
        }
    });

    sendgrid.send(email);

### Click Tracking

    var email = new sendgrid.Email({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.'
    });
    
    email.setFilters({
        'clicktrack': {
            'settings': {
                'enable': 1
            }
        }
    });
    
    sendgrid.send(email);

## How to: Update Email Properties

Some email properties can be overwritten using **set*Property*** or
appended using **add*Property***. For example, you can add additional
recipients by using

    email.addTo('jeff@contoso.com');

or set a filter by using

    email.addFilter('footer', 'enable', 1);
    email.addFilter('footer', 'text/html', '<strong>boo</strong>');

For more information, see [sendgrid-nodejs][].

## How to: Use Additional SendGrid Services

SendGrid offers web-based APIs that you can use to leverage additional
SendGrid functionality from your Azure application. For full
details, see the [SendGrid API documentation][].

## Next Steps

Now that you've learned the basics of the SendGrid Email service, follow
these links to learn more.

-   SendGrid Node.js module repository: [sendgrid-nodejs][]
-   SendGrid API documentation:
    <https://sendgrid.com/docs>
-   SendGrid special offer for Azure customers:
    [http://sendgrid.com/azure.html](https://sendgrid.com/windowsazure.html)
  [special offer]: https://sendgrid.com/windowsazure.html
  [sendgrid-nodejs]: https://github.com/sendgrid/sendgrid-nodejs
  [Filter Settings]: https://sendgrid.com/docs/API_Reference/SMTP_API/apps.html
  [SendGrid API documentation]: https://sendgrid.com/docs
  [cloud-based email service]: https://sendgrid.com/email-solutions
  [transactional email delivery]: https://sendgrid.com/transactional-email
