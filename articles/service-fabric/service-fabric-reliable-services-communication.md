<properties
   pageTitle="Pregled komunikacije pouzdanog Services | Microsoft Azure"
   description="Pregled komunikacije modela pouzdanog usluge uključujući otvaranje slušače na servisima, rješavanje krajnje točke i komunikaciju između servisa."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor="BharatNarasimman"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="how-to-use-the-reliable-services-communication-apis"></a>Kako koristiti komunikacije pouzdanog Services API-ji

Azure tkanina servis kao platforma je potpuno agnostic o komunikaciji između servisa. Sve protokoli i snop su prihvatljiva, iz UDP za HTTP. Je programiranje servisa da biste odabrali kako treba komunikacija servisa. Aplikacija framework pouzdanog servisa nudi snop ugrađene komunikacije, kao i API-ji koje možete koristiti da biste sastavili komponente prilagođene komunikacije. 

## <a name="set-up-service-communication"></a>Postavljanje komunikacije servisa

Pouzdan API servisa koristi jednostavne sučelje za usluge komunikaciju. Da biste otvorili krajnje servisa, jednostavno implementirati ovog sučelja:

```csharp

public interface ICommunicationListener
{
    Task<string> OpenAsync(CancellationToken cancellationToken);

    Task CloseAsync(CancellationToken cancellationToken);

    void Abort();
}

```

Zatim možete dodati ga slušatelj implementaciju komunikacije vraćanjem u nadjačavanje za metodu utemeljen na servis za predmete.

Za bez praćenja stanja servisa:

```csharp
class MyStatelessService : StatelessService
{
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        ...
    }
    ...
}
```

Za s praćenjem stanja servisa:

```csharp
class MyStatefulService : StatefulService
{
    protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
    {
        ...
    }
    ...
}
```

U oba slučaja, vratite se skup slušače. Time se omogućuje uslugu da biste preslušali na više krajnje točke potencijalno pomoću protokola za različite pomoću više slušače. Ako, na primjer, možda ga slušatelj programa HTTP i zasebnom ga slušatelj WebSocket. Svaki ga slušatelj dobiti naziv i rezultirajući skup *naziv: adresa* parove predstavljen kao objekt JSON kad klijent zatraži listening adrese za instancu servisa ili particije.

U bez praćenja stanja servisa na prekoračenje vraća skup ServiceInstanceListeners. Na ServiceInstanceListener sadrži funkciju da biste stvorili programa ICommunicationListener i daje joj naziv. Za s praćenjem stanja servisa na prekoračenje vraća skup ServiceReplicaListeners. To je malo razlikovati od njezinih bez praćenja stanja postoji zamjena u obliku, jer je ServiceReplicaListener mogućnost da biste otvorili programa ICommunicationListener na sekundarnog replike. Ne samo možete koristiti više slušače komunikacije za uslugu, ali možete odrediti koje slušače prihvaćanje zahtjeva na sekundarnog replike, a koje korisnik osluškuju samo primarni replike.

Ako, na primjer, imate ServiceRemotingListener koja vas vodi RPC poziva samo na primarni replike, a na drugu, prilagođene ga slušatelj koja vas vodi čitanja zahtjeve za sekundarnog replike putem HTTP-a:

```csharp
protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new[]
    {
        new ServiceReplicaListener(context =>
            new MyCustomHttpListener(context),
            "HTTPReadonlyEndpoint",
            true),

        new ServiceReplicaListener(context =>
            this.CreateServiceRemotingListener(context),
            "rpcPrimaryEndpoint",
            false)
    };
}
```

> [AZURE.NOTE] Kada stvarate više slušače za uslugu, svaki ga slušatelj **mora** biti zadani jedinstveni naziv.

Na kraju, opišite krajnje točke koji su potrebni za uslugu u na [servis manifesta](service-fabric-application-model.md) u odjeljku na krajnje točke.

```xml
<Resources>
    <Endpoints>
      <Endpoint Name="WebServiceEndpoint" Protocol="http" Port="80" />
      <Endpoint Name="OtherServiceEndpoint" Protocol="tcp" Port="8505" />
    <Endpoints>
</Resources>

```

