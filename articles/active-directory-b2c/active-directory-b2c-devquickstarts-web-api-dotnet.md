<properties
    pageTitle="Azure Active Directory B2C | Microsoft Azure"
    description="Upute za stvaranje web-aplikacije koja se poziva na API na webu pomoću Azure Active Directory B2C."
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

# <a name="azure-ad-b2c-call-a-web-api-from-a-net-web-app"></a>Azure AD B2C: Poziv API web-mjesto iz web-aplikacije za .NET

Pomoću B2C Azure Active Directory (Azure AD), možete dodati samostalno identiteta za napredne značajke upravljanja web-aplikacije i web-API-ji u nekoliko koraka kratki. U ovom se članku obrađuje kako stvoriti Model, prikaz i kontroler .NET (MVC) "popis obaveza" web-aplikacije koje se poziva na API na webu pomoću nošenja tokena

Ovaj članak obuhvaća upravljanje profila s Azure AD B2C i kako implementirati prijavu, prijavu. Ona se usredotočuje na webu pozivanja API-ji nakon već autentičnost korisnika. Ako to već niste učinili, bi trebao počinjati s [.NET web app uvodni vodič](active-directory-b2c-devquickstarts-web-dotnet.md) dodatne informacije o osnovama Azure AD B2C.

## <a name="get-an-azure-ad-b2c-directory"></a>Početak u imeniku Azure AD B2C

Prije korištenja Azure AD B2C morate stvoriti direktorij ili smjernice.  Direktorij je spremnik za sve korisnike, aplikacije, grupe i više.  Ako ne postoji već, [stvorite direktorij B2C](active-directory-b2c-get-started.md) prije nego što nastavite u ovom vodiču.

## <a name="create-an-application"></a>Stvaranje aplikacije komponente

Ćete morati stvoriti aplikaciju u direktoriju B2C. Tako ćete dobiti Azure AD informacije potrebne za sigurno komunikaciju s aplikacijom. U ovom slučaju web app i web API će predstavljati jedan **ID aplikacije**jer oni čine jedan logičke aplikacije. Da biste stvorili aplikaciju, slijedite [ove upute](active-directory-b2c-app-registration.md). Obavezno:

- U aplikaciji obuhvaćaju **web app/API na webu** .
- Unesite `https://localhost:44316/` kao **URL odgovori**. Je zadani URL za ovaj uzorak koda.
- Kopirajte **ID aplikacije** koji vam je dodijeljen aplikacije. Također ćete to kasnije.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Stvaranje police

U Azure AD B2C, svaki korisnički doživljaj definira [pravila](active-directory-b2c-reference-policies.md). Aplikacija web sadrži tri identiteta sučelja: registracija za Office, prijavite se i uređivanje profila. Morate stvoriti jednu pravila svake vrste kao što je opisano u [članku referenca pravila](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy). Kada stvorite tri pravila, ne zaboravite:

- Odaberite **zaslonsko ime** i ostale registracije atribute Pravilnik za prijavu.
- Odaberite aplikaciju zahtjevima **zaslonsko ime** i **ID objekta** u svakoj pravila. Možete odabrati druge zahtjevima.
- Nakon stvaranja, kopirajte **naziv** svakog pravila. Treba imati prefiks `b2c_1_`. Morat ćete nazive pravila kasnije.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Kada stvorite tri police, spremni ste za stvaranje aplikacije.

Imajte na umu da ovaj članak obuhvaća kako koristiti pravila koja ste upravo stvorili. Da biste saznali kako funkcioniraju pravilnici u Azure AD B2C, započnite [.NET web app uvodni vodič](active-directory-b2c-devquickstarts-web-dotnet.md).

## <a name="download-the-code"></a>Preuzimanje kod

Kod za ovaj Praktični vodič [je spremljen na GitHub](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet). Da biste sastavili uzorka tijekom rada, možete [preuzeti skeleton projekt kao .zip datoteku](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet/archive/skeleton.zip). Mogu Kloniraj i na skeleton:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet.git
```

Aplikaciju dovršene nije [dostupan kao .zip datoteku](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet/archive/complete.zip) ili u `complete` podružnice u istom spremištu.

Kada preuzmete uzorak koda, otvorite datoteku .sln Visual Studio za početak rada.

## <a name="configure-the-task-web-app"></a>Konfiguriranje web-aplikaciji zadatka

Da biste dobili `TaskWebApp` možete komunicirati s Azure AD B2C, morate unijeti nekoliko uobičajenih parametre. U na `TaskWebApp` otvaranje projekta u `web.config` datoteka u korijenskom direktoriju projekta i zamijeni vrijednosti u na `<appSettings>` sekcije. Možete ostaviti na `AadInstance`, `RedirectUri`, a `TaskServiceUrl` vrijednosti kao što su.

```
  <appSettings>
    
    ...
    
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
    <add key="api:TaskServiceUrl" value="https://aadb2cplayground.azurewebsites.net" />
  </appSettings>
