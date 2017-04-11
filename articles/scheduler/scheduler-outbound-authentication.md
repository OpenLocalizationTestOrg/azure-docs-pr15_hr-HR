<properties
 pageTitle="Raspored izlaznu provjeru autentičnosti"
 description="Raspored izlaznu provjeru autentičnosti"
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="08/15/2016"
 ms.author="deli"/>

# <a name="scheduler-outbound-authentication"></a>Raspored izlaznu provjeru autentičnosti

Raspored zadataka možda morati pozivanje servisima koje zahtijevaju provjeru autentičnosti. Na taj način, pod nazivom servisa možete odrediti Ako raspored posao možete pristupiti njegovih resursa. Neke od tih servisa obuhvaćaju drugih servisa Azure, Salesforce.com, Facebook i sigurnim prilagođene web-mjestima.

## <a name="adding-and-removing-authentication"></a>Dodavanje i uklanjanje provjere autentičnosti

Dodavanje provjere autentičnosti za posao rasporeda vrlo je jednostavno – dodavanje JSON podređeni element `authentication` da biste na `request` element pri stvaranju ili ažuriranje posao. Tajne proslijediti servis rasporeda u zahtjevu za STAVI, ZAKRPU ili objavu – kao dio sustava `authentication` objekta – vraćaju se nikad ne u odgovore. U odgovore, tajnu podataka postavljena na null ili može imati javno token koji predstavlja čija je autentičnost provjerena entitet.

Da biste uklonili provjeru autentičnosti, STAVITE ili ZAKRPU posao izričito, postavka u `authentication` objekt na null. Nećete vidjeti sve provjere autentičnosti svojstva vratiti u odgovoru.

Trenutno su na samo podržane vrste provjere autentičnosti na `ClientCertificate` modela (za korištenje klijentskih potvrda SSL/TLS), u `Basic` modela (za osnovnu provjeru autentičnosti), a `ActiveDirectoryOAuth` modela (za Active Directory OAuth provjeru autentičnosti.)

## <a name="request-body-for-clientcertificate-authentication"></a>Tijelo zahtjev za provjeru autentičnosti ClientCertificate

Prilikom dodavanja pomoću provjere autentičnosti na `ClientCertificate` modela, navedite sljedeće dodatne elemente u tijelu zahtjeva.  

|Element|Opis|
|:---|:---|
|_Provjera autentičnosti (nadređeni element)_|Objekt provjere autentičnosti za korištenje SSL certifikata klijenta.|
|_Vrsta_|Obavezan. Vrsta provjere autentičnosti. Za SSL certifikate klijent, ta vrijednost mora biti `ClientCertificate`.|
|_pfx_|Obavezan. Base64 kodirani sadržaj PFX datoteke.|
|_lozinke_|Obavezan. Lozinka za pristup datoteci PFX.|


## <a name="response-body-for-clientcertificate-authentication"></a>Tijelo odgovora za provjeru autentičnosti ClientCertificate

Ako se zahtjev pošalje s informacijama za provjeru autentičnosti, odgovor sadrži sljedeće elemente vezane uz provjeru autentičnosti.

|Element |Opis |
|:--|:--|
|_Provjera autentičnosti (nadređeni element)_ |Objekt provjere autentičnosti za korištenje SSL certifikata klijenta.|
|_Vrsta_ |Vrsta provjere autentičnosti. Za SSL certifikate klijent, je vrijednost `ClientCertificate`.|
|_certificateThumbprint_ |Otisak prsta potvrde.|
|_certificateSubjectName_ |Predmet Razlikovni naziv certifikata.|
|_certificateExpiration_ |Datum isteka certifikata.|

## <a name="sample-rest-request-for-clientcertificate-authentication"></a>Zahtjev za OSTALE uzorka za provjeru autentičnosti ClientCertificate

```
PUT https://management.azure.com/subscriptions/1fe0abdf-581e-4dfe-9ec7-e5cb8e7b205e/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "type": "clientcertificate",
          "password": "password",
          "pfx": "pfx key"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
  }
}
```

## <a name="sample-rest-response-for-clientcertificate-authentication"></a>Odgovor OSTATAK uzorka za provjeru autentičnosti ClientCertificate

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 858
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: 56c7b40e-721a-437e-88e6-f68562a73aa8
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 1075219e-e879-4030-bc81-094e54fbabce
x-ms-routing-request-id: WESTUS:20160316T190424Z:1075219e-e879-4030-bc81-094e54fbabce
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:04:23 GMT

