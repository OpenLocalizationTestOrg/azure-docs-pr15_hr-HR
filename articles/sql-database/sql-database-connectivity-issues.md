<properties
    pageTitle="Rješavanje pogreške SQL vezu, tranzitne pogreška | Microsoft Azure"
    description="Upute za otklanjanje poteškoća s dijagnosticiranje i spriječili pogreška SQL veze ili tranzitne pogrešaka u bazi podataka SQL Azure. "
    keywords="SQL vezom, niz za povezivanje, problema s povezivanjem, tranzitne pogrešku, pogreška veze"
    services="sql-database"
    documentationCenter=""
    authors="dalechen"
    manager="felixwu"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="daleche"/>


# <a name="troubleshoot-diagnose-and-prevent-sql-connection-errors-and-transient-errors-for-sql-database"></a>Otklanjanje poteškoća s, dijagnosticiranje i spriječili SQL vezu pogreške i tranzitne pogreške za baze podataka SQL

U ovom se članku opisuje kako spriječiti, otklanjanje poteškoća s, dijagnosticiranje i prevladavanje pogreške veze i tranzitne pogreške koja klijentskoj aplikaciji naiđe kada je stupi u interakciju s bazom podataka SQL Azure. Saznajte kako konfigurirati pokušaj logike, stvaranje niza za povezivanje i prilagodite druge postavke veze.

<a id="i-transient-faults" name="i-transient-faults"></a>

## <a name="transient-errors-transient-faults"></a>Tranzitne pogrešaka tranzitne

Tranzitne pogreške – i tranzitne kvara - ima podlozi uzrok koje uskoro riješiti sam. Povremeni uzroku tranzitne pogreške je kada Azure sustava brzo pomiče resursima hardvera bolje učitavanja saldo različitih radnih opterećenja. Većinu tih ponovno konfiguriranje događaja često dovršite manje od 60 sekundi. Tijekom ovog vremenskog razdoblja ponovno konfiguriranje možda imate problema s povezivanjem s bazom podataka SQL Azure. Aplikacije povezivanje s bazom podataka SQL Azure mora biti ugrađenih očekivati te tranzitne pogreške, njima rukovati implementacijom pokušaj logike u kodu, umjesto da ih pojavljivanje korisnicima kao pogreške aplikacije.

Ako klijentski program koristi ADO.NET, vaš program je uputio o pogrešci tranzitne varanje **SqlException**. Svojstvo **broj** se uspoređuje u popisu tranzitne pogrešaka pri vrhu teme: [šifre pogreške SQL baze podataka SQL klijentske aplikacije](sql-database-develop-error-messages.md).

<a id="connection-versus-command" name="connection-versus-command"></a>

### <a name="connection-versus-command"></a>Povezivanje i naredbe

Ćete Ponovi SQL vezom ili ponovno uspostaviti ovisno o sljedeće:

* **Tranzitne pogreška se pojavljuje prilikom pokušaja povezivanja**: veza koje se treba ponoviti nakon što nekoliko sekundi.

* **Tranzitne pogreška se pojavljuje tijekom odgovarajuću SQL naredbu za upit**: naredbu treba neće biti odmah ponoviti. Umjesto toga nakon stanke, u mora biti freshly uspostaviti vezu. Zatim naredbu možete pokušati.


<a id="j-retry-logic-transient-faults" name="j-retry-logic-transient-faults"></a>

### <a name="retry-logic-for-transient-errors"></a>Ponovnog pokušaja tranzitne pogrešaka


Klijentski programi koji se povremeno naići tranzitne pogreške su Robusniji kada sadrže logike pokušajte ponovno.


Kada program komunicira s bazom podataka SQL Azure do 3 proizvod proizvođača, inquire dobavljača hoće li se na proizvod sadrži logiku pokušaj tranzitne pogrešaka.

<a id="principles-for-retry" name="principles-for-retry"></a>

#### <a name="principles-for-retry"></a>Načela Ponovi


- Pokušaj otvaranja veze moraju se ponoviti ako je pogreška tranzitne.


- SQL naredbe SELECT koja ne uspijeva uz poruku o pogrešci za tranzitne treba ne izravno ponoviti.
 - Umjesto toga uspostavi Osvježi vezu i ponovno pokušajte odabir.


