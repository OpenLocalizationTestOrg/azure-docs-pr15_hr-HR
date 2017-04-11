<properties
    pageTitle="Upute za korištenje spremišta tablica (C++) | Microsoft Azure"
    description="Pohranite strukturiranih podataka u oblak pomoću tablice Azure prostor za pohranu, NoSQL izvor podataka."
    services="storage"
    documentationCenter=".net"
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

# <a name="how-to-use-table-storage-from-c"></a>Upute za korištenje spremišta tablica iz C++

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Pregled  
Ovaj vodič vidjet ćete kako izvršiti uobičajeni scenariji pomoću servisa za pohranu Azure tablice. Primjere zapisuju u C++ i koristite [Azure prostora za pohranu klijenta biblioteku C++](https://github.com/Azure/azure-storage-cpp/blob/master/README.md). Scenariji u kojima je moguće uvrstiti **Stvaranje i brisanje tablice** i **radu s entiteti tablice**.

>[AZURE.NOTE] Ovaj vodič pronalaze klijentska biblioteka za pohranu Azure C++ verziju 1.0.0 i iznad. Preporučena verzija je biblioteka za pohranu klijenta 2.2.0, koji je dostupan putem [NuGet](http://www.nuget.org/packages/wastorage) ili [GitHub](https://github.com/Azure/azure-storage-cpp/).

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]
[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]


## <a name="create-a-c-application"></a>Stvaranje aplikacije C++  
U ovom vodiču će se koristiti za pohranu značajke koje se mogu se izvoditi unutar C++ aplikacije. Da biste to učinili, morat ćete instalirati biblioteku Azure prostora za pohranu klijenta za C++ i stvaranje računa Azure prostora za pohranu u pretplatu za Azure.  

Da biste instalirali biblioteku Azure prostora za pohranu klijenta za C++, možete koristiti na sljedeće načine:

-   **Linux:** Slijedite upute navedene na stranici [Azure prostora za pohranu klijenta biblioteke za C++ README](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) .  
-   **Windows:** U Visual Studio, kliknite **Alati > Upravitelj paketa NuGet > Upravitelj paketa konzole**. Na [konzoli upravitelja NuGet paketa](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) upišite sljedeću naredbu i pritisnite Enter.  

        Install-Package wastorage

## <a name="configure-your-application-to-access-table-storage"></a>Konfiguriranje aplikacija za pristup spremište tablica  
Dodajte obuhvaćaju sljedeće naredbe na vrh C++ datoteku koju želite koristiti Azure prostora za pohranu API-ji za pristup tablice:  

    #include "was/storage_account.h"
    #include "was/table.h"

## <a name="set-up-an-azure-storage-connection-string"></a>Postavljanje niz za povezivanje programa Azure prostora za pohranu  
Klijent za Azure prostora za pohranu koristi niza za povezivanje za pohranu za pohranu krajnje točke i vjerodajnice za pristup servisa za upravljanje podacima. Kada se pokrene klijentska aplikacija, morate unijeti niz za povezivanje za pohranu u sljedećem obliku. Koristite naziv računa za pohranu i tipkovni prečac za pohranu za račun za pohranu koji je naveden u [Azure Portal](https://portal.azure.com) za *AccountName* i *AccountKey* vrijednosti. Informacije o računima za pohranu i pristupnih tipki, potražite u članku [o Azure prostora za pohranu računa](storage-create-storage-account.md). Ovaj primjer pokazuje kako deklarirati statične polja držite niz za povezivanje:  

    // Define the connection string with your values.
    const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));

