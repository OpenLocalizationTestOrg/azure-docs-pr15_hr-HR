<properties
    pageTitle="Real-Time-stat u Azure CDN | Microsoft Azure"
    description="U stvarnom vremenu Statistika sadrži podatke u stvarnom vremenu o performansama Azure CDN tijekom izlaganja sadržaja da biste klijentima."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>

# <a name="real-time-stats-in-microsoft-azure-cdn"></a>U stvarnom vremenu stat u Microsoft Azure CDN

[AZURE.INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Pregled

Ovaj dokument objašnjava Statistika u stvarnom vremenu u programu Microsoft Azure CDN.  Ta je funkcija sadrži podatke u stvarnom vremenu, kao što je propusnost, statusi predmemorije i Istodobni veze u svoj profil CDN tijekom izlaganja sadržaja da biste klijentima. Time se omogućuje neprekidnog nadzora o stanju servisa u bilo kojem trenutku, uključujući Idi live događaje.

Dostupne su sljedeće grafikona:

* [Propusnosti](#bandwidth)
* [Šifre statusa](#status-codes)
* [Statusi predmemorije](#cache-statuses)
* [Veze](#connections)


## <a name="accessing-real-time-stats"></a>Pristupanju stat u stvarnom vremenu

1. [Portal za Azure](https://portal.azure.com)pronađite CDN profila.

    ![Plohu CDN profila](./media/cdn-real-time-stats/cdn-profile-blade.png)

2. Iz profila plohu CDN kliknite gumb **Upravljanje** .

    ![Gumb Upravljanje plohu CDN profila](./media/cdn-real-time-stats/cdn-manage-btn.png)

    Otvorit će se na portal za upravljanje CDN.

3. Zadržite pokazivač na kartici **analize** , a zatim zadržite pokazivač na Potpaleta **Statistika u stvarnom vremenu** .  Kliknite na **HTTP velike objektu**.

    ![Portal za upravljanje CDN](./media/cdn-real-time-stats/cdn-premium-portal.png)

    Prikazuju se u stvarnom vremenu Statistika grafikona.
    
Svaki grafova prikazuje u stvarnom vremenu Statistika za odabrani vremenskog razdoblja, počevši prilikom učitavanja stranice.  Na grafikone automatski se ažuriraju svakih nekoliko sekundi.  Gumb **Osvježi Graph** , ako postoje, će očistiti graph, nakon čega prikazat će samo odabrane podatke.

## <a name="bandwidth"></a>Propusnosti

![Graph propusnosti](./media/cdn-real-time-stats/cdn-bandwidth.png)

Na grafikonu **propusnosti** prikazuju iznos propusnost za postojeću platformu iznad odabrane razdobljem. Osjenčani dio grafikon označava korištenja propusnosti. Točan iznos propusnost koje se trenutačno koristi prikazat će se neposredno ispod linijski grafikon.

## <a name="status-codes"></a>Kodovi stanja

![Grafikona Šifra stanja](./media/cdn-real-time-stats/cdn-status-codes.png)

**Kodovi stanja** grafikonu upućuje na to koliko često su određene HTTP odgovor kodovima pojavljivanja iznad odabrane razdobljem.

> [AZURE.TIP]  Opis svake mogućnosti za HTTP statusa kod, potražite u članku [Azure CDN HTTP kodovima stanja](https://msdn.microsoft.com/library/mt759238.aspx).

Popis HTTP kodovima stanja prikazuje se izravno iznad na grafikonu. Ovaj popis označava svaki status kod koji se mogu uključiti u grafikonu redak i trenutni broj pojavljivanja sekundi za taj Šifra stanja. Prema zadanim postavkama, prikazuje se redak za svaki od tih šifri statusa u grafikonu. Međutim, možete nadzirati samo kodovi stanja koji imaju posebno Značajnost za konfiguraciju CDN. Da biste to učinili, provjerite kodove za željeni status i poništite ostale mogućnosti, a zatim kliknite **Osvježi grafikonu**. 

Privremeno možete sakriti evidentirane podatke za Šifra statusa.  Na legendi neposredno ispod na grafikonu, kliknite Šifra stanja koji želite sakriti. Šifra stanja bit će odmah skriven na grafikonu. Klikom na tom Šifra stanja uzrokovat će tu mogućnost da biste ponovno prikaže.

## <a name="cache-statuses"></a>Statusi predmemorije

![Predmemorija statusi grafikonu](./media/cdn-real-time-stats/cdn-cache-status.png)

Graph **Predmemorije statusi** upućuje na to koliko često se određene vrste predmemorije statusi pojavljivanja iznad odabrane razdobljem. 

> [AZURE.TIP]  Opis svake mogućnosti za predmemorije statusa kod, potražite u članku [Kodovi stanja za Azure CDN predmemoriju](https://msdn.microsoft.com/library/mt759237.aspx).

Popis kodova stanja predmemorije prikazuje se izravno iznad na grafikonu. Ovaj popis označava svaki status kod koji se mogu uključiti u grafikonu retka i trenutni broj pojavljivanja sekundi za taj Šifra stanja. Prema zadanim postavkama, prikazuje se redak za svaki od tih šifri statusa u grafikonu. Međutim, možete nadzirati samo kodovi stanja koji imaju posebno Značajnost za konfiguraciju CDN. Da biste to učinili, provjerite kodove za željeni status i poništite ostale mogućnosti, a zatim kliknite **Osvježi grafikonu**. 

Privremeno možete sakriti prijavljenih podatke za Šifra statusa.  Na legendi neposredno ispod na grafikonu, kliknite Šifra stanja koji želite sakriti. Šifra stanja bit će odmah skriven na grafikonu. Klikom na tom Šifra stanja uzrokovat će tu mogućnost da biste ponovno prikaže.

## <a name="connections"></a>Veze

![Graph veze](./media/cdn-real-time-stats/cdn-connections.png)

U ovom grafikonu upućuje na to koliko veze su uspostavljena edge poslužitelji. Svaki zahtjev za sredstvo koje prolazi kroz naš CDN rezultati u vezu.

## <a name="next-steps"></a>Daljnji koraci

- Primanje obavijesti o s [upozorenja u stvarnom vremenu u Azure CDN](cdn-real-time-alerts.md)
- Pristupili [Dodatna izvješća HTTP](cdn-advanced-http-reports.md)
- Analiza [uzoraka korištenja](cdn-analyze-usage-patterns.md)

