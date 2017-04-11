<properties
    pageTitle=".NET Azure AD Uvod | Microsoft Azure"
    description="Upute za sastavljanje Web API MVC .NET koji se integrira s Azure AD za provjeru autentičnosti i ovlaštenja."
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


# <a name="protect-a-web-api-using-bearer-tokens-from-azure-ad"></a>Zaštita Web API pomoću nošenja tokena iz Azure AD

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Ako ste gradnje aplikacije koja omogućuje pristup zaštićeni resursi morat ćete znati kako se zaštititi tih resursa iz unwarranted programa access.
Azure AD olakšava jednostavne i jasan da biste zaštitili API OAuth nošenja 2.0 pristupna tokena pomoću samo nekoliko redaka koda web-mjesto.

U Asp.NET web-aplikacijama koje možete izvršiti to pomoću Microsoftovu implementaciju sustava OWIN proizvod utemeljenih na zajednice uključeni u .NET Framework 4,5.  Ovdje ćete koristiti OWIN za sastavljanje web "Da biste učinili popis" API koji:
-   Određuje API koji su zaštićeni.
-   Provjerava sadrže li pozive Web API je valjan pristup tokena.

Da biste to učinili, morat ćete:

1. Registracija aplikacije s Azure AD
2. Postavljanje aplikacije da biste koristili kanal OWIN provjeru autentičnosti.
3. Konfiguriranje klijentske aplikacije da biste uputili poziv u da biste učinili popisa Web API-JA

Da biste počeli, [Preuzmite skeleton aplikacije](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/skeleton.zip) ili [preuzeti dovršene uzorka](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip).  Svaka je rješenje za Visual Studio 2013.  Trebat ćete klijent za Azure AD u kojoj se registrirati aplikacije.  Ako ga već imam [upute da biste ga nabavili](active-directory-howto-tenant.md).


## <a name="1--register-an-application-with-azure-ad"></a>*1. Registracija aplikacije s Azure AD*
Radi zaštite vaše aplikacije, najprije morat ćete stvoriti aplikaciju na klijentu i pošaljite Azure AD nekoliko ključa dijelovi informacija.

