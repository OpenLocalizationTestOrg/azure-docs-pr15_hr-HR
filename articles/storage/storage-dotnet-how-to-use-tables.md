<properties
    pageTitle="Početak rada sa spremištem tablica Azure pomoću .NET | Microsoft Azure"
    description="Pohranite strukturiranih podataka u oblak pomoću tablice Azure prostor za pohranu, NoSQL izvor podataka."
    services="storage"
    documentationCenter=".net"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="get-started-with-azure-table-storage-using-net"></a>Početak rada sa spremištem tablica Azure pomoću .NET

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Pregled

Azure spremište tablica je servis koji pohranjuje strukturiranih podataka NoSQL u oblaku. Spremište tablica je spremište ključ/atribut s schemaless dizajna. Budući da je spremište tablica schemaless, jednostavno je prilagodite podataka kao potrebama vaše evolve aplikacije. Pristup podacima je brz i učinkovit za sve vrste aplikacije. Spremište tablica obično je znatno manja trošak od tradicionalni SQL za slične količine podataka.

Spremište tablica možete koristiti za pohranu fleksibilna skupova podataka, kao što su korisničkih podataka za web-aplikacije, adresara, informacije o uređaju i neku drugu vrstu metapodataka koje je potrebno za uslugu. Bilo koji broj entiteti mogu pohraniti u tablici, a pohranu račun može sadržavati bilo koji broj tablice, do kapaciteta ograničenje prostora za pohranu računa.

### <a name="about-this-tutorial"></a>O ovog praktičnog vodiča

Pomoću ovog praktičnog vodiča pokazuje kako .NET kod za neke uobičajeni scenariji pomoću tablice Azure prostor za pohranu, uključujući stvaranje i brisanje tablice i umetanje, ažuriranje, brisanje i slanje upita za podatke tablice.

**Procijenjena vrijeme da biste dovršili:** 45 minuta

**Prerequisities:**

