<properties
    pageTitle="Početak rada s koncentratorima događaja u Java | Microsoft Azure"
    description="Slijedite ovaj Praktični vodič da biste počeli koristiti koncentratorima događaj Azure; Slanje događaji s Java i prime pomoću na EventProcessorHost."
    services="event-hubs"
    documentationCenter=""
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="core"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Početak rada s koncentratorima događaja

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Uvod

Događaj koncentratorima je vrlo skalabilni ingestion sustav koji može intake milijune događaja u sekundi Omogućivanje aplikacije za obradu i analizirajte pretraživanje velikog količine podataka koje je stvorio povezanih uređaja i aplikacije. Kada se spremaju se u događaj koncentratora, možete transformirati i spremiti podatke pomoću bilo kojeg davatelja usluge u stvarnom vremenu analize ili klaster prostora za pohranu.

Dodatne informacije potražite u članku [Pregled koncentratora za događaj][].

Pomoću ovog praktičnog vodiča prikazuje kako ingest poruka u aplikaciji konzole u Java koncentratora za događaj i njihovo dohvaćanje paralelno pomoću biblioteka glavnog računala za Java događaj procesor.

Kako bi obavili ovog praktičnog vodiča trebate sljedeće:

+ Okruženje za razvoj Java. Za ovaj vodič će pretpostavkom da je [Eclipse](https://www.eclipse.org/).

+ Aktivni Azure račun. <br/>Ako nemate račun, možete stvoriti pomoću računa u samo nekoliko minuta. Detalje potražite u članku <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Azure besplatnu probnu verziju</a>.

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-java](../../includes/service-bus-event-hubs-get-started-send-java.md)]

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

Dodatne informacije potražite u [Centru za razvojne inženjere Java](/develop/java/).

<!-- Images. -->
[21]: ./media/event-hubs-java-ephjava-getstarted/ephjava.png
[22]: ./media/event-hubs-java-ephjava-getstarted/java-send.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Pregled događaja koncentratora]: event-hubs-overview.md
[primjer aplikacije koja koristi događaj koncentratora]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Promjena veličine izgleda događaj obrade s koncentratorima događaja]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
 