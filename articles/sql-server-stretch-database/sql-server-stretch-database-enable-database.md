<properties
    pageTitle="Omogućivanje rastezanje baze podataka za baze podataka | Microsoft Azure"
    description="Saznajte kako konfigurirati baze podataka za rastezanje bazu podataka."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/05/2016"
    ms.author="douglasl"/>

# <a name="enable-stretch-database-for-a-database"></a>Omogućivanje rastezanje baze podataka za baze podataka

Da biste konfigurirali postojeću bazu podataka za bazu podataka rastezanje, odaberite zadaci **| Rastezanje | Omogućivanje** za bazu podataka u programu SQL Server Management Studio da biste otvorili čarobnjak za **Omogućivanje baze podataka za rastezanje** . Možete koristiti i Transact\-SQL da biste omogućili rastezanje baze podataka za bazu podataka.

Ako odaberete zadatke **| Rastezanje | Omogućivanje** za pojedinačne tablice, a niste još omogućili baze podataka za bazu podataka rastezanje, čarobnjak konfigurira baze podataka za bazu podataka rastezanje i omogućuje odabir tablice u sklopu postupka. Slijedite korake u nastavku umjesto korake iz [Omogućite rastezanje baze podataka za tablicu](sql-server-stretch-database-enable-database.md).

Omogućivanje rastezanje baze podataka u bazu podataka ili tablice zahtijeva db\_dozvole vlasnika. Omogućivanje Rastegnuto baze podataka u bazu podataka potreban je i dozvole za bazu podataka za KONTROLU.

 >   [AZURE.NOTE] Kasnije, Ako onemogućite rastezanje baze podataka, imajte na umu da onemogućivanje rastezanje baze podataka za tablicu ili za bazu podataka ne briše udaljene objekta. Ako želite da biste izbrisali udaljenoj tablici ili udaljenom bazom podataka, morate ga ispustite pomoću portala za upravljanje Azure. Remote objekte i dalje plaćati Azure troškove dok se ne možete ih izbrisati ručno.

## <a name="before-you-get-started"></a>Prije nego što počnete

-   Konfiguriranje baze podataka za rastezanje, preporučujemo da pokrenete rastezanje Savjetnik za baze podataka da biste odredili baze podataka i tablice koje ispunjavaju uvjete za rastezanje. Rastezanje Savjetnik za baze podataka određuje i tog problema. Dodatne informacije potražite u članku [bazama podataka za otkrivanje i tablica za bazu podataka rastezanje](sql-server-stretch-database-identify-databases.md).

-   Pregledajte [ograničenja za rastezanje baze podataka](sql-server-stretch-database-limitations.md).

