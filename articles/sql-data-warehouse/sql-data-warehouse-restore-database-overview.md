<properties
   pageTitle="Vraćanje SQL Data Warehouse | Microsoft Azure"
   description="Pregled mogućnosti Vraćanje baze podataka za oporavak baze podataka u skladištu podataka za SQL Azure."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="Lakshmi1812"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/29/2016"
   ms.author="lakshmir;barbkess;sonyama"/>


# <a name="sql-data-warehouse-restore"></a>Vraćanje SQL Data Warehouse

> [AZURE.SELECTOR]
- [Pregled][]
- [Portal][]
- [PowerShell][]
- [OSTALE][]

SQL Data Warehouse nudi lokalne i zemljopisnim vraća kao dio njegove podatke skladištu Izrada mogućnosti za oporavak. Koristite sigurnosnih kopija skladištu podataka da biste vratili skladištu podatkovne točke vraćanja u području primarni ili zemlj suvišnih sigurnosne kopije da biste vratili različite zemljopisnom području. U ovom se članku objašnjava specifičnosti vraćanja podataka skladištu.

## <a name="what-is-a-data-warehouse-restore"></a>Što je skladištu vraćanja podataka?

Vraćanje skladištu podataka je novi skladištu podataka stvorenoj iz sigurnosne kopije postojećeg ili skladištu izbrisane podatke. Skladištu vraćene podatke ponovno stvara sigurnosnu kopiju podataka skladištu u određeno vrijeme. Budući da SQL Data Warehouse je raspodijeljeno sustava, skladištu vraćanja podataka stvara se iz mnogo sigurnosne kopije datoteka koje se pohranjuju u Azure blob-ova. 

Vraćanje baze podataka je ključni dio sve poslovne continuity i Izrada strategije oporavak jer ponovno stvara podataka nakon slučajne oštećenja ili brisanja.

Dodatne informacije potražite u članku:

-  [SQL Data Warehouse sigurnosne kopije](sql-data-warehouse-backups.md)
-  [Pregled continuity tvrtke](../sql-database/sql-database-business-continuity.md)

## <a name="data-warehouse-restore-points"></a>Vraćanje točaka skladištu

Kao prednosti korištenja Azure Premium prostora za pohranu, SQL Data Warehouse koristi snimke blobova platforme Azure prostora za pohranu za sigurnosno kopiranje skladištu primarni podataka. Svaki snimke sadrži točku vraćanja koja predstavlja pokretanja snimke. Da biste vratili podataka skladištu, odaberite točku vraćanja i problema naredbu Vrati.  

SQL Data Warehouse uvijek vraća sigurnosne kopije novi skladištu podataka. Možete zadržati skladištu vraćene podatke i trenutnog ili izbrisali jedan od njih. Ako želite da biste zamijenili trenutno skladištu podataka na skladištu vraćene podatke, možete je preimenovati.

Ako vam je potrebna da biste vratili izbrisane ili pauzirano podataka skladištu, možete [stvoriti zahtjev za podršku možete](sql-data-warehouse-get-started-create-support-ticket.md). 

<!-- 
### Can I restore a deleted data warehouse?

Yes, you can restore the last available restore point.

Yes, for the next seven calendar days. When you delete a data warehouse, SQL Data Warehouse actually keeps the data warehouse and its snapshots for seven days just in case you need the data. After seven days, you won't be able to restore to any of the restore points. -->

## <a name="geo-redundant-restore"></a>Vraćanje suvišnih zemlj.

Ako koristite zemlj suvišnih prostora za pohranu, možete vratiti skladištu podataka u [paru podatkovnog centra](../best-practices-availability-paired-regions.md) u različitim zemljopisnom području. Vratit će se skladištu podataka iz zadnjeg dnevna sigurnosna kopija. 

## <a name="restore-timeline"></a>Vraćanje vremenske trake

Vraćanje baze podataka postavite dostupna vraćanja u zadnjih sedam dana. Brze snimke pokrenite svakih četiri do osam sati i dostupne su za sedam dana. Kada je snimka starije od sedam dana, što istekne, a njezine točke vraćanja više nije dostupan.

## <a name="restore-costs"></a>Vraćanje troškovi

Iznos za pohranu za skladištu vraćene podatke je naplaćeno brzinom Azure Premium prostora za pohranu. 

Ako zaustavite vraćene podatke skladištu vam se naplatiti za pohranu brzinom Azure Premium prostora za pohranu. Prednost pauze je vam se naplatiti za DWU računalno resurse.

Dodatne informacije o cijenama SQL Data Warehouse potražite u članku [SQL podataka skladištu cijene](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).

## <a name="uses-for-restore"></a>Koristi za vraćanje

Prvenstveno se koristi za vraćanje podataka skladištu je oporaviti podatke nakon gubitka ili oštećenja slučajne podataka.

Vraćanje skladištu podataka možete koristiti i da biste zadržali sigurnosnu kopiju dulje od sedam dana. Kada je vraćen sigurnosnog kopiranja, ste skladištu podataka putem Interneta, a možete zaustaviti beskonačno da biste spremili računalnim troškove. Pauzirano baze podataka uključuje naknade za pohranu brzinom Azure Premium prostora za pohranu. 

## <a name="related-topics"></a>Povezane teme

### <a name="scenarios"></a>Scenariji

- Pregled continuity tvrtke, potražite u članku [Pregled continuity tvrtke](../sql-database/sql-database-business-continuity.md)


<!-- ### Tasks -->

Da biste izvršili skladištu vraćanja podataka, vraćanje pomoću:

- Portal za Azure, pročitajte članak [Vraćanje podataka skladištu pomoću portala za Azure](sql-data-warehouse-restore-database-portal.md)
- Cmdleta ljuske PowerShell potražite u članku [Vraćanje podataka skladištu pomoću cmdleta ljuske PowerShell](sql-data-warehouse-restore-database-powershell.md)
- POSTAVITE API-ji, pročitajte članak [Vraćanje podataka skladištu pomoću REST API -ji](sql-data-warehouse-restore-database-rest-api.md)

<!-- ### Tutorials -->

<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
[Pregled]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[OSTALE]: ./sql-data-warehouse-restore-database-rest-api.md

<!--MSDN references-->


<!--Other Web references-->
