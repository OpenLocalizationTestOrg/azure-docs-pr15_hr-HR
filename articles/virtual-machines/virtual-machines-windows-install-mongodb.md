<properties
    pageTitle="Stvarna MongoDB veličina VM | Microsoft Azure"
    description="Saznajte kako instalirati MongoDB na programa Azure VM izvodi Windows Server 2012 R2 stvorene pomoću model implementacije Voditelj resursa."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="iainfou"/>

# <a name="install-and-configure-mongodb-on-a-windows-vm-in-azure"></a>Instaliranje i konfiguriranje MongoDB na Windows VM servisu Azure
[MongoDB](http://www.mongodb.org) je popularne Otvori izvor, visokih performansi NoSQL baze podataka. U ovom se članku vodi vas kroz instaliranje i konfiguriranje MongoDB na Windows Server 2012 R2 virtualnog računala (VM) u Azure. Možete [instalirati MongoDB na Linux VM u Azure](virtual-machines-linux-install-mongodb.md).


## <a name="prerequisites"></a>Preduvjeti

Prije nego što instalirate i konfigurirate MongoDB, morate stvoriti na VM i najbolje i dodajte podatkovni disk. Potražite u sljedećim člancima kako stvoriti na VM i dodati na disku podataka:

- [Stvaranje VM za poslužitelj sustava Windows pomoću portala za Azure](virtual-machines-windows-hero-tutorial.md) ili [stvorite VM poslužitelja za Windows pomoću komponente PowerShell Azure](virtual-machines-windows-ps-create.md)
- [Priloži podatkovni disk da biste u sustavu Windows Server VM pomoću portala za Azure](virtual-machines-windows-attach-disk-portal.md) ili [prilaganje podatkovni disk da biste u sustavu Windows Server VM pomoću komponente PowerShell Azure](https://msdn.microsoft.com/library/mt603673.aspx)
    
Da biste započeli instaliranje i konfiguriranje MongoDB, [prijavite se na sustava Windows Server VM](virtual-machines-windows-connect-logon.md) pomoću udaljene radne površine.


## <a name="install-mongodb"></a>Instalacija MongoDB

> [AZURE.IMPORTANT] MongoDB sigurnosnim značajkama, kao što su provjeru autentičnosti i IP adrese uvez, nisu omogućeni prema zadanim postavkama. Značajke sigurnosti želite omogućiti prije implementacije MongoDB za radnog okruženja. Dodatne informacije potražite u članku [MongoDB sigurnost i provjeru autentičnosti](http://www.mongodb.org/display/DOCS/Security+and+Authentication).

1. Nakon što ste povezali s vašeg VM putem udaljene radne površine, otvorite Internet Explorer na izborniku **Start** na na VM.

2. Odaberite **Koristi preporučene sigurnost, privatnost i postavki kompatibilnosti** kada prvi put otvori Internet Explorer, a zatim kliknite **u redu**.

3. Internet Explorer poboljšane konfiguracije sigurnosti omogućena je prema zadanim postavkama. Dodavanje MongoDB web-mjesta na popis dopuštenih web-mjesta:

    - U gornjem desnom kutu odaberite ikonu **Alati** .
    - U **Internetske mogućnosti**odaberite karticu **Sigurnost** , a zatim odaberite ikonu **Pouzdana web-mjesta** .
    - Kliknite gumb **web-mjesta** . Dodavanje _https://\*. mongodb.org_ na popis pouzdanih web-mjesta, a zatim zatvaranje dijaloškog okvira.

    ![Konfiguriranje sigurnosnih postavki u pregledniku Internet Explorer](./media/virtual-machines-windows-install-mongodb/configure-internet-explorer-security.png)

4. Dođite do stranice [MongoDB – preuzimanje](http://www.mongodb.org/downloads) (http://www.mongodb.org/downloads).

5. Po zadanom je trebali biste odabrati edition **Poslužitelj zajednice** i najnovije izdanje trenutno stabilan za Windows Server 2008 R2 64-bitni i noviji. Da biste preuzeli instalacijski program, kliknite **PREUZMI (msi)**.

    ![Preuzimanje instalacijskog programa MongoDB](./media/virtual-machines-windows-install-mongodb/download-mongodb.png)

    Kada se preuzimanje dovrši, pokrenite instalacijski program.

6. Pročitajte i prihvatite licencni ugovor. Kada se od vas zatraži, odaberite **dovrši** instalacija.

7. Na zadnjem zaslonu kliknite **Instaliraj**.


## <a name="configure-the-vm-and-mongodb"></a>Konfiguriranje VM i MongoDB

1. Varijable put ne ažuriraju prema MongoDB instalacijski program. Bez u MongoDB `bin` mjesto u varijablu put, morate navesti cijeli put prilikom svakog korištenja izvršna datoteka MongoDB. Da biste dodali mjesto varijablu put:

    - Desnom tipkom miša kliknite izbornik **Start** , a zatim odaberite **sustav**.
    - Kliknite **Dodatne postavke sustava**, a zatim **Varijable okruženja**.
    - U odjeljku **sustav varijable**, odaberite **put**, a zatim **Uređivanje**.

    ![Konfiguriranje varijable put](./media/virtual-machines-windows-install-mongodb/configure-path-variables.png)

    Dodavanje puta vaš MongoDB `bin` mapu. MongoDB obično je instaliran u `C:\Program Files\MongoDB`. Provjerite je li put instalacije na vašem VM. Sljedeći primjer dodaje zadani MongoDB instalacija lokaciju na `PATH` varijabla:

    ```
    ;C:\Program Files\MongoDB\Server\3.2\bin
    ```

    > [AZURE.NOTE] Ne zaboravite da biste dodali početne točka sa zarezom (`;`) da biste naznačili dodajete mjesto za vaše `PATH` varijabla.

2. Stvaranje direktorija MongoDB podataka i prijaviti se na disku podataka. S izbornika **Start** odaberite **naredbeni redak**. Sljedeći primjeri stvaranje direktorija na pogon F:

    ```
    mkdir F:\MongoData
    mkdir F:\MongoLogs
    ```

3. Započnite MongoDB instancu sljedeću naredbu, Prilagodba put do podataka i sukladno tome prijavite direktorija:

    ```
    mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log
    ```

    Može potrajati nekoliko minuta za MongoDB dodijeliti datoteke dnevnika i započnite slušanje za veze. Sve poruke evidencije upućeni *F:\MongoLogs\mongolog.log* datoteku kao `mongod.exe` poslužitelja pokreće i dodjeljuje datoteke dnevnika.

    > [AZURE.NOTE] Naredbeni redak ostaje usmjeren na ovaj zadatak dok se izvodi na instancu MongoDB. Ostavite otvoren da biste nastavili pokrenut MongoDB prozor naredbenog retka. Ili instalirajte MongoDB kao servis, što je detaljno prikazano u sljedećem koraku.

4. Robusniji MongoDB sučelje, možete instalirati na `mongod.exe` kao usluga. Stvaranje usluge znači da ne morate napustiti naredbeni redak koji se izvodi svaki put kada želite koristiti MongoDB. Stvaranje usluge na sljedeći način, sukladno tome Prilagodba put do direktorija vaše podatke i zapisnika:

    ```
    mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log `
        --logappend  --install
    ```

    Prethodni naredba stvara servisa pod nazivom MongoDB, s opisom "Mongo DB". Također određeni su klauzulom sljedećih parametara:

    - Na `--dbpath` mogućnost određuje mjesto direktoriju podataka.
    - Na `--logpath` mogućnost moraju se koristiti za određivanje datoteku zapisnika jer pokrenuti servis nema naredbeni prozor da biste prikazali izlaz.
    - Na `--logappend` mogućnost određuje da je ponovno pokretanje servisa uzrokuje izlaz da biste dodali postojeću datoteku zapisnika.

  Da biste pokrenuli MongoDB, pokrenite sljedeću naredbu:

    ```
    net start MongoDB
    ```

    Dodatne informacije o stvaranju servis MongoDB potražite u članku [Konfiguriranje servisa Windows za MongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/#mongodb-as-a-windows-service).

## <a name="test-the-mongodb-instance"></a>Testiranje instancu MongoDB

S MongoDB instaliran kao usluga ili izvodi kao jednokratan, možete početi stvaranje i korištenje vaših baza podataka. Da biste započeli MongoDB ljuske za administratora, otvorite drugi prozor naredbenog retka s izbornika **Start** pa unesite sljedeću naredbu:

```
mongo  
```

Možete navesti baze podataka s na `db` naredbe. Umetanje neke podatke na sljedeći način:

```
db.foo.insert( { a : 1 } )
```

Traženje podataka na sljedeći način:

```
db.foo.find()
```

Rezultat je slično kao u sljedećem primjeru:

```
{ "_id" : "ObjectId("57f6a86cee873a6232d74842"), "a" : 1 }
```

Izlaz iz prikaza na `mongo` konzole na sljedeći način:

```
exit
```

## <a name="configure-firewall-and-network-security-group-rules"></a>Konfiguriranje vatrozida i pravila mreže sigurnosne grupe
Sad kad MongoDB je instalirana i pokrenuta, otvorite priključak u vatrozidu za Windows da bi daljinski možete se povezati s MongoDB. Da biste stvorili novo pravilo ulazne dopustili TCP priključak 27017, otvorite administrativne odzivniku komponente PowerShell i upišite sljedeću naredbu:

```powerShell
New-NetFirewallRule -DisplayName "Allow MongoDB" -Direction Inbound `
    -Protocol TCP -LocalPort 27017 -Action Allow
```

Pravilo možete stvoriti i pomoću alata za upravljanje grafički **Vatrozid za Windows s dodatnom sigurnošću** . Stvaranje novog pravila ulazne dopustili TCP priključak 27017.

Ako je potrebno, stvorite pravilo mrežom sigurnosnoj grupi dopustili pristup MongoDB iz izvan postojeće podmreže Azure virtualne mreže. Pravila mreže sigurnosne grupe možete stvoriti pomoću [portala za Azure](virtual-machines-windows-nsg-quickstart-portal.md) ili [Azure PowerShell](virtual-machines-windows-nsg-quickstart-powershell.md). Kao i kod pravila vatrozida za Windows omogućuju TCP priključak 27017 sučelje virtualne mreže vaše VM MongoDB.

> [AZURE.NOTE] TCP priključak 27017 je zadani je priključak koristi MongoDB. Možete promijeniti priključak pomoću na `--port` parametar prilikom pokretanja `mongod.exe` ručno ili iz servisa. Ako promijenite priključak, provjerite je li za ažuriranje pravila vatrozida za Windows i mreže sigurnosne grupe u prethodne korake.


## <a name="next-steps"></a>Daljnji koraci
U ovom ćete praktičnom vodiču naučili kako instalirati i konfigurirati MongoDB na VM sustava Windows. Sada možete pristupiti MongoDB na VM sustava Windows slijedeći dodatne teme u [dokumentaciji MongoDB](https://docs.mongodb.com/manual/).
