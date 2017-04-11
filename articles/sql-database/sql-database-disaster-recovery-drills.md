<properties 
   pageTitle="SQL baza podataka Izrada oporavak Bušilice | Microsoft Azure" 
   description="Dodatne upute i najbolje prakse za korištenje baze podataka SQL Azure za izvođenje Izrada oporavak bušilice koji će pomoći zadržati svoje zaštita njihove privatnosti ovise ključnih poslovnih aplikacija prebacuju pogrešaka i kvarove." 
   services="sql-database" 
   documentationCenter="" 
   authors="anosov1960" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management" 
   ms.date="07/31/2016"
   ms.author="sstein; sashan"/>

#<a name="performing-disaster-recovery-drill"></a>Izvođenje Izrada oporavak analize

Preporučuje se povremeno provodi provjeru spremnosti za aplikaciju za tijek rada za oporavak. Provjera ponašanja aplikacije i posljedice od gubitka podataka i/ili u prekidu obuhvaća taj pomoćni inženjerskim dobro je. Preporučuje se i zahtjevi po Većina standardima kao dio tvrtke continuity certifikata.

Detaljnu analizu oporavak Izrada sastoji se od:

- Simulaciju nedostupnosti sloju podataka
- Oporavak 
- Provjerite valjanost oporavak objavu integritet aplikacije

Ovisno o kako [namijenjene aplikacija za poslovanje](sql-database-business-continuity.md), tijek rada za izvršavanje dubinske analize mogu se razlikovati. U nastavku ćemo opisuje najbolje prakse za vođenje analize za oporavak Izrada u kontekstu baze podataka SQL Azure. 

##<a name="geo-restore"></a>Vraćanje za zemlj.

Da biste spriječili mogućem gubitku podataka kada vođenje analize za oporavak Izrada, preporučujemo da izvođenja analize pomoću okruženje za testiranje stvaranjem kopiju radnog okruženja te ga koristiti da biste provjerili tijeka rada za prebacivanje s aplikacije.
 
####<a name="outage-simulation"></a>Prekida simulacije

U programu Publisher na nedostupnosti brisanje ili preimenovanje izvorišne baze podataka. Time će se pogreška povezivanja programa. 

####<a name="recovery"></a>Oporavak

- Izvođenje zemlj Vraćanje baze podataka u drugi poslužitelj kao što je opisano [u nastavku](sql-database-disaster-recovery.md). 
- Promjena konfiguracije aplikacije za povezivanje s oporavljene baze podataka i slijedeći upute u vodiču za [Konfiguriranje baze podataka nakon oporavka](sql-database-disaster-recovery.md) da biste dovršili oporavka.

####<a name="validation"></a>Provjera valjanosti

- Dovršite dubinske analize provjerom aplikacije integritet objavu oporavka (odnosno nizove veze, prijave, testiranje osnovne funkcije ili drugi dio provjere od postupaka signoffs standardne aplikacije).

##<a name="geo-replication"></a>Zemlj replikacije

Za bazu podataka koja je zaštićena pomoću zemlj replikacije vježbu dubinske analize će obuhvaćaju planiranog prebacivanje na sekundarnom bazu podataka. Planirani prebacivanje osigurava da primarnih i sekundarnih baze podataka ostaju u sinkronizaciji kada se promijenio uloge. Za razliku od neplanirano prebacivanje ovaj postupak neće rezultirati gubitka podataka, tako da se može izvršiti dubinske analize u radnom okruženju. 

####<a name="outage-simulation"></a>Prekida simulacije

U programu Publisher nedostupnosti možete onemogućiti web-aplikacija ili virtualnog računala koji su povezani s bazom podataka. To će rezultirati neuspjeha povezivanje za web-klijentima.

####<a name="recovery"></a>Oporavak

- Provjerite je li konfiguracija aplikacije u regiji DR upućuje na bivši sekundarni koji će postati potpuno pristupiti novi primarni. 
- Izvođenje [Planirano prebacivanje](sql-database-geo-replication-powershell.md#initiate-a-planned-failover) da bi novi primarni sekundarne baze podataka
- Slijedite vodič za [Konfiguriranje baze podataka nakon oporavka](sql-database-disaster-recovery.md) da biste dovršili oporavka.

####<a name="validation"></a>Provjera valjanosti

- Dovršite dubinske analize provjerom aplikacije integritet objavu oporavka (odnosno nizove veze, prijave, testiranje osnovne funkcije ili drugi dio provjere od postupaka signoffs standardne aplikacije).


## <a name="next-steps"></a>Daljnji koraci

- Da biste saznali više o poslovnim continuity scenarijima, potražite u članku [Continuity scenariji](sql-database-business-continuity.md)
- Da biste saznali više o sigurnosne kopije baze podataka SQL Azure automatski, potražite u članku [baze podataka SQL automatskog sigurnosnog kopiranja](sql-database-automated-backups.md)
- Dodatne informacije o korištenju automatizirano sigurnosno kopiranje za oporavak, potražite u članku [Vraćanje baze podataka iz sigurnosne kopije pokrenut servis](sql-database-recovery-using-backups.md)
- Da biste saznali više o brže mogućnosti za oporavak, potražite u članku [Aktivno zemlj replikacije](sql-database-geo-replication-overview.md)  
