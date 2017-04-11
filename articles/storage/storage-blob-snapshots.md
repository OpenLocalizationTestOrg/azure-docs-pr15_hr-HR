<properties
    pageTitle="Stvaranje snimke samo za čitanje blob | Microsoft Azure"
    description="Saznajte kako stvoriti snimku blob za sigurnosno kopiranje podataka blob u određenom trenutku u vremenu. Objašnjenje kako se naplaćuju snimke i kako ih koristiti da biste minimizirali troškove kapaciteta."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="create-a-blob-snapshot"></a>Stvaranje blob snimke

## <a name="overview"></a>Pregled

Snimka je verzija blob koji koristi se u trenutku u vremenu koja je samo za čitanje. Brze snimke korisne su za sigurnosno kopiranje blob-ova. Nakon stvaranja brze snimke, možete čitati, kopirati ili izbrisati, ali ne i mijenjati.

Snimak blob jednak njegov osnovni blob osim što blob URI ima vrijednost **datuma i vremena** dodaju se u blob URI da bi se označio trenutak snimljena snimke. Na primjer, ako stranicu bloba URI je `http://storagesample.core.blob.windows.net/mydrives/myvhd`, snimka URI slično je `http://storagesample.core.blob.windows.net/mydrives/myvhd?snapshot=2011-03-09T01:42:34.9360000Z`. 

> [AZURE.NOTE] Sve snimke zajedničko korištenje osnovni blob URI. Samo razliku između osnovni blob i snimka je dodane vrijednost **datuma i vremena** .

Blob može imati bilo koji broj snimke. Brze snimke zadržava sve dok se izričito brišu. Snimka ne outlive njegov osnovni blob. Možete numerirati snimaka povezan s osnovnim blob-om za praćenje vaš trenutni snimke.

Prilikom stvaranja snimke blob svojstva sustava s blob kopiraju se snimka sadrži jednake vrijednosti. Osnovni blob metapodataka i kopirat će se snimka, ako ne navedete zasebnom metapodataka za brzu snimku kad ga stvorite.

Bilo koji leases povezan s osnovnim blob ne utječe na snimke. Ne dobije Zakup na snimke stanja.

## <a name="create-a-snapshot"></a>Stvaranje snimke stanja

Sljedeći primjer koda prikazuje kako stvoriti snimku u .NET. U ovom se primjeru određuje zasebnom metapodataka za brzu snimku prilikom stvaranja.

    private static async Task CreateBlockBlobSnapshot(CloudBlobContainer container)
    {
        // Create a new block blob in the container.
        CloudBlockBlob baseBlob = container.GetBlockBlobReference("sample-base-blob.txt");

        // Add blob metadata.
        baseBlob.Metadata.Add("ApproxBlobCreatedDate", DateTime.UtcNow.ToString());

        try
        {
            // Upload the blob to create it, with its metadata.
            await baseBlob.UploadTextAsync(string.Format("Base blob: {0}", baseBlob.Uri.ToString()));

            // Sleep 5 seconds.
            System.Threading.Thread.Sleep(5000);

            // Create a snapshot of the base blob.
            // Specify metadata at the time that the snapshot is created to specify unique metadata for the snapshot.
            // If no metadata is specified when the snapshot is created, the base blob's metadata is copied to the snapshot.
            Dictionary<string, string> metadata = new Dictionary<string, string>();
            metadata.Add("ApproxSnapshotCreatedDate", DateTime.UtcNow.ToString());
            await baseBlob.CreateSnapshotAsync(metadata, null, null, null);
        }
        catch (StorageException e)
        {
            Console.WriteLine(e.Message);
            Console.ReadLine();
            throw;
        }
    }
 

## <a name="copy-snapshots"></a>Kopiranje snimke

Operacija kopiranja koje obuhvaćaju blob-ova i snimke pridržavajte se sljedećih pravila:

- Snimku možete kopirati preko njegov osnovni blob. Po prosljeđivanja snimke položaj osnovni blob-om, možete vratiti stariju verziju blob. Ostane brze snimke, ali logaritma blob prebrisat će snimanje kopiju snimku.

- Snimku možete kopirati u odredišnoj blob pod drugim nazivom. Rezultat blob odredište je snimanje blob i ne snimke.

