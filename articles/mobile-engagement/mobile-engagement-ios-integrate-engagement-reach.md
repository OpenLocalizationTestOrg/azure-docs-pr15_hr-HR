<properties
    pageTitle="Azure Mobile radnje iOS Integracija dosegne SDK | Microsoft Azure"
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

#<a name="how-to-integrate-engagement-reach-on-ios"></a>Kako integrirati radnje doći do sustavu iOS

Morate slijediti za integraciju postupak opisan u odjeljku [kako integrirati radnje na dokumentu iOS](mobile-engagement-ios-integrate-engagement.md) prije nego nastavite slijediti ovaj vodič.

Ove dokumentacije zahtijeva XCode 8. Ako ste doista ovise o XCode 7 možda koristi [iOS v3.2.4 SDK radnje](https://aka.ms/r6oouh). Ima li poznatih problema na ovu stariju verziju prilikom pokretanja na uređajima sa sustavom iOS 10: obavijesti sustava nisu actioned. Da biste to ispravili morat ćete provesti zastarjeli API `application:didReceiveRemoteNotification:` u svojoj aplikaciji delegatu na sljedeći način:

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [AZURE.IMPORTANT] **Ne preporučujemo to zaobilazno rješenje** kao takvo ponašanje možete promijeniti u verziju nadogradnje nadolazeće iOS (čak i pomoćne) jer je zastario iOS API-JA. Trebate prebaciti na XCode 8 čim.

### <a name="enable-your-app-to-receive-silent-push-notifications"></a>Omogućivanje aplikacije za primanje tihu automatske obavijesti

[AZURE.INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

##<a name="integration-steps"></a>Integracija korake

### <a name="embed-the-engagement-reach-sdk-into-your-ios-project"></a>Ugrađivanje radnje dosegne SDK iOS projekta

-   Dodajte sdk razgovor u projektu Xcode. U Xcode, idite na **projekta \> Dodaj projekt** , a zatim odaberite u `EngagementReach` mapu.

### <a name="modify-your-application-delegate"></a>Izmjena svog delegata aplikacije

-   Pri vrhu datoteke implementaciju, Uvoz modul dosegne radnje:

        [...]
        #import "AEReachModule.h"

-   Unutar način `applicationDidFinishLaunching:` ili `application:didFinishLaunchingWithOptions:`, stvorite modul razgovor i prenesite na postojeći redak radnje pokretanje:

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
          AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
          [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
          [...]

          return YES;
        }

-   Izmjena niz **"icon.png"** s nazivom slike koji želite koristiti kao ikona s obavijesti.
-   Ako želite koristiti mogućnost *vrijednost iskaznice ažuriranja* u razgovor kampanje ili ako želite koristiti izvorni automatske \<API kampanje SaaS/razgovor oblik/lokalnog automatske\> kampanje, morate omogućiti razgovor modul upravljanje iskaznice ikonu sam (bit će automatski poništite iskaznice aplikacije i vrati i vrijednosti pohranjene radnje svaki put kada se aplikacija nije pokrenuto ili foregrounded). To možete učiniti tako da dodate sljedeći redak nakon pokretanje modula za razgovor:

        [reach setAutoBadgeEnabled:YES];

-   Ako želite rukovati automatske razgovor podataka, morate omogućiti svog delegata aplikacija u skladu sa na `AEReachDataPushDelegate` protocol. Dodajte sljedeći redak nakon pokretanje modula za razgovor:

        [reach setDataPushDelegate:self];

-   Zatim možete implementirati metode `onDataPushStringReceived:` i `onDataPushBase64ReceivedWithDecodedBody:andEncodedBody:` u vašem delegatu aplikacije:

        -(BOOL)didReceiveStringDataPushWithCategory:(NSString*)category body:(NSString*)body
        {
           NSLog(@"String data push message with category <%@> received: %@", category, body);
           return YES;
        }

        -(BOOL)didReceiveBase64DataPushWithCategory:(NSString*)category decodedBody:(NSData *)decodedBody encodedBody:(NSString *)encodedBody
        {
           NSLog(@"Base64 data push message with category <%@> received: %@", category, encodedBody);
           // Do something useful with decodedBody like updating an image view
           return YES;
        }

### <a name="category"></a>Kategorija

Parametra kategorija nije obavezna, prilikom stvaranja kampanje automatske podatke i omogućuje vam da filtriranje podataka ih gura. To je korisno ako želite automatske različite od `Base64` podatke i želite da biste odredili njihove vrste prije nego što ih analize.

**Aplikacija je sada primati i prikaz sadržaja razgovor!**

##<a name="how-to-receive-announcements-and-polls-at-any-time"></a>Upute za primanje obavijesti i ankete u bilo kojem trenutku

Radnje možete poslati razgovor obavijesti krajnjim korisnicima u bilo kojem trenutku pomoću servisa Apple automatske obavijesti.

Da biste omogućili tu funkciju, morat ćete pripremiti aplikacija za Apple automatske obavijesti i izmjena svog delegata aplikacije.

### <a name="prepare-your-application-for-apple-push-notifications"></a>Priprema aplikacija za Apple automatske obavijesti

Slijedite vodič: [kako pripremiti aplikacije Apple automatske obavijesti](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6)

### <a name="add-the-necessary-client-code"></a>Dodavanje koda za potrebne klijenta

*Sada aplikacije registrirani certifikat automatske Apple imat će se u sučelju radnje.*

Ako ga ne obavlja već, morate registrirati aplikaciju primajte automatske obavijesti.

* Uvoz u `User Notification` okvir:

        #import <UserNotifications/UserNotifications.h>

* Dodavanje sljedeći redak prilikom pokretanja aplikacije (obično u `application:didFinishLaunchingWithOptions:`):

        if (NSFoundationVersionNumber >= NSFoundationVersionNumber_iOS_8_0)
        {
            if (NSFoundationVersionNumber > NSFoundationVersionNumber_iOS_9_x_Max)
            {
                [UNUserNotificationCenter.currentNotificationCenter requestAuthorizationWithOptions:(UNAuthorizationOptionBadge | UNAuthorizationOptionSound | UNAuthorizationOptionAlert) completionHandler:^(BOOL granted, NSError * _Nullable error) {}];
            }else
            {
                [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert)   categories:nil]];
            }
            [application registerForRemoteNotifications];
        }
        else
        {
            [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
        }

Zatim, potrebno je omogućuju radnje token uređaja vratio Apple poslužiteljima. To možete učiniti u način pod nazivom `application:didRegisterForRemoteNotificationsWithDeviceToken:` u vašem delegatu aplikacije:

    - (void)application:(UIApplication*)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData*)deviceToken
    {
        [[EngagementAgent shared] registerDeviceToken:deviceToken];
    }

