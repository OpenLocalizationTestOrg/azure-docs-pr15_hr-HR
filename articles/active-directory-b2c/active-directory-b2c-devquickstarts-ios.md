<properties
    pageTitle="Azure Active Directory B2C: Poziv API web-mjesto iz aplikacije iOS pomoću biblioteka trećih strana | Microsoft Azure"
    description="U ovom se članku vidjet ćete kako stvoriti aplikaciju "popis obaveza" za iOS koja se poziva Node.js web-mjesto API pomoću OAuth 2.0 nošenja tokeni pomoću drugih proizvođača biblioteke"
    services="active-directory-b2c"
    documentationCenter="ios"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

< oznake ms.service= "aktivno-direktorija-b2c" ms.workload="identity" ms.tgt_pltfrm="na" ms.devlang="objectivec" ms.topic= "glavni članak"

    ms.date="07/26/2016"
    ms.author="brandwe"/>

# <a name="azure-ad-b2c--call-a-web-api-from-an-ios-application-using-a-third-party-library"></a>Azure AD B2C: Poziv API web-mjesto iz aplikacije iOS korištenjem biblioteci drugih proizvođača

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Identitet platformu Microsoft koristi Otvori standarde kao što su OAuth2 i OpenID povezivanje. Time se razvojnim programerima odražava bilo koje biblioteke što žele integrirati s naših usluga. Da biste olakšali razvojnim inženjerima naš platformu pomoću drugih biblioteka smo ste napisali nekoliko vodiči poput ove da biste demonstate kako konfigurirati trećih strana biblioteke za povezivanje s platformu Microsoft identitet. Većina biblioteka koje implementirati [Specifikacija RFC6749 OAuth2](https://tools.ietf.org/html/rfc6749) bit će moći spojiti na platformi Microsoft Identity.


Ako ne poznajete OAuth2 ili se povežite OpenID velik dio tu konfiguraciju uzorka možda ne mnogo smisla vama. Preporučujemo da pogledate kratak [Pregled protokol smo ste navedenih u nastavku](active-directory-b2c-reference-protocols.md).

> [AZURE.NOTE]
    Neke značajke naš platformi koje imaju izraza te standardima, primjerice uvjetno Access i Intune Upravljanje pravilima, potrebno koristiti naše Otvori izvor Microsoft Azure identiteta biblioteke. 
   
Platforma B2C podržani su neke scenarije servisa Azure Active Directory i značajke.  Da biste utvrdili koristite platformu B2C, Saznajte više o [B2C ograničenja](active-directory-b2c-limitations.md).


## <a name="get-an-azure-ad-b2c-directory"></a>Početak u imeniku Azure AD B2C

Prije korištenja Azure AD B2C morate stvoriti direktorij ili smjernice. Direktorij je spremnik za sve korisnike, aplikacije, grupe i više. Ako ga nemate već, [stvorite direktorij B2C](active-directory-b2c-get-started.md) prije nego što nastavite.

## <a name="create-an-application"></a>Stvaranje aplikacije komponente

Ćete morati stvoriti aplikaciju u direktoriju B2C. Tako ćete dobiti Azure AD informacije potrebne za sigurnu komunikaciju s aplikacijom. Aplikacija i na webu API prikazani su jedan **ID aplikacije** u tom slučaju jer oni čine jedan logičke aplikacije. Da biste stvorili aplikaciju, slijedite [ove upute](active-directory-b2c-app-registration.md). Obavezno:

- Uključuju putem **mobilnog uređaja** u aplikaciji.
- Kopirajte **ID aplikacije** koji vam je dodijeljen aplikacije. Također ćete to kasnije.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Stvaranje police

U Azure AD B2C, svaki korisnički doživljaj definira [pravila](active-directory-b2c-reference-policies.md). Aplikacije sadrži jednu sučelje za identitet: kombinirane prijave i registracije. Morate stvoriti ovo pravilo svake vrste kao što je opisano u [članku referenca pravila](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy). Kada stvorite pravilo, ne zaboravite:

- Odaberite **zaslonsko ime** i registracije atribute pravilo.
- Odaberite aplikaciju zahtjevima **zaslonsko ime** i **ID objekta** u svakoj pravila. Možete odabrati druge zahtjevima.
- Nakon stvaranja, kopirajte **naziv** svakog pravila. Treba imati prefiks `b2c_1_`.  Morat ćete naziv pravila kasnije.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Kada stvorite police, spremni ste za stvaranje aplikacije.


## <a name="download-the-code"></a>Preuzimanje kod

Kod za ovog praktičnog vodiča je spremljen [na GitHub](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-b2c).  Da biste pratili, možete [preuzeti aplikaciju kao u .zip](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-b2c)/archive/master.zip) ili ga Kloniraj:

