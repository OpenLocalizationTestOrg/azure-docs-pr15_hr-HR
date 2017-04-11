<properties
    pageTitle="Azure Active Directory B2C: Korištenje grafikonu API | Microsoft Azure"
    description="Upute za pozivanje grafikonu API-JA za klijent B2C korištenjem identitet aplikacije da biste automatizirali proces."
    services="active-directory-b2c"
    documentationCenter=".net"
    authors="dstrockis"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/30/2016"
    ms.author="dastrock"/>

# <a name="azure-ad-b2c-use-the-graph-api"></a>Azure AD B2C: Korištenje grafikonu API-JA

Azure Active Directory (Azure AD) B2C klijenata često su vrlo velike. To znači da moraju izvršiti programski brojne uobičajene zadatke upravljanja klijentu. Primjer s primarni je upravljanje korisnicima. Bilo bi dobro da biste migrirali postojećeg korisnika iz trgovine B2C klijent. Trebali biste hostira Registracija korisnika na vlastitu stranicu i stvaranje korisničkih računa u Azure AD u pozadini. Te vrste zadataka potrebna mogućnost stvaranje, čitanje, ažuriranje i brisanje korisničke račune. To možete učiniti sljedeće zadatke pomoću API Azure AD grafikonu.

Za B2C klijenata, postoje dva načina rada primarni komunikaciju s API-jem grafikonu.

