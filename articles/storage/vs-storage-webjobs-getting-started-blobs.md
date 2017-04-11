<properties
    pageTitle="Početak rada s blob prostora za pohranu i Visual Studio povezani servisi (WebJob projekti) | Microsoft Azure"
    description="Kako započeti rad sa sustavom blobova u programu project WebJob nakon povezivanja s Azure za pohranu pomoću Visual Studio povezani servisi."
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

# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-webjob-projects"></a>Početak rada s blobova platforme Azure prostora za pohranu i Visual Studio povezani servisi (WebJob projekti)

[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Pregled

Ovaj članak sadrži C# kod primjere koji pokazuju kako pokretanje procesa kada Stvori ili ažurira blobova platforme Azure. Primjere koda pomoću verzije [WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md) 1.x. Kada dodate račun za pohranu WebJob projekt pomoću dijaloški okvir za Visual Studio **Dodavanje povezani servisi** , odgovarajući NuGet Azure prostora za pohranu paketa je instaliran, odgovarajući .NET reference se dodaju u projekt, a nizu za povezivanje za račun za pohranu ažuriraju u datoteci App.config.



## <a name="how-to-trigger-a-function-when-a-blob-is-created-or-updated"></a>Kako pokrenuti funkciju kada blob Stvori ili ažurira

U ovom se odjeljku objašnjava korištenje atributa **BlobTrigger** .

 **Bilješke:** WebJobs SDK skenira datoteke zapisnika da biste pogledali za nove ili promijenjene blob-ova. Taj postupak ne čini funkcionira; Funkcija možda neće se aktivira tek nekoliko minuta ili više nakon stvaranja blob-om.  Ako aplikacija nije potrebno odmah obraditi blob-ova, preporučeni način je da biste stvorili poruku reda čekanja kada stvarate blob-om te koristite atribut [QueueTrigger](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#trigger) umjesto atribut **BlobTrigger** na funkciju koja obrađuje blob-om.

### <a name="single-placeholder-for-blob-name-with-extension"></a>Jednostruko rezervirano mjesto za naziv blob s nastavkom  

Sljedećim primjerom koda kopira blob-ova teksta koji se pojavljuju u spremnik *unos* spremniku *Izlaz* :

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("output/{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

Atribut Graditelj vodi parametra niza koji određuje naziv spremnika i rezervirano mjesto za naziv blob. U ovom primjeru, ako blob pod nazivom *Blob1.txt* je stvorena u spremniku za *unos* funkcija stvara blob pod nazivom *Blob1.txt* u spremniku *Izlaz* .

Možete navesti naziv uzorak s rezerviranim mjestom naziv blob, kao što je prikazano u sljedećim primjerom koda:

        public static void CopyBlob([BlobTrigger("input/original-{name}")] TextReader input,
            [Blob("output/copy-{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

Kod kopira samo blob-ova čiji nazivi počinju s "izvorne-". *Izvorni Blob1.txt* u spremniku za *unos* , na primjer, se kopira *Kopiraj Blob1.txt* u spremniku *Izlaz* .

Ako vam je potrebna za određivanje uzorka naziv za blob nazive koji imaju vitičaste zagrade u nazivu, možete primijeniti dvostruki vitičaste zagrade. Na primjer, ako želite pronaći blob-ova u spremniku *slike* koji su nazivi ovako:

        {20140101}-soundfile.mp3

Ta postavka za vaše uzorak:

        images/{{20140101}}-{name}

U ovom primjeru vrijednost rezervirano mjesto za *naziv* bio *soundfile.mp3*.

### <a name="separate-blob-name-and-extension-placeholders"></a>Rezervirana mjesta nazivom i nastavkom zasebnom blobova platforme

Sljedećim primjerom koda mijenja datotečni nastavak kao kopira blob-ova koji se pojavljuju u spremnik *unos* spremniku *Izlaz* . Kod zapisnike proširenje *unos* blob i postavlja proširenje blob *Izlaz* na *.txt*.

        public static void CopyBlobToTxtFile([BlobTrigger("input/{name}.{ext}")] TextReader input,
            [Blob("output/{name}.txt")] out string output,
            string name,
            string ext,
            TextWriter logger)
        {
            logger.WriteLine("Blob name:" + name);
            logger.WriteLine("Blob extension:" + ext);
            output = input.ReadToEnd();
        }

## <a name="types-that-you-can-bind-to-blobs"></a>Vrste koje se možete povezati blob-ova

Atribut **BlobTrigger** možete koristiti na sljedeće:

* **niz**
* **ZapisivačTeksta**
* **Toka**
* **ICloudBlob**
* **CloudBlockBlob**
* **CloudPageBlob**
* Druge vrste deserijalizirati po [ICloudBlobStreamBinder](#getting-serialized-blob-content-by-using-icloudblobstreambinder)

Ako želite raditi izravno na račun za Azure prostora za pohranu, možete dodati i parametar **CloudStorageAccount** potpis metode.

## <a name="getting-text-blob-content-by-binding-to-string"></a>Početak tekstni sadržaj blob po povezivanja niz

Ako se očekuje tekst blob-ova, **BlobTrigger** se mogu primijeniti parametar **niza** . Sljedećim primjerom koda povezuje blob tekstni **niz** parametar pod nazivom **logMessage**. Funkcija koristi taj parametar pisati sadržaj blob-om WebJobs SDK nadzorne ploče.

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name,
            TextWriter logger)
        {
             logger.WriteLine("Blob name: {0}", name);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }

## <a name="getting-serialized-blob-content-by-using-icloudblobstreambinder"></a>Početak serijalizirani blob sadržaja pomoću ICloudBlobStreamBinder

Sljedećim primjerom koda koristi predmete koji implementira **ICloudBlobStreamBinder** da biste omogućili atribut **BlobTrigger** povezati blob vrstu **WebImage** .

        public static void WaterMark(
            [BlobTrigger("images3/{name}")] WebImage input,
            [Blob("images3-watermarked/{name}")] out WebImage output)
        {
            output = input.AddTextWatermark("WebJobs SDK",
                horizontalAlign: "Center", verticalAlign: "Middle",
                fontSize: 48, opacity: 50);
        }
        public static void Resize(
            [BlobTrigger("images3-watermarked/{name}")] WebImage input,
            [Blob("images3-resized/{name}")] out WebImage output)
        {
            var width = 180;
            var height = Convert.ToInt32(input.Height * 180 / input.Width);
            output = input.Resize(width, height);
        }

Povezivanje kod **WebImage** navedeni su u razredu **WebImageBinder** koja je izvedena iz **ICloudBlobStreamBinder**.

        public class WebImageBinder : ICloudBlobStreamBinder<WebImage>
        {
            public Task<WebImage> ReadFromStreamAsync(Stream input,
                System.Threading.CancellationToken cancellationToken)
            {
                return Task.FromResult<WebImage>(new WebImage(input));
            }
            public Task WriteToStreamAsync(WebImage value, Stream output,
                System.Threading.CancellationToken cancellationToken)
            {
                var bytes = value.GetBytes();
                return output.WriteAsync(bytes, 0, bytes.Length, cancellationToken);
            }
        }

## <a name="how-to-handle-poison-blobs"></a>Kako rukovati poison blob-ova

Ako funkcija **BlobTrigger** ne uspije, SDK poziva ga ponovno u slučaju pogreške koji je uzrok tranzitne pogreške. Ako pogrešku uzrokuje sadržaj blob-om, funkcija neće uspjeti svaki put kada se pokuša obradi blob-om. Prema zadanim postavkama SDK poziva funkcije do 5 puta za dani blob. Peti pokušajte ne uspije, SDK dodaje poruke red pod nazivom *webjobs blobtrigger poison*.

Maksimalan broj ponovne pokušaje je konfigurirati. Istu postavku [MaxDequeueCount](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#configqueue) koristi se za rukovanje poison blob i rukovanje porukama poison red.

Poruka reda čekanja za poison blob-Ova je JSON objekt koji sadrži sljedeća svojstva:

* FunctionId (u obliku *{WebJob name}*. Funkcije. *{Naziv funkcije}*, na primjer: WebJob1.Functions.CopyBlob)
* BlobType ("BlockBlob" ili "PageBlob")
* Naziv spremnika
* BlobName
* E-oznake (identifikator za verziju blob, na primjer: "0x8D1DC6E70A277EF")

U sljedećim primjerom koda funkciju **CopyBlob** sadrži kod koji uzrokuje njegovo uvoza svaki put kada se zove. Nakon SDK je poziva za maksimalni broj ponovne pokušaje, poruke se stvara na red poison blob i tu poruku obrađuje funkciju **LogPoisonBlob** .

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("textblobs/output-{name}")] out string output)
        {
            throw new Exception("Exception for testing poison blob handling");
            output = input.ReadToEnd();
        }

        public static void LogPoisonBlob(
        [QueueTrigger("webjobs-blobtrigger-poison")] PoisonBlobMessage message,
            TextWriter logger)
        {
            logger.WriteLine("FunctionId: {0}", message.FunctionId);
            logger.WriteLine("BlobType: {0}", message.BlobType);
            logger.WriteLine("ContainerName: {0}", message.ContainerName);
            logger.WriteLine("BlobName: {0}", message.BlobName);
            logger.WriteLine("ETag: {0}", message.ETag);
        }

SDK automatski deserializes JSON poruke. Ovo je predmet **PoisonBlobMessage** :

        public class PoisonBlobMessage
        {
            public string FunctionId { get; set; }
            public string BlobType { get; set; }
            public string ContainerName { get; set; }
            public string BlobName { get; set; }
            public string ETag { get; set; }
        }

### <a name="blob-polling-algorithm"></a>Algoritam blob ankete

WebJobs SDK pregledava sve spremnike određen **BlobTrigger** atributa na početku aplikacije. Na računu za velike prostora za pohranu taj pregled može potrajati neko vrijeme da bi se možda neko vrijeme da bi se nalaze se novi blob-ova i funkcije **BlobTrigger** se izvode.

Da biste otkrili nove ili promijenjene blob-ova nakon početka aplikacije, SDK povremeno čita iz zapisnika spremišta blobova platforme. Zapisnike blob su u međuspremniku i dobiti fizički zapisati samo svakih 10 minuta ili zato da bi se može biti vrlo odgode nakon blob Stvori ili ažurira prije odgovarajući **BlobTrigger** funkcija izvršava.

Došlo je do iznimke blob-ova koje stvarate pomoću atribut **Blob** . Kada WebJobs SDK stvara novi blob, ga prosljeđuje novi blob odmah odgovarajuće funkcije **BlobTrigger** . Stoga ako imate lanac blob unosa i izlaza, SDK-a možete ih obrađivati učinkovito. No ako želite niske latencije koji se izvodi na blob obrade funkcije za blob polja koje je stvorila ili ažurirana na neki drugi način, preporučujemo korištenje **QueueTrigger** umjesto **BlobTrigger**.

### <a name="blob-receipts"></a>Blob potvrde

WebJobs SDK jamči da ne sadrži funkciju **BlobTrigger** dobiva se zove više puta za istu blob novi ili ažurirani. To čini čuvanjem *blob potvrde* da bi se odredili Ako navedeni blob verziju obrađen.

Potvrde blob spremaju se u kontejner *azure webjobs domaćini* u račun za Azure prostora za pohranu određen AzureWebJobsStorage niz za povezivanje. Potvrdu blob sadrži sljedeće podatke:

* Funkcija koja je pod nazivom blob-om ("*{WebJob name}*. Funkcije. *{Naziv funkcije}*", na primjer:"WebJob1.Functions.CopyBlob")
* Naziv spremnik
* Vrsta blob ("BlockBlob" ili "PageBlob")
* Naziv blobova platforme
* U e-oznake (identifikator za verziju blob, na primjer: "0x8D1DC6E70A277EF")

Ako želite da biste nametnuli reprocessing od blob potvrdu blobova platforme za taj blob ručno možete izbrisati iz spremnik *azure webjobs domaćini* .

## <a name="related-topics-covered-by-the-queues-article"></a>Povezane teme prekriveni članak reda čekanja

Informacije o kako rukovati blob Obrada reda čekanja poruka je pokrenuo ili za WebJobs SDK scenariji nisu specifične za bloba obrade, potražite u članku [kako koristiti za pohranu Azure reda čekanja s WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md).

Povezane teme u tom članku obuhvaćaju sljedeće:

* Funkcija asinkrone
* Više instanci
* Graceful zatvaranja
* Korištenje atributa WebJobs SDK u tijelu funkcija
* Postavljanje SDK nizu za povezivanje u kod.
* Postavljanje vrijednosti za WebJobs SDK Graditelj parametara u kodu
* Konfiguriranje **MaxDequeueCount** za rukovanje poison blob.
* Ručno pokretanje funkcije
* Pisanje zapisnika

## <a name="next-steps"></a>Daljnji koraci

U ovom se članku nudi primjere koda koji pokazuju kako rukovati uobičajeni scenariji za rad s Azure blob-ova. Dodatne informacije o korištenju Azure WebJobs i WebJobs SDK potražite u članku [Azure WebJobs dokumentaciju resursi](http://go.microsoft.com/fwlink/?linkid=390226).
