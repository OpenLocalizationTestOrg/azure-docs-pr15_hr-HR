<properties
   pageTitle="Korištenje Hadoop Sqoop s zakretanja u HDInsight | Microsoft Azure"
   description="Saznajte kako daljinski slanje Sqoop zadataka HDInsight pomoću zakretanja."
   services="hdinsight"
   documentationCenter=""
   authors="mumian"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/21/2016"
   ms.author="jgao"/>

#<a name="run-sqoop-jobs-with-hadoop-in-hdinsight-with-curl"></a>Pokretanje Sqoop poslove s Hadoop u HDInsight s zakretanja

[AZURE.INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Saznajte kako koristiti zakretanja pokrenuti poslovi Sqoop klaster Hadoop u HDInsight.

Da bismo pokazali kako možete raditi s HDInsight pomoću neobrađenog HTTP zahtjeva za pokretanje, praćenje i dohvaćanje rezultata Sqoop poslove koristi se zakretanja. To funkcionira pomoću WebHCat REST API-JA (prijašnjeg Templeton) dao svoj klaster HDInsight.

> [AZURE.NOTE] Ako su već poznato korištenje sustavom Linux Hadoop poslužiteljima, ali za HDInsight potražite u članku [informacije o korištenju servisa HDInsight na Linux](hdinsight-hadoop-linux-information.md).

##<a name="prerequisites"></a>Preduvjeti

Da biste dovršili korake u ovom članku, morate sljedeće:

* Hadoop na HDInsight klaster (Linux ili utemeljen na sustavu Windows)

* [Zakretanja](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

##<a name="submit-sqoop-jobs-by-using-curl"></a>Slanje Sqoop zadatke pomoću zakretanja

> [AZURE.NOTE] Kada koristite zakretanja ili OSTALE komunikacije s WebHCat, morate autentičnost zahtjeve unosom korisničko ime i lozinku za administratora klaster HDInsight. Naziv klaster morate koristiti i kao dio sustava na Uniform Resource Identifier (URI) koristiti za slanje zahtjeve na poslužitelj.
>
> Da biste naredbe u ovom odjeljku, zamijenite **korisničko ime** korisnika za provjeru autentičnosti klaster, i zamijenite **lozinku** lozinke za korisnički račun. Zamijenite **CLUSTERNAME** naziv svoj klaster.
>
> REST API-JA je osigurani putem [osnovnu provjeru autentičnosti](http://en.wikipedia.org/wiki/Basic_access_authentication). Uvijek trebali napraviti zahtjeva putem sigurne HTTP (HTTPS) da biste bili sigurni da vjerodajnica za sigurno šalju na poslužitelj.

1. Iz naredbenog retka, koristite sljedeću naredbu da biste provjerili možete li se povezati s svoj klaster HDInsight:

        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status

    Trebali biste dobiti odgovor sličnu ovoj:

        {"status":"ok","version":"v1"}

    Su parametri koji se koristi u tu naredbu na sljedeći način:

    * **-u** – korisničko ime i lozinku koje se koriste za provjeru autentičnosti zahtjev.
    * **-G** - označava da je zahtjevom GET.

    Početak URL **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**bit će na isti način za sve zahtjeve za. Put **/status**, znači da je zahtjev da biste se vratili na status WebHCat (poznat i kao Templeton) poslužitelja. 

2. Da biste poslali sqoop zadatak, koristite sljedeće:


        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d command="export --connect jdbc:sqlserver://SQLDATABASESERVERNAME.database.windows.net;user=USERNAME@SQLDATABASESERVERNAME;password=PASSWORD;database=SQLDATABASENAME --table log4jlogs --export-dir /tutorials/usesqoop/data --input-fields-terminated-by \0x20 -m 1" -d statusdir="wasbs:///example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/sqoop

    Su parametri koji se koristi u tu naredbu na sljedeći način:

    * **-d** - jer `-G` ne koristi, zahtjev zadane postavke na način objavu. `-d`određuje vrijednosti podataka koje se šalju sa zahtjevom za.

        * **user.name** - korisnika koji se izvodi naredbu.

        * **naredba** - u Sqoop naredba izvršiti.

        * **statusdir** - direktorija koji će biti zapisane stanje za taj zadatak.

    Ta se naredba mora vratiti ID zadatka koje je moguće koristiti da biste provjerili status zadatka.

        {"id":"job_1415651640909_0026"}

3. Da biste provjerili status posla, koristite sljedeću naredbu. Zamijenite **JOBID** vrijednost vraća u prethodnom koraku. Na primjer, ako je vrijednost `{"id":"job_1415651640909_0026"}`, tada će biti **JOBID** `job_1415651640909_0026`.

        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state

    Ako je zadatak završio, stanje bit će **je uspio**.

    > [AZURE.NOTE] Zahtjev za zakretanja vraća JavaScript objekt notaciju (JSON) dokument s informacijama o posla; jq se koristi za dohvaćanje samo vrijednost stanja.

4. Kada je stanje posla promijenio se da **je uspjelo**, možete dohvatiti rezultate posla iz spremište blobova platforme Azure. Na `statusdir` parametar proslijeđen s upitom sadrži lokaciju Izlazna datoteka; u ovom slučaju **wasbs: / / / primjer/zakretanja**. Tu adresu pohranjuje izlaz posla u direktoriju **primjer/zakretanja** na spremnik zadani prostor za pohranu koriste svoj klaster HDInsight.

    Možete popisa i Preuzmi Preuzmi datoteke pomoću [Azure EŽA](../xplat-cli-install.md). Na primjer, popis datoteka u **primjeru/zakretanja**, koristite sljedeću naredbu:

        azure storage blob list <container-name> example/curl

    Da biste preuzeli datoteke, koristite sljedeće:

        azure storage blob download <container-name> <blob-name> <destination-file>

    > [AZURE.NOTE] Navedite naziv računa za pohranu koji sadrži blob-om pomoću na `-a` i `-k` parametara ili postavljanje na **AZURE\_prostora za POHRANU\_RAČUN** i **AZURE\_prostora za POHRANU\_PRISTUP\_KLJUČ** varijable okruženja. U odjeljku < na href = "hdinsight-prijenos-data.md" target = "_prazno" Dodatne informacije.

##<a name="limitations"></a>Ograničenja

* Skupno izvoz - Linux s operacijskim sustavom HDInsight, Sqoop poveznik za izvoz podataka u Microsoft SQL Server ili baze podataka SQL Azure trenutno ne podržava umeće skupno.

* Grupno slanje promjena - sa sustavom Linux HDInsight kada se koristi u `-batch` prebacivanje prilikom izvršavanja umeće, Sqoop će obavljanje više umeće umjesto grupnog slanja promjena operacije Umetanje.

##<a name="summary"></a>Sažetak

Kao što je prikazano u ovom dokumentu, možete koristiti neobrađenog HTTP zahtjev za pokretanje, praćenje i prikaz rezultata Sqoop zadataka na svoj klaster HDInsight.

Dodatne informacije o OSTALE sučelja koji se koristi u ovom članku potražite u članku <a href="https://sqoop.apache.org/docs/1.99.3/RESTAPI.html" target="_blank">Vodič za Sqoop REST API -JA</a>.

##<a name="next-steps"></a>Daljnji koraci

Općenite informacije o grozd s HDInsight:

* [Korištenje Sqoop s Hadoop na HDInsight](hdinsight-use-sqoop.md)

Dodatne informacije o drugim načinima možete raditi s Hadoop na HDInsight:

* [Korištenje grozd s Hadoop na HDInsight](hdinsight-use-hive.md)

* [Korištenje Svinja s Hadoop na HDInsight](hdinsight-use-pig.md)

* [Korištenje MapReduce s Hadoop na HDInsight](hdinsight-use-mapreduce.md)

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md




[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


