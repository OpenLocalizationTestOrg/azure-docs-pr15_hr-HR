<properties
   pageTitle="Učitavanje podataka iz sustava SQL Server u Azure SQL Data Warehouse (bcp) | Microsoft Azure"
   description="Za veličinom small podataka koristi bcp za izvoz podataka iz sustava SQL Server u plošnu datoteke i uvezli podatke izravno u Azure SQL Data Warehouse."
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
   ms.date="06/30/2016"
   ms.author="lodipalm;barbkess;sonyama"/>


# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-flat-files"></a>Učitavanje podataka iz sustava SQL Server u Azure SQL Data Warehouse (paušalni datoteke)

> [AZURE.SELECTOR]
- [SSIS](sql-data-warehouse-load-from-sql-server-with-integration-services.md)
- [PolyBase](sql-data-warehouse-load-from-sql-server-with-polybase.md)
- [BCP](sql-data-warehouse-load-from-sql-server-with-bcp.md)

Za small skupova podataka, možete koristiti uslužni bcp naredbenog retka za izvoz podataka iz sustava SQL Server, a zatim ga izravno na Azure SQL Data Warehouse.

U ovom ćete praktičnom vodiču koristit će bcp da biste:

- Izvoz tablice iz iz sustava SQL Server pomoću bcp jednom riječi (ili stvaranje jednostavne ogledne datoteke)
- Uvoz tablice s nehijerarhijskom datotekom u SQL Data Warehouse.
- Stvaranje Statistika učitavanja podataka.

>[AZURE.VIDEO loading-data-into-azure-sql-data-warehouse-with-bcp]

## <a name="before-you-begin"></a>Prije početka

### <a name="prerequisites"></a>Preduvjeti

Da biste se pomicali kroz ovaj Praktični vodič, potrebno je:

- SQL Data Warehouse baze podataka
- Bcp naredbenog retka uslužni instaliran
- Sqlcmd naredbenog retka uslužni instaliran

Uslužni programi bcp i sqlcmd možete preuzeti iz [Microsoftova centra za preuzimanje][].

### <a name="data-in-ascii-or-utf-16-format"></a>Podatke u obliku ASCII ili UTF-16

Ako pokušavate ovog praktičnog vodiča s vlastitim podacima, podataka mora pomoću ASCII ili UTF-16 kodiranje jer bcp ne podržava UTF-8. 

PolyBase podržava UTF-8, ali još ne podržava UTF-16. Imajte na umu da ako želite kombinirati bcp s PolyBase morat ćete pretvaranje podataka da biste UTF-8 nakon izvozi se iz sustava SQL Server. 


## <a name="1-create-a-destination-table"></a>1. stvaranje odredišne tablice

Definiranje tablice u SQL Data Warehouse koji će biti odredišne tablice opterećenja. Stupci u tablici mora odgovarati podataka u svakom retku podatkovne datoteke.

Da biste stvorili tablicu, otvorite naredbeni redak, a zatim pomoću sqlcmd.exe pokrenite sljedeću naredbu:


```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    CREATE TABLE DimDate2
    (
        DateId INT NOT NULL,
        CalendarQuarter TINYINT NOT NULL,
        FiscalQuarter TINYINT NOT NULL
    )
    WITH
    (
        CLUSTERED COLUMNSTORE INDEX,
        DISTRIBUTION = ROUND_ROBIN
    );
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

## <a name="4-create-statistics"></a>4. stvaranje Statistika

SQL Data Warehouse ne još nisu podršku automatsko stvaranje ili ažuriranje automatske statistike. Da biste ostvarili najbolje performanse upita, je važno da biste stvorili Statistika na sve stupce sve tablice nakon prve učitavanja ili kada dođe do znatno promjena u podacima. Detaljno objašnjenje Statistika potražite u članku [Statistika][]. 

Pokrenite sljedeću naredbu da biste stvorili Statistika na upravo učitati tablice.

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## <a name="5-export-data-from-sql-data-warehouse"></a>5. Izvoz podataka iz SQL Data Warehouse
Za zabavu, možete izvesti podatke koji se ponovno iz SQL Data Warehouse učitati.  Naredba izvoz jednak točno izvoz iz sustava SQL Server.

No postoji razlika u rezultatima. Budući da se podaci se pohranjuju u raspodijeljeno mjesta unutar SQL Data Warehouse prilikom izvoza podataka svaki čvor računalnim zapisuje ga podataka Izlazna datoteka. Redoslijed podataka u Izlazna datoteka je vjerojatno će biti razlikuje se od redoslijeda podaci u ulaznoj datoteci.

### <a name="export-a-table-and-compare-exported-results"></a>Izvoz tablice i usporedite izvezene rezultata

Da biste vidjeli izvezene podatke, otvorite naredbeni redak i pokrenite sljedeću naredbu pomoću vlastite parametara. Naziv poslužitelja je naziv Azure logičke SQL Server.

```sql
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
Možete provjeriti pravilno izvoza podataka tako da otvorite novu datoteku. Podatke u datoteci mora podudarati ispod teksta, no vjerojatno će se sortirati drugačijim redoslijedom:

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

### <a name="export-the-results-of-a-query"></a>Izvoz rezultata upita

Koristite funkciju **queryout** bcp izvoz rezultata upita umjesto izvoz cijelu tablicu. 

## <a name="next-steps"></a>Daljnji koraci
Pregled učitavanja, potražite u članku [učitavanja podataka u SQL Data Warehouse][].
Dodatne savjete za razvoj potražite u članku [Pregled SQL Data Warehouse razvoj][].
Potražite u članku [Pregled tablica][] ili [Stvaranje TABLICE sintaksa][] za dodatne informacije o stvaranju tablice na SQL Data Warehouse.

<!--Image references-->

<!--Article references-->

[Podatke učitali u SQL Data Warehouse]: ./sql-data-warehouse-overview-load.md
[Pregled SQL Data Warehouse razvoj]: ./sql-data-warehouse-overview-develop.md
[Pregled tablica]: ./sql-data-warehouse-tables-overview.md
[Statistika]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx
[Sintaksa za stvaranje TABLICE]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Microsoftova centra za preuzimanje]: https://www.microsoft.com/download/details.aspx?id=36433
