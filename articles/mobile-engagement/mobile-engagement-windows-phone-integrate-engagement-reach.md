<properties 
    pageTitle="Integracija SDK Silverlight razgovor u sustavu Windows Phone" 
    description="Kako integrirati Azure mobilne radnje razgovor s aplikacijama Silverlight za Windows Phone"                    
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="na" 
    ms.topic="article"
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="windows-phone-silverlight-reach-sdk-integration"></a>Integracija SDK Silverlight razgovor u sustavu Windows Phone

Morate pratiti u Integracija postupak opisan u odjeljku [Windows Phone Silverlight radnje SDK Integracija](mobile-engagement-windows-phone-integrate-engagement.md) prije nego nastavite slijediti ovaj vodič.

##<a name="embed-the-engagement-reach-sdk-into-your-windows-phone-silverlight-project"></a>Ugrađivanje SDK za dosegne radnje u projektu Silverlight za Windows Phone

Ne morate ništa da biste dodali. `EngagementReach`reference i resursima se već nalaze u projektu.

> [AZURE.TIP]  Možete prilagoditi slike koja se nalazi u na `Resources` mapu projekta, osobito ikona robne marke (tu zadanu vrijednost na ikonu radnje).

##<a name="add-the-capabilities"></a>Dodavanje mogućnosti

Radnje dosegne SDK mora neke dodatne mogućnosti.

Otvaranje vaše `WMAppManifest.xml` datoteku i biti sigurni da su deklarirane sljedeće mogućnosti:

-   `ID_CAP_PUSH_NOTIFICATION`
-   `ID_CAP_WEBBROWSERCOMPONENT`

Servis MPNS koriste prvoga da biste omogućili prikaz skočnoj obavijesti. Drugu koristi se za ugrađivanje zadatka preglednika SDK-a.

Uređivanje u `WMAppManifest.xml` datoteku i dodavanje unutar na `<Capabilities />` oznaka:

    <Capability Name="ID_CAP_PUSH_NOTIFICATION" />
    <Capability Name="ID_CAP_WEBBROWSERCOMPONENT" />

##<a name="enable-the-microsoft-push-notification-service"></a>Omogućivanje servisa Microsoft automatske obavijesti

Da biste koristili **Microsoft automatske obavijesti Service** (nazivaju MPNS) na `WMAppManifest.xml` datoteka mora imati programa `<App />` oznaka s na `Publisher` atribut postavite naziv projekta.

##<a name="initialize-the-engagement-reach-sdk"></a>Pokretanje razgovor radnje SDK

### <a name="engagement-configuration"></a>Konfiguriranje radnje

Konfiguracija radnje je centralizirano u na `Resources\EngagementConfiguration.xml` datoteke u projekt.

Uređivanje ove datoteke da biste odredili konfiguracije razgovor:

-   *Neobavezno*označava hoće li se aktivira nativni automatske (MPNS) ili nije između `<enableNativePush>` i `</enableNativePush>` oznake (`true` po zadanom).
-   *Neobavezno*označava naziv kanala automatske između `<channelName>` i `</channelName>` , pružaju isti koje aplikacije možda trenutno koristite ili ostavite prazan.

Ako želite da biste odredili je prilikom izvođenja, možete pozvati sljedeću metodu prije agent za inicijalizaciju radnje:

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
    
    engagementConfiguration.Reach.EnableNativePush = true;                  
    /* [Optional] whether the native push (MPNS) is activated or not. */
    
    engagementConfiguration.Reach.ChannelName = "YOUR_PUSH_CHANNEL_NAME";   
    /* [Optional] Provide the same channel name that your application may currently use. */
    
    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(engagementConfiguration);

> [AZURE.TIP] Možete navesti naziv kanala automatske MPNS aplikacije. Prema zadanim postavkama radnje stvara naziv koji se temelji na na ID programa. Imate ne morate navesti naziv sami, osim ako namjeravate koristiti kanala automatske izvan radnje.

### <a name="engagement-initialization"></a>Pokretanje radnje

Izmjena na `App.xaml.cs`:

-   Dodajte svoje `using` izvješća:

        using Microsoft.Azure.Engagement;

