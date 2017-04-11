<properties
   pageTitle="Upute o sigurnosti baze podataka Azure SQL i ograničenja | Microsoft Azure"
   description="Informirajte se o baza podataka Microsoft Azure SQL smjernice i ograničenja vezane uz sigurnost."
   services="sql-database"
   documentationCenter=""
   authors="BYHAM"
   manager="jhubbard"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="10/18/2016"
   ms.author="rickbyh"/>

# <a name="azure-sql-database-security-guidelines-and-limitations"></a>Upute o sigurnosti baze podataka SQL Azure i ograničenja

U ovoj se temi opisuju baza podataka Microsoft Azure SQL smjernice i ograničenja vezane uz sigurnost. Imajte na umu sljedeće kada upravljanje zaštitom vaše baze podataka SQL Azure.

## <a name="access-to-the-virtual-master-database"></a>Access virtualne glavnom bazom podataka

Obično samo administratori mora imati pristup glavnom bazom podataka. Redovno pristup bazi podataka svaki korisnik mora biti kroz koji nisu administratori sadržavao korisnici baze podataka stvorene u svaku bazu podataka. Kada koristite korisnika sadrži baze podataka, ne morate stvoriti prijave u glavnom bazom podataka. Dodatne informacije potražite u članku [Nalazi korisnici baze podataka – upućivanje vaše baze podataka prijenosni](https://msdn.microsoft.com/library/ff929188.aspx).


## <a name="firewall"></a>Vatrozid

Vatrozid za SQL Server koji je namijenjene cijelu SQL Server Azure obično je konfiguriran putem portala sustava i trebali biste samo dopuštanje IP adresa koji se koriste administratori. Da biste stvorili pravilo baze podataka u dosegu vatrozid koji će se otvoriti potrebne raspon IP adresa za svaku bazu podataka, povezivanje s bazom podataka korisnika, a zatim pomoću [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) Transact-SQL naredbu.

Servis baze podataka SQL Azure dostupna samo putem TCP priključka 1433. Da biste pristupili bazi podataka sustava SQL s računala, provjerite dopušta li vatrozid klijentskog računala izlaznu komunikaciju TCP na TCP priključak 1433. Ako nije potreban za druge aplikacije, blokirati unutarnje veze na TCP priključka 1433. 

U sklopu postupka veze, veze iz Azure virtualnim strojevima vas preusmjerava u drugu IP adresu i priključak, jedinstvene za svaku ulogu suradnika. Broj priključka je u rasponu od 11000 do 11999. Dodatne informacije o TCP priključci potražite u članku [priključci izvan 1433 za ADO.NET 4,5 i V12 SQL baze podataka](sql-database-develop-direct-route-ports-adonet-v12.md).


## <a name="authentication"></a>Provjera autentičnosti

Active Directory pomoću provjere autentičnosti (integrirani sigurnost) kad god je moguće. Informacije o konfiguriranju provjere autentičnosti AD potražite u članku [Povezivanje sa SQL tako da pomoću Azure Active Directory provjera autentičnosti baze podataka](sql-database-aad-authentication.md)i [odabiru načina provjere autentičnosti](https://msdn.microsoft.com/library/ms144284.aspx) u SQL Server knjige na mreži. 

Korisnici sadržavao baze podataka možete koristiti da biste poboljšali skalabilnost. Dodatne informacije potražite u članku [Nalazi baze podataka korisnika – upućivanje vaše baze podataka prijenosni](https://msdn.microsoft.com/library/ff929188.aspx), [CREATE USER (Transact-SQL)](https://technet.microsoft.com/library/ms173463.aspx)i [Koje se nalaze baze podataka](https://technet.microsoft.com/library/ff929071.aspx).

Modul za baze podataka zatvara veze koje ostaju neaktivan tijekom više od 30 minuta. Veza ponovno morate prijave da se može koristiti.

Neprestano aktivna veza s bazom podataka SQL zahtijevaju reauthorization (obavlja modul baze podataka) barem svaka 10 sati. Modul za baze podataka pokušava reauthorization pomoću izvorno poslane lozinke i potreban je bez korisničkom unosu. Radi boljih performansi kada je za ponovno postavljanje lozinke u bazi podataka SQL veza nije moguće reauthenticated, čak i ako se zbog okupljanje je ponovno postaviti vezu. Time se razlikuje od ponašanja lokalnog sustava SQL Server. Ako lozinku promijenjen je veza na početku dozvolu, morate prekinuti vezu, a nova veza stvoren pomoću nove lozinke. Korisnik s dozvolama UKLONI VEZU s bazom podataka možete prekinuti vezu s bazom podataka SQL izričito pomoću naredbe [UKLONI](https://msdn.microsoft.com/library/ms173730.aspx) .

## <a name="logins-and-users"></a>Prijave i korisnika

Kada upravljanje prijave i korisnika u SQL baze podataka, postoje ograničenja.


- Morate biti povezani s **glavnom** bazom podataka prilikom izvršavanja u ``CREATE/ALTER/DROP DATABASE`` izvješća. Korisnik-baze podataka u glavnom bazom podataka koja odgovara razini poslužitelja glavni prijavu ne smiju mijenjati ili ispušteno. 
- SAD engleski je zadani jezik za prijavu glavni razini poslužitelja.
- Samo administratori (glavnica prijava razini poslužitelja ili administrator za Azure AD) i Članovi uloge **dbmanager** baze podataka u **glavnom** bazom podataka imati dozvolu za izvršavanje na ``CREATE DATABASE`` i ``DROP DATABASE`` izvješća.
- Morate biti povezani s glavnom bazom podataka prilikom izvršavanja u ``CREATE/ALTER/DROP LOGIN`` izvješća. No pomoću prijave je discouraged. Umjesto toga koristite korisnika sadrži baze podataka.
- Da biste se povezali s bazom podataka korisnika, navedite naziv baze podataka u nizu za povezivanje.
- Samo glavni prijava u razini poslužitelja i Članovi uloge **loginmanager** baze podataka u **glavnom** bazom podataka imati dozvolu za izvršavanje na ``CREATE LOGIN``, ``ALTER LOGIN``, i ``DROP LOGIN`` izvješća.
- Prilikom izvršavanja u ``CREATE/ALTER/DROP LOGIN`` i ``CREATE/ALTER/DROP DATABASE`` izvješća u aplikaciji ADO.NET pomoću naredbi s parametrima nije dopušteno. Dodatne informacije potražite u članku [naredbe i parametre](https://msdn.microsoft.com/library/ms254953.aspx).
- Prilikom izvršavanja u ``CREATE/ALTER/DROP DATABASE`` i ``CREATE/ALTER/DROP LOGIN`` izjave svaki od ovih izraza mora biti samo naredbi u Transact-SQL grupe. U suprotnom, javlja se pogreška. Na primjer, sljedeće Transact-SQL provjerava postoji li baza podataka. Ako postoji, na ``DROP DATABASE`` naziva da biste uklonili bazu podataka. Budući da u ``DROP DATABASE`` izjava nije samo naredbi u grupu, izvršavanja sljedećeg Transact-SQL naredbe rezultira pogreškom.

```
IF EXISTS (SELECT [name]
           FROM   [sys].[databases]
           WHERE  [name] = N'database_name')
     DROP DATABASE [database_name];
GO
```

- Prilikom izvršavanja u ``CREATE USER`` izvoda s na ``FOR/FROM LOGIN`` mogućnost mora biti samo naredbi u Transact-SQL grupe.
- Prilikom izvršavanja u ``ALTER USER`` izvoda s na ``WITH LOGIN`` mogućnost mora biti samo naredbi u Transact-SQL grupe.
- Da biste ``CREATE/ALTER/DROP`` potreban je korisnik na ``ALTER ANY USER`` dozvola bazu podataka.
- Kad vlasnika baze podataka uloga pokuša da biste dodali ili uklonili neki drugi korisnik baze podataka ili iz uloge baze podataka, može pojaviti sljedeća pogreška: **korisnika ili uloga "Naziv" ne postoji u toj bazi podataka.** Ta se pogreška pojavljuje jer se korisnik nije vidljiv vlasnik. Da biste riješili taj problem, dodijeliti ulogu vlasnika u ``VIEW DEFINITION`` dozvola na korisnike. 

Dodatne informacije o prijave i korisnicima potražite u članku [Upravljanje bazama podataka i prijave u bazi podataka SQL Azure](sql-database-manage-logins.md).


## <a name="see-also"></a>Vidi također

[Vatrozid za baze podataka Azure SQL](sql-database-firewall-configure.md)

[Kako: konfigurirati postavke vatrozida (baze podataka Azure SQL)](sql-database-configure-firewall-settings.md)

[Upravljanje bazama podataka i prijave u bazi podataka SQL Azure](sql-database-manage-logins.md)

[Centar za sigurnost za modul za baze podataka SQL Server i baze podataka Azure SQL](https://msdn.microsoft.com/library/bb510589)