{
  "id": "/subscriptions/1fe0abdf-581e-4dfe-9ec7-e5cb8e7b205e/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
  "type": "Microsoft.Scheduler/jobCollections/jobs",
  "name": "southeastasiajc/httpjob",
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "certificateThumbprint": "88105CG9DF9ADE75B835711D899296CB217D7055",
          "certificateExpiration": "2021-01-01T07:00:00Z",
          "certificateSubjectName": "CN=Scheduler Mgmt",
          "type": "ClientCertificate"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
    "status": {
      "nextExecutionTime": "2016-03-16T19:05:00Z",
      "executionCount": 0,
      "failureCount": 0,
      "faultedCount": 0
    }
  }
}
```

## <a name="request-body-for-basic-authentication"></a>Tijelo zahtjev za osnovnu provjeru autentičnosti

Prilikom dodavanja pomoću provjere autentičnosti na `Basic` modela, navedite sljedeće dodatne elemente u tijelu zahtjeva.

|Element|Opis|
|:--|:--|
|_Provjera autentičnosti (nadređeni element)_ |Objekt provjere autentičnosti za korištenje osnovnu provjeru autentičnosti.|
|_Vrsta_ |Obavezan. Vrsta provjere autentičnosti. Osnovna provjera autentičnosti vrijednosti mora biti `Basic`.|
|_korisničko ime_ |Obavezan. Korisničko ime za provjeru autentičnosti.|
|_lozinke_ |Obavezan. Lozinka za provjeru autentičnosti.|

## <a name="response-body-for-basic-authentication"></a>Odgovor tijela za osnovnu provjeru autentičnosti

Ako se zahtjev pošalje s informacijama za provjeru autentičnosti, odgovor sadrži sljedeće elemente vezane uz provjeru autentičnosti.

|Element|Opis|
|:--|:--|
|_Provjera autentičnosti (nadređeni element)_ |Objekt provjere autentičnosti za korištenje osnovnu provjeru autentičnosti.|
|_Vrsta_ |Vrsta provjere autentičnosti. Osnovna provjera autentičnosti vrijednost je `Basic`.|
|_korisničko ime_ |Čija je autentičnost provjerena korisničko ime.|

## <a name="sample-rest-request-for-basic-authentication"></a>Zahtjev za OSTALE uzorka za osnovnu provjeru autentičnosti

```
PUT https://management.azure.com/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Length: 562
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "type": "basic",
          "username": "user",
          "password": "password"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
  }
}
```

## <a name="sample-rest-response-for-basic-authentication"></a>Odgovor OSTATAK uzorka za osnovnu provjeru autentičnosti

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 701
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: a2dcb9cd-1aea-4887-8893-d81273a8cf04
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 7816f222-6ea7-468d-b919-e6ddebbd7e95
x-ms-routing-request-id: WESTUS:20160316T190506Z:7816f222-6ea7-468d-b919-e6ddebbd7e95
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:05:06 GMT

{  
   "id":"/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
   "type":"Microsoft.Scheduler/jobCollections/jobs",
   "name":"southeastasiajc/httpjob",
   "properties":{  
      "startTime":"2015-05-14T14:10:00Z",
      "action":{  
         "request":{  
            "uri":"https://mywebserviceendpoint.com",
            "method":"GET",
            "headers":{  
               "x-ms-version":"2013-03-01"
            },
            "authentication":{  
               "username":"user1",
               "type":"Basic"
            }
         },
         "type":"http"
      },
      "recurrence":{  
         "frequency":"minute",
         "endTime":"2016-04-10T08:00:00Z",
         "interval":1
      },
      "state":"enabled",
      "status":{  
         "nextExecutionTime":"2016-03-16T19:06:00Z",
         "executionCount":0,
         "failureCount":0,
         "faultedCount":0
      }
   }
}
```

## <a name="request-body-for-activedirectoryoauth-authentication"></a>Tijelo zahtjev za provjeru autentičnosti ActiveDirectoryOAuth

Prilikom dodavanja pomoću provjere autentičnosti na `ActiveDirectoryOAuth` modela, navedite sljedeće dodatne elemente u tijelu zahtjeva.

|Element |Opis |
|:--|:--|
|_Provjera autentičnosti (nadređeni element)_ |Objekt provjere autentičnosti za korištenje ActiveDirectoryOAuth provjeru autentičnosti.|
|_Vrsta_ |Obavezan. Vrsta provjere autentičnosti. Za provjeru autentičnosti ActiveDirectoryOAuth, ta vrijednost mora biti `ActiveDirectoryOAuth`.|
|_klijent_ |Obavezan. Identifikator klijenta za Azure AD klijenta.|
|_ciljne skupine_ |Obavezan. Postavljen je na https://management.core.windows.net/.|
|_clientId_ |Obavezan. Navedite identifikator klijenta za Azure AD aplikaciju.|
|_tajna_ |Obavezan. Tajna klijenta koji zahtijeva token.|

