<properties 
    pageTitle="Sortiranje podataka DocumentDB pomoću Order By | Microsoft Azure" 
    description="Saznajte kako koristiti ORDER BY u upitima DocumentDB u LINQ i SQL i određivanje indeksiranja pravila za upite za ORDER BY." 
    services="documentdb" 
    authors="arramac" 
    manager="jhubbard" 
    editor="cgronlun" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/03/2016" 
    ms.author="arramac"/>

# <a name="sorting-documentdb-data-using-order-by"></a>Sortiranje podataka DocumentDB pomoću Order By
Microsoft Azure DocumentDB podržava upite dokumenata putem SQL JSON dokumenata. Rezultati upita možete naručiti pomoću uvjet ORDER BY u SQL naredbe upita.

Kad pročitate članak ćete je moći odgovaraju na sljedeća pitanja: 

- Kako se upita s Order By?
- Kako konfigurirati indeksiranja pravila za Order By?
- Dolazi što dalje?

[Uzorci](#samples) i [Najčešća pitanja vezana uz](#faq) i dobili.

Referenca na SQL upita potražite u članku [vodič DocumentDB upita](documentdb-sql-query.md).

## <a name="how-to-query-with-order-by"></a>Upute za upit s redoslijedom prema vremenu
Kao što su u ANSI SQL sad možete uključiti uvjet neobavezno Order By u SQL naredbe prilikom postavljanja upita DocumentDB. Uvjet možete uključiti neobavezni argument ASC/DESC za određivanje redoslijeda u kojima mora biti dohvaćeni rezultate. 

### <a name="ordering-using-sql"></a>Redoslijed pomoću SQL
Na primjer ovdje je upit za dohvaćanje knjige prvih 10 silaznim redoslijedom naslovima. 

    SELECT TOP 10 * 
    FROM Books 
    ORDER BY Books.Title DESC

### <a name="ordering-using-sql-with-filtering"></a>Redoslijed pomoću SQL filtriranje
Redoslijed pomoću bilo koje ugniježđene svojstvo unutar dokumenata kao što su Books.ShippingDetails.Weight, a dodatne filtre možete odrediti u WHERE u kombinaciji s Order By kao u ovom primjeru:

    SELECT * 
    FROM Books 
    WHERE Books.SalePrice > 4000
    ORDER BY Books.ShippingDetails.Weight

### <a name="ordering-using-the-linq-provider-for-net"></a>Redoslijed pomoću LINQ davatelja usluga za .NET
Pomoću SDK .NET verzije 1.2.0 i noviji, možete koristiti uvjet OrderBy() ili OrderByDescending() unutar LINQ upita kao u ovom primjeru:

    foreach (Book book in client.CreateDocumentQuery<Book>(UriFactory.CreateDocumentCollectionUri("db", "books"))
        .OrderBy(b => b.PublishTimestamp)
        .Take(100))
    {
        // Iterate through books
    }

DocumentDB podržava redoslijed s jednom numeričke, niz ili logičko svojstvo po upitu, s vrstama upita dodatne uskoro dostupno. Pogledajte [što uskoro sljedeće](#Whats_coming_next) dodatne detalje.

## <a name="configure-an-indexing-policy-for-order-by"></a>Konfiguracija indeksiranja pravila za Order By

Opoziv podržava li DocumentDB dvije vrste indeksa (raspršivanje i raspon), koje možete postaviti određene putove/svojstva, vrste podataka (nizovi na brojeve) i u različitim točnost vrijednosti (maksimalno preciznosti ili fiksnom preciznošću vrijednost). Budući da DocumentDB koristi raspršivanje indeksiranja kao zadani, morate stvoriti novu zbirku s prilagođene indeksiranja pravila s rasponom brojeva, nizovi ili oboje, da biste mogli koristiti Order By. 

>[AZURE.NOTE] Indeksi raspon niz uvedene na srpanj 7, 2015 s REST API-JA verzija 2015 06 03. Za stvaranje pravila za Order By protiv nizovi, morate koristiti SDK verziju 1.2.0 .NET SDK ili verzije 1.1.0 Python, Node.js ili Java SDK.
>
>Prije no što REST API-JA verzija 2015 06 03, zadani pravilnik indeksiranja zbirke nije raspršivanje nizove i brojeva. To je promijenjen Raspršivanje za nizove i raspona brojeva. 

Dodatne informacije potražite u članku [DocumentDB indeksiranja pravila](documentdb-indexing-policies.md).

### <a name="indexing-for-order-by-against-all-properties"></a>Indeksiranje za Order By protiv sva svojstva
Evo kako možete stvoriti zbirke web s "Sve raspon" indeksiranje za Order By na temelju bilo koje/sve numeričke ili niz svojstava koje se pojavljuju u JSON dokumentima u njoj. Ovdje ćemo zamijeniti zadanu vrstu indeks za vrijednosti niza raspon i na maksimalni preciznost (-1).
                   
    DocumentCollection books = new DocumentCollection();
    books.Id = "books";
    books.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });
    
    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), books);  

