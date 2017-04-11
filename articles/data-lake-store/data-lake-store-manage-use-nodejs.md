<properties 
   pageTitle="Početak rada s trgovine Azure podataka Lake pomoću Azure SDK za Node.js | Microsoft Azure"
   description="Saznajte kako koristiti Node.js za rad s računa za spremište Lake podataka i datotečnom sustavu." 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/27/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-azure-sdk-for-nodejs"></a>Početak rada s spremišta Lake podataka za Azure pomoću Azure SDK za Node.js

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST API-JA](data-lake-store-get-started-rest-api.md)
- [Azure EŽA](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)


Saznajte kako koristiti Azure SDK Node.js da biste stvorili Lake spremišta podataka za Azure račun i izvođenje osnovnih operacija kao što su prilikom stvaranja mape, prijenos i preuzimanje podatkovne datoteke, izbrišite račun, itd. Dodatne informacije o spremištu Lake podataka potražite u članku [Pregled od Lake spremišta podataka](data-lake-store-overview.md). Trenutno podržava SDK-a

  *  **Verzija Node.js: 0.10.0 ili noviji**
  *  **Verzija REST API-JA za račun: 2015-10-01 – pregled**
  *  **Verzija REST API-JA za datotečnom sustavu: 2015-10-01 – pregled**

## <a name="prerequisites"></a>Preduvjeti

Prije nego počnete u ovom se članku, morate imati sljedeće:

- **Mogući Azure pretplate**. Pogledajte [Početak Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/).

- **Stvaranje aplikacije komponente Azure Active Directory**. Pomoću aplikacije za Azure AD spremišta podataka Lake aplikacijom Azure AD za provjeru autentičnosti. Postoje različite postupke za provjeru s Azure AD koje su **provjere autentičnosti za krajnjeg korisnika** ili **provjere autentičnosti servis za servis**. Upute i dodatne informacije o provjeri autentičnosti potražite u članku [provjere autentičnosti s trgovinom Lake podatke pomoću servisa Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

## <a name="how-to-install"></a>Kako instalirati

```bash
npm install azure-arm-datalake-store
```

## <a name="authenticate-using-azure-active-directory"></a>Provjera valjanosti putem Azure Active Directory

Isječci u nastavku pokazuju dva zasebna načina provjere autentičnosti s trgovinom Lake podataka pomoću Azure AD. Detaljne rasprave na različite metode koje ćete koristiti za provjeru autentičnosti s trgovinom Lake podataka potražite u članku [provjere autentičnosti s trgovinom Lake podatke pomoću servisa Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

Isječak ispod zahtijeva i unose kao što je naziv domene za Azure AD, ID klijenta za Azure AD aplikacije, itd. Te detalje možete biti dohvaćeni iz Azure AD aplikacije koja morate stvoriti, pojedinosti koje su obuhvatiti i gornje veze.

 ```javascript
 var msrestAzure = require('ms-rest-azure');
 //user authentication
 var credentials = new msRestAzure.UserTokenCredentials('your-client-id', 'your-domain', 'your-username', 'your-password', 'your-redirect-uri');
 //service principal authentication
 var credentials = new msRestAzure.ApplicationTokenCredentials('your-client-id', 'your-domain', 'your-secret');
 ```

## <a name="create-the-data-lake-store-clients"></a>Stvaranje spremišta podataka Lake klijenti

```javascript
var adlsManagement = require("azure-arm-datalake-store");
var acccountClient = new adlsManagement.DataLakeStoreAccountClient(credentials, "your-subscription-id");
var filesystemClient = new adlsManagement.DataLakeStoreFileSystemClient(credentials);
```

## <a name="create-a-data-lake-store-account"></a>Stvaranje računa spremišta Lake podataka

```javascript
var util = require('util');
var resourceGroupName = 'testrg';
var accountName = 'testadlsacct';
var location = 'eastus2';

// account object to create
var accountToCreate = {
  tags: {
    testtag1: 'testvalue1',
    testtag2: 'testvalue2'
  },
  name: accountName,
  location: location
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

## <a name="create-a-file-with-content"></a>Stvaranje datoteke sa sadržajem
```javascript
var util = require('util');
var accountName = 'testadlsacct';
var fileToCreate = '/myfolder/myfile.txt';
var options = {
  streamContents: new Buffer('some string content')
}

filesystemClient.fileSystem.listFileStatus(accountName, fileToCreate, options, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    // no result is returned, only a 201 response for success.
    console.log('response is: ' + util.inspect(response, {depth: null}));
  }
});
```

## <a name="get-a-list-of-files-and-folders"></a>Dobit ćete popis datoteka i mapa

```javascript
var util = require('util');
var accountName = 'testadlsacct';
var pathToEnumerate = '/myfolder';
filesystemClient.fileSystem.listFileStatus(accountName, pathToEnumerate, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="see-also"></a>Vidi također

- [Microsoft Azure SDK Node.js](https://github.com/azure/azure-sdk-for-node)
- [Microsoft Azure SDK Node.js - upravljanje Lake analize podataka](https://www.npmjs.com/package/azure-arm-datalake-analytics)
