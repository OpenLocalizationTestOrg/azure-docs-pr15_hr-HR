<properties
    pageTitle="Konfiguriranje straža podataka tvrtke Oracle u VMs | Microsoft Azure"
    description="Prođite vodič za postavljanje i implementaciju straža podataka tvrtke Oracle na Azure virtualnim strojevima visoke dostupnosti i Izrada oporavka."
    services="virtual-machines-windows"
    authors="rickstercdn"
    manager="timlt"
    documentationCenter=""
    tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="infrastructure-services"
    ms.date="09/06/2016"
    ms.author="rclaus" />

#<a name="configuring-oracle-data-guard-for-azure"></a>Konfiguriranje straža podataka tvrtke Oracle za Azure


Pomoću ovog praktičnog vodiča pokazuje kako postaviti i implementirati straža podataka tvrtke Oracle u okruženju virtualnim računalima sustava Azure visoke dostupnosti i Izrada oporavak. Vodič usredotočuje se na jednosmjerna replikacije za baze podataka koje nisu iz programa RAC Oracle.

Straža podataka tvrtke Oracle podržava zaštita podataka i oporavak Izrada za bazu podataka tvrtke Oracle. Je jednostavno, visokih performansi, Upadajući rješenja za oporavak Izrada, zaštita podataka i visoke dostupnosti cijelu bazu podataka tvrtke Oracle.

