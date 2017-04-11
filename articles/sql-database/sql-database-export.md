<properties
    pageTitle="Arhiviranje baze podataka Azure SQL BACPAC datoteku pomoću portala za Azure"
    description="Arhiviranje baze podataka Azure SQL BACPAC datoteku pomoću portala za Azure"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="08/15/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="archive-an-azure-sql-database-to-a-bacpac-file-using-the-azure-portal"></a>Arhiviranje baze podataka Azure SQL BACPAC datoteku pomoću portala za Azure

> [AZURE.SELECTOR]
- [Portal za Azure](sql-database-export.md)
- [PowerShell](sql-database-export-powershell.md)

U ovom članku navedene upute za arhiviranje baze podataka Azure SQL BACPAC datoteku (koji je spremljen u spremište blobova platforme Azure) pomoću [portala za Azure](https://portal.azure.com).

Kada je potrebno da biste stvorili arhivu baze podataka Azure SQL, shemu baze podataka i podatke možete izvesti u datoteku BACPAC. Datoteka BACPAC se jednostavno ZIP datoteku s datotečnim nastavkom BACPAC. Datoteke BACPAC kasnije moguće pohraniti u spremište blobova platforme Azure ili lokalno spremište u lokalne lokacije i kasnije uvezene natrag u baze podataka SQL Azure ili u SQL Server lokalne instalacije. 

***Razmatranja***

- Za arhiviranje da bi bio putem transakcije dosljedan, morate omogućiti ili da nema pisanje aktivnosti se pojavljuje prilikom izvoza ili koju izvozite iz [putem transakcije dosljedan kopiju](sql-database-copy.md) baze podataka Azure SQL.
- Maksimalna veličina datoteke BACPAC arhiviraju spremište blobova platforme Azure je 200 GB. Za arhiviranje veća BACPAC datoteka za lokalno spremište, koristite uslužni [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) naredbeni redak. Uslužni dolazi s Visual Studio i SQL Server. Možete [preuzeti](https://msdn.microsoft.com/library/mt204009.aspx) najnoviju verziju sustava SQL Server Data Tools da biste dobili uslužni.
- Arhiviranje u spremište Azure premium pomoću datoteke BACPAC nije podržana.
- Ako operaciju izvoza premašuje 20 sati, mogu se otkazati. Da biste poboljšali performanse tijekom izvoza, možete učiniti sljedeće:
 - Privremeno povećati razinu zaštite servisa.
 - Prestat sve za čitanje i pisanje aktivnosti prilikom izvoza.
 - Koristite [indeks](https://msdn.microsoft.com/library/ms190457.aspx) s vrijednostima koje nisu null na sve velike tablice. Izvoz možda bez grupirani indeksi neće uspjeti ako je potrebno više od 6 12 sati. To je jer servis za izvoz morate dovršiti pregled tablicu možete pokušati izvesti cijelu tablicu. Dobar način da biste odredili ako tablica su optimizirani za izvoz je da biste pokrenuli **DBCC SHOW_STATISTICS** i provjerite je li *RANGE_HI_KEY* nije null i njezina vrijednost je dobro raspodjele. Detalje potražite u članku [DBCC SHOW_STATISTICS](https://msdn.microsoft.com/library/ms174384.aspx).


> [AZURE.NOTE] BACPACs se ne treba koristiti za sigurnosno kopiranje i vraćanje operacije. Baze podataka SQL Azure automatski stvara sigurnosne kopije za svakog korisnika bazu podataka. Detalje potražite u članku [Pregled Continuity tvrtke](sql-database-business-continuity.md).

Da biste dovršili ovaj članak potrebno je sljedeće:

- Azure pretplate.
- Baze podataka Azure SQL. 
- [Račun za Azure standardne prostora za pohranu](../storage/storage-create-storage-account.md) s blob spremnik za pohranu na BACPAC u standardni prostora za pohranu.

## <a name="export-your-database"></a>Izvoz baze podataka

Otvorite plohu SQL baze podataka za bazu podataka koju želite izvesti.

> [AZURE.IMPORTANT] Da bi BACPAC datoteku putem transakcije dosljedan treba prvi [stvoriti kopiju baze podataka](sql-database-copy.md) , a zatim izvezite kopiju baze podataka. 

1.  Idite na [portal za Azure](https://portal.azure.com).
2.  Kliknite **baze podataka SQL**.
3.  Kliknite baze podataka koju želite arhivirati.
4.  U plohu SQL baze podataka kliknite **Izvoz** da biste otvorili plohu **Izvoz baze podataka** :

    ![Gumb Izvezi][1]

5.  Kliknite **prostor za pohranu** i odaberite vaše prostora za pohranu račun i blob spremnik pohranjuju u BACPAC:

    ![Izvoz baze podataka][2]

6. Odaberite vrstu provjere autentičnosti. 
7.  Unesite vjerodajnice odgovarajuće provjere autentičnosti za Azure SQL server koja sadrži izvozite bazu podataka.
8.  Kliknite **u redu** za arhiviranje bazu podataka. Klikom na **u redu** stvara zahtjev baze podataka za izvoz i šalje na servis. Izvoz će se vremena ovisi o veličini i složenosti baze podataka i razinu zaštite servisa. Primit ćete obavijest.

    ![Izvoz obavijesti][3]

## <a name="monitor-the-progress-of-the-export-operation"></a>Praćenje napretka operaciju izvoza

1.  Kliknite **SQL poslužitelja**.
2.  Kliknite poslužitelj koji sadrži izvorna (izvor) baza podataka samo arhiviraju.
3.  Pomaknite se do operacije.
4.  U SQL Elektronička ploča poslužitelja kliknite **Uvoz/izvoz povijest**:

    ![Uvoz i izvoz povijest][4]

## <a name="verify-the-bacpac-is-in-your-storage-container"></a>Provjerite je li u BACPAC u vašem spremnik za pohranu

1.  Kliknite **Računi za pohranu**.
2.  Kliknite račun za pohranu pohranjuju u BACPAC arhivu.
3.  Kliknite **spremnika** i odaberite spremnik ste izvezli bazu podataka u detalje (možete preuzeti i spremiti BACPAC na tom mjestu).

    ![pojedinosti .bacpac datoteke][5]  

## <a name="next-steps"></a>Daljnji koraci

- Dodatne informacije o uvozu na BACPAC s bazom podataka SQL Azure, potražite u članku [Uvoz BACPCAC s bazom podataka Azure SQL](sql-database-import.md)
- Dodatne informacije o uvozu na BACPAC s bazom podataka sustava SQL Server potražite u članku [Uvoz BACPCAC s bazom podataka sustava SQL Server](https://msdn.microsoft.com/library/hh710052.aspx)



<!--Image references-->
[1]: ./media/sql-database-export/export.png
[2]: ./media/sql-database-export/export-blade.png
[3]: ./media/sql-database-export/export-notification.png
[4]: ./media/sql-database-export/export-history.png
[5]: ./media/sql-database-export/bacpac-archive.png

