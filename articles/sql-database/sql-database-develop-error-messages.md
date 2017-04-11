<properties
    pageTitle="Kodovi pogrešaka SQL - pogreška vezu baze podataka | Microsoft Azure"
    description="Saznajte više o šifre pogreške SQL baze podataka SQL klijentske aplikacije, kao što su uobičajenih pogrešaka u bazi podataka povezivanja, poteškoća kopiju baze podataka i općenite pogreške. "
    keywords="kod pogreške SQL, access sql, pogreška vezu baze podataka, sql šifre pogrešaka"
    services="sql-database"
    documentationCenter=""
    authors="annemill"
    manager="jhubbard"
    editor="" />


<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="annemill"/>


# <a name="sql-error-codes-for-sql-database-client-applications-database-connection-error-and-other-issues"></a>Kodovi pogrešaka SQL za baze podataka SQL klijentske aplikacije: pogreška veze i ostalih problema za baze podataka


<!--
Old Title on MSDN:  Error Messages (Azure SQL Database)
ShortId on MSDN:  ff394106.aspx
Dx 4cff491e-9359-4454-bd7c-fb72c4c452ca
-->


U ovom se članku navode šifre pogreške SQL za baze podataka SQL klijentskim aplikacijama, uključujući pogreške pri povezivanju baze podataka, tranzitne pogrešaka (nazivaju se i tranzitne kvarove), pogreške upravljanja resursa, problemi kopiju baze podataka, elastic skup i druge pogreške. Većina kategorije su određeni s bazom podataka SQL Azure, a ne primjenjuju se na Microsoft SQL Server.

## <a name="database-connection-errors-transient-errors-and-other-temporary-errors"></a>Pogreške pri povezivanju baze podataka, tranzitne pogrešaka i druge privremene pogreške

U sljedećoj su tablici pokriva SQL šifre pogrešaka za pogreške gubitka veze i druge tranzitne pogreške koje možete naići kada aplikacija pokušava pristup bazi podataka za SQL. Početak upute za prve korake za povezivanje s bazom podataka SQL Azure, potražite u članku [Povezivanje s bazom podataka SQL Azure](sql-database-libraries.md).

### <a name="most-common-database-connection-errors-and-transient-fault-errors"></a>Najčešće pogreške pri povezivanju baze podataka i tranzitne kvara pogreške

Infrastruktura za Azure ima mogućnost dinamički konfigurirajte poslužiteljima kada dođe do podebljanom radnih opterećenja u SQL baze podataka usluge.  Takvo ponašanje dinamički može uzrokovati klijentski program izgubiti vezu s bazom podataka SQL. Ove vrste pogreškom zove *tranzitne kvara*.

Klijentski program je pokušaj logike, možete je pokušati ponovno uspostaviti vezu nakon dodjeljivanja tranzitne kvara vrijeme da biste ispravili sam.  Preporučujemo da odgađanje 5 sekundi prije nego što prvi pokušaj za ponovno. Ponovni nakon stanke kraće od 5 sekundi rizika zbunili servisa u oblaku. Za svaki sljedeći pokušaj odgode eksponencijalno, trebali biste Povećaj maksimalno 60 sekundi do.

Pogreške tranzitne kvara obično manifesta kao jedan od sljedećih poruka o pogrešci iz klijentske programe:

- Baza podataka < db_name > na poslužitelju < Azure_instance > trenutno nije dostupna. Pokušajte ponovno kasnije veze. Ako se problem nastavi pojavljivati, obratite se službi i pošaljite im ID praćenje sesiju < session_id >

- Baza podataka < db_name > na poslužitelju < Azure_instance > trenutno nije dostupna. Pokušajte ponovno kasnije veze. Ako se problem nastavi pojavljivati, obratite se službi i pošaljite im ID praćenje sesiju < session_id >. (Microsoft SQL Server, pogreška: 40613)

- Udaljeno glavno računalo prisilno zatvorio postojeće veze.

- System.Data.Entity.Core.EntityCommandExecutionException: Pojavila se pogreška prilikom izvršavanja definicija naredbe. Pogledajte unutarnju iznimku detalje. ---> System.Data.SqlClient.SqlException: na razini prijenosa dogodila se pogreška prilikom primanja rezultata s poslužitelja. (davatelj usluga: sesiju davatelja, pogreška: 19 - fizičke veze nije moguće koristiti)