Pomoću ovog praktičnog vodiča pretpostavlja da ste već theoretical i praktične znanja na visoke dostupnosti Oracle baze podataka i oporavak Izrada koncepata. Informacije potražite u članku [Oracle web-mjesta](http://www.oracle.com/technetwork/database/features/availability/index.html) i na [Vodič za administraciju i Oracle podataka straža koncepata](https://docs.oracle.com/cd/E11882_01/server.112/e41134/toc.htm).

Osim toga, vodič pretpostavlja već implementirana sljedeće preduvjete:

- Već ste pregledali visoke dostupnosti i Izrada oporavak pitanja vezana uz odjeljak u temi [Oracle virtualnog računala slike - razne pitanja vezana uz](virtual-machines-windows-classic-oracle-considerations.md) . Azure podržava instanci baze podataka tvrtke Oracle samostalni, ali ne Oracle realni aplikacije klastere (Oracle RAC) trenutno.


- Stvorite dva virtualnim strojevima (VMs) u Azure pomoću isti platformu dobili slika Oracle Enterprise Edition. Provjerite jesu li virtualnim strojevima u [istom servis u oblaku](virtual-machines-windows-load-balance.md) i u istom virtualne mreže da biste bili sigurni međusobno mogu pristupiti putem stalni privatne IP adrese. Osim toga, preporučuje se da biste postavili na VMs u istom [dostupnost postavljanje](virtual-machines-windows-manage-availability.md) da biste omogućili Azure smjestiti u zasebnom kvara domene i nadogradnje domene. Straža podataka tvrtke Oracle dostupna samo s Oracle baze podataka Enterprise Edition. Svakom računalu morate imati barem 2 GB memorije i 5 GB prostora na tvrdom disku. Najnovije informacije na platformi dobili VM veličine, potražite u članku [Veličine virtualnog računala za Azure](virtual-machines-windows-sizes.md). Ako vam je potrebna dodatni diskovni jedinica za vaše VMs, možete priložiti dodatnih diskova. Informacije potražite u članku [kako priložiti podatkovni Disk na virtualnog računala](virtual-machines-windows-classic-attach-disk.md).



- Postavite nazive virtualnog računala kao "Machine1" za primarni VM i "Machine2" za čekanja VM na portalu za Azure klasični.

- Postavite varijablu okruženja **ORACLE_HOME** da upućuju na isti oracle korijenski put instalacije u primarni i čekanja virtualnih računala, kao što su `C:\OracleDatabase\product\11.2.0\dbhome_1\database`.

- Prijave u Windows server kao član grupe **administratora** ili član grupe **ORA_DBA** .

U ovom ćete praktičnom vodiču ćete:

Implementacija okruženje za fizičke čekanja baze podataka

1. Stvaranje primarnog baze podataka

2. Priprema primarnom bazom podataka za stvaranje stanja čekanja baze podataka

    1. Omogućite zapisivanje prisilne

    2. Stvaranje datoteke za lozinke

    3. Konfiguriranje zapisnik čekanja Ponovi poništeno

    4. Omogućivanje arhiviranja

    5. Postavljanje parametara za inicijalizaciju primarnom bazom podataka

Stvaranje fizičke čekanja baze podataka

1. Priprema datoteku parametar Inicijalizacija čekanja baze podataka

2. Konfiguriranje ga slušatelj i tnsnames podržava baze podataka na primarnih i čekanja

    1. Konfiguriranje listener.ora na poslužiteljima na čuvanje stavke za oba baze podataka

    2. Držanje stavke za primarnih i čekanja baze podataka, konfigurirati tnsnames.ora na virtualnim strojevima primarnih i čekanja. 

    3. Pokrenite ga slušatelj i provjerite tnsping na oba virtualnim računalima sustava oba servisa.

3. Pokretanje instancu čekanja u stanju nomount

4. Pomoću RMAN Kloniraj baze podataka i stvaranje stanja čekanja baze podataka

5. Pokretanje fizičke čekanja baze podataka u načinu rada za oporavak upravljanih

6. Provjerite je li fizička čekanja baze podataka

> [AZURE.IMPORTANT] Pomoću ovog praktičnog vodiča postavljanje je i testirati u odnosu na sljedeću konfiguraciju hardvera i softvera:
>
>|                      | **Primarni baze podataka**                      | **Čekanja baze podataka**                      |
>|----------------------|-------------------------------------------|-------------------------------------------|
>| **Oracle izdanje**   | Izdanje Enterprise Oracle11g (11.2.0.4.0) | Izdanje Enterprise Oracle11g (11.2.0.4.0) |
>| **Naziv računala**     | Machine1                                  | Machine2                                  |
>| **Operacijski sustav** | Windows 2008 R2                           | Windows 2008 R2                           |
>| **Oracle SID**       | TEST                                      | TEST\_STBY                                |
>| **Memorije**           | Min 2 GB                                  | Min 2 GB                                  |
>| **Prostor na disku**       | Min 5 GB                                  | Min 5 GB                                  |

Za sljedeće izdanja baze podataka tvrtke Oracle i straža podataka tvrtke Oracle možda postoje neka dodatne promjene koje su vam potrebne za implementaciju. Najnovije informacije specifične za verzija, potražite u članku [Straža podataka](http://www.oracle.com/technetwork/database/features/availability/data-guard-documentation-152848.html) i [Podataka tvrtke Oracle](http://www.oracle.com/us/corporate/features/database-12c/index.html) dokumentaciju Oracle web-mjesta.

##<a name="implement-the-physical-standby-database-environment"></a>Implementacija okruženje za fizičke čekanja baze podataka
### <a name="1-create-a-primary-database"></a>1. stvaranje primarnog baze podataka

- Stvaranje primarnom bazom podataka "TESTIRANJE" u primarni virtualnog računala. Informacije potražite u članku Stvaranje i konfiguriranje bazom podataka tvrtke Oracle.
- Da biste vidjeli naziv baze podataka, povezivanje s bazom podataka kao korisnik Sistemskim s SYSDBA uloga u SQL * Plus naredbeni redak i Pokreni sljedeću naredbu:

        SQL> select name from v$database;

        The result will display like the following:

        NAME
        ---------
        TEST
- Nakon toga upita naziva datoteke baze podataka iz prikaza dba_data_files sustava:

        SQL> select file_name from dba_data_files;
        FILE_NAME
        -------------------------------------------------------------------------------
        C:\ <YourLocalFolder>\TEST\USERS01.DBF
        C:\ <YourLocalFolder>\TEST\UNDOTBS01.DBF
        C:\ <YourLocalFolder>\TEST\SYSAUX01.DBF
        C:\<YourLocalFolder>\TEST\SYSTEM01.DBF
        C:\<YourLocalFolder>\TEST\EXAMPLE01.DBF

### <a name="2-prepare-the-primary-database-for-standby-database-creation"></a>2. Priprema primarnom bazom podataka za stvaranje stanja čekanja baze podataka

Prije stvaranja čekanja baze podataka, preporučuje se provjeriti ispravno konfiguriran primarnom bazom podataka. Slijedi popis korake koje su vam potrebne za izvođenje:

1. Omogućite zapisivanje prisilnog
2. Stvaranje datoteke za lozinke
3. Konfiguriranje zapisnik čekanja Ponovi poništeno
4. Omogućivanje arhiviranja
5. Postavljanje parametara za inicijalizaciju primarnom bazom podataka

#### <a name="enable-forced-logging"></a>Omogućite zapisivanje prisilne

Možete implementirati čekanja baze podataka, moramo omogućiti prisilno zapisivanje u primarnom bazom podataka. Ta mogućnost osigurava da čak i ako se završi postupak 'nologging', prisilno zapisivanje ima prednost i sve operacije ste prijavljeni putem zapisnike Ponovi poništeno. Stoga dajemo li sve u primarnom bazom podataka prijavljen je i replikacije da biste na čekanja uključuje sve operacije u primarnom bazom podataka. Da biste omogućili prisilno zapisivanje, pokrenite iskaz alter baze podataka:

    SQL> ALTER DATABASE FORCE LOGGING;

    Database altered.

#### <a name="create-a-password-file"></a>Stvaranje datoteke za lozinke

Da biste mogli isporuka i primjenu arhivirane zapisnika s poslužitelja primarni poslužitelj čekanja, lozinku sistemskim moraju biti jednake na poslužiteljima primarnih i čekanja. Zašto je stvoriti datoteku lozinke na primarnom bazom podataka i njegovo kopiranje čekanja poslužitelja.

>[AZURE.IMPORTANT] Kada koristite baze podataka tvrtke Oracle 12c, postoji novi korisnik **SYSDG**, koje možete koristiti za administriranje straža podataka tvrtke Oracle. Dodatne informacije potražite u članku [promjene u bazi podataka tvrtke Oracle 12 c izdanje](http://docs.oracle.com/database/121/UNXAR/release_changes.htm#UNXAR404).

Osim toga, provjerite je li u ORACLE\_POLAZNO okruženje već definiran u Machine1. Ako nije, definirajte ga kao varijabla okruženja pomoću dijaloškog okvira varijable okruženja. Da biste pristupili taj dijaloški okvir, pokrenuli uslužni **sustava** tako da dvokliknete ikonu sustav na **Upravljačkoj ploči**; Kliknite karticu **Dodatno** pa odaberite **Varijable okruženja**. Da biste postavili varijable okruženja, kliknite gumb **Novo** u odjeljku **Sustav varijabli**. Nakon postavljanja varijable okruženja, zatvorite postojeće naredbenog retka za Windows i otvorite novi.

Pokrenite sljedeću naredbu da biste se prebacili na Oracle\_imenika Polazno, kao što je C:\\OracleDatabase\\proizvoda\\11.2.0\\dbhome\_1\\baze podataka.

    cd %ORACLE_HOME%\database

Zatim stvorite lozinku datoteku pomoću lozinke datoteke stvaranja uslužni, [ORAPWD](http://docs.oracle.com/cd/B28359_01/server.111/b28310/dba007.htm). U istoj Windows naredbeni redak u Machine1 postavljanjem vrijednost lozinke kao lozinka **Sistemskim**pokrenite sljedeću naredbu:

    ORAPWD FILE=PWDTEST.ora PASSWORD=password FORCE=y

Ta naredba stvara datoteku lozinku, pod nazivom kao PWDTEST.ora u na ORACLE\_HOME\\imenik baze podataka. Kopirajte ovu datoteku na % ORACLE\_POLAZNO %\\baze podataka direktoriju Machine2 ručno.

#### <a name="configure-a-standby-redo-log"></a>Konfiguriranje zapisnik čekanja Ponovi poništeno

Nakon toga morate konfigurirati čekanja ponavljanje zapisnika tako da primarni pravilno mogu primati Naredba Ponovi poništeno kada postane u stanju. Unaprijed stvaranjem ovdje omogućuje zapisnike čekanja Ponovi poništeno da biste se automatski stvoriti u stanju. Važno je da konfiguriranje na čekanja ponavljanje zapisnika (SRL) uz iste veličine kao elektroničke ponavljanje zapisnika. Veličina datoteke zapisnika sustava trenutnog stanja čekanja Ponovi poništeno mora u potpunosti odgovarati veličina trenutne datoteke zapisnika online Ponovi poništeno primarnom bazom podataka.

Pokrenite sljedeću naredbu u SQL\*PLUS naredbeni redak u Machine1. Datoteke zapisnika $ v je prikaz sustav koji sadrži informacije o Ponovi poništeno datoteke zapisnika.

    SQL> select * from v$logfile;
    GROUP# STATUS  TYPE    MEMBER                                                       IS_
    ---------- ------- ------- ------------------------------------------------------------ ---
    3         ONLINE  C:\<YourLocalFolder>\TEST\REDO03.LOG               NO
    2         ONLINE  C:\<YourLocalFolder>\TEST\REDO02.LOG               NO
    1         ONLINE  C:\<YourLocalFolder>\TEST\REDO01.LOG               NO
    Next, query the v$log system view, displays log file information from the control file.
    SQL> select bytes from v$log;
    BYTES
    ----------
    52428800
    52428800
    52428800


Imajte na umu da 52428800 50 megabajta.

Zatim u SQL\*Plus prozor, pokrenite sljedeće naredbe da biste dodali novu grupu datoteka zapisnika čekanja Ponovi poništeno čekanja baze podataka i navedite broj koji označava grupe korištenjem uvjeta GRUPE. Korištenje grupe brojeva možete napraviti administriranje čekanja Ponovi poništeno zapisnika datoteke grupe jednostavnije:

    SQL> ALTER DATABASE ADD STANDBY LOGFILE GROUP 4 'C:\<YourLocalFolder>\TEST\REDO04.LOG' SIZE 50M;
    Database altered.
    SQL> ALTER DATABASE ADD STANDBY LOGFILE GROUP 5 'C:\<YourLocalFolder>\TEST\REDO05.LOG' SIZE 50M;
    Database altered.
    SQL> ALTER DATABASE ADD STANDBY LOGFILE GROUP 6 'C:\<YourLocalFolder>\TEST\REDO06.LOG' SIZE 50M;
    Database altered.

Nakon toga pokrenite sljedeće sistemski prikaz popisa informacija o Ponovi poništeno datoteke zapisnika. Ovaj postupak provjerava i grupa za datoteka zapisnika čekanja Ponovi poništeno stvorene:

    SQL> select * from v$logfile;
    GROUP# STATUS  TYPE MEMBER IS_
    ---------- ------- ------- --------------------------------------------- ---
    3         ONLINE C:\<YourLocalFolder>\TEST\REDO03.LOG NO
    2         ONLINE C:\<YourLocalFolder>\TEST\REDO02.LOG NO
    1         ONLINE C:\<YourLocalFolder>\TEST\REDO01.LOG NO
    4         STANDBY C:\<YourLocalFolder>\TEST\REDO04.LOG
    5         STANDBY C:\<YourLocalFolder>\TEST\REDO05.LOG NO
    6         STANDBY C:\<YourLocalFolder>\TEST\REDO06.LOG NO
    6 rows selected.

#### <a name="enable-archiving"></a>Omogućivanje arhiviranja

Nakon toga omogućite arhiviranje ponovnim pokretanjem sljedeće naredbe da biste stavili primarni baze podataka u načinu ARCHIVELOG pa Omogući automatsko arhiviranje. Postavljanje baze podataka i izvodi naredbu archivelog možete omogućiti način zapisnika arhiva.

Prvo, prijavite se kao sysdba. U naredbeni redak Windows pokrenite:

    sqlplus /nolog

    connect / as sysdba

Nakon toga zatvaranja baze podataka u SQL\*Plus naredbeni redak:

    SQL> shutdown immediate;
    Database closed.
    Database dismounted.
    ORACLE instance shut down.

Nakon toga izvršiti naredbu za pokretanje postavljanja postavljanja baze podataka. Na taj način Oracle pridružuje instanci navedenoj bazi podataka.

    SQL> startup mount;
    ORACLE instance started.
    Total System Global Area 1503199232 bytes
    Fixed Size                  2281416 bytes
    Variable Size             922746936 bytes
    Database Buffers          570425344 bytes
    Redo Buffers                7745536 bytes
    Database mounted.

Nakon toga pokrenite:

    SQL> alter database archivelog;
    Database altered.

Nakon toga pokrenite u Alter izjava baze podataka s Otvori uvjet za bazu podataka možete učiniti dostupnim za normalni:

    SQL> alter database open;

    Database altered.

#### <a name="set-primary-database-initialization-parameters"></a>Postavljanje parametara za inicijalizaciju primarnom bazom podataka

Da biste konfigurirali straža podataka, morate stvoriti i konfigurirati čekanja parametre na obični pfile (tekst Inicijalizacija parametar datoteka) prvi. Kada se pfile spreman, morate ga pretvorili u datoteci parametar poslužitelja (SPFILE).

Možete kontrolirati okruženje straža podataka pomoću parametara u u INIT. Datoteka ORA. Kada slijedite ovaj Praktični vodič, morate je ažuriranje primarnog INIT baze podataka. ORA tako da mogu sadržavati i uloge: primarnog ili čekanja.

    SQL> create pfile from spfile;
    File created.

Ćete morati urediti pfile da biste dodali čekanja parametre. Da biste to učinili, otvorite na INITTEST. ORA datoteke na mjestu % ORACLE\_POLAZNO %\\baze podataka. Nakon toga dodati sljedeće naredbe INITTEST.ora datoteku. Konvencije imenovanja vaše INIT. Datoteka ORA je INIT\<YourDatabaseName\>. ORA.

    db_name='TEST'
    db_unique_name='TEST'
    LOG_ARCHIVE_CONFIG='DG_CONFIG=(TEST,TEST_STBY)'
    LOG_ARCHIVE_DEST_1= 'LOCATION=C:\OracleDatabase\archive   VALID_FOR=(ALL_LOGFILES,ALL_ROLES) DB_UNIQUE_NAME=TEST'
    LOG_ARCHIVE_DEST_2= 'SERVICE=TEST_STBY LGWR ASYNC VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE) DB_UNIQUE_NAME=TEST_STBY'
    LOG_ARCHIVE_DEST_STATE_1=ENABLE
    LOG_ARCHIVE_DEST_STATE_2=ENABLE
    REMOTE_LOGIN_PASSWORDFILE=EXCLUSIVE
    LOG_ARCHIVE_FORMAT=%t_%s_%r.arc
    LOG_ARCHIVE_MAX_PROCESSES=30
    # Standby role parameters --------------------------------------------------------------------
    fal_server=TEST_STBY
    fal_client=TEST
    standby_file_management=auto
    db_file_name_convert='TEST_STBY','TEST'
    log_file_name_convert='TEST_STBY','TEST'
    # ---------------------------------------------------------------------------------------------


Prethodnog bloka izjava obuhvaća tri postavljanje važne stavke:
-   **LOG_ARCHIVE_CONFIG...:** Definiranje ID-a jedinstvene baze podataka pomoću izjavu o zaštiti.
-   **LOG_ARCHIVE_DEST_1...:** Definiranje lokalne mape arhivu pomoću izjavu o zaštiti. Preporučujemo stvorite novi direktorij arhiviranja potrebama svoje baze podataka i navedite u lokalnu arhivu pomoću izjavu o zaštiti izričito umjesto korištenja Oracle na zadanu mapu % ORACLE_HOME%\database\archive.
-   **LOG_ARCHIVE_DEST_2... LGWR ASINKRONE...:** definirate writer proces asinkronog zapisnika (LGWR) da biste prikupili podatke za Ponovi poništeno transakcije i prenositi čekanja odredišta. Ovdje, na DB_UNIQUE_NAME određuje jedinstveni naziv za bazu podataka na odredišnom poslužitelju čekanja.

Kada novu datoteku parametar spreman, potrebnih za stvaranje spfile iz nje.

Prvo, zatvaranja baze podataka:

    SQL> shutdown immediate;

    Database closed.

    Database dismounted.

    ORACLE instance shut down.

Nakon toga pokretanje nomount naredbu pokrenuti na sljedeći način:

    SQL> startup nomount pfile='c:\OracleDatabase\product\11.2.0\dbhome_1\database\initTEST.ora';
    ORACLE instance started.
    Total System Global Area 1503199232 bytes
    Fixed Size                  2281416 bytes
    Variable Size             922746936 bytes
    Database Buffers          570425344 bytes
    Redo Buffers                7745536 bytes

Stvorite spfile:

    SQL>create spfile frompfile='c:\OracleDatabase\product\11.2.0\dbhome\_1\database\initTEST.ora';

    File created.

Nakon toga zatvaranja baze podataka:

    SQL> shutdown immediate;

    ORA-01507: database not mounted

Nakon toga koristite naredbu pokretanje da biste pokrenuli instance:

    SQL> startup;
    ORACLE instance started.
    Total System Global Area 1503199232 bytes
    Fixed Size                  2281416 bytes
    Variable Size             922746936 bytes
    Database Buffers          570425344 bytes
    Redo Buffers                7745536 bytes
    Database mounted.
    Database opened.

##<a name="create-a-physical-standby-database"></a>Stvaranje fizičke čekanja baze podataka
U ovom se odjeljku usredotočuje se na koraci koje morate izvršiti u Machine2 da biste pripremili fizičke čekanja baze podataka.

Najprije morate udaljene radne površine da biste Machine2 putem portala za Azure klasični.

Nakon toga na čekanja poslužitelju (Machine2), stvorite sve potrebne mape za čekanja bazu podataka, kao što su C:\\\<YourLocalFolder\>\\TEST. Tijekom pratite ovaj Praktični vodič, provjerite odgovara li struktura mape strukturi mape Machine1 da biste zadržali sve potrebne datoteke, kao što su datoteke controlfile, datafiles, redologfiles, udump, bdump i cdump li. Uz to, definiranje u ORACLE\_HOME and ORACLE\_varijable OSNOVNI okruženja u Machine2. Ako nije, ih definirati kao varijabla okruženja pomoću dijaloškog okvira varijable okruženja. Da biste pristupili taj dijaloški okvir, pokrenuli uslužni **sustava** tako da dvokliknete ikonu sustav na **Upravljačkoj ploči**; Kliknite karticu **Dodatno** pa odaberite **Varijable okruženja**. Da biste postavili varijable okruženja, kliknite gumb **Novo** u odjeljku **Sustav varijabli**. Nakon postavljanja varijable okruženja, morate zatvoriti postojeće naredbenog retka za Windows i otvorite novi da biste vidjeli promjene.

Nakon toga slijedite ove korake:

1. Priprema datoteku parametar Inicijalizacija čekanja baze podataka

2. Konfiguriranje ga slušatelj i tnsnames podržava baze podataka na primarnih i čekanja

    1. Konfiguriranje listener.ora na poslužiteljima na čuvanje stavke za oba baze podataka

    2. Konfiguriranje tnsnames.ora na virtualnim strojevima primarnih i čekanja držati stavke za primarnih i čekanja baze podataka

    3. Pokrenite ga slušatelj i provjerite tnsping na oba virtualnim računalima sustava oba servisa.

3. Pokretanje instancu čekanja u stanju nomount

4. Pomoću RMAN Kloniraj baze podataka i stvaranje stanja čekanja baze podataka

5. Pokretanje fizičke čekanja baze podataka u načinu rada za oporavak upravljanih

6. Provjerite je li fizička čekanja baze podataka

### <a name="1-prepare-an-initialization-parameter-file-for-standby-database"></a>1. na Inicijalizacija parametar datoteku pripremiti za čekanja baze podataka

U ovom se odjeljku objašnjavaju kako pripremiti datoteku parametar Inicijalizacija čekanja baze podataka. Da biste to učinili, kopirajte na INITTEST. ORA datoteke od 1 stroj Machine2 ručno. Trebali biste moći vidjeti na INITTEST. ORA datoteka u % ORACLE\_POLAZNO %\\mapa baze podataka u oba računala. Zatim izmijenite datoteku INITTEST.ora u Machine2 da biste postavili za ulogu čekanja kao što je navedeno ispod:

    db_name='TEST'
    db_unique_name='TEST_STBY'
    db_create_file_dest='c:\OracleDatabase\oradata\test_stby’
    db_file_name_convert=’TEST’,’TEST_STBY’
    log_file_name_convert='TEST','TEST_STBY'


    job_queue_processes=10
    LOG_ARCHIVE_CONFIG='DG_CONFIG=(TEST,TEST_STBY)'
    LOG_ARCHIVE_DEST_1='LOCATION=c:\OracleDatabase\TEST_STBY\archives VALID_FOR=(ALL_LOGFILES,ALL_ROLES) DB_UNIQUE_NAME=’TEST'
    LOG_ARCHIVE_DEST_2='SERVICE=TEST LGWR ASYNC VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE)
    LOG_ARCHIVE_DEST_STATE_1='ENABLE'
    LOG_ARCHIVE_DEST_STATE_2='ENABLE'
    LOG_ARCHIVE_FORMAT='%t_%s_%r.arc'
    LOG_ARCHIVE_MAX_PROCESSES=30


Prethodnog bloka izjava sadrži dvije postavljanje važne stavke:

-   **\*. LOG_ARCHIVE_DEST_1:** ćete morati ručno stvoriti mapu c:\OracleDatabase\TEST_STBY\archives Machine2.
-   **\*. LOG_ARCHIVE_DEST_2:** ovaj korak nije obavezan. Postavite to je možda potrebno kada je primarni računalo u održavanje, a na računalu čekanja postane primarnom bazom podataka.

Zatim, morate početi čekanja instance. Na poslužitelju čekanja baze podataka unesite sljedeću naredbu u naredbenom retku Windows da biste stvorili instancu Oracle stvaranjem servis Windows:

    oradim -NEW -SID TEST\_STBY -STARTMODE MANUAL

Naredba **Oradim** stvara instancu Oracle, ali ne pokreće. Možete pronaći u se C:\\OracleDatabase\\proizvoda\\11.2.0\\dbhome\_1\\direktorija za SMEĆE.

##<a name="configure-the-listener-and-tnsnames-to-support-the-database-on-primary-and-standby-machines"></a>Konfiguriranje ga slušatelj i tnsnames podržava baze podataka na primarnih i čekanja
Prije stvaranja čekanja baze podataka, morate da biste provjerili možete li baza podataka primarnih i čekanja u vašoj konfiguraciji razgovarati s međusobno povezani. Da biste to učinili, morate konfigurirati ga slušatelj i TNSNames ručno ili pomoću uslužni program za konfiguriranje mreže NETCA. Ovo je obavezna zadatak kada koristite Upravitelj za oporavak utility (RMAN).

### <a name="configure-listenerora-on-both-servers-to-hold-entries-for-both-databases"></a>Konfiguriranje listener.ora na poslužiteljima na čuvanje stavke za oba baze podataka

Udaljena radna površina Machine1 i uređivanje datoteke listener.ora kao navedeno u nastavku. Prilikom uređivanja datoteka listener.ora uvijek provjerite da otvorene i zatvorene zagrade posložite u istom stupcu. Listener.ora datoteku možete pronaći u sljedećoj mapi: c:\\OracleDatabase\\proizvoda\\11.2.0\\dbhome\_1\\MREŽNI\\ADMINISTRATOR\\.

    # listener.ora Network Configuration File: C:\OracleDatabase\product\11.2.0\dbhome_1\network\admin\listener.ora

    # Generated by Oracle configuration tools.

    SID_LIST_LISTENER =
      (SID_LIST =
        (SID_DESC =
          (SID_NAME = test)
          (ORACLE_HOME = C:\OracleDatabase\product\11.2.0\dbhome_1)
          (PROGRAM = extproc)
          (ENVS = "EXTPROC_DLLS=ONLY:C:\OracleDatabase\product\11.2.0\dbhome_1\bin\oraclr11.dll")
        )
      )

    LISTENER =
      (DESCRIPTION_LIST =
        (DESCRIPTION =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE1)(PORT = 1521))
          (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC1521))
        )
      )

Sljedeći, Udaljena radna površina da biste web-mjesto Machine2 i u okvir za uređivanje u listener.ora datoteke na sljedeći način: # listener.ora mrežna konfiguracija datoteka: C:\OracleDatabase\product\11.2.0\dbhome_1\network\admin\listener.ora

    # Generated by Oracle configuration tools.

    SID_LIST_LISTENER =
      (SID_LIST =
        (SID_DESC =
          (SID_NAME = test_stby)
          (ORACLE_HOME = C:\OracleDatabase\product\11.2.0\dbhome_1)
          (PROGRAM = extproc)
          (ENVS = "EXTPROC_DLLS=ONLY:C:\OracleDatabase\product\11.2.0\dbhome_1\bin\oraclr11.dll")
        )
      )

    LISTENER =
      (DESCRIPTION_LIST =
        (DESCRIPTION =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE2)(PORT = 1521))
          (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC1521))
        )
      )


