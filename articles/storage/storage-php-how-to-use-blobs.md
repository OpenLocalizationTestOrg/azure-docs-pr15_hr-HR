<properties
    pageTitle="Upute za korištenje spremišta blobova (objekt spremište) iz PHP | Microsoft Azure"
    description="Pohranite nestrukturirane podatke u oblaku s Azure blobova (spremište objekta)."
    documentationCenter="php"
    services="storage"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="how-to-use-blob-storage-from-php"></a>Upute za korištenje spremišta blobova iz PHP

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Pregled

Azure blobova je servis koji pohranjuje nestrukturirane podatke u oblaku kao objekata/blob-ova. Spremište blobova platforme možete pohraniti sve vrste teksta ili binarne podatke, kao što su dokument, medijske datoteke ili program za instalaciju aplikacije. Spremište blobova platforme naziva se i spremište objekata.

Ovaj vodič prikazuje kako izvesti Najčešći scenariji putem servisa za blobova platforme Azure. Primjere zapisuju u PHP i koristiti [SDK Azure i] [download]. Scenariji u kojima je moguće uvrstiti **Prijenos**, **popis**, **Preuzimanje**i **Brisanje** blob-ova. Dodatne informacije o blob polja potražite u odjeljku [sljedeće korake](#next-steps) .

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a>Stvaranje i aplikacije

Samo preduvjeta za stvaranje i aplikacije koji pristupa blobova platforme Azure servis je u pozivanju od klase u Azure SDK za PHP iz unutar. Da biste stvorili aplikacije, uključujući Blok za pisanje možete koristiti bilo koji alat za razvoj.

U ovom vodiču pomoću značajke servisa koja možete naziva i aplikaciji lokalno ili kod koji se izvodi unutar Azure web uloge, ulogu suradnika ili web-mjesta.

## <a name="get-the-azure-client-libraries"></a>Početak biblioteke Azure klijenta

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-access-the-blob-service"></a>Konfiguriranje aplikacija za pristup servisu blobova platforme

Da biste koristili servis blobova platforme Azure API-ji, morate:

1. Referenca autoloader datoteku pomoću naredbe [require_once] i
2. Reference sve klase koje koristite.

Sljedeći primjer pokazuje kako uključiti autoloader datoteku i referencu **ServicesBuilder** predmete.

> [AZURE.NOTE] U ovom se primjeru (i još primjera u ovom članku) pretpostavlja da ste instalirali biblioteke klijenta i za Azure putem Skladatelj. Ako ručno instalirati biblioteke, morate upućuju na `WindowsAzure.php` autoloader datoteku.

    require_once 'vendor/autoload.php';
    use WindowsAzure\Common\ServicesBuilder;


U primjerima koji slijede, u `require_once` izjava uvijek će se prikazati, ali samo klasa potrebne navedeni primjer ispravno izvršiti referencirani.

## <a name="set-up-an-azure-storage-connection"></a>Postavljanje veze sustava Azure prostora za pohranu

Stvoriti instancu klijent za servis za blobova platforme Azure, najprije morate imati valjani niz za povezivanje. Oblik za niz za povezivanje servisa blob je:

Za pristup uživo servisa:

    DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]

Za pristup emulator prostora za pohranu:

    UseDevelopmentStorage=true


Da biste stvorili bilo kojeg Azure service klijenta, morate koristiti **ServicesBuilder** predmete. možeš:

* Prenesite niz za povezivanje izravno ga ili
* Korištenje **CloudConfigurationManager (CCM)** da biste provjerili više vanjskih izvora niz za povezivanje:
    * Prema zadanim postavkama dolazi s podrškom za jedan vanjski izvor - okolini varijabli.
    * Proširivanje klase **ConnectionStringSource** možete dodati novih izvora.

Primjer navedene u nastavku, niz za povezivanje će se proslijediti izravno.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;

    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);

## <a name="create-a-container"></a>Stvaranje spremnika

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

