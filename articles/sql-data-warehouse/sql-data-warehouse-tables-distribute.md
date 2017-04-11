<properties
   pageTitle="Distribucija tablica u SQL Data Warehouse | Microsoft Azure"
   description="Početak rada s distribucija tablice u skladištu podataka za SQL Azure."
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
   ms.date="08/30/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="distributing-tables-in-sql-data-warehouse"></a>Distribucija tablica u SQL Data Warehouse

> [AZURE.SELECTOR]
- [Pregled][]
- [Vrste podataka][]
- [Distribucija][]
- [Indeks][]
- [Particija][]
- [Statistika][]
- [Privremeni][]

SQL Data Warehouse je massively paralelno obrada (MPP) distribuirati bazu podataka sustava.  Dijeljenje podataka i obrada mogućnost preko više čvorove, SQL Data Warehouse može ponuditi veliki skalabilnost - daleko izvan sistemske jedan.  Odlučite kako distribuirati podataka unutar SQL Data Warehouse je jedan od najvažnijih čimbenika za postizanje optimalnih performansi.   Ključ za postizanje optimalnih performansi je maksimiziranja premještanje podataka i shodno ključ za minimiziranje premještanje podataka odabire strategije desnom distribuciju.

## <a name="understanding-data-movement"></a>Objašnjenje premještanje podataka

U sustavu MPP, podaci iz svaku tablicu je podijeljen preko nekoliko podlozi baza podataka.  Većina optimizirana upita na sustavu MPP možete jednostavno prošla izvršiti na pojedinačne raspodijeljeno baze podataka s bez interakcije između drugih baza podataka.  Na primjer, recimo da imate bazu podataka s podacima o prodaji koji sadrži dvije tablice, Prodaja i klijentima.  Ako imate upit koji je potrebno da biste se pridružili prodaje tablice u tablicu klijenta i podijelite i prodaje i kupca tablica se broj kupca umetanja svakog kupca u zasebnu bazu podataka, eventualne upite koji uključivanje prodaje i kupca može riješiti unutar svake baze podataka pomoću poznavanje druge baze podataka.  Za razliku od toga, ako vaše podatke o prodaji podijeljen broj narudžbe i korisničkih podataka prema broju klijenta, pa sve danu bazu podataka neće imati odgovarajuće podatke za svakog korisnika a time i želite li uključiti vaše podatke o prodaji podacima o klijentima, trebali biste dobiti podatke za svakog korisnika iz druge baze podataka.  U ovom primjeru drugi premještanje podataka morate dogoditi da biste premjestili podatke o klijentu podatke o prodaji, tako da mogu se spajaju dvije tablice.  

Premještanje podataka nije uvijek neispravni stvar, katkad je potrebno da biste riješili upita.  No kada ovaj dodatnog koraka možete se izbjegavati, prirodan upit funkcionirat će brže.  Premještanje podataka najčešće nastaje prilikom spojeni na tablicama ili se izvode zbrajanja.  Često morate učiniti oboje, tako da se tijekom ćete moći Optimiziraj za jedan scenarij, kao što je spoj, i dalje potrebna premještanje podataka da biste rješavanje za drugi scenarij, kao što su prikupljanja.  Trik je odrediti koji je znače manje posla.  U većini slučajeva, distribucija tablice velike činjenica često spojene stupac je najučinkovitiji način za smanjenje Većina premještanje podataka.  Distribucija podataka u stupcima spoj je mnogo više uobičajeni način da biste smanjili premještanje podataka od distribucija podataka u stupcima koji je uključen u prikupljanja.

## <a name="select-distribution-method"></a>Odaberite način distribucije

U pozadini SQL Data Warehouse dijeli podataka u 60 baze podataka.  Svaki pojedinačne baze podataka se nazivaju se **raspodjele**.  Kada podataka učita u svaku tablicu, SQL Data Warehouse mora znati kako se dijeljenje podataka preko tih 60 distribucije.  

