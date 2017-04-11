<properties 
    pageTitle="Premještanje podataka sustava SQL Server na Azure virtualnog računala | Azure" 
    description="Premještanje podataka s nehijerarhijskom datoteke ili iz programa lokalnog sustava SQL Server u SQL Server na Azure VM." 
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
    ms.date="09/14/2016" 
    ms.author="bradsev" /> 

# <a name="move-data-to-sql-server-on-an-azure-virtual-machine"></a>Premještanje podataka sustava SQL Server na Azure virtualnog računala

U ovoj se temi navode mogućnosti za premještanje podataka iz paušalni datoteka (CSV ili TSV oblikovanja) ili iz sustava SQL Server na lokaciji SQL Server na Azure virtualnog računala. Ove zadatke za premještanje podataka s oblakom su dio postupka timu podataka Znanstvena.

Temu koja opisuje mogućnosti za premještanje podataka s bazom podataka SQL Azure za učenje za strojno, potražite u članku [Premještanje podataka s bazom podataka SQL Azure za Azure strojnog učenja](machine-learning-data-science-move-sql-azure.md).

Na **izborniku** ispod veze na temu koja opisuje kako ingest podataka u okruženju druge ciljne mjesto spremanja i obrađuju tijekom u timu podataka znanstvenog procesa (TDSP) podatke.

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]


U sljedećoj su tablici navedene su mogućnosti za premještanje podataka sustava SQL Server na Azure virtualnog računala.

<b>IZVOR</b> |<b>ODREDIŠTE: SQL Server na Azure VM</b> |
------------------ |-------------------- |
<b>Nehijerarhijskom datotekom</b> |1. <a href="#insert-tables-bcp">naredbeni redak masovno Kopiraj utility (BCP)</a><br> 2. <a href="#insert-tables-bulkquery">masovno Umetanje SQL upita</a><br> 3. <a href="#sql-builtin-utilities">grafički ugrađene uslužnih programa SQL Server</a>
<b>Lokalnog sustava SQL Server</b> | 1. <a href="#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard">uvođenja bazom podataka sustava SQL Server Microsoft Azure VM Čarobnjak za</a><br> 2. <a href="#export-flat-file">Izvoz s nehijerarhijskom datotekom</a><br> 3. <a href="#sql-migration">Čarobnjak za migraciju SQL baze podataka</a> <br> 4. <a href="#sql-backup">baze podataka sigurnosne kopije i vraćanje</a><br>

Imajte na umu da se ovaj dokument pretpostavlja da su SQL naredbe izvršava sa SQL Server Management Studio ili Visual Studio baze podataka programa Explorer.

