<properties
    pageTitle="Onemogućite rastezanje baze podataka i Premjesti nazad udaljeni podaci | Microsoft Azure"
    description="Saznajte kako onemogućiti rastezanje baze podataka za tablicu i po želji Premjesti nazad udaljeni podaci."
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

# <a name="disable-stretch-database-and-bring-back-remote-data"></a>Onemogućite rastezanje baze podataka i Premjesti nazad udaljeni podaci

Da biste onemogućili rastezanje baze podataka za tablicu, odaberite **rastezanje** za tablice u SQL Server Management Studio. Odaberite jednu od sljedećih mogućnosti.

-   Onemogući **| Ponovno prijenos podataka iz Azure**. Kopiraj udaljeni podaci za tablicu iz Azure natrag na SQL Server pa onemogućite rastezanje baze podataka za tablicu. Ovaj postupak uključuje troškove prijenos podataka, a ne može se otkazati.

-   Onemogući **| Ostaviti podatke servisu Azure**. Onemogućivanje rastezanje baze podataka za tablicu.  Zbog toga odustati od udaljeni podaci za tablicu u Azure.

Možete koristiti i Transact\-SQL da biste onemogućili rastezanje baze podataka za tablicu ili bazu podataka.

Nakon onemogućivanja rastezanje baze podataka za tablicu, tabulatora za migraciju podataka i rezultata upita više ne sadrže rezultate iz udaljene tablice.

Ako samo želite zaustaviti migraciju podataka, potražite u članku [Pauziraj i životopis rastezanje baze podataka](sql-server-stretch-database-pause.md).

>   [AZURE.NOTE] Onemogućivanje rastezanje baze podataka za tablicu ili za bazu podataka neće se izbrisati udaljene objekta. Ako želite da biste izbrisali udaljenoj tablici ili udaljenom bazom podataka, morate ga ispustite pomoću portala za upravljanje Azure. Remote objekte i dalje plaćati Azure troškove dok se ne možete ih izbrisati. Dodatne informacije potražite u članku [SQL Server rastezanje baze podataka cijene](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).

## <a name="disable-stretch-database-for-a-table"></a>Onemogućivanje rastezanje baze podataka za tablicu

### <a name="use-sql-server-management-studio-to-disable-stretch-database-for-a-table"></a>SQL Server Management Studio možete onemogućiti pomoću rastezanje baze podataka za tablicu

1.  U SQL Server Management Studio u programu Explorer objekta, odaberite tablicu za koju želite onemogućiti rastezanje baze podataka.

