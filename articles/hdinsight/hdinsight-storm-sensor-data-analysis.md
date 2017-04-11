<properties
   pageTitle="Analiza podataka senzor pomoću oluja Apache i HBase | Microsoft Azure"
   description="Saznajte kako se povezati Apache oluja virtualne mreže. Pomoću oluja HBase postupak senzor podataka iz centra za događaj i vizualizacija s D3.js."
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
   ms.date="09/20/2016"
   ms.author="larryfr"/>

# <a name="analyze-sensor-data-with-apache-storm-event-hub-and-hbase-in-hdinsight-hadoop"></a>Analiza podataka senzor pomoću Apache oluja, koncentrator za događaj i HBase u HDInsight (Hadoop) 

Saznajte kako pomoću Apache oluja na HDInsight postupak senzor podataka iz centra za događaj Azure, spremite ga u Apache HBase na HDInsight i vizualizacija pomoću D3.js koji se izvodi kao web-aplikaciju programa Azure.

Voditelj resursa Azure predložak koji se koristi u ovom dokumentu pokazuje kako stvoriti više Azure resursa u grupu resursa. Konkretno, stvara Azure virtualne mreže, dva klastere HDInsight (oluja i HBase) i web-aplikacije programa Azure. Node.js implementacija nadzorne ploče u stvarnom vremenu web automatski implementiran na web-aplikaciji.

> [AZURE.NOTE] Informacije u ovom dokumentu i primjer navedena testirate pomoću 3.4 klaster verzije i sustavom Linux 3,3 HDInsight.

## <a name="prerequisites"></a>Preduvjeti

