<properties 
    pageTitle="Priključci izvan 1433 za baze podataka SQL | Microsoft Azure"
    description="Klijent veze iz ADO.NET V12 za baze podataka SQL Azure ponekad zaobići proxy poslužitelj i interakciju izravno s bazom podataka. Priključci osim 1433 postaju važna."
    services="sql-database"
    documentationCenter=""
    authors="MightyPen"
    manager="jhubbard"
    editor="" />


<tags 
    ms.service="sql-database" 
    ms.workload="drivers"
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/17/2016"
    ms.author="annemill"/>


# <a name="ports-beyond-1433-for-adonet-45-and-sql-database-v12"></a>Priključci izvan 1433 za ADO.NET 4,5 i V12 baze podataka SQL


U ovoj se temi opisuju željene promjene u način povezivanja klijenata koji koristi ADO.NET 4,5 ili noviju verziju unosi V12 za baze podataka SQL Azure.


## <a name="v11-of-sql-database-port-1433"></a>V11 SQL baze podataka: priključak 1433


Kada klijentski program koristi ADO.NET 4,5 da bi povezao i upita s V11 baze podataka SQL, internog slijed je na sljedeći način:


1. ADO.NET pokušava da biste se povezali s bazom podataka SQL.

2. ADO.NET koristi priključak 1433 da biste nazvali modula proizvod, a na proizvod koji se povezuje s bazom podataka SQL.

3. Baze podataka SQL šalje njegov odgovor natrag na proizvod prosljeđuje odgovora ADO.NET priključak 1433.


**Terminologija:** Ne možemo opisuju prethodni slijed po kojoj piše da ADO.NET stupi u interakciju s bazom podataka SQL pomoću mogućnosti *usmjeravanje proxy*. Ako nema proizvod je uključen, ne možemo bi izgovorite *Izravni usmjeravanje* koristila.


## <a name="v12-of-sql-database-outside-vs-inside"></a>V12 SQL baze podataka: izvan Dodavanje veze za vanjskih unutar


Za veze na V12 tražimo morate li klijentski program pokreće *izvan* ili *unutar* granicu Azure oblaka. Na Pododlomci koji govori o dva uobičajena scenarija.


#### <a name="outside-client-runs-on-your-desktop-computer"></a>*Izvan:* Klijent koji se izvodi na stolnom računalu


Priključak 1433 je samo priključak koji moraju biti otvoreni na stolnom računalu koje hostira baze podataka SQL klijentsku aplikaciju.


#### <a name="inside-client-runs-on-azure"></a>*Unutar:* Klijent koji se izvodi na Azure


Kada se pokrene klijent unutar granicu Azure oblaka, koristit će što smo da biste uputili poziv *Izravni usmjeravanje* interakciju s bazom podataka sustava SQL server. Kada je uspostaviti vezu, dodatno interakcije između klijentskih i baze podataka obuhvaćaju nema proxy proizvod.


Slijed je na sljedeći način:


1. ADO.NET 4.5 (ili noviji) pokreće kratak interakciju s Azure oblaka i prima broj priključka dinamički identificirani.
 - Broj priključka dinamički identificirani je u rasponu od 11000 11999 ili 14000 14999.

2. ADO.NET zatim se povezuje s poslužiteljem baze podataka SQL izravno s bez proizvod u međuvremenu.

3. Upiti se šalju izravno s bazom podataka, a rezultati se vraćaju izravno na klijentu.


Pobrinite se da priključak raspona 11000 11999 i 14000 14999 na Azure klijentskom računalu ostaju dostupna za ADO.NET 4,5 klijent interakcije s V12 baze podataka SQL.

- Posebno priključke u rasponu mora biti besplatna bilo koje druge izlaznog skočnih prozora.

- Na vašem VM Azure **Vatrozid za Windows s dodatnom sigurnošću** kontrole postavke priključka.
 - Dodavanje pravila za koju navedete protokolom **TCP** uz raspon s vidjet ćete da sintaksa kao što je **11000 11999**možete koristiti [vatrozida korisničkog sučelja](http://msdn.microsoft.com/library/cc646023.aspx) .


## <a name="version-clarifications"></a>Clarifications verzija


U ovom se odjeljku pojašnjava nadimaka koji se odnose na verzije proizvoda. Navodi i neke uparivanja verzija između proizvoda.


#### <a name="adonet"></a>ADO.NET


- ADO.NET 4.0 podržava protokol TDS 7,3, ali ne 7,4.
- ADO.NET 4.5 i noviji podržava protokol TDS 7,4.


#### <a name="sql-database-v11-and-v12"></a>SQL baza podataka V11 i V12


Klijent veze razlike između V11 baze podataka SQL i V12 istaknute su u ovoj temi.


*Bilješke:* Transact-SQL naredbe `SELECT @@version;` vraća vrijednost koji počinju s brojem kao što su "11". ili "12.", a one odgovara naš nazive verzija V11 i V12 za SQL baze podataka.


## <a name="related-links"></a>Srodne veze


- ADO.NET 4.6 objavljen srpanj 20, 2015. Objava na blogu tima za .NET dostupna je [u nastavku](http://blogs.msdn.com/b/dotnet/archive/2015/07/20/announcing-net-framework-4-6.aspx).


- ADO.NET 4,5 objavljen kolovoz 15, 2012. Objava na blogu tima za .NET dostupna je [u nastavku](http://blogs.msdn.com/b/dotnet/archive/2012/08/15/announcing-the-release-of-net-framework-4-5-rtm-product-and-source-code.aspx).
 - Dostupna je na blogu o ADO.NET 4.5.1 [ovdje](http://blogs.msdn.com/b/dotnet/archive/2013/06/26/announcing-the-net-framework-4-5-1-preview.aspx).


- [Popis verzija protokol TDS](http://www.freetds.org/userguide/tdshistory.htm)


- [Pregled razvoj baze podataka za SQL](sql-database-develop-overview.md)


- [Vatrozid za baze podataka SQL Azure](sql-database-firewall-configure.md)


- [Kako: konfigurirati postavke vatrozida u SQL baze podataka](sql-database-configure-firewall-settings.md)

