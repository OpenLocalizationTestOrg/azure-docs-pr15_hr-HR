<properties 
    pageTitle="Praktični vodič preusmjeravanja servisa Bus | Microsoft Azure"
    description="Sastavljanje klijent Bus servisa aplikacija i servisa putem servisa Bus preusmjeravanja."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="tysonn" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/27/2016"
    ms.author="sethm" />

# <a name="service-bus-relay-tutorial"></a>Praktični vodič preusmjeravanja Bus servisa

Pomoću ovog praktičnog vodiča opisuje način stvaranja jednostavne servisa Bus klijentska aplikacija i servisa pomoću mogućnosti "preusmjeravanja" Bus servisa. Odgovarajući vodič koji koristi servis Bus [brokered porukama](../service-bus-messaging/service-bus-messaging-overview.md#Brokered-messaging), potražite u članku [Servis Bus Brokered porukama .NET vodič](../service-bus-messaging/service-bus-brokered-tutorial-dotnet.md).

Rad pomoću ovog praktičnog vodiča pruža poznavati koraci koje su potrebne za stvaranje servisa Bus aplikacije klijenta i usluga. Kao što je verzija njihove WCF usluga je konstrukta koji se izlaže jedan ili više krajnje točke, od kojih svaka izlaže operacije servisa. Krajnja točka servisa određuje adresu gdje servis moguće je pronaći, povezivanje s podacima koji se klijent mora komunicirati s servis i ugovora koji definira funkcionalnost nudi uslugu da biste svojim klijentima. Glavna razlika između programa WCF i servis za servis Bus je prikazuje li se na krajnjoj točki u oblak umjesto lokalno na vašem računalu.

Kada se kroz slijed teme pomoću ovog praktičnog vodiča, imat ćete pokrenuti servis, a klijentsko možete pozvati operacije servisa. Prvi temi opisuje kako postaviti račun. Sljedeći koraci opisuju kako definirati servis koji koristi ugovor, kako implementirati usluge i upute za konfiguriranje servisa u kodu. Koje opisuju i kako hostira i pokrenuti servis. Servis koji je stvoren je koja se sama glavnom računalu i klijenta i usluge pokreću se na istom računalu. Možete konfigurirati servis pomoću koda ili konfiguracijska datoteka.

Konačni tri koraka opisuju kako stvoriti klijentska aplikacija, konfiguriranje klijentska aplikacija i stvaranje i korištenje klijenta koji mogu pristupiti funkciju glavnog računala.

Sve teme u ovom odjeljku pretpostavlja da koristite Visual Studio kao okruženje za razvoj.

## <a name="sign-up-for-an-account"></a>Registracija za račun

U prvi je korak da biste stvorili prostor naziva i dobiti ključ zajednički pristup potpis (SAS). Prostor naziva nudi programa granicu aplikacije za svaku aplikaciju vidljiva kroz Bus servisa. Ključ SAS automatski se generira sustav stvaranja polje naziva servisa. Polje naziva servisa i SAS ključ nudi vjerodajnice za Bus servis za provjeru autentičnosti pristup aplikaciji.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="define-a-wcf-service-contract-to-use-with-service-bus"></a>Definiranje WCF ugovoru za uporabu Bus servisa

Ugovor o usluzi određuje koje operacije podržava servis (Web servisa terminologija za metode ili funkcije). Ugovori se stvaraju definiranjem C++, C# ili Visual Basic sučelja. Svaki način u sučelju odgovara određene operacije servisa. Svaki sučelja mora imati atribut [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) primijenjeno i svaki operacija mora imati atribut [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) primijenjeno. Ako metoda u sučelje koje je atribut [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) atribut [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) , taj način ne prikazuje se. U ovom primjeru pomoću postupka opisanog navedeni kod za sljedeće zadatke. Raspravi veće ugovore i usluga potražite u članku [dizajniranje i servise za implementaciju](https://msdn.microsoft.com/library/ms729746.aspx) u dokumentaciji WCF.

### <a name="to-create-a-service-bus-contract-with-an-interface"></a>Da biste stvorili Bus servisa ugovora sučelja

1. Otvorili Visual Studio kao administrator, desnom tipkom miša kliknete na izbornik **Start** , a zatim odaberite **Pokreni kao administrator**.

2. Stvaranje novog projekta konzole za aplikacije. Kliknite izbornik **datoteka** i odaberite **Novo**, a zatim kliknite **projekt**. U dijaloškom okviru **Novi projekt** kliknite **Visual C#** (Ako **Visual C#** ne prikazuje, pogledajte u odjeljku **Drugih jezika**). Kliknite predložak **Aplikacije konzole** i nazovite ih **EchoService**. Kliknite **u redu** da biste stvorili projekta.

    ![][2]

3. Instalirajte paket NuGet Bus servisa. Paket automatski dodaje reference na biblioteke Bus servisa, kao i WCF **System.ServiceModel**. [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) je prostor za naziv koji vam omogućuje da programski pristup osnovne se značajke WCF. Servis Bus koristi brojne objekti i atributi WCF da biste definirali ugovorima.

    U pregledniku rješenja, desnom tipkom miša kliknite rješenje, a zatim **Upravljanje NuGet paketa rješenja**. Kliknite karticu **Pregled** , a zatim potražite `Microsoft Azure Service Bus`. Provjerite je li u okviru **verzije** odabran naziva projekta. Kliknite **Instaliraj**i prihvatite uvjete korištenja.

    ![][3]

3. U pregledniku rješenja, dvokliknite datoteku Program.cs da biste ga otvorili u uređivaču, ako već nije otvoren.

4. Dodajte sljedeće pomoću naredbe pri vrhu datoteke:

    ```
    using System.ServiceModel;
    using Microsoft.ServiceBus;
    ```

1. Promijenite polja naziva iz njegova zadani naziv **EchoService** u **Microsoft.ServiceBus.Samples**.

    >[AZURE.IMPORTANT] Pomoću ovog praktičnog vodiča koristi C# naziva **Microsoft.ServiceBus.Samples**, što je prostor za naziv vrste Ugovor upravlja koja se koristi u konfiguracijskoj datoteci u koraku [Konfiguriraj WCF klijenta](#configure-the-wcf-client) . Možete odrediti bilo koje polje naziva želite da se prilikom stvaranja uzorka; Međutim, vodič neće funkcionirati ako zatim izmijenite prostori ugovora i servisa sukladno tome, u datoteci konfiguracije računala. Prostor za naziv naveden u datoteci App.config mora biti jednak prostora za naziv naveden u datotekama C#.

1. Odmah nakon na `Microsoft.ServiceBus.Samples` deklariranje prostor naziva, ali unutar naziva, definirajte novo sučelje pod nazivom `IEchoContract` i primijenite na `ServiceContractAttribute` atributa sučelja s vrijednošću prostor naziva **http://samples.microsoft.com/ServiceModel/Relay/**. Vrijednost polja naziva se razlikuje od naziva koji koristite cijeloj opseg koda. Umjesto toga, vrijednost polja naziva služi kao jedinstveni identifikator za ovaj ugovor. Određivanje naziva izričito onemogućuje zadane vrijednosti polja naziva dodat naziv ugovora.

    ```
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
    }
    ```

    >[AZURE.NOTE] Obično naziva ugovora servisa sadrži imenovanja sheme koja sadrži informacije o verziji. Uključujući informacije o verziji u ugovoru naziva servisa omogućuje servise izdvojiti glavnih promjena definiranje nove ugovoru s novi prostor za naziv i će na krajnju točku. Na taj način klijente možete nastaviti koristiti staru ugovor o usluzi bez potrebe za može ažurirati. Informacije o verziji može se sastojati od datuma ili broja Sastavi. Dodatne informacije potražite u članku [Rad s verzijama servisa](http://go.microsoft.com/fwlink/?LinkID=180498). Za potrebe ovog praktičnog vodiča, imenovanja sheme prostora za naziv ugovora servisa ne sadrži informacije o verziji.

1. Unutar na `IEchoContract` sučelja, deklarirati način jedne operacije u `IEchoContract` ugovora izlaže u sučelju i primijeniti na `OperationContractAttribute` atributa na način na koji želite da biste kao dio javnog servisa Bus ugovora.

    ```
    [OperationContract]
    string Echo(string text);
    ```

1. Odmah nakon na `IEchoContract` sučelja definicija, deklarirati kanala koje nasljeđuju iz obje `IEchoContract` i na i na `IClientChannel` sučelja, kao što je prikazano ovdje:

    ```
    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```

    Kanal je objekt WCF putem kojega glavnom i klijentskom prosljeđivanje podataka međusobno povezani. Naknadno će pisanje koda protiv kanala jeku informacija između dvije aplikacije.

1. Na izborniku **Sastavljanje** kliknite **Stvaranje rješenja** ili pritisnite **Ctrl + Shift + B** da biste potvrdili točnost svoje poslovne dosad.

### <a name="example"></a>Primjer

Sljedeći kod prikazuje osnovni sučelja koja definira Bus servisa ugovora.

```
using System;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

Sad kad je sučelje stvorili, možete implementirati sučelje.

## <a name="implement-the-wcf-contract-to-use-service-bus"></a>Implementacija WCF ugovor za korištenje servisa Bus

Stvaranje Bus usluge preusmjeravanja zahtijeva prvi put stvorite ugovor, koja je definirana pomoću sučelja. Dodatne informacije o stvaranju sučelja potražite u prethodnom koraku. Sljedeći je korak za implementaciju sučelje. To uključuje stvaranje klase pod nazivom `EchoService` koji se primjenjuje na korisnički definirane `IEchoContract` sučelja. Nakon što implementirate sučelje, zatim konfigurirati sučelja koristite datoteku konfiguracije App.config. Konfiguracijska datoteka sadrži potrebne informacije za aplikaciju, kao što je naziv servisa, naziv ugovora i vrstu protokol koji se koristi za komunikaciju s Bus servisa. U ovom primjeru pomoću postupka opisanog navedeni kod koji se koristi za sljedeće zadatke. Općenite raspravu o implementaciji ugovoru potražite u članku [Implementacijom ugovorima](https://msdn.microsoft.com/library/ms733764.aspx) u dokumentaciji WCF.

1. Stvaranje nove predmete pod nazivom `EchoService` neposredno iza definicije na `IEchoContract` sučelja. Na `EchoService` klase primjenjuje na `IEchoContract` sučelja. 

    ```
    class EchoService : IEchoContract
    {
    }
    ```
    
    Slično druge implementacije sučelja, definiciju možete implementirati u neku drugu datoteku. No za ovog praktičnog vodiča implementacije nalazi se u istu datoteku kao definiciju sučelja i `Main` način.

1. Primjena atribut [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) na `IEchoContract` sučelja. Atribut određuje naziv usluge i polja naziva. Kada to učinite, u `EchoService` predmete pojavit će se na sljedeći način:

    ```
    [ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
    }
    ```

1. Implementacija u `Echo` način koji su definirani u na `IEchoContract` sučelja u na `EchoService` predmete. 

    ```
    public string Echo(string text)
    {
        Console.WriteLine("Echoing: {0}", text);
        return text;
    }
    ```

1. Kliknite **Stvaranje**, a zatim kliknite **Stvaranje rješenja** da biste potvrdili točnost svoj rad.

### <a name="to-define-the-configuration-for-the-service-host"></a>Da biste definirali konfiguraciju za servis glavnog računala

1. Konfiguracijska datoteka vrlo je sličan WCF konfiguracijska datoteka. Obuhvaća naziv usluge, krajnje točke (to jest, mjesto servisa Bus izlaže za klijente i domaćini komunikaciju s drugom) i povezivanje (Vrsta protokol koji se koristi za komunikaciju). Glavna razlika je krajnje točke u ovom konfigurirani usluge na [NetTcpRelayBinding](https://msdn.microsoft.com/library/azure/microsoft.servicebus.nettcprelaybinding.aspx) povezivanja koja nije dio .NET Framework. [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) jedan je od povezivanja definira Bus servisa.

1. U **Pregledniku rješenja**, dvokliknite datoteku App.config da biste ga otvorili u uređivaču za Visual Studio.

2. U na `<appSettings>` element, zamjena rezerviranih mjesta pod nazivom prostora za naziv servisa i na SAS ključ koji ste kopirali u prethodnom koraku. 

1. Unutar na `<system.serviceModel>` dodajte oznake na `<services>` element. Možete definirati više Bus servisa aplikacija u datoteci jedan konfiguracije. Međutim, pomoću ovog praktičnog vodiča definira samo jedan.
 
    ```
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <services>

        </services>
      </system.serviceModel>
    </configuration>
    ```

1. Unutar na `<services>` element, dodajte je `<service>` elementa koji želite definirati naziv servisa.

    ```
    <service name="Microsoft.ServiceBus.Samples.EchoService">
    </service>
    ```

1. Unutar na `<service>` element, odrediti mjesto ugovor o krajnjoj točki i vrstu povezivanja za krajnju točku.

    ```
    <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding"/>
    ```

    Krajnja točka definira gdje će izgledati klijent za aplikaciju glavnog računala. Naknadno vodič koristi ovaj korak da biste stvorili URI koji potpuno izlaže glavnom računalu putem servisa Bus. Povezivanje deklarira da ne možemo koristite TCP kao protokol za komunikaciju s Bus servisa.

1. Na izborniku **Sastavljanje** kliknite **Stvaranje rješenja** da biste potvrdili točnost svoj rad.

### <a name="example"></a>Primjer

Sljedeći kod prikazuje implementacije ugovor o usluzi.

```
[ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]

    class EchoService : IEchoContract
    {
        public string Echo(string text)
        {
            Console.WriteLine("Echoing: {0}", text);
            return text;
        }
    }
```

Sljedeći kod prikazuje osnovni oblik datoteke App.config pridružene servis glavnog računala.

```
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <services>
      <service name="Microsoft.ServiceBus.Samples.EchoService">
        <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding" />
      </service>
    </services>
    <extensions>
      <bindingExtensions>
        <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
      </bindingExtensions>
    </extensions>
  </system.serviceModel>
</configuration>
```

## <a name="host-and-run-a-basic-web-service-to-register-with-service-bus"></a>Glavno računalo i pokretanje osnovni web-servisa za registriranje Bus servisa

Ovaj korak opisuje kako pokrenuti osnovna servisa Bus usluga.

### <a name="to-create-the-service-bus-credentials"></a>Da biste stvorili vjerodajnice Bus servisa

1. U `Main()`, stvoriti dvije varijable u koju želite pohraniti naziva i na SAS ključ koji se čitaju iz prozora konzole.

    ```
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS key: ");
    string sasKey = Console.ReadLine();
    ```

    Tipku SAS kasnije će se koristiti za pristup projekta Bus servisa. Prostor za naziv je proslijeđena kao parametar `CreateServiceUri` da biste stvorili servisa URI.

4. Korištenje [TransportClientEndpointBehavior](https://msdn.microsoft.com/library/microsoft.servicebus.transportclientendpointbehavior.aspx) objekt deklarirati koji ćete koristiti ključ SAS kao vrstu vjerodajnica. Dodajte sljedeći kod odmah nakon koda dodali u posljednjem koraku.

    ```
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```

### <a name="to-create-a-base-address-for-the-service"></a>Da biste stvorili osnovne adrese za servis

1. Praćenje kod koji ste dodali u posljednjem koraku stvoriti na `Uri` instanca za osnovne adrese servisa. U ovom URI određuje sheme Bus servisa, naziva i put sučelje servisa.

    ```
    Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
    ```

    "sb" je kratica za servis Bus shemu i ukazuje na to da ne možemo koristite TCP kao protokol. To je i prethodno naznačen u konfiguracijskoj datoteci kada [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) navedeno je kao vezu.
    
    U ovom ćete praktičnom vodiču URI nije `sb://putServiceNamespaceHere.windows.net/EchoService`.

### <a name="to-create-and-configure-the-service-host"></a>Stvaranje i konfiguriranje servis glavnog računala

1. Postavite u način povezivanja na `AutoDetect`.

    ```
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```

    Način povezivanja opisuju protokol koji se servis koristi za komunikaciju s Bus servisa; HTTP ili TCP. Pomoću zadane postavke `AutoDetect`, servis pokušava povezati s Bus servisa putem TCP ako je dostupan i HTTP ako TCP nije dostupna. Napomena da to razlikuje se od protokol servis određuje za komunikaciju klijenta. Da protokol ovisi o povezivanju koristi. Usluge, na primjer, možete koristiti [BasicHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.basichttprelaybinding.aspx) povezivanje koji se navodi da njezin krajnju točku (koji se prikazuje na servis Bus) komunicira s klijentima putem HTTP-a. Isti servis ne može odrediti **ConnectivityMode.AutoDetect** tako da se servis komunicira s Bus servisa putem TCP-a.

1. Stvaranje glavno računalo za usluge, korištenje URI stvorili ranije u ovom odjeljku.

    ```
    ServiceHost host = new ServiceHost(typeof(EchoService), address);
    ```

    Glavno računalo za servis je objekt WCF koji instancira servis. Ovdje, proslijedite ga vrstu servis koji želite stvoriti (programa `EchoService` vrsta), a i na željenu adresu prikazivanje usluge.

1. Pri vrhu datoteke Program.cs, dodajte reference na [System.ServiceModel.Description](https://msdn.microsoft.com/library/system.servicemodel.description.aspx) i [Microsoft.ServiceBus.Description](https://msdn.microsoft.com/library/microsoft.servicebus.description.aspx).

    ```
    using System.ServiceModel.Description;
    using Microsoft.ServiceBus.Description;
    ```

1. Vratite se u `Main()`, konfiguriranje krajnju točku da biste omogućili pristup javno.

    ```
    IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);
    ```

    Ovaj korak obavještava Bus usluge koje aplikacije mogu biti pronašao javno Provjera Bus ATOM servisa sažetak sadržaja za projekt. Ako ste postavili **DiscoveryType** na **Privatno**, klijent će i dalje moći pristup servisu. Međutim, servis ne izgledati kada se pretražuje prostora za naziv Bus servisa. Umjesto toga, klijent promijenile znati prije toga put krajnjoj točki.

1. Primjena vjerodajnice servisa krajnje točke servisa definirana u datoteci App.config:

    ```
    foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
    {
        endpoint.Behaviors.Add(serviceRegistrySettings);
        endpoint.Behaviors.Add(sasCredential);
    }
    ```

    Prema uputama u prethodnom koraku, koji može imati deklarirati više servisa i krajnje točke u konfiguracijskoj datoteci. Ako imate, kod bi prolaziti konfiguracijska datoteka i pretraživanje za svaku krajnju točku na koju treba primijeniti vjerodajnice. No za ovog praktičnog vodiča konfiguracijska datoteka sadrži samo jedan krajnjoj točki.

### <a name="to-open-the-service-host"></a>Da biste otvorili servis glavnog računala

1. Otvorite servis.

    ```
    host.Open();
    ```

1. Obavještavanje korisnika je pokrenut servis, a u članku se objašnjava kako isključiti servis.

    ```
    Console.WriteLine("Service address: " + address);
    Console.WriteLine("Press [Enter] to exit");
    Console.ReadLine();
    ```

1. Kada završite, zatvorite servis glavnog računala.

    ```
    host.Close();
    ```

1. Pritisnite **Ctrl + Shift + B** da biste sastavili projekta.

### <a name="example"></a>Primjer

U sljedećem primjeru sadrži ugovoru i implementaciju iz prethodnih koraka u ovom praktičnom vodiču, a hostira uslugu u program konzole. Kompiliranje sljedeće u datoteku pod nazivom EchoService.exe.

```
using System;
using System.ServiceModel;
using System.ServiceModel.Description;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Description;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { };

    [ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
        public string Echo(string text)
        {
            Console.WriteLine("Echoing: {0}", text);
            return text;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {

            ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;         
          
            Console.Write("Your Service Namespace: ");
            string serviceNamespace = Console.ReadLine();
            Console.Write("Your SAS key: ");
            string sasKey = Console.ReadLine();

           // Create the credentials object for the endpoint.
            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            // Create the service URI based on the service namespace.
            Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            // Create the service host reading the configuration.
            ServiceHost host = new ServiceHost(typeof(EchoService), address);

            // Create the ServiceRegistrySettings behavior for the endpoint.
            IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);

            // Add the Service Bus credentials to all endpoints specified in configuration.
            foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
            {
                endpoint.Behaviors.Add(serviceRegistrySettings);
                endpoint.Behaviors.Add(sasCredential);
            }
            
            // Open the service.
            host.Open();

            Console.WriteLine("Service address: " + address);
            Console.WriteLine("Press [Enter] to exit");
            Console.ReadLine();

            // Close the service.
            host.Close();
        }
    }
}
```

## <a name="create-a-wcf-client-for-the-service-contract"></a>Stvaranje WCF klijent za ugovor o uslugama

Sljedeći je korak da biste stvorili osnovni servis Bus klijentska aplikacija i definirati ugovor o usluzi će implementirati u noviji korake. Napomena mnoge od ovih koraka oblik sličan korake koji se koriste za stvaranje usluge: definiranje ugovor, u App.config za uređivanje datoteka pomoću vjerodajnica za povezivanje s Bus servisa i tako dalje. U ovom primjeru pomoću postupka opisanog navedeni kod koji se koristi za sljedeće zadatke.

1. Stvaranje novog projekta u trenutnom rješenju Visual Studio za klijentski program tako da učinite sljedeće:
    1. U Eksploreru za rješenja u istom rješenje koje sadrži servisa, desnom tipkom miša kliknite trenutno rješenje (ne projekt), a zatim kliknite **Dodaj**. Kliknite **Novi projekt**.
    2. U dijaloškom okviru **Dodavanje novog projekta** kliknite **Visual C#** (Ako **Visual C#** ne prikazuje, pogledajte u odjeljku **Drugih jezika**), odaberite predložak **Aplikacije konzole** i nazovite ih **EchoClient**.
    3. Kliknite **u redu**.
<br />

1. U pregledniku rješenja, dvokliknite datoteku Program.cs u projektu **EchoClient** da biste ga otvorili u uređivaču, ako već nije otvoren.

1. Promjena polja naziva iz njegova zadani naziv `EchoClient` da biste `Microsoft.ServiceBus.Samples`.

1. Instalirajte [servis Bus NuGet paketa](https://www.nuget.org/packages/WindowsAzure.ServiceBus). U pregledniku rješenja, desnom tipkom miša kliknite **EchoClient** projekta, a zatim **Upravljanje NuGet paketa**. Kliknite karticu **Pregled** , a zatim potražite `Microsoft Azure Service Bus`. Kliknite **Instaliraj**i prihvatite uvjete korištenja.

    ![][3]

1. Dodavanje u `using` izjava za [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) naziva u datoteci Program.cs. 

    ```
    using System.ServiceModel;
    ```

1. Dodajte definiciju ugovora servisa naziva, kao što je prikazano u sljedećem primjeru. Imajte na umu da ova definicija jednako definiciju koristiti u programu project **servisa** . Trebali biste dodati kod pri vrhu na `Microsoft.ServiceBus.Samples` prostor naziva.

    ```
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```

1. Pritisnite **Ctrl + Shift + B** da biste sastavili klijent.

### <a name="example"></a>Primjer

Sljedeći kod prikazuje se trenutni status Program.cs datoteke u programu project EchoClient.

```
using System;
using Microsoft.ServiceBus;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }


    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

## <a name="configure-the-wcf-client"></a>Konfiguriranje klijenta WCF

U ovom ćete koraku stvoriti datoteku App.config za osnovni klijentska aplikacija koja se može pristupiti servisa stvoreno ovog praktičnog vodiča. Datoteka App.config definira ugovora, povezivanje i naziv krajnje točke. U ovom primjeru pomoću postupka opisanog navedeni kod koji se koristi za sljedeće zadatke.

1. U pregledniku rješenja u programu project **EchoClient** dvokliknite **App.config** da biste otvorili datoteku u uređivaču za Visual Studio.

2. U na `<appSettings>` element, zamjena rezerviranih mjesta pod nazivom prostora za naziv servisa i na SAS ključ koji ste kopirali u prethodnom koraku.

1. Unutar elementa system.serviceModel dodati na `<client>` element.

    ```
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <client>
        </client>
      </system.serviceModel>
    </configuration>
    ```

    Ovaj korak deklarira da definirate klijentska aplikacija WCF stil.

1. Unutar na `client` element, definirajte naziv, ugovora i vrstu povezivanja za krajnju točku.

    ```
    <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IEchoContract"
                    binding="netTcpRelayBinding"/>
    ```

    Ovaj korak definira naziv krajnju točku ugovora definirano u servis i fact klijentska aplikacija koristi TCP možete komunicirati s Bus servisa. Naziv krajnje točke da biste se povezali tu konfiguraciju krajnju točku sa servisom URI se koristi u sljedećem koraku.

1. Kliknite **datoteka**, a potom **Spremi sve**.

## <a name="example"></a>Primjer

Sljedeći kod prikazuje App.config datoteka za klijentski program jeku.

```
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <client>
      <endpoint name="RelayEndpoint"
                      contract="Microsoft.ServiceBus.Samples.IEchoContract"
                      binding="netTcpRelayBinding"/>
    </client>
    <extensions>
      <bindingExtensions>
        <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
      </bindingExtensions>
    </extensions>
  </system.serviceModel>
</configuration>
```

## <a name="implement-the-wcf-client-to-call-service-bus"></a>Implementacija klijent WCF da biste nazvali Bus servisa

U ovom ćete koraku implementirati osnovni klijentska aplikacija koja se može pristupiti servis koji ste prethodno stvorili pomoću ovog praktičnog vodiča. Slično servis, klijent izvodi mnoge iste operacije da biste pristupili Bus servisa:

1. Određuje način povezivanja.
1. Stvara URI koji pronalazi servis glavnog računala.
1. Definira sigurnosne vjerodajnice.
1. Primjenjuje vjerodajnice za povezivanje.
1. Otvorit će se veza.
1. Izvodi zadatke specifičnim aplikacijama.
1. Zatvara vezu.

Međutim, jedna od glavne razlike jest da klijentska aplikacija koristi kanal za povezivanje s Bus servisa dok se servis koristi poziv **ServiceHost**. U ovom primjeru pomoću postupka opisanog navedeni kod koji se koristi za sljedeće zadatke.

### <a name="to-implement-a-client-application"></a>Možete implementirati klijentska aplikacija

1. Postavljanje načina povezivanja za **Automatsko otkrivanje**. Dodajte sljedeći kôd unutar na `Main()` način **EchoClient** aplikacije.

    ```
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```

1. Definiranje varijable držati vrijednosti za polje naziva servisa i SAS ključ koji se čitaju konzoli sustava.

    ```
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS Key: ");
    string sasKey = Console.ReadLine();
    ```

1. Stvaranje URI koji definira mjesto glavnog računala u projektu Bus servisa.

    ```
    Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
    ```

1. Stvaranje objekta vjerodajnica za krajnju točku prostora za naziv vaše usluge.

    ```
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```

1. Stvaranje kanala tvorničke koja učitava konfiguracije opisane u datoteci App.config.

    ```
    ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));
    ```

    Kanal tvorničke je WCF objekt koji stvara kanala putem kojega se komunikaciju aplikacije servisa i klijenta.

1. Primjena vjerodajnice Bus servisa.

    ```
    channelFactory.Endpoint.Behaviors.Add(sasCredential);
    ```

1. Stvaranje i otvorite kanal za servis.

    ```
    IEchoChannel channel = channelFactory.CreateChannel();
    channel.Open();
    ```

1. Pisanje osnovni korisničko sučelje i funkcija jeku.

    ```
    Console.WriteLine("Enter text to echo (or [Enter] to exit):");
    string input = Console.ReadLine();
    while (input != String.Empty)
    {
        try
        {
            Console.WriteLine("Server echoed: {0}", channel.Echo(input));
        }
        catch (Exception e)
        {
            Console.WriteLine("Error: " + e.Message);
        }
        input = Console.ReadLine();
    }
    ```

    Imajte na umu da kod koristi instancu objekta kanala kao proxy za uslugu.

1. Zatvorite kanal i zatvorite na tvorničke.

    ```
    channel.Close();
    channelFactory.Close();
    ```

## <a name="to-run-the-applications"></a>Pokretanje aplikacije

1. Pritisnite **Ctrl + Shift + B** da biste sastavili rješenja. To je sastavlja projekta klijenta i project servis koji ste stvorili u prethodnim koracima.

2. Prije pokretanja klijentsku aplikaciju, morate provjeriti je li pokrenut servisnoj aplikaciji. U Eksploreru za rješenja u Visual Studio, desnom tipkom miša kliknite rješenje **EchoService** , a zatim kliknite **Svojstva**.

3. U dijaloškom okviru svojstva rješenja kliknite **Pokretanje projekt**, a zatim kliknite gumb **više projekata prilikom pokretanja** . Provjerite je li **EchoService** prikazuje prvi na popisu. 

4. Da biste **pokrenuli**postavite okvir **Akcije** za **EchoService** i **EchoClient** projekte.

    ![][5]

5. Kliknite **projekt ovisnosti**. U okvir za **projekte** odaberite **EchoClient**. U okviru **Depends na** provjerite je li uključena **EchoService** .

    ![][6]

6. Kliknite **u redu** da biste zatvorili dijaloški okvir **Svojstva** .

7. Pritisnite **F5** da biste pokrenuli i projektima.

8. Oba konzole za windows otvorite i traže polja naziva. Servis morate pokrenuti prvi, pa u prozoru konzole **EchoService** unesite prostor za naziv i pritisnite **Enter**.

9. Nakon toga se od vas zatraži SAS ključ. Unesite ključ SAS i pritisnite ENTER.

    Evo primjera izlaza iz prozora konzole. Imajte na umu da vrijednosti navedene Ovdje ćete, primjerice samo.

    `Your Service Namespace: myNamespace`
    `Your SAS Key: <SAS key value>`

    Servisne aplikacije ispisuje prozor konzole adresu na kojoj je priključuje, kao što se vidi u sljedećem primjeru.

    `Service address: sb://mynamespace.servicebus.windows.net/EchoService/`
    `Press [Enter] to exit`
    
10. U prozoru konzole **EchoClient** unesite iste podatke koje ste prethodno unijeli za servisnu aplikaciju. Slijedite prethodne korake da biste unijeli istu polje naziva servisa i vrijednosti ključa SAS za klijentsku aplikaciju.

11. Nakon unosa tih vrijednosti klijent otvara kanala na servis i traži da unesete neki tekst kao što se vidi u sljedećem primjeru konzole za izlaz.

    `Enter text to echo (or [Enter] to exit):` 

    Unesite neki tekst da biste poslali servisnoj aplikaciji i pritisnite Enter. Tekst na servisu kroz postupak jeka usluge i pojavljuje se u prozoru konzole za servis kao sljedeći primjer izlaz.

    `Echoing: My sample text`

    Klijentska aplikacija dobiva povratnu vrijednost u `Echo` operacija koja je izvorni tekst i ispisuju njegov prozor konzole. Slijedi primjer izlaza iz prozora konzole za klijenta.

    `Server echoed: My sample text`

12. Možete i dalje slanje tekstnih poruka putem klijentskog programa servisa na taj način. Kada završite, pritisnite tipku Enter u konzole za windows klijenta i servisa da biste završili obje aplikacije.

## <a name="example"></a>Primjer

Sljedeći primjer pokazuje kako stvoriti klijentska aplikacija, kako poziva operacije servisa i kako zatvoriti klijent nakon dovršetka postupka poziv.

```
using System;
using Microsoft.ServiceBus;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
            ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;

            
            Console.Write("Your Service Namespace: ");
            string serviceNamespace = Console.ReadLine();
            Console.Write("Your SAS Key: ");
            string sasKey = Console.ReadLine();



            Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));

            channelFactory.Endpoint.Behaviors.Add(sasCredential);

            IEchoChannel channel = channelFactory.CreateChannel();
            channel.Open();

            Console.WriteLine("Enter text to echo (or [Enter] to exit):");
            string input = Console.ReadLine();
            while (input != String.Empty)
            {
                try
                {
                    Console.WriteLine("Server echoed: {0}", channel.Echo(input));
                }
                catch (Exception e)
                {
                    Console.WriteLine("Error: " + e.Message);
                }
                input = Console.ReadLine();
            }

            channel.Close();
            channelFactory.Close();

        }
    }
}
```

## <a name="next-steps"></a>Daljnji koraci

Pomoću ovog praktičnog vodiča prikazivao sastavljanje klijent Bus servisa aplikacija i servisa pomoću mogućnosti "preusmjeravanja" Bus servisa. Slične vodič koji koristi servis Bus [porukama](../service-bus-messaging/service-bus-messaging-overview.md#Brokered-messaging), potražite u članku [Servis Bus Brokered porukama .NET vodič](../service-bus-messaging/service-bus-brokered-tutorial-dotnet.md).

Da biste saznali više o Bus servisa, potražite u sljedećim temama.

- [Bus usluge SMS pregled](../service-bus-messaging/service-bus-messaging-overview.md)
- [Osnove Bus servisa](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
- [Servis Bus arhitekture](../service-bus-messaging/service-bus-architecture.md)

[Azure classic portal]: http://manage.windowsazure.com

[1]: ./media/service-bus-relay-tutorial/service-bus-policies.png
[2]: ./media/service-bus-relay-tutorial/create-console-app.png
[3]: ./media/service-bus-relay-tutorial/install-nuget.png
[4]: ./media/service-bus-relay-tutorial/create-ns.png
[5]: ./media/service-bus-relay-tutorial/set-projects.png
[6]: ./media/service-bus-relay-tutorial/set-depend.png
