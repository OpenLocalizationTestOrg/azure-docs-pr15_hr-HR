<properties 
   pageTitle="Active Directory provjeru autentičnosti i resursima | Microsoft Azure"
   description="Je Razvojni vodič za provjeru autentičnosti s API resursima Azure i servisa Active Directory za integraciju aplikacije s druge pretplate za Azure."
   services="azure-resource-manager,active-directory"
   documentationCenter="na"
   authors="dushyantgill"
   manager="timlt"
   editor="tysonn" />
<tags 
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/31/2016"
   ms.author="dugill;tomfitz" />

# <a name="how-to-use-azure-active-directory-and-resource-manager-to-manage-a-customers-resources"></a>Kako koristiti Azure Active Directory i resursima za upravljanje resursima klijenta

## <a name="introduction"></a>Uvod

Ako ste softverski programer koji treba da biste stvorili aplikaciju koja upravlja Azure resursi za klijenta, u ovoj se temi objašnjava provjeru s API-ji resursima Azure i dobiti pristup resursa u druge pretplate. 

Pokrenite aplikaciju možete pristupiti resursima API-ji sljedeća dva načina:

1. **Korisniku + aplikacije programa access**: za aplikacije koja pristup ime korisnika za prijavljeni u. Taj se način radi aplikacije, kao što su web-aplikacije i alati naredbenog retka, koje se tiču samo "interaktivne upravljanja" Azure resursa.
1. **Pristup samo za aplikacije**: za aplikacije koji se izvode daemon servise i Zakazani zadaci. Identitet aplikacije moguć je izravan pristup resurse. Taj se način radi aplikacije koje su potrebne Dugoročne "izvanmrežnog pristupa" Azure.

Ova tema sadrži detaljne upute za stvaranje aplikacije koji uključuje obje ove metode autorizacije. Prikazuje kako izvesti svaki korak REST API-JA ili C#. Dovršavanje aplikacije ASP.NET MVC dostupna je na [https://github.com/dushyantgill/VipSwapper/tree/master/CloudSense](https://github.com/dushyantgill/VipSwapper/tree/master/CloudSense).

