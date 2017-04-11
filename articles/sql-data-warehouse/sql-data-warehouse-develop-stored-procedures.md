<properties
   pageTitle="Pohranjene procedure u SQL Data Warehouse | Microsoft Azure"
   description="Savjeti za implementaciju pohranjene procedure u skladištu podataka za SQL Azure za razvoj rješenja."
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
   ms.date="06/30/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="stored-procedures-in-sql-data-warehouse"></a>Pohranjene procedure u skladištu podataka za SQL

SQL Data Warehouse podržava mnoge Transact-SQL značajke koje se nalaze u sustavu SQL Server. Više važno je napomenuti da postoje skaliranje određene značajke koje želimo odražava Maksimiziranje performanse vašeg rješenja.

Međutim, da biste zadržali promjenom veličine i performanse SQL Data Warehouse postoji su i nekih značajki i funkcija koje su razlike behavioral i ostala polja koja nisu podržane.

U ovom se članku objašnjava kako implementirati pohranjene procedure unutar SQL Data Warehouse.

## <a name="introducing-stored-procedures"></a>Uvod u pohranjene procedure
Pohranjene procedure su odlične za encapsulating SQL koda; Spremanje blizu podataka u skladištu podataka. Po encapsulating kod u kojima se upravlja jedinice pohranjene procedure pomoći razvojni inženjeri modularize njihova rješenja olakšavanja veći re-usability kod. Svaki pohranjena procedura možete prihvatiti parametara da biste ih još fleksibilne.

SQL Data Warehouse nudi pojednostavnjeni i pojednostavnjeno pohranjena procedura implementacije. Veličina najveće razlike u usporedbi sa sustavom SQL Server je pohranjena procedura nije unaprijed kompilirane kod. U skladištima podataka ne možemo zabrinuti obično manje s vremenom sastavljanje. Nije važno da kod pohranjena procedura pravilno optimised kada radi protiv velike količine podataka. Cilj je da biste spremili sate, minute i sekunde ne milisekundi. Zbog toga više korisno je spremnici za SQL logike smatrati pohranjene procedure.     

Kada se izvršava SQL Data Warehouse pohranjena procedura SQL naredbe su raščlaniti, prevesti, a optimizirana vrijeme izvođenja. Tijekom tog postupka svaku izjavu pretvara se u raspodijeljeno upita. Da biste poslali upit razlikuje se SQL kod koji se zapravo izvršava na temelju podataka.

## <a name="nesting-stored-procedures"></a>Ugnježđivanje pohranjene procedure
Kada pohranjene procedure pozvati druge pohranjene procedure ili izvoditi dinamički sql, a zatim na unutarnji pohranjene procedure ili kod poziva je rečeno ugnježđivanje.

SQL Data Warehouse podržava najviše 8 razina gniježđenja. Ovo je malo drugačiju sa sustavom SQL Server. Razina ugnijezditi u sustavu SQL Server jest 32.

Poziv gornje razine pohranjena procedura daje rezultat u obliku ugnijezditi razine 1

```sql
EXEC prc_nesting
```
Ako pohranjena procedura čini i drugi izvršavanja PRVE poziva pa to će se povećati razinu ugnijezditi do 2
```sql
CREATE PROCEDURE prc_nesting
AS
EXEC prc_nesting_2  -- This call is nest level 2
GO
EXEC prc_nesting
```
Ako drugi postupak zatim izvršava neke dinamičke sql, a zatim to će se povećati razinu ugnijezditi 3
```sql
CREATE PROCEDURE prc_nesting_2
AS
EXEC sp_executesql 'SELECT 'another nest level'  -- This call is nest level 2
GO
EXEC prc_nesting
```

Napomena SQL Data Warehouse ne podržavamo @@NESTLEVEL. Morat ćete pratiti u razinu zaštite ugnijezditi sami. Je vjerojatno kliknete 8 ugnijezditi razine ograničenje, no ako vidite morat ćete ponovno rad kodu i "stopi" ga tako da stane u to ograničenje.

## <a name="insertexecute"></a>UMETANJE... IZVRŠAVANJE
SQL Data Warehouse ne dopušta zauzeti skup rezultata pohranjena procedura pomoću naredbi programa INSERT. No postoji zamjenski pristup koji se mogu koristiti.

Pogledajte u sljedećem članku [privremenih tablica] na primjer kako to učiniti.

## <a name="limitations"></a>Ograničenja

Postoje neke aspekte Transact-SQL pohranjene procedure koje nije implementirana u SQL Data Warehouse.

Mogućnosti su sljedeće:

- privremeni pohranjene procedure
- numerirani pohranjene procedure
- Prošireni pohranjene procedure
- CLR pohranjene procedure
- mogućnost šifriranja
- mogućnost replikacije
- Parametri s tabličnim vrijednostima
- Parametri samo za čitanje
- Zadani parametri
- izvršavanje konteksta
- Vraćanje izjava

## <a name="next-steps"></a>Daljnji koraci
Dodatne savjete za razvoj potražite u članku [Pregled razvoj][].

<!--Image references-->

<!--Article references-->
[privremenih tablica]: ./sql-data-warehouse-tables-temporary.md#modularizing-code
[Pregled razvoj]: ./sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[nest level]: https://msdn.microsoft.com/library/ms187371.aspx

<!--Other Web references-->
