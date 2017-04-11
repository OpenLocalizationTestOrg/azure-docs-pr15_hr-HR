<properties 
    pageTitle="Nove sheme verzija 2015-08-01-pretpregled" 
    description="Saznajte kako napisati definiciju JSON za najnoviju verziju aplikacije logike" 
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
    
# <a name="new-schema-version-2015-08-01-preview"></a>Nove sheme verzija 2015-08-01-pretpregled

Nove sheme i API verzija za aplikacije logike ima nekoliko poboljšanja koji poboljšanje pouzdanosti i jednostavnog od-korištenje logike aplikacija. Postoje 4 ključne razlike:

1. Vrsta akcije **APIApp** je ažuriran u novu vrstu **APIConnection** akcija.
2. **Ponovite** preimenovana je u **Foreach**.
3. Aplikaciju API-JA **Ga Slušatelj HTTP** više nije potrebna.
4. Pozivanje podređeni tijekovi rada koristi novu shemu.

## <a name="1-moving-to-api-connections"></a>1. premještanje API veze

Najveća promjena je li vam više nije potrebna za implementaciju aplikacije API-JA u pretplatu Azure korištenja API. Možete koristiti API-ji na 2 načina:
* Upravljani API
* Vaše prilagođene Web API-

Svaki od ovih obrađuje malo drugačije jer su različiti njihove upravljanja i hosting modela. Jedna od prednosti ovaj model je više ne koristite ograničeno na resurse koji su raspoređeni u grupu resursa. 

### <a name="managed-apis"></a>Upravljani API-ji

Postoje broj API-kojima upravlja Microsoft u vaše ime, kao što je Office 365, Salesforce, Twitter, FTP itd... Neke od tih upravljani API može se koristiti kao-se, kao što su Bing prijevod, a neke su potrebne konfiguracije. Tu konfiguraciju naziva *veze*.

Na primjer, kada koristite Office 365, morate da biste stvorili vezu koja sadrži Office 365 prijave token. Ovaj token bit će sigurno pohranjene i osvježiti tako da se aplikacijom logike uvijek možete nazvati API-JA sa sustavom Office 365. Osim toga, ako se želite povezati s poslužiteljem SQL ili FTP, morate stvoriti vezu koja sadrži niz za povezivanje. 

Unutar definiciju nazivaju se ove akcije `APIConnection`. Evo primjera vezu koja se poziva Office 365 da biste poslali poruku e-pošte:

```
{
    "actions": {
        "Send_Email": {
            "type": "ApiConnection",
            "inputs": {
                "host": {
                    "api": {
                        "runtimeUrl": "https://msmanaged-na.azure-apim.net/apim/office365"
                    },
                    "connection": {
                        "name": "@parameters('$connections')['shared_office365']['connectionId']"
                    }
                },
                "method": "post",
                "body": {
                    "Subject": "Reminder",
                    "Body": "Don't forget!",
                    "To": "me@contoso.com"
                },
                "path": "/Mail"
            }
        }
    }
}
```

Dio ulaza koja je jedinstvena API veze na `host` objekt. To sastoji se od dva dijela: `api` i `connection`.

U `api` sadrži runtime URL gdje koji upravljani API za nalazi. Vidjet ćete sve dostupne upravljani API-ji za tako da nazovete `GET https://management.azure.com/subscriptions/{subid}/providers/Microsoft.Web/managedApis/?api-version=2015-08-01-preview`.

Prilikom korištenja API, možda ili možda neće imati sve **parametre veze** definirani. Ako ga ne pa nema **veze** potreban je. Ako je tako, zatim morat ćete stvoriti vezu. Kada stvorite tu vezu imat ćete ime koje ste odabrali i zatim pozvati koji u na `connection` objekta unutar na `host` objekt. Da biste stvorili vezu u grupu resursa, nazovite:

```
PUT https://management.azure.com/subscriptions/{subid}/resourceGroups/{rgname}/providers/Microsoft.Web/connections/{name}?api-version=2015-08-01-preview
```

S tijelo sljedeće:


```
{
  "properties": {
    "api": {
      "id": "/subscriptions/{subid}/providers/Microsoft.Web/managedApis/azureblob"
    },
    "parameterValues" : {
        "accountName" : "{The name of the storage account -- the set of parameters is different for each API}"
    }
  },
  "location" : "{Logic app's location}"
}
```

### <a name="deploying-managed-apis-in-an-azure-resource-manager-template"></a>Implementacija upravljani API-ji u predlošku Upravitelj Azure resursa

Možete stvoriti punu aplikaciju u predlošku OKVIRA sve dok je ne potreban interaktivni prijava. Ako je potrebno prijaviti, možete postaviti sve s predloškom ARM, ali će i dalje imati posjetiti portal da biste autorizirali veze. 