### <a name="configure-tnsnamesora-on-the-primary-and-standby-virtual-machines-to-hold-entries-for-both-primary-and-standby-databases"></a>Konfiguriranje tnsnames.ora na virtualnim strojevima primarnih i čekanja držanje stavke za primarnih i čekanja baze podataka

Udaljena radna površina Machine1 i uređivanje datoteke tnsnames.ora kao navedeno u nastavku. Tnsnames.ora datoteku možete pronaći u sljedećoj mapi: c:\\OracleDatabase\\proizvoda\\11.2.0\\dbhome\_1\\MREŽNI\\ADMINISTRATOR\\.

    TEST =
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE1)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = test)
        )
      )

    TEST_STBY =
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE2)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = test_stby)
        )
      )

Udaljena radna površina da biste Machine2 i uređivanje u tnsnames.ora datoteke na sljedeći način:

    TEST =
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE1)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = test)
        )
      )

    TEST_STBY =
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE2)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = test_stby)
        )
      )


### <a name="start-the-listener-and-check-tnsping-on-both-virtual-machines-to-both-services"></a>Pokrenite ga slušatelj i provjerite tnsping na oba virtualnim računalima sustava oba servisa.

Otvorite novi naredbeni redak Windows na primarnih i čekanja virtualnim računalima sustava i pokrenite sljedeće naredbe:

    C:\Users\DBAdmin>tnsping test

    TNS Ping Utility for 64-bit Windows: Version 11.2.0.1.0 - Production on 14-NOV-2013 06:29:08
    Copyright (c) 1997, 2010, Oracle.  All rights reserved.
    Used parameter files:
    C:\OracleDatabase\product\11.2.0\dbhome_1\network\admin\sqlnet.ora
    Used TNSNAMES adapter to resolve the alias
    Attempting to contact (DESCRIPTION = (ADDRESS_LIST = (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE1)(PORT = 1521))) (CONNECT_DATA = (SER
    VICE_NAME = test)))
    OK (0 msec)


    C:\Users\DBAdmin>tnsping test_stby

    TNS Ping Utility for 64-bit Windows: Version 11.2.0.1.0 - Production on 14-NOV-2013 06:29:16
    Copyright (c) 1997, 2010, Oracle.  All rights reserved.
    Used parameter files:
    C:\OracleDatabase\product\11.2.0\dbhome_1\network\admin\sqlnet.ora
    Used TNSNAMES adapter to resolve the alias
    Attempting to contact (DESCRIPTION = (ADDRESS_LIST = (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE2)(PORT = 1521))) (CONNECT_DATA = (SER
    VICE_NAME = test_stby)))
    OK (260 msec)


