<properties 
    pageTitle="Istraživanje podataka u spremište blobova platforme Azure s Pandas | Microsoft Azure" 
    description="Upute za istraživanje podataka koji je pohranjen u spremniku blobova platforme Azure pomoću Pandas." 
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
    ms.date="09/13/2016" 
    ms.author="bradsev" /> 

#<a name="explore-data-in-azure-blob-storage-with-pandas"></a>Istraživanje podataka u spremište blobova platforme Azure s Pandas

U ovom dokumentu opisuje kako istraživati podatke koji su pohranjenu u spremniku blobova platforme Azure pomoću [Pandas](http://pandas.pydata.org/) Python paketa.

Sljedeće veze **izbornika** na temu koja opisuje kako pomoću alata za istraživanje podataka iz različitih okruženja za pohranu. Ovaj zadatak je korak [Postupka znanstvenog podataka]().

[AZURE.INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]


## <a name="prerequisites"></a>Preduvjeti
U ovom se članku pretpostavlja da imate:

* Stvorili račun Azure prostora za pohranu. Ako vam je potrebna upute, potražite u članku [Stvaranje računa za pohranu za Azure](../storage/storage-create-storage-account.md#create-a-storage-account)
* Podatke pohranjene u račun za spremište blobova platforme Azure. Ako vam je potrebna upute, potražite u članku [Premještanje podataka i iz spremišta Azure](../storage/storage-moving-data.md)

## <a name="load-the-data-into-a-pandas-dataframe"></a>Učitavanje podatke u Pandas DataFrame
Da biste istražili i manipulirati skup podataka, ga morate najprije se preuzeti iz izvora blob na lokalna datoteka koja se zatim učitati u Pandas DataFrame. Evo nekoliko koraka da biste slijedite ovaj postupak:

1. Preuzimanje podataka iz Azure bloba sa sljedećim primjerom koda Python putem servisa za blob. Zamijeni varijablu u sljedeći kod određene vrijednosti: 

        from azure.storage.blob import BlobService
        import tables
        
        STORAGEACCOUNTNAME= <storage_account_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        LOCALFILENAME= <local_file_name>        
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>

        #download from blob
        t1=time.time()
        blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
        blob_service.get_blob_to_path(CONTAINERNAME,BLOBNAME,LOCALFILENAME)
        t2=time.time()
        print(("It takes %s seconds to download "+blobname) % (t2 - t1))


2. Čitanje podataka u Pandas podataka – okvir iz preuzete datoteke.

        #LOCALFILE is the file path 
        dataframe_blobdata = pd.read_csv(LOCALFILE)

Sada ste spremni za istraživanje podataka i generiranje značajki na ovaj skup podataka.

##<a name="blob-dataexploration"></a>Primjeri Istraživanje podataka pomoću Pandas

Evo nekoliko primjera načina za istraživanje podataka pomoću Pandas:

1. Provjera **broja redaka i stupaca** 

        print 'the size of the data is: %d rows and  %d columns' % dataframe_blobdata.shape

2. **Provjeri** prve ili zadnje nekoliko **redaka** u sljedeće skupu podataka:

        dataframe_blobdata.head(10)
        
        dataframe_blobdata.tail(10)

3. Provjera **Vrsta podataka** svakog stupca uvezeni kao što su korištenje sljedeći kod uzorka
    
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype

4. Na sljedeći način provjerite **Osnovni stat** za stupce u skupu podataka
 
        dataframe_blobdata.describe()
    
5. Pogledajte broj stavki za svaku vrijednost stupca na sljedeći način

        dataframe_blobdata['<column_name>'].value_counts()

6. **Vrijednosti koje nedostaju Count** i stvarni broj stavki u svakom stupcu pomoću sljedeće ogledne koda

        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
     
7.  Ako imate određenog stupca koji **nema vrijednosti** u podacima, možete ih je ispustite na sljedeći način:

        dataframe_blobdata_noNA = dataframe_blobdata.dropna()
        dataframe_blobdata_noNA.shape

    Drugi način za zamjenu vrijednosti koje nedostaju je s funkcijom način:
    
        dataframe_blobdata_mode = dataframe_blobdata.fillna({'<column_name>':dataframe_blobdata['<column_name>'].mode()[0]})        

8. Stvaranje **histograma** crtanja pomoću varijabli broj intervala za crtanje raspodjele tjednog prikaza kalendara 
    
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
        
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
    
9. Pogledajte **korelacija** između varijabli pomoću programa scatterplot ili pomoću funkcije ugrađene korelacije

        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
        
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()
