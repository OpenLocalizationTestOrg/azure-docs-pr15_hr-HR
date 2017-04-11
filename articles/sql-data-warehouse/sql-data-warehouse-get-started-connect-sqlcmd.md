<properties
   pageTitle="Upit Azure SQL Data Warehouse (sqlcmd) | Microsoft Azure"
   description="Slanje upita Azure SQL Data Warehouse s sqlcmd Utility naredbenog retka."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/06/2016"
   ms.author="barbkess;sonyama"/>

# <a name="query-azure-sql-data-warehouse-sqlcmd"></a>Upit Azure SQL Data Warehouse (sqlcmd)

> [AZURE.SELECTOR]
- [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
- [Azure strojnog učenja](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
- [Visual Studio](sql-data-warehouse-query-visual-studio.md)
- [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 

Ovaj vodič koristi pomoćnog programa naredbenog retka [sqlcmd][] upit programa Data Warehouse SQL Azure.  

## <a name="1-connect"></a>1. povezivanje

Za početak rada s [sqlcmd][], otvorite naredbeni redak i unesite **sqlcmd** nakon čega slijedi niz za povezivanje baze podataka SQL Data Warehouse. Niz za povezivanje zahtijeva sljedećih parametara:

+ **Server (-S):** Poslužitelj u obrascu `<`naziv poslužitelja`>`. database.windows.net
+ **Baze podataka (-d):** Naziv baze podataka.
+ **Omogući navodnike identifikatore (-li):** Ponuđeni identifikatora mora biti omogućen za povezivanje s instancom SQL Data Warehouse.

Da biste koristili SQL Server provjeru autentičnosti, morate dodati parametre korisničkog imena i lozinke:

+ **Korisnika (-U):** Korisnik poslužitelja u obrascu `<`korisnika`>`
+ **Lozinke (-P):** Lozinka pridružene korisniku.

Na primjer, niz za povezivanje može izgledati ovako:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
```

Da biste koristili Azure Active Directory integriranu provjeru autentičnosti, morate dodati parametre Azure Active Directory:

+ **Provjere autentičnosti azure Active Directory (-G):** pomoću servisa Azure Active Directory za provjeru autentičnosti

Na primjer, niz za povezivanje može izgledati ovako:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -G -I
```

> [AZURE.NOTE] Morate [omogućiti provjere autentičnosti Azure Active Directory](sql-data-warehouse-authentication.md) za provjeru autentičnosti pomoću servisa Active Directory.

## <a name="2-query"></a>2. upit za

Nakon veze, možete izdati sve podržane Transact-SQL naredbe protiv instanci.  U ovom primjeru Upiti poslani su interaktivni način rada.

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
1> SELECT name FROM sys.tables;
2> GO
3> QUIT
```

Ovi Sljedeći primjeri pokazuju pokretanja upita u načinu za obradu pomoću mogućnosti – pitanja ili piping sustava SQL na sqlcmd.

```sql
sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I -Q "SELECT name FROM sys.tables;"
```

```sql
"SELECT name FROM sys.tables;" | sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I > .\tables.out
```

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o pojedinosti o mogućnostima koje su dostupne u sqlcmd potražite [dokumentaciju sqlcmd][sqlcmd] .

<!--Image references-->

<!--Article references-->

<!--MSDN references--> 
[sqlcmd]: https://msdn.microsoft.com/library/ms162773.aspx
[Azure portal]: https://portal.azure.com

<!--Other Web references-->
