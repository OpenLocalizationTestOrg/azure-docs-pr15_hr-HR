<properties
    pageTitle="Poziv prilagođene API-JA u aplikacijama logike"
    description="Pomoću svoje prilagođene API hostirane na servisu aplikacije s aplikacijama logike"
    authors="stepsic-microsoft-com"
    manager="dwrede"
    editor=""
    services="logic-apps"
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/31/2016"
    ms.author="stepsic"/>

# <a name="using-your-custom-api-hosted-on-app-service-with-logic-apps"></a>Pomoću svoje prilagođene API hostirane na servisu aplikacije s aplikacijama logike

Iako logike aplikacije ima bogatog skupa 40 + poveznici za različite servise, preporučujemo vam da biste uputili poziv u vlastitu prilagođenu API koji se može izvoditi vlastitog koda. Najjednostavniji i najčešće skalabilni načina za hostiranje vlastite prilagođene API na webu, je za korištenje aplikacije servisa. U ovom se članku opisuje kako poziva u bilo kojem web API smješten u API aplikacije servisa za aplikaciju, web-aplikacije ili mobilnoj aplikaciji.

Informacije o izgradnji API-ji kao okidača ili akcija unutar logike aplikacije, pogledajte [ovog članka](app-service-logic-create-api-app.md).

## <a name="deploy-your-web-app"></a>Implementacija Web App

