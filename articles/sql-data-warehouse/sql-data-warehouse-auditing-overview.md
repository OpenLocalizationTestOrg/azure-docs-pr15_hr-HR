<properties
   pageTitle="Nadzor u skladištu podataka Azure SQL | Microsoft Azure"
   description="Početak rada s nadzora u skladištu podataka za SQL Azure"
   services="sql-data-warehouse"
   documentationCenter=""
   authors="ronortloff"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.workload="data-management"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="09/24/2016" 
   ms.author="rortloff;barbkess;sonyama"/>

# <a name="auditing-in-azure-sql-data-warehouse"></a>Nadzor u skladištu podataka Azure SQL

> [AZURE.SELECTOR]
- [Nadzor](sql-data-warehouse-auditing-overview.md)
- [Otkrivanje prijetnju](sql-data-warehouse-security-threat-detection.md)

Nadzor SQL Data Warehouse omogućuje zapis zapisnik događaja u bazi podataka za reviziju na vašem računu Azure prostora za pohranu. Nadzor olakšava održavanje usklađenosti s propisima, razumijevanje aktivnosti baze podataka i dobiti uvid u nepodudaranja i anomalies koji ukazuje opasnosti tvrtke ili sumnjivu kršenja sigurnost. Nadzor SQL Data Warehouse i integrira u Microsoft Power BI za naniže izvješćivanje i analiza.

Alata za nadzor omogućiti i olakšavaju adherence za standarde usklađenosti, ali ne jamči usklađenosti. Dodatne informacije o Azure programe s podrškom za standarde usklađenosti potražite u <a href="http://azure.microsoft.com/support/trust-center/compliance/" target="_blank">Centru za pouzdanost Azure</a>.

+ [Osnove baza podataka nadzora]
+ [Postavljanje nadzor za bazu podataka]
+ [Analiza zapisnike nadzora i izvješća]

##<a id="subheading-1"></a>Azure SQL podataka skladištu baze podataka nadzor osnove


Nadzor baze podataka SQL Data Warehouse omogućuje vam da:

- **Zadrži** popratnu odabranog događaja. Možete definirati kategorijama od akcija baze podataka za nadzor.
- **Izvješće** o aktivnosti baze podataka. Da biste započeli brzo aktivnosti i događaja izvješćivanja možete koristiti konfiguriranog izvješća i nadzorne ploče.
- **Analiza** izvješća. Možete pronaći sumnjiva događaje, nepoznatu aktivnosti i trendova.

Možete konfigurirati nadziranje sljedećih kategorija događaja:

**Običan SQL** i **s parametrima SQL** za koju se zapisnici prikupljene nadzora klasificirani kao  

- **Pristup podacima**
- **Promjena sheme (DDL)**
- **Promjena podataka (DML)**
- **Poslovni subjekti, uloge i dozvole (DCL)**
- **Pohranjena procedura**, **Prijava** i, **Upravljanje transakcije**.

Za svaku kategoriju događaj, nadzor ili **neuspješnom **neuspjeh** operacije** konfigurirane zasebno.

Dodatne pojedinosti o aktivnosti i događaja nadzirati potražite u članku <a href="http://go.microsoft.com/fwlink/?LinkId=506733" target="_blank">Referenca oblika zapisnika nadzora (Preuzimanje datoteke dokumenta)</a>.

Zapisnike nadzora pohranjuju se na vašem računu Azure prostora za pohranu. Možete odrediti razdoblje zadržavanja na zapisnika nadzora.

Nadzor pravila može se definirati za određenu bazu podataka ili kao zadani pravilnik poslužitelja. Zadani nadzora Pravilnik za poslužitelj će se primijeniti na sve baze podataka na poslužitelju koji imaju na određene overriding baze podataka nadzora definirana pravila.

Prije nego što postavka gore nadzor nadzora potvrdite ako koristite ["hijerarhijski niži klijent"](sql-data-warehouse-auditing-downlevel-clients.md).


##<a id="subheading-2"></a>Postavljanje nadzor za bazu podataka

1. Pokrenite <a href="https://portal.azure.com" target="_blank">Azure Portal</a>.

2. Dođite do plohu konfiguracija baze podataka SQL Data Warehouse / SQL Server koje želite nadzirati. Kliknite gumb **Postavke** na vrhu, a potom u plohu postavke pa odaberite **nadzor**.

    ![][1]

3. U nadzora plohu konfiguracije poništiti potvrdni okvir **Nasljeđuju postavke nadzora s poslužitelja** . Omogućuje određivanje postavki za određenu bazu podataka.

    ![][2]

4. Nakon toga omogućite nadzor klikom na gumb **na** .

    ![][3]

