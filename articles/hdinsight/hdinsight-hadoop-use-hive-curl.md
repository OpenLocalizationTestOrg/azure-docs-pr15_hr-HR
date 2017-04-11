<properties
   pageTitle="Korištenje Hadoop grozd s zakretanja u HDInsight | Microsoft Azure"
   description="Saznajte kako daljinski slanje Svinja zadataka HDInsight pomoću zakretanja."
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
   ms.date="09/07/2016"
   ms.author="larryfr"/>

#<a name="run-hive-queries-with-hadoop-in-hdinsight-with-curl"></a>Pokretanje grozd upita s Hadoop u HDInsight s zakretanja

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

U ovom dokumentu će Saznajte kako koristiti zakretanja za pokretanje grozd upita na Hadoop na klasteru Azure HDInsight.

Da bismo pokazali kako možete raditi s HDInsight pomoću neobrađenog HTTP zahtjeva za pokretanje, praćenje i dohvaćanje rezultata upita grozd koristi se zakretanja. To funkcionira pomoću WebHCat REST API-JA (prijašnjeg Templeton) dao svoj klaster HDInsight.

> [AZURE.NOTE] Ako su već poznato korištenje sustavom Linux Hadoop poslužiteljima, ali za HDInsight potražite u članku [što trebate znati o Hadoop na HDInsight sustavom Linux](hdinsight-hadoop-linux-information.md).

##<a id="prereq"></a>Preduvjeti

Da biste dovršili korake u ovom članku, morate sljedeće:

* Hadoop na HDInsight klaster (Linux ili utemeljen na sustavu Windows)