-   Umetanje `EngagementReach.Instance.Init` tek nakon što instalirate `EngagementAgent.Instance.Init` u `Application_Launching` :

        private void Application_Launching(object sender, LaunchingEventArgs e)
        {
           EngagementAgent.Instance.Init();
           EngagementReach.Instance.Init();
        }

-   Umetanje `EngagementReach.Instance.OnActivated` u na `Application_Activated` metoda:

        private void Application_Activated(object sender, ActivatedEventArgs e)
        {
           EngagementAgent.Instance.OnActivated(e);
           EngagementReach.Instance.OnActivated(e);
        }

> [AZURE.IMPORTANT] Na `EngagementReach.Instance.Init` pokreće se u namjenski niti. Ne morate učiniti sami.

##<a name="app-store-submission-considerations"></a>Slanje pitanja vezana uz iz trgovine App

Microsoft nameće neka pravila prilikom korištenja automatske obavijesti:

Iz dokumentaciju Microsoft [Aplikacije pravila] sekcije 2.9:

1) Morate uputite korisnika da biste prihvatili prima automatske obavijesti. Zatim u vašim postavkama, dodajte način za onemogućivanje automatske obavijesti.

Objekt EngagementReach omogućuje upravljanje u izborno-u i isključivanja, na dva načina `EnableNativePush()` i `DisableNativePush()`. Mogućnost može, stvorite, na primjer, u odjeljku postavke s prekidač onemogućivanje i omogućivanje MPNS.

Možete odlučiti da biste deaktivirali MPNS kroz konfiguraciju radnje\<windows phone – sdk-razgovor-konfiguraciju\>.

> 2.9.1) aplikacije morate najprije opisani u obavijesti koja se pruža i **dobiti izričite dozvole korisnika (Prijava)**te **Navedite mehanizam korisnika putem kojega možete odabrati iz primanje automatske obavijesti**. Sve obavijesti naveli putem servisa Microsoft automatske obavijesti moraju biti usklađene s opisa korisniku i se morate pridržavati svih primjenjivih [Pravila za programe]  [ Content Policies] i [Dodatni preduvjeti za određene vrste aplikacija].

2) Nemojte koristiti previše automatske obavijesti. Radnje će obrađivati obavijesti za vas.

> 2.9.2) aplikacije i njezino korištenje servisa Microsoft automatske obavijesti mora prenapadno ne koristite mrežnom kapacitetu ili propusnost servisa Microsoft automatske obavijesti, ili u suprotnom unduly burden Windows Phone ili drugi uređaj Microsoft ili servis pomoću viškom automatske obavijesti, kao što je određen Microsoft u njegov pametnije diskretni i morate ne ugroziti ili ometati bilo kojeg Microsoftova mreža ili poslužitelje sve poslužitelje trećih strana ili mreža povezani sa servisom Microsoft automatske obavijesti.

3) Je za MPNS da biste poslali criticals podatke. Radnje koristi MPNS, tako da se pravilo primjenjuje i za kampanje stvoriti unutar na radnje sučelja.

> 2.9.3) u automatske obavijesti servis Microsoft može se koristiti za slanje obavijesti koji su zaštita njihove privatnosti ovise ključnih niti ne izričitu može utjecati na prikaz stvarima vijek ili death, uključujući bez ograničenja ključnih obavijesti vezanih uz mjehuričaste uređaja ili uvjeta. MICROSOFT ODRIČE TO IZRIČITO NEKI JAMSTVA DA KORISTITE MICROSOFT AUTOMATSKE OBAVIJESTI SERVISA ILI ISPORUKU MICROSOFT AUTOMATSKE OBAVIJESTI SERVISA OBAVIJESTI ĆE SE OSIGURAO, POGREŠKA SLOBODNI, NITI NE IZRIČITU ZAJAMČENO POJAVITI NA TEMELJU U STVARNOM VREMENU.

**Ne možemo ne jamči da aplikacije proći kroz postupak provjere valjanosti ako ne poštuje te preporuke.**

##<a name="handle-data-push-optional"></a>Slanje podataka ručicu (nije obavezno)

Ako želite da se aplikacije da biste mogli primati ih gura razgovor podataka, imate implementirati dva događaja klase EngagementReach:

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

##<a name="customize-ui-optional"></a>Prilagodba korisničkog Sučelja (nije obavezno)

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

