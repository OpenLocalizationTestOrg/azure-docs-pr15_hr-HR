<properties
    pageTitle="Stvaranje i upravljanje skaliranu izvan baze podataka SQL Azure pomoću elastic poslove | Microsoftovu studiju Azure"
    description="Voditi kroz stvaranje i upravljanje za posao elastic baze podataka."
    services="sql-database"
    documentationCenter=""
    manager="jhubbard"
    authors="ddove"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/27/2016"
    ms.author="ddove"/>

# <a name="create-and-manage-scaled-out-azure-sql-databases-using-elastic-jobs-preview"></a>Stvaranje i upravljanje skaliranu izvan baze podataka SQL Azure pomoću elastic poslove (pretpregled)

> [AZURE.SELECTOR]
- [Portal za Azure](sql-database-elastic-jobs-create-and-manage.md)
- [PowerShell](sql-database-elastic-jobs-powershell.md)


**Elastic baze podataka zadataka** Pojednostavnite Upravljanje grupama baza podataka izvršavanjem administratora operacija poput promjene sheme, upravljanje vjerodajnicama, ažuriranja podataka referenca, zbirka podataka o performansama ili klijenta (Kupac) telemetrijskih zbirke. Elastic zadatke baze podataka trenutno nije dostupno putem portala za Azure i cmdleta ljuske PowerShell. Međutim, Azure portala površine smanjena funkcionalnost ograničen na izvođenja preko sve baze podataka u [skup Elastic baze podataka (pretpregled)](sql-database-elastic-pool.md). Da biste pristupili dodatnim značajkama i izvršavanja skripti u grupi baza podataka uključujući zbirka Prilagođeno definirane ili skup shard (stvorena pomoću [Biblioteka klijentski Elastic baze podataka](sql-database-elastic-scale-introduction.md)), potražite u članku [Stvaranje i upravljanje zadacima pomoću komponente PowerShell](sql-database-elastic-jobs-powershell.md). Dodatne informacije o zadacima potražite u članku [Pregled zadataka Elastic baze podataka](sql-database-elastic-jobs-overview.md). 

## <a name="prerequisites"></a>Preduvjeti

* Azure pretplate. Besplatna probna verzija, potražite u članku [besplatnu probnu verziju jedan mjesec](https://azure.microsoft.com/pricing/free-trial/).
* Elastic baze podataka grupe aplikacija. Potražite u članku [o Elastic grupe baze podataka](sql-database-elastic-pool.md)
* Instalacija komponente za servis posao elastic baze podataka. Potražite u članku [Instaliranje usluga zadatka elastic baze podataka](sql-database-elastic-jobs-service-installation.md).

## <a name="creating-jobs"></a>Stvaranje zadataka

1. Pomoću [portala za Azure](https://portal.azure.com)iz programa postojeći skup posao elastic baze podataka, kliknite **Stvaranje zadatka**.
2. Unesite korisničko ime i lozinku od administratora baze podataka (stvorena pri instalaciji zadataka) za bazu podataka za kontrolu poslove (spremište metapodataka za poslove).

    ![Naziv zadatka, upišite ili zalijepite kod pa kliknite Pokreni][1]
2. U plohu **Stvorite zadatak** upišite naziv za posao.
3. Unesite korisničko ime i lozinku za povezivanje s bazama podataka za ciljnu s dostatne dozvole za izvršavanje skriptu da.
4. Lijepljenje ili upišite T SQL skripta.
5. Kliknite **Spremi** , a zatim kliknite **Pokreni**.

    ![Stvaranje zadataka i pokretanje][5]

## <a name="run-idempotent-jobs"></a>Pokrenite idempotent poslove

Kada pokrenete skriptu uspoređuje sa skupom baze podataka, morate biti nalazi li se skripta idempotent. To jest, skripta moraju imati mogućnost da biste pokrenuli više puta, čak i ako nije uspjela prije u nedovršenu stanju. Ako, na primjer, ako skriptu ne uspije, posao će se automatski ponoviti dok je uspješnog (unutar ograničenja, kao na pokušaj logike naposljetku prestat će se ponovni). Način za to je za korištenje na "Ako postoji" uvjet WHERE i izbrisati sve instance pronađenih prije stvaranja novog objekta. Primjer je prikazano ovdje:

    IF EXISTS (SELECT name FROM sys.indexes
            WHERE name = N'IX_ProductVendor_VendorID')
    DROP INDEX IX_ProductVendor_VendorID ON Purchasing.ProductVendor;
    GO
    CREATE INDEX IX_ProductVendor_VendorID
    ON Purchasing.ProductVendor (VendorID);

Umjesto toga možete koristiti u uvjet "Ako ne postoji" prije stvaranja nove instance:

    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
     CREATE TABLE TestTable(
      TestTableId INT PRIMARY KEY IDENTITY,
      InsertionTime DATETIME2
     );
    END
    GO

    INSERT INTO TestTable(InsertionTime) VALUES (sysutcdatetime());
    GO

Ova skripta ažurira tablice koje ste prethodno stvorili.

    IF NOT EXISTS (SELECT columns.name FROM sys.columns INNER JOIN sys.tables on columns.object_id = tables.object_id WHERE tables.name = 'TestTable' AND columns.name = 'AdditionalInformation')
    BEGIN

    ALTER TABLE TestTable

    ADD AdditionalInformation NVARCHAR(400);
    END
    GO

    INSERT INTO TestTable(InsertionTime, AdditionalInformation) VALUES (sysutcdatetime(), 'test');
    GO


## <a name="checking-job-status"></a>Provjera stanja zadatka

Kada je počeo posla, možete provjeriti u tijeku.

1. Na stranici skup elastic baze podataka kliknite **Upravljanje zadacima**.

    ![Kliknite "Upravljanje zadacima"][2]

2. Kliknite naziv (a) za posao. **STATUS** može biti "Dovršeno" ili "Nije uspjela". Detalji posla prikazuju se (b) s datum i vrijeme stvaranja i pokretanje. Na popisu (c) ispod koje prikazuju tijek skripte svaki bazu podataka u grupu, dodjeljivanja detalja datum i vrijeme.

    ![Provjera dovršeni zadatak][3]


## <a name="checking-failed-jobs"></a>Provjera nije uspjela poslova

Ako je zadatak neuspješan, zapisnik njegova izvođenja mogu pronaći. Kliknite naziv nije uspjelo zadatka da biste vidjeli pojedinosti.

![Provjera nije uspjelo posla][4]


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-create-and-manage/screen-1.png
[2]: ./media/sql-database-elastic-jobs-create-and-manage/click-manage-jobs.png
[3]: ./media/sql-database-elastic-jobs-create-and-manage/running-jobs.png
[4]: ./media/sql-database-elastic-jobs-create-and-manage/failed.png
[5]: ./media/sql-database-elastic-jobs-create-and-manage/screen-2.png

 
