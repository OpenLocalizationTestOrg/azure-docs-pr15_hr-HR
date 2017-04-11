<properties 
pageTitle="Rukovanje događajima za životni ciklus servis u Oblaku | Microsoft Azure" 
description="Saznajte kako koristiti metode životni ciklus uloge u Oblaku u .NET" 
services="cloud-services" 
documentationCenter=".net" 
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

# <a name="customize-the-lifecycle-of-a-web-or-worker-role-in-net"></a>Prilagodba životni ciklus Web ili tempiranja uloga u .NET

Kada stvorite ulogu suradnika, proširivanje klase [RoleEntryPoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx) koji omogućuje metode možete zaobići koje omogućuju reagirati na događaje životni ciklus. Uloge web klase nije obavezan, a potrebno je koristiti za reagirati na događaje životni ciklus.

## <a name="extend-the-roleentrypoint-class"></a>Proširivanje RoleEntryPoint klase

Klase [RoleEntryPoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx) obuhvaća metode koje se nazivaju Azure kada ga **pokreće**, **pokreće**ili **zaustavlja** weba ili tempiranja uloga. Po želji možete nadjačati ovih metoda da biste upravljali ulogama Inicijalizacija, uloge zatvaranja nizove ili niti izvođenja uloge. 

Kada proširivanje **RoleEntryPoint**, imajte na umu sljedeće pojave od metoda:

-   Metode [OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) i [OnStop](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx) vratite Booleove vrijednosti, pa je moguće da biste se vratili na **false** od ovih metoda.

     Ako je kod vraća **false**, prekinut je naglo postupka uloga, bez pokretanja bilo kojeg slijeda zatvaranja možda na mjestu. Općenito govoreći, izbjegavajte iz metode **OnStart** vraća **false** .
     
-   Bilo koji je neuhvaćenu iznimku unutar programa preopterećenja metode **RoleEntryPoint** se smatra neobrađenu iznimku.

     Slučaju iznimku u jedan od načina za životni ciklus Azure će podići [UnhandledException](https://msdn.microsoft.com/library/system.appdomain.unhandledexception.aspx) događaja, a zatim prekinula postupak. Kada je vaša uloga ima je izvanmrežno, će pokrene po Azure. Kada se pojavi neobrađenu iznimku, događaju [prekida](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.stopping.aspx) se potencira i metodu **OnStop** se naziva.

Ako vaša uloga ne pokreće ili je recikliranje između pokretanje, zauzet, i zaustavljanje Državama, kod možda prijavi neobrađenu iznimku u jedan od događaja životni ciklus ponovnog pokretanja svaki put ulogu. U ovom slučaju pomoću događaja [UnhandledException](https://msdn.microsoft.com/library/system.appdomain.unhandledexception.aspx) uzrok iznimku i učiniti je komponenta. Vaša uloga može biti vraćanje i iz metodu [pokrenuti](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) koji uzrokuje ulogu da biste ponovno pokrenuli. Dodatne informacije o uvođenje Američke Države potražite u članku [Uobičajeni problemi koji uzrokuju uloge za Koš za smeće](cloud-services-troubleshoot-common-issues-which-cause-roles-recycle.md).

> [AZURE.NOTE] Ako koristite **Azure alate za Microsoft Visual Studio** za razvoj aplikacija, predlošcima projekata uloga automatsko proširivanje klase **RoleEntryPoint** , na popisu *WebRole.cs* i *WorkerRole.cs* datoteke.

## <a name="onstart-method"></a>Način OnStart

Način **OnStart** bit će pozvan kada je vaša uloga instance na mreži tako da Azure. Dok se izvršava kod OnStart, instancu uloga označen kao **zauzeto** , a bez vanjskih promet preusmjeravat će se ga tako da raspoređivača opterećenja. Možete nadjačati ovu metodu za izvođenje Inicijalizacija posla, kao što su implementacijom rukovatelja događajima i pokretanje [Dijagnostike Azure](cloud-services-how-to-monitor.md).

Ako **OnStart** vraća **vrijednost true**, instancu uspješno pokrenuti i Azure poziva metodu **RoleEntryPoint.Run** . Ako je **OnStart** vraća **false**, uloge prekida odmah, bez izvršavanja sve nizove planiranog zatvaranja.

Sljedeći primjer koda prikazuje kako nadjačati metodu **OnStart** . Ta metoda konfigurira i pokreće dijagnostičkih monitor kada instancu uloga pokreće, a postavlja prijenos zapisivanje podataka na račun za pohranu:

```csharp
public override bool OnStart()
{
    var config = DiagnosticMonitor.GetDefaultInitialConfiguration();

    config.DiagnosticInfrastructureLogs.ScheduledTransferLogLevelFilter = LogLevel.Error;
    config.DiagnosticInfrastructureLogs.ScheduledTransferPeriod = TimeSpan.FromMinutes(5);

    DiagnosticMonitor.Start("DiagnosticsConnectionString", config);

    return true;
}
```

## <a name="onstop-method"></a>Način OnStop

Način **OnStop** naziva nakon uloga instance je izvršena izvanmrežne po Azure i prije izlazi iz postupka. Možete nadjačati ovu metodu da biste nazvali koda potrebnog za vaša uloga instancu čistu isključiti.

> [AZURE.IMPORTANT] Kod koji se izvodi u načinu **OnStop** je ograničeno vrijeme da biste dovršili kada se zove razloga koji nisu u korisnički pokrenutom zatvaranja. Kada se ovaj put minutama, postupak je prekinut, tako da morate provjeriti kod u način mogu **OnStop** brzo pokretanje ili tolerates nije pokrenut do kraja. Način **OnStop** zove se kada se potencira događaju **prekida** .


## <a name="run-method"></a>Pokretanje načina

Možete nadjačati način **pokrenuti** implementaciju niti dugoročnih vaša uloga instance.

Nadjačavanje način **pokrenuti** nije potrebna; zadan pokreće niti koji zauvijek u stanju mirovanja. Ako nadjačali način **pokrenuti** , kod treba beskonačno blokirati. Ako je način **pokrenuti** vraća, uloga je automatski obavljanje brisanja; drugim riječima, Azure podiže događaju **prekida** i poziva metodu **OnStop** tako da vaše nizove isključivanja može izvršiti prije nego što je izvanmrežno ulogu.


### <a name="implementing-the-aspnet-lifecycle-methods-for-a-web-role"></a>Implementacijom metode životni ciklus ASP.NET web uloge

Životni ciklus načine ASP.NET, osim onih koje nudi klase **RoleEntryPoint** možete koristiti da biste upravljali pokretanje i isključivanje redoslijede za uloge web. To može biti korisno radi kompatibilnosti ako su porting postojeće ASP.NET aplikacija za Azure. Životni ciklus metode ASP.NET su pozvane iz **RoleEntryPoint** metode. Na **aplikacije\_pokretanje** način zove završetku metodu **RoleEntryPoint.OnStart** . Na **aplikacije\_završetka** način zove prije nego što se zove metodu **RoleEntryPoint.OnStop** .

## <a name="next-steps"></a>Daljnji koraci
Saznajte kako [stvoriti paket servisa oblaka](cloud-services-model-and-package.md).