<properties
    pageTitle="Vizualni prikaz i otklanjanje poteškoća sa zadacima strujanje analize | Microsoft Azure"
    description="Saznajte kako vizualizacija kanal za posao strujanje Analytics za samostalno otklanjanje poteškoća s pomoću značajke dijagnostike dijagrama."
    keywords=""
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"
/>


# <a name="visualize-and-troubleshoot-stream-analytics-jobs"></a>Vizualni prikaz i otklanjanje poteškoća sa zadacima strujanje analize

U analize strujanje s druge tehnologije oblaku otklanjanje poteškoća ponekad nije potrebno da pogleda Zašto posao ne dobijete očekivani rezultat (ili bilo kakav izlaz za taj relevantnim). Uz to na umu strujanje analize pruža mogućnost za vizualizacija strujanje posao. Ovo je praktičan kao alat za Modeliranje i ima prednost strani te dokumentacije zahtijevanje rade.

Na ploči vizualizacije ulaza su vidljive te upit izvršena i zatim sve na izlaze konfiguriran. Problemi s povezivanjem ili konfiguracije može postati više vidljivu i također može biti korisno da biste vidjeli vizualni prikaz konfiguracije.

## <a name="using-the-diagnosis-diagram-tool"></a>Pomoću alata za Dijagnostika dijagrama

Da biste pristupili ovaj visualizer, jednostavno kliknite gumb "Dijagnostika dijagram" u "Postavke" plohu od s posla strujanje analize.

![Stream-Analytics-Troubleshoot-visualization-Diagnosis-Diagram](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-diagnosis-diagram1.png)

Svaki ulazni i izlazni je boja programiranja da biste naznačili trenutnog stanja komponente, kao što je prikazano u nastavku.

![Stream-Analytics-Troubleshoot-visualization-Input-Map](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-input-map.png)

Kada korisnik želi pregled koraka Srednja upita da biste shvatili uzoraka protok podataka unutar posla, alat za vizualizaciju omogućuje prikaz analitički upita u komponenti korake i tijek niz. Klikom na svakog koraka upita vode odgovarajućih sekcije u upitu uređivanje okno kao što je prikazano. 

![Stream-Analytics-Troubleshoot-visualization-intermediate-Steps](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-intermediate-steps.png)




## <a name="next-steps"></a>Daljnji koraci

- [Uvod u Azure strujanje Analytics](stream-analytics-introduction.md)
- [Prvi koraci pri korištenju Azure strujanje Analytics](stream-analytics-get-started.md)
- [Promjena veličine Azure strujanje analize poslova](stream-analytics-scale-jobs.md)
- [Azure strujanje analize upita jezična referenca](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure strujanje analize upravljanje REST API Reference](https://msdn.microsoft.com/library/azure/dn835031.aspx)
