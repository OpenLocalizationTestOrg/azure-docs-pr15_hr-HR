<properties
   pageTitle="Uvod u vremenski tablica u bazi podataka Azure SQL | Microsoft Azure"
   description="Saznajte kako započeti s korištenjem vremenski tablica u bazi podataka SQL Azure."
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sql-database"
   ms.date="08/29/2016"
   ms.author="carlrab"/>

#<a name="getting-started-with-temporal-tables-in-azure-sql-database"></a>Uvod u vremenski tablica u bazi podataka Azure SQL

Vremenski tablice su nove značajke programibilnosti baze podataka SQL Azure koja omogućuje praćenje i analizirati potpunu povijest promjena u podacima, bez potrebe za prilagođeni kod. Vremenski tablice zadržati podatke usko povezan kontekst vrijeme tako da ga je protumačiti pohranjene podatke koji kao valjan samo unutar određenog razdoblja. Ovo svojstvo vremenski tablica omogućuje učinkovito temeljene analizu i dohvaćanje uvida s podacima proizvod.

##<a name="temporal-scenario"></a>Vremenski scenarija

U ovom se članku prikazuje upute za korištenje vremenski tablice u scenariju u aplikaciji. Pretpostavimo da želite pratiti aktivnosti korisnika na novo web-mjesto koje je razvijaju od nule ili na postojeće web-mjesto koje želite proširiti s analize aktivnosti korisnika. U ovom primjeru pojednostavljeni smo pretpostavlja da je broj posjećenih web-stranica tijekom vremenskog razdoblja pokazatelja koju je potrebno bilježe i nadzirati u bazi podataka za web-mjesta koja se nalazi na baze podataka SQL Azure. Cilj povijesne analize korisničke aktivnosti jest da biste dobili unosa njime mijenjajte dizajn web-mjesta i posjetitelji omogućuju bolje iskustvo.

Model baze podataka za taj scenarij je vrlo jednostavne – metriku aktivnosti korisnika predstavlja s poljem jedan cijeli broj, **PageVisited**i se hvata uz osnovne informacije o korisničkom profilu. Uz to, za analizu vremena činite zadržite nizu redaka za svakog korisnika, pri čemu svaki redak predstavlja broj stranica određenom korisniku posjetili unutar određenog vremenskog razdoblja.

![Shema](./media/sql-database-temporal-tables/AzureTemporal1.png)

Srećom, ne morate staviti sve trud u aplikaciji za održavanje ove informacije aktivnosti. Vremenski tablicama, automatski se taj postupak - što vam omogućuje potpunu fleksibilnost tijekom dizajniranje web-mjesta i još jedanput radi fokusiranja na analiza podataka sam. Samo što morate učiniti je da biste bili sigurni **WebSiteInfo** tablice konfigurirana kao [vremenski sustava određuju](https://msdn.microsoft.com/library/dn935015.aspx#Anchor_0). Slijedi opis točne korake za korištenje vremenski tablice u ovom scenariju.

##<a name="step-1-configure-tables-as-temporal"></a>Korak 1: Konfiguriranje tablice kao vremenski

Ovisno o tome pokretanje novog razvoj ili nadogradnja postojeće aplikacije, će stvoriti vremenski tablice ili izmijeniti postojeće dodavanjem vremenski atribute. Općenito govoreći slučaja, scenariju može biti kombinacije ove dvije mogućnosti. Izvođenje sljedeće radnje pomoću [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) (SSMS), [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) (SSDT) ili drugi alat razvoj Transact-SQL.


> [AZURE.IMPORTANT] Preporučuje se da uvijek koristite najnoviju verziju Management Studio bude sinkroniziranim ažuriranjima za Microsoft Azure i SQL baze podataka. [Ažuriranje SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


###<a name="create-new-table"></a>Stvaranje nove tablice

Pomoću stavku izbornika konteksta "Nove sustava određuju tablice" u programu Explorer SSMS objekt s skriptu predložak vremenski tablice otvorite uređivač upita, a zatim pomoću "Odredite vrijednosti za parametri predložaka" (Ctrl + Shift + M) za popunjavanje predložak:

![SSMSNewTable](./media/sql-database-temporal-tables/AzureTemporal2.png)

U SSDT, odaberite predložak "Vremenski tablice (sustav-određuju)" prilikom dodavanja nove stavke u projekt baze podataka. Koje će se otvoriti dizajnera tablice i omogućuju vam da biste lakše odredili izgled tablice:

![SSDTNewTable](./media/sql-database-temporal-tables/AzureTemporal3.png)

Možete i stvaranje tablica za vremenski navođenjem Transact-SQL naredbe izravno, kao što je prikazano u primjeru u nastavku. Imajte na umu da obavezna elementima svakom tablicom vremenski definiciju RAZDOBLJA i uvjet SYSTEM_VERSIONING s adresom u drugu tablicu korisnika koja će sadržavati verzije povijesne redak:

````
CREATE TABLE WebsiteUserInfo 
(  
    [UserID] int NOT NULL PRIMARY KEY CLUSTERED 
  , [UserName] nvarchar(100) NOT NULL
  , [PagesVisited] int NOT NULL 
  , [ValidFrom] datetime2 (0) GENERATED ALWAYS AS ROW START
  , [ValidTo] datetime2 (0) GENERATED ALWAYS AS ROW END
  , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)
 )  
 WITH (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.WebsiteUserInfoHistory));
