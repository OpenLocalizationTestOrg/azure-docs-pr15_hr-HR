<properties
 pageTitle="Obrada podataka senzor vozila s Apache oluja na HDInsight | Microsoft Azure"
 description="Saznajte kako obrada podataka senzor vozila iz koncentratora događaja pomoću Apache oluja na HDInsight. Dodajte podatke modela iz DocumentDB i pohraniti izlaz za pohranu."
 services="hdinsight,documentdb,notification-hubs"
 documentationCenter=""
 authors="Blackmist"
 manager="jhubbard"
 editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="java"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="08/23/2016"
ms.author="larryfr"/>

#<a name="process-vehicle-sensor-data-from-azure-event-hubs-using-apache-storm-on-hdinsight"></a>Obrada podataka senzor vozila iz koncentratora za događaj Azure HDInsight pomoću Apache oluja

Saznajte kako obrada podataka senzor vozila iz koncentratora za događaj Azure HDInsight pomoću Apache oluja. U ovom se primjeru čita senzor podatke iz koncentratora Azure događaj, obogaćuje podatke Upućivanjem podatke pohranjene u Azure DocumentDB i na kraju pohrane podataka u spremište Azure pomoću sistemske datoteke Hdps (HADOOP).

![HDInsight i dijagram arhitektura Internet stvari (IoT)](./media/hdinsight-storm-iot-eventhub-documentdb/iot.png)

##<a name="overview"></a>Pregled

Dodavanje senzori vozila omogućuje predviđanje opreme probleme koji se temelji na povijesnim podatkovne trendove, kao i poboljšati budućim verzijama na temelju analize korištenja uzorka. Dok tradicionalni MapReduce obrade moguće je koristiti za ovu analizu, moraju imati mogućnost da biste brzo i učinkovito učitali podatke iz svih vozila u Hadoop prije nego što se može dogoditi MapReduce obrada. Uz to, možda želite učiniti analizu puteva kritične pogreške (modul temperatura kočnice, itd.) u stvarnom vremenu.

Azure događaj koncentratora ugrađenih učiniti pretraživanje velikog količinu podataka generira senzori i Apache oluja na HDInsight moguće je koristiti za učitavanje i obradu podataka prije spremanje u HDFS (sigurnosno prostora za pohranu za Azure) za dodatnu obradu MapReduce.

##<a name="solution"></a>Rješenja

Telemetrijskih podataka za modul temperatura, razine okolne temperature i brzinu vozila snima po senzori šalje koncentratora događaja u automobilu vozila identifikacijski broj (VIN) i vremenske oznake. Iz nje, oluja topologije sustavom programa oluja Apache na HDInsight klaster čita podatke, obrađuje je i sprema u HDFS.

Tijekom obrade, u VIN se koristi za dohvaćanje informacija modela iz Azure DocumentDB. Time se dodaje toka podataka prije nego što je pohranjena.

Komponente korištene u topologiji oluja su:

* **EventHubSpout** - čita podatke iz koncentratora Azure događaja

* **TypeConversionBolt** – Pretvara niz JSON iz koncentratora događaja u n-torke koji sadrži vrijednosti pojedinih podataka za modul temperatura, razine okolne temperatura, brzinu, VIN i vremenske oznake

* **DataReferencBolt** - pretražuje vozila modela iz DocumentDB pomoću na VIN

* **WasbStoreBolt** - sprema podatke koje želite HDFS (prostora za pohranu za Azure)

Ovo je dijagram rješenja:

![Topologija oluja](./media/hdinsight-storm-iot-eventhub-documentdb/iottopology.png)

> [AZURE.NOTE] Ovo je dijagram pojednostavljeni i svaku komponentu rješenje može imati više instanci. Na primjer, više instanci svake komponente u topologiji su raspodijeliti čvorove u oluja na klasteru HDInsight.

##<a name="implementation"></a>Implementacija

Potpuni, automatski rješenje za ovaj scenarij dostupna je kao dio [HDInsight oluja Primjeri](https://github.com/hdinsight/hdinsight-storm-examples) spremište na GitHub. Da biste koristili u ovom primjeru, slijedite korake u [IoTExample README. MD](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/IotExample/README.md).

## <a name="next-steps"></a>Daljnji koraci

Dodatne topologija oluja primjer potražite u članku [primjer topologija za oluja na HDInsight](hdinsight-storm-example-topology.md).
