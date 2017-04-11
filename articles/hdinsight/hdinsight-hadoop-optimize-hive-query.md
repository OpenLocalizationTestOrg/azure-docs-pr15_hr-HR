<properties
   pageTitle="Optimiziranje grozd upite za brže izvršavanje u HDInsight | Microsoft Azure"
   description="Saznajte kako optimizirati grozd upite za Hadoop u HDInsight."
   services="hdinsight"
   documentationCenter=""
   authors="rashimg"
   manager="mwinkle"
   editor="cgronlun"
   tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="07/28/2015"
   ms.author="rashimg"/>


# <a name="optimize-hive-queries-for-hadoop-in-hdinsight"></a>Optimiziranje grozd upite za Hadoop u HDInsight

Prema zadanim postavkama, Hadoop klastere nisu optimizirane za performanse. U ovom se članku opisuje nekoliko najčešće grozd performanse optimizaciju metode koje možete primijeniti na našem upite.

##<a name="scale-out-worker-nodes"></a>Promjena veličine izgleda čvorove tempiranja

Povećanje broja radnih čvorove klasteru možete koristiti više mappers i reducers će se pokrenuti paralelno. Možete povećati skaliranje izgleda u HDInsight na dva načina:

- Prilikom dodjele resursa možete navesti broj radnih čvorove pomoću portala za Azure, Azure PowerShell ili više platformi naredbeni redak sučelja.  Dodatne informacije potražite u članku [klastere HDInsight dodjele resursa](hdinsight-provision-clusters.md). Na sljedećem zaslonu Prikaži zaposlenik čvor Konfiguracija portala za Azure:

    ![scaleout_1][image-hdi-optimize-hive-scaleout_1]

- U vrijeme izvođenja koje možete i promjena veličine izgleda klaster bez ponovnog stvaranja jedan. To je prikazano u nastavku.
![scaleout_1][image-hdi-optimize-hive-scaleout_2]

