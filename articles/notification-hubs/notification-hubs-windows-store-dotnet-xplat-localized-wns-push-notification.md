<properties
    pageTitle="Obavijest o koncentratora lokalizirani prijelom novosti vodič"
    description="Saznajte kako koristiti Azure obavijesti koncentratora za slanje obavijesti o novostima lokalizirani prijelom."
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

# <a name="use-notification-hubs-to-send-localized-breaking-news"></a>Korištenje koncentratora obavijesti da biste poslali lokalizirane važnih vijesti

> [AZURE.SELECTOR]
- [C# iz Windows trgovine](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md)
- [iOS](notification-hubs-ios-xplat-localized-apns-push-notification.md)

##<a name="overview"></a>Pregled

U ovoj se temi objašnjava pomoću značajke **predloška** programa Azure obavijesti koncentratora za emitiranje prijelom novosti obavijesti koji ste je lokalizirani jezika i uređaja. U ovom ćete praktičnom vodiču počet ćete aplikacija iz Windows trgovine stvorene u [Koristi koncentratora obavijesti da biste poslali važnih vijesti]. Kada završi, moći ćete se registrirati za kategorije koje vas zanimaju, navedite jezik u kojem želite primati obavijesti i primati samo proslijeđenih obavijesti za odabrane kategorije na tom jeziku.


Postoje dva dijela scenarij:

- u aplikaciji Windows Store omogućuje klijent uređaje da biste odredili jezik, a da biste se pretplatili kategorije novosti različite otpornosti;

- pozadinske odašilje obavijesti, korištenje **oznaka** i **predložak** feautres od koncentratora Azure obavijesti.



##<a name="prerequisites"></a>Preduvjeti

Mora već dovršite vodič za [Korištenje koncentratora obavijesti da biste poslali važnih vijesti] , a kod dostupna, jer ovog praktičnog vodiča nadovezuje izravno kod.

Morate Visual Studio 2012 ili noviji.


##<a name="template-concepts"></a>Predložak koncepti

U [Korištenje koncentratora obavijesti da biste poslali važnih vijesti] ugrađeni aplikaciju koja koriste **oznake** Pretplaćivanje na obavijesti za kategorije različitih novosti.
Mnoge aplikacije, međutim, ciljani više tržištima i potrebna lokalizaciju. To znači da se sadržaj obavijesti sami morati lokalizirani i isporučuju pravilan skup uređaja.
U ovoj temi Pokazat ćemo kako koristiti **predložak** značajka obavijesti koncentratora jednostavno izlaganje obavijesti o novostima lokalizirane prijelom.

Napomena: jedan za slanje obavijesti lokalizirane tako da biste stvorili više verzija sustava svakoj oznaci. Ako, primjerice, za podršku engleski, francuski i Mandarinski, ne možemo potrebni tri različite oznake svijeta novosti: "world_en", "world_fr" i "world_ch". Zatim ćemo promijenile slanje lokalizirane verzije novosti svijeta za svaki od tih oznaka. U ovoj temi koristimo predlošci da biste izbjegli proliferation oznake i obavezne slanja više poruka.

Visoke razine Predlošci su način da biste odredili kako određeni uređaj trebale primiti obavijest. Predložak određuje oblik točno tereta tako da upućuju na svojstva koja su dio poruku poslao vaše aplikacije pozadinskih. U našem slučaju smo poslat će regionalnu shemu agnostic poruku koja sadrži sve jezike:

    {
        "News_English": "...",
        "News_French": "...",
        "News_Mandarin": "..."
    }

Zatim ćemo će osigurati uređaji registrirate predložak koji se odnosi na odgovarajuće svojstvo. Na primjer, aplikacija iz Windows trgovine koji želi poruka jednostavne skočnoj će Registrirajte se za sljedeći predložak sve odgovarajuće oznakama:

    <toast>
      <visual>
        <binding template=\"ToastText01\">
          <text id=\"1\">$(News_English)</text>
        </binding>
      </visual>
    </toast>



Predlošci su vrlo napredne značajke mogu saznati više o u našem članku [Predlošci](notification-hubs-templates-cross-platform-push-messages.md) . 


##<a name="the-app-user-interface"></a>Korisničkom sučelju aplikacije

Ne možemo sada izmijeniti najnovije vijesti aplikaciju koju ste stvorili u članku [Korištenje koncentratora obavijesti da biste poslali važnih vijesti] da biste poslali lokalizirane važnih vijesti pomoću predložaka.

U aplikaciji Windows Store:

Promjena vaše MainPage.xaml da biste uključili kombinirani okvir regionalne sheme:

    <Grid Margin="120, 58, 120, 80"  
            Background="{StaticResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition />
            <ColumnDefinition />
        </Grid.ColumnDefinitions>
        <TextBlock Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"  TextWrapping="Wrap" Text="Breaking News" FontSize="42" VerticalAlignment="Top"/>
        <ComboBox Name="Locale" HorizontalAlignment="Left" VerticalAlignment="Center" Width="200" Grid.Row="1" Grid.Column="0">
            <x:String>English</x:String>
            <x:String>French</x:String>
            <x:String>Mandarin</x:String>
        </ComboBox>
        <ToggleSwitch Header="World" Name="WorldToggle" Grid.Row="2" Grid.Column="0"/>
        <ToggleSwitch Header="Politics" Name="PoliticsToggle" Grid.Row="3" Grid.Column="0"/>
        <ToggleSwitch Header="Business" Name="BusinessToggle" Grid.Row="4" Grid.Column="0"/>
        <ToggleSwitch Header="Technology" Name="TechnologyToggle" Grid.Row="2" Grid.Column="1"/>
        <ToggleSwitch Header="Science" Name="ScienceToggle" Grid.Row="3" Grid.Column="1"/>
        <ToggleSwitch Header="Sports" Name="SportsToggle" Grid.Row="4" Grid.Column="1"/>
        <Button Content="Subscribe" HorizontalAlignment="Center" Grid.Row="5" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click" />
    </Grid>

##<a name="building-the-windows-store-client-app"></a>Stvaranje klijentskih aplikacija iz Windows trgovine

1. U svojoj učionici obavijesti o načinima *StoreCategoriesAndSubscribe* i *SubscribeToCateories* dodajte parametar regionalne postavke.

        public async Task<Registration> StoreCategoriesAndSubscribe(string locale, IEnumerable<string> categories)
        {
            ApplicationData.Current.LocalSettings.Values["categories"] = string.Join(",", categories);
            ApplicationData.Current.LocalSettings.Values["locale"] = locale;
            return await SubscribeToCategories(categories);
        }

        public async Task<Registration> SubscribeToCategories(string locale, IEnumerable<string> categories = null)
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            if (categories == null)
            {
                categories = RetrieveCategories();
            }

            // Using a template registration. This makes supporting notifications across other platforms much easier.
            // Using the localized tags based on locale selected.
            string templateBodyWNS = String.Format("<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(News_{0})</text></binding></visual></toast>", locale);

            return await hub.RegisterTemplateAsync(channel.Uri, templateBodyWNS, "localizedWNSTemplateExample", categories);
        }

    Imajte na umu da umjesto pozivanje metodu *RegisterNativeAsync* ćemo poziva *RegisterTemplateAsync*: ne možemo registriranja određene obavijesti oblik u kojem se predložak ovisi o regionalnoj shemi. Ne možemo i navedite naziv predloška ("localizedWNSTemplateExample"), jer želimo registrirati više od jednog predloška (na primjer jedan za skočnoj obavijesti), a jedan za pločice, a moramo dodijelite naziv da biste mogli ažurirati ili ih izbrisati.

    Napomena Ako uređaj registrira većeg broja predložaka s istu oznaku, na dolazne poruke skupine koji oznaka rezultirati više obavijesti isporučivati uređaj (jedan za svaki predložak). Takvo ponašanje koristan je kada se ista poruka logičke ne može rezultirati više vizualne obavijesti, na primjer prikazuje značku i na skočnu u aplikaciji iz Windows trgovine.

