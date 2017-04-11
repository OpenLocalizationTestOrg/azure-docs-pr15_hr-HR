<properties 
    pageTitle="Univerzalni aplikacije Windows SDK nadograditi postupaka" 
    description="Univerzalni aplikacije Windows SDK nadogradnje postupke za Azure mobilne radnje"                     
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

#<a name="windows-universal-apps-sdk-upgrade-procedures"></a>Univerzalni aplikacije Windows SDK nadograditi postupaka

Ako već imate integrirali stariju verziju radnje u aplikaciji, morate Imajte na umu sljedeće prilikom nadogradnje SDK-a.

Možda ćete morati slijedite nekoliko postupaka ako Propušteni nekoliko verzija SDK-a. Ako, primjerice migriranja iz 0.10.1 u 0.11.0 morate najprije slijedite postupak "iz 0.9.0 za 0.10.1" pa postupak "iz 0.10.1 za 0.11.0".

##<a name="from-330-to-340"></a>Iz 3.3.0 za 3.4.0

### <a name="test-logs"></a>Testiranje zapisnika

Zapisnici konzole za koje je stvorio SDK sada može biti omogućeno/onemogućeno/filtriranja. Da biste prilagodili, ažurirali svojstvo `EngagementAgent.Instance.TestLogEnabled` na jednu vrijednost dostupna na `EngagementTestLogLevel` Enumeracije, na primjer:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

### <a name="resources"></a>Resursi

Poboljšano je prekriveno razgovor. Nije dio paketa resursa SDK NuGet.

Tijekom nadogradnje na novu verziju SDK možete odabrati želite li zadržati postojeće datoteke iz mape prekriveno resursa ili ne:

* Ako prethodna prekrivanja radi za vas ili su Integracija s `WebView` elemenata ručno pa možete odlučiti da biste zadržali postojeći datoteke, on će i dalje funkcionirati. 
* Ako želite ažurirati na novi prekriveno samo zamijeniti cjeline `overlay` mape iz resursa s novom iz paketa SDK (UWP aplikacije: nakon nadogradnje, možete dobiti u novu mapu prekriveno % korisničkog PROFILA\\.nuget\packages\MicrosoftAzure.MobileEngagement\3.4.0\content\win81\Resources).

> [AZURE.WARNING] Pomoću nove prekriveno prebrisat će se sve prilagodbe na prethodne verzije.

##<a name="from-320-to-330"></a>Iz 3.2.0 za 3.3.0

### <a name="resources"></a>Resursi
Ovaj korak odnosi samo prilagođene resursi. Ako ste prilagodili resurse nudi SDK (html, slike, prekriveno) pa ćete morati sigurnosno kopiranje prije nadogradnje i ponovna primjena prilagodbu nadograđena resursa.

##<a name="from-310-to-320"></a>Iz 3.1.0 za 3.2.0

### <a name="resources"></a>Resursi
Ovaj korak odnosi samo prilagođene resursi. Ako ste prilagodili resurse nudi SDK (html, slike, prekriveno) pa ćete morati sigurnosno kopiranje prije nadogradnje i ponovna primjena prilagodbu nadograđena resursa.

### <a name="webview-integration"></a>Integracija prikaza
Nekoliko poboljšanja tako da odgovara čimbenika obrasca na drugi uređaj uvedene u ovoj verziji. Provjerite odgovara li vaš Integracija s prikaza sljedeće:

U vašem XAML stranice ():

            <WebView x:Name="engagement_notification_content" Visibility="Collapsed" Height="80" HorizontalAlignment="Right" VerticalAlignment="Top"/>
            <WebView x:Name="engagement_announcement_content" Visibility="Collapsed" HorizontalAlignment="Right" VerticalAlignment="Top"/> 