* Azure pretplate. Pogledajte [Početak Azure besplatnu probnu verziju](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

    > [AZURE.IMPORTANT] Ne morate postojeće HDInsight klaster; Koraci u ovom dokumentu će stvoriti u sljedećim resursima:
    >
    > * Azure virtualne mreže
    > * Oluja na HDInsight klaster (Linux temelji, 2 tempiranja čvorove)
    > * Na HBase na HDInsight klaster (Linux temelji, 2 tempiranja čvorove)
    > * Web-aplikaciju programa Azure koji je glavno računalo web nadzorne ploče

* [Node.js](http://nodejs.org/): koristi se za pregled nadzorne ploče web lokalno na razvojno okruženje.

* [Java i JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/index.html): služi za razvoj oluja topologije.

* [Maven](http://maven.apache.org/what-is-maven.html): služi za stvaranje i uređivanje projekta.

* [Brojka](http://git-scm.com/): koristiti za preuzimanje projekta s GitHub.

* Klijent za __SSH__ : koristi za povezivanje sa sustavom Linux HDInsight skupina. Dodatne informacije o korištenju SSH sa servisa HDInsight potražite u članku sljedećim dokumentima.

    * [Korištenje SSH s HDInsight iz klijenta za Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

    * [Korištenje SSH s HDInsight od klijenta Linux, Unix ili Mac](hdinsight-hadoop-linux-use-ssh-unix.md)

    > [AZURE.NOTE] Morate imati pristup na `scp` naredba koji se koristi da biste kopirali datoteke između lokalne platforme i klaster HDInsight pomoću SSH.

## <a name="architecture"></a>Arhitektura

![Arhitektura dijagrama](./media/hdinsight-storm-sensor-data-analysis/devicesarchitecture.png)

U ovom se primjeru sastoji se od sljedeće komponente:

* **Azure događaj koncentratora**: sadrži podatke koji se prikupljaju iz senzora. U ovom primjeru aplikacija je pod uvjetom da koje generira podatke.

* **Oluja na HDInsight**: nudi u stvarnom vremenu obradu podataka iz centra za događaj.

* **HBase na HDInsight**: nudi stalni spremišta podataka NoSQL za podatke kada bude obrađen po oluja.

* **Servis za Azure virtualne mreže**: omogućuje sigurno komunikaciju između oluja na HDInsight i HBase na klastere HDInsight.

    > [AZURE.NOTE] Virtualne mreže je potrebno da biste mogli koristiti klijent API Java HBase dok se ne prikazuje preko javno pristupnik za klastere HBase. Instaliranje HBase i oluja klastere u istom virtualne mreže omogućuje klaster oluja (ili neki drugi sustav virtualne mreže) da biste izravno pristupili HBase pomoću API klijent.

* **Nadzorna ploča web-mjesto**: Ogledna nadzorna ploča koje grafikonima podatke u stvarnom vremenu.

    * Na web-mjesto je implementirana u Node.js, tako da mogu se izvoditi na operacijske sustave klijenta za testiranje ili može biti implementirano u Azure web-mjesta.

    * [Socket.IO](http://socket.io/) koristi se za komunikaciju u stvarnom vremenu između topologije oluja i web-mjesta.

        > [AZURE.NOTE] Ovo je za implementaciju detalja. Možete koristiti bilo koji framework komunikacije, kao što su neobrađenog WebSockets ili SignalR.

    * [D3.js](http://d3js.org/) koristi se za prikaz podataka koji se šalju na web-mjesto.

> [AZURE.IMPORTANT] Dva klastere su potrebni, kao što je ne postoji način da biste stvorili jedan HDInsight klaster oluja i HBase podržani.

Topologije čita podatke iz koncentratora događaja pomoću klase [org.apache.storm.eventhubs.spout.EventHubSpout](http://storm.apache.org/releases/0.10.1/javadocs/org/apache/storm/eventhubs/spout/class-use/EventHubSpout.html) i zapisuje podatke u HBase pomoću klase [org.apache.storm.hbase.bolt.HBaseBolt](https://storm.apache.org/javadoc/apidocs/org/apache/storm/hbase/bolt/class-use/HBaseBolt.html) . Komunikacija s web-mjesta se postiže korištenjem [socket.io client.java](https://github.com/nkzawa/socket.io-client.java).

Ovo je dijagram topologije.

![Topologija dijagrama](./media/hdinsight-storm-sensor-data-analysis/sensoranalysis.png)

> [AZURE.NOTE] Ovo je vrlo pojednostavljeni prikaz topologije. Prilikom izvođenja instance komponente svaku od njih će se stvoriti za svaki particija središtu događaja koji je čitanja. Ove instance su raspodijeliti čvorovi u klasteru i podataka usmjeravanja između njih na sljedeći način:
>
> * Podatke iz sustava spout na analizator je rasporediti opterećenje.
> * Podatke iz sustava analizator nadzorne ploče i HBase grupirane ID uređaja tako da se poruke na istom uređaju uvijek tijeka na istom komponentu.

### <a name="topology-components"></a>Topologija komponente

* **EventHub Spout**: U spout je kao dio unijela Apache oluja verzije 0.10.0 i noviji.

    > [AZURE.NOTE] Spout koncentratora događaja koji se koristi u ovom primjeru zahtijeva oluja HDInsight klaster verzije 3,3 ili 3.4. Informacije o korištenju koncentratora događaja pomoću starije verzije programa oluja na HDInsight potražite u članku [postupak događaje iz koncentratora događaj Azure s oluja na HDInsight](hdinsight-storm-develop-java-event-hub-topology.md).

* **ParserBolt.java**: podatke koje je čuje tako da na spout je neobrađenog JSON pa se povremeno više događaj čuje odjednom. U ovom munje pokazuje kako čitati podatke čuje tako da na spout i šalji novi strujanje kao n-torke koji sadrži više polja.

* **DashboardBolt.java**: to prikazano kako koristiti biblioteku Socket.io klijenta za Java slanje podataka stvarnom vremenu web nadzorne ploče.

U ovom se primjeru koristi [Flux](https://storm.apache.org/releases/0.10.0/flux.html) framework definiciju topologije nalazi se u YAML datoteke. Postoje dva:

* __bez hbase.yaml__ - koristi ovu datoteku kada testiranje topologije u razvojno okruženje. Budući da ne možete pristupiti na HBase Java API-JA s izvan virtualne mreže klaster nalazi se u ga ne pomoću komponente HBase.

* __s hbase.yaml__ - koristi ovu datoteku pri implementaciji topologije klaster oluja. Budući da se pokreće se u istu virtualne mreže kao HBase klaster, koristi HBase komponente.

## <a name="prepare-your-environment"></a>Pripremu okruženja

Prije korištenja u ovom primjeru, morate stvoriti programa Azure događaj središte koji čita topologije oluja iz.

### <a name="configure-event-hub"></a>Konfiguriranje događaja koncentratora

Koncentrator za događaj je izvor podataka u ovom primjeru. Poduzmite sljedeće korake da biste stvorili novi događaj koncentrator.

1. [Portal za Azure](https://portal.azure.com)odaberite **+ Novo** -> __Internet stvari__ -> __Koncentratora za događaj__.

2. Na plohu __Stvaranje prostor naziva__ izvedite sljedeće zadatke:

    1. Unesite __naziv__ za prostor za naziv.
    2. Odaberite sloj cijene. __Osnovni__ je dovoljno u ovom primjeru.
    3. Odaberite Azure __pretplatu__ za korištenje.
    4. Odaberite postojeću grupu resursa ili stvorite novi.
    5. Odaberite __mjesto__ za koncentratora za događaj.
    6. Odaberite __Prikvači na nadzornu ploču__, a zatim kliknite __Stvori__.

3. Kada završi postupak stvaranja, prikazuje se plohu događaj koncentratora za vaš prostor naziva. Na tom mjestu, odaberite __+ Dodaj koncentrator za događaj__. Na plohu __Stvaranje koncentrator događaj__ unesite naziv __sensordata__ , a zatim odaberite __Stvori__. Ostavite ostala polja na zadane vrijednosti.

4. Plohu događaj koncentratora za vaš prostor naziva, odaberite __Događaj koncentratora__. Odaberite stavku __sensordata__ .

5. Plohu za sensordata koncentrator događaj, odaberite __pravila za zajedničko korištenje programa access__. Koristite __+ Dodaj__ vezu da biste dodali sljedeća pravila:


  	| Naziv pravila | Zahtjevima |
  	| ----- | ----- |
  	| uređaji | Pošalji |
  	| oluja | Slušanje |

5. Odaberite oba pravila i zabilježite vrijednost __PRIMARNOG ključa__ . Trebat će vam vrijednost za oba pravila u budućim korake.

## <a name="download-and-configure-the-project"></a>Preuzimanje i konfiguriranje projekta

Da biste preuzeli projekta s GitHub, koristite sljedeće.

    git clone https://github.com/Blackmist/hdinsight-eventhub-example

Po završetku naredbu imat ćete sljedeću strukturu direktorija:

    hdinsight-eventhub-example/
        TemperatureMonitor/ - this contains the topology
            resources/
                log4j2.xml - set logging to minimal
                no-hbase.yaml - topology definition for local testing
                with-hbase.yaml - topology definition that uses HBase in a virutal network
            src/ - the Java bolts
            dev.properties - contains configuration values for your environment
        dashboard/nodejs/ - this is the node.js web dashboard
        SendEvents/ - utilities to send fake sensor data

> [AZURE.NOTE] Ovaj dokument ne prelazi sve detalje kod obuhvaćeno uzorka; Međutim, kod potpuno komentar.

Otvorite datoteku **hdinsight-eventhub-example/TemperatureMonitor/dev.properties** i dodajte podatke o događaju koncentrator za sljedeće:

    eventhub.read.policy.name: storm
    eventhub.read.policy.key: KeyForTheStormPolicy
    eventhub.namespace: YourNamespace
    eventhub.name: sensordata

> [AZURE.NOTE] U ovom se primjeru pretpostavlja kao naziv pravila koja sadrži zahtjev __preslušali__ koristi __oluja__ , a koncentratora za događaj zove __sensordata__.

 Spremite datoteku nakon što dodate taj podatak.

## <a name="compile-and-test-locally"></a>Kompiliranje i testiranje lokalno

Prije nego što testiranje, morate pokrenuti nadzorne ploče da biste pogledali izlaz topologije i generiranje podataka za pohranu u središte događaj.

> [AZURE.IMPORTANT] Komponenta HBase ovaj topologije nije aktivna kada testiranje lokalno, kao Java API-JA za klaster HBase ne može pristupiti iz izvan mreže virtualne Azure sadrži skupina.

### <a name="start-the-web-application"></a>Pokretanje web-aplikacije

1. Otvorite novi naredbeni redak ili terminal i mijenjati mape **hdinsight-eventhub-primjer/nadzorne ploče**, a zatim koristite sljedeću naredbu da biste instalirali ovisnosti potrebno web-aplikacije:

        npm install

2. Da biste pokrenuli aplikaciju, koristite sljedeću naredbu:

        node server.js

    Trebali biste vidjeti poruku koja je slična sljedećoj:

        Server listening at port 3000

2. Otvorite web-preglednik i unesite **http://localhost:3000 /** kao adresu. Trebali biste vidjeti stranicu koja je slična sljedećoj:

    ![Nadzorna ploča za web](./media/hdinsight-storm-sensor-data-analysis/emptydashboard.png)

    Ostavite taj naredbeni redak ili terminal otvoren. Nakon testiranja, koristite Ctrl-C da biste prestali web-poslužitelj.

### <a name="start-generating-data"></a>Pokretanje generiranje podataka

> [AZURE.NOTE] Koraci u ovom odjeljku koristite Node.js tako da se koriste na svim platformama. Primjeri drugih jezika potražite u članku **SendEvents** direktorija.

1. Otvorite novi naredbeni redak, ljuske ili terminal i mijenjati mape **hdinsight-eventhub-primjer/SendEvents/nodejs**, a zatim koristite sljedeću naredbu da biste instalirali ovisnosti potrebno aplikacija:

        npm install

2. Otvorite **app.js** datoteku u uređivaču teksta, a zatim dodajte podatke događaj koncentrator ste nabavili ranije:

        // ServiceBus Namespace
        var namespace = 'YourNamespace';
        // Event Hub Name
        var hubname ='sensordata';
        // Shared access Policy name and key (from Event Hub configuration)
        var my_key_name = 'devices';
        var my_key = 'YourKey';
    
    > [AZURE.NOTE] U ovom se primjeru pretpostavlja da koju ste koristili __sensordata__ kao naziv događaja koncentratora i __uređajima__ kao naziv pravila koja sadrži __Pošalji__ zahtjev.

2. Da biste umetnuli nove stavke u središte događaj, koristite sljedeću naredbu:

        node app.js

    Trebali biste vidjeti nekoliko redaka izlaza koje sadrže podataka koji se šalju koncentrator za događaj. Te izgledat će otprilike ovako:

        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"0","Temperature":7}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"1","Temperature":39}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"2","Temperature":86}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"3","Temperature":29}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"4","Temperature":30}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"5","Temperature":5}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"6","Temperature":24}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"7","Temperature":40}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"8","Temperature":43}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"9","Temperature":84}

