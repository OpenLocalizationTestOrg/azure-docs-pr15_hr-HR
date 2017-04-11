<properties
   pageTitle="Povezana predloške s Voditelj resursa | Microsoft Azure"
   description="U članku se opisuje korištenje povezane predložaka u predložak Voditelj resursa Azure da biste stvorili predložak modularan rješenja. Pokazuje kako proći vrijednosti parametara, navedite parametar datoteke i dinamički stvoreni URL-ova."
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
   ms.date="09/02/2016"
   ms.author="tomfitz"/>

# <a name="using-linked-templates-with-azure-resource-manager"></a>Rad s predlošcima povezane s Azure Voditelj resursa

Iz unutar jednog predloška Azure Voditelj resursa možete povezati s drugog predloška koji omogućuje decompose uvođenja u skupu ciljani, svrhu određene predloške. Kao i u slučaju decomposing aplikacije u nekoliko kod klase razlaganja pruža prednosti testiranja, ponovno korištenje i čitljivosti.  

Prenesite parametara iz predloška glavnog povezane predložak, a te parametre možete izravno mapirati parametara ili varijable koji prikazuje pozivanja predložak. Povezane predloška i prenositi izlazna varijabla predložak izvora Omogućivanje razmjene dvosmjerni podataka između predložaka.

## <a name="linking-to-a-template"></a>Povezivanje predloška

Stvaranje veze između dva predložaka dodavanjem implementacije resursa u glavnom predložak koji upućuje na predlošku povezani. Svojstvo **templateLink** na URI povezane predložak. Možete unijeti vrijednosti parametara za povezane predložak navođenjem vrijednosti izravno u predlošku ili povezivanje s datotekom parametar. Sljedeći primjer koristi svojstvo **parametara** da biste odredili vrijednost parametra izravno.

    "resources": [ 
      { 
         "apiVersion": "2015-01-01", 
         "name": "linkedTemplate", 
         "type": "Microsoft.Resources/deployments", 
         "properties": { 
           "mode": "incremental", 
           "templateLink": {
              "uri": "https://www.contoso.com/AzureTemplates/newStorageAccount.json",
              "contentVersion": "1.0.0.0"
           }, 
           "parameters": { 
              "StorageAccountName":{"value": "[parameters('StorageAccountName')]"} 
           } 
         } 
      } 
    ] 

Usluga upravljanja resursima moraju imati mogućnost da biste pristupili povezane predložak. Ne možete navesti lokalne datoteke ili datoteke koja je dostupna samo u lokalnoj mreži povezane predloška. Možete unijeti samo URI vrijednost koja sadrži **http** ili **https**. Jedan je mogućnost potvrdite predložak povezanih s računom za pohranu i korištenje URI za tu stavku, kao što je prikazano u sljedećem primjeru.

    "templateLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
        "contentVersion": "1.0.0.0",
    }

Iako povezane predložak mora biti vanjsko dostupna, ga ne moraju biti obično dostupnima javnosti. Predložak možete dodati račun za privatnih prostora za pohranu koji je dostupan samo vlasnik računa za pohranu. Zatim stvorite zajednički pristup token potpis (SAS) da biste omogućili pristup tijekom implementacije. Dodajte tu token SAS URI za povezane predložak. Upute za postavljanje predloška u račun za pohranu i generiranje SAS tokena, potražite u članku [uvođenja resursa s resursima predloške i Azure PowerShell](resource-group-template-deploy.md) ili [uvođenje resursa s resursima predloške i Azure EŽA](resource-group-template-deploy-cli.md). 

Sljedeći primjer prikazuje predloška nadređenog koja se povezuje s drugog predloška. Povezane predložak se pristupa putem SAS token koji se prenosi u kao parametar.

    "parameters": {
        "sasToken": { "type": "securestring" }
    },
    "resources": [
        {
            "apiVersion": "2015-01-01",
            "name": "linkedTemplate",
            "type": "Microsoft.Resources/deployments",
            "properties": {
              "mode": "incremental",
              "templateLink": {
                "uri": "[concat('https://storagecontosotemplates.blob.core.windows.net/templates/helloworld.json', parameters('sasToken'))]",
                "contentVersion": "1.0.0.0"
              }
            }
        }
    ],

Iako token se prenosi u kao sigurnu niz, URI povezane predložak, uključujući SAS token prijavljen je na postupci implementacije za tu grupu resursa. Da biste ograničili izlaganje, postavite na isteka za token.

