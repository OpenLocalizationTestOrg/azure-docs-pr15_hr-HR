<properties 
    pageTitle="Windows Phone Silverlight radnje SDK Integracija" 
    description="Kako integrirati Azure radnje mobilne aplikacije Silverlight za Windows Phone"                  
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="windows-phone-silverlight-engagement-sdk-integration"></a>Windows Phone Silverlight radnje SDK Integracija

> [AZURE.SELECTOR] 
- [Univerzalni za Windows](mobile-engagement-windows-store-integrate-engagement.md) 
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md) 
- [iOS](mobile-engagement-ios-integrate-engagement.md) 
- [Android](mobile-engagement-android-integrate-engagement.md) 

U ovom postupku opisano najjednostavniji način da biste aktivirali analize Azure Mobile radnje te praćenje funkcije u aplikaciji Windows Phone Silverlight.

Sljedeći koraci nisu dovoljno da biste aktivirali izvješća zapisnika potrebne za izračun svih statističkih podataka vezanih uz korisnika, sesije, aktivnosti, ruši i Technicals. Izvješće zapisnika potrebne za izračunavanje ostalih statističkih podataka kao što su događaji, pogrešaka i zadacima se mora poduzeti ručno pomoću radnje API-JA (pogledajte [upute za korištenje dodatne radnje Mobile API-JA u aplikaciji Windows Phone Silverlight za označavanje](mobile-engagement-windows-phone-use-engagement-api.md) ispod) jer te statistike ovisne o aplikaciji.

##<a name="supported-versions"></a>Podržana verzija

Mobilni radnje SDK za Windows platforme Silverlight može se integrirati u aplikacije skupine:

-   Windows Phone 8.0
-   Windows Phone 8.1 Silverlight

> [AZURE.NOTE] Ako birate Windows Phone 8.1 (koji nisu Silverlight), zatim se s [Windows Univerzalni integracija postupak](mobile-engagement-windows-store-integrate-engagement.md).

##<a name="install-the-mobile-engagement-silverlight-sdk"></a>Instalirajte Silverlight SDK mobilne radnje

Mobilni radnje SDK za Windows platforme Silverlight nije dostupan kao paket Nuget pod nazivom *MicrosoftAzure.MobileEngagement*. Možete ga instalirati iz programa Visual Studio Nuget paket upravitelja. 

##<a name="add-the-capabilities"></a>Dodavanje mogućnosti

Radnje SDK mora neke mogućnosti programa Silverlight SDK za Windows Phone da bi funkcionirao.

Otvaranje vaše `WMAppManifest.xml` datoteku i provjerite je li da se sljedeće mogućnosti prijavljenom u na `Capabilities` ploče:

-   `ID_CAP_NETWORKING`
-   `ID_CAP_IDENTITY_DEVICE`

##<a name="initialize-the-engagement-sdk"></a>Pokretanje radnje SDK

### <a name="engagement-configuration"></a>Konfiguriranje radnje

Konfiguracija radnje je centralizirano u na `Resources\EngagementConfiguration.xml` datoteke u projekt.

Uređivanje ove datoteke da biste odredili:

-   Aplikacija niz za povezivanje u između oznake `<connectionString>` i `<\connectionString>`.

Ako želite da biste odredili je prilikom izvođenja, možete pozvati sljedeću metodu prije agent za inicijalizaciju radnje:

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(engagementConfiguration);

Na portalu klasični Azure prikazat će se niz za povezivanje za svoju aplikaciju.

### <a name="engagement-initialization"></a>Pokretanje radnje

Prilikom stvaranja novog projekta u `App.xaml.cs` generira datoteka. Klase nasljeđuje od `Application` i sadrži mnogo važne načine. Će se koristiti i za inicijalizaciju SDK radnje.

Izmjena na `App.xaml.cs`:

-   Dodajte svoje `using` izvješća:

        using Microsoft.Azure.Engagement;

-   Umetanje `EngagementAgent.Instance.Init` u na `Application_Launching` metoda:

        private void Application_Launching(object sender, LaunchingEventArgs e)
        {
          EngagementAgent.Instance.Init();
        }