**BlobRestProxy** objekt omogućuje stvaranje spremnika blob metodom **createContainer** . Prilikom stvaranja spremniku, možete postaviti mogućnosti na spremnik, ali to tako da nije obavezno. (Primjeru u nastavku prikazuje kako postaviti spremnik popis za kontrolu pristupa (ACL) i metapodacima kontejner.)

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Blob\Models\CreateContainerOptions;
    use MicrosoftAzure\Storage\Blob\Models\PublicAccessType;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    // OPTIONAL: Set public access policy and metadata.
    // Create container options object.
    $createContainerOptions = new CreateContainerOptions();

    // Set public access policy. Possible values are
    // PublicAccessType::CONTAINER_AND_BLOBS and PublicAccessType::BLOBS_ONLY.
    // CONTAINER_AND_BLOBS:
    // Specifies full public read access for container and blob data.
    // proxys can enumerate blobs within the container via anonymous
    // request, but cannot enumerate containers within the storage account.
    //
    // BLOBS_ONLY:
    // Specifies public read access for blobs. Blob data within this
    // container can be read via anonymous request, but container data is not
    // available. proxys cannot enumerate blobs within the container via
    // anonymous request.
    // If this value is not specified in the request, container data is
    // private to the account owner.
    $createContainerOptions->setPublicAccess(PublicAccessType::CONTAINER_AND_BLOBS);

    // Set container metadata.
    $createContainerOptions->addMetaData("key1", "value1");
    $createContainerOptions->addMetaData("key2", "value2");

    try {
        // Create container.
        $blobRestProxy->createContainer("mycontainer", $createContainerOptions);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Pozivanje **setPublicAccess (PublicAccessType::CONTAINER\_i\_blob-ova)** podataka kontejner i blob olakšava pristupačnost putem anonimne zahtjeva. Pozivanje **setPublicAccess(PublicAccessType::BLOBS_ONLY)** postaje samo blob podatke dostupne putem anonimne zahtjeva za. Dodatne informacije o kontejneru ACL-a potražite u članku [Postavljanje spremnik ACL (REST API -JA)][container-acl].

Dodatne informacije o kodovima pogrešaka za servis Blob potražite u članku [Kodovi pogreške servisa Blob][error-codes].

## <a name="upload-a-blob-into-a-container"></a>Prijenos blob u spremniku

Da biste prenijeli datoteke u obliku blob, koristite metodu **BlobRestProxy -> createBlockBlob** . Ovaj postupak stvara blob-om ako ga ne postoji ili prebrisuje ako postoji. Kod primjeru u nastavku pretpostavlja da kontejner je već stvoren i koristi [fopen] [ fopen] da biste otvorili datoteku kao tok.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    $content = fopen("c:\myfile.txt", "r");
    $blob_name = "myblob";

    try {
        //Upload blob
        $blobRestProxy->createBlockBlob("mycontainer", $blob_name, $content);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Imajte na umu da prethodni uzorak prenosi blob kao tok. Međutim, blob možete i moguće prenijeti kao niz uz pomoć, na primjer, u [datoteke\_dobiti\_sadržaj] [ file_get_contents] (opis funkcije). To možete učiniti pomoću prethodni uzorak promjena `$content = fopen("c:\myfile.txt", "r");` da biste `$content = file_get_contents("c:\myfile.txt");`.

## <a name="list-the-blobs-in-a-container"></a>Popis blob-ova u spremniku

Da biste dobili popis blob-ova u spremniku koristiti metodu **BlobRestProxy -> listBlobs** s na **foreach** petlje za ponavljanje prijelaza rezultat. Sljedeći kod prikazuje naziv svaki blob kao rezultat u spremniku i prikazuje njegov URI u preglednik.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    try {
        // List blobs.
        $blob_list = $blobRestProxy->listBlobs("mycontainer");
        $blobs = $blob_list->getBlobs();

        foreach($blobs as $blob)
        {
            echo $blob->getName().": ".$blob->getUrl()."<br />";
        }
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }


## <a name="download-a-blob"></a>Preuzimanje blob

Da biste preuzeli blob, pozvati metodu **BlobRestProxy -> getBlob** , a zatim pozivanje metodu **getContentStream** na rezultirajućem **GetBlobResult** objektu.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    try {
        // Get blob.
        $blob = $blobRestProxy->getBlob("mycontainer", "myblob");
        fpassthru($blob->getContentStream());
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Imajte na umu da gornji primjer dobiva blob kao resurs strujanje (zadano ponašanje). Međutim, možete koristiti u [strujanje\_dobiti\_sadržaj] [ stream-get-contents] funkcija da biste pretvorili toka vraćeni niz.

## <a name="delete-a-blob"></a>Brisanje blob

Da biste izbrisali blob, prenesite spremnik ime i naziv blob i **BlobRestProxy -> deleteBlob**.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    try {
        // Delete blob.
        $blobRestProxy->deleteBlob("mycontainer", "myblob");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## <a name="delete-a-blob-container"></a>Izbrišite spremnik blobova platforme

Na kraju, da biste izbrisali spremniku blob, prenesite naziv spremnika **BlobRestProxy -> deleteContainer**.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    try {
        // Delete container.
        $blobRestProxy->deleteContainer("mycontainer");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste naučili osnove servisa blobova platforme Azure, slijedite ove veze dodatne informacije o složenije zadatke za pohranu.

- Posjetite [blog tima za pohranu za Azure](http://blogs.msdn.com/b/windowsazurestorage/)
- Pogledajte [i blokiranje blob primjer](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/BlockBlobExample.php).
- Pogledajte [primjer blobova platforme za i stranice](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/PageBlobExample.php).
- [Prijenos podataka s AzCopy naredbenog retka uslužni](storage-use-azcopy.md)
 
Dodatne informacije potražite u odjeljku [Centar za razvojne inženjere i](/develop/php/).


[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[container-acl]: http://msdn.microsoft.com/library/azure/dd179391.aspx
[error-codes]: http://msdn.microsoft.com/library/azure/dd179439.aspx
[file_get_contents]: http://php.net/file_get_contents
[require_once]: http://php.net/require_once
[fopen]: http://www.php.net/fopen
[stream-get-contents]: http://www.php.net/stream_get_contents
