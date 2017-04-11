<properties
    pageTitle="Praćenje performansi baze podataka u bazi podataka SQL Azure | Microsoft Azure"
    description="Dodatne informacije o mogućnostima za nadzor bazu podataka s Azure Alati i dinamični Upravljanje prikazima."
    keywords="Praćenje performansi oblaka baze podataka u bazu podataka"
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="09/27/2016"
    ms.author="carlrab"/>

# <a name="monitoring-database-performance-in-azure-sql-database"></a>Praćenje performansi baze podataka u bazi podataka SQL Azure
Praćenje performansi bazom podataka sustava SQL Azure počinje nadzor Upotreba resursa odnosu razinu performanse baze podataka koje odaberete. Nadzor omogućuje određivanje hoće li baza podataka ima kapacitet suvišno ili ne uspijeva jer Resursi su dostignut odgovor, a zatim odredite hoće li je vrijeme da biste prilagodili razinu performanse i [usluge sloju](sql-database-service-tiers.md) baze podataka. Možete nadzirati baze podataka pomoću alata za grafički [Azure portal](https://portal.azure.com) ili pomoću SQL [dinamički Upravljanje prikazima](https://msdn.microsoft.com/library/ms188754.aspx).

## <a name="monitor-databases-using-the-azure-portal"></a>Praćenje baze podataka pomoću portala za Azure

[Portal za Azure](https://portal.azure.com/)možete nadzirati Upotreba jedne baze podataka, odaberite bazu podataka i kliknite grafikon **praćenja** . Tako ćete prikazati **metriku** prozoru možete promijeniti tako da kliknete gumb **Uredi grafikona** . Dodajte sljedeće metriku:

- Postotak CPU-a
- Postotak DTU
- Postotak IO podataka
- Postotak veličine baze podataka

Kada dodate te metriku, možete nastaviti njihov prikaz u grafikon **praćenja** s više detalja u prozoru **metriku** . Sva četiri mjerenja prikazuje postotak average Upotreba odnosu **DTU** kopiju baze podataka. Detalje o DTUs potražite u članku [servis razine](sql-database-service-tiers.md) .

![Servisa sloju praćenje performansi baze podataka.](./media/sql-database-service-tiers/sqldb_service_tier_monitoring.png)

Upozorenja možete konfigurirati i na performanse metriku. Kliknite gumb **Dodaj upozorenja** u prozoru **metriku** . Slijedite Čarobnjak za konfiguriranje vaše upozorenja. Ako prelaze metriku određeni prag ili metriku pada ispod određeni prag imate mogućnost upozorenja.

Na primjer, ako očekujete povećavaju na bazu podataka radi povećanja, možete konfigurirati upozorenja e-pošte kad god bazu podataka dosegne 80% na bilo kojoj od metriku performanse. Koristite ovaj kao Prijevremeni upozorenje da biste utvrdili kada možda ćete morati prijelaz na sljedeću višu razinu performansi.

Metriku performanse i pojednostavljuju određivanje ako možete prijeći na nižu na nižu razinu performansi. Pretpostavimo da koristite standardne S2 baze podataka, a zatim Prikaži sva mjerenja performansi baze podataka u prosjek koristiti više od 10% u bilo kojem trenutku. Vjerojatno da baza podataka će funkcionirati i u standardni S1 je. Međutim, imajte na umu radnih opterećenja koji Šiljak ili mijenjaju prije unošenja odluku da biste premjestili niže razine performansi.

## <a name="monitor-databases-using-dmvs"></a>Monitor baze podataka koje koriste DMVs

Isti metriku prikazat će se na portalu su dostupne putem Sistemski prikazi: [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) u bazi podataka logičke **glavnog** poslužitelja i [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) u bazi podataka za korisnika. Ako vam je potrebna praćenje manje zrnastog podataka preko dulje vremensko razdoblje, koristite **sys.resource_stats** . Ako vam je potrebna praćenje precizniji podataka u manjim vremensko razdoblje, koristite **sys.dm_db_resource_stats** . Dodatne informacije potražite u članku [Performanse smjernice za Azure SQL baze podataka](sql-database-performance-guidance.md#monitoring-resource-use-with-sysresourcestats).

>[AZURE.NOTE] **sys.dm_db_resource_stats** vraća prazan rezultat postavljanje kad se koristi u Web- a tvrtke edition baze podataka, koji je iz upotrebe.

Za grupe elastic baze podataka, možete nadzirati pojedinačne baze podataka u s tehnike navedena u ovom odjeljku. No možete nadzirati i skup kao cjelinu. Informacije potražite u članku [nadzor i upravljanje njima u skup elastic baze podataka](sql-database-elastic-pool-manage-portal.md).
