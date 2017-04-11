<properties
   pageTitle="Indeksiranje tablica u SQL Data Warehouse | Microsoft Azure"
   description="Početak rada s tablicom indeksiranje u skladištu podataka za SQL Azure."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="07/12/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="indexing-tables-in-sql-data-warehouse"></a>Indeksiranje tablica u SQL Data Warehouse

> [AZURE.SELECTOR]
- [Pregled][]
- [Vrste podataka][]
- [Distribucija][]
- [Indeks][]
- [Particija][]
- [Statistika][]
- [Privremeni][]

SQL Data Warehouse nudi nekoliko mogućnosti indeksiranja uključujući [grupirani columnstore indeksi][], [grupirani indekse i nonclustered indeksa][].  Uz to, također nudi u indeks mogućnost bez poznat i kao [skupova][].  U ovom se članku opisuje prednosti svaki indeks vrsta kao i savjete za obavljanje Većina performanse izvan vaše indeksi. Potražite u članku [Stvaranje tablice sintaksa][] za dodatne detalje o stvaranju tablice u SQL Data Warehouse.

## <a name="clustered-columnstore-indexes"></a>Grupirani columnstore indeksa

Prema zadanim postavkama SQL Data Warehouse stvara grupirani columnstore indeks kada nema mogućnosti indeksa su navedene tablice. Grupirani columnstore tablica nude i najvišu razinu spajanja podataka, kao i najbolje performanse cjelokupnog upita.  Grupirani columnstore tablice obično outperform grupirani indeksa ili skupova tablica i obično su najbolji odabir za velike tablice.  Zbog sljedećih razloga grupirani columnstore je najbolje mjesto da biste pokrenuli kada niste sigurni kako indeksirati tablice.  

Da biste stvorili tablicu columnstore grupirani, jednostavno navedite INDEKS COLUMNSTORE u uvjetu s ili ispustiti uvjet WITH:

```SQL
CREATE TABLE myTable   
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( CLUSTERED COLUMNSTORE INDEX );
```

Postoji nekoliko scenarija gdje grupirani columnstore možda neće biti dobar mogućnost:

- Tablica Columnstore ne podržavaju sekundarne koje nisu grupirani indeksi.  Umjesto toga razmotrite skupova ili indeks tablice.
- Tablica Columnstore ne podržavaju varchar(max), nvarchar(max) i varbinary(max).  Umjesto toga razmotrite skupova ili indeks.
- Columnstore tablica može biti manji učinkovitog za tranzitne podatke.  Razmislite o skupova i možda još privremenih tablica.
- Male tablice s manje od 100 milijuna redaka.  Razmislite o skupova tablice.

## <a name="heap-tables"></a>Tablica skupa

Kada se privremeno odredišne podataka na SQL Data Warehouse, možda će se dogoditi da pomoću tablice skupova će postati postupka cjelokupan brže.  To je zato opterećenje za skupova koje su brže nego u indeks tablice, a u nekim slučajevima možete učiniti sljedeće čitanje iz predmemorije.  Ako su učitavanja podataka samo na faze prije pokretanja dodatne transformacije, učitavanje tablice skupova tablicu bit će mnogo brže od učitavanja podataka u tablicu grupirani columnstore. Osim toga, učitavanja podataka u [Privremena][privremene] i učitava mnogo brže nego učitavanje tablice žele.  

Za manje od 100 milijuna retke, small pretraživanja tablica često skupova tablice smisla.  Započnite klaster columnstore tablice da biste postigli optimalne spajanja kada postoji više od 100 milijuna redaka.

Da biste stvorili tablicu skupova, jednostavno odredite SKUPOVA u OVIM uvjetima:

```SQL
CREATE TABLE myTable   
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( HEAP );
```

## <a name="clustered-and-nonclustered-indexes"></a>Grupirani i nonclustered indeksa

Grupirani indeksi možda outperform grupirani columnstore tablice kad u jednom retku mora biti brzo dohvatiti.  Za upite koji se gdje se potrebne za rad s ekstremne brzine jednom ili vrlo malo vrijednostima retka, razmislite o klaster indeksa ili nonclustered sekundarne indeksa.  Nedostatak pomoću grupni indeks je prednosti će se samo upite koji koristite iznimno selektivno filtar na stupcu indeks.  Da biste poboljšali filtar na ostalim stupcima nonclustered indeks možete dodati na ostale stupce.  Međutim, svaki indeks koja se dodaje tablici će dodati prostor i vrijeme obrade opterećenje.

