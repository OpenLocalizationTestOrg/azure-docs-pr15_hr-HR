<properties
    pageTitle="Napomene za Hadoop komponente na Azure HDInsight | Microsoft Azure"
    description="Najnovije izdanje bilješke i verzije Hadoop komponente za Azure HDInsight. Pribaviti Savjeti za razvoj i detalje o Hadoop, oluja Apache i HBase."
    services="hdinsight"
    documentationCenter=""
    editor="cgronlun"
    manager="jhubbard"
    authors="nitinme"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/27/2016"
    ms.author="nitinme"/>


# <a name="release-notes-for-hadoop-components-on-azure-hdinsight"></a>Napomene za Hadoop komponente na Azure HDInsight

## <a name="notes-for-10262016-release-of-r-server-on-hdinsight"></a>Napomene uz izdanje 26/10/2016 R Normalni prikaz na HDInsight

- R poslužitelju za dodjelu resursa klaster HDInsight ima je pojednostavnjen.
- R Normalni prikaz na HDInsight sada je dostupan kao obični HDInsight "R poslužitelja" skupine vrsta i više nije instaliran kao zasebna aplikacija HDInsight. Čvor rub i R poslužitelja binarne datoteke sada dodjeli kao dio implementacije klaster R poslužitelja. To poboljšava brzina i pouzdanost dodjele resursa. Cijene model za poslužitelj R ažurira sukladno tome.
- R poslužitelja klaster vrsta cijena sada se temelji na standardni sloju cijena plus cijena dodatna naknada R poslužitelja. Razina Premium sada biti rezervirana za Premium značajkama kroz različite vrste, a će se koristiti za vrstu klaster R poslužitelja. Ta promjena ne utječe na stvarni cijene poslužitelja R, samo se mijenja način predstavljanja naknada u naplate. Sve postojeće R poslužitelja klastere nastavit će funkcionirati i predloške ARM će se i dalje funkcionirati do obavijest o postupku. **Preporučuje se iako da biste ažurirali skriptiranih implementacijama koristiti novi predložak OKVIRA.**


## <a name="notes-for-08302016-release-of-r-server-on-hdinsight"></a>Napomene uz izdanje 08/30/2016 R Normalni prikaz na HDInsight

Brojevi punu verziju za klastere sustavom Linux HDInsight implementiran s ovom izdanju:

|HDI |Verzija HDI klaster   |HDP |Sastavljanje HDP   |Sastavljanje Ambari |
|----|----------------------|----|------------|-------------|
|3,2 |3.2.1000.0.8268980    |2.2 |2.2.9.1-19  |2.2.1.12-4   |
|3,3 |3.3.1000.0.8268980    |2.3 |2.3.3.1-25  |2.2.1.12-4   |
|3.4 |3.4.1000.0.8269383    |2.4 |2.4.2.4-5   |2.2.1.12-4   |

Brojevi punu verziju za klastere utemeljen na sustavu Windows HDInsight implementiran s ovom izdanju:

|HDI |Verzija HDI klaster   |HDP |Sastavljanje HDP     |
|----|----------------------|----|--------------|
|2.1 |2.1.10.1033.2559206   |1.3 |1.3.12.0-01795|
|3.0 |3.0.6.1033.2559206    |2.0 |2.0.13.0-2117 |
|3.1 |3.1.4.1033.2559206    |2.1 |2.1.16.0-2374 |
|3,2 |3.2.7.1033.2559206    |2.2 |2.2.9.1-11    |
|3,3 |3.3.0.1033.2559206    |2.3 |2.3.3.1-25    |

## <a name="notes-for-08172016-release-of-r-server-on-hdinsight"></a>Napomene uz izdanje 08/17/2016 R Normalni prikaz na HDInsight

