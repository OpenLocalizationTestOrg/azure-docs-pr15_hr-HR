<properties
   pageTitle="Stvaranje web-sučelja aplikacije pomoću platforme ASP.NET osnovne | Microsoft Azure"
   description="Izložiti tkanina servisa aplikacija na webu pomoću ASP.NET osnovne Web API projekta i inter-servisa komunikaciju putem ServiceProxy."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/11/2016"
   ms.author="seanmck"/>


# <a name="build-a-web-service-front-end-for-your-application-using-aspnet-core"></a>Stvaranje web-servisa sučelja aplikacije pomoću Core platforme ASP.NET

Prema zadanim postavkama servisa Azure servisa tkanina omogućuje javno sučelje na webu. Da biste otkrili funkcija vaše aplikacije HTTP klijentima, morat ćete stvoriti projekt web poslužiti kao točku unosa i zatim komunikaciju s pojedinačnih servisa.

U ovom ćete praktičnom vodiču smo će obraditi gdje ćemo stali u ovom praktičnom vodiču za [Stvaranje prve aplikacije u Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md) i dodajte web-servisa ispred brojač s praćenjem stanja servisa. Ako već niste učinili, trebali biste se vratili i koraka do tog vodiča.

## <a name="add-an-aspnet-core-service-to-your-application"></a>Dodavanje servisa platforme ASP.NET osnovne u aplikaciji

ASP.NET temeljni je okvir za razvoj laganih, različite platforme web koje možete koristiti da biste stvorili Moderna web korisničkog Sučelja i web-API-ji. Dodat ćemo projekta programa ASP.NET Web API naš postojeće aplikacije.

>[AZURE.NOTE] Da biste dovršili ovaj Praktični vodič, morat ćete [instalirati .NET Core 1.0][dotnetcore-install].

1. U pregledniku rješenja **usluge** desnom tipkom miša kliknite unutar projekta aplikaciju i odaberite **Dodaj > novi servis tkanina servis**.

    ![Dodavanje novog servisa da biste postojeću aplikaciju][vs-add-new-service]

2. Na stranici **Stvaranje servisa** odaberite **ASP.NET osnovne** i dajte mu naziv.

    ![U dijaloškom okviru za novi servis odabir web-servisa platforme ASP.NET][vs-new-service-dialog]

3. Na sljedećoj stranici pruža skup ASP.NET osnovne predlošcima projekata. Imajte na umu da ih isti predloške koje želite vidjeti ako ste stvorili u ASP.NET osnovne projekt izvan tkanina servisa aplikacija. U ovom ćete praktičnom vodiču smo će odaberite **Web API**. Međutim, možete primijeniti iste koncepata i stvaranje cijelog web-aplikacije.

    ![Odabir vrste projekta platforme ASP.NET][vs-new-aspnet-project-dialog]

    Nakon stvaranja Web API projekta, imat ćete dva servisa u aplikaciji. U nastavku da biste sastavili aplikacije, dodate nove servise na isti način. Svaka se može biti neovisno određuju i nadograđena.

