<properties
    pageTitle="Stvaranje Hadoop, HBase ili oluja klastere na Linux u HDInsight pomoću EŽA za različite platforme Azure | Microsoft Azure"
    description="Saznajte kako stvoriti klastere sustavom Linux HDInsight pomoću Azure EŽA različite platforme, predlošci Voditelj resursa Azure i Azure REST API-JA. Određivanje vrste klaster (Hadoop, HBase ili oluja) ili koristite skripte da biste instalirali prilagođene komponente..."
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

#<a name="create-linux-based-clusters-in-hdinsight-using-the-azure-cli"></a>Stvaranje sustavom Linux klastere u HDInsight pomoću EŽA Azure

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Azure EŽA je programa naredbenog retka različite platforme koja omogućuje upravljanje uslugama za Azure. Korištenja, uz predloške upravljanje Azure resursa, da biste stvorili HDInsight klaster, zajedno s računa pridruženog prostora za pohranu i ostale servise.

Azure Upravljanje resursima Predlošci su JSON dokumente koje opisuju __grupu resursa__ i sve resursa u njemu (primjerice HDInsight.) Takvog utemeljen predložak omogućuje definiranje resursa koje su vam potrebne za HDInsight u jedan predložak. To omogućuje vam upravljanje promjene u grupu cijela kroz __implementacije__, a primijenit će se promjene u čitavu grupu.

Koraci u ovom dokumentu voditi kroz postupak stvaranja novog klaster HDInsight pomoću EŽA Azure i predložak.

> [AZURE.IMPORTANT] Koraci u ovom dokumentu pomoću zadanog broja radnih čvorove (4) za programa klaster HDInsight. Ako planirate više od 32 tempiranja čvorove (prilikom stvaranja klaster ili skaliranje klaster), morate odabrati glavni čvor veličina s najmanje 8 jezgri i 14 GB RAM-a.
>
> Dodatne informacije o čvor veličine i pridruženi troškova potražite u članku [HDInsight cijene](https://azure.microsoft.com/pricing/details/hdinsight/).

##<a name="prerequisites"></a>Preduvjeti

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

- **Mogući Azure pretplate**. Pogledajte [Početak Azure besplatnu probnu verziju](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- __Azure EŽA__. Koraci u ovom dokumentu su zadnje testirati s Azure EŽA verzija 0.10.1.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)] 


### <a name="access-control-requirements"></a>Preduvjeti za kontrolu pristupa

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

##<a name="log-in-to-your-azure-subscription"></a>Prijavite se u pretplatu za Azure

Slijedite korake u [pretplatu na Azure s Azure sučelja naredbenog retka (Azure EŽA) za povezivanje](../xplat-cli-connect.md) i povezati se vaša pretplata na način __prijave__ .

##<a name="create-a-cluster"></a>Stvaranje klaster

Nakon instaliranja i konfiguriranja EŽA Azure mora provesti sljedeće korake iz naredbenog retka, ljuske ili terminal sesiju.

1. Za provjeru autentičnosti u pretplatu za Azure, koristite sljedeću naredbu:

        azure login

    Morat ćete unijeti ime i lozinku. Ako imate više pretplata Azure, pomoću `azure account set <subscriptionname>` da biste postavili pretplatu u koju koristite naredbe za Azure EŽA.

3. Prebacite se u način upravljanja resursima Azure pomoću sljedeće naredbe:

        azure config mode arm

4. Stvorite grupu resursa. Ovu grupu resursa će sadržavati HDInsight klaster te pridružene račun za pohranu.

        azure group create groupname location
        
    * Zamijenite jedinstveni naziv za grupu __naziv grupe__ . 
    * Zamijenite __mjesto__ regiji koju želite stvoriti grupu. 
    
        Da biste postigli popis valjana mjesta, koristite na `azure location list` naredbu, a zatim pomoću mjesta iz __naziv__ stupca.

