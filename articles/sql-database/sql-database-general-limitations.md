<properties
   pageTitle="Smjernice i ograničenja Općenito baze podataka Azure SQL"
   description="Ova stranica opisuje određena Općenito ograničenja za baze podataka SQL Azure, kao i područja interoperabilnost i podrška."
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
   ms.date="09/06/2016"
   ms.author="carlrab" />

# <a name="azure-sql-database-general-limitations-and-guidelines"></a>Smjernice i ograničenja Općenito baze podataka Azure SQL

Ova tema sadrži Općenito ograničenja i smjernice za baze podataka SQL Azure. Razumijevanje kvote, Upravljanje resursima i podršku potražite [Dodatne resurse](#additional-guidelines) na kraju ove teme.

## <a name="connectivity-and-authentication"></a>Povezivanje i provjeru autentičnosti

  - Provjera autentičnosti sustava Windows nije podržana. U odjeljku [Upravljanje bazama podataka i prijave u bazi podataka Azure SQL](sql-database-manage-logins.md). Međutim, provjere autentičnosti Azure Active Directory podržan je određene ograničenja. Potražite u članku [Povezivanje s bazom podataka SQL s provjerom autentičnosti Azure Active Directory](sql-database-aad-authentication.md).

  - Baza podataka Microsoft Azure SQL podržava tablične podatke strujanje (TDS) protokol verzija klijenta 7.3 ili noviji.

  - Nije dopušteno samo TCP/IP veze.

  - SQL Server 2008 SQL Server preglednik nije podržan jer baza podataka Microsoft Azure SQL nema dinamički priključke samo priključak 1433.

## <a name="sql-server-agentjobs"></a>SQL Server Agent/poslova

Baza podataka Microsoft Azure SQL ne podržava agenta poslužitelja SQL Server, no možete upotrijebiti Elastic poslovi izvođenje zadataka po jedan za mnoge baze podataka. Dodatne informacije o Elastic zadacima potražite u članku [Elastic zadatke](sql-database-elastic-jobs-overview.md).

## <a name="sql-server-collation-support"></a>Razvrstavanje podrška za SQL Server

Zadano uparivanje baze podataka koriste baza podataka Microsoft Azure SQL je **SQL_LATIN1_GENERAL_CP1_CI_AS**, pri čemu **LATIN1_GENERAL** je engleski (SAD), **CP1** je kodna stranica 1252, **CI** je velika i mala slova, a **kao** naglaske. Nije moguće promijeniti razvrstavanje za V12 baze podataka. Dodatne informacije o postavljanju na razvrstavanje potražite u članku [UVEZ (Transact-SQL)](https://msdn.microsoft.com/library/ms184391.aspx).

## <a name="naming-requirements"></a>Preduvjeti imenovanja

Nije dopušteno određene korisnička imena sigurnosnih vam razloga. Ne možete koristiti sljedeće nazive:

 - **administrator**
 - **administrator**
 - **goste**
 - **Korijenska**
 - **Pacifička**

Imena za sve nove objekte moraju biti usklađene sa SQL Server pravila za identifikatore. Dodatne informacije potražite u članku [identifikatora](https://msdn.microsoft.com/library/ms175874.aspx).

Uz to, prijavu i korisničko nazivi ne smiju sadržavati na \ znakova (Provjera autentičnosti sustava Windows nije podržan).

## <a name="additional-guidelines"></a>Dodatne upute

- Osim općenitih navedene u ovom članku ograničenja baze podataka SQL ima kvote određenog resursa i ograničenja koji se temelji na **servis sloju**. Pregled servisa razine, potražite u članku [razine servisa SQL baze podataka](sql-database-service-tiers.md).

- Ograničenja druge baze podataka SQL potražite u članku [Ograničenja resursa za Azure SQL baze podataka](sql-database-resource-limits.md).

- Sigurnost povezane upute potražite u članku [upute o sigurnosti baze podataka SQL Azure i ograničenja](sql-database-security-guidelines.md).

- Drugi područje povezano okružuje kompatibilnosti s bazom podataka SQL Azure s lokalnim verzijama sustava SQL Server, kao što su 2014 SQL Server i SQL Server 2016. Najnoviju verziju V12 baze podataka SQL Azure učinio mnogo poboljšanja u ovom području. Dodatne informacije potražite u članku [što je novo u SQL baze podataka V12](sql-database-v12-whats-new.md).

- Informacije o dostupnosti upravljački program i podrška za SQL baze podataka, potražite u članku [Biblioteke veza za SQL baze podataka i SQL Server](sql-database-libraries.md).
