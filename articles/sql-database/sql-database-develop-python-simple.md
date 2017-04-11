<properties
    pageTitle="Povezivanje s bazom podataka SQL pomoću Python | Microsoft Azure"
    description="Prikazuje uzorak koda Python, možete koristiti da biste se povezali s bazom podataka SQL Azure."
    services="sql-database"
    documentationCenter=""
    authors="meet-bhagdev"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="meetb"/>


# <a name="connect-to-sql-database-by-using-python"></a>Povezivanje s bazom podataka SQL pomoću Python


[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)] 


U ovoj se temi objašnjava da bi povezao i upita programa Azure SQL baze podataka pomoću Python. Ovaj primjer možete pokrenuti iz sustava Windows, Ubuntu Linux ili Mac platforme.


## <a name="step-1-create-a-sql-database"></a>Korak 1: Stvaranje baze podataka SQL

Pogledajte na [Uvodna stranica](sql-database-get-started.md) da biste saznali kako stvoriti ogledne baze podataka.  Važno slijedite vodič da biste stvorili u **predlošku baze podataka AdventureWorks**je. Uzorci prikazano u nastavku funkcioniraju samo na **AdventureWorks shemu**. Kada stvarate vaše baze podataka provjerite jesu li omogućite pristup IP adrese za omogućivanjem pravila vatrozida kao što je opisano u [Uvodna stranica](sql-database-get-started.md)

## <a name="step-2-configure-development-environment"></a>Korak 2: Konfiguriranje razvojno okruženje

### <a name="mac-os"></a>**Mac OS**   
### <a name="install-the-required-modules"></a>Instalirajte potrebne moduli
Otvorite svoje terminal i instalirati

    ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
    brew install FreeTDS
    sudo -H pip install pymssql=2.1.1

### <a name="linux-ubuntu"></a>**Linux (Ubuntu)**

Otvorite svoje terminal i idite na direktorij koju namjeravate o stvaranju vaše python skripte. Unesite sljedeće naredbe za instalaciju **FreeTDS** i **pymssql**. pymssql koristi FreeTDS da biste se povezali s bazama podataka za SQL.

    sudo apt-get --assume-yes update
    sudo apt-get --assume-yes install freetds-dev freetds-bin
    sudo apt-get --assume-yes install python-dev python-pip
    sudo pip install pymssql=2.1.1
    
### <a name="windows"></a>**Windows**

