<properties
   pageTitle="Korištenje komponente Python u topologiji oluja na HDinsight | Microsoft Azure"
   description="Saznajte kako pomoću komponente Python s s Apache oluja na Azure HDInsight. Ćete saznati kako koristiti Python komponente iz obje Java temelji, a Clojure temelji oluja topologije."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="python"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="larryfr"/>

#<a name="develop-apache-storm-topologies-using-python-on-hdinsight"></a>Razvoj Apache oluja topologija pomoću Python na HDInsight

Apache oluja podržava više jezika, čak i omogućujući vam da biste spojili komponente s više jezika u jedan topologiji. U ovom dokumentu će Saznajte kako koristiti Python komponente u Java i sustavom Clojure oluja topologija na HDInsight.

##<a name="prerequisites"></a>Preduvjeti

* Python 2.7 ili noviji

* Java JDK 1.7 ili noviji

* [Leiningen](http://leiningen.org/)

##<a name="storm-multi-language-support"></a>Oluja višejezična podrška

Oluja je osmišljeno komponente koji se pišu pomoću programski jezik, no to zahtijeva da komponente znati kako raditi s [Thrift definiciju oluja](https://github.com/apache/storm/blob/master/storm-core/src/storm.thrift). Za Python, modulu prikazuje se kao dio projekta Apache oluja koja omogućuje jednostavno sučelja s oluja. Ovaj modul možete pronaći na [https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py](https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py).

Budući da Apache oluja je Java procesa koji se pokreće na Java virtualni stroj (JVM), komponente na drugim jezicima se provode kao subprocesses. Bitova oluja izvodi u na JVM komunicira s ove subprocesses korištenje JSON poruka koji se šalju putem stdin/stdout. Dodatne informacije o komunikaciji između komponenti pronaći ćete u dokumentaciji [višejezični protokol](https://storm.apache.org/documentation/Multilang-protocol.html) .

###<a name="the-storm-module"></a>Modul za oluja

Modul oluja (https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py) nudi bitova potrebne za stvaranje Python komponente koje funkcioniraju s oluja.

Time se omogućuje zadatke kao što su `storm.emit` slanje redni, i `storm.logInfo` pisati zapisnike. Li biste potaknuli čitanje tijekom tu datoteku i razumijevanje što omogućuje.

##<a name="challenges"></a>Izazove

Pomoću modula __storm.py__ , možete stvoriti spouts Python koje podatke i Vijci koji obrada podataka, no cjelokupan oluja topologije definiciju koji žice gore komunikaciju između komponente i dalje napisan pomoću Java ili Clojure. Osim toga, ako koristite Java, morate stvoriti i Java komponente koje funkcioniraju kao sučelje komponente Python.

Osim toga, jer oluja klastere pokrenuli raspodijeljeno način, koje morate module potrebnih Python komponente provjerite jesu li dostupni na sve čvorove suradnika u klasteru. Oluja ne nudi sve jednostavan je način za to postigli za resurse višejezični – ili ste da biste uvrstili sve ovisnosti kao dio datoteke posudu za topologije ili ručno instalirati ovisnosti na svakom tempiranja čvor u klasteru.

###<a name="java-vs-clojure-topology-definition"></a>Definicija topologije Java nasuprot Clojure

Od dvije metode definiranje topologije Clojure je na najjednostavniji/cleanest kao izravno referenc python komponente u definiciji topologije. Za definicije utemeljena na topologije morate definirati i Java komponente koje rukovati stvari kao deklariranje polja u se redni vratio Python komponente.

U ovom dokumentu, zajedno s projektima primjer opisane su obje metode.

##<a name="python-components-with-a-java-topology"></a>Komponente Python pomoću topologije Java

> [AZURE.NOTE] U ovom se primjeru dostupna je na [https://github.com/Azure-Samples/hdinsight-python-storm-wordcount](https://github.com/Azure-Samples/hdinsight-python-storm-wordcount) u direktoriju __JavaTopology__ . Ovo je Maven temelje projekta. Ako niste upoznati s Maven, potražite u članku [razviti utemeljena na topologija s Apache oluja na HDInsight](hdinsight-storm-develop-java-topology.md) dodatne informacije o stvaranju Maven projekta radi oluja topologije.

Utemeljena na topologije koja koristi Python (ili drugih jezika komponenti JVM) na početku pojavit će se koristiti komponente Java; No ako vam se prikazivati u svakoj od Java spouts/Vijci, vidjet ćete kod sličnu ovoj:

    public SplitBolt() {
        super("python", "countbolt.py");
    }

To su gdje Java poziva Python i izvodi skriptu koja sadrži logiku stvarni munje. Na Java spouts/Vijci (za u ovom primjeru) jednostavno deklarirati polja u koja će čuje podlozi komponenta Python n-torke.

Stvarni Python datoteke spremaju se u na `/multilang/resources` imeničkog u ovom primjeru. Na `/multilang` direktorija su navedeni u __pom.xml__:

<resources>
    <resource>
        <!-- Where the Python bits are kept -->
        <directory>${basedir} / multilang</directory>
    </resource>
</resources>

To uključuje sve datoteke u na `/multilang` mapa u posudu koja će ugrađena iz ovog projekta.

> [AZURE.IMPORTANT] Imajte na umu da samo određuje na `/multilang` direktorija i ne `/multilang/resources`. Oluja očekuje koje nisu JVM resursa u na `resources` direktorija, tako da ga je traže interno već. Stavljanje komponente u ovu mapu omogućuje samo referencu prema nazivu u kodu programskog jezika Java. Na primjer, `super("python", "countbolt.py");`. Drugi način mislite da ga je vidi oluja na `resources` imeničkog kao korijenska (/) prilikom pristupanja višejezični resursi.
>
> Za ovaj primjer projekt na `storm.py` modul obuhvaćeno na `/multilang/resources` direktorija.

###<a name="build-and-run-the-project"></a>Stvaranje i pokretanje projekta

Da biste pokrenuli taj projekt lokalno, jednostavno koristite sljedeću naredbu Maven omogućuje stvaranje i pokretanje u načinu rada za lokalni:

    mvn compile exec:java -Dstorm.topology=com.microsoft.example.WordCount

Uklanjanje postupak pomoću prečaca ctrl + c.

Da biste implementirali projekta programa HDInsight klaster izvodi Apache oluja, poduzmite sljedeće korake:

1. Sastavljanje programa posudu uber:

        mvn package

    To će stvoriti datoteku pod nazivom __WordCount – 1.0 SNAPSHOT.jar__ u na `/target` imeničkog za taj projekt.

2. Prijenos datoteke posudu klaster Hadoop na jedan od sljedećih načina:

    * Za __sustavom Linux__ klastere HDInsight: korištenje `scp WordCount-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:WordCount-1.0-SNAPSHOT.jar` da biste kopirali datoteku posudu klaster, zamjenjuje korisničko ime SSH korisničko ime i CLUSTERNAME pod nazivom klaster HDInsight.

        Kada datoteku dovršetka prijenosa, povežite se s klaster pomoću SSH i početak korištenja topologije`storm jar WordCount-1.0-SNAPSHOT.jar com.microsoft.example.WordCount wordcount`

    * Za __utemeljen na sustavu Windows__ klastere HDInsight: povezivanje s oluja nadzornu ploču tako da HTTPS://CLUSTERNAME.azurehdinsight.net/ u pregledniku. Zamijenite CLUSTERNAME klaster naziva HDInsight i administratore ime i lozinku kada se to od vas zatraži.

        Pomoću obrasca, izvoditi sljedeće radnje:

        * __Posudu datoteke__: odaberite __Pregledaj__, a zatim odaberite datoteku __WordCount 1.0 SNAPSHOT.jar__
        * __Naziv klase__: Unesite`com.microsoft.example.WordCount`
        * __Dodatni Paramters__: kao što su Upišite neslužbeni naziv `wordcount` da biste odredili topologije

        Na kraju, odaberite __Pošalji__ da biste pokrenuli topologije.

> [AZURE.NOTE] Kada se pokrene, pokreće oluja topologije do zaustavljanja (killed). Da biste prestali topologije, koristite na `storm kill TOPOLOGYNAME` naredbe iz naredbenog retka (SSH sesije Linux klaster, primjerice,) ili pomoću korisničkog Sučelja oluja odaberite topologije, a zatim gumb __Ukloni__ .

##<a name="python-components-with-a-clojure-topology"></a>Komponente Python pomoću topologije Clojure

> [AZURE.NOTE] U ovom se primjeru dostupna je na [https://github.com/Azure-Samples/hdinsight-python-storm-wordcount](https://github.com/Azure-Samples/hdinsight-python-storm-wordcount) u direktoriju __ClojureTopology__ .

U ovom topologije stvorena pomoću [Leiningen](http://leiningen.org) da biste [stvorili novi projekt Clojure](https://github.com/technomancy/leiningen/blob/stable/doc/TUTORIAL.md#creating-a-project). Nakon toga nastali sljedeće izmjene scaffolded projektu:

* __Project.clj__: dodali ovisnosti oluja i izostavljene za stavke koje se može uzrokovati probleme kada implementiran na poslužitelj za HDInsight.
* __Resursi/resurse__: Leiningen stvara zadani `resources` imeničkog, no datoteke nalaze se prikazuju se dodavati u korijenskom direktoriju posudu datoteke stvorene iz taj projekt, a oluja očekuje datoteka u direktoriju pod nazivom `resources`. Da bi podređenu direktorij dodan i Python datoteke spremaju se u `resources/resources`. Vrijeme izvođenja, to će se smatrati korijenski (/) za pristup Python komponente.
* __src/wordcount/Core.clj__: datoteka sadrži definiciju topologije te se pozivati iz datoteke __project.clj__ . Dodatne informacije o korištenju Clojure da biste definirali oluja topologije potražite u članku [Clojure DSL](https://storm.apache.org/documentation/Clojure-DSL.html).

###<a name="build-and-run-the-project"></a>Stvaranje i pokretanje projekta

__Stvaranje i pokretanje projekta lokalno__, koristite sljedeću naredbu:

    lein clean, run

Da biste prestali topologije, koristite __Ctrl + C__.

__Omogućuje stvaranje programa uberjar i Implementacija servisa HDInsight__, poduzmite sljedeće korake:

1. Stvaranje programa uberjar koja sadrži Topologija i potrebne ovisnosti:

        lein uberjar

    To će stvoriti novu datoteku pod nazivom `wordcount-1.0-SNAPSHOT.jar` u na `target\uberjar+uberjar` direktorija.
    
2. Primijenite jednu od sljedećih načina implementacije i pokretanja topologije programa HDInsight klaster:

    * __Sustavom Linux HDInsight__
    
        1. Kopirajte datoteku da biste pomoću glavni čvor HDInsight klaster `scp`. Ako, na primjer:
        
                scp wordcount-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:wordcount-1.0-SNAPSHOT.jar
                
            Zamijenite SSH korisnik sustava za klaster i CLUSTERNAME vašim imenom klaster HDInsight korisničko ime.
            
        2. Kada datoteku kopirate klaster, koristite SSH za povezivanje s klaster i slanje posao. Informacije o korištenju SSH sa servisa HDInsight potražite u članku nešto od sljedećeg:
        
            * [Korištenje SSH sa sustavom Linux HDInsight Linux, Unix ili OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
            * [Korištenje SSH sa sustavom Linux HDInsight iz sustava Windows](hdinsight-hadoop-linux-use-ssh-windows.md)
            
        3. Nakon uspostave, koristite sljedeće da biste pokrenuli topologije:
        
                storm jar wordcount-1.0-SNAPSHOT.jar wordcount.core wordcount
    
    * __HDInsight utemeljen na sustavu Windows__
    
        1. Povežite se s oluja nadzornu ploču tako da HTTPS://CLUSTERNAME.azurehdinsight.net/ u pregledniku. Zamijenite CLUSTERNAME klaster naziva HDInsight i administratore ime i lozinku kada se to od vas zatraži.

        2. Pomoću obrasca, izvoditi sljedeće radnje:

            * __Posudu datoteke__: odaberite __Pregledaj__, a zatim odaberite datoteku __wordcount 1.0 SNAPSHOT.jar__
            * __Naziv klase__: Unesite`wordcount.core`
            * __Dodatni Paramters__: kao što su Upišite neslužbeni naziv `wordcount` da biste odredili topologije

            Na kraju, odaberite __Pošalji__ da biste pokrenuli topologije.

> [AZURE.NOTE] Kada se pokrene, pokreće oluja topologije do zaustavljanja (killed). Da biste prestali topologije, koristite na `storm kill TOPOLOGYNAME` naredbe iz naredbenog retka (SSH sesije na klaster Linux) ili pomoću korisničkog Sučelja oluja odaberite topologije, a zatim gumb __Ukloni__ .

##<a name="next-steps"></a>Daljnji koraci

U ovom dokumentu naučili kako koristiti Python komponente iz oluja topologije. Sljedeće dokumente za drugi načini korištenja Python sa servisa HDInsight potražite u članku:

* [Kako koristiti Python tok MapReduce poslove](hdinsight-hadoop-streaming-python.md)
* [Kako koristiti Python korisnički definirane funkcije (UDF) u Svinja i grozd](hdinsight-python.md)