5. Stvaranje računa za pohranu. Taj račun za pohranu će se koristiti kao zadani prostor za pohranu za klaster HDInsight.

        azure storage account create -g groupname --sku-name RAGRS -l location --kind Storage storagename
        
     * Naziv grupe stvorili u prethodnom koraku zamijenite __naziv grupe__ .
     * Zamijenite __mjesto__ na isto mjesto koje se koriste u prethodnom koraku. 
     * Zamijenite __storagename__ jedinstveni naziv računa za pohranu.
     
     > [AZURE.NOTE] Dodatne informacije o parametri koji se koriste u toj naredbi `azure storage account create -h` da biste vidjeli pomoć za tu naredbu.

5. Dohvaćanje ključa koji se koristi za pristup računu za pohranu.

        azure storage account keys list -g groupname storagename
        
    * __Naziv grupe__ zamijenite naziv grupe resursa.
    * Zamijenite __storagename__ naziv računa za pohranu.
    
    U podataka koje se vraćaju spremiti vrijednost __ključa__ za __ključ1__.

6. Stvaranje programa klaster HDInsight.

        azure hdinsight cluster create -g groupname -l location -y Linux --clusterType Hadoop --defaultStorageAccountName storagename.blob.core.windows.net --defaultStorageAccountKey storagekey --defaultStorageContainer clustername --workerNodeCount 2 --userName admin --password httppassword --sshUserName sshuser --sshPassword sshuserpassword clustername

    * __Naziv grupe__ zamijenite naziv grupe resursa.

    * Zamijenite __Hadoop__ vrstu klaster koju želite stvoriti. Na primjer, `Hadoop`, `HBase`, `Storm` ili `Spark`.

        > [AZURE.IMPORTANT] HDInsight klastere Dođite na razne vrste koji odgovara radno opterećenje ni tehnologija koja klaster postavljen je za. Ne postoji podržani način da biste stvorili klaster koji kombinira više vrsta, kao što su oluja i HBase na jedan klaster. 

    * Zamijenite __mjesto__ na isto mjesto koje se koriste u prethodnim koracima.

    * Zamijenite __storagename__ naziv računa za pohranu.

    * Zamijenite __ključ spremišta__ tipku nabavili u prethodnom koraku. 

    * Za na `--defaultStorageContainer` parametar, koristite isti naziv kao i koje koristite za klaster.

    * Zamijenite __administrator__ i __httppassword__ ime i lozinku koju želite koristiti prilikom pristupanja klaster putem HTTP.

    * Zamijenite __sshuser__ i __sshuserpassword__ korisničko ime i lozinku koju želite koristiti prilikom pristupanja klaster pomoću SSH

    Može potrajati nekoliko minuta za postupak stvaranja klaster da biste završili. Obično oko 15.

##<a name="next-steps"></a>Daljnji koraci

Sad kad ste stvorili uspješno je klaster HDInsight pomoću EŽA Azure, koristite sljedeće da biste saznali kako raditi s svoj klaster:

###<a name="hadoop-clusters"></a>Hadoop klastere

* [Korištenje grozd s HDInsight](hdinsight-use-hive.md)
* [Korištenje Svinja s HDInsight](hdinsight-use-pig.md)
* [Korištenje MapReduce s HDInsight](hdinsight-use-mapreduce.md)

###<a name="hbase-clusters"></a>HBase klastere

* [Početak rada s HBase na HDInsight](hdinsight-hbase-tutorial-get-started-linux.md)
* [Razvoj aplikacija Java HBase na HDInsight](hdinsight-hbase-build-java-maven-linux.md)

###<a name="storm-clusters"></a>Klastere oluja

* [Razvoj Java topologija za oluja na HDInsight](hdinsight-storm-develop-java-topology.md)
* [Korištenje komponente Python oluja na HDInsight](hdinsight-storm-develop-python-topology.md)
* [Implementacija i nadzirati topologija s oluja na HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)
