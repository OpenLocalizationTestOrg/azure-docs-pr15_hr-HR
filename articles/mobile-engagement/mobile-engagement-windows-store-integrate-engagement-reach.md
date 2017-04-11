<properties 
    pageTitle="Integracija SDK univerzalni razgovor aplikacije za Windows" 
    description="Kako integrirati Azure mobilne radnje razgovor s univerzalni aplikacijama za Windows"
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-store" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

# <a name="windows-universal-apps-reach-sdk-integration"></a>Integracija SDK univerzalni razgovor aplikacije za Windows

Morate pratiti u Integracija postupak opisan u odjeljku [Windows univerzalni radnje SDK Integracija](mobile-engagement-windows-store-integrate-engagement.md) prije nego nastavite slijediti ovaj vodič.

## <a name="embed-the-engagement-reach-sdk-into-your-windows-universal-project"></a>Ugrađivanje radnje dosegne SDK Windows univerzalni projekta

Ne morate ništa da biste dodali. `EngagementReach`reference i resursima se već nalaze u projektu.

> [AZURE.TIP] Možete prilagoditi slike koja se nalazi u na `Resources` mapu projekta, osobito ikona robne marke (tu zadanu vrijednost na ikonu radnje). Na univerzalni aplikacija možete premjestiti u `Resources` mapu na zajedničkom projekta za zajedničko korištenje sadržaja između aplikacija, ali morate zadržati u `Resources\EngagementConfiguration.xml` datotekama na zadano mjesto, kao što je platformu zavisne.

## <a name="enable-the-windows-notification-service"></a>Omogućivanje servisa obavijesti za Windows

### <a name="windows-8x-and-windows-phone-81-only"></a>Windows 8.x i Windows Phone 8.1 samo

Da biste koristili na **Servis za obavijesti sustava Windows** (nazivaju WNS) u vašem `Package.appxmanifest` datotekama na `Application UI` kliknite `All Image Assets` u okvir lijevo robot. U desnom okviru `Notifications`, promijenite `toast capable` iz `(not set)` da biste `(Yes)`.

### <a name="all-platforms"></a>Sve platforme