I u datoteci povezane .cs:

    using Microsoft.Azure.Engagement;
    using System;
    using Windows.ApplicationModel.Core;
    using Windows.UI.ViewManagement;
    using Windows.UI.Xaml;
    using Windows.UI.Xaml.Navigation;

    namespace My.Namespace.Example
    {
            /// <summary>
            /// An empty page that can be used on its own or navigated to within a Frame.
            /// </summary>
            public sealed partial class ExampleEngagementReachPage : EngagementPage
            {
              public ExampleEngagementReachPage()
              {
                this.InitializeComponent();
            
                /* Set your webview elements to the correct size. */
                SetWebView(width, height);
              }
            
              #region to implement
              /* Attach events when page is navigated. */
              protected override void OnNavigatedTo(NavigationEventArgs e)
              {
                /* Update the webview when the app window is resized. */
                Window.Current.SizeChanged += DisplayProperties_OrientationChanged;

                /* Update the webview when the app/status bar is resized. */
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
                ApplicationView.GetForCurrentView().VisibleBoundsChanged += DisplayProperties_VisibleBoundsChanged; 
    #endif
                base.OnNavigatedTo(e);
              }

              /* When page is left ensure to detach SizeChanged handler. */
              protected override void OnNavigatedFrom(NavigationEventArgs e)
              {
                Window.Current.SizeChanged -= DisplayProperties_OrientationChanged;
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
                ApplicationView.GetForCurrentView().VisibleBoundsChanged -= DisplayProperties_VisibleBoundsChanged;
    #endif
                base.OnNavigatedFrom(e);
              }
              
              /* "width" and "height" are the current size of your application display. */
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
              double width = ApplicationView.GetForCurrentView().VisibleBounds.Width;
              double height = ApplicationView.GetForCurrentView().VisibleBounds.Height;
    #else
              double width =  Window.Current.Bounds.Width;
              double height =  Window.Current.Bounds.Height;
    #endif
            
              /// <summary>
              /// Set your webview elements to the correct size.
              /// </summary>
              /// <param name="width">The width of your current display.</param>
              /// <param name="height">The height of your current display.</param>
              private void SetWebView(double width, double height)
              {
                #pragma warning disable 4014
                CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
                        () =>
                        {
                          this.engagement_notification_content.Width = width;
                          this.engagement_announcement_content.Width = width;
                          this.engagement_announcement_content.Height = height;
                        });
              }
            
              /// <summary>
              /// Handler that takes the Windows.Current.SizeChanged and indicates that webviews have to be resized.
              /// </summary>
              /// <param name="sender">Original event trigger.</param>
              /// <param name="e">Window Size Changed Event arguments.</param>
              private void DisplayProperties_OrientationChanged(object sender, Windows.UI.Core.WindowSizeChangedEventArgs e)
              {
                double width = e.Size.Width;
                double height = e.Size.Height;
            
                /* Set your webview elements to the correct size. */
                SetWebView(width, height);
              }

    #if WINDOWS_PHONE_APP || WINDOWS_UWP              
              /// <summary>
              /// Handler that takes the ApplicationView.VisibleBoundsChanged and indicates that webviews have to be resized
              /// </summary>
              /// <param name="sender">The related application view.</param>
              /// <param name="e">Related event arguments.</param>
              private void DisplayProperties_VisibleBoundsChanged(ApplicationView sender, Object e)
              {
                double width = sender.VisibleBounds.Width;
                double height = sender.VisibleBounds.Height;
            
                /* Set your webview elements to the correct size. */
                SetWebView(width, height);
              }
    #endif
              #endregion
            }
    }

##<a name="from-200-to-300"></a>Iz 2.0.0 za 3.0.0

### <a name="resources"></a>Resursi
Ovaj korak odnosi samo prilagođene resursi. Ako ste prilagodili resurse nudi SDK (html, slike, prekriveno) pa ćete morati sigurnosno kopiranje prije nadogradnje i ponovna primjena prilagodbu nadograđena resursa.

##<a name="from-111-to-200"></a>Iz 1.1.1 za 2.0.0

Sljedeće opisuje kako migrirati programa SDK Integracija sa servisa Capptain u aplikaciju pokreće Azure Mobile radnje koje nudi Capptain SAS. 

> [Azure.IMPORTANT] Capptain i Mobile radnje nisu iste usluge i postupak dolje samo ističe migriranju aplikaciju klijenta. Migracija SDK u aplikaciji će nije moguće migrirati podatke s poslužitelja Capptain poslužiteljima Mobile radnje

Ako premještate iz starije verzije, obratite se Capptain web-mjesta migrirati 1.1.1 prvi put, a zatim primijenite sljedeći postupak

### <a name="nuget-package"></a>Paket Nuget

Da biste zamijenili **Capptain.WindowsPhone** u **MicrosoftAzure.MobileEngagement** Nuget paketa.

### <a name="applying-mobile-engagement"></a>Primjena mobilne radnje

SDK koristi termin `Engagement`. Trebate ažurirati projekta u skladu tu promjenu.

Morate deinstalirati trenutnog Capptain nuget paketa. Preporučujemo da se sve promjene u mapi Capptain resursi će se ukloniti. Ako želite zadržati te datoteke, a zatim kopirajte ih.

Nakon toga, instalirajte Microsoft Azure Engagement nuget paket na projektu. Možete je pronaći izravno na [nuget web-mjesta]. ili ovdje indeks. Ova akcija zamjenjuje sve datoteke resursa koje koristi radnje i dodaje novi DLL radnje reference projekta.

