<properties 
   pageTitle="Azure SQL baza podataka performanse uvid | Microsoft Azure" 
   description="Baza podataka SQL Azure nudi alate performanse da biste lakše prepoznali područja koja mogu poboljšati performanse trenutni upit." 
   services="sql-database" 
   documentationCenter="" 
   authors="stevestein" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management" 
   ms.date="07/19/2016"
   ms.author="sstein"/>

# <a name="sql-database-performance-insight"></a>Uvid performanse baze podataka za SQL

Baze podataka SQL Azure nudi performanse alate za prepoznavanje i poboljšati performanse od vaših baza unosom Inteligentna ugađanje akcije i preporuke. 

1. Otvorite bazu podataka na [Portalu Azure](http://portal.azure.com) , a zatim kliknite **sve postavke** > **performanse **  >  **Pregled** da biste otvorili stranicu **performansi** . 


2. Kliknite **preporuke** da biste otvorili [Savjetnik za baze podataka SQL](#sql-database-advisor)pa kliknite **upiti** da biste otvorili [Uvid performanse upita](#query-performance-insight).

    ![Performanse](./media/sql-database-performance/entries.png)



## <a name="performance-overview"></a>Pregled performansi

Klikom na **Pregled** ili na pločici **performanse** će vas odvesti na nadzorna ploča za bazu podataka. Prikaz sažetka performansi baze podataka, a pomaže vam upravljanje performansama i otklanjanje poteškoća. 

![Performanse](./media/sql-database-performance/performance.png)

- Pločica **preporuke** nudi razrada svega usklađivanje preporuke za bazu podataka (vrh 3 preporuke prikazane su ako postoji više). Klikom na toj pločici vodi vas na **Savjetnik za baze podataka SQL**. 
- Pločica **Tuning aktivnosti** sažetka tijeku i dovršene usklađivanje akcije za bazu podataka, što vam omogućuje brz prikaz u povijesti usklađivanje aktivnosti. Klikom na toj pločici vodi vas na punu ugađanje prikaza povijest za bazu podataka.
- **Automatsko prilagođavanje** pločica prikazuje konfiguracije automatsko prilagođavanje za bazu podataka (akcija koje ugađanje konfigurirani tako da se primjenjuje na bazu podataka). Klikom na toj pločici otvorit će se dijaloški okvir za konfiguraciju automatizaciju.
- Pločica **upita baze podataka** prikazuje sažetak performanse upita za bazu podataka (Ukupna DTU korištenje i prvih resurs troše upite). Klikom na toj pločici vodi vas na **Uvid performanse upita**.



## <a name="sql-database-advisor"></a>Savjetnik za baze podataka SQL


[Savjetnik za baze podataka SQL](sql-database-advisor.md) nudi Inteligentna preporuke za ugađanje kojih možete poboljšati performanse svoje baze podataka. 

- Preporuke o kojem indeksi da biste stvarali i ispustite (i mogućnost da biste primijenili indeks preporuke automatski bez interakcije s korisnikom i povratak automatski preporuke koji imaju negativno utjecati na performanse).
- Preporuke za probleme sheme prepoznaju u bazi podataka.
- Preporuke za upite možete koristiti uz istovremeno s parametrima upita.




## <a name="query-performance-insight"></a>Uvid performanse upita

[Upit performanse uvid](sql-database-query-performance.md) omogućuje vam da manje vremena performanse baze podataka za otklanjanje poteškoća unosom:

- Detaljnije uvid u svoje potrošnje baze podataka resursa (DTU). 
- Gornji procesora koristi upite, možete potencijalno moguće je počeo gledati za bolje performanse. 
- Mogućnost dubinski analizirati prema dolje detalje upita. 


## <a name="additional-resources"></a>Dodatni resursi

- [Azure SQL baze podataka performanse smjernice za jednu baze podataka](sql-database-performance-guidance.md)
- [Kada se skup elastic baze podataka želite koristiti?](sql-database-elastic-pool-guidance.md)