Način distribucije definiran na razini tablice i trenutno postoje dvije mogućnosti:

1. **Kružno** koji distribucija podataka ravnomjerno, ali slučajno.
2. **Raspršivanje Distributed** koji distribuira podataka na temelju raspršivanje vrijednosti iz jednog stupca

Prema zadanim postavkama, kada koje ste definirali način distribucije podataka tablice će se raspodijeliti pomoću postupak **kružnog** raspodjele.  Međutim, kao što je ćete postati više sofisticirane u svoju implementaciju, ćete željeti razmislite o korištenju **raspodijeljene predmemorije** tablice da biste minimizirali premještanje podataka koji će shodno optimiziranja performansi upita.

### <a name="round-robin-tables"></a>Kružno tablice

Korištenje kružnog način raspodjela podataka je vrlo slično kako ga Zvuči.  Kao što je učitana podataka, svaki redak jednostavno se šalje sljedeće raspodjele.  Ova metoda distribucija podataka će uvijek slučajno razmještavanje podataka vrlo preko svih na distribucije.  To jest, postoji bez sortiranje gotovo tijekom postupka kružnog koji postavlja podataka.  Kružno raspodjele ponekad se naziva izravnim raspršivanje zbog toga.  S tablicom raspodijeljeno kružnog nema potrebe da biste shvatili podatke.  Zbog toga kružnog – tablice često provjerite dobar učitavanja ciljnih web-mjesta.

Prema zadanim postavkama, ako je odabran nema način distribucije, postupak kružnog raspodjele će se koristiti.  Međutim, dok su tablice kružnog jednostavno je za korištenje, jer podataka slučajno raspodijeliti sustava znači sustav ne jamči koje raspodjele svaki redak uključena.  Zbog toga sustav ponekad mora pozivanje postupka za premještanje podataka da biste bolje organizirali podatke možete razriješiti upita.  U ovom dodatnog koraka može usporiti upite.

Razmislite o korištenju kružnog distribucije za tablicu u sljedećim scenarijima:

- Prilikom početka rada kao jednostavni početnu točku
- Ako nema očite spojno ključa
- Ako nema stupca dobar prijedlog za raspršivanje distribucija tablice
- Ako u tablici dijeliti uobičajene ključ spoja s ostalim tablicama
- Ako spoj je manji od drugih spojeva u upitu
- Kad je tablica privremena pripremna

Oba ovim se primjerima će stvoriti kružnog tablicu:

```SQL
-- Round Robin created by default
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
;

-- Explicitly Created Round Robin Table
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = ROUND_ROBIN
)
;
```

> [AZURE.NOTE] Dok je kružnog Zadana vrsta tablica koji se izričito u vašem DDL smatra preporučenim načinom rada tako da je namjere izgled tablice da biste drugim korisnicima.

### <a name="hash-distributed-tables"></a>Raspršivanja raspodijeljeno tablice

Pomoću algoritam za **raspršivanje distributed** distribuirati tablice možete poboljšati performanse scenarije mnogo smanjivanjem premještanje podataka u trenutku upita.  Raspršivanje distributed tablice su tablice koje su podijeljene između raspodijeljeno baze podataka u programu algoritam za raspršivanje na jedan stupac koji odaberete.  Stupac raspodjele je što određuje koliko je podijeljen podatke preko raspodijeljeno baze podataka.  Funkcija raspršivanje koristi raspodjele stupca da biste dodijelili redaka distribucije.  Algoritam za raspršivanje i konačne raspodjele je deterministic.  To je iste vrijednosti s istu vrstu podataka će se uvijek ima isti distribucije.    

U ovom se primjeru će stvoriti tablicu distributed ID:

```SQL
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,  DISTRIBUTION = HASH([ProductKey])
)
;
```

## <a name="select-distribution-column"></a>Odaberite stupac distribuciju.

Kada se odlučite za **raspodjelu raspršivanje** tablice, morat ćete odaberite stupac jedan distribuciju.  Kada odaberete stupac raspodjele, postoje tri glavna čimbenici koje treba uzeti u obzir.  