- R Server 8.0.5 – uglavnom izdanje otklanjanje problema. [R poslužitelja napomene](https://msdn.microsoft.com/microsoft-r/notes/r-server-notes) za dodatne informacije potražite u članku. 
- Paket AzureML na rub čvor – [paket R](https://cran.r-project.org/web/packages/AzureML/vignettes/getting_started.html) omogućuje modelima R da biste objavili i Potrošena kao na web-servisa Azure ML.  U odjeljku ["Operationalize na Model"](hdinsight-hadoop-r-server-overview.md#operationalize-a-model) naš ["Pregled od R poslužitelja na HDInsight"](hdinsight-hadoop-r-server-overview.md) članka dodatne informacije.
- Zavisnosti Linux na [gornjoj paketa R najpopularnijih 100](https://github.com/metacran/cranlogs) – te Linux ovisnosti paketa sada unaprijed instaliran.  
- Mogućnost korištenja CRAN repo prilikom dodavanja R paketi za čvorove podataka. U odjeljku ["R instalaciju paketa"](hdinsight-hadoop-r-server-get-started.md#install-r-packages) naš ["Početak rada s poslužiteljem R na HDInsight"](hdinsight-hadoop-r-server-get-started.md) članka dodatne informacije.
- Veća pouzdanost R poslužitelja aktiviranja stvaranja klastere.


## <a name="notes-for-08012016-release-of-hdinsight"></a>Napomene uz izdanje 08/01/2016 HDInsight

Brojevi punu verziju za klastere sustavom Linux HDInsight implementiran s ovom izdanju:

|HDI |Verzija HDI klaster   |HDP |Sastavljanje HDP   |Sastavljanje Ambari |
|----|----------------------|----|------------|-------------|
|3,2 |3.2.1000.0.8028416    |2.2 |2.2.9.1-19  |2.2.1.12-4   |
|3,3 |3.3.1000.0.8028416    |2.3 |2.3.3.1-25  |2.2.1.12-4   |
|3.4 |3.4.1000.0.8053402    |2.4 |2.4.2.4-5   |2.2.1.12-4   |

Brojevi punu verziju za klastere utemeljen na sustavu Windows HDInsight implementiran s ovom izdanju:

|HDI |Verzija HDI klaster   |HDP |Sastavljanje HDP     |
|----|----------------------|----|--------------|
|2.1 |2.1.10.1005.2488842   |1.3 |1.3.12.0-01795|
|3.0 |3.0.6.1005.2488842    |2.0 |2.0.13.0-2117 |
|3.1 |3.1.4.1005.2488842    |2.1 |2.1.16.0-2374 |
|3,2 |3.2.7.1005.2488842    |2.2 |2.2.9.1-11    |
|3,3 |3.3.0.1005.2488842    |2.3 |2.3.3.1-25    |

Ovo izdanje sadrži sljedeća ažuriranja.

| Naslov                                           | Opis                                          | Područje utjecati (na primjer, servis, komponente ili SDK) | Vrsta klaster (na primjer Spark, Hadoop, HBase ili oluja) | JIRA (Ako je primjenjivo) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Promjene klastere HDInsight 3.4 | Zadane vrijednosti sljedećih konfiguracija grozd mijenjaju za bolje performanse <ul><li>`hive.vectorized.execution.reduce.enabled=true`</li><li>`hive.tez.min.partition.factor=1f`</li><li>`hive.tez.max.partition.factor=3f`</li><li>`tez.shuffle-vertex-manager.min-src-fraction=0.9`</li><li>`tez.shuffle-vertex-manager.max-src-fraction=0.95`</li><li>`tez.runtime.shuffle.connect.timeout= 30000`</li></ul>| Servis    | Sve| N/D|
| Sljedeća rješenja uključeni u ovom izdanju | GROZD 13632, HIVE-12897,HIVE-12907,HIVE-12908,HIVE-12988,HIVE-13510,HIVE-13572,HIVE-13716,HIVE-13726,HIVE-12505,HIVE-13632,HIVE-13661,HIVE-13705,HIVE-13743,HIVE-13810,HIVE-13857,HIVE-13902,HIVE-13911,HIVE-13933| Servis    | Sve| N/D

## <a name="notes-for-07142016-release-of-hdinsight"></a>Napomene uz izdanje 07/14/2016 HDInsight

Brojevi punu verziju za klastere sustavom Linux HDInsight implementiran s ovom izdanju:

|HDI |Verzija HDI klaster   |HDP |Sastavljanje HDP   |Sastavljanje Ambari |
|----|----------------------|----|------------|-------------|
|3,2 |3.2.1000.0.7932505    |2.2 |2.2.9.1-11  |2.2.1.12-2   |
|3,3 |3.3.1000.0.7932505    |2.3 |2.3.3.1-18  |2.2.1.12-2   |
|3.4 |3.4.1000.0.7933003    |2.4 |2.4.2.0     |2.2.1.12-2   |

Brojevi punu verziju za klastere utemeljen na sustavu Windows HDInsight implementiran s ovom izdanju:

|HDI |Verzija HDI klaster   |HDP |Sastavljanje HDP     |
|----|----------------------|----|--------------|
|2.1 |2.1.10.989.2441725    |1.3 |1.3.12.0-01795|
|3.0 |3.0.6.989.2441725     |2.0 |2.0.13.0-2117 |
|3.1 |3.1.4.989.2441725     |2.1 |2.1.16.0-2374 |
|3,2 |3.2.7.989.2441725     |2.2 |2.2.9.1-11    |
|3,3 |3.3.0.989.2441725     |2.3 |2.3.3.1-21    |

## <a name="notes-for-07072016-release-of-hdinsight"></a>Napomene uz izdanje 07/07/2016 HDInsight

Brojevi punu verziju za klastere sustavom Linux HDInsight implementiran s ovom izdanju:

|HDI |Verzija HDI klaster   |HDP |Sastavljanje HDP   |
|----|----------------------|----|------------|
|3,2 |3.2.1000.0.7864996    |2.2 |2.2.9.1-11  |
|3,3 |3.3.1000.0.7864996    |2.3 |2.3.3.1-18  |
|3.4 |3.4.1000.0.7861906    |2.4 |2.4.2.0     |

Brojevi punu verziju za klastere utemeljen na sustavu Windows HDInsight implementiran s ovom izdanju:

|HDI |Verzija HDI klaster   |HDP |Sastavljanje HDP     |
|----|----------------------|----|--------------|
|2.1 |2.1.10.977.2413853    |1.3 |1.3.12.0-01795|
|3.0 |3.0.6.977.2413853     |2.0 |2.0.13.0-2117 |
|3.1 |3.1.4.977.2413853     |2.1 |2.1.16.0-2374 |
|3,2 |3.2.7.977.2413853     |2.2 |2.2.9.1-11    |
|3,3 |3.3.0.977.2413853     |2.3 |2.3.3.1-21    |

Ovo izdanje sadrži sljedeća ažuriranja.

| Naslov                                           | Opis                                          | Područje utjecati (na primjer, servis, komponente ili SDK) | Vrsta klaster (na primjer Spark, Hadoop, HBase ili oluja) | JIRA (Ako je primjenjivo) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| [Alati za HDInsight za IntelliJ](hdinsight-apache-spark-intellij-tool-plugin.md) | Dodatak IntelliJ IDEJA za klastere HDInsight Spark sada integriran s Azure komplet alata za IntelliJ. Podržava Azure SDK v2.9.1, najnovije Java SDK-ovi, a obuhvaća sve značajke iz samostalne HDInsight dodatak za IntelliJ.| Alati    | Spark| N/D|
| [Alati za HDInsight za Eclipse](hdinsight-apache-spark-eclipse-tool-plugin.md) | Azure komplet alata za Eclipse sada podržava klastere HDInsight Spark. Omogućuje sljedeće značajke. <ul><li>Stvaranje i pisanje Spark aplikacije jednostavno u Scala i Java prve klase authoring podrška za značajke IntelliSense automatskog oblikovanja, provjera pogrešaka itd.</li><li>Testirajte aplikacije Spark lokalno.</li><li>Slanje poslove HDInsight Spark klaster i dohvatiti rezultate.</li><li>Prijavite se u Azure i pristup sve Spark klastere pridružene pretplate Azure.</li><li>Pronađite sve povezane pohranu resurse svoj klaster HDInsight Spark.</li></ul>| Alati    | Spark| N/D

Počevši s ovom izdanju, ne možemo promijenile OS goste kojemu pravila za klastere sustavom Linux HDInsight. Cilj novog pravilnika je znatno smanjiti broj ponovna zbog zakrpa. Nova pravila će i dalje zakrpu virtualnim strojevima (VMs) na Linux klastere svaku ponedjeljak ili četvrtak počevši od 12 AM UTC nestalnog način preko čvorove u sve navedene klaster. Međutim, sve navedene VM samo ponovno najviše jedanput u 30 dana zbog zakrpa za goste OS. Osim toga, prvo pokretanje za novostvorenu klaster ne dogodit će odrediti od 30 dana od datuma stvaranja klaster.

>[AZURE.NOTE] Te promjene će se primijeniti samo na novostvoreni klastere jednaka ili veća od ovu verziju.

## <a name="notes-for-06062016-release-of-hdinsight"></a>Napomene uz izdanje 06/06/2016 HDInsight

Brojevi punu verziju za klastere HDInsight implementiran s ovom izdanju:

|HDP    |HDI verzija    |Spark verzija  |Broj Ambari izdanja    |Broj HDP izdanja|
|-------|---------------|---------------|-----------------------|----------------|
|2.3    |3.3.1000.0.7702215|    1.5.2|  2.2.1.8-2|  2.3.3.1-18|
|2.4    |3.4.1000.0.7702224|    1.6.1|  2.2.1.8-2|  2.4.2.0|


Ovo izdanje sadrži sljedeća ažuriranja.

| Naslov                                           | Opis                                          | Područje utjecati (na primjer, servis, komponente ili SDK) | Vrsta klaster (na primjer Spark, Hadoop, HBase ili oluja) | JIRA (Ako je primjenjivo) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Spark na HDInsight je Općenito dostupan | U ovom izdanju poziva poboljšanja u dostupnost, skalabilnost i produktivnost da biste otvorili izvor Apache Spark na HDInsight. <ul><li>Industrijske početnih dostupnost SLA % 99.9 čega prikladna za zahtjevne radnih opterećenja enterprise.</li><li>Sloj skalabilni prostora za pohranu Azure podataka Lake spremištu.</li><li>Alati za produktivnost za svaku faza podataka Istraživanje i razvoj. Jupyter bilježnica s prilagođenim otklanjanje Spark omogućiti istraživanje interaktivne podatke, Integracija s nadzorne ploče BI kao što je Power BI, Tableau i Qlik je dobar za zajedničko korištenje brzo podataka i neprekinuti izvješćivanja, IntelliJ dodatak je pouzdan mogućnost za dugoročnu kod artefakt razvoja i ispravljanje pogrešaka.</li></ul>| Servis    | Spark| N/D|
| Alati za HDInsight za IntelliJ | Ovo je dodatak za IntelliJ IDEJA za klastere HDInsight Spark. Omogućuje sljedeće značajke.<ul><li>Stvaranje i pisanje Spark aplikacije jednostavno u Scala i Java prve klase authoring podrška za značajke IntelliSense automatskog oblikovanja, provjera pogrešaka itd.</li><li>Testirajte aplikacije Spark lokalno.</li><li>Slanje poslove HDInsight Spark klaster i dohvatiti rezultate.</li><li>Prijavite se u Azure i pristup sve Spark klastere pridružene pretplate Azure.</li><li>Pronađite sve povezane prostora za pohranu resurse svoj klaster HDInsight Spark.</li><li>Kretanje povijesti zadataka i informacije o zadatku za svoj klaster HDInsight Spark.</li><li>Ispravljanje pogrešaka Spark poslove daljinski na stolnom računalu.</li></ul>| Alati    | Spark| N/D

## <a name="notes-for-05132016-release-of-hdinsight"></a>Napomene uz izdanje 05/13/2016 HDInsight

Brojevi punu verziju za klastere HDInsight implementiran s ovom izdanju:

* HDInsight (Windows) 2.1.10.875.2159884 (HDP 1.3.12.0-01795 - nepromijenjena)
* HDInsight (Windows) 3.0.6.875.2159884 (HDP 2.0.13.0-2117 - nepromijenjena)
* HDInsight (Windows) 3.1.4.922.2266903 (HDP 2.1.15.0-2374 - nepromijenjena)
* HDInsight (Windows) 3.2.7.922.2266903 (HDP 2.2.9.1-11)
* HDInsight (Windows) 3.3.0.922.2266903 (HDP 2.3.3.1-18)
* HDInsight (Linux) 3.2.1000.0.7565644 (HDP 2.2.9.1-11)
* HDInsight (Linux) 3.3.1000.0.7565644 (HDP 2.3.3.1-18)
* HDInsight (Linux) 3.4.1000.0.7548380 (HDP 2.4.2.0)

Ovo izdanje sadrži sljedeća ažuriranja.

| Naslov                                           | Opis                                          | Područje utjecati (na primjer, servis, komponente ili SDK) | Vrsta klaster (na primjer Spark, Hadoop, HBase ili oluja) | JIRA (Ako je primjenjivo) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Mogu potaknuti ažuriranje verziju i druge popravci programskih pogrešaka | Ovo izdanje ažurira u HDInsight klaster verzija Spark do 1.6.1 i popravlja druge programskih pogrešaka| Servis    | Spark| N/D

## <a name="notes-for-04112016-release-of-hdinsight"></a>Napomene uz izdanje 11/04/2016 HDInsight

Brojevi punu verziju za klastere HDInsight implementiran s ovom izdanju:

* HDInsight (Windows) 2.1.10.889.2191206 (HDP 1.3.12.0-01795 - nepromijenjena)
* HDInsight (Windows) 3.0.6.889.2191206 (HDP 2.0.13.0-2117 - nepromijenjena)
* HDInsight (Windows) 3.1.4.889.2191206 (HDP 2.1.15.0-2374 - nepromijenjena)
* HDInsight (Windows) 3.2.7.889.2191206 (HDP 2.2.9.1-10)
* HDInsight (Windows) 3.3.0.889.2191206 (HDP 2.3.3.1-16-nepromijenjen)
* HDInsight (Linux) 3.2.1000.0.7339916 (HDP 2.2.9.1-10)
* HDInsight (Linux) 3.3.1000.0.7339916 (HDP 2.3.3.1-16)
* HDInsight (Linux) 3.4.1000.0.7338911 (HDP 2.4.1.1-3)
* SDK 1.5.8

Ovo izdanje sadrži sljedeća ažuriranja.

| Naslov                                           | Opis                                          | Područje utjecati (na primjer, servis, komponente ili SDK) | Skupine vrstu (na primjer, Hadoop, HBase ili oluja) | JIRA (Ako je primjenjivo) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Nadogradnja prilagođene metastore problemi za HDI 3.4 | Stvaranje klaster će se neće uspjeti ako ste koristili prilagođeni metastore koja se koristila prethodno na prethodnu verziju drugi klaster HDInsight. To je zbog nadogradnje pogreške skripte sada fiksiran| Stvaranje klaster    | Sve | N/D
| Oporavak rušenje Livije | Nudi otpornost status posla za bilo koji zadatak šalje putem Livije | Pouzdanost | Spark na Linux| N/D
| Sadržaj Jupyter HA | Omogućuje spremanje i učitavanje Jupyter sadržaj bilježnice i s računa za pohranu pridruženog klaster. Dodatne informacije potražite u članku [jezgre dostupne za Jupyter bilježnice](hdinsight-apache-spark-jupyter-notebook-kernels.md).| Bilježnica | Spark na Linux| N/D
| Uklanjanje hiveContext u bilježnicama Jupter | Korištenje `%%sql` uz umjesto `%%hive` poseban. SqlContext jednako hiveContext. Dodatne informacije potražite u članku [jezgre dostupne za Jupyter bilježnice](hdinsight-apache-spark-jupyter-notebook-kernels.md)| Bilježnica    | Spark klastere na Linux| N/D
| Postupku neke starije verzije Spark | Starija verzija Spark 1.3.1 bit će uklonjen s usluge na 5/31 | Servis | Spark klastere u sustavu Windows | N/D

## <a name="notes-for-03292016-release-of-hdinsight"></a>Napomene uz izdanje 03/29/2016 HDInsight

Brojevi punu verziju za klastere HDInsight implementiran s ovom izdanju:

* HDInsight (Windows) 2.1.10.875.2159884 (HDP 1.3.12.0-01795 - nepromijenjena)
* HDInsight (Windows) 3.0.6.875.2159884 (HDP 2.0.13.0-2117 - nepromijenjena)
* HDInsight (Windows) 3.1.4.875.2159884 (HDP 2.1.15.0-2374 - nepromijenjena)
* HDInsight (Windows) 3.2.7.875.2159884 (HDP 2.2.9.1-7 - nepromijenjena)
* HDInsight (Windows) 3.3.0.875.2159884 (HDP 2.3.3.1-16)
* HDInsight (Linux) 3.2.1000.0.7193255 (HDP 2.2.9.1-8 - nepromijenjena)
* HDInsight (Linux) 3.3.1000.0.7193255 (HDP 2.3.3.1-7 - nepromijenjena)
* HDInsight (Linux) 3.4.1000.0.7195842 (HDP 2.4.1.0-327)
* SDK 1.5.8

Ovo izdanje sadrži sljedeća ažuriranja.

| Naslov                                           | Opis                                          | Područje utjecati (na primjer, servis, komponente ili SDK) | Skupine vrstu (na primjer, Hadoop, HBase ili oluja) | JIRA (Ako je primjenjivo) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Dodane HDInsight 3.4 verzije i ažurirane verzije HDP za sve klastere HDInsight | To je izdanje smo dodane HDInsight v3.4 (koja se temelji na HDP 2.4) i ste ažurirali i druge verzije HDP. Napomene u HDP 2.4 su dostupne [u nastavku](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.4.0/bk_HDP_RelNotes/content/ch_relnotes_v240.html) i dodatne informacije o HDInsight verzija može biti pronađene [ovdje](hdinsight-component-versioning.md).| Servis    | Sve klastere Linux| N/D
| HDInsight Premium | HDInsight sada je dostupna na dvije kategorije – standardna i Premium. HDInsight Premium je trenutno u pretpregledu dostupna samo za Hadoop i Spark klaster na Linux. Dodatne informacije potražite [u nastavku](hdinsight-component-versioning.md#hdinsight-standard-and-hdinsight-premium).| Servis    | Hadoop i Spark na Linux| N/D
| Microsoft R poslužitelja | HDInsight Premium nudi Microsoft R Server koji se mogu uključiti Hadoop i Spark klastere na Linux. Dodatne informacije potražite u članku [Pregled od R Normalni prikaz na HDInsight](hdinsight-hadoop-r-server-overview.md).| Servis    | Hadoop i Spark na Linux| N/D
| Spark 1.6.0 | HDInsight 3.4 klastere sada uključuju Spark 1.6.0| Servis    | Spark klastere na Linux| N/D
| Poboljšanja Jupyter bilježnice | Bilježnica Jupyter dostupno u sklopu Spark klastere sada sadrže dodatne Spark jezgre. Sadrže poboljšanja kao što su korištenje %% poseban, automatski vizualizaciju i integracija s bibliotekama vizualizacije Python (primjerice matplotlib). Dodatne informacije potražite u članku [jezgre dostupne za Jupyter bilježnice](hdinsight-apache-spark-jupyter-notebook-kernels.md). | Servis | Spark klastere na Linux | N/D

## <a name="notes-for-03222016-release-of-hdinsight"></a>Napomene uz izdanje 03/22/2016 HDInsight

Brojevi punu verziju za klastere HDInsight implementiran s ovom izdanju:

* HDInsight (Windows) 2.1.10.875.2159884 (HDP 1.3.12.0-01795 - nepromijenjena)
* HDInsight (Windows) 3.0.6.875.2159884 (HDP 2.0.13.0-2117 - nepromijenjena)
* HDInsight (Windows) 3.1.4.875.2159884 (HDP 2.1.15.0-2374 - nepromijenjena)
* HDInsight (Windows) 3.2.7.875.2159884 (HDP 2.2.9.1-7 - nepromijenjena)
* HDInsight (Windows) 3.3.0.875.2159884 (HDP 2.3.3.1-16)
* HDInsight (Linux) 3.2.1000.0.7193255 (HDP 2.2.9.1-8 - nepromijenjena)
* HDInsight (Linux) 3.3.1000.0.7193255 (HDP 2.3.3.1-7 - nepromijenjena)
* SDK 1.5.8

Ovo izdanje sadrži sljedeća ažuriranja.

| Naslov                                           | Opis                                          | Područje utjecati (na primjer, servis, komponente ili SDK) | Skupine vrstu (na primjer, Hadoop, HBase ili oluja) | JIRA (Ako je primjenjivo) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Ažurirane verzije HDInsight za sve klastere HDInsight | Uz to je izdanje ažuriranja HDInsight verzije za sve klastere HDInsight| Servis    | Sve| N/D


## <a name="notes-for-03102016-release-of-hdinsight"></a>Napomene uz izdanje 03/10/2016 HDInsight

Brojevi punu verziju za klastere HDInsight implementiran s ovom izdanju:

* HDInsight (Windows) 2.1.10.859.2123216 (HDP 1.3.12.0-01795 - nepromijenjena)
* HDInsight (Windows) 3.0.6.859.2123216 (HDP 2.0.13.0-2117 - nepromijenjena)
* HDInsight (Windows) 3.1.4.859.2123216 (HDP 2.1.15.0-2374 - nepromijenjena)
* HDInsight (Windows) 3.2.7.859.2123216 (HDP 2.2.9.1-7)
* HDInsight (Windows) 3.3.0.859.2123216 (HDP 2.3.3.1-5 - nepromijenjena)
* HDInsight (Linux) 3.2.1000.7076817 (HDP 2.2.9.1-8)
* HDInsight (Linux) 3.3.1000.7076817 (HDP 2.3.3.1-7)
* SDK 1.5.8

Ovo izdanje sadrži sljedeća ažuriranja.

| Naslov                                           | Opis                                          | Područje utjecati (na primjer, servis, komponente ili SDK) | Skupine vrstu (na primjer, Hadoop, HBase ili oluja) | JIRA (Ako je primjenjivo) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Ažurirane verzije HDInsight za sve klastere HDInsight | Uz to je izdanje ažuriranja HDInsight verzije za sve klastere HDInsight| Servis    | Sve| N/D

## <a name="notes-for-01272016-release-of-hdinsight"></a>Napomene uz izdanje 27/01/2016 HDInsight

Brojevi punu verziju za klastere HDInsight implementiran s ovom izdanju:

* HDInsight (Windows) 2.1.10.817.2028315 (HDP 1.3.12.0-01795 - nepromijenjena)
* HDInsight (Windows) 3.0.6.817.2028315 (HDP 2.0.13.0-2117 - nepromijenjena)
* HDInsight (Windows) 3.1.4.817.2028315 (HDP 2.1.15.0-2374 - nepromijenjena)
* HDInsight (Windows) 3.2.7.817.2028315 (HDP 2.2.9.1-1)
* HDInsight (Windows) 3.3.0.817.2028315 (HDP 2.3.3.1-5 - nepromijenjena)
* HDInsight (Linux) 3.2.1000.4072335 (HDP 2.2.9.1-1)
* HDInsight (Linux) 3.3.1000.4072335 (HDP 2.3.3.1-1)
* SDK 1.5.8

Ovo izdanje sadrži sljedeća ažuriranja.

| Naslov                                           | Opis                                          | Područje utjecati (na primjer, servis, komponente ili SDK) | Skupine vrstu (na primjer, Hadoop, HBase ili oluja) | JIRA (Ako je primjenjivo) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Ažurirane verzije HDInsight za sve klastere HDInsight | Uz to je izdanje ažuriranja HDInsight verzije za sve klastere HDInsight| Servis    | Sve| N/D

## <a name="notes-for-12022015-release-of-hdinsight"></a>Napomene uz izdanje 02/12/2015 HDInsight

Brojevi punu verziju za klastere HDInsight implementiran s ovom izdanju:

* HDInsight (Windows) 2.1.10.763.1931434 (HDP 1.3.12.0-01795 - nepromijenjena)
* HDInsight (Windows) 3.0.6.763.1931434 (HDP 2.0.13.0-2117 - nepromijenjena)
* HDInsight (Windows) 3.1.4.763.1931434 (HDP 2.1.15.0-2374 - nepromijenjena)
* HDInsight (Windows) 3.2.7.763.1931434 (HDP 2.2.7.1-34 - nepromijenjena)
* HDInsight (Windows) 3.3.1000.0 (HDP 2.3.3.1-5)
* HDInsight (Linux) 3.2.1000.0.6392801 (HDP 2.2.7.1-34 - nepromijenjena)
* HDInsight (Linux) 3.3.1000.0 (HDP 2.3.3.0-3039)
* SDK 1.5.8

Ovo izdanje sadrži sljedeća ažuriranja.

| Naslov                                           | Opis                                          | Područje utjecati (na primjer, servis, komponente ili SDK) | Skupine vrstu (na primjer, Hadoop, HBase ili oluja) | JIRA (Ako je primjenjivo) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Dodane 3,3 HDInsight verzije i ažurirane verzije HDP za sve klastere HDInsight | To je izdanje smo dodane HDInsight v3.3 (koja se temelji na HDP 2.3) i ste ažurirali i druge verzije HDP. Napomene u HDP 2.3 su dostupne [u nastavku](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.3.0/bk_HDP_RelNotes/content/ch_relnotes_v230.html) i dodatne informacije o HDInsight verzija može biti pronađene [ovdje](hdinsight-component-versioning.md).| Servis    | Sve| N/D

## <a name="notes-for-11302015-release-of-hdinsight"></a>Napomene uz izdanje 30/11/2015 HDInsight

Brojevi punu verziju za klastere HDInsight implementiran s ovom izdanju:

* HDInsight (Windows) 2.1.10.757.1923908 (HDP 1.3.12.0-01795 - nepromijenjena)
* HDInsight (Windows) 3.0.6.757.1923908 (HDP 2.0.13.0-2117 - nepromijenjena)
* HDInsight (Windows) 3.1.4.757.1923908 (HDP 2.1.15.0-2374 - nepromijenjena)
* HDInsight (Windows) 3.2.7.757.1923908 (HDP 2.2.7.1-34)
* HDInsight (Linux) 3.2.1000.0.6392801 (HDP 2.2.7.1-34)
* SDK 1.5.8

Ovo izdanje sadrži sljedeća ažuriranja.

| Naslov                                           | Opis                                          | Područje utjecati (na primjer, servis, komponente ili SDK) | Skupine vrstu (na primjer, Hadoop, HBase ili oluja) | JIRA (Ako je primjenjivo) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Ažurirane verzije HDInsight za sve klastere HDInsight i HDP verzije za klastere HDInsight 3,2 (Windows i Linux) | Uz to je izdanje HDInsight i HDP verzije su ažurirani | Servis    | Sve| N/D


## <a name="notes-for-10272015-release-of-hdinsight"></a>Napomene uz izdanje 27/10/2015 HDInsight

Brojevi punu verziju za klastere HDInsight implementiran s ovom izdanju:

* HDInsight (Windows) 2.1.10.726.1866228 (HDP 1.3.12.0-01795 - nepromijenjena)
* HDInsight (Windows) 3.0.6.726.1866228 (HDP 2.0.13.0-2117 - nepromijenjena)
* HDInsight (Windows) 3.1.4.726.1866228 (HDP 2.1.15.0-2374 - nepromijenjena)
* HDInsight (Windows) 3.2.7.726.1866228 (HDP 2.2.7.1-33)
* HDInsight (Linux) 3.2.1000.0.6035701 (HDP 2.2.7.1-33)
* SDK 1.5.8

Ovo izdanje sadrži sljedeća ažuriranja.

| Naslov                                           | Opis                                          | Područje utjecati (na primjer, servis, komponente ili SDK) | Skupine vrstu (na primjer, Hadoop, HBase ili oluja) | JIRA (Ako je primjenjivo) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Ažurirane verzije HDInsight za sve klastere HDInsight (Windows i Linux) | Uz to je izdanje HDInsight i HDP verzije su ažurirani | Servis    | Sve| N/D
| FIXED klastere Jupyter za Windows Spark s klastere velika slova | Klastere koja je imala DNS naziva naveden u velika slova imali problema s bilježnicama Jupyter zbog programa potvrdite zahtjev polazište. Popravak je da biste promijenili naziv DNS-a za konfiguraciju Jupyter, u mala slova. | Servis    | HDInsight Spark (Windows)| N/D


## <a name="notes-for-10202015-release-of-hdinsight"></a>Napomene uz izdanje 20/10/2015 HDInsight

Brojevi punu verziju za klastere HDInsight implementiran s ovom izdanju:

* HDInsight 2.1.10.716.1846990 (Windows) (HDP 1.3.12.0-01795 - nepromijenjena)
* HDInsight 3.0.6.716.1846990 (Windows) (HDP 2.0.13.0-2117 - nepromijenjena)
* HDInsight 3.1.4.716.1846990 (Windows) (HDP 2.1.16.0-2374)
* HDInsight 3.2.7.716.1846990 (Windows) (HDP 2.2.7.1-0004)
* HDInsight 3.2.1000.0.5930166 (Linux) (HDP 2.2.7.1-0004)
* SDK 1.5.8

Ovo izdanje sadrži sljedeća ažuriranja.

| Naslov                                           | Opis                                          | Područje utjecati (na primjer, servis, komponente ili SDK) | Skupine vrstu (na primjer, Hadoop, HBase ili oluja) | JIRA (Ako je primjenjivo) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Zadana HDP verzija mijenjaju se HDP 2.2 | Na zadanu verziju za klastere HDInsight Windows mijenja se u HDP 2.2. HDInsight verzija 3,2 (HDP 2.2) je Općenito dostupan u od veljača 2015. Ta promjena samo Zrcali na zadanu verziju klaster, kad eksplicitnih odabira nije izvršena prilikom dodjele resursa klaster pomoću portala za Azure, cmdleta ljuske PowerShell ili SDK-a. | Servis    | Sve| N/D                  |
|Promjene u oblik naziva VM za implementaciju više HDInsight na Linux klastere u jednom virtualne mreže | Podrška za implementaciju više klastere HDInsight Linux u jednom virtualne mreže dodaje se u ovom izdanju. Kao dio ovoga, oblik imena virtualnog računala u klasteru promijenio se iz headnode\*, workernode\* i zookeepernode\* da biste hn\*, wn\*, a zk\* odnosno. Nije preporučljivo da biste preuzeli Izravni ovisnost oblika imena virtualnog računala, budući da je to mogu se promijeniti. Koristite "naziv glavnog računala -f" na lokalnom računalu ili Ambari API-ji za određivanje popis domaćini i mapiranje komponente domaćini. Možete pronaći dodatne informacije na [https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/hosts.md](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/hosts.md) i [https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/host-components.md](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/host-components.md). | Servis | HDInsight klastere na Linux | N/D |
| Promjene konfiguracije | Za klastere HDInsight 3.1 sljedećih konfiguracija sada omogućene: <ul><li>tez.yarn.ats.Enabled i yarn.log.server.url. Time se omogućuje poslužitelj za vremenske trake aplikacije i s poslužitelja zapisnika omogućiti da vam poslužiti out zapisnika.</li></ul>Za klastere 3,2 HDInsight sljedećih konfiguracija izmijenjene: <ul><li>mapreduce.fileoutputcommitter.Algorithm.Version postavljen je na 2. Omogućuje korištenje V2 verziju na FileOutputCommitter.</li></ul> | Servis | Sve | N/D |


## <a name="notes-for-09092015-release-of-hdinsight"></a>Napomene uz izdanje 09/09/2015 HDInsight

Brojevi punu verziju za klastere HDInsight implementiran s ovom izdanju:

* HDInsight 2.1.10.675.1768697 (HDP 1.3.12.0-01795 - nepromijenjena)
* HDInsight 3.0.6.675.1768697 (HDP 2.0.13.0-2117 - nepromijenjena)
* HDInsight 3.1.4.675.1768697 (HDP 2.1.15.0-2334 - nepromijenjena)
* HDInsight 3.2.6.675.1768697 (HDP 2.2.6.1-0012 - nepromijenjena)
* SDK 1.5.8

Ovo izdanje sadrži sljedeća ažuriranja.

| Naslov                                           | Opis                                          | Područje utjecati (na primjer, servis, komponente ili SDK) | Skupine vrstu (na primjer, Hadoop, HBase ili oluja) | JIRA (Ako je primjenjivo) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Ažurirane verzije HDInsight za sve klastere HDInsight | Uz to je izdanje su ažurirane verzije HDInsight | Servis    | Sve| N/D                  |

## <a name="notes-for-07312015-release-of-hdinsight"></a>Napomene uz izdanje 07/31/2015 HDInsight

Brojevi punu verziju za klastere HDInsight implementiran s ovom izdanju:

* HDInsight 2.1.10.640.1695824 (HDP 1.3.12.0-01795 - nepromijenjena)
* HDInsight 3.0.6.640.1695824 (HDP 2.0.13.0-2117 - nepromijenjena)
* HDInsight 3.1.4.640.1695824 (HDP 2.1.15.0-2334 - nepromijenjena)
* HDInsight 3.2.6.640.1695824 (HDP 2.2.6.1-0012 - nepromijenjena)
* SDK 1.5.8

Ovo izdanje sadrži sljedeća ažuriranja.

| Naslov                                           | Opis                                          | Područje utjecati (na primjer, servis, komponente ili SDK) | Skupine vrstu (na primjer, Hadoop, HBase ili oluja) | JIRA (Ako je primjenjivo) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Rješavanje Spark ponovno slike tijeka rada čvor klaster | Fiksna problema koji je uzrok Spark klaster čvorove vratiti nakon ponovnog slike | Servis    | Spark| N/D                  |


## <a name="notes-for-07312015-release-of-hdinsight"></a>Napomene uz izdanje 07/31/2015 HDInsight

Brojevi punu verziju za klastere HDInsight implementiran s ovom izdanju:

* HDInsight 2.1.10.635.1684502 (HDP 1.3.12.0-01795 - nepromijenjena)
* HDInsight 3.0.6.635.1684502 (HDP 2.0.13.0-2117 - nepromijenjena)
* HDInsight 3.1.4.635.1684502 (HDP 2.1.15.0-2334 - nepromijenjena)
* HDInsight 3.2.6.635.1684502 (HDP 2.2.6.1-0012 - nepromijenjena)
* SDK 1.5.8

Ovo izdanje sadrži sljedeća ažuriranja.

| Naslov                                           | Opis                                          | Područje utjecati (na primjer, servis, komponente ili SDK) | Skupine vrstu (na primjer, Hadoop, HBase ili oluja) | JIRA (Ako je primjenjivo) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Ažurirane verzije HDInsight za sve klastere HDInsight | Uz to je izdanje su ažurirane verzije HDInsight | Servis    | Sve| N/D                  |


## <a name="notes-for-07072015-release-of-hdinsight"></a>Napomene uz izdanje 07/07/2015 HDInsight

Brojevi punu verziju za klastere HDInsight implementiran s ovom izdanju:

* HDInsight 2.1.10.610.1630216 (HDP 1.3.12.0-01795 - nepromijenjena)
* HDInsight 3.0.6.610.1630216 (HDP 2.0.13.0-2117 - nepromijenjena)
* HDInsight 3.1.4.610.1630216 (HDP 2.1.15.0-2334 - nepromijenjena)
* HDInsight 3.2.4.610.1630216 (HDP 2.2.6.1-0012)
* SDK 1.5.8


Ovo izdanje sadrži sljedeća ažuriranja.

| Naslov                                           | Opis                                          | Područje utjecati (na primjer, servis, komponente ili SDK) | Skupine vrstu (na primjer, Hadoop, HBase ili oluja) | JIRA (Ako je primjenjivo) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Ažurirane verzije HDP za klastere HDInsight 3,2 | Uz to je izdanje HDInsight 3,2 uvodi HDP 2.2.6.1-0012 | Servis    | Sve                                                 | N/D                  |


## <a name="notes-for-06262015-release-of-hdinsight"></a>Napomene uz izdanje 26/06/2015 HDInsight

Brojevi punu verziju za klastere HDInsight implementiran s ovom izdanju:

* HDInsight 2.1.10.601.1610731 (HDP 1.3.12.0-01795 - nepromijenjena)
* HDInsight 3.0.6.601.1610731 (HDP 2.0.13.0-2117 - nepromijenjena)
* HDInsight 3.1.4.601.1610731 (HDP 2.1.15.0-2334 - nepromijenjena)
* HDInsight 3.2.4.601.1610731 (HDP 2.2.6.1-0011)
* SDK 1.5.8


Ovo izdanje sadrži sljedeća ažuriranja.

<table border="1">
<tr>
<th>Naslov</th>
<th>Opis</th>
<th>Područje utjecati (na primjer, servis, komponente ili SDK)</p></th>
<th>Skupine vrstu (na primjer, Hadoop, HBase ili oluja)</th>
<th>JIRA (Ako je primjenjivo)</th>
</tr>


<tr>
<td>Ažurirane verzije HDP za klastere 3,2 HDInsight</td>
<td>Uz to je izdanje HDInsight 3,2 uvodi HDP 2.2.6.1</td>
<td>Servis</td>
<td>Sve</td>
<td>N/D</td>
</tr>

</table>

## <a name="notes-for-06182015-release-of-hdinsight"></a>Napomene uz izdanje 18/06/2015 HDInsight

Brojevi punu verziju za klastere HDInsight implementiran s ovom izdanju:

* HDInsight 2.1.10.596.1601657 (HDP 1.3.12.0-01795 - nepromijenjena)
* HDInsight 3.0.6.596.1601657 (HDP 2.0.13.0-2117 - nepromijenjena)
* HDInsight 3.1.4.596.1601657 (HDP 2.1.15.0-2334)
* HDInsight 3.2.4.596.1601657 (HDP 2.2.6.1-0002)
* SDK 1.5.8


Ovo izdanje sadrži sljedeća ažuriranja.

<table border="1">
<tr>
<th>Naslov</th>
<th>Opis</th>
<th>Područje utjecati (na primjer, servis, komponente ili SDK)</p></th>
<th>Skupine vrstu (na primjer, Hadoop, HBase ili oluja)</th>
<th>JIRA (Ako je primjenjivo)</th>
</tr>


<tr>
<td>Otvoriti dodatne HTTPS priključke</td>
<td>Servis u oblaku sada otvara 5 priključke 8001 i 8005 na klasteru npr. pri https://<clustername>.azurehdinsight.net:8001/. Zahtjevi za URL-ove su autentičnost koristeći istu lozinku mehanizam za osnovnu provjeru autentičnosti kao priključak 443. Sljedeće priključke povezati isti priključak na aktivni headnode. Da biste službi osluškuju priključke headnode i rute do izvan skupine mogu se akcije skripte.</td>
<td>Servis u oblaku</td>
<td>Sve</td>
<td>N/D</td>
</tr>

<tr>
<td>Povremeni problem Miješanje MapReduce za HDInsight 3,2</td>
<td>Popravak za rijetko, Povremeni trkaći uvjeta u Miješanje smanjiti karte na velike klastere rezultira occassional zadatka pogreške. Dodatne informacije potražite u članku <a href="https://issues.apache.org/jira/browse/MAPREDUCE-6361" target="_blank">MAPREDUCE 6361</a> .</td>
<td>Osnovni Hadoop</td>
<td>Sve</td>
<td><a href="https://issues.apache.org/jira/browse/MAPREDUCE-6361" target="_blank">MAPREDUCE 6361</a></td>
</tr>

<tr>
<td>Premještanje na najnovije Azure Java SDK 2.2 HDInsight 3,2</td>
<td>Premješta najnoviju verziju programa Azure SDK za Java koristi upravljački program za WASB. Najnovije SDK ima nekoliko rješenja i napomene za isti dostupne su na https://github.com/Azure/azure-storage-java/blob/master/ChangeLog.txt.</td>
<td>Osnovni Hadoop</td>
<td>Sve</td>
<td><a href="https://issues.apache.org/jira/browse/HADOOP-11959" target="_blank">HADOOP 11959</a></td>
</tr>

<tr>
<td>Premještanje HDP 2.1.15 za klastere HDInsight 3.1</td>
<td>Napomene uz izdanje izdanje Hortonworks dostupne su <a href="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.15-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.15.html" target="_blank">u nastavku</a>.</td>
<td>HDP</td>
<td>Sve</td>
<td>N/D</td>
</tr>

</table>

## <a name="notes-for-06042015-release-of-hdinsight"></a>Napomene uz izdanje 06/04/2015 HDInsight

Brojevi punu verziju za klastere HDInsight implementiran s ovom izdanju:

* HDInsight 2.1.10.583.1575584 (HDP 1.3.12.0-01795 - nepromijenjena)
* HDInsight 3.0.6.583.1575584 (HDP 2.0.13.0-2117 - nepromijenjena)
* HDInsight 3.1.3.583.1575584 (HDP 2.1.12.1-0003 - nepromijenjena)
* HDInsight 3.2.4.583.1575584 (HDP 2.2.6.1-1)
* SDK 1.5.8


Ovo izdanje sadrži sljedeća ažuriranja.

<table border="1">
<tr>
<th>Naslov</th>
<th>Opis</th>
<th>Područje utjecati (na primjer, servis, komponente ili SDK)</p></th>
<th>Skupine vrstu (na primjer, Hadoop, HBase ili oluja)</th>
<th>JIRA (Ako je primjenjivo)</th>
</tr>


<tr>
<td>Ispravak za 502 pogreška neispravan pristupnik za klastere oluja</td>
<td>U ovom izdanju rješava pogrešku utjecaja predavanje posla API koje su uzrokovale na web-mjestu ne radi nakon ponovnog pokretanja.</td>
<td>Servis</td>
<td>Oluja</td>
<td>N/D</td>
</tr>

</table>

## <a name="notes-for-06012015-release-of-hdinsight"></a>Napomene uz izdanje 06/01/2015 HDInsight

Brojevi punu verziju za klastere HDInsight implementiran s ovom izdanju:

* HDInsight 2.1.10.577.1563827 (HDP 1.3.12.0-01795 - nepromijenjena)
* HDInsight 3.0.6.577.1563827 (HDP 2.0.13.0-2117 - nepromijenjena)
* HDInsight 3.1.3.577.1563827 (HDP 2.1.12.1-0003 - nepromijenjena))
* HDInsight 3.2.4.577.1563827 (HDP 2.2.6.0-2800 - nepromijenjena)
* SDK 1.5.8


Ovo izdanje sadrži sljedeća ažuriranja.

<table border="1">
<tr>
<th>Naslov</th>
<th>Opis</th>
<th>Područje utjecati (na primjer, servis, komponente ili SDK)</p></th>
<th>Skupine vrstu (na primjer, Hadoop, HBase ili oluja)</th>
<th>JIRA (Ako je primjenjivo)</th>
</tr>


<tr>
<td>Različite popravci programskih pogrešaka</td>
<td>U ovom izdanju rješava programskih pogrešaka vezanih uz klaster dodjele resursa.</td>
<td>Servis</td>
<td>Sve vrste klaster</td>
<td>N/D</td>
</tr>

</table>

## <a name="notes-for-05272015-release-of-hdinsight"></a>Napomene uz izdanje 27/05/2015 HDInsight

Brojevi punu verziju za klastere HDInsight implementiran s ovom izdanju:

* HDInsight 3.2.4.570.1554102 (HDP 2.2.6.0-2800)
* Druge verzije klaster i SDK nije implementirana kao dio ovo izdanje.


Ovo izdanje sadrži sljedeća ažuriranja.

<table border="1">
<tr>
<th>Naslov</th>
<th>Opis</th>
<th>Područje utjecati (na primjer, servis, komponente ili SDK)</p></th>
<th>Skupine vrstu (na primjer, Hadoop, HBase ili oluja)</th>
<th>JIRA (Ako je primjenjivo)</th>
</tr>


<tr>
<td>Ažuriranje HDP 2.2</td>
<td>U ovom izdanju programa HDInsight 3,2 sadrži HDP 2.2.6 i nekoliko važnih popravci programskih pogrešaka u unosi HDInsight. Napomene u potpunu dostupna je na <a href="http://dev.hortonworks.com.s3.amazonaws.com/HDPDocuments/HDP2/HDP-2.2.6/HDP_RelNotes_v226/index.html">HDP 2.2.6 pustite bilješke</a>.</td>
<td>HDP</td>
<td>Sve vrste klaster</td>
<td>N/D</td>
</tr>

<tr>
<td>Promjena zadanog Yarn spremnik memorije konfiguraciju</td>
<td>U ovom ažuriranju u zadanom dostupne memorije za YARN spremnika (yarn.nodemanager.resource.memory mb i yarn.scheduler.maximum dodijeljeni mb), pokrenute čvor upravitelj, povećan je na 5632MB. Prethodno to je smanjiti 4608MB, ali različite izvođenju zadataka na temelju, nove vrijednosti mora nude bolje pouzdanost i performanse za većinu zadataka, dakle je bolje zadano. Kao uobičajeni, ako ste na uključeno ključnih ovisnost tu konfiguraciju memorije, postavite ga izričito prilikom stvaranja klaster.</td>
<td>HDP</td>
<td>Sve vrste klaster</td>
<td>N/D</td>
</tr>

<tr>
<td>Zadani Config slične značajke za HBase, a zatim oluja klaster</td>
<td>To ažuriranje vraća Hbase i oluja klastere da biste upotrijebili iste vrijednosti YARN configs Hadoop klastere. To možete učiniti za slične značajke kroz sve vrste klaster.</td>
<td>HDP</td>
<td>HBase, oluja</td>
<td>N/D</td>
</tr>

</table>

## <a name="notes-for-05202015-release-of-hdinsight"></a>Napomene uz izdanje 20/05/2015 HDInsight

Brojevi punu verziju za klastere HDInsight implementiran s ovom izdanju:

* HDInsight 2.1.10.564.1542093 (HDP 1.3.12.0-01795 - nepromijenjena)
* HDInsight 3.0.6.564.1542093 (HDP 2.0.13.0-2117 - nepromijenjena)
* HDInsight 3.1.3.564.1542093 (HDP 2.1.12.1-0003)
* HDInsight 3.2.4.564.1542093 (HDP 2.2.4.6-2)
* SDK 1.5.8

Ovo izdanje sadrži sljedeća ažuriranja.

<table border="1">
<tr>
<th>Naslov</th>
<th>Opis</th>
<th>Područje utjecati (na primjer, servis, komponente ili SDK)</p></th>
<th>Skupine vrstu (na primjer, Hadoop, HBase ili oluja)</th>
<th>JIRA (Ako je primjenjivo)</th>
</tr>


<tr>
<td>Podrška za EventHub SCP.NET</td>
<td>Ažurirani klaster paketi za HDInsight oluja prenijeti nove značajke za SCP.NET. Sada će imati pristup novi API-ji u topologiji Sastavljač koje olakšavaju korištenje EventHubSpout ili Java Spouts. Morate ažurirati klijent SCP.NET SDK za rad s novi klastere kao što su ažurirani ugovora. Pojedinosti o nove API-ji, korištenje i ispustite bilješke (uključujući popravci programskih pogrešaka) pogledajte Readme uključeni u paketu nuget SCP.NET.</td>
<td>Dodavanje veze za VANJSKIH Tooling</td>
<td>Oluja klastere HDInsight 3,2</td>
<td>N/D</td>
</tr>

<tr>
<td>Ažuriranje JDBC upravljački program</td>
<td>Verzija programa SQL Server podržane u sqljdbc_4.1.5605.100 ažurirati upravljački program.</td>
<td>Metastore</td>
<td>Sve</td>
<td>N/D</td>
</tr>
</table>

## <a name="notes-for-04272015-release-of-hdinsight"></a>Napomene uz izdanje 27/04/2015 HDInsight

Brojevi punu verziju za klastere HDInsight implementiran s ovom izdanju:

* HDInsight 2.1.10.537.1486660 (HDP 1.3.12.0-01795 - nepromijenjena)
* HDInsight 3.0.6.537.1486660 (HDP 2.0.13.0-2117 - nepromijenjena)
* HDInsight 3.1.3.537.1486660 (HDP 2.1.12.0-2329 - nepromijenjena)
* HDInsight 3.2.3.537.1486660 (HDP 2.2.2.2-4)
* SDK 1.5.8

Ovo izdanje sadrži sljedeća ažuriranja.

<table border="1">
<tr>
<th>Naslov</th>
<th>Opis</th>
<th>Područje utjecati (na primjer, servis, komponente ili SDK)</p></th>
<th>Skupine vrstu (na primjer, Hadoop, HBase ili oluja)</th>
<th>JIRA (Ako je primjenjivo)</th>
</tr>


<tr>
<td>Rješavanje DLL ovisnost</td>
<td>Uklanja HDInsight zavisnost o jedinici Test Framework.</td>
<td>SDK</td>
<td>Hadoop</td>
<td>N/D</td>
</tr>

<tr>
<td>Otklanjanje problema trkaći uvjet</td>
<td>Klaster stvorite zahtjev sada Čekanje na STAVI zahtjev za prihvatiti prije ankete o statusu</td>
<td>SDK</td>
<td>Hadoop</td>
<td>N/D</td>
</tr>
</table>

## <a name="notes-for-04142015-release-of-hdinsight"></a>Napomene uz izdanje 04/14/2015 HDInsight

Brojevi punu verziju za klastere HDInsight implementiran s ovom izdanju:

* HDInsight 2.1.10.521.1453250 (HDP 1.3.12.0-01795 - nepromijenjena)
* HDInsight 3.0.6.521.1453250 (HDP 2.0.13.0-2117 - nepromijenjena)
* HDInsight 3.1.3.521.1453250 (HDP 2.1.12.0-2329 - nepromijenjena)
* HDInsight 3.2.3.525.1459730 (HDP 2.2.2.2-2)
* SDK 1.5.6

Ovo izdanje sadrži sljedeća ažuriranja.

<table border="1">
<tr>
<th>Naslov</th>
<th>Opis</th>
<th>Područje utjecati (na primjer, servis, komponente ili SDK)</p></th>
<th>Skupine vrstu (na primjer, Hadoop, HBase ili oluja)</th>
<th>JIRA (Ako je primjenjivo)</th>
</tr>


<tr>
<td>Tez popravci programskih pogrešaka</td>
<td>Rješenja za Apache TEZ 2214 i TEZ 1923 nalaze se u ovom izdanju 3,2 HDI. Te su izričito potrebne za određeni upiti grozd na Tez koje zahtijevaju da biste Miješanje veliku količinu podataka.
</td>
<td>HDP</td>
<td>Hadoop</td>
<td><a href="https://issues.apache.org/jira/browse/TEZ-2214">TEZ 2214</a></br><a href="https://issues.apache.org/jira/browse/TEZ-1923">TEZ 1923</a></td>
</tr>
</table>

## <a name="notes-for-04062015-release-of-hdinsight"></a>Napomene uz izdanje 06/04/2015 HDInsight

Brojevi punu verziju za klastere HDInsight implementiran s ovom izdanju:

* HDInsight 2.1.10.521.1453250 (HDP 1.3.12.0-01795 - nepromijenjena)
* HDInsight 3.0.6.521.1453250 (HDP 2.0.13.0-2117 - nepromijenjena)
* HDInsight 3.1.3.521.1453250 (HDP 2.1.12.0-2329 - nepromijenjena)
* HDInsight 3.2.3.521.1453250 (HDP 2.2.2.2-1)
* SDK 1.5.6

Ovo izdanje sadrži sljedeća ažuriranja.

<table border="1">
<tr>
<th>Naslov</th>
<th>Opis</th>
<th>Područje utjecati (na primjer, servis, komponente ili SDK)</p></th>
<th>Skupine vrstu (na primjer, Hadoop, HBase ili oluja)</th>
<th>JIRA (Ako je primjenjivo)</th>
</tr>


<tr>
<td>HDInsight .NET SDK 1.5.6</td>
<td>Ažuriranja da biste uklonili neki Interna klase za HDInsight na Linux.</td>
<td>SDK</td>
<td>Hadoop</td>
<td>N/D</td>
</tr>

<tr>
<td>Biblioteka Avro 1.5.6</td>
<td>Dodane <b>KnownTypeAttribute</b> načina <b>GetAllKnownTypes</b>. FIXED NullReferenceException kada je vrsta null GetAllKnownTypes načina.</td>
<td>SDK</td>
<td>Hadoop</td>
<td>N/D</td>
</tr>

<tr>
<td>Popravci programskih pogrešaka</td>
<td>Različite popravci programskih pogrešaka sa servisom</td>
<td>Servis</td>
<td>Sve</td>
<td>N/D</td>
</tr>

</table>
<br>

## <a name="notes-for-04012015-release-of-hdinsight"></a>Napomene uz izdanje 04/01/2015 HDInsight

Brojevi punu verziju za klastere HDInsight implementiran s ovom izdanju:

* HDInsight 2.1.10.513.1431705 (HDP 1.3.12.0-01795)
* HDInsight 3.0.6.513.1431705 (HDP 2.0.13.0-2117)
* HDInsight 3.1.3.513.1431705 (HDP 2.1.12.0-2329)
* HDInsight 3.2.3.513.1431705 (HDP 2.2.2.1-2600)
* SDK 1.5.5

Ovo izdanje sadrži sljedeća ažuriranja.

<table border="1">
<tr>
<th>Naslov</th>
<th>Opis</th>
<th>Područje utjecati (na primjer, servis, komponente ili SDK)</p></th>
<th>Skupine vrstu (na primjer, Hadoop, HBase ili oluja)</th>
<th>JIRA (Ako je primjenjivo)</th>
</tr>


<tr>
<td>Mogućnost omogućiti ili onemogućiti udaljene radne površine vjerodajnice na Windows klastere putem .NET SDK</td>
<td>Programski podrška za omogućivati ili onemogućivati RDP vjerodajnice na klastere sustava Windows.</td>
<td>SDK</td>
<td>Sve</td>
<td>N/D</td>
</tr>

<tr>
<td>Mogućnost da biste omogućili udaljene radne površine vjerodajnice na klastere dok su bile koji se tamo</td>
<td>Programski podrška za omogućivanje udaljene radne površine vjerodajnice, kao što je stvoren je klaster. Time se uklanja dva koraka za prvo dodjeljivanje klaster i omogućivanje udaljene radne površine.</td>
<td>SDK</td>
<td>Sve</td>
<td>N/D</td>
</tr>

<tr>
<td>Nadograđene Python za 2.7.8</td>
<td>Nadograđene Python na klastere HDInsight da biste Python 2.7.8, koja sadrži nekoliko važnih sigurnosnih ispravci HDInsight verzija 2.1, 3.0, 3.1 i 3,2</td>
<td>Servis</td>
<td>Sve</td>
<td>N/D</td>
</tr>

<tr>
<td>Promjena konfiguracije YARN</td>
<td>Promijenjeni YARN konfiguracije yarn.resourcemanager.max dovršiti – aplikacije do 1000 za sve vrste klaster za HDInsight verzije 3.1 i 3,2. Ta vrijednost kontrole samo na popisu dovršeni aplikacija u korisničkom Sučelju YARN. Da biste dobili informacije o aplikacijama koje su poslane prije no što na popisu aplikacija koje se prikazuju na korisničko Sučelje, možete izravno idite na poslužitelj za povijest.</td>
<td>YARN</td>
<td>Sve</td>
<td>N/D</td>
</tr>

<tr>
<td>Promjenu veličine čvorove u programa HBase klaster</td>
<td>HBase klastere sada omogućuju promjenu veličine čvorove (gore i dolje) za HDInsight verzije 3.1 i 3,2</td>
<td>Servis</td>
<td>HBase</td>
<td>N/D</td>
</tr>

<tr>
<td>Nadogradnja JDBC</td>
<td>Upravljački program za SQL JDBC će se nadograditi verzijom sqljdbc_4.0.2206.100 za HDInsight verziju 3,2. Ova verzija sadrži poboljšanja važne sigurnosti.</td>
<td>HDP</td>
<td>Sve</td>
<td>N/D</td>
</tr>

<tr>
<td>Ažuriranje konfiguracije JVM</td>
<td>Ažurirani JVM konfiguracije networkaddress.cache.ttl na 300 sekundi iz zadanu vrijednost od -1 za HDInsight verzije 3.1 i 3,2. Konfiguracija vrijednost kontrole predmemoriranja pravila za uspješan naziv pretraživanja iz servisa naziv. To ispravlja pogreške vezane uz i klastere HBase.</td>
<td>Servis</td>
<td>HBase</td>
<td>N/D</td>
</tr>

<tr>
<td>Nadogradnja na Azure pohranu SDK Java 2.0</td>
<td>Da biste koristili najnoviju verziju prostora za pohranu SDK Azure Java će se nadograditi HDInsight verziju 3,2. Sadrži nekoliko važnih popravci programskih pogrešaka iznad trenutnog 0.6.0 verziju.</td>
<td>HDP</td>
<td>Sve</td>
<td>N/D</td>
</tr>

<tr>
<td>Nadograđena Apache WASB izvornog koda</td>
<td>Verzija 3,2 sada koristi najnoviji kod za upravljački program datotečnom sustavu WASB iz Apache Hadoop HDInsight. Uz tu promjenu upravljački program za WASB sada se koriste kao zasebna posudu. Ovo je isključivo promjenu pakiranje i ne sadrži promjene ponašanja upravljački program za WASB. Naziv datoteke POSUDU je hadoop azure 2.6.0.jar.</td>
<td>HDP</td>
<td>Sve</td>
<td>N/D</td>
</tr>

<tr>
<td>Naziv datoteke ažuriranja u HDInsight 3,2 za posudu</td>
<td>To ažuriranje za HDInsight verziju 3,2 sadrži nekoliko popravci programskih pogrešaka i nekoliko Interna staklenke pakirat kao dio HDP nadograđena. Imajte na umu da su te datoteke POSUDU Interna HDP paket, a ne za izravno koristi aplikacije klijenta. Aplikacija treba pakiranje vlastite verziju na staklenke tako da je nadogradnja staklenke Interna HDP prekinuti aplikacije klijenta.</td>
<td>HDP</td>
<td>Sve</td>
<td>N/D</td>
</tr>

</table>
<br>

## <a name="notes-for-03032015-release-of-hdinsight"></a>Napomene uz izdanje HDInsight 03/03/2015.

Brojevi punu verziju za klastere HDInsight implementiran s ovom izdanju:

* HDInsight 2.1.10.488.1375841 (HDP 1.3.9.0-01351 - nepromijenjena)
* HDInsight 3.0.6.488.1375841 (HDP 2.0.9.0-2097 - nepromijenjena)
* HDInsight 3.1.3.488.1375841 (HDP 2.1.10.0-2290 - nepromijenjena)
* HDInsight 3.2.3.488.1375841 (HDP-2.2.10.0-2340 - nepromijenjena)
* SDK 1.5.0 (nepromijenjena)

Ovo izdanje sadrži sljedeće ažuriranje.

<table border="1">
<tr>
<th>Naslov</th>
<th>Opis</th>
<th>Područje utjecati (na primjer, servis, komponente ili SDK)</p></th>
<th>Skupine vrstu (na primjer, Hadoop, HBase ili oluja)</th>
<th>JIRA</th>
</tr>


<tr>
<td>Poboljšanja pouzdanosti</td>
<td>Ne možemo razdoblju popravci Omogući servis za promjenu veličine bolje uz Poboljšana učitavanja vezana uz stvaranje klaster.</td>
<td>Servis</td>
<td>Sve</td>
<td>N/D</td>
</tr>



</table>
<br>

## <a name="notes-for-02182015-release-of-hdinsight"></a>Napomene uz izdanje 18/02/2015 HDInsight

Brojevi punu verziju za klastere HDInsight implementiran s ovom izdanju:

* HDInsight 2.1.10.471.1342507 (HDP 1.3.9.0-01351 - nepromijenjena)
* HDInsight 3.0.6.471.1342507 (HDP 2.0.9.0-2097 - nepromijenjena)
* HDInsight 3.1.3.471.1342507 (HDP 2.1.10.0-2290 - nepromijenjena)
* HDInsight 3.2.3.471.1342507 (HDP-2.2.10.0-2340)
* SDK 1.5.0

Ovo izdanje sadrži sljedeća ažuriranja.

<table border="1">
<tr>
<th>Naslov</th>
<th>Opis</th>
<th>Područje utjecati (na primjer, servis, komponente ili SDK)</p></th>
<th>Skupine vrstu (na primjer, Hadoop, HBase ili oluja)</th>
<th>JIRA (Ako je primjenjivo)</th>
</tr>


<tr>
<td>HDInsight 3,2 klastere</td>
<td>Hadoop 2.6/HDP2.2 dostupan je sa klastere 3,2 HDInsight. Sadrži najvažnija ažuriranja za sve komponente Otvori izvor. Dodatne informacije potražite u članku što je novo u HDInsight i <a href ="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.2.0/HDP_2.2.0_Release_Notes_20141202_version/index.html" target="_blank">bilješke o izdanju HDP 2.2.0.0</a>.</td>
<td>Otvaranje izvora softver</td>
<td>Sve</td>
<td>N/D</td>
</tr>

<tr>
<td>HDinsight na Linux (pretpregled)</td>
<td>Klastere može uvesti sustavom Ubuntu Linux. Dodatne informacije potražite u članku početak rada s HDInsight na Linux.</td>
<td>Servis</td>
<td>Hadoop</td>
<td>N/D</td>
</tr>

<tr>
<td>Dostupnost oluja Općenito</td>
<td>Apache oluja klastere obično su dostupne. Dodatne informacije potražite u članku početak rada s oluja u HDInsight.</td>
<td>Servis</td>
<td>Oluja</td>
<td>N/D</td>
</tr>

<tr>
<td>Veličina virtualnog računala</td>
<td>Azure HDInsight dostupna je na dodatne virtualnog računala vrste i veličine. HDInsight mogu koristiti A2 A7 veličine ugrađeni glavnu svrhu; Čvorovi D niz koji značajka solid-state pogona (SSDs) i procesori 60 posto brže; i A8 i A9 veličine koji imaju InfiniBand podrška za brzo povezivanje s mrežom.</td>
<td>Servis</td>
<td>Sve</td>
<td>N/D</td>
</tr>

<tr>
<td>Promjena veličine klaster</td>
<td>Možete promijeniti broj čvorove podataka za izvodi HDInsight klaster bez potrebe da biste izbrisali ili ponovno stvoriti. Trenutno samo Hadoop upita i Apache oluja klaster vrste imaju ovu mogućnost, ali podrška za Apache HBase klaster vrsta je prerano da biste počeli pratiti. Dodatne informacije potražite u članku Upravljanje HDInsight klastere.</td>
<td>Servis</td>
<td>Hadoop, oluja</td>
<td>N/D</td>
</tr>

<tr>
<td>Visual Studio tooling</td>
<td>Osim dovršeno tooling za Apache oluja tooling za Apache vrste Hive u Visual Studio ažurirana je da biste uključili dovršavanje izjava, lokalni provjere valjanosti i Poboljšana podrška za ispravljanje pogrešaka. Dodatne informacije potražite u članku početak rada s alatima Hadoop HDInsight za Visual Studio.</td>
<td>Tooling</td>
<td>Hadoop</td>
<td>N/D</td>
</tr>

<tr>
<td>Hadoop poveznik za DocumentDB</td>
<td>S Hadoop poveznik za DocumentDB, možete izvršiti složene zbrajanja, analizu i operacije nad sheme manje JSON dokumente pohranjene u zbirkama DocumentDB ili u bazi podataka računa. Dodatne informacije i u vodič potražite u članku Hadoop pokretanje zadatke pomoću DocumentDB i HDInsight.</td>
<td>Servis</td>
<td>Hadoop</td>
<td>N/D</td>
</tr>

<tr>
<td>Popravci programskih pogrešaka</td>
<td>Unijeli smo različite manji popravci programskih pogrešaka za HDInsight servise. Nema promjene u ponašanju kupca dostupnog se očekuje.</td>
<td>Servis</td>
<td>Sve</td>
<td>N/D</td>
</tr>

</table>
<br>

## <a name="notes-for-02062015-release-of-hdinsight"></a>Napomene uz izdanje 06/02/2015 HDInsight

Brojevi punu verziju za klastere HDInsight implementiran s ovom izdanju:

* HDInsight 2.1.10.463.1325367 (HDP 1.3.9.0-01351 - nepromijenjena)
* HDInsight 3.0.6.463.1325367 (HDP 2.0.9.0-2097 - nepromijenjena)
* HDInsight 3.1.2.463.1325367 (HDP 2.1.10.0-2290)
* SDK N/D

Ovo izdanje sadrži sljedeća ažuriranja.

<table border="1">
<tr>
<th>Naslov</th>
<th>Opis</th>
<th>Područje utjecati (na primjer, servis, komponente ili SDK)</p></th>
<th>Skupine vrstu (na primjer, Hadoop, HBase ili oluja)</th>
<th>JIRA (Ako je primjenjivo)</th>
</tr>


<tr>
<td>Popravci programskih pogrešaka</td>
<td>Unijeli smo različite manji popravci programskih pogrešaka za HDInsight servise. Nema promjene u ponašanju kupca dostupnog se očekuje.</td>
<td>Servis</td>
<td>Sve</td>
<td>N/D</td>
</tr>

<tr>
<td>Ažuriranje HDP 2.1 održavanja</td>
<td>HDInsight 3.1 ažurira se za implementaciju HDP 2.1.10.0. Dodatne informacije potražite u članku <a href ="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.10/bk_releasenotes_hdp_2.1/content/ch_relnotes-HDP-2.1.10.html" target="_blank">HDP 2.1.10 napomene</a>. </td>
<td>Otvaranje izvora softver</td>
<td>Sve</td>
<td>N/D</td>
</tr>

<tr>
<td>Binarni ažuriranja HDP</td>
<td>Postoji nekoliko POSUDU datoteka u HBase za datoteka koje su ažurirane imena. Te se datoteke POSUDU koristi interno HBase, tako da se očekuje kupci uključeno ovisnosti imena te POSUDU datoteke. To obuhvaća:
<ul>
<li>./lib/jetty-6.1.26.hwx.jar</li>
<li>./lib/jetty-sslengine-6.1.26.hwx.jar</li>
<li>./lib/jetty-util-6.1.26.hwx.jar</li>
</ul>
</td>
<td>Otvaranje izvora softver</td>
<td>HBase</td>
<td>N/D</td>
</tr>

</table>
<br>

## <a name="notes-for-1292015-release-of-hdinsight"></a>Napomene uz izdanje 29/1/2015 HDInsight

Brojevi punu verziju za klastere HDInsight implementiran s ovom izdanju:

* HDInsight 2.1.10.455.1309616 (HDP 1.3.9.0-01351 - nepromijenjena)
* HDInsight 3.0.6.455.1309616 (HDP 2.0.9.0-2097 - nepromijenjena)
* HDInsight 3.1.2.455.1309616 (HDP 2.1.9.0-2196 - nepromijenjena)
* SDK N/D

Ovo izdanje sadrži sljedeće ažuriranje.

<table border="1">
<tr>
<th>Naslov</th>
<th>Opis</th>
<th>Područje utjecati (na primjer, servis, komponente ili SDK)</p></th>
<th>Skupine vrstu (na primjer, Hadoop, HBase ili oluja)</th>
<th>JIRA (Ako je primjenjivo)</th>
</tr>


<tr>

<td>Popravci programskih pogrešaka</td>
<td>Unijeli smo nekoliko važnih popravci programskih pogrešaka koje poboljšanje pouzdanosti klastere HDInsight tijekom nadogradnje Azure.</td>
<td>Servis</td>
<td>Sve</td>
<td>N/D</td>
</tr>



</table>
<br>

## <a name="notes-for-152015-release-of-hdinsight"></a>Napomene uz izdanje 5/1/2015 HDInsight

Brojevi punu verziju za klastere HDInsight implementiran s ovom izdanju:

* HDInsight 2.1.10.420.1246118 (HDP 1.3.9.0-01351 - nepromijenjena)
* HDInsight 3.0.6.420.1246118 (HDP 2.0.9.0-2097 - nepromijenjena)
* HDInsight 3.1.2.420.1246118 (HDP 2.1.9.0-2196 - nepromijenjena)


Ovo izdanje sadrži sljedeća ažuriranja.

<table border="1">
<tr>
<th>Naslov</th>
<th>Opis</th>
<th>Komponenta</th>
<th>Vrsta klaster</th>
<th>JIRA (Ako je primjenjivo)</th>
</tr>


<tr>
<td>Uzorci za analizu trend Twitter i sustavom Mahout film preporuke</td>
<td><p>U ovom izdanju konzole za upit HDInsight ima dva uzorka za dodatne:</p>

<p><b>Analiza Trend twitter</b><br>
Javni API-ji nudi web-mjesta kao što su Twitter su korisne izvora podataka za analizu i razumijevanje popularne trendova. U ovom ćete praktičnom vodiču Naučite koristiti grozd dobit ćete popis Twitter korisnici koji šalju Većina tweets koji sadrže određene riječi. </p>

<p><b>Preporuka mahout filma</b><br>
Apache Mahout je programa Apache Hadoop strojnog učenja biblioteke. Mahout sadrži algoritama za obradu podataka (primjerice filtriranje, klasifikaciju i Klasteriranje). U ovom ćete praktičnom vodiču koristiti modul za preporuke za generiranje film preporuke filmovima koji se pojavljuje vaši prijatelji na temelju.</p></td>
<td>Konzola za upit</td>
<td>Hadoop</td>
<td>N/D</td>
</tr>

<tr>
<td>Promjena zadane vrijednosti za konfiguraciju grozd: hive.auto.convert.join.noconditionaltask.size</td>
<td><p>Tu konfiguraciju veličina primjenjuje se automatski pretvoriti mape spojeve. Vrijednost predstavlja zbroj veličina tablice koje se mogu pretvoriti u raspršivanje karte koje odgovaraju u memoriji. U prethodnih izdanja, tu vrijednost veća od zadanu vrijednost od 10 MB do 128 MB. Međutim, novu vrijednost 128 MB zbog toga poslove koji će se neće dospijeća nemaju memorije. U ovom izdanju vraća zadanu vrijednost natrag od 10 MB. Korisnici i dalje možete nadjačati tu vrijednost tijekom stvaranja klaster dali svoje upite i veličine tablice. Dodatne informacije o tu postavku i kako ga zamijeniti potražite u članku <a href="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.0.2/ds_Hive/optimize-joins.html#JoinOptimization-OptimizeAutoJoinConversion" target="_blank">Optimiziranje automatsko pridruživanje Pretvorba</a> u dokumentaciji Hortonworks. </p></td>
<td>Grozd</td>
<td>Hadoop, Hbase</td>
<td>N/D</td>
</tr>

</table>
<br>

## <a name="notes-for-12232014-release-of-hdinsight"></a>Napomene uz izdanje HDInsight 12/23/2014.

U punu verziju za klastere HDInsight implementiran s ovom izdanju su brojevi:

* HDInsight 2.1.10.420.1246783 (nepromijenjena HDP verzija)
* HDInsight 3.0.6.420.1246783 (nepromijenjena HDP verzija)
* HDInsight 3.1.1.420.1246783 (nepromijenjena HDP verzija)

Ovo izdanje sadrži sljedeće ažuriranje.

<table border="1">
<tr>
<th>Naslov</th>
<th>Opis</th>
<th>Komponenta</th>
<th>Vrsta klaster</th>
<th>JIRA (Ako je primjenjivo)</th>
</tr>


<tr>
<td>Povremeni klaster stvaranja pogreške zbog viškom učitavanja</td>
<td><p>Poboljšani algoritam za preuzimanje paketa HDP tijekom stvaranja klaster omogućuje Robusniji obradu pogrešaka zbog viškom Učitaj.</p></td>
<td>Servis</td>
<td>Hadoop, Hbase, oluja</td>
<td>N/D</td>
</tr>



</table>
<br>

## <a name="notes-for-12182014-release-of-hdinsight"></a>Napomene uz izdanje HDInsight 12/18/2014.

Ovo izdanje sadrži sljedeće komponente ažuriranje.

<table border="1">
<tr>
<th>Naslov</th>
<th>Opis</th>
<th>Komponenta</th>
<th>Vrsta klaster</th>
<th>JIRA (Ako je primjenjivo)</th>
</tr>

<tr>
<td><a href = "hdinsight-hadoop-customize-cluster.md" target="_blank">Prilagodba klaster Avalability Općenito</a></td>
<td><p>Prilagodbe omogućuje vam da biste prilagodili svoje klastere Azure HDInsight s projektima dostupnih u zajednici Apache Hadoop. Nova značajka možete Poigrajte i implementacija Hadoop projektima sa servisom Azure HDInsight. Omogućena je putem značajke **Skripte akcije** koje možete izmijeniti Hadoop klastere proizvoljne načine pomoću prilagođene skripte. Ovu prilagodbu dostupna je na svim vrstama HDInsight klastere uključujući Hadoop, HBase i oluja. Da bismo pokazali power tu mogućnost, ne možemo ste navedenih postupak za instalaciju popularne <a href = "hdinsight-hadoop-spark-install.md" target="_blank">Spark</a>, <a href = "hdinsight-hadoop-r-scripts.md" target="_blank">R</a>, <a href = "hdinsight-hadoop-solr-install.md" target="_blank">Solr</a>i <a href = "hdinsight-hadoop-giraph-install.md" target="_blank">Giraph</a> module. U ovom izdanju također se dodaje mogućnost za korisnike da biste odredili svoje prilagođene skripte akcija putem portala za Azure sadrži upute i najbolje prakse upute za stvaranje prilagođene skripte akcija pomoću metode preglednika, a sadrži upute o testiranju akciju skripte. </p></td>
<td>Opće značajke dostupnosti</td>
<td>Sve</td>
<td>N/D</td>
</tr>


</table>
<br>

## <a name="notes-for-12052014-release-of-hdinsight"></a>Napomene uz izdanje HDInsight 12/05/2014.

U punu verziju za klastere HDInsight implementiran s ovom izdanju su brojevi:

* HDInsight 2.1.9.406.1221105 (HDP 1.3.9.0-01351)
* HDInsight 3.0.5.406.1221105 (HDP 2.0.9.0-2097)
* HDInsight 3.1.1.406.1221105 (HDP 2.1.9.0-2196)
* HDInsight SDK n/d

Ovo izdanje sadrži sljedeće komponente ažuriranja.

<table border="1">
<tr>
<th>Naslov</th>
<th>Opis</th>
<th>Komponenta</th>
<th>Vrsta klaster</th>
<th>JIRA (Ako je primjenjivo)</th>
</tr>

<tr>
<td>Otklanjanje problema: Povremeni pogreške pri dodavanju velikog broja particije u tablicu u DDL za vrste Hive </td>
<td><p>Ako je došlo do pogreške Povremeni vezu s bazom podataka metastore grozd prilikom dodavanja mnogo particije u tablicu vrste Hive, DDL vrste Hive uspijeva. Sljedeća naredba će se vidjeti u evidenciju pogrešaka grozd ako dođe do tog problema: </p><p>"Pogreška [glavne]: ql. Upravljački program (SessionState.java:printError(547)) - nije USPJELA: povrata kod 1 iz org.apache.hadoop.hive.ql.exec.DDLTask izvođenja pogreške. MetaException (message:java.lang.RuntimeException: commitTransaction je pozvan ali openTransactionCalls = 0. To vjerojatno označava da nema neizravnane poziva openTransaction/commitTransaction)"</p></td>
<td>Grozd</td>
<td>Hadoop, Hbase</td>
<td>GROZD 482 (to je interni JIRA, tako da ga ne može biti vanjsko navodnike. Primijetit ćete ovdje za referencu.)</td>
</tr>

<tr>
<td>Otklanjanje problema: Povremeni organizacije na konzoli za HDInsight upita</td>
<td>Kada se to dogodi, u zapisniku WebHCat za posao pokretač WebHCat mogu vidjeti sljedeću naredbu: <p>"org.apache.hive.hcatalog.templeton.CatchallExceptionMapper | org.apache.hadoop.ipc.RemoteException(org.apache.hadoop.yarn.exceptions.YarnRuntimeException): ne može se učitati povijest datoteke {wasb url za datoteku povijest} "</p></td>
<td>WebHCat</td>
<td>Hadoop</td>
<td>GROZD 482 (to je interni JIRA, tako da ga ne može biti vanjsko navodnike. Primijetit ćete ovdje za referencu.)</td>
</tr>

<tr>
<td>Otklanjanje problema: Povremeni šiljka u Latencija upita Hbase</td>
<td>Ako se to dogodi, korisnici će obratite pozornost na to Povremeni šiljka od tri sekunde u Latencija upita Hbase. </td>
<td>Pristupnik za HDInsight klaster</td>
<td>HBase</td>
<td>N/D</td>
</tr>

<tr>
<td>HDP POSUDU promjena naziva datoteke</td>
<td>Za HDI skupine verzije 3.0, tamo nekoliko promjena na datotekama POSUDU za interne instalirao HDP. jetty 6.1.26.jar je zamijenjena jetty 6.1.26.hwx.jar. jetty util 6.1.26.jar je zamijenjena jetty util 6.1.26.hwx.jar. Promjene primijeniti Hadoop, Mahout, WebHCat i Oozie projektima.</td>
<td>Hadoop, Mahout, WebHCat, Oozie</td>
<td>Hadoop, HBase</td>
<td>N/D</td>
</tr>

</table>
<br>


## <a name="notes-for-11212014-release-of-hdinsight"></a>Napomene uz izdanje HDInsight 11/21/2014.

U punu verziju za klastere HDInsight implementiran s ovom izdanju su brojevi:

* HDInsight 2.1.9.382.1169709 (isto kao 11/14/2014.)
* HDInsight 3.0.5.382.1169709 (isto kao 11/14/2014.)
* HDInsight 3.1.1.382.1169709 (isto kao 11/14/2014.)
* HDINsight SDK 1.4.0

Ovo izdanje sadrži sljedeće komponente ažuriranja.

<table border="1">
<tr>
<th>Naslov</th>
<th>Opis</th>
<th>Komponenta</th>
<th>Vrsta klaster</th>
<th>JIRA (Ako je primjenjivo)</th>
</tr>

<tr>
<td>Pristupanju zapisnika aplikacije</td>
<td>Mogućnost programski numerirati programima pokrenutima na vašem klastere i preuzeti odgovarajuću specifičnim aplikacijama ili spremnik specifične zapisnika radi ispravljanja pogrešaka problematična aplikacije.</td>
<td>SDK</td>
<td>Hadoop</td>
<td>N/D</td>
</tr>

<tr>
<td>Mogućnost određivanja naziv regije u IHdInsightClient.DeleteCluster </td>
<td>Azure HDInsight SDK-a nudi mogućnost da biste odredili naziv regije prilikom korištenja **DeleteCluster**. Omogućuje deblokiranje korisnike imala dva resursa s istim nazivom u različitim područjima i imao je nije moguće izbrisati bilo koji od njih.</td>
<td>SDK</td>
<td>Sve</td>
<td>N/D</td>
</tr>

<tr>
<td>ClusterDetails.DeploymentId</td>
<td>Objekt **ClusterDetails** vraća **DeploymentID** polje koje predstavlja jedinstveni identifikator za klaster. Uvijek je moraju biti jedinstvene preko pokušaja stvaranja klaster s istim nazivima.</td>
<td>SDK</td>
<td>Sve</td>
<td>N/D</td>
</tr>
</table>
<br>

## <a name="notes-for-11142014-release-of-hdinsight"></a>Napomene uz izdanje HDInsight 11/14/2014.

U punu verziju za klastere HDInsight implementiran s ovom izdanju su brojevi:

* HDInsight 2.1.9.382.1169709
* HDInsight 3.0.5.382.1169709
* HDInsight 3.1.1.382.1169709

Ovo izdanje sadrži sljedeće nove značajke, komponente ažuriranja i popravci programskih pogrešaka.

<table border="1">
<tr>
<th>Naslov</th>
<th>Opis</th>
<th>Komponenta</th>
<th>Vrsta klaster</th>
<th>JIRA (Ako je primjenjivo)</th>
</tr>

<tr>
<td>Skripta Akcije (pretpregled)</td>
<td>Pretpregled značajke prilagodbe klaster koja omogućuje promjenu Hadoop klaster proizvoljne načine pomoću prilagođene skripte. S tom značajkom korisnici mogu isprobati i implementacija projekata koji su dostupni u zajednici Apache Hadoop klastere Azure HDInsight. Značajka prilagodbe je dostupna na svim vrstama klastere HDInsight, uključujući Hadoop, HBase i oluja.</td>
<td>Nove značajke</td>
<td>Sve</td>
<td>N/D</td>
</tr>

<tr>
<td>Ugrađene zadatke za Azure web-mjesta i spremanje zapisnika analize</td>
<td>Konzola za upit HDInsight ima Galerija za početak rada koji podržava rješenja koji rade na podataka ili ogledne podatke.
<p>**Rješenja koji rade na podacima**:<br>
Ne možemo ste stvorili zadatke za neke od Najčešći scenariji analize podataka omogućuje početnu točku za stvaranje vlastitog rješenja. Podatke možete koristiti s posla da biste vidjeli kako funkcionira. Kada ste spremni koristiti ono što ste naučili da biste stvorili rješenje koje je katalog modeliran nakon ugrađene poslu.</p>
<p>**Rješenja koji rade na ogledne podatke**:<br>
Saznajte kako raditi s HDInsight tako prolaska kroz neke osnovne scenariji (kao što su analiza web zapisnika i podaci senzora). Ćete saznati kako koristiti HDInsight radi analize podataka u takvim i kako možete povezati drugih aplikacija i servisa te podatke. Vizualizacija podataka putem veze u programu Microsoft Excel nudi primjera power takvog.</p></td>
<td>Konzola za upit</td>
<td>Hadoop</td>
<td>N/D</td>
</tr>

<tr>
<td>Popravak osipanja memorije u Templeton</td>
<td>Curenje memorije u Templeton koji utjecati na korisnike imali dugo izvodi klastere ili su slanje 100s posla zahtjeve sadrži je adresirana sekunde. Problem manifested kao Templeton 5xx pogrešaka i rješenje je ponovno pokretanje servisa. Grafika više nije potrebna.</td>
<td>Templeton</td>
<td>Sve</td>
<td>https://Issues.Apache.org/jira/Browse/HADOOP-11248</td>
</tr>
</table>
<br>


**Napomena**: da bismo pokazali nove mogućnosti učinio dostupnima prilagodbe klaster, postupke pomoću akcije skriptu da biste instalirali module Spark i R na klaster zabilježeni su. Dodatne informacije potražite u članku:

* [Instaliranje i korištenje Spark 1.0 na klastere HDInsight](hdinsight-hadoop-spark-install.md)
* [Instaliranje i korištenje R na klastere HDInsight Hadoop](hdinsight-hadoop-r-scripts.md)




## <a name="notes-for-11072014-release-of-hdinsight"></a>Napomene uz izdanje HDInsight 11/07/2014.

U punu verziju za klastere HDInsight koji je implementiran u ovo izdanje su brojevi:

* HDInsight 2.1 2.1.9.374.1153876
* HDInsight 3.0 3.0.5.374.1153876
* HDInsight 3.1 3.1.1.374.1153876

Ovo izdanje sadrži sljedeće komponente ažuriranja.

<table border="1">
<tr>
<th>Naslov</th>
<th>Opis</th>
<th>Komponenta</th>
<th>Vrsta klaster</th>
<th>JIRA (Ako je primjenjivo)</th>
</tr>

<tr>
<td>HDP 2.1.7</td>
<td>U ovom izdanju temelji se na servis Hortonworks podataka platformu (HDP) 2.1.7. Dodatne informacije potražite u članku <a href="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html" target="_blank">Napomene za HDP 2.1.7</a>.</td>
<td>HDP</td>
<td>Sve</td>
<td>N/D</td>
</tr>

<tr>
<td>Poslužitelj YARN vremenske trake</td>
<td>Po zadanom je omogućena YARN poslužitelj za vremenske trake (poznat i kao generički aplikacije povijest poslužitelj). Poslužitelj za vremenske trake navedene generički informacije o dovršenim aplikacija (kao što je ID aplikacije, naziv aplikacije, stanja aplikacije, vrijeme slanja aplikacije i vrijeme završetka aplikacije).

Ove informacije aplikacije mogu biti dohvaćeni iz čvor glavni tako da pristupite URI http://headnodehost:8188 ili tako da pokrenete naredbu YARN: yarn aplikacije – popis – appStates sve.

Ove informacije možete također biti dohvaćeni daljinski iako REST API-JA na https://{ClusterDnsName}. azurehdinsight.NET/ws/v1/applicationhistory/.

Više informacija potražite u članku <a href="http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html" target="_blank">YARN vremenske trake poslužitelja</a>.</td>
<td>Servis, YARN</td>
<td>Hadoop, HBase</td>
<td>N/D</td>
</tr>

<tr>
<td>ID za uvođenje klaster</td>
<td>Počevši od verzije 1.3.3.1.5426.29232 SDK, korisnici mogu pristupati jedinstveni ID-a za svaki klaster koje izdaje HDInsight. To omogućuje klijentima da biste utvrdili jedinstveni pojavljivanja klastere kada DNS naziv koji se ponovno iskoristiti preko stvaranje i ispustite scenarijima.</td>
<td>SDK</td>
<td>Sve</td>
<td>N/D</td>
</tr>
</table>
<br>

**Napomena**: problema koji je spriječio broj verzije puno iz prikazuju na portalu ili se vraća u SDK ili komponente Windows PowerShell fiksiran u ovom izdanju.

## <a name="notes-for-10152014-release"></a>Napomene uz izdanje 10/15/2014.

U ovom izdanju hitni popravak fiksiran memorije Templeton koji utječe na podebljanom korisnici Templeton. U nekim slučajevima, korisnici koji intenzivnog exercised Templeton želite vidjeti pogreške manifested kao 500 kodovi pogreške jer zahtjeve nije dovoljno memorije za izvođenje. Zaobilazno rješenje za taj problem je ponovno pokretanje servisa Templeton. Taj se problem riješen.


## <a name="notes-for-1072014-release"></a>Napomene uz izdanje 10/7/2014.

* Prilikom korištenja krajnje Ambari "https://{clusterDns}.azurehdinsight.net/ambari/api/v1/clusters/{clusterDns}.azurehdinsight.net/services/{servicename}/components/{componentname}", polje *host_name* vraća na potpuno kvalificirani naziv domene (FQDN) čvora umjesto samo naziv glavnog računala. Ako, na primjer, umjesto vraćanja "**headnode0**" dobit FQDN "**headnode0. { .Net .azurehdinsight ClusterDNS}**". Ta promjena potreban je da biste olakšali scenariji u kojima se više vrsta klaster (kao što su HBase i Hadoop) može uvesti u jedan virtualne mreže. To se događa, na primjer, korištenje HBase kao pozadinske platformu za Hadoop.

* Ne možemo je navesti nove postavke memorije za zadani klaster HDInsight. Prethodni zadane postavke memorije jeste li odgovarajući način uzima smjernice za broj procesora jezgri uvodi. Nove postavke memorije mora sadržavati bolje zadanih vrijednosti (po Hortonworks preporuke). Da biste to promijenili, pogledajte SDK referentnu dokumentaciju o promjeni klaster konfiguracije. Nove postavke memorije koje se koriste zadane 4 procesora core (8 spremnik) HDInsight klaster su detaljni popis u tablici u nastavku. (Vrijednosti koristi prije no što ovo izdanje također se navode parenthetically.)

<table border="1">
<tr><th>Komponenta</th><th>Dodjela memorije</th></tr>
<tr><td> yarn.Scheduler.minimum dodijeljeni</td><td>768 MB (prethodno 512 MB)</td></tr>
<tr><td> yarn.Scheduler.Maximum dodijeljeni</td><td>6144 MB (nepromijenjena)</td></tr>
<tr><td>yarn.nodemanager.Resource.Memory</td><td>6144 MB (nepromijenjena)</td></tr>
<tr><td>mapreduce.Map.Memory</td><td>768 MB (prethodno 512 MB)</td></tr>
<tr><td>mapreduce.Map.Java.opts</td><td>odabere =-Xmx512m (prethodno - Xmx410m)</td></tr>
<tr><td>mapreduce.Reduce.Memory</td><td>1536 MB (prethodno 1024 MB)</td></tr>
<tr><td>mapreduce.Reduce.Java.opts</td><td>odabere =-Xmx1024m (prethodno - Xmx819m)</td></tr>
<tr><td>yarn.App.mapreduce.AM.Resource</td><td>768 MB (prethodno 1024 MB)</td></tr>
<tr><td>yarn.App.mapreduce.AM.Command</td><td>odabere =-Xmx512m (prethodno - Xmx819m)</td></tr>
<tr><td>mapreduce.Task.IO.Sort</td><td>256 MB (prethodno 200 MB)</td></tr>
<tr><td>tez.AM.Resource.Memory</td><td>1536 MB (nepromijenjena)</td></tr>

</table><br>

Dodatne informacije o postavkama konfiguracije memorije koristi YARN i MapReduce na platformi Hortonworks podataka koji se koristi HDInsight potražite u članku [Određivanje postavki konfiguriranje HDP memorije](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1-latest/bk_installing_manually_book/content/rpm-chap1-11.html). Hortonworks dana i alat za izračun postavke proper memorije.

Vezano uz Azure PowerShell i HDInsight SDK poruku o pogrešci: "*klaster nije konfiguriran za HTTP services access*":

* Ta se pogreška već poznat [problem kompatibilnosti](https://social.msdn.microsoft.com/Forums/azure/a7de016d-8de1-4385-b89e-d2e7a1a9d927/hdinsight-powershellsdk-error-cluster-is-not-configured-for-http-services-access?forum=hdinsight) koji se mogu pojaviti zbog razlika u verziju HDInsight SDK ili Azure PowerShell i verziju klaster. Klastere stvoreno 8/15 ili noviji imaju podršku za novu mogućnost dodjele resursa u virtualne mreže. No tu mogućnost nije ispravno tumači starijom verzijom HDInsight SDK ili Azure PowerShell. Rezultat je došlo u neki postupci za predavanje posla. Ako koristite HDInsight SDK API-ji ili Azure PowerShell cmdleti (**Korištenje AzureRmHDInsightCluster** ili **Pozovite AzureRmHDInsightHiveJob**) da biste poslali zadacima, te operacije možda neće funkcionirati uz poruku o pogrešci "*klaster <clustername> nije konfiguriran za HTTP services access*." Ili (ovisno o postupka), mogla bi vam se druge poruke o pogreškama, kao što su "*ne može povezati s klaster*".

* Problema s kompatibilnošću su razriješiti najnovije verzije HDInsight SDK i Azure PowerShell. Preporučujemo da ažuriranje HDInsight SDK verziju 1.3.1.6 ili noviji Azure PowerShell alata i verziju 0.8.8 ili noviji. Možete pristupiti najnovije SDK HDInsight iz [](http://nuget.codeplex.com/wikipage?title=Getting%20Started) i alati PowerShell Azure pri [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md).



## <a name="notes-for-9122014-release-of-hdinsight-31"></a>Napomene uz izdanje HDInsight 3.1 9/12/2014.

* U ovom izdanju temelji se na servis Hortonworks podataka platformu (HDP) 2.1.5. Popis programskih pogrešaka koje popravlja ovo izdanje, potražite u članku [Fiksno u ovom izdanju](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.5/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.5-fixed.html) stranice na web-mjestu Hortonworks.
* U mapi biblioteke Svinja datoteke "avro-mapred-1.7.4.jar" promijenjen "avro-mapred-1.7.4-hadoop2.jar." Sadržaj ove datoteke sadrži manji popravak problema koji je nerastavljajuće. Preporučuje se kupci provjerite Izravni ovisnosti na naziv datoteke POSUDU da biste izbjegli prijelome prilikom preimenuju se datoteke.


## <a name="notes-for-8212014-release"></a>Napomene uz izdanje 8/21/2014.

* Ne možemo dodajete sljedeću WebHCat konfiguraciju (GROZD-7155) koji se postavlja ograničenje memorije zadani posla kontroler Templeton 1 GB. (Prethodne zadana vrijednost je 512 MB).

     templeton.Mapper.Memory.MB (1024 (=)

    * Promjena adrese sljedeće pogreške koje određene vrste Hive upita imao naiđete na zbog ograničenja donjem memorije: "Spremnik radi izvan ograničenja fizičke memorije."
    * Da biste se vratili na staru zadane vrijednosti, možete postaviti tu vrijednost konfiguracije da 512 putem komponente PowerShell Azure vrijeme stvaranja klaster pomoću sljedeće naredbe:

        Dodavanje AzureRmHDInsightConfigValues-Core@{"templeton.mapper.memory.mb"="512";}


* Naziv glavnog računala uloge zookeeper promijenio u *zookeeper*. To utječe na naziv razlučivost unutar klaster, ali ne utječe na vanjskim REST API-ji. Ako imate komponente koje koriste naziv glavnog računala *zookeepernode* , morate ih ažurirati da koriste novi naziv. Nova imena za tri čvorove zookeeper su:
    * zookeeper0
    * zookeeper1
    * zookeeper2
* Ažurira se HBase verziju podršku matrice. Za proizvodnju za radnih opterećenja HBase podržana je samo HDInsight verzijom 3.1 (HBase verzija 0.98). Verzije 3.0 (koji je dostupni za pretpregled) nije podržan Premještanje naprijed.

## <a name="notes-about-clusters-created-prior-to-8152014"></a>Napomene o klastere stvorena prije 8/15/2014.

Poruka o pogrešci Azure PowerShell ili HDInsight SDK, "klaster <clustername> nije konfiguriran za HTTP services access" (ili druge pogreške poruka ovisno o postupka, kao što su: "Ne može povezati s klaster") može pojavljivati zbog verziju razlikuju Azure PowerShell ili HDInsight SDK i klaster. Klastere stvoreno 8/15 ili noviji imaju podršku za novu mogućnost dodjele resursa u virtualne mreže. Ova mogućnost nije pravilno protumačiti starijom verzijom Azure PowerShell ili SDK HDInsight zbog čega neuspjeha operacija predavanje posla. Ako koristite HDInsight SDK API-ji ili Azure PowerShell cmdleti (kao što su korištenje AzureRmHDInsightCluster ili pozovite AzureRmHDInsightHiveJob) da biste poslali zadacima, s jednim od poruke o pogreškama opisane možda neće uspjeti te operacije.

Problema s kompatibilnošću su razriješiti najnovije verzije HDInsight SDK i Azure PowerShell. Preporučujemo da ažuriranje HDInsight SDK verziju 1.3.1.6 ili noviji Azure PowerShell alata i verziju 0.8.8 ili noviji. Možete dobiti pristup najnovijim SDK HDInsight iz [NuGet][nuget-link]. Alati za Azure PowerShell mogu pristupati pomoću [Instalacijskog programa sustava Microsoft Web platformu][webpi-link].


## <a name="notes-for-7282014-release"></a>Napomene uz izdanje 7/28/2014.

* **HDInsight dostupne u nova područja**: ne možemo proširiti HDInsight zemljopisnim prisutnosti tri područja. Korisnici servisa HDInsight možete stvoriti klastere te područja:
    * Istočnoazijski
    * Sjeverna središnje SAD-a
    * Južna središnje SAD-a
* HDInsight verzija 1.6 (HDP 1.1 i Hadoop 1.0.3) i HDInsight verzija 2.1 (HDP1.3 i Hadoop 1.2) uklanjaju se s portala za Azure. Možete i dalje da biste stvorili Hadoop klastere za te verzije pomoću cmdleta ljuske PowerShell Azure, [Nova AzureRmHDInsightCluster](http://msdn.microsoft.com/library/dn593744.aspx) ili pomoću [HDInsight SDK](http://msdn.microsoft.com/library/azure/dn469975.aspx). Pogledajte na HDInsight komponente stranica [verzija](hdinsight-component-versioning.md) dodatne informacije.
* Promjene Hortonworks podataka platformu (HDP) u ovom izdanju:

<table border="1">
<tr><th>HDP</th><th>Promjena</th></tr>
<tr><td>HDP 1.3 / HDI 2.1</td><td>Bez promjena</td></tr>
<tr><td>HDP 2.0 / HDI 3.0</td><td>Bez promjena</td></tr>
<tr><td>HDP 2.1 / HDI 3.1</td><td>zookeeper: ["3.4.5.2.1.3.0-1948"] -> ["3.4.5.2.1.3.2-0002"]</td></tr>


</table><br>

## <a name="notes-for-6242014-release"></a>Napomene uz izdanje 6/24/2014.

Ovo izdanje sadrži poboljšanja servisa HDInsight:

* **Dostupnost HDP 2.1**: HDInsight 3.1 (koja sadrži HDP 2.1) je Općenito dostupan, a na zadanu verziju za klastere novi.
* **HBase – Azure portala poboljšanja**: ne možemo su dostupnost klastere HBase u pretpregledu. Možete stvoriti HBase klastere na portalu uz samo nekoliko klikova. 

U HBase, možete stvoriti različite u stvarnom vremenu radnih opterećenja na HDInsight, iz interaktivnih web-mjesta na kojima se koriste velikih skupova podataka servisima spremanje senzor i telemetrijskih podataka iz milijune krajnje točke. Sljedeći korak bio radi analize podataka u te radnih opterećenja sa zadacima Hadoop, a to je moguće u HDInsight putem Azure PowerShell i grozd klaster nadzornu ploču.

### <a name="apache-mahout-preinstalled-on-hdinsight-31"></a>Mahout Apache predinstaliran na HDInsight 3.1

 [Mahout](http://hortonworks.com/hadoop/mahout/) je instalirana na klastere HDInsight 3.1 Hadoop tako da pokrenete Mahout poslove bez potrebe za dodatne klaster konfiguracije. Ako, na primjer, možete udaljene u programa Hadoop klaster pomoću protokola udaljene radne površine (RDP), i bez dodatnih koraka možete pokrenite sljedeću naredbu Mahout svijeta pozdrav:

        mahout org.apache.mahout.classifier.df.tools.Describe -p /user/hdp/glass.data -f /user/hdp/glass.info -d I 9 N L  

        mahout org.apache.mahout.classifier.df.BreimanExample -d /user/hdp/glass.data -ds /user/hdp/glass.info -i 10 -t 100

Objašnjenje potpuniji ovog postupka potražite u dokumentaciji za [Breiman primjer](https://mahout.apache.org/users/classification/breiman-example.html) Apache Mahout web-mjesta.


### <a name="hive-queries-can-use-tez-in-hdinsight-31"></a>Grozd upite možete koristiti Tez u HDInsight 3.1

Grozd 0,13 dostupna je u HDInsight 3.1 pa se može pokrenuti upita pomoću Tez, možete leveraged za poboljšanja performansi znatno.
Tez nije Omogući po zadanom grozd upita. Da biste koristili, morate odabrati u. Tez možete omogućiti tako da pokrenete sljedeći isječak koda:

        set hive.execution.engine=tez;
        select sc_status, count(*), histogram_numeric(sc_bytes,5) from website_logs_orc_local group by sc_status;

Hortonworks je objavljen detaljne razrada svega grozd poboljšanja performansi za upit s Tez kao što je isporučena u standardni jednonitnih. Detalje potražite u članku [Benchmarking Apache 13 vrste Hive za Enterprise Hadoop](http://hortonworks.com/blog/benchmarking-apache-hive-13-enterprise-hadoop/).

Dodatne informacije o korištenju grozd s Tez potražite u članku [vrste Hive na Tez](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez).

###<a name="global-availability"></a>Globalni dostupnosti
Uz izdanje HDInsight na Hadoop 2.2, Microsoft omogućila HDInsight u sve glavne geographies kojem je dostupna Azure. Konkretno, podatkovnim centrima Europa i jugoistočne Azije Zapad ste je na mreži. Time se omogućuje klijentima da biste pronašli klastere u podatkovnom centru koji je Zatvori i potencijalno u zonu slične zahtjevima za usklađenosti.


###<a name="dos--donts-between-cluster-versions"></a>Zaduženja i onemogućivanje, između verzija klaster

**Oozie metastores koji se koristi s u klaster HDInsight 3.1 su nije kompatibilna s klastere HDInsight 2.1 i ne može se koristiti s ovom prethodne verzije**.

Prilagođeni Oozie metastore baze podataka uvode se sa sustava HDInsight 3.1 Klaster se ne mogu ponovno koristiti s programa klaster HDInsight 2.1. To vrijedi čak i ako na metastore potječu programa klaster HDInsight 2.1. Ovaj scenarij nije podržana jer shemi metastore dobiva nadograditi kada se koristi s klaster programa HDInsight 3.1 tako da više nije kompatibilan s metastore potrebnih klastere HDInsight 2.1. Bilo koji pokušaj da biste ponovno koristili programa Oozie metastore kojom s je klaster HDInsight 3.1 će prikazati klaster HDInsight 2.1 beskorisno.

**Oozie metastores nije moguće zajednički koristiti preko klastere.**

Oozie metastores su priložene određene klastere pa ih nije moguće zajednički koristiti preko klastere.

###<a name="breaking-changes"></a>Najnovije promjene

**Prefiks sintakse**: samo na "wasbs: / /" sintaksa podržana u HDInsight 3.1 i 3.0 klastere. U starijim "asv: / /" sintaksa je podržano u HDInsight 2.1 i 1,6 klastere, no nije podržan u HDInsight 3.1 ili 3.0 klastere. To znači da svih poslova šalje se HDInsight 3.1 ili 3.0 klaster koji se izričito koristite na "asv: / /" sintaksa neće uspjeti. Na "wasbs: / /" sintaksa se koristiti umjesto toga. Osim toga, zadacima koje se šalje na bilo kojem 3.1 HDInsight ili 3.0 klastere koje su stvorene pomoću postojeće metastore koji sadrže eksplicitne reference na resurse pomoću na "asv: / /" sintaksa neće uspjeti. Ove metastores potrebno je moguće ponovno stvoriti pomoću na "wasbs: / /" sintaksa resursima adresu.


**Priključke**: priključke servis za HDInsight koristi se promijenile. Brojevi koji su korišteni su unutar raspona efemerni priključak operacijskog sustava Windows. Priključci automatski se dodijeliti iz unaprijed definirane efemerni raspona za short-lived Internet protocol sustavom komunikaciju. Novi skup brojeva priključaka servisa dopušteni Hortonworks podataka platformu (HDP) su izvan raspona ovo da biste izbjegli nailazi na sukobe koji se može pojaviti priključke koristi servise glavni čvor. Nove brojeve priključaka treba prouzročiti prijelom promjene. Brojeva koji se koriste se na sljedeći način:

 **HDInsight 1.6 (HDP 1.1)**
<table border="1">
<tr><th>Ime</th><th>Vrijednost</th></tr>
<tr><td>DFS.HTTP.Address</td><td>namenodehost:30070</td></tr>
<tr><td>DFS.datanode.Address</td><td>0.0.0.0:30010</td></tr>
<tr><td>DFS.datanode.HTTP.Address</td><td>0.0.0.0:30075</td></tr>
<tr><td>DFS.datanode.IPC.Address</td><td>0.0.0.0:30020</td></tr>
<tr><td>DFS.secondary.HTTP.Address</td><td>0.0.0.0:30090</td></tr>
<tr><td>mapred.Job.Tracker.HTTP.Address</td><td>jobtrackerhost:30030</td></tr>
<tr><td>mapred.Task.Tracker.HTTP.Address</td><td>0.0.0.0:30060</td></tr>
<tr><td>mapreduce.History.Server.HTTP.Address</td><td>0.0.0.0:31111</td></tr>
<tr><td>templeton.port</td><td>30111</td></tr>
</table><br>

 **HDInsight 3.1 i 3.0 (HDP 2.1 i 2.0)**
<table border="1">
<tr><th>Ime</th><th>Vrijednost</th></tr>
<tr><td>DFS.namenode.HTTP adresa</td><td>namenodehost:30070</td></tr>
<tr><td>DFS.namenode.https adresa</td><td>headnodehost:30470</td></tr>
<tr><td>DFS.datanode.Address</td><td>0.0.0.0:30010</td></tr>
<tr><td>DFS.datanode.HTTP.Address</td><td>0.0.0.0:30075</td></tr>
<tr><td>DFS.datanode.IPC.Address</td><td>0.0.0.0:30020</td></tr>
<tr><td>DFS.namenode.secondary.HTTP adresa</td><td>0.0.0.0:30090</td></tr>
<tr><td>yarn.nodemanager.webapp.Address</td><td>0.0.0.0:30060</td></tr>
<tr><td>templeton.port</td><td>30111</td></tr>
</table><br>

###<a name="dependencies"></a>Zavisnosti

Sljedeće ovisnosti dodani u HDInsight 3.x (HDP2.x):

* guice servlet
* optiq core
* javax.inject
* Aktivacija
* jsr305
* geronimo-jaspic_1.0_spec
* SRP slf4j
* Java xmlbuilder
* ant
* alata za Kompiliranje Commons
* jdo API-ja
* Commons math3
* paranamer
* jaxb impl
* stringtemplate
* eigenbase xom
* Jersey servlet
* Commons-izvršavanja prve
* jaxb API-ja
* jetty sve poslužitelja
* janino
* xercesImpl
* optiq avatica
* jta
* eigenbase svojstva
* groovy sve
* hamcrest core
* pošta
* linq4j
* jpam
* Jersey klijenta
* aopalliance
* geronimo-annotation_1.0_spec
* ant pokretač
* Jersey guice
* XML API-ji
* stax API-ja
* asm commons
* asm stabla
* wadl
* geronimo-jta_1.1_spec
* guice
* leveldbjni sve
* brzine
* jettison
* snappy java
* jetty sve
* Commons dbcp

Sljedeće ovisnosti više ne postoje u HDInsight 3.x (HDP2.x):

* jdeb
* kfs
* sqlline
* ivy
* aspectjrt
* JSON
* Temeljni
* jdo2 API-ja
* avro mapred
* datanucleus enhancer
* jsp
* Commons api-zapisivanje
* Commons Matematika
* JavaEWAH
* aspectjtools
* javolution
* hdfsproxy
* hbase
* snappy

###<a name="version-changes"></a>Promjena verzije

Sljedeće verzije su izvršene između HDInsight 2.x (HDP1.x) i HDInsight 3.x (HDP2.x):

* metriku core: ["2.1.2"] -> ["3.0.0"]
* derbynet: ["10.4.2.0"] -> ["10.10.1.1"]
* datanucleus: ["rdbms-3.0.8'] -> [" rdbms-3.2.9']
* jasper compiler: ["5.5.12"] -> ["5.5.23"]
* log4j: ["1.2.15", "1.2.16"] -> ["1.2.16", "1.2.17"]
* derbyclient: ["10.4.2.0"] -> ["10.10.1.1"]
* httpcore: ["4.2.4"] -> ["4.2.5"]
* hsqldb: ["1.8.0.10"] -> ["2.0.0"]
* jets3t: ["0.6.1"] -> ["0.9.0"]
* protobuf java: ["2.4.1"] -> ["2.5.0"]
* derby: ["10.4.2.0"] -> ["10.10.1.1"]
* jasper: ["runtime-5.5.12'] -> [" runtime-5.5.23']
* Commons daemon: ["1.0.1"] -> ["1.0.13"]
* datanucleus core: ["3.0.9"] -> ["3.2.10"]
* datanucleus api jdo: ["3.0.7"] -> ["3.2.6"]
* zookeeper: ["3.4.5.1.3.9.0-01320"] -> ["3.4.5.2.1.3.0-1948"]
* bonecp: ["0.7.1.RELEASE"] -> ["
* 0.8.0.RELEASE "]


### <a name="drivers"></a>Upravljačke programe
Connnectivity Java baze podataka (JDBC) upravljački program za SQL Server HDInsight koristi interno i ne koriste za vanjski operacije. Ako se želite povezati sa servisom HDInsight pomoću Povezivost otvorene baze podataka (ODBC), koristite Microsoft vrste Hive ODBC upravljački program. Dodatne informacije potražite u članku [Povezivanje programa Excel sa servisom HDInsight pomoću Microsoft vrste Hive ODBC upravljački program](hdinsight-connect-excel-hive-odbc-driver.md).


### <a name="bug-fixes"></a>Popravci programskih pogrešaka

Uz to je izdanje smo osvježene sljedeće verzije HDInsight s nekoliko popravci programskih pogrešaka:

* HDInsight 2.1 (HDP 1.3)
* HDInsight 3.0 (HDP 2.0)
* HDInsight 3.1 (HDP 2.1)


## <a name="hortonworks-release-notes"></a>Napomene o Hortonworks

Napomene za Hortonworks podataka platforme (HDPs) koje se koriste za klastere HDInsight verzije dostupne su na sljedećim mjestima:

* HDInsight verzijom 3.1 koristi Hadoop raspodjelu koji se temelji na [Platformi podataka Hortonworks 2.1.7][hdp-2-1-7]. Ovo je klaster Hadoop zadani stvorena prilikom pomoću portala za Azure nakon 11/7/2014.. HDInsight 3.1 klastere stvorenih prije 11/7/2014 temelje se na [Platformi podataka Hortonworks 2.1.1][hdp-2-1-1]

* HDInsight verzije 3.0 koristi Hadoop raspodjelu koji se temelji na [Hortonworks podataka platformu 2.0][hdp-2-0-8].

* HDInsight verzija 2.1 koristi Hadoop raspodjelu koji se temelji na [Hortonworks podataka platformu 1.3][hdp-1-3-0].

* HDInsight verzija 1.6 koristi Hadoop raspodjelu koji se temelji na [Hortonworks podataka platformu 1.1][hdp-1-1-0].

[hdp-2-1-7]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html

[hdp-2-1-1]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.1/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.1.html

[hdp-2-0-8]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.8.0/bk_releasenotes_hdp_2.0/content/ch_relnotes-hdp2.0.8.0.html

[hdp-1-3-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.0/bk_releasenotes_hdp_1.x/content/ch_relnotes-hdp1.3.0_1.html

[hdp-1-1-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-Win-1.1/bk_releasenotes_HDP-Win/content/ch_relnotes-hdp-win-1.1.0_1.html

[nuget-link]: https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.HDInsight/

[webpi-link]: http://go.microsoft.com/?linkid=9811175&clcid=0x409

[hdinsight-install-spark]: ../hdinsight-hadoop-spark-install/
[hdinsight-r-scripts]: ../hdinsight-hadoop-r-scripts/
 
