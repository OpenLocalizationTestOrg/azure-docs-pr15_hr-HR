<properties
    pageTitle="Azure AD B2C | Microsoft Azure"
    description="Upute za sastavljanje .NET Web API-JA pomoću Azure Active Directory B2C, osigurani pomoću OAuth 2.0 pristup tokeni za provjeru autentičnosti."
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
    ms.topic="hero-article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-active-directory-b2c-build-a-net-web-api"></a>Azure Active Directory B2C: Izrada .NET web API-JA

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

S B2C Azure Active Directory (Azure AD), možete zaštititi API web-mjesto pomoću OAuth 2.0 pristupna tokena. Tokena omogućuju klijent aplikacije koje koriste Azure AD B2C za provjeru autentičnosti u API-JA. U ovom se članku objašnjava da biste stvorili API "popis obaveza".NET modela, prikaz i kontroleru (MVC) koje korisnicima omogućuje CRUD zadataka. Na webu API zaštićen je Azure AD B2C i samo omogućuje autentičnost korisnicima omogućuje upravljanje svoj popis zadataka.

## <a name="create-an-azure-ad-b2c-directory"></a>Stvaranje u imeniku Azure AD B2C

Prije korištenja Azure AD B2C morate stvoriti direktorij ili smjernice. Direktorij je spremnik za sve korisnike, aplikacije, grupe i više. Ako ne postoji već, [stvorite direktorij B2C](active-directory-b2c-get-started.md) prije nego što nastavite u ovom vodiču.

## <a name="create-an-application"></a>Stvaranje aplikacije komponente

Ćete morati stvoriti aplikaciju u direktoriju B2C. Tako ćete dobiti Azure AD informacije potrebne za sigurno komunikaciju s aplikacijom. Da biste stvorili aplikaciju, slijedite [ove upute](active-directory-b2c-app-registration.md). Obavezno:

- U aplikaciji obuhvaćaju **web app** ili **API na webu** .
- Korištenje **preusmjeravanje standardizirani identifikator resursa** `https://localhost:44316/` za web-aplikacije. To je zadano mjesto web-aplikacije klijentu za ovaj uzorak koda.
- Kopirajte **ID aplikacije** koji vam je dodijeljen aplikacije. Morat ćete ga kasnije.

 [AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Stvaranje police

U Azure AD B2C, svaki korisnički doživljaj definira [pravila](active-directory-b2c-reference-policies.md). Klijent u ovom primjeru kod sadrži tri identiteta sučelja: registracija za Office, prijavite se i uređivanje profila. Morat ćete stvoriti pravila za svaku vrstu, kao što je opisano u [članku referenca pravila](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy). Kada stvorite tri police, ne zaboravite:

- Odaberite **Korisnički ID za prijavu** ili **e-pošte registracije** plohu davatelji identiteta.
- Odaberite **zaslonsko ime** i ostale registracije atribute Pravilnik za prijavu.
- Odaberite **zaslonsko ime** i **ID objekta** zahtjevima kao zahtjevima za aplikacije za svaku pravila. Možete odabrati druge zahtjevima.
- Nakon stvaranja, kopirajte **naziv** svakog pravila. Morat ćete te nazive pravila kasnije.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Kada uspješno stvorite tri pravila, spremni ste za stvaranje aplikacije.

## <a name="download-the-code"></a>Preuzimanje kod

Kod za ovaj Praktični vodič [je spremljen na GitHub](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet). Da biste sastavili uzorka tijekom rada, možete [preuzeti skeleton projekt kao .zip datoteku](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet/archive/skeleton.zip). Mogu Kloniraj i na skeleton:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet.git
```

Aplikaciju dovršene nije [dostupan kao .zip datoteku](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet/archive/complete.zip) ili u `complete` podružnice u istom spremištu.

Kada preuzmete uzorak koda, otvorite datoteku .sln Visual Studio za početak rada. Datoteka rješenja sadrži dvije projekata: `TaskWebApp` i `TaskService`. `TaskWebApp`je MVC web-aplikacije koje korisnik stupi u interakciju s. `TaskService`je aplikacije pozadinskih API na Webu koji pohranjuje popis zadataka za svakog korisnika.

## <a name="configure-the-task-web-app"></a>Konfiguriranje web-aplikaciji zadatka

Kada korisnik stupi u interakciju s `TaskWebApp`, klijent šalje zahtjeva za Azure AD i dobiti natrag tokeni koje je moguće koristiti da biste uputili poziv na `TaskService` web-API-JA. Prijava korisnika i dobiti tokena, morate navesti `TaskWebApp` neke informacije o aplikacije. U na `TaskWebApp` otvaranje projekta u `web.config` datoteka u korijenskom direktoriju projekta i zamijeni vrijednosti u na `<appSettings>` sekcije.  Možete ostaviti na `AadInstance`, `RedirectUri`, a `TaskServiceUrl` vrijednosti kao-je.

```
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
    <add key="api:TaskServiceUrl" value="https://localhost:44332/" />
  </appSettings>
```

Ovaj članak obuhvaća sastavnih na `TaskWebApp` klijenta.  Da biste saznali kako izraditi Azure AD B2C pomoću web-aplikacijama, potražite u članku [naš .NET web-aplikacije vodič](active-directory-b2c-devquickstarts-web-dotnet.md).

## <a name="secure-the-api"></a>Sigurne na API-JA

Kada imate klijenta koji se poziva na API ime korisnika, možete zaštititi `TaskService` pomoću OAuth 2.0 nošenja tokena. Vaš API-JA možete prihvatiti i provjeriti tokeni pomoću web-sučelja web-mjesta tvrtke Microsoft Open za biblioteku .NET (OWIN).

### <a name="install-owin"></a>Instalacija OWIN
Početak instalacijom kanal OWIN OAuth provjere autentičnosti:

```
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TaskService
```

### <a name="enter-your-b2c-details"></a>Unesite pojedinosti B2C
Otvaranje u `web.config` datoteke u korijenskoj mapi na `TaskService` project i zamjena vrijednosti u na `<appSettings>` sekcije. Ove vrijednosti koristit će se u biblioteku API i OWIN.  Možete ostaviti na `AadInstance` vrijednost ne mijenja.

```
  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
  </appSettings>
```

### <a name="add-an-owin-startup-class"></a>Dodavanje klase za pokretanje programa OWIN
Dodavanje OWIN početnu klasu da biste na `TaskService` projekta pod nazivom `Startup.cs`.  Na projektu, desnom tipkom miša kliknite odaberite **Dodaj** "i" **Nova stavka**, a zatim potražite OWIN.


```C#
// Startup.cs

// Change the class declaration to "public partial class Startup" - we’ve already implemented part of this class for you in another file.
public partial class Startup
{
    // The OWIN middleware will invoke this method when the app starts
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

### <a name="configure-oauth-20-authentication"></a>Konfiguriranje provjere autentičnosti OAuth 2.0
Otvorite datoteku `App_Start\Startup.Auth.cs`, i implementacija u `ConfigureAuth(...)` metoda:

```C#
// App_Start\Startup.Auth.cs

public partial class Startup
{
    // These values are pulled from web.config
    public static string aadInstance = ConfigurationManager.AppSettings["ida:AadInstance"];
    public static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];
    public static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    public static string signUpPolicy = ConfigurationManager.AppSettings["ida:SignUpPolicyId"];
    public static string signInPolicy = ConfigurationManager.AppSettings["ida:SignInPolicyId"];
    public static string editProfilePolicy = ConfigurationManager.AppSettings["ida:UserProfilePolicyId"];

    public void ConfigureAuth(IAppBuilder app)
    {   
        app.UseOAuthBearerAuthentication(CreateBearerOptionsFromPolicy(signUpPolicy));
        app.UseOAuthBearerAuthentication(CreateBearerOptionsFromPolicy(signInPolicy));
        app.UseOAuthBearerAuthentication(CreateBearerOptionsFromPolicy(editProfilePolicy));
    }

    public OAuthBearerAuthenticationOptions CreateBearerOptionsFromPolicy(string policy)
    {
        TokenValidationParameters tvps = new TokenValidationParameters
        {
            // This is where you specify that your API only accepts tokens from its own clients
            ValidAudience = clientId,
            AuthenticationType = policy,
        };

        return new OAuthBearerAuthenticationOptions
        {
            // This SecurityTokenProvider fetches the Azure AD B2C metadata & signing keys from the OpenIDConnect metadata endpoint
            AccessTokenFormat = new JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider(String.Format(aadInstance, tenant, policy))),
        };
    }
}
```

### <a name="secure-the-task-controller"></a>Sigurne kontrolerom zadatka
Nakon aplikaciju konfiguriran za korištenje provjere autentičnosti OAuth 2.0, možete zaštititi svoje API na webu dodavanjem programa `[Authorize]` oznaka s kontrolerom zadatka. Ovo je kontrolerom gdje svi rukovanje popis zadataka odvija, tako da se treba zaštititi kontrolerom cijelu na razini predmete. Možete dodati u `[Authorize]` oznake za pojedinačne akcije veću kontrolu preciznije.

```C#
// Controllers\TasksController.cs

[Authorize]
public class TasksController : ApiController
{
    ...
}
```

### <a name="get-user-information-from-the-token"></a>Korisničke informacije zatražite od tokena
`TasksController`Zadaci pohranjuju u bazi podataka gdje svaki zadatak je korisnik sustava povezane vlasniku "" zadatak. Vlasnik određuje korisnika **ID objekta**. (To je zašto je potrebno da biste dodali ID objekta kao aplikaciju zahtjeva u sve police.)

```C#
// Controllers\TasksController.cs

public IEnumerable<Models.Task> Get()
{
    string owner = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
    IEnumerable<Models.Task> userTasks = db.Tasks.Where(t => t.owner == owner);
    return userTasks;
}
```

## <a name="run-the-sample-app"></a>Pokrenite aplikaciju uzorka

Na kraju, stvaranje i pokretanje i `TaskWebApp` i `TaskService`. Prijavite se za aplikaciju pomoću adrese e-pošte ili korisničko ime. Stvaranje neke zadatke na korisnikovu popisu zadataka i obratite pozornost na to kako su se ista i u na API čak i kada zaustavite i ponovno pokrenite klijent.

## <a name="edit-your-policies"></a>Uređivanje police

Nakon što ste osigurani API pomoću Azure AD B2C, možete Eksperimentirajte s pravilima pokrenite aplikaciju i prikaz efekata (ili nemaju sadržaja) na na API. Možete rukovati zahtjevima za aplikacije u pravilnike i promjenu korisničkih podataka koji je dostupan na web-mjestu API-JA. Bilo koji zahtjevima koje dodate bit će dostupni .NET MVC web API-JA u na `ClaimsPrincipal` objekta, kao što je opisano u ovom članku.

<!--

## Next steps

You can now move onto more advanced B2C topics. You may try:

[Call a web API from a web app]()

[Customize the UX of your B2C app]()

-->
