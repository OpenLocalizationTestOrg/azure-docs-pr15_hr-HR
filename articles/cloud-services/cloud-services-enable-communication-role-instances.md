<properties 
pageTitle="Komunikacije s ulogama u servise u Oblaku | Microsoft Azure" 
description="Uloga pojave u servise u Oblaku može imati krajnje točke (http, https, tcp, udp) za njih definirali koji komunikaciju s vanjski ili između ostalih instanci uloge." 
services="cloud-services" 
documentationCenter="" 
authors="Thraka" 
manager="timlt" 
editor=""/>
<tags 
ms.service="cloud-services" 
ms.workload="tbd" 
ms.tgt_pltfrm="na" 
ms.devlang="na" 
ms.topic="article" 
ms.date="09/06/2016" 
ms.author="adegeo"/>

# <a name="enable-communication-for-role-instances-in-azure"></a>Omogućivanje komunikacije instanci uloga u azure

Uloge servisa oblaka komunikaciju putem interne i vanjske veze. Vanjske veze nazivaju se **unos krajnje točke** dok interne veze nazivaju **Interna krajnje točke**. U ovoj se temi opisuje kako izmjena [definicija servisa](cloud-services-model-and-package.md#csdef) da biste stvorili krajnje točke.


## <a name="input-endpoint"></a>Krajnja točka za unos
Krajnju točku unosa koristi se kada želite izložiti priključak za vanjski. Navedite vrstu protokol i priključak krajnju točku koja se primjenjuje se za oba vanjskih i internih priključke za krajnju točku. Ako želite, možete odrediti drugi Interni priključak za krajnju točku atribut [localPort](https://msdn.microsoft.com/library/azure/gg557552.aspx#InputEndpoint) .

Krajnju točku unosa možete koristiti sljedeće protokole: **http, https, tcp, udp**.

Da biste stvorili krajnje točke unosa, dodajte podređeni element **InputEndpoint** element **krajnje točke** weba ili tempiranja uloge.

```xml
<Endpoints>
  <InputEndpoint name="StandardWeb" protocol="http" port="80" localPort="80" />
</Endpoints> 
```

## <a name="instance-input-endpoint"></a>Unos krajnjoj točki instanci
Unos krajnje točke instanci su slične unos krajnje točke ali omogućuje mapirati određene dostupnog javnosti priključke za svaku instancu pojedinačnih uloga pomoću prosljeđivanje priključka na raspoređivača opterećenja. Možete navesti jedan priključak dostupnog javnosti ili raspon priključaka.

Krajnja točka unosa instance možete koristiti samo **tcp** i **udp** kao protokol.

Da biste stvorili instancu krajnje točke unosa, dodajte podređeni element **InstanceInputEndpoint** element **krajnje točke** weba ili tempiranja uloge.

```xml
<Endpoints>
  <InstanceInputEndpoint name="Endpoint2" protocol="tcp" localPort="10100">
    <AllocatePublicPortFrom>
      <FixedPortRange max="10109" min="10105" />
    </AllocatePublicPortFrom>
  </InstanceInputEndpoint>
</Endpoints>
```

## <a name="internal-endpoint"></a>Interna krajnje točke
Interna krajnje točke dostupni su za instancu instancu komunikacije. Priključak nije obavezan, a ako se ispusti, dinamični priključak dodijeljene krajnju točku. Raspon može se koristiti. Postoji ograničenje od pet Interna krajnje točke svaku ulogu.

Interna krajnjoj točki možete koristiti sljedeće protokole: **http, tcp i udp, sve**.

Da biste stvorili Interna krajnje točke unosa, dodajte podređeni element **InternalEndpoint** element **krajnje točke** weba ili tempiranja uloge.

```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any" port="8999" />
</Endpoints> 
```

Možete koristiti i raspon.

```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any">
    <FixedPortRange max="8995" min="8999" />
  </InternalEndpoint>
</Endpoints>
```


## <a name="worker-roles-vs-web-roles"></a>Uloga suradnika nasuprot uloge Web

Postoji jedan manji razlika s krajnje točke prilikom rada s tempiranja i ulogama za web. Uloga web, morate imati barem jedan unos krajnju točku koja se pomoću protokola **HTTP** .


```xml
<Endpoints>
  <InputEndpoint name="StandardWeb" protocol="http" port="80" localPort="80" />
  <!-- more endpoints may be declared after the first InputEndPoint -->
</Endpoints>
```

## <a name="using-the-net-sdk-to-access-an-endpoint"></a>Pristup krajnje putem .NET SDK
Biblioteka za upravlja Azure pruža metode uloga instance komunikaciju prilikom izvođenja. Iz kod koji se izvodi u ulozi instancu, možete dohvatiti podatke o postojanje parametra druge instance uloga i njihovih krajnje točke, kao i informacije o trenutnoj instanci ulogu.

> [AZURE.NOTE] Samo možete dohvatiti podatke o uloga instance koji su pokrenuti na servis u oblaku i koje definirali barem jedan Interna krajnjoj točki. Nije moguće dohvatiti podatke o instanci uloge koje su pokrenute na drugi servis.

Svojstvo [instance](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.role.instances.aspx) možete koristiti za dohvaćanje pojavljivanja uloge. Prvi put koristite [CurrentRoleInstance](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.currentroleinstance.aspx) za vraćanje reference na trenutnoj instanci uloga, a zatim pomoću svojstvo [uloga](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstance.role.aspx) vraćanje reference s ulogom sam.

Kada se povežete s uloga instance programski kroz .NET SDK, relativno jednostavno je za pristup podacima krajnjoj točki. Ako, na primjer, nakon već ste povezali s okruženju posebnu ulogu, prikazat će vam priključak određene krajnje točke s kod:

```csharp
int port = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["StandardWeb"].IPEndpoint.Port;
```

Svojstvo **instance** vraća skup **RoleInstance** objekte. Zbirka uvijek sadrži trenutne instance. Ako ulogu definirate krajnje Interna, zbirke obuhvaća trenutne instance, ali nema instanci. Koliko je instanci uloga u zbirci uvijek će biti 1 u slučaju gdje će se bez Interna krajnjoj točki definirana uloge. Ako ulogu određuje internu krajnje točke, njegov instance vidljivim prilikom izvođenja i koliko je instanci u zbirci će odgovarati broj instanci za uloga u konfiguracijskoj datoteci servisa.

> [AZURE.NOTE] Azure upravlja biblioteke ne nudi način određivanja stanja druge instance uloge, ali možete implementirati takve stanje konfiguracije sami ako uslugu treba tu funkciju. [Dijagnostika Azure](cloud-services-dotnet-diagnostics.md) možete koristiti da biste dobili informacije o pokrenute instance uloge.

Da biste utvrdili broj priključka za interne krajnju točku uloga instanci, svojstvo [InstanceEndpoints](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstance.instanceendpoints.aspx) možete koristiti da biste se vratili u rječnik objekt koji sadrži nazive krajnjoj točki i njihove odgovarajuće IP adresa i priključaka. Svojstvo [IPEndpoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstanceendpoint.ipendpoint.aspx) vraća IP adresa i priključak za navedeni krajnjoj točki. Svojstvo **PublicIPEndpoint** vraća priključak za krajnju točku rasporediti opterećenje. Dio IP adresa **PublicIPEndpoint** svojstvo se koristi.

Slijedi primjer iterativna uloga instance.

```csharp
foreach (RoleInstance roleInst in RoleEnvironment.CurrentRoleInstance.Role.Instances)
{
    Trace.WriteLine("Instance ID: " + roleInst.Id);
    foreach (RoleInstanceEndpoint roleInstEndpoint in roleInst.InstanceEndpoints.Values)
    {
        Trace.WriteLine("Instance endpoint IP address and port: " + roleInstEndpoint.IPEndpoint);
    }
}
```

Evo primjera radnih uloge koje dohvaća krajnju točku izložen putem servisa definition i pokreće slušanje za veze.

> [AZURE.WARNING] Kod funkcionirat će samo distribuiranih servisa. Kada se pokrene u Emulator za izračunavanje Azure, zanemaruju se elementi konfiguracija servisa stvorite Izravni priključak krajnje točke (**InstanceInputEndpoint** elementi).

```csharp
using System;
using System.Diagnostics;
using System.Linq;
using System.Net;
using System.Net.Sockets;
using System.Threading;
using Microsoft.WindowsAzure;
using Microsoft.WindowsAzure.Diagnostics;
using Microsoft.WindowsAzure.ServiceRuntime;
using Microsoft.WindowsAzure.StorageClient;

namespace WorkerRole1
{
  public class WorkerRole : RoleEntryPoint
  {
    public override void Run()
    {
      try
      {
        // Initialize method-wide variables
        var epName = "Endpoint1";
        var roleInstance = RoleEnvironment.CurrentRoleInstance;
        
        // Identify direct communication port
        var myPublicEp = roleInstance.InstanceEndpoints[epName].PublicIPEndpoint;
        Trace.TraceInformation("IP:{0}, Port:{1}", myPublicEp.Address, myPublicEp.Port);

        // Identify public endpoint
        var myInternalEp = roleInstance.InstanceEndpoints[epName].IPEndpoint;
                
        // Create socket listener
        var listener = new Socket(
          myInternalEp.AddressFamily, SocketType.Stream, ProtocolType.Tcp);
                
        // Bind socket listener to internal endpoint and listen
        listener.Bind(myInternalEp);
        listener.Listen(10);
        Trace.TraceInformation("Listening on IP:{0},Port: {1}",
          myInternalEp.Address, myInternalEp.Port);

        while (true)
        {
          // Block the thread and wait for a client request
          Socket handler = listener.Accept();
          Trace.TraceInformation("Client request received.");

          // Define body of socket handler
          var handlerThread = new Thread(
            new ParameterizedThreadStart(h =>
            {
              var socket = h as Socket;
              Trace.TraceInformation("Local:{0} Remote{1}",
                socket.LocalEndPoint, socket.RemoteEndPoint);

              // Shut down and close socket
              socket.Shutdown(SocketShutdown.Both);
              socket.Close();
            }
          ));

          // Start socket handler on new thread
          handlerThread.Start(handler);
        }
      }
      catch (Exception e)
      {
        Trace.TraceError("Caught exception in run. Details: {0}", e);
      }
    }

    public override bool OnStart()
    {
      // Set the maximum number of concurrent connections 
      ServicePointManager.DefaultConnectionLimit = 12;

      // For information on handling configuration changes
      // see the MSDN topic at http://go.microsoft.com/fwlink/?LinkId=166357.
      return base.OnStart();
    }
  }
}
```

## <a name="network-traffic-rules-to-control-role-communication"></a>Mrežni promet pravila da biste upravljali ulogama komunikacije
Nakon definiranja Interna krajnje točke mrežni promet pravila (koja se temelji na krajnje točke koju ste stvorili) možete dodati kontrolu način mogu komunicirati uloga instance međusobno. Sljedeći dijagram prikazuje neke uobičajene namjene kontroliranje komunikacije uloga:

![Scenariji za mrežni promet pravila] (./media/cloud-services-enable-communication-role-instances/scenarios.png "Scenariji za mrežni promet pravila")

Sljedeći primjer koda prikazuje definicije uloga uloge prikazano u prethodno dijagramu. Svaka definicija uloga sadrži najmanje jedan Interna krajnjoj točki definirani:

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <WebRole name="WebRole1" vmsize="Medium">
    <Sites>
      <Site name="Web">
        <Bindings>
          <Binding name="HttpIn" endpointName="HttpIn" />
        </Bindings>
      </Site>
    </Sites>
    <Endpoints>
      <InputEndpoint name="HttpIn" protocol="http" port="80" />
      <InternalEndpoint name="InternalTCP1" protocol="tcp" />
    </Endpoints>
  </WebRole>
  <WorkerRole name="WorkerRole1">
    <Endpoints>
      <InternalEndpoint name="InternalTCP2" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
  <WorkerRole name="WorkerRole2">
    <Endpoints>
      <InternalEndpoint name="InternalTCP3" protocol="tcp" />
      <InternalEndpoint name="InternalTCP4" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
</ServiceDefinition>
```

> [AZURE.NOTE] Ograničenje od komunikaciju između uloge se može pojaviti prilikom Interna krajnje točke obje popravi i automatski dodjeljuju priključci.

Prema zadanim postavkama, kada je definiran Interna krajnje točke, komunikacije možete tijeka iz bilo kojeg uloge internu krajnjoj uloge bez ograničenja. Da biste ograničili komunikacije, morate dodati **NetworkTrafficRules** element element **ServiceDefinition** u datoteci definicije servisa.

### <a name="scenario-1"></a>Scenarij 1
Samo omogućuju mrežni promet iz **WebRole1** **WorkerRole1**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-2"></a>Scenarij 2
Samo omogućuje mrežni promet od **WebRole1** **WorkerRole1** i **WorkerRole2**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-3"></a>Scenarij 3
Samo omogućuje mrežni promet od **WebRole1** **WorkerRole1**i **WorkerRole1** **WorkerRole2**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WorkerRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-4"></a>Scenarij 4
Samo omogućuje mrežni promet iz **WebRole1** **WorkerRole1**, **WebRole1** **WorkerRole2**i **WorkerRole1** **WorkerRole2**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo >
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WorkerRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo >
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP4" roleName="WorkerRole2"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

Moguće je pronaći referencu sheme XML elemente koji se koriste iznad [ovdje](https://msdn.microsoft.com/library/azure/gg557551.aspx).

## <a name="next-steps"></a>Daljnji koraci
Dodatne informacije o servisu u Oblaku [modela](cloud-services-model-and-package.md).