Da biste testirali vaše aplikacije u lokalnom računalu utemeljen na sustavu Windows, možete koristiti Azure [emulator prostora za pohranu](storage-use-emulator.md) koje se instaliraju pomoću [Azure SDK](https://azure.microsoft.com/downloads/). Prostor za pohranu emulator je utility koji simulira blobova platforme Azure, reda čekanja i tablice usluge dostupne na vašem računalu lokalne razvoj. Sljedeći primjer prikazuje način deklarirati statične polja držite niz za povezivanje za svoje lokalno spremište emulator:  

    // Define the connection string with Azure storage emulator.
    const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  

Da biste započeli emulator Azure prostora za pohranu, kliknite gumb **Start** ili pritisnite tipku s logotipom sustava Windows. Počnite pisati **Emulator Azure prostora za pohranu**, a zatim **Microsoft Azure prostora za pohranu Emulator** s popisa aplikacija sustava.  

Sljedeća primjera pretpostavlja da koristite jedan od ta dva načina za dohvaćanje niza veze za pohranu.  

## <a name="retrieve-your-connection-string"></a>Dohvaćanje niza za povezivanje  
Klase **cloud_storage_account** možete koristiti za predstavljanje prostora za pohranu podataka o računu. Za dohvaćanje podataka o računu za pohranu iz niza za povezivanje za pohranu, možete koristiti metodu rastavljanju.

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

Nakon toga se referenca **cloud_table_client** klasa, kao što je omogućuje vam dohvaćanje objekata reference za tablice i entiteti pohranjeni u servis za pohranu tablice. Sljedeći kod stvara objekt **cloud_table_client** pomoću objekta račun za pohranu smo dohvate iznad:  

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

## <a name="create-a-table"></a>Stvaranje tablice
**Cloud_table_client** objekt omogućuje vam dohvaćanje objekata reference za tablice i entiteti. Sljedeći kod stvara **cloud_table_client** objekt, a koji se koristi za stvaranje nove tablice.

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);  

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Retrieve a reference to a table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create the table if it doesn't exist.
    table.create_if_not_exists();  

## <a name="add-an-entity-to-a-table"></a>Dodavanje entitet u tablicu
Da biste tablicu dodali entitet, stvorite novi objekt **table_entity** i prenesite **table_operation::insert_entity**. Sljedeći kod koristi ime klijenta kao ključ retka i prezime kao ključ particije. Zajednički je entitet particija i redak ključ identificirati samo entitet u tablici. Entiteti s istim ključem particija se možete mu brže od onih s tipkama različite particije, ali pomoću tipki raznih particija omogućuje veću skalabilnost paralelne operacije. Dodatne informacije potražite u članku [Microsoft Azure prostora za pohranu performanse i skalabilnost kontrolnog popisa](storage-performance-checklist.md).

Sljedeći kod stvara novu instancu **table_entity** neke korisničkih podataka za pohranu. Kod sljedeće poziva **table_operation::insert_entity** da biste stvorili **table_operation** objekt da biste umetnuli entitet u tablicu, a novi entitet tablice povezuje s njim. Na kraju, kod poziva metoda **cloud_table** objekta. I novi **table_operation** šalje zahtjeva za uslugu tablice da biste umetnuli novi entitet kupca u tablici "osobe".  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Retrieve a reference to a table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create the table if it doesn't exist.
    table.create_if_not_exists();

    // Create a new customer entity.
    azure::storage::table_entity customer1(U("Harp"), U("Walter"));

    azure::storage::table_entity::properties_type& properties = customer1.properties();
    properties.reserve(2);
    properties[U("Email")] = azure::storage::entity_property(U("Walter@contoso.com"));

    properties[U("Phone")] = azure::storage::entity_property(U("425-555-0101"));

    // Create the table operation that inserts the customer entity.
    azure::storage::table_operation insert_operation = azure::storage::table_operation::insert_entity(customer1);

    // Execute the insert operation.
    azure::storage::table_result insert_result = table.execute(insert_operation);

