<properties
    pageTitle="Početak rada s provjerom autentičnosti za mobilne aplikacije u aplikaciji Xamarin.Forms | Microsoft Azure"
    description="Saznajte kako pomoću mobilne aplikacije za provjeru autentičnosti korisnika aplikacije Xamarin obrasce pomoću raznih davatelji identiteta, uključujući AAD, Google, Facebook, Twitter i Microsoft."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-xamarinforms-app"></a>Dodavanje provjere autentičnosti Xamarin.Forms aplikacije

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

##<a name="overview"></a>Pregled

U ovoj se temi objašnjava za provjeru autentičnosti korisnika aplikacije usluga mobilne aplikacije iz klijentske aplikacije. U ovom ćete praktičnom vodiču dodajte provjere autentičnosti Xamarin.Forms brzi početak rada projekta davateljem identiteta koji podržava aplikacije servisa. Kada se uspješno provjere autentičnosti i odobrio mobilnu aplikaciju, prikazuje se vrijednost za ID-a korisnika pa će moći pristupiti podacima ograničene tablice.

##<a name="prerequisites"></a>Preduvjeti

Za najbolje rezultat pomoću ovog praktičnog vodiča, preporučujemo da prvo morate dovršiti Praktični vodič za [Stvaranje Xamarin.Forms aplikacije](app-service-mobile-xamarin-forms-get-started.md) . Kada dovršite ovaj Praktični vodič, imat ćete Xamarin.Forms projekt koji je aplikacija za TodoList više platforme.

Ako ne koristite project server preuzete brzi početak rada, morate dodati paket za provjeru autentičnosti proširenje projekta. Dodatne informacije o paketi proširenja server potražite u članku [Rad s poslužiteljem pozadinske .NET SDK za Azure mobilne aplikacije](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

##<a name="register-your-app-for-authentication-and-configure-app-services"></a>Registracija aplikacije za provjeru autentičnosti i konfiguriranje servisa za aplikaciju

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="restrict-permissions-to-authenticated-users"></a>Ograničavanje dozvola korisnicima čija je autentičnost provjerena

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]


##<a name="add-authentication-to-the-portable-class-library"></a>Dodavanje provjere autentičnosti u biblioteku prijenosni klase

