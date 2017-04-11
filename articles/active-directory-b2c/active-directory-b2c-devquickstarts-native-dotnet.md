<properties
    pageTitle="Azure Active Directory B2C | Microsoft Azure"
    description="Sastavljanje Windows stolna računala koja obuhvaća prijavu, prijave, i upravljanje profila pomoću Azure Active Directory B2C."
    services="active-directory-b2c"
    documentationCenter=".net"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-ad-b2c-build-a-windows-desktop-app"></a>Azure AD B2C: Stvaranje aplikacije za radnu površinu sustava Windows

Pomoću B2C Azure Active Directory (Azure AD), možete dodati značajke upravljanja Napredna samostalno identiteta u aplikaciju za stolna računala u nekoliko koraka kratki. U ovom se članku će vam pokazati kako stvoriti aplikaciju za .NET Windows Presentation Foundation (WPF) "popis obaveza" koja obuhvaća prijave, prijavu, Upravljanje korisnicima i profila. Aplikaciju neće sadržavati stavku podrška za prijavu i prijaviti pomoću korisničkog imena ili e-pošte. Pomoću računi društvenih kao što su Facebook i Google ga neće sadržavati i podršku za prijavu i prijavu.

## <a name="get-an-azure-ad-b2c-directory"></a>Početak u imeniku Azure AD B2C

Prije korištenja Azure AD B2C morate stvoriti direktorij ili smjernice.  Direktorij je spremnik za sve korisnike, aplikacije, grupe i više. Ako ne postoji već, [stvorite direktorij B2C](active-directory-b2c-get-started.md) prije nego što nastavite u ovom vodiču.

## <a name="create-an-application"></a>Stvaranje aplikacije komponente

Ćete morati stvoriti aplikaciju u direktoriju B2C. Tako ćete dobiti Azure AD informacije potrebne za sigurno komunikaciju s aplikacijom. Da biste stvorili aplikaciju, slijedite [ove upute](active-directory-b2c-app-registration.md).  Obavezno:

- U aplikaciji obuhvaćaju **nativni klijent** .
- Kopiranje **preusmjeravanje URI** `urn:ietf:wg:oauth:2.0:oob`. Je zadani URL za ovaj uzorak koda.
- Kopirajte **ID aplikacije** koji vam je dodijeljen aplikacije. Jer će vam kasnije.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Stvaranje police

U Azure AD B2C, svaki korisnički doživljaj definira [pravila](active-directory-b2c-reference-policies.md). Ovaj uzorak koda sadrži tri identiteta sučelja: registracija za Office, prijavite se i uređivanje profila. Morate stvoriti pravila za svaku vrstu, kao što je opisano u [članku referenca pravila](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy). Kada stvorite tri pravila, ne zaboravite:

- Odaberite **Korisnički ID za prijavu** ili **e-pošte registracije** plohu davatelji identiteta.
- Odaberite **zaslonsko ime** i ostale registracije atribute Pravilnik za prijavu.
- Odaberite **zaslonsko ime** i **ID objekta** zahtjevima kao zahtjevima za aplikacije za svaku pravila. Možete odabrati druge zahtjevima.
- Nakon stvaranja, kopirajte **naziv** svakog pravila. Treba imati prefiks `b2c_1_`.  Morat ćete te nazive pravila kasnije.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Kada uspješno stvorite tri pravila, spremni ste za stvaranje aplikacije.

## <a name="download-the-code"></a>Preuzimanje kod

