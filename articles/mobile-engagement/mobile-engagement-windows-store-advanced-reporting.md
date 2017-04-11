<properties
    pageTitle="Windows univerzalno dodatna izvješća s MobileApps radnje"
    description="Kako integrirati Azure mobilne radnje s Universal aplikacijama za Windows"                  
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/12/2016"
    ms.author="piyushjo;ricksal" />

# <a name="advanced-reporting-with-the-windows-universal-apps-engagement-sdk"></a>Dodatna izvješća s univerzalni aplikacije Radnje Windows SDK

> [AZURE.SELECTOR]
- [Univerzalni sustava Windows](mobile-engagement-windows-store-advanced-reporting.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-advanced-reporting.md)

U ovoj se temi opisuju dodatne scenarije izvješćivanja u programu Windows univerzalni. Sljedećim scenarijima obuhvaćaju mogućnosti koje možete primijeniti na aplikaciju stvorili praktičnog vodiča za [Početak rada](mobile-engagement-windows-store-dotnet-get-started.md) .

## <a name="prerequisites"></a>Preduvjeti

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

Prije pokretanja ovog praktičnog vodiča, najprije morate dovršiti vodič [Početak rada](mobile-engagement-windows-store-dotnet-get-started.md) , što je namjerno Izravni i jednostavno. Pomoću ovog praktičnog vodiča pokriva dodatne mogućnosti možete odabrati.

## <a name="specifying-engagement-configuration-at-runtime"></a>Određivanje konfiguracije radnje prilikom izvođenja

Konfiguracija radnje je centralizirano u na `Resources\EngagementConfiguration.xml` datoteku projekta, što je kojima je navedeno u članku [Početak rada](mobile-engagement-windows-store-dotnet-get-started.md) .

No možete i navesti je prilikom izvođenja: možete pozvati sljedeću metodu prije agent za inicijalizaciju radnje:

          /* Engagement configuration. */
          EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

          /* Set the Engagement connection string. */
          engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

          /* Initialize Engagement angent with above configuration. */
          EngagementAgent.Instance.Init(e, engagementConfiguration);



## <a name="recommended-method-overload-your-page-classes"></a>Preporučeni način: preopterećenja vaše `Page` klase

Da biste aktivirali izvješćivanja sve zapisnike potrebnih radnje za izračunavanje korisnika, sesije, aktivnosti, ruši i tehničke Statistika, provjerite sve svoje `Page` podređenu klase nasljeđuju od na `EngagementPage` klase.

Evo primjera za stranicu aplikacije. To možete učiniti na isti način za sve stranice aplikacije.

### <a name="c-source-file"></a>C# izvorišna datoteka

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

> [AZURE.IMPORTANT] Ako se vaša stranica nadjačava na `OnNavigatedTo` metoda biti sigurni da biste uputili poziv `base.OnNavigatedTo(e)`. U suprotnom aktivnosti neće biti prijavljuje (u `EngagementPage` pozive `StartActivity` unutar njegov `OnNavigatedTo` način).

### <a name="xaml-file"></a>XAML datoteke

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

### <a name="override-the-default-behaviour"></a>Zanemarivanje zadano ponašanje

Prema zadanim postavkama, naziv klase stranice prijavljuje kao naziv aktivnosti s bez extra. Ako klasa koristi sufiks "Stranica", radnje uklanja.

Da biste nadjačali zadano ponašanje za naziv, dodati kod:

        // in the .xaml.cs file
        protected override string GetEngagementPageName()
        {
          /* your code */
          return "new name";
        }

Da biste izvješće suvišnih informacija s aktivnosti, dodati kod:

        // in the .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
          /* your code */
          return extra;
        }

Načini nazivaju se unutar na `OnNavigatedTo` način stranice.

### <a name="alternate-method-call-startactivity-manually"></a>Alternativni metoda: poziva `StartActivity()` ručno

Ako ne možete ili ne želite preopterećenja vaše `Page` klase, umjesto toga možete pokrenuti aktivnosti tako da nazovete `EngagementAgent` izravno metode.

Preporučujemo da se zovete `StartActivity` unutar vaše `OnNavigatedTo` način stranice.

            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
              base.OnNavigatedTo(e);
              EngagementAgent.Instance.StartActivity("MyPage");
            }

> [AZURE.IMPORTANT]  Provjerite je li pravilno završili sesiju.
>
> Windows SDK univerzalni automatski poziva na `EndActivity` način prilikom zatvaranja. Stoga je **vrlo** preporučuje se da biste uputili poziv na `StartActivity` način kad god se aktivnosti korisnika promijeniti i na **NIKAD** poziva na `EndActivity` način. Ta metoda obavještava poslužitelj radnje trenutnog korisnika je otišao aplikacija će utjecati na sve aplikacije zapisnika.

## <a name="advanced-reporting"></a>Dodatna izvješća

Po želji, trebali biste izvješća specifičnim aplikacijama događaje, pogrešaka i zadacima da biste to učinili, koristite na druge načine pronaći u na `EngagementAgent` klasu. API radnje omogućuje korištenje naprednih mogućnosti sve radnje.

Dodatne informacije potražite u članku [kako koristiti dodatne radnje Mobile API-JA u aplikaciji Windows univerzalni za označavanje](mobile-engagement-windows-store-use-engagement-api.md).
