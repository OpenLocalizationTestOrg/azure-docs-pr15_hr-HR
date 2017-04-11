<properties
   pageTitle="Omogućeno na Azure resursima | Microsoft Azure"
   description="Stvaranje predložaka Voditelj resursa Azure pomoću deklarativno JSON sintakse za implementaciju aplikacije za Azure."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="tomfitz"/>

# <a name="authoring-azure-resource-manager-templates"></a>Voditelj resursa Azure omogućeno

U ovoj se temi opisuju strukturu predloška Azure Voditelj resursa. Prikazuju se različite sekcije predloška i svojstva koja su dostupne u tih odjeljaka. Predložak se sastoji od JSON i izraza koje možete koristiti za sastavljanje vrijednosti za implementaciju sustava. 

Predložak za resurse koji ste već ste implementirali potražite u članku [Izvoz predložak Voditelj resursa Azure iz postojećih resursa](resource-manager-export-template.md). Upute o stvaranju predloška potražite u članku [Vodič za upravljanje resursima predložak](resource-manager-template-walkthrough.md). Preporuke o stvaranju predložaka potražite u članku [preporučeni Načini stvaranja predloška Azure Voditelj resursa](resource-manager-template-best-practices.md).

Dobar uređivač JSON može pojednostavniti zadatak stvaranja predloška. Informacije o korištenju Visual Studio s predloške potražite u članku [Stvaranje i implementacija grupe Azure resursa kroz Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md). Informacije o pomoću koda za dodavanje veze za VANJSKIH potražite u članku [Rad s predlošcima resursima Azure u Visual Studio kod](resource-manager-vs-code.md).

Ograničenje veličine predloškom 1 MB, a parametar datoteke 64 KB. 1 MB ograničenje primjenjuje se na završno stanje predloška kada je proširen s iteracijama resursa definicije i vrijednosti za varijable i parametre. 

## <a name="template-format"></a>Obliku predloška

U njegovoj strukturi najjednostavniji predložak sadrži sljedeće elemente:

    {
       "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "",
       "parameters": {  },
       "variables": {  },
       "resources": [  ],
       "outputs": {  }
    }

| Naziv elementa   | Obavezno | Opis
| :------------: | :------: | :----------
| $schema        |   Da    | Mjesto datoteke sheme JSON koji opisuje verziju jezika predloška. Koristite URL prikazan u prethodnom primjeru.
| contentVersion |   Da    | Verzije predloška (kao što su 1.0.0.0). Možete unijeti bilo koja vrijednost za taj element. Kada implementirate resursa pomoću ovog predloška, vrijednost se može koristiti da biste bili sigurni da se koristi desnom predložak.
| Parametri     |   ne     | Vrijednosti koje služe prilikom implementacije se izvršava da biste prilagodili implementacije resursa.
| varijable      |   ne     | Vrijednosti koje se koriste kao JSON fragmentirane u predlošku da biste pojednostavnili izraza jezika za predložak.
| Resursi      |   Da    | Vrste resursa koji su implementiran ili ažurirati u grupu resursa.
| izlaze        |   ne     | Vrijednosti koje vraćaju se nakon implementacije.

Ne možemo pregledajte sekcije predloška nešto podrobnije u nastavku ovog članka. Zasad, ne možemo pregledati i neke od vidjet ćete da sintaksa koji čine predložak.

## <a name="expressions-and-functions"></a>Izrazima i funkcijama

