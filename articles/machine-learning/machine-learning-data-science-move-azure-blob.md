<properties
    pageTitle="Premještanje podataka i iz blobova platforme Azure | Microsoft Azure"
    description="Premještanje podataka i iz blobova platforme Azure"
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
    ms.author="bradsev;sachouks" />

# <a name="move-data-to-and-from-azure-blob-storage"></a>Premještanje podataka i iz blobova platforme Azure

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

Smjernice o tehnologije koje služe za premještanje podataka da biste i/ili iz spremišta blobova platforme Azure povezani ovdje:

[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]
 
Ovisno o scenariju način je najbolja za vas. U članku [scenariji za napredne analize u Azure strojnog učenja](machine-learning-data-science-plan-sample-scenarios.md) pomaže vam određivanje resursa koji vam je potrebno za brojne tijekove rada znanstvenog podataka koristi u postupku napredne analize.

> [AZURE.NOTE] Uvod u spremište blobova platforme Azure priručniku osnove [blobova platforme Azure](../storage/storage-dotnet-how-to-use-blobs.md) i usluzi [blobova platforme Azure](https://msdn.microsoft.com/library/azure/dd179376.aspx).

Umjesto toga, možete koristiti [Tvorničke Azure podataka](https://azure.microsoft.com/services/data-factory/) da biste: 

- Stvaranje i planiranje kanal koji se preuzima podatke iz spremišta blobova platforme Azure 
- Prenesite objavljene Azure strojnog učenja web-servisa, 
- primanje rezultata predvidljivu analize i 
- Prenesite rezultate prostora za pohranu. 

Dodatne informacije potražite u članku [Stvaranje predvidljivu kanali pomoću tvorničke Azure podataka i Azure strojnog učenja](../data-factory/data-factory-azure-ml-batch-execution-activity.md).

## <a name="prerequisites"></a>Preduvjeti

Ovaj dokument podrazumijeva da imate pretplatu na Azure, s računom za pohranu i odgovarajućeg ključa za pohranu za taj račun. Prije prijenosa i preuzimanja podataka, morate znati Azure prostora za pohranu imenom i računom ključ za račun.

- Da biste postavili Azure pretplatu, pročitajte članak [besplatnu probnu verziju jedan mjesec](https://azure.microsoft.com/pricing/free-trial/).
- Upute za stvaranje računa za pohranu i dohvaćanje račun i podaci o ključu, pročitajte članak [o Azure pohranu računi](../storage/storage-create-storage-account.md).
