<properties
   pageTitle="Autorizacija u složene aplikacije | Microsoft Azure"
   description="Kako izvršiti provjeru autentičnosti u složene aplikacije"
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/02/2016"
   ms.author="mwasson"/>

# <a name="role-based-and-resource-based-authorization-in-multitenant-applications"></a>Na temelju uloga i sustavom resursa autorizacije u složene aplikacije

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Ovaj je članak [dio niza]. Također je dovršena [primjer aplikacije] koja se isporučuje se uz ovaj niz.

Aplikacija ASP.NET osnovne 1.0 je naš [implementaciju referencu] . U ovom članku smo ćete pogledajte dva općenite postupke za provjeru autentičnosti, pomoću autorizacije API-ji navedenih u ASP.NET osnovne 1.0.

-   **Na temelju uloga autorizacije**. Dopuštanja akcije koja se temelji na uloge dodijeljene korisnika. Na primjer, neke akcije zahtijevaju administratorske uloge.
-   **Autorizacija utemeljen na resursa**. Dopuštanja akcije koja se temelji na određeni resurs. Na primjer, svaki resurs ima vlasnika. Vlasnik možete izbrisati resursa; nije moguće drugim korisnicima.

Uobičajeni aplikacija će koristiti kombinacije oboje. Ako, na primjer, da biste izbrisali resursa, korisnik mora biti resursa vlasnik _ili_ administrator.


## <a name="role-based-authorization"></a>Autorizacija na temelju uloga

[Ankete Tailspin] [ Tailspin] računala definira sljedeće uloge:

- Administrator. Možete izvršiti sve operacije CRUD na bilo kojem upitnik koji pripada taj klijent.
- Alat za stvaranje. Možete stvoriti nove ankete
- Čitač. Možete čitati sve ankete koji pripadaju klijentu

Uloge odnose na _korisnike_ aplikacije. U aplikaciji ankete korisnik je programa administratora, autora ili reader.

Rasprave definiranje i Upravljanje ulogama, potražite u članku [aplikacije uloge].

Bez obzira na to kako upravljati uloge, kod autorizacije će izgledati kao. Temeljni ASP.NET 1.0 predstavlja programa apstrakcije naziva [pravila za provjeru autentičnosti][policies]. S tom značajkom definirati pravila za provjeru autentičnosti u kodu, a zatim primijenite ta pravila za kontroler akcije. Pravilo je samostalne s kontrolerom.

### <a name="create-policies"></a>Stvaranje pravila

Da biste definirali pravilo, najprije stvorite predmete koji implementira `IAuthorizationRequirement`. To je najjednostavniji način za dobivanje glavne iz `AuthorizationHandler`. U na `Handle` način, pregledajte odgovarajući claim(s).

Slijedi primjer iz aplikacije ankete Tailspin:

```csharp
public class SurveyCreatorRequirement : AuthorizationHandler<SurveyCreatorRequirement>, IAuthorizationRequirement
{
    protected override void Handle(AuthorizationContext context, SurveyCreatorRequirement requirement)
    {
        if (context.User.HasClaim(ClaimTypes.Role, Roles.SurveyAdmin) ||
            context.User.HasClaim(ClaimTypes.Role, Roles.SurveyCreator))
        {
            context.Succeed(requirement);
        }
    }
}
```

> [AZURE.NOTE] U odjeljku [SurveyCreatorRequirement.cs]

Klase definira obavezu korisnika da biste stvorili novu anketu. Korisnik mora biti u ulozi SurveyAdmin ili SurveyCreator.

U svojoj učionici pokretanje definirati imenovani pravila koja sadrži jedan ili više preduvjeti. Ako postoji više preduvjeti, korisnik mora zadovoljiti _svaki_ zahtjev za ovlašteni. Sljedeći kod definira dva pravila:

```csharp
services.AddAuthorization(options =>
{
    options.AddPolicy(PolicyNames.RequireSurveyCreator,
        policy =>
        {
            policy.AddRequirements(new SurveyCreatorRequirement());
            policy.AddAuthenticationSchemes(CookieAuthenticationDefaults.AuthenticationScheme);
        });

    options.AddPolicy(PolicyNames.RequireSurveyAdmin,
        policy =>
        {
            policy.AddRequirements(new SurveyAdminRequirement());
            policy.AddAuthenticationSchemes(CookieAuthenticationDefaults.AuthenticationScheme);
        });
});
```

> [AZURE.NOTE] U odjeljku [Startup.cs]

