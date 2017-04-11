<properties
   pageTitle="Azure Active Directory 2.0 provjera autentičnosti biblioteke | Microsoft Azure"
   description="Biblioteka kompatibilne klijenta poslužitelja proizvod biblioteke, i povezane biblioteke, izvora i primjere veza za krajnju točku 2.0 Azure Active Directory."
   services="active-directory"
   documentationCenter=""
   authors="skwan"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/30/2016"
   ms.author="skwan;bryanla"/>


# <a name="azure-active-directory-v20-authentication-libraries"></a>Azure Active Directory 2.0 provjera autentičnosti biblioteke
Krajnja točka za Azure Active Directory (Azure AD) 2.0 podržava standardni protokoli OAuth 2.0 i povezivanje 1.0 OpenID. Možete koristiti različite biblioteke tvrtke Microsoft i drugim tvrtkama ili ustanovama s krajnju točku 2.0.

Prilikom stvaranja aplikacije koja koristi krajnju točku 2.0, preporučujemo da koristite biblioteke koje se pišu stručnjaci domene protokol koji prate methodology životni ciklus razvoja na sigurnost (SDL), kao što je [onaj slijedi Microsoft][Microsoft-SDL]. Ako odlučite koda ruke podrška za protokoli, preporučujemo da slijedite SDL methodology i obratite pozornost Zatvori da biste sigurnosna pitanja vezana uz u specifikacije standarda za svaki protokol.

## <a name="types-of-libraries"></a>Vrste biblioteka

Azure AD 2.0 funkcionira s dvije vrste biblioteka:

- **Biblioteka klijenta**. Izvorni klijenata i poslužitelja pomoću klijenta biblioteke možete dobiti pristup tokeni za pozivanje resursa, kao što je Microsoft Graph.
- **Biblioteka proizvod poslužitelja**. Web-aplikacije pomoću poslužitelja proizvod biblioteke za korisničku prijavu. API-ji web pomoću poslužitelja proizvod biblioteke da biste provjerili valjanost tokena koje se šalju tako da klijenti za izvorni ili poslužitelji.

## <a name="library-support"></a>Podrška za biblioteku
Jer bilo koje usklađene sa standardima biblioteke možete odabrati kada koristite krajnju točku 2.0, važno je znati gdje za podršku. Problemi i zahtjeve za značajke u biblioteci kodu, obratite se vlasniku biblioteke. Problemi i zahtjeve za značajke u implementaciji protokol davatelj usluge zatražite od Microsoftova.

Biblioteka dolaze u dvije kategorije podrške:

- **Microsoft podržane**. Microsoft pruža rješenja za ove biblioteke, a zatim je proveo SDL krajnji diligence na te biblioteke.
- **Kompatibilne**. Microsoft je testirati ove biblioteke u osnovna scenarija i potvrđuje rade s krajnju točku 2.0. Microsoft ne nudi ispravci te biblioteke i proveo pregleda te biblioteke. Problemi i zahtjeve za značajke treba usmjeriti na projektu Otvori izvor u biblioteku.

Popis biblioteka koje funkcioniraju s krajnju točku 2.0, potražite u članku sljedeće odjeljke u ovom članku.

## <a name="microsoft-supported-client-libraries"></a>Biblioteka Microsoft podržane klijenta
| Platforme| Naziv biblioteke| Preuzimanje | Izvorni kod | Uzorak |
| :-: | :-: | :-: | :-: | :-: |
| Xamarin .NET, iz Windows trgovine | Provjera autentičnosti pomoću Microsoftova biblioteka (MSAL) za .NET | [Microsoft.Identity.Client (NuGet)][ClientLib-NET-Lib] | [MSAL za .NET (GitHub)][ClientLib-NET-Repo] | [Uzorak nativni klijent za stolna računala za Windows][ClientLib-NET-Sample] |
| Node.js | Microsoft Azure Active Directory Passport.js dodatka | [Passport Azure-AD (npm)][ClientLib-Node-Lib] | [Passport Azure-AD (GitHub)][ClientLib-Node-Repo] | dolazim brzo |

<!--- COMMENTING OUT UNTIL THEY ARE READY
| iOS, Mac | Microsoft Authentication Library (MSAL) for ObjC | In development | In development | In development |
| Android | Microsoft Authentication Library (MSAL) for Android | In development | In development | In development |
| JavaScript | Microsoft Authentication Library (MSAL) for JavaScript | In development | In development | In development |
 -->