## <a name="linking-to-a-parameter-file"></a>Povezivanje s datotekom parametra

Sljedeći primjer koristi svojstvo **parametersLink** za povezivanje s datotekom parametar.

    "resources": [ 
      { 
         "apiVersion": "2015-01-01", 
         "name": "linkedTemplate", 
         "type": "Microsoft.Resources/deployments", 
         "properties": { 
           "mode": "incremental", 
           "templateLink": {
              "uri":"https://www.contoso.com/AzureTemplates/newStorageAccount.json",
              "contentVersion":"1.0.0.0"
           }, 
           "parametersLink": { 
              "uri":"https://www.contoso.com/AzureTemplates/parameters.json",
              "contentVersion":"1.0.0.0"
           } 
         } 
      } 
    ] 

URI vrijednost za parametar povezane datoteke može biti lokalna datoteka, a mora sadržavati **http** ili **https**. Parametar datoteke mogu biti ograničen pristup SAS token.

## <a name="using-variables-to-link-templates"></a>Da biste se povezali predložaka pomoću varijabli

Prijašnjih primjera prikazivao programiranih vrijednosti URL veze za predložak. Taj se način možda radi jednostavnog predloška, ali ne funkcionira dobro kada radite s velikim skup modularan predložaka. Umjesto toga, možete stvoriti statične varijabla koja pohranjuje osnovni URL glavni predloška i zatim dinamičko stvaranje URL-ovi za predloške povezane s tom osnovnom URL-a. Prednost takvog je jednostavno možete premjestiti ili fork predložak jer samo morate promijeniti statične varijablu u glavnom predložak. Glavni predložak prosljeđuje točan ji cijeloj decomposed predložak.

Sljedeći primjer pokazuje kako koristiti osnovni URL da biste stvorili dva URL-ova za povezane predlošci (**sharedTemplateUrl** i **vmTemplate**). 

    "variables": {
        "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
        "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
        "tshirtSizeSmall": {
            "vmSize": "Standard_A1",
            "diskSize": 1023,
            "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
            "vmCount": 2,
            "slaveCount": 1,
            "storage": {
                "name": "[parameters('storageAccountNamePrefix')]",
                "count": 1,
                "pool": "db",
                "map": [0,0],
                "jumpbox": 0
            }
        }
    }

Možete koristiti [deployment()](resource-group-template-functions.md#deployment) da biste dobili osnovni URL za trenutni predložak, a koji koristite za dohvaćanje URL-a za ostale predloške na istom mjestu. Taj se način korisno je ako mjesto za predloške promjene (možda zbog rad s verzijama) ili želite da biste izbjegli konačnog kodiranje URL-ovi u datoteku predloška. 

    "variables": {
        "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"
    }

## <a name="conditionally-linking-to-templates"></a>Uvjetno povezivanje predložaka

Možete povezati različite predloške po Prenos u vrijednost parametra koji se koristi za sastavljanje URI povezane predložak. Taj se način funkcionira dobro kada je potrebno da biste odredili tijekom implementacije koja je povezana predložak za korištenje. Ako, na primjer, možete odrediti jedan predložak za korištenje postojećeg računa za pohranu i drugi predložak za korištenje na novi račun za pohranu.

Sljedeći primjer prikazuje parametar za naziv računa za pohranu i parametar odredite hoće li se na račun za pohranu novi ili postojeći.

    "parameters": {
        "storageAccountName": {
            "type": "String"
        },
        "newOrExisting": {
            "type": "String",
            "allowedValues": [
                "new",
                "existing"
            ]
        }
    },

Stvaranje tjednog prikaza kalendara za predložak URI koja sadrži vrijednost parametra novi ili postojeći.

    "variables": {
        "templatelink": "[concat('https://raw.githubusercontent.com/exampleuser/templates/master/',parameters('newOrExisting'),'StorageAccount.json')]"
    },

Navedite varijable vrijednost za implementaciju resurs.

    "resources": [
        {
            "apiVersion": "2015-01-01",
            "name": "linkedTemplate",
            "type": "Microsoft.Resources/deployments",
            "properties": {
                "mode": "incremental",
                "templateLink": {
                    "uri": "[variables('templatelink')]",
                    "contentVersion": "1.0.0.0"
                },
                "parameters": {
                    "StorageAccountName": {
                        "value": "[parameters('storageAccountName')]"
                    }
                }
            }
        }
    ],

URI razrješava predložak pod nazivom **existingStorageAccount.json** ili **newStorageAccount.json**. Stvaranje predložaka za te ji.

Sljedeći primjer prikazuje **existingStorageAccount.json** predložak.

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "storageAccountName": {
          "type": "String"
        }
      },
      "variables": {},
      "resources": [],
      "outputs": {
        "storageAccountInfo": {
          "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName')),providers('Microsoft.Storage', 'storageAccounts').apiVersions[0])]",
          "type" : "object"
        }
      }
    }

