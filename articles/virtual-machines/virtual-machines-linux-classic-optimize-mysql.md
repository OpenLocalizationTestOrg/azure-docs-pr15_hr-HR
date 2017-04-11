<properties
    pageTitle="Optimiziranje performanse MySQL Linux VMs | Microsoft Azure"
    description="Saznajte kako optimizirati MySQL sustavom programa Azure virtualnog računala (VM) radi Linux."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="NingKuang"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/15/2015"
    ms.author="ningk"/>

#<a name="optimizing-mysql-performance-on-azure-linux-vms"></a>Optimiziranje performanse MySQL Azure Linux VMs

Mnogo je čimbenika koji utjecati na performanse MySQL na Azure, i u virtualne hardver odabira i softverske konfiguracije. U ovom se članku usredotočuje se na optimiziranje performanse kroz prostora za pohranu, sustava i konfiguracija baze podataka.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


##<a name="utilizing-raid-on-an-azure-virtual-machine"></a>Korištenja RAID na Azure virtualnog računala
Prostor za pohranu je ključa faktor koji utječe performanse baze podataka u oblaku okruženju.  U usporedbi s jedan disk RAID omogućuju brži pristup putem istodobnosti.  Pogledajte [Standardne RAID razine](http://en.wikipedia.org/wiki/Standard_RAID_levels) za više detalja.   

Propusnost diska/i i/i reakcija u Azure mogu znatno poboljšati kroz RAID. Naš testira Laboratorija Pokaži/disk i propusnost možete udvostručuje i/i reakcija možete smanjiti za pola u kada je broj diskova RAID udvostručuje (od 2 do 4, 4 do 8 itd.). Detalje potražite u članku [dodatak odgovora](#AppendixA) .  

Osim na disku/i performanse MySQL poboljšava kada Povećaj razinu RAID.  Detalje potražite u članku [B dodatak](#AppendixB) .  

Možda želite razmotriti veličinu bloka. Općenito govoreći kada imate veću veličinu bloka, dobit ćete donjem indirektni, posebice za velike zapisivanja. No kada bloka je prevelika, može dodati i dodatne indirektni, a ne možete koristiti prednosti na RAID. Trenutni Zadana veličina je 512KB koji je dokazano biti optimalnih za većinu Općenito radnog okruženja. Detalje potražite u članku [C dodatak](#AppendixC) .   

Napominjemo da postoje ograničenja na koliko diskova možete dodati za vrste različite virtualnog računala. Ta ograničenja detaljno opisane u [virtualnog računala i veličine servisa oblaka za Azure](http://msdn.microsoft.com/library/azure/dn197896.aspx). Iako možete odabrati da biste postavili RAID manje diskova trebat će vam 4 diskova priložene podataka za praćenje RAID primjer u ovom članku.  

U ovom se članku pretpostavlja da ste već stvorili Linux virtualnog računala i MYSQL instalacije i konfiguracije. Dodatne informacije o Uvod pogledajte kako instalirati MySQL na Azure.  

###<a name="setting-up-raid-on-azure"></a>Postavljanje RAID na Azure
Na sljedeći način prikažite kako stvoriti RAID na Azure pomoću portala za Azure klasični. Možete i postaviti RAID pomoću skripte komponente Windows PowerShell.
U ovom primjeru smo će konfigurirati RAID 0 4 diskova.  

####<a name="step-1-add-a-data-disk-to-your-virtual-machine"></a>Korak 1: Dodavanje podatkovni Disk virtualnog računala  

Na stranici portala za Azure klasični virtualnim strojevima kliknite virtualnog računala na koje želite dodati na disku podataka. U ovom se primjeru virtualnog računala je mysqlnode1.  

![][1]

Na stranici za virtualnog računala, kliknite **nadzorna ploča**.  

![][2]


Na traci zadataka, kliknite **Dodaj privitak**.

![][3]

A zatim kliknite **Dodaj privitak prazan disk**.  

![][4]

Za diskova podataka **Preferenca predmemorije glavno računalo** potrebno postaviti na **ništa**.  

Ovo će dodati jedan prazan disk u virtualnog računala. Ponovite ovaj korak tri puta tako da imate 4 diskova podataka za RAID.  

Dodane pogonima u virtualnog računala možete vidjeti tako da pogledate Evidencija poruka otklanjanje. Ako, na primjer, da biste vidjeli taj na Ubuntu, koristite sljedeću naredbu:  

    sudo grep SCSI /var/log/dmesg

####<a name="step-2-create-raid-with-the-additional-disks"></a>Korak 2: Stvaranje RAID pomoću dodatnih diskova
Slijedite ovaj članak detaljne upute za postavljanje RAID:  

[Konfiguriranje softver RAID na Linux](virtual-machines-linux-configure-raid.md)

>[AZURE.NOTE] Ako koristite XFS datotečnog sustava, slijedite korake u nastavku nakon stvaranja RAID.

Da biste instalirali XFS na Debian, Ubuntu ili Linux nadjev od mentola, koristite sljedeću naredbu:  

    apt-get -y install xfsprogs  

Da biste instalirali XFS na Fedora, CentOS ili RHEL, koristite sljedeću naredbu:  

    yum -y install xfsprogs  xfsdump


####<a name="step-3-set-up-a-new-storage-path"></a>Korak 3: Postavljanje puta novi prostor za pohranu
Koristite sljedeću naredbu:  

    root@mysqlnode1:~# mkdir -p /RAID0/mysql

####<a name="step-4-copy-the-original-data-to-the-new-storage-path"></a>Korak 4: Kopiranje izvorne podatke u novi prostor za pohranu put
Koristite sljedeću naredbu:  

    root@mysqlnode1:~# cp -rp /var/lib/mysql/* /RAID0/mysql/

####<a name="step-5-modify-permissions-so-mysql-can-access-read-and-write-the-data-disk"></a>Korak 5: Izmjena dozvola tako da možete pristupiti MySQL (za čitanje i pisanje) podatkovni disk
Koristite sljedeću naredbu:  

    root@mysqlnode1:~# chown -R mysql.mysql /RAID0/mysql && chmod -R 755 /RAID0/mysql


##<a name="adjust-the-disk-io-scheduling-algorithm"></a>Prilagodite algoritam zakazivanja disk/i
Linux implementira četiri vrste/i planiranje algoritama:  

-   Algoritam NOOP (bez operacije)
-   Krajnji rok algoritam (krajnji rok)
-   Potpuno sajma stavljanja algoritam (CFQ)
-   Proračun razdoblja algoritam (Anticipatory)  

Možete odabrati različite planiranje/i pod različitim scenarijima radi optimiziranja performansi. U okruženju potpuno izravnim pristupom nema velika razlika između algoritama CFQ i krajnji rok za performanse. Općenito preporučuje se da biste postavili okruženje za baze podataka MySQL na krajnji rok za stabilnosti. Ako postoji mnogo uzastopnih/i, CFQ može smanjiti performanse/i diska.   

SSD i ostala oprema, pomoću NOOP ili rok možete postići bolje performanse od zadani raspored.   

Iz otklanjanje 2,5 algoritam zakazivanje zadane/i je rok. Početak iz otklanjanje 2.6.18, CFQ postala algoritam zakazivanje zadane/i.  Možete navesti tu postavku prilikom pokretanja otklanjanje ili dinamički izmjena tu postavku kada je pokrenut sustav.  

Sljedeći primjer pokazuje kako provjeriti i postaviti zadani raspored na algoritam NOOP.  

Za obitelj Debian raspodjele:

###<a name="step-1view-the-current-io-scheduler"></a>Korak 1. prikaz trenutnog/i raspored
Koristite sljedeću naredbu:  

    root@mysqlnode1:~# cat /sys/block/sda/queue/scheduler

Vidjet ćete pratiti izlaz koji pokazuje trenutni raspored.  

    noop [deadline] cfq


###<a name="step-2-change-the-current-device-devsda-of-io-scheduling-algorithm"></a>Korak 2. Promjena trenutni uređaj (/ razvojni/sda) / i algoritam za planiranje rasporeda
Koristite sljedeće naredbe:  

    azureuser@mysqlnode1:~$ sudo su -
    root@mysqlnode1:~# echo "noop" >/sys/block/sda/queue/scheduler
    root@mysqlnode1:~# sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX_DEFAULT="quiet splash elevator=noop"/g' /etc/default/grub
    root@mysqlnode1:~# update-grub

>[AZURE.NOTE] Postavljanje za /dev/sda samostalno nije korisno. Ga mora biti postavljena na sve podatke diskova gdje se nalazi u bazi podataka.  

Trebali biste vidjeti sljedeće Izlaz, koji označava taj grub.cfg je uspješno izgraditi i ažurirati da alat za zakazivanje zadane NOOP.  

    Generating grub configuration file ...
    Found linux image: /boot/vmlinuz-3.13.0-34-generic
    Found initrd image: /boot/initrd.img-3.13.0-34-generic
    Found linux image: /boot/vmlinuz-3.13.0-32-generic
    Found initrd image: /boot/initrd.img-3.13.0-32-generic
    Found memtest86+ image: /memtest86+.elf
    Found memtest86+ image: /memtest86+.bin
    done

Za raspodjelu obitelj Redhat potrebni su jedino sljedeću naredbu:   

    echo 'echo noop >/sys/block/sda/queue/scheduler' >> /etc/rc.local

##<a name="configure-system-file-operations-settings"></a>Konfiguriranje postavki operacije datoteke sustava
Jedan najbolje je da biste onemogućili zapisivanje značajku atime u datotečnom sustavu. Atime je zadnji put datoteke programa access. Kad god pristupa datoteke datotečnom sustavu zapise vremenska oznaka u zapisniku. Međutim, ti podaci rijetko koristiti. Možete onemogućiti ako vam nisu potrebni, čime će znatno smanjiti Ukupno vrijeme pristup disk.  

Da biste onemogućili atime zapisivanje, ćete morati izmijeniti fstab /etc/ datoteka za konfiguraciju sustava datoteka i dodavanje **noatime** mogućnosti.  

Na primjer, uredite /etc/fstab datoteku vim u dodavanje noatime kao što je prikazano u nastavku.  

    # CLOUD_IMG: This file was created/modified by the Cloud Image build process
    UUID=3cc98c06-d649-432d-81df-6dcd2a584d41       /        ext4   defaults,discard        0 0
    #Add the “noatime” option below to disable atime logging
    UUID="431b1e78-8226-43ec-9460-514a9adf060e"     /RAID0   xfs   defaults,nobootwait, noatime 0 0
    /dev/sdb1       /mnt    auto    defaults,nobootwait,comment=cloudconfig 0       2

Nakon toga remount datotečnog sustava pomoću sljedeće naredbe:  

    mount -o remount /RAID0

Testirajte izmijenjene rezultat. Imajte na umu da kada mijenjate testiranje datoteke, vrijeme pristupa neće se ažurirati.  

Prije nego što primjer:     

![][5]

Nakon primjer:

![][6]

##<a name="increase-the-maximum-number-of-system-handles-for-high-concurrency"></a>Povećavanje maksimalni broj sustava pomoću ručica za visoke istodobnosti
MySQL je visoke istodobnosti baze podataka. Zadani broj od ručica za istodobni je 1024 za Linux, koja nije uvijek dovoljno. **Slijedite ove korake da biste povećali ručice Maksimalna Istodobni sustava za podršku visoke istodobnosti MySQL**.

###<a name="step-1-modify-the-limitsconf-file"></a>Korak 1: Izmjena limits.conf datoteka
Dodajte sljedeća četiri retka u datoteci /etc/security/limits.conf da biste povećali najveći dopušteni Istodobni ručice. Imajte na umu da 65536 najveći broj koji podržavaju sustav.   

    * meki nofile 65536
    * teško nofile 65536
    * meki nproc 65536
    * teško nproc 65536

###<a name="step-2-update-the-system-for-the-new-limits"></a>Korak 2: Ažuriranje sustava za nove ograničenja
Pokrenite sljedeće naredbe:  

    ulimit -SHn 65536
    ulimit -SHu 65536

###<a name="step-3-ensure-that-the-limits-are-updated-at-boot-time"></a>Korak 3: Provjerite ograničenja ažuriraju prilikom pokretanja
U datoteci /etc/rc.local staviti sljedeće naredbe prilikom pokretanja tako da ga stupile na snagu prilikom svakog pokretanja.  

    echo “ulimit -SHn 65536” >>/etc/rc.local
    echo “ulimit -SHu 65536” >>/etc/rc.local

##<a name="mysql-database-optimization"></a>Optimizacija baze podataka MySQL
Da biste konfigurirali MySQL Azure kao na računalu na lokalni možete koristiti isti strategije ugađanju performansi.  

Glavni/i pravila za optimizaciju su:   

-   Povećajte predmemoriju.
-   Smanjite/i reakcija.  

Da biste optimizirali MySQL postavke poslužitelja, možete ažurirati datoteke my.cnf, koja je zadana datoteka konfiguracije za poslužitelj i klijentskim računalima.  

Konfiguriranje su sljedeći glavni čimbenici koji utječu na performanse MySQL:  

-   **innodb_buffer_pool_size**: sadrži skupinu međuspremnika u međuspremniku podataka i indeks. To je obično postavljeno na 70% fizičke memorije.
-   **innodb_log_file_size**: to je veličina zapisnika Ponovi poništeno. Pomoću zapisnika Ponovi poništeno operacije pisanja provjerite jesu li brzo pouzdanog i koje se mogu vratiti nakon rušenja. Postavljen je na 512MB, što će vam mnogo prostora za bilježenje operacije pisanja.
-   **max_connections**: ponekad aplikacija ne zatvorite veze ispravno. Veća vrijednost steći poslužitelj još jedanput u koš za idled veze. Maksimalan broj veza je 10000, ali preporučuje najviše 5000.
-   **Innodb_file_per_table**: ta postavka omogućiti ili onemogućiti InnoDB za spremanje tablice u zasebne datoteke. Uključite mogućnost će biti sigurni da nekoliko postupaka napredne Administracija mogu primijeniti učinkovito. S performansama točku prikaza, može brzinu prijenosa prostora tablice i optimiziranja performansi upravljanje debris. Dakle preporučena postavka je Uključeno.</br>
    Iz MySQL 5.6, zadana je postavka Uključeno. Zbog toga ne poduzmete potreban je. Za verzije na drugim, koja je starija od 5.6, zadane postavke je ISKLJUČENA. Potreban je uključivanjem ovaj Uključeno. I primjene ga prije učitan podataka jer utječe samo novostvorenu tablice.
-   **innodb_flush_log_at_trx_commit**: Zadana je vrijednost 1 s opseg postavljeno na 0 ~ 2. Zadana vrijednost je najčešće odgovarajuću mogućnost za samostalne baze podataka MySQL. Postavka 2 omogućuje Većina integriteta podataka te je prikladna za matricu u skupini MySQL. Postavka 0 omogućuje gubitka podataka koji mogu utjecati na pouzdanosti u nekim slučajevima s boljih performansi te je prikladna za podređeni klaster MySQL.
-   **Innodb_log_buffer_size**: međuspremnik zapisnika omogućuje transakcije da se pokreće bez potrebe za pražnjenje zapisnik disk prije potvrđivanja transakcije. Međutim, ako postoji velik binarni objekt ili tekstno polje, predmemorije će biti vrlo brzo potrošena i/Česti disku i će se pokrenuti. To je bolje povećati veličinu međuspremnika ako Innodb_log_waits stanje varijabla nije 0.
-   **query_cache_size**: najbolja je mogućnost onemogućivanja iz na outset. Postavite query_cache_size na 0 (to je sada zadana postavka u MySQL 5.6), a zatim koristiti druge metode da biste ubrzali upita.  

Potražite [Dodatak D](#AppendixD) Usporedba performanse nakon optimizaciju.


##<a name="turn-on-the-mysql-slow-query-log-for-analyzing-the-performance-bottleneck"></a>Uključivanje zapisnika sporo upita MySQL za analizu usko grlo performansi
Zapisnik MySQL sporo upita možete lakše prepoznali sporo upite za MySQL. Nakon omogućavanja zapisnik MySQL sporo upita, možete koristiti alate za MySQL kao što su **mysqldumpslow** za prepoznavanje usko grlo performansi.  

Napominjemo da po zadanom to nije omogućeno. Uključivanje zapisnika sporo upita mogu zauzeti neke procesora resurse. Dakle, preporučuje se da omogućite to privremeno za otklanjanje poteškoća s performansama grla.

###<a name="step-1-modify-mycnf-file-by-adding-the-following-lines-to-the-end"></a>Korak 1: Izmjena datoteka my.cnf dodavanjem sljedeće retke do kraja   

    long_query_time = 2
    slow_query_log = 1
    slow_query_log_file = /RAID0/mysql/mysql-slow.log

###<a name="step-2-restart-mysql-server"></a>Korak 2: Ponovno pokrenite poslužitelj mysql
    service  mysql  restart

###<a name="step-3-check-whether-the-setting-is-taking-effect-using-the-show-command"></a>Korak 3: Provjerite je li postavka traje efekt pomoću naredbe "prikaz"

![][7]   

![][8]

U ovom primjeru, vidjet ćete da značajka sporo upita je uključen. Zatim možete koristiti alat **mysqldumpslow** da bi utvrdio grla performanse i optimiziranja performansi, kao što su dodavanje indeksa.





##<a name="appendix"></a>Dodatak

Slijede uzorka test podataka o performansama sastavio u okruženju ciljano Laboratorija, omogućuju Općenito pozadine na performanse trend podataka s različitim performansama usklađivanje pristupa, no rezultati se mogu razlikovati u različitim verzijama okruženje ili proizvoda.

<a name="AppendixA"></a>Dodatak A:  
**Performanse diska (IOPS) s različitim RAID razina**


![][9]

**Testiranje naredbe:**  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=5G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite

>AZURE. Napomena: Povećavaju ovaj test koristi 64 niti pokušaju kontaktirati gornju granicu RAID.

<a name="AppendixB"></a>Dodatak B:  
**Usporedba performansi (propusnost) MySQL s RAID različite razine**   
(XFS datotečni sustav)


![][10]  
![][11]

**Testiranje naredbe:**

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb

**Usporedba performansi (OLTP) MySQL s različitim RAID razine**  
![][12]

**Testiranje naredbe:**

    time sysbench --test=oltp --db-driver=mysql --mysql-user=root --mysql-password=0ps.123  --mysql-table-engine=innodb --mysql-host=127.0.0.1 --mysql-port=3306 --mysql-socket=/var/run/mysqld/mysqld.sock --mysql-db=test --oltp-table-size=1000000 prepare

<a name="AppendixC"></a>Dodatak C:   
**Usporedba performansi (IOPS) na disku za različite bloka**  
(XFS datotečni sustav)


![][13]

**Testiranje naredbe:**  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=30G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite
    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=1G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite  

Imajte na umu veličinu datoteke koje se koriste za testiranje 30GB i 1GB odnosno, s RAID 0(4 disks) XFS polje sustava.


<a name="AppendixD"></a>Dodatak D:  
**Usporedba performansi (propusnost) MySQL prije i poslije optimizacije**  
(XFS datotečni sustav)


![][14]

**Testiranje naredbe:**

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb,misam

**Postavke konfiguracije za zadane i optmization je na sljedeći način:**

|Parametri |Zadani    |optmization
|-----------|-----------|-----------
|**innodb_buffer_pool_size**    |Ništa   |7G
|**innodb_log_file_size**   |5M |512M
|**max_connections**    |100    |5000
|**innodb_file_per_table**  |0  |1
|**innodb_flush_log_at_trx_commit** |1  |2
|**innodb_log_buffer_size** |8M |128M
|**query_cache_size**   |16M    |0


Dodatne informacije i detaljnije parametara konfiguracije optimizacije, pogledajte upute službeni mysql.  

[http://dev.MySQL.com/doc/refman/5.6/en/innodb-Configuration.HTML](http://dev.mysql.com/doc/refman/5.6/en/innodb-configuration.html)  

[http://dev.MySQL.com/doc/refman/5.6/en/innodb-PARAMETERS.HTML#sysvar_innodb_flush_method](http://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html#sysvar_innodb_flush_method)

**Okruženje za testiranje**  

|Hardvera   |Pojedinosti
|-----------|-------
|Procesor    |AMD Opteron(tm) procesor 4171 HE / 4 jezgri
|Memorije |14G
|na disku   |10G/disk
|os |Ubuntu 14.04.1 LTS



[1]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-01.png
[2]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-02.png
[3]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-03.png
[4]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-04.png
[5]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-05.png
[6]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-06.png
[7]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-07.png
[8]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-08.png
[9]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-09.png
[10]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-10.png
[11]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-11.png
[12]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-12.png
[13]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-13.png
[14]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-14.png
