<properties
   pageTitle="Upravljanje Apache oluja topologija na sustavom Linux HDInsight i | Microsoft Azure"
   description="Saznajte kako implementirati, praćenje i upravljanje Apache oluja topologija pomoću oluja nadzorne ploče na sustavom Linux HDInsight. Pomoću alata za Hadoop za Visual Studio."
   services="hdinsight"
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
   ms.date="09/07/2016"
   ms.author="larryfr"/>

# <a name="deploy-and-manage-apache-storm-topologies-on-linux-based-hdinsight"></a>Upravljanje Apache oluja topologija na sustavom Linux HDInsight i

U ovom dokumentu, Informirajte se o osnovama upravljanje i nadzor oluja topologija sustavom sustavom Linux oluja na klastere HDInsight.

> [AZURE.IMPORTANT] Koraci u ovom članku potreban je oluja Linux utemeljen na klasteru HDInsight. Informacije o implementaciji i nadzor topologija na HDInsight utemeljen na sustavu Windows potražite u članku [uvođenje i upravljanje Apache oluja topologija na utemeljen na sustavu Windows HDInsight](hdinsight-storm-deploy-monitor-topology.md)

## <a name="prerequisites"></a>Preduvjeti

* **Oluja sustavom Linux odgovora na HDInsight klaster**: pogledajte [Početak rada s Apache oluja na HDInsight](hdinsight-apache-storm-tutorial-get-started-linux.md) upute o stvaranju klaster

