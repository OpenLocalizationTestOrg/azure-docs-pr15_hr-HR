<properties
   pageTitle="Vodič za korištenje PolyBase u SQL Data Warehouse | Microsoft Azure"
   description="Upute i preporuke za upotrebu PolyBase u SQL Data Warehouse scenarijima."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="ckarst"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/30/2016"
   ms.author="cakarst;barbkess;sonyama"/>


# <a name="guide-for-using-polybase-in-sql-data-warehouse"></a>Vodič za korištenje PolyBase u SQL Data Warehouse

Ovaj vodič omogućuje praktično informacije o korištenju PolyBase u SQL Data Warehouse.

Početak rada potražite u članku vodič [učitavanja podataka pomoću PolyBase][] .


## <a name="rotating-storage-keys"></a>Rotiranje ključeva za pohranu

S vremena na vrijeme želite promijeniti tipka za pristup sa spremištem blobova sigurnosnih vam razloga.

Većina Elegantna način da biste izvršili taj zadatak tako da slijedite postupak naziva "rotiranje tipke". Možda ste primijetili sadrži dvije tipke za pohranu za vaš račun spremište blobova platforme. To je tako da možete prijelaza

Rotiranje ključeva račun za Azure prostora za pohranu se jednostavno tri koraka

1. Stvaranje drugog baze podataka iz djelokruga vjerodajnica na temelju tipka za pristup sekundarne prostora za pohranu
2. Stvaranje drugog vanjskog izvora podataka utemeljena ove novu vjerodajnice
3. Odbacivanje i stvoriti vanjski tablice koja pokazuje na novi vanjski izvor podataka

Kada ste migrirati sve vanjske tablica s vanjskim izvorom podataka, a zatim možete obavljati zadatke za čišćenje:

1. Padajuće prvi vanjskog izvora podataka
2. Padajuće prve baze podataka iz djelokruga vjerodajnica na temelju tipka za pristup primarni prostora za pohranu
3. Prijavite se Azure i Obnovi jeste li spremni za sljedeći put primarni tipkovni prečac

## <a name="query-azure-blob-storage-data"></a>Spremište blobova platforme Azure upita za podatke
Upite odabiranja vanjske tablice pomoću naziv tablice jednostavno kao da je relacijski tablice.

```sql
-- Query Azure storage resident data via external table.
SELECT * FROM [ext].[CarSensor_Data]
;
```

> [AZURE.NOTE] Upit na vanjsku tablicu uspijeva uz poruku o pogrešci *"Upita prekinutog – Najveća odbacivanje Prag je otvoriti tijekom čitanja iz vanjskog izvora"*. To označava da vanjskih podataka sadrži *prljav* zapisa. Zapis podataka se smatra "prljav ako stvarnih podataka vrste/broj stupaca ne odgovaraju definicije stupaca vanjske tablice ili ako ne u skladu s podacima u obliku navedenom vanjske datoteke. Da biste riješili taj problem, provjerite koristite li točan vanjska tablica i vanjske datoteke oblika definicije i vanjskih podataka u skladu te definicije. U slučaju da su prljav podskup zapisa vanjskih podataka, možete odabrati da biste odbacili ove zapise o korištenju upita pomoću mogućnosti Odbaci stvaranje VANJSKIH DDL TABLICE.


## <a name="load-data-from-azure-blob-storage"></a>Učitavanje podataka iz spremišta blobova platforme Azure
U ovom se primjeru učitava podatke iz spremišta blobova platforme Azure SQL Data Warehouse bazu podataka.

Spremanje podataka izravno uklanja vrijeme prijenos podataka za upite. Spremanje podataka s indeksom columnstore poboljšava performanse upita za analizu upiti prema do 10 x.

Ovaj primjer koristi naredbe za stvaranje SELECT kao tablicu da biste učitali podatke. Nova tablica nasljeđuje stupaca imenovan u upitu. Vrste podataka u tim stupcima nasljeđuje iz definicije vanjska tablica.

