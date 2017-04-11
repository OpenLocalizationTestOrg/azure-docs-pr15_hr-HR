<properties
    pageTitle="Stvaranje značajke za podatke iz spremišta blobova platforme Azure pomoću Panda | Microsoft Azure"
    description="Kako stvoriti značajke za podatke koji su pohranjenu u spremniku blobova platforme Azure pomoću značajke pakiranja Panda Python."
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
    ms.author="bradsev;garye" />

#<a name="create-features-for-azure-blob-storage-data-using-panda"></a>Stvaranje značajke za podatke iz spremišta blobova platforme Azure pomoću Panda

Ovaj dokument prikazuje kako stvoriti značajke za podatke koji su pohranjenu u spremniku blobova platforme Azure pomoću značajke pakiranja [Pandas](http://pandas.pydata.org/) Python. Nakon Strukturiranje upute da biste učitali podatke u okvir Panda podataka, prikazuje kako da biste generirali categorical značajke Python skripte pomoću pokazatelja vrijednosti i binning značajke.

[AZURE.INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]U ovom **izborniku** navode veze na temu koja opisuje kako stvoriti značajke za podatke u različitim okruženjima. Ovaj zadatak je korak u [Timu podataka znanstvenog procesa (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).


## <a name="prerequisites"></a>Preduvjeti

U ovom se članku pretpostavlja da ste stvorili račun spremište blobova platforme Azure i ste pohranili vaše podatke. Ako vam je potrebna upute za postavljanje računa potražite u članku [Stvaranje računa za pohranu za Azure](../storage/storage-create-storage-account.md#create-a-storage-account)


## <a name="load-the-data-into-a-pandas-data-frame"></a>Učitavanje podatke u okvir Pandas podataka
Da biste mogli Istraživanje i manipulirati skup podataka, ga mora se preuzeti iz izvora blob da lokalna datoteka koja se zatim učitati u okviru Pandas podataka. Evo nekoliko koraka da biste slijedite ovaj postupak:

1. Preuzimanje podataka iz Azure bloba s sljedeći kod Python uzorka putem servisa za blob. Zamijeni varijabla kod ispod određene vrijednosti:

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

##<a name="blob-featuregen"></a>Značajka generacije

Sljedeća dva odjeljka pokazuju kako da biste generirali categorical značajke pokazatelj vrijednostima i binning značajke za korištenje Python skripti.

###<a name="blob-countfeature"></a>Pokazatelj vrijednost utemeljena značajka generacije

Categorical značajke moguće je stvoriti na sljedeći način:

1. Provjera raspodjele categorical stupca:

        dataframe_blobdata['<categorical_column>'].value_counts()

2. Generiranje pokazatelj vrijednosti za svaku od vrijednosti u stupcu

        #generate the indicator column
        dataframe_blobdata_identity = pd.get_dummies(dataframe_blobdata['<categorical_column>'], prefix='<categorical_column>_identity')

3. Uključivanje pokazatelj stupac s izvornog okvira podataka

            #Join the dummy variables back to the original data frame
            dataframe_blobdata_with_identity = dataframe_blobdata.join(dataframe_blobdata_identity)

4. Uklanjanje izvorne varijabla sam:

        #Remove the original column rate_code in df1_with_dummy
        dataframe_blobdata_with_identity.drop('<categorical_column>', axis=1, inplace=True)

###<a name="blob-binningfeature"></a>Binning značajka generacije

Za generiranje binned značajke, ne možemo nastaviti na sljedeći način:

1. Dodavanje redoslijed stupaca na intervala numerički stupac

        bins = [0, 1, 2, 4, 10, 40]
        dataframe_blobdata_bin_id = pd.cut(dataframe_blobdata['<numeric_column>'], bins)

2. Pretvaranje binning niz Booleova varijable

        dataframe_blobdata_bin_bool = pd.get_dummies(dataframe_blobdata_bin_id, prefix='<numeric_column>')

3. Na kraju, vratite se u okvir izvorne podatke uključivanje sustava varijable

        dataframe_blobdata_with_bin_bool = dataframe_blobdata.join(dataframe_blobdata_bin_bool)

##<a name="sql-featuregen"></a>Ponovno pisanje podataka da biste blobova platforme Azure i koje se koriste u Azure strojnog učenja

Nakon što ste istražili podataka i stvorili potrebne značajke, možete prenijeti podatke (uzorkovanja ili featurized) da biste je Azure bloba i zauzeti u Azure strojnog učenja na sljedeći način: Imajte na umu da se značajke mogu stvarati u na Azure strojnog učenja Studio kao i.
1. Zapisivanje podataka okvira lokalne datoteke

        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)

2. Prijenos podataka na blobova platforme Azure na sljedeći način:

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

3. Sada podatke možete pročitati s blob-om pomoću modula Azure strojnog učenja [Uvoz podataka](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) kao što je prikazano u nastavku zaslona:

![čitač blob](./media/machine-learning-data-science-process-data-blob/reader_blob.png)
