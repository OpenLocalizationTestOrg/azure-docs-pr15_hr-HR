<properties 
    pageTitle="Prijenos datoteka na račun servisa Media Services pomoću .NET | Microsoft Azure" 
    description="Saznajte kako pronaći i medijskih sadržaja u Media Services stvaranju prenošenje imovine." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016" 
    ms.author="juliako"/>



# <a name="upload-files-into-a-media-services-account-using-net"></a>Prijenos datoteka na račun servisa Media Services pomoću .NET

 > [AZURE.SELECTOR]
 - [.NET](media-services-dotnet-upload-files.md)
 - [OSTALE](media-services-rest-upload-files.md)
 - [Portal](media-services-portal-upload-files.md)

Media Services koje prijenos (ili ingest) digitalni datoteke u sredstava. Entitet **resursa** može sadržavati videozapis, zvuk, slike, minijatura zbirke, tekst zapise i titlova datoteke (i metapodataka o tim datotekama.)  Nakon prijenosa datoteka sadržaj pohranjen sigurno u oblaku za daljnje obrade i strujanje.

Datoteke imovine nazivaju **Datoteke resursa**. **AssetFile** instance i stvarne medijske datoteke su dva različita objekta. AssetFile instance sadrži metapodatke o medijske datoteke dok medijske datoteke sadrži stvarni medijskog sadržaja.

