<properties
    pageTitle="Prošireni događaje u bazi podataka SQL | Microsoft Azure"
    description="U članku se opisuje prošireni događaja (XEvents) u bazi podataka SQL Azure i kako event sesija malo razlikovati od događaj sesije u Microsoft SQL Server."
    services="sql-database"
    documentationCenter=""
    authors="MightyPen"
    manager="jhubbard"
    editor=""
    tags=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="genemi"/>


# <a name="extended-events-in-sql-database"></a>Prošireni događaja u SQL baze podataka

[AZURE.INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

U ovoj se temi objašnjava kako se nešto drugačija implementacije prošireni događaje u bazi podataka SQL Azure u usporedbi s proširenom događaja u Microsoft SQL Server.


- V12 baze podataka SQL stekli značajku prošireni događaje u drugi polovicu kalendara 2015.
- SQL Server je prodao prošireni događaje od 2008.
- Skup značajki prošireni događaja na baze podataka SQL je robusne podskup značajki na SQL Server.


*XEvents* je Neformalna nadimaka koji ponekad se koristi za 'prošireni događaje' u blogove i ostale Neformalna mjesta.


> [AZURE.NOTE] Od 2015 listopad je proširena događaj sesiju značajka aktivirana u bazi podataka SQL Azure na razini pretpregled. Datum opće dostupnih (GA) još nije postavljen.
>
> Azure [Servisnih ažuriranja](https://azure.microsoft.com/updates/?service=sql-database) stranica sadrži objave koje GA najave.


Dodatne informacije o prošireni događaje za baze podataka SQL Azure i Microsoft SQL Server dostupna je na:

- [Brzi početak rada: Prošireni događaja u sustavu SQL Server](http://msdn.microsoft.com/library/mt733217.aspx)
- [Prošireni događaja](http://msdn.microsoft.com/library/bb630282.aspx)


## <a name="prerequisites"></a>Preduvjeti


U ovoj se temi pretpostavlja da ste već neke poznavanje:


- [Servis baze podataka SQL azure](https://azure.microsoft.com/services/sql-database/).


- [Prošireno događaje](http://msdn.microsoft.com/library/bb630282.aspx) u Microsoft SQL Server.
 - Skupno naš dokumentaciju o događajima prošireni primjenjuje se na SQL Server i SQL baze podataka.


Prilikom odabira događaj datoteke kao [cilj](#AzureXEventsTargets), korisno je prethodnog izlaganje sljedeće stavke:


- [Azure servis za pohranu](https://azure.microsoft.com/services/storage/)


- PowerShell
 - [Pomoću ljuske Azure s Azure pohranom](../storage/storage-powershell-guide-full.md) – pruža opsežne informacije o PowerShell i servis za pohranu Azure.


## <a name="code-samples"></a>Primjere koda


Povezane teme sadrže dva uzorka kod:


- [Pozivanje međuspremnik cilj kod za prošireni događaje u SQL baze podataka](sql-database-xevent-code-ring-buffer.md)
 - Kratki jednostavnu skriptu Transact-SQL.
 - Ne možemo naglasiti u temi uzorka kod, kada ste gotovi s cilj Nazovi međuspremnika, trebali biste prekinete njegovih resursa izvršavanjem alter ispuštanja `ALTER EVENT SESSION ... ON DATABASE DROP TARGET ...;` izjava. Naknadno možete dodati drugu instancu programa Nazovi međuspremnik tako da `ALTER EVENT SESSION ... ON DATABASE ADD TARGET ...`.


- [Kod ciljnu datoteku događaja za prošireni događaje u SQL baze podataka](sql-database-xevent-code-event-file.md)
 - Faza 1 je ljuske PowerShell za stvaranje kontejnera za Azure prostora za pohranu.
 - Faza 2 je Transact-SQL koji koristi spremnik Azure prostora za pohranu.


## <a name="transact-sql-differences"></a>Razlike u SQL transakcija


- Kad se izvršiti naredbu za [Stvaranje EVENT SESIJA](http://msdn.microsoft.com/library/bb677289.aspx) na SQL Server, koristite uvjet **Na POSLUŽITELJU** . No na baze podataka SQL uvjet **Uključeno baze podataka** umjesto toga.


- Uvjet **Baze podataka na** odnosi i na [DOGAĐAJ ALTER SESIJE](http://msdn.microsoft.com/library/bb630368.aspx) i [ISPUSTITE EVENT SESIJA](http://msdn.microsoft.com/library/bb630257.aspx) Transact-SQL naredbe.


- Najbolja praksa je da biste uključili event sesija mogućnost **STARTUP_STATE = Uključeno** u svojih izjava **STVORITI DOGAĐAJ SESIJU** ili **ALTER DOGAĐAJ SESIJU** .
 - Vrijednost **= Dalje** podržava automatskog ponovnog pokretanja nakon ponovno konfiguriranje logičke baze podataka jer je prebacivanje.


## <a name="new-catalog-views"></a>Nove prikaze kataloga


Značajka prošireni događaje podržava nekoliko [prikaza kataloga](http://msdn.microsoft.com/library/ms174365.aspx). Prikazi kataloga vas obavijestiti o *metapodataka ili definicije* sesija stvorili kao korisnik događaja u trenutnoj bazi podataka. Prikazi vratiti informacije o pojavljivanja sesije aktivni događaj.


| Naziv<br/>Prikaz kataloga | Opis |
| :-- | :-- |
| **sys.database_event_session_actions** | Vraća redak za svaku akciju na svaki događaj sesiju događaj. |
| **sys.database_event_session_events** | Vraća retku za svaki događaj u sesiju događaj. |
| **sys.database_event_session_fields** | Vraća retku za svaki stupac Prilagodba-mogućnost koja eksplicitno postavljene na događaja i ciljnih web-mjesta. |
| **sys.database_event_session_targets** | Vraća retku za svaki događaj cilj za sesiju događaj. |
| **sys.database_event_sessions** | Vraća u retku za svaki događaj sesiju u bazi podataka za SQL baze podataka. |


U sustavu Microsoft SQL Server slične kataloga prikazi su nazivi koji sadrže *.server\_ * umjesto *.database\_*. Uzorak naziv je kao što je **sys.server_event_%**.


## <a name="new-dynamic-management-views-dmvshttpmsdnmicrosoftcomlibraryms188754aspx"></a>Nove dinamičkog Upravljanje prikazima [(DMVs)](http://msdn.microsoft.com/library/ms188754.aspx)


Baze podataka SQL Azure ima [dinamički Upravljanje prikazima (DMVs)](http://msdn.microsoft.com/library/bb677293.aspx) s podrškom za prošireni događaja. DMVs vas obavijestiti o *aktivnih* sesija događaj.


| Naziv DMV | Opis |
| :-- | :-- |
| **sys.dm_xe_database_session_event_actions** | Vraća informacije o event sesija akcije. |
| **sys.dm_xe_database_session_events** | Vraća informacije o događajima sesiju. |
| **sys.dm_xe_database_session_object_columns** | Prikazuje konfiguracijskih vrijednosti za objekte koji su povezani sa sesijom. |
| **sys.dm_xe_database_session_targets** | Vraća informacije o sesiji ciljnih web-mjesta. |
| **sys.dm_xe_database_sessions** | Vraća redak za svaku sesiju događaja koji je usmjeren na trenutnu bazu podataka. |


U sustavu Microsoft SQL Server imenovane su slične kataloga prikaza bez u * \_baze podataka* dio naziva, kao što su:


- **sys.dm_xe_sessions**, umjesto naziva<br/>**sys.dm_xe_database_sessions**.


### <a name="dmvs-common-to-both"></a>DMVs zajedničke oboje


Za prošireni događaje postoje dodatni DMVs koje su zajedničke baze podataka SQL Azure i Microsoft SQL Server:


- **sys.dm_xe_map_values**
- **sys.dm_xe_object_columns**
- **sys.dm_xe_objects**
- **sys.dm_xe_packages**



 <a name="sqlfindseventsactionstargets" id="sqlfindseventsactionstargets"></a>

## <a name="find-the-available-extended-events-actions-and-targets"></a>Pronalaženje dostupni dodatni Detalji o događajima, akcije i ciljnih web-mjesta


Možete pokrenuti s jednostavne SQL **Odaberite** da biste dobili popis dostupnih događaja, akcije i cilj.


```
SELECT
        o.object_type,
        p.name         AS [package_name],
        o.name         AS [db_object_name],
        o.description  AS [db_obj_description]
    FROM
                   sys.dm_xe_objects  AS o
        INNER JOIN sys.dm_xe_packages AS p  ON p.guid = o.package_guid
    WHERE
        o.object_type in
            (
            'action',  'event',  'target'
            )
    ORDER BY
        o.object_type,
        p.name,
        o.name;
```



<a name="AzureXEventsTargets" id="AzureXEventsTargets"></a>

&nbsp;

## <a name="targets-for-your-sql-database-event-sessions"></a>Ciljevi za vaše sesije događaj SQL baze podataka


Evo ciljnih web-mjesta koja možete snimiti rezultate iz vaše sesija događaja u SQL baze podataka:


- [Cilj Nazovi međuspremnik](http://msdn.microsoft.com/library/ff878182.aspx) - ukratko sadrži podatke za događaj u memoriji.
- [Cilj događaj brojač](http://msdn.microsoft.com/library/ff878025.aspx) - broji svih događaja koji se pojavljuju tijekom sesiju prošireni događaja.
- [Ciljnu datoteku događaj](http://msdn.microsoft.com/library/ff878115.aspx) – zapisivanja dovršeno međuspremnika u spremniku Azure prostora za pohranu.


[Događaj praćenje za sustav Windows (ETW)](http://msdn.microsoft.com/library/ms751538.aspx) API nije dostupna za prošireni događaje u SQL baze podataka.


## <a name="restrictions"></a>Ograničenja


Postoji nekoliko vezane uz sigurnost razlike befitting okruženje oblaka za SQL baze podataka:


- Prošireni događaji su founded na model odvajanja jednog klijenta. Sesiju događaja u jednoj bazi podataka ne možete pristupiti podataka ili događaje iz druge baze podataka.

- **Stvaranje događaja SESIJU** izjava u kontekstu **glavnom** bazom podataka nije moguće problema.


## <a name="permission-model"></a>Model dozvola


Morate imati dozvolu za **kontrolu** baze podataka na problema **Stvaranje EVENT SESIJA** izjava. Vlasnik baze podataka (vlasnika baze podataka) s dozvolom za **kontrolu** .


### <a name="storage-container-authorizations"></a>Autorizacijama spremnik za pohranu


Token SAS generiranje za vaše spremnik Azure prostora za pohranu potrebno je navesti **rwl** za dozvole. Vrijednost **rwl** nudi sljedeće dozvole:


- Čitanje
- Pisanje
- Popis


## <a name="performance-considerations"></a>Pitanja vezana uz performanse


Postoje scenariji kojima intenzivno koristi prošireni događaja možete skupiti više aktivni memorije nego što je dobar cjelokupnog sustava. Stoga baze podataka SQL Azure sustava dinamički postavlja i prilagođava ograničenja aktivni memorija koji mogu biti akumulirani po sesiju događaj. Mnogo je čimbenika idu u dinamičku izračuna.


Ako primite poruku o pogrešci koja vas obavještava da je maksimalna memorije provedeno, su neke korektivne akcije koje možete poduzeti:


- Pokretanje sesije manje Istodobni događaj.


- Pomoću naredbe za sesije događaja na **Stvaranje** i **Izmjena** smanjite količinu memorije navedete na na **MAX\_MEMORIJE** uvjet.


### <a name="network-latency"></a>Latenciju mreže


Cilj **Događaj datoteke** se mogu pojaviti latenciju mreže ili pogreške pri održavanju podataka za pohranu Azure blob-ova. Druge događaje u bazi podataka SQL se može odgoditi dok za komunikaciju mreže da biste dovršili. Odgode može usporiti svoje radno opterećenje.

- Da biste smanjili taj rizik performanse, nemojte postavite mogućnost **EVENT_RETENTION_MODE** na **NO_EVENT_LOSS** u događaj definicije sesiju.


## <a name="related-links"></a>Srodne veze


- [Pomoću Azure komponente PowerShell s Azure prostora za pohranu](../storage/storage-powershell-guide-full.md).
- [Cmdleti za Azure prostora za pohranu](http://msdn.microsoft.com/library/dn806401.aspx)


- [Pomoću ljuske Azure s Azure pohranom](../storage/storage-powershell-guide-full.md) – pruža opsežne informacije o PowerShell i servis za pohranu Azure.
- [Upute za korištenje spremišta blobova iz .NET](../storage/storage-dotnet-how-to-use-blobs.md)


- [Stvaranje VJERODAJNICA (-SQL transakcija)](http://msdn.microsoft.com/library/ms189522.aspx)
- [Stvaranje događaja SESIJU (-SQL transakcija)](http://msdn.microsoft.com/library/bb677289.aspx)


- [Eksplicitno Kehayias bloga o prošireni događaja u Microsoft SQL Server](http://www.sqlskills.com/blogs/jonathan/category/extended-events/)


Druge teme uzorka kod za prošireni događaje dostupne su na sljedećim vezama. Međutim, morate često provjerite sve uzorka da biste vidjeli je li uzorak pronalaze Microsoft SQL Server nasuprot baze podataka SQL Azure. Zatim možete odlučiti hoće li se manje promjene potrebne za pokretanje uzorka.


<!--
('lock_acquired' event.)

- Code sample for SQL Server: [Determine Which Queries Are Holding Locks](http://msdn.microsoft.com/library/bb677357.aspx)
- Code sample for SQL Server: [Find the Objects That Have the Most Locks Taken on Them](http://msdn.microsoft.com/library/bb630355.aspx)
-->
