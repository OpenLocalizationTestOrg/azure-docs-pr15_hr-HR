<properties
pageTitle="Indeksiranje spremište tablica platforme Azure pomoću Azure pretraživanja"
description="Saznajte kako indeksirati podaci spremljeni u tablicama Azure pomoću pretraživanja Azure"
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
ms.date="08/16/2016"
ms.author="eugenesh" />

# <a name="indexing-azure-table-storage-with-azure-search"></a>Indeksiranje spremište tablica platforme Azure pomoću Azure pretraživanja

U ovom se članku objašnjava korištenje pretraživanja za Azure indeksirati podatke pohranjene u spremište tablica Azure. Novi indeksiranje pretraživanja Azure tablice olakšava postupak brzi i objedinjenog. 

> [AZURE.IMPORTANT] Ta je funkcija trenutno u pretpregledu. On je dostupan samo u REST API-JA pomoću verzija **2015-02-28 – pregled** i verzije 2.0 pretpregled SDK .NET. Imajte na umu, pretpregled API-ji su namijenjene testiranje i procjenu, a ne treba koristiti u radnom okruženju.

## <a name="setting-up-azure-table-indexing"></a>Postavljanje indeksiranje tablica platforme Azure

Da biste postavili i konfiguriranje indeksiranje Azure tablice, možete koristiti Azure pretraživanje REST API-JA za stvaranje i upravljanje **indexers** i **izvore podataka** kao što je opisano u [Postupci za indeksiranje](https://msdn.microsoft.com/library/azure/dn946891.aspx). Možete koristiti i [verzije 2.0 – pregled](https://msdn.microsoft.com/library/mt761536%28v=azure.103%29.aspx) .NET SDK. U budućnosti podrška za indeksiranje tablice dodat će se na Azure Portal.

Izvor podataka određuje podatke koje želite indeksirati vjerodajnice potrebne za pristup podacima i pravila koja omogućuju pretraživanje Azure učinkovito prepoznavanje promjene u podacima (Novo, promijenili ili izbrisali retke).

Indeksiranje čita podatke iz izvora podataka i učitava u indeks za traženje cilja.

Da biste postavili indeksiranje tablice:

1. Stvaranje izvora podataka
    - Postavljanje na `type` parametar`azuretable`
    - Prosljeđivanje u nizu za povezivanje za pohranu računa kao u `credentials.connectionString` parametra
    - Navedite pomoću naziv tablice u `container.name` parametra
    - Možete i navesti pomoću upita s `container.query` parametar. Kad god je to moguće, pomoću filtra na PartitionKey radi ostvarivanja najboljih performansi bilo koji upit će rezultirati skeniranja cijelog tablice, koji može uzrokovati slabe performanse velikih tablica.
2. Stvorite indeks pretraživanja sa shemom koja odgovara stupce u tablicu koju želite indeksirati. 
3. Stvaranje indeksiranje povezivanje s izvorom podataka u indeks pretraživanja.

### <a name="create-data-source"></a>Stvaranje izvora podataka

    POST https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "table-datasource",
        "type" : "azuretable",
        "credentials" : { "connectionString" : "<my storage connection string>" },
        "container" : { "name" : "my-table", "query" : "PartitionKey eq '123'" }
    }   

Dodatne informacije o API za stvaranje izvora podataka, potražite u članku [Stvaranje izvora podataka](search-api-indexers-2015-02-28-preview.md#create-data-source).

### <a name="create-index"></a>Stvaranje indeksa 

    POST https://[service name].search.windows.net/indexes?api-version=2015-02-28
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-target-index",
        "fields": [
            { "name": "key", "type": "Edm.String", "key": true, "searchable": false },
            { "name": "SomeColumnInMyTable", "type": "Edm.String", "searchable": true }
        ]
    }

Dodatne informacije o API za stvaranje indeksa, potražite u članku [Stvaranje indeksa](https://msdn.microsoft.com/library/dn798941.aspx)

### <a name="create-indexer"></a>Stvaranje indeksiranje 

Na kraju, stvorite komponente za indeksiranje koja se odnosi na izvor podataka i indeksa cilj. Ako, na primjer:

    POST https://[service name].search.windows.net/indexers?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "table-indexer",
      "dataSourceName" : "table-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" }
    }

Dodatne informacije o indeksiranje API stvaranje potražite u članku [Stvaranje indeksiranje](search-api-indexers-2015-02-28-preview.md#create-indexer).

To je sve to – indeksiranje Sretno!

## <a name="dealing-with-different-field-names"></a>Postupanje s nazivima drugo polje

Često naziva polja u postojeći indeks će se razlikovati od naziva svojstva tablice. **Mapiranje polja** možete koristiti za mapiranje svojstava imena iz tablice za nazive polja u indeksu pretraživanja. Da biste saznali više o mapiranja polja, potražite u članku [mapiranja polja za indeksiranje pretraživanja Azure premostiti razlike između izvora podataka i indeksa pretraživanja](search-indexer-field-mappings.md).

## <a name="handling-document-keys"></a>Rukovanje tipke dokumenta

U odjeljku pretraživanje Azure tipku dokument služi kao jedinstvena identifikacija dokumenta. Svaki indeks pretraživanja mora imati jedan polja ključa vrste `Edm.String`. Za svaki dokument koji se dodaje u indeks potreban je polje ključa (to je samo polja obavezno).

Budući da se reci tablice koji imaju složene ključ, pretraživanje Azure stvara stilova sintetičkih poljem pod nazivom `Key` je Ulančavanje particija ključ i redak vrijednosti ključa. Na primjer, ako redak je PartitionKey je `PK1` i RowKey `RK1`, zatim `Key` bit će vrijednost polja `PK1RK1`. 

> [AZURE.NOTE] Na `Key` vrijednost možda sadrži znakove koji nisu valjani u dokument tipke, kao što su crtica. Možete pomoću baviti znakove koji nisu valjani u `base64Encode` [funkcija mapiranje polja](search-indexer-field-mappings.md#base64EncodeFunction). Ako to učinite, ne zaboravite koristiti URL sigurnih Base64 kodiranja pri prosljeđivanje dokumenta tipke u API poziva kao što su pretraživanja.

## <a name="incremental-indexing-and-deletion-detection"></a>Rastuća indeksiranja i brisanje otkrivanje
 
Kada postavite komponente za indeksiranje tablice da biste pokrenuli prema rasporedu, reindexes novi ili ažurirani retke, utvrdili u retku `Timestamp` vrijednost. Morate navesti pravila za otkrivanje promjena – rastuće omogućeno indeksiranje umjesto vas automatski. 

Da biste naznačili određene dokumente mora se ukloniti iz indeksa, možete koristiti meki Izbriši strategije – brisanje retka, dodavanje svojstva da biste naznačili da je izbrisati, a postavljanje pravila za otkrivanje meki brisanja na izvor podataka. Na primjer, pravila prikazano u nastavku će razmotrite retka izbriše ako je svojstvo `IsDeleted` vrijednošću `"true"`: 

    PUT https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]
    
    {
        "name" : "my-table-datasource",
        "type" : "azuretable",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "table name", "query" : "query" },
        "dataDeletionDetectionPolicy" : { "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy", "softDeleteColumnName" : "IsDeleted", "softDeleteMarkerValue" : "true" }
    }   


## <a name="help-us-make-azure-search-better"></a>Pridonesite poboljšati Azure pretraživanja

Ako imate zahtjeve za značajke ili ideje poboljšanja, provjerite stupili nam na našem [UserVoice web-mjesta](https://feedback.azure.com/forums/263029-azure-search/).