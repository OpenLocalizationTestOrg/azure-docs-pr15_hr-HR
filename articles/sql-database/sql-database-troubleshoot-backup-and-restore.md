<properties
    pageTitle="Otklanjanje poteškoća s sigurnosnog kopiranja i vraćanja s bazom podataka SQL Azure"
    description="Saznajte kako oporaviti oblaka baze podataka iz pogreške i kvarove pomoću sigurnosnog kopiranja i replike u bazi podataka SQL Azure."
    services="sql-database"
    documentationCenter=""
    authors="dalechen"
    manager="felixwu"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2016"
    ms.author="daleche"/>

# <a name="restore-a-database-to-a-previous-point-in-time-restore-a-deleted-database-or-recover-from-a-data-center-outage"></a>Vraćanje baze podataka na prethodnu točku u vremenu, vraćanje izbrisanih baze podataka ili oporavak nedostupnosti za centar za podataka

Baze podataka SQL zadržava replike baze podataka da bi se možete vratiti iz kvarove i pogreške korisnika. Dostupne mogućnosti ovise o sloju baze podataka usluge i mogućnostima koje odaberete. Potražite u članku [Pregled Continuity tvrtke](sql-database-business-continuity.md) detalje i zahtjevi dizajna.

## <a name="to-restore-a-database-to-a-previous-point-in-time"></a>Da biste vratili bazu podataka na prethodnu točku u vremenu
1.  [Portal za Azure](https://azure.microsoft.com/)kliknite **baze podataka SQL**.
2.  Na popisu odaberite bazu podataka, a zatim kliknite **Vrati**.
3.  Upišite novi naziv za bazu podataka, odaberite datum i vrijeme da biste vratili iz, a zatim kliknite **Stvaranje.**
4.  Aplikacija prilagodbe prema potrebi referentni novu bazu podataka. Potražite u članku [oporavak bazu podataka za točku u vremenu](sql-database-recovery-using-backups.md#point-in-time-restore).

## <a name="to-restore-an-accidentally-deleted-database"></a>Da biste vratili slučajno izbrisane baze podataka
1.  [Portal za Azure](https://azure.microsoft.com/)kliknite **SQL poslužitelja**.
2.  Odaberite poslužitelj koji hostira baze podataka s popisa.
3.  Na Elektronička ploča poslužitelja pomaknite se prema dolje, a zatim **izbrisani baze podataka**.
4.  Odaberite bazu podataka da biste vratili, a zatim kliknite **Stvori**.
5.  Aplikacija prilagodbe prema potrebi referentni novu bazu podataka. Potražite u članku [oporavak izbrisanih baze podataka](sql-database-recovery-using-backups.md#deleted-database-restore).

## <a name="to-recover-from-a-regional-datacenter-outage"></a>Da biste vratili iz regionalnog podatkovnog centra prekida
S bazama podataka standardnu i Premium, ako postavite zemlj replicirati secondaries možete oporaviti pomoću ove secondaries. To vam omogućuje da biste vratili bazu podataka s manje potencijalni gubitka podataka. Potražite u članku [oporavak baze podataka Azure SQL pomoću sigurnosne kopije baze podataka za automatiziranog](sql-database-disaster-recovery.md) detalje.

Azure omogućuje i kopija svake baze podataka u nekoj drugoj regiji (zemlj suvišnih sigurnosnu kopiju). Možete stvoriti novu bazu podataka iz ove sigurnosne kopije, koja se naziva zemlj vraćanja, ali je vjerojatno da ćete doći gubitka podataka ako se oslanjate na taj način samostalno.

**Da biste vratili bazu podataka pomoću zemlj vraćanja:**

- [Portal za Azure](https://azure.microsoft.com/)kliknite **Novo**, kliknite **podataka i prostora za pohranu**, kliknite **SQL baze podataka**, a zatim **sigurnosno kopiranje** kao izvor baze podataka. Potražite u članku [oporavak baze podataka Azure SQL iz programa nedostupnosti](sql-database-disaster-recovery.md) detalje.