Instalirajte pymssql [**ovdje**](http://www.lfd.uci.edu/~gohlke/pythonlibs/#pymssql). 

Provjerite jeste li odabrali ispravnu whl datoteku. Na primjer: Ako koristite Python 2.7 na 64-bitnog odaberite: pymssql‑2.1.1‑cp27‑none‑win_amd64.whl. Kada preuzmete datoteku .whl je postavite u mapi C:/Python27.

Sada instalirati upravljački program za pymssql pomoću točaka iz naredbenog retka. CD-a u C:/Python27 i Pokreni sljedeće
    
    pip install pymssql‑2.1.1‑cp27‑none‑win_amd64.whl

Upute za omogućivanje korištenja točaka možete pronaći [ovdje](http://stackoverflow.com/questions/4750806/how-to-install-pip-on-windows)

## <a name="step-3-run-sample-code"></a>Korak 3: Pokretanje uzorak koda

Stvorite datoteku s nazivom **sql_sample.py** i zalijepite sljedeći kod unutar njega. To možete pokrenuti iz naredbenog retka pomoću:
    
    python sql_sample.py

### <a name="connect-to-your-sql-database"></a>Povezivanje s bazom podataka za SQL

Funkcija [pymssql.connect](http://pymssql.org/en/latest/ref/pymssql.html) se koristi za povezivanje s bazom podataka SQL.

    import pymssql
    conn = pymssql.connect(server='yourserver.database.windows.net', user='yourusername@yourserver', password='yourpassword', database='AdventureWorks')


### <a name="execute-an-sql-select-statement"></a>Izvršavanje SQL naredbe SELECT

Funkcija [cursor.execute](http://pymssql.org/en/latest/ref/pymssql.html#pymssql.Cursor.execute) se može koristiti za dohvaćanje skup iz upita bazi podataka SQL rezultata. Ova funkcija zapravo prihvaća bilo koji upit i vraća skup rezultata koji se iterated putem pomoću [cursor.fetchone()](http://pymssql.org/en/latest/ref/pymssql.html#pymssql.Cursor.fetchone).


    import pymssql
    conn = pymssql.connect(server='yourserver.database.windows.net', user='yourusername@yourserver', password='yourpassword', database='AdventureWorks')
    cursor = conn.cursor()
    cursor.execute('SELECT c.CustomerID, c.CompanyName,COUNT(soh.SalesOrderID) AS OrderCount FROM SalesLT.Customer AS c LEFT OUTER JOIN SalesLT.SalesOrderHeader AS soh ON c.CustomerID = soh.CustomerID GROUP BY c.CustomerID, c.CompanyName ORDER BY OrderCount DESC;')
    row = cursor.fetchone()
    while row:
        print str(row[0]) + " " + str(row[1]) + " " + str(row[2])   
        row = cursor.fetchone()


### <a name="insert-a-row-pass-parameters-and-retrieve-the-generated-primary-key"></a>Umetanje retka, prenesite parametara i dohvaćanje generirani primarnog ključa

U bazi podataka SQL svojstvo [IDENTITETA](https://msdn.microsoft.com/library/ms186775.aspx) i objekt [SLIJED](https://msdn.microsoft.com/library/ff878058.aspx) omogućuje automatsko generiranje vrijednosti [primarnog ključa](https://msdn.microsoft.com/library/ms179610.aspx) . 


    import pymssql
    conn = pymssql.connect(server='yourserver.database.windows.net', user='yourusername@yourserver', password='yourpassword', database='AdventureWorks')
    cursor = conn.cursor()
    cursor.execute("INSERT SalesLT.Product (Name, ProductNumber, StandardCost, ListPrice, SellStartDate) OUTPUT INSERTED.ProductID VALUES ('SQL Server Express', 'SQLEXPRESS', 0, 0, CURRENT_TIMESTAMP)")
    row = cursor.fetchone()
    while row:
        print "Inserted Product ID : " +str(row[0])
        row = cursor.fetchone()


### <a name="transactions"></a>Transakcije


U ovom se primjeru kod pokazuje korištenje transakcije u kojem ste:

* Početak transakcije
* Umetanje retka podataka
* Vraćanje svoje transakcije da biste poništili insert 

Zalijepite sljedeći kod unutar sql_sample.py.
    
    import pymssql
    conn = pymssql.connect(server='yourserver.database.windows.net', user='yourusername@yourserver', password='yourpassword', database='AdventureWorks')
    cursor = conn.cursor()
    cursor.execute("BEGIN TRANSACTION")
    cursor.execute("INSERT SalesLT.Product (Name, ProductNumber, StandardCost, ListPrice, SellStartDate) OUTPUT INSERTED.ProductID VALUES ('SQL Server Express New', 'SQLEXPRESS New', 0, 0, CURRENT_TIMESTAMP)")
    cnxn.rollback()

## <a name="next-steps"></a>Daljnji koraci

* Pregledajte [Pregled razvoj baze podataka za SQL](sql-database-develop-overview.md)
* Dodatne informacije o [Upravljački program Python Microsoft SQL Server](https://msdn.microsoft.com/library/mt652092.aspx)
* Posjetite [Centar za razvojne inženjere Python](/develop/python/).

## <a name="additional-resources"></a>Dodatni resursi 

* [Uzorci dizajna za SaaS više klijentske aplikacije s bazom podataka Azure SQL](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Istražite [Mogućnosti SQL baze podataka](https://azure.microsoft.com/services/sql-database/)
