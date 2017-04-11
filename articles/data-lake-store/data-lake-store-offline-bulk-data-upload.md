<properties
   pageTitle="Prijenos velikih količina podataka u spremištu Lake podataka izvanmrežno načine | Microsoft Azure"
   description="Kopirajte podatke iz blob polja za pohranu Azure u spremištu Lake podataka pomoću alata za AdlCopy"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/07/2016"
   ms.author="nitinme"/>

# <a name="use-azure-import-export-service-for-offline-copy-of-data-to-data-lake-store"></a>Korištenje servisa Azure uvoz/izvoz za izvanmrežnu kopiju podataka u spremištu Lake podataka

U ovom se članku ćete saznati kako kopirati velikog skupove podataka (> 200GB) u spremištu Azure podataka Lake načine izvanmrežnu kopiju, kao što su [Servisa Azure uvoz/izvoz](../storage/storage-import-export-service.md). Konkretno, datoteku koja se koristi kao primjer u ovom članku je 339,420,860,416 bajtova odnosno oko 319GB na disku. Pogledajmo nazovite ovo 319GB.tsv datoteke.

Azure servis za uvoz/izvoz omogućuje siguran prijenos velikih količina podataka blobova platforme Azure po dostavu tvrdom disku pogona centar za Azure podataka.

## <a name="prerequisites"></a>Preduvjeti

Prije nego počnete u ovom se članku, morate imati sljedeće:

- **Mogući Azure pretplate**. Pogledajte [Početak Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/).

- **Račun za azure prostora za pohranu**.

- **Račun za azure podataka Lake analize (neobavezno)** – potražite u članku [Početak rada s Azure podataka Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md) za upute za stvaranje računa spremišta Lake podataka.


## <a name="preparing-the-data"></a>Priprema podataka

