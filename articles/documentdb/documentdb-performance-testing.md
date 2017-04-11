<properties 
    pageTitle="DocumentDB promjenom veličine i performanse testiranje | Microsoft Azure" 
    description="Saznajte kako izvesti promjenom veličine i performanse testiranjem Azure DocumentDB"
    keywords="Testiranje performansi"
    services="documentdb" 
    authors="arramac" 
    manager="jhubbard" 
    editor="" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/27/2016" 
    ms.author="arramac"/>

# <a name="performance-and-scale-testing-with-azure-documentdb"></a>Performanse i skaliranje testiranjem Azure DocumentDB
Performanse i testiranje skaliranje je ključa korak u razvoju aplikacija. Za mnoge aplikacije sloju baze podataka sadrži značajan utjecaj na ukupne performanse i skalabilnost i stoga ključna komponenta performanse testiranja. [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) je purpose-built elastic skala i predvidljivi performanse i stoga stane na odlično za aplikacije koje su potrebne sloj visokih performansi baze podataka. 

Ovaj je članak referenca za razvojne inženjere implementacijom performanse test paketi za svoje DocumentDB radnih opterećenja ili procjene DocumentDB aplikacija visokih performansi scenarijima za. Usredotočuje se na prvenstveno Izolirani performanse testiranje baze podataka, ali sadrži i najbolje prakse za proizvodnju aplikacije.

Kad pročitate članak će se moći odgovaraju na sljedeća pitanja:   

- Gdje pronaći ogledne .NET klijentska aplikacija za testiranje performanse Azure DocumentDB? 
- Kako postići propusnost visoke razine s Azure DocumentDB s Moja klijentska aplikacija?

Da biste započeli s kodom, preuzmite projekta s [DocumentDB performanse testiranje uzorka](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark). 

> [AZURE.NOTE] Cilj ove aplikacije jest da bismo pokazali najbolje prakse za izdvajanje bolje performanse iz DocumentDB s malim brojem klijentskim računalima. To je izvršena da bismo pokazali Vršna kapacitet servis, koji se mogu mijenjati veličinu limitlessly.

Ako tražite klijentsko konfiguracija mogućnosti da biste poboljšali performanse DocumentDB, pročitajte članak [DocumentDB performanse savjeti](documentdb-performance-tips.md).

## <a name="run-the-performance-testing-application"></a>Pokretanje performanse testiranje aplikacije
Najbrži način za početak rada je Kompiliranje i pokretanje uzorka .NET ispod, kao što je opisano u koracima u nastavku. Pregledajte izvornog koda i implementirati slične konfiguracije na vlastitu klijentske aplikacije.

**Korak 1:** Preuzmite projekta iz [Uzorka za testiranje DocumentDB performanse](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark)ili fork Github spremište.

**Korak 2:** Promijenite postavke za EndpointUrl, AuthorizationKey, CollectionThroughput i DocumentTemplate (neobavezno) u App.config.

