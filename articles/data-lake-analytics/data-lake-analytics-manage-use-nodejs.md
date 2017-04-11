<properties
   pageTitle="Upravljanje Azure podataka Lake analize korištenja Azure SDK za Node.js | Azure"
   description="Informirajte se o upravljanju analize podataka Lake račune, izvora podataka, zadatke i korisnici koji koriste Azure SDK za Node.js"
   services="data-lake-analytics"
   documentationCenter=""
   authors="edmacauley"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="manage-azure-data-lake-analytics-using-azure-sdk-for-nodejs"></a>Upravljanje Azure podataka Lake analize korištenja Azure SDK za Node.js


[AZURE.INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Azure SDK Node.js može se koristiti za upravljanje računima Lake analize podataka za Azure, zadatke i katalozi. Da biste vidjeli upravljanje teme pomoću drugih alata, kliknite odaberite karticu iznad.

Desno sada podržava:

  *  **Verzija Node.js: 0.10.0 ili noviji**
  *  **Verzija REST API-JA za račun: 2015-10-01 – pregled**
  *  **Verzija REST API-JA za katalog: 2015-10-01 – pregled**
  *  **Verzija REST API-JA za posao:-03-20 – pretpregled 2016**

## <a name="features"></a>Značajke

- Upravljanje računom: Stvaranje početak, popisa, ažuriranje i brisanje.
- Zadatak upravljanja: slanje, se, popisa, Odustani.
- Upravljanje kataloga: početak, popisa, stvaranje (tajne), ažurirajte (tajne), brisanje (tajne).

## <a name="how-to-install"></a>Kako instalirati

```bash
npm install azure-arm-datalake-analytics
```

## <a name="authenticate-using-azure-active-directory"></a>Provjera valjanosti putem Azure Active Directory

 ```javascript
 var msrestAzure = require('ms-rest-azure');
 //user authentication
 var credentials = new msRestAzure.UserTokenCredentials('your-client-id', 'your-domain', 'your-username', 'your-password', 'your-redirect-uri');
 //service principal authentication
 var credentials = new msRestAzure.ApplicationTokenCredentials('your-client-id', 'your-domain', 'your-secret');
 ```

## <a name="create-the-data-lake-analytics-client"></a>Stvaranje klijentskog programa analize podataka Lake

```javascript
var adlaManagement = require("azure-arm-datalake-analytics");
var acccountClient = new adlaManagement.DataLakeAnalyticsAccountClient(credentials, 'your-subscription-id');
var jobClient = new adlaManagement.DataLakeAnalyticsJobClient(credentials, 'azuredatalakeanalytics.net');
var catalogClient = new adlaManagement.DataLakeAnalyticsCatalogClient(credentials, 'azuredatalakeanalytics.net');
```

## <a name="create-a-data-lake-analytics-account"></a>Stvaranje računa analize podataka Lake

```javascript
var util = require('util');
var resourceGroupName = 'testrg';
var accountName = 'testadlaacct';
var location = 'eastus2';

// A Data Lake Store account must already have been created to create
// a Data Lake Analytics account. See the Data Lake Store readme for
// information on doing so. For now, we assume one exists already.
var datalakeStoreAccountName = 'existingadlsaccount';

// account object to create
var accountToCreate = {
  tags: {
    testtag1: 'testvalue1',
    testtag2: 'testvalue2'
  },
  name: accountName,
  location: location,
  properties: {
    defaultDataLakeStoreAccount: datalakeStoreAccountName,
    dataLakeStoreAccounts: [
      {
        name: datalakeStoreAccountName
      }
    ]
  }
};

client.account.create(resourceGroupName, accountName, accountToCreate, function (err, result, request, response) {
  if (err) {
    console.log(err);
    /*err has reference to the actual request and response, so you can see what was sent and received on the wire.
      The structure of err looks like this:
      err: {
        code: 'Error Code',
        message: 'Error Message',
        body: 'The response body if any',
        request: reference to a stripped version of http request
        response: reference to a stripped version of the response
      }
    */
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="get-a-list-of-jobs"></a>Dobit ćete popis zadataka

```javascript
var util = require('util');
var accountName = 'testadlaacct';
jobClient.job.list(accountName, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="get-a-list-of-databases-in-the-data-lake-analytics-catalog"></a>Dobit ćete popis baze podataka u katalogu podataka Lake Analytics
```javascript
var util = require('util');
var accountName = 'testadlaacct';
catalogClient.catalog.listDatabases(accountName, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="see-also"></a>Vidi također

- [Microsoft Azure SDK Node.js](https://github.com/azure/azure-sdk-for-node)
- [Microsoft Azure SDK Node.js - Upravljanje spremištem Lake podataka](https://github.com/Azure/azure-sdk-for-node/tree/autorest/lib/services/dataLake.Store)