Mobilne aplikacije pomoću načina nastavka [LoginAsync] na [MobileServiceClient] korisnika s provjerom autentičnosti aplikacije servisa za prijavu. Ovaj primjer koristi tijek poslužitelja upravljani provjere autentičnosti koja prikazuje vašeg davatelja usluge prijavu sučelja u aplikaciju. Dodatne informacije potražite u članku [poslužitelja upravljani provjere autentičnosti](app-service-mobile-dotnet-how-to-use-client-library.md#serverflow). Omogućuje bolje iskustvo korisnika u svojoj aplikaciji radnog razmotrite umjesto pomoću [provjere autentičnosti u klijentskim upravljani](app-service-mobile-dotnet-how-to-use-client-library.md#clientflow). 

Za provjeru autentičnosti s projektom Xamarin.Forms, definirajte **IAuthenticate** sučelja u prijenosni Biblioteka klasa za aplikaciju. I ažurirati korisničko sučelje definirano u prijenosni Biblioteka klasa da biste dodali **prijavu** gumb, koji korisnik klikne da biste pokrenuli provjeru autentičnosti. Nakon uspješne provjere autentičnosti podataka učita iz mobilne aplikacije pozadine.

Morate provesti **IAuthenticate** sučelje za svaki platformu podržava aplikacije.


1. U Visual Studio ili Xamarin Studio, otvaranje App.cs iz projekta s **Portable** u nazivu, što je prijenosni Biblioteka klasa projekta, zatim dodajte sljedeće `using` izjava:

        using System.Threading.Tasks;

2. U App.cs, dodajte sljedeće `IAuthenticate` neposredno prije sučelja definicija na `App` klase definicija.

        public interface IAuthenticate
        {
            Task<bool> Authenticate();
        }

3. Dodajte sljedeće statične članove predmete **aplikacije** Inicijalizacija sučelje s platformu implementaciji za određene.

        public static IAuthenticate Authenticator { get; private set; }

        public static void Init(IAuthenticate authenticator)
        {
            Authenticator = authenticator;
        }

4. Otvorite TodoList.xaml iz Portable Biblioteka klasa projekta, dodajte sljedeći element **gumb** izgleda element *buttonsPanel* nakon postojećeg gumba: 

        <Button x:Name="loginButton" Text="Sign-in" MinimumHeightRequest="30" 
            Clicked="loginButton_Clicked"/>

    Ovaj gumb pokreće provjeru autentičnosti poslužitelja upravlja s pozadinskom vaše aplikacije za mobilne uređaje.

5. Otvorite TodoList.xaml.cs iz prijenosni Biblioteka klasa projekta, a zatim dodajte sljedeće polje u `TodoList` klasa:

        // Track whether the user has authenticated. 
        bool authenticated = false;


6. Zamijenite metodu **OnAppearing** sljedeći kod:

        protected override async void OnAppearing()
        {
            base.OnAppearing();
    
            // Refresh items only when authenticated.
            if (authenticated == true)
            {
                // Set syncItems to true in order to synchronize the data 
                // on startup when running in offline mode.
                await RefreshItems(true, syncItems: false);

                // Hide the Sign-in button.
                this.loginButton.IsVisible = false;
            }
        }

    To jamči da samo osvježavanja podataka iz servisa nakon autentičnost korisnika.

7. Dodajte sljedeći rukovatelj događaja **Clicked** **TodoList** klasa:

        async void loginButton_Clicked(object sender, EventArgs e)
        {
            if (App.Authenticator != null)
                authenticated = await App.Authenticator.Authenticate();

            // Set syncItems to true to synchronize the data on startup when offline is enabled.
            if (authenticated == true)
                await RefreshItems(true, syncItems: false);
        }

8. Spremite promjene i ponovno stvaranje projekta prijenosni Biblioteka klasa potvrđivanja bez pogrešaka.


##<a name="add-authentication-to-the-android-app"></a>Dodavanje provjere autentičnosti aplikacija za Android

U ovom se odjeljku objašnjava implementira sučelje **IAuthenticate** u programu project aplikacija za Android. Ako ne podržava uređaje sa sustavom Android, preskočite ovaj odjeljak.

1. U Visual Studio ili Xamarin Studio, desnom tipkom miša kliknite **droid** projekta, **postavite kao pokretanje projekta**.

2. Pritisnite F5 da biste započeli projekta u program za ispravljanje pogrešaka, a zatim provjerite je li se potencira neobrađenu iznimku s Šifra stanja 401 (Unauthorized) nakon pokretanja aplikacije. To se događa jer pristup pozadinski ograničen na samo ovlašteni korisnici.

3. Otvorite MainActivity.cs u programu project za Android i dodajte sljedeće `using` izvješća:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;

4. Ažurirajte predmete **MainActivity** možete implementirati sučelje **IAuthenticate** na sljedeći način:

        public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity, IAuthenticate


5. Ažurirajte predmete **MainActivity** dodavanjem polja **MobileServiceUser** i metoda **provjere autentičnosti** , koji je potreban sučelja **IAuthenticate** na sljedeći način:

        // Define a authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            var success = false;
            var message = string.Empty;
            try
            {
                // Sign in with Facebook login using a server-managed flow.
                user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(this,
                    MobileServiceAuthenticationProvider.Facebook);
                if (user != null)
                {
                    message = string.Format("you are now signed-in as {0}.",
                        user.UserId);
                    success = true;
                }
            }
            catch (Exception ex)
            {
                message = ex.Message;
            }

            // Display the success or failure message.
            AlertDialog.Builder builder = new AlertDialog.Builder(this);
            builder.SetMessage(message);
            builder.SetTitle("Sign-in result");
            builder.Create().Show();

            return success;
        }


    Ako koristite davateljem identiteta osim servisa Facebook, odaberite neku drugu vrijednost za [MobileServiceAuthenticationProvider].

6. Dodavanje koda za sljedeće metodu **OnCreate** klase **MainActivity** prije poziv na `LoadApplication()`:

        // Initialize the authenticator before loading the app.
        App.Init((IAuthenticate)this);

    Time ćete da se u autentikatora pokrenut prije opterećenje aplikacije.

7. Ponovno stvaranje aplikaciju, pokrenite ga, a zatim prijave pomoću davatelja usluge provjere autentičnosti odabrali i provjerite je li moguće pristupiti podacima kao korisnika čija je autentičnost provjerena.

