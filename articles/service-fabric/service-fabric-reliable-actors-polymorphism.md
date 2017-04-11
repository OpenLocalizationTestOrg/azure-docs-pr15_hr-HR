<properties
   pageTitle="Polymorphism u framework pouzdanog Glumci | Microsoft Azure"
   description="Stvaranje hijerarhije .NET sučelja i vrsta u okvir pouzdan Glumci da biste ponovno koristili funkcionalnost i definicije API-JA."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/07/2016"
   ms.author="seanmck"/>

# <a name="polymorphism-in-the-reliable-actors-framework"></a>Polymorphism u framework pouzdanog Glumci

Pouzdan Glumci framework omogućuje sastavljanje Glumci pomoću raznih isti tehnika kojima se koristila u dizajnu vezanima uz objekt. Jedna od tih tehnika je polymorphism, koji omogućuje vrste i sučelja nasljeđivanje od više GRG generalizirano roditeljima. Nasljeđivanje u framework pouzdanog Glumci obično slijedi .NET model s nekoliko dodatna ograničenja.

## <a name="interfaces"></a>Sučelja

Pouzdan Glumci framework potrebno je definirati barem jedan sučelje za implementaciju vrstom glumca. Ovo sučelje služi za generiranje proxy predmete koje je moguće koristiti tako da klijenti za komunikaciju s vašeg Glumci. Sučelja mogu nasljeđuju od drugih sučelja pod uvjetom da svaki sučelje koje je primijenjeno po vrsti glumca i sve njegove roditeljima konačni proizlaziti iz IActor. IActor je definirana platformu osnovni sučelje za Glumci. Dakle, klasični polymorphism primjer pomoću oblika može izgledati ovako:

![Hijerarhija sučelje za Glumci oblika][shapes-interface-hierarchy]


## <a name="types"></a>Vrste

Možete stvoriti i hijerarhije vrste glumca koji potječu iz osnovni klase glumca kojemu platforma. Slučaju oblike, možda imate osnovu `Shape` vrstu:

```csharp
public abstract class Shape : Actor, IShape
{
    public abstract Task<int> GetVerticeCount();

    public abstract Task<double> GetAreaAsync();
}
```

Subtypes od `Shape` možete nadjačati metode iz logaritma.

```csharp
[ActorService(Name = "Circle")]
[StatePersistence(StatePersistence.Persisted)]
public class Circle : Shape, ICircle
{
    public override Task<int> GetVerticeCount()
    {
        return Task.FromResult(0);
    }

    public override async Task<double> GetAreaAsync()
    {
        CircleState state = await this.StateManager.GetStateAsync<CircleState>("circle");

        return Math.PI *
            state.Radius *
            state.Radius;
    }
}
```

Napomena u `ActorService` atribut u tipu glumca. Taj atribut govori framework pouzdanog glumca da automatski potrebno stvoriti služba za hostiranje Glumci ove vrste. U nekim slučajevima možda želite stvoriti osnovnu vrstu namijenjen isključivo za zajedničko korištenje funkcije podvrste i nikad koristit će se stvoriti instancu konkretni Glumci. U tim slučajevima, trebali biste koristiti u `abstract` ključne riječi da biste naznačili da nikada ne će stvoriti glumca na temelju te vrste.


## <a name="next-steps"></a>Daljnji koraci

- Pogledajte [kako framework pouzdanog Glumci upravlja tkanina servisa platforme](service-fabric-reliable-actors-platform.md) za pouzdanost, skalabilnost i dosljedni stanje.
- Saznajte više o životnom [ciklusu glumca](service-fabric-reliable-actors-lifecycle.md).

<!-- Image references -->

[shapes-interface-hierarchy]: ./media/service-fabric-reliable-actors-polymorphism/Shapes-Interface-Hierarchy.png
