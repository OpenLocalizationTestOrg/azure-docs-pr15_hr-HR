<properties
   pageTitle="Oporavak Izrada baze podataka SQL | Microsoft Azure"
   description="Saznajte kako oporaviti baze podataka iz otkazivanja s Azure SQL replikacije baze podataka Active zemlj. – i mogućnosti zemlj vraćanja ili nedostupnosti regionalnog podatkovnog centra."
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/13/2016"
   ms.author="carlrab"/>

# <a name="restore-an-azure-sql-database-or-failover-to-a-secondary"></a>Vraćanje baze podataka SQL Azure ili prebacivanje sekundarni

Baze podataka SQL Azure nudi sljedeće mogućnosti za oporavak u do prekida:

- [Aktivni replikacije zemlj.](sql-database-geo-replication-overview.md)
- [Vraćanje za zemlj.](sql-database-recovery-using-backups.md#point-in-time-restore)

Da biste saznali više o poslovne scenarije continuity i značajke potpore scenarija, pogledajte [poslovanje](sql-database-business-continuity.md).

### <a name="prepare-for-the-event-of-an-outage"></a>Priprema za događaj do prekida

Uspjeha oporavak za drugi područja podataka pomoću aktivni replikacije zemlj ili zemlj suvišnih sigurnosne kopije, najprije morate Priprema poslužitelju u drugom podatkovnog centra nedostupnosti postati novi primarni poslužitelj treba morate pojaviti kao i definirani i koraka navedenih i testirati da biste bili sigurni izglađenim oporavak. Ovih pripremnih koraka obuhvaćaju sljedeće:

- Odredite logičke poslužitelja u drugoj regiji postati novi primarni poslužitelj. S aktivni zemlj. – replikacije, to će biti najmanje jedan i možda svaki sekundarni poslužitelji. Za vraćanje zemlj., to obično bit će na poslužitelju u [paru područja](../best-practices-availability-paired-regions.md) za regiju u kojoj se nalazi bazu podataka.
- Prepoznajte i mogućnost definiranja na razini poslužitelja pravila vatrozida potrebno na za korisnike da biste pristupili nove primarni baze podataka.
- Odredite kako ćete preusmjeravanje korisnicima novi primarni poslužitelj, kao što su promjenom nizove veze ili promjenom DNS stavke.
- Prepoznajte i po želji stvaranje prijave koji mora postojati u glavnom bazom podataka na novi primarni poslužitelj i provjerite je li ove prijave ako je bilo koji imaju odgovarajuće dozvole u glavnom bazom podataka. Dodatne informacije potražite u članku [Sigurnost SQL baze podataka nakon oporavka Izrada](sql-database-geo-replication-security-config.md)
- Odredite pravila upozorenje koje će se ažurirati da biste mapirali novu bazu podataka primarni.
- Dokument nadzora konfiguraciji na trenutni primarni baze podataka
- Izvođenje na [Dubinska analiza Izrada oporavak](sql-database-disaster-recovery-drills.md). Da biste se prekida kao zamjenu za zemlj vraćanje, možete izbrisati ili preimenovati izvorišne baze podataka kako bi pogreška povezivanja programa. U programu Publisher na nedostupnosti za Active zemlj. – replikacijom, možete onemogućiti web-aplikacija ili Virtualno računalo povezano s bazom podataka ili Prebacivanje baze podataka uzrokovati pogreške connectity aplikacije.

## <a name="when-to-initiate-recovery"></a>Kada da biste započeli oporavak

Operacija oporavak utječe aplikacije. Potreban je promjena SQL niz za povezivanje ili preusmjeravanje upotrebe DNS-a i moglo bi dovesti do gubitka trajna podataka. Stoga je potrebno riješiti samo kada se prekida se vjerojatno dulje od vašeg računala oporavak vrijeme cilj. Kada aplikacija je implementiran na radni trebali biste izvođenje pravnog praćenja stanja računala i koristiti sljedeće točke podataka za nametanje je li potrebno oporavka:

1.  Pogreška trajna povezivanja iz razina aplikacije u bazu podataka.
2.  Portal za Azure prikazuje upozorenje o incident u regiji s općenite utjecaja.
3.  Poslužitelj baze podataka SQL Azure je označena kao što su smanjene značajke.

Ovisno o odstupanje vaše aplikacije za prekid u radu i moguće tvrtke odgovornosti uzmite u obzir sljedeće mogućnosti za oporavak.

Koristite [Dohvaćanje oporaviti baze podataka](https://msdn.microsoft.com/library/dn800985.aspx) (*LastAvailableBackupDate*) da biste dobili najnovije točke zemlj replicirati vraćanja.

## <a name="wait-for-service-recovery"></a>Pričekajte da oporavak servisa

Rad Azure timovima održavate da biste vratili dostupnost usluge ga brzo moguće, ali ovisno o korijenskom prouzročiti može potrajati sati ili dana.  Ako aplikacija možete tolerate značajan nedostupnost jednostavno možete pričekati oporavak da biste dovršili. U ovom slučaju ne poduzmete na svoj dio potreban je. Vidjet ćete trenutno stanje servisa na našem [Nadzorna ploča za stanje servisa Azure](https://azure.microsoft.com/status/). Nakon oporavka regije vratit će se dostupnost vaša aplikacija.

## <a name="failover-to-geo-replicated-secondary-database"></a>Prebacivanje zemlj replicirati sekundarne bazu podataka

Ako nedostupnost vaše aplikacije može uzrokovati odgovornosti tvrtke, trebali biste koristiti zemlj replicirati baze podataka u aplikaciji. To će se omogućiti aplikacije da biste brzo vratili dostupnost u nekoj drugoj regiji u slučaju se prekida. Saznajte kako [konfigurirati zemlj replikacije](sql-database-geo-replication-portal.md).

Da biste vratili dostupnost podataka potrebno da biste započeli prebacivanje sekundarnom zemlj replicirati na jedan od podržanih načina.

Primijenite jednu od sljedećih vodiči za prebacivanje s zemlj replicirati sekundarne bazom podataka:

- [Prebacivanje na pomoću portala za Azure sekundarni replicirati na zemlj.](sql-database-geo-replication-portal.md)
- [Prebacivanje na sekundarni zemlj replicirati pomoću komponente PowerShell](sql-database-geo-replication-powershell.md)
- [Prebacivanje na pomoću T SQL sekundarni replicirati na zemlj.](sql-database-geo-replication-transact-sql.md)

## <a name="recover-using-geo-restore"></a>Oporavak pomoću vraćanja za zemlj.

Ako vaša aplikacija nedostupnost rezultiraju odgovornosti tvrtke zemlj vraćanja možete koristiti kao način da biste oporavili svoje baze podataka aplikacije. Stvara kopiju baze podataka iz njene najnovije zemlj suvišne sigurnosne kopije.

Primijenite jednu od sljedećih vodilice zemlj vratiti baze podataka u novu regiju:

- [Vraćanje zemlj bazu podataka za novo područje pomoću portala za Azure](sql-database-geo-restore-portal.md)
- [Vraćanje zemlj bazu podataka za novo područje pomoću komponente PowerShell](sql-database-geo-restore-powershell.md)

## <a name="configure-your-database-after-recovery"></a>Konfiguriranje baze podataka nakon oporavka

Ako koristite zemlj replikacije prebacivanje ili zemlj Vrati se može oporaviti iz programa prekida, provjerite povezivanje na nove baze podataka je li pravilno konfigurirana tako da se funkcija normalne aplikaciju možete nastaviti. Ovo je popis za provjeru zadataka pripremiti proizvodnje oporavljene baze podataka.

### <a name="update-connection-strings"></a>Ažuriranje veze nizova

Jer oporavljene baze podataka će se nalaziti u drugi poslužitelj, morate ažurirati niz za povezivanje vašeg računala na tom poslužitelju.

Dodatne informacije o promjeni nizu za povezivanje potražite u članku odgovarajuće razvoj jezik za [biblioteku veze](sql-database-libraries.md).

### <a name="configure-firewall-rules"></a>Konfigurirati pravila vatrozida

Morate da biste bili sigurni da pravila vatrozida konfiguriran na poslužitelj i bazu podataka podudaraju s dimenzijama podešena na primarnom poslužitelju i primarnom bazom podataka. Dodatne informacije potražite u članku [Kako: Konfiguriranje postavki vatrozida (bazom podataka SQL Azure)](sql-database-configure-firewall-settings.md).


### <a name="configure-logins-and-database-users"></a>Konfiguriranje prijave i baze podataka korisnicima

Morate Provjerite postoji li sve prijave koristi aplikacija na poslužitelju na kojem se nalaze oporavljene baze podataka. Dodatne informacije potražite u članku [Konfiguracija sigurnosti zemlj replikacije](sql-database-geo-replication-security-config.md).

>[AZURE.NOTE] Trebali biste konfigurirati i testiranje pravila vatrozida poslužitelja i prijave (i njihove dozvole) tijekom analize za oporavak Izrada. Tih objekata na razini poslužitelja i njihovu konfiguraciju možda neće biti dostupna tijekom se prekida.

### <a name="setup-telemetry-alerts"></a>Postavljanje upozorenja za Telemetriju

Morate da biste bili sigurni da biste mapirali oporavljene baze podataka i drugi poslužitelj će se ažurirati postojeće postavke upozorenja pravilo.

Dodatne informacije o pravilima upozorenja za baze podataka potražite u članku [Primanje obavijesti o upozorenje](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) i [Praćenje stanja servisa](../monitoring-and-diagnostics/insights-service-health.md).

### <a name="enable-auditing"></a>Omogućite nadziranje

Ako za pristup bazi podataka potrebna je provjera, morate omogućiti nadzor nakon oporavka baze podataka. Dobar pokazatelj nadzor da je potrebno je da klijentske aplikacije koristi sigurnu vezu nizove u uzorak *. database.secure.windows.net. Dodatne informacije potražite u članku [Uvod u SQL baze podataka nadzor](sql-database-auditing-get-started.md).


## <a name="next-steps"></a>Daljnji koraci

- Da biste saznali više o sigurnosne kopije baze podataka SQL Azure automatski, potražite u članku [baze podataka SQL automatskog sigurnosnog kopiranja](sql-database-automated-backups.md)
- Da biste saznali više o continuity dizajna i oporavak scenariji za tvrtke, potražite u članku [Continuity scenariji](sql-database-business-continuity.md)
- Dodatne informacije o korištenju automatizirano sigurnosno kopiranje za oporavak, potražite u članku [Vraćanje baze podataka iz sigurnosne kopije pokrenut servis](sql-database-recovery-using-backups.md)