```
git clone git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-b2c.git
```

Ili samo preuzimanje šifru dovršene i započinje s radom odmah: 

```
git clone --branch complete git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-b2c.git
```

## <a name="download-the-third-party-library-nxoauth2-and-launch-a-workspace"></a>Preuzimanje biblioteke nxoauth2 trećih strana i pokretanje radnog prostora programa Groove

Za ovaj vodič koristit ćemo OAuth2Client iz GitHub, u biblioteku OAuth2 za Mac OS X & iOS (Cocoa & Cocoa dodira). Ova biblioteka temelji se na skicu 10 specifikacija OAuth2. Implementira matičnoj aplikaciji profila i podržava krajnja točka za autorizaciju krajnjeg korisnika. To su sve što smo, potreban vam je prema integrat s platformu Microsoft identitet.

### <a name="adding-the-library-to-your-project-using-cocoapods"></a>Dodavanje biblioteke na projektu pomoću CocoaPods

CocoaPods je ovisnost upravitelja za projekte Xcode. Automatski se upravlja gore navedene korake za instalaciju.

```
$ vi Podfile
```
U ovom podfile dodajte sljedeće:

```
 platform :ios, '8.0'
 
 target 'SampleforB2C' do
 
 pod 'NXOAuth2Client'
 
 end
```

Sada se učitati podfile pomoću cocoapods. To će stvoriti novi XCode radni prostor će učitati.

```
$ pod install
...
$ open SampleforB2C.xcworkspace

```

## <a name="the-structure-of-the-project"></a>Struktura projekta

Imamo sljedeću strukturu postavljanje za naše projekt na skeleton:

* **Prikaz matrice** pomoću okna zadatka
* **Dodavanje zadatka prikaza** za podatke o odabranog zadatka
* **Prikaz za prijavu** koje korisnicima omogućuje prijavu u aplikaciju.

Ne možemo će prijelaz u različitih datoteka u programu project da biste dodali provjeru autentičnosti. Ostali dijelovi kod kao što su vizualne kod nije germane identitetu i služe za vas.

## <a name="create-the-settingsplist-file-for-your-application"></a>Stvaranje na `settings.plist` datoteka aplikacije

Je lakše Konfiguriranje aplikacije ako imamo središnje mjesto da biste stavili naš konfiguracijskih vrijednosti. Također olakšava razumijevanje svaka postavka funkcija u aplikaciji. Ne možemo će pod utjecajem *Popis svojstava* kao način možete unijeti te vrijednosti za aplikaciju.

* Stvori/Otvori u `settings.plist` datoteka u odjeljku `Supporting Files` u radnom prostoru aplikacije

* Unesite sljedeće vrijednosti (ne možemo ćete ih pregledate po detalja uskoro)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>accountIdentifier</key>
    <string>B2C_Acccount</string>
    <key>clientID</key>
    <string><client ID></string>
    <key>clientSecret</key>
    <string></string>
    <key>authURL</key>
    <string>https://login.microsoftonline.com/<tenant name>/oauth2/v2.0/authorize?p=<policy name></string>
    <key>loginURL</key>
    <string>https://login.microsoftonline.com/<tenant name>/login</string>
    <key>bhh</key>
    <string>urn:ietf:wg:oauth:2.0:oob</string>
    <key>tokenURL</key>
    <string>https://login.microsoftonline.com/<tenant name>/oauth2/v2.0/token?p=<policy name></string>
    <key>keychain</key>
    <string>com.microsoft.azureactivedirectory.samples.graph.QuickStart</string>
    <key>contentType</key>
    <string>application/x-www-form-urlencoded</string>
    <key>taskAPI</key>
    <string>https://aadb2cplayground.azurewebsites.net</string>
