<properties
    pageTitle="Kako nadzirati prostor za pohranu računa | Microsoft Azure"
    description="Saznajte kako praćenje s računom za pohranu u Azure pomoću portala za Azure."
    services="storage"
    documentationCenter=""
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>

# <a name="monitor-a-storage-account-in-the-azure-portal"></a>Praćenje računa za pohranu na portalu za Azure

## <a name="overview"></a>Pregled

Možete nadzirati računa za pohranu s [Portala za Azure](https://portal.azure.com). Kada konfigurirate račun za pohranu za nadzor putem portala sustava Azure prostora za pohranu koristi [Analytics za pohranu](http://msdn.microsoft.com/library/azure/hh343270.aspx) za praćenje metriku za vaš račun i zapisivanje podataka zahtjev.

> [AZURE.NOTE]Dodatne troškove pridružuju Provjera nadzora podataka na [Portalu Azure](https://portal.azure.com). Dodatne informacije potražite u članku <a href="http://msdn.microsoft.com/library/azure/hh360997.aspx">Analytics za pohranu i naplati</a>. <br />

> Azure pohrani trenutno podržava Analytics za pohranu metrika, ali još ne podržava zapisivanje. Možete omogućiti metriku Azure pohrani putem [Portala za Azure](https://portal.azure.com).

> Prostor za pohranu računi s vrstom replikacije od zona suvišnih prostora za pohranu (ZRS) nemaju metriku ili mogućnost bilježenje omogućeno trenutno. 

> Detaljnije vodič o korištenju Analytics za pohranu i druge alate za prepoznavanje, dijagnosticiranje i otklanjanje poteškoća vezanih uz Azure prostora za pohranu, potražite u članku [Monitor, dijagnosticiranje, i otklanjanje poteškoća s spremište na platformi Microsoft Azure](storage-monitoring-diagnosing-troubleshooting.md).


## <a name="how-to-configure-monitoring-for-a-storage-account"></a>Kako: Konfiguriranje nadzora za račun za pohranu

1. [Portal za Azure](https://portal.azure.com)kliknite **prostora za pohranu**, a zatim naziv računa spremišta da biste otvorili na nadzornoj ploči.

2. Kliknite **Konfiguriraj**pa se pomaknite se do postavki **nadzora** za blob, tablice i servise reda čekanja.

    ![MonitoringOptions](./media/storage-monitor-storage-account/Storage_MonitoringOptions.png)

3. U **nadzor**, postavite razinu nadzor i pravila zadržavanja podataka za svaku uslugu:

    -  Da biste postavili razinu nadzora, odaberite nešto od sljedećeg:

      **Minimalna** - prikuplja metriku kao što su ingress/izlazne, dostupnost, Latencija i uspjeh postotaka koji se pridružuje blob, tablice i reda čekanja services.

      **Tekstni** - uz Minimalna metriku prikuplja isti skup metriku za svaku operaciju prostora za pohranu u API servisa za pohranu Azure. Opširno metriku omogućuju bolje analize problema koji se pojavljuju tijekom operacije aplikacije.

      **Isključivanje** – isključuje nadzor. Postojeće podatke za nadzor dosljedan do kraja razdoblje zadržavanja.

- Da biste postavili pravila zadržavanja podataka **zadržavanja (u danima)**, unesite broj dana tijekom kojih će se zadržavati od 1 do 365 dana podaci. Ako ne želite postaviti pravilnika o zadržavanju, unesite nula. Ako nema pravila zadržavanja, je oko Izbriši nadzor podatke. Preporučujemo da postavke pravilnika o zadržavanju na temelju koliko želite zadržati prostora za pohranu analize podataka za vaš račun tako da se stara i Nekorišteni analize podataka moguće je izbrisati sustav na nema trošak.

4. Kada završite nadzora konfiguraciju, kliknite **Spremi**.

Bi trebao počinjati prikazuju podatke na stranici **Monitor** nakon o sata i na nadzornoj ploči za nadzor.

Dok se ne konfigurirate za račun za pohranu za nadzor, nadzor podaci se prikupljaju, a metriku grafikona na stranici nadzorne ploče i **Monitor** bit će prazni.

Nakon što postavite nadzor razine i pravilnika o zadržavanju, možete odabrati koji dostupna metriku za praćenje [Azure Portal](https://portal.azure.com)i koje metrike za crtanje na grafikonima metriku. Zadani skup metriku se prikazuje na svakoj razini nadzora. **Dodavanje metriku** možete koristiti da biste dodali ili uklonili metriku s popisa metriku.

Metriku spremaju se u račun za pohranu u četiri tablice pod nazivom $MetricsTransactionsBlob, $MetricsTransactionsTable, $MetricsTransactionsQueue i $MetricsCapacityBlob. Dodatne informacije potražite u članku [O metrike pohrane analize](http://msdn.microsoft.com/library/azure/hh343258.aspx).


## <a name="how-to-customize-the-dashboard-for-monitoring"></a>Kako: Prilagodba nadzorne ploče za praćenje

Na nadzornoj ploči, možete odabrati do šest metriku za iscrtati na grafikonu metriku s devet dostupna metriku. Za svaki servis (blob, tablice i reda čekanja), dostupne su metriku dostupnost, postotak uspjeh i zahtjeva za Ukupno. Dostupna na nadzornoj ploči metriku su jednake za minimalne ili opširno nadzor.

1. [Portal za Azure](https://portal.azure.com)kliknite **prostora za pohranu**, a zatim naziv računa spremišta da biste otvorili na nadzornoj ploči.

2. Da biste promijenili metriku koje su prikazane na grafikonu, učinite nešto od sljedećeg:

    - Da biste dodali novu metriku grafikon, kliknite boja potvrdni okvir pokraj metričkim zaglavlja u tablici ispod grafikona.

    - Da biste sakrili metriku iscrtane na grafikonu, poništite okvir boja pokraj metričkim zaglavlja.

        ![Monitoring_nmore](./media/storage-monitor-storage-account/storage_Monitoring_nmore.png)

3. Na grafikonu se po zadanom prikazuju trendova koji se prikazuje samo trenutnu vrijednost svakog metriku (mogućnost u **odnosu** na vrhu grafikona). Da biste prikazali os Y da biste vidjeli apsolutne vrijednosti, odaberite **Apsolutna**.

4. Da biste promijenili vremenski raspon grafikona prikazuje metriku pri vrhu grafikona odaberite 6 sati, 24 sata ili 7 dana.


## <a name="how-to-customize-the-monitor-page"></a>Kako: Prilagodba stranice monitora

Na stranici **monitora** možete pogledati potpunog skupa metriku za vaš račun za pohranu.

- Ako je vaš račun za pohranu minimalnog nadzor konfigurirana, metriku kao što su ingress/izlazne, dostupnost, Latencija i postotaka uspjeh se pridružuje s blob, tablice i servise reda čekanja.

- Ako je vaš račun za pohranu opširno nadzor konfigurirana, metriku dostupne su na precizniji razlučivost operacije pojedinačne prostora za pohranu uz zbrajanja razini usluge.

Pomoću sljedećih postupaka možete odabrati koji metrike pohrane da biste prikazali u metriku grafikone i tablice koje se prikazuju na stranici **Monitor** . Te postavke ne utječe na zbirke, zbrajanja i prostora za pohranu podataka na račun za pohranu za nadzor.

## <a name="how-to-add-metrics-to-the-metrics-table"></a>Kako: dodavanje metriku tablicu mjerenja


1. [Portal za Azure](https://portal.azure.com)kliknite **prostora za pohranu**, a zatim naziv računa spremišta da biste otvorili na nadzornoj ploči.

2. Kliknite **Monitor**.

    Otvorit će se stranica **Monitor** . Prema zadanim postavkama tablici metriku prikazuje podskup metrike koji su dostupni za nadzor. Na slici prikazuje zadani prikaz monitora za pohranu račun s opširno nadzor konfigurirana za sve tri servise. Pomoću **Dodavanje metriku** odaberite metrike koju želite nadzirati iz sva dostupna mjerenja.

    ![Monitoring_VerboseDisplay](./media/storage-monitor-storage-account/Storage_Monitoring_VerboseDisplay.png)

    > [AZURE.NOTE] Razmislite o troškovima prilikom odabira metriku. Postoje transakcije i izlazne troškove osvježavanje nadzora prikazuje. Dodatne informacije potražite u članku [Analytics za pohranu i naplati](http://msdn.microsoft.com/library/azure/hh360997.aspx).

3. Kliknite **Dodaj metriku**.

    Zbrajanja mjernih podataka koji su dostupni u minimalne nadzor su pri vrhu popisa. Ako je potvrđen okvir, metriku prikazuje se na popisu metriku.

    ![AddMetricsInitialDisplay](./media/storage-monitor-storage-account/Storage_AddMetrics_InitialDisplay.png)

4. Postavite pokazivač miša na desnoj strani dijaloškog okvira da biste prikazali klizač koje možete povući da biste se pomicali dodatne metriku u prikaz.

    ![AddMetricsScrollbar](./media/storage-monitor-storage-account/Storage_AddMetrics_Scrollbar.png)


5. Kliknite strelicu prema dolje po metrike da biste proširili popis operacije metriku implementaciju ograničen je uključiti. Odaberite svaku operaciju koja želite prikazati u tablici metriku [Azure Portal](https://portal.azure.com).

    Na sljedećoj slici sadrži je proširen metriku POSTOTKA pogreške AUTORIZACIJA.

    ![ExpandCollapse](./media/storage-monitor-storage-account/Storage_AddMetrics_ExpandCollapse.png)


6. Nakon što odaberete metriku za sve servise, kliknite u redu (kvačica) da biste ažurirali nadzora konfiguracije. Odabrani metriku dodaju metriku tablice.

7. Da biste izbrisali metričkih podataka iz tablice, kliknite metriku da biste je odabrali, a zatim **Izbrišite metriku**.

    ![DeleteMetric](./media/storage-monitor-storage-account/Storage_DeleteMetric.png)

## <a name="how-to-customize-the-metrics-chart-on-the-monitor-page"></a>Kako: Prilagodba metriku grafikona na stranici monitora

1. Na stranici **Monitor** računa za pohranu, u tablici metriku odaberite do 6 metriku za iscrtati na grafikonu metriku. Da biste odabrali metričkih podataka, kliknite potvrdni okvir na njene lijeve strane. Da biste uklonili metričkih podataka iz grafikona, poništite potvrdni okvir.

2. Da biste se prebacili na grafikonu između relativnih vrijednosti (Završna vrijednost prikazana samo) i apsolutne vrijednosti (prikaza osi Y), odaberite **relativne** ili **apsolutne** pri vrhu grafikona.

3.  Da biste promijenili vrijeme u rasponu metriku grafikon prikazuje, odaberite **6 sati**, **24 sata**ili **7 dana** pri vrhu grafikona.



## <a name="how-to-configure-logging"></a>Kako: Konfiguriranje zapisivanja

Za svaki servis za pohranu dostupno za vaš račun za pohranu (blob, tablice i reda čekanja), možete spremiti dijagnostički zapisnici zahtjeva za čitanje, pisanje zahtjeve i/ili brisanje zahtjeva za, a možete postaviti pravila zadržavanja podataka za svaki servis.

1. [Portal za Azure](https://portal.azure.com)kliknite **prostora za pohranu**, a zatim naziv računa spremišta da biste otvorili na nadzornoj ploči.

2. Kliknite **Konfiguriraj**pa se pomaknite se do odjeljka **zapisivanje**pomoću strelice dolje na tipkovnici.

    ![Storagelogging](./media/storage-monitor-storage-account/Storage_LoggingOptions.png)


3. Za svaki servis (blob, tablice i reda čekanja) konfigurirajte sljedeće:

    - Vrste zahtjev za prijavu: zahtjevi za čitanje, pisanje zahtjeve i brisanje zahtjeva za.

    - Broj dana koliko će se zadržavati prijavljenih podataka. Unesite nula ako ne želite da biste postavili pravila zadržavanja. Ako ste postavili pravila zadržavanja, je najviše brisanje zapisnike.

4. Kliknite **Spremi**.

Dijagnostički zapisnici spremaju se u spremniku blob pod nazivom $logs na vašem računu za pohranu. Informacije o pristupu spremnik $logs, potražite u članku [O prostora za pohranu analize zapisivanje](http://msdn.microsoft.com/library/azure/hh343262.aspx).
