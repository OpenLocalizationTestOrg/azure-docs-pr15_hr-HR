<properties
   pageTitle="Ograničenja kapaciteta SQL Data Warehouse | Microsoft Azure"
   description="Maksimalna vrijednost za veze, baze podataka, tablice i upite za SQL Data Warehouse."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/01/2016"
   ms.author="sonyama;barbkess;jrj"/>

# <a name="sql-data-warehouse-capacity-limits"></a>Ograničenja SQL Data Warehouse kapaciteta

Sljedeća tablica sadrži maksimalne vrijednosti za razne komponente Azure SQL Data Warehouse dopuštene.


## <a name="workload-management"></a>Upravljanje radno opterećenje

| Kategorija            | Opis                                       | Najviše            |
| :------------------ | :------------------------------------------------ | :----------------- |
| [Jedinice skladištu podataka (DWU)][]| Max DWU za jednu SQL Data Warehouse | 6000               |
| [Jedinice skladištu podataka (DWU)][]| Max DWU za jednu SQL server         | 6000 prema zadanim postavkama<br/><br/> Prema zadanim postavkama svaki SQL server (npr. myserver.database.windows.net) ima DTU kvote 45,000 koji omogućuje do 6000 DWU. U ovom kvote je jednostavno ograničenje sigurnost. Možete povećati kvotu [Stvaranje zahtjev za podršku možete][] i odabirom *kvote* kao vrstu zahtjev.  Da biste izračunali vaše DTU mora, pomnožite s 7.5 Ukupno DWU potrebne. Trenutni DTU potrošnje iz Elektronička ploča poslužitelja SQL možete pogledati na portalu. Baza podataka za privremeno i nepotvrđen pauzirano Brojanje prema DTU kvote. |
| Povezivanje s bazom podataka | Istodobni otvorenih sesija                          | 1024<br/><br/>Podržavamo maksimalno aktivne veze 1024, od kojih svaka Pošalji zahtjeve za baze podataka SQL Data Warehouse u isto vrijeme. Imajte na umu da postoje ograničenja broja upite koji se može zapravo istovremeno. Kada se prekorači ograničenje istodobnosti, zahtjev prelazi u interni reda čekanja gdje čeka obraditi.|
| Povezivanje s bazom podataka | Maksimalna memorija za pripremljeni izvješća            | 20 MB              |
| [Upravljanje radno opterećenje][] | Najveći broj Istodobni upita                    | 32<br/><br/> Prema zadanim postavkama SQL Data Warehouse možete izvršiti najviše 32 Istodobni upita i redova preostalo upita.<br/><br/>Razina istodobnosti može smanjiti kada korisnicima dodjeljuje se na višu predmete resursa ili SQL Data Warehouse je konfiguriran za korištenje niskog DWU. Neki upiti, kao što su upiti DMV uvijek se mogu pokretati.|
| [Tempdb][]          | Maksimalna veličina Tempdb                                | 399 GB po DW100. Stoga pri DWU1000 Tempdb veličinom odgovara 3,99 TB |


## <a name="database-objects"></a>Objekti baze podataka

| Kategorija          | Opis                                  | Najviše            |
| :---------------- | :------------------------------------------- | :----------------- |
| Baze podataka          | Maksimalna veličina                                     | 240 TB spojene na disku<br/><br/>Taj prostor ne ovisi o tempdb ili zapisnika prostora i stoga se taj prostor predano trajna tablice.  Sažimanje grupirani columnstore je očekivano na 5 X.  Ovaj sažimanja omogućuje baze podataka radi povećanja približno 1 PB kada su grupirani columnstore (zadanu tablicu vrstu) sve tablice.|
| Tablica             | Maksimalna veličina                                     | 60 TB spojene na disku   |
| Tablica             | Tablica po bazi podataka                          | milijarde 2          |
| Tablica             | Stupaca po tablici                            | 1024 stupaca       |
| Tablica             | Bajtova po stupcu                             | Zavisne u stupcu [Vrsta podataka][].  Ograničenje je 8000 za vrste podataka char 4000 za nvarchar ili 2 GB za vrste podataka MAX.|
| Tablica             | Bajta po retku definirani veličina                  | 8060 bajtova<br/><br/>Broja bajtova po retku izračunava se na isti način kao i za SQL Server s sažimanja stranice. Kao što je SQL Server SQL Data Warehouse podržava retka prelijevanja prostora za pohranu koji omogućuje **dužine varijable stupaca** tako da se pomiču isključivanje retka. Kada se reci dužine varijable pomiču se isključivanje retka, samo 24-bajtni korijenski sprema se u glavni zapis. Dodatne informacije potražite u članku MSDN [retka višak podataka premašuju 8 KB][] .|
| Tablica             | Particije po tablici                    | 15.000<br/><br/>Visoke performanse, preporučujemo minimiziranje broj razdjeljivanja potrebno dok još uvijek podrške s poslovnim potrebama. Broj particije rastom indirektnog za podataka definicija jezik (DDL) i podataka rukovanje jezik (DML) operacija rastom i uzrokuje pad performansi.|
| Tablica             | Znakova po vrijednosti granicu particije.| 4000 |
| Indeks             | Osobe koje nisu grupirani indeksi po tablici.        | 999<br/><br/>Odnosi se samo rowstore tablice.|
| Indeks             | Grupirani indeksi po tablici.            | 1<br><br/>Odnosi se na tablice rowstore i columnstore.|
| Indeks             | Indeksirajte veličina ključa.                          | 900 bajtova.<br/><br/>Odnosi se samo rowstore indeksi.<br/><br/>Indeksi stupaca varchar s maksimalne veličine više od 900 bajta moguće je ako postojeće podatke u stupcima nije veća od 900 bajta stvaranja indeksa. No kasnije UMETNUTI ili ažuriranje akcije na stupce koji uzrokuju ukupnu veličinu smije biti veća od 900 bajta neće uspjeti.|
| Indeks             | Ključni stupaca po indeksa.                   | 16<br/><br/>Odnosi se samo rowstore indeksi. Indeksi grupirani columnstore obuhvaćaju sve stupce.|
| Statistika        | Veličina spojeni stupac vrijednosti.      | 900 bajtova.         |
| Statistika        | Stupci po Statistika objektu.           | 32                 |
| Statistika        | Statistika stvoreno stupaca po tablici. | 30.000            |
| Pohranjene procedure | Maksimalan broj razina gniježđenja.               | 8                 |
| Prikaz              | Stupce po prikazu                         | 1024             |


