<properties 
    pageTitle="Kod XEvent događaj datoteke baze podataka SQL | Microsoft Azure" 
    description="Nudi PowerShell i Transact-SQL za uzorak two-phase kod koji pokazuje ciljnu datoteku događaj prošireni događaj na baze podataka SQL Azure. Azure prostora za pohranu je obaveznu komponentu scenarij." 
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


# <a name="event-file-target-code-for-extended-events-in-sql-database"></a>Kod ciljnu datoteku događaja za prošireni događaje u SQL baze podataka

[AZURE.INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

Želite uzorak dovršeno koda za robusne omogućuje prikupljanje izvješća i informacije za prošireni događaj.


U sustavu Microsoft SQL Server [ciljnu datoteku događaj](http://msdn.microsoft.com/library/ff878115.aspx) se koristi za pohranu izlaze događaja u datoteku tvrdog diska. No takve datoteke nisu dostupne s bazom podataka SQL Azure. Umjesto toga ćemo pomoću servisa Azure prostora za pohranu podržava ciljnu datoteku događaj.


Ovaj članak prikazuje uzorak two-phase kod:


- PowerShell stvaranje kontejnera za Azure prostora za pohranu u oblaku.

- SQL transakcija:
 - Da biste dodijelili spremnik Azure prostora za pohranu programa ciljnu datoteku događaj.
 - Za stvaranje i pokretanje sesije događaja i tako dalje.


## <a name="prerequisites"></a>Preduvjeti


- Račun za Azure i pretplate. Možete se prijaviti za [besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/).


- Bilo kojoj možete stvoriti tablicu u bazi podataka.
 - Po želji možete [stvoriti bazu podataka pokazni **AdventureWorksLT** ](sql-database-get-started.md) u minutama.


- SQL Server Management Studio (ssms.exe), najbolje najnoviju mjesečnog ažuriranja verziju. Možete preuzeti najnoviju ssms.exe iz:
 - Temi pod naslovom [Preuzimanje SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx).
 - [Izravnu vezu za preuzimanje.](http://go.microsoft.com/fwlink/?linkid=616025)


- Morate imati [modula Azure PowerShell](http://go.microsoft.com/?linkid=9811175) instaliran.
 - Module sadrže naredbe kao što su – **Novo AzureStorageAccount**.


## <a name="phase-1-powershell-code-for-azure-storage-container"></a>Faza 1: PowerShell kod za pohranu Azure kontejner


U ovom PowerShell je faza 1 two-phase kod uzorka.

Skripta počinje naredbe da biste očistili nakon na prethodni moguće pokrenuti i je rerunnable.



1. Zalijepite skriptu PowerShell jednostavan uređivač teksta kao što su Notepad.exe i spremite skriptu kao datoteku s nastavkom **.ps1**.

2. Kao Administrator pokrenuti Očisti filtar.

3. U naredbeni redak upišite<br/>`Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser`<br/>a zatim pritisnite Enter.

4. Očisti filtar, otvorite datoteku **.ps1** . Pokrenite skriptu.

5. Skripta pokrene novi prozor u kojem se prijavite u Azure.
 - Ako ponovno pokrenite skriptu ne prekida sesiju imate mogućnost praktičan komentiranja out naredbu **Dodaj AzureAccount** .


![Očisti filtar s modul Azure instalirane, spremni pokrenuti skriptu.][30_powershell_ise]


&nbsp;


```
## TODO: Before running, find all 'TODO' and make each edit!!

#--------------- 1 -----------------------


# You can comment out or skip this Add-AzureAccount
# command after the first run.
# Current PowerShell environment retains the successful outcome.

'Expect a pop-up window in which you log in to Azure.'


Add-AzureAccount

#-------------- 2 ------------------------


'
TODO: Edit the values assigned to these variables, especially the first few!
'

# Ensure the current date is between
# the Expiry and Start time values that you edit here.

$subscriptionName    = 'YOUR_SUBSCRIPTION_NAME'
$policySasExpiryTime = '2016-01-28T23:44:56Z'
$policySasStartTime  = '2015-08-01'


$storageAccountName     = 'gmstorageaccountxevent'
$storageAccountLocation = 'West US'
$contextName            = 'gmcontext'
$containerName          = 'gmcontainerxevent'
$policySasToken         = 'gmpolicysastoken'


# Leave this value alone, as 'rwl'.
$policySasPermission = 'rwl'

#--------------- 3 -----------------------


# The ending display lists your Azure subscriptions.
# One should match the $subscriptionName value you assigned
#   earlier in this PowerShell script. 

'Choose an existing subscription for the current PowerShell environment.'


Select-AzureSubscription -SubscriptionName $subscriptionName


#-------------- 4 ------------------------


'
Clean up the old Azure Storage Account after any previous run, 
before continuing this new run.'


If ($storageAccountName)
{
    Remove-AzureStorageAccount -StorageAccountName $storageAccountName
}

#--------------- 5 -----------------------

[System.DateTime]::Now.ToString()

'
Create a storage account. 
This might take several minutes, will beep when ready.
  ...PLEASE WAIT...'

New-AzureStorageAccount `
    -StorageAccountName $storageAccountName `
    -Location           $storageAccountLocation

[System.DateTime]::Now.ToString()

[System.Media.SystemSounds]::Beep.Play()


'
Get the primary access key for your storage account.
'


$primaryAccessKey_ForStorageAccount = `
    (Get-AzureStorageKey `
        -StorageAccountName $storageAccountName).Primary

"`$primaryAccessKey_ForStorageAccount = $primaryAccessKey_ForStorageAccount"

'Azure Storage Account cmdlet completed.
Remainder of PowerShell .ps1 script continues.
'

#--------------- 6 -----------------------


# The context will be needed to create a container within the storage account.

'Create a context object from the storage account and its primary access key.
'

$context = New-AzureStorageContext `
    -StorageAccountName $storageAccountName `
    -StorageAccountKey  $primaryAccessKey_ForStorageAccount


'Create a container within the storage account.
'


$containerObjectInStorageAccount = New-AzureStorageContainer `
    -Name    $containerName `
    -Context $context


'Create a security policy to be applied to the SAS token.
'

New-AzureStorageContainerStoredAccessPolicy `
    -Container  $containerName `
    -Context    $context `
    -Policy     $policySasToken `
    -Permission $policySasPermission `
    -ExpiryTime $policySasExpiryTime `
    -StartTime  $policySasStartTime 

'
Generate a SAS token for the container.
'
Try
{
    $sasTokenWithPolicy = New-AzureStorageContainerSASToken `
        -Name    $containerName `
        -Context $context `
        -Policy  $policySasToken
}
Catch 
{
    $Error[0].Exception.ToString()
}

#-------------- 7 ------------------------


'Display the values that YOU must edit into the Transact-SQL script next!:
'

"storageAccountName: $storageAccountName"
"containerName:      $containerName"
"sasTokenWithPolicy: $sasTokenWithPolicy"

'
REMINDER: sasTokenWithPolicy here might start with "?" character, which you must exclude from Transact-SQL.
'

'
(Later, return here to delete your Azure Storage account. See the preceding - Remove-AzureStorageAccount -StorageAccountName $storageAccountName)'

'
Now shift to the Transact-SQL portion of the two-part code sample!'

# EOFile
```


&nbsp;


Imajte na umu nekoliko imenovanih vrijednosti koja se ispisuje skriptu PowerShell kada završava. Potrebno je urediti te se vrijednosti u Transact-SQL skriptu koja slijedi kao faza 2.


## <a name="phase-2-transact-sql-code-that-uses-azure-storage-container"></a>Faza 2: Transakcija SQL kod koji koristi pohranu Azure spremnik


- U fazi 1 ovaj uzorak koda ste pokrenuli skriptu PowerShell stvaranje kontejnera za Azure prostora za pohranu.
- Zatim u fazi 2 sljedeću skriptu Transact-SQL morate koristiti spremnik.


Skripta počinje naredbe da biste očistili nakon na prethodni moguće pokrenuti i je rerunnable.


Skriptu PowerShell ispisati nekoliko imenovanih vrijednosti kad je završen. Potrebno je urediti Transact-SQL skriptu da biste koristili te se vrijednosti. **Popis obveza** možete pronaći u Transact-SQL skriptu da biste pronašli Uredi točke.


1. Otvorite SQL Server Management Studio (ssms.exe).

2. Povezivanje s bazom podataka SQL Azure baze podataka.

3. Kliknite da biste otvorili novi okno upita.

4. Zalijepite sljedeću skriptu Transact-SQL u oknu upita.

5. Pronađite svaku **obveze** u skripti i unesite odgovarajuće izmjene.

6. Spremite i pokrenuti skriptu.


&nbsp;


> [AZURE.WARNING] Vrijednost ključa SAS generira prethodni skriptu PowerShell može započeti znakom na '? " (upitnik). Kada koristite tipku SAS u sljedeću skriptu T SQL, morate *ukloniti vodeće "?"*. U suprotnom pokušajima možda blokira sigurnost.


&nbsp;


```
---- TODO: First, run the PowerShell portion of this two-part code sample.
---- TODO: Second, find every 'TODO' in this Transact-SQL file, and edit each.

---- Transact-SQL code for Event File target on Azure SQL Database.


SET NOCOUNT ON;
GO


----  Step 1.  Establish one little table, and  ---------
----  insert one row of data.


IF EXISTS
    (SELECT * FROM sys.objects
        WHERE type = 'U' and name = 'gmTabEmployee')
BEGIN
    DROP TABLE gmTabEmployee;
END
GO


CREATE TABLE gmTabEmployee
(
    EmployeeGuid         uniqueIdentifier   not null  default newid()  primary key,
    EmployeeId           int                not null  identity(1,1),
    EmployeeKudosCount   int                not null  default 0,
    EmployeeDescr        nvarchar(256)          null
);
GO


INSERT INTO gmTabEmployee ( EmployeeDescr )
    VALUES ( 'Jane Doe' );
GO


------  Step 2.  Create key, and  ------------
------  Create credential (your Azure Storage container must already exist).


IF NOT EXISTS
    (SELECT * FROM sys.symmetric_keys
        WHERE symmetric_key_id = 101)
BEGIN
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '0C34C960-6621-4682-A123-C7EA08E3FC46' -- Or any newid().
END
GO


IF EXISTS
    (SELECT * FROM sys.database_scoped_credentials
        -- TODO: Assign AzureStorageAccount name, and the associated Container name.
        WHERE name = 'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent')
BEGIN
    DROP DATABASE SCOPED CREDENTIAL
        -- TODO: Assign AzureStorageAccount name, and the associated Container name.
        [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent] ;
END
GO


CREATE
    DATABASE SCOPED
    CREDENTIAL
        -- use '.blob.',   and not '.queue.' or '.table.' etc.
        -- TODO: Assign AzureStorageAccount name, and the associated Container name.
        [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent]
    WITH
        IDENTITY = 'SHARED ACCESS SIGNATURE',  -- "SAS" token.
        -- TODO: Paste in the long SasToken string here for Secret, but exclude any leading '?'.
        SECRET = 'sv=2014-02-14&sr=c&si=gmpolicysastoken&sig=EjAqjo6Nu5xMLEZEkMkLbeF7TD9v1J8DNB2t8gOKTts%3D'
    ;
GO


------  Step 3.  Create (define) an event session.  --------
------  The event session has an event with an action,
------  and a has a target.

IF EXISTS
    (SELECT * from sys.database_event_sessions
        WHERE name = 'gmeventsessionname240b')
BEGIN
    DROP
        EVENT SESSION
            gmeventsessionname240b
        ON DATABASE;
END
GO


CREATE
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE

    ADD EVENT
        sqlserver.sql_statement_starting
            (
            ACTION (sqlserver.sql_text)
            WHERE statement LIKE 'UPDATE gmTabEmployee%'
            )
    ADD TARGET
        package0.event_file
            (
            -- TODO: Assign AzureStorageAccount name, and the associated Container name.
            -- Also, tweak the .xel file name at end, if you like.
            SET filename =
                'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent/anyfilenamexel242b.xel'
            )
    WITH
        (MAX_MEMORY = 10 MB,
        MAX_DISPATCH_LATENCY = 3 SECONDS)
    ;
GO


------  Step 4.  Start the event session.  ----------------
------  Issue the SQL Update statements that will be traced.
------  Then stop the session.

------  Note: If the target fails to attach,
------  the session must be stopped and restarted.

ALTER
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE
    STATE = START;
GO


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM gmTabEmployee;

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 2
    WHERE EmployeeDescr = 'Jane Doe';

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 13
    WHERE EmployeeDescr = 'Jane Doe';

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM gmTabEmployee;
GO


ALTER
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE
    STATE = STOP;
GO


-------------- Step 5.  Select the results. ----------

SELECT
        *, 'CLICK_NEXT_CELL_TO_BROWSE_ITS_RESULTS!' as [CLICK_NEXT_CELL_TO_BROWSE_ITS_RESULTS],
        CAST(event_data AS XML) AS [event_data_XML]  -- TODO: In ssms.exe results grid, double-click this cell!
    FROM
        sys.fn_xe_file_target_read_file
            (
                -- TODO: Fill in Storage Account name, and the associated Container name.
                'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent/anyfilenamexel242b',
                null, null, null
            );
GO


-------------- Step 6.  Clean up. ----------

DROP
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE;
GO

DROP DATABASE SCOPED CREDENTIAL
    -- TODO: Assign AzureStorageAccount name, and the associated Container name.
    [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent]
    ;
GO

DROP TABLE gmTabEmployee;
GO

PRINT 'Use PowerShell Remove-AzureStorageAccount to delete your Azure Storage account!';
GO
```


&nbsp;


Cilj ne uspije priložiti prilikom pokretanja, morate zaustaviti i ponovno pokrenite sesiju događaja:


```
ALTER EVENT SESSION ... STATE = STOP;
GO
ALTER EVENT SESSION ... STATE = START;
GO
```


&nbsp;


## <a name="output"></a>Izlaz


Kada Transact-SQL skripte dovrši, kliknite ćeliju u zaglavlju stupca **event_data_XML** . Jedan **<event>** prikazuje element koji prikazuje jednu Naredba UPDATE.

Evo neki **<event>** element koja je stvorena tijekom testiranja:


&nbsp;


```
<event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T19:18:45.420Z">
  <data name="state">
    <value>0</value>
    <text>Normal</text>
  </data>
  <data name="line_number">
    <value>5</value>
  </data>
  <data name="offset">
    <value>148</value>
  </data>
  <data name="offset_end">
    <value>368</value>
  </data>
  <data name="statement">
    <value>UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 2
    WHERE EmployeeDescr = 'Jane Doe'</value>
  </data>
  <action name="sql_text" package="sqlserver">
    <value>

SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM gmTabEmployee;

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 2
    WHERE EmployeeDescr = 'Jane Doe';

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 13
    WHERE EmployeeDescr = 'Jane Doe';

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM gmTabEmployee;
</value>
  </action>
</event>
```

&nbsp;


Prethodni Transact-SQL skripte koristi sljedeću funkciju sustava za čitanje na event_file:

- [sys.fn_xe_file_target_read_file (Transact-SQL)](http://msdn.microsoft.com/library/cc280743.aspx)


Objašnjenje Napredne mogućnosti za prikaz podataka iz prošireni događaje dostupna je na:

- [Napredni prikaz ciljnih podataka iz prošireni događaja](http://msdn.microsoft.com/library/mt752502.aspx)

&nbsp;


## <a name="converting-the-code-sample-to-run-on-sql-server"></a>Pretvorba uzorak koda za izvođenje na SQL Server


Pretpostavimo da ste željeli pokrenuti prethodni primjer Transact-SQL Microsoft SQL Server.


- Zbog jednostavnosti, želite želite potpuno korištenje spremnik za pohranu Azure zamijenite jednostavnog kao što su **C:\myeventdata.xel**. Datoteku želite staviti na lokalni disk računala na kojem se nalazi SQL Server.


- Ne morate bilo kakvu vrstu Transact-SQL naredbe za **Stvaranje GLAVNI KLJUČ** i **Stvaranje VJERODAJNICA**.


- U naredbu **Stvori EVENT SESIJA** u uvjetu njegov **Dodavanje CILJNE** želite zamijeniti Http vrijednost dodijeljeno pokušaj **filename =** s nizom cijeli put kao što je **C:\myfile.xel**.
 - Bez računa za pohranu Azure moraju biti uključen.


## <a name="more-information"></a>Dodatne informacije


Dodatne informacije o računima i spremnika u servisu Azure prostora za pohranu potražite u članku:

- [Upute za korištenje spremišta blobova iz .NET](../storage/storage-dotnet-how-to-use-blobs.md)
- [Dodjela naziva i pozivate spremnika, blob-ova i metapodaci](http://msdn.microsoft.com/library/azure/dd135715.aspx)
- [Rad s korijenski spremnik](http://msdn.microsoft.com/library/azure/ee395424.aspx)
- [Nastave 1: Stvorite pravilo pohranjene access i zajednički pristup potpis na Azure spremnik](http://msdn.microsoft.com/library/dn466430.aspx)
    - [Lekciju 2: Stvaranje SQL Server vjerodajnica koriste zajednički pristup potpis](http://msdn.microsoft.com/library/dn466435.aspx)




<!--
Image references.
-->

[30_powershell_ise]: ./media/sql-database-xevent-code-event-file/event-file-powershell-ise-b30.png

