<properties
   pageTitle="Korištenje bcp da biste učitali podatke u SQL Data Warehouse | Microsoft Azure"
   description="Naučite koje bcp te kako ga koristiti za podatke skladištenje scenarijima."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="twounder"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/10/2016"
   ms.author="mausher;barbkess;sonyama"/>


# <a name="load-data-with-bcp"></a>Učitavanje podataka s bcp

> [AZURE.SELECTOR]
- [Redgate](sql-data-warehouse-load-with-redgate.md)  
- [Tvorničke podataka](sql-data-warehouse-get-started-load-with-azure-data-factory.md)  
- [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)  
- [BCP](sql-data-warehouse-load-with-bcp.md)


**[BCP][]** je uslužni program naredbenog retka skupno opterećenja koji omogućuje vam da biste kopirali podatke između sustava SQL Server, podatkovne datoteke i SQL Data Warehouse. Koristite bcp da biste uvezli velikog broja redaka u SQL Data Warehouse tablice ili izvoz podataka iz tablica sustava SQL Server u podatkovne datoteke. Osim kada se koristi s mogućnošću queryout, bcp zahtijeva poznavanje Transact-SQL.

BCP je brz i jednostavan način možete premjestiti manje skupa podataka u i iz baze podataka SQL Data Warehouse. Exact količinu podataka koji se preporučuje da biste Učitaj/izdvojiti putem bcp ovise na mrežne veze centar za Azure podataka.  Općenito govoreći, dimenzije tablica možete učitati i lako dobivenih s bcp, no bcp se preporučuje za učitavanje ili izdvajanje velike količine podataka.  Polybase je preporučeni alat za učitavanje i izdvajanje velike količine podataka kao bolje posao korištenje arhitektura massively paralelno obrada SQL Data Warehouse.

Bcp omogućuje sljedeće:

- Korištenje programa jednostavne naredbenog retka za da biste učitali podatke u SQL Data Warehouse.
- Korištenje jednostavne programa naredbenog retka za izdvajanje podataka iz SQL Data Warehouse.

Pomoću ovog praktičnog vodiča vidjet ćete kako da biste:

- Uvoz podataka u tablicu pomoću na bcp u naredbi
- Izvoz podataka iz tablice uisng bcp jednom riječi

>[AZURE.VIDEO loading-data-into-azure-sql-data-warehouse-with-bcp]

## <a name="prerequisites"></a>Preduvjeti

Da biste se pomicali kroz ovaj Praktični vodič, potrebno je:

- SQL Data Warehouse baze podataka
- Uslužni naredbenog retka bcp instaliran
- Uslužni naredbenog retka SQLCMD instalacije

>[AZURE.NOTE] Uslužni programi bcp i sqlcmd možete preuzeti iz [Microsoftova centra za preuzimanje][].

## <a name="import-data-into-sql-data-warehouse"></a>Uvoz podataka u SQL Data Warehouse

U ovom ćete praktičnom vodiču će stvaranje tablice u skladištu podataka za SQL Azure i uvezli podatke u tablici.

### <a name="step-1-create-a-table-in-azure-sql-data-warehouse"></a>Korak 1: Stvaranje tablice u skladištu podataka za SQL Azure

Pokrenite sljedeći upit za stvaranje tablice na vašoj instanci pomoću sqlcmd iz naredbenog retka:

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

>[AZURE.NOTE] Potražite u članku [Pregled tablica][] ili [Stvaranje TABLICE sintaksa][] dodatne informacije o stvaranju tablice na SQL Data Warehouse i mogućnosti koje su dostupne u OVIM uvjetima.

### <a name="step-2-create-a-source-data-file"></a>Korak 2: Stvorili datoteku izvora podataka

Otvorite Blok za pisanje i kopirajte sljedeće retke podataka u novu tekstnu datoteku, a zatim spremanje datoteke u lokalnu temp imenik, C:\Temp\DimDate2.txt.

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

> [AZURE.NOTE] Nije važno Imajte na umu te bcp.exe ne podržava šifriranje datoteke UTF-8. Koristite ASCII datoteke ili datoteke UTF-16 kodirani prilikom korištenja bcp.exe.

### <a name="step-3-connect-and-import-the-data"></a>Korak 3: Povezivanje i uvoz podataka
Korištenje bcp, možete povezati i uvoz podataka pomoću sljedeće naredbe za zamjenu vrijednosti po potrebi:

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t  ','
```

Možete provjeriti podatke učitana tako da pokrenete sljedeći upit pomoću sqlcmd:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

To mora vratiti sljedeće rezultate:

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

### <a name="step-4-create-statistics-on-your-newly-loaded-data"></a>Korak 4: Stvaranje Statistika na upravo učitavanja podataka

Azure SQL Data Warehouse ne još nisu podršku automatsko stvaranje i automatsko ažuriranje Statistika. Da biste ostvarili najbolje performanse zatražite od upite, važno je da Statistika se mogu stvarati na sve stupce sve tablice nakon prve učitavanja ili znatno promjene se pojavljuju u podacima. Detaljno objašnjenje Statistika u sintaksi [statističkih podataka][] u grupi razvoju tema. U nastavku je brzi primjer kako stvoriti statistike o na tabled učita u ovom primjeru

Izvođenje sljedeće naredbe za stvaranje STATISTIKA iz sqlcmd upit:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## <a name="export-data-from-sql-data-warehouse"></a>Izvoz podataka iz SQL Data Warehouse
U ovom ćete praktičnom vodiču ćete stvoriti podatkovne datoteke iz tablice u SQL Data Warehouse. Ne možemo će izvoz podataka koju smo stvorili iznad u novu podatkovnu datoteku pod nazivom DimDate2_export.txt.

### <a name="step-1-export-the-data"></a>Korak 1: Izvoz podataka

Korištenje uslužni bcp, možete povezivanje i izvoz podataka pomoću sljedeće naredbe za zamjenu vrijednosti po potrebi:

```sql
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
Možete provjeriti pravilno izvoza podataka tako da otvorite novu datoteku. Podatke u datoteci moraju se podudarati ispod teksta:

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

>[AZURE.NOTE] Zbog prirode raspodijeljeno sustavima, redoslijed podataka možda neće biti jednaki preko baze podataka SQL Data Warehouse. Druga je mogućnost da biste koristili funkciju **queryout** bcp pisanje upit izdvajanja umjesto izvoz cijelu tablicu.

## <a name="next-steps"></a>Daljnji koraci
Pregled učitavanja, potražite u članku [učitavanja podataka u SQL Data Warehouse][].
Dodatne savjete za razvoj potražite u članku [Pregled SQL Data Warehouse razvoj][].

<!--Image references-->

<!--Article references-->

[Podatke učitali u SQL Data Warehouse]: ./sql-data-warehouse-overview-load.md
[Pregled SQL Data Warehouse razvoj]: ./sql-data-warehouse-overview-develop.md
[Pregled tablica]: ./sql-data-warehouse-tables-overview.md
[Statistika]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->
[BCP]: https://msdn.microsoft.com/library/ms162802.aspx
[Sintaksa za stvaranje TABLICE]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Microsoftova centra za preuzimanje]: https://www.microsoft.com/download/details.aspx?id=36433
