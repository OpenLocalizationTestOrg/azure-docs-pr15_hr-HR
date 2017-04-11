<properties 
    pageTitle="Uzorak podataka u blobova platforme Azure spremnika, SQL Server i vrste Hive tablice | Microsoft Azure" 
    description="Upute za istraživanje podataka pohranjenih u različitim Azure enviromnents." 
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

#<a name="heading"></a>Uzorak podataka u blobova platforme Azure spremnika, SQL Server i vrste Hive tablice

U ovom veze na dokumente na temu koja opisuje kako ogledne podatke koji su pohranjenu u neku od tri različita mjesta Azure:

- **Blobova platforme Azure spremnik podataka** uzorkovanja je tako da ga preuzmete programski i uzorkovanje je s uzorak Python koda.
- **Podatke sustava SQL Server** je uzorkovanja pomoću SQL i Python programskom jeziku. 
- **Vrste Hive podatke u tablici** je uzorkovanja korištenje grozd upita.

Na **izborniku** ispod veze na temu koja opisuje kako ogledne podatke iz svake od tih okruženja Azure prostora za pohranu. 

[AZURE.INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

Ovaj zadatak uzorkovanje je korak u [Timu podataka znanstvenog procesa (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

## <a name="why-sample-data"></a>Zašto ogledne podatke?

Ako je prevelik dataset plan za analizu, obično je dobro dolje ogledne podatke da biste smanjili veličinu manje no predstavniku i jednostavnije. To olakšava razumijevanje podataka, istraživanje i inženjering značajke. Njegova uloga u postupku analize Cortana je da biste omogućili brzo prototyping funkcija obradu podataka i strojnog učenja modela.