-   Razvlačenje baze podataka prenosi podatke u Azure. Stoga ćete morati imati račun za Azure i pretplatu za naplatu. Da biste dobili Azure račun, [kliknite ovdje](http://azure.microsoft.com/pricing/free-trial/).

-   Imate vezu i prijavite se informacije potrebne da biste stvorili novi poslužitelj za Azure ili da biste odabrali postojeći Azure poslužitelj.

## <a name="EnableTSQLServer"></a>Preduvjeta: Omogućivanje rastezanje baze podataka na poslužitelju
Prije omogućivanja mogućnosti rastezanje baze podataka u bazi podataka ili tablice, morate omogućiti na lokalnom poslužitelju. Ovaj postupak zahtijeva sustava ili serveradmin dozvole.

-   Ako imate potrebne administratorske dozvole, čarobnjak za **Omogućivanje baze podataka za rastezanje** konfigurira poslužitelj za rastezanje.

-   Ako nemate odgovarajuće dozvole, administrator mora omogućiti mogućnost ručno tako da pokrenete **sp\_konfiguriranje** prije nego što pokrenete čarobnjak ili je administrator da biste pokrenuli čarobnjak.

Da biste ručno omogućavanje rastezanje baze podataka na poslužitelju, pokrenite **sp\_konfiguriranje** i uključite mogućnost **udaljeni podaci arhiva** . U sljedećem primjeru omogućuje **arhiviranje udaljeni podaci** mogućnost postavljanjem njezina vrijednost 1.

```
EXEC sp_configure 'remote data archive' , '1';
GO

RECONFIGURE;
GO
```
Dodatne informacije potražite u članku [Konfiguriranje udaljeni podaci arhiviranje mogućnost konfiguracije poslužitelja](https://msdn.microsoft.com/library/mt143175.aspx) i [sp_configure (Transact-SQL)](https://msdn.microsoft.com/library/ms188787.aspx).

## <a name="Wizard"></a>Pomoću čarobnjaka za omogućivanje rastezanje baze podataka u bazi podataka
Informacije o omogućiti baze podataka za korištenje čarobnjaka za rastezanje, uključujući informacije koje morate unijeti i mogućnosti koje morate učiniti, pročitajte članak [Početak rada tako da pokrenete omogućiti baze podataka za korištenje čarobnjaka za rastezanje](sql-server-stretch-database-wizard.md).

## <a name="EnableTSQLDatabase"></a>Korištenje Transact\-SQL da biste omogućili rastezanje baze podataka u bazu podataka
Prije omogućivanja mogućnosti rastezanje baze podataka na pojedinačna tablica, morate omogućiti na bazu podataka.

Omogućivanje Rastegnuto baze podataka u bazu podataka ili tablice zahtijeva db\_dozvole vlasnika. Omogućivanje rastezanje baze podataka u bazu podataka potreban je i dozvole za bazu podataka za KONTROLU.

1.  Prije početka, odaberite postojeći Azure poslužitelj za podatke koji se prenosi Rastegnuto baze podataka ili stvorite novi poslužitelj za Azure.

2.  Na poslužitelj za Azure stvaranje pravila vatrozida s rasponu IP adresa sustava SQL Server koja omogućuje komunikaciju SQL Server s udaljeni poslužitelj.

    Možete jednostavno pronaći vrijednosti potrebna i stvaranje pravila vatrozida po pokušaja povezivanja s poslužiteljem Azure iz programa Explorer objekta u SQL Server Management Studio (SSMS). SSMS pomaže vam da biste stvorili pravilo tako da otvorite sljedeći dijaloški okvir koji već sadrži tražene vrijednosti IP adresa.

    ![Stvaranje pravila vatrozida u SSMS][FirewallRule]

3.  Da biste konfigurirali baze podataka SQL Server za bazu podataka rastezanje, baze podataka mora imati glavni ključ za bazu podataka. Glavni ključ baze podataka secures vjerodajnice za bazu podataka rastezanje koristi za povezivanje s udaljenom bazom podataka. Slijedi primjer koji stvara novi ključ matrica baze podataka.

    ```tsql
    USE <database>;
    GO

    CREATE MASTER KEY ENCRYPTION BY PASSWORD ='<password>';
    GO
    ```

    Dodatne informacije o glavni ključ baze podataka potražite u članku [Stvaranje MATRICE KLJUČ (Transact-SQL)](https://msdn.microsoft.com/library/ms174382.aspx) i [Stvaranje osnovne ključ za baze podataka](https://msdn.microsoft.com/library/aa337551.aspx).

4.  Kada konfigurirate baze podataka za bazu podataka rastezanje, morate unijeti vjerodajnice za rastezanje baze podataka koju želite koristiti za komunikaciju između na lokalnom sustavu SQL Server i udaljeni poslužitelj za Azure. Imate dvije mogućnosti.

    -   Možete unijeti vjerodajnicu za administratora.

        -   Ako omogućite rastezanje baze podataka pomoću čarobnjaka za, to možete stvoriti vjerodajnicu.

        -   Namjeravate omogućiti rastezanje baze podataka tako da pokrenete **ALTER baze podataka**, morate stvoriti vjerodajnica ručno prije nego što pokrenete **ALTER baze podataka** da biste omogućili rastezanje baze podataka.

        Slijedi primjer koji stvara novu vjerodajnice.

        ```tsql
        CREATE DATABASE SCOPED CREDENTIAL <db_scoped_credential_name>
            WITH IDENTITY = '<identity>' , SECRET = '<secret>';
        GO
        ```

        Dodatne informacije o vjerodajnicu potražite u članku [Stvaranje baze podataka iz DJELOKRUGA VJERODAJNICA (Transact-SQL)](https://msdn.microsoft.com/library/mt270260.aspx). Stvaranje vjerodajnicu potrebne su dozvole PROMIJENITI bilo koje VJERODAJNICA.

    -   Koristite račun pridruženim usluga za SQL Server možete komunicirati s udaljeni poslužitelj za Azure kada su svi ispunjeni sljedeći uvjeti.

        -   Račun servisa u kojem se izvodi instancu sustava SQL Server je račun domene.

        -   Račun domene pripada domeni čije Active Directory je povezani Azure Active Directory.

        -   Udaljeni poslužitelj za Azure nije konfigurirano tako da podržava provjeru autentičnosti Azure Active Directory.

        -   Račun servisa u kojem se izvodi instancu sustava SQL Server, morate ga konfigurirati kao račun dbmanager ili sustava na udaljeni poslužitelj za Azure.

5.  Da biste konfigurirali baze podataka za bazu podataka rastezanje, pokrenite naredbe ALTER DATABASE.

    1.  Za argument POSLUŽITELJA Navedite naziv postojećeg Azure poslužitelja, uključujući na `.database.windows.net` dio naziva \- , na primjer, `MyStretchDatabaseServer.database.windows.net`.

    2.  Podijelite postojeće vjerodajnica za administratora u argumentu VJERODAJNICA ili je odredite VANJSKIM\_SERVISA\_RAČUNA = Uključeno. U sljedećem primjeru sadrži postojeće vjerodajnica.

    ```tsql
    ALTER DATABASE <database name>
        SET REMOTE_DATA_ARCHIVE = ON
            (
                SERVER = '<server_name>',
                CREDENTIAL = <db_scoped_credential_name>
            ) ;
    GO
    ```

## <a name="next-steps"></a>Daljnji koraci
-   [Omogućivanje rastezanje baze podataka za tablicu](sql-server-stretch-database-enable-table.md) da biste omogućili dodatne tablice.

-   [Baza podataka rastezanje monitora](sql-server-stretch-database-monitor.md) za prikaz statusa migracije podataka.

-   [Zaustavljanje i nastavak rastezanje baze podataka](sql-server-stretch-database-pause.md)

-   [Upravljanje i rješavanje problema s rastezanje baze podataka](sql-server-stretch-database-manage.md)

-   [Sigurnosno kopiranje baze podataka rastezanje omogućeno](sql-server-stretch-database-backup.md)

## <a name="see-also"></a>Vidi također

[Prepoznavanje baze podataka i tablica za bazu podataka rastezanje](sql-server-stretch-database-identify-databases.md)

[Postavljanje mogućnosti ALTER baze podataka (-SQL transakcija)](https://msdn.microsoft.com/library/bb522682.aspx)

[FirewallRule]: ./media/sql-server-stretch-database-enable-database/firewall.png