### <a name="determining-your-tenant-identifier"></a>Određivanje vaš klijent identifikator

Identifikator klijenta za Azure AD klijenta možete pronaći tako da pokrenete `Get-AzureAccount` u Azure PowerShell.

## <a name="response-body-for-activedirectoryoauth-authentication"></a>Tijelo odgovora za provjeru autentičnosti ActiveDirectoryOAuth

Ako se zahtjev pošalje s informacijama za provjeru autentičnosti, odgovor sadrži sljedeće elemente vezane uz provjeru autentičnosti.

|Element |Opis |
|:--|:--|
|_Provjera autentičnosti (nadređeni element)_ |Objekt provjere autentičnosti za korištenje ActiveDirectoryOAuth provjeru autentičnosti.|
|_Vrsta_ |Vrsta provjere autentičnosti. Za provjeru autentičnosti ActiveDirectoryOAuth, je vrijednost `ActiveDirectoryOAuth`.|
|_klijent_ |Identifikator klijenta za Azure AD klijenta. |
|_ciljne skupine_ |Postavljen je na https://management.core.windows.net/.|
|_clientId_ |Identifikator klijenta za Azure AD aplikacije.|

## <a name="sample-rest-request-for-activedirectoryoauth-authentication"></a>Zahtjev za OSTALE uzorka za provjeru autentičnosti ActiveDirectoryOAuth

```
PUT https://management.azure.com/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Length: 757
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "tenant":"microsoft.onmicrosoft.com",
          "audience":"https://management.core.windows.net/",
          "clientId":"dc23e764-9be6-4a33-9b9a-c46e36f0c137",
          "secret": "G6u071r8Gjw4V4KSibnb+VK4+tX399hkHaj7LOyHuj5=",
          "type":"ActiveDirectoryOAuth"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
  }
}
```

## <a name="sample-rest-response-for-activedirectoryoauth-authentication"></a>Odgovor OSTATAK uzorka za provjeru autentičnosti ActiveDirectoryOAuth

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 885
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: 86d8e9fd-ac0d-4bed-9420-9baba1af3251
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 5183bbf4-9fa1-44bb-98c6-6872e3f2e7ce
x-ms-routing-request-id: WESTUS:20160316T191003Z:5183bbf4-9fa1-44bb-98c6-6872e3f2e7ce
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:10:02 GMT

{  
   "id":"/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
   "type":"Microsoft.Scheduler/jobCollections/jobs",
   "name":"southeastasiajc/httpjob",
   "properties":{  
      "startTime":"2015-05-14T14:10:00Z",
      "action":{  
         "request":{  
            "uri":"https://mywebserviceendpoint.com",
            "method":"GET",
            "headers":{  
               "x-ms-version":"2013-03-01"
            },
            "authentication":{  
               "tenant":"microsoft.onmicrosoft.com",
               "audience":"https://management.core.windows.net/",
               "clientId":"dc23e764-9be6-4a33-9b9a-c46e36f0c137",
               "type":"ActiveDirectoryOAuth"
            }
         },
         "type":"http"
      },
      "recurrence":{  
         "frequency":"minute",
         "endTime":"2016-04-10T08:00:00Z",
         "interval":1
      },
      "state":"enabled",
      "status":{  
         "lastExecutionTime":"2016-03-16T19:10:00.3762123Z",
         "nextExecutionTime":"2016-03-16T19:11:00Z",
         "executionCount":5,
         "failureCount":5,
         "faultedCount":1
      }
   }
}
```

## <a name="see-also"></a>Vidi također


 [Što je raspored?](scheduler-intro.md)

 [Azure raspored koncepte, terminologija i entitet hijerarhije](scheduler-concepts-terms.md)

 [Početak rada s raspored na portalu za Azure](scheduler-get-started-portal.md)

 [Tarife i naplata u rasporedu Azure](scheduler-plans-billing.md)

 [Referenca za Azure raspored REST API-JA](https://msdn.microsoft.com/library/mt629143)

 [Azure referenca cmdleta ljuske PowerShell za raspored](scheduler-powershell-reference.md)

 [Azure visoku dostupnost raspored i pouzdanosti](scheduler-high-availability-reliability.md)

 [Azure ograničenja raspored, zadanih vrijednosti i šifre pogreške](scheduler-limits-defaults-errors.md)
