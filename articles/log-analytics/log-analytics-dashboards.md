<properties
    pageTitle="Stvaranje prilagođene nadzorne ploče u prijava analitiku | Microsoft Azure"
    description="Ovaj ćete vodič jednostavnije objasniti kako nadzornih ploča analize zapisnika možete vizualizirati sve pretraživanjima spremljenih zapisnika daje jednu leće za prikaz vaše okruženje."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="create-a-custom-dashboard-in-log-analytics"></a>Stvaranje prilagođene nadzorne ploče u zapisnik Analytics

Ovaj ćete vodič jednostavnije objasniti kako prijava analitiku nadzornih ploča možete vizualizirati sve pretraživanjima spremljenih zapisnika daje jednu leće za prikaz vaše okruženje.

![Ogledna nadzorna ploča](./media/log-analytics-dashboards/oms-dashboards-example-dash.png)

Sve prilagođene nadzorne koje ste stvorili na portalu OMS dostupne su i u mobilnu aplikaciju OMS. Pogledajte sljedeće stranice dodatne informacije o aplikacijama.

- [OMS mobilne aplikacije iz Microsoft Store](http://www.windowsphone.com/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865)
- [OMS mobilne aplikacije iz Apple iTunes](https://itunes.apple.com/app/microsoft-operations-management/id1042424859?mt=8)

![Mobilni nadzorne ploče](./media/log-analytics-dashboards/oms-search-mobile.png)

## <a name="how-do-i-create-my-dashboard"></a>Kako stvoriti nadzornoj ploči?

Da biste započeli, idite na stranici pregled OMS. Vidjet ćete pločicu **Moje nadzorne ploče** na lijevoj strani. Kliknite da biste kroz razine naniže u nadzorne ploče.

![Pregled](./media/log-analytics-dashboards/oms-dashboards-overview.png)


## <a name="adding-a-tile"></a>Dodavanje pločicu

Na nadzornim pločama, pločica se pokreće spremljenih zapisnika pretraživanja. OMS isporučuje se s mnogo unaprijed unijeli spremljene zapisnika pretraživanja, da biste mogli započeti odmah. Pomoću sljedećih koraka koji strukture kako ćete to učiniti.

U Moje nadzorne ploče prikazu, jednostavno kliknite **Prilagodi** da biste unijeli prilagoditi način rada.

![Slikovnih](./media/log-analytics-dashboards/oms-dashboards-pictorial01.png)

 Na ploči koji će se otvoriti na desnoj strani stranice prikazuju se svi radni prostor spremljenih zapisnika pretraživanja. Da biste vizualizacija spremljenih zapisnika pretraživanja kao pločicu, pokazivač miša postavite iznad spremljeno pretraživanje, a zatim kliknite znak **plus** .

![Dodavanje pločica 1](./media/log-analytics-dashboards/oms-dashboards-pictorial02.png)

Kada kliknete znak **plus** , novi pločica se prikazuje u prikazu Moje nadzorne ploče.

![Dodavanje pločica 2](./media/log-analytics-dashboards/oms-dashboards-pictorial03.png)


## <a name="edit-a-tile"></a>Uređivanje pločice

U Moje nadzorne ploče prikazu, jednostavno kliknite **Prilagodi** da biste unijeli prilagoditi način rada. Kliknite pločicu koju želite urediti. U desnom oknu promjene da biste uredili, a omogućuje odabir mogućnosti:

![Uređivanje pločice](./media/log-analytics-dashboards/oms-dashboards-pictorial04.png)

![Uređivanje pločice](./media/log-analytics-dashboards/oms-dashboards-pictorial05.png)

### <a name="tile-visualizations"></a>Vizualizacija pločice#
Postoje tri vrste vizualizacija pločice da biste odabrali neku:

|Vrsta grafikona|Funkcija|
|---|---|
|![Trakasti grafikon](./media/log-analytics-dashboards/oms-dashboards-bar-chart.png)|Prikazuje vremenske crte za rezultate pretraživanja spremljenih zapisnika na trakastom grafikonu ili popisa rezultata prema polju ovisno o ako pretraživanjem zapisnika objedinjuje rezultata prema polju ili ne.
|![mjerenja](./media/log-analytics-dashboards/oms-dashboards-metric.png)|Prikazuje vaše pristupa rezultat zbroja zapisnika pretraživanja kao broj na pločici. Metričkim pločice dopušta postavljanje prag koji će istaknuti pločicu kad zbirka dostigne praga.|
|![redak](./media/log-analytics-dashboards/oms-dashboards-line.png)|Prikazuje vremenske crte spremljenih zapisnika pretraživanja rezultat pristupa s vrijednostima kao linijski grafikon.|

### <a name="threshold"></a>Prag
Možete stvoriti praga na pločici pomoću metričkim vizualizacije. Na odaberite da biste stvorili vrijednosti praga na pločici. Odaberite želite li da biste istaknuli pločicu kada je vrijednost je iznad ili ispod prag koji ste odabrali, a zatim postavite vrijednost praga.

## <a name="organizing-the-dashboard"></a>Organiziranje nadzorne ploče
Organiziranje nadzorne ploče, prijeđite na prikaz Moje nadzorne ploče, a zatim **Prilagodi** da biste unijeli prilagoditi način rada. Kliknite i povucite pločicu koju želite premjestiti pa ga onda pomaknite na mjesto gdje želite da se pločicu biti.

![Organiziranje nadzorne ploče](./media/log-analytics-dashboards/oms-dashboards-organize.png)

## <a name="remove-a-tile"></a>Uklanjanje pločicu
Da biste uklonili pločicu, idite u prikazu Moje nadzorne ploče te kliknite **Prilagodi** da biste unijeli prilagoditi način rada. Odaberite pločicu koju želite ukloniti, a zatim u desnom oknu odaberite **Uklanjanje pločica**.

![Uklanjanje pločicu](./media/log-analytics-dashboards/oms-dashboards-remove-tile.png)

## <a name="next-steps"></a>Daljnji koraci

- Stvaranje [upozorenja](log-analytics-alerts.md) u zapisnik Analytics za generiranje obavijesti i remediate problema.
