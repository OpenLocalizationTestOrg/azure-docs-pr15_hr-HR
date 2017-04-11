<properties
   pageTitle="Neprekidno poslovanje u oblaku – baze podataka oporavak - bazom podataka SQL | Microsoft Azure"
   description="Saznajte kako baze podataka SQL Azure podržava poslovanje u oblak i oporavak baze podataka te pomaže zadržati zaštita njihove privatnosti ovise ključnih oblaka aplikacija."
   keywords="neprekidno poslovanje, oblaka poslovanje, oporavak Izrada baze podataka, oporavak baze podataka"
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
   ms.workload="NA"
   ms.date="10/13/2016"
   ms.author="carlrab"/>

# <a name="overview-of-business-continuity-with-azure-sql-database"></a>Pregled poslovanje s bazom podataka SQL Azure

U ovom pregled opisuju mogućnosti koje bazom podataka SQL Azure nudi za poslovanje i Izrada oporavak. Pruža mogućnosti, preporuke i vodiči za oporavak u disruptive događaja koji bi mogli uzrokovati gubitak podataka ili uzrokovati baze podataka i aplikacije postane dostupna. Rasprave obuhvaća što učiniti kada se korisnik pogreška aplikacije utječe na integritet podataka, Azure regija sadrži do prekida ili aplikacije potreban je održavanja. 

## <a name="sql-database-features-that-you-can-use-to-provide-business-continuity"></a>Značajke SQL baze podataka koje možete koristiti za poslovanje

Baze podataka SQL sadrži nekoliko značajki continuity tvrtke, uključujući automatskog sigurnosnog kopiranja i replikacija nije obavezno baze podataka. Svaka ima različite karakteristike za procijenjenu oporavku (UMETNI) i mogućem gubitku podataka za nedavne transakcije. Kada saznate te mogućnosti možete odabrati među njima - i, u većini slučajeva, njihovo zajedničko korištenje za različitim scenarijima. Tijekom razvoja poslovni plan za continuity, potrebno je razumjeti Maksimalna prihvatljiva vrijeme nakon kojeg se aplikacija potpuno oporavlja nakon disruptive događaj – to je vaš cilj oporavak vrijeme (RTO). Morate razumjeti maksimalno povećali nedavnih ažuriranje podataka (razdoblje) aplikaciju možete tolerate gubitka prilikom oporavak nakon disruptive događaj – točke cilj oporavak (RPO). 

U sljedećoj su tablici uspoređuje UMETNI i RPO za tri Najčešći scenariji.

| Mogućnost |  Osnovni sloju | Standardni sloju  | Razina Premium |
|---|---|---|---|
| Pokažite na vrijeme vraćanje iz sigurnosne kopije | Bilo koji točku vraćanja unutar sedam dana   | Bilo koji točku vraćanja 35 dana  | Bilo koji točku vraćanja 35 dana |
Zemlj vraćanje iz sigurnosne kopije za replicirati na zemlj. | UMETNI < 12h, RPO < 1h   | UMETNI < 12h, RPO < 1h   | UMETNI < 12h, RPO < 1h |
|Aktivni replikacije zemlj. | < 30s, RPO < 5s UMETNI   | UMETNI < 30s, RPO < 5s | < 30s, RPO < 5s UMETNI |


### <a name="use-database-backups-to-recover-a-database"></a>Korištenje sigurnosne kopije baze podataka za oporavak baze podataka

SQL baze podataka automatski izvodi kombinaciju puni sigurnosne kopije baze podataka, tjedno, razlikovno baza podataka zaračunava sigurnosne kopije i zapisnik transakcija sigurnosne kopije svakih pet minuta da biste zaštitili svoju tvrtku od gubitka podataka. Ove sigurnosne kopije spremaju se u lokalno suvišnih prostora za pohranu 35 dana za baze podataka u servis razine standardnu i Premium i sedam dana za baze podataka u sloju osnovna usluga – potražite u članku [servis razine](sql-database-service-tiers.md) detalje na servis razine. Ako je razdoblje zadržavanja za servisnog sloja sustava ne zadovoljava preduvjete svoje tvrtke, možete povećati razdoblje zadržavanja promjenom [sloju servisa](sql-database-scale-up.md). Punog i razlikovnog baze podataka sigurnosne kopije i replicirati [upareni podatkovnog centra](../best-practices-availability-paired-regions.md) za zaštitu od nedostupnosti centar podataka – potražite u članku [sigurnosne kopije baze podataka za automatsko](sql-database-automated-backups.md) više pojedinosti.

