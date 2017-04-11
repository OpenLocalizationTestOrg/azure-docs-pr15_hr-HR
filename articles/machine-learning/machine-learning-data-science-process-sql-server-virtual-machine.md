<properties 
    pageTitle="Obrada podataka iz SQL Azure | Microsoft Azure" 
    description="Postupak podaci iz SQL Azure" 
    services="machine-learning" 
    documentationCenter="" 
    authors="garyericson" 
    manager="jhubbard" 
    editor="" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/16/2016" 
    ms.author="fashah;garye;bradsev" /> 

#<a name="heading"></a>Postupak podataka u sustavu SQL Server virtualnog računala na Azure

U ovom dokumentu opisuje kako istraživati podatke i generiranje značajke za podatke pohranjene u programa SQL Server VM na Azure. To možete učiniti tako wrangling podataka pomoću SQL ili pomoću programski jezik kao što su Python.


> [AZURE.NOTE] SQL naredbe uzorak u ovom dokumentu pretpostavlja da se podaci nalaze u SQL Server. Ako nije, pogledajte oblaka znanstvenog postupak karte da biste saznali kako možete premjestiti podataka sustava SQL Server.

##<a name="SQL"></a>Pomoću SQL

Ne možemo opišite sljedeće zadatke wrangling podataka u ovom odjeljku pomoću SQL:

