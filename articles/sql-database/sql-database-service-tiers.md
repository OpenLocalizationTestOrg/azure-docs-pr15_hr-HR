<properties
    pageTitle="Mogućnosti & performanse baze podataka SQL: servisa razine | Microsoft Azure"
    description="Usporedite baze podataka SQL performanse i tvrtke continuity značajke razine servisa da biste saldo trošak i mogućnostima koje skaliranja."
    keywords="mogućnosti za baze podataka, performanse baze podataka"
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor="CarlRabeler"/>

<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="08/10/2016"
    ms.author="carlrab"/>

# <a name="sql-database-options-and-performance-understand-whats-available-in-each-service-tier"></a>Mogućnosti SQL baze podataka i performanse: objašnjenje što dostupna je u sloju svaki servis

[Baze podataka SQL Azure](sql-database-technical-overview.md) nudi tri servisa razine s više razina performansi za rukovanje različitih radnih opterećenja. Svaka razina performanse nudi rastuće skup resursa namijenjene izlaganje smanjuju propusnost. Možete upravljati svaku bazu podataka u vlastitom [sloju servisa](sql-database-service-tiers.md#standalone-database-service-tiers-and-performance-levels) vlastitu razinom performansi. Možete upravljati i više baza podataka u [elastic skup](sql-database-service-tiers.md#elastic-pool-service-tiers-and-performance-in-edtus) s zajednički skup resursa. Dostupni resursi za baze podataka samostalni su izražena pomoću jedinice transakcije baze podataka (DTUs) i elastic grupe pomoću elastic DTUs ili eDTUs. Dodatne informacije o DTUs i eDTUs, potražite u članku [što je na DTU](sql-database-what-is-a-dtu.md). 

U oba slučaja razine usluge obuhvaćaju **Osnovni**, **standardne**i **Premium**. Mogućnosti baze podataka u te razine slični su za samostalne baze podataka i elastic grupe, no postoje Dodatne napomene za elastic grupe. Ovaj članak sadrži detalje o razine servisa za samostalne baze podataka i elastic grupe.

## <a name="service-tiers-and-database-options"></a>Razine servisa i mogućnosti baze podataka
Osnovni, Standardno, i Premium servis razine imaju vrijeme aktivnosti SLA 99,99% i ponuditi predvidljivi performanse, fleksibilne tvrtke continuity mogućnosti, značajke sigurnosti i svaki sat naplata. Sljedeća tablica sadrži primjere najbolje razine najprikladniji za radnih opterećenja drugu aplikaciju.

| Razina usluge | Cilj radnih opterećenja |
|---|---|
| **Osnovni** | Najprikladnije su za male baze podataka, obično jedan jedne aktivne operacije u određeno vrijeme za podršku. Primjeri baza podataka koristi se za razvoj i testiranje ili manjih razmjera diskovni koristiti aplikacije. |
| **Standardna** | Idi na mogućnost za većinu aplikacija oblaka više Istodobni upita za podršku. Primjeri radna grupa ili web-aplikacije. |
| **Premium** | Osmišljeno za transakcijskih velik, mnogi korisnici Istodobni za podršku i potrebno najvišu razinu mogućnosti poslovne continuity. Primjeri su baze podataka zaštita njihove privatnosti ovise ključnih aplikacije za podršku. |

>[AZURE.NOTE] Web- a tvrtke izdanja iz upotrebe. Ako planirate da biste nastavili koristiti Web- a tvrtke izdanja, pročitajte [Najčešća pitanja vezana uz suton](https://azure.microsoft.com/pricing/details/sql-database/web-business/) .

## <a name="standalone-database-service-tiers-and-performance-levels"></a>Samostalne baze podataka usluge razine i performanse razine
Samostalne baza podataka, postoje više razina performanse u sloju svaki servis. Imate fleksibilnost odaberite željenu razinu koji najbolje odgovara vašem zahtjevima. Ako vam je potrebna za promjenu veličine prema gore ili prema dolje, možete jednostavno promijeniti razine bazu podataka. Detalje potražite u [Promjena razine baze podataka usluge i razine performansi](sql-database-scale-up.md) .

Karakteristike performansi nalaziti primjenjuju se na baze podataka stvorene pomoću [V12 SQL baze podataka](sql-database-v12-whats-new.md). Bez obzira na broj baza podataka nalazi baze podataka može vidjeti sigurno skup resursa i osobina očekivane performanse baze podataka ne utječe na.

[AZURE.INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

>[AZURE.NOTE] Detaljno objašnjenje sve retke u ovoj tablici razine servisa potražite u članku [Mogućnosti sloju usluge i ograničenja](sql-database-performance-guidance.md#service-tier-capabilities-and-limits).

## <a name="elastic-pool-service-tiers-and-performance-in-edtus"></a>Elastic skup servisa razine i performanse eDTUs
Osim stvaranja i skaliranje samostalnu bazu podataka, imate mogućnost upravljanja više baza podataka unutar [elastic skup](sql-database-elastic-pool.md). Sve baze podataka u elastic skup imaju zajednički skup resursa. Osobina performanse mjere po *Elastic jedinice transakcije baze podataka* (eDTUs). Kao što je s bazama podataka samostalni, grupe dolaze u tri razine usluge: **Osnovni** **standardne**i **Premium**. Za grupe, te razine tri servisa i dalje definirati ograničenja ukupne performanse i nekoliko značajki.

Grupe omogućuju baze podataka za zajedničko korištenje i trošiti resurse DTU bez obzira na to Dodjela razine performanse karakteristično za svaku bazu podataka u. Ako, na primjer, možete prijeći samostalnu bazu podataka u standardni skup pomoću 0 eDTUs za eDTU maksimalni baze podataka postavite prilikom konfiguriranja na resurse. Grupe omogućuju više baza podataka s različitim radnih opterećenja učinkovito korištenje eDTU resurse koji su dostupni za cijelu grupu. Detalje potražite u članku [cijena i performanse zahtjevi za elastic grupe aplikacija](sql-database-elastic-pool-guidance.md) .

U sljedećoj tablici opisane značajke razine servisa za grupu.

[AZURE.INCLUDE [SQL DB service tiers table for elastic pools](../../includes/sql-database-service-tiers-table-elastic-db-pools.md)]

Svaku bazu podataka unutar zajedničko područje i pridržava osobina samostalne baze podataka za taj sloju. Ako, na primjer, osnovni skup ima ograničenje za maksimalni sesije po skup 4800 28800, ali pojedinačne baze podataka unutar osnovni skup ima ograničenje baze podataka od 300 sesije.

## <a name="choosing-a-service-tier"></a>Odabir servisa sloj

Da biste odlučili na servis sloju, najprije određivanje hoće li baza podataka mora biti samostalnu bazu podataka ili dio elastic grupe aplikacija. 

### <a name="choosing-a-service-tier-for-a-standalone-database"></a>Odabir servisa sloju samostalne baze podataka

Da biste odlučili na sloju servisa za samostalnu bazu podataka, najprije određivanje značajke bazi podataka koje su vam potrebne da biste odabrali vaše izdanje SQL baze podataka:

- Veličina (2 GB u najviše Basic, 250 GB Maksimalna za standardnu i 500 GB najviše 1 TB za Premium - ovisno o razini performanse) baze podataka
- Razdoblje zadržavanja sigurnosne kopije baze podataka (7 dana za Basic, 35 dana za standardnu i 35 dana za Premium)

Nakon što ste zaključili izdanje SQL baze podataka, spremni ste za određivanje razine performansi za bazu podataka (broj DTUs). Možete pogoditi te [Skaliranje gore ili dolje dinamički](sql-database-scale-up.md) koji se temelji na stvarni iskustvo. [Kalkulator DTU](http://dtucalculator.azurewebsites.net/) možete koristiti i za određivanje broja DTUs potrebne. 

### <a name="choosing-a-service-tier-for-an-elastic-database-pool"></a>Odabir servisa sloju za grupe aplikacija programa elastic baze podataka.

Da biste odlučili na sloju servisa za grupe aplikacija programa elastic baze podataka, najprije određivanje značajke bazi podataka koje su vam potrebne da biste odabrali sloju servisa za svoje područje.

- Veličina baze podataka (2 GB za Basic, 250 GB za standardnu i 500 GB za Premium)
- Razdoblje zadržavanja sigurnosne kopije baze podataka (7 dana za Basic, 35 dana za standardnu i 35 dana za Premium)
- Broj baza podataka po grupe aplikacija (400 za Basic, 400 za standardnu i 50 za Premium)
- Maksimalna prostor za pohranu po skup (117 GB za Basic, 1200 za Standard, a 750 za Premium)

Nakon što ste zaključili sloju servisa za vaše područje, spremni ste za određivanje razine performansi za skup (eDTUs). Možete pogoditi, a zatim [proširenja ili dinamički skaliranje prema dolje](sql-database-elastic-pool-manage-portal.md#change-performance-settings-of-a-pool) na temelju stvarne sučelje. [Kalkulator DTU](http://dtucalculator.azurewebsites.net/) možete koristiti za određivanje broja DTUs potrebne za svaku bazu podataka u zajedničko područje da biste odredili gornju granicu za na resurse.

## <a name="next-steps"></a>Daljnji koraci
- Saznajte više o cijene za te razine na [Cijene za SQL baze podataka](https://azure.microsoft.com/pricing/details/sql-database/).
- Dodatne pojedinosti [elastic grupe](sql-database-elastic-pool-guidance.md) i [cijena i performanse zahtjevi za elastic grupe](sql-database-elastic-pool-guidance.md).
- Saznajte kako [, upravljanje, i promjena veličine elastic grupe](sql-database-elastic-pool-manage-portal.md) i [monitora performanse samostalne baze podataka](sql-database-single-database-monitor.md).
- Sad kad znate o razine SQL baze podataka, ih isprobate s [besplatan račun](https://azure.microsoft.com/pricing/free-trial/) i upute [za stvaranje prve baze podataka SQL](sql-database-get-started.md).

## <a name="additional-resources"></a>Dodatni resursi

- [Dizajniranje obrazaca za više klijentu SaaS aplikacije pomoću baze podataka SQL Azure](sql-database-design-patterns-multi-tenancy-saas-applications.md)
- [Microsoftova virtualna Academy videozapisa tečaja na elastic mogućnosti baze podataka u bazi podataka SQL Azure](https://mva.microsoft.com/en-US/training-courses/elastic-database-capabilities-with-azure-sql-db-16554)