##<a name="start-up-the-standby-instance-in-nomount-state"></a>Pokretanje instancu čekanja u stanju nomount
Da biste postavili okruženje za podršku čekanja baze podataka na čekanja virtualnog računala (MACHINE2).

Najprije kopirajte datoteku s lozinkom primarni računalu (Machine1) čekanja računala (Machine2) ručno. To je potrebno kao što je lozinka **sistemskim** moraju biti jednake na oba računala.

Zatim otvorite naredbeni redak sustava Windows u Machine2 i postavljanje varijable okruženja da upućuju na stanje mirovanja baze podataka na sljedeći način:

    SET ORACLE_HOME=C:\OracleDatabase\product\11.2.0\dbhome_1
    SET ORACLE_SID=TEST_STBY

Nakon toga pokretanje čekanja baze podataka u stanju nomount i generiranje spfile.

Pokretanje baze podataka:

    SQL>shutdown immediate;

    SQL>startup nomount
    ORACLE instance started.

    Total System Global Area  747417600 bytes
    Fixed Size                  2179496 bytes
    Variable Size             473960024 bytes
    Database Buffers          264241152 bytes
    Redo Buffers                7036928 bytes


##<a name="use-rman-to-clone-the-database-and-to-create-a-standby-database"></a>Pomoću RMAN Kloniraj baze podataka i stvaranje stanja čekanja baze podataka
Da biste preuzeli sve sigurnosnu kopiju primarni baze podataka da biste stvorili fizičke čekanja baze podataka možete koristiti uslužni Upravitelj za oporavak (RMAN).

