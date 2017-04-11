<properties
    pageTitle="Zapisivanje i pogreškom u aplikacijama logike | Microsoft Azure"
    description="Prikaz slučaja stvarnih koristi napredne pogreške rukovanja i zapisivanje s aplikacijama logike"
    keywords=""
    services="logic-apps"
    authors="hedidin"
    manager="anneta"
    editor=""
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/29/2016"
    ms.author="b-hoedid"/>

# <a name="logging-and-error-handling-in-logic-apps"></a>Zapisivanje i u aplikacijama logike rukovanja pogreškama

U ovom se članku opisuje kako možete proširiti logike aplikacije da biste bolje podržavaju rukovanje iznimke. Korištenje stvarnih slučaja i naše odgovor na pitanje, od "Logika aplikacija podržava iznimku i pogreškama?"

>[AZURE.NOTE]Trenutna verzija značajke logike aplikacije servisa za aplikaciju Microsoft Azure nudi standardni predložak za odgovore akcija.
>To uključuje Interna provjere valjanosti i odgovore pogreške vratio aplikaciju API-JA.

## <a name="overview-of-the-use-case-and-scenario"></a>Pregled koristi slučaj i scenarija

Sljedeći članak vrijedi koristi za ovog članka.
Poznati zdravstvene tvrtke ili ustanove aktivan nam za razvoj Azure rješenje koje želite stvoriti pacijenta portal pomoću Microsoft Dynamics CRM Online. Su potrebne za slanje obveza zapise između Dynamics CRM Online pacijenta portal i Salesforce.  Ne možemo su zatražiti da koriste standard [HL7 FHIR](http://www.hl7.org/implement/standards/fhir/) za sve zapise pacijenta.

Projekt ima dvije glavne preduvjete:  

 -  Metode za prijavu zapisa koji su poslane s portala sustava Dynamics CRM Online
 -  Način za prikaz sve pogreške koje je došlo je do unutar tijeka rada


## <a name="how-we-solved-the-problem"></a>Kako ćemo otkloniti problem

>[AZURE.TIP] [Grupiranje korisnika za integraciju](http://www.integrationusergroup.com/do-logic-apps-support-error-handling/ "Integracija korisnika grupe")možete pogledati više razine videozapis projekta.

Ne možemo [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/ "Azure DocumentDB") odabran kao spremište zapisa zapisnika i pogreške (DocumentDB odnosi se na zapise kao dokumenti). Budući da logike aplikacije standardni predložak za sve odgovore, bi ne imamo da biste stvorili prilagođenu shemu. Ne možemo stvoriti API aplikacije da biste **umetnuli** i **upit** za pogreške i zapisnika zapisi. Za svaki unos u aplikaciji API-JA ne možemo nije definirati i shemu.  

Drugi preduvjet je za brisanje zapisa nakon određenog datuma. DocumentDB ima svojstvo naziva [Vrijeme do aktivacije](https://azure.microsoft.com/blog/documentdb-now-supports-time-to-live-ttl/ "Vrijeme do aktivacije") (TTL), a to dopušteno nam da biste postavili **Vrijeme do aktivacije** vrijednosti za svaki zapis ili zbirke. To ukloniti morati ručno brisanje zapisa u DocumentDB.

### <a name="creation-of-the-logic-app"></a>Stvaranje aplikacije logike

U prvi je korak da biste stvorili aplikaciju logiku i učitavanje u alatu za dizajniranje. U ovom primjeru smo koriste nadređenih i podređenih logike aplikacije. Recimo da smo da ste već stvorili nadređenog i namjeravate stvoriti jednu podređeni logike aplikacije.

Budući da ne možemo ćete biti zapisivanje zapis koji dolaze iz Dynamics CRM Online Započnimo pri vrhu. Moramo koristite okidač zahtjev jer aplikaciju logike nadređenog pokreće ovaj podređenih.

> [AZURE.IMPORTANT] Da biste dovršili ovaj Praktični vodič, morat ćete stvoriti DocumentDB baze podataka i dvije zbirke (prijave i pogreškama).

### <a name="logic-app-trigger"></a>Pokretanje aplikacije logike

Ne možemo koristite okidač zahtjev kao što je prikazano u sljedećem primjeru.

```` json
"triggers": {
        "request": {
          "type": "request",
          "kind": "http",
          "inputs": {
            "schema": {
              "properties": {
                "CRMid": {
                  "type": "string"
                },
                "recordType": {
                  "type": "string"
                },
                "salesforceID": {
                  "type": "string"
                },
                "update": {
                  "type": "boolean"
                }
              },
              "required": [
                "CRMid",
                "recordType",
                "salesforceID",
                "update"
              ],
              "type": "object"
            }
          }
        }
      },

````


### <a name="steps"></a>Koraci

Ne možemo morati prijaviti izvora (zahtjev) pacijenta zapis s portala sustava Dynamics CRM Online.

1. Moramo da biste novi zapis obveza s Dynamics CRM Online.
    Pokretanje koji dolaze iz CRM pruža nam na **CRM PatentId**, **vrstu zapisa** **Novo ili ažuriranje zapisa** (novi ili ažurirati Booleove vrijednosti), i **SalesforceId**. Budući da se koristi samo za ažuriranje **SalesforceId** može biti null.
    Ne možemo će dobiti CRM zapis pomoću CRM **PatientID** i **Vrstu zapisa**.
1. Sljedeći je korak da biste dodali naša aplikacija za DocumentDB API **InsertLogEntry** operacija kao što je prikazano u sljedeće slike.


#### <a name="insert-log-entry-designer-view"></a>Umetanje prikaz dizajnera zapisnika

![Umetanje unosa zapisnika](./media/app-service-logic-scenario-error-and-exception-handling/lognewpatient.png)

#### <a name="insert-error-entry-designer-view"></a>Umetanje prikaz dizajnera pogreške
![Umetanje unosa zapisnika](./media/app-service-logic-scenario-error-and-exception-handling/insertlogentry.png)

#### <a name="check-for-create-record-failure"></a>Potvrdite za stvaranje zapisa nije uspjelo

![Uvjet](./media/app-service-logic-scenario-error-and-exception-handling/condition.png)


## <a name="logic-app-source-code"></a>Logika aplikacije izvornog koda

>[AZURE.NOTE]  U nastavku su samo uzorka. Jer ovog praktičnog vodiča temelje se na implementacija trenutno u radnog, vrijednost **Izvora čvor** možda se neće prikazivati svojstva vezani uz zakazivanje obveze.

### <a name="logging"></a>Bilježenje u zapisnik
Sljedećim primjerom koda aplikacije logike prikazuje kako rukovati zapisivanje.

#### <a name="log-entry"></a>Stavka evidencije
Ovo je logike aplikacije izvornog koda za umetanje stavka evidencije.

``` json
"InsertLogEntry": {
        "metadata": {
        "apiDefinitionUrl": "https://.../swagger/docs/v1",
        "swaggerSource": "website"
        },
        "type": "Http",
        "inputs": {
        "body": {
            "date": "@{outputs('Gets_NewPatientRecord')['headers']['Date']}",
            "operation": "New Patient",
            "patientId": "@{triggerBody()['CRMid']}",
            "providerId": "@{triggerBody()['providerID']}",
            "source": "@{outputs('Gets_NewPatientRecord')['headers']}"
        },
        "method": "post",
        "uri": "https://.../api/Log"
        },
        "runAfter":    {
            "Gets_NewPatientecord": ["Succeeded"]
        }
}
```

#### <a name="log-request"></a>Zahtjev za zapisnika

Ovo je poruka zahtjev zapisnika objavljena na aplikaciju API-JA.

``` json
    {
    "uri": "https://.../api/Log",
    "method": "post",
    "body": {
        "date": "Fri, 10 Jun 2016 22:31:56 GMT",
        "operation": "New Patient",
        "patientId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "providerId": "",
        "source": "{\"Pragma\":\"no-cache\",\"x-ms-request-id\":\"e750c9a9-bd48-44c4-bbba-1688b6f8a132\",\"OData-Version\":\"4.0\",\"Cache-Control\":\"no-cache\",\"Date\":\"Fri, 10 Jun 2016 22:31:56 GMT\",\"Set-Cookie\":\"ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1\",\"Server\":\"Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0\",\"X-AspNet-Version\":\"4.0.30319\",\"X-Powered-By\":\"ASP.NET\",\"Content-Length\":\"1935\",\"Content-Type\":\"application/json; odata.metadata=minimal; odata.streaming=true\",\"Expires\":\"-1\"}"
        }
    }

```


#### <a name="log-response"></a>Evidentiraj odgovor

Ovo je poruka s odgovorom zapisnika u aplikaciji API-JA.

``` json
{
    "statusCode": 200,
    "headers": {
        "Pragma": "no-cache",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:32:17 GMT",
        "Server": "Microsoft-IIS/8.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "964",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "ttl": 2592000,
        "id": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0_1465597937",
        "_rid": "XngRAOT6IQEHAAAAAAAAAA==",
        "_self": "dbs/XngRAA==/colls/XngRAOT6IQE=/docs/XngRAOT6IQEHAAAAAAAAAA==/",
        "_ts": 1465597936,
        "_etag": "\"0400fc2f-0000-0000-0000-575b3ff00000\"",
        "patientID": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "timestamp": "2016-06-10T22:31:56Z",
        "source": "{\"Pragma\":\"no-cache\",\"x-ms-request-id\":\"e750c9a9-bd48-44c4-bbba-1688b6f8a132\",\"OData-Version\":\"4.0\",\"Cache-Control\":\"no-cache\",\"Date\":\"Fri, 10 Jun 2016 22:31:56 GMT\",\"Set-Cookie\":\"ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1\",\"Server\":\"Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0\",\"X-AspNet-Version\":\"4.0.30319\",\"X-Powered-By\":\"ASP.NET\",\"Content-Length\":\"1935\",\"Content-Type\":\"application/json; odata.metadata=minimal; odata.streaming=true\",\"Expires\":\"-1\"}",
        "operation": "New Patient",
        "salesforceId": "",
        "expired": false
    }
}

```

Sada Pogledajmo rukovanja korake pogreškama.


### <a name="error-handling"></a>Rukovanje pogreškama

Sljedećim primjerom koda logike aplikacija prikazuje kako biste mogli implementirati pogreškama.

#### <a name="create-error-record"></a>Stvorite zapis o pogrešci

Ovo je izvorni kod logike aplikacije za stvaranje zapisa poslovnog pogreške.

``` json
"actions": {
    "CreateErrorRecord": {
        "metadata": {
        "apiDefinitionUrl": "https://.../swagger/docs/v1",
        "swaggerSource": "website"
        },
        "type": "Http",
        "inputs": {
        "body": {
            "action": "New_Patient",
            "isError": true,
            "crmId": "@{triggerBody()['CRMid']}",
            "patientID": "@{triggerBody()['CRMid']}",
            "message": "@{body('Create_NewPatientRecord')['message']}",
            "providerId": "@{triggerBody()['providerId']}",
            "severity": 4,
            "source": "@{actions('Create_NewPatientRecord')['inputs']['body']}",
            "statusCode": "@{int(outputs('Create_NewPatientRecord')['statusCode'])}",
            "salesforceId": "",
            "update": false
        },
        "method": "post",
        "uri": "https://.../api/CrMtoSfError"
        },
        "runAfter":
        {
            "Create_NewPatientRecord": ["Failed" ]
        }
    }
}          
```

#### <a name="insert-error-into-documentdb--request"></a>Umetanje pogreške u DocumentDB – zahtjev

``` json

{
    "uri": "https://.../api/CrMtoSfError",
    "method": "post",
    "body": {
        "action": "New_Patient",
        "isError": true,
        "crmId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "patientId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "message": "Salesforce failed to complete task: Message: duplicate value found: Account_ID_MED__c duplicates value on record with id: 001U000001c83gK",
        "providerId": "",
        "severity": 4,
        "salesforceId": "",
        "update": false,
        "source": "{\"Account_Class_vod__c\":\"PRAC\",\"Account_Status_MED__c\":\"I\",\"CRM_HUB_ID__c\":\"6b115f6d-a7ee-e511-80f5-3863bb2eb2d0\",\"Credentials_vod__c\",\"DTC_ID_MED__c\":\"\",\"Fax\":\"\",\"FirstName\":\"A\",\"Gender_vod__c\":\"\",\"IMS_ID__c\":\"\",\"LastName\":\"BAILEY\",\"MasterID_mp__c\":\"\",\"C_ID_MED__c\":\"851588\",\"Middle_vod__c\":\"\",\"NPI_vod__c\":\"\",\"PDRP_MED__c\":false,\"PersonDoNotCall\":false,\"PersonEmail\":\"\",\"PersonHasOptedOutOfEmail\":false,\"PersonHasOptedOutOfFax\":false,\"PersonMobilePhone\":\"\",\"Phone\":\"\",\"Practicing_Specialty__c\":\"FM - FAMILY MEDICINE\",\"Primary_City__c\":\"\",\"Primary_State__c\":\"\",\"Primary_Street_Line2__c\":\"\",\"Primary_Street__c\":\"\",\"Primary_Zip__c\":\"\",\"RecordTypeId\":\"012U0000000JaPWIA0\",\"Request_Date__c\":\"2016-06-10T22:31:55.9647467Z\",\"ONY_ID__c\":\"\",\"Specialty_1_vod__c\":\"\",\"Suffix_vod__c\":\"\",\"Website\":\"\"}",
        "statusCode": "400"
    }
}
```

#### <a name="insert-error-into-documentdb--response"></a>Umetanje pogreške DocumentDB – odgovor


``` json
{
    "statusCode": 200,
    "headers": {
        "Pragma": "no-cache",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:31:57 GMT",
        "Server": "Microsoft-IIS/8.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "1561",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "id": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0-1465597917",
        "_rid": "sQx2APhVzAA8AAAAAAAAAA==",
        "_self": "dbs/sQx2AA==/colls/sQx2APhVzAA=/docs/sQx2APhVzAA8AAAAAAAAAA==/",
        "_ts": 1465597912,
        "_etag": "\"0c00eaac-0000-0000-0000-575b3fdc0000\"",
        "prescriberId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "timestamp": "2016-06-10T22:31:57.3651027Z",
        "action": "New_Patient",
        "salesforceId": "",
        "update": false,
        "body": "CRM failed to complete task: Message: duplicate value found: CRM_HUB_ID__c duplicates value on record with id: 001U000001c83gK",
        "source": "{\"Account_Class_vod__c\":\"PRAC\",\"Account_Status_MED__c\":\"I\",\"CRM_HUB_ID__c\":\"6b115f6d-a7ee-e511-80f5-3863bb2eb2d0\",\"Credentials_vod__c\":\"DO - Degree level is DO\",\"DTC_ID_MED__c\":\"\",\"Fax\":\"\",\"FirstName\":\"A\",\"Gender_vod__c\":\"\",\"IMS_ID__c\":\"\",\"LastName\":\"BAILEY\",\"MterID_mp__c\":\"\",\"Medicis_ID_MED__c\":\"851588\",\"Middle_vod__c\":\"\",\"NPI_vod__c\":\"\",\"PDRP_MED__c\":false,\"PersonDoNotCall\":false,\"PersonEmail\":\"\",\"PersonHasOptedOutOfEmail\":false,\"PersonHasOptedOutOfFax\":false,\"PersonMobilePhone\":\"\",\"Phone\":\"\",\"Practicing_Specialty__c\":\"FM - FAMILY MEDICINE\",\"Primary_City__c\":\"\",\"Primary_State__c\":\"\",\"Primary_Street_Line2__c\":\"\",\"Primary_Street__c\":\"\",\"Primary_Zip__c\":\"\",\"RecordTypeId\":\"012U0000000JaPWIA0\",\"Request_Date__c\":\"2016-06-10T22:31:55.9647467Z\",\"XXXXXXX\":\"\",\"Specialty_1_vod__c\":\"\",\"Suffix_vod__c\":\"\",\"Website\":\"\"}",
        "code": 400,
        "errors": null,
        "isError": true,
        "severity": 4,
        "notes": null,
        "resolved": 0
        }
}
```

#### <a name="salesforce-error-response"></a>Odgovor pogreška Salesforce

``` json
{
    "statusCode": 400,
    "headers": {
        "Pragma": "no-cache",
        "x-ms-request-id": "3e8e4884-288e-4633-972c-8271b2cc912c",
        "X-Content-Type-Options": "nosniff",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:31:56 GMT",
        "Set-Cookie": "ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1",
        "Server": "Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "205",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "status": 400,
        "message": "Salesforce failed to complete task: Message: duplicate value found: Account_ID_MED__c duplicates value on record with id: 001U000001c83gK",
        "source": "Salesforce.Common",
        "errors": []
    }
}

```

### <a name="returning-the-response-back-to-the-parent-logic-app"></a>Vraćanje odgovor vratite se u aplikaciju logike nadređenog

Nakon što ste odgovor, možete je proslijediti vratite se u aplikaciju logike nadređenog.

#### <a name="return-success-response-to-the-parent-logic-app"></a>Vraćanje uspjeh odgovor na aplikaciju logike nadređenog

``` json
"SuccessResponse": {
    "runAfter":
        {
            "UpdateNew_CRMPatientResponse": ["Succeeded"]
        },
    "inputs": {
        "body": {
            "status": "Success"
    },
    "headers": {
    "   Content-type": "application/json",
        "x-ms-date": "@utcnow()"
    },
    "statusCode": 200
    },
    "type": "Response"
}
```

#### <a name="return-error-response-to-the-parent-logic-app"></a>Vraćanje odgovor pogreška aplikaciju logike nadređenog

``` json
"ErrorResponse": {
    "runAfter":
        {
            "Create_NewPatientRecord": ["Failed"]
        },
    "inputs": {
        "body": {
            "status": "BadRequest"
        },
        "headers": {
            "Content-type": "application/json",
            "x-ms-date": "@utcnow()"
        },
        "statusCode": 400
    },
    "type": "Response"
}

```


## <a name="documentdb-repository-and-portal"></a>Spremište DocumentDB i portal

Naš rješenje dodati dodatne mogućnosti uz [DocumentDB](https://azure.microsoft.com/services/documentdb).

### <a name="error-management-portal"></a>Portal za upravljanje pogreške

Da biste pogledali pogreške, možete stvoriti web-aplikaciju programa MVC da biste prikazali pogreške zapise iz DocumentDB. **Popis**, **pojedinosti**, **Uređivanje**i **Brisanje** operacije nalaze se u trenutnoj verziji.

> [AZURE.NOTE]Uređivanje operacija: DocumentDB ne Zamijeni cijelog dokumenta.
> Zapisi koji se prikazuju na **popisu** i **Detalji** prikazi su samo uzorka. Nisu stvarni pacijenta obveza zapisa.

Slijede primjeri naše MVC detalje aplikacije stvorene pomoću prethodno opisan način.

#### <a name="error-management-list"></a>Popis pogrešaka upravljanja

![Popis pogrešaka](./media/app-service-logic-scenario-error-and-exception-handling/errorlist.png)

#### <a name="error-management-detail-view"></a>Pogreške upravljanja detaljni prikaz

![Detalje o pogrešci](./media/app-service-logic-scenario-error-and-exception-handling/errordetails.png)

### <a name="log-management-portal"></a>Portal za upravljanje zapisnika

Da biste pogledali zapisnike, također koju smo stvorili web-aplikaciju programa MVC.  Slijede primjeri naše MVC detalje aplikacije stvorene pomoću prethodno opisan način.

#### <a name="sample-log-detail-view"></a>Prikaz detalja za uzorak zapisnika

![Prikaz detalja zapisnika](./media/app-service-logic-scenario-error-and-exception-handling/samplelogdetail.png)

### <a name="api-app-details"></a>Detalje aplikacije API-JA

#### <a name="logic-apps-exception-management-api"></a>Logika aplikacije iznimku upravljanje API-JA

Naša Otvori izvor logike aplikacije iznimku upravljanje API aplikacija nudi sljedeće funkcije.

Postoje dvije kontrolera:

- **ErrorController** umeće pogrešku zapis (dokument) u zbirci DocumentDB.
- **LogController** Umeće zapis zapisnika (dokument) u zbirci DocumentDB.

> [AZURE.TIP] Korištenje obje kontrolera `async Task<dynamic>` operacije. Time se omogućuje operacije se riješiti tijekom rada, pa ćemo stvoriti shemi DocumentDB u tijelu postupak.

Svaki dokument u DocumentDB mora imati jedinstveni ID-a. Smo `PatientId` i dodavanje vremenske oznake koje se pretvara u Unix vremenske oznake vrijednost (double). Ne možemo skratiti da biste uklonili decimalni vrijednost.

Možete pogledati šifru izvora naš kontrolera pogreške API [iz GitHub](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi/blob/master/Logic App Exception Management API/Controllers/ErrorController.cs).

Ne možemo poziv na API iz logike aplikacije pomoću sljedeće sintakse.

``` json
 "actions": {
        "CreateErrorRecord": {
          "metadata": {
            "apiDefinitionUrl": "https://.../swagger/docs/v1",
            "swaggerSource": "website"
          },
          "type": "Http",
          "inputs": {
            "body": {
              "action": "New_Patient",
              "isError": true,
              "crmId": "@{triggerBody()['CRMid']}",
              "prescriberId": "@{triggerBody()['CRMid']}",
              "message": "@{body('Create_NewPatientRecord')['message']}",
              "salesforceId": "@{triggerBody()['salesforceID']}",
              "severity": 4,
              "source": "@{actions('Create_NewPatientRecord')['inputs']['body']}",
              "statusCode": "@{int(outputs('Create_NewPatientRecord')['statusCode'])}",
              "update": false
            },
            "method": "post",
            "uri": "https://.../api/CrMtoSfError"
          },
          "runAfter": {
              "Create_NewPatientRecord": ["Failed"]
            }
        }
 }
```

Izraz u prethodnom primjeru kod je provjera stanja *Create_NewPatientRecord* **nije uspjelo**.

## <a name="summary"></a>Sažetak

- Jednostavno možete implementirati zapisnika i u aplikaciji logike rukovanja pogreškama.
- DocumentDB možete koristiti kao spremište zapisa zapisnika i pogreške (dokumenti).
- MVC možete koristiti za stvaranje portala za prikaz zapisnika i pogreške zapisa.

### <a name="source-code"></a>Izvorni kod
Izvorni kod za upravljanje iznimku logike aplikacije API aplikacije dostupan je u ovaj [GitHub spremište](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi "Logike aplikacije iznimku upravljanje API").


## <a name="next-steps"></a>Daljnji koraci
- [Prikaz više primjera logike aplikacije i scenarijima](app-service-logic-examples-and-scenarios.md)
- [Informirajte se o aplikacijama logike Alati za nadzor](app-service-logic-monitor-your-logic-apps.md)
- [Stvaranje predloška automatiziranog implementacije logike aplikacije](app-service-logic-create-deploy-template.md)
