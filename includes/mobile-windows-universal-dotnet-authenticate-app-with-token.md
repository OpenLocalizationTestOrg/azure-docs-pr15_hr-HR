
1. U datoteci MainPage.xaml.cs projekt, dodajte sljedeće **pomoću** naredbe:

        using System.Linq;      
        using Windows.Security.Credentials;

2. Zamijenite metodu **AuthenticateAsync** sljedeći kod:

        private async System.Threading.Tasks.Task<bool> AuthenticateAsync()
        {
            string message;
            bool success = false;

            // This sample uses the Facebook provider.
            var provider = MobileServiceAuthenticationProvider.Facebook;

            // Use the PasswordVault to securely store and access credentials.
            PasswordVault vault = new PasswordVault();
            PasswordCredential credential = null;

            try
            {
                // Try to get an existing credential from the vault.
                credential = vault.FindAllByResource(provider.ToString()).FirstOrDefault();
            }
            catch (Exception)
            {
                // When there is no matching resource an error occurs, which we ignore.
            }

            if (credential != null)
            {
                // Create a user from the stored credentials.
                user = new MobileServiceUser(credential.UserName);
                credential.RetrievePassword();
                user.MobileServiceAuthenticationToken = credential.Password;

                // Set the user from the stored credentials.
                App.MobileService.CurrentUser = user;

                // Consider adding a check to determine if the token is 
                // expired, as shown in this post: http://aka.ms/jww5vp.

                success = true;
                message = string.Format("Cached credentials for user - {0}", user.UserId);
            }
            else
            {
                try
                {
                    // Login with the identity provider.
                    user = await App.MobileService
                        .LoginAsync(provider);

                    // Create and store the user credentials.
                    credential = new PasswordCredential(provider.ToString(),
                        user.UserId, user.MobileServiceAuthenticationToken);
                    vault.Add(credential);

                    success = true;
                    message = string.Format("You are now logged in - {0}", user.UserId);
                }
                catch (MobileServiceInvalidOperationException)
                {
                    message = "You must log in. Login Required";
                }
            }
            
            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();

            return success;
        }

    U ovoj verziji programa **AuthenticateAsync**aplikacija pokušava koristiti vjerodajnicama pohranjenima u **PasswordVault** za pristup servisu. Na obični prijave i provodi kada nema pohranjenih vjerodajnica.

    >[AZURE.NOTE]Predmemorirane token je možda istekla, a isteka tokena može pojaviti nakon provjere autentičnosti kada se koristi aplikacija. Da biste saznali kako utvrditi je token istekla, potražite u članku [Traženje tokeni istekle provjere autentičnosti](http://aka.ms/jww5vp). Rješenje zadužen za autorizaciju pogreške vezane uz istekle tokena, potražite u članku objavljivanje [Međuspremanje i obrađuju istekle tokena u Azure mobilne usluge upravlja SDK-a](http://blogs.msdn.com/b/carlosfigueira/archive/2014/03/13/caching-and-handling-expired-tokens-in-azure-mobile-services-managed-sdk.aspx). 

3. Ponovno pokrenite aplikaciju dvaput.

    Potrebno je ponovno obavijest na prvog pokretanja prijavu pomoću davatelja. Međutim, na drugi ponovno pokretanje predmemorirane vjerodajnice koristi, a prijavu je zaobiđena. 