Naposljetku, morate obavijestiti SDK radnje kada aplikacija primi udaljene obavijest. Da biste to učinili, nazovite metodu `applicationDidReceiveRemoteNotification:fetchCompletionHandler:` u vašem delegatu aplikacije:

    - (void)application:(UIApplication*)application didReceiveRemoteNotification:(NSDictionary*)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
    }

> [AZURE.NOTE] Gore navedeni način je uvedena u iOS 7. Ako birate iOS < 7, provjerite jeste li implementirati način `application:didReceiveRemoteNotification:` u delegat aplikacije i poziv `applicationDidReceiveRemoteNotification` na EngagementAgent prosljeđivanjem ništicu umjesto na `handler` argumenta:

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [AZURE.IMPORTANT] Prema zadanim postavkama, dosegne radnje kontrole na completionHandler. Ako želite ručno odgovoriti na `handler` blokirati u kodu, što prođete ništicu na `handler` argumenta i kontrola nakon dovršetka blokirati sami. Pogledajte na `UIBackgroundFetchResult` vrsta popis mogućih vrijednosti.


### <a name="full-example"></a>Potpuna primjer

Evo primjera cijelog integriranja:

    #pragma mark -
    #pragma mark Application lifecycle

    - (BOOL)application:(UIApplication*)application didFinishLaunchingWithOptions:(NSDictionary*)launchOptions
    {
      /* Reach module */
      AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
      [reach setAutoBadgeEnabled:YES];

      /* Engagement initialization */
      [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
      [[EngagementAgent shared] setPushDelegate:self];

      /* Views */
      [window addSubview:[tabBarController view]];
      [window makeKeyAndVisible];

      [application registerForRemoteNotificationTypes:UIRemoteNotificationTypeAlert|UIRemoteNotificationTypeBadge|UIRemoteNotificationTypeSound];
      return YES;
    }

    - (void)application:(UIApplication*)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData*)deviceToken
    {
      [[EngagementAgent shared] registerDeviceToken:deviceToken];
    }

    - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
    }

