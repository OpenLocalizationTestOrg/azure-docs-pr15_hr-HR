<properties
    pageTitle="Dodavanje provjere autentičnosti u aplikaciju za univerzalni platforme Windows (UWP) | Azure mobilne aplikacije"
    description="Saznajte kako pomoću mobilne aplikacije za Azure aplikacija servis za provjeru autentičnosti korisnika aplikacije univerzalni platforme Windows (UWP) u različitim davatelji identiteta, uključujući: AAD, Google, Facebook, Twitter i Microsoft."
    services="app-service\mobile"
    documentationCenter="windows"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-windows-app"></a>Dodavanje provjere autentičnosti aplikacija za Windows

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

U ovoj se temi objašnjava da biste dodali provjere autentičnosti utemeljene na oblaku u mobilnoj aplikaciji. U ovom ćete praktičnom vodiču dodajte provjere autentičnosti brzi početak rada projekta univerzalni platforme Windows (UWP) za mobilne aplikacije pomoću davatelja identiteta koji podržava aplikacije servisa za Azure. Nakon se uspješno provjere autentičnosti i odobrio pozadinskog sustava mobilnu aplikaciju, prikazuje se vrijednost za ID-a korisnika.

Pomoću ovog praktičnog vodiča temelji se na mobilne aplikacije brzi početak rada. Najprije morate dovršiti vodič [Početak rada s mobilne aplikacije](app-service-mobile-windows-store-dotnet-get-started.md).

##<a name="register"></a>Registracija aplikacije za provjeru autentičnosti i konfiguriranje servisa za aplikaciju

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="permissions"></a>Ograničavanje dozvola korisnicima čija je autentičnost provjerena

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Sada možete provjeriti anonimni pristup vašem pozadinskog je onemogućen. S projektom aplikacije UWP postaviti kao projekt pokretanja, uvođenje i pokrenuti aplikaciju; Provjerite je li se potencira neobrađenu iznimku s Šifra stanja 401 (Unauthorized) nakon pokretanja aplikacije. To se događa jer aplikacija pokušava pristup kodu mobilne aplikacije kao Neprovjereni korisnika, ali *TodoItem* tablica sada zahtijeva provjeru autentičnosti.

Nakon toga će se ažurirati aplikaciju za provjeru autentičnosti korisnika prije traži resursa iz aplikacije servisa.

##<a name="add-authentication"></a>Dodavanje provjere autentičnosti za aplikaciju

1. U na UWP aplikacije datoteke MainPage.cs projekta i dodajte sljedeće koda za predmete MainPage:
    
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

3. Komentar u novom ili brisanje poziv na **ButtonRefresh_Click** (i metode **InitLocalStoreAsync** ) u postojeće nadjačavanje način **OnNavigatedTo** . To sprječava podatke učitavanja prije provjere autentičnosti korisnika. Nakon toga će dodajte gumb **Prijava u** aplikaciju koja pokreće provjeru autentičnosti.

4. Dodajte sljedeće koda MainPage klasa:

        private async void ButtonLogin_Click(object sender, RoutedEventArgs e)
        {
            // Login the user and then load data from the mobile app.
            if (await AuthenticateAsync())
            {
                // Switch the buttons and load items from the mobile app.
                ButtonLogin.Visibility = Visibility.Collapsed;
                ButtonSave.Visibility = Visibility.Visible;
                //await InitLocalStoreAsync(); //offline sync support.
                await RefreshTodoItems();
            }
        }
        
5. Otvorite datoteku MainPage.xaml projekta, pronađite element koji definira gumb **Spremi** i zamijeniti ga sljedeći kod:

        <Button Name="ButtonSave" Visibility="Collapsed" Margin="0,8,8,0" 
                Click="ButtonSave_Click">
            <StackPanel Orientation="Horizontal">
                <SymbolIcon Symbol="Add"/>
                <TextBlock Margin="5">Save</TextBlock>
            </StackPanel>
        </Button>
        <Button Name="ButtonLogin" Visibility="Visible" Margin="0,8,8,0" 
                Click="ButtonLogin_Click" TabIndex="0">
            <StackPanel Orientation="Horizontal">
                <SymbolIcon Symbol="Permissions"/>
                <TextBlock Margin="5">Sign in</TextBlock> 
            </StackPanel>
        </Button>

9. Pokrenite aplikaciju, kliknite gumb **prijavite se u** i prijavite se u aplikaciju pomoću davatelja identiteta odabranom, pritisnite tipku F5. Kada vaš prijavu ne uspije, aplikaciju pokreće bez pogrešaka, a možete vaše pozadinskog upita, a zatim napravite ažuriranja podataka.


##<a name="tokens"></a>Spremanje token za provjeru autentičnosti na klijentskom računalu

U prethodnom primjeru prikazivao standard prijavu, što zahtijeva klijenta za kontakt davatelja identiteta i aplikacije servisa prilikom svakog pokretanja aplikacije. Ne samo nije ovu metodu, možete pokrenuti u korištenje-odnosi problema s više od jednog klijenta trebao da biste pokrenuli aplikaciju u isto vrijeme. Bolje pristup je predmemoriju ovlaštenja vratio aplikacije servisa i pokušajte koristiti u ovom prvom prije korištenja je utemeljen na davatelja prijavu.

>[AZURE.NOTE]Možete predmemoriju token izdala aplikacije servisa neovisno o tome koristite klijent upravlja ili servis upravlja provjere autentičnosti. Pomoću ovog praktičnog vodiča koristi servis upravlja provjere autentičnosti.

[AZURE.INCLUDE [mobile-windows-universal-dotnet-authenticate-app-with-token](../../includes/mobile-windows-universal-dotnet-authenticate-app-with-token.md)]

##<a name="next-steps"></a>Daljnji koraci

Sad kad Završi ovog praktičnog vodiča osnovnu provjeru autentičnosti, razmotrite nastavka na jedan od sljedećih vodiči za:

+ [Automatske obavijesti dodati aplikacije](app-service-mobile-windows-store-dotnet-get-started-push.md)  
  Saznajte kako dodati automatske obavijesti podržava aplikacije i konfigurirati mobilnu aplikaciju pozadinskog sustava da biste koristili koncentratora Azure obavijesti da biste poslali automatske obavijesti.

+ [Omogućivanje izvanmrežne sinkronizacije za aplikaciju](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Saznajte kako dodati aplikacije pomoću programa pozadinskog mobilnu aplikaciju Izvanmrežna podrška. Izvanmrežna sinkronizacija omogućuje krajnjim korisnicima omogućuje interakciju s mobilne aplikacije&mdash;prikaz, dodavanje ili izmjenu podataka&mdash;čak i kad je nema mrežne veze.


<!-- URLs. -->
[Get started with your mobile app]: app-service-mobile-windows-store-dotnet-get-started.md