Udaljena radna površina da biste čekanja virtualnog računala (MACHINE2) i pokrenite uslužni RMAN navođenjem pune veze niza za oba CILJNE (primarni baza podataka, Machine1) i POMOĆNA (čekanja baza podataka, Machine2) instance.

>[AZURE.IMPORTANT] Pomoću provjere autentičnosti za operacijski sustav jer postoji nijedna baza podataka na računalu čekanja poslužitelja još.

    C:\> RMAN TARGET sys/password@test AUXILIARY sys/password@test_STBY

    RMAN>DUPLICATE TARGET DATABASE
      FOR STANDBY
      FROM ACTIVE DATABASE
      DORECOVER
        NOFILENAMECHECK;

##<a name="start-the-physical-standby-database-in-managed-recovery-mode"></a>Pokretanje fizičke čekanja baze podataka u načinu rada za oporavak upravljanih
Pomoću ovog praktičnog vodiča pokazuje kako stvoriti fizičke čekanja baze podataka. Dodatne informacije o stvaranju logičke čekanja baze podataka, potražite u dokumentaciji za Oracle.

Otvaranje kopije SQL\*Plus naredbeni redak i omogućivanje straža podataka na čekanja virtualnog računala ili na poslužitelju (MACHINE2) na sljedeći način:

    SHUTDOWN IMMEDIATE;
    STARTUP MOUNT;
    ALTER DATABASE RECOVER MANAGED STANDBY DATABASE DISCONNECT FROM SESSION;

