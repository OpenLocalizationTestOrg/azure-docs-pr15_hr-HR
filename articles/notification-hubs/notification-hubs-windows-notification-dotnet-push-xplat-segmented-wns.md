<properties
    pageTitle="Koristite koncentratora obavijesti da biste poslali važnih vijesti (Windows Universal)"
    description="Korištenje Azure obavijesti koncentratora s oznakama u Registracija da biste poslali važnih vijesti univerzalni aplikacija u sustavu Windows."
    services="notification-hubs"
    documentationCenter="windows"
    authors="ysxu"
    manager="erikre"
    editor=""/>


<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="use-notification-hubs-to-send-breaking-news"></a>Korištenje koncentratora obavijesti da biste poslali važnih vijesti


[AZURE.INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]


##<a name="overview"></a>Pregled

U ovoj se temi objašnjava da biste koristili Azure obavijesti koncentratora za emitiranje prijelom novosti obavijesti u Windows trgovini ili aplikacija za Windows Phone 8.1 (koji nisu Silverlight). Ako birate Windows Phone 8.1 Silverlight, pogledajte verzija za [Windows Phone](notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md) . Po dovršetku će moći Registrirajte se za najnovije vijesti kategorije koje vas zanimaju te primati samo slanje obavijesti za te kategorije. Taj se scenarij uobičajeni obrazac za mnoge aplikacije gdje ste obavijesti slati grupe korisnika koji su prethodno deklarirane kamata u njima, npr RSS reader, proizvod tvrtke aplikacije za sporta glazbe i tako dalje. 

Scenariji za emitiranje omogućene su uključivanjem jednu ili više _oznake_ prilikom stvaranja je registracija u središtu obavijesti. Kada se šalju obavijesti o oznaci, sve uređaje koje ste registrirali za oznaku će primiti obavijest. Budući da oznake jednostavno nizovi, ne moraju unaprijed dodjeli. Dodatne informacije o oznake, pogledajte [usmjeravanje koncentratora obavijesti i oznaka izraza](notification-hubs-tags-segment-push-message.md).

##<a name="prerequisites"></a>Preduvjeti

U ovoj se temi sastavlja u aplikaciju koju ste stvorili u [Početak rada s obavijesti koncentratora][get-started]. Prije pokretanja ovog praktičnog vodiča, morate već dovršite [Početak rada s obavijesti koncentratora][get-started].

##<a name="add-category-selection-to-the-app"></a>Dodavanje kategorija odabira u aplikaciju

U prvi je korak da biste dodali elemente korisničkog Sučelja na postojeću glavnu stranicu korisniku omogućuju odaberite kategorije da biste registrirali. Kategorije koji je odabrao korisnik pohranjuju se na uređaj. Prilikom pokretanja aplikacije Registracija uređaja se stvara u koncentratora za obavijesti s odabrane kategorije kao oznake.

1. Otvorite datoteku MainPage.xaml projekta, a zatim kopirajte sljedeći kod u elementu **rešetke** :

        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition/>
                <ColumnDefinition/>
            </Grid.ColumnDefinitions>
            <TextBlock Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"  TextWrapping="Wrap" Text="Breaking News" FontSize="42" VerticalAlignment="Top" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="World" Name="WorldToggle" Grid.Row="1" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Politics" Name="PoliticsToggle" Grid.Row="2" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Business" Name="BusinessToggle" Grid.Row="3" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Technology" Name="TechnologyToggle" Grid.Row="1" Grid.Column="1" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Science" Name="ScienceToggle" Grid.Row="2" Grid.Column="1" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Sports" Name="SportsToggle" Grid.Row="3" Grid.Column="1" HorizontalAlignment="Center"/>
            <Button Name="SubscribeButton" Content="Subscribe" HorizontalAlignment="Center" Grid.Row="4" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click"/>
        </Grid>


2. Desnom tipkom miša kliknite projekt **zajednički se koristi** i dodajte nove predmete pod nazivom **obavijesti** **javno** mijenjanje dodati definiciju predmete, a zatim dodajte sljedeće naredbe **pomoću** nove datotekom kod:

        using Windows.Networking.PushNotifications;
        using Microsoft.WindowsAzure.Messaging;
        using Windows.Storage;
        using System.Threading.Tasks;