</dict>
</plist>
```

Gornja otvorite u web-aplikaciji detalja.


Za `authURL`, `loginURL`, `bhh`, `tokenURL` Primijetit ćete da je vam biti potrebno ispuniti vaše ime klijenta. Ovo je ime klijenta B2C klijentu koji je dodijeljen. Ako, primjerice, `kidventusb2c.onmicrosoft.com`. Ako koristite naš Otvori izvor Microsoft Azure identiteta biblioteke smo bi padajući ove podatke uz pomoć naš metapodataka krajnjoj točki. Napravili smo napravljenoga od izdvajanje te vrijednosti za vas.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

Na `keychain` vrijednost je spremnik biblioteku NXOAuth2Client će se koristiti za stvaranje spremišta lozinki za pohranu na tokena. Ako želite da biste dobili SSO svim aplikacijama brojne možete navesti iste spremišta lozinki u svakoj od aplikacija i kao zahtjev za korištenje tog spremišta lozinki u vašem entitements XCode. To možete pronaći u dokumentaciji Apple.

Na `<policy name>` na kraju URL-a svake su mjesta gdje želite smjestiti pravila koju ste stvorili iznad. Aplikacija će poziv ta pravila ovisno o tijeka.

U `taskAPI` je krajnju točku OSTALE ćemo podići s vaš token B2C da biste dodali zadataka ili upita postojeće zadatke. To je postavljen za ovaj uzorak. Ne morate promijeniti za uzorak rad.

Do kraja ove vrijednosti su potrebne za korištenje biblioteke i jednostavno stvaranje mjesta za vas provesti vrijednosti u kontekstu.

Imamo na `settings.plist` datoteku stvorili, moramo kod za čitanje.

## <a name="set-up-a-appdata-class-to-read-our-settings"></a>Postavljanje AppData predmete pročitati naš postavke

Recimo da jednostavne datoteka koji se tek nakon naše `settngs.plist` datoteke smo stvorili iznad i nemojte te postavke avaialble u budućnosti sve klasu. Jer smo ne želite da biste stvorili novu kopiju podataka svaki put razredu pita za nju, ne možemo će koristiti jednočlana uzorak i vratiti samo u istoj instanci stvara svaki put postala je zahtjev za postavke

* Stvaranje programa `AppData.h` datoteke:

```objc
#import <Foundation/Foundation.h>

@interface AppData : NSObject

@property(strong) NSString *accountIdentifier;
@property(strong) NSString *taskApiString;
@property(strong) NSString *authURL;
@property(strong) NSString *clientID;
@property(strong) NSString *loginURL;
@property(strong) NSString *bhh;
@property(strong) NSString *keychain;
@property(strong) NSString *tokenURL;
@property(strong) NSString *clientSecret;
@property(strong) NSString *contentType;

+ (id)getInstance;

@end
```

* Stvaranje programa `AppData.m` datoteke:

```objc
#import "AppData.h"

@implementation AppData

