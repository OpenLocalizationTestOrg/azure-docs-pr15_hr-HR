<properties
    pageTitle="Početak rada s Azure mobilne radnje za Cordova/Phonegap"
    description="Saznajte kako koristiti radnje Mobile Azure s analize i automatske obavijesti za Cordova/Phonegap aplikacije."
    services="mobile-engagement"
    documentationCenter="Mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-phonegap"
    ms.devlang="js"
    ms.topic="hero-article" 
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-cordovaphonegap"></a>Početak rada s Azure mobilne radnje za Cordova/Phonegap

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

U ovoj se temi objašnjava korištenje Azure Mobile radnje za razumijevanje Upotreba aplikacije i automatske obavijesti poslati segmentiranog korisnicima za mobilnu aplikaciju razvio s Cordova.

U ovom ćete praktičnom vodiču smo će stvoriti prazan Cordova aplikaciju putem Mac i integrirati SDK radnje Mobile. Prikuplja osnovne analize podatke i prima automatske obavijesti pomoću Apple automatske obavijesti sustava (APN-ove) za iOS i razmjenu Google Cloud (GCM) za Android. Ne možemo će implementirati za iOS ili Android uređaj za testiranje. 

> [AZURE.NOTE] Da biste dovršili ovaj Praktični vodič, morate imati račun za Azure active poruka. Ako nemate račun, možete stvoriti besplatnu probnu računa u samo nekoliko minuta. Detalje potražite u članku [Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-cordova-get-started).

Pomoću ovog praktičnog vodiča potrebno je sljedeće:

