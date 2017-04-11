<properties 
    pageTitle="Integracija s Windows SDK radnje univerzalni aplikacije" 
    description="Kako integrirati Azure mobilne radnje s univerzalni aplikacijama za Windows"                  
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

# <a name="windows-universal-apps-engagement-sdk-integration"></a>Integracija s Windows SDK radnje univerzalni aplikacije

> [AZURE.SELECTOR] 
- [Univerzalni sustava Windows](mobile-engagement-windows-store-integrate-engagement.md) 
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md) 
- [iOS](mobile-engagement-ios-integrate-engagement.md) 
- [Android](mobile-engagement-android-integrate-engagement.md) 

U ovom postupku opisano najjednostavniji način da biste aktivirali analize radnje korisnika te praćenje funkcije u programu Windows univerzalni.

Sljedeći koraci nisu dovoljno da biste aktivirali izvješća zapisnika potrebne za izračun svih statističkih podataka vezanih uz korisnika, sesije, aktivnosti, ruši i Technicals. Izvješće zapisnika potrebne za izračunavanje ostalih statističkih podataka kao što su događaji, pogrešaka i poslove se mora poduzeti ručno pomoću radnje API-JA (pogledajte [kako koristiti dodatne radnje Mobile označavanje API-JA u aplikaciji Windows univerzalni](mobile-engagement-windows-store-use-engagement-api.md) jer te statistike aplikacije zavisne.

## <a name="supported-versions"></a>Podržane verzije

Mobilni radnje SDK za Windows univerzalni aplikacija može se integrirati u Windows Runtime i ciljne aplikacije za univerzalni platformu sustava Windows:

-   Windows 8
-   Windows 8.1
-   Windows Phone 8.1
-   Windows 10 (površini i mobilni linije)

> [AZURE.NOTE] Ako birate Silverlight za Windows Phone, zatim se s [postupak Integracija s programom Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md).

## <a name="install-the-mobile-engagement-universal-apps-sdk"></a>Instalacija aplikacije SDK univerzalni mobilne radnje

### <a name="all-platforms"></a>Sve platforme

Mobilni radnje SDK za univerzalni aplikacija Windows nije dostupan kao paket Nuget pod nazivom *MicrosoftAzure.MobileEngagement*. Možete ga instalirati iz programa Visual Studio Nuget paket upravitelja.

### <a name="windows-8x-and-windows-phone-81"></a>Windows 8.x i Windows Phone 8.1

NuGet automatski uvodi SDK resursa u na `Resources` mapa u korijenu aplikacije projekta.

### <a name="windows-10-universal-windows-platform-applications"></a>Aplikacija za Windows 10 univerzalni platformu sustava Windows

NuGet implementirati automatski SDK resursa u aplikaciji UWP još. To morate učiniti ga ručno dok resursi implementacije je reintroduced u NuGet:

1.  Otvorite Eksplorer za datoteke.
2.  Pomaknite se do sljedećeg mjesta (**x.x.x** je verzija radnje instalirate): *korisničkog PROFILA %\\.nuget\packages\MicrosoftAzure.MobileEngagement\\**x.x.x**\\content\win81*
3.  Povucite i ispustite mapu **resursa** u eksploreru za datoteke u korijen projekta u Visual Studio.
4.  U Visual Studio odabir projekta i aktiviranje ikonu **Prikaži sve datoteke** pri vrhu **Preglednik rješenja**.
5.  Neke datoteke nisu uvršteni u projektu. Da biste uvezli ih istodobno desnom tipkom miša kliknite mapu **Resursi** **izuzeti iz projekta** , a zatim drugi desnom tipkom miša kliknite mapu **resursa** , **Uključi u programu project** da biste ponovno uključili cijelu mapu. Sve datoteke iz mape **Resursi** sada nalaze se u projektu.

## <a name="add-the-capabilities"></a>Dodavanje mogućnosti

Radnje SDK mora neke mogućnosti programa Windows SDK da bi funkcionirao.

Otvaranje vaše `Package.appxmanifest` datoteku i biti sigurni da su deklarirane sljedeće mogućnosti:

-   `Internet (Client)`

## <a name="initialize-the-engagement-sdk"></a>Pokretanje radnje SDK

### <a name="engagement-configuration"></a>Konfiguriranje radnje

Konfiguracija radnje je centralizirano u na `Resources\EngagementConfiguration.xml` datoteke u projekt.

Uređivanje ove datoteke da biste odredili:

-   Aplikacija niz za povezivanje u između oznake `<connectionString>` i `<\connectionString>`.

Ako želite da biste odredili je prilikom izvođenja, možete pozvati sljedeću metodu prije agent za inicijalizaciju radnje:
          
          /* Engagement configuration. */
          EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

          /* Set the Engagement connection string. */
          engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

          /* Initialize Engagement angent with above configuration. */
          EngagementAgent.Instance.Init(e, engagementConfiguration);

Na portalu klasični Azure prikazat će se niz za povezivanje za svoju aplikaciju.

### <a name="engagement-initialization"></a>Pokretanje radnje

Prilikom stvaranja novog projekta u `App.xaml.cs` generira datoteka. Klase nasljeđuje od `Application` i sadrži mnogo važne načine. Će se koristiti i za inicijalizaciju SDK radnje.

Izmjena na `App.xaml.cs`:

-   Dodajte svoje `using` izvješća:

        using Microsoft.Azure.Engagement;

-   Odredite način zajedničkog korištenja za inicijalizaciju radnje jednom za sve pozive:

        private void InitEngagement(IActivatedEventArgs e)
        {
          EngagementAgent.Instance.Init(e);
        
          // or
        
          EngagementAgent.Instance.Init(e, engagementConfiguration);
        }
        
-   Pozivanje `InitEngagement` u na `OnLaunched` metoda:

        protected override void OnLaunched(LaunchActivatedEventArgs e)
        {
          InitEngagement(e);
        }

-   Kada se pokrene aplikacije pomoću prilagođene sheme, neku drugu aplikaciju ili naredbeni redak, a zatim na `OnActivated` zove se način. Morate započeti SDK radnje pri aplikacijom je aktiviran. Da biste to učinili, nadjačati `OnActivated` metoda:

        protected override void OnActivated(IActivatedEventArgs args)
        {
          InitEngagement(args);
        }

> [AZURE.IMPORTANT] Ne možemo svakako radnju želite dodati radnje za inicijalizaciju na drugom mjestu aplikacije.

## <a name="basic-reporting"></a>Osnovni izvješćivanje o pogreškama

### <a name="recommended-method-overload-your-page-classes"></a>Preporučeni način: preopterećenja vaše `Page` klase

Da biste aktivirali izvješća sve zapisnike potrebnih radnje za izračunavanje korisnika, sesije, aktivnosti, ruši i tehničke Statistika, možete jednostavno napraviti sve svoje `Page` podređenu klase nasljeđuju od na `EngagementPage` klase.

Evo primjera kako to učiniti za stranicu aplikacije. To možete učiniti na isti način za sve stranice aplikacije.

#### <a name="c-source-file"></a>C# izvorišna datoteka

Izmjena stranici `.xaml.cs` datoteke:

-   Dodajte svoje `using` izvješća:

        using Microsoft.Azure.Engagement;

-   Zamjena `Page` s `EngagementPage`:

**Bez radnje:**
    
        namespace Example
        {
          public sealed partial class ExamplePage : Page
          {
            [...]
          }
        }

**S mogućnostima:**

        using Microsoft.Azure.Engagement;
        
        namespace Example
        {
          public sealed partial class ExamplePage : EngagementPage 
          {
            [...]
          }
        }

> [AZURE.IMPORTANT] Ako se vaša stranica nadjačava na `OnNavigatedTo` metoda biti sigurni da biste uputili poziv `base.OnNavigatedTo(e)`. U suprotnom aktivnost će biti prijavio je (u `EngagementPage` pozive `StartActivity` unutar njegov `OnNavigatedTo` način).

#### <a name="xaml-file"></a>XAML datoteke

Izmjena stranici `.xaml` datoteke:

-   Dodajte svoje deklaracija prostora naziva:

        xmlns:engagement="using:Microsoft.Azure.Engagement"

-   Zamjena `Page` s `engagement:EngagementPage`:

**Bez radnje:**

        <Page>
            <!-- layout -->
            ...
        </Page>

**S mogućnostima:**

        <engagement:EngagementPage 
            xmlns:engagement="using:Microsoft.Azure.Engagement">
            <!-- layout -->
            ...
        </engagement:EngagementPage >

#### <a name="override-the-default-behaviour"></a>Zanemarivanje zadano ponašanje

Prema zadanim postavkama, naziv klase stranice prijavljuje kao naziv aktivnosti s bez extra. Ako je klasa koristi sufiks "Stranica", radnje je i ukloniti.

Ako želite promijeniti zadano ponašanje za naziv, jednostavno dodajte to kod:

        // in the .xaml.cs file
        protected override string GetEngagementPageName()
        {
          /* your code */
          return "new name";
        }

Ako želite da biste prijavili nekoliko dodatnih informations s aktivnosti, možete to dodati kod:

        // in the .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
          /* your code */
          return extra;
        }

