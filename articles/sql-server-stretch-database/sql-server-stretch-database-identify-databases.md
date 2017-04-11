<properties
    pageTitle="Prepoznavanje baze podataka i tablica za bazu podataka rastezanje tako da pokrenete rastezanje Savjetnik za baze podataka | Microsoft Azure"
    description="Saznajte kako prepoznati baze podataka i tablice koje su ikone za rastezanje bazu podataka."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/14/2016"
    ms.author="douglasl"/>

# <a name="identify-databases-and-tables-for-stretch-database-by-running-stretch-database-advisor"></a>Prepoznavanje baze podataka i tablica za bazu podataka rastezanje tako da pokrenete rastezanje Savjetnik za baze podataka

Da biste odredili baze podataka i tablice koje su ikone za bazu podataka rastezanje, preuzmite Savjetnik nadogradnje za SQL Server 2016 i pokrenite rastezanje Savjetnik za bazu podataka. Razvlačenje Savjetnik za baze podataka određuje i tog problema.

## <a name="download-and-install-upgrade-advisor"></a>Preuzimanje i instalacija Savjetnik za nadogradnju
Preuzmite i instalirajte savjetnika za nadogradnju s [ovdje](http://go.microsoft.com/fwlink/?LinkID=613421). Ovaj alat koji ne nalazi na medij za instalaciju sustava SQL Server.

## <a name="run-the-stretch-database-advisor"></a>Pokretanje Advisor rastezanje baze podataka

1.  Pokretanje nadogradnje Advisor.

2.  Odaberite **scenariji**, a zatim odaberite **POKRENI SAVJETNIK RASTEZANJE baze podataka**.

3.  Na plohu **Pokrenuti Savjetnik rastezanje baze podataka** kliknite **Odabir baze podataka da BISTE ANALIZIRALI**.

4.  Na plohu **Odaberite baze podataka** unesite ili odaberite naziv poslužitelja i podatke za provjeru autentičnosti. Kliknite **Poveži**.

5.  Pojavit će se popis baza podataka na odabranom poslužitelju. Odaberite baze podataka koje želite analizirati. Kliknite **Odaberi**.

6.  Na plohu **Pokrenuti Savjetnik rastezanje baze podataka** kliknite **Pokreni**.  Pokreće se analiza.

## <a name="review-the-results"></a>Pregledajte rezultate

1.  Po završetku analizu na plohu **Analyzed baze podataka** odaberite jednu od baze podataka u kojima se analizirati da biste prikazali plohu **rezultate analize** .

    Plohu **rezultate analize** popis preporučenih tablica u odabrane baze podataka koji zadovoljavaju kriterij preporuke zadani.

2.  Na popisu tablica na **rezultate analize** plohu odaberite jednu ili preporučeni tablica da bi se prikazao plohu **tablice rezultata** .

    Ako onemogućuju postoje problemi, **tablice rezultata** plohu popis tog problema za odabranu tablicu. Informacije o blokiranja problemima otkrio rastezanje Savjetnik za baze podataka potražite u članku [ograničenja za rastezanje bazu podataka](sql-server-stretch-database-limitations.md).

3.  Na popisu blokiranja problemi na plohu **tablice rezultata** , odaberite jednu od problema da biste prikazali dodatne informacije o odabrane problem i predlaže ublažiti korake. Ako želite konfigurirati odabranu tablicu za bazu podataka rastezanje, implementirati korake predložene ublažiti.

## <a name="next-step"></a>Sljedeći korak
Omogućivanje rastezanje baze podataka.

-   Da biste omogućili rastezanje baze podataka na bazom **baze podataka**, potražite u članku [Omogućivanje rastezanje baze podataka za bazu podataka](sql-server-stretch-database-enable-database.md).

-   Da biste omogućili rastezanje baze podataka u drugoj **tablici**, kada rastezanje već omogućeno na bazu podataka, potražite u članku [Omogućivanje rastezanje baze podataka za tablicu](sql-server-stretch-database-enable-table.md).

## <a name="see-also"></a>Vidi također

[Ograničenja za rastezanje bazu podataka](sql-server-stretch-database-limitations.md)

[Omogućivanje rastezanje baze podataka za baze podataka](sql-server-stretch-database-enable-database.md)

[Omogućivanje rastezanje baze podataka za tablicu](sql-server-stretch-database-enable-table.md)

[Svih tema vezanih uz Azure SQL Server rastezanje baze podataka usluge](sql-server-stretch-database-index-all-articles.md)
