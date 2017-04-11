<properties
    pageTitle="Pregled metriku u Microsoft Azure | Microsoft Azure"
    description="Saznajte kako prilagoditi nadzora grafikona u Azure."
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

# <a name="overview-of-metrics-in-microsoft-azure"></a>Pregled metriku u Microsoft Azure

Sve servise Azure pratiti ključa metriku koja omogućuju vam praćenje stanja performanse, dostupnost i način korištenja servisa. Možete pogledati te metriku na portalu za Azure, a možete koristiti i [REST API -JA](https://msdn.microsoft.com/library/azure/dn931930.aspx) ili [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) da programski pristupe potpunog skupa metriku.

Za neke usluge možda ćete morati uključiti dijagnostiku da biste vidjeli sve metriku. Za druge korisnike, kao što su virtualnim strojevima dobit ćete osnovni skup metriku, ali morate omogućiti na svim postaviti visoko učestalost metriku. Potražite u članku [Omogućivanje nadzor i dijagnostici](insights-how-to-use-diagnostics.md) da biste saznali više.

## <a name="using-monitoring-charts"></a>Pomoću nadzora grafikona

Bilo koji od metriku stvaranja grafikona im iznad bilo koje vremensko razdoblje odaberete.

1. [Portal za Azure](https://portal.azure.com/)kliknite **Pregledaj**, a zatim resurs koji vas zanima nadzor.

2. U odjeljku **nadzor** sadrži najvažnije metriku za svaki Azure resurs. Ako, na primjer, na koji se web-aplikacijama sadrži **zahtjeva za te pogreške**, pri čemu su virtualnog računala promijenile **postotak procesora** i **na disku za čitanje i pisanje**:  ![leće praćenja](./media/insights-how-to-customize-monitoring/Insights_MonitoringChart.png)

3. Klikom na bilo koji od njih vode plohu **metriku** . Na plohu osim na grafikonu se tablicu koja prikazuje agregacije metriku (kao što je prosjek, minimum i maksimum, tijekom vremenskog razdoblja koje ste odabrali). Ispod koje su upozorenja pravila za resurs.
    ![Metričkim plohu](./media/insights-how-to-customize-monitoring/Insights_MetricBlade.png)

4. Da biste prilagodili crte koje se pojavljuju, kliknite gumb **Uredi** na grafikonu ili, naredba **Uređivanje grafikona** na metrike plohu.

5. Na plohu uređivanje upita možete učiniti tri stvari:
    - Promjena vremenskog raspona
    - Prebacivanje prikaza između traka i crta
    - Odaberite drugi metics ![uređivanje upita](./media/insights-how-to-customize-monitoring/Insights_EditQuery.png)

6. Promjena vremenskog raspona dovoljno je odaberete neki drugi raspon (kao što su **Proteklog Hour**) i kliknete **Spremi** pri dnu zaslona u plohu. Možete odabrati i **Prilagođena**, koji omogućuje vam da odaberete bilo koju vremensko razdoblje tijekom proteklih dva tjedna. Na primjer, možete vidjeti na cijelu dva tjedna ili, samo 1 sat jučer. Upišite u tekstni okvir te unesite drugu sat.
    ![Prilagođeni vremenskog raspona](./media/insights-how-to-customize-monitoring/Insights_CustomTime.png)

7. Ispod vremenski raspon koji promjene odaberite bilo koji broj metriku za prikaz na grafikonu.

8. Kada kliknete Spremi promjene spremit će se za koji je određeni resurs. Na primjer, ako imate dvije virtualnim strojevima i promjena grafikona u jednoj aplikaciji, ona neće utjecati na drugi.

## <a name="creating-side-by-side-charts"></a>Stvaranje grafikona jedno uz drugo

Napredna prilagodba na portalu možete dodati proizvoljan broj grafikona koje želite.

1. Na izborniku **...** pri vrhu na plohu kliknite **Dodavanje pločica**:  
    ![Dodavanje izbornika](./media/insights-how-to-customize-monitoring/Insights_AddMenu.png)
2. Nakon toga ste odaberite grafikon možete odabrati iz **galerije** na desnoj strani zaslona:  ![galerije](./media/insights-how-to-customize-monitoring/Insights_Gallery.png)
3. Ako ne vidite metriku koju želite, uvijek možete dodati jedan od unaprijed metriku i **Uređivanje** grafikona da bi se prikazala metriku koje su vam potrebne.

## <a name="monitoring-usage-quotas"></a>Nadzor korištenja kvote

Većina metriku pokazuju trendove tijekom vremena, ali neke podatke, kao što je kvota za korištenje su točke u vrijeme informacije s praga.

Možete vidjeti i kvotama za korištenje na plohu za resurse koji imaju kvote:

![Korištenje](./media/insights-how-to-customize-monitoring/Insights_UsageChart.png)

Kao što je s metrika, možete koristiti [REST API -JA](https://msdn.microsoft.com/library/azure/dn931963.aspx) ili [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) da programski pristupe potpunog skupa kvote za korištenje.

## <a name="next-steps"></a>Daljnji koraci

* [Primanje upozorenja](insights-receive-alert-notifications.md) kad god metrike presjek praga.
* [Omogućite praćenje i dijagnostici](insights-how-to-use-diagnostics.md) da biste prikupili detaljne velika učestalost metriku o vašoj usluzi.
* Da biste bili sigurni na servisu dostupan je i odredište [automatski skalirali broj instanci](insights-how-to-scale.md) .
* [Performanse aplikacije monitor](../application-insights/app-insights-azure-web-apps.md) ako želite da biste shvatili točno onako kako kod izvršava u oblaku.
* Da biste pristupili klijent analitičke podatke o preglednicima koji posjetite web-mjesto, slijedite [Uvide aplikacije za JavaScript aplikacije i web-stranice](../application-insights/app-insights-web-track-usage.md) .
* [Monitor dostupnost i odziv web-stranicu](../application-insights/app-insights-monitor-web-app-availability.md) aplikacije uvida tako da možete saznati ako stranici nije dostupno.
