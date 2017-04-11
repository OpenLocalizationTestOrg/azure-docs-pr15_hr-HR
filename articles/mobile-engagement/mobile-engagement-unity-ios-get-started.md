<properties
    pageTitle="Početak rada s Azure Mobile radnje za implementaciju jedinstvo iOS"
    description="Saznajte kako koristiti radnje Mobile Azure s analize i automatske obavijesti za aplikacije jedinstvo implementacije za uređaje sa sustavom iOS."
    services="mobile-engagement"
    documentationCenter="unity"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-unity-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-unity-ios-deployment"></a>Početak rada s Azure Mobile radnje za implementaciju jedinstvo iOS

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

U ovoj se temi saznati kako koristiti Azure Mobile radnje da biste shvatili Upotreba app i slanju automatske obavijesti da biste segmentirajući korisnici aplikacije jedinstvo kada uvođenje na uređaju sa sustavom iOS.
Pomoću ovog praktičnog vodiča koristi klasične jedinstvo stariju verziju Marko vodič kao početnu točku. Morate slijediti korake u ovom [praktičnom vodiču](mobile-engagement-unity-roll-a-ball.md) prije nastavka Mobile radnje integraciju sa servisom smo demonstracije u nastavku praktičnom vodiču. 

Pomoću ovog praktičnog vodiča potrebno je sljedeće:

+ [Uređivač jedinstvo](http://unity3d.com/get-unity)
+ [Mobilni radnje jedinstvo SDK](https://aka.ms/azmeunitysdk)
+ Uređivač XCode

> [AZURE.NOTE] Da biste dovršili ovaj Praktični vodič, morate imati račun za Azure active poruka. Ako nemate račun, možete stvoriti besplatnu probnu računa u samo nekoliko minuta. Detalje potražite u članku [Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-unity-ios-get-started).

##<a id="setup-azme"></a>Postavljanje mobilnih radnje za aplikaciju za iOS

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Povezivanje aplikacije s pozadinskom Mobile radnje

###<a name="import-the-unity-package"></a>Uvoz paketa jedinstvo

1. Preuzmite [paket jedinstvo radnje Mobile](https://aka.ms/azmeunitysdk) i spremite je na lokalno računalo. 

2. Idite na **Resursi -> Uvoz paketa -> Prilagođena paket** , a zatim odaberite paket koji ste preuzeli u ovaj korak. 

    ![][70] 

3. Provjerite je li sve datoteke nije odabrana i kliknite gumb **Uvezi** . 

    ![][71] 

4. Kad je uvoz uspješan, vidjet ćete uvezene datoteke SDK u projektu.  

    ![][72] 

###<a name="update-the-engagementconfiguration"></a>Ažuriranje na EngagementConfiguration

1. Otvaranje kopije datoteka skripte **EngagementConfiguration** iz mape SDK i ažuriranje na **IOS\_VEZA\_NIZ** s nizom za povezivanje ste nabavili ranije Azure portal.  

    ![][73]

2. Spremite datoteku. 

###<a name="configure-the-app-for-basic-tracking"></a>Konfiguriranje aplikacija za osnovni praćenja

1. Otvaranje kopije skripte **PlayerController** priložiti objekt reproduktora za uređivanje. 

2. Dodajte sljedeće pomoću naredbe:

        using Microsoft.Azure.Engagement.Unity;

3. Dodajte sljedeće da biste na `Start()` način
    
        EngagementAgent.Initialize();
        EngagementAgent.StartActivity("Home");

###<a name="deploy-and-run-the-app"></a>Implementacija i pokretanje aplikacija

1. Priključite uređaju sa sustavom iOS na vašem računalu. 

2. Otvaranje kopije **datoteke -> postavke za sastavljanje** 

    ![][40]

3. Odaberite **iOS** , a zatim kliknite na **Platformi promjenu**

    ![][41]

    ![][42]

4. Kliknite **postavke reproduktora** i Navedite valjani identifikator paket. 

    ![][53]

5. Na kraju kliknite **Stvaranje i pokretanje**

    ![][54]

6. Možda ćete morati unijeti naziv mape za spremanje paketa iOS. 

    ![][43]

7. Ako sve dolazi s precizno će se prevesti projekta i otvarajte ga prema gore na XCode aplikacije. 

8. Provjerite je li **grupirajte identifikator** točan u projektu.  

    ![][75]

10. Pokrenite aplikaciju u XCode tako da je paket implementiran na uređaju povezani i trebali biste vidjeti vaše igra jedinstvo na telefonu! 

##<a id="monitor"></a>Povezivanje aplikacije pomoću nadzora u stvarnom vremenu

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Omogućivanje automatske obavijesti i poruka u aplikaciji

Radnje Mobile omogućuje interakciju s korisnicima i DOSEGNE s automatske obavijesti i u aplikaciji poruka u kontekstu kampanja. U ovom modul naziva RAZGOVOR na portalu Mobile radnje.
Ne morate učiniti sve dodatne konfiguracijske u aplikaciji za primanje obavijesti, a zatim je instalacijski program za njega.

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- Images. -->
[40]: ./media/mobile-engagement-unity-ios-get-started/40.png
[41]: ./media/mobile-engagement-unity-ios-get-started/41.png
[42]: ./media/mobile-engagement-unity-ios-get-started/42.png
[43]: ./media/mobile-engagement-unity-ios-get-started/43.png
[53]: ./media/mobile-engagement-unity-ios-get-started/53.png
[54]: ./media/mobile-engagement-unity-ios-get-started/54.png
[70]: ./media/mobile-engagement-unity-ios-get-started/70.png
[71]: ./media/mobile-engagement-unity-ios-get-started/71.png
[72]: ./media/mobile-engagement-unity-ios-get-started/72.png
[73]: ./media/mobile-engagement-unity-ios-get-started/73.png
[74]: ./media/mobile-engagement-unity-ios-get-started/74.png
[75]: ./media/mobile-engagement-unity-ios-get-started/75.png