````

Kada stvorite sustava određuju vremenski tablice, automatski se stvara popratnu povijest tablice sa zadanom konfiguracijom. Zadane tablice povijest sadrži grupni indeks B stabla na stupcima razdoblja (end start) s stranice sažimanja omogućena. Tu konfiguraciju je optimalnih Većina slučajevi u kojima vremenski tablice koriste, posebice za [nadzor podataka](https://msdn.microsoft.com/library/mt631669.aspx#Anchor_0). 

U ovom slučaju, ne možemo usmjerite tema temeljene trend putem dulje povijesti podataka te s veći skupovima podataka, pa je izbor za pohranu za tablici povijesti grupirani columnstore indeksa. Grupirani columnstore nudi odlično sažimanja i performanse analytical upita. Vremenski tablice što je fleksibilnosti da biste konfigurirali indeksi tablice trenutni i vremenski potpuno zasebno. 

**Napomena**: Columnstore indeksi dostupni su samo u sloju servisa premium.

Sljedeću skriptu prikazuje kako se zadani indeks na tablici povijesti možete promijeniti da biste grupirani columnstore:

````
CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory
ON dbo.WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON); 
````

Vremenski tablice prikazane su u programu Explorer objekt s određenim ikonom za lakše identifikacijski tijekom njegova povijest tablice prikazuje se kao podređeni čvor.

![AlterTable](./media/sql-database-temporal-tables/AzureTemporal4.png)

###<a name="alter-existing-table-to-temporal"></a>Promijeniti postojeću tablicu za vremenski

Pogledajmo pokrivaju zamjenski scenarij u kojem tablici WebsiteUserInfo već postoji, ali nije namijenjena za čuvati povijest promjena. U tom slučaju možete jednostavno proširiti postojećoj tablici postati vremenski, kao što je prikazano u sljedećem primjeru:

````
ALTER TABLE WebsiteUserInfo 
ADD 
    ValidFrom datetime2 (0) GENERATED ALWAYS AS ROW START HIDDEN  
        constraint DF_ValidFrom DEFAULT DATEADD(SECOND, -1, SYSUTCDATETIME())
    , ValidTo datetime2 (0)  GENERATED ALWAYS AS ROW END HIDDEN   
        constraint DF_ValidTo DEFAULT '9999.12.31 23:59:59.99'
    , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo); 

ALTER TABLE WebsiteUserInfo  
SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.WebsiteUserInfoHistory));
GO

CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory
ON dbo.WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON); 
````

##<a name="step-2-run-your-workload-regularly"></a>Korak 2: Redovno svoje radno opterećenje

Glavni prednost vremenski tablice jest da nije potrebno da biste promijenili ili Prilagodba web-mjesta na bilo koji način za izvođenje evidentiranje promjena. Nakon stvaranja, vremenski tablice proziran zadržava prethodne verzije redak svaki put kada se izmjene izvedete podataka. 

Da bi se pod utjecajem automatske promjene evidentiranja scenarij određeni, recimo samo ažuriranja stupca **PagesVisited** svaki put kad korisnik završava her/his sesiju na web-mjestu:

````
UPDATE WebsiteUserInfo  SET [PagesVisited] = 5 
WHERE [UserID] = 1;
````

Važno je da obratite pozornost na to da upit s ažuriranjem ne morate znati točno vrijeme kada je došlo je do stvarnu operaciju ni kako proteklih podataka će se sačuvati za buduće analize. Oba aspekte automatski će se rukovati iz baze podataka SQL Azure. Sljedeći dijagram prikazuje kako se generira povijest podataka na svakog ažuriranja.

![TemporalArchitecture](./media/sql-database-temporal-tables/AzureTemporal5.png)

##<a name="step-3-perform-historical-data-analysis"></a>Korak 3: Izvođenje povijesne analize

