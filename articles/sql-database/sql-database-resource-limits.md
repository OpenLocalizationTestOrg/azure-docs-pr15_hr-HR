<properties
    pageTitle="Ograničenja resursa za baze podataka Azure SQL"
    description="Ova stranica opisuje neke uobičajene ograničenja resursa za bazu podataka SQL Azure."
    services="sql-database"
    documentationCenter="na"
    authors="CarlRabeler"
    manager="jhubbard"
    editor="monicar" />


<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="10/13/2016"
    ms.author="carlrab" />


# <a name="azure-sql-database-resource-limits"></a>Ograničenja resursa za Azure SQL baze podataka

## <a name="overview"></a>Pregled

Baze podataka SQL Azure upravlja resursa za bazu podataka pomoću dvije različite mehanizme: **Upravljanja resursima** i **Provođenje ograničenja**. U ovoj se temi objašnjava te dva glavna područja Upravljanje resursima.

## <a name="resource-governance"></a>Upravljanje resursa
Jedna od ciljeva dizajn servisa razine Basic, standardna i Premium je za bazu podataka SQL Azure ponašati kao da se baza podataka se izvodi na vlastitom računalu potpuno Izolirani iz druge baze podataka. Resurs upravljanja oponaša takvo ponašanje. Ako Upotreba Zbrojeno resursa dostigne maksimalno dostupna procesora, memorije, / i zapisnika i resurse/podataka i dodijeljene baze podataka, upravljanja resursa će red čekanja upita u izvođenja i dodijelili resurse u redu čekanja upita kao što su oslobodili.

Kao na namjenski računalo korištenja svih dostupnih resursa rezultirat će dulje izvršavanje trenutno izvršavanja upita, može uzrokovati vremensko ograničenje naredbe na klijentskom računalu. Aplikacije s izrazito pokušaj i aplikacije koje se izvršiti upite odabiranja baze podataka pomoću velika učestalost možete naići na pogreške poruke prilikom izvršavanja nove upite kada do limita istovremeni zahtjevi.

### <a name="recommendations"></a>Preporuka:
Praćenje Upotreba resursa, kao i vrijeme average odaziva upita kada nearing Maksimalna Upotreba baze podataka. Kada nailazi na višu upita latencies obično su vam tri mogućnosti:

1.  Smanjite količinu zahtjevi za bazu podataka da biste spriječili vremensko ograničenje i snop gore zahtjeva.

2.  Dodijelite na višu razinu performanse u bazu podataka.

3.  Optimiziranje upita da biste smanjili Upotreba resursa svaki upita. Dodatne informacije potražite u odjeljku Tuning Hinting upita u članku performanse smjernice za Azure SQL baze podataka.

## <a name="enforcement-of-limits"></a>Provođenje ograničenja
Resursi koji nisu procesora, memorije, / i zapisnika i/podataka i su postavio odbijanju nove zahtjeve kada je dostigne ograničenja. Klijenti primit će [poruku o pogrešci](sql-database-develop-error-messages.md) ovisno o tome koje se do.

Na primjer, broj veza s bazom podataka SQL, kao i broj istovremeni zahtjevi može obraditi nisu dopuštene. Baze podataka SQL omogućuje broj veza s bazom podataka biti veći od broja istovremeni zahtjevi za podršku okupljanje. Aplikaciju možete jednostavno upravlja količinu veze koje su dostupne, iznos paralelno zahtjeva za je često puta teže za procjenu i kontrolirali. Osobito tijekom Vršna opterećenje kada aplikacija ili šalje previše zahtjeve ili baze podataka dosegne njegov resurs ograničenjima i pokreće gomilanje radnih niti zbog dulje izvodi upite, možete se pogreške.

## <a name="service-tiers-and-performance-levels"></a>Razine usluge i razine performansi

Postoje razine usluge i razine performansi za oba samostalne baze podataka i elastic grupe.

### <a name="standalone-databases"></a>Samostalne baze podataka

Za samostalnu bazu podataka ograničenja baze podataka određuje razinu baze podataka usluge sloju i performanse. U sljedećoj tablici opisane karakteristike osnovni standardne i Premium baze podataka na promjenjivih razine performansi.

[AZURE.INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

### <a name="elastic-pools"></a>Elastic grupe

Zajedničko korištenje resursa [elastic grupe](sql-database-elastic-pool.md) preko baze podataka u. U sljedećoj tablici opisane značajke Basic, standardna i Premium grupe elastic baze podataka.

[AZURE.INCLUDE [SQL DB service tiers table for elastic databases](../../includes/sql-database-service-tiers-table-elastic-db-pools.md)]

Definiciju prošireno svakog resursa navedene u tablicama prethodne potražite u članku opise svojstava u [Mogućnosti sloju usluge i ograničenja](sql-database-performance-guidance.md#service-tier-capabilities-and-limits). Pregled servisa razine, potražite u članku [Azure SQL baze podataka usluge razine i performanse razine](sql-database-service-tiers.md).

## <a name="other-sql-database-limits"></a>Druga baza podataka SQL ograničenja

| Područje | Ograničenje | Opis |
|---|---|---|
| Baze podataka koje koriste Automated izvoz po pretplati | 10 | Automatsko izvoz omogućuje vam da biste stvorili prilagođeni raspored za sigurnosno kopiranje baze podataka za SQL. Dodatne informacije potražite u članku [baze podataka SQL: podrška za automatski izvozi baze podataka SQL](http://weblogs.asp.net/scottgu/windows-azure-july-updates-sql-database-traffic-manager-autoscale-virtual-machines).|
| Baza podataka po poslužitelju | Do 5000 | Najviše 5000 baze podataka dopušteno po poslužitelju na poslužiteljima V12. |  
| DTUs po poslužitelju | 45000 | 45000 DTUs dostupnih po poslužitelju na poslužiteljima V12 za dodjelu resursa baze podataka, elastic grupe i skladištima podataka. |



## <a name="resources"></a>Resursi

[Azure pretplate i ograničenja servisa, kvota i ograničenjima](../azure-subscription-service-limits.md)

[Azure SQL baze podataka usluge razine i performanse razine](sql-database-service-tiers.md)

[Poruke o pogreškama za klijentske programe za SQL baze podataka](sql-database-develop-error-messages.md)