Kod za u ovoj se temi se izvodi kao web-aplikacije koje možete isprobati na [http://vipswapper.azurewebsites.net/cloudsense](http://vipswapper.azurewebsites.net/cloudsense). 

## <a name="what-the-web-app-does"></a>Funkcija web-aplikaciji

Web-aplikacije:

1. Znak u Azure korisnika.
2. Se traži od korisnika da biste dodijelili pristup web app upravitelju resursa.
3. Dohvaća korisniku + token aplikacije programa access za pristup Voditelj resursa.
4. Koristi token (iz korak 3) poziva Voditelj resursa i aplikacije servisa glavnicu dodijeliti uloga u pretplatu, koji omogućuje pristup Dugoročne aplikacije pretplate.
5. Dohvaća tokena samo za aplikaciju programa access.
6. Koristi token (iz korak 5) za upravljanje resursima u pretplati putem upravitelja resursa.

Evo tijek završetka do kraja web-aplikacije.

![Provjera autentičnosti resursima tijek](./media/resource-manager-api-authentication/Auth-Swim-Lane.png)

Kao korisnik, unesite id za pretplatu za pretplatu koju želite koristiti:

![Navedite id pretplate](./media/resource-manager-api-authentication/sample-ux-1.png)

Odaberite račun koji želite koristiti za zapisivanje u.

![Odaberite račun](./media/resource-manager-api-authentication/sample-ux-2.png)

Unesite vjerodajnice.

![Unesite vjerodajnice](./media/resource-manager-api-authentication/sample-ux-3.png)

Dopustiti pristup aplikacije za Azure pretplate:
 
![Dopuštanje pristupa](./media/resource-manager-api-authentication/sample-ux-4.png)
 
Upravljanje pretplatama povezanog:

![Povezivanje pretplate](./media/resource-manager-api-authentication/sample-ux-7.png)


## <a name="register-application"></a>Registracija aplikacije

Prije početka kodiranje registrirati web-aplikaciju programa s Azure Active Directory (AD). Registracija aplikacije stvara središnje identiteta za aplikacije u Azure AD. Obuhvaćaju se osnovne informacije o aplikaciji kao što su OAuth ID klijenta, odgovor URL-ove i vjerodajnice za aplikacije koji se koristi za provjeru autentičnosti i pristup Azure resursima API-ji. Registracija aplikacije zapisa i razne ovlaštenog dozvole koje se aplikacija treba prilikom pristupanja APIs Microsoft ime korisnika. 

Budući da se aplikacijom pristupa druge pretplate, već morate konfigurirati kao više klijentske aplikacije. Da biste prošla provjeru, unesite domenu pridružene servisa Active Directory. Da biste vidjeli domene povezane s servisa Active Directory, prijavite se na [portal za klasični](https://manage.windowsazure.com). Odaberite servisa Active Directory, a zatim **domene**.

Sljedeći primjer pokazuje kako registrirati aplikaciju pomoću Azure PowerShell. Morate imati najnoviju verziju (kolovoz 2016) Azure PowerShell za ovaj da bi naredba funkcionirala. 

    $app = New-AzureRmADApplication -DisplayName "{app name}" -HomePage "https://{your domain}/{app name}" -IdentifierUris "https://{your domain}/{app name}" -Password "{your password}" -AvailableToOtherTenants $true
    
Da biste se prijavili kao AD aplikaciju, morate aplikacije id i lozinku. Da biste vidjeli id aplikacije koje se vraćaju iz prethodne naredbe, koristite:

    $app.ApplicationId

Sljedeći primjer pokazuje kako registrirati aplikaciju pomoću Azure EŽA. 

    azure ad app create --name {app name} --home-page https://{your domain}/{app name} --identifier-uris https://{your domain}/{app name} --password {your password} --available true

Rezultati obuhvatiti ID programa koje je potrebno prilikom provjere autentičnosti kao aplikaciju.

### <a name="optional-configuration---certificate-credential"></a>Neobavezni konfiguracija – potvrde vjerodajnica

Azure AD podržava i vjerodajnice za potvrdu za aplikacije: Stvaranje samopotpisanog certifikata, zadržavanje privatni ključ i dodavanje javni ključ svoju registraciju Azure AD aplikacije. Za provjeru autentičnosti, aplikacija šalje small tereta Azure AD prijavljeni pomoću privatni ključ i Azure AD Provjeri valjanost potpis pomoću javni ključ koji ste registrirali.

Informacije o stvaranju aplikacije AD pomoću certifikata, potražite u članku [Korištenje ljuske PowerShell Azure da biste stvorili glavni pristupa resursima servisa](resource-group-authenticate-service-principal.md#create-service-principal-with-certificate) ili [Korištenje Azure EŽA da biste stvorili glavni pristupa resursima servisa](resource-group-authenticate-service-principal-cli.md#create-service-principal-with-certificate).

## <a name="get-tenant-id-from-subscription-id"></a>Id klijenta zatražite od id pretplate

Da biste zatražili tokena koje je moguće koristiti da biste nazvali Voditelj resursa, aplikacija treba znati ID klijenta koji hostira Azure pretplate Azure AD klijent. Najvjerojatnije korisnici znati identifikacijskim pretplatu, no možda ne znaju njihove klijentu ID-a za Active Directory. Da biste dobili id korisnika klijentu, od korisnika traži id pretplate. Prilikom slanja zahtjeva o pretplati, unesite taj id pretplate:

    https://management.azure.com/subscriptions/{subscription-id}?api-version=2015-01-01

Zahtjev neće uspjeti jer je korisnik ima niste prijavljeni u još, ali možete dohvatiti id klijenta s odgovor. U tom iznimku dohvatiti id klijenta s vrijednost zaglavlja odgovora za **Provjeru autentičnosti "www"**. Pogledajte Ova implementacija u način [GetDirectoryForSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L20) .

## <a name="get-user--app-access-token"></a>Početak korisniku + token aplikacije programa access

Aplikacija preusmjerava korisnika Azure AD pomoću programa OAuth 2.0 autorizirali zatražiti - autentičnost korisnikove vjerodajnice i vratiti je autorizacije kod. Aplikacija koristi autorizacije kod da biste pristupili programa tokena za Voditelj resursa. Način [ConnectSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/Controllers/HomeController.cs#L42) stvara zahtjev za provjeru autentičnosti.

Ova tema prikazuje zahtjeve REST API-JA za provjeru autentičnosti korisnika. Da biste izvršili provjeru autentičnosti u kodu možete koristiti i pomoćnih biblioteka. Dodatne informacije o te biblioteke potražite u članku [Azure Active Directory provjera autentičnosti biblioteke](./active-directory/active-directory-authentication-libraries.md). Smjernice o integraciji upravljanje identitetom u aplikaciji, potražite u članku [Vodič za Azure Active Directory programiranje](./active-directory/active-directory-developers-guide.md).

### <a name="auth-request-oauth-20"></a>Zahtjev za provjeru autentičnosti (OAuth 2.0)

Azure AD ovlasti krajnjoj problema programa Open ID povezivanje/OAuth2.0 autorizirali zahtjev:

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize

Parametrima niza upita koji su dostupni za taj zahtjev opisana su u temi [zahtjev je kod za autorizaciju](./active-directory/active-directory-protocols-oauth-code.md#request-an-authorization-code) .

Sljedeći primjer prikazuje način da biste zatražili OAuth2.0 autorizacije:

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize?client_id=a0448380-c346-4f9f-b897-c18733de9394&response_mode=query&response_type=code&redirect_uri=http%3a%2f%2fwww.vipswapper.com%2fcloudsense%2fAccount%2fSignIn&resource=https%3a%2f%2fgraph.windows.net%2f&domain_hint=live.com

Azure AD potvrđuje korisnika, a zatim po potrebi traži od korisnika da biste dodijelili dozvolu za aplikaciju. Vraća kod autorizacije URL odgovor aplikacije. Ovisno o tražene response_mode Azure AD ili šalje natrag podatke u nizu upita ili kao post podatke.

    code=AAABAAAAiL****FDMZBUwZ8eCAA&session_state=2d16bbce-d5d1-443f-acdf-75f6b0ce8850

### <a name="auth-request-open-id-connect"></a>Zahtjev za provjeru autentičnosti (Otvori ID povezivati)

Ako ne samo želite pristupiti Voditelj resursa Azure ime korisnika, ali i korisnicima omogućuje prijavite se na svoj račun za Azure AD pomoću aplikacije, izdavanje programa Open ID Connect autorizirali zahtjev. S Otvori povezati ID-a, aplikacije i prima programa id_token iz Azure AD aplikacije možete koristiti za prijavu u korisnika.

Parametrima niza upita koji su dostupni za taj zahtjev opisana su u temi [pošaljite zahtjev za prijavu](./active-directory/active-directory-protocols-openid-connect-code.md#send-the-sign-in-request) .

Zahtjev za otvaranje povezivanje ID primjer je:

     https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize?client_id=a0448380-c346-4f9f-b897-c18733de9394&response_mode=form_post&response_type=code+id_token&redirect_uri=http%3a%2f%2fwww.vipswapper.com%2fcloudsense%2fAccount%2fSignIn&resource=https%3a%2f%2fgraph.windows.net%2f&scope=openid+profile&nonce=63567Dc4MDAw&domain_hint=live.com&state=M_12tMyKaM8

Azure AD potvrđuje korisnika, a zatim po potrebi traži od korisnika da biste dodijelili dozvolu za aplikaciju. Vraća kod autorizacije URL odgovor aplikacije. Ovisno o tražene response_mode Azure AD ili šalje natrag podatke u nizu upita ili kao post podatke.

Primjer Otvori povezivanje ID odgovor je:

    code=AAABAAAAiL*****I4rDWd7zXsH6WUjlkIEQxIAA&id_token=eyJ0eXAiOiJKV1Q*****T3GrzzSFxg&state=M_12tMyKaM8&session_state=2d16bbce-d5d1-443f-acdf-75f6b0ce8850

### <a name="token-request-oauth20-code-grant-flow"></a>Zahtjev za tokena (OAuth2.0 kod Grant tijek)

Sad kad se aplikacija primila kod autorizacije iz Azure AD, vrijeme je da biste pristupili na tokena za Azure Voditelj resursa.  Objavljivanje programa OAuth2.0 kod Grant tokena zahtjev za krajnju točku Azure AD tokena: 

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Token

Parametrima niza upita koji su dostupni za taj zahtjev opisana su u temi [pomoću koda za autorizaciju](./active-directory/active-directory-protocols-oauth-code.md#use-the-authorization-code-to-request-an-access-token) .

Sljedeći primjer prikazuje zahtjev za kod grant token s vjerodajnicama lozinku:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1

    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012

    grant_type=authorization_code&code=AAABAAAAiL9Kn2Z*****L1nVMH3Z5ESiAA&redirect_uri=http%3A%2F%2Flocalhost%3A62080%2FAccount%2FSignIn&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_secret=olna84E8*****goScOg%3D

Kada radite s vjerodajnicama certifikata, stvorite JSON Web tokena (JWT) i znak (RSA SHA256) pomoću privatni ključ vaše aplikacije potvrde vjerodajnica. Vrste zahtjeva za token prikazane su u [JWT tokena zahtjevima](./active-directory/active-directory-protocols-oauth-code.md#jwt-token-claims). Referenca, potražite u članku [Active Directory provjere autentičnosti biblioteke (.NET) kod](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/blob/dev/src/ADAL.PCL.Desktop/CryptographyHelper.cs) za potpisivanje tokena JWT pridruživanju klijenta.

[Otvaranje povezivanje ID specifikacija](http://openid.net/specs/openid-connect-core-1_0.html#ClientAuthentication) detalje potražite na provjera autentičnosti klijenta. 

Sljedeći primjer prikazuje zahtjev za kod grant token s potvrde vjerodajnica:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1
    
    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012
    
    grant_type=authorization_code&code=AAABAAAAiL9Kn2Z*****L1nVMH3Z5ESiAA&redirect_uri=http%3A%2F%2Flocalhost%3A62080%2FAccount%2FSignIn&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer&client_assertion=eyJhbG*****Y9cYo8nEjMyA

Primjer odgovor token grant kod: 

    HTTP/1.1 200 OK

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1432039858","not_before":"1432035958","resource":"https://management.core.windows.net/","access_token":"eyJ0eXAiOiJKV1Q****M7Cw6JWtfY2lGc5A","refresh_token":"AAABAAAAiL9Kn2Z****55j-sjnyYgAA","scope":"user_impersonation","id_token":"eyJ0eXAiOiJKV*****-drP1J3P-HnHi9Rr46kGZnukEBH4dsg"}

#### <a name="handle-code-grant-token-response"></a>Rukovanje kod grant tokena odgovor

Uspješan tokena odgovor sadrži (korisnik + aplikacija) token za pristup za Azure Voditelj resursa. Aplikacija koristi token za pristup da biste pristupili resursima ime korisnika. Vijek pristupna tokena izdala Azure AD je jedan sat. Je vjerojatno da web-aplikacija nije potrebno obnoviti (korisnik + aplikacija) token za pristup. Ako je potrebno obnoviti token za pristup, pomoću osvježavanja token koji aplikacije prima tokena odgovor. Objavljivanje programa OAuth2.0 tokena zahtjev za krajnju točku Azure AD tokena: 

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Token

Parametri se koriste uz zahtjev za osvježavanjem opisana su u [Osvježavanje token za pristup](./active-directory/active-directory-protocols-oauth-code.md#refreshing-the-access-tokens).

Sljedeći primjer pokazuje kako koristiti osvježavanje tokena:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1

    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012

    grant_type=refresh_token&refresh_token=AAABAAAAiL9Kn2Z****55j-sjnyYgAA&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_secret=olna84E8*****goScOg%3D

Iako se tokeni osvježavanja može koristiti da biste dobili novi pristupna tokena za Azure Voditelj resursa, nisu prikladna za izvanmrežni pristup putem aplikacije. Ograničeno je vijek tokeni osvježavanja, a osvježavanja tokeni koje su vezane uz korisnika. Ako korisnik napusti tvrtku ili ustanovu, aplikacije pomoću token osvježavanja gubi programa access. Ovaj pristup nije prikladna za aplikacije koje koriste timovima upravljanje njihove Azure resursi.

## <a name="check-if-user-can-assign-access-to-subscription"></a>Potvrdite ako korisnik možete dodijeliti pristup pretplate

Aplikacija sada ima token da biste pristupili Voditelj resursa Azure ime korisnika. Sljedeći je korak povezati aplikacije s pretplatom. Nakon povezivanja aplikacije mogu upravljati tim pretplate čak i kad korisnik ne postoji (Dugoročne izvanmrežnog pristupa). 

Za svaku pretplatu za povezivanje nazovite [dozvole za popise resursima](https://msdn.microsoft.com/library/azure/dn906889.aspx) API da biste odredili ima li korisnik pristup upravljanja pravima za pretplatu.

Način [UserCanManagerAccessForSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L44) uzorka aplikacije ASP.NET MVC implementira ovog poziva.

Sljedeći primjer prikazuje način da biste zatražili korisničke dozvole za pretplatu. 83cfe939-2402-4581-b761-4f59b0a041e4 je id pretplate.

    GET https://management.azure.com/subscriptions/83cfe939-2402-4581-b761-4f59b0a041e4/providers/microsoft.authorization/permissions?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV1QiLC***lwO1mM7Cw6JWtfY2lGc5A

Primjer odgovor da biste dobili korisničke dozvole na pretplatu je:

    HTTP/1.1 200 OK

    {"value":[{"actions":["*"],"notActions":["Microsoft.Authorization/*/Write","Microsoft.Authorization/*/Delete"]},{"actions":["*/read"],"notActions":[]}]}

Dozvole za API vraća više dozvola. Svaki dozvola sastoji se od dopuštene Akcije (akcije) i dopušteno Akcije (notactions). Ako postoji akcije na popisu dopuštenih akcije potrebne dozvole, a nema na popisu notactions ta dozvola, korisnik može da biste izvršili tu akciju. **Microsoft.Authorization/roleassignments/Write** je akcija da koji daje pristupa upravljanja pravima. Aplikacija morate analizirati rezultat dozvole da biste potražili podudaranja regex ovog niza akcija u akcije i notactions svaku dozvolu.

## <a name="get-app-only-access-token"></a>Pristup samo za aplikacije tokena

Sada, znate da ako korisniku dodijelili pristup Azure pretplatu. Daljnji koraci su:

1. Dodijelite odgovarajuću ulogu RBAC identitetu vaše aplikacije u sklopu pretplate.
2. Provjera valjanosti dodjelu pristupa postavljanjem upita za aplikacije dozvola na pretplatu ili pristupom Voditelj resursa pomoću token samo za aplikacije.
1. Snimanje veze u strukturu aplikacije "povezanog pretplate" podataka – persisting id pretplate.

Pogledajmo bliže u prvom koraku. Da biste dodijelili odgovarajuću ulogu RBAC identiteta aplikacije, morate odrediti:

- Id objekta identitet vaše aplikacije u korisnikovu Azure Active Directory
- Identifikator RBAC uloge koje je potrebno vaše aplikacije u sklopu pretplate

Kada aplikacija potvrđuje korisnika iz Azure AD, stvara glavni objekt servisa za aplikaciju u tom Azure AD. Azure omogućuje RBAC uloge želite dodijeliti upravitelji servisa da biste dodijelili izravan pristup odgovarajućem aplikacijama Azure resursa. Ova je akcija točno želimo da biste učinili. Upit API Azure AD grafikonu da biste utvrdili identifikator glavnice servisa aplikacije u prijavljeni korisnik je Azure AD.

Imate samo token za pristup za Azure Voditelj resursa – trebate novi token pristup da biste uputili poziv API Azure AD grafikonu. Svaku aplikaciju u Azure AD s dozvolom za upit glavni objekt vlastitu servisa, pa je dovoljno token za pristup samo za aplikacije.

<a id="app-azure-ad-graph">
### <a name="get-app-only-access-token-for-azure-ad-graph-api"></a>Pristup samo za aplikacije tokena za Azure AD grafikonu API-JA

Za aplikacije za provjeru autentičnosti i znak za Azure AD grafikonu API poslao klijent vjerodajnica Grant OAuth2.0 tokena zahtjev za tijek za Azure AD tokena krajnje (**https://login.microsoftonline.com/ {directory_domain_name} / OAuth2/Token**).

Način [GetObjectIdOfServicePrincipalInOrganization](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureADGraphAPIUtil.cs) primjer aplikacije ASP.net MVC dobiva samo za aplikaciju programa access tokena za API grafikonu pomoću za provjeru autentičnosti biblioteke imenika Active Directory za .NET.

Parametrima niza upita koji su dostupni za taj zahtjev opisana su u temi [zahtjev tokena programa Access](./active-directory/active-directory-protocols-oauth-service-to-service.md#request-an-access-token) .

Na primjer zahtjev za klijent vjerodajnica dodijelite tokena: 

    POST https://login.microsoftonline.com/62e173e9-301e-423e-bcd4-29121ec1aa24/oauth2/token HTTP/1.1
    Content-Type: application/x-www-form-urlencoded
    Content-Length: 187</pre>
    <pre>grant_type=client_credentials&client_id=a0448380-c346-4f9f-b897-c18733de9394&resource=https%3A%2F%2Fgraph.windows.net%2F &client_secret=olna8C*****Og%3D

Primjer odgovor token Dodjela vjerodajnica za klijent: 

    HTTP/1.1 200 OK

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1432039862","not_before":"1432035962","resource":"https://graph.windows.net/","access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiJodHRwczovL2dyYXBoLndpbmRv****G5gUTV-kKorR-pg"}

### <a name="get-objectid-of-application-service-principal-in-user-azure-ad"></a>Dobivanje ID objekta aplikacije servisa glavnice u korisnika Azure AD

Sada pomoću token za pristup samo za aplikacije poslati upit [servisa Azure AD grafikonu upravitelji](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity) API da biste odredili Id objekta servisa aplikacije glavni u direktoriju.

Način [GetObjectIdOfServicePrincipalInOrganization](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureADGraphAPIUtil.cs#) primjer aplikacije ASP.net MVC implementira ovog poziva.

Sljedeći primjer prikazuje način da biste zatražili glavnicu aplikacije servisa. a0448380-c346-4f9f-b897-c18733de9394 je id klijenta aplikacije.

    GET https://graph.windows.net/62e173e9-301e-423e-bcd4-29121ec1aa24/servicePrincipals?api-version=1.5&$filter=appId%20eq%20'a0448380-c346-4f9f-b897-c18733de9394' HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJK*****-kKorR-pg

Sljedeći primjer prikazuje odgovor na zahtjev za uslugu aplikacije glavni 

    HTTP/1.1 200 OK

    {"odata.metadata":"https://graph.windows.net/62e173e9-301e-423e-bcd4-29121ec1aa24/$metadata#directoryObjects/Microsoft.DirectoryServices.ServicePrincipal","value":[{"odata.type":"Microsoft.DirectoryServices.ServicePrincipal","objectType":"ServicePrincipal","objectId":"9b5018d4-6951-42ed-8a92-f11ec283ccec","deletionTimestamp":null,"accountEnabled":true,"appDisplayName":"CloudSense","appId":"a0448380-c346-4f9f-b897-c18733de9394","appOwnerTenantId":"62e173e9-301e-423e-bcd4-29121ec1aa24","appRoleAssignmentRequired":false,"appRoles":[],"displayName":"CloudSense","errorUrl":null,"homepage":"http://www.vipswapper.com/cloudsense","keyCredentials":[],"logoutUrl":null,"oauth2Permissions":[{"adminConsentDescription":"Allow the application to access CloudSense on behalf of the signed-in user.","adminConsentDisplayName":"Access CloudSense","id":"b7b7338e-683a-4796-b95e-60c10380de1c","isEnabled":true,"type":"User","userConsentDescription":"Allow the application to access CloudSense on your behalf.","userConsentDisplayName":"Access CloudSense","value":"user_impersonation"}],"passwordCredentials":[],"preferredTokenSigningKeyThumbprint":null,"publisherName":"vipswapper"quot;,"replyUrls":["http://www.vipswapper.com/cloudsense","http://www.vipswapper.com","http://vipswapper.com","http://vipswapper.azurewebsites.net","http://localhost:62080"],"samlMetadataUrl":null,"servicePrincipalNames":["http://www.vipswapper.com/cloudsense","a0448380-c346-4f9f-b897-c18733de9394"],"tags":["WindowsAzureActiveDirectoryIntegratedApp"]}]}

### <a name="get-azure-rbac-role-identifier"></a>Dobiti identifikator Azure RBAC uloga

Da biste dodijelili odgovarajuću ulogu RBAC glavni uslugu, morate odrediti identifikator uloge Azure RBAC.

Desni RBAC uloga aplikacije:

- Ako aplikacija samo nadzire pretplatu, bez ikakvih promjena, zahtijeva čitač dozvole samo u sklopu pretplate. Dodijeliti ulogu **Reader** .
- Ako aplikacija upravlja Azure pretplatu, stvaranje i izmjena i brisanje entiteti je potrebna je jedna od suradničke dozvole.
  - Da biste upravljali određene vrste resursa, dodijelite uloge suradnika specifične za resursa (suradnička virtualnog računala virtualne mreža suradnika, suradničke račun za pohranu, itd.)
  - Da biste upravljali bilo koju vrstu resursa, dodijeliti ulogu **suradnika** .

Dodjela uloge za aplikaciju je vidljiva korisnicima, pa odaberite najmanje potrebne ovlasti.

Nazovite [definicije uloge resursima API](https://msdn.microsoft.com/library/azure/dn906879.aspx) da popis svih uloga Azure RBAC i pretraživanje, a zatim ponavljanje rezultat da biste pronašli željenu ulogu definiciju po nazivu.

Način [GetRoleId](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L246) uzorka aplikacije ASP.net MVC implementira ovog poziva.

Sljedeći primjer zahtjev prikazuje kako doći do identifikator Azure RBAC uloge. 09cbd307-aa71-4aca-b346-5f253e6e3ebb je id pretplate.

    GET https://management.azure.com/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV*****fY2lGc5

Odgovor je u sljedećem obliku: 

    HTTP/1.1 200 OK

    {"value":[{"properties":{"roleName":"API Management Service Contributor","type":"BuiltInRole","description":"Lets you manage API Management services, but not access to them.","scope":"/","permissions":[{"actions":["Microsoft.ApiManagement/Services/*","Microsoft.Authorization/*/read","Microsoft.Resources/subscriptions/resources/read","Microsoft.Resources/subscriptions/resourceGroups/read","Microsoft.Resources/subscriptions/resourceGroups/resources/read","Microsoft.Resources/subscriptions/resourceGroups/deployments/*","Microsoft.Insights/alertRules/*","Microsoft.Support/*"],"notActions":[]}]},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/312a565d-c81f-4fd8-895a-4e21e48d571c","type":"Microsoft.Authorization/roleDefinitions","name":"312a565d-c81f-4fd8-895a-4e21e48d571c"},{"properties":{"roleName":"Application Insights Component Contributor","type":"BuiltInRole","description":"Lets you manage Application Insights components, but not access to them.","scope":"/","permissions":[{"actions":["Microsoft.Insights/components/*","Microsoft.Insights/webtests/*","Microsoft.Authorization/*/read","Microsoft.Resources/subscriptions/resources/read","Microsoft.Resources/subscriptions/resourceGroups/read","Microsoft.Resources/subscriptions/resourceGroups/resources/read","Microsoft.Resources/subscriptions/resourceGroups/deployments/*","Microsoft.Insights/alertRules/*","Microsoft.Support/*"],"notActions":[]}]},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/ae349356-3a1b-4a5e-921d-050484c6347e","type":"Microsoft.Authorization/roleDefinitions","name":"ae349356-3a1b-4a5e-921d-050484c6347e"}]}

Ne morate pozvati ovaj API pridonositi. Nakon što ste određuje poznati GUID definicije uloge, možete stvoriti id definicije uloga kao:

    /subscriptions/{subscription_id}/providers/Microsoft.Authorization/roleDefinitions/{well-known-role-guid}

Evo poznati GUID od najčešće korištenih ugrađene uloga:

| Uloga | GUID |
| ----- | ------ |
| Čitač | acdd72a7-3385-48ef-bd42-f606fba81ae7
| Suradnik | b24988ac-6180-42a0-ab88-20f7382dd24c
| Suradnik virtualnog računala | d73bb868-a0df-4d4d-bd69-98a00b01fccb
| Virtualna mreža suradnika | b34d265f-36f7-4a0d-a4d4-e158ca92e90f
| Prostor za pohranu računa suradnika | 86e8f5dc-a6e9-4c67-9d15-de283e8eac25
| Suradnik web-mjesta | de139f84-1756-47ae-9be6-808fbbe84772
| Suradnik Plan za web | 2cc479cb-7b4d-49a8-b449-8c00fd0f0a4b
| SQL Server suradnika | 6d8ee4ec-f05a-4a1d-8b00-a9b17e38b437
| SQL DB suradnika | 9b7fa17d-e63e-47b0-bb0a-15c516ac86ec

### <a name="assign-rbac-role-to-application"></a>Dodijeli ulogu RBAC aplikacije

Imate sve što trebate dodijeliti odgovarajuću ulogu RBAC u funkcioniranju servisa glavni pomoću s [resursima stvaranje Dodjela uloge](https://msdn.microsoft.com/library/azure/dn906887.aspx) API-JA.

Način [GrantRoleToServicePrincipalOnSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L170) uzorka aplikacije ASP.net MVC implementira ovog poziva.

Primjer zahtjev za dodjelu uloga RBAC aplikacije: 

    PUT https://management.azure.com/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/microsoft.authorization/roleassignments/4f87261d-2816-465d-8311-70a27558df4c?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV1QiL*****FlwO1mM7Cw6JWtfY2lGc5
    Content-Type: application/json
    Content-Length: 230

    {"properties": {"roleDefinitionId":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7","principalId":"c3097b31-7309-4c59-b4e3-770f8406bad2"}}

U zahtjevu za koriste sljedeće vrijednosti:

| GUID | Opis |
| ------ | --------- |
| 09cbd307-aa71-4aca-b346-5f253e6e3ebb | id pretplate
| c3097b31-7309-4c59-b4e3-770f8406bad2 | id objekta servisa glavnice aplikacije
| acdd72a7-3385-48ef-bd42-f606fba81ae7 | id ulozi čitač
| 4f87261d-2816-465d-8311-70a27558df4c | Novi guid stvorene za Dodjela nove uloge

Odgovor je u sljedećem obliku: 

    HTTP/1.1 201 Created

    {"properties":{"roleDefinitionId":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7","principalId":"c3097b31-7309-4c59-b4e3-770f8406bad2","scope":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb"},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleAssignments/4f87261d-2816-465d-8311-70a27558df4c","type":"Microsoft.Authorization/roleAssignments","name":"4f87261d-2816-465d-8311-70a27558df4c"}

### <a name="get-app-only-access-token-for-azure-resource-manager"></a>Pristup samo za aplikacije tokena za Azure Voditelj resursa

Da biste provjerili valjanost te aplikacije sadrži željeni pristupiti na pretplatu, zadatak test pretplate pomoću programa token samo za aplikacije.

Da biste dobili tokena samo za aplikaciju programa access, slijedite upute iz odjeljka [pristupiti samo za aplikacije tokena za Azure AD grafikonu API-JA](#app-azure-ad-graph), s drukčijom vrijednošću za parametar resursa: 

    https://management.core.windows.net/

Način [ServicePrincipalHasReadAccessToSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L110) primjer aplikacije ASP.NET MVC može vidjeti samo za aplikaciju programa access tokena za Azure Voditelj resursa pomoću za provjeru autentičnosti biblioteke imenika Active Directory za .net.

#### <a name="get-applications-permissions-on-subscription"></a>Dohvaćanje aplikacije dozvola na pretplatu

Da biste provjerili ima li vaša aplikacija željeni pristup Azure pretplate na, možda poziva i [Resursima dozvole](https://msdn.microsoft.com/library/azure/dn906889.aspx) API-JA. Taj se način je slična kako odrediti ima li korisnik pristup upravljanja pravima za pretplatu. Međutim, ovaj put nazovite dozvole API-JA s token pristup samo za aplikaciju koju ste primili u prethodnom koraku.

Način [ServicePrincipalHasReadAccessToSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L110) uzorka aplikacije ASP.NET MVC implementira ovog poziva.

## <a name="manage-connected-subscriptions"></a>Upravljanje povezanim pretplate

Odgovarajuću ulogu RBAC dodjele usluzi vaše aplikacije glavni u sklopu pretplate aplikacija možete nadzor/upravljati korištenjem tokeni pristup samo za aplikacije za Azure Voditelj resursa.

Ako vlasnik pretplate uklanja Dodjela uloge vaše aplikacije pomoću klasičnog portal ili alati naredbenog retka, aplikacije više se ne mogu pristupiti tom pretplate. U tom slučaju trebali biste obavještavaju korisnika da je veza s pretplatom severed iz izvan aplikacije i dati mogućnost "popravak" veze. "Popravak" bi jednostavno ponovno stvoriti Dodjela uloge koji je izbrisan je izvan mreže.

Baš kao i omogućen korisnika da biste se povezali pretplate u aplikaciji, morate omogućiti korisniku previše prekinuti pretplate. Iz programa access upravljanje točku prikaza, prekinuti znači uklanjanje Dodjela uloga koja sadrži aplikacije servisa glavnicu na pretplatu. Po želji, sve stanja u aplikaciji za pretplatu možda će biti uklonjene previše. Samo korisnici s dozvolom pristupa upravljanje u sklopu pretplate mogu prekinuti vezu s pretplatom.

[Način RevokeRoleFromServicePrincipalOnSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L200) aplikaciju uzorka ASP.net MVC implementira ovog poziva.

To je – korisnici mogu sada jednostavno povezati i upravljanje njihove Azure pretplate s aplikacijom.

