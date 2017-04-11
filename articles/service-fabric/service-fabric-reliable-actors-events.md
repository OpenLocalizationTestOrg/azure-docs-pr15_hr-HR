<properties
   pageTitle="Pouzdan Glumci događaje | Microsoft Azure"
   description="Uvod u događaje za servis tkanina pouzdanog Glumci."
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
   ms.date="08/30/2016"
   ms.author="amanbha"/>


# <a name="actor-events"></a>Glumca događaja
Događaji glumca omogućuje slanje obavijesti najbolje trud iz na glumca klijente. Događaji glumca dizajnirani za komunikaciju glumca klijenta i ne treba koristiti za komunikaciju glumca glumca.

Sljedeće koda pokazalo kako se koristi glumca događaja u aplikaciji.

Definiranje sučelja koji opisuje događaja koji se objavljuje na glumca. Ovo sučelje mora biti izvedena iz na `IActorEvents` sučelja. Argumenti od metoda moraju biti [ugovora serializable podataka](service-fabric-reliable-actors-notes-on-actor-type-serialization.md). Metode mora vratiti Poništi, kao događaj obavijesti jednosmjerna i najbolje truda.

```csharp
public interface IGameEvents : IActorEvents
{
    void GameScoreUpdated(Guid gameId, string currentScore);
}
```

Deklariranje događaja koji se objavljuje glumca u sučelju glumca.

```csharp
public interface IGameActor : IActor, IActorEventPublisher<IGameEvents>
{
    Task UpdateGameStatus(GameStatus status);

    Task<string> GetGameScore();
}
```

Na klijentskoj strani implementirati rukovatelja događajima.

```csharp
class GameEventsHandler : IGameEvents
{
    public void GameScoreUpdated(Guid gameId, string currentScore)
    {
        Console.WriteLine(@"Updates: Game: {0}, Score: {1}", gameId, currentScore);
    }
}
```

Na klijentskom računalu, stvorite proxy poslužitelj za glumca koje objavljuje događaja i pretplata na njegov događaja.

```csharp
var proxy = ActorProxy.Create<IGameActor>(
                    new ActorId(Guid.Parse(arg)), ApplicationName);

await proxy.SubscribeAsync<IGameEvents>(new GameEventsHandler());
```

U slučaju failovers, na glumca možda neće uspjeti putem drugi proces ili čvor. Proxy glumca upravlja aktivni pretplate i automatski ponovno pretplaćuje ih. Možete kontrolirati interval ponovnog pretplate putem na `ActorProxyEventExtensions.SubscribeAsync<TEvent>` API-JA. Otkazivanje pretplate, koristite na `ActorProxyEventExtensions.UnsubscribeAsync<TEvent>` API-JA.

Na glumca, jednostavno objavljivanje događaje u kojem su unesene. Ako postoje pretplatnike događaj, izvođenja Glumci će im poslati obavijest.

```csharp
var ev = GetEvent<IGameEvents>();
ev.GameScoreUpdated(Id.GetGuidId(), score);
```

## <a name="next-steps"></a>Daljnji koraci
 - [Glumca reentrancy](service-fabric-reliable-actors-reentrancy.md)
 - [Dijagnostika glumca i praćenje performansi](service-fabric-reliable-actors-diagnostics.md)
 - [Dokumentacija referenca glumca API-JA](https://msdn.microsoft.com/library/azure/dn971626.aspx)
 - [Ogledni kod](https://github.com/Azure/servicefabric-samples)
