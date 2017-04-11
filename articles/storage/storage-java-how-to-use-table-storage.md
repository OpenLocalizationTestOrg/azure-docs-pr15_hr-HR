<properties
    pageTitle="Upute za korištenje spremišta tablica iz Java | Microsoft Azure"
    description="Pohranite strukturiranih podataka u oblak pomoću tablice Azure prostor za pohranu, NoSQL izvor podataka."
    services="storage"
    documentationCenter="java"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="how-to-use-table-storage-from-java"></a>Upute za korištenje spremišta tablica iz Java

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Pregled

Ovaj vodič vidjet ćete kako izvršiti uobičajeni scenariji putem servisa za pohranu za Azure tablice. Primjere zapisuju u Java i korištenje [SDK za pohranu Azure Java][]. Scenariji u kojima je moguće **Stvaranje**, **unos**i **Brisanje** tablica, kao i **umetanja**, **slanje upita**, **Izmjena**i **Brisanje** entiteti obuhvaćaju u tablici. Dodatne informacije o tablicama potražite u odjeljku [sljedeće korake](#Next-Steps) .

Napomena: Programa SDK dostupan je za razvojne inženjere koji koriste Azure prostora za pohranu na uređaje sa sustavom Android. Dodatne informacije potražite u članku [Azure SDK za pohranu za Android][].

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Stvaranje aplikacije Java

U ovom vodiču će se koristiti za pohranu značajke koje se mogu se pokrenuti aplikacije Java lokalno ili u kod koji se izvodi unutar uloga web ili uloga suradnika u Azure.

Da biste to učinili, morat ćete instalirati Java Development Kit (JDK) i stvorite račun za Azure prostora za pohranu u pretplatu za Azure. Nakon što ste to učinili, morat ćete provjerite ispunjava li sustav razvoj minimalni preduvjeti i ovisnosti koje su navedene u spremištu [SDK za pohranu Azure Java][] na GitHub. Ako sustav zadovoljava te preduvjete, slijedite upute za preuzimanje i instaliranje biblioteke za pohranu Azure za Java na računalu te spremištu. Nakon što ste dovršili te zadatke, moći da biste stvorili Java aplikacije koja koristi se primjerima u ovom članku.

## <a name="configure-your-application-to-access-table-storage"></a>Konfiguriranje aplikacija za pristup spremište tablica

Dodajte sljedeće naredbe Uvezi na vrh Java datoteku koju želite koristiti za pohranu Microsoft Azure API-ji za pristup tablice:

    // Include the following imports to use table APIs
    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.table.*;
    import com.microsoft.azure.storage.table.TableQuery.*;

## <a name="setup-an-azure-storage-connection-string"></a>Postavljanje niz za povezivanje programa Azure prostora za pohranu

Klijent za Azure prostora za pohranu koristi niza za povezivanje za pohranu za pohranu krajnje točke i vjerodajnice za pristup servisa za upravljanje podacima. Kada se pokrene u klijentskoj aplikaciji, morate unijeti niz za povezivanje za pohranu u sljedećem obliku pomoću naziv računa za pohranu i pristup primarni ključ za račun za pohranu koji je naveden u [Azure Portal](https://portal.azure.com) za *AccountName* i *AccountKey* vrijednosti. Ovaj primjer pokazuje kako deklarirati statične polja držite niz za povezivanje:

    // Define the connection-string with your values.
    public static final String storageConnectionString =
        "DefaultEndpointsProtocol=http;" +
        "AccountName=your_storage_account;" +
        "AccountKey=your_storage_account_key";

U aplikaciji radi unutar uloga u Microsoft Azure, taj niz moguće pohraniti u datoteci konfiguracije servisa, *ServiceConfiguration.cscfg*te im možete pristupiti s pozivom **RoleEnvironment.getConfigurationSettings** korakom. Evo primjera dohvaćanje niza za povezivanje iz **postavka** element pod nazivom *StorageConnectionString* u konfiguracijskoj datoteci servisa:

    // Retrieve storage account from connection-string.
    String storageConnectionString =
        RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");

Sljedeća primjera pretpostavlja da koristite jedan od ta dva načina za dohvaćanje niza veze za pohranu.

## <a name="how-to-create-a-table"></a>Kako: Stvaranje tablice

Objekt programa **CloudTableClient** omogućuje vam dohvaćanje objekata referenca za tablice i entiteti. Sljedeći kod stvara **CloudTableClient** objekt, a koristi ga da biste stvorili novi objekt **CloudTable** koji predstavlja tablice pod nazivom "osobe". (Napomena: postoje dodatni Načini stvaranja objekata **CloudStorageAccount** ; dodatne informacije potražite u članku **CloudStorageAccount** [Azure prostora za pohranu klijenta SDK referenca].)

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

       // Create the table client.
       CloudTableClient tableClient = storageAccount.createCloudTableClient();

       // Create the table if it doesn't exist.
       String tableName = "people";
       CloudTable cloudTable = tableClient.getTableReference(tableName);
       cloudTable.createIfNotExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-list-the-tables"></a>Kako: popis tablice

Da biste dobili popis tablica, nazovite metodu **CloudTableClient.listTables()** za dohvaćanje iterable popis naziva tablica.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Loop through the collection of table names.
        for (String table : tableClient.listTables())
        {
          // Output each table name.
          System.out.println(table);
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-add-an-entity-to-a-table"></a>Kako: dodavanje entitet u tablicu

Mapirajte entiteti Java objekata pomoću prilagođenih klasu implementacijom **TableEntity**. Zbog praktičnosti klase **TableServiceEntity** implementira **TableEntity** i koristi odraz za mapiranje svojstava načinima dobavljač i postavljač pod nazivom za svojstva. Da biste tablicu dodali entitet, stvorite predmete koji definira svojstva vaše entitet. Sljedeći kod definira je entitet predmete koju koristi ime klijenta kao redak ključ i prezime kao ključ particije. Zajednički je entitet particija i redak ključ identificirati samo entitet u tablici. Moguće je brže od onih s tipkama različite particije mu entiteti s istim ključem particija.

    public class CustomerEntity extends TableServiceEntity {
        public CustomerEntity(String lastName, String firstName) {
            this.partitionKey = lastName;
            this.rowKey = firstName;
        }

        public CustomerEntity() { }

        String email;
        String phoneNumber;

        public String getEmail() {
            return this.email;
        }

        public void setEmail(String email) {
            this.email = email;
        }

        public String getPhoneNumber() {
            return this.phoneNumber;
        }

        public void setPhoneNumber(String phoneNumber) {
            this.phoneNumber = phoneNumber;
        }
    }

Postupci tablice koje obuhvaćaju entiteti zahtijevaju **TableOperation** objekt. Taj objekt definira operaciju koja će se izvršiti na entitet, koje možete izvršavati s objektom **CloudTable** . Sljedeći kod stvara novu instancu klase **CustomerEntity** neke korisničkih podataka za pohranu. Kod zatim poziva **TableOperation.insertOrReplace** da biste stvorili **TableOperation** objekt da biste umetnuli entitet u tablicu i novu **CustomerEntity** povezuje s njim. Na kraju, kod poziva metodu **izvoditi** na objekt **CloudTable** , navodeći "osobe" tablica i na nove **TableOperation**, koja se šalje zahtjev servis za pohranu umetnite novi entitet kupca u tablici "osobe" ili zamjene entitet ako već postoji.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a new customer entity.
        CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
        customer1.setEmail("Walter@contoso.com");
        customer1.setPhoneNumber("425-555-0101");

        // Create an operation to add the new customer to the people table.
        TableOperation insertCustomer1 = TableOperation.insertOrReplace(customer1);

        // Submit the operation to the table service.
        cloudTable.execute(insertCustomer1);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-insert-a-batch-of-entities"></a>Kako: Umetanje skupine entiteti

Skupine entiteti na servis za tablicu možete umetnuti u jedan postupak pisanja. Sljedeći kod stvara **TableBatchOperation** objekt, a zatim dodaje tri Umetanje operacije da biste ga. Svaka Umetanje operacija je dodao stvaranja novog objekta entitet, postavljanje vrijednosti i zatim pozivanje metode za **Umetanje** na **TableBatchOperation** objektu želite pridružiti entitet nova operacija za umetanje. Zatim kod poziva **izvoditi** na objekt **CloudTable** , navodeći "osobe" tablica i **TableBatchOperation** objektom, čime se šalje obradu Postupci tablice servis za pohranu u jedan zahtjev.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Define a batch operation.
        TableBatchOperation batchOperation = new TableBatchOperation();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a customer entity to add to the table.
        CustomerEntity customer = new CustomerEntity("Smith", "Jeff");
        customer.setEmail("Jeff@contoso.com");
        customer.setPhoneNumber("425-555-0104");
        batchOperation.insertOrReplace(customer);

       // Create another customer entity to add to the table.
       CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
       customer2.setEmail("Ben@contoso.com");
       customer2.setPhoneNumber("425-555-0102");
       batchOperation.insertOrReplace(customer2);

       // Create a third customer entity to add to the table.
       CustomerEntity customer3 = new CustomerEntity("Smith", "Denise");
       customer3.setEmail("Denise@contoso.com");
       customer3.setPhoneNumber("425-555-0103");
       batchOperation.insertOrReplace(customer3);

       // Execute the batch of operations on the "people" table.
       cloudTable.execute(batchOperation);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

Neke stvari obratiti pažnju na obradu postupci:

- Možete izvršiti do 100 Umetanje, brisanje, spajanje, zamijeniti, umetanje ili spajanje, i umetnuti ili zamjena operacije u bilo koju kombinaciju grupe za jedan.
- Skupne operacije može imati operacije Vrati ako je operaciju samo u grupu.
- Svi entiteti u operaciji jedne grupe moraju imati isti particija ključa.
- Skupne operacije ograničeno je na teret za podataka 4MB.

## <a name="how-to-retrieve-all-entities-in-a-partition"></a>Kako: dohvaćanje svi entiteti u particije

Da biste tablicu za entiteti u particije za upite, možete koristiti **TableQuery**. Nazovite **TableQuery.from** za stvaranje upita na određene tablice koji vraća vrstu navedeni rezultata. Sljedeći kod navodi filtar za entiteti gdje je "Kopač" tipku particije. **TableQuery.generateFilterCondition** je metoda preglednika da biste stvorili filtre za upite. Pozivanje **gdje** na referencu vratio **TableQuery.from** način da biste primijenili filtar na upit. Pri izvođenju upit s pozivom **izvoditi** na objekt **CloudTable** vraća **Iterator** s navedena vrsta rezultata **CustomerEntity** . Možete koristiti **Iterator** vraćeni u je za svaki petlja trošiti rezultate. Kod ispisuje polja svaki entitet u rezultatima upita na konzolu.

    try
    {
        // Define constants for filters.
        final String PARTITION_KEY = "PartitionKey";
        final String ROW_KEY = "RowKey";
        final String TIMESTAMP = "Timestamp";

        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

       // Create a cloud table object for the table.
       CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a filter condition where the partition key is "Smith".
        String partitionFilter = TableQuery.generateFilterCondition(
           PARTITION_KEY,
           QueryComparisons.EQUAL,
           "Smith");

       // Specify a partition query, using "Smith" as the partition key filter.
       TableQuery<CustomerEntity> partitionQuery =
           TableQuery.from(CustomerEntity.class)
           .where(partitionFilter);

        // Loop through the results, displaying information about the entity.
        for (CustomerEntity entity : cloudTable.execute(partitionQuery)) {
            System.out.println(entity.getPartitionKey() +
                " " + entity.getRowKey() +
                "\t" + entity.getEmail() +
                "\t" + entity.getPhoneNumber());
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-retrieve-a-range-of-entities-in-a-partition"></a>Kako: dohvaćanje raspon entiteti u particije

Ako ne želite poslati upit svi entiteti particije, možete odrediti raspon pomoću operatori usporedbe u filtru. Sljedeći kod spaja dva filtra da biste dobili svi entiteti u particije "Kovač" gdje se ključ retka (ime) počinje slovom najviše "E" abecede. Nakon toga ispisuje rezultata upita. Ako koristite entiteti dodano u tablicu u grupi Umetanje ovog vodiča, samo dva entiteti vraćaju ovaj put (Ben i Kevin); Jeff Novak nije uključen.

    try
    {
        // Define constants for filters.
        final String PARTITION_KEY = "PartitionKey";
        final String ROW_KEY = "RowKey";
        final String TIMESTAMP = "Timestamp";

        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

       // Create the table client.
       CloudTableClient tableClient = storageAccount.createCloudTableClient();

       // Create a cloud table object for the table.
       CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a filter condition where the partition key is "Smith".
        String partitionFilter = TableQuery.generateFilterCondition(
           PARTITION_KEY,
           QueryComparisons.EQUAL,
           "Smith");

        // Create a filter condition where the row key is less than the letter "E".
        String rowFilter = TableQuery.generateFilterCondition(
           ROW_KEY,
           QueryComparisons.LESS_THAN,
           "E");

        // Combine the two conditions into a filter expression.
        String combinedFilter = TableQuery.combineFilters(partitionFilter,
            Operators.AND, rowFilter);

        // Specify a range query, using "Smith" as the partition key,
        // with the row key being up to the letter "E".
        TableQuery<CustomerEntity> rangeQuery =
           TableQuery.from(CustomerEntity.class)
           .where(combinedFilter);

        // Loop through the results, displaying information about the entity
        for (CustomerEntity entity : cloudTable.execute(rangeQuery)) {
            System.out.println(entity.getPartitionKey() +
                " " + entity.getRowKey() +
                "\t" + entity.getEmail() +
                "\t" + entity.getPhoneNumber());
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-retrieve-a-single-entity"></a>Kako: dohvaćanje jedan entitet

Upit za dohvaćanje jedne, specifične entitet možete napisati. Sljedeći kod poziva **TableOperation.retrieve** s particije ključ i redak ključne parametre da biste odredili kupca "Jeff kovač", umjesto stvaranja **TableQuery** i pomoću filtara na isti način. Pri izvođenju postupka dohvaćanje vraća samo jedan entitet, umjesto zbirku. Način **getResultAsType** oblikuje rezultat vrste dodjele cilja, **CustomerEntity** objekt. Ta vrsta nije kompatibilno s vrstom navedeni za upit, izbačena će iznimka. Ako nema entitet ima točan particija i redak ključ koji se podudaraju, vraća se vrijednost null. Određivanje particija i redak tipke u upitu je najbrži način za dohvaćanje jedan entitet iz tablice servisa.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Retrieve the entity with partition key of "Smith" and row key of "Jeff"
        TableOperation retrieveSmithJeff =
           TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

       // Submit the operation to the table service and get the specific entity.
       CustomerEntity specificEntity =
            cloudTable.execute(retrieveSmithJeff).getResultAsType();

        // Output the entity.
        if (specificEntity != null)
        {
            System.out.println(specificEntity.getPartitionKey() +
                " " + specificEntity.getRowKey() +
                "\t" + specificEntity.getEmail() +
                "\t" + specificEntity.getPhoneNumber());
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-modify-an-entity"></a>Kako: izmjena entitet

Da biste izmijenili entitet, dohvatiti iz tablice servisa, unesite promjene na objekt entitet, i spremite željene promjene natrag na servis za tablicu operacijom Zamijeni i cirkularnih pisama. Sljedeći kod mijenja postojeći klijent telefonski broj. Umjesto pozivanje **TableOperation.insert** kao što smo jeste li za umetanje, kod poziva **TableOperation.replace**. Način **CloudTable.execute** poziva servisa tablice, a entitet je zamijenjena, osim ako neku drugu aplikaciju ga promijenili u vremenu jer ga dohvatili ove aplikacije. Kada se to dogodi, nije izbačena, a entitet mora biti dohvaćeni, izmijeniti i ponovno spremaju. Ovaj uzorak pokušaj optimistična Procjena istodobnosti nije zajednička sustava raspodijeljeno spremišta.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Retrieve the entity with partition key of "Smith" and row key of "Jeff".
        TableOperation retrieveSmithJeff =
           TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

        // Submit the operation to the table service and get the specific entity.
        CustomerEntity specificEntity =
          cloudTable.execute(retrieveSmithJeff).getResultAsType();

        // Specify a new phone number.
        specificEntity.setPhoneNumber("425-555-0105");

        // Create an operation to replace the entity.
        TableOperation replaceEntity = TableOperation.replace(specificEntity);

        // Submit the operation to the table service.
        cloudTable.execute(replaceEntity);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-query-a-subset-of-entity-properties"></a>Kako: podskup entitet svojstva za upite

Upit u tablicu možete dohvatiti samo nekoliko svojstva iz entitet. Ovu tehniku naziva projekcije, smanjuje propusnost i poboljšati performanse upita, posebice za velike entiteti. Upit u sljedeći kod koristi metodu **Odaberite** da biste vratili samo adrese e-pošte od entiteti u tablici. Rezultati su Predviđeni u zbirku **niza** uz pomoć programa **EntityResolver**, koji ne pretvorbi vrsta na entiteti vraćena s poslužitelja. Dodatne informacije o projekciji u [Azure tablice: Uvod Upsert i projekcije upita][]. Imajte na umu da projekcije nije podržana na emulator lokalno spremište tako kod će se pokrenuti samo ako koristite račun na servisu tablice.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Define a projection query that retrieves only the Email property
        TableQuery<CustomerEntity> projectionQuery =
           TableQuery.from(CustomerEntity.class)
           .select(new String[] {"Email"});

        // Define a Entity resolver to project the entity to the Email value.
        EntityResolver<String> emailResolver = new EntityResolver<String>() {
            @Override
            public String resolve(String PartitionKey, String RowKey, Date timeStamp, HashMap<String, EntityProperty> properties, String etag) {
                return properties.get("Email").getValueAsString();
            }
        };

        // Loop through the results, displaying the Email values.
        for (String projectedString :
            cloudTable.execute(projectionQuery, emailResolver)) {
                System.out.println(projectedString);
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-insert-or-replace-an-entity"></a>Kako: Umetanje i zamjena entitet

Često želite dodati entitet tablice bez ako već postoji u tablici. Umetanje ili Zamijeni postupak omogućuje poslati jedan zahtjev koji će umetnuti entitet ako ne postoji ili zamijeniti postojeći ako ga ne. Stvaranje na prethodnog primjera, sljedeći kod umeće ili zamjenjuje entitet za "Walter Harp". Kada stvorite novi entitet, kod poziva metodu **TableOperation.insertOrReplace** . Kod zatim poziva **izvoditi** na objekt **CloudTable** s tablice i insert ili zamijenite operaciju tablice kao parametre. Da biste ažurirali samo dio entitet, metodu **TableOperation.insertOrMerge** može se koristiti umjesto toga. Imajte na umu te umetanje-ili-Zamijeni nije podržana na emulator lokalno spremište tako kod će se pokrenuti samo ako koristite račun na servisu tablice. Možete dodatne informacije o umetanje ili Zamijeni i umetanje ili-pisma u ovoj [Azure tablice: Uvod u Upsert i projekcije upita][].

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a new customer entity.
        CustomerEntity customer5 = new CustomerEntity("Harp", "Walter");
        customer5.setEmail("Walter@contoso.com");
        customer5.setPhoneNumber("425-555-0106");

        // Create an operation to add the new customer to the people table.
        TableOperation insertCustomer5 = TableOperation.insertOrReplace(customer5);

        // Submit the operation to the table service.
        cloudTable.execute(insertCustomer5);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-delete-an-entity"></a>Kako: brisanje entitet

Jednostavno možete izbrisati entitet nakon što ga dohvatili. Nakon što se dohvate entitet, nazovite **TableOperation.delete** entitet da biste izbrisali. Zatim pozivanje **izvoditi** na **CloudTable** objekt. Sljedeći kod dohvaća i briše entitet klijenta.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Create an operation to retrieve the entity with partition key of "Smith" and row key of "Jeff".
        TableOperation retrieveSmithJeff = TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

        // Retrieve the entity with partition key of "Smith" and row key of "Jeff".
        CustomerEntity entitySmithJeff =
            cloudTable.execute(retrieveSmithJeff).getResultAsType();

        // Create an operation to delete the entity.
        TableOperation deleteSmithJeff = TableOperation.delete(entitySmithJeff);

        // Submit the delete operation to the table service.
        cloudTable.execute(deleteSmithJeff);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-delete-a-table"></a>Kako: brisanje tablice

Na kraju, sljedeći kod briše tablicu s računom za pohranu. Tablice koji je izbrisan bit će dostupan moguće ponovno stvoriti za neko vrijeme nakon brisanja, obično manje od četrdeset sekundi.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Delete the table and all its data if it exists.
        CloudTable cloudTable = new CloudTable("people",tableClient);
        cloudTable.deleteIfExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste naučili osnove spremište tablica, slijedite ove veze da biste saznali kako obavljati zadatke složenije prostora za pohranu.

- [Azure prostora za pohranu SDK Java][]
- [Azure prostora za pohranu klijenta SDK Reference][]
- [Azure prostora za pohranu REST API-JA][]
- [Blog tima za Azure prostora za pohranu][]

Dodatne informacije potražite u odjeljku [Razvojni centar za Java](/develop/java/).


[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure prostora za pohranu SDK Java]: https://github.com/azure/azure-storage-java
[Azure prostora za pohranu SDK za Android]: https://github.com/azure/azure-storage-android
[Azure prostora za pohranu klijenta SDK Reference]: http://dl.windowsazure.com/storage/javadoc/
[Azure prostora za pohranu REST API-JA]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Blog tima za Azure prostora za pohranu]: http://blogs.msdn.com/b/windowsazurestorage/
[Azure tablice: Uvod u korištenje Upsert i projekcije upita]: http://blogs.msdn.com/b/windowsazurestorage/archive/2011/09/15/windows-azure-tables-introducing-upsert-and-query-projection.aspx
