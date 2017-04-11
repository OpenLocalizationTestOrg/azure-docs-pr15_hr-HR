<properties
   pageTitle="Savjeti za korištenje Hadoop na sustavom Linux HDInsight | Microsoft Azure"
   description="Implementacija savjete za korištenje sustavom Linux HDInsight (Hadoop) klastere poznatog okruženja Linux izvodi u Azure oblaka."
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
   ms.date="09/13/2016"
   ms.author="larryfr"/>

# <a name="information-about-using-hdinsight-on-linux"></a>Informacije o korištenju servisa HDInsight na Linux

Sustavom Linux klastere Azure HDInsight navedite Hadoop na poznatih Linux okruženju izvodi u Azure oblaka. Za većinu zadataka trebalo bi funkcionirati točno onako kao Hadoop na Linux instalacije. Ovaj dokument pozive izvan određenog razlike koje ste trebali imati na umu.

##<a name="prerequisites"></a>Preduvjeti

U mnogim koraka u ovom dokumentu koriste sljedeće uslužni programi, koji mogu moraju biti instalirani na računalu.

* [cURL](https://curl.haxx.se/) - koristi za komunikaciju s koji se temelji na web usluge
* [jq](https://stedolan.github.io/jq/) - koristi raščlaniti JSON dokumenata
* [Azure EŽA](../xplat-cli-install.md) - za daljinsko upravljanje uslugama za Azure

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell-and-cli.md)] 

## <a name="domain-names"></a>Nazivi domena

Na potpuno kvalificirani naziv domene (FQDN) da biste koristili prilikom povezivanja s Internetom klaster je ** &lt;clustername >. azurehdinsight.net** ili (u slučaju samo SSH) ** &lt;clustername-ssh >. azurehdinsight.net**.

Interno, svaki čvor u klasteru ima naziv koji je dodijeljen tijekom klaster konfiguracije. Da biste pronašli nazive klaster, posjetite stranicu __domaćini__ na korisničko Sučelje Web Ambari ili koristite sljedeće da biste prikazali popis domaćini iz Ambari REST API-JA:

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/hosts" | jq '.items[].Hosts.host_name'

Zamijenite lozinku računa administratora i __CLUSTERNAME__ pod nazivom svoj klaster __lozinku__ . To će vratiti JSON dokument koji sadrži popis domaćini u klasteru, a zatim jq povlači na `host_name` element vrijednost za svaku glavno računalo u klasteru.

Ako vam je potrebna da biste pronašli naziv čvor određenog servisa, možete upita Ambari za tu komponentu. Na primjer, da biste pronašli hosts čvor naziva HDFS, koristite sljedeće.

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/NAMENODE" | jq '.host_components[].HostRoles.host_name'

To vraća JSON dokumenta s opisom usluge i jq povlači se samo na `host_name` vrijednost za hosts.

## <a name="remote-access-to-services"></a>Daljinski pristup servisima

* **Ambari (web)** – https://&lt;clustername >. azurehdinsight.net

    Autentičnost pomoću klaster administratora korisnika i lozinku i prijavite se u sustav Ambari. Time se i koristi klaster administratora korisnika i lozinku.

    Provjera autentičnosti je običan tekst – uvijek koristi HTTPS kako biste bili sigurni da je sigurna veza.

    > [AZURE.IMPORTANT] Dok Ambari za svoj Klaster se može pristupiti izravno putem Interneta, funkcionalnosti ovisi pristupa čvorove prema nazivu interne domene koriste klaster. Budući da je to interne domene naziv, a ne javno, primit ćete "poslužitelj nije pronađen" pogreške pri pokušaju pristupiti neke značajke putem Interneta.
    >
    > Da biste koristili punu funkcionalnost telefona Ambari web-mjesta korisničkog Sučelja, pomoću programa tunelom SSH da biste promet web proxy čvor glavni klaster. Potražite u članku [Korištenje SSH tuneliranje da biste pristupili web Ambari korisničkog Sučelja, ResourceManager, JobHistory, NameNode, Oozie, i druge web-mjesta korisničkog Sučelja](hdinsight-linux-ambari-ssh-tunnel.md)