- Kada se kopira izvor blob, snimka od blob izvora ne kopiraju odredište. Kada blob odredište prebrisat će kopiju, snimka povezan s izvornog blob odredište ostaju ostaje netaknuta.

- Kada stvorite snimku blob blok, popis s blob izvršenja blokova se kopira i da biste snimku. Bilo koji nepotvrđenim blokova se kopiraju.

## <a name="specify-an-access-condition"></a>Određivanje uvjeta programa access

Možete odrediti uvjet access tako da se snimka stvara se samo ako je uvjet zadovoljen. Da biste odredili uvjet pristup, koristite svojstvo **AccessCondition** . Ako se ispuni određeni uvjet, snimka se stvara i servis Blob vraća Šifra stanja HTTPStatusCode.PreconditionFailed.

## <a name="delete-snapshots"></a>Brisanje snimke

Nije moguće izbrisati blob s snimaka osim ako se brišu snimki. Brisanje snimke stanja pojedinačno ili navedite sve snimaka izbrišu nakon brisanja blob izvora. Ako pokušate izbrisati blob koji i dalje ima snimke, rezultat se pogreška.

Sljedeći primjer koda pokazuje kako izbrisati blob i njegov snimke u .NET, gdje `blockBlob` je varijable vrste **CloudBlockBlob**:

    await blockBlob.DeleteIfExistsAsync(DeleteSnapshotsOption.IncludeSnapshots, null, null, null);

## <a name="snapshots-with-azure-premium-storage"></a>Brze snimke s Azure Premium prostora za pohranu

Pomoću snimaka prati Premium pohrane tih pravila:

- Maksimalan broj snimaka po stranici blob na računu za pohranu premium iznosi 100. Ako je to ograničenje premaši, operacija Blob snimku prikazuje kod pogreške 409 (**SnapshotCountExceeded**).

- Snimak blob stranice može potrajati na računu za pohranu premium svakih 10 minuta. Ako se taj stopa prekorači, operacija Blob snimke vraća kod pogreške 409 (**SnaphotOperationRateExceeded**).

- Ne možete pozvati dobiti Blob snimke blob stranice na računu za pohranu premium za čitanje. Pozivanje dobiti Blob na brze snimke u račun za pohranu premium vraća kod pogreške 400 (**InvalidOperation**). Međutim, uputili poziv dobiti svojstva Blob i dobiti metapodataka Blob protiv brze snimke u račun za pohranu premium.

- Da biste pročitali snimku, možete koristiti operacija Blob Kopiraj da biste kopirali snimku drugi blob stranice na računu. Odredište blob za kopiranje mora imati snimka postojeće. Ako odredišna blob snimke, postupak Blob kopiju prikazuje kod pogreške 409 (**SnapshotsPresent**).

## <a name="return-the-absolute-uri-to-a-snapshot"></a>Vratite apsolutne URI snimke stanja

C# kod u primjeru stvara snimke stanja i piše out apsolutne URI za primarno mjesto.

    //Create the blob service client object.
    const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";

    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    //Get a reference to a container.
    CloudBlobContainer container = blobClient.GetContainerReference("sample-container");
    container.CreateIfNotExists();

    //Get a reference to a blob.
    CloudBlockBlob blob = container.GetBlockBlobReference("sampleblob.txt");
    blob.UploadText("This is a blob.");

    //Create a snapshot of the blob and write out its primary URI.
    CloudBlockBlob blobSnapshot = blob.CreateSnapshot();
    Console.WriteLine(blobSnapshot.SnapshotQualifiedStorageUri.PrimaryUri);

## <a name="understand-how-snapshots-accrue-charges"></a>Objašnjenje kako će se snimke ako skupi troškovi

Stvaranja brze snimke, koja je samo za čitanje kopiju blob, može uzrokovati naknade za pohranu dodatni podaci za vaš račun. Prilikom dizajniranja aplikacije, važno je imati na umu kako te troškove možda ako skupi tako da možete minimizirati nepotrebne troškove.

### <a name="important-billing-considerations"></a>Važne stavke za naplatu

Sljedeći popis sadrži važne stavke voditi računa prilikom stvaranja brze snimke.

- Prostora za pohranu računa uključuje naknade za jedinstveni blokira ili stranica, hoće li se ona nalazi u blob-om ili snimke. Vaš račun doći do dodatnih troškova za snimke povezan s blob tek nakon što ažurirate blob na kojima se temelje. Nakon što ažurirate osnovni blob-om, udaljuje iz njegova snimke. Kada se to dogodi, vam se naplatiti za jedinstveni blokira ili stranice u svakom blob ili brze snimke.

