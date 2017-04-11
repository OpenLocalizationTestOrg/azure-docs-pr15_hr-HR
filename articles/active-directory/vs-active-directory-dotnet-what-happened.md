<properties
    pageTitle="Što se dogodilo s MVC projektu (Visual Studio Azure Active Directory povezani servisa) | Microsoft Azure "
    description="U članku se opisuje što se događa s projektom MVC kada se povežete s Azure AD pomoću Visual Studio povezani servisi"
    services="active-directory"
    documentationCenter="na"
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

# <a name="what-happened-to-my-mvc-project-visual-studio-azure-active-directory-connected-service"></a>Što se dogodilo s MVC projektu (Visual Studio Azure Active Directory povezani servisa)?

> [AZURE.SELECTOR]
> - [Početak rada](vs-active-directory-dotnet-getting-started.md)
> - [Šta se dogodilo](vs-active-directory-dotnet-what-happened.md)



## <a name="references-have-been-added"></a>Dodane su reference

### <a name="nuget-package-references"></a>Reference NuGet paketa

- **Microsoft.IdentityModel.Protocol.Extensions**
- **Microsoft.Owin**
- **Microsoft.Owin.Host.SystemWeb**
- **Microsoft.Owin.Security**
- **Microsoft.Owin.Security.Cookies**
- **Microsoft.Owin.Security.OpenIdConnect**
- **Owin**
- **System.IdentityModel.Tokens.Jwt**

### <a name="net-references"></a>Reference za .NET

- **Microsoft.IdentityModel.Protocol.Extensions**
- **Microsoft.Owin**
- **Microsoft.Owin.Host.SystemWeb**
- **Microsoft.Owin.Security**
- **Microsoft.Owin.Security.Cookies**
- **Microsoft.Owin.Security.OpenIdConnect**
- **Owin**
- **System.IdentityModel**
- **System.IdentityModel.Tokens.Jwt**
- **System.Runtime.Serialization**

## <a name="code-has-been-added"></a>Dodana je kod

### <a name="code-files-were-added-to-your-project"></a>Kod datoteke koje su dodane u projekt

Provjera autentičnosti početnu klasu, **App_Start/Startup.Auth.cs** dodan u projekt koji sadrži pokretanje logike za provjeru autentičnosti Azure AD. Osim toga, kontroler klase Controllers/AccountController.cs dodan koja sadrži **SignIn()** i **SignOut()** metode. Na kraju, dio prikaza, **Views/Shared/_LoginPartial.cshtml** dodan koja sadrži vezu na akcija za prijava/SignOut.

### <a name="startup-code-was-added-to-your-project"></a>Pokretanje sustava dodan u projekt

Ako već imate početnu klasu u projektu, način **konfiguracije** ažurirana je da biste uključili poziv **ConfigureAuth(app)**. U suprotnom početnu klasu dodan u projekt.

### <a name="your-appconfig-or-webconfig-has-new-configuration-values"></a>App.config ili web.config sadrži nove vrijednosti za konfiguraciju

Dodane su sljedeće unose konfiguracije.


    <appSettings>
        <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
        <add key="ida:AADInstance" value="https://login.microsoftonline.com/" />
        <add key="ida:Domain" value="The selected Azure AD Domain" />
        <add key="ida:TenantId" value="The Id of your selected Azure AD Tenant" />
        <add key="ida:PostLogoutRedirectUri" value="Your project start page" />
    </appSettings>

### <a name="an-azure-active-directory-ad-app-was-created"></a>Stvorena je aplikaciju Azure Active Directory (AD)
Aplikaciju za Azure AD stvorena u direktoriju koji ste odabrali u čarobnjaku.

##<a name="if-i-checked-disable-individual-user-accounts-authentication-what-additional-changes-were-made-to-my-project"></a>Ako je li potvrđen okvir *onemogućite provjeru autentičnosti pojedinačne korisničke račune*, koje dodatne su promjene izvršene u projektu?
Uklonjeni su NuGet paket reference, a datoteka su uklonjene i sigurnosnu kopiju. Ovisno o stanju projekta, možda ćete morati ručno dodatne reference i datoteke uklonite ili izmjena šifru po potrebi.

### <a name="nuget-package-references-removed-for-those-present"></a>NuGet paket reference ukloniti (za one izlaganje)

- **Microsoft.AspNet.Identity.Core**
- **Microsoft.AspNet.Identity.EntityFramework**
- **Microsoft.AspNet.Identity.Owin**

### <a name="code-files-backed-up-and-removed-for-those-present"></a>Kod datoteke sigurnosne kopije i ukloniti (za one izlaganje)

