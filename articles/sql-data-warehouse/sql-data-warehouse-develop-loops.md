<properties
   pageTitle="Petlje u SQL Data Warehouse | Microsoft Azure"
   description="Savjeti za Transact-SQL petlje i zamjene pokazivači u skladištu podataka za SQL Azure za razvoj rješenja."
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
   ms.date="06/14/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="loops-in-sql-data-warehouse"></a>Petlje u SQL Data Warehouse
SQL Data Warehouse podržava Ponavljaj [DOK][] više puta izvođenje izjava blokova. To će i dalje za sve dok se određeni uvjeti istiniti ili dok kod posebno prekida pomoću petlje u `BREAK` ključnih riječi. Petlje osobito korisni su za zamjenu pokazivači koji su definirani u SQL kod. Srećom, gotovo sve pokazivači koji se pišu u SQL kodu su fokusiranje, pročitajte samo različite. Petlje [DOK] stoga su odličan zamjenski ako sami morate zamijeniti nešto pronaći.

## <a name="leveraging-loops-and-replacing-cursors-in-sql-data-warehouse"></a>Korištenje petlje i zamjene pokazivači u skladištu podataka za SQL
Međutim, prije čitanje u zaglavlje zamolite si sljedeća pitanja: "Nije moguće ovaj pokazivač ponovno pisana da biste koristili operacije skup koji se temelji?". U mnogim slučajevima odgovor neće biti da i često je najbolji način. Operacija skup koji se temelji često izvodi znatno brže nego je pristup s iteracijama, redak po redak.

Brzo naprijed pokazivači samo za čitanje možete jednostavno zamijeniti konstrukta petlje. U nastavku je jednostavnog primjera. U ovom se primjeru kod ažurira statističke podatke za svaku tablicu u bazi podataka. Po iterating preko tablica u petlje možemo izvršiti svaku naredbu u nizu.

Prvo, stvorite privremenu tablicu koja sadrži broj jedinstvenih redaka koristi za prepoznavanje pojedinačne naredbe:

```
CREATE TABLE #tbl
WITH
( DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT  ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) AS Sequence
,       [name]
,       'UPDATE STATISTICS '+QUOTENAME([name]) AS sql_code
FROM    sys.tables
;
```

Drugo, pokretanje varijable potrebne za izvršavanje petlje:

```
DECLARE @nbr_statements INT = (SELECT COUNT(*) FROM #tbl)
,       @i INT = 1
;
```

Sada Ponavljaj putem naredbe izvršavanja ih jedan po jedan:

```
WHILE   @i <= @nbr_statements
BEGIN
    DECLARE @sql_code NVARCHAR(4000) = (SELECT sql_code FROM #tbl WHERE Sequence = @i);
    EXEC    sp_executesql @sql_code;
    SET     @i +=1;
END
```

Na kraju ispustite privremena stvorili u prvom koraku

```
DROP TABLE #tbl;
```


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="next-steps"></a>Daljnji koraci
Dodatne savjete za razvoj potražite u članku [Pregled razvoj][].

<!--Image references-->

<!--Article references-->
[Pregled razvoj]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[VRIJEME]: https://msdn.microsoft.com/library/ms178642.aspx


<!--Other Web references-->