+ (id)getInstance {
  static AppData *instance = nil;
  static dispatch_once_t onceToken;

  dispatch_once(&onceToken, ^{
    instance = [[self alloc] init];

    NSDictionary *dictionary = [NSDictionary
        dictionaryWithContentsOfFile:[[NSBundle mainBundle]
                                         pathForResource:@"settings"
                                                  ofType:@"plist"]];
    instance.accountIdentifier = [dictionary objectForKey:@"accountIdentifier"];
    instance.clientID = [dictionary objectForKey:@"clientID"];
    instance.clientSecret = [dictionary objectForKey:@"clientSecret"];
    instance.authURL = [dictionary objectForKey:@"authURL"];
    instance.loginURL = [dictionary objectForKey:@"loginURL"];
    instance.bhh = [dictionary objectForKey:@"bhh"];
    instance.tokenURL = [dictionary objectForKey:@"tokenURL"];
    instance.keychain = [dictionary objectForKey:@"keychain"];
    instance.contentType = [dictionary objectForKey:@"contentType"];
    instance.taskApiString = [dictionary objectForKey:@"taskAPI"];

  });

  return instance;
}
@end
```

Sad ćemo možete jednostavno doći pri oglednim podacima tako da nazovete podršku jednostavno `  AppData *data = [AppData getInstance];` u bilo kojem od naše klase kao što ćete potražite u nastavku.



## <a name="set-up-the-nxoauth2client-library-in-your-appdelegate"></a>Postavljanje biblioteke NXOAuth2Client u vašem AppDelegate

Biblioteka NXOAuthClient zahtijeva neke vrijednosti da biste postavili sve značajke. Kada to je dovršena možete koristiti token koji je aquired da biste nazvali REST API-JA. Budući da bismo znali u `AppDelegate` će se pozivati bilo kojem trenutku smo učitavanje aplikacije smo stavljate naš konfiguracijskih vrijednosti u toj datoteci smisla.
* Otvaranje `AppDelegate.m` datoteka

* Uvoz neke datoteke zaglavlja koristit ćemo kasnije.

```objc
#import "NXOAuth2.h" // the Identity library we are using
#import "AppData.h" // the class we just created we will use to load the settings of our application
```

* Dodavanje u `setupOAuth2AccountStore` metoda u na AppDelegate

Potrebna da biste stvorili programa AccountStore i zatim sažetak sadržaja je smo upravo čita u iz podataka u `settings.plist` datoteku.

Evo nekoliko stvari koje trebali imati na umu o B2C servis sada da bi kod više razumljiv:


1. Azure AD B2C koristi *pravila* koje ste dobili od parametre upita da bi vaš zahtjev za servis. Time se omogućuje Azure Active Directory će poslužiti kao nezavisne servisa samo za aplikacije. Da bi se pružaju te vrlo upit parametara Trebamo dati na `kNXOAuth2AccountStoreConfigurationAdditionalAuthenticationParameters:` način s oglednim parametrima prilagođenog pravilnika. 

2. Azure AD B2C koristi dosega na približno na isti način kao poslužitelji OAuth2. No Budući da koristite B2C je suvišni o provjere autentičnosti korisnika kao pristupa resursima neke opsega apsolutno potrebni su redoslijedom za tijek da bi ispravno funkcionirala. Ovo je na `openid` opseg. Naš identiteta Microsoft SDK-ovi automatski prikazivao u `openid` opseg za da koje nećete vidjeti u našem SDK konfiguracije. Jer smo koristite biblioteku trećih strana, međutim, moramo da biste odredili taj opseg.

```objc
- (void)setupOAuth2AccountStore {
  AppData *data = [AppData getInstance]; // The singleton we use to get the settings

  NSDictionary *customHeaders =
      [NSDictionary dictionaryWithObject:@"application/x-www-form-urlencoded"
                                  forKey:@"Content-Type"];

  // Azure B2C needs
  // kNXOAuth2AccountStoreConfigurationAdditionalAuthenticationParameters for
  // sending policy to the server,
  // therefore we use -setConfiguration:forAccountType:
  NSDictionary *B2cConfigDict = @{
    kNXOAuth2AccountStoreConfigurationClientID : data.clientID,
    kNXOAuth2AccountStoreConfigurationSecret : data.clientSecret,
    kNXOAuth2AccountStoreConfigurationScope :
        [NSSet setWithObjects:@"openid", data.clientID, nil],
    kNXOAuth2AccountStoreConfigurationAuthorizeURL :
        [NSURL URLWithString:data.authURL],
    kNXOAuth2AccountStoreConfigurationTokenURL :
        [NSURL URLWithString:data.tokenURL],
    kNXOAuth2AccountStoreConfigurationRedirectURL :
        [NSURL URLWithString:data.bhh],
    kNXOAuth2AccountStoreConfigurationCustomHeaderFields : customHeaders,
    //      kNXOAuth2AccountStoreConfigurationAdditionalAuthenticationParameters:customAuthenticationParameters
  };

  [[NXOAuth2AccountStore sharedStore] setConfiguration:B2cConfigDict
                                        forAccountType:data.accountIdentifier];
}
```
Nakon toga provjerite je li poziva u AppDelegate u odjeljku `didFinishLaunchingWithOptions:` način. 

```
[self setupOAuth2AccountStore];
```


## <a name="create-a-loginviewcontroller-class-that-we-will-use-to-handle-authentication-requests"></a>Stvaranje na `LoginViewController` predmete koji koristimo za rukovanje zahtjevima za provjeru autentičnosti

Koristimo s prikaza za račun za prijavu. Time se omogućuje nam Pitaj korisnika za dodatnih čimbenika kao što su SMS tekstne poruke (Ako je konfiguriran) ili pak predati poruka o pogrešci ponovno korisnika. Ovdje ćete postaviti smo gore u prikaza i kasnije pisanje koda za rukovanje pozive koji će se dogoditi u prikaza s Microsoftovim servisom za identitet.

* Stvaranje na `LoginViewController.h` klase

```objc
@interface LoginViewController : UIViewController <UIWebViewDelegate>
@property(weak, nonatomic) IBOutlet UIWebView *loginView; // Our webview that we will use to do authentication

