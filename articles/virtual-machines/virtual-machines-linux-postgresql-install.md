<properties
    pageTitle="Postavljanje PostgreSQL na Linux VM | Microsoft Azure"
    description="Saznajte kako instalirati i konfigurirati PostgreSQL na Linux virtualnog računala u Azure"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="SuperScottz"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="02/01/2016"
    ms.author="mingzhan"/>


# <a name="install-and-configure-postgresql-on-azure"></a>Instaliranje i konfiguriranje PostgreSQL na Azure

PostgreSQL je dodatno Otvori izvor baze podataka slične Oracle i DB2. Uključuje značajke enterprise spremnu kao što su potpuno ACID usklađenost, pouzdanog transakcijskih obrada i kontrola istodobnosti više verzija. Podržava i Standardi kao što su ANSI SQL i SQL/SR (uključujući wrappers vanjskih podataka za Oracle, MySQL, MongoDB i mnoge druge). To je vrlo extensible s podrškom za veće od 12 proceduralne jezika, DŽIN i GiST indeksi, podrška za prostorno podataka i više nalik NoSQL značajke za JSON ili ključ vrijednost-aplikacije utemeljene na.

U ovom se članku će Saznajte kako instalirati i konfigurirati PostgreSQL na Azure virtualnog računala radi Linux.