Ove automatsko sigurnosne kopije baze podataka možete koristiti da biste oporavili baze podataka iz različitih disruptive događaje, u podatkovnom centru i drugi podatkovnog centra. Pomoću sigurnosne kopije baze podataka za automatsko procijenjeno vrijeme oporavak ovisi o nekoliko čimbenika uključujući ukupan broj baza podataka za oporavak u istom području u isto vrijeme, veličina baze podataka, zapisnik transakcija i propusnost mreže. U većini slučajeva, vrijeme oporavak je manji od 12 sati. Kada oporavak područja s podacima, mogućem gubitku podataka ograničeno je na 1 sat po zemlj suvišnih prostora za pohranu svaki sat sigurnosne kopije razlikovno baze podataka. 

> [AZURE.IMPORTANT] Da biste oporavili pomoću automatskog sigurnosnog kopiranja, morate biti član ulogu suradnika SQL Server ili vlasnik pretplate - potražite u članku [RBAC: ugrađene uloge](../active-directory/role-based-access-built-in-roles.md). Možete vratiti pomoću portala za Azure, PowerShell ili REST API-JA. Ne možete koristiti Transact-SQL.

Korištenje automatskog sigurnosnog kopiranja kao tvrtke continuity i oporavak mehanizam ako aplikacija:

- Ne smatra zaštita njihove privatnosti ovise od ključne važnosti.
- Nema povezivanja SLA stoga nedostupnost ili dulje od 24 sata neće rezultirati financijske odgovornosti.
- Ima malo rata promjena podataka (niska transakcije po satu), a do gubitka jedan sat promjena do gubitka prihvatljiva podataka. 
- Trošak je i mala slova. 

Ako vam je potrebna brže oporavak, koristite [Active replikacije zemlj.](sql-database-geo-replication-overview.md) (spominju Dalje). Ako trebate omogućiti oporavak podataka iz polja razdoblje starije od 35 dana, razmislite o arhiviranje bazu podataka redovito BACPAC datoteke (komprimiranu datoteku koja sadrži shemu baze podataka i pridruženih podataka) pohranjene u spremište blobova platforme Azure ili na neko drugo mjesto po izboru. Dodatne informacije o stvaranju putem transakcije dosljedan bazu podataka arhivsku potražite u članku [Stvaranje kopiju baze podataka](sql-database-copy.md) i [Izvoz kopiju baze podataka](sql-database-export.md). 

### <a name="use-active-geo-replication-to-reduce-recovery-time-and-limit-data-loss-associated-with-a-recovery"></a>Smanjivanje oporavak vremena i ograničiti gubitak podataka povezan s na oporavak pomoću aktivni replikacijom zemlj.

Osim u slučaju tvrtke prekidu pomoću sigurnosne kopije baze podataka za oporavak baze podataka, koristite [Active replikacije zemlj](sql-database-geo-replication-overview.md) za konfiguriranje baze podataka da bi do četiri čitljiv sekundarne baze podataka u područja po izboru. Ovim sekundarni bazama podataka su sinkronizirani s primarnom bazom podataka pomoću mehanizam za asinkronog replikacijom. Značajka se koristi da biste zaštitili tvrtke prekidu slučaju nedostupnosti za centar za podatke ili tijekom nadogradnje aplikacije. Aktivni replikacije zemlj se može koristiti i omogućuju bolje performanse upita za upite koji su samo za čitanje geografski raspršenim korisnicima.

