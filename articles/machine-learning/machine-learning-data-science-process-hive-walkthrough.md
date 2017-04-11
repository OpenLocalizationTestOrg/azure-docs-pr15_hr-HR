<properties
    pageTitle="Proces znanstvenog timu podataka u akciji: korištenje Hadoop klaster | Microsoft Azure"
    description="Pomoću postupka timu podataka znanosti za kraj do kraja scenarij u employing programa HDInsight Hadoop klaster omogućuje stvaranje i implementaciju modela pomoću javno dostupna skup podataka."
    services="machine-learning,hdinsight"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="hangzh;bradsev" />


# <a name="the-team-data-science-process-in-action-using-hdinsight-hadoop-clusters"></a>Proces znanstvenog timu podataka u akciji: korištenje klastere HDInsight Hadoop

U ovom vodiču koristimo [Timu podataka znanstvenog procesa (TDSP)](data-science-process-overview.md) u u scenariju završetka do kraja pomoću [Azure HDInsight Hadoop klaster](https://azure.microsoft.com/services/hdinsight/) za pohranu, istraživanje i značajka inženjeringom podatke iz javno dostupna dataset [Trips taksi SEBI](http://www.andresmh.com/nyctaxitrips/) i dolje ogledne podatke. Modeli podataka ugrađenih s Azure strojno učenje učiniti binarnim i multiclass klasifikaciju i regresije predvidljivu zadaci.

Objašnjenje koji pokazuje kako rukovati većim skup podataka (1 terabajta) za slične scenarija pomoću HDInsight Hadoop klastere za obradu podataka potražite u članku [Timu podataka znanstvenog postupak – pomoću Azure HDInsight Hadoop klastere na 1 TB skup podataka](machine-learning-data-science-process-hive-criteo-walkthrough.md).

Također je moguće koristiti za IPython bilježnicu da biste izvršili zadatke navedene u prikazu pomoću dataset 1 TB. Korisnici koji biste željeli pokušati takvog trebali proučiti temi [Criteo vodič pomoću vrste Hive ODBC veze](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) .


## <a name="dataset"></a>Opis NEW taksi Trips Dataset

NEW taksi putovanja podaci otprilike 20GB sažete vrijednosti odvojenih zarezom (CSV) datoteke (dekomprimirati ~ 48GB), comprising više od 173 milijuna pojedinačne trips i na fares platiti za svaki put. Svaki zapis putovanja obuhvaća prijem i predaju mjesto i vrijeme, anonymized hack (upravljačkog programa) broj licenci i broj medallion (taksi na jedinstveni ID-ja). Podaci pokriva sve trips u godini 2013 uvjet, a u sljedeća dva skupova podataka za svaki mjesec:

1. CSV datoteke 'trip_data' sadrže detalje putovanja, kao što su broj passengers, prijem i dropoff točaka, trajanja putovanja, a duljina putovanja. Evo nekoliko uzoraka zapisa:

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868

2. CSV datoteke 'trip_fare' sadrže detalje o prijevoz platiti za svaki put, kao što je način plaćanja, prijevoz iznos, dodatna naknada i poreza, savjete i tolls, i ukupan iznos plaćen. Evo nekoliko uzoraka zapisa:

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Jedinstveni ključ da biste se pridružili putovanja\_podataka i putovanja\_prijevoz sastoji se od polja: medallion, hack\_krajnjeg i prijem\_datuma i vremena.

Da biste sve pojedinosti važne za određeni putovanja, dovoljno je da biste se pridružili s tri tipke: "medallion", "probiti\_licence" i "prijem\_datetime".

Ne možemo opisuju neke više detalja podataka kada ne možemo pohraniti u tablice grozd uskoro.

## <a name="mltasks"></a>Primjeri predviđanje zadataka
Kada Približavanje podatke, određivanje vrste predviđanja želite napraviti na temelju njegove analizu olakšava pojašnjavaju zadataka koje ćete morati uključiti u proces.
Ovdje navodimo tri primjera predviđanje probleme koji smo u ovaj vodič čije formulation temelji se na adresu u *Savjet\_iznos*:

1. **Binarni klasifikacija**: predviđanje hoće li Savjet je plaćenu putovanja, odnosno na *Savjet\_iznos* koji je veći od 0 kn je pozitivan primjer, dok je *Savjet\_iznos* od 0 kn je negativan primjer.

        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0

2. **Multiclass klasifikacija**: za predviđanje raspon savjet iznosi plaćenu na putovanja. Ćemo podijeliti na *Savjet\_iznos* u pet intervale ili klasa:

        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20

3. **Zadatak regresije**: za predviđanje količinu savjet platiti za putovanje.  


## <a name="setup"></a>Postavljanje programa HDInsight Hadoop klaster za napredne analize

>[AZURE.NOTE] To je obično zadatak za **administratore** .

Možete postaviti Azure okruženje za napredne analize koji uključuje se HDInsight klaster tri koraka:

1. [Stvaranje računa za pohranu](../storage/storage-create-storage-account.md): račun za pohranu se koristi za spremanje podataka u spremište blobova platforme Azure. Podaci koji se koriste u klastere HDInsight i nalazi se ovdje.

2. [Prilagodba Hadoop Azure HDInsight klastere za napredne analize postupka i tehnologije](machine-learning-data-science-customize-hadoop-cluster.md). Ovaj korak stvara sustava Hadoop Azure HDInsight klaster sa 64-bitni Anaconda Python 2.7 instalirano sve čvorove. Postoje dva važnih koraka Imajte na umu prilikom prilagodbe svoj klaster HDInsight.

    * Imajte na umu da biste se povezali s računom za pohranu stvorena u koraku 1 s svoj klaster HDInsight prilikom stvaranja. Taj račun za pohranu koristi se za pristup podacima koji se obrađuju unutar klaster.

    * Nakon stvaranja klaster Omogućivanje daljinskog pristupa za čvor glavni klaster. Idite na karticu **Konfiguracija** , a zatim kliknite **Omogući udaljene**. Ovaj korak određuje korisničke vjerodajnice koristiti za prijavu na udaljenom.

3. [Stvaranje Azure strojnog učenja radnog prostora](machine-learning-create-workspace.md): ovo računalo Azure učenje radnog prostora služi za izradu modela strojnog učenja. Ovaj zadatak je adresirana nakon dovršetka do početne podatke Istraživanje i dolje uzorkovanje pomoću klaster HDInsight.

## <a name="getdata"></a>Dohvaćanje podataka iz javnog izvora

>[AZURE.NOTE] To je obično zadatak za **administratore** .

Da biste [SEBI taksi Trips](http://www.andresmh.com/nyctaxitrips/) skup podataka s njegova javno mjesto, možete koristiti bilo koji od metode opisane u [Premjesti podatke iz spremišta blobova Azure](machine-learning-data-science-move-azure-blob.md) da biste kopirali podatke na vašem računalu.

U nastavku ćemo opisuju kako koristiti AzCopy za prijenos datoteka koja sadrži podatke. Da biste preuzeli i instalirali AzCopy slijedite upute na [Početak rada s AzCopy naredbenog retka uslužni](../storage/storage-use-azcopy.md).

1. U prozoru naredbenog retka problema sljedeće naredbe AzCopy *< path_to_data_folder >* zamjenjuje željeni odredište:


        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S

2. Kada se dovrši kopiju, ukupno 24 zipana datoteka su u mapi podataka odabrali. Raspakiraj preuzete datoteke u istom direktorij na lokalnom računalu. Zabilježite mapu u kojoj se nalaze datoteke koje nisu komprimirane. Ta će se nazivaju i na *< put\_da biste\_unzipped_data\_datoteke\> * je što slijedi.


## <a name="upload"></a>Prijenos podataka na zadani spremnik Azure HDInsight Hadoop klaster

>[AZURE.NOTE] To je obično zadatak za **administratore** .

U okviru sljedeće naredbe AzCopy zamijenite sljedećih parametara stvarnih vrijednosti koje ste naveli prilikom stvaranja klaster Hadoop i unzipping podatkovne datoteke.

* ***& #60; path_to_data_folder >*** direktorija (zajedno put) na računalu koje sadrže raspakiranu datoteku paketa podatkovnih datoteka  
* ***& #60; naziv računa spremišta Hadoop klaster >*** račun za pohranu koji je povezan s svoj klaster HDInsight
* ***& #60; zadane spremnik klaster Hadoop >*** spremnik zadanom koristi svoj klaster. Imajte na umu da spremnika zadani je naziv obično isti naziv kao klaster sam. Ako, na primjer, Klaster se zove "abc123.azurehdinsight.net", spremnik zadani je abc123.
* ***& #60; ključ za račun za pohranu >*** ključ za račun za pohranu koji se koristi svoj klaster

Iz naredbenog retka ili komponente Windows PowerShell prozora na računalu, pokrenite sljedeće dvije AzCopy naredbe.

Ta se naredba prenosi podatke putovanja ***nyctaxitripraw*** direktorij u zadanom spremnik klaster Hadoop.

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxitripraw /DestKey:<storage account key> /S /Pattern:trip_data_*.csv

Ta se naredba prenosi podatke prijevoz ***nyctaxifareraw*** direktorij u zadanom spremnik klaster Hadoop.

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxifareraw /DestKey:<storage account key> /S /Pattern:trip_fare_*.csv

Podaci moraju se sada u spremište blobova platforme Azure i spremne za potrošnju unutar klaster HDInsight.

## <a name="#download-hql-files"></a>Prijava na glavni čvor Hadoop klaster i i Priprema za analizu exploratory podataka

>[AZURE.NOTE] To je obično zadatak za **administratore** .

Da biste pristupili čvor glavni klaster za analizu podataka exploratory i dolje navedeni podaci, slijedite postupka opisanog u [programu Access lakši čvor od Hadoop klaster](machine-learning-data-science-customize-hadoop-cluster.md#headnode).

U ovom vodiču prvenstveno koristimo upite napisan [vrste Hive](https://hive.apache.org/), jezika za upite za SQL nalik za izvođenje explorations preliminarni podataka. Upiti grozd spremaju se u .hql datoteke. Ne možemo pa dolje ogledne ti podaci će se koristiti za stvaranje modela unutar Azure strojnog učenja.

Da biste pripremili klaster za analizu podataka exploratory, ne možemo preuzmite .hql datoteka koja sadrži odgovarajući grozd skripte iz [github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) u lokalnom direktoriju (C:\temp) na glavni čvor. Da biste to učinili, otvorite **naredbeni redak** s unutar čvor glavni klaster i problema sljedeće dvije naredbe:

    set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/DataScienceProcess/DataScienceScripts/Download_DataScience_Scripts.ps1'

    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

Ove dvije naredbe će preuzeti sve .hql datoteke koje je potrebno u ovom vodiču za lokalni direktorij ***C:\temp & #92;*** u glavni čvor.

## <a name="#hive-db-tables"></a>Stvaranje baze podataka grozd i tablica particije prema mjesecu

>[AZURE.NOTE] To je obično zadatak za **administratore** .

Spremni smo da biste stvorili grozd tablice za naše dataset taksi SEBI.
U glavni čvor Hadoop klaster, otvorite ***Hadoop naredbenog retka*** na radnoj površini čvor glavni pa unesite direktorija grozd tako da unesete naredbu

    cd %hive_home%\bin

>[AZURE.NOTE] **Pokreni sve grozd naredbe u prikazu s iznad smeće grozd / direktorija upit. To će voditi brigu o probleme put automatski. Koristimo uvjete "Grozd direktorija pitanje", "grozd smeće / direktorija pitanje", i "Hadoop naredbeni redak" naizmjence u ovom vodiču.**

Iz imenika zatraži grozd, unesite sljedeću naredbu u Hadoop naredbenog retka glavni čvora slanje grozd upit sa stvaranjem grozd baze podataka i tablice:

    hive -f "C:\temp\sample_hive_create_db_and_tables.hql"

Ovdje je sadržaj u ***C:\temp\sample\_grozd\_stvaranje\_db\_i\_tables.hql*** datoteke čime se grozd baze podataka ***nyctaxidb*** i tablice ***putovanja*** i ***prijevoz***.

    create database if not exists nyctaxidb;

    create external table if not exists nyctaxidb.trip
    (
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count int,
        trip_time_in_secs double,
        trip_distance double,
        pickup_longitude double,
        pickup_latitude double,
        dropoff_longitude double,
        dropoff_latitude double)  
    PARTITIONED BY (month int)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/trip' TBLPROPERTIES('skip.header.line.count'='1');

    create external table if not exists nyctaxidb.fare
    (
        medallion string,
        hack_license string,
        vendor_id string,
        pickup_datetime string,
        payment_type string,
        fare_amount double,
        surcharge double,
        mta_tax double,
        tip_amount double,
        tolls_amount double,
        total_amount double)
    PARTITIONED BY (month int)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/fare' TBLPROPERTIES('skip.header.line.count'='1');

Ova skripta grozd stvara dvije tablice:

* "putovanja" tablica sadrži putovanja detalje o svakom ride (upravljački program detalje prikupljanja vrijeme, udaljenost putovanja i puta)
* "prijevoz" tablica sadrži detalje prijevoz (prijevoz iznos, iznos savjet, tolls i surcharges).

Ako trebate sve dodatna pomoć za ove postupke ili želite istražiti zamjenski one, potražite u odjeljku [Slanje vrste Hive upita izravno iz Hadoop naredbenog retka ](machine-learning-data-science-move-hive-tables.md#submit).

## <a name="#load-data"></a>Učitavanje podataka u grozd tablice tako da particije

>[AZURE.NOTE] To je obično zadatak za **administratore** .

Dataset taksi SEBI ima s prirodnim particija prema mjesecu, koristimo da biste omogućili brži obrada i upit vremena. PowerShell naredbe ispod (proizlaze iz imenika grozd pomoću **Hadoop naredbenog retka**) učitati podatke u "putovanja" i "prijevoz" grozd tablice particije po mjesecima.

    for /L %i IN (1,1,12) DO (hive -hiveconf MONTH=%i -f "C:\temp\sample_hive_load_data_by_partitions.hql")

Na *uzorka\_grozd\_učitavanje\_podataka\_po\_partitions.hql* datoteka sadrži sljedeće naredbe **UČITATI** .

    LOAD DATA INPATH 'wasb:///nyctaxitripraw/trip_data_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.trip PARTITION (month=${hiveconf:MONTH});
    LOAD DATA INPATH 'wasb:///nyctaxifareraw/trip_fare_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.fare PARTITION (month=${hiveconf:MONTH});

Imajte na umu broj upita grozd ovdje koristimo u postupku Istraživanje obuhvaćaju pogled na samo jedna particija ili na samo nekoliko particije. No tih upita nije moguće pokrenuti preko cijele podataka.

### <a name="#show-db"></a>Prikaz baze podataka u HDInsight Hadoop klaster

Da biste prikazali baze podataka stvorene u HDInsight Hadoop klaster unutar prozora Hadoop naredbenog retka, pokrenite sljedeću naredbu u Hadoop naredbenog retka:

    hive -e "show databases;"

### <a name="#show-tables"></a>Prikaži grozd tablica u bazi podataka nyctaxidb

Da biste prikazali tablice u bazi podataka nyctaxidb, pokrenite sljedeću naredbu u Hadoop naredbenog retka:

    hive -e "show tables in nyctaxidb;"

Ne možemo možete potvrditi tablice imaju slanjem naredbe u nastavku:

    hive -e "show partitions nyctaxidb.trip;"

Očekivani rezultat je prikazano u nastavku:

    month=1
    month=10
    month=11
    month=12
    month=2
    month=3
    month=4
    month=5
    month=6
    month=7
    month=8
    month=9
    Time taken: 2.075 seconds, Fetched: 12 row(s)

Isto tako, ne možemo možete biti sigurni da su tablice prijevoz particije slanjem naredbe u nastavku:

    hive -e "show partitions nyctaxidb.fare;"

Očekivani rezultat je prikazano u nastavku:

    month=1
    month=10
    month=11
    month=12
    month=2
    month=3
    month=4
    month=5
    month=6
    month=7
    month=8
    month=9
    Time taken: 1.887 seconds, Fetched: 12 row(s)

## <a name="#explore-hive"></a>Istraživanje podataka i značajka inženjering u grozd

>[AZURE.NOTE] To je obično zadatak **Pozvana podataka** .

Istraživanje podataka i značajka inženjerska zadaci za podatke učitava u tablicama grozd je moguće napraviti pomoću grozd upita. Evo primjera zadatke kao što su da vodit ćemo vas kroz u ovom odjeljku:

- Prikaz prvih 10 zapisa u obje tablice.
- Istraživanje podataka distribucija nekoliko polja u različitim prozorima vremena.
- Istražite kvalitete podataka dužinu i širinu polja.
- Generiranje binarnim i multiclass klasifikacije natpise na temelju na **Savjet\_iznos**.
- Generiranje značajke po računalstvo međusobne Izravni putovanja.

### <a name="exploration-view-the-top-10-records-in-table-trip"></a>Informacije: Prikaz prvih 10 zapisa u tablicu putovanja

>[AZURE.NOTE] To je obično zadatak **Pozvana podataka** .

Da biste vidjeli izgleda podataka, ne možemo pregledajte 10 zapise iz svaku tablicu. Na upitu direktorija grozd konzole Hadoop naredbenog retka za provjeru zapise pokretanje sljedeća dva upita zasebno.

Da biste zapise prvih 10 u tablici "putovanja" s prvom mjesecu:

    hive -e "select * from nyctaxidb.trip where month=1 limit 10;"

Da biste gornji 10 zapisa u tablici "prijevoz" s prvom mjesecu:

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;"

Često je korisno spremiti zapise u datoteku jednostavan prikaz. Promjena male gore navedeni upit izvršava sljedeće:

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;" > C:\temp\testoutput

### <a name="exploration-view-the-number-of-records-in-each-of-the-12-partitions"></a>Informacije: Prikaz broja zapisa u svakoj od 12 particije

>[AZURE.NOTE] To je obično zadatak **Pozvana podataka** .

Kamata je kako broj trips mijenja tijekom kalendarske godine. Grupiranje prema mjesecu omogućuje nam da biste vidjeli što raspodjele trips izgleda.

    hive -e "select month, count(*) from nyctaxidb.trip group by month;"

Tako ćete dobiti nam izlaz:

    1       14776615
    2       13990176
    3       15749228
    4       15100468
    5       15285049
    6       14385456
    7       13823840
    8       12597109
    9       14107693
    10      15004556
    11      14388451
    12      13971118
    Time taken: 283.406 seconds, Fetched: 12 row(s)

Ovdje, prvi stupac sadrži mjesec, a druga je broj trips za taj mjesec.

Ne možemo možete zbrojiti ukupan broj zapisa u skupu podataka naš putovanja slanjem sljedeću naredbu naredbenom grozd direktorija.

    hive -e "select count(*) from nyctaxidb.trip;"

To rezultira:

    173179759
    Time taken: 284.017 seconds, Fetched: 1 row(s)

Pomoću naredbi slične onima prikazanim za skup podataka putovanja, ne možemo možete izdati grozd upita iz upita grozd direktorija za skup podataka prijevoz da biste provjerili valjanost broj zapisa.

    hive -e "select month, count(*) from nyctaxidb.fare group by month;"

Tako ćete dobiti nam izlaz:

    1       14776615
    2       13990176
    3       15749228
    4       15100468
    5       15285049
    6       14385456
    7       13823840
    8       12597109
    9       14107693
    10      15004556
    11      14388451
    12      13971118
    Time taken: 253.955 seconds, Fetched: 12 row(s)

Imajte na umu da su potpuno isti broj trips mjesečno vratio za oba skupa podataka. To sadrži prvi Provjera valjanosti da podatke učitan je ispravno.

Brojanje ukupan broj zapisa u skupu podataka prijevoz možete učiniti pomoću naredbe ispod iz imenika upit grozd:

    hive -e "select count(*) from nyctaxidb.fare;"

To rezultira:

    173179759
    Time taken: 186.683 seconds, Fetched: 1 row(s)

Ukupan broj zapisa u obje tablice također jednak je. Ovo osigurava da podatke učitan je ispravno drugi provjere valjanosti.

### <a name="exploration-trip-distribution-by-medallion"></a>Istraživanje: Putovanja raspodjele po medallion

>[AZURE.NOTE] To je obično zadatak **Pozvana podataka** .

U ovom se primjeru označava medallion (taksi brojeve) s više od 100 trips unutar određenog vremenskog razdoblja. Prednosti upit iz programa access particioniranom tablice jer ga je conditioned prema varijable particija **mjesec**. Rezultati upita zapisuju se queryoutput.tsv lokalne datoteke u `C:\temp` na glavni čvor.

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

Ovdje je sadržaj *uzorka\_grozd\_putovanja\_count\_po\_medallion.hql* datoteke za provjeru.

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

Medallion u skupu podataka NEW taksi označava jedinstveni cab. Možete utvrđujemo koji cabs su "zauzet" zatražiti koje korisnik više od određenog broja trips izvršene u određenom vremenskom razdoblju. U sljedećem primjeru označava cabs koja je proizvela više od stotinu trips u prvom tri mjeseca i sprema rezultate upita u lokalne datoteke C:\temp\queryoutput.tsv.

Ovdje je sadržaj *uzorka\_grozd\_putovanja\_count\_po\_medallion.hql* datoteke za provjeru.

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

Iz imenika upit grozd problema naredbe u nastavku:

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a>Istraživanje: Putovanja raspodjele medallion i hack_license

>[AZURE.NOTE] To je obično zadatak **Pozvana podataka** .

Kada Istraživanje skup podataka često želimo da biste pregledali broj ko-poruke o otkazivanju grupa vrijednosti. U ovom se odjeljku navedite primjera kako to učiniti za cabs i upravljačke programe.

Na *uzorka\_grozd\_putovanja\_count\_po\_medallion\_license.hql* datoteka grupe prijevoz skupa podataka na "medallion" i "hack_license" i vraća broji svaki kombinacije. U nastavku su njegov sadržaj.

    SELECT medallion, hack_license, COUNT(*) as trip_count
    FROM nyctaxidb.fare
    WHERE month=1
    GROUP BY medallion, hack_license
    HAVING trip_count > 100
    ORDER BY trip_count desc;

Ovaj upit vraća cab i upravljački program za određeni kombinacije poredane silaznim broj trips.

Iz imenika zatraži grozd, pokrenite:

    hive -f "C:\temp\sample_hive_trip_count_by_medallion_license.hql" > C:\temp\queryoutput.tsv

Rezultati upita zapisuju u lokalnu datoteku C:\temp\queryoutput.tsv.

### <a name="exploration-assessing-data-quality-by-checking-for-invalid-longitudelatitude-records"></a>Istraživanje: Assessing kvalitete podataka potvrđivanjem za zapise koji nisu valjani dužinu/zemljopisnu širinu

>[AZURE.NOTE] To je obično zadatak **Pozvana podataka** .

Uobičajeni cilj analiza podataka exploratory je weed članku koji nisu valjani i loše zapisi. Primjer u ovom odjeljku određuje hoće li dužinu ili zemljopisnu širinu polja sadrže vrijednost daleko izvan područja SEBI. Budući da je vjerojatnost da takvih zapisa imaju je pogrešne dužinu-zemljopisnu širinu vrijednosti, rado bismo ih uklonili iz sve podatke koji će se koristiti za Modeliranje.

Ovdje je sadržaj *uzorka\_grozd\_kvalitete\_assessment.hql* datoteke za provjeru.

        SELECT COUNT(*) FROM nyctaxidb.trip
        WHERE month=1
        AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(pickup_latitude AS float) NOT BETWEEN 30 AND 90
        OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(dropoff_latitude AS float) NOT BETWEEN 30 AND 90);


Iz imenika zatraži grozd, pokrenite:

    hive -S -f "C:\temp\sample_hive_quality_assessment.hql"

Argument *Do nedjelje* obuhvaćeno ta naredba ukida ispisa zaslona status zadataka vrste Hive karte/smanjivanje. To je korisno jer će se zaslon ispisa izlaza upita grozd čitljiviji.

### <a name="exploration-binary-class-distributions-of-trip-tips"></a>Istraživanje: Binarni predmete distribucija savjeta za putovanja

> [AZURE.NOTE] To je obično zadatak **Pozvana podataka** .

Binarni klasifikacija problema navedene u odjeljku [primjera zadataka predviđanje](machine-learning-data-science-process-hive-walkthrough.md#mltasks) je korisno je znati je li savjet odobren ili ne. Binarni je raspodjele savjeta:

* Savjet dali (klase 1, savjet\_iznos > 0 kn)  
* Nema savjet (predmete 0, savjet\_iznos = 0 kn).

Na *uzorka\_grozd\_tipped\_frequencies.hql* to čini datoteku prikazano u nastavku.

    SELECT tipped, COUNT(*) AS tip_freq
    FROM
    (
        SELECT if(tip_amount > 0, 1, 0) as tipped, tip_amount
        FROM nyctaxidb.fare
    )tc
    GROUP BY tipped;

Iz imenika zatraži grozd, pokrenite:

    hive -f "C:\temp\sample_hive_tipped_frequencies.hql"


### <a name="exploration-class-distributions-in-the-multiclass-setting"></a>Istraživanje: Klase distribucija multiclass postavku

> [AZURE.NOTE] To je obično zadatak **Pozvana podataka** .

Multiclass klasifikacija problema navedene u odjeljku [primjera zadataka predviđanje](machine-learning-data-science-process-hive-walkthrough.md#mltasks) tom skupu podataka i lends sam da biste prirodnim klasifikacija gdje željeli bismo predviđanje količinu dobili savjete. Koristimo intervale definiranja raspona savjet u upitu. Da biste dobili predmete distribucija za različite savjet raspona, koristimo na *uzorka\_grozd\_savjet\_raspon\_frequencies.hql* datoteku. U nastavku su njegov sadržaj.

    SELECT tip_class, COUNT(*) AS tip_freq
    FROM
    (
        SELECT if(tip_amount=0, 0,
            if(tip_amount>0 and tip_amount<=5, 1,
            if(tip_amount>5 and tip_amount<=10, 2,
            if(tip_amount>10 and tip_amount<=20, 3, 4)))) as tip_class, tip_amount
        FROM nyctaxidb.fare
    )tc
    GROUP BY tip_class;

Pokrenite sljedeću naredbu konzoli Hadoop naredbenog retka:

    hive -f "C:\temp\sample_hive_tip_range_frequencies.hql"

### <a name="exploration-compute-direct-distance-between-two-longitude-latitude-locations"></a>Istraživanje: Izračun Izravni udaljenost između dva mjesta dužinu širinu

> [AZURE.NOTE] To je obično zadatak **Pozvana podataka** .

Imate mjera Izravni udaljenost omogućuje nam da biste saznali nepodudaranje između njega i udaljenost stvarni putovanja. Ta značajka ne možemo motivirate tako da ističu da je putnika može biti manji vjerojatno savjet ako ih otkriti da upravljački program sadrži namjerno zauzima ih dulje usmjeravanje.

Da biste pogledali usporedbu između stvarni putovanja udaljenost i [Haversine udaljenost](http://en.wikipedia.org/wiki/Haversine_formula) između dvije točke dužinu širinu (udaljenost "odlično krug"), koristimo trigonometrijske funkcije dostupne unutar grozd, odnosno:

    set R=3959;
    set pi=radians(180);

    insert overwrite directory 'wasb:///queryoutputdir'

    select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude, trip_distance, trip_time_in_secs,
    ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
     *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
     *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
     /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
     +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*
     pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance
    from nyctaxidb.trip
    where month=1
    and pickup_longitude between -90 and -30
    and pickup_latitude between 30 and 90
    and dropoff_longitude between -90 and -30
    and dropoff_latitude between 30 and 90;

U upitu iznad, R je polumjer zemlji u miljama i pi se pretvara u radijane. Imajte na umu da točke dužinu širinu "filtriraju" da biste uklonili vrijednosti koje su far from područje SEBI.

U ovom slučaju ne možemo pisati naš rezultate direktorij pod nazivom "queryoutputdir". Slijed naredbi prikazano u nastavku najprije stvoriti taj imenik Izlaz, a zatim pokreće naredbu grozd.

Iz imenika zatraži grozd, pokrenite:

    hdfs dfs -mkdir wasb:///queryoutputdir

    hive -f "C:\temp\sample_hive_trip_direct_distance.hql"


Rezultati upita zapisuju se 9 Azure blob-ova ***queryoutputdir/000000\_0*** za ***queryoutputdir/000008\_0*** u odjeljku zadani spremnik klaster Hadoop.

Da biste vidjeli veličinu pojedinačne blob-ova, ne možemo pokrenite sljedeću naredbu iz imenika upit grozd:

    hdfs dfs -ls wasb:///queryoutputdir

Da biste vidjeli sadržaj određene datoteke, izgovorite 000000\_0, koristimo Hadoop na `copyToLocal` naredbe, stoga.

    hdfs dfs -copyToLocal wasb:///queryoutputdir/000000_0 C:\temp\tempfile

> [AZURE.WARNING] `copyToLocal`može biti vrlo sporo za velike datoteke, a ne preporučuje se za korištenje s njima.  

Ključne prednosti pojavljuju ti podaci nalaze u blobova platforme Azure je smo možda Istraživanje podataka u Azure strojnog učenja pomoću [Uvoz podataka] [ import-data] modul.


## <a name="#downsample"></a>Dolje ogledne podatke i Sastavi modelima u Azure strojnog učenja

> [AZURE.NOTE] To je obično zadatak **Pozvana podataka** .

Nakon analize faze exploratory podataka smo spremni za dolje oglednih podataka za izgradnju modelima u Azure strojnog učenja. U ovom se odjeljku Pokazat ćemo pomoću upita grozd dolje ogledne podatke, zatim pristupiti iz [Uvezene podatke] [ import-data] modul u Azure strojnog učenja.

### <a name="down-sampling-the-data"></a>Dolje uzorkovanje podataka

Postoje dva koraka u ovom postupku. Najprije ćemo spojite tablice **nyctaxidb.trip** i **nyctaxidb.fare** na tri tipke koje se nalaze u sve zapise: "medallion", "probiti\_licence", a "prijem\_datetime". Ne možemo zatim generiranje u binarni klasifikacija natpis **tipped** i klasifikacija više klase oznake **Savjet\_klase**.

Da biste mogli koristiti dolje ogledne podatke izravno iz [Uvezene podatke] [ import-data] modul u Azure strojnog učenja, potrebno je spremiti rezultate gore navedeni upit za interne tablicu vrste Hive. U što slijedi, ne možemo stvaranje internog tablicu vrste Hive i popunjavanje njegov sadržaj spojene i prema dolje ogledne podatke.

Upit primjenjuje standardne funkcije grozd izravno da biste generirali sat dan, tjedan u godini, weekday (1 označava za ponedjeljak i 7 označava za nedjelja) iz na "prijem\_datetime" polje i izravno udaljenost između prijem i dropoff mjesta. Korisnici može se odnositi na [LanguageManual UDF](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) popis takvih funkcija.

Upit zatim dolje uzoraka podataka tako da se rezultata upita može uklapaju u Studio za učenje Azure računala. Samo o 1% izvorne dataset uvezena je u na Studio.

U nastavku slijede sadržaj *uzorka\_grozd\_Priprema\_za\_aml\_full.hql* datoteku koju Priprema podataka za model izgradnju u Azure strojnog učenja.

        set R = 3959;
        set pi=radians(180);

        create table if not exists nyctaxidb.nyctaxi_downsampled_dataset (

        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        pickup_hour string,
        pickup_week string,
        weekday string,
        passenger_count int,
        trip_time_in_secs double,
        trip_distance double,
        pickup_longitude double,
        pickup_latitude double,
        dropoff_longitude double,
        dropoff_latitude double,
        direct_distance double,
        payment_type string,
        fare_amount double,
        surcharge double,
        mta_tax double,
        tip_amount double,
        tolls_amount double,
        total_amount double,
        tipped string,
        tip_class string
        )
        row format delimited fields terminated by ','
        lines terminated by '\n'
        stored as textfile;

        --- now insert contents of the join into the above internal table

        insert overwrite table nyctaxidb.nyctaxi_downsampled_dataset
        select
        t.medallion,
        t.hack_license,
        t.vendor_id,
        t.rate_code,
        t.store_and_fwd_flag,
        t.pickup_datetime,
        t.dropoff_datetime,
        hour(t.pickup_datetime) as pickup_hour,
        weekofyear(t.pickup_datetime) as pickup_week,
        from_unixtime(unix_timestamp(t.pickup_datetime, 'yyyy-MM-dd HH:mm:ss'),'u') as weekday,
        t.passenger_count,
        t.trip_time_in_secs,
        t.trip_distance,
        t.pickup_longitude,
        t.pickup_latitude,
        t.dropoff_longitude,
        t.dropoff_latitude,
        t.direct_distance,
        f.payment_type,
        f.fare_amount,
        f.surcharge,
        f.mta_tax,
        f.tip_amount,
        f.tolls_amount,
        f.total_amount,
        if(tip_amount>0,1,0) as tipped,
        if(tip_amount=0,0,
        if(tip_amount>0 and tip_amount<=5,1,
        if(tip_amount>5 and tip_amount<=10,2,
        if(tip_amount>10 and tip_amount<=20,3,4)))) as tip_class

        from
        (
        select
        medallion,
        hack_license,
        vendor_id,
        rate_code,
        store_and_fwd_flag,
        pickup_datetime,
        dropoff_datetime,
        passenger_count,
        trip_time_in_secs,
        trip_distance,
        pickup_longitude,
        pickup_latitude,
        dropoff_longitude,
        dropoff_latitude,
        ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
        *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
        *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
        /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
        +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance,
        rand() as sample_key

        from nyctaxidb.trip
        where pickup_latitude between 30 and 90
            and pickup_longitude between -90 and -30
            and dropoff_latitude between 30 and 90
            and dropoff_longitude between -90 and -30
        )t
        join
        (
        select
        medallion,
        hack_license,
        vendor_id,
        pickup_datetime,
        payment_type,
        fare_amount,
        surcharge,
        mta_tax,
        tip_amount,
        tolls_amount,
        total_amount
        from nyctaxidb.fare
        )f
        on t.medallion=f.medallion and t.hack_license=f.hack_license and t.pickup_datetime=f.pickup_datetime
        where t.sample_key<=0.01

Da biste pokrenuli ovaj upit iz imenika upit grozd:

    hive -f "C:\temp\sample_hive_prepare_for_aml_full.hql"

Sada imamo Interna tablice "nyctaxidb.nyctaxi_downsampled_dataset", koji se može pristupiti pomoću [Uvoz podataka] [ import-data] modula iz Azure strojnog učenja. Osim toga, možemo koristiti ovaj skup podataka za stvaranje modela strojnog učenja.  

### <a name="use-the-import-data-module-in-azure-machine-learning-to-access-the-down-sampled-data"></a>Korištenje modul za uvoz podataka u Azure strojnog učenja za pristup podacima sampled prema dolje

Kao preduvjeti za izdavanje grozd upita u [Uvoz podataka] [ import-data] modul Azure strojnog učenja, moramo pristup programa Azure strojnog učenja radnog prostora i pristup vjerodajnice klaster i njegov račun povezan prostora za pohranu.

Detalji [Uvoz podataka] o[ import-data] modul i parametara za unos:

**Poslužitelj HCatalog URI**: Ako je naziv klaster abc123, a zatim je jednostavno: https://abc123.azurehdinsight.net

**Hadoop korisničko ime** : korisničko ime za klaster (**ne** korisničko ime za daljinski pristup)

**Lozinka za račun Hadoop usluge** : lozinku za klaster (**ne** lozinku za daljinski pristup)

**Mjesto za izlazne podatke** : to je odabran biti Azure.

**Naziv računa spremišta Azure** : naziv računa spremišta zadani pridružene klaster.

**Naziv Azure spremnika** : to je zadani naziv spremnik za klaster i najčešće klaster naziv. Za klaster pod nazivom "abc123", to je samo abc123.

> [AZURE.IMPORTANT] **Svakoj tablici želimo upit pomoću [Uvoz podataka] [ import-data] modul u Azure strojnog učenja mora biti interni tablice.** Savjet za određivanje ako je tablica T u bazi podataka D.db Interna tablice je na sljedeći način.

Iz imenika upit grozd problema naredbu:

    hdfs dfs -ls wasb:///D.db/T

Ako je tablica Interna tablice i ona se popunjava, ovdje mora pokazati njegov sadržaj. Da biste utvrdili je li tablica Interna tablice i tako da pomoću programa Explorer prostora za pohranu za Azure. Koristi se za idite na zadani naziv spremnik klaster, a zatim možete filtrirati prema nazivu tablice. Ako tablica i njezin sadržaj prikaže, potvrđuje da je interna tablice.

Evo snimke upita grozd i [Uvoz podataka] [ import-data] modula:

![](./media/machine-learning-data-science-process-hive-walkthrough/1eTYf52.png)

Napomena od naše uzorkovanja podaci nalaze u spremniku zadani prema dolje, rezultirajuća grozd upit iz Azure strojnog učenja je vrlo jednostavne te je samo na "odaberite * iz nyctaxidb.nyctaxi\_smanjene\_podataka".

Skup podataka možda sada se koristiti kao početnu točku za stvaranje modela strojnog učenja.

### <a name="mlmodel"></a>Stvaranje modela u Azure strojnog učenja

Možemo sada da biste prešli na model sastavnih i model implementacije u [Azure strojnog učenja](https://studio.azureml.net). Podaci su spremni za nam da biste koristili u adresiranja probleme predviđanje označena iznad:

**1. binarni klasifikacija**: za predviđanje li Savjet je platiti za putovanje.

**Učenik koristi:** Dva predmete logistika regresije

na. Za taj problem, naš oznaka ciljne (ili predmet) je "tipped". Naš izvorne dolje uzorkovanja dataset ima nekoliko stupaca koji su osipanjem cilj za ovaj eksperiment klasifikacija. Posebno: savjet\_klase, savjet\_iznos i ukupni zbroj\_iznos Otkrij informacije o cilj natpis koji nije dostupan na testiranje vremena. Ne možemo ukloniti te stupce iz razmatranje korištenja [Odabir stupaca u skupu podataka] [ select-columns] modul.

Snimka u nastavku prikazuje naš eksperiment za predviđanje hoće li je savjet platiti za dani putovanja.

![](./media/machine-learning-data-science-process-hive-walkthrough/QGxRz5A.png)

b. Za ovaj eksperiment naš distribucija oznaka ciljne otprilike su 1:1.

Snimka u nastavku prikazuje distribuciju savjet klase oznake binarni klasifikacija problema.

![](./media/machine-learning-data-science-process-hive-walkthrough/9mM4jlD.png)

Kao rezultat ćemo dobiti programa AUC 0.987 kao što je prikazano na slici u nastavku.

![](./media/machine-learning-data-science-process-hive-walkthrough/8JDT0F8.png)

**2. multiclass klasifikacija**: za predviđanje raspon savjet iznosi plaćenu putovanja, pomoću prethodno definirane klase.

**Učenik koristi:** Multiclass logistika regresije

na. Za taj problem, je naš cilj (ili predmet) natpis "savjet\_klasa" koje možete poduzeti jednu od pet vrijednosti (0,1,2,3,4). Kao u slučaju binarni klasifikacija imamo nekoliko stupaca koji su osipanjem cilj za ovaj eksperiment. Posebno: tipped, savjet\_ukupni iznos\_iznos Otkrij informacije o cilj natpis koji nije dostupan na testiranje vremena. Ne možemo ukloniti te stupce pomoću [Odabir stupaca u skupu podataka] [ select-columns] modul.

Snimke u nastavku prikazuje naš eksperiment za predviđanje u koje smeće savjet se vjerojatno padaju (predmete 0: savjet = 0 kn, klase 1: savjet > 0 kn i savjet < = $5, klase 2: savjet > $5 i savjet < = $10, klase 3: savjet > $10 i savjet < = 20 USD, predmete 4: savjet > 20 USD)

![](./media/machine-learning-data-science-process-hive-walkthrough/5ztv0n0.png)

Sada Pokazat ćemo izgleda naš stvarni test predmete raspodjele. Vidimo dok su sustavi predmete 0 i 1, predmet, drugi klase jesu li rijetko.

![](./media/machine-learning-data-science-process-hive-walkthrough/Vy1FUKa.png)

b. Za ovaj eksperiment ćemo pomoću matrice zbrku pogledajte naše accuracies predviđanje. To je prikazano u nastavku.

![](./media/machine-learning-data-science-process-hive-walkthrough/cxFmErM.png)

Imajte na umu da dok je našeg accuracies predmete na sustavi klase prilično dobro, model ne radi odlično "učenje" na rijetkim klase.


**3. regresije zadatka**: za predviđanje količinu savjet platiti za putovanje.

**Učenik koristi:** Stabla odlučivanja boosted

na. Za taj problem, je naš cilj (ili predmet) natpis "savjet\_iznos". Naš cilj osipanjem u ovom slučaju su: tipped, savjet\_predmete, ukupni\_iznos; ove varijable otkrivaju informacija o savjet iznos koji se obično dostupne prilikom testiranje vremena. Ne možemo ukloniti te stupce pomoću [Odabir stupaca u skupu podataka] [ select-columns] modul.

Snimka belows prikazuje naš eksperiment za predviđanje količinu navedeni savjet.

![](./media/machine-learning-data-science-process-hive-walkthrough/11TZWgV.png)

b. Za probleme regresije, ne možemo Izmjerite accuracies naš predviđanje tako da pogledate kvadratne pogreške u predviđanja, koeficijent određivanja i sviđa mi. Pokazat ćemo ih ispod.

![](./media/machine-learning-data-science-process-hive-walkthrough/Jat9mrz.png)

Ne možemo potražite u odjeljku o koeficijent određivanja je 0.709 implying otprilike 71% varijance je objašnjeno po našem modelu koeficijente.

> [AZURE.IMPORTANT] Da biste saznali više o Azure strojnog učenja i upute za pristup i koristiti, pogledajte [što je strojnog učenja?](machine-learning-what-is-machine-learning.md). Vrlo koristan resurs za reprodukciju s mnogo strojnog učenja eksperimenata na Azure strojnog učenja je [Cortana obavještavanje galerije](https://gallery.cortanaintelligence.com/). Galerija pokriva gama eksperimenata i omogućuje temeljito Uvod u raspon mogućnosti Azure strojnog učenja.

## <a name="license-information"></a>Informacije o licenci

Ovaj vodič za uzorak i njegov popratnu skripte zajednički koriste Microsoft u odjeljku MIT licenca. Provjerite datoteke LICENSE.txt u direktoriju uzorak koda na GitHub više pojedinosti.

## <a name="references"></a>Reference

• [Andrés Monroy NEW taksi Trips stranici za preuzimanje](http://www.andresmh.com/nyctaxitrips/)  
• [FOILing NEW-taksi putovanja podataka po Chris Whong](http://chriswhong.com/open-data/foil_nyc_taxi/)   
• [Taksi SEBI i Limousine provizije Istraživanje te statistike](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)


[2]: ./media/machine-learning-data-science-process-hive-walkthrough/output-hive-results-3.png
[11]: ./media/machine-learning-data-science-process-hive-walkthrough/hive-reader-properties.png
[12]: ./media/machine-learning-data-science-process-hive-walkthrough/binary-classification-training.png
[13]: ./media/machine-learning-data-science-process-hive-walkthrough/create-scoring-experiment.png
[14]: ./media/machine-learning-data-science-process-hive-walkthrough/binary-classification-scoring.png
[15]: ./media/machine-learning-data-science-process-hive-walkthrough/amlreader.png

<!-- Module References -->
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
