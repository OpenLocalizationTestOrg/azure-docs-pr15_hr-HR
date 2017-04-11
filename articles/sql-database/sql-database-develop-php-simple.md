<properties
    pageTitle="Povezivanje s bazom podataka SQL pomoću i u sustavu Windows | Microsoft Azure"
    description="Predstavlja program za uzorak i koje se povezuje s bazom podataka SQL Azure Windows klijenta i navode veze na komponente potreban softver potreban za klijent."
    services="sql-database"
    documentationCenter=""
    authors="meet-bhagdev"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="php"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="meetb"/>


# <a name="connect-to-sql-database-by-using-php-on-windows"></a>Povezivanje s bazom podataka SQL pomoću PHP u sustavu Windows


[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)] 


U ovoj se temi prikazuje kako možete povezati s bazom podataka SQL Azure s klijentska aplikacija pisane PHP koji se izvodi u sustavu Windows.

## <a name="step-1--configure-development-environment"></a>Korak 1: Konfiguriranje razvojno okruženje

[Konfiguriranje platforme za razvoj i](https://msdn.microsoft.com/library/mt720663.aspx)

## <a name="step-2-create-a-sql-database"></a>Korak 2: Stvaranje baze podataka SQL

Pogledajte na [Uvodna stranica](sql-database-get-started.md) da biste saznali kako stvoriti ogledne baze podataka.  Važno slijedite vodič da biste stvorili u **predlošku baze podataka AdventureWorks**je. Uzorci prikazano u nastavku funkcioniraju samo na **AdventureWorks shemu**.


## <a name="step-3-get-connection-details"></a>Korak 3: Dohvaćanje detalja veze

[AZURE.INCLUDE [sql-database-include-connection-string-details-20-portalshots](../../includes/sql-database-include-connection-string-details-20-portalshots.md)]


## <a name="step-4-run-sample-code"></a>Korak 4: Pokretanje uzorak koda

* [Dokaz pojam povezati SQL pomoću PHP](https://msdn.microsoft.com/library/mt720665.aspx)
* [Povezivanje resiliently SQL s PHP](https://msdn.microsoft.com/library/mt720667.aspx)


## <a name="next-steps"></a>Daljnji koraci

* Pregledajte [Pregled razvoj baze podataka za SQL](sql-database-develop-overview.md)
* Dodatne informacije o [Upravljački program PHP Microsoft SQL Server](https://msdn.microsoft.com/library/dn865013.aspx)
* Dodatne informacije o PHP instalaciju i korištenje potražite u članku [Pristup SQL poslužitelja baze podataka s PHP](http://social.technet.microsoft.com/wiki/contents/articles/1258.accessing-sql-server-databases-from-php.aspx).

## <a name="additional-resources"></a>Dodatni resursi 

* [Uzorci dizajna za SaaS više klijentske aplikacije s bazom podataka Azure SQL](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Istražite [Mogućnosti SQL baze podataka](https://azure.microsoft.com/services/sql-database/)
