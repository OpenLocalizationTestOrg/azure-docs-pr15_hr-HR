<properties
    pageTitle="Početak rada s koncentratorima događaja u C | Microsoft Azure"
    description="Slijedite ovaj Praktični vodič da biste počeli koristiti Azure događaj koncentratora; Slanje događaje u C i prime Java pomoću voditelju procesor događaj."
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
    ms.date="09/27/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Početak rada s koncentratorima događaja

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Uvod

Događaj koncentratora je vrlo skalabilni ingestion sustav koji može intake milijune događaja u sekundi Omogućivanje aplikacije za obradu i analizirajte pretraživanje velikog količine podataka koje je stvorio povezanih uređaja i aplikacije. Kada se spremaju se u događaj koncentratorima, možete transformirati i spremiti podatke pomoću bilo kojeg davatelja usluge u stvarnom vremenu analize ili klaster prostora za pohranu.

Dodatne informacije potražite [Pregled koncentratorima događaj][].

U ovom ćete praktičnom vodiču ćete naučiti ingest poruka u aplikaciji konzole u C koncentratora za događaj i njihovo dohvaćanje paralelno pomoću biblioteke C# [Glavno računalo procesor događaj][] .

Kako bi obavili ovog praktičnog vodiča trebate sljedeće:

+ Okruženje za razvoj C. Za ovaj vodič smo će da snop gcc na [Azure Linux VM](../virtual-machines/virtual-machines-linux-quick-create-cli.md) s Ubuntu 14.04. Upute za ostale okruženja objavit ćemo zajedno u vanjske veze.

+ Microsoft Visual Studio Express za Windows

+ Aktivni Azure račun. <br/>Ako nemate račun, možete stvoriti pomoću računa u samo nekoliko minuta. Detalje potražite u članku <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Azure besplatnu probnu verziju</a>.

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-c](../../includes/service-bus-event-hubs-get-started-send-c.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephjava](../../includes/service-bus-event-hubs-get-started-receive-ephjava.md)]

## <a name="run-the-applications"></a>Pokretanje aplikacije

Sada ste spremni za pokretanje aplikacije.

1.  Pokrenite projekt **tekstnog okvira** .

    ![][21]

2.  Pokrenite program za **pošiljatelja** i obratite pozornost na to događaja pojaviti u prozoru tekstnog okvira.

    ![][24]

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste već sastavljali rad aplikacije koje stvara koncentratora za događaj i šalje i prima podatke, možete premjestiti sljedećim scenarijima:

- U Dovršeno [Ogledni program koji koristi događaj koncentratora][].
- Primjer [Skaliranje out događaj obrade s koncentratorima događaj][] .
- [Pregled događaja koncentratora][]

<!-- Images. -->
[21]: ./media/event-hubs-c-ephjava-getstarted/ephjava.png
[24]: ./media/event-hubs-c-ephjava-getstarted/receive-eph-c.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Glavno računalo procesor događaja]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Pregled događaja koncentratora]: event-hubs-overview.md
[primjer aplikacije koja koristi događaj koncentratora]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Promjena veličine izgleda događaj obrade s koncentratorima događaja]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
