<properties
    pageTitle="Početak rada s Azure CDN SDK za Node.js | Microsoft Azure"
    description="Saznajte kako napisati Node.js aplikacije da biste upravljali Azure CDN."
    services="cdn"
    documentationCenter="nodejs"
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/15/2016"
    ms.author="casoper"/>

# <a name="get-started-with-azure-cdn-development"></a>Početak rada s razvoj Azure CDN

> [AZURE.SELECTOR]
- [Node.js](cdn-app-dev-node.md)
- [.NET](cdn-app-dev-net.md)

[Azure CDN SDK Node.js](https://www.npmjs.com/package/azure-arm-cdn) možete koristiti da biste automatizirali stvaranje i Upravljanje profilima CDN i krajnje točke.  Pomoću ovog praktičnog vodiča vodi kroz stvaranje jednostavne aplikacije konzole Node.js koji pokazuje nekoliko dostupno operacije.  Pomoću ovog praktičnog vodiča je namijenjen opisuju svim aspektima Azure CDN SDK za Node.js detaljno.

Da biste dovršili ovaj Praktični vodič, koje treba već [Node.js](http://www.nodejs.org) **4.x.x** ili noviji instalacije i konfiguracije.  Možete koristiti bilo kojem uređivaču teksta koji želite stvoriti Node.js aplikacije.  Da biste napisali ovog praktičnog vodiča, mogu koristiti [Visual Studio kod](https://code.visualstudio.com).  

> [AZURE.TIP] [Dovršena projekta iz ovog praktičnog vodiča](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74) dostupna je za preuzimanje na MSDN-u.

[AZURE.INCLUDE [cdn-app-dev-prep](../../includes/cdn-app-dev-prep.md)]

## <a name="create-your-project-and-add-npm-dependencies"></a>Stvaranje projekta i dodavanje NPM ovisnosti

Nakon što smo ste stvorili grupu resursa za naše profile CDN i dao naš Azure AD aplikacije dozvolu za Upravljanje profilima CDN i krajnje točke u toj grupi, ne možemo mogu početi stvarati naš aplikacije.

Stvorite mapu u koju želite spremiti svoju aplikaciju.  S konzole softvera pomoću alata za Node.js u trenutni put, postavite na kojemu se nalazite na nove mape i pokretanje projekta izvršavanjem:
    
    npm init
    
Zatim primit ćete niz pitanja Inicijalizacija projekta.  **Ulazna točka**, pomoću ovog praktičnog vodiča koristi *app.js*.  Možete vidjeti moje mogućnosti u sljedećem primjeru.

![NPM init Izlaz](./media/cdn-app-dev-node/cdn-npm-init.png)

Naš projekta odmah pokrenuti s datotekom *packages.json* .  Naš projekta namjeravate koristiti neke Azure biblioteke koje se nalaze u paketima NPM.  Ćemo koristiti prilikom izvođenja klijentskog Azure za Node.js (ms-rest-azure) i Azure CDN klijentska biblioteka za Node.js (azure-arm-CD-a).  Dodat ćemo one u projekt kao ovisnosti.
 
    npm install --save ms-rest-azure
    npm install --save azure-arm-cdn

Završetku pakete instalacije *package.json* datoteke trebao bi izgledati slično kao u ovom primjeru (verzija brojeva mogu se razlikovati):

``` json
{
  "name": "cdn_node",
  "version": "1.0.0",
  "description": "Azure CDN Node.js tutorial project",
  "main": "app.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "Cam Soper",
  "license": "MIT",
  "dependencies": {
    "azure-arm-cdn": "^0.2.1",
    "ms-rest-azure": "^1.14.4"
  }
}
```

Na kraju, koristite uređivač teksta, stvorite prazne tekstne datoteke i spremite ga u korijenskoj mapi naš projekta mapu kao *app.js*.  Ne možemo sada ste spremni za početak pisanja koda.

## <a name="requires-constants-authentication-and-structure"></a>Zahtijeva, konstante, provjeru autentičnosti i strukture

S *app.js* otvorena u našem uređivač, recimo se osnovna struktura naš program napisan.

1. Dodajte na "zahtijeva" za naše pakete NPM pri vrhu sa sljedećim:

    ``` javascript
    var msRestAzure = require('ms-rest-azure');
    var cdnManagementClient = require('azure-arm-cdn');
    ```

2. Ne možemo potrebno definirati neke konstante će koristiti naše metode.  Dodajte sljedeće.  Ne zaboravite da biste zamijenili rezervirana mjesta, uključujući u ** &lt;uglate zagrade&gt;**, pomoću vlastitih vrijednosti po potrebi.

    ``` javascript
    //Tenant app constants
    const clientId = "<YOUR CLIENT ID>";
    const clientSecret = "<YOUR CLIENT AUTHENTICATION KEY>"; //Only for service principals
    const tenantId = "<YOUR TENANT ID>";

    //Application constants
    const subscriptionId = "<YOUR SUBSCRIPTION ID>";
    const resourceGroupName = "CdnConsoleTutorial";
    const resourceLocation = "<YOUR PREFERRED AZURE LOCATION, SUCH AS Central US>";
    ```

3. Zatim ćemo ćete stvoriti instancu CDN klijent za upravljanje i dajte mu naš vjerodajnice.

    ``` javascript
    var credentials = new msRestAzure.ApplicationTokenCredentials(clientId, tenantId, clientSecret);
    var cdnClient = new cdnManagementClient(credentials, subscriptionId);
    ```
    
    Ako koristite provjeru autentičnosti pojedinačnih korisnika, te dva retka izgledat će malo drugačije.

    >[AZURE.IMPORTANT] Ovaj uzorak koda koristiti samo ako odabirete da bi provjere autentičnosti pojedinačnih korisnika umjesto glavni servisa.  Pripazite da štititi pojedinačne korisničke vjerodajnice te kako ih držati tajnu.

    ``` javascript
    var credentials = new msRestAzure.UserTokenCredentials(clientId, 
        tenantId, '<username>', '<password>', '<redirect URI>');
    var cdnClient = new cdnManagementClient(credentials, subscriptionId);
    ```

    Ne zaboravite da biste zamijenili stavke u ** &lt;uglate zagrade&gt; ** s točne podatke.  Za `<redirect URI>`, koristite preusmjeravanje URI uneseni kada aplikacija registrira u Azure AD.
    

4.  Naš program konzole Node.js će potrajati nekoliko parametara naredbenog retka.  Recimo da barem je jedan Provjera valjanosti proslijeđen parametar.

    ```javascript
    //Collect command-line parameters
    var parms = process.argv.slice(2);

    //Do we have parameters?
    if(parms == null || parms.length == 0)
    {
        console.log("Not enough parameters!");
        console.log("Valid commands are list, delete, create, and purge.");
        process.exit(1);
    }
    ```

5. Nam koji se pojavljuju u glavni dio naš program koje ćemo grana druge funkcije koje parametara su preneseni na temelju.

    ```javascript
    switch(parms[0].toLowerCase())
    {
        case "list":
            cdnList();
            break;

        case "create":
            cdnCreate();
            break;
        
        case "delete":
            cdnDelete();
            break;

        case "purge":
            cdnPurge();
            break;

        default:
            console.log("Valid commands are list, delete, create, and purge.");
            process.exit(1);
    }
    ```

6.  Na nekoliko mjesta u naš program smo morat ćete provjerite je li desnom broj parametara su proslijeđen i prikazati pomoć ako ne izgleda ispravno.  Stvaranje funkcije da biste to učinili.

    ```javascript
    function requireParms(parmCount) {
        if(parms.length < parmCount) {
            usageHelp(parms[0].toLowerCase());
            process.exit(1);
        }
    }

    function usageHelp(cmd) {
        console.log("Usage for " + cmd + ":");
        switch(cmd)
        {
            case "list":
                console.log("list profiles");
                console.log("list endpoints <profile name>");
                break;

            case "create":
                console.log("create profile <profile name>");
                console.log("create endpoint <profile name> <endpoint name> <origin hostname>");
                break;
            
            case "delete":
                console.log("delete profile <profile name>");
                console.log("delete endpoint <profile name> <endpoint name>");
                break;

            case "purge":
                console.log("purge <profile name> <endpoint name> <path>");
                break;

            default:
                console.log("Invalid command.");
        }
    }
    ```

7. Na kraju, funkcije smo hoćete li koristiti na klijent za upravljanje CDN su asinkronog, tako da ih morate način da biste nazvali natrag kad su.  Pogledajmo Provjerite koji možete prikazati Izlaz iz klijent management CDN (ako ih ima) i izađite iz programa bez poteškoća.

    ```javascript
    function callback(err, result, request, response) {
        if (err) {
            console.log(err);
            process.exit(1);
        } else {
            console.log((result == null) ? "Done!" : result);
            process.exit(0);
        }
    }
    ```

Osnovna struktura naš program napisan, ne možemo trebali biste stvoriti funkcije pod nazivom utemeljene na našem parametara.

## <a name="list-cdn-profiles-and-endpoints"></a>Popis CDN profila i krajnje točke

Započnimo s koda za naše postojeće profile i krajnje točke.  Moje kodom komentare pružaju sintaksa očekivani da bismo znali gdje dolazi svaki parametar.

```javascript
// list profiles
// list endpoints <profile name>
function cdnList(){
    requireParms(2);
    switch(parms[1].toLowerCase())
    {
        case "profiles":
            console.log("Listing profiles...");
            cdnClient.profiles.listByResourceGroup(resourceGroupName, callback);
            break;

        case "endpoints":
            requireParms(3);
            console.log("Listing endpoints...");
            cdnClient.endpoints.listByProfile(parms[2], resourceGroupName, callback);
            break;

        default:
            console.log("Invalid parameter.");
            process.exit(1);
    }
}
```

## <a name="create-cdn-profiles-and-endpoints"></a>Stvaranje profila CDN i krajnje točke

Zatim ćemo pisanje funkcije da biste stvorili profili i krajnje točke.

```javascript
function cdnCreate() {
    requireParms(2);
    switch(parms[1].toLowerCase())
    {
        case "profile":
            cdnCreateProfile();
            break;

        case "endpoint":
            cdnCreateEndpoint();
            break;

        default:
            console.log("Invalid parameter.");
            process.exit(1);
    }
}

// create profile <profile name>
function cdnCreateProfile() {
    requireParms(3);
    console.log("Creating profile...");
    var standardCreateParameters = {
        location: resourceLocation,
        sku: {
            name: 'Standard_Verizon'
        }
    };

    cdnClient.profiles.create(parms[2], standardCreateParameters, resourceGroupName, callback);
}

// create endpoint <profile name> <endpoint name> <origin hostname>        
function cdnCreateEndpoint() {
    requireParms(5);
    console.log("Creating endpoint...");
    var endpointProperties = {
        location: resourceLocation,
        origins: [{
            name: parms[4],
            hostName: parms[4]
        }]
    };

    cdnClient.endpoints.create(parms[3], endpointProperties, parms[2], resourceGroupName, callback);
}
```

## <a name="purge-an-endpoint"></a>Čišćenje krajnje točke

Pod pretpostavkom da je stvorena krajnju točku, jedan zadatak za uobičajene koje želimo izvesti u naš program je čišćenja sadržaja u našem krajnjoj točki.

```javascript
// purge <profile name> <endpoint name> <path>
function cdnPurge() {
    requireParms(4);
    console.log("Purging endpoint...");
    var purgeContentPaths = [ parms[3] ];
    cdnClient.endpoints.purgeContent(parms[2], parms[1], resourceGroupName, purgeContentPaths, callback);
}
```

## <a name="delete-cdn-profiles-and-endpoints"></a>Brisanje CDN profila i krajnje točke

Zadnji funkcija smo neće sadržavati stavku briše krajnje točke i profile.

```javascript
function cdnDelete() {
    requireParms(2);
    switch(parms[1].toLowerCase())
    {
        // delete profile <profile name>
        case "profile":
            requireParms(3);
            console.log("Deleting profile...");
            cdnClient.profiles.deleteIfExists(parms[2], resourceGroupName, callback);
            break;

        // delete endpoint <profile name> <endpoint name>
        case "endpoint":
            requireParms(4);
            console.log("Deleting endpoint...");
            cdnClient.endpoints.deleteIfExists(parms[3], parms[2], resourceGroupName, callback);
            break;

        default:
            console.log("Invalid parameter.");
            process.exit(1);
    }
}
```

## <a name="running-the-program"></a>Izvođenje programa

Ne možemo izvršiti sada naš program Node.js pomoću naš omiljene ispravljanje pogrešaka ili na konzoli sustava.

> [AZURE.TIP] Ako koristite Visual Studio kod kao program za ispravljanje pogrešaka, morat ćete postaviti vaše okruženje za prosljeđivanje u parametara naredbenog retka.  Visual Studio kod to čini u datoteci **lanuch.json** .  Pronađite svojstvo pod nazivom **argumenata** i dodajte polja niz vrijednosti za parametre, tako da izgleda ovako: `"args": ["list", "profiles"]`.

Započnimo stavku naš profila.

![Popis profila](./media/cdn-app-dev-node/cdn-list-profiles.png)

Ne možemo idite natrag prazno polje.  Budući da ne imamo sve profila u našem grupa resursa koje očekuje.  Pogledajmo sada stvorite profil.

![Stvaranje profila](./media/cdn-app-dev-node/cdn-create-profile.png)

Sada dodajte krajnje točke.

![Stvaranje krajnje točke](./media/cdn-app-dev-node/cdn-create-endpoint.png)

Na kraju, recimo izbrisati naš profil.

![Brisanje profila](./media/cdn-app-dev-node/cdn-delete-profile.png)

## <a name="next-steps"></a>Daljnji koraci

Da biste vidjeli dovršene projekta iz ovog vodiča [uzorka za preuzimanje](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74).

Da biste potražite u članku referenca za Azure CDN SDK-a za Node.js, pogledajte [reference](http://azure.github.io/azure-sdk-for-node/azure-arm-cdn/latest/).

Da biste pronašli dodatne dokumentaciju na Azure SDK Node.js, prikaz [punu referencu](http://azure.github.io/azure-sdk-for-node/).

Upravljanje resurse CDN sa [servisom PowerShell](./cdn-manage-powershell.md).

