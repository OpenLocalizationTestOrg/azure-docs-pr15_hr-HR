<properties
    pageTitle="U memoriji OLTP poboljšava mjerača SQL txn performansi | Microsoft Azure"
    description="Da biste poboljšali performanse transakcijskih u postojećoj bazi podataka SQL adopt OLTP u memoriji."
    services="sql-database"
    documentationCenter=""
    authors="jodebrui"
    manager="jhubbard"
    editor="MightyPen"/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="jodebrui"/>


# <a name="use-in-memory-oltp-preview-to-improve-your-application-performance-in-sql-database"></a>Korištenje memorije OLTP (pretpregled) da biste poboljšali performanse računala u SQL baze podataka

Da biste poboljšali performanse radno opterećenje OLTP u bazama podataka za SQL Azure [Premium](sql-database-service-tiers.md) bez povećati razinu performanse mogu se [OLTP u memoriji](sql-database-in-memory.md) .

Slijedite ove korake da biste prihvaćaju u memoriji OLTP u postojeću bazu podataka.

## <a name="step-1-ensure-your-premium-database-supports-in-memory-oltp"></a>Korak 1: Premium baze podataka provjerite podržava li OLTP u memoriji

Baza podataka Premium stvara u studenom 2015 ili noviji podržava značajku u memoriji. Provjerili možete li bazu podataka Premium podržava značajku u memoriji tako da pokrenete sljedeći Transact-SQL naredbe. U memoriji podržana ako vraćeni rezultat je 1 (ne 0):

```
SELECT DatabasePropertyEx(Db_Name(), 'IsXTPSupported');
```

*XTP* skraćenica za *Ekstremne obradu transakcije*

Ako se s postojećom bazom podataka mora se premjestiti u novu bazu podataka V12 Premium, možete koristiti sljedeće tehnike za izvoz i uvoz podataka.

#### <a name="export-steps"></a>Upute za izvoz

Izvoz bazu podataka radnog na bacpac tako da koristi nešto od sljedećeg:

- Funkcija [Izvoz](sql-database-export.md) na [portal](https://portal.azure.com/).

- Funkcija **izvoza podataka sloja aplikacije** u programa [ažuran SSMS.exe](http://msdn.microsoft.com/library/mt238290.aspx) (SQL Server Management Studio).
 1. U **Programu Explorer objekt**Proširite čvor **baze podataka** .
 2. Desnom tipkom miša kliknite čvor svoje baze podataka.
 3. Kliknite **Zadaci** > **Izvoz podataka sloja aplikacije**.
 4. Rad u prozor čarobnjaka koji se prikazuje.


#### <a name="import-steps"></a>Upute za uvoz

Uvoz u bacpac u novu bazu podataka Premium.

1. Azure [portal](https://portal.azure.com/)
 - Idite na poslužitelj.
 - Odaberite mogućnost [Uvoz baze podataka](sql-database-import.md) .
 - Odaberite Premium cijene sloju.

2. Korištenje SSMS da biste uvezli u bacpac:
 - U **Programu Explorer objekt**desnom tipkom miša kliknite čvor **baze podataka** .
 - Kliknite **Uvoz podataka sloja aplikacije**.
 - Rad u prozor čarobnjaka koji se prikazuje.


## <a name="step-2-identify-objects-to-migrate-to-in-memory-oltp"></a>Korak 2: Prepoznavanje objekte Migriraj OLTP u memoriji

SSMS sadrži **Pregled za analizu performansi transakcije** izvješće koje možete pokrenuti protiv baze podataka pomoću aktivni radno opterećenje. Izvješće služi za identifikaciju tablica i pohranjenim procedurama koje su ikone za migraciju da biste u memoriji OLTP.

U SSMS da biste generirali izvješće alata:
- U **Programu Explorer objekt**desnom tipkom miša kliknite čvor svoje baze podataka.
- Kliknite **izvješća o** > **Standardna izvješća** > **Pregled za analizu performansi transakcije**.

Dodatne informacije potražite u članku [Determining ako tablica ili pohranjene Procedure mora biti podrazumijeva OLTP u memoriji](http://msdn.microsoft.com/library/dn205133.aspx).


## <a name="step-3-create-a-comparable-test-database"></a>Korak 3: Stvaranje usporediti testiranje baze podataka

Pretpostavimo da izvješće prikazuje baza podataka sadrži tablicu koja će im se pretvoriti u tablicu memorija optimizirana. Preporučujemo da prvo testirati da biste potvrdili na oznaka tako da testirate.

Potreban vam je test kopiju baze podataka proizvodnje. Testiranje baze podataka mora biti na istoj razini sloju servis kao radnog baze podataka.

Da biste olakšali testiranja, dotjerati testiranje baze podataka na sljedeći način:

1. Povezivanje s bazom podataka za testiranje pomoću SSMS.

2. Da biste izbjegli potrebe mogućnost s (snimka) u upitima, postavite mogućnost baze podataka kao što je prikazano u sljedećim T SQL naredbe:
```
ALTER DATABASE CURRENT
    SET
        MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT = ON;
```


## <a name="step-4-migrate-tables"></a>Korak 4: Migriranje tablice

Morate stvoriti i popunjavanje memorija optimizirana kopija tablice koje želite testirati. Možete je stvoriti tako da koristi nešto od sljedećeg:

- Pri ruci memorije optimizaciju čarobnjaka u SSMS.
- Ručno T-SQL.


#### <a name="memory-optimization-wizard-in-ssms"></a>Čarobnjak za optimizaciju memorije u SSMS

Da biste koristili tu mogućnost migracije:

1. Povezivanje baza podataka s SSMS.

2. U **Programu Explorer objekt**desnom tipkom miša kliknite tablicu, a zatim **Savjetnik za optimizaciju memorije**.
 - Prikazat će se čarobnjak za **Alat za optimizaciju Savjetnik za tablicu memorije** .

3. U čarobnjaku kliknite **Provjera valjanosti migracije** (ili gumb **Dalje** ) da biste vidjeli ako tablica ima nepodržane značajke koje nisu podržana u memorija optimizirana tablice. Dodatne informacije potražite u članku:
 - *Kontrolni popis za optimizaciju memorije* u [Savjetnik za optimizaciju memorije](http://msdn.microsoft.com/library/dn284308.aspx).
 - [SQL transakcija konstrukta ne podržava OLTP u memoriji](http://msdn.microsoft.com/library/dn246937.aspx).
 - [Migracija na OLTP u memoriji](http://msdn.microsoft.com/library/dn247639.aspx).

4. Ako tablica ima nema značajke koje nisu podržane, program advisor možete izvršiti stvarni sheme i migracije podataka umjesto vas.


#### <a name="manual-t-sql"></a>Ručno T-SQL

Da biste koristili tu mogućnost migracije:

1. Povezivanje s testiranje baze podataka pomoću SSMS (ili sličan utility).

2. Nabavite dovršeno T SQL skripte za tablice i njegove indekse.
 - U SSMS, desnom tipkom miša kliknite čvor vaše tablice.
 - Kliknite **skripta tablice u obliku** > **Stvori da biste** > **novi prozor upit**.

3. U prozoru skripte dodajte WITH (MEMORY_OPTIMIZED = Uključeno) iskaz CREATE TABLE.

4. Ako postoji GRUPIRANI indeks, promijenite NONCLUSTERED.

5. Da biste preimenovali postojeću tablicu pomoću SP_RENAME.

6. Stvorite novu kopiju memorija optimizirana tablice tako da pokrenete uređeni skriptu CREATE TABLE.

7. Kopirajte podatke u tablici memorija optimizirana pomoću Umetanje... ODABERITE * U:
    
```
INSERT INTO <new_memory_optimized_table>
        SELECT * FROM <old_disk_based_table>;
```


## <a name="step-5-optional-migrate-stored-procedures"></a>Korak (nije obavezno) 5: migriranje pohranjene procedure

Značajka u memoriji možete izmijeniti pohranjene procedure za bolje performanse.


### <a name="considerations-with-natively-compiled-stored-procedures"></a>Razmatranja s nativno kompilirane pohranjene procedure

Nativno kompilirane pohranjena procedura mora imati sljedeće mogućnosti na njegov T SQL s uvjet WHERE:

- NATIVE_COMPILATION

- SCHEMABINDING: tablica značenje pohranjena procedura ne mogu imati definicije stupca promijeniti na način koji utječe na pohranjena procedura, osim ako se ispusti pohranjene procedure.


Izvorni modul morate koristiti jedan veliki [ATOMSKE blokova](http://msdn.microsoft.com/library/dn452281.aspx) za upravljanje transakcije. Postoji nema uloge za izričite TRANSAKCIJE POČETKA ili VRAĆANJE TRANSAKCIJE. Ako kod otkrije krši pravilo tvrtke, možete prekinuti atomske blok s Izjava o [VRATITI](http://msdn.microsoft.com/library/ee677615.aspx) .


### <a name="typical-create-procedure-for-natively-compiled"></a>Uobičajeni CREATE PROCEDURE za nativno prevedena

Obično T-SQL da biste stvorili nativno kompilirane pohranjena procedura slično je sljedeći predložak:

```
CREATE PROCEDURE schemaname.procedurename
    @param1 type1, …
    WITH NATIVE_COMPILATION, SCHEMABINDING
    AS
        BEGIN ATOMIC WITH
            (TRANSACTION ISOLATION LEVEL = SNAPSHOT,
            LANGUAGE = N'your_language__see_sys.languages'
            )
        …
        END;
```

- Za TRANSACTION_ISOLATION_LEVEL, snimka je najčešću vrijednost za nativno kompilirane pohranjena procedura. Međutim, podskup druge vrijednosti i podržani su:
 - ČITANJE KONTROLIRANI
 - SERIALIZABLE


- JEZIK vrijednosti mora postojati u prikazu sys.languages.


### <a name="how-to-migrate-a-stored-procedure"></a>Kako migrirati pohranjena procedura

Nekoliko koraka migracije:


1. Nabavite skripta CREATE PROCEDURE za obične interpreted pohranjena procedura.

2. Dopune njegovo zaglavlje tako da odgovara predlošku prethodne.

3. Provjerili koristi li sve značajke koje nisu podržane u nativno kompilirane pohranjene procedure pohranjena procedura T SQL kod. Ako je potrebno, implementirati zaobilazna rješenja.
 - Detalje potražite u članku [Problemi premještanja za nativno prevedena spremljene procedure](http://msdn.microsoft.com/library/dn296678.aspx).

4. Preimenujte stari pohranjena procedura pomoću SP_RENAME. Ili jednostavno je ISPUSTITE.

5. Pokrenite uređivati skripte stvorili T-SQL postupak.


## <a name="step-6-run-your-workload-in-test"></a>Korak 6: Pokretanje svoje radno opterećenje u testu

Pokrenite radno opterećenje test bazu podataka koja je slična radno opterećenje koji se izvodi u bazi podataka radnog. To treba otkrivanje rast performanse postići pomoću značajke u memoriji za tablice i pohranjenim procedurama.

Glavna atribute povećavaju su:

- Broj Istodobni veze.

- Omjer za čitanje/pisanje.


Da biste prilagodili i pokrenite test posla, razmislite o korištenju alata pri ruci ostress.exe, a što je prikazano [ovdje](sql-database-in-memory.md).


Da biste minimizirali latenciju mreže, pokrenite testiranju u istom Azure zemljopisnom području tamo gdje postoje baze podataka.


## <a name="step-7-post-implementation-monitoring"></a>Korak 7: Nadzor nakon implementacije

Imajte na umu praćenje performansi Učinci implementacije za u memoriji u radni:

- [Prostor za pohranu u memoriji za monitor](sql-database-in-memory-oltp-monitoring.md).

- [Nadzor baze podataka SQL Azure pomoću dinamičke Upravljanje prikazima](sql-database-monitoring-with-dmvs.md)


## <a name="related-links"></a>Srodne veze

- [OLTP (u memoriji optimizaciju) u memoriji](http://msdn.microsoft.com/library/dn133186.aspx)

- [Uvod u nativno kompilirane pohranjene procedure](http://msdn.microsoft.com/library/dn133184.aspx)

- [Savjetnik za optimizaciju memorije](http://msdn.microsoft.com/library/dn284308.aspx)

