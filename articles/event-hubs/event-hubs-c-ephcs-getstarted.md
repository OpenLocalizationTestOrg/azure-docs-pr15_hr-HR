<properties
    pageTitle="Početak rada s koncentratorima događaja u C i C# | Microsoft Azure"
    description="Slijedite ovaj Praktični vodič da biste počeli koristiti koncentratorima događaj Azure; Slanje događaje u C i primanje hem u C# pomoću na EventProcessorHost."
    services="event-hubs"
    documentationCenter=""
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="c"
    ms.devlang="csharp"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Početak rada s koncentratorima događaja

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Uvod

Događaj koncentratorima je vrlo skalabilni ingestion sustav koji može intake milijune događaja u sekundi Omogućivanje aplikacije za obradu i analizirajte pretraživanje velikog količine podataka koje je stvorio povezanih uređaja i aplikacije. Kada se spremaju se u događaj koncentratora, možete transformirati i spremiti podatke pomoću bilo kojeg davatelja usluge u stvarnom vremenu analize ili klaster prostora za pohranu.

Dodatne informacije potražite [Pregled koncentratorima događaj][].

U ovom ćete praktičnom vodiču ćete naučiti ingest poruka u aplikaciji konzole u C koncentratora za događaj i njihovo dohvaćanje paralelno pomoću biblioteke C# [Glavno računalo procesor događaj][] .

Da biste dovršili ovaj Praktični vodič ćete sljedeće:

+ Okruženje za razvoj C. Za ovaj vodič smo će da snop gcc na [Azure Linux VM](../virtual-machines/virtual-machines-linux-quick-create-cli.md) s Ubuntu 14.04. Upute za ostale okruženja objavit ćemo zajedno u vanjske veze.

+ Microsoft Visual Studio Express za Windows

+ Aktivni Azure račun. Ako nemate račun, možete stvoriti besplatnu probnu računa u samo nekoliko minuta. Detalje potražite u članku [Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/).

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-c](../../includes/service-bus-event-hubs-get-started-send-c.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephcs](../../includes/service-bus-event-hubs-get-started-receive-ephcs.md)]

## <a name="run-the-applications"></a>Pokretanje aplikacije

Sada ste spremni za pokretanje aplikacije.

1.  Pokrenite projekt **tekstnog okvira** s Visual Studio, a zatim pričekajte da biste pokrenuli primatelje za sve particije.

    ![][21]

2.  Pokrenite program **pošiljatelj** , a potražite u članku događaja pojaviti u prozoru tekstnog okvira.

    ![][24]

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste već sastavljali rad aplikacije koje stvara koncentratora za događaj i šalje i prima podatke, možete premjestiti sljedećim scenarijima:

- U Dovršeno [Ogledni program koji koristi događaj koncentratora][].
- Primjer [Skaliranje out događaj obrade s koncentratorima događaj][] .
- [Pregled događaja koncentratora][]

<!-- Images. -->
[21]: ./media/event-hubs-c-ephcs-getstarted/run-csharp-ephcs1.png
[24]: ./media/event-hubs-c-ephcs-getstarted/receive-eph-c.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Glavno računalo procesor događaja]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Pregled događaja koncentratora]: event-hubs-overview.md
[primjer aplikacije koja koristi događaj koncentratora]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Promjena veličine izgleda događaj obrade s koncentratorima događaja]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