Morate sinkronizirati aplikacije na Microsoftov račun i platformi radnje. Za to ćete morati stvoriti račun ili kada se prijavite na [Razvojni centar za windows](https://dev.windows.com). Nakon toga stvorite novu aplikaciju i pronađite SID i tajnu ključ. Sučelju radnje idite na postavke aplikacije u `native push` i zalijepite vjerodajnice. Nakon toga, desnom tipkom miša kliknite na projektu, odaberite `store` i `Associate App with the Store...`. Samo želite odaberite aplikaciju stvorite prije nego što je sinkronizacija.

## <a name="initialize-the-engagement-reach-sdk"></a>Pokretanje razgovor radnje SDK

Izmjena na `App.xaml.cs`:

-   Umetanje `EngagementReach.Instance.Init` tek nakon što instalirate `EngagementAgent.Instance.Init` u vašem `InitEngagement` metoda:

        private void InitEngagement(IActivatedEventArgs e)
        {
          EngagementAgent.Instance.Init(e);
          EngagementReach.Instance.Init(e);
        }

    Na `EngagementReach.Instance.Init` pokreće se u namjenski niti. Ne morate učiniti sami.

> [AZURE.NOTE] Ako koristite automatske obavijesti negdje drugdje u aplikaciji imate [automatske kanal za zajedničko korištenje](#push-channel-sharing) s dosegne radnje.

## <a name="integration"></a>Integracija

Radnje nudi dva načina za dodavanje reklamni Natpisi u aplikaciji za razgovor i interstitial prikazi za najave i ankete u aplikaciji: Integracija preklapanja i ručno Integracija prikazi web. Ne treba kombinirati obje pristup na istoj stranici.

Odabir između dva Integracija nije sažeta ovako:

-   Možete odabrati integraciju sa servisom prekriveno ako stranica već nasljeđuje agenta `EngagementPage`, je samo pitanje zamjene `EngagementPage` po `EngagementPageOverlay` i `xmlns:engagement="using:Microsoft.Azure.Engagement"` po `xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"` na stranicama.
-   Prikazi web možete odabrati ručno Integracija upute za precizno postavite dosegne korisničkog Sučelja unutar stranice ili ako ne želite da biste dodali još jedan nasljeđivanje na stranice. 

### <a name="overlay-integration"></a>Integracija preklapanja

Preklapanje radnje dinamički dodaje elemente korisničkog Sučelja služi za prikaz kampanje razgovor na stranici. Ako preklapanja ne odgovaraju izgled razmotrite prikazi web ručno Integracija umjesto toga.

U promjenu datoteka .xaml `EngagementPage` referencu na`EngagementPageOverlay`

-   Dodajte svoje deklaracija prostora naziva:

        xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"

-   Zamjena `engagement:EngagementPage` s `engagement:EngagementPageOverlay`:

**S EngagementPage:**

        <engagement:EngagementPage 
            xmlns:engagement="using:Microsoft.Azure.Engagement">
        
            <!-- Your layout -->
        </engagement:EngagementPage>

**S EngagementPageOverlay:**

        <engagement:EngagementPageOverlay 
            xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay">
        
            <!-- Your layout -->
        </engagement:EngagementPageOverlay>

Zatim u datoteci .cs oznaka na stranicu u `EngagementPageOverlay` umjesto `EngagementPage` i uvoz `Microsoft.Azure.Engagement.Overlay`.

            using Microsoft.Azure.Engagement.Overlay;

-   Zamjena `EngagementPage` s `EngagementPageOverlay`:

**S EngagementPage:**

            using Microsoft.Azure.Engagement;
            
            namespace Example
            {
              public sealed partial class ExamplePage : EngagementPage
              {
                [...]
              }
            }

**S EngagementPageOverlay:**

            using Microsoft.Azure.Engagement.Overlay;
            
            namespace Example
            {
              public sealed partial class ExamplePage : EngagementPageOverlay 
              {
                [...]
              }
            }


Dodaje prekrivanja radnje u `Grid` element pri vrhu stranice sastoji od izgled i dva `WebView` elementa jedan za na natpisu i drugu interstitial prikaza.

Možete prilagoditi elementi prekriveno izravno u programu u `EngagementPageOverlay.cs` datoteku.

### <a name="web-views-manual-integration"></a>Ručno Integracija prikaza za web

Razgovor će pretraživanje stranica za dva `WebView` elemenata odgovoran za prikaz na natpisu i interstitial prikaz. Samo što morate učiniti tako da dodate ta dva `WebView` elemenata negdje na stranicama, Evo jednog primjera:

    <Grid x:Name="engagementGrid">

      <!-- Your layout -->

      <WebView x:Name="engagement_notification_content" Visibility="Collapsed" Height="80" HorizontalAlignment="Stretch" VerticalAlignment="Top"/>
      <WebView x:Name="engagement_announcement_content" Visibility="Collapsed"  HorizontalAlignment="Stretch"  VerticalAlignment="Stretch"/>
    </Grid>


U ovom primjeru u `WebView` su elementi Rastegnuto Prilagodi njihove spremniku koji se automatski ponovno postavlja veličinu ih u slučaju zaslona zakretanje ili u prozoru Promjena veličine.

> [AZURE.WARNING] Važno je da bi isti imenovanje `engagement_notification_content` i `engagement_announcement_content` za na `WebView` elemente. Razgovor je prepoznavanje ih tako da njegovo ime. 

## <a name="handle-datapush-optional"></a>Držač datapush (neobavezno)

Ako želite da se aplikacije da biste mogli primati ih gura razgovor podataka, imate implementirati dva događaja klase EngagementReach:

U App.xaml.cs u Graditelj App() dodati:

            EngagementReach.Instance.DataPushStringReceived += (body) =>
            {
              Debug.WriteLine("String data push message received: " + body);
              return true;
            };
            
            EngagementReach.Instance.DataPushBase64Received += (decodedBody, encodedBody) =>
            {
              Debug.WriteLine("Base64 data push message received: " + encodedBody);
              // Do something useful with decodedBody like updating an image view
              return true;
            };

Vidjet ćete da povratnog svaki način vraća na Booleove vrijednosti. Radnje šalje povratnih informacija do kraja natrag nakon i isporuka automatske podataka. Ako je povratnog vraća false, u `exit` povratne informacije bit će Pošalji. U suprotnom će biti `action`. Ako je povratni postavljen za događaje, u `drop` povratne informacije vratit će se radnje.

> [AZURE.WARNING] Radnje nije primati feedbacks višestrukih grafikona za podatke pribadače. Ako namjeravate postaviti nekoliko rukovatelja neki događaj, imajte na umu da će na posljednju odgovarati povratnih informacija nešto šalje. U ovom slučaju, preporučujemo da biste uvijek vraća iste vrijednosti da biste izbjegli pregledniji povratnih informacija na sučeljem.

## <a name="customize-ui-optional"></a>Prilagodba korisničkog Sučelja (nije obavezno)

### <a name="first-step"></a>Prvi korak

Ne možemo omogućuju vam prilagodbu razgovor korisničkog Sučelja.

Da biste to učinili, morate stvoriti podklase od na `EngagementReachHandler` predmete.

**Ogledni kod:**

            using Microsoft.Azure.Engagement;
            
            namespace Example
            {
              internal class ExampleReachHandler : EngagementReachHandler
              {
               // Override EngagementReachHandler methods depending on your needs
              }
            }

Postavite sadržaj u `EngagementReach.Instance.Handler` polja s prilagođenog objekta u svoje `App.xaml.cs` klase unutar na `App()` način.

**Ogledni kod:**

            protected override void OnLaunched(LaunchActivatedEventArgs args)
            {
              // your app initialization 
              EngagementReach.Instance.Handler = new ExampleReachHandler();
              // Engagement Agent and Reach initialization
            }

> [AZURE.NOTE]Prema zadanim postavkama radnje koristi vlastitu implementaciju sustava `EngagementReachHandler`.
> Ne morate stvoriti vlastite pa ako to učinite, ne morate odbaciti svaki način. Zadano ponašanje je da biste odabrali objekt osnove radnje.

### <a name="web-view"></a>Web-tablice

Prema zadanim postavkama, razgovor koristit će ugrađeni resursi DLL-a za prikaz obavijesti i stranica.

Omogućuje potpunu prilagodbe mogućnosti samo koristimo web-tablice. Ako želite da biste prilagodili izglede, nadjačati izravno datoteka resursa `EngagementAnnouncement.html` i `EngagementNotification.html`. Radnje mora sav kod u `<body></body>` da biste pokrenuli pravilno. No možete dodati oznaku vanjski `engagement_webview_area`.

Međutim, možete odlučiti koristiti vlastitu resursi.

Možete nadjačati `EngagementReachHandler` načine u svoje podklase reći radnje koristite izgleda, ali pobrinuti se da biste ugrađene mehanizam radnje:

**Ogledni kod:**
            
            // In your subclass of EngagementReachHandler
            
            public override string GetAnnouncementHTML()
            {
              return base.GetAnnouncementHTML();
            }
            public override string GetAnnouncementName()
            {
              return base.GetAnnouncementName();
            }
            public override string GetNotfificationHTML()
            {
              return base.GetNotfificationHTML();
            }
            public override string GetNotfificationName()
            {
              return base.GetNotfificationName();
            }


Po zadanom je AnnouncementHTML `ms-appx-web:///Resources/EngagementAnnouncement.html`. Predstavlja html datoteka koja dizajniranje sadržaj automatske poruke (objava tekst, Web anoucement i objava ankete). AnnouncementName je `engagement_announcement_content`. To je naziv prikaza dizajna xaml stranica.

NotfificationHTML je `ms-appx-web:///Resources/EngagementNotification.html`. Predstavlja html datoteka koja dizajniranje obavijesti automatskih poruka. NotfificationName je `engagement_notification_content`. To je naziv prikaza dizajna xaml stranica.

### <a name="customization"></a>Prilagodba

Možete prilagoditi obavijesti i objava web-prikaz ima ako sačuvati radnje objekta. Oprez poduzeti te prikaza objekta opisan je tri puta – prvi put u vašem xaml, drugi put u datoteci .cs u metodu "setwebview()" i trećeg vremena u html datoteka.

-   U vašem xaml opisuju trenutne komponente prikaza grafički izgled.
-   U datoteci .cs možete definirati "setwebview()" Postavljanje dimenzije dva prikaza (obavijesti, objava). Vrlo učinkovitih je kada je veličine aplikacije.
-   U html datoteku radnje smo opisuju prikaza sadržaja, dizajn i položaja elemenata između međusobno povezani.

### <a name="launch-message"></a>Pokretanje poruke

Kad korisnik klikne na obavijest sustava (skočnoj), radnje pokreće aplikaciju, Učitaj sadržaj poruke automatske i prikaz stranice odgovarajuće kampanje.

Postoji razmak između pokretanje aplikacije i prikaz stranice (ovisno o brzini mreže).

Da biste naznačili korisniku je li nešto učitavanje, mora sadržavati vizualne podatke, kao što su traku napretka ili pokazatelj tijeka. Radnje ne prepoznaje da sam, ali nudi nekoliko rukovatelja umjesto vas.

Možete implementirati povratnog u App.xaml.cs u "Javno App() {}" dodati:

            /* The application has launched and the content is loading.
             * You should display an indicator here.
             */
            EngagementReach.Instance.RetrieveLaunchMessageStarted += () => { [...] };
            
            /* The application has finished loading the content and the page
             * is about to be displayed.
             * You should hide the indicator here.
             */
            EngagementReach.Instance.RetrieveLaunchMessageCompleted += () => { [...] };
            
            /* The content has been loaded, but an error has occurred.
             * You can provide an information to the user.
             * You should hide the indicator here.
             */
            EngagementReach.Instance.RetrieveLaunchMessageFailed += () => { [...] };

Povratnog možete postaviti u način "Javno App() {}" od vašeg `App.xaml.cs` datoteke, prije kapaciteta u `EngagementReach.Instance.Init()` poziva.

> [AZURE.TIP] Svaki rukovatelj pozove niti korisničkog Sučelja. Ne morate brinuti prilikom korištenja MessageBox ili nešto korisničkog Sučelja vezanih uz.

##<a id="push-channel-sharing"></a>Automatske zajedničko korištenje kanala

Ako koristite automatske obavijesti druge svrhe u aplikaciji imate koristite automatske kanala zajedničko korištenje značajke SDK radnje. To je da biste izbjegli propuštenih pribadače.

- Možete navesti vlastite automatske kanala Inicijalizacija dosegne radnje. SDK koristit će se umjesto traži novi.

Ažuriranje za inicijalizaciju radnje dosegne automatske kanala u na `InitEngagement` način iz na `App.xaml.cs` datoteke:
    
    /* Your own push channel logic... */
    var pushChannel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
    
    /*...Engagement initialization */
    EngagementAgent.Instance.Init(e);
    EngagementReach.Instance.Init(e,pushChannel);

- Osim toga, ako samo želite trošiti kanala automatske nakon Inicijalizacija razgovor, a možete postaviti na povratni radnje dosegne da biste dobili kanala automatske jednom stvorena je pomoću SDK-a.

Postavite svoje povratni na bilo koje mjesto **nakon** Inicijalizacija razgovor:

    /* Set action on the SDK push channel. */
    EngagementReach.Instance.SetActionOnPushChannel((PushNotificationChannel channel) => 
    {
      /* The forwarded channel can be null if its creation fails for any reason. */
      if (channel != null)
      {
        /* Your own push channel logic... */
      });
    }

## <a name="custom-scheme-tip"></a>Savjet prilagođene sheme

Dajemo koristi prilagođenu shemu. Različite vrste URI možete slati iz radnje sučelju koja će se koristiti u aplikaciji radnje. Zadana shema kao što su `http, ftp, ...` su možete upravljati tako da Windows, prozor će upit ako su bez zadane aplikacije instalirana na uređaj. Možete stvoriti i vlastite prilagođene sheme za svoju aplikaciju.

Jednostavan način da biste postavili prilagođenu shemu u aplikaciji je da biste otvorili vaše `Package.appxmanifest` otvorite u `Declarations` ploča. Odaberite `Protocol` u dostupna deklaracija pomaknite se okvir i dodajte ga. Uređivanje u `Name` polje protokol za nove želji naziv.

Sada da biste koristili protokol koji je potrebno, uredite vaše `App.xaml.cs` s na `OnActivated` način i nemojte zaboraviti i Inicijalizacija radnje ovdje:

            /// <summary>
            /// Enter point when app his called by another way than user click
            /// </summary>
            /// <param name="args">Activation args</param>
            protected override void OnActivated(IActivatedEventArgs args)
            {
              /* Init engagement like it was launch by a custom uri scheme */
              EngagementAgent.Instance.Init(args);
              EngagementReach.Instance.Init(args);
            
              //TODO design action to do when app is launch
            
              #region Custom scheme use
              if (args.Kind == ActivationKind.Protocol)
              {
                ProtocolActivatedEventArgs myProtocol = (ProtocolActivatedEventArgs)args;
            
                if (myProtocol.Uri.Scheme.Equals("protocolName"))
                {
                  string path = myProtocol.Uri.AbsolutePath;
                }
              }
              #endregion
 