##<a name="add-authentication-to-the-ios-app"></a>Dodavanje provjere autentičnosti za aplikaciju za iOS

U ovom se odjeljku objašnjava implementira sučelje **IAuthenticate** u programu project za aplikaciju iOS. Ako ne podržava uređaje sa sustavom iOS, preskočite ovaj odjeljak.

1. U Visual Studio ili Xamarin Studio, desnom tipkom miša kliknite projekt **iOS** , **postavite kao pokretanje projekta**.

2. Pritisnite F5 da biste započeli projekta u program za ispravljanje pogrešaka, a zatim provjerite je li se potencira neobrađenu iznimku uz kod stanja 401 (Unauthorized) nakon pokretanja aplikacije. To se događa jer pristup pozadinski ograničen na samo ovlašteni korisnici.

4. Otvorite AppDelegate.cs u programu project za iOS i dodajte sljedeće `using` izvješća:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;

4. Ažurirajte predmete **AppDelegate** možete implementirati sučelje **IAuthenticate** na sljedeći način:

        public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate, IAuthenticate

5. Ažurirajte predmete **AppDelegate** dodavanjem polja **MobileServiceUser** i metoda **provjere autentičnosti** , koji je potreban sučelja **IAuthenticate** na sljedeći način:

        // Define a authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            var success = false;
            var message = string.Empty;
            try
            {
                // Sign in with Facebook login using a server-managed flow.
                if (user == null)
                {
                    user = await TodoItemManager.DefaultManager.CurrentClient
                        .LoginAsync(UIApplication.SharedApplication.KeyWindow.RootViewController,
                        MobileServiceAuthenticationProvider.Facebook);
                    if (user != null)
                    {
                        message = string.Format("You are now signed-in as {0}.", user.UserId);
                        success = true;                        
                    }
                }        
            }
            catch (Exception ex)
            {
               message = ex.Message;
            }

            // Display the success or failure message.
            UIAlertView avAlert = new UIAlertView("Sign-in result", message, null, "OK", null);
            avAlert.Show();         

            return success;
        }

    Ako koristite davateljem identiteta osim servisa Facebook, odaberite neku drugu vrijednost za [MobileServiceAuthenticationProvider].

6. Dodajte sljedeći redak koda metodu **FinishedLaunching** prije poziv na `LoadApplication()`: 

        App.Init(this);

    To jamči da se pokreće na autentikatora prije učitavanja aplikaciju.

7. Ponovno stvaranje aplikaciju, pokrenite ga, a zatim prijave pomoću davatelja usluge provjere autentičnosti odabrali i provjerite je li moguće pristupiti podacima kao korisnika čija je autentičnost provjerena.


##<a name="add-authentication-to-windows-app-projects"></a>Dodavanje provjere autentičnosti projektima aplikacija za Windows

U ovom se odjeljku objašnjava implementira sučelje **IAuthenticate** u sustavu Windows 8.1 i Windows Phone 8.1 aplikacije projekata. Isti se koraci primjenjuju za projekte univerzalni platforme Windows (UWP). Ako ne podržava uređaje sa sustavom Windows, preskočite ovaj odjeljak.

1. U Visual Studio, desnom tipkom miša kliknite **WinApp** ili **WinPhone81** projekta, **postavite kao pokretanje projekta**.

2. Pritisnite F5 da biste započeli projekta u program za ispravljanje pogrešaka, a zatim provjerite je li se potencira neobrađenu iznimku s Šifra stanja 401 (Unauthorized) nakon pokretanja aplikacije. To se događa jer pristup pozadinski ograničen na samo ovlašteni korisnici.

3. Otvorite MainPage.xaml.cs za projekt programa Windows aplikacije, a zatim dodajte sljedeće `using` izvješća:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
        using Windows.UI.Popups;
        using <your_Portable_Class_Library_namespace>;

    Zamjena `<your_Portable_Class_Library_namespace>` s prostorom za biblioteku prijenosni predmete.

4. Ažurirajte predmete **MainPage** implementaciju sučelja **IAuthenticate** na sljedeći način:

        public sealed partial class MainPage : IAuthenticate


