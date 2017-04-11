<properties
    pageTitle=".NET Azure AD Uvod | Microsoft Azure"
    description="Upute za stvaranje web-aplikacije MVC .NET koje integrira Azure AD za prijavu."
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

# <a name="aspnet-web-app-sign-in--sign-out-with-azure-ad"></a>ASP.NET Web App za prijavu i odjavu s Azure AD

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Azure AD olakšava jednostavne i jasan spoljni upravljanje identitetom web app, koja omogućuje jedan prijave i sign-out sa samo nekoliko redaka koda.  U Asp.NET web-aplikacijama koje možete izvršiti sljedeće pomoću implementacija Microsoftov proizvod OWIN utemeljenih na zajednice korisnika koji se uključiti u .NET Framework 4,5.  U nastavku ćemo pomoću OWIN da biste:
-   Korisnik se prijaviti u aplikaciju pomoću Azure AD kao davatelja identiteta.
-   Prikaz neke informacije o korisniku.
-   Prijava korisnika iz aplikacije.

Da biste to učinili, morat ćete:

1. Registracija aplikacije s Azure AD
2. Postavljanje aplikacije da biste koristili kanal OWIN provjeru autentičnosti.
3. Koristi OWIN za izdavanje prijave i sign-out zahtjeve za Azure AD.
4. Ispis podataka o korisniku.

Da biste počeli, [Preuzimanje skeleton aplikacije](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) ili [Preuzimanje dovršene uzorka](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip).  Trebat ćete klijent za Azure AD u kojoj se registrirati aplikacije.  Ako ga već imam [upute da biste ga nabavili](active-directory-howto-tenant.md).

## <a name="1--register-an-application-with-azure-ad"></a>*1. Registracija aplikacije s Azure AD*
Da biste omogućili aplikacije za provjeru autentičnosti korisnika, najprije morate registrirati novu aplikaciju u klijentu.

- Prijavite se na Portal za Azure upravljanje.
- U navigacijskom oknu na lijevoj strani kliknite **Servisa Active Directory**.
- Odaberite klijenta na kojem želite da biste registrirali aplikaciju.
- Kliknite karticu **aplikacije** pa kliknite Dodaj u donjem ladica.
- Slijedite upite i stvaranje je nove **web-aplikacije i/ili WebAPI**.
    - **Naziv** aplikacije opisane su aplikacije krajnjim korisnicima
    -   **URL za prijavu** nije osnovni URL aplikacije.  Zadana vrijednost u skeleton je `https://localhost:44320/`.
    - **Aplikacija ID URI** je jedinstveni identifikator za svoju aplikaciju.  Konvencije jest korištenje `https://<tenant-domain>/<app-name>`, npr.`https://contoso.onmicrosoft.com/my-first-aad-app`
- Nakon što ste dovršili registraciju, AAD će dodijeliti aplikacije klijentskog Jedinstveni identifikator.  Potreban vam je tu vrijednost u sljedećim odjeljcima tako da ga kopirali na kartici Konfiguriraj.

## <a name="2-set-up-your-app-to-use-the-owin-authentication-pipeline"></a>*2. postavljanje aplikacije da biste koristili kanal OWIN provjera autentičnosti*
Ovdje, ne možemo konfigurirati proizvod OWIN koristiti protokol za povezivanje OpenID provjere autentičnosti.  OWIN će se koristiti za izdavanje zahtjeva za prijavu i sign-out, upravljanje sesija i informacije o korisniku, između ostalog.

-   Da biste započeli, dodajte pakete NuGet OWIN proizvod projekt pomoću konzole za Upravitelj paketa.

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
PM> Install-Package Microsoft.Owin.Security.Cookies
PM> Install-Package Microsoft.Owin.Host.SystemWeb
```

-   Dodavanje programa OWIN početnu klasu projekt pod nazivom `Startup.cs` desnom klikom na projektu--> **Dodaj** --> traženje "OWIN"-->**Nova stavka** .  Proizvod OWIN će pozivanje na `Configuration(...)` način prilikom pokretanja aplikacije.
-   Promjena klase izvješća da biste `public partial class Startup` -već Implementirali smo dio klase umjesto vas u drugu datoteku.  U na `Configuration(...)` način, provjerite na poziv ConfgureAuth(...) da biste postavili provjere autentičnosti za web-aplikacije  

```C#
public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

