<properties
   pageTitle="Izrada oporavak solutions - SQL baza podataka aktivna zemlj. – replikacijom u oblaku | Microsoft Azure"
   description="Saznajte kako dizajnirati oblaka Izrada oporavak rješenja za tvrtke continuity planiranje pomoću zemlj replikacije za sigurnosno kopiranje podataka aplikacije s bazom podataka SQL Azure."
   keywords="Izrada oporavak, Izrada oporavak rješenja, sigurnosno kopiranje podataka aplikacije, zemlj replikacije u oblaku tvrtke continuity planiranje"
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
   ms.date="07/20/2016"
   ms.author="sashan"/>

# <a name="design-an-application-for-cloud-disaster-recovery-using-active-geo-replication-in-sql-database"></a>Dizajniranje aplikacije za oblak Izrada oporavak u bazi podataka SQL pomoću aktivni replikacije zemlj.


> [AZURE.NOTE] [Aktivni replikacije zemlj](sql-database-geo-replication-overview.md) sada je dostupan za sve baze podataka u sve razine.



Saznajte kako koristiti [Active replikacije zemlj](sql-database-geo-replication-overview.md) u bazi podataka SQL dizajna baze podataka aplikacije prebacuju regionalne pogrešaka i do teškog oštećenja kvarove. Za tvrtke continuity planiranje, razmislite o implementaciji Topologija aplikacije, na razini ugovor o usluzi birate, promet latencije i troškove. U ovom se članku smo pogledajte uobičajene uzorke aplikacije i razgovarati o prednosti i gubitke svake mogućnosti. Informacije o Active zemlj. – replikacije s Elastic grupe potražite u članku [Strategije oporavka Izrada Elastic skup](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).

## <a name="design-pattern-1-active-passive-deployment-for-cloud-disaster-recovery-with-a-co-located-database"></a>Dizajn uzorak 1: aktivno pasivni uvođenje za oporavak Izrada oblaka s bazom podataka za zajednički nalazi

Ta je mogućnost najprikladniji za aplikacije sa sljedećim karakteristikama:

+ Aktivni instancu u jednom području Azure
+ Istaknuti ovisnosti na čitanje i pisanje (RW) pristup podacima
+ Povezivanje regije-između logike aplikacije i baza podataka nije prihvatljiva zbog Latencija i promet trošak    

U ovom slučaju implementacije Topologija aplikacije optimiziran je za obradu regionalne disasters prilikom svih komponenti aplikacije su utjecati i morate prebacivanje kao jedinica. Za geografske zalihosti replicirati logike računala i baze podataka u drugoj regiji, ali ne koriste se za radno opterećenje aplikacije u običnom uvjetima. Aplikacije u području sekundarni mora biti konfiguriran za korištenje SQL niz za povezivanje s bazom podataka sustava sekundarni. Upravitelj promet je postaviti tako da koristiti [metodu usmjeravanja prebacivanje](../traffic-manager/traffic-manager-configure-failover-routing-method.md).  

> [AZURE.NOTE] [Upravitelj Azure promet](../traffic-manager/traffic-manager-overview.md) se koristi u ovom se članku radi ilustracije. Možete koristiti rješenjem ujednačavanje opterećenja koji podržava metodu usmjeravanja prebacivanje.    

