<properties
   pageTitle="Otklanjanje poteškoća s praćenje događaja | Microsoft Azure"
   description="Najčešći problemi tijekom implementacija usluge na tkanina usluge za Microsoft Azure."
   services="service-fabric"
   documentationCenter=".net"
   authors="mattrowmsft"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="03/31/2016"
   ms.author="mattrow"/>


# <a name="troubleshoot-common-issues-when-you-deploy-services-on-azure-service-fabric"></a>Rješavanje uobičajenih problema prilikom implementirali servise na tkanina servisa Azure

Kada koristite usluge na vašem računalu za razvojne inženjere je jednostavno je za korištenje [Visual Studio alate za ispravljanje pogrešaka](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md). Za udaljene klastere [izvješća o stanju](service-fabric-view-entities-aggregated-health.md) uvijek su dobro mjesto za početak. Najlakši načini pristupa takva izvješća su putem komponente PowerShell ili [SFX](service-fabric-visualizing-your-cluster.md). U ovom se članku pretpostavlja ispravljanje pogrešaka udaljene klaster te su osnovni objašnjenje kako koristiti neku od tih alata.

##<a name="application-crash"></a>Zatvaranje aplikacije
Na "je particija ispod ciljani replike ili instance broj" Izvješće je dobro podatak koji se ruši usluge. Da biste saznali gdje se ruši usluge traje malo više istrage. Kada na servisu radi na razini, najboljeg prijatelja bit će skup well-thought iz kašnjenja.  Predlažemo da pokušajte [Azure Dijagnostika](service-fabric-diagnostics-how-to-setup-wad.md) za te kašnjenja prikupljanje i korištenje rješenja kao što su [Elastic pretraživanja](service-fabric-diagnostic-how-to-use-elasticsearch.md) za prikaz i pretraživanje na kašnjenja.

![Stanje particija SFX](./media/service-fabric-diagnostics-troubleshoot-common-scenarios/crashNewApp.png)

###<a name="during-service-or-actor-initialization"></a>Tijekom pokretanja servisa ili glumca
Željene iznimke prije nego što se pokreće servis vrsta uzrokovat će postupka pad. Za sljedeće vrste ruši zapisnik događaja aplikacije će prikazati pogrešku iz servisa.
Ovo su najčešće iznimke da biste vidjeli prije nego što se pokreće servis.

***System.IO.FileNotFoundException***

Ta se pogreška često je zbog nedostaju ovisnosti skupa. Potvrdite okvir svojstva CopyLocal u Visual Studio ili predmemoriji sklopova za čvor.

***System.Runtime.InteropServices.COMException***
 *pri System.Fabric.Interop.NativeRuntime+IFabricRuntime.RegisterStatefulServiceFactory (IntPtr, IFabricStatefulServiceFactory)*
 
 To označava naziv vrste registrirani usluge ne odgovara manifest servisa.

[Dijagnostika Azure](service-fabric-diagnostics-how-to-setup-wad.md) moguće je konfigurirati da biste prenijeli zapisnik događaja aplikacije za sve čvorove automatski.

###<a name="runasync-or-onactivateasync"></a>RunAsync() ili OnActivateAsync()
Ako se događa se srušiti tijekom inicijalizaciju ili pokrenuti vrsta registrirani servisa ili glumca, iznimku će biti prepoznate tkanina servisa Azure. Možete pogledati te davatelja EventSource detaljne u odjeljku "Sljedeći koraci".

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o postojeće Dijagnostika nudi tkanina servisa:

* [Pouzdan Glumci dijagnostiku](service-fabric-reliable-actors-diagnostics.md)
* [Pouzdan Dijagnostika Services](service-fabric-reliable-services-diagnostics.md)
