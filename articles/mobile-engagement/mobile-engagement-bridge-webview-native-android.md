<properties 
    pageTitle="Most Android prikaza s nativni Mobile radnje sa sustavom Android SDK" 
    description="U članku se opisuje kako stvoriti most između prikaza koji se izvodi Javascript i izvorni Mobile radnje sa sustavom Android SDK-a"      
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="erikre" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-android" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="bridge-android-webview-with-native-mobile-engagement-android-sdk"></a>Most Android prikaza s nativni Mobile radnje sa sustavom Android SDK

> [AZURE.SELECTOR]
- [Android mosta](mobile-engagement-bridge-webview-native-android.md)
- [iOS mosta](mobile-engagement-bridge-webview-native-ios.md)

Neke mobilne aplikacije osmišljeni su kao hibridnog aplikacije kojima samoj aplikaciji je razvio pomoću nativni razvoj Android, ali neke ili sve na zaslonima čak i prikazivanja unutar Android prikaza. I dalje mogu zauzeti Mobile radnje sa sustavom Android SDK kao što je aplikacija i ovog praktičnog vodiča opisuje kako o na taj način. Šifre uzorka temelji se na Android dokumentaciju [ovdje](https://developer.android.com/guide/webapps/webview.html#BindingJavaScript). Opisuje kako takvog dokumentirane nije moguće koristiti za implementaciju na isti način za najčešće korištene metode Mobile radnje sa sustavom Android SDK-tako da se prikaza iz aplikacije za hibridno možete i pokrenuti zahtjevi za praćenje događaja, zadatke, pogreške, aplikacija informacije dok ih piping putem naš SDK sa sustavom Android. 

1. Najprije, morate biti sigurni da imate nestaje kroz naš [Vodič za početak rada](mobile-engagement-android-get-started.md) da biste integrirali Mobile SDK Android radnje u svojoj aplikaciji hibridnog. Kada to učinite, vaše `OnCreate` način će izgledati ovako.  
    
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
    
            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("<Mobile Engagement Conn String>");
            EngagementAgent.getInstance(this).init(engagementConfiguration);
        }

2. Sada provjerite je li hibridnog aplikacije sadrži zaslon s s prikaza na njemu. Kod za njega će biti sličan sljedeće gdje se učitavaju lokalni HTML datoteka **Sample.html** u prikaza u na `onCreate` način zaslona. 

        private void SetWebView() {
            WebView myWebView = (WebView) findViewById(R.id.webview);
            myWebView.loadUrl("file:///android_asset/Sample.html");
        }

        protected void onCreate(Bundle savedInstanceState) {
            ...
            ...
            SetWebView();
        }