+ XCode koje možete instalirati iz trgovine aplikacija za Mac (za implementaciju u iOS)
+ [Android SDK & Emulator](http://developer.android.com/sdk/installing/index.html) (za implementaciju u Android)
+ Automatske obavijesti certifikat (.p12) možete dobiti od jabuka Razvojni centar za APN-ove
+ Broj projekta GCM koje možete dobiti od konzole za razvojne inženjere Google za GCM
+ [Dodatak za Cordova mobilne radnje](https://www.npmjs.com/package/cordova-plugin-ms-azure-mobile-engagement)

> [AZURE.NOTE] Dodatak za Cordova na [Github](https://github.com/Azure/azure-mobile-engagement-cordova) možete pronaći izvornog koda i datoteke ReadMe

##<a id="setup-azme"></a>Postavljanje mobilnih radnje Cordova aplikacije

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Povezivanje aplikacije s pozadinskom Mobile radnje

Pomoću ovog praktičnog vodiča predstavlja na "Osnovni Integracija" što je s minimalnim postaviti potrebne za prikupljanje podataka i slanje automatske obavijesti. 

S Cordova da bismo pokazali integraciju sa servisom ćemo će stvoriti osnovni aplikacije:

###<a name="create-a-new-cordova-project"></a>Stvaranje novog Cordova projekta

1. Pokrenite *Terminal* prozora na računalu Mac i upišite sljedeće koji će se stvoriti novi projekt Cordova iz zadanog predloška. Provjerite je li objavljivanje profila na koncu koristite za implementaciju aplikacije za iOS ustanova upotrebljava 'com.mycompany.myapp' kao ID aplikacije 

        $ cordova create azme-cordova com.mycompany.myapp
        $ cd azme-cordova

2. Izvođenje sljedeće da biste konfigurirali projekta za **iOS** i pokretanje u iOS Simulator:

        $ cordova platform add ios 
        $ cordova run ios

3. Izvesti sljedeće radnje konfiguracije projekt za **Android** i pokretanje u Android emulator. Provjerite imaju li postavki Android SDK Emulator svoj cilj kao Google API-ji (Google Inc.) s Procesor / ABI kao ARM API-ji za Google.  

        $ cordova platform add android
        $ cordova run android

4.  Dodavanje dodatka Cordova konzolu. 

        $ cordova plugin add cordova-plugin-console 

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Povezivanje aplikacije s pozadinskom Mobile radnje

1. Instalirajte modul Azure Mobile radnje Cordova tijekom pružanja vrijednostima varijable da biste konfigurirali dodatak:

        cordova plugin add cordova-plugin-ms-azure-mobile-engagement    
            --variable AZME_IOS_CONNECTION_STRING=<iOS Connection String> 
            --variable AZME_IOS_REACH_ICON=... (icon name WITH extension) 
            --variable AZME_ANDROID_CONNECTION_STRING=<Android Connection String> 
            --variable AZME_ANDROID_REACH_ICON=... (icon name WITHOUT extension)       
            --variable AZME_ANDROID_GOOGLE_PROJECT_NUMBER=... (From your Google Cloud console for sending push notifications) 
            --variable AZME_ACTION_URL =... (URL scheme which triggers the app for deep linking)
            --variable AZME_ENABLE_NATIVE_LOG=true|false
            --variable AZME_ENABLE_PLUGIN_LOG=true|false

*Android dosegne ikona* : mora biti naziv resursa bez sve proširenje ni drawable prefiks (ex: mynotificationicon), i datoteku ikone morate kopirati u android projekta (platforme/android/res/drawable)

*iOS dosegne ikona* : mora biti naziv resursa s njezin datotečni nastavak (ex: mynotificationicon.png), a ikona datoteke mora biti dodan u projektu iOS s XCode (pomoću izbornika za dodavanje datoteke)

##<a id="monitor"></a>Omogućivanje nadzora u stvarnom vremenu

1. U programu project Cordova - uređivanje **www/js/index.js** da biste dodali primitku poziv Mobile radnje deklarirati novu aktivnost jednom *deviceReady* događaj.

         onDeviceReady: function() {
                Engagement.startActivity("myPage",{});
            }

2. Pokrenite aplikaciju:
        
    - **Za iOS**
    
        U `Terminal` prozor pokretanje aplikacije u novu instancu Simulator izvršavanjem sljedeće:

            cordova run ios

    - **Za Android**
        
        U `Terminal` prozor pokretanje aplikacije u novu instancu emulator izvršavanjem sljedeće:

            cordova run android

3. Vidjet ćete sljedeće u konzole zapisnika:

        [Engagement] Agent: Session started
        [Engagement] Agent: Activity 'myPage' started
        [Engagement] Connection: Established
        [Engagement] Connection: Sent: appInfo
        [Engagement] Connection: Sent: startSession
        [Engagement] Connection: Sent: activity name='myPage'

##<a id="monitor"></a>Povezivanje aplikacije pomoću nadzora u stvarnom vremenu

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Omogućivanje automatske obavijesti i poruka u aplikaciji

Radnje Mobile omogućuje interakciju s korisnicima pomoću automatske obavijesti i u aplikaciji poruka u kontekstu kampanja. U ovom modul naziva RAZGOVOR na portalu Mobile radnje.
U sljedećim se odjeljcima će se instalacija aplikacije primati.

###<a name="configure-push-credentials-for-mobile-engagement"></a>Konfiguriranje automatske vjerodajnica za mobilne radnje

Da biste omogućili Mobile radnje da biste poslali automatske obavijesti u vaše ime, morate dopustiti pristup Apple iOS certifikat ili ključ za API GCM poslužitelja. 
    
1. Otvorite portal Mobile radnje. Provjerite je li se nalazite u aplikaciji ne možemo koristite za taj projekt, a zatim kliknite gumb **Engage** pri dnu:
    
    ![][1]
    
2. Na stranici Postavke će krenuti portalom radnje. There kliknite odjeljak koji **Izvorni automatske** :
    
    ![][2]

3. Konfiguriranje iOS ključ za API certifikata/GCM poslužitelja

    **[iOS]**

    na. Odaberite vaše .p12, prenijeti i upišite svoju lozinku:
    
    ![][3]

    **[Android]**

    na. Kliknite ikonu uređivanje ispred **Ključ za API -JA** u odjeljku postavke GCM i skočni prozor koji prikazuje prema gore, zalijepite ključ poslužitelja GCM i kliknite **u redu**. 
        
    ![][4]

###<a name="enable-push-notifications-in-the-cordova-app"></a>Omogućivanje automatske obavijesti u aplikaciji Cordova

Uređivanje **www/js/index.js** da biste dodali poziv na mobilni radnje da biste zatražili automatske obavijesti i deklarirati rukovatelja:

     onDeviceReady: function() {
           Engagement.initializeReach(  
                // on OpenUrl  
                function(_url) {   
                alert(_url);   
                });  
            Engagement.startActivity("myPage",{});  
        }

###<a name="run-the-app"></a>Pokrenite aplikaciju

**[iOS]**

1. Koristit ćemo XCode omogućuje stvaranje i implementacija aplikacije na uređaju da biste testirali automatske obavijesti jer iOS samo omogućuje slanje obavijesti za stvarni uređaj. Idite na mjesto na kojem je stvorena Cordova projektu, a zatim otvorite **...\platforms\ios** mjesto. Otvaranje kopije datoteke nativni .xcodeproj u XCode. 
    
2. Stvorite i implementirajte Cordova aplikacije na uređaju sa sustavom iOS pomoću računa koji ima na dodjele resursa profil koji sadrži certifikat koji ste upravo prenijeli portal radnje Mobile i aplikacije ID-a koji odgovara one koje ste naveli prilikom stvaranja aplikacije Cordova. Pročitajte članak *identifikator paket* u svoje **Resursi\*-info.plist** datoteke u XCode tako da odgovara prema gore. 

3. Vidjet ćete skočnog standardni iOS na uređaju koja kaže da aplikaciju zahtijeva dozvolu za slanje obavijesti. Dodjela dozvola. 

**[Android]**

Jednostavno možete koristiti u emulator pokretanje aplikacija za Android, kao što je GCM obavijesti podržani su na Android emulator. 

    cordova run android

##<a id="send"></a>Poslati obavijest aplikacije

Ćemo sada stvoriti jednostavan kampanje automatske obavijesti koja će se slati na automatske u aplikaciju za pokretanje na uređaju:

1. Idite na karticu **dosegne** portalu za mobilne radnje

2. Kliknite **Novu objavu** da biste stvorili kampanje pribadače

    ![][6]

3. Navedite unose da biste stvorili kampanje **[Android]**
    
    - Navedite **naziv** kampanje. 
    - Odaberite **Vrstu isporuke** kao *obavijest o sustavu* *jednostavne*
    - Odaberite **vrijeme isporuke** kao *"bilo koje vrijeme"*
    - Navedite **Naslov** za vaše obavijesti koji će biti prvi redak u pribadače.
    - Unesite **poruku** za vaše obavijesti koja će služiti kao tijelo poruke. 

    ![][11]

4. Navedite unose da biste stvorili kampanje **[iOS]**

    - Navedite **naziv** kampanje. 
    - Odaberite **vrijeme isporuke** kao *"iz aplikacije samo"*
    - Navedite **Naslov** za vaše obavijesti koji će biti prvi redak u automatske.
    - Unesite **poruku** za svoje obavijesti koja će služiti kao tijela poruke. 
 
    ![][12]

5. Pomaknite se prema dolje, a zatim u odjeljku sadržaja odaberite **samo obavijesti**

    ![][8]

6. [Neobavezni] Također pružaju akciju URL. Provjerite koristi li URL shema dobili prilikom konfiguriranja na dodatak **AZME\_PREUSMJERAVANJE\_URL** varijabla – primjerice *myapp://test*.  

7. Završite postavljanje osnovni kampanje moguće. Sada se pomaknuti prema dolje i kliknite gumb **Stvori** da biste spremili kampanje.

8. Na kraju **Aktiviraj** kampanje
    
    ![][10]

9. Automatske obavijesti na mobilnom uređaju ili emulator sada trebali biste vidjeti kao dio kampanje. 

##<a id="next-steps"></a>Daljnji koraci
[Pregled sve načine dostupno u sklopu Cordova Mobile radnje SDK](https://github.com/Azure/azure-mobile-engagement-cordova)

<!-- Images. -->

[1]: ./media/mobile-engagement-cordova-get-started/engage-button.png
[2]: ./media/mobile-engagement-cordova-get-started/engagement-portal.png
[3]: ./media/mobile-engagement-cordova-get-started/native-push-settings.png
[4]: ./media/mobile-engagement-cordova-get-started/api-key.png
[6]: ./media/mobile-engagement-cordova-get-started/new-announcement.png
[8]: ./media/mobile-engagement-cordova-get-started/campaign-content.png
[10]: ./media/mobile-engagement-cordova-get-started/campaign-activate.png
[11]: ./media/mobile-engagement-cordova-get-started/campaign-first-params-android.png
[12]: ./media/mobile-engagement-cordova-get-started/campaign-first-params-ios.png

