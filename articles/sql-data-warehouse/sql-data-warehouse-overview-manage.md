<properties
   pageTitle="Upravljanje bazama podataka u skladištu podataka za SQL Azure | Microsoft Azure"
   description="Pregled upravljanje bazama podataka za SQL Data Warehouse. Obuhvaća alate za upravljanje, DWUs i skaliranje Izlaz, otklanjanje poteškoća s performansama performanse upita, uspostavljanje dobar sigurnosne pravilnike i vraćanje baze podataka iz oštećenja podataka ili regionalne prekida."
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
   ms.date="08/16/2016"
   ms.author="barbkess;sonyama;"/>

# <a name="manage-databases-in-azure-sql-data-warehouse"></a>Upravljanje bazama podataka u skladištu podataka za SQL Azure

SQL Data Warehouse automatizira brojne aspekte upravljanje bazama podataka programa. Na primjer, da biste skalirali performanse samo morate prilagoditi platiti pravilnu razinu računalnim resurse i omogućuju SQL Data Warehouse obaviti posao skaliranje izgleda i skaliranje natrag. 

Undoubtedly želite nadzirati svoje radno opterećenje prepoznavanje vašim potrebama performanse kao i otklanjanje poteškoća s dugoročnih upita. Također ćete izvršiti nekoliko zadataka sigurnost za upravljanje dozvolama za korisnike i prijave.

U ovom pregled pokriva ove aspekte upravljanja SQL Data Warehouse.

- Alati za upravljanje
- Promjena veličine računalnim
- Zaustavljanje i životopisa
- Najbolje prakse performansi
- Nadzor upita
- Sigurnost
- Sigurnosno kopiranje i vraćanje

## <a name="management-tools"></a>Alati za upravljanje

Pomoću raznih alata za upravljanje bazama podataka u SQL Data Warehouse. Kao što je upravljanje bazama podataka, razvijate preference alat za svaku vrstu zadataka koje morate izvršiti.

### <a name="azure-portal"></a>Portal za Azure
[Portal za Azure][] je za portal koji se temelji na web mjesto možete stvoriti, ažurirati, i Izbriši baze podataka i praćenje baze podataka resursa. Ovaj alat je odličan je ako ste tek ste počeli raditi s Azure, upravljanje mali broj baza podataka skladištu podataka ili morate brzo učiniti nešto.

Početak rada s portala za Azure, potražite u članku [Stvaranje SQL Data Warehouse (portal za Azure)][].

### <a name="sql-server-data-tools-in-visual-studio"></a>SQL Server Data Tools u Visual Studio
[SQL Server Data Tools][] (SSDT) u Visual Studio radite dopušta povezivanje, upravljanje i razvoj baze podataka. Ako ste je razvojni inženjer upoznati s Visual Studio ili druge integriranih razvojnih okruženja (IDEs), isprobajte SSDT u Visual Studio.

SSDT obuhvaća Explorer objekta SQL Server koji omogućuje vizualni prikaz, povezivanje i izvršavanje skripti SQL Data Warehouse bazama podataka. Da biste brzo povezali SQL Data Warehouse, možete jednostavno kliknite gumb **Otvori u Visual Studio** na naredbenoj traci prilikom prikaza Detalji baze podataka na portalu klasični Azure.  

Početak rada s SSDT u Visual Studio, potražite u članku [Upita Azure SQL Data Warehouse s Visual Studio][].

### <a name="command-line-tools"></a>Alati naredbenog retka
Alati naredbenog retka idealne su za automatiziranje vaše radnih opterećenja.  PowerShell i sqlcmd dva načina sjajno da biste automatizirali procese.  Preporučujemo da se ove alate za upravljanje velik broj logičke poslužitelji i implementacija promjene resursa u okruženju proizvodnje kao što je potrebno zadataka možete postavljanje upita skriptiranih i zatim automatski.

### <a name="dynamic-management-views"></a>Dinamični Upravljanje prikazima 

DMVs su butter i kruha upravljanja SQL Data Warehouse. Gotovo sve informacije koje površine na portalu ovisi o DMVs. Da biste vidjeli popis DMVs skladištu podataka za SQL potražite u članku [Sistemski prikazi SQL Data Warehouse][].

Početak rada potražite u članku [Povezivanje i upit s sqlcmd][]i [Stvaranje baze podataka (PowerShell)][].

## <a name="scale-compute"></a>Promjena veličine računalnim

U SQL Data Warehouse možete brzo skaliranje performanse izgleda ili ponovno rastućim računalnim resurse procesora i memorije/i propusnosti ili. Da biste skalirali performanse, sve što trebate napraviti je podesiti broj jedinica na skladištu podataka (DWUs) koje SQL Data Warehouse dodjeljuje u bazu podataka. SQL Data Warehouse brzo omogućuje promjenu i rukuje svi temeljni promjene hardvera i softvera.