Kod za ovaj Praktični vodič [je spremljen na GitHub](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet). Da biste sastavili uzorka tijekom rada, možete [preuzeti skeleton projekt kao .zip datoteku](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/skeleton.zip). Mogu Kloniraj i na skeleton:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet.git
```

Aplikaciju dovršene nije [dostupan kao .zip datoteku](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip) ili u `complete` podružnice u istom spremištu.

Kada preuzmete uzorak koda, otvorite datoteku .sln Visual Studio za početak rada. U `TaskClient` je WPF stolna računala koje korisnik stupi u interakciju s projekt. Za potrebe ovog praktičnog vodiča, poziva zadatak pozadinske web API-JA, smješten u Azure kojoj su pohranjeni popis zadataka za svakog korisnika.  Nije potrebno da biste sastavili API na webu, već ga radi za vas.

Da biste saznali kako web-mjesto API sigurno potvrđuje zahtjeve pomoću Azure AD B2C, pogledajte [članak API na webu Uvod u rad](active-directory-b2c-devquickstarts-api-dotnet.md).

## <a name="execute-policies"></a>Izvršavanje pravila
Pokrenite aplikaciju komunicira s Azure AD B2C slanjem poruka provjere autentičnosti koje navode pravilnik žele izvršiti kao dio HTTP zahtjev. Za .NET aplikacije za stolna računala možete koristiti pretpregled Microsoft provjera autentičnosti biblioteke (MSAL) da biste poslali poruke OAuth 2.0 provjeru autentičnosti, pravila za izvršavanje te pronađite tokeni koje mogu pozivati web API-ji.

### <a name="install-msal"></a>Instalacija MSAL
Dodavanje MSAL da biste na `TaskClient` projekt pomoću konzole za Visual Studio paketa Manager.

```
PM> Install-Package Microsoft.Identity.Client -IncludePrerelease
```

### <a name="enter-your-b2c-details"></a>Unesite pojedinosti B2C
Otvorite datoteku `Globals.cs` i zamijeniti svih vrijednosti nekretnina vlastitu. Klase se koristi u `TaskClient` reference korištene vrijednosti.

```C#
public static class Globals
{
    ...

    // TODO: Replace these five default with your own configuration values
    public static string tenant = "fabrikamb2c.onmicrosoft.com";
    public static string clientId = "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6";
    public static string signInPolicy = "b2c_1_sign_in";
    public static string signUpPolicy = "b2c_1_sign_up";
    public static string editProfilePolicy = "b2c_1_edit_profile";

    ...
}
```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]


### <a name="create-the-publicclientapplication"></a>Stvaranje na PublicClientApplication
Primarni klasa MSAL je `PublicClientApplication`. Klase predstavlja vaše aplikacije u sustavu za Azure AD B2C. Prilikom stvaranja initalizes aplikacije instancu `PublicClientApplication` u `MainWindow.xaml.cs`. To se može koristiti u prozoru.

```C#
protected async override void OnInitialized(EventArgs e)
{
    base.OnInitialized(e);

    pca = new PublicClientApplication(Globals.clientId)
    {
        // MSAL implements an in-memory cache by default.  Since we want tokens to persist when the user closes the app, 
        // we've extended the MSAL TokenCache and created a simple FileCache in this app.
        UserTokenCache = new FileCache(),
    };
    
    ...
```

### <a name="initiate-a-sign-up-flow"></a>Pokretanje registracije tijek
Kada korisnik odabere predznaku prema gore, želite započeti registracije tijek koje koristi pravila za prijavu koju ste stvorili. Pomoću MSAL jednostavno zovete `pca.AcquireTokenAsync(...)`. Parametri prenesite na `AcquireTokenAsync(...)` odrediti koje token primite, pravila koriste u zahtjev za provjeru autentičnosti i više.

```C#
private async void SignUp(object sender, RoutedEventArgs e)
{
    AuthenticationResult result = null;
    try
    {
        // Use the app's clientId here as the scope parameter, indicating that
        // you want a token to the your app's backend web API (represented by
        // the cloud hosted task API).  Use the UiOptions.ForceLogin flag to
        // indicate to MSAL that it should show a sign-up UI no matter what.
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                Globals.signUpPolicy);

        // Upon success, indicate in the app that the user is signed in.
        SignInButton.Visibility = Visibility.Collapsed;
        SignUpButton.Visibility = Visibility.Collapsed;
        EditProfileButton.Visibility = Visibility.Visible;
        SignOutButton.Visibility = Visibility.Visible;

        // When the request completes successfully, you can get user 
        // information from the AuthenticationResult
        UsernameLabel.Content = result.User.Name;

        // After the sign up successfully completes, display the user's To-Do List
        GetTodoList();
    }

    // Handle any exeptions that occurred during execution of the policy.
    catch (MsalException ex)
    {
        if (ex.ErrorCode != "authentication_canceled")
        {
            // An unexpected error occurred.
            string message = ex.Message;
            if (ex.InnerException != null)
            {
                message += "Inner Exception : " + ex.InnerException.Message;
            }

            MessageBox.Show(message);
        }

        return;
    }
}
```

### <a name="initiate-a-sign-in-flow"></a>Pokretanje tijeka za prijavu
Možete pokrenuti tijek za prijavu na isti način kao i kada pokrenete registracije tijek. Prilikom prijave, provjerite isti poziv MSAL, ovaj put pomoću pravila za prijavu:

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
    AuthenticationResult result = null;
    try
    {
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                    string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                    Globals.signInPolicy);
        ...
```

