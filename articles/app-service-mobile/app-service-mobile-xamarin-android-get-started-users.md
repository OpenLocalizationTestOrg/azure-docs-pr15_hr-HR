<properties
    pageTitle="Početak rada s provjerom autentičnosti mobilne aplikacije u Xamarin Android"
    description="Saznajte kako pomoću mobilne aplikacije za provjeru autentičnosti korisnika aplikacije Xamarin Android kroz mnoštvo davatelji identiteta, uključujući AAD, Google, Facebook, Twitter i Microsoft."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-xamarinandroid-app"></a>Dodavanje provjere autentičnosti Xamarin.Android aplikacije

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

U ovoj se temi objašnjava za provjeru autentičnosti korisnika mobilne aplikacije iz klijentske aplikacije. Pomoću ovog praktičnog vodiča za brzo pokretanje projekta pomoću davatelja identiteta koji podržavaju Azure mobilne aplikacije dodajte provjere autentičnosti. Kada se uspješno provjere autentičnosti i ovlašteni u mobilnu aplikaciju, prikazuje se vrijednost za ID-a korisnika.

Pomoću ovog praktičnog vodiča temelji se na mobilnu aplikaciju brzi početak rada. Najprije se morate dovršiti vodič [Stvaranje Xamarin.Android aplikacije]. Ako ne koristite project server preuzete brzi početak rada, morate dodati paket za provjeru autentičnosti proširenje projekta. Dodatne informacije o paketi proširenja server potražite u članku [Rad s poslužiteljem pozadinske .NET SDK za Azure mobilne aplikacije](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

##<a name="register"></a>Registracija aplikacije za provjeru autentičnosti i konfiguriranje servisa za aplikaciju

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="permissions"></a>Ograničavanje dozvola korisnicima čija je autentičnost provjerena

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

U Visual Studio ili Xamarin Studio, pokrenite projekt klijenta na uređaju ili emulator. Provjerite je li se potencira neobrađenu iznimku s Šifra stanja 401 (Unauthorized) nakon pokretanja aplikacije. To se događa jer aplikacija pokušava pristupiti mobilnu aplikaciju pozadinskog sustava kao Neprovjereni korisnika. U tablici *TodoItem* sada zahtijeva provjeru autentičnosti.

Nakon toga koji će se ažurirati aplikaciju klijent resursa zahtjev iz pozadine za mobilnu aplikaciju s korisnikom čija je autentičnost provjerena.

##<a name="add-authentication"></a>Dodavanje provjere autentičnosti za aplikaciju

Aplikaciju ažurira se od korisnika traži da dodirnite gumb **Prijava** , a prije nego što se prikazuju podaci za provjeru autentičnosti.

1. Dodajte sljedeći kod **TodoActivity** klasa:

        // Define a authenticated user.
        private MobileServiceUser user;
        private async Task<bool> Authenticate()
        {
                var success = false;
                try
                {
                    // Sign in with Facebook login using a server-managed flow.
                    user = await client.LoginAsync(this,
                        MobileServiceAuthenticationProvider.Facebook);
                    CreateAndShowDialog(string.Format("you are now logged in - {0}",
                        user.UserId), "Logged in!");

                    success = true;
                }
                catch (Exception ex)
                {
                    CreateAndShowDialog(ex, "Authentication failed");
                }
                return success;
        }

        [Java.Interop.Export()]
        public async void LoginUser(View view)
        {
            // Load data only after authentication succeeds.
            if (await Authenticate())
            {
                //Hide the button after authentication succeeds.
                FindViewById<Button>(Resource.Id.buttonLoginUser).Visibility = ViewStates.Gone;

                // Load the data.
                OnRefreshItemsSelected();
            }
        }

    Time ste stvorili nov način za provjeru autentičnosti korisnika i rukovatelja način za novi gumb **Prijava** . Korisnik u gornji primjer kod provjere autentičnosti putem servisa Facebook prijava. Dijaloški okvir koristi se za prikaz korisničkog ID-a nakon provjere autentičnosti.

    > [AZURE.NOTE] Ako koristite davateljem identiteta osim servisa Facebook, promijenite vrijednost prenesena **LoginAsync** iznad da biste nešto od sljedećeg: _MicrosoftAccount_, _Twitteru_, _Google_ili _WindowsAzureActiveDirectory_.

3. Metoda **OnCreate** , brisanje ili iz komentara u sljedeći redak koda:

        OnRefreshItemsSelected ();

4. U datoteci Activity_To_Do.axml dodajte sljedeće *LoginUser* gumb definiciju prije postojećeg *AddItem* gumba:

        <Button
            android:id="@+id/buttonLoginUser"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:onClick="LoginUser"
            android:text="@string/login_button_text" />

5. Dodajte sljedeći element datoteka resursa Strings.xml:

        <string name="login_button_text">Sign in</string>

6. U Visual Studio ili Xamarin Studio, pokrenite projekt klijenta na uređaju ili emulator i prijavite se pomoću davatelja identiteta odabrano.

    Kada uspješno prijaviti, aplikaciju prikazat će se ID za prijavu i popis stavki obveze i unesete ažuriranja podataka.


<!-- URLs. -->
[Stvaranje Xamarin.Android aplikacije]: app-service-mobile-xamarin-android-get-started.md
