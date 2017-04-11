<properties
    pageTitle="Primjeri NoSQL Node.js za DocumentDB | Microsoft Azure"
    description="Pronađite NoSQL Node.js Primjeri na github za uobičajene zadatke u DocumentDB, uključujući CRUD operacije JSON dokumenata u bazama podataka za NoSQL."
    keywords="Primjeri Node.js"
    services="documentdb"
    authors="moderakh"
    manager="jhubbard"
    editor="monicar"
    documentationCenter="nodejs"/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="moderakh"/>


# <a name="documentdb-nodejs-examples"></a>Primjeri DocumentDB Node.js

> [AZURE.SELECTOR]
- [Primjeri .NET](documentdb-dotnet-samples.md)
- [Primjeri Node.js](documentdb-nodejs-samples.md)
- [Primjeri Python](documentdb-python-samples.md)
- [Galerija jednostavnih kod Azure](https://azure.microsoft.com/documentation/samples/?service=documentdb)

Uzorak rješenja koja izvođenje operacije CRUD i ostalih uobičajenih operacija Azure DocumentDB resursa nalaze se u spremištu GitHub [azure documentdb nodejs](https://github.com/Azure/azure-documentdb-node/tree/master/samples) . Ovaj članak sadrži:

- Veze sa zadacima u svakoj od Node.js primjer projektnih datoteka.
- Veze s povezanim API reference sadržaj.

**Preduvjeti**

1. Potreban vam je račun za Azure da biste koristili u ovim se primjerima Node.js:
    - Možete je [besplatno otvorite račun za Azure](https://azure.microsoft.com/pricing/free-trial/): dohvaćanje kredita možete koristiti da biste isprobali plaćenu servisa Azure i čak i kada se koriste najviše možete zadržati račun i korištenje slobodno Azure servisa, kao što su web-mjesta. Vaša kreditna kartica nikad naplatiti, osim ako izričito Promjena postavki i od nje zatražite da se naplatiti.
   - Možete [aktivirati Visual Studio pretplatnika pogodnosti](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/): vaše Visual Studio pretplata vam kredita svakog mjeseca, koje možete koristiti za plaćenu Azure servise.
2. Morate [Node.js SDK](documentdb-sdk-node.md).

    > [AZURE.NOTE] Svaki uzorak samostalnih, postavlja sam i čisti nakon sam. Kao takve, uzorcima izdati više poziva [DocumentClient.createCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createCollection). Svaki put kada to možete učiniti pretplate će se naplatiti 1 h korištenje po sloju performanse zbirke stvoren.

## <a name="database-examples"></a>Primjeri baze podataka

Datoteka [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/DatabaseManagement/app.js) projekta [DatabaseManagement](https://github.com/Azure/azure-documentdb-node/tree/master/samples/DatabaseManagement) pokazuje kako izvršiti sljedeće zadatke.

Zadatak | Referenca API-JA
--- | ---
[Stvaranje baze podataka](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L121-L131) | [DocumentClient.createDatabase](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createDatabase)
[Račun za bazu podataka za upite](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L146-L171) | [DocumentClient.queryDatabases](http://azure.github.io/azure-documentdb-node/DocumentClient.html#queryDatabases)
[Čitanje baze podataka po ID-a](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L89-L99) | [DocumentClient.readDatabase](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readDatabase)
[Popis baze podataka za račun](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L111-L119) | [DocumentClient.readDatabases](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readDatabase)
[Brisanje baze podataka](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L133-L144) | [DocumentClient.deleteDatabase](http://azure.github.io/azure-documentdb-node/DocumentClient.html#deleteDatabase)

## <a name="collection-examples"></a>Primjeri zbirke

Datoteka [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/CollectionManagement/app.js) projekta [CollectionManagement](https://github.com/Azure/azure-documentdb-node/tree/master/samples/CollectionManagement) pokazuje kako izvršiti sljedeće zadatke.

Zadatak | Referenca API-JA
--- | ---
[Stvaranje zbirke](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L97-L118) | [DocumentClient.createCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createCollection)
[Pročitajte popis svih zbirki u bazi podataka.](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L120-L130) | [DocumentClient.readCollections](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readCollections)
[Zbirka došli _isto](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L132-L141) | [DocumentClient.readCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readCollection)
[Dohvaćanje zbirke po ID-a](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L143-L156) | [DocumentClient.readCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readCollection)
[Početak performanse sloju zbirke](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L158-L186) | [DocumentQueryable.queryOffers](http://azure.github.io/azure-documentdb-node/DocumentClient.html#queryOffers)
[Promjena performanse sloju zbirke](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L188-L202) | [DocumentClient.replaceOffer](http://azure.github.io/azure-documentdb-node/DocumentClient.html#replaceOffer)
[Brisanje zbirke](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L204-L215) | [DocumentClient.deleteCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#deleteCollection)

## <a name="document-examples"></a>Primjeri dokumenta

Datoteka [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/DocumentManagement/app.js) projekta [DocumentManagement](https://github.com/Azure/azure-documentdb-node/tree/master/samples/DocumentManagement) pokazuje kako izvršiti sljedeće zadatke.

Zadatak | Referenca API-JA
--- | ---
[Stvaranje dokumenata](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L153-L177) | [DocumentClient.createDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createDocument)
[Čitanje dokumenta sažetka sadržaja za zbirku](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L179-L189) | [DocumentClient.readDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readDocument)
[Čitanje dokumenata po ID-a](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L191-L201) | [DocumentClient.readDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readDocument)
[Čitanje dokumenta samo ako je promijenjena dokumenta](https://github.com/Azure/azure-documentdb-node/blob/0778eadea7abb2af41e8c22a239dc872c584f421/samples/DocumentManagement/app.js#L79-L107) | [DocumentClient.readDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readDocument)<br/>[RequestOptions.accessCondition](http://azure.github.io/azure-documentdb-node/global.html#RequestOptions)
[Upit za dokumente](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L82-L110) | [DocumentClient.queryDocuments](http://azure.github.io/azure-documentdb-node/DocumentClient.html#queryDocuments)
[Zamjena dokumenta](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L112-L119) |  [DocumentClient.replaceDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#replaceDocument)
[Zamjena dokumenta s uvjetnim e-oznake, potvrdite okvir](https://github.com/Azure/azure-documentdb-node/blob/0778eadea7abb2af41e8c22a239dc872c584f421/samples/DocumentManagement/app.js#L147-L164) |  [DocumentClient.replaceDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#replaceDocument)<br/>[RequestOptions.accessCondition](http://azure.github.io/azure-documentdb-node/global.html#RequestOptions)
[Brisanje dokumenta](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L122-L133) | [DocumentClient.deleteDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#deleteDocument)

## <a name="indexing-examples"></a>Primjeri indeksiranja

Datoteka [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/IndexManagement/app.js) projekta [IndexManagement](https://github.com/Azure/azure-documentdb-node/tree/master/samples/IndexManagement) pokazuje kako izvršiti sljedeće zadatke.

Zadatak | Referenca API-JA
--- | ---
Stvaranje zbirke sa zadanom indeksiranja | [DocumentClient.createDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html)
[Ručno indeksiranje određeni dokument](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L185-L238) | [indexingDirective: "Uključi"](http://azure.github.io/azure-documentdb-node/global.html#indexingDirective)
[Ručno izostaviti određeni dokument iz indeksa](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L120-L183) | [RequestOptions.indexingDirective](http://azure.github.io/azure-documentdb-node/global.html#RequestOptions)
[Korištenje drži indeksiranje Masovni uvoz ili čitanje velike zbirke](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L240-L269) | [IndexingMode.Lazy](http://azure.github.io/azure-documentdb-node/global.html#IndexingMode)
[Umetanje određenih putova dokumenta u indeksiranja](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L433-L444) | [IndexingPolicy.IncludedPaths](http://azure.github.io/azure-documentdb-node/global.html#IndexingPolicy)
[Isključivanje određenih putova iz indeksiranja](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L427-L450) | [ExcludedPath](http://azure.github.io/azure-documentdb-node/global.html#IndexingPolicy)
[Dopusti pregleda na put niz tijekom operacije na raspon](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L271-L347)| [ExcludedPath.EnableScanInQuery](http://azure.github.io/azure-documentdb-node/global.html#FeedOptions)
[Stvaranje indeksa raspon na put niza](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L349-L425) | [DocumentClient.queryDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#queryDocument)
[Stvaranje zbirke s indexPolicy zadani, a zatim ažurirati putem Interneta](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L519-L614) | [DocumentClient.createCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createCollection)<br> [DocumentClient.replaceCollection#replaceCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html)

Dodatne informacije o indeksiranja potražite u članku [DocumentDB indeksiranja pravila](documentdb-indexing-policies.md).

## <a name="server-side-programming-examples"></a>Primjere programiranja poslužiteljsko

Datoteka [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/ServerSideScripts/app.js) projekta [ServerSideScripts](https://github.com/Azure/azure-documentdb-node/tree/master/samples/ServerSideScripts) pokazuje kako izvršiti sljedeće zadatke.

Zadatak | Referenca API-JA
--- | ---
[Stvaranje pohranjena procedura](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.ServerSideScripts/app.js#L44-L71) | [DocumentClient.createStoredProcedure](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createStoredProcedure)
[Izvršavanje pohranjena procedura](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.ServerSideScripts/app.js#L73-L90) | [DocumentClient.executeStoredProcedure](http://azure.github.io/azure-documentdb-node/DocumentClient.html#executeStoredProcedure)

Dodatne informacije o poslužiteljsko programiranje potražite u članku [DocumentDB poslužiteljsko programiranje: pohranjene procedure, okidača baze podataka i korisnički definiranih funkcija](documentdb-programming.md).

## <a name="partitioning-examples"></a>Stvaranje particija Primjeri

Datoteka [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/Partitioning/app.js) projekta [Partitioning](https://github.com/Azure/azure-documentdb-node/tree/master/samples/Partitioning) pokazuje kako izvršiti sljedeće zadatke.

Zadatak | Referenca API-JA
--- | ---
[Korištenje programa HashPartitionResolver](https://github.com/Azure/azure-documentdb-node/blob/ce0fc3c4e70b0279091a1e03620a668d93a14fc2/samples/Partitioning/app.js#L53-L103) | [HashPartitionResolver](http://azure.github.io/azure-documentdb-node/HashPartitionResolver.html)

Dodatne informacije o stvaranju particija podataka u DocumentDB potražite u članku [particija i skaliranje podataka u DocumentDB](documentdb-partition-data.md).
