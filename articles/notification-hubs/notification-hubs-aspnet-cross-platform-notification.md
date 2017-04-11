<properties
    pageTitle="Različite platforme obavijesti poslati korisnicima s obavijesti koncentratora (ASP.NET)"
    description="Saznajte kako koristiti predloške koncentratora obavijesti da biste poslali u jedan zahtjev obavijest platformu agnostic namijenjen sve platforme."
    services="notification-hubs"
    documentationCenter=""
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/03/2016" 
    ms.author="yuaxu"/>

# <a name="send-cross-platform-notifications-to-users-with-notification-hubs"></a>Slanje obavijesti za različite platforme korisnicima s obavijesti koncentratora


U prethodnom izvješću [obavijesti korisnici s obavijesti koncentratora]naučili kako automatske obavijesti na svim uređajima registrirane određenog korisnika čija je autentičnost provjerena. U taj ćete praktičnom vodiču više zahtjeva za su potrebna za slanje obavijesti svaki platformu podržani klijent. Obavijesti koncentratora podržava predložaka koji omogućuju vam da odredite način na koji se određeni uređaj želi primati obavijesti. Time se pojednostavljuje slanja obavijesti za različite platforme. U ovoj se temi opisuje način da iskoristite prednost predloške za slanje u jedan zahtjev obavijest platformu agnostic namijenjen sve platforme. Detaljnije informacije o predlošcima, potražite u članku [Pregled koncentratora za Azure obavijesti][Templates].

> [AZURE.NOTE] Obavijest o koncentratora omogućuje uređaj da biste registrirali većeg broja predložaka s istom oznakom. U ovom slučaju dolazne poruke ciljanja ta oznaka rezultira više obavijesti koji se isporučuju s uređajem, jedan za svaki predložak. Omogućuje prikaz ista poruka u više vizualne obavijesti, kao što su kao značku i kao skočnoj obavijesti u sustavu Windows trgovine.

Izvršite sljedeće korake za slanje obavijesti za različite platforme pomoću predložaka:

1. U pregledniku rješenja u Visual Studio proširite mapu **kontrolera** , a zatim otvorite datoteku RegisterController.cs.

2. Pronađite blokova Šifra u **objavu** način koji stvara novu Registracija Zamijeni na `switch` sadržaja s sljedeći kod:

        switch (deviceUpdate.Platform)
        {
            case "mpns":
                var toastTemplate = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                    "<wp:Notification xmlns:wp=\"WPNotification\">" +
                       "<wp:Toast>" +
                            "<wp:Text1>$(message)</wp:Text1>" +
                       "</wp:Toast> " +
                    "</wp:Notification>";
                registration = new MpnsTemplateRegistrationDescription(deviceUpdate.Handle, toastTemplate);
                break;
            case "wns":
                toastTemplate = @"<toast><visual><binding template=""ToastText01""><text id=""1"">$(message)</text></binding></visual></toast>";
                registration = new WindowsTemplateRegistrationDescription(deviceUpdate.Handle, toastTemplate);
                break;
            case "apns":
                var alertTemplate = "{\"aps\":{\"alert\":\"$(message)\"}}";
                registration = new AppleTemplateRegistrationDescription(deviceUpdate.Handle, alertTemplate);
                break;
            case "gcm":
                var messageTemplate = "{\"data\":{\"message\":\"$(message)\"}}";
                registration = new GcmTemplateRegistrationDescription(deviceUpdate.Handle, messageTemplate);
                break;
            default:
                throw new HttpResponseException(HttpStatusCode.BadRequest);
        }

    Kod poziva ovisne način da biste stvorili predložak Registracija umjesto nativni registracije. Postojeće registracije potrebno nije moguće mijenjati jer je predložak registracije proizlaziti iz izvorne registracije.

3. U kontrolerom **obavijesti** zamijenite metodu **sendNotification** sljedeći kod:

        public async Task<HttpResponseMessage> Post()
        {
            var user = HttpContext.Current.User.Identity.Name;
            var userTag = "username:" + user;

            var notification = new Dictionary<string, string> { { "message", "Hello, " + user } };
            await Notifications.Instance.Hub.SendTemplateNotificationAsync(notification, userTag);

            return Request.CreateResponse(HttpStatusCode.OK);
        }

    Kod će poslati primatelju obavijest da biste sve platforme u isto vrijeme i bez potrebe za određivanje nativni tereta. Obavijest koncentratora stvara i nudi točan opseg na svim uređajima s vrijednošću navedeni _oznaku_ , kao što je navedeno u registriranih predložaka.

4. Ponovno objavite projekt pozadinsku WebApi.

5. Ponovno pokrenite aplikaciju klijenta i potvrda uspješnog registracije.

6. (Neobavezno) Implementacija aplikacije klijenta na drugi uređaj, a zatim pokrenite aplikaciju.

    Imajte na umu da će se prikazati obavijest na svakom uređaju.

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste dovršili ovaj Praktični vodič, Saznajte više o koncentratora obavijesti i predlošci u ovim temama:

+ **[Korištenje koncentratora obavijesti da biste poslali važnih vijesti]** <br/>Prikazuje drugi scenarij za korištenje predložaka

+  **[Pregled koncentratora Azure obavijesti][Templates]**<br/>Pregled tema ima detaljnije informacije o predlošcima.


<!-- Anchors. -->

<!-- Images. -->




<!-- URLs. -->
[Push to users ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Push to users Mobile Services]: /manage/services/notification-hubs/notify-users/
[Visual Studio 2012 Express for Windows 8]: http://go.microsoft.com/fwlink/?LinkId=257546

[Korištenje koncentratora obavijesti da biste poslali važnih vijesti]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
[Azure Notification Hubs]: http://go.microsoft.com/fwlink/p/?LinkId=314257
[Obavijestiti korisnike s obavijesti koncentratora]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Templates]: http://go.microsoft.com/fwlink/p/?LinkId=317339
[Notification Hub How to for Windows Store]: http://msdn.microsoft.com/library/windowsazure/jj927172.aspx
