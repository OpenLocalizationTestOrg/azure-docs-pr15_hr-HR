<properties
    pageTitle="Azure AD 2.0 .NET API na webu | Microsoft Azure"
    description="Sastavljanje Web Api MVC .NET koje prihvaća tokena iz obje osobnog Microsoftova Account i računa tvrtke ili obrazovne ustanove."
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
    ms.date="10/10/2016"
    ms.author="dastrock"/>

# <a name="secure-an-mvc-web-api"></a>Sigurne web MVC API-JA

S Azure Active Directory krajnju točku 2.0, možete zaštititi i Web API pomoću [OAuth 2.0](active-directory-v2-protocols.md#oauth2-authorization-code-flow) pristupna tokena Omogućavanje korisnicima i osobni Microsoftov račun i rad ili školske račune na siguran pristup webu API-jem.

> [AZURE.NOTE]
    Neke scenarije servisa Azure Active Directory i značajke podržava krajnju točku 2.0.  Da biste utvrdili koristite krajnju točku 2.0, Saznajte više o [2.0 ograničenja](active-directory-v2-limitations.md).

U ASP.NET web-mjesto API-ji koje možete izvršiti to pomoću tvrtke Microsoft OWIN proizvod uključeni u .NET Framework 4,5.  U nastavku ćemo koristiti OWIN da biste sastavili Web API MVC "Da biste učinili popis" koji klijentima omogućuje stvaranje i pročitajte zadataka s popisa obaveza korisnika.  Na webu API će provjeriti da zahtjevi za sadrže token valjan pristup i Odbaci sve zahtjeve za koje na zaštićenom usmjeravanje proći provjere valjanosti.  Ovaj primjer gradnje pomoću Visual Studio 2015.

## <a name="download"></a>Preuzimanje
Kod za ovog praktičnog vodiča je spremljen [na GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet).  Da biste pratili, možete [preuzeti aplikacije skeleton kao u .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) ili Kloniraj na skeleton:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git
```

Aplikaciju skeleton obuhvaća sve predloženog kodove za jednostavne API-JA, ali nema svih dijelova identitet vezanih. Ako ne želite pratiti, koje možete umjesto toga Kloniraj ili [preuzmete dovršeni uzorka](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip).

```
git clone https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git
```

## <a name="register-an-app"></a>Registracija aplikacije
Stvaranje nove aplikacije na [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)ili slijedite ove [detaljne upute](active-directory-v2-app-registration.md).  Obavezno:

- Kopiraj dolje **Id aplikacije** dodijeljene aplikacije, morat ćete ga uskoro.

Rješenje visual studio sadrži i u "TodoListClient", koji je jednostavno WPF aplikacija.  Na TodoListClient služi da bismo pokazali kako korisnik znakove u te kako klijent izdati zahtjeve za API na webu.  U ovom slučaju na TodoListClient i u TodoListService prikazani su iste aplikacije.  Da biste konfigurirali na TodoListClient, trebali biste i:

- Dodajte platformu **mobilne** aplikacije.


## <a name="install-owin"></a>Instalacija OWIN

Sad kad ste registrirali aplikaciju, morate postaviti aplikacije možete komunicirati s krajnju točku 2.0 da biste provjerili valjanost dolaznih zahtjeva i tokeni.

- Da biste započeli, otvorite rješenje i dodajte pakete NuGet OWIN proizvod projekt TodoListService korištenju konzole za Upravitelj paketa.

```
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
```

## <a name="configure-oauth-authentication"></a>Konfiguriranje provjere autentičnosti OAuth

- Dodavanje programa OWIN početnu klasu projekt TodoListService pod nazivom `Startup.cs`.  Desni klik na projektu--> **Dodaj** --> traženje "OWIN"-->**Nova stavka** .  Proizvod OWIN će pozivanje na `Configuration(…)` način prilikom pokretanja aplikacije.
- Promjena klase izvješća da biste `public partial class Startup` -već Implementirali smo dio klase umjesto vas u drugu datoteku.  U na `Configuration(…)` način, provjerite na poziv ConfgureAuth(...) da biste postavili provjere autentičnosti za web-aplikacije.

```C#
public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

- Otvorite datoteku `App_Start\Startup.Auth.cs` i implementacija u `ConfigureAuth(…)` način, postavit će Web API da biste prihvatili tokena iz krajnju točku 2.0.

```C#
public void ConfigureAuth(IAppBuilder app)
{
        var tvps = new TokenValidationParameters
        {
                // In this app, the TodoListClient and TodoListService
                // are represented using the same Application Id - we use
                // the Application Id to represent the audience, or the
                // intended recipient of tokens.

                ValidAudience = clientId,

                // In a real applicaiton, you might use issuer validation to
                // verify that the user's organization (if applicable) has
                // signed up for the app.  Here, we'll just turn it off.

                ValidateIssuer = false,
        };

        // Set up the OWIN pipeline to use OAuth 2.0 Bearer authentication.
        // The options provided here tell the middleware about the type of tokens
        // that will be recieved, which are JWTs for the v2.0 endpoint.

        // NOTE: The usual WindowsAzureActiveDirectoryBearerAuthenticaitonMiddleware uses a
        // metadata endpoint which is not supported by the v2.0 endpoint.  Instead, this
        // OpenIdConenctCachingSecurityTokenProvider can be used to fetch & use the OpenIdConnect
        // metadata document.

        app.UseOAuthBearerAuthentication(new OAuthBearerAuthenticationOptions
        {
                AccessTokenFormat = new Microsoft.Owin.Security.Jwt.JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider("https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration")),
        });
}
```

- Sada možete koristiti `[Authorize]` atribute zaštiti kontrolera i akcije s provjerom autentičnosti nošenja OAuth 2.0.  Ukrašavanje na `Controllers\TodoListController.cs` predmete s oznaku ovlasti.  To će obavezno korisnika da biste se prijavili prije pristupanja toj stranici.

```C#
[Authorize]
public class TodoListController : ApiController
{
```

- Kada ovlašteni pozivatelja uspješno poziva jednu od na `TodoListController` API-ji, akcije možda će biti potrebno pristup informacijama o pozivatelju.  OWIN omogućuje pristup zahtjevima unutar token nošenja putem na `ClaimsPrincpal` objekt.  

```C#
public IEnumerable<TodoItem> Get()
{
    // You can use the ClaimsPrincipal to access information about the
    // user making the call.  In this case, we use the 'sub' or
    // NameIdentifier claim to serve as a key for the tasks in the data store.

    Claim subject = ClaimsPrincipal.Current.FindFirst(ClaimTypes.NameIdentifier);

    return from todo in todoBag
           where todo.Owner == subject.Value
           select todo;
}
```

-   Na kraju, otvorite na `web.config` datoteku u korijenskoj mapi TodoListService projekta, a potom unesite konfiguracijskih vrijednosti u na `<appSettings>` sekciju.
  - Vaše `ida:Audience` je **Id aplikacije** aplikacije koje ste unijeli na portalu.

## <a name="configure-the-client-app"></a>Konfiguriranje aplikacije klijenta
Prije nego što možete vidjeti popis servisa obveze u akciji, morate konfigurirati klijenta popisa popis obveza da bi mogli dobiti tokena iz krajnju točku 2.0 i upućivanje poziva na servis.

- U programu project TodoListClient otvorite `App.config` , a zatim unesite konfiguracijskih vrijednosti u na `<appSettings>` sekciju.
  - Vaše `ida:ClientId` Id aplikacije koji ste kopirali na portalu.

Na kraju, očistite, stvaranje i pokretanje svaki projekt!  Sada imate Web API MVC .NET koje prihvaća tokena iz obje Microsoftovi računi i računi tvrtke ili obrazovne ustanove.  Prijavite na TodoListClient i poziva vaše web-mjesto API-ja za dodavanje zadataka na korisnikovu popisu zadataka.

Za referencu, dovršeni uzorka (bez konfiguracijskih vrijednosti) [prikazuje se kao .zip ovdje](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/complete.zip)ili pak mogu Kloniraj ga iz GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git```

## <a name="next-steps"></a>Daljnji koraci
Sada se može pomaknuti na dodatne teme.  Preporučujemo vam da biste isprobali:

[Pozivanje API web-mjesto iz web-aplikacije >>](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md)

Za dodatne resurse, pogledajte:
- [Vodič za razvojne inženjere 2.0 >>](active-directory-appmodel-v2-overview.md)
- [Oznaka StackOverflow "azure-aktivno-directory" >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Sigurnosna ažuriranja za naši proizvodi

Pozivamo vas da biste dobili obavijesti kada doći do sigurnošću potražite na [toj stranici](https://technet.microsoft.com/security/dd252948) i pretplata na upozorenja savjeta za sigurnost.