## <a name="microsoft-supported-server-middleware-libraries"></a>Biblioteka proizvod Microsoft podržane poslužitelja
| Platforme| Naziv biblioteke| Preuzimanje | Izvorni kod | Uzorak |
| :-: | :-: | :-: | :-: | :-: |
| .NET 4.x | OWIN OpenID povezivanje proizvod platforme ASP.NET | [Microsoft.Owin.Security.OpenIdConnect (NuGet)][ServerLib-Net4-Owin-Oidc-Lib] | [Katana projekta (CodePlex)][ServerLib-Net4-Owin-Oidc-Repo] | [Uzorak web app][ServerLib-Net4-Owin-Oidc-Sample] |
| .NET 4.x | Proizvod OAuth nošenja OWIN za platforme ASP.NET | [Microsoft.Owin.Security.OAuth (NuGet)][ServerLib-Net4-Owin-Oauth-Lib] | [Katana projekta (CodePlex)][ServerLib-Net4-Owin-Oauth-Repo] | [Ogledna API na webu][ServerLib-Net4-Owin-Oauth-Sample] |
| Temeljni .NET | OWIN OpenID povezivanje proizvod za .NET Core | [Microsoft.AspNetCore.Authentication.OpenIdConnect (NuGet)][ServerLib-NetCore-Owin-Oidc-Lib] | [Sigurnost ASP.NET (GitHub)][ServerLib-NetCore-Owin-Oidc-Repo] | [Uzorak web app][ServerLib-NetCore-Owin-Oidc-Sample] |
| Temeljni .NET | Proizvod OAuth nošenja OWIN za .NET Core | [Microsoft.AspNetCore.Authentication.OAuth (NuGet)][ServerLib-NetCore-Owin-Oauth-Lib] | [Sigurnost ASP.NET (GitHub)][ServerLib-NetCore-Owin-Oauth-Repo] | dolazim brzo |
| Node.js | Microsoft Azure Active Directory Passport.js dodatka | [Passport Azure-AD (npm)][ServerLib-Node-Lib] | [Passport Azure-AD (GitHub)][ServerLib-Node-Repo] | [Uzorak web app][ServerLib-Node-Sample] |
<!--- COMMENTING UNTIL SAMPLE IS AVAILABLE
| .NET 4.x, .NET Core | JSON Web Token Handler for .NET | [System.IdentityModel.Tokens.Jwt (NuGet)][ServerLib-Net-Jwt-Lib] | [Azure AD identity model extensions for .NET (GitHub)][ServerLib-Net-Jwt-Repo] | Coming soon |
--->
## <a name="compatible-client-libraries"></a>Biblioteka kompatibilne klijenta
| Platforme| Naziv biblioteke | Testirano verzija | Izvorni kod | Uzorak |
| :-: | :-: | :-: | :-: | :-: |
| Android | [OIDCAndroidLib](https://github.com/kalemontes/OIDCAndroidLib/wiki) | 0.2.1 | [OIDCAndroidLib](https://github.com/kalemontes/OIDCAndroidLib) | [Ogledna nativni aplikacije](active-directory-v2-devquickstarts-android.md) |
| iOS | [NXOAuth2Client](https://github.com/nxtbgthng/OAuth2Client) | 1.2.8 | [NXOAuth2Client](https://github.com/nxtbgthng/OAuth2Client) | [Ogledna nativni aplikacije](active-directory-v2-devquickstarts-ios.md)|
| JavaScript | [Hello.js](https://adodson.com/hello.js/) | 1.13.5 | [Hello.js](https://github.com/MrSwitch/hello.js) | dolazim brzo |
| Python Flask | [Flask OAuthlib](https://github.com/lepture/flask-oauthlib) | 0.9.3 | [Flask OAuthlib](https://github.com/lepture/flask-oauthlib) | dolazim brzo |
| Ruby | [OmniAuth](https://github.com/omniauth/omniauth/wiki) | omniauth:1.3.1</br>omniauth-oauth2:1.4.0 | [OmniAuth](https://github.com/omniauth/omniauth)</br>[OmniAuth OAuth2](https://github.com/intridea/omniauth-oauth2) | dolazim brzo |
<!--- REMOVING BRANDON'S FOR NOW
|  |  |  |  |  |
| Android | [OAuth2 Client](https://github.com/wuman/android-oauth-client) |   | [OAuth2 Client](https://github.com/wuman/android-oauth-client)  | Coming soon  |
| Java | [WSO2 Identity Server](https://docs.wso2.com/display/IS500/Introducing+the+Identity+Server) | [Version 5.2.0](http://wso2.com/products/identity-server/) | [Source](https://docs.wso2.com/display/IS500/Building+from+Source) | [Samples index](https://docs.wso2.com/display/IS500/Samples)  |
| Java | [Java Gluu Server](https://gluu.org/docs/) |   | [oxAuth](https://github.com/GluuFederation/oxAuth)  | Coming soon |
| Node.js | [NPM passport-openidconnect](https://www.npmjs.com/package/passport-openidconnect) | 0.0.1  | [Passport-OpenID Connect](https://github.com/jaredhanson/passport-openidconnect) | Coming soon  |
| PHP | [OpenID Connect Basic Client](https://github.com/jumbojett/OpenID-Connect-PHP) |   | [OpenID Connect Basic Client](https://github.com/jumbojett/OpenID-Connect-PHP)  | Coming soon  |
-->

## <a name="compatible-server-middleware-libraries"></a>Biblioteka proizvod kompatibilan s poslužitelja
dolazim brzo

## <a name="related-content"></a>Srodni sadržaji
Dodatne informacije o krajnjoj 2.0 Azure AD potražite u članku [Pregled 2.0 za Azure AD aplikacije model][AAD-App-Model-V2-Overview].

Za Pridonesite sužavanje i oblika sadržaj, koristite značajku komentara Disqus pri kraju ovog članka Slanje povratnih informacija.

<!--Image references-->

<!--Reference style links -->
[AAD-App-Model-V2-Overview]: active-directory-appmodel-v2-overview.md
[ClientLib-NET-Lib]: http://www.nuget.org/packages/Microsoft.Identity.Client
[ClientLib-NET-Repo]: https://github.com/AzureAD/microsoft-authentication-library-for-dotnet
[ClientLib-NET-Sample]: active-directory-v2-devquickstarts-wpf.md
[ClientLib-Node-Lib]: https://www.npmjs.com/package/passport-azure-ad
[ClientLib-Node-Repo]: https://github.com/AzureAD/passport-azure-ad
[ClientLib-Node-Sample]:
[ClientLib-Iosmac-Lib]:
[ClientLib-Iosmac-Repo]:
[ClientLib-Iosmac-Sample]:
[ClientLib-Android-Lib]:
[ClientLib-Android-Repo]:
[ClientLib-Android-Sample]:
[ClientLib-Js-Lib]:
[ClientLib-Js-Repo]:
[ClientLib-Js-Sample]:
[Microsoft-SDL]: http://www.microsoft.com/sdl/default.aspx
[ServerLib-Net4-Owin-Oidc-Lib]: https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/
[ServerLib-Net4-Owin-Oidc-Repo]: http://katanaproject.codeplex.com/
[ServerLib-Net4-Owin-Oidc-Sample]: active-directory-v2-devquickstarts-dotnet-web.md
[ServerLib-Net4-Owin-Oauth-Lib]: https://www.nuget.org/packages/Microsoft.Owin.Security.OAuth/
[ServerLib-Net4-Owin-Oauth-Repo]: http://katanaproject.codeplex.com/
[ServerLib-Net4-Owin-Oauth-Sample]: https://azure.microsoft.com/en-us/documentation/articles/active-directory-v2-devquickstarts-dotnet-api/
[ServerLib-Net-Jwt-Lib]: https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt
[ServerLib-Net-Jwt-Repo]: https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet
[ServerLib-Net-Jwt-Sample]:/
[ServerLib-NetCore-Owin-Oidc-Lib]: https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.OpenIdConnect/
[ServerLib-NetCore-Owin-Oidc-Repo]: https://github.com/aspnet/Security
[ServerLib-NetCore-Owin-Oidc-Sample]: https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore-v2
[ServerLib-NetCore-Owin-Oauth-Lib]: https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.OAuth/
[ServerLib-NetCore-Owin-Oauth-Repo]: https://github.com/aspnet/Security
[ServerLib-NetCore-Owin-Oauth-Sample]:/
[ServerLib-Node-Lib]: https://www.npmjs.com/package/passport-azure-ad
[ServerLib-Node-Repo]: https://github.com/AzureAD/passport-azure-ad/
[ServerLib-Node-Sample]: https://azure.microsoft.com/en-us/documentation/articles/active-directory-v2-devquickstarts-node-web/
