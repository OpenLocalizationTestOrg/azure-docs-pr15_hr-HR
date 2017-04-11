<properties
    pageTitle="Pregled događaja i zapisnika nadzora"
    description="Saznajte kako da biste vidjeli sve događajima koji se odvijaju u pretplatu za Azure."
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
    ms.date="04/28/2015"
    ms.author="robb"/>

# <a name="view-events-and-audit-logs"></a>Pregled događaja i zapisnika nadzora

Sve operacije obavljene Azure resursa potpuno revizije Azure resursa upravitelj, od stvaranja i brisanja za dodjelu i opoziv programa access. Možete pregledavati ti zapisnici na portalu za Azure, a možete koristiti i [REST API -JA](https://msdn.microsoft.com/library/azure/dn931927.aspx) ili [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) da programski pristupe potpunog skupa događaja.

## <a name="browse-the-events-impacting-your-azure-subscription"></a>Pregled događaja koje utječu na pretplatu Azure

1. Prijava na [Portal za Azure](https://portal.azure.com/).
2. Kliknite **Pregledaj** i odaberite **zapisnika nadzora**.  
    ![Pregled koncentratora](./media/insights-debugging-with-events/Insights_Browse.png)
3. Otvorit će se plohu prikazivanja svih događaja koji su neki od pretplate utjecati zadnjih sedam dana. Pri vrhu je grafikona s prikazom podataka po razini i ispod koje je cijeli popis zapisnika:  ![svih događaja](./media/insights-debugging-with-events/Insights_AllEvents.png)

>[AZURE.NOTE] 500 najnovije događaje za dani pretplatu možete vidjeti samo na portalu za Azure.

4. Kliknite na bilo koju stavku zapisnika da biste pogledali događaje koja je proizvela prema gore. Ako, na primjer, ako pokrenete nešto u grupu resursa, mnogo različitih resursa može biti stvorene ili izmijenjene. Za svaku stavku možete vidjeti:
    * **Razinu** događaj – na primjer, je mogao biti samo nešto da biste pratili (**Informational**) ili kada na nešto je nestalo pogrešan da morate znati o (**pogreška**).
    * **Status** - konačni status obično bit će **je uspjelo** ili **nije uspjelo**, ali ne može biti **prihvaćena** za dugoročnih postupaka.
    * Došlo je do *Kada* događaj.
    * *Koji je* izvršila operaciju, ako svi. Sve operacije izvršavaju korisnici, neke se izvode pozadinskog Services da bi oni će imati **pozivatelja**.
    * **ID korelacije** događaja – to je jedinstveni identifikator za tu vrstu operacije.

5. Iz njega možete posjetiti plohu detalje da biste vidjeli pojedinosti događaja.

    ![Grupa resursa](./media/insights-debugging-with-events/Insights_EventDetails.png)

    Za događaje **nije uspjelo** obično sadrži **Substatus** i sekciji **svojstava** koji sadrže korisnih pojedinosti za ispravljanje pogrešaka svrhe.

## <a name="filter-to-specific-logs"></a>Filtriranje određene zapisnika

Da biste vidjeli događaja koji se odnose na određene entitet ili određene vrste, možete filtrirati plohu zapisnika nadzora klikom na naredbu **Filtar** . Koristite plohu filtar da biste promijenili u **vrijeme obuhvaćaju** od plohu zapisnika nadzora.

Kada kliknete tu naredbu, otvorit će se novi plohu:

![Filtar](./media/insights-debugging-with-events/Insights_EventFilter.png)

Postoje četiri vrste filtara:

1. Putem pretplate
2. **Grupa resursa**
3. Po **vrsti resursa**
4. Prema određenom **resursa** – za to morate prošle u potpunu *ID resursa* resursa koje vas zanimaju

Osim toga, možete filtrirati i događaja koji je obavio događaja ili, razinu događaja.

Kada dovršite odaberete što želite vidjeti, kliknite gumb **Ažuriraj** pri dnu zaslona u plohu.

## <a name="monitor-events-impacting-specific-resources"></a>Praćenje događaja koje utječu na određene resursi

1. Kliknite **Pregledaj** da biste pronašli resurs koji vas zanima. Možete vidjeti i sve zapisnike za cijelu **grupu resursa**.
2. Na plohu resursa, pomaknite se prema dolje dok ne pronađete pločicu **događaja** .  
    ![Pločica događaja](./media/insights-debugging-with-events/Insights_EventsTile.png)
3. Kliknite na toj pločici da biste pogledali događaje filtrirane resursa koji ste odabrali. Naredba **Filtriraj** možete koristiti da biste promijenili vremenski raspon ili primijenite točno određene filtre.

## <a name="next-steps"></a>Daljnji koraci

* [Primanje upozorenja](insights-receive-alert-notifications.md) kad god se događa događaja.
* [Metriku monitor servisa](insights-how-to-customize-monitoring.md) da biste bili sigurni na servisu dostupan je i odredište.
* [Praćenje stanja servisa](insights-service-health.md) da biste saznali kada Azure naišao prekidi performanse smanjene performanse ili servis.  
