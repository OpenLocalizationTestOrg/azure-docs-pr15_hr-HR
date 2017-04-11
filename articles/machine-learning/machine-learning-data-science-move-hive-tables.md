<properties
    pageTitle="Stvaranje i podatke učitali u grozd tablice iz spremišta blobova | Microsoft Azure"
    description="Stvaranje tablice grozd i učitavanje podataka u blob za vrste hive tablice"
    services="machine-learning,storage"
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
    ms.date="09/14/2016"
    ms.author="bradsev" />


#<a name="create-and-load-data-into-hive-tables-from-azure-blob-storage"></a>Stvaranje i podatke učitali u grozd tablice iz blobova platforme Azure

U ovoj se temi predstavlja generički grozd upiti koji stvorite grozd tablice i učitavanje podataka iz spremišta blobova platforme Azure. Nekoliko smjernica i navedeni su na particija grozd tablice i korištenju na Optimizirano retka stupčastu (ORC) oblikovanja da biste poboljšali performanse upita.

U ovom **izborniku** navode veze na temu koja opisuje kako ingest podatke u ciljnu okruženja mjesto spremanja i obrađuju tijekom u timu podataka znanstvenog procesa (TDSP) podatke.

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]


## <a name="prerequisites"></a>Preduvjeti
U ovom se članku pretpostavlja da imate:

* Stvorili račun Azure prostora za pohranu. Ako vam je potrebna upute, pročitajte članak [o Azure prostora za pohranu računi](../storage/storage-create-storage-account.md). 
* Dodjeli prilagođene klaster Hadoop sa servisom HDInsight.  Ako vam je potrebna upute, potražite u članku [Prilagodba Hadoop Azure HDInsight klastere za napredne analize](machine-learning-data-science-customize-hadoop-cluster.md).
* Omogućeno daljinski pristup klaster, prijavljen i otvoriti Hadoop konzole za naredbenog retka. Ako vam je potrebna upute, potražite u članku [pristup glave čvor od Hadoop klaster](machine-learning-data-science-customize-hadoop-cluster.md#headnode).

## <a name="upload-data-to-azure-blob-storage"></a>Prijenos podataka u spremište blobova platforme Azure
Ako ste stvorili Azure virtualnog računala slijedeći upute u [Postavljanje Azure virtualnog računala za napredne analize](machine-learning-data-science-setup-virtual-machine.md), ova skriptna datoteka treba preuzet na na *C:\\korisnika\\\<korisničko ime\>\\dokumenata\\podataka znanstvenog skripte* imeničkog na virtualnog računala. Ove vrste Hive upite samo zahtijevaju priključite u vlastitu shemu podataka i konfiguracija spremište blobova platforme Azure u odgovarajućim poljima bude spremna za slanje.

Pretpostavimo da podaci za grozd tablice u tabličnom obliku **koje nisu komprimirane** , a da podatke prenesen na zadane postavke (ili dodatni) spremnik račun za pohranu koji se koristi Hadoop klaster.

Ako želite vježbe na **SEBI taksi putovanja podataka**, morate:

- **Preuzmite** na 24 [NEW taksi putovanja podatkovne](http://www.andresmh.com/nyctaxitrips) datoteke (12 put datoteke i datoteke 12 prijevoz),
- **raspakiraj** sve datoteke u .csv datoteke, a zatim
- **Prijenos** da zadani (ili odgovarajuće spremnik) Azure prostora za pohranu računa koji je stvoren pomoću postupka strukturiranih u temi [prilagoditi Hadoop Azure HDInsight klastere za napredne analize postupka i tehnologije](machine-learning-data-science-customize-hadoop-cluster.md) . Postupak da biste prenijeli datoteke .csv spremniku zadani na računu za pohranu pronaći ćete na ovoj [stranici](machine-learning-data-science-process-hive-walkthrough.md#upload).


## <a name="submit"></a>Kako poslati grozd upita

Pomoću mogu poslati grozd upita:

1. [Slanje upita grozd kroz Hadoop naredbenog retka u headnode Hadoop klaster](#headnode)
2. [Slanje upita grozd pomoću uređivača vrste Hive](#hive-editor)
3. [Slanje upita grozd pomoću naredbe ljuske PowerShell za Azure](#ps)

Upiti grozd su nalik SQL. Ako ste upoznati s SQL, vidjet ćete [vrste Hive SQL Cheat lista korisnika](http://hortonworks.com/wp-content/uploads/2013/05/hql_cheat_sheet.pdf) korisno.

Prilikom slanja upita grozd, možete kontrolirati Odredišni izlaz iz upita grozd, hoće li se na zaslonu ili lokalna datoteka na glavni čvor ili blobova platforme Azure.


###<a name="headnode"></a>1. slati upiti grozd kroz Hadoop naredbenog retka u headnode Hadoop klaster

Ako je upit grozd složene, slanje izravno u glavni čvor u Hadoop klaster obično potencijalnih klijenata za brže uključiti od slanja vrste Hive uređivač ili Azure PowerShell skripti.

Prijavite se na čvor glavni klaster Hadoop, otvorite naredbeni redak Hadoop na radnoj površini čvor glavni pa unesite naredba `cd %hive_home%\bin`.

Postoje tri načina slanja grozd upita u Hadoop naredbenog retka:

* izravno
* korištenje .hql datoteka
* s konzole za grozd

#### <a name="submit-hive-queries-directly-in-hadoop-command-line"></a>Slanje upita grozd izravno u programu Hadoop naredbenog retka.

Naredbu kao što su možete pokrenuti `hive -e "<your hive query>;` slanje jednostavne upite grozd izravno u programu Hadoop naredbenog retka. Slijedi primjer gdje crvenim okvirom navode naredbu koja se šalje grozd upita i zeleni okvir navode Izlaz iz grozd upita.

![Stvaranje radnog prostora](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-1.png)

#### <a name="submit-hive-queries-in-hql-files"></a>Slanje upita grozd u .hql datoteke

Kada upit grozd složenije i sadrži više redaka, uređivanje upita u naredbeni redak ili grozd konzole nije praktično. Alternative jest korištenje uređivaču teksta u čvor glavni klaster Hadoop da biste spremili grozd upita u datoteci .hql u lokalnom direktoriju glavni čvor. Zatim grozd upita u datoteci .hql mogu poslati pomoću na `-f` argument na sljedeći način:

    hive -f "<path to the .hql file>"

![Stvaranje radnog prostora](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-3.png)


**Izostavlja tijeku status zaslona ispis grozd upita**

Prema zadanim postavkama, kada se upit grozd šalje se u Hadoop naredbenog retka tijek zadatka karte/smanjivanje je ispisati na zaslonu. Izostavlja ispis zaslona napretka posla karte/smanjivanje, možete koristiti argument `-S` ("S" velikim slovima) u naredbi retka na sljedeći način:

    hive -S -f "<path to the .hql file>"
    hive -S -e "<Hive queries>"

#### <a name="submit-hive-queries-in-hive-command-console"></a>Slanje grozd upita u grozd konzolu.

Možete i najprije unijeti grozd konzolu za upravljanje tako da pokrenete naredbu `hive` u Hadoop naredbenog retka i slanje grozd upita u grozd konzolu. Evo primjera. U ovom primjeru dva okvira crvena istaknuli naredbi koje se koriste za unos grozd konzolu za upravljanje i upit grozd odnosno poslan konzole za grozd. Zeleni okvir ističe Izlaz iz grozd upita.

![Stvaranje radnog prostora](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-2.png)

Prijašnjih primjera izravno izlaz grozd rezultate upita na zaslonu. U lokalnu datoteku možete napisati Izlaz na glavni čvor ili blobova platforme Azure. Nakon toga možete koristiti druge alate radi daljnje analize izlaz upita grozd.

**Izlaz grozd rezultate upita u lokalnu datoteku.**

Da biste izlaz grozd rezultate upita za lokalni direktorij na glavni čvor morate poslati upit grozd u Hadoop naredbenog retka na sljedeći način:

    hive -e "<hive query>" > <local path in the head node>

U sljedećem primjeru rezultat upita grozd je zapisan u datoteku `hivequeryoutput.txt` u direktoriju `C:\apps\temp`.

![Stvaranje radnog prostora](./media/machine-learning-data-science-move-hive-tables/output-hive-results-1.png)

**Izlaz grozd rezultate upita za blobova platforme Azure**

Možete poslati i grozd rezultate upita za Azure blob, unutar zadanog spremnik klaster Hadoop. Grozd upit za to je na sljedeći način:

    insert overwrite directory wasb:///<directory within the default container> <select clause from ...>

U sljedećem primjeru izlaz upita grozd napisan direktorij blob `queryoutputdir` unutar zadanog spremnik klaster Hadoop. Ovdje, što je potrebno za davanje naziva direktorija, bez naziva blob. Pojavila se pogreška ako se Expression.Error unesete direktorija i blob imena, kao što su `wasb:///queryoutputdir/queryoutput.txt`.

![Stvaranje radnog prostora](./media/machine-learning-data-science-move-hive-tables/output-hive-results-2.png)

Ako otvorite spremnik zadani klaster Hadoop pomoću programa Explorer Azure prostora za pohranu, vidjet ćete izlaz upita grozd kao što je prikazano na sljedećoj slici. Filtar (istaknuto crvenim okvirom) možete primijeniti samo dohvatiti blob s navedena slova u nazivima.

![Stvaranje radnog prostora](./media/machine-learning-data-science-move-hive-tables/output-hive-results-3.png)

###<a name="hive-editor"></a>2. slati upiti grozd pomoću uređivača vrste Hive

Konzola za upit (vrste Hive uređivač) možete koristiti i unosom URL-a obrasca *https://&#60; Naziv klaster Hadoop >.azurehdinsight.net/Home/HiveEditor* u web-pregledniku. Morate biti prijavljeni vidi konzole i tako da vam je potrebno vjerodajnice Hadoop klaster ovdje.

###<a name="ps"></a>3. slati upiti grozd pomoću naredbe ljuske PowerShell za Azure

Da biste poslali grozd upite možete koristiti i PowerShell. Upute potražite u članku [Slanje vrste Hive zadatke pomoću komponente PowerShell](../hdinsight/hdinsight-submit-hadoop-jobs-programmatically.md#hive-powershell).


## <a name="create-tables"></a>Stvaranje baze podataka grozd i tablice

Upiti grozd se zajednički koriste u [spremištu Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_db_tbls_load_data_generic.hql) , a možete preuzeti iz nje.

Evo grozd upit koji stvara tablicu vrste Hive.

    create database if not exists <database name>;
    CREATE EXTERNAL TABLE if not exists <database name>.<table name>
    (
        field1 string,
        field2 int,
        field3 float,
        field4 double,
        ...,
        fieldN string
    )
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' lines terminated by '<line separator>'
    STORED AS TEXTFILE LOCATION '<storage location>' TBLPROPERTIES("skip.header.line.count"="1");

Slijede opisi polja koja ćete morati uključiti i druge konfiguracije:

- **& #60; naziv baze podataka >**: naziv baze podataka koju želite stvoriti. Ako samo želite li koristiti zadanu bazu podataka, upit na *Stvaranje baze podataka...* može biti ispušten.
- **& #60; naziv tablice >**: naziv tablice u kojoj želite stvoriti u navedenoj bazi podataka. Ako želite koristiti zadane baze podataka u tablici možete se izravno poziva *& #60; naziv tablice >* bez & #60; naziv baze podataka >.
- **& #60; razdjelnik polja >**: razdjelnik koji razgraničava polja u podatkovnu datoteku koja se prenose u tablicu vrste Hive.
- **& #60; crte razdjelnika >**: razdjelnik koji razgraničava redaka u podatkovnoj tablici.
- **& #60; mjesto za spremanje >**: mjesto Azure prostora za pohranu za spremanje podataka grozd tablice. Ako ne navedete *mjesto & #60; mjesto za spremanje >*, tablice i baza podataka spremaju se u *grozd/skladištu/* imeničkog u zadani spremnik klaster grozd prema zadanim postavkama. Ako želite da biste odredili mjesto za pohranu, mjesto za pohranu mora biti unutar zadanog kontejnera za baze podataka i tablice. Tom mjestu ne može se nazivaju mjesto ovisno o kontejneru zadani klaster u obliku *' wasb: / / / & #60; imenika 1 > /'* ili *' wasb: / / / & #60; imenika 1 > / & #60; direktorija 2 > /'*, itd. Kada se izvršava upit, relativnih direktorija stvaraju se unutar kontejnera zadani.
- **TBLPROPERTIES("Skip.Header.line.Count"="1")**: Ako je podatkovnoj datoteci redak zaglavlja, morate dodati ovo svojstvo **na kraju** upita za *Stvaranje tablice* . U suprotnom se učitava redak zaglavlja kao zapisa u tablicu. Ako podatkovne datoteke u redak zaglavlja, tu konfiguraciju može biti ispušten u upitu.

## <a name="load-data"></a>Učitavanje podataka u grozd tablice
Evo grozd upita koja učitava podatke u tablicu vrste Hive.

    LOAD DATA INPATH '<path to blob data>' INTO TABLE <database name>.<table name>;

- **& #60; put do blob podataka >**: Ako je datoteka blob prenijeti tablicu vrste Hive u zadanom spremnik klaster HDInsight Hadoop u *& #60; put do blob podataka >* mora biti u obliku *' wasb: / / / & #60; direktorija u ovom spremniku > / & #60; blob naziv datoteke >'*. Blob datoteka može biti u dodatne spremnik klaster HDInsight Hadoop. U ovom slučaju *& #60; put do blob podataka >* mora biti u obliku *' wasb: / / & #60; spremnik name>@&#60;storage naziv računa >.blob.core.windows.net/ & #60; blob naziv datoteke >'*.

    >[AZURE.NOTE] Blob podatke koje želite prenijeti tablicu vrste Hive mora biti u zadane ili dodatnog kontejnera računa za pohranu za klaster Hadoop. U suprotnom upit *UČITAVANJA podataka* ne uspije gdje ga ne može pristupiti podacima.


## <a name="partition-orc"></a>Dodatne teme: particije tablice i spremište grozd podatke u obliku ORC

Ako su podaci velik, particija tablici korisno je upite za koje je potrebno pregledati nekoliko particije tablice. Ako, primjerice, je pametnije je particija podaci zapisnika web-mjesta po datumima.

Osim particija grozd tablice, dobro je i radi pohrane podataka grozd u obliku Optimizirano retka stupčastu (ORC). Dodatne informacije o oblikovanju ORC potražite u članku <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC#LanguageManualORC-ORCFiles" target="_blank">pomoću ORC datotekama poboljšavaju performanse kada je za čitanje, pisanje i obrada podataka</a>.

### <a name="partitioned-table"></a>Particioniranom tablice
Evo grozd upit koji stvara particioniranom tablicu i učitava podataka u njega.

    CREATE EXTERNAL TABLE IF NOT EXISTS <database name>.<table name>
    (field1 string,
    ...
    fieldN string
    )
    PARTITIONED BY (<partitionfieldname> vartype) ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>'
         lines terminated by '<line separator>' TBLPROPERTIES("skip.header.line.count"="1");
    LOAD DATA INPATH '<path to the source file>' INTO TABLE <database name>.<partitioned table name>
        PARTITION (<partitionfieldname>=<partitionfieldvalue>);

Prilikom postavljanja upita particioniranom tablice, preporučuje se da biste dodali uvjet particije na **Početak** u `where` uvjet kao to poboljšava efficacy znatno pretraživanja.

    select
        field1, field2, ..., fieldN
    from <database name>.<partitioned table name>
    where <partitionfieldname>=<partitionfieldvalue> and ...;

### <a name="orc"></a>Spremanje grozd podataka u obliku ORC

Ne možete izravno učitavanje podataka iz spremišta blobova u grozd tablice koja je spremljena u obliku ORC. Ovdje su navedeni koraci koje koje morate poduzeti da biste učitali podatke iz Azure blob polja tablice grozd spremljene u obliku ORC.

Stvaranje programa vanjska tablica **POHRANJENE kao TEXTFILE** i učitavanje podataka iz spremišta blobova na tablicu.

        CREATE EXTERNAL TABLE IF NOT EXISTS <database name>.<external textfile table name>
        (
            field1 string,
            field2 int,
            ...
            fieldN date
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>'
            lines terminated by '<line separator>' STORED AS TEXTFILE
            LOCATION 'wasb:///<directory in Azure blob>' TBLPROPERTIES("skip.header.line.count"="1");

        LOAD DATA INPATH '<path to the source file>' INTO TABLE <database name>.<table name>;

Stvaranje internog tablice s na istu shemu kao vanjska tablica u koraku 1, s istom graničnik i pohrane podataka grozd u obliku ORC.

        CREATE TABLE IF NOT EXISTS <database name>.<ORC table name>
        (
            field1 string,
            field2 int,
            ...
            fieldN date
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' STORED AS ORC;

Odaberite podatke iz vanjske tablice u koraku 1 i umetnite u tablici ORC

        INSERT OVERWRITE TABLE <database name>.<ORC table name>
            SELECT * FROM <database name>.<external textfile table name>;

>[AZURE.NOTE] Ako tablica TEXTFILE *& #60; naziv baze podataka >. & #60; vanjski textfile naziv tablice >* sadrži particije u KORAKU 3 u `SELECT * FROM <database name>.<external textfile table name>` naredba odabire varijabla particije kao polja u vraćenom skupa podataka. Umetanje u na *& #60; naziv baze podataka >. & #60; ORC naziv tablice >* prekida se nakon *& #60; naziv baze podataka >. & #60; ORC naziv tablice >* nema varijabla particije kao polje u shemi tablice. U ovom slučaju morate izričito odabrati polja koja želite da se umeće *& #60; naziv baze podataka >. & #60; ORC naziv tablice >* na sljedeći način:

        INSERT OVERWRITE TABLE <database name>.<ORC table name> PARTITION (<partition variable>=<partition value>)
           SELECT field1, field2, ..., fieldN
           FROM <database name>.<external textfile table name>
           WHERE <partition variable>=<partition value>;

Je sigurno ispustite na *& #60; vanjski textfile naziv tablice >* prilikom korištenja sljedeći upit nakon sve podatke umetnut u *& #60; naziv baze podataka >. & #60; ORC naziv tablice >*:

        DROP TABLE IF EXISTS <database name>.<external textfile table name>;

Nakon slijedite ovaj postupak, trebali biste dobiti tablice s podacima u obliku ORC spreman za korištenje.  
