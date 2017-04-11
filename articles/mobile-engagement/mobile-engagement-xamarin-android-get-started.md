<properties
    pageTitle="Početak rada s Azure mobilne radnje za Xamarin.Android"
    description="Saznajte kako koristiti radnje Mobile Azure s analize i automatske obavijesti za Xamarin.Android aplikacije."
    services="mobile-engagement"
    documentationCenter="xamarin"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="06/16/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-xamarinandroid-apps"></a>Početak rada s Azure mobilne radnje za Xamarin.Android aplikacije

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Ova tema prikazuje kako ga koristiti Azure Mobile radnje da biste shvatili Upotreba aplikacije i automatske obavijesti poslati segmentiranog korisnicima Xamarin.Android aplikacije.
Pomoću ovog praktičnog vodiča pokazuje jednostavni scenarij emitiranje pomoću mobilnog radnje. U njoj Stvorite praznu aplikaciju Xamarin.Android koji prikuplja osnovne podatke i prima automatske obavijesti pomoću poruka Google Cloud (GCM).

Pomoću ovog praktičnog vodiča potrebno je sljedeće:

+ [Xamarin Studio](http://xamarin.com/studio). Možete koristiti i Visual Studio s Xamarin, ali ovog praktičnog vodiča koristi Xamarin Studio. Upute za instalaciju potražite u članku [Postavljanje i instalirajte za Visual Studio i Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).
+ [Mobilni radnje Xamarin SDK](https://www.nuget.org/packages/Microsoft.Azure.Engagement.Xamarin/)

> [AZURE.NOTE] Da biste dovršili ovaj Praktični vodič, morate imati račun za Azure active poruka. Ako nemate račun, možete stvoriti besplatnu probnu računa u samo nekoliko minuta. Detalje potražite u članku [Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-android-get-started).

##<a id="setup-azme"></a>Postavljanje mobilnih radnje za aplikacija za Android

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Povezivanje aplikacije s pozadinskom Mobile radnje

Pomoću ovog praktičnog vodiča predstavlja na "Osnovni Integracija", što je minimalnim potrebne za prikupljanje podataka i slanje automatske obavijesti. 

Ne možemo će stvoriti osnovni aplikacije s Xamarin Studio da bismo pokazali integraciju sa servisom.

###<a name="create-a-new-xamarinandroid-project"></a>Stvaranje novog Xamarin.Android projekta

1. Pokretanje **Xamarin Studio** idite na karticu **datoteka** -> **nove** -> **rješenja** 

    ![][1]

2. Odaberite **Aplikacija za Android** , a zatim provjerite je li na odabranom jeziku **C#** i kliknite **Dalje**.

    ![][2]

3. Unesite **Naziv aplikacije** i **identifikator tvrtke ili ustanove**. Obavezno potvrdite **Google Play s servisi** , a zatim kliknite **Dalje**. 

    ![][3]
    
4. Ako je potrebno ažurirati **Naziv projekta**, **Rješenje naziv** i **mjesto** , a zatim kliknite **Stvori**.

    ![][4]
 
Xamarin Studio će stvoriti aplikaciju u kojoj će smo integrirati Mobile radnje. 

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Povezivanje aplikacije s pozadinskom Mobile radnje

1. Desnom tipkom miša kliknite mapu **paketa** u windows rješenja, a zatim odaberite **Dodaj paketa...**

    ![][5]

2. Traženje **Microsoft Azure Mobile radnje Xamarin SDK** i dodajte ga rješenje.  

    ![][6]
   
3. Otvorite **MainActivity.cs** i dodajte sljedeće pomoću naredbe:

        using Microsoft.Azure.Engagement;
        using Microsoft.Azure.Engagement.Activity;

4. U na `OnCreate` način, dodajte sljedeće Inicijalizacija veze s pozadinskom Mobile radnje. Provjerite je li da biste dodali **ConnectionString**. 

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.ConnectionString = "YourConnectionStringFromAzurePortal";
        EngagementAgent.Init(engagementConfiguration);

###<a name="add-permissions-and-a-service-declaration"></a>Dodavanje dozvole i servis za deklariranje

1. Otvaranje **Manifest.xml** datoteku u odjeljku Svojstva mape. Odaberite karticu izvor tako da ažurirate izravno XML izvor.
 
2. Dodavanje tih dozvola Manifest.xml (koju je moguće pronaći u mapi **Svojstva** ) projekta neposredno prije ili nakon na `<application>` oznaka:

        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>

3. Dodajte sljedeće između na `<application>` i `</application>` oznake deklarirati agent servisa:

        <service
            android:name="com.microsoft.azure.engagement.service.EngagementService"
            android:exported="false"
            android:label="<Your application name>"
            android:process=":Engagement"/>

4. U kodu upravo zalijepljenog zamjena `"<Your application name>"` na naljepnici. Prikazuje se na izborniku **Postavke** koju korisnici mogu gledati services pokrenut na uređaju. Možete dodati riječ "Servisa" u taj natpis, primjerice.

###<a name="send-a-screen-to-mobile-engagement"></a>Slanje na zaslon Mobile radnje

Da biste pokrenuli slanje podataka i jamči da su aktivni korisnici, barem jedan zaslon mora poslati pozadinskog Mobile radnje. Za taj postupak-Pobrinite se da u `MainActivity` nasljeđuje od `EngagementActivity` umjesto `Activity`.

    public class MainActivity : EngagementActivity
    
Osim toga, ako ne nasljeđuju od `EngagementActivity` , a zatim morate dodati `.StartActivity` i `.EndActivity` načine u `OnResume` i `OnPause` odnosno.  

        protected override void OnResume()
            {
                EngagementAgent.StartActivity(EngagementAgentUtils.BuildEngagementActivityName(Java.Lang.Class.FromType(this.GetType())), null);
                base.OnResume();             
            }
    
            protected override void OnPause()
            {
                EngagementAgent.EndActivity();
                base.OnPause();            
            }

##<a id="monitor"></a>Povezivanje aplikacije pomoću nadzora u stvarnom vremenu

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Omogućivanje automatske obavijesti i poruka u aplikaciji

Radnje Mobile omogućuje interakciju s i DOSEGNE korisnika s automatske obavijesti i u aplikaciji poruka u kontekstu kampanja. U ovom modul naziva RAZGOVOR na portalu Mobile radnje.
U sljedećim se odjeljcima postavlja aplikacije primati.

[AZURE.INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

[AZURE.INCLUDE [Enable in-app messaging](../../includes/mobile-engagement-android-send-push.md)]

[AZURE.INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

<!-- Images -->
[1]: ./media/mobile-engagement-xamarin-android-get-started/1.png
[2]: ./media/mobile-engagement-xamarin-android-get-started/2.png
[3]: ./media/mobile-engagement-xamarin-android-get-started/3.png
[4]: ./media/mobile-engagement-xamarin-android-get-started/4.png
[5]: ./media/mobile-engagement-xamarin-android-get-started/5.png
[6]: ./media/mobile-engagement-xamarin-android-get-started/6.png