## <a name="insert-a-batch-of-entities"></a>Umetanje skupine entiteti
Skupine entiteti na servis za tablicu možete umetnuti u jedan postupak pisanja. Sljedeći kod stvara **table_batch_operation** objekt, a zatim zbrojiti tri Umetanje operacije da biste ga. Svaka operacija Umetanje je dodao stvaranja novog objekta entitet, postavljanje vrijednosti i zatim pozivanje metode za umetanje na **table_batch_operation** objektu želite pridružiti entitet nova operacija za umetanje. Nakon toga **cloud_table.execute** zove dopušta izvršavanje operacija.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Define a batch operation.
    azure::storage::table_batch_operation batch_operation;

    // Create a customer entity and add it to the table.
    azure::storage::table_entity customer1(U("Smith"), U("Jeff"));

    azure::storage::table_entity::properties_type& properties1 = customer1.properties();
    properties1.reserve(2);
    properties1[U("Email")] = azure::storage::entity_property(U("Jeff@contoso.com"));
    properties1[U("Phone")] = azure::storage::entity_property(U("425-555-0104"));

    // Create another customer entity and add it to the table.
    azure::storage::table_entity customer2(U("Smith"), U("Ben"));

    azure::storage::table_entity::properties_type& properties2 = customer2.properties();
    properties2.reserve(2);
    properties2[U("Email")] = azure::storage::entity_property(U("Ben@contoso.com"));
    properties2[U("Phone")] = azure::storage::entity_property(U("425-555-0102"));

    // Create a third customer entity to add to the table.
    azure::storage::table_entity customer3(U("Smith"), U("Denise"));

    azure::storage::table_entity::properties_type& properties3 = customer3.properties();
    properties3.reserve(2);
    properties3[U("Email")] = azure::storage::entity_property(U("Denise@contoso.com"));
    properties3[U("Phone")] = azure::storage::entity_property(U("425-555-0103"));

    // Add customer entities to the batch insert operation.
    batch_operation.insert_or_replace_entity(customer1);
    batch_operation.insert_or_replace_entity(customer2);
    batch_operation.insert_or_replace_entity(customer3);

    // Execute the batch operation.
    std::vector<azure::storage::table_result> results = table.execute_batch(batch_operation);

Neke stvari obratiti pažnju na obradu postupci:  

-   Do 100 Umetanje, brisanje, pisma, Zamijeni, umetanje ili pisma i umetanje ili Zamijeni operacija možete izvesti u bilo koju kombinaciju jedne grupe.  
-   Skupne operacije može imati operacije Vrati ako je operaciju samo u grupu.  
-   Svi entiteti u operaciji jedne grupe moraju imati isti particija ključa.  
-   Skupne operacije ograničeno je na 4 MB podataka tereta.  

## <a name="retrieve-all-entities-in-a-partition"></a>Dohvaćanje svi entiteti u particije
Da biste upita u tablicu za sve entiteti u particije, koristite **table_query** objekt. Sljedeći primjer koda navodi filtar za entiteti gdje je "Kopač" tipku particije. U ovom se primjeru ispisuje polja svaki entitet u rezultatima upita na konzolu.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Construct the query operation for all customer entities where PartitionKey="Smith".
    azure::storage::table_query query;

    query.set_filter_string(azure::storage::table_query::generate_filter_condition(U("PartitionKey"), azure::storage::query_comparison_operator::equal, U("Smith")));

    // Execute the query.
    azure::storage::table_query_iterator it = table.execute_query(query);

    // Print the fields for each customer.
    azure::storage::table_query_iterator end_of_results;
    for (; it != end_of_results; ++it)
    {
        const azure::storage::table_entity::properties_type& properties = it->properties();

        std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key()
            << U(", Property1: ") << properties.at(U("Email")).string_value()
            << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;
    }  

Upit u sljedećem primjeru objedinjuje sve entiteti koji zadovoljavaju kriterije filtriranja. Ako imate velike tablice, a potrebno da biste preuzeli entiteti tablice često, preporučujemo da spremate podatke u BLOB-ova Azure prostora za pohranu umjesto toga.

## <a name="retrieve-a-range-of-entities-in-a-partition"></a>Dohvaćanje raspon entiteti u particije
Ako ne želite poslati upit svi entiteti particije, možete odrediti raspon kombiniranjem particija filtar za ključne s filtrom ključa retka. Sljedeći primjer koda koristi dva filtra da biste dobili entiteti sve particije "Kovač" gdje tipku retka (ime) starijima od E abecede počinje slovom i ispisali rezultate upita.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create the table query.
    azure::storage::table_query query;

    query.set_filter_string(azure::storage::table_query::combine_filter_conditions(
        azure::storage::table_query::generate_filter_condition(U("PartitionKey"),
        azure::storage::query_comparison_operator::equal, U("Smith")),
        azure::storage::query_logical_operator::op_and,
        azure::storage::table_query::generate_filter_condition(U("RowKey"), azure::storage::query_comparison_operator::less_than, U("E"))));

    // Execute the query.
    azure::storage::table_query_iterator it = table.execute_query(query);

    // Loop through the results, displaying information about the entity.
    azure::storage::table_query_iterator end_of_results;
    for (; it != end_of_results; ++it)
    {
        const azure::storage::table_entity::properties_type& properties = it->properties();

        std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key()
            << U(", Property1: ") << properties.at(U("Email")).string_value()
            << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;
    }  

