<properties 
   pageTitle="Početak rada s podacima Lake analize pomoću REST API-JA | Microsoft Azure" 
   description="Korištenje WebHDFS REST API-ji za izvođenje operacije na analize podataka Lake" 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="10/19/2016"
   ms.author="jgao"/>

# <a name="get-started-with-azure-data-lake-analytics-using-rest-apis"></a>Početak rada s Azure podataka Lake analize pomoću REST API-ji

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Saznajte kako koristiti WebHDFS REST API-ji i podataka Lake analize REST API-ji za upravljanje računima analize podataka Lake, zadacima i kataloga. 

## <a name="prerequisites"></a>Preduvjeti

- **Mogući Azure pretplate**. Pogledajte [Početak Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/).

- **Stvaranje aplikacije komponente Azure Active Directory**. Pomoću aplikacije za Azure AD analize podataka Lake aplikacijom Azure AD za provjeru autentičnosti. Postoje različite postupke za provjeru s Azure AD koje su **provjere autentičnosti za krajnjeg korisnika** ili **provjere autentičnosti servis za servis**. Upute i dodatne informacije o provjeri autentičnosti potražite u članku [provjeru s podacima Lake analize pomoću servisa Azure Active Directory](../data-lake-store/data-lake-store-authenticate-using-active-directory.md).

