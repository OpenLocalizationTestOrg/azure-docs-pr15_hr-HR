<properties
   pageTitle="Nadzor i upravljanje klastere HDInsight pomoću Apache Ambari REST API-JA | Microsoft Azure"
   description="Saznajte kako koristiti Ambari za nadzor i upravljanje sustavom Linux HDInsight klastere. U ovom dokumentu će Saznajte kako koristiti Ambari REST API-JA uključene klastere HDInsight."
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
   ms.date="09/20/2016"
   ms.author="larryfr"/>

#<a name="manage-hdinsight-clusters-by-using-the-ambari-rest-api"></a>Upravljanje klastere HDInsight pomoću Ambari REST API-JA

[AZURE.INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

Apache Ambari pojednostavnjuje upravljanje i nadzor klastere Hadoop unosom lako da biste koristili web korisničkog Sučelja i REST API-JA. Ambari nalazi se na klastere sustavom Linux HDInsight i služi za praćenje klaster i unijeli promjene konfiguracije. U ovom dokumentu, Informirajte se o osnovama rad s Ambari REST API-JA izvođenjem uobičajene zadatke pomoću zakretanja.

##<a name="prerequisites"></a>Preduvjeti

* [cURL](http://curl.haxx.se/): zakretanja je utility više platformi koje je moguće koristiti za rad s REST API-ji iz naredbenog retka. U ovom dokumentu se koristi za komunikaciju s Ambari REST API-JA.
* [jq](https://stedolan.github.io/jq/): jq je više platformi uslužni program naredbenog retka za rad s dokumentima JSON. U ovom dokumentu, koristi se za raščlaniti JSON dokumenata vratio Ambari REST API-JA.
* [Azure EŽA](../xplat-cli-install.md): programa naredbenog retka za različite platforme za rad sa servisa Azure.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)] 

##<a id="whatis"></a>Što je Ambari?

[Apache Ambari](http://ambari.apache.org) zahvaljujući Hadoop upravljanja jednostavniji omogućuje jednostavno se koristi web korisničkog Sučelja koje je moguće koristiti za dodjelu resursa, upravljanje i praćenje klastere Hadoop. Razvojni inženjeri možete integrirati te mogućnosti u svojim aplikacijama pomoću [Ambari REST API -ji](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

Ambari navedeni su po zadanom sa sustavom Linux HDInsight klastere.

##<a name="rest-api"></a>REST API-JA

Osnovni URI za Ambari REST API-JA na HDInsight je https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME, pri čemu je __CLUSTERNAME__ svoj klaster.

> [AZURE.IMPORTANT] Dok je naziv klaster u dijelu domene potpuno kvalificiran naziv (FQDN) URI (CLUSTERNAME.azurehdinsight.net) velika i mala slova, drugih instanci u URI su velika i mala slova. Ako, na primjer, ako svoj klaster zove MyCluster, ovo su valjane ji:
>
> `https://mycluster.azurehdinsight.net/api/v1/clusters/MyCluster`
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/MyCluster`
>
> Sljedeće ji vratiti pogrešku jer pojavu drugi naziv nije ispravan slučaj.
>
> `https://mycluster.azurehdinsight.net/api/v1/clusters/mycluster`
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/mycluster`

Povezivanje s Ambari na HDInsight zahtijeva HTTPS. Provjera autentičnosti veze, morate koristiti naziv računa za administratore (zadano je __administrator__) i lozinku koje ste naveli prilikom stvaranja klaster.

Sljedeći primjer koristi zakretanja da bi zahtjevom GET protiv REST API-JA. Zamijenite administratorsku lozinku za klaster __lozinku__ . Zamijenite __CLUSTERNAME__ naziv skupine:

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME"
    
Odgovor je JSON dokument koji započinje s informacijama sličnu ovoj:

    {
    "href" : "http://10.0.0.10:8080/api/v1/clusters/CLUSTERNAME",
    "Clusters" : {
        "cluster_id" : 2,
        "cluster_name" : "CLUSTERNAME",
        "health_report" : {
        "Host/stale_config" : 0,
        "Host/maintenance_state" : 0,
        "Host/host_state/HEALTHY" : 7,
        "Host/host_state/UNHEALTHY" : 0,
        "Host/host_state/HEARTBEAT_LOST" : 0,
        "Host/host_state/INIT" : 0,
        "Host/host_status/HEALTHY" : 7,
        "Host/host_status/UNHEALTHY" : 0,
        "Host/host_status/UNKNOWN" : 0,
        "Host/host_status/ALERT" : 0

Budući da je to JSON, lakše je korištenje JSON analizatora za rad s podacima. Na primjer, u sljedećem primjeru pomoću jq da biste prikazali samo na `health_report` element.

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME" | jq '.Clusters.health_report'

##<a name="example-get-the-fqdn-of-cluster-nodes"></a>Primjer: Dohvaćanje FQDN čvorove klaster

Kada radite s HDInsight, možda ćete morati informacije na potpuno kvalificirani naziv domene (FQDN) klaster čvora. Možete jednostavno dohvatiti FQDN za različite čvorove u skupini pomoću sljedeće:

* __Čvorovi glave__:`curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/NAMENODE" | jq '.host_components[].HostRoles.host_name'`
* __Čvorovi tempiranja__:`curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/DATANODE" | jq '.host_components[].HostRoles.host_name'`
* __Čvorovi zookeeper__:`curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" | jq '.host_components[].HostRoles.host_name'`

Napomena svaki od sljedećih primjera slijede isti uzorak:

1. Upit komponente koje ćemo znati pokreće te čvorove.
2. Dohvaćanje na `host_name` elemenata koji sadrže FQDN za ove čvorove.

Na `host_components` element povrata dokument sadrži više stavki. Korištenje `.host_components[]`, i određivanje put unutar elementa će prolazi kroz svaku stavku izvući vrijednost iz određene put. Ako želite samo jednu vrijednost, kao što su prvu stavku FQDN možete vratiti stavke kao zbirka, a zatim odaberite određenu stavku:

    jq '[.host_components[].HostRoles.host_name][0]'

To vraća prvi FQDN iz zbirke.

##<a name="example-get-the-default-storage-account-and-container"></a>Primjer: Traženje zadanog računa za pohranu i spremnik

Kada stvorite programa HDInsight klaster, morate koristiti račun za Azure prostora za pohranu i spremniku blob kao zadani prostor za pohranu za klaster. Ambari možete koristiti za dohvaćanje te podatke nakon stvaranja klaster. Ako, na primjer, ako želite programski izravno u spremnik zapisivanje podataka.

Sljedeće će dohvatiti URI WASB zadani pohrane klastere:

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'
    
> [AZURE.NOTE] To vraća prvi konfiguraciju primjenjuje se na poslužitelju (`service_config_version=1`,) koja sadrži te podatke. Ako dohvatite vrijednost koja je izmijenjena nakon stvaranja klaster, možda ćete morati popis konfiguriranje verzije i dohvaćanje najnovijih jedan.

To vraća vrijednost slično sljedećem primjeru, pri čemu je __SPREMNIK__ spremnik zadani, a __ACCOUNTNAME__ naziv računa spremišta Azure:

    wasbs://CONTAINER@ACCOUNTNAME.blob.core.windows.net

Ove informacije možete koristiti s [Azure EŽA](../xplat-cli-install.md) prijenos ili preuzimanje podataka iz spremnik.

1. Pronađite grupu resursa za pohranu račun. Zamijenite __ACCOUNTNAME__ naziv računa spremišta dohvaćen iz Ambari:

        azure storage account list --json | jq '.[] | select(.name=="ACCOUNTNAME").resourceGroup'
    
    Ovo prikazuje naziv grupe resursa za račun.
    
    > [AZURE.NOTE] Ako se ništa ne je vratio sljedeću naredbu, ćete morati promijeniti EŽA Azure u način upravljanja resursima Azure i ponovno pokrenite naredbu. Da biste prešli u način upravljanja resursima Azure, koristite sljedeću naredbu:
    >
    > `azure config mode arm`
    
2. Ključ za račun za pohranu. Grupa resursa u prethodnom koraku zamijenite __naziv GRUPE__ . Zamijenite __ACCOUNTNAME__ naziv računa spremišta:

        azure storage account keys list -g GROUPNAME ACCOUNTNAME --json | jq '.storageAccountKeys.key1'

    U ovom primjeru rezultat je primarni ključ za račun.
    
3. Korištenje naredbe Prenesi za spremanje datoteke u spremniku:

        azure storage blob upload -a ACCOUNTNAME -k ACCOUNTKEY -f FILEPATH --container __CONTAINER__ -b BLOBPATH
        
    Zamijenite __ACCOUNTNAME__ naziv računa za pohranu. Zamijenite __ACCOUNTKEY__ tipku prethodno dohvatiti. __FILEPATH__ je put do datoteke koju želite prenijeti, dok je __BLOBPATH__ put u spremniku.

    Na primjer, ako želite da se pojavljuju u HDInsight na wasbs://example/data/filename.txt datoteka, zatim __BLOBPATH__ bio `example/data/filename.txt`.

##<a name="example-update-ambari-configuration"></a>Primjer: Ažuriranje Ambari konfiguracija

1. Trenutna konfiguracija koji pohranjuje Ambari kao "željena konfiguracija" pojavljuje se:

        curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME?fields=Clusters/desired_configs"
        
    U ovom se primjeru vraća JSON dokument koji sadrži trenutnu konfiguraciju (otkrije vrijednost _oznake_ ) za komponente instalirano klaster. Sljedeći primjer je u isječak iz vraćenih iz Spark klaster vrste podataka.
    
        "spark-metrics-properties" : {
            "tag" : "INITIAL",
            "user" : "admin",
            "version" : 1
        },
        "spark-thrift-fairscheduler" : {
            "tag" : "INITIAL",
            "user" : "admin",
            "version" : 1
        },
        "spark-thrift-sparkconf" : {
            "tag" : "INITIAL",
            "user" : "admin",
            "version" : 1
        }

    S tog popisa, morate kopirati naziv komponente (na primjer, __spark\_thrift\_sparkconf__ i vrijednost __oznake__ .
    
2. Dohvaćanje konfiguraciju za komponentu i oznaka pomoću sljedeće naredbe. Zamijenite __spark thrift sparkconf__ i __POČETNOG__ komponente i oznake koje želite dohvatiti konfiguraciju za.

        curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations?type=spark-thrift-sparkconf&tag=INITIAL" | jq --arg newtag $(echo version$(date +%s%N)) '.items[] | del(.href, .version, .Config) | .tag |= $newtag | {"Clusters": {"desired_config": .}}' > newconfig.json
    
    Zakretanja dohvaća JSON dokument, a zatim jq štiti izmjene s podacima za stvaranje predloška. Predložak koristi se za Dodaj/izmijeni konfiguracijskih vrijednosti. Konkretno čini sljedeće:
    
    * Stvara jedinstvene vrijednosti koje sadrže niz "verzija" i datum koji je pohranjen u __newtag__.
    * Stvara korijen dokumenta za nove željena konfiguracija.
    * Dohvaća sadržaj u `.items[]` polja i dodaje je u odjeljku __desired_config__ element.
    * Brisanje __href__, __verzije__i elemente __Config__ nisu po potrebi tih elemenata da biste poslali novu konfiguraciju.
    * Dodaje novi element __oznake__ i postavlja vrijednost na __verziju ###__. Brojčani dio temelji se na trenutni datum. Svaki konfiguracije mora imati jedinstveni oznake.
    
    Na kraju, podaci se spremaju u dokument __newconfig.json__ . Struktura dokumenta trebao izgledati slično kao u sljedećem primjeru:
    
        {
            "Clusters": {
                "desired_config": {
                "tag": "version1459260185774265400",
                "type": "spark-thrift-sparkconf",
                "properties": {
                    ....
                 },
                 "properties_attributes": {
                     ....
                 }
            }
        }

3. Otvorite __newconfig.json__ dokument i izmjena/dodavanje vrijednosti __Svojstva__ objekta. Sljedeći primjer mijenja vrijednost __"spark.yarn.am.memory"__ od __"1 g"__ u __"3 g"__ i, te dodaje novi element za __"spark.kryoserializer.buffer.max"__ s vrijednošću __"256 m"__.

        "spark.yarn.am.memory": "3g",
        "spark.kyroserializer.buffer.max": "256m",

    Kada završite s unosom izmjene, spremite datoteku.

4. Koristite sljedeću naredbu da biste poslali ažurirane konfiguracija da Ambari.

        cat newconfig.json | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME"
        
    Ta se naredba cijevi sadržaj datoteke __newconfig.json__ u zahtjev za zakretanja koja se šalje klaster kao novi željena konfiguracija. Zahtjev za zakretanja vraća JSON dokumenta. Element __versionTag__ u ovom dokumentu moraju se podudarati verzije koje ste poslali i objekt __configs__ će sadržavati zatražene promjene konfiguracije.

###<a name="example-restart-a-service-component"></a>Primjer: Ponovno pokrenite komponenta za servis

Sada ako pogledate web Ambari korisničko Sučelje servisa Spark označava je potrebno je ponovno pokrenuti prije Nova konfiguracija stupile na snagu. Poduzmite sljedeće korake da biste ponovno pokrenite servis.

1. Da biste omogućili održavanje servisa za Spark, koristite sljedeće:

        echo '{"RequestInfo": {"context": "turning on maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"ON"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"

    Ta se naredba šalje JSON dokument poslužitelj (koje se nalaze u na `echo` naredbe,) koji uključuje održavanja.
    Možete provjeriti je li servis sada u načinu održavanja pomoću zahtjev za sljedeće:
    
        curl -u admin:PASSWORD -H "X-Requested-By: ambari" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK" | jq .ServiceInfo.maintenance_state
        
    To će vratiti vrijednost `"ON"`.

3. Nakon toga koristite sljedeće da biste isključili usluge:

        echo '{"RequestInfo": {"context" :"Stopping the Spark service"}, "Body": {"ServiceInfo": {"state": "INSTALLED"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"
        
    Ta naredba Vrati odgovor otprilike ovako.
    
        {
            "href" : "http://10.0.0.18:8080/api/v1/clusters/CLUSTERNAME/requests/29",
            "Requests" : {
                "id" : 29,
                "status" : "Accepted"
            }
        }
    
    Na `href` vrijednost koju vraća ovu URI koristi interne IP adresu čvora klaster. Da biste ga koristili izvan klaster, zamijenite dio "10.0.0.18:8080" FQDN klaster. Ako, na primjer, sljedeća naredba dohvaća status zahtjeva.
    
        curl -u admin:PASSWORD -H "X-Requested-By: ambari" "https://CLUSTERNAME/api/v1/clusters/CLUSTERNAME/requests/29" | jq .Requests.request_status
    
    Ako je to vraća vrijednost `"COMPLETED"` , a zatim Završi zahtjev.

4. Kada se dovrši prethodni zahtjev, koristite sljedeće pokrenuti servis.

        echo '{"RequestInfo": {"context" :"Restarting the Spark service"}, "Body": {"ServiceInfo": {"state": "STARTED"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"

    Nakon ponovnog pokretanja servis je novih postavki konfiguracije.

5. Da biste isključili način održavanja na kraju, koristite sljedeće.

        echo '{"RequestInfo": {"context": "turning off maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"OFF"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"

##<a name="next-steps"></a>Daljnji koraci

Dovršavanje referenca REST API-JA, potražite u članku [Ambari API Reference V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

> [AZURE.NOTE] Neke funkcije Ambari je onemogućen, kao što je upravlja HDInsight servisa u oblaku; na primjer, dodavanjem ili uklanjanjem domaćini iz skupine ili dodate nove servise.