Sintaksa Osnovni predložak je JSON. Međutim, izrazima i funkcijama proširivanje JSON koja je dostupna u predlošku. S izrazima, stvorite vrijednosti koje nisu točno slovni vrijednosti. Izrazi zatvorene uglatim zagradama [a], te se izvode kada je implementiran u predložak. Izraze mogu pojaviti bilo gdje u vrijednost niza JSON i uvijek vratiti neku drugu vrijednost JSON. Ako trebate koristiti slovni niz koji započinje uglata zagrada [, morate koristiti dvije uglate zagrade [[.

Obično ćete koristiti izraze s funkcijama za izvođenje operacija za konfiguriranje uvođenje. Baš kao i u JavaScript, funkcija pozive su oblikovani kao **functionName(arg1,arg2,arg3)**. Svojstva se referencirati pomoću operatora točke i [indeks].

Sljedeći primjer pokazuje kako koristiti nekoliko funkcija tijekom izgradnje vrijednosti:
 
    "variables": {
       "location": "[resourceGroup().location]",
       "usernameAndPassword": "[concat('parameters('username'), ':', parameters('password'))]",
       "authorizationHeader": "[concat('Basic ', base64(variables('usernameAndPassword')))]"
    }

Cijeli popis predloška funkcija potražite u članku [funkcije predloška Azure Voditelj resursa](resource-group-template-functions.md). 


## <a name="parameters"></a>Parametri

U odjeljku parametara predložak, navedite vrijednosti koje možete unos pri implementaciji resurse. Ove vrijednosti parametara omogućuju vam da biste prilagodili implementacijskih unosom vrijednosti koje su prilagođene za okruženju (kao što je razvojni, testiranje i Proizvodnja). Ne morate unijeti parametara u predlošku, ali bez parametara predložak želite implementirati uvijek iste resurse s istim nazivima, mjesta i svojstva.

Koristite ove vrijednosti parametara u predlošku da biste postavili vrijednosti za distribuiranih resurse. Samo parametre koje su deklarirani u odjeljku parametara može se koristiti u ostale sekcije predloška.

Definiranje parametara sa sljedećom strukturom:

    "parameters": {
       "<parameter-name>" : {
         "type" : "<type-of-parameter-value>",
         "defaultValue": "<default-value-of-parameter>",
         "allowedValues": [ "<array-of-allowed-values>" ],
         "minValue": <minimum-value-for-int>,
         "maxValue": <maximum-value-for-int>,
         "minLength": <minimum-length-for-string-or-array>,
         "maxLength": <maximum-length-for-string-or-array-parameters>,
         "metadata": {
             "description": "<description-of-the parameter>" 
         }
       }
    }

| Naziv elementa   | Obavezno | Opis
| :------------: | :------: | :----------
| parameterName  |   Da    | Naziv parametra. Mora biti valjane identifikator JavaScript.
| Vrsta           |   Da    | Vrsta vrijednosti parametra. Pogledajte popis ispod dopuštene vrste.
| defaultValue   |   ne     | Zadana vrijednost za parametar, ako je vrijednost za parametar.
| allowedValues  |   ne     | Polja dopuštena vrijednost za parametar da biste bili sigurni da desne vrijednosti navedene.
| minValue       |   ne     | Minimalna vrijednost za parametar vrsta int tu vrijednost je uključujući te dvije vrijednosti.
| maxValue       |   ne     | Najveća vrijednost za parametre vrsta int, tu vrijednost je uključivo.
| minLength      |   ne     | Minimalna duljina niza, secureString i parametara vrsta polja tu vrijednost je uključujući te dvije vrijednosti.
| maxLength      |   ne     | Maksimalna duljina niza, secureString i parametara vrsta polja, tu vrijednost je uključujući te dvije vrijednosti.
| Opis    |   ne     | Opis parametar koji će se prikazati korisnicima predloška putem sučelja portala prilagođeni predložak.

Dopuštene vrste i vrijednosti su:

- **niz**
- **secureString**
- **INT**
- **booleovom**
- **objekt** 
- **secureObject**
- **polja**

Da biste odredili parametar kao Neobavezno, navedite defaultValue (može biti prazni niz). 

Ako odredite naziv parametra koji odgovara jednoj od parametara u naredbu za uvođenje predloška, morat ćete navesti vrijednost za parametar s postfix **FromTemplate**. Ako, na primjer, ako sadrži parametar pod nazivom **ResourceGroupName** u predlošku koji je jednak parametar **ResourceGroupName** u [Novo AzureRmResourceGroupDeployment] [ deployment2cmdlet] cmdlet, morat ćete navesti vrijednost za **ResourceGroupNameFromTemplate**. Općenito govoreći, izbjegavajte ovaj zbrku ne imenovanje parametara s istim nazivom kao parametar koji se koristi za implementaciju operacije.

>[AZURE.NOTE] Sve lozinke, tipke i druge tajne trebali biste koristiti vrstu **secureString** . Parametri predloška s vrstom secureString ne može pročitati nakon implementacije resursa. 

Sljedeći primjer prikazuje način za definiranje parametara:

    "parameters": {
      "siteName": {
        "type": "string",
        "defaultValue": "[concat('site', uniqueString(resourceGroup().id))]"
      },
      "hostingPlanName": {
        "type": "string",
        "defaultValue": "[concat(parameters('siteName'),'-plan')]"
      },
      "skuName": {
        "type": "string",
        "defaultValue": "F1",
        "allowedValues": [
          "F1",
          "D1",
          "B1",
          "B2",
          "B3",
          "S1",
          "S2",
          "S3",
          "P1",
          "P2",
          "P3",
          "P4"
        ]
      },
      "skuCapacity": {
        "type": "int",
        "defaultValue": 1,
        "minValue": 1
      }
    }

Kako se tijekom implementacije za unos vrijednosti parametra, pročitajte članak [uvođenja aplikacije pomoću predloška Azure Voditelj resursa](resource-group-template-deploy.md#parameter-file). 

## <a name="variables"></a>Varijable

U odjeljku varijable izgraditi vrijednosti koje se mogu koristiti u predlošku. Obično varijable temelje se na vrijednosti navedene iz parametre. Nije potrebno definirati varijable, ali se često Pojednostavnite predložak smanjivanjem složenim izrazima.

Definiranje varijable s sljedeću strukturu:

    "variables": {
       "<variable-name>": "<variable-value>",
       "<variable-name>": { 
           <variable-complex-type-value> 
       }
    }

Sljedeći primjer prikazuje način da biste definirali varijablu konstruirana iz dvije vrijednosti parametara:

     "variables": {
       "connectionString": "[concat('Name=', parameters('username'), ';Password=', parameters('password'))]"
    }

U sljedećem primjeru prikazuje varijabla koja je složene vrste JSON i varijabli koje su konstruirana iz druge varijable:

    "parameters": {
       "environmentName": {
         "type": "string",
         "allowedValues": [
           "test",
           "prod"
         ]
       }
    },
    "variables": {
       "environmentSettings": {
         "test": {
           "instancesSize": "Small",
           "instancesCount": 1
         },
         "prod": {
           "instancesSize": "Large",
           "instancesCount": 4
         }
       },
       "currentEnvironmentSettings": "[variables('environmentSettings')[parameters('environmentName')]]",
       "instancesSize": "[variables('currentEnvironmentSettings').instancesSize]",
       "instancesCount": "[variables('currentEnvironmentSettings').instancesCount]"
    }

## <a name="resources"></a>Resursi

U odjeljku resursi definirajte resurse koji su implementiran ili ažurirati. U ovom se odjeljku možete dobiti složene jer morate razumjeti vrste implementacije omogućuju desnom vrijednosti. 

Definiranje resursa s sljedeću strukturu:

    "resources": [
       {
         "apiVersion": "<api-version-of-resource>",
         "type": "<resource-provider-namespace/resource-type-name>",
         "name": "<name-of-the-resource>",
         "location": "<location-of-resource>",
         "tags": "<name-value-pairs-for-resource-tagging>",
         "comments": "<your-reference-notes>",
         "dependsOn": [
           "<array-of-related-resource-names>"
         ],
         "properties": "<settings-for-the-resource>",
         "copy": {
           "name": "<name-of-copy-loop>",
           "count": "<number-of-iterations>"
         }
         "resources": [
           "<array-of-child-resources>"
         ]
       }
    ]

| Naziv elementa             | Obavezno | Opis
| :----------------------: | :------: | :----------
| apiVersion               |   Da    | Verzija REST API-JA namijenjen stvaranju resurs.
| Vrsta                     |   Da    | Vrsta resursa. Ta vrijednost je kombinacija naziva davatelja resursa i vrstu resursa (kao što je **Microsoft.Storage/storageAccounts**).
| ime                     |   Da    | Naziv resursa. Naziv moraju pratiti URI komponente ograničenja definirano u RFC3986. Osim toga, Azure servise koji izlažu naziv resursa za izvan stranama provjeru naziv da biste provjerili je nije pokušaj lažiraju drugi identitet. U odjeljku [Provjerite naziv resursa](https://msdn.microsoft.com/library/azure/mt219035.aspx).
| mjesto                 |   Mijenja se  | Podržani zemlj mjesta od navedenih resursa. Možete odabrati bilo koju od mjesta dostupnih, ali obično je li bolje da biste odabrali neko sličan onome korisnika. Obično je i logično da biste postavili resurse koji interakciju jedna s drugom u istom području. Većinu resursa potreban mjesto, ali neke vrste (kao što su Dodjela uloge) zahtijevaju mjesto.
| oznaka                     |   ne     | Oznake koje su vezane uz resurs.
| Komentari                 |   ne     | Bilješke za dokumentiranje resursa u predlošku
| dependsOn                |   ne     | Resursi koji ovisi o resursa koji se definiraju. Vrednuju se međuzavisnosti resurse i resursima uvode se njihov redoslijed zavisne. Kada su resursi ovise o međusobno povezani, implementacije paralelno. Vrijednost može biti odvojenih zarezom popis resursa imena ili resursa odredišnih stilova.
| Svojstva               |   ne     | Postavke konfiguracije specifične za resursa. Vrijednosti za svojstva jednaki su vrijednosti navedene u tijelu zahtjeva za operaciju REST API-JA (STAVI metoda) da biste stvorili resurs. Veza na dokumentaciju sheme resursa ili REST API-JA, potražite u članku [davatelji Voditelj resursa, područja, verzije API-JA i sheme](resource-manager-supported-services.md).
| Kopiraj                     |   ne     | Ako je potrebno više instanci, broj resursa da biste stvorili. Dodatne informacije potražite u članku [Stvaranje više instanci resursa u Azure Voditelj resursa](resource-group-create-multiple.md). |
| Resursi                |   ne     | Podređeni resursa koje ovise o resursa koji se definiraju. Možete unijeti samo vrste resursa koji je dopušteno prema shemi nadređenog resursa. Potpuno kvalificiran naziv podređene vrste resursa obuhvaća nadređene vrste resursa, kao što su **Microsoft.Web/sites/extensions**. Zavisnost o resursa nadređenog se podrazumijeva; morate definirati izričito te ovisnosti. 

Kada zna koje vrijednosti navesti za **apiVersion**, **Vrsta**i **mjesto** nije odmah očite. Srećom, možete odrediti te vrijednosti putem Azure PowerShell ili Azure EŽA.

Da biste svim davateljima resursa sa **servisom PowerShell**, koristite:

    Get-AzureRmResourceProvider -ListAvailable

Na popisu vraćeni pronađite davatelja resursa koji vas zanima. Da biste dobili vrste resursa za davatelja resursa (kao što je prostor za pohranu), koristite:

    (Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Storage).ResourceTypes

Da biste verzije API-JA za vrstu resursa (takve račune za pohranu), koristite:

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Storage).ResourceTypes | Where-Object ResourceTypeName -eq storageAccounts).ApiVersions

Da biste podržani mjesta za vrstu resursa, koristite:

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Storage).ResourceTypes | Where-Object ResourceTypeName -eq storageAccounts).Locations

Da biste svim davateljima resursa s **EŽA Azure**, koristite:

    azure provider list

Na popisu vraćeni pronađite davatelja resursa koji vas zanima. Da biste dobili vrste resursa za davatelja resursa (kao što je prostor za pohranu), koristite:

    azure provider show Microsoft.Storage

Da biste API verzije i podržanim mjesta, koristite:

    azure provider show Microsoft.Storage --details --json

Dodatne informacije o davateljima resursa potražite u članku [davatelji Voditelj resursa, područja, verzije API-JA i sheme](resource-manager-supported-services.md).

U odjeljku resursa sadrži polje resursa koje želite uvesti. Unutar svakog resursa možete definirati i polja od podređenih resursa. Stoga vaše sekciji resursi može imati strukturu kao što su:

    "resources": [
       {
           "name": "resourceA",
       },
       {
           "name": "resourceB",
           "resources": [
               {
                   "name": "firstChildResourceB",
               },
               {   
                   "name": "secondChildResourceB",
               }
           ]
       },
       {
           "name": "resourceC",
       }
    ]


Sljedeći primjer prikazuje **Microsoft.Web/serverfarms** resursa i resursa **Microsoft.Web/sites** dijete **proširenja** resursa. Obratite pozornost na to da web-mjesto označene kao zavisne na farmi poslužitelja jer na farmi poslužitelja mora postojati prije nego što se može uvesti u web-mjesta. Primijetite prevelika da resursa **proširenja** podređenih web-mjesta.

    "resources": [
      {
        "apiVersion": "2015-08-01",
        "name": "[parameters('hostingPlanName')]",
        "type": "Microsoft.Web/serverfarms",
        "location": "[resourceGroup().location]",
        "tags": {
          "displayName": "HostingPlan"
        },
        "sku": {
          "name": "[parameters('skuName')]",
          "capacity": "[parameters('skuCapacity')]"
        },
        "properties": {
          "name": "[parameters('hostingPlanName')]",
          "numberOfWorkers": 1
        }
      },
      {
        "apiVersion": "2015-08-01",
        "type": "Microsoft.Web/sites",
        "name": "[parameters('siteName')]",
        "location": "[resourceGroup().location]",
        "tags": {
          "environment": "test",
          "team": "Web"
        },
        "dependsOn": [
          "[concat('Microsoft.Web/serverFarms/', parameters('hostingPlanName'))]"
        ],
        "properties": {
          "name": "[parameters('siteName')]",
          "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
        },
        "resources": [
          {
            "apiVersion": "2015-08-01",
            "type": "extensions",
            "name": "MSDeploy",
            "dependsOn": [
              "[concat('Microsoft.Web/sites/', parameters('siteName'))]"
            ],
            "properties": {
              "packageUri": "https://auxmktplceprod.blob.core.windows.net/packages/StarterSite-modified.zip",
              "dbType": "None",
              "connectionString": "",
              "setParameters": {
                "Application Path": "[parameters('siteName')]"
              }
            }
          }
        ]
      }
    ]


## <a name="outputs"></a>Izlaze

U odjeljku izlaze Navedite vrijednosti koje se vraćaju s implementacije. Na primjer, možete vratiti URI da biste pristupili distribuiranih resursa.

Sljedeći primjer prikazuje strukturu definiciju izlaz:

    "outputs": {
       "<outputName>" : {
         "type" : "<type-of-output-value>",
         "value": "<output-value-expression>"
       }
    }

| Naziv elementa   | Obavezno | Opis
| :------------: | :------: | :----------
| outputName     |   Da    | Naziv vrijednosti izlaz. Mora biti valjane identifikator JavaScript.
| Vrsta           |   Da    | Vrsta vrijednosti izlaz. Izlaznih vrijednosti podržava iste vrste kao ulaznih parametara za predložak.
| vrijednost          |   Da    | Predložak jezik izraz koji je izračunat i vraćen kao vrijednost izlaz.


Sljedeći primjer prikazuje vrijednost koja se vraća u odjeljku izlaza.

    "outputs": {
       "siteUri" : {
         "type" : "string",
         "value": "[concat('http://',reference(resourceId('Microsoft.Web/sites', parameters('siteName'))).hostNames[0])]"
       }
    }

Dodatne informacije o radu s izlaz potražite u članku [zajedničko korištenje stanja u predlošcima Azure Voditelj resursa](best-practices-resource-manager-state.md).

## <a name="next-steps"></a>Daljnji koraci
- Da biste pogledali potpuni predloške za različite vrste rješenja, potražite u članku [Azure predložaka za brzi početak rada](https://azure.microsoft.com/documentation/templates/).
- Dodatne informacije o funkcijama koje možete koristiti unutar predloška potražite u odjeljku [Azure resursima predložak funkcije](resource-group-template-functions.md).
- Da biste kombinirali većeg broja predložaka tijekom implementacije, potražite u članku [Rad s predlošcima povezane s Azure Voditelj resursa](resource-group-linked-templates.md).
- Možda ćete morati resursi koji postoje u skupini drugi resurs. To je uobičajen scenarij prilikom rada s računima za pohranu ili virtualne mreže koje su zajedničke više grupa resursa. Dodatne informacije potražite u članku [resourceId (opis funkcije)](resource-group-template-functions.md#resourceid).


[deployment2cmdlet]: https://msdn.microsoft.com/library/mt740620(v=azure.200).aspx