Odaberite jedan stupac koji će:

1. Ne može se ažurirati
2. Razmještavanje podataka, izbjegavanje podataka skew
3. Minimiziranje premještanje podataka

### <a name="select-distribution-column-which-will-not-be-updated"></a>Odaberite stupac za raspodjelu koji će se ažurirati

Raspodjele se poredak stupaca koji se mogu ažurirati, stoga, odaberite stupac koji sadrži statičke vrijednosti.  Ako stupac ćete morati ažurirati, nije obično dobro raspodjele kandidata.  Ako slučaj kojem morate ažurirati stupca raspodjele, to možete učiniti tako da najprije brisanje retka i umetnuti novi redak.

### <a name="select-distribution-column-which-will-distribute-data-evenly"></a>Odaberite stupac za raspodjelu koji će razmještavanje podataka

Budući da raspodijeljeno sustava izvodi samo jednako brzo kao njegov najmanju raspodjele, važno je podijelite posao ravnomjerno preko na distribucija da biste postigli Balansirani izvođenja u sustavu.  Način rada podijeljen raspodijeljeno sustavu temelji se na gdje se nalaze podaci za svaki raspodjele.  Time se omogućuje važno da biste odabrali stupac desno distribucije za distribuciju podataka tako da svaki raspodjele je jednak rad i počet će se u isto vrijeme da biste dovršili njegov dio posla.  Prilikom rada dobro je podijeljen u sustavu, podatke je raspoređen po na distribucije.  Kada podataka nisu ravnomjerno Balansirani, ne možemo nazovite ovo **skew podataka**.  

Da biste ravnomjerno dijeljenje podataka i izbjegli skew podataka, razmotrite sljedeće prilikom odabira stupca raspodjele:

1. Odaberite stupac koji sadrži vrlo broj različitih vrijednosti.
2. Izbjegavajte distribucija podataka u stupcima s nekoliko različitih vrijednosti. 
3. Izbjegavajte distribucija podataka u stupcima s velika učestalost vrijednosti null.
4. Izbjegavajte distribucija podataka u stupcima datuma.

Budući da je svaka vrijednost hashed 1 od 60 distribucija, da biste postigli ravnomjerna distribucija ćete da biste odabrali stupac koji je vrlo jedinstven i sadrži više od 60 jedinstvene vrijednosti.  Da biste ilustrirali, razmotrite slučaja gdje stupac sadrži samo 40 jedinstvene vrijednosti.  Ako je taj stupac odabran kao ključ za raspodjelu, podaci za tu tablicu želite krenuti na 40 distribucija najviše, ostavite 20 distribucija s nema podataka i bez obrade da biste učinili.  Nasuprot tome, na 40 distribucija promijenile više zadataka koje ako podatke je ravnomjerno podjele na više od 60 distribucije.  Primjer podataka skew je taj scenarij.

U sustavu MPP svakog koraka upita čeka sve distribucija da biste dovršili njihove zajednički rad.  Ako jedan raspodjele je način rad više od ostalih, zatim resursa u distribucija su zapravo utrošeno samo one koje čekaju na zauzet raspodjele.  Prilikom rada nisu ravnomjerno podjele preko svih distribucija, ne možemo nazovite ovo **obrade skew**.  Obrada skew uzrokovat će sporije od ako povećavaju može biti ravnomjerno dodijeliti većem na distribucija izvršavanje upita.  Skew podataka vodi na obradu skew.

Izbjegavajte distribucija iznimno koji stupac kao vrijednosti null će sve krenuti na iste distribucije. Distribucija na stupac datuma također mogu uzrokovati obrada skew jer će sve podatke za navedeni datum krenuti na iste distribucije. Ako više korisnika su izvršavanja upita sva filtriranja na isti datum, a zatim će se način samo 1 od 60 distribucija sav rad nakon određenog datuma bit će samo na jednom distribuciju. U ovom scenariju upiti vjerojatno će se izvoditi manja od ako podaci nisu ravnomjerno šire sve na distribucija 60 puta. 

