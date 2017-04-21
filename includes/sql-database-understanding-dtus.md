Jedinica transakcije baze podataka (DTU) je mjerne jedinice u SQL baze podataka koja predstavlja relativni moć baze podataka na temelju mjera stvarnog života: transakcije baze podataka. Ne možemo zapisivali skupa postupaka koji su uobičajeni za mrežna transakcija obrade zahtjeva (OLTP), a zatim mjeri koliko je transakcija nije moguće izvršiti sekundi u potpunosti učitan uvjete (koji je kratka verzija, možete pročitati kategorije pojedinosti u [Pregled usporednih](../articles/sql-database/sql-database-benchmark-overview.md)). 

Ako, na primjer, Premium P11 za rad sa baze podataka pomoću 1750 DTUs nudi 350 x više DTU izračunati power od osnovni baze podataka pomoću 5 DTUs. 

![Uvod u SQL baze podataka: jedna baza podataka DTUs sloju i razinu.](./media/sql-database-understanding-dtus/single_db_dtus.png)

>[AZURE.NOTE] Ako premještate postojeću bazu podataka sustava SQL Server, možete koristiti alat za trećih strana, [Kalkulator DTU baze podataka SQL Azure](http://dtucalculator.azurewebsites.net/), da biste dobili procjenu razinu performanse i usluge sloju bazu podataka možda će biti potrebno u bazi podataka SQL Azure.

### <a name="dtu-vs-edtu"></a>DTU nasuprot eDTU

DTU za jednu baze podataka prevodi izravno u eDTU za elastic baze podataka. Ako, na primjer, baze podataka u osnovni elastic baze podataka skup nudi eDTUs do 5. To je isti performanse kao jedan osnovni baze podataka. Razlika je elastic bazu podataka neće zauzeti sve eDTUs iz na resurse dok je on se. 

![Uvod u SQL baze podataka: Elastic grupe tako da sloju.](./media/sql-database-understanding-dtus/sqldb_elastic_pools.png)

Pomaže u primjeru jednostavne. Iskoristite skup osnovni elastic baze podataka s 1000 DTUs i ispustite 800 baze podataka u njoj. Pod uvjetom da koriste samo 200 800 baze podataka u bilo kojem trenutku u vremenu (5 DTU X 200 = 1000) ne kliknete kapacitet u grupu, a ne slabije performanse baze podataka. U ovom se primjeru pojednostavnjeni je za prikaz. Realni matematičke je nešto složenije. Na portalu ne matematičke operacije umjesto vas, a čini preporuke korištenje povijesne baze podataka na temelju. U odjeljku [cijena i performanse zahtjevi za grupe aplikacija programa elastic baze podataka](../articles/sql-database/sql-database-elastic-pool-guidance.md) da biste saznali kako funkcioniraju preporuke ili da biste učinili matematičke operacije. 
