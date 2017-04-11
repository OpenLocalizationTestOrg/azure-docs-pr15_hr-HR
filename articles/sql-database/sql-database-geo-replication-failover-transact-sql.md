<properties 
    pageTitle="Pokretanje planiranog ili neplanirano Prebacivanje baze podataka SQL Azure s Transact-SQL | Microsoft Azure" 
    description="Pokretanje planiranog ili neplanirano prebacivanje za baze podataka SQL Azure pomoću Transact-SQL" 
    services="sql-database" 
    documentationCenter="" 
    authors="CarlRabeler" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-management"
    ms.date="08/29/2016"
    ms.author="carlrab"/>

# <a name="initiate-a-planned-or-unplanned-failover-for-azure-sql-database-with-transact-sql"></a>Pokretanje planiranog ili neplanirano Prebacivanje baze podataka SQL Azure s Transact-SQL


> [AZURE.SELECTOR]
- [Portal za Azure](sql-database-geo-replication-failover-portal.md)
- [PowerShell](sql-database-geo-replication-failover-powershell.md)
- [T-SQL](sql-database-geo-replication-failover-transact-sql.md)


U ovom se članku objašnjava da biste započeli prebacivanje s bazom podataka SQL sekundarni pomoću Transact-SQL. Da biste konfigurirali zemlj replikacije, potražite u članku [Konfiguriranje zemlj. – replikacije baze podataka SQL Azure](sql-database-geo-replication-transact-sql.md).



Da biste započeli prebacivanje, potrebno je sljedeće:

- Prijava koja se nalazi DBManager na primarni, imate db_ownership će zemlj replicirati, lokalnu bazu podataka i mora biti DBManager na partnera poslužitelji u koju ćete konfigurirati zemlj replikacije.
- SQL Server Management Studio (SSMS)


> [AZURE.IMPORTANT] Preporučuje se da uvijek koristite najnoviju verziju Management Studio ostati sinkronizirani s ažuriranjima za Microsoft Azure i SQL baze podataka. [Ažuriranje SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).




## <a name="initiate-a-planned-failover-promoting-a-secondary-database-to-become-the-new-primary"></a>Pokretanje planiranog prebacivanje prosljeđivanja sekundarni baze podataka da biste se novi primarni

Iskaz **ALTER baze podataka** možete koristiti da biste unaprijedili sekundarne baze podataka postane nove primarni baze podataka na planirane način da biste smanjili razinu postojeći primarni postati sekundarni. Izjavu o zaštiti se izvršava na glavnom bazom podataka na poslužitelju logičke baze podataka SQL Azure u kojoj se nalazi zemlj replicirati sekundarne bazu podataka koja je u tijeku ulogu. Ta je funkcija je namijenjen planiranog prebacivanje, kao što su tijekom bušilice DR i zahtijeva dostupnosti primarnom bazom podataka.

Naredba izvodi tijeka rada za sljedeće:

1. Privremeno prelazi replikacijom sinkrono načinu rada, uzrok sve preostale transakcije da biste se isprazniti sekundarnom i blokiranje sve nove transakcije;

2. Promjena uloga dvije baze podataka u partnerstvo zemlj replikacijom.  

