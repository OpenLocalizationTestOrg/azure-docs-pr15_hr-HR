<properties
   pageTitle="Sigurnosno kopiranje SQL Data Warehouse | Microsoft Azure"
   description="Saznajte više o sigurnosne kopije ugrađene baze podataka SQL Data Warehouse koji omogućuju vam da biste vratili programa Azure SQL Data Warehouse točku vraćanja ili drugu zemljopisnom području."
   services="sql-data-warehouse"
   documentationCenter=""
   authors="lakshmi1812"
   manager="barbkess"
   editor="monicar"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/06/2016"
   ms.author="lakshmir;barbkess"/>

# <a name="sql-data-warehouse-backups"></a>SQL Data Warehouse sigurnosne kopije

SQL Data Warehouse nudi lokalne i zemljopisnim sigurnosne kopije kao dio njegove sigurnosne mogućnosti skladištu podataka. Uključuju snimke blobova platforme Azure prostora za pohranu i zemlj suvišnih prostora za pohranu. Pomoću sigurnosnih kopija skladištu podataka da biste vratili skladištu podataka točku vraćanja u području primarni ili vratiti različite zemljopisnom području. U ovom se članku objašnjava specifičnosti kopija u SQL Data Warehouse.

## <a name="what-is-a-data-warehouse-backup"></a>Što je skladištu sigurnosnu kopiju podataka?

Sigurnosnu kopiju podataka skladištu se nalaze podaci koje možete koristiti da biste vratili podataka skladištu određeno vrijeme.  Budući da SQL Data Warehouse je raspodijeljeno sustava, sigurnosnu kopiju podataka skladištu sastoji se od mnogo datoteka koje se pohranjuju u Azure blob-ova. 

Sigurnosne kopije baze podataka su ključni dio sve poslovne continuity i Izrada strategije oporavak jer zaštita podataka iz slučajne oštećenja ili brisanja. Dodatne informacije potražite u članku [Pregled continuity tvrtke](../sql-database/sql-database-business-continuity.md).

## <a name="data-redundancy"></a>Redundanciju podataka jer

SQL Data Warehouse štiti podatke spremanjem podataka u lokalno suvišnih (LRS) Azure Premium prostora za pohranu. Značajka za pohranu Azure pohranjuje većeg broja primjeraka sinkrono podataka u centru za lokalnih podataka da bi zaštita prozirne podataka ako su lokalizirane pogreške. Redundanciju podataka jer omogućuje čitanje podataka možete preživi infrastrukture probleme kao što su pogreške na disku. Redundanciju podataka jer osigurava poslovanje s kvara koji je pogreške i vrlo dostupna infrastrukture.

Da biste saznali više o:

- Azure Premium prostor za pohranu, pročitajte članke [Uvod u Azure Premium prostora za pohranu](../storage/storage-premium-storage.md).
- Lokalno suvišnih prostora za pohranu potražite u članku [replikacije Azure prostora za pohranu](../storage/storage-redundancy.md#locally-redundant-storage).


## <a name="azure-storage-blob-snapshots"></a>Azure spremišta blobova snimke

SQL Data Warehouse kao prednosti korištenja Azure Premium prostora za pohranu, koristi snimke blobova platforme Azure prostora za pohranu za sigurnosno kopiranje podataka skladištu lokalno. Možete vratiti podatke skladištu točku vraćanja snimke. Brze snimke pokrenite najmanje svakih osam sati i dostupne su za sedam dana.  

Da biste saznali više o:

- Snimke blobova platforme Azure potražite u članku [Stvaranje blob snimke](../storage/storage-blob-snapshots.md).


## <a name="geo-redundant-backups"></a>Kopija suvišnih zemlj.

Svaka 24 sata, SQL Data Warehouse pohranjuje skladištu svih podataka u standardne prostora za pohranu. Za podudara s vremenom zadnje snimke stvoren je u skladištu svih podataka. Standardni prostora za pohranu pripada zemlj suvišnih prostora za pohranu račun s pristupom čitanja (RA GRS). Značajka Azure RA-GRS replicira datoteke sigurnosne kopije u [paru podatkovnog centra](../best-practices-availability-paired-regions.md). U ovom zemlj replikacije osigurava skladištu podataka možete vratiti u slučaju da ne možete pristupiti snimki u vašoj regiji primarni. 

Ta značajka po zadanom uključena. Ako ne želite koristiti zemlj suvišnih sigurnosne kopije, koje možete odustati. 

>[AZURE.NOTE] U Azure spremište termina *replikacije* se odnosi na kopiranje datoteka s jednog mjesta na drugo. SQL- *replikacije baze podataka* se odnosi na čuvanje više sekundarne baza podataka sinkronizira s primarnom bazom podataka. 

Da biste saznali više o:
- Zemlj suvišnih prostora za pohranu potražite u članku [replikacije Azure prostora za pohranu](../storage/storage-redundancy.md).
- Prostor za pohranu RA GRS, potražite u članku [zemlj suvišnih pohranom pristup za čitanje](../storage/storage-redundancy.md#read-access-geo-redundant-storage).

## <a name="data-warehouse-backup-schedule-and-retention-period"></a>Podaci skladištu raspored sigurnosnog kopiranja i razdoblje zadržavanja

SQL Data Warehouse stvara snimaka na mreži podataka skladištima svakih četiri do osam sati i zadržava svaki snimke sedam dana. Možete vratiti bazu podataka na mreži jedne od točaka vraćanja u zadnjih sedam dana. 

Da biste vidjeli nakon pokretanja zadnje snimke, pokrenite ovaj upit na mreži Data Warehouse SQL. 

```sql
select top 1 *
from sys.pdw_loader_backup_runs 
order by run_id desc;
```

Ako morate zadržati snimku dulje od sedam dana, novi skladištu podataka možete vratiti točku vraćanja. Nakon dovršetka vraćanja SQL Data Warehouse pokreće stvaranja brze snimke na skladištu nove podatke. Ako ne mijenjate skladištu nove podatke, snimki ostati prazan i stoga je minimalna trošak snimke. Nije moguće zaustaviti i baze podataka da biste spriječili SQL Data Warehouse stvaranja brze snimke.


### <a name="what-happens-to-my-backup-retention-while-my-data-warehouse-is-paused"></a>Što se događa moje sigurnosne kopije zadržavanja dok je pauziran skladištu moje podatke?

SQL Data Warehouse stvaranje snimaka i istječe snimke dok je pauziran podataka skladištu. Snimka dob ne mijenja dok je pauziran skladištu podataka. Snimka zadržavanja temelji se na broj dana u skladištu podataka je na mreži, ne dana u kalendaru.

Ako, na primjer, ako snimka započinje listopad 1 4 pm i skladištu podataka je pauziran listopad 3 pri 4 pm, snimka je dva dana. Kad god skladištu podataka dolazi vratite u mrežni snimku nije dva dana. Ako u skladištu podataka dolazi online listopad 5 pri 4 pm, snimka dva dana i ostaje pet više dana.

Kada se vratite u mrežni skladištu podataka, SQL Data Warehouse primijenila nova snimke i istekne snimke ako imaju više od sedam dana podataka.

### <a name="how-long-is-the-retention-period-for-a-dropped-data-warehouse"></a>Koliko je razdoblje zadržavanja za izostavljanje podataka skladištu?
Kada se prekine podataka skladištu, skladištu podataka i snimki spremili sedam dana i zatim uklanjaju. Skladištu podataka možete vratiti na bilo koju od točke spremljenu vraćanja.

> [AZURE.IMPORTANT] Ako ste izbrisali logički instanca SQL poslužitelja, sve baze podataka koji pripadaju instanci brišu se i i nije moguć. Nije moguće vratiti izbrisane poslužitelja.

## <a name="data-warehouse-backup-costs"></a>Sigurnosno kopiranje troškove u skladištu podataka

Ukupni trošak skladištu primarni podataka i sedam dana blobova platforme Azure snimke se zaokružuje na najbliži TB. Ako, na primjer, ako skladištu podataka je 1,5 TB te snimki koristiti 100 GB, se naplatiti za 2 TB brzina stope Azure Premium prostora za pohranu. 

>[AZURE.NOTE] Svaki snimke prethodno je prazan, a rastom dok mijenjate skladištu primarni podataka. Sve snimke povećavati kao skladištu promjene podataka. Stoga troškove prostora za pohranu za snimke Povećaj prema stopa promjene.

Ako koristite zemlj suvišnih prostora za pohranu, primit ćete naknadu zasebnom prostora za pohranu. Prostor za pohranu suvišnih zemlj naplate pri standardna satnica pristup za čitanje geografski suvišnih prostora za pohranu (RA GRS).

Dodatne informacije o cijenama SQL Data Warehouse potražite u članku [SQL podataka skladištu cijene](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).

## <a name="using-database-backups"></a>Pomoću sigurnosne kopije baze podataka

Prvenstveno se koristi za sigurnosne kopije skladištu podataka za SQL je da biste vratili skladištu podataka jedne od točaka vraćanja unutar razdoblje zadržavanja.  

- Da biste vratili SQL warehouse podataka potražite u članku [Vraćanje SQL warehouse podataka](sql-data-warehouse-restore-database-overview.md).


## <a name="related-topics"></a>Povezane teme

### <a name="scenarios"></a>Scenariji

- Pregled continuity tvrtke, potražite u članku [Pregled continuity tvrtke](../sql-database/sql-database-business-continuity.md)


<!-- ### Tasks -->

- Da biste vratili podataka skladištu, potražite u članku [Vraćanje SQL warehouse podataka](sql-data-warehouse-restore-database-overview.md)

<!-- ### Tutorials -->

