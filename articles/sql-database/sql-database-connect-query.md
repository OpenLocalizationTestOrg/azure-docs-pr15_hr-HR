<properties
    pageTitle="Povezivanje s bazom podataka SQL s upitom C# | Microsoft Azure"
    description="Napišite program u C# za slanje upita i povezivanje s bazom podataka SQL. Informacije o IP adrese, nizove veze, sigurna prijava i besplatne Visual Studio."
    services="sql-database"
    keywords="c# upita baze podataka, c# upita, povezivanje s bazom podataka, SQL c.#"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="08/17/2016"
    ms.author="stevestein"/>



# <a name="connect-to-a-sql-database-with-visual-studio"></a>Povezivanje s bazom podataka SQL s Visual Studio

> [AZURE.SELECTOR]
- [Visual Studio](sql-database-connect-query.md)
- [SSMS](sql-database-connect-query-ssms.md)
- [Excel](sql-database-connect-excel.md)

Saznajte kako se povezati s bazom podataka Azure SQL u Visual Studio. 

## <a name="prerequisites"></a>Preduvjeti


Da biste se povezali s bazom podataka SQL pomoću Visual Studio, potrebno je sljedeće: 


- SQL baze podataka možete povezati. U ovom se članku koristi oglednu bazu podataka **AdventureWorks** . Da biste dobili oglednu bazu podataka AdventureWorks, potražite u članku [Stvaranje probne baze podataka](sql-database-get-started.md).


- Visual Studio ažuriranje 2013 4 (ili noviji). Microsoft pruža Visual Studio zajednice *besplatno*.
 - [Web-mjesto zajednice za Visual Studio, u okvir za preuzimanje](http://www.visualstudio.com/products/visual-studio-community-vs)
 - [Dodatne mogućnosti slobodno Visual Studio](http://www.visualstudio.com/products/free-developer-offers-vs.aspx)




## <a name="open-visual-studio-from-the-azure-portal"></a>Otvori Visual Studio s portala za Azure


1. Prijavite se na [portal za Azure](https://portal.azure.com/).

2. Kliknite **Više usluge** > **baze podataka SQL**
3. Otvorite bazu podataka plohu **AdventureWorks** pronalaženjem i klikom na bazu podataka *AdventureWorks* .

6. Kliknite gumb **Alati** pri vrhu plohu baze podataka:

    ![Novi upit. Povezivanje s bazom podataka sustava SQL server: SQL Server Management Studio](./media/sql-database-connect-query/tools.png)

7. Kliknite **Otvori u Visual Studio** (Ako vam je potrebna Visual Studio, kliknite vezu za preuzimanje):

    ![Novi upit. Povezivanje s bazom podataka sustava SQL server: SQL Server Management Studio](./media/sql-database-connect-query/open-in-vs.png)


8. Visual Studio otvorit će se prozor **za povezivanje s poslužiteljem** već postavio za povezivanje s poslužitelja i baze podataka koji ste odabrali na portalu.  (Kliknite **Mogućnosti** da biste potvrdili da se veza postavljen tako da točan baze podataka). Unesite lozinku administratora poslužitelja, a zatim kliknite **Poveži**.


    ![Novi upit. Povezivanje s bazom podataka sustava SQL server: SQL Server Management Studio](./media/sql-database-connect-query/connect.png)


8. Ako ne morate postaviti pravila vatrozida za vaše računalo IP adresa koji se *ne može povezati* poruka ovdje. Da biste stvorili pravilo vatrozid, potražite u članku [Konfiguriranje pravilo vatrozid razini poslužitelja baze podataka SQL Azure](sql-database-configure-firewall-settings.md).


9. Nakon uspješnog povezivanja, **SQL Server objekt Explorer** otvara se prozor s vezom na vlastitu bazu podataka.

    ![Novi upit. Povezivanje s bazom podataka sustava SQL server: SQL Server Management Studio](./media/sql-database-connect-query/sql-server-object-explorer.png)


## <a name="run-a-sample-query"></a>Izvođenje upita uzorka

Nakon što smo ste povezani s bazom podataka, na sljedeći način Prikaži pokretanje jednostavnog upita:

2. Desnom tipkom miša kliknite bazu podataka, a zatim odaberite **Novi upit**.

    ![Novi upit. Povezivanje s bazom podataka sustava SQL server: SQL Server Management Studio](./media/sql-database-connect-query/new-query.png)

3. U prozoru upita, kopirajte i zalijepite sljedeći kod.

        SELECT
        CustomerId
        ,Title
        ,FirstName
        ,LastName
        ,CompanyName
        FROM SalesLT.Customer;

4. Kliknite gumb **izvrši** da biste pokrenuli upit:

    ![Uspjeh. Povezivanje s bazom podataka sustava SQL server: SVisual Studio](./media/sql-database-connect-query/run-query.png)

## <a name="next-steps"></a>Daljnji koraci

- Otvaranje baze podataka SQL u Visual Studio koristi SQL Server Data Tools. Dodatne informacije potražite u članku [SQL Server Data Tools](https://msdn.microsoft.com/library/hh272686.aspx).
- Da biste se povezali s bazom podataka SQL pomoću koda, potražite u članku [Povezivanje s bazom podataka SQL pomoću .NET (C#)](sql-database-develop-dotnet-simple.md).