Načini nazivaju se unutar na `OnNavigatedTo` način stranice.

### <a name="alternate-method-call-startactivity-manually"></a>Alternativni metoda: poziva `StartActivity()` ručno

Ako ne možete ili ne želite preopterećenja vaše `Page` klase, umjesto toga možete pokrenuti aktivnosti tako da nazovete `EngagementAgent` izravno metode.

Preporučujemo da biste uputili poziv `StartActivity` unutar vaše `OnNavigatedTo` način stranice.

            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
              base.OnNavigatedTo(e);
              EngagementAgent.Instance.StartActivity("MyPage");
            }

> [AZURE.IMPORTANT]  Provjerite je li pravilno završili sesiju.
> 
> Windows SDK univerzalni automatski poziva na `EndActivity` način prilikom zatvaranja. Stoga je **vrlo** preporučuje se da biste uputili poziv na `StartActivity` način kad god se aktivnosti korisnika promijeniti i na **NIKAD** poziva na `EndActivity` metoda ovu metodu šalje radnje poslužitelju da trenutnog korisnika sadrži ostavite aplikaciju, to utječe na sve aplikacije zapisnika.

## <a name="advanced-reporting"></a>Dodatna izvješća

Po želji, trebali biste izvješće aplikacije određene događaje, pogrešaka i zadacima, da biste to učinili, koristite na druge načine pronađeni u na `EngagementAgent` predmete. API radnje omogućuje korištenje svih radnje, dodatne mogućnosti.

