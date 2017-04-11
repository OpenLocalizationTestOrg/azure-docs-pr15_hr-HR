<properties 
    pageTitle="Kako koristiti blobova platforme Azure s WebJobs SDK" 
    description="Saznajte kako koristiti blobova platforme Azure s WebJobs SDK. Pokretanje procesa kada se pojavi novi blob u spremniku i obradu "poison blob-ova"." 
    services="app-service\web, storage" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="06/01/2016" 
    ms.author="tdykstra"/>

# <a name="how-to-use-azure-blob-storage-with-the-webjobs-sdk"></a>Kako koristiti blobova platforme Azure s WebJobs SDK

## <a name="overview"></a>Pregled

Ovaj vodič sadrži C# kod primjere koji pokazuju kako pokretanje procesa kada Stvori ili ažurira blobova platforme Azure. Primjere koda pomoću [WebJobs SDK](websites-dotnet-webjobs-sdk.md) verzije 1.x.

Primjere koda koji pokazuju kako stvoriti blob-ova, potražite u članku [kako koristiti za pohranu Azure reda čekanja s WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md). 
        
Vodič pretpostavlja znati [kako stvoriti WebJob projekta u Visual Studio sa veze nizovi koji upućuju na račun servisa za pohranu](websites-dotnet-webjobs-sdk-get-started.md) ili s [većim brojem računa za pohranu](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).

## <a id="trigger"></a>Kako pokrenuti funkciju kada blob Stvori ili ažurira

U ovom se odjeljku pokazuje kako treba koristiti s `BlobTrigger` atribut. 

> [AZURE.NOTE] WebJobs SDK skenira datoteke zapisnika da biste pogledali za nove ili promijenjene blob-ova. Ovaj postupak nije u stvarnom vremenu; Funkcija možda neće se aktivira tek nekoliko minuta ili više nakon stvaranja blob-om. Osim toga, osnovicu [za pohranu zapisnika stvaraju "najbolje naporima"](https://msdn.microsoft.com/library/azure/hh343262.aspx) ; Nema jamstva da će zabilježene svih događaja. U nekim uvjetima možda Propušteni zapisnika. Ako brzine i pouzdanosti ograničenja blob okidača nisu prihvatljivi aplikacije, preporučeni način je stvaranje reda čekanja poruke kada stvarate blob-om te koristite atribut [QueueTrigger](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#trigger) umjesto na `BlobTrigger` atribut funkciju koja obrađuje blob-om.

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

## <a id="types"></a>Vrste koje se možete povezati blob-ova

Možete koristiti u `BlobTrigger` atribut na sljedeće:

* `string`
* `TextReader`
* `Stream`
* `ICloudBlob`
* `CloudBlockBlob`
* `CloudPageBlob`
* `CloudBlobContainer`
* `CloudBlobDirectory`
* `IEnumerable<CloudBlockBlob>`
* `IEnumerable<CloudPageBlob>`
* Druge vrste deserijalizirati po [ICloudBlobStreamBinder](#icbsb) 

Ako želite raditi izravno na račun za Azure prostora za pohranu, možete dodati na `CloudStorageAccount` parametar potpis metode.

Primjeri, potražite u članku [kod povezivanja blobova platforme azure webjobs sdk spremište na GitHub.com](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/BlobBindingEndToEndTests.cs).

## <a id="string"></a>Početak tekstni sadržaj blob po povezivanje niz

Ako se očekuje tekst blob-ova, `BlobTrigger` se može primijeniti na `string` parametar. Sljedećim primjerom koda povezuje blob tekst da biste na `string` parametar pod nazivom `logMessage`. Funkcija koristi taj parametar pisati sadržaj blob-om WebJobs SDK nadzorne ploče. 
 
        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name, 
            TextWriter logger)
        {
             logger.WriteLine("Blob name: {0}", name);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }

## <a id="icbsb"></a>Početak serijalizirani blob sadržaja pomoću ICloudBlobStreamBinder

Sljedećim primjerom koda koristi predmete koji implementira `ICloudBlobStreamBinder` da biste omogućili u `BlobTrigger` atribut povezati blob da biste na `WebImage` vrsta.

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

Na `WebImage` povezivanje kod ugrađen u na `WebImageBinder` klase koja je izvedena iz `ICloudBlobStreamBinder`.

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

## <a name="getting-the-blob-path-for-the-triggering-blob"></a>Prvi put blobova platforme za pokretački blobova platforme

Da biste dobili spremnik ime i naziv blob blob u kojem se aktivirala funkciju, uključiti u `blobTrigger` niz parametra u potpisu (opis funkcije).

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name,
            string blobTrigger,
            TextWriter logger)
        {
             logger.WriteLine("Full blob path: {0}", blobTrigger);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }


## <a id="poison"></a>Kako rukovati poison blob-ova

Kada je `BlobTrigger` funkcija ne uspije, SDK poziva ga ponovno u slučaju pogreške koji je uzrok tranzitne pogreške. Ako pogrešku uzrokuje sadržaj blob-om, funkcija neće uspjeti svaki put kada se pokuša obradi blob-om. Prema zadanim postavkama SDK poziva funkcije do 5 puta za dani blob. Peti pokušajte ne uspije, SDK dodaje poruke red pod nazivom *webjobs blobtrigger poison*.