5. U nadzora plohu konfiguracije odaberite **DETALJE za POHRANU** da biste otvorili plohu prostora za pohranu za zapisnike nadzora. Odaberite račun za Azure prostora za pohranu spremanje zapisnika i, razdoblje zadržavanja. **Savjet:** Koristiti isti prostor za pohranu račun za sve nadziru baze podataka da biste izvukli što više iz predložaka konfiguriranog izvješća.

    ![][4]

6. Kliknite gumb **u redu** da biste spremili Detalji o konfiguraciji prostora za pohranu.


7. U odjeljku **ZAPISIVANJE tako da DOGAĐAJU**, kliknite **neuspješnom **SLANJU** da biste se prijavili Svi događaji** ili odaberite pojedinačni događaj kategorije.


8. Nadzor konfigurirate za bazu podataka, možda ćete morati alter klijent da bi ispravno zabilježene nadzor podatke u nizu za povezivanje. Provjerite tema [Izmjena FDQN poslužitelja u nizu za povezivanje](sql-data-warehouse-auditing-downlevel-clients.md) za hijerarhijski niži klijentske veze.

9. Kliknite **u redu**.


##<a id="subheading-3">Analiza zapisnike nadzora i izvješća</a>

Zapisnike nadzora se pridružuje u skup spremište tablica s prefiksom **SQLDBAuditLogs** u račun za Azure prostora za pohranu koje ste odabrali prilikom instalacije. Možete pogledati pomoću alata kao što su <a href="http://azurestorageexplorer.codeplex.com/" target="_blank">Explorer Azure prostora za pohranu</a>datoteka zapisnika.

Predložak izvješća konfiguriranog nadzorne ploče nije dostupan kao <a href="http://go.microsoft.com/fwlink/?LinkId=403540" target="_blank">koje je moguće preuzeti proračunsku tablicu programa Excel</a> za brzu analizu podaci iz zapisnika. Da biste koristili predložak nadzornih zapisnika, potrebno je Excel 2013 ili noviji i Power Query, koji možete preuzeti <a href="http://www.microsoft.com/download/details.aspx?id=39379">ovdje</a>.

Predložak sadrži fictional ogledne podatke u njemu, a Power Query možete postaviti da biste uvezli u zapisniku nadzora izravno s vašeg računa Azure prostora za pohranu.

Detaljnije upute o radu s predložak izvješća Saznajte u <a href="http://go.microsoft.com/fwlink/?LinkId=506731">Način za (preuzimanje dokumenta)</a>.

![][5]


##<a id="subheading-4">Savjeti za korištenje u proizvodnje</a>
Opis u ovom odjeljku odnosi se na snimki zaslona iznad. <a href="https://portal.azure.com" target="_blank">Portal za Azure</a> ili <a href= "https://manage.windowsazure.com/" target="_bank">Klasični Azure klasični Portal</a> može se koristiti.


##<a id="subheading-5"></a>Prostor za pohranu ključa Regeneration

U radni se vjerojatno da povremeno osvježite ključeva za pohranu. Kada osvježite ključeva ćete morati ponovno spremite pravila. Postupak je na sljedeći način:.


1. Prebacivanje **Tipkovni prečac za pohranu** iz *primarnog* *sekundarne* i **SPREMITE**u nadzora plohu konfiguracije (prethodno opisan u članku nadzor odjeljak Postavljanje).
![][4]
2. Idite na plohu konfiguracije prostora za pohranu i **Obnovi** *Primarni ključ za Access*.

3. Prijeđite natrag za nadzor plohu konfiguracije prijeđite **Tipkovni prečac za pohranu** iz *sekundarne* *primarni* , a zatim pritisnite **SPREMI**.

4. Vratite se u prostor za pohranu korisničkog Sučelja i **Obnovi** *Sekundarne tipkovni prečac* (kao što je Priprema za idućeg kruga osvježavanja tipke.

##<a id="subheading-6"></a>Automatizacija
Postoji nekoliko cmdleta ljuske PowerShell možete koristiti za konfiguriranje nadzora u bazi podataka SQL Azure. Da biste pristupili cmdleti za nadzor koje morate imati PowerShell u načinu rada za Azure Voditelj resursa.

> [AZURE.NOTE] Modul [Azure Voditelj resursa](https://msdn.microsoft.com/library/dn654592.aspx) trenutno u pretpregledu. Možda neće imati iste mogućnosti upravljanja kao modul Azure.

Kada ste u načinu rada za upravljanje resursima Azure, pokrenite `Get-Command *AzureSql*` da biste dobili popis dostupnih cmdleta.


<!--Anchors-->
[Osnove baza podataka nadzora]: #subheading-1
[Postavljanje nadzor za bazu podataka]: #subheading-2
[Analiza zapisnike nadzora i izvješća]: #subheading-3


<!--Image references-->
[1]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing.png
[2]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-inherit.png
[3]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-enable.png
[4]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-storage-account.png
[5]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-dashboard.png


<!--Link references-->