Osim instance glavnog računala, razmislite o implementaciji mala [aplikacija ulogu suradnika](cloud-services-choose-me.md#tellmecs) koji nadzire primarnom bazom podataka slanjem periodičku T SQL samo za čitanje (Retke) naredbe. Možete je koristiti za automatsko pokretanje prebacivanje, da biste generirali upozorenja na konzoli za administratore vaše aplikacije ili za oba. Da biste bili sigurni da nadzor ne utječe kvarove razini regija, implementirati nadzora instanci aplikacije za svako područje i imaju svaku instancu povezani s primarnom bazom podataka u području primarni i sekundarni baze podataka u lokalnom regiji. 

> [AZURE.NOTE] I nadzor aplikacija mora biti aktivna i isprobati primarnih i sekundarnih baze podataka. Drugu mogućnost se može se koristiti da biste otkrili pogreške u sekundarni regija i upozorenje ako aplikacija nije zaštićen.     

Sljedeći dijagram prikazuje tu konfiguraciju prije nego što se prekida.

![Konfiguracija zemlj. – replikacije baze podataka SQL. Oporavak Izrada oblaka.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-1.png)

Nakon prekida u području primarni nadzor aplikacija otkriva primarnom bazom podataka nije dostupna, a registrira upozorenja. Ovisno o SLA vašeg računala možete odlučiti koliko uzastopnih nadzora probes treba uspjeti prije nego što je deklarira baze podataka prekida. Da biste postigli usklađenih prebacivanje točka računala i baze podataka, imat ćete nadzor aplikacija poduzeti sljedeće korake:

1. [Ažuriranje statusa primarni točka](https://msdn.microsoft.com/library/hh758250.aspx) da bi se pokrenuti prebacivanje točka.
2. Nazovite sekundarni baze podataka da biste [započeli Prebacivanje baze podataka](sql-database-geo-replication-portal.md).

Nakon prebacivanje, aplikacija će obrade zahtjeva za korisnika u području sekundarne, ali će ostati Suradnja nalazi s bazom podataka jer primarnom bazom podataka sada će se u području sekundarne. Ovaj scenarij pokazuju sljedeći dijagram. U sve dijagrame pune crte određivanje aktivne veze, točkaste crte ukazuju obustavljenom veze, a zatim znak tabulatora označava okidača akcija.


![Zemlj replikacije: Prebacivanje sekundarne bazu podataka. Sigurnosno kopiranje podataka aplikacije.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-2.png)

Ako se prekida se dogodi u području sekundarne, obustavljena je replikacije vezu između primarnih i sekundarnih baze podataka i nadzor aplikacija registrira upozorenje prikazuje se primarnom bazom podataka. Performanse aplikacije neće utjecati, ali aplikacije pristajete prikazanog i stoga pri riziku u slučaju obje regije neće funkcionirati u nizu.

> [AZURE.NOTE] Preporučujemo da samo konfiguracije implementaciju s jednog područja DR. Ovo je jer Većina Azure geographies imaju dva područja. Ove konfiguracije će zaštitite svoju aplikaciju od Katastrofalna pogreška oba područja. Vjerojatno događaj takve pogreške, možete oporaviti baze podataka u regiji treći pomoću [operacije zemlj vraćanja](sql-database-disaster-recovery.md#recovery-using-geo-restore).

Kada se prekida se mitigated, sekundarne baze podataka se automatski sinkronizira s primarni. Tijekom sinkronizacije, performanse primarni može se malo utjecati ovisno o količinu podataka koje su potrebne za sinkronizaciju. Sljedeći dijagram prikazuje se prekida u području sekundarni.

![Sekundarni baza podataka sinkronizirana s primarni. Oporavak Izrada oblaka.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-3.png)


Ključne **prednosti** ovaj uzorak dizajn su:

+ SQL niz za povezivanje na svakom području postavljeno tijekom implementacije aplikacija, a ne mijenja nakon prebacivanje.
+ Prebacivanje kao aplikacija ne utječe performanse aplikacije i baza podataka nalaze se uvijek Suradnja.

Glavni **tradeoff** je da instancu suvišne aplikacije u području sekundarni koristi samo za oporavak Izrada.

## <a name="design-pattern-2-active-active-deployment-for-application-load-balancing"></a>Dizajn uzorak 2: uvođenje aktivno aktivno za aplikaciju opterećenja
Ta oblaka Izrada oporavak je mogućnost najprikladniji za aplikacije sa sljedećim karakteristikama:

+ Čita visoka omjer baze podataka za zapisivanje
+ Latencija pisanje baze podataka ne utječu na sučelje za krajnjeg korisnika  
+ Samo za čitanje logike biti odvojeni od logike za čitanje i pisanje pomoću niza za povezivanje različitih
+ Logika samo za čitanje ne ovisi o podacima koji se u potpunosti sinkronizirana s najnovijim ažuriranjima  

Ako aplikacija te karakteristike, krajnji korisnik veze preko više instanci aplikacije u različitim područjima za ujednačavanje opterećenja možete poboljšati performanse i sučelje za krajnjeg korisnika. Možete implementirati ujednačavanje opterećenja, svako područje imat instancu aktivne aplikacije s logike (RW) čitanja i pisanja povezan s primarnom bazom podataka u području primarni. Sekundarni baze podataka u istom području kao instanca aplikacije mora biti povezan (Retke) logike samo za čitanje. Upravitelj promet trebali postaviti za uporabu [točka nadzor](../traffic-manager/traffic-manager-monitoring.md) omogućen za svaku instancu aplikacije [kružnog dodjeljivanja usmjeravanje](../traffic-manager/traffic-manager-configure-round-robin-routing-method.md) ili [performanse usmjeravanja](../traffic-manager/traffic-manager-configure-performance-routing-method.md) .

Kao uzorak #1, razmislite o implementaciji slične nadzor aplikacija. No za razliku od uzorak #1, nadzor aplikacija neće smatrati ODGOVORNIM ni za pokretanje prebacivanje točka.

> [AZURE.NOTE] Dok ovaj uzorak koristi više od jedne sekundarne baze podataka, samo jedan od na secondaries želite koristiti za prebacivanje razloga zabilježili neke starije verzije. Ovaj uzorak zahtijeva pristup samo za čitanje sekundarnom, bit će potrebno aktivni replikacije zemlj..

Upravitelj promet trebali biste konfigurirati performanse usmjeravanje usmjerite veze korisnika na najbliži na korisnikovu geografsku lokaciju instancu aplikacije. Sljedeći dijagram prikazuje tu konfiguraciju prije nego što se prekida.
![Bez prekida: performanse usmjeravanja na najbliži aplikacije. Replikacija zemlj..](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-1.png)

Ako je baza podataka prekida otkriven u primarni regiji, uputite prebacivanje primarni baze podataka jedan sekundarni područja promjene mjesta primarnom bazom podataka. Upravitelj promet automatski isključuje izvanmrežne točka iz tablicu usmjeravanja no i dalje usmjeravanje prometa krajnjeg korisnika za preostale online instance. Budući da primarni baze podataka sada u nekoj drugoj regiji, sve instance online morate promijeniti svoje čitanja i pisanja SQL niz za povezivanje za povezivanje s novi primarni. Nije važno da ste izvršili tu promjenu prije započinjanja Prebacivanje baze podataka. Nizove veze SQL samo za čitanje treba ostaviti nepromijenjen kao što je uvijek pokažite na bazu podataka u istom području. Koraci za prebacivanje su:  

1. Promijenite pokažite na novu primarni čitanja i pisanja SQL vezu nizova.
2. Nazovite određenu sekundarne baze podataka prema [Prebacivanje pokrenete bazu podataka](sql-database-geo-replication-portal.md).

Sljedeći dijagram prikazuje Nova konfiguracija nakon na prebacivanje.
![Konfiguracija nakon prebacivanje. Oporavak Izrada oblaka.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-2.png)

U slučaju prekida u jednom sekundarni područja Upravitelj promet iz automatski uklanjaju izvanmrežne točka u tom području u tablicu usmjeravanja. Obustavljena je kanala replikaciju sekundarnu bazu podataka u tom području. Budući da ostala područja dobili dodatne korisničke promet u ovom scenariju, performanse aplikacije utječe tijekom se prekida. Kada se prekida se mitigated, sekundarne baze podataka u području impacted odmah se sinkronizira s primarni. Tijekom sinkronizacije performanse primarni može se malo utjecati ovisno o količinu podataka koje su potrebne za sinkronizaciju. Sljedeći dijagram prikazuje se prekida u jednom sekundarne područja.

![Prekida u sekundarni regiji. Izrada oporavak - zemlj replikacije u oblaku.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-3.png)

Ključne **prednosti** ovaj uzorak dizajna je skalirali radno opterećenje aplikacije preko više secondaries da biste postigli na optimalne performanse krajnjeg korisnika. **Tradeoffs** tu mogućnost su:

+ Čitanje i pisanje veze između instanci računala i baze podataka su različitim latencije i trošak
+ Performanse aplikacije utječe tijekom na prekida
+ Aplikacija instanci su potrebne za dinamično promjena SQL niz za povezivanje nakon Prebacivanje baze podataka.  

> [AZURE.NOTE] Sličan pristup može se koristiti za offload specijalizirane radnih opterećenja kao što su izvješća zadataka, alata za poslovnu inteligenciju ili sigurnosne kopije. Obično te radnih opterećenja zauzeti značajnu baze podataka resursa stoga preporučuje se da odredite jedan sekundarni baze podataka za njih s razinom performansi uparuje predviđenu radno opterećenje.

## <a name="design-pattern-3-active-passive-deployment-for-data-preservation"></a>Dizajn uzorak 3: aktivna pasivni implementaciju za čuvanje podataka  
Ta je mogućnost najprikladniji za aplikacije sa sljedećim karakteristikama:

+ Gubitka podataka je rizik visoke tvrtke. Prebacivanje baze podataka mogu se koristiti samo ne uspije ako je trajno se prekida.
+ Aplikaciju možete raditi u "načinu samo za čitanje" za određeno vremensko razdoblje.

U ovaj uzorak aplikacije prelazi u načinu samo za čitanje kada ste povezani s sekundarne baze podataka. Logika aplikacije u području primarni nalazi se Suradnja s primarnom bazom podataka i pristajete u načinu za čitanje i pisanje (RW). Logika aplikacije u području sekundarne nalazi se Suradnja sa sekundarnom baze podataka i spremno za rad u načinu samo za čitanje (Retke).  Upravitelj promet trebali postaviti za uporabu [Prebacivanje usmjeravanje](../traffic-manager/traffic-manager-configure-failover-routing-method.md) [točka nadzor](../traffic-manager/traffic-manager-monitoring.md) omogućen za oba instanci aplikacije.

Sljedeći dijagram prikazuje tu konfiguraciju prije nego što se prekida.
![Implementacija aktivno pasivni prije prebacivanja u slučaju pogreške. Oporavak Izrada oblaka.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-1.png)

Kada upravitelj promet otkrije neuspješno povezivanje primarni regija, automatski prelazi korisnika promet instanci aplikacije u području sekundarne. Uz ovaj uzorak je važno te **ne** pokretanje Prebacivanje baze podataka nakon otkrije se prekida. Aplikacija u području sekundarni je aktiviran i funkcionira u načinu samo za čitanje pomoću sekundarne baze podataka. To je slaganje na sljedećem su dijagramu.

![Prekida: Aplikacija u načinu samo za čitanje. Oporavak Izrada oblaka.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-2.png)

Nakon prekida u području primarni je mitigated, promet Upravitelj otkriva vraćanje povezivanje u području primarnog i prelazi korisnika promet natrag na instancu aplikacije u području primarni. Tu instancu aplikacije primijenila i pristajete u načinu za čitanje i pisanje pomoću primarnom bazom podataka.

> [AZURE.NOTE] Budući da se ovaj uzorak potreban pristup samo za čitanje sekundarne bit će potrebno aktivni replikacije zemlj..

U slučaju prekida u području sekundarne upravitelju promet označava točka aplikacije u području primarni su smanjene značajke, a zatim se obustavlja replikacije kanala. Međutim, ovaj nedostupnosti utjecati performanse aplikacije tijekom se prekida. Kada se prekida se mitigated, sekundarne baze podataka odmah se sinkronizira s primarni. Tijekom sinkronizacije performanse primarni može se malo utjecati ovisno o količinu podataka koje su potrebne za sinkronizaciju.

![Prekida: Sekundarni bazu podataka. Oporavak Izrada oblaka.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-3.png)

Ovaj uzorak dizajn ima nekoliko **prednosti**:

+ Ga izbjegava gubitka podataka tijekom privremene kvarove.
+ To nije potrebno implementaciju aplikacija za nadzor, kao što je oporavak aktivira Upravitelj promet.
+ Nedostupnost ovisi o samo brzinu promet Upravitelj otkrije pogreška povezivanje koji se može konfigurirati.

**Tradeoffs** su:

+ Aplikacija moraju imati mogućnost raditi u načinu samo za čitanje.
+ Potrebna je za dinamično prebaciti na njega, kada je povezano s bazom podataka samo za čitanje.
+ Resumption operacija čitanja i pisanja zahtijeva oporavak primarni regije, što znači svih podataka access može biti onemogućen zbog sata ili čak i dana.

> [AZURE.NOTE] U slučaju trajna servisa prekida u regiji, morate ručno aktivirati Prebacivanje baze podataka i prihvatite gubitka podataka. Aplikacija će funkcionirati u području sekundarni s čitanja i pisanja pristup bazi podataka.

## <a name="business-continuity-planning-choose-an-application-design-for-cloud-disaster-recovery"></a>Planiranje continuity tvrtke: birate dizajn aplikacije za oporavak Izrada oblaka

Strategije za oporavak Izrada određene oblaka možete kombinirati ili proširivanje te dizajn uzoraka najbolje prema potrebama vaše aplikacije.  Kao što je rečeno ranije, strategije odaberete temelji se na SLA želite ponuditi klijentima i implementaciju Topologija aplikacije. Da biste lakše vodič za svoj odabir, u sljedećoj su tablici uspoređuje gubitka Procijenjena podataka na temelju odabira ili oporavak pokažite cilj (RPO) i očekivano oporavak vrijeme (UMETNI).

| Uzorak | RPO | UMETNI
| :--- |:--- | :---
| Uvođenje aktivno pasivni za oporavak Izrada pomoću Suradnja nalazi baze podataka programa access | Pristup za čitanje i pisanje < 5 sec | Vrijeme otkrivanja neuspjelog + prebacivanje API poziva + aplikacije potvrdu testiranje
| Uvođenje aktivno aktivno za aplikaciju opterećenja | Pristup za čitanje i pisanje < 5 sec | Vrijeme otkrivanja neuspjelog + prebacivanje API poziva + niz za povezivanje SQL promjena + aplikacije potvrdu testiranje
| Uvođenje aktivno pasivni za čuvanje podataka | Pristup samo za čitanje < 5 s dozvolom za uređivanje = nula | Pristup samo za čitanje = vrijeme za otkrivanje nije uspjelo povezivanje + test za potvrdu aplikacije <br>Pristup za čitanje i pisanje = vrijeme da biste smanjili na prekida

## <a name="next-steps"></a>Daljnji koraci

- Da biste saznali više o sigurnosne kopije baze podataka SQL Azure automatski, potražite u članku [baze podataka SQL automatskog sigurnosnog kopiranja](sql-database-automated-backups.md)
- Pregled za continuity tvrtke i scenariji potražite u članku [Pregled continuity tvrtke](sql-database-business-continuity.md)
- Dodatne informacije o korištenju automatizirano sigurnosno kopiranje za oporavak, potražite u članku [Vraćanje baze podataka iz sigurnosne kopije pokrenut servis](sql-database-recovery-using-backups.md)
- Da biste saznali više o brže mogućnosti za oporavak, potražite u članku [Aktivno zemlj replikacije](sql-database-geo-replication-overview.md)  
- Dodatne informacije o korištenju automatizirano sigurnosno kopiranje za arhiviranje, pročitajte članak [kopiranje baze podataka](sql-database-copy.md)
- Informacije o Active zemlj. – replikacije s Elastic grupe potražite u članku [Strategije oporavka Izrada Elastic skup](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