Kada otvorite čekanja baze podataka u načinu **postavljanja** , zapisnika arhive i dalje dostave i postupka upravljanih oporavka nastavlja zapisnika primjene čekanja bazu podataka. Na taj način ostaje li ažuran s primarnom bazom podataka čekanja baze podataka. Imajte na umu da čekanja baze podataka se ne mogu učiniti dostupnima izvješćivanja tijekom određenog razdoblja.

Kada otvorite čekanja baze podataka u načinu za **ČITANJE samo** , prijavu u arhivu dostave Nastavi. No zaustavlja postupka upravljanih oporavka. Zbog toga čekanja baze podataka radi postati sve zastarjeli dok je nastaviti postupka upravljanih oporavka. Pristup čekanja baze podataka za izvješćivanja tijekom određenog razdoblja, ali podataka možda neće odražavati najnovije promjene.

Općenito govoreći, preporučujemo da ostavite čekanja baze podataka u načinu **postavljanja** da se podaci u bazi podataka čekanja ažuran ako postoji pogreška primarnom bazom podataka. Međutim, možete zadržati čekanja baze podataka u načinu za **ČITANJE samo** za izvješćivanja ovisno o preduvjeti za aplikaciju sustava. Sljedeći koraci pokazuju kako omogućiti straža podataka u načinu samo za čitanje pomoću SQL\*Plus:

    SHUTDOWN IMMEDIATE;
    STARTUP MOUNT;
    ALTER DATABASE OPEN READ ONLY;


