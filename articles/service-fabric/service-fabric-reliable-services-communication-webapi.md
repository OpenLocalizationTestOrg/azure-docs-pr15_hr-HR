<properties
   pageTitle="Servis komunikaciju s ASP.NET Web API | Microsoft Azure"
   description="Saznajte kako implementirati pomoću OWIN koja se sama nalaze u pouzdanog API servisa platforme ASP.NET Web API-JA servisa komunikacije korak po korak."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="get-started-service-fabric-web-api-services-with-owin-self-hosting"></a>Početak rada: API Web tkanina usluge usluge s ugovorima o OWIN koja se sama hosting

Azure tkanina servisa power stavlja u rukama kada ste odlučili kako želite da se servisa za komunikaciju s korisnicima i međusobno. Pomoću ovog praktičnog vodiča usredotočuje se na implementacijom komunikacije servis pomoću ASP.NET Web API otvaranje web-sučelja za .NET (OWIN) koja se sama nalaze u servis tkanina pouzdanog Services API-JA. Ne možemo ćete delve Duboko u uključiv komunikacije pouzdanog Services API-JA. Ćemo i koristiti Web API u detaljnim primjeru da bi se prikazala kako postaviti ga slušatelj prilagođene komunikacije.


## <a name="introduction-to-web-api-in-service-fabric"></a>Uvod u API na webu u tkanina servisa

API na webu ASP.NET je popularne i Napredna framework za izgradnju HTTP API-ji pri vrhu .NET Framework. Ako se još ne nalazite poznato framework, potražite u članku [Uvod u ASP.NET Web API 2](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) da biste saznali više.

API na webu u tkanina servis je isti ASP.NET Web API poznajete i volite. Razlika je u tome koje *glavno računalo* Web API aplikacije. Nećete koristiti Microsoft Internet Information Services (IIS). Da biste bolje razumjeli razlike, recimo prelomiti u dva dijela:

 1. Web API aplikacije (uključujući kontrolera i modela)
 2. Glavno računalo (web-poslužitelj, obično IIS)

Ne mijenja same Web API aplikacije. Se ne razlikuje se od Web API aplikacije možda ste napisali u prošlosti, a trebali biste moći jednostavno pokazivačem preko većinu kodu aplikacije. No ako ste nalaze u IIS-u, gdje host aplikacije mogu malo razlikovati od što ste navikli. Prije nego što smo dobili hostinga dio, Započnimo s nešto više poznate: Web API aplikacije.


## <a name="create-the-application"></a>Stvaranje aplikacije

Započnite tako da stvorite novu aplikaciju servisa tkanina s jednom bez praćenja stanja servisa u Visual Studio 2015:

![Stvaranje nove aplikacije servisa tkanina](media/service-fabric-reliable-services-communication-webapi/webapi-newproject.png)

Visual Studio predložak bez praćenja stanja servisa pomoću Web API nije dostupna. U ovom ćete praktičnom vodiču bismo ćete stvoriti projekta Web API ispočetka čiji je rezultat što želite dobiti ako odaberete ovaj predložak.

Odaberite prazan projekt bez praćenja stanja servisa da biste saznali kako izraditi projekta Web API od nule ili možete započeti s predloškom Web API bez praćenja stanja servisa i jednostavno praćenje izlaganja.  

![Stvaranje jednog bez praćenja stanja servisa](media/service-fabric-reliable-services-communication-webapi/webapi-newproject2.png)

U prvi je korak da biste izvukli neke NuGet paketi za API na webu. Paket želimo da biste koristili je Microsoft.AspNet.WebApi.OwinSelfHost. Paket sadrži sve potrebne Web API paketa i paketa *glavnog računala* . To će biti važna kasnije.

![Stvaranje Web API pomoću upravitelja paketima NuGet](media/service-fabric-reliable-services-communication-webapi/webapi-nuget.png)

Nakon instalacije pakete možete početi sastavnih out osnovna struktura projekta Web API. Ako ste iskoristili Web API, strukturu projekta trebao bi izgledati vrlo poznat. Započnite dodavanjem u `Controllers` direktorija i kontroler jednostavne vrijednosti:

**ValuesController.cs**

