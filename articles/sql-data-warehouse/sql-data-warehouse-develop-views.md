<properties
   pageTitle="Prikazi u SQL Data Warehouse | Microsoft Azure"
   description="Savjeti za korištenje Transact-SQL prikaza programa Azure SQL Data Warehouse za razvoj rješenja."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="07/01/2016"
   ms.author="jrj;barbkess;sonyama"/>


# <a name="views-in-sql-data-warehouse"></a>Prikazi u skladištu podataka za SQL

U SQL Data Warehouse osobito korisni su prikazi. Može se koristiti na mnogo različitih načina poboljšava se kvaliteta vašeg rješenja.  U ovom se članku ističe nekoliko primjera kako obogaćivanje rješenje s prikaza, kao i ograničenja koja treba smatrati.

> [AZURE.NOTE] Sintaksa za `CREATE VIEW` se spominju u ovom članku. Pogledajte članak [Stvaranje PRIKAZA][] na MSDN-ovu referenca informacije.

## <a name="architectural-abstraction"></a>Arhitektonski apstrakcije
Vrlo uobičajeni obrazac za aplikaciju je za ponovno stvaranje tablica pomoću stvaranje TABLICE kao odaberite (CTAS) iza koje slijedi objekt Preimenovanje uzorak whilst učitavanja podataka.

U primjeru u nastavku dodaje novi datum zapisa dimenziju datum. Imajte na umu kako novu tabble DimDate_New, stvaranja i zatim preimenovati da biste zamijenili izvornu verziju tablice.

```sql
CREATE TABLE dbo.DimDate_New
WITH (DISTRIBUTION = ROUND_ROBIN
, CLUSTERED INDEX (DateKey ASC)
)
AS
SELECT *
FROM   dbo.DimDate  AS prod
UNION ALL
SELECT *
FROM   dbo.DimDate_stg AS stg
;

RENAME OBJECT DimDate TO DimDate_Old;
RENAME OBJECT DimDate_New TO DimDate;

```

Međutim, ovaj pristup može rezultirati tablice koji se pojavljuje i nestaje za vrijeme iz prikaza na korisnika, kao i poruke o pogreškama "tablica ne postoji". Prikazi se može koristiti da biste korisnicima omogućili sloj dosljedan prezentacije whilst preimenuju se podlozi objekte. Unosom korisnici pristup podacima kroz u prikazima, znači da korisnici ne moraju imati vidljivost tablice u podlozi. To omogućuje dosljedno korisničko sučelje dok jamči da podatke skladištu dizajnera možete poslovanja u podatkovni model i poboljšajte performanse pomoću CTAS tijekom učitavanja postupak podatke.    

## <a name="performance-optimization"></a>Optimizaciju performansi
Prikazi koristi možete se i da biste nametnuli performanse Optimizirano spojeva između tablica. Na primjer, prikaz možete ugraditi ključa suvišnih raspodjele kao dio kriterij spojno da biste minimizirali premještanje podataka.  Još jedna prednost prikaza može biti da biste nametnuli određenog upita ili spojno podsjetnik. Korištenje prikaza na taj način jamčiti da spojevi uvijek se izvode optimalnih način koji se izbjegava potrebe za korisnike koji žele Imajte na umu točan konstrukta za svoje spojeva.

## <a name="limitations"></a>Ograničenja
Prikazi u SQL Data Warehouse su samo metapodataka.  Zbog sljedećih mogućnosti nisu dostupne:

-   Ne postoji mogućnost povezivanja shema
-   Osnovni tablica nije moguće ažurirati putem prikaza
-   Prikazi nije moguće stvoriti putem privremene tablice
-   Ne postoji podrška za PROŠIRIVANJE / NOEXPAND Savjeti
-   Ne postoje indeksiranih prikazi u skladištu podataka za SQL


## <a name="next-steps"></a>Daljnji koraci
Dodatne savjete za razvoj potražite u članku [Pregled SQL Data Warehouse razvoj][].
Za `CREATE VIEW` sintaksa pogledajte [Stvori PRIKAZ][].

<!--Image references-->

<!--Article references-->
[Pregled SQL Data Warehouse razvoj]: ./sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[STVARANJE PRIKAZA]: https://msdn.microsoft.com/en-us/library/ms187956.aspx

<!--Other Web references-->
