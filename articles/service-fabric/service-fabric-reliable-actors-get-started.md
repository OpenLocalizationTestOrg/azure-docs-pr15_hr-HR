<properties
   pageTitle="Početak rada sa servisa tkanina pouzdanog Glumci | Microsoft Azure"
   description="Pomoću ovog praktičnog vodiča vodit će vas kroz korake stvaranja, ispravljanje pogrešaka i implementacija jednostavne utemeljen na glumca servisa putem servisa tkanina pouzdanog Glumci."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/25/2016"
   ms.author="vturecek"/>

# <a name="getting-started-with-reliable-actors"></a>Početak rada s pouzdanog Glumci

> [AZURE.SELECTOR]
- [C# u sustavu Windows](service-fabric-reliable-actors-get-started.md)
- [Java na Linux](service-fabric-reliable-actors-get-started-java.md)

U ovom se članku objašnjava osnove pouzdanog Glumci tkanina servisa Azure i vodi vas kroz stvaranje ispravljanje pogrešaka i implementacija jednostavne aplikacije pouzdanog glumca u Visual Studio.

## <a name="installation-and-setup"></a>Instalacija i postavljanje
Prije nego što počnete, provjerite imate li servis tkanina razvojno okruženje postavljanje na vašem računalu.
Ako vam je potrebna da biste postavili ste ga, pročitajte detaljne upute o [tome da biste postavili okruženje za razvoj](service-fabric-get-started.md).

## <a name="basic-concepts"></a>Osnovni koncepti
Za početak rada s pouzdanog Glumci, što je potrebno da biste shvatili nekoliko osnovni koncepti:

 * **Glumca servisa**. Pouzdan Glumci su pakirat u pouzdanog servise koje možete uvesti u infrastrukture tkanina servisa. Glumca instanci su aktivirane u instancu imenovani servisa.
 
 * **Registracija glumca**. Kao pouzdan servisi sustava pouzdanog glumca usluge mora biti registriran u runtime tkanina servisa. Osim toga, vrstu glumca mora biti registriran u vrijeme izvođenja glumca.
 
 * **Glumca sučelja**. Sučelje glumca koristi se za definiranje svakako upisani javno sučelja glumca. U model terminologija pouzdanog glumca sučelja glumca definira vrste poruka koje u glumca mogu razumjeti i proces. Sučelje glumca koristi drugim Glumci i klijentske aplikacije za "slanje" (asinkrono) poruka u glumca. Pouzdan Glumci možete implementirati više sučelja.
 
 * **ActorProxy predmet**. Klase ActorProxy koristi klijentske aplikacije za pozivanje metode vidljiva kroz glumca sučelja. Klase ActorProxy omogućuje dvije važne radovi:
    * Naziv razlučivost: je moći pronaći na glumca u skupini (Traži čvor klaster gdje se hostira).
    * Nije uspjelo rukovanje: možete ponovno pokušajte instanci način i ponovno riješiti glumca mjesto nakon, na primjer, pogreške koje je potrebno glumca da biste je premještena na drugi čvor u klasteru.

Sljedeća pravila koji se odnose na glumca sučelja su Spominjanje:

- Ne može biti pretrpan glumca sučelja metode.
- Glumca sučelja metode ne smije imati odgovor, ref ili neobavezni parametri.
- Generički sučelja nisu podržani.

## <a name="create-a-new-project-in-visual-studio"></a>Stvaranje novog projekta u Visual Studio
Kada instalirate alate tkanina servisa za Visual Studio, možete stvoriti nove vrste projekta. Nove vrste projekta su kategoriji **oblaka** dijaloškog okvira **Novi projekt** .


![Servis tkanina alate za Visual Studio – novi projekt][1]

U sljedećem dijaloškom okviru možete odabrati vrstu projekta koji želite stvoriti.

![Servis tkanina predlošcima projekata][5]

Za projekt HelloWorld ćemo pomoću servisa tkanina pouzdanog Glumci servisa.

Kada stvorite rješenje, trebali biste vidjeti sljedeću strukturu:

![Struktura servisa tkanina projekta][2]

## <a name="reliable-actors-basic-building-blocks"></a>Pouzdan Glumci osnovni sastavnih blokova

Uobičajeni pouzdanog Glumci rješenja sastoji se od tri projekata:

* **Aplikacija projekta (MyActorApplication)**. Ovo je projekta kojim se paketi sve usluge zajedno radi implementacije. Sadrži skripte *ApplicationManifest.xml* i ljuske PowerShell za upravljanje aplikacija.

* **Project sučelja (MyActor.Interfaces)**. Ovo je projekt koji sadrži definiciju sučelje za na glumca. U programu project MyActor.Interfaces možete definirati sučelja koja će se koristiti tako da Glumci u rješenje. Sučelja za glumca može se definirati u bilo kojem projektu pod nazivom, no sučelje definira glumca ugovora koji zajednički koriste implementaciju glumca i klijenti poziva na glumca, pa obično smisla da biste definirali u sklopu programa koji razlikuje se od implementaciju glumca i mogu zajednički koristiti tako da više projekata.

```csharp
public interface IMyActor : IActor
{
    Task<string> HelloWorld();
}
```

* **Servis projekta glumca (MyActor)**. Ovo je korištena za definiranje servis tkanina servisa koji će se za hostiranje na glumca projekta. Sadrži implementacije u glumca. Implementacija glumca je klasa koja je izvedena iz osnovne vrste `Actor` i implementira interface(s) koji su definirani u programu project MyActor.Interfaces. Za predmete glumca morate provesti i Graditelj koje prihvaća programa `ActorService` instancu i `ActorId` i prosljeđuje baza `Actor` predmete. Time se omogućuje za unos ovisnost Graditelj od ovisnosti platforme.

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<string> HelloWorld()
    {
        return Task.FromResult("Hello world!");
    }
}
```

Servis glumca mora biti registriran u vrsta servisa u runtime tkanina servisa. U redoslijedu glumca servisa za pokretanje sustava instanci glumca vrsti vašega glumca mora biti registriran i sa servisom glumca. Na `ActorRuntime` postupak prijave izvodi to funkcioniralo za Glumci.

```csharp
internal static class Program
{
    private static void Main()
    {
        try
        {
            ActorRuntime.RegisterActorAsync<MyActor>(
                (context, actorType) => new ActorService(context, actorType, () => new MyActor())).GetAwaiter().GetResult();

            Thread.Sleep(Timeout.Infinite);
        }
        catch (Exception e)
        {
            ActorEventSource.Current.ActorHostInitializationFailed(e.ToString());
            throw;
        }
    }
}