>[AZURE.NOTE]Kada odaberete naziva datoteke programa resursa, primjenjuju se Imajte na umu sljedeće:
>
>- Media Services koristi vrijednost svojstva IAssetFile.Name kada sastavljanje URL-ovi za strujanje sadržaj (na primjer, http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Zbog toga šifriranje u obliku postotaka nije dopušteno. Svojstvo **ime** ne smije sadržavati sljedeće [znakove postotak kodiranje-rezervirana](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]". Osim toga, može postojati samo jedan '. "za nastavak.
>
>- Duljina naziva ne smije biti veća od 260 znakova.

Kada stvorite resursi, možete odrediti sljedeće mogućnosti za šifriranje. 

- Koristi se **ništa** – nema šifriranja. To je zadana vrijednost. Imajte na umu da koristite tu mogućnost sadržaja nije zaštićena na putu ili na ostale u prostor za pohranu.
Ako namjeravate održati programa MP4 korištenje progresivno preuzimanje koristiti tu mogućnost. 
- **CommonEncryption** - koristite ovu mogućnost ako prenosite sadržaj koji je već šifrirane i zaštićen uobičajenih šifriranje ili PlayReady DRM (na primjer, izglađenim strujanje zaštićeni PlayReady DRM).
- **EnvelopeEncrypted** – pomoću te mogućnosti prenosite HLS šifrirane i zaštićene AES. Imajte na umu da datoteke mora su kodirani i šifrirane Upravitelj pretvoriti.
- **StorageEncrypted** - šifrira lokalno putem AES 256 bita šifriranja, a zatim prijenosi je prostora za pohranu Azure na kojem je pohranjena šifrirane na ostale Očisti sadržaj. Resursi koji su zaštićeni šifriranjem prostora za pohranu se automatski nešifrirani smjestiti u sustavu šifriranu datoteku prije no što kodiranja i po želji se ponovno šifrirane prije no što prenesete natrag kao novi izlaz resursa. Slučaj prvenstveno se koristi za pohranu šifriranje je kada želite sigurne visoke kvalitete unos medijske datoteke s šifriranjem na ostale na disku.

    Media Services omogućuje šifriranje prostora za pohranu na disku za imovinu, ne postavite pokazivač-na-žičani kao što je digitalnim pravima Manager (DRM).

    Ako je vaš resursa za pohranu šifrirane, morate konfigurirati pravila za isporuku resursa. Dodatne informacije potražite u članku [Konfiguriranje resursa isporuke pravila](media-services-dotnet-configure-asset-delivery-policy.md).

Ako navedete za vaše resursa šifriranje s **CommonEncrypted** mogućnost ili mogućnost **EnvelopeEncypted** , morat ćete pridružiti vaše resursa **ContentKey**. Dodatne informacije potražite u članku [kako stvoriti na ContentKey](media-services-dotnet-create-contentkey.md). 

Ako navedete za vaše resursa šifriranje mogućnošću **StorageEncrypted** Media Services SDK za .NET će stvoriti **StorateEncrypted** **ContentKey** za vaše resursa.


U ovoj se temi objašnjava koristiti Media Services .NET SDK, kao i Media Services .NET SDK proširenja za prijenos datoteka u Media Services resursa.

 
## <a name="upload-a-single-file-with-media-services-net-sdk"></a>Prijenos jedne datoteke s Media Services .NET SDK 

Uzorak koda koristi .NET SDK za izvođenje sljedećih zadataka: 

- Stvara praznu resursa.
- Stvara instancu AssetFile koje želite pridružiti imovine.
- Stvara instancu AccessPolicy koji definira dozvole i trajanje pristupa sredstava.
- Stvara instancu Locator koja omogućuje pristup sredstava.
- Prenosite jednu medijsku datoteku u Media Services. 

        
        static public IAsset CreateAssetAndUploadSingleFile(AssetCreationOptions assetCreationOptions, string singleFilePath)
        {
            if (!File.Exists(singleFilePath))
            {
                Console.WriteLine("File does not exist.");
                return null;
            }

            var assetName = Path.GetFileNameWithoutExtension(singleFilePath);
            IAsset inputAsset = _context.Assets.Create(assetName, assetCreationOptions); 

            var assetFile = inputAsset.AssetFiles.Create(Path.GetFileName(singleFilePath));

            Console.WriteLine("Created assetFile {0}", assetFile.Name);

            var policy = _context.AccessPolicies.Create(
                                    assetName,
                                    TimeSpan.FromDays(30),
                                    AccessPermissions.Write | AccessPermissions.List);

            var locator = _context.Locators.CreateLocator(LocatorType.Sas, inputAsset, policy);

            Console.WriteLine("Upload {0}", assetFile.Name);

            assetFile.Upload(singleFilePath);
            Console.WriteLine("Done uploading {0}", assetFile.Name);

            locator.Delete();
            policy.Delete();

            return inputAsset;
        }

##<a name="upload-multiple-files-with-media-services-net-sdk"></a>Prijenos više datoteka s Media Services .NET SDK 

Sljedeći kod prikazuje kako stvoriti sredstvo i prijenos više datoteka.

Kod čini sljedeće:
    
-   Stvara sredstvo prazan metodom CreateEmptyAsset definirano u prethodnom koraku.
    
-   Stvara instancu **AccessPolicy** koji definira dozvole i trajanje pristupa sredstava.
    
-   Stvara instancu **Locator** koja omogućuje pristup sredstava.
    
-   Stvara instancu komponente **BlobTransferClient** . Ta vrsta predstavlja klijent koje se primjenjuju na Azure blob-ova. U ovom primjeru koristimo klijent za praćenje napretka prijenos. 
    
-   Zbraja kroz datoteke u navedeni direktorij i stvara instancu **AssetFile** za svaku datoteku.
    
-   Prenosite datoteke u Media Services metodom **UploadAsync** . 
    
>[AZURE.NOTE] Upotrijebite metodu UploadAsync pozivi su blokiranje i datoteke prenose paralelno.
    
    
        static public IAsset CreateAssetAndUploadMultipleFiles(AssetCreationOptions assetCreationOptions, string folderPath)
        {
            var assetName = "UploadMultipleFiles_" + DateTime.UtcNow.ToString();

            IAsset asset = _context.Assets.Create(assetName, assetCreationOptions);

            var accessPolicy = _context.AccessPolicies.Create(assetName, TimeSpan.FromDays(30),
                                                                AccessPermissions.Write | AccessPermissions.List);

            var locator = _context.Locators.CreateLocator(LocatorType.Sas, asset, accessPolicy);

            var blobTransferClient = new BlobTransferClient();
            blobTransferClient.NumberOfConcurrentTransfers = 20;
            blobTransferClient.ParallelTransferThreadCount = 20;

            blobTransferClient.TransferProgressChanged += blobTransferClient_TransferProgressChanged;

            var filePaths = Directory.EnumerateFiles(folderPath);

            Console.WriteLine("There are {0} files in {1}", filePaths.Count(), folderPath);

            if (!filePaths.Any())
            {
                throw new FileNotFoundException(String.Format("No files in directory, check folderPath: {0}", folderPath));
            }

            var uploadTasks = new List<Task>();
            foreach (var filePath in filePaths)
            {
                var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
                Console.WriteLine("Created assetFile {0}", assetFile.Name);

                // It is recommended to validate AccestFiles before upload. 
                Console.WriteLine("Start uploading of {0}", assetFile.Name);
                uploadTasks.Add(assetFile.UploadAsync(filePath, blobTransferClient, locator, CancellationToken.None));
            }

            Task.WaitAll(uploadTasks.ToArray());
            Console.WriteLine("Done uploading the files");

            blobTransferClient.TransferProgressChanged -= blobTransferClient_TransferProgressChanged;

            locator.Delete();
            accessPolicy.Delete();

            return asset;
        }
    
    static void  blobTransferClient_TransferProgressChanged(object sender, BlobTransferProgressChangedEventArgs e)
    {
        if (e.ProgressPercentage > 4) // Avoid startup jitter, as the upload tasks are added.
        {
            Console.WriteLine("{0}% upload competed for {1}.", e.ProgressPercentage, e.LocalFile);
        }
    }



Kada prenesete velikog broja resursi, imajte na umu sljedeće.

- Stvorite novi objekt **CloudMediaContext** po niti. Klase **CloudMediaContext** nije niti sigurni.
 
- Smanjivanje NumberOfConcurrentTransfers zadanu vrijednost od 2 veću vrijednost kao što je 5. Ovo svojstvo utječe na sve pojave **CloudMediaContext**. 
 
- Zadrži ParallelTransferThreadCount na zadanu vrijednost od 10.
 
##<a id="ingest_in_bulk"></a>Ingesting imovine masovno pomoću Media Services .NET SDK 

Prijenos velikih resursa datoteka može biti usko grlo tijekom stvaranja resursa. Ingesting resursima u e-pošte ili "Masovno Ingesting" uključuje decoupling stvaranja resursa iz postupka prijenos. Da biste koristili skupno ingesting pristup, stvorite manifest (IngestManifest) koji opisuje imovina i njezine povezane datoteke. Zatim upotrijebite metodu prijenos po izboru da biste prenijeli pridružene datoteke na manifest blob spremniku. Microsoft Azure Media Services nadzirane spremnik blob pridružene manifest. Nakon prijenosa datoteke spremniku blobova platforme Microsoft Azure Media Services dovršava stvaranje resursa ovisno o konfiguraciji resursa u manifestu (IngestManifestAsset).


Da biste stvorili novi poziv IngestManifest metodu stvaranje koji prikazuje zbirke IngestManifests na na CloudMediaContext. Ovaj postupak će stvoriti novi IngestManifest nazivom manifesta pružate.

    IIngestManifest manifest = context.IngestManifests.Create(name);

Stvaranje sredstva koja će biti povezan s skupno IngestManifest. Konfiguriranje mogućnosti željeni šifriranja na resursa za skupno ingesting.

    // Create the assets that will be associated with this bulk ingest manifest
    IAsset destAsset1 = _context.Assets.Create(name + "_asset_1", AssetCreationOptions.None);
    IAsset destAsset2 = _context.Assets.Create(name + "_asset_2", AssetCreationOptions.None);

Na IngestManifestAsset sredstvo pridružuje skupno IngestManifest za skupno ingesting. Također se povezuje AssetFiles da bi se svaki resursa. Da biste stvorili programa IngestManifestAsset, pomoću načina za stvaranje na poslužitelju kontekstu.

Sljedeći primjer prikazuje dodavanjem IngestManifestAssets dva nova koja će povezati dva imovine koje ste prethodno stvorili skupno ingest manifest. Svaki IngestManifestAsset povezuje i skup datoteke koje će biti prenesene za svaku imovinu tijekom ingesting skupno.  

    string filename1 = _singleInputMp4Path;
    string filename2 = _primaryFilePath;
    string filename3 = _singleInputFilePath;
    
    IIngestManifestAsset bulkAsset1 =  manifest.IngestManifestAssets.Create(destAsset1, new[] { filename1 });
    IIngestManifestAsset bulkAsset2 =  manifest.IngestManifestAssets.Create(destAsset2, new[] { filename2, filename3 });
    
Možete koristiti bilo koji velika brzina klijentska aplikacija koje je moguće povezati s prijenosom datoteka resursa za pohranu spremniku blob URI nudi svojstvo **IIngestManifest.BlobStorageUriForUpload** na IngestManifest. Jedan najvažnije velika brzina prijenos servis je [Aspera na zahtjev za Azure aplikaciju](https://datamarket.azure.com/application/2cdbc511-cb12-4715-9871-c7e7fbbb82a6). Možete upisati i kod da biste prenijeli datoteke imovine kao što je prikazano u sljedećem primjeru kod.
    
    static void UploadBlobFile(string destBlobURI, string filename)
    {
        Task copytask = new Task(() =>
        {
            var storageaccount = new CloudStorageAccount(new StorageCredentials(_storageAccountName, _storageAccountKey), true);
            CloudBlobClient blobClient = storageaccount.CreateCloudBlobClient();
            CloudBlobContainer blobContainer = blobClient.GetContainerReference(destBlobURI);
    
            string[] splitfilename = filename.Split('\\');
            var blob = blobContainer.GetBlockBlobReference(splitfilename[splitfilename.Length - 1]);
    
            using (var stream = System.IO.File.OpenRead(filename))
                blob.UploadFromStream(stream);
    
            lock (consoleWriteLock)
            {
                Console.WriteLine("Upload for {0} completed.", filename);
            }
        });
    
        copytask.Start();
    }

U sljedećem primjeru kod prikazan je kod za prijenos datoteka resursa za uzorak koji se koristi u ovoj temi.
    
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename1);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename2);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename3);
    

