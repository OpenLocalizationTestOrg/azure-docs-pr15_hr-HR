<properties
   pageTitle="Transakcije u SQL Data Warehouse | Microsoft Azure"
   description="Savjeti za implementaciju transakcije u skladištu podataka za SQL Azure za razvoj rješenja."
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
   ms.date="07/31/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="transactions-in-sql-data-warehouse"></a>Transakcije u skladištu podataka za SQL

Kao što biste očekivali SQL Data Warehouse podržava transakcije kao dio skladištu povećavaju podataka. Međutim, da biste bili sigurni performanse SQL Data Warehouse se održava na razini neke značajke ograničena su u usporedbi sa sustavom SQL Server. U ovom se članku ističe razlike popise i u drugim korisnicima. 

## <a name="transaction-isolation-levels"></a>Razine odvajanja transakcije
SQL Data Warehouse implementira ACID transakcije. Međutim, odvajanja transakcijskih podrške ograničeno je na `READ UNCOMMITTED` i to nije moguće promijeniti. Možete implementirati broj kodiranje metode da biste spriječili prljav čitanja podataka ako se problem. Najpopularniji metode pod utjecajem CTAS i tablice particija promjene (često naziva klizača uzorak prozor) da biste korisnicima onemogućili slanje upita za podatke koji se i dalje pripremljeni. Prikazi koje unaprijed filtriranje podataka je popularne pristup.  

## <a name="transaction-size"></a>Veličina transakcije
Izmjena transakcije jedan podatkovni ograničen je veličina. Ograničenje danas primjenjuje "po raspodjele". Zbog toga da ukupni alokacija se izračunava množenjem ograničenje po count raspodjele. Da biste djelomičnog maksimalan broj redaka u transakcije podijelite kapaciteta raspodjele ukupnu veličinu svakog retka. Za stupce dužine varijable razmislite o poduzimanja je prosječna stupca duljine umjesto korištenja maksimalne veličine.

U tablici u nastavku sljedeće pretpostavke uvedena su:

* Došlo je do ravnomjerna distribucija podataka 
* Duljina average retka je 250 bajtova

| [DWU][]    | Kapac po raspodjele (GiB) | Broj distribucije | MAKSIMALNA veličina transakcije (GiB) | # Redaka po distribuciju. | Maksimalni broj redaka po transakcije |
| ------ | -------------------------- | ----------------------- | -------------------------- | ----------------------- | ------------------------ |
| DW100  |  1                         | 60                      |   60                       |   4,000,000             |    240,000,000           |
| DW200  |  1,5                       | 60                      |   90                       |   6,000,000             |    360,000,000           |
| DW300  |  2.25                      | 60                      |  135                       |   9,000,000             |    540,000,000           |
| DW400  |  3                         | 60                      |  180                       |  12,000,000             |    720,000,000           |
| DW500  |  3.75                      | 60                      |  225                       |  15.000.000             |    900,000,000           |
| DW600  |  4,5                       | 60                      |  270                       |  18,000,000             |  1,080,000,000           |
| DW1000 |  7.5                       | 60                      |  450                       |  30,000,000             |  1,800,000,000           |
| DW1200 |  9                         | 60                      |  540                       |  36,000,000             |  2,160,000,000           |
| DW1500 | 11.25                      | 60                      |  675                       |  45,000,000             |  2,700,000,000           |
| DW2000 | 15                         | 60                      |  900                       |  60,000,000             |  3,600,000,000           |
| DW3000 | 22.5                       | 60                      |  1,350                     |  90,000,000             |  5,400,000,000           |
| DW6000 | 45                         | 60                      |  2,700                     | 180,000,000             | 10,800,000,000           |

Ograničenje veličine transakcije primjenjuje se po transakcija ili operacija. Se primjenjuje na sve Istodobni transakcije. Stoga je dopušteno pojedine transakcije te količine podataka zapisivanja u zapisnik. 

Optimiziranje i smanjiti količinu podataka zapisuju u zapisniku potražite u članku [transakcije najbolje prakse][] .

> [AZURE.WARNING] Transakcije Maksimalna veličina mogu samo postići za RASPRŠIVANJE ili čak ROUND_ROBIN distributed tablice gdje se nalazi širenja podataka. Ako je transakcija je zapisivanje podataka asimetričnu način da biste na distribucija zatim ograničenje je vjerojatno proslijediti poziv prije transakcije Maksimalna veličina.
<!--REPLICATED_TABLE-->

## <a name="transaction-state"></a>Stanje transakcije
SQL Data Warehouse koristi funkciju XACT_STATE() da biste prijavili nije uspjelo transakcija s vrijednošću -2. To znači da se transakcije nije uspjelo i je označena za vraćanje samo

> [AZURE.NOTE] Korištenje broja -2 pomoću funkcije XACT_STATE za označavanje nije uspjelo transakcije predstavlja različite ponašanje sa sustavom SQL Server. SQL Server koristi vrijednost -1 za predstavljanje nepotvrđen committable transakcije. SQL Server možete tolerate neke pogreške unutar transakcije bez potrebe za biti označene kao nepotvrđen committable. Na primjer `SELECT 1/0` će uzrokovati pogrešku, ali ne prisilno transakcija u nepotvrđen committable stanje. SQL Server i omogućuje čitanje nepotvrđen committable transakcija. Međutim, SQL Data Warehouse ne dopušta učinite sljedeće. Ako dođe do pogreške u SQL Data Warehouse transakcije će automatski unijeti stanje-2 i nećete moći učiniti nešto dodatno odaberite izjave dok iskaz je poništena. Stoga je važno da biste provjerili kodu aplikacije da biste vidjeli ako koristi XACT_STATE() tijekom morati izmijenite kod.