Sljedeći primjer prikazuje **newStorageAccount.json** predložak. Obratite pozornost na to da kao što su postojeće prostora za pohranu računa predložak objekta poslovnog subjekta za pohranu vraća u na izlaza. Glavni predložak funkcionira s ili povezane predložak.

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "storageAccountName": {
          "type": "string"
        }
      },
      "resources": [
        {
          "type": "Microsoft.Storage/storageAccounts",
          "name": "[parameters('StorageAccountName')]",
          "apiVersion": "2016-01-01",
          "location": "[resourceGroup().location]",
          "sku": {
            "name": "Standard_LRS"
          },
          "kind": "Storage",
          "properties": {
          }
        }
      ],
      "outputs": {
        "storageAccountInfo": {
          "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('StorageAccountName')),providers('Microsoft.Storage', 'storageAccounts').apiVersions[0])]",
          "type" : "object"
        }
      }
    }

## <a name="complete-example"></a>Primjer dovršeno

Sljedeći primjer predlošci Prikaži pojednostavljeni raspored povezane predložaka za ilustraciju nekoliko koncepta u ovom članku. Pretpostavlja se da Predlošci su dodani u istu spremnik u račun za pohranu s pristupom javno isključena. Povezane predložak prosljeđuje vrijednost na glavnom predložak u odjeljku **Proizvodi** .

Datoteka **parent.json** sastoji se od:

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "containerSasToken": { "type": "string" }
      },
      "resources": [
        {
          "apiVersion": "2015-01-01",
          "name": "linkedTemplate",
          "type": "Microsoft.Resources/deployments",
          "properties": {
            "mode": "incremental",
            "templateLink": {
              "uri": "[concat(uri(deployment().properties.templateLink.uri, 'helloworld.json'), parameters('containerSasToken'))]",
              "contentVersion": "1.0.0.0"
            }
          }
        }
      ],
      "outputs": {
        "result": {
          "type": "object",
          "value": "[reference('linkedTemplate').outputs.result]"
        }
      }
    }

Datoteka **helloworld.json** sastoji se od:

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {},
      "variables": {},
      "resources": [],
      "outputs": {
        "result": {
            "value": "Hello World",
            "type" : "string"
        }
      }
    }
    
U komponente PowerShell token pribaviti spremnik i implementirati predloške pomoću:

    Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates
    $token = New-AzureStorageContainerSASToken -Name templates -Permission r -ExpiryTime (Get-Date).AddMinutes(30.0)
    New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateUri ("https://storagecontosotemplates.blob.core.windows.net/templates/parent.json" + $token) -containerSasToken $token

U EŽA Azure token pribaviti spremnik i implementirati predloške pomoću sljedeći kod. Trenutno, navedite naziv za implementaciju prilikom korištenja predloška URI koji uključuje SAS token.  

    expiretime=$(date -I'minutes' --date "+30 minutes")  
    azure storage container sas create --container templates --permissions r --expiry $expiretime --json | jq ".sas" -r
    azure group deployment create -g ExampleGroup --template-uri "https://storagecontosotemplates.blob.core.windows.net/templates/parent.json?{token}" -n tokendeploy  

Morat ćete unijeti token SAS kao parametar. Morate počinjati token s **?**.

## <a name="next-steps"></a>Daljnji koraci
- Dodatne informacije o definiranju redoslijeda implementacije za resurse, potražite u članku [Definiranje ovisnosti u predlošcima Voditelj resursa za Azure](resource-group-define-dependencies.md)
- Da biste saznali kako definirati jedan resurs, ali stvoriti mnogo instanci, potražite u članku [Stvaranje više instanci resursa u Azure Voditelj resursa](resource-group-create-multiple.md)