Komunikacije ga slušatelj može pristupiti na krajnjoj točki resursima dodijeliti iz na `CodePackageActivationContext` u na `ServiceContext`. Ga slušatelj pa možete pokrenuti slušanje zahtjeva za prilikom otvaranja.

```csharp
var codePackageActivationContext = serviceContext.CodePackageActivationContext;
var port = codePackageActivationContext.GetEndpoint("ServiceEndpoint").Port;

```

> [AZURE.NOTE] Resursi za krajnje točke su uobičajeni cijelog servisnog paketu, a oni su dodijeliti putem servisa tkanina prilikom aktiviranja značajke pakiranja za servis. Više replike servis koji se nalazi u istom ServiceHost mogu zajednički koristiti isti priključak. To znači da ga slušatelj komunikacije treba podržava priključak zajedničko korištenje. Preporučeni način za to je da ga slušatelj komunikacije za particija ID i ID replike/instance generira adresu preslušavanja.

### <a name="service-address-registration"></a>Registracija adresu usluge

Sistemski servis naziva *Servis dodjele naziva* pokreće servis tkanina klastere. Servis dodjele naziva je registru za servise i njihove adrese koji svaku instancu ili replike servisa priključuje na. Kada u `OpenAsync` način je `ICommunicationListener` dovrši, njegov Povratna vrijednost dobiva registrirana na servisu imenovanja. U ovom povratnu vrijednost koja se dobiva objavljenim u članku servis dodjele naziva je niz čija se vrijednost može biti bilo što uopće. Vrijednost ovog niza je što klijenti prikazat će se kada se traži adresu za uslugu iz servisa za imenovanje.

```csharp
public Task<string> OpenAsync(CancellationToken cancellationToken)
{
    EndpointResourceDescription serviceEndpoint = serviceContext.CodePackageActivationContext.GetEndpoint("ServiceEndpoint");
    int port = serviceEndpoint.Port;

    this.listeningAddress = string.Format(
                CultureInfo.InvariantCulture,
                "http://+:{0}/",
                port);
                        
    this.publishAddress = this.listeningAddress.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);
            
    this.webApp = WebApp.Start(this.listeningAddress, appBuilder => this.startup.Invoke(appBuilder));
    
    // the string returned here will be published in the Naming Service.
    return Task.FromResult(this.publishAddress);
}
```

Servis tkanina nudi API-ji koji omogućuje klijenata i ostale servise zatim zatražite tu adresu tako da naziv usluge. To je važno jer adresa servisa nije statične. Usluge premještaju se klasteru opterećenja i dostupnosti svrhe resursa. Ovo je mehanizam koji klijentima omogućuje razriješiti listening adresu za uslugu.

> [AZURE.NOTE] Za potpuni walk-through kako pisati programa `ICommunicationListener`, pogledajte [API -JA servisa tkanina Web services s OWIN koja se sama hosting](service-fabric-reliable-services-communication-webapi.md)

## <a name="communicating-with-a-service"></a>Komunikacija sa servisom
Pouzdan API servisa nudi sljedeće biblioteke pisati klijenti za komunikaciju sa servisima.

### <a name="service-endpoint-resolution"></a>Servis za krajnje točke razlučivosti
Prvi je korak u komunikacije sa servisom je da biste riješili adrese krajnjoj točki particija ili instanca servisa koji želite razgovarati s. Na `ServicePartitionResolver` utility klasa je osnovni prim koji olakšava klijenti za određivanje krajnjoj točci servisa prilikom izvođenja. U terminologija servisa tkanina postupak utvrđivanja krajnjoj točci servisa zove *razlučivost krajnja točka servisa*.

Da biste se povezuju sa servisima unutar klaster, `ServicePartitionResolver` moguće stvoriti pomoću zadane postavke. To je preporučuje korištenje za većinu situacija:

```csharp
ServicePartitionResolver resolver = ServicePartitionResolver.GetDefault();
```