Možete odrediti tijek skupno ingesting za sva sredstva pridruženih **IngestManifest** tako provjere svojstvo Statistika **IngestManifest**. Da biste ažurirali informacije o tijeku, morate koristiti novi **CloudMediaContext** svaki put ankete svojstvo Statistika.

Sljedeći primjer prikazuje provjere programa IngestManifest po **ID-a**.
    
    static void MonitorBulkManifest(string manifestID)
    {
       bool bContinue = true;
       while (bContinue)
       {
          CloudMediaContext context = GetContext();
          IIngestManifest manifest = context.IngestManifests.Where(m => m.Id == manifestID).FirstOrDefault();
    
          if (manifest != null)
          {
             lock(consoleWriteLock)
             {
                Console.WriteLine("\nWaiting on all file uploads.");
                Console.WriteLine("PendingFilesCount  : {0}", manifest.Statistics.PendingFilesCount);
                Console.WriteLine("FinishedFilesCount : {0}", manifest.Statistics.FinishedFilesCount);
                Console.WriteLine("{0}% complete.\n", (float)manifest.Statistics.FinishedFilesCount / (float)(manifest.Statistics.FinishedFilesCount + manifest.Statistics.PendingFilesCount) * 100);
    
                if (manifest.Statistics.PendingFilesCount == 0)
                {
                   Console.WriteLine("Completed\n");
                   bContinue = false;
                }
             }
    
             if (manifest.Statistics.FinishedFilesCount < manifest.Statistics.PendingFilesCount)
                Thread.Sleep(60000);
          }
          else // Manifest is null
             bContinue = false;
       }
    }
    