> [AZURE.NOTE] Prije dodjele resursa zbirki s visoke propusnost, pogledajte [Cijene stranice](https://azure.microsoft.com/pricing/details/documentdb/) za procjenu troškove po zbirci. DocumentDB računi za pohranu i propusnost neovisno na svaki sat osnova, pa možete spremiti troškove brisanjem ili smanjiti propusnost zbirke DocumentDB nakon testiranja.

**Korak 3:** Kompiliranje i pokretanje aplikacija konzole iz naredbenog retka. Trebali biste vidjeti izlaz kao što je sljedeća:

    Summary:
    ---------------------------------------------------------------------
    Endpoint: https://docdb-scale-demo.documents.azure.com:443/
    Collection : db.testdata at 50000 request units per second
    Document Template*: Player.json
    Degree of parallelism*: 500
    ---------------------------------------------------------------------

    DocumentDBBenchmark starting...
    Creating database db
    Creating collection testdata
    Creating metric collection metrics
    Retrying after sleeping for 00:03:34.1720000
    Starting Inserts with 500 tasks
    Inserted 661 docs @ 656 writes/s, 6860 RU/s (18B max monthly 1KB reads)
    Inserted 6505 docs @ 2668 writes/s, 27962 RU/s (72B max monthly 1KB reads)
    Inserted 11756 docs @ 3240 writes/s, 33957 RU/s (88B max monthly 1KB reads)
    Inserted 17076 docs @ 3590 writes/s, 37627 RU/s (98B max monthly 1KB reads)
    Inserted 22106 docs @ 3748 writes/s, 39281 RU/s (102B max monthly 1KB reads)
    Inserted 28430 docs @ 3902 writes/s, 40897 RU/s (106B max monthly 1KB reads)
    Inserted 33492 docs @ 3928 writes/s, 41168 RU/s (107B max monthly 1KB reads)
    Inserted 38392 docs @ 3963 writes/s, 41528 RU/s (108B max monthly 1KB reads)
    Inserted 43371 docs @ 4012 writes/s, 42051 RU/s (109B max monthly 1KB reads)
    Inserted 48477 docs @ 4035 writes/s, 42282 RU/s (110B max monthly 1KB reads)
    Inserted 53845 docs @ 4088 writes/s, 42845 RU/s (111B max monthly 1KB reads)
    Inserted 59267 docs @ 4138 writes/s, 43364 RU/s (112B max monthly 1KB reads)
    Inserted 64703 docs @ 4197 writes/s, 43981 RU/s (114B max monthly 1KB reads)
    Inserted 70428 docs @ 4216 writes/s, 44181 RU/s (115B max monthly 1KB reads)
    Inserted 75868 docs @ 4247 writes/s, 44505 RU/s (115B max monthly 1KB reads)
    Inserted 81571 docs @ 4280 writes/s, 44852 RU/s (116B max monthly 1KB reads)
    Inserted 86271 docs @ 4273 writes/s, 44783 RU/s (116B max monthly 1KB reads)
    Inserted 91993 docs @ 4299 writes/s, 45056 RU/s (117B max monthly 1KB reads)
    Inserted 97469 docs @ 4292 writes/s, 44984 RU/s (117B max monthly 1KB reads)
    Inserted 99736 docs @ 4192 writes/s, 43930 RU/s (114B max monthly 1KB reads)
    Inserted 99997 docs @ 4013 writes/s, 42051 RU/s (109B max monthly 1KB reads)
    Inserted 100000 docs @ 3846 writes/s, 40304 RU/s (104B max monthly 1KB reads)

    Summary:
    ---------------------------------------------------------------------
    Inserted 100000 docs @ 3834 writes/s, 40180 RU/s (104B max monthly 1KB reads)
    ---------------------------------------------------------------------
    DocumentDBBenchmark completed successfully.


**Koraku 4 (Ako je potrebno):** Propusnost prijavljenih (Pravi na s) iz alata mora biti isto ili veće od dodijeljenu propusnost zbirke. Ako nije, povećati DegreeOfParallelism u malim pomacima može vam pomoći dosegne ograničenje. Ako plateaus propusnost iz aplikacije za klijenta, pokretanje više instanci aplikacije na istom ili drugom strojeva pomoći će vam dosegne dodijeljenu ograničenje preko različitih instance. Ako vam je potrebna pomoć za taj korak, napišite poruku e-pošte da biste askdocdb@microsoft.com ili ispunite zahtjev za podršku možete.

Nakon što dodate aplikaciju pokrenut, možete ga isprobati različitim [Indeksiranje pravila](documentdb-indexing-policies.md) i [dosljednost razine](documentdb-consistency-levels.md) da biste shvatili njihov utjecaj na propusnost i Latencija. Pregledajte izvornog koda i implementirati slične konfiguracija vlastite test paketi ili programi radnog.

## <a name="next-steps"></a>Daljnji koraci
U ovom se članku smo gledali kako možete izvršiti performanse i skaliranje testiranjem DocumentDB pomoću aplikacije konzole za .NET. Potražite putem veza u nastavku dodatne informacije o radu s DocumentDB.

* [DocumentDB performanse testiranja uzorka](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark)
* [Mogućnosti konfiguracije klijenta radi poboljšanja performansi DocumentDB](documentdb-performance-tips.md)
* [Poslužiteljsko particija u DocumentDB](documentdb-partition-data.md)
* [DocumentDB zbirke i performanse razine](documentdb-performance-levels.md)
* [DocumentDB .NET SDK dokumentaciji na MSDN-u](https://msdn.microsoft.com/library/azure/dn948556.aspx)
* [Uzorci DocumentDB .NET](https://github.com/Azure/azure-documentdb-net)
* [Blog DocumentDB na performanse Savjeti](https://azure.microsoft.com/blog/2015/01/20/performance-tips-for-azure-documentdb-part-1-2/)
