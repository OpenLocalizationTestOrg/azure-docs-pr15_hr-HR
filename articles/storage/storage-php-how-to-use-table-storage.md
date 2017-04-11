<properties
    pageTitle="Upute za korištenje spremišta tablica iz PHP | Microsoft Azure"
    description="Saznajte kako pomoću servisa tablice iz PHP za stvaranje i brisanje tablice, umetanje, brisanje i upit u tablici."
    services="storage"
    documentationCenter="php"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="php"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="how-to-use-table-storage-from-php"></a>Upute za korištenje spremišta tablica iz PHP

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Pregled

Ovaj vodič prikazuje kako izvesti uobičajeni scenariji pomoću servisa Azure tablice. Primjere zapisuju u PHP i koristiti [SDK Azure i][download]. Scenariji u kojima je moguće uvrstiti **Stvaranje i brisanje tablice i umetanje, brisanje, i postavljanje upita entiteti u tablici**. Dodatne informacije o servisa Azure tablice potražite u odjeljku [sljedeće korake](#next-steps) .

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a>Stvaranje i aplikacije

Samo preduvjeta za stvaranje i aplikacije koji pristupa servisa Azure tablice je na upućuju od klase u Azure SDK za PHP iz unutar. Da biste stvorili aplikacije, uključujući Blok za pisanje možete koristiti bilo koji alat za razvoj.

U ovom vodiču pomoću značajke servisa tablica koje možete pozvati iz aplikacije PHP lokalno ili kod koji se izvodi unutar Azure web ulogu, ulogu suradnika ili web-mjesta.

## <a name="get-the-azure-client-libraries"></a>Početak biblioteke Azure klijenta

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-access-the-table-service"></a>Konfiguriranje aplikacija za pristup servisu tablice

Da biste pomoću servisa Azure tablice API-ji, morate:

1. Referenca autoloader datoteku pomoću [require_once] [ require_once] naredbe, a
2. Reference sve klase koje koristite.

Sljedeći primjer pokazuje kako uključiti autoloader datoteku i referencu **ServicesBuilder** predmete.

> [AZURE.NOTE] U ovom se primjeru (i još primjera u ovom članku) pretpostavlja da ste instalirali biblioteke klijenta i za Azure putem Skladatelj. Ako ručno instalirati biblioteke, morate upućuju na <code>WindowsAzure.php</code> autoloader datoteku.

    require_once 'vendor/autoload.php';
    use WindowsAzure\Common\ServicesBuilder;


U primjerima koji slijede, u `require_once` izjava uvijek prikazani, ali samo klasa potrebne navedeni primjer ispravno izvršiti referencirani.

## <a name="set-up-an-azure-storage-connection"></a>Postavljanje veze sustava Azure prostora za pohranu

Stvoriti instancu klijent za servis za Azure tablice, prvo morate imati valjani niz za povezivanje. Oblik za niz za povezivanje servisa tablice je:

Za pristup uživo servisa:

    DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]

Za pristup prostora za pohranu emulator:

    UseDevelopmentStorage=true


Da biste stvorili bilo kojeg servisa Azure klijenta, morate koristiti **ServicesBuilder** klasu. možeš:

* Prenesite niz za povezivanje izravno ga ili
* Korištenje **CloudConfigurationManager (CCM)** da biste provjerili više vanjskih izvora niz za povezivanje:
    * prema zadanim postavkama dolazi s podrškom za jedan vanjski izvor - okolini varijable
    * možete dodati nove izvore produženjem klase **ConnectionStringSource**

Primjer navedene u nastavku, niz za povezivanje će se proslijediti izravno.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;

    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);


## <a name="create-a-table"></a>Stvaranje tablice

Objekt programa **TableRestProxy** omogućuje vam stvaranje tablice pomoću metode **createTable** . Prilikom stvaranja tablice, možete postaviti ograničenja servisa tablice. (Dodatne informacije o vremenskog ograničenja servisa tablice potražite u članku [Postavka vremenska ograničenja tablice terenu][table-service-timeouts].)

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    try {
        // Create table.
        $tableRestProxy->createTable("mytable");
    }
    catch(ServiceException $e){
        $code = $e->getCode();
        $error_message = $e->getMessage();
        // Handle exception based on error codes and messages.
        // Error codes and messages can be found here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
    }