### <a name="if-you-have-your-own-unusernotificationcenterdelegate-implementation"></a>Ako imate UNUserNotificationCenterDelegate implementaciju

SDK ima vlastitu implementaciju UNUserNotificationCenterDelegate protokol. Koristi se tako da u SDK praćenje životnog ciklusa obavijesti o mogućnostima na uređajima sa sustavom na iOS 10 ili noviji. Ako u SDK otkrije svog delegata ga će koristiti vlastitu implementaciju jer se može biti samo jedna UNUserNotificationCenter delegat svaku aplikaciju. To znači da će imati logike radnje vlastite delegatu.

Da biste to postigli na dva načina.

Jednostavno po svog delegata za prosljeđivanje poziva SDK:

    #import <UIKit/UIKit.h>
    #import "EngagementAgent.h"
    #import <UserNotifications/UserNotifications.h>


    @interface MyAppDelegate : NSObject <UIApplicationDelegate, UNUserNotificationCenterDelegate>
    @end

    @implementation MyAppDelegate

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
    {
      // Your own logic.

      [[EngagementAgent shared] userNotificationCenterWillPresentNotification:notification withCompletionHandler:completionHandler]
    }

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse:(UNNotificationResponse *)response withCompletionHandler:(void(^)())completionHandler
    {
      // Your own logic.

      [[EngagementAgent shared] userNotificationCenterDidReceiveNotificationResponse:response withCompletionHandler:completionHandler]
    }
    @end

Ili nasljeđuju na `AEUserNotificationHandler` klase

    #import "AEUserNotificationHandler.h"
    #import "EngagementAgent.h"

    @interface CustomUserNotificationHandler :AEUserNotificationHandler
    @end

    @implementation CustomUserNotificationHandler

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
    {
      // Your own logic.

      [super userNotificationCenter:center willPresentNotification:notification withCompletionHandler:completionHandler];
    }

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse: UNNotificationResponse *)response withCompletionHandler:(void(^)())completionHandler
    {
      // Your own logic.

      [super userNotificationCenter:center didReceiveNotificationResponse:response withCompletionHandler:completionHandler];
    }

    @end

> [AZURE.NOTE] Možete odrediti hoće li obavijest dolazi s mogućnostima ili ne prosljeđivanjem njegov `userInfo` rječnik agenta `isEngagementPushPayload:` klase način.

##<a name="how-to-customize-campaigns"></a>Kako prilagoditi kampanje

### <a name="notifications"></a>Obavijesti

Postoje dvije vrste obavijesti: obavijesti o sustavu i u aplikaciji.

Obavijesti sustava rješava iOS, a nije moguće prilagoditi.

Obavijesti u aplikaciji vrše prikaza koji se dinamički dodaje trenutni prozor aplikacije. To se naziva obavijesti preklapanje. Obavijesti prekrivanja odlični su za brzo Integracija jer ne trebaju izmjenu bilo kojem prikazu u aplikaciji.

#### <a name="layout"></a>Raspored

Da biste promijenili izgled obavijesti u aplikaciji, možete jednostavno izmijeniti datoteku `AENotificationView.xib` vašim potrebama, ukoliko zadržati oznaka vrijednosti i vrste postojeće subviews.

Obavijesti u aplikaciji, po zadanom se prikazuju pri dnu zaslona. Ako želite da se prikazuju na vrhu zaslona, Uredi na navedeni `AENotificationView.xib` i promijenite u `AutoSizing` svojstvo glavni prikaz tako da mogu biti zadržane pri vrhu njegov superview.

#### <a name="categories"></a>Kategorije

