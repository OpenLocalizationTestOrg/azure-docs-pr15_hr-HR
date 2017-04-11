<properties 
    pageTitle="Provjere dugoročnih postupaka | Microsoft Azure" 
    description="U ovoj se temi objašnjava ankete dugoročnih postupaka." 
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
    ms.date="09/26/2016" 
    ms.author="juliako"/>


#<a name="delivering-live-streaming-with-azure-media-services"></a>Izlaganja uživo strujanje servisa Azure Media Services

##<a name="overview"></a>Pregled

Microsoft Azure Media Services nudi API-ji Pošalji zahtjeve Media Services da biste pokrenuli operacije (na primjer: Stvaranje, pokretanje, zaustavljanje ili brisanje kanala). Te operacije su dugoročnih.

Media Services .NET SDK pruža API-ji koji poslali zahtjev i pričekajte da biste dovršili postupak (interno, API-ji su provjere za operaciju tijeku neke intervalima). Na primjer, prilikom pozivanja kanala. Start(), metodu vraća nakon pokretanja kanal. Možete koristiti i asinkronog verziju: await kanala. StartAsync() (informacije o utemeljenoj na zadacima asinkronog uzorak potražite u članku [DODIRNITE](https://msdn.microsoft.com/library/hh873175(v=vs.110).aspx)). API-ji koji pošaljite zahtjev za operaciju, a zatim provjeriti ima li status postupak dok se ne dovrši nazivaju se "ankete metode". Načini (osobito verzija asinkrone) preporučuje se za aplikacije obogaćenog klijenta i/ili s praćenjem stanja servisa.

Postoje scenariji gdje se aplikacije ne može proći dugo izvodi http zahtjev i želi ručno provjeriti tijek postupka. Uobičajeni primjer bio preglednika interakcija s bez praćenja stanja web-servisa: kada preglednik zatraži da biste stvorili kanala, web-servisa pokreće dugo izvodi postupak i vraća Identifikator operacije u preglednik. U pregledniku pa zatražite od web-servisa da biste dobili status operacija na temelju s ID-a. Media Services .NET SDK nudi API-ji koji su korisne za taj scenarij. Ove API-ji nazivaju se "koje nisu ankete metode".
"Metode koje nisu ankete" imate sljedeće obrazac imenovanja: slanje operacija*OperationName*(na primjer, SendCreateOperation). Slanje*OperationName*operacija metode vratiti objekt **IOperation** ; vraćeni objekt sadrži informacije koje je moguće koristiti za praćenje postupak. Slanje*OperationName*OperationAsync metode vratiti **zadatka<IOperation>**.

Trenutno podržava sljedeće klase koje nisu ankete metoda: **kanal**, **StreamingEndpoint**i **Program**.

Provjeriti ima li status operacije, pomoću metode **GetOperation** na **OperationBaseCollection** predmete. Pomoću sljedećih razdoblja da biste provjerili status operacije: za **kanal** i **StreamingEndpoint** operacije, koristite 30 sekundi; Da biste postigli operacije **Program** , koristite 10 sekundi.


##<a name="example"></a>Primjer

Sljedeći primjer određuje klasu pod nazivom **ChannelOperations**. Ove definicije klase može biti početnu točku za definicije web servis za predmete. Zbog jednostavnosti, u sljedećem se primjeru koristi verzije koje nisu asinkrone metode.

U primjeru prikazano i načina na koji klijent mogu koristiti ovu klasu.

###<a name="channeloperations-class-definition"></a>Definicija ChannelOperations klase

    /// <summary> 
    /// The ChannelOperations class only implements 
    /// the Channel’s creation operation. 
    /// </summary> 
    public class ChannelOperations
    {
        // Read values from the App.config file.
        private static readonly string _mediaServicesAccountName =
            ConfigurationManager.AppSettings["MediaServicesAccountName"];
        private static readonly string _mediaServicesAccountKey =
            ConfigurationManager.AppSettings["MediaServicesAccountKey"];
    
        // Field for service context.
        private static CloudMediaContext _context = null;
        private static MediaServicesCredentials _cachedCredentials = null;
    
        public ChannelOperations()
        {
                _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName,
                    _mediaServicesAccountKey);
    
                _context = new CloudMediaContext(_cachedCredentials);    }
    
        /// <summary>  
        /// Initiates the creation of a new channel.  
        /// </summary>  
        /// <param name="channelName">Name to be given to the new channel</param>  
        /// <returns>  
        /// Operation Id for the long running operation being executed by Media Services. 
        /// Use this operation Id to poll for the channel creation status. 
        /// </returns> 
        public string StartChannelCreation(string channelName)
        {
            var operation = _context.Channels.SendCreateOperation(
                new ChannelCreationOptions
                {
                    Name = channelName,
                    Input = CreateChannelInput(),
                    Preview = CreateChannelPreview(),
                    Output = CreateChannelOutput()
                });
    
            return operation.Id;
        }
    
        /// <summary> 
        /// Checks if the operation has been completed. 
        /// If the operation succeeded, the created channel Id is returned in the out parameter.
        /// </summary> 
        /// <param name="operationId">The operation Id.</param> 
        /// <param name="channel">
        /// If the operation succeeded, 
        /// the created channel Id is returned in the out parameter.</param>
        /// <returns>Returns false if the operation is still in progress; otherwise, true.</returns> 
        public bool IsCompleted(string operationId, out string channelId)
        {
            IOperation operation = _context.Operations.GetOperation(operationId);
            bool completed = false;
    
            channelId = null;
    
            switch (operation.State)
            {
                case OperationState.Failed:
                    // Handle the failure. 
                    // For example, throw an exception. 
                    // Use the following information in the exception: operationId, operation.ErrorMessage.
                    break;
                case OperationState.Succeeded:
                    completed = true;
                    channelId = operation.TargetEntityId;
                    break;
                case OperationState.InProgress:
                    completed = false;
                    break;
            }
            return completed;
        }
    
    
        private static ChannelInput CreateChannelInput()
        {
            return new ChannelInput
            {
                StreamingProtocol = StreamingProtocol.RTMP,
                AccessControl = new ChannelAccessControl
                {
                    IPAllowList = new List<IPRange>
                    {
                        new IPRange
                        {
                            Name = "TestChannelInput001",
                            Address = IPAddress.Parse("0.0.0.0"),
                            SubnetPrefixLength = 0
                        }
                    }
                }
            };
        }
    
        private static ChannelPreview CreateChannelPreview()
        {
            return new ChannelPreview
            {
                AccessControl = new ChannelAccessControl
                {
                    IPAllowList = new List<IPRange>
                    {
                        new IPRange
                        {
                            Name = "TestChannelPreview001",
                            Address = IPAddress.Parse("0.0.0.0"),
                            SubnetPrefixLength = 0
                        }
                    }
                }
            };
        }
    
        private static ChannelOutput CreateChannelOutput()
        {
            return new ChannelOutput
            {
                Hls = new ChannelOutputHls { FragmentsPerSegment = 1 }
            };
        }
    }

###<a name="the-client-code"></a>Kod klijenta

    ChannelOperations channelOperations = new ChannelOperations();
    string opId = channelOperations.StartChannelCreation("MyChannel001");
    
    string channelId = null;
    bool isCompleted = false;
    
    while (isCompleted == false)
    {
        System.Threading.Thread.Sleep(TimeSpan.FromSeconds(30));
        isCompleted = channelOperations.IsCompleted(opId, out channelId);
    }
    
    // If we got here, we should have the newly created channel id.
    Console.WriteLine(channelId);
 


##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
