<properties
    pageTitle="Azure AD 2.0 .NET Web App | Microsoft Azure"
    description="Upute za stvaranje web-aplikacije MVC .NET koje se korisnici pomoću oba Microsoftova Account i računi tvrtke ili obrazovne ustanove."
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

# <a name="add-sign-in-to-an-net-mvc-web-app"></a>Dodavanje prijavu u web-aplikaciji programa .NET MVC

S krajnju točku 2.0 možete brzo dodati provjere autentičnosti na web-aplikacije s podrškom za oba Microsoftovi računi i računi tvrtke ili obrazovne ustanove.  U ASP.NET web-aplikacijama koje možete izvršiti to pomoću tvrtke Microsoft OWIN proizvod uključeni u .NET Framework 4,5.

> [AZURE.NOTE]
    Neke scenarije servisa Azure Active Directory i značajke podržava krajnju točku 2.0.  Da biste utvrdili koristite krajnju točku 2.0, Saznajte više o [2.0 ograničenja](active-directory-v2-limitations.md).

 Ovdje bismo stvoriti na web-aplikacije koje koristi OWIN Prijava korisnika, prikazuju neke informacije o korisniku i Prijava korisnika iz aplikacije.
 
 ## <a name="download"></a>Preuzimanje
Kod za ovog praktičnog vodiča je spremljen [na GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet).  Da biste pratili, možete [preuzeti aplikacije skeleton kao u .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) ili Kloniraj na skeleton:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet.git```

Aplikaciju dovršene navedeni su na kraju ovog vodiča.

## <a name="register-an-app"></a>Registracija aplikacije
Stvaranje nove aplikacije na [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)ili slijedite ove [detaljne upute](active-directory-v2-app-registration.md).  Obavezno:

- Kopiraj dolje **Id aplikacije** dodijeljene aplikacije, morat ćete ga uskoro.
- Dodajte **Web** platformu za aplikacije.
- Unesite točne **Preusmjeravanje URI**. Preusmjeravanje uri naznačuje da Azure AD gdje treba usmjeriti odgovore provjere autentičnosti – zadano za ovog praktičnog vodiča `https://localhost:44326/`.

## <a name="install--configure-owin-authentication"></a>Instaliranje i konfiguriranje provjere autentičnosti OWIN
Ovdje, ne možemo konfigurirati proizvod OWIN koristiti protokol za povezivanje OpenID provjere autentičnosti.  OWIN će se koristiti za izdavanje zahtjeva za prijavu i sign-out, upravljanje sesija i informacije o korisniku, između ostalog.

-   Da biste započeli, otvorite na `web.config` datoteku u korijenskoj mapi projekta, a potom unesite vrijednosti za konfiguraciju vaše aplikacije u na `<appSettings>` sekciju.
    -   U `ida:ClientId` je **Id aplikacije** dodijeljene aplikacije na portalu za registraciju.
    -   U `ida:RedirectUri` je **Preusmjeravanje Uri** koju ste unijeli na portalu.

-   Zatim dodajte pakete NuGet OWIN proizvod projekt pomoću konzole za Upravitelj paketa.

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
PM> Install-Package Microsoft.Owin.Security.Cookies
PM> Install-Package Microsoft.Owin.Host.SystemWeb
```

-   Dodavanje programa "OWIN pokretanje klasa" u projekt pod nazivom `Startup.cs` desnom klikom na projektu--> **Dodaj** --> traženje "OWIN"-->**Nova stavka** .  Proizvod OWIN će pozivanje na `Configuration(...)` način prilikom pokretanja aplikacije.
-   Promjena klase izvješća da biste `public partial class Startup` -već Implementirali smo dio klase umjesto vas u drugu datoteku.  U na `Configuration(...)` način, provjerite na poziv ConfigureAuth(...) da biste postavili provjere autentičnosti za web-aplikacije  

```C#
[assembly: OwinStartup(typeof(Startup))]

namespace TodoList_WebApp
{
    public partial class Startup
    {
        public void Configuration(IAppBuilder app)
        {
            ConfigureAuth(app);
        }
    }
}
```

-   Otvorite datoteku `App_Start\Startup.Auth.cs` i implementacija u `ConfigureAuth(...)` način.  Parametri unesete u `OpenIdConnectAuthenticationOptions` će vam poslužiti kao koordinate za aplikaciju možete komunicirati s Azure AD.  Također morat ćete postaviti provjera autentičnosti kolačića - proizvod OpenID povezivanje koristi kolačići ispod na naslovnice.

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
                                     RedirectUri = redirectUri,
                                     Scope = "openid email profile",
                                     ResponseType = "id_token",
                                     PostLogoutRedirectUri = redirectUri,
                                     TokenValidationParameters = new TokenValidationParameters
                                     {
                                             ValidateIssuer = false,
                                     },
                                     Notifications = new OpenIdConnectAuthenticationNotifications
                                     {
                                             AuthenticationFailed = OnAuthenticationFailed,
                                     }
                             });
             }
```

