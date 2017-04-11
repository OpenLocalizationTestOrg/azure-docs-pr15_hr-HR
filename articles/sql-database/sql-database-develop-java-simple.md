<properties
    pageTitle="Povezivanje s bazom podataka SQL pomoću Java JDBC u sustavu Windows | Microsoft Azure"
    description="Prikazuje uzorak koda programskog jezika Java, možete koristiti da biste se povezali s bazom podataka SQL Azure. Uzorak koristi JDBC te će se pokrenuti na klijentskom računalu sa sustavom Windows."
    services="sql-database"
    documentationCenter=""
    authors="LuisBosquez"
    manager="jhubbard"
    editor="genemi"/>


<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="lbosq"/>


# <a name="connect-to-sql-database-by-using-java-with-jdbc-on-windows"></a>Povezivanje s bazom podataka SQL pomoću Java JDBC u sustavu Windows


[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)] 


U ovoj se temi predstavlja uzorak koda programskog jezika Java koje možete koristiti da biste se povezali s bazom podataka SQL Azure. Ogledna Java oslanja na na Java Development Kit (JDK) verzije 1.8. Uzorak se povezuje s bazom podataka SQL Azure pomoću JDBC upravljački program.

## <a name="step-1--configure-development-environment"></a>Korak 1: Konfiguriranje razvojno okruženje

[Konfiguriranje platforme za razvoj Java](https://msdn.microsoft.com/library/mt720658.aspx)

## <a name="step-2-create-a-sql-database"></a>Korak 2: Stvaranje baze podataka SQL

Pogledajte na [Uvodna stranica](sql-database-get-started.md) da biste saznali kako stvoriti bazu podataka.  

## <a name="step-3-get-connection-string"></a>Korak 3: Dohvaćanje niza veze

[AZURE.INCLUDE [sql-database-include-connection-string-jdbc-20-portalshots](../../includes/sql-database-include-connection-string-jdbc-20-portalshots.md)]

> [AZURE.NOTE] Ako koristite JTDS JDBC upravljački program, a zatim morat ćete dodati "ssl = zahtijevaju" URL veze niz, a vi morate postaviti sljedeće mogućnosti za na JVM "-Djsse.enableCBCProtection=false". Ta mogućnost JVM onemogućuje rješenje za sigurnost slabe, pa provjerite da znate što rizika sudjeluje prije postavljanja ove mogućnosti.

## <a name="step-4-run-sample-code"></a>Korak 4: Pokretanje uzorak koda

* [Dokaz pojam povezati SQL pomoću jezika Java](https://msdn.microsoft.com/library/mt720656.aspx)

## <a name="next-steps"></a>Daljnji koraci

* Posjetite [Centar za razvojne inženjere Java](/develop/java/).
* Pregledajte [Pregled razvoj baze podataka za SQL](sql-database-develop-overview.md)
* Dodatne informacije o [Upravljački program JDBC Microsoft SQL Server](https://msdn.microsoft.com/library/mt484311.aspx)

## <a name="additional-resources"></a>Dodatni resursi 

* [Uzorci dizajna za SaaS više klijentske aplikacije s bazom podataka Azure SQL](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Istražite [Mogućnosti SQL baze podataka](https://azure.microsoft.com/services/sql-database/)
