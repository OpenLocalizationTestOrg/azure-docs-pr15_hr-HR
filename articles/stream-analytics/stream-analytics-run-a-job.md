<properties 
    pageTitle="Kako pokrenuti strujanje poslovi u strujanje analize | Microsoft Azure" 
    description="Kako pokrenuti strujanje posao u Azure strujanje analize | učenje put segmenta."
    keywords="strujanje poslova"
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>

# <a name="how-to-run-a-streaming-job-in-azure-stream-analytics"></a>Kako pokrenuti strujanje posao u Azure strujanje analize

Kada posao unos, upita i izlazni sve nisu navedeni možete pokrenuti posao strujanje analize.

Da biste pokrenuli vaš posao:

1.  Klasični Azure portalu na nadzornoj ploči posao kliknite **pokretanje** pri dnu stranice.

    ![Pokretanje zadatka gumb](./media/stream-analytics-run-a-job/1-stream-analytics-run-a-job.png)  

    Na portalu Azure kliknite **pokretanje** pri vrhu stranice posao.

    ![Azure portala početka posla gumb](./media/stream-analytics-run-a-job/4-stream-analytics-run-a-job.png)  

2.  Navesti vrijednost **Izlaz pokretanje** da biste odredili kada posao započet će proizvodnje izlaz. Zadane postavke za zadatke koji su prethodno nije pokrenut je **Vrijeme početka posla**, što znači da posao odmah započeti obrada podataka. Možete odrediti i **Prilagođeni** put u prošlosti (za troše proteklih podataka) ili u budućnosti (da biste odgodili obrada do buduće vremena). Za slučaj da se kada posao je prethodno pokrenuti i zaustaviti, na **Zadnji put prestao** je mogućnost dostupna za nastavak posla od posljednjeg izlazne i izbjegli gubitak podataka.  

    ![Pokrenite tok posla vremena](./media/stream-analytics-run-a-job/2-stream-analytics-run-a-job.png)  

    ![Azure portala početka strujanje posao vremena](./media/stream-analytics-run-a-job/5-stream-analytics-run-a-job.png)  

3.  Potvrdili odabir. Status zadatka promijenit će se *Početni* i uskoro premjestit će *pokrenuti* kada je pokrenut posao. Možete pratiti napredak postupka **pokretanje** u odjeljku **Središte obavijesti**:

    ![strujanje napretka posla](./media/stream-analytics-run-a-job/3-stream-analytics-run-a-job.png)  

    ![Azure portal strujanje napretka posla](./media/stream-analytics-run-a-job/6-stream-analytics-run-a-job.png)  

## <a name="get-help"></a>Zatražite pomoć
Dodatnu pomoć, pokušajte našem [forumu Azure strujanje Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Daljnji koraci

- [Uvod u Azure strujanje Analytics](stream-analytics-introduction.md)
- [Prvi koraci pri korištenju Azure strujanje Analytics](stream-analytics-get-started.md)
- [Promjena veličine Azure strujanje analize poslova](stream-analytics-scale-jobs.md)
- [Azure strujanje analize upita jezična referenca](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure strujanje analize upravljanje REST API Reference](https://msdn.microsoft.com/library/azure/dn835031.aspx)