```
    "resources": [{
        "apiVersion": "2015-08-01-preview",
        "name": "azureblob",
        "type": "Microsoft.Web/connections",
        "location": "[resourceGroup().location]",
        "properties": {
            "api": {
                "id": "[concat(subscription().id,'/providers/Microsoft.Web/locations/westus/managedApis/azureblob')]"
            },
            "parameterValues": {
                "accountName": "[parameters('storageAccountName')]",
                "accessKey": "[parameters('storageAccountKey')]"
            }
        }
    }, {
        "type": "Microsoft.Logic/workflows",
        "apiVersion": "2015-08-01-preview",
        "name": "[parameters('logicAppName')]",
        "location": "[resourceGroup().location]",
        "dependsOn": [
            "[resourceId('Microsoft.Web/connections', 'azureblob')]"
        ],
        "properties": {
            "sku": {
                "name": "[parameters('sku')]",
                "plan": {
                    "id": "[concat(resourceGroup().id, '/providers/Microsoft.Web/serverfarms/',parameters('svcPlanName'))]"
                }
            },
            "definition": {
                "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2015-08-01-preview/workflowdefinition.json#",
                "actions": {
                    "Create_file": {
                        "type": "apiconnection",
                        "inputs": {
                            "host": {
                                "api": {
                                    "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/azureblob"
                                },
                                "connection": {
                                    "name": "@parameters('$connections')['azureblob']['connectionId']"
                                }
                            },
                            "method": "post",
                            "queries": {
                                "folderPath": "[concat('/',parameters('containerName'))]",
                                "name": "helloworld.txt"
                            },
                            "body": "@decodeDataUri('data:,Hello+world!')",
                            "path": "/datasets/default/files"
                        },
                        "conditions": []
                    }
                },
                "contentVersion": "1.0.0.0",
                "outputs": {},
                "parameters": {
                    "$connections": {
                        "defaultValue": {},
                        "type": "Object"
                    }
                },
                "triggers": {
                    "recurrence": {
                        "type": "Recurrence",
                        "recurrence": {
                            "frequency": "Day",
                            "interval": 1
                        }
                    }
                }
            },
            "parameters": {
                "$connections": {
                    "value": {
                        "azureblob": {
                            "connectionId": "[concat(resourceGroup().id,'/providers/Microsoft.Web/connections/azureblob')]",
                            "connectionName": "azureblob",
                            "id": "[concat(subscription().id,'/providers/Microsoft.Web/locations/westus/managedApis/azureblob')]"
                        }

                    }
                }
            }
        }
    }]
```

Vidjet ćete u ovom primjeru da su veze samo normalne resursima koji žive u grupu resursa. Koje se odnose managedAPIs dostupne u svoju pretplatu.

### <a name="your-custom-web-apis"></a>Prilagođeni Web API-ji

Ako koristite vlastitu API-(Konkretno, ne Microsoft upravlja one), trebali biste pomoću ugrađenih akcijskih **HTTP** da biste nazvali ih. Da bi idealna sučelje, trebali biste izložiti krajnjoj točki swagger za vaše API-JA. To će omogućiti dizajneru aplikacije logike za prikazivanje unosa i izlaze za vaše API-JA. Bez swagger, dizajnera samo moći da bi se prikazala unosa i izlaza kao neprozirne JSON objekte.

Slijedi primjer prikazanim novim `metadata.apiDefinitionUrl` svojstvo:
```
{
   "actions": {
        "mycustomAPI": {
            "type": "http",
            "metadata" : {
              "apiDefinitionUrl" : "https://mysite.azurewebsites.net/api/apidef/"  
            },
            "inputs": {
                "uri": "https://mysite.azurewebsites.net/api/getsomedata",
                "method" : "GET"
            }
        }
    }
}
```

Ako glavno računalo Web API-JA na **Aplikacije servisa** , a zatim će automatski prikazivati na popisu akcije koje su dostupne u alatu za dizajniranje. Ako nije, morat ćete izravno zalijepite URL. Krajnja točka swagger mora biti neovlašten da bi se moći koristiti unutar dizajner logike aplikacije (iako možda sigurne API sam s bilo kakve metode podržava na Swagger).

### <a name="using-your-already-deployed-api-apps-with-2015-08-01-preview"></a>Pomoću aplikacije već distribuiranih API 2015-08-01 – pregled

Ako prethodno implementiran aplikaciju API-JA, možete je poziva putem **HTTP** akciju.

