<properties
    pageTitle="Azure Mobile radnje iOS Integracija SDK | Microsoft Azure"
    description="Najnovija ažuriranja i postupke za iOS SDK za Azure Mobile radnje"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="piyushjo" />

#<a name="how-to-integrate-engagement-on-ios"></a>Kako integrirati radnje sustavu iOS

> [AZURE.SELECTOR]
- [Univerzalni za Windows](mobile-engagement-windows-store-integrate-engagement.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-integrate-engagement.md)

U ovom postupku opisano najjednostavniji način da biste aktivirali analize radnje korisnika te praćenje funkcije u aplikaciji za iOS.

Radnje SDK zahtijeva iOS6 + i Xcode 8: implementacija ciljne aplikacije mora biti najmanje iOS 6.

> [AZURE.NOTE]
> Ako ste doista ovise o XCode 7 možda koristi [iOS v3.2.4 SDK radnje](https://aka.ms/r6oouh). Ima li poznatih problema na razgovor modul ove prethodne verzije dok se izvodi na uređajima sa sustavom iOS 10 potražite u članku [Modul Integracija razgovor](mobile-engagement-ios-integrate-engagement-reach.md) više pojedinosti. Ako odlučite koristiti SDK v3.2.4, a zatim samo preskočiti na `UserNotifications.framework` uvoz u sljedećem koraku.

Sljedeći koraci nisu dovoljno da biste aktivirali izvješća zapisnika potrebne za izračun svih statističkih podataka vezanih uz korisnika, sesije, aktivnosti, ruši i Technicals. Izvješće zapisnika potrebne za izračunavanje ostalih statističkih podataka kao što su događaji, pogrešaka i zadacima se mora poduzeti ručno pomoću radnje API-JA (pogledajte [kako koristiti dodatne radnje Mobile označavanje API-JA u aplikaciju za iOS](mobile-engagement-ios-use-engagement-api.md) jer te statistike aplikacije zavisne.

##<a name="embed-the-engagement-sdk-into-your-ios-project"></a>Ugrađivanje SDK radnje iOS projekta

- Preuzmite iOS SDK iz [ovdje](http://aka.ms/qk2rnj).

- Dodavanje SDK radnje u projekt iOS: u Xcode, desnom tipkom miša kliknite na projektu i odaberite **"... dodati datoteke"** pa odaberite na `EngagementSDK` mapu.

- Radnje potrebna dodatna okviri za rad: u programu project explorer mrežu projekta i odaberite odgovarajuće cilj. Zatim otvorite karticu **"Sastavljanje faze"** i na izborniku **"binarni s biblioteka veza"** dodavanje tih okviri:

    -   `UserNotifications.framework`– Postavljanje kao veza`Optional`
    -   `AdSupport.framework`– Postavljanje kao veza`Optional`
    -   `SystemConfiguration.framework`
    -   `CoreTelephony.framework`
    -   `CFNetwork.framework`
    -   `CoreLocation.framework`
    -   `libxml2.dylib`

> [AZURE.NOTE] AdSupport framework mogu se ukloniti. Radnje mora ovaj okvir da biste prikupili na IDFA. Međutim, može se onemogućiti zbirke IDFA \<ios – sdk-radnje-idfa\> radi usklađivanja s novog Apple pravilnika o ovom ID-a.

##<a name="initialize-the-engagement-sdk"></a>Pokretanje radnje SDK

Ćete morati izmijeniti svog delegata aplikacije:

-   Pri vrhu datoteke implementaciju, Uvoz agent radnje:

        [...]
        #import "EngagementAgent.h"

-   Pokretanje radnje unutar metodu '**applicationDidFinishLaunching:**"ili"**aplikacije: didFinishLaunchingWithOptions:**":

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
          [...]
          [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];
          [...]
        }

##<a name="basic-reporting"></a>Osnovni izvješćivanje o pogreškama

### <a name="recommended-method-overload-your-uiviewcontroller-classes"></a>Preporučeni način: preopterećenja vaše `UIViewController` klase

Da biste aktivirali izvješća sve zapisnike potrebnih radnje za izračunavanje korisnika, sesije, aktivnosti, ruši i tehničke Statistika, možete jednostavno napraviti sve svoje `UIViewController` podređenu klase nasljeđuju od na `EngagementViewController` klase (ista pravila za `UITableViewController`  - \> `EngagementTableViewController`).

**Bez radnje:**

    #import <UIKit/UIKit.h>

    @interface Tab1ViewController : UIViewController<UITextFieldDelegate> {
      UITextField* myTextField1;
      UITextField* myTextField2;
    }

    @property (nonatomic, retain) IBOutlet UITextField* myTextField1;
    @property (nonatomic, retain) IBOutlet UITextField* myTextField2;

**S mogućnostima:**

    #import <UIKit/UIKit.h>
    #import "EngagementViewController.h"

    @interface Tab1ViewController : EngagementViewController<UITextFieldDelegate> {
      UITextField* myTextField1;
      UITextField* myTextField2;
    }

    @property (nonatomic, retain) IBOutlet UITextField* myTextField1;
    @property (nonatomic, retain) IBOutlet UITextField* myTextField2;

### <a name="alternate-method-call-startactivity-manually"></a>Alternativni metoda: poziva `startActivity()` ručno

Ako ne možete ili ne želite preopterećenja vaše `UIViewController` klase, umjesto toga možete pokrenuti aktivnosti tako da nazovete `EngagementAgent`, izravno metode.

> [AZURE.IMPORTANT] IOS SDK automatski poziva na `endActivity()` način prilikom zatvaranja. Stoga je *vrlo* preporučuje se da biste uputili poziv na `startActivity` način kad god se aktivnosti korisnika promijeniti i na *NIKAD* poziva na `endActivity` način, budući da nazovete podršku za ovu metodu navodi trenutna sesija da biste ga je prekinuti.

##<a name="location-reporting"></a>Izvješćivanje o pogreškama mjesto

Uvjeti pružanja usluge za Apple ne dopuštaju za korištenje mjesto praćenja samo u svrhu Statistički podaci o aplikacijama. Dakle, preporučuje se da biste omogućili mjesto izvješća samo ako aplikacija i koristiti mjesto praćenja nekog drugog razloga.

Počevši s operacijskim sustavom iOS 8, morate navesti opis kako aplikaciju koristi mjesto servisa postavljanjem niz ključ [NSLocationWhenInUseUsageDescription] ili [NSLocationAlwaysUsageDescription] u datoteci Info.plist pokrenite aplikaciju. Ako želite mjesto izvješća u pozadini s mogućnostima, dodajte ključ NSLocationAlwaysUsageDescription. U svim ćete drugim slučajevima, dodajte ključ NSLocationWhenInUseUsageDescription.

### <a name="lazy-area-location-reporting"></a>Izvješćivanje o pogreškama drži područje mjesto

Izvješćivanje o pogreškama drži područje mjesto omogućuje da biste prijavili država, regija i kraj povezane s uređajima. Ovu vrstu izvješćivanja o mjestu koristi samo mrežna mjesta (na temelju ID-a u ćeliju ili Wi-Fi veze). Područje uređaj prijavljuje najviše jednom po sesiji. U GPS nikada niste koristili i ovu vrstu izvješća mjesto ima vrlo mali (ne želite reći bez) utjecaj na baterija.

Prijavljenim područja se koriste za izračun geografske statistike o korisnika, sesije, događaji i pogreške. Možete također se koriste kao kriterij u razgovor kampanja. Zadnji poznati područje potrebnom za uređaj mogu biti dohvaćeni zahvaljujući [API uređaja].

Da biste omogućili drži područje mjesto izvješćivanja, dodajte sljedeći redak nakon Inicijalizacija agent radnje:

    - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
    {
      [...]
      [[EngagementAgent shared] setLazyAreaLocationReport:YES];
      [...]
    }

### <a name="real-time-location-reporting"></a>Izvješćivanje o mjestu u stvarnom vremenu

Izvješćivanje o mjestu u stvarnom vremenu omogućuje da biste prijavili zemljopisnoj širini i dužini povezan s uređajima. Prema zadanim postavkama ovu vrstu izvješćivanja o mjestu koristi samo mrežna mjesta (na temelju ID-a u ćeliju ili Wi-Fi veze), a prijavljivanje aktivan samo kad je program pokrenut u planu (odnosno tijekom sesije).

Mjesta u stvarnom vremenu trenutno *ne* koristi za izračun statistike. Svoj razlog samo je omogućiti korištenje zemlj fencing stvarnom vremenu \<razgovor publike geofencing\> kriterij u razgovor kampanja.

Da biste omogućili reporting lokacija u stvarnom vremenu, dodajte sljedeći redak nakon Inicijalizacija agent radnje:

    [[EngagementAgent shared] setRealtimeLocationReport:YES];

#### <a name="gps-based-reporting"></a>GPS temelji izvješćivanje o pogreškama

Prema zadanim postavkama, izvješćivanje lokacije u stvarnom vremenu koristi samo temelji mrežna. Da biste omogućili korištenje GPS temelji mjesta (koje su znatno preciznije), dodajte:

    [[EngagementAgent shared] setFineRealtimeLocationReport:YES];

#### <a name="background-reporting"></a>Izvješćivanje o pogreškama pozadine

Prema zadanim postavkama, izvješćivanje mjesto u stvarnom vremenu aktivan samo prilikom pokretanja aplikacije u planu (odnosno tijekom sesije). Da biste omogućili izvješćivanje i u pozadini, dodajte:

    [[EngagementAgent shared] setBackgroundRealtimeLocationReport:YES withLaunchOptions:launchOptions];

> [AZURE.NOTE] Kada aplikacija izvodi u pozadini, samo mreže temelji mjesta prijavljene, čak i ako je omogućeno u GPS.

Implementacija Ova funkcija podići [startMonitoringSignificantLocationChanges] kada aplikacija ulazi u pozadini. Imajte na umu da je će automatski ponovno pokrenuti aplikaciju u pozadini ako stigne novi događaj mjesto.

##<a name="advanced-reporting"></a>Dodatna izvješća

Po želji, ako želite da biste prijavili aplikacije određene događaje, pogrešaka i zadacima, morate koristiti API radnje pomoću metode od na `EngagementAgent` predmete. Objekta te klase mogu biti dohvaćeni tako da nazovete podršku za `[EngagementAgent shared]` statične način.

API radnje omogućuje korištenje svih naprednih mogućnosti radnje korisnika i detaljne se u članku korištenje radnje API na iOS (dobro, kao i u tehničku dokumentaciju o na `EngagementAgent` klase).

##<a name="disable-idfa-collection"></a>Onemogućivanje IDFA zbirke

Prema zadanim postavkama radnje će koristiti [IDFA] identificirati samo korisnika. No ako ne koristite oglašavanje na neko drugo mjesto u aplikaciji, koji mogu biti odbio pregled postupka App Store. Zbirka IDFA može se onemogućiti tako da dodate preprocessor makronaredbe `ENGAGEMENT_DISABLE_IDFA` u datoteci pch (ili u `Build Settings` aplikacije). To će Provjerite postoji li bez reference na `ASIdentifierManager`, `advertisingIdentifier` ili `isAdvertisingTrackingEnabled` u sastavljanje aplikacije.

Integracija u datoteci **prefix.pch** :

    #define ENGAGEMENT_DISABLE_IDFA
    ...

Možete provjeriti da zbirke IDFA ispravno onemogućeno u aplikaciji potvrđivanjem zapisnike test radnje. U odjeljku Integracija testiranje\<ios – sdk-radnje-test-idfa\> dokumentaciju za dodatne informacije.

##<a name="disable-log-reporting"></a>Onemogućivanje izvještavanja o zapisnika

### <a name="using-a-method-call"></a>Pomoću metode poziva

Ako želite da se radnje prestanu slati zapisnike, da biste uputili poziv:

    [[EngagementAgent shared] setEnabled:NO];

U ovom poziv je stalni: koristi `NSUserDefaults` za pohranu informacija.

Možete omogućiti zapisnika izvješćivanja ponovno tako da nazovete istu funkciju s `YES`.

### <a name="integration-in-your-settings-bundle"></a>Integracija u paket vaše postavke

Umjesto da nazovete podršku za tu funkciju, možete i integrirati tu postavku izravno u postojeće `Settings.bundle` datoteku. Niz `engagement_agent_enabled` moraju se koristiti kao na identifikator preferenca i mora biti povezan skretnice Uključivanje i isključivanje (`PSToggleSwitchSpecifier`).

Sljedeći primjer `Settings.bundle` prikazuje kako implementirati je:

    <dict>
        <key>PreferenceSpecifiers</key>
        <array>
            <dict>
                <key>DefaultValue</key>
                <true/>
                <key>Key</key>
                <string>engagement_agent_enabled</string>
                <key>Title</key>
                <string>Log reporting enabled</string>
                <key>Type</key>
                <string>PSToggleSwitchSpecifier</string>
            </dict>
        </array>
        <key>StringsTable</key>
        <string>Root</string>
    </dict>

<!-- URLs. -->
[Uređaj API-JA]: http://go.microsoft.com/?linkid=9876094
[NSLocationWhenInUseUsageDescription]:https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26
[NSLocationAlwaysUsageDescription]:https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW18
[startMonitoringSignificantLocationChanges]:http://developer.apple.com/library/IOs/#documentation/CoreLocation/Reference/CLLocationManager_Class/CLLocationManager/CLLocationManager.html#//apple_ref/occ/instm/CLLocationManager/startMonitoringSignificantLocationChanges
[IDFA]:https://developer.apple.com/library/ios/documentation/AdSupport/Reference/ASIdentifierManager_Ref/ASIdentifierManager.html#//apple_ref/occ/instp/ASIdentifierManager/advertisingIdentifier
