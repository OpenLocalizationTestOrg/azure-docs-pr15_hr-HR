
1. Otvorite datoteku zajedničke projekta MainPage.cs i dodajte sljedeće koda klase MainPage:
    
        // Define a member variable for storing the signed-in user. 
        private MobileServiceUser user;

        // Define a method that performs the authentication process
        // using a Facebook sign-in. 
        private async System.Threading.Tasks.Task<bool> AuthenticateAsync()
        {
            string message;
            bool success = false;
            try
            {
                // Change 'MobileService' to the name of your MobileServiceClient instance.
                // Sign-in using Facebook authentication.
                user = await App.MobileService
                    .LoginAsync(MobileServiceAuthenticationProvider.Facebook);
                message =
                    string.Format("You are now signed in - {0}", user.UserId);

                success = true;
            }
            catch (InvalidOperationException)
            {
                message = "You must log in. Login Required";
            }

            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
            return success;
        }

    Kod potvrđuje korisnika s Facebooka prijava. Ako koristite davateljem identiteta osim servisa Facebook, promijenite vrijednost **MobileServiceAuthenticationProvider** iznad vrijednosti za davatelja usluga.

3. Komentar u novom ili izbrišite poziv na način **RefreshTodoItems** u postojeće nadjačavanje način **OnNavigatedTo** .

    To sprječava podatke učitavanja prije provjere autentičnosti korisnika. Nakon toga će dodajte gumb **Prijava u** aplikaciju koja pokreće provjeru autentičnosti.

4. Dodajte sljedeće koda MainPage klasa:

        private async void ButtonLogin_Click(object sender, RoutedEventArgs e)
        {
            // Login the user and then load data from the mobile app.
            if (await AuthenticateAsync())
            {
                // Hide the login button and load items from the mobile app.
                ButtonLogin.Visibility = Windows.UI.Xaml.Visibility.Collapsed;
                //await InitLocalStoreAsync(); //offline sync support.
                await RefreshTodoItems();
            }
        }
        
5. U programu project aplikacija iz Windows trgovine, otvorite datoteku MainPage.xaml projekta i dodajte sljedeći element **gumb** neposredno prije element koji definira gumb **Spremi** :

        <Button Name="ButtonLogin" Click="ButtonLogin_Click" 
                        Visibility="Visible">Sign in</Button>

6. U programu project aplikacije iz trgovine Windows Phone, dodajte sljedeći element **gumba** u **ContentPanel**nakon element **tekstni okvir** :

        <Button Grid.Row ="1" Grid.Column="1" Name="ButtonLogin" Click="ButtonLogin_Click" 
            Margin="10, 0, 0, 0" Visibility="Visible">Sign in</Button>

8. Otvorite zajedničku datoteku App.xaml.cs projekta i dodati sljedeći kod:

        protected override void OnActivated(IActivatedEventArgs args)
        {
            // Windows Phone 8.1 requires you to handle the respose from the WebAuthenticationBroker.
            #if WINDOWS_PHONE_APP
            if (args.Kind == ActivationKind.WebAuthenticationBrokerContinuation)
            {
                // Completes the sign-in process started by LoginAsync.
                // Change 'MobileService' to the name of your MobileServiceClient instance. 
                App.MobileService.LoginComplete(args as WebAuthenticationBrokerContinuationEventArgs);
            }
            #endif

            base.OnActivated(args);
        }

    Ako već postoji način **OnActivated** , jednostavno dodajte na `#if...#endif` blokova koda.

9. Da biste pokrenuli aplikaciju iz Windows trgovine, kliknite gumb **prijavite se u** i prijavite se u aplikaciju pomoću davatelja identiteta odabranom, pritisnite tipku F5. 

    Kada uspješno prijaviti, aplikaciju pokretati bez pogrešaka i trebali biste moći vaše pozadinskog upita, a zatim napravite ažuriranja podataka.

10. Desnom tipkom miša kliknite projekt aplikacije iz trgovine Windows Phone, kliknite **Postavi kao pokretanje projekta**, a zatim Ponavljajte prethodni korak da biste potvrdili da Windows Phone trgovine aplikaciju i pokreće se ispravno.  

 