<properties
    pageTitle="Početak rada s provjerom autentičnosti za mobilne aplikacije u Xamarin za iOS"
    description="Saznajte kako pomoću mobilne aplikacije za provjeru autentičnosti korisnika aplikacije iOS Xamarin kroz mnoštvo davatelji identiteta, uključujući AAD, Google, Facebook, Twitter i Microsoft."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-xamarinios-app"></a>Dodavanje provjere autentičnosti Xamarin.iOS aplikacije

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

U ovoj se temi objašnjava za provjeru autentičnosti korisnika aplikacije usluga mobilne aplikacije iz klijentske aplikacije. U ovom ćete praktičnom vodiču dodajte provjere autentičnosti Xamarin.iOS brzi početak rada projekta davateljem identiteta koji podržava aplikacije servisa. Kada se uspješno provjere autentičnosti i odobrio mobilnu aplikaciju, prikazuje se vrijednost za ID-a korisnika i moći pristupiti podacima ograničene tablice.

Najprije morate dovršiti vodič [Stvaranje Xamarin.iOS aplikacije]. Ako ne koristite project server preuzete brzi početak rada, morate dodati paket za provjeru autentičnosti proširenje projekta. Dodatne informacije o paketi proširenja server potražite u članku [Rad s poslužiteljem pozadinske .NET SDK za Azure mobilne aplikacije](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

##<a name="register-your-app-for-authentication-and-configure-app-services"></a>Registracija aplikacije za provjeru autentičnosti i konfiguriranje servisa za aplikaciju

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="restrict-permissions-to-authenticated-users"></a>Ograničavanje dozvola korisnicima čija je autentičnost provjerena

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

&nbsp;&nbsp;4. u Visual Studio ili Xamarin Studio, pokrenite projekt klijenta na uređaju ili emulator. Provjerite je li se potencira neobrađenu iznimku s Šifra stanja 401 (Unauthorized) nakon pokretanja aplikacije. Neuspjeh prijavljen je na konzolu za ispravljanje pogrešaka. Tako da u Visual Studio, trebali biste vidjeti pogreške u izlaznom prozoru.

&nbsp;&nbsp;Neovlaštenim prekida događa jer aplikacija pokušava pristupiti mobilnu aplikaciju pozadinskog sustava kao Neprovjereni korisnika. U tablici *TodoItem* sada zahtijeva provjeru autentičnosti.

Nakon toga koji će se ažurirati aplikaciju klijent resursa zahtjev iz pozadine za mobilnu aplikaciju s korisnikom čija je autentičnost provjerena.

##<a name="add-authentication-to-the-app"></a>Dodavanje provjere autentičnosti za aplikaciju

U ovom ćete odjeljku će izmjena aplikacije za prikaz na zaslonu za prijavu prije prikaza podataka. Prilikom pokretanja aplikacije će ne povezati se s aplikacije servisa i neće se prikazati sve podatke. Nakon kada prvi put koji korisnik izvodi gesta osvježavanja pojavit će se zaslon za prijavu; Nakon uspješne prijava prikazat će se popis stavki obveze.

1. U programu project klijenta otvorite datoteku **QSTodoService.cs** i dodajte sljedeće pomoću naredbe i `MobileServiceUser` s pristupnika za predmete QSTodoService:

    ```
        using UIKit;
    ```

        // Logged in user
        private MobileServiceUser user;
        public MobileServiceUser User { get { return user; } }

2. Dodajte nov način pod nazivom **provjere autentičnosti** za **QSTodoService** s definicijom sljedeće:


        public async Task Authenticate(UIViewController view)
        {
            try
            {
                user = await client.LoginAsync(view, MobileServiceAuthenticationProvider.Facebook);
            }
            catch (Exception ex)
            {
                Console.Error.WriteLine (@"ERROR - AUTHENTICATION FAILED {0}", ex.Message);
            }
        }

    >[AZURE.NOTE] Ako koristite davateljem identiteta osim na Facebook, promijenite vrijednost proslijediti **LoginAsync** iznad da biste nešto od sljedećeg: _MicrosoftAccount_, _Twitteru_, _Google_ili _WindowsAzureActiveDirectory_.

3. Otvorite **QSTodoListViewController.cs**. Izmjena definiciju način **ViewDidLoad** uklanjanje poziv **RefreshAsync()** pri kraju:

        public override async void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            todoService = QSTodoService.DefaultService;
           await todoService.InitializeStoreAsync ();

           RefreshControl.ValueChanged += async (sender, e) => {
                await RefreshAsync ();
           }

            // Comment out the call to RefreshAsync
            // await RefreshAsync ();
        }


4. Promijenite način **RefreshAsync** za provjeru autentičnosti ako je svojstvo **korisničkog** null. Dodajte sljedeći kôd pri vrhu definiciju metoda:

        // start of RefreshAsync method
        if (todoService.User == null) {
            await QSTodoService.DefaultService.Authenticate (this);
            if (todoService.User == null) {
                Console.WriteLine ("couldn't login!!");
                return;
            }
        }
        // rest of RefreshAsync method

5. U Visual Studio ili Xamarin Studio povezan s vašeg Xamarin sastavljanje glavnog računala na računalu Mac, pokrenite projekt klijent ciljanja uređaja ili emulator. Provjerite je li koji prikazuje aplikacije bez podataka.

    Izvođenje gesta osvježavanja po izvlačenja prema dolje po popisu stavki, uzrokovat će se zaslon za prijavu da se pojavi. Kada uspješno unesete valjani vjerodajnice, aplikaciju prikazat će popis stavki obveze i unesete ažuriranja podataka.


<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Stvaranje Xamarin.iOS aplikacije]: app-service-mobile-xamarin-ios-get-started.md
