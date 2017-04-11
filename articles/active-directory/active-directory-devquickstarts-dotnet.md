<properties
    pageTitle=".NET Azure AD Uvod | Microsoft Azure"
    description="Sastavljanje aplikacija za radnu površinu sustava Windows .NET koja integrira Azure AD za prijavu i poziva Azure AD zaštićen API-ji pomoću OAuth."
    services="active-directory"
    documentationCenter=".net"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>


# <a name="integrate-azure-ad-into-a-windows-desktop-wpf-app"></a>Integriranje Azure AD u aplikaciju za radnu površinu WPF za Windows

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Ako razvijate aplikaciji za stolna računala, Azure AD olakšava jednostavne i jasan za provjeru autentičnosti korisnika s računa za Active Directory.  Omogućuje i aplikacije da biste sigurno zauzeti sve API zaštićen Azure AD, kao što su API-ji sa sustavom Office 365 ili Azure API na webu.

.NET nativni klijenti koje moraju imati pristup zaštićeni resursa, Azure AD predstavlja provjera autentičnosti biblioteke imenika Active Directory ili ADAL.  ADAL-isključivom svrhom u život je da biste olakšali aplikacije da biste dobili pristup tokena.  Da bismo pokazali samo kako je lako, ovdje bismo ćete stvoriti popis zadataka za .NET WPF aplikacije koje:

-   Uzima pristupiti tokeni za pozivanje API Azure AD grafikonu [OAuth 2.0 protokol provjere autentičnosti](https://msdn.microsoft.com/library/azure/dn645545.aspx).
-   Pretražuje direktorija za korisnike s Navedeni pseudonim.
-   Znak korisnika izgleda.

Da biste sastavili potpuni aplikacije raditi, morat ćete:

2. Registracija aplikacije s Azure AD.
3. Instaliranje i konfiguriranje ADAL.
5. Da biste dobili tokena iz Azure AD pomoću ADAL.

Da biste počeli, [Preuzmite skeleton aplikacije](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/skeleton.zip) ili [preuzeti dovršene uzorka](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).  Trebat ćete klijent za Azure AD Dodavanje korisnika i Registracija aplikacije.  Ako još nemate klijent, [Saznajte kako da biste ga nabavili](active-directory-howto-tenant.md).

## <a name="1-register-the-directorysearcher-application"></a>*1. registrirati DirectorySearcher aplikacije*
Da biste omogućili aplikacije da biste dobili tokena, ćete najprije morate registrirati u klijentu za Azure AD i dati dozvolu za pristup grafikonu Azure AD API-JA:

-   Prijavite se na Portal za upravljanje Azure
-   U navigacijskom oknu lijevoj strani kliknite **Servisa Active Directory**
-   Odaberite klijenta u kojoj se registrirati aplikaciju.
-   Kliknite karticu **aplikacije** pa kliknite **Dodaj** u donjem ladica.
-   Slijedite upite i stvorite novu **Nativni klijentska aplikacija**.
    -   **Naziv** aplikacije opisane su aplikacije krajnjim korisnicima
    -   Kombinacija sheme i niz Azure AD pomoću kojega će vratiti tokena odgovore je **Preusmjeravanje Uri** .  Unesite vrijednost određene aplikacije, npr. `http://DirectorySearcher`.
-   Nakon što ste dovršili registraciju, AAD će dodijeliti aplikacije identifikatora jedinstvene klijenta.  Potreban vam je tu vrijednost u sljedećim odjeljcima tako da ga kopirali na kartici **Konfiguriraj** .
- I na kartici **Konfiguriraj** pronađite odjeljak "Dozvole za ostale programe".  Aplikacija "Azure Active Directory" dodajte dozvolu **pristupa vaše imenik tvrtke ili ustanove** u odjeljku **Dodijeliti dozvole**.  To će omogućiti aplikacije da biste upit grafikonu API-JA za korisnike.

## <a name="2-install--configure-adal"></a>*2. instalirati i konfigurirati ADAL*
Sad kad ste aplikacije u Azure AD, možete instalirati ADAL i svoj identitet vezanih kod.  ADAL da biste mogli komunicirati s Azure AD, morate pružiti neke informacije o registraciji vaše aplikacije.
-   Započnite dodavanjem ADAL DirectorySearcher projekt pomoću konzole za Upravitelj paketa.

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

-   U programu project DirectorySearcher otvorite `app.config`.  Zamjena vrijednosti elemenata u na `<appSettings>` sekciju u skladu s vizualnim vrijednosti unos u Azure Portal.  Kod pozivati te vrijednosti svaki put kada se koristi ADAL.
    -   U `ida:Tenant` je domena Azure AD klijent, npr. contoso.onmicrosoft.com
    -   U `ida:ClientId` je clientId aplikaciju koju ste kopirali iz portalu.
    -   U `ida:RedirectUri` je preusmjeravanje URL-a registrirana na portalu.

## <a name="3--use-adal-to-get-tokens-from-aad"></a>*3. ADAL da biste pristupili slijedite tokena iz AAD*
Osnovno načelo iza ADAL je da svaki put kada aplikacije potreban token za pristup, jednostavno poziva `authContext.AcquireTokenAsync(...)`, a ADAL ne ostale.  

-   U na `DirectorySearcher` projekt, otvara `MainWindow.xaml.cs` i pronađite u `MainWindow()` način.  Prvi je korak za pokretanje aplikacije programa `AuthenticationContext` -ADAL je primarni klasu.  Ovo je gdje proći ADAL koordinate potrebne za komunikaciju s Azure AD i recite im kako predmemoriju tokena.

```C#
public MainWindow()
{
    InitializeComponent();

    authContext = new AuthenticationContext(authority, new FileCache());

    CheckForCachedToken();
}
```

- Potražite na `Search(...)` metodu koja će se pozivaju kada gumb cliks korisnika "Pretraživanje" u korisničkom Sučelju aplikacije.  Ta metoda čini zahtjevom GET API Azure AD grafikonu upit za korisnike čije UPN počinje s određenom traženi pojam.  No da bi upit API grafikonu, morate unijeti u access_token u na `Authorization` zaglavlje zahtjeva – to je Odakle dolaze ADAL.

```C#
private async void Search(object sender, RoutedEventArgs e)
{
    // Validate the Input String
    if (string.IsNullOrEmpty(SearchText.Text))
    {
        MessageBox.Show("Please enter a value for the To Do item name");
        return;
    }

    // Get an Access Token for the Graph API
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Auto));
        UserNameLabel.Content = result.UserInfo.DisplayableId;
        SignOutButton.Visibility = Visibility.Visible;
    }
    catch (AdalException ex)
    {
        // An unexpected error occurred, or user canceled the sign in.
        if (ex.ErrorCode != "access_denied")
            MessageBox.Show(ex.Message);

        return;
    }

    ...
}
```
- Kada aplikacije zahtjeve token tako da nazovete `AcquireTokenAsync(...)`, ADAL će pokušati vratili token bez korisnika traži vjerodajnice.  Ako ADAL određuje da korisnik nije potrebno prijaviti da biste dobili token, on će prikazati dijaloški okvir za prijavu, prikupljanje korisnikove vjerodajnice i vratite token nakon uspješne provjere autentičnosti.  Ako je ADAL ne mogu vratiti token iz bilo kojeg razloga će vratiti na `AdalException`.
- Primijetit ćete da se `AuthenticationResult` objekt sadrži na `UserInfo` objekt koji se može koristiti za prikupljanje informacija morati aplikacije.  U DirectorySearcher, `UserInfo` služi za prilagodbu korisničkog Sučelja aplikacije s id korisnika.

- Želimo kad korisnik klikne gumb "Odjava", provjerite sljedeće poziv `AcquireTokenAsync(...)` će uputite korisnika da se prijavite.  S ADAL, to je dovoljno očistite predmemoriju tokena:

```C#
private void SignOut(object sender = null, RoutedEventArgs args = null)
{
    // Clear the token cache
    authContext.TokenCache.Clear();

    ...
}
```

- Međutim, ako korisnik kliknete gumb "Odjava", će želite da biste zadržali sesija za buduću upotrebu su pokrenuti na DirectorySearcher.  Kada se pokreće aplikaciju, možete provjeriti ADAL-tokena predmemoriju za postojeće token i sukladno tome ažuriranje korisničkog Sučelja.  U na `CheckForCachedToken()` način, provjerite drugi poziv `AcquireTokenAsync(...)`, ovaj put prosljeđivanje u na `PromptBehavior.Never` parametar.  `PromptBehavior.Never`ADAL će se obavještava da korisnik mora zatražiti za prijavu, a ADAL umjesto toga treba vratiti iznimku ako se ne mogu vratiti token.

```C#
public async void CheckForCachedToken() 
{
    // As the application starts, try to get an access token without prompting the user.  If one exists, show the user as signed in.
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Never));
    }
    catch (AdalException ex)
    {
        if (ex.ErrorCode != "user_interaction_required")
        {
            // An unexpected error occurred.
            MessageBox.Show(ex.Message);
        }

        // If user interaction is required, proceed to main page without singing the user in.
        return;
    }

    // A valid token is in the cache
    SignOutButton.Visibility = Visibility.Visible;
    UserNameLabel.Content = result.UserInfo.DisplayableId;
}
```

Čestitamo! Sada imate rad .NET WPF aplikaciju koja je mogućnost provjeru autentičnosti korisnika, sigurno poziva Web API-ji pomoću OAuth 2.0, a osnovne informacije o korisniku.  Ako to već niste učinili, sada je vrijeme za popunjavanje vaš klijent s nekim korisnicima.  Pokrenite aplikaciju DirectorySearcher i prijavite pomoću nekog od tih korisnika.  Traženje drugih korisnika na temelju njihove UPN-a.  Zatvorite aplikaciju i ponovno ga pokrenuti.  Obratite pozornost na to kako sesija ostaje netaknuta.  Odjava i ponovno prijavite i mjehuričaste grafikone.

ADAL olakšava sve ove značajke uobičajenih identiteta ugraditi u aplikaciji.  Ga brine sve poslovne dirty umjesto vas - upravljanje predmemorije, podrška OAuth protokola izlaganje korisnika s Prijava korisničko Sučelje, osvježavanje istekle tokeni i drugo.  Sve što trebate znati je jedan poziv API `authContext.AcquireTokenAsync(...)`.

Za referencu, dovršeni uzorka (bez konfiguracijskih vrijednosti) navedeni su [u nastavku](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).  Možete odmah premjestiti dodatne scenarije.  Preporučujemo vam da biste isprobali:

[Sigurne .NET Web API-JA s Azure AD >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