Kada nema stupaca dobar prijedlog postoji, a zatim razmislite o korištenju kružnog kao način distribucije.

### <a name="select-distribution-column-which-will-minimize-data-movement"></a>Odaberite stupac raspodjele koji će minimiziranje premještanje podataka

Minimiziranje premještanje podataka tako da odaberete stupca desno raspodjele je jedan od najvažnijih Strategije za optimiziranje performanse SQL Data Warehouse.  Premještanje podataka najčešće nastaje prilikom spojeni na tablicama ili se izvode zbrajanja.  Stupci koji se koriste u `JOIN`, `GROUP BY`, `DISTINCT`, `OVER` i `HAVING` rečenice sve olakšavaju **dobar** raspršivanja raspodjelu kandidata. 

Na druge Ruka, stupci u `WHERE` uvjet učinite **ne** provjerite za dobar raspršivanje stupca kandidata uzrokuje obrada skew jer ograničavaju koje distribucija sudjelovanje u upitu.  Dobar primjer stupac na koji se može biti tempting za raspodjelu na, ali često može uzrokovati ovaj obrada skew je stupac datuma.

Općenito govoreći, ako imate dvije velike tablice činjenica često uvrštene u spoj, će dobiti Većina performanse raspodijeliti obje tablice na jedan od stupaca spoja.  Ako imate tablicu koja nikad se pridružite druge tablice činjenica velik, zatim potražite na stupce koji se često u na `GROUP BY` uvjet.

Postoji nekoliko ključa kriterij koji se mora podudarati da biste izbjegli premještanje podataka tijekom spoja:

1. Tablice uvrštene u spoju mora biti raspršivanje distributed na **jedan** stupaca koji sudjeluju u spoja.
2. Vrste podataka stupaca Pridruži se mora podudarati između obje tablice.
3. Stupci mora biti pridruženo s operator jednakosti.
4. Vrsta spoja možda neće biti u `CROSS JOIN`.


## <a name="troubleshooting-data-skew"></a>Skew podatke za otklanjanje poteškoća

Kada podatke tablice distribuira raspodjele metodom raspršivanje postoji mogućnost da bi se neke distribucija izobličiti da bi neproporcionalno više podataka od drugih. Skew viškom podataka može utjecati na performanse upita jer konačni rezultat raspodijeljenom upitu morate pričekati najviše izvodi raspodjele da biste završili. Ovisno o stupnju skew podataka možda ćete morati je adresa.

### <a name="identifying-skew"></a>Označavanje skew

Jednostavan način za identifikaciju tablice, kao što je korelacije je za korištenje `DBCC PDW_SHOWSPACEUSED`.  Ovo je vrlo brz i jednostavan način da biste vidjeli broj redaka tablice koje se pohranjuju u svakoj od 60 distribucija baze podataka.  Imajte na umu da najčešće Balansirani performanse redaka u tablici raspodijeljeno mora biti širenje ravnomjerno preko svih distribucija.

```sql
-- Find data skew for a distributed table
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

Međutim, upit dinamički Upravljanje prikazima Azure SQL Data Warehouse (DMV) možete izvršiti više detaljnu analizu.  Da biste započeli, stvorite prikaza [dbo.vTableSizes][] prikaza pomoću SQL iz članka[Pregled] [Tablica pregled].  Nakon stvaranja prikaza, pokrenite ovaj upit da biste odredili koje je tablice imati više od 10% podataka skew.

```sql
select *
from dbo.vTableSizes 
where two_part_name in 
    (
    select two_part_name
    from dbo.vTableSizes
    where row_count > 0
    group by two_part_name
    having min(row_count * 1.000)/max(row_count * 1.000) > .10
    )
