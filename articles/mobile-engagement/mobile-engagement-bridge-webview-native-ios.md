<properties 
    pageTitle="IOS most prikaza uz nativni Mobile radnje iOS SDK" 
    description="U članku se opisuje kako stvoriti most između prikaza koji se izvodi Javascript i izvorni iOS Mobile radnje SDK"      
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
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="bridge-ios-webview-with-native-mobile-engagement-ios-sdk"></a>IOS most prikaza uz nativni Mobile radnje iOS SDK

> [AZURE.SELECTOR]
- [Android mosta](mobile-engagement-bridge-webview-native-android.md)
- [iOS mosta](mobile-engagement-bridge-webview-native-ios.md)

Neke mobilne aplikacije osmišljeni su kao hibridnog aplikacije kojima samoj aplikaciji je razvio pomoću razvoja izvorne iOS cilj C, ali neke ili sve na zaslonima čak i prikazivanja unutar iOS prikaza. I dalje mogu zauzeti Mobile radnje iOS SDK unutar takve aplikacije i ovog praktičnog vodiča opisuje kako o na taj način. 

Postoje dva načina prikaza da biste postigli iako su oba nedokumentiranom:

- Najprije nešto opisan je [veza](http://stackoverflow.com/questions/9826792/how-to-invoke-objective-c-method-from-javascript-and-send-back-data-to-javascrip) koji se odnosi na Prijava na `UIWebViewDelegate` na web-tablice i zamka – i – odmah-Odustani mjesto promjena u Javascript. 
- Drugi nešto temelji se na ovu [sesiju WWDC 2013](https://developer.apple.com/videos/play/wwdc2013/615), na način koji je čišćenje od prvoga i što smo slijediti ovaj vodič. Imajte na umu da takvog funkcionira samo na iOS7 i noviji. 

Za sustava iOS premostiti uzorka, slijedite ove korake:

1. Najprije, morate biti sigurni da imate nestaje kroz naš [Vodič za početak rada](mobile-engagement-ios-get-started.md) da biste integrirali iOS Mobile radnje SDK aplikaciji hibridnog. Po želji možete omogućiti test na sljedeći način prijave tako da možete vidjeti metode SDK dok ih ne možemo pokrenuti-u prikaza. 
    
        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
           ....
            [EngagementAgent setTestLogEnabled:YES];
           ....
        }

2. Sada provjerite je li hibridnog aplikacije sadrži zaslon s s prikaza na njemu. Da biste možete dodati u `Main.storyboard` aplikacije. 

3. Pridružiti ovaj prikaza **ViewController** tako da kliknete i povučete u prikaza iz scenu kontroleru prikaz da biste na `ViewController.h` uređivanje zaslonu tako da ga smjestite neposredno ispod na `@interface` redak. 

4. Kada to učinite, dijaloški okvir pojavit će se zatražiti naziv. Navedite naziv kao **prikaza**. Vaše `ViewController.h` datoteke trebao bi izgledati ovako:

        #import <UIKit/UIKit.h>
        #import "EngagementViewController.h"
        
        @interface ViewController : EngagementViewController
        @property (strong, nonatomic) IBOutlet UIWebView *webView;
        
        @end

5. Ne možemo ažurirat će se u `ViewController.m` datoteka kasnije, no najprije će stvoriti datoteku most koji stvara u omot putem nekih često korištenih iOS Mobile radnje SDK metode. Stvaranje nove datoteke zaglavlje naziva **EngagementJsExports.h** koji koristi u `JSExport` mehanizam opisane u ispunjavaju prethodno navedene [sesiju](https://developer.apple.com/videos/play/wwdc2013/615) da biste otkrili metode nativni iOS. 

        #import <Foundation/Foundation.h>
        #import <JavaScriptCore/JavascriptCore.h>
        
        @protocol EngagementJsExports <JSExport>
        
        + (void) sendEngagementEvent:(NSString*) name :(NSString*)extras;
        + (void) startEngagementJob:(NSString*) name :(NSString*)extras;
        + (void) endEngagementJob:(NSString*) name;
        + (void) sendEngagementError:(NSString*) name :(NSString*)extras;
        + (void) sendEngagementAppInfo:(NSString*) appInfo;
        
        @end

        @interface EngagementJs : NSObject <EngagementJsExports>

        @end

6. Zatim ćemo stvoriti drugi dio datoteke most. Stvorite datoteku s nazivom **EngagementJsExports.m** koje će sadržavati implementaciju stvaranje stvarni wrappers tako da nazovete metode SDK iOS Mobile radnje. I Imajte na umu da mi se analize u `extras` predugački iz prikaza javascript i umetanja koji u programa `NSMutableDictionary` objekta koji želite proslijediti način pozive SDK radnje.  

        #import <UIKit/UIKit.h>
        #import "EngagementAgent.h"
        #import "EngagementJsExports.h"
        
        @implementation EngagementJs
        
        +(void) sendEngagementEvent:(NSString*)name :(NSString*)extras {
           NSMutableDictionary* extrasInput = [self ParseExtras:extras];
           [[EngagementAgent shared] sendEvent:name extras:extrasInput];
        }
        
        + (void) startEngagementJob:(NSString*) name :(NSString*)extras {
           NSMutableDictionary* extrasInput = [self ParseExtras:extras];
           [[EngagementAgent shared] startJob:name extras:extrasInput];
        }
        
        + (void) endEngagementJob:(NSString*) name {
           [[EngagementAgent shared] endJob:name];
        }
        
        + (void) sendEngagementError:(NSString*) name :(NSString*)extras {
           NSMutableDictionary* extrasInput = [self ParseExtras:extras];
           [[EngagementAgent shared] sendError:name extras:extrasInput];
        }
        
        + (void) sendEngagementAppInfo:(NSString*) appInfo {
           NSMutableDictionary* appInfoInput = [self ParseExtras:appInfo];
           [[EngagementAgent shared] sendAppInfo:appInfoInput];
        }
        
        + (NSMutableDictionary*) ParseExtras:(NSString*) input {
           NSData *data = [input dataUsingEncoding:NSUTF8StringEncoding];
           NSError* error = nil;
           NSMutableDictionary* extras = [NSJSONSerialization JSONObjectWithData:data options:0 error:&error];
           
           return extras;
        }
        
        @end

5. Sada ćemo vratite se na **ViewController.m** i ažurirati sljedeći kod: 
        
        #import <JavaScriptCore/JavaScriptCore.h>
        #import "ViewController.h"
        #import "EngagementJsExports.h"
        
        @interface ViewController ()
        
        @end
        
        @implementation ViewController
        
        - (void)viewDidLoad
        {
           self.webView.delegate = self;
           [super viewDidLoad];
           [self loadWebView];
        }
        
        - (void)loadWebView {
           NSBundle* mainBundle = [NSBundle mainBundle];
           NSURL* htmlPage = [mainBundle URLForResource:@"LocalPage" withExtension:@"html"];
           
           NSURLRequest* urlReq = [NSURLRequest requestWithURL:htmlPage];
           [self.webView loadRequest:urlReq];
        }
        
        - (void)webViewDidFinishLoad:(UIWebView*)wv
        {
           JSContext* context = [wv valueForKeyPath:@"documentView.webView.mainFrame.javaScriptContext"];
           
           context[@"EngagementJs"] = [EngagementJs class];
        }
        
        - (void)webView:(UIWebView*)wv didFailLoadWithError:(NSError*)error
        {
           NSLog(@"Error for WEBVIEW: %@", [error description]);
        }
        
        - (void)didReceiveMemoryWarning {
           [super didReceiveMemoryWarning];
           // Dispose of any resources that can be recreated.
        }
        
        @end

6. Imajte na umu sljedeće točke o datoteci **ViewController.m** :

    - U na `loadWebView` način, ne možemo učitavaju lokalni HTML datoteku pod nazivom **LocalPage.html** čije kod smo pregledati i dalje. 
    - U na `webViewDidFinishLoad` način, ne možemo su grabbing na `JsContext` i pridruživanje je naš omot predmete. Tako ćete omogućiti pozivanje našeg omot SDK metode pomoću ručice za **EngagementJs** -u prikaza. 

7. Stvaranje datoteke naziva **LocalPage.html** sa sljedeći kod:

        <!doctype html>
        <html>
           <head>
               <style type='text/css'>
                   html { font-family:Helvetica; color:#222; }
                   h1 { color:steelblue; font-size:22px; margin-top:16px; }
                   h2 { color:grey; font-size:14px; margin-top:18px; margin-bottom:0px; }
               </style>
               
               <script type="text/javascript">
               
               window.onerror = function(err)
               {
                   alert('window.onerror: ' + err);
               }
               
               function send(inputId)
               {
                   var input = document.getElementById(inputId);
                   if(input)
                   {
                       var value = input.value;
                       // Example of how extras info can be passed with the Engagement logs
                       var extras = '{"CustomerId":"MS290011"}';
                   }
                   
                   if(value && value.length > 0)
                   {
                       switch(inputId)
                       {
                           case "event":
                           EngagementJs.sendEngagementEvent(value, extras);
                           break;
                           
                           case "job":
                           EngagementJs.startEngagementJob(value, extras);
                           window.setTimeout(
                           function(){
                               EngagementJs.endEngagementJob(value);
                           }, 10000 );
                           break;
        
                           case "error":
                           EngagementJs.sendEngagementError(value, extras);
                           break;
                           
                           case "appInfo":
                           var appInfo = '{"customer_name":"' + value + '"}';
                           EngagementJs.sendEngagementAppInfo(appInfo);
                           break;
                       }
                   }
               }
               </script>
               
           </head>
           <body>
               <h1>Bridge Tester</h1>
               
               <div id='engagement'>
                   
                   <br/>
                   <h2>Event</h2>
                   <input type="text" id="event" size="35">
                   <button onclick="send('event')">Send</button>
                   
                   <br/>
                   <h2>Job</h2>
                   <input type="text" id="job" size="35">
                   <button onclick="send('job')">Send</button>
                   
                   <br/>
                   <h2>Error</h2>
                   <input type="text" id="error" size="35">
                   <button onclick="send('error')">Send</button
                   
                   <br/>
                   <h2>AppInfo</h2>
                   <input type="text" id="appInfo" size="35">
                   <button onclick="send('appInfo')">Send</button>
               
               </div>
           </body>
        </html>

8. Imajte na umu sljedeće točke o iznad HTML datoteku:

    -   Sadrži skup unos okviri u koje možete unijeti podatke koji će se koristiti kao nazive na događaj, posla, pogreška, AppInfo. Prilikom klika na gumb pokraj tog prikaza, Javascript koja naposljetku poziva metode iz datoteke most prenesite poziv i iOS Mobile radnje SDK upućuje se poziv. 
    -   Ne možemo označavanje na neki statične dodatne informacije da biste događaja, zadatke i čak pogreške da bismo pokazali kako to možete učiniti. Dodatne informacije šalju kao što je JSON niz koji, ako vam se prikazivati na `EngagementJsExports.m` datoteke, je raščlaniti i proslijeđena uz slanje događaje, zadatke, pogreške. 
    -   Radnje posao Mobile je kicked pod nazivom koji ste naveli u okviru za unos pokretanje za 10 sekundi i isključiti. 
    -   Radnje Mobile appinfo ili oznakom se prenosi s customer_name kao statične ključ i vrijednost koju ste unijeli u unos kao vrijednost za oznaku. 
 
9. Pokrenite aplikaciju i vidjet ćete sljedeće. Sada upišite neki ime za testiranje događaja kao što je sljedeća i kliknite **Pošalji** uz nju. 

    ![][1]

10. Sada ako odete na kartici **Monitor** aplikacije i pogledajte informacije u odjeljku **događaji -> detalje**, prikazat će se događaj prikazivati uz statične aplikacije-informacije koje ćemo šaljete. 

    ![][2]

<!-- Images. -->
[1]: ./media/mobile-engagement-bridge-webview-native-ios/sending-event.png
[2]: ./media/mobile-engagement-bridge-webview-native-ios/event-output.png