### <a name="initiate-an-edit-profile-flow"></a>Pokretanje programa tijek Uređivanje profila
Ponovno možete izvršiti Pravilnik za uređivanje profila na isti način:

```C#
private async void EditProfile(object sender, RoutedEventArgs e)
{
    AuthenticationResult result = null;
    try
    {
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                    string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                    Globals.editProfilePolicy);
```

U svim tim slučajevima MSAL ili vraća token u `AuthenticationResult` ili throws iznimka. Svaki put kada primite token od MSAL, možete koristiti u `AuthenticationResult.User` objekt da biste ažurirali korisničkih podataka u aplikaciji, primjerice korisničkog Sučelja. ADAL predmemorira i tokena za korištenje na druge dijelove aplikacija.


### <a name="check-for-tokens-on-app-start"></a>Provjeri tokeni pri otvaranju aplikacije
Da biste pratili korisnikove prijave stanja možete koristiti i MSAL.  U aplikaciji, želimo korisnika da biste ostali prijavljeni čak i kada se zatvoriti aplikaciju i ponovno je otvorite.  Ponovno unutar na `OnInitialized` nadjačati, koristite MSAL na `AcquireTokenSilent` način da biste provjerili predmemorirani tokena:

```C#
AuthenticationResult result = null;
try
{
    // If the user has has a token cached with any policy, we'll display them as signed-in.
    TokenCacheItem tci = pca.UserTokenCache.ReadItems(Globals.clientId).Where(i => i.Scope.Contains(Globals.clientId) && !string.IsNullOrEmpty(i.Policy)).FirstOrDefault();
    string existingPolicy = tci == null ? null : tci.Policy;
    result = await pca.AcquireTokenSilentAsync(new string[] { Globals.clientId }, string.Empty, Globals.authority, existingPolicy, false);

    SignInButton.Visibility = Visibility.Collapsed;
    SignUpButton.Visibility = Visibility.Collapsed;
    EditProfileButton.Visibility = Visibility.Visible;
    SignOutButton.Visibility = Visibility.Visible;
    UsernameLabel.Content = result.User.Name;
    GetTodoList();
}
catch (MsalException ex)
{
    if (ex.ErrorCode == "failed_to_acquire_token_silently")
    {
        // There are no tokens in the cache.  Proceed without calling the To Do list service.
    }
    else
    {
        // An unexpected error occurred.
        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }
        MessageBox.Show(message);
    }
    return;
}
```

## <a name="call-the-task-api"></a>Pozivanje zadatka API-JA
Sada ste koristili MSAL za izvođenje pravila i dobiti tokena.  Kada želite koristiti jedan tokena da biste nazvali zadatak API ponovno pomoću MSAL na `AcquireTokenSilent` način da biste provjerili predmemorirani tokena:

```C#
private async void GetTodoList()
{
    AuthenticationResult result = null;
    try
    {
        // Here we want to check for a cached token, independent of whatever policy was used to acquire it.
        TokenCacheItem tci = pca.UserTokenCache.ReadItems(Globals.clientId).Where(i => i.Scope.Contains(Globals.clientId) && !string.IsNullOrEmpty(i.Policy)).FirstOrDefault();
        string existingPolicy = tci == null ? null : tci.Policy;

        // Use AcquireTokenSilent to indicate that MSAL should throw an exception if a token cannot be acquired
        result = await pca.AcquireTokenSilentAsync(new string[] { Globals.clientId }, string.Empty, Globals.authority, existingPolicy, false);

    }
    // If a token could not be acquired silently, we'll catch the exception and show the user a message.
    catch (MsalException ex)
    {
        // There is no access token in the cache, so prompt the user to sign-in.
        if (ex.ErrorCode == "failed_to_acquire_token_silently")
        {
            MessageBox.Show("Please sign up or sign in first");
            SignInButton.Visibility = Visibility.Visible;
            SignUpButton.Visibility = Visibility.Visible;
            EditProfileButton.Visibility = Visibility.Collapsed;
            SignOutButton.Visibility = Visibility.Collapsed;
            UsernameLabel.Content = string.Empty;
        }
        else
        {
            // An unexpected error occurred.
            string message = ex.Message;
            if (ex.InnerException != null)
            {
                message += "Inner Exception : " + ex.InnerException.Message;
            }
            MessageBox.Show(message);
        }

        return;
    }
    ...
```