## <a name="retrieve-a-single-entity"></a>Dohvaćanje jedne entitet
Upit za dohvaćanje jedne, specifične entitet možete napisati. Sljedeći kod koristi **table_operation::retrieve_entity** da biste odredili kupca "Jeff kovač". Ovu metodu vraća samo jedan entitet, umjesto zbirka i vraćena vrijednost je u **table_result**. Određivanje particija i redak tipke u upitu je najbrži način za dohvaćanje jedan entitet iz tablice servisa.  

    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Retrieve the entity with partition key of "Smith" and row key of "Jeff".
    azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
    azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

    // Output the entity.
    azure::storage::table_entity entity = retrieve_result.entity();
    const azure::storage::table_entity::properties_type& properties = entity.properties();

    std::wcout << U("PartitionKey: ") << entity.partition_key() << U(", RowKey: ") << entity.row_key()
        << U(", Property1: ") << properties.at(U("Email")).string_value()
        << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;

## <a name="replace-an-entity"></a>Zamjena entitet
Da biste zamijenili entitet, dohvatiti iz tablice servisa, izmjena objekta entitet, a zatim spremite promjene natrag na servis za tablice. Sljedeći kod mijenja postojeći klijent telefonski broj i e-pošte adresa. Umjesto pozivanje **table_operation::insert_entity**kod koristi **table_operation::replace_entity**. Zbog toga entitet potpuno zamijeniti na poslužitelju, osim ako entitet na poslužitelju je promijenjen nakon što ste ih dohvatili, u tom slučaju neće uspjeti postupak. To je da biste spriječili sa slučajno prebrisivanje promjena između dohvaćanja i ažuriranje komponenta za druge aplikacije. Proper rukovanje to je ponovno dohvatiti entitet, unesite promjene (Ako je još uvijek valjan) i zatim izvedite druge **table_operation::replace_entity** operacije. U sljedećem odjeljku vidjet ćete kako nadjačati tu funkciju.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Replace an entity.
    azure::storage::table_entity entity_to_replace(U("Smith"), U("Jeff"));
    azure::storage::table_entity::properties_type& properties_to_replace = entity_to_replace.properties();
    properties_to_replace.reserve(2);

    // Specify a new phone number.
    properties_to_replace[U("Phone")] = azure::storage::entity_property(U("425-555-0106"));

    // Specify a new email address.
    properties_to_replace[U("Email")] = azure::storage::entity_property(U("JeffS@contoso.com"));

    // Create an operation to replace the entity.
    azure::storage::table_operation replace_operation = azure::storage::table_operation::replace_entity(entity_to_replace);

    // Submit the operation to the Table service.
    azure::storage::table_result replace_result = table.execute(replace_operation);

## <a name="insert-or-replace-an-entity"></a>Umetanje ili-zamijene entitet
Operacija **table_operation::replace_entity** neće uspjeti ako entitet promijenjen je nakon što ste ih dohvatili s poslužitelja. Osim toga, morate dohvatiti entitet s poslužitelja najprije redoslijedom za **table_operation::replace_entity** uspio. Ponekad, no ne znate ako postoji entitet na poslužitelju i trenutne vrijednosti pohranjene u njoj se nevažnih – ažuriranje trebali biste ih sve prebrisati. Da biste to postigli, koristit ćete **table_operation::insert_or_replace_entity** postupak. Ovaj postupak umeće entitet, ako ne postoji ili zamjenjuje ako je tako, bez obzira na to kad je izvršena zadnjeg ažuriranja. U sljedećem primjeru kod entitet klijenta Jeff Novak i dalje se dohvate, ali se zatim sprema na poslužitelj putem **table_operation::insert_or_replace_entity**. Prebrisat će se sve ažuriranjima entitet između dohvaćanja i postupak ažuriranja.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Insert-or-replace an entity.
    azure::storage::table_entity entity_to_insert_or_replace(U("Smith"), U("Jeff"));
    azure::storage::table_entity::properties_type& properties_to_insert_or_replace = entity_to_insert_or_replace.properties();

    properties_to_insert_or_replace.reserve(2);

    // Specify a phone number.
    properties_to_insert_or_replace[U("Phone")] = azure::storage::entity_property(U("425-555-0107"));

    // Specify an email address.
    properties_to_insert_or_replace[U("Email")] = azure::storage::entity_property(U("Jeffsm@contoso.com"));

    // Create an operation to insert-or-replace the entity.
    azure::storage::table_operation insert_or_replace_operation = azure::storage::table_operation::insert_or_replace_entity(entity_to_insert_or_replace);

    // Submit the operation to the Table service.
    azure::storage::table_result insert_or_replace_result = table.execute(insert_or_replace_operation);

