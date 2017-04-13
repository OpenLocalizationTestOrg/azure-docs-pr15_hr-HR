<properties
   pageTitle="Docker i sastavljanje na virtualnog računala | Microsoft Azure"
   description="Brzi Uvod u rad s Sastavljanje i Docker na virtualnim računalima sustava Linux servisu Azure"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="09/22/2016"
   ms.author="iainfou"/>

# <a name="get-started-with-docker-and-compose-to-define-and-run-a-multi-container-application-on-an-azure-virtual-machine"></a>Početak rada s Docker i sastavljanje definiranje i pokrenuti više spremniku Azure virtualnog računala

Početak rada s web-mjesto Docker i u okvir za [Sastavljanje](http://github.com/docker/compose) definiranje i pokretanje složene aplikacije na računalo virtualne Linux u Azure. S sastavljanje, poslužite se jednostavne tekstne datoteke da biste odredili aplikacije koji se sastoji od više Docker spremnika. Zatim Okretni gore aplikacije u jednom naredbi koje čini sve za implementaciju definirani okruženju. 

Na primjer, u ovom se članku objašnjava da biste brzo postavili bloga WordPress s pozadinskom bazom podataka MariaDB SQL na programa VM Ubuntu. Sastavljanje možete koristiti i da biste postavili složenije aplikacije.


## <a name="step-1-set-up-a-linux-vm-as-a-docker-host"></a>Korak 1: Postavljanje Linux VM kao Docker glavno računalo

Koristite različite Azure postupke i dostupne slike ili Voditelj resursa predložaka u Azure Marketplace da biste stvorili Linux VM i postaviti je kao Docker glavno računalo. Na primjer, potražite u članku [Korištenje nastavka VM Docker za implementaciju okruženju sustava](virtual-machines-linux-dockerextension.md) da biste brzo stvorili programa VM Ubuntu s nastavkom Azure Docker VM pomoću [predloška za brzi početak rada](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). 

Kada koristite proširenje Docker VM, vaše VM automatski postaviti kao glavno računalo Docker i sastavljanje već instaliran. U primjeru u tom članku prikazuje kako koristiti [Azure sučelje naredbenog retka za Mac i Linux, Windows](../xplat-cli-install.md) (Azure EŽA) u načinu Voditelj resursa da biste stvorili na VM.

Osnovni naredba iz prethodnog dokumenta stvara grupu resursa pod nazivom `myResourceGroup` u na `West US` mjesto i implementira na VM s nastavkom Azure Docker VM instaliran:

```bash
azure group create --name myResourceGroup --location "West US" \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

## <a name="step-2-verify-that-compose-is-installed"></a>Korak 2: Provjerite je li instaliran sastavljanje

Po dovršetku implementacijskih SSH svoje nove Docker glavno računalo za korištenje DNS-om naziv koji vode implementacije. Možete koristiti `azure vm show -g myDockerResourceGroup -n myDockerVM` da biste pogledali detalje o VM, uključujući naziv DNS-a.

Da biste provjerili je li sastavljanje instaliran na VM, pokrenite sljedeću naredbu:

```bash
docker-compose --version
```

Pogledajte izlaz slično `docker-compose 1.6.2, build 4d72027`.

>[AZURE.TIP] Ako ste koristili drugi način za stvaranje Docker glavno računalo i morate instalirati sastavljanje, potražite u [dokumentaciji za sastavljanje](https://github.com/docker/compose/blob/882dc673ce84b0b29cd59b6815cb93f74a6c4134/docs/install.md).


## <a name="step-3-create-a-docker-composeyml-configuration-file"></a>Korak 3: Stvaranje docker compose.yml konfiguracijska datoteka

Zatim stvorite na `docker-compose.yml` datoteke koja je samo tekst konfiguracije datoteka da biste definirali spremnika Docker pokrenuti na VM. Datoteku određuje sliku da biste pokrenuli za svakog spremnika (ili može biti Sastavi iz programa Dockerfile), varijable okruženja potrebne i ovisnosti, priključke i veza između spremnika. Informacije o sintaksi yml datoteke potražite u članku [Referenca za sastavljanje datoteke](http://docs.docker.com/compose/yml/).

Stvaranje na `docker-compose.yml` datoteke na sljedeći način:

```bash
touch docker-compose.yml
```

Dodajte neki podaci datoteku pomoću omiljene uređivač. Sljedeći primjer koristi u `vi` uređivač:

```bash
vi docker-compose.yml
```

U sljedećem primjeru zalijepite u tekstnu datoteku. Tu konfiguraciju koristi slike iz [Registra DockerHub](https://registry.hub.docker.com/_/wordpress/) za instalaciju WordPress (Otvori izvor popularnim i sadržaj upravljanje sustav) i povezane pozadinskom bazom podataka MariaDB SQL. Unesite vlastiti `MYSQL_ROOT_PASSWORD` na sljedeći način:

```bash
wordpress:
  image: wordpress
  links:
    - db:mysql
  ports:
    - 80:80

db:
  image: mariadb
  environment:
    MYSQL_ROOT_PASSWORD: <your password>
```

## <a name="step-4-start-the-containers-with-compose"></a>Korak 4: Počinju spremnike sastavljanje

U imeniku isti kao vaše `docker-compose.yml` datoteku, pokrenite sljedeću naredbu (ovisno o okruženju, morat ćete pokrenuti `docker-compose` pomoću `sudo`.):

```bash
docker-compose up -d

```

Ta se naredba pokreće spremnika Docker naveden u `docker-compose.yml`. Traje minutu ili dvije za ovaj korak da biste dovršili. Pogledajte izlaz slično kao u sljedećem primjeru:

```bash
Creating wordpress_db_1...
Creating wordpress_wordpress_1...
...
```

>[AZURE.NOTE] Obavezno koristite mogućnost **-d** na pokretanja tako da se spremnike neprestano radi u pozadini.

Da biste provjerili spremnike prema gore, upišite `docker-compose ps`. Trebali biste vidjeti otprilike ovako:

```bash
Name             Command             State              Ports
-------------------------------------------------------------------------
wordpress_db_1     /docker-           Up                 3306/tcp
             entrypoint.sh
             mysqld
wordpress_wordpr   /entrypoint.sh     Up                 0.0.0.0:80->80
ess_1              apache2-for ...                       /tcp
```

Sada se možete povezati s WordPress izravno na VM na priključak 80. Otvorite web-preglednik i unesite naziv DNS vaše VM (kao što su `http://myresourcegroup.westus.cloudapp.azure.com`). Sada trebali biste vidjeti na WordPress početni zaslon, gdje možete dovršiti instalaciju i početak rada s aplikacijom.

![WordPress početni zaslon][wordpress_start]


## <a name="next-steps"></a>Daljnji koraci

* Idite na [Docker VM proširenje korisničkom priručniku](https://github.com/Azure/azure-docker-extension/blob/master/README.md) za dodatne mogućnosti da biste konfigurirali web-mjesto Docker i u okvir za sastavljanje u vašem VM Docker. Ako, na primjer, jedan je mogućnost da biste umetnuli datoteku yml sastavljanje (pretvoriti u JSON) izravno u konfiguraciji proširenje Docker VM.
* Potražite u članku [Referenca za sastavljanje naredbenog retka](http://docs.docker.com/compose/reference/) i [korisničkom priručniku](http://docs.docker.com/compose/) za više primjera stvaranja i implementacija aplikacije više spremnik.
* Ili pomoću predloška Azure Voditelj resursa, vaše vlastite ili jedan pridonio iz [zajednice](https://azure.microsoft.com/documentation/templates/)za implementaciju programa VM Azure s Docker i postaviti s sastavljanje aplikacije. Na primjer, predložak [uvođenja bloga WordPress s Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-wordpress-mysql) koristi Docker i sastavljanje brzo uvođenje WordPress s pozadinskom MySQL na programa VM Ubuntu.
* Pokušajte integriranje sastavite Docker s klaster [Docker Swarm](virtual-machines-linux-docker-swarm.md) . Potražite u članku [Korištenje sastavite s Swarm](https://docs.docker.com/compose/swarm/) scenarije.

<!--Image references-->

[wordpress_start]: ./media/virtual-machines-linux-docker-compose-quickstart/WordPress.png
