<properties 
    pageTitle="Pokretanje planiranog ili neplanirano Prebacivanje baze podataka SQL Azure sa servisom PowerShell | Microsoft Azure" 
    description="Pokretanje planirane ili neplanirano Prebacivanje baze podataka SQL Azure pomoću komponente PowerShell" 
    services="sql-database" 
    documentationCenter="" 
    authors="stevestein" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management" 
    ms.date="08/29/2016"
    ms.author="sstein"/>

# <a name="initiate-a-planned-or-unplanned-failover-for-azure-sql-database-with-powershell"></a>Pokretanje planiranog ili neplanirano Prebacivanje baze podataka SQL Azure pomoću komponente PowerShell



> [AZURE.SELECTOR]
- [Portal za Azure](sql-database-geo-replication-failover-portal.md)
- [PowerShell](sql-database-geo-replication-failover-powershell.md)
- [T-SQL](sql-database-geo-replication-failover-transact-sql.md)


U ovom se članku objašnjava da biste započeli planiranog ili neplanirano prebacivanje za baze podataka SQL pomoću komponente PowerShell. Da biste konfigurirali zemlj replikacije, potražite u članku [Konfiguriranje zemlj. – replikacije baze podataka SQL Azure](sql-database-geo-replication-powershell.md).



## <a name="initiate-a-planned-failover"></a>Pokretanje planiranog prebacivanje

Pomoću cmdleta **Skup AzureRmSqlDatabaseSecondary** s parametrom **– Prebacivanje** Promicanje sekundarni baze podataka postane nove primarni baze podataka, da biste smanjili razinu postojeći primarni postane sekundarni. Ta je funkcija namijenjen je planiranog prebacivanje, kao što su tijekom Izrada oporavak bušilice, i zahtijeva dostupnosti primarnom bazom podataka.

Naredba izvodi tijeka rada za sljedeće:

1. Privremeno prijelaz replikacije sinkrono način rada. Time će se sve preostale transakcije da biste se isprazniti sekundarnom.

2. Prebacivanje uloge dvije baze podataka u partnerstvo zemlj replikacije.  

U ovom nizu jamčiti dvije baze podataka sinkroniziraju prije prijeđite uloge i zbog toga će se izvršiti bez gubitka podataka. Postoji kratki razdoblje tijekom kojeg su obje baze podataka nije dostupan (redoslijedu sekundi 0 do 25) dok se mijenja uloge. Cijeli postupak traje manje od nekoliko minuta da biste dovršili normalnim okolnostima. Dodatne informacije potražite u članku [Postavljanje AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/library/mt619393.aspx).




Ovaj cmdlet će vratiti po dovršetku postupka prijelaza primarni sekundarni baze podataka.

Sljedeća naredba prebacuje uloge baze podataka pod nazivom "mydb" na poslužitelju "srv2" u odjeljku na resursa grupe "rg2" za primarni. Izvorni primarni "db2" na koji je povezan s će se prebaciti sekundarne nakon što potpuno sinkroniziraju dvije baze podataka.

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" –ResourceGroupName "rg2” –ServerName "srv2”
    $database | Set-AzureRmSqlDatabaseSecondary -Failover


> [AZURE.NOTE] U slučajevima rijetko moguće je postupak možete završiti, a mogu se prikazati ne reagira. U ovom slučaju korisnika možete poziva naredbe za prebacivanje prisilno (neplanirano prebacivanje) i prihvatite gubitka podataka.


## <a name="initiate-an-unplanned-failover-from-the-primary-database-to-the-secondary-database"></a>Pokretanje neplanirano prebacivanje s primarnom bazom podataka sekundarne baze podataka


**Postavljanje AzureRmSqlDatabaseSecondary** cmdlet možete koristiti s parametri **– Prebacivanje** i **- AllowDataLoss** Promicanje sekundarne baze podataka postane nove primarni baze podataka neplanirano način, nameće smanjivanje postojeći primarni postati sekundarni u trenutku kada primarni baze podataka više nije dostupan.

Ta je funkcija namijenjen je Izrada oporavak kada vraćanje dostupnosti baze podataka je ključnih i gubitak podataka je prihvatljiva. Kada se poziva prisilne prebacivanje, navedena sekundarni baza podataka odmah postaje primarnom bazom podataka i počinje prihvaćanja transakcija pisanja. Čim izvorne primarni baze podataka se može povezati s sažeti primarni nakon operacije prisilne prebacivanje, rastuće sigurnosne kopije je primijenjena na izvorne primarni baze podataka i postala je stari primarnom bazom podataka u bazu podataka za sekundarne nove baze podataka primarni; potom je samo replike novi primarni.

No Budući da točke u vrijeme vraćanje nije podržana na sekundarnom baze podataka, ako želite da podaci izvršenom stare primarni bazu podataka koju ste je replicirati novu bazu podataka za primarni, trebali biste sudjelovati CSS-a da biste vratili bazu podataka u poznate zapisnika sigurnosnog kopiranja.

> [AZURE.NOTE] Ako se naredba izdaje kada primarnih i sekundarnih su Internetu stare primarni će postati novi sekundarni bez sinkronizacije podataka. Ako je primarni potvrđuju transakcije kada se naredba izdaje gubitka podataka mogu se pojaviti.


Ako se primarni baza podataka ima više secondaries naredbu djelomično uspjeti. Sekundarni izvršenja naredbe više neće biti primarni. Stari primarni no će ostati primarni, odnosno dva očuvat će završe u kojima nisu dosljedno stanje i povezanim obustavljenom replikacije vezu. Korisnik će morati ručno popravite tu konfiguraciju korištenja API-JA "uklanjanje sekundarne" na bilo koju od ovih primarni baza podataka.


Sljedeća naredba prebacuje uloge baze podataka pod nazivom "mydb" za primarni kada primarni nije dostupna. Izvorni primarni "mydb" na koji je povezan s će se prebaciti sekundarni kada se vratite u mrežni način. U tom trenutku sinkronizacije može dovesti do gubitka podataka.

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" –ResourceGroupName "rg2” –ServerName "srv2”
    $database | Set-AzureRmSqlDatabaseSecondary –Failover -AllowDataLoss




## <a name="next-steps"></a>Daljnji koraci   

- Nakon prebacivanje, provjerite je li preduvjeti za poslužitelj i bazu podataka za provjeru autentičnosti konfigurirane na novu primarni. Detalje potražite u članku [Sigurnost SQL baze podataka nakon oporavka Izrada](sql-database-geo-replication-security-config.md).
- Naučite oporavak nakon Izrada pomoću aktivni zemlj. – replikacije, uključujući pre i objavljuju oporavak korake i detaljnu analizu oporavak Izrada potražite u članku [Izrada oporavak Bušilice](sql-database-disaster-recovery.md)
- Sasha Nosov članka na blogu o aktivni zemlj. – replikacije, u odjeljku [Isticanje na nove mogućnosti zemlj replikacije](https://azure.microsoft.com/blog/spotlight-on-new-capabilities-of-azure-sql-database-geo-replication/)
- Informacije o dizajniranju oblaka aplikacije da biste koristili aktivni replikacije zemlj potražite u članku [dizajniranje oblaka aplikacije za poslovanje pomoću zemlj replikacije](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- Informacije o korištenju aktivni replikacije zemlj s grupe elastic baze podataka potražite u članku [Strategije oporavka Izrada Elastic skup](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
- Pregled continurity tvrtke, potražite u članku [Pregled Continuity tvrtke](sql-database-business-continuity.md)
