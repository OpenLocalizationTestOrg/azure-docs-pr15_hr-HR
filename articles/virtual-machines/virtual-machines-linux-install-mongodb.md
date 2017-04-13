<properties
   pageTitle="Kliknite pločicu MongoDB Linux VM | Microsoft Azure"
   description="Saznajte kako instalirati i konfigurirati MongoDB na Linux virtualnog računala u Azure pomoću modela implementacije Voditelj resursa."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="09/29/2016"
   ms.author="iainfou"/>

# <a name="install-and-configure-mongodb-on-a-linux-vm-in-azure"></a>Instaliranje i konfiguriranje MongoDB na Linux VM servisu Azure
[MongoDB](http://www.mongodb.org) je popularne Otvori izvor, visokih performansi NoSQL baze podataka. U ovom se članku objašnjava instaliranja i konfiguriranja MongoDB na Linux VM u Azure pomoću modela implementacije Voditelj resursa. Primjeri prikazane su detaljno kako da biste:

- [Ručno instalirati i konfigurirati osnovne MongoDB instanci](#manually-install-and-configure-mongodb-on-a-vm)
- [Stvaranje osnovne MongoDB instance pomoću predloška Voditelj resursa](#create-basic-mongodb-instance-on-centos-using-a-template)
- [Stvaranje složenih MongoDB sharded klaster s replike postavlja pomoću predloška Voditelj resursa](#create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template)


## <a name="prerequisites"></a>Preduvjeti
U ovom se članku potrebno je sljedeće:

- Azure račun ([dobiti besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/)).
- [Azure EŽA](../xplat-cli-install.md) prijavljeni`azure login`
- na Azure EŽA *mora biti* Voditelj resursa Azure način korištenja`azure config mode arm`


## <a name="manually-install-and-configure-mongodb-on-a-vm"></a>Ručno instaliranje i konfiguriranje MongoDB na na VM
MongoDB [sadrže upute za instalaciju](https://docs.mongodb.com/manual/administration/install-on-linux/) za Linux distros uključujući je vaša crveno / CentOS, SUSE, Ubuntu i Debian. Sljedeći primjer stvara na `CoreOS` VM pomoću ključa SSH spremljenu na `.ssh/azure_id_rsa.pub`. Odgovaranje upite za naziv računa za pohranu, naziv DNS-a i administratorske vjerodajnice:

```bash
azure vm quick-create --ssh-publickey-file .ssh/azure_id_rsa.pub --image-urn CentOS
```

Prijavite se na VM pomoću javnu IP adresu prikazuje na kraju u prethodnom koraku VM stvaranja:

```bash
ssh ops@40.78.23.145
```

Da biste dodali instalacijskim za MongoDB, stvorite je `yum` spremište datoteke na sljedeći način:

```bash
sudo touch /etc/yum.repos.d/mongodb-org-3.2.repo
```

Otvorite datoteku repo MongoDB za uređivanje. Dodajte sljedeće retke:

```bash
[mongodb-org-3.2]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.2/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.2.asc
```

Instalacija MongoDB pomoću `yum` na sljedeći način:

```bash
sudo yum install -y mongodb-org
```

Prema zadanim postavkama SELinux provest će se na slikama CentOS koji sprječava pristup MongoDB. Instalacija alata za upravljanje pravilima i konfiguriranje SELinux da biste omogućili MongoDB da biste upravljali njegov zadani TCP priključak 27017 na sljedeći način. 

```bash
sudo yum install -y policycoreutils-python
sudo semanage port -a -t mongod_port_t -p tcp 27017
```

Pokretanje servisa MongoDB na sljedeći način:

```bash
sudo service mongod start
```

Provjerite je li MongoDB instalaciju putem veze pomoću lokalnoj `mongo` klijent:

```bash
mongo
```

Sada testiranje instancu MongoDB tako da dodate neke podatke, a zatim pretraživanje:

```
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```

Po želji konfigurirati MongoDB na automatsko pokretanje prilikom ponovnog pokretanja sustava:

```bash
sudo chkconfig mongod on
```


## <a name="create-basic-mongodb-instance-on-centos-using-a-template"></a>Stvaranje osnovne instance MongoDB na CentOS pomoću predloška
Osnovni MongoDB instance možete stvoriti na jednom CentOS VM pomoću sljedećih predloška Azure brzi početak rada s Github. Ovaj predložak koristi prilagođene skripte proširenja za Linux da biste dodali na `yum` spremište novostvorenu VM CentOS i zatim instaliraj MongoDB.

- [Osnovni MongoDB instancu na CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-on-centos) - https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json

Sljedeći primjer stvara grupu resursa s nazivom `myResourceGroup` u na `WestUS` regija. Unesite vlastiti vrijednosti na sljedeći način:

```bash
azure group create --name myResourceGroup --location WestUS \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json
```

> [AZURE.NOTE] Azure EŽA vratiti upit nekoliko sekundi stvaranja implementaciju, ali instalacije i konfiguracije traje nekoliko minuta da biste dovršili. Provjera statusa implementacija s `azure group deployment show myResourceGroup`, sukladno tome unijeti naziv grupu resursa. Čekati na `ProvisioningState` prikazuje "Je uspjelo" prije no što pokušate SSH da biste na VM.

Kada implementacijskih dovrši, SSH da biste na VM. Dohvaćanje IP adrese VM korištenja u `azure vm show` naredbe kao u sljedećem primjeru:

```bash
azure vm show --resource-group myResourceGroup --name myVM
```

Pri kraju Izlaz, u `Public IP address` se prikazuje. SSH za vaše VM s IP adresom vaše VM:

```bash
ssh ops@138.91.149.74
```

Provjerite je li MongoDB instalaciju putem veze pomoću lokalnoj `mongo` klijenta na sljedeći način:

```bash
mongo
```

Sada testiranje instanci tako da dodate neke podatke, a zatim pretraživanje na sljedeći način:

```
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```


## <a name="create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template"></a>Stvaranje složenih klaster Sharded MongoDB na CentOS pomoću predloška
Možete stvarati složene MongoDB sharded klaster pomoću sljedećih predloška Azure brzi početak rada s Github. Ovaj predložak slijedi [MongoDB sharded klaster najbolje prakse](https://docs.mongodb.com/manual/core/sharded-cluster-components/) zalihosti i visoke dostupnosti. Predložak se stvara dva shards s tri čvorove u svakom skupu replike. Jedan config poslužitelja replike postavljen s tri čvorove i stvorili, plus dva `mongos` usmjerivač poslužitelji za omogućuje dosljednost aplikacijama iz sustava shards.

- [MongoDB Sharding klaster na CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-sharding-centos) – https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json

> [AZURE.WARNING] Implementacija složene klaster sharded MongoDB zahtijeva više od 20 jezgri koji se obično count core zadani po regijama za pretplatu. Otvorite zahtjev za Azure podršku da biste povećali vaš osnovni broj.

Sljedeći primjer stvara grupu resursa s nazivom `myResourceGroup` u na `WestUS` regija. Unesite vlastiti vrijednosti na sljedeći način:

```bash
azure group create --name myResourceGroup --location WestUS \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json
```

> [AZURE.NOTE] Azure EŽA vratiti upit nekoliko sekundi stvaranja implementaciju, ali instalacije i konfiguracije možete preuzeti jedan sat da biste dovršili. Provjera statusa implementacija s `azure group deployment show myResourceGroup`, prilagodite naziv grupe resursa sukladno tome. Čekati na `ProvisioningState` prikazuje "Je uspjelo" prije povezivanja s VMs.


## <a name="next-steps"></a>Daljnji koraci
U ovim se primjerima povežete s instancom MongoDB lokalno iz na VM. Ako se želite povezati s instancom MongoDB iz drugog VM ili mrežom, provjerite je li odgovarajuću [stvaraju se pravila mreže sigurnosne grupe](virtual-machines-linux-nsg-quickstart.md).

Dodatne informacije o stvaranju pomoću predložaka potražite u članku [Pregled upravljanja resursima Azure](../azure-resource-manager/resource-group-overview.md).

Predlošci Voditelj resursa Azure pomoću proširenja za prilagođene skripte da biste preuzeli i pokrenuli skripte na vašem VMs. Dodatne informacije potražite u članku [Korištenje Azure prilagođene skripte nastavka Linux virtualnim računalima sustava](virtual-machines-linux-extensions-customscript.md).