<properties
    pageTitle="Proces znanstvenog timu podataka u akciji: pomoću SQL Data Warehouse | Microsoft Azure"
    description="Postupak naprednom Analitikom i tehnologija u akciji"  
    services="machine-learning"
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
    ms.date="06/24/2016"
    ms.author="bradsev;hangzh;weig"/>


# <a name="the-team-data-science-process-in-action-using-sql-data-warehouse"></a>Proces znanstvenog timu podataka u akciji: pomoću SQL Data Warehouse

U ovom ćete praktičnom vodiču vodit ćemo vas kroz stvaranje i implementacija strojnog učenja modela pomoću SQL Data Warehouse (SQL DW) za javno dostupna dataset – [NEW taksi Trips](http://www.andresmh.com/nyctaxitrips/) skupu podataka. Binarni klasifikacija model konstruirana predviđa hoće li plaćenu savjeta za putovanje i modelima multiclass klasifikaciju i regresije su i spominju koji predviđanje distribucije za iznose savjet plaćenu.

Postupak slijedi tijeka rada s [Tim podacima znanstvenog procesa (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) . Pokazat ćemo postavljanje znanstvenog okruženju podatke učitali podatke u SQL DW i kako koristiti SQL DW ili pak IPython bilježnicu za istraživanje podataka i inženjeringom značajke model. Zatim Pokazat ćemo upute za stvaranje i implementaciju model s Azure strojnog učenja.


## <a name="dataset"></a>Skup podataka Trips taksi SEBI

Podaci NEW taksi putovanja sastoji se od otprilike 20GB komprimirane CSV datoteke (dekomprimirati ~ 48GB), više od 173 milijuna pojedinačne trips i na fares snimki platiti za svaki put. Svaki zapis putovanja obuhvaća mjesta web-mjesto prijem i u okvir za predaju i vremena, anonymized hack (upravljačkog programa) broj licenci i broj medallion (taksi na jedinstveni ID-ja). Podaci pokriva sve trips u godini 2013 uvjet, a u sljedeća dva skupova podataka za svaki mjesec:

1. Datoteka **trip_data.csv** sadrži detalja putovanja, kao što su broj passengers, prijem i dropoff točaka, trajanja putovanja, a duljina putovanja. Evo nekoliko uzoraka zapisa:

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868

2. Datoteka **trip_fare.csv** sadrži detalje o prijevoz platiti za svaki put, kao što je način plaćanja, prijevoz iznos, dodatna naknada i poreza, savjete i tolls, i ukupan iznos plaćen. Evo nekoliko uzoraka zapisa:

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

**Jedinstveni ključ** za uključivanje putovanja\_podataka i putovanja\_prijevoz sastoji se od sljedeća tri polja:

- medallion,
- probiti\_licence i
- prijem\_datuma i vremena.

## <a name="mltasks"></a>Tri vrste predviđanje zadataka

Ne možemo formulate tri predviđanje poteškoća na temelju na *Savjet\_iznos* za ilustraciju tri vrste Modeliranje zadataka:

1. **Binarni klasifikacija**: predviđanje li Savjet je platili za putovanja, odnosno na *Savjet\_iznos* koji je veći od 0 kn je pozitivan primjer, dok u *Savjet\_iznos* od 0 kn je negativan primjer.

2. **Multiclass klasifikacije**: za predviđanje raspon savjet plaćenu na putovanja. Ćemo podijeliti na *Savjet\_iznos* u pet intervale ili klasa:

        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20

3. **Zadatak regresije**: za predviđanje količinu savjet platiti za putovanje.  


## <a name="setup"></a>Postavljanje okruženja znanstvenog Azure podataka za napredne analize

Da biste postavili okruženje za Azure podataka znanstvenog, slijedite ove korake.

**Stvaranje vlastite računa spremišta blobova platforme Azure**

- Prilikom dodjele resursa vlastite blobova platforme Azure odaberite zemlj.-lokaciju svoje blobova platforme Azure u ili što bliže moguće **Jug središnje SAD -a**, što je gdje NEW taksi pohrane podataka. Podaci će se kopirati pomoću AzCopy iz spremnik za pohranu javno blob spremnika u račun za pohranu. Što bliže Azure blobova Jug središnje NAM, brže izvršiti taj zadatak (korak 4).
- Da biste stvorili račun Azure prostora za pohranu, slijedite korake navedene na [račune za pohranu o Azure](../storage/storage-create-storage-account.md). Obavezno bilješke na vrijednosti za sljedeća vjerodajnice za pohranu računa će biti potrebno kasnije u ovom vodiču.

  - **Naziv računa spremišta**
  - **Ključ za račun za pohranu**
  - **Naziv spremnika** (koji želite da se podaci koji se spremaju u spremište blobova platforme Azure)

**Dodjela vašoj instanci Azure SQL DW.**
Slijedite dokumentaciji na [Stvaranje SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md) Dodjela instancu komponente SQL Data Warehouse. Obavezno provjerite notacije na sljedeće SQL Data Warehouse vjerodajnice koje će se koristiti u noviji korake.

  - **Naziv poslužitelja**: <server Name>. database.windows.net
  - **Naziv SQLDW (baza podataka)**
  - **Korisničko ime**
  - **Lozinke**

**Instalirajte Visual Studio 2015 i SQL Server Data Tools.** Upute potražite u članku [Instalacija Visual Studio 2015 i/ili SSDT (SQL Server Data Tools) za SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-install-visual-studio.md).

**Povežite se s vašeg DW Azure SQL s Visual Studio.** Upute potražite u članku korake 1 i 2 u [Povezivanje s Data Warehouse SQL Azure s Visual Studio](../sql-data-warehouse/sql-data-warehouse-connect-overview.md).

>[AZURE.NOTE] Da biste **stvorili glavnog ključa**se izvoditi sljedeće SQL upita na bazu podataka koju ste stvorili u SQL Data Warehouse (umjesto upita u-korak 3 od tema povezivanje).

    BEGIN TRY
           --Try to create the master key
        CREATE MASTER KEY
    END TRY
    BEGIN CATCH
           --If the master key exists, do nothing
    END CATCH;

**Stvaranje Azure strojnog učenja radnog prostora u odjeljku pretplate Azure.** Upute potražite u članku [Stvaranje radnog prostora programa Azure strojnog učenja](machine-learning-create-workspace.md).

## <a name="getdata"></a>Učitavanje podatke u SQL Data Warehouse

Otvorite konzole za Windows PowerShell. Pokrenite sljedeće komponente PowerShell naredbe da biste preuzeli primjer SQL skripte datoteke koje dijelimo s vama na Github na lokalnom direktoriju koji ste naveli s parametrom *– DestDir*. Promijenite vrijednost parametra *– DestDir* sve lokalnom direktoriju. Ako ne postoji *- DestDir* , stvorit će se tako da skriptu PowerShell.

>[AZURE.NOTE] Bilo bi dobro da biste **Pokreni kao Administrator** prilikom izvršavanja sljedeću skriptu komponente PowerShell ako direktorija *DestDir* potrebne administratorske ovlasti da biste stvorili ili pisati u nju.

    $source = "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/Download_Scripts_SQLDW_Walkthrough.ps1"
    $ps1_dest = "$pwd\Download_Scripts_SQLDW_Walkthrough.ps1"
    $wc = New-Object System.Net.WebClient
    $wc.DownloadFile($source, $ps1_dest)
    .\Download_Scripts_SQLDW_Walkthrough.ps1 –DestDir 'C:\tempSQLDW'

Nakon uspješne izvođenja aktivni radni imenik mijenja *– DestDir*. Trebali biste moći vidjeti zaslona kao što su ispod:

![][19]

U vašem *-DestDir*izvršavanje sljedeću skriptu komponente PowerShell u načinu rada za administratore:

    ./SQLDW_Data_Import.ps1

Kada se pokrene skriptu PowerShell prvi put, koji će se tražiti da podatke iz svoje DW SQL Azure i računa za spremište blobova platforme Azure za unos. Kada se dovrši ovu skriptu PowerShell radi prvi put, vjerodajnice unos koji će napisan na konfiguracijska datoteka SQLDW.conf u direktoriju prikazivanja radne. Buduća Pokreni ovaj skriptna datoteka ljuske PowerShell sadrži mogućnost da biste pročitali sve potrebne parametara iz ove konfiguracije datoteke. Ako morate promijeniti neke parametre, možete odabrati da biste ulazne parametre na zaslonu nakon upit tako da izbrišete datoteku konfiguracije a unosu vrijednosti parametara prikazanim ili promijenili vrijednosti parametara tako da uredite datoteku SQLDW.conf u direktoriju *- DestDir* .

>[AZURE.NOTE] Da biste izbjegli sukobe naziv sheme s onima koje već postoje u vašem DW SQL Azure prilikom čitanja parametara izravno iz datoteke SQLDW.conf, slučajni broj 3 znamenke dodaje se naziv sheme iz datoteke SQLDW.conf kao zadani naziv sheme za svaki Pokreni. Skriptu PowerShell može vam poslati upit za naziv sheme: naziv može biti naveden u diskretni korisnika.

Datoteka **skripte komponente PowerShell** izvršava sljedeće zadatke:

- **Preuzima i instalira AzCopy**, ako AzCopy već nije instaliran

        $AzCopy_path = SearchAzCopy
        if ($AzCopy_path -eq $null){
            Write-Host "AzCopy.exe is not found in C:\Program Files*. Now, start installing AzCopy..." -ForegroundColor "Yellow"
            InstallAzCopy
            $AzCopy_path = SearchAzCopy
        }
            $env_path = $env:Path
            for ($i=0; $i -lt $AzCopy_path.count; $i++){
                if ($AzCopy_path.count -eq 1){
                    $AzCopy_path_i = $AzCopy_path
                } else {
                    $AzCopy_path_i = $AzCopy_path[$i]
                }
                if ($env_path -notlike '*' +$AzCopy_path_i+'*'){
                    Write-Host $AzCopy_path_i 'not in system path, add it...'
                    [Environment]::SetEnvironmentVariable("Path", "$AzCopy_path_i;$env_path", "Machine")
                    $env:Path = [System.Environment]::GetEnvironmentVariable("Path","Machine")
                    $env_path = $env:Path
                }

- **Kopije podataka na račun za pohranu privatne blob** iz javne blob s AzCopy

        Write-Host "AzCopy is copying data from public blob to yo storage account. It may take a while..." -ForegroundColor "Yellow"
        $start_time = Get-Date
        AzCopy.exe /Source:$Source /Dest:$DestURL /DestKey:$StorageAccountKey /S
        $end_time = Get-Date
        $time_span = $end_time - $start_time
        $total_seconds = [math]::Round($time_span.TotalSeconds,2)
        Write-Host "AzCopy finished copying data. Please check your storage account to verify." -ForegroundColor "Yellow"
        Write-Host "This step (copying data from public blob to your storage account) takes $total_seconds seconds." -ForegroundColor "Green"


- **Opterećenje podataka pomoću Polybase (izvršavanjem LoadDataToSQLDW.sql) da biste svoje DW SQL Azure** s računa za pohranu privatne blob pomoću sljedeće naredbe.

    - Stvori shemu

            EXEC (''CREATE SCHEMA {schemaname};'');

    - Stvaranje baze podataka iz djelokruga vjerodajnica

            CREATE DATABASE SCOPED CREDENTIAL {KeyAlias}
            WITH IDENTITY = ''asbkey'' ,
            Secret = ''{StorageAccountKey}''

    - Stvaranje vanjskog izvora podataka za programa Azure spremište blobova platforme

            CREATE EXTERNAL DATA SOURCE {nyctaxi_trip_storage}
            WITH
            (
                TYPE = HADOOP,
                LOCATION =''wasbs://{ContainerName}@{StorageAccountName}.blob.core.windows.net'',
                CREDENTIAL = {KeyAlias}
            )
            ;

            CREATE EXTERNAL DATA SOURCE {nyctaxi_fare_storage}
            WITH
            (
                TYPE = HADOOP,
                LOCATION =''wasbs://{ContainerName}@{StorageAccountName}.blob.core.windows.net'',
                CREDENTIAL = {KeyAlias}
            )
            ;

    - Stvaranje vanjskom formatu datoteke za csv datoteke. Podaci koje nisu komprimirane, a polja odvojene znakom kanala.

            CREATE EXTERNAL FILE FORMAT {csv_file_format}
            WITH
            (   
                FORMAT_TYPE = DELIMITEDTEXT,
                FORMAT_OPTIONS  
                (
                    FIELD_TERMINATOR ='','',
                    USE_TYPE_DEFAULT = TRUE
                )
            )
            ;

    - Stvaranje vanjske prijevoz i putovanja tablice za skup podataka taksi NEW u spremište blobova platforme Azure.

            CREATE EXTERNAL TABLE {external_nyctaxi_fare}
            (
                medallion varchar(50) not null,
                hack_license varchar(50) not null,
                vendor_id char(3),
                pickup_datetime datetime not null,
                payment_type char(3),
                fare_amount float,
                surcharge float,
                mta_tax float,
                tip_amount float,
                tolls_amount float,
                total_amount float
            )
            with (
                LOCATION    = ''/nyctaxifare/'',
                DATA_SOURCE = {nyctaxi_fare_storage},
                FILE_FORMAT = {csv_file_format},
                REJECT_TYPE = VALUE,
                REJECT_VALUE = 12     
            )  


            CREATE EXTERNAL TABLE {external_nyctaxi_trip}
            (
                medallion varchar(50) not null,
                hack_license varchar(50)  not null,
                vendor_id char(3),
                rate_code char(3),
                store_and_fwd_flag char(3),
                pickup_datetime datetime  not null,
                dropoff_datetime datetime,
                passenger_count int,
                trip_time_in_secs bigint,
                trip_distance float,
                pickup_longitude varchar(30),
                pickup_latitude varchar(30),
                dropoff_longitude varchar(30),
                dropoff_latitude varchar(30)
            )
            with (
                LOCATION    = ''/nyctaxitrip/'',
                DATA_SOURCE = {nyctaxi_trip_storage},
                FILE_FORMAT = {csv_file_format},
                REJECT_TYPE = VALUE,
                REJECT_VALUE = 12         
            )

    - Učitavanje podataka iz vanjske tablica u spremište blobova platforme Azure SQL Data Warehouse

            CREATE TABLE {schemaname}.{nyctaxi_fare}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            SELECT *
            FROM   {external_nyctaxi_fare}
            ;

            CREATE TABLE {schemaname}.{nyctaxi_trip}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            SELECT *
            FROM   {external_nyctaxi_trip}
            ;

    - Stvaranje podatkovne tablice uzorka (NYCTaxi_Sample) i umetanje podataka da biste ga s odabirom SQL upita na putovanja i prijevoz tablicama. (Nekoliko koraka u ovom vodiču treba koristiti ovaj primjer tablice.)

            CREATE TABLE {schemaname}.{nyctaxi_sample}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            (
                SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount, f.total_amount, f.tip_amount,
                tipped = CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END,
                tip_class = CASE
                        WHEN (tip_amount = 0) THEN 0
                        WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
                        WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
                        WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
                        ELSE 4
                    END
                FROM {schemaname}.{nyctaxi_trip} t, {schemaname}.{nyctaxi_fare} f
                WHERE datepart("mi",t.pickup_datetime) = 1
                AND t.medallion = f.medallion
                AND   t.hack_license = f.hack_license
                AND   t.pickup_datetime = f.pickup_datetime
                AND   pickup_longitude <> ''0''
                AND   dropoff_longitude <> ''0''
            )
            ;

Zemljopisnu lokaciju svoje račune za pohranu utječe učitava.

>[AZURE.NOTE] Ovisno o geografski vašeg računa za pohranu privatne blob, postupak kopiranje podataka iz javne blob na svoj račun privatnih prostora za pohranu može potrajati otprilike 15 minuta ili čak i produljili i postupak učitavanja podataka s računa za pohranu na DW SQL Azure može potrajati 20 minuta ili dulje.  

Morat ćete odlučiti što želite ako imate duplicirane izvorišne i odredišne datoteke.

>[AZURE.NOTE] Ako .csv datoteke iz kojeg će se kopirati javno blobova na račun servisa za pohranu privatne blob već postoje u račun za pohranu privatne blob, AzCopy pita želite li ih prebrisati. Ako ne želite ih prebrisati, **n** upit za unos. Ako želite da biste prebrisali **sve** od im, unos **u** upit. Možete i unos **y** da biste prebrisali .csv datoteke pojedinačno.

![Iscrtavanje #21][21]

Možete koristiti na vlastitim podacima. Ako se podaci nalaze na računalu na lokaciji u aplikaciji realni vijek, i dalje možete koristiti AzCopy prijenos podataka na lokaciji na vaše privatne blobova platforme Azure. Što je potrebno da biste promijenili mjesto **izvora** `$Source = "http://getgoing.blob.core.windows.net/public/nyctaxidataset"`, naredba AzCopy datoteke skriptu PowerShell lokalni direktorij koja sadrži podatke.


>[AZURE.TIP] Ako vaši podaci već u svoje privatne blobova platforme Azure u aplikaciji realni vijek, možete preskočiti korak AzCopy u skriptu PowerShell i izravno prijenos podataka Azure SQL DW. To je potrebno dodatne izmjene skriptu da biste prilagodili oblik podataka.


Ovu skriptu Powershell i priključuje Azure SQL DW podatke u primjer datoteke za istraživanje podataka SQLDW_Explorations.sql, SQLDW_Explorations.ipynb i SQLDW_Explorations_Scripts.py tako da ih tri spremni da biste se pokušali odmah nakon dovršetka skriptu PowerShell.

Nakon izvođenja uspješan, prikazat će se zaslon kao što su ispod:

![][20]

## <a name="dbexplore"></a>Istraživanje podataka i značajka inženjering u skladištu podataka za SQL Azure

U ovom ćete odjeljku smo izvođenje Istraživanje podataka i generiranje značajka tako da pokrenete SQL upita na temelju Azure SQL DW izravno pomoću **Visual Studio Tools podataka**. Sve SQL upiti koji se koriste u ovom odjeljku pronaći ćete u ogledne skripte pod nazivom *SQLDW_Explorations.sql*. Datoteka je već preuzeta na lokalnom direktoriju pomoću skripte komponente PowerShell. Može dohvatiti i to iz [Github](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/SQLDW_Explorations.sql). No datoteke u Github nema informacije Azure SQL DW kabel.

Povežite se s vašeg DW SQL Azure pomoću Visual Studio SQL DW korisničko ime i lozinku i otvorite **SQL objekt Explorer** da biste potvrdili baze podataka i tablice uvezete. Dohvaćanje datoteka *SQLDW_Explorations.sql* .

>[AZURE.NOTE] Da biste otvorili uređivač upita za paralelne skladištu podataka (PDW), koristite naredbu **Novi upit** dok je u **Programu Explorer objekta SQL**odabran vaše PDW. PDW ne podržava standardnu SQL uređivač upita.

Slijedi nekoliko vrsta podataka izvođenje Istraživanje i značajka generacije zadataka u ovom odjeljku:

- Istraživanje podataka distribucija nekoliko polja u različitim prozorima vremena.
- Istražite kvalitete podataka dužinu i širinu polja.
- Generiranje binarnim i multiclass klasifikacije natpise na temelju na **Savjet\_iznos**.
- Generiranje značajke i računalnim/Usporedba međusobne putovanja.
- Spajanje dvije tablice i izdvojili slučajni uzorak koji će se koristiti za izradu modela.

### <a name="data-import-verification"></a>Provjera za uvoz podataka

Tih upita omogućuje brzo provjere broj redaka i stupaca u tablicama popunjeno ranije putem Polybase na paralelne skupnog uvoza,

    -- Report number of rows in table <nyctaxi_trip> without table scan
    SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_trip>')

    -- Report number of columns in table <nyctaxi_trip>
    SELECT COUNT(*) FROM information_schema.columns WHERE table_name = '<nyctaxi_trip>' AND table_schema = '<schemaname>'

**Izlaz:** Trebali biste dobiti 173,179,759 redaka i stupaca 14.

### <a name="exploration-trip-distribution-by-medallion"></a>Istraživanje: Putovanja raspodjele po medallion

U ovom primjeru upita označava medallions (taksi brojeve) dovršiti više od 100 trips unutar određenog vremenskog razdoblja. Upit će im particioniranom tablice programa access jer ga je conditioned prema shemi particija **prijem\_datetime**. Slanje upita cijeli skup podataka i provjerite koristite particioniranom tablice i/ili indeksirati pregleda.

    SELECT medallion, COUNT(*)
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    GROUP BY medallion
    HAVING COUNT(*) > 100

**Izlaz:** Upit mora vratiti tablicu s redaka navodeći 13,369 medallions (taxis) i broj putovanja izvršiti tako da ih 2013. Zadnji stupac sadrži zbroj broj trips dovršiti.

### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a>Istraživanje: Putovanja raspodjele medallion i hack_license

U ovom se primjeru označava medallions (taksi brojeve), a hack_license brojeva (upravljačke programe) završio više od 100 trips unutar određenog vremenskog razdoblja.

    SELECT medallion, hack_license, COUNT(*)
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130131'
    GROUP BY medallion, hack_license
    HAVING COUNT(*) > 100

**Izlaz:** Upit mora vratiti tablicu s 13,369 redaka koji određuje 13,369 automobila/upravljački program ID-a koji ste završili rad više tu 100 trips u 2013. Zadnji stupac sadrži zbroj broj trips dovršiti.

### <a name="data-quality-assessment-verify-records-with-incorrect-longitude-andor-latitude"></a>Procjena kvalitete podataka: Provjerite je li zapisa koji sadrže netočan dužini i/ili zemljopisnu širinu

U ovom se primjeru istražuje stanje Ako bilo koji od dužine i/ili zemljopisnu širinu polja ili sadrže vrijednost koja nije valjana (radian stupnjeva mora biti između -90 i 90), ili (0, 0) koordinata.

    SELECT COUNT(*) FROM <schemaname>.<nyctaxi_trip>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(pickup_latitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_latitude AS float) NOT BETWEEN -90 AND 90
    OR    (pickup_longitude = '0' AND pickup_latitude = '0')
    OR    (dropoff_longitude = '0' AND dropoff_latitude = '0'))

**Izlaz:** Upit vraća 837,467 trips koji imaju koji nisu valjani dužini i/ili zemljopisnu širinu polja.

### <a name="exploration-tipped-vs-not-tipped-trips-distribution"></a>Istraživanje: Tipped nasuprot nije zakrenut trips raspodjele

U ovom se primjeru pronalazi broj trips koji su tipped nasuprot broj koji su tipped u određenom vremenskom razdoblju (ili u cijeli skup podataka ako koji prekriva cijelu godinu kao što je postavljen ovdje). Ta se distribucija odražava raspodjele binarni oznaku koja će se koristiti kasnije binarni klasifikacija Modeliranje.

    SELECT tipped, COUNT(*) AS tip_freq FROM (
      SELECT CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped, tip_amount
      FROM <schemaname>.<nyctaxi_fare>
      WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tipped

**Izlaz:** Upit mora vratiti sljedeće učestalosti savjeta za godinu 2013: 90,447,622 tipped i 82,264,709 ne-tipped.

### <a name="exploration-tip-classrange-distribution"></a>Istraživanje: Savjet predmete/raspon raspodjele

U ovom se primjeru formula izračunava raspodjele savjet raspona u određenom vremenskom razdoblju (ili u cijeli skup podataka ako koji prekriva cijelu godinu). Ovo je raspodjele klase oznake koja će se koristiti kasnije multiclass klasifikacija Modeliranje.

    SELECT tip_class, COUNT(*) AS tip_freq FROM (
        SELECT CASE
            WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tip_class

**Izlaz:**

|tip_class  | tip_freq |
| --------- | -------|
|1  | 82230915 |
|2  | 6198803 |
|3  | 1932223 |
|0  | 82264625 |
|4  | 85765 |

### <a name="exploration-compute-and-compare-trip-distance"></a>Istraživanje: Izračun i usporedite udaljenost putovanja

U ovom se primjeru pretvara prijem i predaju dužini i zemljopisnu širinu za SQL Zemljopis točaka, formula izračunava udaljenost putovanja pomoću SQL Zemljopis točke razlika i vraća slučajni uzorak rezultata za usporedbu. U primjeru ograničenjima rezultate u valjani koordinate samo pomoću upita procjenu kvalitete podataka prekriveno neke starije verzije.

    /****** Object:  UserDefinedFunction [dbo].[fnCalculateDistance] ******/
    SET ANSI_NULLS ON
    GO

    SET QUOTED_IDENTIFIER ON
    GO

    IF EXISTS (SELECT * FROM sys.objects WHERE type IN ('FN', 'IF') AND name = 'fnCalculateDistance')
      DROP FUNCTION fnCalculateDistance
    GO

    -- User-defined function to calculate the direct distance  in mile between two geographical coordinates.
    CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)

    RETURNS float
    AS
    BEGIN
        DECLARE @distance decimal(28, 10)
        -- Convert to radians
        SET @Lat1 = @Lat1 / 57.2958
        SET @Long1 = @Long1 / 57.2958
        SET @Lat2 = @Lat2 / 57.2958
        SET @Long2 = @Long2 / 57.2958
        -- Calculate distance
        SET @distance = (SIN(@Lat1) * SIN(@Lat2)) + (COS(@Lat1) * COS(@Lat2) * COS(@Long2 - @Long1))
        --Convert to miles
        IF @distance <> 0
        BEGIN
            SET @distance = 3958.75 * ATAN(SQRT(1 - POWER(@distance, 2)) / @distance);
        END
        RETURN @distance
    END
    GO

    SELECT pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS DirectDistance
    FROM <schemaname>.<nyctaxi_trip>
    WHERE datepart("mi",pickup_datetime)=1
    AND CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND pickup_longitude != '0' AND dropoff_longitude != '0'

### <a name="feature-engineering-using-sql-functions"></a>Značajka inženjering pomoću SQL funkcijama

SQL funkcijama katkad može biti učinkovitog mogućnost inženjering značajke. U ovom vodiču definirali SQL funkciju da biste izračunali Izravni udaljenost između prijem i dropoff mjesta. Možete pokrenuti sljedeći SQL skripte u **Visual Studio Tools podataka**.

Evo SQL skriptu koja definira funkciju udaljenost.

    SET ANSI_NULLS ON
    GO

    SET QUOTED_IDENTIFIER ON
    GO

    IF EXISTS (SELECT * FROM sys.objects WHERE type IN ('FN', 'IF') AND name = 'fnCalculateDistance')
      DROP FUNCTION fnCalculateDistance
    GO

    -- User-defined function calculate the direct distance between two geographical coordinates.
    CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)

    RETURNS float
    AS
    BEGIN
        DECLARE @distance decimal(28, 10)
        -- Convert to radians
        SET @Lat1 = @Lat1 / 57.2958
        SET @Long1 = @Long1 / 57.2958
        SET @Lat2 = @Lat2 / 57.2958
        SET @Long2 = @Long2 / 57.2958
        -- Calculate distance
        SET @distance = (SIN(@Lat1) * SIN(@Lat2)) + (COS(@Lat1) * COS(@Lat2) * COS(@Long2 - @Long1))
        --Convert to miles
        IF @distance <> 0
        BEGIN
            SET @distance = 3958.75 * ATAN(SQRT(1 - POWER(@distance, 2)) / @distance);
        END
        RETURN @distance
    END
    GO

Slijedi primjer da biste nazvali ove funkcije za generiranje značajke u SQL upita:

    -- Sample query to call the function to create features
    SELECT pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS DirectDistance
    FROM <schemaname>.<nyctaxi_trip>
    WHERE datepart("mi",pickup_datetime)=1
    AND CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND pickup_longitude != '0' AND dropoff_longitude != '0'

**Izlaz:** Ovaj upit generira tablicu (2,803,538 recima) s prijem i dropoff latitudes i longitudes i odgovarajuće Izravni udaljenosti u miljama. Evo rezultate za prvo 3 retka:

||pickup_latitude | pickup_longitude    | dropoff_latitude |dropoff_longitude | DirectDistance |
|---| --------- | -------|-------| --------- | -------|
|1  | 40.731804 | -74.001083 | 40.736622 | -73.988953 | .7169601222 |
|2  | 40.715794 | -74,010635 | 40.725338 | -74.00399 | .7448343721 |
|3  | 40.761456 | -73.999886 | 40.766544 | -73.988228 | 0.7037227967 |



### <a name="prepare-data-for-model-building"></a>Priprema podataka za sastavni modela

Sljedeći upit spojevi u **nyctaxi\_putovanja** i **nyctaxi\_prijevoz** tablice, generira u binarni klasifikacije natpis **tipped**, natpis više predmete klasifikacije **Savjet\_klase**, i izdvaja uzorka iz cijelog spojene skup podataka. Za stvaranje uzoraka obavlja preuzimanjem podskup trips na temelju prikupljanja vremena.  Ovaj upit možete kopirati zatim zalijepiti izravno u [Azure strojnog učenja Studio](https://studio.azureml.net) [Uvoz podataka] [ import-data] modul za ingestion Izravni podataka iz baze podataka SQL instanci u Azure. Upit isključuje zapisa koji sadrže netočan (0, 0) koordinata.

    SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,  f.total_amount, f.tip_amount,
        CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped,
        CASE WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM <schemaname>.<nyctaxi_trip> t, <schemaname>.<nyctaxi_fare> f
    WHERE datepart("mi",t.pickup_datetime) = 1
    AND   t.medallion = f.medallion
    AND   t.hack_license = f.hack_license
    AND   t.pickup_datetime = f.pickup_datetime
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'

Kada budete spremni nastaviti Azure strojnog učenja, možda jedan od sljedećih načina:  

1. Spremanje konačnom upitu SQL izdvojiti i uzorak podataka i Kopiraj – Zalijepi upit izravno u [Uvoz podataka] [ import-data] modul u Azure strojnog učenja, ili
2. Zadržava uzorkovanja i izgrađen podataka namjeravate koristiti za model izgradnju u novu tablicu SQL DW i koristiti novu tablicu u [Uvoz podataka] [ import-data] modul u Azure strojnog učenja. Skriptu PowerShell u prethodnom koraku sadrži učinili umjesto vas. Možete pročitati izravno iz ove tablice u modulu za uvoz podataka.


## <a name="ipnb"></a>Istraživanje podataka i značajka inženjering IPython bilježnice

U ovom ćete odjeljku smo će se izvršiti Istraživanje podataka i generiranje značajku pomoću oba Python i SQL upite odabiranja SQL DW ste ranije stvorili. Primjer bilježnice IPython pod nazivom **SQLDW_Explorations.ipynb** i Python skriptna datoteka **SQLDW_Explorations_Scripts.py** preuzet na lokalnom direktoriju. Dostupne su i na [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/SQLDW). Isti u skriptama Python su ove dvije datoteke. Datoteka skripte Python navedeni su vam u slučaju da nemate poslužitelj za IPython bilježnice. U odjeljku **Python 2.7**osmišljeni su ove dvije ogledne Python datoteke.

Potrebne Azure SQL DW informacije u uzorku IPython bilježnice i skriptna datoteka Python preuzeti na lokalno računalo ima je priključen pomoću skripte komponente PowerShell prethodno. To su izvršna bez izmjene.

Ako ste već postavili radnog prostora programa AzureML, možete izravno prijenos uzorka IPython bilježnice sa servisom AzureML IPython bilježnice i pokretanja ga. Evo nekoliko koraka da biste prenijeli na servis AzureML IPython bilježnicu:

1. Prijavite se u svoj AzureML radni prostor, kliknite "Studio" pri vrhu pa kliknite "BILJEŽNICE" na lijevoj strani web-stranice.

    ![Iscrtavanje #22][22]

2. Kliknite "NOVO" u donjem lijevom kutu web-stranice, a zatim odaberite "Python 2". Zatim navedite naziv bilježnice i kliknite kvačicu da biste stvorili novu praznu IPython bilježnicu.

    ![Iscrtavanje #23][23]

3. Kliknite simbol "Jupyter" u lijevom gornjem kutu novu bilježnicu IPython.

    ![Iscrtavanje #24][24]

4. Povucite i ispustite uzorka IPython bilježnicu na stranici **stabla** servisa AzureML IPython bilježnice pa kliknite **Prenesi**. Nakon toga uzorka IPython bilježnica će se prenijeti u servis AzureML IPython bilježnice.

    ![Iscrtavanje #25][25]

Da biste pokrenuli uzorka IPython bilježnice ili na Python skripte datoteku, a zatim sljedeće Python paketa su vam potrebne. Ako koristite servis AzureML IPython bilježnice, te pakete nije instalirana.

    - pandas
    - numpy
    - matplotlib
    - pyodbc
    - PyTables

Preporučena niz prilikom izrade naprednih analitičkih rješenja na AzureML velike podacima je sljedeće:

- Pročitajte u mali uzorak podataka u u okvir podaci u memoriji.
- Izvođenje neke vizualizacije i explorations pomoću sampled podataka.
- Eksperimentirajte s značajka inženjering pomoću sampled podataka.
- Za veće Istraživanje podataka, rukovanje podataka i značajka inženjering koristi Python za izdavanje SQL upitima izravno u odnosu na SQL DW.
- Odlučite veličina uzorka biti prikladna za Azure strojnog učenja modela sastavnih.

Na followings su nekoliko Istraživanje podataka, vizualizaciju podataka i značajka inženjerska primjere. Dodatne podatke explorations pronaći ćete u uzorku IPython bilježnice i oglednu datoteku za skripte Python.

### <a name="initialize-database-credentials"></a>Pokretanje vjerodajnice za bazu podataka

Pokretanje postavke veze baze podataka u sljedeće varijable:

    SERVER_NAME=<server name>
    DATABASE_NAME=<database name>
    USERID=<user name>
    PASSWORD=<password>
    DB_DRIVER = <database driver>

### <a name="create-database-connection"></a>Stvorite vezu s bazom podataka

Ovo je niz za povezivanje stvara vezu s bazom podataka.

    CONNECTION_STRING = 'DRIVER={'+DRIVER+'};SERVER='+SERVER_NAME+';DATABASE='+DATABASE_NAME+';UID='+USERID+';PWD='+PASSWORD
    conn = pyodbc.connect(CONNECTION_STRING)

### <a name="report-number-of-rows-and-columns-in-table-nyctaxitrip"></a>Izvješće broj redaka i stupaca u tablici < nyctaxi_trip >

    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_trip>')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('<nyctaxi_trip>') AND table_schema = ('<schemaname>')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

- Ukupan broj redaka = 173179759  
- Ukupan broj stupaca = 14

### <a name="report-number-of-rows-and-columns-in-table-nyctaxifare"></a>Izvješće broj redaka i stupaca u tablici < nyctaxi_fare >

    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_fare>')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('<nyctaxi_fare>') AND table_schema = ('<schemaname>')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

- Ukupan broj redaka = 173179759  
- Ukupan broj stupaca = 11

### <a name="read-in-a-small-data-sample-from-the-sql-data-warehouse-database"></a>Uzorak small podataka iz baze podataka SQL podataka skladištu u čitanje

    t0 = time.time()

    query = '''
        SELECT TOP 10000 t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax,
            f.tolls_amount, f.total_amount, f.tip_amount
        FROM <schemaname>.<nyctaxi_trip> t, <schemaname>.<nyctaxi_fare> f
        WHERE datepart("mi",t.pickup_datetime) = 1
        AND   t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
    '''

    df1 = pd.read_sql(query, conn)

    t1 = time.time()
    print 'Time to read the sample table is %f seconds' % (t1-t0)

    print 'Number of rows and columns retrieved = (%d, %d)' % (df1.shape[0], df1.shape[1])

Vrijeme da biste pročitali ogledne tablice je 14.096495 sekundi.  
Broj redaka i stupaca dohvaća = (1000 21).

### <a name="descriptive-statistics"></a>Opisna statistika

Sada ste spremni za proučavanje podataka u sampled. Ne možemo počinju pogled na neki Opisna statistika za na **putovanja\_udaljenost** (ili druga polja odaberite da biste odredili).

    df1['trip_distance'].describe()

### <a name="visualization-box-plot-example"></a>Vizualizaciju: Primjer crtanja okvir

Sljedeće smo pogledajte okvirima za putovanja udaljenost vizualizacija u quantiles.

    df1.boxplot(column='trip_distance',return_type='dict')

![Iscrtavanje #1][1]

### <a name="visualization-distribution-plot-example"></a>Vizualizaciju: Primjer crtanja distribucije

Crtanje vizualizacija distribuciju i histogram za međusobne uzorkovanja putovanja.

    fig = plt.figure()
    ax1 = fig.add_subplot(1,2,1)
    ax2 = fig.add_subplot(1,2,2)
    df1['trip_distance'].plot(ax=ax1,kind='kde', style='b-')
    df1['trip_distance'].hist(ax=ax2, bins=100, color='k')

![Iscrtavanje #2][2]

### <a name="visualization-bar-and-line-plots"></a>Vizualizacija: Trakasti i linijski iscrtavaju

U ovom primjeru ćemo intervala udaljenost putovanja u pet intervale i vizualizacija binning rezultate.

    trip_dist_bins = [0, 1, 2, 4, 10, 1000]
    df1['trip_distance']
    trip_dist_bin_id = pd.cut(df1['trip_distance'], trip_dist_bins)
    trip_dist_bin_id

Možemo iscrtati iznad distribucije za smeće na traci ili crtanja s crtom:

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='bar')

![Iscrtavanje #3][3]

i

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='line')

![Iscrtavanje #4][4]

### <a name="visualization-scatterplot-examples"></a>Vizualizacije: Primjeri Scatterplot

Pokazat ćemo raspršeni crtanja između **putovanja\_vrijeme\_u\_sekunde** i **putovanja\_udaljenost** da biste vidjeli postoji li sve korelacije

    plt.scatter(df1['trip_time_in_secs'], df1['trip_distance'])

![Iscrtavanje #6][6]

Na sličan način možete provjeriti odnos između **stopa\_kod** i **putovanja\_udaljenost**.

    plt.scatter(df1['passenger_count'], df1['trip_distance'])

![Iscrtavanje #8][8]


### <a name="data-exploration-on-sampled-data-using-sql-queries-in-ipython-notebook"></a>Istraživanje podataka na ogledne podatke pomoću SQL upitima u bilježnici IPython

U ovom ćete odjeljku ćemo istražiti distribucija podataka pomoću sampled podataka koji je ista i u novu tablicu u koju smo stvorili iznad. Imajte na umu da slične explorations možete izvršiti pomoću izvorna tablica.

#### <a name="exploration-report-number-of-rows-and-columns-in-the-sampled-table"></a>Istraživanje: Izvješće broj redaka i stupaca u tablici sampled

    nrows = pd.read_sql('''SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_sample>')''', conn)
    print 'Number of rows in sample = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''SELECT count(*) FROM information_schema.columns WHERE table_name = ('<nyctaxi_sample>') AND table_schema = '<schemaname>'''', conn)
    print 'Number of columns in sample = %d' % ncols.iloc[0,0]

#### <a name="exploration-tippednot-tripped-distribution"></a>Istraživanje: Tipped/ne tripped distribuciju.

    query = '''
        SELECT tipped, count(*) AS tip_freq
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY tipped
        '''

    pd.read_sql(query, conn)

#### <a name="exploration-tip-class-distribution"></a>Istraživanje: Savjet predmete raspodjele

    query = '''
        SELECT tip_class, count(*) AS tip_freq
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY tip_class
    '''

    tip_class_dist = pd.read_sql(query, conn)

#### <a name="exploration-plot-the-tip-distribution-by-class"></a>Istraživanje: Iscrtati raspodjele savjet po klase

    tip_class_dist['tip_freq'].plot(kind='bar')

![Iscrtavanje #26][26]


#### <a name="exploration-daily-distribution-of-trips"></a>Istraživanje: Dnevni distribucija trips

    query = '''
        SELECT CONVERT(date, dropoff_datetime) AS date, COUNT(*) AS c
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY CONVERT(date, dropoff_datetime)
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-per-medallion"></a>Istraživanje: Putovanja raspodjele po medallion

    query = '''
        SELECT medallion,count(*) AS c
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY medallion
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-by-medallion-and-hack-license"></a>Istraživanje: Putovanja raspodjele licencom medallion i hack

    query = '''select medallion, hack_license,count(*) from <schemaname>.<nyctaxi_sample> group by medallion, hack_license'''
    pd.read_sql(query,conn)


#### <a name="exploration-trip-time-distribution"></a>Istraživanje: Putovanja vrijeme raspodjele

    query = '''select trip_time_in_secs, count(*) from <schemaname>.<nyctaxi_sample> group by trip_time_in_secs order by count(*) desc'''
    pd.read_sql(query,conn)

#### <a name="exploration-trip-distance-distribution"></a>Istraživanje: Putovanja udaljenost raspodjele

    query = '''select floor(trip_distance/5)*5 as tripbin, count(*) from <schemaname>.<nyctaxi_sample> group by floor(trip_distance/5)*5 order by count(*) desc'''
    pd.read_sql(query,conn)

#### <a name="exploration-payment-type-distribution"></a>Istraživanje: Plaćanja vrsta raspodjele

    query = '''select payment_type,count(*) from <schemaname>.<nyctaxi_sample> group by payment_type'''
    pd.read_sql(query,conn)

#### <a name="verify-the-final-form-of-the-featurized-table"></a>Provjerite je li obrascu konačne tablice featurized

    query = '''SELECT TOP 100 * FROM <schemaname>.<nyctaxi_sample>'''
    pd.read_sql(query,conn)

## <a name="mlmodel"></a>Stvaranje modela u Azure strojnog učenja

Sada ćemo spremni nastaviti s sastavnih modela i model implementacije u [Azure strojnog učenja](https://studio.azureml.net). Podaci su spremni koja će se koristiti u bilo kojem od problema predviđanje označena ranije, odnosno:

1. **Binarni klasifikacija**: za predviđanje li Savjet je platiti za putovanje.

2. **Multiclass klasifikacije**: za predviđanje raspon savjet plaćenu prema prethodno definirane klase.

3. **Zadatak regresije**: za predviđanje količinu savjet platiti za putovanje.  



Da biste započeli vježbu Modeliranje, prijavite se radni prostor **Azure strojnog učenja** . Ako još niste stvorili strojnog učenja radnog prostora, potražite u članku [Stvaranje radnog prostora programa Azure ML](machine-learning-create-workspace.md).

1. Početak rada s Azure strojnog učenja, potražite u članku [što je Azure strojnog učenja Studio?](machine-learning-what-is-ml-studio.md)

2. Prijavite se u sustav [Azure strojnog učenja Studio](https://studio.azureml.net).

3. Studio početnoj stranici nudi mnoštvo informacije, videozapise, vodiče, veze na referencu moduli i ostale resurse. Dodatne informacije o Azure strojnog učenja pogledajte [Azure strojno dokumentaciju medijskog](https://azure.microsoft.com/documentation/services/machine-learning/).

Uobičajeni obuka eksperiment sastoji se od sljedećih koraka:

1. Stvaranje **+ NOVO** eksperiment.
2. Dohvaćanje podataka u Azure ML.
3. Unaprijed obraditi, pretvaranje i upravljali podatke prema potrebi.
4. Generiranje značajke prema potrebi.
5. Podijelite podatke u obuka/provjere valjanosti/testiranje skupove podataka (ili imaju zasebna skupova podataka za svaku).
6. Odaberite jednu ili više strojnog učenja algoritmima pomoću ovisno o problemu učenje da biste riješili. Npr binarni klasifikacija multiclass klasifikacija, regresije.
7. Obuka jedan ili više modela pomoću obuka skupu podataka.
8. Rezultat pomoću obučeni model(s) provjere valjanosti skupu podataka.
9. Analiza model(s) za izračunavanje odgovarajući metriku za učenje problem.
10. Precizno podesite na model(s) i odaberite najbolje model za implementaciju.

U ovom vježbu smo ste već istražili izgrađen podatke u SQL Data Warehouse i odlučili na veličina uzorka ingest u Azure ML. Ovdje je postupak da biste sastavili jedan ili više predviđanje modela:

1. Dohvaćanje podataka u ML Azure pomoću [Uvoz podataka] [ import-data] modula, dostupne u odjeljku **unos podataka i izlaz** . Dodatne informacije potražite u članku [Uvoz podataka] [ import-data] modul referenca stranice.

    ![Azure ML uvoz podataka][17]

2. Odaberite **Baze podataka SQL Azure** kao **izvor podataka** u oknu **Svojstva** .

3. Unesite naziv DNS baze podataka u polje **naziv poslužitelja baze podataka** . Oblik:`tcp:<your_virtual_machine_DNS_name>,1433`

4. Unesite **Naziv baze podataka** u odgovarajućem polju.

5. Unesite *korisničko ime za SQL* **Server korisničko ime**i *lozinku* u okvir **Lozinka korisničkog računa poslužitelja**.

6. Potvrdite mogućnost **Prihvati sve certifikat poslužitelja** .

7. U područje teksta uređivanje **upita baze podataka** Zalijepi upita koji se izdvaja potrebne polja (uključujući izračunata polja kao što su oznake) baze podataka i dolje uzoraka podatke tako da je veličina uzorka željeni.

Primjer eksperimenta binarni klasifikacija čitanje podataka izravno iz baze podataka SQL Data Warehouse je na slici u nastavku (ne zaboravite da biste zamijenili nyctaxi_trip nazive tablica i nyctaxi_fare naziv sheme i nazive tablica koji se koriste u vašem prikazu). Slične eksperimenata se mogu konstruirana za multiclass klasifikaciju i regresije problema.

![Azure ML vlaku][10]

> [AZURE.IMPORTANT] Modeliranje podataka izdvajanje i uzorkovanje Primjeri upita prikazuje se u ranijim odlomcima **sve oznake za vježbe tri Modeliranje nalaze se u upitu**. Programa važne (obavezno) u svakoj od vježbe Modeliranje je korak da biste **izuzeli** nepotrebne naljepnica za druge probleme dva i sve druge **curenje cilj**. Na primjer, prilikom korištenja binarni klasifikacija, koristite oznaku **tipped** i isključiti polja **Savjet\_klase**, **Savjet\_iznos**, a **Ukupni\_iznos**. Drugu mogućnost su cilj osipanjem jer oni ukazuju na savjet plaćenu.
>
> Da biste Isključi sve nepotrebne stupce ili ciljani osipanjem, možete koristiti [Odabir stupaca u skupu podataka] [ select-columns] modula ili [Uređivanje metapodataka][edit-metadata]. Dodatne informacije potražite u odjeljku [Odabir stupaca u skupu podataka] [ select-columns] i [Uređivanje metapodataka] [ edit-metadata] referencu stranice.

## <a name="mldeploy"></a>Implementacija modelima u Azure strojnog učenja

Kada je spreman modela, jednostavno ga možete implementirati kao web-servisa izravno iz pokusa. Dodatne informacije o implementaciji Azure ML web-servisi potražite u članku [uvođenja u web-servisa Azure strojnog učenja](machine-learning-publish-a-machine-learning-web-service.md).

Da biste implementirali nove web-usluge, morate:

1. Stvaranje bilježenja rezultata eksperiment.
2. Implementacija web-servisa.

Da biste stvorili bilježenja rezultata eksperiment eksperiment za obuku **dovršen** , kliknite **Stvori bilježenje rezultata POIGRAJTE se** u donjem akcijsku traku.

![Azure bilježenje rezultata][18]

Da biste stvorili bilježenja rezultata eksperiment koji se temelji na komponenti pokusa obuka će pokušati Azure strojnog učenja. Točnije, ona će:

1. Spremite model obučeni i uklonite module za obuku modela.
2. Odredite logičke **ulazni priključak** za predstavljanje shemi očekivani ulaznih podataka.
3. Odredite logičke **izlazni priključak** za predstavljanje shemi očekivani web servisa izlaz.

Nakon stvaranja bilježenja rezultata pokusa, pregledajte ih i provjerite po potrebi prilagodite. Uobičajeni prilagođavanje je da biste zamijenili unos skup podataka i/ili upita jedan koji isključuje natpis polja, kao što je to neće biti dostupne kada se zove se servis. Preporučuje se i preporučuje da biste smanjili veličinu unos skup podataka i/ili upita u nekoliko zapisa, dovoljno samo za označavanje unosa shemu. Za izlazni priključak uobičajeno je za uključivanje sve ulazna polja i samo na **Testu dobije naljepnica** i **Testu dobije vjerojatnosti** za izlaz pomoću [Odabir stupaca u skupu podataka] [ select-columns] modul.

Uzorak bilježenje rezultata eksperiment navedeni su na slici u nastavku. Kada budete spremni za implementaciju, kliknite gumb **OBJAVLJIVANJE web-SERVISA** na donjem Akcijska traka.

![Objavljivanje Azure ML][11]


## <a name="summary"></a>Sažetak
Da biste recap što smo dovršili ovog praktičnog vodiča vodič, stvorite znanstvenog okruženju Azure podataka u radili s velikim javno dataset poduzimanja kroz timu podataka znanstvenog postupak, sasvim iz podataka acquisition oblikovanja obuka, a zatim implementacija u web-servisa Azure strojnog učenja.

### <a name="license-information"></a>Informacije o licenci

Ovaj vodič za uzorak i njegov popratnim skripte i IPython notebook(s) zajednički koriste Microsoft u odjeljku MIT licenca. Provjerite datoteke LICENSE.txt u direktoriju uzorak koda na GitHub više pojedinosti.

## <a name="references"></a>Reference

• [Andrés Monroy NEW taksi Trips stranici za preuzimanje](http://www.andresmh.com/nyctaxitrips/)  
• [FOILing NEW-taksi putovanja podataka po Chris Whong](http://chriswhong.com/open-data/foil_nyc_taxi/)   
• [Taksi SEBI i Limousine provizije Istraživanje te statistike](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)


[1]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_26_1.png
[2]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_28_1.png
[3]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_35_1.png
[4]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_36_1.png
[5]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_39_1.png
[6]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_42_1.png
[7]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_44_1.png
[8]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_46_1.png
[9]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_71_1.png
[10]: ./media/machine-learning-data-science-process-sqldw-walkthrough/azuremltrain.png
[11]: ./media/machine-learning-data-science-process-sqldw-walkthrough/azuremlpublish.png
[12]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ssmsconnect.png
[13]: ./media/machine-learning-data-science-process-sqldw-walkthrough/executescript.png
[14]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sqlserverproperties.png
[15]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sqldefaultdirs.png
[16]: ./media/machine-learning-data-science-process-sqldw-walkthrough/bulkimport.png
[17]: ./media/machine-learning-data-science-process-sqldw-walkthrough/amlreader.png
[18]: ./media/machine-learning-data-science-process-sqldw-walkthrough/amlscoring.png
[19]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ps_download_scripts.png
[20]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ps_load_data.png
[21]: ./media/machine-learning-data-science-process-sqldw-walkthrough/azcopy-overwrite.png
[22]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-1.png
[23]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-2.png
[24]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-3.png
[25]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-4.png
[26]: ./media/machine-learning-data-science-process-sqldw-walkthrough/tip_class_hist_1.png


<!-- Module References -->
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
