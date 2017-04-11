<properties
    pageTitle="Upit indeksu pretraživanja Azure | Microsoft Azure | Servis za pretraživanje glavnom računalu oblaka"
    description="Sastavljanje upita za pretraživanje u odjeljku Azure pretraživanje i korištenje parametara pretraživanja za filtriranje i sortiranje rezultata pretraživanja."
    services="search"
    manager="jhubbard"
    documentationCenter=""
    authors="ashmaka"
/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="query-your-azure-search-index"></a>Indeks pretraživanja Azure za upite
> [AZURE.SELECTOR]
- [Pregled](search-query-overview.md)
- [Portal](search-explorer.md)
- [.NET](search-query-dotnet.md)
- [OSTALE](search-query-rest-api.md)

Prilikom slanja zahtjeva za pretraživanje pretraživanje Azure, postoji nekoliko parametara koje možete navesti uzduž stvarni riječi upisane u okvir za pretraživanje aplikacije. Parametara upita omogućuju da biste postigli neke dublju kontrolu nad sučelje za pretraživanje po cijelom tekstu.

U nastavku je popis koji ukratko objašnjava uobičajene namjene parametara upit u pretraživanju Azure. Puni opseg parametara upita i njihovo ponašanje, potražite u članku detaljne stranica za [REST API -JA](https://msdn.microsoft.com/library/azure/dn798927.aspx) i [.NET SDK](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.searchparameters_properties.aspx).

## <a name="types-of-queries"></a>Vrste upita

Azure pretraživanje nudi mnoge mogućnosti za stvaranje iznimno naprednih upita. Dvije glavne vrste upita koristit će se `search` i `filter`. A `search` upit traži jedan ili više izraza u svim poljima _pretraživanja_ u indeks, a funkcionira onako kako ste očekivali tražilicama kao što su Google ili Bing za rad. A `filter` upita procjenjuje Booleov izraz preko svih _koje je moguće filtrirati_ polja u indeks. Za razliku od `search` upita, `filter` upiti odgovara točno sadržaj polja, što znači da su velika i mala slova niza polja.

Pomoću pretraživanja i filtri zajedno ili zasebno. Ako koristite ih zajedno, najprije primjenjuje filtar na cijeli indeks, a zatim izvodi pretraživanje na rezultate filtra. Filtri stoga može biti korisno postupak da biste poboljšali performanse upita jer oni smanjiti skup dokumenata koju je potrebno upit za pretraživanje za obradu.

Vidjet ćete da sintaksa za izraze za filtar je podskup [OData filtar jezik](https://msdn.microsoft.com/library/azure/dn798921.aspx). Za upite pretraživanja možete koristiti [pojednostavljeni sintaksa](https://msdn.microsoft.com/library/azure/dn798920.aspx) ili [Lucene sintaksu upita](https://msdn.microsoft.com/library/azure/mt589323.aspx) koji se spominju u nastavku.

### <a name="simple-query-syntax"></a>Sintaksa za jednostavne upite
[Sintaksa za jednostavne upite](https://msdn.microsoft.com/library/azure/dn798920.aspx) je zadani jezik upita koristili u pretraživanju Azure. Sintaksa za jednostavne upite podržava broj Uobičajeni operatori za pretraživanje uključujući AND, OR, NOT, izraz, sufiks i prednost operatora.

### <a name="lucene-query-syntax"></a>Sintaksa upita Lucene
[Sintaksa upita Lucene](https://msdn.microsoft.com/library/azure/mt589323.aspx) omogućuje vam korištenje jezik široko prihvatile i izražajna upita koji je razvio kao dio [Apache Lucene](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html).

Pomoću ove sintakse upita omogućuje jednostavno postigli sljedeće mogućnosti: [iz djelokruga polja upita](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_fields), [mutno pretraživanja](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_fuzzy), [Blizina pretraživanja](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_proximity), [povećanje termina](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_termboost), [Uobičajeni izraz pretraživanja](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_regex), [zamjenskih pretraživanje](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_wildcard), [Osnove sintaksa](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_syntax)i [upita pomoću logički operatori](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_boolean).



## <a name="ordering-results"></a>Redoslijed rezultata
Prilikom primanja rezultate upita za pretraživanje, možete zatražiti da pretraživanje Azure služi rezultate poredane po vrijednosti u određenom polju. Prema zadanim postavkama pretraživanja Azure narudžbe rezultate pretraživanja na temelju položaj broja rezultata pretraživanja svaki dokument, dobivena iz [TF IDF](https://en.wikipedia.org/wiki/Tf%E2%80%93idf).

Ako želite da se Azure pretraživanje tako da vraća rezultate poredane po vrijednosti umjesto rezultata pretraživanja možete koristiti u `orderby` pretraživati parametar. Možete navesti vrijednost u `orderby` parametar sadrže nazive polja i pozive u [ `geo.distance()` funkcija](https://msdn.microsoft.com/library/azure/dn798921.aspx) geoprostornim vrijednosti. Svaki izraz možete slijedi `asc` da biste naznačili da se rezultati zatražili uzlaznim redoslijedom, a `desc` da biste naznačili da se rezultati potrebne silazno. Zadani rang uzlaznim redoslijedom.

## <a name="paging"></a>Broja stranica
Azure pretraživanje olakšava implementirati broja stranica rezultata pretraživanja. Pomoću na `top` i `skip` parametre možete provođenju izdati zahtjeva za pretraživanje koje vam omogućuju da primaju ukupni skup rezultata pretraživanja u kojima se upravlja, uređeni podskupova koje se jednostavno omogućiti prakse dobar pretraživanje korisničkog Sučelja. Prilikom primanja te manje podskupova rezultata, možete primati i broj dokumenata u ukupni skup rezultata pretraživanja.

Možete se Saznajte više o po stranicama rezultata pretraživanja u članku [kako na stranicu rezultata pretraživanja Azure pretraživanja](search-pagination-page-layout.md).


## <a name="hit-highlighting"></a>Kliknite isticanje
U odjeljku pretraživanje Azure naglašavanje točno dio rezultata pretraživanja koje odgovaraju upit za pretraživanje postala je lako pomoću na `highlight`, `highlightPreTag`, i `highlightPostTag` parametara. Polja koja _mogu pretraživati_ možete odrediti treba imati usklađena tekst ističu te određivanje vraća točan niz oznake da biste dodali na početak i završetak usklađena teksta Azure pretraživanje.
