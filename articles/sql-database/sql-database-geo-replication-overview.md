<properties
    pageTitle="Aktivni zemlj. – replikacije baze podataka Azure SQL"
    description="Active replikacijom zemlj omogućuje postavljanje 4 replike baze podataka u bilo kojem od Azure podatkovnim centrima."
    services="sql-database"
    documentationCenter="na"
    authors="stevestein"
    manager="jhubbard"
    editor="monicar" />


<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="NA"
    ms.date="09/26/2016"
    ms.author="sstein" />

# <a name="overview-sql-database-active-geo-replication"></a>Pregled: SQL baza podataka aktivna zemlj. – replikacije

Aktivni replikacije zemlj omogućuje vam da biste konfigurirali do četiri čitljiv sekundarne baze podataka na istom ili drugom podataka centra mjesta (regije). Sekundarni baza podataka dostupnih za slanje upita i prebacivanje u slučaju nedostupnosti za centar za podatke ili Nemogućnost povezivanja s primarnom bazom podataka.

>[AZURE.NOTE] Sada je dostupan za sve baze podataka u sve razine servisa Active zemlj. – replikacije (čitljivi secondaries). U 2017 Travanj koje nisu čitljiv sekundarne vrsta će biti povučena iz upotrebe, a zatim postojeće baze podataka koje nisu čitljiv automatski nadogradit će se čitati secondaries.

 Možete konfigurirati aktivni zemlj. – replikacije pomoću [portala za Azure](sql-database-geo-replication-portal.md), [PowerShell](sql-database-geo-replication-powershell.md), [Transact-SQL](sql-database-geo-replication-transact-sql.md)ili [REST API-JA – stvaranje ili ažuriranje baze podataka](https://msdn.microsoft.com/library/azure/mt163685.aspx).

> [AZURE.SELECTOR]
- [Konfiguriranje: Portal za Azure](sql-database-geo-replication-portal.md)
- [Konfiguriranje: PowerShell](sql-database-geo-replication-powershell.md)
- [Konfiguriranje: T-SQL](sql-database-geo-replication-transact-sql.md)

Ako za bilo koje razloga vaš primarni baze podataka ne uspije ili jednostavno mora biti izvanmrežno, možete *Prebacivanje* u neku od svojih sekundarne baza podataka. Kada je prebacivanje aktiviran na neki od sekundarne baze podataka, drugi secondaries automatski će se povezuje s novi primarni.

Možete prebacivanje na sekundarni pomoću [portala za Azure](sql-database-geo-replication-failover-portal.md), [PowerShell](sql-database-geo-replication-failover-powershell.md), [Transact-SQL](sql-database-geo-replication-failover-transact-sql.md), [REST API - planirano prebacivanje](https://msdn.microsoft.com/ibrary/azure/mt575007.aspx)ili [REST API - neplanirano prebacivanje](https://msdn.microsoft.com/library/azure/mt582027.aspx).


> [AZURE.SELECTOR]
- [Prebacivanje: Portal za Azure](sql-database-geo-replication-failover-portal.md)
- [Prebacivanje: PowerShell](sql-database-geo-replication-failover-powershell.md)
- [Prebacivanje: T-SQL](sql-database-geo-replication-failover-transact-sql.md)

Nakon prebacivanje, provjerite je li preduvjeti za poslužitelj i bazu podataka za provjeru autentičnosti konfigurirane na novu primarni. Detalje potražite u članku [Sigurnost SQL baze podataka nakon oporavka Izrada](sql-database-geo-replication-security-config.md).


Značajka aktivni replikacije zemlj implementira mehanizam osigurati redundanciju baze podataka unutar iste područja Microsoft Azure ili u različitim područjima (zalihosti zemlj.). Aktivni replikacije zemlj asinkrono replicira izvršenja transakcije iz baze podataka na najviše četiri kopije baze podataka na različitim poslužiteljima pomoću čitati izvršenja snimke odvajanja (RCSI) za odvajanja. Kada je aktivna replikacije zemlj konfiguriran, sekundarne se baza podataka na navedenom poslužitelju. Izvorna baza podataka postaje primarnom bazom podataka. Primarni baze podataka asinkrono replicira izvršenja transakcije svakom sekundarne baze podataka. Potpuna transakcije su replicirati. Na bilo koji navedeni točka sekundarne baze podataka može se malo iza primarnom bazom podataka, sekundarne podatkovne uvijek je nikad ne moraju djelomična transakcije. Konkretne podatke RPO možete pronaći na [Pregled od poslovanje](sql-database-business-continuity.md).

Prednosti primarni od aktivnog zemlj. – replikacije je s vremenom niskog oporavak pruža rješenja za oporavak Izrada razinom baze podataka. Kada položite sekundarni baze podataka na poslužitelju u nekoj drugoj regiji, dodajte maksimalni resilience u aplikaciji. Zalihosti regije-omogućuje aplikacije za oporavak trajni gubitak cijelu podatkovnog centra ili dijelove Standard uzrokovanih prirodnim disasters, do teškog oštećenja Ljudski pogreške ili zlonamjernih djela. Na sljedećoj je slici prikazan primjer od aktivnog zemlj. – replikacije konfiguriran za korištenje u regiji Sjeverna središnje NAM i sekundarnih primarni u regiji Jug središnje NAM.

![Odnos zemlj replikacije](./media/sql-database-active-geo-replication/geo-replication-relationship.png)

Druga pogodnost ključne je da se baza podataka za sekundarne nalaze čitljiv i može se koristiti za offload radnih opterećenja samo za čitanje kao što su izvješća zadataka. Ako namjeravate samo korištenje sekundarnog baze podataka za ujednačavanje opterećenja, možete je stvoriti u području isti kao primarni. Stvaranje sekundarni u istom području, povećajte resilience aplikacije za katastrofalnih.  

Drugim situacijama gdje se mogu koristiti Active zemlj replikacijom obuhvaćaju sljedeće:

- **Migracija baze podataka**: koristite Active replikacijom zemlj za Migriraj bazu podataka s jednog poslužitelja na drugo u mreži s minimalne isključiti.
- **Nadograđuje aplikacije**: Stvaranje dodatnog sekundarni kao Neuspjelo natrag kopiju tijekom nadogradnje aplikacije.

Da biste postigli realni poslovanje, dodavanje zalihosti baze podataka između podatkovnim centrima je samo dio rješenja. Oporavak programa aplikacije (servis) završetka do kraja nakon Katastrofalna pogreška zahtijeva oporavak sve komponente koje čine usluge i o njima ovisne servisi. Primjeri te komponente uključuju klijentski softver (na primjer, preglednik s prilagođeni JavaScript), web-sučelja, za pohranu i DNS-a. Ključne je važnosti jesu li sve komponente prebacuju na istom pogrešaka i postaju dostupne unutar cilj oporavak vrijeme (RTO) aplikacije. Stoga ćete morati prepoznavanje sve ovisne servise i razumijevanje mogućnosti pružaju i jamstva. Pa morate poduzeti odgovarajuće korake za Pobrinite se da vaš servis funkcije tijekom prebacivanje servise koji ovisi. Dodatne informacije o dizajniranje rješenja za oporavak Izrada potražite u članku [Dizajniranje rješenja oblaka za Izrada oporavak pomoću aktivni zemlj. – replikacije](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

## <a name="active-geo-replication-capabilities"></a>Aktivni mogućnostima replikacije za zemlj.
Značajka aktivni replikacije zemlj sadrži sljedeće ključne značajke:

- **Automatsko asinkronog replikacije**: sekundarne baze podataka možete stvoriti samo dodavanjem postojećoj bazi podataka. Sekundarne moguće je stvoriti samo u drugi poslužitelj baze podataka SQL Azure. Nakon stvaranja, sekundarne baze podataka se popunjava podatke kopirane s primarnom bazom podataka. Ovaj postupak zove seeding. Nakon sekundarne baza podataka stvorena je i seeded, ažuriranja s primarnom bazom podataka su asinkrono replicirati na sekundarnom baze podataka automatski. Asinkrona replikacije znači da je transakcija poboljšanju na primarnom bazom podataka prije nego što se oni su replicirati na sekundarnom baze podataka. 

- **Više sekundarne baza podataka**: dva ili više sekundarne baza podataka za povećavanje zalihosti i razinu zaštite primarnom bazom podataka i aplikacije. Ako postoji više sekundarne baza podataka, će ostati aplikacija zaštićenu čak i ako jedan sekundarni baza podataka ne uspije. Ako postoji samo jednu sekundarne bazu podataka, a ne uspijeva, aplikacija je izložen riziku dok je stvorena nova sekundarne baza podataka.

- **Čitljive sekundarne baza podataka**: aplikacije mogu pristupiti sekundarne baze podataka za samo za čitanje operacije pomoću istom ili drugom sigurnosni upravitelji koji se koristi za pristup primarnom bazom podataka. Baza podataka za sekundarne raditi u načinu odvajanja snimku stanja da biste bili sigurni replikacije ažuriranja primarni (ponovno reproducira zapisnika) ne kasni upiti koji se izvršava na sekundarnom.

>[AZURE.NOTE] Ponovi zapisnika kasni se na sekundarnom baze podataka ako postoje sheme ažuriranja koja primate od primarni koje je potrebno sheme lock sekundarne baze podataka.

- **Aktivni zemlj. – replikacije elastic skup baza podataka**: aktivna replikacije zemlj moguće je konfigurirati za sve baze podataka u bilo kojem elastic baze podataka. Sekundarni baze podataka može biti u drugom elastic baze podataka. Baza podataka u pravilnim, sekundarne može biti do elastic baze podataka skup i Potpredsjednik obrnuto pod uvjetom da su iste razine servisa. 

- **Prilagodljivo performanse razinu sekundarne baze podataka**: sekundarne baze podataka mogu se kreirati pomoću niže razine performanse od primarnih. Baza podataka primarnih i sekundarnih moraju imati isti sloju servisa. Tu mogućnost ne preporučuje za aplikacije s aktivnosti pisanje visoka baze podataka jer kašnjenja povećana replikacijom povećava rizik od gubitka podataka znatno nakon na prebacivanje. Osim toga, nakon prebacivanje performanse aplikacije utječe dok se novi primarni je nadograditi na višu razinu performansi. Grafikon postotka IO zapisnika portala za Azure omogućuje dobar način za procjenu razinom minimalnog performansi sekundarni potrebnog za nastavka rješenja za učitavanje replikacije. Na primjer, ako je vaš primarni baza podataka P6 (1000 DTU), a IO postotak zapisnika 50% sekundarne mora biti najmanje P4 (500 DTU). Također možete dohvatiti podatke IO pomoću [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) ili [sys.dm_db_resource_stats]( https://msdn.microsoft.com/library/dn800981.aspx) prikaza baze podataka.  Dodatne informacije o performansama razine SQL baze podataka potražite u članku [Mogućnosti SQL baze podataka i performanse](sql-database-service-tiers.md). 

- **Korisnički kontrolirano prebacivanje i failback**: sekundarne baze podataka možete izričito prijeći temeljna funkcija u bilo kojem trenutku aplikacije ni korisnik. Tijekom realni nedostupnosti mogućnost "neplanirano" moraju se, koji odmah promiče sekundarni biti primarni. Kada nije uspjelo primarni oporavlja i ponovno je dostupna, sustav automatski označava oporavljene primarni kao sekundarni i Premjesti je ažurirao novi primarni. Zbog asinkronog prirode replikacijom malu količinu podataka može biti izgubljene tijekom neplanirano failovers ne uspijete primarni prije nego što je replicira najnovije promjene na sekundarnom. Ako primarni s više secondaries ne uspije iznad, sustav automatski reconfigures replikacije odnose i povezuje preostale secondaries upravo čija je razina povećana primarni bez intervencije korisnika. Nakon prekida koje su uzrokovale na prebacivanje je mitigated, možda je poželjno da biste se vratili aplikacije primarni područje. Da biste to učinili, naredbe za prebacivanje treba pozvati s mogućnošću "planiranog". 

- **Zadržavanje vjerodajnice i pravila vatrozida u sinkronizaciji**: preporučujemo korištenje [pravila vatrozida baze podataka](sql-database-firewall-configure.md) za zemlj replicirati baza podataka tako da ta pravila moguće je replicirati s bazom podataka da biste bili sigurni da svi sekundarne baze podataka imaju ista pravila vatrozida kao primarni. Taj se način nema potrebe za klijente za ručno konfiguriranje i održavanje pravila vatrozida na poslužiteljima koji se nalaze i primarnih i sekundarnih baze podataka. Isto tako, pomoću [koje se nalaze korisnici baze podataka](sql-database-manage-logins.md) za pristup podacima osigurava primarnih i sekundarnih baze podataka uvijek imaju isti korisnik vjerodajnice učinili tijekom na prebacivanje, postoji nema to izbjeglo zbog nepodudarnosti s prijave i lozinke. Pomoću Zbrajanje [Azure Active Directory](../active-directory/active-directory-whatis.md), korisnici možete upravljati korisničkog pristupa primarnih i sekundarnih baze podataka i bez potrebe za upravljanje vjerodajnice u sasvim baze podataka.

## <a name="upgrading-or-downgrading-a-primary-database"></a>Nadogradnja ili prijelaz na stariju primarnom bazom podataka
Možete nadograditi ili prijeći na nižu primarnom bazom podataka različite performanse razinu (unutar iste servisa sloju) bez prekida veze nijedna sekundarne baza podataka. Prilikom nadogradnje, preporučujemo da prvo nadogradnju sekundarne baze podataka i nadogradnja primarni. Kada prijelaz na stariju, obrnutim redoslijedom: prvo prijeći na nižu primarnih i prijeći na nižu sekundarne. 

Sekundarni baze podataka mora biti u istu sloju servisa primarnim, pa Migracija primarnom bazom podataka na drugi servis sloju, morate prekinuti vezu zemlj replikacije i preimenovati sekundarne baze podataka ili jednostavno je ispustite. Zatim migrirati primarni novog servisnog sloja sustava i konfigurirajte zemlj replikacije. Vaše nove sekundarni će se stvaraju automatski s istu razinu performanse kao primarni prema zadanim postavkama.

## <a name="preventing-the-loss-of-critical-data"></a>Sprječavanje gubitka podataka od ključne važnosti
Zbog visokom latencijom mreža širokog područja neprekinuti Kopiraj koristi mehanizam za asinkronog replikacije. Asinkrona replikacije čini gubitka podataka unavoidable ako dođe do pogreške. Međutim, neke aplikacije možda ćete morati bez gubitka podataka. Da biste zaštitili te kritičnih ažuriranja, na razvojni inženjer možete nazvati sustava postupak [sp_wait_for_database_copy_sync](https://msdn.microsoft.com/library/dn467644.aspx) odmah nakon potvrđivanja transakcije. Pozivanje **sp_wait_for_database_copy_sync** blokova pozivanja niti do posljednje izvršenja transakcije sadrži je replicirati na sekundarnom baze podataka. Postupak će pričekati da sve u redu čekanja transakcije ste su označeni sekundarne baze podataka. **sp_wait_for_database_copy_sync** implementaciju ograničen je na određene neprekinuti Kopiraj vezu. Bilo koji korisnik s pravima veze s primarnom bazom podataka da biste uputili poziv ovaj postupak.

>[AZURE.NOTE] Kašnjenja uzrokovana poziva proceduru **sp_wait_for_database_copy_sync** može biti vrlo. Odgoda ovisi o veličini duljine zapisnik transakcija koristilo i ovog poziva vratili dok je replicirati cijelu zapisnika. Izbjegavajte pozivanje postupak osim ako uistinu potrebne.

## <a name="programmatically-managing-active-geo-replication"></a>Programsko upravljanje aktivni replikacije zemlj.

Kako je opisano prethodno aktivni replikacije zemlj moguće je upravljati programski pomoću komponente PowerShell Azure i REST API-JA. Tablice u nastavku opisuju postavljanje dostupne naredbe.

- **API resursima Azure i sigurnost na temelju uloga**: aktivna replikacije zemlj sadrži skup [Azure resursima API -ji]( https://msdn.microsoft.com/library/azure/mt163571.aspx) za upravljanje, uključujući [utemeljen na Voditelj resursa Azure PowerShell cmdleti](sql-database-geo-replication-powershell.md). Ove API-ji zahtijevaju korištenje grupa resursa i podrška sigurnosti na temelju uloga (RBAC). Dodatne informacije o tome kako implementirati pristup uloga potražite u članku [Azure Role-Based kontrola pristupa](../active-directory/role-based-access-control-configure.md).

>[AZURE.NOTE] Mnoge nove značajke sustava Active zemlj. – replikacijom podržani su samo korištenju [Upravitelja Azure resursa](../azure-resource-manager/resource-group-overview.md) na temelju [Azure SQL REST API -JA](https://msdn.microsoft.com/library/azure/mt163571.aspx) i [cmdleta ljuske PowerShell za baze podataka SQL Azure](https://msdn.microsoft.com/library/azure/mt574084.aspx). (Klasične) REST API-JA] (https://msdn.microsoft.com/library/azure/dn505719.aspx) i [baze podataka SQL Azure (klasični) cmdleta](https://msdn.microsoft.com/library/azure/dn546723.aspx) podržani za kompatibilnost s prijašnjim verzijama tako da pomoću API-ji Voditelj resursa Azure temelji se preporučuje. 


### <a name="transact-sql"></a>SQL transakcija

|Naredba|Opis|
|-------|-----------|
|[ZAMIJENI bazu podataka (baza podataka Azure SQL)](https://msdn.microsoft.com/library/mt574871.aspx)|Korištenje argumenta dodavanje SEKUNDARNI POSLUŽITELJ Uključeno da biste stvorili sekundarni baze podataka za postojeću bazu podataka i pokreće podataka replikacije|
|[ZAMIJENI bazu podataka (baza podataka Azure SQL)](https://msdn.microsoft.com/library/mt574871.aspx)|Korištenje PREBACIVANJE ili FORCE_FAILOVER_ALLOW_DATA_LOSS da biste se prebacili sekundarne baza podataka biti primarni da biste započeli prebacivanje
|[ZAMIJENI bazu podataka (baza podataka Azure SQL)](https://msdn.microsoft.com/library/mt574871.aspx)|Koristite UKLONI SEKUNDARNI POSLUŽITELJ Uključeno da biste prekinuli podataka replikacijom između SQL baze podataka i navedena sekundarni baza podataka.|
|[sys.geo_replication_links (bazom podataka SQL Azure)](https://msdn.microsoft.com/library/mt575501.aspx)|Vraća informacije o sve postojeće veze replikacije za svaku bazu podataka na poslužitelju baze podataka SQL Azure logičke.|
|[sys.dm_geo_replication_link_status (bazom podataka SQL Azure)](https://msdn.microsoft.com/library/mt575504.aspx)|Dohvaća zadnji put replikacije, posljednje replikacije kašnjenja i druge podatke o vezi replikacije za dani SQL baze podataka.|
|[sys.dm_operation_status (bazom podataka SQL Azure)](https://msdn.microsoft.com/library/dn270022.aspx)|Prikazuje se status za sve baze podataka operacije uključujući stanje veze replikacije.|
|[sp_wait_for_database_copy_sync (bazom podataka SQL Azure)](https://msdn.microsoft.com/library/dn467644.aspx)|pokreće program Pričekajte da se sve izvršenja transakcije replicirati i označeni Aktivni sekundarni baze podataka.|
||||

### <a name="powershell"></a>PowerShell

|Cmdlet|Opis|
|------|-----------|
|[Get-AzureRmSqlDatabase](https://msdn.microsoft.com/en-us/library/azure/mt603648.aspx)|Dohvaća jedan ili više baza podataka.|
|[Novi AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/library/mt603689.aspx)|Stvara sekundarni bazu podataka za postojeću bazu podataka i pokrenuti replikacijom podataka.|
|[Postavljanje AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/en-us/library/mt619393.aspx)|Prelazi sekundarni baza podataka biti primarni da biste započeli prebacivanje.|
|[Uklanjanje AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/en-us/library/mt603457.aspx)|Prekida replikacije podataka između SQL baze podataka i navedena sekundarne baza podataka.|
|[Get-AzureRmSqlDatabaseReplicationLink](https://msdn.microsoft.com/library/mt619330.aspx)|Dohvaća zemlj replikacije veze između baze podataka SQL Azure i grupu resursa ili SQL Server.|
||||

### <a name="rest-api"></a>REST API-JA

|API-JA|Opis|
|---|-----------|
|[Stvaranje ili ažuriranje baze podataka (createMode vraćanja =)](https://msdn.microsoft.com/library/azure/mt163685.aspx)|Stvara, ažuriranja i vraća primarni ili sekundarni baze podataka.|
|[Početak stvaranje ili ažuriranje stanje baze podataka](https://msdn.microsoft.com/library/azure/mt643934.aspx)|Vraća status tijekom operacije Stvori.|
|[Postavljanje sekundarne bazu podataka kao primarni (planiranog prebacivanje)](https://msdn.microsoft.com/ibrary/azure/mt575007.aspx)|Promicanje sekundarne baze podataka u zemlj replikacije partnerstvo postati nove primarni baze podataka.|
|[Postavljanje sekundarni bazu podataka kao primarni (neplanirano prebacivanje u slučaju pogreške)](https://msdn.microsoft.com/library/azure/mt582027.aspx)|Da biste prisilno je prebacivanje na sekundarnom bazu podataka i postavljanje sekundarne kao primarni.|
|[Dohvati veze na replikacije](https://msdn.microsoft.com/library/azure/mt600929.aspx)|Dohvaća sve veze za replikaciju za dani SQL baze podataka u zemlj replikacije partnerstvo. Dohvaća informacije koje su vidljive u prikazu sys.geo_replication_links kataloga.|
|[Dohvaćanje veze za replikaciju](https://msdn.microsoft.com/library/azure/mt600778.aspx)|Dohvaća određene replikacije vezu za dani SQL baze podataka u zemlj replikacije partnerstvo. Dohvaća informacije koje su vidljive u prikazu sys.geo_replication_links kataloga.|
||||



## <a name="next-steps"></a>Daljnji koraci

- Pregled za continuity tvrtke i scenariji potražite u članku [Pregled continuity tvrtke](sql-database-business-continuity.md)
- Da biste saznali više o sigurnosne kopije baze podataka SQL Azure automatski, potražite u članku [baze podataka SQL automatizirano sigurnosno kopiranje](sql-database-automated-backups.md).
- Dodatne informacije o korištenju automatizirano sigurnosno kopiranje za oporavak, potražite u članku [Vraćanje baze podataka iz sigurnosne kopije pokrenut servis](sql-database-recovery-using-backups.md).
- Dodatne informacije o korištenju automatizirano sigurnosno kopiranje za arhiviranje, potražite u članku [kopiranje baze podataka](sql-database-copy.md).
- Da biste saznali više o preduvjeti za provjeru autentičnosti za novi primarni poslužitelj i bazu podataka, u odjeljku [Zaštita baze podataka SQL nakon oporavka Izrada](sql-database-geo-replication-security-config.md).
