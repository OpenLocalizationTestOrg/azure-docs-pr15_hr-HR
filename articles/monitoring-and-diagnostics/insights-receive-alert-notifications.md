<properties
    pageTitle="Primanje upozorenja za Azure servise | Microsoft Azure"
    description="Primati obavijesti kada se zadovolje uvjeti upozorenja pravila."
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
    ms.date="09/08/2015"
    ms.author="robb"/>

# <a name="receive-alert-notifications"></a>Primanje upozorenja

Možete primati upozorenja ovisno o praćenju metriku ili događaji na servisa Azure.

Upozorenja pravilo na metrike vrijednosti, kada se vrijednost navedena metrika siječe prag dodijeljeni upozorenja postaje aktivna i pravila možete poslati obavijest. Za pravilo upozorenja o događajima pravilo možete poslati obavijest na *svaki* događaj ili, samo kada se određeni broj događaja javiti.

Kada stvorite pravilo upozorenja, možete odabrati mogućnosti da biste poslali obavijest putem e-pošte da biste administrator servisa i dodatnih administratora ili drugog administratora možete odrediti. Obavijesti e-pošte šalje se kada pravilo postaje aktivna i kada je riješen upozorenja uvjet.

Možete koristiti [REST API -JA](https://msdn.microsoft.com/library/azure/dn931945.aspx) ili [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) za konfiguriranje i programatski pristup informacijama o upozorenja pravila.

## <a name="create-an-alert-rule"></a>Stvaranje upozorenja pravila

1. [Portal](https://portal.azure.com/)kliknite **Pregledaj**, a zatim resurs koji vas zanima nadzor.

2. Kliknite pločicu **upozorenja pravila** u leće **operacija** .

3. Kliknite naredbu **Dodaj upozorenje** .

    ![Dodavanje upozorenje](./media/insights-receive-alert-notifications/Insights_AddAlert.png)

4. Naziv upozorenja pravilo i odaberite opis koji će se vidjeti obavijesti e-pošte.

5. Kad odaberete **metriku** ćete odaberite uvjet i vrijednost praga za metriku. Ovo je vremenskog razdoblja koja koristi Azure na monitoru i crtanja upozorenja aktivnost.

    ![Uvjet i praga.](./media/insights-receive-alert-notifications/Insights_ConditionAndThreshold.png)

6. Možete odaberite **događaji**i dobili obavijest kad se dogodi određene događaj.

    ![Događaji](./media/insights-receive-alert-notifications/Insights_Events.png)

7. Na kraju, možete odabrati da biste poslali obavijest e-pošte odgovorne administratorima.

Nakon što kliknete **Spremi**, za nekoliko minuta će informirali kad god metriku odaberete premašuje prag.

## <a name="managing-your-alert-rules"></a>Upravljanje pravilima upozorenja

Kada ste stvorili pravilo upozorenja, možete pogledati Pretpregled na upozorenju prag usporedbi metriku na prethodni dan.

![Događaji](./media/insights-receive-alert-notifications/Insights_EditAlert.png)


Naravno možete urediti ovo pravilo za upozorenja i **Onemogućivanje** i **Omogućivanje** je ako želite privremeno prekinuti primanje obavijesti o njemu.

## <a name="next-steps"></a>Daljnji koraci

* [Konfiguriranje webhooks na upozorenja](insights-webhooks-alerts.md) za usmjeravanje obavijesti raznih kanala
* [Monitor servisa metriku](insights-how-to-customize-monitoring.md) da biste bili sigurni na servisu dostupan je i odredište.
* [Omogućite praćenje i dijagnostici](insights-how-to-use-diagnostics.md) da biste prikupili detaljne velika učestalost metriku o vašoj usluzi.
* [Monitor dostupnost i odziv web-stranicu](../application-insights/app-insights-monitor-web-app-availability.md) aplikacije uvida tako da možete saznati ako stranici nije dostupno.
* [Performanse aplikacije monitor](../application-insights/app-insights-azure-web-apps.md) ako želite da biste shvatili točno onako kako kod izvršava u oblaku.
* [Pregled događaja i zapisnika nadzora](insights-debugging-with-events.md) da biste saznali sve koji dogodilo na servisu.
* [Praćenje stanja servisa](insights-service-health.md) da biste saznali kada Azure naišao prekidi performanse smanjene performanse ili servis.