5. Ažurirajte predmete **MainPage** dodavanjem polja **MobileServiceUser** i metoda **provjere autentičnosti** , koji je potreban sučelja **IAuthenticate** na sljedeći način:

        // Define a authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            string message = string.Empty;
            var success = false;

            try
            {
                // Sign in with Facebook login using a server-managed flow.
                if (user == null)
                {
                    user = await TodoItemManager.DefaultManager.CurrentClient
                        .LoginAsync(MobileServiceAuthenticationProvider.Facebook);
                    if (user != null)
                    {
                        success = true;
                        message = string.Format("You are now signed-in as {0}.", user.UserId);
                    }
                }
                
            }
            catch (Exception ex)
            {
                message = string.Format("Authentication Failed: {0}", ex.Message);
            }

            // Display the success or failure message.
            await new MessageDialog(message, "Sign-in result").ShowAsync();

            return success;
        }


    Ako koristite davateljem identiteta osim servisa Facebook, odaberite neku drugu vrijednost za [MobileServiceAuthenticationProvider].

6. Dodavanje sljedeći redak koda u Graditelj klase **MainPage** prije poziv na `LoadApplication()`:

        // Initialize the authenticator before loading the app.
        <your_Portable_Class_Library_namespace>.App.Init(this);
 
    Zamjena `<your_Portable_Class_Library_namespace>` s prostorom za biblioteku prijenosni predmete.  
    Ako je ovo WinApp projekta, možete preskočiti prema dolje do korak 8. Sljedeći korak odnosi samo na WinPhone81 projekta, mjesto na koje morate poduzeti povratnog prijava.

7. (Neobavezno) U programu project aplikacije **WinPhone81** , otvorite App.xaml.cs i dodajte sljedeće `using` izvješća:

        using Microsoft.WindowsAzure.MobileServices;
        using <your_Portable_Class_Library_namespace>;

    Zamjena `<your_Portable_Class_Library_namespace>` s prostorom za biblioteku prijenosni predmete.

8.  Dodajte sljedeće nadjačavanje način **OnActivated** predmete **aplikacije** :

        protected override void OnActivated(IActivatedEventArgs args)
        {
            base.OnActivated(args);

            // We just need to handle activation that occurs after web authentication. 
            if (args.Kind == ActivationKind.WebAuthenticationBrokerContinuation)
            {
                // Get the client and call the LoginComplete method to complete authentication.
                var client = TodoItemManager.DefaultManager.CurrentClient as MobileServiceClient;
                client.LoginComplete(args as WebAuthenticationBrokerContinuationEventArgs);
            }
        }

    Kada nadjačavanje način već postoji, jednostavno dodajte uvjetno kod iz iznad isječka.

7. Ponovno stvaranje aplikaciju, pokrenite ga, a zatim prijave pomoću davatelja usluge provjere autentičnosti odabrali i provjerite je li moguće pristupiti podacima kao korisnika čija je autentičnost provjerena.

##<a name="next-steps"></a>Daljnji koraci

Sad kad Završi ovog praktičnog vodiča osnovnu provjeru autentičnosti, razmotrite nastavka na jedan od sljedećih vodiči za:

+ [Automatske obavijesti dodati aplikacije](app-service-mobile-xamarin-forms-get-started-push.md)  
  Saznajte kako dodati automatske obavijesti podržava aplikacije i konfigurirati mobilnu aplikaciju pozadinskog sustava da biste koristili koncentratora Azure obavijesti da biste poslali automatske obavijesti.

+ [Omogućivanje izvanmrežne sinkronizacije za aplikaciju](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Saznajte kako dodati aplikacije pomoću programa pozadinskog mobilnu aplikaciju Izvanmrežna podrška. Izvanmrežna sinkronizacija omogućuje krajnjim korisnicima omogućuje interakciju s mobilne aplikacije&mdash;prikaz, dodavanje ili izmjenu podataka&mdash;čak i kad je nema mrežne veze.

<!-- Images. -->

<!-- URLs. -->
[apns object]: http://go.microsoft.com/fwlink/p/?LinkId=272333
[LoginAsync]: https://msdn.microsoft.com/library/azure/dn268341(v=azure.10).aspx
[MobileServiceClient]: https://msdn.microsoft.com/library/azure/JJ553674(v=azure.10).aspx
[MobileServiceAuthenticationProvider]: https://msdn.microsoft.com/library/azure/jj730936(v=azure.10).aspx

