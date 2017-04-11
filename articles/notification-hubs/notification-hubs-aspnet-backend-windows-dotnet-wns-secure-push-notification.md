<properties
    pageTitle="Automatske koncentratora Azure obavijesti sigurna"
    description="Saznajte kako da biste poslali sigurnu automatske obavijesti Azure. Primjere koda pisane C# pomoću .NET API-JA."
    documentationCenter="windows"
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"/>

<tags
    ms.service="notification-hubs" 
    ms.workload="mobile"
    ms.tgt_pltfrm="windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

#<a name="azure-notification-hubs-secure-push"></a>Automatske obavijesti Azure koncentratora sigurne

> [AZURE.SELECTOR]
- [Univerzalni za Windows](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
- [iOS](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
- [Android](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)


##<a name="overview"></a>Pregled

Podrška za automatske obavijesti u Microsoft Azure omogućuje vam da biste pristupili infrastrukture jednostavno koristi multiplatform, skalirana izlaz automatske koji za komercijalni ispis olakšava implementacije slanje obavijesti za korisnika i enterprise aplikacije za mobilne platforme.

Zbog propisima ili sigurnosnih ograničenja, ponekad aplikacije možda htjeti uključiti nešto u obavijesti koja se ne možete prenositi putem Infrastruktura standardne automatske obavijesti. Pomoću ovog praktičnog vodiča opisuje način da biste postigli iste doživljaj slanjem povjerljivih podataka putem sigurne, čija je autentičnost provjerena veze između klijentskog uređaja i pozadinska aplikacija.

Visoke razine tijeka je kako slijedi:

1. Aplikacija pozadinske:
    - Služi za pohranu sigurne tereta u pozadinskoj bazi podataka.
    - Šalje ID obavijest uređaj (sigurne ne šalju nikakvi podaci).
2. Aplikacija na uređaju, prilikom primanja obavijesti:
    - Uređaj kontakta pozadinske zahtijeva sigurnu opseg.
    - Aplikaciju možete prikazati opseg kao obavijest na uređaju.

Važno Imajte na umu da u prethodnom tijek (i u ovom ćete praktičnom vodiču), ne možemo pretpostavlja da uređaj pohranjuje token za provjeru autentičnosti u lokalno spremište kada se korisnik prijavljuje je. Time će se jamčiti potpuno objedinjenog doživljaj, kao što je uređaja možete dohvatiti na obavijest sigurne tereta pomoću ovaj token. Ako aplikacija pohranite tokeni za provjeru autentičnosti na uređaj ili ako je istekao tokena aplikaciju uređaj nakon prima obavijest prikazati generički obavijesti postavljanja upita korisniku pokrenite aplikaciju. Aplikaciju pa potvrđuje korisnika i prikazuje opseg obavijesti.

Pomoću ovog praktičnog vodiča sigurne automatske pokazuje kako sigurno Pošalji automatske obavijesti. Vodič sastavlja na vodič [Obavijestiti korisnike](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) da bi se trebali biste najprije dovršite korake u tom praktičnom vodiču.

> [AZURE.NOTE] Pomoću ovog praktičnog vodiča pretpostavlja da ste stvorili i konfigurirali koncentratora za obavijesti, kao što je opisano u [Početak rada s obavijesti koncentratora (Windows trgovina)](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md).
Osim toga, imajte na umu da Windows Phone 8.1 zahtijeva vjerodajnice sustava Windows (ne Windows Phone), a da pozadinske zadatke ne radi u sustavu Windows Phone 8.0 ili Silverlight 8.1. Za aplikacije iz Windows trgovine, možete primati obavijesti putem zadatak u pozadini samo ako je aplikaciju zaključani zaslon omogućeno (kliknite potvrdni okvir u na Appmanifest).

[AZURE.INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-windows-phone-project"></a>Izmjena projekt programa Windows Phone

1. U programu project **NotifyUserWindowsPhone** dodati sljedeći kod App.xaml.cs da biste registrirali automatske pozadinski zadatak. Dodavanje sljedeći redak koda na kraju na `OnLaunched()` metoda:

        RegisterBackgroundTask();

2. Ne napuštajući App.xaml.cs, dodajte sljedeće odmah nakon koda u `OnLaunched()` metoda:

        private async void RegisterBackgroundTask()
        {
            if (!Windows.ApplicationModel.Background.BackgroundTaskRegistration.AllTasks.Any(i => i.Value.Name == "PushBackgroundTask"))
            {
                var result = await BackgroundExecutionManager.RequestAccessAsync();
                var builder = new BackgroundTaskBuilder();

                builder.Name = "PushBackgroundTask";
                builder.TaskEntryPoint = typeof(PushBackgroundComponent.PushBackgroundTask).FullName;
                builder.SetTrigger(new Windows.ApplicationModel.Background.PushNotificationTrigger());
                BackgroundTaskRegistration task = builder.Register();
            }
        }

3. Dodajte sljedeće `using` naredbe pri vrhu datoteke App.xaml.cs:

        using Windows.Networking.PushNotifications;
        using Windows.ApplicationModel.Background;

4. Na izborniku **datoteka** u Visual Studio, kliknite **Spremi sve**.

## <a name="create-the-push-background-component"></a>Stvaranje komponentu automatske pozadine

Sljedeći je korak da biste stvorili pozadinu komponentu automatske.

1. U pregledniku rješenja, desnom tipkom miša kliknite čvor najviše razine rješenja (**SecurePush rješenja** u ovom slučaju), zatim kliknite **Dodaj**, a zatim kliknite **Novi projekt**.

2. Proširite **Trgovine aplikacija**, a zatim kliknite **Windows Phone aplikacije**, a zatim kliknite **Komponenta za vrijeme izvođenja sustava Windows (Windows Phone)**. Naziv projekta **PushBackgroundComponent**, a zatim **u redu** da biste stvorili projekta.

    ![][12]

3. U pregledniku rješenja, desnom tipkom miša kliknite projekt **PushBackgroundComponent (Windows Phone 8.1)** , a zatim kliknite **Dodaj**, a zatim kliknite **Predmet**. Naziv nove klase **PushBackgroundTask.cs**. Kliknite **Dodaj** da biste generirali klasu.

4. Sav sadržaj definiciju prostor naziva **PushBackgroundComponent** zamijenite sljedeći kod zamjenjujući rezervirano mjesto `{back-end endpoint}` s krajnje pozadinsku dobiven prilikom implementacije sustava pozadinsku:

        public sealed class Notification
            {
                public int Id { get; set; }
                public string Payload { get; set; }
                public bool Read { get; set; }
            }

            public sealed class PushBackgroundTask : IBackgroundTask
            {
                private string GET_URL = "{back-end endpoint}/api/notifications/";

                async void IBackgroundTask.Run(IBackgroundTaskInstance taskInstance)
                {
                    // Store the content received from the notification so it can be retrieved from the UI.
                    RawNotification raw = (RawNotification)taskInstance.TriggerDetails;
                    var notificationId = raw.Content;

                    // retrieve content
                    BackgroundTaskDeferral deferral = taskInstance.GetDeferral();
                    var httpClient = new HttpClient();
                    var settings = ApplicationData.Current.LocalSettings.Values;
                    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string)settings["AuthenticationToken"]);

                    var notificationString = await httpClient.GetStringAsync(GET_URL + notificationId);

                    var notification = JsonConvert.DeserializeObject<Notification>(notificationString);

                    ShowToast(notification);

                    deferral.Complete();
                }

                private void ShowToast(Notification notification)
                {
                    ToastTemplateType toastTemplate = ToastTemplateType.ToastText01;
                    XmlDocument toastXml = ToastNotificationManager.GetTemplateContent(toastTemplate);
                    XmlNodeList toastTextElements = toastXml.GetElementsByTagName("text");
                    toastTextElements[0].AppendChild(toastXml.CreateTextNode(notification.Payload));
                    ToastNotification toast = new ToastNotification(toastXml);
                    ToastNotificationManager.CreateToastNotifier().Show(toast);
                }
            }

5. U pregledniku rješenja, desnom tipkom miša kliknite projekt **PushBackgroundComponent (Windows Phone 8.1)** , a zatim kliknite **Upravljanje NuGet paketa**.

6. S lijeve strane kliknite na **mreži**.

7. U okvir za **pretraživanje** upišite **Http klijenta**.

8. Na popisu rezultata kliknite **Microsoft HTTP klijenta biblioteke**, a zatim kliknite **Instaliraj**. Dovršite instalaciju.

9. Vratite se u okviru NuGet **pretraživanja** upišite **Json.net**. Instalirajte paket **Json.NET** , a zatim zatvorite prozor Upravitelj NuGet paketa.

10. Dodajte sljedeće `using` naredbe pri vrhu datoteke **PushBackgroundTask.cs** :

        using Windows.ApplicationModel.Background;
        using Windows.Networking.PushNotifications;
        using System.Net.Http;
        using Windows.Storage;
        using System.Net.Http.Headers;
        using Newtonsoft.Json;
        using Windows.UI.Notifications;
        using Windows.Data.Xml.Dom;

11. U Eksploreru za rješenja u projektu **NotifyUserWindowsPhone (Windows Phone 8.1)** , desnom tipkom miša kliknite **reference**, a zatim kliknite **Dodaj referenca...**. U dijaloškom okviru Upravitelj referenca potvrdite okvir pokraj **PushBackgroundComponent**, a zatim kliknite **u redu**.

12. U pregledniku rješenja, dvokliknite **Package.appxmanifest** u programu project **NotifyUserWindowsPhone (Windows Phone 8.1)** . U odjeljku **obavijesti**, postavite **Skočnoj instaliranih** na **da**.

    ![][3]

13. I dalje u **Package.appxmanifest**, kliknite izbornik **deklaracija** blizu vrha. Na padajućem popisu **Dostupna deklaracija** kliknite **Pozadinske zadatke**, a zatim kliknite **Dodaj**.

14. U **Package.appxmanifest**, u odjeljku **Svojstva**potvrdite **automatske obavijesti**.

15. U **Package.appxmanifest**, u odjeljku **Postavke aplikacije**, upišite **PushBackgroundComponent.PushBackgroundTask** u polju **Točka unosa** .

    ![][13]

16. Na izborniku **datoteka** kliknite **Spremi sve**.

## <a name="run-the-application"></a>Pokrenite aplikaciju

Da biste pokrenuli aplikaciju, učinite sljedeće:

1. U Visual Studio, pokrenite **AppBackend** Web API. Prikazat će se u ASP.NET web-stranicu.

2. U Visual Studio, pokrenite aplikaciju Windows Phone **NotifyUserWindowsPhone (Windows Phone 8.1)** . Windows Phone emulator pokreće, a automatski učitava aplikaciju.

3. U aplikaciji **NotifyUserWindowsPhone** korisničkog Sučelja, unesite korisničko ime i lozinku. To može biti bilo koji niz znakova, ali moraju biti iste vrijednosti.

4. U aplikaciji **NotifyUserWindowsPhone** korisničkog Sučelja kliknite **prijaviti i registrirati**. Zatim kliknite **Pošalji automatske**.

[3]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push3.png
[12]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push12.png
[13]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push13.png
