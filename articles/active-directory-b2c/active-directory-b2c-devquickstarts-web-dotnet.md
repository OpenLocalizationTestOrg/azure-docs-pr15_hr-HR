<properties
    pageTitle="Azure Active Directory B2C | Microsoft Azure"
    description="Upute za stvaranje web-aplikacije koja sadrži prijavu, prijave, i upravljanje profila pomoću Azure Active Directory B2C."
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

# <a name="azure-ad-b2c-build-a-net-web-app"></a>Azure AD B2C: Stvaranje web-aplikacijama .NET

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Pomoću B2C Azure Active Directory (Azure AD), možete dodati samostalno identiteta za napredne značajke upravljanja na web-aplikaciju u nekoliko koraka kratki. U ovom se članku navode će kako stvoriti web-aplikacijama .NET Model, prikaz i kontroleru (MVC) koja sadrži korisnički prijave, prijavu i upravljanje profila. Aplikaciju će obuhvaćaju podršku za prijavu i prijaviti pomoću korisničkog imena ili e-pošte, a zatim pomoću računi društvenih kao što su Facebook i Google.

## <a name="get-an-azure-ad-b2c-directory"></a>Početak u imeniku Azure AD B2C

Prije korištenja Azure AD B2C morate stvoriti direktorij ili smjernice. Direktorij je spremnik za sve korisnike, aplikacije, grupe i više.  Ako ne postoji već, [stvorite direktorij B2C](active-directory-b2c-get-started.md) prije nego što nastavite u ovom vodiču.

## <a name="create-an-application"></a>Stvaranje aplikacije komponente

Ćete morati stvoriti aplikaciju u direktoriju B2C. Tako ćete dobiti Azure AD informacije potrebne za sigurno komunikaciju s aplikacijom. Da biste stvorili aplikaciju, slijedite [ove upute](active-directory-b2c-app-registration.md).  Obavezno:

- U aplikaciji obuhvaćaju **web app/API na webu** .
- Unesite `https://localhost:44316/` kao na **preusmjeravanje URI**. Je zadani URL za ovaj uzorak koda.
- Kopirajte dolje **ID aplikacije** koji vam je dodijeljen aplikacije.  Jer će vam kasnije.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Stvaranje police

U Azure AD B2C, svaki korisnički doživljaj definira [pravila](active-directory-b2c-reference-policies.md). Ovaj uzorak koda sadrži tri identiteta sučelja: registracija za Office, prijavite se i uređivanje profila. Morate stvoriti jednu pravila svake vrste kao što je opisano u [članku referenca pravila](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy).

>[AZURE.NOTE] Azure AD B2C podržava kombinirane registracije ili prijave u pravilnik koji ne istaknute ovog praktičnog vodiča.  Registracije ili pravila za prijavu prikazuju se u [ovom ćete praktičnom vodiču ekvivalentne](active-directory-b2c-devquickstarts-web-dotnet-susi.md).

Kada stvorite tri pravila, ne zaboravite:

- Odaberite **Korisnički ID za prijavu** ili **e-pošte registracije** plohu davatelji identiteta.
- Odaberite **zaslonsko ime** i ostale registracije atribute Pravilnik za prijavu.
- Odaberite Preuzmi **zaslonsko ime** kao aplikaciju zahtjeva u svakoj pravila. Možete odabrati druge zahtjevima.
- Nakon stvaranja, kopirajte **naziv** svakog pravila. Morat ćete nazive pravila kasnije.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Kada stvorite tri police, spremni ste za stvaranje aplikacije.  

## <a name="download-the-code-and-configure-authentication"></a>Preuzmite kod i konfiguriranje provjere autentičnosti