Taj se redoslijed jamčiti dvije baze podataka sinkroniziraju prije prijeđite uloge i zbog toga će se izvršiti bez gubitka podataka. Postoji kratki razdoblje tijekom kojeg su obje baze podataka nije dostupan (redoslijedu sekundi 0 do 25) dok se mijenja uloge. Primarni baza podataka ima više sekundarne baza podataka, naredba automatski konfigurirajte secondaries povezati nove primarni.  Cijeli postupak traje manje od nekoliko minuta da biste dovršili normalnim okolnostima. Dodatne informacije potražite u članku [Bazu podataka s ALTER (Transact-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) i [Servis razine](sql-database-service-tiers.md).


Poduzmite sljedeće korake da biste započeli planiranog prebacivanje.

1. U programu Management Studio povezati s poslužiteljem baze podataka SQL Azure logičke u kojoj se nalazi zemlj replicirati sekundarne baze podataka.

2. Otvorite mapu u bazama podataka, proširite mapu **Sustava baze podataka** , desnom tipkom miša kliknite **glavni**, a zatim **Novi upit**.

3. Da biste se prebacili sekundarne baze podataka temeljna funkcija, koristite sljedeću naredbu **ALTER baze podataka** .

        ALTER DATABASE <MyDB> FAILOVER;

4. Kliknite **izvršavanje** da biste pokrenuli upit.

>[AZURE.NOTE] U koje se rijetko pojavljuju se slučajevima moguće je postupak možete završiti, a mogu se prikazati stuck. U ovom slučaju korisnika možete izvršiti naredbu za prebacivanje prisilno i prihvatite gubitka podataka.


## <a name="initiate-an-unplanned-failover-from-the-primary-database-to-the-secondary-database"></a>Pokretanje neplanirano prebacivanje s primarnom bazom podataka sekundarne baze podataka

Iskaz **ALTER baze podataka** možete koristiti da biste unaprijedili sekundarne baze podataka postane nove primarni baze podataka neplanirano način, nameće smanjivanje postojeći primarni postati sekundarni u trenutku kada primarni databse više nije dostupan. Izjavu o zaštiti se izvršava na glavnom bazom podataka na poslužitelju logičke baze podataka SQL Azure u kojoj se nalazi zemlj replicirati sekundarne bazu podataka koja je u tijeku ulogu.

Ta je funkcija namijenjen je Izrada oporavak kada vraćanje dostupnosti baze podataka je ključnih i gubitak podataka je prihvatljiva. Kada se poziva prisilne prebacivanje, navedenoj bazi podataka za sekundarne odmah postaje primarnom bazom podataka i počinje prihvaćanja transakcija pisanja. Čim izvorne primarni baze podataka se može povezati s sažeti primarni, rastuće sigurnosne kopije je primijenjena na izvorne primarni baze podataka i postala je stari primarnom bazom podataka u bazu podataka za sekundarne nove baze podataka primarni; potom je samo sinkronizaciju replike novi primarni.

Međutim, jer točke u vrijeme vraćanje nije podržana na sekundarnom baze podataka, ako korisnik želi oporaviti podatke izvršenom stare primarni bazu podataka koju imali ne je replicirati na novu bazu podataka primarni prije došlo je do prisilne prebacivanje, korisnika morat ćete sudjelovati podršku da biste oporavili to gubitka podataka.

Primarni baza podataka ima više sekundarne baza podataka, naredba automatski konfigurirajte secondaries povezati nove primarni.

Poduzmite sljedeće korake da biste započeli neplanirano prebacivanje.

1. U programu Management Studio povezati s poslužiteljem baze podataka SQL Azure logičke u kojoj se nalazi zemlj replicirati sekundarne baze podataka.

2. Otvorite mapu u bazama podataka, proširite mapu **Sustava baze podataka** , desnom tipkom miša kliknite **glavni**, a zatim **Novi upit**.

3. Da biste se prebacili sekundarne baze podataka temeljna funkcija, koristite sljedeću naredbu **ALTER baze podataka** .

        ALTER DATABASE <MyDB>   FORCE_FAILOVER_ALLOW_DATA_LOSS;

4. Kliknite **izvršavanje** da biste pokrenuli upit.

>[AZURE.NOTE] Ako se naredba izdaje kada primarnih i sekundarnih su Internetu stare primarni će postati novi sekundarni bez sinkronizacije podataka. Ako je primarni potvrđuju transakcije kada se naredba izdaje gubitka podataka mogu se pojaviti.



## <a name="next-steps"></a>Daljnji koraci   

- Nakon prebacivanje, provjerite je li preduvjeti za poslužitelj i bazu podataka za provjeru autentičnosti konfigurirane na novu primarni. Detalje potražite u članku [Sigurnost SQL baze podataka nakon oporavka Izrada](sql-database-geo-replication-security-config.md).
- Naučite oporavak nakon Izrada pomoću aktivni zemlj. – replikacije, uključujući pre i objavljuju oporavak korake i detaljnu analizu oporavak Izrada potražite u članku [Oporavak Izrada](sql-database-disaster-recovery.md)
- Sasha Nosov članka na blogu o aktivni zemlj. – replikacije, u odjeljku [Isticanje na nove mogućnosti zemlj replikacije](https://azure.microsoft.com/blog/spotlight-on-new-capabilities-of-azure-sql-database-geo-replication/)
- Informacije o dizajniranju oblaka aplikacije da biste koristili aktivni replikacije zemlj potražite u članku [dizajniranje oblaka aplikacije za poslovanje pomoću zemlj replikacije](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- Informacije o korištenju aktivni replikacije zemlj s grupe elastic baze podataka potražite u članku [Strategije oporavka Izrada Elastic skup](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
- Pregled continurity tvrtke, potražite u članku [Pregled Continuity tvrtke](sql-database-business-continuity.md)
