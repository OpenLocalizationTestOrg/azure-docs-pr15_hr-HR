<properties
    pageTitle="Skalabilni znanstvenog podataka u Lake Azure podataka: vodič do završetka do kraja | Microsoft Azure"
    description="Kako koristiti Azure podataka Lake Istraživanje i binarna klasifikacije zadacima podataka na skup podataka."  
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
    ms.date="09/19/2016"
    ms.author="bradsev;weig"/>


# <a name="scalable-data-science-in-azure-data-lake-an-end-to-end-walkthrough"></a>Skalabilni znanstvenog podataka u Lake Azure podataka: vodič do završetka do kraja

Ovaj vodič prikazuje kako koristiti Lake Azure podataka da biste pokrenuli binarni klasifikacija zadaci na uzorak taksi putovanja SEBI i Istraživanje podataka i prijevoz skup podataka za predviđanje hoće li savjet isplate tako da na prijevoz. Vodi vas kroz korake [Timu podataka znanstvenog postupak](http://aka.ms/datascienceprocess)završetka do kraja, iz podataka acquisition oblikovanja obuka, a zatim implementacija web-servisa koje objavljuje modela.


### <a name="azure-data-lake-analytics"></a>Analitički Lake Azure podataka

[Microsoft Azure podataka Lake](https://azure.microsoft.com/solutions/data-lake/) sadrži sve mogućnosti potrebne da biste olakšali fizičari podataka radi pohrane podataka veličine, oblika i brzinu, a zatim izvršite obradu podataka, naprednom analitikom i strojnog učenja Modeliranje s visoke skalabilnost na učinkovit način.   Plaćate na temelju po posla, samo kada podataka zapravo obrade. Azure podataka Lake Analytics uključuje U SQL, na jeziku koji se prelijeva deklarativno prirode SQL s izražajna moć C# možete unijeti skalabilni distributed mogućnost upita. Omogućuje vam da obradi nestrukturirane podatke primjenom sheme na čitanje, Umetanje prilagođene logiku i korisnički definirane funkcije (UDF-ove), a obuhvaća mogućnost proširenja da biste omogućili precizno grained kontrolu nad kako izvoditi na razini. Dodatne informacije o filozofije dizajna iza U SQL potražite u članku [Visual Studio blogu](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).

Analize podataka Lake ujedno ključa dio paketa analitičkih podataka Cortana i radi s Azure SQL Data Warehouse, Power BI i tvorničke podataka. Tako ćete dobiti potpuni oblaka velikih skupova podataka i naprednom analitikom platforme.

Ovaj vodič započinje po opisuju preduvjeti i resurse koji su vam potrebne za dovršenje zadatka s analize Lake podataka koji čine postupka znanstvenog podataka i kako ih instalirati. Zatim se navode koraci za obradu podataka pomoću U SQL i završava tako da pokazuje kako koristiti Python i grozd s Azure strojnog učenja Studio omogućuje stvaranje i implementaciju predvidljivu modela. 


### <a name="u-sql-and-visual-studio"></a>U SQL i Visual Studio

Ovaj vodič preporučuje korištenje Visual Studio da biste uredili U SQL skripte za obradu skupu podataka. U SQL skripte koje se ovdje opisuju i navedenih u zasebnu datoteku. Postupak obuhvaća ingesting, istraživanje i uzorkovanje podatke. Također se prikazuje kako Pokreni U SQL postavljanje upita skriptiranih s portala za Azure. Grozd tablice se stvaraju za podatke u povezane HDInsight klaster radi lakšeg sastavnih i implementaciju binarni klasifikacija modela u Azure strojnog učenja Studio.  


### <a name="python"></a>Python

Ovaj vodič sadrži i odjeljak koji prikazuje kako stvorite i implementirajte predvidljivu modela pomoću Python Azure strojnog učenja Studio.  Skripte Python bilježnice Jupyter dajemo za sljedeće korake ovog postupka. Bilježnica sadrži kôd za neke dodatne značajke inženjerskim korake i modelima izgradnju kao što su multiclass klasifikaciju i regresije Modeliranje osim modela binarni klasifikacija navedene u nastavku. Zadatak regresije je za predviđanje iznos koji se temelji na druge značajke savjet savjet. 


### <a name="azure-machine-learning"></a>Azure strojnog učenja
Azure strojnog učenja Studio se koristi za stvaranje i implementaciju predvidljivu modela. To možete učiniti pomoću dva pristupa: prvo s Python skripte i zatim grozd tablica programa klaster HDInsight (Hadoop).


### <a name="scripts"></a>Skripte

Samo glavni koraci su strukturirane u ovom vodiču. Potpuna **U SQL skripte** i **Jupyter bilježnice** možete preuzeti iz [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough).


## <a name="prerequisites"></a>Preduvjeti

Prije nego počnete ove teme, morate imati sljedeće:

- Azure pretplate. Ako nije već postoji, potražite u članku [Početak Azure besplatnu probnu verziju](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- [Preporučeno] Visual Studio 2013 ili 2015. Ako već nemate jedna od ovih verzija instalirana, možete preuzeti besplatnu edition zajednice iz [ovdje](https://www.visualstudio.com/visual-studio-homepage-vs.aspx). Kliknite gumb **Preuzmi zajednice 2015** u odjeljku Visual Studio. 

>[AZURE.NOTE] Umjesto Visual Studio, možete koristiti i Azure Portal za slanje upita Lake Azure podataka. Dajemo će upute o tome kako biste to učinili i s Visual Studio i na portalu u odjeljku pod naslovom **Podaci postupak U SQL**. 

- Prijava za pregled Lake Azure podataka

>[AZURE.NOTE] Morate nabaviti odobrenje da biste koristili Azure podataka Lake pohrane (ADLS) i Azure podataka Lake analize (ADLA) kao što su ti servisi u pretpregledu. Zatražit će se da biste se registrirali prilikom stvaranja prvog ADLS ili ADLA. Da biste sigh prema gore, kliknite **prijavite se da biste pretpregledali**, pročitajte ugovor, a zatim kliknite **u redu**. Na primjer, Evo ADLS registracije stranice:

 ![2](./media/machine-learning-data-science-process-data-lake-walkthrough/2-ADLA-preview-signup.PNG)


## <a name="prepare-data-science-environment-for-azure-data-lake"></a>Priprema podataka znanstvenog okruženja za Lake Azure podataka
Da biste pripremili okruženja znanstvenog podataka za ovaj vodič, stvorite u sljedećim resursima:

- Spremište Lake Azure podataka (ADLS) 
- Azure podataka Lake analize (ADLA)
- Azure računa spremišta blobova platforme
- Račun za Azure strojnog učenja Studio
- Azure Data Lake Tools za Visual Studio (preporučeno)

U ovom se odjeljku daju upute o stvaranju svakom od tih resursa. Ako odlučite koristiti grozd tablice s Azure strojnog učenja umjesto Python, za izradu modela, i morat ćete Dodjela resursa za klaster HDInsight (Hadoop). Postupak zamjenski u opisane u odgovarajućoj sekciji ispod.
<br/>
>AZURE. Imajte na UMU **Spremišta Lake za Azure podataka** mogu se kreirati ili zasebno ili prilikom stvaranja **Lake analize podataka za Azure** kao zadani prostor za pohranu. Upute za stvaranje svakom od tih resursa zasebno ispod referencirani, ali račun za pohranu podataka Lake potrebno nije moguće stvoriti zasebno.
<br/>
### <a name="create-an-azure-data-lake-store"></a>Stvaranje je iz trgovine Azure podataka Lake

Stvaranje programa ADLS s [portala za Azure](http://portal.azure.com). Detalje potražite u članku [Stvaranje programa HDInsight klaster s trgovinom Lake podataka pomoću portala za Azure](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md). Obavezno Postavljanje identiteta AAD klaster u plohu **izvora podataka** od plohu **Neobavezno konfiguracije** opisane postoji. 

 ![3](./media/machine-learning-data-science-process-data-lake-walkthrough/3-create-ADLS.PNG)


### <a name="create-an-azure-data-lake-analytics-account"></a>Stvorite račun za Azure podataka Lake Analytics
Stvorite račun ADLA s [Portala za Azure](http://portal.azure.com). Detalje potražite u članku [Praktični vodič: početak rada s analize Lake podataka za Azure pomoću portala za Azure](../data-lake-analytics/data-lake-analytics-get-started-portal.md). 

 ![4](./media/machine-learning-data-science-process-data-lake-walkthrough/4-create-ADLA-new.PNG)


### <a name="create-an-azure-blob-storage-account"></a>Stvaranje računa za spremište blobova platforme Azure
Stvorite račun za spremište blobova platforme Azure s [Portala za Azure](http://portal.azure.com). Detalje potražite u članku stvaranje sekcije račun za pohranu u [račune za pohranu o Azure](../storage/storage-create-storage-account.md).
    
 ![5](./media/machine-learning-data-science-process-data-lake-walkthrough/5-Create-Azure-Blob.PNG)


### <a name="set-up-an-azure-machine-learning-studio-account"></a>Postavljanje računa Azure strojnog učenja Studio
Prijavite se gore/u Azure strojnog učenja Studio sa stranice [Azure strojnog učenja](https://azure.microsoft.com/services/machine-learning/) . Kliknite gumb **počnite s radom odmah** , a zatim odaberite "Slobodnog prostora" ili "Standardne radnog prostora". Nakon toga će se da biste stvorili eksperimenata u Azure ML Studio.  

### <a name="install-azure-data-lake-tools-recommended"></a>Instalacija alata za Lake Azure podataka [preporučeno]
Instalacija alata za Lake Azure podataka za vašu verziju programa Visual Studio iz [Azure Data Lake Tools za Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504).

 ![6](./media/machine-learning-data-science-process-data-lake-walkthrough/6-install-ADL-tools-VS.PNG)

Kada se instalacija uspješno završi, otvorite Visual Studio. Trebali biste vidjeti Lake podatkovne kartice izbornika na vrhu. Resursi za Azure prikazivati u lijevom oknu kada se prijavite na račun za Azure.

 ![7](./media/machine-learning-data-science-process-data-lake-walkthrough/7-install-ADL-tools-VS-done.PNG)


## <a name="the-nyc-taxi-trips-dataset"></a>Skup podataka Trips taksi SEBI
Zadani skup podataka ne možemo koristi je javno dostupna skup podataka – [NEW taksi Trips skup podataka](http://www.andresmh.com/nyctaxitrips/). Podaci NEW taksi putovanja sastoji se od otprilike 20GB komprimirane CSV datoteke (dekomprimirati ~ 48GB), više od 173 milijuna pojedinačne trips i na fares snimki platiti za svaki put. Svaki zapis putovanja obuhvaća mjesta web-mjesto prijem i u okvir za predaju i vremena, anonymized hack (upravljačkog programa) broj licenci i broj medallion (taksi na jedinstveni ID-ja). Podaci pokriva sve trips u godini 2013 uvjet, a u sljedeća dva skupova podataka za svaki mjesec:

 - 'trip_data' CSV sadrži detalje za putovanja, kao što su broj passengers, prijem i dropoff točaka, trajanja putovanja, a duljina putovanja. Evo nekoliko uzoraka zapisa:

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count, trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868

 - "trip_fare" CSV sadrži detalje prijevoz platiti za svaki put, kao što je način plaćanja, prijevoz iznos, dodatna naknada i poreza, savjete i tolls, i ukupan iznos plaćen. Evo nekoliko uzoraka zapisa:

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Jedinstveni ključ da biste se pridružili putovanja\_podataka i putovanja\_prijevoz sastoji se od sljedeća tri polja: medallion, hack\_licenci i prijem\_datuma i vremena. Neobrađenog CSV datoteke možete pristupiti s blob javno Azure prostora za pohranu. U SQL skripta za ovaj spoj je u odjeljku [Uključivanje putovanja i prijevoz tablice](#join) .

## <a name="process-data-with-u-sql"></a>Obrada podataka s U SQL

Zadaci obradu podataka koja je prikazana u ovom odjeljku obuhvaćaju ingesting, provjeru kvalitete, istraživanje i uzorkovanje podatke. Pokazat ćemo i kako se uključiti putovanja i prijevoz tablice. Konačni sekcije prikazuje izvođenja U SQL skriptiranih zadatak s portala za Azure. Evo veze na svakom Podsekcija:

- [Ingestion podataka: pročitajte u podatke iz javne blobova platforme](#ingest)
- [Provjera kvalitete podataka](#quality)
- [Istraživanje podataka](#explore)
- [Spajanje putovanja i prijevoz tablice](#join)
- [Stvaranje uzoraka podataka](#sample)
- [Izvođenje zadataka U SQL](#run)

U SQL skripte koje se ovdje opisuju i navedenih u zasebnu datoteku. Potpuna **U SQL skripte** možete preuzeti iz [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough).

Izvršavanje U-SQL, Otvori Visual Studio, kliknite **datoteke--> novo--> projekta**, odaberite **U SQL projekta**, naziv i spremite ga u mapu.

![8](./media/machine-learning-data-science-process-data-lake-walkthrough/8-create-USQL-project.PNG)

>[AZURE.NOTE] Moguće je izvesti U SQL umjesto Visual Studio pomoću portala za Azure. Možete do Lake analize podataka za Azure resursa na portalu i slanje upita izravno kao ilustracija na sljedećoj slici.

![9](./media/machine-learning-data-science-process-data-lake-walkthrough/9-portal-submit-job.PNG)

### <a name="ingest"></a>Podataka Ingestion: Čitanje u podatke iz javne blobova platforme

Mjesto podatke u blobova platforme Azure je navedena kao **wasb://container_name@blob_storage_account_name.blob.core.windows.net/blob_name** i može se izdvajati pomoću **Extractors.Csv()**. Zamijenite vlastitim spremnik ime i naziv računa za pohranu u sljedeće skripte za container_name@blob_storage_account_name adresu wasb. Budući da nazivi datoteka nalaze u istom obliku, možete koristiti **putovanja\_data_ {\*\}.csv** za čitanje u svim datotekama 12 putovanja. 

    ///Read in Trip data
    @trip0 =
        EXTRACT 
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count string,
        trip_time_in_secs string,
        trip_distance string,
        pickup_longitude string,
        pickup_latitude string,
        dropoff_longitude string,
        dropoff_latitude string
    // This is reading 12 trip data from blob
    FROM "wasb://container_name@blob_storage_account_name.blob.core.windows.net/nyctaxitrip/trip_data_{*}.csv"
    USING Extractors.Csv();

Budući da postoje zaglavlja u prvom retku, moramo da biste uklonili zaglavlja i promjena vrste stupaca u odgovarajuće one. Možemo ili spremanje računa obrađeni podataka za pohranu Lake podataka za Azure pomoću **swebhdfs://data_lake_storage_name.azuredatalakestorage.net/folder_name/file_name**_ ili spremište blobova platforme Azure pomoću **wasb://container_name@blob_storage_account_name.blob.core.windows.net/blob_name**. 

    // change data types
    @trip =
        SELECT 
        medallion,
        hack_license,
        vendor_id,
        rate_code,
        store_and_fwd_flag,
        DateTime.Parse(pickup_datetime) AS pickup_datetime,
        DateTime.Parse(dropoff_datetime) AS dropoff_datetime,
        Int32.Parse(passenger_count) AS passenger_count,
        Double.Parse(trip_time_in_secs) AS trip_time_in_secs,
        Double.Parse(trip_distance) AS trip_distance,
        (pickup_longitude==string.Empty ? 0: float.Parse(pickup_longitude)) AS pickup_longitude,
        (pickup_latitude==string.Empty ? 0: float.Parse(pickup_latitude)) AS pickup_latitude,
        (dropoff_longitude==string.Empty ? 0: float.Parse(dropoff_longitude)) AS dropoff_longitude,
        (dropoff_latitude==string.Empty ? 0: float.Parse(dropoff_latitude)) AS dropoff_latitude
    FROM @trip0
    WHERE medallion != "medallion";

    ////output data to ADL
    OUTPUT @trip   
    TO "swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_trip.csv"
    USING Outputters.Csv(); 

    ////Output data to blob
    OUTPUT @trip   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_trip.csv"
    USING Outputters.Csv();  

Isto tako ćemo može čitati u prijevoz skupove podataka. Desnom tipkom miša kliknite Lake spremišta podataka za Azure, možete odabrati da biste vidjeli podatke u **Azure Portal--> Explorer podataka** ili **Eksplorer za datoteke** u Visual Studio. 

 ![10](./media/machine-learning-data-science-process-data-lake-walkthrough/10-data-in-ADL-VS.PNG)

 ![11](./media/machine-learning-data-science-process-data-lake-walkthrough/11-data-in-ADL.PNG)


### <a name="quality"></a>Provjera kvalitete podataka

Nakon putovanja i prijevoz tablice su pročitane u, provjere kvalitete podataka možete učiniti na sljedeći način. Rezultat CSV datoteke mogu biti Izlaz u spremište blobova platforme Azure ili Lake spremišta podataka za Azure. 

Pronađite broj medallions i jedinstveni broj medallions:

    ///check the number of medallions and unique number of medallions
    @trip2 =
        SELECT
        medallion,
        vendor_id,
        pickup_datetime.Month AS pickup_month
        FROM @trip;
    
    @ex_1 =
        SELECT
        pickup_month, 
        COUNT(medallion) AS cnt_medallion,
        COUNT(DISTINCT(medallion)) AS unique_medallion
        FROM @trip2
        GROUP BY pickup_month;
        OUTPUT @ex_1   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_1.csv"
    USING Outputters.Csv(); 

Pronalaženje te medallions koja je imala više od 100 trips:

    ///find those medallions that had more than 100 trips
    @ex_2 =
        SELECT medallion,
               COUNT(medallion) AS cnt_medallion
        FROM @trip2
        //where pickup_datetime >= "2013-01-01t00:00:00.0000000" and pickup_datetime <= "2013-04-01t00:00:00.0000000"
        GROUP BY medallion
        HAVING COUNT(medallion) > 100;
        OUTPUT @ex_2   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_2.csv"
    USING Outputters.Csv(); 

Pronađite zapise koji nisu valjani pomoću pickup_longitude:

    ///find those invalid records in terms of pickup_longitude
    @ex_3 =
        SELECT COUNT(medallion) AS cnt_invalid_pickup_longitude
        FROM @trip
        WHERE
        pickup_longitude <- 90 OR pickup_longitude > 90;
        OUTPUT @ex_3   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_3.csv"
    USING Outputters.Csv(); 

Traženje vrijednosti koje nedostaju za neke varijable:

    //check missing values
    @res =
        SELECT *,
               (medallion == null? 1 : 0) AS missing_medallion
        FROM @trip;
    
    @trip_summary6 =
        SELECT 
            vendor_id,
        SUM(missing_medallion) AS medallion_empty, 
        COUNT(medallion) AS medallion_total,
        COUNT(DISTINCT(medallion)) AS medallion_total_unique  
        FROM @res
        GROUP BY vendor_id;
    OUTPUT @trip_summary6
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_16.csv"
    USING Outputters.Csv();



### <a name="explore"></a>Istraživanje podataka

Možemo neke Istraživanje podataka da biste bolje razumijevanje podataka.

Traži distribucija zakrenut i koje nisu tipped trips:

    ///tipped vs. not tipped distribution
    @tip_or_not =
        SELECT *,
               (tip_amount > 0 ? 1: 0) AS tipped
        FROM @fare;
    
    @ex_4 =
        SELECT tipped,
               COUNT(*) AS tip_freq
        FROM @tip_or_not
        GROUP BY tipped;
        OUTPUT @ex_4   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_4.csv"
    USING Outputters.Csv(); 

Pronalaženje raspodjele savjet iznos vrijednostima cut-off: 0,5,10 i 20 dolarima.

    //tip class/range distribution
    @tip_class =
        SELECT *,
               (tip_amount >20? 4: (tip_amount >10? 3:(tip_amount >5 ? 2:(tip_amount > 0 ? 1: 0)))) AS tip_class
        FROM @fare;
    @ex_5 =
        SELECT tip_class,
               COUNT(*) AS tip_freq
        FROM @tip_class
        GROUP BY tip_class;
        OUTPUT @ex_5   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_5.csv"
    USING Outputters.Csv(); 

Pronalaženje osnovni Statistika putovanja udaljenost:

    // find basic statistics for trip_distance
    @trip_summary4 =
        SELECT 
            vendor_id,
            COUNT(*) AS cnt_row,
            MIN(trip_distance) AS min_trip_distance,
            MAX(trip_distance) AS max_trip_distance,
            AVG(trip_distance) AS avg_trip_distance 
        FROM @trip
        GROUP BY vendor_id;
    OUTPUT @trip_summary4
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_14.csv"
    USING Outputters.Csv();

Pronalaženje percentili putovanja udaljenost:

    // find percentiles of trip_distance
    @trip_summary3 =
        SELECT DISTINCT vendor_id AS vendor,
                        PERCENTILE_DISC(0.25) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc,
                        PERCENTILE_DISC(0.5) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc,
                        PERCENTILE_DISC(0.75) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc
        FROM @trip;
       // group by vendor_id;
    OUTPUT @trip_summary3
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_13.csv"
    USING Outputters.Csv(); 


### <a name="join"></a>Spajanje putovanja i prijevoz tablice

Putovanje i prijevoz tablice možete se spajaju medallion, hack_license i pickup_time.

    //join trip and fare table

    @model_data_full =
    SELECT t.*, 
    f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,  f.total_amount, f.tip_amount,
    (f.tip_amount > 0 ? 1: 0) AS tipped,
    (f.tip_amount >20? 4: (f.tip_amount >10? 3:(f.tip_amount >5 ? 2:(f.tip_amount > 0 ? 1: 0)))) AS tip_class
    FROM @trip AS t JOIN  @fare AS f
    ON   (t.medallion == f.medallion AND t.hack_license == f.hack_license AND t.pickup_datetime == f.pickup_datetime)
    WHERE   (pickup_longitude != 0 AND dropoff_longitude != 0 );

    //// output to blob
    OUTPUT @model_data_full   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_7_full_data.csv"
    USING Outputters.Csv(); 

    ////output data to ADL
    OUTPUT @model_data_full   
    TO "swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_ex_7_full_data.csv"
    USING Outputters.Csv(); 


Za svaku razinu putnika count izračunati broj zapisa, savjet prosječni iznos, varijancu savjet iznosa, postotak zakrenut trips.

    // contigency table
    @trip_summary8 =
        SELECT passenger_count,
               COUNT(*) AS cnt,
               AVG(tip_amount) AS avg_tip_amount,
               VAR(tip_amount) AS var_tip_amount,
               SUM(tipped) AS cnt_tipped,
               (float)SUM(tipped)/COUNT(*) AS pct_tipped
        FROM @model_data_full
        GROUP BY passenger_count;
        OUTPUT @trip_summary8
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_17.csv"
    USING Outputters.Csv();


### <a name="sample"></a>Stvaranje uzoraka podataka

Najprije ćemo slučajno odaberete 0,1% podaci iz spojene tablice:

    //random select 1/1000 data for modeling purpose
    @addrownumberres_randomsample =
    SELECT *,
            ROW_NUMBER() OVER() AS rownum
    FROM @model_data_full;
    
    @model_data_random_sample_1_1000 =
    SELECT *
    FROM @addrownumberres_randomsample
    WHERE rownum % 1000 == 0;
    
    OUTPUT @model_data_random_sample_1_1000   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_7_random_1_1000.csv"
    USING Outputters.Csv(); 

Zatim ćemo učiniti stratified uzorkovanje binarni varijable tip_class:

    //stratified random select 1/1000 data for modeling purpose
    @addrownumberres_stratifiedsample =
    SELECT *,
            ROW_NUMBER() OVER(PARTITION BY tip_class) AS rownum
    FROM @model_data_full;
    
    @model_data_stratified_sample_1_1000 =
    SELECT *
    FROM @addrownumberres_stratifiedsample
    WHERE rownum % 1000 == 0;
    //// output to blob
    OUTPUT @model_data_stratified_sample_1_1000   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_9_stratified_1_1000.csv"
    USING Outputters.Csv(); 
    ////output data to ADL
    OUTPUT @model_data_stratified_sample_1_1000   
    TO "swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_ex_9_stratified_1_1000.csv"
    USING Outputters.Csv(); 


### <a name="run"></a>Izvođenje zadataka U SQL

Kada završite s uređivanjem U SQL skripte, možete ih poslati s poslužiteljem pomoću računa za Azure podataka Lake analize. Kliknite **Lake podataka**, **Slanje posla**, odaberite svoj **Račun analize**, odaberite **Parallelism**pa kliknite gumb **Pošalji** .  

 ![12](./media/machine-learning-data-science-process-data-lake-walkthrough/12-submit-USQL.PNG)

Kada se zadatak uspješno je complied, status posla prikazat će se u Visual Studio za nadzor. Nakon posao završi s radom, možete čak i ponavljanje izvođenja procesa zadatka i dodatne korake usko grlo za poboljšanje učinkovitosti posao. Možete posjetiti i Azure portal da biste provjerili status svoje zadatke U SQL.

 ![13](./media/machine-learning-data-science-process-data-lake-walkthrough/13-USQL-running-v2.PNG)


 ![14](./media/machine-learning-data-science-process-data-lake-walkthrough/14-USQL-jobs-portal.PNG)


Sada možete provjeriti Izlazna datoteka u spremište blobova platforme Azure ili Azure Portal. Koristit ćemo stratified oglednih podataka za naše Modeliranje u sljedećem koraku.

 ![15](./media/machine-learning-data-science-process-data-lake-walkthrough/15-U-SQL-output-csv.PNG)

 ![16](./media/machine-learning-data-science-process-data-lake-walkthrough/16-U-SQL-output-csv-portal.PNG)


## <a name="build-and-deploy-models-in-azure-machine-learning"></a>Stvorite i implementirajte modelima u Azure strojnog učenja

Ne možemo demonstrirati dvije mogućnosti dostupne za dohvaćanje podataka Azure strojnog učenja Sastavljanje i 

- U prvu mogućnost koristiti sampled podatke koji zapisuje Azure blobova platforme (u **podataka uzorkovanje** koraku gore), a pomoću Python omogućuje stvaranje i implementaciju modela iz Azure strojnog učenja. 
- Iz druge mogućnosti koje upit podatke u Azure podataka Lake izravno putem grozd upita. Ova mogućnost zahtijeva stvaranje nove HDInsight klaster ili koristiti postojeće klaster za HDInsight gdje tablice grozd pokažite NY taksi podataka u spremište Lake podataka za Azure.  Ćemo razmotriti obje te mogućnosti u nastavku. 

## <a name="option-1-use-python-to-build-and-deploy-machine-learning-models"></a>Mogućnost 1: Korištenje Python omogućuje stvaranje i implementaciju na računalu modelima učenje

Omogućuje stvaranje i implementaciju strojnog učenja modela pomoću Python, stvorite Jupyter bilježnicu na lokalnom računalu ili u Azure strojnog učenja Studio. Bilježnice Jupyter navedeni su na [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough) sadrži cijeli koda za istraživanje, vizualizacija podataka, značajka inženjering, Modeliranje i implementaciju. U ovom se članku Pokazat ćemo samo Modeliranje i implementaciju. 

### <a name="import-python-libraries"></a>Uvoz Python biblioteke

Da biste pokrenuli uzorka Jupyter bilježnice ili na Python skripte datoteku, a zatim sljedeće Python paketa su vam potrebne. Ako koristite servis AzureML bilježnice, te pakete nije instalirana.

    import pandas as pd
    from pandas import Series, DataFrame
    import numpy as np
    import matplotlib.pyplot as plt
    from time import time
    import pyodbc
    import os
    from azure.storage.blob import BlobService
    import tables
    import time
    import zipfile
    import random
    import sklearn
    from sklearn.linear_model import LogisticRegression
    from sklearn.cross_validation import train_test_split
    from sklearn import metrics
    from __future__ import division
    from sklearn import linear_model
    from azureml import services


### <a name="read-in-the-data-from-blob"></a>Čitanje podataka s blob

- Niz za povezivanje   

        CONTAINERNAME = 'test1'
        STORAGEACCOUNTNAME = 'XXXXXXXXX'
        STORAGEACCOUNTKEY = 'YYYYYYYYYYYYYYYYYYYYYYYYYYYY'
        BLOBNAME = 'demo_ex_9_stratified_1_1000_copy.csv'
        blob_service = BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
    
- Pročitajte u obliku teksta

        t1 = time.time()
        data = blob_service.get_blob_to_text(CONTAINERNAME,BLOBNAME).split("\n")
        t2 = time.time()
        print(("It takes %s seconds to read in "+BLOBNAME) % (t2 - t1))

 ![17](./media/machine-learning-data-science-process-data-lake-walkthrough/17-python_readin_csv.PNG)    
 
- Dodavanje naziva stupaca i odvojite stupaca

        colnames = ['medallion','hack_license','vendor_id','rate_code','store_and_fwd_flag','pickup_datetime','dropoff_datetime',
        'passenger_count','trip_time_in_secs','trip_distance','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude',
        'payment_type', 'fare_amount', 'surcharge', 'mta_tax', 'tolls_amount',  'total_amount', 'tip_amount', 'tipped', 'tip_class', 'rownum']
        df1 = pd.DataFrame([sub.split(",") for sub in data], columns = colnames)
    


- Promijenite neki stupci numeričke

        cols_2_float = ['trip_time_in_secs','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude',
        'fare_amount', 'surcharge','mta_tax','tolls_amount','total_amount','tip_amount', 'passenger_count','trip_distance'
        ,'tipped','tip_class','rownum']
        for col in cols_2_float:
            df1[col] = df1[col].astype(float)

### <a name="build-machine-learning-models"></a>Sastavljanje strojnog učenja modela

Ovdje možemo sastavljanje binarni klasifikacija model za predviđanje li putovanja tipped ili ne. U bilježnici Jupyter možete pronaći druge dva modela: multiclass klasifikaciju i regresije modela.

- Najprije ćemo potrebnih za stvaranje sustava varijabli koje je moguće koristiti u scikit – Saznajte modela

        df1_payment_type_dummy = pd.get_dummies(df1['payment_type'], prefix='payment_type_dummy')
        df1_vendor_id_dummy = pd.get_dummies(df1['vendor_id'], prefix='vendor_id_dummy')

- Stvaranje okvira podataka na Modeliranje

        cols_to_keep = ['tipped', 'trip_distance', 'passenger_count']
        data = df1[cols_to_keep].join([df1_payment_type_dummy,df1_vendor_id_dummy])
        
        X = data.iloc[:,1:]
        Y = data.tipped

- Obuka i testiranje 60 40 podjele

        X_train, X_test, Y_train, Y_test = train_test_split(X, Y, test_size=0.4, random_state=0)

- Logistika regresije u skupu obuka

        model = LogisticRegression()
        logit_fit = model.fit(X_train, Y_train)
        print ('Coefficients: \n', logit_fit.coef_)
        Y_train_pred = logit_fit.predict(X_train)

       ![c1](./media/machine-learning-data-science-process-data-lake-walkthrough/c1-py-logit-coefficient.PNG)

- Rezultat testiranja skup podataka

        Y_test_pred = logit_fit.predict(X_test)

- Izračun procjenu mjerenja

        fpr_train, tpr_train, thresholds_train = metrics.roc_curve(Y_train, Y_train_pred)
        print fpr_train, tpr_train, thresholds_train
        
        fpr_test, tpr_test, thresholds_test = metrics.roc_curve(Y_test, Y_test_pred) 
        print fpr_test, tpr_test, thresholds_test
        
        #AUC
        print metrics.auc(fpr_train,tpr_train)
        print metrics.auc(fpr_test,tpr_test)
        
        #Confusion Matrix
        print metrics.confusion_matrix(Y_train,Y_train_pred)
        print metrics.confusion_matrix(Y_test,Y_test_pred)

       ![c2](./media/machine-learning-data-science-process-data-lake-walkthrough/c2-py-logit-evaluation.PNG)


 
### <a name="build-web-service-api-and-consume-it-in-python"></a>Stvaranje API servisa Web i zauzeti u Python

Želimo operationalize strojnog učenja model kada je ugrađena. Ovdje koristimo binarni logistika model kao primjer. Provjerite je li u scikit – Saznajte verzija na lokalno računalo nije 0.15.1. Ne morate brinuti o tome ako koristite servisa Azure ML studio.

- Pronađite vjerodajnice radnog prostora iz Azure ML studio postavki. Azure strojnog učenja Studio, kliknite **Postavke** --> **naziv** --> **Tokeni za provjeru autentičnosti**. 

    ![C3](./media/machine-learning-data-science-process-data-lake-walkthrough/c3-workspace-id.PNG)


        workspaceid = 'xxxxxxxxxxxxxxxxxxxxxxxxxxx'
        auth_token = 'xxxxxxxxxxxxxxxxxxxxxxxxxxx'

- Stvaranje web-servisa

        @services.publish(workspaceid, auth_token) 
        @services.types(trip_distance = float, passenger_count = float, payment_type_dummy_CRD = float, payment_type_dummy_CSH=float, payment_type_dummy_DIS = float, payment_type_dummy_NOC = float, payment_type_dummy_UNK = float, vendor_id_dummy_CMT = float, vendor_id_dummy_VTS = float)
        @services.returns(int) #0, or 1
        def predictNYCTAXI(trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH,payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS ):
            inputArray = [trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH, payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS]
            return logit_fit.predict(inputArray)

- Dobiti vjerodajnice servisa za web

        url = predictNYCTAXI.service.url
        api_key =  predictNYCTAXI.service.api_key
        
        print url
        print api_key

        @services.service(url, api_key)
        @services.types(trip_distance = float, passenger_count = float, payment_type_dummy_CRD = float, payment_type_dummy_CSH=float,payment_type_dummy_DIS = float, payment_type_dummy_NOC = float, payment_type_dummy_UNK = float, vendor_id_dummy_CMT = float, vendor_id_dummy_VTS = float)
        @services.returns(float)
        def NYCTAXIPredictor(trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH,payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS ):
            pass

- Nazovite API usluge za Web. Ćete morati pričekati 5 10 sekundi nakon prethodnom koraku.

        NYCTAXIPredictor(1,2,1,0,0,0,0,0,1)

       ![c4](./media/machine-learning-data-science-process-data-lake-walkthrough/c4-call-API.PNG)


## <a name="option-2-create-and-deploy-models-directly-in-azure-machine-learning"></a>Mogućnost 2: Stvaranje i implementacija modela izravno u Azure strojnog učenja

Azure strojnog učenja Studio može čitati podatke izravno iz spremišta Lake za Azure podataka, a zatim može koristiti za stvaranje i implementacija modela. Taj se način koristi tablicu vrste Hive koja pokazuje u trgovini Lake Azure podataka. To su potrebne da dodjeli biti zasebnom Azure HDInsight klaster, na kojem je stvoren tablicu vrste Hive. U sljedećim se odjeljcima pokazati kako to učiniti. 

### <a name="create-an-hdinsight-linux-cluster"></a>Stvaranje programa HDInsight Linux klaster

Stvaranje HDInsight klaster (Linux) s [portala za Azure](http://portal.azure.com). Detalje potražite u članku u odjeljku **Stvaranje programa HDInsight klaster s pristupom Lake spremišta podataka za Azure** [Stvaranje programa HDInsight klaster s trgovinom Lake podataka pomoću portala za Azure](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

 ![18](./media/machine-learning-data-science-process-data-lake-walkthrough/18-create_HDI_cluster.PNG)

### <a name="create-hive-table-in-hdinsight"></a>Stvorite tablicu vrste Hive u HDInsight

Sada ćemo stvoriti grozd tablice koja će se koristiti u Azure strojnog učenja Studio u skupini HDInsight pomoću podatke pohranjene u spremištu Lake za Azure podataka u prethodnom koraku. Idite na HDInsight klaster upravo stvorili. Kliknite **Postavke** --> **Svojstva** --> **Klaster AAD identiteta** --> **ADLS pristup**, provjerite je li vaš račun za Azure podataka Lake spremište dodan na popisu s čitanje, pisanje i izvršavanje prava. 

 ![19](./media/machine-learning-data-science-process-data-lake-walkthrough/19-HDI-cluster-add-ADLS.PNG)


Zatim kliknite **nadzorna ploča** uz gumb **Postavke** , a prozora pojavit će se. Kliknite **Prikaz vrste Hive** u gornjem desnom kutu stranice i prikazat će se **Uređivač upita**.

 ![20](./media/machine-learning-data-science-process-data-lake-walkthrough/20-HDI-dashboard.PNG)

 ![21](./media/machine-learning-data-science-process-data-lake-walkthrough/21-Hive-Query-Editor-v2.PNG)


Zalijepite sljedeće skripte grozd da biste stvorili tablicu. Mjesto izvora podataka se Azure podataka Lake iz reference na ovaj način: **adl://data_lake_store_name.azuredatalakestore.net:443/folder_name/naziv_datoteke**.

    CREATE EXTERNAL TABLE nyc_stratified_sample
    (
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count string,
        trip_time_in_secs string,
        trip_distance string,
        pickup_longitude string,
        pickup_latitude string,
        dropoff_longitude string,
        dropoff_latitude string,
      payment_type string,
      fare_amount string,
      surcharge string,
      mta_tax string,
      tolls_amount string,
      total_amount string,
      tip_amount string,
      tipped string,
      tip_class string,
      rownum string
      )
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    LOCATION 'adl://data_lake_storage_name.azuredatalakestore.net:443/nyctaxi_folder/demo_ex_9_stratified_1_1000_copy.csv';


Kada je upit završi s radom, prikazat će se rezultati ovako:

 ![22](./media/machine-learning-data-science-process-data-lake-walkthrough/22-Hive-Query-results.PNG)



### <a name="build-and-deploy-models-in-azure-machine-learning-studio"></a>Stvorite i implementirajte modelima u Azure strojnog učenja Studio

Ne možemo sada ste spremni za stvaranje i implementaciju modelu koji predviđa hoće li se savjet plaćanje s Azure strojnog učenja. Stratified ogledne podatke spreman će se koristiti u ovom binarni klasifikacija (savjet ili ne) problem. Predvidljivu modela pomoću multiclass klasifikacije (tip_class) i regresije (tip_amount) možete biti ugrađeni i uvode se sa Azure strojnog učenja Studio, ali ovdje samo Pokazat ćemo kako rukovati slova pomoću binarni klasifikacije modela.

1. Dohvaćanje podataka u Azure ML **Uvoz podataka** modul dostupne u odjeljku **unos podataka i izlaz** . Dodatne informacije potražite u članku referenca stranice [modul za uvoz podataka](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) .
2. Odaberite **Vrste Hive upita** kao **izvora podataka** u oknu **Svojstva** .
3. Zalijepite sljedeću skriptu grozd u uređivaču **vrste Hive upita baze podataka**

        select * from nyc_stratified_sample;

4. Unesite URI HDInsight klaster (to može pronaći u Portal za Azure), Hadoop vjerodajnice, mjesto izlazne podatke i Azure naziv računa za spremište naziv/ključ/kontejner.

 ![23](./media/machine-learning-data-science-process-data-lake-walkthrough/23-reader-module-v3.PNG)  

Na slici u nastavku prikazuje se primjer eksperimenta binarni klasifikacija čitanja podataka iz tablice grozd.

 ![24](./media/machine-learning-data-science-process-data-lake-walkthrough/24-AML-exp.PNG)

Nakon stvaranja pokusa kliknite **Postavljanje web-servisa** --> **Predvidljivu web-servisa**

 ![25](./media/machine-learning-data-science-process-data-lake-walkthrough/25-AML-exp-deploy.PNG)

Pokretanje automatski stvara bilježenje rezultata eksperiment, kada se dovrši, kliknite **Implementacija web-servisa**

 ![26](./media/machine-learning-data-science-process-data-lake-walkthrough/26-AML-exp-deploy-web.PNG)

Nadzorna ploča službe za web uskoro će se prikazati:

 ![27](./media/machine-learning-data-science-process-data-lake-walkthrough/27-AML-web-api.PNG)


## <a name="summary"></a>Sažetak

S povećanim ovaj vodič stvorite okruženja znanstvenog podataka za stvaranje skalabilni završetka do kraja rješenja Azure podataka Lake. Ovo okruženje korišten za analizu velike javno skup podataka, poduzimanja Kanonski koracima postupka znanstvenog podataka iz podataka acquisition kroz tečaj model, a zatim implementacija modela kao web-servisa. U SQL korišten za obradu, istraživanje i uzorak podataka. Python i grozd su koristiti u sklopu Azure strojnog učenja Studio omogućuje stvaranje i implementaciju predvidljivu modela.

## <a name="whats-next"></a>Što je sljedeće?

Tečaj za [Timu podataka znanstvenog procesa (TDSP)](http://aka.ms/datascienceprocess) nalaze veze na temu koja opisuje svakog koraka u postupku napredne analize. Postoji niz vodiči detaljni popis na stranici [timu podataka znanstvenog procese](data-science-process-walkthroughs.md) koje će odražavati kako koristiti resurse i usluge u različitim scenarijima predvidljivu analize:

- [Proces znanstvenog timu podataka u akciji: pomoću SQL Data Warehouse](machine-learning-data-science-process-sqldw-walkthrough.md)
- [Proces znanstvenog timu podataka u akciji: korištenje klastere HDInsight Hadoop](machine-learning-data-science-process-hive-walkthrough.md)
- [Proces znanstvenog timu podataka: pomoću SQL Server](machine-learning-data-science-process-sql-walkthrough.md)
- [Pregled postupka znanstvenog podataka pomoću Spark Azure HDInsight](machine-learning-data-science-spark-overview.md)

