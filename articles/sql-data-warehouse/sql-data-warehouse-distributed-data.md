<properties
   pageTitle="Raspodijeljenih podataka i distribuirati mogućnosti tablice za sustave Massively paralelno Processing (MPP) SQL Data Warehouse i Parallel Data Warehouse | Microsoft Azure"
   description="Saznajte kako se podaci raspoređeni Massively paralelno Processing (MPP) i mogućnosti za isporuku tablica u Azure SQL Data Warehouse i Parallel Data Warehouse."
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
   ms.date="10/10/2016"
   ms.author="barbkess"/>


# <a name="distributed-data-and-distributed-tables-for-massively-parallel-processing-mpp"></a>Raspodijeljeno podataka i raspodijeljeno tablice za Massively paralelno Processing (MPP)

Saznajte kako se korisničkih podataka distribuira u Azure SQL Data Warehouse i Parallel Data Warehouse koje su sustavi tvrtke Microsoft Massively paralelno Processing (MPP). Dizajniranje skladištu podataka za učinkovito korištenje raspodijeljeno podataka vam omogućuje da biste postigli prednosti MPP arhitektura za obradu upita. Nekoliko mogućnosti dizajna baze podataka može sadržavati značajan utjecaj na poboljšanje performansi upita.  

>[AZURE.NOTE] Azure SQL Data Warehouse i Parallel Data Warehouse koristiti isti dizajn Massively paralelno Processing (MPP), ali imaju nekoliko razlike zbog podlozi platforme. SQL Data Warehouse je kao Service (PaaS) koja se izvršava na Azure. Parallel Data Warehouse pokreće na analize platformu sustava (web-aplikacije Apps) koji je lokalni uređaj koji se izvodi na poslužitelju Windows.

## <a name="what-is-distributed-data"></a>Što je raspodijeljeno podataka?

U SQL Data Warehouse i Parallel Data Warehouse, raspodijeljeno podataka se odnosi na podatak korisnika koji je pohranjen na više mjesta u sustavu MPP. Svaki od tih mjesta funkcionira kao neovisno prostora za pohranu i obrada jedinica koja se izvršava upita na njegov dio podataka. Raspodijeljeno podaci osnova za pokretanje upita paralelno da biste postigli performanse visoke upita.

Distribuiranje podataka skladištu podataka dodjeljuje svaki redak tablice korisnika na jedno mjesto raspodijeljeno.  Tablice s raspršivanje raspodjele metodom ili web-kružnog možete distribuirati. Način distribucije naveden je u iskaz CREATE TABLE. 

## <a name="hash-distributed-tables"></a>Raspršivanje distributed tablice
  
Sljedeći dijagram prikazuje kako se pohranjuju potpuno (koji nisu distributed tablica) kao tablicu raspršivanje distribuirati. Deterministic funkcija dodjeljuje svaki redak pripadati jedan distribuciju. U definiciju tablice jedan od stupaca je označen kao stupac za raspodjelu. Raspršivanje funkcija koristi vrijednost u stupcu za raspodjelu dodijeliti svakom retku na popis za raspodjelu.

Postoje pitanja vezana uz performanse za odabir stupca za raspodjelu, kao što su distinctness, skew podataka i vrstama upita pokrenuli u sustavu.
  
![Raspodijeljeno tablice] (media/sql-data-warehouse-distributed-data/hash-distributed-table.png "Raspodijeljeno tablice")  
  
-   Svaki redak pripada jedan distribuciju.  
  
-   Raspršivanje deterministic algoritam dodjeljuje svaki redak jedan distribuciju.  
  
-   Broj redaka tablice po raspodjele mijenja se kao što je prikazano po različitim veličinama tablice.

## <a name="round-robin-distributed-tables"></a>Kružno raspodijeljeno tablice

Raspodijeljeno tablice kružnog distribuira retke dodjelom svaki redak na popis za raspodjelu u nizu način. Je brzi da biste učitali podatke u tablicu – kružnog, ali performanse upita može biti sporije.  Pridruživanje tablice kružnog – obično zahtijeva reshuffling retke koje želite omogućiti upit da bi proizveo točan rezultat koji traje.

## <a name="distributed-storage-locations-are-called-distributions"></a>Mjesta za pohranu raspodijeljeno nazivaju distribucije

