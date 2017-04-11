<properties
    pageTitle="Zadržite pokazivač i nastavili migraciju podataka (baza podataka rastezanje) | Microsoft Azure"
    description="Saznajte kako zaustavljanje migracije podataka za Azure."
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
    ms.date="06/14/2016"
    ms.author="douglasl"/>

# <a name="pause-and-resume-data-migration-stretch-database"></a>Zadržite pokazivač i nastavili migraciju podataka (baza podataka rastezanje)

Zaustavljanje migracije podataka za Azure, odaberite **rastezanje** za tablicu u SQL Server Management Studio, a zatim **zadržite pokazivač** pauziranje migraciju podataka i **nastavili** da biste nastavili migraciju podataka. Možete koristiti i Transact\-SQL zaustavljanje migracije podataka.

Zaustavljanje migraciju podataka na pojedinačna tablica kada želite otkloniti poteškoće s lokalnog poslužitelja ili Maksimiziraj propusnost mreže dostupna.

## <a name="pause-data-migration"></a>Zaustavljanje migracije podataka

### <a name="use-sql-server-management-studio-to-pause-data-migration"></a>Zadržite pokazivač migracije podataka pomoću SQL Server Management Studio

1.  U SQL Server Management Studio u programu Explorer objekta, odaberite na rastezanje\-omogućeno tablicu za koju želite zaustaviti migracije podataka.

2.  Desnom\-kliknite i odaberite **rastezanje**, a zatim odaberite **Zaustavi**.

### <a name="use-transact-sql-to-pause-data-migration"></a>Korištenje Transact\-SQL pauziranje migracije podataka
Pokrenite sljedeću naredbu.

```tsql
USE <Stretch-enabled database name>;
GO
ALTER TABLE <Stretch-enabled table name>  
    SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = PAUSED ) ) ;  
GO
```

## <a name="resume-data-migration"></a>Nastavak migracije podataka

### <a name="use-sql-server-management-studio-to-resume-data-migration"></a>Da biste nastavili migraciju podataka pomoću SQL Server Management Studio

1.  U SQL Server Management Studio u programu Explorer objekta, odaberite na rastezanje\-omogućeno tablicu za koju želite nastaviti migracije podataka.

2.  Desnom\-kliknite i odaberite **rastezanje**, a zatim **Nastavi**.

### <a name="use-transact-sql-to-resume-data-migration"></a>Korištenje Transact\-SQL da biste nastavili migraciju podataka
Pokrenite sljedeću naredbu.

```tsql
USE <Stretch-enabled database name>;
GO
ALTER TABLE <Stretch-enabled table name>   
    SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = OUTBOUND ) ) ;  
 GO
```

## <a name="check-whether-migration-is-active-or-paused"></a>Provjerite je li migracije aktivna ni pauzirano

### <a name="use-sql-server-management-studio-to-check-whether-migration-is-active-or-paused"></a>Provjerite je li migracije aktivna ni pauzirano pomoću SQL Server Management Studio
U sustavu SQL Server Management Studio otvorite **Monitor rastezanje baze podataka** i provjerite vrijednost u stupcu **Stanje za migraciju** . Dodatne informacije potražite u članku [monitora i otklanjanje poteškoća s migracije podataka](sql-server-stretch-database-monitor.md).

### <a name="use-transact-sql-to-check-whether-migration-is-active-or-paused"></a>Provjerite je li migracije aktivna ni pauzirano pomoću Transact-SQL
Prikaz kataloga **sys.remote_data_archive_tables** upita, a zatim provjerite vrijednost u stupcu **is_migration_paused** . Dodatne informacije potražite u članku [sys.remote_data_archive_tables](https://msdn.microsoft.com/library/dn935003.aspx).

## <a name="see-also"></a>Vidi također

[ALTER TABLE (Transact-SQL)](https://msdn.microsoft.com/library/ms190273.aspx)
[monitora i otklanjanje poteškoća s migracije podataka](sql-server-stretch-database-monitor.md)