2. Dodajte sljedeće način za dohvaćanje pohranjene regionalne sheme:

        public string RetrieveLocale()
        {
            var locale = (string) ApplicationData.Current.LocalSettings.Values["locale"];
            return locale != null ? locale : "English";
        }

3. U MainPage.xaml.cs, ažurirati servisa kliknite rukovatelj dohvaćanje trenutnu vrijednost kombinirani okvir regionalne postavke i omogućuje da biste poziv za predmete obavijesti kao što je prikazano:

        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
            var locale = (string)Locale.SelectedItem;

            var categories = new HashSet<string>();
            if (WorldToggle.IsOn) categories.Add("World");
            if (PoliticsToggle.IsOn) categories.Add("Politics");
            if (BusinessToggle.IsOn) categories.Add("Business");
            if (TechnologyToggle.IsOn) categories.Add("Technology");
            if (ScienceToggle.IsOn) categories.Add("Science");
            if (SportsToggle.IsOn) categories.Add("Sports");

            var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(locale,
                 categories);

            var dialog = new MessageDialog("Locale: " + locale + " Subscribed to: " + 
                string.Join(",", categories) + " on registration Id: " + result.RegistrationId);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }


4. Naposljetku, u datoteci App.xaml.cs, provjerite jeste li ažurirati vaš `InitNotificationsAsync` način za dohvaćanje regionalnoj shemi i koristi kod pretplate:

        private async void InitNotificationsAsync()
        {
            var result = await notifications.SubscribeToCategories(notifications.RetrieveLocale());

            // Displays the registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }


##<a name="send-localized-notifications-from-your-back-end"></a>Slanje lokalizirane obavijesti iz vaše pozadinske

[AZURE.INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]






<!-- Anchors. -->
[Template concepts]: #concepts
[The app user interface]: #ui
[Building the Windows Store client app]: #building-client
[Send notifications from your back-end]: #send
[Next Steps]:#next-steps

<!-- Images. -->

<!-- URLs. -->
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs: ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Notify users with Notification Hubs: Mobile Services]: /manage/services/notification-hubs/notify-users
[Korištenje koncentratora obavijesti da biste poslali važnih vijesti]: /manage/services/notification-hubs/breaking-news-dotnet

[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-dotnet
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-dotnet
[Get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-dotnet
[Push notifications to app users]: /develop/mobile/tutorials/push-notifications-to-app-users-dotnet
[Authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-dotnet
[JavaScript and HTML]: /develop/mobile/tutorials/get-started-with-push-js

[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