Da biste stvorili tablicu indeks, jednostavno odredite INDEKS u OVIM uvjetima:

```SQL
CREATE TABLE myTable   
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( CLUSTERED INDEX (id) );
```

Da biste dodali u indeks koji nisu grupirani na tablicu, jednostavno koristite sljedeću sintaksu:

```SQL
CREATE INDEX zipCodeIndex ON t1 (zipCode);
```

> [AZURE.NOTE] Indeks za koje nisu grupirani je po zadanom stvaraju kada se koristi CREATE INDEX. Osim toga, indeks za koje nisu grupirani dopušteno je samo za pohranu retka tablice (SKUPOVA ili GRUPIRANI INDEKSA). Trenutno nije dopuštena koje nisu grupirani indeksi pri vrhu COLUMNSTORE INDEKS.


## <a name="optimizing-clustered-columnstore-indexes"></a>Optimiziranje grupirani columnstore indeksa

Grupirani columnstore tablice su organizirane u podacima u segmenata.  Imate segmenta visoke kvalitete je važnosti postizanja performanse optimalne upita za tablicu columnstore.  Za broj redaka u grupi Komprimirana retka možete mjeriti kvalitete segmenta.  Kvaliteta segmenta je najčešće optimalnih gdje se nalaze barem 100K redaka po Komprimirana retka grupirati i dobiti performanse kao broj redaka po retku grupe pristup 1 048 576 redaka koji se većina redaka koji se mogu sadržavati grupe redaka.

U nastavku prikaz se mogu stvoriti i koristiti u sustavu za izračunavanje prosjeka redaka po retku grupirati i prepoznavanje sve podgrupama optimalnih klaster columnstore indeksi.  Zadnji stupac prikaz generirat će kao SQL naredbe kojih je moguće ponovno stvaranje vaše indeksi.

```sql
CREATE VIEW dbo.vColumnstoreDensity
AS
SELECT
        GETDATE()                                                               AS [execution_date]
,       DB_Name()                                                               AS [database_name]
,       s.name                                                                  AS [schema_name]
,       t.name                                                                  AS [table_name]
,   COUNT(DISTINCT rg.[partition_number])                   AS [table_partition_count]
,       SUM(rg.[total_rows])                                                    AS [row_count_total]
,       SUM(rg.[total_rows])/COUNT(DISTINCT rg.[distribution_id])               AS [row_count_per_distribution_MAX]
,   CEILING ((SUM(rg.[total_rows])*1.0/COUNT(DISTINCT rg.[distribution_id]))/1048576) AS [rowgroup_per_distribution_MAX]
,       SUM(CASE WHEN rg.[State] = 0 THEN 1                   ELSE 0    END)    AS [INVISIBLE_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE 0    END)    AS [INVISIBLE_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE NULL END)    AS [INVISIBLE_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE NULL END)    AS [INVISIBLE_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE NULL END)    AS [INVISIBLE_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 1 THEN 1                   ELSE 0    END)    AS [OPEN_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE 0    END)    AS [OPEN_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 2 THEN 1                   ELSE 0    END)    AS [CLOSED_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE 0    END)    AS [CLOSED_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 3 THEN 1                   ELSE 0    END)    AS [COMPRESSED_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE 0    END)    AS [COMPRESSED_rowgroup_rows]
,       SUM(CASE WHEN rg.[State] = 3 THEN rg.[deleted_rows]   ELSE 0    END)    AS [COMPRESSED_rowgroup_rows_DELETED]
,       MIN(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_AVG]
,       'ALTER INDEX ALL ON ' + s.name + '.' + t.NAME + ' REBUILD;'             AS [Rebuild_Index_SQL]
FROM    sys.[pdw_nodes_column_store_row_groups] rg
JOIN    sys.[pdw_nodes_tables] nt                   ON  rg.[object_id]          = nt.[object_id]
                                                    AND rg.[pdw_node_id]        = nt.[pdw_node_id]
                                                    AND rg.[distribution_id]    = nt.[distribution_id]
JOIN    sys.[pdw_table_mappings] mp                 ON  nt.[name]               = mp.[physical_name]
JOIN    sys.[tables] t                              ON  mp.[object_id]          = t.[object_id]
JOIN    sys.[schemas] s                             ON t.[schema_id]            = s.[schema_id]
GROUP BY
        s.[name]
,       t.[name]
;
```