```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:ClientSecret" value="E:i~5GHYRF$Y7BcM" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
    <add key="api:TaskServiceUrl" value="https://aadb2cplayground.azurewebsites.net" />
  </appSettings>

## <a name="get-access-tokens-and-call-the-task-api"></a>Dobiti pristup tokeni i nazovite zadatka API-JA

U ovom se odjeljku obrađuje kako koristiti token primljene prijava s Azure AD B2C da biste pristupili API koji je i osigurani s Azure AD B2C web-mjesto.

Ovaj članak obuhvaća pojedinosti o tome kako sigurne na API-JA. Da biste saznali kako web-mjesto API sigurno potvrđuje zahtjeve pomoću Azure AD B2C, pogledajte [članak API na webu Uvod u rad](active-directory-b2c-devquickstarts-api-dotnet.md).

### <a name="save-the-sign-in-token"></a>Spremanje znak u tokena

Najprije provjere autentičnosti korisnika (pomoću bilo koju od police) i dobivate token za prijavu Azure AD B2C.  Ako niste sigurni kako izvršiti pravila, vratite se i pokušajte s [.NET web app uvodni vodič](active-directory-b2c-devquickstarts-web-dotnet.md) dodatne informacije o osnovama Azure AD B2C.

Otvorite datoteku `App_Start\Startup.Auth.cs`.  Postoji jedan važnih promjena koje morate unijeti da biste na `OpenIdConnectAuthenticationOptions` -morate postaviti `SaveSignInToken = true`.

```C#
// App_Start\Startup.Auth.cs

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
        AuthenticationFailed = OnAuthenticationFailed,
    },
    Scope = "openid",
    ResponseType = "id_token",

    TokenValidationParameters = new TokenValidationParameters
    {
        NameClaimType = "name",
        
        // Add this line to reserve the sign in token for later use
        SaveSigninToken = true,
    },
};
```

### <a name="get-a-token-in-the-controllers"></a>Dobivanje token u na kontrolera

U `TasksController` je zadužen za komunikaciju s weba API-JA, slanje zahtjeva za HTTP API-JA za čitanje, stvaranje i brisanje zadataka.  Becuase je u API osigurani po Azure AD B2C, morate najprije dohvatiti tokena koje ste spremili na ovaj korak.

```C#
// Controllers\TasksController.cs

public async Task<ActionResult> Index()
{
    try { 

        var bootstrapContext = ClaimsPrincipal.Current.Identities.First().BootstrapContext as System.IdentityModel.Tokens.BootstrapContext;
        
    ...
}
```

U `BootstrapContext` sadrži token koji koju ste nabavili izvršavanjem nešto pravila za B2C za prijavu.

### <a name="read-tasks-from-the-web-api"></a>Čitanje zadataka s weba API-JA

Kada imate token, možete je priložiti u HTTP `GET` zahtjev u u `Authorization` zaglavlje da biste sigurno poziva `TaskService`:

```C#
// Controllers\TasksController.cs

public async Task<ActionResult> Index()
{
    try { 

        ...

        HttpClient client = new HttpClient();
        HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, serviceUrl + "/api/tasks");

        // Add the token acquired from ADAL to the request headers
        request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", bootstrapContext.Token);
        HttpResponseMessage response = await client.SendAsync(request);

        if (response.IsSuccessStatusCode)
        {
            String responseString = await response.Content.ReadAsStringAsync();
            JArray tasks = JArray.Parse(responseString);
            ViewBag.Tasks = tasks;
            return View();
        }
        else
        {
            // If the call failed with access denied, show the user an error indicating they might need to sign-in again.
            if (response.StatusCode == System.Net.HttpStatusCode.Unauthorized)
            {
                return new RedirectResult("/Error?message=Error: " + response.ReasonPhrase + " You might need to sign in again.");
            }
        }

        return new RedirectResult("/Error?message=An Error Occurred Reading To Do List: " + response.StatusCode);
    }
    catch (Exception ex)
    {
        return new RedirectResult("/Error?message=An Error Occurred Reading To Do List: " + ex.Message);
    }
}

```

### <a name="create-and-delete-tasks-on-the-web-api"></a>Stvaranje i brisanje zadataka na webu API-JA

Slijedite isti uzorak prilikom slanja `POST` i `DELETE` zahtjevima za web API-JA, pomoću na `BootstrapContext` za dohvaćanje token za prijavu. Ne možemo implementirana akciju stvaranje umjesto vas. Možete pokušati dovršetka akciju brisanja u `TasksController.cs`.

## <a name="run-the-sample-app"></a>Pokrenite aplikaciju uzorka

Na kraju, stvaranje i pokretanje aplikacije. Prijavite se i prijavite se i stvaranje zadataka za prijavljeni korisnik. Odjavljivanje i prijavite se kao drugi korisnik. Stvaranje zadataka za tog korisnika. Obratite pozornost na to kako Zadaci su pohranjene po korisniku na API-JA, jer u API izdvaja identitet korisnika iz token primi.

Za referencu, dovršene [prikazuje se kao .zip datoteku](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet/archive/complete.zip). Mogu Kloniraj i to iz GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet.git```

<!--

## Next steps

You can now move on to more advanced B2C topics. You might try:

[Call a web API from a web app]()

[Customize the UX for a B2C app]()

-->