- Ako je naredba SQL UPDATE ne uspije uz tranzitne poruku o pogrešci, svježim trebali biste moguće uspostaviti vezu prije no što je ažuriranje ponoviti.
 - Logika pokušaj osigurati da dovršiti transakcije cijelu bazu podataka ili koji cijelu transakcije je vraćen.


#### <a name="other-considerations-for-retry"></a>Druge napomene za Ponovi


- Program za obradu koji se automatski pokreće nakon radno vrijeme, a koje će dovršiti prije Jutro, možete smijete vrlo strpljivi s dugo vremenske intervale između njegov pokušaja pokušajte ponovno.


- Trebali biste korisničkog sučelja programa računa za Ljudski srednju vrijednost da bi se dobilo nakon predugačak aktivnost čekanje.
 - Međutim, rješenje ne smije biti pokušati svakih nekoliko sekundi, jer je to pravilo možete flood sustavu zahtjeve.


#### <a name="interval-increase-between-retries"></a>Povećanje interval između ponovne pokušaje



Preporučujemo da odgađanje 5 sekundi prije nego što prvi pokušaj za ponovno. Ponovni nakon stanke kraće od 5 sekundi rizika zbunili servisa u oblaku. Za svaki sljedeći pokušaj odgode eksponencijalno, trebali biste Povećaj maksimalno 60 sekundi do.