## <a name="loads"></a>Opterećenje

| Kategorija          | Opis                                  | Najviše            |
| :---------------- | :------------------------------------------- | :----------------- |
| Opterećenje Polybase    | Bajtova po retku                                | 32.768<br/><br/>Opterećenje Polybase ograničeni su na manje od 32K učitavanja redaka oba, a ne može se učitati VARCHR(MAX), NVARCHAR(MAX) ili VARBINARY(MAX).  Dok je ograničenje postoji danas, ona će biti uklonjena prilično uskoro.<br/><br/>


## <a name="queries"></a>Upiti

| Kategorija          | Opis                                  | Najviše            |
| :---------------- | :------------------------------------------- | :----------------- |
| Upit             | U redu čekanja upita na tablicama korisnika.               | 1000               |
| Upit             | Istodobni upite na Sistemski prikazi.          | 100                |
| Upit             | U redu čekanja upite na Sistemski prikazi               | 1000               |
| Upit             | Maksimalna parametara                           | 2098               |
| Grupe             | Maksimalna veličina                                 | 65, 536 * 4096        |
| Odabir rezultata    | Stupaca u retku                              | 4096<br/><br/>Nikad ne mogu imati više od 4096 stupaca po retku u rezultatu odaberite. Nema jamstva možete uvijek imate 4096. Ako tarifa upit traži privremena, 1024 stupaca po tablici Maksimalna naplata.|
| ODABERITE            | Ugniježđene podupiti                            | 32<br/><br/>Nikad ne mogu imati više od 32 ugniježđene podupiti u naredbi SELECT. Nema jamstva možete uvijek imate 32. Ako, na primjer, SPOJ može uzrokovati podupita u plan upita. Broj podupiti također može biti Ograničeno dostupnom memorijom.|
| ODABERITE            | Stupaca po SPOJ                             | 1024 stupaca<br/><br/>Nikad ne mogu imati više od 1024 stupaca u SPOJA. Nema jamstva možete uvijek imate 1024. Ako SPOJ plan zahtijeva privremene tablice s više stupaca od rezultat SPOJ, 1024 ograničenje se primjenjuje na privremena tablica. |
| ODABERITE            | Bajta po GRUPIRAJ po stupcima.                  | 8060<br/><br/>Stupci u uvjet GROUP BY može sadržavati najviše 8060 bajtova.|
| ODABERITE            | Bajta po stupcima ORDER BY                   | 8060 bajtova.<br/><br/>Stupci u uvjet ORDER BY može sadržavati najviše 8060 bajtova.|
| Identifikatori i konstante po izjava | Broj referencirani identifikatora i konstante. | 65.535<br/><br/>SQL Data Warehouse ograničava broj identifikatora i konstantama koje se mogu nalaziti u jednom izraza u upitu. To ograničenje je 65.535. Prekoračenje broja rezultira SQL Server pogreške 8632. Dodatne informacije potražite u članku [Interna pogreška: do ograničenja usluga izraz][].|


## <a name="metadata"></a>Metapodataka

| Prikaz sustava                        | Maksimalan broj redaka |
| :--------------------------------- | :------------|
| sys.dm_pdw_component_health_alerts | 10 000       |
| sys.dm_pdw_dms_cores               | 100          |
| sys.dm_pdw_dms_workers             | Ukupan broj DMS zaposlenici zaduženi za zadnjih 1000 SQL zahtjeve. |
| sys.dm_pdw_errors                  | 10 000       |
| sys.dm_pdw_exec_requests           | 10 000       |
| sys.dm_pdw_exec_sessions           | 10 000       |
| sys.dm_pdw_request_steps           | Ukupan broj korake za najnovije zahtjeve SQL 1000 spremljene u sys.dm_pdw_exec_requests. |
| sys.dm_pdw_os_event_logs           | 10 000       |
| sys.dm_pdw_sql_requests            | Najnovije 1000 SQL zahtjeve spremljene u sys.dm_pdw_exec_requests. |


## <a name="next-steps"></a>Daljnji koraci
Dodatne informacije za referencu, potražite u članku [Pregled SQL Data Warehouse referenca][].

<!--Image references-->

<!--Article references-->
[Jedinice skladištu podataka (DWU)]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[Pregled SQL Data Warehouse referenca]: ./sql-data-warehouse-overview-reference.md
[Upravljanje radno opterećenje]: ./sql-data-warehouse-develop-concurrency.md
[Tempdb]: ./sql-data-warehouse-tables-temporary.md
[Vrsta podataka]: ./sql-data-warehouse-tables-data-types.md
[Stvaranje zahtjev za podršku možete]: /sql-data-warehouse-get-started-create-support-ticket.md

<!--MSDN references-->
[Višak redak podataka premašuju 8 KB]: https://msdn.microsoft.com/library/ms186981.aspx
[Interna pogreška: do servisa ograničenja izraza]: https://support.microsoft.com/kb/913050