Prije korištenja servisa za uvoz/izvoz, ne možemo morate prekinuti se preneseni **u kopije koje su manje od 200 GB** veličine podatkovne datoteke. To je zato alat za uvoz rad s datotekama veća od 200GB. Da biste u skladu s time, pomoću ovog praktičnog vodiča ćemo podijeliti datoteku u blokova 100GB bajtova svaki. To možete učiniti pomoću [Cygwin](https://cygwin.com/install.html). Cygwin podržava Linux naredbe koje korisnicima omogućuje jednostavno učinite sljedeće. Za naše slučaj koristimo sljedeću naredbu.

    split -b 100m 319GB.tsv

Operacija Podijeli stvara datoteke s nazivima u nastavku.

    319GB.tsv-part-aa

    319GB.tsv-part-ab

    319GB.tsv-part-ac

    319GB.tsv-part-ad

## <a name="get-disks-ready-with-data"></a>Priprema diskova s podacima

Slijedite upute u [Pomoću servisa Azure uvoz/izvoz](../storage/storage-import-export-service.md) (u odjeljku **Priprema pogonima** ) da biste pripremili tvrdog diska. Evo ukupnog toka o tome kako pripremiti pogon.

1. Nabavu tvrdi disk koji zadovoljava preduvjete za Auzre servis za uvoz/izvoz.

2. Prepoznavanje računa za pohranu Azure gdje će se podaci kopirali jednom Promijeni poslane u centru za Azure podataka.

3. Pomoću [Alata za uvoz/izvoz Azure](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409), uslužni program naredbenog retka. Evo oglednih isječak o korištenju alata.

    ````
    WAImportExport PrepImport /sk:<StorageAccountKey> /t: <TargetDriveLetter> /format /encrypt /logdir:e:\myexportimportjob\logdir /j:e:\myexportimportjob\journal1.jrn /id:myexportimportjob /srcdir:F:\demo\ExImContainer /dstdir:importcontainer/vf1/
    ````
    Dodatne isječci uzorka o korištenju **Alata za uvoz/izvoz Azure**potražite u članku [Pomoću servisa Azure uvoz/izvoz](../storage/storage-import-export-service.md) .

4. Iznad naredba stvara datoteku dnevnik na navedeno mjesto. Da biste stvorili posao uvoza iz [Azure klasični Portal](https://manage.windowsazure.com)će koristiti ove datoteke dnevnika.

## <a name="create-an-import-job"></a>Stvaranje posao uvoza

Sad možete stvarati posao uvoza slijedeći upute na [Pomoću servisa Azure uvoz/izvoz](../storage/storage-import-export-service.md) (u odjeljku **Stvaranje posla uvoza** ). Za ovaj zadatak uvoza s druge detalje nuditi dnevnik datoteku stvorili prilikom pripreme diskovnih pogona.

## <a name="physically-ship-the-disks"></a>Fizički isporuka na disk

Sada možete fizički isporuka diskova centar za Azure podataka koju kopirali podatke putem za Azure blob polja za pohranu koje ste naveli prilikom stvaranja posla uvoza. Osim toga, prilikom stvaranja posla, odlučili možete unijeti informacije o praćenju noviju verziju, koju možete sada vratite posla uvoza i ažurirati broja za praćenje.

## <a name="copy-data-from-azure-storage-blobs-to-azure-data-lake-store"></a>Kopirajte podatke iz blob polja za pohranu Azure u spremištu Lake podataka za Azure

Nakon što status posla uvoza prikazuje dovršene, možete provjeriti li podaci dostupni su u Azure blob polja za pohranu koje ste naveli. Da biste premjestili podatke s blob-ova za pohranu Azure Lake spremišta podataka za Azure možete koristiti na različite načine. Sve dostupne mogućnosti za prijenos podataka, potražite u članku [Ingesting podataka u spremištu Lake podataka](data-lake-store-data-scenarios.md#ingest-data-into-data-lake-store).

U ovom ćete odjeljku možemo vam ponuditi definicije JSON koje možete koristiti za stvaranje kanala na tvorničke Azure podataka za kopiranje podataka. Možete koristiti te definicije JSON s [portala za Azure](../data-factory/data-factory-copy-activity-tutorial-using-azure-portal.md) ili [Visual Studio](../data-factory/data-factory-copy-activity-tutorial-using-visual-studio.md) ili [Azure PowerShell](../data-factory/data-factory-copy-activity-tutorial-using-powershell.md).

### <a name="source-linked-service-azure-storage-blob"></a>Izvor povezane usluge (blobova platforme Azure prostora za pohranu)

````
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "description": "",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
    }
}
````

### <a name="target-linked-service-azure-data-lake-store"></a>Ciljani povezane usluge (Azure podataka Lake trgovina)

````
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "description": "",
        "typeProperties": {
            "authorization": "<Click 'Authorize' to allow this data factory and the activities it runs to access this Data Lake Store with your access rights>",
            "dataLakeStoreUri": "https://<adls_account_name>.azuredatalakestore.net/webhdfs/v1",
            "sessionId": "<OAuth session id from the OAuth authorization session. Each session id is unique and may only be used once>"
        }
    }
}
````
### <a name="input-data-set"></a>Unos skup podataka
````
{
    "name": "InputDataSet",
    "properties": {
        "published": false,
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "importcontainer/vf1/"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
````
### <a name="output-data-set"></a>Izlazni skup podataka
````
{
"name": "OutputDataSet",
"properties": {
  "published": false,
  "type": "AzureDataLakeStore",
  "linkedServiceName": "AzureDataLakeStoreLinkedService",
  "typeProperties": {
    "folderPath": "/importeddatafeb8job/"
    },
  "availability": {
    "frequency": "Hour",
    "interval": 1
    }
  }
}
````
### <a name="pipeline-copy-activity"></a>Kanal (Kopiraj aktivnosti)
````
{
    "name": "CopyImportedData",
    "properties": {
        "description": "Pipeline with copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "AzureDataLakeStoreSink",
                        "copyBehavior": "PreserveHierarchy",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "InputDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "OutputDataSet"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "AzureBlobtoDataLake",
                "description": "Copy Activity"
            }
        ],
        "start": "2016-02-08T22:00:00Z",
        "end": "2016-02-08T23:00:00Z",
        "isPaused": false,
        "pipelineMode": "Scheduled"
    }
}
````
Dodatne informacije o korištenju tvorničke Azure podataka za premještanje podataka iz blobova platforme Azure prostora za pohranu i spremište Lake za Azure podataka potražite u članku [Premještanje podataka iz blobova platforme Azure prostora za pohranu trgovina Azure podataka Lake pomoću tvorničke Azure podataka](../data-factory/data-factory-azure-datalake-connector.md#sample-copy-data-from-azure-blob-to-azure-data-lake-store).

## <a name="reconstruct-the-data-files-in-azure-data-lake-store"></a>Ponovno sastavit podatkovne datoteke u spremištu Lake podataka za Azure

Ne možemo pokrenuti pomoću datoteke koju je 319GB veličine i prekinuta je prema dolje u datoteke manje veličine tako da ga može prenijeti putem servisa za uvoz/izvoz Azure. Sad kad se podaci nalaze u spremištu Lake podataka za Azure, možemo reconstrcut datoteke na izvornu veličinu. Da biste to učinili možete koristiti sljedeće cmldts Azure PowerShell.

````
# Login to our account
Login-AzureRmAccount

# List your subscriptions
Get-AzureRmSubscription

# Switch to the subscription you want to work with
Set-AzureRmContext –SubscriptionId
Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

# Join  the files
Join-AzureRmDataLakeStoreItem -AccountName "<adls_account_name" -Paths "/importeddatafeb8job/319GB.tsv-part-aa","/importeddatafeb8job/319GB.tsv-part-ab", "/importeddatafeb8job/319GB.tsv-part-ac", "/importeddatafeb8job/319GB.tsv-part-ad" -Destination "/importeddatafeb8job/MergedFile.csv”
````

## <a name="next-steps"></a>Daljnji koraci

- [Zaštite podataka u spremištu Lake podataka](data-lake-store-secure-data.md)
- [Korištenje analize Lake Azure podataka s spremišta Lake podataka](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure HDInsight pomoću spremišta Lake podataka](data-lake-store-hdinsight-hadoop-use-portal.md)