Dodatne informacije o različitim virtualnim strojevima podržava servisa HDInsight potražite u članku [HDInsight cijene](https://azure.microsoft.com/pricing/details/hdinsight/).

##<a name="enable-tez"></a>Omogućivanje Tez

[Apache Tez](http://hortonworks.com/hadoop/tez/) je zamjenski izvođenja mehanizam za modul MapReduce:

![tez_1][image-hdi-optimize-hive-tez_1]


Tez se brže jer:

- Izvršavanje usmjereni Acyclic grafikon (DAG) kao jedan zadatak u modul MapReduce, DAG koji je prikazan zahtijeva svaki skup mappers nakon kojeg je jedan skup reducers. Zbog toga više MapReduce zadataka spun za svaki upit grozd. Tez nema takve ograničenja i možete obrada složene DAG kao jedan zadatak stoga minimiziranje indirektni zadatak pokretanja.
- **Avoids nepotrebne zapisivanja** Zbog više zadataka koji se spun za istog upita grozd u modul MapReduce, izlazna svaki zadatak sastavljene za HDFS za Srednja podataka. Budući da Tez minimizira broj zadataka za svaki upit grozd se mogućnost da bi se izbjeglo nepotrebno pisanje.
- **Kašnjenja Minimizes pokretanja** Tez je bolje da biste minimizirali pokretanja odgode smanjiti broj mappers potrebne da biste pokrenuli i i poboljšanje optimizaciju cijeloj.
- **Ponovo koristi spremnika** Kad god je moguće Tez može ponovno koristiti spremnike da bi se smanjuje Latencija zbog pokreće spremnika.
- **Neprekinuti optimizaciju tehnike** Najčešći optimizaciju je učinjeno tijekom sastavljanja faze. No dostupna je dodatne informacije o ulaza koje omogućuju bolje optimizaciju tijekom izvođenja. Tez koristi continous optimizacije tehnike koji omogućuje ga da biste optimizirali plan Dodatno u fazi runtime.

Dodatne informacije o koncepte, kliknite [ovdje](http://hortonworks.com/hadoop/tez/)

Možete unijeti bilo koji upit grozd Tez omogućiti dodavanje prefiksa upit o postavci u nastavku:

    set hive.execution.engine=tez;

Za klastere utemeljen na sustavu Windows HDInsight Tez mora biti omogućen prilikom dodjele resursa. Slijedi primjer Azure PowerShell skripte za dodjelu resursa Hadoop klaster s Tez omogućen:

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $clusterName = "[HDInsightClusterName]"
    $location = "[AzureDataCenter]" #i.e. West US
    $dataNodes = 32 # number of worker nodes in the cluster

    $defaultStorageAccountName = "[DefaultStorageAccountName]"
    $defaultStorageContainerName = "[DefaultBlobContainerName]"
    $defaultStorageAccountKey = $defaultStorageAccountKey = Get-AzureStorageKey $defaultStorageAccountName.ToLower() | %{ $_.Primary }

    $hdiUserName = "[HTTPUserName]"
    $hdiPassword = "[HTTPUserPassword]"

    $hdiSecurePassword = ConvertTo-SecureString $hdiPassword -AsPlainText -Force
    $hdiCredential = New-Object System.Management.Automation.PSCredential($hdiUserName, $hdiSecurePassword)

    $hiveConfig = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightHiveConfiguration'
    $hiveConfig.Configuration = @{ "hive.execution.engine"="tez" }

    New-AzureHDInsightClusterConfig -ClusterSizeInNodes $dataNodes -HeadNodeVMSize Standard_D14 -DataNodeVMSize Standard_D14 |
    Set-AzureHDInsightDefaultStorage -StorageAccountName "$defaultStorageAccountName.blob.core.windows.net" -StorageAccountKey $defaultStorageAccountKey -StorageContainerName $defaultStorageContainerName |
    Add-AzureHDInsightConfigValues -Hive $hiveConfig |
    New-AzureHDInsightCluster -Name $clusterName -Location $location -Credential $hdiCredential

    
> [AZURE.NOTE] Sustavom Linux HDInsight klastere imati Tez po zadanom omogućena.
    

## <a name="hive-partitioning"></a>Vrste Hive particija

/ I operacija je usko grlo glavne performansi za pokretanje grozd upita. Performanse mogu poboljšati ako mogu se smanjiti količinu podataka koje je potrebno je moguće čitati. Prema zadanim postavkama, grozd upita pregledajte cijele tablice grozd. Ovo je sjajno za upite kao skenira tablice, no upite za koje je potrebno pregledati malu količinu podataka (primjerice upita za filtriranje) Time ste stvorili nepotrebne indirektni. Grozd particija omogućuje grozd upite za pristup samo potrebne količinu podataka u tablicama grozd.

Grozd particija implementira se putem reorganiziranje sirovim podacima u novi direktorija uz svaki particija imate vlastitu direktorij – gdje particija definirao korisnik. Sljedeći dijagram prikazuje particija tablicu vrste Hive po stupcu *godina*. Za svaku godinu stvara se novi imenik.

![particija][image-hdi-optimize-hive-partitioning_1]

Stvaranje particija što valja:

- **Učinite ne under-particija** - particija u stupcima sa samo nekoliko vrijednosti mogu prouzročiti vrlo malo particije. Ako, na primjer, particija na spol će stvoriti samo dvije particije će biti stvoren (Muška i ženski), stoga samo smanjiti latenciju maksimalno pola.

- **Ne particija previše** – na na druge ekstremne, stvaranje particije na stupac koji sadrži jedinstvene vrijednosti (npr. korisnički ID) uzrokovat će više particija uzrokuje mnogo opterećenjem na namenode klaster kao imat će učiniti veliku količinu direktorija.

- **Izbjegavajte podataka skew** - pametan odabir ključ za stvaranje particija tako da su sve particije čak i veličine. Primjer je particija na *Stanje* može uzrokovati broj zapisa u odjeljku Kaliforniji biti gotovo 30 x koji Vermont zbog razlike u populacije.

Da biste stvorili particije tablice, koristite uvjet *Particije po* :

    CREATE TABLE lineitem_part
        (L_ORDERKEY INT, L_PARTKEY INT, L_SUPPKEY INT,L_LINENUMBER INT,
         L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
         L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
         L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE        STRING, L_SHIPINSTRUCT STRING, L_SHIPMODE STRING,
         L_COMMENT STRING)
    PARTITIONED BY(L_SHIPDATE STRING)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    STORED AS TEXTFILE;

Nakon stvaranja particioniranom tablice, možete stvoriti statičnog particija ili dinamički particija.

- **Statički particija** znači da se već sharded podatke uvedene u odgovarajuće direktorija i mogu postavljati grozd particije ručno na temelju lokacije direktorija. Prikazana je u nastavku isječak koda.

        INSERT OVERWRITE TABLE lineitem_part
        PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’)
        SELECT * FROM lineitem 
        WHERE lineitem.L_SHIPDATE = ‘5/23/1996 12:00:00 AM’

        ALTER TABLE lineitem_part ADD PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’))
        LOCATION ‘wasbs://sampledata@ignitedemo.blob.core.windows.net/partitions/5_23_1996/'

