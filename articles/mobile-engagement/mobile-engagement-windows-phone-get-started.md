<properties
    pageTitle="Početak rada s aplikacijama Azure Mobile radnje za Windows Phone Silverlight"
    description="Saznajte kako koristiti radnje Mobile Azure s analize i automatske obavijesti za Windows Phone Silverlight aplikacije."
    services="mobile-engagement"
    documentationCenter="windows"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-windows-phone-silverlight-apps"></a>Početak rada s aplikacijama Azure Mobile radnje za Windows Phone Silverlight

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

U ovoj se temi objašnjava da biste koristili Azure Mobile radnje za razumijevanje Upotreba aplikacije i automatske obavijesti poslati segmentiranog korisnicima aplikacije za Windows Phone Silverlight.
Pomoću ovog praktičnog vodiča pokazuje jednostavni scenarij za emitiranje pomoću mobilnog radnje. U njoj Stvorite praznu aplikaciju za Windows Phone Silverlight koja prikuplja osnovne podatke i prima automatske obavijesti koristeći Microsoft automatske obavijesti servisa (MPNS).

> [AZURE.NOTE] Ako birate Windows Phone 8.1 (koji nisu Silverlight), pročitajte [vodič Windows univerzalni](mobile-engagement-windows-store-dotnet-get-started.md).

Pomoću ovog praktičnog vodiča potrebno je sljedeće:

+ Visual Studio 2013
+ [MicrosoftAzure.MobileEngagement] Nuget paketa

> [AZURE.NOTE] Da biste dovršili ovaj Praktični vodič, morate imati račun za Azure active poruka. Ako nemate račun, možete stvoriti besplatnu probnu računa u samo nekoliko minuta. Detalje potražite u članku [Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-windows-phone-get-started).

##<a id="setup-azme"></a>Postavljanje mobilnih radnje za sustava Windows Phone aplikacija

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Povezivanje aplikacije s pozadinskom Mobile radnje

Pomoću ovog praktičnog vodiča predstavlja na "Osnovni Integracija", što je minimalnim potrebne za prikupljanje podataka i slanje automatske obavijesti. Potpuna Integracija dokumentaciju pronaći ćete u [Mobile radnje Windows Phone SDK Integracija](mobile-engagement-windows-phone-sdk-overview.md)

Pomoću programa Visual Studio demonstrirati integraciju sa servisom ćemo će stvoriti osnovni aplikacije.

###<a name="create-a-new-windows-phone-silverlight-project"></a>Stvaranje novog projekta Silverlight za Windows Phone

Sljedeći koraci pretpostavlja korištenje Visual Studio 2015 Premda koraci su slično u ranijim verzijama programa Visual Studio. 

1. Pokrenite Visual Studio, a na zaslonu **Polazno** odaberite **Novi projekt**.

2. Na skočnom izborniku odaberite **Windows 8** -> **Windows Phone** -> **Prilagođenom web -aplikacijom (Windows Phone Silverlight)**. Unesite **naziv** aplikacije i **naziv rješenja**, a zatim kliknite **u redu**.

    ![][1]

3. Možete odabrati ciljnim **Windows Phone 8.0** ili **Windows Phone 8.1**.

Sada stvorite novu aplikaciju Windows Phone Silverlight u koju će smo integrirati radnje SDK za Azure Mobile.

###<a name="connect-your-app-to-the-mobile-engagement-backend"></a>Povezivanje aplikacije s pozadinskom Mobile radnje

1. Instalirajte paket nuget [MicrosoftAzure.MobileEngagement] u projektu.

2. Otvaranje `WMAppManifest.xml` (u odjeljku Svojstva mapa) i provjerite je li sljedeće su deklarirane (ih dodati ako nisu) u na `<Capabilities />` oznaka:

        <Capability Name="ID_CAP_NETWORKING" />
        <Capability Name="ID_CAP_IDENTITY_DEVICE" />

    ![][2]

3. Sada zalijepite niz za povezivanje koju ste ranije kopirali radnje mobilne aplikacije i zalijepite ga u na `Resources\EngagementConfiguration.xml` datoteke, između na `<connectionString>` i `</connectionString>` oznaka:

    ![][3]

