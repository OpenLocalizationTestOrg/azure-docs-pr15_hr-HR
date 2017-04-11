<properties
   pageTitle="Upravljanje računalnim power u skladištu podataka SQL Azure (pregled) | Microsoft Azure"
   description="Performanse skaliranje više mogućnosti u skladištu podataka za SQL Azure. Skaliranje prilagodbom DWUs ili zaustaviti i nastaviti računalnim resursi troškova."
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
   ms.date="09/03/2016"
   ms.author="barbkess;sonyama"/>

# <a name="manage-compute-power-in-azure-sql-data-warehouse-overview"></a>Upravljanje računalnim power u skladištu podataka SQL Azure (pregled)

> [AZURE.SELECTOR]
- [Pregled](sql-data-warehouse-manage-compute-overview.md)
- [Portal](sql-data-warehouse-manage-compute-portal.md)
- [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
- [OSTALE](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)

Arhitektura SQL Data Warehouse odvaja i računalnim, dopustite svaki skaliranja neovisno prostor za pohranu. Zbog toga skaliranje out performanse prilikom spremanja troškove plaćanjem samo performanse kada vam je potrebna. 

Ovaj pregled opisuje sljedeće mogućnosti skaliranje iz performanse SQL Data Warehouse i daje preporuke o tome kako i kada ih koristiti. 

- Promjena veličine izračunati power prilagodbom [podataka skladištu jedinice (DWUs)][]
- Zaustavljanje računalnim resursi

<a name="scale-performance-bk"></a>

## <a name="scale-performance"></a>Promjena veličine performansi

U SQL Data Warehouse možete brzo skaliranje performanse izgleda ili ponovno rastućim računalnim resurse procesora i memorije/i propusnosti ili. Da biste skalirali performanse, sve što trebate napraviti je podesiti broj [podataka skladištu jedinice (DWUs)][] koji SQL Data Warehouse dodjeljuje u bazu podataka. SQL Data Warehouse brzo omogućuje promjenu i rukuje svi temeljni promjene hardvera i softvera.

Nestaje su dani tamo gdje vam je potreban da biste istražili koju vrstu procesora, koliko memorije ili koju vrstu spremišta potrebne su odličan performanse u skladištu podataka. Postavljanjem Data Warehouse u oblaku, više ne morate baviti najniže razine hardverske probleme. Umjesto toga SQL Data Warehouse Traži ovo pitanje: brzinu želite li za analizu podataka? 

### <a name="how-do-i-scale-performance"></a>Kako se skaliranje performansi?

Da biste elastically povećali ili smanjili struje računalnim, jednostavno promijenite postavku [podataka skladištu jedinice (DWUs)][] za bazu podataka. Performanse povećava se linearno prilikom dodavanja više DWU.  Na više razine DWU, morate dodati više od 100 DWUs da biste obratite pozornost na značajnu poboljšanja performansi. Da biste lakše odabrali smisleni Skače u DWUs, nudimo razine DWU koje daju najbolje rezultate.
 
Da biste prilagodili DWUs, možete koristiti bilo koji od ovih metoda pojedinačne.

- [Promjena veličine izračunati power pomoću portala za Azure][]
- [Promjena veličine izračunati power sa servisom PowerShell][]
- [Promjena veličine računalnim power s REST API-ji][]
- [Promjena veličine izračunati power s TSQL][]

### <a name="how-many-dwus-should-i-use"></a>Koliko DWUs trebao koristiti?
 
Da biste u SQL Data Warehouse performanse mijenja veličinu Linearno i promjena iz jedne računalnim skaliranje na drugu (recimo od 100 DWUs za 2000 DWUs) se događa u sekundama. Tako ćete dobiti fleksibilnost isprobati različite postavke DWU dok ne odredite vašu situaciju najbolje pristajanje.

Da biste razumjeli što je to prikladno DWU vrijednosti, pokušajte skaliranje gore i dolje i pokrenut nekoliko upita nakon učitavanja podataka. Budući da je brzi skaliranje, možete ga isprobati broj različite razine performanse u jedan sat ili manje. Imajte na umu SQL Data Warehouse namijenjen za obradu velike količine podataka te da biste vidjeli prave mogućnosti skaliranja, osobito na veću nudimo ljestvice ćete koji želite koristiti u velikom skupu podataka koji izvršenja ili premašuje 1 TB.

Preporuke za pronalaženje najbolje DWU za svoje radno opterećenje:

1. Skladištu podataka u razvoju, započnite odabirom mali broj DWUs.  Dobru početnu točku je DW400 ili DW200.
2. Praćenje performansi aplikacije, opažanja odabran broj DWUs u usporedbi s performansama se.
3. Odredite koliko brže ili sporije performanse treba vam dosegne razinu optimalne performanse za potrebama, pod pretpostavkom linearni mjerilo.
4. Povećajte ili smanjite broj DWUs razmjerno kako mnogo brže ili sporije želite svoje radno opterećenje za izvođenje. Servis će brže i prilagodite računalnim resursa koji će zadovoljava preduvjete za nove DWU.
5. Nastavite s prilagodbama dok ne dođete do razinu optimalne performanse za preduvjeti za tvrtke.

### <a name="when-should-i-scale-dwus"></a>Kada skaliranje DWUs?

Kada vam je potrebna brže rezultate, povećajte vaše DWUs i platiti za veće performansi.  Kada morate manje izračunati power, smanjite vaše DWUs i platiti samo za što vam je potrebno. 

Preporuke za kada za promjenu veličine DWUs:

1. Ako je vaša aplikacija koje fluktuiraju radno opterećenje, skala DWU razine prema gore ili dolje da biste primiti peaks i niska točke. Na primjer, ako svoje radno opterećenje peaks obično na kraju mjeseca, namjeravate dodajte dodatne DWUs tijekom te Vršna dana, a zatim skaliranje kada Vršna razdoblje.
2. Prije nego što načinite podebljanom podataka operacije učitavanje i transformaciju, DWUs proširenja tako da se podaci su dostupni brže.

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Zaustavljanje računalnim

[AZURE.INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Pauziranje baze podataka, koristite neku od ovih metoda pojedinačne.

- [Zaustavljanje računalnim pomoću portala za Azure][]
- [Zaustavljanje računalnim sa servisom PowerShell][]
- [Zaustavljanje računalnim s REST API-ji][]

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Životopis računalnim

[AZURE.INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

Da biste nastavili baze podataka, koristite neku od ovih metoda pojedinačne.

- [Životopis računalnim pomoću portala za Azure][]
- [Životopis računalnim sa servisom PowerShell][]
- [Životopis računalnim s REST API-ji][]

## <a name="permissions"></a>Dozvole

Promjena veličine baze podataka potrebno dozvole opisane u [ALTER baze podataka][].  Zaustavljanje i nastavak potrebno dozvolu za [Suradnika DB SQL][] , posebno Microsoft.Sql/servers/databases/action.

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Daljnji koraci
Pogledajte sljedeće članke lakše shvatili koncepte neke dodatne ključnog učinka:

- [Upravljanje radno opterećenje i istodobnosti][]
- [Pregled dizajna tablice][]
- [Tablica raspodjele][]
- [Indeksiranje tablice][]
- [Particija tablice][]
- [Statistički podaci o tablice][]
- [Najbolje prakse][]

<!--Image reference-->

<!--Article references-->
[jedinice skladištu podataka (DWUs)]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units

[Promjena veličine izračunati power pomoću portala za Azure]: ./sql-data-warehouse-manage-compute-portal.md#scale-compute-bk
[Promjena veličine izračunati power sa servisom PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#scale-compute-bk
[Promjena veličine računalnim power s REST API-ji]: ./sql-data-warehouse-manage-compute-rest-api.md#scale-compute-bk
[Promjena veličine izračunati power s TSQL]: ./sql-data-warehouse-manage-compute-tsql.md#scale-compute-bk

[capacity limits]: ./sql-data-warehouse-service-capacity-limits.md

[Zaustavljanje računalnim pomoću portala za Azure]:  ./sql-data-warehouse-manage-compute-portal.md#pause-compute-bk
[Zaustavljanje računalnim sa servisom PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#pause-compute-bk
[Zaustavljanje računalnim s REST API-ji]: ./sql-data-warehouse-manage-compute-rest-api.md#pause-compute-bk

[Životopis računalnim pomoću portala za Azure]:  ./sql-data-warehouse-manage-compute-portal.md#resume-compute-bk
[Životopis računalnim sa servisom PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#resume-compute-bk
[Životopis računalnim s REST API-ji]: ./sql-data-warehouse-manage-compute-rest-api.md#resume-compute-bk

[Upravljanje radno opterećenje i istodobnosti]: ./sql-data-warehouse-develop-concurrency.md
[Pregled dizajna tablice]: ./sql-data-warehouse-tables-overview.md
[Tablica raspodjele]: ./sql-data-warehouse-tables-distribute.md
[Indeksiranje tablice]: ./sql-data-warehouse-tables-index.md
[Particija tablice]: ./sql-data-warehouse-tables-partition.md
[Statistika tablice]: ./sql-data-warehouse-tables-statistics.md
[Najbolje prakse]: ./sql-data-warehouse-best-practices.md 
[development overview]: ./sql-data-warehouse-overview-develop.md

[SQL DB suradnika]: ../active-directory/role-based-access-built-in-roles.md#sql-db-contributor

<!--MSDN references-->
[ZAMIJENI BAZU PODATAKA]: https://msdn.microsoft.com/library/mt204042.aspx

<!--Other Web references-->
[Azure portal]: http://portal.azure.com/