### <a name="start-the-topology"></a>Pokretanje topologije

2. Otvorite novi naredbeni redak, ljuske ili imenika __Servisa hdinsight-eventhub-primjer/TemperatureMonitor__terminal i promjena, a zatim koristite sljedeću naredbu da biste pokrenuli topologije:

        mvn compile exec:java -Dexec.args="--local -R /no-hbase.yaml --filter dev.properties"
    
    Ako koristite PowerShell, upotrijebite sljedeće:

        mvn compile exec:java "-Dexec.args=--local -R /no-hbase.yaml --filter dev.properties"

    > [AZURE.NOTE] Ako se u sustavu Unix/Linux/OS X, a imate [instaliran oluja u razvojno okruženje](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html), možete koristiti sljedeće naredbe:
    >
    > `mvn compile package`
    > `storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /no-hbase.yaml`

    Time ćete topologije definirana u datoteci __bez hbase.yaml__ u načinu rada za lokalni. Vrijednosti koji se nalazi u datoteci __dev.properties__ sadrže informacije o vezi za događaj koncentratora. Kada se pokrene, topologije čita stavke iz koncentratora za događaj i šalje ih na nadzornoj ploči radi na lokalno računalo. Trebali biste vidjeti retke koji se prikazuju u web nadzorne ploče, sličnu ovoj:

    ![Nadzorna ploča s podacima](./media/hdinsight-storm-sensor-data-analysis/datadashboard.png)

