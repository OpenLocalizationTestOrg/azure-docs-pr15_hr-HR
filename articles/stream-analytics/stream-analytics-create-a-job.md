<properties 
    pageTitle="Kako stvoriti posao obrade analize podataka za analizu strujanje | Microsoft Azure" 
    description="Stvaranje zadatka obrade analize podataka za strujanje analize | učenje put segmenta."
    keywords="Obrada analitičkih podataka i podataka"
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

# <a name="how-to-create-a-data-analytics-processing-job-for-stream-analytics"></a>Upute za stvaranje posao obrade analize podataka za strujanje Analytics

Najviše razine resursa u Azure strujanje Analitika je posao Analitika strujanje.  Sastoji se od jedan ili više izvora ulazne podatke, upit na kojem se iznosi ono transformaciju podataka i jedan ili više izlaz ciljnih web-mjesta koji se zapisuju rezultate. Zajedno te omogućiti korisniku analitiku podataka obrade tok scenarijima podataka.

Da biste počeli koristiti strujanje analize, počnite tako da stvorite novi zadatak strujanje analize.  Imajte na umu da tu akciju ima bez naplate posljedice dok se ne pokreće posao.

1.  Prijavite se na na mreži [Azure klasični portal](http://manage.windowsazure.com) ili [Azure portal](https://portal.azure.com/).
2.  Na portalu: **Kliknite Novo**, zatim **Podatkovne usluge** ili **Analize podataka** ovisno o portal a zatim kliknite **Analitika strujanje Azure** ili **Strujanje analize** , a zatim **Brzo stvaranje**.

    ![Čarobnjak za posao obrade analize podataka](./media/stream-analytics-create-a-job/1-stream-analytics-create-a-job.png)  

    ![Stvaranje analize podataka obrade posla](./media/stream-analytics-create-a-job/4-stream-analytics-create-a-job.png)  

3.  Navedite željena konfiguracija za posao strujanje analize.
    - U okvir **Naziv zadatka** unesite naziv za identifikaciju posao strujanje analize. Kada se provjerava **Naziv zadatka** , u okvir Naziv zadatka pojavit će se Zelena kvačica. **Naziv zadatka** može sadržavati samo alfanumeričke znakove i "-" znakova i mora biti između 3 i 63 znakova.
    - Pomoću **područja** u Azure portal ili **mjesto** na portalu za Azure odredite zemljopisnu lokaciju na mjesto na koje želite pokrenuti posla.
    - Ako pomoću portala za Azure, odaberite ili stvorite račun za pohranu da biste koristili kao **Račun regionalne nadzor prostora za pohranu**. Taj račun za pohranu služi za pohranu nadzora podataka za sve zadatke strujanje analize koji se izvodi u tom području.
    - Ako pomoću portala za Azure navesti novu ili postojeću **Grupu resursa** držite povezani resursi za svoju aplikaciju.

4.  Kada nije konfiguriran nove mogućnosti posao strujanje analize, kliknite **Stvaranje zadatka strujanje analize**. To može potrajati nekoliko minuta za posao strujanje analize će biti stvoren. Da biste provjerili status, možete pratiti napredak u središtu obavijesti.

    ![Podaci analize obrada posla notfications koncentratora](./media/stream-analytics-create-a-job/2-stream-analytics-create-a-job.png)  

    ![Azure portala podataka analize obrade posao Stvaranje zadatka](./media/stream-analytics-create-a-job/5-stream-analytics-create-a-job.png)  

5.  Prikazat će se novi zadatak sa statusom **stvorena**. Obratite pozornost na gumb **Start** je onemogućen. Prije no što počnete posla već morate konfigurirati unos posla, upita i izlaz.

    ![Obrada analitičkih podataka i podataka posla stanja](./media/stream-analytics-create-a-job/3-stream-analytics-create-a-job.png)  

    ![Azure portala podataka analize obradu stanja zadatka posla](./media/stream-analytics-create-a-job/6-stream-analytics-create-a-job.png)  

## <a name="get-help"></a>Zatražite pomoć
Dodatnu pomoć, pokušajte našem [forumu Azure strujanje Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Daljnji koraci

- [Uvod u Azure strujanje Analytics](stream-analytics-introduction.md)
- [Prvi koraci pri korištenju Azure strujanje Analytics](stream-analytics-get-started.md)
- [Promjena veličine Azure strujanje analize poslova](stream-analytics-scale-jobs.md)
- [Azure strujanje analize upita jezična referenca](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure strujanje analize upravljanje REST API Reference](https://msdn.microsoft.com/library/azure/dn835031.aspx)