Ako primarnom bazom podataka dolazi izvanmrežne neočekivano ili ćete morati poduzeti je izvan mreže za održavanje aktivnosti, možete brzo povećanje sekundarni postaju primarni (naziva se i u prebacivanje) i konfiguriranje aplikacija za povezivanje s upravo čija je razina povećana primarni. S planiranog prebacivanje, nema gubitka podataka. S neplanirano prebacivanje, možda postoje neka mali gubitka podataka za vrlo nedavne transakcije zbog prirode asinkronog replikacije. Nakon prebacivanje, možete ga kasnije failback – prema plana ili kada se vratite u mrežni podatkovnog centra. U svakom slučaju, korisnici imate mali nedostupnost i morati ponovno povezali. 

> [AZURE.IMPORTANT] Da biste koristili aktivni replikacije zemlj., morate biti vlasnik pretplatu ili imate administratorske dozvole u sustavu SQL Server. Možete konfigurirati i prebacivanje pomoću portala za Azure, PowerShell ili REST API-JA pomoću dozvole na pretplatu ili pomoću Transact-SQL koriste dozvole u sustavu SQL Server.

Koristite Active replikacije zemlj ako aplikacija zadovoljavaju neki od ovih kriterija:

- Zaštita njihove privatnosti ovise je važnosti.
- Ima na razini ugovor o usluzi (SLA) koje omogućuju 24 sata ili više isključiti.
- Nedostupnost rezultirat će financijske odgovornosti.
- Sadrži velike podataka promjena visoke i gubitka jedan sat podataka nije prihvatljiva.
- Dodatni trošak aktivni replikacije zemlj je manja od potencijalne financijske odgovornosti i povezani poslovni gubitak.

## <a name="recover-a-database-after-a-user-or-application-error"></a>Oporavak baze podataka nakon korisnik ili aplikacija pogreške

* Nitko je savršen! Korisnik može slučajno izbrišete neke podatke, slučajno padajući važne tablice ili čak i ispustite cijelu bazu podataka. Ili aplikacije možda slučajno Prebriši dobar podatke neispravni zbog do oštećenja aplikacije. 

U ovom slučaju to su mogućnosti za oporavak.

### <a name="perform-a-point-in-time-restore"></a>Vraćanje točke u vrijeme

Automatskog sigurnosnog kopiranja možete koristiti da biste oporavili kopiju baze podataka poznate dobar točku u vremenu, pod uvjetom da je vrijeme unutar razdoblje zadržavanja bazu podataka. Kada se Vrati bazu podataka, možete izvorna baza podataka zamijenite vraćenu bazu podataka ili kopirajte potrebnih podataka iz vraćene podatke u izvornoj bazi podataka. Ako bazu podataka koristi Active replikacije zemlj., preporučujemo da kopirate potrebnih podataka iz vraćene kopije u izvornoj bazi podataka. Ako se izvorna baza podataka zamijeniti vraćenu bazu podataka, morat ćete ponovno konfigurira i ponovno pokrenite sinkroniziranje aktivni zemlj. – replikacije (što može potrajati vrlo velike baze podataka). 

