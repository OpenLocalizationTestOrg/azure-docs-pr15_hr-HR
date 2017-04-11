<properties
    pageTitle="Korištenje koncentratora obavijesti da biste poslali važnih vijesti (Windows Phone)"
    description="Koristiti Azure obavijesti koncentratora oznaka u registracije za slanje važnih vijesti u aplikaciji Windows Phone."
    services="notification-hubs"
    documentationCenter="windows"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

# <a name="use-notification-hubs-to-send-breaking-news"></a>Korištenje koncentratora obavijesti da biste poslali važnih vijesti

[AZURE.INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]


##<a name="overview"></a>Pregled

U ovoj se temi objašnjava da biste koristili Azure obavijesti koncentratora za emitiranje prijelom novosti obavijesti za Windows Phone 8.0/8.1 Silverlight aplikacije. Ako birate iz Windows trgovine ili aplikacija za Windows Phone 8.1, pogledajte [Windows univerzalni](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md) verziju. Po dovršetku će moći Registrirajte se za najnovije vijesti kategorije koje vas zanimaju te primati samo slanje obavijesti za te kategorije. Taj se scenarij uobičajeni obrazac za mnoge aplikacije gdje se obavijesti imaju slati grupe korisnika koje ste prethodno deklarirane kamate na njima, npr RSS čitač, aplikacije za sporta glazbu, itd.

Scenariji za emitiranje omogućeni uključivanjem jednu ili više _oznaka_ prilikom stvaranja je registracija u središtu obavijesti. Kada se šalju obavijesti o oznaci, sve uređaje koje ste registrirali za oznaku će primiti obavijest. Budući da oznake jednostavno nizovi, ne moraju unaprijed dodjeli. Dodatne informacije o oznake, pogledajte [usmjeravanje koncentratora obavijesti i oznaka izraza](notification-hubs-tags-segment-push-message.md).

##<a name="prerequisites"></a>Preduvjeti

U ovoj se temi sastavlja u aplikaciju koju ste stvorili u [Početak rada s koncentratorima obavijesti]. Prije pokretanja ovog praktičnog vodiča, morate već dovršite [Početak rada s koncentratorima obavijesti].

##<a name="add-category-selection-to-the-app"></a>Dodavanje kategorija odabira u aplikaciju

U prvi je korak da biste dodali elemente korisničkog Sučelja na postojeću glavnu stranicu korisniku omogućuju odaberite kategorije da biste registrirali. Kategorije koji je odabrao korisnik pohranjuju se na uređaj. Prilikom pokretanja aplikacije Registracija uređaja se stvara u koncentratora za obavijesti s odabrane kategorije kao oznake.

1. Otvorite datoteku MainPage.xaml projekt, a zatim Zamijeni elemenata **rešetke** pod nazivom `TitlePanel` i `ContentPanel` s sljedeći kod:

        <StackPanel x:Name="TitlePanel" Grid.Row="0" Margin="12,17,0,28">
            <TextBlock Text="Breaking News" Style="{StaticResource PhoneTextNormalStyle}" Margin="12,0"/>
            <TextBlock Text="Categories" Margin="9,-7,0,0" Style="{StaticResource PhoneTextTitle1Style}"/>
        </StackPanel>

        <Grid Name="ContentPanel" Grid.Row="1" Margin="12,0,12,0">
            <Grid.RowDefinitions>
                <RowDefinition Height="auto"/>
                <RowDefinition Height="auto" />
                <RowDefinition Height="auto" />
                <RowDefinition Height="auto" />
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition />
                <ColumnDefinition />
            </Grid.ColumnDefinitions>
            <CheckBox Name="WorldCheckBox" Grid.Row="0" Grid.Column="0">World</CheckBox>
            <CheckBox Name="PoliticsCheckBox" Grid.Row="1" Grid.Column="0">Politics</CheckBox>
            <CheckBox Name="BusinessCheckBox" Grid.Row="2" Grid.Column="0">Business</CheckBox>
            <CheckBox Name="TechnologyCheckBox" Grid.Row="0" Grid.Column="1">Technology</CheckBox>
            <CheckBox Name="ScienceCheckBox" Grid.Row="1" Grid.Column="1">Science</CheckBox>
            <CheckBox Name="SportsCheckBox" Grid.Row="2" Grid.Column="1">Sports</CheckBox>
            <Button Name="SubscribeButton" Content="Subscribe" HorizontalAlignment="Center" Grid.Row="3" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click" />
        </Grid>

2. U programu project, stvorite novi klase pod nazivom **obavijesti**, dodajte **javno** mijenjanje definiciju klase, a zatim dodajte sljedeće naredbe **pomoću** nove datotekom kod:

        using Microsoft.Phone.Notification;
        using Microsoft.WindowsAzure.Messaging;
        using System.IO.IsolatedStorage;
        using System.Windows;

