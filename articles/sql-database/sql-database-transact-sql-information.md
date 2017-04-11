<properties
   pageTitle="Nije podržana u T-SQL baze podataka Azure SQL | Microsoft Azure"
   description="Transakcija SQL naredbe koje manje od u potpunosti podržane u bazi podataka SQL Azure"
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
   ms.date="08/30/2016"
   ms.author="rick.byham@microsoft.com"/>

# <a name="azure-sql-database-transact-sql-differences"></a>Razlike u Transact-SQL baze podataka SQL Azure


Većina značajki Transact-SQL koje ovise o tome aplikacija podržava Microsoft SQL Server i baze podataka SQL Azure. Slijedi popis djelomično podržane značajke za aplikacije:

- Vrste podataka.
- Operatori.
- Funkcije niza, logičke, aritmetičku i pokazivač.

Međutim, baze podataka SQL Azure namijenjen je izdvajanja značajke iz bilo kojeg ovisnosti na **glavnom** bazom podataka. Sljedeći consequence mnogo aktivnosti razini poslužitelja su prikladna za SQL baze podataka i nisu podržani. Značajke koje su izostavljen je iz sustava SQL Server obično nisu podržane u SQL baze podataka.

> [AZURE.NOTE]
> U ovoj se temi govori o značajkama koje su dostupne u SQL baze podataka kada se nadograditi na najnoviju verziju; V12 baze podataka SQL. Dodatne informacije o V12 potražite u članku [SQL baza podataka V12 što je novo](sql-database-v12-whats-new.md).

U sljedećim odjeljcima navedeni značajke koje su djelomično podržani i značajkama koje su potpuno podržane.


## <a name="features-partially-supported-in-sql-database-v12"></a>Značajke koje se djelomično podržane u V12 baze podataka SQL

V12 baze podataka SQL ne podržava neke, ali ne i svih argumenata koji postoje u odgovarajuću SQL Server 2016 Transact-SQL naredbe. Na primjer, naredba Create PROCEDURE dostupna Međutim mogućnosti CREATE PROCEDURE nisu dostupne. Pogledajte teme povezane sintaksa detalje o podržanim područja svaku izjavu o.