Kada mijenjate navedeni izgleda, izmijenite izgleda sve obavijesti. Kategorije omogućuju da odredite raznim ciljnim izgleda (vjerojatno ponašanja) za obavijesti. Kategorije možete navesti prilikom stvaranja kampanje razgovor. Imajte na umu da kategorije omogućuju vam prilagodbu najave i ankete, koji je opisan u nastavku ovog dokumenta.

Da biste registrirali rukovatelja kategorija za obavijesti, morate dodati pozivu kada je pokrenut modul za razgovor.

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerNotifier:myNotifier forCategory:@"my_category"];
    ...

`myNotifier`mora biti instancu objekta koji je sukladan s protokol `AENotifier`.

Možete implementirati metode protokol za sebe ili možete odabrati reimplement postojeće predmete `AEDefaultNotifier` koji već izvodi većinu posla.

Ako, na primjer, ako želite ponovno definirati prikaz obavijesti za određenu kategoriju, slijedite ovaj primjer:

    #import "AEDefaultNotifier.h"
    #import "AENotificationView.h"
    @interface MyNotifier : AEDefaultNotifier
    @end

    @implementation MyNotifier

    -(NSString*)nibNameForCategory:(NSString*)category
    {
      return "MyNotificationView";
    }

    @end

U ovom se primjeru jednostavne kategorije pretpostavlja da ste datoteku pod nazivom `MyNotificationView.xib` u vašem paket glavnog računala. Ako je način ne može pronaći odgovarajući `.xib`, neće se prikazati obavijest i radnje će izlazne poruke na konzoli sustava.

Navedeni nib datoteke moraju poštovati sljedeća pravila:

-   Samo smiju sadržavati jedan prikaz.
-   Subviews moraju biti iste vrste kao one unutar datoteke navedene nib pod nazivom`AENotificationView.xib`
-   Subviews mora imati isti oznake kao one unutar na navedeni nib datoteku pod nazivom`AENotificationView.xib`

> [AZURE.TIP] Samo kopirajte navedeni nib datoteku, pod nazivom `AENotificationView.xib`, i započeli rad iz nje. No pripazite da, prikaz u ovoj datoteci nib je povezan klasa `AENotificationView`. Klase redefined metodu `layoutSubViews` premještanje i promjena veličine njegov subviews ovisnosti o kontekstu. Preporučujemo vam da biste zamijenili pomoću programa `UIView` ili prilagođeni prikaz klasu.

Ako vam je potrebna detaljnije prilagođavanja obavijesti (Ako želite primjerice da biste učitali prikaza izravno iz kod), preporučuje se da biste razmotrite kod i predmete dokumentacija navedeni izvor od `Protocol ReferencesDefaultNotifier` i `AENotifier`.

Imajte na umu da možete koristiti isti obavijesti za više kategorija.

Možete redefined i obavijesti zadani ovako:

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerNotifier:myNotifier forCategory:kAEReachDefaultCategory];

##### <a name="notification-handling"></a>Rukovanje obavijesti

Kada koristite zadane kategorije, neki postupci za životni ciklus nazivaju na na `AEReachContent` objekt Statistika izvješća i ažurirati stanje kampanje:

-   Kada u aplikaciji, koji se prikaže obavijest u `displayNotification` način zove (koji izvješća Statistika) tako da `AEReachModule` ako `handleNotification:` vraća `YES`.
-   Ako nehotice se obavijesti, na `exitNotification` pod nazivom način statistike prijavljuje i dalje kampanje sada se obrađuju.
-   Ako kliknete obavijesti `actionNotification` je pod nazivom statistike prijavljuje i pridruženi akcija provodi.

Ako svoju implementaciju sustava `AENotifier` zaobilazi zadano ponašanje moram nazvati ovih metoda životnog ciklusa tako da sami. Sljedeći primjeri pokazuju nekim slučajevima gdje je zadano ponašanje zaobići:

-   Ne proširi `AEDefaultNotifier`, npr implementirana kategorija rukovanje ispočetka.
-   Koje overrode `prepareNotificationView:forContent:`, ne zaboravite da biste mapirali barem `onNotificationActioned` ili `onNotificationExited` na neki od vašeg U.I kontrole.

> [AZURE.WARNING] Ako `handleNotification:` throws iznimku, sadržaj se briše i `drop` je pod nazivom u statistici to prijavljuje i dalje kampanje sada se obrađuju.