Svaki od ovih datoteka je sigurnosnu kopiju i ukloniti iz projekta. Datoteke sigurnosne kopije nalaze se u mapi "Sigurnosno kopiranje" u korijenu direktorija u projekt.

- **App_Start\IdentityConfig.CS**
- **Controllers\ManageController.CS**
- **Models\IdentityModels.CS**
- **Models\ManageViewModels.CS**

### <a name="code-files-backed-up-for-those-present"></a>Kod datoteke sigurnosne kopije (za one izlaganje)

Svaki od ovih datoteka je sigurnosnu kopiju prije nego je zamijenjena. Datoteke sigurnosne kopije nalaze se u mapi "Sigurnosno kopiranje" u korijenu direktorija u projekt.

- **Startup.CS**
- **App_Start\Startup.AUTH.CS**
- **Controllers\AccountController.CS**
- **Views\Shared\_LoginPartial.cshtml**

## <a name="if-i-checked-read-directory-data-what-additional-changes-were-made-to-my-project"></a>Ako provjere *čitanje direktorija podataka*koje dodatne su promjene izvršene u projektu?

Dodane su dodatne reference.

###<a name="additional-nuget-package-references"></a>Dodatne reference NuGet paketa

- **EntityFramework**
- **Microsoft.Azure.ActiveDirectory.GraphClient**
- **Microsoft.Data.Edm**
- **Microsoft.Data.OData**
- **Microsoft.Data.Services.Client**
- **Microsoft.IdentityModel.Clients.ActiveDirectory**
- **System.Spatial**

###<a name="additional-net-references"></a>Dodatne reference .NET

- **EntityFramework**
- **EntityFramework.SqlServer**
- **Microsoft.Azure.ActiveDirectory.GraphClient**
- **Microsoft.Data.Edm**
- **Microsoft.Data.OData**
- **Microsoft.Data.Services.Client**
- **Microsoft.IdentityModel.Clients.ActiveDirectory**
- **Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms**
- **System.Spatial**

###<a name="additional-code-files-were-added-to-your-project"></a>Dodatne kod datoteke koje su dodane u projekt

Dvije datoteke dodani podržava tokena predmemoriranje: **Models\ADALTokenCache.cs** i **Models\ApplicationDbContext.cs**.  Dodatni kontroler i prikaz dodani ilustracija pristupanju podatke korisničkog profila pomoću Azure grafikonu API-ji.  Te su datoteke **Controllers\UserProfileController.cs** i **Views\UserProfile\Index.cshtml**.

###<a name="additional-startup-code-was-added-to-your-project"></a>Dodatni kod prilikom pokretanja dodan u projekt

U datoteci **startup.auth.cs** za **obavijesti** član **OpenIdConnectAuthenticationOptions**dodan je novi objekt **OpenIdConnectAuthenticationNotifications** .  To je da biste omogućili primanje kod OAuth i exchange za token za pristup.

###<a name="additional-changes-were-made-to-your-appconfig-or-webconfig"></a>Dodatne su promjene na app.config ili web.config

Dodane su sljedeće dodatne konfiguracijske stavke.

    <appSettings>
        <add key="ida:ClientSecret" value="Your Azure AD App's new client secret" />
    </appSettings>

Dodane sljedećim odjeljcima konfiguraciju i niz za povezivanje.

    <configSections>
        <!-- For more information on Entity Framework configuration, visit http://go.microsoft.com/fwlink/?LinkID=237468 -->
        <section name="entityFramework" type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=6.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" requirePermission="false" />
    </configSections>
    <connectionStrings>
        <add name="DefaultConnection" connectionString="Data Source=(localdb)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\aspnet-[AppName + Generated Id].mdf;Initial Catalog=aspnet-[AppName + Generated Id];Integrated Security=True" providerName="System.Data.SqlClient" />
    </connectionStrings>
    <entityFramework>
        <defaultConnectionFactory type="System.Data.Entity.Infrastructure.LocalDbConnectionFactory, EntityFramework">
          <parameters>
            <parameter value="mssqllocaldb" />
          </parameters>
        </defaultConnectionFactory>
        <providers>
          <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
        </providers>
    </entityFramework>


###<a name="your-azure-active-directory-app-was-updated"></a>Ažurirana Azure Active Directory aplikacije
Azure Active Directory aplikacije ažurirana da biste uključili dozvola za *čitanje direktorija podataka* i dodatni ključ koji je stvoren koji zatim korišten kao *ida: ClientSecret* u datoteci **web.config** .

[Dodatne informacije o servisu Azure Active Directory](https://azure.microsoft.com/services/active-directory/)