Sad kad ste stvorili u prikazu, pokrenite ovaj upit da biste pronašli tablice s grupama retka s manje od 100K redaka.  Naravno, preporučujemo vam da biste povećali prag 100K ako tražite dodatne kvalitete optimalnih segmenta. 

```sql
SELECT  *
FROM    [dbo].[vColumnstoreDensity]
WHERE   COMPRESSED_rowgroup_rows_AVG < 100000
        OR INVISIBLE_rowgroup_rows_AVG < 100000
```

Kada pokrenete upit možete početi pogledati podatke i analiza rezultata. Ova tablica donosi što da biste potražili u analizi grupe redaka.


| Stupac                             | Upute za korištenje ove podatke                                                                                                                                                                      |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [table_partition_count]            | Ako je particije tablice, koji može očekivati da biste vidjeli veći grupa otvorena redaka broji. Svaki particije u distribuciju nije u teorija imaju grupu Otvori retka pridružen. To faktor u analize. Malu tablicu koja sadrži su particije nije moguće optimizirati uklanjanjem na particija potpuno kao što je to bi olakšao sažimanja.                                                                        |
| [row_count_total]                  | Broj retka zbroja za tablicu. Ako, na primjer, možete koristiti tu vrijednost za izračunavanje postotka redaka u Komprimirana stanju.                                                                      |
| [row_count_per_distribution_MAX]   | Ako su svi reci ravnomjerno distribuirali tu vrijednost bio cilj broj redaka po distribuciju. Usporedite ovu vrijednost s na compressed_rowgroup_count.                                 |
| [COMPRESSED_rowgroup_rows]         | Ukupan broj redaka u obliku columnstore za tablicu.                                                                                                                                 |
| [COMPRESSED_rowgroup_rows_AVG]     | Ako Prosječan broj redaka znatno manje maksimalan broj redaka za grupe redaka, razmislite o recompress podatke pomoću CTAS ili ALTER ponovno stvaranje INDEKSA                     |
| [COMPRESSED_rowgroup_count]        | Broj grupe redaka u obliku columnstore. Ako je taj broj vrlo visoka odnosu tablicu je pokazatelj gustoće columnstore nedostaje.                                  |
| [COMPRESSED_rowgroup_rows_DELETED] | Reci logično brišu se u obliku columnstore. Ako je broj visoke odnosu Veličina tablice, razmislite o vraćanju particija ili Ponovna izgradnja indeksa, kao što je time ih fizički. |
| [COMPRESSED_rowgroup_rows_MIN]     | Koristi se u kombinaciji sa stupcima AVG i MAX za razumijevanje raspon vrijednosti za grupe redaka u vašem columnstore. Malim brojem tijekom učitavanja prag (102,400 po particija poravnat raspodjele) predlaže optimizacije dostupne u učitavanja podataka                                                                                                                                                 |
| [COMPRESSED_rowgroup_rows_MAX]     | Kao iznad                                                                                                                                                                                  |
| [OPEN_rowgroup_count]              | Otvaranje redak grupe su normalno. Jedan očekivali razumno jedne grupe OTVORI redak po tablici raspodjele (60). Viškom brojeva predložiti podataka učitavanja preko particije. Provjera dvaput stvaranje particija strategije da je zvuk                                                                                                                                                                                                |
| [OPEN_rowgroup_rows]               | Svake grupe redaka može imati 1 048 576 redaka u njemu kao maksimalno. Da biste vidjeli kako cijeli grupe Otvori redaka trenutno koristite tu vrijednost                                                                 |
| [OPEN_rowgroup_rows_MIN]           | Otvaranje grupe označavaju da je podataka koja se redovito učitava u tablici ili da biste da prethodna opterećenje spilled preostalo redaka u ovoj grupi retka. Korištenje MIN, MAX, PROSJ stupce da biste vidjeli kojom količinom podataka sat je otvorena redak grupe. Za male tablice Razlog može biti 100% svih podataka! U tom se slučaju izmijeniti INDEKS ponovno stvaranje da biste nametnuli sortiranje podataka na columnstore.                                                                       |
| [OPEN_rowgroup_rows_MAX]           | Kao iznad                                                                                                                                                                                  |
| [OPEN_rowgroup_rows_AVG]           | Kao iznad                                                                                                                                                                                  |
| [CLOSED_rowgroup_rows]             | Pogledajte grupiranje redaka zatvoreni retka kao sanity kvačica.                                                                                                                                       |
| [CLOSED_rowgroup_count]            | Broj retka zatvoreni grupa mora biti niska Ako bilo koji možete vidjeti uopće. Komprimirana rowg roups pomoću INDEKSA ALTER može pretvoriti grupe skrivenih redaka... REORGANISE naredbu. No to nije normalno potrebna. Zatvoreni grupe automatski pretvaraju se u grupe redaka columnstore "n-torke mover" proces pozadine.                                                                                               |
| [CLOSED_rowgroup_rows_MIN]         | Zatvoreni redak grupe mora imati vrlo visoka ispune stopa. Ako je stopa ispune za grupu zatvoreni retka niske, zatim daljnje analize u columnstore potreban je.                                   |
| [CLOSED_rowgroup_rows_MAX]         | Kao iznad                                                                                                                                                                                  |
| [CLOSED_rowgroup_rows_AVG]         | Kao iznad                                                                                                                                                                                  |
| [Rebuild_Index_SQL]         | SQL ponovno stvaranje indeksa columnstore za tablicu                                                                                                                                                     |

