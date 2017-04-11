<properties
   pageTitle="SSMS podrške za Azure AD MFA s bazom podataka SQL i SQL Data Warehouse | Microsoft Azure"
   description="Korištenje više Factored provjere autentičnosti uz SSMS za SQL baze podataka i SQL Data Warehouse."
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
   ms.date="10/04/2016"
   ms.author="rick.byham@microsoft.com"/>

# <a name="ssms-support-for-azure-ad-mfa-with-sql-database-and-sql-data-warehouse"></a>SSMS podrška za Azure AD MFA s bazom podataka SQL i SQL Data Warehouse

Azure SQL baze podataka i Azure SQL Data Warehouse sada podržava veze iz sustava SQL Server Management Studio (SSMS) pomoću *Active Directory univerzalni provjere autentičnosti u*. Aktivni univerzalni provjera autentičnosti direktorija je u tijeku rada za interaktivno koji podržava *Azure višestruke provjere autentičnosti* (MFA). Azure MFA olakšava zaštitu pristup s podacima i aplikacijama dok korisnik zahtjev za jednostavne postupak prijave za sastanak. On nudi Jaka provjera autentičnosti s raspon mogućnosti jednostavno provjere – telefonski poziv, tekstne poruke, a zatim pametne kartice s PIN-a ili obavijesti za mobilne uređaje – dopuštanje korisnicima da biste odabrali način radije. Opis višestruka provjera autentičnosti, potražite u članku [Višestruke provjere autentičnosti](../multi-factor-authentication/multi-factor-authentication.md).

SSMS sada podržava:

- Interaktivni MFA s Azure AD uz mogućnost provjere valjanosti skočni dijaloški okvir.
- Koji nisu interaktivni Active Directory lozinku i Active Directory integrirane provjere metode koje se mogu koristiti u mnogo različitih aplikacija (ADO.NET JDBC, ODBC, itd.). Ta dva načina nikad rezultirati skočni dijaloške okvire.

Kada je račun konfiguriran za MFA tijek rada interaktivne provjere autentičnosti zahtijeva dodatne korisničke interakcije kroz skočni dijaloške okvire, pomoću pametne kartice i Dr. Kada je račun konfiguriran za MFA, korisnik mora odaberite Azure univerzalni provjere autentičnosti za povezivanje. Ako korisnički račun ne zahtijeva MFA, korisnik i dalje možete koristiti druge dviju provjere autentičnosti Azure Active Directory mogućnosti.

## <a name="universal-authentication-limitations-for-sql-database-and-sql-data-warehouse"></a>Univerzalni ograničenja provjere autentičnosti za SQL baze podataka i SQL Data Warehouse

- SSMS je alat za samo trenutno je omogućen za MFA kroz aktivni univerzalni provjere autentičnosti direktorija.
- Prijavite se u samo jedan račun Azure Active Directory možete za instance komponente SSMS pomoću univerzalni provjere autentičnosti. Da biste se prijavili kao neki drugi račun za Azure AD, morate koristiti drugu instancu programa SSMS. (Ovo ograničenje ograničeno je na aktivni univerzalni provjere autentičnosti direktorija; možete se prijaviti na različitim poslužiteljima pomoću provjere autentičnosti lozinke Active Directory, Active Directory integrirane provjere ili SQL Server provjeru autentičnosti).
- SSMS podržava Active univerzalni provjere autentičnosti direktorija za vizualizaciju Explorer objekta, uređivača upita i spremište upita.
- DacFx ni dizajner sheme podržava univerzalni provjeru autentičnosti.
- MSA računa za Active Directory univerzalni provjeru nisu podržani.
- Aktivni univerzalni provjere autentičnosti direktorija nije podržan u SSMS za korisnike koji su uvezeni u trenutnom Active Directory iz imenika Active druge Azure. Ti korisnici nisu podržane, jer ga je potrebna ID klijenta za provjeru valjanosti računa, a postoji bez mehanizam koji omogućuje.
- Ne postoje dodatni softver pravila za Active Directory univerzalni ovjeru osim što morate koristiti podržanu verziju programa SSMS.

## <a name="configuration-steps"></a>Navedeni koraci za konfiguraciju

Implementacijom višestruka provjera autentičnosti zahtijeva četiri osnovne korake.

