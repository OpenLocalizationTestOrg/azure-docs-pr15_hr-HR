<properties 
    pageTitle="Istraživanje podataka u SQL Server virtualnog računala na Azure | Microsoft Azure" 
    description="Upute za istraživanje podataka pohranjenih u programa SQL Server VM na Azure." 
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
    ms.date="09/13/2016" 
    ms.author="bradsev" /> 

#<a name="explore-data-in-sql-server-virtual-machine-on-azure"></a>Istraživanje podataka u SQL Server virtualnog računala na Azure


U ovom dokumentu opisuje kako istraživati podatke koji su pohranjenu u programa SQL Server VM na Azure. To možete učiniti tako wrangling podataka pomoću SQL ili pomoću programski jezik kao što su Python.

Sljedeće veze **izbornika** na temu koja opisuje kako pomoću alata za istraživanje podataka iz različitih okruženja za pohranu. Ovaj zadatak je korak u na Cortana analize procesa (KAPICA).

[AZURE.INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]


> [AZURE.NOTE] SQL naredbe uzorak u ovom dokumentu pretpostavlja da se podaci nalaze u SQL Server. Ako nije, pogledajte oblaka znanstvenog postupak karte da biste saznali kako možete premjestiti podataka sustava SQL Server.



## <a name="sql-dataexploration"></a>Istraživanje podataka SQL sa SQL skripte

Evo nekoliko ogledne skripte SQL koje je moguće koristiti za istraživanje podataka trgovine u sustavu SQL Server.

1. Broj opažanja prema danu

    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 

2. Dohvaćanje razina u categorical stupca

    `select  distinct <column_name> from <databasename>`

3. Početak broj razina u kombinaciji s dva stupca categorical 

    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`

4. Početak distribucije za brojčanih stupaca

    `select <column_name>, count(*) from <tablename> group by <column_name>`

> [AZURE.NOTE] Praktično primjerice, možete koristiti [dataset taksi SEBI](http://www.andresmh.com/nyctaxitrips/) i pogledajte IPNB pod naslovom [Podaci NEW wrangling pomoću IPython bilježnice i SQL Server](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) za walk-through do kraja do kraja.

##<a name="python"></a>Istraživanje podataka SQL s Python

Korištenje Python istraživati podatke i generiranje značajke kada se podaci nalaze u SQL Server je slična obrada podataka u blobova platforme Azure pomoću Python, kao što je navedenih u [blobova platforme Azure postupak podataka u svom okruženju znanstvenog podataka](machine-learning-data-science-process-data-blob.md). Podatke morate učitati iz baze podataka u pandas DataFrame, a zatim možete dodatno obraditi. Opisujemo postupke povezivanje s bazom podataka i učitavanje podataka u DataFrame u ovom odjeljku.

Sljedeći format niza za povezivanje može se koristiti za povezivanje s bazom podataka sustava SQL Server Python pomoću pyodbc (Zamijeni naziv poslužitelja, dbname, korisničko ime i lozinku za određene vrijednosti):

    #Set up the SQL Azure connection
    import pyodbc   
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

U [biblioteci Pandas](http://pandas.pydata.org/) u Python nudi bogatog skupa strukture podataka i alata za analizu podataka za rukovanje nizovima podataka za programiranjem Python. Sljedeći kod čita vraća rezultate iz baze podataka sustava SQL Server u okvir Pandas podataka:

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

Sada možete raditi s Pandas DataFrame u temi [blobova platforme Azure postupak podataka u svom okruženju znanstvenog podataka](machine-learning-data-science-process-data-blob.md).

## <a name="cortana-analytics-process-in-action-example"></a>Proces analize Cortane u primjeru akcija

Primjer završetka do kraja prođite kroz proces analize Cortana pomoću javnog skup podataka potražite u članku [u tim znanstvenog proces podataka u akciji: pomoću SQL Server](machine-learning-data-science-process-sql-walkthrough.md).

 