## <a name="causes-of-poor-columnstore-index-quality"></a>Uzroci loše columnstore indeks kvalitete

Ako ste identificirali tablice s segmenta loše kvalitete, želite da biste odredili uzrok.  U nastavku su neke druge uobičajeni uzroci loše segmenta quaility:

1. Memorije tlaka kada gradnje indeksa
2. Velik broj DML operacije
3. Small ili trickle operacije učitavanja
4. Previše particije

Sljedećih čimbenika mogu prouzročiti columnstore index da bi znatno manji od 1 milijun redaka optimalne po grupe redaka.  Također mogu uzrokovati retke da biste prešli na grupe redaka delta umjesto grupe sažete redaka. 

### <a name="memory-pressure-when-index-was-built"></a>Memorije tlaka kada gradnje indeksa

Broj redaka po grupi Komprimirana retka izravno povezani s širinu retka i iznos memorije za obradu grupe redaka.  Kada se reci zapisuju u columnstore tablice u odjeljku memorije, columnstore segmenta kvalitete mogu se.  Stoga, najbolje je da bi se dobilo sesiju koji je pisanju pristup columnstore indeksa tablice suvišni memorije moguće.  Budući da je trade-off između memorije i istodobnosti, smjernice o Dodjela desnom memorije ovisi o podataka u svakom retku tablice, količinu DWU ste dodijeliti sa sustavom i iznos istodobnosti slobodnih možete dati sesiju u kojem je zapisivanje podataka u tablicu.  Kao najbolji način preporučujemo ako koristite DW400 DW600 i mediumrc ako koristite DW1000 i iznad, počevši s xlargerc ako koristite DW300 ili manje largerc.

### <a name="high-volume-of-dml-operations"></a>Velik broj DML operacije

Velik broj DML operacije koje ažuriranje i brisanje redaka može uzrokovati inefficiency u na columnstore. Ovo je osobito true kad se mijenjaju Većina reci u grupe redaka.

- Brisanje retka iz grupe sažete redaka samo logično označava redak izbrisane. Redak će ostati u grupi Komprimirana retka dok se izgraditi particija ili tablicu.
- Umetanja retka dodaje retka u tablici Interna rowstore naziva grupa delta retka. Umetnuti redak se pretvara u columnstore dok grupe redaka delta pun i označen kao što je zatvoren. Grupe redaka zatvoreni kada dođu maksimalnog kapaciteta 1 048 576 redaka. 
- Ažuriranje retka u obliku columnstore obrađuje kao logička Izbriši, a zatim umetnut. Umetnuti redak mogu biti pohranjeni u spremištu delta.

Odbacivanja ažuriranja i umetnite operacije koje Premašili prag skupno 102,400 redaka po particija poravnati raspodjele će biti napisani izravno u obliku columnstore. Međutim, pod pretpostavkom ravnomjerna distribucija, trebali biste biti izmjena više od milijun 6.144 redaka u jednoj operaciji za to da se pokreće. Ako je broj redaka za dani particija poravnat raspodjele je manji od 102,400 retke idu u trgovini delta i će ostati došlo do potrebne retke umetnuti ili izmijeniti da biste zatvorili grupe redaka ili indeks ponovno ne stvoriti.

### <a name="small-or-trickle-load-operations"></a>Small ili trickle operacije učitavanja

Mali učitava da u SQL Data Warehouse ponekad su poznati kao trickle opterećenje. Obično predstavljaju najbliži konstante strujanja podataka koji se ingested sustav. Međutim, kao što je ovaj tok blizu neprekinuti količinu redaka nije osobito velike. Više često od podataka nije znatno u odjeljku prag potrebne za Izravni učitavanja columnstore oblik.

