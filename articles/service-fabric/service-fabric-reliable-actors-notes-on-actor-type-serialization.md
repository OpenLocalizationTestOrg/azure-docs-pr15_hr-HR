<properties
   pageTitle="Pouzdan Glumci bilješke na glumca upišite serijalizacije | Microsoft Azure"
   description="U članku se opisuje osnovni sistemski preduvjeti za definiranje serializable klase koje je moguće koristiti da biste definirali stanja servisa tkanina pouzdanog Glumci i sučelja"
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
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="notes-on-service-fabric-reliable-actors-type-serialization"></a>Bilješke na servis tkanina pouzdanog Glumci upišite serijalizacije


Argumenti sve metode, vrste rezultata zadaci vratio svaki način u glumca sučelja i objekata pohranjenih u upravitelju stanje glumca moraju biti [Podataka ugovora serializable](https://msdn.microsoft.com/library/ms731923.aspx)... Vrijedi i za argumente metode definirano u [glumca događaj sučelja](service-fabric-reliable-actors-events.md#actor-events). (Glumca događaj sučelja metode uvijek vratiti Poništi.)

## <a name="custom-data-types"></a>Prilagođene vrste podataka

U ovom primjeru sljedeće glumca sučelja određuje način na koji vraća prilagođene vrste podataka naziva `VoicemailBox`.

```csharp
public interface IVoiceMailBoxActor : IActor
{
    Task<VoicemailBox> GetMailBoxAsync();
}
```

Sučelje nije impelemented tako da glumca koji koristi upravitelju stanja za pohranu na `VoicemailBox` objekta:

```csharp
[StatePersistence(StatePersistence.Persisted)]
public class VoiceMailBoxActor : Actor, IVoicemailBoxActor
{
    public VoiceMailBoxActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<VoicemailBox> GetMailboxAsync()
    {
        return this.StateManager.GetStateAsync<VoicemailBox>("Mailbox");
    }
}

```

U ovom primjeru u `VoicemailBox` Serijalizirano je objekt kada:
 - Objekt se prenose između instancu glumca i pozivatelj.
 - Objekt se sprema u upravitelju stanje gdje ga je ista i na disk, a replicirati na ostale čvorove.
 
Pouzdan glumca framework koristi serijalizacije DataContract. Dakle, objekti prilagođene podatke i svojim članovima mora biti dodavati napomene atributa **DataContract** i **DataMember** odnosno

```csharp
[DataContract]
public class Voicemail
{
    [DataMember]
    public Guid Id { get; set; }

    [DataMember]
    public string Message { get; set; }

    [DataMember]
    public DateTime ReceivedAt { get; set; }
}
```

```csharp
[DataContract]
public class VoicemailBox
{
    public VoicemailBox()
    {
        this.MessageList = new List<Voicemail>();
    }

    [DataMember]
    public List<Voicemail> MessageList { get; set; }

    [DataMember]
    public string Greeting { get; set; }
}
```

## <a name="next-steps"></a>Daljnji koraci
 - [Životni ciklus i smeća zbirke glumca](service-fabric-reliable-actors-lifecycle.md)
 - [Glumca trajanju i podsjetnike](service-fabric-reliable-actors-timers-reminders.md)
 - [Glumca događaja](service-fabric-reliable-actors-events.md)
 - [Glumca reentrancy](service-fabric-reliable-actors-reentrancy.md)
 - [Polymorphism glumca i uzorke vezanima uz objekt dizajna](service-fabric-reliable-actors-polymorphism.md)
 - [Dijagnostika glumca i praćenje performansi](service-fabric-reliable-actors-diagnostics.md)