#### <a name="include-notification-as-part-of-an-existing-view"></a>Uključite obavijesti kao dio postojećeg prikaza

Prekrivanja su odlične za brzo integraciju, ali je ponekad nije praktično ili može imati neželjene popratne pojave.

Ako niste zadovoljni sustava preklapanja u nekim prikaza, možete ga prilagoditi za tih prikaza.

Možete odabrati da biste obuhvatili naše obavijesti izgleda postojećih prikaza. Da biste to učinili, postoji dva stilovi implementacije:

1.  Dodavanje prikaza obavijesti pomoću alata za izradu sučelja

    -   Otvaranje *sučelja sastavljača*
    -   Postavite 320 x 60 (ili 768 x 60 ako ste na iPad) `UIView` mjesto na koje želite da se pojavi obavijest
    -   Postavite vrijednost oznake za ovaj prikaz: **36822491**

2.  Dodajte programski prikaz obavijesti. Kada je pokrenuta prikaza, jednostavno dodajte sljedeći kod:

        UIView* notificationView = [[UIView alloc] initWithFrame:CGRectMake(0, 0, 320, 60)]; //Replace x and y coordinate values to your needs.
        notificationView.tag = NOTIFICATION_AREA_VIEW_TAG;
        [self.view addSubview:notificationView];

`NOTIFICATION_AREA_VIEW_TAG`Akcija makronaredbe možete pronaći u `AEDefaultNotifier.h`.

> [AZURE.NOTE] Obavijesti zadani automatski otkriva izgleda obavijesti uključen je u tom prikazu te će dodati pomoću preklapanja za njega.

### <a name="announcements-and-polls"></a>Objava i ankete

#### <a name="layouts"></a>Izgledi

Možete izmijeniti datoteke `AEDefaultAnnouncementView.xib` i `AEDefaultPollView.xib` pod uvjetom da zadržite oznaka vrijednosti i vrste postojeće subviews.

#### <a name="categories"></a>Kategorije

##### <a name="alternate-layouts"></a>Zamjenski izgledi

Kao što su obavijesti, kategorija s kampanjom može se koristiti da bi zamjenski rasporeda za najave i ankete.

Da biste stvorili kategoriju obavijest, proširivanje **AEAnnouncementViewController** i registrirati kada je pokrenuta modul za razgovor:

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerAnnouncementController:[MyCustomAnnouncementViewController class] forCategory:@"my_category"];

> [AZURE.NOTE] Prilikom svakog korisnika kliknite obavijest o za obavijest s kategorijom "Moje\_kategorija", upravljaču registrirani prikaz (u tom slučaju `MyCustomAnnouncementViewController`) će pokrenuti tako da nazovete metodu `initWithAnnouncement:` i prikaz dodat će se trenutni prozor aplikacije.

U vašoj implementaciji sustava na `AEAnnouncementViewController` predmete morat ćete čitati svojstvo `announcement` Inicijalizacija vaše subviews. Razmislite o primjeru u nastavku, gdje dva prikazana su pokrenuti pomoću `title` i `body` svojstva na `AEReachAnnouncement` klasa:

    -(void)loadView
    {
        [super loadView];

        UILabel* titleLabel = [[UILabel alloc] initWithFrame:CGRectMake(10, 20, 300, 60)];
        titleLabel.font = [UIFont systemFontOfSize:32.0];
        titleLabel.text = self.announcement.title;

        UILabel* bodyLabel = [[UILabel alloc] initWithFrame:CGRectMake(10, 20, 300, 60)];
        bodyLabel.font = [UIFont systemFontOfSize:24.0];
        bodyLabel.text = self.announcement.body;

        [self.view addSubview:titleLabel];
        [self.view addSubview:bodyLabel];
    }

Ako ne želite učitati svoje prikaze po sebi, ali samo želite ponovno koristiti zadani objava prikaza izgled, možete jednostavno napraviti upravljaču prilagođenog prikaza proširuje navedene klase `AEDefaultAnnouncementViewController`. U tom slučaju dupliciranje datoteke nib `AEDefaultAnnouncementView.xib` i preimenovati tako da mogu se učitati po upravljaču prilagođenog prikaza (za kontroler pod nazivom `CustomAnnouncementViewController`, nazovite datoteku nib `CustomAnnouncementView.xib`).