##<a name="upload-files-using-net-sdk-extensions"></a>Prijenos datoteka pomoću alata za .NET SDK proširenja 

U primjeru u nastavku prikazuje kako prenijeti u jednu datoteku pomoću .NET SDK proširenja. U ovom slučaju koristi metodu **CreateFromFile** , ali asinkronog verzija i nije dostupan (**CreateFromFileAsync**). Način **CreateFromFile** omogućuje vam da odredite naziv datoteke, mogućnost šifriranja i na povratnog da bi se izvješćivanje o tijeku prijenos datoteke.


    static public IAsset UploadFile(string fileName, AssetCreationOptions options)
    {
        IAsset inputAsset = _context.Assets.CreateFromFile(
            fileName,
            options,
            (af, p) =>
            {
                Console.WriteLine("Uploading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });
    
        Console.WriteLine("Asset {0} created.", inputAsset.Id);
    
        return inputAsset;
    }

U sljedećem primjeru poziva UploadFile (opis funkcije) i određuje prostora za pohranu šifriranja kao mogućnost za stvaranje resursa.  


    var asset = UploadFile(@"C:\VideoFiles\BigBuckBunny.mp4", AssetCreationOptions.StorageEncrypted);


##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="next-step"></a>Sljedeći korak

Sad kad ste prenijeli sredstvo Media Services, idite na [kako doći do procesor medijskih sadržaja][] teme.

[Kako nabaviti procesor medijske sadržaje]: media-services-get-media-processor.md
 
