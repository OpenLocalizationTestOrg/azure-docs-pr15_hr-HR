<properties
    pageTitle="Baze podataka SQL razviti pregled | Microsoft Azure"
    description="Informirajte se o dostupna povezivanje biblioteke i najbolje prakse za aplikacije za povezivanje s bazom podataka SQL."
    services="sql-database"
    documentationCenter=""
    authors="annemill"
    manager="jhubbard"
    editor="genemi"/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/17/2016"
    ms.author="annemill"/>

# <a name="sql-database-development-overview"></a>Pregled razvoj baze podataka za SQL
U ovom se članku vodi kroz osnovne pitanja vezana uz koji je razvojni inženjer trebali imati na umu prilikom pisanja koda za povezivanje s bazom podataka SQL Azure.

## <a name="language-and-platform"></a>Jezik i platforme
Su dostupne za različite programskog jezika i platforme primjere koda. Možete pronaći veze na primjere koda na: 

* Dodatne informacije: [biblioteke veza za SQL baze podataka i SQL Server](sql-database-libraries.md)

## <a name="resource-limitations"></a>Ograničenja resursa
Baze podataka SQL Azure upravlja resursa za bazu podataka pomoću dvije različite mehanizme: upravljanja resursima i provođenje ograničenja.

* Dodatne informacije: [ograničenja resursa Azure SQL baze podataka](sql-database-resource-limits.md)

## <a name="security"></a>Sigurnost
Baze podataka SQL Azure nudi resursi za ograničavanje pristupa, zaštita podataka i nadzor aktivnosti u SQL baze podataka.

* Dodatne informacije: [Zaštita baze podataka sustava SQL](sql-database-security.md)

## <a name="authentication"></a>Provjera autentičnosti
* Baze podataka SQL Azure podržava i SQL Server provjere autentičnosti korisnika i prijave, kao i [Azure Active Directory provjere autentičnosti](sql-database-aad-authentication.md) korisnika i prijave.
* Morate navesti određenu bazu podataka, umjesto vraćanje na *glavnom* bazom podataka.
* Da biste prešli na drugu bazu podataka ne možete je koristiti naredbu Transact-SQL **POMOĆU myDatabaseName;** SQL baze podataka.
* Dodatne informacije: [Sigurnost baze podataka SQL: Upravljanje sigurnošću baze podataka programa access i prijavite](sql-database-manage-logins.md)

## <a name="resiliency"></a>Otpornost
Kada se pojavi tranzitne pogreške pri povezivanju s bazom podataka SQL, kod potrebno ponovno poziv.  Preporučujemo da koji se ponovno pokušajte logike backoff za korištenje logike tako da se pretrpati SQL baze podataka s više klijenata ponovni istodobno.

* Kod uzorke: primjere koda koji pokazuju ponovnog pokušaja, u odjeljku uzorci na jeziku po izboru pri: [biblioteke veza za SQL baze podataka i SQL Server](sql-database-libraries.md)
* Dodatne informacije: [poruke o pogreškama za klijentske programe za SQL baze podataka](sql-database-develop-error-messages.md)

## <a name="managing-connections"></a>Upravljanje vezama
* U veze logiku klijent nadjačati zadane vremensko ograničenje biti 30 sekundi.  Zadana vrijednost od 15 sekundi je prevelik skraćeno veze koje ovise o tome s Internetom.
* Ako koristite [skup veza](http://msdn.microsoft.com/library/8xx3tyca.aspx), ne zaboravite prekinuti vezu izravnih poruka programa je aktivno ne koristite, a ne Priprema radi ponovnog korištenja.

## <a name="network-considerations"></a>Razmatranja mreže
* Na računalu koje hostira klijentski program, provjerite je li vatrozid omogućuje izlaznu komunikaciju TCP na priključak 1433.  Dodatne informacije: [Konfiguriranje Vatrozida za baze podataka SQL Azure](sql-database-configure-firewall-settings.md)
* Ako klijentski program povezuje V12 baze podataka SQL tijekom klijent na Azure virtualnog računala (VM), morate otvoriti određenog raspona priključak na na VM. Dodatne informacije: [priključke izvan 1433 za ADO.NET 4,5 i V12 baze podataka SQL](sql-database-develop-direct-route-ports-adonet-v12.md)
* Klijent veze s V12 za baze podataka SQL Azure ponekad zaobići proxy poslužitelj i interakciju izravno s bazom podataka. Priključci osim 1433 postaju važna. Dodatne informacije: [priključke izvan 1433 za ADO.NET 4,5 i V12 baze podataka SQL](sql-database-develop-direct-route-ports-adonet-v12.md)

## <a name="data-sharding-with-elastic-scale"></a>Sharding podataka s Elastic skalom
Elastic mjerilo pojednostavljuje postupak promjene veličine out (i u). 

* [Uzorci dizajna za SaaS više klijentske aplikacije s bazom podataka Azure SQL](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* [Podaci o njima ovisne usmjeravanja](sql-database-elastic-scale-data-dependent-routing.md)
* [Početak rada s pretpregled Elastic mjerilo za Azure SQL baze podataka](sql-database-elastic-scale-get-started.md)

## <a name="next-steps"></a>Daljnji koraci

Istražite [Mogućnosti SQL baze podataka](https://azure.microsoft.com/services/sql-database/).