3. Kopirajte sljedeći kod u novi predmet **obavijesti** :

        private NotificationHub hub;

        // Registration task to complete registration in the ChannelUriUpdated event handler
        private TaskCompletionSource<Registration> registrationTask;

        public Notifications(string hubName, string listenConnectionString)
        {
            hub = new NotificationHub(hubName, listenConnectionString);
        }

        public IEnumerable<string> RetrieveCategories()
        {
            var categories = (string)IsolatedStorageSettings.ApplicationSettings["categories"];
            return categories != null ? categories.Split(',') : new string[0];
        }

        public async Task<Registration> StoreCategoriesAndSubscribe(IEnumerable<string> categories)
        {
            var categoriesAsString = string.Join(",", categories);
            var settings = IsolatedStorageSettings.ApplicationSettings;
            if (!settings.Contains("categories"))
            {
                settings.Add("categories", categoriesAsString);
            }
            else
            {
                settings["categories"] = categoriesAsString;
            }
            settings.Save();

            return await SubscribeToCategories();
        }

        public async Task<Registration> SubscribeToCategories()
        {
            registrationTask = new TaskCompletionSource<Registration>();

            var channel = HttpNotificationChannel.Find("MyPushChannel");

            if (channel == null)
            {
                channel = new HttpNotificationChannel("MyPushChannel");
                channel.Open();
                channel.BindToShellToast();
                channel.ChannelUriUpdated += channel_ChannelUriUpdated;

                // This is optional, used to receive notifications while the app is running.
                channel.ShellToastNotificationReceived += channel_ShellToastNotificationReceived;
            }

            // If channel.ChannelUri is not null, we will complete the registrationTask here.  
            // If it is null, the registrationTask will be completed in the ChannelUriUpdated event handler.
            if (channel.ChannelUri != null)
            {
                await RegisterTemplate(channel.ChannelUri);
            }
            
            return await registrationTask.Task;
        }

        async void channel_ChannelUriUpdated(object sender, NotificationChannelUriEventArgs e)
        {
            await RegisterTemplate(e.ChannelUri);
        }

        async Task<Registration> RegisterTemplate(Uri channelUri)
        {
            // Using a template registration to support notifications across platforms.
            // Any template notifications that contain messageParam and a corresponding tag expression
            // will be delivered for this registration.

            const string templateBodyMPNS = "<wp:Notification xmlns:wp=\"WPNotification\">" +
                                                "<wp:Toast>" +
                                                    "<wp:Text1>$(messageParam)</wp:Text1>" +
                                                "</wp:Toast>" +
                                            "</wp:Notification>";

            // The stored categories tags are passed with the template registration.

            registrationTask.SetResult(await hub.RegisterTemplateAsync(channelUri.ToString(), 
                templateBodyMPNS, "simpleMPNSTemplateExample", this.RetrieveCategories()));

            return await registrationTask.Task;
        }

        // This is optional. It is used to receive notifications while the app is running.
        void channel_ShellToastNotificationReceived(object sender, NotificationEventArgs e)
        {
            StringBuilder message = new StringBuilder();
            string relativeUri = string.Empty;

            message.AppendFormat("Received Toast {0}:\n", DateTime.Now.ToShortTimeString());

            // Parse out the information that was part of the message.
            foreach (string key in e.Collection.Keys)
            {
                message.AppendFormat("{0}: {1}\n", key, e.Collection[key]);

                if (string.Compare(
                    key,
                    "wp:Param",
                    System.Globalization.CultureInfo.InvariantCulture,
                    System.Globalization.CompareOptions.IgnoreCase) == 0)
                {
                    relativeUri = e.Collection[key];
                }
            }

            // Display a dialog of all the fields in the toast.
            System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() => 
            { 
                MessageBox.Show(message.ToString()); 
            });
        }


    Klase koristi Izolirani prostora za pohranu za pohranu kategorije novosti koji se nalazi ovaj uređaj da biste primali. Sadrži i metode da biste registrirali za te kategorije pomoću registraciju za obavijesti [predložak](notification-hubs-templates-cross-platform-push-messages.md) .


4. U datoteci App.xaml.cs projekta, dodajte sljedeće svojstvo za predmete **aplikacije** . Zamjena na `<hub name>` i `<connection string with listen access>` rezerviranih mjesta s naziva koncentrator obavijesti i niz za povezivanje za *DefaultListenSharedAccessSignature* koji ste nabavili neke starije verzije.

        public Notifications notifications = new Notifications("<hub name>", "<connection string with listen access>");

    > [AZURE.NOTE] Budući da su vjerodajnice koje su distributed s klijentskom aplikacijom obično sigurne, samo distribucija ključ za pristup preslušavanja s aplikacijom klijenta. Poslušajte omogućuje pristup ne može se mijenjati aplikaciju za registraciju na obavijesti, ali postojeće registracije i ne može se obavijesti poslati. Puni pristup ključ se koristi u zaštićenim pozadinskog servisa za slanje obavijesti i promjene postojećih registracije.