Sada kada je omogućen vremenski verzija sustava, povijesne analize je upit samo jednu od vas. U ovom se članku ćemo vam ponuditi nekoliko primjera tu adresu uobičajeni analizu scenariji – Saznajte sve detalje, istražili razne mogućnosti uvodi uvjet [Za SYSTEM_TIME](https://msdn.microsoft.com/library/dn935015.aspx#Anchor_3) .

Da biste vidjeli vodećih korisnika 10 naručili prema broju posjećenih web-stranica na jedan sat prije, pokrenite ovaj upit:

````
DECLARE @hourAgo datetime2 = DATEADD(HOUR, -1, SYSUTCDATETIME());
SELECT TOP 10 * FROM dbo.WebsiteUserInfo FOR SYSTEM_TIME AS OF @hourAgo
ORDER BY PagesVisited DESC
````

Možete jednostavno izmijeniti ovaj upit da biste analizirali posjeta web-mjesta na dan prije, mjesec dana ili bilo kojem trenutku u prošlosti želite.

Da biste izvršili osnovne statističku analizu za prethodni dan, koristite u sljedećem primjeru:

````
DECLARE @twoDaysAgo datetime2 = DATEADD(DAY, -2, SYSUTCDATETIME());
DECLARE @aDayAgo datetime2 = DATEADD(DAY, -1, SYSUTCDATETIME());

SELECT UserID, SUM (PagesVisited) as TotalVisitedPages, AVG (PagesVisited) as AverageVisitedPages,
MAX (PagesVisited) AS MaxVisitedPages, MIN (PagesVisited) AS MinVisitedPages,
STDEV (PagesVisited) as StDevViistedPages
FROM dbo.WebsiteUserInfo 
FOR SYSTEM_TIME BETWEEN @twoDaysAgo AND @aDayAgo
GROUP BY UserId
````

Da biste potražili aktivnosti određenog korisnika unutar neko vrijeme, pomoću koje se NALAZE u uvjetu:

````
DECLARE @hourAgo datetime2 = DATEADD(HOUR, -1, SYSUTCDATETIME());
DECLARE @twoHoursAgo datetime2 = DATEADD(HOUR, -2, SYSUTCDATETIME());
SELECT * FROM dbo.WebsiteUserInfo 
FOR SYSTEM_TIME CONTAINED IN (@twoHoursAgo, @hourAgo)
WHERE [UserID] = 1;
````

Možete uključiti prikaz trendova i uzoraka korištenja u programa intuitivno način vrlo lako je osobito prikladno za vremenski upite grafika vizualizacije:

![TemporalGraph](./media/sql-database-temporal-tables/AzureTemporal6.png)

##<a name="evolving-table-schema"></a>Razvoj shemu tablica

Obično, morat ćete promijeniti shemu vremenski tablica dok rade razvoja aplikacija. Za to jednostavno pokrenuti običnog ALTER TABLE naredbe i baze podataka SQL Azure će komponenta Proširi promjene na tablici povijesti. Sljedeću skriptu prikazuje kako dodati dodatne atribut za praćenje:

````
/*Add new column for tracking source IP address*/
ALTER TABLE dbo.WebsiteUserInfo 
ADD  [IPAddress] varchar(128) NOT NULL CONSTRAINT DF_Address DEFAULT 'N/A';
````

Isto tako, možete promijeniti definicija stupca dok je aktivna mogućnost svoje radno opterećenje:

````
/*Increase the length of name column*/
ALTER TABLE dbo.WebsiteUserInfo 
    ALTER COLUMN  UserName nvarchar(256) NOT NULL;
````

Na kraju, možete ukloniti stupac koji vam više nije potrebna.

````
/*Drop unnecessary column */
ALTER TABLE dbo.WebsiteUserInfo 
    DROP COLUMN TemporaryColumn; 
````
    
Umjesto toga koristite najnoviju [SSDT](https://msdn.microsoft.com/library/mt204009.aspx) da biste promijenili shemu vremenski tablica dok ste povezani s bazom podataka (mrežni način rada) ili kao dio baze podataka projekta (izvanmrežni način rada).

##<a name="controlling-retention-of-historical-data"></a>Kontroliranje zadržavanja povijesne podataka

S sustava određuju vremenski tablicama, tablici povijesti mogu povećati obične tablice Veličina baze podataka većeg broja. Povijest velike i ikad rast tablica može postati problema i zbog troškove čisto prostora za pohranu, kao i nametanja na performanse poreza na vremenski upita. Dakle, razvoj pravila zadržavanja podataka za upravljanje podacima u tablici povijesti je važno aspekt planiranje i upravljanje životnim ciklusom svaki vremenski tablice. S bazom podataka SQL Azure, imate sljedeće postupke za upravljanje povijesne podatke u tablici vremenski:

- [Particija tablice](https://msdn.microsoft.com/library/mt637341.aspx#Anchor_2)
- [Čišćenje prilagođene skripte](https://msdn.microsoft.com/library/mt637341.aspx#Anchor_3)

##<a name="next-steps"></a>Daljnji koraci

Detaljne informacije o vremenski tablica potražite u članku [MSDN dokumentacije](https://msdn.microsoft.com/library/dn935015.aspx).
Posjetite 9 kanala da biste čuli [priče za uspjeh vremenski implemenation stvarnih klijenata](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) i gledanje na [live vremenski pokazni](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016).
