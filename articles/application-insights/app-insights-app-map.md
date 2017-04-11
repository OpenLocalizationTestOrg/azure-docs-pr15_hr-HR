<properties 
    pageTitle="Kartu aplikaciju u aplikacije uvida | Microsoft Azure" 
    description="Vizualnu prezentaciju međuzavisnosti komponente aplikacije koje su označene KPI-ja i upozorenja." 
    services="application-insights" 
    documentationCenter=""
    authors="SoubhagyaDash" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/15/2016" 
    ms.author="awills"/>
 
# <a name="application-map-in-application-insights"></a>Kartu aplikaciju u aplikacije uvida

U [Uvida aplikacije za Visual Studio](app-insights-overview.md), karte aplikacija je vizualni izgled odnose ovisnosti komponenti aplikacije. Svaku od njih prikazuje KPI-ja kao što su opterećenja, performanse, pogrešaka i upozorenja radi pronalaženje bilo koju komponentu uzrokuju problem s performansama ili nije uspjelo. Možete klikati iz sve komponente za detaljnije Dijagnostika i jedno i drugo uvida aplikacije i, ako je aplikacija koristi Azure usluge – Azure Dijagnostika, kao što je Savjetnik za baze podataka SQL preporuke.

Kao što su drugim grafikonima možete prikvačiti na kartu aplikacije Azure nadzorne ploče, pri čemu je potpuno funkcionalni. 

## <a name="open-the-application-map"></a>Otvorite kartu aplikacije

Otvorite kartu iz plohu pregled aplikacije:

![Otvorite kartu aplikacije](./media/app-insights-app-map/01.png)

![Karta aplikacije](./media/app-insights-app-map/02.png)

Prikazuje se karte:

* Testira dostupnosti
* Klijentske strane komponente (nadzirati JavaScript SDK)
* Komponenta strani poslužitelja
* Zavisnosti komponenti postupak klijentske i poslužiteljske

Možete proširivati i sažimati ovisnost vezu grupe:

![Gumb Sažmi okno osoba](./media/app-insights-app-map/03.png)
 
Ako imate velik broj ovisnosti jedne vrste (SQL, HTTP itd.), možda prikažu grupirane. 


![Grupirani ovisnosti](./media/app-insights-app-map/03-2.png)
 
 
## <a name="spot-problems"></a>Spot problema

Svaki čvor ima odgovarajuće pokazatelji, kao što su stope opterećenja, performanse i pogreške za tu komponentu. 

Ikona upozorenje istaknite moguće probleme. Narančasta upozorenje znači da postoje pogreške u zahtjevima, prikaza stranice ili ovisnost pozive. Crvena znači brzina pogreške iznad 5%.


![Ikona pogreške](./media/app-insights-app-map/04.png)

 
Aktivna i upozorenja Prikaži gore: 


![aktivni upozorenja](./media/app-insights-app-map/05.png)
 
Ako koristite SQL Azure, postoji ikonu koja se prikazuje kada postoje preporuke o kako možete poboljšati performanse. 


![Preporuke za Azure](./media/app-insights-app-map/06.png)

Kliknite bilo koju ikonu da biste vidjeli dodatne detalje:


![preporuke za Azure](./media/app-insights-app-map/07.png)
 
 
## <a name="diagnostic-click-through"></a>Kliknite Dijagnostika putem

Svaki od čvorove na karti nudi ciljnim kliknite kroz za dijagnostiku. Mogućnosti razlikuju se ovisno o vrsti čvor.

![Mogućnosti poslužitelja](./media/app-insights-app-map/09.png)

 
Za komponente koje se nalaze u Azure, mogućnosti uključuju izravne veze na njih.


## <a name="filters-and-time-range"></a>Filtri i vremenskog raspona

Prema zadanim postavkama, karte navedene sve podatke koje su dostupne za odabrani vremenski raspon. No možete filtrirati da uvrstite samo određene operacije imena ili ovisnosti.

* Naziv operacije: To uključuje prikaza stranice i vrste zahtjev strani poslužitelja. Uz tu se mogućnost karte prikazuje KPI-JA na čvor strani poslužitelja/klijent za samo odabrane operacije. Prikazuje ovisnosti naziva u kontekstu te određene operacije.
* Ovisnost osnovni naziv: To uključuje zavisnosti na strani preglednika AJAX-a i zavisnosti na strani poslužitelja. Ako izvješće prilagođene ovisnost telemetrijskih s TrackDependency API-jem, oni također će prikazati ovdje. Možete odabrati ovisnosti da bi se prikazala na karti. Napominjemo da trenutku, to će ne filtrira zahtjeve strani poslužitelja ili prikazi stranice na strani klijenta.


![Postavite filtre](./media/app-insights-app-map/11.png)

 
 
## <a name="save-filters"></a>Spremanje filtara

Da biste spremili filtre koje ste primijenili, prikvačiti filtrirani prikaz na [nadzornoj ploči](app-insights-dashboards.md).


![Prikvači na nadzornoj ploči](./media/app-insights-app-map/12.png)
 


## <a name="feedback"></a>Povratne informacije

Imajte na [Slanje povratnih informacija putem mogućnosti portala povratne informacije](app-insights-get-dev-support.md).


![Slika MapLink-1](./media/app-insights-app-map/13.png)


