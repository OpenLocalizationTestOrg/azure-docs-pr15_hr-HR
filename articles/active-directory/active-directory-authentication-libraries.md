<properties
   pageTitle="Biblioteka za provjeru autentičnosti Azure Active Directory | Microsoft Azure"
   description="Na Azure AD provjera autentičnosti biblioteke (ADAL) omogućuje klijentskog računala inženjerima omogućuje jednostavno provjeru autentičnosti korisnika na cloud ili lokalni Active Directory (AD) i dobiti pristup tokeni za osiguravanje API poziva."
   services="active-directory"
   documentationCenter=""
   authors="bryanla"
   manager="mbaldwin"
   editor="mbaldwin" />
<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/11/2016"
   ms.author="mbaldwin" />

# <a name="azure-active-directory-authentication-libraries"></a>Biblioteka za provjeru autentičnosti Azure Active Directory

Provjera autentičnosti biblioteke (ADAL) za Azure AD omogućuje klijentskog računala inženjerima omogućuje jednostavno provjeru autentičnosti korisnika na cloud ili lokalni Active Directory (AD) i dobiti pristup tokeni za osiguravanje API poziva. ADAL ima mnoge značajke autentičnosti provjerite jednostavnije za razvojne inženjere, kao što su asinkronog podršku, konfigurirati tokena predmemorije koja pohranjuje tokeni access i osvježavanje tokena, automatsko osvježavanje tokena kada istekne token za pristup i osvježavanje token je dostupna, i još mnogo toga. Obradom većinu složenosti ADAL mogu pomoći za razvojne inženjere fokus na poslovne logike u svoje aplikacije i jednostavno zaštita resursa pritom ne bude stručnjaka na sigurnost.

ADAL dostupna je na različite platforme.

