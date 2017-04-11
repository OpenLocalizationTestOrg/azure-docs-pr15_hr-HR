<properties
    pageTitle="Baza podataka na poslužitelju trenutno nije dostupna, povezivanje s bazom podataka SQL | Microsoft Azure"
    description="Otklanjanje poteškoća s baze podataka na poslužitelj nije dostupan pogreške kada se aplikacija poveže s bazom podataka SQL."
    services="sql-database"
    documentationCenter=""
    authors="dalechen"
    manager="felixwu"
    editor=""
    keywords="Baza podataka na poslužitelju trenutno nije dostupna, povezivanje s bazom podataka sql"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="daleche"/>

# <a name="error-database-on-server-is-not-currently-available-when-connecting-to-sql-database"></a>Pogreška "Baze podataka na poslužitelju nije dostupan" prilikom povezivanja s bazom podataka sql
[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

Kada aplikacija povezuje se s bazom podataka Azure SQL, dobit ćete sljedeću poruku o pogrešci:

```
Error code 40613: "Database <x> on server <y> is not currently available. Please retry the connection later. If the problem persists, contact customer support, and provide them the session tracing ID of <z>"
```

> [AZURE.NOTE] Poruka o pogrešci je obično tranzitne (short-lived).

Ta se pogreška pojavljuje kada Azure baze podataka se premjestiti (ili rekonfigurirati) i aplikacije prekine veza s bazom podataka SQL. SQL baza podataka ponovno konfiguriranje događaja pojavljuje se zbog planiranog događaja (na primjer, Nadogradnja softvera) ili događaja neplanirano (na primjer, rušenje postupak, ili opterećenja). Većina ponovno konfiguriranje događaja su obično short-lived i mora dovršiti manje od 60 sekundi najviše. Međutim, ti događaji povremeno traje dulje da biste završili, kao što su kada velike transakcije uzrokuje dugoročnih oporavak.

## <a name="steps-to-resolve-transient-connectivity-issues"></a>Koraci za rješavanje problema s povezivanjem tranzitne
1.  Provjerite [Nadzorna ploča sustava Microsoft Azure službe](https://azure.microsoft.com/status) za sve poznate kvarove do kojih je došlo tijekom vremena tijekom kojeg se su pogrešaka u aplikaciji.
2. Aplikacije koje povezivanje na servise u oblaku, kao što su baze podataka SQL Azure trebali biste očekivali periodičku ponovno konfiguriranje događaja i implementirati ponovno logike za obradu tih pogrešaka umjesto pojavljivanje kao pogreške aplikacije korisnicima. Pregledajte odjeljak [tranzitne pogreške prilikom](sql-database-connectivity-issues.md) i najbolje prakse i dizajn pogrešaka pri [Razvoju pregled baze podataka za SQL](sql-database-develop-overview.md) dodatne informacije i opće ponovite strategije. Zatim, pogledajte primjere koda na [Biblioteke veza za SQL baze podataka i SQL Server](sql-database-libraries.md) za specifičnosti.
3.  Kao što je baze podataka izvršenja njegov ograničenja resursa, čini možete biti tranzitne povezivanje problem. Potražite u članku [otklanjanje problema s performansama](sql-database-troubleshoot-performance.md).
4.  Nastavite problema s povezivanjem, ili ako trajanje za koju aplikaciju sustava naiđe na pogrešku premašuje 60 sekundi ili ako vidite više pojava pogreške na navedeni dan, datoteka na zahtjev za Azure podršku tako da odaberete **Dobili podršku** na web-mjestu za [Podršku Azure](https://azure.microsoft.com/support/options) .

## <a name="next-steps"></a>Daljnji koraci
- Ako vam se prikaže poruka o pogrešci u drugu, računaju se [poruka o pogrešci](sql-database-develop-error-messages.md) promatrajući uzrok.
- Ako se problem ne stalni, potražite upute u [Otklanjanje uobičajenih problema s povezivanjem s bazom podataka SQL Azure](sql-database-troubleshoot-common-connection-issues.md).