Dodatne informacije i detaljne upute za vraćanje baze podataka na točku u vremenu pomoću portala za Azure ili pomoću komponente PowerShell potražite u članku [Vraćanje točke u vrijeme](sql-database-recovery-using-backups.md#point-in-time-restore). Nije moguće vratiti pomoću Transact-SQL.

### <a name="restore-a-deleted-database"></a>Vraćanje izbrisane baze podataka

Ako se briše bazu podataka, ali logičke poslužitelj je izbrisan, izbrisanu bazu podataka možete vratiti na mjesto na kojem je izbrisana. To vraća sigurnosnu kopiju baze podataka na istom logičke SQL server iz kojeg je izbrisana. Možete vratiti s izvornim nazivom ili novi naziv ili vraćenu bazu podataka.

Dodatne informacije i detaljne upute za vraćanje izbrisanih baze podataka pomoću portala za Azure ili pomoću komponente PowerShell potražite u članku [Obnavljanje izbrisanih baze podataka](sql-database-recovery-using-backups.md#deleted-database-restore). Nije moguće vratiti pomoću Transact-SQL.

> [AZURE.IMPORTANT] Ako je izbrisana logičke poslužitelj, ne možete oporaviti izbrisane baze podataka. 

### <a name="import-from-a-database-archive"></a>Uvoz iz baze podataka arhive

Ako je došlo je do gubitka podataka izvan trenutno razdoblje zadržavanja za automatskog sigurnosnog kopiranja i ste je arhiviranje bazu podataka, možete [Uvoz arhivirane BACPAC datoteke](sql-database-import.md) s novom bazom podataka. Sada možete zamijeniti izvornu bazu podataka uvezenih bazu podataka ili kopirajte potrebnih podataka iz uvezene podatke u izvornoj bazi podataka. 

## <a name="recover-a-database-to-another-region-from-an-azure-regional-data-center-outage"></a>Oporavak baze podataka u drugoj regiji iz programa nedostupnosti centar za Azure regionalni podaci

<!-- Explain this scenario -->

Iako se rijetko, centar za Azure podataka može sadržavati do prekida. Kada se pojavi se prekida, uzrokuje prekidu tvrtke koje samo prezimena nekoliko minuta ili može trajati sati. 

- Jedna od mogućnosti jest čekanja za bazu podataka dolazi vratite u mrežni po centru nedostupnosti podataka. Taj je postupak za aplikacije koje možete smijete da bi izvanmrežne baze podataka. Ako, na primjer, razvoj projekt ili besplatnu probnu verziju ne morate raditi na neprestano. Kada u podatkovnom centru ima do prekida, neće znati koliko će se prekida zadnji, pa tu mogućnost funkcionira samo ako ne trebate bazu podataka za neko vrijeme.
- Druga mogućnost je ili prebacivanje na drugi područja podataka ako koristite Active replikacije zemlj ili oporavak pomoću zemlj suvišnih sigurnosne kopije baze podataka (Vraćanje zemlj.). Prebacivanje traje samo nekoliko sekundi dok oporavak iz sigurnosnih kopija vodi sati.

Kada poduzmite akciju, koliko dugo traje oporaviti, a koliko gubitka podataka plaćati slučaju nedostupnosti za centar za podataka ovisi o tome kako odlučite koristiti značajku continuity tvrtke gore spomenute u aplikaciji. Uistinu, možete kombinirati sigurnosne kopije baze podataka i aktivni zemlj. – replikacije ovisno o preduvjeti za aplikaciju. Rasprave zahtjevi dizajna aplikacije za samostalno baze podataka i elastic grupe pomoću tih značajki continuity tvrtke, potražite u članku [Dizajn aplikaciju za oporavak Izrada oblaka](sql-database-designing-cloud-solutions-for-disaster-recovery.md) i [Strategije oporavka Izrada Elastic skup](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).

U nastavku odjeljcima pregled koraci za oporavak sigurnosne kopije baze podataka ili aktivni replikacije zemlj.. Detaljne korake uključujući planiranje preduvjeta, korake za oporavak objavu i o tome kako se prekida kao zamjenu za izvođenje analize za oporavak Izrada potražite u članku [oporavak SQL baze podataka iz programa prekida](sql-database-disaster-recovery.md).

### <a name="prepare-for-an-outage"></a>Priprema za sinkronizaciju programa prekida

Bez obzira na to tvrtke continuity značajku koristite, morate:

- Prepoznajte i Priprema poslužitelju ciljni, uključujući pravila vatrozida razini poslužitelja, prijave i razine dozvola za glavnom bazom podataka.
- Određivanje preusmjeravanje klijenata i klijentske aplikacije da biste novi poslužitelj
- Dokument druge ovisnosti, kao što su nadzor postavke i upozorenja 
 
Ako ne planiranje i Priprema ispravno, prebacivanja aplikacija online kada je prebacivanje ili na oporavak traje dodatne i vjerojatno potreban je i otklanjanje poteškoća trenutku opterećenjem - kombinaciju neispravni.

### <a name="failover-to-a-geo-replicated-secondary-database"></a>Prebacivanje s zemlj replicirati sekundarne bazom podataka 

Ako koristite Active replikacije zemlj kao oporavak mehanizam, [prisilno prebacivanje na sekundarni zemlj replicirati](sql-database-disaster-recovery.md#failover-to-geo-replicated-secondary-database). Sekundi, sekundarne ulogu postaju novi primarnih i bude spremna za snimanje nove transakcije i odgovorili na eventualne upite - sa samo nekoliko sekundi gubitka podataka za podatke koji su imali nije još je replicirati. Informacije o Automatizacija procesa prebacivanje, potražite u članku [dizajna aplikaciju za oporavak Izrada oblaka](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

> [AZURE.NOTE] Kada se vratite u mrežni podatkovnog centra, možete failback na izvornom primarni (ili ne).

### <a name="perform-a-geo-restore"></a>Vraćanje zemlj. 

Ako koristite automatizirano sigurnosno kopiranje s replikacijom zemlj suvišnih prostora za pohranu kao oporavak mehanizam, [pokretanje baze podataka oporavak pomoću zemlj vraćanja](sql-database-disaster-recovery.md#recover-using-geo-restore). Oporavak obično odvija u roku od 12 sati - s gubitka podataka do jedan sat u određeno vrijeme zadnje svaki sat razlikovno sigurnosno kopiranje s snimanja i repliciranu. Dok se ne dovrši oporavka, baza podataka nije uspio snimiti sve transakcije ili odgovorili na eventualne upite. 

> [AZURE.NOTE] Ako podatkovnog centra se vratite u mrežni način prije nego što počnete aplikaciju oporavljene bazu podataka, jednostavno možete otkazati oporavka.  

### <a name="perform-post-failover--recovery-tasks"></a>Izvođenje poštanski prebacivanje / oporavak zadataka 

Nakon oporavka iz ili oporavak mehanizam prije nego što vaši korisnici morate izvršiti sljedeće dodatne zadatke i aplikacije ponovno se s radom:

- Preusmjeravanje klijenata i klijentske aplikacije u novi poslužitelj i vraćenu bazu podataka
- Provjera pravila vatrozida odgovarajuće razini poslužitelja na mjestu za korisnike koji žele povezati (ili koristite [bazu podataka razinom vatrozidima](sql-database-firewall-configure.md#creating-database-level-firewall-rules))
- Provjerite je li odgovarajuća prijave i razine dozvola za glavnom bazom podataka će se prikazati (ili koristite [nalaze korisnika](https://msdn.microsoft.com/library/ff929188.aspx))
- Konfiguriranje nadzora, sukladno situaciji
- Konfiguriranje upozorenja, po potrebi

## <a name="upgrade-an-application-with-minimal-downtime"></a>Nadogradnja aplikacije s minimalnim nedostupnost

Ponekad aplikacija treba poduzeti izvanmrežne zbog planiranog održavanja kao što su nadogradnju aplikacije. [Upravljanje aplikacije nadogradnje](sql-database-manage-application-rolling-upgrade.md) opisuje način korištenja aktivni replikacije zemlj da biste omogućili vodoravnim nadogradnje oblaka aplikacije da biste minimizirali prekid u radu tijekom nadogradnje i navedite put za oporavak u događaja nešto pošlo po redu. U ovom se članku razmatra dvije različite metode orchestrating postupka nadogradnje te iznose prednosti i gubitke svake mogućnosti.

## <a name="next-steps"></a>Daljnji koraci

Rasprave zahtjevi dizajna aplikacije za samostalno baza podataka i elastic grupe potražite u članku [dizajna aplikaciju za oporavak Izrada oblaka](sql-database-designing-cloud-solutions-for-disaster-recovery.md) i [Strategije oporavka Izrada Elastic skup](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).