```

Ako pokretanje novog projekta u Visual Studio, a imate samo jedan glumca definicija, Registracija uključen je prema zadanim postavkama kod koje generira Visual Studio. Ako definirate druge Glumci u servisu, morate dodati Registracija glumca pomoću:

```csharp
 ActorRuntime.RegisterActorAsync<MyOtherActor>();

```

> [AZURE.TIP] Izvođenje servisa tkanina Glumci emits neke [događaja i mjerača performansi vezane uz glumca metode](service-fabric-reliable-actors-diagnostics.md#actor-method-events-and-performance-counters). Su korisne su za dijagnostiku i praćenje performansi.


## <a name="debugging"></a>Ispravljanje pogrešaka

Alati za servis tkanina za Visual Studio podržava ispravljanje pogrešaka na lokalnom računalu. Ispravljanje pogrešaka sesiju možete najprije odlazak tipku F5. Visual Studio sastavlja (Ako je potrebno) paketa. Također uvodi aplikaciju na lokalni klaster tkanina servisa i pridružuje program za ispravljanje pogrešaka.

Tijekom postupka implementacije, vidjet ćete tijeka u **izlaznom** prozoru.

![Ispravljanje pogrešaka izlaznom prozoru tkanina servisa][3]


## <a name="next-steps"></a>Daljnji koraci
 - [Kako koristiti pouzdanog Glumci tkanina servisa platforme](service-fabric-reliable-actors-platform.md)
 - [Upravljanje glumca stanja](service-fabric-reliable-actors-state-management.md)
 - [Životni ciklus i smeća zbirke glumca](service-fabric-reliable-actors-lifecycle.md)
 - [Dokumentacija referenca glumca API-JA](https://msdn.microsoft.com/library/azure/dn971626.aspx)
 - [Ogledni kod](https://github.com/Azure/servicefabric-samples)


<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject.PNG
[2]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-projectstructure.PNG
[3]: ./media/service-fabric-reliable-actors-get-started/debugging-output.PNG
[4]: ./media/service-fabric-reliable-actors-get-started/vs-context-menu.png
[5]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject1.PNG
