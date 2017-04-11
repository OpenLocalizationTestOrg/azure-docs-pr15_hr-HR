<properties
    pageTitle="Azure AD iOS Uvod | Microsoft Azure"
    description="Sastavljanje iOS aplikacije koja integrira Azure AD za prijavu i poziva Azure AD zaštićen API-ji pomoću OAuth."
    services="active-directory"
    documentationCenter="ios"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>

# <a name="integrate-azure-ad-into-an-ios-app"></a>Integriranje Azure AD u aplikaciji sustava iOS

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)] 

Azure AD omogućuje provjeru autentičnosti biblioteke imenika Active Directory ili ADAL, za iOS klijenti koje moraju imati pristup zaštićeni resursi.  ADAL-isključivom svrhom u životu je da biste olakšali aplikacije da biste dobili pristup tokena.  Da bismo pokazali samo kako je lako, ovdje bismo ćete stvoriti popis obaveza C ciljne aplikacije koje:

-   Uzima pristupiti tokeni za pozivanje API Azure AD grafikonu [OAuth 2.0 protokol provjere autentičnosti](https://msdn.microsoft.com/library/azure/dn645545.aspx).
-   Pretražuje direktorij za korisnike s Navedeni pseudonim.

Da biste sastavili dovršeno aplikacije raditi, morat ćete:

2. Registracija aplikacije s Azure AD.
3. Instaliranje i konfiguriranje ADAL.
5. Da biste dobili tokena iz Azure AD pomoću ADAL.

Da biste počeli, [Preuzmite skeleton aplikacije](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/skeleton.zip) ili [preuzeti dovršene uzorka](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).  Trebat ćete klijent za Azure AD Dodavanje korisnika i Registracija aplikacije.  Ako još nemate klijent, [Saznajte kako da biste ga nabavili](active-directory-howto-tenant.md).

> [AZURE.TIP] Isprobajte pretpregled značajke naš novi [portala za razvojne inženjere](https://identity.microsoft.com/Docs/iOS) koji će vam omogućiti brz početak rada s Azure Active Directory u samo nekoliko minuta!  Portala za razvojne inženjere će vas voditi kroz postupak registracije aplikacije i integracija Azure AD u kodu.  Kada završite, imat ćete jednostavan aplikacije koje možete provjere autentičnosti korisnika u klijentu i s pozadinskom koje možete prihvatiti tokeni i izvršiti provjeru valjanosti. 

## <a name="1-determine-what-your-redirect-uri-will-be-for-ios"></a>*1. određuju što vaš URI preusmjeravanje bit će za iOS*

Da biste sigurno pokretanje aplikacije u određenim slučajevima SSO tražimo stvaranje **Preusmjeravanje URI** u određenom obliku. Da biste bili sigurni da se tokena vraćati ispravne aplikaciji koja ih tražiti koristi se URI za preusmjeravanje.

IOS oblik za preusmjeravanje URI je:

```
<app-scheme>://<bundle-id>
```

-   **aap sheme** – to je registrirana u projektu XCode. To je kako druge aplikacije da biste uputili poziv vam. Možete pronaći to u odjeljku Info.plist -> vrste URL -> identifikator URL-a. Stvorite jedan Ako još nemate jednu ili više konfiguriran.
-   **paket id** – to je identifikator paket koji se nalazi na izborniku "identiteta" Poništi postavki projekta u XCode.

Primjer za brzi početak rada kod bio: ***msquickstart://com.microsoft.azureactivedirectory.samples.graph.QuickStart***

## <a name="2-register-the-directorysearcher-application"></a>*2. registrirati DirectorySearcher aplikacije*
Da biste omogućili aplikacije da biste dobili tokena, ćete najprije morate registrirati u klijentu za Azure AD i dati dozvolu za pristup grafikonu Azure AD API-JA:

-   Prijavite se na Portal za upravljanje Azure
-   U navigacijskom oknu lijevoj strani kliknite **Servisa Active Directory**
-   Odaberite klijenta u kojoj se registrirati aplikaciju.
-   Kliknite karticu **aplikacije** pa kliknite **Dodaj** u donjem ladica.
-   Slijedite upite i stvorite novu **Nativni klijentska aplikacija**.
    -   **Naziv** aplikacije opisane su aplikacije krajnjim korisnicima
    -   Kombinacija sheme i niz Azure AD pomoću kojega će vratiti tokena odgovore je **Preusmjeravanje Uri** .  Unesite vrijednost određene aplikacije na temelju informacija koje su iznad.
-   Nakon što ste dovršili registraciju, AAD će dodijeliti aplikacije klijent Jedinstveni identifikator.  Potreban vam je tu vrijednost u sljedećim odjeljcima tako da ga kopirali na kartici **Konfiguriraj** .
- I na kartici **Konfiguriraj** pronađite odjeljak "Dozvole za ostale aplikacije".  Aplikacija "Azure Active Directory" dodajte dozvolu **pristupa vaše imenik tvrtke ili ustanove** u odjeljku **Dodijeliti dozvole**.  To će omogućiti aplikacije da biste upit grafikonu API-JA za korisnike.

## <a name="3-install--configure-adal"></a>*3. instalirati i konfigurirati ADAL*
Sad kad ste aplikacije u Azure AD, možete instalirati ADAL i svoj identitet vezanih kod.  ADAL da biste mogli komunicirati s Azure AD, morate pružiti neke informacije o registraciji vaše aplikacije.
-   Započnite dodavanjem ADAL DirectorySearcher projekt pomoću Cocapods.

```
$ vi Podfile
```
U ovom podfile dodajte sljedeće:

```
source 'https://github.com/CocoaPods/Specs.git'
link_with ['QuickStart']
xcodeproj 'QuickStart'

pod 'ADALiOS'
```

Sada se učitati podfile pomoću cocoapods. To će stvoriti novi XCode radni prostor će učitati.

```
$ pod install
...
$ open QuickStart.xcworkspace
```

-   U programu project brzi početak rada, otvorite datoteku plist `settings.plist`.  Zamjena vrijednosti elemenata u odjeljku u skladu s vizualnim vrijednosti unos u Azure Portal.  Kod pozivati te vrijednosti svaki put kada se koristi ADAL.
    -   U `tenant` je domena Azure AD klijent, npr. contoso.onmicrosoft.com
    -   U `clientId` je clientId aplikaciju koju ste kopirali iz portalu.
    -   U `redirectUri` je preusmjeravanje URL-a registrirana na portalu.

## <a name="4--use-adal-to-get-tokens-from-aad"></a>*4. ADAL da biste pristupili slijedite tokena iz AAD*
Osnovno načelo iza ADAL je da svaki put kada aplikacije potreban token za pristup, jednostavno poziva na completionBlock `+(void) getToken : `, a ADAL ne ostale.  

-   U na `QuickStart` projekt, otvara `GraphAPICaller.m` i pronađite u `// TODO: getToken for generic Web API flows. Returns a token with no additional parameters provided.` komentar blizu vrha.  Ovo je gdje proći ADAL koordinate kroz CompletionBlock za komunikaciju s Azure AD i recite im kako predmemoriju tokena.

```ObjC
+(void) getToken : (BOOL) clearCache
           parent:(UIViewController*) parent
completionHandler:(void (^) (NSString*, NSError*))completionBlock;
{
    AppData* data = [AppData getInstance];
    if(data.userItem){
        completionBlock(data.userItem.accessToken, nil);
        return;
    }

    ADAuthenticationError *error;
    authContext = [ADAuthenticationContext authenticationContextWithAuthority:data.authority error:&error];
    authContext.parentController = parent;
    NSURL *redirectUri = [[NSURL alloc]initWithString:data.redirectUriString];

    [ADAuthenticationSettings sharedInstance].enableFullScreen = YES;
    [authContext acquireTokenWithResource:data.resourceId
                                 clientId:data.clientId
                              redirectUri:redirectUri
                           promptBehavior:AD_PROMPT_AUTO
                                   userId:data.userItem.userInformation.userId
                     extraQueryParameters: @"nux=1" // if this strikes you as strange it was legacy to display the correct mobile UX. You most likely won't need it in your code.
                          completionBlock:^(ADAuthenticationResult *result) {

                              if (result.status != AD_SUCCEEDED)
                              {
                                  completionBlock(nil, result.error);
                              }
                              else
                              {
                                  data.userItem = result.tokenCacheStoreItem;
                                  completionBlock(result.tokenCacheStoreItem.accessToken, nil);
                              }
                          }];
}

```

- Sada ćemo morati koristiti ovaj token za traženje korisnika u grafikonu. Pronalaženje na `// TODO: implement SearchUsersList` komentaru način unese zahtjevom GET API Azure AD grafikonu upit za korisnika čije UPN počinje s određenom traženi pojam.  No da bi upit API grafikon, morate unijeti u access_token u na `Authorization` zaglavlja zahtjeva – to je Odakle dolaze ADAL.

```ObjC
+(void) searchUserList:(NSString*)searchString
                parent:(UIViewController*) parent
       completionBlock:(void (^) (NSMutableArray* Users, NSError* error)) completionBlock
{
    if (!loadedApplicationSettings)
    {
        [self readApplicationSettings];
    }

    AppData* data = [AppData getInstance];

    NSString *graphURL = [NSString stringWithFormat:@"%@%@/users?api-version=%@&$filter=startswith(userPrincipalName, '%@')", data.taskWebApiUrlString, data.tenant, data.apiversion, searchString];


    [self craftRequest:[self.class trimString:graphURL]
                parent:parent
     completionHandler:^(NSMutableURLRequest *request, NSError *error) {

         if (error != nil)
         {
             completionBlock(nil, error);
         }
         else
         {

             NSOperationQueue *queue = [[NSOperationQueue alloc]init];

             [NSURLConnection sendAsynchronousRequest:request queue:queue completionHandler:^(NSURLResponse *response, NSData *data, NSError *error) {

                 if (error == nil && data != nil){

                     NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:data options:0 error:nil];

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
                         s.name =[keyValuePairs valueForKey:@"givenName"];

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
     }];

}

```
- Kada aplikacije zahtjeve token tako da nazovete `getToken(...)`, ADAL će pokušati vratili token bez korisnika traži vjerodajnice.  Ako ADAL određuje da korisnik nije potrebno prijaviti da biste dobili token ga će prikazati dijaloški okvir za prijavu, prikupljanje korisnikove vjerodajnice i povratna token nakon uspješne provjere autentičnosti.  Ako je ADAL ne mogu vratiti token iz bilo kojeg razloga će vratiti na `AdalException`.
- Primijetit ćete da se `AuthenticationResult` objekt sadrži na `tokenCacheStoreItem` objekt koji se može koristiti za prikupljanje informacija morati aplikacije.  U brzi početak rada, `tokenCacheStoreItem` koristi se za određivanje ako authenitcation već pojavio.


## <a name="step-5-build-and-run-the-application"></a>Korak 5: Stvaranje i pokretanje aplikacija



Čestitamo! Sada imate aplikaciju za rad sa sustavom iOS koja ima omogućuje provjeru autentičnosti korisnika, sigurno poziva Web API-ji pomoću OAuth 2.0, a osnovne informacije o korisniku.  Ako to već niste učinili, sada je vrijeme za popunjavanje vaš klijent s nekim korisnicima.  Pokrenite aplikaciju brzi početak rada i prijavite pomoću nekog od tih korisnika.  Traženje drugih korisnika na temelju njihove UPN-a.  Zatvorite aplikaciju i ponovno ga pokrenuti.  Obratite pozornost na to kako sesija ostaje netaknuta.

ADAL olakšava sve ove značajke uobičajenih identiteta ugraditi u aplikaciji.  Ga brine sve poslovne dirty umjesto vas - upravljanje predmemorije, podrška OAuth protokola izlaganje korisnika s Prijava korisničko Sučelje, osvježavanje istekle tokeni i drugo.  Sve što trebate znati je jedan poziv API `getToken`.

Za referencu, dovršeni uzorka (bez konfiguracijskih vrijednosti) navedeni su [u nastavku](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).  

## <a name="additional-scenarios"></a>Dodatni scenariji
Možete odmah premjestiti dodatne scenarije.  Preporučujemo vam da biste isprobali:

- [Sigurne Node.JS web-mjesto API-JA s Azure AD](active-directory-devquickstarts-webapi-nodejs.md)
- Saznajte [kako omogućiti SSO unakrsno aplikacije na iOS pomoću ADAL](active-directory-sso-ios.md)  

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
