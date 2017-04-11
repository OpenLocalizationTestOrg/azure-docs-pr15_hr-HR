<properties
    pageTitle="Pregled upita elastic baze podataka Azure SQL baze podataka | Microsoft Azure"
    description="Pregled značajki elastic upita"    
    services="sql-database"
    documentationCenter=""  
    manager="jhubbard"
    authors="torsteng"/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/27/2016"
    ms.author="torsteng" />

# <a name="azure-sql-database-elastic-database-query-overview-preview"></a>Azure pregled (pretpregled) baze podataka SQL elastic baze podataka upita

Značajka upita elastic baze podataka (u pretpregledu) omogućuje vam da biste pokrenuli Transact-SQL upit koji obuhvaća više baza podataka u Azure SQL baze podataka (SQLDB). Omogućuje izvođenje upita izdvojiti bazu podataka da biste pristupili udaljene tablica i povezivanje alata Microsoft i drugih proizvođača (Excel PowerBI, Tableau, itd.) za slanje upita za podatke razine s više baza podataka. Pomoću ove značajke možete skaliranje out upite za razine velike podataka u bazi podataka SQL i vizualni prikaz rezultata u izvješćima poslovne inteligencije (BI).

## <a name="documentation"></a>Dokumentacija

* [Početak rada s upiti unakrsno-baze podataka](sql-database-elastic-query-getting-started-vertical.md)
* [Prijavite se putem oblaka skalirana iz baze podataka](sql-database-elastic-query-getting-started.md)
* [Slanje upita za baze podataka sharded oblaka (vodoravno particije)](sql-database-elastic-query-horizontal-partitioning.md)
* [Slanje upita za baze podataka u oblaku s različitim shema (okomito particije)](sql-database-elastic-query-vertical-partitioning.md)
* [SP\_izvršavanje \_remote](https://msdn.microsoft.com/library/mt703714)


## <a name="why-use-elastic-queries"></a>Zašto koristiti elastic upita?

**Baze podataka Azure SQL**

Slanje upita za baze podataka Azure SQL potpuno u T-SQL. Time se omogućuje za ispitivanje udaljena baza podataka samo za čitanje. To nudi mogućnost za trenutni Lokalni korisnici sustava SQL Server da biste migrirali aplikacije koje koriste tri - a four - part imena ili povezanog poslužitelja SQL DB.

**Dostupno u standardnom sloju** Elastic upita podržana je u sloju standardne performansi uz performanse sloju Premium. U odjeljku Pretpregled ograničenja ispod na performanse ograničenja za niže razine performansi.

**Automatske udaljena baza podataka**

Elastic upita sada možete automatske SQL parametara udaljena baza podataka za izvršavanje.

**Pohranjena procedura izvođenja**

Izvršavanje poziva daljinske pohranjene procedure ili udaljenom funkcije pomoću [sp\_izvršavanje \_udaljene](https://msdn.microsoft.com/library/mt703714).

**Fleksibilnost**

Vanjski tablice s elastic upita sada može se odnositi na udaljenom tablice s različitim shemu ili naziv tablice.

## <a name="elastic-database-query-scenarios"></a>Scenariji upita elastic baze podataka

Cilj je da biste olakšali upite scenariji kojima više baza podataka priložiti redaka u jedan ukupni rezultat. Upit se može sastojati ili korisnik ili aplikacija izravno ili neizravno putem alata koji su povezani s bazom podataka. To je posebno korisno prilikom stvaranja izvješća na temelju komercijalne BI ili podataka Alati za integraciju – ili bilo koju aplikaciju koja nije moguće promijeniti. Uz upit s elastic upita preko nekoliko baze podataka u programu poznato okruženje sustava SQL Server povezivanjem u alatima kao što je Excel, PowerBI, Tableau ili Cognos.
Elastic upit omogućuje jednostavan pristup cijelu zbirku baze podataka pomoću upita izdala SQL Server Management Studio ili Visual Studio i olakšava upita izdvojiti bazu podataka iz Framework entitet ili druge BRAZAC okruženja. Slika 1 prikazuje scenarija gdje postojeće oblaka aplikacije (koji se koristi u [biblioteci klijent elastic baze podataka](sql-database-elastic-database-client-library.md)) sastavlja na sloj skalirana iz podataka i elastic upita se koristi za izvješćivanje izdvojiti bazu podataka.

**Slika 1** Upit elastic baze podataka koji se koristi u sloju skalirana iz podataka

![Upit elastic baze podataka koji se koristi u sloju skalirana iz podataka][1]

Scenariji za klijenta za elastic upit su određene argumentima sljedećih topologija:

* **Okomita particija – upiti unakrsno-baze podataka** (Topologije 1): okomito particije podataka između broj baza podataka u sloj podataka. Obično različite skupove tablica se nalaze na različite baze podataka. To znači da shemi razlikuje na različite baze podataka. Na primjer, sve tablice za zalihe su na jednoj bazi podataka dok su sve računovodstveni povezane tablice na drugu bazu podataka. Uobičajena slučaja koristi s ovom topologije potreban je jedan upit preko ili prikupiti izvješća preko tablica u nekoliko baza podataka.
* **Vodoravna particija – Sharding** (Topologije 2): vodoravno particije podataka za retke raspodijelite skaliranu izgleda podataka sloju. Na taj se način je jednake na sve baze podataka koji sudjeluju u shemi. Taj se način se zove "sharding". Sharding mogu izvršiti, a upravljanih bazi (1) elastic Alati biblioteke ili (2) samopotpisani-sharding. Elastic upit koristi za upit ili posložiti izvješća preko mnogo shards.

> [AZURE.NOTE] Najbolje funkcionira Povremeni izvješćivanja scenarijima kojima se većina obrade možete izvršiti na sloju podataka za upit elastic baze podataka. Za velike izvješćivanja radnih opterećenja ili skladištenje scenarijima podataka s više složene upite, također razmislite o korištenju [Azure SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/).


## <a name="elastic-database-query-topologies"></a>Elastic topologija upita baze podataka

### <a name="topology-1-vertical-partitioning--cross-database-queries"></a>1 topologija: Okomito particija – izdvojiti bazu podataka upita

Da biste započeli kodiranje, potražite u članku [Uvod u rad s upitom izdvojiti bazu podataka (okomito particija)](sql-database-elastic-query-getting-started-vertical.md).

Da biste podatke koji se nalaze u bazi podataka SQLDB dostupne drugim bazama podataka SQLDB se poslužite elastic upita. Time se omogućuje upita iz jedne baze podataka da biste se pozvali na tablice u bilo koju drugu udaljene SQLDB bazu podataka. U prvi je korak da biste definirali vanjskog izvora podataka za svaki udaljena baza podataka. Vanjski izvor podataka definirana je u lokalnu bazu podataka iz kojeg želite da biste pristupili tablice koja se nalazi na udaljenom bazom podataka. Nisu potrebne na udaljenom bazom podataka promjene. Uobičajeni okomiti stvaranje particija scenarijima za gdje različite baze podataka imaju različite sheme, elastic upiti mogu se implementirati uobičajena slučaja koristi kao što je pristup referentnih podataka i izdvojiti bazu podataka upita.

**Referentnih podataka**: 1 topologije služi za upravljanje podacima u referenci. Na slici u nastavku, dvije tablice (T1 i T2) s referentnih podataka čuvaju namjenski bazu podataka. Korištenje upita za elastic, sada možete pristupiti tablice T1 i T2 daljinski iz druge baze podataka, kao što je prikazano na slici. Korištenje topologije 1 ako je referenca tablice su small ili udaljenom upita u tablicu referenca imati selektivno predikati.

**Slika 2** Okomiti particija – pomoću elastic upita u upit referentnih podataka

![Okomiti particija – pomoću elastic upita u upit referentnih podataka][3]

**Slanje upita izdvojiti bazu podataka**: Elastic upiti omogućiti korištenje slučajeva koje je potrebno ispitivanje preko nekoliko SQLDB baza podataka. Slika 3 prikazuje četiri različite baze podataka: CRM, zalihe, ljudskih Resursa i Proizvodi. Upiti koji se izvode na jedan od baze podataka potreban i pristup jedne ili svih drugih baza podataka. Korištenje elastic upita možete konfigurirati bazu podataka za taj slučaj tako da pokrenete nekoliko jednostavnih DDL naredbe za svaku od četiri baze podataka. Nakon tu konfiguraciju jednokratni pristup udaljenoj tablici jednostavan je upućuju na lokalnu tablicu s upitima T-SQL ili vaše alatima za Poslovno obavještavanje. Ako se Udaljena upiti vraćaju rezultate iz velike, preporučuje se takvog.

**Slika 3** Okomiti particija – pomoću elastic upita u upit preko različitih baza podataka

![Okomiti particija – pomoću elastic upita u upit preko različitih baza podataka][4]

### <a name="topology-2-horizontal-partitioning--sharding"></a>Topologija 2: Vodoravne particija – sharding

Korištenje elastic upita za izvođenje izvješćivanja zadataka putem sharded, odnosno, vodoravno particije, razina podataka potreban [karte shard elastic baze podataka](sql-database-elastic-scale-shard-map-management.md) za predstavljanje baze podataka u sloju podataka. Obično koristi samo jedan shard karte u ovom scenariju i namjenski baze podataka pomoću mogućnosti elastic upita služi kao točku unosa za izvješćivanje upita. Samo ova namjenski baza podataka potreban pristup shard kartu. Slika 4 prikazuje ova Topologija i svoju konfiguraciju s kartom elastic upita baze podataka i shard. Baza podataka u sloju podataka može biti bilo koje verzije baze podataka SQL Azure i edition. Dodatne informacije o biblioteci klijent elastic baze podataka i stvaranje shard karte potražite u članku [Shard mapiranje upravljanje](sql-database-elastic-scale-shard-map-management.md).

**Slika 4** Vodoravna particija – korištenje elastic upita za izvješćivanje putem razine sharded podataka

![Vodoravna particija – korištenje elastic upita za izvješćivanje putem razine sharded podataka][5]

> [AZURE.NOTE] Baza podataka za upit namjenski elastic baze podataka mora biti v12 bazom podataka SQL. Nema ograničenja na shards sami.

Da biste započeli kodiranje, potražite u članku [Uvod u rad s elastic upita baze podataka za vodoravno particija (sharding)](sql-database-elastic-query-getting-started.md).

## <a name="implementing-elastic-database-queries"></a>Implementacijom elastic upita baze podataka

Koraci za implementaciju elastic upit za stvaranje particija scenariji okomitim i vodoravnim se spominju u sljedećim odjeljcima. Oni i pogledajte detaljnije dokumentaciju za stvaranje particija scenarija.

### <a name="vertical-partitioning---cross-database-queries"></a>Okomiti particija - izdvojiti bazu podataka upita

Sljedeći koraci konfigurirati elastic upita baze podataka za okomite stvaranje particija scenarija koji zahtijevaju pristup u tablicu koja se nalazi na udaljenom SQLDB bazama podataka na istu shemu.

*    [STVORITE GLAVNI KLJUČ](https://msdn.microsoft.com/library/ms174382.aspx) mymasterkey
*    [Stvaranje baze podataka iz DJELOKRUGA VJERODAJNICA](https://msdn.microsoft.com/library/mt270260.aspx) mycredential
*    [Stvaranje/PADAJUĆE VANJSKI IZVOR podataka](https://msdn.microsoft.com/library/dn935022.aspx) mydatasource vrste **RDBMS**
*    [VANJSKA TABLICA Stvori/PADAJUĆE](https://msdn.microsoft.com/library/dn935021.aspx) MojaTablica

Nakon izvođenja DDL naredbe, možete pristupiti daljinski tablice "MojaTablica" kao da je lokalne tablice. Baze podataka SQL Azure automatski otvara vezu s udaljenom bazom podataka, obrađuje vaš zahtjev na udaljenom bazom podataka i vraća rezultat.
Dodatne informacije o koraci potrebni za okomite stvaranje particija scenarij pronaći ćete u [elastic upita za particija okomito](sql-database-elastic-query-vertical-partitioning.md).  

### <a name="horizontal-partitioning---sharding"></a>Vodoravna particija - sharding

Sljedeći koraci konfigurirati elastic upita baze podataka za vodoravno scenarije stvaranje particija koji zahtijevaju pristup skup tablice koje se nalaze na (obično) nekoliko udaljene SQLDB baza podataka:

*    [STVORITE GLAVNI KLJUČ](https://msdn.microsoft.com/library/ms174382.aspx) mymasterkey
*    [Stvaranje baze podataka iz DJELOKRUGA VJERODAJNICA](https://msdn.microsoft.com/library/mt270260.aspx) mycredential
*    Stvaranje [karte shard](sql-database-elastic-scale-shard-map-management.md) koji predstavlja vaš sloju podataka pomoću klijentska biblioteka elastic baze podataka.   
*    [Stvaranje/PADAJUĆE VANJSKI IZVOR podataka](https://msdn.microsoft.com/library/dn935022.aspx) mydatasource vrste **SHARD_MAP_MANAGER**
*    [VANJSKA TABLICA Stvori/PADAJUĆE](https://msdn.microsoft.com/library/dn935021.aspx) MojaTablica

Nakon što izvršite ove korake, možete pristupiti vodoravno particioniranom tablice "MojaTablica" kao da je lokalne tablice. Baze podataka SQL Azure automatski otvara višestruke paralelno veze udaljene baze podataka u tablicama se fizički pohranjuju, obrađuje zahtjeve na udaljena baza podataka i vraća rezultat.
Dodatne informacije o koraci potrebni za vodoravno stvaranje particija scenarij pronaći ćete u [elastic upit za vodoravno particija](sql-database-elastic-query-horizontal-partitioning.md).

## <a name="t-sql-querying"></a>T-SQL upita
Kada definirate vaše vanjskih izvora podataka i vanjske tablice, možete koristiti obične nizove veze za SQL Server da biste se povezali s bazama podataka koje ste definirali vanjske tablice. Možete zatim pokrenuti T SQL naredbe nad vanjskim tablica na tu vezu s ograničenjima navedene u nastavku. Dodatne informacije i primjeri T SQL upite možete pronaći u dokumentaciji o temama za [particija vodoravna](sql-database-elastic-query-horizontal-partitioning.md) i [okomita particija](sql-database-elastic-query-vertical-partitioning.md).

## <a name="connectivity-for-tools"></a>Povezivanje za alate
Uobičajeni nizove veze za SQL Server možete koristiti da biste se povezali aplikacije i Alati za integraciju BI ili podataka s bazama podataka koje sadrže vanjske tablice. Provjerite je li SQL Server podržana kao izvor podataka za vaše alat. Nakon uspostave pogledajte elastic upita baze podataka i vanjske tablice u toj bazi podataka kao što želite učiniti s bilo koje druge baze podataka SQL Server koji se povezuju sa svoje alatom.

> [AZURE.IMPORTANT] Provjera autentičnosti pomoću servisa Azure Active Directory elastic upita trenutno nisu podržani.

## <a name="cost"></a>Trošak

Elastic upita uvrštava u trošak baze podataka SQL Azure baze podataka. Imajte na umu da podržava topologija gdje su udaljene baze podataka u centru za različite podatke od krajnje elastic upita, ali izlazne podatke iz udaljena baza podataka se naplaćuju običnog [Azure stope](https://azure.microsoft.com/pricing/details/data-transfers/).

## <a name="preview-limitations"></a>Ograničenja za pretpregled
* Izvodi prvog elastic upita može potrajati nekoliko minuta na standardni performanse sloju. Ovaj put je potrebno da biste učitali funkcije elastic upita; Učitavanje performanse poboljšava s više razine performansi.
* Skriptiranje vanjskim izvorima podataka ili vanjski tablice iz SSMS ili SSDT još nije podržana.
* Uvoz/izvoz za SQL DB još ne podržava vanjskih izvora podataka i vanjske tablice. Ako je potrebno koristiti uvoz/izvoz ispustite te objekte prije izvoza, a zatim ih ponovno stvorite nakon uvoza.
* Upit elastic baze podataka trenutno podržava samo pristup samo za čitanje na vanjski tablice. Međutim, koristite punu funkcionalnost telefona T SQL baze podataka kojima se definira vanjska tablica. To može biti korisno, npr., zadržava privremene rezultata pomoću, npr., odaberite < column_list > u < local_table > ili da biste definirali pohranjene procedure elastic upita baze podataka koja se odnose na vanjske tablice.
* Osim nvarchar(max), LOB vrste nisu podržane u definicije vanjska tablica. Kao zaobilazno rješenje, možete stvoriti prikaz na udaljenom bazom podataka koja se oblikuje vrstu LOB u nvarchar(max), definirati vanjska tablica putem prikaza umjesto Osnovna tablica i pa ga pretvoriti u Izvorna vrsta LOB u upitima programa.
* Statistika stupca nad vanjskim tablice trenutno nije podržano. Statistika tablice podržane su, ali morate ručno stvorili.

## <a name="feedback"></a>Povratne informacije
Ponovno slanje povratnih informacija o iskustva s elastic upiti s nama na Disqus ispod, forumi MSDN ili Stackoverflow. Ne možemo vas zanimaju sve vrste povratne informacije o servisu (nedostataka, grubi rubove, značajka praznine).

## <a name="more-information"></a>Dodatne informacije

U sljedećim dokumentima možete pronaći dodatne informacije o ispitivanje više bazu podataka i okomiti stvaranje particija scenarije:

* [Slanje upita izdvojiti bazu podataka i okomiti stvaranje particija pregled](sql-database-elastic-query-vertical-partitioning.md)
* Isprobajte naše detaljne Praktični vodič da biste potpun dobrog primjera koji se izvodi u minutama: [Uvod u rad s upitom izdvojiti bazu podataka (okomito particija)](sql-database-elastic-query-getting-started-vertical.md).


Dodatne informacije o vodoravni particija i sharding scenariji dostupna je u nastavku:

* [Vodoravna particija i sharding pregled](sql-database-elastic-query-horizontal-partitioning.md)
* Isprobajte naše detaljne Praktični vodič da biste potpun dobrog primjera koji se izvodi u minutama: [Uvod u rad s elastic upita baze podataka za vodoravno particija (sharding)](sql-database-elastic-query-getting-started.md).


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-query-overview/overview.png
[2]: ./media/sql-database-elastic-query-overview/topology1.png
[3]: ./media/sql-database-elastic-query-overview/vertpartrrefdata.png
[4]: ./media/sql-database-elastic-query-overview/verticalpartitioning.png
[5]: ./media/sql-database-elastic-query-overview/horizontalpartitioning.png

<!--anchors-->
