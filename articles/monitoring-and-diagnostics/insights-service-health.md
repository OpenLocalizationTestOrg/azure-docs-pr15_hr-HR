<properties
    pageTitle="Praćenje stanja servisa Azure pomoću Azure nadzornik aktivnosti zapisnika | Microsoft Azure"
    description="Saznajte kada Azure naišao prekidi performanse smanjene performanse ili servis. "
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
    ms.date="10/20/2016"
    ms.author="robb"/>

# <a name="track-azure-service-health-using-azure-monitor-activity-logs"></a>Praćenje stanja servisa Azure pomoću Azure nadzornik aktivnosti zapisnika

Azure publicizes svaki put kada postoji u prekida ili performanse smanjene performanse servisa. Možete pregledavati ovih događaja na portalu za Azure, a možete koristiti i [REST API -JA](https://msdn.microsoft.com/library/azure/dn931927.aspx) ili [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) da programski pristupe potpunog skupa događaja.

## <a name="browse-the-service-health-logs-for-your-subscription"></a>Pregled zapisnike o stanju servisa za vašu pretplatu

1. Prijava na [Portal za Azure](https://portal.azure.com/).

2. Na **Početak** trebali biste vidjeti pločicu naziva **Stanje servisa**. Kliknite je.

    ![Početna stranica](./media/insights-service-health/Insights_Home.png)

3. Pogledajte popis svih područja Azure. Kliknite bilo koju regije da bi se prikazala zapisnik aktivnosti upita koji prikazuje incidentima koje imaju bilo koji od pretplate utjecati zadnja 24 sata.

    ![Stanje servisa za aktivnosti zapisnika pretplate](./media/insights-service-health/AzureActivityLogServiceHealth3.png)

4. Detalje o incident pojedinačne možete vidjeti tako da kliknete taj događaj u tablici.

5. Promijenite **vremenski raspon** da biste vidjeli više vremenskog razdoblja.

## <a name="next-steps"></a>Daljnji koraci

* [Dostupnost monitora i odziv web-stranicu](../application-insights/app-insights-monitor-web-app-availability.md) aplikacije uvida tako da možete saznati ako stranici nije dostupno.