-   Prijavite se na [Portal za upravljanje Azure](https://manage.windowsazure.com)
-   U navigacijskom oknu lijevoj strani kliknite **Servisa Active Directory**
-   Odaberite klijenta u kojoj se registrirati aplikaciju.
-   Kliknite karticu **aplikacije** pa kliknite **Dodaj** u donjem ladica.
-   Slijedite upite i stvaranje je nove **web-aplikacije i/ili WebAPI**.
    -   **Naziv** aplikacije opisane su aplikacije krajnjim korisnicima.  Unesite "popis servisa".
    -   Kombinacija sheme i niz koju Azure AD koristila da biste se vratili sve tokeni aplikacije zatražio je **Preusmjeravanje Uri** . Unesite `https://localhost:44321/` za tu vrijednost.
-   Nakon što ste dovršili registraciju, idite na karticu **Konfiguracija** i pronađite polje **ID URI aplikacije** .  Unesite identifikatora specifične za klijenta za tu vrijednost, npr.`https://contoso.onmicrosoft.com/TodoListService`
- Spremanje konfiguracije.  Na portalu ostavite otvorenim – i morate registrirati klijentska aplikacija uskoro.

## <a name="2-set-up-your-app-to-use-the-owin-authentication-pipeline"></a>*2. postavljanje aplikacije da biste koristili kanal OWIN provjera autentičnosti*

Sad kad ste registrirali aplikaciju s Azure AD, morate postaviti aplikacije možete komunicirati s Azure AD da biste provjerili valjanost dolaznih zahtjeva i tokeni.

-   Da biste započeli, otvorite rješenje i dodajte pakete NuGet OWIN proizvod projekt TodoListService korištenju konzole za Upravitelj paketa.

```
PM> Install-Package Microsoft.Owin.Security.ActiveDirectory -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
```

-   Dodavanje programa OWIN početnu klasu projekt TodoListService pod nazivom `Startup.cs`.  Desni klik na projektu--> **Dodaj** --> traženje "OWIN"-->**Nova stavka** .  Proizvod OWIN će pozivanje na `Configuration(…)` način prilikom pokretanja aplikacije.
-   Promjena klase izvješća da biste `public partial class Startup` -već Implementirali smo dio klase umjesto vas u drugu datoteku.  U na `Configuration(…)` način, provjerite na poziv ConfgureAuth(...) da biste postavili provjere autentičnosti za web-aplikacije.

```C#
public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

-   Otvorite datoteku `App_Start\Startup.Auth.cs` i implementacija u `ConfigureAuth(…)` način.  Parametri unesete u `WindowsAzureActiveDirectoryBearerAuthenticationOptions` će vam poslužiti kao koordinate za aplikaciju možete komunicirati s Azure AD.

```C#
public void ConfigureAuth(IAppBuilder app)
{
    app.UseWindowsAzureActiveDirectoryBearerAuthentication(
        new WindowsAzureActiveDirectoryBearerAuthenticationOptions
        {
            Audience = ConfigurationManager.AppSettings["ida:Audience"],
            Tenant = ConfigurationManager.AppSettings["ida:Tenant"]
        });
}
```

-   Sada možete koristiti `[Authorize]` atribute zaštiti kontrolera i akcije s provjerom autentičnosti nošenja JWT.  Ukrašavanje na `Controllers\TodoListController.cs` predmete s oznaku ovlasti.  To će obavezno korisnika da biste se prijavili prije pristupanja toj stranici.

```C#
[Authorize]
public class TodoListController : ApiController
{
```

- Kada ovlašteni pozivatelja uspješno poziva jednu od na `TodoListController` API-ji, akcije možda će biti potrebno pristup informacijama o pozivatelju.  OWIN omogućuje pristup zahtjevima unutar token nošenja putem na `ClaimsPrincpal` objekt.  
- Uobičajeni zahtjev za web-mjesto API-ji je da biste provjerili valjanost "dosezi" koje su prisutne u token – Time se osigurava da krajnji korisnik ima pristanak za dozvole koje su potrebne za pristup servisu popis obveze:

```C#
public IEnumerable<TodoItem> Get()
{
    // user_impersonation is the default permission exposed by applications in AAD
    if (ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/scope").Value != "user_impersonation")
    {
        throw new HttpResponseException(new HttpResponseMessage {
          StatusCode = HttpStatusCode.Unauthorized,
          ReasonPhrase = "The Scope claim does not contain 'user_impersonation' or scope claim not found"
        });
    }
    ...
}
```

-   Na kraju, otvorite na `web.config` datoteku u korijenskoj mapi TodoListService projekta, a potom unesite konfiguracijskih vrijednosti u na `<appSettings>` sekciju.
  - U `ida:Tenant` je naziv Azure AD klijent, primjerice "contoso.onmicrosoft.com".
  - Vaše `ida:Audience` je ID URI aplikacije aplikacije koje ste unijeli na portalu za Azure.

## <a name="3--configure-a-client-application--run-the-service"></a>*3. konfigurirajte klijentska aplikacija i pokrenuti servis*
Prije no što vidite usluge popisa obveze u praksi, morate konfigurirati klijent obveze popis tako da ga možete zatražiti tokeni AAD i upućivanje poziva na servis.

- Vratite se na [Portal za upravljanje Azure](https://manage.windowsazure.com)
- Stvorite novu aplikaciju u klijentu za Azure AD pa odaberite **Izvorni klijentske aplikacije** u rezultata upita.
    -   **Naziv** aplikacije opisane su aplikacije krajnjim korisnicima
    -   Unesite `http://TodoListClient/` za **Preusmjeravanje Uri** vrijednost.
- Nakon što ste dovršili registraciju, AAD će dodijeliti aplikacije jedinstveni **Id klijenta**. Potreban vam je tu vrijednost u sljedećim koracima, pa ga kopirali na kartici Konfiguriraj.
- I na kartici **Konfiguriraj** pronađite odjeljak "Dozvole za ostale programe". Kliknuti "Dodaj aplikaciju." Na padajućem izborniku "Prikaz" odaberite "Sve aplikacije" pa kliknite gornju kvačicu. Pronađite i kliknite da biste učinili popis uslugu i kliknite kvačicu dolje da biste dodali aplikaciju. Odaberite "Da biste učinili popis servis za pristup" na padajućem izborniku "Dodijeliti dozvole", a spremanje konfiguracije.


- U Visual Studio, otvorite `App.config` u TodoListClient projekta, a zatim unesite konfiguracijskih vrijednosti u na `<appSettings>` sekciju.
  - U `ida:Tenant` je naziv Azure AD klijent, primjerice "contoso.onmicrosoft.com".
  - Vaše `ida:ClientId` aplikacije ID-a koji ste kopirali na portalu Azure.
  - Vaše `todo:TodoListResourceId` je ID URI aplikacije aplikacije da biste učinili servisa popisa koje ste unijeli na portalu za Azure.

Na kraju, očistite, stvaranje i pokretanje svaki projekt!  Ako to već niste učinili, sada je vrijeme da biste stvorili novog korisnika na klijentu s na *. domenu onmicrosoft.com.  Prijavite se u klijent učinite popisa s tog korisnika i dodati neke zadatke korisnika da biste učinili popis.

Za referencu, dovršeni uzorka (bez konfiguracijskih vrijednosti) navedeni su [u nastavku](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip).  Možete odmah premjestiti više scenariji dodatne identitet.

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
