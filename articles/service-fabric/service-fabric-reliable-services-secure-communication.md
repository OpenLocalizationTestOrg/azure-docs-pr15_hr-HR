<properties
   pageTitle="Pomoć za sigurnu komunikaciju za servise u servis tkanina | Microsoft Azure"
   description="Pregled uputa za pomoć za sigurnu komunikaciju za pouzdan servise koji su pokrenuti na klasteru tkanina servisa Azure."
   services="service-fabric"
   documentationCenter=".net"
   authors="suchiagicha"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="07/06/2016"
   ms.author="suchiagicha"/>

# <a name="help-secure-communication-for-services-in-azure-service-fabric"></a>Pomoć za sigurnu komunikaciju za servise u tkanina servisa Azure

Sigurnost je jedan od najvažnijih aspekte komunikacije. Aplikacija framework pouzdanog Services nudi nekoliko gotovih komunikacije snop i alate koje možete koristiti za poboljšanje sigurnosti. U ovom se članku obraćajte se o poboljšanju sigurnosti kada koristite remoting servisa i stoga komunikacije Windows Communication Foundation (WCF).

## <a name="help-secure-a-service-when-youre-using-service-remoting"></a>Zaštita servisa kada koristite remoting servisa

Ne možemo hoćete li koristiti postojeće [primjer](service-fabric-reliable-services-communication-remoting.md) kojem se objašnjava kako postaviti remoting za pouzdan servise. Da biste na servis za sigurnu kada koristite remoting servisa, slijedite ove korake:

1. Stvaranje sučelja, `IHelloWorldStateful`, koji definira metode koje će biti dostupna tijekom poziva udaljene procedure o vašoj usluzi. Na servisu će koristiti `FabricTransportServiceRemotingListener`, koji je deklariran u na `Microsoft.ServiceFabric.Services.Remoting.FabricTransport.Runtime` prostor naziva. Ovo je programa `ICommunicationListener` implementaciju koja nudi remoting mogućnosti.

    ```csharp
    public interface IHelloWorldStateful : IService
    {
        Task<string> GetHelloWorld();
    }

    internal class HelloWorldStateful : StatefulService, IHelloWorldStateful
    {
        protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
        {
            return new[]{
                    new ServiceReplicaListener(
                        (context) => new FabricTransportServiceRemotingListener(context,this))};
        }

        public Task<string> GetHelloWorld()
        {
            return Task.FromResult("Hello World!");
        }
    }
    ```

2. Dodajte ga slušatelj postavke i sigurnosne vjerodajnice.

    Provjerite je li certifikat koji želite koristiti radi zaštite vaše komunikacije servis je instalirano sve čvorove u klasteru. Da ga slušatelj postavke i sigurnosne vjerodajnice možete unijeti na dva načina:

    1. Pošaljite im izravno u kod usluge:

        ```csharp
        protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
        {
            FabricTransportListenerSettings listenerSettings = new FabricTransportListenerSettings
            {
                MaxMessageSize = 10000000,
                SecurityCredentials = GetSecurityCredentials()
            };
            return new[]
            {
                new ServiceReplicaListener(
                    (context) => new FabricTransportServiceRemotingListener(context,this,listenerSettings))
            };
        }

        private static SecurityCredentials GetSecurityCredentials()
        {
            // Provide certificate details.
            var x509Credentials = new X509Credentials
            {
                FindType = X509FindType.FindByThumbprint,
                FindValue = "4FEF3950642138446CC364A396E1E881DB76B48C",
                StoreLocation = StoreLocation.LocalMachine,
                StoreName = "My",
                ProtectionLevel = ProtectionLevel.EncryptAndSign
            };
            x509Credentials.RemoteCommonNames.Add("ServiceFabric-Test-Cert");
            return x509Credentials;
        }
        ```
    2. Pošaljite im pomoću [konfiguracije paketa](service-fabric-application-model.md):

        Dodavanje u `TransportSettings` sekcija u datoteci settings.xml.

        ```xml
        <!--Section name should always end with "TransportSettings".-->
        <!--Here we are using a prefix "HelloWorldStateful".-->
        <Section Name="HelloWorldStatefulTransportSettings">
            <Parameter Name="MaxMessageSize" Value="10000000" />
            <Parameter Name="SecurityCredentialsType" Value="X509" />
            <Parameter Name="CertificateFindType" Value="FindByThumbprint" />
            <Parameter Name="CertificateFindValue" Value="4FEF3950642138446CC364A396E1E881DB76B48C" />
            <Parameter Name="CertificateStoreLocation" Value="LocalMachine" />
            <Parameter Name="CertificateStoreName" Value="My" />
            <Parameter Name="CertificateProtectionLevel" Value="EncryptAndSign" />
            <Parameter Name="CertificateRemoteCommonNames" Value="ServiceFabric-Test-Cert" />
        </Section>
        ```

        U ovom slučaju na `CreateServiceReplicaListeners` način izgledat će ovako:

        ```csharp
        protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
        {
            return new[]
            {
                new ServiceReplicaListener(
                    (context) => new FabricTransportServiceRemotingListener(
                        context,this,FabricTransportListenerSettings.LoadFrom("HelloWorldStateful")))
            };
        }
        ```

         Ako dodate na `TransportSettings` sekcija u datoteci settings.xml bez prefiksa, `FabricTransportListenerSettings` učitava sve postavke iz ovog odjeljka prema zadanim postavkama.

         ```xml
         <!--"TransportSettings" section without any prefix.-->
         <Section Name="TransportSettings">
             ...
         </Section>
         ```
         U ovom slučaju na `CreateServiceReplicaListeners` način izgledat će ovako:

         ```csharp
         protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
         {
             return new[]
             {
                 return new[]{
                         new ServiceReplicaListener(
                             (context) => new FabricTransportServiceRemotingListener(context,this))};
             };
         }
         ```

