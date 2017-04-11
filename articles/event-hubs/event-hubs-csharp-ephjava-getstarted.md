<properties
    pageTitle="Početak rada s koncentratorima događaja u C# | Microsoft Azure"
    description="Slijedite ovaj Praktični vodič da biste počeli koristiti koncentratorima događaj Azure; Slanje događaje u C# i prime Java pomoću na EventProcessorHost."
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
    ms.date="09/27/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Početak rada s koncentratorima događaja

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Uvod

Događaj koncentratora je servis koji obrađuje velikih količina podataka događaja (telemetrijskih) iz povezanih uređaja i aplikacije. Nakon prikupljanje podataka u događaj koncentratora, možete spremiti podatke pomoću klaster prostora za pohranu ili njihovo pretvaranje pomoću davatelja usluga analize u stvarnom vremenu. Ova veliki događaj prikupljanje i obrada mogućnost je komponentu ključa Moderna aplikacije arhitekturi uključujući Internet od stvari (IoT).

Pomoću ovog praktičnog vodiča pokazuje kako pomoću portala za Azure klasični da biste stvorili koncentratora za događaj. Također pokazuje kako prikupiti poruke u pomoću aplikacije konzole za pisane C# koncentratora za događaj i kako njihovo dohvaćanje paralelno pomoću biblioteka glavnog računala za Java događaj procesor.

Da biste dovršili ovaj Praktični vodič, potrebno je sljedeće:

+ [Microsoft Visual Studio](http://visualstudio.com)

+ Aktivni Azure račun. <br/>Ako ga nemate, možete stvoriti pomoću računa u samo nekoliko minuta. Detalje potražite u članku [Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F target="_blank").

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-csharp](../../includes/service-bus-event-hubs-get-started-send-csharp.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephjava](../../includes/service-bus-event-hubs-get-started-receive-ephjava.md)]

## <a name="run-the-applications"></a>Pokretanje aplikacije

Sada ste spremni za pokretanje aplikacije.

1.  Pokrenite projekt **tekstnog okvira** .

    ![][21]

2.  Pokrenite projekt **pošiljatelja** .

    ![][22]

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste već sastavljali rad aplikacije koje stvara koncentratora za događaj i šalje i prima podatke, možete premjestiti sljedećim scenarijima:

- U Dovršeno [Ogledni program koji koristi događaj koncentratora][].
- Primjer [Skaliranje out događaj obrade s koncentratorima događaj][] .
- [Pregled događaja koncentratora][]

<!-- Images. -->
[21]: ./media/event-hubs-csharp-ephjava-getstarted/ephjava.png
[22]: ./media/event-hubs-csharp-ephjava-getstarted/cs-send.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Pregled događaja koncentratora]: event-hubs-overview.md
[primjer aplikacije koja koristi događaj koncentratora]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Promjena veličine izgleda događaj obrade s koncentratorima događaja]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
