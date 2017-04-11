<properties
   pageTitle="Neprekidno poslovanje u oblaku - vraćanje izbrisane baze podataka – baze podataka SQL | Microsoft Azure"
   description="Saznajte više o Vrati točke u vrijeme koje omogućuje vam da biste vratili baze podataka SQL Azure prethodnog točke u vremenu (do 35 dana)."
   services="sql-database"
   documentationCenter=""
   authors="stevestein"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/01/2016"
   ms.author="sstein"/>

# <a name="recover-an-azure-sql-database-using-automated-database-backups"></a>Oporavak baze podataka Azure SQL pomoću sigurnosne kopije automatiziranog baze podataka

Baze podataka SQL pruža tri mogućnosti za oporavak baze podataka pomoću [baze podataka SQL automatizirano sigurnosno kopiranje](sql-database-automated-backups.md). Vraćanje baze podataka iz sigurnosne kopije pokrenut servis tijekom njihova [razdoblje zadržavanja](sql-database-service-tiers.md) za:

- Nove baze podataka na istom poslužitelju logičke oporaviti navedeni točku u vrijeme unutar razdoblje zadržavanja. 
- Baza podataka na istom poslužitelju logičke oporaviti brisanja vremena za izbrisanu bazu podataka.
- Nove baze podataka na bilo koje logičke poslužitelju u bilo kojem regiji oporaviti najnovije dnevnih kopija u zemlj replicirati blobova (RA GRS).

[Baze podataka SQL automatskog sigurnosnog kopiranja](sql-database-automated-backups.md) možete koristiti i da biste stvorili je [kopirajte bazu podataka](sql-database-copy.md) na bilo koje logičke poslužitelju u bilo kojem području koje je putem transakcije dosljedno trenutne baze podataka SQL. Kopiranje baze podataka i [Izvoz na BACPAC](sql-database-export.md) možete koristiti za arhiviranje putem transakcije dosljedan kopiju baze podataka za dugoročno spremanje izvan vaše razdoblje zadržavanja ili da biste prenijeli kopiju baze podataka u lokalnog ili Azure VM instancu sustava SQL Server.

## <a name="recovery-time"></a>Oporavak vremena

