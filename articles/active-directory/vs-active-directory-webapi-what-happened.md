<properties
    pageTitle="Što se dogodilo s WebApi projektu (Visual Studio Azure Active Directory povezani servisa) | Microsoft Azure "
    description="U članku se opisuje što se događa s projektom MVC WebApi s Azure AD pomoću Visual Studio"
  services="active-directory"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="web"
    ms.tgt_pltfrm="vs-what-happened"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="tarcher"/>

# <a name="what-happened-to-my-webapi-project-visual-studio-azure-active-directory-connected-service"></a>Što se dogodilo s WebApi projektu (Visual Studio Azure Active Directory povezani servisa)

> [AZURE.SELECTOR]
> - [Početak rada](vs-active-directory-webapi-getting-started.md)
> - [Šta se dogodilo](vs-active-directory-webapi-what-happened.md)

##<a name="references-have-been-added"></a>Dodane su reference

###<a name="nuget-package-references"></a>Reference NuGet paketa

- `Microsoft.Owin`
- `Microsoft.Owin.Host.SystemWeb`
- `Microsoft.Owin.Security`
- `Microsoft.Owin.Security.ActiveDirectory`
- `Microsoft.Owin.Security.Jwt`
- `Microsoft.Owin.Security.OAuth`
- `Owin`
- `System.IdentityModel.Tokens.Jwt`

###<a name="net-references"></a>Reference za .NET

- `Microsoft.Owin`
- `Microsoft.Owin.Host.SystemWeb`
- `Microsoft.Owin.Security`
- `Microsoft.Owin.Security.ActiveDirectory`
- `Microsoft.Owin.Security.Jwt`
- `Microsoft.Owin.Security.OAuth`
- `Owin`
- `System.IdentityModel.Tokens.Jwt`

##<a name="code-changes"></a>Kod promjene

###<a name="code-files-were-added-to-your-project"></a>Kod datoteke koje su dodane u projekt

Provjera autentičnosti početnu klasu, **App_Start/Startup.Auth.cs** dodan u projekt koji sadrži pokretanje logike za provjeru autentičnosti Azure AD.

###<a name="startup-code-was-added-to-your-project"></a>Pokretanje sustava dodan u projekt

Ako već imate početnu klasu u projektu, način **konfiguracije** ažurirana da biste uključili poziv `ConfigureAuth(app)`. U suprotnom početnu klasu dodan u projekt.


###<a name="your-appconfig-or-webconfig-file-has-new-configuration-values"></a>Datoteku app.config ili web.config ima nove vrijednosti konfiguracije.

Dodane su sljedeće unose konfiguracije.
```
    `<appSettings>
            <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
            <add key="ida:Tenant" value="Your selected Azure AD Tenant" />
            <add key="ida:Audience" value="The App ID Uri from the wizard" />
    </appSettings>`
```

###<a name="an-azure-ad-app-was-created"></a>Stvorena je Azure AD aplikacije

Aplikaciju za Azure AD stvorena u direktoriju koji ste odabrali u čarobnjaku.

[Dodatne informacije o servisu Azure Active Directory](https://azure.microsoft.com/services/active-directory/)

##<a name="if-i-checked-disable-individual-user-accounts-authentication-what-additional-changes-were-made-to-my-project"></a>Ako je li potvrđen okvir *onemogućite provjeru autentičnosti pojedinačne korisničke račune*, koje dodatne su promjene izvršene u projektu?
Uklonjeni su NuGet paket reference, a datoteka su uklonjene i sigurnosnu kopiju. Ovisno o stanju projekta, možda ćete morati ručno dodatne reference i datoteke uklonite ili izmjena šifru po potrebi.

###<a name="nuget-package-references-removed-for-those-present"></a>NuGet paket reference ukloniti (za one izlaganje)

- `Microsoft.AspNet.Identity.Core`
- `Microsoft.AspNet.Identity.EntityFramework`
- `Microsoft.AspNet.Identity.Owin`

###<a name="code-files-backed-up-and-removed-for-those-present"></a>Kod datoteke sigurnosne kopije i ukloniti (za one izlaganje)

Svaki od ovih datoteka je sigurnosnu kopiju i ukloniti iz projekta. Datoteke sigurnosne kopije nalaze se u mapi "Sigurnosno kopiranje" u korijenu direktorija u projekt.

- `App_Start\IdentityConfig.cs`
- `Controllers\AccountController.cs`
- `Controllers\ManageController.cs`
- `Models\IdentityModels.cs`
- `Providers\ApplicationOAuthProvider.cs`

###<a name="code-files-backed-up-for-those-present"></a>Kod datoteke sigurnosne kopije (za one izlaganje)

Svaki od ovih datoteka je sigurnosnu kopiju prije nego je zamijenjena. Datoteke sigurnosne kopije nalaze se u mapi "Sigurnosno kopiranje" u korijenu direktorija u projekt.

- `Startup.cs`
- `App_Start\Startup.Auth.cs`

##<a name="if-i-checked-read-directory-data-what-additional-changes-were-made-to-my-project"></a>Ako provjere *čitanje direktorija podataka*koje dodatne su promjene izvršene u projektu?

###<a name="additional-changes-were-made-to-your-appconfig-or-webconfig"></a>Dodatne su promjene na app.config ili web.config

Dodane su sljedeće dodatne konfiguracijske stavke.

```
    `<appSettings>
        <add key="ida:Password" value="Your Azure AD App's new password" />
    </appSettings>`
```

###<a name="your-azure-active-directory-app-was-updated"></a>Ažurirana Azure Active Directory aplikacije
Azure Active Directory aplikacije ažurirana da biste uključili dozvola za *čitanje direktorija podataka* i dodatni ključ koji je stvoren koji zatim korišten kao *Ida: lozinku* u na `web.config` datoteku.

[Dodatne informacije o servisu Azure Active Directory](https://azure.microsoft.com/services/active-directory/)