Svakom raspodijeljeno mjestu zove distribucije. Nakon pokretanja upita paralelno svaki raspodjele izvodi SQL upita na njegov dio podataka. SQL Data Warehouse koristi SQL baze podataka da biste pokrenuli upit. Parallel Data Warehouse koristi SQL Server da biste pokrenuli upit. Ovaj dizajn zajednički nothing arhitektura je osnova postizanja skaliranje iz paralelne računalstvo.

### <a name="can-i-view-the-distributions"></a>Možete pogledati na distribucija?

Svaki raspodjele je ID za raspodjelu, a vidljiv u prikazima sustav koji se odnose na SQL Data Warehouse i Parallel Data Warehouse. ID raspodjele možete koristiti za otklanjanje poteškoća s performansama upita i drugih problema. Popis sistemskih prikaza, potražite u članku [Prikaz MPP sustava](sql-data-warehouse-reference-tsql-statements.md).

## <a name="difference-between-a-distribution-and-a-compute-node"></a>Razlika između raspodjele i računalnim čvor

Raspodjele je osnovna jedinica za pohranu raspodijeljeno podataka i obradu paralelne upita. Distribucija grupiraju se u računalnim čvorove. Svaki čvor računalnim prati jedan ili više distribucije.  

-   Analitički sustav u platformu koristi računalnim čvorove kao središnje komponentu hardverske arhitekturi i skaliranje iz mogućnosti. Uvijek koristi osam distribucija po čvor računalnim, kao što je prikazano u dijagramu za raspršivanje distributed tablice. Broj računalnim čvorove i stoga broj distribucija, određen broj računalnim čvorove nabavljate za na uređaj. Na primjer, ako kupite osam računalnim čvorove dobiti 64 distribucija (8 računalnim čvorove x 8 distribucija/čvora). 

-   SQL Data Warehouse ima fiksnim brojem 60 distribucija i prilagodljivo broj računalnim čvorove. Čvorovi računalnim primjenjuju se s resursima Azure računanja i prostora za pohranu. Broj računalnim čvorove možete promijeniti prema radno opterećenje za servis pozadinskog sustava i računalnih kapacitet (DWUs) odredite za skladištu podataka. Kada se broj računalnim čvorove promjene, broj distribucija po računalnim čvor i promjena. 

### <a name="can-i-view-the-compute-nodes"></a>Možete pogledati čvorove računalnim?

Svaki računalnim čvor ima čvor ID, a vidljiv u prikazima sustav koji se odnose na SQL Data Warehouse i Parallel Data Warehouse.  Vidjet ćete čvor računalnim traženjem stupca node_id u prikazima sustava čiji nazivi počinju sys.pdw_nodes. Popis sistemskih prikaza, potražite u članku [Prikaz MPP sustava](sql-data-warehouse-reference-tsql-statements.md).

## <a name="Replicated"></a>Replicirati tablice za Parallel Data Warehouse 
  
Odnosi se na: Napredne postavke preglednika Data Warehouse

Uz raspodijeljeno tablice Parallel Data Warehouse nudi mogućnost za replikaciju tablice. Tablicu koja je pohranjena u potpunosti na svakom računalnim čvor je *replicirati tablice* je. Replikaciju tablice uklanja potrebno za prijenos reci tablice među računalnim čvorove prije korištenja tablice u join ili zbrajanja. Repliciranu tablice izvedivo pomoću male tablice su samo zbog dodatnog prostora za pohranu potrebnih za spremanje cijelog tablice na svakom računalnim čvor.  
  
Sljedeći dijagram prikazuje repliciranu tablice koja je spremljena na svakom računalnim čvor. Repliciranu tablice pohranjuju se na sve diskova dodijeljene čvor računalnim. Ovaj strategije disk je implementiran putem filegroups SQL Server.  
  
![Replicated tablice] (media/sql-data-warehouse-distributed-data/replicated-table.png "Replicated tablice") 
  
## <a name="next-steps"></a>Daljnji koraci
  
Učinkovito korištenje raspodijeljeno tablica potražite u članku [Distributing tablica u SQL Data Warehouse](sql-data-warehouse-tables-distribute.md)  
  



  