Maksimalan broj ponovne pokušaje je konfigurirati. Istu postavku [MaxDequeueCount](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#configqueue) koristi se za rukovanje poison blob i rukovanje porukama poison red. 

Poruka reda čekanja za poison blob-Ova je JSON objekt koji sadrži sljedeća svojstva:

* FunctionId (u obliku *{WebJob name}*. Funkcije. *{Naziv funkcije}*, na primjer: WebJob1.Functions.CopyBlob)
* BlobType ("BlockBlob" ili "PageBlob")
* Naziv spremnika
* BlobName
* E-oznake (identifikator za verziju blob, na primjer: "0x8D1DC6E70A277EF")

U sljedećim primjerom koda u `CopyBlob` funkcija ima kod koji uzrokuje njegovo uvoza svaki put kada se zove. Kada SDK poziva za maksimalni broj ponovne pokušaje, poruke se stvara na red poison blob i tu poruku obrađuje na `LogPoisonBlob` (opis funkcije). 

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

SDK automatski deserializes JSON poruke. Evo na `PoisonBlobMessage` klasa: 

        public class PoisonBlobMessage
        {
            public string FunctionId { get; set; }
            public string BlobType { get; set; }
            public string ContainerName { get; set; }
            public string BlobName { get; set; }
            public string ETag { get; set; }
        }

### <a id="polling"></a>Algoritam blob ankete

WebJobs SDK skenira sve spremnike određen `BlobTrigger` atributa na početku aplikacije. U računu velike prostora za pohranu taj pregled može potrajati neko vrijeme da bi se možda neko vrijeme prije nalaze se novi blob-ova i `BlobTrigger` se provode funkcije.

Da biste otkrili nove ili promijenjene blob-ova nakon početka aplikacije, SDK povremeno čita iz zapisnika spremišta blobova platforme. Zapisnici blob su u međuspremniku i dobiti fizički zapisati samo svakih 10 minuta ili tako, pa može biti vrlo odgode nakon blob Stvori ili ažurira prije odgovarajući `BlobTrigger` izvršava (opis funkcije). 

Postoji iznimku blob-ova koje stvarate pomoću na `Blob` atribut. Kada WebJobs SDK stvara novi blob, ona prosljeđuje novi blob odmah na sve odgovarajuće `BlobTrigger` funkcije. Stoga ako imate lanac blob unosa i izlaza, SDK-a možete ih obrađivati učinkovito. No ako želite niske latencije koji se izvodi na blob obrada funkcije za blob polja koje je stvorila ili ažurirana na neki drugi način, preporučujemo korištenje `QueueTrigger` umjesto `BlobTrigger`.

### <a id="receipts"></a>Blob potvrde

WebJobs SDK provjerava nije li koji nema `BlobTrigger` funkcija dobiva pod naslovom više puta za istu blob novi ili ažurirani. To čini čuvanjem *blob potvrde* da bi se odredili Ako navedeni blob verziju obrađen.

Potvrde blob spremaju se u kontejner *azure webjobs domaćini* u račun za Azure prostora za pohranu određen AzureWebJobsStorage niz za povezivanje. Potvrdu blob sadrži sljedeće podatke:

* Funkcija koja je pod nazivom blob-om ("*{WebJob name}*. Funkcije. *{Naziv funkcije}*", na primjer:"WebJob1.Functions.CopyBlob")
* Naziv spremnika
* Vrsta blob ("BlockBlob" ili "PageBlob")
* Naziv blobova platforme
* U e-oznake (identifikator za verziju blob, na primjer: "0x8D1DC6E70A277EF")

Ako želite da biste nametnuli reprocessing od blob potvrdu blobova platforme za taj blob ručno možete izbrisati iz spremnik *azure webjobs domaćini* .

## <a id="queues"></a>Povezane teme prekriveni članak reda čekanja

Informacije o kako rukovati blob Obrada reda čekanja poruka je pokrenuo ili za WebJobs SDK scenariji nisu specifične za bloba obrade, potražite u članku [kako koristiti za pohranu Azure reda čekanja s WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md). 

Povezane teme u tom članku obuhvaćaju sljedeće:

* Funkcija asinkrone
* Više instanci
* Graceful zatvaranja
* Korištenje atributa WebJobs SDK u tijelu funkcija
* Postavljanje veze nizovi SDK u kod.
* Postavljanje vrijednosti za WebJobs SDK Graditelj parametara u kodu
* Konfiguriranje `MaxDequeueCount` za rukovanje poison blob.
* Ručno pokretanje funkcije
* Pisanje zapisnika

## <a id="nextsteps"></a>Daljnji koraci

Ovaj vodič nudi primjere koda koji pokazuju kako rukovati uobičajeni scenariji za rad s Azure blob-ova. Dodatne informacije o korištenju Azure WebJobs i WebJobs SDK potražite u članku [Azure WebJobs preporučuje resursi](http://go.microsoft.com/fwlink/?linkid=390226).
 