|Platforme|Naziv paketa|Klijent poslužitelj|Preuzimanje|Izvorni kod|Dokumentacija i uzorka|
|---|---|---|---|---|---|
|.NET klijent, iz Windows trgovine UWP, Xamarin iOS i Android|Biblioteka za provjeru autentičnosti Active Directory (ADAL) za .NET v3 |Klijent|[Microsoft.IdentityModel.Clients.ActiveDirectory (NuGet)](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory)|[ADAL za .NET (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet)|[Dokumentacija](https://docs.microsoft.com/active-directory/adal/microsoft.identitymodel.clients.activedirectory)|
|.NET klijent, iz Windows trgovine Windows Phone 8.1 |Biblioteka za provjeru autentičnosti Active Directory (ADAL) za .NET v2 |Klijent|[Microsoft.IdentityModel.Clients.ActiveDirectory (NuGet)](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/2.28.2)|[ADAL za .NET (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/releases/tag/v2.28.2)|[Dokumentacija](https://docs.microsoft.com/active-directory/adal/v2/microsoft.identitymodel.clients.activedirectory)|
|JavaScript|Biblioteka za provjeru autentičnosti servisa Active Directory (ADAL) za JavaScript|Klijent|[ADAL za JavaScript (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-js)|[ADAL za JavaScript (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-js)|Primjer: [SinglePageApp DotNet (Github)](https://github.com/AzureADSamples/SinglePageApp-DotNet)|
|IOS OS X|Biblioteka za provjeru autentičnosti servisa Active Directory (ADAL) za cilj C|Klijent|[ADAL za cilj-C (CocoaPods)](http://cocoadocs.org/docsets/ADAL/)|[ADAL za cilj-C (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-objc)|Primjer: [NativeClient iOS (Github)](https://github.com/AzureADSamples/NativeClient-iOS)|
|Android|Biblioteka za provjeru autentičnosti servisa Active Directory (ADAL) za Android|Klijent|[ADAL za Android (središnje spremište)](http://search.maven.org/remotecontent?filepath=com/microsoft/aad/adal/)|[ADAL za Android (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-android)|Primjer: [NativeClient Android (Github)](https://github.com/AzureADSamples/NativeClient-Android)|
|Node.js|Biblioteka za provjeru autentičnosti Active Directory (ADAL) za Node.js|Klijent|[ADAL za Node.js (npm)](https://www.npmjs.com/package/adal-node)|[ADAL za Node.js (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-nodejs)|Primjer: [WebAPI Nodejs (Github)](https://github.com/AzureADSamples/WebAPI-Nodejs)|
|Node.js|Microsoft Azure Active Directory Passport proizvod provjere autentičnosti za čvor|Klijent|[Azure Active Directory Passport za Node.js (npm)](https://www.npmjs.com/package/passport-azure-ad)|[Azure Active Directory za Node.js (Github)](https://github.com/AzureAD/passport-azure-ad)||
|Java|Biblioteka za provjeru autentičnosti servisa Active Directory (ADAL) za Java|Klijent|[ADAL za Java (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-java)|[ADAL za Java (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-java)||
|.NET|Identitet protokol proširenja za Microsoft .NET Framework 4,5|Poslužitelj|[Microsoft.IdentityModel.Protocol.Extensions (NuGet)](https://www.nuget.org/packages/Microsoft.IdentityModel.Protocol.Extensions)|[Azure AD identiteta modela datotečne nastavke za .NET (Github)](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet)||
|.NET|Rukovatelj tokena JSON Web za Microsoft .net Framework 4,5|Poslužitelj|[System.IdentityModel.Tokens.Jwt (NuGet)](https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt)|[Azure AD identiteta modela datotečne nastavke za .NET (Github)](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet)||
|.NET|OWIN proizvod koji omogućuje programu korištenje tehnologije tvrtke Microsoft za provjeru autentičnosti.|Poslužitelj|[Microsoft.Owin.Security.ActiveDirectory (NuGet)](https://www.nuget.org/packages/Microsoft.Owin.Security.ActiveDirectory/)|[OWIN (CodePlex)](http://katanaproject.codeplex.com)||
|.NET|OWIN proizvod koji omogućuje programu korištenje OpenIDConnect za provjeru autentičnosti.|Poslužitelj|[Microsoft.Owin.Security.OpenIdConnect (NuGet)](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect)|[OWIN (CodePlex)](http://katanaproject.codeplex.com)|Primjer: [WebApp-OpenIDConnecty-DotNet (Github)](https://github.com/AzureADSamples/WebApp-OpenIDConnect-DotNet)|
|.NET|OWIN proizvod koji omogućuje programu korištenje WS Federacija za provjeru autentičnosti.|Poslužitelj|[Microsoft.Owin.Security.WsFederation (NuGet)](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation)|[OWIN (CodePlex)](http://katanaproject.codeplex.com)|Primjer: [WebApp-WSFederation-DotNet (Github)](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet)|

## <a name="scenarios"></a>Scenariji

Ovo su tri uobičajeni scenariji u kojima ADAL moguće je koristiti za provjeru autentičnosti.  

### <a name="authenticating-users-of-a-client-application-to-a-remote-resource"></a>Provjera autentičnosti korisnika klijentsku aplikaciju s udaljenog resursa

U ovom scenariju razvojni inženjer ima klijent, kao što su WPF aplikacije koje su potrebne za pristup udaljene resursu osigurava Azure AD, kao što je web-mjesto API-JA. On ima Azure pretplatu ne zna pozvati API do web, a zna Azure AD klijentu koji koristi API na webu. Kao rezultat ADAL kojima možete koristiti da biste olakšali provjera autentičnosti s Azure AD ili potpuno prenošenjem sučelje za provjeru autentičnosti da biste ADAL izričito obradom korisničke vjerodajnice. ADAL čini jednostavno provjere autentičnosti korisnika, dobiti access token i osvježavanje tokena iz Azure AD, a zatim koristite token za pristup da biste je zahtjeva za API na webu.

Uzorak kod koji pokazuje scenarij pomoću provjere autentičnosti za Azure AD potražite u članku [Matičnoj aplikaciji WPF klijent za API na webu](https://github.com/azureadsamples/nativeclient-dotnet).

### <a name="authenticating-a-server-application-to-a-remote-resource"></a>Provjera autentičnosti poslužitelja aplikacija na udaljenom resurs

U ovom scenariju razvojni inženjer sadrži aplikacije koje se izvode na poslužitelju, koje treba pristupiti daljinski resursa osigurava Azure AD, kao što je web-mjesto API-JA. On ima Azure pretplatu ne zna pozvati do servisa, a zna koristi klijentu za Azure AD web API. Zbog toga ADAL on možete koristiti da biste olakšali provjera autentičnosti s Azure AD izričito obradom vjerodajnice za aplikacije. ADAL čini je lako dohvatiti token iz Azure AD pomoću vjerodajnica za klijent aplikacije, a zatim pomoću tog token da biste zahtjeva za API na webu. ADAL bavi i upravljanje vijek token za pristup tako da ga predmemoriranje i obnavljanje prema potrebi. Uzorak kod koji pokazuje scenarij potražite u članku [Aplikacije konzole za API na webu](https://github.com/AzureADSamples/Daemon-DotNet).

### <a name="authenticating-a-server-application-on-behalf-of-a-user-to-access-a-remote-resource"></a>Provjera autentičnosti poslužitelja aplikacije ime korisnika za pristup udaljene resursu

U ovom scenariju razvojni inženjer sadrži aplikacije koje se izvode na poslužitelju, koje treba pristupiti daljinski resursa osigurava Azure AD, kao što je web-mjesto API-JA. Da biste unijeli ime korisnika u Azure AD i mora zahtjev. On ima Azure pretplatu ne zna pozvati do web API-JA, a zna Azure AD smjernice za korištenje servisa. Kada korisnik provjere autentičnosti u web-aplikaciju aplikaciju možete pristupiti i pripadnog koda autorizacije za korisnika iz Azure AD. Web-aplikaciju možete koristiti ADAL da biste dobili token za pristup i osvježavanje tokena ime korisnika pomoću autorizacije klijent i kod vjerodajnicama aplikacije Azure AD. Kada je web-aplikacija ima pristup tokena, nazovite možete web API dok token istekne. Kada istekne token web-aplikaciju možete koristiti ADAL da biste dobili novi token pristup pomoću osvježavanje tokena prethodno primljen.


## <a name="see-also"></a>Vidi također

[Vodič za razvojne inženjere Azure Active Directory](active-directory-developers-guide.md)

[Scenariji za provjeru autentičnosti za Azure Active directory](active-directory-authentication-scenarios.md)

[Primjere koda Azure Active Directory](active-directory-code-samples.md)
