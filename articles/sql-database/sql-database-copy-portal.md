<properties
    pageTitle="Kopiranje baze podataka Azure SQL pomoću portala za Azure | Microsoft Azure"
    description="Stvaranje kopije baze podataka Azure SQL"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="09/19/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>



# <a name="copy-an-azure-sql-database-using-the-azure-portal"></a>Kopirajte bazu SQL Azure pomoću portala za Azure

> [AZURE.SELECTOR]
- [Pregled](sql-database-copy.md)
- [Portal za Azure](sql-database-copy-portal.md)
- [PowerShell](sql-database-copy-powershell.md)
- [T-SQL](sql-database-copy-transact-sql.md)

Sljedeći koraci pokazuju kako da biste kopirali SQL baze podataka pomoću [portala za Azure](https://portal.azure.com) na istom poslužitelju ili na drugi poslužitelj.

Da biste kopirali SQL baze podataka, potrebne su vam sljedeće stavke:

- Azure pretplate. Ako vam je potrebna pretplata na Azure jednostavno kliknite **BESPLATNU probnu VERZIJU** pri vrhu ove stranice, a zatim vratite do kraja ovog članka.
- SQL baze podataka da biste kopirali. Ako nemate SQL baze podataka, stvoriti jednu slijedeći korake u ovom se članku: [Stvaranje prve baze podataka SQL Azure](sql-database-get-started.md).


## <a name="copy-your-sql-database"></a>Kopiranje baze podataka sustava SQL

Otvorite stranicu SQL baze podataka za baze podataka koje želite kopirati:

1.  Idite na [portal za Azure](https://portal.azure.com).
2.  Kliknite **Dodatne usluge** > **baze podataka SQL**, a zatim željenu bazu podataka.
3.  Na stranici baze podataka SQL kliknite **Kopiraj**:

    ![SQL baze podataka](./media/sql-database-copy-portal/sql-database-copy.png)

1.  Na stranici **Kopiraj** navedeni zadani naziv baze podataka. Upišite drugo ime ako želite (sve baze podataka na poslužitelju mora sadržavati jedinstvene nazive).
2.  Odaberite **ciljni poslužitelja**. Poslužitelju ciljni je na kojem je stvorena kopiju baze podataka. Možete kopirati bazu podataka u istom poslužitelju ili na drugi poslužitelj. Možete stvoriti na poslužitelju ili na popisu odaberite postojeći poslužitelj. 
3.  Nakon odabira mogućnosti **poslužitelju ciljni**, **skup Elastic baze podataka**i **određivanje cijena sloju** omogućit će se. Ako je poslužitelj zajedničko područje, možete kopirati bazu podataka u njega.
3.  Kliknite **u redu** da biste pokrenuli proces Kopiraj.

    ![SQL baze podataka](./media/sql-database-copy-portal/copy-page.png)


## <a name="monitor-the-progress-of-the-copy-operation"></a>Praćenje napretka kopiranja

- Nakon pokretanja kopiju, kliknite portala obavijest detalje.

    ![obavijesti][3]
 
    ![monitora][4]


## <a name="verify-the-database-is-live-on-the-server"></a>Provjerite je li baza podataka uživo na poslužitelju

- Kliknite **Dodatne usluge** > **baze podataka SQL** i provjerite je li novu bazu podataka je na **mreži**.


## <a name="resolve-logins"></a>Razrješavanje prijave

Da biste riješili prijave nakon dovršetka postupka Kopiraj, potražite u članku [Razrješavanje prijave](sql-database-copy-transact-sql.md#resolve-logins-after-the-copy-operation-completes)


## <a name="next-steps"></a>Daljnji koraci

- Potražite u članku [kopiranje baze podataka Azure SQL](sql-database-copy.md) pregled kopiranje baze podataka SQL Azure.
- Potražite u članku [kopiju baze podataka Azure SQL pomoću komponente PowerShell](sql-database-copy-powershell.md) za kopiranje baze podataka pomoću komponente PowerShell.
- Potražite u članku [kopiju baze podataka Azure SQL pomoću T SQL](sql-database-copy-transact-sql.md) za kopiranje baze podataka pomoću Transact-SQL.
- Pogledajte [kako upravljati sigurnost baze podataka Azure SQL nakon oporavka Izrada](sql-database-geo-replication-security-config.md) dodatne informacije o upravljanju korisnicima i prijave pri kopiranju baze podataka na drugi poslužitelj za logičke.



## <a name="additional-resources"></a>Dodatni resursi

- [Upravljanje prijave](sql-database-manage-logins.md)
- [Povezivanje s bazom podataka SQL s SQL Server Management Studio i obaviti primjer T SQL upita](sql-database-connect-query-ssms.md)
- [Izvoz u BACPAC baze podataka](sql-database-export.md)
- [Pregled Continuity tvrtke](sql-database-business-continuity.md)
- [Dokumentacija SQL baze podataka](https://azure.microsoft.com/documentation/services/sql-database/)




<!--Image references-->
[1]: ./media/sql-database-copy-portal/copy.png
[2]: ./media/sql-database-copy-portal/copy-ok.png
[3]: ./media/sql-database-copy-portal/copy-notification.png
[4]: ./media/sql-database-copy-portal/monitor-copy.png