3. Dok se izvodi na nadzornoj ploči, koristite na `node app.js` naredbe iz prethodnih koraka da biste poslali nove podatke s koncentratorima događaj. Budući da slučajno generiraju temperatura vrijednosti, na grafikonu trebali biste ažurirati za prikaz velike promjene u temperature.

    > [AZURE.NOTE] Mora biti u direktoriju __Hdinsight-eventhub-primjer/SendEvents/Nodejs__ kada se koristi u `node app.js` naredbe.

3. Nakon potvrđivanja da to funkcionira, zaustavite topologije pomoću Ctrl + C. Koristite Ctrl + C da biste zaustavili i lokalne web-poslužitelj.

## <a name="create-a-storm-and-hbase-cluster"></a>Stvaranje oluja i HBase klaster

Da biste pokrenuli topologije na HDInsight, a da biste omogućili munje HBase, morate stvoriti novi oluja klaster i HBase klaster. Koraci u ovom odjeljku pomoću [predloška Azure Voditelj resursa](../resource-group-template-deploy.md) da biste stvorili novu Azure virtualne mreže i oluja i HBase klaster na virtualne mreže. Predložak web-aplikaciju programa Azure stvara i uvodi kopiju nadzorne ploče u nju.

> [AZURE.NOTE] Virtualne mreže se koristi bi topologije sustavom klaster oluja izravno možete komunicirati s HBase klaster pomoću HBase Java API-JA.

