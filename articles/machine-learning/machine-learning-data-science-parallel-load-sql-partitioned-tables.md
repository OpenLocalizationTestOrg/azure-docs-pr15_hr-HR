<properties 
    pageTitle="Paralelan masovnog uvoza podataka pomoću SQL particiju tablice | Microsoft Azure" 
    description="Paralelni masovnog uvoza podataka pomoću SQL particiju tablice" 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev"
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016" 
    ms.author="bradsev" /> 

# <a name="parallel-bulk-data-import-using-sql-partition-tables"></a>Paralelni masovnog uvoza podataka pomoću SQL particiju tablice

Ovom se dokumentu opisuje kako izraditi particioniranom tablice za brzo paralelne masovnog uvoza podataka SQL Server bazu podataka. Za velike učitavanje/prijenosa podataka SQL bazu podataka, uvoz podataka SQL DB i naknadna upiti mogu poboljšati pomoću _particije tablice i prikazi_. 


## <a name="create-a-new-database-and-a-set-of-filegroups"></a>Stvaranje nove baze podataka i skup filegroups

- [Stvaranje nove baze podataka](https://technet.microsoft.com/library/ms176061.aspx) (Ako ne postoji)
- Dodavanje baze podataka filegroups bazu podataka koji će držite particioniranom fizičkih datoteka

  Napomena: To se može učiniti s [STVORITI bazu podataka](https://technet.microsoft.com/library/ms176061.aspx) ako je novi ili [ALTER bazu podataka](https://msdn.microsoft.com/library/bb522682.aspx) ako već postoji baza podataka

- Dodavanje jedne ili više datoteka (prema potrebi) za svaku grupu datoteka baze podataka

 > [AZURE.NOTE] Odredite grupu datoteka cilj koji drži podacima za tu particiju i naziv(e) datoteke fizičke baze podataka gdje će biti pohranjeno grupu datoteka podataka.
 
Sljedeći primjer stvara novu bazu podataka s tri filegroups nije primarni i zapisnik grupe koja sadrži jednu datoteku fizičke u svakom. Datoteke baze podataka stvaraju u zadanu mapu podataka SQL poslužitelja kao konfigurirane u SQL Server instanci. Dodatne informacije o zadana mjesta datoteka pogledajte [Mjesta datoteka za zadani i pod nazivom instance sustava SQL Server](https://msdn.microsoft.com/library/ms143547.aspx).

    DECLARE @data_path nvarchar(256);
    SET @data_path = (SELECT SUBSTRING(physical_name, 1, CHARINDEX(N'master.mdf', LOWER(physical_name)) - 1)
      FROM master.sys.master_files
      WHERE database_id = 1 AND file_id = 1);
    
    EXECUTE ('
        CREATE DATABASE <database_name>
        ON  PRIMARY 
        ( NAME = ''Primary'', FILENAME = ''' + @data_path + '<primary_file_name>.mdf'', SIZE = 4096KB , FILEGROWTH = 1024KB ), 
        FILEGROUP [filegroup_1] 
        ( NAME = ''FileGroup1'', FILENAME = ''' + @data_path + '<file_name_1>.ndf'' , SIZE = 4096KB , FILEGROWTH = 1024KB ), 
        FILEGROUP [filegroup_2] 
        ( NAME = ''FileGroup1'', FILENAME = ''' + @data_path + '<file_name_2>.ndf'' , SIZE = 4096KB , FILEGROWTH = 1024KB ), 
        FILEGROUP [filegroup_3] 
        ( NAME = ''FileGroup1'', FILENAME = ''' + @data_path + '<file_name>.ndf'' , SIZE = 102400KB , FILEGROWTH = 10240KB ), 
        LOG ON 
        ( NAME = ''LogFileGroup'', FILENAME = ''' + @data_path + '<log_file_name>.ldf'' , SIZE = 1024KB , FILEGROWTH = 10%)
    ')
    
## <a name="create-a-partitioned-table"></a>Stvaranje particioniranom tablice

Stvoriti particioniranom tablice prema shemi podataka mapirani filegroups baze podataka stvorene u prethodnom koraku. Kada se podaci uvezeni u particioniranom tablice masovnog, zapisi će se raspodijeliti među filegroups prema shemu particiju kao što je opisano ispod.

**Za stvaranje tablice particiju, morate:**

- [Stvaranje particije funkcija](https://msdn.microsoft.com/library/ms187802.aspx) koji određuje raspon vrijednosti/granice uključeni u svakoj tablici pojedinačne particiju, npr., ograničiti particije po mjesecu (neke\_datetime\_polje) u godini 2013:

        CREATE PARTITION FUNCTION <DatetimeFieldPFN>(<datetime_field>)  
        AS RANGE RIGHT FOR VALUES (
            '20130201', '20130301', '20130401',
            '20130501', '20130601', '20130701', '20130801',
            '20130901', '20131001', '20131101', '20131201' )

- [Stvaranje particije shemu](https://msdn.microsoft.com/library/ms179854.aspx) koja mapira svaki raspon particije u funkcija partition fizičke grupu datoteka npr.:

        CREATE PARTITION SCHEME <DatetimeFieldPScheme> AS  
        PARTITION <DatetimeFieldPFN> TO (
        <filegroup_1>, <filegroup_2>, <filegroup_3>, <filegroup_4>,
        <filegroup_5>, <filegroup_6>, <filegroup_7>, <filegroup_8>,
        <filegroup_9>, <filegroup_10>, <filegroup_11>, <filegroup_12> )

  Savjet: Da biste provjerili raspone na snazi u svaku particiju prema funkcija/shemu, pokrenite sljedeći upit:

        SELECT psch.name as PartitionScheme,
            prng.value AS ParitionValue,
            prng.boundary_id AS BoundaryID
        FROM sys.partition_functions AS pfun
        INNER JOIN sys.partition_schemes psch ON pfun.function_id = psch.function_id
        INNER JOIN sys.partition_range_values prng ON prng.function_id=pfun.function_id
        WHERE pfun.name = <DatetimeFieldPFN>

- [Stvaranje particioniranom tablice](https://msdn.microsoft.com/library/ms174979.aspx) (s) obzirom na podatkovnu shemu i navedite particiju shema i ograničenja polje koristi particije tablice, npr.:

        CREATE TABLE <table_name> ( [include schema definition here] )
        ON <TablePScheme>(<partition_field>)

Za dodatne informacije pogledajte [Stvaranje tablica particije i indekse](https://msdn.microsoft.com/library/ms188730.aspx).


## <a name="bulk-import-the-data-for-each-individual-partition-table"></a>Masovno uvoziti podatke za svaku tablicu pojedinačne particiju

- Možda koristite BCP, MASOVNO umetanje ili druge metode što [Čarobnjak za migraciju sustava SQL Server](http://sqlazuremw.codeplex.com/). Navedeni primjer koristi metodu BCP.

- [Alter baze podataka](https://msdn.microsoft.com/library/bb522682.aspx) da biste promijenili shemu zapisivanja transakcije BULK_LOGGED za minimiziranje indirektnog troška zapisivanje, npr.:

        ALTER DATABASE <database_name> SET RECOVERY BULK_LOGGED

- Da biste ubrzali učitavanja podataka, pokretanje masovne operacije uvoza paralelno. Savjete o expediting masovnog uvoz veliki podataka u SQL Server bazama podataka u odjeljku [učitati 1 TB manje od 1 sat](http://blogs.msdn.com/b/sqlcat/archive/2006/05/19/602142.aspx).

Primjer učitavanje pomoću BCP paralelne podataka je sljedeću skriptu PowerShell.

    # Set database name, input data directory, and output log directory
    # This example loads comma-separated input data files
    # The example assumes the partitioned data files are named as <base_file_name>_<partition_number>.csv
    # Assumes the input data files include a header line. Loading starts at line number 2.

    $dbname = "<database_name>"
    $indir  = "<path_to_data_files>"
    $logdir = "<path_to_log_directory>"

    # Select authentication mode
    $sqlauth = 0
    
    # For SQL authentication, set the server and user credentials
    $sqlusr = "<user@server>"
    $server = "<tcp:serverdns>"
    $pass   = "<password>"

    # Set number of partitions per table - Should match the number of input data files per table
    $numofparts = <number_of_partitions>
       
    # Set table name to be loaded, basename of input data files, input format file, and number of partitions
    $tbname = "<table_name>"
    $basename = "<base_input_data_filename_no_extension>"
    $fmtfile = "<full_path_to_format_file>"
   
    # Create log directory if it does not exist
    New-Item -ErrorAction Ignore -ItemType directory -Path $logdir
      
    # BCP example using Windows authentication
    $ScriptBlock1 = {
       param($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $num)
       bcp ($dbname + ".." + $tbname) in ($indir + "\" + $basename + "_" + $num + ".csv") -o ($logdir + "\" + $tbname + "_" + $num + ".txt") -h "TABLOCK" -F 2 -C "RAW" -f ($fmtfile) -T -b 2500 -t "," -r \n
    }
    
    # BCP example using SQL authentication
    $ScriptBlock2 = {
       param($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $num, $sqlusr, $server, $pass)
       bcp ($dbname + ".." + $tbname) in ($indir + "\" + $basename + "_" + $num + ".csv") -o ($logdir + "\" + $tbname + "_" + $num + ".txt") -h "TABLOCK" -F 2 -C "RAW" -f ($fmtfile) -U $sqlusr -S $server -P $pass -b 2500 -t "," -r \n
    }
    
    # Background processing of all partitions
    for ($i=1; $i -le $numofparts; $i++)
    {
       Write-Output "Submit loading trip and fare partitions # $i"
       if ($sqlauth -eq 0) {
          # Use Windows authentication
          Start-Job -ScriptBlock $ScriptBlock1 -Arg ($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $i)
       } 
       else {
          # Use SQL authentication
          Start-Job -ScriptBlock $ScriptBlock2 -Arg ($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $i, $sqlusr, $server, $pass)
       }
    }
    
    Get-Job
    
    # Optional - Wait till all jobs complete and report date and time
    date
    While (Get-Job -State "Running") { Start-Sleep 10 }
    date


## <a name="create-indexes-to-optimize-joins-and-query-performance"></a>Stvaranje indeksa za optimizaciju spojeva i performanse upita

- Ako će izdvojiti podatke za Modeliranje iz više tablica, stvorite indekse na spoj tipke da biste poboljšali performanse spoj.

- [Stvaranje indeksa](https://technet.microsoft.com/library/ms188783.aspx) (grupirani ili koji nisu grupirani) ciljanje istoj grupi datoteka za svaku particiju za npr.:

        CREATE CLUSTERED INDEX <table_idx> ON <table_name>( [include index columns here] )
        ON <TablePScheme>(<partition)field>)
ili,

        CREATE INDEX <table_idx> ON <table_name>( [include index columns here] )
        ON <TablePScheme>(<partition)field>)

 > [AZURE.NOTE] Možda odlučite stvoriti indekse prije masovnog uvoza podataka. Stvaranje indeksa prije masovnog uvoza će usporiti učitavanje podataka.


## <a name="advanced-analytics-process-and-technology-in-action-example"></a>Napredno Analytics proces i tehnologija u primjeru akcija

Na primjer vodič završetka završetka procesa Analytics Cortana pomoću javne dataset, pogledajte [Cortana Analytics proces u akciju: pomoću SQL Server](machine-learning-data-science-process-sql-walkthrough.md).
 