- (void)handleOAuth2AccessResult:(NSURL *)accessResult; // Allows us to get a token after we've received an Access code.
- (void)setupOAuth2AccountStore; // We will need to add to our OAuth2AccountStore we setup in our AppDelegate
- (void)requestOAuth2Access; // This is where we invoke our webview.
```

Ne možemo će stvoriti svaki od ovih metoda navedenih u nastavku.

> [AZURE.NOTE] 
    Provjerite je li se povezati s `loginView` za stvarni prikaza koji se nalazi unutar vaše scenarija. U suprotnom neće imati prikaza koji možete izbaciti kad dođe vrijeme za provjeru autentičnosti.

* Stvaranje na `LoginViewController.m` klase

* Dodavanje neke varijable provesti stanja kao što smo provjeru autentičnosti

```objc
NSURL *myRequestedUrl; \\ The URL request to Azure Active Directory 
NSURL *myLoadedUrl; \\ The URL loaded for Azure Active Directory
bool loginFlow = FALSE; 
bool isRequestBusy; \\ A way to give status to the thread that the request is still happening
NSURL *authcode; \\ A placeholder for our auth code.
```

* Zanemarivanje načina prikaza za rukovanje provjere autentičnosti

Moramo možete zaključiti na prikaza ponašanje želimo kada korisnik mora prijava kako je opisano iznad. Možete jednostavno izrezivanje i zalijepite kod u nastavku.

```objc
- (void)resolveUsingUIWebView:(NSURL *)URL {
  // We get the auth token from a redirect so we need to handle that in the
  // webview.

  if (![NSThread isMainThread]) {
    [self performSelectorOnMainThread:@selector(resolveUsingUIWebView:)
                           withObject:URL
                        waitUntilDone:YES];
    return;
  }

  NSURLRequest *hostnameURLRequest =
      [NSURLRequest requestWithURL:URL
                       cachePolicy:NSURLRequestUseProtocolCachePolicy
                   timeoutInterval:10.0f];
  isRequestBusy = YES;
  [self.loginView loadRequest:hostnameURLRequest];

  NSLog(@"resolveUsingUIWebView ready (status: UNKNOWN, URL: %@)",
        self.loginView.request.URL);
}

