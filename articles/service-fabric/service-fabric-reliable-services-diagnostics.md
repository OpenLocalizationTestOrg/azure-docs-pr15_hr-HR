<properties
   pageTitle="S praćenjem stanja servisa pouzdanog Dijagnostika | Microsoft Azure"
   description="Dijagnostičke funkcija pouzdanog Services s praćenjem stanja"
   services="service-fabric"
   documentationCenter=".net"
   authors="AlanWarwick"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="05/17/2016"
   ms.author="alanwar"/>

# <a name="diagnostic-functionality-for-stateful-reliable-services"></a>Dijagnostičke funkcija pouzdanog Services s praćenjem stanja
Predmete s praćenjem stanja pouzdanog Services StatefulServiceBase emits [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) događaja koji se mogu koristiti za servis za ispravljanje pogrešaka, navedite uvida u način u vrijeme izvođenja radi i pomoć za otklanjanje poteškoća.

## <a name="eventsource-events"></a>EventSource događaja
Naziv EventSource klase s praćenjem stanja pouzdanog Services StatefulServiceBase je "Microsoft-ServiceFabric-servisima". Događaje iz ovog izvora događaj prikazuju se u prozoru [Dijagnostika događaja](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md#view-service-fabric-system-events-in-visual-studio) kada servis se [debugged u Visual Studio](service-fabric-debugging-your-application.md).

Primjeri Alati i tehnologije koji pomažu u prikupljanje i/ili prikaz događaji EventSource su [PerfView](http://www.microsoft.com/download/details.aspx?id=28567), [Microsoft Azure Dijagnostika](../cloud-services/cloud-services-dotnet-diagnostics.md)i [Microsoft TraceEvent biblioteke](http://www.nuget.org/packages/Microsoft.Diagnostics.Tracing.TraceEvent).

## <a name="events"></a>Događaji

|Naziv događaja|ID događaja|Razina|Opis događaja|
|----------|--------|-----|-----------------|
|StatefulRunAsyncInvocation|1|Informativna|Prilikom pokretanja servisa RunAsync zadatka|
|StatefulRunAsyncCancellation|2|Informativna|Prilikom otkaže zadatak RunAsync servisa|
|StatefulRunAsyncCompletion|3|Informativna|Prilikom dovršetka zadatka RunAsync servisa|
|StatefulRunAsyncSlowCancellation|4|Upozorenje|Prilikom zadatak RunAsync servisa traje predugo otkazivanja|
|StatefulRunAsyncFailure|5|Pogreška|Prilikom zadatak RunAsync servisa throws iznimku|

## <a name="interpret-events"></a>Tumačenje događaja

Događaji StatefulRunAsyncInvocation, StatefulRunAsyncCompletion i StatefulRunAsyncCancellation su korisne Writer servisa da biste shvatili životnim ciklusom servisa –, kao i vrijeme prilikom rada, Otkazano ili dovršeno servisa. To može biti korisno kada problemi sa servisom za ispravljanje pogrešaka i informacije o životnom ciklusu servisa.

Servis autorima treba platiti obratite pažnju na događaje StatefulRunAsyncSlowCancellation i StatefulRunAsyncFailure jer pokazuju problemi sa servisom.

StatefulRunAsyncFailure je čuje kad god RunAsync() zadatak servisa throws iznimku. Obično je iznimka pokazuje pogrešku ili pogrešku u servisu. Uz iznimku uzrokuje servis nije uspjela, tako da se premješta na drugi čvor. To može biti skupi postupak i može usporiti zahtjevi dok se premješta servis. Servis autorima trebali biste uzrok iznimku i, ako je to moguće, prevladavanje.

StatefulRunAsyncSlowCancellation je čuje kad god otkazivanja zahtjev za zadatak RunAsync traje dulje od četiri sekunde. Kada je servis traje predugo otkazivanja, utječe mogućnost za servis da biste brzo ponovno pokrenuti na drugi čvor. To može utjecati cjelokupan dostupnost servisa.