Primjer kod logike pokušaj, pogledajte:

- [Biblioteka veza za SQL baze podataka i SQL Server](sql-database-libraries.md) 
- [Akcije da biste ispravili pogreške pri povezivanju i tranzitne pogrešaka u bazi podataka SQL](sql-database-connectivity-issues.md)

Rasprave u *Blokiranje razdoblje* za klijente koji koriste ADO.NET dostupan je u [SQL Server veze red čekanja (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx).

### <a name="transient-fault-error-codes"></a>Kodovi pogrešaka tranzitne kvara

Sljedeće pogreške su tranzitne i treba ponoviti logike aplikacije 

| Kod pogreške | Težinu | Opis |
| ---: | ---: | :--- |
| 4060 | 16 | Nije moguće otvoriti bazu podataka "% & #x2a; ls" zatražio prijavu. Prijava u nije uspjelo. |
|40197|17|Servis je naišao na pogrešku obrade zahtjeva. Pokušajte ponovno. Šifra pogreške %d.<br/><br/>Primit ćete ta se pogreška kada je servis dolje softver ili hardver nadogradnje, hardverske pogreške ili prebacivanje problema. Kod pogreške (%d) ugrađen u porukama o pogrešci 40197 pruža dodatne informacije o vrsti pogreške ili prebacivanje do kojih je došlo. Primjeri pogreške kodovi su ugrađene unutar poruke o pogrešci 40197 su 40020, 40143, 40166 i 40540.<br/><br/>Ponovno povezivanje s poslužiteljem baze podataka SQL će automatski povezati s dobar kopiju baze podataka. Aplikacija morate Uhvatite evidencije pogrešaka u 40197, kod pogreške ugrađene (%d) unutar poruke o otklanjanju poteškoća i pokušajte se povezati s bazom podataka SQL dok su resursi dostupni, a zatim ponovno uspostaviti vezu.|
|40501|20|Servis privremeno nije dostupan. Ponovno zahtjev nakon 10 sekundi. Incidenta ID: %ls. Kod: %d.<br/><br/>*Bilješke:* Dodatne informacije potražite u članku:<br/>• [Ograničenja resursa baze podataka SQL azure](sql-database-resource-limits.md).
|40613|17|Baza podataka '% & #x2a; ls' na poslužitelju '% & #x2a; ls' trenutno nije dostupna. Pokušajte ponovno kasnije veze. Ako se problem nastavi pojavljivati, obratite se službi i pošaljite im sesiju praćenje ID '% & #x2a; ls'.|
|49918|16|Nije moguće obraditi zahtjev. Nema dovoljno resursa obrade zahtjeva.<br/><br/>Servis privremeno nije dostupan. Pokušajte ponovno zahtjev. |
|49919|16|Nije moguće postupak stvaranje ili ažuriranje zahtjev. Previše mnoge stvaranje ili ažuriranje operacije u tijeku za pretplatu "% ld".<br/><br/>Servis je zauzet obrade više stvaranje ili ažuriranje zahtjeva za pretplatu ili poslužitelju. Zahtjevi za trenutno su blokirane za optimizaciju resursa. Upit [sys.dm_operation_status](https://msdn.microsoft.com/library/dn270022.aspx) za operacije na čekanju. Pričekajte dok se na čekanju stvaranje ili ažuriranje zahtjeva dovršite ili izbrisali jedan od zahtjeva za na čekanju i ponovno pokušajte vaš zahtjev kasnije. |
|49920|16|Nije moguće obraditi zahtjev. Previše operacije u tijeku za pretplatu "% ld".<br/><br/>Servis je zauzet obradu više zahtjeva za ovu pretplatu. Zahtjevi za trenutno su blokirane za optimizaciju resursa. Upit [sys.dm_operation_status](https://msdn.microsoft.com/library/dn270022.aspx) za status operacije. Čekanje dok se ne čeka zahtjeve dovršite ili izbrisali jedan od zahtjeva za na čekanju i pokušajte ponovno vaš zahtjev kasnije. |

## <a name="database-copy-errors"></a>Pogreške kopiju baze podataka

Prilikom kopiranja baze podataka u bazi podataka SQL Azure možete biti pronađene sljedeće pogreške. Dodatne informacije potražite u članku [kopiranje baze podataka SQL Azure](sql-database-copy.md).


|Kod pogreške|Težinu|Opis|
|---:|---:|:---|
|40635|16|Klijenta s IP adresom '% & #x2a; ls' privremeno je onemogućen.|
|40637|16|Stvaranje kopije baze podataka trenutno je onemogućen.|
|40561|16|Kopiranje baze podataka nije uspjelo. Izvorišni ili ciljni baze podataka ne postoji.|
|40562|16|Kopiranje baze podataka nije uspjelo. Izvorišne baze podataka otkazan.|
|40563|16|Kopiranje baze podataka nije uspjelo. Ciljna baza podataka otkazan.|
|40564|16|Baza podataka nije uspjela zbog Interna pogreška. Odbacivanje ciljne baze podataka, a zatim pokušajte ponovno.|
|40565|16|Kopiranje baze podataka nije uspjelo. Najviše 1 kopiju Istodobni baze podataka iz istog izvora je dopušteno. Odbacivanje ciljne baze podataka, a zatim pokušajte ponovno.|
|40566|16|Baza podataka nije uspjela zbog Interna pogreška. Odbacivanje ciljne baze podataka, a zatim pokušajte ponovno.|
|40567|16|Baza podataka nije uspjela zbog interne pogreške. Odbacivanje ciljne baze podataka, a zatim pokušajte ponovno.|
|40568|16|Kopiranje baze podataka nije uspjelo. Izvorna baza podataka ima više nije dostupan. Odbacivanje ciljne baze podataka, a zatim pokušajte ponovno.|
|40569|16|Kopiranje baze podataka nije uspjelo. Ciljna baza podataka ima više nije dostupan. Odbacivanje ciljne baze podataka, a zatim pokušajte ponovno.|
|40570|16|Baza podataka nije uspjela zbog Interna pogreška. Odbacivanje ciljne baze podataka, a zatim pokušajte ponovno.|
|40571|16|Baza podataka nije uspjela zbog Interna pogreška. Odbacivanje ciljne baze podataka, a zatim pokušajte ponovno.|

## <a name="resource-governance-errors"></a>Pogreške upravljanja resursa

Sljedeće pogreške su uzrokovanih viškom korištenje resursa dok radite s bazom podataka SQL Azure. Ako, na primjer:

- Transakcije je otvorena predugo.
- Transakcije je držanjem previše zaključavanja.
- Aplikacija koristi previše memorije.
- Aplikacija previše troše `TempDb` prostora.

Povezane teme:

* Detaljnije informacije o dostupne su ovdje: [ograničenja resursa baze podataka SQL Azure](sql-database-resource-limits.md)

|Kod pogreške|Težinu|Opis|
|---:|---:|:---|
|10928|20|ID resursa: %d. Ograničenje %s za bazu podataka je %d te se do. Dodatne informacije potražite u članku [http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637).<br/><br/>ID resursa označava resurs koji je Pristigla ograničenje. Za Identifikator resursa radnih niti = 1. Za sesije, ID resursa = 2.<br/><br/>*Bilješke:* Dodatne informacije o tu pogrešku i kako da biste riješili problem, potražite u članku:<br/>• [Ograničenja resursa baze podataka SQL azure](sql-database-resource-limits.md). |
|10929|20|ID resursa: %d. Jamstva minimalne %s %d te je ograničenje najvećeg %d i trenutnog korištenja baze podataka je %d. Međutim, poslužitelj je trenutno zauzet i ne podržava veće od %d zahtjeve za tu bazu podataka. Dodatne informacije potražite u članku [http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637). U suprotnom, pokušajte ponovno kasnije.<br/><br/>ID resursa označava resurs koji je Pristigla ograničenje. Za Identifikator resursa radnih niti = 1. Za sesije, ID resursa = 2.<br/><br/>*Bilješke:* Dodatne informacije o tu pogrešku i kako da biste riješili problem, potražite u članku:<br/>• [Ograničenja resursa baze podataka SQL azure](sql-database-resource-limits.md).|
|40544|20|Baza podataka je Pristigla kvota veličina. Particija ili brisanje podataka ispustite indeksa ili pročitajte dokumentaciju za moguća rješenja.|
|40549|16|Sesije je prekinut jer ste instalirali dugoročnih transakcije. Pokušajte ćete skratiti svoje transakcije.|
|40550|16|Sesije prekinut je jer ga je dobiven previše zaključavanja. Pokušajte čitanje ili izmjena manji broj redaka u jednu transakcije.|
|40551|16|Sesije je prekinuta zbog viškom `TEMPDB` korištenje. Pokušajte izmjena upit da biste smanjili korištenje prostora privremene tablice.<br/><br/>*Savjet:* Ako koristite privremenih objekata, Štednja prostora na `TEMPDB` baze podataka tako da ispustite privremenih objekata nakon više nije potrebna, sesiju.|
|40552|16|Sesije je prekinuta zbog korištenje prostora zapisnika viškom transakcije. Pokušajte izmjena manji broj redaka u jednu transakcije.<br/><br/>*Savjet:* Ako započnete skupno umeće pomoću na `bcp.exe` utility ili `System.Data.SqlClient.SqlBulkCopy` klase, pokušajte upotrijebiti u `-b batchsize` ili `BatchSize` mogućnosti da biste ograničili broj redaka koji se kopiraju u poslužitelju u pojedine transakcije. Ako se ponovno stvaranje indeks na `ALTER INDEX` naredbe, pokušajte koristiti u `REBUILD WITH ONLINE = ON` mogućnost.|
|40553|16|Sesije je prekinuta zbog viškom memorije. Pokušajte izmjena upit za obradu manje redaka.<br/><br/>*Savjet:* Smanjite broj `ORDER BY` i `GROUP BY` operacije u Transact-SQL kodu smanjuje potrebe za memorijom upita.|

## <a name="elastic-pool-errors"></a>Elastic skup pogreške

Su sljedeće pogreške vezane uz stvaranje i korištenje Elastics grupe.

| Pogreške | ErrorSeverity | ErrorFormat | ErrorInserts | ErrorCause | ErrorCorrectiveAction |
| :-- | :-- | :-- | :-- | :-- | :-- |
| 1132 | EX_RESOURCE | Elastic skup je Pristigla ograničenje prostora za pohranu. Korištenje prostora za pohranu za elastic grupe aplikacija ne može prelaziti (%d) MB prostora. | Ograničenje prostora elastic skup u MB prostora. | Pokušaj zapisivanje podataka u bazu podataka kada ograničenje prostora za pohranu od elastic skup sadrži je dostigne. | Razmislite o povećanju DTUs od elastic skup ako je moguće da biste povećali ograničenje prostora za pohranu, smanjiti prostor za pohranu koriste pojedinačne baze podataka unutar elastic skup ili uklanjanje baze podataka iz elastic grupe aplikacija. |
| 10929 | EX_USER | Jamstva minimalne %s %d te je ograničenje najvećeg %d i trenutnog korištenja baze podataka je %d. Međutim, poslužitelj je trenutno zauzet i ne podržava veće od %d zahtjeve za tu bazu podataka. Pogledajte [http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637) za pomoć. U suprotnom, pokušajte ponovno kasnije. | Min DTU po bazi podataka; Max DTU po bazi podataka | Ukupan broj Istodobni zaposlenici zaduženi za (zahtjeve) preko sve baze podataka u elastic pokušali premašuje ograničenje grupe aplikacija. | Razmislite o povećanju DTUs elastic skup ako je moguće da biste povećali ograničenje tempiranja ili uklanjanje baze podataka iz elastic grupe aplikacija. |
| 40844 | EX_USER | Baza podataka '%ls' na poslužitelju '%ls' '%ls' edition baze podataka u elastic skup i ne smije sadržavati odnos neprekinuti Kopiraj. | Naziv baze podataka, edition baze podataka, a zatim naziv poslužitelja | Naredbe StartDatabaseCopy izdaje za koje nisu premium db u elastic grupe aplikacija. | dolazim brzo |
| 40857 | EX_USER | Elastic skup nije pronađen za poslužitelj: '%ls', naziv elastic grupe aplikacija: '%ls'. | Naziv poslužitelja; Naziv grupe elastic aplikacija | Određeni skup elastic ne postoji u navedenom poslužitelju. | Navedite naziv valjani elastic skup. |
| 40858 | EX_USER | Elastic skup '%ls' već postoji u poslužitelja: '%ls' | elastic naziv grupe aplikacija, naziv poslužitelja | Određeni skup elastic već postoji u navedenom logičke poslužitelju. | Unesite novi naziv grupe elastic aplikacija. |
| 40859 | EX_USER | Elastic grupe aplikacija ne podržava usluga sloju '%ls'. | Razina elastic skup servisa | Navedeni servis sloju nije podržana za dodjelu resursa elastic skup. | Podijelite odgovarajuće izdanje ili servis sloju ostavite prazno da biste koristili zadani sloja u sustavu. |
| 40860 | EX_USER | Elastic skup '%ls' servisa cilj '%ls' kombinacija i nije valjana. | elastic naziv grupe aplikacija; Ciljna naziv razini usluge | Elastic ciljne skupine aplikacija i servisa može se navesti zajedno samo ako je servis cilj je navedena kao "ElasticPool". | Navedite točne kombinacije elastic grupe aplikacija i servisa cilj. |
| 40861 | EX_USER | Izdanje baze podataka ' %. *ls' ne može se razlikovati od sloju elastic skup servisa koji je ' %.* ls'. | izdanje baze podataka, sloju elastic skup servisa | Izdanje baze podataka se razlikuje od servisa sloju elastic skup. | Navedite edition baze podataka koji se razlikuje od servisa sloju elastic skup.  Imajte na umu da edition baze podataka ne mora biti navedena. |
| 40862 | EX_USER | Elastic skup naziv mora biti navesti ako servis cilj elastic skup. | Ništa | Cilj servisa elastic skup identificirati samo elastic grupe aplikacija. | Navedite naziv elastic skup ako pomoću servisa cilj elastic skup. |
| 40864 | EX_USER | DTUs za elastic skup mora biti najmanje (%d) DTUs za servis sloju ' %. * ls'. | DTUs za elastic skup; elastic skup sloju servisa. | Pokušaj da biste postavili DTUs za elastic skup ispod minimalnu granicu. | Pokušajte ponovno postavljanje na DTUs za na elastic grupe aplikacija za barem minimalnu granicu. |
| 40865 | EX_USER | DTUs za elastic grupe aplikacija ne smije biti (%d) DTUs za servis sloju ' %. * ls'. | DTUs za elastic skup; elastic skup sloju servisa. | Pokušaj da biste postavili DTUs za elastic skup iznad maksimalno ograničenje. | Pokušajte ponovno postavljanje DTUs za elastic skup ne više od maksimalno ograničenje. |
| 40867 | EX_USER | DTU Maks po bazi podataka mora biti najmanje (%d) za servis sloju "%. * ls'. | Max DTU po bazi podataka; Razina elastic skup servisa | Pokušaj da biste postavili max DTU po bazi podataka ispod podržanog ograničenja. | Razmotrite korištenje sloju elastic skup servisa koji podržava željenu postavku. |
| 40868 | EX_USER | DTU Maks po bazi podataka ne smije biti (%d) za servis sloju ' %. * ls'. | Max DTU po bazi podataka; elastic skup sloju servisa. | Pokušaj da biste postavili max DTU po bazi podataka izvan podržanog ograničenja. | Razmotrite korištenje sloju elastic skup servisa koji podržava željenu postavku. |
| 40870 | EX_USER | Min DTU po bazi podataka ne smije biti (%d) za servis sloju ' %. * ls'. | Min DTU po bazi podataka; elastic skup servisa sloju. | Pokušaj da biste postavili min DTU po bazi podataka izvan podržanog ograničenja. | Razmotrite korištenje sloju elastic skup servisa koji podržava željenu postavku. |
| 40873 | EX_USER | Broj baza podataka (%d) i DTU min po bazi podataka (%d) ne može prelaziti DTUs elastic skup (%d). | Broj baza podataka u elastic skup; Min DTU po bazi podataka; DTUs elastic skup. | Pokušaj da biste odredili DTU min za baze podataka u elastic koji premašuje DTUs elastic grupe aplikacija. | Razmislite o povećanju DTUs elastic skup ili Smanji min DTU po bazi podataka ili smanjiti broj baza podataka u elastic. |
| 40877 | EX_USER | Elastic grupe aplikacija se ne može izbrisati ako nijedne baze podataka. | Ništa | Elastic skup sadrži jednu ili više baza podataka i stoga se ne može izbrisati. | Uklonite baze podataka s elastic skup da biste ga izbrisali. |
| 40881 | EX_USER | Elastic skup ' %. * ls' je popunjena baze podataka broj.  Za elastic skup s (%d) DTUs ograničenje broja baze podataka za elastic grupe aplikacija ne smije prelaziti (%d). | Naziv grupe aplikacija elastic; Baza podataka broj ograničenje od elastic skup; e DTUs za resurse. | Pokušavate stvoriti ili do limita count baze podataka elastic skup elastic skup dodati baze podataka. | Razmislite o povećanju DTUs elastic skup ako je moguće da biste povećali ograničenje baze podataka ili uklanjanje baze podataka iz elastic grupe aplikacija. |
| 40889 | EX_USER | DTUs ili ograničenje prostora za pohranu za elastic skup ' %. * ls' se ne može smanjiti jer koji ne bi im dovoljno prostora za pohranu za njegov baze podataka. | Naziv elastic skup. | Pokušava smanjiti ograničenje prostora za pohranu od elastic skup ispod njegova korištenja za pohranu. | Razmotrite korištenje prostora za pohranu pojedinačne baze podataka u elastic smanjivanje ili uklanjanje baze podataka na resurse da biste smanjili razinu DTUs ili ograničenje prostora za pohranu. |
| 40891 | EX_USER | Min DTU po bazi podataka (%d) ne može prelaziti max DTU po bazi podataka (%d). | Min DTU po bazi podataka; DTU Maks po bazi podataka. | Pokušaj da biste postavili min DTU po veće od maksimalno DTU po bazi podataka u bazi podataka. | Provjerite je li min DTU po baze podataka premašuju maksimalno DTU po bazi podataka. |
| TBD | EX_USER | Veličina prostora za pohranu za pojedinačne bazu podataka u elastic grupe aplikacija ne može prelaziti maksimalnog ograničenja veličine dopušteno mjerodavnim ' %. * ls' servisa elastic skup sloju. | Razina elastic skup servisa | Maksimalni Veličina baze podataka premašuje maksimalnog ograničenja veličine dopušteno mjerodavnim elastic skup sloja u sustavu. | Postavite maksimalni veličinu baze podataka unutar ograničenja maksimalnog ograničenja veličine dopušteno mjerodavnim elastic skup sloja u sustavu. |

Povezane teme:

* [Stvaranje baze podataka za elastic grupe aplikacija (C#)](sql-database-elastic-pool-create-csharp.md) 
* [Upravljanje elastic baze podataka grupe aplikacija (C#)](sql-database-elastic-pool-manage-csharp.md). 
* [Stvaranje skupa elastic baze podataka (PowerShell)](sql-database-elastic-pool-create-powershell.md) 
* [Nadzor i upravljanje skup elastic baze podataka (PowerShell)](sql-database-elastic-pool-manage-powershell.md).

## <a name="general-errors"></a>Općenite pogreške

Sljedeće pogreške podijeljene sve prethodne kategorije.

|Kod pogreške|Težinu|Opis|
|---:|---:|:---|
|15006|16|<AdministratorLogin>nije valjani naziv jer sadrži znakove koji nisu valjani.|
|18452|14|Prijava nije uspjela. Prijava u potječe iz nepouzdanih domene i ne mogu se koristiti sa sustavom Windows authentication.%. & #x2a; ls (prijave u sustav Windows nisu podržane u ovoj verziji programa SQL Server.)|
|18456|14|Prijava nije uspjela za korisnika ' %. & #x2a;ls'.%. & #x2a % ls. & #x2a; ls (za korisnika nije uspjela prijavu "% & #x2a; ls". Promjena lozinke nije uspjelo. Promjena lozinke tijekom prijavljivanja nije podržan u ovoj verziji programa SQL Server.)|
|18470|14|Prijava nije uspjela za korisnika '% & #x2a; ls'. Razlog: Račun je disabled.% & #x2a; ls|
|40014|16|Više baza podataka se ne može koristiti u istom transakcijom.|
|40054|16|Tablice bez grupni indeks nisu podržane u ovoj verziji programa SQL Server. Stvaranje indeksa grupirani pa pokušajte ponovno.|
|40133|15|Ovaj postupak nije podržan u ovoj verziji programa SQL Server.|
|40506|16|Navedeni SID nije valjan za ovu verziju sustava SQL Server.|
|40507|16|"%. & #x2a; ls' nije moguće pozvati s parametrima u ovoj verziji programa SQL Server.|
|40508|16|KORIŠTENJE naredbe nije podržan da biste se prebacivali između baza podataka. Koristite novu vezu da biste se povezali s drugom bazom podataka.|
|40510|16|Naredba '% & #x2a; ls' nije podržan u ovoj verziji programa SQL Server|
|40511|16|Ugrađene funkcije '% & #x2a; ls' nije podržan u ovoj verziji programa SQL Server.|
|40512|16|Zastarjeli značajka '%ls' nije podržana u ovoj verziji programa SQL Server.|
|40513|16|Poslužitelj varijable '% & #x2a; ls' nije podržan u ovoj verziji programa SQL Server.|
|40514|16|'%ls' nije podržan u ovoj verziji programa SQL Server.|
|40515|16|Reference za naziv baze podataka i/ili poslužitelju u '% & #x2a; ls' nije podržan u ovoj verziji programa SQL Server.|
|40516|16|Globalni temp objekti nisu podržani u ovoj verziji programa SQL Server.|
|40517|16|Mogućnost ključnu riječ "ili" Izjava '% & #x2a; ls' nije podržan u ovoj verziji programa SQL Server.|
|40518|16|Naredba DBCC '% & #x2a; ls' nije podržan u ovoj verziji programa SQL Server.|
|40520|16|S_MSG sigurnosni klasa "%" nije podržan u ovoj verziji programa SQL Server.|
|40521|16|S_MSG sigurnosni klasa "%" nije podržan u opseg poslužitelja u ovoj verziji programa SQL Server.|
|40522|16|Vrsta glavnog '% & #x2a; ls' baze podataka nije podržan u ovoj verziji programa SQL Server.|
|40523|16|Stvaranje implicitnog korisničkih '% & #x2a; ls' nije podržan u ovoj verziji programa SQL Server. Prije korištenja izričito stvorite korisnika.|
|40524|16|Vrsta podataka '% & #x2a; ls' nije podržan u ovoj verziji programa SQL Server.|
|40525|16|S '%.ls' nije podržan u ovoj verziji programa SQL Server.|
|40526|16|"%. & #x2a; davatelj usluga za skup redaka ls korisnika nije podržan u ovoj verziji programa SQL Server.|
|40527|16|Povezani poslužitelji nisu podržane u ovoj verziji programa SQL Server.|
|40528|16|Korisnici ne može se mapirati certifikata, asimetričnim ključeve ili prijave u sustav Windows u ovoj verziji programa SQL Server.|
|40529|16|Ugrađene funkcije '% & #x2a; ls' u oponašanja kontekst nije podržan u ovoj verziji programa SQL Server.|
|40532|11|Nije moguće otvoriti poslužitelja "% & #x2a; ls" zatražio prijave. Prijava u nije uspjelo.|
|40553|16|Sesije je prekinuta zbog viškom memorije. Pokušajte izmjena upit za obradu manje redaka.<br/><br/>*Bilješke:* Smanjite broj `ORDER BY` i `GROUP BY` operacije u Transact-SQL kodu pomaže smanjiti potrebe za memorijom upita.|
|40604|16|Stvaranje/ALTER baze podataka nije moguće jer prekoračuje kvote poslužitelja.|
|40606|16|Prilaganje baze podataka nije podržan u ovoj verziji programa SQL Server.|
|40607|16|Prijava u sustav Windows nisu podržane u ovoj verziji programa SQL Server.|
|40611|16|Poslužitelji može sadržavati najviše 128 vatrozid definirana pravila.|
|40614|16|IP adresa za početak pravila vatrozida ne može prelaziti kraj IP adresa.|
|40615|16|Nije moguće otvoriti poslužitelja '{0}"zatražio prijavu. Klijenta s IP adresom "{1}" nije dopuštena za pristup poslužitelju.  Da biste omogućili pristup pomoću portala za baze podataka SQL ili pokrenuti sp_set_firewall_rule na glavnom bazom podataka da biste stvorili pravilo Vatrozid za taj IP adresa ili raspon adresu.  Može potrajati do pet minuta za ove promjene stupile na snagu.|
|40617|16|Naziv pravila vatrozida koji započinje <rule name> predug. Maksimalna duljina je 128.|
|40618|16|Naziv pravila vatrozida ne može biti prazna.|
|40620|16|Prijava u nije uspjela za korisnika "% & #x2a; ls". Promjena lozinke nije uspjelo. Promjena lozinke tijekom prijavljivanja nije podržan u ovoj verziji programa SQL Server.|
|40627|20|Operacija na poslužitelju "{0}" i baze podataka "{1}" je u tijeku. Pričekajte nekoliko minuta prije ponovnog pokušaja.|
|40630|16|Nije uspjela provjera valjanosti lozinku. Lozinka ne zadovoljava preduvjete pravila jer je premalen.|
|40631|16|Lozinka koju ste naveli predug. Lozinka mora imati više od 128 znakova.|
|40632|16|Nije uspjela provjera valjanosti lozinku. Lozinka ne zadovoljava preduvjete pravila jer nije dovoljno složene.|
|40636|16|Ne možete koristiti naziv rezervirane baze podataka '% & #x2a; ls' u ovaj postupak.|
|40638|16|Id pretplate koji nisu valjani < id pretplate >. Pretplata ne postoji.|
|40639|16|Zahtjev nisu u skladu sa shemom: <schema error>.|
|40640|20|Poslužitelj je naišao neočekivane iznimke.|
|40641|16|Na određeno mjesto nije valjana.|
|40642|17|Poslužitelj je trenutno zauzet. Pokušajte ponovno kasnije.|
|40643|16|Vrijednost navedena zaglavlja x ms-verzija nije valjana.|
|40644|14|Autorizirajte pristup navedena pretplata nije uspjelo.|
|40645|16|Naziv poslužitelja <servername> ne može biti prazna niti null. Ona može samo se sastoji od malim slovima "a"-"z", brojeve 0-9 i spojnice. Spojnice možda ne potencijalnog klijenta ili trag u nazivu.|
|40646|16|ID pretplate ne može biti prazna.|
|40647|16|Pretplata < id pretplate nemate server naziv poslužitelja.|
|40648|17|Izvedeno je previše zahtjeva. Pokušajte ponovno.|
|40649|16|Navedeni su nije valjana vrsta sadržaja. Podržana je samo/xml-a aplikacije.|
|40650|16|Pretplatu < id pretplate > ne postoji ili nije spreman za operaciju.|
|40651|16|Nije uspjelo stvaranje poslužitelja jer je onemogućena < id pretplate > pretplate.|
|40652|16|Ne možete premještati ili stvorite poslužitelj. Pretplata < id pretplate > će biti dulji od kvota poslužitelja.|
|40671|17|Pogreške u komunikaciji između pristupnika i servisa za upravljanje. Pokušajte ponovno.|
|40852|16|Nije moguće otvoriti bazu podataka ' %. *ls' na poslužitelju ' %.* ls' zatražio prijavu. Pristup bazi podataka dopuštena samo pomoću niza za povezivanje omogućena sigurnost. Da biste pristupili Ova baza podataka, izmijenite nizu za povezivanje sadrži "sigurne" na poslužitelju FQDN -.database.windows "naziv poslužitelja".net mora biti izmijenjeni tako da .database "naziv poslužitelja". `secure`. windows.net.|
|45168|16|U sustavu SQL Azure je opterećenju pa je postavljanje gornju granicu na istovremene operacije DB CRUD za pojedinačni poslužitelj (npr., stvaranje i baze podataka). Poslužitelj koji je naveden u poruci o pogrešci premašila maksimalni broj Istodobni veza. Pokušajte ponovno kasnije.|
|45169|16|U sustavu SQL azure je opterećenju, a da postavite gornju granicu broj operacije CRUD Istodobni server jedne pretplate (npr., stvaranje i server). Pretplata na navedeno u poruci o pogrešci premašila maksimalni broj Istodobni veza, a zahtjev je odbijen. Pokušajte ponovno kasnije.|

## <a name="related-links"></a>Srodne veze

- [Smjernice i ograničenja Općenito baze podataka Azure SQL](sql-database-general-limitations.md)
- [Ograničenja resursa za Azure SQL baze podataka](sql-database-resource-limits.md)
