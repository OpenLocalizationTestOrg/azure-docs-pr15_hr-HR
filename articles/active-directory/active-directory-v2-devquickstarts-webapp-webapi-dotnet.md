<properties
    pageTitle="Azure AD 2.0 .NET Web App | Microsoft Azure"
    description="Sastavljanje .NET MVC web-aplikacijama to pozive web-servisa pomoću osobnog Microsoftova računa i raditi ili školske račune za prijavu."
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
    ms.date="10/27/2016"
    ms.author="dastrock"/>

# <a name="calling-a-web-api-from-a-net-web-app"></a>Pozivanje API web-mjesto iz web-aplikacije za .NET

S krajnju točku 2.0 možete brzo dodati provjere autentičnosti na web-aplikacije i web-API-ji s podrška za oba Microsoftovi računi i računi tvrtke ili obrazovne ustanove.  Ovdje, ne možemo izdanja programa MVC web-aplikacije koje se korisnici u korištenje OpenID povezali, uz pomoć s Microsoftovim OWIN proizvod.  Web-aplikaciji će dobiti OAuth 2.0 pristupna tokena za web-mjesto api osigurava OAuth 2.0 koji omogućuje stvaranje, čitanje i brisanje na određenom korisniku "popis obaveza".

Pomoću ovog praktičnog vodiča sadrži uglavnom pomoću MSAL Nabava i korištenje tokena pristupa u web-aplikacijama, što je opisano u potpunu [ovdje](active-directory-v2-flows.md#web-apps).  Kao preduvjeti, preporučujemo vam da najprije Saznajte kako [dodati osnovni Prijava na web-aplikacijama](active-directory-v2-devquickstarts-dotnet-web.md) ili kako [pravilno sigurne API web-mjesto](active-directory-v2-devquickstarts-dotnet-api.md).

> [AZURE.NOTE]
    Neke scenarije servisa Azure Active Directory i značajke podržava krajnju točku 2.0.  Da biste utvrdili koristite krajnju točku 2.0, Saznajte više o [2.0 ograničenja](active-directory-v2-limitations.md).

## <a name="download-sample-code"></a>Preuzimanje uzorak koda

Kod za ovog praktičnog vodiča je spremljen [na GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet).  Da biste pratili, možete [preuzeti aplikacije skeleton kao u .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/skeleton.zip) ili Kloniraj na skeleton:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet.git```

Osim toga, možete [preuzeti aplikaciju dovršene kao u .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip) ili Kloniraj dovršene aplikacije:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet.git```

## <a name="register-an-app"></a>Registracija aplikacije
Stvaranje nove aplikacije na [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)ili slijedite ove [detaljne upute](active-directory-v2-app-registration.md).  Obavezno:

- Kopiraj dolje **Id aplikacije** dodijeljena aplikacije, morat ćete ga uskoro.
- Stvaranje **Aplikacije tajna** vrste **lozinku** i kopirajte dolje njegovom vrijednošću za kasnije
- Dodajte **Web** platformu za aplikacije.
- Unesite točne **Preusmjeravanje URI**. Preusmjeravanje uri naznačuje da Azure AD gdje treba usmjeriti odgovore provjere autentičnosti – zadano za ovog praktičnog vodiča `https://localhost:44326/`.


## <a name="install-owin"></a>Instalacija OWIN
Dodavanje OWIN proizvod NuGet paketa u `TodoList-WebApp` projekta korištenju konzole za Upravitelj paketa.  Proizvod OWIN će se koristiti za izdavanje zahtjeva za prijavu i sign-out, upravljanje sesija i informacije o korisniku, između ostalog.

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect -ProjectName TodoList-WebApp
PM> Install-Package Microsoft.Owin.Security.Cookies -ProjectName TodoList-WebApp
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoList-WebApp
```

## <a name="sign-the-user-in"></a>Prijava korisnika
Sada možete konfigurirati proizvod OWIN za korištenje [OpenID povezivanje protokol provjere autentičnosti](active-directory-v2-protocols.md#openid-connect-sign-in-flow).  

-   Otvaranje u `web.config` datoteke u korijenskoj mapi na `TodoList-WebApp` project i unesite vrijednosti za konfiguraciju vaše aplikacije u na `<appSettings>` sekcije.
    -   U `ida:ClientId` je **Id aplikacije** dodijeljene aplikacije na portalu za registraciju.
    - U `ida:ClientSecret` je **Aplikaciji tajna** koji ste stvorili na portalu za registraciju.
    -   U `ida:RedirectUri` je **Preusmjeravanje Uri** koju ste unijeli na portalu.
- Otvaranje u `web.config` datoteke u korijenskom direktoriju u `TodoList-Service` projekta i zamijeni u `ida:Audience` isti **Id aplikacije** kao prethodna.


- Otvorite datoteku `App_Start\Startup.Auth.cs` i dodavanje `using` za proizvode iz serije biblioteke odozgo.
- U istoj datoteci implementirati na `ConfigureAuth(...)` način.  Parametri unesete u `OpenIDConnectAuthenticationOptions` će vam poslužiti kao koordinate za aplikaciju možete komunicirati s Azure AD.

```C#
public void ConfigureAuth(IAppBuilder app)
{
    app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

    app.UseCookieAuthentication(new CookieAuthenticationOptions());

    app.UseOpenIdConnectAuthentication(
        new OpenIdConnectAuthenticationOptions
        {

                    // The `Authority` represents the v2.0 endpoint - https://login.microsoftonline.com/common/v2.0 
                    // The `Scope` describes the permissions that your app will need.  See https://azure.microsoft.com/documentation/articles/active-directory-v2-scopes/
                    // In a real application you could use issuer validation for additional checks, like making sure the user's organization has signed up for your app, for instance.

                    ClientId = clientId,
                    Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, "common", "/v2.0 "),
                    Scope = "openid email profile offline_access",
                    RedirectUri = redirectUri,
                    PostLogoutRedirectUri = redirectUri,
                    TokenValidationParameters = new TokenValidationParameters
                    {
                        ValidateIssuer = false,
                    },

                    // The `AuthorizationCodeReceived` notification is used to capture and redeem the authorization_code that the v2.0 endpoint returns to your app.

                    Notifications = new OpenIdConnectAuthenticationNotifications
                    {
                        AuthenticationFailed = OnAuthenticationFailed,
                        AuthorizationCodeReceived = OnAuthorizationCodeReceived,
                    }

        });
}
// ...
```

## <a name="use-msal-to-get-access-tokens"></a>Pomoću MSAL možete dobiti pristup tokena
U na `AuthorizationCodeReceived` obavijesti, ne možemo želite koristiti [2.0 OAuth zajedno s OpenID povezivanje](active-directory-v2-protocols.md#openid-connect-with-oauth-code-flow) da biste iskoristili authorization_code za token za pristup servisu popis zadataka.  MSAL možete olakšavaju postupak umjesto vas:

- Prvo instalirajte verziji pretpregleda MSAL:

```PM> Install-Package Microsoft.Identity.Client -ProjectName TodoList-WebApp -IncludePrerelease```
- I dodali drugo `using` naredbu na `App_Start\Startup.Auth.cs` datoteke za MSAL.
- Sada dodajte nov način u `OnAuthorizationCodeReceived` rukovatelja događajima.  Ovaj rukovatelj će koristiti MSAL dobiti pristupni token za API popis obaveza i će sadržavati token u predmemoriji za tokena MSAL je za kasnije:

```C#
private async Task OnAuthorizationCodeReceived(AuthorizationCodeReceivedNotification notification)
{
        string userObjectId = notification.AuthenticationTicket.Identity.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
        string tenantID = notification.AuthenticationTicket.Identity.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;
        string authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenantID, string.Empty);
        ClientCredential cred = new ClientCredential(clientId, clientSecret);

        // Here you ask for a token using the web app's clientId as the scope, since the web app and service share the same clientId.
        app = new ConfidentialClientApplication(Startup.clientId, redirectUri, cred, new NaiveSessionCache(userObjectId, notification.OwinContext.Environment["System.Web.HttpContextBase"] as HttpContextBase)) {};
        var authResult = await app.AcquireTokenByAuthorizationCodeAsync(new string[] { clientId }, notification.Code);
}
// ...
```

- U web-aplikacijama MSAL ima extensible tokena predmemorije koje je moguće koristiti za pohranu tokena.  Ovaj primjer implementira u `NaiveSessionCache` koji koristi spremište sesiju http tokeni predmemorije.

<!-- TODO: Token Cache article -->


## <a name="call-the-web-api"></a>Poziv u API na webu
Sada je vrijeme da biste korišteni access_token koju ste nabavili u 3.  Otvorite web-aplikaciji `Controllers\TodoListController.cs` datoteku koja daje sve zahtjeve za CRUD API popis zadataka.

- Možete koristiti MSAL ponovno ovdje radi dohvaćanja access_tokens iz predmemorije MSAL.  Najprije dodajte na `using` izjava za MSAL u tu datoteku.

    `using Microsoft.Identity.Client;`

- U na `Index` Akcije, korištenje MSAL `AcquireTokenSilentAsync` način da biste dobili programa access_token koje je moguće koristiti za čitanje podataka iz servisa za popis zadataka:

```C#
// ...
string userObjectID = ClaimsPrincipal.Current.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
string tenantID = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;
string authority = String.Format(CultureInfo.InvariantCulture, Startup.aadInstance, tenantID, string.Empty);
ClientCredential credential = new ClientCredential(Startup.clientId, Startup.clientSecret);