Kada poziv `AcquireTokenSilentAsync(...)` uspješnog i token nalazi se u predmemoriji, možete dodati simbol na `Authorization` zaglavlje HTTP zahtjev. Pomoću tog zaglavlja web zadatka API će autentičnost zahtjevom za čitanje popis obaveza korisnika:

```C#
    ...
    // Once the token has been returned by MSAL, add it to the http authorization header, before making the call to access the To Do list service.
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.Token);

    // Call the To Do list service.
    HttpResponseMessage response = await httpClient.GetAsync(Globals.taskServiceUrl + "/api/tasks");
    ...
```

## <a name="sign-the-user-out"></a>Odjava korisnika
Na kraju, možete koristiti MSAL da biste završili sesiju korisnika s aplikacijom kada korisnik odabere **Odjava**.  Prilikom korištenja MSAL, to će se postiže poništavanjem svih tokena iz predmemorije tokena:

```C#
private void SignOut(object sender, RoutedEventArgs e)
{
    // Clear any remnants of the user's session.
    pca.UserTokenCache.Clear(Globals.clientId);

    // This is a helper method that clears browser cookies in the browser control that MSAL uses, it is not part of MSAL.
    ClearCookies();

    // Update the UI to show the user as signed out.
    TaskList.ItemsSource = string.Empty;
    SignInButton.Visibility = Visibility.Visible;
    SignUpButton.Visibility = Visibility.Visible;
    EditProfileButton.Visibility = Visibility.Collapsed;
    SignOutButton.Visibility = Visibility.Collapsed;
    return;
}
```

## <a name="run-the-sample-app"></a>Pokrenite aplikaciju uzorka

Na kraju, stvaranje i pokretanje uzorka.  Prijavite se za aplikaciju pomoću programa e-pošte adresa ili korisničko ime. Odjava i ponovno prijavite istog korisnika. Uređivanje profila za tog korisnika. Odjavljivanje i prijavite se pomoću nekog drugog korisnika.

## <a name="add-social-idps"></a>Dodavanje društvene IDPs

Aplikaciju trenutno podržava samo korisnik prijave i prijavu koja koristi **Lokalni računi**. To su računi pohranjena u direktoriju B2C koje koriste korisničko ime i lozinku. Pomoću Azure AD B2C možete dodati podrška za drugi davatelji identiteta (IDPs) bez promjena bilo kod.

Da biste dodali društvenih IDPs aplikacije, započnite slijedeći detaljne upute u sljedećim člancima. Za svaku IDP želite podržava, morate registrirati aplikacije u tom sustavu i dobiti ID-a za klijenta

- [Postavljanje servisa Facebook kao programa IDP](active-directory-b2c-setup-fb-app.md)
- [Postavljanje Google kao programa IDP](active-directory-b2c-setup-goog-app.md)
- [Postavljanje Amazon kao programa IDP](active-directory-b2c-setup-amzn-app.md)
- [Postavljanje servisa LinkedIn kao programa IDP](active-directory-b2c-setup-li-app.md)

Nakon što dodate davatelji identiteta B2C direktorija, morate je uređivati svaki od tri police da biste dodali novi IDPs kao što je opisano u [članku referenca pravila](active-directory-b2c-reference-policies.md). Nakon što spremite police, ponovno pokrenite aplikaciju. Trebali biste vidjeti novi IDPs dodani kao prijave i registracije mogućnosti u svakoj od vašeg identiteta iskustvo.

Možete Eksperimentirajte s pravilima i pridržavajte utjecaju na aplikaciju uzorka. Dodavanje ili uklanjanje IDPs, rukovati zahtjevima aplikacije ili promijenite atribute za prijavu. Eksperimentirajte dok se ne može vidjeti kako pravilnika, zahtjevi za provjeru autentičnosti i MSAL sve zajedno.

Za referencu, dovršene [prikazuje se kao .zip datoteku](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip). Mogu Kloniraj i to iz GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet.git```