order by two_part_name, row_count
;
```

### <a name="resolving-data-skew"></a>Rješavanje podataka skew

Sve skew nije dovoljno za jamči popravak.  U nekim slučajevima performanse tablice u neki upiti mogu su nanošenje skew podataka.  Da biste odlučili Ako treba riješiti podataka skew u tablici, morate se upoznati najveću moguću o količine podataka i upitima u svoje radno opterećenje.   Jedan možete pogledati utjecaj skew tako da slijedite korake u članku [Nadzor upita][] praćenje utjecaj skew na performanse upita, a posebice utjecaj na koliko upita poduzeti da biste dovršili na pojedinačne distribucije.

Raspodjela podataka je pitanje pronalaženje desnom saldo između minimiziranje skew podataka i minimiziranje premještanje podataka. To možete suprotne ciljeve, a ponekad želite zadržati podatke skew da biste smanjili premještanje podataka. Na primjer, kada je stupac raspodjele često zajednički stupac u spojevi i zbrajanja, koje će biti minimiziranje premještanje podataka. Prednost pojavljuju premještanja minimalnog podataka možda su posljedice pojavljuju podataka skew. 

Na uobičajeni način da biste riješili skew podataka je da biste ponovno stvorili tablicu sa različitim distribuciju. Budući da ne postoji način da biste promijenili raspodjele stupca u postojećoj tablici, način da biste promijenili raspodjele tablice da biste morali stvarati dokumente s [] [CTAS].  Evo dva primjera kako riješiti skew podataka:

### <a name="example-1-re-create-the-table-with-a-new-distribution-column"></a>Primjer 1: Ponovno stvorite tablicu s novom stupcu distribuciju.

U ovom se primjeru koristi [[CTAS]] da biste ponovno stvorili tablicu sa stupcem različite raspršivanje raspodjele. 

```sql
CREATE TABLE [dbo].[FactInternetSales_CustomerKey] 
WITH (  CLUSTERED COLUMNSTORE INDEX
     ,  DISTRIBUTION =  HASH([CustomerKey])
     ,  PARTITION       ( [OrderDateKey] RANGE RIGHT FOR VALUES (   20000101, 20010101, 20020101, 20030101
                                                                ,   20040101, 20050101, 20060101, 20070101
                                                                ,   20080101, 20090101, 20100101, 20110101
                                                                ,   20120101, 20130101, 20140101, 20150101
                                                                ,   20160101, 20170101, 20180101, 20190101
                                                                ,   20200101, 20210101, 20220101, 20230101
                                                                ,   20240101, 20250101, 20260101, 20270101
                                                                ,   20280101, 20290101
                                                                )
                        )
    )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
OPTION  (LABEL  = 'CTAS : FactInternetSales_CustomerKey')
;

--Create statistics on new table
CREATE STATISTICS [ProductKey] ON [FactInternetSales_CustomerKey] ([ProductKey]);
CREATE STATISTICS [OrderDateKey] ON [FactInternetSales_CustomerKey] ([OrderDateKey]);
CREATE STATISTICS [CustomerKey] ON [FactInternetSales_CustomerKey] ([CustomerKey]);
CREATE STATISTICS [PromotionKey] ON [FactInternetSales_CustomerKey] ([PromotionKey]);
CREATE STATISTICS [SalesOrderNumber] ON [FactInternetSales_CustomerKey] ([SalesOrderNumber]);
CREATE STATISTICS [OrderQuantity] ON [FactInternetSales_CustomerKey] ([OrderQuantity]);
CREATE STATISTICS [UnitPrice] ON [FactInternetSales_CustomerKey] ([UnitPrice]);
CREATE STATISTICS [SalesAmount] ON [FactInternetSales_CustomerKey] ([SalesAmount]);

