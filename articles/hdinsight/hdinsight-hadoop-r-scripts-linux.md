<properties
    pageTitle="Instalacija R na sustavom Linux HDInsight | Microsoft Azure"
    description="Saznajte kako instalirati i koristiti R da biste prilagodili klastere Hadoop sustavom Linux."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="larryfr"/>

# <a name="install-and-use-r-on-hdinsight-hadoop-clusters"></a>Instaliranje i korištenje R na klastere HDInsight Hadoop

R možete instalirati na bilo koju vrstu klaster u Hadoop na HDInsight pomoću **Skripte akcija** klaster prilagodbe. Time se omogućuje fizičari podataka i analitičar analitičar podataka R da biste implementirali koristite Napredna MapReduce/YARN programiranje framework za obradu velikih količina podataka na Hadoop klastere koje su uvedene u HDInsight.

> [AZURE.IMPORTANT] [Premium sloju](https://azure.microsoft.com/pricing/details/hdinsight/) koja nudi za HDInsight obuhvaća R poslužitelju kao dio svoj klaster HDInsight. Time se omogućuje skripte R da biste koristili MapReduce i Spark da biste pokrenuli raspodijeljeno computations. Dodatne informacije potražite u članku [Prvi koraci pri korištenju R Normalni prikaz na HDInsight](hdinsight-hadoop-r-server-get-started.md). 


## <a name="what-is-r"></a>Što je R?

<a href="http://www.r-project.org/" target="_blank">R projekta za statističke računalstvo</a> je Otvori izvor jezik i okruženje za računalstvo Statistika. R nudi stotine ugrađenih statističke funkcije i vlastitu programski jezik koji kombinira aspekte funkcionirati i vezanima uz objekt programiranje. Također nudi proširenom grafička mogućnosti. R je Preferirani okruženje za programiranje za većinu profesionalan statisticians i fizičari u razna polja.

R skripte mogu se izvoditi na klastere Hadoop u HDInsight koji su prilagođeni pomoću skripte akcije stvaranja da biste instalirali R okruženju. R je kompatibilan s Azure Blob prostora za pohranu (WASB) tako da možete podataka koji je pohranjen na dva moguća pomoću R HDInsight.

## <a name="what-the-script-does"></a>Funkcija skripta

Akcija skripte koristili za instaliranje R na svoj klaster HDInsight instalira sljedeće Ubuntu paketa, čime se omogućuje osnovnu instalaciju R:

* [r osnovni](http://packages.ubuntu.com/precise/r-base): paket osnovni GNU R
* [r osnovni razvojni](http://packages.ubuntu.com/precise/r-base-dev): Auxilliary GNU R paketa

Sljedeće RHadoop paketi i instalirate, nude Integracija s MapReduce i HDFS:

* [rmr2](https://github.com/RevolutionAnalytics/rmr2): razvojnim programerima R da biste koristili Hadoop MapReduce
* [rhdfs](https://github.com/RevolutionAnalytics/rhdfs): razvojnim programerima R da biste koristili Hadoop HDFS (WASB za HDInsight)

Uz to, sljedeće R paketa instaliranih je:

| R paketa | Što omogućuje |
| --------- | ---------------- |
| [rJava](https://cran.r-project.org/web/packages/rJava/index.html) | Nisku razinu R Java sučelja. |
| [Rcpp](https://cran.r-project.org/web/packages/Rcpp/index.html) | Integracija R i C++. |
| [RJSONIO](https://cran.r-project.org/web/packages/RJSONIO/index.html) | Serijalizirati/ukloniti serijski broj objekata R JSON |
| [bitops](https://cran.r-project.org/web/packages/bitops/index.html) | Funkcije za operacije na razini bita na vektori cijeli broj. |
| [sažetka](https://cran.r-project.org/web/packages/digest/index.html) | Stvaranje Digests šifriranja raspršivanje R objekata. |
| [funkcionalno](https://cran.r-project.org/web/packages/functional/index.html) | Curry, sastavljanje i druge funkcije veće narudžbe |
| [reshape2](https://cran.r-project.org/web/packages/reshape2/index.html) | Flexibly Preustroj i prikupljati podatke. |
| [stringr](https://cran.r-project.org/web/packages/stringr/index.html) | Jednostavan, dosljedan Wrappers za uobičajene postupaka niza. |
| [plyr](https://cran.r-project.org/web/packages/plyr/index.html) | Alati za razdvajanje, zatvaranje i kombiniranje podataka. |
| [caTools](https://cran.r-project.org/web/packages/caTools/index.html) | Alati za premještanje prozora Statistika GIF, Base64, ROC AUC itd. |
| [stringdist](https://cran.r-project.org/web/packages/stringdist/index.html) | Određivanje niz koji se podudaraju, a zatim niz udaljenost funkcije. |

## <a name="install-r-using-script-actions"></a>Instalacija R pomoću akcije skripte

Sljedeće skripte akciju koristi se za instaliranje R na programa klaster HDInsight. https://hdiconfigactions.blob.Core.Windows.NET/linuxrconfigactionv01/r-Installer-v01.SH
    
Ovo poglavlje sadrži upute o tome kako koristiti skriptu prilikom stvaranja novog klaster pomoću portala za Azure. 

> [AZURE.NOTE] Azure PowerShell, EŽA Azure, HDInsight .NET SDK ili Voditelj resursa Azure predložaka također se poslužite da biste primijenili akcije skripte. Akcije skripte možete primijeniti i na već pokrenut klastere. Dodatne informacije potražite u članku [Prilagodba HDInsight klastere s akcijama skripte](hdinsight-hadoop-customize-cluster-linux.md).

1. Pokretanje dodjeljivanje klaster pomoću koraka u [klastere HDInsight sustavom Linux dodjele resursa](hdinsight-hadoop-provision-linux-clusters.md#portal), ali dovršiti dodjele resursa.

2. Na plohu **Neobavezno konfiguracije** odaberite **Akcije skripte**i unijeti podatke u nastavku:

    * __Naziv__: Upišite neslužbeni naziv za skripte akciju.
    * __SKRIPTA URI__: https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh
    * __LAKŠI__: Odaberite ovu mogućnost
    * __TEMPIRANJA__: Odaberite ovu mogućnost
    * __ZOOKEEPER__: Odaberite ovu mogućnost da biste instalirali na čvor Zookeeper.
    * __Parametri__: to polje ostavite prazno

3. Pri dnu **Akcije skripte**, pomoću gumba **Odaberite** spremanje konfiguracije. Na kraju, pomoću gumba **Odaberite** pri dnu plohu **Neobavezno konfiguracije** da biste spremili informacije o konfiguraciji nije obavezno.

4. Nastavite dodjele resursa klaster, kao što je opisano u [klastere sustavom Linux dodjele resursa HDInsight](hdinsight-hadoop-provision-linux-clusters.md#portal).

## <a name="run-r-scripts"></a>Pokretanje skripti R

Nakon klaster Završi dodjele resursa, poduzmite sljedeće korake da biste koristili R za izvođenje operacije MapReduce na klaster.

1. Povezivanje s klaster HDInsight pomoću SSH:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    Dodatne informacije o korištenju SSH sa servisa HDInsight potražite u sljedećim člancima:

    * [Korištenje SSH sa sustavom Linux Hadoop na HDInsight Linux, Unix ili OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [Korištenje SSH sa sustavom Linux Hadoop na HDInsight iz sustava Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

2. Iz na `username@hn0-CLUSTERNAME:~$` upit, unesite sljedeću naredbu da biste započeli sesiju interaktivne R:

        R

3. Unesite sljedeći program R. Time se generira brojevi od 1 do 100 i potom množi s 2.

        library(rmr2)
        ints = to.dfs(1:100)
        calc = mapreduce(input = ints, map = function(k, v) cbind(v, 2*v))

    U prvom retku poziva biblioteke rmr2 RHadoop koji se koristi za MapReduce operacije.

    Drugi redak generira vrijednosti 1-100, a zatim sprema ih pomoću sustava Hadoop datoteku `to.dfs`.

    U trećem retku stvara MapReduce procesa pomoću funkcije nudi rmr2 i počinje obrada. Trebali biste vidjeti nekoliko redaka pomaknite prethodnih obrada započinje.

4. Da biste vidjeli privremeni put izlaz MapReduce spremljenom na sljedeće, koristite sljedeće:

        print(calc())

    Trebali biste nešto slično kao `/tmp/file5f615d870ad2`. Da biste pogledali stvarni Izlaz, koristite sljedeće:

        print(from.dfs(calc))

    Izlaz trebao bi izgledati ovako:

        [1,]  1 2
        [2,]  2 4
        .
        .
        .
        [98,]  98 196
        [99,]  99 198
        [100,] 100 200

5. Da biste izlaz R, unesite sljedeće:

        q()


## <a name="next-steps"></a>Daljnji koraci

- [Instalacija i korištenje klaster nijanse na HDInsight](hdinsight-hadoop-hue-linux.md). Nijanse je korisničkog Sučelja koja olakšava stvaranje, pokrenuti i spremanje Svinja i grozd zadataka, kao i pregled zadani prostor za pohranu za vaše HDInsight skupine web-mjesto.

- [Instalacija Giraph na klastere HDInsight](hdinsight-hadoop-giraph-install.md). Da biste instalirali Giraph na HDInsight Hadoop klastere pomoću klaster prilagodbe. Giraph omogućuje vam da biste izvršili obrada grafikonu pomoću Hadoop i može se koristiti sa servisom Azure HDInsight.

- [Instalacija Solr na klastere HDInsight](hdinsight-hadoop-solr-install.md). Da biste instalirali Solr na HDInsight Hadoop klastere pomoću klaster prilagodbe. Solr omogućuje izvođenje operacija napredna pretraživanja na pohranjene podatke.

- [Instalacija nijanse na klastere HDInsight](hdinsight-hadoop-hue-linux.md). Instalirajte nijanse na klastere HDInsight Hadoop pomoću klaster prilagodbe. Nijanse je skup web-aplikacije koje se koriste za interakciju s Hadoop klaster.

[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
