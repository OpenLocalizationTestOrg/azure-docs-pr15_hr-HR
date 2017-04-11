<properties 
   pageTitle="Performanse sustava Windows i Linux mjerača u prijava analitiku | Microsoft Azure"
   description="Prijava analitiku u analiza performansi na Windows i Linux agenata prikuplja mjerača performansi.  U ovom se članku objašnjava kako konfigurirati skup mjerača performansi za oba prozora i Linux agenata, detalje o one spremaju se u spremištu OMS te kako ih analizirati na portalu OMS."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/27/2016"
   ms.author="bwren" />

# <a name="windows-and-linux-performance-data-sources-in-log-analytics"></a>Windows i Linux performanse podatkovnih izvora u zapisnik Analytics 

Mjerača performansi sustava Windows i Linux omogućuju uvid u performanse hardverske komponente, operacijskih sustava i aplikacije.  Prijava analitiku možete prikupiti mjerača performansi intervalima često korištenih za analizu blizu stvarnom vremenu (NRT) osim Zbrajanje podataka o performansama za analizu dulje termina i izvješćivanje o pogreškama.

![Mjerača performansi](media/log-analytics-data-sources-performance-counters/overview.png)

## <a name="configuring-performance-counters"></a>Konfiguriranje mjerača performansi

Konfiguriranje mjerača performansi na [izborniku Podaci u postavke zapisnika analize](log-analytics-data-sources.md#configuring-data-sources).

Kada konfigurirate Windows ili Linux performanse mjerača za novi radni prostor OMS, imate mogućnost da biste brzo stvoriti nekoliko uobičajenih mjerača.  One navedene su s potvrdni okvir uz svaku.  Provjerite je li provjeravaju se sve mjerača želite stvaranja i pa kliknite **Dodaj mjerača odabrane performansi**.

![Konfiguriranje mjerača performansi sustava Windows](media/log-analytics-data-sources-performance-counters/configure-windows.png)

Slijedite ovaj postupak da biste dodali novi brojač performanse sustava Windows da biste prikupili.

1. Upišite naziv brojač u tekstni okvir u obliku *\counter objekta (instance)*.  Kada počnete upisivati, predstavljanja s odgovarajućom popis uobičajenih mjerača.  S popisa ili upišite jednu od svojih možete odabrati jedan od sljedećih načina brojač.  Možete vratiti i sve instance za određeni brojač navođenjem *object\counter*. 
2. Kliknite **+** ili pritisnite tipku **Enter** da biste dodali brojač na popis.
3. Kada dodate brojač, koristi zadani 10 sekundi **Interval uzorka**.  To možete promijeniti tako da biste veću vrijednost do 1800 sekundi (30 minuta) ako želite da biste smanjili zahtjeve spremišta podataka prikupljenih performansi.
4. Kada završite dodavanjem mjerača, kliknite gumb **Spremi** pri vrhu zaslona da biste spremili konfiguracije.

![Konfiguriranje mjerača Linux performansi](media/log-analytics-data-sources-performance-counters/configure-linux.png)

Slijedite ovaj postupak da biste dodali novi brojač performanse Linux da biste prikupili.

1. Prema zadanim postavkama, sve promjene konfiguracije automatski pomiču se za sve agenata.  Za agente Linux konfiguracijska datoteka se šalje prikupljanje podataka Fluentd.  Ako želite da biste izmijenili ovu datoteku ručno na svaki pojedini agent Linux, poništite okvir *Primijeni ispod konfiguracije Moje strojeva Linux*.
2. Upišite naziv brojač u tekstni okvir u obliku *\counter objekta (instance)*.  Kada počnete upisivati, predstavljanja s odgovarajućom popis uobičajenih mjerača.  S popisa ili upišite jednu od svojih možete odabrati jedan od sljedećih načina brojač.  
2. Kliknite **+** ili pritisnite tipku **Enter** da biste dodali brojač popis drugim mjerača objekta.
3. Sve mjerača za objekt koristiti isti **Interval uzorka**.  Zadana je vrijednost 10 sekundi.  To promijeniti na veću vrijednost do 1800 sekundi (30 minuta) ako želite da biste smanjili zahtjeve spremišta podataka prikupljenih performansi.
4. Kada završite dodavanjem mjerača, kliknite gumb **Spremi** pri vrhu zaslona da biste spremili konfiguracije.

## <a name="data-collection"></a>Prikupljanje podataka

Prijava analitiku prikuplja sve mjerača navedeni performansi njihove intervalu navedeni ogledni na sve agenata koji sadrže counter instaliran.  Podaci se pridružuje i neobrađenog podaci su dostupni u svim prikazima zapisnika pretraživanja za trajanje određen pretplate OMS.


## <a name="performance-record-properties"></a>Performanse svojstvima zapisa

Performanse zapisa vrstu **mjerača performansi** i imate svojstava u tablici u nastavku.

| Svojstvo | Opis |
|:--|:--|
| Računalo         | Računalo na kojem je prikupljenih događaj. |
| CounterName      | Naziv mjerača performansi |
| CounterPath      | Cijeli put brojač u obrascu \\ \\ \<računala >\\object(instance)\\brojač. |
| CounterValue     | Numeričke vrijednosti brojač.  |
| InstanceName     | Naziv instance događaj.  Pražnjenje ako nema instanci. |
| ObjectName       | Naziv objekta performansi |
| SourceSystem  | Vrsta agent trenutku prikupljanja podataka. <br> Povezivanje OpsManager – agent za Windows, bilo izravno ili SCOM <br> Linux – svi agenti Linux  <br> AzureStorage – Azure dijagnostiku |
| TimeGenerated       | Datum i vrijeme je ogledne podatke. |


## <a name="sizing-estimates"></a>Procjenjuje za promjenu veličine

 Grubi Procjena za zbirku određeni brojač intervalima 10 sekundi je otprilike 1 MB po danu po instance.  Možete odrediti preduvjeti za pohranu za određeni brojač pomoću sljedeće formule.

    1 MB x (number of counters) x (number of agents) x (number of instances)

## <a name="log-searches-with-performance-records"></a>Zapisnika pretraživanja sa zapisima performansi

Sljedeća tablica sadrži različite Primjeri zapisnika pretraživanja koji dohvaćaju performanse zapisa.

| Upit | Opis |
|:--|:--|
| Vrsta = mjerača performansi | Svih podataka o performansama |
| Vrsta = mjerača performansi računala = "Mojeracunalo" | Svih podataka o performansama s određenog računala |
| Vrsta = mjerača performansi CounterName = "Trenutni Disk Duljina reda čekanja" | Svih podataka o performansama za određeni brojač |
| Vrsta = mjerača performansi (ObjectName = procesor) CounterName = "% procesor vrijeme" InstanceName = _Total & #124; Izmjerite Avg(Average) kao AVGCPU na računalu | Prosječna procesora na svim računalima |
| Vrsta = mjerača performansi (CounterName = "% procesor vrijeme") & #124;  Izmjerite max(Max) na računalu | Maksimalna procesora na svim računalima |
| Vrsta = mjerača performansi ObjectName = LogicalDisk CounterName = "Trenutni Disk Duljina reda čekanja" računalo = "MyComputerName" & #124; Izmjerite Avg(Average) po InstanceName | Prosječna duljina trenutnu red Disk kroz sve instance određenom računalu |
| Vrsta = mjerača performansi CounterName = "DiskTransfers/sec" & #124; Izmjerite percentile95(Average) na računalu | 95. percentil od Disk prijenosi/Sec na svim računalima |
| Vrsta = mjerača performansi CounterName = InstanceName "% procesor vrijeme" = "_Total" & #124; Izmjerite avg(CounterValue) intervalom računalo 1 sat | Svaki sat prosjek procesora na svim računalima |
| Vrsta = mjerača performansi računala = "Mojeracunalo" CounterName = % * InstanceName = _Total & #124; Izmjerite percentile70(CounterValue) intervalom CounterName 1 sat | Svaki sat 70 percentil svaki % postotka brojač određenog računala |
| Vrsta = mjerača performansi CounterName = InstanceName "% procesor vrijeme" = "_Total" (računalo = "Mojeracunalo") & #124; Izmjerite min(CounterValue), avg(CounterValue), percentile75(CounterValue), max(CounterValue) intervalom računalo 1 sat | Svaki sat prosjeka minimalne, maksimalne i 75 percentil procesora koristi za određeno računalo |

## <a name="viewing-performance-data"></a>Prikaz podataka o performansama

Kada pokrenete zapisnika traženje podataka o performansama, prikaz **zapisnika** prikazuje se po zadanom.  Da biste u obrazac za grafički prikaz podataka, kliknite **metriku**.  Detaljni grafički prikaz, kliknite na **+** pokraj brojač.  

![Sažeti prikaz za mjerenja](media/log-analytics-data-sources-performance-counters/metricscollapsed.png)

Ako je vremenski raspon koji ste odabrali 6 sati ili manje, grafikon se ažurira svakih nekoliko sekundi.  Trenutnim podacima se prikazuje na desnoj strani grafikonu u Svijetloplava.

![Prikaz metriku proširiti s trenutnim podacima](media/log-analytics-data-sources-performance-counters/metricsexpanded.png)

Prikupljanje podataka o performansama u zapisniku pretraživanja potražite [na zahtjev metričkim zbrajanja i vizualizacija u OMS](http://blogs.technet.microsoft.com/msoms/2016/02/26/on-demand-metric-aggregation-and-visualization-in-oms/).

## <a name="next-steps"></a>Daljnji koraci

- Informirajte se o [zapisniku pretraživanja](log-analytics-log-searches.md) radi analize podataka prikupljenih iz izvora podataka i rješenja.  
- Izvoz prikupljene podatke u [Power BI](log-analytics-powerbi.md) radi dodatnih vizualizacija i analize.