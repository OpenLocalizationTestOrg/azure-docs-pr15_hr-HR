<properties 
    pageTitle="Premještanje podataka s bazom podataka SQL Azure za Azure strojnog učenja | Azure" 
    description="Stvaranje tablica sustava SQL i učitavanja podataka SQL tablici" 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/14/2016"
    ms.author="bradsev" /> 

# <a name="move-data-to-an-azure-sql-database-for-azure-machine-learning"></a>Premještanje podataka s bazom podataka SQL Azure za Azure strojnog učenja

U ovoj se temi navode mogućnosti za premještanje podataka s nehijerarhijskom datoteka (CSV ili TSV oblikovanja) ili s podataka pohranjenih u lokalnog sustava SQL Server s bazom podataka Azure SQL. Ove zadatke za premještanje podataka s oblakom su dio postupka timu podataka Znanstvena.

Temu koja opisuje mogućnosti za premještanje podataka sustava SQL Server na lokaciji za učenje za strojno, potražite u članku [Premještanje podataka za SQL Server na Azure virtualnog računala](machine-learning-data-science-move-sql-server-virtual-machine.md).

Sljedeće veze **izbornika** na temu koja opisuje kako ingest podatke u ciljnu okruženja mjesto spremanja i obrađuju tijekom u timu podataka znanstvenog procesa (TDSP) podatke.

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

U sljedećoj su tablici navedene su mogućnosti za premještanje podataka s bazom podataka SQL Azure.

<b>IZVOR</b> |<b>ODREDIŠTE: Azure SQL baze podataka</b> |
-------------- |--------------------------------|
<b>Nehijerarhijskom datotekom (CSV ili TSV oblikovani)</b> |<a href="#bulk-insert-sql-query">Masovno Umetanje SQL upita |
<b>SQL Server na lokaciji</b> | 1. <a href="#export-flat-file">izvoz Nehijerarhijskom datotekom<br> 2. <a href="#insert-tables-bcp">Čarobnjak za migraciju SQL baze podataka<br> 3. <a href="#db-migration">baze podataka sigurnosne kopije i vraćanje<br> 4. <a href="#adf">tvorničke azure podataka |


## <a name="prereqs"></a>Preduvjeti
Postupci opisani u nastavku morate imati:

* Za **Azure pretplate**. Ako nemate pretplatu, možete se prijaviti za [besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/).
* **Račun za Azure prostora za pohranu**. Koristite račun za Azure prostora za pohranu za spremanje podataka pomoću ovog praktičnog vodiča. Ako nemate račun za Azure prostora za pohranu, u članku [Stvaranje računa za pohranu](storage-create-storage-account.md#create-a-storage-account) . Kada stvorite račun za pohranu, morate nabaviti ključ za račun za pristup prostora za pohranu. Potražite u članku [Prikaz, Kopiraj i regenerate prostora za pohranu pristupnih tipki](storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).
* Pristup s **bazom podataka Azure SQL**. Ako morate postaviti baze podataka SQL Azure, [Uvod u rad s bazom podataka sustava Microsoft Azure SQL](../sql-database/sql-database-get-started.md) pruža informacije o tome kako Dodjela nove instance programa baze podataka SQL Azure.
* Instalirana i konfiguriran **Azure PowerShell** lokalno. Upute potražite u članku [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md).

**Podaci**: procesa migracije su planirati pomoću [NEW taksi skup podataka](http://chriswhong.com/open-data/foil_nyc_taxi/). Dataset taksi SEBI sadrži podatke o putovanja podataka i fairs te je dostupna, kao što je naznačeno taj unos na blobova platforme Azure: [NEW taksi podataka](http://www.andresmh.com/nyctaxitrips/). Uzorak i opis te datoteke nalaze se u [NEW taksi Trips Dataset opis](machine-learning-data-science-process-sql-walkthrough.md#dataset).
 
Možete prilagoditi postupke koje se ovdje opisuju skup vlastitim podacima ili slijedite korake opisan NEW taksi skupu podataka. Da biste prenijeli NEW taksi skupu podataka u bazu podataka sustava SQL Server na lokaciji, slijedite postupak za navedene u [Skupno uvoz podataka u bazu podataka sustava SQL Server](machine-learning-data-science-process-sql-walkthrough.md#dbload). Ove su upute su za SQL Server na programa Azure virtualnog računala, ali isti je postupak za prijenos sustava SQL Server na lokaciji.


## <a name="file-to-azure-sql-database"></a>Premještanje podataka s nehijerarhijskom datotekom izvora baze podataka Azure SQL

Podaci u plošnu datoteka (CSV ili TSV oblikovanu), možete premjestiti s bazom podataka Azure SQL pomoću masovnog Umetanje upita za SQL.

### <a name="bulk-insert-sql-query"></a>Masovno Umetanje SQL upita

Korake za postupak pomoću masovnog Umetanje SQL upita slični su onima u odjeljcima za premještanje podataka iz izvora nehijerarhijskom datotekom SQL Server na programa Azure VM. Detalje potražite u članku [Masovno Umetanje SQL upita](machine-learning-data-science-move-sql-server-virtual-machine.md#insert-tables-bulkquery).


##<a name="sql-on-prem-to-sazure-sql-database"></a>Premještanje podataka iz lokalnog sustava SQL Server s bazom podataka Azure SQL

Izvorišni podaci spremljeni u sustava SQL Server na lokaciji, postoje li razne mogućnosti za premještanje podataka s bazom podataka Azure SQL:

1. [Izvoz Nehijerarhijskom datotekom](#export-flat-file) 
2. [Čarobnjak za migraciju SQL baze podataka](#insert-tables-bcp)
3. [Baze podataka sigurnosne kopije i vraćanje](#db-migration)
4. [Tvorničke Azure podataka](#adf)

Koraci za prva tri vrlo su slične sekcijama [Premještanje](machine-learning-data-science-move-sql-server-virtual-machine.md) podataka za SQL Server na Azure virtualnog računala pokrivaju te iste postupke. Veza na odgovarajuće sekcije u toj temi navode se na sljedeće upute.

###<a name="export-flat-file"></a>Izvoz Nehijerarhijskom datotekom

Korake za ovaj izvoz s nehijerarhijskom datotekom slični su onima obuhvaćeno [Izvoz Nehijerarhijsku datoteku](machine-learning-data-science-move-sql-server-virtual-machine.md#export-flat-file).

###<a name="insert-tables-bcp"></a>Čarobnjak za migraciju SQL baze podataka

Upute za korištenje čarobnjaka za migraciju baze podataka SQL slični su onima u [Čarobnjak za migraciju SQL baze podataka](machine-learning-data-science-move-sql-server-virtual-machine.md#sql-migration).

###<a name="db-migration"></a>Baze podataka sigurnosne kopije i vraćanje

Upute za korištenje baze podataka sigurnosno kopiranje i vraćanje slični su onima obuhvaćeno [sigurnosno kopiranje i vraćanje baze podataka](machine-learning-data-science-move-sql-server-virtual-machine.md#sql-backup).

###<a name="adf"></a>Tvorničke Azure podataka

Postupak za premještanje podataka s bazom podataka Azure SQL s tvorničke Azure podataka (ADF) navedeni su u temi [Premještanje podataka iz lokalnog sustava SQL server u SQL Azure s tvorničke Azure podataka](machine-learning-data-science-move-sql-azure-adf.md). U ovoj se temi objašnjava premještanje podataka iz baze podataka sustava SQL Server na lokaciji s bazom podataka Azure SQL putem spremište blobova platforme Azure pomoću ADF. 

Razmislite o korištenju ADF kada podataka nije potrebno neprestano migrirati u scenariju s hibridnog pristupa resursima na lokaciji i oblaka i kada podaci se transakcije ili potrebno izmijeniti ili imaju poslovne logike u njega dodali kada migrira. ADF omogućuje za planiranje i praćenje zadataka pomoću jednostavne JSON skripte koji upravljaju premještanje podataka na temelju periodički. ADF ima i druge mogućnosti kao što je podrška za složene operacije.




