<properties
    pageTitle="MariaDB (MySQL) klaster sustavom Azure"
    description="Stvaranje na MariaDB + Galera MySQL skupine virtualnim računalima sustava na Azure"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="sabbour"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="04/15/2015"
    ms.author="v-ahsab"/>

# <a name="mariadb-mysql-cluster---azure-tutorial"></a>Klaster MariaDB (MySQL) – vodič za Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

> [AZURE.NOTE]  MariaDB Enterprise klaster sada je dostupan u Azure Marketplace.  Novi nuditi će automatski implementirati MariaDB Galera klaster okvira. Trebali biste koristiti nove ponude iz https://azure.microsoft.com/en-us/marketplace/partners/mariadb/cluster-maxscale/ 

Ne možemo stvarate na više osnovne [Galera](http://galeracluster.com/products/) klaster [MariaDBs](https://mariadb.org/en/about/), robusne, skalabilni i pouzdan Upadajući zamjena za MySQL za rad u okruženju iznimno dostupna na virtualnim strojevima sa sustavom Azure.

## <a name="architecture-overview"></a>Pregled arhitekture

U ovoj se temi izvršava sljedeće korake:

1. Stvaranje 3 čvor klaster
2. Razdvajanje diskova podataka s operacijskim Sustavom diska
3. Stvaranje diskova podataka u RAID-0/Prugasta postavke da biste povećali IOPS
4. Pomoću Azure raspoređivača opterećenja saldo opterećenje za čvorove 3
5. Da biste minimizirali rada koji se ponavljaju, stvorite VM sliku koja sadrži MariaDB + Galera te ga koristiti da biste stvorili druge skupine VMs.

![Arhitektura](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Setup.png)

> [AZURE.NOTE]  U ovoj se temi koristi Alati za [Azure EŽA](../xplat-cli-install.md) , pa provjerite da biste ih preuzeli i povezati pretplate Azure prema uputama. Ako vam je potrebna referenca naredbe koje su dostupne u EŽA Azure, pogledajte ovu vezu da biste [reference Azure EŽA naredbi](../virtual-machines-command-line-tools.md). Ćete morati [stvoriti ključa SSH za provjeru autentičnosti] i zabilježite **.pem mjesto datoteke**.


## <a name="creating-the-template"></a>Stvaranje predloška

### <a name="infrastructure"></a>Infrastruktura

1. Stvaranje grupe sustava afinitet zajedno držanje resursi

        azure account affinity-group create mariadbcluster --location "North Europe" --label "MariaDB Cluster"

2. Stvaranje virtualne mreže

        azure network vnet create --address-space 10.0.0.0 --cidr 8 --subnet-name mariadb --subnet-start-ip 10.0.0.0 --subnet-cidr 24 --affinity-group mariadbcluster mariadbvnet

3. Stvaranje računa za pohranu za hostiranje naš diskova. Napomena da koje bi trebalo biti smještanje više od 40 intenzivnog koristiti diskova na isti račun za pohranu da biste izbjegli odlazak 20 000 račun ograničenje prostora za pohranu IOPS. U ovom slučaju Ispričavamo se daleko iz taj broj pa ćemo ćete sve pohranite na isti račun za jednostavnosti

        azure storage account create mariadbstorage --label mariadbstorage --affinity-group mariadbcluster

3. Pronalaženje naziva slika CentOS 7 virtualnog računala

        azure vm image list | findstr CentOS
To će otprilike ovako izlaz `5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926`. Korištenje naziva u sljedećem koraku.

4. Stvaranje predloška VM zamjena **/path/to/key.pem** put pohranjuju SSH tipku generirani .pem

        azure vm create --virtual-network-name mariadbvnet --subnet-names mariadb --blob-url "http://mariadbstorage.blob.core.windows.net/vhds/mariadbhatemplate-os.vhd"  --vm-size Medium --ssh 22 --ssh-cert "/path/to/key.pem" --no-ssh-password mariadbtemplate 5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926 azureuser

5. Prilaganje 4 x 500GB podataka diskova VM za korištenje u konfiguraciji RAID

        FOR /L %d IN (1,1,4) DO azure vm disk attach-new mariadbhatemplate 512 http://mariadbstorage.blob.core.windows.net/vhds/mariadbhatemplate-data-%d.vhd

6. SSH u predložak VM koji ste stvorili na **mariadbhatemplate.cloudapp.net:22** i povezati pomoću privatni ključ.

### <a name="software"></a>Softver

1. Nabavite korijen

        sudo su

2. Instaliranje RAID podrške:

     - Instalacija mdadm

                yum install mdadm

     - Stvaranje konfiguracije RAID0/pruga s s datotečnim sustavom EXT4

                mdadm --create --verbose /dev/md0 --level=stripe --raid-devices=4 /dev/sdc /dev/sdd /dev/sde /dev/sdf
                mdadm --detail --scan >> /etc/mdadm.conf
                mkfs -t ext4 /dev/md0

     - Stvaranje direktorija točke postavljanja

                mkdir /mnt/data

     - Dohvaćanje UUID novostvorenu RAID uređaja

                blkid | grep /dev/md0

     - Uređivanje /etc/fstab

                vi /etc/fstab

     - Dodavanje uređaja u njega da biste omogućili automatsko mouting nakon ponovnog pokretanja zamjenjuju na UUID vrijednost dobivenog od naredbe **blkid** prije

                UUID=<UUID FROM PREVIOUS>   /mnt/data ext4   defaults,noatime   1 2

     - Postavljanje nove particije

                mount /mnt/data

3. Instalirajte MariaDB:

     - Stvaranje datoteke MariaDB.repo:

                vi /etc/yum.repos.d/MariaDB.repo

     - Ispunite ga s u nastavku sadržaja

                [mariadb]
                name = MariaDB
                baseurl = http://yum.mariadb.org/10.0/centos7-amd64
                gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
                gpgcheck=1

     - Uklonite postojeće postfix i mariadb libs da biste izbjegli sukobe

            yum remove postfix mariadb-libs-*

     - Instalacija MariaDB sa Galera

            yum install MariaDB-Galera-server MariaDB-client galera

4. Premještanje direktoriju podataka MySQL s uređajem RAID bloka

     - Kopirajte trenutnog direktorija MySQL u novom položaju i uklonili stare direktorija

            cp -avr /var/lib/mysql /mnt/data  
            rm -rf /var/lib/mysql

     - Postavljanje dozvola za novi direktorij sukladno tome

            chown -R mysql:mysql /mnt/data && chmod -R 755 /mnt/data/  

     - Stvaranje symlink pokazivanjem stare direktorija na novo mjesto na RAID

            ln -s /mnt/data/mysql /var/lib/mysql

5. Budući da [će SELinux ometati operacije klaster](http://galeracluster.com/documentation-webpages/configuration.html#selinux), potrebno je da biste onemogućili za trenutnoj sesiji (dok se ne pojavi kompatibilna verzija). Uređivanje `/etc/selinux/config` da biste onemogućili za kasnije ponovnog pokretanja:

            setenforce 0

       then editing `/etc/selinux/config` to set `SELINUX=permissive`

6. Provjerite valjanost pokreće MySQL

    - Pokretanje MySQL

            service mysql start

    - Sigurne MySQL instalaciju, korijenski lozinke, uklonite anonimnim korisnicima onemogućite prijavu udaljene korijenski i uklanjanje testiranje baze podataka

            mysql_secure_installation

    - Stvaranje korisnika na bazu podataka za klaster operacije i, Neobavezno, aplikacija

            mysql -u root -p
            GRANT ALL PRIVILEGES ON *.* TO 'cluster'@'%' IDENTIFIED BY 'p@ssw0rd' WITH GRANT OPTION; FLUSH PRIVILEGES;
            exit

   - Zaustavljanje MySQL

            service mysql stop

7. Stvaranje rezervirano mjesto za konfiguraciju

    - Uređivanje konfiguracije MySQL da biste stvorili rezervirano mjesto za postavke klaster. Zamjena na **`<Vairables>`** ili uklonite sada. Koje će se dogoditi kada ćemo stvoriti na VM iz ovog predloška.

            vi /etc/my.cnf.d/server.cnf

    - Uređivanje u odjeljku **[galera]** i isključite

    - Uređivanje u odjeljku **[mariadb]**

            wsrep_provider=/usr/lib64/galera/libgalera_smm.so
            binlog_format=ROW
            wsrep_sst_method=rsync
            bind-address=0.0.0.0 # When set to 0.0.0.0, the server listens to remote connections
            default_storage_engine=InnoDB
            innodb_autoinc_lock_mode=2

            wsrep_sst_auth=cluster:p@ssw0rd # CHANGE: Username and password you created for the SST cluster MySQL user
            #wsrep_cluster_name='mariadbcluster' # CHANGE: Uncomment and set your desired cluster name
            #wsrep_cluster_address="gcomm://mariadb1,mariadb2,mariadb3" # CHANGE: Uncomment and Add all your servers
            #wsrep_node_address='<ServerIP>' # CHANGE: Uncomment and set IP address of this server
            #wsrep_node_name='<NodeName>' # CHANGE: Uncomment and set the node name of this server

8. Otvaranje potreban priključaka na vatrozidu (pomoću FirewallD na CentOS 7)

    - MySQL:`firewall-cmd --zone=public --add-port=3306/tcp --permanent`
    - GALERA:`firewall-cmd --zone=public --add-port=4567/tcp --permanent`
    - NEMATE GALERA:`firewall-cmd --zone=public --add-port=4568/tcp --permanent`
    - RSYNC:`firewall-cmd --zone=public --add-port=4444/tcp --permanent`
    - Ponovno učitaj vatrozida:`firewall-cmd --reload`

9.  Optimiziranje sustava performanse. Pogledajte ovaj članak na [Strategije za ugađanje performansi](virtual-machines-linux-classic-optimize-mysql.md) za dodatne informacije

    - Ponovno uredili MySQL konfiguracijska datoteka

            vi /etc/my.cnf.d/server.cnf

    - Uređivanje u odjeljku **[mariadb]** i dodavati u nastavku

    > [AZURE.NOTE] Preporučuje **innodb\_međuspremnika\_pool_size** biti 70% vaše VM memorije. Što je postavljena na ovdje 2.45GB za Azure VM srednje s 3,5 GB RAM-a.

            innodb_buffer_pool_size = 2508M # The buffer pool contains buffered data and the index. This is usually set to 70% of physical memory.
            innodb_log_file_size = 512M #  Redo logs ensure that write operations are fast, reliable, and recoverable after a crash
            max_connections = 5000 # A larger value will give the server more time to recycle idled connections
            innodb_file_per_table = 1 # Speed up the table space transmission and optimize the debris management performance
            innodb_log_buffer_size = 128M # The log buffer allows transactions to run without having to flush the log to disk before the transactions commit
            innodb_flush_log_at_trx_commit = 2 # The setting of 2 enables the most data integrity and is suitable for Master in MySQL cluster
            query_cache_size = 0

10. Zaustavite MySQL, onemogućivanje servisa MySQL pokretanje na pokretanje da biste izbjegli upropaštavanja klaster prilikom dodavanja nove čvor i deprovision na računalu.

        service mysql stop
        chkconfig mysql off
        waagent -deprovision

11. Snimite VM putem portala sustava. (Trenutno [problem #1268 u EŽA Azure] Alati opisuju fact slike zabilježene pomoću alata za Azure EŽA snimiti priložene podataka diskova.)

    - Isključivanje računala putem portala sustava
    - Kliknite na snimanje i navedite naziv slike kao **mariadb galera sliku** i unesite opis i provjerite "Ste pokretanja waagent".
    ![Snimite virtualnog računala](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Capture.png)
    ![snimiti virtualnog računala](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Capture2.PNG)

## <a name="creating-the-cluster"></a>Stvaranje klaster

Stvaranje 3 VMs iz predloška koji upravo stvorili i zatim konfiguriranje i klaster.

1. Stvaranje prve VM 7 CentOS iz slike **mariadb galera slike** koju ste stvorili, pod uvjetom naziv virtualne mreže **mariadbvnet** i podmreže **mariadb**na računalu veličina **Srednje**, prosljeđivanje u servis u Oblaku naziv biti **mariadbha** (ili naziv koji želite pristupiti putem mariadbha.cloudapp.net) postavljanje ime ovog računala **mariadb1** i korisničko ime na kojem se **azureuser**i omogućivanje pristupa SSH i prosljeđivanje u SSH certifikata .pem datoteke i zamjene **/path/to/key.pem** putom gdje ste spremljeni SSH tipku generirani .pem.

    > [AZURE.NOTE] Naredbe u nastavku podijelit će se preko više redaka za prikaz, no svaki kao jedan redak unesite.

        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 22
        --vm-name mariadb1
        mariadbha mariadb-galera-image azureuser

2. Stvaranje 2 više virtualnim strojevima _povezivanjem_ im trenutno stvoren **mariadbha** servis u Oblaku, promjenom **VM naziv** , kao i **SSH priključak** jedinstveni priključak ne sukobu s drugim VMs u istom servis u Oblaku.

        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 23
        --vm-name mariadb2
        --connect mariadbha mariadb-galera-image azureuser
kao i za MariaDB3

        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 24
        --vm-name mariadb3
        --connect mariadbha mariadb-galera-image azureuser

3. Morat ćete dobiti internu IP adresu svake od 3 VMs za sljedeći korak:

    ![Dohvaćanje IP adrese](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/IP.png)

4. SSH u 3 VMs i i uređivanje datoteka konfiguracije u svakom

        sudo vi /etc/my.cnf.d/server.cnf

    uncommenting **`wsrep_cluster_name`** i **`wsrep_cluster_address`** uklanjanjem na **#** na početku i provjere valjanosti su uistinu ono što želite.
    Uz to, zamijenite **`<ServerIP>`** u **`wsrep_node_address`** i **`<NodeName>`** u **`wsrep_node_name`** s na VM IP adresa i naziva odnosno i uklonite one crte.

5. Pokretanje klaster na MariaDB1 i pričekajte pri pokretanju

        sudo service mysql bootstrap
        chkconfig mysql on

6. Pokretanje MySQL na MariaDB2 i MariaDB3 i pričekajte pri pokretanju

        sudo service mysql start
        chkconfig mysql on

## <a name="load-balancing-the-cluster"></a>Klaster za ujednačavanje opterećenja
Prilikom stvaranja grupirani VMs dodao ih u Availablity skup naziva **clusteravset** provjerite je li ih smještaju se na različitim kvara i ažuriranje domene i da Azure nikad ne održavanja na svim računalima odjednom. Tu konfiguraciju zadovoljava preduvjete za podržavati tako da tom Azure servisa razinu ugovor (SLA).

Sada pomoću opterećenja Azure saldo zahtjeva između naš 3 čvorove.

Pokretanje sustava ispod naredbi na vašem računalu pomoću EŽA Azure.
Struktura parametara naredba je:`azure vm endpoint create-multiple <MachineName> <PublicPort>:<VMPort>:<Protocol>:<EnableDirectServerReturn>:<Load Balanced Set Name>:<ProbeProtocol>:<ProbePort>`

    azure vm endpoint create-multiple mariadb1 3306:3306:tcp:false:MySQL:tcp:3306
    azure vm endpoint create-multiple mariadb2 3306:3306:tcp:false:MySQL:tcp:3306
    azure vm endpoint create-multiple mariadb3 3306:3306:tcp:false:MySQL:tcp:3306

Na kraju, budući da u EŽA postavlja interval probni raspoređivača opterećenja 15 sekundi (koje se mogu malo predugačak), ga promijenite u portal u odjeljku **krajnje točke** za sve u VMs

![Uređivanje krajnje točke](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Endpoint.PNG)

zatim kliknite Postavi Reconfigure The Load-Balanced i ići

![Konfigurirajte učitavanje Balansirani skup](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Endpoint2.PNG)

zatim promijenite intervala koji isprobati 5 sekundi i spremanje

![Promjena probni Interval](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Endpoint3.PNG)

## <a name="validating-the-cluster"></a>Provjera valjanosti klaster

Unos uložili mnogo truda obavlja. Klaster mora biti odmah dostupna na `mariadbha.cloudapp.net:3306` koji će kliknite raspoređivača opterećenja i usmjeravanje zahtjeva između 3 VMs provođenju i učinkovitije.

Pomoću omiljene MySQL klijent za povezivanje ili samo povezivanje neku od VMs da biste provjerili funkcionira ovoj grupi.

     mysql -u cluster -h mariadbha.cloudapp.net -p

Zatim stvorite novu bazu podataka i popunjavanje sadrži podatke

    CREATE DATABASE TestDB;
    USE TestDB;
    CREATE TABLE TestTable (id INT NOT NULL AUTO_INCREMENT PRIMARY KEY, value VARCHAR(255));
    INSERT INTO TestTable (value)  VALUES ('Value1');
    INSERT INTO TestTable (value)  VALUES ('Value2');
    SELECT * FROM TestTable;

Rezultirat će u tablici u nastavku

    +----+--------+
  	| id | value  |
    +----+--------+
  	|  1 | Value1 |
  	|  4 | Value2 |
    +----+--------+
    2 rows in set (0.00 sec)

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Daljnji koraci

U ovom se članku koji ste stvorili u 3 čvor MariaDB + Galera iznimno dostupna klaster na Azure virtualnim strojevima sa sustavom CentOS 7. Na VMs su rasporediti s Azure raspoređivača opterećenja opterećenje.

Preporučujemo vam da razmotrite [ukazuju na klaster MySQL na Linux](virtual-machines-linux-classic-mysql-cluster.md) i načina za [optimiziranje i testiranje MySQL performanse Azure Linux VMs](virtual-machines-linux-classic-optimize-mysql.md).

<!--Anchors-->
[Architecture overview]: #architecture-overview
[Creating the template]: #creating-the-template
[Creating the cluster]: #creating-the-cluster
[Load balancing the cluster]: #load-balancing-the-cluster
[Validating the cluster]: #validating-the-cluster
[Next steps]: #next-steps

<!--Image references-->

<!--Link references-->
[Galera]: http://galeracluster.com/products/
[MariaDBs]: https://mariadb.org/en/about/
[Stvaranje ključa SSH za provjeru autentičnosti]:http://www.jeff.wilcox.name/2013/06/secure-linux-vms-with-ssh-certificates/
[problem #1268 u EŽA Azure]:https://github.com/Azure/azure-xplat-cli/issues/1268
