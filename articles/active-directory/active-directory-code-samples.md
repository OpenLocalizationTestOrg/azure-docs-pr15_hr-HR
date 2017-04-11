<properties
   pageTitle="Primjere koda Azure Active Directory | Microsoft Azure"
   description="Indeks primjere koda Azure Active Directory, razvrstan po scenarij."
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="priyamohanram"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin"/>

# <a name="azure-active-directory-code-samples"></a>Primjere koda Azure Active Directory

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Da biste dodali provjere autentičnosti i autorizacije web-aplikacije i na webu API-je možete koristiti Microsoft Azure Active Directory (Azure AD). U ovom se odjeljku veze da biste primjere koda koji vam pokazuju kako to učiniti i koda koje možete koristiti u aplikacije. Na stranici ogledni kod možete pronaći detaljne čitanje-ja tema koje će vam pomoći u preduvjeti, instalacija i postavljanje međuverzije. I kod je komentar pomoću kojih se objašnjava ključnih sekcije.

Da biste shvatili osnovni scenarij za svaku vrstu uzorka, potražite u članku provjera autentičnosti scenariji za Azure AD.

Doprinos našeg primjera na GitHub: [Microsoft Azure Active Directory uzoraka i dokumentacije](https://github.com/Azure-Samples?page=3&query=active-directory).

## <a name="web-browser-to-web-application"></a>Web-preglednika u web-aplikaciju

Ove primjera pokazuju kako napisati web-aplikacije koja upućuje korisnikov preglednik da biste se prijavili ih Azure AD.



| Jezik/platforme | Uzorak | Opis
| ----------------- | ------ | -----------
| C# / .NET | [WebApp OpenIDConnect DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) | Za provjeru autentičnosti korisnika iz klijent za Azure AD pomoću OpenID povezivanje (ASP.Net OpenID povezivanje OWIN proizvod).
| C# / .NET | [WebApp-MultiTenant-OpenIdConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-multitenant-openidconnect) | Više klijentom .NET MVC web-aplikacije koje koristi OpenID povezivanje (ASP.Net OpenID povezivanje OWIN proizvod) za provjeru autentičnosti korisnika iz više klijenata za Azure AD.
| C# / .NET | [WebApp WSFederation DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-wsfederation) | Korištenje WS Federacija (ASP.Net WS Federacija OWIN proizvod) za provjeru autentičnosti korisnika iz klijent za Azure AD.






## <a name="single-page-application-spa"></a>Jedna stranica aplikacije (SPA)

Ovaj primjer pokazuje kako pisati jednu stranicu aplikacije zaštićen s Azure AD.  

| Jezik/platforme | Uzorak | Opis
| ----------------- | ------ | -----------
| JavaScript, C# / .NET | [SinglePageApp DotNet](https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp) | Pomoću ADAL JavaScript i Azure AD sigurne implementirana u u ASP.NET API na webu pozadinskih aplikacije utemeljene na AngularJS jedne stranice.


## <a name="native-application-to-web-api"></a>Izvorna aplikacija za API na webu

Ove primjere koda pokazati kako izraditi nativni klijent aplikacije koje mogu pozivati web API-ji koji su zaštićeni po Azure AD. Koriste [Azure AD provjera autentičnosti biblioteke (ADAL)](active-directory-authentication-libraries.md) i [2.0 OAuth u Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx).

| Jezik/platforme | Uzorak | Opis
| ----------------- | ------ | -----------
| JavaScript | [NativeClient MultiTarget Cordova](https://github.com/Azure-Samples/active-directory-cordova-multitarget) | Koristiti ADAL dodatak za Apache Cordova za izradu aplikaciju Apache Cordova koja se poziva na API na webu i koristi Azure AD za provjeru autentičnosti.
| C# / .NET | [NativeClient DotNet](https://github.com/Azure-Samples/active-directory-dotnet-native-desktop) | Aplikacija za .NET WPF koja se poziva API web-mjesto koje je osigurani pomoću Azure AD.
| C# / .NET | [NativeClient WindowsStore](https://github.com/Azure-Samples/active-directory-dotnet-windows-store) | Aplikacija iz Windows trgovine koja poziva API web-mjesto koje je osigurani s Azure AD.
| C# / .NET | [NativeClient-WebAPI-MultiTenant-WindowsStore](https://github.com/Azure-Samples/active-directory-dotnet-webapi-multitenant-windows-store) | Aplikacija iz Windows trgovine koja se poziva više klijentu web API koji je osigurani s Azure AD.
| C# / .NET | [WebAPI OnBehalfOf DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof) | Aplikacija za nativni klijent koja se poziva web API dobiva token djelovanje ime izvornog korisnika, a zatim koristi token da biste pozvali druge API na webu.
| C# / .NET | [NativeClient WindowsPhone8.1](https://github.com/Azure-Samples/active-directory-dotnet-windowsphone-8.1) | Iz Windows trgovine aplikacija za Windows Phone 8.1 poziva API web-mjesto koje je osigurani po Azure AD.
| ObjC | [NativeClient iOS](https://github.com/Azure-Samples/active-directory-ios) | IOS aplikacije koja se poziva na API na Webu koji zahtijeva Azure AD za provjeru autentičnosti.
| C# / .NET | [WebAPI ManuallyValidateJwt DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapi-manual-jwt-validation) | Aplikacija za nativni klijent koja obuhvaća logike za obradu JWT tokena na web-mjestu API-JA, umjesto korištenja OWIN proizvod.
| C# / Xamarin | [NativeClient, Xamarin i Android](https://github.com/Azure-Samples/active-directory-xamarin-android) | Xamarin vezu da biste na izvorni Azure AD provjera autentičnosti biblioteke (ADAL) za biblioteku sa sustavom Android.
| C# / Xamarin | [NativeClient Xamarin iOS](https://github.com/Azure-Samples/active-directory-xamarin-ios) | Xamarin vezu da biste na izvorni Azure AD provjera autentičnosti biblioteke (ADAL) za iOS.
| C# / Xamarin | [NativeClient MultiTarget DotNet](https://github.com/Azure-Samples/active-directory-dotnet-native-multitarget) | Project Xamarin koja pronalaze pet platforme i poziva API web-mjesto koje je osigurani po Azure AD.
| C# / .NET | [NativeClient-Headless-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-native-headless) | Matičnoj aplikaciji koja izvršava koji nije interaktivan provjeru autentičnosti i poziva API web-mjesto koje je osigurani po Azure AD.



## <a name="web-application-to-web-api"></a>Web-aplikaciju za API na webu

Ove primjere koda Prikaži kako koristiti [2.0 OAuth u Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx) za stvaranje web-aplikacije koje mogu pozivati web-API-ji koji su zaštićeni po Azure AD.

| Jezik/platforme | Uzorak | Opis
| ----------------- | ------ | -----------
| C# / .NET | [WebApp-WebAPI-OpenIDConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-openidconnect) | Nazovite web API-JA s prijavljeni u korisničke dozvole.
|  C# / .NET | [WebApp-WebAPI-OAuth2-AppIdentity-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-appidentity) | Nazovite web API-JA s dozvolama za aplikacije.
| C# / .NET | [WebApp-WebAPI-OAuth2-identitetu korisnika – Dotnet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity) | Dodajte autorizacije s [OAuth 2.0 u Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx) postojeću web-aplikaciju da bi ga da biste uputili poziv API web-mjesto.
| JavaScript | [WebAPI Nodejs](https://github.com/Azure-Samples/active-directory-node-webapi) | Postavljanje REST API-JA servisa koji je integriran s Azure AD za zaštitu API-JA. Obuhvaća Node.js poslužitelj s Web API.
| C# / .NET | [WebApp-WebAPI-MultiTenant-OpenIdConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-multitenant-openidconnect) |  Više klijentu MVC web-aplikaciji koja koristi OpenID povezivanje (ASP.Net OpenID povezivanje OWIN proizvod) za provjeru autentičnosti korisnika iz klijent za Azure AD. Koristi se kod autorizacije pozvati API grafikonu.

## <a name="server-or-daemon-application-to-web-api"></a>Poslužitelj ili Daemon aplikaciju za API na webu

Ove primjere koda pokazati kako izraditi daemon ili poslužitelj za aplikaciju koja se dobiva resurse u web-API pomoću [Azure AD provjera autentičnosti biblioteke (ADAL)](active-directory-authentication-libraries.md) i [2.0 OAuth u Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx).

| Jezik/platforme | Uzorak | Opis
| ----------------- | ------ | -----------
| C# / .NET | [Daemon DotNet](https://github.com/Azure-Samples/active-directory-dotnet-daemon) | Program konzole poziva na API na webu. Klijent vjerodajnica je lozinka.
| C# / .NET | [Daemon CertificateCredential DotNet](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential) | Konzole za aplikaciju koja se poziva na API na webu. Klijent vjerodajnica je certifikat.


## <a name="calling-azure-ad-graph-api"></a>Pozivanje Azure AD grafikonu API-JA

Ove uzorak koda pokazati kako izraditi aplikacije koje mogu pozivati Azure AD grafikonu API-JA za čitanje i pisanje direktorija podataka.

| Jezik/platforme | Uzorak | Opis
| ----------------- | ------ | -----------
| Java | [WebApp GraphAPI Java](https://github.com/Azure-Samples/active-directory-java-graphapi-web) | Web-aplikacije koja koristi grafikonu API-JA za pristup podacima Azure AD direktorija.
| PHP | [WebApp GraphAPI PHP](https://github.com/Azure-Samples/active-directory-php-graphapi-web) | Web-aplikacije koja koristi grafikonu API-JA za pristup podacima Azure AD direktorija.
| C# / .NET | [WebApp GraphAPI DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-web) | Web-aplikacije koja koristi grafikonu API-JA za pristup podacima Azure AD direktorija.
| C# / .NET | [ConsoleApp GraphAPI DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-console) | Aplikacija konzolu pokazuje uobičajene pozive čitanje i pisanje na grafikonu API-JA i prikazuje izvršavanje dodjele licence za korisnika i ažuriranje minijature fotografije korisnika i veze.
| C# / .NET | [ConsoleApp-GraphAPI-DiffQuery-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-diffquery) | Konzola za aplikaciju koja koristi razlikovna upita u grafikonu API-JA da biste dobili periodičku promjene korisnik u klijentu za Azure AD.
| C# / .NET | [WebApp-GraphAPI-DirectoryExtensions-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-directoryextensions-web) | Aplikaciju MVC koristi grafikonu API upita da biste generirali jednostavne tvrtke organizacijskog grafikona.
| PHP | [WebApp-GraphAPI-DirectoryExtensions-PHP](https://github.com/Azure-Samples/active-directory-php-graphapi-directoryextensions-web) | Aplikacija i koja se poziva API grafikonu registrirati datotečni nastavak i pročitajte, ažuriranje i brisanje vrijednosti iz atributa kućni broj.


## <a name="authorization"></a>Autorizacija

Ove kod primjera pokazuju kako pomoću Azure AD za autorizaciju.

| Jezik/platforme | Uzorak | Opis
| ----------------- | ------ | -----------
| C# / .NET | [WebApp GroupClaims DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-groupclaims) | Izvođenje kontrola uloge koje se temelje pristupa (RBAC) u aplikaciji koja je integriran s Azure AD pomoću zahtjevima grupe Azure Active Directory.
| C# / .NET | [WebApp RoleClaims DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-roleclaims) | Izvođenje kontrola uloga temelji pristupa (RBAC) pomoću servisa Azure Active Directory aplikacije uloga u aplikaciji koja je integriran s Azure AD.


## <a name="legacy-walkthroughs"></a>Stari prikazi

Ove vodiči koristiti malo starije tehnologiju, ali i dalje mogu biti koji vas zanimaju.

| Jezik/platforme | Uzorak | Opis
| ----------------- | ------ | -----------
| C# / .NET | [Provjeru autentičnosti na temelju uloga i ACL-poštu u aplikaciji Microsoft Azure AD](http://go.microsoft.com/fwlink/?LinkId=331694) | Izvođenje na temelju uloga autorizacija (RBAC) i autorizacije ACL-poštu u aplikaciji koja je integriran s Azure AD.
| C# / .NET |  [AAL – OSTALE servisu Windows Store app – provjera autentičnosti](http://go.microsoft.com/fwlink/?LinkId=330605) |  Koristite [Azure AD provjera autentičnosti biblioteke (ADAL)](active-directory-authentication-libraries.md) (prijašnjeg naziva AAL) za Beta iz trgovine Windows da biste dodali mogućnosti provjere autentičnosti korisnika aplikaciju iz Windows trgovine.
| C# / .NET | [Provjera autentičnosti ADAL - nativni aplikaciju servisa REST - s AAD putem dijaloški okvir preglednika](http://go.microsoft.com/fwlink/?LinkId=259814) |  Dodajte mogućnosti provjere autentičnosti korisnika s klijentskim WPF pomoću [Azure AD provjera autentičnosti biblioteke (ADAL)](active-directory-authentication-libraries.md) .
| C# / .NET | [Provjera autentičnosti ADAL - nativni aplikaciju servisa REST - s ACS putem dijaloški okvir preglednika](http://code.msdn.microsoft.com/AAL-Native-App-to-REST-de57f2cc) |  Pomoću [Azure AD provjera autentičnosti biblioteke (ADAL)](active-directory-authentication-libraries.md) i [Pristup kontrola servisa 2.0 (ACS)](http://msdn.microsoft.com/library/azure/hh147631.aspx) da biste dodali mogućnosti provjere autentičnosti korisnika WPF klijentu.
| C# / .NET | [ADAL - provjeru autentičnosti poslužitelja s poslužiteljem](http://go.microsoft.com/fwlink/?LinkId=259816) | Korištenje [Azure AD provjera autentičnosti biblioteke (ADAL)](active-directory-authentication-libraries.md) na servis za sigurnu poziva od strane procesa poslužitelja sa servisom REST API-JA za MVC4 Web.
| C# / .NET | [Dodavanje prijave na web-aplikacije pomoću Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) | Konfiguriranje aplikacije za .NET za izvođenje web jedinstvenu prijavu protiv Azure AD imenik tvrtke.
| C# / .NET | [Razvoj više klijentu web-aplikacije s Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-webapp-multitenant-openidconnect) | Koristite Azure AD da biste dodali jedinstvenu prijavu i mogućnosti programa access direktorija jednu .NET aplikaciju za radi preko više tvrtke ili ustanove.
JAVA | [Java uzorka aplikaciju za Azure AD grafikonu API-JA](http://go.microsoft.com/fwlink/?LinkId=263969) | Korištenje grafikonu API-JA za pristup podacima direktorija iz Azure AD.
PHP | [PHP uzorak aplikaciju za Azure AD grafikonu API-JA](http://code.msdn.microsoft.com/PHP-Sample-App-For-Windows-228c6ddb) | Korištenje grafikonu API-JA za pristup podacima direktorija iz Azure AD.
| C# / .NET | [Ogledna aplikaciju za Azure AD grafikonu API-JA](http://go.microsoft.com/fwlink/?LinkID=262648) | Korištenje grafikonu API-JA za pristup podacima direktorija iz Azure AD.
| C# / .NET | [Ogledna aplikacija za Azure AD grafikonu razlikovno upita](http://go.microsoft.com/fwlink/?LinkId=275398) | Da biste dobili periodičku promjene korisnik u klijentu za Azure AD, poslužite se razlikovno upitom API grafikonu.
| C# / .NET | [Ogledna aplikacije za integraciju oblaka više klijentske aplikacije za Azure AD](http://go.microsoft.com/fwlink/?LinkId=275397) | Integriranje više klijentske aplikacije u Azure AD.
| C# / .NET | [Zaštita aplikacija iz Windows trgovine i REST Web Service pomoću Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-windows-store) | Stvaranje jednostavne API na webu resursa i klijentska aplikacija iz trgovine Windows Azure AD pomoću i [Azure AD provjera autentičnosti biblioteke (ADAL)](active-directory-authentication-libraries.md).
| C# / .NET| [Korištenje grafikonu API za Azure AD za upite](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-web) | Konfiguriranje aplikacije Microsoft .NET da biste koristili Azure AD grafikonu API-JA za pristup podacima iz imenika Azure AD klijenta.

## <a name="see-also"></a>Vidi također

##### <a name="other-resources"></a>Ostali resursi

[Vodič za programiranje Azure Active Directory](active-directory-developers-guide.md)

[Azure AD grafikonu API konceptualni i Reference](https://msdn.microsoft.com/library/azure/hh974476.aspx)

[Azure AD grafikonu API pomoćnih biblioteka](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient)