3. Kopirajte sljedeći kod u novi predmet **obavijesti** :

        private NotificationHub hub;

        public Notifications(string hubName, string listenConnectionString)
        {
            hub = new NotificationHub(hubName, listenConnectionString);
        }

        public async Task<Registration> StoreCategoriesAndSubscribe(IEnumerable<string> categories)
        {
            ApplicationData.Current.LocalSettings.Values["categories"] = string.Join(",", categories);
            return await SubscribeToCategories(categories);
        }

        public IEnumerable<string> RetrieveCategories()
        {
            var categories = (string) ApplicationData.Current.LocalSettings.Values["categories"];
            return categories != null ? categories.Split(','): new string[0];
        }

        public async Task<Registration> SubscribeToCategories(IEnumerable<string> categories = null)
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            if (categories == null)
            {
                categories = RetrieveCategories();
            }

            // Using a template registration to support notifications across platforms.
            // Any template notifications that contain messageParam and a corresponding tag expression
            // will be delivered for this registration.

            const string templateBodyWNS = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";

            return await hub.RegisterTemplateAsync(channel.Uri, templateBodyWNS, "simpleWNSTemplateExample",
                    categories);
        }

    Klase koristi lokalno spremište za pohranu kategorije novosti koja sadrži ovaj uređaj da biste primali. Imajte na umu da umjesto pozivanje metodu *RegisterNativeAsync* ćemo poziva *RegisterTemplateAsync* da biste registrirali za kategorije pomoću predloška registracije. 
    
    Ne možemo i navedite naziv predloška ("simpleWNSTemplateExample"), jer želimo registrirati više od jednog predloška (na primjer jedan za skočnoj obavijesti), a jedan za pločice, a moramo dodijelite naziv da biste mogli ažurirati ili ih izbrisati.

    Napomena Ako uređaj registrira većeg broja predložaka s istu oznaku, na dolazne poruke skupine koji oznaka rezultirati više obavijesti isporučivati uređaj (jedan za svaki predložak). Takvo ponašanje koristan je kada se ista poruka logičke ne može rezultirati više vizualne obavijesti, na primjer prikazuje značku i na skočnu u aplikaciji iz Windows trgovine.

    Dodatne informacije o predlošcima potražite u članku [Predlošci](notification-hubs-templates-cross-platform-push-messages.md).




4. U datoteci App.xaml.cs projekt, dodajte sljedeće svojstvo predmete **aplikacije** :

        public Notifications notifications = new Notifications("<hub name>", "<connection string with listen access>");

    Ovo svojstvo se koristi za stvaranje i pristup **obavijesti** instance.

    U gornjem kod zamijenite na `<hub name>` i `<connection string with listen access>` rezerviranih mjesta s naziva koncentrator obavijesti i niz za povezivanje za *DefaultListenSharedAccessSignature* koji ste nabavili neke starije verzije.

    > [AZURE.NOTE] Budući da su vjerodajnice koje su distributed s klijentskom aplikacijom obično sigurne, samo distribucija ključ za pristup preslušavanja s aplikacijom klijenta. Poslušajte omogućuje pristup ne može se mijenjati aplikaciju za registraciju na obavijesti, ali postojeće registracije i ne može se obavijesti poslati. Puni pristup ključ se koristi u zaštićenim pozadinskog servisa za slanje obavijesti i promjene postojećih registracije.

5. U MainPage.xaml.cs, dodajte sljedeći redak:

        using Windows.UI.Popups;

6. U datoteci MainPage.xaml.cs projekt, dodajte na sljedeći način:

        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
            var categories = new HashSet<string>();
            if (WorldToggle.IsOn) categories.Add("World");
            if (PoliticsToggle.IsOn) categories.Add("Politics");
            if (BusinessToggle.IsOn) categories.Add("Business");
            if (TechnologyToggle.IsOn) categories.Add("Technology");
            if (ScienceToggle.IsOn) categories.Add("Science");
            if (SportsToggle.IsOn) categories.Add("Sports");

            var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(categories);

            var dialog = new MessageDialog("Subscribed to: " + string.Join(",", categories) + " on registration Id: " + result.RegistrationId);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }

    Ovaj postupak stvara popis kategorija i koristi predmete **obavijesti** za pohranu na popisu u lokalno spremište i registrirati odgovarajuće oznake putem koncentratora za obavijesti. Kada se mijenjaju kategorija, Registracija je ponovno stvoriti pomoću novih kategorija.

Aplikacija je moći pohranu skup kategorija u lokalno spremište na uređaju te morate registrirati središtu obavijesti svaki put kada korisnik promijeni odabira kategorije.

##<a name="register-for-notifications"></a>Registrirajte se za obavijesti

Ove korake registrirati putem središta obavijesti na pokretanja pomoću konfiguracije kategorije koje su pohranjeni u lokalno spremište.

