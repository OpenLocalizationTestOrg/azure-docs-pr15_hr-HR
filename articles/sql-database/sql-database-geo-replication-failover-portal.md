<properties 
    pageTitle="Pokretanje planiranog ili neplanirano prebacivanje za baze podataka SQL Azure pomoću portala za Azure | Microsoft Azure" 
    description="Pokretanje planiranog ili neplanirano Prebacivanje baze podataka SQL Azure pomoću portala za Azure" 
    services="sql-database" 
    documentationCenter="" 
    authors="stevestein" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-management" 
    ms.date="08/29/2016"
    ms.author="sstein"/>

# <a name="initiate-a-planned-or-unplanned-failover-for-azure-sql-database-with-the-azure-portal"></a>Pokretanje planiranog ili neplanirano prebacivanje za baze podataka SQL Azure pomoću portala za Azure


> [AZURE.SELECTOR]
- [Portal za Azure](sql-database-geo-replication-failover-portal.md)
- [PowerShell](sql-database-geo-replication-failover-powershell.md)
- [T-SQL](sql-database-geo-replication-failover-transact-sql.md)


U ovom se članku objašnjava da biste započeli prebacivanje na sekundarnom SQL baze podataka pomoću [portala za Azure](http://portal.azure.com). Da biste konfigurirali zemlj replikacije, potražite u članku [Konfiguriranje zemlj. – replikacije baze podataka SQL Azure](sql-database-geo-replication-portal.md).


## <a name="initiate-a-failover"></a>Pokretanje programa prebacivanje

Sekundarni baze podataka možete prijeći postaju primarni.  

1. U Pregledaj [Azure portal](http://portal.azure.com) s primarnom bazom podataka u partnerstvo zemlj replikacije.
2. Na plohu SQL baze podataka odaberite **sve postavke** > **Zemlj replikacije**.
3. Na popisu **SECONDARIES** odaberite bazu podataka koju želite postaju novi primarni, a zatim kliknite **Prebacivanje**.

    ![Prebacivanje][2]

4. Kliknite **da** da biste započeli s pomoćnim.

Naredba odmah će se prebaciti sekundarne baze podataka u primarni uloge. 

Postoji kratki razdoblje tijekom kojeg su obje baze podataka nije dostupan (redoslijedu sekundi 0 do 25) dok se mijenja uloge. Primarni baza podataka ima više sekundarne baza podataka, naredba automatski konfigurirajte secondaries povezati nove primarni. Cijeli postupak traje manje od nekoliko minuta da biste dovršili normalnim okolnostima. 

>[AZURE.NOTE] Ako je primarni na Internetu i potvrđuju transakcije kada se naredba izdaje gubitka podataka može doći.


## <a name="next-steps"></a>Daljnji koraci   

- Nakon prebacivanje, provjerite je li preduvjeti za poslužitelj i bazu podataka za provjeru autentičnosti konfigurirane na novu primarni. Detalje potražite u članku [Sigurnost SQL baze podataka nakon oporavka Izrada](sql-database-geo-replication-security-config.md).
- Naučite oporavak nakon Izrada pomoću aktivni zemlj. – replikacije, uključujući pre i objavljuju oporavak korake i detaljnu analizu oporavak Izrada potražite u članku [Izrada oporavak Bušilice](sql-database-disaster-recovery.md)
- Sasha Nosov članka na blogu o aktivni zemlj. – replikacije, u odjeljku [Isticanje na nove mogućnosti zemlj replikacije](https://azure.microsoft.com/blog/spotlight-on-new-capabilities-of-azure-sql-database-geo-replication/)
- Informacije o dizajniranju oblaka aplikacije da biste koristili aktivni replikacije zemlj potražite u članku [dizajniranje oblaka aplikacije za poslovanje pomoću zemlj replikacije](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- Informacije o korištenju aktivni replikacije zemlj s grupe elastic baze podataka potražite u članku [Strategije oporavka Izrada Elastic skup](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
- Pregled continurity tvrtke, potražite u članku [Pregled Continuity tvrtke](sql-database-business-continuity.md)




<!--Image references-->
[1]: ./media/sql-database-geo-replication-failover-portal/failover.png
[2]: ./media/sql-database-geo-replication-failover-portal/secondaries.png