Da biste zamijenili zadana kategorija objave, jednostavno registrirati upravljaču prilagođenog prikaza za kategorije definirane u `kAEReachDefaultCategory`:

    [reach registerAnnouncementController:[MyCustomAnnouncementViewController class] forCategory:kAEReachDefaultCategory];

Ankete moguće je prilagoditi na isti način:

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerPollController:[MyCustomPollViewController class] forCategory:@"my_category"];

Ovaj put, na navedeni `MyCustomPollViewController` morate proširiti `AEPollViewController`. Ili možete odabrati da biste proširili s kontrolerom zadane: `AEDefaultPollViewController`.

> [AZURE.IMPORTANT] Nemojte zaboraviti da biste uputili poziv ili `action` (`submitAnswers:` za kontrolera prikaz prilagođene ankete) ili `exit` način prije nego što je nehotice kontrolerom prikaz. U suprotnom Statistika neće biti poslana (odnosno nema analize na kampanje) i dodatne važnije sljedeći kampanje neće biti obaviješteni dok ponovnog pokretanja postupka aplikacije.

##### <a name="implementation-example"></a>Primjer implementacije

U ovoj implementaciji u prikazu prilagođene objava učita iz vanjske xib datoteke.

Kao što su za prilagodbu napredne obavijesti, preporučuje se možete pregledati na izvornom kodu standardne implementacije.

`CustomAnnouncementViewController.h`

    //Interface
    @interface CustomAnnouncementViewController : AEAnnouncementViewController {
      UILabel* titleLabel;
      UITextView* descTextView;
      UIWebView* htmlWebView;
      UIButton* okButton;
      UIButton* cancelButton;
    }

    @property (nonatomic, retain) IBOutlet UILabel* titleLabel;
    @property (nonatomic, retain) IBOutlet UITextView* descTextView;
    @property (nonatomic, retain) IBOutlet UIWebView* htmlWebView;
    @property (nonatomic, retain) IBOutlet UIButton* okButton;
    @property (nonatomic, retain) IBOutlet UIButton* cancelButton;

    -(IBAction)okButtonClicked:(id)sender;
    -(IBAction)cancelButtonClicked:(id)sender;

`CustomAnnouncementViewController.m`

    //Implementation
    @implementation CustomAnnouncementViewController
    @synthesize titleLabel;
    @synthesize descTextView;
    @synthesize htmlWebView;
    @synthesize okButton;
    @synthesize cancelButton;

    -(id)initWithAnnouncement:(AEReachAnnouncement*)anAnnouncement
    {
      self = [super initWithNibName:@"CustomAnnouncementViewController" bundle:nil];
      if (self != nil) {
        self.announcement = anAnnouncement;
      }
      return self;
    }

    - (void) dealloc
    {
      [titleLabel release];
      [descTextView release];
      [htmlWebView release];
      [okButton release];
      [cancelButton release];
      [super dealloc];
    }

    - (void)viewDidLoad {
      [super viewDidLoad];

      /* Init announcement title */
      titleLabel.text = self.announcement.title;

      /* Init announcement body */
      if(self.announcement.type == AEAnnouncementTypeHtml)
      {
        titleLabel.hidden = YES;
        htmlWebView.hidden = NO;
        [htmlWebView loadHTMLString:self.announcement.body baseURL:[NSURL URLWithString:@"http://localhost/"]];
      }
      else
      {
        titleLabel.hidden = NO;
        htmlWebView.hidden = YES;
        descTextView.text = self.announcement.body;
      }

      /* Set action button label */
      if([self.announcement.actionLabel length] > 0)
        [okButton setTitle:self.announcement.actionLabel forState:UIControlStateNormal];

      /* Set exit button label */
      if([self.announcement.exitLabel length] > 0)
        [cancelButton setTitle:self.announcement.exitLabel forState:UIControlStateNormal];
    }

    #pragma mark Actions

    -(IBAction)okButtonClicked:(id)sender
    {
        [self action];
    }

    -(IBAction)cancelButtonClicked:(id)sender
    {
        [self exit];
    }

    @end