```csharp
using System.Collections.Generic;
using System.Web.Http;
    
namespace WebService.Controllers
{
    public class ValuesController : ApiController
    {
        // GET api/values 
        public IEnumerable<string> Get()
        {
            return new string[] { "value1", "value2" };
        }

        // GET api/values/5 
        public string Get(int id)
        {
            return "value";
        }

        // POST api/values 
        public void Post([FromBody]string value)
        {
        }

        // PUT api/values/5 
        public void Put(int id, [FromBody]string value)
        {
        }

        // DELETE api/values/5 
        public void Delete(int id)
        {
        }
    }
}

```

Nakon toga dodajte početnu klasu na korijenskom projekta da biste registrirali postupka, formatters i konfiguracija postavljanje. Ovo je i gdje se Web API priključuje na *glavno računalo*, koji će biti kasnije revisited. 

**Startup.CS**

```csharp
using System.Web.Http;
using Owin;

namespace WebService
{
    public static class Startup
    {
        public static void ConfigureApp(IAppBuilder appBuilder)
        {
            // Configure Web API for self-host. 
            HttpConfiguration config = new HttpConfiguration();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );

            appBuilder.UseWebApi(config);
        }
    }
}
```

To je to dijela aplikacije. Sada ćemo postavite samo na Web API projekta izgled. Dosad nisu izgledat mnogo različitih projekata Web API možda ste napisali u prošlosti ili Osnovni predložak Web API. Poslovne logike unose se u kontrolera i modela kao i obično.

Sada što učinite smo poduzeti u vezi hosting tako da se ne možemo zapravo ga mogli pokrenuti?

## <a name="service-hosting"></a>Usluge hostinga

U tkanina servisa na servisu pokreće se u *matični proces servisa*, izvršnu datoteku koja se pokreće kod usluge. Prilikom pisanja na servis pomoću pouzdanog API servisa, servis projekta kompilira samo na izvršnu datoteku koja registrira servis vrsta i pokreće kod. To se događa u većini slučajeva, kada pišete usluge na servis tkanina u .NET. Kada otvorite Program.cs u programu project bez praćenja stanja servisa, trebali biste vidjeti:

```csharp
using System;
using System.Diagnostics;
using System.Threading;
using Microsoft.ServiceFabric.Services.Runtime;

internal static class Program
{
    private static void Main()
    {
        try
        {
            ServiceRuntime.RegisterServiceAsync("WebServiceType",
                context => new WebService(context)).GetAwaiter().GetResult();

            ServiceEventSource.Current.ServiceTypeRegistered(Process.GetCurrentProcess().Id, typeof(WebService).Name);

            // Prevents this host process from terminating so services keeps running. 
            Thread.Sleep(Timeout.Infinite);
        }
        catch (Exception e)
        {
            ServiceEventSource.Current.ServiceHostInitializationFailed(e.ToString());
            throw;
        }
    }
}

```

Ako koji izgleda sumnjivo točku unosa konzole za aplikaciju, to je jer je.

Dodatne pojedinosti o proces servis glavnog računala i servis za registraciju su izvan raspona ovog članka. Dok je važno je znati Zasad tog *servisa kod se izvodi u vlastitom postupak*.

## <a name="self-host-web-api-with-an-owin-host"></a>Samostalno glavno računalo Web API pomoću programa OWIN glavnog računala