Kod za ovaj primjer [je spremljen na GitHub](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet). Da biste sastavili uzorka tijekom rada, možete [preuzeti skeleton projekt kao .zip datoteku](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet/archive/skeleton.zip). Mogu Kloniraj i na skeleton:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet.git
```

Dovršena Ogledna nije [dostupan kao .zip datoteku](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet/archive/complete.zip) ili u `complete` podružnice u istom spremištu.

Kada preuzmete uzorak koda, otvorite datoteku .sln Visual Studio za početak rada.

Pokrenite aplikaciju komunicira s Azure AD B2C slanjem poruka provjere autentičnosti koje navode pravilnik žele izvršiti kao dio HTTP zahtjev. Za .NET web-aplikacije možete koristiti proizvod tvrtke Microsoft OWIN Pošalji zahtjeve za povezivanje OpenID provjeru autentičnosti, pravila za izvršavanje, upravljati korisničke sesije i drugo.

Da biste započeli, dodajte pakete NuGet OWIN proizvod u projekt pomoću konzole za Visual Studio paketa Manager.

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
PM> Install-Package Microsoft.Owin.Security.Cookies
PM> Install-Package Microsoft.Owin.Host.SystemWeb
```

Zatim otvorite na `web.config` datoteka u korijenskom direktoriju projekta, a zatim unesite vrijednosti za konfiguraciju vaše aplikacije u na `<appSettings>` u odjeljku zamjene postojećih vrijednosti.

```
<configuration>
  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
  </appSettings>
...
```

