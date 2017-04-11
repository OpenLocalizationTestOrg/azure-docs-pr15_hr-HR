<properties 
    pageTitle="Premještanje podataka i iz blobova platforme Azure pomoću programa Explorer prostora za pohranu Azure | Microsoft Azure" 
    description="Premještanje podataka i iz blobova platforme Azure pomoću programa Explorer prostor za pohranu za Azure" 
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
    ms.date="08/31/2016"
    ms.author="bradsev" />

# <a name="move-data-to-and-from-azure-blob-storage-using-azure-storage-explorer"></a>Premještanje podataka i iz blobova platforme Azure pomoću programa Explorer Azure prostora za pohranu

Azure prostora za pohranu Explorer je alat za besplatne od Microsofta koji vam omogućuje rad s podacima Azure prostora za pohranu u sustavu Windows, Mac OS ni Linux. U ovoj se temi opisuje kako ga koristiti za prijenos i preuzimanje podataka iz spremišta blobova platforme Azure. Alat za mogu se preuzeti sa [Microsoft Azure prostora za pohranu Explorer](http://storageexplorer.com/).

Smjernice o tehnologije koje služe za premještanje podataka da biste i/ili iz spremišta blobova platforme Azure povezani ovdje:
 
[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]   

 
> [AZURE.NOTE] Ako koristite VM koji je postavljen s skripte koje ste dobili od [podataka znanstvenog virtualnog računala u Azure](machine-learning-data-science-virtual-machines.md), zatim Azure prostora za pohranu Explorer već instaliran na na VM.
 
> [AZURE.NOTE] Uvod u spremište blobova platforme Azure priručniku [Osnove blobova platforme Azure](../storage/storage-dotnet-how-to-use-blobs.md) i [Servis blobova platforme Azure](https://msdn.microsoft.com/library/azure/dd179376.aspx).   

## <a name="prerequisites"></a>Preduvjeti

Ovaj dokument podrazumijeva da imate pretplatu na Azure, s računom za pohranu i odgovarajućeg ključa za pohranu za taj račun. Prije prijenosa i preuzimanja podataka, morate znati Azure prostora za pohranu imenom i računom ključ za račun. 

- Da biste postavili Azure pretplatu, pročitajte članak [besplatnu probnu verziju jedan mjesec](https://azure.microsoft.com/pricing/free-trial/).
- Upute za stvaranje računa za pohranu i dohvaćanje račun i podaci o ključu, pročitajte članak [o Azure pohranu računi](../storage/storage-create-storage-account.md). Zabilježite pristupni ključ za račun za pohranu potrebno ovaj ključ za povezivanje s računom pomoću alata za Azure pohranu Explorer.
- Alat za Azure pohranu Explorer mogu se preuzeti sa [Microsoft Azure pohranu Explorer](http://storageexplorer.com/). Prihvatite zadane postavke tijekom instalacije.


<a id="explorer"></a>
## <a name="use-azure-storage-explorer"></a>Pomoću programa Explorer Azure prostora za pohranu 

Sljedeći koraci dokumenta upute za prijenos i preuzimanje podataka pomoću programa Explorer Azure prostora za pohranu. 

1.  Pokrenite Microsoft Azure prostora za pohranu Explorer.
2.  Da biste otvorili čarobnjak za **prijavite se na račun servisa...** , odaberite ikonu **Postavke Azure računa** , a zatim **Dodaj račun** , a zatim unesite vjerodajnice. ![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/add-an-azure-store-account.png)
3.  Da biste otvorili čarobnjak za **Povezivanje sa spremištem Azure** , odaberite ikonu za **Povezivanje sa spremištem Azure** . ![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/connect-to-azure-storage-1.png)
4. Unesite ključ programa access s računa za Azure prostora za pohranu na čarobnjaka za **Povezivanje sa spremištem Azure** a zatim **Dalje**. ![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/connect-to-azure-storage-2.png)
5. Unesite naziv računa za pohranu u okviru **naziv računa** , a zatim odaberite **Dalje**. ![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/attach-external-storage.png)
6. Račun za pohranu dodali sada mora biti naveden. Da biste stvorili spremniku blob na računu za pohranu, desnom tipkom miša kliknite čvor **Blob spremnika** u taj račun, odaberite **Stvaranje spremnika Blob**i unesite naziv.
7. Da biste podatke u spremniku, odaberite ciljni spremnik i kliknite gumb **Prenesi** .![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/storage-accounts.png)
8. Kliknite na **…** desno od okvira **datoteka** , odaberite jednu ili više datoteka da biste prenijeli iz datotečnog sustava, a zatim kliknite **Prenesi** da biste započeli s prijenosom datoteka.![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/upload-files-to-blob.png)
7. Da biste preuzeli podatke, a zatim odaberete blob-om u odgovarajuće spremnik da biste preuzeli i kliknite **Preuzmi**. ![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/download-files-from-blob.png)


