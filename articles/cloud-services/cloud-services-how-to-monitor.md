<properties 
    pageTitle="Upute za praćenje servis u oblaku | Microsoft Azure" 
    description="Saznajte kako pomoću portala za Azure klasični praćenje servise u oblaku." 
    services="cloud-services" 
    documentationCenter="" 
    authors="rboucher" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/04/2015" 
    ms.author="robb"/>


# <a name="how-to-monitor-cloud-services"></a>Upute za praćenje servise u Oblaku

[AZURE.INCLUDE [disclaimer](../../includes/disclaimer.md)]

Možete nadzirati `key` performanse metriku oblaka servisa Azure klasični portalu. Možete postaviti razinu nadzor minimalne i opširno za svaku ulogu servisa, a možete prilagoditi nadzora prikazuje. Opširno nadzora podaci se pohranjuju na račun za pohranu koji mogu pristupiti izvan portalu. 

Nadzor koji se prikazuje na portalu za Azure klasični su izrazito može konfigurirati. Možete odabrati metrike koju želite nadzirati metriku popisu na stranici **Monitor** , a možete odabrati koji metriku za iscrtati na grafikonima metriku na stranici **monitoru** i na nadzornoj ploči. 

## <a name="concepts"></a>Koncepti

Prema zadanim postavkama minimalnog nadzor je namijenjeno nove servise u oblaku pomoću mjerača performansi prikupili s operacijskim sustavom glavno računalo za uloge instance (virtualnim strojevima). Minimalna metriku ograničeni su na procesora postotak, podataka u, podataka izvan, propusnost diska čitanje i pisanje propusnost diska. Konfiguriranjem opširno nadzor možete primati dodatne metriku na temelju podataka o performansama unutar virtualnim strojevima (uloga instance). Opširno metriku omogućuju bolje analize problema koji se pojavljuju tijekom operacije aplikacije.

Prema zadanim postavkama podataka o performansama brojač iz uloge instance uzorkovanja i prenesene instancu uloga u vremenskim razmacima 3 minute. Kada omogućite opširno praćenje podataka o brojač neobrađenog performansama se pridružuje za svaku instancu uloga i kroz sve instance uloge za svaku ulogu intervalima od pet minuta, 1 sat i 12 sati. Skupne podatke očišćene je nakon 10 dana.

Kada omogućite opširno nadzor skupne podatke za nadzor su spremljeni u tablicama na vašem računu za pohranu. Da biste omogućili opširno nadzor uloge, morate konfigurirati niza za povezivanje dijagnostiku koja je povezana s računom za pohranu. Prostor za pohranu za različite račune možete koristiti za različite uloge.

Imajte na umu da Omogućivanje opširno nadzor povećava troškove prostora za pohranu vezane uz spremanje podataka, prijenos podataka i transakcije prostora za pohranu. Minimalna nadzor potreban račun za pohranu. Podatke za metriku prikazat će se na razini minimalnog nadzora se pohranjuju u svoj račun za pohranu, čak i ako postavite razinu nadzora na opširno.


## <a name="how-to-configure-monitoring-for-cloud-services"></a>Kako: Konfiguriranje nadzora za servise u oblaku

Koristite sljedeće postupke za konfiguriranje opširno ili minimalnog nadzor Azure klasični portalu. 

### <a name="before-you-begin"></a>Prije početka

- Stvorite račun za pohranu za spremanje podataka za nadzor. Prostor za pohranu za različite račune možete koristiti za različite uloge. Dodatne informacije potražite u članku pomoć za **Račune za pohranu**ili potražite u članku [upute za stvaranje računa za pohranu](/manage/services/storage/how-to-create-a-storage-account/).