* **Poznavanje s SSH i Pronađenim**: dodatne informacije o korištenju SSH i Pronađenim sa servisa HDInsight potražite u članku na sljedeći način:

    * **Linux, Unix ili OS X klijenata**: potražite u članku [Korištenje SSH s operacijskim sustavom Linux Hadoop na HDInsight Linux, OS X ili Unix](hdinsight-hadoop-linux-use-ssh-unix.md)

    * **Klijenti za Windows**: potražite u članku [Korištenje SSH sa sustavom Linux Hadoop na HDInsight iz sustava Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

* **Klijent programa Pronađenim**: to je dao sve sustavima Linux, Unix i OS X. Za klijente sustava Windows, preporučujemo da PSCP, koji je dostupan na [stranici za preuzimanje PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

## <a name="start-a-storm-topology"></a>Pokretanje oluja topologije

### <a name="using-ssh-and-the-storm-command"></a>Korištenje SSH i naredbu oluja

1. Korištenje SSH za povezivanje s klaster HDInsight. Zamjena **korisničko ime** na naziv svoje SSH prijava. Zamijenite **CLUSTERNAME** klaster naziva HDInsight:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    Dodatne informacije o korištenju SSH da biste se povezali svoj klaster servisa HDInsight potražite u članku sljedeće dokumente:
    
    * **Linux, Unix ili OS X klijenata**: potražite u članku [Korištenje SSH s operacijskim sustavom Linux Hadoop na HDInsight Linux, OS X ili Unix](hdinsight-hadoop-linux-use-ssh-unix.md)
    
    * **Klijenti za Windows**: potražite u članku [Korištenje SSH sa sustavom Linux Hadoop na HDInsight iz sustava Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

2. Da biste pokrenuli topologija primjera, koristite sljedeću naredbu:

        storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-0.9.3.2.2.4.9-1.jar storm.starter.WordCountTopology WordCount

    To će pokrenuti topologije WordCount primjer na klaster. Nasumično bit će generiranje rečenica i Brojanje pojavljivanja svake riječi u rečenica.

    > [AZURE.NOTE] Prilikom slanja topologije na klaster, najprije morate kopirati posudu datoteku koja sadrži klaster prije korištenja u `storm` naredbe. To je moguće napraviti pomoću na `scp` naredbe iz klijentskog programa tamo gdje postoje datoteku. Na primjer,`scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`
    >
    > Primjer WordCount i još primjera starter oluja već nalaze na svoj klaster pri `/usr/hdp/current/storm-client/contrib/storm-starter/`.

### <a name="programmatically"></a>Programski

Programski po komunikacije sa servisom Nimbus smješten u svoj klaster možete implementirati topologije za oluja na HDInsight. [https://github.com/Azure-samples/hdinsight-Java-deploy-storm-Topology](https://github.com/Azure-Samples/hdinsight-java-deploy-storm-topology) navedeni primjer Java aplikacije koji pokazuje kako uvesti i pokrenuti topologije putem servisa Nimbus.

## <a name="monitor-and-manage-using-the-storm-command"></a>Nadzor i upravljanje pomoću naredbe za oluja

Na `storm` utility vam omogućuje rad s pokretanjem topologija iz naredbenog retka. Slijedi popis često korištene naredbe. Korištenje `storm -h` cijeli popis naredbi.

### <a name="list-topologies"></a>Topologija popisa

Koristite sljedeću naredbu da biste dobili popis svih pokrenuto topologija:

    storm list
    
To će vratiti informacije sličnu ovoj:

    Topology_name        Status     Num_tasks  Num_workers  Uptime_secs
    -------------------------------------------------------------------
    WordCount            ACTIVE     29         2            263

### <a name="deactivate-and-reactivate"></a>Deaktivirajte i ponovno aktivirati

Deaktivacija topologije zaustavlja se dok je drugom procesu ili ponovno aktivirati. Da biste deaktivirali i ponovno aktivirati, koristite sljedeće:

    storm Deactivate TOPOLOGYNAME
    
    storm Activate TOPOLOGYNAME

### <a name="kill-a-running-topology"></a>Ukloni izvodi topologije

Topologija oluja jednom pokreće, nastavlja izvodi do zaustavljanja. Da biste prestali topologije, koristite sljedeću naredbu:

    storm stop TOPOLOGYNAME

### <a name="rebalance"></a>Poduzme

Rebalancing topologije omogućuje sustav da biste pregledavali parallelism topologije. Ako, na primjer, ako su promijenjene veličine klaster da biste dodali Dodatne napomene, rebalancing omogućuje izvodi topologije da biste koriste novi čvorovi.

> [AZURE.WARNING] Najprije rebalancing topologije Deaktiviraj topologiji, a zatim ravnomjerno redistributes zaposlenici zaduženi za preko klaster, a zatim na kraju vraća topologije u stanje u kojem je bila prije rebalancing došlo je do. Stoga ako topologije bio aktivan, će postati active ponovno. Ako je deaktiviran će ostati deaktiviran.

    storm rebalance TOPOLOGYNAME

## <a name="monitor-and-manage-using-the-storm-ui"></a>Nadzor i upravljanje pomoću oluja korisničkog Sučelja

Korisničko Sučelje oluja omogućuje web sučelje za rad s pokretanjem topologija, a nalazi se na svoj klaster HDInsight. Da biste pogledali oluja korisničko Sučelje, pomoću web-pregledniku otvorite __https://CLUSTERNAME.azurehdinsight.net/stormui__, pri čemu je __CLUSTERNAME__ svoj klaster.

> [AZURE.NOTE] Ako se od vas zatraži da navede korisničko ime i lozinku, unesite klaster administrator (administratora) i lozinku koju koristi pri stvaranju klaster.


### <a name="main-page"></a>Glavne stranice

Glavne stranice korisničkog sučelja oluja nalaze se sljedeće informacije:

* **Klaster sažetak**: osnovne informacije o klaster oluja.

* **Topologija sažetak**: popis izvodi topologija. Da biste pogledali dodatne informacije o određenim topologija pomoću veze u ovoj sekciji.

* **Nadzornik sažetak**: informacije o nadzornik oluja.

* **Konfiguriranje nimbus**: Nimbus konfiguracije za klaster.

### <a name="topology-summary"></a>Topologija sažetka

Odabir veze u odjeljku **Topologija sažetka** prikazuje sljedeće informacije o topologiji:

* **Topologija sažetak**: osnovne informacije o topologije.

* **Topologija akcije**: Upravljanje akcije koje možete izvršiti za topologije.

  * **Aktiviraj**: Obrada životopise deaktiviran topologije.

  * **Deaktiviranje**: zaustavlja izvodi topologije.

  * **Poduzme**: prilagođava parallelism topologije. Izvodi topologija treba poduzme kada ste promijenili broj čvorovi u klasteru. Time se omogućuje topologije da biste prilagodili parallelism za vaše za poboljšani ili smanjiti broj čvorovi u klasteru.

    Dodatne informacije potražite u članku <a href="http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html" target="_blank">objašnjenje parallelism oluja topologije</a>.

  * **Ukloni**: prekida oluja topologije nakon u navedenom vremenskom roku.

* **Topologija stat**: statističkih podataka o topologije. Postavljanje vremenskog razdoblja za preostale stavke na stranici pomoću veze u stupcu **prozora** .

* **Spouts**: spouts koristi topologije. Da biste pogledali dodatne informacije o određenim spouts pomoću veze u ovoj sekciji.

* **Vijci s maticama**: Vijci koristi topologije. Da biste pogledali dodatne informacije o određenim Vijci pomoću veze u ovoj sekciji.

* **Konfiguracija topologije**: konfiguracija topologije odabranog.

### <a name="spout-and-bolt-summary"></a>Spout i sažetak munje

Odabirom s spout **Spouts** ili **Vijci s Maticama** sekcija prikazuje sljedeće informacije o odabrane stavke:

* **Komponenta sažetak**: osnovne informacije o spout ili munje.

* **Spout/munje stat**: statističkih podataka o spout ili munje. Postavljanje vremenskog razdoblja za preostale stavke na stranici pomoću veze u stupcu **prozora** .

* **Unos stat** (samo za munje): informacije o unos strujanja troše na munje.

* **Izlaz stat**: informacije o strujanja čuje tako da to spout ili munje.

* **Executors**: informacije o slučajevima spout ili munje. Odaberite stavku **priključak** za određene executor da biste pogledali evidenciju dijagnostičke informacije za tu instancu.

* **Pogreške**: pogreška za to spout ili munje.

## <a name="rest-api"></a>REST API-JA

Korisničko Sučelje oluja ugrađena je pri vrhu REST API-JA, pa možete izvršiti slične upravljanje i nadzor funkcionalnost pomoću REST API-JA. REST API-JA možete koristiti za stvaranje prilagođenih alata za Upravljanje projektom i nadzor topologija oluja.

Dodatne informacije potražite u članku [Oluja korisničkog Sučelja REST API -JA](http://storm.apache.org/releases/0.9.6/STORM-UI-REST-API.html). Sljedeće informacije je za korištenje REST API-JA s Apache oluja na HDInsight.

> [AZURE.IMPORTANT] Oluja REST API-JA ne javno dostupno putem Interneta, a morate pristupiti pomoću programa tunelom SSH HDInsight čvor glavni klaster. Informacije o stvaranju i korištenju programa tunelom SSH potražite u članku [Korištenje SSH tuneliranje da biste pristupili web Ambari korisničkog Sučelja, ResourceManager, JobHistory, NameNode, Oozie, i druge web-mjesta korisničkog Sučelja](hdinsight-linux-ambari-ssh-tunnel.md).

### <a name="base-uri"></a>Osnovni URI-JA

Osnovni URI za REST API-JA na sustavom Linux HDInsight klastere dostupna je na glavni čvor pri **api/https://HEADNODEFQDN:8744/v1/**; Međutim, naziv domene glavni čvora generira tijekom stvaranja klaster, a nije statične.

Na potpuno kvalificirani naziv domene (FQDN) za čvor glavni klaster možete pronaći na nekoliko različitih načina:

* __Iz programa SSH sesiju__: pomoću naredbe `headnode -f` iz sesiju SSH na klaster.

* __S weba Ambari__: odaberite __usluge__ na vrhu stranice, a zatim odaberite __oluja__. Na kartici __Sažetak__ odaberite __Poslužitelj oluja korisničkog Sučelja__. FQDN čvor oluja korisničkog Sučelja i REST API-JA na kojoj se izvršava bit će se pri vrhu stranice.

* __Iz Ambari REST API -JA__: pomoću naredbe `curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/STORM/components/STORM_UI_SERVER"` Dohvaćanje informacija o čvor pokrenute oluja korisničkog Sučelja i REST API-JA na. Zamijenite administratorsku lozinku za klaster __lozinku__ . __CLUSTERNAME__ zamijenite nazivom klaster. U odgovoru, "host_name" sadrži FQDN čvor.

### <a name="authentication"></a>Provjera autentičnosti

Zahtjevi za REST API-JA morate koristiti **osnovnu provjeru autentičnosti**pa koristite HDInsight klaster administrator ime i lozinku.

> [AZURE.NOTE] Budući da se osnovna provjera autentičnosti pošalje pomoću običnog teksta, trebali biste **uvijek** koristi HTTPS za zaštitu komunikacije s klaster.

### <a name="return-values"></a>Vraćanje vrijednosti

Informacije koje je vratio REST API-JA možda samo moći koristiti iz unutar klaster ili virtualnim strojevima na istoj mreži virtualne Azure kao klaster. Na primjer, na potpuno kvalificirani naziv domene (FQDN) vraća za poslužitelje Zookeeper neće biti dostupno putem Interneta.

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste naučili kako uvesti i praćenje topologija putem nadzorne ploče za oluja, Saznajte kako [razviti utemeljena na topologija pomoću Maven](hdinsight-storm-develop-java-topology.md).

Popis više topologija primjer, potražite u članku [primjer topologija za oluja na HDInsight](hdinsight-storm-example-topology.md).
