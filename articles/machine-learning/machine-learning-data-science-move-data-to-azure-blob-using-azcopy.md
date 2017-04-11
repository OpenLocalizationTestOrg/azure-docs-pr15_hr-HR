<properties
    pageTitle="Premještanje podataka i iz blobova platforme Azure pomoću AzCopy | Microsoft Azure"
    description="Premještanje podataka i iz blobova platforme Azure pomoću AzCopy"
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

# <a name="move-data-to-and-from-azure-blob-storage-using-azcopy"></a>Premještanje podataka i iz blobova platforme Azure pomoću AzCopy

AzCopy je naredbenog retka utility namijenjen prijenos, preuzimanje i kopiranje podataka i s blob, datoteke i spremište tablica sustava Microsoft Azure.

Upute za instalaciju AzCopy i dodatne informacije o korištenju s platforme Azure, potražite u članku [Početak rada s AzCopy naredbenog retka uslužni](../storage/storage-use-azcopy.md).

Smjernice o tehnologije koje služe za premještanje podataka da biste i/ili iz spremišta blobova platforme Azure povezani ovdje:

[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]


> [AZURE.NOTE] Ako koristite VM koji je postavljen s skripte koje ste dobili od [podataka znanstvenog virtualnog računala u Azure](machine-learning-data-science-virtual-machines.md), zatim AzCopy je već instalirana na na VM.

> [AZURE.NOTE] Uvod u spremište blobova platforme Azure priručniku osnove [blobova platforme Azure](../storage/storage-dotnet-how-to-use-blobs.md) i usluzi [blobova platforme Azure](https://msdn.microsoft.com/library/azure/dd179376.aspx).


## <a name="prerequisites"></a>Preduvjeti

Ovaj dokument podrazumijeva da imate pretplatu na Azure, s računom za pohranu i odgovarajućeg ključa za pohranu za taj račun. Prije prijenosa i preuzimanja podataka, morate znati Azure prostora za pohranu imenom i računom ključ za račun.

- Da biste postavili Azure pretplatu, pročitajte članak [besplatnu probnu verziju jedan mjesec](https://azure.microsoft.com/pricing/free-trial/).

- Upute za stvaranje računa za pohranu i dohvaćanje račun i podaci o ključu, pročitajte članak [o Azure pohranu računi](../storage/storage-create-storage-account.md).


## <a name="run-azcopy-commands"></a>Pokretanje naredbe AzCopy

Da biste pokrenuli AzCopy naredbe, otvorite prozor naredbenog i idite do AzCopy instalaciju na vašem računalu, gdje se nalazi izvršna AzCopy.exe. 

Osnovni Sintaksa naredbe AzCopy je:

    AzCopy /Source:<source> /Dest:<destination> [Options]

>[AZURE.NOTE] Možete dodati mjesto za instalaciju AzCopy putu sustava i pokrenite naredbe iz bilo kojeg imenika. Prema zadanim postavkama AzCopy instaliran *% ProgramFiles(x86) %\Microsoft SDKs\Azure\AzCopy* ili *%ProgramFiles%\Microsoft SDKs\Azure\AzCopy*.

## <a name="upload-files-to-an-azure-blob"></a>Prijenos datoteka na blobova platforme Azure

Da biste prenijeli datoteke, koristite sljedeću naredbu:

    # Upload from local file system
    AzCopy /Source:<your_local_directory> /Dest: https://<your_account_name>.blob.core.windows.net/<your_container_name> /DestKey:<your_account_key> /S


## <a name="download-files-from-an-azure-blob"></a>Preuzimati datoteke iz blobova platforme Azure

Da biste preuzeli datoteke iz blobova platforme Azure, koristite sljedeću naredbu:

    # Downloading blobs to local file system
    AzCopy /Source:https://<your_account_name>.blob.core.windows.net/<your_container_name>/<your_sub_directory_at_blob>  /Dest:<your_local_directory> /SourceKey:<your_account_key> /Pattern:<file_pattern> /S


## <a name="transfer-blobs-between-azure-containers"></a>Prijenos blob-ova između Azure spremnika

Da biste prenijeli blob-ova između Azure spremnika, koristite sljedeću naredbu:

    # Transferring blobs between Azure containers
    AzCopy /Source:https://<your_account_name1>.blob.core.windows.net/<your_container_name1>/<your_sub_directory_at_blob1> /Dest:https://<your_account_name2>.blob.core.windows.net/<your_container_name2>/<your_sub_directory_at_blob2> /SourceKey:<your_account_key1> /DestKey:<your_account_key2> /Pattern:<file_pattern> /S

    <your_account_name>: your storage account name
    <your_account_key>: your storage account key
    <your_container_name>: your container name
    <your_sub_directory_at_blob>: the sub directory in the container
    <your_local_directory>: directory of local file system where files to be uploaded from or the directory of local file system files to be downloaded to
    <file_pattern>: pattern of file names to be transferred. The standard wildcards are supported


## <a name="tips-for-using-azcopy"></a>Savjeti za korištenje AzCopy

> [AZURE.TIP]   
> 1. Kada **Prijenos** datoteke, */S* prenosi rekurzivno datoteke. Bez taj parametar datoteka u direktorijima ne prenose.  
> 2. **Preuzimanje** datoteke */S* pretražuje rekurzivno spremnik dok sve datoteke u navedenom direktoriju i poddirektorije ili sve datoteke koje odgovaraju navedenim uzorkom u direktoriju dani i poddirektorije, preuzimaju se.  
> 3.  Ne možete navesti **određene blob datoteka** da biste preuzeli pomoću */Source* parametra. Da biste preuzeli određenom datotekom, navedite naziv datoteke blob da biste preuzeli pomoću */Pattern* parametra. **/S** parametar može se koristiti da bi AzCopy potražite uzorak rekurzivno naziv za datoteku. Bez parametra uzorak AzCopy preuzimaju i sve datoteke u tom direktoriju.