3. Sada stvoriti most datoteku s nazivom **WebAppInterface** koji stvara u omot iznad neke najčešće korištenih Mobile radnje sa sustavom Android SDK metode pomoću na `@JavascriptInterface` pristup što je opisano u [dokumentaciji za Android](https://developer.android.com/guide/webapps/webview.html#BindingJavaScript):

        import android.content.Context;
        import android.os.Bundle;
        import android.util.Log;
        import android.webkit.JavascriptInterface;
        
        import com.microsoft.azure.engagement.EngagementAgent;
        
        import org.json.JSONArray;
        import org.json.JSONObject;
        
        public class WebAppInterface {
            Context mContext;
        
            /** Instantiate the interface and set the context */
            WebAppInterface(Context c) {
                mContext = c;
            }
        
            @JavascriptInterface
            public void sendEngagementEvent(String name, String extras ){
                EngagementAgent.getInstance(mContext).sendEvent(name, ParseExtras(extras));
            }
        
            @JavascriptInterface
            public void startEngagementJob(String name, String extras ){
                EngagementAgent.getInstance(mContext).startJob(name, ParseExtras(extras));
            }
        
            @JavascriptInterface
            public void endEngagementJob(String name){
                EngagementAgent.getInstance(mContext).endJob(name);
            }
        
            @JavascriptInterface
            public void sendEngagementError(String name, String extras ){
                EngagementAgent.getInstance(mContext).sendError(name, ParseExtras(extras));
            }
        
            @JavascriptInterface
            public void sendEngagementAppInfo(String appInfo){
                EngagementAgent.getInstance(mContext).sendAppInfo(ParseExtras(appInfo));
            }
        
            public Bundle ParseExtras(String input) {
                Bundle extras = new Bundle();
        
                try {
                    JSONObject jObject = new JSONObject(input);
                    extras.putString(jObject.names().getString(0),
                            jObject.get(jObject.names().getString(0)).toString());
                } catch (Exception e) {
                    e.printStackTrace();
                }
                return extras;
            }
        }  

4. Kada stvorili smo iznad most datoteku, moramo da biste bili sigurni da je povezan s oglednim prikaza. Da se to dogodi, najprije morate uređivanje vaše `SetWebview` način tako da izgleda ovako:

        private void SetWebView() {
            WebView myWebView = (WebView) findViewById(R.id.webview);
            myWebView.loadUrl("file:///android_asset/Sample.html");
            WebSettings webSettings = myWebView.getSettings();
            webSettings.setJavaScriptEnabled(true);
            myWebView.addJavascriptInterface(new WebAppInterface(this), "EngagementJs");
        }

5. U gornjem isječak smo naziva `addJavascriptInterface` želite pridružiti naš predmete most naš prikaza i i stvorili bolji pod nazivom **EngagementJs** da biste uputili poziv metode iz datoteke most. 

6. Sada stvorite sljedeću datoteku pod nazivom **Sample.html** u projektu u mapu s nazivom **sredstva** koja se učitava u na prikaz za web i odakle smo poziv metode iz datoteke most.

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
                      log('window.onerror: ' + err);
                    }
        
                    send = function(inputId)
                    {
                        var input = document.getElementById(inputId);
                        if(input)
                        {
                            var value = input.value;
                            // Example of how extras info can be passed with the Engagement logs
                            var extras = '{"CustomerId":"MS290011"}';
        
                            if(value && value.length > 0)
                            {
                                switch(inputId)
                                {
                                    case "event":
                                    EngagementJs.sendEngagementEvent(value, extras);
                                    break;
        
                                    case "job":
                                    EngagementJs.startEngagementJob(value, extras);
                                    window.setTimeout( function()
                                    {
                                      EngagementJs.endEngagementJob(value);
                                    }, 10000 );
                                    break;
        
                                    case "error":
                                    EngagementJs.sendEngagementError(value, extras);
                                    break;
        
                                    case "appInfo":
                                    EngagementJs.sendEngagementAppInfo({"customer_name":value});
                                    break;
                                }
                            }
                        }
                    }
                </script>
            </head>
            <body>
                <h1>Bridge Tester</h1>
                <div id='engagement'>
                    <h2>Event</h2>
                    <input type="text" id="event" size="35">
                    <button onclick="send('event')">Send</button>
        
                    <h2>Job</h2>
                    <input type="text" id="job" size="35">
                    <button onclick="send('job')">Send</button>
        
                    <h2>Error</h2>
                    <input type="text" id="error" size="35">
                    <button onclick="send('error')">Send</button>
        
                    <h2>AppInfo</h2>
                    <input type="text" id="appInfo" size="35">
                    <button onclick="send('appInfo')">Send</button>
                </div>
            </body>
        </html>

8. Imajte na umu sljedeće točke o iznad HTML datoteku:

    -   Sadrži skup unos okviri u koje možete unijeti podatke koji će se koristiti kao nazive na događaj, posla, pogreška, AppInfo. Prilikom klika na gumb pokraj tog prikaza, Javascript koja naposljetku poziva metode iz datoteke most prenesite poziv i radnje Mobile za Android SDK upućuje se poziv. 
    -   Ne možemo označavanje na neki statične dodatne informacije da biste događaja, zadatke i čak pogreške da bismo pokazali kako to možete učiniti. Dodatne informacije šalju kao što je JSON niz koji, ako vam se prikazivati na `WebAppInterface` datoteke, je raščlaniti i vratite na uređaju sa sustavom Android `Bundle` i prenosi uz slanje događaje, zadatke, pogreške. 
    -   Radnje posao Mobile je kicked pod nazivom koji ste naveli u okviru za unos pokretanje za 10 sekundi i isključiti. 
    -   Radnje Mobile appinfo ili oznakom se prenosi s customer_name kao statične ključ i vrijednost koju ste unijeli u unos kao vrijednost za oznaku. 
 
9. Pokrenite aplikaciju i vidjet ćete sljedeće. Sada upišite neki ime za testiranje događaja kao što je sljedeća i kliknite **Pošalji** ispod njega. 

    ![][1]

10. Sada ako odete na kartici **Monitor** aplikacije i pogledajte informacije u odjeljku **događaji -> detalje**, prikazat će se događaj prikazivati uz statične aplikacije-informacije koje ćemo šaljete. 

    ![][2]

<!-- Images. -->
[1]: ./media/mobile-engagement-bridge-webview-native-android/sending-event.png
[2]: ./media/mobile-engagement-bridge-webview-native-android/event-output.png
