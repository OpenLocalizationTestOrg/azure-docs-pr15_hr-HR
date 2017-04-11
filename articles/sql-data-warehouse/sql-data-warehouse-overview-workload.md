<properties
   pageTitle="Radno opterećenje skladištu podataka"
   description="SQL Data Warehouse elasticity omogućuje povećavanje, Smanji ili pauziranje računalnim power pomoću skale sliding jedinica skladištu podataka (DWUs). U ovom se članku objašnjava skladištu metriku podataka i kakav je odnos između DWUs. "
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="07/25/2016"
   ms.author="barbkess;mausher;jrj;sonyama"/>


# <a name="data-warehouse-workload"></a>Radno opterećenje skladištu podataka
Radno opterećenje skladištu podataka odnosi se na sve operacije koje transpire na temelju podataka skladištu. Skladištu povećavaju podataka obuhvaća cijeli postupak učitavanja podataka u skladištu analiziranja i izvješćivanje o pogreškama na skladištu podataka, upravljanje podacima u skladištu podataka i izvoz podataka iz skladištu podataka. Dubine i breadth te komponente često su commensurate s razinom dospijeće skladištu podataka.


## <a name="new-to-data-warehousing"></a>Novi skladištenju?
Podaci skladištu je zbirka podataka koje je učitana iz izvora podataka i koristi se za obavljanje zadataka poslovne inteligencije kao što je analiza podataka i izvješćivanje o pogreškama.

Skladištima podataka određene su upiti koji skeniranje veći broj redaka, velike raspone podataka i može vratiti razmjerno velike rezultate u svrhu analizu i izvješćivanje. Skladištima podataka i su određene argumentima razmjerno velike podataka opterećenje nasuprot small transakcije razinom umeće/ažuriranja/briše.

- Podataka skladištu najbolje se izvodi kad se podaci spremaju na način koji optimizira upite koje je potrebno pregledati velikog broja redaka ili velike raspone podataka. Ta vrsta pregleda najbolje funkcionira kada podaci se pohranjuju, a pretražuje po stupcima, umjesto redaka.

>[AZURE.NOTE] Indeksa u memoriji columnstore koji koristi pohranu stupac sadrži do 10 x pozitivne sažimanja i 100 x pozitivne performanse upita putem klasične binarni stabala za izvješćivanje i analitiku upite. Ne možemo uzmite u obzir columnstore indeksi kao standard za pohranu i pregledavanje velike podataka u podataka skladištu.

- Podaci skladištu preduvjeti se razlikuju od sustav koji optimizira online transakcije obrade (OLTP). Sustav OLTP ima mnoge Umetanje, ažuriranje i brisanje operacije. Traženje te operacije na određene retke u tablici. Tablica takvu izvršiti najbolje kada su podaci spremljeni u način za redak po redak. Podatke možete sortirati i brzo pretražuju s dijeljenja i conquer pristup naziva binarni stabla ili btree pretraživanja.


## <a name="data-loading"></a>Učitavanje podataka
Učitavanje podataka je prevelik dio skladištu povećavaju podataka. Tvrtke obično je zauzet OLTP sustav koji prati promjene tijekom dana kada korisnici stvaraju poslovne transakcije. Povremeno, često tijekom noći tijekom održavanja prozor, transakcije su premještene ili kopirane na skladištu podataka. Kada se podaci nalaze u skladištu podataka, analitičar analitičar podataka možete provesti analizu i donošenje odluka tvrtke na podacima.

- Najčešći postupak učitavanja poziva ETL Izdvoji, transformacija i Učitaj. Podataka obično mora transformacije tako da bude usklađene s drugim podacima u skladištu podataka. Prethodno, tvrtke koristi poseban ETL poslužitelj za izvođenje pretvorbe. Sada s tako brzo massively paralelno obrada možete podatke učitali u SQL Data Warehouse najprije i izvedite pretvorbe. Ovaj postupak zove Izdvoji, učitavanje i pretvaranje (ELT), a postaje novi standard za radno opterećenje skladištu za podatke.