>[AZURE.TIP] Dodatne informacije o izradi ASP.NET Core services potražite u [Dokumentaciji Core ASP.NET](https://docs.asp.net).

## <a name="run-the-application"></a>Pokrenite aplikaciju

Da biste saznati što smo ste dovršili, recimo Implementirajte novu aplikaciju i razmotrite zadano ponašanje u kojoj se navode predložak API servisa platforme ASP.NET osnovne Web.

1. Pritisnite F5 u Visual Studio za ispravljanje pogrešaka aplikacije.

2. Po dovršetku implementacije Visual Studio će se pokrenuti preglednika korijen servisa platforme ASP.NET Web API – nešto poput http://localhost:33003. Broj priključka slučajno dodijeljeni, a mogu se razlikovati na vašem računalu. Predložak ASP.NET osnovne Web API-JA ne nudi zadano ponašanje za korijenu, pa će se pogreška u pregledniku.

3. Dodavanje `/api/values` na mjesto u pregledniku. To će pozivanje na `Get` način na ValuesController u predlošku Web API-JA. Zadani odgovor na kojemu predložak – JSON polja koja sadrže dva niza će se vratiti:

    ![Zadane vrijednosti koje su rezultat iz predloška ASP.NET osnovne Web API-JA][browser-aspnet-template-values]

    Do kraja vodič će zamijenili smo ove zadane vrijednosti vrijednošću najnovije brojač iz naših s praćenjem stanja servisa.


## <a name="connect-the-services"></a>Povezivanje servisa

Servis tkanina pruža dovršeno fleksibilnost u kako komunicirati s pouzdanog services. U jednoj aplikaciji, možda ćete imati servise koje su dostupne putem TCP druge servise koji se može pristupiti putem HTTP REST API te i dalje druge servise koji su dostupni putem web sockets. Pozadina na mogućnosti dostupne i tradeoffs koji je uključen, potražite u članku [Communicating sa servisima](service-fabric-connect-and-communicate-with-services.md). U ovom ćete praktičnom vodiču ćemo će poduzmite jedan od jednostavniji pristupa i koristiti u `ServiceProxy` / `ServiceRemotingListener` klase koje služe SDK-a.

U na `ServiceProxy` pristup (katalog modeliran poziva udaljene procedure ili RPC-a), koje ste definirali sučelja će poslužiti kao javno ugovor za servis. Nakon toga koristiti taj sučelja za generiranje predmete proxy poslužitelja za interakciju sa servisom.


### <a name="create-the-interface"></a>Napravite sučelje

Ne možemo počet će stvaranjem sučelja će poslužiti kao ugovor između s praćenjem stanja servisa i njegov klijenata, uključujući ASP.NET osnovne projekta.

1. U pregledniku rješenja, desnom tipkom miša kliknite rješenje i odaberite **Dodaj** > **Novi projekt**.

2. Odaberite stavku **Visual C#** u lijevom navigacijskom oknu, a zatim odaberite predložak **Biblioteka klasa** . Provjerite je li verzija .NET Framework postavljena na **4.5.2**.

    ![Stvaranje projekta programa sučelja s praćenjem stanja servisa][vs-add-class-library-project]

3. Da bi sučelja se može koristiti `ServiceProxy`, on mora proizlaziti iz IService sučelja. Ovo sučelje uvrštava u jednom od servisa tkanina NuGet paketa. Da biste dodali paket, desnom tipkom miša kliknite novi projekt Biblioteka klasa i odaberite **Upravljanje NuGet paketa**.

4. Traženje paketa **Microsoft.ServiceFabric.Services** i instalirajte ga.

    ![Dodavanje servisa NuGet paketa][vs-services-nuget-package]

5. U Biblioteka klasa stvaranje sučelja s jednom metode `GetCountAsync`, i proširivanje sučelje iz IService.

    ```c#
    namespace MyStatefulService.Interfaces
    {
        using Microsoft.ServiceFabric.Services.Remoting;

        public interface ICounter: IService
        {
            Task<long> GetCountAsync();
        }
    }
    ```


### <a name="implement-the-interface-in-your-stateful-service"></a>Implementacija sučelja na servisu s praćenjem stanja

Sad kad ste definirali sučelje, potrebna je implementirati u s praćenjem stanja servisa.

1. Na servisu s praćenjem stanja dodajte reference na razrednom projektu biblioteku koja sadrži sučelje.

    ![Dodavanje reference na razrednom projektu biblioteke u s praćenjem stanja servisa][vs-add-class-library-reference]

2. Pronađite predmete koji nasljeđuju od `StatefulService`, kao što su `MyStatefulService`, i proširivanje je za implementaciju sustava `ICounter` sučelja.

    ```c#
    using MyStatefulService.Interfaces;

    ...

    public class MyStatefulService : StatefulService, ICounter
    {        
          // ...
    }
    ```

3. Sada implementirati jedan način koji je definiran u na `ICounter` sučelje, `GetCountAsync`.

    ```c#
    public async Task<long> GetCountAsync()
    {
      var myDictionary =
        await this.StateManager.GetOrAddAsync<IReliableDictionary<string, long>>("myDictionary");

        using (var tx = this.StateManager.CreateTransaction())
        {          
            var result = await myDictionary.TryGetValueAsync(tx, "Counter");
            return result.HasValue ? result.Value : 0;
        }
    }
    ```


### <a name="expose-the-stateful-service-using-a-service-remoting-listener"></a>Izlaganje s praćenjem stanja servisa pomoću na servis remoting ga slušatelj

S na `ICounter` sučelju implementirana završnom koraku s omogućivanjem s praćenjem stanja servisa da biste se callable s drugih servisa je da biste otvorili komunikacijskog kanala. S praćenjem stanja servisa tkanina servisa predstavlja može odbaciti način koji se naziva `CreateServiceReplicaListeners`. Ako koristite taj način, možete odrediti jedan ili više slušače komunikacije, ovisno o vrsti komunikacije koje želite omogućiti za uslugu.

>[AZURE.NOTE] Ekvivalentan način otvaranja komunikacijskog kanala da biste bez praćenja stanja servisa zove `CreateServiceInstanceListeners`.

U ovom slučaju ne možemo zamijenit će postojeće `CreateServiceReplicaListeners` način i navedite instance komponente `ServiceRemotingListener`, koji stvara krajnje RPC koje je callable klijenata putem `ServiceProxy`.  

```c#
using Microsoft.ServiceFabric.Services.Remoting.Runtime;

...

protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new List<ServiceReplicaListener>()
    {
        new ServiceReplicaListener(
            (context) =>
                this.CreateServiceRemotingListener(context))
    };
}
```


### <a name="use-the-serviceproxy-class-to-interact-with-the-service"></a>Koristite klasa ServiceProxy za interakciju sa servisom

Naš s praćenjem stanja servisa je sada primanje promet s ostalim servisima. Ostaje pa sve što je dodavanje koda za komunicirati s njim iz web-servisa platforme ASP.NET.

1. U projektu ASP.NET, dodajte referencu Biblioteka klasa koja sadrži na `ICounter` sučelja.

2. Na izborniku **Sastavljanje** otvorite **Upravitelj konfiguracije**. Trebali biste vidjeti otprilike ovako:

    ![Biblioteka klasa konfiguracije manager s prikazom kao AnyCPU][vs-configuration-manager]

    Imajte na umu razrednom projektu biblioteke **MyStatefulService.Interface**konfiguriran za izgradnju za bilo koji procesora. Da bi ispravno funkcionirala tkaninom servisa, to morate izričito usmjeravati x64. Kliknite padajući izbornik platformu odaberite **Novo**, a zatim stvorite programa x64 platformu konfiguracije.

    ![Stvaranje nove platformu za Biblioteka klasa][vs-create-platform]

3. Dodajte paketa Microsoft.ServiceFabric.Services u ASP.NET projekt, baš kao i koje za projekt Biblioteka klasa ste prethodno poduzeli. Taj će vam se `ServiceProxy` predmete.

4. U mapi **kontrolera** otvorite na `ValuesController` predmete. Imajte na umu da se `Get` način samo trenutno vraća niz programiranih raspon "vrijednost1" i "vrijednost2" – koji odgovara što smo vidjeli ranije u pregledniku. Ova implementacija zamijenite sljedeći kod:

    ```c#
    using MyStatefulService.Interfaces;
    using Microsoft.ServiceFabric.Services.Remoting.Client;

    ...

    public async Task<IEnumerable<string>> Get()
    {
        ICounter counter =
            ServiceProxy.Create<ICounter>(new Uri("fabric:/MyApplication/MyStatefulService"), new ServicePartitionKey(0));

        long count = await counter.GetCountAsync();

        return new string[] { count.ToString() };
    }
    ```

    U prvom retku kod je ključa. Da biste stvorili proxy ICounter s praćenjem stanja servisa, morate unijeti dva dijelovi informacija: ID particije i naziv servisa.

    Možete koristiti particija skala s praćenjem stanja servisima po prekidanje gore njihovo stanje u različitim memorijski blokovi na temelju tipke koje sami definirate, kao što je ID klijenta ili poštanski broj. U našem trivial računala s praćenjem stanja servisa ima samo jedna particija pa tipku nije bitan. Bilo koju tipku koje ste omogućili vodi do iste particije. Da biste saznali više o stvaranju particija services, Saznajte [kako particija servisa tkanina pouzdanog Services](service-fabric-concepts-partitioning.md).

    Naziv usluge je URI tkanina obrasca: /&lt;application_name&gt;/&lt;service_name&gt;.

    Pomoću ove dvije dijelovi informacija tkanina servisa mogu identificirati samo stroju koji zahtjeva moraju se poslati u. Na `ServiceProxy` predmete i jednostavno rukuje kutije gdje prekida se računalom na kojem je smještena particija s praćenjem stanja servisa i drugo računalo mora dobiti ulogu njegov odvija. U ovom apstrakcije čini pisanja koda za klijent radnju s ostalim servisima znatno jednostavniji.

    Kada imamo proxy poslužitelj, ne možemo jednostavno pozvati na `GetCountAsync` način i vratili njezinim rezultatom.

5. Pritisnite F5 da biste pokrenuli izmijenjene aplikacije. Kao prije, Visual Studio će automatski pokrenuti web-preglednik da korijenu web project. Dodavanje puta "API-ja na vrijednosti", a trebali biste vidjeti vraćena brojač vrijednost.

    ![Vrijednost s praćenjem stanja brojač prikazuje se u pregledniku][browser-aspnet-counter-value]

    Osvježite preglednik povremeno da biste vidjeli vrijednost brojač ažurirati.


>[AZURE.WARNING] Web-poslužitelj ASP.NET osnovne navedenih u predlošku naziva Kestrel, je [trenutno nije podržano za rukovanje Izravni internetski promet](https://docs.asp.net/en/latest/fundamentals/servers.html#kestrel). Scenariji radnog razmislite o hosting ASP.NET osnovne krajnje točke iza [API upravljanja] [ api-management-landing-page] ili neki drugi pristupnika mjesto na Internetu. Imajte na umu tkanina servis nije podržana za implementaciju u IIS.


## <a name="what-about-actors"></a>Je li moguće koristiti Glumci?

Pomoću ovog praktičnog vodiča filtriran na Dodavanje web-sučelja koja se prenosi s s praćenjem stanja servisa. Međutim, možete pratiti slične modela razgovarati s Glumci. Zapravo je Pomalo jednostavniji.

Prilikom stvaranja projekta programa glumca Visual Studio automatski generira za projekt sučelja. Možete koristiti taj sučelje za generiranje proxyja kojemu je provjerena glumca u project web komunikaciju s glumca. Komunikacijskog kanala navedeni su automatski. Tako da ne morate ništa koja je jednaka uspostavljanje na `ServiceRemotingListener` kao da niste s praćenjem stanja servisa ovog praktičnog vodiča.

## <a name="how-web-services-work-on-your-local-cluster"></a>Funkcioniranje web-servisa na vašem lokalnom klaster

Općenito govoreći, možete implementirati točno iste tkanina servisa aplikacije više strojne grupe klaster koji je implementiran na vašem lokalnom klaster i biti vrlo sigurni da će funkcionirati kao što ste očekivali. To je zato je vaše lokalne klaster jednostavno pet čvor konfiguraciju koja je sažet na jednom računalu.

Kada je riječ o web-servisi, no postoji jedan nuance ključa. Kada svoj klaster nalazi iza opterećenja, kao što je to čini Azure, morate osigurati da web-servisi primjenjuju se na svakom računalu Budući da raspoređivača opterećenja će jednostavno-kružnog promet na na računalima. To možete učiniti tako da postavite na `InstanceCount` za servis za posebne vrijednost-"1".

Za razliku od toga kada pokrenete web-servisa lokalno, koje je potrebno provjeriti tu samo jednu instancu servis pokrenut. U suprotnom će naići u sukobu s više procesa koji se priključuje na isti put i priključak. Kao rezultat broj instanci servisa za web mora biti postavljeno na "1" za lokalne implementacije.

Da biste saznali kako konfigurirati različite vrijednosti za različite okruženja, potražite u članku [Upravljanje aplikacije parametara za više okruženja](service-fabric-manage-multiple-environment-app-configuration.md).

## <a name="next-steps"></a>Daljnji koraci

- [Stvaranje klaster u Azure za implementaciju aplikacije u oblak](service-fabric-cluster-creation-via-portal.md)
- [Dodatne informacije o komunikaciji sa servisima](service-fabric-connect-and-communicate-with-services.md)
- [Dodatne informacije o stvaranju particija s praćenjem stanja servisa](service-fabric-concepts-partitioning.md)

<!-- Image References -->

[vs-add-new-service]: ./media/service-fabric-add-a-web-frontend/vs-add-new-service.png
[vs-new-service-dialog]: ./media/service-fabric-add-a-web-frontend/vs-new-service-dialog.png
[vs-new-aspnet-project-dialog]: ./media/service-fabric-add-a-web-frontend/vs-new-aspnet-project-dialog.png
[browser-aspnet-template-values]: ./media/service-fabric-add-a-web-frontend/browser-aspnet-template-values.png
[vs-add-class-library-project]: ./media/service-fabric-add-a-web-frontend/vs-add-class-library-project.png
[vs-add-class-library-reference]: ./media/service-fabric-add-a-web-frontend/vs-add-class-library-reference.png
[vs-services-nuget-package]: ./media/service-fabric-add-a-web-frontend/vs-services-nuget-package.png
[browser-aspnet-counter-value]: ./media/service-fabric-add-a-web-frontend/browser-aspnet-counter-value.png
[vs-configuration-manager]: ./media/service-fabric-add-a-web-frontend/vs-configuration-manager.png
[vs-create-platform]: ./media/service-fabric-add-a-web-frontend/vs-create-platform.png


<!-- external links -->
[dotnetcore-install]: https://www.microsoft.com/net/core#windows
[api-management-landing-page]: https://azure.microsoft.com/en-us/services/api-management/