Nekoliko čimbenika utječe oporavku da biste vratili bazu podataka pomoću sigurnosnog kopiranja automatiziranog baze podataka: 
 - Veličina baze podataka
 - Razina performansi baze podataka
 - Broj zapisnicima transakcija koji je uključen
 - Iznos aktivnosti koje treba biti replayed da biste oporavili točku vraćanja
 - Ako je vraćanja na drugo područje propusnost mreže 
 - Broj Istodobni vraćanja zahtjeva koji se obrađuju u regiji cilj. 
 
 Za bazu podataka koja je vrlo velike i/ili aktivni vraćanja može potrajati nekoliko sati. Ako postoji tijekom duljih prekida u regiji, moguće je da će biti velikog broja zemlj vraćanja zahtjeve obrađuje u drugim regijama. Ako postoji velik broj zahtjeva za to povećati oporavku za baze podataka u tom području. Većina baza podataka vraća dovršeno u roku od 12 sati.

 Postoji nema ugrađene mogućnosti za skupno vraćanja. Na [baze podataka SQL Azure: potpuni oporavak poslužitelja](https://gallery.technet.microsoft.com/Azure-SQL-Database-Full-82941666) skripta je primjera jedan način obavljanju ovaj zadatak.

> [AZURE.IMPORTANT] Da biste oporavili pomoću automatskog sigurnosnog kopiranja, morate biti član uloga suradnika SQL Server u pretplatu ili biti vlasnik pretplate. Možete vratiti pomoću Azure portalu PowerShell ili REST API-JA. Ne možete koristiti Transact-SQL. 

## <a name="point-in-time-restore"></a>Vraćanje točke u vrijeme

Vraćanje točke u vrijeme omogućuje vam da biste vratili postojeću bazu podataka kao novu bazu podataka za ranije točke u vremenu na istom poslužitelju logičke pomoću [baze podataka SQL automatizirano sigurnosno kopiranje](sql-database-automated-backups.md). Postojeću bazu podataka ne može se prebrisati. Možete se vratiti na ranije točke u vremenu pomoću [portala za Azure](sql-database-point-in-time-restore-portal.md), [PowerShell](sql-database-point-in-time-restore-powershell.md) ili [REST API -JA](https://msdn.microsoft.com/library/azure/mt163685.aspx).

> [AZURE.SELECTOR]
- [Vrati točke u vrijeme: Portal za Azure](sql-database-point-in-time-restore-portal.md)
- [Vraćanje točke u vrijeme: PowerShell](sql-database-point-in-time-restore-powershell.md)

Baza podataka može vratiti na bilo kojem razina performansi ili elastic grupe aplikacija. Koje je potrebno provjeriti imate li dovoljno kvote DTU na logičke poslužitelju ili elastic skup. Imajte na umu li vraćanja stvara novu bazu podataka te sloju i performanse razina usluge vraćene baze podataka mogu se razlikovati od trenutnog stanja uživo baze podataka. Kada završi, vraćenu bazu podataka je običnom potpuno pristupiti online baza podataka naplaćuje u normalni stope na temelju njegove sloju i performanse razina usluge. Ne plaćati naknade za vraćanje baze podataka dok se ne dovrši.

Općenito Vraćanje baze podataka earler točku za oporavak. Kada to učinite, možete obradi vraćenu bazu podataka kao zamjenu za izvorne baze podataka ili ga koristiti za dohvaćanje podataka iz, a zatim ažurirajte izvorne baze podataka. 

- ***Baze podataka zamjena:*** Ako vraćenu bazu podataka je namijenjen kao zamjenu za izvorne baze podataka, trebali biste provjeriti razinom performansi i/ili servisa sloju odgovarajućih i promjena veličine baze podataka ako je potrebno. Možete preimenovati izvorne baze podataka i zatim izvorni naziv pomoću naredbe ALTER DATABASE u T-SQL dodijelite vraćenu bazu podataka. 
- ***Oporavak podataka:*** Ako planirate za dohvaćanje podataka iz vraćenu bazu podataka da biste oporavili od korisnika ili aplikacije pogreške, zasebno morat ćete pisanje i izvršiti bilo kakve skripte oporavak podatke potrebne za izdvajanje podataka iz vraćenu bazu podataka s izvornom bazom podataka. Iako se postupak vraćanja može potrajati da biste dovršili, vraćanje podataka baze podataka će biti vidljiv na popisu baza podataka tijekom. Ako izbrišete bazu podataka tijekom vraćanja, ona će odustati od operacije i vam se neće naplatiti za bazu podataka koju niste dovršili vraćanja. 

Detaljne informacije o korištenju točke u vrijeme Vrati se može oporaviti iz korisnika i pogreške aplikacija potražite u članku [točke-u – vrijeme Vrati](sql-database-recovery-using-backups.md#point-in-time-restore)

## <a name="deleted-database-restore"></a>Vraćanje izbrisane baze podataka

Vraćanje izbrisane baze podataka omogućuje vam da biste vratili izbrisanu bazu podataka na vrijeme brisanja izbrisane baze podataka na istom poslužitelju logičke pomoću [baze podataka SQL automatizirano sigurnosno kopiranje](sql-database-automated-backups.md). 

> [AZURE.IMPORTANT] Ako izbrišete instanca poslužitelja baze podataka SQL Azure, sve njegove baze podataka brišu se i i nije moguć. Ne postoji podrška za vraćanje izbrisanih poslužitelj trenutačno.

Možete koristiti isti ili novi naziv baze podataka za vraćenu bazu podataka. Možete koristiti [portal za Azure](sql-database-restore-deleted-database-portal.md), [PowerShell](sql-database-restore-deleted-database-powershell.md) ili [OSTALE (createMode = vraćanja)](https://msdn.microsoft.com/library/azure/mt163685.aspx). 

> [AZURE.SELECTOR]
- [Izbrisane Vraćanje baze podataka: portal za Azure](sql-database-restore-deleted-database-portal.md)
- [Izbrisane Vraćanje baze podataka: PowerShell](sql-database-restore-deleted-database-powershell.md)

## <a name="geo-restore"></a>Vraćanje za zemlj.

Zemlj vraćanja omogućuje vam da biste vratili bazi podataka sustava SQL na bilo kojem poslužitelju u bilo kojem Azure regiji iz najnovije zemlj replicirati [automatiziranog dnevna sigurnosna kopija](sql-database-automated-backups.md). Zemlj vraćanja koristi zemlj suvišnih sigurnosnu kopiju kao izvor i može se koristiti za oporavak baze podataka, čak i ako se ne može pristupiti zbog do prekida bazu podataka ili podatkovnog centra. Možete koristiti [portal za Azure](sql-database-geo-restore-portal.md), [PowerShell](sql-database-geo-restore-powershell.md)ili [OSTALE (createMode = oporavak)](https://msdn.microsoft.com/library/azure/mt163685.aspx) 

> [AZURE.SELECTOR]
- [Vrati zemlj.: Portal za Azure](sql-database-geo-restore-portal.md)
- [Zemlj Vrati: PowerShell](sql-database-geo-restore-powershell.md)

Zemlj vraćanja je zadana mogućnost oporavka kada bazu podataka nije dostupan zbog incident u području koje se hostira baze podataka. Ako velike skaliranje incident u regiji rezultate u nedostupnosti aplikacije za baze podataka, poslužite se zemlj Vrati da biste vratili bazu podataka iz najnovija sigurnosna kopija na poslužitelju u druga regija. Sve sigurnosne kopije su zemlj replicirati, a mogu sadržavati razmak između kada je sigurnosne kopije snimanja i zemlj replicirati na na Azure blob u nekoj drugoj regiji. Odgode može biti jedan sat da bi se slučaju na Izrada može se na 1 sat gubitka podataka ubuduće, odnosno RPO do 1 sat. Na sljedećoj je slici prikazan Vraćanje baze podataka iz zadnjeg dnevnih sigurnosne kopije.

![Vraćanje za zemlj.](./media/sql-database-geo-restore/geo-restore-2.png)

Detaljne informacije o korištenju zemlj Vrati se može oporaviti iz programa nedostupnosti potražite u članku [oporavak iz programa prekida](sql-database-disaster-recovery.md)

> [AZURE.IMPORTANT] Dok zemlj vraćanja dostupan je sa svim razine servisa, je najčešće osnovni rješenja oporavak za Izrada u SQL baze podataka pomoću najdulje RPO i procjenu oporavak vrijeme (UMETNI). Za osnovni baze podataka s maksimalne veličine 2 GB zemlj.-vraćanja nudi pametnije DR rješenje s programa UMETNI 12 sati. Za veće standardni prikaz ili Premium baze podataka, ako znatno kraće vrijeme oporavak su želji ili da biste smanjili vjerojatnost od gubitka podataka razmislite o korištenju aktivni replikacijom zemlj.. Aktivni replikacije zemlj nudi mnogo donjem RPO i UMETNI kao što je potrebno je samo pokretanje prebacivanje da biste kontinuirano repliciranu sekundarni. Detalje potražite u članku [Aktivni replikacije zemlj.](sql-database-geo-replication-overview.md).

## <a name="programmatically-performing-recovery-using-automated-backups"></a>Programsko izvođenje oporavak pomoću automatskog sigurnosnog kopiranja

Kako je opisano iznad, u addiition Azure portal oporavak baze podataka mogu izvršiti programmically pomoću komponente PowerShell Azure i REST API-JA. U tablicama u nastavku opisuju postavljanje dostupne naredbe.

### <a name="powershell"></a>PowerShell

|Cmdlet|Opis|
|------|-----------|
|[Get-AzureRmSqlDatabase](https://msdn.microsoft.com/en-us/library/azure/mt603648.aspx)|Dohvaća jedan ili više baza podataka.|
|[Get-AzureRMSqlDeletedDatabaseBackup](https://msdn.microsoft.com/en-us/library/azure/mt693387.aspx)|Dohvaća izbrisanu bazu podataka možete vratiti.|
|[Get-AzureRmSqlDatabaseGeoBackup](https://msdn.microsoft.com/library/azure/mt693388.aspx)|Dohvaća zemlj suvišnih sigurnosnu kopiju baze podataka.|
|[Vraćanje AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt693390.aspx)|Vraća SQL baze podataka.|
||||

### <a name="rest-api"></a>REST API-JA

|API-JA|Opis|
|---|-----------|
|[OSTALE (createMode = oporavak)](https://msdn.microsoft.com/library/azure/mt163685.aspx)|Vraća baze podataka|
|[Početak stvaranje ili ažuriranje stanje baze podataka](https://msdn.microsoft.com/library/azure/mt643934.aspx)|Vraća status tijekom operacije vraćanja|
||||



## <a name="summary"></a>Sažetak

Automatsko sigurnosno kopiranje zaštiti vaših baza podataka od korisnika i pogrešaka u aplikaciji, brisanja slučajne baze podataka i tijekom duljih kvarove. Rješenje nula administratori nula trošak dostupan je sa sve baze podataka SQL. 

## <a name="next-steps"></a>Daljnji koraci

- Pregled za continuity tvrtke i scenariji potražite u članku [Pregled continuity tvrtke](sql-database-business-continuity.md)
- Da biste saznali više o sigurnosne kopije baze podataka SQL Azure automatski, potražite u članku [baze podataka SQL automatskog sigurnosnog kopiranja](sql-database-automated-backups.md)
- Da biste saznali više o brže mogućnosti za oporavak, potražite u članku [Aktivno zemlj replikacije](sql-database-geo-replication-overview.md)  
- Dodatne informacije o korištenju automatizirano sigurnosno kopiranje za arhiviranje, pročitajte članak [kopiranje baze podataka](sql-database-copy.md)
