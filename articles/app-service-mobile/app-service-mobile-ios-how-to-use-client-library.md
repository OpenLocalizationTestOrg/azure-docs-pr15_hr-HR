<properties
    pageTitle="Upute za korištenje sa sustavom iOS SDK za Azure mobilne aplikacije"
    description="Upute za korištenje sa sustavom iOS SDK za Azure mobilne aplikacije"
    services="app-service\mobile"
    documentationCenter="ios"
    authors="ysxu"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="how-to-use-ios-client-library-for-azure-mobile-apps"></a>Upute za korištenje iOS klijentska biblioteka za Azure mobilne aplikacije

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Ovaj vodič ručica za izvođenje uobičajeni scenariji pomoću najnovije [aplikacije Mobile Azure iOS SDK][1]. Ako ste novi korisnik aplikacije Mobile Azure, prvi cjelovit [Azure mobilne aplikacije brzi početak rada] da biste stvorili pozadinskog, stvorite tablicu i preuzimanje ugrađene iOS Xcode projekta. U ovom vodiču smo fokus na klijentskoj strani iOS SDK. Da biste saznali više o poslužiteljsko SDK pozadinski, potražite u članku HOWTOs SDK poslužitelja.

## <a name="reference-documentation"></a>Dokumentacija reference

IOS klijent SDK potražite u dokumentaciji referenca nalazi se ovdje: [Azure mobilne aplikacije iOS Reference klijenta][2].

## <a name="supported-platforms"></a>Podržane platforme

IOS SDK podržava cilj C projektima, Swift 2.2 projekata i Swift 2.3 projekata za iOS verzije 8.0 ili noviji.

Provjera autentičnosti "poslužitelj tijek" koristi s prikaza za izloženi korisničkog Sučelja.  Ako je uređaj nije moći prikazati korisničko Sučelje prikaza, a zatim potreban je drugi način provjere autentičnosti koja je izlaze proizvoda.  
Taj SDK nije stoga prikladan za nadzor vrsta ili na sličan način ograničena uređaje.

##<a name="Setup"></a>Postavljanje i preduvjeti

Ovaj vodič pretpostavlja da ste stvorili s pozadinskom s tablicom. Ovaj vodič pretpostavlja da tablica ima istu shemu kao tablice u tim vodičima. Ovaj vodič i pretpostavlja da u kodu upućuje `MicrosoftAzureMobile.framework` i uvoz `MicrosoftAzureMobile/MicrosoftAzureMobile.h`.

##<a name="create-client"></a>Kako: Stvaranje klijenta

Da biste pristupili Azure mobilne aplikacije pozadinskog sustava u projektu, stvorite je `MSClient`. Zamjena `AppUrl` aplikacije URL-om. Možete ostaviti `gatewayURLString` i `applicationKey` prazan. Ako postavljate pristupnik za provjeru autentičnosti, popunjavanje `gatewayURLString` pristupnika URL-om.

**Cilj C**:

```
MSClient *client = [MSClient clientWithApplicationURLString:@"AppUrl"];
```

**Swift**:

```
let client = MSClient(applicationURLString: "AppUrl")
```


##<a name="table-reference"></a>Kako: Stvaranje referenci tablice

Access ili ažuriranje podataka, stvaranje reference na tablici pozadinskog. Zamjena `TodoItem` s nazivom tablice

**Cilj C**:

```
MSTable *table = [client tableWithName:@"TodoItem"];
```

**Swift**:

```
let table = client.tableWithName("TodoItem")
```


##<a name="querying"></a>Kako: upit podatke

Da biste stvorili upita baze podataka, upit s `MSTable` objekt. Sljedeći upit dohvaća sve stavke `TodoItem` i prijavljuje teksta svake stavke.

**Cilj C**:

```
[table readWithCompletion:^(MSQueryResult *result, NSError *error) {
        if(error) { // error is nil if no error occured
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) { // items is NSArray of records that match query
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**Swift**:

```
table.readWithCompletion { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```

##<a name="filtering"></a>Kako: filtar vraćenih podataka

Da biste filtrirali rezultate, nema više dostupnih mogućnosti.

Da biste filtrirali pomoću predikata, poslužite se `NSPredicate` i `readWithPredicate`. Sljedeći filtri vraćaju podataka da biste pronašli samo nepotpuna oznake stavki.

**Cilj C**:

```
// Create a predicate that finds items where complete is false
NSPredicate * predicate = [NSPredicate predicateWithFormat:@"complete == NO"];
// Query the TodoItem table
[table readWithPredicate:predicate completion:^(MSQueryResult *result, NSError *error) {
        if(error) {
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) {
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**Swift**:

```
// Create a predicate that finds items where complete is false
let predicate =  NSPredicate(format: "complete == NO")
// Query the TodoItem table
table.readWithPredicate(predicate) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```

##<a name="query-object"></a>Kako: korištenje MSQuery

Za izvođenje složenih upita (uključujući sortiranje i broja stranica), stvaranje programa `MSQuery` objekt izravno ili pomoću predikata:

**Cilj C**:

```
MSQuery *query = [table query];
MSQuery *query = [table queryWithPredicate: [NSPredicate predicateWithFormat:@"complete == NO"]];
```

**Swift**:

```
let query = table.query()
let query = table.queryWithPredicate(NSPredicate(format: "complete == NO"))
```

`MSQuery`možete kontrolirati nekoliko upit ponašanjem.

* Navedite redoslijed rezultata
* Ograničavanje koja se polja da biste se vratili
* Ograničavanje broja zapisa da biste se vratili
* Određivanje ukupan broj u odgovor
* Navedite prilagođeni upit parametrima niza u zahtjevu za
* Primjena dodatne funkcije

Izvršavanje programa `MSQuery` upit tako da nazovete `readWithCompletion` na objekt.

## <a name="sorting"></a>Kako: sortiranje podataka pomoću MSQuery

Da biste sortirali rezultate, Pogledajmo primjer. Da biste sortirali po polju "tekst" uzlaznim redoslijedom, a zatim po "dovršeno" silazno, pozovite `MSQuery` ovako:

**Cilj C**:

```
[query orderByAscending:@"text"];
[query orderByDescending:@"complete"];
[query readWithCompletion:^(MSQueryResult *result, NSError *error) {
        if(error) {
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) {
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**Swift**:

```
query.orderByAscending("text")
query.orderByDescending("complete")
query.readWithCompletion { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```


## <a name="selecting"></a><a name="parameters"></a>Kako: ograničavanje polja i proširivanje parametrima niza upita s MSQuery

Da biste ograničili polja koja se vraća u upitu, navedite naziv polja u svojstvu **selectFields** . U ovom se primjeru vraća samo tekst i dovršene polja:

**Cilj C**:

```
query.selectFields = @[@"text", @"complete"];
```

**Swift**:

```
query.selectFields = ["text", "complete"]
```

Da biste uvrstili parametrima niza dodatne upita u zahtjevu za poslužitelja (na primjer, jer prilagođene skripte poslužiteljsko pomoću njih), popunjavanje `query.parameters` ovako:

**Cilj C**:

```
query.parameters = @{
    @"myKey1" : @"value1",
    @"myKey2" : @"value2",
};
```

**Swift**:

```
query.parameters = ["myKey1": "value1", "myKey2": "value2"]
```

## <a name="paging"></a>Kako: Konfiguriranje veličine stranice

S aplikacijama Mobile Azure, veličina stranice određuje broj zapisa koji se povlače u trenutku iz tablica pozadinskog. Poziv `pull` podataka pa bi skupna kopije podataka na temelju ovu veličinu stranice dok se ne nema više zapisa da biste izvukli.

Nije moguće konfigurirati veličine stranice pomoću **MSPullSettings** kao što je prikazano u nastavku. Zadana veličina stranice je 50, a primjeru u nastavku mijenja u 3.

Nije moguće konfigurirati drugu veličinu stranice radi boljih performansi. Ako imate velik broj slogova small podataka, veličine stranice visoke smanjuje broj spajanje na poslužitelj. 

Tom se postavkom određuje samo veličine stranice na klijentskoj strani. Ako klijent traži stranice veći od podržava pozadinska aplikacija Mobile, veličine stranice je capped maksimalnom pozadinski konfiguriran za podršku. 

Ta je postavka i _broj_ zapisa podataka, ne _bajtni veličina_.

Ako povećanje veličine stranice klijent [trebala povećati i veličine stranice na poslužitelju](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#_how-to-adjust-the-table-paging-size).

**Cilj C**:

```
  MSPullSettings *pullSettings = [[MSPullSettings alloc] initWithPageSize:3];
  [table  pullWithQuery:query queryId:@nil settings:pullSettings
                        completion:^(NSError * _Nullable error) {
                               if(error) {
                    NSLog(@"ERROR %@", error);
                } 
                           }];
```


**Swift**:

```
let pullSettings = MSPullSettings(pageSize: 3)
table.pullWithQuery(query, queryId:nil, settings: pullSettings) { (error) in
    if let err = error {
        print("ERROR ", err)
    } 
}
```

##<a name="inserting"></a>Kako: Umetanje radnog lista

Da biste umetnuli novi redak tablice, stvorite je `NSDictionary` i pozivanje `table insert`. Ako je omogućeno [Dinamički shemu] , pozadinski mobilne aplikacije servisa za Azure automatski generira nove stupce koji se temelji na na `NSDictionary`.

Ako `id` je nisu navedene, pozadinski automatski stvara novi jedinstveni ID. Navedite vlastitu `id` e-poštu sustava adrese, korisnička imena, ili vlastite prilagođene vrijednosti kao ID-a. Pružanje svoj ID olakšalo možda njihovo spojevi i logike vezanima uz tvrtke baze podataka.

U `result` sadrži nove stavke koje ste umetnuli. Ovisno o logiku server može imati dodatne ili izmijenjene podatke u usporedbi s što proslijeđen na poslužitelj.

**Cilj C**:

```
NSDictionary *newItem = @{@"id": @"custom-id", @"text": @"my new item", @"complete" : @NO};
[table insert:newItem completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**Swift**:

```
let newItem = ["id": "custom-id", "text": "my new item", "complete": false]
table.insert(newItem) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

##<a name="modifying"></a>Kako: izmjenu podataka

Da biste ažurirali postojeći redak, izmijenite stavke i poziv `update`:

**Cilj C**:

```
NSMutableDictionary *newItem = [oldItem mutableCopy]; // oldItem is NSDictionary
[newItem setValue:@"Updated text" forKey:@"text"];
[table update:newItem completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**Swift**:

```
if let newItem = oldItem.mutableCopy() as? NSMutableDictionary {
    newItem["text"] = "Updated text"
    table2.update(newItem as [NSObject: AnyObject], completion: { (result, error) -> Void in
        if let err = error {
            print("ERROR ", err)
        } else if let item = result {
            print("Todo Item: ", item["text"])
        }
    })
}
```

Umjesto toga možete navesti Identifikator retka i ažurirane polja:

**Cilj C**:

```
[table update:@{@"id":@"custom-id", @"text":"my EDITED item"} completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**Swift**:

```
table.update(["id": "custom-id", "text": "my EDITED item"]) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

Barem na `id` atributa mora biti postavljeno pri stvaranju ažuriranja.

##<a name="deleting"></a>Kako: brisanje podataka

Da biste izbrisali neku stavku, pozovite `delete` sa stavkom:

**Cilj C**:

```
[table delete:item completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

**Swift**:

```
table.delete(newItem as [NSObject: AnyObject]) { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

Osim toga, izbrišite unosom Identifikatora retka:

**Cilj C**:

```
[table deleteWithId:@"37BBF396-11F0-4B39-85C8-B319C729AF6D" completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

**Swift**:

```
table.deleteWithId("37BBF396-11F0-4B39-85C8-B319C729AF6D") { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

Barem na `id` atributa mora biti postavljen kada unošenju briše.

##<a name="customapi"></a>Kako: poziva prilagođene API-JA

Pomoću prilagođenih API-JA, možete izložiti sve funkcije pozadinskog. Nema da biste mapirali operacije na tablicu. Ne samo želite dobiti veću kontrolu nad poruka, možete čak i čitanje/skup zaglavlja i promijeniti oblik tijela odgovor. Da biste saznali kako stvoriti prilagođene API na pozadinski, pročitajte [Prilagođene API -ji](app-service-mobile-node-backend-how-to-use-server-sdk.md#work-easy-apis)

Da biste nazvali prilagođene API poziva `MSClient.invokeAPI`. Zahtjeva i odgovora sadržaja smatraju JSON. Da biste koristili druge vrste medijskih sadržaja [pomoću preopterećenja od `invokeAPI` ] [ 5].  Da biste na `GET` zahtjev umjesto na `POST` zahtjev, skup parametar `HTTPMethod` da biste `"GET"` i parametar `body` da biste `nil` (budući da zahtjevi za DOHVAĆANJE nemaju tijela poruke.) Ako prilagođena API-JA podržava druge HTTP glagoli, promijenite `HTTPMethod` pravilno.

**Cilj C**:

```
[self.client invokeAPI:@"sendEmail"
                  body:@{ @"contents": @"Hello world!" }
            HTTPMethod:@"POST"
            parameters:@{ @"to": @"bill@contoso.com", @"subject" : @"Hi!" }
               headers:nil
            completion: ^(NSData *result, NSHTTPURLResponse *response, NSError *error) {
                if(error) {
                    NSLog(@"ERROR %@", error);
                } else {
                    // Do something with result
                }
            }];
```

**Swift**:

```
client.invokeAPI("sendEmail",
            body: [ "contents": "Hello World" ],
            HTTPMethod: "POST",
            parameters: [ "to": "bill@contoso.com", "subject" : "Hi!" ],
            headers: nil)
            {
                (result, response, error) -> Void in
                if let err = error {
                    print("ERROR ", err)
                } else if let res = result {
                          // Do something with result
                }
        }
```

##<a name="templates"></a>Kako: Register automatske predložaka za slanje obavijesti za različite platforme

Da biste registrirali predlošci, Prenesite predloške metodom **client.push registerDeviceToken** aplikaciji klijenta.

**Cilj C**:

```
[client.push registerDeviceToken:deviceToken template:iOSTemplate completion:^(NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    }
}];
```

**Swift**:

```
    client.push?.registerDeviceToken(NSData(), template: iOSTemplate, completion: { (error) in
        if let err = error {
            print("ERROR ", err)
        }
    })
```

Predloške su vrste NSDictionary i mogu sadržavati više predložaka u sljedećem obliku:

**Cilj C**:

```
NSDictionary *iOSTemplate = @{ @"templateName": @{ @"body": @{ @"aps": @{ @"alert": @"$(message)" } } } };
```

**Swift**:

```
let iOSTemplate = ["templateName": ["body": ["aps": ["alert": "$(message)"]]]]
```

Sve oznake uklanjaju iz zahtjeva za vrijednosnicu.  Da biste dodali oznake instalacije ili predložaka unutar instalacije, potražite u članku [Rad s poslužiteljem pozadinske .NET SDK za Azure mobilne aplikacije][4].  Da biste poslali obavijesti vezanih uz pomoću ove registriranih predložaka, rad s [Obavijesti koncentratora API -ji][3].

##<a name="errors"></a>Kako: obradu pogrešaka

Prilikom pozivanja na pozadinskog mobilne aplikacije servisa za Azure bloka dovršetka sadrži programa `NSError` parametar. Kada se pojavi pogreška, taj parametar je vrijednost pogreške koje nisu null. U kodu, potvrdite taj parametar i rukovati pogrešku ako je potrebno, kao što je prikazano u prethodnom koda.

Datoteka [`<WindowsAzureMobileServices/MSError.h>`] [6] definira konstante `MSErrorResponseKey`, `MSErrorRequestKey`, i `MSErrorServerItemKey`. Da biste dobili više podataka koji se odnose na pogrešku:

**Cilj C**:

```
NSDictionary *serverItem = [error.userInfo objectForKey:MSErrorServerItemKey];
```

**Swift**:

```
let serverItem = error.userInfo[MSErrorServerItemKey]
```

Osim toga, datoteku definira konstante za svaki kod pogreške:

**Cilj C**:

```
if (error.code == MSErrorPreconditionFailed) {
```

**Swift**:

```
if (error.code == MSErrorPreconditionFailed) {
```

## <a name="adal"></a>Kako: provjere autentičnosti korisnika završavaju na provjera autentičnosti biblioteke imenika Active Directory

U Active Directory provjera autentičnosti biblioteke (ADAL) možete koristiti da biste se prijavili korisnicima u aplikaciji pomoću servisa Azure Active Directory. Provjera autentičnosti tijek klijenta pomoću davatelja identiteta SDK se preporučuje korištenje na `loginWithProvider:completion:` način.  Provjera autentičnosti klijenta tijek nudi više nativni UX dojam i omogućuje dodatne prilagodbe.

1. Konfiguriranje sustava pozadinski mobilne aplikacije za prijavu AAD slijedeći [upute za konfiguriranje aplikacije servisa za prijavu na servisu Active Directory] [ 7] vodič. Provjerite jeste li dovršili neobavezan korak registriranja aplikacija za nativni klijent. Za iOS, preporučujemo da preusmjeravanje URI je obrasca `<app-scheme>://<bundle-id>`. Dodatne informacije potražite u članku [brzi početak rada sa sustavom iOS ADAL][8].

2. Instalirajte ADAL pomoću Cocoapods. Uređivanje vaše Podfile da biste dodali sljedeću definiciju zamjene **Vaš PROJEKTA** s nazivom Xcode projekta:

        source 'https://github.com/CocoaPods/Specs.git'
        link_with ['YOUR-PROJECT']
        xcodeproj 'YOUR-PROJECT'

   i u Pod:

        pod 'ADALiOS'

3. Korištenje Terminal, pokrenite `pod install` iz imenika koji sadrži projekta, a zatim otvorite generirani Xcode radnog prostora (ne projekt).

4. Dodavanje koda za sljedeće aplikacije, prema jezik koji koristite. U svakoj, provjerite sljedeće zamjena:

    * **Umetanje IZDAVANJE ovdje** zamijenite nazivom klijenta dodjeli aplikacije. Oblik mora biti https://login.windows.net/contoso.onmicrosoft.com. Ta vrijednost mogu kopirati na kartici domena u vašem Azure Active Directory [Azure klasični portalu].
    * **Umetanje-RESURSA – ID-ovdje** zamijenite ID klijenta za vaše pozadinskog mobilne aplikacije. ID klijenta možete dobiti na kartici **Dodatno** u odjeljku **Azure Active Directory postavke** na portalu.
    * **Umetanje-KLIJENT-ID-ovdje** zamijenite ID klijenta koji ste kopirali iz aplikacije za nativni klijent.
    * **Umetanje-PREUSMJERAVANJE-URI-ovdje** zamijenite krajnje _/.auth/login/done_ web-mjesta, pomoću HTTPS shemu. Ta vrijednost mora biti sličan _https://contoso.azurewebsites.net/.auth/login/done_.

**Cilj C**:

    #import <ADALiOS/ADAuthenticationContext.h>
    #import <ADALiOS/ADAuthenticationSettings.h>
    // ...
    - (void) authenticate:(UIViewController*) parent
               completion:(void (^) (MSUser*, NSError*))completionBlock;
    {
        NSString *authority = @"INSERT-AUTHORITY-HERE";
        NSString *resourceId = @"INSERT-RESOURCE-ID-HERE";
        NSString *clientId = @"INSERT-CLIENT-ID-HERE";
        NSURL *redirectUri = [[NSURL alloc]initWithString:@"INSERT-REDIRECT-URI-HERE"];
        ADAuthenticationError *error;
        ADAuthenticationContext *authContext = [ADAuthenticationContext authenticationContextWithAuthority:authority error:&error];
        authContext.parentController = parent;
        [ADAuthenticationSettings sharedInstance].enableFullScreen = YES;
        [authContext acquireTokenWithResource:resourceId
                                     clientId:clientId
                                  redirectUri:redirectUri
                              completionBlock:^(ADAuthenticationResult *result) {
                                  if (result.status != AD_SUCCEEDED)
                                  {
                                      completionBlock(nil, result.error);;
                                  }
                                  else
                                  {
                                      NSDictionary *payload = @{
                                                                @"access_token" : result.tokenCacheStoreItem.accessToken
                                                                };
                                      [client loginWithProvider:@"aad" token:payload completion:completionBlock];
                                  }
                              }];
    }


**Swift**:

    // add the following imports to your bridging header:
    //      #import <ADALiOS/ADAuthenticationContext.h>
    //      #import <ADALiOS/ADAuthenticationSettings.h>

    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let authority = "INSERT-AUTHORITY-HERE"
        let resourceId = "INSERT-RESOURCE-ID-HERE"
        let clientId = "INSERT-CLIENT-ID-HERE"
        let redirectUri = NSURL(string: "INSERT-REDIRECT-URI-HERE")
        var error: AutoreleasingUnsafeMutablePointer<ADAuthenticationError?> = nil
        let authContext = ADAuthenticationContext(authority: authority, error: error)
        authContext.parentController = parent
        ADAuthenticationSettings.sharedInstance().enableFullScreen = true
        authContext.acquireTokenWithResource(resourceId, clientId: clientId, redirectUri: redirectUri) { (result) in
                if result.status != AD_SUCCEEDED {
                    completion(nil, result.error)
                }
                else {
                    let payload: [String: String] = ["access_token": result.tokenCacheStoreItem.accessToken]
                    client.loginWithProvider("aad", token: payload, completion: completion)
                }
            }
    }

## <a name="facebook-sdk"></a>Kako: provjeru autentičnosti korisnika s Facebooka SDK za iOS

Facebook SDK za iOS možete koristiti da biste se prijavili korisnicima u aplikaciji pomoću servisa Facebook.  Pomoću provjere autentičnosti na za klijent tijek se preporučuje korištenje na `loginWithProvider:completion:` način.  Provjera autentičnosti klijenta tijek nudi više nativni UX dojam i omogućuje dodatne prilagodbe.

1. Konfiguriranje sustava pozadinski mobilne aplikacije za Facebook prijavu slijedeći [upute za konfiguriranje aplikacije servisa za prijavu na Facebook] [ 9] vodič.

2. Instalirajte SDK Facebook za iOS slijedeći [Facebook SDK za iOS – prvi koraci] [ 10] dokumentacije. Umjesto stvaranja aplikacije, možete dodati platformu iOS svoju registraciju postojeće. 

3. Dokumentacija za Facebook, obuhvaća neke kod cilj C u delegat aplikacije. Ako koristite **Swift**, možete koristiti sljedeće prijevode za AppDelegate.swift:
  
        // Add the following import to your bridging header:
        //      #import <FBSDKCoreKit/FBSDKCoreKit.h>
        
        func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool {
            FBSDKApplicationDelegate.sharedInstance().application(application, didFinishLaunchingWithOptions: launchOptions)
            // Add any custom logic here.
            return true
        }

        func application(application: UIApplication, openURL url: NSURL, sourceApplication: String?, annotation: AnyObject?) -> Bool {
            let handled = FBSDKApplicationDelegate.sharedInstance().application(application, openURL: url, sourceApplication: sourceApplication, annotation: annotation)
            // Add any custom logic here.
            return handled
        }

4. Osim dodavanja `FBSDKCoreKit.framework` u projekt i dodajte referencu `FBSDKLoginKit.framework` na isti način. 

4. Dodavanje koda za sljedeće aplikacije, prema jezik koji koristite. 

**Cilj C**:

    #import <FBSDKLoginKit/FBSDKLoginKit.h>
    #import <FBSDKCoreKit/FBSDKAccessToken.h>
    // ...
    - (void) authenticate:(UIViewController*) parent
               completion:(void (^) (MSUser*, NSError*)) completionBlock;
    {       
        FBSDKLoginManager *loginManager = [[FBSDKLoginManager alloc] init];
        [loginManager
         logInWithReadPermissions: @[@"public_profile"]
         fromViewController:parent
         handler:^(FBSDKLoginManagerLoginResult *result, NSError *error) {
             if (error) {
                 completionBlock(nil, error);
             } else if (result.isCancelled) {
                 completionBlock(nil, error);
             } else {
                 NSDictionary *payload = @{
                                           @"access_token":result.token.tokenString
                                           };
                 [client loginWithProvider:@"facebook" token:payload completion:completionBlock];
             }
         }];
    }

**Swift**:

    // Add the following imports to your bridging header:
    //      #import <FBSDKLoginKit/FBSDKLoginKit.h>
    //      #import <FBSDKCoreKit/FBSDKAccessToken.h>
    
    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let loginManager = FBSDKLoginManager()
        loginManager.logInWithReadPermissions(["public_profile"], fromViewController: parent) { (result, error) in
            if (error != nil) {
                completion(nil, error)
            }
            else if result.isCancelled {
                completion(nil, error)
            }
            else {
                let payload: [String: String] = ["access_token": result.token.tokenString]
                client.loginWithProvider("facebook", token: payload, completion: completion)
            }
        }
    }

## <a name="twitter-fabric"></a>Kako: provjeru autentičnosti korisnika tkaninom na Twitteru za iOS

Tkanina za iOS možete koristiti da biste se prijavili korisnicima u aplikaciji pomoću Twitter. Provjera autentičnosti klijenta tijek se preporučuje korištenje na `loginWithProvider:completion:` način, kao što je pruža više nativni UX dojam i omogućuje dodatne prilagodbe.

1. Konfiguriranje sustava pozadinski mobilne aplikacije za prijavu Twitter slijedeći [upute za konfiguriranje aplikacije servisa za prijavu na Twitter](app-service-mobile-how-to-configure-twitter-authentication.md) vodič.

2. Dodajte tkanina projekta praćenja u dokumentaciji [tkanina za iOS – prvi koraci] i postavljanjem TwitterKit.

    > [AZURE.NOTE] Prema zadanim postavkama tkanina za vas stvara Twitter aplikacije. Možete izbjegavajte stvaranje aplikacije po Registriranje potrošača ključ i potrošača tajna koji ste stvorili ranije pomoću sljedećih koda.  Osim toga, možete zamijeniti potrošača ključ i potrošača tajna vrijednosti koje nude aplikacije servisa s vrijednostima koje vidite u [Tkanina nadzorne ploče]. Ako odaberete tu mogućnost, ne zaboravite da biste postavili URL povratnog vrijednosti rezervirano mjesto, kao što su `https://<yoursitename>.azurewebsites.net/.auth/login/twitter/callback`.

    Ako odlučite koristiti tajne koju ste ranije stvorili, dodavanje sljedeći kod svog delegata aplikacije:
    
    **Cilj C**:

        #import <Fabric/Fabric.h>
        #import <TwitterKit/TwitterKit.h>
        // ...
        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
            [[Twitter sharedInstance] startWithConsumerKey:@"your_key" consumerSecret:@"your_secret"];
            [Fabric with:@[[Twitter class]]];
            // Add any custom logic here.
            return YES;
        }
        
    **Swift**:
    
        import Fabric
        import TwitterKit
        // ...
        func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool {
            Twitter.sharedInstance().startWithConsumerKey("your_key", consumerSecret: "your_secret")
            Fabric.with([Twitter.self])
            // Add any custom logic here.
            return true
        }
    
3. Dodavanje koda za sljedeće aplikacije, prema jezik koji koristite. 

**Cilj C**:

    #import <TwitterKit/TwitterKit.h>
    // ...
    - (void)authenticate:(UIViewController*)parent completion:(void (^) (MSUser*, NSError*))completionBlock
    {
        [[Twitter sharedInstance] logInWithCompletion:^(TWTRSession *session, NSError *error) {
            if (session) {
                NSDictionary *payload = @{
                                            @"access_token":session.authToken,
                                            @"access_token_secret":session.authTokenSecret
                                        };
                [client loginWithProvider:@"twitter" token:payload completion:completionBlock];
            } else {
                completionBlock(nil, error);
            }
        }];
    }

**Swift**:

    import TwitterKit
    // ...
    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let client = self.table!.client
        Twitter.sharedInstance().logInWithCompletion { session, error in
            if (session != nil) {
                let payload: [String: String] = ["access_token": session!.authToken, "access_token_secret": session!.authTokenSecret]
                client.loginWithProvider("twitter", token: payload, completion: completion)
            } else {
                completion(nil, error)
            }
        }
    }

## <a name="google-sdk"></a>Kako: provjeru autentičnosti korisnika s SDK Google, prijave za iOS

Google prijavu SDK-a za iOS možete koristiti da biste se prijavili korisnicima u aplikaciji pomoću Google račun.  Google nedavno objaviti promjene njihova OAuth sigurnosna pravila.  Promjene pravila u budućnosti će zahtijevaju korištenje Google SDK.

1. Konfiguriranje sustava pozadinski mobilne aplikacije za prijavu Google slijedeći [upute za konfiguriranje aplikacije servisa za prijavu u Google](app-service-mobile-how-to-configure-google-authentication.md) vodič.

2. Instalirajte Google SDK za iOS slijedeći u dokumentaciji za [prijavu Google za iOS – pokretanje integriranje](https://developers.google.com/identity/sign-in/ios/start-integrating) . Možda prijeđite u odjeljku "Autentičnost s na pozadinskog poslužitelja".

3. Dodajte sljedeće svog delegata `signIn:didSignInForUser:withError:` metoda prema jezik koji koristite.

**Cilj C**:

        NSDictionary *payload = @{
                                  @"id_token":user.authentication.idToken,
                                  @"authorization_code":user.serverAuthCode
                                  };
        
        [client loginWithProvider:@"google" token:payload completion:^(MSUser *user, NSError *error) {
            // ...
        }];

**Swift**:

        let payload: [String: String] = ["id_token": user.authentication.idToken, "authorization_code": user.serverAuthCode]
        client.loginWithProvider("google", token: payload) { (user, error) in
            // ...
        }

4. Provjerite je li dodate na sljedeći način `application:didFinishLaunchingWithOptions:` u svog delegata aplikacije "SERVER_CLIENT_ID" zamjenjuje isti ID koji ste koristili za konfiguriranje servisa za aplikacije u koraku 1.

**Cilj C**:

        [GIDSignIn sharedInstance].serverClientID = @"SERVER_CLIENT_ID";
 
 **Swift**:
 
        GIDSignIn.sharedInstance().serverClientID = "SERVER_CLIENT_ID"

 
 5. Dodavanje koda za sljedeće aplikacije u UIViewController koji implementira u `GIDSignInUIDelegate` protokol, u skladu sa standardima jezika koji koristite.  Odjavljeni prije nego što se ponovno prijavite i Iako ne morate ponovno unesite vjerodajnice, pogledajte pristanak dijaloškog okvira.  Samo pozvati ovu metodu dok je istekla token sesiju.
 
 **Cilj C**:

        #import <Google/SignIn.h>
        // ...
        - (void)authenticate
        {
                [GIDSignIn sharedInstance].uiDelegate = self;
                [[GIDSignIn sharedInstance] signOut];
                [[GIDSignIn sharedInstance] signIn];
        }
 
 **Swift**:
    
        // ...
        func authenticate() {
            GIDSignIn.sharedInstance().uiDelegate = self
            GIDSignIn.sharedInstance().signOut()
            GIDSignIn.sharedInstance().signIn()
        }
        
<!-- Anchors. -->

[What is Mobile Services]: #what-is
[Concepts]: #concepts
[Setup and Prerequisites]: #Setup
[How to: Create the Mobile Services client]: #create-client
[How to: Create a table reference]: #table-reference
[How to: Query data from a mobile service]: #querying
[Filter returned data]: #filtering
[Sort returned data]: #sorting
[Return data in pages]: #paging
[Select specific columns]: #selecting
[How to: Bind data to the user interface]: #binding
[How to: Insert data into a mobile service]: #inserting
[How to: Modify data in a mobile service]: #modifying
[How to: Authenticate users]: #authentication
[Cache authentication tokens]: #caching-tokens
[How to: Upload images and large files]: #blobs
[How to: Handle errors]: #errors
[How to: Design unit tests]: #unit-testing
[How to: Customize the client]: #customizing
[Customize request headers]: #custom-headers
[Customize data type serialization]: #custom-serialization
[Next Steps]: #next-steps
[How to: Use MSQuery]: #query-object

<!-- Images. -->

<!-- URLs. -->
[Azure mobilne aplikacije Quick Start]: app-service-mobile-ios-get-started.md

[Add Mobile Services to Existing App]: /develop/mobile/tutorials/get-started-data
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Validate and modify data in Mobile Services by using server scripts]: /develop/mobile/tutorials/validate-modify-and-augment-data-ios
[Mobile Services SDK]: https://go.microsoft.com/fwLink/p/?LinkID=266533
[Authentication]: /develop/mobile/tutorials/get-started-with-users-ios
[iOS SDK]: https://developer.apple.com/xcode

[Handling Expired Tokens]: http://go.microsoft.com/fwlink/p/?LinkId=301955
[Live Connect SDK]: http://go.microsoft.com/fwlink/p/?LinkId=301960
[Permissions]: http://msdn.microsoft.com/library/windowsazure/jj193161.aspx
[Service-side Authorization]: mobile-services-javascript-backend-service-side-authorization.md
[Use scripts to authorize users]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
[Dinamični sheme]: http://go.microsoft.com/fwlink/p/?LinkId=296271
[How to: access custom parameters]: /develop/mobile/how-to-guides/work-with-server-scripts#access-headers
[Create a table]: http://msdn.microsoft.com/library/windowsazure/jj193162.aspx
[NSDictionary object]: http://go.microsoft.com/fwlink/p/?LinkId=301965
[ASCII control codes C0 and C1]: http://en.wikipedia.org/wiki/Data_link_escape_character#C1_set
[CLI to manage Mobile Services tables]: ../virtual-machines-command-line-tools.md#Mobile_Tables
[Conflict-Handler]: mobile-services-ios-handling-conflicts-offline-data.md#add-conflict-handling

[Tkanina nadzorne ploče]: https://www.fabric.io/home
[Tkanina za iOS – prvi koraci]: https://docs.fabric.io/ios/fabric/getting-started.html
[1]: https://github.com/Azure/azure-mobile-apps-ios-client/blob/master/README.md#ios-client-sdk
[2]: http://azure.github.io/azure-mobile-apps-ios-client/
[3]: https://msdn.microsoft.com/library/azure/dn495101.aspx
[4]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#tags
[5]: http://azure.github.io/azure-mobile-services/iOS/v3/Classes/MSClient.html#//api/name/invokeAPI:data:HTTPMethod:parameters:headers:completion:
[6]: https://github.com/Azure/azure-mobile-services/blob/master/sdk/iOS/src/MSError.h
[7]: app-service-mobile-how-to-configure-active-directory-authentication.md
[8]: ../active-directory/active-directory-devquickstarts-ios.md#em1-determine-what-your-redirect-uri-will-be-for-iosem
[9]: app-service-mobile-how-to-configure-facebook-authentication.md
[10]: https://developers.facebook.com/docs/ios/getting-started
