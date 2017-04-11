<properties 
    pageTitle="Praćenje Media Services posao obavijesti s .NET pomoću prostora za pohranu reda čekanja Azure | Microsoft Azure" 
    description="Saznajte kako koristiti Azure red za pohranu praćenje Media Services posao obavijesti. Uzorak koda napisan C# i koristi Media Services SDK za .NET." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016"   
    ms.author="juliako"/>

# <a name="use-azure-queue-storage-to-monitor-media-services-job-notifications-with-net"></a>Praćenje Media Services posao obavijesti s .NET pomoću reda čekanja Azure prostora za pohranu

Kada pokrenete zadacima, često tražite način za praćenje napretka posla. Možete provjeriti napredak pomoću prostora za pohranu reda čekanja Azure praćenje obavijesti posao Media Services (kao što je opisano u nastavku) ili definiranje StateChanged rukovatelj događajima (kao što je opisano u [ovoj](media-services-check-job-progress.md) temi.  

## <a name="use-azure-queue-storage-to-monitor-media-services-job-notifications"></a>Praćenje Media Services posao obavijesti pomoću reda čekanja Azure prostora za pohranu

Microsoft Azure Media Services sadrži mogućnost obavijesti o [Azure red za pohranu](../storage-dotnet-how-to-use-queues.md#what-is) tijekom obrade poslove medijske sadržaje. U ovoj se temi pokazuje kako se te poruke obavijesti iz spremišta red.

Poruka je isporučena u redu čekanja za pohranu možete pristupiti s bilo kojeg mjesta na svijetu. Red čekanja za Azure poruka arhitektura je pouzdan i vrlo skalabilni. Provjere reda čekanja za pohranu preporučuje se na druge načine. 

Jedan je uobičajeni scenarij za slušanje Media Services obavijesti ako razvijate sustavu za upravljanje sadržajem koji mora poduzeti neke dodatne zadatka nakon kodiranja posao dovršava (na primjer, okidača sljedeći korak u tijeku rada ili objavljivanje sadržaja). 

###<a name="considerations"></a>Razmatranja

Prilikom razvoja aplikacije Media Services koje koriste reda čekanja Azure prostora za pohranu, razmotrite sljedeće.

- Servis reda čekanja ne nudi jamstva first-u – first out (FIFO) naručili isporuke. Dodatne informacije potražite u članku [redova Azure i Azure servisa Bus redova usporedbi i Contrasted](https://msdn.microsoft.com/library/azure/hh767287.aspx).
- Azure prostora za pohranu redova nije servis za automatsko prosljeđivanje; morate provjeriti reda. 
- Možete imati bilo koji broj redova. Dodatne informacije potražite u članku [Reda čekanja servisa REST API -JA](https://msdn.microsoft.com/library/azure/dd179363.aspx).
- Azure prostora za pohranu redova sadrži neke ograničenja i specifičnosti koji su opisani u sljedećem članku: [Azure redovima i Azure servisa Bus redova usporedbi i Contrasted](https://msdn.microsoft.com/library/azure/hh767287.aspx).

###<a name="code-example"></a>Primjer koda

Primjer koda u ovom odjeljku čini sljedeće:

1. Određuje klasu **EncodingJobMessage** mapira oblik poruke s obavijesti. Kod deserializes poruke koje ste primili iz reda čekanja u objekata vrste **EncodingJobMessage** .
1. Iz datoteke app.config učita Media Services i pohranu podataka o računu na. Da biste stvorili objekata **CloudMediaContext** i **CloudQueue** koristi taj podatak.
1. Stvara čekanja koji prima obavijest poruke o kodiranja posao.
1. Stvara obavijesti krajnju točku koja je mapirana reda.
1. Pridružuje krajnju točku obavijesti za posao i šalje kodiranja posao. Možete imati više obavijesti krajnje točke priložiti posao.
1. U ovom primjeru smo zanima samo konačni savezne države posao obrade tako ćemo proslijediti **NotificationJobState.FinalStatesOnly** metoda **AddNew** . 
        
        job.JobNotificationSubscriptions.AddNew(NotificationJobState.FinalStatesOnly, _notificationEndPoint);
1. Ako NotificationJobState.All, možete očekivati da biste sve obavijesti o promjeni stanje: u redu čekanja -> zakazano -> Obrada -> dovršeno. Međutim, kao što je naznačeno ranije, servisa Azure prostora za pohranu redova ne jamči uređeni isporuke. Možete koristiti vremenske oznake svojstvo (definirano u tipu EncodingJobMessage u primjeru u nastavku) redoslijed porukama. Moguće je da ćete dobiti duplicirane obavijesti o. Svojstvo e-oznake (definirano u tipu EncodingJobMessage) koristiti za traženje duplikata. Moguće je da će preskočiti neke obavijesti o promjeni stanja. 
1. Čeka se za posao da biste završili stanje potvrđivanjem reda čekanja svakih 10 sekundi. Briše poruke kada se obrađuju.
1. Brisanje reda čekanja i krajnju točku obavijesti.

>[AZURE.NOTE]Preporučeni način praćenje stanja posao je slušanje obavijesti poruke, kao što je prikazano u sljedećem primjeru.
>
>Osim toga, mogli provjeriti na stanje posao korištenjem svojstva **IJob.State** .  Poruke s obavijesti o dovršetka posla, možda stižu prije stanja na **IJob** postavljen na **Dovršeno**. Svojstvo **IJob.State** odražava stanje točne s napraviti manje odgode.

    
    using System;
    using System.Linq;
    using System.Configuration;
    using System.IO;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using System.Collections.Generic;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Web;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Queue;
    using System.Runtime.Serialization.Json;
    
    namespace JobNotification
    {
        public class EncodingJobMessage
        {
            // MessageVersion is used for version control. 
            public String MessageVersion { get; set; }
        
            // Type of the event. Valid values are 
            // JobStateChange and NotificationEndpointRegistration.
            public String EventType { get; set; }
        
            // ETag is used to help the customer detect if 
            // the message is a duplicate of another message previously sent.
            public String ETag { get; set; }
        
            // Time of occurrence of the event.
            public String TimeStamp { get; set; }
    
            // Collection of values specific to the event.
    
            // For the JobStateChange event the values are:
            //     JobId - Id of the Job that triggered the notification.
            //     NewState- The new state of the Job. Valid values are:
            //          Scheduled, Processing, Canceling, Cancelled, Error, Finished
            //     OldState- The old state of the Job. Valid values are:
            //          Scheduled, Processing, Canceling, Cancelled, Error, Finished
    
            // For the NotificationEndpointRegistration event the values are:
            //     NotificationEndpointId- Id of the NotificationEndpoint 
            //          that triggered the notification.
            //     State- The state of the Endpoint. 
            //          Valid values are: Registered and Unregistered.
    
            public IDictionary<string, object> Properties { get; set; }
        }
    
        class Program
        {
            private static CloudMediaContext _context = null;
            private static CloudQueue _queue = null;
            private static INotificationEndPoint _notificationEndPoint = null;
    
            private static readonly string _singleInputMp4Path =
                Path.GetFullPath(@"C:\supportFiles\multifile\BigBuckBunny.mp4");
    
            static void Main(string[] args)
            {
                // Get the values from app.config file.
                string mediaServicesAccountName = ConfigurationManager.AppSettings["MediaServicesAccountName"];
                string mediaServicesAccountKey = ConfigurationManager.AppSettings["MediaServicesAccountKey"];
                string storageConnectionString = ConfigurationManager.AppSettings["StorageConnectionString"];
    
    
                string endPointAddress = Guid.NewGuid().ToString();
    
                // Create the context. 
                _context = new CloudMediaContext(mediaServicesAccountName, mediaServicesAccountKey);
    
                // Create the queue that will be receiving the notification messages.
                _queue = CreateQueue(storageConnectionString, endPointAddress);
    
                // Create the notification point that is mapped to the queue.
                _notificationEndPoint = 
                        _context.NotificationEndPoints.Create(
                        Guid.NewGuid().ToString(), NotificationEndPointType.AzureQueue, endPointAddress);
    
    
                if (_notificationEndPoint != null)
                {
                    IJob job = SubmitEncodingJobWithNotificationEndPoint(_singleInputMp4Path);
                    WaitForJobToReachedFinishedState(job.Id);
                }
    
                // Clean up.
                _queue.Delete();      
                _notificationEndPoint.Delete();
           }
    
    
            static public CloudQueue CreateQueue(string storageAccountConnectionString, string endPointAddress)
            {
                CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageAccountConnectionString);
    
                // Create the queue client
                CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    
                // Retrieve a reference to a queue
                CloudQueue queue = queueClient.GetQueueReference(endPointAddress);
    
                // Create the queue if it doesn't already exist
                queue.CreateIfNotExists();
    
                return queue;
            }
     
    
            public static IJob SubmitEncodingJobWithNotificationEndPoint(string inputMediaFilePath)
            {
                // Declare a new job.
                IJob job = _context.Jobs.Create("My MP4 to Smooth Streaming encoding job");
    
                //Create an encrypted asset and upload the mp4. 
                IAsset asset = CreateAssetAndUploadSingleFile(AssetCreationOptions.StorageEncrypted, 
                    inputMediaFilePath);
    
                // Get a media processor reference, and pass to it the name of the 
                // processor to use for the specific task.
                IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
    
                // Create a task with the conversion details, using a configuration file. 
                ITask task = job.Tasks.AddNew("My encoding Task",
                    processor,
                    "H264 Multiple Bitrate 720p",
                    Microsoft.WindowsAzure.MediaServices.Client.TaskOptions.None);
    
                // Specify the input asset to be encoded.
                task.InputAssets.Add(asset);
    
                // Add an output asset to contain the results of the job.
                task.OutputAssets.AddNew("Output asset",
                    AssetCreationOptions.None);
    
                // Add a notification point to the job. You can add multiple notification points.  
                job.JobNotificationSubscriptions.AddNew(NotificationJobState.FinalStatesOnly, 
                    _notificationEndPoint);
    
                job.Submit();
    
                return job;
            }
    
            public static void WaitForJobToReachedFinishedState(string jobId)
            {
                int expectedState = (int)JobState.Finished;
                int timeOutInSeconds = 600;
    
                bool jobReachedExpectedState = false;
                DateTime startTime = DateTime.Now;
                int jobState = -1;
    
                while (!jobReachedExpectedState)
                {
                    // Specify how often you want to get messages from the queue.
                    Thread.Sleep(TimeSpan.FromSeconds(10));
    
                    foreach (var message in _queue.GetMessages(10))
                    {
                        using (Stream stream = new MemoryStream(message.AsBytes))
                        {
                            DataContractJsonSerializerSettings settings = new DataContractJsonSerializerSettings();
                            settings.UseSimpleDictionaryFormat = true;
                            DataContractJsonSerializer ser = new DataContractJsonSerializer(typeof(EncodingJobMessage), settings);
                            EncodingJobMessage encodingJobMsg = (EncodingJobMessage)ser.ReadObject(stream);
    
                            Console.WriteLine();
    
                            // Display the message information.
                            Console.WriteLine("EventType: {0}", encodingJobMsg.EventType);
                            Console.WriteLine("MessageVersion: {0}", encodingJobMsg.MessageVersion);
                            Console.WriteLine("ETag: {0}", encodingJobMsg.ETag);
                            Console.WriteLine("TimeStamp: {0}", encodingJobMsg.TimeStamp);
                            foreach (var property in encodingJobMsg.Properties)
                            {
                                Console.WriteLine("    {0}: {1}", property.Key, property.Value);
                            }
    
                            // We are only interested in messages 
                            // where EventType is "JobStateChange".
                            if (encodingJobMsg.EventType == "JobStateChange")
                            {
                                string JobId = (String)encodingJobMsg.Properties.Where(j => j.Key == "JobId").FirstOrDefault().Value;
                                if (JobId == jobId)
                                {
                                    string oldJobStateStr = (String)encodingJobMsg.Properties.
                                                                Where(j => j.Key == "OldState").FirstOrDefault().Value;
                                    string newJobStateStr = (String)encodingJobMsg.Properties.
                                                                Where(j => j.Key == "NewState").FirstOrDefault().Value;
    
                                    JobState oldJobState = (JobState)Enum.Parse(typeof(JobState), oldJobStateStr);
                                    JobState newJobState = (JobState)Enum.Parse(typeof(JobState), newJobStateStr);
    
                                    if (newJobState == (JobState)expectedState)
                                    {
                                        Console.WriteLine("job with Id: {0} reached expected state: {1}", 
                                            jobId, newJobState);
                                        jobReachedExpectedState = true;
                                        break;
                                    }
                                }
                            }
                        }
                        // Delete the message after we've read it.
                        _queue.DeleteMessage(message);
                    }
    
                    // Wait until timeout
                    TimeSpan timeDiff = DateTime.Now - startTime;
                    bool timedOut = (timeDiff.TotalSeconds > timeOutInSeconds);
                    if (timedOut)
                    {
                        Console.WriteLine(@"Timeout for checking job notification messages, 
                                            latest found state ='{0}', wait time = {1} secs",
                            jobState,
                            timeDiff.TotalSeconds);
    
                        throw new TimeoutException();
                    }
                }
            }
       
            static private IAsset CreateAssetAndUploadSingleFile(AssetCreationOptions assetCreationOptions, string singleFilePath)
            {
                var asset = _context.Assets.Create("UploadSingleFile_" + DateTime.UtcNow.ToString(), 
                    assetCreationOptions);
    
                var fileName = Path.GetFileName(singleFilePath);
    
                var assetFile = asset.AssetFiles.Create(fileName);
    
                Console.WriteLine("Created assetFile {0}", assetFile.Name);
                Console.WriteLine("Upload {0}", assetFile.Name);
    
                assetFile.Upload(singleFilePath);
                Console.WriteLine("Done uploading of {0}", assetFile.Name);
    
                return asset;
            }
    
            static private IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
            {
                var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
                    ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
    
                if (processor == null)
                    throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
    
                return processor;
            }
        }
    }

Gornji primjer proizvodi sljedeći rezultat. Mijenjati će vam vrijednosti.
    
    Created assetFile BigBuckBunny.mp4
    Upload BigBuckBunny.mp4
    Done uploading of BigBuckBunny.mp4
    
    EventType: NotificationEndPointRegistration
    MessageVersion: 1.0
    ETag: e0238957a9b25bdf3351a88e57978d6a81a84527fad03bc23861dbe28ab293f6
    TimeStamp: 2013-05-14T20:22:37
        NotificationEndPointId: nb:nepid:UUID:d6af9412-2488-45b2-ba1f-6e0ade6dbc27
        State: Registered
        Name: dde957b2-006e-41f2-9869-a978870ac620
        Created: 2013-05-14T20:22:35
    
    EventType: JobStateChange
    MessageVersion: 1.0
    ETag: 4e381f37c2d844bde06ace650310284d6928b1e50101d82d1b56220cfcb6076c
    TimeStamp: 2013-05-14T20:24:40
        JobId: nb:jid:UUID:526291de-f166-be47-b62a-11ffe6d4be54
        JobName: My MP4 to Smooth Streaming encoding job
        NewState: Finished
        OldState: Processing
        AccountName: westeuropewamsaccount
    job with Id: nb:jid:UUID:526291de-f166-be47-b62a-11ffe6d4be54 reached expected 
    State: Finished
    

## <a name="next-step"></a>Sljedeći korak

Pregled Media Services Tečajevi

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
