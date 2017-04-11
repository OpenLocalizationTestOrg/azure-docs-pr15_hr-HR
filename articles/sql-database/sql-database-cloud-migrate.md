<properties
   pageTitle="Migracija baze podataka SQL Server s bazom podataka SQL | Microsoft Azure"
   description="Saznajte kako otprilike lokalnog sustava SQL Server Migracija baze podataka s bazom podataka SQL Azure u oblaku. Pomoću alata za migraciju baze podataka za provjeru kompatibilnosti prije migracije baze podataka."
   keywords="baze podataka za migraciju, Migracija baze podataka za sql server, a zatim Alati za migraciju baze podataka, migrirati bazu podataka, migrirati bazu podataka za sql"
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
   ms.workload="sqldb-migrate"
   ms.date="08/24/2016"
   ms.author="carlrab"/>

# <a name="sql-server-database-migration-to-sql-database-in-the-cloud"></a>Migracija baze podataka SQL Server s bazom podataka SQL u oblaku

U ovom se članku saznat ćete kako će se migrirati lokalnog sustava SQL Server 2005 ili noviji baze podataka s bazom podataka SQL Azure. U ovaj postupak migracije baze podataka migracije sheme i podataka iz baze podataka sustava SQL Server u okruženju sustava trenutni u SQL baze podataka. Da, postojeću bazu podataka najprije mora proći test kompatibilnosti. S [V12 baze podataka SQL](sql-database-v12-whats-new.md)postoji nekoliko preostale probleme s kompatibilnošću, osim problem koji se odnose na razini poslužitelja i izdvojiti bazu podataka operacija. Baza podataka i aplikacije koje se temelje na [djelomično ili nepodržane funkcije](sql-database-transact-sql-information.md) morate neke ponovno inženjerska da biste riješili problem te nekompatibilnosti prije moguće je premjestiti baze podataka SQL Server.

Da biste migrirali, sljedeće su navedeni koraci da iskoristite:

- **Test za kompatibilnost**: provjera valjanosti baza podataka kompatibilna s [SQL baza podataka V12](sql-database-v12-whats-new.md). 
- **Rješavanje problema s kompatibilnošću, ako postoje**: Ako ne uspije provjera valjanosti, morate popraviti pogreške provjere valjanosti.  
- **Izvedi migracije** Kada kompatibilan bazu podataka, možete koristiti jednu ili više načina da biste izvršili migraciju. 

