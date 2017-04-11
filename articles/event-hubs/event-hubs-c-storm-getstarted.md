<properties
    pageTitle="Početak rada s koncentratorima događaj s C i Apache oluja | Microsoft Azure"
    description="Slijedite ovaj Praktični vodič da biste počeli koristiti Azure događaj koncentratora; Slanje događaje u C i prime programa klasteru Apache oluja."
    services="event-hubs"
    documentationCenter=""
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="c"
    ms.devlang="java"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Početak rada s koncentratorima događaja

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Uvod

Događaj koncentratora je vrlo skalabilni ingestion sustav koji može intake milijune događaja u sekundi Omogućivanje aplikacije za obradu i analizirajte pretraživanje velikog količine podataka koje je stvorio povezanih uređaja i aplikacije. Kada se spremaju se u događaj koncentratorima, možete transformirati i spremiti podatke pomoću bilo kojeg davatelja usluge u stvarnom vremenu analize ili klaster prostora za pohranu.

Dodatne informacije potražite [Pregled koncentratora za događaj].

U ovom ćete praktičnom vodiču ćete naučiti ingest poruka u aplikaciji konzole u C koncentratora za događaj i njihovo dohvaćanje paralelno pomoću Apache oluja.

Da biste dovršili ovaj Praktični vodič ćete sljedeće:

+ Okruženje za razvoj C. Za ovaj vodič smo će da snop gcc na [Azure Linux VM](../virtual-machines/virtual-machines-linux-quick-create-cli.md) s Ubuntu 14.04. Upute za ostale okruženja objavit ćemo zajedno u vanjske veze.

+ Java okruženje za razvoj konfiguriran za izvođenje [Maven](http://maven.apache.org/). Za ovaj vodič će pretpostavkom da je [Eclipse](https://www.eclipse.org/).

+ Aktivni Azure račun. Ako nemate račun, možete stvoriti pomoću računa u samo nekoliko minuta. Detalje potražite u članku [Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/).

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-c](../../includes/service-bus-event-hubs-get-started-send-c.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-storm](../../includes/service-bus-event-hubs-get-started-receive-storm.md)]

## <a name="run-the-applications"></a>Pokretanje aplikacije

Sada ste spremni za pokretanje aplikacije.

1.  Pokretanje klase **LogTopology** s Eclipse, a zatim pričekajte da biste pokrenuli primatelje za sve particije.

2.  Pokrenite program za **pošiljatelja** , a potražite u članku događaja pojaviti u prozoru tekstnog okvira.

    ![][23]

> [AZURE.NOTE] U ovom ćete praktičnom vodiču samo pomoću oluja u načinu rada za lokalni svrhe razvoj. Pogledajte [Pregled oluja HDInsight] i službeni [Apache oluja] dokumentaciju dodatne informacije u implementacijama oluja i uzorke.

## <a name="next-steps"></a>Daljnji koraci

U sljedećim resursima dostupni su za razvoj aplikacija integriranje koncentratora za događaj i oluja.

- [Analiza podataka senzor s oluja i HDInsight][] je cijeli scenarij vodič pomoću koncentratora događaj, oluja i HBase ingest podaci senzora programa Hadoop klasteru.
- Vodič za pisanje oluja kanali pomoću C# [razvoju strujanje obradu podataka aplikacije s SCP.NET i C# na oluja i HDInsight][] je.

<!-- Images. -->
[23]: ./media/event-hubs-c-storm-getstarted/receive-storm3.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Event Processor Host]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Pregled događaja koncentratora]: event-hubs-overview.md

[Apache oluja]: https://storm.incubator.apache.org
[Pregled oluja HDInsight]: ../hdinsight/hdinsight-storm-overview.md/
[Analiza podataka senzor s oluja i HDInsight]: ../hdinsight/hdinsight-storm-sensor-data-analysis.md
[Razvoj strujanje obradu podataka aplikacije s SCP.NET i C# na oluja i HDInsight]: ../hdinsight/hdinsight-storm-develop-csharp-visual-studio-topology.md