Given da kodu aplikacije API na webu nalazi se u vlastitom postupak, kako učiniti privučete ga na web-poslužitelju? Unesite [OWIN](http://owin.org/). OWIN je jednostavno ugovor između .NET web-aplikacije i web-poslužiteljima. Najčešći kada se koristi platforme ASP.NET (najviše MVC 5), web-aplikaciji čvrsto je povezano za IIS kroz sadrži. Međutim, Web API implementira OWIN, pa možete napisati web-aplikacije koja je samostalne na web-poslužitelju koji mu. Zbog toga možete koristiti *koja se sama hostira* OWIN web-poslužitelju koji možete pokrenuti u vlastiti postupak. To odgovara savršeno s tkanina usluge hostinga modela smo samo opisane.

U ovom se članku ćemo koristiti Katana kao glavno računalo OWIN Web API aplikacije. Katana je implementacija Otvori izvor OWIN glavnog računala utemeljena na [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx) i Windows [HTTP poslužitelja API -JA](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).

> [AZURE.NOTE] Da biste saznali više o Katana, otvorite [Katana web-mjesta](http://www.asp.net/aspnet/overview/owin-and-katana/an-overview-of-project-katana). Kratak pregled kako koristiti Katana za samostalno hostiranje Web API potražite u članku [Korištenje OWIN Self-Host ASP.NET Web API 2](http://www.asp.net/web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api).


## <a name="set-up-the-web-server"></a>Postavljanje web-poslužitelj

Pouzdan API servisa omogućuje koji možete umetnuti u snop komunikacije koje omogućuju korisnicima i klijenti da biste se povezali sa servisom komunikacije ulaza:

```csharp

protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    ...
}

```

Web-poslužitelj (i sve druge komunikacije stoga koristite u budućnosti, kao što je WebSockets) trebali biste koristiti sučelja ICommunicationListener pravilno Integracija sa sustavom. Razlozi za to će postati više vidljivu u sljedećim koracima.

Prvo, stvorite predmete naziva OwinCommunicationListener koji implementira ICommunicationListener:

**OwinCommunicationListener.cs**

```csharp
using Microsoft.Owin.Hosting;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Owin;
using System;
using System.Fabric;
using System.Globalization;
using System.Threading;
using System.Threading.Tasks;

namespace WebService
{
    internal class OwinCommunicationListener : ICommunicationListener
    {
        public void Abort()
        {
        }

        public Task CloseAsync(CancellationToken cancellationToken)
        {
        }

        public Task<string> OpenAsync(CancellationToken cancellationToken)
        {
        }
    }
}
```

Sučelje ICommunicationListener pruža tri načina za upravljanje ga slušatelj za komunikaciju servisa:

 - *OpenAsync*. Pokrenite slušanje zahtjeva za.
 - *CloseAsync*. Zaustavi slušanje zahtjeva za, Završi sve in-flight zahtjeve i isključiti bez poteškoća.
 - *Prekid*. Sve otkazivanje i prekid odmah.

Da bismo započeli, dodajte privatne predmete članova za što ga slušatelj potrebno će funkcionirati. Te bit će pokrenuti putem na Graditelj i koristiti kasnije prilikom postavljanja listening URL-a.

```csharp
internal class OwinCommunicationListener : ICommunicationListener
{
    private readonly ServiceEventSource eventSource;
    private readonly Action<IAppBuilder> startup;
    private readonly ServiceContext serviceContext;
    private readonly string endpointName;
    private readonly string appRoot;

    private IDisposable webApp;
    private string publishAddress;
    private string listeningAddress;

    public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName)
        : this(startup, serviceContext, eventSource, endpointName, null)
    {
    }

    public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName, string appRoot)
    {
        if (startup == null)
        {
            throw new ArgumentNullException(nameof(startup));
        }

        if (serviceContext == null)
        {
            throw new ArgumentNullException(nameof(serviceContext));
        }

        if (endpointName == null)
        {
            throw new ArgumentNullException(nameof(endpointName));
        }

        if (eventSource == null)
        {
            throw new ArgumentNullException(nameof(eventSource));
        }

        this.startup = startup;
        this.serviceContext = serviceContext;
        this.endpointName = endpointName;
        this.eventSource = eventSource;
        this.appRoot = appRoot;
    }
   

    ...

```

## <a name="implement-openasync"></a>Implementacija OpenAsync

Da biste postavili web-poslužitelj, potrebne su vam dvije dijelovi informacija:

 - *Prefiks put URL-a odgovora*. Iako to nije obavezno, dobro je za postavljanje to sada tako da sigurno možete hostirati više web-servisi u aplikaciji.
 - *Priključak*.

Prije nego što se priključak za web-poslužitelj, važno je razumjeti da tkanina servis nudi aplikacijskom sloju koji funkcionira kao međuspremnik između aplikacija i Temeljni operacijski sustav koji se izvodi na. Kao takve, tkanina servisa omogućuje da biste konfigurirali *krajnje točke* za usluge. Servis tkanina osigurava krajnje točke dostupnih servisa da biste koristili. Na taj način, ne morate konfigurirati ih sami u podlozi OS okruženju. Aplikacije servisa tkanina u različitim okruženjima jednostavno možete hostirati bez promjene u aplikaciji. (Na primjer, možete hostirati iste aplikacije u Azure ili vlastite podatkovnog centra.)

Konfigurirajte krajnje HTTP PackageRoot\ServiceManifest.xml:

```xml

<Resources>
    <Endpoints>
        <Endpoint Name="ServiceEndpoint" Type="Input" Protocol="http" Port="8281" />
    </Endpoints>
</Resources>

```

Budući da se pokreće matični proces servisa u odjeljku ograničena vjerodajnice (mrežni servis u sustavu Windows), važno je ovaj korak. To znači da na servisu neće imati pristup postavljanje krajnje HTTP na vlastitu. Pomoću konfiguraciju krajnju točku servisa tkanina zna da biste postavili popis za kontrolu pristupa veliko početno slovo (ACL) za URL koji će osluškuju servis. Servis tkanina omogućuje i standardne mjesto da biste konfigurirali krajnje točke.


Vratite se u OwinCommunicationListener.cs, možete pokrenuti implementacijom OpenAsync. Ovo je kojem započnete web-poslužitelj. Prvo se podaci o krajnjoj točki i stvaranje URL koji će osluškuju servis. URL će se razlikovati ovisno o tome koristi li se ga slušatelj bez praćenja stanja servisa ili s praćenjem stanja servisa. S praćenjem stanja servisa ga slušatelj potrebne za stvaranje jedinstvenu adresu za svaku s praćenjem stanja servisa replike se očekuje podatke. Bez praćenja stanja servisa adresu može biti mnogo jednostavnijim. 

```csharp
public Task<string> OpenAsync(CancellationToken cancellationToken)
{
    var serviceEndpoint = this.serviceContext.CodePackageActivationContext.GetEndpoint(this.endpointName);
    var protocol = serviceEndpoint.Protocol;
    int port = serviceEndpoint.Port;

    if (this.serviceContext is StatefulServiceContext)
    {
        StatefulServiceContext statefulServiceContext = this.serviceContext as StatefulServiceContext;

        this.listeningAddress = string.Format(
            CultureInfo.InvariantCulture,
            "{0}://+:{1}/{2}{3}/{4}/{5}",
            protocol,
            port,
            string.IsNullOrWhiteSpace(this.appRoot)
                ? string.Empty
                : this.appRoot.TrimEnd('/') + '/',
            statefulServiceContext.PartitionId,
            statefulServiceContext.ReplicaId,
            Guid.NewGuid());
    }
    else if (this.serviceContext is StatelessServiceContext)
    {
        this.listeningAddress = string.Format(
            CultureInfo.InvariantCulture,
            "{0}://+:{1}/{2}",
            protocol,
            port,
            string.IsNullOrWhiteSpace(this.appRoot)
                ? string.Empty
                : this.appRoot.TrimEnd('/') + '/');
    }
    else
    {
        throw new InvalidOperationException();
    }
    
    ...

```

Imajte na umu da "http://+" koristi u nastavku. To je da biste bili sigurni da je web-poslužitelj priključuje na sve dostupne adrese, uključujući localhost, FQDN i IP računala.

Implementacija OpenAsync jedan je od najvažnijih razloga zašto web-poslužitelj (ili bilo koji stogu komunikacije) je implementirana kao što je ICommunicationListener, a ne samo pojavljuju otvoriti izravno iz `RunAsync()` u servisu. Povratna vrijednost iz OpenAsync je adresa koju web-poslužitelj priključuje na. Kada se u sustav, vraća se tu adresu, registrira adresu sa servisom. Servis tkanina nudi API-JA koji omogućuje klijenata i ostale servise zatim zatražite tu adresu tako da naziv usluge. To je važno jer adresa servisa nije statične. Usluge premještaju se klasteru opterećenja i dostupnosti svrhe resursa. Ovo je mehanizam koji klijentima omogućuje razriješiti listening adresu za uslugu.

Uz to na umu OpenAsync pokreće web-poslužitelj i vraća adresu koja se priključuje na. Napomena da se očekuje podatke na "http://+", ali prije OpenAsync vraća adresu, na "+" je zamijenjena funkcijom IP ili FQDN čvora se trenutno nalazi. Adresa koji je vratio ovu metodu nije registriran na sustavu. Preporučuje se i što klijenti i ostale servise vidjeti kada zatražite adresu na servis. Za klijente s njim povezati pravilno koje su im potrebne stvarni IP ili FQDN adresu.

```csharp
    ...

    this.publishAddress = this.listeningAddress.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);

    try
    {
        this.eventSource.Message("Starting web server on " + this.listeningAddress);

        this.webApp = WebApp.Start(this.listeningAddress, appBuilder => this.startup.Invoke(appBuilder));

        this.eventSource.Message("Listening on " + this.publishAddress);

        return Task.FromResult(this.publishAddress);
    }
    catch (Exception ex)
    {
        this.eventSource.Message("Web server failed to open endpoint {0}. {1}", this.endpointName, ex.ToString());

        this.StopWebServer();

        throw;
    }
}

```

Imajte na umu to odnosi početnu klasu koji je proslijeđen u OwinCommunicationListener u na Graditelj. Web-poslužitelj koristi ovu instancu pokretanja da bi se povezati Web API aplikacije.

Na `ServiceEventSource.Current.Message()` retka prikazat će se u prozoru dijagnostičkih događaja kasnije, prilikom pokretanja aplikacije da biste potvrdili da web-poslužitelj uspješno pokrenut.

## <a name="implement-closeasync-and-abort"></a>Implementacija CloseAsync i prekid

Na kraju, implementirati CloseAsync i prekid da biste prestali web-poslužitelj. Web-poslužitelj može se zaustaviti po rashoda ručicu poslužitelja koji je stvoren tijekom OpenAsync.

```csharp
public Task CloseAsync(CancellationToken cancellationToken)
{
    this.eventSource.Message("Closing web server on endpoint {0}", this.endpointName);
            
    this.StopWebServer();

    return Task.FromResult(true);
}

public void Abort()
{
    this.eventSource.Message("Aborting web server on endpoint {0}", this.endpointName);
    
    this.StopWebServer();
}

private void StopWebServer()
{
    if (this.webApp != null)
    {
        try
        {
            this.webApp.Dispose();
        }
        catch (ObjectDisposedException)
        {
            // no-op
        }
    }
}
```

U ovom primjeru implementaciju CloseAsync i prekid samo zaustavljate web-poslužitelj. Koje se mogu odlučiti za izvođenje obavljanje više usklađenih zatvaranja web-poslužitelja u CloseAsync. Isključivanje računala, na primjer, nije moguće Pričekajte in-flight zahtjevi za dovršiti prije povrata.

## <a name="start-the-web-server"></a>Pokretanje na web-poslužitelj

Sada ste spremni za stvaranje i vraćanje instance komponente OwinCommunicationListener da biste pokrenuli web-poslužitelj. Vratite se u mapu servisa klase (WebService.cs) nadjačati na `CreateServiceInstanceListeners()` metoda:

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    var endpoints = Context.CodePackageActivationContext.GetEndpoints()
                           .Where(endpoint => endpoint.Protocol == EndpointProtocol.Http || endpoint.Protocol == EndpointProtocol.Https)
                           .Select(endpoint => endpoint.Name);

    return endpoints.Select(endpoint => new ServiceInstanceListener(
        serviceContext => new OwinCommunicationListener(Startup.ConfigureApp, serviceContext, ServiceEventSource.Current, endpoint), endpoint));
}
```

To je mjesto Web API *aplikacije* i OWIN *glavnog računala* na kraju ispunjavaju. Host (OwinCommunicationListener) dobiva instance *aplikacije* (Web API) putem početnu klasu. Servis tkanina zatim upravlja njegova životnog ciklusa. Ovaj isti uzorak obično se može pratiti s bilo koje stogu komunikacije.

## <a name="put-it-all-together"></a>Ih objediniti sve

U ovom primjeru, ne morate ništa učiniti na `RunAsync()` način, pa možete jednostavno ukloniti te Odbaci.

Implementacija konačni servisa mora biti vrlo jednostavne. Samo se potrebne za stvaranje ga slušatelj komunikacije:

```csharp
using System;
using System.Collections.Generic;
using System.Fabric;
using System.Fabric.Description;
using System.Linq;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Microsoft.ServiceFabric.Services.Runtime;

namespace WebService
{
    internal sealed class WebService : StatelessService
    {
        public WebService(StatelessServiceContext context)
            : base(context)
        { }

        protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
        {
            var endpoints = Context.CodePackageActivationContext.GetEndpoints()
                                   .Where(endpoint => endpoint.Protocol == EndpointProtocol.Http || endpoint.Protocol == EndpointProtocol.Https)
                                   .Select(endpoint => endpoint.Name);

            return endpoints.Select(endpoint => new ServiceInstanceListener(
                serviceContext => new OwinCommunicationListener(Startup.ConfigureApp, serviceContext, ServiceEventSource.Current, endpoint), endpoint));
        }
    }
}
```

Čitav `OwinCommunicationListener` klasa:

```csharp
using System;
using System.Diagnostics;
using System.Fabric;
using System.Globalization;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.Owin.Hosting;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Owin;

namespace WebService
{
    internal class OwinCommunicationListener : ICommunicationListener
    {
        private readonly ServiceEventSource eventSource;
        private readonly Action<IAppBuilder> startup;
        private readonly ServiceContext serviceContext;
        private readonly string endpointName;
        private readonly string appRoot;

        private IDisposable webApp;
        private string publishAddress;
        private string listeningAddress;

        public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName)
            : this(startup, serviceContext, eventSource, endpointName, null)
        {
        }

        public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName, string appRoot)
        {
            if (startup == null)
            {
                throw new ArgumentNullException(nameof(startup));
            }

            if (serviceContext == null)
            {
                throw new ArgumentNullException(nameof(serviceContext));
            }

            if (endpointName == null)
            {
                throw new ArgumentNullException(nameof(endpointName));
            }

            if (eventSource == null)
            {
                throw new ArgumentNullException(nameof(eventSource));
            }

            this.startup = startup;
            this.serviceContext = serviceContext;
            this.endpointName = endpointName;
            this.eventSource = eventSource;
            this.appRoot = appRoot;
        }

        public Task<string> OpenAsync(CancellationToken cancellationToken)
        {
            var serviceEndpoint = this.serviceContext.CodePackageActivationContext.GetEndpoint(this.endpointName);
            var protocol = serviceEndpoint.Protocol;
            int port = serviceEndpoint.Port;

            if (this.serviceContext is StatefulServiceContext)
            {
                StatefulServiceContext statefulServiceContext = this.serviceContext as StatefulServiceContext;

                this.listeningAddress = string.Format(
                    CultureInfo.InvariantCulture,
                    "{0}://+:{1}/{2}{3}/{4}/{5}",
                    protocol,
                    port,
                    string.IsNullOrWhiteSpace(this.appRoot)
                        ? string.Empty
                        : this.appRoot.TrimEnd('/') + '/',
                    statefulServiceContext.PartitionId,
                    statefulServiceContext.ReplicaId,
                    Guid.NewGuid());
            }
            else if (this.serviceContext is StatelessServiceContext)
            {
                this.listeningAddress = string.Format(
                    CultureInfo.InvariantCulture,
                    "{0}://+:{1}/{2}",
                    protocol,
                    port,
                    string.IsNullOrWhiteSpace(this.appRoot)
                        ? string.Empty
                        : this.appRoot.TrimEnd('/') + '/');
            }
            else
            {
                throw new InvalidOperationException();
            }

            this.publishAddress = this.listeningAddress.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);

            try
            {
                this.eventSource.Message("Starting web server on " + this.listeningAddress);

                this.webApp = WebApp.Start(this.listeningAddress, appBuilder => this.startup.Invoke(appBuilder));

                this.eventSource.Message("Listening on " + this.publishAddress);

                return Task.FromResult(this.publishAddress);
            }
            catch (Exception ex)
            {
                this.eventSource.Message("Web server failed to open endpoint {0}. {1}", this.endpointName, ex.ToString());

                this.StopWebServer();

                throw;
            }
        }

        public Task CloseAsync(CancellationToken cancellationToken)
        {
            this.eventSource.Message("Closing web server on endpoint {0}", this.endpointName);

            this.StopWebServer();

            return Task.FromResult(true);
        }

        public void Abort()
        {
            this.eventSource.Message("Aborting web server on endpoint {0}", this.endpointName);

            this.StopWebServer();
        }

        private void StopWebServer()
        {
            if (this.webApp != null)
            {
                try
                {
                    this.webApp.Dispose();
                }
                catch (ObjectDisposedException)
                {
                    // no-op
                }
            }
        }
    }
}
```

Sad kad ste umetnuli svih dijelova na mjestu, projekta trebao bi izgledati kao tipičnog računala Web API s točke unosa pouzdanog API servisa i programa OWIN glavnog računala:


![API na webu s točke unosa pouzdanog API servisa i OWIN glavnog računala](media/service-fabric-reliable-services-communication-webapi/webapi-projectstructure.png)

## <a name="run-and-connect-through-a-web-browser"></a>Pokrenite i povežite se putem web-preglednika

Ako još niste učinili, [postavite razvojno okruženje](service-fabric-get-started.md).


Sada možete izraditi i implementacija usluge. Pritisnite **F5** u Visual Studio omogućuje stvaranje i implementaciju aplikacija. U prozoru dijagnostičkih događaja, trebali biste vidjeti poruku koja označava da je web-poslužitelj otvoriti na http://localhost:8281 /.


![Prozor za Visual Studio dijagnostičkih događaja](media/service-fabric-reliable-services-communication-webapi/webapi-diagnostics.png)

> [AZURE.NOTE] Ako neki drugi proces na računalu već otvoren priključak, možda ćete vidjeti do pogreške. To znači da ga slušatelj nije moguće otvoriti. Ako je to slučaj, pokušajte koristiti drugi priključak za konfiguraciju krajnju točku u ServiceManifest.xml.


Kada je pokrenut servis, otvorite preglednik, a zatim otvorite [api/http://localhost:8281/vrijednosti](http://localhost:8281/api/values) da biste ga testirali.

## <a name="scale-it-out"></a>Promjena veličine ga

Obično skaliranje odgovor bez praćenja stanja web-aplikacije znači da dodate više računala, a koji se vrti gore web-aplikacije na njima. Modul djelovanje servisa tkanina to možete učiniti za vas kad god se novi čvorovi dodaju se klaster. Kada stvorite pojavljivanja bez praćenja stanja servisa, možete odrediti koliko je instanci koju želite stvoriti. Servis tkanina postavlja taj broj instanci na čvorovi u klasteru. I jamči da se neće stvarati više instanci na bilo kojem jedan čvor. Možete uputiti i tkanina servisa da biste uvijek stvoriti instancu na svakoj čvor navođenjem **-1** za broj instanci. Time će se jamčiti da svaki put kada dodate čvorove skaliranja out svoj klaster, instance bez praćenja stanja servisa na stvorit će se novi čvorovi. Ta vrijednost je svojstvo instance servisa tako da je postavljen prilikom stvaranja instanca servisa. To možete učiniti putem komponente PowerShell:

```powershell

New-ServiceFabricService -ApplicationName "fabric:/WebServiceApplication" -ServiceName "fabric:/WebServiceApplication/WebService" -ServiceTypeName "WebServiceType" -Stateless -PartitionSchemeSingleton -InstanceCount -1

```

To možete učiniti i kada definirate zadanu uslugu u projektu Visual Studio bez praćenja stanja servisa:

```xml

<DefaultServices>
  <Service Name="WebService">
    <StatelessService ServiceTypeName="WebServiceType" InstanceCount="-1">
      <SingletonPartition />
    </StatelessService>
  </Service>
</DefaultServices>

```

Dodatne informacije o stvaranju aplikacije i instanci servisa potražite u članku [uvođenja aplikacije](service-fabric-deploy-remove-applications.md).

## <a name="next-steps"></a>Daljnji koraci

[Ispravljanje pogrešaka aplikacije usluge tkanina pomoću Visual Studio](service-fabric-debugging-your-application.md)