Rasprave u *Blokiranje razdoblje* za klijente koji koriste ADO.NET dostupan je u [SQL Server veze red čekanja (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx).

Možda želite postaviti maksimalni broj ponovne pokušaje program koja se sama prekida.


#### <a name="code-samples-with-retry-logic"></a>Primjere koda s Ponovi


Primjere koda s pokušaj, u različitim programskog jezika, dostupne su na:

- [Biblioteka veza za SQL baze podataka i SQL Server](sql-database-libraries.md)


<a id="k-test-retry-logic" name="k-test-retry-logic"></a>

#### <a name="test-your-retry-logic"></a>Testiranje logiku Ponovi


Da biste testirali logiku pokušaj kao zamjenu ili uzrokovati pogrešku nego što se može ispraviti dok se izvodi programa.


##### <a name="test-by-disconnecting-from-the-network"></a>Testirajte prekid veze s mrežom


Možete testirati i logiku pokušaj način je da biste prekinuli klijentsko računalo s mrežom dok se izvodi program. Bit će pogrešku:

- **SqlException.Number** = 11001
- Poruka: "Nema takvog glavnog računala se nazivaju"


Prvi pokušaj pokušaj, dio programa možete ispravljanje pogrešno napisane riječi i pokušaj povezivanja.


Da bi sustav učinila praktično, prije nego što počnete programa isključite računalo s mrežom. Zatim program prepoznaje vrijeme izvođenja parametar koji uzrokuje program:

1. Privremeno dodati 11001 popisu pogrešaka smatrati prolaznim.
2. Kao i obično pokušati svoju prvu vezu.
3. Kada je otkrivena pogreška, uklonite 11001 s popisa.
4. Prikazuje se poruka koja upozorava korisnika priključiti računala u mreži.
 - Zadržite pokazivač dodatno izvođenja pomoću metode **Console.ReadLine** ili dijaloški okvir gumb u redu. Korisnik pritisne tipku Enter nakon računalom povezan s mrežom.
5. Pokušajte ponovno povezati, očekuje uspjeh.


##### <a name="test-by-misspelling-the-database-name-when-connecting"></a>Testirajte pogrešno upisani naziv baze podataka prilikom povezivanja


Program možete nastojanju pišete korisničko ime prije prvog pokušaja povezivanja. Bit će pogrešku:

- **SqlException.Number** = 18456
- Poruka: "Prijava za korisnika"WRONG_MyUserName"nije uspjela."


Prvi pokušaj pokušaj, dio programa možete ispravljanje pogrešno napisane riječi i pokušaj povezivanja.


Da bi sustav učinila praktično, vaš program nije prepoznati vrijeme izvođenja parametar koji uzrokuje program:

1. Privremeno dodajte 18456 popisu pogrešaka smatrati prolaznim.
2. Dodajte nastojanju 'WRONG_' korisničko ime.
3. Kada je otkrivena pogreška, uklonite 18456 s popisa.
4. Uklanjanje 'WRONG_' korisničko ime.
5. Pokušajte ponovno povezati, očekuje uspjeh.

<a id="net-sqlconnection-parameters-for-connection-retry" name="net-sqlconnection-parameters-for-connection-retry"></a>

### <a name="net-sqlconnection-parameters-for-connection-retry"></a>.NET SqlConnection parametara za pokušaj veze


Klijentski program povezuje s bazom podataka SQL Azure pomoću .NET Framework klase **System.Data.SqlClient.SqlConnection**, trebali biste koristiti .NET 4.6.1 ili noviji tako da omogućuje korištenje njegov značajka pokušaj veze. Pojedinosti o značajci [ovdje](http://go.microsoft.com/fwlink/?linkid=393996).


<!--
2015-11-30, FwLink 393996 points to dn632678.aspx, which links to a downloadable .docx related to SqlClient and SQL Server 2014.
-->


Prilikom stvaranja [niz za povezivanje](http://msdn.microsoft.com/library/System.Data.SqlClient.SqlConnection.connectionstring.aspx) za **SqlConnection** objekta, trebali biste koordinaciji vrijednosti jednu od sljedećih parametara:

- ConnectRetryCount &nbsp; &nbsp; *(Zadana vrijednost je 1. Raspon je 0 do 255)*
- ConnectRetryInterval &nbsp; &nbsp; *(Zadana vrijednost je 1 sekunde. Raspon je od 1 do 60)*
- Vremensko ograničenje veze &nbsp; &nbsp; *(Zadana vrijednost je 15 sekundi. Raspon je 0 do 2147483647)*


Konkretno, odabranom vrijednosti trebali napraviti sljedeće jednakosti true:

- Vremensko ograničenje veze = ConnectRetryCount * ConnectionRetryInterval

Ako, na primjer, ako je broj = 3 i interval = 10 sekundi vremensko ograničenje od samo 29 sekundi ne prilično dali sustav dovoljno vremena za njegov 3 i konačni pokušaj pri povezivanju: 29 < 3 * 10.

<a id="connection-versus-command" name="connection-versus-command"></a>

### <a name="connection-versus-command"></a>Povezivanje i naredbe


Parametri **ConnectRetryCount** i **ConnectRetryInterval** omogućuju objekta **SqlConnection** ponoviti postupak povezivanje bez koja upozorava ili bothering programa, kao što su povratka kontrole programa. Na ponovne pokušaje se može pojaviti u sljedećim situacijama:

- Pozivanje metode mySqlConnection.Open
- Pozivanje metode mySqlConnection.Execute

Postoji u subtlety. Ako dođe do pogreške u tranzitne dok je *upit* izvršena, objekt **SqlConnection** ponoviti postupak za povezivanje pa ga certainly ne ponovnog upita. Međutim, **SqlConnection** vrlo brzo provjerava vezu prije no što pošaljete upita za izvršavanje. Ako Brza provjera otkrije problema s vezom, **SqlConnection** ponovnih pokušaja operacije povezivanja. Ako se na pokušaj potvrdi, upit koji se šalje za izvršavanje.


#### <a name="should-connectretrycount-be-combined-with-application-retry-logic"></a>Trebali biste ConnectRetryCount se kombinirati s logike pokušaj aplikacije?

Pretpostavimo da aplikacije sadrži logiku robusne prilagođene pokušajte ponovno. Ga mogli ponoviti postupak povezivanje 4 vremena. Ako dodate **ConnectRetryInterval** i **ConnectRetryCount** = 3 u nizu za povezivanje, će se povećati pokušaj broja 4 * 3 = 12 ponovne pokušaje. Možda niste željeli kao što je velik broj ponovne pokušaje.

<a id="a-connection-connection-string" name="a-connection-connection-string"></a>

## <a name="connections-to-azure-sql-database"></a>Veza s bazom podataka Azure SQL

<a id="c-connection-string" name="c-connection-string"></a>

### <a name="connection-connection-string"></a>Veze: Niz za povezivanje


Potrebne za povezivanje s bazom podataka SQL Azure niz za povezivanje malo razlikuje se od niza za povezivanje s Microsoft SQL Server. Kopirajte niz za povezivanje za bazu podataka s [Portala za Azure](https://portal.azure.com/).


[AZURE.INCLUDE [sql-database-include-connection-string-20-portalshots](../../includes/sql-database-include-connection-string-20-portalshots.md)]


<a id="b-connection-ip-address" name="b-connection-ip-address"></a>

### <a name="connection-ip-address"></a>Veze: IP adresu


Morate konfigurirati baze podataka SQL server da biste prihvatili komunikacije s IP adrese računala na kojem je smještena klijentskom programu. To se tako da uredite postavke vatrozida putem [Portala za Azure](https://portal.azure.com/).


Ako zaboravite da biste konfigurirali IP adresu programa neće uspjeti s poruku pogreške pri ruci da potrebne IP adresa.


[AZURE.INCLUDE [sql-database-include-ip-address-22-v12portal](../../includes/sql-database-include-ip-address-22-v12portal.md)]


Dodatne informacije potražite u članku: [Kako: Konfiguriranje postavki vatrozida na baze podataka SQL](sql-database-configure-firewall-settings.md)


<a id="c-connection-ports" name="c-connection-ports"></a>

### <a name="connection-ports"></a>Veze: priključci


Obično samo morate da biste bili sigurni da 1433 otvoren priključak za izlazni komunikaciju, na računalu koje hostira klijentskom programu.


Ako, na primjer, kada klijentski program nalazi se na računalu sa sustavom Windows, vatrozid za Windows na glavnom računalu omogućuje da biste otvorili priključak 1433:


1. Otvorite upravljačku ploču
2. &gt;Sve stavke Upravljačka ploča
3. &gt;Vatrozid za Windows
4. &gt;Napredne postavke
5. &gt;Izlaznog pravila
6. &gt;Akcija
7. &gt;Novo pravilo


Ako klijentski program nalazi se na Azure virtualnog računala (VM), pročitajte:<br/>[Priključci izvan 1433 za ADO.NET 4,5 i V12 SQL baze podataka](sql-database-develop-direct-route-ports-adonet-v12.md).


Informacije o cofiguration priključaka i IP adrese, potražite u članku: [Vatrozid za baze podataka SQL Azure](sql-database-firewall-configure.md)


<a id="d-connection-ado-net-4-5" name="d-connection-ado-net-4-5"></a>

### <a name="connection-adonet-461"></a>Veza: ADO.NET 4.6.1


Ako vaš program koristi ADO.NET klase kao što su **System.Data.SqlClient.SqlConnection** da biste se povezali s bazom podataka SQL Azure, preporučujemo da koristite .NET Framework verzije 4.6.1 ili noviji.


ADO.NET 4.6.1:

- Baze podataka SQL Azure, postoji veća pouzdanost prilikom otvaranja veze pomoću metode amortizacije **SqlConnection.Open** . Način na koji se **Otvori** sada ugrađuje najbolji mehanizme pokušaj trud u odgovor tranzitne kvarove, za određene pogreške unutar veze isteklo razdoblje.
- Podržava okupljanje. To obuhvaća učinkovitog potvrdu koja funkcionira connection objekt pruža programa.



Kada koristite neki objekt veze iz skupa veza, preporučujemo da vaš program za privremeno zatvorite vezu ako odmah ne treba. Ponovno otvaranje veze nije skupi je način stvaranja nove veze.


Ako koristite ADO.NET 4.0 ili starijem, preporučujemo da nadograditi na najnoviju ADO.NET.

- Od 2015 studenom, možete [preuzeti ADO.NET 4.6.1](http://blogs.msdn.com/b/dotnet/archive/2015/11/30/net-framework-4-6-1-is-now-available.aspx).


<a id="e-diagnostics-test-utilities-connect" name="e-diagnostics-test-utilities-connect"></a>

## <a name="diagnostics"></a>Dijagnostika

<a id="d-test-whether-utilities-can-connect" name="d-test-whether-utilities-can-connect"></a>

### <a name="diagnostics-test-whether-utilities-can-connect"></a>Dijagnostika: Testiranje li uslužni programi mogu povezati


Ako vaš program ne daje da biste se povezali s bazom podataka SQL Azure, jedan dijagnostičkih je mogućnost pokušaja povezivanja s uslužni program. Najbolje uslužni se želite povezati pomoću iste biblioteke koje koristi vaš program.


Na bilo kojem računalu sa sustavom Windows, pokušajte sljedeće uslužni programi:

- SQL Server Management Studio (ssms.exe), koji se povezuje pomoću servisa za ADO.NET.
- sqlcmd.exe, koji se povezuje pomoću [ODBC](http://msdn.microsoft.com/library/jj730308.aspx).


Nakon uspostave testirajte li radi kratki SQL odabir upita.


<a id="f-diagnostics-check-open-ports" name="f-diagnostics-check-open-ports"></a>

### <a name="diagnostics-check-the-open-ports"></a>Dijagnostika: Provjera otvorene priključke


Pretpostavimo da sumnjate da se prilikom pokušaja povezivanja neuspješnih zbog problema s priključak. Na računalu možete pokrenuti program koji izvješća o konfiguracijama priključka.


Na Linux sljedeće uslužni programi mogu biti korisne:

- `netstat -nap`
- `nmap -sS -O 127.0.0.1`
 - (Promijenite vrijednost primjer tako da bude IP adresa).


U sustavu Windows [PortQry.exe](http://www.microsoft.com/download/details.aspx?id=17148) utility može biti koristan. Slijedi primjer izvođenja koji mu situaciji priključaka na poslužitelju baze podataka SQL Azure, a koji je pokrenut na prijenosnom računalu:


```
[C:\Users\johndoe\]
>> portqry.exe -n johndoesvr9.database.windows.net -p tcp -e 1433

Querying target system called:
 johndoesvr9.database.windows.net

Attempting to resolve name to IP address...
Name resolved to 23.100.117.95

querying...
TCP port 1433 (ms-sql-s service): LISTENING

[C:\Users\johndoe\]
>>
```


<a id="g-diagnostics-log-your-errors" name="g-diagnostics-log-your-errors"></a>

### <a name="diagnostics-log-your-errors"></a>Dijagnostike: Vaš pogreške zapisnika


Povremeni problem katkad najbolje prepoznate po otkrivanje Općenito uzorka tijekom dana ili tjedana.


Klijent vam mogu pomoći pri na Dijagnostika prijavom sve pogreške naiđe. Možda ćete moći stavke evidencije povezivanje s podacima o pogrešci koje baze podataka SQL Azure zapisnike sam interno.


Enterprise biblioteke 6 (EntLib60) nudi .NET upravlja klase radi jednostavnijeg zapisivanja:

- [5 – dovoljno kvartalne isključivanje zapisnik: pomoću aplikacije blokiranja zapisivanje](http://msdn.microsoft.com/library/dn440731.aspx)


<a id="h-diagnostics-examine-logs-errors" name="h-diagnostics-examine-logs-errors"></a>

### <a name="diagnostics-examine-system-logs-for-errors"></a>Dijagnostika: Pregledajte sustav zapisnika pogrešaka


Evo nekih odaberite Transact-SQL naredbe taj upit zapisnike o pogrešci i druge podatke.


| Upit zapisnika | Opis |
| :-- | :-- |
| `SELECT e.*`<br/>`FROM sys.event_log AS e`<br/>`WHERE e.database_name = 'myDbName'`<br/>`AND e.event_category = 'connectivity'`<br/>`AND 2 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, e.end_time, GetUtcDate())`<br/>`ORDER BY e.event_category,`<br/>&nbsp;&nbsp;`e.event_type, e.end_time;` | Prikaz [sys.event_log](http://msdn.microsoft.com/library/dn270018.aspx) nudi informacije o pojedinačnim događajima, uključujući neke koji mogu uzrokovati pogreške tranzitne ili povezivanje pogreške.<br/><br/>Najbolje **start_time** ili **end_time** vrijednosti možete povezivanje s podacima o kada je klijentski program naišao problema.<br/><br/>**Savjet:** Morate se povezati s **glavnom** bazom podataka da biste pokrenuli to. |
| `SELECT c.*`<br/>`FROM sys.database_connection_stats AS c`<br/>`WHERE c.database_name = 'myDbName'`<br/>`AND 24 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, c.end_time, GetUtcDate())`<br/>`ORDER BY c.end_time;` | Prikaz [sys.database_connection_stats](http://msdn.microsoft.com/library/dn269986.aspx) nudi Zbrojeno brojanja vrsta događaja dodatne dijagnostike sustava.<br/><br/>**Savjet:** Morate se povezati s **glavnom** bazom podataka da biste pokrenuli to. |

<a id="d-search-for-problem-events-in-the-sql-database-log" name="d-search-for-problem-events-in-the-sql-database-log"></a>

### <a name="diagnostics-search-for-problem-events-in-the-sql-database-log"></a>Dijagnostiku: Traženje problema događaje u zapisniku baze podataka SQL


Možete tražiti stavke o događajima problem u zapisniku baze podataka SQL Azure. Isprobajte sljedeće naredbe SELECT Transact-SQL u **glavnom** bazom podataka:


```
SELECT
   object_name
  ,CAST(f.event_data as XML).value
      ('(/event/@timestamp)[1]', 'datetime2')                      AS [timestamp]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="error"]/value)[1]', 'int')             AS [error]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="state"]/value)[1]', 'int')             AS [state]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="is_success"]/value)[1]', 'bit')        AS [is_success]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="database_name"]/value)[1]', 'sysname') AS [database_name]
FROM
  sys.fn_xe_telemetry_blob_target_read_file('el', null, null, null) AS f
WHERE
  object_name != 'login_event'  -- Login events are numerous.
  and
  '2015-06-21' < CAST(f.event_data as XML).value
        ('(/event/@timestamp)[1]', 'datetime2')
ORDER BY
  [timestamp] DESC
;
```


#### <a name="a-few-returned-rows-from-sysfnxetelemetryblobtargetreadfile"></a>Nekoliko vraćenih redaka iz sys.fn_xe_telemetry_blob_target_read_file


Sljedeći je vraćenih redaka možda izgleda. Null vrijednosti prikazane nisu često null u drugim recima.


```
object_name                   timestamp                    error  state  is_success  database_name

database_xml_deadlock_report  2015-10-16 20:28:01.0090000  NULL   NULL   NULL        AdventureWorks
```


<a id="l-enterprise-library-6" name="l-enterprise-library-6"></a>

## <a name="enterprise-library-6"></a>Biblioteka Enterprise 6


Enterprise biblioteke 6 (EntLib60) je okvir od .NET klase koji olakšava implementirati robusne klijenti servisa u oblaku, od kojih je servis baze podataka SQL Azure. Možete pronaći teme trakom za svako područje u kojem EntLib60 vam mogu pomoći Ako prvi posjetite:

- [Biblioteka Enterprise 6 – Travanj 2013](http://msdn.microsoft.com/library/dn169621%28v=pandp.60%29.aspx)


Pokušaj logike za rukovanje tranzitne pogreške je jedno područje u kojem EntLib60 vam mogu pomoći:

- [4 – perseverance, tajna sve uspjehe: pomoću aplikacije blokiranja rukovanje tranzitne kvara](http://msdn.microsoft.com/library/dn440719%28v=pandp.60%29.aspx)


> [AZURE.NOTE] Izvorni kod za EntLib60 dostupna je za [Preuzimanje](http://go.microsoft.com/fwlink/p/?LinkID=290898)javno. Microsoft je planovi da biste dodatno značajka ažuriranja i održavanje ažuriranja EntLib.

<a id="entlib60-classes-for-transient-errors-and-retry" name="entlib60-classes-for-transient-errors-and-retry"></a>

### <a name="entlib60-classes-for-transient-errors-and-retry"></a>Klase EntLib60 za tranzitne pogreške i pokušajte ponovno


Sljedeće klase EntLib60 osobito korisni su za logiku pokušajte ponovno. Sve to su ili su Dodatno u odjeljku naziva **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**:

*U prostoru naziva* *Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling* *:*

- **RetryPolicy** klasa
 - Metodom **ExecuteAction**


- **ExponentialBackoff** klasa


- **SqlDatabaseTransientErrorDetectionStrategy** klasa


- **ReliableSqlConnection** klasa
 - Način **ExecuteCommand**


U prostoru naziva **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling.TestSupport**:

- **AlwaysTransientErrorDetectionStrategy** klasa

- **NeverTransientErrorDetectionStrategy** klasa


Evo veze na informacije o EntLib60:

- Besplatni [rezervirajte preuzimanje: vodič za programere Microsoft Enterprise biblioteku, 2 Edition](http://www.microsoft.com/download/details.aspx?id=41145)

- Najbolje prakse: [ponovno pokušajte opće smjernice](../best-practices-retry-general.md) sadrži izvrstan detaljnije rasprave logike pokušajte ponovno.

- Preuzimanje NuGet biblioteke [Enterprise – zadužen za tranzitne kvara bloka aplikacije 6.0](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/)

<a id="entlib60-the-logging-block" name="entlib60-the-logging-block"></a>

### <a name="entlib60-the-logging-block"></a>EntLib60: Zapisivanje bloka


- Blokiranje zapisivanje je vrlo fleksibilne i konfigurirati rješenja koja vam omogućuje:
 - Stvaranje i spremanje zapisnika poruka u raznih mjesta.
 - Kategoriziraj i filtriranje poruka.
 - Prikupljanje kontekstne informacije koje su korisne za ispravljanje pogrešaka i praćenje, kao i za nadzor i opće zapisivanje preduvjeti.


- Blokiranje zapisivanje abstracts funkcija zapisivanja u zapisnik odredište tako da je dosljedan, bez obzira na mjesto i vrstu spremišta zapisivanje ciljne aplikacije kod.


Detalje potražite u člancima: [5 - kao jednostavno kao kvartalne isključiti na zapisnika: pomoću aplikacije blokiranja zapisivanje](https://msdn.microsoft.com/library/dn440731%28v=pandp.60%29.aspx)

<a id="entlib60-istransient-method-source-code" name="entlib60-istransient-method-source-code"></a>

### <a name="entlib60-istransient-method-source-code"></a>EntLib60 IsTransient metoda izvornog koda


Nakon toga iz klase **SqlDatabaseTransientErrorDetectionStrategy** je C# izvorni kod za metodu **IsTransient** . Izvorni kod pojašnjava pogrešaka koje su smatraju tranzitne i worthy od Ponovi na Travanj 2013.

Brojne **//comment** redaka uklonjeni su iz ovu kopiju da biste istaknuli čitljivosti.


```
public bool IsTransient(Exception ex)
{
  if (ex != null)
  {
    SqlException sqlException;
    if ((sqlException = ex as SqlException) != null)
    {
      // Enumerate through all errors found in the exception.
      foreach (SqlError err in sqlException.Errors)
      {
        switch (err.Number)
        {
            // SQL Error Code: 40501
            // The service is currently busy. Retry the request after 10 seconds.
            // Code: (reason code to be decoded).
          case ThrottlingCondition.ThrottlingErrorNumber:
            // Decode the reason code from the error message to
            // determine the grounds for throttling.
            var condition = ThrottlingCondition.FromError(err);

            // Attach the decoded values as additional attributes to
            // the original SQL exception.
            sqlException.Data[condition.ThrottlingMode.GetType().Name] =
              condition.ThrottlingMode.ToString();
            sqlException.Data[condition.GetType().Name] = condition;

            return true;

          case 10928:
          case 10929:
          case 10053:
          case 10054:
          case 10060:
          case 40197:
          case 40540:
          case 40613:
          case 40143:
          case 233:
          case 64:
            // DBNETLIB Error Code: 20
            // The instance of SQL Server you attempted to connect to
            // does not support encryption.
          case (int)ProcessNetLibErrorCode.EncryptionNotSupported:
            return true;
        }
      }
    }
    else if (ex is TimeoutException)
    {
      return true;
    }
    else
    {
      EntityException entityException;
      if ((entityException = ex as EntityException) != null)
      {
        return this.IsTransient(entityException.InnerException);
      }
    }
  }

  return false;
}
```


## <a name="next-steps"></a>Daljnji koraci

- Otklanjanje poteškoća s drugim uobičajenih problema s povezivanjem baze podataka SQL Azure, potražite u članku [Otklanjanje poteškoća s povezivanjem s bazom podataka SQL Azure](sql-database-troubleshoot-common-connection-issues.md).

- [Red čekanja (ADO.NET) povezivanja za SQL Server](http://msdn.microsoft.com/library/8xx3tyca.aspx)


- [*Retrying* je programa 2.0 Apache licencirani općenite namjene ponovni biblioteke, pisan **Python**, da biste pojednostavnili zadatak dodavanje ponašanje Ponovi na gotovo bilo čega.](https://pypi.python.org/pypi/retrying)
