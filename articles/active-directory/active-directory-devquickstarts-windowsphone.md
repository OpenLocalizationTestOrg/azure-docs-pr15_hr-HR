<properties
    pageTitle="Azure AD Windows telefon Uvod | Microsoft Azure"
    description="Sastavljanje aplikacija za Windows Phone koja integrira Azure AD za prijavu i poziva Azure AD zaštićen API-ji pomoću OAuth."
    services="active-directory"
    documentationCenter="windows"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>



# <a name="integrate-azure-ad-with-a-windows-phone-app"></a>Integrirati Azure AD za Windows Phone aplikacija

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Ako razvijate aplikacija za Windows Phone 8.1, Azure AD olakšava jednostavne i jasan za provjeru autentičnosti korisnika s računa za Active Directory.  Omogućuje i aplikacije da biste sigurno trošiti sve API zaštićen Azure AD, kao što su API-ji sa sustavom Office 365 ili Azure API na webu.

> [AZURE.NOTE] Ovaj uzorak koda koristi ADAL 2.0.  Za najnovije tehnologiju, trebali biste umjesto isprobajte naše [Windows Universal praktičnom vodiču pomoću ADAL v3.0](active-directory-devquickstarts-windowsstore.md).  Ako se uistinu izgradnja aplikacije za Windows Phone 8.1, to su na pravom mjestu.  ADAL 2.0 i dalje u potpunosti podržane, a preporučeni način od razvoj aplikacija agianst Windows Phone 8.1 koristi Azure AD.

.NET nativni klijenti koje moraju imati pristup zaštićeni resursa, Azure AD predstavlja provjera autentičnosti biblioteke imenika Active Directory ili ADAL.  ADAL-isključivom svrhom u životu je da biste olakšali aplikacije da biste dobili pristup tokena.  Da bismo pokazali samo kako je lako, ovdje bismo ćete stvoriti na "direktorija Searcher" aplikacija za Windows Phone 8.1 koji:

-   Uzima pristupiti tokeni za pozivanje API Azure AD grafikonu [OAuth 2.0 protokol provjere autentičnosti](https://msdn.microsoft.com/library/azure/dn645545.aspx).
-   Pretražuje direktorij za korisnike s određenom UPN.
-   Znak korisnika izgleda.

Da biste sastavili dovršeno aplikacije raditi, morat ćete:

2. Registracija aplikacije s Azure AD.
3. Instaliranje i konfiguriranje ADAL.
5. Da biste dobili tokena iz Azure AD pomoću ADAL.

Da biste počeli, [Preuzmite skeleton projekt](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/skeleton.zip) ili [preuzeti dovršene uzorka](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/complete.zip).  Svaka je rješenje za Visual Studio 2013.  Trebat ćete klijent za Azure AD Dodavanje korisnika i Registracija aplikacije.  Ako još nemate klijent, [Saznajte kako da biste ga nabavili](active-directory-howto-tenant.md).

## <a name="1-register-the-directory-searcher-application"></a>*1. registrirati Searcher aplikacije direktorija*
Da biste omogućili aplikacije da biste dobili tokena, ćete najprije morate registrirati u klijentu za Azure AD i dati dozvolu za pristup grafikonu Azure AD API-JA:

-   Prijavite se na [Portal za upravljanje Azure](https://manage.windowsazure.com)
-   U navigacijskom oknu lijevoj strani kliknite **Servisa Active Directory**
-   Odaberite klijenta u kojoj se registrirati aplikaciju.
-   Kliknite karticu **aplikacije** pa kliknite **Dodaj** u donjem ladica.
-   Slijedite upite i stvorite novu **Nativni klijentska aplikacija**.
    -   **Naziv** aplikacije opisane su aplikacije krajnjim korisnicima
    -   Kombinacija sheme i niz Azure AD pomoću kojega će vratiti tokena odgovore je **Preusmjeravanje Uri** .  Unesite vrijednost rezervirano mjesto za sada, npr. `http://DirectorySearcher`.  Ne možemo zamijenit ćete tu vrijednost kasnije.
-   Nakon što ste dovršili registraciju, AAD će dodijeliti aplikacije klijent Jedinstveni identifikator.  Potreban vam je tu vrijednost u sljedećim odjeljcima tako da ga kopirali na kartici **Konfiguriraj** .
- I na kartici **Konfiguriraj** pronađite odjeljak "Dozvole za ostale aplikacije".  Aplikacija "Azure Active Directory" dodajte dozvolu **pristupa vaše imenik tvrtke ili ustanove** u odjeljku **Dodijeliti dozvole**.  To će omogućiti aplikacije da biste upit grafikonu API-JA za korisnike.

## <a name="2-install--configure-adal"></a>*2. instalirati i konfigurirati ADAL*
Sad kad ste aplikacije u Azure AD, možete instalirati ADAL i svoj identitet vezanih kod.  ADAL da biste mogli komunicirati s Azure AD, morate pružiti neke informacije o registraciji vaše aplikacije.
-   Započnite dodavanjem ADAL projekt DirectorySearcher korištenju konzole za Upravitelj paketa.

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

-   U programu project DirectorySearcher otvorite `MainPage.xaml.cs`.  Zamjena vrijednosti u na `Config Values` regija u skladu s vizualnim vrijednosti unos u Azure Portal.  Kod pozivati te vrijednosti svaki put kada se koristi ADAL.
    -   U `tenant` je domena Azure AD klijent, npr. contoso.onmicrosoft.com
    -   U `clientId` je clientId aplikaciju koju ste kopirali iz portalu.
-   Sada morate da biste otkrili povratnog uri za aplikaciju Windows Phone.  Postavljanje programa točku prekida u tom retku u na `MainPage` metoda:

```
redirectURI = Windows.Security.Authentication.Web.WebAuthenticationBroker.GetCurrentApplicationCallbackUri();
```
- Pokrenite aplikaciju i kopirati odvojite vrijednost `redirectUri` kada je kliknete na točku prekida.  Ona bi trebala sličiti

```
ms-app://s-1-15-2-1352796503-54529114-405753024-3540103335-3203256200-511895534-1429095407/
```

- Na kartici **Konfiguriranje** aplikacije na portalu za upravljanje Azure, zamijenite vrijednost **RedirectUri** tu vrijednost.  

## <a name="3--use-adal-to-get-tokens-from-aad"></a>*3. ADAL da biste pristupili slijedite tokena iz AAD*
Osnovno načelo iza ADAL je da svaki put kada aplikacije potreban token za pristup, jednostavno pozove `authContext.AcquireToken(…)`, a ADAL ne ostale.  

-   Prvi je korak za pokretanje aplikacije programa `AuthenticationContext` -ADAL je primarni predmete.  Ovo je gdje proći ADAL koordinate potrebne za komunikaciju s Azure AD i recite im kako predmemoriju tokena.

```C#
public MainPage()
{
    ...

    // ADAL for Windows Phone 8.1 builds AuthenticationContext instances through a factory
    authContext = AuthenticationContext.CreateAsync(authority).GetResults();
}
```

- Potražite na `Search(...)` metodu koja će se pozivaju kada gumb cliks korisnika "Pretraživanje" u korisničkom Sučelju aplikacije.  Ta metoda čini zahtjevom GET API Azure AD grafikonu upit za korisnike čije UPN počinje s određenom traženi pojam.  No da bi upit API grafikon, morate unijeti u access_token u na `Authorization` zaglavlja zahtjeva – to je Odakle dolaze ADAL.

```C#
private async void Search(object sender, RoutedEventArgs e)
{
    ...

    // Try to get a token without triggering any user prompt.
    // ADAL will check whether the requested token is in ADAL's token cache or can otherwise be obtained without user interaction.
    AuthenticationResult result = await authContext.AcquireTokenSilentAsync(graphResourceId, clientId);
    if (result != null && result.Status == AuthenticationStatus.Success)
    {
        // A token was successfully retrieved.
        QueryGraph(result);
    }
    else
    {
        // Acquiring a token without user interaction was not possible.
        // Trigger an authentication experience and specify that once a token has been obtained the QueryGraph method should be called
        authContext.AcquireTokenAndContinue(graphResourceId, clientId, redirectURI, QueryGraph);
    }
}
```
- Ako interaktivne provjeru autentičnosti nije potrebno, ADAL će koristiti Windows Phone Web provjere autentičnosti Broker (WAB) i [model u nastavku](http://www.cloudidentity.com/blog/2014/06/16/adal-for-windows-phone-8-1-deep-dive/) da biste prikazali Azure AD stranici za prijavu.  Kada korisnik prijavi, pokrenite aplikaciju mora proći ADAL rezultate WAB interakciju.  Ovo je lako i svodi implementacije u `ContinueWebAuthentication` sučelja:

```C#
// This method is automatically invoked when the application
// is reactivated after an authentication interaction through WebAuthenticationBroker.
public async void ContinueWebAuthentication(WebAuthenticationBrokerContinuationEventArgs args)
{
    // pass the authentication interaction results to ADAL, which will
    // conclude the token acquisition operation and invoke the callback specified in AcquireTokenAndContinue.
    await authContext.ContinueAcquireTokenAsync(args);
}
```

- Sada je vrijeme da biste koristili u `AuthenticationResult` koje vraćaju ADAL aplikacije.  U na `QueryGraph(...)` povratnog, priložite access_token koju ste nabavili zahtjev za početak u zaglavlju autorizacije:

```C#
private async void QueryGraph(AuthenticationResult result)
{
    if (result.Status != AuthenticationStatus.Success)
    {
        MessageDialog dialog = new MessageDialog(string.Format("If the error continues, please contact your administrator.\n\nError: {0}\n\nError Description:\n\n{1}", result.Error, result.ErrorDescription), "Sorry, an error occurred while signing you in.");
        await dialog.ShowAsync();
    }

    // Add the access token to the Authorization Header of the call to the Graph API, and call the Graph API.
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

    ...
}
```
- Možete koristiti u `AuthenticationResult` objekt koji želite prikazati informacije o korisniku u svojoj aplikaciji. U na `QueryGraph(...)` metoda koristiti rezultat da biste prikazali id korisnika na stranici:

```C#
// Update the Page UI to represent the signed in user
ActiveUser.Text = result.UserInfo.DisplayableId;
```
- Na kraju, možete koristiti ADAL Prijava korisnika iz u aplikaciji.  Želimo kad korisnik klikne gumb "Odjava", provjerite sljedeće poziv `AcquireTokenSilentAsync(...)` neće uspjeti.  S ADAL, to je dovoljno očistite predmemoriju tokena:

```C#
private void SignOut()
{
    // Clear session state from the token cache.
    authContext.TokenCache.Clear();

    ...
}
```

Čestitamo! Sada imate rad aplikaciju za Windows Phone koja sadrži mogućnost provjeru autentičnosti korisnika, sigurno poziva Web API-ji pomoću OAuth 2.0, a osnovne informacije o korisniku.  Ako to već niste učinili, sada je vrijeme za popunjavanje vaš klijent s nekim korisnicima.  Pokrenite aplikaciju DirectorySearcher i prijavite pomoću nekog od tih korisnika.  Traženje drugih korisnika na temelju njihove UPN-a.  Zatvorite aplikaciju i ponovno ga pokrenuti.  Obratite pozornost na to kako sesija ostaje netaknuta.  Odjava i ponovno prijavite i mjehuričaste grafikone.

ADAL olakšava sve ove značajke uobičajenih identiteta ugraditi u aplikaciji.  Ga brine sve poslovne dirty umjesto vas - upravljanje predmemorije, podrška OAuth protokola izlaganje korisnika s Prijava korisničko Sučelje, osvježavanje istekle tokeni i drugo.  Sve što trebate znati je jedan poziv API `authContext.AcquireToken*(…)`.

Za referencu, dovršeni uzorka (bez konfiguracijskih vrijednosti) navedeni su [u nastavku](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/complete.zip).  Sada pomicati po na scenarije dodatne identitet.  Preporučujemo vam da biste isprobali:

[Sigurne .NET Web API-JA s Azure AD >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]