Da biste se povezuju sa servisima u različitim klaster, na `ServicePartitionResolver` moguće stvoriti pomoću skupa krajnje točke klaster pristupnika. Imajte na umu da krajnje točke pristupnika samo različite krajnje točke za povezivanje s istom klaster. Ako, na primjer:

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver("mycluster.cloudapp.azure.com:19000", "mycluster.cloudapp.azure.com:19001");
```

Osim toga, `ServicePartitionResolver` može biti zadano funkcije za stvaranje na `FabricClient` da biste koristili interno: 
 
```csharp
public delegate FabricClient CreateFabricClientDelegate();
```

`FabricClient`je objekt koji se koristi za komunikaciju s klaster tkanina servisa za razne operacije upravljanja na klaster. To je korisno ako želite veću kontrolu nad kako `ServicePartitionResolver` stupi u interakciju s svoj klaster. `FabricClient`izvodi predmemoriranje interno te je obično da biste stvorili, stoga je važno da biste ponovno koristili `FabricClient` instance najveću moguću. 

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver(() => CreateMyFabricClient());
```

Način za rješavanje koristi se za dohvaćanje adresu servisa ili particije servisa za particioniranom servise.

```csharp
ServicePartitionResolver resolver = ServicePartitionResolver.GetDefault();

ResolvedServicePartition partition =
    await resolver.ResolveAsync(new Uri("fabric:/MyApp/MyService"), new ServicePartitionKey(), cancellationToken);
```

Adresa servisa može riješiti pomoću programa `ServicePartitionResolver`, ali još posla je potrebna da bi riješi adresa mogu se ispravno. Klijent ćete prepoznati li pokušaja povezivanja nije uspjela zbog tranzitne pogreške i možete pokušati (npr., servis premještene ili je privremeno nedostupan), ili trajna pogreška (npr., servis je izbrisan ili traženi resurs više ne postoji). Instance servisa ili replike možete pomicati čvorovi čvor u bilo kojem trenutku više razloga. Adresa servisa razriješiti kroz `ServicePartitionResolver` možda zastarjele po vremenu kod klijent pokušava povezati. U tom slučaju ponovno klijent morat ćete ponovno razriješiti adresu. Pružanje prethodnih `ResolvedServicePartition` upućuje na to da se Razrješavanje nije potrebno ponovno umjesto jednostavno dohvaćanje predmemorirani adresu.

Obično kod klijenta potrebna rad s na `ServicePartitionResolver` izravno. To se stvara i proslijeđen komunikacije factories klijenta u pouzdanog API servisa. Na factories interno koristiti za razrješavanje za generiranje klijentski objekt koji se mogu koristiti za komunikaciju sa servisima.

### <a name="communication-clients-and-factories"></a>Klijenti za komunikaciju i factories

Biblioteka tvorničke komunikacije implementira uobičajeni uzorak pokušaj kvara obradu koji olakšava ponovnog pokušaja povezivanja s krajnje točke riješi servisa. Biblioteka tvorničke nudi mehanizam pokušaj davanja rukovatelja pogreške.

`ICommunicationClientFactory`definira osnovni sučelja pomoću tvorničke klijent komunikacije koja daje klijenti koje možete razgovarati s uslugom tkanina servisa. Implementacija u CommunicationClientFactory ovisi o snopu komunikacije tkanina servisa servis koristi gdje klijent želi. Omogućuje pouzdanog API servisa na `CommunicationClientFactoryBase<TCommunicationClient>`. Ovo omogućuje osnovni implementaciji sustava na `ICommunicationClientFactory` sučelje i izvršava zadatke koji su zajednički sve snop komunikacije. (Obuhvaćaju sljedeće zadatke pomoću programa `ServicePartitionResolver` da biste odredili krajnju točku usluge). Klijenti obično implementirati Apstraktni klase CommunicationClientFactoryBase učiniti logiku koja se odnose na hrpu komunikacije.

Klijent komunikacije samo prima adresu i koristi za povezivanje s uslugom. Klijent možete koristiti bilo kakve protokol u sklopu.

```csharp
class MyCommunicationClient : ICommunicationClient
{
    public ResolvedServiceEndpoint Endpoint { get; set; }

    public string ListenerName { get; set; }

    public ResolvedServicePartition ResolvedServicePartition { get; set; }
}
```

Klijent tvorničke prvenstveno je zadužen za stvaranje klijenti komunikacije. Za klijente koji ne održavati stalni veze, kao što je klijent za HTTP na tvorničke samo potrebno je stvoriti i vratili se klijent. Ostali protokoli koji održavati stalni veze, kao što su neki binarni protokoli treba moguće provjeriti i tako da tvorničke da biste odredili hoće li vezu potrebno ponovno stvara.  