> [AZURE.TIP] Umjesto toga, možete koristiti [Tvorničke Azure podataka](https://azure.microsoft.com/services/data-factory/) za stvaranje i planiranje kanal koji će premještanje podataka s VM SQL Server na Azure. Dodatne informacije potražite u članku [Kopiranje podataka s tvorničke Azure podataka (Kopiraj aktivnosti)](../data-factory/data-factory-data-movement-activities.md).


## <a name="prereqs"></a>Preduvjeti
Pomoću ovog praktičnog vodiča podrazumijeva da imate:

* Za **Azure pretplate**. Ako nemate pretplatu, možete se prijaviti za [besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/).
* **Račun za Azure prostora za pohranu**. Račun za Azure prostora za pohranu će se koristiti za spremanje podataka pomoću ovog praktičnog vodiča. Ako nemate račun za Azure prostora za pohranu, u članku [Stvaranje računa za pohranu](../storage/storage-create-storage-account.md#create-a-storage-account) . Kada stvorite račun za pohranu, morat ćete dobiti ključ za račun za pristup prostora za pohranu. Potražite u članku [Prikaz, Kopiraj i regenerate prostora za pohranu pristupnih tipki](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).
* Dodjela resursa **sustava SQL Server na Azure VM**. Upute potražite u članku [Postavljanje sustava SQL Server Azure virtualnog računala kao bilježnicu IPython poslužitelj za napredne analize](machine-learning-data-science-setup-sql-server-virtual-machine.md).
* Instalirana i konfiguriran **Azure PowerShell** lokalno. Upute potražite u članku [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md).


## <a name="filesource_to_sqlonazurevm"></a>Premještanje podataka s nehijerarhijskom datotekom izvora SQL Server na VM programa Azure

Ako se podaci nalaze u s nehijerarhijskom datotekom (raspoređeni u obliku retka i stupca), možete se premješta na SQL Server VM na Azure putem sljedećih načina:

1. [Naredbeni redak masovno Kopiraj utility (BCP)](#insert-tables-bcp) 
2. [Masovno Umetanje SQL upita](#insert-tables-bulkquery)
3. [Grafički uslužni programi ugrađene u sustavu SQL Server (Uvoz/izvoz, SSIS)](#sql-builtin-utilities)


### <a name="insert-tables-bcp"></a>Naredbeni redak masovno Kopiraj utility (BCP)

BCP je naredbenog retka utility instaliran u sustavu SQL Server i najbrže načina da biste premjestili sadržaj. Radi preko svih tri varijante SQL Server (lokalnog sustava SQL Server, SQL Azure i SQL Server VM na Azure). 

> [AZURE.NOTE]**Gdje treba Moji podaci biti za BCP?**  
> Dok nije potreban, imate datoteka koja sadrži izvorišne podatke koji se nalazi na istom računalu kao cilj SQL server omogućuje brže prijenosi (mreže brzinu Dodavanje veze za vanjskih na lokalnom disku IO brzina). Možete premjestiti paušalni datoteke koja sadrži podatke s računalom gdje je SQL Server instaliran pomoću razne datoteke kopiranje alate kao što su [AZCopy](../storage/storage-use-azcopy.md), [Explorer Azure prostora za pohranu](http://storageexplorer.com/) ili windows kopirajte i zalijepite putem Remote Desktop Protocol (RDP).

1. Provjerite je li se stvaraju na baze podataka SQL Server ciljne baze podataka i tablice. Evo primjera kako se te pomoću na `Create Database` i `Create Table` naredbe:

        CREATE DATABASE <database_name>
    
        CREATE TABLE <tablename>
        (
            <columnname1> <datatype> <constraint>,
            <columnname2> <datatype> <constraint>,
            <columnname3> <datatype> <constraint>
        ) 
    
2. Generiranje u oblik datoteke koji opisuje sheme za tablicu tako da izdavanja sljedeću naredbu iz naredbenog retka na računalu koje je instaliran bcp.

    `bcp dbname..tablename format nul -c -x -f exportformatfilename.xml -S servername\sqlinstance -T -t \t -r \n` 

3. Umetanje podataka u bazu podataka pomoću naredbe bcp na sljedeći način. To surađivati iz naredbenog retka uz pretpostavku da je na tom računalu instaliran SQL Server:

    `bcp dbname..tablename in datafilename.tsv -f exportformatfilename.xml -S servername\sqlinstancename -U username -P password -b block_size_to_move_in_single_attemp -t \t -r \n`

> **Optimiziranje BCP umeće** Pogledajte u sljedećem članku ["Smjernice za optimiziranje Masovni uvoz"](https://technet.microsoft.com/library/ms177445%28v=sql.105%29.aspx) da biste optimizirali takve umeće.


### <a name="insert-tables-bulkquery-parallel"></a>Parallelizing umeće za brže premještanje podataka

Ako su podaci koje premještate velik, možete ubrzati stvari izvršavanjem istodobno više naredbi BCP paralelno skriptu PowerShell.

> [AZURE.NOTE]**Velikih skupova podataka Ingestion** Da biste optimizirali podataka učitavanje velikih i velikih skupova podataka, particija tablica logičke i fizičkih baze podataka pomoću više tablica filegroups i particija. Dodatne informacije o stvaranju i učitavanja podataka u particije tablice potražite u članku [Paralelno učitavanja SQL particija tablice](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).


Ispod skriptu PowerShell uzorka demonstrirati paralelno umeće pomoću bcp:
    
    $NO_OF_PARALLEL_JOBS=2

     Set-ExecutionPolicy RemoteSigned #set execution policy for the script to execute
     # Define what each job does
       $ScriptBlock = {
           param($partitionnumber)

           #Explictly using SQL username password
           bcp database..tablename in datafile_path.csv -F 2 -f format_file_path.xml -U username@servername -S tcp:servername -P password -b block_size_to_move_in_single_attempt -t "," -r \n -o path_to_outputfile.$partitionnumber.txt

            #Trusted connection w.o username password (if you are using windows auth and are signed in with that credentials)
            #bcp database..tablename in datafile_path.csv -o path_to_outputfile.$partitionnumber.txt -h "TABLOCK" -F 2 -f format_file_path.xml  -T -b block_size_to_move_in_single_attempt -t "," -r \n 
      }
    

    # Background processing of all partitions
    for ($i=1; $i -le $NO_OF_PARALLEL_JOBS; $i++)
    {
      Write-Debug "Submit loading partition # $i"
      Start-Job $ScriptBlock -Arg $i      
    }
    

    # Wait for it all to complete
    While (Get-Job -State "Running")
    {
      Start-Sleep 10
      Get-Job
    }
    
    # Getting the information back from the jobs
    Get-Job | Receive-Job
    Set-ExecutionPolicy Restricted #reset the execution policy


### <a name="insert-tables-bulkquery"></a>Masovno Umetanje SQL upita

[Masovno Umetanje SQL upita](https://msdn.microsoft.com/library/ms188365) može se koristiti za uvoz podataka u bazu podataka iz redaka/stupaca koji se temelje datoteka (podržane vrste su obuhvaćeno[Priprema podataka za skupno izvoz i uvoz (SQL Server)](https://msdn.microsoft.com/library/ms188609)) temu. 

Slijede neki primjeri naredbi za masovno Umetanje su kao ispod:  

1. Analiza podataka i postavite željene prilagođene mogućnosti prije uvoza da biste bili sigurni da baze podataka SQL Server pretpostavlja isti oblik posebno poljima kao što su datumi. Evo primjera kako postaviti oblik datuma kao godina, mjesec i dan (Ako se vaši podaci sadrže datum u obliku godina, mjesec i dan):

        SET DATEFORMAT ymd; 
    
2. Uvoz podataka putem skupnog uvoza naredbe:

        BULK INSERT <tablename>
        FROM    
        '<datafilename>'
        WITH 
        (
        FirstRow=2,
        FIELDTERMINATOR =',', --this should be column separator in your data
        ROWTERMINATOR ='\n'   --this should be the row separator in your data
        )
      

### <a name="sql-builtin-utilities"></a>Ugrađeni uslužnih programa SQL Server

SQL Server integracije Services (SSIS) možete koristiti da biste uvezli podatke u SQL Server VM na Azure s nehijerarhijskom datotekom. SSIS dostupan je u tim okruženjima studio. Detalje potražite u članku [integraciju servisa (SSIS) i Studio okruženja](https://technet.microsoft.com/library/ms140028.aspx):

- Detalje o SQL Server Data Tools potražite u članku [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/tools.aspx)  
- Informacije o čarobnjaku za uvoz/izvoz potražite u članku [Uvoz SQL Server i čarobnjaka za izvoz](https://msdn.microsoft.com/library/ms141209.aspx)


## <a name="sqlonprem_to_sqlonazurevm"></a>Premještanje podataka iz lokalnog sustava SQL Server u SQL Server na Azure VM

Možete koristiti sljedeće Strategije migracije:

1. [Čarobnjak za Microsoft Azure VM implementacija bazi podataka sustava SQL Server](#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard)
2. [Izvoz Nehijerarhijskom datotekom](#export-flat-file) 
3. [Čarobnjak za migraciju SQL baze podataka](#sql-migration)
4. [Baze podataka sigurnosne kopije i vraćanje](#sql-backup)

Ne možemo opišite svaki od ovih ispod:

### <a name="deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard"></a>Čarobnjak za Microsoft Azure VM implementacija bazi podataka sustava SQL Server

**Uvođenje bazom podataka sustava SQL Server Microsoft Azure VM Čarobnjak za** je jednostavno i preporučeni način za premještanje podataka iz instancu sustava SQL Server lokalnog sustava SQL Server na VM programa Azure. Detaljne upute kao i rasprave druge alternativnih, potražite u članku [Migracija baze podataka SQL Server na programa Azure VM](../virtual-machines/virtual-machines-windows-migrate-sql.md).

### <a name="export-flat-file"></a>Izvoz Nehijerarhijskom datotekom

Različite metode može se koristiti za skupno izvoz podataka iz sustava SQL Server na lokalni kao navedenih u temi [Masovni uvoz i izvoz od podataka (SQL Server)](https://msdn.microsoft.com/library/ms175937.aspx) . Ovaj dokument obrađuje skupno Kopiraj Program (BCP) kao primjer. Nakon izvoza podataka u s nehijerarhijskom datotekom, moguće je uvesti u drugu SQL server pomoću Masovni uvoz. 

1. Izvoz podataka iz lokalnog sustava SQL Server u datoteku pomoću uslužni bcp na sljedeći način

    `bcp dbname..tablename out datafile.tsv -S  servername\sqlinstancename -T -t \t -t \n -c`

2. Stvaranje baze podataka i tablicu na SQL Server VM Azure korištenjem na `create database` i `create table` shema tablice izvozi u koraku 1.

3. Stvorite oblik datoteke s opisom shemu tablica podataka nije moguće izvesti/uvoza. Detalje o datoteke u obliku opisana su u [Stvaranje oblik datoteke (SQL Server)](https://msdn.microsoft.com/library/ms191516.aspx).

    Oblik datoteke generacije kada radi BCP s računala sustava SQL Server 

        bcp dbname..tablename format nul -c -x -f exportformatfilename.xml -S servername\sqlinstance -T -t \t -r \n 

    Oblik datoteke generacije kada daljinski radi BCP protiv SQL Server 

        bcp dbname..tablename format nul -c -x -f  exportformatfilename.xml  -U username@servername.database.windows.net -S tcp:servername -P password  --t \t -r \n
        
    
4. Koristite neku od metoda opisan u odjeljku [Premještanje podataka iz izvora datoteke](#filesource_to_sqlonazurevm) da biste premjestili podatke u plošnu datotekama sustava SQL Server.

### <a name="sql-migration"></a>Čarobnjak za migraciju SQL baze podataka

[Čarobnjak za migraciju baze podataka SQL Server](http://sqlazuremw.codeplex.com/) omogućuje jasan premještanje podataka s dvije instance sustava SQL server. Omogućuje korisniku da biste mapirali shemi podataka između izvora i odredišne tablice, odaberite vrste stupaca i razne radovi. Koristi skupno Kopiraj (BCP) u odjeljku s naslovnice. Snimka zaslona zaslon dobrodošlice za korištenje čarobnjaka za migraciju baze podataka SQL prikazano u nastavku.  

![Čarobnjak za migraciju SQL Server][2]

### <a name="sql-backup"></a>Baze podataka sigurnosne kopije i vraćanje

SQL Server podržava: 

1. [Baze podataka sigurnosne kopije i vratili funkcije](https://msdn.microsoft.com/library/ms187048.aspx) (i za lokalne datoteke ili bacpac Izvoz u blob) i [Aplikacije sloju podatke](https://msdn.microsoft.com/library/ee210546.aspx) (pomoću bacpac). 
2. Mogućnost da biste stvorili izravno SQL Server VMs na Azure s kopirane baze podataka ili kopiranje postojećoj bazi podataka SQL Azure. Dodatne informacije potražite u članku [Korištenje čarobnjaka za kopiju baze podataka](https://msdn.microsoft.com/library/ms188664.aspx). 

Snimka zaslona nazad baza podataka/vraćanje mogućnosti iz sustava SQL Server Management Studio je prikazano u nastavku.

![Alat za uvoz sustava SQL Server][1]

## <a name="resources"></a>Resursi

[Migracija baze podataka SQL Server na Azure VM](../virtual-machines/virtual-machines-windows-migrate-sql.md)

[SQL Server na virtualnim računalima sustava Azure pregled](../virtual-machines/virtual-machines-windows-sql-server-iaas-overview.md)

[1]: ./media/machine-learning-data-science-move-sql-server-virtual-machine/sqlserver_builtin_utilities.png
[2]: ./media/machine-learning-data-science-move-sql-server-virtual-machine/database_migration_wizard.png