##<a name="verify-the-physical-standby-database"></a>Provjerite je li fizička čekanja baze podataka
U ovom se odjeljku objašnjava kako provjeriti konfiguracije visoke dostupnosti kao administrator.

Otvaranje kopije SQL\*Plus prozor naredbenog retka i potvrdite arhiviraju ponavljanje zapisnika na čekanja virtualnog računala (Machine2):

    SQL> show parameters db_unique_name;

    NAME                                TYPE       VALUE
    ------------------------------------ ----------- ------------------------------
    db_unique_name                      string     TEST_STBY

    SQL> SELECT NAME FROM V$DATABASE

    SQL> SELECT SEQUENCE#, FIRST_TIME, NEXT_TIME, APPLIED FROM V$ARCHIVED_LOG ORDER BY SEQUENCE#;

    SEQUENCE# FIRST_TIM NEXT_TIM APPLIED
    ----------------  ---------------  --------------- ------------
    45                    23-FEB-14   23-FEB-14   YES
    45                    23-FEB-14   23-FEB-14   NO
    46                    23-FEB-14   23-FEB-14   NO
    46                    23-FEB-14   23-FEB-14   YES
    47                    23-FEB-14   23-FEB-14   NO
    47                    23-FEB-14   23-FEB-14   NO

Otvaranje kopije SQL * Plus naredbeni redak logfiles prozor i prijelaz na primarni računalu (Machine1):

    SQL> alter system switch logfile;
    System altered.

    SQL> archive log list
    Database log mode              Archive Mode
    Automatic archival             Enabled
    Archive destination            C:\OracleDatabase\archive
    Oldest online log sequence     69
    Next log sequence to archive   71
    Current log sequence           71

