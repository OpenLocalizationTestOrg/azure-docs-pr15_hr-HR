<properties
    pageTitle="Konfiguriranje Oracle GoldenGate u VMs | Microsoft Azure"
    description="Prođite vodič za postavljanje i implementaciju Oracle GoldenGate na Azure Windows Server VMs visoke dostupnosti i Izrada oporavka."
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


#<a name="configuring-oracle-goldengate-for-azure"></a>Konfiguriranje Oracle GoldenGate za Azure


Pomoću ovog praktičnog vodiča pokazuje kako postaviti Oracle GoldenGate za Azure virtualnim strojevima okruženja za visoke dostupnosti i Izrada oporavak. Vodič usredotočuje se na [dvosmjerni replikacije](http://docs.oracle.com/goldengate/1212/gg-winux/GWUAD/wu_about_gg.htm) za baze podataka koje nisu iz programa RAC Oracle i zahtijeva jesu li oba web-mjesta aktivna.

Oracle GoldenGate podržava distribucija podataka i Integracija podataka. Omogućuje vam da biste postavili distribucija podataka i rješenje sinkronizacije podataka putem konfiguracija replikacije Oracle Oracle i nudi rješenje fleksibilne visoke dostupnosti. Oracle GoldenGate nadopunjuju straža podataka tvrtke Oracle s njegovim mogućnostima replikacije da biste omogućili informacije na razini tvrtke distribucije i nula nedostupnost nadogradnji i migracije. Detaljne informacije potražite u članku [Korištenje Oracle GoldenGate s straža podataka tvrtke Oracle](http://docs.oracle.com/cd/E11882_01/server.112/e17157/unplanned.htm).

Oracle GoldenGate sadrži sljedeće komponente glavni: izdvajanje podataka pump, Replicat, tragove ili izdvojiti datoteke, Checkpoints, upravitelj i prikupljanje. Da bi se piše Dvosmjerno replikacije između dva web-mjesta, morate stvoriti i pokretanje svih komponenti na oba web-mjesta. Detaljne informacije o Oracle GoldenGate arhitektura, potražite u članku [Vodič za Oracle GoldenGate](http://docs.oracle.com/goldengate/1212/gg-winux/index.html).

Pomoću ovog praktičnog vodiča pretpostavlja da ste već theoretical i praktične znanja visoke dostupnosti Oracle baze podataka i oporavak Izrada koncepti, kao i [Oracle GoldenGate](http://docs.oracle.com/goldengate/1212/gg-winux/index.html). Dodatne informacije potražite u članku [Oracle web-mjesta](http://www.oracle.com/technetwork/database/features/availability/index.html).

Osim toga, vodič pretpostavlja već implementirana sljedeće preduvjete:

- Već ste pregledali visoke dostupnosti i Izrada oporavak pitanja vezana uz odjeljak u temi [Oracle virtualnog računala slike - razne pitanja vezana uz](virtual-machines-windows-classic-oracle-considerations.md) . Imajte na umu da Azure podržava instanci baze podataka tvrtke Oracle samostalni, ali ne Oracle realni aplikacije klastere (Oracle RAC) trenutno.

- Softver tvrtke Oracle GoldenGate ste preuzeli s web-mjesto [Tvrtke Oracle preuzimanja](http://www.oracle.com/us/downloads/index.html) . Odaberete u proizvoda paketa Oracle Fusion proizvod – Integracija podataka. Zatim odaberete Oracle GoldenGate na Oracle v11.2.1 multimedijski paket za Microsoft Windows x64 (64-bitni) za bazom podataka tvrtke Oracle 11 g. Nakon toga preuzmite Oracle GoldenGate V11.2.1.0.3 za Oracle 11g 64-bitni na Windows 2008 (64-bitni).

- Stvorite dva virtualnim strojevima (VMs) u Azure pomoću Oracle Enterprise Edition u sustavu Windows Server. Provjerite jesu li da virtualnim strojevima u [istom servis u oblaku](virtual-machines-linux-load-balance.md) i u istoj [Virtualne mreže](https://azure.microsoft.com/documentation/services/virtual-network/) da biste bili sigurni međusobno mogu pristupiti putem stalni privatne IP adrese.

- Na portalu za Azure klasični sam postavila nazive virtualnog računala kao "MachineGG1" za web-mjesta A i "MachineGG2" za B web-mjesta.

- Ste stvorili baza podataka za testiranje "TestGG1" na web-mjesta odgovora i "TestGG2" na web-mjesta B.

- Prijave u Windows server kao član grupe administratora ili član grupe **ORA_DBA** .

U ovom ćete praktičnom vodiču ćete:

1. Postavljanje baze podataka na web-mjestu A i B web-mjesta  

    1. Izvođenje učitavanja početne podataka

2. Priprema web-mjestu A i B web-mjesta za replikacije baze podataka

3. Stvaranje svih objekata potrebne za podršku DDL replikacije

4. Konfiguriranje upravitelja GoldenGate na web-mjestu A i B web-mjesta

5. Stvaranje grupe za izdvajanje i Pumpa se podaci procese na web-mjestu A i B web-mjesta

    1. Stvaranje postupaka izdvojiti i Pumpa se podataka na web-mjestu odgovora

    2. Stvaranje tablice GoldenGate kontrolne točke na web-mjesta B

    3. Dodavanje REPLICAT na web-mjestu B

    4. Stvaranje postupaka izdvojiti i Pumpa se podataka na web-mjesta B

    5. Stvaranje tablice GoldenGate kontrolne točke na web-mjestu odgovora

    6. Dodavanje REPLICAT na web-mjestu odgovora

    7. Dodavanje trandata na web-mjestu A i B web-mjesta

    8. Pokretanje procesa izdvojiti i Pumpa se podataka na web

    9. Pokretanje procesa izdvojiti i Pumpa se podaci na web-mjesta B

    10. Pokreni proces REPLICAT na web-mjestu odgovora

    11. Pokreni proces REPLICAT na web-mjesta B

6. Provjerite je li proces dvosmjerni replikacije

>[AZURE.IMPORTANT] Pomoću ovog praktičnog vodiča je postavljanje i testirati u odnosu na sljedeću konfiguraciju softver:
>
>|                        | **Web-baze podataka**              | **Web-mjesta B baze podataka**              |
>|------------------------|----------------------------------|----------------------------------|
>| **Oracle izdanje**     | Oracle11g izdanje 2 – (11.2.0.1) | Oracle11g izdanje 2 – (11.2.0.1) |
>| **Naziv računala**       | MachineGG1                       | MachineGG2                       |
>| **Operacijski sustav**   | Windows 2008 R2                  | Windows 2008 R2                  |
>| **Oracle SID**         | TESTGG1                          | TESTGG2                          |
>| **Shema replikacije** | LUKA                            | LUKA                            |

Za sljedeće izdanja baze podataka tvrtke Oracle i Oracle GoldenGate možda postoje neka dodatne promjene koje su vam potrebne za implementaciju. Najnovije verzije određene informacije, potražite u članku [Oracle GoldenGate](http://docs.oracle.com/goldengate/1212/gg-winux/index.html) i [Podataka tvrtke Oracle](http://www.oracle.com/us/corporate/features/database-12c/index.html) dokumentaciju Oracle web-mjesta. Na primjer, izdanje 11.2.0.4 izvorišne baze podataka i novijim verzijama snimanje DDL se izvodi na poslužitelju logmining asinkrono i potreban je bez posebno okidača, tablica ili drugih objekata baze podataka za instalaciju. Oracle GoldenGate nadogradnje moguće je izvesti bez prekida korisničke programe. Kada je izdvojiti u integriranom načinu rada s bazom podataka tvrtke Oracle 11g izvora na stariju od verzije 11.2.0.4 potreban je korištenja okidača DDL i objekte za podršku. Detaljne upute potražite u članku [Instaliranje i konfiguriranje Oracle GoldenGate za bazu podataka tvrtke Oracle](http://docs.oracle.com/goldengate/1212/gg-winux/GIORA.pdf).

##<a name="1-setup-database-on-site-a-and-site-b"></a>1. Postavljanje baze podataka na web-mjestu A i B web-mjesta
U ovom se odjeljku objašnjava kako izvršiti preduvjeti baze podataka na web-mjestu A i B. web-mjesta Morate izvršiti sve korake ovog odjeljka na oba web-mjesta: web-mjestu A i B. web-mjesta

Prvi, Udaljena radna površina da biste web-mjestu A i B web-mjesta putem portala za Azure klasični. Otvorite naredbeni redak za Windows i stvorite osnovne mape za Oracle GoldenGate instalacijske datoteke:

    mkdir C:\OracleGG

Nakon toga raspakiraj i instalirati softver tvrtke Oracle GoldenGate u ovu mapu. Nakon ovaj korak pokrenete GoldenGate softver naredba tumačenja (GGSCI) izvršavanjem sljedeće naredbe:

    C:\OracleGG\.\ggsci

Možete koristiti [GGSCI](http://docs.oracle.com/goldengate/1212/gg-winux/GWUAD/wu_gettingstarted.htm) da biste pokrenuli nekoliko naredbi koje konfiguriranje, kontrola i monitor Oracle GoldenGate.

Nakon toga pokrenite sljedeću naredbu da biste stvorili sve podmape iz instalacijski paket:

    GGSCI (Hostname) 1> CREATE SUBDIRS

Pokrenite sljedeću naredbu da biste izašli iz naredbenog retka GGSCI:

    GGSCI (Hostname) 1> EXIT

Nakon toga morati stvoriti korisnik baze podataka koja će se koristiti, Upravitelj GoldenGate Oracle, izdvojiti i replikacije procesi. Imajte na umu da možete stvoriti pojedinačne korisnike za svakog procesa ili konfiguriranje samo jedan korisnik zajednički. U ovom ćete praktičnom vodiču ćemo stvoriti jednog korisnika koja se naziva kao ggate. Nakon toga ne možemo dati taj korisnik potrebne ovlasti. Imajte na umu da morate izvesti sljedeće radnje na web-mjestu A i B. web-mjesta

Otvaranje kopije SQL\*Plus prozoru za naredbe na web-mjestu A i B web-mjesta s ovlastima administratora baze podataka pomoću **SYSDBA**, kao što su:

    Enter username: / as sysdba

I pokretanje:

    SQL> create tablespace ggs_data   datafile 'c:\OracleDatabase\oradata\<DBNAME>\<DBNAME>ggs_data01.dbf' size 200m;
    SQL> create user ggate identified by ggate default tablespace ggs_data  temporary tablespace temp;
          grant connect, resource to ggate;
          grant select any dictionary, select any table to ggate;
          grant create table to ggate;
          grant flashback any table to ggate;
          grant execute on dbms_flashback to ggate;
          grant execute on utl_file to ggate;
          grant create any table to ggate;
          grant insert any table to ggate;
          grant update any table to ggate;
          grant delete any table to ggate;
          grant drop any table to ggate;

Nakon toga pronađite u INIT\<DatabaseSID\>. ORA datoteka u % ORACLE\_POLAZNO %\\baze podataka mapu na web-mjestu A i B web-mjesta i dodati INITTEST.ora od sljedećih parametara baze podataka:

    UNDO\_MANAGEMENT=AUTO
    UNDO\_RETENTION=86400

Potpuni popis svih naredbi za Oracle GoldenGate GGSCI, potražite u članku [Referenca za Oracle GoldenGate za Windows](http://docs.oracle.com/goldengate/1212/gg-winux/GWURF/ggsci_commands.htm).

### <a name="perform-initial-data-load"></a>Izvođenje učitavanja početne podataka

Učitavanja početne podatke možete izvesti u bazi podataka tako da slijedite nekoliko načina. Na primjer, možete koristiti [Oracle GoldenGate Izravni učitavanja](http://docs.oracle.com/goldengate/1212/gg-winux/GWUAD/wu_initsync.htm) ili obični uslužni programi izvoz i uvoz izvoz podataka u tablici s web-mjesta odgovora B. web-mjesta

Da bismo pokazali ponavljanja postupka Oracle GoldenGate, pomoću ovog praktičnog vodiča prikazuje stvaranje tablice na web-mjestu A i B web-mjesta pomoću sljedeće naredbe.

Najprije otvorite SQL\*Plus naredbu prozor i pokrenite sljedeću naredbu da biste stvorili tablicu programa zaliha na web-mjesta A i B web-mjesta baze podataka:

    create table scott.inventory
    (prod_id number,
    prod_category varchar2(20),
    qty_in_stock number,
    last_dml timestamp default systimestamp);

Nakon toga dodali ograničenje novostvorenu tablicu na web-mjestu A i B web-mjesta baza podataka:

    alter table scott.inventory add constraint pk_inventory primary key (prod_id) ;

Nakon toga dodijelite sve ovlasti za novu tablicu zaliha za ggate korisnika na web-mjestu A i B: web-mjesta

    grant all on scott.inventory to ggate;

Zatim stvorite i omogućivanje okidač baze podataka INVENTORY_CDR_TRG, novostvorenu tablicu da biste provjerili sve transakcije u novu tablicu bilježe ako korisnik nije ggate. Izvođenje ovog postupka na web-mjestu A i B. web-mjesta

    CREATE OR REPLACE TRIGGER INVENTORY_CDR_TRG
    BEFORE UPDATE
    ON SCOTT.INVENTORY
    REFERENCING NEW AS New OLD AS Old
    FOR EACH ROW
    BEGIN
    IF SYS_CONTEXT ('USERENV', 'SESSION_USER') != 'GGATE'
    THEN
    :NEW.LAST_DML := SYSTIMESTAMP;
    END IF;
    END;
    /


##<a name="2-prepare-site-a-and-site-b-for-database-replication"></a>2. Priprema web-mjestu A i B web-mjesta za replikacije baze podataka
U ovom se odjeljku objašnjava kako pripremiti web-mjestu A i B web-mjesta za replikacije baze podataka. Morate izvršiti sve korake ovog odjeljka na oba web-mjesta: web-mjestu A i B. web-mjesta

Prvi, Udaljena radna površina da biste web-mjestu A i B web-mjesta putem portala za Azure klasični. Prijelaz u bazu podataka na način rada archivelog pomoću SQL * Plus u prozoru za naredbe:

    sql>shutdown immediate
    sql>startup mount
    sql>alter database archivelog;
    sql>alter database open;


Nakon toga omogućite minimalnog [Dodatni zapisivanje](http://docs.oracle.com/cd/E11882_01/server.112/e22490/logminer.htm) na sljedeći način:

    SQL> ALTER DATABASE ADD SUPPLEMENTAL LOG DATA (ALL) COLUMNS;

Nakon toga priprema baze podataka za podršku replikacije DDL (jezika za definiranje podataka):

    SQL> alter system set recyclebin=off scope=spfile;

Zatim, isključivanje i ponovno pokretanje baze podataka:

    sql>shutdown immediate
    sql>startup


##<a name="3-create-all-necessary-objects-to-support-ddl-replication"></a>3. stvaranje svih objekata potrebne za podršku DDL replikacije
U ovom se odjeljku navedeni skripte koje su vam potrebne za stvaranje svih objekata potrebne za podršku DDL replikacije. Morate pokrenuti skripte navedena u ovom odjeljku na web-mjestu A i B. web-mjesta

Otvorite naredbeni redak sustava Windows, a zatim otvorite mapu Oracle GoldenGate, kao što su C:\\OracleGG. Pokretanje SQL\*Plus naredbeni redak s ovlastima administratora baze podataka, kao što su korištenje **SYSDBA** na web-mjestu A i B. web-mjesta

Izvedite sljedeće skripte:

    SQL> @marker_setup.sql  
    Enter GoldenGate schema name: ggate
    SQL> @ddl_setup.sql  
    Enter GoldenGate schema name: ggate
    SQL> @role_setup.sql
    Enter GoldenGate schema name: ggate
    SQL> grant ggs_ggsuser_role to ggate;
     Grant succeeded.
     SQL> @ddl_enable
     Trigger altered.
     SQL> @ddl_pin ggate


Alat za GoldenGate Oracle potreban prijave razine tablice za podršku DDL (jezika za definiranje podataka). To je zašto, a zatim Omogući dodatna Prijava na razini tablice pomoću naredbe DODAJ TRANDATA. Otvorite Oracle GoldenGate tumačenja u prozoru za naredbe prijava bazu podataka, a zatim pokrenite naredbu DODAJ TRANDATA:

    GGSCI 5> DBLOGIN USERID ggate, PASSWORD ggate

    GGSCI(Hostname) 6> add trandata scott.inventory

##<a name="4-configure-goldengate-manager-on-site-a-and-site-b"></a>4. podešavanje upravitelja GoldenGate na web-mjestu A i B web-mjesta
Upravitelj GoldenGate Oracle izvodi broj funkcije kao što su početni GoldenGate procese, upravljanje datotekama zapisnika trag i izvješćivanje o pogreškama.

Morate konfigurirati postupka Oracle GoldenGate Upravitelj na web-mjestu A i B. web-mjesta Da biste to učinili, poduzeti sljedeće korake na web-mjestu A i B. web-mjesta

Otvorite Windows naredbu prozor i pokretanje naredbe tumačenja Oracle GoldenGate:

    cd C:\OracleGG\
    c:\OracleGG>ggsci
    Oracle GoldenGate Command Interpreter for Oracle
    Version 11.2.1.0.3 14400833 OGGCORE_11.2.1.0.3_PLATFORMS_120823.1258
    Windows x64 (optimized), Oracle 11g on Aug 23 2012 16:50:36
    Copyright (C) 1995, 2012, Oracle and/or its affiliates. All rights reserved.


Zapisuje GGSCI sesije u bazu podataka da bi se za izvršavanje naredbe koje utječu na bazu podataka:

    GGSCI (HostName) 1> DBLOGIN USERID ggate, PASSWORD ggate
    Successfully logged into database.

Prikaz stanja i kašnjenja (pri čemu je odgovarajući) za sve procese upraviteljskim, izdvojite i Replicat u sustavu:

    GGSCI (HostName) 2> info all
    Program     Status      Group       Lag           Time Since Chkpt
    MANAGER     STOPPED

Otvorite datoteku parametar pomoću naredbe za uređivanje PARAMS i dodavanje sljedeće informacije:

    GGSCI (HostName) 3> edit params mgr
    PORT 7809
    USERID ggate, PASSWORD ggate
    PURGEOLDEXTRACTS  C:\OracleGG\dirdat\ex, USECHECKPOINTS

Prikaz stanja i kašnjenja (pri čemu je odgovarajući) za sve procese upraviteljskim, izdvojiti i Replicat u sustavu:

    GGSCI (HostName) 46> info all
    Program     Status      Group       Lag           Time Since Chkpt
    MANAGER     STOPPED

Zapisuje GGSCI sesije u bazu podataka da bi se za izvršavanje naredbe koje utječu na bazu podataka:

    GGSCI (HostName) 47> dblogin USERID ggate, PASSWORD ggate

    Successfully logged into database.

Pokretanje postupka za Upravitelj:

    GGSCI (HostName) 48> start manager
    Manager started.

##<a name="5-create-extract-group-and-data-pump-processes-on-site-a-and-site-b"></a>5. stvaranje izdvojiti postupaka grupe i Pumpa se podataka na web-mjestu A i B web-mjesta

###<a name="create-extract-and-data-pump-processes-on-site-a"></a>Stvaranje postupaka izdvojiti i Pumpa se podataka na web-mjestu odgovora

Potrebnih za stvaranje procesa izdvojiti i Pumpa se podataka na web i web-mjesta B. udaljene radne površine na web-mjestu A i B web-mjesta putem portala za Azure klasični. Otvaranje GGSCI naredba tumačenja prozor prema gore. Pokrenite sljedeće naredbe na web-mjesta o:

    GGSCI (MachineGG1) 14> add extract ext1 tranlog begin now
    EXTRACT added.
    GGSCI (MachineGG1) 4> add exttrail C:\OracleGG\dirdat\lt, extract ext1
    EXTTRAIL added.
    GGSCI (MachineGG1) 16> add extract dpump1 exttrailsource C:\OracleGG\dirdat\aa
    EXTRACT added.
    GGSCI (MachineGG1) 17> add rmttrail C:\OracleGG\dirdat\ab extract dpump1
    RMTTRAIL added.

Otvorite datoteku parametar pomoću naredbe za uređivanje PARAMS i dodavanje sljedeće informacije: GGSCI (MachineGG1) 18 > Uređivanje params ext1 IZDVOJITI ext1 korisnički ID ggate, lozinka ggate EXTTRAIL C:\OracleGG\dirdat\aa TRANLOGOPTIONS EXCLUDEUSER scott.inventory ggate TABLICE, GETBEFORECOLS (Uključeno ažuriranje KEYINCLUDING (prod_category, qty_in_stock, last_dml), Dalje brisanje KEYINCLUDING (prod_category, qty_in_stock, last_dml))

Otvorite datoteku parametar pomoću naredbe za uređivanje PARAMS i dodavanje sljedeće informacije:

    GGSCI (MachineGG1) 15> edit params dpump1
    EXTRACT dpump1
     USERID ggate, PASSWORD ggate
     RMTHOST ActiveGG2orcldb, MGRPORT 7809, TCPBUFSIZE 100000
     RMTTRAIL C:\OracleGG\dirdat\ab
     PASSTHRU
     TABLE scott.inventory;

###<a name="create-a-goldengate-checkpoint-table-on-site-b"></a>Stvaranje tablice GoldenGate kontrolne točke na web-mjesta B

Nakon toga morate dodati tablicu kontrolne točke na web-mjesta B. Da biste to učinili, morate otvoriti GoldenGate naredbeni tumačenja prozor i pokretanje:

    C:\OracleGG\ggsci
    GGSCI (MachineGG2) 1> DBLOGIN USERID ggate, PASSWORD ggate
    Successfully logged into database.

I zatim dodajte tablici kontrolne točke u bazu podataka, pri čemu je ggate vlasnik:

    GGSCI (MachineGG2) 2> ADD CHECKPOINTTABLE ggate.checkpointtable
    Successfully created checkpoint table ggate.checkpointtable.

Dodajte naziv tablice točke potvrdite GLOBALS datoteku na poslužitelju ciljni koji je B web-mjesta u ovom ćete koraku. Uređivanje datoteke GLOBALS na B: web-mjesta

    GGSCI (MachineGG2) 1> EDIT PARAMS ./GLOBALS

Nakon toga dodavanjem parametara CHECKPOINTTABLE postojeću datoteku GLOBALS:

    GGSCHEMA ggate
    CHECKPOINTTABLE ggate.checkpointtable

Kao konačan korak, spremite i zatvorite datoteku GLOBALS parametar.


###<a name="add-replicat-on-site-b"></a>Dodavanje REPLICAT na web-mjestu B
U ovom se odjeljku opisuje kako dodati REPLICAT postupak "REP2" na web-mjesta B.

Pomoću naredbe DODAJ REPLICAT da biste stvorili grupu Replicat na B: web-mjesta

    GGSCI (MachineGG2) 37> add replicat rep2 exttrail C:\OracleGG\dirdatab, checkpointtable ggate.checkpointtable

Otvorite datoteku parametar pomoću naredbe za uređivanje PARAMS i dodavanje sljedeće informacije:

    GGSCI (MachineGG2) 10> edit params rep2
    REPLICAT rep2
    ASSUMETARGETDEFS
    USERID ggate, PASSWORD ggate
    DISCARDFILE C:\OracleGG\dirdat\discard.txt, append,megabytes 10
    MAP scott.inventory, TARGET scott.inventory;

###<a name="create-extract-and-data-pump-processes-on-site-b"></a>Stvaranje postupaka izdvojiti i Pumpa se podataka na web-mjesta B

U ovom se odjeljku opisuje kako stvoriti novi proces izdvojiti "EXT2" i novi proces podataka pump "DPUMP2" na web-mjesta B:

    GGSCI (MachineGG2) 3> add extract ext2 tranlog begin now
     EXTRACT added.
    GGSCI (MachineGG2) 4> add exttrail C:\OracleGG\dirdat\ac extract ext2
     EXTTRAIL added.
    GGSCI (MachineGG2) 5> add extract dpump2 exttrailsource C:\OracleGG\dirdat\ac
     EXTRACT added.
    GGSCI (MachineGG2) 6> add rmttrail C:\OracleGG\dirdat\ad extract dpump2
     RMTTRAIL added.

Otvorite datoteku parametar pomoću naredbe za uređivanje PARAMS i dodavanje sljedeće informacije:

    GGSCI (MachineGG2) 31> edit params ext2
    EXTRACT ext2
    USERID ggate, PASSWORD ggate
    EXTTRAIL C:\OracleGG\dirdat\ac
    TRANLOGOPTIONS EXCLUDEUSER ggate
    TABLE scott.inventory,
    GETBEFORECOLS (
    ON UPDATE KEYINCLUDING (prod_category,qty_in_stock, last_dml),
    ON DELETE KEYINCLUDING (prod_category,qty_in_stock, last_dml));

Otvorite datoteku parametar pomoću naredbe za uređivanje PARAMS i dodavanje sljedeće informacije:

    GGSCI (MachineGG2) 32> edit params dpump2
    EXTRACT dpump2
    USERID ggate, PASSWORD ggate
    RMTHOST MachineGG1, MGRPORT 7809, TCPBUFSIZE 100000
    RMTTRAIL C:\OracleGG\dirdat\ad
    PASSTHRU
    TABLE scott.inventory;

###<a name="create-a-goldengate-checkpoint-table-on-site-a"></a>Stvaranje tablice GoldenGate kontrolne točke na web-mjestu odgovora

Otvorite Oracle GoldenGate tumačenja u prozoru za naredbe, a zatim stvorite tablicu Kontrolna točka:

    GGSCI (MachineGG1) 1> DBLOGIN USERID ggate, PASSWORD ggate
    Successfully logged into database.

    GGSCI (MachineGG1) 2> ADD CHECKPOINTTABLE ggate.checkpointtable

    Successfully created checkpoint table ggate.checkpointtable.

Morate dodati naziv tablice točke potvrdite GLOBALS datoteka na web-mjestu A.

Otvaranje kopije Oracle GoldenGate prevoditelj u prozoru za naredbe i uređivanje GLOBALS datoteke na web-mjesta A:

    GGSCI (MachineGG1) 1> EDIT PARAMS ./GLOBALS
    Add the CHECKPOINTTABLE parameter to the existing GLOBALS file as follows:
    GGSCHEMA ggate
    CHECKPOINTTABLE ggate.checkpointtable

Spremite i zatvorite datoteku parametar GLOBALS.

###<a name="add-replicat-on-site-a"></a>Dodavanje REPLICAT na web-mjestu odgovora

U ovom se odjeljku opisuje kako dodati REPLICAT postupak "REP1" na web-mjestu A.

Sljedeća naredba stvara rep1 za grupu Replicat pod nazivom trag i povezane checkpointtable.

    GGSCI (MachineGG1) 21> add replicat rep1 exttrail C:\OracleGG\dirdat\ad,checkpointtable ggate.checkpointtable
     REPLICAT added.

Otvorite datoteku parametar pomoću naredbe za uređivanje PARAMS i dodavanje sljedeće informacije:

    GGSCI (MachineGG1) 10> edit params rep1
    REPLICAT rep1
    ASSUMETARGETDEFS
    USERID ggate, PASSWORD ggate
    DISCARDFILE C:\OracleGG\dirdat\discard.txt, append, megabytes 10
    MAP scott.inventory, TARGET scott.inventory;

### <a name="add-trandata-on-site-a-and-site-b"></a>Dodavanje trandata na web-mjestu A i B web-mjesta

Omogućite dodatni zapisivanje na razini tablice pomoću naredbe DODAJ TRANDATA. Otvorite Oracle GoldenGate tumačenja u prozoru za naredbe, prijavite se na bazu podataka, a zatim pokrenite naredbu DODAJ TRANDATA.

Udaljena radna površina da biste MachineGG1, otvorite Oracle GoldenGate naredba tumačenja i pokretanje:

    GGSCI (MachineGG1) 11> dblogin userid ggate password ggate
     Successfully logged into database.
    GGSCI (MachineGG1) 12> add trandata scott.inventory cols (prod_category,qty_in_stock, last_dml)
    GGSCI (MachineGG1) 13> info trandata scott.inventory
    Logging of supplemental redo log data is enabled for table SCOTT.INVENTORY.
    Columns supplementally logged for table SCOTT.INVENTORY: PROD_ID, PROD_CATEGORY, QTY_IN_STOCK, LAST_DML.

Udaljena radna površina da biste MachineGG2, otvorite Oracle GoldenGate naredba tumačenja i pokretanje:

    GGSCI (MachineGG2) 18> dblogin userid ggate password ggate
     Successfully logged into database.
    GGSCI (MachineGG2) 14> add trandata scott.inventory cols (prod_category,qty_in_stock, last_dml)
    Logging of supplemental redo data enabled for table SCOTT.INVENTORY.

Prikaz podataka o stanju razine tablice dodatni zapisivanja:

    GGSCI (MachineGG2) 15> info trandata scott.inventory
    Logging of supplemental redo log data is enabled for table SCOTT.INVENTORY.
    Columns supplementally logged for table SCOTT.INVENTORY: PROD_ID, PROD_CATEGORY, QTY_IN_STOCK, LAST_DML.

###<a name="add-trandata-on-site-a-and-site-b"></a>Dodavanje trandata na web-mjestu A i B web-mjesta

Omogućite dodatni zapisivanje na razini tablice pomoću naredbe DODAJ TRANDATA. Otvorite Oracle GoldenGate tumačenja u prozoru za naredbe, prijavite se na bazu podataka, a zatim pokrenite naredbu DODAJ TRANDATA.

Udaljena radna površina da biste MachineGG1, otvorite Oracle GoldenGate naredba tumačenja i pokretanje:

    GGSCI (MachineGG1) 11> dblogin userid ggate password ggate
     Successfully logged into database.
    GGSCI (MachineGG1) 12> add trandata scott.inventory cols (prod_category,qty_in_stock, last_dml)
    GGSCI (MachineGG1) 13> info trandata scott.inventory
    Logging of supplemental redo log data is enabled for table SCOTT.INVENTORY.
    Columns supplementally logged for table SCOTT.INVENTORY: PROD_ID, PROD_CATEGORY, QTY_IN_STOCK, LAST_DML.

Udaljena radna površina da biste MachineGG2, otvorite Oracle GoldenGate naredba prevoditelj i pokretanje:

    GGSCI (MachineGG2) 18> dblogin userid ggate password ggate
     Successfully logged into database.
    GGSCI (MachineGG2) 14> add trandata scott.inventory cols (prod_category,qty_in_stock, last_dml)
    Logging of supplemental redo data enabled for table SCOTT.INVENTORY.
    Display information about the state of table-level supplemental logging:
    GGSCI (MachineGG2) 15> info trandata scott.inventory
    Logging of supplemental redo log data is enabled for table SCOTT.INVENTORY.
    Columns supplementally logged for table SCOTT.INVENTORY: PROD_ID, PROD_CATEGORY, QTY_IN_STOCK, LAST_DML.

###<a name="start-extract-and-data-pump-processes-on-site-a"></a>Pokretanje procesa izdvojiti i Pumpa se podataka na web

Pokretanje procesa ext1 izdvojiti na A: web-mjesta

    GGSCI (MachineGG1) 31> start extract ext1
    Sending START request to MANAGER …
    EXTRACT EXT1 starting

Pokretanje dpump1 postupak za pump podataka na web-mjesta o:

    GGSCI (MachineGG1) 23> start extract dpump1
    Sending START request to MANAGER …
    EXTRACT DPUMP1 starting
Prikaz informacija o grupi ext1 izdvojiti: GGSCI (MachineGG1) 32 > informacije izdvojiti ext1 IZDVOJITI EXT1 posljednje rada 2013, 11 i 25 08:03 kašnjenja Kontrolna točka IZVODI Status 00:00:00 (ažurirane 00:00:02 prije) zapisnika čitanje Kontrolna točka Oracle ponavljanje 2013 zapisnike, 11 i 25 08:03:18 Seqno 6, SKN 3230720 RBA 0.1074371 (1074371)

Prikaz stanja i kašnjenja (pri čemu je relevantnih) za sve procese upraviteljskim, izdvojiti i Replicat u sustavu:

    GGSCI (MachineGG1) 16> info all
    Program     Status      Group       Lag at Chkpt  Time Since Chkpt

    MANAGER     RUNNING
    EXTRACT     RUNNING     DPUMP1      00:00:00      00:46:33
    EXTRACT     RUNNING     EXT1        00:00:00      00:00:04

###<a name="start-extract-and-data-pump-processes-on-site-b"></a>Pokretanje procesa izdvojiti i Pumpa se podaci na web-mjesta B

Pokretanje procesa ext2 izdvojiti na B: web-mjesta

    GGSCI (MachineGG2) 22> start extract ext2
    Sending START request to MANAGER …
    EXTRACT EXT2 starting

Pokretanje dpump2 postupak za pump podataka na web-mjesta B:

    GGSCI (MachineGG2) 23> start extract dpump2
    Sending START request to MANAGER …
    EXTRACT DPUMP2 starting

Prikaz stanja i kašnjenja (pri čemu je odgovarajuću) za sve procese upraviteljskim, izdvojiti i Replicat u sustavu:

    GGSCI (ActiveGG2orcldb) 6> info all
    Program     Status      Group       Lag at Chkpt  Time Since Chkpt

    MANAGER     RUNNING
    EXTRACT     RUNNING     DPUMP2      00:00:00      136:13:33
    EXTRACT     RUNNING     EXT2        00:00:00      00:00:04

### <a name="start-replicat-process-on-site-a"></a>Pokreni proces REPLICAT na web-mjestu odgovora

U ovom se odjeljku opisuje način da biste pokrenuli postupak REPLICAT "REP1" na web-mjestu A.

Pokreni proces Replicat na web-mjesta o:

    GGSCI (MachineGG1) 38> start replicat rep1
    Sending START request to MANAGER …
    REPLICAT REP1 starting

Prikaz stanja Replicat grupe:

    GGSCI (MachineGG1) 39> status replicat rep1
     REPLICAT REP1: RUNNING

###<a name="start-replicat-process-on-site-b"></a>Pokreni proces REPLICAT na web-mjesta B

U ovom se odjeljku opisuje način da biste započeli postupak REPLICAT "REP2" na web-mjesta B.

Pokreni proces Replicat na B: web-mjesta

    GGSCI (MachineGG2) 26> start replicat rep2
    Sending START request to MANAGER …
    REPLICAT REP2 starting

Prikaz stanja Replicat grupe:

    GGSCI (MachineGG2) 27> status replicat rep2
    REPLICAT REP2: RUNNING

##<a name="6-verify-the-bi-directional-replication-process"></a>6. Provjerite je li proces dvosmjerne replikacije

Da biste provjerili konfiguracije Oracle GoldenGate, umetanje retka u bazu podataka na web-mjesta A. udaljene radne površine na web-mjesta A. Otvori gore SQL * Plus u prozoru za naredbe i pokrenite: SQL > odaberite naziv iz baze podataka$ v;

    NAME
    ———
    TESTGG

    SQL> insert into inventory values  (100,’TV’,100,sysdate);

    1 row created.

    SQL> commit;

    Commit complete.

Zatim, potvrdite okvir ako su tom retku replicirati na web-mjesta B. Da biste to učinili, udaljene radne površine na web-mjesta B. Otvori novi prozor SQL Plus i pokretanje:

    SQL> select name from v$database;

    NAME
    ———
    TESTGG

    SQL> select * from inventory;

    PROD_ID PROD_CATEGORY QTY_IN_STOCK LAST_DML
    ———- ——————– ———— ———
    100 TV 100 22-MAR-13

Umetanje novog zapisa na web-mjesta B:

    SQL> insert into inventory  values  (101,’DVD’,10,sysdate);
    1 row created.

    SQL> commit;

    Commit complete.

Udaljena radna površina web i provjerite je li u replikacije dogodilo:

    SQL> select * from inventory;

    PROD_ID PROD_CATEGORY QTY_IN_STOCK LAST_DML
    ———- ——————– ———— ———
    100 TV 100 22-MAR-13
    101 DVD 10 22-MAR-13

##<a name="additional-resources"></a>Dodatni resursi
[Oracle virtualnog računala slike za Azure](virtual-machines-linux-classic-oracle-images.md)
