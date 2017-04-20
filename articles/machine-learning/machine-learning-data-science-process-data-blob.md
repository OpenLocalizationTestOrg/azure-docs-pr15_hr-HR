<properties 
    pageTitle="Obradu podataka Azure bloba s naprednim analytics | Microsoft Azure" 
    description="Proces podataka u Azure blobova." 
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
    ms.date="09/19/2016"
    ms.author="fashah;garye;bradsev" /> 

#<a name="heading"></a>Proces Azure blob podataka s naprednim analytics

Ovaj dokument pokriva Upoznavanje podataka i generiranje značajke iz podataka spremljenih u Azure blobova. 

## <a name="load-the-data-into-a-pandas-data-frame"></a>Učitavanje podataka u okvir podataka Pandas
Za istraživanje i rukovanje dataset ga mora biti preuzete iz izvora blob lokalne datoteke koju zatim možete učitati u okvir podataka Pandas. Ovdje su korake slijedite ovaj postupak:

1. Preuzimanje podataka iz Azure bloba s sljedeći kod Python uzorak korištenjem usluge bloba. Zamijeni varijable u kodu ispod određene vrijednosti: 

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


2. Čitanje podataka u Pandas podataka-okvir iz preuzete datoteke.

        #LOCALFILE is the file path 
        dataframe_blobdata = pd.read_csv(LOCALFILE)

Sada ste spremni za istraživanje podataka i generiranje značajki na ovoj dataset.


##<a name="blob-dataexploration"></a>Za pretraživanje podataka

Evo nekoliko primjera načina za istraživanje podataka pomoću Pandas:

1. Provjeri broj redaka i stupaca 

        print 'the size of the data is: %d rows and  %d columns' % dataframe_blobdata.shape

2. Provjeri prvi ili posljednji nekoliko redaka u dataset kao ispod:

        dataframe_blobdata.head(10)
        
        dataframe_blobdata.tail(10)

3. Provjerite vrstu podataka svakog stupca uvezen kao pomoću sljedeće uzorak koda
    
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype

4. Sljedeći način provjerite osnovni stats stupaca u skupu podataka
 
        dataframe_blobdata.describe()
    
5. Pogledajte sljedeći broj unosa za svaku vrijednost stupca

        dataframe_blobdata['<column_name>'].value_counts()

6. Brojanje nedostaje vrijednostima i stvarni broj stavke u svakom stupcu koristite sljedeći uzorak koda

        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
     
7.  Ako imate nedostaje vrijednosti za određeni stupac podataka, možete ih ispustite kako slijedi:

        dataframe_blobdata_noNA = dataframe_blobdata.dropna()
        dataframe_blobdata_noNA.shape

    Drugi način za zamjenu nedostaje vrijednosti je s funkcijom načinu:
    
        dataframe_blobdata_mode = dataframe_blobdata.fillna({'<column_name>':dataframe_blobdata['<column_name>'].mode()[0]})        

8. Stvaranje histogram crtanja pomoću varijable broja regalna skladišta za iscrtavanje raspodjele varijabla 
    
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
        
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
    
9. Pogledajte korelacija između varijabli pomoću na scatterplot ili pomoću funkcije ugrađene korelacije

        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
        
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()
    
    
##<a name="blob-featuregen"></a>Generiranje značajka
    
Možemo može generirati značajke pomoću Python kako slijedi:

###<a name="blob-countfeature"></a>Vrijednost pokazatelja temelji generiranje značajka

Categorical značajke se mogu kreirati kako slijedi:

1. Provjeri raspodjele categorical stupca:
    
        dataframe_blobdata['<categorical_column>'].value_counts()

2. Generiranje pokazatelj vrijednosti za svaki stupac vrijednosti

        #generate the indicator column
        dataframe_blobdata_identity = pd.get_dummies(dataframe_blobdata['<categorical_column>'], prefix='<categorical_column>_identity')

3. Pridruži se pokazatelj stupac s izvorne podatke okvira 
 
            #Join the dummy variables back to the original data frame
            dataframe_blobdata_with_identity = dataframe_blobdata.join(dataframe_blobdata_identity)

4. Uklanjanje izvorne varijablu sam:

        #Remove the original column rate_code in df1_with_dummy
        dataframe_blobdata_with_identity.drop('<categorical_column>', axis=1, inplace=True)
    
###<a name="blob-binningfeature"></a>Generiranje binning značajka

Za generiranje binned značajke smo nastavite kako slijedi:

1. Dodavanje redoslijeda stupaca regalnog skladišta numerički stupac
 
        bins = [0, 1, 2, 4, 10, 40]
        dataframe_blobdata_bin_id = pd.cut(dataframe_blobdata['<numeric_column>'], bins)
        
2. Pretvori binning slijed boolean varijable

        dataframe_blobdata_bin_bool = pd.get_dummies(dataframe_blobdata_bin_id, prefix='<numeric_column>')
    
3. Konačno, pridružite lažni varijable za izvorne podatke okvira

        dataframe_blobdata_with_bin_bool = dataframe_blobdata.join(dataframe_blobdata_bin_bool) 


##<a name="sql-featuregen"></a>Zapisivanje podataka natrag Azure blob i troše Azure stroj učenje

Nakon što ste istražili podataka i stvoriti potrebne značajke možete prenijeti podatke (uzorkovanja ili featurized) da biste je Azure bloba i potroši u učenje Azure stroj pomoću sljedećih koraka: Imajte na umu da dodatne značajke mogu se kreirati na Azure stroj učenje Studio kao i. 
1. Pisanje podataka okvira lokalne datoteke

        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)

2. Prenesi podatke na Azure blob kako slijedi:

        from azure.storage.blob import BlobService
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

3. Sada podataka možete pročitati iz blob korištenjem Azure stroj učenje [Uvoz podataka] [ import-data] modul kao što je prikazano na zaslonu dolje:
 
![čitač bloba][1]

[1]: ./media/machine-learning-data-science-process-data-blob/reader_blob.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
 
