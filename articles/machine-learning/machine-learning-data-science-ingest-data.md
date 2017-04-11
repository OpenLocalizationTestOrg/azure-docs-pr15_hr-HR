<properties 
    pageTitle="Podatke učitali u okruženju za pohranu za analizu | Microsoft Azure" 
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
    ms.date="09/19/2016" 
    ms.author="bradsev" />

# <a name="load-data-into-storage-environments-for-analytics"></a>Podatke učitali u okruženju za pohranu za analizu

Proces znanstvenog timu podataka potreban je podataka biti ingested ili učitava u raznih okruženja prostora za pohranu za različite obrade ili analizirati najprikladnije način u svakoj fazi postupka. Odredišta podataka obično koristi za obradu obuhvaćaju blobova platforme Azure, baze podataka SQL Azure SQL Server na Azure VM HDInsight (Hadoop), a Azure strojnog učenja. 

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

U ovom **izborniku** navode veze na temu koja opisuje kako ingest podataka u te ciljne okruženja gdje podaci se pohranjuju, a obrađuju.

Tehnički i poslovne potrebe, kao i početni položaj oblikovanje, a veličina podataka će odrediti cilj okruženja u koju podataka mora biti ingested da biste postigli ciljeve analize. Nije neuobičajeno scenariju obavezne podataka će se premjestiti između nekoliko okruženja da biste postigli raznih zadatke koji su potrebni za sastavljanje predvidljivu modela. Ovaj niz zadataka možete uvrstiti, na primjer, Istraživanje podataka, stara obradi dokumenata, čišćenje, dolje uzorkovanje i obuka modela.