5. U MainPage.xaml.cs, dodajte sljedeći redak:

        using Windows.UI.Popups;

6. U datoteci MainPage.xaml.cs projekt, dodajte na sljedeći način:

        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
          var categories = new HashSet<string>();
          if (WorldCheckBox.IsChecked == true) categories.Add("World");
          if (PoliticsCheckBox.IsChecked == true) categories.Add("Politics");
          if (BusinessCheckBox.IsChecked == true) categories.Add("Business");
          if (TechnologyCheckBox.IsChecked == true) categories.Add("Technology");
          if (ScienceCheckBox.IsChecked == true) categories.Add("Science");
          if (SportsCheckBox.IsChecked == true) categories.Add("Sports");
    
          var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(categories);
    
          MessageBox.Show("Subscribed to: " + string.Join(",", categories) + " on registration id : " +
             result.RegistrationId);
        }

    Ovaj postupak stvara popis kategorija i koristi predmete **obavijesti** za pohranu na popisu u lokalno spremište i registrirati odgovarajuće oznake putem koncentratora za obavijesti. Kada se mijenjaju se kategorije, Registracija je ponovno stvoriti pomoću novih kategorija.

Aplikacija je moći pohranu skup kategorije u lokalno spremište na uređaju te morate registrirati središtu obavijesti svaki put kada korisnik promijeni odabira kategorije.

##<a name="register-for-notifications"></a>Registrirajte se za obavijesti

Ove korake registrirati putem središta obavijesti na pokretanja pomoću konfiguracije kategorije koje su pohranjeni u lokalno spremište.

> [AZURE.NOTE] Budući da kanala URI dodijeljeni tako da na Microsoft automatske obavijesti servisa (MPNS) možete promijeniti u bilo kojem trenutku, trebala bi se registrirati za obavijesti često da biste izbjegli pogreške obavijesti. U ovom se primjeru registrira za obavijesti prilikom svakog pokretanja aplikacije. Za aplikacije koje se često više puta dan, vjerojatno preskočite Registracija da biste očuvali propusnost Ako manji od jednog dana prošlo od prethodnog registracije.


1. Otvorite datoteku App.xaml.cs dodajte **asinkrone** pritisku na **Application_Launching** način i zamjena koncentratora obavijesti Registracija kod koji ste dodali u [Početak rada s koncentratorima obavijesti] s sljedeći kod:

        private async void Application_Launching(object sender, LaunchingEventArgs e)
        {
            var result = await notifications.SubscribeToCategories();

            if (result != null)
                System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() =>
                {
                    MessageBox.Show("Registration Id :" + result.RegistrationId, "Registered", MessageBoxButton.OK);
                });
        }

    To jamči da prilikom svakog pokretanja aplikacije ga dohvaća kategorija iz lokalno spremište i zahtjeva za registraciju za te kategorije.

2. U datoteci MainPage.xaml.cs projekt, dodajte sljedeći kod koji implementira **OnNavigatedTo** metoda:

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
            var categories = ((App)Application.Current).notifications.RetrieveCategories();

            if (categories.Contains("World")) WorldCheckBox.IsChecked = true;
            if (categories.Contains("Politics")) PoliticsCheckBox.IsChecked = true;
            if (categories.Contains("Business")) BusinessCheckBox.IsChecked = true;
            if (categories.Contains("Technology")) TechnologyCheckBox.IsChecked = true;
            if (categories.Contains("Science")) ScienceCheckBox.IsChecked = true;
            if (categories.Contains("Sports")) SportsCheckBox.IsChecked = true;
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

    ![][2]

3. Nakon što primite potvrdu da vaše kategorije su pretplate dovršen, pokrenite aplikaciju konzole za slanje obavijesti za svake kategorije. Provjerite je li samo dobit ćete obavijest za kategorije ste se pretplatili.

    ![][3]

U ovoj se temi ste završili rad.

<!--##Next steps

In this tutorial we learned how to broadcast breaking news by category. Consider completing one of the following tutorials that highlight other advanced Notification Hubs scenarios:

+ [Use Notification Hubs to broadcast localized breaking news]

    Learn how to expand the breaking news app to enable sending localized notifications.

+ [Notify users with Notification Hubs]

    Learn how to push notifications to specific authenticated users. This is a good solution for sending notifications only to specific users.
-->

<!-- Anchors. -->
[Add category selection to the app]: #adding-categories
[Register for notifications]: #register
[Send notifications from your back-end]: #send
[Run the app and generate notifications]: #test-app
[Next Steps]: #next-steps

<!-- Images. -->
[1]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-breakingnews.png
[2]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-registration.png
[3]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-toast.png



<!-- URLs.-->
[Početak rada s obavijesti koncentratora]: /manage/services/notification-hubs/get-started-notification-hubs-wp8/
[Use Notification Hubs to broadcast localized breaking news]: ../breakingnews-localized-wp8.md
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users/
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Phone]: ??

