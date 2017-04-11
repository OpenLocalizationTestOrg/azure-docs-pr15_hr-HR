<properties 
    pageTitle="Međuspremnik Nazovi XEvent kod za baze podataka SQL | Microsoft Azure" 
    description="Nudi uzorka Transact-SQL kod koji je stvoren jednostavno i brzo pomoću cilja Nazovi međuspremnik u bazi podataka SQL Azure." 
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


# <a name="ring-buffer-target-code-for-extended-events-in-sql-database"></a>Pozivanje međuspremnik cilj kod za prošireni događaje u SQL baze podataka

[AZURE.INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

Želite uzorak dovršeno koda za najlakše brze snimke izvješća i informacije za prošireni događaja tijekom testiranja. Najjednostavniji cilj za prošireni događaj podatke je [Međuspremnik Nazovi cilj](http://msdn.microsoft.com/library/ff878182.aspx).


U ovoj se temi predstavlja uzorka Transact-SQL kod koji:


1. Stvaranje tablice s podacima da bismo pokazali s.

2. Stvara sesije za postojeće prošireni događaja, odnosno **sqlserver.sql_statement_starting**.
    - Događaj ograničeno je na SQL naredbe koje sadrže određeni niz ažuriranja: **Izjava kao što su "ažuriranje tabEmployee %"**.
    - Odabire za slanje izlaz događaja ciljno vrste međuspremnika Nazovi, odnosno **package0.ring_buffer**.

3. Pokreće sesiju događaj.

4. Problemi s nekoliko jednostavnih ažuriranje SQL naredbe.

5. Problemi SQL odaberite dohvatiti događaj Izlaz iz međuspremnika Nazovi.
    - pridruženo je **sys.dm_xe_database_session_targets** i druge prikaze dinamički management (DMVs).

6. Prekida sesiju događaj.

7. Izostavlja cilj Nazovi međuspremnik, da biste predali njegovih resursa.

8. Izostavlja sesiju događaja i pokazni videozapis tablice.


## <a name="prerequisites"></a>Preduvjeti


- Račun za Azure i pretplate. Možete se prijaviti za [besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/).


- Bilo kojoj možete stvoriti tablicu u bazi podataka.
 - Po želji možete [stvoriti bazu podataka pokazni **AdventureWorksLT** ](sql-database-get-started.md) u minutama.


- SQL Server Management Studio (ssms.exe), najbolje najnoviju mjesečnog ažuriranja verziju. Možete preuzeti najnoviju ssms.exe iz:
 - Temi pod naslovom [Preuzimanje SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx).
 - [Izravnu vezu za preuzimanje.](http://go.microsoft.com/fwlink/?linkid=616025)


## <a name="code-sample"></a>Uzorak koda


S vrlo pomoćna izmjenom sljedećim primjerom koda Nazovi međuspremnik mogu se izvoditi na baze podataka SQL Azure ili Microsoft SQL Server. Razlika je prisutnost čvor '_database' naziv neki prikazi dinamički management (DMVs) koristiti u uvjetu FROM u koraku 5. Ako, na primjer:

- sys.dm_xe**_database**_session_targets
- sys.dm_xe_session_targets


&nbsp;


```
GO
----  Transact-SQL.
---- Step set 1.

SET NOCOUNT ON;
GO


IF EXISTS
    (SELECT * FROM sys.objects
        WHERE type = 'U' and name = 'tabEmployee')
BEGIN
    DROP TABLE tabEmployee;
END
GO


CREATE TABLE tabEmployee
(
    EmployeeGuid         uniqueIdentifier   not null  default newid()  primary key,
    EmployeeId           int                not null  identity(1,1),
    EmployeeKudosCount   int                not null  default 0,
    EmployeeDescr        nvarchar(256)          null
);
GO


INSERT INTO tabEmployee ( EmployeeDescr )
    VALUES ( 'Jane Doe' );
GO

---- Step set 2.


IF EXISTS
    (SELECT * from sys.database_event_sessions
        WHERE name = 'eventsession_gm_azuresqldb51')
BEGIN
    DROP EVENT SESSION eventsession_gm_azuresqldb51
        ON DATABASE;
END
GO


CREATE
    EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    ADD EVENT
        sqlserver.sql_statement_starting
            (
            ACTION (sqlserver.sql_text)
            WHERE statement LIKE '%UPDATE tabEmployee%'
            )
    ADD TARGET
        package0.ring_buffer
            (SET
                max_memory = 500   -- Units of KB.
            );
GO

---- Step set 3.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    STATE = START;
GO

---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
GO

---- Step set 5.


SELECT
    se.name                      AS [session-name],
    ev.event_name,
    ac.action_name,
    st.target_name,
    se.session_source,
    st.target_data,
    CAST(st.target_data AS XML)  AS [target_data_XML]
FROM
               sys.dm_xe_database_session_event_actions  AS ac

    INNER JOIN sys.dm_xe_database_session_events         AS ev  ON ev.event_name = ac.event_name
        AND CAST(ev.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_session_object_columns AS oc
         ON CAST(oc.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_session_targets        AS st
         ON CAST(st.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_sessions               AS se
         ON CAST(ac.event_session_address AS BINARY(8)) = CAST(se.address AS BINARY(8))
WHERE
        oc.column_name = 'occurrence_number'
    AND
        se.name        = 'eventsession_gm_azuresqldb51'
    AND
        ac.action_name = 'sql_text'
ORDER BY
    se.name,
    ev.event_name,
    ac.action_name,
    st.target_name,
    se.session_source
;
GO

---- Step set 6.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    STATE = STOP;
GO

---- Step set 7.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    DROP TARGET package0.ring_buffer;
GO

---- Step set 8.


DROP EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE;
GO

DROP TABLE tabEmployee;
GO
```


&nbsp;


## <a name="ring-buffer-contents"></a>Pozivanje sadržaja međuspremnika


Da biste pokrenuli uzorak koda koristi ssms.exe.


Da biste vidjeli rezultate, kliknuli smo ćeliju ispod zaglavlja stupca **target_data_XML**.

Zatim u oknu s rezultatima ne možemo kliknete ćeliju ispod zaglavlja stupca **target_data_XML**. U ovom kliknite kartice neke druge datoteke u ssms.exe koji sadržaj ćelije rezultat je prikazuje se u obliku XML.


Izlaz prikazuju se u sljedećim blok. Izgleda dugo, ali je samo dva **<event>** elemente.


&nbsp;


```
<RingBufferTarget truncated="0" processingTime="0" totalEventsProcessed="2" eventCount="2" droppedCount="0" memoryUsed="1728">
  <event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T15:29:31.317Z">
    <data name="state">
      <type name="statement_starting_state" package="sqlserver" />
      <value>0</value>
      <text>Normal</text>
    </data>
    <data name="line_number">
      <type name="int32" package="package0" />
      <value>7</value>
    </data>
    <data name="offset">
      <type name="int32" package="package0" />
      <value>184</value>
    </data>
    <data name="offset_end">
      <type name="int32" package="package0" />
      <value>328</value>
    </data>
    <data name="statement">
      <type name="unicode_string" package="package0" />
      <value>UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102</value>
    </data>
    <action name="sql_text" package="sqlserver">
      <type name="unicode_string" package="package0" />
      <value>
---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
</value>
    </action>
  </event>
  <event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T15:29:31.327Z">
    <data name="state">
      <type name="statement_starting_state" package="sqlserver" />
      <value>0</value>
      <text>Normal</text>
    </data>
    <data name="line_number">
      <type name="int32" package="package0" />
      <value>10</value>
    </data>
    <data name="offset">
      <type name="int32" package="package0" />
      <value>340</value>
    </data>
    <data name="offset_end">
      <type name="int32" package="package0" />
      <value>486</value>
    </data>
    <data name="statement">
      <type name="unicode_string" package="package0" />
      <value>UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015</value>
    </data>
    <action name="sql_text" package="sqlserver">
      <type name="unicode_string" package="package0" />
      <value>
---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
</value>
    </action>
  </event>
</RingBufferTarget>
```


#### <a name="release-resources-held-by-your-ring-buffer"></a>Resursi za izdanje zaključale vaše Nazovi međuspremnik


Kada ste gotovi s vašeg međuspremnik Nazovi, možete ga ukloniti i pustite njegovih resursa izdavanja do **MIJENJATI** kao što je sljedeća:


```
ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    DROP TARGET package0.ring_buffer;
GO
```


Definicija event sesija je ažurirati, ali ne prekine. Kasnije možete dodati drugu instancu programa međuspremnik Nazovi sesiju događaja:


```
ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    ADD TARGET
        package0.ring_buffer
            (SET
                max_memory = 500   -- Units of KB.
            );
```


## <a name="more-information"></a>Dodatne informacije


Primarni temu za prošireni događaje na baze podataka SQL Azure je:


- [Prošireno razmatranja događaja u SQL baze podataka](sql-database-xevent-db-diff-from-svr.md)koji contrasts neke aspekte prošireni događaja koje se razlikuju baze podataka SQL Azure i Microsoft SQL Server.


Druge teme uzorka kod za prošireni događaje dostupne su na sljedećim vezama. Međutim, morate često provjerite sve uzorka da biste vidjeli je li uzorak pronalaze Microsoft SQL Server nasuprot baze podataka SQL Azure. Zatim možete odlučiti hoće li se manje promjene potrebne za pokretanje uzorka.


- Uzorak koda za baze podataka SQL Azure: [kod ciljnu datoteku događaj za prošireni događaje u bazi podataka SQL](sql-database-xevent-code-event-file.md)


<!--
('lock_acquired' event.)

- Code sample for SQL Server: [Determine Which Queries Are Holding Locks](http://msdn.microsoft.com/library/bb677357.aspx)
- Code sample for SQL Server: [Find the Objects That Have the Most Locks Taken on Them](http://msdn.microsoft.com/library/bb630355.aspx)
-->
