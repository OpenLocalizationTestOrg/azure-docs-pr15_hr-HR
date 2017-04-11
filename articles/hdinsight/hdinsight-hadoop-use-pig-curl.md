<properties
   pageTitle="Korištenje Hadoop Svinja s zakretanja u HDInsight | Microsoft Azure"
   description="Saznajte kako koristiti zakretanja pokrenuti poslovi latinica Svinja Hadoop klaster u Azure HDInsight."
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
   ms.date="08/23/2016"
   ms.author="larryfr"/>

#<a name="run-pig-jobs-with-hadoop-on-hdinsight-by-using-curl"></a>Pokrenite poslove Svinja s Hadoop na HDInsight pomoću zakretanja

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

U ovom dokumentu će Saznajte kako koristiti zakretanja pokrenuti latinica Svinja zadatke programa klaster Azure HDInsight. Svinja latinica programski jezik omogućuje opisuju transformacije koja se primjenjuju na ulaznih podataka da bi proizveo željeni izlazni rezultat.

Da bismo pokazali kako možete raditi s HDInsight pomoću neobrađenog HTTP zahtjeva za pokretanje, praćenje i dohvaćanje rezultata Svinja poslove koristi se zakretanja. To funkcionira pomoću WebHCat REST API-JA (prijašnjeg Templeton) koju je dodijelio svoj klaster HDInsight.

> [AZURE.NOTE] Ako su već poznato korištenje sustavom Linux Hadoop poslužiteljima, ali se novi korisnik servisa HDInsight, potražite u članku [Savjeti za HDInsight sustavom Linux](hdinsight-hadoop-linux-information.md).

##<a id="prereq"></a>Preduvjeti

Da biste dovršili korake u ovom članku, morate sljedeće:

* Klaster Azure HDInsight (Hadoop na HDInsight) (sustavom Linux ili utemeljen na sustavu Windows)