- Omogućivanje Azure Dijagnostika za svoje uloge servisa oblaka. Potražite u članku [Konfiguriranje Dijagnostika za servise u Oblaku](https://msdn.microsoft.com/library/azure/dn186185.aspx#BK_EnableBefore).

Provjerite je li niz za povezivanje Dijagnostika koje su prisutne u konfiguraciji uloga. Ne možete uključiti opširno nadzor dok omogućiti i dijagnostiku Azure i sadrže niz za povezivanje Dijagnostika u konfiguraciji uloga.   

> [AZURE.NOTE] Projekti ciljanja Azure SDK 2,5 automatski u niste uključili niz za povezivanje Dijagnostika predložak projekta. Za ove projekte ćete morati ručno dodati niz za povezivanje Dijagnostika konfiguraciju uloga.

**Ručno dodavanje niz za povezivanje Dijagnostika konfiguracije uloga**

1. Otvaranje projekta u Oblaku u Visual Studio
2. Dvokliknite na **ulogu** da biste otvorili dizajner uloge, a zatim odaberite karticu **Postavke**
3. Potražite postavku pod nazivom **Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString**. 
4. Ako nema tu postavku pa kliknite **Dodavanje postavka** gumb da biste dodali konfiguracije i promijeniti vrstu za Nova postavka **ConnectionString**
5. Postavite vrijednost za niz za povezivanje s klikom na gumb **…** . Otvorit će se dijaloški okvir možete odabrati na račun za pohranu.

    ![Postavke za Visual Studio](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioDiagnosticsConnectionString.png)

### <a name="to-change-the-monitoring-level-to-verbose-or-minimal"></a>Da biste promijenili razinu nadzora opširno ili minimalan

1. [Azure klasični portal](https://manage.windowsazure.com/)otvorite stranicu **Konfiguriranje** servisa implementacije oblaka.

2. **Razine**, kliknite **tekstni** ili **najmanje**. 

3. Kliknite **Spremi**.

Kada uključite opširno nadzor bi trebao počinjati prikazuju nadzora podataka na portalu za Azure klasični unutar sat.

Podataka neobrađenog performanse i skupne podatke o praćenju spremaju se u račun za pohranu u tablicama pogodni po ID za uvođenje uloge. 

## <a name="how-to-receive-alerts-for-cloud-service-metrics"></a>Kako: primati upozorenja za metriku servisa oblaka

Možete primati upozorenja na temelju na servis u oblaku metriku za nadzor. Na stranici portala za Azure klasični **Management Services** možete stvoriti pravilo za pokretanje upozorenja kada odaberete metriku dosegne vrijednost koju navedete. Možete odabrati i da bi e-pošte poslane kada se pokrene upozorenje. Dodatne informacije potražite u članku [Kako: primanje upozorenja i upravljanje upozorenja pravila u Azure](http://go.microsoft.com/fwlink/?LinkId=309356).

## <a name="how-to-add-metrics-to-the-metrics-table"></a>Kako: dodavanje metriku tablicu mjerenja

1. [Azure klasični portal](http://manage.windowsazure.com/)otvorite stranicu **Monitor** servisa u oblaku.

    Prema zadanim postavkama tablici metriku prikazuje podskup dostupna metriku. Na slici prikazan zadani opširno metriku za servise u oblaku, što je ograničeno na performanse brojač Memory\Available MBytes s podacima koji se pridružuje na razini ulogu. **Dodavanje metriku** koristite da biste odabrali dodatne zbrajanja i uloga razinu metriku za praćenje na portalu za Azure klasični.

    ![Opširno prikaza](./media/cloud-services-how-to-monitor/CloudServices_DefaultVerboseDisplay.png)
 
2. Da biste dodali metriku metriku tablice:

    1. Kliknite **Dodavanje metriku** da biste otvorili **Odaberite metriku**prikazano u nastavku.

        Da biste prikazali mogućnosti koje su dostupne proširen je metriku prvi dostupan. Za svaki metriku gornji mogućnost prikazuje skupne podatke nadzora za sve uloge. Osim toga, možete odabrati pojedinačne uloge da biste prikazali podataka.

        ![Dodavanje mjerenja](./media/cloud-services-how-to-monitor/CloudServices_AddMetrics.png)

    2. Da biste odabrali metriku za prikaz

        - Kliknite strelicu prema dolje po metriku da biste proširili nadzora mogućnosti.
        - Potvrdite okvire za svaku nadzora mogućnost koju želite prikazati.

        Do 50 metriku možete prikazati u tablici metriku.

        > [AZURE.TIP] U opširno nadzor popisu metriku mogu sadržavati desetke metriku. Da biste prikazali klizača, postavite pokazivač miša na desnoj strani dijaloškog okvira. Da biste filtrirali popis, kliknite ikonu pretraživanje pa unesite tekst u okvir za pretraživanje kao što je prikazano u nastavku.
    
        ![Dodaj pretraživanje mjerenja](./media/cloud-services-how-to-monitor/CloudServices_AddMetrics_Search.png)


3. Kada završite s odabirom metriku, kliknite u redu (kvačica).

    Odabrane metriku dodaju metriku tablice, kao što je prikazano u nastavku.

    ![monitor mjerenja](./media/cloud-services-how-to-monitor/CloudServices_Monitor_UpdatedMetrics.png)

 
4. Da biste izbrisali metričkih podataka iz tablice metriku, kliknite metrički da biste je odabrali, a zatim **Izbrišite metrika**. (Samo vidite **Izbrisati metrički** kad imate metrički odabrana.)

### <a name="to-add-custom-metrics-to-the-metrics-table"></a>Da biste dodali prilagođenu metriku tablicu mjerenja

**Tekstni** nadzor razine nalaze se popis zadani metriku koja možete nadzirati na portalu. Uz to možete nadzirati prilagođene metriku ni mjerača performansi definira aplikaciju putem portala sustava.

Sljedeći koraci pretpostavlja uključile **tekstni** nadzor razine i imaju konfiguriranje aplikacija za prikupljanje i prijenos mjerača prilagođene performansi. 

Da biste prikazali mjerača prilagođene performansi na portalu morate ažurirati konfiguraciju u wad kontrola spremnik:
 
1. Otvorite blob wad kontrola spremnik na vašem računu Dijagnostika za pohranu. Koristite Visual Studio ili prostora za pohranu explorer da biste to učinili.

    ![Explorer poslužitelja za Visual Studio](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioBlobExplorer.png)

2. Pronađite put blob pomoću uzorak **Naziv uloge/DeploymentId/RoleInstance** da biste pronašli konfiguraciju vaša uloga instance. 

    ![Explorer prostora za pohranu za Visual Studio](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioStorage.png)
3. Preuzmite konfiguracijska datoteka za vaše instancu ulogu i ažurirati da biste obuhvatili sve mjerača prilagođene performansi. Na primjer za praćenje *na disku napišite bajtova/sec* *pogon C* dodajte sljedeće u odjeljku **PerformanceCounters\Subscriptions** čvor

    ```xml
    <PerformanceCounterConfiguration>
    <CounterSpecifier>\LogicalDisk(C:)\Disk Write Bytes/sec</CounterSpecifier>
    <SampleRateInSeconds>180</SampleRateInSeconds>
    </PerformanceCounterConfiguration>
    ```
4. Spremite promjene i prenesite datoteku konfiguracije natrag na isto mjesto prebrisivanje postojeću datoteku u blob-om.
5. Uključivanje ili isključivanje opširno način u konfiguraciji Azure klasični portala za. Ako ste u načinu rada za opširno već imate da biste se prebacivali minimalni i ponovno opširno.
6. Prilagođeni performanse brojač sada bit će dostupni u dijaloškom okviru **Dodavanje metriku** . 

## <a name="how-to-customize-the-metrics-chart"></a>Kako: Prilagodba grafikona mjerenja

1. Odabir do 6 metriku za iscrtati na grafikonu metriku tablice metriku. Da biste odabrali metrike, kliknite potvrdni okvir na njene lijeve strane. Da biste uklonili metrike metriku grafikona, poništite njegov potvrdni okvir u tablici metriku.

    Označavanja mjernih podataka u tablici metriku metriku dodaju se metriku grafikona. Na zaslonu usko, na padajućem popisu **n više** sadrži metričkim zaglavlja koji ne stane na zaslon.

 
2. Za prebacivanje između relativnih vrijednosti (Završna vrijednost samo za svaki metriku) i apsolutne vrijednosti (prikaza osi Y), odaberite zavisne ili apsolutna pri vrhu grafikona.

    ![Relativne i apsolutne](./media/cloud-services-how-to-monitor/CloudServices_Monitor_RelativeAbsolute.png)

3. Da biste promijenili vremenski raspon grafikona prikazuje metriku odaberite 1 sat, 24 sata ili 7 dana pri vrhu grafikona.

    ![Razdoblje za prikaz monitora](./media/cloud-services-how-to-monitor/CloudServices_Monitor_DisplayPeriod.png)

    Na grafikonu metriku nadzorne ploče razlikuje se način za iscrtavanje metriku. Postoji Standardni skup metriku i metriku su dodati ili ukloniti tako da odaberete metričkim zaglavlja.

### <a name="to-customize-the-metrics-chart-on-the-dashboard"></a>Da biste prilagodili metriku grafikona na nadzornoj ploči

1. Otvorite nadzorna ploča za servis u oblaku.

2. Dodavanje i uklanjanje metriku iz grafikona:

    - Da biste iscrtali novu metriku, odaberite potvrdni okvir za mjerenje u zaglavljima grafikon. Na zaslonu usko, kliknite strelicu prema dolje po ** *n*??metrics** da biste iscrtali metričkih podataka u područje zaglavlja grafikona ne može prikazati.

    - Da biste izbrisali metriku iscrtane na grafikonu, poništite potvrdni okvir po njegovo zaglavlje.

3. Prebacivanje između **relativnih** i **apsolutnih** prikazuje.

4. Odaberite 1 sat, 24 sata ili 7 dana podataka za prikaz.

## <a name="how-to-access-verbose-monitoring-data-outside-the-azure-classic-portal"></a>Kako: Access opširno praćenje podataka izvan Azure klasični portal

Opširno nadzora podaci se pohranjuju u tablicama na računima za pohranu koji ste naveli za svaku ulogu. Za svaki servis implementaciji oblaka šest tablica stvaraju se uloge. Dvije tablice se stvaraju za svaku (5 minuta, 1 sat i 12 sati). Jedan od ovih tablica pohranjuje zbrajanja uloga razinom; druge tablice pohranjuje zbrajanja instanci uloge. 

Nazivi tablica imaju sljedećem obliku:

```
WAD*deploymentID*PT*aggregation_interval*[R|RI]Table
```

pri čemu je:

- *deploymentID* je GUID dodijeljene uvođenje servisa oblaka

- *aggregation_interval* = 5 M, 1 H ili 12 H

- uloga razinom zbrajanja = R

- zbirne funkcije uloga instanci = RI

Na primjer, sljedeće tablice želite spremiti opširno nadzora podataka pridružuje u vremenskim razmacima 1 sat:

```
WAD8b7c4233802442b494d0cc9eb9d8dd9fPT1HRTable (hourly aggregations for the role)

WAD8b7c4233802442b494d0cc9eb9d8dd9fPT1HRITable (hourly aggregations for role instances)
```
