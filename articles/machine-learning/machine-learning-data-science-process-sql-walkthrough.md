<properties
    pageTitle="Proces znanosti podataka tima u akciju: pomoću SQL Server | Microsoft Azure"
    description="Napredno Analytics proces i tehnologije akcija"  
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
    ms.author="fashah;bradsev"/>


# <a name="the-team-data-science-process-in-action-using-sql-server"></a>Proces znanosti podataka tima u akciju: pomoću SQL Server

U ovaj Praktični vodič, možete vodič izgradnji i implementaciji model učenja strojne grupe pomoću SQL Server i javno dostupni dataset--dataset [Trips taksi SEBI](http://www.andresmh.com/nyctaxitrips/) . Postupak slijedi tijeka rada znanosti standardnih podataka: ingest i istraživati podatke značajke inženjeru za olakšali učenje, a zatim izgraditi i uvođenje model.


## <a name="dataset"></a>NEW taksi Trips opis Dataset

Putovanje taksi NEW podataka je oko 20GB komprimirane datoteke CSV (dekomprimirati ~ 48GB), comprising pojedinačnih trips više od milijuna 173 i u fares plaćenu za svaki putovanje. Svaki zapis putovanja uključuje prijem i predaju mjesto i vrijeme, hack anonymized (upravljačkog programa) broj licence i broj medallion (jedinstveni id taksi 's). Podatke u godini 2013 pokriva sve trips i pružene u sljedeće dvije skupovima podataka za svaki mjesec:

1. 'trip_data' CSV sadrži detalje putovanja, kao što je broj passengers, prijem i dropoff točaka, trajanje putovanja i duljina putovanja. Evo nekoliko uzoraka zapisa:

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868

2. 'trip_fare' CSV sadrži detalje prijevoz plaćen za svaki putovanje kao što je vrsta uplate, iznos prijevoz, dodatna naknada i poreza, savjete i tolls, i ukupan iznos plaćen. Evo nekoliko uzoraka zapisa:

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Jedinstveni ključ za pridruživanje putovanja\_podataka i putovanja\_prijevoz sastoji od polja: medallion, hack\_licenčnim i prijem\_datetime.

## <a name="mltasks"></a>Primjeri zadataka predviđanje

Možemo će formulate tri predviđanje probleme na temelju u *Savjet\_iznos*, odnosno:

1. Binarni klasifikacija: predviđanje hoće li Savjet je plaćenu za putovanje, tj na *Savjet\_iznos* koji je veći od $0 je pozitivan primjer, dok je *Savjet\_iznos* $ 0 je negativan primjer.

2. Multiclass klasifikacija: za predviđanje raspon savjet plaćenu na putovanju. Možemo podijeliti na *Savjet\_iznos* u pet regalna skladišta ili klase:

        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20

3. Zadatak regresije: za predviđanje iznos savjet plaćenu za putovanje.  


## <a name="setup"></a>Postavka gore u Azure podataka znanosti okruženje za napredno analytics

Kao što možete vidjeti iz vodiča [Planiranje okruženje](machine-learning-data-science-plan-your-environment.md) , postoji nekoliko mogućnosti za rad s dataset NEW taksi Trips u Azure:

- Rad s podacima u Azure blob polja zatim model učenja Azure strojne grupe
- Učitavanje podataka u SQL Server bazu podataka zatim model učenja Azure strojne grupe

U ovaj vodič smo će demonstrirati paralelne Masovni uvoz podataka na SQL Server, u za pretraživanje podataka, značajka inženjerskih i dolje uzorkovanja pomoću SQL Server Management Studio kao i pomoću IPython bilježnice. [Ogledne skripte](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) i [IPython bilježnica](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks) se zajednički koriste u GitHub. Primjer bilježnice IPython za rad s podacima u Azure blob polja dostupna je na istom mjestu.

Da biste postavili okruženje znanosti Azure podataka:

1. [Stvaranje računa za pohranu](../storage/storage-create-storage-account.md)

2. [Stvaranje Azure stroj učenje radnog prostora](machine-learning-create-workspace.md)

3. [Dodjela resursa virtualni stroj znanosti podataka](machine-learning-data-science-setup-sql-server-virtual-machine.md), koja će služiti kao kao i poslužitelja SQL server IPython bilježnice.

    > [AZURE.NOTE] Ogledne skripte i IPython bilježnica će biti preuzete virtualni stroj znanosti podataka tijekom postupka postavljanja. Kada nakon instalacije skripte VM dovrši, uzorcima bit će u biblioteci dokumenata vašeg VM:  
    > - Uzorak skripte:`C:\Users\<user_name>\Documents\Data Science Scripts`  
    > - Uzorak IPython bilježnice:`C:\Users\<user_name>\Documents\IPython Notebooks\DataScienceSamples`  
    > gdje `<user_name>` je vaš VM Windows korisničko ime. Možemo će uzeti uzorak mape kao **Skripte uzoraka** i **Uzorak IPython bilježnice**.


Na temelju veličine dataset, mjesto izvora podataka i okruženje odabranu ciljnu Azure, ovaj scenarij je sličan [scenarij \#5: velike dataset lokalne datoteke ciljne SQL Server Azure VM](../machine-learning-data-science-plan-sample-scenarios.md#largelocaltodb).

## <a name="getdata"></a>Dohvati podatke iz javne izvora

Da biste dobili dataset [NEW taksi Trips](http://www.andresmh.com/nyctaxitrips/) iz javno mjesto, možda koristite bilo metode opisane u [Premjestiti podatke iz spremišta blobova Azure](machine-learning-data-science-move-azure-blob.md) za kopiranje podataka novi virtualni stroj.

Da biste kopirali podatke pomoću AzCopy:

1. Prijavite se u sustav virtualni stroj (VM)

2. Stvorite novi direktorij u VM podatkovni disk (Napomena: koristite privremene diska koja dolazi s VM kao podatkovni Disk).

3. U prozoru naredbeni redak, pokrenite sljedeće Azcopy naredbenog retka, zamjenu < path_to_data_folder > s vaše mape podataka kreirane u (2):

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S

    Kada AzCopy na dovrši, ukupno 24 zipane CSV datoteke (12 za putovanje\_podataka i za putovanje 12\_prijevoz) trebao bi biti u mapi podataka.

4. Raspakiraj preuzete datoteke. Imajte na umu mapu gdje se nalaze nekomprimirane datoteke. Ova mapa će biti nazivaju na < put\_za\_podataka\_datoteke\>.

## <a name="dbload"></a>Masovnog uvoza podataka u bazu podataka SQL poslužitelja

Performanse učitavanje/prijenos velike količine podataka SQL baze podataka i sljedećim upitima možete poboljšati pomoću _particije tablice i prikazi_. U ovoj sekciji će smo slijedite upute opisane u [Paralelni masovnog uvoza pomoću SQL particiju tablice podataka](machine-learning-data-science-parallel-load-sql-partitioned-tables.md) za stvaranje nove baze podataka i učitati podatke u particioniranom tablice paralelno.

1. Dok ste prijavljeni na vašem VM pokrenuti **SQL Server Management Studio**.

2. Poveži se koristeći Windows provjera autentičnosti.

    ![Povežite SSMS][12]

3. Ako još nije promijenio način provjere autentičnosti SQL Server i kreira novom korisniku prijavu za SQL, otvorite datoteku skripte pod nazivom **promijeniti\_auth.sql** u mapi **Skripte uzoraka** . Promijenite zadano korisničko ime i lozinku. Pritisnite **! Izvršavanje** u alatnoj traci pokrenuti skriptu.

    ![Izvršavanje skripti][13]

4. Provjerite i/ili promjena SQL Server zadane baza podataka i zapisnik mape da biste osigurali koji novostvoreni baza podataka će biti spremljen u podatkovni Disk. SQL Server VM slika koja je optimizirana za datawarehousing opterećenje je prethodno konfiguriranih s podataka i zapisnik diskova. Ako vaš VM ne uključuju podatkovni Disk i tijekom postupka postavljanja VM dodani novi virtualni tvrdi diskovi, promijenite zadane mape kako slijedi:

    - Desnom tipkom miša kliknite naziv SQL poslužitelja na lijevoj ploči i kliknite **Svojstva**.

        ![Svojstva poslužitelja SQL][14]

    - Odaberite **Postavke baze podataka** s popisa **Odaberite stranicu** ulijevo.

    - Provjerite i/ili promjena **baze podataka zadana mjesta** mjesta **Podatkovni Disk** vaš izbor. Ovo je gdje se nalaze nove baze podataka ako stvorili zadane postavke lokacije.

        ![Zadane postavke SQL baze podataka][15]  

5. Da biste stvorili novu bazu podataka i skup filegroups držite particioniranom tablice, otvorite skripte uzoraka **Stvaranje\_db\_default.sql**. Skripta će stvoriti novu bazu podataka pod nazivom **TaxiNYC** i 12 filegroups zadano mjesto podataka. Svaku grupu datoteka držite jedan mjesec putovanja\_podataka i putovanja\_prijevoz podataka. Po želji izmijenite naziv baze podataka. Pritisnite **! Izvršavanje** pokrenuti skriptu.

6. Zatim stvorite dvije tablice particiju jedan za putovanje svog\_podataka i drugi za putovanje svog\_prijevoz. Otvorite skripte uzoraka **Stvaranje\_particioniranom\_table.sql**, koji će:

    - Stvori particiju funkciju podijeliti podatke po mjesecu.
    - Stvaranje particije sheme za mapiranje podataka svaki mjesec u drugu grupu datoteka.
    - Stvorite dvije tablice particioniranom mapirani shemu particiju: **nyctaxi\_putovanje** će držite putovanje svog\_podataka i **nyctaxi\_prijevoz** će držite putovanje svog\_prijevoz podataka.

    Pritisnite **! Izvršavanje** za pokretanje skripti i stvaranje particioniranom tablica.

7. U mapi **Skripte uzoraka** postoje dva uzorka PowerShell skripte navedeni demonstrirati masovne za paralelne uvozi podataka SQL Server tablice.

    - **bcp\_paralelne\_generic.ps1** je generički skriptu da paralelna masovnog uvoza podataka u tablicu. Izmijenite skripte za postavljanje varijable unos i cilj kako je naznačeno u recima komentara u skriptu.
    - **bcp\_paralelne\_nyctaxi.ps1** je prethodno konfiguriranih verzija generički skripte i mogu se koristiti za učitavanje obje tablice za podatke Trips taksi SEBI.  

8. Desnom tipkom miša kliknite na **bcp\_paralelne\_nyctaxi.ps1** naziv skripte i kliknite **Uređivanje** da biste otvorili u PowerShellu. Pregledajte unaprijed varijable i izmijeniti prema naziv odabrane baze podataka, mapu za unos podataka, ciljnu mapu zapisnika i putove ogledni oblik datoteke **nyctaxi_trip.xml** i **nyctaxi\_fare.xml** (navedeni u mapi **Probne skripte** ).

    ![Masovnog uvoza podataka][16]

    Možda odaberite način provjere autentičnosti, zadani je provjera autentičnosti sustava Windows. Kliknite zelenu strelicu na alatnoj traci za pokretanje. Skripta će pokrenuti 24 operacije uvoza masovnog u paralelni, 12 za svaku particioniranom tablicu. Otvaranjem SQL Server zadanu mapu podataka kao skup iznad možda nadgledanje napretka uvoza podataka.

9. Skripta PowerShell izvješća početnog i završnog vremena. Kada sve masovno uvozi dovršeno prijavio je vrijeme dovršetka. Potvrdite ciljnu mapu zapisnika provjerite uvozi masovnog bile su uspješne, tj, Nema prijavljenih pogrešaka u ciljnu mapu zapisnika.

10. Bazu podataka sada je spreman za pretraživanje, značajka engineering i ostale operacije po želji. Budući da tablica particije obzirom na u **prijem\_datetime** polje, upiti koji sadrže **prijem\_datetime** uvjete u uvjetu **gdje** će im shemu particiju.

11. U **SQL Server Management Studio**Istraži navedeni primjer skripte **uzorka\_queries.sql**. Za pokretanje upita uzorka, istaknite retke upita, a zatim **! Izvršavanje** na alatnoj traci.

12. NEW taksi Trips podataka učitanih u dvije zasebne tablice. Da biste poboljšali spoj operacije, preporučljivo je indeksirati tablice. Ogledna skripta **Stvaranje\_particioniranom\_index.sql** stvara particioniranom indekse na ključ spoja kompozitni **medallion hack\_licenci i prijem\_datetime**.

## <a name="dbexplore"></a>Za pretraživanje podataka i inženjeringa značajka u SQL poslužitelja

U ovoj sekciji smo će izvršiti pretraživanje podataka i generiranje značajka pokretanjem SQL upita izravno u **SQL Server Management Studio** pomoću prethodno stvorili baze podataka SQL poslužitelja. Ogledna skripta pod nazivom **uzorka\_queries.sql** navedeni u mapi **Skripte uzoraka** . Ako se razlikuje od zadane preinačiti skriptu da biste promijenili naziv baze podataka: **TaxiNYC**.

U ovoj vježbi promijenit ćemo će:

- Povezivanje s **SQL Server Management Studio** ili Windows provjera autentičnosti pomoću značajke ili pomoću SQL provjeru autentičnosti i SQL korisničko ime i lozinku.
- Istražite distribucija podataka nekoliko polja u različitoj windows vrijeme.
- Istražili kvaliteta podataka dužinu i širinu polja.
- Generiranje naljepnica binarnim i multiclass klasifikacija temelji na u **Savjet\_iznos**.
- Generiranje značajke i izračunajte i Usporedba putovanja udaljenosti.
- Spojite dvije tablice i izdvojiti slučajni uzorak koji će se koristiti za izgradnju modela.

Kada ste spremni nastaviti Azure stroj učenje, možda ili:  

1. Spremanje konačni SQL upita za izdvajanje i uzorak podataka i kopiranja-lijepljenja upit izravno u [Uvoz podataka] [ import-data] modul Azure stroj učenje ili
2. Ustraj uzorkovanja i izgrađen podataka koje namjeravate koristiti za izgradnju nove tablice baze podataka model i koristiti novu tablicu u [Uvoz podataka] [ import-data] modul učenje Azure strojne grupe.

U ovoj sekciji smo će spremiti konačni upita za izdvajanje i uzorak podataka. Druga metoda planirati u sekciji [u za pretraživanje podataka i značajka inženjerstvo IPython bilježnice](#ipnb) .

Za brzu provjeru broj redaka i stupaca u tablicama popunjavaju ranije pomoću paralelne masovnog uvoza

    -- Report number of rows in table nyctaxi_trip without table scan
    SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('nyctaxi_trip')

    -- Report number of columns in table nyctaxi_trip
    SELECT COUNT(*) FROM information_schema.columns WHERE table_name = 'nyctaxi_trip'

#### <a name="exploration-trip-distribution-by-medallion"></a>Pretraživanje: Putovanja raspodjele po medallion

Ovaj primjer identificira medallion (taksi brojevi) s više od 100 trips unutar određenog vremenskog razdoblja. Upit želite pogodnost access tablicu u particioniranom otkad je conditioned prema shemi particija **prijem\_datetime**. Ispitivanje puni dataset također Provjerite koristite particioniranom tablice i/ili indeksirati skeniranja.

    SELECT medallion, COUNT(*)
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    GROUP BY medallion
    HAVING COUNT(*) > 100

#### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a>Pretraživanje: Putovanja raspodjele po medallion i hack_license

    SELECT medallion, hack_license, COUNT(*)
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20130131'
    GROUP BY medallion, hack_license
    HAVING COUNT(*) > 100

#### <a name="data-quality-assessment-verify-records-with-incorrect-longitude-andor-latitude"></a>Procjena kvalitete podataka: Provjerite zapise s neispravna Zemljopisna dužina i/ili zemljopisna širina

Ovaj primjer istražuje stanje Ako nijedno od dužine i/ili zemljopisna širina polja ili sadrži nevaljanu vrijednost (stupnjeva radian mora biti između -90 i 90), ili ste (0, 0) koordinate.

    SELECT COUNT(*) FROM nyctaxi_trip
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(pickup_latitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_latitude AS float) NOT BETWEEN -90 AND 90
    OR    (pickup_longitude = '0' AND pickup_latitude = '0')
    OR    (dropoff_longitude = '0' AND dropoff_latitude = '0'))

#### <a name="exploration-tipped-vs-not-tipped-trips-distribution"></a>Za pretraživanje: Tipped nasuprot nije Tipped Trips raspodjele

Ovaj primjer pronalazi broj trips su tipped nasuprot ne tipped u određeno vrijeme razdoblje (ili u punu dataset ako koji pokriva cijelu godinu). Ovu raspodjelu odražava raspodjele binarni natpis će se kasnije koristiti za Modeliranje binarni klasifikaciju.

    SELECT tipped, COUNT(*) AS tip_freq FROM (
      SELECT CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped, tip_amount
      FROM nyctaxi_fare
      WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tipped

#### <a name="exploration-tip-classrange-distribution"></a>Za pretraživanje: Klasa raspon raspodjele savjete

Ovaj primjer izračunava raspodjele savjet raspone u određenom vremenskom razdoblju (ili u punu dataset ako koji pokriva cijelu godinu). Ovo je raspodjele klase natpis koji će se koristiti kasnije za Modeliranje multiclass klasifikaciju.

    SELECT tip_class, COUNT(*) AS tip_freq FROM (
        SELECT CASE
            WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tip_class

#### <a name="exploration-compute-and-compare-trip-distance"></a>Pretraživanje: Izračunati i usporedite udaljenost putovanja

Ovaj primjer pretvara Zemljopisna dužina prijem i predaju i zemljopisna širina SQL Zemljopis točke, izračunava udaljenost putovanja pomoću SQL Zemljopis točke razlika i vraća slučajni uzorak rezultate usporedbe. U primjeru Ograničava rezultate valjani koordinate samo pomoću upita procjenu kvalitete podataka pokriveni ranije.

    SELECT
    pickup_location=geography::STPointFromText('POINT(' + pickup_longitude + ' ' + pickup_latitude + ')', 4326)
    ,dropoff_location=geography::STPointFromText('POINT(' + dropoff_longitude + ' ' + dropoff_latitude + ')', 4326)
    ,trip_distance
    ,computedist=round(geography::STPointFromText('POINT(' + pickup_longitude + ' ' + pickup_latitude + ')', 4326).STDistance(geography::STPointFromText('POINT(' + dropoff_longitude + ' ' + dropoff_latitude + ')', 4326))/1000, 2)
    FROM nyctaxi_trip
    tablesample(0.01 percent)
    WHERE CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND   CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'

#### <a name="feature-engineering-in-sql-queries"></a>Značajka Engineering u SQL upita

Natpis generiranje i Zemljopis pretvorbe za pretraživanje upiti mogu se koristiti i za generiranje natpise značajke uklanjanjem dijela inventure. Dodatna značajka inženjerskih SQL Primjeri su pružene u sekciji [u za pretraživanje podataka i značajka inženjerstvo IPython bilježnice](#ipnb) . Je učinkovitije pokrenuti značajku generiranje upite na puni dataset ili veliki podskup ga pomoću SQL upita koji se izvode izravno na SQL Server instanca baze podataka. Upiti se mogu izvršiti u **SQL Server Management Studio**, IPython bilježnice ili bilo koji alat/okruženje za razvoj kojem možete pristupiti bazi podataka lokalno ili udaljeno.

#### <a name="preparing-data-for-model-building"></a>Priprema podataka za sastavni modela

Sljedeći upit spojevi u **nyctaxi\_putovanje** i **nyctaxi\_prijevoz** tablice, generira u binarni klasifikacija natpis **tipped**, natpis klasifikacija više klase **Savjet\_klase**, i izdvaja 1% slučajni uzorak iz cijelog spojena dataset. Ovaj upit možete kopirati zatim zalijepili izravno u [Azure stroj učenje Studio](https://studio.azureml.net) [Uvoz podataka] [ import-data] modul za ingestion direktne podacima iz instanca SQL Server baze podataka u Azure. Upit isključuje zapise s neispravna (0, 0) koordinate.

    SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,  f.total_amount, f.tip_amount,
        CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped,
        CASE WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM nyctaxi_trip t, nyctaxi_fare f
    TABLESAMPLE (1 percent)
    WHERE t.medallion = f.medallion
    AND   t.hack_license = f.hack_license
    AND   t.pickup_datetime = f.pickup_datetime
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'


## <a name="ipnb"></a>Za pretraživanje podataka i inženjeringa značajka u bilježnici IPython

U ovoj sekciji smo će izvršiti pretraživanje podataka i generiranje značajka pomoću Python i SQL upite odabiranja prethodno stvorili baze podataka SQL poslužitelja. Primjer bilježnice IPython s nazivom **machine-Learning-data-science-process-sql-story.ipynb** je ugrađena u mapu **Bilježnice IPython uzorka** . Bilježnica je dostupna na [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks).

Preporučena slijed kada radite s veliki podataka je sljedeće:

- Čitanje malih uzoraka podataka u okvir podataka u memoriju.
- Izvođenje neke vizualizacije i explorations pomoću sampled podataka.
- Eksperimentirajte s značajka engineering pomoću sampled podataka.
- Za veće podataka za pretraživanje, rukovanje podacima i inženjeringa značajka koristite Python izravno protiv baze podataka SQL poslužitelja Azure VM izdati SQL upita.
- Odlučiti veličina uzorka za sastavni model učenja Azure strojne grupe.

Kada budete spremni nastaviti Azure stroj učenje, možda ili:  

1. Spremanje konačni SQL upita za izdvajanje i uzorak podataka i kopiranja-lijepljenja upit izravno u [Uvoz podataka] [ import-data] modul učenje Azure strojne grupe. Ova metoda planirati u sekciji [Sastavni modele u učenje Azure strojne grupe](#mlmodel) .    
2. Ustraj uzorkovanja i izgrađen podataka namjeravate koristiti za model izgradnji u novu tablicu baze podataka, a zatim koristiti nove tablice u [Uvoz podataka] [ import-data] modula.

Sljedeće su nekoliko u za pretraživanje podataka vizualizaciju podataka te značajka inženjerskih primjeri. Za više primjera pogledajte primjer bilježnice SQL IPython u mapu **Bilježnice IPython uzorka** .

#### <a name="initialize-database-credentials"></a>Inicijalizacija vjerodajnice baze podataka

Inicijalizacija postavke veze baze podataka u sljedeće varijable:

    SERVER_NAME=<server name>
    DATABASE_NAME=<database name>
    USERID=<user name>
    PASSWORD=<password>
    DB_DRIVER = <database server>

#### <a name="create-database-connection"></a>Stvaranje veze baze podataka

    CONNECTION_STRING = 'DRIVER={'+DRIVER+'};SERVER='+SERVER_NAME+';DATABASE='+DATABASE_NAME+';UID='+USERID+';PWD='+PASSWORD
    conn = pyodbc.connect(CONNECTION_STRING)

#### <a name="report-number-of-rows-and-columns-in-table-nyctaxitrip"></a>Izvješće broj redaka i stupaca u tablicu nyctaxi_trip

    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('nyctaxi_trip')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('nyctaxi_trip')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

- Ukupan broj redaka = 173179759  
- Ukupan broj stupaca = 14

#### <a name="read-in-a-small-data-sample-from-the-sql-server-database"></a>Uzorak mali podataka iz baze podataka SQL poslužitelja u čitanje

    t0 = time.time()

    query = '''
        SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax,
            f.tolls_amount, f.total_amount, f.tip_amount
        FROM nyctaxi_trip t, nyctaxi_fare f
        TABLESAMPLE (0.05 PERCENT)
        WHERE t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
    '''

    df1 = pd.read_sql(query, conn)

    t1 = time.time()
    print 'Time to read the sample table is %f seconds' % (t1-t0)

    print 'Number of rows and columns retrieved = (%d, %d)' % (df1.shape[0], df1.shape[1])

Vrijeme za čitanje primjera tablice je 6.492000 sekundi  
Broj redaka i stupaca dohvaća = (84952, 21)

#### <a name="descriptive-statistics"></a>Opisna statistika

Sada spremni istraživati podatke sampled. Možemo započeti s pogledate Opisna statistika za u **putovanje\_udaljenost** (ili bilo koje drugo) polja:

    df1['trip_distance'].describe()

#### <a name="visualization-box-plot-example"></a>Vizualizacija: Primjer crtanja okvir

Dalje ćemo pogledati crtanja okvir za putovanje udaljenost vizualizirati u quantiles

    df1.boxplot(column='trip_distance',return_type='dict')

![Iscrtaj #1][1]

#### <a name="visualization-distribution-plot-example"></a>Vizualizacija: Primjer crtanja raspodjele

    fig = plt.figure()
    ax1 = fig.add_subplot(1,2,1)
    ax2 = fig.add_subplot(1,2,2)
    df1['trip_distance'].plot(ax=ax1,kind='kde', style='b-')
    df1['trip_distance'].hist(ax=ax2, bins=100, color='k')

![Iscrtaj #2][2]

#### <a name="visualization-bar-and-line-plots"></a>Vizualizacije: Traka i crta iscrtavaju

U ovom primjeru ćemo putovanja udaljenost u pet regalna skladišta regalnog skladišta i vizualizirati binning rezultate.

    trip_dist_bins = [0, 1, 2, 4, 10, 1000]
    df1['trip_distance']
    trip_dist_bin_id = pd.cut(df1['trip_distance'], trip_dist_bins)
    trip_dist_bin_id

Možemo iscrtati iznad distribuciju regalnog skladišta u trake ili crte crtanja kao dolje

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='bar')

![Iscrtaj #3][3]

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='line')

![Iscrtaj #4][4]

#### <a name="visualization-scatterplot-example"></a>Vizualizacije: Primjer Scatterplot

Pokaži smo raspršeni crtanja između **putovanje\_vrijeme\_u\_sekunde** i **putovanje\_udaljenost** da biste vidjeli postoji li bilo korelacije

    plt.scatter(df1['trip_time_in_secs'], df1['trip_distance'])

![Iscrtaj #6][6]

Slično tome možete provjeriti odnos između **stopa\_kod** i **putovanje\_udaljenost**.

    plt.scatter(df1['passenger_count'], df1['trip_distance'])

![Iscrtaj #8][8]

### <a name="sub-sampling-the-data-in-sql"></a>Pod uzorkovanja podataka SQL

Prilikom pripreme podataka za model gradi [Azure stroj učenje Studio](https://studio.azureml.net), možete odlučiti na **SQL upitu koristiti izravno u modul za uvoz podataka** ili Ustraj izgrađen i uzorkovanja podatke u novu tablicu koju nije moguće koristiti u [Uvoz podataka] [ import-data] modul za jednostavan * *Odaberite* iz < vaše\_nove\_tablice\_ime >**.

U ovoj sekciji smo će stvoriti novu tablicu držite uzorkovanja i izgrađen podataka. Primjer direktni SQL upit za model sastavni je ugrađena u sekciji [u za pretraživanje podataka i značajka inženjerskih u SQL Server](#dbexplore) .

#### <a name="create-a-sample-table-and-populate-with-1-of-the-joined-tables-drop-table-first-if-it-exists"></a>Stvaranje uzorka tablicu i popunjavanje 1% od spojenih tablica. Ispustite tablica prvi ako on postoji.

U ovoj sekciji smo spojite tablice **nyctaxi\_putovanje** i **nyctaxi\_prijevoz**, izdvojiti 1% slučajni uzorak i Ustraj sampled podataka u novi naziv tablice **nyctaxi\_jedan\_posto**:

    cursor = conn.cursor()

    drop_table_if_exists = '''
        IF OBJECT_ID('nyctaxi_one_percent', 'U') IS NOT NULL DROP TABLE nyctaxi_one_percent
    '''

    nyctaxi_one_percent_insert = '''
        SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount, f.total_amount, f.tip_amount
        INTO nyctaxi_one_percent
        FROM nyctaxi_trip t, nyctaxi_fare f
        TABLESAMPLE (1 PERCENT)
        WHERE t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
        AND   pickup_longitude <> '0' AND dropoff_longitude <> '0'
    '''

    cursor.execute(drop_table_if_exists)
    cursor.execute(nyctaxi_one_percent_insert)
    cursor.commit()

### <a name="data-exploration-using-sql-queries-in-ipython-notebook"></a>Pomoću SQL upita u bilježnici IPython u za pretraživanje podataka

U ovoj sekciji možemo istražiti distribucija podataka korištenjem 1% uzorkovanja podataka koji postojane u novoj tablici smo stvorena iznad. Imajte na umu da slične explorations možete izvršiti pomoću izvorna tablica, po izboru pomoću **TABLESAMPLE** ograničiti pretraživanje uzorak ili ograničavanjem rezultate određenog vremenskog razdoblja pomoću u **prijem\_datetime** particije, kao što je prikazano u sekciji [u za pretraživanje podataka i značajka inženjerskih u SQL Server](#dbexplore) .

#### <a name="exploration-daily-distribution-of-trips"></a>Pretraživanje: Dnevno raspodjele trips

    query = '''
        SELECT CONVERT(date, dropoff_datetime) AS date, COUNT(*) AS c
        FROM nyctaxi_one_percent
        GROUP BY CONVERT(date, dropoff_datetime)
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-per-medallion"></a>Pretraživanje: Putovanja raspodjele po medallion

    query = '''
        SELECT medallion,count(*) AS c
        FROM nyctaxi_one_percent
        GROUP BY medallion
    '''

    pd.read_sql(query,conn)

### <a name="feature-generation-using-sql-queries-in-ipython-notebook"></a>Značajka generiranje pomoću SQL upita u bilježnici IPython

U ovoj sekciji smo će generirati nove natpise i značajke izravno pomoću SQL upita operacijski 1% primjera tablice smo stvorili u prethodnoj sekciji.

#### <a name="label-generation-generate-class-labels"></a>Generiranje oznake: Generiranje naljepnica klasa

U sljedećem primjeru smo generirati dva skupa natpise koristite za Modeliranje:

1. Binarni natpise klase **tipped** (predviđanje ako će dobiti savjet)
2. Multiclass naljepnice **Savjet\_klase** (predviđanje savjet regalnog skladišta ili raspon)

        nyctaxi_one_percent_add_col = '''
            ALTER TABLE nyctaxi_one_percent ADD tipped bit, tip_class int
        '''

        cursor.execute(nyctaxi_one_percent_add_col)
        cursor.commit()

        nyctaxi_one_percent_update_col = '''
            UPDATE nyctaxi_one_percent
            SET
               tipped = CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END,
               tip_class = CASE WHEN (tip_amount = 0) THEN 0
                                WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
                                WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
                                WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
                                ELSE 4
                            END
        '''

        cursor.execute(nyctaxi_one_percent_update_col)
        cursor.commit()

#### <a name="feature-engineering-count-features-for-categorical-columns"></a>Značajka Engineering: Brojanje značajke za Categorical stupaca

Ovaj primjer pretvorbe categorical polje u numeričko polje zamjenjujući svaku kategoriju s count njegova pojavljivanja u podacima.

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent ADD cmt_count int, vts_count int
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        WITH B AS
        (
            SELECT medallion, hack_license,
                SUM(CASE WHEN vendor_id = 'cmt' THEN 1 ELSE 0 END) AS cmt_count,
                SUM(CASE WHEN vendor_id = 'vts' THEN 1 ELSE 0 END) AS vts_count
            FROM nyctaxi_one_percent
            GROUP BY medallion, hack_license
        )

        UPDATE nyctaxi_one_percent
        SET nyctaxi_one_percent.cmt_count = B.cmt_count,
            nyctaxi_one_percent.vts_count = B.vts_count
        FROM nyctaxi_one_percent A INNER JOIN B
        ON A.medallion = B.medallion AND A.hack_license = B.hack_license
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="feature-engineering-bin-features-for-numerical-columns"></a>Značajka Engineering: Regala značajke za numeričke stupce

Ovaj primjer pretvorbe neprekinuti numeričko polje u gotovu konfiguraciju kategorija raspona, tj, transformacija numeričko polje u categorical polje.

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent ADD trip_time_bin int
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        WITH B(medallion,hack_license,pickup_datetime,trip_time_in_secs, BinNumber ) AS
        (
            SELECT medallion,hack_license,pickup_datetime,trip_time_in_secs,
            NTILE(5) OVER (ORDER BY trip_time_in_secs) AS BinNumber from nyctaxi_one_percent
        )

        UPDATE nyctaxi_one_percent
        SET trip_time_bin = B.BinNumber
        FROM nyctaxi_one_percent A INNER JOIN B
        ON A.medallion = B.medallion
        AND A.hack_license = B.hack_license
        AND A.pickup_datetime = B.pickup_datetime
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="feature-engineering-extract-location-features-from-decimal-latitudelongitude"></a>Značajka Engineering: Izdvajanje mjesto značajke iz decimalni zemljopisna širina/Zemljopisna dužina

Ovaj primjer raščlanjuje Decimalno predstavljanje zemljopisna širina i/ili Zemljopisna dužina polje u više polja regija različite preciznosti kao što, država, Grad, grada, blok, itd. Imajte na umu da nova polja zemlj nije mapirano na stvarne lokacije. Informacije na lokacijama geocode mapiranje potražite [Bing karte REST Services](https://msdn.microsoft.com/library/ff701710.aspx).

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent
        ADD l1 varchar(6), l2 varchar(3), l3 varchar(3), l4 varchar(3),
            l5 varchar(3), l6 varchar(3), l7 varchar(3)
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        UPDATE nyctaxi_one_percent
        SET l1=round(pickup_longitude,0)
            , l2 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 1 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),1,1) ELSE '0' END     
            , l3 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 2 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),2,1) ELSE '0' END     
            , l4 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 3 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),3,1) ELSE '0' END     
            , l5 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 4 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),4,1) ELSE '0' END     
            , l6 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 5 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),5,1) ELSE '0' END     
            , l7 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 6 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),6,1) ELSE '0' END
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="verify-the-final-form-of-the-featurized-table"></a>Provjerite konačni obrasca tablice featurized

    query = '''SELECT TOP 100 * FROM nyctaxi_one_percent'''
    pd.read_sql(query,conn)

Sada ćemo spremni nastaviti s sastavni modela i model implementacije u [Učenje Azure strojne grupe](https://studio.azureml.net). Podaci su spremni za bilo problema predviđanja identificirani ranije, odnosno:

1. Binarni klasifikacija: predviđanje hoće li Savjet je platili za putovanje.

2. Multiclass klasifikacija: za predviđanje raspon savjet platiti prema prethodno definirane klase.

3. Zadatak regresije: za predviđanje iznos savjet plaćenu za putovanje.  


## <a name="mlmodel"></a>Sastavni modele u učenje Azure stroj

Da biste započeli vježbi modeliranja prijaviti u učenje stroj Azure radnog prostora. Ako još niste stvorili stroj učenje radnog prostora, pogledajte [Stvori prostor za učenje Azure strojne grupe](machine-learning-create-workspace.md).

1. Da biste započeli s Azure stroj učenje, pogledajte [što je Azure stroj učenje Studio?](machine-learning-what-is-ml-studio.md)

2. Prijavite se u sustav [Azure stroj učenje Studio](https://studio.azureml.net).

3. Studio Početna stranica pruža velik broj informacije, videozapise, vodiče, veze referenca module i drugim resursima. Za dodatne informacije o Azure stroj učenje konzultirajte [Azure učenje dokumentaciju strojne grupe](https://azure.microsoft.com/documentation/services/machine-learning/).

Tipične osposobljavanje eksperimentiraj sastoji se od sljedećeg:

1. Stvaranje eksperimentiraj **+ NOVO** .
2. Dohvaćanje podataka za učenje Azure strojne grupe.
3. Unaprijed obraditi pretvorbu i rukovanje podacima prema potrebi.
4. Generiranje značajke po potrebi.
5. Podijelite podataka osposobljavanje/provjere valjanosti/testiranje skupovima podataka (ili imaju zasebne skupovima podataka za svaku).
6. Odaberite jedan ili više algoritmi stroj učenje ovisno o problemu učenje rješavanja. Npr., binarni klasifikacija, multiclass klasifikacija, regresije.
7. Uvježbavanje jedan ili više modela pomoću osposobljavanje dataset.
8. Rezultat dataset provjere valjanosti pomoću obučeni model(s).
9. Procijeniti model(s) izračunati odgovarajuću metriku za učenje problem.
10. Precizno podešavanje na model(s) i odaberite najbolje model za uvođenje.

U ovoj vježbi promijenit ćemo imati već istražili izgrađen podatke u SQL Server i odlučili na veličinu uzorka ingest učenje Azure strojne grupe. Za izgradnju jedan ili više modeli predviđanje smo odlučili:

1. Dohvaćanje podataka Azure stroj Učenje korištenja [Uvoz podataka] [ import-data] modula, dostupne u sekciji **podataka ulaz i izlaz** . Dodatne informacije potražite u odjeljku [Uvoz podataka] [ import-data] modul referenca stranice.

    ![Učenje Azure stroj uvoz podataka][17]

2. Odaberite **Azure SQL baze podataka** kao **izvora podataka** na ploči **Svojstva** .

3. Unesite naziv DNS baze podataka u polje **naziv poslužitelja baze podataka** . Oblik:`tcp:<your_virtual_machine_DNS_name>,1433`

4. Unesite **Naziv baze podataka** u odgovarajućem polju.

5. Unesite **SQL korisničko ime** u u **naziv aqccount poslužitelja korisnika i lozinku u na **poslužitelju korisnički račun lozinku **.

6. Provjerite mogućnost **Prihvati sve certifikata poslužitelja** .

7. U područje teksta uređivanje **upita baze podataka** Zalijepi upita koji izdvaja potrebne polja (uključujući bilo izračunata polja kao što su natpisi) baze podataka i dolje uzorke podataka na željeni uzorak veličinu.

Primjer binarni klasifikacija eksperimenta čitanje podataka izravno iz SQL Server baze podataka je na slici dolje. Slično eksperimenata možete izvrtanjem za klasifikaciju multiclass i problema regresije.

![Podučavanje učenje Azure stroj][10]

> [AZURE.IMPORTANT] U podacima modeliranja izdvajanje uzorkovanje Primjeri upit dani i u prethodne sekcije, **sve natpise vježbama tri modeliranja su uključeni u upit**. Važno (obavezno) korak u svakom vježbama modeliranja je za **isključivanje** nepotrebnih natpisa za druge dvije probleme i bilo koji drugi **cilj curenje**. Npr., prilikom korištenja binarni klasifikacija koristite natpis **tipped** i izostavljanje polja **Savjet\_klase**, **Savjet\_iznos**, i **Ukupno\_iznos**. U potonjem su ciljne osipanjem Budući ukazuju savjet platiti.
>
> Za isključivanje nepotrebne stupce ili ciljnu osipanjem, možda koristite [Odaberite stupce u Dataset] [ select-columns] ili [Uređivanje metapodataka]modula[edit-metadata]. Za dodatne informacije pogledajte [Odaberite stupce u Dataset] [ select-columns] i [Uređivanje metapodataka] [ edit-metadata] referencu stranice.

## <a name="mldeploy"></a>Uvođenje modeli Azure stroj učenje

Kada je vaš model spreman, jednostavno ga može uvesti kao web-usluga izravno iz pokusa. Dodatne informacije o implementaciji Azure stroj učenje web-usluge potražite [uvođenja Azure stroj učenje web-usluge](machine-learning-publish-a-machine-learning-web-service.md).

Uvođenje nove web-usluge, morate:

1. Stvaranje bodovanja eksperimentiraj.
2. Uvođenje web-usluge.

Stvaranje bodovanja eksperimentiraj iz eksperimentiraj uvježbavanje **završeno** u donjem akcijske trake kliknite **Stvaranje BODOVANJA EKSPERIMENTIRATI** .

![Azure bodovanja][18]

Učenje Azure strojne grupe pokušat će stvoriti bodovanja eksperimentiraj komponente eksperimentiraj Osposobljavanje na temelju. Točnije, on će:

1. Spremanje modela obučeni i ukloniti module osposobljavanje modela.
2. Odredite logičke **ulazni priključak** za predstavljanje sheme očekivanih unos podataka.
3. Odredite logičke **izlazni priključak** za predstavljanje sheme izlaza očekivani web usluge.

Kada se kreira bodovanja eksperimentiraj, pregledajte ga i prema potrebi prilagodite. Tipična prilagođavanja je zamijenite unos dataset i/ili upita jedan koji isključuje natpis polja kao te neće biti dostupan kada naziva servisa. Također je dobro smanjiti veličinu unos dataset i/ili upita nekoliko zapisa dovoljno samo da biste naznačili unos sheme. Za izlazni priključak je uobičajena za uključivanje sva ulazna polja i samo **Oznake osvojila** i **Osvojila vjerojatnosti** izlaza pomoću [Odaberite stupce u Dataset] [ select-columns] modula.

Bodovanja eksperimentiraj uzorak je na slici dolje. Kada je spremna za uvođenje, kliknite gumb **OBJAVITI web-usluge** u donjem akcijske trake.

![Objavi Azure stroj učenje][11]

Da biste ovaj Praktični vodič recap ste stvorili znanosti okruženju Azure podataka radio velike javne dataset usidrili iz podataka nabave za model osposobljavanje i implementaciji Azure stroj učenje web-usluge.

### <a name="license-information"></a>Informacije o licenci

Ovaj vodič uzorak i njegov koja prati skripte i IPython notebook(s) zajednički koriste Microsoft pod licencom ograničenja. Provjerite datoteku LICENSE.txt u imeniku uzorak koda na GitHub za više detalja.

### <a name="references"></a>Reference

• [Andrés Monroy NEW taksi Trips preuzimanje stranice](http://www.andresmh.com/nyctaxitrips/)  
• [FOILing's NEW taksi putovanja podataka po Chris Whong](http://chriswhong.com/open-data/foil_nyc_taxi/)   
• [Taksi SEBI i Limousine provizije istraživanja i Statistika](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)


[1]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_26_1.png
[2]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_28_1.png
[3]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_35_1.png
[4]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_36_1.png
[5]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_39_1.png
[6]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_42_1.png
[7]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_44_1.png
[8]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_46_1.png
[9]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_71_1.png
[10]: ./media/machine-learning-data-science-process-sql-walkthrough/azuremltrain.png
[11]: ./media/machine-learning-data-science-process-sql-walkthrough/azuremlpublish.png
[12]: ./media/machine-learning-data-science-process-sql-walkthrough/ssmsconnect.png
[13]: ./media/machine-learning-data-science-process-sql-walkthrough/executescript.png
[14]: ./media/machine-learning-data-science-process-sql-walkthrough/sqlserverproperties.png
[15]: ./media/machine-learning-data-science-process-sql-walkthrough/sqldefaultdirs.png
[16]: ./media/machine-learning-data-science-process-sql-walkthrough/bulkimport.png
[17]: ./media/machine-learning-data-science-process-sql-walkthrough/amlreader.png
[18]: ./media/machine-learning-data-science-process-sql-walkthrough/amlscoring.png


<!-- Module References -->
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
