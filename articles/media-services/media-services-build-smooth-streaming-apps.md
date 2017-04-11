<properties 
    pageTitle="Glačanje strujanje vodič aplikacija iz Windows trgovine | Microsoft Azure" 
    description="Saznajte kako pomoću servisa Azure Media Services radi stvaranja aplikacije C# iz Windows trgovine s XML MediaElement kontrole za reprodukciju izglađenim strujanje sadržaja." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"  
    ms.author="juliako"/>



#<a name="how-to-build-a-smooth-streaming-windows-store-application"></a>Sastavljanje prelazili putem glatke strujanje aplikacija iz Windows trgovine

Smooth Streaming klijent SDK za sustava Windows 8 omogućuje inženjerima omogućuje stvaranje aplikacija iz Windows trgovine koji može reproducirati na zahtjev i uživo Smooth Streaming sadržaja. Uz osnovne reprodukcijom Smooth Streaming sadržaja SDK-a nudi obogaćenog značajke kao što su zaštita Microsoft PlayReady, kvalitete zvuka razinu ograničenja, Live DVR strujanje prijelaz, slušanje ažuriranja statusa (kao što su promjene na razini kvalitete) i događaji pogrešaka i tako dalje. Dodatne informacije o podržanim značajkama potražite u članku [Napomene](http://www.iis.net/learn/media/smooth-streaming/smooth-streaming-client-sdk-for-windows-8-release-notes). Dodatne informacije potražite u članku [reproduktora Framework za Windows 8](http://playerframework.codeplex.com/). 

Pomoću ovog praktičnog vodiča sadrži četiri predavanja:

1. Stvaranje osnovne izglađenim strujanje aplikacije za pohranu
2. Dodavanje klizača da biste odredili tijeku medijske sadržaje
3. Odaberite Smooth Streaming strujanja
4. Odaberite Smooth Streaming zapise

##<a name="prerequisites"></a>Preduvjeti

- Windows 8 32-bitne ili 64-bitni. [Windows 8 Enterprise procjenu](http://msdn.microsoft.com/evalcenter/jj554510.aspx) možete pristupiti iz MSDN.
- Visual Studio 2012 ili Visual Studio Express 2012 (ili novija verzija). Probnoj verziji možete pristupiti iz [ovdje](http://www.microsoft.com/visualstudio/11/downloads).
- [Microsoft prelazili putem glatke strujanje klijent SDK za Windows 8](http://visualstudiogallery.msdn.microsoft.com/04423d13-3b3e-4741-a01c-1ae29e84fea6?SRC=Homehttp://visualstudiogallery.msdn.microsoft.com/04423d13-3b3e-4741-a01c-1ae29e84fea6?SRC=Home).


Dovršeni rješenje za svaki nastave mogu se preuzeti sa primjere koda za razvojne inženjere MSDN (kod Galerija): 

- [Lekciju 1](http://code.msdn.microsoft.com/Smooth-Streaming-Client-0bb1471f) - prelazili putem glatke jednostavne Windows 8 na strujanje Media Player 
- Nadzor [nastave 2](http://code.msdn.microsoft.com/A-simple-Windows-8-Smooth-ee98f63a) – jednostavno Windows 8 Smooth Streaming reproduktor s klizač, 
- [Lekciju 3](http://code.msdn.microsoft.com/A-Windows-8-Smooth-883c3b44) – Windows 8 jednostavni strujanje reproduktor s odabirom strujanje  
- [Lekciju 4](http://code.msdn.microsoft.com/A-Windows-8-Smooth-aa9e4907) - prelazili putem glatke Windows 8 na strujanje reproduktor s odabirom evidentiranje.

##<a name="lesson-1-create-a-basic-smooth-streaming-store-application"></a>Lekciju 1: Stvaranje osnovne izglađenim strujanje aplikacije za pohranu

U ovom nastave ćete stvoriti iz Windows trgovine aplikacija MediaElement kontrole za reprodukciju izglađenim strujanje sadržaja.  Pokrenuti program izgleda ovako:

![Primjer aplikacije prelazili putem glatke strujeće iz Windows trgovine][PlayerApplication]
 
Dodatne informacije o razvoju aplikacija iz Windows trgovine, potražite u članku [razviti sjajno aplikacija za Windows 8](http://msdn.microsoft.com/windows/apps/br229512.aspx). U ovom nastave sadrži sljedeće postupke:

1.  Stvaranje projekta iz Windows trgovine
2.  Dizajn korisničko sučelje (XAML)
3.  Izmjena kod u pozadini datoteka
4.  Kompiliranje i testiranje aplikacije

**Stvaranje projekta iz Windows trgovine**

1.  Pokrenite Visual Studio 2012 ili noviji.
2.  Na izborniku **datoteka** kliknite **Novo**, a zatim **projekt**.
3.  U dijaloškom okviru novi projekt, upišite ili odaberite sljedeće vrijednosti:

Ime|Vrijednost
---|---
Predložak grupe|Instaliran/predloške/Visual C# / Windows pohraniti
Predložak|Prilagođenom web-aplikacijom (XAML)
Ime|SSPlayer
Mjesto|C:\SSTutorials
Naziv rješenja|SSPlayer
Stvaranje direktorija za rješenja|(odabrano)

4.  Kliknite **u redu**.

**Da biste dodali referenca Smooth Streaming SDK klijenta**

1.  Iz programa Explorer rješenja, desnom tipkom miša kliknite **SSPlayer**, a zatim **Dodajte referencu**.
2.  Upišite ili odaberite sljedeće vrijednosti:

Ime|Vrijednost
---|---
Referentna grupa|Windows/proširenja
Referenca|Odaberite Microsoft prelazili putem glatke strujanje klijent SDK za Windows 8 i Microsoft Visual C++ izvođenje paketa
    
3.  Kliknite **u redu**. 

Kad dodate reference, morate odabrati ciljano platformu (x64 ili x86), dodavanjem reference neće funkcionirati za konfiguraciju platforme bilo koje procesora.  U pregledniku rješenja, vidjet ćete žuti Označi upozorenje o tim dodali reference.

**Da biste dizajnirali reproduktora korisničkog sučelja**

1.  Preglednik rješenja, dvaput kliknite **MainPage.xaml** da biste ga otvorili u prikazu dizajna.
2.  Pronađite u ** &lt;rešetke&gt; ** i ** &lt;/Grid&gt; ** oznake XAML datoteke i zalijepite sljedeći kod između dvije oznake:

        <Grid.RowDefinitions>
            <RowDefinition Height="20"/>    <!-- spacer -->
            <RowDefinition Height="50"/>    <!-- media controls -->
            <RowDefinition Height="100*"/>  <!-- media element -->
            <RowDefinition Height="80*"/>   <!-- media stream and track selection -->
            <RowDefinition Height="50"/>    <!-- status bar -->
        </Grid.RowDefinitions>
        
        <StackPanel Name="spMediaControl" Grid.Row="1" Orientation="Horizontal">
            <TextBlock x:Name="tbSource" Text="Source :  " FontSize="16" FontWeight="Bold" VerticalAlignment="Center" />
            <TextBox x:Name="txtMediaSource" Text="http://ecn.channel9.msdn.com/o9/content/smf/smoothcontent/elephantsdream/Elephants_Dream_1024-h264-st-aac.ism/manifest" FontSize="10" Width="700" Margin="0,4,0,10" />
            <Button x:Name="btnSetSource" Content="Set Source" Width="111" Height="43" Click="btnSetSource_Click"/>
            <Button x:Name="btnPlay" Content="Play" Width="111" Height="43" Click="btnPlay_Click"/>
            <Button x:Name="btnPause" Content="Pause"  Width="111" Height="43" Click="btnPause_Click"/>
            <Button x:Name="btnStop" Content="Stop"  Width="111" Height="43" Click="btnStop_Click"/>
            <CheckBox x:Name="chkAutoPlay" Content="Auto Play" Height="55" Width="Auto" IsChecked="{Binding AutoPlay, ElementName=mediaElement, Mode=TwoWay}"/>
            <CheckBox x:Name="chkMute" Content="Mute" Height="55" Width="67" IsChecked="{Binding IsMuted, ElementName=mediaElement, Mode=TwoWay}"/>
        </StackPanel>

        <StackPanel Name="spMediaElement" Grid.Row="2" Height="435" Width="1072"
                    HorizontalAlignment="Center" VerticalAlignment="Center">
            <MediaElement x:Name="mediaElement" Height="356" Width="924" MinHeight="225"
                          HorizontalAlignment="Center" VerticalAlignment="Center" 
                          AudioCategory="BackgroundCapableMedia" />
            <StackPanel Orientation="Horizontal">
                <Slider x:Name="sliderProgress" Width="924" Height="44"
                        HorizontalAlignment="Center" VerticalAlignment="Center"
                        PointerPressed="sliderProgress_PointerPressed"/>
                <Slider x:Name="sliderVolume" 
                        HorizontalAlignment="Right" VerticalAlignment="Center" Orientation="Vertical" 
                        Height="79" Width="148" Minimum="0" Maximum="1" StepFrequency="0.1" 
                        Value="{Binding Volume, ElementName=mediaElement, Mode=TwoWay}" 
                        ToolTipService.ToolTip="{Binding Value, RelativeSource={RelativeSource Mode=Self}}"/>
            </StackPanel>
        </StackPanel>

        <StackPanel Name="spStatus" Grid.Row="4" Orientation="Horizontal">
            <TextBlock x:Name="tbStatus" Text="Status :  " 
               FontSize="16" FontWeight="Bold" VerticalAlignment="Center" HorizontalAlignment="Center" />
            <TextBox x:Name="txtStatus" FontSize="10" Width="700" VerticalAlignment="Center"/>
        </StackPanel>

    Kontrola MediaElement koristi se za reproduciranje medijskih sadržaja. Klizač kontrole pod nazivom sliderProgress će se u sljedeći nastave kontrolirati tijek medijske sadržaje.

3.  Pritisnite **CTRL + S** da biste spremili datoteku.

Kontrola MediaElement ne podržava Smooth Streaming sadržaja iz tvorničke. Da biste omogućili podršku Smooth Streaming, morate se registrirati bajt strujanje rukovatelj Smooth Streaming datotečni nastavak i MIME vrsta.  Da biste registrirali, upotrijebite metodu "MediaExtensionManager.RegisterByteStremHandler" Windows.Media prostora za naziv.

U ovoj datoteci XAML povezane s kontrolama su neke rukovatelja događajima.  Morate definirati tih rukovatelja događajima.

**Da biste izmijenili kod u pozadini datoteka**

1.  Iz programa Explorer rješenja, desnom tipkom miša kliknite **MainPage.xaml**, a zatim **Prikaži kod**.
2.  Pri vrhu datoteke, dodajte sljedeće pomoću naredbe:

        using Windows.Media;

3.  Na početku klase **MainPage** član dodali u sljedeće podatke:

        private MediaExtensionManager extensions = new MediaExtensionManager();

4.  Na kraju Graditelj **MainPage** dodajte sljedeća dva retka:

        extensions.RegisterByteStreamHandler("Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", ".ism", "text/xml");
        extensions.RegisterByteStreamHandler("Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", ".ism", "application/vnd.ms-sstr+xml");
        
5.  Na kraju klasu **MainPage** prošle sljedeći kod:

        #region UI Button Click Events
        private void btnPlay_Click(object sender, RoutedEventArgs e)
        {
            mediaElement.Play();
            txtStatus.Text = "MediaElement is playing ...";
        }
        
        private void btnPause_Click(object sender, RoutedEventArgs e)
        {
            mediaElement.Pause();
            txtStatus.Text = "MediaElement is paused";
        }
        
        private void btnSetSource_Click(object sender, RoutedEventArgs e)
        {
            sliderProgress.Value = 0;
            mediaElement.Source = new Uri(txtMediaSource.Text);
        
            if (chkAutoPlay.IsChecked == true)
            {
                txtStatus.Text = "MediaElement is playing ...";
            }
            else
            {
                txtStatus.Text = "Click the Play button to play the media source.";
            }
        }
        
        private void btnStop_Click(object sender, RoutedEventArgs e)
        {
            mediaElement.Stop();
            txtStatus.Text = "MediaElement is stopped";
        }
        
        private void sliderProgress_PointerPressed(object sender, PointerRoutedEventArgs e)
        {
            txtStatus.Text = "Seek to position " + sliderProgress.Value;
            mediaElement.Position = new TimeSpan(0, 0, (int)(sliderProgress.Value));
        }
        #endregion

    Rukovatelj događajima sliderProgress_PointerPressed definirana je ovdje.  Nema više radi učiniti da biste ga osposobili, koji će biti obuhvaćeno sljedeći nastave ovog praktičnog vodiča.
6.  Pritisnite **CTRL + S** da biste spremili datoteku.

U dovršeni kod u pozadini datoteke će izgledati ovako:

![Codeview u aplikaciji za Visual Studio od Smooth Streaming iz Windows trgovine][CodeViewPic]

**Kompiliranje i testiranje aplikacije**

1.  Na izborniku **SASTAVLJANJE** kliknite **Upravitelj konfiguracije**.
2.  Promijenite **Aktivno rješenje platformu** tako da odgovara razvojne platforme.
3.  Pritisnite **F6** prikupiti projekta. 
4.  Pritisnite **F5** da biste pokrenuli aplikaciju.
5.  Pri vrhu aplikacije, možete koristiti zadanu Smooth Streaming URL-a ili unesite neku drugu. 
6.  Kliknite **Postavljanje izvora**. Budući da se **Automatski reproducirati** omogućena je prema zadanim postavkama, medijskih sadržaja moraju se automatski reproducirati.  Možete kontrolirati medijskih sadržaja pomoću **reproduciranje**, **Zaustavljanje** i **Zaustavljanje** gumbi.  Možete kontrolirati Glasnoća medijskog sadržaja pomoću okomitog klizača.  No vodoravni klizač za kontrolu tijeku medijskih sadržaja nije potpuno implementirana još. 

Dovršite lesson1.  U ovom nastave pomoću MediaElement kontrole za reprodukciju Smooth Streaming sadržaj.  U sljedećem nastave dodat će klizač da biste odredili tijeku Smooth Streaming sadržaja.


##<a name="lesson-2-add-a-slider-bar-to-control-the-media-progress"></a>Lekciju 2: Dodavanje klizača da biste odredili tijeku medijske sadržaje
U nastave 1 stvorene iz Windows trgovine aplikacija pomoću MediaElement XAML kontrole za reprodukciju Smooth Streaming medijskog sadržaja.  Dođe neke osnovne media funkcije kao što su početka, tabulatora i Zaustavi.  U ovom nastave s aplikacijom dodat će traka kontrole klizača.

U ovom ćete praktičnom vodiču koristit ćemo vremena da biste ažurirali položaju klizač, ovisno o trenutnom položaju MediaElement kontrole.  Klizač početka i završetka i vrijeme potrebno je ažurirati u slučaju aktivni sadržaj.  To se može bolje riješiti ažuriranje događaj prilagodljivo izvora.

Medijskih su objekti koji generiranje medijski podaci.  Razrješavanje izvor uzima URL-a ili bajt strujanje i stvara izvor odgovarajući medij za sadržaj.  Razrješavanje izvor uobičajeni je način za aplikacije za stvaranje izvora medijske sadržaje. 

U ovom nastave sadrži sljedeće postupke:

1.  Registrirajte se rukovatelj Smooth Streaming 
2.  Dodavanje rukovatelja razine događajima za Upravitelj prilagodljivo izvora
3.  Dodavanje razine događaj rukovatelja prilagodljivo izvora
4.  Dodavanje MediaElement rukovatelja događajima
5.  Dodavanje klizača traka povezane s kodom
6.  Kompiliranje i testiranje aplikacije

**Da biste registrirali bajt strujanje rukovatelj Smooth Streaming i prenesite na propertyset**

1.  Iz programa Explorer rješenja, desnom tipkom miša kliknite **MainPage.xaml**, a zatim **Prikaži kod**.
2.  Na početku datoteku, dodajte sljedeće pomoću naredbe:

        using Microsoft.Media.AdaptiveStreaming;

3.  Na početku klase MainPage dodajte članove sljedeće podatke:

        private Windows.Foundation.Collections.PropertySet propertySet = new Windows.Foundation.Collections.PropertySet();             
        private IAdaptiveSourceManager adaptiveSourceManager;
    
4.  Unutar Graditelj **MainPage** dodajte sljedeći kod nakon na **to. Pokretanje Components();** Linijski grafikoni i registracija kod retke pisane prethodne nastave:
    
        // Gets the default instance of AdaptiveSourceManager which manages Smooth 
        //Streaming media sources.
        adaptiveSourceManager = AdaptiveSourceManager.GetDefault();
        // Sets property key value to AdaptiveSourceManager default instance.
        // {A5CE1DE8-1D00-427B-ACEF-FB9A3C93DE2D}" must be hardcoded.
        propertySet["{A5CE1DE8-1D00-427B-ACEF-FB9A3C93DE2D}"] = adaptiveSourceManager;
    
5.  Unutar Graditelj **MainPage** izmjena dvije metode RegisterByteStreamHandler da biste dodali na četvrtu parametara:

        // Registers Smooth Streaming byte-stream handler for ".ism" extension and, 
        // "text/xml" and "application/vnd.ms-ss" mime-types and pass the propertyset. 
        // http://*.ism/manifest URI resources will be resolved by Byte-stream handler.
        extensions.RegisterByteStreamHandler(
            "Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", 
            ".ism", 
            "text/xml", 
            propertySet );
        extensions.RegisterByteStreamHandler(
            "Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", 
            ".ism", 
            "application/vnd.ms-sstr+xml", 
        propertySet);

6.  Pritisnite **CTRL + S** da biste spremili datoteku.

**Da biste dodali prilagodljivo izvor Upravitelj razine rukovatelj događajima**

1.  Iz programa Explorer rješenja, desnom tipkom miša kliknite **MainPage.xaml**, a zatim **Prikaži kod**.
2.  Unutar klase **MainPage** član dodali u sljedeće podatke:

        private AdaptiveSource adaptiveSource = null;

3.  Na kraju klase **MainPage** , dodajte sljedeći rukovatelj događajima:
    
        #region Adaptive Source Manager Level Events
        private void mediaElement_AdaptiveSourceOpened(AdaptiveSource sender, AdaptiveSourceOpenedEventArgs args)
        {
            adaptiveSource = args.AdaptiveSource;
        }
        #endregion Adaptive Source Manager Level Events

4.  Na kraju Graditelj **MainPage** , dodajte sljedeći redak da biste se pretplatili događaj otvaranje prilagodljivo izvora:
    
    adaptiveSourceManager.AdaptiveSourceOpenedEvent += novi AdaptiveSourceOpenedEventHandler(mediaElement_AdaptiveSourceOpened);

5.  Pritisnite **CTRL + S** da biste spremili datoteku.

**Da biste dodali rukovatelja razine događajima prilagodljivo izvora**

1.  Iz programa Explorer rješenja, desnom tipkom miša kliknite **MainPage.xaml**, a zatim **Prikaži kod**.
2.  Unutar klase **MainPage** član dodali u sljedeće podatke:
    
        private AdaptiveSourceStatusUpdatedEventArgs adaptiveSourceStatusUpdate; 
        private Manifest manifestObject;
    
3.  Na kraju klase **MainPage** , dodajte sljedeće rukovatelja događajima:

        #region Adaptive Source Level Events
        private void mediaElement_ManifestReady(AdaptiveSource sender, ManifestReadyEventArgs args)
        {
            adaptiveSource = args.AdaptiveSource;
            manifestObject = args.AdaptiveSource.Manifest;
        }
        
        private void mediaElement_AdaptiveSourceStatusUpdated(AdaptiveSource sender, AdaptiveSourceStatusUpdatedEventArgs args)
        {
            adaptiveSourceStatusUpdate = args;
        }
        
        private void mediaElement_AdaptiveSourceFailed(AdaptiveSource sender, AdaptiveSourceFailedEventArgs args)
        {
            txtStatus.Text = "Error: " + args.HttpResponse;
        }
        #endregion Adaptive Source Level Events

4.  Na kraju metodu **mediaElement AdaptiveSourceOpened** , dodajte sljedeći kod da biste se pretplatili događajima:
    
        adaptiveSource.ManifestReadyEvent +=
                    mediaElement_ManifestReady;
        adaptiveSource.AdaptiveSourceStatusUpdatedEvent += 
            mediaElement_AdaptiveSourceStatusUpdated;
        adaptiveSource.AdaptiveSourceFailedEvent += 
            mediaElement_AdaptiveSourceFailed;
    
5.  Pritisnite **CTRL + S** da biste spremili datoteku.

Isti događaji dostupni su na prilagodljivo izvor Upravitelj razini kao i koje se mogu koristiti za rukovanje funkcionalnost zajedničke sve medijskih elemenata u aplikaciji. Svaki AdaptiveSource obuhvaća vlastitu događaja, a svi događaji AdaptiveSource će biti kaskadno u odjeljku AdaptiveSourceManager.

**Da biste dodali rukovatelja događajima Element medijske sadržaje**

1.  Iz programa Explorer rješenja, desnom tipkom miša kliknite **MainPage.xaml**, a zatim **Prikaži kod**.
2.  Na kraju klase **MainPage** , dodajte sljedeće rukovatelja događajima:
    
        #region Media Element Event Handlers 
        private void MediaOpened(object sender, RoutedEventArgs e)
        {
            txtStatus.Text = "MediaElement opened";
        }
        
        private void MediaFailed(object sender, ExceptionRoutedEventArgs e)
        {
            txtStatus.Text= "MediaElement failed: " + e.ErrorMessage;
        }
        
        private void MediaEnded(object sender, RoutedEventArgs e)
        {
            txtStatus.Text ="MediaElement ended.";
        }
        #endregion Media Element Event Handlers

3.  Na kraju Graditelj **MainPage** , dodajte sljedeći kod indeks događajima:
    
        mediaElement.MediaOpened += MediaOpened;
        mediaElement.MediaEnded += MediaEnded;
        mediaElement.MediaFailed += MediaFailed;

4.  Pritisnite **CTRL + S** da biste spremili datoteku.

**Da biste dodali klizača povezane s kodom**

1.  Iz programa Explorer rješenja, desnom tipkom miša kliknite **MainPage.xaml**, a zatim **Prikaži kod**.
2.  Na početku datoteku, dodajte sljedeće pomoću naredbe:
    
        using Windows.UI.Core;

3.  Unutar klase **MainPage** dodajte članove sljedeće podatke:
    
        public static CoreDispatcher _dispatcher;
        private DispatcherTimer sliderPositionUpdateDispatcher;
    
4.  Na kraju Graditelj **MainPage** , dodajte sljedeći kod:

        _dispatcher = Window.Current.Dispatcher;
        PointerEventHandler pointerpressedhandler = new PointerEventHandler(sliderProgress_PointerPressed);
        sliderProgress.AddHandler(Control.PointerPressedEvent, pointerpressedhandler, true);    
    
5.  Na kraju klase **MainPage** , dodajte sljedeći kod:
    
        #region sliderMediaPlayer
        private double SliderFrequency(TimeSpan timevalue)
        {
            long absvalue = 0;
            double stepfrequency = -1;
        
            if (manifestObject != null)
            {
                absvalue = manifestObject.Duration - (long)manifestObject.StartTime;
            }
            else
            {
                absvalue = mediaElement.NaturalDuration.TimeSpan.Ticks;
            }
        
            TimeSpan totalDVRDuration = new TimeSpan(absvalue);
        
            if (totalDVRDuration.TotalMinutes >= 10 && totalDVRDuration.TotalMinutes < 30)
            {
               stepfrequency = 10;
            }
            else if (totalDVRDuration.TotalMinutes >= 30 
                     && totalDVRDuration.TotalMinutes < 60)
            {
                stepfrequency = 30;
            }
            else if (totalDVRDuration.TotalHours >= 1)
            {
                stepfrequency = 60;
            }
        
            return stepfrequency;
        }
        
        void updateSliderPositionoNTicks(object sender, object e)
        {
            sliderProgress.Value = mediaElement.Position.TotalSeconds;
        }
        
        public void setupTimer()
        {
            sliderPositionUpdateDispatcher = new DispatcherTimer();
            sliderPositionUpdateDispatcher.Interval = new TimeSpan(0, 0, 0, 0, 300);
            startTimer();
        }

        public void startTimer()
        {
            sliderPositionUpdateDispatcher.Tick += updateSliderPositionoNTicks;
            sliderPositionUpdateDispatcher.Start();
        }
        
        // Slider start and end time must be updated in case of live content
        public async void setSliderStartTime(long startTime)
        {
            await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
                TimeSpan timespan = new TimeSpan(adaptiveSourceStatusUpdate.StartTime);
                double absvalue = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero);
                sliderProgress.Minimum = absvalue;
            });
        }
        
        // Slider start and end time must be updated in case of live content
        public async void setSliderEndTime(long startTime)
        {
            await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
                TimeSpan timespan = new TimeSpan(adaptiveSourceStatusUpdate.EndTime);
                double absvalue = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero);
                sliderProgress.Maximum = absvalue;
            });
        }
        #endregion sliderMediaPlayer

    **Bilješke:** CoreDispatcher se koristi da biste unijeli promjene korisničkog Sučelja niti iz koje nisu niti korisničkog Sučelja. U slučaju usko grlo otpremnik niti za razvojne inženjere možete odabrati da biste koristili otpremnik nudi korisničkog Sučelja element čiji Kra jnjeg da biste ažurirali.  Ako, na primjer:
    
        await sliderProgress.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => { TimeSpan 
          timespan = new TimeSpan(adaptiveSourceStatusUpdate.EndTime); 
        double absvalue  = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero); 
          sliderProgress.Maximum = absvalue; }); 
        

6.  Na kraju metodu **mediaElement_AdaptiveSourceStatusUpdated** , dodajte sljedeći kod:
    
        setSliderStartTime(args.StartTime);
        setSliderEndTime(args.EndTime);

7.  Na kraju metodu **MediaOpened** , dodajte sljedeći kod:
    
    sliderProgress.StepFrequency = SliderFrequency(mediaElement.NaturalDuration.TimeSpan); sliderProgress.Width = mediaElement.Width; setupTimer();

8.  Pritisnite **CTRL + S** da biste spremili datoteku.

**Kompiliranje i testiranje aplikacije**

1. Pritisnite **F6** prikupiti projekta. 
2.  Pritisnite **F5** da biste pokrenuli aplikaciju.
3.  Pri vrhu aplikacije, možete koristiti zadanu Smooth Streaming URL-a ili unesite neku drugu. 
4.  Kliknite **Postavljanje izvora**. 
5.  Testirajte klizača.

Ste dovršili nastave 2.  U ovom nastave dodaje klizač aplikacije. 

##<a name="lesson-3-select-smooth-streaming-streams"></a>Lekciju 3: Odaberite Smooth Streaming strujanja
Gladak strujeće se može strujanje sadržaja s više jezika audiozapisa koji je moguće odabrati po gledatelji.  U ovom nastave ćete omogućiti gledateljima da biste odabrali strujanja. U ovom nastave sadrži sljedeće postupke:

1. Izmjena XAML datoteke
2. Mijenjanje behand kod
3. Kompiliranje i testiranje aplikacije


**Da biste izmijenili XAML datoteke**

1. Iz programa Explorer rješenja, desnom tipkom miša kliknite **MainPage.xaml**, a zatim **Dizajner prikaza**.
2. Pronađite &lt;Grid.RowDefinitions&gt;, i izmjena na RowDefinitions tako da ih izgleda:

        <Grid.RowDefinitions>            
            <RowDefinition Height="20"/>
            <RowDefinition Height="50"/>
            <RowDefinition Height="100"/>
            <RowDefinition Height="80"/>
            <RowDefinition Height="50"/>
        </Grid.RowDefinitions>

3. Unutar na &lt;rešetke&gt;&lt;/Grid&gt; oznake, dodajte sljedeći kod da biste definirali kontrola okvira popisa, tako da korisnici mogu vidjeti popis dostupnih strujanja i odaberite strujanja:

        <Grid Name="gridStreamAndBitrateSelection" Grid.Row="3">
            <Grid.RowDefinitions>
                <RowDefinition Height="300"/>
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="250*"/>
                <ColumnDefinition Width="250*"/>
            </Grid.ColumnDefinitions>
            <StackPanel Name="spStreamSelection" Grid.Row="1" Grid.Column="0">
                <StackPanel Orientation="Horizontal">
                    <TextBlock Name="tbAvailableStreams" Text="Available Streams:" FontSize="16" VerticalAlignment="Center"></TextBlock>
                    <Button Name="btnChangeStreams" Content="Submit" Click="btnChangeStream_Click"/>
                </StackPanel>
                <ListBox x:Name="lbAvailableStreams" Height="200" Width="200" Background="Transparent" HorizontalAlignment="Left" 
                    ScrollViewer.VerticalScrollMode="Enabled" ScrollViewer.VerticalScrollBarVisibility="Visible">
                    <ListBox.ItemTemplate>
                        <DataTemplate>
                            <CheckBox Content="{Binding Name}" IsChecked="{Binding isChecked, Mode=TwoWay}" />
                        </DataTemplate>
                    </ListBox.ItemTemplate>
                </ListBox>
            </StackPanel>
        </Grid>

4. Pritisnite **CTRL + S** da biste spremili promjene.


**Da biste izmijenili kod u pozadini datoteka**

1. Iz programa Explorer rješenja, desnom tipkom miša kliknite **MainPage.xaml**, a zatim **Prikaži kod**.
2. Unutar prostora naziva SSPlayer Dodavanje novog klasa: klase #region strujanje
    
        public class Stream
        {
            private IManifestStream stream;
            public bool isCheckedValue;
            public string name;
    
            public string Name
            {
                get { return name; }
                set { name = value; }
            }
    
            public IManifestStream ManifestStream
            {
                get { return stream; }
                set { stream = value; }
            }
    
            public bool isChecked
            {
                get { return isCheckedValue; }
                set
                {
                    // mMke the video stream always checked.
                    if (stream.Type == MediaStreamType.Video)
                    {
                        isCheckedValue = true;
                    }
                    else
                    {
                        isCheckedValue = value;
                    }
                }
            }
    
            public Stream(IManifestStream streamIn)
            {
                stream = streamIn;
                name = stream.Name;
            }
        }
        #endregion class Stream

3. Na početku klase MainPage dodajte sljedeće varijable definicije:

        private List<Stream> availableStreams;
        private List<Stream> availableAudioStreams;
        private List<Stream> availableTextStreams;
        private List<Stream> availableVideoStreams;

4. Unutar klase MainPage dodajte sljedeće područja:

        #region stream selection
        ///<summary>
        ///Functionality to select streams from IManifestStream available streams
        /// </summary>
        
        // This function is called from the mediaElement_ManifestReady event handler 
        // to retrieve the streams and populate them to the local data members.
        public void getStreams(Manifest manifestObject)
        {
            availableStreams = new List<Stream>();
            availableVideoStreams = new List<Stream>();
            availableAudioStreams = new List<Stream>();
            availableTextStreams = new List<Stream>();
        
            try
            {
                for (int i = 0; i<manifestObject.AvailableStreams.Count; i++)
                {
                    Stream newStream = new Stream(manifestObject.AvailableStreams[i]);
                    newStream.isChecked = false;
        
                    //populate the stream lists based on the types
                    availableStreams.Add(newStream);
        
                    switch (newStream.ManifestStream.Type)
                    {
                        case MediaStreamType.Video:
                            availableVideoStreams.Add(newStream);
                            break;
                        case MediaStreamType.Audio:
                            availableAudioStreams.Add(newStream);
                            break;
                        case MediaStreamType.Text:
                            availableTextStreams.Add(newStream);
                            break;
                    }
        
                    // Select the default selected streams from the manifest.
                    for (int j = 0; j<manifestObject.SelectedStreams.Count; j++)
                    {
                        string selectedStreamName = manifestObject.SelectedStreams[j].Name;
                        if (selectedStreamName.Equals(newStream.Name))
                        {
                            newStream.isChecked = true;
                            break;
                        }
                    }
                }
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
        
        // This function set the list box ItemSource
        private async void refreshAvailableStreamsListBoxItemSource()
        {
            try
            {
                //update the stream check box list on the UI
                await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, ()
                    => { lbAvailableStreams.ItemsSource = availableStreams; });
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
        
        // This function creates a selected streams list
        private void createSelectedStreamsList(List<IManifestStream> selectedStreams)
        {
            bool isOneVideoSelected = false;
            bool isOneAudioSelected = false;
        
            // Only one video stream can be selected
            for (int j = 0; j<availableVideoStreams.Count; j++)
            {
                if (availableVideoStreams[j].isChecked && (!isOneVideoSelected))
                {
                    selectedStreams.Add(availableVideoStreams[j].ManifestStream);
                    isOneVideoSelected = true;
                }
            }
        
            // Select the frist video stream from the list if no video stream is selected
            if (!isOneVideoSelected)
            {
                availableVideoStreams[0].isChecked = true;
                selectedStreams.Add(availableVideoStreams[0].ManifestStream);
            }
        
            // Only one audio stream can be selected
            for (int j = 0; j<availableAudioStreams.Count; j++)
            {
                if (availableAudioStreams[j].isChecked && (!isOneAudioSelected))
                {
                    selectedStreams.Add(availableAudioStreams[j].ManifestStream);
                    isOneAudioSelected = true;
                    txtStatus.Text = "The audio stream is changed to " + availableAudioStreams[j].ManifestStream.Name;
                }
            }
        
            // Select the frist audio stream from the list if no audio steam is selected.
            if (!isOneAudioSelected)
            {
                availableAudioStreams[0].isChecked = true;
                selectedStreams.Add(availableAudioStreams[0].ManifestStream);
            }
        
            // Multiple text streams are supported.
            for (int j = 0; j < availableTextStreams.Count; j++)
            {
                if (availableTextStreams[j].isChecked)
                {
                    selectedStreams.Add(availableTextStreams[j].ManifestStream);
                }
            }
        }
        
        // Change streams on a smooth streaming presentation with multiple video streams.
        private async void changeStreams(List<IManifestStream> selectStreams)
        {
            try
            {
                IReadOnlyList<IStreamChangedResult> returnArgs =
                    await manifestObject.SelectStreamsAsync(selectStreams);
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
        #endregion stream selection

5. Pronađite metodu mediaElement_ManifestReady, dodati sljedeći kod na kraju funkciju:
    
        getStreams(manifestObject);
        refreshAvailableStreamsListBoxItemSource();

    Kada MediaElement manifest spreman, kod može vidjeti popis dostupnih strujanja i popunjava okvir s popisom korisničko Sučelje s popisom.

6. Unutar klase MainPage pronađite korisničkog Sučelja gumbi kliknite područje događaja, a zatim dodajte definiciju sljedeće funkcije:

        private void btnChangeStream_Click(object sender, RoutedEventArgs e)
        {
            List<IManifestStream> selectedStreams = new List<IManifestStream>();

            // Create a list of the selected streams
            createSelectedStreamsList(selectedStreams);

            // Change streams on the presentation
            changeStreams(selectedStreams);
        }

**Kompiliranje i testiranje aplikacije**

1. Pritisnite **F6** prikupiti projekta. 
2.  Pritisnite **F5** da biste pokrenuli aplikaciju.
3.  Pri vrhu aplikacije, možete koristiti zadanu Smooth Streaming URL-a ili unesite neku drugu. 
4.  Kliknite **Postavljanje izvora**. 
5.  Zadani jezik je audio_eng. Pokušajte se prebaciti između audio_eng i audio_es. Svaki put kada, odaberite novi strujanje, mora kliknuti gumb Pošalji.

Ste dovršili nastave 3.  U ovom nastave dodajte funkciju da biste odabrali strujanja.

##<a name="lesson-4-select-smooth-streaming-tracks"></a>Lekciju 4: Odaberite Smooth Streaming zapise
Prezentacije Smooth Streaming mogu sadržavati više videodatoteke šifriranih različite kvalitete razine (brzine prijenosa) i rješenja. U ovom nastave omogućit će korisnicima odaberite prati. U ovom nastave sadrži sljedeće postupke:

1. Izmjena XAML datoteke
2. Mijenjanje behand kod
3. Kompiliranje i testiranje aplikacije

**Da biste izmijenili XAML datoteke**

1. Iz programa Explorer rješenja, desnom tipkom miša kliknite **MainPage.xaml**, a zatim **Dizajner prikaza**.
2. Pronađite na &lt;rešetke&gt; oznaka s nazivom **gridStreamAndBitrateSelection**, dodati sljedeći kod na kraju oznaci:

        <StackPanel Name="spBitRateSelection" Grid.Row="1" Grid.Column="1">
         <StackPanel Orientation="Horizontal">
             <TextBlock Name="tbBitRate" Text="Available Bitrates:" FontSize="16" VerticalAlignment="Center"/>
             <Button Name="btnChangeTracks" Content="Submit" Click="btnChangeTrack_Click" />
         </StackPanel>
         <ListBox x:Name="lbAvailableVideoTracks" Height="200" Width="200" Background="Transparent" HorizontalAlignment="Left" 
                  ScrollViewer.VerticalScrollMode="Enabled" ScrollViewer.VerticalScrollBarVisibility="Visible">
             <ListBox.ItemTemplate>
                 <DataTemplate>
                     <CheckBox Content="{Binding Bitrate}" IsChecked="{Binding isChecked, Mode=TwoWay}"/>
                 </DataTemplate>
             </ListBox.ItemTemplate>
         </ListBox>
        </StackPanel>

3. Pritisnite **CTRL + S** da biste spremili promjene he


**Da biste izmijenili kod u pozadini datoteka**

1. Iz programa Explorer rješenja, desnom tipkom miša kliknite **MainPage.xaml**, a zatim **Prikaži kod**.
2. Unutar prostora naziva SSPlayer dodajte nove klasa:
    
        #region class Track
        public class Track
        {
            private IManifestTrack trackInfo;
            public string _bitrate;
            public bool isCheckedValue;
    
            public IManifestTrack TrackInfo
            {
                get { return trackInfo; }
                set { trackInfo = value; }
            }
    
            public string Bitrate
            {
                get { return _bitrate; }
                set { _bitrate = value; }
            }
    
            public bool isChecked
            {
                get { return isCheckedValue; }
                set
                {
                    isCheckedValue = value;
                }
            }
    
            public Track(IManifestTrack trackInfoIn)
            {
                trackInfo = trackInfoIn;
                _bitrate = trackInfoIn.Bitrate.ToString();
            }
            //public Track() { }
        }
        #endregion class Track

3. Na početku klase MainPage dodajte sljedeće varijable definicije:
    
        private List<Track> availableTracks;

4. Unutar klase MainPage dodajte sljedeće područja:
    
        #region track selection
        /// <summary>
        /// Functionality to select video streams
        /// </summary>

        /// This Function gets the tracks for the selected video stream
        public void getTracks(Manifest manifestObject)
        {
            availableTracks = new List<Track>();

            IManifestStream videoStream = getVideoStream();
            IReadOnlyList<IManifestTrack> availableTracksLocal = videoStream.AvailableTracks;
            IReadOnlyList<IManifestTrack> selectedTracksLocal = videoStream.SelectedTracks;

            try
            {
                for (int i = 0; i < availableTracksLocal.Count; i++)
                {
                    Track thisTrack = new Track(availableTracksLocal[i]);
                    thisTrack.isChecked = true;

                    for (int j = 0; j < selectedTracksLocal.Count; j++)
                    {
                        string selectedTrackName = selectedTracksLocal[j].Bitrate.ToString();
                        if (selectedTrackName.Equals(thisTrack.Bitrate))
                        {
                            thisTrack.isChecked = true;
                            break;
                        }
                    }
                    availableTracks.Add(thisTrack);
                }
            }
            catch (Exception e)
            {
                txtStatus.Text = e.Message;
            }
        }

        // This function gets the video stream that is playing
        private IManifestStream getVideoStream()
        {
            IManifestStream videoStream = null;
            for (int i = 0; i < manifestObject.SelectedStreams.Count; i++)
            {
                if (manifestObject.SelectedStreams[i].Type == MediaStreamType.Video)
                {
                    videoStream = manifestObject.SelectedStreams[i];
                    break;
                }
            }
            return videoStream;
        }

        // This function set the UI list box control ItemSource
        private async void refreshAvailableTracksListBoxItemSource()
        {
            try
            {
                // Update the track check box list on the UI 
                await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, ()
                    => { lbAvailableVideoTracks.ItemsSource = availableTracks; });
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }        
        }

        // This function creates a list of the selected tracks.
        private void createSelectedTracksList(List<IManifestTrack> selectedTracks)
        {
            // Create a list of selected tracks
            for (int j = 0; j < availableTracks.Count; j++)
            {
                if (availableTracks[j].isCheckedValue == true)
                {
                    selectedTracks.Add(availableTracks[j].TrackInfo);
                }
            }
        }

        // This function selects the tracks based on user selection 
        private void changeTracks(List<IManifestTrack> selectedTracks)
        {
            IManifestStream videoStream = getVideoStream();
            try
            {
                videoStream.SelectTracks(selectedTracks);
            }
            catch (Exception ex)
            {
                txtStatus.Text = ex.Message;
            }
        }
        #endregion track selection

5. Pronađite metodu mediaElement_ManifestReady, dodati sljedeći kod na kraju funkciju:

        getTracks(manifestObject);
        refreshAvailableTracksListBoxItemSource();

6. Unutar klase MainPage pronađite korisničkog Sučelja gumbi kliknite područje događaja, a zatim dodajte definiciju sljedeće funkcije:

        private void btnChangeStream_Click(object sender, RoutedEventArgs e)
        {
            List<IManifestStream> selectedStreams = new List<IManifestStream>();

            // Create a list of the selected streams
            createSelectedStreamsList(selectedStreams);

            // Change streams on the presentation
            changeStreams(selectedStreams);
        }

**Kompiliranje i testiranje aplikacije**

1. Pritisnite **F6** prikupiti projekta. 
2.  Pritisnite **F5** da biste pokrenuli aplikaciju.
3.  Pri vrhu aplikacije, možete koristiti zadanu Smooth Streaming URL-a ili unesite neku drugu. 
4.  Kliknite **Postavljanje izvora**. 
5.  Prema zadanim postavkama odabrani su svi zapisi strujanje videozapisa. Eksperimentiranje stopa promjene bitne, možete odabrati najniže brzinu prijenosa dostupne i odaberite najveće brzinu prijenosa dostupna. Nakon svake promjene, kliknite Pošalji.  Vidjet ćete promjene kvalitetu videozapisa.

Ste dovršili nastave 4.  U ovom nastave dodajte funkciju da biste odabrali zapise.


##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="other-resources"></a>Ostali resursi:
- [Sastavljanje Smooth Streaming Windows 8 JavaScript aplikacijom napredne značajke](http://blogs.iis.net/cenkd/archive/2012/08/10/how-to-build-a-smooth-streaming-windows-8-javascript-application-with-advanced-features.aspx)
- [Gladak strujanje Tehnički pregled](http://www.iis.net/learn/media/on-demand-smooth-streaming/smooth-streaming-technical-overview)

[PlayerApplication]: ./media/media-services-build-smooth-streaming-apps/SSClientWin8-1.png
[CodeViewPic]: ./media/media-services-build-smooth-streaming-apps/SSClientWin8-2.png
 
