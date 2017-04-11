<properties
    pageTitle="Premještanje podataka i iz blobova platforme Azure pomoću Python | Microsoft Azure"
    description="Premještanje podataka i iz blobova platforme Azure pomoću Python"
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
    ms.date="09/14/2016"
    ms.author="bradsev" />

# <a name="move-data-to-and-from-azure-blob-storage-using-python"></a>Premještanje podataka i iz blobova platforme Azure pomoću Python

U ovoj se temi opisuje kako popis, prijenos i preuzimanje blob-ova pomoću Python API-JA. S Python API-jem navedenih u Azure SDK, možete učiniti sljedeće:

- Stvaranje spremnika
- Prijenos blob u spremniku
- Preuzimanje blob-ova
- Popis blob-ova u spremniku
- Brisanje blob

Dodatne informacije o korištenju Python API potražite u članku [kako pomoću servisa za pohranu servisa Blob Python](../storage/storage-python-how-to-use-blob-storage.md).

Smjernice o tehnologije koje služe za premještanje podataka da biste i/ili iz spremišta blobova platforme Azure povezani ovdje:

[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]


> [AZURE.NOTE] Ako koristite VM koji je postavljen s skripte koje ste dobili od [podataka znanstvenog virtualnog računala u Azure](machine-learning-data-science-virtual-machines.md), zatim AzCopy je već instalirana na na VM.

> [AZURE.NOTE] Uvod u spremište blobova platforme Azure priručniku osnove [blobova platforme Azure](../storage/storage-dotnet-how-to-use-blobs.md) i usluzi [blobova platforme Azure](https://msdn.microsoft.com/library/azure/dd179376.aspx).


## <a name="prerequisites"></a>Preduvjeti

Ovaj dokument podrazumijeva da imate pretplatu na Azure, s računom za pohranu i odgovarajućeg ključa za pohranu za taj račun. Prije prijenosa i preuzimanja podataka, morate znati Azure prostora za pohranu imenom i računom ključ za račun.

- Da biste postavili Azure pretplatu, pročitajte članak [besplatnu probnu verziju jedan mjesec](https://azure.microsoft.com/pricing/free-trial/).

- Upute za stvaranje računa za pohranu i dohvaćanje račun i podaci o ključu, pročitajte članak [o Azure pohranu računi](../storage/storage-create-storage-account.md).


## <a name="upload-data-to-blob"></a>Prijenos podataka blobova platforme

Dodajte sljedeće isječak blizu vrha sve Python koda u kojoj želite li programatski pristup Azure prostora za pohranu:

    from azure.storage.blob import BlobService

Objekt **BlobService** omogućuje rad s spremnika i blob-ova. Sljedeći kod stvara objekt BlobService pomoću računa imenom i računom ključa za pohranu. Zamijenite naziv računa i ključ računa realni računa i ključa.

    blob_service = BlobService(account_name="<your_account_name>", account_key="<your_account_key>")

Prijenos podataka blob pomoću sljedećih načina:

1. Stavite\_blok\_blob\_iz\_put (prenosi sadržaj datoteke u navedenom parametru path)
2. Stavite\_block_blob\_iz\_datoteke (prenosi sadržaj iz već otvorena datoteka/strujanje)
3. Stavite\_blok\_blob\_iz\_bajtova (prijenosi pojavila polja bajtova)
4. Stavite\_blok\_blob\_iz\_tekst (prenosi vrijednost određeni tekst koristeći navedeno šifriranje)

Sljedeći primjer kod prenosi lokalna datoteka spremnik:

    blob_service.put_block_blob_from_path("<your_container_name>", "<your_blob_name>", "<your_local_file_name>")

Sljedeći primjer kod prenosi sve datoteke (bez direktorija) u lokalnom direktoriju sa spremištem blobova:

    from azure.storage.blob import BlobService
    from os import listdir
    from os.path import isfile, join

    # Set parameters here
    ACCOUNT_NAME = "<your_account_name>"
    ACCOUNT_KEY = "<your_account_key>"
    CONTAINER_NAME = "<your_container_name>"
    LOCAL_DIRECT = "<your_local_directory>"     

    blob_service = BlobService(account_name=ACCOUNT_NAME, account_key=ACCOUNT_KEY)
    # find all files in the LOCAL_DIRECT (excluding directory)
    local_file_list = [f for f in listdir(LOCAL_DIRECT) if isfile(join(LOCAL_DIRECT, f))]

    file_num = len(local_file_list)
    for i in range(file_num):
        local_file = join(LOCAL_DIRECT, local_file_list[i])
        blob_name = local_file_list[i]
        try:
            blob_service.put_block_blob_from_path(CONTAINER_NAME, blob_name, local_file)
        except:
            print "something wrong happened when uploading the data %s"%blob_name


## <a name="download-data-from-blob"></a>Preuzimanje podataka iz blobova platforme

Preuzimanje podataka iz blob pomoću sljedećih načina:
1. Početak\_blob\_da biste\_put
2. Početak\_blob\_da biste\_datoteka
3. Početak\_blob\_da biste\_bajtova
4. Početak\_blob\_da biste\_teksta

Načini izvršiti potrebne chunking kada veličina podataka premašuje 64 MB.

Sljedeći primjer kod preuzeti sadržaj blob u spremniku lokalna datoteka:

    blob_service.get_blob_to_path("<your_container_name>", "<your_blob_name>", "<your_local_file_name>")

Sljedeći kod uzorka s spremnik preuzima sve blob-ova. Koristi popis\_blob polja da biste dobili popis dostupnih blob-ova u spremniku i preuzeti ih lokalnog imenika.

    from azure.storage.blob import BlobService
    from os.path import join

    # Set parameters here
    ACCOUNT_NAME = "<your_account_name>"
    ACCOUNT_KEY = "<your_account_key>"
    CONTAINER_NAME = "<your_container_name>"
    LOCAL_DIRECT = "<your_local_directory>"     

    blob_service = BlobService(account_name=ACCOUNT_NAME, account_key=ACCOUNT_KEY)

    # List all blobs and download them one by one
    blobs = blob_service.list_blobs(CONTAINER_NAME)
    for blob in blobs:
        local_file = join(LOCAL_DIRECT, blob.name)
        try:
            blob_service.get_blob_to_path(CONTAINER_NAME, blob.name, local_file)
        except:
            print "something wrong happened when downloading the data %s"%blob.name