Dodatne informacije potražite u članku [kako koristiti dodatne radnje Mobile API-JA u aplikaciji Windows univerzalni za označavanje](mobile-engagement-windows-store-use-engagement-api.md).

##<a name="advanced-configuration"></a>Napredna konfiguracija

### <a name="disable-automatic-crash-reporting"></a>Onemogućivanje automatskog rušenje izvješća

Možete onemogućiti automatsko rušenje izvješćivanja značajke radnje. Zatim, kada neobrađenu iznimku će se pojaviti, radnje ne ništa.

> [AZURE.WARNING] Ako planirate onemogućiti tu značajku, imajte na umu da kada neobrađenu neočekivanog će se pojaviti u aplikaciji, radnje neće slati rušenje **i** ne zatvorite sesiju i zadacima.

Da biste onemogućili automatsko rušenje izvješćivanja, samo prilagoditi konfiguraciju ovisno o način na koji je deklariran:

#### <a name="from-engagementconfigurationxml-file"></a>Iz `EngagementConfiguration.xml` datoteka

Postavite izvješće rušenje `false` između `<reportCrash>` i `</reportCrash>` oznake.

#### <a name="from-engagementconfiguration-object-at-run-time"></a>Iz `EngagementConfiguration` objekta u vrijeme izvođenja

Izvješće rušenje postavite na false pomoću EngagementConfiguration objekta.

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
        
        /* Disable Engagement crash reporting. */
        engagementConfiguration.Agent.ReportCrash = false;

### <a name="burst-mode"></a>Brzi niz načinu rada

Prema zadanim postavkama, izvješća o usluzi radnje zapisnike u stvarnom vremenu. Ako aplikacija vrlo često izvješća zapisnika, bolje je da biste međuspremnika zapisnike, a da biste prijavili njih odjedanput na u pravilnim vremenskim base (to se naziva "način neprekidnog rada").

Da biste to učinili, nazovite metoda:

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

Argument je vrijednost u **milisekundama**. U bilo kojem trenutku, ako želite da biste ponovno aktivirali u stvarnom vremenu zapisivanje, samo pozvati metodu bez sve parametra ili vrijednost 0.

Način neprekidnog rada malo povećati trajanja baterija, ali je utjecaj na monitoru radnje: sve sesije i zadacima trajanje će biti zaokruženi praga neprekidnog rada (dakle, sesije i zadacima kraće nego praga neprekidnog rada možda neće biti vidljiva). Preporučuje se da biste koristili praga neprekidnog rada više od 30000 (30s). Morate imati na umu spremljenih zapisnika ograničeni su na 300 stavki. Ako pošaljete je predugačak može izgubiti neka zapisnika.

> [AZURE.WARNING] Prag neprekidnog rada ne može konfigurirati na manje od 1s razdoblje. Ako pokušate da biste to učinili, SDK prikazat će se praćenje uz pogrešku i automatski ponovno Postavi zadane vrijednosti, odnosno 0s. To će pokrenuti SDK da biste prijavili zapisnike u stvarnom vremenu.

[here]:http://www.nuget.org/packages/Capptain.WindowsCS
[NuGet website]:http://docs.nuget.org/docs/start-here/overview
 
