<properties
    pageTitle="Početak rada s monitora Azure | Microsoft Azure"
    description="Početak rada s monitora Azure dobiti uvid u operacija resurse i poduzmite akciju temeljiti podataka."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="johnkem"/>

# <a name="get-started-with-azure-monitor"></a>Početak rada s monitora Azure

Azure Monitor je servis platformu koja predstavlja jedan izvor za nadzor Azure resursi. Pomoću nadzora Azure možete vizualizacija, upit, usmjeravanje, arhiviranje i akcija na stranici metrika i zapisnika koji dolaze iz resursa u Azure. Možete raditi s tim podacima pomoću na monitoru portala plohu, [PowerShell cmdleti za nadzor](./insights-powershell-samples.md), [Više platformi EŽA](insights-cli-samples.md)ili [Azure Monitor REST API -ji](https://msdn.microsoft.com/library/dn931943.aspx). U ovom se članku vodit ćemo kroz nekoliko komponenti za ključne monitora Azure pomoću portala za demonstraciju.

1. Na portalu dođite do **nove servise** i pronaći željenu mogućnost **Monitor** . Kliknite ikonu zvjezdice da biste dodali tu mogućnost na popis favorita tako da bude uvijek pristupačne na lijevoj navigacijskoj traci.

    ![Praćenje na popisu servisa](./media/monitoring-get-started/monitor-more-services.png)

2. Kliknite mogućnost **monitora** otvorite plohu **Monitor** . U ovom plohu objedinjuje sve svoje nadzora postavke i podatke u jednom prikazu Konsolidirani. Najprije se otvara u odjeljku **zapisnik aktivnosti** .

    ![Navigacija plohu monitora](./media/monitoring-get-started/monitor-blade-nav.png)

    > [AZURE.WARNING] Mogućnosti **usluge obavijesti** i **obavijesti grupe** prikazuje iznad pojavit će se samo na one koji su se pridružili privatne pregled tih značajki.

    Azure monitora ima tri osnovne kategorije podataka za nadzor: **zapisnik aktivnosti**, **metrike**i **dijagnostičke zapisnike**.

3. Kliknite da biste bili sigurni da se prikazuje u odjeljku zapisnik aktivnosti **prijave aktivnosti** .

    ![Plohu zapisnik aktivnosti](./media/monitoring-get-started/monitor-act-log-blade.png)

    [**Zapisnik aktivnosti**](./monitoring-overview-activity-logs.md) opisuju sve operacije izvršiti na resursa u svoju pretplatu. Korištenje zapisnik aktivnosti, možete odrediti na "što, tko i kada ' za stvaranje, ažuriranje i brisanje operacije resursa u svoju pretplatu. Na primjer, zapisnik aktivnosti obavijestit će vas kada je zaustavljeno web-aplikacijama i tko je zaustavljeno. Aktivnosti zapisnika događaja su pohranjena na platformi i dostupni za upit za 90 dana.
   
    Možete stvoriti i spremati upite za uobičajeni filtri, a zatim Prikvači najvažnije upita na portalu nadzornu ploču da biste uvijek znali događaja koji zadovoljavaju kriterij dogodili.

4. Filtriranje prikaza za grupu za određeni resurs putem prošli tjedan, a zatim kliknite gumb **Spremi** .

    ![Spremite upit zapisnik aktivnosti](./media/monitoring-get-started/monitor-act-log-save.png)

5. Sada kliknite gumb **Prikvači** .

    ![Kliknite pin za zapisnik aktivnosti](./media/monitoring-get-started/monitor-act-log-pin.png)

    Većina prikaza u prikazu mogu biti prikvačena na nadzornu ploču. To vam olakšava stvaranje jedan izvor informacije o radu sa servisom podataka na servisa. 

6. Vratite se na nadzornu ploču. Sada možete vidjeti se prikazuje u nadzorne ploče upita (i broj rezultata). To je korisno ako želite da biste brzo vidjeli najviša-profilu akcije koje ste nedavno pojavio u vašoj pretplati, npr. dodijeljena novo radno mjesto ili na VM je izbrisana.

    ![Zapisnik aktivnosti prikvačene nadzorne ploče](./media/monitoring-get-started/monitor-act-log-db.png)

7. Vratite se na pločici **Monitor** , a zatim kliknite u odjeljku **metriku** . Najprije morate odabrati resurs filtriranja i odabirom pomoću padajućem izborniku mogućnosti pri vrhu na plohu.

    ![Filtriranje resursa za metriku](./media/monitoring-get-started/monitor-met-filter.png)

    Svi Azure resursi šalji [**metriku**](./monitoring-overview-metrics.md). Prikaz objedinjuje sve metriku u jednom oknu koje tako da mogu lako razumjeti kako izvršavate resurse.

8. Kada odaberete resursa, sva dostupna mjerenja će se pojaviti na lijevoj strani u plohu. Možete odjednom grafikona više metriku tako da odaberete metriku i izmijeniti grafikon raspona vrsta i vrijeme. Možete pogledati i svim metričkim upozorenja postavljena na ovaj resurs.

    ![Metričkim plohu](./media/monitoring-get-started/monitor-metric-blade.png)

    > [AZURE.NOTE] Nekoliko mjernih podataka dostupni su samo omogućivanjem [Uvida aplikacije](../application-insights/app-insights-overview.md) i/ili Windows ili Linux Azure Dijagnostika na vašem resursa.

9. Kada ste zadovoljni grafikona, koristite gumb **Prikvači** da biste prikvačili na nadzornu ploču.

10. Povratak plohu **Monitor** te kliknite **dijagnostičke zapisnike**.

    ![Dijagnostičke zapisnike plohu](./media/monitoring-get-started/monitor-diaglogs-blade.png)

    [**Dijagnostičke zapisnike**](monitoring-overview-of-diagnostic-logs.md) su zapisnika čuje *po* resursa koje daju podatke o postupak koji je određeni resurs. Na primjer, logiku aplikacije tijeka rada Zapisnici i mreže sigurnosne grupe pravila mjerača su obje vrste dijagnostičke zapisnike. Ti zapisnici možete biti pohranjene u račun za pohranu, strujanjem koncentratora za događaj i/ili šalje [Zapisnika analize](../log-analytics/log-analytics-overview.md). Prijava analitiku je proizvod upotrebljiva obavještavanje tvrtke Microsoft za napredno pretraživanje i upozorenjem.
   
    Na portalu možete pregledavati i filtrirati popis svih resursa u svoju pretplatu za prepoznavanje ako imaju dijagnostičke zapisnike omogućena.

11. Kliknite resursa u plohu dijagnostičke zapisnike. Ako dijagnostičke zapisnike pohranjuju se na račun za pohranu, vidjet ćete popis svaki sat zapisnika koju možete preuzeti izravno.

    ![Dijagnostičke zapisnike za jedan resurs](./media/monitoring-get-started/monitor-diaglogs-detail.png)

    Možete kliknuti i **Dijagnostičkih postavki**, koji vam omogućuje da biste postavili ili promijenili postavke za arhiviranje s računom za pohranu, strujanje s koncentratorima događaj ili slanje u radni prostor zapisnika analize.

    ![Omogućivanje dijagnostičkih zapisnika](./media/monitoring-get-started/monitor-diaglogs-enable.png)

    Ako postavljate dijagnostičke zapisnike da biste prijava analitiku pa ih možete pretraživati u odjeljku **pretraživanje zapisnika** portala.

12. Pomaknite se do odjeljka **upozorenja** plohu Monitor.

    ![plohu upozorenja za javnosti](./media/monitoring-get-started/monitor-alerts-nopp.png)

    Ovdje možete upravljati sva [**upozorenja**](./monitoring-overview-alerts.md) na Azure resurse. To obuhvaća upozorenja na metriku, aktivnosti zapisnika događaja (u privatnu pretpregled), testira web aplikacije uvida (lokacije) i određene proaktivne Dijagnostika uvida aplikacije. Upozorenja možete pokrenuti slati poruke e-pošte ili HTTP POST webhook URL.
   
13. Kliknite **Dodaj metričkim upozorenja** za stvaranje upozorenja.

    ![Dodavanje metričkim upozorenja](./media/monitoring-get-started/monitor-alerts-add.png)

    Upozorenja možete zatim Prikvači na nadzornu ploču da biste jednostavno vidjeli stanje u bilo kojem trenutku.

14. U odjeljku Monitor uključuje i veze na [Aplikacije uvida](../application-insights/app-insights-overview.md) aplikacije i rješenja za upravljanje [Zapisnika analize](../log-analytics/log-analytics-overview.md) . Te druge Microsoftove proizvode imati precizno Integracija s Azure Monitor.

15. Ako ne koristite uvide aplikacije ili analize zapisnika, vjerojatno ima li Azure Monitor partnerstvo s trenutnom zapisnika nadzora, i upozorenjem proizvoda. Potražite u članku naše [stranice partnere](./monitoring-partners.md) cijeli popis i upute za integraciju.

Tako da slijedite korake u nastavku i prikvačivanje sve relevantne pločice na nadzornu ploču, možete stvoriti potpun prikaze aplikacije i infrastrukture poput ove:

![Nadzorna ploča za Azure monitora](./media/monitoring-get-started/monitor-final-dash.png)

## <a name="next-steps"></a>Daljnji koraci
- Pročitajte [Pregled Azure monitora](./monitoring-overview.md)
