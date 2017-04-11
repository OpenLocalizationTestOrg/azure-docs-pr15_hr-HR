<properties 
pageTitle="Indeksiranje operacije (Azure pretraživanje servisa REST API-JA: 2015-02-28-pretpregled) | Pretpregled Azure pretraživanja API-JA" 
description="Indeksiranje operacije (Azure pretraživanje servisa REST API-JA: 2015-02-28-pretpregled)" 
services="search" 
documentationCenter="" 
authors="chaosrealm" 
manager="pablocas"
editor="" />

<tags 
ms.service="search" 
ms.devlang="rest-api" 
ms.workload="search" 
ms.topic="article"  
ms.tgt_pltfrm="na" 
ms.date="09/07/2016" 
ms.author="eugenesh" />

#<a name="indexer-operations-azure-search-service-rest-api-2015-02-28-preview"></a>Indeksiranje operacije (Azure pretraživanje servisa REST API-JA: 2015-02-28-pretpregled)#

> [AZURE.NOTE] U ovom se članku opisuju indexers u [2015-02-28-pretpregled REST API-JA](search-api-2015-02-28-preview.md). Ova verzija API-JA zbraja pretpregled verzijama indeksiranje spremište blobova platforme Azure s izdvajanjem dokument i spremište tablica Azure indeksiranje plus ostala poboljšanja. U API podržava i obično dostupnih (GA) indexers, uključujući indexers za baze podataka SQL Azure, SQL Server Azure VMs i Azure DocumentDB.

## <a name="overview"></a>Pregled ##

Možete Azure pretraživanje integrirati izravno s nekoliko uobičajenih izvora podataka u koji uklanjanjem potrebe za pisanje koda indeksirati podatke. Da biste postavili to prema gore, možete nazvati Azure API pretraživanja za stvaranje i upravljanje **indexers** i **izvore podataka**. 

**Indeksiranje** je resurs koji povezuje se s cilj pretraživanja indeksi izvora podataka. Indeksiranje koristi se na sljedeće načine: 

- Izvođenje jednokratnu kopiju podataka za popunjavanje indeksa.
- Sinkronizirajte indeksa promjenama u izvoru podataka na raspored. Raspored je dio definiciju indeksiranje.
- Pozivanje na zahtjev za ažuriranje indeksa prema potrebi. 

