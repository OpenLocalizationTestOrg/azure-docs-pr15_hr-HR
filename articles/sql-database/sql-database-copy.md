<properties
    pageTitle="Kopiranje baze podataka Azure SQL | Microsoft Azure"
    description="Stvaranje kopije baze podataka Azure SQL"
    services="sql-database"
    documentationCenter=""
    authors="anosov1960"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="10/24/2016"
    ms.author="sstein; sashan"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>



# <a name="copy-an-azure-sql-database"></a>Kopiranje baze podataka Azure SQL

> [AZURE.SELECTOR]
- [Pregled](sql-database-copy.md)
- [Portal za Azure](sql-database-copy-portal.md)
- [PowerShell](sql-database-copy-powershell.md)
- [T-SQL](sql-database-copy-transact-sql.md)

Azure [SQL baze podataka automatskog sigurnosnog kopiranja](sql-database-automated-backups.md) možete koristiti da biste stvorili kopiju baze podataka sustava SQL. Kopiraj bazu podataka kao značajka zemlj replikacije koristi istu tehnologiju. No, za razliku od zemlj replikacije je prekida vezu replikacije kao kada se dovrši seeding fazu. Stoga je baza podataka Kopiraj snimke stanja izvorišne baze podataka na vrijeme zahtjev za kopiranje.  
Možete stvoriti kopiju baze podataka na istom poslužitelju ili na drugi poslužitelj. Servis sloju i performanse razinu (cijene sloju) kopiju baze podataka isti su kao izvorišne baze podataka prema zadanim postavkama. Kada se koristi s API-JA, možete odabrati različite performanse razinu unutar iste sloju servisa (izdanje). Po dovršetku kopiju kopiju postaje potpuno funkcionalni, neovisno bazu podataka. Sada možete nadograditi ili vraćanje ga na stariju verziju na bilo kojem izdanje. Prijave, korisnici i dozvole može upravljati pojedinačno.  

Kada kopirate baze podataka na isti poslužitelj logičke, iste prijave može se koristiti na oba baze podataka. Sigurnost glavni koristite da biste kopirali bazu podataka postaje vlasnik baze podataka (vlasnika baze podataka) na novu bazu podataka. Svi korisnici baze podataka, njihove dozvole i njihove sigurnosni identifikatori (SID-ovi) kopiraju kopiju baze podataka.  

Kada kopirate baze podataka na drugi poslužitelj za logičke, sigurnost glavni na novom poslužitelju postaje vlasnik baze podataka na novu bazu podataka. Ako koristite [nalaze korisnici baze podataka](sql-database-manage-logins.md) za pristup podacima osigurava primarnih i sekundarnih baze podataka uvijek imaju isti korisničke vjerodajnice da bi se nakon dovršetka Kopiraj odmah joj možete pristupiti s istim vjerodajnicama. Ako koristite [Azure Active Directory](../active-directory/active-directory-whatis.md), možete isključiti u potpunosti potrebe za upravljanje vjerodajnicama u kopiji. Međutim, kada kopirate baze podataka u novi poslužitelj, access prijava koji se temelji obično neće funkcionirati jer će prijave ne postoji na novom poslužitelju. Pogledajte [kako upravljati sigurnost baze podataka Azure SQL nakon oporavka Izrada](sql-database-geo-replication-security-config.md) dodatne informacije o upravljanju prijave pri kopiranju baze podataka na drugi poslužitelj za logičke. 

Da biste kopirali SQL baze podataka, potrebno je sljedeće:

- Azure pretplate. Ako vam je potrebna pretplata na Azure jednostavno kliknite **BESPLATNU probnu VERZIJU** pri vrhu ove stranice, a zatim vratite do kraja ovog članka.
- SQL baze podataka da biste kopirali. Ako nemate SQL baze podataka, stvoriti jednu slijedeći korake u ovom se članku: [Stvaranje prve baze podataka SQL Azure](sql-database-get-started.md).

## <a name="next-steps"></a>Daljnji koraci

- Potražite u članku [kopiranje baze podataka Azure SQL pomoću portala za Azure](sql-database-copy-portal.md) da biste kopirali baze podataka pomoću portala za Azure.
- Potražite u članku [kopiju baze podataka Azure SQL pomoću komponente PowerShell](sql-database-copy-powershell.md) za kopiranje baze podataka pomoću komponente PowerShell.
- Potražite u članku [kopiju baze podataka Azure SQL pomoću T SQL](sql-database-copy-transact-sql.md) za kopiranje baze podataka pomoću Transact-SQL.
- Pogledajte [kako upravljati sigurnost baze podataka Azure SQL nakon oporavka Izrada](sql-database-geo-replication-security-config.md) dodatne informacije o upravljanju korisnicima i prijave pri kopiranju baze podataka na drugi poslužitelj za logičke.



## <a name="additional-resources"></a>Dodatni resursi

- [Upravljanje prijave](sql-database-manage-logins.md)
- [Povezivanje s bazom podataka SQL s SQL Server Management Studio i obaviti primjer T SQL upita](sql-database-connect-query-ssms.md)
- [Izvoz u BACPAC baze podataka](sql-database-export.md)
- [Pregled Continuity tvrtke](sql-database-business-continuity.md)
- [Dokumentacija SQL baze podataka](https://azure.microsoft.com/documentation/services/sql-database/)