Imate da biste očistili reference projekta brisanjem Capptain DLL reference. Ako se ne bi, verziju Capptain bit će u sukobu i dogodit će se pogreške.

Ako ste prilagodili Capptain resursa, kopirajte stari sadržaj datoteke, pa ih zalijepite u nove datoteke radnje. Napominjemo da morate ažurirati xaml i cs datoteke.

Nakon tih koraka samo morate zamijeniti stari Capptain reference tako da novi reference radnje.

1. Svi prostori naziva Capptain, morate ažurirati.

    Prije migracije:
    
        using Capptain.Agent;
        using Capptain.Reach;
    
    Nakon migracije:
    
        using Microsoft.Azure.Engagement;

2. Svi Tečajevi Capptain koji sadrže "Capptain" mora sadržavati "Radnje".

    Prije migracije:
    
        public sealed partial class MainPage : CapptainPage
        {
          protected override string GetCapptainPageName()
          {
            return "Capptain Demo";
          }
          ...
        }
    
    Nakon migracije:
    
        public sealed partial class MainPage : EngagementPage
        {
          protected override string GetEngagementPageName()
          {
            return "Engagement Demo";
          }
          ...
        }

3. Xaml datoteke Capptain prostor naziva i atributi i promijenite.

    Prije migracije:
    
        <capptain:CapptainPage
        ...
        xmlns:capptain="clr-namespace:Capptain.Agent;assembly=Capptain.Agent.WP"
        ...
        </capptain:CapptainPage>
    
    Nakon migracije:
    
        <engagement:EngagementPage
        ...
        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
        ...
        </engagement:EngagementPage>

4. Preklapanje stanice
    > [AZURE.IMPORTANT] Preklapanje i promjena. Njegov novi prostor za naziv je `Microsoft.Azure.Engagement.Overlay`. Ima će se koristiti u xaml i cs datoteke. Nadalje `CapptainGrid` će biti pod nazivom `EngagementGrid`, `capptain_notification_content` i `capptain_announcement_content` imenovane su `engagement_notification_content` i `engagement_announcement_content`.
    
    Za prekriveno:
    
        <capptain:CapptainPageOverlay
          xmlns:capptain="using:Capptain.Overlay"
          ...
        </capptain:CapptainPageOverlay>
    
    Postane:
    
        <EngagementPageOverlay
          engagement="using:Microsoft.Azure.Engagement.Overlay"
          ...
        </engagement:EngagementPageOverlay>

5. Za ostale resurse kao što su slike Capptain i HTML datoteke, uzmite u obzir da su također biti preimenovana da biste koristili "Radnje".

### <a name="project-declaration"></a>Deklariranje projekta

Na Package.appxmanifest `File Type Associations` ažurirana iz:

 -   capptain\_dosegne\_sadržaja da biste radnje\_dosegne\_sadržaja
 -   capptain\_zapisnika\_datoteku da biste radnje\_zapisnika\_datoteka

### <a name="application-id--sdk-key"></a>ID aplikacije / SDK ključ

Radnje koristi niz za povezivanje. Ne morate navesti ID aplikacije i ključa SDK s mogućnostima Mobile, imate samo za određivanje niza za povezivanje. Možete je postaviti tako na EngagementConfiguration datoteku.

Konfiguracija radnje možete postaviti u svoje `Resources\EngagementConfiguration.xml` datoteke u projekt.

Uređivanje ove datoteke da biste odredili:

-   Aplikacija niz za povezivanje u između oznake `<connectionString>` i `<\connectionString>`.

Ako želite da biste odredili je prilikom izvođenja, možete pozvati sljedeću metodu prije agent za inicijalizaciju radnje:

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
    
    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(args, engagementConfiguration);

Na portalu klasični Azure prikazat će se niz za povezivanje za svoju aplikaciju.

### <a name="items-name-change"></a>Promjena naziva stavki

Sve stavke pod nazivom *capptain* sadrže je pod nazivom *radnje*. Na sličan način za *Capptain* za *radnje*.

Primjeri najčešće korištenih Capptain stavki:

-   CapptainConfiguration sada pod nazivom EngagementConfiguration
-   CapptainAgent sada pod nazivom EngagementAgent
-   CapptainReach sada pod nazivom EngagementReach
-   CapptainHttpConfig sada pod nazivom EngagementHttpConfig
-   GetCapptainPageName sada pod nazivom GetEngagementPageName

Imajte na umu da je preimenovati i utječe na nadjačati metode.

 