-   Umetanje `EngagementAgent.Instance.OnActivated` u na `Application_Activated` metoda:

        private void Application_Activated(object sender, ActivatedEventArgs e)
        {
           EngagementAgent.Instance.OnActivated(e);
        }

> [AZURE.WARNING] Ne možemo svakako radnju želite dodati radnje za inicijalizaciju na drugom mjestu aplikacije. Međutim, imajte na umu koji se `EngagementAgent.Instance.Init` način pokreće na namjenski niti, a ne na niti korisničkog Sučelja.

##<a name="basic-reporting"></a>Osnovni izvješćivanje o pogreškama

### <a name="recommended-method--overload-your-phoneapplicationpage-classes"></a>Preporučeni način: preopterećenja vaše `PhoneApplicationPage` klase

Da biste aktivirali izvješća sve zapisnike potrebnih radnje za izračunavanje korisnika, sesije, aktivnosti, ruši i tehničke Statistika, možete jednostavno napraviti sve svoje `PhoneApplicationPage` podređenu klase nasljeđuju od na `EngagementPage` klase.

Evo primjera kako to učiniti za stranicu aplikacije. To možete učiniti na isti način za sve stranice aplikacije.

#### <a name="c-source-file"></a>C# izvorišna datoteka

Izmjena stranici `.xaml.cs` datoteke:

-   Dodajte svoje `using` izvješća:

        using Microsoft.Azure.Engagement;

-   Zamjena `PhoneApplicationPage` s `EngagementPage` :

**Bez radnje:**

        namespace Example
        {
          public partial class ExamplePage : PhoneApplicationPage
          {
            [...]
          }
        }

**S mogućnostima:**

        using Microsoft.Azure.Engagement;
        
        namespace Example
        {
          public partial class ExamplePage : EngagementPage 
          {
            [...]
          }
        }

> [AZURE.WARNING] Ako vaša stranica nasljeđuje od na `OnNavigatedTo` način, pripazite da biste omogućili u `base.OnNavigatedTo(e)` poziva. U suprotnom aktivnosti ne se prijavljivati. Uistinu, u `EngagementPage` je pozivanje `StartActivity` unutar na `OnNavigatedTo` način.

#### <a name="xaml-file"></a>XAML datoteke

Izmjena stranici `.xaml` datoteke:

-   Dodajte svoje deklaracija prostora naziva:

        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"

-   Zamjena `phone:PhoneApplicationPage` s `engagement:EngagementPage` :

**Bez radnje:**

        <phone:PhoneApplicationPage>
            <!-- layout -->
        </phone:PhoneApplicationPage>

**S mogućnostima:**

        <engagement:EngagementPage 
            xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP">
        
            <!-- layout -->
        </engagement:EngagementPage >

#### <a name="override-the-default-behavior"></a>Zanemarivanje zadano ponašanje

Prema zadanim postavkama, naziv klase stranice prijavljuje kao naziv aktivnosti s bez extra. Ako je klasa koristi sufiks "Stranica", radnje je i ukloniti.

Ako želite promijeniti zadano ponašanje za naziv, jednostavno dodajte to kod:

        // in the .xaml.cs file
        protected override string GetEngagementPageName()
        {
           /* your code */
           return "new name";
        }

Ako želite da biste prijavili nekih dodatnih informacija s aktivnosti, možete to dodati kod:

        // in the .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
           /* your code */
           return extra;
        }

Načini nazivaju se unutar na `OnNavigatedTo` način stranice.

### <a name="alternate-method-call-startactivity-manually"></a>Alternativni metoda: poziva `StartActivity()` ručno

Ako ne možete ili ne želite preopterećenja vaše `PhoneApplicationPage` klase, umjesto toga možete pokrenuti aktivnosti tako da nazovete `EngagementAgent` izravno metode.

Preporučujemo da se zovete `StartActivity` unutar vaše `OnNavigatedTo` način vaše PhoneApplicationPage.

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
           base.OnNavigatedTo(e);
           EngagementAgent.Instance.StartActivity("MyPage");
        }