Postavite sadržaj u `EngagementReach.Instance.Handler` polja s prilagođenog objekta u svoje `App.xaml.cs` klase unutar na `Application_Launching` način.

**Ogledni kod:**

    private void Application_Launching(object sender, LaunchingEventArgs e)
    {
       // your app initialization 
       EngagementReach.Instance.Handler = new ExampleReachHandler();
       // Engagement Agent and Reach initialization
    }

> [AZURE.NOTE] Prema zadanim postavkama radnje koristi vlastitu implementaciju sustava `EngagementReachHandler`. Ne morate stvoriti vlastite pa ako to učinite, ne morate odbaciti svaki način. Zadano ponašanje je da biste odabrali objekt osnove radnje.

### <a name="layouts"></a>Izgledi

Prema zadanim postavkama, razgovor koristit će ugrađeni resursi DLL-a za prikaz obavijesti i stranica.

Međutim, možete odlučiti koristiti vlastite resurse tako da odražava vaša marke u te komponente.

Možete nadjačati `EngagementReachHandler` načine u svoje podklase reći radnje za korištenje izgleda:

**Ogledni kod:**

    // In your subclass of EngagementReachHandler
    
    public override string GetTextViewAnnouncementUri()
    {
       // return the path of your own xaml
    }
    
    public override string GetWebViewAnnouncementUri()
    {
       // return the path of your own xaml
    }
    
    public override string GetPollUri()
    {
       // return the path of your own xaml
    }
    
    public override EngagementNotificationView CreateNotification(EngagementNotificationViewModel viewModel)
    {
       // return a new instance of your own notification
    }

> [AZURE.TIP] Na `CreateNotification` način možete vratiti null. Obavijest neće se prikazati, a zatim će biti odbačeni kampanje razgovor.

Da biste pojednostavnili implementaciju izgleda i dajemo našim xaml koji vam može poslužiti kao temelj za svoj kod. Se nalaze u arhivu radnje SDK (/ src/razgovor /).

> [AZURE.WARNING] Izvori koje nudimo vam se točno istom oni koji koristimo. Da ako želite izravno ih mijenjati, ne zaboravite da biste promijenili naziva i naziva.

### <a name="notification-position"></a>Položaj obavijesti

Prema zadanim postavkama, prikazuje se obavijest u aplikaciji pri dnu na lijevoj strani aplikacije. Takvo ponašanje možete promijeniti tako da nadjačavanje na `GetNotificationPosition` način u `EngagementReachHandler` objekt.

    // In your subclass of EngagementReachHandler
    
    public override EngagementReachHandler.NotificationPosition GetNotificationPosition(EngagementNotificationViewModel viewModel)
    {
       // return a value of the EngagementReachHandler.NotificationPosition enum (TOP or BOTTOM)
    }

Trenutno na raspolaganju u `BOTTOM` (zadano) i `TOP` položaja.

### <a name="launch-message"></a>Pokretanje poruke

Kad korisnik klikne na obavijest sustava (skočnoj), radnje pokreće aplikaciju, Učitaj sadržaj poruke automatske i prikaz stranice odgovarajuće kampanje.

Postoji razmak između pokretanje aplikacije i prikaz stranice (ovisno o brzini mreže).

Da biste naznačili korisniku je li nešto učitavanje, mora sadržavati vizualne podatke, kao što su traku napretka ili pokazatelj tijeka. Radnje ne prepoznaje da sam, ali nudi nekoliko rukovatelja umjesto vas.

Možete implementirati povratnog učiniti:

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

Povratnog možete postaviti u svoje `Application_Launching` način vaše `App.xaml.cs` datoteke, prije kapaciteta u `EngagementReach.Instance.Init()` poziva.

> [AZURE.TIP] Svaki rukovatelj pozove niti korisničkog Sučelja. Ne morate brinuti prilikom korištenja MessageBox ili nešto korisničkog Sučelja vezane uz.

[Pravila za programe]:http://msdn.microsoft.com/library/windows/apps/hh184841(v=vs.105).aspx
[Content Policies]:http://msdn.microsoft.com/library/windows/apps/hh184842(v=vs.105).aspx
[Dodatni preduvjeti za određenu aplikaciju vrste]:http://msdn.microsoft.com/library/windows/apps/hh184838(v=vs.105).aspx
 