```csharp
public class MyCommunicationClientFactory : CommunicationClientFactoryBase<MyCommunicationClient>
{
    protected override void AbortClient(MyCommunicationClient client)
    {
    }

    protected override Task<MyCommunicationClient> CreateClientAsync(string endpoint, CancellationToken cancellationToken)
    {
    }

    protected override bool ValidateClient(MyCommunicationClient clientChannel)
    {
    }

    protected override bool ValidateClient(string endpoint, MyCommunicationClient client)
    {
    }
}
```

Na kraju, rukovatelja iznimku je reponsible za određivanje koje akcija koja se izvodi kad dođe do iznimke. Iznimke su kategorizirani u **retriable** i **koje nisu retriable**. 

 - **Osobe koje nisu retriable** iznimke jednostavno se ponovno izbačena natrag da biste pozivatelju. 
 - U **tranzitne** i **koje nisu prolaznim**dodatno kategorizirane **Retriable** iznimke.
  - **Tranzitne** iznimke su oni koje možete jednostavno ponoviti bez ponovno rješavanje adresa krajnje točke usluge. Te se neće sadržavati tranzitne problema s mrežom ili odgovora na servis pogreške osim onih koji upućuju na krajnjoj točki adresa servisa ne postoji. 
  - **Osobe koje nisu prolaznim** iznimke su oni koje je potrebno adresu krajnje točke servisa da biste se ponovno riješi. Uključuju iznimke koje označavaju krajnja točka servisa nije moguće pristupiti, koji označava servis je premještena na drugi čvor. 

Na `TryHandleException` čini odluku o određenom iznimke. Ako ga **ne znate** što odluke da biste o iznimku, ona mora vratiti **false**. Ako je **znali** što odluke da biste, on mora sukladno tome postaviti rezultat i vratili **true**.
 
```csharp
class MyExceptionHandler : IExceptionHandler
{
    public bool TryHandleException(ExceptionInformation exceptionInformation, OperationRetrySettings retrySettings, out ExceptionHandlingResult result)
    {
        // if exceptionInformation.Exception is known and is transient (can be retried without re-resolving)
        result = new ExceptionHandlingRetryResult(exceptionInformation.Exception, true, retrySettings, retrySettings.DefaultMaxRetryCount);
        return true;


        // if exceptionInformation.Exception is known and is not transient (indicates a new service endpoint address must be resolved)
        result = new ExceptionHandlingRetryResult(exceptionInformation.Exception, false, retrySettings, retrySettings.DefaultMaxRetryCount);
        return true;

        // if exceptionInformation.Exception is unknown (let the next IExceptionHandler attempt to handle it)
        result = null;
        return false;
    }
}
```
### <a name="putting-it-all-together"></a>Izgradnja je sve
Pomoću programa `ICommunicationClient`, `ICommunicationClientFactory`, i `IExceptionHandler` ugrađeni oko komunikacijski protokol na `ServicePartitionClient` je je sve zajedno prelama i njihovi petlje razlučivost adresu kvara obradu i servisni particija oko te komponente.

```csharp
private MyCommunicationClientFactory myCommunicationClientFactory;
private Uri myServiceUri;

var myServicePartitionClient = new ServicePartitionClient<MyCommunicationClient>(
    this.myCommunicationClientFactory,
    this.myServiceUri,
    myPartitionKey);

var result = await myServicePartitionClient.InvokeWithRetryAsync(async (client) =>
   {
      // Communicate with the service using the client.
   },
   CancellationToken.None);

```

## <a name="next-steps"></a>Daljnji koraci
 - Pogledajte primjer HTTP komunikacije između servisa [uzorka projekt na GitHUb](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/master/Services/WordCount).

 - [Poziva udaljene procedure s remoting pouzdanog Services](service-fabric-reliable-services-communication-remoting.md)

 - [API koja koristi OWIN pouzdanog Services na webu](service-fabric-reliable-services-communication-webapi.md)

 - [WCF komunikacije pomoću pouzdanog Services](service-fabric-reliable-services-communication-wcf.md)