// Here you ask for a token using the web app's clientId as the scope, since the web app and service share the same clientId.
app = new ConfidentialClientApplication(Startup.clientId, redirectUri, credential, new NaiveSessionCache(userObjectID, this.HttpContext)){};
result = await app.AcquireTokenSilentAsync(new string[] { Startup.clientId });
// ...
```

- Uzorak zatim dodaje dobivene token zahtjev HTTP GET kao u `Authorization` zaglavlje, servis za popis zadataka koji se koristi za provjeru autentičnosti zahtjev.
- Vraća popis obaveza usluge na `401 Unauthorized` odgovor, access_tokens u MSAL prekorače koji nisu valjani zbog nekog razloga.  U ovom slučaju, trebali biste ispustite sve access_tokens iz predmemorije MSAL i prikaže poruku u kojoj se možda morati prijaviti ponovno, koji će se pokrenuti tijek tokena acquisition korisniku.

```C#
// ...
// If the call failed with access denied, then drop the current access token from the cache,
// and show the user an error indicating they might need to sign-in again.
if (response.StatusCode == System.Net.HttpStatusCode.Unauthorized)
{
        app.AppTokenCache.Clear(Startup.clientId);

        return new RedirectResult("/Error?message=Error: " + response.ReasonPhrase + " You might need to sign in again.");
}
// ...
```

- Isto tako, ako MSAL ne može da biste se vratili na access_token iz bilo kojeg razloga, trebali biste uputite korisnika da biste ponovno se prijavite.  Ovo je lako i svodi toga bilo `MSALException`:

```C#
// ...
catch (MsalException ee)
{
        // If MSAL could not get a token silently, show the user an error indicating they might need to sign in again.
        return new RedirectResult("/Error?message=An Error Occurred Reading To Do List: " + ee.Message + " You might need to log out and log back in.");
}
// ...
```

- U isti točno `AcquireTokenSilentAsync` poziv je implementd u na `Create` i `Delete` akcije.  U web-aplikacijama, možete koristiti ovu metodu MSAL da biste dobili access_tokens koje po potrebi ih u svojoj aplikaciji.  MSAL će voditi brigu o pri dohvaćanju predmemoriranje i osvježavanje tokeni umjesto vas.

Na kraju, stvaranje i pokretanje aplikacije!  Prijavite se pomoću Microsoftova Account ili račun za Azure AD i obratite pozornost na to kako identitet korisnika prikazuje se na gornjoj navigacijskoj traci.  Dodavanje i brisanje neke stavke s popisa obaveza korisnika da biste vidjeli OAuth 2.0 osigurani API poziva na djelu.  Sada imate web-aplikacije i web API oba osigurani pomoću standardnih industrijskih protokola, koji možete provjeru autentičnosti korisnika s njihovim osobne i poslovne/školske računi.

Za referencu, dovršena Ogledna (bez konfiguracijskih vrijednosti) na [navedeni su u nastavku](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip).  

## <a name="next-steps"></a>Daljnji koraci

Za dodatne resurse, pogledajte:
- [Vodič za razvojne inženjere 2.0 >>](active-directory-appmodel-v2-overview.md)
- [Oznaka StackOverflow "azure-aktivno-directory" >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Sigurnosna ažuriranja za naši proizvodi

Pozivamo vas da biste dobili obavijesti kada doći do sigurnošću potražite na [toj stranici](https://technet.microsoft.com/security/dd252948) i pretplata na upozorenja savjeta za sigurnost.
