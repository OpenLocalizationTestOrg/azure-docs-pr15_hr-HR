<properties 
    pageTitle="Kako napisati upita u strujanje analize | Microsoft Azure" 
    description="Pisanje upita u strujanje analize i podataka iz upita | učenje put segmenta."
    keywords="upute za pisanje upita, upita za podatke, pisanje upita, pisanje upita"
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

# <a name="how-to-write-queries-in-stream-analytics"></a>Upute za pisanje upita u strujanje Analytics

Zapisivanje upita za strujanje obrade logike u Azure strujanje Analitika implementirana kao "položaj upita" koji je definiran prije posao pokreće i izvršeno podataka kao što je dostigne posao. Transformaciju podataka izražen je na jeziku nalik SQL upit koji je uglavnom podskup T SQL s nekih dodanu jezik kao što je [Prikaz u prozorima](https://msdn.microsoft.com/library/azure/dn835019.aspx) koristi da biste izrazili vremenski semantiku.

## <a name="writing-queries"></a>Zapisivanje upita: ##

1. U vaš posao analize strujanje na portalu za upravljanje Azure kliknite **upit**.

    ![Upit s izdvajanjem](./media/stream-analytics-write-queries/1-stream-analytics-write-queries.png)  

    Na portalu za Azure kliknite **upit**.

    ![Odaberite Pretpregled upita](./media/stream-analytics-write-queries/query-preview-portal.png)  

2.  Novi zadaci imati predloška upita za jednostavniji početak rada. Predložak upita izvodi "prolazni" upit koji projekata sva polja iz ulaznog događaje u izlaz.  

    - Ako najmanje jedan ulazni i izlazni ste definirali za svoj posao, možete zamijeniti rezerviranog mjesta "[YourOutputAlias]" i "[YourInputAlias]" polja s pseudonime ulazni i izlazni rezultat koji želite koristiti prvi put. Osim toga, možete i dalje autora i testiranje upita na portalu klasični Azure bez definiranja unosa i izlaza na poslu.
    - Ako želite izvršiti dodatne obrada od jednostavnih prolazni, možete urediti definicije upita. Da biste započeli prilikom stvaranja upita, pogledajte neke uobičajene upita snimaju uzoraka [ovdje](stream-analytics-stream-analytics-query-patterns.md).  
  
    ![Upit za podatke prozora](./media/stream-analytics-write-queries/2-stream-analytics-write-queries.png)  

## <a name="to-validate-query-data-is-working"></a>Da biste provjerili valjanost podataka upita radi: ##

Možete testirati upit ponaša očekivani tako da pokrenete u web-pregledniku na jednu ili više lokalne JSON datoteka koja sadrži podatke za testiranje. To neće pokrenuti posao ili neki naplate utjecati.

> [AZURE.NOTE] Trenutno testiranje u pregledniku upita nije podržan na portalu za Azure.  

1.  Provjerite ima li bez pogrešaka u upitu (u suprotnom gumb za testiranje će biti onemogućena), a zatim kliknite gumb za testiranje.  

    ![Upit za podatke Test](./media/stream-analytics-write-queries/3-stream-analytics-write-queries.png)  

2.  Zatražit će se za određivanje datoteka za svaku unosi referencirani u upitu. U ovom primjeru upita predložak je preostalo kao-je, pa se dijaloški okvir upita za ulazni pod nazivom "yourinputalias".  

    ![Testiranje podatkovni upit](./media/stream-analytics-write-queries/4-stream-analytics-write-queries.png)  

3.  Pronađite datoteku za testiranje. Nekoliko ogledne datoteke dostupne su na [github](https://github.com/Azure/azure-stream-analytics/tree/master/Sample Data) , a možete dohvatiti i ogledne podatke iz vlastite unosa strujanja podataka putem funkciju ogledne podatke na kartici unosa.  

    ![Unos upita](./media/stream-analytics-write-queries/5-stream-analytics-write-queries.png)  

4.  Nakon zatvaranja dijalog, vaše će se izvoditi upit nad podacima test i prikazat će se rezultati pri dnu stranice upita.  

    ![Sažetak upita](./media/stream-analytics-write-queries/6-stream-analytics-write-queries.png)  

## <a name="get-help"></a>Zatražite pomoć
Dodatnu pomoć, pokušajte našem [forumu Azure strujanje Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Daljnji koraci

- [Uvod u Azure strujanje Analytics](stream-analytics-introduction.md)
- [Prvi koraci pri korištenju Azure strujanje Analytics](stream-analytics-get-started.md)
- [Promjena veličine Azure strujanje analize poslova](stream-analytics-scale-jobs.md)
- [Azure strujanje analize upita jezična referenca](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure strujanje analize upravljanje REST API Reference](https://msdn.microsoft.com/library/azure/dn835031.aspx)