STVORITE tablicu kao odaberite je na Visoko performant Transact-SQL naredbi koja učitava podataka paralelno sve računalnim čvorove SQL Data Warehouse.  Izvorno je razvio za modul massively paralelno obrada (MPP) u sustavu platformu analize i sada se nalazi u SQL Data Warehouse.

```sql
-- Load data from Azure blob storage to SQL Data Warehouse

CREATE TABLE [dbo].[Customer_Speed]
WITH
(   
    CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([CarSensor_Data].[CustomerKey])
)
AS
SELECT *
FROM   [ext].[CarSensor_Data]
;
```

U odjeljku [Stvaranje TABLICE kao odabir (- SQL transakcija)][].

## <a name="create-statistics-on-newly-loaded-data"></a>Stvaranje Statistika upravo učitavanja podataka

Azure SQL Data Warehouse ne još nisu podršku automatsko stvaranje i automatsko ažuriranje Statistika.  Da biste ostvarili najbolje performanse zatražite od upite, važno je da Statistika se mogu stvarati na sve stupce sve tablice nakon prve učitavanja ili znatno promjene se pojavljuju u podacima.  Detaljno objašnjenje Statistika u sintaksi [Statistika][] u grupi razvoju tema.  U nastavku je brzi primjer kako stvoriti statistike o na tabled učita u ovom primjeru.

```sql
create statistics [SensorKey] on [Customer_Speed] ([SensorKey]);
create statistics [CustomerKey] on [Customer_Speed] ([CustomerKey]);
create statistics [GeographyKey] on [Customer_Speed] ([GeographyKey]);
create statistics [Speed] on [Customer_Speed] ([Speed]);
create statistics [YearMeasured] on [Customer_Speed] ([YearMeasured]);
```

## <a name="export-data-to-azure-blob-storage"></a>Izvoz podataka u spremište blobova platforme Azure
U ovom se odjeljku objašnjava izvoz podataka iz SQL Data Warehouse u spremište blobova platforme Azure. U ovom se primjeru koristi stvaranje VANJSKIH tablicu kao odaberite što je s iznimno performant Transact-SQL naredbe za izvoz podataka paralelno iz sve čvorove računalnim.

Sljedeći primjer stvara vanjsku tablicu Weblogs2014 pomoću definicije stupaca i podatke iz vlasnika baze podataka. Tablica weblogovi. Definicija vanjskog tablice pohranjena u SQL Data Warehouse i izvoze se rezultati naredbe SELECT direktorij "/ arhiviranje/log2014 /" u kontejneru blob određen izvor podataka. Podatke izvozi se u obliku datoteke određeni tekst.

```sql
CREATE EXTERNAL TABLE Weblogs2014 WITH
(
    LOCATION='/archive/log2014/',
    DATA_SOURCE=azure_storage,
    FILE_FORMAT=text_file_format
)
AS
SELECT
    Uri,
    DateRequested
FROM
    dbo.Weblogs
WHERE
    1=1
    AND DateRequested > '12/31/2013'
    AND DateRequested < '01/01/2015';
```


## <a name="working-around-the-polybase-utf-8-requirement"></a>Rad oko obavezne PolyBase UTF-8
Pri izlaganje PolyBase podržava učitavanje podatkovnih datoteka koje su UTF-8 kodiran. Pomoću UTF-8 isti znak kodiranje kao ASCII PolyBase će podršku učitavanja podataka ASCII kodiran. Međutim, PolyBase ne podržava druge kodiranje znakova kao što su UTF-16 / Unicode ili Prošireni ASCII znakova. Prošireni ASCII sadrži znakove naglaske kao što su prijeglas koji nije zajednička njemački.

Da biste riješili taj zahtjev je ponovno pisanje UTF-8 šifriranje najbolji odgovor.

Da biste to učinili na nekoliko načina. U nastavku su dva načina prikaza pomoću komponente Powershell:

### <a name="simple-example-for-small-files"></a>Primjer jednostavne za male datoteke