- **Dinamični particija** znači da želite grozd da biste automatski stvorili particije umjesto vas. Budući da se ne možemo ste već stvorili stvaranje particija tablice iz pripremna tablice, sve možemo trebate napraviti je podatke u tablici particioniranom umetnuti kao što je prikazano u nastavku:

        SET hive.exec.dynamic.partition = true;
        SET hive.exec.dynamic.partition.mode = nonstrict;
        INSERT INTO TABLE lineitem_part
        PARTITION (L_SHIPDATE)
        SELECT L_ORDERKEY as L_ORDERKEY, L_PARTKEY as L_PARTKEY , 
             L_SUPPKEY as L_SUPPKEY, L_LINENUMBER as L_LINENUMBER,
             L_QUANTITY as L_QUANTITY, L_EXTENDEDPRICE as L_EXTENDEDPRICE,
             L_DISCOUNT as L_DISCOUNT, L_TAX as L_TAX, L_RETURNFLAG as       L_RETURNFLAG, L_LINESTATUS as L_LINESTATUS, L_SHIPDATE as       L_SHIPDATE_PS, L_COMMITDATE as L_COMMITDATE, L_RECEIPTDATE as   L_RECEIPTDATE, L_SHIPINSTRUCT as L_SHIPINSTRUCT, L_SHIPMODE as      L_SHIPMODE, L_COMMENT as L_COMMENT, L_SHIPDATE as L_SHIPDATE FROM lineitem;

Dodatne informacije potražite u članku [Particije tablice](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-PartitionedTables).

##<a name="use-the-orcfile-format"></a>Korištenje ORCFile oblika

Grozd podržava drugim oblicima datoteka. Ako, na primjer:

- **Tekst**: to je zadani oblik datoteke i funkcionalna na većini scenarija
- **Avro**: funkcionira i za interoperabilnost scenariji
- **ORC/Parquet**: najprikladniji za performanse

Oblik ORC (Optimizirano retka stupčastu) je vrlo učinkovit način pohrane podataka grozd. U usporedbi s drugim oblicima ORC ima sljedeće prednosti:

- Podrška za složene vrste uključujući DateTime i vrstama složenih i djelomično strukturirani
- do spajanja 70%
- Indeksi svaki od 10 000 redaka koje omogućuju preskakanje redaka
- značajan neposredne u izvođenju izvođenja

