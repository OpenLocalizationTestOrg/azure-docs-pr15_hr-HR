<properties
   pageTitle="Što je prijava analitiku? | Microsoft Azure"
   description="Prijava analitiku je servis u operacije upravljanja paket (OMS) koji olakšava prikupljanje i analize podataka u radu sa servisom generira resursa u vašem oblaka i lokalnog okruženja.  Ovaj članak sadrži kratak pregled različitih komponente zapisnika analize i veze na detaljne sadržaj."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/06/2016"
   ms.author="bwren" />

# <a name="what-is-log-analytics"></a>Što je prijava analitiku?
Prijava analitiku je servis u [paket za upravljanje operacije \(OMS\) ](../operations-management-suite/operations-management-suite-overview.md) koji olakšava prikupljanje i analiziranje podataka generira resursa u vašem oblaka i lokalnog okruženja. Pruža u stvarnom vremenu uvide pomoću integriranu pretraživanja i prilagođene nadzornih ploča lako analizirati milijune zapisa preko svih radnih opterećenja i poslužitelji bez obzira na njihov lokacije.


## <a name="log-analytics-components"></a>Prijava analitiku komponente
Pri centar sustava zapisnika analitičkih podataka nije spremište OMS koji se nalazi u oblaku Azure.  Prikupljanja podataka u spremište iz povezanih izvora konfiguriranje izvora podataka i dodavanje rješenja za svoju pretplatu.  Izvori podataka i rješenja će svaki stvoriti različite vrste zapisa koje imaju vlastiti skup svojstava, ali i dalje mogu se analizirati zajedno u upitima u spremište.  Omogućuje korištenje alate i načina za rad s različitim podatke prikupljene putem različitih izvora.


![Spremište OMS](media/log-analytics-overview/overview.png)


Povezani izvori su računala i ostale resurse koji generiranja podatke prikupljene putem zapisnika analize.  To možete obuhvatiti agenata instaliran [Windows](log-analytics-windows-agents.md) i [Linux](log-analytics-linux-agents.md) na računalima na kojima izravno povezati ili agenata na [povezani grupu za upravljanje sustava centar za komponentu Operations Manager](log-analytics-om-agents.md).  Prijava analitiku možete prikupljanje podataka iz [spremišta Azure](log-analytics-azure-storage.md).

[Izvori podataka](log-analytics-data-sources.md) su različite vrste podataka prikupljenih iz svaki povezani izvor.  To obuhvaća događaja i [podataka o performansama](log-analytics-data-sources-performance-counters.md) iz [sustava Windows](log-analytics-data-sources-windows-events.md) i Linux agenata osim izvora kao što su [IIS zapisnika](log-analytics-data-sources-iis-logs.md)i [prilagođeni tekst zapisnika](log-analytics-data-sources-custom-logs.md).  Konfiguriranje svaki izvor podataka koji želite prikupiti i konfiguraciji automatski isporučuju u svaki povezani izvor.


## <a name="analyzing-log-analytics-data"></a>Analiza podataka iz zapisnika Analytics
Većina interakciju s prijava analitiku će se putem portala sustava OMS koji se izvodi u bilo kojem pregledniku, a omogućuje vam pristup konfiguracijske postavke i više alata za analizu i djelovanje na prikupljene podatke.  Na portalu omogućuje korištenje [zapisnika pretraživanja](log-analytics-log-searches.md) koju sastavljanje upita da biste analizirali podatke prikupljene, [nadzornih ploča](log-analytics-dashboards.md) koje možete prilagoditi pomoću grafičkih prikaza priznanje za najboljeg pretraživanja i [rješenja](log-analytics-add-solutions.md) koja sadrže dodatne funkcije i alata za analizu.

![OMS portal](media/log-analytics-overview/portal.png)