- Za interaktivne, jednokratno izvođenje zadatke, trebali biste poslužiti kao administratorski račun u klijentu B2C prilikom izvršavanja zadataka. Ovaj način zahtijeva administratora da se prijavi pomoću vjerodajnica da bi taj administrator možete izvršavati sve pozive na grafikonu API-JA.
- Za automatiziranog, neprekinuti zadatke, poslužite se neke vrste računa servisa pružiti potrebne ovlasti za izvođenje zadataka upravljanja. U Azure AD to možete učiniti Registracija aplikacije i provjeru autentičnosti Azure AD. To možete učiniti pomoću **ID aplikacije** koja koristi [vjerodajnice klijent OAuth 2.0 dodijeliti](../active-directory/active-directory-authentication-scenarios.md#daemon-or-server-application-to-web-api). U ovom slučaju aplikacije djeluje kao sam ne kao korisnik, da biste uputili poziv API grafikonu.

U ovom se članku smo ćete navode kako izvesti predmet automatski koristi. Da bismo pokazali, bismo ćete stvoriti .NET 4,5 `B2CGraphClient` koji se izvodi korisnik stvaranje, čitanje, ažuriranje i brisanje operacije (CRUD). Klijent neće imati Windows sučeljem naredbenog retka (EŽA) koja omogućuje pozivanje različite metode. Međutim, kod zapisuje ponašati neinteraktivne, automatiziranog način.

## <a name="get-an-azure-ad-b2c-tenant"></a>Početak klijent za Azure AD B2C

Prije stvaranja aplikacije ili korisnika ili uopće interakciju s Azure AD, trebat će klijent za Azure AD B2C i globalni administrator računa u klijentu. Ako nemate klijent već, [Početak rada s Azure AD B2C](active-directory-b2c-get-started.md).

## <a name="register-a-service-application-in-your-tenant"></a>Registracija aplikacije servisa na klijentu

Nakon što ste B2C klijent, potrebnih za stvaranje servisne aplikacije pomoću cmdleta ljuske PowerShell za Azure AD.
Najprije preuzmite i instalirajte na [Microsoft Online Services prijavu pomoćnika](http://go.microsoft.com/fwlink/?LinkID=286152). Zatim preuzeti i instalirati [64-bitni modul Azure Active Directory za Windows PowerShell](http://go.microsoft.com/fwlink/p/?linkid=236297).

> [AZURE.IMPORTANT]
Da biste koristili API grafikonu s klijentom B2C, morate registrirati namjenski aplikacije pomoću komponente PowerShell. Slijedite upute u ovom članku da to učinite. Nije moguće ponovno koristiti aplikacije B2C već postojeće koje ste registrirali na portalu za Azure.

Kada instalirate modul ljuske PowerShell sustava, otvorite ljusku PowerShell i povezati se B2C klijentu. Nakon pokretanja `Get-Credential`, pojavit će se upit za korisničko ime i lozinku, unesite korisničko ime i lozinku računa B2C klijentskog administratora.

```
> $msolcred = Get-Credential
> Connect-MsolService -credential $msolcred
```

Prije nego što stvorite aplikaciju, morate stvoriti novi **klijent tajna**.  Aplikacija će koristiti tajna klijenta za provjeru autentičnosti Azure AD i dobiti pristup tokena. Možete stvoriti valjani tajna PowerShell:

```
> $bytes = New-Object Byte[] 32
> $rand = [System.Security.Cryptography.RandomNumberGenerator]::Create()
> $rand.GetBytes($bytes)
> $rand.Dispose()
> $newClientSecret = [System.Convert]::ToBase64String($bytes)
> $newClientSecret
```

Naredba konačan treba ispisati svoje nove tajna klijent. Kopirajte vezu negdje gdje sigurni. Morat ćete ga kasnije. Sada možete stvoriti aplikaciju unosom novi klijent tajna kao vjerodajnice za aplikaciju:

```
> New-MsolServicePrincipal -DisplayName "My New B2C Graph API App" -Type password -Value $newClientSecret

DisplayName           : My New B2C Graph API App
ServicePrincipalNames : {dd02c40f-1325-46c2-a118-4659db8a55d5}
ObjectId              : e2bde258-6691-410b-879c-b1f88d9ef664
AppPrincipalId        : dd02c40f-1325-46c2-a118-4659db8a55d5
TrustedForDelegation  : False
AccountEnabled        : True
Addresses             : {}
KeyType               : Password
KeyId                 : a261e39d-953e-4d6a-8d70-1f915e054ef9
StartDate             : 9/2/2015 1:33:09 AM
EndDate               : 9/2/2016 1:33:09 AM
Usage                 : Verify
```

Ako uspješno stvorite aplikaciju, trebali biste ispis svojstava aplikacije kao što su iznad njih. Potreban vam je i `ObjectId` i `AppPrincipalId`, pa je kopirajte te se vrijednosti.

Kada stvorite aplikaciju na klijentu B2C, morate dodijeliti dozvole potrebne za izvođenje operacija CRUD korisnika. Dodijelite uloge tri aplikacije: direktorij čitateljima (korisnika za čitanje), direktorija autorima (Stvaranje i ažuriranje korisnika), i administratora za račun korisnika (za brisanje korisnika). Te uloge imaju poznati identifikatora tako da možete zamijeniti u `-RoleMemberObjectId` parametar s `ObjectId` odozgo i pokrenite sljedeće naredbe. Da biste vidjeli popis svih uloga direktorija, pokušajte pokrenuti `Get-MsolRole`.

```
> Add-MsolRoleMember -RoleObjectId 88d8e3e3-8f55-4a1e-953a-9b9898b8876b -RoleMemberObjectId <Your-ObjectId> -RoleMemberType servicePrincipal
> Add-MsolRoleMember -RoleObjectId 9360feb5-f418-4baa-8175-e2a00bac4301 -RoleMemberObjectId <Your-ObjectId> -RoleMemberType servicePrincipal
> Add-MsolRoleMember -RoleObjectId fe930be7-5e62-47db-91af-98c3a49a38b1 -RoleMemberObjectId <Your-ObjectId> -RoleMemberType servicePrincipal
```

Možete sada imate program koji imaju dozvolu za stvaranje, čitanje, ažuriranje i brisanje korisnika iz klijenta sustava B2C.

## <a name="download-configure-and-build-the-sample-code"></a>Preuzimanje, konfiguriranje i sastavljanje uzorak koda

Najprije preuzmite uzorak koda i obavite pokrenut. Zatim ćemo se detaljnije na njega.  Možete [preuzeti uzorak koda kao .zip datoteku](https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet/archive/master.zip). Mogu Kloniraj i ga pretvoriti u imenik izboru:

```
git clone https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet.git
```

Otvaranje u `B2CGraphClient\B2CGraphClient.sln` Visual Studio rješenja u Visual Studio. U na `B2CGraphClient` projekta, otvorite datoteku `App.config`. Zamijeni postavki tri aplikacije vlastitih vrijednosti:

```
<appSettings>
    <add key="b2c:Tenant" value="contosob2c.onmicrosoft.com" />
    <add key="b2c:ClientId" value="{The AppPrincipalId from above}" />
    <add key="b2c:ClientSecret" value="{The client secret you generated above}" />
</appSettings>
```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

Nakon toga desnom tipkom miša kliknite na `B2CGraphClient` rješenje i ponovno stvaranje uzorka. Ako ste uspješan, trebala bi se pojaviti na `B2C.exe` izvršne datoteke koja se nalazi u `B2CGraphClient\bin\Debug`.

## <a name="build-user-crud-operations-by-using-the-graph-api"></a>Izrada korisničkog CRUD operacija korištenjem API grafikonu

Da biste koristili u B2CGraphClient, otvorite je `cmd` Windows naredba zatraži i promjena direktorija da biste na `Debug` direktorija. Pokrenite na `B2C Help` naredbe.

```
> cd B2CGraphClient\bin\Debug
> B2C Help
```

Time će se prikazati i kratak opis svaku naredbu. Svaki put kada jednu od ovih naredbi pozivanje `B2CGraphClient` unese zahtjeva za Azure AD grafikonu API-JA.

### <a name="get-an-access-token"></a>Početak token za pristup

Bilo koji zahtjev za API grafikonu zahtijeva token za pristup za provjeru autentičnosti. `B2CGraphClient`koristi u Otvori izvor Active Directory provjera autentičnosti biblioteke (ADAL) da bi dobiti pristup tokena. ADAL olakšava tokena acquisition tako da omogućuje jednostavno API-JA i pobrinite važne pojedinosti, kao što su predmemoriranje tokeni programa access. Ne morate koristiti ADAL da biste dobili tokena, no. Tokeni možete otvoriti i tako da obratite HTTP zahtjeva.

> [AZURE.NOTE]
    Ovaj uzorak koda koristi ADAL v2 da bi se komunicirati s API grafikonu.  Morate koristiti ADAL v2 ili v3 da biste dobili pristup tokeni koje mogu koristiti u sklopu Azure AD grafikonu API-JA.

Kada `B2CGraphClient` pokrene, stvara instancu na `B2CGraphClient` predmete. Graditelj klase postavlja se scaffolding ADAL provjere autentičnosti:

```C#
public B2CGraphClient(string clientId, string clientSecret, string tenant)
{
    // The client_id, client_secret, and tenant are provided in Program.cs, which pulls the values from App.config
    this.clientId = clientId;
    this.clientSecret = clientSecret;
    this.tenant = tenant;

    // The AuthenticationContext is ADAL's primary class, in which you indicate the tenant to use.
    this.authContext = new AuthenticationContext("https://login.microsoftonline.com/" + tenant);

    // The ClientCredential is where you pass in your client_id and client_secret, which are
    // provided to Azure AD in order to receive an access_token by using the app's identity.
    this.credential = new ClientCredential(clientId, clientSecret);
}
```

Ćemo koristiti u `B2C Get-User` naredbe kao primjer. Kada `B2C Get-User` se poziva bez sve dodatne unose EŽA pozive u `B2CGraphClient.GetAllUsers(...)` način. Ta metoda poziva `B2CGraphClient.SendGraphGetRequest(...)`, koja se šalje zahtjev HTTP GET za API grafikonu. Prije nego što `B2CGraphClient.SendGraphGetRequest(...)` šalje DOHVATI zahtjev, je najprije dobiti programa access tokena pomoću ADAL:

```C#
public async Task<string> SendGraphGetRequest(string api, string query)
{
    // First, use ADAL to acquire a token by using the app's identity (the credential)
    // The first parameter is the resource we want an access_token for; in this case, the Graph API.
    AuthenticationResult result = authContext.AcquireToken("https://graph.windows.net", credential);

    ...

```

Možete pristupiti na tokena za API grafikonu tako da nazovete podršku za ADAL `AuthenticationContext.AcquireToken(...)` način. ADAL vraća programa `access_token` koja predstavlja identitet aplikacije.

### <a name="read-users"></a>Čitanje korisnika

Ako želite da biste dobili popis korisnika ili se određeni korisnički iz API grafikonu, možete poslati na HTTP `GET` zatražiti na `/users` krajnjoj točki. Zahtjev za sve korisnike u klijent izgleda ovako:

```
GET https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

Da biste vidjeli taj zahtjev, pokrenite:

 ```
 > B2C Get-User
 ```

Postoje dvije važne stvari koje možete Imajte na umu:

- Pristupni token dobivene putem ADAL dodaje se na `Authorization` zaglavlje pomoću na `Bearer` shemu.
- Za B2C korisnicima, morate koristiti parametar upita `api-version=1.6`.

Obje te detalje rukuje na `B2CGraphClient.SendGraphGetRequest(...)` metoda:

```C#
public async Task<string> SendGraphGetRequest(string api, string query)
{
    ...

    // For B2C user management, be sure to use the 1.6 Graph API version.
    HttpClient http = new HttpClient();
    string url = "https://graph.windows.net/" + tenant + api + "?" + "api-version=1.6";
    if (!string.IsNullOrEmpty(query))
    {
        url += "&" + query;
    }

    // Append the access token for the Graph API to the Authorization header of the request by using the Bearer scheme.
    HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, url);
    request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
    HttpResponseMessage response = await http.SendAsync(request);

    ...
```

### <a name="create-consumer-user-accounts"></a>Stvaranje potrošača korisničkih računa

Prilikom stvaranja korisničkih računa u klijentu B2C, možete poslati na HTTP `POST` zatražiti na `/users` krajnja točka:

```
POST https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 338

{
    // All of these properties are required to create consumer users.

    "accountEnabled": true,
    "signInNames": [                            // controls which identifier the user uses to sign in to the account
        {
            "type": "emailAddress",             // can be 'emailAddress' or 'userName'
            "value": "joeconsumer@gmail.com"
        }
    ],
    "creationType": "LocalAccount",            // always set to 'LocalAccount'
    "displayName": "Joe Consumer",              // a value that can be used for displaying to the end user
    "mailNickname": "joec",                     // an email alias for the user
    "passwordProfile": {
        "password": "P@ssword!",
        "forceChangePasswordNextLogin": false   // always set to false
    },
    "passwordPolicies": "DisablePasswordExpiration"
}
```

Da biste stvorili korisničke korisnici moraju većinu tih svojstava u zahtjev. Da biste saznali više, kliknite [ovdje](https://msdn.microsoft.com/library/azure/ad/graph/api/users-operations#CreateLocalAccountUser). Imajte na umu da se `//` Komentari su dodani slici. Nemojte uvrstiti ih u zahtjev za stvarni.

Da biste vidjeli zahtjev, pokrenite jedan od sljedećih naredbi:

```
> B2C Create-User ..\..\..\usertemplate-email.json
> B2C Create-User ..\..\..\usertemplate-username.json
```

Na `Create-User` naredba vodi .json datoteke kao ulazni parametar. Sadrži prikaz JSON korisničkom objektu. U primjeru koda postoje dvije ogledne datoteke .json: `usertemplate-email.json` i `usertemplate-username.json`. Možete izmijeniti te datoteke u skladu s potrebama. Osim obavezna polja iznad, nekoliko neobavezna polja koje možete koristiti uključeni su u te datoteke. Detalje o neobavezna polja pronaći ćete u [grafikonu Azure AD API entitet reference](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity).

Možete vidjeti kako će se zahtjev za objavu konstruirana u `B2CGraphClient.SendGraphPostRequest(...)`.

- Ga pridružuje token za pristup da biste na `Authorization` zaglavlje zahtjev.
- Postavlja `api-version=1.6`.
- U tijelu zahtjeva za uključuje JSON korisničkom objektu.

> [AZURE.NOTE]
Ako je račune koji želite migrirati iz postojećeg korisnika trgovine donjem snage lozinke od [granica lomljenja jaku lozinku postavio Azure AD B2C](https://msdn.microsoft.com/library/azure/jj943764.aspx), možete onemogućiti pomoću zahtjeva jaku lozinku na `DisableStrongPassword` vrijednosti u na `passwordPolicies` svojstvo. Na primjer, možete izmijeniti zahtjev za stvaranje korisnički navedene iznad na sljedeći način: `"passwordPolicies": "DisablePasswordExpiration, DisableStrongPassword"`.

### <a name="update-consumer-user-accounts"></a>Ažurirajte korisničke korisničkih računa

Kada ažurirate korisničke objekte, postupak je sličan onome koji koristite za stvaranje korisnički objekti. No taj proces koristi HTTP `PATCH` metoda:

```
PATCH https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 37

{
    "displayName": "Joe Consumer",              // this request updates only the user's displayName
}
```

Pokušajte ažurirati korisnik ažuriranjem datotekama JSON novim podacima. Možete koristiti `B2CGraphClient` da biste pokrenuli jednu od ove naredbe:

```
> B2C Update-User <user-object-id> ..\..\..\usertemplate-email.json
> B2C Update-User <user-object-id> ..\..\..\usertemplate-username.json
```

Provjera na `B2CGraphClient.SendGraphPatchRequest(...)` način pojedinosti o slanju zahtjev.

### <a name="search-users"></a>Traženje korisnika

Korisnicima možete potražiti u klijentu B2C na nekoliko načina. Jedan pomoću korisnika objekt ID ili dvije, pomoću korisnikove prijave identifikatorom (odnosno u `signInNames` svojstvo).

Pokrenite sljedeće naredbe za pretraživanje za određenog korisnika:

```
> B2C Get-User <user-object-id>
> B2C Get-User <filter-query-expression>
```

Evo nekoliko primjera:

```
> B2C Get-User 2bcf1067-90b6-4253-9991-7f16449c2d91
> B2C Get-User $filter=signInNames/any(x:x/value%20eq%20%27joeconsumer@gmail.com%27)
```

### <a name="delete-users"></a>Brisanje korisnika

Postupak za brisanje korisnika je jasan. Korištenje HTTP `DELETE` metodu i konstrukta URL-a s točno ID objekta:

```
DELETE https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

Da biste vidjeli primjer, unesite sljedeću naredbu, a zatim prikažite zahtjev za brisanje koje se ispisuje na konzolu:

```
> B2C Delete-User <object-id-of-user>
```

Provjera na `B2CGraphClient.SendGraphDeleteRequest(...)` način pojedinosti o slanju zahtjev.

Možete izvršiti mnoge druge akcije s API-jem Azure AD grafikonu uz upravljanje korisnicima. [Azure AD grafikonu API reference](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) sadrži detalje za svaku akciju, zajedno s zahtjeva za uzorak.

## <a name="use-custom-attributes"></a>Korištenje prilagođene atribute

Većina potrošača aplikacija potrebno da biste pohranili neke vrste podataka korisničkih profila. Jedan to možete učiniti tako da biste odredili prilagođeni atribut u klijentu B2C. Zatim možete tretirati atribut na isti način kao i druga svojstva obradi na korisničkom objektu. Možete ažurirati atribut, brisanje atribut, upit tako da atribut, pošaljite atribut kao zahtjev u tokeni za prijavu i drugo.

Da biste odredili prilagođeni atribut u klijentu B2C, potražite u članku [Referenca za prilagođeni atribut B2C](active-directory-b2c-reference-custom-attr.md).

Možete pogledati prilagođene atribute definirane u klijentu B2C pomoću `B2CGraphClient`:

```
> B2C Get-B2C-Application
> B2C Get-Extension-Attribute <object-id-in-the-output-of-the-above-command>
```

Izlaz iz ove funkcije otkriva pojedinosti svake prilagođeni atribut, kao što su:

```JSON
{
      "odata.type": "Microsoft.DirectoryServices.ExtensionProperty",
      "objectType": "ExtensionProperty",
      "objectId": "cec6391b-204d-42fe-8f7c-89c2b1964fca",
      "deletionTimestamp": null,
      "appDisplayName": "",
      "name": "extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number",
      "dataType": "Integer",
      "isSyncedFromOnPremises": false,
      "targetObjects": [
        "User"
      ]
}
```

Koristite puno ime, kao što su `extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number`, kao svojstvo na korisničke objekte.  Ažuriranje .json datoteke s novo svojstvo i vrijednosti za to svojstvo, a zatim pokrenite:

```
> B2C Update-User <object-id-of-user> <path-to-json-file>
```

Pomoću `B2CGraphClient`, imate servisne aplikacije koje možete upravljati programski B2C korisnika klijentu. `B2CGraphClient`koristi vlastitu identitet aplikacije za provjeru autentičnosti API Azure AD grafikonu. Tokeni ga nabaviti i pomoću klijenta tajna. Kao što je ta je funkcija ugraditi u aplikaciji, imajte na umu nekoliko ključne točke za B2C aplikacije:

- Trebate dodijeliti aplikacije odgovarajuće dozvole u klijentu.
- Zasad, morate koristiti ADAL v2 da biste dobili pristup tokena. (Možete slati poruke protokol izravno bez korištenja biblioteci.)
- Kada poziv API grafikonu, koristite `api-version=1.6`.
- Kada stvorite i ažuriranje potrošača korisnika, nekoliko svojstava su potrebni, prethodno opisan.

Ako imate pitanja ili zahtjeve za akcije koje želite izvesti pomoću API grafikonu na klijentu B2C, ostavite komentar na članak ili je spremite problem u spremištu uzorka GitHub kod.