## <a name="send-authentication-requests"></a>Slanje zahtjeva za provjeru autentičnosti
Pokrenite aplikaciju sada pravilno konfigurirani možete komunicirati s krajnje 2.0 koristiti protokol za povezivanje OpenID provjeru autentičnosti.  OWIN sadrži snimanja brigu o sve ugly pojedinosti obratite poruka provjere autentičnosti, provjera valjanosti tokena iz Azure AD i održavanje korisničke sesije.  Sve što je ostaje da bi se dobilo korisnicima omogućuje prijava i odjava.

- Možete koristiti autorizirali oznake u vašem kontrolera obavezne korisniku se prijavi prije pristupanja određene stranice.  Otvaranje `Controllers\HomeController.cs`, i dodajte u `[Authorize]` oznaka s kontrolerom o.

```C#
[Authorize]
public ActionResult About()
{
  ...
```

-   Možete koristiti i OWIN izravno izdavanje zahtjeva provjeru autentičnosti iz unutar.  Otvaranje `Controllers\AccountController.cs`.  U SignIn() i SignOut() Akcije, odnosno izdavanje OpenID povezivanje test i sign-out zahtjeva.

```C#
public void SignIn()
{
    // Send an OpenID Connect sign-in request.
    if (!Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge(new AuthenticationProperties { RedirectUri = "/" }, OpenIdConnectAuthenticationDefaults.AuthenticationType);
    }
}

// BUGBUG: Ending a session with the v2.0 endpoint is not yet supported.  Here, we just end the session with the web app.  
public void SignOut()
{
    // Send an OpenID Connect sign-out request.
    HttpContext.GetOwinContext().Authentication.SignOut(CookieAuthenticationDefaults.AuthenticationType);
    Response.Redirect("/");
}
```

-   Sada otvorite `Views\Shared\_LoginPartial.cshtml`.  To su gdje ćete prikaže korisniku prijave i sign-out veze pokrenite aplikaciju i ispisati korisničko ime u prikazu.

```HTML
@if (Request.IsAuthenticated)
{
    <text>
        <ul class="nav navbar-nav navbar-right">
            <li class="navbar-text">

                @*The 'preferred_username' claim can be used for showing the user's primary way of identifying themselves.*@

                Hello, @(System.Security.Claims.ClaimsPrincipal.Current.FindFirst("preferred_username").Value)!
            </li>
            <li>
                @Html.ActionLink("Sign out", "SignOut", "Account")
            </li>
        </ul>
    </text>
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li>@Html.ActionLink("Sign in", "SignIn", "Account", routeValues: null, htmlAttributes: new { id = "loginLink" })</li>
    </ul>
}
```

## <a name="display-user-information"></a>Prikaz korisničkih podataka
Prilikom provjere autentičnosti korisnika završavaju na OpenID povezivanje, krajnju točku 2.0 vraća programa id_token aplikaciju koja sadrži [zahtjevima](active-directory-v2-tokens.md#id_tokens)ili assertions o korisniku.  Možete koristiti te zahtjevima za prilagodbu aplikacije:

- Otvaranje u `Controllers\HomeController.cs` datoteku.  Možete pristupiti zahtjevima za korisnika u vašem kontrolera putem na `ClaimsPrincipal.Current` objektima upravitelja sigurnost.

```C#
[Authorize]
public ActionResult About()
{
    ViewBag.Name = ClaimsPrincipal.Current.FindFirst("name").Value;

    // The object ID claim will only be emitted for work or school accounts at this time.
    Claim oid = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier");
    ViewBag.ObjectId = oid == null ? string.Empty : oid.Value;

    // The 'preferred_username' claim can be used for showing the user's primary way of identifying themselves
    ViewBag.Username = ClaimsPrincipal.Current.FindFirst("preferred_username").Value;

    // The subject or nameidentifier claim can be used to uniquely identify the user
    ViewBag.Subject = ClaimsPrincipal.Current.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;

    return View();
}
```

## <a name="run"></a>Pokreni

Na kraju, stvaranje i pokretanje aplikacije!   Prijavite se pomoću osobnog Microsoftova Account ili pak računa tvrtke ili obrazovne ustanove i obratite pozornost na to kako identitet korisnika prikazuje se na gornjoj navigacijskoj traci.  Sada imate web-aplikacijama osigurani pomoću standardnih industrijskih protokola koje možete provjeru autentičnosti korisnika s njihovim osobne i poslovne/školske računi.

Za referencu, dovršeni uzorka (bez konfiguracijskih vrijednosti) [prikazuje se kao .zip ovdje](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/complete.zip)ili pak mogu Kloniraj ga iz GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet.git```

## <a name="next-steps"></a>Daljnji koraci

Sada se može pomaknuti na dodatne teme.  Preporučujemo vam da biste isprobali:

[Sigurne API Web s na krajnjoj točki 2.0 >>](active-directory-devquickstarts-webapi-dotnet.md)

Za dodatne resurse, pogledajte:
- [Vodič za razvojne inženjere 2.0 >>](active-directory-appmodel-v2-overview.md)
- [Oznaka StackOverflow "azure-aktivno-directory" >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Sigurnosna ažuriranja za naši proizvodi

Pozivamo vas da biste dobili obavijesti kada doći do sigurnošću potražite na [toj stranici](https://technet.microsoft.com/security/dd252948) i pretplata na upozorenja savjeta za sigurnost.