* **Ambari (REST)** – https://&lt;clustername >.azurehdinsight.net/ambari

    > [AZURE.NOTE] Autentičnost pomoću klaster administratora korisnika i lozinke.
    >
    > Provjera autentičnosti je običan tekst – uvijek koristi HTTPS kako biste bili sigurni da je sigurna veza.

* **WebHCat (Templeton)** – https://&lt;clustername >.azurehdinsight.net/templeton

    > [AZURE.NOTE] Autentičnost pomoću klaster administratora korisnika i lozinke.
    >
    > Provjera autentičnosti je običan tekst – uvijek koristi HTTPS kako biste bili sigurni da je sigurna veza.

* **SSH** - &lt;clustername >-ssh.azurehdinsight.net na priključak 22 ili 23. Povezivanje s primarni headnode 23 koristi se za povezivanje sa sekundarnom koristi se priključak 22. Dodatne informacije o glavni čvorove potražite u članku [dostupnosti i pouzdanost klastere Hadoop u HDInsight](hdinsight-high-availability-linux.md).

    > [AZURE.NOTE] Čvorovi glavni klaster samo možete pristupiti putem SSH na klijentskom računalu. Nakon uspostave čvorove tempiranja pa možete pristupiti pomoću SSH iz programa headnode.

## <a name="file-locations"></a>Mjesta datoteka

Datoteke vezane uz Hadoop možete pronaći na čvorove klaster pri `/usr/hdp`. Taj direktorij sadrži sljedeće direktorijima:

* __2.2.4.9-1__: taj imenik pod nazivom verzije platforme podataka Hortonworks koristi HDInsight, tako da se broj na svoj klaster mogu se razlikovati od one koja je navedena u nastavku.
* __trenutni__: taj imenik sadrži veze na direktorija u imenik __2.2.4.9-1__ i postoji tako da ne morate unositi broj verzije (koji se može promijeniti,) svaki put kada želite pristupiti datoteci.

Oglednim podacima i POSUDU datoteke možete pronaći na Hadoop Distributed datoteka sustava (HDFS) ili blobova platforme Azure prostor za pohranu uz ' / primjer ' ili ' wasbs: / / / primjer ".

## <a name="hdfs-azure-blob-storage-and-storage-best-practices"></a>HDFS, spremište blobova platforme Azure i najbolje prakse za pohranu

U većini Hadoop distribucija, HDFS sigurnosno po lokalno spremište na računalima u klasteru. Dok je učinkovitog, možda ćete skup za rješenja u oblaku koje se naplaćuju zaračunava ili minute za računalnim resurse.

HDInsight koristi spremište blobova platforme Azure kao zadanog spremišta, koji pruža sljedeće prednosti:

* Jeftino Dugoročne prostora za pohranu

* Pristupačnost iz vanjske servise kao što je web-mjesta, uslužni programi za prijenos i preuzimanje datoteka, različitih jezika SDK-ovi i web-preglednika

Budući da je zadano spremište za HDInsight, obično ne morate ništa da biste ga koristiti. Na primjer, sljedeća naredba prikazat će se popis datoteka u mapi **/example/data** koja je spremljena na spremište blobova platforme Azure:

    hdfs dfs -ls /example/data

Neke naredbe možda potrebno da biste odredili koriste spremište blobova platforme. Za to, možete prefiks naredbu **wasb: / /**, ili **wasbs: / /**.

HDInsight omogućuje i pridružiti više računa spremišta blobova platforme klaster. Da biste pristupili podataka na račun koji nisu zadani Blob prostora za pohranu, možete koristiti oblik **wasbs: / /&lt;container-name>@&lt;naziv računa >.blob.core.windows.net/**. Na primjer, sljedeće prikazat će se popis sadržaja imenika **/example/data** za navedeni spremnik i računa spremišta blobova platforme:

    hdfs dfs -ls wasbs://mycontainer@mystorage.blob.core.windows.net/example/data