Voditelj resursa predložak koji se koristi u ovom dokumentu nalazi se u spremniku javno blob pri __https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-hbase-storm-cluster-in-vnet.json__.

1. Kliknite gumb sljedeće se prijaviti Azure i otvorite predložak Voditelj resursa na portalu za Azure.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-storm-cluster-in-vnet.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Iz plohu **parametara** unesite sljedeće:

    ![HDInsight parametara](./media/hdinsight-storm-sensor-data-analysis/parameters.png)
    
    * **BASECLUSTERNAME**: tu vrijednost koristit će se kao osnovni naziv u oluja i HBase klaster. Na primjer, unos __hdi__ će stvoriti oluja klaster pod nazivom __oluja hdi__ i programa HBase klaster pod nazivom __hbase hdi__.
    * __CLUSTERLOGINUSERNAME__: korisničko ime za administratore za klastere oluja i HBase.
    * __CLUSTERLOGINPASSWORD__: administratorsku lozinku korisnika za klastere oluja i HBase.
    * __SSHUSERNAME__: U SSH korisnika da biste stvorili za klastere oluja i HBase.
    * __SSHPASSWORD__: lozinku za korisnika SSH za klastere oluja i HBase.
    * __Lokacija__: skupina stvorit će se u regiji.
    
    Kliknite __u redu__ da biste spremili parametre.
    
3. Da biste stvorili novu grupu resursa ili odaberite postojeći koristiti grupu __resursa__ .

4. U padajući izbornik __lokacija resursa grupe__ odaberite na isto mjesto kao koju ste odabrali za parametar __mjesto__ .

5. Odaberite __pravne uvjete__, a zatim odaberite __Stvori__.

6. Na kraju, provjerite __Prikvači na nadzornu ploču__ , a zatim odaberite __Stvori__. Trebat će otprilike 20 minuta da biste stvorili skupina.

Kada stvorite resursa koji će biti preusmjereni na plohu za grupu resursa koji sadrži klastere i nadzorne ploče za web.

![Plohu grupu resursa za vnet i klastere](./media/hdinsight-storm-sensor-data-analysis/groupblade.png)

> [AZURE.IMPORTANT] Obratite pozornost na to da su nazivi klastere HDInsight __Oluja BASENAME__ i __hbase BASENAME__, pri čemu je BASENAME ime koje ste naveli u predložak. Prilikom povezivanja s skupina će koristiti te nazive u noviji korake. Imajte na umu da je naziv web-mjesta nadzorne ploče __basename-nadzorne ploče__. Koristit ćete to kasnije prilikom pregledavanja na nadzornoj ploči.

## <a name="configure-the-dashboard-bolt"></a>Konfiguriranje munje nadzorne ploče

Da biste slali podatke na nadzornoj ploči u uveden kao web-aplikacijama, mijenjate sljedeći redak u datoteci __dev.properties__ :

    dashboard.uri: http://localhost:3000

Promjena `http://localhost:3000` da biste `http://BASENAME-dashboard.azurewebsites.net` i spremite datoteku. Zamijenite __BASENAME__ osnovni naziv ste unijeli u prethodnom koraku. Možete koristiti i grupa resursa koje ste prethodno stvorili odaberite nadzornu ploču i prikaz URL-a.

## <a name="create-the-hbase-table"></a>Stvaranje tablice HBase

Da biste pohranili podatke u HBase, ne možemo morate stvoriti tablicu. Općenito želite unaprijed stvoriti resursi koji je potrebno oluja pisati, kao stvaranja resursa iz unutar oluja topologije može rezultirati više, raspodijeljeno kopija kod stvaranja isti resurs. Stvaranje resursa izvan topologije i samo za čitanje/pisanje i analitiku pomoću oluja.

