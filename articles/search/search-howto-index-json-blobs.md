<properties
pageTitle="Indeksiranje JSON blob-ova pomoću komponente za indeksiranje blobova platforme Azure pretraživanja"
description="Indeksiranje JSON blob-ova pomoću komponente za indeksiranje blobova platforme Azure pretraživanja"
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
ms.date="07/26/2016"
ms.author="eugenesh" />

# <a name="indexing-json-blobs-with-azure-search-blob-indexer"></a>Indeksiranje JSON blob-ova pomoću komponente za indeksiranje blobova platforme Azure pretraživanja 

U ovom se članku objašnjava konfiguriranje indeksiranje blobova platforme Azure pretraživanja da biste izdvojili strukturirani sadržaj s blob-ova koji sadrže JSON.

## <a name="scenarios"></a>Scenariji

Prema zadanim postavkama, [Indeksiranje pretraživanja Azure blob](search-howto-indexing-azure-blob-storage.md) raščlanjuje JSON blob-ova kao jedan blok teksta. Često želite zadržati strukturu JSON dokumenata. Na primjer, dobili JSON dokumenta 

    { 
        "article" : {
            "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13" 
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

Možda ćete morati raščlaniti u dokument Azure pretraživanja s "tekst", "datePublished" i "oznake" polja.

Osim toga, kada vaš blob-ova sadrže **polja JSON objekata**, preporučujemo vam svaki element polja postati sa zasebnim dokumentom Azure pretraživanja. Na primjer, dobili blob s ovom JSON:  

    [
        { "id" : "1", "text" : "example 1" },
        { "id" : "2", "text" : "example 2" },
        { "id" : "3", "text" : "example 3" }
    ]

možete popuniti indeksu pretraživanja Azure s dokumentima u zasebnom 3, svaka s poljima "id" i "tekst". 

> [AZURE.IMPORTANT] Ta je funkcija trenutno u pretpregledu. On je dostupan samo u REST API-JA pomoću verzija **2015-02-28-pretpregled**. Imajte na umu, pretpregled API-ji su namijenjene testiranje i procjenu, a ne treba koristiti u radnom okruženju. 

## <a name="setting-up-json-indexing"></a>Postavljanje JSON indeksiranja

Da biste indeksirati JSON blob-ova, postavite na `parsingMode` konfiguracije parametar `json` (za indeksiranje svaki blob kao jedan dokument) ili `jsonArray` (Ako je vaš blob-ova sadrže JSON polja): 

    {
      "name" : "my-json-indexer",
      ... other indexer properties
      "parameters" : { "configuration" : { "parsingMode" : "json" | "jsonArray" } }
    }

Ako je potrebno, odaberite Svojstva izvornog dokumenta JSON koristi za popunjavanje indeksa za traženje cilja pomoću **mapiranja polja** .  To je opisan u nastavku. 

> [AZURE.IMPORTANT] Prilikom korištenja `json` ili `jsonArray` Raščlanjivanje način Azure pretraživanja pretpostavlja da sve blob polja u izvoru podataka će se JSON. Ako vam je potrebna podrška kombinacije JSON i koje nisu JSON blob-ova u isti izvor podataka, obratite nam se keyboardshortcuts@Microsoft.commailto na [našem UserVoice web-mjesta](https://feedback.azure.com/forums/263029-azure-search).

## <a name="using-field-mappings-to-build-search-documents"></a>Da biste sastavili pretraživanje dokumenata pomoću mapiranje polja 

Trenutno pretraživanje Azure ne možete indeksirati proizvoljne JSON dokumente izravno jer podržava samo jednostavne vrste podataka, niz polja, i GeoJSON točaka. Međutim, možete koristiti **mapiranja polja** da biste izdvojili dijelova dokumenta JSON i "Podignite" ih u najviše razine polja pretraživanje dokumenta. Da biste saznali više o osnovama mapiranja polja, potražite u članku [mapiranja polja za indeksiranje pretraživanja Azure premostiti razlike između izvora podataka i indeksa pretraživanja](search-indexer-field-mappings.md).

Uskoro vratiti dokumentu JSON primjer: 

    { 
        "article" : {
            "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13" 
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

Recimo da imate indeks pretraživanja sa sljedećim poljima: `text` vrste Edm.String, `date` vrste Edm.DateTimeOffset, a `tags` vrste Collection(Edm.String). Da biste mapirali vaše JSON u željeni oblik, koristite sljedeće mapiranja polja: 

    "fieldMappings" : [ 
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
    ]

Određeni su klauzulom izvor naziva polja u odjeljku mapiranja pomoću [Pokazivača JSON](http://tools.ietf.org/html/rfc6901) notacije. Započnite kose crte da biste se pozvali korijenu dokumentu JSON, zatim dubinski željeni svojstvo (pri proizvoljne razina gniježđenja) pomoću prosljeđivanje kosa crta odvojene put. 

Možete uputiti elementima pojedinačnih polja pomoću polje indeksa. Ako, na primjer, da biste odabrali prvi element polja "oznake" u gornjem primjeru, koristite mapiranje polja ovako:

    { "sourceFieldName" : "/article/tags/0", "targetFieldName" : "firstTag" }

> [AZURE.NOTE] Ako izvorni naziv polja na putu mapiranje polja upućuje na svojstvo koje se ne postoji u JSON, to mapiranje preskače bez pogrešaka. To možete učiniti tako da se podržavamo dokumenata s različitim shema (koji se zajednički koristi slova). Jer nema provjere valjanosti, morate izvršiti da biste izbjegli pogreške u specifikacije mapiranje polja. 

Ako dokumente JSON sadržavati samo jednostavne svojstva najviše razine, ne morate mapiranja polja uopće. Ako, na primjer, ako vaš JSON izgleda ovako, najviše razine svojstva "tekst", "datePublished" i "oznake" izravno preslikava odgovarajuća polja u indeks pretraživanja: 
 
    { 
       "text" : "A hopefully useful article explaining how to parse JSON blobs",
       "datePublished" : "2016-04-13" 
       "tags" : [ "search", "storage", "howto" ]    
    }

## <a name="indexing-nested-json-arrays"></a>Indeksiranje ugniježđene JSON polja

Što se događa ako na koju želite indeksirati polja JSON objekata, ali tog polja je ugniježđeno negdje u dokumentu? Možete odabrati koji svojstvo sadrži pomoću polja u `documentRoot` konfiguracija svojstava. Na primjer, ako vaš blob-ova izgledati ovako: 

    { 
        "level1" : {
            "level2" : [
                { "id" : "1", "text" : "Use the documentRoot property" }, 
                { "id" : "2", "text" : "to pluck the array you want to index" },
                { "id" : "3", "text" : "even if it's nested inside the document" }  
            ]
        }
    } 

koristite tu konfiguraciju indeksirati polja sadržanog u svojstvu "level2": 

    {
        "name" : "my-json-array-indexer",
        ... other indexer properties
        "parameters" : { "configuration" : { "parsingMode" : "jsonArray", "documentRoot" : "/level1/level2" } }
    }


## <a name="request-examples"></a>Primjeri zahtjev

Stavljanje to sve zajedno, Evo primjera dovršeno payloads. 

Izvor podataka: 

    POST https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<my storage connection string>" },
        "container" : { "name" : "my-container", "query" : "optional, my-folder" }
    }   

Indeksiranje:

    POST https://[service name].search.windows.net/indexers?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "useJsonParser" : true } }, 
      "fieldMappings" : [ 
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
      ]
    }

## <a name="help-us-make-azure-search-better"></a>Pridonesite poboljšati Azure pretraživanja

Ako imate zahtjeve za značajke ili ideje poboljšanja, provjerite stupili nam na našem [UserVoice web-mjesta](https://feedback.azure.com/forums/263029-azure-search/).