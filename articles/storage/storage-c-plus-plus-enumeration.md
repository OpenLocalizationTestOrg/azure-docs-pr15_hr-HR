<properties
    pageTitle="Popis resursa Azure prostora za pohranu s bibliotekom sustava Microsoft Azure prostora za pohranu klijenta za C++ | Microsoft Azure"
    description="Saznajte kako koristiti stavku API-ji u programu Microsoft Azure prostora za pohranu klijenta biblioteka za C++ numerirati spremnika, blob-ova, redove, tablice i entiteti."
    documentationCenter=".net"
    services="storage"
    authors="dineshmurthy"
    manager="jahogg"
    editor="tysonn"/>
<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="dineshm"/>

# <a name="list-azure-storage-resources-in-c"></a>Popis resursa Azure prostora za pohranu u C++

Unos operacije su vam ključne za mnoge razvoj scenariji s Azure prostora za pohranu. U ovom se članku opisuje kako učinkovito najčešće Nabrajanje objekata u spremište Azure pomoću stavku u biblioteci klijenta za pohranu sustava Microsoft Azure namijenjeno C++ API-ji.

>[AZURE.NOTE] Ovaj vodič pronalaze biblioteku Azure prostora za pohranu klijenta za verziju C++ 2.x koji je dostupan putem [NuGet](http://www.nuget.org/packages/wastorage) ili [GitHub](https://github.com/Azure/azure-storage-cpp).

Klijentska biblioteka za pohranu navedene različite načine na popis ili upit objekte Azure prostora za pohranu. U ovom se članku adrese sljedećim scenarijima:

-   Popis spremnika u račun
-   Popis blob-ova u spremniku ili virtualne blob direktorija
-   Popis redova putem računa
-   Popis tablica u račun
-   Entiteti upita u tablicu

Svaki od ovih metoda prikazan je pomoću različitih overloads različitim scenarijima.

## <a name="asynchronous-versus-synchronous"></a>Asinkronog nasuprot sinkronizirano

Budući da je u biblioteci za pohranu klijenta za C++ ugrađena pri vrhu [C++ OSTALE biblioteke](https://github.com/Microsoft/cpprestsdk), čini podržavamo asinkronog operacije pomoću [pplx::task](http://microsoft.github.io/cpprestsdk/classpplx_1_1task.html). Ako, na primjer:

    pplx::task<list_blob_item_segment> list_blobs_segmented_async(continuation_token& token) const;

Sinkrono operacije prelamanje odgovarajuće asinkronog postupci:

    list_blob_item_segment list_blobs_segmented(const continuation_token& token) const
    {
        return list_blobs_segmented_async(token).get();
    }

Ako radite s više usporedno izvršavanje zadataka programe ili servise, preporučujemo da koristite asinkrone API-ji izravno umjesto stvaranja niti da biste nazvali sinkronizaciju API, znatno utječe performansi.

## <a name="segmented-listing"></a>Segmentirani stavku

Skaliranje za pohranu u oblaku zahtijeva segmentiranog stavku. Ako, na primjer, može imati putem programa milijuna blob-ova u spremniku programa blobova platforme Azure ili putem programa milijarde entiteti u tablici programa Azure. To su ne theoretical brojeva, ali slučajeve korištenja stvarnih klijenata.

Stoga je nepraktično popis svih objekata u jedan odgovor. Umjesto toga možete popisa objekata pomoću broja stranica. Svaki od unos API-ji ima na *segmentirajući* preopterećenja.

Odgovor za unos segmentirani postupak obuhvaća:

-   <i>_segment</i>, koja sadrži skup pronađenih rezultata za jedan poziv API stavku.
-   *continuation_token*, koji se prenosi sljedeći poziv da bi se na sljedećoj stranici rezultata. Kada nema više rezultata da biste se vratili, token nastavka je null.

Ako, na primjer, uobičajeni poziv da biste dobili popis svih blob-ova u spremniku može izgledati kao sljedeće isječak koda. Kod je dostupan u našem [uzorke](https://github.com/Azure/azure-storage-cpp/blob/master/Microsoft.WindowsAzure.Storage/samples/BlobsGettingStarted/Application.cpp):

    // List blobs in the blob container
    azure::storage::continuation_token token;
    do
    {
        azure::storage::list_blob_item_segment segment = container.list_blobs_segmented(token);
        for (auto it = segment.results().cbegin(); it != segment.results().cend(); ++it)
    {
        if (it->is_blob())
        {
            process_blob(it->as_blob());
        }
        else
        {
            process_diretory(it->as_directory());
        }
    }

        token = segment.continuation_token();
    }
    while (!token.empty());

Imajte na umu da broj rezultata koji se vraća na stranici upravlja parametar *max_results* u preopterećenja svaki API-jem, primjerice:

    list_blob_item_segment list_blobs_segmented(const utility::string_t& prefix, bool use_flat_blob_listing,
        blob_listing_details::values includes, int max_results, const continuation_token& token,
        const blob_request_options& options, operation_context context)

Ako ne navedete parametar *max_results* , zadana vrijednost maksimalno do 5000 rezultata vraća se u jednu stranicu.

Imajte na umu da upit poslan spremište tablica Azure može vratiti nijedan zapis ili manje zapisa od vrijednosti parametra *max_results* koji ste naveli, čak i ako token nastavka nije prazno. Jedan od razloga možda nije li upit ne dovrši u pet sekundi. Dok god token nastavka nije prazno, prijeđite na upit, a kod treba pretpostavlja veličina segmenta rezultata.

Preporučena kodiranje uzorak većini scenarija je segmentirajući popis, koja omogućuje eksplicitnih tijek stavku ili slanje upita i kako odgovori svaki zahtjev za uslugu. Osobito za C++ programe ili servise, niže razine kontrole nad tijeku unos može pomoći kontrola memorije i performanse.

## <a name="greedy-listing"></a>Pohlepa stavku

Ranijim verzijama biblioteke za pohranu klijenta za C++ (verzija 0.5.0 pretpregled i starijim verzijama) uključeni koje nisu segmentirajući stavku API-ji za tablice i redove, kao u sljedećem primjeru:

    std::vector<cloud_table> list_tables(const utility::string_t& prefix) const;
    std::vector<table_entity> execute_query(const table_query& query) const;
    std::vector<cloud_queue> list_queues() const;

Načini su implementirana kao wrappers segmentirani API-ji. Za svaki odgovor segmentiranog stavku kod dodaje rezultate vektor i vraća rezultate kad skenirali cijelog spremnika.

Taj se način možda funkcioniraju kad je račun za pohranu ili tablica sadrži mali broj objekata. Međutim, s povećava broj objekata, memorije obavezno nije povećati bez ograničenja, jer sve rezultate ostaje u memoriji. Jedan unos operacije može potrajati vrlo dugo vrijeme tijekom kojeg pozivatelj imali nikakve informacije o tijeku.

Ove pohlepa stavku API-ji u SDK ne postoje u C#, Java ili JavaScript Node.js okruženju. Da biste izbjegli potencijalne probleme korištenja ove pohlepa API-ji, ne možemo ih uklonili u verziji 0.6.0 pretpregled.

Ako vaš se kodom poziva te pohlepa API-ji:

    std::vector<azure::storage::table_entity> entities = table.execute_query(query);
    for (auto it = entities.cbegin(); it != entities.cend(); ++it)
    {
        process_entity(*it);
    }

Zatim izmijenite kod da biste koristili segmentiranog unos API-ji:

    azure::storage::continuation_token token;
    do
    {
        azure::storage::table_query_segment segment = table.execute_query_segmented(query, token);
        for (auto it = segment.results().cbegin(); it != segment.results().cend(); ++it)
        {
            process_entity(*it);
        }

        token = segment.continuation_token();
    } while (!token.empty());

Navođenjem parametar *max_results* segmenta saldo između brojeva zahtjeve i memorije koristi da bi odgovarao pitanja vezana uz performanse aplikacije.

Osim toga, ako koristite segmentiranog unos API-ji, ali spremanje podataka u lokalnom zbirke u "pohlepa" stil, također preporučujemo da refactor kod za rukovanje pohrana podataka u lokalnom zbirke pažljivo na razini.

## <a name="lazy-listing"></a>Drži stavku

Iako pohlepa stavku potenciju potencijalne probleme, je praktičan ako u spremniku nema previše objekte.

Ako koristite i C# ili Oracle Java SDK-ovi, morate biti upoznati s brojiva model programiranja, koji nudi drži-stila popis, gdje podatke na određenim pomak je samo dohvaćanja ako je potrebno. U C++ iterator utemeljen predložak omogućuje sličan pristup.

Uobičajeni drži unos API-JA, a pomoću **list_blobs** kao primjer izgleda ovako:

    list_blob_item_iterator list_blobs() const;

Isječak uobičajeni koda koji koristi uzorak drži unos može izgledati ovako:

    // List blobs in the blob container
    azure::storage::list_blob_item_iterator end_of_results;
    for (auto it = container.list_blobs(); it != end_of_results; ++it)
    {
        if (it->is_blob())
        {
            process_blob(it->as_blob());
        }
        else
        {
            process_directory(it->as_directory());
        }
    }

Imajte na umu da drži unos dostupna je samo u sinkronizirano načinu.

U usporedbi s pohlepa stavku drži unos dohvaćanja podataka samo kada je to potrebno. U odjeljku naslovnice, ona dohvaćanja podataka iz spremišta Azure samo kada se sljedeći iterator premješta u sljedeći segmenta. Stoga memorije koristi se kontrolira bounded veličina, a postupak je brz.

Drži unos API-ji sadrži klijentska biblioteka za pohranu za C++ u verziji 2.2.0.

## <a name="conclusion"></a>Zaključak

U ovom se članku smo navedene različite overloads za popis API-ji za različite objekte u biblioteci za pohranu klijenta za C++. Sažetak:

-   API-ji asinkrone preporučuje se u odjeljku više scenariji usporedno izvršavanje zadataka.
-   Segmentirani unos preporučuje se u većini scenarija.
-   Drži stavku prikazuje se u biblioteci kao praktičan omot u sinkronizirano scenarijima.
-   Pohlepa unos ne preporučuje se i uklonjena iz biblioteke.

##<a name="next-steps"></a>Daljnji koraci

Dodatne informacije o Azure i prostor za pohranu klijenta biblioteka za C++ potražite u sljedećim člancima.

-   [Upute za korištenje spremišta blobova iz C++](storage-c-plus-plus-how-to-use-blobs.md)
-   [Upute za korištenje spremišta tablica iz C++](storage-c-plus-plus-how-to-use-tables.md)
-   [Kako koristiti reda čekanja za pohranu iz C++](storage-c-plus-plus-how-to-use-queues.md)
-   [Dokumentacije C++ API Azure biblioteka za pohranu klijenta.](http://azure.github.io/azure-storage-cpp/)
-   [Blog tima za Azure prostora za pohranu](http://blogs.msdn.com/b/windowsazurestorage/)
-   [Dokumentacija Azure prostora za pohranu](https://azure.microsoft.com/documentation/services/storage/)
