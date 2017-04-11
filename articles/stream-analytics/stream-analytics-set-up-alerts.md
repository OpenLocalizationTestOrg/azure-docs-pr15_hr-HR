<properties
    pageTitle="Postavljanje upozorenja za upite na strujanje analize | Microsoft Azure"
    description="Razumijevanje strujanje analize upozorenjem"
    keywords="Postavljanje upozorenja"
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


# <a name="set-up-alerts-for-azure-stream-analytics-jobs"></a>Postavljanje upozorenja za Azure strujanje analize poslove

## <a name="introduction-monitor-page"></a>Uvod: Stranica monitora

Upozorenja možete postaviti tako da pokreću upozorenja kada metrike dosegne uvjet koji navedete.

Na primjer, "Ako je izlazna događaji tijekom posljednjeg 15 minuta < 100, slanje obavijesti e-pošte ID e-pošte: xyz@company.com”.

Pravila možete postaviti na metriku putem portala sustava ili može biti konfigurirano [programski](https://code.msdn.microsoft.com/windowsazure/Receive-Email-Notifications-199e2c9a) iznad podataka za zapisnik postupka.

## <a name="set-up-alerts-through-the-azure-classic-portal"></a>Postavljanje upozorenja putem klasične portala za Azure

Postavljanje upozorenja na portalu za Azure klasični na dva načina:  

1.  Kartica **Monitor** analize toka posla  
2.  Operacije prijava Management services  

## <a name="set-up-alert-through-the-monitor-tab-of-the-job-in-the-portal"></a>Postavljanje upozorenja putem kartici Monitor posla na portalu

1.  Na kartici monitor odaberite metriku, kliknite gumb **Dodaj pravilo** na dnu zaslona nadzornu ploču i postavljanje pravila.  

    ![Nadzorna ploča](./media/stream-analytics-set-up-alerts/01-stream-analytics-set-up-alerts.png)  

2.  Definiranje naziva i opisa upozorenja  

    ![Stvaranje pravila](./media/stream-analytics-set-up-alerts/02-stream-analytics-set-up-alerts.png)  

3.  Unesite pragovi, prozor za procjenu upozorenja i akcije upozorenja  

    ![Definiranje uvjeta](./media/stream-analytics-set-up-alerts/03-stream-analytics-set-up-alerts.png)  

## <a name="set-up-alerts-through-the-operations-logs"></a>Postavljanje upozorenja putem zapisnike operacije

1.  Idite na karticu **upozorenja** u Management Services [Azure klasični Portal](https://manage.windowsazure.com).  
2.  Kliknite **Dodaj pravilo**  

    ![Kriterij](./media/stream-analytics-set-up-alerts/04-stream-analytics-set-up-alerts.png)  

3.  Definiranje naziva i opisa upozorenja. Odaberite "Strujanje analize" kao vrsta servisa i naziva zadatka kao naziv usluge.  

    ![Definiranje upozorenja](./media/stream-analytics-set-up-alerts/05-stream-analytics-set-up-alerts.png)  

## <a name="set-up-alerts-in-the-azure-portal"></a>Postavljanje upozorenja na portalu za Azure ##

Na portalu Azure pronađite strujanje analize posao koja vas zanima tako upozorava na, a zatim kliknite u odjeljku **nadzor** .  U **metriku** plohu koji će se otvoriti kliknite naredbu **Dodaj upozorenje** .

  ![Azure postavljanje portala](./media/stream-analytics-set-up-alerts/06-stream-analytics-set-up-alerts.png)  

Naziv upozorenja pravilo i odaberite opis koji će se vidjeti obavijesti e-pošte.

Kad odaberete metriku ćete odaberite uvjet i vrijednost praga za metriku.

  ![Azure portala odaberite metrički](./media/stream-analytics-set-up-alerts/07-stream-analytics-set-up-alerts.png)  

Više pojedinosti o konfiguriranju upozorenja na portalu za Azure potražite u članku [primanje upozorenja](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).  

## <a name="get-help"></a>Zatražite pomoć
Dodatnu pomoć, pokušajte našem [forumu Azure strujanje Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Daljnji koraci

- [Uvod u Azure strujanje Analytics](stream-analytics-introduction.md)
- [Prvi koraci pri korištenju Azure strujanje Analytics](stream-analytics-get-started.md)
- [Promjena veličine Azure strujanje analize poslova](stream-analytics-scale-jobs.md)
- [Azure strujanje analize upita jezična referenca](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure strujanje analize upravljanje REST API Reference](https://msdn.microsoft.com/library/azure/dn835031.aspx)
