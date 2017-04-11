<properties
    pageTitle="Kako koristiti Azure blobova iz iOS | Microsoft Azure"
    description="Pohranite nestrukturirane podatke u oblaku s Azure blobova (spremište objekta)."
    services="storage"
    documentationCenter="ios"
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="micurd"/>

# <a name="how-to-use-blob-storage-from-ios"></a>Upute za korištenje spremišta blobova s operacijskim sustavom iOS

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Pregled

U ovom se članku vidjet ćete kako izvršiti uobičajeni scenariji pomoću spremište blobova platforme Microsoft Azure. Primjere zapisuju u cilj C i koristite [Azure prostora za pohranu klijenta biblioteke za iOS](https://github.com/Azure/azure-storage-ios). Scenariji u kojima je moguće uvrstiti **Prijenos**, **popis**, **Preuzimanje**i **Brisanje** blob-ova. Dodatne informacije o blob polja potražite u odjeljku [Sljedeće korake](#next-steps) . Možete preuzeti i [ogledne aplikacije](https://github.com/Azure/azure-storage-ios/tree/master/BlobSample) da biste brzo vidjeli korištenje Azure prostora za pohranu u aplikaciji sustava iOS.

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="import-the-azure-storage-ios-library-into-your-application"></a>Uvoz iOS biblioteka za pohranu Azure u aplikaciji

IOS biblioteka za pohranu Azure možete uvesti u svoju aplikaciju pomoću [CocoaPod Azure prostora za pohranu](https://cocoapods.org/pods/AZSClient) ili tako da uvezete datoteku **Framework** .

## <a name="cocoapod"></a>CocoaPod

1. Ako to još niste učinili, [Instalirajte CocoaPods](https://guides.cocoapods.org/using/getting-started.html#toc_3) na računalu tako da otvorite terminal prozor i pokretanjem sljedeće naredbe

        sudo gem install cocoapods

2. Sljedeći u imeniku projekta (direktorija koja sadrži vaše `.xcodeproj` datoteka), stvorite novu datoteku pod nazivom `Podfile`(nema datotečni nastavak). Dodajte na sljedeći način `Podfile` i spremanje

        pod 'AZSClient'

3. U prozoru terminal idite do projekta i pokrenite sljedeću naredbu

        pod install

4. Ako vaš `.xcodeproj` je otvorite u Xcode, zatvorite je. U direktoriju projekta otvoriti datoteku novostvorenu projekta koji će imati na `.xcworkspace` nastavak. Ovo je datoteka za koju ćete radite s sada na.

## <a name="framework"></a>Framework
Da biste koristili biblioteku iOS Azure prostora za pohranu, najprije morate za izradu datoteka framework.

1. Najprije preuzmite ili Kloniraj [repo azure, za pohranu i ios](https://github.com/azure/azure-storage-ios).

2. Idite u *azure, za pohranu i ios* -> *Biblioteka* -> *Azure prostora za pohranu klijenta biblioteke*, a zatim otvorite `AZSClient.xcodeproj` u Xcode.

3. U gornjem lijevom kutu Xcode, promijenite aktivni shemu iz "Azure prostora za pohranu klijenta biblioteke" "Framework".

4. Stvaranje projekta (⌘ + B). Time ćete stvoriti na `AZSClient.framework` datoteku na radnu površinu.

Zatim možete uvesti framework datoteku u aplikacija tako da učinite sljedeće:

1. Stvorite novi projekt ili otvorite postojeći projekt Xcode.

2. Kliknite na projektu u lijevom navigacijskom oknu, a zatim kliknite karticu *Općenito* pri vrhu uređivača projekta.

3. U odjeljku *povezani okviri i biblioteke* kliknite gumb Dodaj (+).

4. Kliknite *Dodaj druge...*. Pronađite i dodajte u `AZSClient.framework` datoteku koju ste upravo stvorili.

5. U odjeljku *povezani okviri i biblioteke* kliknite gumb Dodaj (+).

6. Na popisu biblioteka već dobili traženje `libxml2.2.dylib` i dodajte ga u projekt.

7. Kliknite karticu *Postavke sastavljanje* pri vrhu uređivača projekta.

8. U odjeljku *Putovi pretraživanja* dvokliknite *Putova Framework pretraživanja* i dodajte put do vaše `AZSClient.framework` datoteku.

## <a name="import-statement"></a>Naredba Uvoz
Morat ćete obuhvaćaju sljedeće naredbe uvoz u datoteci koju želite pozvati API-JA za pohranu Azure.

    // Include the following import statement to use blob APIs.
    #import <AZSClient/AZSClient.h>

[AZURE.INCLUDE [storage-mobile-authentication-guidance](../../includes/storage-mobile-authentication-guidance.md)]

## <a name="asynchronous-operations"></a>Asinkronog operacije
> [AZURE.NOTE] Sve metode koje obavljaju zahtjev za uslugu su asinkronog operacije. U primjere koda nalaze se da imaju ovih metoda dovršetka događajima. Kod unutar rukovatelj dovršetka pokrenut će se **nakon** dovršetka zahtjev. Kod nakon dovršetka rukovatelj će se pokrenuti **dok** zahtjev postala je u tijeku.

## <a name="create-a-container"></a>Stvaranje spremnika
Svaki blobova platforme Azure pohrane u moraju se nalaziti u spremniku. Sljedeći primjer pokazuje kako stvoriti spremnika, koji se naziva *newcontainer*na vašem računu za pohranu Ako još ne postoji. Kada odaberete naziv vaše spremnik biti mindful pravila imenovanja prethodno navedenim.

    -(void)createContainer{
      NSError *accountCreationError;

      // Create a storage account object from a connection string.
      AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

      if(accountCreationError){
         NSLog(@"Error in creating account.");
      }

      // Create a blob service client object.
      AZSCloudBlobClient *blobClient = [account getBlobClient];

      // Create a local container object.
      AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"newcontainer"];

      // Create container in your Storage account if the container doesn't already exist
      [blobContainer createContainerIfNotExistsWithCompletionHandler:^(NSError *error, BOOL exists) {
         if (error){
             NSLog(@"Error in creating container.");
         }
      }];
    }

Možete potvrditi da se to ne funkcionira, pogled na [Microsoft Azure prostora za pohranu Explorer](http://storageexplorer.com) i provjera *newcontainer* je na popisu spremnika za vaš račun za pohranu.

## <a name="set-container-permissions"></a>Postavljanje dozvola za spremnik
Dozvole u spremniku konfigurirana je za **privatne** access prema zadanim postavkama. No spremnika nude nekoliko različitih mogućnosti za pristup spremnik:

- **Privatno**: spremnik i blob podataka mogu čitati samo vlasnik računa.

- **Blob**: Blob podataka u ovom spremniku je moguće čitati putem anonimni zahtjev, ali spremnik podataka nije dostupna. Klijenti nije moguće numerirati blob polja unutar kontejnera putem anonimni zahtjev.

- **Spremnik**: spremnik i blob podataka mogu čitati putem anonimni zahtjev. Klijenti možete numerirati blob polja unutar kontejnera putem anonimni zahtjev, ali nije moguće numerirati spremnika subjekta prostora za pohranu.

Sljedeći primjer pokazuje kako stvoriti kontejnera s dozvolama za pristup **spremnik** koji omogućuje pristup javnim, samo za čitanje za sve korisnike na Internetu:

    -(void)createContainerWithPublicAccess{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        // Create container in your Storage account if the container doesn't already exist
        [blobContainer createContainerIfNotExistsWithAccessType:AZSContainerPublicAccessTypeContainer requestOptions:nil operationContext:nil completionHandler:^(NSError *error, BOOL exists){
            if (error){
                NSLog(@"Error in creating container.");
            }
        }];
    }

## <a name="upload-a-blob-into-a-container"></a>Prijenos blob u spremniku
Kao što je rečeno u odjeljku [Blob servisa koncepata](#blob-service-concepts) blobova nudi tri različite vrste blob-ova: blokirati blob-ova, dodavanje blob-ova i stranica blob-ova. U ovom trenutku iOS biblioteka za pohranu Azure podržava samo bloka blob-ova. U većini slučajeva, blok blob je preporučena vrsta da biste koristili.

Sljedeći primjer pokazuje kako prenijeti blob bloka iz programa NSString. Ako blob s istim nazivom već postoji u ovom spremniku, prebrisat će se sadržaj ovog blob.

    -(void)uploadBlobToContainer{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        [blobContainer createContainerIfNotExistsWithAccessType:AZSContainerPublicAccessTypeContainer requestOptions:nil operationContext:nil completionHandler:^(NSError *error, BOOL exists)
         {
             if (error){
                 NSLog(@"Error in creating container.");
             }
             else{
                 // Create a local blob object
                 AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob"];

                 // Upload blob to Storage
                 [blockBlob uploadFromText:@"This text will be uploaded to Blob Storage." completionHandler:^(NSError *error) {
                     if (error){
                         NSLog(@"Error in creating blob.");
                     }
                 }];
             }
         }];
    }

Možete potvrditi da se to ne funkcionira, pogled na [Microsoft Azure prostora za pohranu Explorer](http://storageexplorer.com) i provjera sadrži li spremnik *containerpublic*blob-om, *sampleblob*. U ovom se primjeru koristi javno spremnik tako da možete provjeriti i to uspješnog tako da URI blob-ova:

    https://nameofyourstorageaccount.blob.core.windows.net/containerpublic/sampleblob

Osim prijenos blob bloka iz programa NSString slične metode postoji NSData, NSInputStream ili lokalna datoteka.

## <a name="list-the-blobs-in-a-container"></a>Popis blob-ova u spremniku
Sljedeći primjer prikazuje način da biste dobili popis svih blob-ova u spremniku. Prilikom izvršavanja ovaj postupak, biti mindful od sljedećih parametara:     

- **continuationToken** - token nastavka predstavlja gdje bi trebao počinjati postupak za stavku. Ako je ne tokena koje se nude, ona će se popis blob-ova od početka. Bilo koji broj blob-ova može biti navedena od nule do maksimalno skupu. Čak i ako se ta metoda vraća nula rezultata ako `results.continuationToken` nije ništicu, mogu postojati i dodatni blob polja na servis koji nije naveden.
- **prefiks** - možete odrediti prefiks koji će se koristiti za unos blob. Samo blob-ova koji počinju ovaj prefiks će se prikazati.
- **useFlatBlobListing** – kao što je rečeno u odjeljku [imenovanje i referentni spremnika i blob-ova](#naming-and-referencing-containers-and-blobs) , premda je servis Blob shemu paušalni prostora za pohranu možete stvoriti hijerarhiju virtualne imenovanje blob polja s informacije o putu. Međutim, nije ravnomjerne unos trenutno nije podržano; To je uskoro dostupno. Zasad mora biti tu vrijednost`YES`
- **blobListingDetails** – možete odrediti stavke koje želite uključiti prilikom unosa blob-ova
    - `AZSBlobListingDetailsNone`: Popis samo izvršenja blob-ova i vratite blob metapodataka.
    - `AZSBlobListingDetailsSnapshots`: Popis predavanja blob-ova i blob snimke.
    - `AZSBlobListingDetailsMetadata`: Dohvaćanje blob metapodataka za svaku blob vraćaju se na popisu.
    - `AZSBlobListingDetailsUncommittedBlobs`: Popis zajamčenoj i nepotvrđenim blob-ova.
    - `AZSBlobListingDetailsCopy`: Sadržavati svojstva Kopiraj na popisu.
    - `AZSBlobListingDetailsAll`: Popis dostupnih izvršenja blob-ova, nepotvrđenim blob-ova i snimke i vratite sve status polja metapodataka i kopija te blob-ova.
- **maxResults** - maksimalni broj rezultata da biste se vratili za ovaj postupak. Da biste postavili granicu koristite -1.
- **completionHandler** - bloka kod za izvršavanje s rezultatima postupak za stavku.

U ovom primjeru se koristi za način helper rekurzivno poziva na popisu blob-Ova metoda svaki put nastavka tokena, vraća se.

    -(void)listBlobsInContainer{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        //List all blobs in container
        [self listBlobsInContainerHelper:blobContainer continuationToken:nil prefix:nil blobListingDetails:AZSBlobListingDetailsAll maxResults:-1 completionHandler:^(NSError *error) {
            if (error != nil){
                NSLog(@"Error in creating container.");
            }
        }];
    }

    //List blobs helper method
    -(void)listBlobsInContainerHelper:(AZSCloudBlobContainer *)container continuationToken:(AZSContinuationToken *)continuationToken prefix:(NSString *)prefix blobListingDetails:(AZSBlobListingDetails)blobListingDetails maxResults:(NSUInteger)maxResults completionHandler:(void (^)(NSError *))completionHandler
    {
        [container listBlobsSegmentedWithContinuationToken:continuationToken prefix:prefix useFlatBlobListing:YES blobListingDetails:blobListingDetails maxResults:maxResults completionHandler:^(NSError *error, AZSBlobResultSegment *results) {
            if (error)
            {
                completionHandler(error);
            }
            else
            {
                for (int i = 0; i < results.blobs.count; i++) {
                    NSLog(@"%@",[(AZSCloudBlockBlob *)results.blobs[i] blobName]);
                }
                if (results.continuationToken)
                {
                    [self listBlobsInContainerHelper:container continuationToken:results.continuationToken prefix:prefix blobListingDetails:blobListingDetails maxResults:maxResults completionHandler:completionHandler];
                }
                else
                {
                    completionHandler(nil);
                }
            }
        }];
    }


## <a name="download-a-blob"></a>Preuzimanje blob

Sljedeći primjer pokazuje kako preuzeti blob NSString objekt.

    -(void)downloadBlobToString{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        // Create a local blob object
        AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob"];

        // Download blob
        [blockBlob downloadToTextWithCompletionHandler:^(NSError *error, NSString *text) {
            if (error) {
                NSLog(@"Error in downloading blob");
            }
            else{
                NSLog(@"%@",text);
            }
        }];
    }

## <a name="delete-a-blob"></a>Brisanje blob

Sljedeći primjer prikazuje način da biste izbrisali blob.

    -(void)deleteBlob{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        // Create a local blob object
        AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob1"];

        // Delete blob
        [blockBlob deleteWithCompletionHandler:^(NSError *error) {
            if (error) {
                NSLog(@"Error in deleting blob.");
            }
        }];
    }

## <a name="delete-a-blob-container"></a>Izbrišite spremnik blobova platforme

Sljedeći primjer prikazuje način da biste izbrisali spremnik.

    -(void)deleteContainer{
      NSError *accountCreationError;

      // Create a storage account object from a connection string.
      AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

      if(accountCreationError){
         NSLog(@"Error in creating account.");
      }

      // Create a blob service client object.
      AZSCloudBlobClient *blobClient = [account getBlobClient];

      // Create a local container object.
      AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

      // Delete container
      [blobContainer deleteContainerIfExistsWithCompletionHandler:^(NSError *error, BOOL success) {
         if(error){
             NSLog(@"Error in deleting container");
         }
      }];
    }

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste naučili kako koristiti blobova s operacijskim sustavom iOS, slijedite ove veze da biste saznali više o biblioteci iOS i servis za pohranu.

- [Azure biblioteka za pohranu klijenta za iOS](https://github.com/azure/azure-storage-ios)
- [Azure prostora za pohranu iOS referentnu dokumentaciju](http://azure.github.io/azure-storage-ios/)
- [Servise za pohranu Azure REST API-JA](https://msdn.microsoft.com/library/azure/dd179355.aspx)
- [Blog tima za Azure prostora za pohranu](http://blogs.msdn.com/b/windowsazurestorage)

Ako imate pitanja vezana uz tu biblioteku slobodno objaviti na našem [forum MSDN Azure](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=windowsazuredata) ili [Prelijevanje stogu](http://stackoverflow.com/questions/tagged/windows-azure-storage+or+windows-azure-storage+or+azure-storage-blobs+or+azure-storage-tables+or+azure-table-storage+or+windows-azure-queues+or+azure-storage-queues+or+azure-storage-emulator+or+azure-storage-files).
Ako imate značajka prijedloge za pohranu Azure, ponovno objaviti na [Povratnih informacija za pohranu Azure](https://feedback.azure.com/forums/217298-storage/).
