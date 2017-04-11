<properties
    pageTitle="Početak rada s Azure mobilne radnje za Xamarin.iOS"
    description="Saznajte kako koristiti radnje Mobile Azure s analize i automatske obavijesti za Xamarin.iOS aplikacije."
    services="mobile-engagement"
    documentationCenter="xamarin"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-xamarinios-apps"></a>Početak rada s Azure mobilne radnje za Xamarin.iOS aplikacije

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

U ovoj se temi objašnjava korištenje Azure Mobile radnje za razumijevanje Upotreba aplikacije i automatske obavijesti poslati segmentiranog korisnicima u aplikaciji Xamarin.iOS.
U ovom ćete praktičnom vodiču Stvorite praznu aplikaciju Xamarin.iOS koji prikuplja osnovne podatke i prima automatske obavijesti pomoću Apple automatske obavijesti sustava (APN-ove).

Pomoću ovog praktičnog vodiča potrebno je sljedeće:

+ [Xamarin Studio](http://xamarin.com/studio). Možete koristiti i Visual Studio s Xamarin, ali ovog praktičnog vodiča koristi Xamarin Studio. Upute za instalaciju potražite u članku [Postavljanje i instalirajte za Visual Studio i Xamarin](https://msdn.microsoft.com/library/mt613162.aspx). 
+ [Mobilni radnje Xamarin SDK](https://www.nuget.org/packages/Microsoft.Azure.Engagement.Xamarin/)

> [AZURE.NOTE] Da biste dovršili ovaj Praktični vodič, morate imati račun za Azure active poruka. Ako nemate račun, možete stvoriti besplatnu probnu računa u samo nekoliko minuta. Detalje potražite u članku [Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-ios-get-started).

##<a id="setup-azme"></a>Postavljanje mobilnih radnje za aplikaciju za iOS

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Povezivanje aplikacije s pozadinskom Mobile radnje

Pomoću ovog praktičnog vodiča predstavlja na "Osnovni Integracija" što je s minimalnim postaviti potrebne za prikupljanje podataka i slanje automatske obavijesti.

S Xamarin da bismo pokazali integraciju sa servisom ćemo će stvoriti osnovni aplikacije:

###<a name="create-a-new-xamarinios-project"></a>Stvaranje novog Xamarin.iOS projekta

1. Pokrenite Xamarin Studio. Idite na **datoteka** -> **nove** -> **rješenja** 

    ![][1]

2. Odaberite **Jednu aplikaciju za prikaz**, provjerite je li na odabranom jeziku **C#** , a zatim kliknite **Dalje**.

    ![][2]

3. Unesite **Naziv aplikacije** i **Identifikator tvrtke ili ustanove** , a zatim kliknite **Dalje**. 

    ![][3]

    > [AZURE.IMPORTANT] Provjerite je li objavljivanje profila na koncu koristite za implementaciju aplikacije za iOS je pomoću aplikacije ID-a koji odgovara točno s identifikatorom paket imate ovdje. 

4. Ako je potrebno ažurirati **Projektu**, **Rješenje naziv** i **mjesto** , a zatim kliknite **Stvori**.

    ![][4]
 
Xamarin Studio će stvoriti aplikaciju pokazni videozapis u kojem će smo integrirati Mobile radnje. 

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Povezivanje aplikacije s pozadinskom Mobile radnje

1. Desnom tipkom miša kliknite mapu **paketa** u windows rješenja, a zatim odaberite **Dodavanje paketa...**

    ![][5]

2. Traženje **Microsoft Azure Mobile radnje Xamarin SDK** i dodajte ga rješenje.  

    ![][6]
   
3. Otvorite **AppDelegate.cs** i dodajte sljedeće pomoću naredbe:

        using Microsoft.Azure.Engagement.Xamarin;

4. U način **FinishedLaunching** dodajte na sljedeći način pokrenuti veze s pozadinskom Mobile radnje. Provjerite je li za dodavanje **ConnectionString**. Kod koristi i sustava **NotificationIcon** koju je dodao SDK Mobile radnje koje želite zamijeniti. 

        EngagementConfiguration config = new EngagementConfiguration {
                        ConnectionString = "YourConnectionStringFromAzurePortal",
                        NotificationIcon = UIImage.FromBundle("close")
                    };
        EngagementAgent.Init (config);

##<a id="monitor"></a>Omogućivanje nadzora u stvarnom vremenu

Da biste pokrenuli slanje podataka i jamči da su aktivni korisnici, barem jedan zaslon mora poslati pozadinskog Mobile radnje.

1. Otvorite **ViewController.cs** i dodajte sljedeće pomoću naredbe:

        using Microsoft.Azure.Engagement.Xamarin;

2. Zamjena predmete iz kojeg `ViewController` nasljeđuje od `UIViewController` da biste `EngagementViewController`. 

##<a id="monitor"></a>Povezivanje aplikacije pomoću nadzora u stvarnom vremenu

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Omogućivanje automatske obavijesti i poruka u aplikaciji

Radnje Mobile omogućuje interakciju s korisnicima i DOSEGNE s automatske obavijesti i u aplikaciji poruka u kontekstu kampanja. U ovom modul naziva RAZGOVOR na portalu Mobile radnje.
U sljedećim se odjeljcima Postavljanje aplikacije primati.

### <a name="modify-your-application-delegate"></a>Izmjena svog delegata aplikacije

1. Otvorite **AppDelegate.cs** i dodajte sljedeće pomoću naredbe:

        using System; 

2. Sada unutar na `FinishedLaunching` način, dodajte sljedeće da biste registrirali za automatsko prosljeđivanje poruka nakon`EngagementAgent.init(...)`

        if (UIDevice.CurrentDevice.CheckSystemVersion(8,0))
        {
            var pushSettings = UIUserNotificationSettings.GetSettingsForTypes (
                (UIUserNotificationType.Badge |
                    UIUserNotificationType.Sound |
                    UIUserNotificationType.Alert),
                null);
            UIApplication.SharedApplication.RegisterUserNotificationSettings (pushSettings);
            UIApplication.SharedApplication.RegisterForRemoteNotifications ();
        }
        else
        {
            UIApplication.SharedApplication.RegisterForRemoteNotificationTypes (
                UIRemoteNotificationType.Badge |
                UIRemoteNotificationType.Sound |
                UIRemoteNotificationType.Alert);
        }

3. Na kraju – ažuriranje ili dodajte na sljedeće načine:

        public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, 
            Action<UIBackgroundFetchResult> completionHandler)
        {
            EngagementAgent.ApplicationDidReceiveRemoteNotification(userInfo, completionHandler);
        }

        public override void RegisteredForRemoteNotifications (UIApplication application, NSData deviceToken)
        {
            // Register device token on Engagement
            EngagementAgent.RegisterDeviceToken(deviceToken);
        }

        public override void FailedToRegisterForRemoteNotifications(UIApplication application, NSError error)
        {
            Console.WriteLine("Failed to register for remote notifications: Error '{0}'", error);
        }

4. U datoteci **Info.plist** rješenje Provjerite odgovara li onome **Identifikator paket** **Aplikacije ID** imate u profilu za dodjelu resursa u Razvojni centar za Apple. 

    ![][7]

5. U isti **Info.plist** , provjerite da ste provjerili **Omogućivanje načina rada pozadine** i **Udaljena obavijesti**. 

    ![][8]

6. Pokrenite aplikaciju na uređaju ste povezali s ovaj objavljivanje profil. 

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- Images. -->
[1]: ./media/mobile-engagement-xamarin-ios-get-started/new-solution.png
[2]: ./media/mobile-engagement-xamarin-ios-get-started/app-type.png
[3]: ./media/mobile-engagement-xamarin-ios-get-started/configure-project-name.png
[4]: ./media/mobile-engagement-xamarin-ios-get-started/configure-project-confirm.png
[5]: ./media/mobile-engagement-xamarin-ios-get-started/add-nuget.png
[6]: ./media/mobile-engagement-xamarin-ios-get-started/add-nuget-azme.png
[7]: ./media/mobile-engagement-xamarin-ios-get-started/info-plist-confirm-bundle.png
[8]: ./media/mobile-engagement-xamarin-ios-get-started/info-plist-configure-push.png
