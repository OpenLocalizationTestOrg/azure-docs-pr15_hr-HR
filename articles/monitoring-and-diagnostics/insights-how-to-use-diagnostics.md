<properties
    pageTitle="Omogući nadzor i dijagnostici u Microsoft Azure | Microsoft Azure "
    description="Saznajte kako postaviti Dijagnostika za resurse u Azure."
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

# <a name="enable-monitoring-and-diagnostics"></a>Omogućivanje nadzor i dijagnostike

[Portal za Azure](https://portal.azure.com)možete konfigurirati bogatih, često korištenih, nadzor i Dijagnostika podataka o resurse. Da biste konfigurirali Dijagnostika programski možete koristiti i [REST API -JA](https://msdn.microsoft.com/library/azure/dn931932.aspx) ili [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) .

Dijagnostika, nadzor i metriku podataka u Azure sprema se u račun za pohranu po izboru. Omogućuje korištenje bilo kakve tooling želite pročitati podatke, u eksploreru za pohranu Power BI te tooling drugih proizvođača.

## <a name="when-you-create-a-resource"></a>Prilikom stvaranja resursa

Većina servisi omogućuju da omogućite Dijagnostika kada prvi put stvorite ih [Azure Portal](https://portal.azure.com).

1. Idite na **Novo** , a zatim odaberite resurs koji vas zanima.

2. Odaberite **neobavezno konfiguracija**.
    ![Dijagnostika plohu](./media/insights-how-to-use-diagnostics/Insights_CreateTime.png)

3. Odaberite **Dijagnostika**pa kliknite **na**. Morat ćete odaberite račun za pohranu koju želite da se Dijagnostika spremiti u. Prikazat će se naplatiti stope podatke za pohranu i transakcija prilikom slanja Dijagnostika s računom za pohranu.

4. Kliknite **u redu** pa stvorite resurs.

## <a name="change-settings-for-an-existing-resource"></a>Promjena postavki za postojeći resurs

Ako ste već stvorili resursa i upute za promjenu postavki dijagnostike (da biste, primjerice promijenili na razini zbirke podataka), to možete učiniti tog prava na portalu za Azure.

1. Idite na resursa i kliknite naredbu **Postavke** .

2. Odaberite **Dijagnostika**.

3. **Dijagnostika** plohu sadrži sve moguće Dijagnostika i nadzora zbirke podataka za taj resurs. Za neke resurse možete odabrati i pravilnika o **zadržavanju** za podatke za čišćenje programa s računa za pohranu.
    ![Dijagnostika za pohranu](./media/insights-how-to-use-diagnostics/Insights_StorageDiagnostics.png)

4. Nakon što ste odabrali postavke, kliknite naredbu **Spremi** . U roku malo dok za praćenje podataka koja će se prikazivati ako su je omogućiti za prvi put.

### <a name="categories-of-data-collection-for-virtual-machines"></a>Kategorije podataka zbirke virtualnim strojevima
Za virtualnim strojevima sve metriku i zapisnika ostat će zabilježen u intervalima od minute tako da možete uvijek imate najnovije informacije o vašem računalu.

- **Osnovni metriku** : stanje metriku o virtualnog računala kao što su procesora i memorije
- **Metriku mreže i na webu** : metriku o mrežnih veza i web-usluge
- **.NET metriku** : metriku o aplikacije .NET i ASP.NET koje rade na virtualnog računala
- **SQL metriku** : Ako su pokrenuti servis Microsoft SQL, njegov metriku performansi
- **Zapisnici aplikacije događaja sustava Windows** : događaja sustava Windows koji su poslani kanal za aplikaciju
- **Zapisnici sustava događaja sustava Windows** : događaja sustava Windows koji su poslani kanal za sustav. To obuhvaća i sve događaje iz [Microsoft Antimalware](http://go.microsoft.com/fwlink/?LinkID=404171&clcid=0x409).
- **Zapisnici sigurnost događaja sustava Windows** : događaja sustava Windows koji su poslani kanal za sigurnost
- **Zapisnike infrastrukture za dijagnostiku** : zapisivanje o zbirke infrastruktura za dijagnostiku
- **Zapisnici IIS** : zapisnici o IIS poslužitelj

Imajte na umu da trenutno nisu podržani određene distribucija Linux i, na virtualnog računala mora biti instaliran Agent za goste.

## <a name="next-steps"></a>Daljnji koraci

* [Primanje upozorenja](insights-receive-alert-notifications.md) kad god radu događajima ili metriku Unakrsna praga.
* [Metriku monitora servisa](insights-how-to-customize-monitoring.md) da biste bili sigurni na servisu dostupan je i odredište.
* [Automatski skalirali broj instanci](insights-how-to-scale.md) da biste bili sigurni vaš skaliranje servis koji se temelji na zahtjev.
* [Performanse aplikacije monitor](../application-insights/app-insights-azure-web-apps.md) ako želite da biste shvatili točno onako kako kod izvršava u oblaku.
* [Pregled događaja i zapisnika nadzora](insights-debugging-with-events.md) da biste saznali sve koji dogodilo na servisu.
* [Praćenje stanja servisa](insights-service-health.md) da biste saznali kada Azure naišao prekidi performanse smanjene performanse ili servis.
