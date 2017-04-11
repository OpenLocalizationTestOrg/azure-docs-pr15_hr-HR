<properties
    pageTitle="Početak rada s upitima izdvojiti bazu podataka (okomito particija) | Microsoft Azure"   
    description="kako koristiti elastic upita baze podataka s okomito particije baze podataka"
    services="sql-database"
    documentationCenter=""  
    manager="jhubbard"
    authors="torsteng"/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/23/2016"
    ms.author="torsteng" />

# <a name="get-started-with-cross-database-queries-vertical-partitioning-preview"></a>Početak rada s upitima izdvojiti bazu podataka (okomito particija) (pretpregled)

Upit elastic baze podataka (pretpregled) za baze podataka SQL Azure omogućuje vam pokretanje T SQL upite koje se protežu na više baza podataka pomoću točku jednu vezu. Ova se tema odnosi [Okomito particije baze podataka](sql-database-elastic-query-vertical-partitioning.md).  

Kada završi, će: upute za konfiguriranje i korištenje baze podataka SQL Azure za izvođenje upita koje se protežu na više povezanih baza podataka. 

Dodatne informacije o značajci upita elastic baze podataka, pročitajte članak [Pregled upita elastic baze podataka za baze podataka SQL Azure](sql-database-elastic-query-overview.md). 

## <a name="create-the-sample-databases"></a>Stvaranje ogledne baze podataka

Za početak s moramo da biste stvorili dvije baze podataka, **klijenti** i **narudžbe**, u istom ili drugom logičke poslužiteljima.   

Izvršiti sljedeće upita baze podataka **narudžbe** da biste stvorili tablicu **OrderInformation** i ogledne podatke za unos. 

    CREATE TABLE [dbo].[OrderInformation]( 
        [OrderID] [int] NOT NULL, 
        [CustomerID] [int] NOT NULL 
        ) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (123, 1) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (149, 2) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (857, 2) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (321, 1) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (564, 8) 

Sada se izvršiti sljedeći upit **Kupci** bazu podataka da biste stvorili tablicu **CustomerInformation** i ogledne podatke za unos. 

    CREATE TABLE [dbo].[CustomerInformation]( 
        [CustomerID] [int] NOT NULL, 
        [CustomerName] [varchar](50) NULL, 
        [Company] [varchar](50) NULL 
        CONSTRAINT [CustID] PRIMARY KEY CLUSTERED ([CustomerID] ASC) 
    ) 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (1, 'Jack', 'ABC') 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (2, 'Steve', 'XYZ') 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (3, 'Lylla', 'MNO') 

## <a name="create-database-objects"></a>Stvaranje objekata baze podataka
### <a name="database-scoped-master-key-and-credentials"></a>Opsegu glavni ključ i vjerodajnice za bazu podataka

1. Otvorite SQL Server Management Studio ili SQL Server Data Tools u Visual Studio.
2. Povezivanje s bazom podataka narudžbe i izvršiti sljedeće T SQL naredbe:

        CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>'; 
        CREATE DATABASE SCOPED CREDENTIAL ElasticDBQueryCred 
        WITH IDENTITY = '<username>', 
        SECRET = '<password>';  

    "Korisničko ime" i "lozinku" mora biti korisničkog imena i lozinke koje koristite za prijavu u bazu podataka za korisnika.
    Provjera autentičnosti pomoću servisa Azure Active Directory elastic upita trenutno nisu podržani.

### <a name="external-data-sources"></a>Vanjski izvori podataka
Da biste stvorili vanjski izvor podataka, pokrenite sljedeću naredbu narudžbe bazu podataka: 

    CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc WITH 
        (TYPE = RDBMS, 
        LOCATION = '<server_name>.database.windows.net', 
        DATABASE_NAME = 'Customers', 
        CREDENTIAL = ElasticDBQueryCred, 
    ) ;

### <a name="external-tables"></a>Vanjski tablice
Stvorite vanjska tablica u bazi podataka narudžbe, koji odgovara definiciju tablice CustomerInformation:

    CREATE EXTERNAL TABLE [dbo].[CustomerInformation] 
    ( [CustomerID] [int] NOT NULL, 
      [CustomerName] [varchar](50) NOT NULL, 
      [Company] [varchar](50) NOT NULL) 
    WITH 
    ( DATA_SOURCE = MyElasticDBQueryDataSrc) 

## <a name="execute-a-sample-elastic-database-t-sql-query"></a>Izvršavanje primjer elastic baze podataka T SQL upita

Kada definirate vanjskog izvora podataka i vanjske tablice T SQL sada možete koristiti za dohvaćanje vanjskih tablice. Izvršavanje ovog upita narudžbe bazu podataka: 

    SELECT OrderInformation.CustomerID, OrderInformation.OrderId, CustomerInformation.CustomerName, CustomerInformation.Company 
    FROM OrderInformation 
    INNER JOIN CustomerInformation 
    ON CustomerInformation.CustomerID = OrderInformation.CustomerID 

## <a name="cost"></a>Trošak

Trenutno značajku upita elastic baze podataka se tekst uvrštava u trošak baze podataka SQL Azure.  

Informacije o cijenama potražite u članku [Cijene za SQL baze podataka](/pricing/details/sql-database). 


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->

<!--anchors-->