Ako, na primjer, ako koristite zajedničke mrežne mape na popisu datoteka, možda imate otprilike ovako u definicije **2014. – 12-01 – pregled** shema verzija:

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2014-12-01-preview/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token": {
            "defaultValue": "eyJ0eX...wCn90",
            "type": "String",
            "metadata": {
                "token": {
                    "name": "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token"
                }
            }
        }
    },
    "actions": {
        "dropboxconnector": {
            "type": "ApiApp",
            "inputs": {
                "apiVersion": "2015-01-14",
                "host": {
                    "id": "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector",
                    "gateway": "https://avdemo.azurewebsites.net"
                },
                "operation": "ListFiles",
                "parameters": {
                    "FolderPath": "/myfolder"
                },
                "authentication": {
                    "type": "Raw",
                    "scheme": "Zumo",
                    "parameter": "@parameters('/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token')"
                }
            }
        }
    }
}
```

Možete stvoriti ekvivalentnu HTTP akciju kao što su ispod (parametara dio ostaje definiciju aplikacije logike nepromijenjena):

```
{
    "actions": {
        "dropboxconnector": {
            "type": "Http",
            "metadata" : {
              "apiDefinitionUrl" : "https://avdemo.azurewebsites.net/api/service/apidef/dropboxconnector/?api-version=2015-01-14&format=swagger-2.0-standard"  
            },
            "inputs": {
                "uri": "https://avdemo.azurewebsites.net/api/service/invoke/dropboxconnector/ListFiles?api-version=2015-01-14",
                "method" : "POST",
                "body": {
                    "FolderPath": "/myfolder"
                },
                "authentication": {
                    "type": "Raw",
                    "scheme": "Zumo",
                    "parameter": "@parameters('/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token')"
                }
            }
        }
    }
}
```

Prolaska kroz tih svojstava jedan po jedan:

| Svojstvo akcije |  Opis |
| --------------- | -----------  |
| `type` | `Http`umjesto`APIapp` |
| `metadata.apiDefinitionUrl` | Ako želite koristiti ovu akciju u dizajneru aplikacije logike ćete koje želite uvrstiti krajnju točku metapodataka. To je sastavljena iz:`{api app host.gateway}/api/service/apidef/{last segment of the api app host.id}/?api-version=2015-01-14&format=swagger-2.0-standard` |
| `inputs.uri` | To je sastavljena iz:`{api app host.gateway}/api/service/invoke/{last segment of the api app host.id}/{api app operation}?api-version=2015-01-14` |
| `inputs.method` | Uvijek`POST` |
| `inputs.body` | Jednako aplikacije parametara API-ja | 
| `inputs.authentication` | Jednako provjere autentičnosti za aplikaciju API-ja |

Taj se način surađivati za sve akcije aplikacije API-JA. Međutim, imajte na umu te prethodne API aplikacije više nisu podržane, a trebali biste premjestiti u neku od dvije druge mogućnosti iznad (upravljani API-JA ili hosting prilagođene Web API-JA).

## <a name="2-repeat-renamed-to-foreach"></a>2. ponavljanje preimenovati Foreach

Prethodne verzije sheme smo dobili mnogo povratnu informaciju te **ponovite** je zbunjujuće i niste pravilno snimiti je li zaista je za svaki petlje. Kao rezultat ćemo imati preimenovana je u **Foreach**. Ako, na primjer:

```
{
    "actions": {
        "pingBing": {
            "type": "Http",
            "repeat": "@range(0,2)",
            "inputs": {
                "method": "GET",
                "uri": "https://www.bing.com/search?q=@{repeatItem()}"
            }
        }
    }
}
```

Želite sada biti napisani velikim:

```
{
    "actions": {
        "pingBing": {
            "type": "Http",
            "foreach": "@range(0,2)",
            "inputs": {
                "method": "GET",
                "uri": "https://www.bing.com/search?q=@{item()}"
            }
        }
    }
}
```

Prethodno funkciju `@repeatItem()` koristila referentni trenutne stavke koja se iterated iznad. To pojednostavnjenom kako bi samo `@item()`. 

### <a name="referencing-the-outputs-of-the-foreach"></a>Pozivate izlaze na Foreach
Da biste dodatno pojednostavili izlaze **Foreach** akcije ne će biti prelomljeni u objekt koji se naziva **repeatItems**. To znači da, dok su izlaze iznad ponavljanje:

```
{
    "repeatItems": [
        {
            "name": "pingBing",
            "inputs": {
                "uri": "https://www.bing.com/search?q=0",
                "method": "GET"
            },
            "outputs": {
                "headers": { },
                "body": "<!DOCTYPE html><html lang=\"en\" xml:lang=\"en\" xmlns=\"http://www.w3.org/1999/xhtml\" xmlns:Web=\"http://schemas.live.com/Web/\">...</html>"
            }
            "status": "Succeeded"
        }
    ]
}
```

Sada će biti:

```
[
    {
        "name": "pingBing",
        "inputs": {
            "uri": "https://www.bing.com/search?q=0",
            "method": "GET"
        },
        "outputs": {
            "headers": { },
            "body": "<!DOCTYPE html><html lang=\"en\" xml:lang=\"en\" xmlns=\"http://www.w3.org/1999/xhtml\" xmlns:Web=\"http://schemas.live.com/Web/\">...</html>"
        }
        "status": "Succeeded"
    }
]
```

Kada se pozivate te izlaze, da biste se tijelo akciju bit će potrebno učiniti:

```
{
    "actions": {
        "secondAction" : {
            "type" : "Http",
            "repeat" : "@outputs('pingBing').repeatItems",
            "inputs" : {
                "method" : "POST",
                "uri" : "http://www.example.com",
                "body" : "@repeatItem().outputs.body"
            }
        }
    }
}
```

Sada možete učiniti umjesto:

```
{
    "actions": {
        "secondAction" : {
            "type" : "Http",
            "foreach" : "@outputs('pingBing')",
            "inputs" : {
                "method" : "POST",
                "uri" : "http://www.example.com",
                "body" : "@item().outputs.body"
            }
        }
    }
}
```

S tim promjenama, funkcija `@repeatItem()`, `@repeatBody()` i `@repeatOutputs()` uklanjaju.

## <a name="3-native-http-listener"></a>3. nativni ga slušatelj HTTP 
Mogućnosti ga Slušatelj HTTP sada su ugrađene, tako da vam više nije potrebna za implementaciju aplikacije HTTP ga Slušatelj API-JA. Saznajte više o [potpuni kako vaše logike aplikacije krajnjoj točki callable ovdje](app-service-logic-http-endpoint.md). 

S tim promjenama, funkciju `@accessKeys()` se uklanja, a zatim je zamijenjena na `@listCallbackURL()` funkcija radi dobivanja krajnju točku (kada je potrebno). Osim toga, sada morate definirati barem jedan okidača aplikaciji logike sada. Ako želite `/run` tijek rada, morate koristiti neku od na `manual`, `apiConnectionWebhook` ili `httpWebhook` okidača. 

## <a name="4-calling-child-workflows"></a>4. tijekovi rada podređenim zovete

Prethodno, pozivanje podređeni tijekovi rada potreban namjeravate taj tijek rada, početak token za pristup, a zatim lijepljenjem koji u definicije logike aplikaciju koju želite pozvati te podređene. S novom verzijom sheme aplikacijama modul logike automatsko generiranje SAS prilikom izvođenja za podređeni tijek rada, što znači da ne morate sve tajne zalijepite definiciju.  Evo jednog primjera:

```
"mynestedwf" : {
    "type" : "workflow",
    "inputs" : {
        "host" : {
            "id" : "/subscriptions/xxxxyyyyzzz/resourceGroups/rg001/providers/Microsoft.Logic/mywf001",
            "triggerName" : "myendpointtrigger"
        },
        "queries" : {
            "extrafield" : "specialValue"
        },
        "headers" : {
            "x-ms-date" : "@utcnow()",
            "Content-type" : "application/json"
        },
        "body" : {
            "contentFieldOne" : "value100",
            "anotherField" : 10.001
        }
    },
    "conditions" : []
}
```

Drugi unaprjeđivanja je smo će biti daje podređeni tijekovi rada puni pristup dolazni zahtjev. To znači da možete proslijediti parametara u odjeljku *upita* i u objektu *zaglavlja* , a da u potpunosti možete definirati cijelo tijelo.

Na kraju, postoje potrebne promjene da biste podređeni tijek rada. Dok je prije no što nije jednostavno zovete podređeni tijek rada izravno; sada, morat ćete definirati krajnja točka za pokretanje tijeka rada za nadređeno da biste uputili poziv. Općenito govoreći, to znači da će se dodati okidača od vrsta **ručno** i koji koristi u definiciji nadređenog. Imajte na umu da u `host` svojstvo posebno ima na `triggerName`jer morate navesti koje okidača koje pozivate.

## <a name="other-changes"></a>Ostale promjene

### <a name="new-queries-property"></a>Novo svojstvo upita
Sve vrste akcija sada podržava novi unos naziva **upita**. To se može biti strukturirane objekt umjesto vas ručno prikupiti niz.

### <a name="parse-function-renamed"></a>Funkcija parse() preimenovano
Kao što smo će uskoro biti dodavanje više vrsta sadržaja, na `parse()` funkcija preimenovana je u `json()`.

## <a name="coming-soon-enterprise-integration-apis"></a>Uskoro dostupno: API-ji za integraciju Enterprise
Tom trenutku u trenutku, ne možemo ne još su upravlja verzijama API Integracija za Enterprise (primjerice AS2). To će biti uskoro dostupno u [vodič](http://www.zdnet.com/article/microsoft-outlines-its-cloud-and-server-integration-roadmap-for-2016/). U međuvremenu možete koristiti svoje postojeće distribuiranih BizTalk API-ji putem HTTP akciju kao prekriveno iznad u "koristi već distribuiranih aplikacija API."