1. Korištenje SSH za povezivanje s HBase klaster pomoću SSH korisnika i lozinke koje se dodjeljuje predloška prilikom stvaranja klaster. Na primjer, ako se povezujete pomoću na `ssh` naredbu, koristite sljedeću sintaksu:

        ssh USERNAME@hbase-BASENAME-ssh.azurehdinsight.net
    
    U toj naredbi zamijenite __korisničko ime__ SSH korisničko ime koje ste naveli prilikom stvaranja klaster i __BASENAME__ s osnovnim ime koje ste naveli. Kada se to od vas zatraži, unesite lozinku za korisnika SSH.

2. Sesije SSH početnom ljuske HBase.

        hbase shell
    
    Kada je učitana ljuske, prikazat će se `hbase(main):001:0>` upit.

3. Iz ljuske HBase unesite sljedeću naredbu da biste stvorili tablicu da biste pohranili podatke senzora:

        create 'SensorData', 'cf'

4. Provjerite da su tablice stvorena pomoću sljedeće naredbe:

        scan 'SensorData'
        
    To vraća informacije slično sljedećem primjeru, koji označava da nema 0 redaka u tablici.
    
        ROW                   COLUMN+CELL                                       0 row(s) in 0.1900 seconds

5. Unesite sljedeće da biste izašli iz ljuske HBase:

        exit

## <a name="configure-the-hbase-bolt"></a>Konfiguriranje munje HBase

Da biste napisali HBase iz skupine oluja, navedite munje HBase s detaljima konfiguracije svoj klaster HBase. Najjednostavniji način za to je da biste preuzeli __hbase site.xml__ iz skupine i uključiti ga u projektu. Morate uklonite i nekoliko ovisnosti u datoteci __pom.xml__ koji učitavanje komponente oluja hbase i potrebne ovisnosti.

> [AZURE.IMPORTANT] Morate preuzeti i datoteke oluja hbase.jar navedeni su na vašem oluja na klasteru klaster 3,3 ili 3.4 HDInsight; Ova verzija ne Sastavi da biste radili s HBase 1.1.x koji se koristi za HBase na HDInsight 3,3 i 3.4 klastere. Ako koristite komponentu oluja hbase s drugih mjesta, može prevesti protiv stariju verziju HBase.

### <a name="download-the-hbase-sitexml"></a>Preuzmite hbase site.xml

Preuzimanje datoteke __hbase site.xml__ iz skupine pomoću Pronađenim iz naredbenog retka. U sljedećem primjeru korisnik SSH koje ste naveli prilikom stvaranja klaster i __BASENAME__ s osnovnim ime koje ste prethodno naveli zamijenite __korisničko ime__ . Kada se to od vas zatraži, unesite lozinku za korisnika SSH. Zamjena na `/path/to/TemperatureMonitor/resources/hbase-site.xml` putom datoteke u projekt TemperatureMonitor.

    scp USERNAME@hbase-BASENAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml /path/to/TemperatureMonitor/resources/hbase-site.xml

To će preuzeti __hbase site.xml__ navedeni put.

### <a name="download-and-install-the-storm-hbase-component"></a>Preuzmite i instalirajte komponentu oluja hbase

1. Preuzimanje datoteke __oluja hbase.jar__ iz skupine oluja pomoću Pronađenim iz naredbenog retka. U sljedećem primjeru korisnik SSH koje ste naveli prilikom stvaranja klaster i __BASENAME__ s osnovnim ime koje ste prethodno naveli zamijenite __korisničko ime__ . Kada se to od vas zatraži, unesite lozinku za korisnika SSH.

        scp USERNAME@storm-BASENAME-ssh.azurehdinsight.net:/usr/hdp/current/storm-client/contrib/storm-hbase/storm-hbase*.jar .

    To će preuzeti datoteku pod nazivom `storm-hbase-####.jar`, pri čemu ### je broj verzije oluja za ovaj klaster. Zabilježite broj, kao što je kasnije koristili.

