<properties 
    pageTitle="Ispravljanje pogrešaka pomoću operacije i usluge zapisnike u strujanje analize | Microsoft Azure" 
    description="S uputama korištenje analize strujanje operacija zapisnika" 
    keywords="servis zapisnika"
    services="stream-analytics" 
    documentationCenter="" 
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

# <a name="debug-stream-analytics-jobs-using-service-and-operation-logs"></a>Ispravljanje pogrešaka strujanje analize zadatke pomoću servisa i operacija zapisnika

Sve servise Azure navedite radu zapisivanje poruke pojedinosti zapisa koji se odnose na operacija upravljanja korisnicima. U analize strujanje Azure, ove informacije koje se mogu koristiti za ispravljanje pogrešaka svrhe, kao što je prikaz statusa posla, napretka posla, a neuspjeh poruka da biste pratili tijek zadatka s vremenom iz započnite obrada za izlaz.

## <a name="find-operation-logs-in-the-azure-management-portal"></a>Pronalaženje operacija prijavi na portalu za upravljanje Azure

Zapisnik postupka možete pristupiti na dva načina:  

- Nadzorna ploča analize toka posla  
- Upravljanje servisima na portalu za Azure klasični  

## <a name="dashboard-of-the-stream-analytics-job"></a>Nadzorna ploča analize toka posla

Veza na odgovarajuće zapisnike posla strujanje analize prikazuje se na kartici posla nadzorne ploče. Ako kliknete tu vezu, postavlja filtre na način da prikazuje najnovije zapisnika za taj određeni posao.

  ![Odaberite servise upravljanja zapisnika](./media/stream-analytics-operation-logs/01-stream-analytics-operation-logs.png)  

## <a name="management-services"></a>Upravljanje servisima

Da biste ručno zapisnike postupak za strujanje analize i drugih servisa Azure klasični portalu:

1.  Kliknite **Management Services** [Azure klasični portal](https://manage.windowsazure.com).
2.  Odaberite **Strujanje Analytics** za **vrstu** i naziv posla za **Naziv usluge**.  

  ![Odaberite strujanje Analytics](./media/stream-analytics-operation-logs/02-stream-analytics-operation-logs.png)  

## <a name="find-audit-logs-in-the-azure-portal"></a>Pronalaženje zapisnike nadzora na portalu za Azure ##

Da biste pronašli radu zapisnika za svoj posao strujanje analize na portalu za Azure, kliknite **Pregledaj** i odaberite **zapisnika nadzora**.

  ![Portal za Azure odaberite strujanje Analytics](./media/stream-analytics-operation-logs/06-stream-analytics-operation-logs.png)  

Otvorit će se plohu prikazuje događaje iz zadnjih sedam dana za sve resurse za pretplatu.  Možete filtrirati da biste pogledali događaje navedite vrste ili vremenski okvir tako da kliknete naredbu **Filtar** .

  ![Portal za Azure odaberite strujanje Analytics](./media/stream-analytics-operation-logs/07-stream-analytics-operation-logs.png)  

## <a name="get-log-details"></a>Dohvaćanje detalja zapisnika

Možete filtrirati prema vremenski raspon i Status da biste pogledali zapisnike za svoj posao.

Na portalu za upravljanje Azure kliknite gumb **pojedinosti** pri dnu prozora da biste vidjeli dodatne detalje o odabrani događaj. 

  ![Odabir detalja](./media/stream-analytics-operation-logs/03-stream-analytics-operation-logs.png)  

Na portalu Azure kliknite stavka evidencije da biste vidjeli detaljne događaje unutar njega.

  ![Portal za Azure odaberite pojedinosti](./media/stream-analytics-operation-logs/08-stream-analytics-operation-logs.png)  

Iz nje, plohu **detalja** možete otvoriti i klikom na događaj.

  ![Portal za Azure odaberite pojedinosti](./media/stream-analytics-operation-logs/09-stream-analytics-operation-logs.png)  

## <a name="debug-a-failed-job"></a>Nije uspjelo posao za ispravljanje pogrešaka

Na portalu za upravljanje Azure kliknite ikonu pretraživanja i vrsta "nije uspjela". Tako ćete dobiti rezultat sve zapisnika s pogreške. 

  ![Ispravljanje pogrešaka nije uspjelo posla](./media/stream-analytics-operation-logs/04-stream-analytics-operation-logs.png)  

Na portalu Azure možete filtrirati prema razina poruke da biste pogledali događaje **od ključne važnosti** .

  ![Azure portala za ispravljanje pogrešaka](./media/stream-analytics-operation-logs/10-stream-analytics-operation-logs.png)  

Možete odabrati neku od na pogreške i kliknite **Detalji** dodatne informacije o pogrešci.  Neke poruke o pogreškama sadrže i upute da biste smanjili problem. 

  ![Pojedinosti operacije](./media/stream-analytics-operation-logs/05-stream-analytics-operation-logs.png)  

U slučaju da trebate obratiti [službi za podršku](https://azure.microsoft.com/support/options/) ili podatke u tim putem [MSDN forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics), uzmite u obzir pojedinosti operacije Konkretno **ID korelacije**. 

## <a name="get-help"></a>Zatražite pomoć
Dodatnu pomoć, pokušajte našem [forumu Azure strujanje Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Daljnji koraci

- [Uvod u Azure strujanje Analytics](stream-analytics-introduction.md)
- [Prvi koraci pri korištenju Azure strujanje Analytics](stream-analytics-get-started.md)
- [Promjena veličine Azure strujanje analize poslova](stream-analytics-scale-jobs.md)
- [Azure strujanje analize upita jezična referenca](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure strujanje analize upravljanje REST API Reference](https://msdn.microsoft.com/library/azure/dn835031.aspx)