> [AZURE.IMPORTANT] Provjerite je li pravilno završili sesiju.
>
> SDK automatski poziva na `EndActivity` način prilikom zatvaranja. Stoga je **vrlo** preporučuje se da biste uputili poziv na `StartActivity` način kad god se aktivnosti korisnika promijeniti i na **NIKAD** poziva na `EndActivity` način. Ta metoda šalje poruku s poslužiteljem radnje trenutnog korisnika je otišao aplikacije, a to utječe na sve aplikacije zapisnika.

##<a name="advanced-reporting"></a>Dodatna izvješća

Po želji, trebali biste izvješće aplikacije određene događaje, pogrešaka i zadacima, da biste to učinili, koristite na druge načine pronađeni u na `EngagementAgent` predmete. API radnje omogućuje korištenje svih radnje, dodatne mogućnosti.

Dodatne informacije potražite u članku [kako koristiti dodatne radnje Mobile API-JA u aplikaciji Windows Phone Silverlight za označavanje](mobile-engagement-windows-phone-use-engagement-api.md).

##<a name="advanced-configuration"></a>Napredna konfiguracija

### <a name="disable-automatic-crash-reporting"></a>Onemogućivanje automatskog rušenje izvješća

Možete onemogućiti automatsko rušenje izvješćivanja značajke radnje. Zatim, kada neobrađenu iznimku će se pojaviti, radnje ne ništa.

> [AZURE.WARNING] Ako planirate onemogućiti tu značajku, imajte na umu da kada neupravljani neočekivanog će se izvršiti u aplikaciji, radnje neće slati rušenje **i** će zatvoriti sesiju i zadacima.

Da biste onemogućili automatsko rušenje izvješćivanja, samo prilagoditi konfiguraciju ovisno o način na koji je deklariran:

#### <a name="from-engagementconfigurationxml-file"></a>Iz `EngagementConfiguration.xml` datoteka

Postavite izvješće rušenje `false` između `<reportCrash>` i `</reportCrash>` oznake.

#### <a name="from-engagementconfiguration-object-at-run-time"></a>Iz `EngagementConfiguration` objekta u vrijeme izvođenja

Postavite rušenje izvješća na false pomoću objekta EngagementConfiguration.

        /* Engagement configuration. */

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration(); engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
        /\* Disable Engagement crash reporting. \*/ engagementConfiguration.Agent.ReportCrash = false;

### <a name="burst-mode"></a>Brzi niz načinu rada

Prema zadanim postavkama, izvješća o usluzi radnje zapisnike u stvarnom vremenu. Ako aplikacija vrlo često izvješća zapisnika, bolje je da biste međuspremnika zapisnike, a da biste prijavili njih odjedanput na u pravilnim vremenskim base (to se naziva "način neprekidnog rada").

Da biste to učinili, nazovite metoda:

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

Argument je vrijednost u **milisekundama**. U bilo kojem trenutku, ako želite da biste ponovno aktivirali u stvarnom vremenu zapisivanje, samo pozvati metodu bez sve parametra ili vrijednost 0.

Način neprekidnog rada malo povećati trajanja baterija, ali je utjecaj na monitoru radnje: sve sesije i zadacima trajanje će biti zaokruženi praga neprekidnog rada (dakle, sesije i zadacima kraće nego praga neprekidnog rada možda neće biti vidljiva). Preporučuje se da biste koristili praga neprekidnog rada više od 30000 (30s). Morate imati na umu spremljenih zapisnika ograničeni su na 300 stavki. Ako pošaljete je predugačak može izgubiti neka zapisnika.

> [AZURE.WARNING] Prag neprekidnog rada ne može konfigurirati na razdoblje koje su manje od jedne sekunde. Ako pokušate učinite, SDK vode Prati pogreške i će automatski postaviti na zadane vrijednosti, odnosno nula sekundi. To će pokrenuti SDK da biste prijavili zapisnike u stvarnom vremenu.
 
