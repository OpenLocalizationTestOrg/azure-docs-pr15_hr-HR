<properties
    pageTitle="Što je SQL baze podataka? Uvod u SQL baze podataka | Microsoft Azure"
    description="Početak Uvod u SQL baze podataka: tehničke pojedinosti i mogućnosti programa Microsoft relacijske baze podataka sustava upravljanja (RDBMS) u oblaku."
    keywords="Uvod u sql, Uvod u sql, što je sql baze podataka"
    services="sql-database"
    documentationCenter=""
    authors="shontnew"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="08/16/2016"
   ms.author="shkurhek"/>

# <a name="what-is-sql-database-introduction-to-sql-database"></a>Što je SQL baze podataka? Uvod u SQL baze podataka

Baze podataka SQL je servis relacijske baze podataka u oblaku koja se temelji na Microsoft SQL Server modul tržištu prored ima zaštita njihove privatnosti ovise ključnih mogućnosti. Baze podataka SQL nudi predvidljivi performanse, skalabilnost bez nedostupnost, poslovanje i zaštitu podataka – sve uz Administracija pri od nule. Možete se fokusirati na razvoja brzog aplikacija i accelerating vremena na tržištu, umjesto upravljanje virtualnim strojevima i infrastrukture. Jer se temelji na modul [Sustava SQL Server](https://msdn.microsoft.com/library/bb545450.aspx) , baze podataka SQL podržava postojeće SQL Server alata, biblioteke i API-ji, što olakšava premještanje i proširivanje u oblak.

Ovaj je članak Uvod u rad s bazom podataka SQL osnovni koncepti i značajke koje su vezane uz performanse, skalabilnost i mogućnost upravljanja s vezama na detalja. Ako ste spremni da biste se prebacili u, možete ga [Stvaranje prve baze podataka SQL](sql-database-get-started.md) ili [Stvaranje grupe aplikacija programa elastic baze podataka](sql-database-elastic-pool-create-portal.md) u minutama. Ako želite da se dublju dive, pogledajte ovaj videozapis 30 minuta.

> [AZURE.VIDEO azurecon-2015-get-started-with-azure-sql-database]

## <a name="adjust-performance-and-scale-without-downtime"></a>Prilagodba performanse i skaliranje bez nedostupnost

Baze podataka SQL dostupan je u Basic, standardna i Premium *razine servisa*. Svaki servis sloju nudi [različite razine performanse i mogućnosti](sql-database-service-tiers.md) za podršku lightweight da biste radnih opterećenja heavyweight baze podataka. Možete izrađivati prvi aplikacije na male baze podataka za nekoliko bucks mjesečno, zatim [Promijeni sloju servisa](sql-database-scale-up.md) ručno ili programski u bilo kojem trenutku tijekom aplikacije širenjem virusa te cijelom svijetu, bez nedostupnost aplikacije ili klijentima.

Za mnoge tvrtke i aplikacije mogućnost stvaranja baze podataka i birati performanse jedne baze podataka prema gore ili dolje na zahtjev je dovoljno, osobito ako uzoraka korištenja relativno predvidljivi. Ali ako imate nepredvidljive upotrebe, je bio teško upravljanje troškove i modelu tvrtke.

[Elastic grupe](sql-database-elastic-pool.md) u bazi podataka SQL riješili taj problem. Koncept je jednostavno. Dodijeliti performanse na resurse i platiti za zajednički rad na resurse umjesto performanse jedne baze podataka. Ne morate birati performanse baze podataka prema gore ili dolje. Baza podataka u grupu, pod nazivom *elastic baza podataka*automatski promijeniti omjer gore i prema dolje do susret zahtjev. Elastic baza podataka zauzeti, ali ne premašuju ograničenja na resurse tako da se vaš trošak ostaje predvidljivi čak i ako se ne korištenje baze podataka. Što se događa 's više možete [dodavati i uklanjati baze podataka na resurse](sql-database-elastic-pool-manage-portal.md), skaliranje aplikacije s handful baza podataka tisućama sve u okviru proračuna koji omogućuje upravljanje. Da biste saznali više o dizajn obrazaca za SaaS aplikacije koje se koriste elastic grupe, uvidjeti [dizajna za više klijentske aplikacije SaaS s bazom podataka SQL Azure](sql-database-design-patterns-multi-tenancy-saas-applications.md).

Oba načina rada – jedan ili elastic – vi ste zaključali. Možete izmiješati jedne baze podataka s grupe elastic baze podataka i promijeniti razine servisa jedne baze podataka te grupe da biste stvorili Inovativne dizajna. Nadalje, uz power i razgovor Azure, možete mix i match servisa Azure SQL baze podataka ne odgovara vašim potrebama dizajn jedinstvenim modernim aplikacije, pogon učinkovitosti trošak i resursa i otključavanje novi poslovanje.

No kako vam Usporedba relativni performanse baze podataka i grupe baze podataka? Kako znati desnom tipkom miša kliknite-tabulator kada birate prema gore i dolje? Odgovor je transakcija jedinica baze podataka (DTU) za jedan baze podataka i elastic DTU (eDTU) za elastic baze podataka i grupe baze podataka. Potražite u članku [Mogućnosti SQL baze podataka i performanse: objašnjenje što dostupna je u sloju svaki servis](sql-database-service-tiers.md) detalje.

## <a name="keep-your-app-and-business-running"></a>Neka vaše aplikacije i poslovanjem

Azure, grana početne 99,99% dostupnost razine ugovor o usluzi [(SLA)](http://azure.microsoft.com/support/legal/sla/), pokreće globalna mreža podatkovnim centrima Microsoft upravlja olakšava održavanje aplikacija sa sustavom 24-7. Svaki SQL baze podataka iskoristiti sve prednosti Zaštita od ugrađenih podataka, odstupanje kvara i zaštitu podataka koje bi inače imate dizajna, kupite, stvaranje i upravljanje. Ipak, ovisno o tome na zahtjeve vaša tvrtka možda potražnje dodatne slojeva zaštite da biste bili sigurni aplikacije i poslovanja možete oporaviti brzo slučaju na Izrada, pogrešku ili nešto drugo. Baze podataka SQL sloju svaki servis nudi drugi izbornik značajke možete koristiti da biste dobili prema gore i pokrenut će ostati na taj način. Vraćanje točke u vrijeme možete koristiti da biste se vratili bazu podataka na prethodno stanje as far back as 35 dana. Osim toga, ako s podatkovnim centrom hostira baze podataka sučelja programa nedostupnosti, možete prebacivanje na replike baze podataka u nekoj drugoj regiji. Ili možete koristiti replike nadogradnji ili relocation za različite regije.

![Zemlj. – replikacija baze podataka SQL](./media/sql-database-technical-overview/azure_sqldb_map.png)


Detalje o različitim poslovnim continuity značajki dostupnih za servis za različite razine potražite u članku [Poslovanje](sql-database-business-continuity.md) .

## <a name="secure-your-data"></a>Zaštitu podataka
SQL Server ima običajima sigurnost pune podataka da baze podataka SQL upholds sa značajkama koje ograničiti pristup, zaštiti podataka i olakšavaju praćenje aktivnosti. Potražite u članku [Zaštita baze podataka sustava SQL](sql-database-security.md) za brzi rundown sigurnosne mogućnosti koje imate u SQL baze podataka. Potražite u [Centru za sigurnost za modul za baze podataka SQL Server i SQL baze podataka](https://msdn.microsoft.com/library/bb510589) za detaljnije prikaz sigurnosnih značajki. I posjetite [Centar za pouzdanost Azure](https://azure.microsoft.com/support/trust-center/security/) informacije o sigurnosti platforme Azure korisnika.

## <a name="next-steps"></a>Daljnji koraci
Sad kad ste pročitajte Uvod u SQL baze podataka i odgovoriti na pitanje "Što je baza podataka SQL?", spremni ste za:

- Potražite u članku [cijene stranice](https://azure.microsoft.com/pricing/details/sql-database/) za jednoj bazi podataka i usporedbu trošak elastic baze podataka i izračuna.
- Saznajte više o [elastic grupe](sql-database-elastic-pool.md).
- Početak rada kao što su [Stvaranje prve baze podataka](sql-database-get-started.md).
- [Povezivanje i upita s SSMS](sql-database-connect-query-ssms.md)
- Stvaranje prve aplikacije u C#, Java, Node.js, PHP, Python ili Ruby: [biblioteke veza za SQL baze podataka i SQL Server](sql-database-libraries.md)
- U odjeljku indeksa naslova i opisa [svih](sql-database-index-all-articles.md)tema servisa Azure sql-baze podataka.