Informacije o ograničenjima nazive tablica, u odjeljku [Osnove podatkovnog modela servisa u tablici][table-data-model].

## <a name="add-an-entity-to-a-table"></a>Dodavanje entitet u tablicu

Da biste tablicu dodali entitet, stvorite novi objekt **entitet** i prenesite ga **TableRestProxy -> insertEntity**. Imajte na umu da kada stvarate entitet, morate navesti na `PartitionKey` i `RowKey`. Te su odredišnih stilova za entitet i vrijednosti koje možete mu mnogo brže od ostalih svojstava entitet. Sustav koristi `PartitionKey` automatski distribuirati entiteti tablice iznad čvorove mnogo prostora za pohranu. Entiteti s istim `PartitionKey` pohranjuju se na istom čvor. (Izvođenje operacije na više entiteti pohranjene na istom čvor bolje nego na entiteti pohranjene preko različitih čvorove.) U `RowKey` je jedinstveni ID entitet unutar particije.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\Entity;
    use MicrosoftAzure\Storage\Table\Models\EdmType;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $entity = new Entity();
    $entity->setPartitionKey("tasksSeattle");
    $entity->setRowKey("1");
    $entity->addProperty("Description", null, "Take out the trash.");
    $entity->addProperty("DueDate",
                         EdmType::DATETIME,
                         new DateTime("2012-11-05T08:15:00-08:00"));
    $entity->addProperty("Location", EdmType::STRING, "Home");

    try{
        $tableRestProxy->insertEntity("mytable", $entity);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
    }

Informacije o svojstva tablice i vrste potražite u članku [Osnove podatkovnog modela servisa u tablici][table-data-model].

Klase **TableRestProxy** nudi dvije druge metode za umetanje entiteti: **insertOrMergeEntity** i **insertOrReplaceEntity**. Da biste koristili ovih metoda, stvorite novi **entitet** , a zatim prosljeđivanje kao parametar za obje metode. Svaki način će umetnuti entitet ako ne postoji. Ako već postoji entitet, **insertOrMergeEntity** ažurira vrijednosti nekretnina ako svojstava već postoji i dodaje se nova svojstva ako ne postoji, dok **insertOrReplaceEntity** potpuno zamjenjuje postojeći entitet. Sljedeći primjer pokazuje kako koristiti **insertOrMergeEntity**. Ako je entitet s `PartitionKey` "tasksSeattle" i `RowKey` "1" još ne postoji, ona će se umetnuti. Međutim, ako je prethodno umetnut (kao što je prikazano u primjeru iznad), u `DueDate` ažurirat će se svojstva, i `Status` dodat će se svojstva. Na `Description` i `Location` svojstva ažuriraju se, ali s vrijednostima koje učinkovito ih ostavite nepromijenjena. Ako te potonjem dva svojstva ne dodaje kao što je prikazano u primjeru, ali postojali ciljni entitet, njihove postojeće vrijednosti želite ostaviti nepromijenjen.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\Entity;
    use MicrosoftAzure\Storage\Table\Models\EdmType;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    //Create new entity.
    $entity = new Entity();

    // PartitionKey and RowKey are required.
    $entity->setPartitionKey("tasksSeattle");
    $entity->setRowKey("1");

    // If entity exists, existing properties are updated with new values and
    // new properties are added. Missing properties are unchanged.
    $entity->addProperty("Description", null, "Take out the trash.");
    $entity->addProperty("DueDate", EdmType::DATETIME, new DateTime()); // Modified the DueDate field.
    $entity->addProperty("Location", EdmType::STRING, "Home");
    $entity->addProperty("Status", EdmType::STRING, "Complete"); // Added Status field.

    try {
        // Calling insertOrReplaceEntity, instead of insertOrMergeEntity as shown,
        // would simply replace the entity with PartitionKey "tasksSeattle" and RowKey "1".
        $tableRestProxy->insertOrMergeEntity("mytable", $entity);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }


## <a name="retrieve-a-single-entity"></a>Dohvaćanje jedne entitet

Način **TableRestProxy -> getEntity** omogućuje vam da dohvatite jedan entitet, slanje upita za njegov `PartitionKey` i `RowKey`. U primjeru niže tipku particija `tasksSeattle` i redak ključ `1` prosljeđuju **getEntity** korakom.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    try {
        $result = $tableRestProxy->getEntity("mytable", "tasksSeattle", 1);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

    $entity = $result->getEntity();

    echo $entity->getPartitionKey().":".$entity->getRowKey();

## <a name="retrieve-all-entities-in-a-partition"></a>Dohvaćanje svi entiteti u particije

Upiti entitet konstruirana su pomoću filtara (Dodatne informacije potražite u članku [slanje upita tablice i entiteti][filters]). Da biste dohvatili sve entiteti u particije, koristite filtar "PartitionKey eq *naziv_pogona*". Sljedeći primjer prikazuje način za dohvaćanje svi entiteti u na `tasksSeattle` particija prosljeđivanjem filtra metodu **queryEntities** .

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $filter = "PartitionKey eq 'tasksSeattle'";

    try {
        $result = $tableRestProxy->queryEntities("mytable", $filter);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

    $entities = $result->getEntities();

    foreach($entities as $entity){
        echo $entity->getPartitionKey().":".$entity->getRowKey()."<br />";
    }

## <a name="retrieve-a-subset-of-entities-in-a-partition"></a>Dohvaćanje podskup entiteti u particije

Isti uzorak koji se koriste u prethodnom primjeru može se koristiti za dohvaćanje sve podskup entiteti u particije. Podskup entiteti dohvatite ovise o filtra koristite (Dodatne informacije potražite u članku [slanje upita tablice i entiteti][filters]). Sljedeći primjer pokazuje kako pomoću filtra za dohvaćanje svi entiteti s određenim `Location` i `DueDate` manje od određenog datuma.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $filter = "Location eq 'Office' and DueDate lt '2012-11-5'";

    try {
        $result = $tableRestProxy->queryEntities("mytable", $filter);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

    $entities = $result->getEntities();

    foreach($entities as $entity){
        echo $entity->getPartitionKey().":".$entity->getRowKey()."<br />";
    }

## <a name="retrieve-a-subset-of-entity-properties"></a>Dohvaćanje podskup entitet svojstva

Upit možete dohvatiti podskup entitet svojstva. Ovu tehniku naziva *projekcije*, smanjuje propusnost i poboljšati performanse upita, posebice za velike entiteti. Da biste odredili svojstva dohvaćali, prenesite naziv svojstva način **upita -> addSelectField** . Da biste dodali još svojstava možete nazvati ovu metodu više puta. Nakon izvršavanja **TableRestProxy -> queryEntities**, vraćena entiteti će imati samo odabrane svojstva. (Ako želite vratiti podskup entiteti tablice pomoću filtra kao što je prikazano u upitima iznad.)

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\QueryEntitiesOptions;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $options = new QueryEntitiesOptions();
    $options->addSelectField("Description");

    try {
        $result = $tableRestProxy->queryEntities("mytable", $options);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

    // All entities in the table are returned, regardless of whether
    // they have the Description field.
    // To limit the results returned, use a filter.
    $entities = $result->getEntities();

    foreach($entities as $entity){
        $description = $entity->getProperty("Description")->getValue();
        echo $description."<br />";
    }

## <a name="update-an-entity"></a>Ažuriranje entitet

Postojeći entitet se može ažurirati pomoću metode **entitet -> setProperty** i **entitet -> addProperty** na entitet, a zatim pozivanje **TableRestProxy -> updateEntity**. U sljedećem primjeru dohvaća entitet, mijenja jedno svojstvo, uklanja drugi svojstvo i dodaje novo svojstvo. Imajte na umu da možete ukloniti svojstvo postavljanjem njegovom vrijednošću **null**.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\Entity;
    use MicrosoftAzure\Storage\Table\Models\EdmType;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $result = $tableRestProxy->getEntity("mytable", "tasksSeattle", 1);

    $entity = $result->getEntity();

    $entity->setPropertyValue("DueDate", new DateTime()); //Modified DueDate.

    $entity->setPropertyValue("Location", null); //Removed Location.

    $entity->addProperty("Status", EdmType::STRING, "In progress"); //Added Status.

    try {
        $tableRestProxy->updateEntity("mytable", $entity);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## <a name="delete-an-entity"></a>Brisanje entitet

Da biste izbrisali entitet, prenesite naziv tablice, a na entitet `PartitionKey` i `RowKey` **TableRestProxy -> deleteEntity** korakom.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    try {
        // Delete entity.
        $tableRestProxy->deleteEntity("mytable", "tasksSeattle", "2");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Imajte na umu da istodobnosti dividende, možete postaviti na e-oznake za entitet će se izbrisati pomoću metode **DeleteEntityOptions -> setEtag** i prosljeđivanje **DeleteEntityOptions** objekta u **deleteEntity** kao i četvrti parametar.

## <a name="batch-table-operations"></a>Postupci za obradu tablice

Način **obrade -> TableRestProxy** omogućuje dopušta izvršavanje operacija više u jedan zahtjev. S uzorkom uključuje dodate operacije **BatchRequest** objekt, a zatim prosljeđivanje objekt **BatchRequest** metodu **TableRestProxy -> grupe** . Da biste dodali operaciju **BatchRequest** objekta, možete bilo koju od sljedećih metoda više puta pozovete:

* **addInsertEntity** (dodaje operaciju insertEntity)
* **addUpdateEntity** (dodaje operaciju updateEntity)
* **addMergeEntity** (dodaje operacije mergeEntity)
* **addInsertOrReplaceEntity** (dodaje operaciju insertOrReplaceEntity)
* **addInsertOrMergeEntity** (dodaje operaciju insertOrMergeEntity)
* **addDeleteEntity** (dodaje deleteEntity operacije)

Sljedeći primjer prikazuje način dopušta izvršavanje operacija **insertEntity** i **deleteEntity** u jedan zahtjev:

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\Entity;
    use MicrosoftAzure\Storage\Table\Models\EdmType;
    use MicrosoftAzure\Storage\Table\Models\BatchOperations;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    // Create list of batch operation.
    $operations = new BatchOperations();

    $entity1 = new Entity();
    $entity1->setPartitionKey("tasksSeattle");
    $entity1->setRowKey("2");
    $entity1->addProperty("Description", null, "Clean roof gutters.");
    $entity1->addProperty("DueDate",
                          EdmType::DATETIME,
                          new DateTime("2012-11-05T08:15:00-08:00"));
    $entity1->addProperty("Location", EdmType::STRING, "Home");

    // Add operation to list of batch operations.
    $operations->addInsertEntity("mytable", $entity1);

    // Add operation to list of batch operations.
    $operations->addDeleteEntity("mytable", "tasksSeattle", "1");

    try {
        $tableRestProxy->batch($operations);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Dodatne informacije o Grupno slanje promjena Postupci tablice potražite u članku [Izvođenje transakcije grupu entitet][entity-group-transactions].

## <a name="delete-a-table"></a>Brisanje tablice

Na kraju, da biste izbrisali tablicu, naziv tablice prepustite metodu **TableRestProxy -> deleteTable** .

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    try {
        // Delete table.
        $tableRestProxy->deleteTable("mytable");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste naučili osnove servisa Azure tablice, slijedite ove veze dodatne informacije o složenije zadatke za pohranu.

- Posjetite [blog tima za pohranu za Azure](http://blogs.msdn.com/b/windowsazurestorage/)

Dodatne informacije potražite u odjeljku [Razvojni centar za PHP](/develop/php/).

[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[require_once]: http://php.net/require_once
[table-service-timeouts]: http://msdn.microsoft.com/library/azure/dd894042.aspx

[table-data-model]: http://msdn.microsoft.com/library/azure/dd179338.aspx
[filters]: http://msdn.microsoft.com/library/azure/dd894031.aspx
[entity-group-transactions]: http://msdn.microsoft.com/library/azure/dd894038.aspx