> [AZURE.NOTE] Budući da kanala URI dodijeljeni tako da na Windows obavijesti servisa (WNS) možete promijeniti u bilo kojem trenutku, trebala bi se registrirati za obavijesti često da biste izbjegli pogreške obavijesti. U ovom se primjeru registrira za obavijesti prilikom svakog pokretanja aplikacije. Za aplikacije koje se često više puta dan, vjerojatno preskočite Registracija da biste sačuvali propusnosti Ako manji od jednog dana prošlo od prethodnog registracije.

1. Otvorite datoteku App.xaml.cs i ažuriranje metodu **InitNotificationsAsync** na `notifications` predmete da biste se pretplatili na temelju kategorija.

        // *** Remove or comment out these lines *** 
        //var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
        //var hub = new NotificationHub("your hub name", "your listen connection string");
        //var result = await hub.RegisterNativeAsync(channel.Uri);
    
        var result = await notifications.SubscribeToCategories();

    To jamči da prilikom svakog pokretanja aplikacije ga dohvaća kategorija iz lokalno spremište i zahtjeve registeration za te kategorije. Način **InitNotificationsAsync** stvoren kao dio [Početak rada s obavijesti koncentratora] [ get-started] vodič.

3. U datoteci MainPage.xaml.cs projekt, dodajte sljedeći kod *OnNavigatedTo* način:

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
            var categories = ((App)Application.Current).notifications.RetrieveCategories();

            if (categories.Contains("World")) WorldToggle.IsOn = true;
            if (categories.Contains("Politics")) PoliticsToggle.IsOn = true;
            if (categories.Contains("Business")) BusinessToggle.IsOn = true;
            if (categories.Contains("Technology")) TechnologyToggle.IsOn = true;
            if (categories.Contains("Science")) ScienceToggle.IsOn = true;
            if (categories.Contains("Sports")) SportsToggle.IsOn = true;
        }

    Time će se obnoviti na glavnu stranicu na temelju status prethodno spremljene kategorije.

Aplikacija je dovršen i možete spremiti skup kategorije u lokalno spremište uređaj koristi za registriranje središtu obavijesti svaki put kada korisnik promijeni odabira kategorije. Zatim ćemo definirati pozadinskog koji mogu slati obavijesti kategorija za aplikaciju.

##<a name="sending-tagged-notifications"></a>Slanje obavijesti označenog

[AZURE.INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

##<a name="run-the-app-and-generate-notifications"></a>Pokrenite aplikaciju i generiranje obavijesti

1. U Visual Studio, pritisnite F5 Kompiliranje i pokrenite aplikaciju.

    ![][1]

    Napomena da aplikaciju korisničkog Sučelja nudi skup preklapa koji vam omogućuje odaberite kategorije da biste se pretplatili.

2. Omogućivanje jednu ili više kategorija preklapa, a zatim kliknite **Pretplata**.

    Aplikaciju pretvara odabrane kategorije u oznake i zahtjeve novi Registracija uređaja za odabrane oznake u središtu obavijesti. Registrirani kategorije su vraća i prikazana u dijaloškom okviru.

    ![][19]

4. Poslati obavijest o novoj pozadinski jedan od sljedećih načina:

    + **Konzole aplikacije:** pokrenite aplikaciju konzolu.

    + **Java/PHP:** pokrenuti aplikaciju/skriptu.

    Obavijesti za odabrane kategorije prikazuju se kao skočnoj obavijesti.

    ![][14]

##<a name="next-steps"></a>Daljnji koraci

U ovom ćete praktičnom vodiču smo naučili kako emitiranje važnih vijesti po kategoriji. Imajte na umu dovršavanje neku od sljedeće vodiči za kojima se ističu druge napredne scenarije koncentratora obavijesti:

+ [Korištenje obavijesti koncentratora za emitiranje lokalizirane važnih vijesti]

    Saznajte kako da biste proširili aplikaciju otpornosti novosti da biste omogućili slanje lokalizirane obavijesti.



<!-- Anchors. -->
[Add category selection to the app]: #adding-categories
[Register for notifications]: #register
[Send notifications from your back-end]: #send
[Run the app and generate notifications]: #test-app
[Next Steps]: #next-steps

<!-- Images. -->
[1]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-breakingnews-win1.png

[14]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-windows-toast-2.png


[19]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-windows-reg-2.png

<!-- URLs.-->
[get-started]: /manage/services/notification-hubs/getting-started-windows-dotnet/
[Korištenje obavijesti koncentratora za emitiranje lokalizirane važnih vijesti]: /manage/services/notification-hubs/breaking-news-localized-dotnet/
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users
[Mobile Service]: /develop/mobile/tutorials/get-started/
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