1. **Konfiguriranje Azure Active Directory** – dodatne informacije potražite u članku [integraciji vaših lokalnih identiteta sa servisu Azure Active Directory](../active-directory/active-directory-aadconnect.md), [dodajte vlastiti naziv domene za Azure AD](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), [Microsoft Azure sada podržava vanjski pristup u sustavu Windows Server Active Directory](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), [administriranje Azure AD direktorija](https://msdn.microsoft.com/library/azure/hh967611.aspx)i [Upravljanje Azure AD pomoću komponente Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx).

2. **Konfiguriranje MFA** – detaljne upute potražite u članku [Konfiguriranje Azure višestruka provjera autentičnosti](../multi-factor-authentication/multi-factor-authentication-whats-next.md). 

3. **Konfiguriranje SQL baze podataka ili SQL Data Warehouse za provjeru autentičnosti za Azure AD** – detaljne upute potražite u članku [Povezivanje baze podataka SQL ili SQL podataka skladištu tako da pomoću Azure Active Directory provjeru autentičnosti](sql-database-aad-authentication.md).

4. **Preuzimanje SSMS** – na klijentskom računalu, preuzmite najnoviju SSMS (najmanje kolovoz 2016), iz [Preuzimanje SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx).

## <a name="connecting-by-using-universal-authentication-with-ssms"></a>Povezivanje pomoću univerzalni provjere autentičnosti SSMS

Sljedeći koraci pokazati kako pomoću najnovije SSMS povežite s bazom podataka SQL ili SQL Data Warehouse.

1. Da biste se povezali pomoću univerzalni provjeru autentičnosti, u dijaloškom okviru za **Povezivanje s poslužiteljem** , odaberite **Aktivni univerzalni provjere autentičnosti direktorija**.
![1mfa-univerzalno-povezivanje][1]

2. Kao i obično za SQL baze podataka i SQL Data Warehouse kliknite **Mogućnosti** i odredite bazu podataka u dijaloškom okviru **Mogućnosti** . Zatim kliknite **Poveži**.
3. Kada se pojavi dijaloški okvir **prijavite se na svoj račun** , navedite račun i lozinku za vaš identitet Azure Active Directory.
![2mfa – prijava][2]

    > [AZURE.NOTE] Za univerzalni provjera autentičnosti pomoću računa koji zahtijevaju MFA povežete sada. Za korisnike koji zahtijevaju MFA, nastavite sa sljedećim koracima.
 
4. Može prikazati dvije MFA postavljanje dijaloške okvire. To operacija ovisi o administratoru MFA postavku i stoga možda nije obavezno. Za domenu MFA omogućeno ovaj korak katkad je unaprijed definiranih (Ako, na primjer, domenu zahtijeva korisnicima pomoću pametne kartice i PIN-a).  
![3mfa postavljanje][3]

5. Drugi mogućih jednom dijaloški okvir omogućuje odabir detalja načina provjere autentičnosti. Mogući mogućnosti konfigurirana je administrator.
![4mfa-potvrda-1][4]
 
6. Azure Active Directory šalje potvrda informacije o vama. Kada primite kod za potvrdu, unesite u okvir **kod za potvrdu Enter** , a zatim kliknite **Prijava**.
![5mfa-potvrda-2][5]

Po dovršetku provjere SSMS povezuje obično presuming valjanih se vjerodajnica i vatrozid.

##<a name="next-steps"></a>Daljnji koraci  

Drugim korisnicima dopustiti pristup bazi podataka: [Provjera autentičnosti baze podataka SQL i autorizacije: dodjelu pristupa](sql-database-manage-logins.md)  
Provjerite je li drugi možete se povezati s kroz Vatrozid: [Konfiguriranje pravilo vatrozid razini poslužitelja baze podataka SQL Azure pomoću portala za Azure](sql-database-configure-firewall-settings.md)


[1]: ./media/sql-database-ssms-mfa-auth/1mfa-universal-connect.png
[2]: ./media/sql-database-ssms-mfa-auth/2mfa-sign-in.png
[3]: ./media/sql-database-ssms-mfa-auth/3mfa-setup.png
[4]: ./media/sql-database-ssms-mfa-auth/4mfa-verify-1.png
[5]: ./media/sql-database-ssms-mfa-auth/5mfa-verify-2.png

