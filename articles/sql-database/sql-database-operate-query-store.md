<properties
   pageTitle="Radi pohrane upita u bazi podataka Azure SQL"
   description="Saznajte kako raditi u spremište upita u bazi podataka SQL Azure"
   keywords=""
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="sqldb-performance"
   ms.workload="data-management"
   ms.date="08/16/2016"
   ms.author="carlrab"/>

# <a name="operating-the-query-store-in-azure-sql-database"></a>Radi pohrane upita u bazi podataka Azure SQL 

Spremište upita u Azure je značajka potpuno upravljanih baze podataka koja kontinuirano prikuplja i predstavlja povijesnim informacija o sve upite. Možete smatrati o spremišta upita slična je aviona leta podataka snimač značajno pojednostavljuje i oblaka otklanjanje poteškoća s performansama upita i Lokalni korisnici. U ovom se članku objašnjava određene vidove radi pohrane upita u Azure. Pomoću ove podatke unaprijed prikupljene upita, možete brzo dijagnosticiranje i Riješite probleme s performansama i stoga provedu još jedanput usredotočite se na svoje poslovanje. 

Spremište upita je [globalno dostupne](https://azure.microsoft.com/updates/general-availability-azure-sql-database-query-store/) u bazi podataka SQL Azure od studenom, 2015. Spremište upita su temelj za analizu performansi i ugađanje značajke, kao što su [Savjetnik za baze podataka SQL i nadzorna ploča](https://azure.microsoft.com/updates/sqldatabaseadvisorga/). Trenutku objavljivanja u ovom se članku spremište upit se izvodi u više od 200,000 korisnika baze podataka u Azure, prikupljanje informacija vezanih uz upit nekoliko mjeseca, bez prekida.

> [AZURE.IMPORTANT] Microsoft je u tijeku aktiviranje spremišta upita za sve Azure SQL baze podataka (postojeće i novo). 

## <a name="optimal-query-store-configuration"></a>Konfiguracija optimalnih spremnika upita

U ovom se odjeljku opisuju zadane postavke optimalnih konfiguracije namijenjene da biste bili sigurni pouzdanog operacija trgovine upita i o njima ovisne značajke, kao što su [Savjetnik za baze podataka SQL i nadzorna ploča](https://azure.microsoft.com/updates/sqldatabaseadvisorga/). Konfiguriranje zadanog optimiziran je za prikupljanje neprekinuti podataka koji je minimalnog vrijeme utrošeno na ISKLJUČENO/READ_ONLY stanja.

| Konfiguracija | Opis | Zadani | Komentar |
| ------------- | ----------- | ------- | ------- |
| MAX_STORAGE_SIZE_MB | Određuje ograničenje prostora podataka upita spremišta podataka može potrajati unutar z Korisnička baza podataka | 100 | Provesti za nove baze podataka |
| INTERVAL_LENGTH_MINUTES | Određuje veličinu doba dana tijekom kojeg pridružuje i ista i statističkih podataka prikupljenih runtime za tarife upita. Svaki tarifa aktivnog upita sadrži najviše jedan redak za neko vrijeme definirana pomoću tu konfiguraciju | 60   | Provesti za nove baze podataka |
| STALE_QUERY_THRESHOLD_DAYS | Čišćenje temeljene pravila koja određuje razdoblje zadržavanja postojanog runtime Statistika i neaktivne upita | 30 | Primijeniti na nove baze podataka i bazama podataka s prethodne zadane (367) |
| SIZE_BASED_CLEANUP_MODE | Određuje je li čišćenja podataka odvija veličina podataka iz trgovine upita izvršenja ograničenje | AUTOMATSKO | Provesti za sve baze podataka |
| QUERY_CAPTURE_MODE | Određuje hoće li se prate sve upite ili samo podskup upita | AUTOMATSKO | Provesti za sve baze podataka |
| FLUSH_INTERVAL_SECONDS | Određuje najveći razdoblje tijekom kojeg zabilježene runtime Statistika čuvaju u memoriji, prije nikakvu na disk | 900 | Provesti za nove baze podataka |
||||||

> [AZURE.IMPORTANT] Zadane vrijednosti primjenjuju automatski u faza aktivacije spremišta upita u sve baze podataka Azure SQL (pogledajte prethodni važne bilješke). Nakon ovog svijetlo gore baze podataka SQL Azure neće biti promjene konfiguracije vrijednosti postavio klijentima, osim ako ih ne mogu negativno utjecati na primarni radno opterećenje ili pouzdanog operacije spremišta upita.

Ako želite da ostanete pomoću prilagođenih postavki, koristite [ALTER baze podataka pomoću mogućnosti u spremište upita](https://msdn.microsoft.com/library/bb522682.aspx) da biste vratili konfiguracije prethodno stanje. Potražite u članku [Najbolje prakse s trgovinom u upit](https://msdn.microsoft.com/library/mt604821.aspx) da bi se Saznajte kako vrha odabrali optimalnih konfiguracije parametara.

## <a name="next-steps"></a>Daljnji koraci

[Uvid performanse baze podataka za SQL](sql-database-performance.md)

## <a name="additional-resources"></a>Dodatni resursi

Za dodatne informacije o odjavi u sljedećim člancima:

- [Snimač leta podataka baze podataka](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database) 

- [Praćenje performansi pomoću spremišta upita](https://msdn.microsoft.com/library/dn817826.aspx)

- [Scenariji za korištenje spremišta upita](https://msdn.microsoft.com/library/mt614796.aspx)

- [Praćenje performansi pomoću spremišta upita](https://msdn.microsoft.com/library/dn817826.aspx) 
