<properties
    pageTitle="Nadzor prostora za pohranu u memoriji za XTP | Microsoft Azure"
    description="Procjena i nadzor prostora za pohranu u memoriji za XTP koriste, kapaciteta. Razrješavanje pogreške kapaciteta 41823"
    services="sql-database"
    documentationCenter=""
    authors="jodebrui"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="jodebrui"/>


# <a name="monitor-in-memory-oltp-storage"></a>Spremanje u memoriji OLTP monitora

Kada se koristi [u memoriji OLTP](sql-database-in-memory.md)podataka u memorija optimizirana tablice i varijable tablica nalazi se u OLTP u memoriji za pohranu. Svaki sloju servisa Premium ima najveći OLTP u memoriji prostora za pohranu veličinu, koji je opisan u [članku SQL baze podataka usluge razine](sql-database-service-tiers.md#service-tiers-for-single-databases). Kada se to ograničenje prekorači, umetati i ažurirati operacije pokretanje neuspješnih (uz poruku o pogrešci 41823). U tom trenutku ćete potrebno da biste izbrisati podatke da biste oslobodili memorije ili nadogradnja sloju performanse baze podataka.

## <a name="determine-whether-data-will-fit-within-the-in-memory-storage-cap"></a>Određivanje hoće li se podaci stane u kapica prostora za pohranu u memoriji

Određivanje kapaciteta za pohranu: potražite u [članku SQL baze podataka usluge razine](sql-database-service-tiers.md#service-tiers-for-single-databases) tipka CAPS LOCK prostora za pohranu različite razine servisa Premium.

Procjena memorije preduvjeti za memorija optimizirana tablice funkcionira na isti način za SQL Server jer ona ne u bazi podataka SQL Azure. Potrajati nekoliko minuta da biste pregledali teme na [MSDN-u](https://msdn.microsoft.com/library/dn282389.aspx).

Imajte na umu da se u tablicu i varijable reci tablice, kao i indeksi, Brojanje prema veličina podataka Maks korisnika. Uz to, ALTER TABLE mora dovoljno prostora da biste stvorili novu verziju cijelu tablicu i njegove indekse.

## <a name="monitoring-and-alerting"></a>Nadzor i upozorenjem

Možete nadzirati prostor za pohranu u memoriji koristi kao postotak u [kapac prostora za pohranu za vaše sloju performanse](sql-database-service-tiers.md#service-tiers-for-single-databases) Azure [portal](https://portal.azure.com/): 

- Pronađite okvir Upotreba resursa plohu baze podataka i kliknite Uredi.
- Odaberite metriku `In-Memory OLTP Storage percentage`.
- Da biste dodali upozorenja, kliknite okvir Upotreba resursa da biste otvorili metričkim plohu, a zatim kliknite Dodaj upozorenje.

Ili da biste prikazali Upotreba prostora za pohranu u memoriji sljedeći upit:

    SELECT xtp_storage_percent FROM sys.dm_db_resource_stats


## <a name="correct-out-of-memory-situations---error-41823"></a>Ispravljanje-memorije situacijama - pogreške 41823

Izvodi rezultate iz memorije u Umetanje, ažuriranje i stvaranje operacije ne uspijeva uz poruku o pogrešci 41823.

Poruka o pogrešci 41823 označava da memorija optimizirana tablice i varijable tablica ste premašili maksimalne veličine.

Da biste riješili ovu pogrešku na jedan od sljedećih načina:


- Brisanje podataka iz memorije Optimizirano tablica, potencijalno rasteretite podatke koje želite tradicionalni temeljenog na disku tablice; ili,
- Nadogradite sloju servisa s dovoljno prostora za pohranu u memoriji za podatke morate zadržati u tablicama memorija optimizirana.

## <a name="next-steps"></a>Daljnji koraci
Dodatne resurse vezane uz o [nadzor Azure SQL baze podataka pomoću dinamičke Upravljanje prikazima](sql-database-monitoring-with-dmvs.md)
