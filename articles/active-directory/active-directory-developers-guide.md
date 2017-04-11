<properties
   pageTitle="Vodič za Azure Active Directory programiranje | Microsoft Azure"
   description="Ovaj članak sadrži potpun vodič kroz resurse vezanima uz za razvojne inženjere za Azure Active Directory."
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/24/2016"
   ms.author="mbaldwin"/>


# <a name="azure-active-directory-developers-guide"></a>Vodič za Azure Active Directory programiranje

## <a name="overview"></a>Pregled
Kao upravljanje identitetom kao platforma servisa (IDMaaS), Azure Active Directory (AD) omogućuje razvojnim inženjerima na učinkovit način možete integrirati upravljanje identitetom u svojim aplikacijama. U sljedećim člancima pružaju pregled o implementaciji i ključne značajke programa Azure AD. Predlažemo da ih pročitati redoslijedom ili prelazak na [Prvi koraci u](#getting-started) ako ste spremni istražujte.


1. [Prednosti integracije Azure AD](./develop/active-directory-how-to-integrate.md): otkriti zašto Integracija s Azure AD nudi najbolje rješenje za sigurna prijava i autorizacije.

1. [Scenariji za provjeru autentičnosti Azure AD](active-directory-authentication-scenarios.md): prednosti pojednostavljeni provjere autentičnosti u Azure AD za prijavu u aplikaciju.

1. [Integriranje aplikacije s Azure AD](active-directory-integrating-applications.md): upute za dodavanje, ažuriranje i uklanjanje aplikacija iz Azure AD i o izgleda integrirane aplikacije.

1. [Azure AD grafikonu API](active-directory-graph-api.md): korištenje API Azure AD grafikonu programatski pristup Azure AD putem krajnje točke REST API-JA. Putem [Programa Microsoft Graph](https://graph.microsoft.io/)dostupan je i API Azure AD grafikonu. Microsoft Graph pruža objedinjenih API koja omogućuje pristup većem broju Microsoftovim servisom u oblaku API-ji, kroz jedan krajnjoj točki REST API-JA i s jednom pristup token.

1. [Azure AD provjera autentičnosti biblioteke](active-directory-authentication-libraries.md): jednostavno provjeru autentičnosti korisnika da biste dobili pristup tokeni pomoću Azure AD provjera autentičnosti biblioteke za .NET, JavaScript, cilj C, Android i drugo.


## <a name="getting-started"></a>Početak rada

Ovim praktičnim vodičima prilagođeni su za više platforme i omogućuju brzo pokretanje razvoj s Azure Active Directory. Kao preduvjeta, morate [dobiti klijent za Azure Active Directory](active-directory-howto-tenant.md).

### <a name="mobile-and-pc-application-quick-start-guides"></a>Mobile i PC vodiči za brzi početak rada aplikacije

|[![iOS](./media/active-directory-developers-guide/ios.png)](active-directory-devquickstarts-ios.md)|[![Android](./media/active-directory-developers-guide/android.png)](active-directory-devquickstarts-android.md)|[![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-dotnet.md)|[![Univerzalni za Windows](./media/active-directory-developers-guide/windows.png)](active-directory-devquickstarts-windowsstore.md)|[![Xamarin](./media/active-directory-developers-guide/xamarin.png)](active-directory-devquickstarts-xamarin.md)|[![Cordova](./media/active-directory-developers-guide/cordova.png)](active-directory-devquickstarts-cordova.md)|[![OAuth 2.0](./media/active-directory-developers-guide/oauth-2.png)](active-directory-protocols-oauth-code.md)
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
|[iOS](active-directory-devquickstarts-ios.md)|[Android](active-directory-devquickstarts-android.md)|[.NET](active-directory-devquickstarts-dotnet.md)|[Univerzalni za Windows](active-directory-devquickstarts-windowsstore.md)|[Xamarin](active-directory-devquickstarts-xamarin.md)|[Cordova](active-directory-devquickstarts-cordova.md)|[Integriranje izravno s OAuth 2.0](active-directory-protocols-oauth-code.md)|

### <a name="web-application-quick-start-guides"></a>Vodiči za Brzi start web aplikacije

|[![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-webapp-dotnet.md)|[![Java](./media/active-directory-developers-guide/java.png)](active-directory-devquickstarts-webapp-java.md)|[![AngularJS](./media/active-directory-developers-guide/angularjs.png)](active-directory-devquickstarts-angular.md)|[![JavaScript](./media/active-directory-developers-guide/javascript.png)](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi)|[![Node.js](./media/active-directory-developers-guide/nodejs.png)](active-directory-devquickstarts-openidconnect-nodejs.md) | [![Povezivanje OpenID](./media/active-directory-developers-guide/openid-connect.png)](active-directory-protocols-openid-connect-code.md)
|:--:|:--:|:--:|:--:|:--:|:--:|
|[.NET](active-directory-devquickstarts-webapp-dotnet.md)|[Java](active-directory-devquickstarts-webapp-java.md)|[AngularJS](active-directory-devquickstarts-angular.md)|[JavaScript](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi)|[Node.js](active-directory-devquickstarts-openidconnect-nodejs.md)|[Integriranje izravno s OpenID povezivanje](active-directory-protocols-openid-connect-code.md)|

### <a name="web-api-quick-start-guides"></a>Vodiči za brzi početak rada za API na webu

|[![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-webapi-dotnet.md)|[![Node.js](./media/active-directory-developers-guide/nodejs.png)](active-directory-devquickstarts-webapi-nodejs.md)
|:--:|:--:|
|[.NET](active-directory-devquickstarts-webapi-dotnet.md)|[Node.js](active-directory-devquickstarts-webapi-nodejs.md)

### <a name="querying-the-directory-quickstart-guide"></a>Slanje upita direktorija vodič za brzi početak rada

| [![.NET](./media/active-directory-developers-guide/graph.png)](active-directory-graph-api-quickstart.md)|
|:--:|
|[Graph API-JA](active-directory-graph-api-quickstart.md)|

## <a name="how-tos"></a>How-tos

Ti članci opisuju kako izvoditi određene zadatke pomoću servisa Azure Active Directory:

- [Početak klijent za Azure AD](active-directory-howto-tenant.md)
- [Prijavite se u bilo koji korisnik Azure AD pomoću uzorak više klijentske aplikacije](active-directory-devhowto-multi-tenant-overview.md)
- Omogućivanje jedinstvene Prijave unakrsno aplikacije pomoću ADAL, [Android](active-directory-sso-android.md) i uređaje [sa sustavom iOS](active-directory-sso-ios.md)
- [Provjerite aplikacije AppSource Certificirano za Azure AD](active-directory-devhowto-appsource-certified.md)
- [Popis aplikacija u galeriji aplikacija Azure AD](active-directory-app-gallery-listing.md)
- [Slanje web-aplikacije za Office 365 Prodavač nadzorne ploče](https://msdn.microsoft.com/office/office365/howto/submit-web-apps-seller-dashboard)
- [Razumijevanje programski manifest Azure Active Directory](active-directory-application-manifest.md)
- [Razumijevanje izgleda za nabavu gumbe prijave i aplikacije u klijentskoj aplikaciji](active-directory-branding-guidelines.md)
- [Pregled: Kako izraditi aplikacije koje Prijava korisnika pomoću računa i osobne i tvrtke ili obrazovne ustanove](active-directory-appmodel-v2-overview.md)
- [Pregled: Kako izraditi aplikacije koje se prijavite i prijavite se u koje korisnici](../active-directory-b2c/active-directory-b2c-overview.md)
- [Pretpregled: Konfiguriranje tokena vijekom trajanja u Azure AD](active-directory-configurable-token-lifetimes.md) pomoću komponente PowerShell. Potražite u članku [pravila operacije](https://msdn.microsoft.com/library/azure/ad/graph/api/policy-operations) i [pravila entitet](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#policy-entity) detalje o konfiguriranju putem API Azure AD grafikonu.

## <a name="reference"></a>Referenca

Ti članci upućivanje foundation za OSTALE i provjera autentičnosti biblioteke imenika API-ji, protokola, pogreške, primjere koda i krajnje točke.  

###  <a name="support"></a>Podrška
- [Označeni pitanja](http://stackoverflow.com/questions/tagged/azure-active-directory): pronalaženje Azure Active Directory rješenja u stogu prelijevanje traženje oznaka [azure active directory](http://stackoverflow.com/questions/tagged/azure-active-directory) i [adal](http://stackoverflow.com/questions/tagged/adal).
- [Pojmovnik za razvojne inženjere za Azure AD](active-directory-dev-glossary.md) za definicije neke od najčešće korištenih uvjete vezane uz razvoj aplikacija i integracija potražite u članku.

### <a name="code"></a>Kod

- [Biblioteka Otvori izvor Azure Active Directory](http://github.com/AzureAD): da biste pronašli izvor u biblioteku najjednostavnije pomoću naš [popis biblioteke](active-directory-authentication-libraries.md).

- [Uzorci Azure Active Directory](https://github.com/azure-samples?query=active-directory): da biste prešli na popisu uzoraka najjednostavnije pomoću [indeksa primjere koda](active-directory-code-samples.md).

- [Active Directory provjera autentičnosti biblioteke (ADAL) za .NET](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet) - referentnu dokumentaciju dostupna je za [najnovije glavne verzije](https://docs.microsoft.com/active-directory/adal/microsoft.identitymodel.clients.activedirectory) i [prethodni glavnu verziju](https://docs.microsoft.com/active-directory/adal/v2/microsoft.identitymodel.clients.activedirectory).

### <a name="graph-api"></a>Graph API-JA

- [Graph API reference](https://msdn.microsoft.com/library/azure/hh974476.aspx): OSTALE reference za Azure grafikonu API za Active Directory. [Prikaz interaktivne grafikonu API sučelje za referencu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

- [Opsega dozvola grafikonu API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes): opsega dozvola OAuth 2.0 koji se koriste za kontrolu pristupa s aplikaciju za direktorija podataka na klijentu.

### <a name="authentication-and-authorization-protocols"></a>Provjera autentičnosti i autorizacije protokola

- [Potpisivanje ključa prilikom prelaska u Azure AD](active-directory-signing-key-rollover.md): informacije o prijavi Azure AD ključa prilikom prelaska cadence i kako ažurirati ključ za Najčešći scenariji aplikacije.

- [OAuth 2.0 protokol: pomoću koda grant autorizacije](active-directory-protocols-oauth-code.md): Dodjela kod za autorizaciju protokol OAuth 2.0 možete koristiti da biste autorizirali pristup web-aplikacije i smjernice za Web API servisa Azure Active Directory.

- [OAuth 2.0 protokol: objašnjenje implicitno grant](active-directory-dev-understanding-oauth2-implicit-grant.md): dodatne informacije o grant implicitno autorizacije i je li to najbolje odgovara aplikacija.

- [OAuth 2.0 protokol: servis za servis za pozive pomoću klijenta vjerodajnice](active-directory-protocols-oauth-service-to-service.md): Dodjela u vjerodajnice OAuth 2.0 klijent dopušta web-servisa (povjerljive klijent) da biste koristili vlastiti vjerodajnice za provjeru autentičnosti prilikom pozivanja drugih servisa za web, umjesto oponaša korisnika. U ovom scenariju klijent je obično srednjem sloju web-servisa, daemon servisa ili web-mjesta.

- [Protokol za povezivanje 1.0 OpenID: prijava i provjera autentičnosti](active-directory-protocols-openid-connect-code.md): protokol za povezivanje 1.0 u OpenID proširuje OAuth 2.0 za korištenje kao protokol za provjeru autentičnosti. Klijentska aplikacija možete primati u id_token za upravljanje procesom za prijavu ili proširiti tijek kod autorizacije primati poruku id_token i autorizacije kod.

- [Referenca protokol SAML 2.0](active-directory-saml-protocol-reference.md): U SAML 2.0 protokol omogućuje aplikacije možete unijeti na jednom za prijave svojim korisnicima.

- [Protokol WS Federacija 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html): Azure Active Directory podržava WS Federacija 1.2 po Web Services vanjski pristup verzija 1.2 specifikacija. Dodatne informacije o dokumentu metapodataka za vanjski pristup, pročitajte članak [Metapodataka za vanjski pristup](active-directory-federation-metadata.md).

- [Podržane vrste tokena i zahtjeva](active-directory-token-and-claims.md): koristite ovaj vodič za razumijevanje i procjenu zahtjevima u tokeni SAML 2.0 i tokeni JSON Web (JWT).

## <a name="videos"></a>Videozapisi

### <a name="build"></a>Sastavljanje

Ove pregled prezentacije na razvoj aplikacija pomoću servisa Azure Active Directory značajka zvučnika koji rade izravno u tim. Prezentacija pokrivaju temeljne teme, uključujući IDMaaS, provjere autentičnosti, vanjski pristup identitetu i jedinstvenu prijavu.

- [Microsoft identiteta: Stanje unije i buduće smjeru](https://azure.microsoft.com/documentation/videos/build-2016-microsoft-identity-state-of-the-union-and-future-direction/)
- [Azure Active Directory: Upravljanje identitetom kao servis za Moderna aplikacije](https://azure.microsoft.com/documentation/videos/build-2015-azure-active-directory-identity-management-as-a-service-for-modern-applications/)
- [Razvoj Moderna web-aplikacije pomoću servisa Azure Active Directory](https://azure.microsoft.com/documentation/videos/build-2015-develop-modern-web-applications-with-azure-active-directory/)
- [Razvoj Moderna izvornim aplikacijama Azure Active Directory](https://azure.microsoft.com/documentation/videos/build-2015-develop-modern-native-applications-with-azure-active-directory/)

### <a name="azure-friday"></a>Azure petak
[Azure petak](https://azure.microsoft.com/documentation/videos/azure-friday/) je ponavljajućeg petak 1:1 serija videozapisa s posvećenu da vam kratki (10 – 15 minuta) intervjua s stručnjaka na raznim Azure teme.  Koristite značajku filtriranja usluge na stranici da biste vidjeli videozapise Azure Active Directory.

- [Azure identiteta 101](https://azure.microsoft.com/documentation/videos/azure-identity-basics/)
- [Azure identiteta 102](https://azure.microsoft.com/documentation/videos/azure-identity-creating-active-directory/)
- [Azure identiteta 103](https://azure.microsoft.com/documentation/videos/azure-identity-application-to-authenticate/)

## <a name="social"></a>Društvene

- [Blog tima za Active Directory](http://blogs.technet.com/b/ad/): najnovije razvoja u svijetu Azure Active Directory.

- [Azure Active Directory grafikonu timskog bloga](http://blogs.msdn.com/b/aadgraphteam): Azure Active Directory informacije koje se odnose na grafikonu API-JA.

- [Oblak identiteta](http://www.cloudidentity.net): vođenje bilježaka na upravljanje identitetom kao servis iz na glavni Azure Active Directory Poslijepodne.  

- [Azure Active Directory na Twitteru](https://twitter.com/azuread): najave Azure Active Directory u 140 znakova ili manje.

## <a name="windows-server-on-premises-development"></a>Windows Server lokalnog razvoj
Upute o korištenju Windows Server i Active Directory Federation Services (ADFS) razvoj potražite u članku:

- [AD FS scenariji za razvojne inženjere](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/ad-fs-scenarios-for-developers): nudi pregled komponente AD FS i kako funkcionira, s pojedinostima na scenarije podržani provjere autentičnosti/autorizacije.
- [Vodiči za AD FS](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/ad-fs-development): popis članaka walk-through koja pružaju detaljne upute o implementacijom tokova povezane provjere autentičnosti/autorizacije.