[AZURE.INCLUDE [active-directory-b2c-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

Nakon toga dodati klase za pokretanje programa OWIN projekt pod nazivom `Startup.cs`. Desnom tipkom miša kliknite na projektu odaberite **Dodaj** "i" **Nova stavka**, a zatim potražite "OWIN." **Pripazite da biste promijenili klasu izvješća da biste `public partial class Startup` **. Dio klase umjesto vas ne možemo implementirana u neku drugu datoteku. Proizvod OWIN će pozivanje na `Configuration(...)` način prilikom pokretanja aplikacije. U ovu metodu upućivanje poziva na `ConfigureAuth(...)`koju postavljate provjere autentičnosti za aplikaciju.

```C#
// Startup.cs

public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

Otvorite datoteku `App_Start\Startup.Auth.cs` i implementacija u `ConfigureAuth(...)` način.  Parametri unesete u `OpenIdConnectAuthenticationOptions` služi kao koordinate za aplikaciju možete komunicirati s Azure AD. Morate postaviti provjera autentičnosti kolačića. Proizvod OpenID povezivanje koristi kolačiće da bi se zadržao korisničke sesije, između ostalog.

```C#
// App_Start\Startup.Auth.cs

public partial class Startup
{
    // App config settings
    private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    private static string aadInstance = ConfigurationManager.AppSettings["ida:AadInstance"];
    private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];
    private static string redirectUri = ConfigurationManager.AppSettings["ida:RedirectUri"];

    // B2C policy identifiers
    public static string SignUpPolicyId = ConfigurationManager.AppSettings["ida:SignUpPolicyId"];
    public static string SignInPolicyId = ConfigurationManager.AppSettings["ida:SignInPolicyId"];
    public static string ProfilePolicyId = ConfigurationManager.AppSettings["ida:UserProfilePolicyId"];

    public void ConfigureAuth(IAppBuilder app)
    {
        app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

        app.UseCookieAuthentication(new CookieAuthenticationOptions());

        // Configure OpenID Connect middleware for each policy
        app.UseOpenIdConnectAuthentication(CreateOptionsFromPolicy(SignUpPolicyId));
        app.UseOpenIdConnectAuthentication(CreateOptionsFromPolicy(ProfilePolicyId));
        app.UseOpenIdConnectAuthentication(CreateOptionsFromPolicy(SignInPolicyId));
    }

    // Used for avoiding yellow-screen-of-death
    private Task AuthenticationFailed(AuthenticationFailedNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> notification)
    {
        notification.HandleResponse();
        if (notification.Exception.Message == "access_denied")
        {
            notification.Response.Redirect("/");
        }
        else
        {
            notification.Response.Redirect("/Home/Error?message=" + notification.Exception.Message);
        }

        return Task.FromResult(0);
    }

    private OpenIdConnectAuthenticationOptions CreateOptionsFromPolicy(string policy)
    {
        return new OpenIdConnectAuthenticationOptions
        {
            // For each policy, give OWIN the policy-specific metadata address, and
            // set the authentication type to the id of the policy
            MetadataAddress = String.Format(aadInstance, tenant, policy),
            AuthenticationType = policy,

            // These are standard OpenID Connect parameters, with values pulled from web.config
            ClientId = clientId,
            RedirectUri = redirectUri,
            PostLogoutRedirectUri = redirectUri,
            Notifications = new OpenIdConnectAuthenticationNotifications
            {
                AuthenticationFailed = AuthenticationFailed,
            },
            Scope = "openid",
            ResponseType = "id_token",

            // This piece is optional - it is used for displaying the user's name in the navigation bar.
            TokenValidationParameters = new TokenValidationParameters
            {
                NameClaimType = "name",
            },
        };
    }
}
```

## <a name="send-authentication-requests-to-azure-ad"></a>Slanje zahtjeva za provjeru autentičnosti Azure AD
Pokrenite aplikaciju sada pravilno konfigurirani možete komunicirati s Azure AD B2C pomoću protokol za povezivanje OpenID provjere autentičnosti.  OWIN sadrži snimanja brigu o sve pojedinosti obratite poruka provjere autentičnosti, provjera valjanosti tokena iz Azure AD i održavanje korisničke sesije.  Sve što je ostaje da biste započeli tijek za svakog korisnika.

Kada korisnik odabere **Registracija za Office**, **prijavite se u**ili **Uređivanje profila** u web-aplikaciji, pridruženi akciju se poziva u `Controllers\AccountController.cs`. U svakom slučaju, možete koristiti ugrađenu metode OWIN za pokretanje desnom pravila:

```C#
// Controllers\AccountController.cs

public void SignIn()
{
    if (!Request.IsAuthenticated)
    {
        // To execute a policy, you simply need to trigger an OWIN challenge.
        // You can indicate which policy to use by specifying the policy id as the AuthenticationType
        HttpContext.GetOwinContext().Authentication.Challenge(
            new AuthenticationProperties () { RedirectUri = "/" }, Startup.SignInPolicyId);
    }
}

public void SignUp()
{
    if (!Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge(
            new AuthenticationProperties() { RedirectUri = "/" }, Startup.SignUpPolicyId);
    }
}


public void Profile()
{
    if (Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge(
            new AuthenticationProperties() { RedirectUri = "/" }, Startup.ProfilePolicyId);
    }
}
```

Možete koristiti za `[Authorize]` oznaku u vašem kontrolera koji zahtijeva izvršavanja neke pravila ako niste prijavljeni korisnik. Otvaranje `Controllers\HomeController.cs` i dodajte u `[Authorize]` oznaka s kontrolerom zahtjevima.  OWIN ćete odabrati zadnju pravila konfiguriran za izvršavanje kada se oznaka autorizacija poziva.

```C#
// Controllers\HomeController.cs

// You can use the Authorize decorator to execute a policy if the user is not already signed in the app.
[Authorize]
public ActionResult Claims()
{
  ...
```

OWIN možete koristiti i za odjavu korisnika iz aplikacije. In `Controllers\AccountController.cs`:  

```C#
// Controllers\AccountController.cs

public void SignOut()
{
    // To sign out the user, you should issue an OpenIDConnect sign out request
    if (Request.IsAuthenticated)
    {
        IEnumerable<AuthenticationDescription> authTypes = HttpContext.GetOwinContext().Authentication.GetAuthenticationTypes();
        HttpContext.GetOwinContext().Authentication.SignOut(authTypes.Select(t => t.AuthenticationType).ToArray());
        Request.GetOwinContext().Authentication.GetAuthenticationTypes();
    }
}
```

## <a name="display-user-information"></a>Prikaz korisničkih podataka
Kada provjeru autentičnosti korisnika pomoću OpenID povezivanje Azure AD vraća token za ID-a za aplikaciju koja sadrži **zahtjevima**. To su assertions o korisniku. Možete koristiti zahtjevima za prilagodbu aplikacije.  

Otvaranje u `Controllers\HomeController.cs` datoteku. Možete pristupiti zahtjevima korisnički u vašem kontrolera putem na `ClaimsPrincipal.Current` objektima upravitelja sigurnost.

```C#
// Controllers\HomeController.cs

[Authorize]
public ActionResult Claims()
{
    Claim displayName = ClaimsPrincipal.Current.FindFirst(ClaimsPrincipal.Current.Identities.First().NameClaimType);
    ViewBag.DisplayName = displayName != null ? displayName.Value : string.Empty;
    return View();
}
```

Bilo koji zahtjeva koja prima aplikacija na isti način možete pristupiti.  Popis svih zahtjevima aplikaciju prima dostupna vam na stranici **zahtjevima** .

## <a name="run-the-sample-app"></a>Pokrenite aplikaciju uzorka

Na kraju, stvaranje i pokretanje aplikacije. Prijavite se za aplikaciju pomoću programa e-pošte adresa ili korisničko ime. Odjava i ponovno prijavite istog korisnika. Uređivanje profila za tog korisnika. Odjavljivanje i prijavite se kao drugi korisnik. Imajte na umu da se podaci prikazuju na kartici **zahtjevima** odgovara informacija koje ste konfigurirali police.

## <a name="add-social-idps"></a>Dodavanje društvenih IDPs

Trenutno aplikacija podržava samo korisnik prijave i prijavu pomoću **Lokalni računi**. To su računi pohranjena u direktoriju B2C koje koriste korisničko ime i lozinku. Pomoću Azure AD B2C možete dodati podrška za drugi **davatelji identiteta** (IDPs) bez promjena bilo kod.

Da biste dodali društvene IDPs aplikacije, započnite slijedeći detaljne upute u sljedećim člancima. Za svaku IDP želite podržava, morate registrirati aplikacije u tom sustavu i dobiti ID-a za klijenta

- [Postavljanje servisa Facebook kao programa IDP](active-directory-b2c-setup-fb-app.md)
- [Postavljanje Google kao programa IDP](active-directory-b2c-setup-goog-app.md)
- [Postavljanje Amazon kao programa IDP](active-directory-b2c-setup-amzn-app.md)
- [Postavljanje servisa LinkedIn kao programa IDP](active-directory-b2c-setup-li-app.md)

Nakon što dodate davatelji identiteta B2C direktorija, morate svaki od tri police da biste dodali novi IDPs uređivanje kao što je opisano u [članku referenca pravila](active-directory-b2c-reference-policies.md). Nakon što spremite police, ponovno pokrenite aplikaciju.  Trebali biste vidjeti novi IDPs dodani kao prijave i registracije mogućnosti u svakoj od vašeg identiteta iskustvo.

Možete Eksperimentirajte s pravilima i pridržavajte se efekt na aplikaciju uzorka. Dodavanje ili uklanjanje IDPs, rukovati zahtjevima aplikacije ili promijenite atribute za prijavu. Eksperimentirajte dok se ne može vidjeti kako pravilnika, zahtjevi za provjeru autentičnosti i OWIN sve zajedno.

Za referencu, dovršeni uzorka (bez konfiguracijskih vrijednosti) [prikazuje se kao .zip datoteku](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet/archive/complete.zip). Mogu Kloniraj i to iz GitHub:

```
git clone --branch complete https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet.git
```

<!--

## Next steps

You can now move on to more advanced B2C topics. You might try:

[Call a web API from a web app]()

[Customize the UX for a B2C app]()

-->
