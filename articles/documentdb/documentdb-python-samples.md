<properties 
    pageTitle="Primjeri NoSQL Python za DocumentDB | Microsoft Azure" 
    description="Pronađite NoSQL Python Primjeri na github za uobičajene zadatke u DocumentDB, uključujući CRUD operacije JSON dokumenata u bazama podataka za NoSQL." 
    keywords="Primjeri Python"
    services="documentdb" 
    authors="moderakh" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter="python"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="04/18/2016" 
    ms.author="moderakh"/>


# <a name="documentdb-python-examples"></a>Primjeri DocumentDB Python

> [AZURE.SELECTOR]
- [Primjeri .NET](documentdb-dotnet-samples.md)
- [Primjeri Node.js](documentdb-nodejs-samples.md)
- [Primjeri Python](documentdb-python-samples.md)
- [Galerija jednostavnih kod Azure](https://azure.microsoft.com/documentation/samples/?service=documentdb)

Uzorak rješenja koja izvođenje operacije CRUD i ostalih uobičajenih operacija Azure DocumentDB resursa nalaze se u spremištu GitHub [azure documentdb python](https://github.com/Azure/azure-documentdb-python/tree/master/samples) . Ovaj članak sadrži:

- Veze sa zadacima u svakoj od Python primjer projektnih datoteka. 
- Veze s povezanim API reference sadržaj.

**Preduvjeti**

1. Potreban vam je račun za Azure da biste koristili u ovim se primjerima Python:
    - Možete je [besplatno otvorite račun za Azure](https://azure.microsoft.com/pricing/free-trial/): dohvaćanje kredita možete koristiti da biste isprobali plaćenu servisa Azure pa čak i kada se koriste najviše možete voditi računa i korištenje slobodno Azure servisa, kao što su web-mjesta. Vaša kreditna kartica nikad naplatiti, osim ako izričito Promjena postavki i od nje zatražite da se naplatiti.
   - Možete [aktivirati Visual Studio pretplatnika pogodnosti](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/): vaše Visual Studio pretplata vam kredita svakog mjeseca, koje možete koristiti za plaćenu Azure servise.
2. Morate [Python SDK](documentdb-sdk-python.md). 

    > [AZURE.NOTE] Svaki uzorak samostalnih, postavlja sam i čisti nakon sam. Kao takve, uzorcima izdati više pozive na [document_client. CreateCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html). Svaki put kada to možete učiniti pretplate će se naplatiti 1 h korištenje po sloju performanse zbirke stvoren. 

## <a name="database-examples"></a>Primjeri baze podataka

Datoteka [Program.py](https://github.com/Azure/azure-documentdb-python/tree/master/samples/DatabaseManagement/Program.py) projekta [DatabaseManagement](https://github.com/Azure/azure-documentdb-python/tree/master/samples/DatabaseManagement) pokazuje kako izvedite sljedeće zadatke.

Zadatak | Referenca API-JA
--- | ---
[Stvaranje baze podataka](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L65-L76) | [document_client. CreateDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)
[Račun za bazu podataka za upite](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L49-L62) | [document_client. QueryDatabases](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)
[Čitanje baze podataka po ID-a](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L79-L96) | [document_client. ReadDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)
[Popis baze podataka za račun](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L99-L110) | [document_client. ReadDatabases](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)
[Brisanje baze podataka](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L113-L126) | [document_client. DeleteDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)

## <a name="collection-examples"></a>Primjeri zbirke 

Datoteka [Program.py](https://github.com/Azure/azure-documentdb-python/tree/master/samples/CollectionManagement/Program.py) projekta [CollectionManagement](https://github.com/Azure/azure-documentdb-python/tree/master/samples/CollectionManagement) pokazuje kako izvršiti sljedeće zadatke.

Zadatak | Referenca API-JA
--- | ---
[Stvaranje zbirke](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L84-L135) | [document_client. CreateCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Pročitajte popis svih zbirki u bazi podataka.](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L198-L225) | [document_client. ListCollections](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Dohvaćanje zbirke po ID-a](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L178-L195) | [document_client. ReadCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Početak performanse sloju zbirke](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L139-L161) | [DocumentQueryable.QueryOffers](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Promjena performanse sloju zbirke](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L163-L175) | [document_client. ReplaceOffer](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Brisanje zbirke](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L212-L225) | [document_client. DeleteCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
