<properties
   pageTitle="Prenesite rješenje SQL Data Warehouse | Microsoft Azure"
   description="Migracija smjernice za prenošenje rješenje platformu Microsoft Azure SQL Data Warehouse."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/30/2016"
   ms.author="barbkess;jrj;sonyama"/>

# <a name="migrate-your-solution-to-sql-data-warehouse"></a>Prenesite rješenje SQL Data Warehouse

SQL Data Warehouse je sustav raspodijeljeno baze podataka koji elastically mijenja veličinu da bi odgovarao vašim potrebama. Da biste zadržali performanse i promjena veličine, sve značajke programa SQL Server primjenjuju se unutar SQL Data Warehouse. U sljedećim temama za migraciju s dodirnim zaslonom na nekim ključni faktori radi migracije rješenje u SQL Data Warehouse. Dizajniranje podataka skladištima skaliranja predstavlja različite dizajn obrazaca i tako tradicionalni pristupa nisu uvijek najbolje. Stoga vidjet ćete da adapting postojeće rješenje osigurava moći iskoristiti raspodijeljeno platformu nudi SQL Data Warehouse.

Također je važno Imajte na umu da je SQL Data Warehouse platforme koji se temelji na Microsoft Azure. Stoga dio migraciju možda dobro obuhvaćaju prijenos podataka s oblakom. Prijenos je predmet u vlastitom desnom i pažljivo razmatranje; osobito kao povećanje količine. Prijenos i učitavanja podataka su samostalni teme.

## <a name="migration-guidance"></a>Vodič za migraciju

Provjerite je li pročitali ste pomoću ovih članaka da biste bili sigurni neke razlike proizvoda i osnovne koncepte razumijevanje prije inicijativu migraciju.

- [Migriranje sheme][]
- [Migracija podataka][]
- [Migriranje kod][]

## <a name="next-steps"></a>Daljnji koraci

MAČKA (kupac savjeta Team) ima neki sjajno smjernica za SQL Data Warehouse koji ih objavite putem blogova.  Pogledajte njihove članak, [pod prijenos podataka Data Warehouse SQL Azure u praksi][] Dodatne upute o migracije.

<!--Image references-->

<!--Article references-->
[Migriranje sheme]: sql-data-warehouse-migrate-schema.md
[Migracija podataka]: sql-data-warehouse-migrate-data.md
[Migriranje kod]: sql-data-warehouse-migrate-code.md


<!--MSDN references-->


<!--Other Web references-->
[Prijenos podataka Azure SQL Data Warehouse efekte.]: https://blogs.msdn.microsoft.com/sqlcat/2016/08/18/migrating-data-to-azure-sql-data-warehouse-in-practice/