U zapisniku arhivirane Ponovi poništeno na čekanja virtualnog računala (Machine2):

    SQL> SELECT SEQUENCE#, FIRST_TIME, NEXT_TIME, APPLIED FROM V$ARCHIVED_LOG ORDER BY SEQUENCE#;

    SEQUENCE# FIRST_TIM NEXT_TIM APPLIED
    ----------------  ---------------  --------------- ------------
    45                    23-FEB-14   23-FEB-14   YES
    46                    23-FEB-14   23-FEB-14   YES
    47                    23-FEB-14   23-FEB-14   YES
    48                    23-FEB-14   23-FEB-14   YES

    49                    23-FEB-14   23-FEB-14   YES
    50                    23-FEB-14   23-FEB-14   IN-MEMORY

Provjerite sve razmak na čekanja virtualnog računala (Machine2):

    SQL> SELECT * FROM V$ARCHIVE_GAP;
    no rows selected.

Drugi način provjere nije biti prebacivanje čekanja bazu podataka, a zatim testirajte ako je moguće failback s primarnom bazom podataka. Da biste aktivirali čekanja bazu podataka kao primarni baze podataka, koristite sljedeće naredbe:

    SQL> ALTER DATABASE RECOVER MANAGED STANDBY DATABASE FINISH;
    SQL> ALTER DATABASE ACTIVATE STANDBY DATABASE;

Ako niste omogućili flashback na izvorne primarni baze podataka, preporučuje se ispustite izvorne primarni baze podataka i ponovno stvoriti kao čekanja baze podataka.

Preporučujemo da omogućite flashback baze podataka na primarnih i čekanja baze podataka. Kada se dogodi s pomoćnim, primarnom bazom podataka možete biti flashed natrag u vrijeme prije na prebacivanje i brzo pretvoriti u čekanja baze podataka.

##<a name="additional-resources"></a>Dodatni resursi
[Oracle virtualnog računala slike za Azure](virtual-machines-windows-classic-oracle-images.md)
