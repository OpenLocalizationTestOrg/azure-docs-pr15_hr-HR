<properties
    pageTitle="Prijenos podataka u pretraživanju Azure | Microsoft Azure | Servis za pretraživanje glavnom računalu oblaka"
    description="Saznajte kako prenijeti podatke indeksa u pretraživanju Azure."
    services="search"
    documentationCenter=""
    authors="ashmaka"
    manager="jhubbard"
    editor=""
    tags=""/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="upload-data-to-azure-search"></a>Prijenos podataka Azure pretraživanja
> [AZURE.SELECTOR]
- [Pregled](search-what-is-data-import.md)
- [.NET](search-import-data-dotnet.md)
- [OSTALE](search-import-data-rest-api.md)


## <a name="data-upload-models-in-azure-search"></a>Prijenos podataka modelima u Azure pretraživanja
Dva su načina za popunjavanje indeksu pretraživanja Azure s podacima. Prva mogućnost ručno margina podataka u indeks pretraživanja Azure [REST API -JA](search-import-data-rest-api.md) ili [.NET SDK](search-import-data-dotnet.md). Druga mogućnost je indeksu pretraživanja Azure [pokažite podržanih izvora](search-indexer-overview.md) i pričekajte Azure pretraživanja automatski uvesti podatke u servis za pretraživanje.

Ovaj vodič samo pokrivaju upute o korištenju automatskih model prijenos podataka (što je podržano samo u [REST API -JA](search-import-data-rest-api.md) i [.NET SDK](search-import-data-dotnet.md)), ali i dalje možete saznati više o u istaknuti model.

### <a name="push-data-to-an-index"></a>Slanje podataka u indeks

Taj se način odnosi se na programski slanje podataka Azure pretraživanje tako da bi bio dostupan za pretraživanje. Za aplikacije koje se pojavljuju nisku Latencija preduvjeti (npr. Ako je potrebno pretraživanje operacije da biste se sinkroniziraju s bazama podataka za dinamičku zaliha), model automatske je vaša jedina mogućnosti.

Možete koristiti [REST API -JA](https://msdn.microsoft.com/library/azure/dn798930.aspx) ili [.NET SDK](search-import-data-dotnet.md) za slanje podataka u indeks. Trenutno ne postoji alat podrška za margina podataka putem portala sustava.

Taj se način je fleksibilnije od modela istaknuti jer dokumente možete prenijeti pojedinačno ili u grupama (do 1000 po grupe ili 16 MB, bez obzira ograničenje dolazi prvi). Automatske model omogućuje i prijenos dokumenata na Azure pretraživanje bez obzira na to gdje se nalazi vaše podatke.

### <a name="pull-data-into-an-index"></a>Povlačenje podataka u indeks

Model istaknuti pretražuje podržanih izvora i automatski prenosi podatke u pretraživanje Azure indeksa umjesto vas. Ako praćenja promjena i brisanje postojeće dokumente osim prepoznavati nove dokumente, indexers uklonit morati aktivno upravljanje podacima u indeks.

U odjeljku pretraživanje Azure tu mogućnost je implementirana putem *indexers*, trenutno dostupno za [spremište blobova (pretpregled)](search-howto-indexing-azure-blob-storage.md), [DocumentDB](http://aka.ms/documentdb-search-indexer) [baze podataka Azure SQL, i SQL Server na Azure VMs](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md).

Funkcija indeksiranje predstavljeni [Azure Portal](search-import-data-portal.md) kao i kao [REST API -JA](https://msdn.microsoft.com/library/azure/dn946891.aspx).