- [cURL](http://curl.haxx.se/). U ovom se članku koristi zakretanja da bismo pokazali kako REST API-JA pozive protiv analize podataka Lake računa.

## <a name="authenticate-with-azure-active-directory"></a>Provjeru s Azure Active Directory

Postoje dva načina provjere autentičnosti s Azure Active Directory.

### <a name="end-user-authentication-interactive"></a>Provjera autentičnosti krajnjeg korisnika (interaktivne)

Pomoću ove metode aplikacije traži od korisnika da se prijavite i sve operacije se izvode u kontekstu korisnika. 

Slijedite ove korake za interaktivno provjere autentičnosti:

1. Putem aplikacije, preusmjeravanje korisnika na sljedeći URL:

        https://login.microsoftonline.com/<TENANT-ID>/oauth2/authorize?client_id=<CLIENT-ID>&response_type=code&redirect_uri=<REDIRECT-URI>

    >[AZURE.NOTE] \<PREUSMJERAVANJE URI > treba kodirati za korištenje u URL-a. Tako, da biste postigli https://localhost, koristite `https%3A%2F%2Flocalhost`)

    Za potrebe ovog praktičnog vodiča možete zamjena vrijednosti rezervirano mjesto u URL-iznad i zalijepiti ga u adresnoj traci preglednika web. Će biti preusmjereni za provjeru autentičnosti pomoću svoje Azure prijava. Kada ste uspješno prijaviti, odgovor se prikazuje u adresnu traku web-preglednika. Odgovor bit će u sljedećem obliku:
        
        http://localhost/?code=<AUTHORIZATION-CODE>&session_state=<GUID>

2. Snimite autorizacije koda iz odgovor. Za ovaj vodič kopirajte kod za provjeru autentičnosti iz adresne trake web-preglednika i prenesite ga u zahtjevu za objavu krajnju točku tokena kao što je prikazano u nastavku:

        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token \
        -F redirect_uri=<REDIRECT-URI> \
        -F grant_type=authorization_code \
        -F resource=https://management.core.windows.net/ \
        -F client_id=<CLIENT-ID> \
        -F code=<AUTHORIZATION-CODE>

    >[AZURE.NOTE] U ovom slučaju na \<PREUSMJERAVANJE URI > potrebna šifrirana.

3. Je odgovor JSON objekt koji sadrži token za pristup (npr., `"access_token": "<ACCESS_TOKEN>"`) i osvježavanje token (npr., `"refresh_token": "<REFRESH_TOKEN>"`). Aplikacije koristi token za pristup prilikom pristupanja Lake spremišta podataka za Azure i token Osvježi da biste dobili novi token pristup kada istekne token za pristup.

        {"token_type":"Bearer","scope":"user_impersonation","expires_in":"3599","expires_on":"1461865782","not_before": "1461861882","resource":"https://management.core.windows.net/","access_token":"<REDACTED>","refresh_token":"<REDACTED>","id_token":"<REDACTED>"}

4.  Kada istekne token za pristup možete zatražiti novi token za pristup pomoću token osvježavanja kao što je prikazano u nastavku:

         curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
            -F grant_type=refresh_token \
            -F resource=https://management.core.windows.net/ \
            -F client_id=<CLIENT-ID> \
            -F refresh_token=<REFRESH-TOKEN>
 
Dodatne informacije o provjere autentičnosti korisnika interaktivne potražite u članku [autorizacija kod dodijeliti tijek](https://msdn.microsoft.com/library/azure/dn645542.aspx).

### <a name="service-to-service-authentication-non-interactive"></a>Provjera autentičnosti servisa servisa (koji nije interaktivan)

Pomoću ove metode aplikacije sadrži vlastitu vjerodajnice za izvođenje operacije. To, morate izdati zahtjev za objavu poput prikazano u nastavku: 

    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

Izlaz iz zahtjev neće sadržavati stavku oznaku ovlaštenja (označena tako da `access-token` u nastavku Izlaz) koji će se naknadno proći s pozive REST API-JA. Spremanje token za provjeru autentičnosti u tekstnoj datoteci; trebat će vam u nastavku ovog članka.

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

U ovom se članku koristi na **koji nije interaktivan** pristup. Dodatne informacije o koji nije interaktivan (servis za servis pozivi) potražite u članku [servis za pozive za servis pomoću vjerodajnica](https://msdn.microsoft.com/library/azure/dn645543.aspx).
## <a name="create-a-data-lake-analytics-account"></a>Stvaranje računa analize podataka Lake

Da biste mogli stvarati analize podataka Lake računa morate stvoriti grupu resursa Azure i spremišta podataka Lake račun.  Potražite u članku [Stvaranje računa spremišta Lake podataka](../data-lake-store/data-lake-store-get-started-rest-api.md#create-a-data-lake-store-account).

Sljedeća naredba zakretanja pokazuje kako stvoriti račun:

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<NewAzureDataLakeAnalyticsAccountName>?api-version=2016-11-01 -d@"C:\tutorials\adla\CreateDataLakeAnalyticsAccountRequest.json"

Zamjena \< `REDACTED` \> s ovlaštenja, \< `AzureSubscriptionID` \> ID svoje pretplate \< `AzureResourceGroupName` \> postojeću grupu resursa Azure naziva i \< `NewAzureDataLakeAnalyticsAccountName` \> pod novim nazivom podaci Lake analize računa. Opseg zahtjev za tu naredbu nalazi se u datoteci **CreateDatalakeAnalyticsAccountRequest.json** kojemu je na `-d` parametar iznad. Sadržaj datoteke input.json otprilike ovako:

    {  
        "location": "East US 2",  
        "name": "myadla1004",  
        "tags": {},  
        "properties": {  
            "defaultDataLakeStoreAccount": "my1004store",  
            "dataLakeStoreAccounts": [  
                {  
                    "name": "my1004store"  
                }     
            ]
        }  
    }  


## <a name="list-data-lake-analytics-accounts-in-a-subscription"></a>Popis podataka Lake analize računi u pretplatu

Sljedeća naredba zakretanja pokazuje kako popis računa u pretplatu:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/providers/Microsoft.DataLakeAnalytics/Accounts?api-version=2016-11-01

Zamjena \< `REDACTED` \> s ovlaštenja, \< `AzureSubscriptionID` \> koristeći ID pretplate. Izlaz slično je:

    {
        "value": [
            {
            "properties": {
                "provisioningState": "Succeeded",
                "state": "Active",
                "endpoint": "myadla0831.azuredatalakeanalytics.net",
                "accountId": "21e74660-0941-4880-ae72-b143c2615ea9",
                "creationTime": "2016-09-01T12:49:12.7451428Z",
                "lastModifiedTime": "2016-09-01T12:49:12.7451428Z"
            },
            "location": "East US 2",
            "tags": {},
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla0831rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla0831",
            "name": "myadla0831",
            "type": "Microsoft.DataLakeAnalytics/accounts"
            },
            {
            "properties": {
                "provisioningState": "Succeeded",
                "state": "Active",
                "endpoint": "myadla1004.azuredatalakeanalytics.net",
                "accountId": "3ff9b93b-11c4-43c6-83cc-276292eeb350",
                "creationTime": "2016-10-04T20:46:42.287147Z",
                "lastModifiedTime": "2016-10-04T20:46:42.287147Z"
            },
            "location": "East US 2",
            "tags": {},
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004",
            "name": "myadla1004",
            "type": "Microsoft.DataLakeAnalytics/accounts"
            }
        ]
    }

## <a name="get-information-about-a-data-lake-analytics-account"></a>Informacije o računu za analize podataka Lake

Sljedeća naredba zakretanja prikazuje kako doći do podataka o računu:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<DataLakeAnalyticsAccountName>?api-version=2015-11-01

Zamjena \< `REDACTED` \> s ovlaštenja, \< `AzureSubscriptionID` \> ID svoje pretplate \< `AzureResourceGroupName` \> postojeću grupu resursa Azure naziva i \< `DataLakeAnalyticsAccountName` \> s nazivom postojećeg računa analize podataka Lake. Izlaz slično je:

    {
        "properties": {
            "defaultDataLakeStoreAccount": "my1004store",
            "dataLakeStoreAccounts": [
            {
                "properties": {
                "suffix": "azuredatalakestore.net"
                },
                "name": "my1004store"
            }
            ],
            "provisioningState": "Creating",
            "state": null,
            "endpoint": null,
            "accountId": "3ff9b93b-11c4-43c6-83cc-276292eeb350",
            "creationTime": null,
            "lastModifiedTime": null
        },
        "location": "East US 2",
        "tags": {},
        "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004",
        "name": "myadla1004",
        "type": "Microsoft.DataLakeAnalytics/accounts"
    }

## <a name="list-data-lake-stores-of-a-data-lake-analytics-account"></a>Popis podataka Lake trgovine analize podataka Lake računa

Sljedeća naredba zakretanja pokazuje kako popis služi za pohranu podataka Lake računa:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<DataLakeAnalyticsAccountName>/DataLakeStoreAccounts/?api-version=2016-11-01

Zamjena \< `REDACTED` \> s ovlaštenja, \< `AzureSubscriptionID` \> ID svoje pretplate \< `AzureResourceGroupName` \> postojeću grupu resursa Azure naziva i \< `DataLakeAnalyticsAccountName` \> s nazivom postojećeg računa analize podataka Lake. Izlaz slično je:

    {
        "value": [
            {
            "properties": {
                "suffix": "azuredatalakestore.net"
            },
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004/dataLakeStoreAccounts/my1004store",
            "name": "my1004store",
            "type": "Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts"
            }
        ]
    }

## <a name="submit-u-sql-jobs"></a>Slanje U SQL poslova

Sljedeća naredba zakretanja služi za slanje U SQL posla:

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/Jobs/<NewGUID>?api-version=2016-03-20-preview -d@"C:\tutorials\adla\SubmitADLAJob.json"

Zamjena \< `REDACTED` \> s ovlaštenja, \< `DataLakeAnalyticsAccountName` \> s nazivom postojećeg računa analize podataka Lake. Opseg zahtjev za tu naredbu nalazi se u datoteci **SubmitADLAJob.json** kojemu je na `-d` parametar iznad. Sadržaj datoteke input.json otprilike ovako:

    {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "degreeOfParallelism": 1,
        "priority": 1000,
        "properties": {
            "type": "USql",
            "script": "@searchlog =\n    EXTRACT UserId          int,\n            Start           DateTime,\n            Region          string,\n            Query          
        string,\n            Duration        int?,\n            Urls            string,\n            ClickedUrls     string\n    FROM \"/Samples/Data/SearchLog.tsv\"\n    US
        ING Extractors.Tsv();\n\nOUTPUT @searchlog   \n    TO \"/Output/SearchLog-from-Data-Lake.csv\"\nUSING Outputters.Csv();"
        }
    }

Izlaz slično je:

    {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "myadl@SPI",
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "2016-10-05T13:54:59.9871859+00:00",
        "state": "Compiling",
        "result": "Succeeded",
        "stateAuditRecords": [
            {
            "newState": "New",
            "timeStamp": "2016-10-05T13:54:59.9871859+00:00",
            "details": "userName:myadl@SPI;submitMachine:N/A"
            }
        ],
        "properties": {
            "owner": "myadl@SPI",
            "resources": [],
            "runtimeVersion": "default",
            "rootProcessNodeId": "00000000-0000-0000-0000-000000000000",
            "algebraFilePath": "adl://myadls0831.azuredatalakestore.net/system/jobservice/jobs/Usql/2016/10/05/13/54/8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a/algebra.xml",
            "compileMode": "Semantic",
            "errorSource": "Unknown",
            "totalCompilationTime": "PT0S",
            "totalPausedTime": "PT0S",
            "totalQueuedTime": "PT0S",
            "totalRunningTime": "PT0S",
            "type": "USql"
        }
    }


## <a name="list-u-sql-jobs"></a>Popis U SQL poslova

Sljedeća naredba zakretanja prikazuje način za prikaz popisa zadataka U SQL:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/Jobs?api-version=2016-11-01 

Zamjena \< `REDACTED` \> s ovlaštenja, a \< `DataLakeAnalyticsAccountName` \> s nazivom postojećeg računa analize podataka Lake. 


Izlaz slično je:

    {
    "value": [
        {
        "jobId": "65cf1691-9dbe-43cd-90ed-1cafbfb406fb",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "someone@microsoft.com",
        "account": null,
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "Wed, 05 Oct 2016 13:46:53 GMT",
        "startTime": "Wed, 05 Oct 2016 13:47:33 GMT",
        "endTime": "Wed, 05 Oct 2016 13:48:07 GMT",
        "state": "Ended",
        "result": "Succeeded",
        "errorMessage": null,
        "storageAccounts": null,
        "stateAuditRecords": null,
        "logFilePatterns": null,
        "properties": null
        },
        {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "someoneadl@SPI",
        "account": null,
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "Wed, 05 Oct 2016 13:54:59 GMT",
        "startTime": "Wed, 05 Oct 2016 13:55:43 GMT",
        "endTime": "Wed, 05 Oct 2016 13:56:11 GMT",
        "state": "Ended",
        "result": "Succeeded",
        "errorMessage": null,
        "storageAccounts": null,
        "stateAuditRecords": null,
        "logFilePatterns": null,
        "properties": null
        }
    ],
    "nextLink": null,
    "count": null
    }


## <a name="get-catalog-items"></a>Stavki kataloga

Sljedeća naredba zakretanja prikazuje kako doći do baze podataka iz kataloga:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/catalog/usql/databases?api-version=2016-11-01

Izlaz slično je:

    {
    "@odata.context":"https://myadla0831.azuredatalakeanalytics.net/sqlip/$metadata#databases","value":[
        {
        "computeAccountName":"myadla0831","databaseName":"mytest","version":"f6956327-90b8-4648-ad8b-de3ff09274ea"
        },{
        "computeAccountName":"myadla0831","databaseName":"master","version":"e8bca908-cc73-41a3-9564-e9bcfaa21f4e"
        }
    ]
    }

## <a name="see-also"></a>Vidi također

- Da biste vidjeli složeniji upit, potražite u članku [zapisnika analiza web-mjesta pomoću Azure podataka Lake analize](data-lake-analytics-analyze-weblogs.md).
- Prvi koraci u razvoju aplikacija U SQL, potražite u članku [razviti U – SQL skripte pomoću alata za Lake podataka za Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- Da biste saznali U SQL, potražite u članku [Početak rada s jezikom Azure podataka Lake analize U-SQL](data-lake-analytics-u-sql-get-started.md).
- Upravljanje zadacima, potražite u članku [Upravljanje Lake Analytics za Azure podataka pomoću portala za Azure](data-lake-analytics-manage-use-portal.md).
- Da biste dobili pregled analize podataka Lake, potražite u članku [Pregled Azure podataka Lake analize](data-lake-analytics-overview.md).
- Da biste vidjeli iste praktičnom vodiču pomoću drugih alata, kliknite karticu Birači pri vrhu stranice.
- Da biste se prijavili Dijagnostika informacije potražite u članku [Accessing dijagnostički zapisnici Lake analitičkih podataka za Azure](data-lake-analytics-diagnostic-logs.md)