- Kad zamijenite blok unutar bloka blob, to naknadno se naplaćuje kao jedinstveni blok. To vrijedi čak i ako bloka ima isti ID blok i iste podatke kao što je u snimku. Nakon izvršenja bloka ponovno udaljuje iz njegova postoji zamjena u obliku u bilo kojem snimke, a koje će se naplatiti svoje podatke. Isto se odnosi na stranice u blob stranice koji će se ažurirati jednake podatke.

- Zamjena blob bloka tako da nazovete metodu **UploadFile**, **UploadText**, **UploadStream**ili **UploadByteArray** zamjenjuje sve blokovi u blob-om. Ako imate snimke povezane s tom blob, sada granaju sve blokova u osnovnoj blob i snimku, a koje će se naplatiti sve blokova u oba blob-ova. To vrijedi čak i ako podatke osnovni blob i snimku ostaju isti.

- Servis blobova platforme Azure nema znači da biste odredili hoće li dva blokovi sadrže identične podatke. Svaki blok koja je prenesena i izvršenom tretira kao jedinstveni, čak i ako sadrži iste podatke i na istom bloka ID-a. Jer naknade ako skupi za jedinstveni blokove, važno je uzeti u obzir koju ažurirati blob koji sadrži snimku rezultira dodatne jedinstveni blokova i dodatnih troškova.

> [AZURE.NOTE] Najbolje prakse određuju upravljate snimke pažljivo radi izbjegavanja naknada za dodatni. Preporučujemo da upravljanje snimkama na sljedeći način:

> - Izbrišite i ponovno stvorite snimke povezan s blob kada ažurirate blob-om, čak i ako ažurirate identične podacima, osim ako dizajnu aplikacije potreban je održavate snimke. Tako da izbrišete, a zatim ponovno stvaranje snimki u blob, možete omogućiti da blob i snimaka ne granaju.

> - Ako su održavanje snimki za blob, nemojte pozivanje **UploadFile**, **UploadText**, **UploadStream**ili **UploadByteArray** da biste ažurirali blob-om. Ta metoda Zamijeni sve blokova u blob-om, tako da se osnovni blob i snimke granaju bitno. Umjesto toga, ažurirajte Najmanji mogući broj blokova **PutBlock** i **PutBlockList** načine.


### <a name="snapshot-billing-scenarios"></a>Snimka scenariji za naplatu


Sljedećim scenarijima pokazuje na koji način ako skupi naknade za blokiranje blob i njegov snimke.

U scenariju 1 osnovni blob nije ažuriran nakon snimku snimljena, tako da su troškovi nastali samo za jedinstveni blokovi 1, 2 i 3.

![Azure resursa za pohranu](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-1.png)

U slučaju 2 osnovni blob ažurirano, ali snimku još nije. Blokiranje 3 ažurirana pa čak i ako se u njemu iste podatke i isti ID nije isti kao blokirati 3 u snimku. Zbog toga račun je naplatiti četiri blokova.

![Azure resursa za pohranu](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-2.png)

U slučaju 3 osnovni blob ažurirano, ali snimku još nije. Blokiranje 3 je zamijenjena blok 4 u osnovnoj blob, ali snimku i dalje odražava bloka 3. Zbog toga račun je naplatiti četiri blokova.

![Azure resursa za pohranu](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-3.png)

U scenariju 4 osnovni blob potpuno ažurirana i sadrži nema njegov izvorni blokova. Kao rezultat računa se naplatiti osam jedinstveni blokova. Ovaj scenarij se može dogoditi ako koristite način ažuriranja kao što su **UploadFile**, **UploadText**, **UploadFromStream**ili **UploadByteArray**jer ovih metoda zamijeniti sav sadržaj blob.

![Azure resursa za pohranu](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-4.png)

## <a name="next-steps"></a>Daljnji koraci

Dodatni Primjeri uporabe blobova, potražite u članku [Primjere koda Azure](https://azure.microsoft.com/documentation/samples/?service=storage&term=blob). Možete preuzeti aplikaciju za uzorak i ga pokrenuti ili potražite kod na GitHub. 
