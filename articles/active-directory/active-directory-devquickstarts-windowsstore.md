<properties
    pageTitle="Iz trgovine Windows Azure AD Uvod | Microsoft Azure"
    description="Sastavljanje aplikacija iz Windows trgovine koja integrira Azure AD za prijavu i poziva Azure AD zaštićen API-ji pomoću OAuth."
    services="active-directory"
    documentationCenter="windows"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>


# <a name="integrate-azure-ad-with-a-windows-store-app"></a>Azure AD integrirane aplikacije iz Windows trgovine

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Ako razvijate aplikacije za iz trgovine Windows Azure AD olakšava jednostavne i jasan za provjeru autentičnosti korisnika s računa za Active Directory.  Omogućuje i aplikacije da biste sigurno zauzeti sve API zaštićen Azure AD, kao što su API-ji sa sustavom Office 365 ili Azure API na webu.

Iz Windows trgovine aplikacija za stolna računala koje je potrebno da biste pristupili zaštićeni resursima, Azure AD predstavlja provjera autentičnosti biblioteke imenika Active Directory ili ADAL.  ADAL-isključivom svrhom u život je da biste olakšali aplikacije da biste dobili pristup tokena.  Da bismo pokazali samo kako je lako, ovdje bismo ćete stvoriti na "Direktorija Searcher" iz Windows trgovine koji:

-   Uzima pristupiti tokeni za pozivanje API Azure AD grafikonu [OAuth 2.0 protokol provjere autentičnosti](https://msdn.microsoft.com/library/azure/dn645545.aspx).
-   Pretražuje direktorija za korisnike s određenom UPN.
-   Znak korisnika izgleda.

Da biste sastavili dovršeno radu aplikacije, morat ćete:

2. Registracija aplikacije s Azure AD.
3. Instaliranje i konfiguriranje ADAL.
5. Da biste dobili tokena iz Azure AD pomoću ADAL.

Da biste počeli, [Preuzmite skeleton projekt](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/skeleton.zip) ili [preuzeti dovršene uzorka](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip).  Svaka je rješenje za Visual Studio 2015.  Trebat ćete klijent za Azure AD Dodavanje korisnika i Registracija aplikacije.  Ako još nemate klijent, [Saznajte kako da biste ga nabavili](active-directory-howto-tenant.md).

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
- I na kartici **Konfiguriraj** pronađite odjeljak "Dozvole za ostale programe".  Aplikacija "Azure Active Directory" dodajte pravo **pristupa directory kao korisnik prijavljeni u** odjeljku **Dodijeliti dozvole**.  To će omogućiti aplikacije da biste upit grafikonu API-JA za korisnike.

## <a name="2-install--configure-adal"></a>*2. instalirati i konfigurirati ADAL*
Sad kad ste aplikacije u Azure AD, možete instalirati ADAL i svoj identitet vezanih kod.  ADAL da biste mogli komunicirati s Azure AD, morate pružiti neke informacije o registraciji vaše aplikacije.
-   Započnite dodavanjem ADAL DirectorySearcher projekt pomoću konzole za Upravitelj paketa.

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

-   U programu project DirectorySearcher otvorite `MainPage.xaml.cs`.  Zamjena vrijednosti u na `Config Values` regija u skladu s vizualnim vrijednosti unos u Azure Portal.  Kod pozivati te vrijednosti svaki put kada se koristi ADAL.
    -   U `tenant` je domena Azure AD klijent, npr. contoso.onmicrosoft.com
    -   U `clientId` je clientId aplikaciju koju ste kopirali iz portalu.
-   Sada morate da biste otkrili povratnog uri za Windows Store app.  Postavljanje programa točku prekida u tom retku u na `MainPage` metoda:

```
redirectURI = Windows.Security.Authentication.Web.WebAuthenticationBroker.GetCurrentApplicationCallbackUri();
```
- Stvaranje rješenja, provjerite je li se vraćaju sve reference paketa.  Ako nema paketa, otvorite gore Upravitelj paketa Nuget i vraćanje pakete.
- Pokrenite aplikaciju i kopirati odvojite vrijednost `redirectUri` kada je kliknete na točku prekida.  Ona bi trebala sličiti

```
ms-app://s-1-15-2-1352796503-54529114-405753024-3540103335-3203256200-511895534-1429095407/
```

- Na kartici **Konfiguriranje** aplikacije na portalu za upravljanje Azure, zamijenite vrijednost **RedirectUri** tu vrijednost.  

## <a name="3--use-adal-to-get-tokens-from-aad"></a>*3. ADAL da biste pristupili slijedite tokena iz AAD*
Osnovno načelo iza ADAL je da svaki put kada aplikacije potreban token za pristup, jednostavno poziva `authContext.AcquireToken(…)`, a ADAL ne ostale.  

-   Prvi je korak za pokretanje aplikacije programa `AuthenticationContext` -ADAL je primarni klasu.  Ovo je gdje proći ADAL koordinate potrebne za komunikaciju s Azure AD i recite im kako predmemoriju tokena.

```C#
public MainPage()
{
    ...

    authContext = new AuthenticationContext(authority);
}
```

- Potražite na `Search(...)` metodu koja će pozvani kad korisnik klikne gumb "Pretraživanje" u korisničkom Sučelju aplikacije.  Ta metoda čini zahtjevom GET API Azure AD grafikonu upit za korisnike čije UPN počinje s određenom traženi pojam.  No da bi upit API grafikonu, morate unijeti u access_token u na `Authorization` zaglavlje zahtjeva – to je Odakle dolaze ADAL.

```C#
private async void Search(object sender, RoutedEventArgs e)
{
    ...
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectURI, new PlatformParameters(PromptBehavior.Auto, false));
    }
    catch (AdalException ex)
    {
        if (ex.ErrorCode != "authentication_canceled")
        {
            ShowAuthError(string.Format("If the error continues, please contact your administrator.\n\nError: {0}\n\nError Description:\n\n{1}", ex.ErrorCode, ex.Message));
        }
        return;
    }
    ...
}
```
- Kada aplikacije zahtjeve token tako da nazovete `AcquireTokenAsync(...)`, ADAL će pokušati vratili token bez korisnika traži vjerodajnice.  Ako ADAL određuje da korisnik nije potrebno prijaviti da biste dobili token, on će prikazati dijaloški okvir za prijavu, prikupljanje korisnikove vjerodajnice i vratite token nakon uspješne provjere autentičnosti.  Ako se ne mogu vratiti token iz bilo kojeg razloga ADAL na `AuthenticationResult` status će se pogreška.

- Sada je vrijeme da biste koristili u access_token koju ste upravo nabavili.  Također u na `Search(...)` metoda token priložiti zahtjevu za početak API grafikonu u zaglavlju autorizacije:

```C#
// Add the access token to the Authorization Header of the call to the Graph API, and call the Graph API.
httpClient.DefaultRequestHeaders.Authorization = new HttpCredentialsHeaderValue("Bearer", result.AccessToken);

```
- Možete koristiti u `AuthenticationResult` objekt koji želite prikazati informacije o korisniku u aplikaciju programa, kao što je id korisnika:

```C#
// Update the Page UI to represent the signed in user
ActiveUser.Text = result.UserInfo.DisplayableId;
```
- Na kraju, možete koristiti ADAL Prijava korisnika iz u aplikaciji.  Želimo kad korisnik klikne gumb "Odjava", provjerite sljedeće poziv `AcquireTokenAsync(...)` vode znak u prikazu.  S ADAL, to je dovoljno očistite predmemoriju tokena:

```C#
private void SignOut()
{
    // Clear session state from the token cache.
    authContext.TokenCache.Clear();

    ...
}
```

Čestitamo! Sada imate rad iz Windows trgovine koji ima mogućnost za provjeru autentičnosti korisnika, sigurno poziva Web API-ji pomoću OAuth 2.0, a osnovne informacije o korisniku.  Ako to već niste učinili, sada je vrijeme za popunjavanje vaš klijent s nekim korisnicima.  Pokrenite aplikaciju DirectorySearcher i prijavite pomoću nekog od tih korisnika.  Traženje drugih korisnika na temelju njihove UPN-a.  Zatvorite aplikaciju i ponovno ga pokrenuti.  Obratite pozornost na to kako sesija ostaje netaknuta.  Odjava (klikom desnom tipkom miša da biste prikazali traci pri dnu), a zatim se ponovno prijavite u i mjehuričaste grafikone.

ADAL olakšava sve ove značajke uobičajenih identiteta ugraditi u aplikaciji.  Ga brine sve poslovne dirty umjesto vas - upravljanje predmemorije, podrška OAuth protokola izlaganje korisnika s Prijava korisničko Sučelje, osvježavanje istekle tokeni i drugo.  Sve što trebate znati je jedan poziv API `authContext.AcquireToken*(…)`.

Za referencu, dovršeni uzorka (bez konfiguracijskih vrijednosti) navedeni su [u nastavku](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip).  Sada pomicati po na scenarije dodatne identitet.  Preporučujemo vam da biste isprobali:

[Sigurne .NET Web API-JA s Azure AD >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
