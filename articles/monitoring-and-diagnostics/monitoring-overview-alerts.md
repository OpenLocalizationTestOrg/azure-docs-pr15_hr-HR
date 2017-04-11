<properties
    pageTitle="Pregled upozorenja u programu Microsoft Azure | Microsoft Azure"
    description="Upozorenja omogućuju vam praćenje metriku Azure resursa, događaji i zapisnika i primati obavijesti kada se ispuni uvjet koji navedete."
    authors="rboucher"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/24/2016"
    ms.author="robb"/>

# <a name="overview-of-alerts-in-microsoft-azure"></a>Pregled upozorenja u programu Microsoft Azure


U ovom se članku opisuje što upozorenja nalaze, njihove prednosti te kako započeti s korištenjem ih.  

## <a name="what-are-alerts"></a>Što su upozorenja?
Upozorenja su način nadzora metriku Azure resursa, događaje, ili zapisnika i zatim ne primite obavijest kada se uvjet koji navedete ispunjen.

Možete primati upozorenja na temelju:

- **Metričkim vrijednostima**: pokreće upozorenje kada se vrijednost navedena metrika presjek prag koje dodijelite u bilo kojem smjeru. To jest, ga i pokreće kad prvi put ispuni uvjet i pa nakon toga kad koji uvjet je više ne zadovoljavaju.
- **Aktivnosti zapisnika događaja**: ovo upozorenje možete pokrenuti svaki događaj ili samo kad se pojave određeni broj događaja.

## <a name="alerts-in-different-azure-services"></a>Upozorenja u različitim servisima za Azure

Upozorenja dostupne su u različitim servisima, uključujući:

- **Aplikacija uvida**: omogućuje web-test i metriku upozorenja. Potražite u članku [Postavljanje upozorenja u aplikaciji uvida](../application-insights/app-insights-alerts.md) i [dostupnost monitora i odziv bilo kojeg web-mjesta](../application-insights/app-insights-monitor-web-app-availability.md).
- **Zapisnik analize (operacije upravljanja paket)**: omogućuje usmjeravanje zapisnicima dijagnostičkih podataka za zapisnik analize. Paket za upravljanje operacije omogućuje metrika, zapisnika i ostalih upozorenja. Dodatne informacije potražite u članku [upozorenja u zapisnik analize](../log-analytics/log-analytics-alerts.md).  
- **Azure Monitor**: omogućuje upozorenja na temelju metričkim vrijednostima i aktivnosti zapisnika događaja. Azure Monitor obuhvaća [Azure Monitor REST API -JA](https://msdn.microsoft.com/library/dn931943.aspx).  Dodatne informacije potražite u članku [Korištenje portala za Azure, komponente PowerShell ili sučelja naredbenog retka za stvaranje upozorenja](insights-alerts-portal.md).

## <a name="alert-actions"></a>Akcije za upozorenje
Možete konfigurirati upozorenje učinite sljedeće:

- Slanje obavijesti e-poštom administrator servisa, dodatnih administratora ili adrese dodatne e-pošte koju navedete.
- Nazovite webhook koji omogućuje pokretanje Automatizacija dodatne akcije.

 ![Upozorenja objašnjenje](./media/monitoring-overview-alerts/AlertsOverviewResource3.png)


## <a name="next-steps"></a>Daljnji koraci

Dohvaćanje podataka o upozorenja pravila i konfiguriranje ih pomoću:

- [Portal za Azure](insights-alerts-portal.md)
- [PowerShell](insights-alerts-powershell.md)
- [Sučelje naredbenog retka (EŽA)](insights-alerts-command-line-interface.md)
- [Azure Monitor REST API-JA](https://msdn.microsoft.com/library/azure/dn931945.aspx)