Najprije morate za implementaciju sustava API kao web-aplikacijama u aplikacije servisa. Ovdje navedene upute pokriva osnovni implementacije: [Stvaranje web-aplikacije programa ASP.NET](../app-service-web/web-sites-dotnet-get-started.md).  Dok je u bilo kojem API-JA možete pozvati iz logike aplikacije, za najbolje mogućnosti rada preporučujemo da dodate Swagger metapodataka jednostavno Integracija s akcijama logike aplikacije.  Detalje potražite na [Dodavanje swagger](../app-service-api/app-service-api-dotnet-get-started.md#use-swagger-api-metadata-and-ui).

### <a name="api-settings"></a>Postavke API-JA

Kako bi designer aplikacije logike raščlaniti vaše Swagger, važno omogućiti CORS i postaviti svojstva APIDefinition web-aplikacije je.  Ovo je vrlo lako postavljene unutar portala za Azure.  Jednostavno otvorite plohu postavke web-aplikacije, u odjeljku API postavili 'API definiciju' URL datoteke swagger.json (to je obično https://{name}.azurewebsites.net/swagger/docs/v1) i dodavanje CORS pravila za "*" da biste omogućili zahtjeva iz aplikacija logike dizajner za.

## <a name="calling-into-the-api"></a>Pozivanje u na API-JA

Unutar portal za aplikacije logike, ako ste postavili CORS, a zatim svojstva API definicija moraju ćete moći jednostavno dodajte API prilagođene akcije unutar vaše tijek.  U dizajneru možete odabrati da biste pregledali pretplate web-mjesta za prikaz popisa na web-mjesta s URL-om swagger na definirani.  Možete koristiti i HTTP + Swagger akcije da pokažete na swagger dostupne akcije i unosa.  Na kraju, uvijek možete stvoriti zahtjev akcija HTTP poziva sve API-JA, čak i one koje su ili izložiti swagger dokument.

Ako želite radi zaštite vaše API-JA, zatim postoje nekoliko različitih načina da biste to učinili:

1. Nije kod potrebna promjena - Azure Active Directory može se koristiti radi zaštite vaše API bez promjene kod niti ponovno uvođenje.
1. Nametanje osnovne provjere autentičnosti, AAD provjere autentičnosti ili provjera autentičnosti certifikata u kodu vaše API-JA.

## <a name="securing-calls-to-your-api-without-a-code-change"></a>Zaštita pozive API-JA bez promjene kod

U ovom ćete odjeljku stvorit ćete dva Azure Active Directory aplikacije – jedan logike aplikacije i jedan za web-aplikacije.  Ćete autentičnost pozive na web-aplikaciju pomoću upravitelja servisa (id klijenta i tajna) povezan s aplikacijom AAD logike aplikacije. Na kraju, obuhvatili aplikacije pošti u definicije logike aplikacije.

### <a name="part-1-setting-up-an-application-identity-for-your-logic-app"></a>Dio 1: Postavljanje identitet aplikacije logike aplikacije

Ovo je što aplikaciju logike koristi za provjeru autentičnosti servisa active directory. Samo *potrebno* je učiniti jednom za direktorija. Na primjer, možete koristiti isti identiteta za sve aplikacije logike, iako jedinstveni identiteta po logike aplikacije mogu stvoriti i ako želite. Možete učiniti u korisničkog Sučelja ili pomoću komponente PowerShell.

#### <a name="create-the-application-identity-using-the-azure-classic-portal"></a>Stvaranje identitet aplikacije pomoću portala za Azure klasični

1. Dođite do [servisa Active directory na portalu za Azure klasični](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory) i odaberite direktorija koji koristite za web-aplikacije
2. Kliknite karticu **aplikacije**
3. Kliknite **Dodaj** na naredbenoj traci pri dnu stranice
4. Naziv vašeg identiteta da biste koristili, kliknite strelicu za sljedeće
5. Stavite u jedinstveni niz oblikovan kao domena u dvije tekstne okvire i kliknite kvačicu
6. Kliknite karticu **Konfiguracija** za ovu aplikaciju
7. Kopiranje **ID klijenta**, to će se koristiti u aplikaciji logike
8. U odjeljku **tipke** kliknite **Odaberite trajanje** i odaberite godinu ili 2 godina
9. Kliknite gumb **Spremi** pri dnu zaslona (možda ćete morati pričekati nekoliko sekundi)
10. Sada obavezno da biste kopirali tipku u okvir. To koristit će se u aplikaciji logike

#### <a name="create-the-application-identity-using-powershell"></a>Stvaranje identitet aplikacije pomoću komponente PowerShell
1. `Switch-AzureMode AzureResourceManager`
2. `Add-AzureAccount`
3. `New-AzureADApplication -DisplayName "MyLogicAppID" -HomePage "http://someranddomain.tld" -IdentifierUris "http://someranddomain.tld" -Password "Pass@word1!"`
4. Ne zaboravite da biste kopirali **ID klijenta**, **ID aplikacije** i lozinka koje ste koristili

### <a name="part-2-protect-your-web-app-with-an-aad-app-identity"></a>Dio 2: Zaštita Web App s identitetom AAD aplikacije

Ako je već uvedena Web app samo je možete omogućiti na portalu. U suprotnom, možete napraviti Omogućivanje autorizacije dio uvođenja Upravitelj Azure resursa.

#### <a name="enable-authorization-in-the-azure-portal"></a>Omogući provjeru autentičnosti na portalu za Azure

1. Dođite do web-aplikaciju i kliknite **Postavke** na naredbenoj traci.
2. Kliknite **Autorizacija/provjere autentičnosti**.
3. **Uključiti.**

Sada aplikacije automatski se stvara za vas. Potrebne su vam ID klijenta za ovu aplikaciju za dio 3, pa ćete morati:

1. Idite [na portalu za Azure klasični servisa Active directory](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory) i odaberite direktorija.
2. Traženje aplikacije u okvir za pretraživanje
3. Kliknite je na popisu
4. Kliknite karticu **Konfiguracija**
5. Trebali biste vidjeti **ID klijenta**

#### <a name="deploying-your-web-app-using-an-arm-template"></a>Implementacija web-aplikaciju programa pomoću predloška ARM

Najprije morate stvoriti aplikaciju za web-aplikacije. Mora biti razlikuje od aplikacije koji se koristi za logike aplikacije. Najprije slijedite korake iznad u dijelu 1, ali sada za **početnu stranicu** i **IdentifierUris** stvarni https://**URL** web-aplikacije.

>[AZURE.NOTE]Kada stvorite aplikaciju za web-aplikacije, morate koristiti [Azure klasični portala pristup](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory)kao cmdleta ljuske PowerShell postavite potrebne dozvole za prijavu korisnika na web-mjesto.

Kada imate klijent ID i ID klijenta uključuju sljedeće kao resurs sub web-aplikacije u predlošku implementacije:

```
"resources" : [
    {
        "apiVersion" : "2015-08-01",
        "name" : "web",
        "type" : "config",
        "dependsOn" : [
            "[concat('Microsoft.Web/sites/','parameters('webAppName'))]"
        ],
        "properties" : {
            "siteAuthEnabled": true,
            "siteAuthSettings": {
              "clientId": "<<clientID>>",
              "issuer": "https://sts.windows.net/<<tenantID>>/",
            }
        }
    }
]
```

Da biste pokrenuli implementacije automatski koja uvodi prazno Web app i logike aplikacije zajedno koji koriste AAD, kliknite gumb sljedeće:

[![Implementacija Azure](./media/app-service-logic-custom-hosted-api/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-logic-app-custom-api%2Fazuredeploy.json)

Dovršavanje predloška potražite u članku [logike aplikacije poziva u prilagođeni API -JA hostirane na servisu aplikacije i zaštićen AAD](https://github.com/Azure/azure-quickstart-templates/blob/master/201-logic-app-custom-api/azuredeploy.json).


### <a name="part-3-populate-the-authorization-section-in-the-logic-app"></a>Dio 3: Popunjavanje autorizacije sekciju u aplikaciji logike

U odjeljku **autorizacije** **HTTP** akcija:`{"tenant":"<<tenantId>>", "audience":"<<clientID from Part 2>>", "clientId":"<<clientID from Part 1>>","secret": "<<Password or Key from Part 1>>","type":"ActiveDirectoryOAuth" }`

| Element | Opis |
|---------|-------------|
| Vrsta | Vrsta provjere autentičnosti. Za provjeru autentičnosti ActiveDirectoryOAuth, vrijednost je ActiveDirectoryOAuth. |
| klijent | Služi za identifikaciju klijentu za AD identifikator klijenta. |
| ciljne skupine | Obavezan. Resurs koji se povezujete. |
| clientID | Identifikator klijenta za Azure AD aplikacije. |
| tajna | Obavezan. Tajna klijenta koji zahtijeva token. |

Iznad predložak već ima to postaviti, ali ako izrađujete aplikaciju logike izravno, morat ćete uključiti puni autorizacije sekciju.

## <a name="securing-your-api-in-code"></a>Zaštita API-JA u kodu

### <a name="certificate-auth"></a>Provjera autentičnosti certifikata

Certifikati klijenta možete koristiti da biste provjerili valjanost dolaznih zahtjeva na web-aplikaciju. Pogledajte [Kako da biste konfigurirali TLS međusobna provjere autentičnosti za web-aplikaciju](../app-service-web/app-service-web-configure-tls-mutual-auth.md) za postavljanje kod.

U odjeljku *autorizacije* mora sadržavati: `{"type": "clientcertificate","password": "test","pfx": "long-pfx-key"}`.

| Element | Opis |
|---------|-------------|
| Vrsta | Obavezan. Vrsta provjere autentičnosti. Za SSL certifikate klijent, ta vrijednost mora biti ClientCertificate. |
| pfx | Obavezan. Base64 kodirani sadržaj PFX datoteke. |
| lozinke | Obavezan. Lozinka za pristup datoteci PFX. |

### <a name="basic-auth"></a>Osnovna provjera autentičnosti

Osnovna provjera autentičnosti (primjerice korisničko ime i lozinka) možete koristiti da biste provjerili valjanost dolaznih zahtjeva. Osnovna provjera autentičnosti je uobičajeni obrazac i sve možete raditi u bilo koji jezik sastavljanje aplikacije u.

U odjeljku *autorizacije* mora sadržavati: `{"type": "basic","username": "test","password": "test"}`.

| Element | Opis |
|---------|-------------|
| Vrsta | Obavezan. Vrsta provjere autentičnosti. Osnovna provjera autentičnosti ta vrijednost mora biti Basic. |
| korisničko ime | Obavezan. Korisničko ime za provjeru autentičnosti. |
| lozinke | Obavezan. Lozinka za provjeru autentičnosti. |

### <a name="handle-aad-auth-in-code"></a>Držač za provjeru autentičnosti AAD u kodu

Prema zadanim postavkama provjere autentičnosti Azure Active Directory koja omogućuju na portalu učinite preciznije autorizacije. Ako, na primjer, ga zaključavanje API određenog korisnika ili aplikacije, ali samo određeni klijenta.

Ako želite da biste ograničili na API samo logike aplikacije, na primjer, kod, možete izdvojiti zaglavlja koje sadrži JWT i provjerite tko Pozivatelj je, odbijanje zahtjeva za s koje se ne podudaraju.

Odlazak Dodatno, ako želite implementirati u potpunosti vlastitog koda, a ne pod utjecajem značajku Portal, možete u ovom se članku: [Korištenje servisa Active Directory za provjeru autentičnosti u servisu Azure aplikacije](../app-service-web/web-sites-authentication-authorization.md).

I dalje morate slijediti gore navedeni koraci za stvaranje identitet aplikacije logike aplikacije i koje koristite da biste pozvali na API-JA.