3. Kada se uključujete metode zaštićenim servis pomoću stog remoting umjesto korištenja u `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxy` predmete da biste stvorili proxy usluge, korištenje `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxyFactory`. Prenesite `FabricTransportSettings`, koja sadrži `SecurityCredentials`.

    ```csharp

    var x509Credentials = new X509Credentials
    {
        FindType = X509FindType.FindByThumbprint,
        FindValue = "4FEF3950642138446CC364A396E1E881DB76B48C",
        StoreLocation = StoreLocation.LocalMachine,
        StoreName = "My",
        ProtectionLevel = ProtectionLevel.EncryptAndSign
    };
    x509Credentials.RemoteCommonNames.Add("ServiceFabric-Test-Cert");

    FabricTransportSettings transportSettings = new FabricTransportSettings
    {
        SecurityCredentials = x509Credentials,
    };

    ServiceProxyFactory serviceProxyFactory = new ServiceProxyFactory(
        (c) => new FabricTransportServiceRemotingClientFactory(transportSettings));

    IHelloWorldStateful client = serviceProxyFactory.CreateServiceProxy<IHelloWorldStateful>(
        new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

    Ako je kod klijenta radi kao dio usluge, možete učitati `FabricTransportSettings` iz datoteke settings.xml. Stvaranje sekcije TransportSettings koja je slična kod usluge kao što je prikazano neke starije verzije. Da biste kod klijenta, izvršite sljedeće promjene:

    ```csharp

    ServiceProxyFactory serviceProxyFactory = new ServiceProxyFactory(
        (c) => new FabricTransportServiceRemotingClientFactory(FabricTransportSettings.LoadFrom("TransportSettingsPrefix")));

    IHelloWorldStateful client = serviceProxyFactory.CreateServiceProxy<IHelloWorldStateful>(
        new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

    Ako klijent nije pokrenut kao dio usluge, možete stvoriti datoteku client_name.settings.xml na istom mjestu gdje se nalazi na client_name.exe. Zatim stvorite TransportSettings sekciju u toj datoteci.

    Slično servisa, ako dodajete na `TransportSettings` sekcija bez sve prefiks u settings.xml/client_name.settings.xml klijent `FabricTransportSettings` učitava sve postavke iz ovog odjeljka prema zadanim postavkama.

    U tom slučaju starijim kod dodatno paran pojednostavnjeni:  

    ```csharp

    IHelloWorldStateful client = ServiceProxy.Create<IHelloWorldStateful>(
                 new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

## <a name="help-secure-a-service-when-youre-using-a-wcf-based-communication-stack"></a>Zaštita servisa kada koristite WCF standardnim komunikacijskim stogu

Ne možemo hoćete li koristiti postojeće [primjer](service-fabric-reliable-services-communication-wcf.md) kojem se objašnjava kako postaviti WCF standardnim komunikacijskim stog za pouzdan servise. Da biste na servis za sigurnu kada koristite WCF standardnim komunikacijskim snop, slijedite ove korake:

1. Servis, najprije morate zaštititi ga slušatelj WCF komunikacije (`WcfCommunicationListener`) koje ste stvorili. Da biste to učinili, izmijenite u `CreateServiceReplicaListeners` način.

    ```csharp
    protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
    {
        return new[]
        {
            new ServiceReplicaListener(
                this.CreateWcfCommunicationListener)
        };
    }

    private WcfCommunicationListener<ICalculator> CreateWcfCommunicationListener(StatefulServiceContext context)
    {
       var wcfCommunicationListener = new WcfCommunicationListener<ICalculator>(
            serviceContext:context,
            wcfServiceObject:this,
            // For this example, we will be using NetTcpBinding.
            listenerBinding: GetNetTcpBinding(),
            endpointResourceName:"WcfServiceEndpoint");

        // Add certificate details in the ServiceHost credentials.
        wcfCommunicationListener.ServiceHost.Credentials.ServiceCertificate.SetCertificate(
            StoreLocation.LocalMachine,
            StoreName.My,
            X509FindType.FindByThumbprint,
            "9DC906B169DC4FAFFD1697AC781E806790749D2F");
        return wcfCommunicationListener;
    }

    private static NetTcpBinding GetNetTcpBinding()
    {
        NetTcpBinding b = new NetTcpBinding(SecurityMode.TransportWithMessageCredential);
        b.Security.Message.ClientCredentialType = MessageCredentialType.Certificate;
        return b;
    }
    ```

2. U klijentu, u `WcfCommunicationClient` klase koja je stvorena u prethodnom [primjeru](service-fabric-reliable-services-communication-wcf.md) ostaje nepromijenjen. No morate odbaciti na `CreateClientAsync` način `WcfCommunicationClientFactory`:

    ```csharp
    public class SecureWcfCommunicationClientFactory<TServiceContract> : WcfCommunicationClientFactory<TServiceContract> where TServiceContract : class
    {
        private readonly Binding clientBinding;
        private readonly object callbackObject;
        public SecureWcfCommunicationClientFactory(
            Binding clientBinding,
            IEnumerable<IExceptionHandler> exceptionHandlers = null,
            IServicePartitionResolver servicePartitionResolver = null,
            string traceId = null,
            object callback = null)
            : base(clientBinding, exceptionHandlers, servicePartitionResolver,traceId,callback)
        {
            this.clientBinding = clientBinding;
            this.callbackObject = callback;
        }

        protected override Task<WcfCommunicationClient<TServiceContract>> CreateClientAsync(string endpoint, CancellationToken cancellationToken)
        {
            var endpointAddress = new EndpointAddress(new Uri(endpoint));
            ChannelFactory<TServiceContract> channelFactory;
            if (this.callbackObject != null)
            {
                channelFactory = new DuplexChannelFactory<TServiceContract>(
                this.callbackObject,
                this.clientBinding,
                endpointAddress);
            }
            else
            {
                channelFactory = new ChannelFactory<TServiceContract>(this.clientBinding, endpointAddress);
            }
            // Add certificate details to the ChannelFactory credentials.
            // These credentials will be used by the clients created by
            // SecureWcfCommunicationClientFactory.  
            channelFactory.Credentials.ClientCertificate.SetCertificate(
                StoreLocation.LocalMachine,
                StoreName.My,
                X509FindType.FindByThumbprint,
                "9DC906B169DC4FAFFD1697AC781E806790749D2F");
            var channel = channelFactory.CreateChannel();
            var clientChannel = ((IClientChannel)channel);
            clientChannel.OperationTimeout = this.clientBinding.ReceiveTimeout;
            return Task.FromResult(this.CreateWcfCommunicationClient(channel));
        }
    }
    ```

    Korištenje `SecureWcfCommunicationClientFactory` da biste stvorili WCF komunikacije klijenta (`WcfCommunicationClient`). Koristite klijent za pozivanje metode servisa.

    ```csharp
    IServicePartitionResolver partitionResolver = ServicePartitionResolver.GetDefault();

    var wcfClientFactory = new SecureWcfCommunicationClientFactory<ICalculator>(clientBinding: GetNetTcpBinding(), servicePartitionResolver: partitionResolver);

    var calculatorServiceCommunicationClient =  new WcfCommunicationClient(
        wcfClientFactory,
        ServiceUri,
        ServicePartitionKey.Singleton);

    var result = calculatorServiceCommunicationClient.InvokeWithRetryAsync(
        client => client.Channel.Add(2, 3)).Result;
    ```

## <a name="next-steps"></a>Daljnji koraci

* [API s OWIN pouzdanog Services na webu](service-fabric-reliable-services-communication-webapi.md)
