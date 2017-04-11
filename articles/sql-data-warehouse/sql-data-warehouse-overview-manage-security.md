<properties
   pageTitle="Zaštita baze podataka u SQL Data Warehouse | Microsoft Azure"
   description="Savjeti za osiguravanje baze podataka u skladištu podataka za SQL Azure za razvoj rješenja."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="ronortloff"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/24/2016"
   ms.author="rortloff;barbkess;sonyama"/>

# <a name="secure-a-database-in-sql-data-warehouse"></a>Zaštita baze podataka u skladištu podataka za SQL

> [AZURE.SELECTOR]
- [Pregled sigurnosti](sql-data-warehouse-overview-manage-security.md)
- [Provjera autentičnosti](sql-data-warehouse-authentication.md)
- [Šifriranje (Portal)](sql-data-warehouse-encryption-tde.md)
- [Šifriranje (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)

U ovom se članku vodi kroz osnove zaštita Azure SQL Data Warehouse baze podataka. Točnije, u ovom se članku će početak rada s resursima za ograničavanje pristupa, zaštita podataka i nadzor aktivnosti u bazi podataka.

## <a name="connection-security"></a>Sigurnost veze

Sigurnost veze se odnosi na kako ograničiti i sigurnu vezu u bazu podataka pomoću pravila vatrozida i šifriranje veze.

Pravila vatrozida koriste poslužitelj i bazu podataka da biste odbacili pokušaja povezivanja s IP adrese nisu izričito whitelisted. Da dopušta veze iz aplikacije ili javnu IP adresu klijentskom računalu, najprije morate stvoriti pravila vatrozida razini poslužitelja pomoću portala za Azure, REST API-JA ili PowerShell. Kao preporučenim načinom rada, trebali biste ograničiti dopušteno kroz vatrozid za poslužitelj najveću moguću rasponi IP adresa.  Da biste pristupili Azure SQL Data Warehouse s lokalnog računala, provjerite je li vatrozid na vašoj mreži i lokalnom računalu omogućuje izlaznu komunikaciju na TCP priključak 1433.  Dodatne informacije potražite u članku [baze podataka SQL Azure vatrozid][], [sp_set_firewall_rule][]i [sp_set_database_firewall_rule][].

Po zadanom su šifrirane veze s SQL Data Warehouse.  Izmjena postavke veze da biste onemogućili šifriranje se zanemaruju.

## <a name="authentication"></a>Provjera autentičnosti

Provjera autentičnosti se odnosi na kako dokazivanje vašeg identiteta prilikom povezivanja s bazom podataka. SQL Data Warehouse trenutno podržava provjeru autentičnosti sustava SQL Server s korisničko ime i lozinku kao Azure Active Directory. 

Pri stvaranju logičke poslužitelj za bazu podataka koju ste naveli "administrator poslužitelja" Prijava pomoću korisničkog imena i lozinke. Koristi te vjerodajnice, koje možete provjeru autentičnosti sve baze podataka na tom poslužitelju kao vlasnik baze podataka ili "vlasnika baze podataka" kroz provjera autentičnosti SQL poslužitelja.

Međutim, kao preporučenim načinom rada vaše tvrtke ili ustanove korisnici trebali biste koristiti neki drugi račun za provjeru autentičnosti. Na taj način možete ograničiti dozvole dodijeljene aplikacije i smanjenje rizika zlonamjerni aktivnosti u slučaju da je podložno napada za unos SQL kodu aplikacije. 

Da biste stvorili korisnik provjerene autentičnosti SQL Server, povezivanje s **glavnom** bazom podataka na poslužitelju s vašu prijavu administrator poslužitelja i stvorite novi poslužitelj za prijavu.  Osim toga, je dobro stvorili korisnika u glavnom bazom podataka za Azure SQL Data Warehouse korisnike. Stvaranje korisnika u matrici korisnicima omogućuje Prijava pomoću alata kao što je SSMS bez navođenja naziv baze podataka.  Omogućuje da koristite explorer objekt da biste prikazali sve baze podataka na SQL server.

```sql
-- Connect to master database and create a login
CREATE LOGIN ApplicationLogin WITH PASSWORD = 'Str0ng_password';
CREATE USER ApplicationUser FOR LOGIN ApplicationLogin;
```

Nakon toga povezivanje s **bazom podataka SQL Data Warehouse** s vašu prijavu administrator poslužitelja i stvaranje baze podataka korisnika koji se temelji na poslužitelj za prijavu koju ste upravo stvorili.

```sql
-- Connect to SQL DW database and create a database user
CREATE USER ApplicationUser FOR LOGIN ApplicationLogin;
```

Ako korisnik će način dodatne operacije kao što su stvaranje prijave ili stvaranje nove baze podataka, oni morat ćete biti dodijeljena na `Loginmanager` i `dbmanager` ulogama u glavnom bazom podataka. Dodatne informacije o sljedećim ulogama additonal i provjera autentičnosti s bazom podataka SQL potražite u članku [Upravljanje bazama podataka i prijave u bazi podataka SQL Azure][].  Dodatne informacije o Azure AD za SQL Data Warehouse potražite u članku [Povezivanje sa SQL podataka skladištu tako da pomoću Azure Active Directory provjeru autentičnosti][].


## <a name="authorization"></a>Autorizacija

Autorizacija se odnosi na što sve možete raditi u bazi podataka programa Azure SQL Data Warehouse, a to upravlja članstva korisnički račun uloge i dozvole. Kao preporučenim načinom rada treba dodijeliti korisnicima najmanje ovlasti potrebne. Azure SQL Data Warehouse taj postupak olakšava da biste upravljali s ulogama u T-SQL:

```sql
EXEC sp_addrolemember 'db_datareader', 'ApplicationUser'; -- allows ApplicationUser to read data
EXEC sp_addrolemember 'db_datawriter', 'ApplicationUser'; -- allows ApplicationUser to write data
```

Računa administratora poslužitelja s kojim se povezujete je član db_owner koja ima izdavanje ništa unutar baze podataka. Spremite ovaj račun za implementaciju nadogradnje sheme i ostalih operacija upravljanja. Koristite račun "ApplicationUser" s više ograničene dozvole za povezivanje iz aplikacije u bazu podataka s najmanje ovlasti potrebne za svoju aplikaciju.

Načina da biste dodatno ograničiti korisnika pruža baze podataka SQL Azure:

- Zrnastog [dozvole][] omogućuju kontrolu operacijama možete učiniti na pojedinačne stupci, tablice, prikaza, postupke, i drugih objekata u bazi podataka. Dozvole za zrnastog omogućuju Većina kontrolirati i dajte minimalne potrebne dozvole. Sustav zrnastog dozvola je Pomalo složene i će potrebni su neke slučaja učinkovito korištenje.
- [Uloge baze podataka][] koji nisu db_datareader i db_datawriter se može koristiti za stvaranje naprednijih aplikacije korisničkih računa ili manje Napredna upravljanje računima. Uloge ugrađene fixed baze podataka nude jednostavan način da biste dodijelili dozvole, ali može uzrokovati dodjelu dozvola za više nego što je potrebno.
- [Spremljene procedure][] može se koristiti za ograničavanje akcije koje možete poduzeti u bazu podataka.

Upravljanje bazama podataka i logički poslužitelje na portalu klasični Azure ili pomoću upravitelja resursa API Azure upravlja dodjele uloga portala korisnički račun. Dodatne informacije o ovoj temi potražite u članku [Upravljanje pristupom utemeljeno na ulogama Azure portalu][].

## <a name="encryption"></a>Šifriranje

Azure SQL podataka skladištu prozirne podataka šifriranja (TDE) pomaže u zaštiti prijetnju zlonamjerni aktivnosti izvođenjem u stvarnom vremenu šifriranje i dešifriranje podataka na ostale.  Kada šifrirali bazu podataka, pridruženi sigurnosne kopije i datoteke zapisnika transakcije šifrirane bez promjene aplikacija. TDE šifrira prostora za pohranu cijelu bazu podataka pomoću ključa simetričnu naziva ključa za šifriranje baze podataka. U bazi podataka SQL ključa za šifriranje baze podataka zaštićen certifikata ugrađene poslužitelja. Potvrda ugrađene poslužitelja nije jedinstvena za svaki baze podataka SQL server. Microsoft automatski zakreće ove potvrde barem svaka 90 dana. Algoritam za šifriranje koristi SQL Data Warehouse je AES 256. Opći opis TDE potražite u članku [Prozirne šifriranje podataka][].

Možete šifrirati bazu podataka pomoću [Portala za Azure] [ Encryption with Portal] ili [T SQL][Encryption with TSQL].

## <a name="next-steps"></a>Daljnji koraci

Detalji i primjeri za povezivanje SQL Data Warehouse s različitim protokola, potražite u članku [Povezivanje sa SQL Data Warehouse][].

<!--Image references-->

<!--Article references-->
[Povezivanje s SQL Data Warehouse]: ./sql-data-warehouse-connect-overview.md
[Encryption with Portal]: ./sql-data-warehouse-encryption-tde.md
[Encryption with TSQL]: ./sql-data-warehouse-encryption-tde-tsql.md
[Povezivanje s SQL Data Warehouse pomoću provjere autentičnosti Azure Active Directory]: ./sql-data-warehouse-authentication.md

<!--MSDN references-->
[Vatrozid za baze podataka SQL Azure]: https://msdn.microsoft.com/library/ee621782.aspx
[sp_set_firewall_rule]: https://msdn.microsoft.com/library/dn270017.aspx
[sp_set_database_firewall_rule]: https://msdn.microsoft.com/library/dn270010.aspx
[Uloge baze podataka]: https://msdn.microsoft.com/library/ms189121.aspx
[Upravljanje bazama podataka i prijave u bazi podataka SQL Azure]: https://msdn.microsoft.com/library/ee336235.aspx
[Dozvole]: https://msdn.microsoft.com/library/ms191291.aspx
[Pohranjene procedure]: https://msdn.microsoft.com/library/ms190782.aspx
[Šifriranje prozirne podataka]: https://msdn.microsoft.com/library/bb934049.aspx
[Azure portal]: https://portal.azure.com/

<!--Other Web references-->
[Kontrola pristupa na temelju uloga portalu Azure]: https://azure.microsoft.com/documentation/articles/role-based-access-control-configure