--Rename the tables
RENAME OBJECT [dbo].[FactInternetSales] TO [FactInternetSales_ProductKey];
RENAME OBJECT [dbo].[FactInternetSales_CustomerKey] TO [FactInternetSales];
```

### <a name="example-2-re-create-the-table-using-round-robin-distribution"></a>Primjer 2: Ponovno stvaranje tablice pomoću kružnog distribucije

U ovom se primjeru koristi [[CTAS]] da biste ponovno stvorili tablicu s kružnog umjesto raspršivanje distribuciju. Ta promjena će stvoriti čak i distribucija podataka teret premještanja povećana podataka. 

```sql
CREATE TABLE [dbo].[FactInternetSales_ROUND_ROBIN] 
WITH (  CLUSTERED COLUMNSTORE INDEX
     ,  DISTRIBUTION =  ROUND_ROBIN
     ,  PARTITION       ( [OrderDateKey] RANGE RIGHT FOR VALUES (   20000101, 20010101, 20020101, 20030101
                                                                ,   20040101, 20050101, 20060101, 20070101
                                                                ,   20080101, 20090101, 20100101, 20110101
                                                                ,   20120101, 20130101, 20140101, 20150101
                                                                ,   20160101, 20170101, 20180101, 20190101
                                                                ,   20200101, 20210101, 20220101, 20230101
                                                                ,   20240101, 20250101, 20260101, 20270101
                                                                ,   20280101, 20290101
                                                                )
                        )
    )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
OPTION  (LABEL  = 'CTAS : FactInternetSales_ROUND_ROBIN')
;

--Create statistics on new table
CREATE STATISTICS [ProductKey] ON [FactInternetSales_ROUND_ROBIN] ([ProductKey]);
CREATE STATISTICS [OrderDateKey] ON [FactInternetSales_ROUND_ROBIN] ([OrderDateKey]);
CREATE STATISTICS [CustomerKey] ON [FactInternetSales_ROUND_ROBIN] ([CustomerKey]);
CREATE STATISTICS [PromotionKey] ON [FactInternetSales_ROUND_ROBIN] ([PromotionKey]);
CREATE STATISTICS [SalesOrderNumber] ON [FactInternetSales_ROUND_ROBIN] ([SalesOrderNumber]);
CREATE STATISTICS [OrderQuantity] ON [FactInternetSales_ROUND_ROBIN] ([OrderQuantity]);
CREATE STATISTICS [UnitPrice] ON [FactInternetSales_ROUND_ROBIN] ([UnitPrice]);
CREATE STATISTICS [SalesAmount] ON [FactInternetSales_ROUND_ROBIN] ([SalesAmount]);

--Rename the tables
RENAME OBJECT [dbo].[FactInternetSales] TO [FactInternetSales_HASH];
RENAME OBJECT [dbo].[FactInternetSales_ROUND_ROBIN] TO [FactInternetSales];
```

## <a name="next-steps"></a>Daljnji koraci

Da biste saznali više o dizajna tablice, potražite u članku [Raspodijeli][], [Index][], [particija][], [Vrste podataka][], [Statistika][] i[privremene] članaka [Privremenih tablica].

Pregled najbolje prakse, potražite u članku [Najbolje prakse skladištu podataka SQL][].


<!--Image references-->

<!--Article references-->
[Pregled]: ./sql-data-warehouse-tables-overview.md
[Vrste podataka]: ./sql-data-warehouse-tables-data-types.md
[Distribucija]: ./sql-data-warehouse-tables-distribute.md
[Indeks]: ./sql-data-warehouse-tables-index.md
[Particija]: ./sql-data-warehouse-tables-partition.md
[Statistika]: ./sql-data-warehouse-tables-statistics.md
[Privremeni]: ./sql-data-warehouse-tables-temporary.md
[Najbolje prakse skladištu podataka SQL]: ./sql-data-warehouse-best-practices.md
[Nadzor upita]: ./sql-data-warehouse-manage-monitor.md
[dbo.vTableSizes]: ./sql-data-warehouse-tables-overview.md#querying-table-sizes

<!--MSDN references-->
[DBCC PDW_SHOWSPACEUSED()]: https://msdn.microsoft.com/library/mt204028.aspx

<!--Other Web references-->