4. U na `App.xaml.cs` datoteke:

    na. Dodavanje u `using` izjava:

            using Microsoft.Azure.Engagement;

    b. Pokretanje SDK u na `Application_Launching` metoda:

            private void Application_Launching(object sender, LaunchingEventArgs e)
            {
              EngagementAgent.Instance.Init();
            }

    c. Umetanje sljedeće u na `Application_Activated`:

            private void Application_Activated(object sender, ActivatedEventArgs e)
            {
               EngagementAgent.Instance.OnActivated(e);
            }

##<a id="monitor"></a>Omogućiti praćenje u stvarnom vremenu

Da biste pokrenuli slanje podataka i provjera jesu li korisnici aktivna, pozadinski Mobile radnje mora poslati barem jedan zaslon (aktivnosti).

1. U MainPage.xaml.cs, dodajte na `using` izjava:

        using Microsoft.Azure.Engagement;

2. Osnovni klasa **MainPage**, koji je prije **PhoneApplicationPage**, zamijenite **EngagementPage**.

        class MainPage : EngagementPage 
    
3. U vašem `MainPage.xml` datoteke:

    na. Dodajte svoje deklaracija prostora naziva:

         xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"

    b. Zamjena `phone:PhoneApplicationPage` u naziv XML oznake s `engagement:EngagementPage`.

##<a id="monitor"></a>Povezivanje aplikacije pomoću nadzora u stvarnom vremenu

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Omogućivanje automatske obavijesti i poruka u aplikaciji

Radnje Mobile omogućuje interakciju i dosegne korisnika s automatske obavijesti i razmjena poruka u aplikaciji u kontekstu kampanja. U ovom modul naziva RAZGOVOR na portalu Mobile radnje.
U sljedećim se odjeljcima Postavljanje aplikacije primati.

###<a name="enable-your-app-to-receive-mpns-push-notifications"></a>Omogućivanje aplikacije za primanje MPNS automatske obavijesti

Dodavanje nove mogućnosti za svoje `WMAppManifest.xml` datoteke:

        ID_CAP_PUSH_NOTIFICATION
        ID_CAP_WEBBROWSERCOMPONENT

   ![][5]

###<a name="initialize-the-reach-sdk"></a>Pokretanje SDK za RAZGOVOR

1. U `App.xaml.cs`, poziva `EngagementReach.Instance.Init();` u funkciji **Application_Launching** odmah nakon agent za inicijalizaciju:

        private void Application_Launching(object sender, LaunchingEventArgs e)
        {
           EngagementAgent.Instance.Init();
           EngagementReach.Instance.Init();
        }

2. U `App.xaml.cs`, poziva `EngagementReach.Instance.OnActivated(e);` u funkciji **Application_Activated** odmah nakon agent za inicijalizaciju:

        private void Application_Activated(object sender, ActivatedEventArgs e)
        {
           EngagementAgent.Instance.OnActivated(e);
           EngagementReach.Instance.OnActivated(e);
        }

Sve je spremno. Sada ćemo će provjerite je li da koju ste pravilno Nasmije out ta osnovna integracija.

##<a id="send"></a>Pošaljite obavijest u aplikaciju

[AZURE.INCLUDE [Create Windows Push campaign](../../includes/mobile-engagement-windows-push-campaign.md)]

Trebali biste sada vidjeti obavijest na uređaju koji prikazat će se kao obavijest u aplikaciji Ako aplikaciju otvoren u suprotnom kao skočnoj obavijesti kao što je sljedeća: 

![][6]

<!-- URLs. -->
[MicrosoftAzure.MobileEngagement]: http://go.microsoft.com/?linkid=9874664
[Mobile Engagement Windows Phone SDK documentation]: ../mobile-engagement-windows-phone-integrate-engagement/

<!-- Images. -->
[1]: ./media/mobile-engagement-windows-phone-get-started/project-properties.png
[2]: ./media/mobile-engagement-windows-phone-get-started/wmappmanifest-capabilities.png
[3]: ./media/mobile-engagement-windows-phone-get-started/add-connection-string.png
[5]: ./media/mobile-engagement-windows-phone-get-started/reach-capabilities.png
[6]: ./media/mobile-engagement-windows-phone-get-started/push-screenshot.png