- (BOOL)webView:(UIWebView *)webView
    shouldStartLoadWithRequest:(NSURLRequest *)request
                navigationType:(UIWebViewNavigationType)navigationType {
  AppData *data = [AppData getInstance];

  NSLog(@"webView:shouldStartLoadWithRequest: %@ (%li)", request.URL,
        (long)navigationType);

  // The webview is where all the communication happens. Slightly complicated.

  myLoadedUrl = [webView.request mainDocumentURL];
  NSLog(@"***Loaded url: %@", myLoadedUrl);

  // if the UIWebView is showing our authorization URL or consent URL, show the
  // UIWebView control
  if ([request.URL.absoluteString rangeOfString:data.authURL
                                        options:NSCaseInsensitiveSearch]
          .location != NSNotFound) {
    self.loginView.hidden = NO;
  } else if ([request.URL.absoluteString rangeOfString:data.loginURL
                                               options:NSCaseInsensitiveSearch]
                 .location != NSNotFound) {
    // otherwise hide the UIWebView, we've left the authorization flow
    self.loginView.hidden = NO;
  } else if ([request.URL.absoluteString rangeOfString:data.bhh
                                               options:NSCaseInsensitiveSearch]
                 .location != NSNotFound) {
    // otherwise hide the UIWebView, we've left the authorization flow
    self.loginView.hidden = YES;
    [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
  } else {
    self.loginView.hidden = NO;
    // read the Location from the UIWebView, this is how Microsoft APIs is
    // returning the
    // authentication code and relation information. This is controlled by the
    // redirect URL we chose to use from Microsoft APIs
    // continue the OAuth2 flow
    // [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
  }

  return YES;
}

```

* Pisanje koda za rukovanje rezultat zahtjeva za OAuth2

Ne možemo potreban kod koji će se rukovati redirectURL koji dolaze vratite na prikaz za web. Ako nije uspjela, ne možemo će se pokušajte ponovno. U međuvremenu biblioteku dat će pogrešku potražite u članku na konzoli ili rukovati asyncronously. 

```objc
- (void)handleOAuth2AccessResult:(NSURL *)accessResult {
  // parse the response for success or failure
  if (accessResult)
  // if success, complete the OAuth2 flow by handling the redirect URL and
  // obtaining a token
  {
    [[NXOAuth2AccountStore sharedStore] handleRedirectURL:accessResult];
  } else {
    // start over
    [self requestOAuth2Access];
  }
}
```

* Postavljanje factories obavijesti.

Ćemo stvoriti istu metodu smo niste u na `AppDelegate` noviji, ali ovaj put dodat ćemo neka `NSNotification`s da biste Recite nam što se događa u naših usluga. Ne možemo postaviti na observer koji će nam sve promjene s token. Nakon što smo dobili token smo vratili korisnika natrag na `masterView`.



```objc
- (void)setupOAuth2AccountStore {
  [[NSNotificationCenter defaultCenter]
      addObserverForName:NXOAuth2AccountStoreAccountsDidChangeNotification
                  object:[NXOAuth2AccountStore sharedStore]
                   queue:nil
              usingBlock:^(NSNotification *aNotification) {
                if (aNotification.userInfo) {
                  // account added, we have access
                  // we can now request protected data
                  NSLog(@"Success!! We have an access token.");
                  dispatch_async(dispatch_get_main_queue(), ^{

                    MasterViewController *masterViewController =
                        [self.storyboard
                            instantiateViewControllerWithIdentifier:@"master"];
                    [self.navigationController
                        pushViewController:masterViewController
                                  animated:YES];
                  });
                } else {
                  // account removed, we lost access
                }
              }];

  [[NSNotificationCenter defaultCenter]
      addObserverForName:NXOAuth2AccountStoreDidFailToRequestAccessNotification
                  object:[NXOAuth2AccountStore sharedStore]
                   queue:nil
              usingBlock:^(NSNotification *aNotification) {
                NSError *error = [aNotification.userInfo
                    objectForKey:NXOAuth2AccountStoreErrorKey];
                NSLog(@"Error!! %@", error.localizedDescription);
              }];
}

```
* Dodavanje koda za rukuje korisnika svaki put kada se pokrene zahtjeva za prijavu lokalnog

Stvaranje metode koje će se pozivati svaki put kada imamo zahtjev za provjeru autentičnosti. Ta će se način koji stvara na prikaz za web

```objc
- (void)requestOAuth2Access {
  AppData *data = [AppData getInstance];

  // in order to login to Mircosoft APIs using OAuth2 we must show an embedded
  // browser (UIWebView)
  [[NXOAuth2AccountStore sharedStore]
           requestAccessToAccountWithType:data.accountIdentifier
      withPreparedAuthorizationURLHandler:^(NSURL *preparedURL) {
        // navigate to the URL returned by NXOAuth2Client

        NSURLRequest *r = [NSURLRequest requestWithURL:preparedURL];
        [self.loginView loadRequest:r];
      }];
}
```

* Na kraju, recimo poziva sve načine smo ste napisali iznad svaki put na `LoginViewController` učita. Ne možemo to dodavanjem metode naše `viewDidLoad` način Apple daje nam

```objc
  [super viewDidLoad];
  // Do any additional setup after loading the view.

  // OAuth2 Code

  self.loginView.delegate = self;
  [self requestOAuth2Access];
  [self setupOAuth2AccountStore];
  NSURLCache *URLCache =
      [[NSURLCache alloc] initWithMemoryCapacity:4 * 1024 * 1024
                                    diskCapacity:20 * 1024 * 1024
                                        diskPath:nil];
  [NSURLCache setSharedURLCache:URLCache];
```

Sada ste gotovi sa stvaranjem glavno sredstvo smo ćete interakciju s oglednim aplikacija za prijavu u. Nakon što smo ste prijavljeni, ne možemo morat ćete koristiti naše tokeni smo dobili. Za taj ćemo stvorit ćete neke Pomoćnik za kod koji će se poziva REST API-ji za nam se korištenjem biblioteke.


## <a name="create-a-graphapicaller-class-to-handle-our-requests-to-a-rest-api"></a>Stvaranje na `GraphAPICaller` predmete rukovati naš zahtjeva za REST API-JA

Imamo konfiguracije učitati svaki put ne možemo učitavanje naša aplikacija. Sada ćemo potrebno nešto učiniti s njim kada imamo token. 

* Stvaranje na `GraphAPICaller.h` datoteka

```objc
@interface GraphAPICaller : NSObject <NSURLConnectionDataDelegate>

+ (void)addTask:(Task *)task
completionBlock:(void (^)(bool, NSError *error))completionBlock;

+ (void)getTaskList:(void (^)(NSMutableArray *, NSError *error))completionBlock;

@end
```

Vi vidite od kod koje ćemo hoće li stvarati na dva načina: jedan da biste zadatke s API, a drugi za dodavanje zadataka na API-JA.

Sad kad smo postavite naš sučelja, dodat ćemo stvarni implementaciju:

* Stvaranje programa`GraphAPICaller.m file`

```objc
@implementation GraphAPICaller

// 
// Gets the tasks from our REST endpoint we specified in settings
//

+ (void)getTaskList:(void (^)(NSMutableArray *, NSError *))completionBlock

{
  AppData *data = [AppData getInstance];

  NSString *taskURL =
      [NSString stringWithFormat:@"%@%@", data.taskApiString, @"/api/tasks"];

  NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
  NSMutableArray *Tasks = [[NSMutableArray alloc] init];

  NSArray *accounts = [store accountsWithAccountType:data.accountIdentifier];
  [NXOAuth2Request performMethod:@"GET"
      onResource:[NSURL URLWithString:taskURL]
      usingParameters:nil
      withAccount:accounts[0]
      sendProgressHandler:^(unsigned long long bytesSend,
                            unsigned long long bytesTotal) {
        // e.g., update a progress indicator
      }
      responseHandler:^(NSURLResponse *response, NSData *responseData,
                        NSError *error) {
        // Process the response
        if (!error) {
          NSDictionary *dataReturned =
              [NSJSONSerialization JSONObjectWithData:responseData
                                              options:0
                                                error:nil];
          NSLog(@"Graph Response was: %@", dataReturned);

          if ([dataReturned count] != 0) {

            for (NSMutableDictionary *theTask in dataReturned) {

              Task *t = [[Task alloc] init];
              t.name = [theTask valueForKey:@"Text"];

              [Tasks addObject:t];
            }
          }

          completionBlock(Tasks, nil);
        } else {
          completionBlock(nil, error);
        }

      }];
}

// 
// Adds a task from our REST endpoint we specified in settings
//

+ (void)addTask:(Task *)task
completionBlock:(void (^)(bool, NSError *error))completionBlock {

  AppData *data = [AppData getInstance];

  NSString *taskURL =
      [NSString stringWithFormat:@"%@%@", data.taskApiString, @"/api/tasks"];

  NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
  NSDictionary *params = [self convertParamsToDictionary:task.name];

  NSArray *accounts = [store accountsWithAccountType:data.accountIdentifier];
  [NXOAuth2Request performMethod:@"POST"
      onResource:[NSURL URLWithString:taskURL]
      usingParameters:params
      withAccount:accounts[0]
      sendProgressHandler:^(unsigned long long bytesSend,
                            unsigned long long bytesTotal) {
        // e.g., update a progress indicator
      }
      responseHandler:^(NSURLResponse *response, NSData *responseData,
                        NSError *error) {
        // Process the response
        if (responseData) {
          NSDictionary *dataReturned =
              [NSJSONSerialization JSONObjectWithData:responseData
                                              options:0
                                                error:nil];
          NSLog(@"Graph Response was: %@", dataReturned);

          completionBlock(TRUE, nil);
        } else {
          completionBlock(FALSE, error);
        }

      }];
}

+ (NSDictionary *)convertParamsToDictionary:(NSString *)task {
  NSMutableDictionary *dictionary = [[NSMutableDictionary alloc] init];

  [dictionary setValue:task forKey:@"Text"];

  return dictionary;
}

@end
```

## <a name="run-the-sample-app"></a>Pokrenite aplikaciju uzorka

Na kraju, stvaranje i pokretanje aplikacije u Xcode. Registracija za Office ili prijavite se u aplikaciju i stvorite zadatke za nekog korisnika prijavljeni u. Odjava i ponovno prijavite kao drugi korisnik i stvaranje zadataka za tog korisnika.

Imajte na umu da se zadaci su pohranjene po korisniku na API-JA, jer u API izdvaja identitet korisnika iz token za pristup koja ga Prima.


## <a name="next-steps"></a>Daljnji koraci

Sada se može pomaknuti na naprednije B2C teme. Morat ćete:

[Pozivanje Node.js web-mjesto API iz Node.js web-aplikacije]()

[Prilagodba UX za aplikaciju B2C]()