* [Zakretanja](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

##<a id="curl"></a>Pokrenite Svinja zadatke pomoću zakretanja

> [AZURE.NOTE] Kada koristite zakretanja ili OSTALE komunikacije s WebHCat, morate autentičnost zahtjeve unosom administrator korisničko ime i lozinku za klaster HDInsight. Naziv klaster morate koristiti i kao dio sustava na Uniform Resource Identifier (URI) koja se koristi za zahtjeve poslati na poslužitelj.
>
> Da biste naredbe u ovom odjeljku, zamijenite **korisničko ime** korisnika za provjeru autentičnosti klaster, i zamijenite **lozinku** lozinke za korisnički račun. Zamijenite **CLUSTERNAME** naziv svoj klaster.
>
> REST API-JA je osigurani putem [provjere autentičnosti osnovni programa access](http://en.wikipedia.org/wiki/Basic_access_authentication). Uvijek trebali napraviti zahtjeva putem sigurne HTTP (HTTPS) da biste bili sigurni da vjerodajnica za sigurno šalju na poslužitelj.

1. Iz naredbenog retka, koristite sljedeću naredbu da biste provjerili možete li se povezati s svoj klaster HDInsight:

        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status

    Trebali biste dobiti odgovor sličnu ovoj:

        {"status":"ok","version":"v1"}

    Su parametri koji se koristi u tu naredbu na sljedeći način:

    * **-u**: korisničko ime i lozinku koje se koriste za provjeru autentičnosti zahtjev
    * **-G**: označava da je zahtjevom GET

    Početak URL **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**bit će na isti način za sve zahtjeve za. Put **/status**, znači da je zahtjev da biste se vratili status WebHCat (poznat i kao Templeton) poslužitelja.

2. Da biste poslali latinica Svinja zadatak za klaster, koristite sljedeći kod:

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="LOGS=LOAD+'wasbs:///example/data/sample.log';LEVELS=foreach+LOGS+generate+REGEX_EXTRACT($0,'(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)',1)+as+LOGLEVEL;FILTEREDLEVELS=FILTER+LEVELS+by+LOGLEVEL+is+not+null;GROUPEDLEVELS=GROUP+FILTEREDLEVELS+by+LOGLEVEL;FREQUENCIES=foreach+GROUPEDLEVELS+generate+group+as+LOGLEVEL,COUNT(FILTEREDLEVELS.LOGLEVEL)+as+count;RESULT=order+FREQUENCIES+by+COUNT+desc;DUMP+RESULT;" -d statusdir="wasbs:///example/pigcurl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/pig

    Su parametri koji se koristi u tu naredbu na sljedeći način:

    * **-d**: jer `-G` ne koristi, zahtjev zadane postavke na način objavu. `-d`određuje vrijednosti podataka koje se šalju sa zahtjevom za.

        * **User.name**: korisnika koji se izvodi naredbu
        * **izvršavanje**: U Svinja latinica naredbe za izvođenje
        * **statusdir**: direktorij koji će biti zapisane stanje za taj zadatak

    > [AZURE.NOTE] Uočite da se zamijene razmake koje sadrži naredbe latinica Svinja s `+` znaka kada se koristi s zakretanja.

    Ta se naredba mora vratiti ID zadatka koje je moguće koristiti da biste provjerili status posla, na primjer:

        {"id":"job_1415651640909_0026"}

3. Da biste provjerili status posla, koristite sljedeću naredbu. Zamijenite **JOBID** vrijednost vraća u prethodnom koraku. Na primjer, ako je vrijednost `{"id":"job_1415651640909_0026"}`, tada će biti **JOBID** `job_1415651640909_0026`.

        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state

    Ako je zadatak završio, stanje bit će **je uspio**.

    > [AZURE.NOTE] Zahtjev za zakretanja vraća JavaScript objekt notaciju (JSON) dokument s informacijama o posla, a jq se koristi za preuzimanje samo vrijednost stanja.

##<a id="results"></a>Prikaz rezultata

Kada je stanje posla promijenio se da **je uspjelo**, možete dohvatiti rezultate posla iz spremište blobova platforme Azure. Na `statusdir` parametar proslijeđen s upitom sadrži lokaciju Izlazna datoteka; u ovom slučaju **wasbs: / / / primjer/pigcurl**. Tu adresu pohranjuje izlaz posla u direktoriju **primjer/pigcurl** u željeni spremnik zadani prostor za pohranu koriste svoj klaster HDInsight.

Možete popisa i Preuzmi Preuzmi datoteke pomoću [Azure EŽA](../xplat-cli-install.md). Na primjer, popis datoteka u **primjeru/pigcurl**, koristite sljedeću naredbu:

    azure storage blob list <container-name> example/pigcurl

Da biste preuzeli datoteke, koristite sljedeće:

    azure storage blob download <container-name> <blob-name> <destination-file>

> [AZURE.NOTE] Morate navesti naziv računa za pohranu koji sadrži blob-om pomoću na `-a` i `-k` parametara ili postavljanje na **AZURE\_prostora za POHRANU\_RAČUN** i **AZURE\_prostora za POHRANU\_PRISTUP\_KLJUČ** varijable okruženja.

##<a id="summary"></a>Sažetak

Kao što je prikazano u ovom dokumentu, možete koristiti neobrađenog HTTP zahtjev za pokretanje, praćenje i prikaz rezultata Svinja zadataka na svoj klaster HDInsight.

Dodatne informacije o OSTALE sučelja koji se koristi u ovom članku potražite u članku [Referenca WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).

##<a id="nextsteps"></a>Daljnji koraci

Općenite informacije o Svinja na HDInsight:

* [Korištenje Svinja s Hadoop na HDInsight](hdinsight-use-pig.md)

Informacije o drugim načinima možete raditi s Hadoop na HDInsight:

* [Korištenje grozd s Hadoop na HDInsight](hdinsight-use-hive.md)

* [Korištenje MapReduce s Hadoop na HDInsight](hdinsight-use-mapreduce.md)
