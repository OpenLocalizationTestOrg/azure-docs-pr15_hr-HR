<properties
   pageTitle="Provjera autentičnosti za Azure SQL Data Warehouse | Microsoft Azure"
   description="Azure Active Directory (AAD) i SQL Server provjere autentičnosti za Azure SQL Data Warehouse."
   services="sql-data-warehouse"
   documentationCenter=""
   authors="byham"
   manager="barbkess"
   editor=""
   tags=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/24/2016"
   ms.author="rickbyh;barbkess;sonyama"/>

# <a name="authentication-to-azure-sql-data-warehouse"></a>Provjera autentičnosti za Azure SQL Data Warehouse

> [AZURE.SELECTOR]
- [Pregled sigurnosti](sql-data-warehouse-overview-manage-security.md)
- [Provjera autentičnosti](sql-data-warehouse-authentication.md)
- [Šifriranje (Portal)](sql-data-warehouse-encryption-tde.md)
- [Šifriranje (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)

Da biste povezali SQL Data Warehouse, mora proći u sigurnosne vjerodajnice radi provjere autentičnosti. Nakon uspostave veze, određene veze konfigurirane postavke kao dio uspostavljanje sesiju upita.  

Dodatne informacije o sigurnosti i omogućivanje veze s skladištu podataka potražite u članku [sigurne baze podataka u SQL Data Warehouse][].

## <a name="sql-authentication"></a>SQL provjera autentičnosti
Da biste povezali SQL Data Warehouse, navedite sljedeće podatke:

- Potpuno kvalificiran naziv poslužitelja
- Odredite SQL provjera autentičnosti
- Korisničko ime
- Lozinke
- Zadanu bazu podataka (neobavezno)

Prema zadanim postavkama veza se povezuje s *glavnom* bazom podataka, a ne korisnika bazu podataka. Povezivanje baze podataka za korisnika, možete odabrati učinite nešto od sljedećeg:

- Kada se registrirate poslužitelj za SQL Server objekt Explorer SSDT, SSMS, ili u nizu za povezivanje aplikacije, navesti zadanu bazu podataka. Na primjer, uključiti InitialCatalog parametar za ODBC vezu.
- Prije stvaranja sesije u SSDT istaknite baze podataka za korisnika.

> [AZURE.NOTE] Transact-SQL naredbe **Koristi MyDatabase;** nije podržana za promjenu baze podataka za vezu. Povezivanje s SQL Data Warehouse s SSDT upute, potražite u članku [upit s Visual Studio][] .

## <a name="azure-active-directory-aad-authentication"></a>Provjera autentičnosti za Azure Active Directory (AAD)

[Azure Active Directory] [ What is Azure Active Directory] provjera autentičnosti je mehanizam povezivanja Microsoft Azure SQL Data Warehouse pomoću identiteta servisa Azure Active Directory (Azure AD). S provjerom autentičnosti Azure Active Directory, možete središnje Upravljanje identitetima korisnika baze podataka i druge Microsoftove servise na jednom središnjem mjestu. Središnje upravljanje ID-a nudi jedno mjesto za upravljanje korisnicima SQL Data Warehouse i pojednostavljuje upravljanja dozvolama. 

### <a name="benefits"></a>Prednosti

Azure Active Directory pogodnosti obuhvaćaju sljedeće:

- Alternativa provjeru autentičnosti sustava SQL Server.
- Pridonosi zaustavite proliferation identitetima korisnika na poslužiteljima baze podataka.
- Omogućuje zakretanje lozinku na jednom mjestu
- Upravljanje dozvolama za baze podataka pomoću vanjske grupe (AAD).
- Uklanja pohranu lozinki tako da omogućite integriranu provjeru autentičnosti sustava Windows i druge oblike provjere autentičnosti koje podržava Azure Active Directory.
- Koristi sadržavao korisnici baze podataka za provjeru identiteta na razini baze podataka.
- Podržava provjeru autentičnosti utemeljenu na token za aplikacije za povezivanje s SQL Data Warehouse.
- Podržava višestruka provjera autentičnosti putem aktivni univerzalni provjere autentičnosti direktorija za SQL Server Management Studio. Opis višestruka provjera autentičnosti, potražite u članku [SSMS podrške za Azure AD MFA s bazom podataka SQL i SQL Data Warehouse](../sql-database/sql-database-ssms-mfa-authentication.md).

> [AZURE.NOTE] Azure Active Directory je i dalje relativno novo i ima određena ograničenja. Da biste bili sigurni da Azure Active Directory dobro rješenje za vaše okruženje, pročitajte članak [Azure AD značajke i ograničenja][], posebno Dodatne napomene.

### <a name="configuration-steps"></a>Navedeni koraci za konfiguraciju

Slijedite ove korake da biste konfigurirali provjere autentičnosti Azure Active Directory.

1. Stvaranje i popunjavanje Azure Active Directory
2. Neobavezno: Pridružiti ili promijeniti koji je trenutno povezan s pretplatom Azure active directory
3. Stvaranje administrator Azure Active Directory za Azure SQL Data Warehouse.
4. Konfiguriranje klijentskim računalima
5. Stvaranje korisnika sadrži baze podataka u bazi podataka mapirani identitetima Azure AD
6. Povezivanje s skladištu podataka pomoću identiteta Azure AD

Trenutno Azure Active Directory korisnika ne prikazuju u programu Explorer SSDT objekta. Kao zaobilazno rješenje, pogledati korisnike [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx).
  
### <a name="find-the-details"></a>Pronalaženje detalje
- Dovršite detaljne upute. Detaljne upute za konfiguriranje i korištenje provjere autentičnosti Azure Active Directory jednaki gotovo za baze podataka SQL Azure i Azure SQL Data Warehouse. Slijedite detaljne upute u članku [Povezivanje sa SQL baze podataka ili SQL podataka skladištu tako da pomoću Azure Active Directory provjeru autentičnosti](../sql-database/sql-database-aad-authentication.md).
- Stvorite prilagođene baze podataka uloge i dodavanje korisnika u uloge. Zatim Dodjela zrnastog dozvola za uloge. Dodatne informacije potražite u članku [Početak rada s dozvolama za modul baze podataka](https://msdn.microsoft.com/library/mt667986.aspx).

## <a name="next-steps"></a>Daljnji koraci

Da biste započeli ispitivanje skladištu podataka s Visual Studio i drugim aplikacijama, potražite u članku [upit s Visual Studio][].

<!-- Article references -->
[Zaštita baze podataka u skladištu podataka za SQL]: ./sql-data-warehouse-overview-manage-security.md
[Upit s Visual Studio]: ./sql-data-warehouse-query-visual-studio.md
[What is Azure Active Directory]: ../active-directory/active-directory-whatis.md
[Azure AD značajke i ograničenja]: ../sql-database/sql-database-aad-authentication.md#azure-ad-features-and-limitations