Kod postavlja i shemu provjere autentičnosti koja govori ASP.NET koji proizvod provjere autentičnosti pokretati ne uspijete autorizacije. U ovom slučaju ne možemo navesti provjere autentičnosti proizvod kolačića jer proizvod provjere autentičnosti kolačića možete preusmjeriti korisnika na stranici "Zabranjeno". Mjesto na stranici zabranjeno postavljen AccessDeniedPath mogućnost za proizvod kolačića; potražite u članku [Konfiguriranje provjere autentičnosti proizvod].

### <a name="authorize-controller-actions"></a>Autorizirajte kontroler akcije

Na kraju, da biste autorizirali akcije u programa kontroler MVC postavljanje pravila u na `Authorize` atribut:

```csharp
[Authorize(Policy = "SurveyCreatorRequirement")]
public IActionResult Create()
{
    // ...
}
```

U starijim verzijama programa ASP.NET bi postavite svojstvo **uloge** na atribut:

```csharp
// old way
[Authorize(Roles = "SurveyCreator")]

```

I dalje podržane u ASP.NET osnovne 1.0, ali ima neke Nedostaci usporedbi sa autorizacije pravila:

-   Pretpostavlja da se vrsta određenog zahtjeva. Pravila možete potražiti na bilo koju vrstu zahtjeva. Uloge su samo vrstu zahtjeva.
-   Naziv uloge je programiranih u atribut. S pravilima, logika autorizacije je na jednom mjestu olakšano ažuriranje ili čak i učitavanje iz konfiguracijske postavke.
-   Pravila omogućuju složenije autorizacije odluke (npr., dob > = 21) koji ne može se izraziti prema članstvu jednostavne uloga.

## <a name="resource-based-authorization"></a>Autorizacije resursa koji se temelje

_Resurs temelje autorizacije_ pojavljuje se kad god autorizacija ovisi o određeni resurs koji mogu utjecati na postupak. U aplikaciji Tailspin ankete svaki upitnik ima vlasnika i nula-prema-više suradnika.

-   Vlasnik može čitanje, ažuriranje, brisanje, objavite, i poništavanje objave upitnik.
-   Vlasnik možete dodijeliti upitnik suradnika.
-   Suradnici mogu čitati i ažurirati upitnik.

Imajte na umu da "vlasnik" i "suradničke" ne uloge aplikacije; pohranjuju se po ankete u bazi podataka za aplikacije. Da biste provjerili je li korisnik može se izbrisati upitnik, na primjer, aplikaciju provjerava je li korisnik vlasnik za tu anketu.

U ASP.NET osnovne 1.0 implementirati utemeljen na resursa autorizacije dijelovima koji potječu od **AuthorizationHandler** i nadjačavanje metodu **rukovati** .

```csharp
public class SurveyAuthorizationHandler : AuthorizationHandler<OperationAuthorizationRequirement, Survey>
{
     protected override void Handle(AuthorizationContext context, OperationAuthorizationRequirement operation, Survey resource)
    {
    }
}
```

Obratite pozornost na to klase svakako uneseni za objekte upitnika.  Registrirajte se za predmete za DI prilikom pokretanja:

```csharp
services.AddSingleton<IAuthorizationHandler>(factory =>
{
    return new SurveyAuthorizationHandler();
});
```

Da biste izvršili autorizacije provjere, koristite **IAuthorizationService** sučelje, koje možete ubaciti u vašem kontrolera. Sljedeći kôd provjerava je li korisnik može čitati anketu:

```csharp
if (await _authorizationService.AuthorizeAsync(User, survey, Operations.Read) == false)
{
    return new HttpStatusCodeResult(403);
}
```

Jer smo prenesite na `Survey` objekt će pozivanje ovog poziva na `SurveyAuthorizationHandler`.

U kodu autorizacije dobar način je aggregate sve od korisničke dozvole na temelju uloga i sustavom resursa, a zatim potvrdite zbrajanja postavljanje protiv željeni postupak.
Slijedi primjer iz aplikacije ankete. Aplikacija definira nekoliko vrsta dozvola:

- Administrator
- Suradnik
- Alat za stvaranje
- Vlasnik
- Čitač

Aplikacije i definira skup moguće operacije na ankete:

- Stvaranje
- Čitanje
- Ažuriranje
- Brisanje
- Objavljivanje
- Unpublsh

Sljedeći kod stvara popis dozvole za određeni korisnički i ankete. Obratite pozornost na to da kod razmatra i uloge korisnika aplikacije i polja vlasnik/suradnika u upitnik.