2.  Desnom\-kliknite i odaberite **rastezanje**, a zatim odaberite jednu od sljedećih mogućnosti.

    -   Onemogući **| Ponovno prijenos podataka iz Azure**. Kopiraj udaljeni podaci za tablicu iz Azure natrag na SQL Server pa onemogućite rastezanje baze podataka za tablicu. Ta naredba nije moguće poništiti.

        >   [AZURE.NOTE] Kopiranje udaljeni podaci za tablicu iz Azure natrag na SQL Server uključuje troškove prijenos podataka. Dodatne informacije potražite u odjeljku [Detalji cijene prijenosa podataka](https://azure.microsoft.com/pricing/details/data-transfers/).

        Nakon što se sve udaljeni podaci kopiran iz Azure sigurnosno SQL Server, Rastegnuto onemogućen je za tablice.

    -   Onemogući **| Ostaviti podatke servisu Azure**. Onemogućivanje rastezanje baze podataka za tablicu.  Zbog toga odustati od udaljeni podaci za tablicu u Azure.

    >   [AZURE.NOTE] Onemogućivanje rastezanje baze podataka za tablicu izbrisati udaljeni podaci ili udaljenom tablice. Ako želite da biste izbrisali udaljene tablicu, morate ga ispustite pomoću portala za upravljanje Azure. I dalje plaćati Azure troškove dok ne izbrišete udaljenoj tablici. Dodatne informacije potražite u članku [SQL Server rastezanje baze podataka cijene](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).

### <a name="use-transact-sql-to-disable-stretch-database-for-a-table"></a>Korištenje Transact\-SQL da biste onemogućili rastezanje baze podataka za tablice

-   Da biste onemogućili rastezanje za tablice i Kopiraj udaljeni podaci za tablicu iz Azure natrag na SQL Server, pokrenite sljedeću naredbu. Nakon što se sve udaljeni podaci kopiran iz Azure sigurnosno SQL Server, Rastegnuto onemogućen je za tablice.

    Ta naredba nije moguće poništiti.

    ```tsql
    USE <Stretch-enabled database name>;
    GO
    ALTER TABLE <Stretch-enabled table name>  
       SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = INBOUND ) ) ;
    GO
    ```
    >   [AZURE.NOTE] Kopiranje udaljeni podaci za tablicu iz Azure natrag na SQL Server uključuje troškove prijenos podataka. Dodatne informacije potražite u odjeljku [Detalji cijene prijenosa podataka](https://azure.microsoft.com/pricing/details/data-transfers/).

-   Da biste onemogućili rastezanje za tablicu, a zatim ćete udaljeni podaci, pokrenite sljedeću naredbu.

    ```tsql
    ALTER TABLE <table_name>
       SET ( REMOTE_DATA_ARCHIVE = OFF_WITHOUT_DATA_RECOVERY ( MIGRATION_STATE = PAUSED ) ) ;
    ```

>   [AZURE.NOTE] Onemogućivanje rastezanje baze podataka za tablicu izbrisati udaljeni podaci ili udaljenom tablice. Ako želite da biste izbrisali udaljene tablicu, morate ga ispustite pomoću portala za upravljanje Azure. I dalje plaćati Azure troškove dok ne izbrišete udaljenoj tablici. Dodatne informacije potražite u članku [SQL Server rastezanje baze podataka cijene](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).

## <a name="disable-stretch-database-for-a-database"></a>Onemogućivanje rastezanje baze podataka za baze podataka
Prije rastezanje baze podataka možete onemogućiti za bazu podataka, morate onemogućiti rastezanje baze podataka na pojedinačne rastezanje\-omogućeno tablica u bazi podataka.

### <a name="use-sql-server-management-studio-to-disable-stretch-database-for-a-database"></a>SQL Server Management Studio možete onemogućiti pomoću rastezanje baze podataka za baze podataka

1.  SQL Server Management Studio u programu Explorer objekta, odaberite bazu podataka za koju želite onemogućiti rastezanje baze podataka.

2.  Desnom\-kliknite i odaberite **Zadaci**i zatim odaberite **rastezanje**, a zatim **Onemogući**.

>   [AZURE.NOTE] Onemogućivanje rastezanje baze podataka za bazu podataka ne briše se udaljena baza podataka. Ako želite da biste izbrisali udaljena baza podataka, morate ga ispustite pomoću portala za upravljanje Azure. Udaljena baza podataka i dalje plaćati Azure troškove dok ne izbrišete. Dodatne informacije potražite u članku [SQL Server rastezanje baze podataka cijene](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).

### <a name="use-transact-sql-to-disable-stretch-database-for-a-database"></a>Korištenje Transact\-SQL da biste onemogućili rastezanje baze podataka za baze podataka
Pokrenite sljedeću naredbu.

```tsql
ALTER DATABASE <database name>
    SET REMOTE_DATA_ARCHIVE = OFF ;
```

>   [AZURE.NOTE] Onemogućivanje rastezanje baze podataka za bazu podataka ne briše se udaljena baza podataka. Ako želite da biste izbrisali udaljena baza podataka, morate ga ispustite pomoću portala za upravljanje Azure. Udaljena baza podataka i dalje plaćati Azure troškove dok ne izbrišete. Dodatne informacije potražite u članku [SQL Server rastezanje baze podataka cijene](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).

## <a name="see-also"></a>Vidi također

[Postavljanje mogućnosti ALTER baze podataka (-SQL transakcija)](https://msdn.microsoft.com/library/bb522682.aspx)

[Zaustavljanje i nastavak rastezanje baze podataka](sql-server-stretch-database-pause.md)