2. Koristite sljedeću naredbu da biste instalirali komponentu u lokalnom spremištu Maven na razvojno okruženje. Time se omogućuje Maven da biste pronašli paket za sastavljanje projekta. Zamjena __####__ s brojem verzije uključeni u nazivu datoteke.

        mvn install:install-file -Dfile=storm-hbase-####.jar -DgroupId=org.apache.storm -DartifactId=storm-hbase -Dversion=#### -Dpackaging=jar
    
    Ako koristite PowerShell, koristite sljedeću naredbu:

        mvn install:install-file "-Dfile=storm-hbase-####.jar" "-DgroupId=org.apache.storm" "-DartifactId=storm-hbase" "-Dversion=####" "-Dpackaging=jar"

### <a name="enable-the-storm-hbase-component-in-the-project"></a>Omogućivanje komponentu oluja hbase u projektu

1. Otvorite datoteku __TemperatureMonitor/pom.xml__ i izbrišite sljedeće retke:

        <!-- uncomment this section to enable the hbase-bolt
        end comment for hbase-bolt section -->
    
    > [AZURE.IMPORTANT] Izbrisati samo tih dvaju redaka; Nemojte brisati bilo koji od redaka između njih.
    
    Time se omogućuje nekoliko komponenti koje su potrebne prilikom komunikacije s HBase pomoću munje hbase.

2. Pronalaženje sljedeće retke, a zatim zamijenite __####__ s brojem verzije datoteke oluja hbase prethodno preuzeti.

        <dependency>
            <groupId>org.apache.storm</groupId>
            <artifactId>storm-hbase</artifactId>
            <version>####</version>
        </dependency>

    > [AZURE.IMPORTANT] Broj verzije mora podudarati verzija koristi prilikom instalacije komponente u lokalnom spremištu Maven, kao što je Maven na temelju tih podataka da biste učitali komponentu kada Stvaranje projekta.

2. Spremite datoteku __pom.xml__ .

## <a name="build-package-and-deploy-the-solution-to-hdinsight"></a>Stvaranje paketa i implementacija rješenja za HDInsight

U razvojno okruženje, poduzmite sljedeće korake za implementaciju topologije oluja klaster oluja.

1. Iz imenika __TemperatureMonitor__ , koristite sljedeću naredbu za izvođenje novi Sastavi i pakirati POSUDU iz projekta:

        mvn clean compile package

    To će stvoriti datoteku pod nazivom **TemperatureMonitor 1.0 SNAPSHOT.jar** u direktoriju **cilj** projekta.

2. Da biste prenijeli datoteke __TemperatureMonitor 1.0 SNAPSHOT.jar__ na svoj klaster oluja pomoću pronađenim. U sljedećem primjeru korisnik SSH koje ste naveli prilikom stvaranja klaster i __BASENAME__ s osnovnim ime koje ste prethodno naveli zamijenite __korisničko ime__ . Kada se to od vas zatraži, unesite lozinku za korisnika SSH.

        scp target\TemperatureMonitor-1.0-SNAPSHOT.jar USERNAME@storm-BASENAME-ssh.azurehdinsight.net:TemperatureMonitor-1.0-SNAPSHOT.jar
    
    > [AZURE.NOTE] Kao što je nekoliko megabajta ID-a to može potrajati nekoliko minuta da biste prenijeli datoteke.

    Da biste prenijeli datoteke __dev.properties__ pomoću pronađenim kao što je to sadrži informacije koje služe za povezivanje s koncentratorima događaja i nadzorne ploče.

        scp dev.properties USERNAME@storm-BASENAME-ssh.azurehdinsight.net:dev.properties

3. Nakon prijenosa datoteka povezati klaster pomoću SSH.

        ssh USERNAME@storm-BASENAME-ssh.azurehdinsight.net

4. Iz sesije SSH, koristite sljedeću naredbu da biste pokrenuli topologije.

        storm jar TemperatureMonitor-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /with-hbase.yaml --filter dev.properties
    
    Time ćete topologije pomoću definiciju topologije u datoteci __s hbase.yaml__ i konfiguracijskih vrijednosti u datoteci __dev.properties__ .

3. Nakon topologije, otvorite preglednik nije objavljena na Azure na web-mjesto, zatim koristite na `node app.js` naredbu za slanje podataka u koncentrator za događaj. Prikazat će se ažurirati da biste prikazali podatke na nadzornoj ploči web.

    ![Nadzorna ploča](./media/hdinsight-storm-sensor-data-analysis/datadashboard.png)

## <a name="view-hbase-data"></a>Prikaz HBase podataka