>[AZURE.NOTE] Imajte na umu da Order By će vratiti samo rezultate vrste podataka (niz i broj) indeksiranih s na RangeIndex. Ako, na primjer, ako imate zadani indeksiranje pravila koja sadrži samo RangeIndex brojeva, Order By protiv put s vrijednosti niza će vratiti nema dokumenata.

### <a name="indexing-for-order-by-for-a-single-property"></a>Indeksiranje za Order By za jedno svojstvo
Evo kako možete stvoriti zbirka s indeksiranje za Order By protiv samo svojstvo naslova, što je niz. Postoje dva puta, jedan za svojstvo naslova ("/ Naslov /?") s indeksiranje raspona, a drugo za svaku svojstvo s zadanu indeksiranja shemu, što je ključ za nizove i raspon za brojeve.                    
    
    booksCollection.IndexingPolicy.IncludedPaths.Add(
        new IncludedPath { 
            Path = "/Title/?", 
            Indexes = new Collection<Index> { 
                new RangeIndex(DataType.String) { Precision = -1 } } 
            });
    
    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), booksCollection);  


## <a name="samples"></a>Uzorci
Pogledajte ovaj [Github uzoraka projekta](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/Queries) koji pokazuje kako koristiti Order By, uključujući stvaranje pravila za indeksiranje i označavanje stranica pomoću Order By. Primjere su Otvori izvor i Pozivamo vas slanje zahtjeva za istaknuti prilozima koji nije moguće koristiti druge DocumentDB razvojnim inženjerima. Pogledajte [doprinos smjernice](https://github.com/Azure/azure-documentdb-net/blob/master/Contributing.md) za upute za suradnju.  

## <a name="faq"></a>NAJČEŠĆA PITANJA

**Što je očekivani potrošnje zahtjev jedinica (Pravi) Order By upita?**

Budući da Order By koristi DocumentDB indeks pretraživanja, broj jedinica zahtjev potrošena Order By upiti će biti sličan ekvivalentan upita bez Order By. Kao što je bilo koje druge operacije na DocumentDB, broj jedinica zahtjev ovisi o veličine i oblika dokumenata, kao i složenost upita. 


**Što je očekivani indeksiranje indirektni za Order By?**

Indeksiranja indirektni pohrane bit će proporcionalno broj svojstava. U na Scenarij najgoreg slučaja, indirektni indeks će biti 100% podataka. Nema razlike u indirektni propusnost (zahtjev jedinice) između raspona/Order By indeksiranja i indeksiranje zadani raspršivanje.

**Kako za upite Moje postojeće podatke u DocumentDB pomoću Order By**

Da biste sortirali rezultate upita pomoću Order By, mijenjate indeksiranja pravila zbirke da biste koristili vrstu indeks raspon protiv svojstvo koristi za sortiranje. Potražite u članku [Izmjena indeksiranje pravila](documentdb-indexing-policies.md#modifying-the-indexing-policy-of-a-collection). 

**Što su trenutno ograničenja Order By?**

Order By možete navesti samo s svojstvo ili numerički ili niz kada je raspon indeksirana s maksimalno preciznost (-1).

Ne možete izvršiti sljedeće:
 
- Order By sa svojstvima Interna niz kao što je id, _rid i _isto (uskoro dostupno).
- Order By sa svojstvima izvedene iz rezultata na spoj intra dokumenta (uskoro dostupno).
- Poredaj prema više svojstva (uskoro dostupno).
- Order By s upitima na baze podataka, zbirke, korisnici, dozvole ili privici (uskoro dostupno).
- Poredaj po sa svojstvima izračunatom npr rezultat izraza ili UDF/built-u funkciji.

Order By je trenutno ne podržava više particija upita pri korištenju programa Explorer upita na portalu za Azure.

## <a name="troubleshooting"></a>Otklanjanje poteškoća

Ako se pojavi pogreška da Order By nije podržan, provjerite koristite li verziju [SDK](documentdb-sdk-dotnet.md) koji podržava Order By. 

## <a name="next-steps"></a>Daljnji koraci

Fork [Github uzoraka projekta](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/Queries) i počnite redoslijed podataka! 

## <a name="references"></a>Reference
* [Referenca DocumentDB upita](documentdb-sql-query.md)
* [Referenca za pravila DocumentDB indeksiranja](documentdb-indexing-policies.md)
* [Referenca za DocumentDB SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)
* [DocumentDB Order By uzorka](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/Queries)
 

