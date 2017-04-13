<properties
    pageTitle="Postavljanje MySQL na Linux VM | Microsoft Azure "
    description="Saznajte kako instalirati stog MySQL na Linux virtualnog računala (Ubuntu ili RedHat grupa OS) u Azure"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="SuperScottz"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/01/2016"
    ms.author="mingzhan"/>


#<a name="how-to-install-mysql-on-azure"></a>Kako instalirati MySQL na Azure


U ovom se članku će Saznajte kako instalirati i konfigurirati MySQL na Azure virtualnog računala radi Linux.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


##<a name="install-mysql-on-your-virtual-machine"></a>Kliknite pločicu MySQL virtualnog računala

> [AZURE.NOTE] Morate imati već na Microsoft Azure virtualnog računala radi Linux kako bi obavili ovog praktičnog vodiča. Pročitajte članak [Azure Linux VM vodič](virtual-machines-linux-quick-create-cli.md) za stvaranje i postavljanje VM Linux s `mysqlnode` nazivu VM i `azureuser` kao korisnik prije nastavka.

U ovom slučaju koristi priključak 3306 kao priključak MySQL.  

Povezivanje s Linux VM koju ste stvorili putem putty. Ako je ovo prvi put koristite Azure Linux VM, pogledajte upute za korištenje putty povezati Linux VM [ovdje](virtual-machines-linux-mac-create-ssh-keys.md).

Koristit ćemo spremište paketa da biste instalirali MySQL5.6 kao primjer u ovom članku. Zapravo, MySQL5.6 sadrži dodatne poboljšanja performansi od MySQL5.5.  Dodatne informacije [u nastavku](http://www.mysqlperformanceblog.com/2013/02/18/is-mysql-5-6-slower-than-mysql-5-5/).


###<a name="how-to-install-mysql56-on-ubuntu"></a>Kako instalirati MySQL5.6 na Ubuntu
Koristit ćemo Linux VM s Ubuntu iz Azure ovdje.

- Korak 1: Instalacija MySQL poslužitelja 5.6 da biste prešli na `root` korisnika:

            #[azureuser@mysqlnode:~]sudo su -

    Instalirajte poslužitelj mysql 5.6:

            #[root@mysqlnode ~]# apt-get update
            #[root@mysqlnode ~]# apt-get -y install mysql-server-5.6

    Tijekom instalacije, prikazat će se u dijaloški okvir prozor poping do Pitaj koje možete postaviti lozinku korijenski MySQL ispod i koje morate postaviti lozinku ovdje.

    ![Slika](./media/virtual-machines-linux-mysql-install/virtual-machines-linux-install-mysql-p1.png)


    Unesite lozinku da biste potvrdili.

    ![Slika](./media/virtual-machines-linux-mysql-install/virtual-machines-linux-install-mysql-p2.png)

- Korak 2: Prijava MySQL poslužitelja

    Po završetku instalaciju poslužitelja MySQL MySQL servis pokrenut će se automatski. Možete prijaviti poslužitelj MySQL s `root` korisnika.
    Korištenje u nastavku naredbu za prijavu i unos lozinke.

             #[root@mysqlnode ~]# mysql -uroot -p

- Korak 3: Upravljanje servisom za izvodi MySQL

    (a) se status servisa MySQL

             #[root@mysqlnode ~]# service mysql status

    (b) pokrenuti servis MySQL

             #[root@mysqlnode ~]# service mysql start

    (c) Zaustavljanje servisa MySQL

             #[root@mysqlnode ~]# service mysql stop

    (d) ponovno pokrenite servis za MySQL

             #[root@mysqlnode ~]# service mysql restart


###<a name="how-to-install-mysql-on-red-hat-os-family-like-centos-oracle-linux"></a>Kako instalirati MySQL na crveno je vaša OS obitelji kao što su CentOS, Oracle Linux
Koristit ćemo Linux VM CentOS ili Oracle Linux ovdje.

- Korak 1: Dodavanje spremište MySQL Yum prebaci na `root` korisnika:

            #[azureuser@mysqlnode:~]sudo su -

    Preuzmite i instalirajte paket izdanje MySQL:

            #[root@mysqlnode ~]# wget http://repo.mysql.com/mysql-community-release-el6-5.noarch.rpm
            #[root@mysqlnode ~]# yum localinstall -y mysql-community-release-el6-5.noarch.rpm

- Korak 2: Uredite ispod datoteku da biste omogućili spremište MySQL za preuzimanje paketa MySQL5.6.

            #[root@mysqlnode ~]# vim /etc/yum.repos.d/mysql-community.repo

    Ažurirajte svaku vrijednost parametra tu datoteku u nastavku:

        \# *Enable to use MySQL 5.6*

        [mysql56-community]
        name=MySQL 5.6 Community Server

        baseurl=http://repo.mysql.com/yum/mysql-5.6-community/el/6/$basearch/

        enabled=1

        gpgcheck=1

        gpgkey=file:/etc/pki/rpm-gpg/RPM-GPG-KEY-mysql

- Korak 3: Instalacija MySQL MySQL spremištu instalirati MySQL:

           #[root@mysqlnode ~]#yum install mysql-community-server

    Paket MySQL RPM i sve povezane pakete instalirat će se.

- Korak 4: Upravljanje servisom za izvodi MySQL

    (a) provjerite status servisa MySQL poslužitelja:

           #[root@mysqlnode ~]#service mysqld status

    (b) provjerite je li zadani poslužitelj priključak MySQL pokrenut:

           #[root@mysqlnode ~]#netstat  –tunlp|grep 3306


    (c) početak MySQL poslužitelja:

           #[root@mysqlnode ~]#service mysqld start

    (d) prekinuli MySQL poslužitelju učinite sljedeće:

           #[root@mysqlnode ~]#service mysqld stop

    (e) MySQL postavljanje da biste pokrenuli kada sustava pokretanje kumulativne:

           #[root@mysqlnode ~]#chkconfig mysqld on


###<a name="how-to-install-mysql-on-suse-linux"></a>Kako instalirati MySQL na SUSE Linux
Koristit ćemo Linux VM s OpenSUSE ovdje.

- Korak 1: Preuzimanje i instalacija MySQL poslužitelja

    Prijelaz na `root` korisnika putem ispod naredbe:  

           #sudo su -

    Preuzmite i instalirajte paket MySQL:

           #[root@mysqlnode ~]# zypper update

           #[root@mysqlnode ~]# zypper install mysql-server mysql-devel mysql

- Korak 2: Upravljanje servisom za izvodi MySQL

    (a) potvrdite status MySQL poslužitelja:

           #[root@mysqlnode ~]# rcmysql status

    (b) potvrdite li zadani je priključak poslužitelja MySQL:

           #[root@mysqlnode ~]# netstat  –tunlp|grep 3306


    (c) početak MySQL poslužitelja:

           #[root@mysqlnode ~]# rcmysql start

    (d) prekinuli MySQL poslužitelju učinite sljedeće:

           #[root@mysqlnode ~]# rcmysql stop

    (e) MySQL skup da biste pokrenuli kada sustava pokretanje zbirnog:

           #[root@mysqlnode ~]# insserv mysql

###<a name="next-step"></a>Sljedeći korak
Pronađite više o korištenju i informacije o MySQL [ovdje](https://www.mysql.com/).