[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


## <a name="install-postgresql"></a>Instalacija PostgreSQL

> [AZURE.NOTE] Morate već imati Azure virtualnog računala radi Linux kako bi obavili ovog praktičnog vodiča. Stvaranje i postavljanje Linux VM prije nastavka potražite u članku [vodič Azure Linux VM](virtual-machines-linux-quick-create-cli.md).

U ovom slučaju koristi priključak 1999 kao priključak PostgreSQL.  

Povezivanje s Linux VM koju ste stvorili putem PuTTY. Ako je ovo prvi put koristite programa Azure Linux VM, pogledajte [upute za korištenje SSH s Linux na Azure](virtual-machines-linux-mac-create-ssh-keys.md) da biste saznali kako koristiti PuTTY za povezivanje s Linux VM.

1. Pokrenite sljedeću naredbu da biste prešli na korijenskom (administratora):

        # sudo su -

2. Neke distribucija imati ovisnosti koja morate instalirati prije instalacije PostgreSQL. Provjerite svoje distro na ovom popisu i pokrenuti odgovarajuću naredbu:

    - Osnovni Linux crveno razgovor:

            # yum install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  

    - Osnovni Linux debian:

            # apt-get install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam libxslt-devel tcl-devel python-devel -y  

    - SUSE Linux:

            # zypper install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  

3. Preuzmite PostgreSQL u korijenskom direktoriju i raspakirajte paket:

        # wget https://ftp.postgresql.org/pub/source/v9.3.5/postgresql-9.3.5.tar.bz2 -P /root/

        # tar jxvf  postgresql-9.3.5.tar.bz2

    Navedenog predstavlja primjer. Detaljnije preuzimanja adresa možete pronaći u [indeks pub/izvor /](https://ftp.postgresql.org/pub/source/).

4. Da biste započeli sastavljanje, pokrenite sljedeće naredbe:

        # cd postgresql-9.3.5

        # ./configure --prefix=/opt/postgresql-9.3.5

5. Ako želite izraditi sve što može biti ugrađena, uključujući dokumentacija (HTML i Muškarac stranica) i dodatnih modula (contrib), pokrenite sljedeću naredbu:

        # gmake install-world

    Trebali biste dobiti sljedeću poruku o potvrdi:

        PostgreSQL, contrib, and documentation successfully made. Ready to install.

## <a name="configure-postgresql"></a>Konfiguriranje PostgreSQL

1. (Neobavezno) Stvaranje simboličke veze da biste skratili PostgreSQL referenca ne obuhvaća broj verzije:

        # ln -s /opt/pgsql9.3.5 /opt/pgsql

2. Stvaranje direktorija za bazu podataka:

        # mkdir -p /opt/pgsql_data

3. Stvaranje korisnika koji nisu korijenski i izmijenite profil za tog korisnika. Zatim prijeđite u novom korisniku (pod nazivom *postgres* u našem primjeru):

        # useradd postgres

        # chown -R postgres.postgres /opt/pgsql_data

        # su - postgres

   > [AZURE.NOTE] Sigurnosnih vam razloga PostgreSQL koristi korisnika koji nisu korijenski pokretanje, započnite ili zatvorite bazu podataka.


4. Uređivanje datoteke *bash_profile* tako da unesete naredbi u nastavku. Te retke dodaje se na kraj *bash_profile* datoteke:

        cat >> ~/.bash_profile <<EOF
        export PGPORT=1999
        export PGDATA=/opt/pgsql_data
        export LANG=en_US.utf8
        export PGHOME=/opt/pgsql
        export PATH=\$PATH:\$PGHOME/bin
        export MANPATH=\$MANPATH:\$PGHOME/share/man
        export DATA=`date +"%Y%m%d%H%M"`
        export PGUSER=postgres
        alias rm='rm -i'
        alias ll='ls -lh'
        EOF

5. Izvršavanje *bash_profile* datoteke:

        $ source .bash_profile

6. Provjerite valjanost instalacije sustava pomoću sljedeće naredbe:

        $ which psql

    Ako je vaša instalacija uspije, vidjet ćete sljedeće odgovor:

        /opt/pgsql/bin/psql

7. Možete provjeriti i PostgreSQL verzija:

        $ psql -V

8. Pokretanje baze podataka:

        $ initdb -D $PGDATA -E UTF8 --locale=C -U postgres -W

    Trebali biste dobiti sljedeći rezultat:

![Slika](./media/virtual-machines-linux-postgresql-install/no1.png)

## <a name="set-up-postgresql"></a>Postavljanje PostgreSQL

<!--    [postgres@ test ~]$ exit -->

Pokrenite sljedeće naredbe:

    # cd /root/postgresql-9.3.5/contrib/start-scripts

    # cp linux /etc/init.d/postgresql

Izmjena dvije varijable u datoteci /etc/init.d/postgresql. Prefiks postavlja se put instalacija PostgreSQL: **/opt/pgsql**. PGDATA postavljena na put za pohranu podataka PostgreSQL: **/opt/pgsql_data**.

    # sed -i '32s#usr/local#opt#' /etc/init.d/postgresql

    # sed -i '35s#usr/local/pgsql/data#opt/pgsql_data#' /etc/init.d/postgresql

![Slika](./media/virtual-machines-linux-postgresql-install/no2.png)

Promjena da bi izvršna datoteka:

    # chmod +x /etc/init.d/postgresql

Pokrenite PostgreSQL:

    # /etc/init.d/postgresql start

Provjera je li krajnje PostgreSQL:

    # netstat -tunlp|grep 1999

Trebali biste vidjeti sljedeći rezultat:

![Slika](./media/virtual-machines-linux-postgresql-install/no3.png)

## <a name="connect-to-the-postgres-database"></a>Povezivanje s bazom podataka Postgres

Prebacite se korisniku postgres opet:

    # su - postgres

Stvaranje Postgres baze podataka:

    $ createdb events

Povezivanje s bazom podataka za događaja koji ste upravo stvorili:

    $ psql -d events

## <a name="create-and-delete-a-postgres-table"></a>Stvaranje i brisanje tablice Postgres

Sad kad ste povezali s bazom podataka, u njoj možete stvoriti tablice.

Na primjer, stvaranje nove tablice Postgres primjeru pomoću sljedeće naredbe:

    CREATE TABLE potluck (name VARCHAR(20), food VARCHAR(30),   confirmed CHAR(1), signup_date DATE);

Sada ste postavili tablice četiri stupca sa sljedećim nazivi stupaca i ograničenja:

1. Stupac "naziv" sadrži ograničen naredba VARCHAR da bi se u odjeljku 20 znakova.
2. Stupac "hrana" pokazuje stavke hrane koja će se srušiti svaku osobu. VARCHAR ograničenjima ovaj tekst biti u odjeljku 30 znakova.
3. Stupac "potvrđena" zapisa li osoba koja ima RSVP'd da biste na Neformalna zakuska. Prihvatljiva vrijednosti su "D" i "N".
4. Polje "datum" stupac prikazuje kada se registrirali za događaj. Postgres zahtijeva kao gggg mm dd. pisana datuma

Trebali biste vidjeti sljedeće ako tablica sadrži uspješno stvorili:

![Slika](./media/virtual-machines-linux-postgresql-install/no4.png)

Pomoću sljedeće naredbe možete provjeriti i strukturu tablice:

![Slika](./media/virtual-machines-linux-postgresql-install/no5.png)

### <a name="add-data-to-a-table"></a>Dodavanje podataka u tablicu

Najprije umetnite podatke u redak:

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('John', 'Casserole', 'Y', '2012-04-11');

Trebali biste vidjeti ovaj izlaz:

![Slika](./media/virtual-machines-linux-postgresql-install/no6.png)

Nekoliko dodatnih osoba možete dodati u tablicu. Evo nekih mogućnosti ili stvorite vlastiti:

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Sandy', 'Key Lime Tarts', 'N', '2012-04-14');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES ('Tom', 'BBQ','Y', '2012-04-18');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Tina', 'Salad', 'Y', '2012-04-18');

### <a name="show-tables"></a>Prikaži tablice

Da biste prikazali tablicu, koristite sljedeću naredbu:

    select * from potluck;

Rezultat je:

![Slika](./media/virtual-machines-linux-postgresql-install/no7.png)

### <a name="delete-data-in-a-table"></a>Brisanje podataka u tablici

Da biste izbrisali podatke u tablici, koristite sljedeću naredbu:

    delete from potluck where name=’John’;

Ovo se briše sve podatke u retku "Luka". Rezultat je:

![Slika](./media/virtual-machines-linux-postgresql-install/no8.png)

### <a name="update-data-in-a-table"></a>Ažuriranje podataka u tablici

Da biste ažurirali podatke u tablici, koristite sljedeću naredbu. Za ovaj Sandy je potvrdio da ona je sudjelovanje na njima, pa ne možemo će se promijeniti svoj Napomenuti iz "N" i "D":

    UPDATE potluck set confirmed = 'Y' WHERE name = 'Sandy';


##<a name="get-more-information-about-postgresql"></a>Dodatne informacije o PostgreSQL
Sad kad ste dovršili instalacija PostgreSQL u programa Azure Linux VM, mogli uživati pomoću u Azure. Da biste saznali više o PostgreSQL, posjetite sustava [PostgreSQL web-mjesta](http://www.postgresql.org/).
