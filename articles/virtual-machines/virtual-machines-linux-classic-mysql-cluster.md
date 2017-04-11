<properties
    pageTitle="Clusterize MySQL s uravnoteženja skupovima | Microsoft Azure"
    description="Postavljanje dostupnosti uravnoteženja, Visoko Linux MySQL klaster stvorene pomoću model klasični implementacije na Azure"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="bureado"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/14/2015"
    ms.author="jparrel"/>

# <a name="using-load-balanced-sets-to-clusterize-mysql-on-linux"></a>Korištenje uravnoteženja skupovi za clusterize MySQL na Linux

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Svrha u ovom se članku je da biste istražili i ilustrirali različite dostupne za implementaciju iznimno raspoloživim servisima Linux utemeljen na Microsoft Azure Istraživanje poslužitelj MySQL visoke dostupnosti kao na primer pristupa. Videozapis ilustraciju takvog dostupna je na [kanal 9](http://channel9.msdn.com/Blogs/Open/Load-balancing-highly-available-Linux-services-on-Windows-Azure-OpenLDAP-and-MySQL).

Ne možemo strukture zajednički nothing dva čvor jednu matricu MySQL visoke dostupnosti rješenje na temelju DRBD, Corosync i Pacemaker. Samo jedan čvor radi MySQL odjednom. Čitanje i pisanje DRBD resursa i ograničeno je na čvor samo jedan po jedan.

Nema potrebe za VIP rješenje kao što su LVS jer omogućujemo Microsoft Azure učitavanja raspoređen skupova za funkciju kružnog i otkrivanje krajnje točke, uklanjanje i graceful oporavak na VIP. Na VIP je globalno usmjeravati IPv4 adresa dodijelio Microsoft Azure kada smo stvorili servisa u oblaku.

Dostupne su neke druge moguće arhitekturi za MySQL uključujući NBD klaster, Percona i Galera kao i nekoliko rješenja proizvod, uključujući barem jedan kao VM na [VM Depot](http://vmdepot.msopentech.com). Kao ova rješenja možete replicirati na jednosmjernog nasuprot višesmjernog ili emitiranje i nemojte koristiti zajednički prostor za pohranu ili više mreža sučelja, scenariji mora biti lakše implementirati na Microsoft Azure.

Naravno te klasteriranja arhitekturi mogu se proširiti ostale proizvode kao što je PostgreSQL i OpenLDAP na sličan način. Ako, na primjer, ovaj postupak za ujednačavanje sa zajedničkom nothing opterećenja uspješno je testirati s više matrica OpenLDAP, a koje možete gledati na blogu našu 9 kanala.

## <a name="getting-ready"></a>Priprema

Potrebno je račun za Microsoft Azure s valjanu pretplatu moći stvoriti barem dva (2) VMs (o. koristila u ovom primjeru), na mrežu i podmreži, granu afinitet i raspoloživost postaviti, kao i mogućnost da biste stvorili novi VHDs u području isti kao servisa u oblaku, a da biste priložili Linux VMs.

### <a name="tested-environment"></a>Okruženje za testirano

- Ubuntu 13.10
  - DRBD
  - Poslužitelj MySQL
  - Corosync i Pacemaker

### <a name="affinity-group"></a>Afinitet grupe

Grupe sustava afinitet rješenja je stvorio zapisivanje u Azure klasični portala klizanje prema dolje do postavki i stvaranje nove grupe afinitet. Dodijeljeno resursa koje su stvorene kasnije će se dodijeliti ovu grupu afinitet.

### <a name="networks"></a>Mreža

Stvara novu mrežu i podmreži će se stvoriti u mreži. Ne možemo odabrali 10.10.10.0/24 mreža uz samo jedan /24 podmreže unutar.

### <a name="virtual-machines"></a>Virtualnim strojevima

Prvi VM 13.10 Ubuntu stvoren pomoću galerije Ubuntu Endorsed sliku, a naziva `hadb01`. U tijeku, pod nazivom hadb stvara se novi servis u oblaku. Ne možemo ga pozovite taj način da biste ilustrirali prirode zajedničke, uravnoteženja koji će se servis imati kada ćemo dodavati još resursa. Stvaranje `hadb01` je uneventful i dovršenim pomoću portala za. Krajnje točke za SSH automatski se stvara i našu mrežu stvoren je odabran. Ne možemo odabrati da biste stvorili novi dostupnosti za na VMs.

Nakon stvaranja prvog VM (Tehnički, prilikom stvaranja servisa u oblaku) ne možemo nastaviti da biste stvorili drugi VM `hadb02`. Za drugi VM i koristit ćemo Ubuntu 13.10 VM iz galerije pomoću portala, ali ne možemo će odaberite da biste koristili postojeći oblaka servis `hadb.cloudapp.net`, umjesto stvaranja novog. Postavljanje mreže i dostupnost mora biti automatski označena za us. Krajnje SSH stvorit će se, previše.

Nakon stvaranja i VMs ćemo će uzeti u obzir SSH priključak za `hadb01` (TCP 22) i `hadb02` (automatski dodijeljeni po Azure)

### <a name="attached-storage"></a>Spremanje privitka

Radimo novi disk priložiti oba VMs i stvoriti nove diskova 5 GB u postupku. Na diskova će se nalaziti u spremniku VHD koristi za naše diskova glavni operacijski sustav. Kada se stvara i priložene diskova nema potrebe za nam da biste ponovno pokrenuli Linux kao Jezgra sustava prikazat će se na novi uređaj (obično `/dev/sdc`, možete provjeriti `dmesg` za izlaz)

Na svakom VM ne možemo nastaviti da biste stvorili novi particija pomoću `cfdisk` (primarni, Linux particija) i napišite novu tablicu particije. **Stvaranje datotečnom sustavu na particija** .

## <a name="setting-up-the-cluster"></a>Postavljanje klaster

U oba VMs Ubuntu moramo omogućuje instalaciju Corosync, Pacemaker i DRBD APT. Korištenje `apt-get`:

    sudo apt-get install corosync pacemaker drbd8-utils.

**Instalacija MySQL trenutno** . Debian i Ubuntu instalacije skripte će pokrenuti u direktoriju podataka MySQL na `/var/lib/mysql`, ali potrebna jer će imenika zamijenila datotečnom sustavu DRBD, to kasnije.

Sada ćemo treba provjeriti i (pomoću `/sbin/ifconfig`) i VMs koristite adrese u podmreže 10.10.10.0/24 i da ih možete pomoću naredbe ping međusobno po nazivu. Po želji možete koristiti `ssh-keygen` i `ssh-copy-id` da biste bili sigurni oba VMs možete započeti komunikaciju SSH bez lozinke.

### <a name="setting-up-drbd"></a>Postavljanje DRBD

Ćemo će stvoriti DRBD resurs koji se koristi u podlozi `/dev/sdc1` particija čime se dobiva na `/dev/drbd1` resursa može biti oblikovana pomoću ext3 i koristiti u primarnih i sekundarnih čvorove. Da biste to učinili, otvorite `/etc/drbd.d/r0.res` i kopirajte sljedeću definiciju resursa. U oba VMs, učinite sljedeće:

    resource r0 {
      on `hadb01` {
        device  /dev/drbd1;
        disk   /dev/sdc1;
        address  10.10.10.4:7789;
        meta-disk internal;
      }
      on `hadb02` {
        device  /dev/drbd1;
        disk   /dev/sdc1;
        address  10.10.10.5:7789;
        meta-disk internal;
      }
    }

Kada to učinite, pokretanje pomoću resursa `drbdadm` u oba VMs:

    sudo drbdadm -c /etc/drbd.conf role r0
    sudo drbdadm up r0

I na kraju, primarnih (`hadb01`) prisilno vlasništva (primarni) DRBD resursa:

    sudo drbdadm primary --force r0

Ako pregledate sadržaj/procedura/drbd (`sudo cat /proc/drbd`) na oba VMs, trebali biste vidjeti `Primary/Secondary` na `hadb01` i `Secondary/Primary` na `hadb02`dosljedan rješenjem sada. 5 GB disku sinkronizirat će se putem mreže 10.10.10.0/24 bez troškova klijentima.

Kada se sinkronizira disk na datotečnom sustavu možete stvoriti na `hadb01`. Za testiranje koristi ext2, ali sljedećih uputa stvorit će se ext3 datotečnom sustavu:

    mkfs.ext3 /dev/drbd1

### <a name="mounting-the-drbd-resource"></a>Postavljanje DRBD resursa

Na `hadb01` ćemo sada spremni ste za postavljanje DRBD resursi. Korištenje debian i Izvedenice `/var/lib/mysql` kao direktoriju podataka MySQL korisnika. Jer smo još niste instalirali MySQL, ne možemo ćete stvoriti imenik i dostupnosti DRBD resursa. On `hadb01`:

    sudo mkdir /var/lib/mysql
    sudo mount /dev/drbd1 /var/lib/mysql

## <a name="setting-up-mysql"></a>Postavljanje MySQL

Sada ste spremni instalirati MySQL na `hadb01`:

    sudo apt-get install mysql-server

Za `hadb02`, imate dvije mogućnosti. Možete instalirati mysql-poslužitelj sada koji će stvoriti /var/lib/mysql i Ispuna s novi direktoriju podataka, a zatim nastavite da biste uklonili sadržaj. On `hadb02`:

    sudo apt-get install mysql-server
    sudo service mysql stop
    sudo rm –rf /var/lib/mysql/*

Je prebacivanje na drugu mogućnost `hadb02` , a zatim instalirati mysql-poslužitelja postoji (instalacije skripte Primijetit ćete postojeće instalacije i nećete dodirne)

On `hadb01`:

    sudo drbdadm secondary –force r0

On `hadb02`:

    sudo drbdadm primary –force r0
    sudo apt-get install mysql-server

Ako ne namjeravate prebacivanje DRBD sada, jednostavnije iako arguably manje elegantni je prvu mogućnost. Nakon postavljanja to, možete početi s radom na baze podataka MySQL. Na `hadb02` (ili bez obzira poslužitelja je od njih aktivan, prema DRBD):

    mysql –u root –p
    CREATE DATABASE azureha;
    CREATE TABLE things ( id SERIAL, name VARCHAR(255) );
    INSERT INTO things VALUES (1, "Yet another entity");
    GRANT ALL ON things.\* TO root;

**Upozorenje**: izjavu o zaštiti posljednje onemogućuje učinkovito provjere autentičnosti korisnika korijenski u ovoj tablici. To treba zamijeniti tako da vaš radni klase DOPUSTITI naredbe i uključen samo u svrhu opisne.

Morate omogućiti umrežavanje za MySQL ako želite da bude upita iz izvan VMs, koja je svrha ovaj vodič. Na oba VMs otvorite `/etc/mysql/my.cnf` i pronađite `bind-address`, promjena iz 127.0.0.1 u 0.0.0.0. Kada spremite datoteku, problema s `sudo service mysql restart` na vaš trenutni primarni.

### <a name="creating-the-mysql-load-balanced-set"></a>Stvaranje opterećenje MySQL raspoređen skup

Ne možemo će vratite se na portal i pronađite u `hadb01` VM, a zatim krajnje točke. Ne možemo će stvorili krajnju točku, odaberite MySQL (TCP 3306) s padajućeg izbornika i crtičnih u okviru *Stvaranje nove učitavanja raspoređen skup* . Ćemo podići naš rasporediti opterećenje krajnjoj točki `lb-mysql`. Ne možemo ostavlja iskoristili mogućnosti samostalno osim vrijeme možemo ćete smanjiti 5 (u sekundama, minimalne)

Nakon stvaranja krajnju točku smo idite na `hadb02`, krajnje točke, i stvorili krajnju točku, ali ne možemo će odaberite `lb-mysql`, zatim na padajućem izborniku odaberite MySQL. Azure EŽA možete koristiti i za ovaj korak.

U ovom trenutku imamo sve potrebna za ručnu operaciju klaster.

### <a name="testing-the-load-balanced-set"></a>Testiranje opterećenje raspoređen postavljanje

Testira može izvoditi s mjesta izvan računala, u ovom slučaju pomoću bilo kojeg MySQL klijenta, kao i aplikacije (na primjer, phpMyAdmin izvodi kao web-mjesto za Azure) koristi alat naredbenog retka MySQL, u drugom okviru Linux:

    mysql azureha –u root –h hadb.cloudapp.net –e "select * from things;"

### <a name="manually-failing-over"></a>Ručno neuspješnih iznad

Failovers možete zamjenu sada tako da isključite MySQL, prijelaz primarni DRBD korisnika i ponovno pokretanje MySQL.

Na hadb01:

    service mysql stop && umount /var/lib/mysql ; drbdadm secondary r0

Zatim na hadb02:

    drbdadm primary r0 ; mount /dev/drbd1 /var/lib/mysql && service mysql start

Nakon što ste prebacivanje ručno možete ponoviti udaljenog upita i trebali biste radi savršeno.

## <a name="setting-up-corosync"></a>Postavljanje Corosync

Corosync je podlozi infrastrukture klaster potrebne za Pacemaker rad. Za intervala sustava v1 i v2 korisnike (i drugih methodologies kao što je Ultramonkey) Corosync je podjelu radovi CRM dok Pacemaker više sličnih Hearbeat funkcija.

Glavni ograničenje za Corosync Azure je da Corosync želi višesmjernog putem emitiranje putem jednosmjernog komunikacije, ali mreže Microsoft Azure podržava samo jednosmjernog.

Srećom, Corosync ima jednosmjernog načina rada, a samo realni ograničenje, jer sve čvorove ne komuniciraju među same *automagically*, morate definirati čvorove u konfiguraciji datoteke, uključujući njihove IP adrese. Primjer datoteke Corosync možete koristiti za jednosmjernog "i" samo promjena povezati adresu, čvor popise i zapisivanje direktorija (Ubuntu koristi `/var/log/corosync` dok u primjeru datoteka koristi `/var/log/cluster`) i omogućavanja kvorum alate.

**Bilješke na `transport: udpu` uputa u nastavku i ručno definirani IP adresa za čvorove**.

Na `/etc/corosync/corosync.conf` za oba čvorove:

    totem {
      version: 2
      crypto_cipher: none
      crypto_hash: none
      interface {
        ringnumber: 0
        bindnetaddr: 10.10.10.0
        mcastport: 5405
        ttl: 1
      }
      transport: udpu
    }

    logging {
      fileline: off
      to_logfile: yes
      to_syslog: yes
      logfile: /var/log/corosync/corosync.log
      debug: off
      timestamp: on
      logger_subsys {
        subsys: QUORUM
        debug: off
        }
      }

    nodelist {
      node {
        ring0_addr: 10.10.10.4
        nodeid: 1
      }

      node {
        ring0_addr: 10.10.10.5
        nodeid: 2
      }
    }

    quorum {
      provider: corosync_votequorum
    }

Ne možemo Kopiraj datoteku konfiguracije u oba VMs i pokretanje Corosync u oba čvorove:

    sudo service start corosync

Uskoro nakon pokretanja servisa treba klaster uspostaviti u trenutnom Nazovi i kvorum mora biti constituted. Ne možemo možete provjeriti funkcionalnost tako da pročitate zapisnika ili:

    sudo corosync-quorumtool –l

Slijedite na Izlaz slično kao na slici u nastavku:

![corosync quorumtool - l uzorka Izlaz](media/virtual-machines-linux-classic-mysql-cluster/image001.png)

## <a name="setting-up-pacemaker"></a>Postavljanje Pacemaker

Pacemaker koristi klaster za praćenje za resurse, definirajte kada otvorite očuvat prema dolje i prelazak tih resursa secondaries. Resursi može se definirati iz skupa dostupna skripte ili iz LSB (init nalik) skripti među druge opcije.

Želimo Pacemaker "vlasništvo" DRBD resursa, u mountpoint i servis MySQL. Ako Pacemaker možete uključiti i isključiti DRBD, ga postaviti / umount i početak i kraj MySQL u redoslijedu desno kada je nešto neispravni se događa s primarni naš postavljanje je dovršeno.

Kada prvi put instalirate Pacemaker, konfiguraciju mora biti dovoljno, jednostavno nešto kao što su:

    node $id="1" hadb01
      attributes standby="off"
    node $id="2" hadb02
      attributes standby="off"

Provjerite tako da pokrenete `sudo crm configure show`. Sada stvoriti datoteku (recimo, `/tmp/cluster.conf`) s u sljedećim resursima:

    primitive drbd_mysql ocf:linbit:drbd \
          params drbd_resource="r0" \
          op monitor interval="29s" role="Master" \
          op monitor interval="31s" role="Slave"

    ms ms_drbd_mysql drbd_mysql \
          meta master-max="1" master-node-max="1" \
            clone-max="2" clone-node-max="1" \
            notify="true"

    primitive fs_mysql ocf:heartbeat:Filesystem \
          params device="/dev/drbd/by-res/r0" \
          directory="/var/lib/mysql" fstype="ext3"

    primitive mysqld lsb:mysql

    group mysql fs_mysql mysqld

    colocation mysql_on_drbd \
           inf: mysql ms_drbd_mysql:Master

    order mysql_after_drbd \
           inf: ms_drbd_mysql:promote mysql:start

    property stonith-enabled=false

    property no-quorum-policy=ignore

A sada je učitali u konfiguraciji (samo morate učiniti u jedan čvor):

    sudo crm configure
      load update /tmp/cluster.conf
      commit
      exit

Osim toga, provjerite je li Pacemaker započinje pokretanje u oba čvorovi:

    sudo update-rc.d pacemaker defaults

Nakon nekoliko sekundi i korištenje `sudo crm_mon –L`, provjerite je li jednu od vašeg čvorove postala matrice za klaster, i sustavom svih resursa. Postavljanja i ps možete koristiti da biste provjerili resurse koristite.

Sljedeća slika prikazuje `crm_mon` s jednog čvor zaustavljen (Izlaz pomoću kontrola C)

![crm_mon čvor zaustavljeno](media/virtual-machines-linux-classic-mysql-cluster/image002.png)

I snimka zaslona prikazuje oba čvorove s jedne matrice i jedan podređeni:

![crm_mon radu matrica/podređeni](media/virtual-machines-linux-classic-mysql-cluster/image003.png)

## <a name="testing"></a>Testiranje

Ispričavamo se jeste li spremni za programa simulacije automatsko prebacivanje. Dva su načina za taj postupak: meki i teško. Meki onako kako se pomoću funkcije isključivanja s klaster: ``crm_standby -U `uname -n` -v on``. Korištenje to na matrici na podređeni se. Imajte na umu da biste to ponovno postavljena na Isključeno (crm_mon će vam reći je jedan čvor u stanje mirovanja)

Na tvrdi način je isključuje primarni VM (hadb01) putem portala sustava ili promjena runlevel na VM (odnosno zastoj, isključivanje), a zatim ćemo pomažu Corosync i Pacemaker po signalizacija matrice prelaska prema dolje. To može se testirati (korisno za održavanje windows), ali ne možemo možete pokrenuti scenarij samo Zamrzavanjem na VM.

## <a name="stonith"></a>STONITH

Mora biti moguće izdati VM zatvaranja putem EŽA Azure uspije STONITH skriptu koja određuje fizički uređaj. Možete koristiti `/usr/lib/stonith/plugins/external/ssh` kao osnova i Omogući STONITH u konfiguraciji na klaster. Azure EŽA potrebno globalno instalirati, a zatim Objavi postavke/profil mora biti učitan za korisnika u klaster.

Ogledni kod resurs koji je dostupan na [GitHub](https://github.com/bureado/aztonith). Morate promijeniti na klaster konfiguraciju tako da dodate na sljedeći način `sudo crm configure`:

    primitive st-azure stonith:external/azure \
      params hostlist="hadb01 hadb02" \
      clone fencing st-azure \
      property stonith-enabled=true \
      commit

**Bilješke:** skripta ne izvođenje gore/dolje provjere. Izvorni resursa SSH imali 15 ping provjere, ali oporavak programa Azure VM može biti više varijabli.

## <a name="limitations"></a>Ograničenja

Primjenjuju se sljedeća ograničenja:

- Skripta za resursa DRBD linbit koja upravlja DRBD kao resurs u Pacemaker koristi `drbdadm down` prilikom isključivanja čvor, čak i ako čvor samo će čekanja. Ovo nije idealna jer na podređeni će nije moguće sinkroniziranje resursa DRBD dok matricu dobiva zapisivanja. Ako matricu također uspjeti, na podređeni može potrajati putem stanju starije datotečnom sustavu. Postoje dva načina potencijalne rješavanja ovo:
  - Nametanje na `drbdadm up r0` u sve čvorove klaster putem lokalnog watchdog (ne clusterized) ili,
  - Uređivanje linbit DRBD skripte Provjerite koji `down` je ne poziva, u `/usr/lib/ocf/resource.d/linbit/drbd`.
- Opterećenja potrebne barem 5 sekundi da biste odgovorili, tako da se aplikacija mora biti klaster svjesni i biti fleksibilnije vremenskog ograničenja; ostale arhitekturi mogu pomoći, na primjer redova u aplikaciji, middlewares upita i Dr.
- Usklađivanje MySQL je potrebno provjeriti pisanje obavlja sane tempom i predmemorije su isprazniti na disk često moguće da biste umanjili štetu memorije
- Pisanje performanse bit će zavisne u VM interconnect parametar virtualne Budući da je mehanizam koristi DRBD za replikaciju uređaj
