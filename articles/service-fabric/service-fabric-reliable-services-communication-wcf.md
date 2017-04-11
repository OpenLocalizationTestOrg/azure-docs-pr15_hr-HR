<properties
   pageTitle="Pouzdan stogu komunikacije WCF servisa | Microsoft Azure"
   description="Ugrađeni stog komunikacije WCF u tkanina servisa omogućuje komunikaciju WCF klijentski servis za pouzdan servise."
   services="service-fabric"
   documentationCenter=".net"
   authors="BharatNarasimman"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="07/26/2016"
   ms.author="bharatn"/>

# <a name="wcf-based-communication-stack-for-reliable-services"></a>WCF standardnim komunikacijskim stog pouzdanog Services
Pouzdani servisi framework omogućuje autora servisa da biste odabrali stog komunikacije koje ih želite koristiti za svoje servisa. Možete uključiti u stogu komunikacije po izboru putem **ICommunicationListener** vratio metode [CreateServiceReplicaListeners ili CreateServiceInstanceListeners](service-fabric-reliable-services-communication.md) . Pruža implementacija stog komunikacije koji se temelji na sustava Windows Communication Foundation (WCF) za autore servis koji želite koristiti utemeljen na WCF komunikacije.

## <a name="wcf-communication-listener"></a>Ga Slušatelj WCF komunikacije
Implementacija specifične za WCF **ICommunicationListener** osigurava **Microsoft.ServiceFabric.Services.Communication.Wcf.Runtime.WcfCommunicationListener** predmete.

Lest izgovorite imamo ugovoru vrste`ICalculator`

```csharp
[ServiceContract]
public interface ICalculator
{
    [OperationContract]
    Task<int> Add(int value1, int value2);
}
```

Na ga slušatelj komunikacije WCF možemo stvoriti u servisu na sljedeći način.

```csharp

protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new[] { new ServiceReplicaListener((context) =>
        new WcfCommunicationListener<ICalculator>(
            wcfServiceObject:this,
            serviceContext:context,
            //
            // The name of the endpoint configured in the ServiceManifest under the Endpoints section
            // that identifies the endpoint that the WCF ServiceHost should listen on.
            //
            endpointResourceName: "WcfServiceEndpoint",

            //
            // Populate the binding information that you want the service to use.
            //
            listenerBinding: WcfUtility.CreateTcpListenerBinding()
        )
    )};
}

```

## <a name="writing-clients-for-the-wcf-communication-stack"></a>Klijenti za stog WCF komunikacije za pisanje
Pisanje klijenti mogli komunicirati sa servisima pomoću WCF framework predstavlja **WcfClientCommunicationFactory**, što je implementacije specifične za WCF [ClientCommunicationFactoryBase](service-fabric-reliable-services-communication.md).

```csharp

public WcfCommunicationClientFactory(
    Binding clientBinding = null,
    IEnumerable<IExceptionHandler> exceptionHandlers = null,
    IServicePartitionResolver servicePartitionResolver = null,
    string traceId = null,
    object callback = null);
```

WCF komunikacijskog kanala možete pristupiti iz **WcfCommunicationClient** stvorio **WcfCommunicationClientFactory**.

```csharp

public class WcfCommunicationClient : ServicePartitionClient<WcfCommunicationClient<ICalculator>>
   {
       public WcfCommunicationClient(ICommunicationClientFactory<WcfCommunicationClient<ICalculator>> communicationClientFactory, Uri serviceUri, ServicePartitionKey partitionKey = null, TargetReplicaSelector targetReplicaSelector = TargetReplicaSelector.Default, string listenerName = null, OperationRetrySettings retrySettings = null)
           : base(communicationClientFactory, serviceUri, partitionKey, targetReplicaSelector, listenerName, retrySettings)
       {
       }
   }

```

Kod klijenta možete koristiti **WcfCommunicationClientFactory** zajedno s **WcfCommunicationClient** koji implementira **ServicePartitionClient** da bi utvrdio krajnju točku usluge i komunicirati s uslugom.

```csharp
// Create binding
Binding binding = WcfUtility.CreateTcpClientBinding();
// Create a partition resolver
IServicePartitionResolver partitionResolver = ServicePartitionResolver.GetDefault();
// create a  WcfCommunicationClientFactory object.
var wcfClientFactory = new WcfCommunicationClientFactory<ICalculator>
    (clientBinding: binding, servicePartitionResolver: partitionResolver);

//
// Create a client for communicating with the ICalculator service that has been created with the
// Singleton partition scheme.
//
var calculatorServiceCommunicationClient =  new WcfCommunicationClient(
                wcfClientFactory,
                ServiceUri,
                ServicePartitionKey.Singleton);

//
// Call the service to perform the operation.
//
var result = calculatorServiceCommunicationClient.InvokeWithRetryAsync(
                client => client.Channel.Add(2, 3)).Result;

```
>[AZURE.NOTE] Zadani ServicePartitionResolver pretpostavlja da klijent nije ostalo na isti kao servis. Ako odnosno nije slučaj, stvorite objekt ServicePartitionResolver i prenesite krajnje točke za klaster veze.

## <a name="next-steps"></a>Daljnji koraci
* [Poziva udaljene procedure s remoting pouzdanog Services](service-fabric-reliable-services-communication-remoting.md)

* [API s OWIN pouzdanog Services na webu](service-fabric-reliable-services-communication-webapi.md)

* [Osiguravanje komunikacije za pouzdan servise](service-fabric-reliable-services-secure-communication.md)