Da biste omogućili ORC oblik, najprije stvorite tablicu s uvjet *spremljene kao ORC*:

    CREATE TABLE lineitem_orc_part
        (L_ORDERKEY INT, L_PARTKEY INT,L_SUPPKEY INT, L_LINENUMBER INT,
         L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
         L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
         L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE STRING,
         L_SHIPINSTRUCT STRING, L_SHIPMODE STRING, L_COMMENT     STRING)
    PARTITIONED BY(L_SHIPDATE STRING)
    STORED AS ORC;

Nakon toga Umetanje podataka u tablici ORC iz pripremna tablice. Ako, na primjer:

    INSERT INTO TABLE lineitem_orc
    SELECT L_ORDERKEY as L_ORDERKEY, 
           L_PARTKEY as L_PARTKEY , 
           L_SUPPKEY as L_SUPPKEY,
           L_LINENUMBER as L_LINENUMBER,
           L_QUANTITY as L_QUANTITY, 
           L_EXTENDEDPRICE as L_EXTENDEDPRICE,
           L_DISCOUNT as L_DISCOUNT,
           L_TAX as L_TAX,
           L_RETURNFLAG as L_RETURNFLAG,
           L_LINESTATUS as L_LINESTATUS,
           L_SHIPDATE as L_SHIPDATE,
           L_COMMITDATE as L_COMMITDATE,
           L_RECEIPTDATE as L_RECEIPTDATE, 
           L_SHIPINSTRUCT as L_SHIPINSTRUCT,
           L_SHIPMODE as L_SHIPMODE,
           L_COMMENT as L_COMMENT
    FROM lineitem;

Dodatne informacije o formatu ORC [ovdje](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC).

##<a name="vectorization"></a>Vectorization

Vectorization omogućuje grozd za obradu skupine 1024 retke zajedno umjesto obrade jedan redak po jedan. To znači da jednostavne operacije gotovi brže jer su manje interni kod potrebni za pokretanje.

Da biste omogućili vectorization prefiks grozd upit s sljedeće postavke:

    set hive.vectorized.execution.enabled = true;

Dodatne informacije potražite u članku [Vectorized izvršavanje upita](https://cwiki.apache.org/confluence/display/Hive/Vectorized+Query+Execution).


##<a name="other-optimization-methods"></a>Drugi načini optimizaciju

Postoji više metoda optimizaciju koja možete imati na umu, na primjer:

- **Vrste hive bucketing:** postupak koji omogućuje skupine ili fazi velikih skupova podataka radi optimiziranja performansi upita.
- **Uključivanje optimizacije:** optimizacije izvršavanje upita grozd, planiranje da biste poboljšali učinkovitost spojevi i smanjili potrebe za savjete za korisnika. Dodatne informacije potražite u članku [Uključivanje optimizirati](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+JoinOptimization#LanguageManualJoinOptimization-JoinOptimization).
- **Povećavanje Reducers**

##<a id="nextsteps"></a>Daljnji koraci
U ovom se članku ste naučili nekoliko uobičajenih grozd upita optimizacije metoda. Dodatne informacije potražite u sljedećim člancima:

- [Korištenje Apache grozd u HDInsight](hdinsight-use-hive.md)
- [Analizirajte podatke o kašnjenju leta pomoću grozd u HDInsight](hdinsight-analyze-flight-delay-data.md)
- [Analiziranje podataka Twitter pomoću grozd u HDInsight](hdinsight-analyze-twitter-data.md)
- [Analiza podataka senzor korištenju konzole za vrste Hive upita na Hadoop u HDInsight](hdinsight-hive-analyze-sensor-data.md)
- [Korištenje grozd s HDInsight da biste analizirali zapisnika s web-mjesta](hdinsight-hive-analyze-website-log.md)


[image-hdi-optimize-hive-scaleout_1]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_1.png
[image-hdi-optimize-hive-scaleout_2]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_2.png
[image-hdi-optimize-hive-tez_1]: ./media/hdinsight-hadoop-optimize-hive-query/tez_1.png
[image-hdi-optimize-hive-partitioning_1]: ./media/hdinsight-hadoop-optimize-hive-query/partitioning_1.png
