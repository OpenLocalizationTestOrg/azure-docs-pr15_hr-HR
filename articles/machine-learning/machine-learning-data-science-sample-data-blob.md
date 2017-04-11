<properties 
    pageTitle="Ogledne podatke iz Azure bloba pohranu | Microsoft Azure" 
    description="Ogledni podaci u spremište blobova platforme Azure" 
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

#<a name="heading"></a>Ogledni podaci u Azure bloba prostora za pohranu


Ovaj dokument pokriva uzorkovanje podataka koji su pohranjeni u spremište blobova platforme Azure programski preuzimanja i uzorkovanje ga pomoću postupaka pisane Python.

**Zašto uzorak podataka?**
Ako je prevelik dataset plan za analizu, obično je dobro dolje ogledne podatke da biste smanjili veličinu manje no predstavniku i jednostavnije. To olakšava razumijevanje podataka, istraživanje i inženjering značajke. Njegova uloga u postupku analize Cortana je da biste omogućili brzo prototyping funkcija obradu podataka i strojnog učenja modela.

Na **izborniku** ispod veze na temu koja opisuje kako ogledne podatke iz različitih okruženja za pohranu. 

[AZURE.INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

Ovaj zadatak uzorkovanje je korak u [Timu podataka znanstvenog procesa (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).


## <a name="download-and-down-sample-data"></a>Preuzimanje i dolje oglednih podataka
1. Preuzimanje podataka iz spremišta blobova platforme Azure putem servisa za blob iz sljedeći kod Python uzorak: 

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

2. Čitanje podataka u Pandas podataka – okvir iz datoteke preuzete iznad.

        import pandas as pd

        #directly ready from file on disk
        dataframe_blobdata = pd.read_csv(LOCALFILE)

3. Dolje oglednih podataka pomoću na `numpy`, `random.choice` na sljedeći način:

        # A 1 percent sample
        sample_ratio = 0.01 
        sample_size = np.round(dataframe_blobdata.shape[0] * sample_ratio)
        sample_rows = np.random.choice(dataframe_blobdata.index.values, sample_size)
        dataframe_blobdata_sample = dataframe_blobdata.ix[sample_rows]

Sada možete raditi s gornji okvir podataka s uzorka postotak 1 za dodatne informacije i generiranje značajke.

##<a name="heading"></a>Prijenos podataka i pročitajte u Azure strojnog učenja

Sljedeći primjer kod možete koristiti za dolje ogledne podatke i koristiti ga izravno u Azure ML:

1. Zapisivanje podataka okvira lokalne datoteke

        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)

2. Prenesite lokalne datoteke programa blobova platforme Azure pomoću koda za sljedeće ogledne:

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
            print ("Something went wrong with uploading to the blob:"+ BLOBNAME)

3. Čitanje podataka iz blobova platforme Azure pomoću Azure ML [Uvoz podataka](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) kao što je prikazano na slici u nastavku:
 
![čitač blob](./media/machine-learning-data-science-sample-data-blob/reader_blob.png)

 