U nastavku je jednostavno jedan redak skriptu Powershell koji stvara datoteku.

```PowerShell
Get-Content <input_file_name> -Encoding Unicode | Set-Content <output_file_name> -Encoding utf8
```

Međutim, whilst to je jednostavno ponovno kodiranje podataka je po nisu means na najučinkovitiji. Io strujanje primjeru u nastavku je vrlo, mnogo brže i postiže isti rezultat.

### <a name="io-streaming-example-for-larger-files"></a>Primjer IO strujanje za veće datoteke

Uzorak koda ispod je složenije, ali kao što je strujanja redaka podataka iz izvora ciljnim ona je mnogo učinkovitije. Koristite ovaj pristup za veće datoteke.

```PowerShell
#Static variables
$ascii = [System.Text.Encoding]::ASCII
$utf16le = [System.Text.Encoding]::Unicode
$utf8 = [System.Text.Encoding]::UTF8
$ansi = [System.Text.Encoding]::Default
$append = $False

#Set source file path and file name
$src = [System.IO.Path]::Combine("C:\input_file_path\","input_file_name.txt")

#Set source file encoding (using list above)
$src_enc = $ansi

#Set target file path and file name
$tgt = [System.IO.Path]::Combine("C:\output_file_path\","output_file_name.txt")

#Set target file encoding (using list above)
$tgt_enc = $utf8

$read = New-Object System.IO.StreamReader($src,$src_enc)
$write = New-Object System.IO.StreamWriter($tgt,$append,$tgt_enc)

while ($read.Peek() -ne -1)
{
    $line = $read.ReadLine();
    $write.WriteLine($line);
}
$read.Close()
$read.Dispose()
$write.Close()
$write.Dispose()
```

## <a name="next-steps"></a>Daljnji koraci
Dodatne informacije o premještanju podataka SQL Data Warehouse potražite u članku [Pregled za migraciju podataka][].

<!--Image references-->

<!--Article references-->
[Load data with bcp]: ./sql-data-warehouse-load-with-bcp.md
[Učitavanje podataka s PolyBase]: ./sql-data-warehouse-get-started-load-with-polybase.md
[Statistika]: ./sql-data-warehouse-tables-statistics.md
[Pregled za migraciju podataka]: ./sql-data-warehouse-overview-migrate.md

<!--MSDN references-->
[supported source/sink]: https://msdn.microsoft.com/library/dn894007.aspx
[copy activity]: https://msdn.microsoft.com/library/dn835035.aspx
[SQL Server destination adapter]: https://msdn.microsoft.com/library/ms141095.aspx
[SSIS]: https://msdn.microsoft.com/library/ms141026.aspx

[CREATE EXTERNAL DATA SOURCE (Transact-SQL)]: https://msdn.microsoft.com/library/dn935022.aspx
[Stvaranje VANJSKIH OBLIK datoteke (-SQL transakcija)]: https://msdn.microsoft.com/library/dn935026) .aspx [Stvori VANJSKA TABLICA (-SQL transakcija)]: https://msdn.microsoft.com/library/dn935021.aspxx

[DROP EXTERNAL DATA SOURCE (Transact-SQL)]: https://msdn.microsoft.com/library/mt146367.aspx
[DROP EXTERNAL FILE FORMAT (Transact-SQL)]: https://msdn.microsoft.com/library/mt146379.aspx
[DROP EXTERNAL TABLE (Transact-SQL)]: https://msdn.microsoft.com/library/mt130698.aspx

[Stvaranje TABLICE kao odaberite (-SQL transakcija)]: https://msdn.microsoft.com/library/mt204041.aspx
[INSERT...SELECT (Transact-SQL)]: https://msdn.microsoft.com/library/ms174335.aspx
[CREATE MASTER KEY (Transact-SQL)]: https://msdn.microsoft.com/library/ms174382.aspx
[CREATE CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/ms189522.aspx
[CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/mt270260.aspx
[DROP CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/ms189450.aspx

<!-- External Links -->
