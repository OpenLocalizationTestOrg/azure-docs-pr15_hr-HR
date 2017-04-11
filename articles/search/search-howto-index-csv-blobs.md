<properties
pageTitle="Indeksiranje CSV blob-ova pomoću komponente za indeksiranje blobova platforme Azure pretraživanje | Microsoft Azure"
description="Saznajte kako indeksirati CSV blob-ova pomoću pretraživanja Azure"
services="search"
documentationCenter=""
authors="chaosrealm"
manager="pablocas"
editor="" />

<tags
ms.service="search"
ms.devlang="rest-api"
ms.workload="search" ms.topic="article"  
ms.tgt_pltfrm="na"
ms.date="07/12/2016"
ms.author="eugenesh" />

# <a name="indexing-csv-blobs-with-azure-search-blob-indexer"></a>Indeksiranje CSV blob-ova pomoću komponente za indeksiranje blobova platforme Azure pretraživanja 

Prema zadanim postavkama, [Indeksiranje pretraživanja Azure blob](search-howto-indexing-azure-blob-storage.md) raščlanjuje zarezom blob-ova tekst kao jedan blok teksta. Međutim, s blob-ova koji sadrži podatke CSV, često želite za svaki redak u obrađuje blobova platforme kao sa zasebnim dokumentom. Na primjer, uz sljedeće razdvojeni tekstni: 

    id, datePublished, tags
    1, 2016-01-12, "azure-search,azure,cloud" 
    2, 2016-07-07, "cloud,mobile" 

Možda ćete morati raščlaniti u 2 dokumente svaki koji sadrže "id", "datePublished" i "oznake" polja.

U ovom članku ćete naučiti raščlaniti CSV blob-ova s blob indeksiranje pretraživanja Azure. 

> [AZURE.IMPORTANT] Ta je funkcija trenutno u pretpregledu. On je dostupan samo u REST API-JA pomoću verzija **2015-02-28-pretpregled**. Imajte na umu, pretpregled API-ji su namijenjene testiranje i procjenu, a ne treba koristiti u radnom okruženju. 

## <a name="setting-up-csv-indexing"></a>Postavljanje CSV indeksiranja

Indeksiranje CSV blob-ova, stvaranje ili ažuriranje definicije indeksiranje s na `delimitedText` Raščlanjivanje način:  

    {
      "name" : "my-csv-indexer",
      ... other indexer properties
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "firstLineContainsHeaders" : true } }
    }

Dodatne informacije o indeksiranje API stvaranje potražite u članku [Stvaranje indeksiranje](search-api-indexers-2015-02-28-preview.md#create-indexer).

`firstLineContainsHeaders`upućuje na to da prvi redak (koji nisu prazni) za svaki blob sadrži zaglavlja.
Blob polja ne sadrže li na redak zaglavlja Početna, u konfiguraciji indeksiranje treba navesti zaglavlja: 

    "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } } 

Trenutno, samo UTF-8 kodiranje nije podržano. Osim toga, samo zarez `','` znak podržana kao graničnik. Ako vam je potrebna podrška za ostale kodiranja ili graničnike, obratite nam se keyboardshortcuts@Microsoft.commailto na [našem UserVoice web-mjesta](https://feedback.azure.com/forums/263029-azure-search).

> [AZURE.IMPORTANT] Kada koristite razdvojeni tekstni Raščlanjivanje način, Azure pretraživanje pretpostavlja da sve blob polja u izvoru podataka će se CSV. Ako vam je potrebna podrška kombinacije CSV i koje nisu CSV blob-ova u isti izvor podataka, obratite nam se keyboardshortcuts@Microsoft.commailto na [našem UserVoice web-mjesta](https://feedback.azure.com/forums/263029-azure-search).

## <a name="request-examples"></a>Primjeri zahtjev

Stavljanje to sve zajedno, Evo primjera tereta dovršeno. 

Izvor podataka: 

    POST https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<my storage connection string>" },
        "container" : { "name" : "my-container", "query" : "<optional, my-folder>" }
    }   

Indeksiranje:

    POST https://[service name].search.windows.net/indexers?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-csv-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } }
    }

## <a name="help-us-make-azure-search-better"></a>Pridonesite poboljšati Azure pretraživanja

Ako imate zahtjeve za značajke ili ideje poboljšanja, provjerite stupili nam na našem [UserVoice web-mjesta](https://feedback.azure.com/forums/263029-azure-search/).