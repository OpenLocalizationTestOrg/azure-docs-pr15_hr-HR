<properties
    pageTitle="Početak rada s radnje Azure Mobile za Android jedinstvo implementacije"
    description="Saznajte kako koristiti radnje Mobile Azure s analize i automatske obavijesti za aplikacije jedinstvo implementacije za uređaje sa sustavom iOS."
    services="mobile-engagement"
    documentationCenter="unity"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-unity-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-unity-android-deployment"></a>Početak rada s radnje Azure Mobile za Android jedinstvo implementacije

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

U ovoj se temi saznati kako koristiti Azure Mobile radnje da biste shvatili Upotreba app i slanju automatske obavijesti da biste segmentirajući korisnici aplikacije jedinstvo kada uvođenje na uređaju sa sustavom Android.
Pomoću ovog praktičnog vodiča koristi klasične jedinstvo stariju verziju Marko vodič kao početnu točku. Morate slijediti korake u ovom [praktičnom vodiču](mobile-engagement-unity-roll-a-ball.md) prije nastavka Mobile radnje integraciju sa servisom smo demonstracije u nastavku praktičnom vodiču. 

Pomoću ovog praktičnog vodiča potrebno je sljedeće:

+ [Uređivač jedinstvo](http://unity3d.com/get-unity)
+ [Mobilni radnje jedinstvo SDK](https://aka.ms/azmeunitysdk)
+ Google Android SDK

> [AZURE.NOTE] Da biste dovršili ovaj Praktični vodič, morate imati račun za Azure active poruka. Ako nemate račun, možete stvoriti besplatnu probnu računa u samo nekoliko minuta. Detalje potražite u članku [Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-unity-android-get-started).

##<a id="setup-azme"></a>Postavljanje mobilnih radnje za aplikacija za Android

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Povezivanje aplikacije s pozadinskom Mobile radnje

###<a name="import-the-unity-package"></a>Uvoz jedinstvo paketa

1. Preuzmite [Mobile radnje jedinstvo paketa](https://aka.ms/azmeunitysdk) i spremite je na lokalno računalo. 

2. Idite na **Resursi -> Uvoz paketa -> Prilagođena paket** , a zatim odaberite paket koji ste preuzeli u ovaj korak. 

    ![][70] 

3. Provjerite je li sve datoteke nije odabrana i kliknite gumb **Uvezi** . 

    ![][71] 

4. Kad je uvoz uspješan, vidjet ćete uvezene datoteke SDK u projektu.  

    ![][72] 

###<a name="update-the-engagementconfiguration"></a>Ažuriranje na EngagementConfiguration

1. Otvaranje kopije datoteka skripte **EngagementConfiguration** iz mape SDK i ažuriranje na **ANDROID\_VEZA\_NIZ** s nizom za povezivanje ste nabavili ranije Azure portal.  

    ![][73]

2. Spremanje datoteke 

3. Izvršavanje **datoteke -> radnje -> Generiraj Android Manifest**. Ovo je dodatak dodao radnje SDK Mobile i klikom na njega automatski ažurirati postavke projekta. 

    ![][74]

> [AZURE.IMPORTANT] Provjerite jeste li izvesti svaki put kada ažurirate **EngagementConfiguration** datoteku u suprotnom promjene ne odražavaju se u aplikaciji. 

###<a name="configure-the-app-for-basic-tracking"></a>Konfiguriranje aplikacija za osnovni praćenja

1. Otvaranje kopije skripte **PlayerController** priložiti objekt reproduktora za uređivanje. 

2. Dodajte sljedeće pomoću naredbe:

        using Microsoft.Azure.Engagement.Unity;

3. Dodajte sljedeće da biste na `Start()` način
    
        EngagementAgent.Initialize();
        EngagementAgent.StartActivity("Home");

###<a name="deploy-and-run-the-app"></a>Implementacija i pokretanje aplikacija
Provjerite jeste li povezani sa sustavom Android SDK prije implementacije jedinstvo aplikacije na uređaju instaliran na vašem računalu. 

1. Povezivanje sa sustavom Android uređaj na vaše računalo. 

2. Otvaranje kopije **datoteke -> postavke za sastavljanje** 

    ![][40]

3. Odaberite **sa sustavom Android** , a zatim kliknite na **Platformi promjenu**

    ![][51]

    ![][52]

4. Kliknite **postavke reproduktora** i Navedite valjani identifikator paket. 

    ![][53]

5. Na kraju kliknite **Stvaranje i pokretanje**

    ![][54]

6. Možda ćete morati unijeti naziv mape za spremanje paketa za Android. 

7. Ako sve prelazi precizno, paket će biti implementirano u povezani uređaj i trebali biste vidjeti vaše igra jedinstvo na telefonu! 

##<a id="monitor"></a>Povezivanje aplikacije pomoću nadzora u stvarnom vremenu

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Omogućivanje automatske obavijesti i poruka u aplikaciji

[AZURE.INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

###<a name="update-the-engagementconfiguration"></a>Ažuriranje na EngagementConfiguration

1. Otvaranje kopije datoteka skripte **EngagementConfiguration** iz mape SDK i ažuriranje na **ANDROID\_GOOGLE\_broj** **Broj projekta Google** ste nabavili ranije portal Google Cloud za razvojne inženjere. To je niz vrijednost pa provjerite jeste li stavite u dvostruke navodnike. 

    ![][75]

2. Spremite datoteku. 

3. Izvršavanje **datoteke -> radnje -> Generiraj Android Manifest**. Ovo je dodatak dodao radnje SDK Mobile i klikom na njega automatski ažurirati postavke projekta. 

    ![][74]

###<a name="configure-the-app-to-receive-notifications"></a>Konfiguriranje aplikacija za primanje obavijesti

1. Otvaranje kopije skripte **PlayerController** priložiti objekt reproduktora za uređivanje. 

2. Dodajte sljedeće da biste na `Start()` način

        EngagementReachAgent.Initialize();

3. Sad kad se ažurira aplikaciju, uvođenje i pokretanje aplikacija na uređaju po upute navedene u nastavku. 

[AZURE.INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

<!-- Images -->
[40]: ./media/mobile-engagement-unity-android-get-started/40.png
[70]: ./media/mobile-engagement-unity-android-get-started/70.png
[71]: ./media/mobile-engagement-unity-android-get-started/71.png
[72]: ./media/mobile-engagement-unity-android-get-started/72.png
[73]: ./media/mobile-engagement-unity-android-get-started/73.png
[74]: ./media/mobile-engagement-unity-android-get-started/74.png
[75]: ./media/mobile-engagement-unity-android-get-started/75.png
[51]: ./media/mobile-engagement-unity-android-get-started/51.png
[52]: ./media/mobile-engagement-unity-android-get-started/52.png
[53]: ./media/mobile-engagement-unity-android-get-started/53.png
[54]: ./media/mobile-engagement-unity-android-get-started/54.png
