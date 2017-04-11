<properties
   pageTitle="Učitavanje podataka iz CSV datoteke u Databaase SQL Azure (bcp) | Microsoft Azure"
   description="Za veličinom small podataka koristi bcp da biste uvezli podatke u bazu podataka SQL Azure."
   services="sql-database"
   documentationCenter="NA"
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/13/2016"
   ms.author="carlrab"/>


# <a name="load-data-from-csv-into-azure-sql-data-warehouse-flat-files"></a>Učitavanje podataka iz CSV u Azure SQL Data Warehouse (paušalni datoteke)

Pomoćnog programa naredbenog retka bcp možete koristiti da biste uvezli podatke iz CSV datoteke u bazu podataka SQL Azure.

## <a name="before-you-begin"></a>Prije početka

### <a name="prerequisites"></a>Preduvjeti

Da biste se pomicali kroz ovaj Praktični vodič, potrebno je:

- Logička poslužitelj baze podataka SQL Azure i baze podataka
- Bcp naredbenog retka uslužni instaliran
- Sqlcmd naredbenog retka uslužni instaliran

Uslužni programi bcp i sqlcmd možete preuzeti iz [Microsoftova centra za preuzimanje][].

### <a name="data-in-ascii-or-utf-16-format"></a>Podatke u obliku ASCII ili UTF-16

Ako pokušavate ovog praktičnog vodiča s vlastitim podacima, podataka mora pomoću ASCII ili UTF-16 kodiranje jer bcp ne podržava UTF-8. 

## <a name="1-create-a-destination-table"></a>1. stvaranje odredišne tablice

Definiranje tablice u bazi podataka SQL kao odredišne tablice. Stupci u tablici mora odgovarati podataka u svakom retku podatkovne datoteke.

Da biste stvorili tablicu, otvorite naredbeni redak, a zatim pomoću sqlcmd.exe pokrenite sljedeću naredbu:


```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    CREATE TABLE DimDate2
    (
        DateId INT NOT NULL,
        CalendarQuarter TINYINT NOT NULL,
        FiscalQuarter TINYINT NOT NULL
    )
    ;
"
```


## <a name="2-create-a-source-data-file"></a>2. stvorili datoteku izvora podataka

Otvorite Blok za pisanje i kopirajte sljedeće retke podataka u novu tekstnu datoteku, a zatim spremanje datoteke u lokalnu temp imenik, C:\Temp\DimDate2.txt. U ovom podaci u obliku ASCII.

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

(Neobavezno) Da biste izvezli svoje podatke iz baze podataka sustava SQL Server, otvorite naredbeni redak, a zatim pokrenite sljedeću naredbu. TableName, naziv poslužitelja, ImeBazePodataka, korisničko ime i lozinku zamijenite vlastitim podacima.

```sql
bcp <TableName> out C:\Temp\DimDate2_export.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <Password> -q -c -t ','
```

## <a name="3-load-the-data"></a>3. učitali podatke
Da biste učitali podatke, otvorite naredbeni redak, a zatim pokrenite sljedeću naredbu zamjenu vrijednosti za naziv poslužitelja, naziv baze podataka, korisničko ime i lozinka s vlastitim podacima.

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <password> -q -c -t  ','
```

Da biste provjerili pravilno učitati podatke, koristite sljedeću naredbu

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

Rezultati trebao bi izgledati ovako:

DateId |Kalendarski kvartal |FiscalQuarter
----------- |--------------- |-------------
20150101 |1 |3
20150201 |1 |3
20150301 |1 |3
20150401 |2 |4
20150501 |2 |4
20150601 |2 |4
20150701 |3 |1
20150801 |3 |1
20150801 |3 |1
20151001 |4 |2
20151101 |4 |2
20151201 |4 |2


## <a name="next-steps"></a>Daljnji koraci

Migracija baze podataka sustava SQL Server, potražite u članku [Migracija baze podataka sustava SQL Server](sql-database-cloud-migrate.md).

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx
[CREATE TABLE syntax]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Microsoftova centra za preuzimanje]: https://www.microsoft.com/download/details.aspx?id=36433
