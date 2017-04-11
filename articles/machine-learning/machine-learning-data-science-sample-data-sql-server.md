<properties 
    pageTitle="Uzorak podataka u sustavu SQL Server na Azure | Microsoft Azure" 
    description="Ogledne podatke iz sustava SQL Server na Azure" 
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
    ms.author="fashah;garye;bradsev" /> 

#<a name="heading"></a>Ogledne podatke iz sustava SQL Server na Azure


Ovaj dokument pokazuje kako ogledne podatke pohranjene u sustavu SQL Server na Azure SQL ili Python programski jezik. Također se prikazuje kako premjestiti ogledne podatke u Azure strojnog učenja tako da ga spremite na datoteku, prenosi blobova platforme Azure, a zatim je pročitate u Azure strojnog učenja Studio.

Uzorkovanje Python koristi biblioteka ODBC [pyodbc](https://code.google.com/p/pyodbc/) da biste se povezali sa sustavom SQL Server na Azure i [Pandas](http://pandas.pydata.org/) biblioteka za stvaranje uzoraka.

>[AZURE.NOTE] Ogledna SQL koda u ovom dokumentu pretpostavlja da se podaci nalaze u SQL Server na Azure. Ako nije, pogledajte temi [Premještanje podataka za SQL Server na Azure](machine-learning-data-science-move-sql-server-virtual-machine.md) upute za premještanje podataka sustava SQL Server na Azure.

**Zašto uzorak podataka?**
Ako je prevelik dataset plan za analizu, obično je dobro dolje ogledne podatke da biste smanjili veličinu manje no predstavniku i jednostavnije. To olakšava razumijevanje podataka, istraživanje i inženjering značajke. Njegova uloga u [Timu podataka znanstvenog procesa (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) jest omogućivanje brzo prototyping funkcija obradu podataka i strojnog učenja modela.

Na **izborniku** ispod veze na temu koja opisuje kako ogledne podatke iz različitih okruženja za pohranu. 

[AZURE.INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

Ovaj zadatak uzorkovanje je korak u [Timu podataka znanstvenog procesa (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

##<a name="SQL"></a>Pomoću SQL

U ovom se odjeljku opisuju nekoliko načina pomoću SQL za izvođenje jednostavne slučajno uzorkovanje na temelju podataka u bazi podataka. Odaberite način ovisno o veličini podataka i njegov raspodjele.

Dvije stavke ispod pokazalo kako se koristi newid u sustavu SQL Server da biste izvršili u uzorkovanje. Način koji ćete odabrati ovisi o tome kako slučajni uzorak se želite (pk_id u primjeru koda pretpostavlja se da automatski generirani primarnog ključa).

1. Manje izričite slučajni uzorak

        select  * from <table_name> where <primary_key> in 
        (select top 10 percent <primary_key> from <table_name> order by newid())

2. Više slučajni uzorak 

        SELECT * FROM <table_name>
        WHERE 0.1 >= CAST(CHECKSUM(NEWID(), <primary_key>) & 0x7fffffff AS float)/ CAST (0x7fffffff AS int)

Tablesample mogu se koristiti za stvaranje uzoraka kao i što je prikazano ispod. Ako je veličina poštanskog podataka velik (uz pretpostavku da podatke na različitim stranicama se povezuju) i za upit da biste dovršili u pametnije vremenu možda bolje pristup.

    SELECT *
    FROM <table_name> 
    TABLESAMPLE (10 PERCENT)

>[AZURE.NOTE] Možete istraživati i generiranje značajke iz ove sampled podatke spremanjem u novu tablicu


###<a name="sql-aml"></a>Povezivanje s Azure strojnog učenja

Izravno upite možete koristiti na ogledne iznad Azure strojnog učenja [Uvoz podataka] [ import-data] modul dolje ogledne podatke u hodu i dohvaćali pokusa Azure strojnog učenja. Snimka zaslona za čitanje sampled podataka pomoću modula čitač je prikazano u nastavku:
   
![čitač sql][1]

##<a name="python"></a>Korištenje Python programskom jeziku 

U ovom se odjeljku objašnjavaju pomoću [pyodbc biblioteke](https://code.google.com/p/pyodbc/) da biste uspostavili programa ODBC povezivanje s bazom podataka sustava SQL server u Python. Niz za povezivanje baze podataka je kako slijedi: (zamijeniti naziv poslužitelja, dbname, korisničko ime i lozinku s konfiguracijom):

    #Set up the SQL Azure connection
    import pyodbc   
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

Biblioteka [Pandas](http://pandas.pydata.org/) u Python nudi bogatog skupa strukture podataka i alata za analizu podataka za rukovanje nizovima podataka za programiranjem Python. Kod ispod čita 0,1% ogledne podatke iz tablice u bazi podataka Azure SQL u Pandas podatke:

    import pandas as pd

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select column1, cloumn2... from <table_name> tablesample (0.1 percent)''', conn)

Sada možete raditi s sampled podacima u okvir Pandas podataka. 

###<a name="python-aml"></a>Povezivanje s Azure strojnog učenja

Sljedeći primjer kod možete koristiti da biste spremili dolje ogledne podatke u datoteku i prijenos blobova platforme Azure. Podaci u blob-om moguće izravno čitati u pokusa Azure strojnog učenja pomoću [Uvoz podataka] [ import-data] modul. Koraci su na sljedeći način: 

1. Zapisivanje podataka okvira pandas lokalne datoteke

        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)

2. Prijenos datoteke u lokalnu blobova platforme Azure

        from azure.storage import BlobService
        import tables

        STORAGEACCOUNTNAME= <storage_account_name>
        LOCALFILENAME= <local_file_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>

        output_blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)    
        localfileprocessed = os.path.join(os.getcwd(),LOCALFILENAME) #assuming file is in current working directory
        
        try:
       
        #perform upload
        output_blob_service.put_block_blob_from_path(CONTAINERNAME,BLOBNAME,localfileprocessed)
        
        except:         
            print ("Something went wrong with uploading blob:"+BLOBNAME)

3. Čitanje podataka iz blobova platforme Azure pomoću Azure strojnog učenja [Uvoz podataka] [ import-data] modul kao što je prikazano u nastavku hvatanje zaslona:
 
![čitač blob][2]

## <a name="the-team-data-science-process-in-action-example"></a>Proces znanstvenog timu podataka u primjeru akcija

Primjer završetka do kraja prođite kroz proces znanstvenog timu podataka pomoću javnog skup podataka potražite u članku [proces znanstvenog timu podataka u akciji: pomoću SQL poslužitelja](machine-learning-data-science-process-sql-walkthrough.md).

[1]: ./media/machine-learning-data-science-sample-sql-server-virtual-machine/reader_database.png
[2]: ./media/machine-learning-data-science-sample-sql-server-virtual-machine/reader_blob.png

 [import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