Prijava analitiku nudi sintaksu upita za brzo dohvaćanje i Konsolidacija podataka u spremištu.  Možete stvoriti i spremiti [Zapisnika pretraživanja](log-analytics-log-searches.md) radi izravno analize podataka na portalu OMS ili zapisnika pretraživanja pokrenuli automatski stvoriti upozorenje ako rezultate upita naznačite važna uvjet.

![Pretraživanje zapisnika](media/log-analytics-overview/log-search.png)

Da bi se dobilo brzi grafički prikaz stanja cjelokupan okruženju, možete dodati vizualizacija spremljenih zapisnika pretraživanja na [nadzornoj ploči](log-analytics-dashboards.md).   

![Nadzorna ploča](media/log-analytics-overview/dashboard.png)

Da bi se analize podataka izvan prijava analitiku možete izvesti podatke u spremištu OMS u alate kao što je [Power BI](log-analytics-powerbi.md) ili Excel.  Možete koristiti i [API zapisnika pretraživanja](log-analytics-log-search-api.md) da biste sastavili Prilagođena rješenja pod utjecajem zapisnika analize podataka ili za integraciju s drugim sustavima.

## <a name="solutions"></a>Rješenja
Rješenja dodavanje funkcionalnosti zapisnika analize.  Oni prvenstveno pokreću se u oblak i navedite analize podataka prikupljenih u spremištu OMS. Također mogu definirati nove vrste zapisa da biste se prikupljaju koje možete analizirati zapisnika pretraživanja ili putem dodatne korisničko sučelje koje ste dobili od rješenje na nadzornoj ploči OMS.  

![Promjena praćenja rješenja](media/log-analytics-overview/change-tracking.png)


Rješenja su dostupne za različite funkcije, a možete jednostavno pregledati dostupnih rješenja i [dodajte ih u radnom prostoru OMS](log-analytics-add-solutions.md) iz galerije rješenja.  Većina će automatski biti implementirano i početi s radom odmah dok drugim korisnicima biti potrebna neke konfiguracije.

![Galerija rješenja](media/log-analytics-overview/solution-gallery.png)

## <a name="log-analytics-architecture"></a>Prijava analitiku arhitektura
Preduvjeti za implementaciju prijava analitiku su minimalni jer središnje komponente nalaze se u oblaku Azure.  To obuhvaća spremište uz servise koji omogućuju povezivanje i analizirati podatke prikupljene.  Portal možete pristupiti s bilo kojeg preglednika pa bez zahtjeva za klijentski softver.

Morate instalirati agenata na računala sa [sustavom Windows](log-analytics-windows-agents.md) i [Linux](log-analytics-linux-agents.md) , ali postoji bez dodatnih agent potrebne za računala koja se već su članovi na [povezani SCOM upravljanja grupe](log-analytics-om-agents.md).  SCOM agenata će i dalje možete komunicirati s poslužiteljima upravljanja koji će se proslijediti svoje podatke za prijava analitiku.  Neka rješenja kroz potrebno agenata izravno komunicirati s zapisnika analize.  U dokumentaciji za svakog rješenja bit će naveden njegov komunikacijskim potrebama.

[Registracija za analizu zapisnika](log-analytics-get-started.md), ćete stvarati programa OMS radnog prostora.  Radnog prostora možete smatrati okruženju jedinstveni OMS s vlastitom spremište podataka, izvora podataka i rješenja. Više radne prostore možete stvoriti u vašoj pretplati podržava više okruženja kao što su radni i testiranje.

![Prijava analitiku arhitektura](media/log-analytics-overview/architecture.png)


## <a name="next-steps"></a>Daljnji koraci

- [Prijavite se za besplatan račun prijava analitiku](log-analytics-get-started.md) testirati u vlastitu okruženju.
- Prikaz različitih [Izvora podataka](log-analytics-data-sources.md) dostupnih za prikupljanje podataka u spremište OMS.
- [Pregled dostupnih rješenja u galeriju rješenja](log-analytics-add-solutions.md) za dodavanje funkcionalnosti zapisnika analize.
