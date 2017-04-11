<properties
    pageTitle="Početak rada s koncentratorima događaja u C# | Microsoft Azure"
    description="Slijedite ovaj Praktični vodič za početak pomoću Azure događaj koncentratorima C# i koristite na EventProcessorHost."
    services="event-hubs"
    documentationCenter=""
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/02/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Početak rada s koncentratorima događaja

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Uvod

Događaj koncentratora je servis koji obrađuje velikih količina podataka događaja (telemetriju) iz povezanih uređaja i aplikacije. Nakon prikupljanje podataka u događaj koncentratora, možete spremiti podatke pomoću klaster prostora za pohranu ili njihovo pretvaranje pomoću davatelja usluga analize u stvarnom vremenu. Ova veliki događaj prikupljanje i obrada mogućnost je komponentu ključa Moderna aplikacije arhitekturi uključujući Internet od stvari (IoT).

Pomoću ovog praktičnog vodiča pokazuje kako pomoću portala za Azure klasični da biste stvorili koncentratora za događaj. Također pokazuje kako prikupiti poruke u koncentratora za događaj pomoću aplikacije konzole za pisan C# i upute za njihovo dohvaćanje paralelni pomoću biblioteke C# [Glavno računalo procesor događaj][] .

Da biste dovršili ovaj Praktični vodič, potrebno je sljedeće:

+ [Microsoft Visual Studio](http://visualstudio.com)

+ Aktivni Azure račun. Ako ga nemate, možete stvoriti pomoću računa u samo nekoliko minuta. Detalje potražite u članku [Azure besplatnu probnu verziju](https://azure.microsoft.com/free/).

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-csharp](../../includes/service-bus-event-hubs-get-started-send-csharp.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephcs](../../includes/service-bus-event-hubs-get-started-receive-ephcs.md)]

## <a name="run-the-applications"></a>Pokretanje aplikacije

Sada ste spremni za pokretanje aplikacije.

1. Iz programa Visual Studio, otvorite projekt **tekstnog okvira** koji ste prethodno stvorili.

2. Desnom tipkom miša kliknite rješenje **tekstnog okvira** , a zatim kliknite **Dodaj**, a zatim kliknite **Postojeći projekt**.
 
3. Pronađite postojeće Sender.csproj datoteku, a zatim dvokliknite da biste ga dodali rješenje.
 
4. Ponovno, desnom tipkom miša kliknite rješenje **tekstnog okvira** , a zatim kliknite **Svojstva**. Prikazat će se stranica svojstva **tekstnog okvira** .

5. Kliknite **Projekt prilikom pokretanja**, a zatim kliknite gumb **više projekata prilikom pokretanja** . Da biste **započeli**postavljanje okvir **Akcije** za projekte **primatelja** i **pošiljatelja** .

    ![][19]

6. Kliknite **projekt ovisnosti**. U okvir za **projekte** kliknite **pošiljatelj**. U okviru **Depends na** provjerite je li uključena **tekstnog okvira** .

    ![][20]

7. Kliknite **u redu** da biste zatvorili dijaloški okvir **Svojstva** .

1.  Pritisnite F5 da biste pokrenuli projekt **tekstnog okvira** s Visual Studio, a zatim pričekajte da biste pokrenuli primatelje za sve particije.

    ![][21]

2.  Project **pošiljatelj** će se automatski pokrenuti. Pritisnite tipku **Enter** u prozoru konzole i pogledajte događaja pojaviti u prozoru tekstnog okvira.

    ![][22]

Pritisnite **Ctrl + C** u prozoru **pošiljatelj** da biste završili aplikacije pošiljatelj, a zatim pritisnite tipku **Enter** u prozoru tekstnog okvira da biste isključili tu aplikaciju.

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste već sastavljali radu aplikacije koje stvara koncentratora za događaj i šalje i prima podatke, možete premjestiti sljedećim scenarijima:

- U Dovršeno [Ogledni program koji koristi događaj koncentratora][].
- Primjer [Skaliranje out događaj obrade s koncentratorima događaj][] .
- [Pregled događaja koncentratora][]

<!-- Images. -->
[19]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj1.png
[20]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj2.png
[21]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs1.png
[22]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs2.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Glavno računalo procesor događaja]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Pregled događaja koncentratora]: event-hubs-overview.md
[primjer aplikacije koja koristi događaj koncentratora]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Promjena veličine izgleda događaj obrade s koncentratorima događaja]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
[queued messaging solution]: ../service-bus-messaging/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md
 
