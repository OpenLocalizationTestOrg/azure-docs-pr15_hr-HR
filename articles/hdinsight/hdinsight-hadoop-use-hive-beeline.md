<properties
   pageTitle="Korištenje Beeline za rad s grozd na HDInsight (Hadoop) | Microsoft Azure"
   description="Saznajte kako koristiti SSH za povezivanje s klaster Hadoop u HDInsight, a zatim interaktivno poslati grozd upita pomoću Beeline. Beeline je utility za rad s HiveServer2 putem JDBC."
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
   ms.date="10/10/2016"
   ms.author="larryfr"/>

#<a name="use-hive-with-hadoop-in-hdinsight-with-beeline"></a>Korištenje grozd s Hadoop u HDInsight s Beeline

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

U ovom se članku će Saznajte kako koristiti sigurne ljuske (SSH) za povezivanje sa sustavom Linux HDInsight klaster, a zatim interaktivno poslati grozd upita pomoću alata za [Beeline](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients#HiveServer2Clients-Beeline–NewCommandLineShell) naredbenog retka.

> [AZURE.NOTE] Za povezivanje s grozd beeline koristi JDBC. Dodatne informacije o korištenju JDBC s grozd, potražite u članku [Povezivanje grozd na Azure HDInsight pomoću upravljački program za vrste Hive JDBC](hdinsight-connect-hive-jdbc-driver.md).

##<a id="prereq"></a>Preduvjeti

Da biste dovršili korake u ovom članku, morate sljedeće:

* U operacijskim sustavom Linux Hadoop na klasteru HDInsight.

* Klijent za SSH. Linux, Unix i Mac OS mora obuhvaćati klijent za SSH. Korisnici sustava Windows morate preuzeti klijent, kao što su [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

##<a id="ssh"></a>Povezivanje s SSH

Povezivanje s na potpuno kvalificirani naziv domene (FQDN) od svoj klaster HDInsight pomoću naredbe "SSH". FQDN bit će naziv koji ste dali klaster, zatim **. azurehdinsight.net**. Na primjer, sljedeće želite povezati klaster pod nazivom **myhdinsight**:

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**Ako ste unijeli ključ certifikat za provjeru autentičnosti SSH** stvaranja HDInsight klaster, možda ćete morati navedite mjesto privatni ključ na klijentskim sustavima:

    ssh admin@myhdinsight-ssh.azurehdinsight.net -i ~/mykey.key

**Ako upišete lozinku za provjeru autentičnosti SSH** stvaranja HDInsight klaster, morat ćete unijeti lozinku kad se to od vas zatraži.

Dodatne informacije o korištenju SSH sa servisa HDInsight potražite u članku [Korištenje SSH s operacijskim sustavom Linux Hadoop na HDInsight Linux, OS X i Unix](hdinsight-hadoop-linux-use-ssh-unix.md).

###<a name="putty-windows-based-clients"></a>PuTTY (klijenti utemeljen na sustavu Windows)

Windows ne nudi ugrađene SSH klijenta. Preporučujemo korištenje **PuTTY**, koji možete preuzeti s [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

Dodatne informacije o korištenju PuTTY potražite u članku [Korištenje SSH sa sustavom Linux Hadoop na HDInsight iz sustava Windows ](hdinsight-hadoop-linux-use-ssh-windows.md).

##<a id="beeline"></a>Koristite naredbu Beeline

1. Nakon uspostave, koristite sljedeće da biste pokrenuli Beeline:

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin

    To će pokrenuti klijent Beeline te je povežite JDBC URL. Ovdje, `localhost` koristi jer HiveServer2 pokreće na oba glavni čvorovi u klasteru pa ćemo koristite Beeline izravno na primarni headnode.
    
    Nakon dovršetka naredbu će dođete na na `jdbc:hive2://localhost:10001/>` upit.

3. Naredbe beeline obično počinju s `!` znak, primjerice `!help` prikazuje pomoć. No u `!` često će ispušteni. Na primjer, `help` će funkcionirati i.

    Ako pogledate pomoć, primijetit ćete `!sql`, koja se koristi za izvođenje naredbe HiveQL. Međutim, HiveQL pa obično se koristi da možete izostaviti prethodni `!sql`. Sljedeća dva izraza imaju točno iste rezultate; Prikaz tablice koje su trenutno dostupne putem grozd:
    
        !sql show tables;
        show tables;
    
    Na novu klaster, mora biti navedena samo jednu tablicu: __hivesampletable__.

4. Da biste prikazali shema na hivesampletable, koristite sljedeće:

        describe hivesampletable;
        
    Vratit će sljedeće informacije:
    
        +-----------------------+------------+----------+--+
        |       col_name        | data_type  | comment  |
        +-----------------------+------------+----------+--+
        | clientid              | string     |          |
        | querytime             | string     |          |
        | market                | string     |          |
        | deviceplatform        | string     |          |
        | devicemake            | string     |          |
        | devicemodel           | string     |          |
        | state                 | string     |          |
        | country               | string     |          |
        | querydwelltime        | double     |          |
        | sessionid             | bigint     |          |
        | sessionpagevieworder  | bigint     |          |
        +-----------------------+------------+----------+--+

    Prikazat će se stupaca u tablici. Dok se ne možemo nije izvršiti neke upite odabiranja te podatke, umjesto toga stvaranje novost tablice da bismo pokazali kako podatke učitali u grozd i primijeniti shemu.
    
5. Unesite sljedeće naredbe za stvaranje nove tablice pod nazivom **log4jLogs** pomoću oglednih podataka koji ste dobili uz klaster HDInsight:

        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Te naredbe izvoditi sljedeće radnje:

    * **DROP TABLE** – brisanje tablice i podatkovne datoteke u slučaju da već postoji u tablici.
    * **Stvaranje VANJSKE TABLICE** – stvara novu tablicu 'vanjskih u grozd. Vanjski tablice spremati samo definiciju tablice u grozd. Podaci ostaje na izvornom mjestu.
    * **OBLIKOVANJE REDAKA** - govori vrste Hive kako će se podaci oblikovani. U ovom slučaju odvojenih zarezom polja u svakom zapisnika.
    * **POHRANJENE mjesto kao TEXTFILE** - govori vrste Hive gdje se nalazi podatke pohranjene (imenik oglednim podacima), a da je pohranjenih kao tekst.
    * **Odaberite** – odabir ukupan broj sve retke u kojima je stupac **t4** sadrži vrijednost **[pogreške]**. To mora vratiti vrijednost **3** kao što su tri retke koji sadrže vrijednost.
    * **INPUT__FILE__NAME kao što su "%.log"** - govori vrste Hive koje ćemo samo mora vratiti podatke iz datoteka u. zapisnika. Obično samo promijenile podataka pomoću na istu shemu u istu mapu kada postavljanje upita pomoću grozd, no taj primjer zapisnik pohranjuju se drugi oblici podataka.

    > [AZURE.NOTE] Vanjski tablice treba koristiti kada očekujete podatke u podlozi ažurirati iz vanjskog izvora, kao što su prijenos proces automatiziranog podataka ili pak tako da drugi MapReduce postupak, ali uvijek želite grozd upita da biste koristili najnovije podatke.
    >
    > Odbacivanje vanjska tablica ne **ne** Izbriši podatke, samo definiciju tablice.
    
    Izlaz iz ta naredba mora biti sličnu ovoj:
    
        INFO  : Tez session hasn't been created yet. Opening session
        INFO  :
        
        INFO  : Status: Running (Executing on YARN cluster with App id application_1443698635933_0001)
        
        INFO  : Map 1: -/-      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0(+1)/1  Reducer 2: 0/1
        INFO  : Map 1: 0(+1)/1  Reducer 2: 0/1
        INFO  : Map 1: 1/1      Reducer 2: 0/1
        INFO  : Map 1: 1/1      Reducer 2: 0(+1)/1
        INFO  : Map 1: 1/1      Reducer 2: 1/1
        +----------+--------+--+
        |   sev    | count  |
        +----------+--------+--+
        | [ERROR]  | 3      |
        +----------+--------+--+
        1 row selected (47.351 seconds)

4. Da biste napustili Beeline, koristite `!quit`.

##<a id="file"></a>Pokrenite HiveQL datoteku

Beeline može se koristiti i da biste pokrenuli datoteku koja sadrži naredbe HiveQL. Poduzmite sljedeće korake da biste stvorili datoteku, a zatim ga pomoću Beeline pokrenuti.

1. Da biste stvorili novu datoteku pod nazivom __query.hql__, koristite sljedeću naredbu:

        nano query.hql
        
2. Kada se uređivač otvori, koristite sljedeće kao sadržaj datoteke. Ovaj upit će stvoriti novu tablicu 'Interna pod nazivom **errorLogs**:

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    Te naredbe izvoditi sljedeće radnje:

    * **Stvaranje TABLICA ako ne postoji** - stvara tablicu, ako se još ne postoji. Budući da ne koristi **VANJSKI** ključnu riječ, to je interni tablice, koja je pohranjena u skladištu grozd podataka i potpuno upravlja grozd.
    * **SPREMLJENI kao ORC** - sprema podatke u obliku Optimizirano retka stupčastu (ORC). Ovo je vrlo optimizirana i učinkovito oblik za spremanje grozd podataka.
    * PREBRIŠI **Umetanje... Odaberite** - odabire redaka iz tablice **log4jLogs** koja sadrži **[pogreške]**, a zatim umeće podatke u tablicu **errorLogs** .
    
    > [AZURE.NOTE] Za razliku od vanjskih tablice ispuštanje Interna tablica će izbrisati podatke u podlozi kao i.
    
3. Da biste spremili datoteku, pomoću prečaca __Ctrl__+ ___X__, a zatim unesite __Y__i na kraju __Enter__.

4. Koristite sljedeće da biste pokrenuli datoteku pomoću Beeline. __Naziv glavnog računala__ zamijenite nazivom nabavili ranije glavni čvor i __lozinku__ pomoću lozinke za administratorski račun:

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin -i query.hql

    > [AZURE.NOTE] Na `-i` parametar pokreće Beeline, pokreće naredbe u datoteci query.hql i ostaje u Beeline pri na `jdbc:hive2://localhost:10001/>` upit. Možete i pokrenuti datoteku pomoću na `-f` parametar koji se vratiti tulumu kada datoteku obrađen.

5. Da biste provjerili tablici **errorLogs** stvoren, koristite sljedeću naredbu da biste se vratili sve retke iz **errorLogs**:

        SELECT * from errorLogs;

    Tri redaka podataka bi trebala biti vraćena, svi koji sadrže **[pogreške]** u t4 stupca:
    
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | errorlogs.t1  | errorlogs.t2  | errorlogs.t3  | errorlogs.t4  | errorlogs.t5  | errorlogs.t6  | errorlogs.t7  |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | 2012-02-03    | 18:35:34      | SampleClass0  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 18:55:54      | SampleClass1  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 19:25:27      | SampleClass4  | [ERROR]       | incorrect     | id            |               |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        3 rows selected (1.538 seconds)

## <a name="more-about-beeline-connectivity"></a>Dodatne informacije o Beeline povezivanja

Koraci u ovom dokumentu korištenje `localhost` za povezivanje s HiveServer2 sustavom headnode klaster. Dok je možete koristiti na naziv glavnog računala ili na potpuno kvalificirani naziv domene od na headnode one zahtijevaju dodatne korake za postupak (korake da biste pronašli naziv glavnog računala ili FQDN). Korištenje `localhost` je dovoljno prilikom korištenja Beeline iz na headnode.

Ako imate na rub čvor u svoj klaster s Beeline instaliran, morat ćete koristite vrijednost naziv glavnog računala ili FQDN u headnode da biste se povezali.

Ako imate Beeline instalirani na klijentskom računalu izvan svoj klaster, možete se povezati pomoću sljedeće naredbe. Zamijenite __CLUSTERNAME__ naziv svoj klaster HDInsight. Zamijenite __LOZINKE__ lozinku za račun za administratore (HTTP prijava).

    beeline -u 'jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;ssl=true?hive.server2.transport.mode=http;hive.server2.thrift.http.path=hive2' -n admin -p PASSWORD

Imajte na umu da parametara/URI razlikuje se od kada radi izravno na headnode ili na rub čvorovi unutar klaster. To je zato klaster povezati s Internetom koristi javno pristupnik koji usmjerava promet putem priključak 443. Osim toga, nekoliko drugih servisa su vidljiva kroz javno pristupnika na priključak 443, pa URI razlikuje se od kada izravno povezivanje. Prilikom povezivanja s Internetom morate autentičnost i sesiju unosom lozinke.

##<a id="summary"></a><a id="nextsteps"></a>Daljnji koraci

Kao što možete vidjeti naredbu Beeline omogućuje jednostavnu interaktivno pokrenuti grozd upita programa klaster HDInsight.

Općenite informacije o grozd u HDInsight:

* [Korištenje grozd s Hadoop na HDInsight](hdinsight-use-hive.md)

Dodatne informacije o drugim načinima možete raditi s Hadoop na HDInsight:

* [Korištenje Svinja s Hadoop na HDInsight](hdinsight-use-pig.md)

* [Korištenje MapReduce s Hadoop na HDInsight](hdinsight-use-mapreduce.md)

Ako koristite Tez grozd, pogledajte sljedeće dokumente za ispravljanje pogrešaka informacije:

* [Korištenje Tez korisničko Sučelje na HDInsight utemeljen na sustavu Windows](hdinsight-debug-tez-ui.md)

* [Korištenje prikaza Ambari Tez na sustavom Linux HDInsight](hdinsight-debug-ambari-tez-view.md)

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

[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html


[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md


[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

