<properties
    pageTitle="Stvaranje značajke za podatke u programa Hadoop klaster korištenja upita grozd | Microsoft Azure"
    description="Primjeri grozd upite koje generiraju značajke u podataka pohranjenih u programa Azure HDInsight Hadoop klaster."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="hangzh;bradsev" />


#<a name="create-features-for-data-in-an-hadoop-cluster-using-hive-queries"></a>Stvaranje značajke za podatke u programa Hadoop klaster korištenje grozd upita

Ovaj dokument prikazuje kako stvoriti značajke za podatke pohranjene u programa Azure HDInsight Hadoop klaster korištenje grozd upita. Ove vrste Hive upita pomoću ugrađene vrste Hive korisnički definirane funkcije (UDF-ove), skripte za koje postoje.

Operacije potrebne za stvaranje značajke mogu biti intenzivno memorije. Performanse upita grozd postaje više ključnih u tim slučajevima, a mogu poboljšati po usklađivanje određene parametre. Usklađivanje parametara opisan u odjeljku konačna.

Primjeri upiti koje su prikazane su specifične za [NEW taksi putovanja podataka](http://chriswhong.com/open-data/foil_nyc_taxi/) scenariji također se navode u [spremištu Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts). Tih upita već imate navedeni shema podataka i spremni ste za slanje da biste pokrenuli. U odjeljku konačni također se spominju parametre koje korisnici mogu podesite tako da se mogu poboljšati performanse grozd upita.

[AZURE.INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]U ovom **izborniku** navode veze na temu koja opisuje kako stvoriti značajke za podatke u različitim okruženjima. Ovaj zadatak je korak u [Timu podataka znanstvenog procesa (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).


## <a name="prerequisites"></a>Preduvjeti
U ovom se članku pretpostavlja da imate:

* Stvorili račun Azure prostora za pohranu. Ako vam je potrebna upute, potražite u članku [Stvaranje računa za pohranu za Azure](../storage/storage-create-storage-account.md#create-a-storage-account)
* Dodjeli prilagođene klaster Hadoop sa servisom HDInsight.  Ako vam je potrebna upute, potražite u članku [Prilagodba Azure HDInsight Hadoop klastere za napredne analize](machine-learning-data-science-customize-hadoop-cluster.md).
* Podaci prenesena grozd tablica u klastere Azure HDInsight Hadoop. Ako nije, slijedite [Stvaranje i učitavanja podataka s tablicama grozd](machine-learning-data-science-move-hive-tables.md) da je najprije prenesete podataka grozd tablice.
* Omogućeno daljinski pristup klaster. Ako vam je potrebna upute, potražite u članku [pristup glave čvor od Hadoop klaster](machine-learning-data-science-customize-hadoop-cluster.md#headnode).


##<a name="hive-featureengineering"></a>Značajka generacije

U ovom ćete odjeljku opisane su nekoliko primjera načina u kojem značajke mogu biti generiranje korištenje grozd upita. Nakon što ste generiran dodatnim značajkama, možete ih dodati u stupcima u postojeću tablicu ili stvaranje nove tablice s dodatnim značajkama i primarni ključ, koji se spajaju pa s izvorna tablica. Evo primjera prikazivati:

1. [Učestalost temelji značajka generacije](#hive-frequencyfeature)
2. [Rizici Categorical varijable u binarni klasifikacija](#hive-riskfeature)
3. [Izdvajanje značajke iz polja datuma i vremena](#hive-datefeatures)
4. [Izdvajanje značajke iz tekstnog polja](#hive-textfeatures)
5. [Izračun udaljenost između GPS koordinata.](#hive-gpsdistance)

###<a name="hive-frequencyfeature"></a>Učestalost temelji značajka generacije

Često je korisno za izračun učestalosti razine categorical varijabla ili učestalost određenih kombinacija razine iz većeg broja categorical varijabli. Korisnici mogu koristiti sljedeću skriptu da biste izračunali te učestalosti:

        select
            a.<column_name1>, a.<column_name2>, a.sub_count/sum(a.sub_count) over () as frequency
        from
        (
            select
                <column_name1>,<column_name2>, count(*) as sub_count
            from <databasename>.<tablename> group by <column_name1>, <column_name2>
        )a
        order by frequency desc;


###<a name="hive-riskfeature"></a>Rizika Categorical varijabli u binarni klasifikacija

U binarni klasifikacija moramo pretvoriti numerička categorical varijable numerički značajki kada modelima koristi se samo poduzeti numerički značajki. To možete učiniti jer zamjenjuju svaku razinu numerička numerički rizika. U ovom ćete odjeljku Pokazat ćemo neki generički grozd upiti koja izračunavaju vrijednosti rizika (vjerojatnost zapisnika) categorical tjednog prikaza kalendara.


        set smooth_param1=1;
        set smooth_param2=20;
        select
            <column_name1>,<column_name2>,
            ln((sum_target+${hiveconf:smooth_param1})/(record_count-sum_target+${hiveconf:smooth_param2}-${hiveconf:smooth_param1})) as risk
        from
            (
            select
                <column_nam1>, <column_name2>, sum(binary_target) as sum_target, sum(1) as record_count
            from
                (
                select
                    <column_name1>, <column_name2>, if(target_column>0,1,0) as binary_target
                from <databasename>.<tablename>
                )a
            group by <column_name1>, <column_name2>
            )b

U ovom primjeru varijable `smooth_param1` i `smooth_param2` postavljene na glačanje rizika vrijednosti izračunate iz podataka. Rizicima imati rasponu između -Inf i Inf. Rizika > 0 označava vjerojatnost da je jednak 1 cilj veća od 0,5.

Nakon rizika izračunava se tablice, korisnici mogu dodijeliti rizika vrijednosti tablice spajanjem s tablicom rizika. Upit spojno grozd nije naveden u prethodnom odjeljku.

###<a name="hive-datefeatures"></a>Izdvajanje značajki iz polja datuma i vremena

Grozd dolazi sa skupom UDF-ove za obradu polja datuma i vremena. U grozd, je zadani oblik datuma i vremena ' gggg-MM-dd 00:00:00 "(" 1970-01-01 12:21:32 ", primjerice). U ovom ćete odjeljku Pokazat ćemo primjeri koji izdvojiti dan u mjesecu, mjesec iz polja datuma i vremena i druge primjeri koji pretvaranje niza datuma i vremena u obliku osim zadanog oblika datuma i vremena niz po zadanom oblikovanje.

        select day(<datetime field>), month(<datetime field>)
        from <databasename>.<tablename>;

Ovaj upit grozd pretpostavlja da u *& #60; polja datuma i vremena >* u zadani oblik datuma i vremena.

Ako se polje datuma i vremena nisu zadani oblik, morate prvo pretvoriti polja datuma i vremena u Unix vremensku oznaku, a zatim Pretvorba vremenskog pečata Unix datetime niz koji se nalazi u okviru zadani oblik. Kada je s datumom i vremenom u zadanom obliku, korisnici možete primijeniti ugrađene datetime UDF-ove za izdvajanje značajke.

        select from_unixtime(unix_timestamp(<datetime field>,'<pattern of the datetime field>'))
        from <databasename>.<tablename>;

U ovom upitu, ako u *& #60; polja datuma i vremena >* sadrži uzorak kao što su *26/03/2015 12:04:39*, *"& #60; uzorak polja datuma i vremena >'* mora biti `'MM/dd/yyyy HH:mm:ss'`. Da biste testirali, možete pokrenuti korisnika

        select from_unixtime(unix_timestamp('05/15/2015 09:32:10','MM/dd/yyyy HH:mm:ss'))
        from hivesampletable limit 1;

*Hivesampletable* u ovom upitu dolazi unaprijed instaliranih na sve klastere Azure HDInsight Hadoop prema zadanim postavkama kada su dodjeli skupina.


###<a name="hive-textfeatures"></a>Izdvajanje značajke iz tekstna polja

Kada tablicu vrste Hive ima polje teksta koji sadrži niz riječi koje su zarezima, razmake, sljedeći upit izdvaja duljinu niza i broj riječi u nizu.

        select length(<text field>) as str_len, size(split(<text field>,' ')) as word_num
        from <databasename>.<tablename>;

###<a name="hive-gpsdistance"></a>Izračun udaljenosti između skupova GPS koordinata.

Upit iz odjeljka možete izravno primjenjuju na SEBI taksi putovanja podatke. Svrha ovaj upit je da biste prikazali kako primijeniti ugrađene matematičkih funkcija u grozd da biste generirali značajke.

Polja koje se koriste u upit su GPS koordinate mjesta prijem i dropoff, pod nazivom *prijem\_dužinu*, *prijem\_zemljopisnu širinu*, *dropoff\_dužinu*, a *dropoff\_zemljopisnu širinu*. Upiti koji izračunati Izravni udaljenost između koordinate prijem i dropoff su:

        set R=3959;
        set pi=radians(180);
        select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude,
            ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
            *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
            *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
            /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
            +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*
            pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance
        from nyctaxi.trip
        where pickup_longitude between -90 and 0
        and pickup_latitude between 30 and 90
        and dropoff_longitude between -90 and 0
        and dropoff_latitude between 30 and 90
        limit 10;

Matematičkih jednadžbi koje izračun udaljenost između dva GPS koordinate pronaći ćete na web-mjestu <a href="http://www.movable-type.co.uk/scripts/latlong.html" target="_blank">Movable Type skripte</a> autor Peter Lapisu. U svoj Javascript funkciju `toRad()` je samo *lat_or_lon*pi/180*, koji se pretvara stupnjeve u radijane. Ovdje, *lat_or_lon * je zemljopisnu širinu i dužinu. Jer grozd unijeti funkciju `atan2`, ali pruža funkciju `atan`, `atan2` funkcija implementira se putem `atan` funkcija u gore navedeni upit grozd pomoću definiciju navedene u <a href="http://en.wikipedia.org/wiki/Atan2" target="_blank">Wikipediji</a>.

![Stvaranje radnog prostora](./media/machine-learning-data-science-create-features-hive/atan2new.png)

Cijeli popis grozd ugrađene korisnički definiranih funkcija pronaći ćete u odjeljku **Ugrađene funkcije** na <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-MathematicalFunctions" target="_blank">wiki vrste Hive Apache</a>).  

## <a name="tuning"></a>Dodatne teme: podešavanje vrste Hive parametara da biste poboljšali brzinu upita

Zadana postavka parametar grozd klaster možda neće biti prikladna za upite grozd i podatke koji su obrade upita. U ovom se odjeljku ćemo razmotriti neke parametre koje korisnici mogu ugađanje koje poboljšati performanse grozd upita. Korisnici morate dodati parametar usklađivanje upita prije upiti obrada podataka.

1. **Prostor Java skupova**: za upite koje obuhvaćaju pridruživanje velikih skupova podataka ili obradu dugo zapisa, **dovoljno mjesta skupova** jedan je od uobičajenih pogrešaka. To može biti je počeo gledati postavljanjem parametri *mapreduce.map.java.opts* i *mapreduce.task.io.sort.mb* željene vrijednosti. Evo jednog primjera:

        set mapreduce.map.java.opts=-Xmx4096m;
        set mapreduce.task.io.sort.mb=-Xmx1024m;


    Ovaj parametar dodjeljuje 4GB memorije Java skupova prostor i i zahvaljujući sortiranje učinkovitiji dodjeljivanje dodatnu memoriju za njega. Nije dobro isprobavati te alokacija postoje li bilo koji zadatak pogreška pogreške vezane uz skupova razmak.

2. **DFS blokirati veličina** : ovaj parametar postavlja Najmanja jedinica podataka u kojoj su pohranjeni u datotečnom sustavu. Na primjer, ako je veličina bloka DFS 128MB, zatim Svi podaci veličina manja od i do 128MB pohranjena u jedan blok, tijekom podataka koji je veći od 128MB govornik dodatni blokova. Odabir veličina vrlo malen bloka uzrokuje velike folije u Hadoop jer je čvor naziva za obradu mnogo više zahtjeva da biste pronašli odgovarajući blok vezani uz u datoteku. Preporučenim postavkama kada povezanima s gigabajta (ili veća) podaci:

        set dfs.block.size=128m;

3. **Optimizacija uključivanje operacije u grozd** : dok operacija spajanja u mapu/smanjivanje framework obično odvija u smanjivanje fazi, katkad je Ogromno pozitivne mogu postići zakazivanjem spojevi u fazi karte (naziva se i "mapjoins"). Da biste usmjerili grozd da biste to učinili kad god je moguće, ne možemo možete postaviti:

        set hive.auto.convert.join=true;

4. **Određuje koliko će mappers grozd** : dok Hadoop omogućuje korisniku da biste postavili broj reducers, broj mappers obično je moguće postaviti korisnik. Ovisi o trik koji omogućuje donekle kontrole na taj broj je odaberite Hadoop varijable, *mapred.min.split.size* i *mapred.max.split.size* kao veličinu svakog zadatka mape:

        num_maps = max(mapred.min.split.size, min(mapred.max.split.size, dfs.block.size))

    Najčešće zadane vrijednosti *mapred.min.split.size* je 0, koji *mapred.max.split.size* je **Long.MAX** , a koji *dfs.block.size* 64 MB. Kao što je Vidimo, dali veličina podataka usklađivanje parametara "postavljanjem" ih zatražit podesite broj mappers koristi.

5. Nekoliko drugih više **Napredne mogućnosti** za optimiziranje performanse grozd navode se ispod. Te omogućuju vam da biste postavili memorije dodijeliti mapiranje i smanjiti zadaci, a može biti korisna u tweaking performansi. Imajte na umu *mapreduce.reduce.memory.mb* može biti veći od veličine fizičke memorije svakog čvora tempiranja klaster Hadoop.

        set mapreduce.map.memory.mb = 2048;
        set mapreduce.reduce.memory.mb=6144;
        set mapreduce.reduce.java.opts=-Xmx8192m;
        set mapred.reduce.tasks=128;
        set mapred.tasktracker.reduce.tasks.maximum=128;