Na primjer, u sustavu SQL Server se može pojaviti transakcije koji izgleda ovako:

```sql
SET NOCOUNT ON;
DECLARE @xact_state smallint = 0;

BEGIN TRAN
    BEGIN TRY
        DECLARE @i INT;
        SET     @i = CONVERT(INT,'ABC');
    END TRY
    BEGIN CATCH
        SET @xact_state = XACT_STATE();

        SELECT  ERROR_NUMBER()    AS ErrNumber
        ,       ERROR_SEVERITY()  AS ErrSeverity
        ,       ERROR_STATE()     AS ErrState
        ,       ERROR_PROCEDURE() AS ErrProcedure
        ,       ERROR_MESSAGE()   AS ErrMessage
        ;
        
        IF @@TRANCOUNT > 0
        BEGIN
            PRINT 'ROLLBACK';
            ROLLBACK TRAN;
        END

    END CATCH;

IF @@TRANCOUNT >0
BEGIN
    PRINT 'COMMIT';
    COMMIT TRAN;
END

SELECT @xact_state AS TransactionState;
```

Ako ostavite kod kao što su iznad nje će se sljedeća poruka o pogrešci:

Poruka 111233, razina 16, stanje 1, redak 1 111233; Trenutni transakcija je prekinuta, a sve promjene na čekanju ste poništena. Uzrok: Transakcije u stanju samo za vraćanje nije izričito vraćena je prije DDL, DML ili iskaza SELECT.

Nećete i dobiti rezultat funkcije ERROR_ *.

U SQL Data Warehouse kod mora malo promijenit:

```sql
SET NOCOUNT ON;
DECLARE @xact_state smallint = 0;

BEGIN TRAN
    BEGIN TRY
        DECLARE @i INT;
        SET     @i = CONVERT(INT,'ABC');
    END TRY
    BEGIN CATCH
        SET @xact_state = XACT_STATE();
        
        IF @@TRANCOUNT > 0
        BEGIN
            PRINT 'ROLLBACK';
            ROLLBACK TRAN;
        END

        SELECT  ERROR_NUMBER()    AS ErrNumber
        ,       ERROR_SEVERITY()  AS ErrSeverity
        ,       ERROR_STATE()     AS ErrState
        ,       ERROR_PROCEDURE() AS ErrProcedure
        ,       ERROR_MESSAGE()   AS ErrMessage
        ;
    END CATCH;

IF @@TRANCOUNT >0
BEGIN
    PRINT 'COMMIT';
    COMMIT TRAN;
END

SELECT @xact_state AS TransactionState;
```

Očekivano ponašanje sada opaženih. Pogreška transakcija upravlja i funkcije ERROR_ * unesite vrijednosti za prema očekivanjima.

Sve što je promijenjena je u `ROLLBACK` transakcije morali dogoditi prije čitanje informacija o pogrešci u na `CATCH` blok.

## <a name="errorline-function"></a>Funkcija Error_Line()
To vrijedi i sudjelovanje da SQL Data Warehouse ne implementirati ili ne podržava funkciju ERROR_LINE(). Ako imate to u kodu, morat ćete ukloniti biti usklađen sa SQL Data Warehouse. Umjesto toga koristite upit natpise u kodu za implementaciju ekvivalentne funkcije. Pogledajte u članku [NALJEPNICE][] za dodatne informacije o toj značajci.

## <a name="using-throw-and-raiserror"></a>Korištenje VARANJE i RAISERROR
VARANJE je više Moderna implementacija za prilikom otvaranja okvira podešavanje iznimke u SQL Data Warehouse, ali RAISERROR podržano je i. Postoji nekoliko razlike koje treba planiranju no.

- Korisnički definirane brojevi ne može biti u rasponu 100,000 150.000 VARANJE poruke o pogreškama
- Poruka o pogrešci RAISERROR rješava pri 50.000
- Upotreba sys.messages nije podržana

## <a name="limitiations"></a>Limitiations
SQL Data Warehouse imaju nekoliko ograničenja koja se odnose na transakcije.

Su na sljedeći način:

- Nema raspodijeljeno transakcije
- Nema ugniježđene transakcije dopušteno
- Bez spremanja točaka dopuštene
- Nema imenovani transakcije
- Nema označenu transakcije
- Nema podrške za DDL kao što su `CREATE TABLE` unutar korisnika definirani transakcije

## <a name="next-steps"></a>Daljnji koraci
Da biste saznali više o optimiziranje transakcije, potražite u članku [transakcije najbolje prakse][].  Da biste saznali više o drugim SQL Data Warehouse najbolje prakse, potražite u članku [najbolje prakse za SQL Data Warehouse][].

<!--Image references-->

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[development overview]: ./sql-data-warehouse-overview-develop.md
[Najbolje prakse transakcije]: ./sql-data-warehouse-develop-best-practices-transactions.md
[Najbolje prakse za SQL Data Warehouse]: ./sql-data-warehouse-best-practices.md
[OZNAKA]: ./sql-data-warehouse-develop-label.md

<!--MSDN references-->

<!--Other Web references-->
