<properties
   pageTitle="Korištenje MapReduce i zakretanja s Hadoop u HDInsight | Microsoft Azure"
   description="Saznajte kako daljinski pokreće MapReduce poslove s Hadoop na HDInsight pomoću zakretanja."
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
   ms.date="09/27/2016"
   ms.author="larryfr"/>

#<a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-curl"></a>Mada MapReduce poslove s Hadoop HDInsight pomoću zakretanja

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

U ovom dokumentu će Saznajte kako koristiti zakretanja pokrenuti poslovi MapReduce Hadoop na klasteru HDInsight.

Da bismo pokazali kako možete raditi s HDInsight pomoću neobrađenog HTTP zahtjeva za pokretanje MapReduce poslove koristi se zakretanja. To funkcionira pomoću WebHCat REST API-JA (prijašnjeg Templeton) dao svoj klaster HDInsight.

> [AZURE.NOTE] Ako ste već upoznati s komponentom sustavom Linux Hadoop poslužiteljima, ali ste novi korisnik servisa HDInsight, potražite u članku [što trebate znati o sustavom Linux Hadoop na HDInsight](hdinsight-hadoop-linux-information.md).

##<a id="prereq"></a>Preduvjeti

Da biste dovršili korake u ovom članku, morate sljedeće:

* Hadoop na HDInsight klaster (Linux ili utemeljen na sustavu Windows)

* [Zakretanja](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

##<a id="curl"></a>Pokrenite MapReduce zadatke pomoću zakretanja

> [AZURE.NOTE] Kada koristite zakretanja ili OSTALE komunikacije s WebHCat, morate provjeriti autentičnost zahtjeve unosom HDInsight klaster administrator korisničko ime i lozinku. Naziv klaster morate koristiti i kao dio URI koji se koristi za zahtjeve poslati na poslužitelj.
>
> Da biste naredbe u ovom odjeljku, zamijenite **korisničko ime** korisnika za provjeru autentičnosti klaster i **Lozinka** pomoću lozinke za korisnički račun. Zamijenite **CLUSTERNAME** naziv svoj klaster.
>
> Pomoću [provjere autentičnosti osnovni pristup](http://en.wikipedia.org/wiki/Basic_access_authentication)je osigurani REST API-JA. Zahtjevi za uvijek trebali napraviti pomoću HTTPS da biste bili sigurni da vjerodajnica za sigurno šalju na poslužitelj.

1. Iz programa naredbenog retka, koristite sljedeću naredbu da biste provjerili možete li se povezati s svoj klaster HDInsight:

        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status

    Trebali biste dobiti odgovor sličnu ovoj:

        {"status":"ok","version":"v1"}

    Su parametri koji se koristi u tu naredbu na sljedeći način:

    * **-u**: označava korisničko ime i lozinku koje se koriste za provjeru autentičnosti zahtjev
    * **-G**: označava da je zahtjevom GET

    Na početku URI **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**jednak je za sve zahtjeve.

2. Da biste poslali MapReduce zadatak, koristite sljedeću naredbu:

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d jar=wasbs:///example/jars/hadoop-mapreduce-examples.jar -d class=wordcount -d arg=wasbs:///example/data/gutenberg/davinci.txt -d arg=wasbs:///example/data/CurlOut https://CLUSTERNAME.azurehdinsight.net/templeton/v1/mapreduce/jar

    Na kraj URI (/ mapreduce/posudu) pokazuje WebHCat zahtjev će pokrenuti posao MapReduce iz klase u datoteci posudu. Su parametri koji se koristi u tu naredbu na sljedeći način:

    * **-d**: `-G` ne koristi zahtjev zadane postavke na način objavu. `-d`određuje vrijednosti podataka koje se šalju sa zahtjevom za.

        * **User.name**: korisnika koji se izvodi naredbu
        * **posudu**: mjesto posudu datoteku koja sadrži razred tako da je pokrenuo
        * **klasa**: klasa koja sadrži logiku MapReduce
        * **argument**: argumentima koji će se proslijediti posao MapReduce; u ovom slučaju, unos tekstnu datoteku i direktorija koji se koriste za izlaz

    Ta se naredba mora vratiti ID zadatka koje je moguće koristiti da biste provjerili status zadatka:

        {"id":"job_1415651640909_0026"}

3. Da biste provjerili status posla, koristite sljedeću naredbu. Zamijenite **JOBID** vrijednost vraća u prethodnom koraku. Na primjer, ako je vrijednost `{"id":"job_1415651640909_0026"}`, a zatim na JOBID bio `job_1415651640909_0026`.

        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state

    Ako je zadatak dovršen, stanje će biti "uspjelo".

    > [AZURE.NOTE] Zahtjev za zakretanja vraća JSON dokument s informacijama o posla; jq se koristi za dohvaćanje samo vrijednost stanja.

4. Kada je stanje posla promijenio se da **je uspjelo**, možete dohvatiti rezultate posla iz spremište blobova platforme Azure. Na `statusdir` parametra koji se s upitom sadrži lokaciju Izlazna datoteka; u ovom slučaju **wasbs: / / / primjer/zakretanja**. Tu adresu pohranjuje izlaz posla u direktoriju **primjer/zakretanja** u željeni spremnik zadani prostor za pohranu koriste svoj klaster HDInsight.

Možete popisa i Preuzmi Preuzmi datoteke pomoću [Azure EŽA](../xplat-cli-install.md). Na primjer, popis datoteka u **primjeru/zakretanja**, koristite sljedeću naredbu:

    azure storage blob list <container-name> example/curl

Da biste preuzeli datoteke, koristite sljedeće:

    azure storage blob download <container-name> <blob-name> <destination-file>

> [AZURE.NOTE] Morate navesti naziv računa za pohranu koji sadrži blob-om pomoću na `-a` i `-k` parametara ili postavljanje na **AZURE\_prostora za POHRANU\_RAČUN** i **AZURE\_prostora za POHRANU\_PRISTUP\_KLJUČ** varijable okruženja. Pogledajte [Kako prenijeti podatke HDInsight](hdinsight-upload-data.md) dodatne informacije.

##<a id="summary"></a>Sažetak

Kao što je prikazano u ovom dokumentu, možete koristiti neobrađenog HTTP zahtjev za pokretanje, praćenje i prikaz rezultata grozd zadataka u svoj klaster HDInsight.

Dodatne informacije o OSTALE sučelja koja se koristi u ovom članku potražite u članku [Referenca WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).

##<a id="nextsteps"></a>Daljnji koraci

Opće informacije o zadacima MapReduce u HDInsight:

* [Korištenje MapReduce s Hadoop na HDInsight](hdinsight-use-mapreduce.md)

Informacije o drugim načinima možete raditi s Hadoop na HDInsight:

* [Korištenje grozd s Hadoop na HDInsight](hdinsight-use-hive.md)

* [Korištenje Svinja s Hadoop na HDInsight](hdinsight-use-pig.md)
