<properties
   pageTitle="Korištenje Hadoop Svinja u HDInsight | Microsoft Azure"
   description="Saznajte kako koristiti Svinja s Hadoop na HDInsight."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/14/2016"
   ms.author="larryfr"/>

# <a name="use-pig-with-hadoop-on-hdinsight"></a>Korištenje Svinja s Hadoop na HDInsight

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

[Svinja Apache](http://pig.apache.org/) je za stvaranje programe za Hadoop pomoću proceduralne jezik naziva *latinica Svinja*. Svinja je alternative Java za stvaranje rješenja *MapReduce* pa je u sklopu Azure HDInsight.

U ovom se članku opisano kako možete koristiti Svinja s HDInsight.

##<a id="why"></a>Zašto koristiti Svinja?

Jedno od izazove obrada podataka pomoću MapReduce u Hadoop je implementacijom logiku obrada pomoću samo kartu i smanjivanje funkcija. Za složene obradu često morate prekinuti obrada u više MapReduce operacije koje su povezane zajedno da biste postigli željeni rezultat.

Umjesto prisilno koje možete koristiti samo mapu i smanjiti funkcije, Svinja omogućuje vam da biste definirali obrada kao niz transformacije koji se podaci preljeva kroz daju željeni rezultat.

Svinja latiničnog jezika omogućuje opisuju toka podataka iz neobrađenog unosa, po jedan ili više transformacije, čime se dobiva željeni izlazni rezultat. Svinja latinica programe, slijedite ovaj Općenito uzorak:

- **Učitavanje**: čitanje podataka da biste se rukovati iz sustava datoteka
- **Pretvaranje**: rukovanje podacima
- **Ispis ili iz trgovine**: slanje podataka na zaslonu ili spremiti za obradu

Svinja latinica podržava korisnički definirane funkcije (UDF), koji omogućuje pozivanje vanjske komponente implementirati logiku koja je teško modela u latinica Svinja.

Dodatne informacije o Svinja latinica potražite u članku [Svinja latinica referenca ručno 1](http://pig.apache.org/docs/r0.7.0/piglatin_ref1.html) i [2 za ručno referenca latinica Svinja](http://pig.apache.org/docs/r0.7.0/piglatin_ref2.html).

Primjer korištenja UDF-ovi s Svinja, potražite u članku sljedeće dokumente:

* [Korištenje DataFu s Svinja u HDInsight](hdinsight-hadoop-use-pig-datafu-udf.md) - DataFu je zbirka korisno UDF-ove održava Apache

* [Korištenje Python s Svinja i grozd u HDInsight](hdinsight-python.md)

* [Korištenje C# s grozd i Svinja u HDInsight](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

##<a id="data"></a>O oglednim podacima

U ovom se primjeru koristi Ogledna datoteka *log4j* , koji je pohranjen na **/example/data/sample.log** u spremniku za pohranu na blob. Svaki zapisnika unutar datoteke sastoji se od redak polja koji sadrži na `[LOG LEVEL]` polja da bi se prikazala vrstu i težinu, na primjer:

    2012-02-03 20:26:41 SampleClass3 [ERROR] verbose detail for id 1527353937

U prethodnom primjeru razinu zapisnika je pogreška.

> [AZURE.NOTE] Možete i generiranje log4j datoteka pomoću alata za zapisivanje [Apache Log4j](http://en.wikipedia.org/wiki/Log4j) i zatim prenesite datoteke na blob. Upute potražite u članku [Prijenos podataka za HDInsight](hdinsight-upload-data.md) . Dodatne informacije o korištenju blob-ova u Azure spremište su sa servisa HDInsight potražite u članku [Korištenje blobova platforme Azure s HDInsight](hdinsight-hadoop-use-blob-storage.md).

Ogledni podaci pohranjena u spremište blobova platforme Azure, što HDInsight koristi kao zadani datotečni sustav za klastere Hadoop. HDInsight možete pristupiti datotekama pohranjenima u BLOB-ova pomoću prefiks **wasb** . Na primjer, da biste pristupili sample.log datoteku, služite sljedeću sintaksu:

    wasbs:///example/data/sample.log

Budući da je WASB zadani prostor za pohranu za HDInsight, datoteku možete pristupiti i pomoću **/example/data/sample.log** iz Svinja latinica.

> [AZURE.NOTE] Vidjet ćete da sintaksa, **wasbs: / / /**, koristi se za pristup datoteka pohranjenih u spremniku zadani prostor za pohranu za svoj klaster HDInsight. Ako dodatni prostor za pohranu računi koje ste naveli pri dodjeli svoj klaster, a želite pristupiti datotekama pohranjenima u tih računa, podatke možete pristupiti navođenjem spremnik naziv i pohranu računa adresu, na primjer: **wasbs://mycontainer@mystorage.blob.core.windows.net/example/data/sample.log**.


##<a id="job"></a>O uzorka posla

Sljedeći zadatak Svinja latinica učitava **sample.log** datoteke iz spremišta zadano za svoj klaster HDInsight. Zatim izvodi niz transformacije koji kao rezultat ukupan broj koliko je puta došlo je do svaku razinu zapisnika ulaznih podataka. Rezultati su dumped u STDOUT.

    LOGS = LOAD 'wasbs:///example/data/sample.log';
    LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
    FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
    GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
    FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
    RESULT = order FREQUENCIES by COUNT desc;
    DUMP RESULT;

Sljedeća slika prikazuje razrada svega svaki transformaciju funkcija s podacima.

![Grafički prikaz pretvorbe][image-hdi-pig-data-transformation]

##<a id="run"></a>Pokreni latinica Svinja

HDInsight mogu se izvoditi Svinja latinica zadatke pomoću na različite načine. U sljedećoj su tablici pomoću odlučiti koju metodu vam najviše odgovara, a zatim slijedite vezu vodič.

| **Tu mogućnost koristite** ako želite...                                   | .. .an **interaktivne** shell | ... **Skupna** obrada | .. .with **klaster operacijski sustav** | .. .from taj **klijent operacijski sustav** |
|:--------------------------------------------------------------|:---------------------------:|:-----------------------:|:------------------------------------------|:-----------------------------------------|
| [SSH](hdinsight-hadoop-use-pig-ssh.md)                        |              ✔              |            ✔            | Linux                                     | Linux, Unix, Mac OS X ili Windows        |
| [Zakretanja](hdinsight-hadoop-use-pig-curl.md)                      |           &nbsp;            |            ✔            | Linux ili Windows                          | Linux, Unix, Mac OS X ili Windows        |
| [.NET SDK Hadoop](hdinsight-hadoop-use-pig-dotnet-sdk.md) |           &nbsp;            |            ✔            | Linux ili Windows                          | Windows (Zasad)                        |
| [Komponente Windows PowerShell](hdinsight-hadoop-use-pig-powershell.md)  |           &nbsp;            |            ✔            | Linux ili Windows                          | Windows                                  |
| [Udaljena radna površina](hdinsight-hadoop-use-pig-remote-desktop.md)  |              ✔              |            ✔            | Windows                                   | Windows                                  |


## <a name="running-pig-jobs-on-azure-hdinsight-using-on-premises-sql-server-integration-services"></a>Izvođenje zadataka Svinja na Azure HDInsight pomoću lokalnog SQL Integracijskim uslugama poslužitelja

Da biste pokrenuli Svinja posao možete koristiti i SQL Server integraciju servisa (SSIS). Azure paket značajki za SSIS nudi sljedeće komponente koje funkcioniraju sa zadacima Svinja na HDInsight.


- [Azure HDInsight Svinja zadatka][pigtask]
- [Azure Upravitelja veze za pretplatu][connectionmanager]


Dodatne informacije o paket značajki za Azure za SSIS [ovdje][ssispack].


##<a id="nextsteps"></a>Daljnji koraci

Sad kad ste naučili kako koristiti Svinja s HDInsight, koristite sljedeće veze da biste istražili druge načine za rad sa servisom Azure HDInsight.

* [Prijenos podataka HDInsight][hdinsight-upload-data]
* [Korištenje grozd s HDInsight][hdinsight-use-hive]
* [Korištenje Sqoop s HDInsight](hdinsight-use-sqoop.md)
* [Korištenje Oozie s HDInsight](hdinsight-use-oozie.md)
* [Korištenje MapReduce poslove s HDInsight][hdinsight-use-mapreduce]

[check]: ./media/hdinsight-use-pig/hdi.checkmark.png

[apachepig-home]: http://pig.apache.org/
[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html
[curl]: http://curl.haxx.se/
[pigtask]: http://msdn.microsoft.com/library/mt146781(v=sql.120).aspx
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx

[hdinsight-storage]: hdinsight-use-blob-storage.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: ../hdinsight-get-started.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md#mapreduce-sdk

[Powershell-install-configure]: ../powershell-install-configure.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx

[image-hdi-log4j-sample]: ./media/hdinsight-use-pig/HDI.wholesamplefile.png
[image-hdi-pig-data-transformation]: ./media/hdinsight-use-pig/HDI.DataTransformation.gif
[image-hdi-pig-powershell]: ./media/hdinsight-use-pig/hdi.pig.powershell.png
[image-hdi-pig-architecture]: ./media/hdinsight-use-pig/HDI.Pig.Architecture.png