SQL Server nudi nekoliko načina za obavljanje svaki od tih zadataka. Ovaj članak sadrži pregled dostupni načini za svaki zadatak. Sljedeći dijagram prikazuje korake i metode.

  ![Dijagram VSSSDT migracije](./media/sql-database-cloud-migrate/03VSSSDTDiagram.png)
  
 > [AZURE.NOTE] Migracija koji nisu iz programa SQL Server baze podataka, uključujući Microsoft Access, Sybase, MySQL Oracle i DB2 s bazom podataka SQL Azure, potražite u članku [Pomoćnika za SQL Server migracije](http://blogs.msdn.com/b/ssma/).

## <a name="database-migration-tools-test-sql-server-database-compatibility-with-sql-database"></a>Alati za migraciju baze podataka testiranje baza podataka sustava SQL Server kompatibilna s SQL baze podataka

Da biste provjerili probleme s kompatibilnošću baze podataka SQL prije nego što počnete postupak migracije baze podataka, primijenite jednu od sljedećih načina:

> [AZURE.SELECTOR]
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
- [SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md)
- [Savjetnik za nadogradnju](http://www.microsoft.com/download/details.aspx?id=48119)
- [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)

- [SQL Server Data Tools za Visual Studio ("SSDT")](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md): SSDT koristi najnovije pravila za kompatibilnost za otkrivanje nekompatibilnosti V12 SQL baze podataka. Ako se otkriju nekompatibilnosti, može riješiti otkriveni problemi izravno u ovom alatu. Ta je metoda preporučeni je način da biste testirali i otklanjanje problema vezanih uz kompatibilnost V12 SQL baze podataka. 
- [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md): SqlPackage je programa naredbenog retka koji testira za kompatibilnost problema i generira izvješće koje sadrži otkriveni problemi. Ako koristite ovaj alat, provjerite je li koristiti najnovije pravila kompatibilnosti koristite najnoviju verziju. Ako su otkrivene pogreške, morate koristiti neki drugi alat da biste riješili eventualne probleme s kompatibilnošću otkriveni - SSDT preporučuje se.  
- [Čarobnjak aplikacije sloju u izvoz podataka u programu SQL Server Management Studio](sql-database-cloud-migrate-determine-compatibility-ssms.md): čarobnjak otkrije izvješćima i pogrešaka na zaslon. Ako su otkrivene pogreške, možete nastaviti i dovršiti migracije u SQL baze podataka. Ako su otkrivene pogreške, morate koristiti neki drugi alat da biste riješili eventualne probleme s kompatibilnošću otkriven - SSDT preporučuje se.
- [U Microsoft SQL Server 2016 nadograditi Savjetnik za pretpregled](http://www.microsoft.com/download/details.aspx?id=48119): ovaj alat samostalne koja je trenutno u pretpregledu, otkriva i generira izvješće o nekompatibilnosti V12 SQL baze podataka. Ovaj alat još ne sadrži najnovije kompatibilnosti pravila. Ako su otkrivena bez pogrešaka, možete nastaviti i dovršiti migracije u SQL baze podataka. Ako su otkrivene pogreške, morate koristiti neki drugi alat da biste riješili eventualne probleme s kompatibilnošću otkriven - SSDT preporučuje se. 
- [Čarobnjak za migraciju SQL Azure ("SAMW")](sql-database-cloud-migrate-fix-compatibility-issues.md): SAMW codeplex alat je koji koristi pravila V11 za baze podataka SQL Azure kompatibilnosti za otkrivanje nekompatibilnosti V12 za baze podataka SQL Azure. Ako su otkrivena nekompatibilnosti, neke od problema moguće popraviti izravno u ovom alatu. Ovaj alat pronaći nekompatibilnosti koje treba popraviti. Prvi baze podataka SQL Azure pomoć alata za migraciju dostupan je i aktivno podržano u zajednici korisnika sustava SQL Server. Osim toga, ovaj alat mogu provoditi Migracija iz alata za samu. 

## <a name="fix-database-migration-compatibility-issues"></a>Rješavanje problema s kompatibilnošću Migracija baze podataka

Ako otkrije probleme s kompatibilnošću, morate ih ispraviti prije nastavka Migracija baze podataka sustava SQL Server. Postoji velik broj problema s kompatibilnošću koje biste mogli naići, ovisno o vašoj na verziju sustava SQL Server u izvorišne baze podataka i složenost premještate bazu podataka. Starijih verzija sustava SQL Server imati više problema s kompatibilnošću. Pomoću sljedećih resursa, osim ciljano Internet pretraživanje pomoću tražilica mogućnosti:

- [Značajke bazi podataka sustava SQL Server nije podržan u bazi podataka SQL Azure](sql-database-transact-sql-information.md)
- [Funkcija modul ukinute baze podataka u sustavu SQL Server 2016](https://msdn.microsoft.com/library/ms144262%28v=sql.130%29)
- [Funkcija modul ukinute baze podataka u sustavu SQL Server 2014.](https://msdn.microsoft.com/library/ms144262%28v=sql.120%29)
- [Funkcija modul ukinute baze podataka u sustavu SQL Server 2012.](https://msdn.microsoft.com/library/ms144262%28v=sql.110%29)
- [Funkcija modul ukinute baze podataka u SQL Server 2008 R2](https://msdn.microsoft.com/library/ms144262%28v=sql.105%29)
- [Funkcija modul ukinute baze podataka u SQL Server 2005](https://msdn.microsoft.com/library/ms144262%28v=sql.90%29)

Osim pretraživanje Interneta i korištenje tih resursa pomoću [forumi zajednice korisnika MSDN SQL Server](https://social.msdn.microsoft.com/Forums/sqlserver/home?category=sqlserver) ili [StackOverflow](http://stackoverflow.com/).

Da biste riješili probleme otkriven, primijenite jednu od sljedećih Alati za migraciju baze podataka:

> [AZURE.SELECTOR]
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- [SSMS](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md)
- [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)

- Koristi [SQL Server Data Tools za Visual Studio ("SSDT")](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md): da biste koristili SSDT, koje uvoziti sheme baze podataka SQL Server Data Tools za Visual Studio "SSDT") i izraditi projekta za implementaciju V12 SQL baze podataka. Zatim ispravite sve otkriveni problemi u SSDT. Po dovršetku sinkronizirati promjene natrag na izvorišne baze podataka (ili kopiju izvorišne baze podataka. SSDT trenutno preporučeni je način da biste testirali i otklanjanje problema vezanih uz kompatibilnost V12 SQL baze podataka. Slijedite vezu za [walk-through pomoću SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md).
- Korištenje [SQL Server Management Studio ("SSMS")](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md): da biste koristili SSMS, izvršavanje Transact-SQL naredbe da biste ispravili pogreške otkriven pomoću alata za drugi. Ta je metoda prvenstveno za napredne korisnike da biste izmijenili shemu baze podataka izravno u izvorišne baze podataka. 
- Koristite [Čarobnjak za migraciju SQL Azure ("SAMW")](sql-database-cloud-migrate-fix-compatibility-issues.md): da biste koristili SAMW, generiranje Transact-SQL skripte iz izvorišne baze podataka. Čarobnjak pretvorbe skriptu, kad god je to moguće, da biste u shemi kompatibilan s V12 baze podataka SQL. Po dovršetku SAMW možete povezati s V12 baze podataka SQL izvršiti skriptu. Ovaj alat analizira i praćenje datoteke da biste odredili problemi s kompatibilnošću. Skripta možete generirao samo shemu ili mogu sadržavati podatke u obliku BCP.

## <a name="migrate-a-compatible-sql-server-database-to-sql-database"></a>Migriranje kompatibilne baze podataka SQL Server u SQL baze podataka

Da biste migrirali kompatibilne baze podataka SQL Server, Microsoft pruža nekoliko načina za migraciju različitim scenarijima za. Način koji ćete odabrati ovisi o tome na odstupanje za nedostupnost, veličini i složenosti bazom podataka sustava SQL Server i stanje veze u oblak Microsoft Azure.  

> [AZURE.SELECTOR]
- [Čarobnjak za migraciju SSMS](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md)
- [Izvoz u datoteku BACPAC](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [Uvezi iz datoteke BACPAC](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [Transakcijskih replikacije](sql-database-cloud-migrate-compatible-using-transactional-replication.md)

Da biste odabrali način migracije, prvo pitanje na blog pitajte je smijete ako može potrajati baze podataka iz radnog tijekom migracije. Migracija baze podataka dok je aktivna transakcije su koje su se pojavile može rezultirati nedosljednosti baze podataka i oštećenje moguće baze podataka. Postoje brojne načine postupno bazu podataka, iz onemogućiti povezivanje s klijentom za stvaranje [snimke baze podataka](https://msdn.microsoft.com/library/ms175876.aspx).

Da biste migrirali s minimalnim nedostupnost, koristite [replikacije transakcije SQL Server](sql-database-cloud-migrate-compatible-using-transactional-replication.md) bazu podataka zadovoljava preduvjete za transakcijskih replikacije. Ako možete smijete neke nedostupnost ili izvodite test Migracija baze podataka radnog kasnije migracije, razmislite o jedan od sljedeća tri načina:

- [Čarobnjak za migraciju SSMS](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md): za male srednje bazama podataka migracije kompatibilne SQL Server 2005 ili noviji baze podataka jednostavno je radi [Implementacije baze podataka radi Čarobnjak za baze podataka Microsoft Azure](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md) u SQL Server Management Studio.
- [Izvoz u datoteku BACPAC](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md) , a zatim [Uvezi iz datoteke BACPAC](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md): Ako ste izazove povezivanja (bez povezivanja, niskoj propusnosti mreže ili vremenskog ograničenja problemi) i srednje velike bazama podataka, koristite [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) datoteke. Ako koristite taj način, možete izvesti shemu sustava SQL Server i podatke u BACPAC datoteku. Zatim uvoz datoteke BACPAC u SQL baze podataka pomoću podataka sloju aplikacije čarobnjaka za izvoz u SQL Server Management Studio ili uslužni [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) naredbeni redak.
- Zajedničko korištenje BACPAC i BCP: pomoću [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) datoteke i [BCP](https://msdn.microsoft.com/library/ms162802.aspx) za većom baze podataka da biste postigli veću parallelization performanse povećava koriste Premda veći složenosti. Ako koristite taj način, zasebno migrirati u shemi i podatke.
 - [Izvoz u shemi samo u datoteku BACPAC](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md).
 - [Uvoz u shemi samo iz datoteke BACPAC](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md) u SQL baze podataka.
 - Da biste izdvojili podatke u plošnu datoteke, a zatim [paralelno učitavanja](https://technet.microsoft.com/library/dd425070.aspx) te datoteke u bazu podataka SQL Azure pomoću [BCP](https://msdn.microsoft.com/library/ms162802.aspx) .

     ![SQL Server baze podataka migracije - migrirati SQL baze podataka s oblakom.](./media/sql-database-cloud-migrate/01SSMSDiagram_new.png)

## <a name="next-steps"></a>Daljnji koraci

- [Pretpregled za Savjetnik za nadogradnju sustava Microsoft SQL Server 2016](http://www.microsoft.com/download/details.aspx?id=48119)
- [Najnovija verzija sustava SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Najnovija verzija sustava SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)

##<a name="additional-resources"></a>Dodatni resursi

- [SQL baza podataka V12](sql-database-v12-whats-new.md)
[Transact-SQL djelomično ili nepodržane funkcije](sql-database-transact-sql-information.md)
- [Migracija baze podataka koje nisu iz programa SQL Server pomoću pomoćnika za SQL Server migracije](http://blogs.msdn.com/b/ssma/)