- Baza podataka: [Stvaranje](https://msdn.microsoft.com/library/dn268335.aspx )/[ALTER](https://msdn.microsoft.com/library/ms174269.aspx)
- DMVs obično dostupni su za značajke koje su dostupne.
- Funkcija: [Stvaranje](https://msdn.microsoft.com/library/ms186755.aspx)/[ALTER (funkcija)](https://msdn.microsoft.com/library/ms186967.aspx)
- [UKLANJANJE](https://msdn.microsoft.com/library/ms173730.aspx) 
- Prijave: [Stvaranje](https://msdn.microsoft.com/library/ms189751.aspx)/[ALTER prijava](https://msdn.microsoft.com/library/ms189828.aspx)
- Pohranjene procedure: [Stvaranje](https://msdn.microsoft.com/library/ms187926.aspx)/[ALTER postupak](https://msdn.microsoft.com/library/ms189762.aspx)
- Tablica: [Stvaranje](https://msdn.microsoft.com/library/dn305849.aspx)/[ALTER](https://msdn.microsoft.com/library/ms190273.aspx)
- Vrste (Prilagođeno): [Stvaranje vrste](https://msdn.microsoft.com/library/ms175007.aspx)
- Korisnici: [Stvaranje](https://msdn.microsoft.com/library/ms173463.aspx)/[ALTER korisnika](https://msdn.microsoft.com/library/ms176060.aspx)
- Prikazi: [Stvaranje](https://msdn.microsoft.com/library/ms187956.aspx)/[IZMIJENI PRIKAZ](https://msdn.microsoft.com/library/ms173846.aspx)

## <a name="features-not-supported-in-sql-database"></a>Značajke nisu podržane u SQL baze podataka

- Razvrstavanje sustava objekata
- Veza odnosi: ORIGINAL_DB_NAME naredbe krajnjoj točki. SQL baze podataka ne podržava provjeru autentičnosti sustava Windows, ali ne podržava provjeru autentičnosti za slične Azure Active Directory. Neke vrste provjere autentičnosti zahtijevaju najnoviju verziju SSMS. Dodatne informacije potražite u članku [Povezivanje sa SQL baze podataka ili SQL podataka skladištu tako da pomoću Azure Active Directory provjeru autentičnosti](sql-database-aad-authentication.md).
- Unakrsni upiti baze podataka pomoću tri ili četiri dijela naziva. (Samo za čitanje izdvojiti bazu podataka upita podržani pomoću [upita elastic baze podataka](sql-database-elastic-query-overview.md))
- Ulančavanje vlasništvo unakrsni baze podataka, POUZDANOG postavka
- Prikupljanje podataka
- Dijagrami baze podataka
- Pošta baze podataka
- DATABASEPROPERTY (umjesto toga koristite DATABASEPROPERTYEX)
- IZVRŠAVANJE kao prijave
- Šifriranja: extensible upravljanja ključem
- Eventing: događaje, obavijesti o događaju, obavijesti upita
- Značajke vezane uz položaj datoteke baze podataka, veličina i datoteke baze podataka koja se automatski upravlja Microsoft Azure.
- Značajke koje se odnose na visoke dostupnosti koji upravlja se putem računa za Microsoft Azure: sigurnosne kopije, vraćanje AlwaysOn baze podataka, zapisnika dostavu, Načini rada za oporavak. Dodatne informacije potražite u članku Azure sigurnosnu kopiju baze podataka SQL i vraćanja.
- Značajke koje ovise o čitač zapisnika sustavom SQL baze podataka: automatske replikacije, snimiti za promjenu podataka.
- Značajke koje ovise o agenta za SQL Server ili MSDB baze podataka: zadacima, upozorenja, operatori, utemeljen na pravila upravljanja, pošte baze podataka, a zatim središnje upravljanje poslužiteljima.
- SVOJSTVO FILESTREAM
- Funkcija: fn_get_sql fn_virtualfilestats, fn_virtualservernodes
- Globalni privremenih tablica
- Postavke vezane uz hardver poslužitelja: memorije radnih niti, afiniteta procesora, praćenje zastavice, itd. Umjesto toga koristite razine usluge.
- HAS_DBACCESS
- UKLONI STAT POSLA
- Povezani poslužitelji, OPENQUERY, OPENROWSET, OPENDATASOURCE, MASOVNO umetanje i od četiri dijela imena
- Matrica/ciljnim poslužiteljima
- .NET framework [CLR Integracija sa sustavom SQL Server](http://msdn.microsoft.com/library/ms254963.aspx)
- Governor resursa
- Semantičkog pretraživanja
- Vjerodajnice za poslužitelj. Baza podataka umjesto ograničena vjerodajnice.
- Stavke Sever razini: sys.login_token IS_SRVROLEMEMBER, uloge poslužitelja. Razine dozvola za poslužitelj nisu dostupne iako neke zamjenjuju dozvole na razini baze podataka. Neke DMVs razini poslužitelja nisu dostupne iako neke zamjenjuju DMVs razinom baze podataka.
- Serverless express: localdb, instance korisnika
- Broker servisa sustava
- POSTAVLJANJE REMOTE_PROC_TRANSACTIONS
- ISKLJUČIVANJE
- sp_addmessage
- Mogućnosti sp_configure i KONFIGURIRAJTE. Neke mogućnosti dostupnih [ALTER](https://msdn.microsoft.com/library/mt629158.aspx)konfiguraciju iz DJELOKRUGA baze podataka.
- sp_helpuser
- sp_migrate_user_to_contained
- SQL Server nadzora. Baze podataka SQL nadzor umjesto toga koristite.
- SQL Server Profiler
- Praćenje sustava SQL Server
- Praćenje oznake. Neke stavke zastavicom za praćenje premještene Načini kompatibilnosti.
- SQL transakcija ispravljanje pogrešaka
- Okidača: Iz djelokruga poslužitelja ili okidača za prijavu
- KORIŠTENJE naredbe: da biste promijenili kontekst baze podataka u drugu bazu podataka, morate stvoriti novu vezu u novu bazu podataka.


## <a name="full-transact-sql-reference"></a>Punu referencu Transact-SQL

Dodatne informacije o Transact-SQL gramatike, korištenje i primjere potražite u članku [Referenca Transact-SQL (modul baze podataka)](https://msdn.microsoft.com/library/bb510741.aspx) u SQL Server knjige na mreži. 

### <a name="about-the-applies-to-tags"></a>O oznake "Odnosi se na"

Referenca za Transact-SQL sadrži teme vezane uz verzija sustava SQL Server 2008 prisutnosti. Ispod naslova tema ima je ikona traci popis četiri platforme SQL Server, a koji označava primjenjivošću kako bi. Na primjer, grupe dostupnosti uvedene u sustavu SQL Server 2012. U temi [Stvori GRUPU AVAILABILTY](https://msdn.microsoft.com/library/ff878399.aspx) pokazuje naredbu pripada ** SQL Server (počevši od 2012). Iskaz ne primjenjuju se na SQL Server 2008, SQL Server 2008 R2, baze podataka SQL Azure, Azure SQL Data Warehouse ili Parallel Data Warehouse.

U nekim slučajevima Općenito predmet teme koje se mogu koristiti u proizvod, ali postoje pomoćna razlike između proizvoda. Razlike označene su pri središnjim točkama u temi po potrebi.

