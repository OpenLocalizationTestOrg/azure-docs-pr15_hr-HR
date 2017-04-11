<properties
   pageTitle="Korištenje tvorničke Azure podataka sa SQL Data Warehouse | Microsoft Azure"
   description="Savjeti za korištenje tvorničke Azure podataka (ADF) pomoću Azure SQL Data Warehouse za razvoj rješenja."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/08/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="use-azure-data-factory-with-sql-data-warehouse"></a>Korištenje tvorničke Azure podataka sa SQL Data Warehouse

Azure tvorničke podataka sadrži potpuno upravljanih način za orchestrating prijenos podataka i izvršavanja pohranjene procedure na SQL Data Warehouse.  To će vam omogućiti na jednostavnije postaviti i raspored složene izdvojiti pretvaranje i učitavanje (ETL) postupke sa SQL Data Warehouse. Potpuniji pregled tvorničke Azure podataka potražite u članku [Azure podataka tvorničke dokumentacije][].

## <a name="data-movement"></a>Premještanje podataka

Azure tvorničke podataka omogućuje premještanje podataka između lokalnih izvora i različite servise za Azure.  Cjelokupan, trenutni Integracija s Azure podataka tvorničke podržava premještanje podataka da biste i sa sljedećih mjesta:

+ Spremište blobova platforme Azure
+ Baze podataka Azure SQL
+ Lokalnog sustava SQL Server
+ SQL Server na IaaS

Informacije o postavljanju podataka Kopiraj aktivnosti potražite u članku [Kopiranje podataka s tvorničke Azure podataka][]

## <a name="stored-procedures"></a>Pohranjene procedure
 Na isti način koristi za zakazivanje prijenos podataka tvorničke Azure podatke i omogućuje orkestrirali izvršavanja pohranjene procedure.  Time se omogućuje složenije kanali će biti stvoren i proširuje prednosti računalne programi SQL Data Warehouse Azure podataka tvorničke mogućnost.

## <a name="next-steps"></a>Daljnji koraci
Pregled integracije potražite u članku [Pregled integracije SQL Data Warehouse][].
Dodatne savjete za razvoj potražite u članku [Pregled SQL Data Warehouse razvoj][].

<!--Image references-->

<!--Article references-->

[Kopirajte podatke s tvorničke Azure podataka]: ../data-factory/data-factory-data-movement-activities.md
[Pregled SQL Data Warehouse razvoj]: ./sql-data-warehouse-overview-develop.md
[Pregled integracije SQL Data Warehouse]: ./sql-data-warehouse-overview-integrate.md

<!--MSDN references-->

<!--Other Web references-->
[Azure dokumentaciju tvorničke podataka]:https://azure.microsoft.com/documentation/services/data-factory/