U ovih situacija često je bolje najprije krenuti podatke u spremište blobova platforme Azure i pričekajte skupiti prije učitavanja. Ovu tehniku često se nazivaju kao *grupnog slanja micro – promjena*.

### <a name="too-many-partitions"></a>Previše particije

Druge stvari koje treba razmotriti je utjecaj particija na grupirani columnstore tablica.  Prije nego što particija, SQL Data Warehouse već dijeli podataka u 60 baze podataka.  Daljnje particija dijeli podataka.  Ako particija podataka, koje će želite uzeti u obzir da **svaki** particije moraju imati najmanje 1 milijun redaka prednosti iz grupirani columnstore indeksa.  Ako particija tablice u 100 particije, zatim tablice moraju imati barem 6 milijarde reci koje treba pogodnost grupirani columnstore indeksa (60 distribucija *particije 100* milijuna 1 redaka). Ako tablica 100 particija 6 milijarde redaka, smanjite broj particije ili preporučujemo da umjesto toga koristite skupova tablice.

Kada se s nekim podacima učitan tablica, slijedite na ispod korake za otkrivanje i ponovno stvaranje tablice s podgrupama optimalnih klaster columnstore indeksi.

## <a name="rebuilding-indexes-to-improve-segment-quality"></a>Ponovno stvaranje indeksa radi poboljšanja kvalitete segmenta

### <a name="step-1-identify-or-create-user-which-uses-the-right-resource-class"></a>Korak 1: Pronađite ili stvorite korisnika koji koristi predmete desnom resursa

Jedan brz način odmah poboljšati kvalitetu segmenta je ponovno stvaranje indeksa.  SQL vratio iznad prikaza koji će vratiti izjava za ALTER ponovno stvaranje INDEKSA kojih je moguće ponovno stvaranje vaše indeksi.  Kada novog sustava indeksi, ne zaboravite dodjelu dovoljno memorije u sesiju koji će ponovno stvaranje indeksa.  Da biste to učinili, povećajte predmete resursa korisnika koji ima dozvole za ponovno stvaranje indeksa na tablici preporučene najmanje.  Klasa resursa korisnik vlasnik baze podataka nije moguće promijeniti tako da se ako niste stvorili korisnika u sustavu, morat ćete učiniti prvi put.  Najmanji preporučujemo da je xlargerc ako koristite DW300 ili manje, largerc ako koristite DW400 DW600 i mediumrc ako koristite DW1000 i iznad.

Slijedi primjer kako dodijeliti dodatnu memoriju korisniku povećavanjem svoje predmete resursa.  Dodatne informacije o resursu klase i kako stvoriti novi korisnik može pronaći u [istodobnosti i radno opterećenje upravljanje] [ Concurrency] članka.

```sql
EXEC sp_addrolemember 'xlargerc', 'LoadUser'
```

### <a name="step-2-rebuild-clustered-columnstore-indexes-with-higher-resource-class-user"></a>Korak 2: Ponovno stvaranje indeksa grupirani columnstore veći korisniku predmete resursa
Prijava u ulozi korisnika iz korak 1 (npr. LoadUser), koji je sada pomoću veći predmete resursa i izvršavanje naredbe izmijeniti INDEKS.  Provjerite je li da taj korisnik s dozvolom za ALTER tablice koje je u tijeku izgraditi indeks.  Ovi primjeri pokazuju kako ponovno stvaranje cijele columnstore indeksa ili ponovno stvaranje jedna particija. Na velikih tablica je više praktično ponovno stvaranje indeksi jedna particija odjednom.

Osim toga, umjesto novog indeksa nije kopirajte tablicu u novu tablicu pomoću [CTAS][].  Način na koji je najbolji? Za velike količine podataka, [CTAS][] je obično brže nego [Izmijeniti INDEKS][]. Za manje količine podataka, [Izmijeniti INDEKS][] lakše je koristiti i neće biti potrebno da biste zamijenili u tablici.  Dodatne informacije o tome kako ponovno stvaranje indeksa s CTAS potražite u članku **Rebuilding indeksi s CTAS i izmjenu particija** ispod.

```sql
-- Rebuild the entire clustered index
ALTER INDEX ALL ON [dbo].[DimProduct] REBUILD
```

```sql
-- Rebuild a single partition
ALTER INDEX ALL ON [dbo].[FactInternetSales] REBUILD Partition = 5
```