## <a name="query-a-subset-of-entity-properties"></a>Podskup entitet svojstva za upite  
Upit u tablicu možete dohvatiti samo nekoliko svojstva iz entitet. Upit u sljedeći kod koristi metodu **table_query::set_select_columns** da biste vratili samo adrese e-pošte od entiteti u tablici.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Define the query, and select only the Email property.
    azure::storage::table_query query;
    std::vector<utility::string_t> columns;

    columns.push_back(U("Email"));
    query.set_select_columns(columns);

    // Execute the query.
    azure::storage::table_query_iterator it = table.execute_query(query);

    // Display the results.
    azure::storage::table_query_iterator end_of_results;
    for (; it != end_of_results; ++it)
    {
        std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key();

        const azure::storage::table_entity::properties_type& properties = it->properties();
        for (auto prop_it = properties.begin(); prop_it != properties.end(); ++prop_it)
        {
            std::wcout << ", " << prop_it->first << ": " << prop_it->second.str();
        }

        std::wcout << std::endl;
    }

>[AZURE.NOTE] Slanje upita nekoliko svojstva iz entitet je učinkovitija operacija od dohvaćanje sva svojstva.

## <a name="delete-an-entity"></a>Brisanje entitet
Jednostavno možete izbrisati entitet nakon što ga dohvatili. Nakon što se dohvate entitet, nazovite **table_operation::delete_entity** entitet da biste izbrisali. Zatim pozovite metodu **cloud_table.execute** . Sljedeći kod dohvaća i briše entitet s particije ključ od "Kopač" i ključ za redak "Jeff".  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create an operation to retrieve the entity with partition key of "Smith" and row key of "Jeff".
    azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
    azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

    // Create an operation to delete the entity.
    azure::storage::table_operation delete_operation = azure::storage::table_operation::delete_entity(retrieve_result.entity());

    // Submit the delete operation to the Table service.
    azure::storage::table_result delete_result = table.execute(delete_operation);  

## <a name="delete-a-table"></a>Brisanje tablice
Sljedeći primjer koda na kraju, brisanje tablice s računa za pohranu. Tablica koja je izbrisana neće biti dostupno da biste se ponovno stvara za neko vrijeme nakon brisanja.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create an operation to retrieve the entity with partition key of "Smith" and row key of "Jeff".
    azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
    azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

    // Create an operation to delete the entity.
    azure::storage::table_operation delete_operation = azure::storage::table_operation::delete_entity(retrieve_result.entity());

    // Submit the delete operation to the Table service.
    azure::storage::table_result delete_result = table.execute(delete_operation);

## <a name="next-steps"></a>Daljnji koraci
Sad kad ste naučili osnove spremište tablica, slijedite ove veze da biste saznali više o Azure pohranu:  

-   [Upute za korištenje spremišta blobova iz C++](storage-c-plus-plus-how-to-use-blobs.md)
-   [Kako koristiti reda čekanja za pohranu iz C++](storage-c-plus-plus-how-to-use-queues.md)
-   [Popis resursa Azure prostora za pohranu u C++](storage-c-plus-plus-enumeration.md)
-   [Prostor za pohranu klijenta biblioteka za C++ reference](http://azure.github.io/azure-storage-cpp)
-   [Azure dokumentaciju za pohranu](https://azure.microsoft.com/documentation/services/storage/)