* [Zakretanja](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

##<a id="curl"></a>Pokretanje grozd upita pomoću zakretanja

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

    Početak URL **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**bit će na isti način za sve zahtjeve za. Put **/status**, znači da je zahtjev da biste se vratili na status WebHCat (poznat i kao Templeton) poslužitelja. Možete zatražiti i verziju grozd pomoću sljedeće naredbe:

        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/version/hive

    To će se vratiti odgovor sličnu ovoj:

        {"module":"hive","version":"0.13.0.2.1.6.0-2103"}

2. Da biste stvorili novu tablicu pod nazivom **log4jLogs**, koristite sljedeće:

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="set+hive.execution.engine=tez;DROP+TABLE+log4jLogs;CREATE+EXTERNAL+TABLE+log4jLogs(t1+string,t2+string,t3+string,t4+string,t5+string,t6+string,t7+string)+ROW+FORMAT+DELIMITED+FIELDS+TERMINATED+BY+' '+STORED+AS+TEXTFILE+LOCATION+'wasbs:///example/data/';SELECT+t4+AS+sev,COUNT(*)+AS+count+FROM+log4jLogs+WHERE+t4+=+'[ERROR]'+AND+INPUT__FILE__NAME+LIKE+'%25.log'+GROUP+BY+t4;" -d statusdir="wasbs:///example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/hive

    Su parametri koji se koristi u tu naredbu na sljedeći način:

    * **-d** - jer `-G` ne koristi, zahtjev zadane postavke na način objavu. `-d`određuje vrijednosti podataka koje se šalju sa zahtjevom za.

        * **user.name** - korisnika koji se izvodi naredbu.

        * **izvršavanje** - u HiveQL naredbe za izvršavanje.

        * **statusdir** - direktorija koji će biti zapisane stanje za taj zadatak.

    Te naredbe izvoditi sljedeće radnje:

    * **DROP TABLE** – brisanje tablice i podatkovne datoteke ako već postoji u tablici.

    * **Stvaranje VANJSKE TABLICE** – stvara novu tablicu 'vanjskih u grozd. Vanjski tablice u grozd spremiti samo definiciju tablice. Podaci ostaje na izvornom mjestu.

        > [AZURE.NOTE] Vanjski tablice treba koristiti kada očekujete podatke u podlozi ažurirati iz vanjskog izvora, kao što su prijenos proces automatiziranog podataka ili pak tako da drugi MapReduce postupak, ali uvijek želite grozd upita da biste koristili najnovije podatke.
        >
        > Odbacivanje vanjska tablica ne **ne** Izbriši podatke, samo definiciju tablice.

    * **OBLIKOVANJE REDAKA** - govori vrste Hive kako će se podaci oblikovani. U ovom slučaju odvojenih zarezom polja u svakom zapisnika.

    * **POHRANJENE mjesto kao TEXTFILE** - govori vrste Hive gdje se nalazi podatke pohranjene (imenik oglednim podacima), a da je pohranjenih kao tekst.

    * **Odaberite** – odabir ukupan broj sve retke u kojima je stupac **t4** sadrži vrijednost **[pogreške]**. To mora vratiti vrijednost **3** kao što su tri retke koji sadrže vrijednost.

    > [AZURE.NOTE] Uočite da se zamijene razmaka između HiveQL naredbe na `+` znaka kada se koristi s zakretanja. Ponuđeni vrijednosti koje sadrže prostora, kao što je razdjelnika, trebali biste ne zamjenjuje po `+`.

    * **INPUT__FILE__NAME kao što su "% 25.log"** - Ova ograničenja pretraživanja samo pomoću datoteke u. zapisnika. Ako to ne postoji, grozd pokušajte pretražiti sve datoteke u taj imenik i poddirektorije, uključujući datoteke koje odgovaraju shemi stupca definirana za tu tablicu.

    > [AZURE.NOTE] Napomena % 25 je URL kodirani obrazac %, tako da je stvarni uvjet `like '%.log'`. Postotak ima će se kodirati URL, kao što se smatra posebnog znaka u URL-ova.

    Ta se naredba mora vratiti ID zadatka koje je moguće koristiti da biste provjerili status zadatka.

        {"id":"job_1415651640909_0026"}

3. Da biste provjerili status posla, koristite sljedeću naredbu. Zamijenite **JOBID** vrijednost vraća u prethodnom koraku. Na primjer, ako je vrijednost `{"id":"job_1415651640909_0026"}`, tada će biti **JOBID** `job_1415651640909_0026`.

        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state

    Ako je zadatak završio, stanje bit će **je uspio**.

    > [AZURE.NOTE] Zahtjev za zakretanja vraća JavaScript objekt notacije (JSON) dokument s informacijama o posla; jq se koristi za dohvaćanje samo vrijednost stanja.

4. Kada je stanje posla promijenio se da **je uspjelo**, možete dohvatiti rezultate posla iz spremište blobova platforme Azure. Na `statusdir` parametar proslijeđen s upitom sadrži lokaciju Izlazna datoteka; u ovom slučaju **wasbs: / / / primjer/zakretanja**. Tu adresu pohranjuje izlaz posla u direktoriju **primjer/zakretanja** na spremnik zadani prostor za pohranu koriste svoj klaster HDInsight.

    Možete popisa i Preuzmi Preuzmi datoteke pomoću [Azure EŽA](../xplat-cli-install.md). Na primjer, popis datoteka u **primjeru/zakretanja**, koristite sljedeću naredbu:

        azure storage blob list <container-name> example/curl

    Da biste preuzeli datoteke, koristite sljedeće:

        azure storage blob download <container-name> <blob-name> <destination-file>

    > [AZURE.NOTE] Navedite naziv računa za pohranu koji sadrži blob-om pomoću na `-a` i `-k` parametara ili postavljanje na **AZURE\_prostora za POHRANU\_RAČUN** i **AZURE\_prostora za POHRANU\_PRISTUP\_KLJUČ** varijable okruženja. U odjeljku < na href = "hdinsight-prijenos-data.md" target = "_prazno" Dodatne informacije.

6. Da biste stvorili novu tablicu 'Interna pod nazivom **errorLogs**pomoću sljedeće naredbe:

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="set+hive.execution.engine=tez;CREATE+TABLE+IF+NOT+EXISTS+errorLogs(t1+string,t2+string,t3+string,t4+string,t5+string,t6+string,t7+string)+STORED+AS+ORC;INSERT+OVERWRITE+TABLE+errorLogs+SELECT+t1,t2,t3,t4,t5,t6,t7+FROM+log4jLogs+WHERE+t4+=+'[ERROR]'+AND+INPUT__FILE__NAME+LIKE+'%25.log';SELECT+*+from+errorLogs;" -d statusdir="wasbs:///example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/hive

    Te naredbe izvoditi sljedeće radnje:

    * **Stvaranje TABLICA ako ne postoji** - stvara tablicu, ako se još ne postoji. Budući da ne koristi **VANJSKI** ključnu riječ, to je interni tablice, koja je pohranjena u skladištu grozd podataka i potpuno upravlja grozd.

        > [AZURE.NOTE] Za razliku od vanjskih tablice ispuštanje Interna tablica će izbrisati podatke u podlozi kao i.

    * **SPREMLJENI kao ORC** - sprema podatke u obliku Optimizirano retka stupčastu (ORC). Ovo je vrlo optimizirana i učinkovito oblik za spremanje grozd podataka.
    * PREBRIŠI **Umetanje... Odaberite** - odabire redaka iz tablice **log4jLogs** koja sadrži **[pogreške]**, a zatim umeće podatke u tablicu **errorLogs** .
    * **Odaberite** - odabire sve retke iz nove tablice **errorLogs** .

7. Koristite ID zadatka vraća da biste provjerili status zadatka. Kada je uspio, koristite Azure EŽA za Mac i Linux Windows kao što je opisano prethodno da biste preuzeli i prikaz rezultata. Izlaz smiju sadržavati tri retke koji sadrže **[pogreške]**.


##<a id="summary"></a>Sažetak

Kao što je prikazano u ovom dokumentu, možete koristiti neobrađenog HTTP zahtjev za pokretanje, praćenje i prikaz rezultata grozd zadataka na svoj klaster HDInsight.

Dodatne informacije o OSTALE sučelja koji se koristi u ovom članku potražite u članku <a href="https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference" target="_blank">Referenca WebHCat</a>.

##<a id="nextsteps"></a>Daljnji koraci

Općenite informacije o grozd s HDInsight:

* [Korištenje grozd s Hadoop na HDInsight](hdinsight-use-hive.md)

Dodatne informacije o drugim načinima možete raditi s Hadoop na HDInsight:

* [Korištenje Svinja s Hadoop na HDInsight](hdinsight-use-pig.md)

* [Korištenje MapReduce s Hadoop na HDInsight](hdinsight-use-mapreduce.md)

Ako koristite Tez grozd, pogledajte sljedeće dokumente za ispravljanje pogrešaka informacije:

* [Korištenje Tez korisničko Sučelje na HDInsight utemeljen na sustavu Windows](hdinsight-debug-tez-ui.md)

* [Korištenje prikaza Ambari Tez na Linux temelje HDInsight](hdinsight-debug-ambari-tez-view.md)

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


