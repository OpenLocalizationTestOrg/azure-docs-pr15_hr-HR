<properties
   pageTitle="Učitavanje oglednim podacima u SQL Data Warehouse | Microsoft Azure"
   description="Učitavanje oglednim podacima u SQL Data Warehouse"
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
   ms.date="08/16/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

#<a name="load-sample-data-into-sql-data-warehouse"></a>Učitavanje oglednim podacima u SQL Data Warehouse

Slijedite ove jednostavne korake za učitavanje i upita baze podataka tvrtke Adventure Works uzorka. Te skripte prvo pokretanje izvješća pomoću sqlcmd SQL koje ćete stvarati tablice i prikaze. Kada stvorite tablice skripte će koristiti bcp da biste učitali podatke.  Ako još nemate sqlcmd i bcp instaliran, slijedite ove veze da biste [instalirali bcp][] i da biste [instalirali sqlcmd][].

##<a name="load-sample-data"></a>Učitavanje oglednih podataka

1. Preuzmite [Adventure Works ogledne skripte za SQL Data Warehouse][] zip datoteku.

2. Izdvajanje datoteka iz preuzete zip direktorij na lokalnom računalu.

3. Uređivanje aw_create.bat izdvojenu datoteku i postaviti sljedeće varijable lokacije na vrhu datoteke.  Ne zaboravite da biste napustili bez razmaka između na "=", a parametar.  U nastavku slijede Primjeri kako izgleda uređivanjem.

    ```
    server=mylogicalserver.database.windows.net
    user=mydwuser
    password=Mydwpassw0rd
    database=mydwdatabase
    ```

4. Iz cmd redak za Windows pokreće uređivati aw_create.bat.  Provjerite je li u mapi koju ste spremili uređivati verziju aw_create.bat.
Ova skripta će...
    * Odbacivanje sve Adventure Works tablicama ili prikazima koje već postoje u bazi podataka
    * Stvaranje Adventure Works tablica i prikaza
    * Učitavanje svaku tablicu Adventure Works pomoću bcp
    * Provjerite valjanost brojanja redaka za svaku tablicu Adventure Works
    * Prikupljanje statističkih podataka za svaki stupac za svaku tablicu Adventure Works


##<a name="query-sample-data"></a>Ogledni podaci za upite

Kada ste u SQL Data Warehouse učitan ogledne podatke možete brzo pokretanje nekoliko upita.  Da biste pokrenuli upit, povezivanje s novostvorenu Adventure Works bazu podataka u DW za SQL Azure pomoću Visual Studio i SSDT, kao što je opisano u dokumentu [upita s Visual Studio][] .

Primjer jednostavne iskaza select da biste dobili informacije Zaposlenici:

```sql
SELECT * FROM DimEmployee;
```

Primjer složeniji upit pomoću stavki kao što su GRUPE tako da biste vidjeli ukupni iznos za sve prodaju svakog dana:

```sql
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
FROM FactInternetSales
GROUP BY OrderDateKey
ORDER BY OrderDateKey;
```

Primjer odaberite s uvjetu WHERE radi filtriranja narudžbe iz prije određenog datuma:

```
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
FROM FactInternetSales
WHERE OrderDateKey > '20020801'
GROUP BY OrderDateKey
ORDER BY OrderDateKey;
```

SQL Data Warehouse podržava gotovo sve konstrukta T SQL koji podržava SQL Server.  Sve razlike su zabilježeni u našem dokumentaciju [migrirati kod][] .

## <a name="next-steps"></a>Daljnji koraci
Sad kad ste vodili izgledi pokušati neki upiti s oglednim podacima, pogledajte kako [razviti][], [Učitavanje][]ili [migrirati][] u SQL Data Warehouse.

<!--Image references-->

<!--Article references-->
[Migriranje]: sql-data-warehouse-overview-migrate.md
[razvoj]: sql-data-warehouse-overview-develop.md
[Učitavanje]: sql-data-warehouse-overview-load.md
[upit s Visual Studio]: sql-data-warehouse-query-visual-studio.md
[Migriranje kod]: sql-data-warehouse-migrate-code.md
[Instalacija bcp]: sql-data-warehouse-load-with-bcp.md
[Instalacija sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md

<!--Other Web references-->
[Adventure Works ogledne skripte za SQL Data Warehouse]: https://migrhoststorage.blob.core.windows.net/sqldwsample/AdventureWorksSQLDW2012.zip
