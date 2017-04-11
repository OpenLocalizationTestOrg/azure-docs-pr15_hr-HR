<properties
    pageTitle="Početak rada s koncentratorima događaja u C# s Apache oluja | Microsoft Azure"
    description="Slijedite ovaj Praktični vodič da biste počeli koristiti koncentratorima događaj Azure; Slanje događaja u C# i prime programa klasteru Apache oluja."
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
    ms.topic="article" 
    ms.date="09/06/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Početak rada s koncentratorima događaja

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Uvod

Događaj koncentratorima je vrlo skalabilni ingestion sustav koji može intake milijune događaja u sekundi Omogućivanje aplikacije za obradu i analizirajte pretraživanje velikog količine podataka koje je stvorio povezanih uređaja i aplikacije. Kada se spremaju se u događaj koncentratora, možete transformirati i spremiti podatke pomoću bilo kojeg davatelja usluge u stvarnom vremenu analize ili klaster prostora za pohranu.

Dodatne informacije potražite [Pregled koncentratora za događaj].

U ovom ćete praktičnom vodiču ćete naučiti ingest poruka u aplikaciji konzole u C# koncentratora za događaj i njihovo dohvaćanje paralelno pomoću Apache oluja.

Kako bi obavili ovog praktičnog vodiča trebate sljedeće:

+ [Microsoft Visual Studio](http://visualstudio.com)

+ Java okruženje za razvoj konfiguriran za izvođenje [Maven](http://maven.apache.org/). Za ovaj vodič će pretpostavkom da je [Eclipse](https://www.eclipse.org/)

+ Aktivni Azure račun. <br/>Ako nemate račun, možete stvoriti pomoću računa u samo nekoliko minuta. Detalje potražite u članku <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Azure besplatnu probnu verziju</a>.

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-csharp](../../includes/service-bus-event-hubs-get-started-send-csharp.md)]


[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-storm](../../includes/service-bus-event-hubs-get-started-receive-storm.md)]

## <a name="run-the-applications"></a>Pokretanje aplikacije

Sada ste spremni za pokretanje aplikacije.

1.  Pokretanje klase **LogTopology** s Eclipse, a zatim pričekajte da biste pokrenuli primatelje za sve particije.

2.  Pokrenite projekt **pošiljatelja** , pritisnite tipku **Enter** u prozoru konzole i pogledajte događaja pojaviti u prozoru tekstnog okvira.

    ![][22]

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste već sastavljali rad aplikacije koje stvara koncentratora za događaj i šalje i prima podatke, možete premjestiti sljedećim scenarijima:

- U Dovršeno [Ogledni program koji koristi događaj koncentratora][].
- Primjer [Skaliranje out događaj obrade s koncentratorima događaj][] .

<!-- Images. -->
[22]: ./media/event-hubs-csharp-storm-getstarted/receive-storm1.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Pregled događaja koncentratora]: event-hubs-overview.md
[primjer aplikacije koja koristi događaj koncentratora]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Promjena veličine izgleda događaj obrade s koncentratorima događaja]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
 