### <a name="what-blob-storage-is-the-cluster-using"></a>Što blobova koristi klaster?

Tijekom stvaranja klaster koje ste odabrali za korištenje postojećeg računa za Azure prostora za pohranu i spremnik ili stvorite novi. Zatim, ako vjerojatno ste izgubili o njemu. Zadani račun za pohranu i spremnik možete pronaći pomoću Ambari REST API-JA.

1. Koristite sljedeću naredbu za dohvaćanje informacija o konfiguraciji HDFS pomoću zakretanja i filtrirati pomoću [jq](https://stedolan.github.io/jq/):

        curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'
    
    > [AZURE.NOTE] To će vratiti prvi konfiguraciju primjenjuje se na poslužitelju (`service_config_version=1`,) koja će sadržavati taj podatak. Ako dohvaćate vrijednost koja je izmijenjena nakon stvaranja klaster, možda ćete morati popis konfiguriranje verzije i dohvaćanje najnovijih jedan.

    To će vratiti vrijednost slično sljedećem, pri čemu je __SPREMNIK__ spremnik zadani, a __ACCOUNTNAME__ naziv računa spremišta Azure:

        wasbs://CONTAINER@ACCOUNTNAME.blob.core.windows.net

1. Dobili grupu resursa za pohranu račun, pomoću [EŽA Azure](../xplat-cli-install.md). U sljedeću naredbu, zamijenite __ACCOUNTNAME__ naziv računa spremišta dohvaćeni iz Ambari:

        azure storage account list --json | jq '.[] | select(.name=="ACCOUNTNAME").resourceGroup'
    
    Vratit će naziv grupe resursa za račun.
    
    > [AZURE.NOTE] Ako se ništa ne je vratio sljedeću naredbu, ćete morati promijeniti EŽA Azure u način upravljanja resursima Azure i ponovno pokrenite naredbu. Da biste prešli u način upravljanja resursima Azure, koristite sljedeću naredbu.
    >
    > `azure config mode arm`
    
2. Ključ za račun za pohranu. Grupa resursa u prethodnom koraku zamijenite __naziv GRUPE__ . Zamijenite __ACCOUNTNAME__ naziv računa spremišta:

        azure storage account keys list -g GROUPNAME ACCOUNTNAME --json | jq '.[0].value'

    Vratit će primarni ključ za račun.

Možete pronaći i pohranu podataka pomoću portala za Azure:

1. [Portal za Azure](https://portal.azure.com/)odaberite svoj klaster HDInsight.

2. U odjeljku __Osnove__ odaberite __sve postavke__.

3. Na stranici __Postavke__odaberite __Tipke Azure prostora za pohranu__.

4. Na __Tipke Azure prostora za pohranu__, odaberite neku od račune za pohranu na popisu. Time će se prikazati podatke o računu za pohranu.

5. Odaberite ikonu ključa. Time će se prikazati tipke za taj račun za pohranu.

### <a name="how-do-i-access-blob-storage"></a>Kako pristupiti blobova?

Osim pomoću naredbe Hadoop iz skupine postoje na različite načine da biste pristupili blob-ova:

* [Azure EŽA za Mac i Linux Windows](../xplat-cli-install.md): sučelje naredbenog retka naredbe za rad s Azure. Nakon instalacije, koristite na `azure storage` naredbu za pomoć za korištenje prostor za pohranu ili `azure blob` za naredbe specifične za blob.

* [blobxfer.PY](https://github.com/Azure/azure-batch-samples/tree/master/Python/Storage): na python skripte za rad s blob-ova u Azure prostora za pohranu.

* Raznih SDK-ovi:

    * [Java](https://github.com/Azure/azure-sdk-for-java)

    * [Node.js](https://github.com/Azure/azure-sdk-for-node)

    * [PHP](https://github.com/Azure/azure-sdk-for-php)

    * [Python](https://github.com/Azure/azure-sdk-for-python)

    * [Ruby](https://github.com/Azure/azure-sdk-for-ruby)

    * [.NET](https://github.com/Azure/azure-sdk-for-net)

* [Prostor za pohranu REST API-JA](https://msdn.microsoft.com/library/azure/dd135733.aspx)

##<a name="scaling"></a>Promjena veličine svoj klaster

Klaster skaliranje značajka omogućuje vam da biste promijenili broj čvorove podataka koristi klaster koja se izvodi u Azure HDInsight bez potrebe za izbrisati, a zatim ponovno stvorite klaster.

Možete izvesti skaliranja radnje tijekom druge zadatke ili procesa su pokrenuti na klaster.

Različite vrste utječe skaliranje na sljedeći način:

* __Hadoop__: kada skaliranja broj čvorove na neki servis u klasteru ponovno pokrenete. To može uzrokovati zadataka radi ili na čekanju uvoza nakon dovršetka postupka skaliranja. Nakon dovršetka postupka možete je ponovno pošaljite poslove.

* __HBase__: regionalne poslužitelji se automatski raspoređen za nekoliko minuta nakon dovršetka postupka skaliranja. Da biste ručno saldo regionalne poslužitelje, poduzmite sljedeće korake:

    1. Povezivanje s klaster HDInsight pomoću SSH. Dodatne informacije o korištenju SSH s HDInsight potražite u nekom od sljedećih dokumenata:

        * [Korištenje SSH s HDInsight Linux, Unix i Mac OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

        * [Korištenje SSH s HDInsight iz sustava Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

    1. Da biste pokrenuli ljuske HBase, koristite sljedeće:

            hbase shell

    2. Kada ljuske HBase učitana, koristite sljedeće da biste ručno saldo regionalnog poslužitelja:

            balancer

* __Oluja__: poduzme sve pokrenute topologija oluja u kad je izvršena skaliranja operacija. Time se omogućuje topologije da biste ponovo prilagodili postavke parallelism na temelju novi broj čvorovi u klasteru. Da biste poduzme izvodi topologija, primijenite jednu od sljedećih mogućnosti:

    * __SSH__: povezivanje s poslužiteljem i koristite sljedeću naredbu za poduzme topologije:

            storm rebalance TOPOLOGYNAME

        Možete odrediti i parametara da biste nadjačali savjeti parallelism izvorno nudi topologije. Na primjer, `storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10` će konfiguracija topologije 5 radnih procesa, 3 executors za komponentu plava spout i 10 executors za komponentu žuta munje.

    * __Korisničko Sučelje oluja__: slijedite ove korake da biste poduzme topologije pomoću oluja korisničkog Sučelja.

        1. Otvorite __https://CLUSTERNAME.azurehdinsight.net/stormui__ u web-preglednik, pri čemu je CLUSTERNAME svoj klaster oluja. Ako se to od vas zatraži, unesite ime administratora (administratora) klaster HDInsight i lozinka koje ste naveli prilikom stvaranja klaster.

        3. Odaberite topologije želite poduzme, a zatim odaberite gumb __poduzme__ . Unesite vrijeme prije nego što se provodi operacija rebalance.

Dodatne informacije o skaliranje svoj klaster HDInsight, pogledajte:

* [Upravljanje Hadoop klastere u HDInsight pomoću portala za Azure](hdinsight-administer-use-portal-linux.md#scaling)

* [Upravljanje Hadoop klastere u HDinsight pomoću ljuske PowerShell za Azure](hdinsight-administer-use-command-line.md#scaling)

## <a name="how-do-i-install-hue-or-other-hadoop-component"></a>Kako instalirati nijanse (ili druge komponente Hadoop)?

HDInsight je upravljani servis, što znači da čvorove klasteru možda uništava i automatski brišu po Azure ako je otkriven problem. Zbog toga ne preporučuje se da biste ručno instalirali stvari izravno na čvorove klaster. Umjesto toga koristite [Akcije skripte HDInsight](hdinsight-hadoop-customize-cluster.md) kada morate instalirati sljedeće:

* Usluga ili web-mjestu kao što su Spark ili nijanse.
* Komponente koje je potrebno promjene konfiguracije na više čvorovi u klasteru. Ako, na primjer, varijabla potrebna okruženja, stvaranje zapisivanje imenika ili stvaranje konfiguracijska datoteka.

Akcije skripte su tulumu skripte koje su pokrenuli tijekom dodjeljivanja klaster, a mogu se instalirati i konfigurirati dodatne komponente na klaster. Primjer skripte služe za instalaciju sljedeće komponente:

* [Nijanse](hdinsight-hadoop-hue-linux.md)
* [Giraph](hdinsight-hadoop-giraph-install-linux.md)
* [Solr](hdinsight-hadoop-solr-install-linux.md)

Informacije o razvoju skripte Akcije, potražite u članku [razvoja skripte akciju s HDInsight](hdinsight-hadoop-script-actions-linux.md).

###<a name="jar-files"></a>Posudu datoteke

Neke Hadoop tehnologije isporučuju samostalnih posudu datoteke koje su sadrže funkcija koje se koriste kao dio posla MapReduce ili iz unutar Svinja ili grozd. Dok se mogu se instalirati pomoću akcije skripte, oni često ne zahtijevaju sve postavljanje i možete samo biti prenijeli skupine nakon dodjele resursa i koristiti izravno. Ako želite li komponenta LIJEKA reimaging od klaster makle posudu datoteku možete spremiti u WASB.

Ako, na primjer, ako želite koristiti najnoviju verziju [DataFu](http://datafu.incubator.apache.org/)možete preuzeti posudu koja sadrži projekta i prijenos klaster HDInsight. Zatim pratite DataFu dokumentaciju o tome kako ga koristiti iz Svinja ili grozd.

> [AZURE.IMPORTANT] Neke komponente koje su datoteke posudu samostalne dostupna je HDInsight, ali se ne nalaze na putu. Ako tražite određenu komponentu prati možete koristiti da biste ga pronašli na svoj klaster:
>
> ```find / -name *componentname*.jar 2>/dev/null```
>
> Vratit će put sve odgovarajuće posudu datoteke.

Ako klaster već sadrži verziju komponente kao samostalnu posudu datoteku, ali želite koristiti neku drugu verziju, možete prenijeti nove verzije komponente klaster i pokušajte koristiti u svoje zadatke.

> [AZURE.WARNING] Komponente dao klaster HDInsight potpuno podržane, a Microsoft Support pomoći će vam da biste izdvojili i rješavanje problema vezanih uz te komponente.
>
> Prilagođene komponente dobili komercijalno pametnije podršku radi daljnje rješavanje problema. To može rezultirati rješavanju problema ili s pitanjem želite li sudjelovati dostupnih kanala tehnologija Otvori izvor gdje se nalazi niže stručna znanja za taj tehnologiju. Na primjer, postoje mnogo web-mjesta zajednice koje je moguće koristiti, npr.: [MSDN forum za HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Projekti Apache imaju web-mjesta projekta na [http://apache.org](http://apache.org), na primjer: [Hadoop](http://hadoop.apache.org/), [Spark](http://spark.apache.org/).

## <a name="next-steps"></a>Daljnji koraci

* [Migrirati iz HDInsight utemeljen na sustavu Windows da biste sustavom Linux](hdinsight-migrate-from-windows-to-linux.md)
* [Korištenje grozd s HDInsight](hdinsight-use-hive.md)
* [Korištenje Svinja s HDInsight](hdinsight-use-pig.md)
* [Korištenje MapReduce poslove s HDInsight](hdinsight-use-mapreduce.md)