**Indeksiranje** je korisna kada želite da se u obični ažuriranja indeksa. Možete postaviti na istoj razini raspored kao dio definicije indeksiranje ili ga pokrenuti na zahtjev pomoću [Pokrenuti indeksiranje](#RunIndexer). 

**Izvor podataka** određuje podataka koje je potrebno indeksirati, vjerodajnice za pristup podacima i pravila da biste omogućili pretraživanje Azure učinkovito prepoznavanje promjene u podacima (kao što su promijenili ili izbrisali retke u tablici baze podataka). Je definiran kao neovisno resursa tako da ga može koristiti više indexers.

Trenutno podržava sljedećih izvora podataka:

- **Baze podataka Azure SQL** i **SQL Server na Azure VMs**. Ciljano walk-through, potražite u [ovom članku](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md). 
- **Azure DocumentDB**. Ciljano walk-through, potražite u [ovom članku](../documentdb/documentdb-search-indexer.md). 
- **Spremište blobova platforme azure**, uključujući sljedeće oblike dokumenata: PDF-a, Microsoft Office (DOCX/dokumenta, XSLX/XLS, PPTX/PPT, poruka), HTML, XML, ZIP i običnog teksta datoteke (uključujući JSON). Ciljano walk-through, potražite u [ovom članku](search-howto-indexing-azure-blob-storage.md).
- **Spremište tablica platforme azure**. Ciljano walk-through, potražite u [ovom članku](search-howto-indexing-azure-tables.md).
     
Ne možemo se preporučuje se dodavanje podrška za dodatne izvore podataka u budućnosti. Da biste nam je prioritet te odluke, unesite povratne informacije na [forum za povratne informacije Azure pretraživanja](http://feedback.azure.com/forums/263029-azure-search).

Maksimalna ograničenja vezane uz indeksiranje i podataka iz izvora resursima potražite u članku [Ograničenja servisa](search-limits-quotas-capacity.md) .

## <a name="typical-usage-flow"></a>Tipična Upotreba toka ##

Možete stvoriti i upravljanje indexers i izvora podataka putem jednostavne zahtjeva za HTTP (objavu, a zatim DOHVATI, STAVI, brisanje) u odnosu na navedeni `data source` ili `indexer` resursa.

Postavljanje automatskog indeksiranja je obično u četiri koraka:

1. Odredite izvor podataka koji sadrži podatke koje je potrebno indeksirati. Imajte na umu da Azure pretraživanje možda ne podržava sve vrste podataka koje su prisutne u izvoru podataka. Na popisu potražite u članku [podržane vrste podataka](https://msdn.microsoft.com/library/azure/dn798938.aspx) .

2. Stvaranje indeksa pretraživanja Azure čije sheme kompatibilan s izvora podataka.
  
3. Stvaranje izvora podataka za Azure pretraživanje kao što je opisano u [Stvaranje izvora podataka](#CreateDataSource).
  
4. Stvorite indeksiranje pretraživanja Azure kao što je opisano [Stvaranje indeksiranje](#CreateIndexer).

Trebali biste planirati o stvaranju indeksiranje jedan za svaki cilj indeks i podataka kombinaciju izvora. Imate više indexers pisanja u isti indeks, a možete koristiti i isti izvor podataka za više indexers. Međutim, indeksiranje možete samo zauzeti jedan izvor podataka po, a možete pisati samo jedan indeks. 

Nakon stvaranja indeksiranje, možete dohvatiti ima li status izvođenja operacijom [Dobiti Status komponente za indeksiranje](#GetIndexerStatus) . Indeksiranje možete izvesti i u bilo kojem trenutku (umjesto ili uz pokrenut povremeno na raspored) pomoću operacije [Pokrenuti indeksiranje](#RunIndexer) .

<!-- MSDN has 2 art files plus a API topic link list -->


## <a name="create-data-source"></a>Stvaranje izvora podataka ##

U odjeljku pretraživanje Azure izvora podataka se koristi s indexers, dajući informacije veze za osvježavanje podataka ad-hoc ili zakazani cilj indeksa. Možete stvoriti novi izvor podataka unutar servisa za pretraživanje Azure koji se pomoću zahtjev HTTP POST.
    
    POST https://[service name].search.windows.net/datasources?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

Osim toga, možete koristiti STAVI i navedite naziv izvora podataka na URI. Ako ne postoji izvor podataka, će biti stvorena.

    PUT https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]

**Napomena**: maksimalan broj izvora podataka dopušteno razlikuje za cijene sloju. Besplatna usluga omogućuje najviše 3 izvora podataka. Standardnog servisa omogućuje 50 izvora podataka. Detalje potražite u članku [Ograničenja servisa](search-limits-quotas-capacity.md) .

**Zahtjev**

HTTPS potreban je za sve zahtjeve za uslugu. Zahtjev za **Stvaranje izvora podataka** možete konstruirana metodom objave ili STAVI. Prilikom korištenja objavu, navedite naziv izvora podataka u tijelu zahtjeva uz definiciju izvora podataka. STAVI, naziv je dio URL-a. Ako ne postoji izvor podataka, stvaranja. Ako već postoji, ažuriran je u novu definiciju. 

Naziv izvora podataka mora biti mala slova, započnite slovo ili broj, bez kose crte ili točke i biti manje od 128 znakova. Nakon pokretanja naziv izvora podataka s slovo ili broj, ostatak naziv možete uključiti slovo, broj i crtice, pod uvjetom da crtice nisu uzastopni. Detalje potražite u članku [imenovanje pravila](https://msdn.microsoft.com/library/azure/dn857353.aspx) .

Na `api-version` potreban je. Trenutna verzija nije `2015-02-28`.

**Zahtjev za zaglavlja**

Sljedeći popis sadrži opis zaglavlja obaveznih i neobaveznih zahtjev. 

- `Content-Type`: Potreban. To postavljena na`application/json`
- `api-key`: Potreban. Na `api-key` se koriste za provjeru zahtjev za servis za pretraživanje. To je vrijednost niza, jedinstvene za uslugu. Zahtjev za **Stvaranje izvora podataka** mora sadržavati do `api-key` zaglavlje postavljeno na ključ administrator (umjesto ključa upita). 
 
Već, morat ćete naziv usluge za sastavljanje zahtjev za URL-a. Možete dobiti naziv usluge i `api-key` iz nadzornu ploču servisa [Azure Portal](https://portal.azure.com/). Potražite u članku [Stvaranje servisa za pretraživanje na portalu](search-create-service-portal.md) za navigaciju stranicom pomoći.

<a name="CreateDataSourceRequestSyntax"></a>
**Sintaksa tijelu zahtjeva**

U tijelu zahtjeva za sadrži definicije izvora podataka koji sadrži vrstu izvora podataka vjerodajnice za čitanje podataka, kao i neobavezne podatke promjena otkrivanje i pravila otkrivanja brisanja podataka koji se koriste za učinkovito prepoznavanje i brisanja podataka u izvoru podataka kada se koristi s povremeno zakazani komponente za indeksiranje. 

Vidjet ćete da sintaksa za strukturiranje opseg zahtjev je na sljedeći način. Zahtjev za uzorak navedeni su dodatno na u ovoj temi.

    { 
        "name" : "Required for POST, optional for PUT. The name of the data source",
        "description" : "Optional. Anything you want, or nothing at all",
        "type" : "Required. Must be one of 'azuresql', 'documentdb', 'azureblob', or 'azuretable'",
        "credentials" : { "connectionString" : "Required. Connection string for your data source" },
        "container" : { "name" : "Required. The name of the table, collection, or blob container you wish to index" },
        "dataChangeDetectionPolicy" : { Optional. See below for details }, 
        "dataDeletionDetectionPolicy" : { Optional. See below for details }
    }

Zahtjev sadrži sljedeća svojstva: 

- `name`: Potreban. Naziv izvora podataka. Naziv izvora podataka morate samo sadrže mala slova, brojki ili crtice, ne možete pokretati ni završavati crtice i ograničen je na 128 znakova.
- `description`: Neobavezni opis. 
- `type`: Potreban. Mora biti jedna od podržanih vrsta izvora:
    - `azuresql`– Baza podataka azure SQL ili SQL Server na Azure VMs
    - `documentdb`-Azure DocumentDB
    - `azureblob`-Spremište blobova platforme azure
    - `azuretable`-Spremište tablica azure
- `credentials`:
    - Obavezno `connectionString` svojstvo određuje niz veze za izvor podataka. Oblikovanje niza za povezivanje ovisi o vrstu izvora podataka: 
        - Za Azure SQL, to je obično niz za povezivanje sustava SQL Server. Ako koristite Azure portal za dohvaćanje niza za povezivanje, poslužite se `ADO.NET connection string` mogućnost.
        - Niz za povezivanje za DocumentDB, mora biti u sljedećem obliku: `"AccountEndpoint=https://[your account name].documents.azure.com;AccountKey=[your account key];Database=[your database id]"`. Sve vrijednosti su potrebni. Možete ih pronaći [Azure portal](https://portal.azure.com/).  
        - Blobova platforme Azure i spremište tablica, to je niz za povezivanje računa za pohranu. Oblik je opisano [u nastavku](https://azure.microsoft.com/documentation/articles/storage-configure-connection-string/). Potreban je HTTPS protokola krajnjoj točki.  
- `container`, obavezno: određuje podatke koje želite indeksirati pomoću na `name` i `query` svojstva: 
    - `name`, obavezno:
        - Azure SQL: određuje tablicu ili prikaz. Možete koristiti sheme puni nazive, kao što su `[dbo].[mytable]`.
        - DocumentDB: određuje zbirke. 
        - Spremište blobova platforme Azure: određuje spremnik za pohranu.
        - Spremište tablica Azure: određuje naziv tablice. 
    - `query`, Neobavezno:
        - DocumentDB: omogućuje vam da biste naveli upit koji se poravnava proizvoljne izgleda dokumenta JSON u paušalni sheme pretraživanja Azure možete indeksirati.  
        - Spremište blobova platforme Azure: omogućuje vam da odredite virtualna mapa unutar kontejnera blob. Na primjer, za blob put `mycontainer/documents/blob.pdf`, `documents` mogu se koristiti kao virtualna mapa.
        - Azure spremište tablica: omogućuje vam da biste naveli upit koji filtrira skup redaka koji želite uvesti.
        - Azure SQL: upita nije podržan. Ako vam je potrebna tu funkciju, provjerite glasati za [ovaj prijedlog](https://feedback.azure.com/forums/263029-azure-search/suggestions/9893490-support-user-provided-query-in-sql-indexer)
- Neobavezna `dataChangeDetectionPolicy` i `dataDeletionDetectionPolicy` svojstva opisana su u nastavku.

<a name="DataChangeDetectionPolicies"></a>
**Pravila za otkrivanje promjenu podataka**

Svrha pravilo za otkrivanje promjena podataka je učinkovito prepoznavanje stavki promijenjene podatke. Podržani pravila razlikuju se ovisno o vrsti izvora podataka. Odjeljcima opisuju svaki pravila. 

***Pravila za otkrivanje promjena vodenog žiga*** 

Koristite ovaj pravilnik kada vaš izvor podataka sadrži stupac ili svojstvo koji zadovoljava sljedeće kriterije:
 
- Umeće sve navesti vrijednost u stupcu. 
- Sva ažuriranja stavke i promijenite vrijednost stupca. 
- Povećava vrijednosti u ovom stupcu sa svim promjenama.
- Upit koji koristi izraz filtra slično sljedećem `WHERE [High Water Mark Column] > [Current High Water Mark Value]` učinkovito izvršavaju.

Na primjer, prilikom korištenja Azure SQL izvora podataka, u indeksiranim `rowversion` idealna prijedlog za uporabu s pravilima za oznaku visoke vode je stupac. 

Ovo pravilo možete navesti na sljedeći način:

    { 
        "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
        "highWaterMarkColumnName" : "[a row version or last_updated column name]" 
    } 

> [AZURE.NOTE] Prilikom korištenja DocumentDB izvora podataka, morate koristiti u `_ts` svojstvo koje ste dobili od DocumentDB. 

> [AZURE.NOTE] Prilikom korištenja blobova platforme Azure izvora podataka, pretraživanja Azure automatski koristi vodenog žiga promijeniti otkrivanje pravila na temelju s blob posljednji izmijenio vremenske oznake; ne morate odrediti takve pravilo.   

***SQL integriran promjena otkrivanje pravila***

Ako bazu podataka sustava SQL podržava [Evidentiranje promjena](https://msdn.microsoft.com/library/bb933875.aspx), preporučujemo korištenje SQL integrirani promjena praćenja pravila. Ovo pravilo omogućuje najučinkovitiji evidentiranje promjena, a omogućuje Azure pretraživanja da biste odredili izbrisane retke bez potrebe da bi stupca eksplicitnih "meki Izbriši" u sheme.

Integrirano evidentiranja promjena je podržano počevši od sljedeće verzije baze podataka SQL Server: 
- SQL Server 2008 R2, ako koristite SQL Server Azure VMs.
- Azure SQL baza podataka V12, ako koristite baze podataka SQL Azure.  

Kada pomoću SQL integrirano praćenje promjena pravila navedite pravilo za otkrivanje brisanja zasebne podatke – to pravilo ima ugrađenu podršku za označavanje izbrisati retke. 

Ovo pravilo može se koristiti samo s tablicama; nije moguće koristiti s prikazima. Morate omogućiti za tablicu koristite mogli koristiti ovaj pravilnik evidentiranje promjena. Potražite u članku [Omogućivanje i onemogućivanje evidentiranje promjena](https://msdn.microsoft.com/library/bb964713.aspx) upute.    
 
Kada strukturiranja zahtjev za **Stvaranje izvora podataka** , pravila za evidentiranje promjena SQL integriran možete navesti na sljedeći način:

    { 
        "@odata.type" : "#Microsoft.Azure.Search.SqlIntegratedChangeTrackingPolicy" 
    }

<a name="DataDeletionDetectionPolicies"></a>
**Pravila za otkrivanje brisanja podataka**

Svrha pravilo za otkrivanje brisanja podataka je učinkovito prepoznavanje stavke izbrisane podatke. Trenutno je jedini podržani pravila u `Soft Delete` pravila koja omogućuje označavanje izbrisane stavke na temelju vrijednosti u `soft delete` stupca ili svojstvo u izvoru podataka. Ovo pravilo možete navesti na sljedeći način:

    { 
        "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
        "softDeleteColumnName" : "the column that specifies whether a row was deleted", 
        "softDeleteMarkerValue" : "the value that identifies a row as deleted" 
    }

**BILJEŠKE:** Podržani su samo stupce s niza, cijeli broj ili Booleove vrijednosti. Vrijednost koja se koristi kao `softDeleteMarkerValue` mora biti niz, čak i ako se odgovarajući stupac sadrži cijele brojeve ili Booleove vrijednosti. Na primjer, ako je vrijednost koja se pojavljuje u izvoru podataka 1, koristite `"1"` kao na `softDeleteMarkerValue`.    

<a name="CreateDataSourceRequestExamples"></a>
**Primjeri tijelu zahtjeva**

Ako namjeravate koristiti izvor podataka s indeksiranje koja se izvršava na raspored ovaj primjer pokazuje kako odrediti promjena i brisanje pravila za otkrivanje: 

    { 
        "name" : "asqldatasource",
        "description" : "a description",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:....database.windows.net,1433;Database=...;User ID=...;Password=...;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "sometable" },
        "dataChangeDetectionPolicy" : { "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy", "highWaterMarkColumnName" : "RowVersion" }, 
        "dataDeletionDetectionPolicy" : { "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy", "softDeleteColumnName" : "IsDeleted", "softDeleteMarkerValue" : "true" }
    }

Ako namjeravate samo korištenje izvora podataka za jednokratni kopiju podataka, može biti ispušten pravila:

    { 
        "name" : "asqldatasource",
        "description" : "anything you want, or nothing at all",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:....database.windows.net,1433;Database=...;User ID=...;Password=...;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "sometable" }
    } 

**Odgovor**

Za uspješan zahtjev: 201 stvorena. 

<a name="UpdateDataSource"></a>
## <a name="update-data-source"></a>Ažuriranje izvora podataka ##

Možete ažurirati postojećeg izvora podataka pomoću zahtjev HTTP STAVITI. Navedite naziv izvora podataka da biste ažurirali na zahtjev za URI:

    PUT https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

Na `api-version` potreban je. Trenutna verzija nije `2015-02-28`. [Rad s verzijama Azure pretraživanje](https://msdn.microsoft.com/library/azure/dn864560.aspx) sadrži pojedinosti i dodatne informacije o druge verzije.

Na `api-key` mora biti ključa administrator (umjesto ključa upita). Pogledajte odjeljak provjere autentičnosti u [Pretraživanje servisa REST API -JA](https://msdn.microsoft.com/library/azure/dn798935.aspx) da biste saznali više o ključevima. [Stvaranje servisa za pretraživanje na portalu](search-create-service-portal.md) objašnjava kako doći do URL servisa i svojstvima ključa koji se koriste u zahtjevu za.

**Zahtjev**

Sintaksa tijelo zahtjev je kao [zahtjeva za stvaranje izvora podataka](#CreateDataSourceRequestSyntax).

> [AZURE.NOTE] Neka svojstva ne može se ažurirati na postojećeg izvora podataka. Na primjer, nije moguće promijeniti vrstu postojećeg izvora podataka.  

> [AZURE.NOTE] Ako ne želite da biste promijenili niz za povezivanje za postojećeg izvora podataka, možete odrediti na literal `<unchanged>` za niz za povezivanje. Ta je mogućnost praktična u slučajevima gdje trebate ažurirati podatkovnog izvora, no nemate jednostavan pristup niz za povezivanje Budući da je sigurnost osjetljivih podataka.

**Odgovor**

Za uspješan zahtjev za: 201 stvoreno Ako novi izvor podataka nije stvoren i 204 bez sadržaja ako je ažurirano postojećeg izvora podataka.

<a name="ListDataSource"></a>
## <a name="list-data-sources"></a>Popis izvora podataka ##

Operacija **Popis izvora podataka** vraća popis izvora podataka na servisu Azure pretraživanja. 

    GET https://[service name].search.windows.net/datasources?api-version=[api-version]
    api-key: [admin key]

Na `api-version` potreban je. Trenutna verzija nije `2015-02-28`. 

Na `api-key` mora biti ključa administrator (umjesto ključa upita). Pogledajte odjeljak provjere autentičnosti u [Pretraživanje servisa REST API -JA](https://msdn.microsoft.com/library/azure/dn798935.aspx) da biste saznali više o ključevima. [Stvaranje servisa za pretraživanje na portalu](search-create-service-portal.md) objašnjava kako doći do URL servisa i svojstvima ključa koji se koriste u zahtjevu za.

**Odgovor**

Za uspješan zahtjev: 200 u redu.

Ovo su u tijelu odgovor primjer:

    {
      "value" : [
        {
          "name": "datasource1",
          "type": "azuresql",
          ... other data source properties
        }]
    }

Imajte na umu da možete filtrirati i odgovor na svojstva koja vas zanima. Na primjer, ako želite da se samo popis naziva izvora podataka, koristiti OData `$select` upita mogućnost:

    GET /datasources?api-version=205-02-28&$select=name

U ovom slučaju odgovor gornjem primjeru pojavit će se na sljedeći način: 

    {
      "value" : [ { "name": "datasource1" }, ... ]
    }

Ovo je korisno postupak da biste spremili propusnosti ako imate mnogo izvora podataka u servis za pretraživanje.

<a name="GetDataSource"></a>
## <a name="get-data-source"></a>Dohvaćanje izvora podataka ##

Operacija **Se izvor podataka** s pretraživanja Azure dohvaća definiciju izvora podataka.

    GET https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]
    api-key: [admin key]

Na `api-version` potreban je. Trenutna verzija nije `2015-02-28`. 

Na `api-key` mora biti ključa administrator (umjesto ključa upita). Pogledajte odjeljak provjere autentičnosti u [Pretraživanje servisa REST API -JA](https://msdn.microsoft.com/library/azure/dn798935.aspx) da biste saznali više o ključevima. [Stvaranje servisa za pretraživanje na portalu](search-create-service-portal.md) objašnjava kako doći do URL servisa i svojstvima ključa koji se koriste u zahtjevu za.

**Odgovor**

Šifra stanja: 200 u redu, vraća se uspješno odgovora.

Odgovor je slična primjerima u [primjeru zahtjeva za stvaranje izvora podataka](#CreateDataSourceRequestExamples): 

    { 
        "name" : "asqldatasource",
        "description" : "a description",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:....database.windows.net,1433;Database=...;User ID=...;Password=...;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "sometable" },
        "dataChangeDetectionPolicy" : { 
            "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
            "highWaterMarkColumnName" : "RowVersion" }, 
        "dataDeletionDetectionPolicy" : { 
            "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
            "softDeleteColumnName" : "IsDeleted", 
            "softDeleteMarkerValue" : "true" }
    }

> [AZURE.NOTE] Postavljanje na `Accept` zaglavlje zahtjev za `application/json;odata.metadata=none` kada nazovete podršku za ovaj API kao time uzrokovat će `@odata.type` atribut ispustiti iz odgovor, a vi nećete moći razlikovanje promjena podataka i pravila za otkrivanje brisanja podataka različitih vrsta. 

<a name="DeleteDataSource"></a>
## <a name="delete-data-source"></a>Brisanje izvora podataka ##

Operacija **Brisanje izvora podataka** uklanja izvor podataka iz servisa Azure pretraživanja.

    DELETE https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]
    api-key: [admin key]

> [AZURE.NOTE] Ako bilo koji indexers Referenca izvora podataka koji ste brisanje; operacija brisanja će se i dalje nastaviti. Međutim, te indexers će prijelaza u stanju pogreške nakon njihove sljedećeg pokretanja.  

Na `api-version` potreban je. Trenutna verzija nije `2015-02-28`. 

Na `api-key` mora biti ključa administrator (umjesto ključa upita). Pogledajte odjeljak provjere autentičnosti u [Pretraživanje servisa REST API -JA](https://msdn.microsoft.com/library/azure/dn798935.aspx) da biste saznali više o ključevima. [Stvaranje servisa za pretraživanje na portalu](search-create-service-portal.md) objašnjava kako doći do URL servisa i svojstvima ključa koji se koriste u zahtjevu za.

**Odgovor**

Šifra stanja: 204 bez sadržaja, vraća se uspješno odgovora.

<a name="CreateIndexer"></a>
## <a name="create-indexer"></a>Stvaranje indeksiranje ##

Možete stvoriti novu indeksiranje unutar servisa za pretraživanje Azure koji se pomoću zahtjev HTTP POST.
    
    POST https://[service name].search.windows.net/indexers?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

Osim toga, možete koristiti STAVI i navedite naziv izvora podataka na URI. Ako ne postoji izvor podataka, će biti stvorena.

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]

> [AZURE.NOTE] Maksimalni broj indexers dopušteno razlikuje cijene sloju. Besplatni servis omogućuje do 3 indexers. Standardnog servisa omogućuje 50 indexers. Detalje potražite u članku [Ograničenja servisa](search-limits-quotas-capacity.md) .

Na `api-version` potreban je. Trenutna verzija nije `2015-02-28`. 

Na `api-key` mora biti ključa administrator (umjesto ključa upita). Pogledajte odjeljak provjere autentičnosti u [Pretraživanje servisa REST API -JA](https://msdn.microsoft.com/library/azure/dn798935.aspx) da biste saznali više o ključevima. [Stvaranje servisa za pretraživanje na portalu](search-create-service-portal.md) objašnjava kako doći do URL servisa i svojstvima ključa koji se koriste u zahtjevu za.


<a name="CreateIndexerRequestSyntax"></a>
**Sintaksa tijelu zahtjeva**

U tijelu zahtjeva za sadrži definiciju indeksiranje koji određuje izvor podataka i ciljne indeks za indeksiranje, kao i neobavezna raspored indeksiranja i parametre. 


Vidjet ćete da sintaksa za strukturiranje opseg zahtjev je na sljedeći način. Zahtjev za uzorak navedeni su dodatno na u ovoj temi.

    { 
        "name" : "Required for POST, optional for PUT. The name of the indexer",
        "description" : "Optional. Anything you want, or null",
        "dataSourceName" : "Required. The name of an existing data source",
        "targetIndexName" : "Required. The name of an existing index",
        "schedule" : { Optional. See Indexing Schedule below. },
        "parameters" : { Optional. See Indexing Parameters below. },
        "fieldMappings" : { Optional. See Field Mappings below. },
        "disabled" : Optional boolean value indicating whether the indexer is disabled. False by default.  
    }

**Raspored za indeksiranje**

Indeksiranje po želji možete odrediti raspored. Ako postoji raspored indeksiranje će se pokrenuti povremeno po raspored. Raspored sadrži sljedećim atributima:

- `interval`: Potreban. Pokreće se trajanje vrijednost koja određuje interval ili razdoblje, za indeksiranje. Najmanji dopušteni interval je pet minuta; na najdulje je jedan dan. Moraju biti oblikovani kao vrijednost "dayTimeDuration" XSD (ograničeni podskup vrijednost [ISO 8601 trajanje](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) ). Obrazac za to je: `"P[nD][T[nH][nM]]"`. Primjeri: `PT15M` svakih 15 minuta `PT2H` za svaku dva sata. 

- `startTime`: Potreban. U UTC datuma i vremena kada indeksiranje bi trebao počinjati pokrenut. 

**Parametri za indeksiranje**

Indeksiranje po želji možete navesti nekoliko parametara koje utječu na njegovo ponašanje. Sve parametre nisu obavezni.  

- `maxFailedItems`: Broj stavki koje mogu se neće indeksirati prije pokretanja programa za indeksiranje smatra se pogreška. Zadana vrijednost je 0. Informacije o stavke, vraća se postupkom [Dobiti Status komponente za indeksiranje](#GetIndexerStatus) . 

- `maxFailedItemsPerBatch`: Broj stavki koje mogu se neće indeksirati svaku seriju prije pokretanja programa za indeksiranje smatra se pogreška. Zadana vrijednost je 0.

- `base64EncodeKeys`: Određuje hoće li tipke dokument biti base-64 kodiran. Azure pretraživanje uvodi ograničenja za znakove koji se mogu postojati u ključu dokumenta. Vrijednosti u izvoru podataka, međutim, možda sadrži znakove koji nisu valjani. Ako je potrebno indeksirati takve vrijednosti kao dokument tipke, možete postaviti se oznaka na true. Zadana vrijednost je `false`.

- `batchSize`: Određuje broj stavki koji se čitaju iz izvora podataka i indeksirati kao jedan grupe da biste poboljšali performanse. Zadani ovisi o vrsti izvora podataka: je 1000 za Azure SQL i DocumentDB i 10 za spremište blobova platforme Azure.

**Mapiranje polja**

Mapiranje polja možete koristiti za mapiranje naziva polja u izvoru podataka u drugi naziv polja u indeksu cilj. Na primjer, razmotrite izvorišnu tablicu s poljem `_id`. Azure pretraživanje ne dopušta počevši od podvlaka tako da se polje mora biti preimenovana naziv polja. To možete učiniti pomoću na `fieldMappings` svojstvo indeksiranje na sljedeći način: 
    
    "fieldMappings" : [ { "sourceFieldName" : "_id", "targetFieldName" : "id" } ] 

Možete navesti više mapiranja polja: 

    "fieldMappings" : [ 
        { "sourceFieldName" : "_id", "targetFieldName" : "id" },
        { "sourceFieldName" : "_timestamp", "targetFieldName" : "timestamp" },
     ]

Izvorišno i odredišno polje imena su velika i mala slova.

<a name="FieldMappingFunctions"></a>
***Funkcija mapiranje polja***

Mapiranje polja i omogućuje pretvaranje izvorišnih polja vrijednosti pomoću *funkcije mapiranje*.

Trenutno je podržano samo jedan takve funkcija: `jsonArrayToStringCollection`. Ga raščlanjuje polje koje sadrži niz koji je oblikovan kao JSON polja u polje Collection(Edm.String) u indeksu cilj. Je namijenjen za korištenje sa Azure SQL indeksiranje posebno jer SQL ne sadrži vrstu nativni zbirke podataka. Može se koristiti na sljedeći način: 

    "fieldMappings" : [ { "sourceFieldName" : "tags", "mappingFunction" : { "name" : "jsonArrayToStringCollection" } } ] 

Na primjer, ako je izvorno polje sadrži niz `["red", "white", "blue"]`, zatim ciljno polje Vrsta `Collection(Edm.String)` će popunjen tri vrijednosti `"red"`, `"white"` i `"blue"`.

Imajte na umu da se `targetFieldName` svojstvo nije obavezno; Ako izostavljeni, u `sourceFieldName` koristi vrijednost. 

<a name="CreateIndexerRequestExamples"></a>
**Primjeri tijelu zahtjeva**

Sljedeći primjer stvara indeksiranje koja se kopira podatke iz tablice upućuje na `ordersds` izvora podataka da biste na `orders` indeksa na raspored koji započinje Sij 1, 2015 UTC-a i pokreće zaračunava. Svaki poziva za indeksiranje neće biti uspješan ako najviše 5 stavki koji se neće indeksirati svaku seriju i više od 10 stavki nije uspjelo indeksiranje Ukupno. 

    {
        "name" : "myindexer",
        "description" : "a cool indexer",
        "dataSourceName" : "ordersds",
        "targetIndexName" : "orders",
        "schedule" : { "interval" : "PT1H", "startTime" : "2015-01-01T00:00:00Z" },
        "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 5, "base64EncodeKeys": false }
    }

**Odgovor**

201 stvoreno za zahtjev za uspješan.


<a name="UpdateIndexer"></a>
## <a name="update-indexer"></a>Ažuriranje za indeksiranje ##

Možete ažurirati postojeće indeksiranje pomoću zahtjev HTTP STAVITI. Navedite naziv komponente za indeksiranje da biste ažurirali na zahtjev za URI:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

Na `api-version` potreban je. Trenutna verzija nije `2015-02-28`. 

Na `api-key` mora biti ključa administrator (umjesto ključa upita). Pogledajte odjeljak provjere autentičnosti u [Pretraživanje servisa REST API -JA](https://msdn.microsoft.com/library/azure/dn798935.aspx) da biste saznali više o ključevima. [Stvaranje servisa za pretraživanje na portalu](search-create-service-portal.md) objašnjava kako doći do URL servisa i svojstvima ključa koji se koriste u zahtjevu za.

**Zahtjev**

Sintaksa tijelo zahtjev, jednako je kao [zahtjeva za stvaranje indeksiranje](#CreateIndexerRequestSyntax).

**Odgovor**

Za uspješan zahtjev: 201 stvoreno ako je novi program za indeksiranje stvoren i 204 bez sadržaja ako postojeće indeksiranje ažuriran.


<a name="ListIndexers"></a>
## <a name="list-indexers"></a>Popis Indexers ##

Operacija **Indexers popis** vraća popis indexers na servisu Azure pretraživanja. 

    GET https://[service name].search.windows.net/indexers?api-version=[api-version]
    api-key: [admin key]


Na `api-version` potreban je. Pretpregled verzija `2015-02-28-Preview`. [Rad s verzijama Azure pretraživanje](https://msdn.microsoft.com/library/azure/dn864560.aspx) sadrži pojedinosti i dodatne informacije o druge verzije.

Na `api-key` mora biti ključa administrator (umjesto ključa upita). Pogledajte odjeljak provjere autentičnosti u [Pretraživanje servisa REST API -JA](https://msdn.microsoft.com/library/azure/dn798935.aspx) da biste saznali više o ključevima. [Stvaranje servisa za pretraživanje na portalu](search-create-service-portal.md) objašnjava kako doći do URL servisa i svojstvima ključa koji se koriste u zahtjevu za.

**Odgovor**

Za uspješan zahtjev: 200 u redu.

Ovo su u tijelu odgovor primjer:

    {
      "value" : [
      {
        "name" : "myindexer",
        "description" : "a cool indexer",
        "dataSourceName" : "ordersds",
        "targetIndexName" : "orders",
        ... other indexer properties
      }]
    }

Imajte na umu da možete filtrirati i odgovor na svojstva koja vas zanima. Na primjer, ako želite da se samo popis imena indeksiranje, koristiti OData `$select` upita mogućnost:

    GET /indexers?api-version=2014-10-20-Preview&$select=name

U ovom slučaju odgovor gornjem primjeru pojavit će se na sljedeći način: 

    {
      "value" : [ { "name": "myindexer" } ]
    }

Ovo je korisno postupak da biste spremili propusnosti ako imate mnogo indexers u servis za pretraživanje.


<a name="GetIndexer"></a>
## <a name="get-indexer"></a>Početak indeksiranje ##

Postupak **Dohvati indeksiranje** dobiva definiciju indeksiranje pretraživanja Azure.

    GET https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]
    api-key: [admin key]

Na `api-version` potreban je. Pretpregled verzija `2015-02-28-Preview`. 

Na `api-key` mora biti ključa administrator (umjesto ključa upita). Pogledajte odjeljak provjere autentičnosti u [Pretraživanje servisa REST API -JA](https://msdn.microsoft.com/library/azure/dn798935.aspx) da biste saznali više o ključevima. [Stvaranje servisa za pretraživanje na portalu](search-create-service-portal.md) objašnjava kako doći do URL servisa i svojstvima ključa koji se koriste u zahtjevu za.

**Odgovor**

Šifra stanja: 200 u redu, vraća se uspješno odgovora.

Odgovor je slična primjerima u [zahtjevima za indeksiranje za stvaranje primjera](#CreateIndexerRequestExamples): 

    {
        "name" : "myindexer",
        "description" : "a cool indexer",
        "dataSourceName" : "ordersds",
        "targetIndexName" : "orders",
        "schedule" : { "interval" : "PT1H", "startTime" : "2015-01-01T00:00:00Z" },
        "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 5, "base64EncodeKeys": false }
    }


<a name="DeleteIndexer"></a>
## <a name="delete-indexer"></a>Brisanje komponente za indeksiranje ##

**Brisanje indeksiranje** postupak uklanja indeksiranje iz servisa Azure pretraživanja.

    DELETE https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]
    api-key: [admin key]

Nakon brisanja indeksiranje izvršavanja indeksiranje u tijeku tada će se pokrenuti do završetka, no bez daljnje izvršavanja će se zakazati. Pokušava koristiti nepostojećeg indeksiranje rezultirat će Šifra stanja HTTP 404 nije pronađeno. 
 
Na `api-version` potreban je. Pretpregled verzija `2015-02-28-Preview`. 

Na `api-key` mora biti ključa administrator (umjesto ključa upita). Pogledajte odjeljak provjere autentičnosti u [Pretraživanje servisa REST API -JA](https://msdn.microsoft.com/library/azure/dn798935.aspx) da biste saznali više o ključevima. [Stvaranje servisa za pretraživanje na portalu](search-create-service-portal.md) objašnjava kako doći do URL servisa i svojstvima ključa koji se koriste u zahtjevu za.

**Odgovor**

Šifra stanja: 204 bez sadržaja, vraća se uspješno odgovora.

<a name="RunIndexer"></a>
## <a name="run-indexer"></a>Pokretanje komponente za indeksiranje ##

Uz pokrenut povremeno na raspored, indeksiranje mogu se pozvati i na zahtjev putem **Pokrenuti indeksiranje** postupak: 

    POST https://[service name].search.windows.net/indexers/[indexer name]/run?api-version=[api-version]
    api-key: [admin key]

Na `api-version` potreban je. Pretpregled verzija `2015-02-28-Preview`. 

Na `api-key` mora biti ključa administrator (umjesto ključa upita). Pogledajte odjeljak provjere autentičnosti u [Pretraživanje servisa REST API -JA](https://msdn.microsoft.com/library/azure/dn798935.aspx) da biste saznali više o ključevima. [Stvaranje servisa za pretraživanje na portalu](search-create-service-portal.md) objašnjava kako doći do URL servisa i svojstvima ključa koji se koriste u zahtjevu za.

**Odgovor**

Šifra stanja: 202 prihvaćena, vraća se uspješno odgovora.

<a name="GetIndexerStatus"></a>
## <a name="get-indexer-status"></a>Dohvatite Status indeksiranje ##

Operacija **Se Status indeksiranje** dohvaća trenutni povijest statusa i izvođenja indeksiranje: 

    GET https://[service name].search.windows.net/indexers/[indexer name]/status?api-version=[api-version]
    api-key: [admin key]


Na `api-version` potreban je. Pretpregled verzija `2015-02-28-Preview`. 

Na `api-key` mora biti ključa administrator (umjesto ključa upita). Pogledajte odjeljak provjere autentičnosti u [Pretraživanje servisa REST API -JA](https://msdn.microsoft.com/library/azure/dn798935.aspx) da biste saznali više o ključevima. [Stvaranje servisa za pretraživanje na portalu](search-create-service-portal.md) objašnjava kako doći do URL servisa i svojstvima ključa koji se koriste u zahtjevu za.

**Odgovor**

Šifra stanja: 200 u redu za uspješan odgovor.

Odgovor tijelo sadrži informacije o ukupnog statusa stanja indeksiranje, zadnjeg poziva za indeksiranje, kao i povijest nedavne instanci komponente za indeksiranje (ako postoje). 

Tijelo odgovor uzorka izgleda ovako: 

    {
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
         },
        "executionHistory":[ {
            "status":"success",
            "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
        }]
    }

**Status indeksiranje**

Status indeksiranje može biti jedna od sljedećih vrijednosti:

- `running`upućuje na to da indeksiranje normalno. Imajte na umu da neke komponente za indeksiranje izvršavanja mogu i dalje se ne uspijeva, pa je dobro provjeriti na `lastResult` kao i svojstva. 

- `error`upućuje na to indeksiranje došlo je do pogreške koja se ne može ispraviti bez Ljudski intervencije. Ako, na primjer, vjerodajnicama izvora podataka možda istekla ili shemi izvora podataka ili indeksa ciljne se promijenilo u na važne način. 

**Rezultat izvođenja indeksiranje**

I rezultat izvođenja indeksiranje sadrži podatke o jednom indeksiranje izvršavanja. Najnovije rezultat je kada povučete kao u `lastResult` svojstvo stanja za indeksiranje. Ostale nedavne, ako postoje, rezultata kao u `executionHistory` svojstvo stanja za indeksiranje. 

Rezultat izvođenja indeksiranje sadrži sljedeća svojstva: 

- `status`: stanje sustava izvršavanja. Detalje potražite u članku [Indeksiranje izvođenja Status](#IndexerExecutionStatus) ispod. 

- `errorMessage`: poruka o pogrešci za Neuspjelo izvršavanje. 

- `startTime`: vrijeme u UTC-a kada ovaj izvođenja pokreće.

- `endTime`: vrijeme u UTC-a kada je završen ovaj izvršavanja. Ta vrijednost nije postavljen ako izvršavanja još uvijek je u tijeku.

- `errors`: polja pogreške na razini stavke, ako ih ima. Svaka stavka sadrži ključ dokumenta (`key` svojstvo) i poruka o pogrešci (`errorMessage` svojstvo). 

- `itemsProcessed`: broj podataka izvora stavke (na primjer, reci tablice) indeksiranje pokušali indeksirati tijekom ovog izvršavanja. 

- `itemsFailed`: broj stavki koje nije uspjela tijekom ovog izvođenja.  
 
- `initialTrackingState`: uvijek `null` za prvu izvršavanje indeksiranje, ili ako se podaci mijenjaju praćenje pravila nije omogućena na izvor podataka koji se koristi. Ako je omogućeno takvog pravilo, u sljedećim izvršavanja tu vrijednost pokazuje na prvi (najmanje) evidentiranja promjena vrijednosti koje su obradili ovaj izvođenja. 

- `finalTrackingState`: uvijek `null` ako se podaci mijenjaju praćenje pravila nije omogućena na izvor podataka koji se koristi. U suprotnom označava evidentiranje vrijednost uspješno obradili ovaj izvođenja najnovije promjena (najviša rezolucija). 

<a name="IndexerExecutionStatus"></a>
**Status izvođenja indeksiranje**

Status izvođenja indeksiranje snima status jedan indeksiranje izvršavanja. Može imati sljedeće vrijednosti:

- `success`upućuje na to je uspješno dovršena izvođenja komponente za indeksiranje.

- `inProgress`upućuje na to da izvođenja Indeksiranje je u tijeku. 

- `transientFailure`upućuje na to je indeksiranje izvođenja nije uspjela. U odjeljku `errorMessage` svojstvo detalje. Neuspjeh možda ili ne zahtijeva Ljudski intervencije da biste riješili problem,-, na primjer, rješavanje nekompatibilnost programa za sheme između izvora podataka i indeksa cilj zahtijeva akcije korisnika, dok je isključiti za izvor podataka privremene ne. Indeksiranje instanci će i dalje po rasporedu, ako je prezentacija. 

- `persistentFailure`upućuje na to da indeksiranje nije uspjela na način koji zahtijeva Ljudski intervencije. Zaustavi izvršavanja zakazanog indeksiranje. Nakon adresiranja problem, koristite Vrati API za indeksiranje da biste ponovno pokrenuli zakazani izvršavanja. 

- `reset`upućuje na to da ponovno indeksiranje po poziv za ponovno postavljanje indeksiranje API-JA (pogledajte informacije u nastavku). 

<a name="ResetIndexer"></a>
## <a name="reset-indexer"></a>Ponovno postavljanje programa za indeksiranje ##

Operacija **Vrati indeksiranje** vraća promjena praćenja stanja pridružene indeksiranje. Omogućuje vam pokretanje iz početka ponovnim indeksiranjem (na primjer, ako je promijenjena sheme izvor podataka) ili promijenite Pravilnik za otkrivanje promjena podataka povezan s indeksiranje izvora podataka.   

    POST https://[service name].search.windows.net/indexers/[indexer name]/reset?api-version=[api-version]
    api-key: [admin key]

Na `api-version` potreban je. Pretpregled verzija `2015-02-28-Preview`. 

Na `api-key` mora biti ključa administrator (umjesto ključa upita). Pogledajte odjeljak provjere autentičnosti u [Pretraživanje servisa REST API -JA](https://msdn.microsoft.com/library/azure/dn798935.aspx) da biste saznali više o ključevima. [Stvaranje servisa za pretraživanje na portalu](search-create-service-portal.md) objašnjava kako doći do URL servisa i svojstvima ključa koji se koriste u zahtjevu za.

**Odgovor**

Šifra stanja: 204 bez sadržaja za uspješan odgovor.

## <a name="mapping-between-sql-data-types-and-azure-search-data-types"></a>Mapiranje između SQL vrste podataka i vrste podataka Azure pretraživanja ##

<table style="font-size:12">
<tr>
<td>Vrsta podataka za SQL</td>  
<td>Indeks ciljne dopuštene vrste polja</td>
<td>Bilješke</td>
</tr>
<tr>
<td>bitne</td>
<td>Edm.Boolean, Edm.String</td>
<td></td>
</tr>
<tr>
<td>INT, smallint, tinyint</td>
<td>Edm.Int32, Edm.Int64, Edm.String</td>
<td></td>
</tr>
<tr>
<td>bigint</td>
<td>Edm.Int64, Edm.String</td>
<td></td>
</tr>
<tr>
<td>realni, plutati</td>
<td>Edm.Double, Edm.String</td>
<td></td>
</tr>
<tr>
<td>smallmoney, novca<br>broja decimalnih mjesta<br>brojčani
</td>
<td>Edm.String</td>
<td>Azure pretraživanje ne podržava vrpce decimalni vrste Edm.Double jer je to ćete izgubiti preciznosti
</td>
</tr>
<tr>
<td>CHAR, nchar, varchar, nvarchar</td>
<td>Edm.String<br/>Collection(EDM.String)</td>
<td>U ovom dokumentu pojedinosti o transformiraju niz stupac u na Collection(Edm.String) potražite u članku [Funkcije mapiranje polja](#FieldMappingFunctions)</td>
</tr>
<tr>
<td>smalldatetime, datumom i vremenom, datetime2, datum, datetimeoffset</td>
<td>Edm.DateTimeOffset, Edm.String</td>
<td></td>
</tr>
<tr>
<td>uniqueidentifer</td>
<td>Edm.String</td>
<td></td>
</tr>
<tr>
<td>Zemljopis</td>
<td>Edm.GeographyPoint</td>
<td>Podržani su samo Zemljopis instanci vrste TOČKU s 4326 SRID (što je zadana postavka)</td>
</tr>
<tr>
<td>ROWVERSION</td>
<td>N/D</td>
<td>Redak verziju stupaca nije moguće spremiti u indeks pretraživanja, ali se može koristiti za evidentiranje promjena</td>
</tr>
<tr>
<td>vrijeme, vremenski raspon<br>Binarni, varbinary, slika,<br>XML geometriju, vrste CLR</td>
<td>N/D</td>
<td>Nije podržano</td>
</tr>
</table>

## <a name="mapping-between-json-data-types-and-azure-search-data-types"></a>Mapiranje između JSON vrste podataka i vrste podataka Azure pretraživanja ##

<table style="font-size:12">
<tr>
<td>Vrsta podataka JSON</td> 
<td>Indeks ciljne dopuštene vrste polja</td>
<td>Bilješke</td>
</tr>
<tr>
<td>booleovom</td>
<td>Edm.Boolean, Edm.String</td>
<td></td>
</tr>
<tr>
<td>Cjelobrojni brojeva</td>
<td>Edm.Int32, Edm.Int64, Edm.String</td>
<td></td>
</tr>
<tr>
<td>S pomičnim zarezom brojeva</td>
<td>Edm.Double, Edm.String</td>
<td></td>
</tr>
<tr>
<td>niz</td>
<td>Edm.String</td>
<td></td>
</tr>
<tr>
<td>polja prim vrste, npr. ["a", "b"; "c"]</td>
<td>Collection(EDM.String)</td>
<td></td>
</tr>
<tr>
<td>Nizovi koji izgleda kao i kod datuma</td>
<td>Edm.DateTimeOffset, Edm.String</td>
<td></td>
</tr>
<tr>
<td>GeoJSON točke objekata</td>
<td>Edm.GeographyPoint</td>
<td>Točke GeoJSON su JSON objekti u sljedećem obliku: {"Vrsta": "Mjesto", "koordinate": [dugo, lat]} </td>
</tr>
<tr>
<td>Ostale objekte JSON</td>
<td>N/D</td>
<td>Nije podržana. Azure pretraživanje trenutno podržava samo jednostavne vrste i zbirki niza</td>
</tr>
</table>