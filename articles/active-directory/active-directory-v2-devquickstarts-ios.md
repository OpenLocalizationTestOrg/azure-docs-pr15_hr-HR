<properties
    pageTitle="Azure AD 2.0 aplikaciji iOS | Microsoft Azure"
    description="Sastavljanje aplikaciju iOS koja se prijavi korisnika završavaju na oba osobni Microsoftov račun i računa tvrtke ili obrazovne ustanove pomoću drugih proizvođača biblioteke."
    services="active-directory"
    documentationCenter=""
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="06/28/2016"
    ms.author="brandwe"/>

# <a name="add-sign-in-to-an-ios-app-using-a-third-party-library-with-graph-api-using-the-v20-endpoint"></a>Dodavanje prijavu iOS aplikacije drugih proizvođača biblioteke pomoću grafikonu API-JA pomoću krajnju točku 2.0

Identitet platformu Microsoft koristi Otvori standarde kao što su OAuth2 i OpenID povezivanje. Razvojni inženjeri možete koristiti bilo koje biblioteke žele integrirati s naših usluga. Da biste lakše razvojnim inženjerima naš platformu pomoću drugih biblioteka, ne možemo ste napisali nekoliko vodiči poput ove da bismo pokazali kako konfigurirati biblioteke drugih proizvođača za povezivanje s identiteta platformu Microsoft. Većina biblioteka koje implementirati [Specifikacija RFC6749 OAuth2](https://tools.ietf.org/html/rfc6749) možete povezati s platformu Microsoft identitet.

Uz aplikaciju sustava stvara ovaj vodič, korisnici mogu prijavite se u tvrtki ili ustanovi i zatim traženje drugim korisnicima u tvrtki ili ustanovi pomoću API grafikonu.

Ako ste novi korisnik OAuth2 ili povezivanje OpenID, približno tu konfiguraciju uzorka možda neće imati smisla vama. Preporučujemo da pročitate [protokoli 2.0 – OAuth 2.0 autorizacije kod tijek](active-directory-v2-protocols-oauth-code.md) za pozadinu.


> [AZURE.NOTE]
    Neke značajke naš platformu kojima je potreban izraz standardima OAuth2 ili povezivanje OpenID, primjerice uvjetno pristup i upravljanje pravilima Intune, potrebno koristiti naše Otvori izvor Microsoft Azure identiteta biblioteke.

Krajnja točka 2.0 ne podržava sve scenariji Azure Active Directory i značajke.

> [AZURE.NOTE]
    Da biste utvrdili koristite krajnju točku 2.0, Saznajte više o [2.0 ograničenja](active-directory-v2-limitations.md).

## <a name="download-code-from-github"></a>Preuzmite kod iz GitHub
Kod za ovog praktičnog vodiča je spremljen [na GitHub](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-v2).  Da biste pratili, možete [preuzeti aplikacije skeleton kao u .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) ili Kloniraj na skeleton:

```
git clone --branch skeleton git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-v2.git
```

Možete preuzeti samo uzorak i započeti odmah:

```
git clone git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-v2.git
```

## <a name="register-an-app"></a>Registracija aplikacije
Stvaranje nove aplikacije na [portal za registraciju aplikacije](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)ili slijedite detaljne upute pri [kako registrirati aplikacije s krajnju točku 2.0](active-directory-v2-app-registration.md).  Obavezno:

- Kopirajte **Id aplikacije** koji vam je dodijeljen aplikacije jer će vam je potrebna uskoro.
- Dodajte platformu **mobilne** aplikacije.
- Kopirajte **URI preusmjeravanje** na portalu. Morate koristiti zadanu vrijednost od `urn:ietf:wg:oauth:2.0:oob`.


## <a name="download-the-third-party-nxoauth2-library-and-create-a-workspace"></a>Preuzimanje biblioteke NXOAuth2 drugih proizvođača i stvaranje radnog prostora

Za ovaj vodič će koristiti OAuth2Client iz GitHub, što je u biblioteku OAuth2 za Mac OS X i iOS (Cocoa i Cocoa dodira). Ova biblioteka temelji se na skicu 10 specifikacija OAuth2. Implementira matičnoj aplikaciji profila i podržava krajnja točka za provjeru autentičnosti korisnika. To su sve što morate integrirati s identiteta platformu Microsoft.

### <a name="add-the-library-to-your-project-by-using-cocoapods"></a>Dodavanje biblioteke u projekt pomoću CocoaPods

CocoaPods je ovisnost upravitelja za projekte Xcode. Automatski se upravlja prethodne korake za instalaciju.

```
$ vi Podfile
```
1. U ovom podfile dodajte sljedeće:

    ```
     platform :ios, '8.0'

     target 'QuickStart' do

     pod 'NXOAuth2Client'

     end
    ```

2. Da biste učitali u podfile pomoću CocoaPods. To će stvoriti novi radni prostor Xcode koji će učitati.

    ```
    $ pod install
    ...
    $ open QuickStart.xcworkspace
    ```

## <a name="explore-the-structure-of-the-project"></a>Istražite strukturu projekta

Sljedeću strukturu postavljena za naše projekt na skeleton:

- Prikaz matrice pomoću UPN pretraživanja
- Detaljni prikaz podataka o odabranom korisniku
- Prikaz prijavu koju korisnik mogu se prijaviti aplikaciju upiti za grafikon sustava

Ne možemo premjestit će različitih datoteka u u skeleton da biste dodali provjeru autentičnosti. Druge dijelove koda, kao što su vizualni kod ne odnose na identiteta, ali služe za vas.

## <a name="set-up-the-settingsplst-file-in-the-library"></a>Postavljanje settings.plst datoteke u biblioteci

-   U programu project brzi početak rada otvorite na `settings.plist` datoteku. Zamjena vrijednosti elemenata u odjeljku u skladu s vrijednostima koje ste koristili na portalu za Azure. Kod pozivati te vrijednosti svaki put kada se koristi za provjeru autentičnosti biblioteke imenika Active Directory.
    -   U `clientId` je ID klijenta aplikacije koju ste kopirali na portalu.
    -   U `redirectUri` je preusmjeravanja URL u kojemu se na portal.

## <a name="set-up-the-nxoauth2client-library-in-your-loginviewcontroller"></a>Postavljanje biblioteke NXOAuth2Client u vašem LoginViewController

Biblioteka NXOAuth2Client zahtijeva neke vrijednosti da biste postavili sve značajke. Kada dovršite taj zadatak, za pozivanje API grafikon možete koristiti nabave token. Budući da `LoginView` bit će pod nazivom bilo kojem trenutku potrebna za provjeru autentičnosti smisla da biste postavili konfiguracijskih vrijednosti u tu datoteku.

- Dodajmo neke vrijednosti u `LoginViewController.m` datoteku da biste postavili kontekst za provjeru autentičnosti i ovlaštenja. Pojedinosti o vrijednostima slijedite kod.

    ```objc
    NSString *scopes = @"openid offline_access User.Read";
    NSString *authURL = @"https://login.microsoftonline.com/common/oauth2/v2.0/authorize";
    NSString *loginURL = @"https://login.microsoftonline.com/common/login";
    NSString *bhh = @"urn:ietf:wg:oauth:2.0:oob?code=";
    NSString *tokenURL = @"https://login.microsoftonline.com/common/oauth2/v2.0/token";
    NSString *keychain = @"com.microsoft.azureactivedirectory.samples.graph.QuickStart";
    static NSString * const kIDMOAuth2SuccessPagePrefix = @"session_state=";
    NSURL *myRequestedUrl;
    NSURL *myLoadedUrl;
    bool loginFlow = FALSE;
    bool isRequestBusy;
    NSURL *authcode;
    ```

Pogledajmo detalje o kod.

Prvi niz namijenjen `scopes`.  Na `User.Read` vrijednost omogućuje čitanje osnovni profila traje Prijava korisnika.

Dodatne informacije o sve dostupne opsege u [opsega dozvola Microsoft Graph](https://graph.microsoft.io/docs/authorization/permission_scopes).

Za `authURL`, `loginURL`, `bhh`, i `tokenURL`, trebali biste koristiti vrijednosti koje ste prethodno naveli. Ako koristite Otvori izvor Microsoft Azure identiteta biblioteke, ćemo uvesti te podatke prema dolje za pomoću naš metapodataka krajnje točke. Napravili smo napravljenoga od izdvajanje te vrijednosti za vas.

Na `keychain` vrijednost je spremnik biblioteku NXOAuth2Client će se koristiti za stvaranje spremišta lozinki za pohranu na tokena. Ako želite jasne brojne aplikacije jedinstvenu prijavu (SSO), možete odrediti isti spremišta lozinki u svakoj od aplikacija i zahtijeva korištenje tog spremišta lozinki prava na Xcode. To je objašnjeno u dokumentaciji Apple.

Do kraja ove vrijednosti su potrebne za korištenje biblioteke i stvaranje mjesta za vas provesti vrijednosti u kontekstu.

### <a name="create-a-url-cache"></a>Stvaranje URL predmemoriju

Unutar `(void)viewDidLoad()`, koji se uvijek naziva nakon učitavanja prikaza, sljedeći kod primes predmemoriju za naše.

Dodavanje koda za sljedeće:

```objc
- (void)viewDidLoad {
    [super viewDidLoad];
    self.loginView.delegate = self;
    [self setupOAuth2AccountStore];
    [self requestOAuth2Access];
    NSURLCache *URLCache = [[NSURLCache alloc] initWithMemoryCapacity:4 * 1024 * 1024
                                                         diskCapacity:20 * 1024 * 1024
                                                             diskPath:nil];
    [NSURLCache setSharedURLCache:URLCache];

}
```

### <a name="create-a-webview-for-sign-in"></a>Stvaranje prikaza za prijavu

Na prikaza možete Pitaj korisnika za dodatnih čimbenika kao što su SMS tekstne poruke (Ako je konfiguriran) ili povratak poruke o pogreškama korisnika. Ovdje ćete postaviti gore na prikaz za web, a zatim kasnije pisati kod za rukovanje pozive koji će se dogoditi u prikaza servisa za identitet.

```objc
-(void)requestOAuth2Access {
    //to sign in to Microsoft APIs using OAuth2, we must show an embedded browser (UIWebView)
    [[NXOAuth2AccountStore sharedStore] requestAccessToAccountWithType:@"myGraphService"
                                   withPreparedAuthorizationURLHandler:^(NSURL *preparedURL) {
                                       //navigate to the URL returned by NXOAuth2Client

                                       NSURLRequest *r = [NSURLRequest requestWithURL:preparedURL];
                                       [self.loginView loadRequest:r];
                                   }];
}
```

### <a name="override-the-webview-methods-to-handle-authentication"></a>Zanemarivanje načina prikaza za rukovanje provjere autentičnosti

Da biste prepoznali na prikaza na što se događa kada korisnik mora se prijaviti kao što je navedeno prethodno, možete zalijepiti sljedeći kod.

```objc
- (void)resolveUsingUIWebView:(NSURL *)URL {

    // We get the auth token from a redirect so we need to handle that in the webview.

    if (![NSThread isMainThread]) {
        [self performSelectorOnMainThread:@selector(resolveUsingUIWebView:) withObject:URL waitUntilDone:YES];
        return;
    }

    NSURLRequest *hostnameURLRequest = [NSURLRequest requestWithURL:URL cachePolicy:NSURLRequestUseProtocolCachePolicy timeoutInterval:10.0f];
    isRequestBusy = YES;
    [self.loginView loadRequest:hostnameURLRequest];

    NSLog(@"resolveUsingUIWebView ready (status: UNKNOWN, URL: %@)", self.loginView.request.URL);
}

- (BOOL)webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request navigationType:(UIWebViewNavigationType)navigationType {

    NSLog(@"webView:shouldStartLoadWithRequest: %@ (%li)", request.URL, (long)navigationType);

    // The webview is where all the communication happens. Slightly complicated.

    myLoadedUrl = [webView.request mainDocumentURL];
    NSLog(@"***Loaded url: %@", myLoadedUrl);

    //if the UIWebView is showing our authorization URL or consent URL, show the UIWebView control
    if ([request.URL.absoluteString rangeOfString:authURL options:NSCaseInsensitiveSearch].location != NSNotFound) {
        self.loginView.hidden = NO;
    } else if ([request.URL.absoluteString rangeOfString:loginURL options:NSCaseInsensitiveSearch].location != NSNotFound) {
        //otherwise hide the UIWebView, we've left the authorization flow
        self.loginView.hidden = NO;
    } else if ([request.URL.absoluteString rangeOfString:bhh options:NSCaseInsensitiveSearch].location != NSNotFound) {
        //otherwise hide the UIWebView, we've left the authorization flow
        self.loginView.hidden = YES;
        [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
    }
    else {
        self.loginView.hidden = NO;
        //read the Location from the UIWebView, this is how Microsoft APIs is returning the
        //authentication code and relation information. This is controlled by the redirect URL we chose to use from Microsoft APIs
        //continue the OAuth2 flow
       // [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
    }

    return YES;

}
```

### <a name="write-code-to-handle-the-result-of-the-oauth2-request"></a>Pisanje koda za rukovanje rezultat zahtjeva za OAuth2

Sljedeći kod će obrađivati redirectURL koji vraća-u prikaza. Ako provjera autentičnosti nije bio uspješan, kod će pokušajte ponovno. U međuvremenu, u biblioteku dat će pogreške koje se mogu vidjeti na konzoli ili rukovati asinkrono.

```objc
- (void)handleOAuth2AccessResult:(NSString *)accessResult {

    AppData* data = [AppData getInstance];

    //parse the response for success or failure
     if (accessResult)
    //if success, complete the OAuth2 flow by handling the redirect URL and obtaining a token
     {
         [[NXOAuth2AccountStore sharedStore] handleRedirectURL:accessResult];
    } else {
        //start over
        [self requestOAuth2Access];
    }
}
```

### <a name="set-up-the-oauth-context-called-account-store"></a>Postavljanje konteksta OAuth (naziva se spremište računa)

Ovdje možete nazvati `-[NXOAuth2AccountStore setClientID:secret:authorizationURL:tokenURL:redirectURL:forAccountType:]` spremište zajedničke računa za svaki servis koji želite aplikacije da biste mogli pristupiti. Vrsta računa je niz koji se koristi kao identifikator za određene usluge. Budući da pristupaju Graph API, kod odnosi kao `"myGraphService"`. Zatim postavite na observer koji će vam reći ništa promjene s token. Kada se pojavi token, se vratite u korisnika natrag na na `masterView`.



```objc
- (void)setupOAuth2AccountStore {


        AppData* data = [AppData getInstance];

    [[NXOAuth2AccountStore sharedStore] setClientID:data.clientId
                                             secret:data.secret
                                              scope:[NSSet setWithObject:scopes]
                                   authorizationURL:[NSURL URLWithString:authURL]
                                           tokenURL:[NSURL URLWithString:tokenURL]
                                        redirectURL:[NSURL URLWithString:data.redirectUriString]
                                      keyChainGroup: keychain
                                     forAccountType:@"myGraphService"];

    [[NSNotificationCenter defaultCenter] addObserverForName:NXOAuth2AccountStoreAccountsDidChangeNotification
                                                      object:[NXOAuth2AccountStore sharedStore]
                                                       queue:nil
                                                  usingBlock:^(NSNotification *aNotification) {
                                                      if (aNotification.userInfo) {
                                                          //account added, we have access
                                                          //we can now request protected data
                                                          NSLog(@"Success!! We have an access token.");
                                                          dispatch_async(dispatch_get_main_queue(),^ {

                                                              MasterViewController* masterViewController = [self.storyboard instantiateViewControllerWithIdentifier:@"masterView"];
                                                              [self.navigationController pushViewController:masterViewController animated:YES];
                                                          });
                                                      } else {
                                                          //account removed, we lost access
                                                      }
                                                  }];

    [[NSNotificationCenter defaultCenter] addObserverForName:NXOAuth2AccountStoreDidFailToRequestAccessNotification
                                                      object:[NXOAuth2AccountStore sharedStore]
                                                       queue:nil
                                                  usingBlock:^(NSNotification *aNotification) {
                                                      NSError *error = [aNotification.userInfo objectForKey:NXOAuth2AccountStoreErrorKey];
                                                      NSLog(@"Error!! %@", error.localizedDescription);
                                                  }];
}
```

## <a name="set-up-the-master-view-to-search-and-display-the-users-from-the-graph-api"></a>Postavljanje prikaz matrice za pretraživanje i prikaz korisnika iz API grafikonu

Aplikaciju za matricu, prikaz i kontroleru (MVC) koja se prikazuje vraćenih podataka u rešetki izlazi iz ovog vodiča i mnogo online vodiči za objašnjavaju kako stvoriti jednu. Ovaj kod se skeleton datoteku. Međutim, morate baviti nekoliko stvari u ovoj aplikaciji MVC:

* INTERCEPT kada korisnik unese nešto u polje za pretraživanje
* Pružanje objekt podataka natrag na MasterView da bi mogao prikazati rezultate u rešetki

Ćemo učiniti one ispod.

### <a name="add-a-check-to-see-if-youre-logged-in"></a>Dodavanje provjere da biste vidjeli ako ste prijavljeni

Aplikacija ne malo ako korisnik nije prijavljen, tako da bude pametno Provjerite postoji li već token u predmemoriji. Ako nije, preusmjeravanje LoginView za korisnika za prijavu. Ako opoziv, najbolji način da biste učinili akcije prilikom učitavanja prikaza je koristiti u `viewDidLoad()` način koji omogućuje Apple us.

```objc
- (void)viewDidLoad {
    [super viewDidLoad];


    NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
    NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];

        if (accounts.count == 0) {

        dispatch_async(dispatch_get_main_queue(),^ {

            LoginViewController* userSelectController = [self.storyboard instantiateViewControllerWithIdentifier:@"LoginUserView"];
            [self.navigationController pushViewController:userSelectController animated:YES];
        });
        }
```

### <a name="update-the-table-view-when-data-is-received"></a>Ažuriranje prikaz tablice primitku podataka

Kada API grafikonu vraća podatke, morate prikazati podatke. Zbog jednostavnosti, Evo kod da biste ažurirali tablicu. Desni vrijednosti možete zalijepiti samo u kodu predloženog MVC.

```objc
#pragma mark - Table View

- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView {
    return 1;
}

- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section {

        return [upnArray count];
}

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath {

    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:@"TaskPrototypeCell" forIndexPath:indexPath];

    if ( cell == nil ) {
        cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:@"TaskPrototypeCell"];
    }

    User *user = nil;
     user = [upnArray objectAtIndex:indexPath.row];


    // Configure the cell
    cell.textLabel.text = user.name;
    [cell setAccessoryType:UITableViewCellAccessoryDisclosureIndicator];

    return cell;
}

```

### <a name="provide-a-way-to-call-the-graph-api-when-someone-types-in-the-search-field"></a>Omogućuje pozivanje API grafikonu kada se piše u polje za pretraživanje

Kada korisnik unese pretraživanja u okviru, morate shove koji API grafikonu. Na `GraphAPICaller` predmete koje sastavljate će biti sljedeći kod, funkcija lookup razdvaja iz prezentacije. Zasad, recimo napišite kod koji sažetaka sadržaja znakove pretraživanja na grafikonu API-JA. Ne možemo to unosom metode pod nazivom `lookupInGraph`, koji traje niz koji smo želite potražiti.

```objc

-(void)lookupInGraph:(NSString *)searchText {
if (searchText.length > 0) {

    };



        [GraphAPICaller searchUserList:searchText completionBlock:^(NSMutableArray* returnedUpns, NSError* error) {
            if (returnedUpns) {


                upnArray = returnedUpns;


            }
            else
            {
                UIAlertView *alertView = [[UIAlertView alloc]initWithTitle:nil message:[[NSString alloc]initWithFormat:@"Error : %@", error.localizedDescription] delegate:nil cancelButtonTitle:@"Retry" otherButtonTitles:@"Cancel", nil];

                [alertView setDelegate:self];

                dispatch_async(dispatch_get_main_queue(),^ {
                    [alertView show];
                });
            }


        }];


}
```

## <a name="write-a-helper-class-to-access-the-graph-api"></a>Pisanje pomoćne klase pristup grafikonu API-JA

Ovo je core naš aplikacije. Dok ostale je Umetanje koda u zadani uzorak MVC od Applea, ovdje pisanja koda za upiti za grafikon sustava dok korisnik upisuje, a zatim se vratite tih podataka. Ovdje je kod, a Detaljno objašnjenje slijedi.

### <a name="create-a-new-objective-c-header-file"></a>Stvaranje nove datoteke zaglavlja cilj C

Dajte naziv datoteci `GraphAPICaller.h`, dodajte sljedeći kod.

```objc
@interface GraphAPICaller : NSObject<NSURLConnectionDataDelegate>

+(void) searchUserList:(NSString*)searchString
       completionBlock:(void (^) (NSMutableArray*, NSError* error))completionBlock;

@end
```

Ovdje vidjeti navedena metoda vodi niza te vraća na completionBlock. U ovom completionBlock kao koje možda imaju pogoditi, ažurirat će se tablice unosom objekt s popunjena podatke u stvarnom vremenu kao korisnik pretraživanja.


### <a name="create-a-new-objective-c-file"></a>Stvaranje nove datoteke cilj C

Dajte naziv datoteci `GraphAPICaller.m`, i dodajte na sljedeći način.

```objc
+(void) searchUserList:(NSString*)searchString
       completionBlock:(void (^) (NSMutableArray* Users, NSError* error)) completionBlock
{
    if (!loadedApplicationSettings)
    {
        [self readApplicationSettings];
    }

    AppData* data = [AppData getInstance];

    NSString *graphURL = [NSString stringWithFormat:@"%@%@/users", data.graphApiUrlString, data.apiversion];

    NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
    NSDictionary* params = [self convertParamsToDictionary:searchString];

    NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];
    [NXOAuth2Request performMethod:@"GET"
                        onResource:[NSURL URLWithString:graphURL]
                   usingParameters:params
                       withAccount:accounts[0]
               sendProgressHandler:^(unsigned long long bytesSend, unsigned long long bytesTotal) {
                   // e.g., update a progress indicator
               }
                   responseHandler:^(NSURLResponse *response, NSData *responseData, NSError *error) {
                       // Process the response
                       if (responseData) {
                           NSError *error;
                           NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:responseData options:0 error:nil];
                           NSLog(@"Graph Response was: %@", dataReturned);

                           // We can grab the top most JSON node to get our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];

                           // Don't be thrown off by the key name being "value". It really is the name of the
                           // first node. :-)

                           //each object is a key value pair
                           NSDictionary *keyValuePairs;
                           NSMutableArray* Users = [[NSMutableArray alloc]init];

                           for(int i =0; i < graphDataArray.count; i++)
                           {
                               keyValuePairs = [graphDataArray objectAtIndex:i];

                               User *s = [[User alloc]init];
                               s.upn = [keyValuePairs valueForKey:@"userPrincipalName"];
                               s.name =[keyValuePairs valueForKey:@"displayName"];
                               s.mail =[keyValuePairs valueForKey:@"mail"];
                               s.businessPhones =[keyValuePairs valueForKey:@"businessPhones"];
                               s.mobilePhones =[keyValuePairs valueForKey:@"mobilePhone"];


                               [Users addObject:s];
                           }

                           completionBlock(Users, nil);
                       }
                       else
                       {
                           completionBlock(nil, error);
                       }

                   }];
}

```

Pogledajmo ovu metodu detaljno.

Osnovni kod se na `NXOAuth2Request`, način koji vodi parametre koje ste već definirali u datoteci settings.plist.

U prvi je korak za sastavljanje desnom grafikonu API poziva. Budući da su pozivanje `/users`, koji navedete dodavanjem resursa grafikonu API uz verziju. Je li bolje staviti u datoteci postavke vanjskog jer je to možete promijeniti kako nastaju na API-JA.


```objc
NSString *graphURL = [NSString stringWithFormat:@"%@%@/users", data.graphApiUrlString, data.apiversion];
```

Ćete morati navedite parametre koje će daju i u grafikonu API poziv. Je *Važno* da ne spremate parametara u krajnju točku resursa jer koji je scrubbed za sve znakove potvrđivanja URI-JA prilikom izvođenja. Sav kod upita mora se navesti u parametre.

```objc

NSDictionary* params = [self convertParamsToDictionary:searchString];
```

Mogli biste primijetiti to poziva na `convertParamsToDictionary` metoda koje još niste napisali. Pogledajmo učinite odmah na kraju datoteku:

```objc
+(NSDictionary*) convertParamsToDictionary:(NSString*)searchString
{
    NSMutableDictionary* dictionary = [[NSMutableDictionary alloc]init];

        NSString *query = [NSString stringWithFormat:@"startswith(givenName, '%@')", searchString];

           [dictionary setValue:query forKey:@"$filter"];



    return dictionary;
}

```
Zatim ćemo pomoću na `NXOAuth2Request` način da biste se vratili podatke iz API-JA u JSON OSNOVNI oblik.

```objc
NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];
    [NXOAuth2Request performMethod:@"GET"
                        onResource:[NSURL URLWithString:graphURL]
                   usingParameters:params
                       withAccount:accounts[0]
               sendProgressHandler:^(unsigned long long bytesSend, unsigned long long bytesTotal) {
                   // e.g., update a progress indicator
               }
                   responseHandler:^(NSURLResponse *response, NSData *responseData, NSError *error) {
                       // Process the response
                       if (responseData) {
                           NSError *error;
                           NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:responseData options:0 error:nil];
                           NSLog(@"Graph Response was: %@", dataReturned);

                           // We can grab the top most JSON node to get our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];
```

Na kraju, Pogledajmo kako vratiti podatke u na MasterViewController. Podaci vraća kao serijalizirani i mora biti deserijalizirati i učitani u objekt koji se može raditi u MainViewController. U tu svrhu sadrži na skeleton na `User.m/h` datoteku koja se stvara korisničkom objektu. Popunjavanje objekt korisnika s podacima na grafikonu.

```objc
                           // We can grab the top most JSON node to get our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];

                           // Don't be thrown off by the key name being "value". It really is the name of the
                           // first node. :-)

                           //each object is a key value pair
                           NSDictionary *keyValuePairs;
                           NSMutableArray* Users = [[NSMutableArray alloc]init];

                           for(int i =0; i < graphDataArray.count; i++)
                           {
                               keyValuePairs = [graphDataArray objectAtIndex:i];

                               User *s = [[User alloc]init];
                               s.upn = [keyValuePairs valueForKey:@"userPrincipalName"];
                               s.name =[keyValuePairs valueForKey:@"displayName"];
                               s.mail =[keyValuePairs valueForKey:@"mail"];
                               s.businessPhones =[keyValuePairs valueForKey:@"businessPhones"];
                               s.mobilePhones =[keyValuePairs valueForKey:@"mobilePhone"];


                               [Users addObject:s];
```


## <a name="run-the-sample"></a>Pokretanje uzorka

Ako ste koristili u skeleton ili iza zajedno u prikazu aplikacije sada trebale bi funkcionirati. Pokrenite simulator i kliknite **Prijava** da biste koristili aplikaciju.

## <a name="get-security-updates-for-our-product"></a>Sigurnosna ažuriranja za naše proizvoda

Pozivamo vas da biste dobili obavijesti kada doći do sigurnošću posjeta [TechCenter za sigurnost](https://technet.microsoft.com/security/dd252948) i pretplata na upozorenja savjeta za sigurnost.