- [Microsoft Visual Studio](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx)
- [Azure prostora za pohranu klijenta biblioteka za .NET](https://www.nuget.org/packages/WindowsAzure.Storage/)
- [Azure Upravitelj konfiguracije za .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
- [Račun za Azure prostora za pohranu](storage-create-storage-account.md#create-a-storage-account)

[AZURE.INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

### <a name="more-samples"></a>Dodatne primjere

Dodatni Primjeri uporabe spremište tablica, potražite u članku [Početak rada sa spremištem tablica Azure u .NET](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/). Možete preuzeti aplikaciju za uzorak i ga pokrenuti ili potražite kod na GitHub.


[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### <a name="add-namespace-declarations"></a>Dodavanje prostora za naziv deklaracija

Dodajte sljedeće `using` naredbe na vrh na `program.cs` datoteke:

    using Microsoft.Azure; // Namespace for CloudConfigurationManager
    using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
    using Microsoft.WindowsAzure.Storage.Table; // Namespace for Table storage types

### <a name="parse-the-connection-string"></a>Raščlaniti niz za povezivanje

[AZURE.INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-the-table-service-client"></a>Stvaranje klijentskog servisa tablice

Klase **CloudTableClient** omogućuje vam dohvaćanje tablica i entiteti pohranjene u spremište tablica. Ovdje je jedan od načina da biste stvorili klijentskog programa servisa:

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

Sada ste spremni za pisanje koda koji se čita podatke iz i zapisuje podatke sa spremištem tablica.

## <a name="create-a-table"></a>Stvaranje tablice

Ovaj primjer pokazuje kako stvoriti tablice ako je već postoji:

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Retrieve a reference to the table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create the table if it doesn't exist.
    table.CreateIfNotExists();

## <a name="add-an-entity-to-a-table"></a>Dodavanje entitet u tablicu

Entiteti mapiranje C\# objekata pomoću prilagođenih predmete izvedene iz **TableEntity**. Da biste tablicu dodali entitet, stvorite predmete koji definira svojstva vaše entitet. Sljedeći kod definira programa klase entitet koji koristi ime klijenta kao ključ retka i prezime kao ključ particije. Zajednički je entitet particija i redak ključ identificirati samo entitet u tablici. Entiteti s istim ključem particija se možete mu brže od onih s tipkama različite particije, ali pomoću tipki raznih particija omogućuje veću skalabilnost paralelne operacije.  Za neko svojstvo koje će biti pohranjene na servisu tablice svojstvo mora biti javno svojstvo podržane vrste koju izlaže oba `get` i `set`.
Osim toga, vaš entitet vrstu *morate* izložiti parametar manje Graditelj.

    public class CustomerEntity : TableEntity
    {
        public CustomerEntity(string lastName, string firstName)
        {
            this.PartitionKey = lastName;
            this.RowKey = firstName;
        }

        public CustomerEntity() { }

        public string Email { get; set; }

        public string PhoneNumber { get; set; }
    }

Postupci tablice koje obuhvaćaju entiteti se izvode putem **CloudTable** objekta koji ste prethodno stvorili u odjeljku "Stvaranje tablice". Operacije za obavljanje predstavlja **TableOperation** objekt.  Sljedeći primjer koda prikazuje stvaranje **CloudTable** objekt, a zatim **CustomerEntity** objekt.  Da biste pripremili postupak, stvara se **TableOperation** objekt da biste umetnuli entitet klijenta u tablicu.  Na kraju se izvršava postupak tako da nazovete **CloudTable.Execute**.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";

    // Create the TableOperation object that inserts the customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);

    // Execute the insert operation.
    table.Execute(insertOperation);

## <a name="insert-a-batch-of-entities"></a>Umetanje skupine entiteti

Skupine entiteti možete umetnuti u tablicu u jedan operacija pisanja. Neke napomene za obradu operacije:

-  Na raspolaganju vam ažuriranja, briše i umetnuti u isti postupak jedne grupe.
-  Jedan skupne operacije može sadržavati do 100 entiteti.
-  Svi entiteti u operaciji jedne grupe moraju imati isti particija ključa.
-  Dok je moguće za izvođenje upita u obliku skupne operacije, mora biti samo operacije u grupu.

<!-- -->
Sljedeći primjer koda stvara dva objekte i dodaje svakom **TableBatchOperation** pomoću metode za **Umetanje** . Nakon toga **CloudTable.Execute** zove dopušta izvršavanje operacija.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create the batch operation.
    TableBatchOperation batchOperation = new TableBatchOperation();

    // Create a customer entity and add it to the table.
    CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
    customer1.Email = "Jeff@contoso.com";
    customer1.PhoneNumber = "425-555-0104";

    // Create another customer entity and add it to the table.
    CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
    customer2.Email = "Ben@contoso.com";
    customer2.PhoneNumber = "425-555-0102";

    // Add both customer entities to the batch insert operation.
    batchOperation.Insert(customer1);
    batchOperation.Insert(customer2);

    // Execute the batch operation.
    table.ExecuteBatch(batchOperation);

## <a name="retrieve-all-entities-in-a-partition"></a>Dohvaćanje svi entiteti u particija

Da biste upita u tablicu za sve entiteti u particije, koristite **TableQuery** objekt.
Sljedeći primjer koda navodi filtar za entiteti gdje je "Kopač" tipku particije. U ovom se primjeru ispisuje polja svaki entitet u rezultatima upita na konzolu.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Construct the query operation for all customer entities where PartitionKey="Smith".
    TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>().Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

    // Print the fields for each customer.
    foreach (CustomerEntity entity in table.ExecuteQuery(query))
    {
        Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
            entity.Email, entity.PhoneNumber);
    }

## <a name="retrieve-a-range-of-entities-in-a-partition"></a>Dohvaćanje raspon entiteti u particije

Ako ne želite poslati upit svi entiteti particije, možete odrediti raspon kombiniranjem particija filtar za ključne s filtrom ključa retka. Sljedeći primjer koda koristi dva filtra da biste dobili entiteti sve particije "Kovač" gdje tipku retka (ime) starijima od E abecede počinje slovom i ispisali rezultate upita.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create the table query.
    TableQuery<CustomerEntity> rangeQuery = new TableQuery<CustomerEntity>().Where(
        TableQuery.CombineFilters(
            TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"),
            TableOperators.And,
            TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.LessThan, "E")));

    // Loop through the results, displaying information about the entity.
    foreach (CustomerEntity entity in table.ExecuteQuery(rangeQuery))
    {
        Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
            entity.Email, entity.PhoneNumber);
    }

## <a name="retrieve-a-single-entity"></a>Dohvaćanje jedne entitet

Upit za dohvaćanje jedne, specifične entitet možete napisati. Sljedeći kod koristi **TableOperation** da biste odredili kupca "Ben kovač".
Ta metoda vraća samo jedan entitet umjesto zbirka i vraćene vrijednosti u **TableResult.Result** **CustomerEntity** objekt.
Određivanje particija i redak tipke u upitu je najbrži način za dohvaćanje jedan entitet iz tablice servisa.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the retrieve operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);

    // Print the phone number of the result.
    if (retrievedResult.Result != null)
       Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
    else
       Console.WriteLine("The phone number could not be retrieved.");

## <a name="replace-an-entity"></a>Zamjena entitet

Da biste ažurirali entitet, dohvatiti iz tablice servisa, izmjena objekta entitet, a zatim spremite promjene natrag na servis za tablice. Sljedeći kod mijenja postojeći klijent telefonski broj. Umjesto da nazovete podršku za **Umetanje**, kod koristi **Zamijeni**. Zbog toga entitet potpuno zamijenjene na poslužitelju, osim ako entitet na poslužitelju je promijenjen nakon što ste ih dohvatili, u tom slučaju neće uspjeti postupak.  To je da biste spriječili sa slučajno prebrisivanje promjena između dohvaćanja i ažuriranje komponenta za druge aplikacije.  Proper rukovanje to je ponovno dohvatiti entitet, unesite promjene (Ako je još uvijek valjan) i zatim izvedite druge operacije **zamijeniti** .  U sljedećem odjeljku vidjet ćete kako nadjačati tu funkciju.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity object.
    CustomerEntity updateEntity = (CustomerEntity)retrievedResult.Result;

    if (updateEntity != null)
    {
       // Change the phone number.
       updateEntity.PhoneNumber = "425-555-0105";

       // Create the Replace TableOperation.
       TableOperation updateOperation = TableOperation.Replace(updateEntity);

       // Execute the operation.
       table.Execute(updateOperation);

       Console.WriteLine("Entity updated.");
    }

    else
       Console.WriteLine("Entity could not be retrieved.");

## <a name="insert-or-replace-an-entity"></a>Umetanje ili-zamijene entitet

**Zamjena** operacije neće uspjeti ako entitet promijenjen je nakon što ste ih dohvatili s poslužitelja.  Osim toga, morate dohvatiti entitet s poslužitelja najprije redoslijedom za operaciju **Zamjena** uspio.
Ponekad, međutim, ne znate ako postoji entitet na poslužitelju i trenutne vrijednosti pohranjene u njoj se nevažnih. Ažuriranje prebrisivanje trebali biste ih sve.  Da biste to postigli, koristite postupak **InsertOrReplace** .  Ovaj postupak umeće entitet, ako ne postoji ili zamjenjuje ako je tako, bez obzira na to kad je izvršena zadnjeg ažuriranja.  U sljedećem primjeru kod entitet klijenta Ben Novak i dalje se dohvate, ali se zatim sprema na poslužitelj putem **InsertOrReplace**.  Prebrisat će se sve ažuriranjima entitet između operacije dohvaćanja i ažuriranja.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity object.
    CustomerEntity updateEntity = (CustomerEntity)retrievedResult.Result;

    if (updateEntity != null)
    {
       // Change the phone number.
       updateEntity.PhoneNumber = "425-555-1234";

       // Create the InsertOrReplace TableOperation.
       TableOperation insertOrReplaceOperation = TableOperation.InsertOrReplace(updateEntity);

       // Execute the operation.
       table.Execute(insertOrReplaceOperation);

       Console.WriteLine("Entity was updated.");
    }

    else
       Console.WriteLine("Entity could not be retrieved.");

## <a name="query-a-subset-of-entity-properties"></a>Podskup entitet svojstva za upite

Tablice upita možete dohvatiti samo nekoliko svojstva iz entitet umjesto sva svojstva entitet. Ovu tehniku naziva projekcije, smanjuje propusnost i poboljšati performanse upita, posebice za velike entiteti. Upit u sljedeći kod vraća samo adrese e-pošte od entiteti u tablici. To možete učiniti pomoću upita te **DynamicTableEntity** i **EntityResolver**. Možete se Saznajte više o projekciju na [Uvod Upsert i projekcije upita blog objaviti][]. Imajte na umu da projekcije nije podržana na emulator lokalno spremište tako kod koji se pokreće samo kada koristite račun za servis za tablice.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Define the query, and select only the Email property.
    TableQuery<DynamicTableEntity> projectionQuery = new TableQuery<DynamicTableEntity>().Select(new string[] { "Email" });

    // Define an entity resolver to work with the entity after retrieval.
    EntityResolver<string> resolver = (pk, rk, ts, props, etag) => props.ContainsKey("Email") ? props["Email"].StringValue : null;

    foreach (string projectedEmail in table.ExecuteQuery(projectionQuery, resolver, null, null))
    {
        Console.WriteLine(projectedEmail);
    }

## <a name="delete-an-entity"></a>Brisanje entitet

Jednostavno možete izbrisati entitet nakon što ste dohvatili, koristeći isti uzorak prikazano o ažuriranju entitet.  Sljedeći kod dohvaća i briše entitet klijenta.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a retrieve operation that expects a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity.
    CustomerEntity deleteEntity = (CustomerEntity)retrievedResult.Result;

    // Create the Delete TableOperation.
    if (deleteEntity != null)
    {
       TableOperation deleteOperation = TableOperation.Delete(deleteEntity);

       // Execute the operation.
       table.Execute(deleteOperation);

       Console.WriteLine("Entity deleted.");
    }

    else
       Console.WriteLine("Could not retrieve the entity.");

## <a name="delete-a-table"></a>Brisanje tablice

Sljedeći primjer koda na kraju, brisanje tablice s računa za pohranu. Tablica koja je izbrisana neće biti dostupno da biste se ponovno stvara za neko vrijeme nakon brisanja.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Delete the table it if exists.
    table.DeleteIfExists();

## <a name="retrieve-entities-in-pages-asynchronously"></a>Asinkrono dohvaćanje entiteti na stranicama

Ako čitate velikog broja entiteti, a želite postupak/Prikaži entiteti kao što su učitavaju umjesto čekanje ih sve da biste se vratili, entiteti možete dohvatiti pomoću segmentiranog upita. U ovom se primjeru objašnjava kako bi vratio rezultate na stranicama pomoću uzorak asinkrone Await tako da se izvršavanje je blokirana dok čekate za veliki skup rezultata da biste se vratili. Dodatne informacije o korištenju uzorak asinkrone Await u .NET, potražite u članku [asinkronog programiranja uz asinkrone i Await (C# i Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx).

    // Initialize a default TableQuery to retrieve all the entities in the table.
    TableQuery<CustomerEntity> tableQuery = new TableQuery<CustomerEntity>();

    // Initialize the continuation token to null to start from the beginning of the table.
    TableContinuationToken continuationToken = null;

    do
    {
        // Retrieve a segment (up to 1,000 entities).
        TableQuerySegment<CustomerEntity> tableQueryResult =
            await table.ExecuteQuerySegmentedAsync(tableQuery, continuationToken);

        // Assign the new continuation token to tell the service where to
        // continue on the next iteration (or null if it has reached the end).
        continuationToken = tableQueryResult.ContinuationToken;

        // Print the number of rows retrieved.
        Console.WriteLine("Rows retrieved {0}", tableQueryResult.Results.Count);

    // Loop until a null continuation token is received, indicating the end of the table.
    } while(continuationToken != null);

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste naučili osnove spremište tablica, slijedite ove veze dodatne informacije o složenije prostora za pohranu zadataka:

- U odjeljku Dodatne uzoraka tablice za pohranu u [Početak rada sa spremištem tablica Azure u .NET](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/)
- Prikaz tablice servisa referentnu dokumentaciju za sve pojedinosti o API-ji dostupne:
    - [Prostor za pohranu klijenta biblioteka za .NET reference](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
    - [Referenca REST API-JA](http://msdn.microsoft.com/library/azure/dd179355)
- Saznajte kako da biste pojednostavnili kod u pišete za rad sa spremištem Azure pomoću [Azure WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-get-started.md)
- Prikaz još vodilica značajku dodatne informacije o dodatne mogućnosti za spremanje podataka u Azure.
    - [Početak rada s Azure blobova pomoću .NET](storage-dotnet-how-to-use-blobs.md) pohranjujete nestrukturirane podatke.
    - [Povezivanje s bazom podataka SQL pomoću .NET (C#)](../sql-database/sql-database-develop-dotnet-simple.md) da biste pohranili relacijskih podataka.

  [Download and install the Azure SDK for .NET]: /develop/net/
  [Creating an Azure Project in Visual Studio]: http://msdn.microsoft.com/library/azure/ee405487.aspx

  [Blob5]: ./media/storage-dotnet-how-to-use-table-storage/blob5.png
  [Blob6]: ./media/storage-dotnet-how-to-use-table-storage/blob6.png
  [Blob7]: ./media/storage-dotnet-how-to-use-table-storage/blob7.png
  [Blob8]: ./media/storage-dotnet-how-to-use-table-storage/blob8.png
  [Blob9]: ./media/storage-dotnet-how-to-use-table-storage/blob9.png

  [Predstavljanje Upsert i projekcije upita na blogu]: http://blogs.msdn.com/b/windowsazurestorage/archive/2011/09/15/windows-azure-tables-introducing-upsert-and-query-projection.aspx
  [.NET Client Library reference]: http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
  [Azure Storage Team blog]: http://blogs.msdn.com/b/windowsazurestorage/
  [Configure Azure Storage connection strings]: http://msdn.microsoft.com/library/azure/ee758697.aspx
  [OData]: http://nuget.org/packages/Microsoft.Data.OData/5.0.2
  [Edm]: http://nuget.org/packages/Microsoft.Data.Edm/5.0.2
  [Spatial]: http://nuget.org/packages/System.Spatial/5.0.2
  [How to: Programmatically access Table storage]: #tablestorage