> [AZURE.NOTE] U programu SQL Server 2016 sada možete izvršavati analitičke podatke u stvarnom vremenu na tablici OLTP. To ne zamjenjuje potrebe za podataka skladištu za pohranu i analiza podataka, no nudi način za analizu u stvarnom vremenu.

### <a name="reporting-and-analysis-queries"></a>Izvješćivanje i analiza upita
Izvješćivanje i analiza upita često se kategorizirati u mali, Srednje i velike na temelju više kriterija, ali obično se temelji na vrijeme. U većini skladištima podataka, postoji kombiniranim povećavaju brzo pokretanje nasuprot dugoročnih upita. U svakom slučaju, važno je da biste odredili ovaj mix i da biste odredili njegov učestalost (hourly, svaki dan, mjesec podrobne, Kvartal završetka i tako dalje). Da biste shvatili da radno opterećenje kombiniranim upita povezano s istodobnosti, važno je potencijalnih klijenata proper kapacitet Planiranje podataka skladištu.

- Planiranje kapaciteta može biti složeno za radno opterećenje kombiniranim upita, osobito kada vam je potrebna dugo koliko će se dodati kapacitet skladištu podataka. SQL Data Warehouse uklanja hitnosti planiranja kapaciteta jer možete Povećaj i Smanji računalnim kapaciteta u bilo kojem trenutku i od prostora za pohranu i računalnim kapaciteta su neovisno veličine.

### <a name="data-management"></a>Upravljanje podacima
Upravljanje podacima je važno, osobito kada znate moglo bi vam ponestati prostora na disku u skorijoj budućnosti. Skladištima podataka obično podjelu podataka smisleni raspona koje se spremaju kao particije u tablici. Svi proizvodi utemeljene na SQL Server omogućuju premještanje particije i tablice. Particija prijelaz omogućuje premještanje starijim podacima manje skupi prostor za pohranu i pritom zadržati najnovije podatke na internetski prostor za pohranu.

- Indeksi Columnstore podržava particioniranom tablice. Za kazala columnstore particioniranom tablice se koriste za upravljanje podacima i arhiviranje. Za tablice koje se pohranjuju redak po redak, particije imati veći uloga u performansama upita.  

- PolyBase reproducira važnu ulogu u upravljanju podacima. Imate pomoću PolyBase, mogućnost za arhiviranje starijih podataka Hadoop ili Azure blobova.  To nudi razne mogućnosti Budući da je odjavio podatke.  Može trajati dulje za dohvaćanje podataka iz Hadoop, ali tradeoff vrijeme dohvaćanja možda su trošak prostora za pohranu.

### <a name="exporting-data"></a>Izvoz podataka
Da biste oslobodili za izvješća i analiza podataka je slanje podataka iz skladištu podataka na poslužitelje rezervirana za pokretanje izvješća i analiza. Sljedećim poslužiteljima nazivaju marts podataka. Ako, na primjer, vi unaprijed obradu podataka izvješća i izvoz je iz skladištu podataka na mnogo poslužitelje svijeta da biste učiniti dostupnima za kupce i analitičar analitičar podataka.

- Za generiranje izvješća, svaki noći nije popunjavanja samo za čitanje izvješćivanja poslužitelji snimku dnevnih podataka. Tako ćete dobiti veću propusnost za kupce tijekom smanjiti potrebe za resursima računalnim na skladištu podataka. Iz razmjer na sigurnost podataka marts omogućuju vam da smanjite broj korisnicima koji imaju pristup skladištu podataka.
- Za analizu, možete izraditi kocki komponente analysis na skladištu podataka i pokrećete skladištu podatke za analizu ili unaprijed obrada podataka i izvoz s poslužiteljem analizu za daljnje analize.

## <a name="next-steps"></a>Daljnji koraci
Sad kad znate nekoliko riječi o SQL Data Warehouse, Saznajte kako brzo [stvoriti SQL Data Warehouse][] i [Učitavanje ogledne podatke][].

<!--Image references-->

<!--Article references-->
[Učitavanje oglednih podataka]: ./sql-data-warehouse-load-sample-databases.md
[Stvaranje SQL Data Warehouse]: ./sql-data-warehouse-get-started-provision.md

<!--MSDN references-->

<!--Other web references-->
