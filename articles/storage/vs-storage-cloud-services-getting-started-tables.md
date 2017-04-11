<properties
    pageTitle="Početak rada sa spremištem tablica i Visual Studio povezani services (Servisi u oblaku) | Microsoft Azure"
    description="Upute za početak rada s spremište tablica Azure u oblak servisa projekta u Visual Studio nakon povezivanja s računom za pohranu pomoću Visual Studio povezani servisi"
    services="storage"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="web"
    ms.tgt_pltfrm="vs-getting-started"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="tarcher"/>

# <a name="getting-started-with-azure-table-storage-and-visual-studio-connected-services-cloud-services-projects"></a>Uvod u spremište tablica platforme Azure i Visual Studio povezani services (oblaka services projekata)

[AZURE.INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

##<a name="overview"></a>Pregled

U ovom se članku opisuje kako početi koristiti spremište tablica platforme Azure u Visual Studio nakon što ste stvorili ili račun Azure prostora za pohranu u projektu services oblaka pozivaju pomoću dijaloškog okvira Visual Studio **Dodavanje povezani servisi** . Operacija **Dodavanje povezani servisi** instalira odgovarajuće NuGet paketa da biste pristupili Azure prostora za pohranu u projektu i dodaje niz za povezivanje za račun za pohranu datoteka konfiguracije projekta.

Servis za pohranu Azure tablice omogućuje pohranu velikih količina strukturiranih podataka. Servis je datastore NoSQL koje prihvaća čija je autentičnost provjerena pozive s unutrašnje i vanjske Azure oblaka. Azure tablice su idealna za pohranu strukturirane, koji nisu relacijskih podataka.

Da bismo započeli, najprije morate stvoriti tablicu na vašem računu za pohranu. Pokazat ćemo vam kako stvoriti Azure tablice u kodu i upute za izvođenje osnovnih tablice i operacije entitet, kao što su dodavanje, izmjena, za čitanje i entiteti tablice za čitanje. Primjere zapisuju u C\# kod, a zatim koristite [spremište na platformi Microsoft Azure klijentska biblioteka za .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).

**BILJEŠKE:** Neke od API-ji izvršiti pozive izvan Azure prostora za pohranu su asinkronog. Dodatne informacije potražite [Asynchronous programiranje asinkrone i Await](http://msdn.microsoft.com/library/hh191443.aspx) . Ispod kod pretpostavlja da koriste asinkrone programiranje metode.

- [Početak rada sa spremištem tablica Azure pomoću .NET](storage-dotnet-how-to-use-tables.md) potražite dodatne informacije o programski rukovanje tablice.
- Opće informacije o Azure prostora za pohranu potražite u članku [dokumentaciju za pohranu](https://azure.microsoft.com/documentation/services/storage/) .
- Opće informacije o servisima za Azure oblaka potražite u članku [dokumentaciju za servise u Oblaku](https://azure.microsoft.com/documentation/services/cloud-services/) .
- Dodatne informacije o programskom ASP.NET aplikacija potražite u članku [ASP.NET](http://www.asp.net) .

## <a name="access-tables-in-code"></a>Tablice programa Access u kodu

Da biste pristupili tablica u oblak servisa projektima, morate uključiti sljedeće stavke C# izvorne datoteke koje pristup spremište tablica platforme Azure.

1. Provjerite je li polje naziva deklaracija pri vrhu datoteke C# uključiti te **pomoću** naredbe.

        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Table;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;

2. Pronađite objekt **CloudStorageAccount** koji predstavlja prostor za pohranu podataka o računu. Koristite sljedeći kod da biste dobili niz za povezivanje za pohranu i podataka o računu za pohranu iz servisa Azure konfiguracije.

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage account name>
         _AzureStorageConnectionString"));
> [AZURE.NOTE]  Koristiti sve iznad kod ispred koda u sljedeća primjera.

3. Pronađite objekt **CloudTableClient** referentni tablice objekata na vašem računu za pohranu.

         // Create the table client.
         CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

4. Pronađite objekt za reference **CloudTable** referentni određenom tablicom i entiteti.

        // Get a reference to a table named "peopleTable".
        CloudTable peopleTable = tableClient.GetTableReference("peopleTable");

## <a name="create-a-table-in-code"></a>Stvaranje tablice u kodu

Da biste stvorili tablica platforme Azure, jednostavno dodajte poziv **CreateIfNotExistsAsync** da biste na kada dobijete **CloudTable** objekt kao što je opisano u odjeljku "Pristup tablice u kodu".

    // Create the CloudTable if it does not exist.
    await peopleTable.CreateIfNotExistsAsync();

## <a name="add-an-entity-to-a-table"></a>Dodavanje entitet u tablicu

Da biste tablicu dodali entitet, stvorite klasu koja definira svojstva vaše entitet. Sljedeći kod definira je entitet predmete naziva **CustomerEntity** koji koristi ime klijenta kao tipku retka i ime i prezime kao ključ particije.

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

Postupci tablice koje obuhvaćaju entiteti gotovi pomoću **CloudTable** objekta koji ste stvorili ranije u "pristup tablice u kodu." Objekt **TableOperation** predstavlja operaciju koja će se izvršiti. Sljedeći primjer koda prikazuje kako stvoriti **CloudTable** objekta i **CustomerEntity** objekt. Da biste pripremili postupka, **TableOperation** stvara se da biste umetnuli entitet klijenta u tablicu. Na kraju se izvršava postupak tako da nazovete **CloudTable.ExecuteAsync**.

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";

    // Create the TableOperation that inserts the customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);

    // Execute the insert operation.
    await peopleTable.ExecuteAsync(insertOperation);


## <a name="insert-a-batch-of-entities"></a>Umetanje skupine entiteti

Možete umetnuti više entiteti u tablicu u operaciji jedan unos. Sljedeći primjer koda stvara dva objekte ("Jeff kopač" i "Ben kovač"), ih dodaje u objekt **TableBatchOperation** pomoću metode za umetanje, a zatim pokreće postupak tako da nazovete **CloudTable.ExecuteBatchAsync**.

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
    await peopleTable.ExecuteBatchAsync(batchOperation);

## <a name="get-all-of-the-entities-in-a-partition"></a>Čine entiteti u particije

Da biste upit tablice za sve entiteti u particija koristite **TableQuery** objekt. Sljedeći primjer koda navodi filtar za entiteti gdje je "Kopač" tipku particije. U ovom se primjeru ispisuje polja svaki entitet u rezultatima upita na konzolu.

    // Construct the query operation for all customer entities where PartitionKey="Smith".
    TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>()
        .Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

    // Print the fields for each customer.
    TableContinuationToken token = null;
    do
    {
        TableQuerySegment<CustomerEntity> resultSegment = await peopleTable.ExecuteQuerySegmentedAsync(query, token);
        token = resultSegment.ContinuationToken;

        foreach (CustomerEntity entity in resultSegment.Results)
        {
            Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
            entity.Email, entity.PhoneNumber);
        }
    } while (token != null);

    return View();


## <a name="get-a-single-entity"></a>Početak jedan entitet

Možete sastaviti upit da biste dobili jedinstvenog i određene entitet. Sljedeći kod koristi **TableOperation** objekt da biste odredili klijenta koji se zove "Ben kovač". Ta metoda vraća samo jedan entitet, umjesto zbirka i vraćene vrijednosti u **TableResult.Result** je **CustomerEntity** objekt. Određivanje particija i redak tipke u upitu je najbrži način za dohvaćanje jedan entitet iz **tablice** servisa.

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the retrieve operation.
    TableResult retrievedResult = await peopleTable.ExecuteAsync(retrieveOperation);

    // Print the phone number of the result.
    if (retrievedResult.Result != null)
       Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
    else
       Console.WriteLine("The phone number could not be retrieved.");

## <a name="delete-an-entity"></a>Brisanje entitet
Možete izbrisati entitet kada je pronađete. Sljedeći kod traži kupca entitet pod nazivom "Ben kovač", a ako se pronađe, briše se.

    // Create a retrieve operation that expects a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = peopleTable.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity object.
    CustomerEntity deleteEntity = (CustomerEntity)retrievedResult.Result;

    // Create the Delete TableOperation and then execute it.
    if (deleteEntity != null)
    {
       TableOperation deleteOperation = TableOperation.Delete(deleteEntity);

       // Execute the operation.
       await peopleTable.ExecuteAsync(deleteOperation);

       Console.WriteLine("Entity deleted.");
    }

    else
       Console.WriteLine("Couldn't delete the entity.");

## <a name="next-steps"></a>Daljnji koraci

[AZURE.INCLUDE [vs-storage-dotnet-tables-next-steps](../../includes/vs-storage-dotnet-tables-next-steps.md)]
