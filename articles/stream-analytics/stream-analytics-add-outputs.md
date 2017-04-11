<properties 
    pageTitle="Upute za konfiguriranje podataka Proizvodi za strujanje analize poslove | Microsoft Azure" 
    description="Konfiguriranje izlaze za strujanje analize poslove | učenje put segmenta."
    keywords="Slanje podataka premještanje podataka"
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

# <a name="how-to-configure-data-outputs-for-stream-analytics-jobs"></a>Upute za konfiguriranje podataka Proizvodi za strujanje Analitika poslova

Azure strujanje analize poslove moguće je povezati jedan ili više izlaze podataka koji određuju veze za postojeće primatelj za podatke. Tijekom analize toka posla obrađuje i pretvorbe ulazne podatke, strujanja podataka izlaz događaja zapisuje izlaz vaš posao.

Strujanje analize podataka izlaze može se koristiti za izvora u stvarnom vremenu pomoću nadzornih ploča ili upozorenja okidanje tijekova rada za premještanje podataka ili jednostavno arhiviranje podataka za skupna obrada kasnije. Analitički strujanje ima prve klase Integracija s nekoliko Azure servise koji su zabilježeni u Ovdje detaljno.

Da biste dodali u izlaz strujanje Analitika posla:

1. Na portalu klasični Azure kliknite **izlaze** , a zatim **Dodajte izlazna** u vaš posao strujanje analize.

    ![Dodavanje izlaze](./media/stream-analytics-add-outputs/1-stream-analytics-add-outputs.png)  

    Na portalu za Azure kliknite pločicu **izlaze** u vaš posao strujanje analize.

    ![Azure Porta dodavanje izlaze](./media/stream-analytics-add-outputs/5-stream-analytics-add-outputs.png)

2. Određivanje vrste izlaz:

    ![Odaberite vrstu podataka premještanja](./media/stream-analytics-add-outputs/2-stream-analytics-add-outputs.png)  

    ![Portal za Azure odaberite vrstu podataka premještanja](./media/stream-analytics-add-outputs/6-stream-analytics-add-outputs.png)

3. Navedite neslužbeni naziv za ovaj Izlaz u okvir **Pseudonim izlaz** . Ovaj naziv mogu se u upitu vaš posao kasnije da biste se pozvali na izlaz.  
    
    Unesite ostatak svojstva potrebna veze za povezivanje s izlaz.  Ta polja razlikuju se po vrsti izlazne i definiraju Ovdje detaljno.  

    ![Dodavanje svojstva izlaz podataka](./media/stream-analytics-add-outputs/3-stream-analytics-add-outputs.png)  

4. Ovisno o vrsti Izlaz, morate odrediti kako serijalizirani ili oblikovani podaci. Postavke određene serijalizacije za svaku vrstu izlaza su navedenih u nastavku.

    Unesite ostatak svojstva potrebna veze za povezivanje s izvorom podataka. Ta polja razlikuju se po vrsti vrste unos i izvora i definiraju detaljno [ovdje](stream-analytics-create-a-job.md).  

    ![Dodavanje podataka izlaz koncentrator događaja](./media/stream-analytics-add-outputs/4-stream-analytics-add-outputs.png)  

    ![Izlaz Azure portala podataka koncentrator događaja](./media/stream-analytics-add-outputs/7-stream-analytics-add-outputs.png)  

> [Azure.Note] Bilo kojeg elementa izlaz dodali na posao mora postojati posao pokreće i događaje početak slijedi. Na primjer, ako koristite spremište blobova platforme kao u izlaz, posao će ne stvorite račun za pohranu automatski. Potrebne za stvaranje korisnik prije pokretanja posla ASA.

## <a name="get-help"></a>Zatražite pomoć
Dodatnu pomoć, pokušajte našem [forumu Azure strujanje Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Daljnji koraci

- [Uvod u Azure strujanje Analytics](stream-analytics-introduction.md)
- [Prvi koraci pri korištenju Azure strujanje Analytics](stream-analytics-get-started.md)
- [Promjena veličine Azure strujanje analize poslova](stream-analytics-scale-jobs.md)
- [Azure strujanje analize upita jezična referenca](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure strujanje analize upravljanje REST API Reference](https://msdn.microsoft.com/library/azure/dn835031.aspx)