```csharp
protected override void Handle(AuthorizationContext context, OperationAuthorizationRequirement operation, Survey resource)
{
    var permissions = new List<UserPermissionType>();
    string userTenantId = context.User.GetTenantIdValue();
    int userId = ClaimsPrincipalExtensions.GetUserKey(context.User);
    string user = context.User.GetUserName();

    if (resource.TenantId == userTenantId)
    {
        // Admin can do anything, as long as the resource belongs to the admin's tenant.
        if (context.User.HasClaim(ClaimTypes.Role, Roles.SurveyAdmin))
        {
            context.Succeed(operation);
            return;
        }

        if (context.User.HasClaim(ClaimTypes.Role, Roles.SurveyCreator))
        {
            permissions.Add(UserPermissionType.Creator);
        }
        else
        {
            permissions.Add(UserPermissionType.Reader);
        }

        if (resource.OwnerId == userId)
        {
            permissions.Add(UserPermissionType.Owner);
        }
    }
    if (resource.Contributors != null && resource.Contributors.Any(x => x.UserId == userId))
    {
        permissions.Add(UserPermissionType.Contributor);
    }
    if (ValidateUserPermissions[operation](permissions))
    {
        context.Succeed(operation);
    }
}
```

> [AZURE.NOTE] Potražite u članku [SurveyAuthorizationHandler.cs].

U aplikaciji za više klijentu osigurati da dozvole ne "osipanje" s podacima drugi klijent. U aplikaciji ankete dozvolu za suradnika je dopušteno preko klijenata &mdash; imenovanje nekog od drugi klijent kao u contriubutor. Druge vrste dozvola su ograničeni resurse koji pripadaju klijentu za tog korisnika. Da biste nametnuli taj zahtjev, kod provjerava ID klijenta prije dodjelu dozvola. (U `TenantId` polja kao što je dodijeljen stvaranja upitnik.)

Sljedeći je korak da biste provjerili operacija (čitanje, ažuriranje, brisanje, itd) na temelju dozvola. Aplikaciju ankete implementira ovaj korak pomoću tablica za traženje funkcija:

```csharp
static readonly Dictionary<OperationAuthorizationRequirement, Func<List<UserPermissionType>, bool>> ValidateUserPermissions
    = new Dictionary<OperationAuthorizationRequirement, Func<List<UserPermissionType>, bool>>

    {
        { Operations.Create, x => x.Contains(UserPermissionType.Creator) },

        { Operations.Read, x => x.Contains(UserPermissionType.Creator) ||
                                x.Contains(UserPermissionType.Reader) ||
                                x.Contains(UserPermissionType.Contributor) ||
                                x.Contains(UserPermissionType.Owner) },

        { Operations.Update, x => x.Contains(UserPermissionType.Contributor) ||
                                x.Contains(UserPermissionType.Owner) },

        { Operations.Delete, x => x.Contains(UserPermissionType.Owner) },

        { Operations.Publish, x => x.Contains(UserPermissionType.Owner) },

        { Operations.UnPublish, x => x.Contains(UserPermissionType.Owner) }
    };
```


## <a name="next-steps"></a>Daljnji koraci

- Pročitajte sljedeći članak u ovom nizu: [Zaštita s pozadinskom web-API-JA u složene aplikacije][web-api]
- Da biste saznali više o autorizacije resursa koji se temelje u ASP.NET 1.0 Core, potražite u članku [Autorizacija koji se temelji na resursa][rbac].

<!-- Links -->
[Tailspin]: guidance-multitenant-identity-tailspin.md
[dio niza]: guidance-multitenant-identity.md
[Uloge aplikacije]: guidance-multitenant-identity-app-roles.md
[policies]: https://docs.asp.net/en/latest/security/authorization/policies.html
[rbac]: https://docs.asp.net/en/latest/security/authorization/resourcebased.html
[Implementacija reference]: guidance-multitenant-identity-tailspin.md
[SurveyCreatorRequirement.cs]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/src/Tailspin.Surveys.Security/Policy/SurveyCreatorRequirement.cs
[Startup.CS]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/src/Tailspin.Surveys.Web/Startup.cs
[Konfiguriranje provjere autentičnosti proizvod]: guidance-multitenant-identity-authenticate.md#configuring-the-authentication-middleware
[SurveyAuthorizationHandler.cs]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/src/Tailspin.Surveys.Security/Policy/SurveyAuthorizationHandler.cs
[primjer aplikacije]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps
[web-api]: guidance-multitenant-identity-web-api.md