-   Otvorite datoteku `App_Start\Startup.Auth.cs` i implementacija u `ConfigureAuth(...)` način.  Parametri unesete u `OpenIDConnectAuthenticationOptions` će vam poslužiti kao koordinate za aplikaciju možete komunicirati s Azure AD.  Također morat ćete postaviti provjera autentičnosti kolačića - proizvod OpenID povezivanje koristi kolačići ispod na naslovnice.

```C#
public void ConfigureAuth(IAppBuilder app)
{
    app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

    app.UseCookieAuthentication(new CookieAuthenticationOptions());

    app.UseOpenIdConnectAuthentication(
        new OpenIdConnectAuthenticationOptions
        {
            ClientId = clientId,
            Authority = authority,
            PostLogoutRedirectUri = postLogoutRedirectUri,
        });
}
```

-   Na kraju, otvorite na `web.config` datoteku u korijenskoj mapi projekta, a potom unesite konfiguracijskih vrijednosti u na `<appSettings>` sekciju.
    -   Pokrenite aplikaciju `ida:ClientId` je Guid koji ste kopirali na portalu Azure u koraku 1.
    -   U `ida:Tenant` je naziv Azure AD klijent, primjerice "contoso.onmicrosoft.com".
    -   Vaše `ida:PostLogoutRedirectUri` označava Azure AD gdje treba preusmjeravaju korisnik završetku zahtjeva za sign-out uspješno.

## <a name="3-use-owin-to-issue-sign-in-and-sign-out-requests-to-azure-ad"></a>*3. koristi OWIN za izdavanje prijave i sign-out zahtjeve za Azure AD*
Pokrenite aplikaciju sada pravilno konfigurirani možete komunicirati s Azure AD pomoću provjere autentičnosti protokolom OpenID povezivanje.  OWIN sadrži snimanja brigu o sve ugly pojedinosti obratite poruka provjere autentičnosti, provjera valjanosti tokena iz Azure AD i održavanje korisničke sesije.  Sve što je ostaje da bi se dobilo korisnicima omogućuje prijava i odjava.

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
public void SignOut()
{
    // Send an OpenID Connect sign-out request.
    HttpContext.GetOwinContext().Authentication.SignOut(
        OpenIdConnectAuthenticationDefaults.AuthenticationType, CookieAuthenticationDefaults.AuthenticationType);
}
```

-   Sada otvorite `Views\Shared\_LoginPartial.cshtml`.  To su gdje ćete prikaže korisniku prijave i sign-out veze pokrenite aplikaciju i ispisati korisničko ime u prikazu.

```HTML
@if (Request.IsAuthenticated)
{
    <text>
        <ul class="nav navbar-nav navbar-right">
            <li class="navbar-text">
                Hello, @User.Identity.Name!
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

## <a name="4--display-user-information"></a>*4. podaci o korisniku prikaz*
Prilikom provjere autentičnosti korisnika završavaju na OpenID povezivanje, Azure AD vraća programa id_token aplikaciji koja sadrži "zahtjevima" ili assertions o korisniku.  Možete koristiti te zahtjevima za prilagodbu aplikacije:

- Otvaranje u `Controllers\HomeController.cs` datoteku.  Možete pristupiti zahtjevima za korisnika u vašem kontrolera putem na `ClaimsPrincipal.Current` objektima upravitelja sigurnost.

```C#
public ActionResult About()
{
    ViewBag.Name = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Name).Value;
    ViewBag.ObjectId = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
    ViewBag.GivenName = ClaimsPrincipal.Current.FindFirst(ClaimTypes.GivenName).Value;
    ViewBag.Surname = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Surname).Value;
    ViewBag.UPN = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Upn).Value;

    return View();
}
```

Na kraju, stvaranje i pokretanje aplikacije!  Ako to već niste učinili, sada je vrijeme da biste stvorili novog korisnika na klijentu s na *. domenu onmicrosoft.com.  Prijavite se pomoću tog korisnika i obratite pozornost na to kako identitet korisnika prikazuje se na gornjoj navigacijskoj traci.  Odjava i ponovno prijavite kao neki drugi korisnik u klijentu.  Ako vam se osjećaj osobito ambitious, registrirati i pokrenuti drugu instancu programa aplikacije (s vlastitom clientId) i gledanje potražite u članku jedinstvene prijave u akciji.

Za referencu, dovršena Ogledna (bez konfiguracijskih vrijednosti) na [navedeni su u nastavku](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip).  

Sada se može pomaknuti na dodatne teme.  Preporučujemo vam da biste isprobali:

[Web-mjesto API-JA s Azure AD za sigurnu >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
