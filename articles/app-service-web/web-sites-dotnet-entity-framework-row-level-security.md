<properties
    pageTitle="Praktični vodič: Web app s više klijentske baze podataka pomoću Framework entitet i sigurnost na razini retka"
    description="Saznajte kako razviti ASP.NET MVC 5 web app s više klijentske baze podataka SQL backent, pomoću Framework entitet i sigurnost na razini retka."
  metaKeywords="azure asp.net mvc entity framework multi tenant row level security rls sql database"
    services="app-service\web"
    documentationCenter=".net"
    manager="jeffreyg"
  authors="tmullaney"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="04/25/2016"
    ms.author="thmullan"/>

# <a name="tutorial-web-app-with-a-multi-tenant-database-using-entity-framework-and-row-level-security"></a>Praktični vodič: Web app s više klijentske baze podataka pomoću Framework entitet i sigurnost na razini retka

Pomoću ovog praktičnog vodiča pokazuje kako stvoriti aplikaciju za web više klijenta s "[zajedničkom bazom podataka, zajedničke sheme](https://msdn.microsoft.com/library/aa479086.aspx)" tenancy modelom, pomoću Framework entitet i [Sigurnost na razini retka](https://msdn.microsoft.com/library/dn765131.aspx). U ovaj model jedan baza podataka sadrži podatke za više klijenata i svaki redak u svaku tablicu je pridružen na "klijent ID-a." Redak razinu sigurnosti (RLS), nova značajka za baze podataka SQL Azure, koristi se za zabranili pristup tuđe podataka drugih korisnika. Potreban je samo jedne male promjenu aplikacije. Središnje logike pristup klijentu unutar same baze podataka, RLS pojednostavljuje kod aplikacije i smanjuje rizik od oštećenja slučajne podataka između klijenata.

Započnimo s jednostavne aplikacije Contact Manager iz [Stvaranje ASP.NET MVP aplikacije pomoću provjere autentičnosti i SQL DB i implementacija aplikacije servisa za Azure](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md). Desno sada aplikacija omogućuje sve korisnike (klijenti) da biste vidjeli sve kontakte:

![Aplikacija Contact Manager prije omogućivanja RLS](./media/web-sites-dotnet-entity-framework-row-level-security/ContactManagerApp-Before.png)

Uz samo nekoliko male promjene, dodat ćemo podršku za više tenancy da bi korisnici mogli da biste vidjeli samo kontakte koji pripadaju ih.

## <a name="step-1-add-an-interceptor-class-in-the-application-to-set-the-sessioncontext"></a>Korak 1: Dodavanje programa klase Interceptor u aplikaciju za postavljanje na SESSION_CONTEXT

Postoji jedan aplikacije promjena moramo da biste. Budući da svi korisnici aplikacije povezali s bazom podataka pomoću isti niz za povezivanje (odnosno isti prijava SQL), trenutno ne postoji način za pravilo RLS znati koji je korisnik mora filtriranju. Taj se način je vrlo uobičajeni u web-aplikacijama jer omogućuje učinkovito red čekanja povezivanja, ali znači moramo drugi način za prepoznavanje trenutnog korisnika aplikacije u bazi podataka. Rješenje je imati aplikaciju koja je postavljena par ključa vrijednosti za trenutni ID korisnika u [SESSION_CONTEXT](https://msdn.microsoft.com/library/mt590806) odmah nakon otvaranja veze, prije nego što je izvršava eventualne upite. SESSION_CONTEXT je u trgovini ključa vrijednosti iz djelokruga sesije i korisnički ID pohranjeni u njemu naš pravila RLS će se koristiti za identificiranje trenutnog korisnika.

Dodat ćemo [interceptor](https://msdn.microsoft.com/data/dn469464.aspx) (posebice u [DbConnectionInterceptor](https://msdn.microsoft.com/library/system.data.entity.infrastructure.interception.idbconnectioninterceptor)), nova značajka u entitet Framework (EF) 6, da biste automatski postavili trenutni ID korisnika u SESSION_CONTEXT izvršavanjem T SQL naredbi kad god EF otvara vezu.

1.  Otvaranje projekta ContactManager u Visual Studio.
2.  Desnom tipkom miša kliknite mapu modela u pregledniku rješenja, a zatim odaberite Dodaj > predmet.
3.  Naziv nove klasa "SessionContextInterceptor.cs", a zatim kliknite Dodaj.
4.  Zamijenite sadržaj SessionContextInterceptor.cs sljedeći kod.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Data.Common;
using System.Data.Entity;
using System.Data.Entity.Infrastructure.Interception;
using Microsoft.AspNet.Identity;

namespace ContactManager.Models
{
    public class SessionContextInterceptor : IDbConnectionInterceptor
    {
        public void Opened(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
            // Set SESSION_CONTEXT to current UserId whenever EF opens a connection
            try
            {
                var userId = System.Web.HttpContext.Current.User.Identity.GetUserId();
                if (userId != null)
                {
                    DbCommand cmd = connection.CreateCommand();
                    cmd.CommandText = "EXEC sp_set_session_context @key=N'UserId', @value=@UserId";
                    DbParameter param = cmd.CreateParameter();
                    param.ParameterName = "@UserId";
                    param.Value = userId;
                    cmd.Parameters.Add(param);
                    cmd.ExecuteNonQuery();
                }
            }
            catch (System.NullReferenceException)
            {
                // If no user is logged in, leave SESSION_CONTEXT null (all rows will be filtered)
            }
        }
        
        public void Opening(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void BeganTransaction(DbConnection connection, BeginTransactionInterceptionContext interceptionContext)
        {
        }

        public void BeginningTransaction(DbConnection connection, BeginTransactionInterceptionContext interceptionContext)
        {
        }

        public void Closed(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void Closing(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void ConnectionStringGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionStringGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionStringSet(DbConnection connection, DbConnectionPropertyInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionStringSetting(DbConnection connection, DbConnectionPropertyInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionTimeoutGetting(DbConnection connection, DbConnectionInterceptionContext<int> interceptionContext)
        {
        }

        public void ConnectionTimeoutGot(DbConnection connection, DbConnectionInterceptionContext<int> interceptionContext)
        {
        }

        public void DataSourceGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void DataSourceGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void DatabaseGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void DatabaseGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void Disposed(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void Disposing(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void EnlistedTransaction(DbConnection connection, EnlistTransactionInterceptionContext interceptionContext)
        {
        }

        public void EnlistingTransaction(DbConnection connection, EnlistTransactionInterceptionContext interceptionContext)
        {
        }

        public void ServerVersionGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void ServerVersionGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void StateGetting(DbConnection connection, DbConnectionInterceptionContext<System.Data.ConnectionState> interceptionContext)
        {
        }

        public void StateGot(DbConnection connection, DbConnectionInterceptionContext<System.Data.ConnectionState> interceptionContext)
        {
        }
    }

    public class SessionContextConfiguration : DbConfiguration
    {
        public SessionContextConfiguration()
        {
            AddInterceptor(new SessionContextInterceptor());
        }
    }
}
```

To je potrebna promjena samo aplikacije. Nastaviti i stvaranje i objavljivanje aplikacija.

## <a name="step-2-add-a-userid-column-to-the-database-schema"></a>Korak 2: Dodavanje stupca ID korisnika shemu baze podataka

Sljedeći je korak da biste dodali stupac ID korisnika tablicu Kontakti za svaki redak pridružiti korisnika (klijentsko). Ne možemo će promijenite shemom izravno u bazi podataka, tako da se ne možemo ne morate uključiti polje u našem EF podatkovnog modela.

Povezivanje s bazom podataka izravno pomoću SQL Server Management Studio ili Visual Studio i zatim izvršite sljedeće T-SQL:

```
ALTER TABLE Contacts ADD UserId nvarchar(128)
    DEFAULT CAST(SESSION_CONTEXT(N'UserId') AS nvarchar(128))
```

Time se dodaje korisnički ID stupca u tablicu Kontakti. Koristimo vrstu podataka nvarchar(128) tako da odgovara UserIds pohranjene u tablici AspNetUsers pa ćemo stvoriti ZADANI ograničenja koja se automatski postavlja korisnički ID za novoumetnuti retke biti korisnički ID pohranjena u SESSION_CONTEXT.

Sada u tablici izgleda ovako:

![Tablicu SSMS kontakti](./media/web-sites-dotnet-entity-framework-row-level-security/SSMS-Contacts.png)

Prilikom stvaranja novih kontakata, oni će automatski se dodijeliti točan korisnički ID. Pokazni videozapis svrhe, međutim, recimo dodijeliti nekoliko te postojeće kontakte postojećeg korisnika.

Ako ste stvorili nekoliko korisnika u aplikaciji već (npr., koristeći lokalne, Google ili Facebook računi), vidjet ćete ih u tablici AspNetUsers. U nastavku snimka postoji samo jedan korisnik dosad.

![SSMS AspNetUsers tablice](./media/web-sites-dotnet-entity-framework-row-level-security/SSMS-AspNetUsers.png)

Kopiranje Id user1@contoso.com, i zalijepite je u nastavku T-SQL naredbu. Izvršavanje izjavu o zaštiti tri kontakte povezivati s ovom ID korisnika.

```
UPDATE Contacts SET UserId = '19bc9b0d-28dd-4510-bd5e-d6b6d445f511'
WHERE ContactId IN (1, 2, 5)
```

## <a name="step-3-create-a-row-level-security-policy-in-the-database"></a>Korak 3: Stvaranje pravila za sigurnost na razini retka u bazi podataka

U posljednjem koraku je da biste stvorili sigurnosna pravila koja koristi u ID korisnika u SESSION_CONTEXT automatski filtriranje rezultata koji je vratio upit.

Dok još uvijek povezani s bazom podataka, izvršiti sljedeće T-SQL:

```
CREATE SCHEMA Security
go

CREATE FUNCTION Security.userAccessPredicate(@UserId nvarchar(128))
    RETURNS TABLE
    WITH SCHEMABINDING
AS
    RETURN SELECT 1 AS accessResult
    WHERE @UserId = CAST(SESSION_CONTEXT(N'UserId') AS nvarchar(128))
go

CREATE SECURITY POLICY Security.userSecurityPolicy
    ADD FILTER PREDICATE Security.userAccessPredicate(UserId) ON dbo.Contacts,
    ADD BLOCK PREDICATE Security.userAccessPredicate(UserId) ON dbo.Contacts
go

```

Kod ne tri stvari. Najprije stvara novu shemu kao preporučenim načinom rada za prikupljanje i ograničavanje pristupa RLS objekte. Nakon toga stvara predikata funkciju koja će vratiti "1" kada korisnički ID retka zadovoljava ID korisnika u SESSION_CONTEXT. Na kraju, stvara sigurnosna pravila koji dodaje tu funkciju kao filtar i blokiranje predikata na tablice Kontakti. Filtar predikata uzrokuje upita da biste vratili samo retke koji pripadaju trenutnog korisnika i predikata bloka služi kao zaštitu da biste spriječili aplikacije ikad slučajno umetanja retka za korisnika u redu.

Sada pokrenite aplikaciju i prijavite se kao user1@contoso.com. Taj korisnik sada vidi samo kontakte smo dodijeljene ovom korisnički ID ranije:

![Aplikacija Contact Manager prije omogućivanja RLS](./media/web-sites-dotnet-entity-framework-row-level-security/ContactManagerApp-After.png)

Da biste provjerili valjanost to Dodatno, pokušajte Registracija novog korisnika. Prikazat će se kontakata, jer nema dodijeljenih na njih. Ako ih stvorite novi kontakt, on će se dodijeliti ih, a samo će moći vidjeti.

## <a name="next-steps"></a>Daljnji koraci

To je to! Jednostavan Contact Manager web-aplikaciji je pretvorena u klijentu za više jedno mjesto svaki korisnik ima vlastiti popis kontakata. Pomoću sigurnosti na razini retka smo ste izbjegava složenost provoditi klijentu pristup logike u našem kod aplikacije. U ovom prozirnost omogućuje aplikacije radi fokusiranja na problem realni tvrtke konkretnom i i smanjuje rizik slučajno propušta podataka kao što je aplikacija je kodna baza poveća.

Pomoću ovog praktičnog vodiča sadrži samo izgreben površina što je moguće s RLS. Ako, primjerice, je moguće više imaju sofisticirane ili logike zrnastog pristup, a je moguće spremiti samo trenutni korisnički ID u u SESSION_CONTEXT. Preporučuje se i moguće [integrirati RLS s bibliotekama klijenta za alate elastic baze podataka](../sql-database/sql-database-elastic-tools-multi-tenant-row-level-security.md) za podršku shards više klijenta u sloj skaliranje iz podataka.

Osim tih mogućnosti i Radimo na čak i poboljšati RLS. Ako imate pitanja, ideja ili stvari koje biste željeli da biste vidjeli, obratite nam se keyboardshortcuts@Microsoft.commailto u komentarima. Cijenimo vaše povratne informacije!