Nakon što ste poslali podatke pomoću topologije `node app.js`, poduzmite sljedeće korake za povezivanje s HBase i provjerite je li napisan da podatke tablice koji ste prethodno stvorili.

1. Korištenje SSH za povezivanje s HBase klaster.

        ssh USERNAME@hbase-BASENAME-ssh.azurehdinsight.net

2. Sesije SSH početnom ljuske HBase.

        hbase shell
    
    Kada je učitana ljuske, prikazat će se `hbase(main):001:0>` upit.

2. Prikaz redaka iz tablice:

        scan 'SensorData'
        
    To će se vratiti podatke slično sljedećem, koja označava da nema 0 redaka u tablici.
    
        hbase(main):002:0> scan 'SensorData'
        ROW                             COLUMN+CELL
        \x00\x00\x00\x00               column=cf:temperature, timestamp=1467290788277, value=\x00\x00\x00\x04
        \x00\x00\x00\x00               column=cf:timestamp, timestamp=1467290788277, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x01               column=cf:temperature, timestamp=1467290788348, value=\x00\x00\x00M
        \x00\x00\x00\x01               column=cf:timestamp, timestamp=1467290788348, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x02               column=cf:temperature, timestamp=1467290788268, value=\x00\x00\x00R
        \x00\x00\x00\x02               column=cf:timestamp, timestamp=1467290788268, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x03               column=cf:temperature, timestamp=1467290788269, value=\x00\x00\x00#
        \x00\x00\x00\x03               column=cf:timestamp, timestamp=1467290788269, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x04               column=cf:temperature, timestamp=1467290788356, value=\x00\x00\x00>
        \x00\x00\x00\x04               column=cf:timestamp, timestamp=1467290788356, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x05               column=cf:temperature, timestamp=1467290788326, value=\x00\x00\x00\x0D
        \x00\x00\x00\x05               column=cf:timestamp, timestamp=1467290788326, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x06               column=cf:temperature, timestamp=1467290788253, value=\x00\x00\x009
        \x00\x00\x00\x06               column=cf:timestamp, timestamp=1467290788253, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x07               column=cf:temperature, timestamp=1467290788229, value=\x00\x00\x00\x12
        \x00\x00\x00\x07               column=cf:timestamp, timestamp=1467290788229, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x08               column=cf:temperature, timestamp=1467290788336, value=\x00\x00\x00\x16
        \x00\x00\x00\x08               column=cf:timestamp, timestamp=1467290788336, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x09               column=cf:temperature, timestamp=1467290788246, value=\x00\x00\x001
        \x00\x00\x00\x09               column=cf:timestamp, timestamp=1467290788246, value=2015-02-10T14:43.05.00320Z
        10 row(s) in 0.1800 seconds

    > [AZURE.NOTE] Ovaj postupak pregleda samo će vratiti najviše 10 redaka iz tablice.

## <a name="delete-your-clusters"></a>Brisanje vaše klastere

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Da biste istodobno izbrisali klastere, za pohranu i web-aplikacije, izbrišite grupu resursa koji ih sadrži.

## <a name="next-steps"></a>Daljnji koraci

Sada ste naučili kako koristiti oluja pročitati podatke iz koncentratora događaj, spremite ga u HBase i Vizualizirajte podatke na vanjskim nadzorne ploče pomoću Socket.io i D3.js.

* Još primjera od oluja topologija sa servisa HDInsight potražite u članku:

    * [Primjer topologija za oluja na HDInsight](hdinsight-storm-example-topology.md)

* Dodatne informacije o Apache oluja potražite u članku [Apache oluja](https://storm.incubator.apache.org/) web-mjesta.

* Dodatne informacije o HBase na HDInsight potražite u članku [HBase pregled HDInsight](hdinsight-hbase-overview.md).

* Dodatne informacije o Socket.io potražite u članku [socket.io](http://socket.io/) web-mjesta.

* Dodatne informacije o D3.js potražite u članku [D3.js - podataka utemeljenima na dokumentima](http://d3js.org/).

* Informacije o stvaranju topologija u Java, potražite u članku [razviti Java topologija za Apache oluja na HDInsight](hdinsight-storm-develop-java-topology.md).

* Informacije o stvaranju topologija u .NET potražite u članku [razviti C# topologija za Apache oluja na HDInsight pomoću Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

[azure-portal]: https://portal.azure.com