1. [Istraživanje podataka](#sql-dataexploration)
2. [Značajka generacije](#sql-featuregen)

###<a name="sql-dataexploration"></a>Istraživanje podataka
Evo nekoliko ogledne skripte SQL koje je moguće koristiti za istraživanje podataka trgovine u sustavu SQL Server.


> [AZURE.NOTE] Praktično primjerice, možete koristiti [dataset taksi SEBI](http://www.andresmh.com/nyctaxitrips/) i pogledajte IPNB pod naslovom [Podaci NEW wrangling pomoću IPython bilježnice i SQL Server](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) za walk-through do kraja do kraja.

1. Broj opažanja prema danu

    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 

2. Dohvaćanje razina u categorical stupca

    `select  distinct <column_name> from <databasename>`

3. Početak broj razina u kombinaciji s dva stupca categorical 

    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`

4. Početak distribucije za brojčanih stupaca

    `select <column_name>, count(*) from <tablename> group by <column_name>`


###<a name="sql-featuregen"></a>Značajka generacije

U ovom se odjeljku opisuju smo načina stvaranja značajke pomoću SQL:  

1. [Broj koji se temelje značajka generacije](#sql-countfeature)
2. [Binning značajka generacije](#sql-binningfeature)
3. [Vraćanje na značajke iz jednog stupca](#sql-featurerollout)


> [AZURE.NOTE] Kada stvoriti dodatne značajke, možete ih dodati u stupcima postojeće tablice ili stvaranje nove tablice s dodatnim značajkama i primarni ključ, koji se spajaju s izvorna tablica. 

###<a name="sql-countfeature"></a>Broj koji se temelje značajka generacije

Ovaj dokument prikazuje broj značajke za generiranje dva načina. Prva metoda koristi Uvjetni zbroj, a druga metoda koristi uvjet "gdje". Te možete pa se spajaju s izvorna tablica (pomoću stupaca primarnog ključa) da bi broj značajke uz izvorne podatke.

    select <column_name1>,<column_name2>,<column_name3>, COUNT(*) as Count_Features from <tablename> group by <column_name1>,<column_name2>,<column_name3> 

    select <column_name1>,<column_name2> , sum(1) as Count_Features from <tablename> 
    where <column_name3> = '<some_value>' group by <column_name1>,<column_name2> 

###<a name="sql-binningfeature"></a>Binning značajka generacije

Sljedeći primjer prikazuje način da biste generirali binned značajke po binning (pomoću 5 intervale) brojčanog stupca koje je moguće koristiti kao značajka umjesto:

    `SELECT <column_name>, NTILE(5) OVER (ORDER BY <column_name>) AS BinNumber from <tablename>`


###<a name="sql-featurerollout"></a>Vraćanje na značajke iz jednog stupca

U ovom ćete odjeljku smo Demonstracija snimljene fotografije iz jednog stupca u tablici da biste generirali dodatne značajke. U primjeru pretpostavlja da postoji zemljopisnu širinu i dužinu stupca u tablici iz kojeg želite generirati značajke.

Evo kratkog primer podataka o lokaciji zemljopisnu širinu i dužinu (resourced iz stackoverflow [kako mjerenje točnosti zemljopisnu širinu i dužinu?](http://gis.stackexchange.com/questions/8650/how-to-measure-the-accuracy-of-latitude-and-longitude)). To je korisno je znati prije featurizing polje mjesto:

- Znak nam govore smo jesu li Sjeverna ili Jug, Istok i Zapad globus.
- Osim nule stotine znamenku nam govore priznanje dužinu, ne zemljopisnu širinu!
- Na tens znamenku daje položaj da biste oko 1000 kilometrima. Pruža nam korisne informacije o koji kontinent i prezentacija Radimo na.
- Jedinice znamenku (stupanj jedan broja decimalnih mjesta) na položaj daje do 111 kilometrima (60 nautička milja, 69 milja). To možete nam reći otprilike značenje velike statusa ili država smo u.
- Prvi decimalna mjesta vrijedi do 11.1 km: može razlikovati položaj velikih gradova u susjednom velike grada.
- Drugi decimalna mjesta vrijedi do 1.1 km: ga odvojite seosku jedan od sljedećeg.
- Treći decimalna mjesta vrijedi od najviše 110: možete prepoznati velike agricultural polja ili institucijski campus.
- Četvrti decimalna mjesta vrijedi do 11 od: možete prepoznati paketu sustava. To je usporediti s uobičajeni točnosti uncorrected jedinica za GPS s bez smetnje.
- Peti decimalna mjesta vrijedi do 1.1 od: ga međusobno razlikovanje stabla. Točnost da ta razina s komercijalne GPS jedinica mogu se postići s razlikovno ispravak.
- Šesti decimalna mjesta vrijedi do 0.11 od: to možete koristiti za raspoređivanje strukture detaljno za dizajniranje landscapes, stvaranje roads. Mora biti više dovoljno dobar za praćenje premještanja glaciers i rivers. To se može postići prihvaćanjem painstaking mjere s GPS, kao što su differentially ispravnog GPS.

Podaci o mjestu može biti featurized na sljedeći način, razdvajanje regija, mjesto i podataka o gradu. Imajte na umu da OSTALE krajnju točku kao što je Bing karte API dostupna možete nazvati pri [pronašli mjesto za točku](https://msdn.microsoft.com/library/ff701710.aspx) da biste saznali regije/Okrug.

    select 
        <location_columnname>
        ,round(<location_columnname>,0) as l1       
        ,l2=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 1 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),1,1) else '0' end     
        ,l3=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 2 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),2,1) else '0' end     
        ,l4=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 3 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),3,1) else '0' end     
        ,l5=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 4 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),4,1) else '0' end     
        ,l6=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 5 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),5,1) else '0' end     
        ,l7=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 6 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),6,1) else '0' end     
    from <tablename>

Iznad značajke temeljena se dodatno poslužite da biste generirali count dodatne značajke kao što je opisano ranije. 


> [AZURE.TIP] Programski možete umetnuti zapise pomoću na jeziku po izboru. Možda ćete morati umetnuti podatke u blokova za poboljšanje učinkovitosti pisanje (na primjer kako to učiniti pomoću pyodbc, pročitajte članak [uzorka A HelloWorld da biste pristupili SQLServer s python](https://code.google.com/p/pypyodbc/wiki/A_HelloWorld_sample_to_access_mssql_with_python)). Drugi druga je da biste umetnuli podataka u bazu podataka pomoću [BCP utility](https://msdn.microsoft.com/library/ms162802.aspx).

###<a name="sql-aml"></a>Povezivanje s Azure strojnog učenja

Novi generirani značajke mogu biti dodane kao stupac postojećoj tablici ili pohranjene u novu tablicu i pridružen izvorna tablica za strojnog učenja. Značajke možete generirati ili pristupiti ako već stvorili, [Uvoz podataka] [ import-data] modul u Azure strojnog učenja kao što je prikazano u nastavku:

![čitatelji azureml][1] 

##<a name="python"></a>Pomoću programskog jezika kao što su Python

Korištenje Python istraživati podatke i generiranje značajke kada se podaci nalaze u SQL Server je slična obrada podataka u blobova platforme Azure pomoću Python navedenih u [blobova platforme Azure postupak podataka u koju podataka znanstvenog okruženje](machine-learning-data-science-process-data-blob.md). Podatke morate učitati iz baze podataka u okvir pandas podataka, a zatim možete dodatno obraditi. Opisujemo postupke povezivanje s bazom podataka i učitavanje podataka u okvir podaci u ovom odjeljku.

Sljedeći format niza za povezivanje može se koristiti za povezivanje s bazom podataka sustava SQL Server Python pomoću pyodbc (Zamijeni naziv poslužitelja, dbname, korisničko ime i lozinku za određene vrijednosti):

    #Set up the SQL Azure connection
    import pyodbc   
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

U [biblioteci Pandas](http://pandas.pydata.org/) u Python nudi bogatog skupa strukture podataka i alata za analizu podataka za rukovanje nizovima podataka za programiranjem Python. Kod ispod čita vraća rezultate iz baze podataka sustava SQL Server u okvir Pandas podataka:

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

Sada možete raditi s okvira Pandas podataka u članku [blobova platforme Azure postupak podataka u koju podataka znanstvenog okruženje](machine-learning-data-science-process-data-blob.md).

## <a name="azure-data-science-in-action-example"></a>Znanstvena Azure podataka u primjeru akcija

Primjer završetka do kraja prođite kroz proces znanstvenog podataka Azure pomoću javnog skup podataka potražite u članku [Proces znanstvenog Azure podataka u akciji](machine-learning-data-science-process-sql-walkthrough.md).

[1]: ./media/machine-learning-data-science-process-sql-server-virtual-machine/reader_db_featurizedinput.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
 