```sql
-- Rebuild a single partition with archival compression
ALTER INDEX ALL ON [dbo].[FactInternetSales] REBUILD Partition = 5 WITH (DATA_COMPRESSION = COLUMNSTORE_ARCHIVE)
```

```sql
-- Rebuild a single partition with columnstore compression
ALTER INDEX ALL ON [dbo].[FactInternetSales] REBUILD Partition = 5 WITH (DATA_COMPRESSION = COLUMNSTORE)
```

Indeksa u SQL Data Warehouse je operaciju izvanmrežno.  Dodatne informacije o rasponu indeksi potražite u odjeljku ALTER ponovno stvaranje INDEKSA u [Columnstore indeksi defragmentaciju][] i temi sintaksa [Izmijeniti INDEKS][].
 
### <a name="step-3-verify-clustered-columnstore-segment-quality-has-improved"></a>Korak 3: Provjera kvalitete segmenta grupirani columnstore poboljšane
Ponovno pokrenite upit koji identificirani tablica s slabo fazi kvalitete i potvrdila kvaliteta segmenta poboljšane.  Ako segmenta kvalitete jeste li poboljšali, Razlog može biti jesu li redaka u tablici vrlo širine.  Razmislite o korištenju klasu noviji resursa ili DWU kada novog sustava indeksi.

 
## <a name="rebuilding-indexes-with-ctas-and-partition-switching"></a>Ponovno stvaranje indeksa s CTAS i izmjenu partition

U ovom se primjeru koristi [CTAS][] i particija promjene ponovno stvaranje particija tablice. 

```sql
-- Step 1: Select the partition of data and write it out to a new table using CTAS
CREATE TABLE [dbo].[FactInternetSales_20000101_20010101]
    WITH    (   DISTRIBUTION = HASH([ProductKey])
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101,20010101
                                )
                            )
            )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
WHERE   [OrderDateKey] >= 20000101
AND     [OrderDateKey] <  20010101
;

-- Step 2: Create a SWITCH out table
CREATE TABLE dbo.FactInternetSales_20000101
    WITH    (   DISTRIBUTION = HASH(ProductKey)
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101
                                )
                            )
            )
AS
SELECT *
FROM    [dbo].[FactInternetSales]
WHERE   1=2 -- Note this table will be empty

-- Step 3: Switch OUT the data 
ALTER TABLE [dbo].[FactInternetSales] SWITCH PARTITION 2 TO  [dbo].[FactInternetSales_20000101] PARTITION 2;

-- Step 4: Switch IN the rebuilt data
ALTER TABLE [dbo].[FactInternetSales_20000101_20010101] SWITCH PARTITION 2 TO  [dbo].[FactInternetSales] PARTITION 2;
```

Dodatne informacije o ponovno stvoriti particije pomoću `CTAS`, potražite u članku [particije][] .

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije potražite u člancima na [Pregled tablica][Pregled], [Vrste podataka u tablici][Vrste podataka], [distribucija tablice][Raspodijeli], [particija tablice][particije], [Zadržavanje tablice Statistika][Statistika] i [Privremenih tablica][privremene].  Da biste saznali više o najbolje prakse, potražite u članku [Najbolje prakse skladištu podataka SQL][].

<!--Image references-->

<!--Article references-->
[Pregled]: ./sql-data-warehouse-tables-overview.md
[Vrste podataka]: ./sql-data-warehouse-tables-data-types.md
[Distribucija]: ./sql-data-warehouse-tables-distribute.md
[Indeks]: ./sql-data-warehouse-tables-index.md
[Particija]: ./sql-data-warehouse-tables-partition.md
[Statistika]: ./sql-data-warehouse-tables-statistics.md
[Privremeni]: ./sql-data-warehouse-tables-temporary.md
[Concurrency]: ./sql-data-warehouse-develop-concurrency.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[Najbolje prakse skladištu podataka SQL]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->
[ISKAZ ALTER INDEKSA]: https://msdn.microsoft.com/library/ms188388.aspx
[skupova]: https://msdn.microsoft.com/library/hh213609.aspx
[Grupirani indekse i nonclustered indeksa]: https://msdn.microsoft.com/library/ms190457.aspx
[Stvaranje tablice sintaksu]: https://msdn.microsoft.com/library/mt203953.aspx
[Columnstore indeksi defragmentaciju]: https://msdn.microsoft.com/library/dn935013.aspx#Anchor_1
[Grupirani columnstore indeksa]: https://msdn.microsoft.com/library/gg492088.aspx

<!--Other Web references-->