Da biste saznali više o skaliranje DWUs, potražite u članku [performanse mjerilo][].

##  <a name="pause-and-resume"></a>Zaustavljanje i životopisa

Da biste spremili troškove, možete zaustaviti i nastavak računalnim resursa na zahtjev. Ako, na primjer, ako nećete koristiti bazu podataka tijekom noći i dane vikenda, možete pauziranja te vrijeme i nastaviti tijekom dana. Dok je pauziran bazu podataka neće naplatiti za DWUs.

Dodatne informacije potražite u članku [Zaustavljanje izračunati][]i [izračunati životopis][].

## <a name="performance-best-practices"></a>Najbolje prakse performansi

Prilikom početka rada s novu tehnologiju, otkrivaju savjete i trikove funkcionira najbolje desno od početka možete uštedjeti veliku količinu vremena.  Tražit će najbolje prakse cijeloj mnoge naš teme.

Da biste vidjeli više sažetak najvažnije pitanja vezana uz prilikom razvoja svoje radno opterećenje, potražite u članku [Skladištu i najbolje prakse u podataka za SQL][].

## <a name="query-monitoring"></a>Nadzor upita

Ponekad upita traje predugo, ali niste sigurni koji je utvrditi uzrok. SQL Data Warehouse ima dinamički upravljanje prikaza (DMVs) koje možete koristiti da biste utvrdili koji upit traje predugo. 

Da biste pronašli dugoročnih upita, potražite u članku [praćenje svoje radno opterećenje pomoću DMVs][].

## <a name="security"></a>Sigurnost

Da biste zadržali sigurne sustava, mora biti na upozorenju i zaštite od bilo koju vrstu neovlaštenog pristupa. Sigurnosni sustav nije potrebno provjerite jesu li pravila vatrozida postavite tako da se samo ovlašteni IP adrese možete se povezati. Potrebne ispravnu autorizaciju korisničke vjerodajnice. Kada korisnik ima povezani s bazom podataka, korisnik mora imati samo dozvole za izvođenje Minimalni broj akcija. Radi zaštite podataka, možete koristiti šifriranje. Važno da bi je i nadzor i praćenje tako koje možete vratili na prošle događaje ako postoji neki sumnjivoj aktivnosti.

Dodatne informacije o upravljanju sigurnosti, lakši putem [Pregled sigurnosti][].

## <a name="backup-and-restore"></a>Sigurnosno kopiranje i vraćanje

Imate pouzdanog backps podataka je ključni dio bilo koji radni baze podataka. SQL Data Warehouse čuva podatke koji sigurni tako da automatski sigurnosno kopiranje aktivne baze podataka u pravilnim vremenskim razmacima. Ove sigurnosne kopije omogućuju oporaviti iz scenarija koji ste oštećenja podataka ili slučajno prekine podataka ili baze podataka.  Raspored sigurnosnog kopiranja podataka pravilnika o zadržavanju i vraćanje baze podataka, potražite u članku [Vraćanje iz snimke][].

## <a name="next-steps"></a>Daljnji koraci
Korištenje dobar dizajn baze podataka načela će vam tako biti lakše upravljanje baze podataka u SQL Data Warehouse. Da biste saznali više, lakši putem [Pregled razvoj][].

<!--Image references-->

<!--Article references-->
[Stvaranje SQL Data Warehouse (Portal za Azure)]: sql-data-warehouse-get-started-provision.md
[Stvaranje baze podataka (PowerShell)]: sql-data-warehouse-get-started-provision-powershell
[connection]: sql-data-warehouse-develop-connections.md
[Data Warehouse Azure SQL upita s Visual Studio]: sql-data-warehouse-query-visual-studio.md
[Povezivanje i upita s sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md
[Pregled razvoj]: sql-data-warehouse-overview-develop.md
[Praćenje svoje radno opterećenje pomoću DMVs]: sql-data-warehouse-manage-monitor.md
[Zaustavljanje računalnim]: sql-data-warehouse-manage-compute-overview.md#pause-compute-bk
[Vraćanje iz snimke]: sql-data-warehouse-restore-database-overview.md
[Životopis računalnim]: sql-data-warehouse-manage-compute-overview.md#resume-compute-performance-bk
[Promjena veličine performansi]: sql-data-warehouse-manage-compute-overview.md#scale-performance-bk
[Pregled sigurnosti]: sql-data-warehouse-overview-manage-security.md
[Najbolje prakse skladištu podataka SQL]: sql-data-warehouse-best-practices.md
[SQL Data Warehouse Sistemski prikazi]: sql-data-warehouse-reference-tsql-system-views.md

<!--MSDN references-->
[SQL Server Data Tools]: https://msdn.microsoft.com/library/mt204009.aspx

<!--Other web references-->
[Portal za Azure]: http://portal.azure.com/
